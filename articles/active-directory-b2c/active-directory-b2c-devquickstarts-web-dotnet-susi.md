<properties
    pageTitle="Azure Active Directory-B2C | Microsoft Azure"
    description="Miten voit luoda web-sovelluksen, jossa on kirjautumisen, kirjaudu sisään ja salasanan avulla Azure Active Directory-B2C."
    services="active-directory-b2c"
    documentationCenter=".net"
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="07/22/2016"
    ms.author="dastrock"/>

# <a name="azure-ad-b2c-sign-up--sign-in-in-a-aspnet-web-app"></a>Azure AD-B2C: Kirjautuminen ja kirjaudu sisään ASP.NET Web App-sovelluksessa

<!-- TODO [AZURE.INCLUDE [active-directory-b2c-devquickstarts-web-switcher](../../includes/active-directory-b2c-devquickstarts-web-switcher.md)]-->

Azure Active Directory (Azure AD) B2C avulla voit lisätä tehokkaita Omatoiminen tunnistetietojen hallintaominaisuudet web App-sovellukseen, muutaman lyhyt vaiheen avulla. Tässä artikkelissa keskustella luomisesta ASP.NET web Appissa, joka sisältää käyttäjän kirjautuminen, kirjaudu sisään ja salasanan. Sovelluksen sisällytetään tuki kirjautumista ja kirjaudu sisään käyttämällä käyttäjänimi tai sähköpostiosoite ja käyttämällä yhteisöpalvelujen, kuten Facebook- ja Google.

