<properties
    pageTitle="Azure Active Directory-B2C | Microsoft Azure"
    description="Kirjaudu sisään, kirjautuminen, jonka verkkosovelluksen muodostaminen ja Profiilin hallinta Azure Active Directory-B2C avulla."
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

# <a name="azure-ad-b2c-build-a-net-web-app"></a>Azure AD-B2C: Muodosta .NET verkkosovellukseen

<!-- TODO [AZURE.INCLUDE [active-directory-b2c-devquickstarts-web-switcher](../../includes/active-directory-b2c-devquickstarts-web-switcher.md)]-->

Azure Active Directory (Azure AD) B2C avulla voit lisätä tehokkaita Omatoiminen tunnistetietojen hallintaominaisuuksien web App-sovellukseen, muutaman lyhyt vaiheen avulla. Tässä artikkelissa käsitellään luomisesta .NET-mallin näkymä-ohjain (MVC) verkkosovellukseen, joka sisältää käyttäjän kirjautuminen, kirjaudu sisään ja Profiilin hallinta. Sovelluksen sisällytetään tuki kirjautumista ja kirjaudu sisään käyttämällä käyttäjänimi tai sähköpostiosoite ja käyttämällä yhteisöpalvelujen, kuten Facebook- ja Google.

## <a name="get-an-azure-ad-b2c-directory"></a>Hae Azure AD B2C-kansio

Ennen kuin voit käyttää Azure AD B2C, voit luoda hakemiston tai vuokraaja. Kansio on kaikkien käyttäjien, sovellukset, ryhmät ja lisää säilö.  Jos sitä ei ole vielä ole, ennen kuin voit [luoda hakemiston B2C](active-directory-b2c-get-started.md) edelleen tässä oppaassa.

## <a name="create-an-application"></a>Sovelluksen luominen

Seuraavaksi haluat luoda sovelluksen B2C hakemistossa. Näin pitää yhteyttä suojatusti sovelluksen tarvitsemiaan Azure AD-tietoja. Voit luoda sovelluksen noudattamalla [näitä ohjeita](active-directory-b2c-app-registration.md).  Muista:

- Sisällytä **web app ja verkko-Ohjelmointirajapinnan** sovellus.
- Kirjoita `https://localhost:44316/` kuin **uudelleenohjata URI**. Tässä esimerkissä koodin oletusarvoisen URL-osoite on.
- Kopioi **Tunnus** , jolla määritetään sovelluksesi alaspäin.  Tarvitset sen myöhemmin.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Luoda oman käytäntöjä

Azure AD B2C jokaisen käyttäjäkokemuksen on määritetty [käytännön](active-directory-b2c-reference-policies.md)avulla. Koodi-malli sisältää kolme tunnistetietojen kokemukset: Rekisteröidy, kirjaudu sisään ja Muokkaa profiilia. Haluat yhden käytännön kunkin tyypin [käytännön viitata artikkelissa](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy)kuvatulla tavalla.

>[AZURE.NOTE] Azure AD-B2C tukee myös yhdistetyn Rekisteröidy tai Kirjaudu käytäntö, jossa on esitelty ei ole tässä opetusohjelmassa.  Rekisteröinti tai Kirjaudu sisään käytännön näkyy [vastaavat tässä](active-directory-b2c-devquickstarts-web-dotnet-susi.md)opetusohjelmassa.

Kun olet luonut kolme käytäntöjä, muista:

- Valitse **Käyttäjätunnus kirjautuminen** tai **sähköpostin kirjautuminen** tunnistetietojen toimittajat-sivu.
- Valitse ilmoittautuminen käytännön **Näyttönimi** ja kirjautuminen määritteet.
- Valitse sovelluksena **Näyttönimi** -ryhmän varaa jokaisen käytännössä. Voit valita muita saatavat.
- Kopioi kunkin käytännön **nimen** , sen luomisen jälkeen. Tarvitset käytännön nimet myöhemmin.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Kun olet luonut kolme käytäntöjä, olet valmis luomaan sovelluksesi.  

## <a name="download-the-code-and-configure-authentication"></a>Lataa koodi ja käyttöoikeuksien määrittäminen

