<properties
    pageTitle="Azure AD v2.0 .NET Web Appin | Microsoft Azure"
    description="Muodostaminen .NET MVC verkkosovellukseen puhelut sivuston palveluihin henkilökohtainen Microsoft-tilit ja työpaikan tai oppilaitoksen tilit-kirjautuminen."
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
    ms.date="10/27/2016"
    ms.author="dastrock"/>

# <a name="calling-a-web-api-from-a-net-web-app"></a>Verkko-Ohjelmointirajapinnan kutsumista .NET web Appista

V2.0 päätepisteissä voit nopeasti lisätä todentaminen web Apps-sovellusten ja verkko-ohjelmointirajapinnan tuki sekä henkilökohtainen Microsoft-tilit ja työpaikan tai oppilaitoksen tilit.  Tässä on luoda MVC-verkkosovelluksen tietokenttiä kirjautuu käyttäjien OpenID yhdistämistä, käyttämällä joitakin Microsoftin OWIN middleware avulla.  Web-sovelluksen OAuth 2.0 access tunnusten hankkiminen api suojattu OAuth 2.0, jonka avulla luoda sivuston, lue ja poistaa tietyn käyttäjän "Tehtävät-luettelon".

Tässä opetusohjelmassa keskitytään ensisijaisesti käyttämällä MSAL hankkia ja käyttää access tunnusten kuvattu koko [Tässä](active-directory-v2-flows.md#web-apps)online-sovelluksessa.  Etukäteen, haluat ehkä ensin tietoja lisäämisestä [basic - kirjautuminen web App-sovellukseen](active-directory-v2-devquickstarts-dotnet-web.md) tai [Verkko-Ohjelmointirajapinnan](active-directory-v2-devquickstarts-dotnet-api.md)suojaamisesta oikein.

> [AZURE.NOTE]
    V2.0 päätepisteen tukee kaikkia Azure Active Directory-skenaariot ja toiminnot.  Voit selvittää, jos sinun on käytettävä v2.0 päätepisteen, lue lisätietoja [v2.0 rajoitukset](active-directory-v2-limitations.md).

## <a name="download-sample-code"></a>Ladata malli

Tässä opetusohjelmassa koodi määritetään [GitHub](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet).  Jos haluat seurata, voit [ladata sovelluksen rakenne .zip nimellä](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet/archive/skeleton.zip) tai Kloonaa rakenne:

```git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet.git```

Vaihtoehtoisesti voit [ladata .zip kuin valmiin sovelluksen](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet/archive/complete.zip) tai valmiin sovelluksen Kloonaa:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet.git```

## <a name="register-an-app"></a>Sovelluksen rekisteröiminen
Luo uuden sovelluksen [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)tai noudattamalla seuraavia [toimimalla seuraavasti](active-directory-v2-app-registration.md).  Varmista, että:

- Kopioi alaspäin **Sovellustunnus** sovelluksen myönnetyt, sinun on se pian.
- Luo **Sovelluksen salaisuus** **salasana** -tyypin ja kopioi alaspäin arvonsa myöhempää käyttöä varten
- Lisää **WWW** -ympäristö, kun sovellus.
- Kirjoita oikeat **Uudelleenohjata URI**. Redirect uri ilmaisee, jossa todennus vastaukset olisi ohjautuu – Tässä opetusohjelmassa oletusarvo on Azure AD `https://localhost:44326/`.


## <a name="install-owin"></a>Asenna OWIN
Lisätä OWIN middleware NuGet-paketteja, `TodoList-WebApp` project-pakettien hallinta-konsolin avulla.  OWIN middleware käytetään antaa Kirjaudu sisään ja kirjaudu ulos pyyntöjä ja hallita käyttäjän istunnon käyttäjän, muun muassa tietoja.

```
PM> Install-Package Microsoft.Owin.Security.OpenIdConnect -ProjectName TodoList-WebApp
PM> Install-Package Microsoft.Owin.Security.Cookies -ProjectName TodoList-WebApp
PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TodoList-WebApp
```

## <a name="sign-the-user-in"></a>Käyttäjän kirjautuminen
Nyt määrittää OWIN middleware [OpenID yhteyden todennus-protokollaa](active-directory-v2-protocols.md#openid-connect-sign-in-flow).  

-   Avaa `web.config` ylimmällä tiedoston `TodoList-WebApp` project ja antaa sinua sovelluksen kokoonpanon arvoja `<appSettings>` osa.
    -   `ida:ClientId` On määritetty sovelluksen rekisteröinnin portaalissa **Tunnus** .
    - `ida:ClientSecret` On **Sovelluksen salaisuus** luomasi rekisteröinti-portaalissa.
    -   `ida:RedirectUri` On lisätty portaalin **Uudelleenohjata Uri** .
- Avaa `web.config` ylimmällä tiedoston `TodoList-Service` project ja korvaa `ida:Audience` sama **Tunnus** kuin yllä.


- Avaa tiedosto `App_Start\Startup.Auth.cs` ja lisää `using` edellä kirjastot-lauseet.
- Samaa tiedostoa, valitse Toteuta `ConfigureAuth(...)` menetelmää.  Saadaan parametreille `OpenIDConnectAuthenticationOptions` toimivat koordinaatteja, kun sovellus Azure AD yhteydessä.

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
                    Scope = "openid email profile offline_access",
                    RedirectUri = redirectUri,
                    PostLogoutRedirectUri = redirectUri,
                    TokenValidationParameters = new TokenValidationParameters
                    {
                        ValidateIssuer = false,
                    },

                    // The `AuthorizationCodeReceived` notification is used to capture and redeem the authorization_code that the v2.0 endpoint returns to your app.

                    Notifications = new OpenIdConnectAuthenticationNotifications
                    {
                        AuthenticationFailed = OnAuthenticationFailed,
                        AuthorizationCodeReceived = OnAuthorizationCodeReceived,
                    }

        });
}
// ...
```

## <a name="use-msal-to-get-access-tokens"></a>Access-tunnusten MSAL avulla
Valitse `AuthorizationCodeReceived` ilmoituksen haluamme lunastaa varten access-tunnuksen Tehtävät-luettelon palveluun authorization_code [OAuth 2.0 Yritystietopalveluiden kanssa OpenID yhteyden](active-directory-v2-protocols.md#openid-connect-with-oauth-code-flow) avulla.  MSAL voit tehdä tämän prosessin helposti puolestasi:

- Asenna ensin MSAL preview-versio:

```PM> Install-Package Microsoft.Identity.Client -ProjectName TodoList-WebApp -IncludePrerelease```
- Ja lisätä toisen `using` -lauseen `App_Start\Startup.Auth.cs` MSAL-tiedosto.
- Lisää nyt uusi menetelmä `OnAuthorizationCodeReceived` tapahtumakäsittelijä.  Tämä käsittelytoiminto käyttämällä MSAL hankkia access-tunnuksen API Tehtävät-luettelo ja tallentaa tunnuksen MSAL päivän vahvistustunnuksen välimuistin myöhempää käyttöä varten:

```C#
private async Task OnAuthorizationCodeReceived(AuthorizationCodeReceivedNotification notification)
{
        string userObjectId = notification.AuthenticationTicket.Identity.FindFirst("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier").Value;
        string tenantID = notification.AuthenticationTicket.Identity.FindFirst("http://schemas.microsoft.com/identity/claims/tenantid").Value;
        string authority = String.Format(CultureInfo.InvariantCulture, aadInstance, tenantID, string.Empty);
        ClientCredential cred = new ClientCredential(clientId, clientSecret);

        // Here you ask for a token using the web app's clientId as the scope, since the web app and service share the same clientId.
        app = new ConfidentialClientApplication(Startup.clientId, redirectUri, cred, new NaiveSessionCache(userObjectId, notification.OwinContext.Environment["System.Web.HttpContextBase"] as HttpContextBase)) {};
        var authResult = await app.AcquireTokenByAuthorizationCodeAsync(new string[] { clientId }, notification.Code);
}
// ...
```

- Web apps-MSAL on extensible suojaustunnuksen välimuisti, jonka avulla voidaan tallentaa tunnukset.  Tässä esimerkissä toteuttaa `NaiveSessionCache` välimuistin tunnusten http istunnon tallennusvälineiden käyttäen.

<!-- TODO: Token Cache article -->


## <a name="call-the-web-api"></a>Verkko-Ohjelmointirajapinnan kutsu
Nyt on käytettävä vaiheessa 3 on hankittu access_token todella aika.  Avaa web-sovelluksen `Controllers\TodoListController.cs` tiedosto, joka tekee API Tehtävät-luettelon CRUD-pyynnöt.

- Voit käyttää MSAL uudelleen tähän hakeaksesi access_tokens MSAL välimuistista.  Lisää ensin `using` MSAL tämän tiedoston tietosuojatiedot.

    `using Microsoft.Identity.Client;`

- Valitse `Index` toiminnon, käytä MSAL `AcquireTokenSilentAsync` menetelmä saat access_token, jonka avulla voidaan lukea tietoja tehtäväluettelo-palvelusta:

```C#
// ...
string userObjectID = ClaimsPrincipal.Current.FindFirst("http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier").Value;
string tenantID = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/tenantid").Value;
string authority = String.Format(CultureInfo.InvariantCulture, Startup.aadInstance, tenantID, string.Empty);
ClientCredential credential = new ClientCredential(Startup.clientId, Startup.clientSecret);