Tässä opetusohjelmassa eroaa [sekä muut .NET web opetusohjelma](active-directory-b2c-devquickstarts-web-dotnet.md) siten, että se käyttää [rekisteröitynyt tai kirjautunut käytäntö](active-directory-b2c-reference-policies.md#create-a-sign-up-or-sign-in-policy) antaa käyttäjän rekisteröinti ja kirjaudu sisään käyttämällä yhden painikkeen kahden kerran sijaan (yksi kirjautuminen ja kirjaudu sisään).  Valitse nutshell on tai Kirjaudu sisään käytännön avulla käyttäjät voivat kirjautua sisään aiemmin luotu tili Jos kirjoittamisen tai luoda uuden, jos se on niiden sovelluksen ensimmäisen kerran.

## <a name="get-an-azure-ad-b2c-directory"></a>Hae Azure AD B2C-kansio

Ennen kuin voit käyttää Azure AD B2C, voit luoda hakemiston tai vuokraaja. Kansio on kaikkien käyttäjien, sovellukset, ryhmät ja lisää säilö.  Jos sitä ei ole vielä ole, ennen kuin voit [luoda hakemiston B2C](active-directory-b2c-get-started.md) edelleen tässä oppaassa.

## <a name="create-an-application"></a>Sovelluksen luominen

Seuraavaksi haluat luoda sovelluksen B2C hakemistossa. Näin pitää yhteyttä suojatusti sovelluksen tarvitsemiaan Azure AD-tietoja. Voit luoda sovelluksen noudattamalla [näitä ohjeita](active-directory-b2c-app-registration.md).  Muista:

- Sisällytä **web app ja verkko-Ohjelmointirajapinnan** sovellus.
- Kirjoita `https://localhost:44316/` kuin **uudelleenohjata URI**. Tässä esimerkissä koodi oletusarvoisen URL-osoite on.
- Kopioi **Tunnus** , jolla määritetään sovelluksen alaspäin.  Tarvitset sen myöhemmin.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Luoda oman käytäntöjä

Azure AD B2C jokaisen käyttäjäkokemus on määritetty [käytännön](active-directory-b2c-reference-policies.md)avulla. Koodin tässä esimerkissä on kaksi identity-versiota: **Kirjautuminen ja kirjaudu sisään**ja **salasanan**.  Haluat yhden käytännön kunkin tyypin [käytännön viitata artikkelissa](active-directory-b2c-reference-policies.md)kuvatulla tavalla. Kun luot kaksi käytäntöjä, muista:

- Valitse **Käyttäjätunnus kirjautuminen** tai **sähköpostin kirjautuminen** tunnistetietojen toimittajat-sivu.
- Valitse rekisteröitymistoiminto ja kirjaudu sisään käytännön **Näyttönimi** ja rekisteröitymistoiminto määritteet.
- Valitse sovelluksena **Näyttönimi** -ryhmän varaa jokaisen käytännössä. Voit valita myös muut vaateet.
- Kopioi kunkin käytännön **nimen** , sen luomisen jälkeen. Tarvitset käytännön nimet myöhemmin.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Kun olet luonut kaksi käytäntöjä, olet valmis luomaan sovelluksen.

## <a name="download-the-code-and-configure-authentication"></a>Lataa koodi ja käyttöoikeuksien määrittäminen

Tämä esimerkki [määritetään GitHub](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet-SUSI)koodi. Voit luoda otosten kuitenkin, [Lataa rakenne projektin .zip-tiedosto](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet-SUSI/archive/skeleton.zip). Voit myös kopioida rakenne:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet-SUSI.git
```

Esimerkki valmiista on myös [käytettävissä .zip-tiedostona](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet-SUSI/archive/complete.zip) tai `complete` saman säilö haaraa.

Kun olet ladannut sample code, Avaa Visual Studio .sln tiedosto avulla pääset alkuun.

Sovelluksen yhteydessä Azure AD B2C lähettämällä todennus pyyntöjen, joka määrittää käytännön, se haluaa suorittaa pyynnön osana. .NET-web-sovellusten voit käyttää Microsoftin OWIN kirjaston lähettää OpenID yhteyden todennus-pyyntöjä, suorita käytäntöjä, hallinta ja käyttäjäistuntojen.

Aloita Lisää OWIN middleware NuGet pakettien projektiin Visual Studio paketin hallinta-konsolin avulla.

```
Install-Package Microsoft.Owin.Security.OpenIdConnect
Install-Package Microsoft.Owin.Security.Cookies
Install-Package Microsoft.Owin.Host.SystemWeb
Install-Package System.IdentityModel.Tokens.Jwt
```

Avaa seuraavaksi `web.config` projektin ylimmällä tiedosto ja kirjoita sinua sovelluksen kokoonpanon arvoja `<appSettings>` ‑osassa alhaisempien arvojen korvaaminen omalla.  Sivuista voi jäädä `ida:RedirectUri` ja `ida:AadInstance` arvot tavoin muuttumattomana.

```
<configuration>
  <appSettings>

    ...

    <add key="ida:Tenant" value="fabrikamb2c.onmicrosoft.com" />
    <add key="ida:ClientId" value="90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6" />
    <add key="ida:AadInstance" value="https://login.microsoftonline.com/{0}{1}{2}" />
    <add key="ida:RedirectUri" value="https://localhost:44316/" />
    <add key="ida:SusiPolicyId" value="b2c_1_susi" />
    <add key="ida:PasswordResetPolicyId" value="b2c_1_reset" />
  </appSettings>
...
```

[AZURE.INCLUDE [active-directory-b2c-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

Lisää seuraavaksi OWIN-käynnistys luokan projektin nimi `Startup.cs`. Projektin hiiren kakkospainikkeella ja valitse **Lisää** ja **Uuden kohteen**Hae "OWIN." Muuttaa luokan ilmoituksen `public partial class Startup`. Olemme toteutettu tähän luokkaan osa puolestasi toiseen tiedostoon. OWIN middleware käynnistää `Configuration(...)` silloin, kun sovellus käynnistyy. Tämä menetelmä Varmista puhelun `ConfigureAuth(...)`, jossa voit määrittää käyttöoikeuden, kun sovellus.

```C#
// Startup.cs

public partial class Startup
{
    public void Configuration(IAppBuilder app)
    {
        ConfigureAuth(app);
    }
}
```

Avaa tiedosto `App_Start\Startup.Auth.cs` ja käyttöönotto `ConfigureAuth(...)` menetelmää.  Saadaan Parametrit `OpenIdConnectAuthenticationOptions` sovelluksen koordinaatit pitää yhteyttä Azure AD yhteyshenkilönä. Haluat myös eväste todentamisen määrittäminen. OpenID yhteyden middleware evästeet säilyttää käyttäjäistuntojen, muun muassa.

```C#
// App_Start\Startup.Auth.cs

public partial class Startup
{
    // App config settings
    private static string clientId = ConfigurationManager.AppSettings["ida:ClientId"];
    private static string aadInstance = ConfigurationManager.AppSettings["ida:AadInstance"];
    private static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];
    private static string redirectUri = ConfigurationManager.AppSettings["ida:RedirectUri"];

    // B2C policy identifiers
    public static string SusiPolicyId = ConfigurationManager.AppSettings["ida:SusiPolicyId"];
    public static string PasswordResetPolicyId = ConfigurationManager.AppSettings["ida:PasswordResetPolicyId"];

    public void ConfigureAuth(IAppBuilder app)
    {
        app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

        app.UseCookieAuthentication(new CookieAuthenticationOptions());

        // Configure OpenID Connect middleware for each policy
        app.UseOpenIdConnectAuthentication(CreateOptionsFromPolicy(PasswordResetPolicyId));
        app.UseOpenIdConnectAuthentication(CreateOptionsFromPolicy(SusiPolicyId));

    }

    private Task OnSecurityTokenValidated(SecurityTokenValidatedNotification<OpenIdConnectMessage, OpenIdConnectAuthenticationOptions> notification)
    {
        // If you wanted to keep some local state in the app (like a db of signed up users),
        // you could use this notification to create the user record if it does not already
        // exist.

        return Task.FromResult(0);
    }

    private OpenIdConnectAuthenticationOptions CreateOptionsFromPolicy(string policy)
    {
        return new OpenIdConnectAuthenticationOptions
        {
            // For each policy, give OWIN the policy-specific metadata address, and
            // set the authentication type to the id of the policy
            MetadataAddress = String.Format(aadInstance, tenant, policy),
            AuthenticationType = policy,

            // These are standard OpenID Connect parameters, with values pulled from web.config
            ClientId = clientId,
            RedirectUri = redirectUri,
            PostLogoutRedirectUri = redirectUri,
            Notifications = new OpenIdConnectAuthenticationNotifications
            {
                AuthenticationFailed = AuthenticationFailed,
                SecurityTokenValidated = OnSecurityTokenValidated,
            },
            Scope = "openid",
            ResponseType = "id_token",

            // This piece is optional - it is used for displaying the user's name in the navigation bar.
            TokenValidationParameters = new TokenValidationParameters
            {
                NameClaimType = "name",
            },
        };
    }
}
...
```

## <a name="send-authentication-requests-to-azure-ad"></a>Lähetä käyttöoikeuksien Azure AD
Sovellus on nyt määritetty oikein pitää yhteyttä Azure AD B2C OpenID yhteyden todennus-protokollan avulla.  OWIN on otettava varoen kaikkien työstämistä todennus viestejä, vahvistaminen Azure AD tunnusta ja ylläpito käyttäjäistunnon tietoja.  Kaikki pysyy on aloittaa kunkin käyttäjän työnkulku.

Kun käyttäjä valitsee **Kirjautuminen** tai **Unohtuiko salasana?** online-sovelluksessa liittyvä toiminto käynnistetään `Controllers\AccountController.cs`. Joka tapauksessa voit käyttää valmiita OWIN menetelmiä käynnistettävän oikean käytäntö:

```C#
// Controllers\AccountController.cs