Tämä esimerkki [määritetään GitHub](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet)koodi. Voit luoda otosten kuitenkin, [Lataa rakenne projektin .zip-tiedosto](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet/archive/skeleton.zip). Voit myös kopioida rakenne:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet.git
```

Esimerkki valmiista on myös [käytettävissä .zip-tiedostona](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet/archive/complete.zip) tai `complete` saman säilö haaraa.

Kun olet ladannut sample code, Avaa Visual Studio .sln tiedosto avulla pääset alkuun.

Sovelluksen yhteydessä Azure AD B2C todennus-viestejä, joka määrittää käytännön, he haluavat osana HTTP-pyynnön suorittaminen. .NET-web-sovellusten voit käyttää Microsoftin OWIN middleware lähettää OpenID yhteyden todennus-pyyntöjä, suorita käytäntöjä, hallinta ja käyttäjien istunnot.

Aloita Lisää OWIN middleware NuGet pakettien projektiin Visual Studio paketin hallinta-konsolin avulla.

```
PM> Install-Package Microsoft.Owin.Security.OpenIdConnect
PM> Install-Package Microsoft.Owin.Security.Cookies
PM> Install-Package Microsoft.Owin.Host.SystemWeb
```

Avaa seuraavaksi `web.config` projektin ylimmällä tiedosto ja kirjoita sinua sovelluksen kokoonpanon arvoja `<appSettings>` ‑osassa korvaa olemassa olevat arvot.

```
<configuration>
  <appSettings>
    <add key="webpages:Version" value="3.0.0.0" />
    <add key="webpages:Enabled" value="false" />
    <add key="ClientValidationEnabled" value="true" />
    <add key="UnobtrusiveJavaScriptEnabled" value="true" />
    <add key="ida:Tenant" value="fabrikamb2c.onmicrosoft.com" />
    <add key="ida:ClientId" value="90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6" />
    <add key="ida:AadInstance" value="https://login.microsoftonline.com/{0}/v2.0/.well-known/openid-configuration?p={1}" />
    <add key="ida:RedirectUri" value="https://localhost:44316/" />
    <add key="ida:SignUpPolicyId" value="b2c_1_sign_up" />
    <add key="ida:SignInPolicyId" value="b2c_1_sign_in" />
    <add key="ida:UserProfilePolicyId" value="b2c_1_edit_profile" />
  </appSettings>
...
```

[AZURE.INCLUDE [active-directory-b2c-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

Lisää seuraavaksi OWIN-käynnistys luokan projektin nimi `Startup.cs`. Projektin hiiren kakkospainikkeella ja valitse **Lisää** ja **Uuden kohteen**Hae "OWIN." **Varmista, että voit muuttaa luokan ilmoituksen `public partial class Startup` **. Olemme toteutettu tähän luokkaan osa puolestasi toiseen tiedostoon. OWIN middleware kutsuu `Configuration(...)` menetelmää, kun sovellus käynnistyy. Tämä menetelmä Varmista puhelun `ConfigureAuth(...)`, jossa voit määrittää käyttöoikeuden, kun sovellus.

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

Avaa tiedosto `App_Start\Startup.Auth.cs` ja käyttöönotto `ConfigureAuth(...)` menetelmää.  Saadaan Parametrit `OpenIdConnectAuthenticationOptions` sovelluksen koordinaatit pitää yhteyttä Azure AD yhteyshenkilönä. Haluat myös eväste todentamisen määrittäminen. OpenID yhteyden middleware evästeet säilyttää käyttäjäistunnot, muun muassa.

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
    public static string SignUpPolicyId = ConfigurationManager.AppSettings["ida:SignUpPolicyId"];
    public static string SignInPolicyId = ConfigurationManager.AppSettings["ida:SignInPolicyId"];
    public static string ProfilePolicyId = ConfigurationManager.AppSettings["ida:UserProfilePolicyId"];

    public void ConfigureAuth(IAppBuilder app)
    {
        app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

        app.UseCookieAuthentication(new CookieAuthenticationOptions());

        // Configure OpenID Connect middleware for each policy
        app.UseOpenIdConnectAuthentication(CreateOptionsFromPolicy(SignUpPolicyId));
        app.UseOpenIdConnectAuthentication(CreateOptionsFromPolicy(ProfilePolicyId));
        app.UseOpenIdConnectAuthentication(CreateOptionsFromPolicy(SignInPolicyId));
    }

    // Used for avoiding yellow-screen-of-death
    private Task AuthenticationFailed(AuthenticationFailedNotification<OpenIdConnectMessage, OpenIdConnectAuthenticationOptions> notification)
    {
        notification.HandleResponse();
        if (notification.Exception.Message == "access_denied")
        {
            notification.Response.Redirect("/");
        }
        else
        {
            notification.Response.Redirect("/Home/Error?message=" + notification.Exception.Message);
        }

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
```

## <a name="send-authentication-requests-to-azure-ad"></a>Lähetä käyttöoikeuksien Azure AD
Sovellus on nyt määritetty oikein pitää yhteyttä Azure AD B2C OpenID yhteyden todennus-protokollan avulla.  OWIN on otettava varoen kaikkien työstämistä todennus viestejä, vahvistaminen Azure AD tunnusta ja ylläpito käyttäjäistunnon tietoja.  Kaikki pysyy on aloittaa kunkin käyttäjän työnkulku.

Kun käyttäjä valitsee **rekisteröitynyt**, **Kirjaudu sisään**tai **Muokkaa profiilia** web App-sovelluksessa, liittyvä toiminto käynnistetään `Controllers\AccountController.cs`. Joka tapauksessa voit käyttää valmiita OWIN menetelmiä käynnistettävän oikean käytäntö:

