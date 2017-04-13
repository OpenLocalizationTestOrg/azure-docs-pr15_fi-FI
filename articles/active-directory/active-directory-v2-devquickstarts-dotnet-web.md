<properties
    pageTitle="Azure AD v2.0 .NET Web Appin | Microsoft Azure"
    description=".NET MVC-verkkosovelluksen tietokenttiä kirjautuu-käyttäjät sekä oman Microsoft Account muodostaminen ja työpaikan tai oppilaitoksen tilit."
    services="active-directory"
    documentationCenter=".net"
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="dastrock"/>

# <a name="add-sign-in-to-an-net-mvc-web-app"></a>Kirjaudu sisään .NET MVC web-sovelluksen lisääminen

V2.0 päätepisteissä voit nopeasti lisätä web Apps-sovellusten tuki kummankin henkilökohtainen Microsoft-tilin todennus ja työpaikan tai oppilaitoksen tilit.  ASP.NET web Apps-sovelluksissa voit tehdä tämän käyttämällä Microsoftin OWIN middleware sisältyy .NET Framework 4.5.

> [AZURE.NOTE]
    V2.0 päätepisteen tukee kaikkia Azure Active Directory-skenaariot ja toiminnot.  Voit selvittää, jos sinun on käytettävä v2.0 päätepisteen, lue lisätietoja [v2.0 rajoitukset](active-directory-v2-limitations.md).

 Seuraavassa on luoda web-sovellusta, joka käyttää OWIN käyttäjän kirjautuminen, näyttää käyttäjän tietoja ja kirjaudu ulos sovelluksesta käyttäjä.
 
 ## <a name="download"></a>Lataa
Tässä opetusohjelmassa koodi määritetään [GitHub](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet).  Jos haluat seurata, voit [ladata sovelluksen rakenne .zip nimellä](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet/archive/skeleton.zip) tai Kloonaa rakenne:

```git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet.git```

Valmiin sovelluksen annetaan jäljempänä tässä opetusohjelmassa.

## <a name="register-an-app"></a>Sovelluksen rekisteröiminen
Luo uuden sovelluksen [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)tai noudattamalla seuraavia [toimimalla seuraavasti](active-directory-v2-app-registration.md).  Varmista, että:

- Kopioi alaspäin **Sovellustunnus** sovelluksen myönnetyt, sinun on se pian.
- Lisää **WWW** -ympäristö, kun sovellus.
- Kirjoita oikeat **Uudelleenohjata URI**. Redirect uri ilmaisee, jossa todennus vastaukset olisi ohjautuu – Tässä opetusohjelmassa oletusarvo on Azure AD `https://localhost:44326/`.

## <a name="install--configure-owin-authentication"></a>Asenna ja määritä OWIN todennus
Tässä on määrittää OWIN middleware OpenID yhteyden todennus-protokollaa.  OWIN käytetään antaa Kirjaudu sisään ja kirjaudu ulos pyyntöjä ja hallita käyttäjän istunnon käyttäjän, muun muassa tietoja.

-   Avaa- `web.config` projektin ylimmällä tiedosto ja kirjoita sinua sovelluksen kokoonpanon arvoja `<appSettings>` osa.
    -   `ida:ClientId` On määritetty sovelluksen rekisteröinnin portaalissa **Tunnus** .
    -   `ida:RedirectUri` On lisätty portaalin **Uudelleenohjata Uri** .

-   Lisää seuraavaksi OWIN middleware NuGet pakettien pakettien hallinta-konsolin käyttämisestä projektiin.

```
PM> Install-Package Microsoft.Owin.Security.OpenIdConnect
PM> Install-Package Microsoft.Owin.Security.Cookies
PM> Install-Package Microsoft.Owin.Host.SystemWeb
```

