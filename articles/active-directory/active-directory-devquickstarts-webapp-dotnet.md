<properties
    pageTitle="Azure AD-.NET aloittaminen | Microsoft Azure"
    description="Miten voit luoda .NET MVC-verkkosovelluksen tietokenttiä integroituu Azure AD kirjauduttaessa."
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

# <a name="aspnet-web-app-sign-in--sign-out-with-azure-ad"></a>ASP.NET Web App-Kirjaudu sisään ja kirjaudu ulos ja Azure AD

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Azure AD on yksinkertainen ja selkeä ulkoistaa web-sovelluksen jäsenyyksien hallinta tarjoaa yhden Kirjaudu sisään ja kirjaudu ulos vain muutaman viivoilla koodin.  Asp.NET web Apps-sovelluksissa voit tehdä tämän käyttämällä Microsoftin soveltaminen yhteisön perustuva-OWIN middleware sisältyy .NET Framework 4.5.  Käytämme tähän OWIN avulla.
-   Kirjaudu käyttäjän sovellukseen käyttämällä Azure AD tunnistetietojen toimittaja.
-   Näyttää käyttäjän tietoja.
-   Kirjaudu ulos sovelluksesta käyttäjä.

Jotta voit tehdä tämän, sinun on:

1. Voit rekisteröidä sovelluksen Azure AD
2. Määrittää sovelluksen OWIN todennus putkijohto.
3. Kirjaudu sisään ja kirjaudu ulos pyynnöt antamaan Azure AD OWIN avulla.
4. Tulostaa käyttäjän tietoja.

Jos haluat aloittaa, [Lataa sovelluksen rakenne](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/skeleton.zip) tai [Lataa Esimerkki valmiista](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/complete.zip).  Sinun on myös Azure AD-vuokraajan, jolla sovellus rekisteröidään.  Jos sinulla ei sitä vielä ole, [Lue, miten voit hankkia sen](active-directory-howto-tenant.md).

## <a name="1--register-an-application-with-azure-ad"></a>*1. rekisteröidä Azure AD sovellus*
Käyttäjien todentamiseen sovelluksen käyttöön ensin tarvitset uusi sovellus rekisteröidään alihallintaan.

- Kirjaudu sisään Azure hallinta-portaalin.
- Valitse vasemmalla olevasta siirtymisruudussa **Active Directorysta**.
- Valitse missä haluat rekisteröidä sovellus vuokraaja.
- Valitse **sovellukset** -välilehti ja valitse Lisää ala-laatikon.
- Seuraa ohjeita ja luo uusi **Web-sovelluksen ja/tai WebAPI**.
    - Sovelluksen **nimen** kuvataan sovelluksesi loppukäyttäjille
    -   **Sign-On URL** on sovelluksen pääkansion URL-osoite.  Oletusarvo rakenne on `https://localhost:44320/`.
    - **Sovelluksen tunnus URI** on yksilöllinen sovelluksen.  Konferenssi on käyttää `https://<tenant-domain>/<app-name>`, kuten`https://contoso.onmicrosoft.com/my-first-aad-app`
- Kun olet suorittanut rekisteröinti, AAD Määritä sovelluksen asiakkaan yksilöllinen tunnus.  Tarvitset tätä arvoa seuraavat osiot niin kopioi se määritys-välilehti.

## <a name="2-set-up-your-app-to-use-the-owin-authentication-pipeline"></a>*2. määrittää sovelluksen OWIN todennus myyntijakso*
Tässä on määrittää OWIN middleware OpenID yhteyden todennus-protokollaa.  OWIN käytetään antaa Kirjaudu sisään ja kirjaudu ulos pyyntöjä ja hallita käyttäjän istunnon käyttäjän, muun muassa tietoja.

-   Aloita projektin Package hallinta-konsolin OWIN middleware NuGet pakettien lisääminen.

```
PM> Install-Package Microsoft.Owin.Security.OpenIdConnect
PM> Install-Package Microsoft.Owin.Security.Cookies
PM> Install-Package Microsoft.Owin.Host.SystemWeb
```

-   Lisää OWIN käynnistys-luokan projektin nimi `Startup.cs` oikealle, valitse projektin--> **Lisää** --> **Uuden kohteen** --> Etsi "OWIN".  OWIN middleware käynnistää `Configuration(...)` silloin, kun sovellus käynnistyy.
-   Muuttaa luokan ilmoituksen `public partial class Startup` -jo indeksoitiin tähän luokkaan osa puolestasi toiseen tiedostoon.  Valitse `Configuration(...)` -menetelmä tehdä ConfgureAuth(...) puhelun web App-sovelluksen käyttöoikeuksien määrittäminen  

```C#
public partial class Startup
{
    public void Configuration(IAppBuilder app)
    {
        ConfigureAuth(app);
    }
}
```