// Here you ask for a token using the web app's clientId as the scope, since the web app and service share the same clientId.
app = new ConfidentialClientApplication(Startup.clientId, redirectUri, credential, new NaiveSessionCache(userObjectID, this.HttpContext)){};
result = await app.AcquireTokenSilentAsync(new string[] { Startup.clientId });
// ...
```

- Otosten Lisää tuloksena tunnuksen kuin HTTP GET-pyyntö `Authorization` otsikosta, jossa tehtäväluettelo-palvelu käyttää tarkistamiseen pyynnön.
- Jos tehtäväluettelo-palvelu palauttaa `401 Unauthorized` vastausta, valitse MSAL access_tokens on saattavat muuttua virheellisiksi jostain syystä.  Tässä tapauksessa olisi Avaa mikä tahansa access_tokens MSAL välimuistin ja näyttää käyttäjän sanoma, jossa ne on ehkä kirjautua sisään uudelleen, joka käynnistää tunnuksen hankkiminen kulun.

```C#
// ...
// If the call failed with access denied, then drop the current access token from the cache,
// and show the user an error indicating they might need to sign-in again.
if (response.StatusCode == System.Net.HttpStatusCode.Unauthorized)
{
        app.AppTokenCache.Clear(Startup.clientId);

        return new RedirectResult("/Error?message=Error: " + response.ReasonPhrase + " You might need to sign in again.");
}
// ...
```

- Vastaavasti jos MSAL ei voi palauttaa access_token jostakin syystä, sinun kannattaa opasta käyttäjän kirjautua sisään uudelleen.  Tämä onnistuu helposti vain pyytämisestä jokin `MSALException`:

```C#
// ...
catch (MsalException ee)
{
        // If MSAL could not get a token silently, show the user an error indicating they might need to sign in again.
        return new RedirectResult("/Error?message=An Error Occurred Reading To Do List: " + ee.Message + " You might need to log out and log back in.");
}
// ...
```

- Tarkka sama `AcquireTokenSilentAsync` kutsu on implementd- `Create` ja `Delete` toiminnot.  Web Apps-sovelluksissa voit MSAL tätä menetelmää access_tokens saat aina, kun tarvitset niitä sovelluksen.  MSAL huolehtia hankkiminen ja tallentamisesta välimuistiin päivittäminen tunnusten puolestasi.

Lopuksi luominen ja suorittaminen sovelluksen!  Kirjaudu sisään Microsoft-Account tai Azure AD-tiliä, niin huomaat, miten käyttäjän tunnistetiedot näkyy siirtymispalkissa.  Lisää ja Poista kohteita käyttäjän Tehtävät-luettelon ja tarkastella OAuth-2.0 suojattu API puheluiden toiminnassa.  Web-sovelluksen ja verkko-Ohjelmointirajapinnan molemmat suojattu alan vakio protokollia, joka voi todentaa niiden henkilökohtainen ja työpaikan tai oppilaitoksen tilin käyttäjille on nyt.

Viitteen, esimerkki valmiista (ilman määritysarvot), [on annettu tähän](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-WebAPI-OpenIdConnect-DotNet/archive/complete.zip).  

## <a name="next-steps"></a>Seuraavat vaiheet

Muita tietolähteitä Kuittaa ulos:
- [V2.0 Sovelluskehittäjän opas >>](active-directory-appmodel-v2-overview.md)
- [StackOverflow "azure-active directory-tunniste >>](http://stackoverflow.com/questions/tagged/azure-active-directory)

## <a name="get-security-updates-for-our-products"></a>Päivitysten hakeminen tuotteitamme

On suositeltavaa, kun suojauksen tapaukset esiintyvät käymällä [Tämän sivun](https://technet.microsoft.com/security/dd252948) ja tilaamalla neuvoa suojausvaroitusten ilmoitusten saaminen.