public void Login()
{
    if (!Request.IsAuthenticated)
    {
        // To execute a policy, you simply need to trigger an OWIN challenge.
        // You can indicate which policy to use by specifying the policy id as the AuthenticationType
        HttpContext.GetOwinContext().Authentication.Challenge(
            new AuthenticationProperties() { RedirectUri = "/" }, Startup.SusiPolicyId);
    }
}

public void ResetPassword()
{
    if (!Request.IsAuthenticated)
    {
        HttpContext.GetOwinContext().Authentication.Challenge(
        new AuthenticationProperties() { RedirectUri = "/" }, Startup.PasswordResetPolicyId);
    }
}
```

Rekisteröinti suorituksen aikana tai käytännön kirjautuminen, käyttäjällä on mahdollisuus valita **Unohtuiko salasana?** linkki.  Tässä tapauksessa Azure AD B2C lähettää sovelluksen virhesanomasta, joka ilmaisee sen kirjoittavien salasanan palauttaminen käytännön.  Tämä virhe voit siepata `Startup.Auth.cs` avulla `AuthenticationFailed` ilmoitus:

```C#
// Used for avoiding yellow-screen-of-death TODO
private Task AuthenticationFailed(AuthenticationFailedNotification<OpenIdConnectMessage, OpenIdConnectAuthenticationOptions> notification)
{
    notification.HandleResponse();

    if (notification.ProtocolMessage.ErrorDescription != null && notification.ProtocolMessage.ErrorDescription.Contains("AADB2C90118"))
    {
        // If the user clicked the reset password link, redirect to the reset password route
        notification.Response.Redirect("/Account/ResetPassword");
    }
    else if (notification.Exception.Message == "access_denied")
    {
        // If the user canceled the sign in, redirect back to the home page
        notification.Response.Redirect("/");
    }
    else
    {
        notification.Response.Redirect("/Home/Error?message=" + notification.Exception.Message);
    }

    return Task.FromResult(0);
}
```


Lisäksi käynnistettäessä käytännön erikseen, voit käyttää `[Authorize]` tunnistetta oman ohjaimet, jotka suoritetaan käytännön, jos käyttäjä ei ole kirjautunut sisään. Avaa `Controllers\HomeController.cs` ja lisätä `[Authorize]` tunnisteen saatavat valvojalle.  OWIN Valitsee viimeisen käytäntöä määritetty milloin `[Authorize]` tunniste on osumien.

```C#
// Controllers\HomeController.cs