-   Lisää "OWIN käynnistys luokka" projektiin `Startup.cs` oikealle, valitse projektin--> **Lisää** --> **Uuden kohteen** --> Etsi "OWIN".  OWIN middleware käynnistää `Configuration(...)` silloin, kun sovellus käynnistyy.
-   Muuttaa luokan ilmoituksen `public partial class Startup` -jo indeksoitiin tähän luokkaan osa puolestasi toiseen tiedostoon.  Valitse `Configuration(...)` -menetelmä tehdä ConfigureAuth(...) puhelun web App-sovelluksen käyttöoikeuksien määrittäminen  

```C#
[assembly: OwinStartup(typeof(Startup))]

namespace TodoList_WebApp
{
    public partial class Startup
    {
        public void Configuration(IAppBuilder app)
        {
            ConfigureAuth(app);
        }
    }
}
```

-   Avaa tiedosto `App_Start\Startup.Auth.cs` ja käyttöönotto `ConfigureAuth(...)` menetelmää.  Saadaan Parametrit `OpenIdConnectAuthenticationOptions` toimivat sovelluksen koordinaatit Azure AD yhteydessä.  Sinun on myös eväste todentamisen määrittäminen – OpenID yhteyden middleware evästeet kattaa alapuolella.

```C#
public void ConfigureAuth(IAppBuilder app)
             {
                     app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

                     app.UseCookieAuthentication(new CookieAuthenticationOptions());

                     app.UseOpenIdConnectAuthentication(
                             new OpenIdConnectAuthenticationOptions
                             {
                                     // The `Authority` represents the v2.0 endpoint - https://login.microsoftonline.com/common/v2.0 
                                     // The `Scope` describes the permissions that your app will need.  See https://azure.microsoft.com/documentation/articles/active-directory-v2-scopes/
                                     // In a real application you could use issuer validation for additional checks, like making sure the user's organization has signed up for your app, for instance.

                                     ClientId = clientId,
                                     Authority = String.Format(CultureInfo.InvariantCulture, aadInstance, "common", "/v2.0 "),
                                     RedirectUri = redirectUri,
                                     Scope = "openid email profile",
                                     ResponseType = "id_token",
                                     PostLogoutRedirectUri = redirectUri,
                                     TokenValidationParameters = new TokenValidationParameters
                                     {
                                             ValidateIssuer = false,
                                     },
                                     Notifications = new OpenIdConnectAuthenticationNotifications
                                     {
                                             AuthenticationFailed = OnAuthenticationFailed,
                                     }
                             });
             }
```

## <a name="send-authentication-requests"></a>Lähetä käyttöoikeuksien
Sovellus on nyt määritetty oikein pitää yhteyttä v2.0 päätepisteen OpenID yhteyden todennus-protokollan avulla.  OWIN on otettava varoen kaikkien työstämistä todennus viestejä, vahvistaminen Azure AD tunnusta ja ylläpito käyttäjäistunnon rumalta tietoja.  Kaikki pysyy myös antaa käyttäjille voi kirjautua sisään ja ulos.

- Voit sallia oman ohjaimet edellyttää, että käyttäjä kirjautuu ennen käyttöä tietyn sivun tunnisteet.  Avaa `Controllers\HomeController.cs`, ja lisää `[Authorize]` tunnisteen tietoja valvojalle.

```C#
[Authorize]
public ActionResult About()
{
  ...
```

-   Voit myös antaa käyttöoikeuden pyynnöt suoraan kuluessa koodisi OWIN.  Avaa `Controllers\AccountController.cs`.  SignIn() ja SignOut() toiminnot Lähetä OpenID yhteyden todennus ja kirjaudu ulos pyynnöt tarpeen mukaan.

```C#
public void SignIn()
{
    // Send an OpenID Connect sign-in request.
    if (!Request.IsAuthenticated)
    {
        HttpContext.GetOwinContext().Authentication.Challenge(new AuthenticationProperties { RedirectUri = "/" }, OpenIdConnectAuthenticationDefaults.AuthenticationType);
    }
}

// BUGBUG: Ending a session with the v2.0 endpoint is not yet supported.  Here, we just end the session with the web app.  
public void SignOut()
{
    // Send an OpenID Connect sign-out request.
    HttpContext.GetOwinContext().Authentication.SignOut(CookieAuthenticationDefaults.AuthenticationType);
    Response.Redirect("/");
}
```