-   Avaa tiedosto `App_Start\Startup.Auth.cs` ja käyttöönotto `ConfigureAuth(...)` menetelmää.  Saadaan Parametrit `OpenIDConnectAuthenticationOptions` toimivat sovelluksen koordinaatit Azure AD yhteydessä.  Sinun on myös eväste todentamisen määrittäminen – OpenID yhteyden middleware evästeet kattaa alapuolella.

```C#
public void ConfigureAuth(IAppBuilder app)
{
    app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

    app.UseCookieAuthentication(new CookieAuthenticationOptions());

    app.UseOpenIdConnectAuthentication(
        new OpenIdConnectAuthenticationOptions
        {
            ClientId = clientId,
            Authority = authority,
            PostLogoutRedirectUri = postLogoutRedirectUri,
        });
}
```

-   Avaa- `web.config` projektin pääkansiossa tiedosto ja kirjoita määritys-arvot- `<appSettings>` osa.
    -   Sinua sovelluksen `ida:ClientId` on vaiheessa 1 Azure-portaalista kopioimasi GUID-tunnus.
    -   `ida:Tenant` On Azure AD-vuokraajan, kuten "contoso.onmicrosoft.com" nimi.
    -   Oman `ida:PostLogoutRedirectUri` ilmaisee Azure AD, johon käyttäjä ohjataan Kirjaudu ulos pyyntö onnistuneesti päätyttyä.

## <a name="3-use-owin-to-issue-sign-in-and-sign-out-requests-to-azure-ad"></a>*3. Kirjaudu sisään ja kirjaudu ulos pyynnöt antamaan Azure AD OWIN Käytä*
Sovellus on nyt määritetty oikein pitää yhteyttä Azure AD-OpenID yhteyden todennus-protokollan avulla.  OWIN on otettava varoen kaikkien työstämistä todennus viestejä, vahvistaminen Azure AD tunnusta ja ylläpito käyttäjäistunnon rumalta tietoja.  Kaikki pysyy myös antaa käyttäjille voi kirjautua sisään ja ulos.

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
public void SignOut()
{
    // Send an OpenID Connect sign-out request.
    HttpContext.GetOwinContext().Authentication.SignOut(
        OpenIdConnectAuthenticationDefaults.AuthenticationType, CookieAuthenticationDefaults.AuthenticationType);
}
```

-   Avaa `Views\Shared\_LoginPartial.cshtml`.  Tämä on kohtaa, johon käyttäjän sinua sovelluksen Kirjaudu sisään ja kirjaudu ulos-linkkien näyttäminen ja tulostaa käyttäjän nimi näkymässä.

```HTML
@if (Request.IsAuthenticated)
{
    <text>
        <ul class="nav navbar-nav navbar-right">
            <li class="navbar-text">
                Hello, @User.Identity.Name!
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

## <a name="4--display-user-information"></a>*4. näyttää käyttäjätiedot*
Käyttäjät, joilla on OpenID yhteyden todennuksessa Azure AD palauttaa id_token sovellus, joka sisältää "vaateet" ja vahvistukset käyttäjästä.  Voit käyttää näitä väitteitä mukauttamiseen sovelluksen:

- Avaa `Controllers\HomeController.cs` tiedosto.  Voit käyttää käyttäjän saatavat oman ohjaimet kautta `ClaimsPrincipal.Current` suojaus Pääobjektin.

```C#
public ActionResult About()
{
    ViewBag.Name = ClaimsPrincipal.Current.FindFirst(ClaimTypes.Name).Value;
    ViewBag.ObjectId = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier").Value;
    ViewBag.GivenName = ClaimsPrincipal.Current.FindFirst(ClaimTypes.GivenName).Value;
    ViewBag.Surname = ClaimsPrincipal.Current.FindFirst(ClaimTypes.Surname).Value;
    ViewBag.UPN = ClaimsPrincipal.Current.FindFirst(ClaimTypes.Upn).Value;

    return View();
}
```

Lopuksi luominen ja suorittaminen sovelluksen!  Jos et ole jo, nyt on aika, voit luoda uuden käyttäjän kanssa vuokraajan *. onmicrosoft.com-toimialue.  Kirjaudu sisään kyseisen käyttäjän, niin huomaat, miten käyttäjän tunnistetiedot näkyy siirtymispalkissa.  Kirjaudu ulos ja kirjaudu takaisin sisään vuokraajan toiselle käyttäjälle.  Jos olet tiedostoja erityisen kunnianhimoiset Rekisteröi ja suorittaa toiminnon esiintymässä tämän sovelluksen (ja oma clientId) ja katso Katso single-merkki.

Viitteen, esimerkki valmiista (ilman määritysarvot), [on annettu tähän](https://github.com/AzureADQuickStarts/WebApp-OpenIdConnect-DotNet/archive/complete.zip).  

Voit nyt siirtää aiheita sivulle.  Voit halutessasi kokeilla:

[Suojattu verkko-Ohjelmointirajapinnan kanssa Azure AD >>](active-directory-devquickstarts-webapi-dotnet.md)

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