// You can use the Authorize decorator to execute a certain policy if the user is not already signed into the app.
[Authorize]
public ActionResult Claims()
{
  ...
```

Voit käyttää myös OWIN sovelluksen käyttäjän ulos. In `Controllers\AccountController.cs`:  

```C#
// Controllers\AccountController.cs

public void Logout()
{
    // To sign out the user, you should issue an OpenIDConnect sign out request.
    if (Request.IsAuthenticated)
    {
        IEnumerable<AuthenticationDescription> authTypes = HttpContext.GetOwinContext().Authentication.GetAuthenticationTypes();
        HttpContext.GetOwinContext().Authentication.SignOut(authTypes.Select(t => t.AuthenticationType).ToArray());
        Request.GetOwinContext().Authentication.GetAuthenticationTypes();
    }
}
```

## <a name="display-user-information"></a>Näyttää Käyttäjätiedot
Käyttäjien todentamiseen käyttämällä OpenID yhteyden, kun Azure AD palauttaa ID-tunnusta, joka sisältää **saatavat**-sovellukseen. Nämä ovat vahvistukset käyttäjästä. Voit mukauttaa sovelluksen saatavat.  

Avaa `Controllers\HomeController.cs` tiedosto. Voit käyttää käyttäjän saatavat oman ohjaimet kautta `ClaimsPrincipal.Current` suojaus Pääobjektin.

```C#
// Controllers\HomeController.cs

[Authorize]
public ActionResult Claims()
{
    Claim displayName = ClaimsPrincipal.Current.FindFirst(ClaimsPrincipal.Current.Identities.First().NameClaimType);
    ViewBag.DisplayName = displayName != null ? displayName.Value : string.Empty;
    return View();
}
```

Voit käyttää ryhmän, joka sovelluksesi vastaanottaa samalla tavalla.  Kaikki sovelluksen vastaanottaa saatavat luettelo on käytettävissä **saatavat** -sivulla.

## <a name="run-the-sample-app"></a>Suorita malli-sovellus

Lopuksi voit luoda ja suorittaa sovelluksen. Tilaa sovelluksen käyttämällä sähköposti- tai käyttäjän nimi. Kirjaudu ulos ja kirjaudu takaisin sisään sama käyttäjä. Muokkaa kyseisen käyttäjän profiiliin. Kirjaudu ulos ja eri käyttäjänä tilaa. Huomaa, että **saatavat** -välilehdessä tiedot vastaavat tiedot, jotka olet määrittänyt oman käytännöt.

## <a name="add-social-idps"></a>Lisää sosiaalisen IDPs

Sovellus tukee tällä hetkellä vain käyttäjän kirjautuminen ja kirjaudu sisään käyttämällä **paikalliset tilit**. Nämä ovat B2C hakemistossa tallennetut tilit, jotka käyttävät käyttäjänimi ja salasana. Azure AD B2C avulla voit lisätä tuki **tunnistetietojen toimittajat** (IDPs) muuttamatta koodisi.

Voit lisätä sovelluksen sosiaalisen IDPs, Aloita näistä artikkeleista yksityiskohtaisia ohjeita noudattamalla. Kunkin IDP haluat tukea sinun on sovellus rekisteröidään järjestelmän ja hanki on asiakas.

- [Facebook IDP määrittäminen](active-directory-b2c-setup-fb-app.md)
- [Määritetty Google IDP](active-directory-b2c-setup-goog-app.md)
- [Amazon IDP määrittäminen](active-directory-b2c-setup-amzn-app.md)
- [LinkedIn IDP määrittäminen](active-directory-b2c-setup-li-app.md)

Kun olet lisännyt tunnistetietojen toimittajat B2C-kansioon, sinun on muokattava kunkin oman kolme käytännöt sisältämään uuden IDPs [käytännön viitata artikkelissa](active-directory-b2c-reference-policies.md)kuvatulla tavalla. Kun olet tallentanut oman käytäntöjä, suorita sovellus uudelleen.  Näkyviin tulee uusi IDPs lisätään Kirjaudu sisään ja kirjautumisvaihtoehdot kussakin henkilöllisyytesi kohtaa.

Voit kokeilla oman käytännöt ja noudata tehosteen malli-sovellukseen. Lisää tai poista IDPs, käsitellä sovelluksen vaateet tai vaihtaa rekisteröitymistoiminto määritteet. Kokeile, kunnes näet, miten käytäntöjä, käyttöoikeuksien ja OWIN sitominen yhdessä.

Viitteen, esimerkki valmiista (ilman määritysarvot), [on annettu .zip-tiedostona](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet-SUSI/archive/complete.zip). Voit myös Kloonaa se GitHub kohteesta:

```
git clone --branch complete https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet-SUSI.git
```

<!--

## Next steps

You can now move on to more advanced B2C topics. You might try:

[Call a web API from a web app]()

[Customize the UX for a B2C app]()

-->