-   Avaa `Views\Shared\_LoginPartial.cshtml`.  Tämä on kohtaa, johon käyttäjän sinua sovelluksen Kirjaudu sisään ja kirjaudu ulos-linkkien näyttäminen ja tulostaa käyttäjän nimi näkymässä.

```HTML
@if (Request.IsAuthenticated)
{
    <text>
        <ul class="nav navbar-nav navbar-right">
            <li class="navbar-text">

                @*The 'preferred_username' claim can be used for showing the user's primary way of identifying themselves.*@

                Hello, @(System.Security.Claims.ClaimsPrincipal.Current.FindFirst("preferred_username").Value)!
            </li>
            <li>
                @Html.ActionLink("Sign out", "SignOut", "Account")
            </li>
        </ul>
    </text>
}
else
{
    <ul class="nav navbar-nav navbar-right">
        <li>@Html.ActionLink("Sign in", "SignIn", "Account", routeValues: null, htmlAttributes: new { id = "loginLink" })</li>
    </ul>
}
```

## <a name="display-user-information"></a>Näyttää Käyttäjätiedot
Käyttäjät, joilla on OpenID yhteyden todennuksessa v2.0 päätepisteen palauttaa id_token, joka sisältää [vaateet](active-directory-v2-tokens.md#id_tokens)ja vahvistukset käyttäjästä-sovellukseen.  Voit käyttää näitä väitteitä mukauttamiseen sovelluksen:

- Avaa `Controllers\HomeController.cs` tiedosto.  Voit käyttää käyttäjän saatavat oman ohjaimet kautta `ClaimsPrincipal.Current` suojaus Pääobjektin.

```C#
[Authorize]
public ActionResult About()
{
    ViewBag.Name = ClaimsPrincipal.Current.FindFirst("name").Value;

    // The object ID claim will only be emitted for work or school accounts at this time.
    Claim oid = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier");
    ViewBag.ObjectId = oid == null ? string.Empty : oid.Value;

    // The 'preferred_username' claim can be used for showing the user's primary way of identifying themselves
    ViewBag.Username = ClaimsPrincipal.Current.FindFirst("preferred_username").Value;

    // The subject or nameidentifier claim can be used to uniquely identify the user
    ViewBag.Subject = ClaimsPrincipal.Current.FindFirst("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier").Value;

    return View();
}
```

## <a name="run"></a>Suorita

Lopuksi luominen ja suorittaminen sovelluksen!   Kirjaudu sisään Microsoft-Account tai työpaikan tai oppilaitoksen tiliä, niin huomaat, miten käyttäjän tunnistetiedot näkyy siirtymispalkissa.  Nyt on suojattu alan vakio protokollia, jotka voi todentaa niiden henkilökohtainen ja työpaikan tai oppilaitoksen tilin käyttäjille verkkosovellukseen.

Viitteen Esimerkki valmiista (ilman määritysarvot) [tähän .zip taulukkona](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet/archive/complete.zip), tai voit voit käytetään sen GitHub:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIdConnect-DotNet.git```

## <a name="next-steps"></a>Seuraavat vaiheet

Voit nyt siirtää aiheita sivulle.  Voit halutessasi kokeilla:

[Suojattu verkko-Ohjelmointirajapinnan kanssa v2.0 päätepisteen >>](active-directory-devquickstarts-webapi-dotnet.md)

Muita tietolähteitä Kuittaa ulos:
- [V2.0 Sovelluskehittäjän opas >>](active-directory-appmodel-v2-overview.md)
- [StackOverflow "azure-active directory-tunniste >>](http://stackoverflow.com/questions/tagged/azure-active-directory)

## <a name="get-security-updates-for-our-products"></a>Päivitysten hakeminen tuotteitamme

On suositeltavaa, kun suojauksen tapaukset esiintyvät käymällä [Tämän sivun](https://technet.microsoft.com/security/dd252948) ja tilaamalla neuvoa suojausvaroitusten ilmoitusten saaminen.