```C#
// Controllers\AccountController.cs

public void SignIn()
{
    if (!Request.IsAuthenticated)
    {
        // To execute a policy, you simply need to trigger an OWIN challenge.
        // You can indicate which policy to use by specifying the policy id as the AuthenticationType
        HttpContext.GetOwinContext().Authentication.Challenge(
            new AuthenticationProperties () { RedirectUri = "/" }, Startup.SignInPolicyId);
    }
}

public void SignUp()
{
    if (!Request.IsAuthenticated)
    {
        HttpContext.GetOwinContext().Authentication.Challenge(
            new AuthenticationProperties() { RedirectUri = "/" }, Startup.SignUpPolicyId);
    }
}


public void Profile()
{
    if (Request.IsAuthenticated)
    {
        HttpContext.GetOwinContext().Authentication.Challenge(
            new AuthenticationProperties() { RedirectUri = "/" }, Startup.ProfilePolicyId);
    }
}
```

Voit myös käyttää `[Authorize]` oman ohjaimet tunnistetta, joka edellyttää tiettyjen käytännön suorittamisen, jos käyttäjä ei ole kirjautunut sisään. Avaa `Controllers\HomeController.cs` ja lisätä `[Authorize]` tunnisteen saatavat valvojalle.  OWIN Valitsee viimeisen käytännön määritetty suorittamaan, kun Hyväksy-tunniste käynnistetään.

```C#
// Controllers\HomeController.cs

// You can use the Authorize decorator to execute a policy if the user is not already signed in the app.
[Authorize]
public ActionResult Claims()
{
  ...
```

Voit käyttää myös OWIN sovelluksen käyttäjän ulos. In `Controllers\AccountController.cs`:  

```C#
// Controllers\AccountController.cs

public void SignOut()
{
    // To sign out the user, you should issue an OpenIDConnect sign out request
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

Voit käyttää ryhmän, joka sovelluksesi vastaanottaa samalla tavalla.  Kaikki sovelluksen vastaanottaa vaateita luettelo on käytettävissä **saatavat** -sivulla.

## <a name="run-the-sample-app"></a>Suorita malli-sovellus

Lopuksi voit luoda ja suorittaa sovelluksesi. Tilaa sovelluksen käyttämällä sähköposti- tai käyttäjän nimi. Kirjaudu ulos ja kirjaudu takaisin sisään sama käyttäjä. Muokkaa kyseisen käyttäjän profiiliin. Kirjaudu ulos ja eri käyttäjänä tilaa. Huomaa, että **saatavat** -välilehdessä tiedot vastaavat tiedot, jotka olet määrittänyt oman käytännöt.

## <a name="add-social-idps"></a>Lisää sosiaalisen IDPs

Sovellus tukee tällä hetkellä vain käyttäjän kirjautuminen ja kirjaudu sisään käyttämällä **paikalliset tilit**. Nämä ovat B2C hakemistossa tallennetut tilit, jotka käyttävät käyttäjänimi ja salasana. Azure AD B2C avulla voit lisätä tuki **tunnistetietojen toimittajat** (IDPs) muuttamatta koodisi.

Voit lisätä sosiaalisia IDPs sovelluksen, Aloita näissä artikkeleissa yksityiskohtaisia ohjeita noudattamalla. Kunkin IDP haluat tukea sinun on sovelluksen rekisteröiminen, että järjestelmään ja saada on asiakas.

- [Facebook IDP määrittäminen](active-directory-b2c-setup-fb-app.md)
- [Määritetty Google IDP](active-directory-b2c-setup-goog-app.md)
- [Amazon IDP määrittäminen](active-directory-b2c-setup-amzn-app.md)
- [LinkedIn IDP määrittäminen](active-directory-b2c-setup-li-app.md)

Kun olet lisännyt tunnistetietojen toimittajat B2C-kansioon, sinun on muokattava kunkin oman kolme käytännöt sisältämään uuden IDPs [käytännön viitata artikkelissa](active-directory-b2c-reference-policies.md)kuvatulla tavalla. Kun olet tallentanut oman käytäntöjä, suorita sovellus uudelleen.  Näkyviin tulee uusi IDPs lisätään Kirjaudu sisään ja kirjautumisvaihtoehdot kussakin henkilöllisyytesi kohtaa.

Voit kokeilla oman käytännöt ja noudata tehosteen malli-sovellukseen. Lisää tai poista IDPs, käsitellä sovelluksen saatavat tai vaihtaa rekisteröitymistoiminto määritteet. Kokeile, kunnes näet, miten käytäntöjä, käyttöoikeuksien ja OWIN sitominen yhdessä.

Viitteen, esimerkki valmiista (ilman määritysarvot), [on annettu .zip-tiedostona](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet/archive/complete.zip). Voit myös Kloonaa se GitHub kohteesta:

```
git clone --branch complete https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet.git
```

<!--

## Next steps

You can now move on to more advanced B2C topics. You might try:

[Call a web API from a web app]()

[Customize the UX for a B2C app]()

-->
