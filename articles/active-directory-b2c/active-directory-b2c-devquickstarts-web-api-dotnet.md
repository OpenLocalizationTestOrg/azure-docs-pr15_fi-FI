<properties
    pageTitle="Azure Active Directory-B2C | Microsoft Azure"
    description="Voit soittaa verkko-Ohjelmointirajapinnan käyttämällä Azure Active Directory-B2C web-sovelluksen luominen."
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

# <a name="azure-ad-b2c-call-a-web-api-from-a-net-web-app"></a>Azure AD-B2C: Soita verkko-Ohjelmointirajapinnan .NET web Appista

Azure Active Directory (Azure AD) B2C avulla voit lisätä tehokkaita Omatoiminen tunnistetietojen hallintaominaisuudet web Apps-sovellusten ja web ohjelmointirajapinnan muutaman lyhyt vaiheen avulla. Tässä artikkelissa käsitellään, joka kutsuu verkko-Ohjelmointirajapinnan haltijan tunnusten käyttämällä .NET-mallin näkymä-ohjain (MVC) "tehtäväluettelo-web-sovelluksen luominen

Tässä artikkelissa ei käsitellä ottamisesta käyttöön, kirjaudu sisään, kirjautuminen ja Azure AD B2C profiilin hallinta. Se ohjeessa on puheluja web API, kun käyttäjä on jo todennettu. Jos et ole jo, olisi aloitetaan [.NET web app aloitusopas](active-directory-b2c-devquickstarts-web-dotnet.md) lisätietoja Azure AD B2C perusteet.

## <a name="get-an-azure-ad-b2c-directory"></a>Hae Azure AD B2C-kansio

Ennen kuin voit käyttää Azure AD B2C, voit luoda hakemiston tai palveltavan.  Kansio on kaikkien käyttäjien, sovellukset, ryhmiä ja lisää säilö.  Jos sitä ei ole vielä ole, ennen kuin voit [luoda hakemiston B2C](active-directory-b2c-get-started.md) edelleen tässä oppaassa.

## <a name="create-an-application"></a>Sovelluksen luominen

Seuraavaksi haluat luoda sovelluksen B2C hakemistossa. Näin pitää yhteyttä suojatusti sovelluksen tarvitsemiaan Azure AD-tietoja. Tässä tapauksessa web app- ja verkko-Ohjelmointirajapinnan edustaa yksi **Tunnus**, koska ne muodostavat yhteen looginen-sovellukseen. Voit luoda sovelluksen noudattamalla [näitä ohjeita](active-directory-b2c-app-registration.md). Muista:

- Sisällytä **web app ja verkko-Ohjelmointirajapinnan** sovellus.
- Kirjoita `https://localhost:44316/` **vastaa URL**-osoitteeksi. Tässä esimerkissä koodin oletusarvoisen URL-osoite on.
- Kopioi **Tunnus** , jolla määritetään sovelluksen. Tarvitset myös tämän myöhemmin.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Luoda oman käytäntöjä

Azure AD B2C jokaisen käyttäjäkokemuksen on määritetty [käytännön](active-directory-b2c-reference-policies.md)avulla. Web-sovelluksen sisältää kolme tunnistetietojen kokemukset: Rekisteröidy, kirjaudu sisään ja Muokkaa profiilia. Haluat yhden käytännön kunkin tyypin [käytännön viitata artikkelissa](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy)kuvatulla tavalla. Kun olet luonut kolme käytäntöjä, muista:

- Valitse kirjautumisen käytännön **Näyttönimi** ja ilmoittautuminen määritteet.
- Valitse jokaisen käytännön **Näyttönimi** ja **Objektitunnus** sovelluksen saatavat. Voit valita muita saatavat.
- Kopioi kunkin käytännön **nimen** , sen luomisen jälkeen. Tunnisteen pitäisi olla etuliite `b2c_1_`. Tarvitset käytännön nimet myöhemmin.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Luotuasi kolme käytännöt olet valmis luomaan sovelluksen.

Huomaa, että tässä artikkelissa ei käsitellä käyttämisestä käytännöt juuri luomasi. Lisätietoja siitä, kuinka käytännöt toimivat Azure AD B2C, aloita [.NET web app aloitusopas](active-directory-b2c-devquickstarts-web-dotnet.md).

## <a name="download-the-code"></a>Lataa koodi

Tämän opetusohjelman [määritetään GitHub](https://github.com/AzureADQuickStarts/B2C-WebApp-WebAPI-OpenIDConnect-DotNet)koodi. Voit luoda otosten kuitenkin, [Lataa rakenne projektin .zip-tiedosto](https://github.com/AzureADQuickStarts/B2C-WebApp-WebAPI-OpenIDConnect-DotNet/archive/skeleton.zip). Voit myös kopioida rakenne:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-WebApp-WebAPI-OpenIDConnect-DotNet.git
```

Valmis-sovellus on myös [saatavilla .zip-tiedostona](https://github.com/AzureADQuickStarts/B2C-WebApp-WebAPI-OpenIDConnect-DotNet/archive/complete.zip) tai `complete` saman säilö haaraa.

Kun olet ladannut sample code, Avaa Visual Studio .sln tiedosto avulla pääset alkuun.

## <a name="configure-the-task-web-app"></a>Tehtävän web App-sovelluksen määrittäminen

Saat `TaskWebApp` Azure AD B2C kanssa kommunikoimiseen haluat säätää muutamalla yhteiset parametrit. - `TaskWebApp` Avaa project `web.config` projektin ylimmällä tiedoston ja korvaa arvot `<appSettings>` osan. Voit jättää `AadInstance`, `RedirectUri`, ja `TaskServiceUrl` arvot sellaisina kuin ne ovat.

```
  <appSettings>
    
    ...
    
    <add key="ida:ClientId" value="90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6" />
    <add key="ida:AadInstance" value="https://login.microsoftonline.com/{0}/v2.0/.well-known/openid-configuration?p={1}" />
    <add key="ida:RedirectUri" value="https://localhost:44316/" />
    <add key="ida:SignUpPolicyId" value="b2c_1_sign_up" />
    <add key="ida:SignInPolicyId" value="b2c_1_sign_in" />
    <add key="ida:UserProfilePolicyId" value="b2c_1_edit_profile" />
    <add key="api:TaskServiceUrl" value="https://aadb2cplayground.azurewebsites.net" />
  </appSettings>
```

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]  <appSettings>
    <add key="webpages:Version" value="3.0.0.0" />
    <add key="webpages:Enabled" value="false" />
    <add key="ClientValidationEnabled" value="true" />
    <add key="UnobtrusiveJavaScriptEnabled" value="true" />
    <add key="ida:Tenant" value="fabrikamb2c.onmicrosoft.com" />
    <add key="ida:ClientId" value="90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6" />
    <add key="ida:ClientSecret" value="E:i~5GHYRF$Y7BcM" />
    <add key="ida:AadInstance" value="https://login.microsoftonline.com/{0}/v2.0/.well-known/openid-configuration?p={1}" />
    <add key="ida:RedirectUri" value="https://localhost:44316/" />
    <add key="ida:SignUpPolicyId" value="b2c_1_sign_up" />
    <add key="ida:SignInPolicyId" value="b2c_1_sign_in" />
    <add key="ida:UserProfilePolicyId" value="b2c_1_edit_profile" />
    <add key="api:TaskServiceUrl" value="https://aadb2cplayground.azurewebsites.net" />
  </appSettings>

## <a name="get-access-tokens-and-call-the-task-api"></a>Hae access tunnusten ja soita tehtävän Ohjelmointirajapinta

Tässä osassa käsitellään käyttämisestä vastaanotettu Kirjautumisvirheitä aiheuttavien Azure AD B2C tunnuksen käyttö verkko-Ohjelmointirajapinnan, joka on myös suojattu Azure AD B2C kanssa.

Tässä artikkelissa ei käsitellä suojaamisesta Ohjelmointirajapinnan tietoja. Lue, miten verkko-Ohjelmointirajapinnan todentaa pyynnöt suojatusti Azure AD B2C avulla, tutustu [Verkko-Ohjelmointirajapinnan aloittaminen artikkelissa](active-directory-b2c-devquickstarts-api-dotnet.md).

### <a name="save-the-sign-in-token"></a>Kirjaudu tallennetaan tunnus

Ensin todennetaan (käyttämällä jotakin oman käytännöt) ja kirjaudu sisään-tunnuksen vastaanottaa Azure AD B2C.  Jos et ole varma siitä, kuinka käytännöt, palaa ja yritä [.NET web app aloitusopas](active-directory-b2c-devquickstarts-web-dotnet.md) lisätietoja Azure AD B2C perusteet.

Avaa tiedosto `App_Start\Startup.Auth.cs`.  On tärkeää muutoksen, sinun on tehtävä `OpenIdConnectAuthenticationOptions` -sinun on määritettävä `SaveSignInToken = true`.

```C#
// App_Start\Startup.Auth.cs

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
        AuthenticationFailed = OnAuthenticationFailed,
    },
    Scope = "openid",
    ResponseType = "id_token",

    TokenValidationParameters = new TokenValidationParameters
    {
        NameClaimType = "name",
        
        // Add this line to reserve the sign in token for later use
        SaveSigninToken = true,
    },
};
```

### <a name="get-a-token-in-the-controllers"></a>Hae tunnus ohjaimet

`TasksController` Vastaava verkko-Ohjelmointirajapinnan-pyyntöjen lähettäminen Ohjelmointirajapinnan voi lukea, luoda ja poistaa tehtävien yhteydessä.  Koska Ohjelmointirajapinnan on suojattu Azure AD B2C, sinun on ensin hakea edellä vaiheessa tunnuksen.

```C#
// Controllers\TasksController.cs

public async Task<ActionResult> Index()
{
    try { 

        var bootstrapContext = ClaimsPrincipal.Current.Identities.First().BootstrapContext as System.IdentityModel.Tokens.BootstrapContext;
        
    ...
}
```

`BootstrapContext` Sisältää tunnuksen, joka on hankittu suorittamalla B2C käytännöt kirjautuminen.

### <a name="read-tasks-from-the-web-api"></a>Tehtävien lukea verkko-Ohjelmointirajapinnan

Kun sinulla tunnistetta, voit liittää sen HTTP `GET` pyynnön `Authorization` otsikon suojatusti Soita `TaskService`:

```C#
// Controllers\TasksController.cs

public async Task<ActionResult> Index()
{
    try { 

        ...

        HttpClient client = new HttpClient();
        HttpRequestMessage request = new HttpRequestMessage(HttpMethod.Get, serviceUrl + "/api/tasks");

        // Add the token acquired from ADAL to the request headers
        request.Headers.Authorization = new AuthenticationHeaderValue("Bearer", bootstrapContext.Token);
        HttpResponseMessage response = await client.SendAsync(request);

        if (response.IsSuccessStatusCode)
        {
            String responseString = await response.Content.ReadAsStringAsync();
            JArray tasks = JArray.Parse(responseString);
            ViewBag.Tasks = tasks;
            return View();
        }
        else
        {
            // If the call failed with access denied, show the user an error indicating they might need to sign-in again.
            if (response.StatusCode == System.Net.HttpStatusCode.Unauthorized)
            {
                return new RedirectResult("/Error?message=Error: " + response.ReasonPhrase + " You might need to sign in again.");
            }
        }

        return new RedirectResult("/Error?message=An Error Occurred Reading To Do List: " + response.StatusCode);
    }
    catch (Exception ex)
    {
        return new RedirectResult("/Error?message=An Error Occurred Reading To Do List: " + ex.Message);
    }
}

```

### <a name="create-and-delete-tasks-on-the-web-api"></a>Luo ja poista verkko-Ohjelmointirajapinnan tehtävät

Noudata samoissa, kun lähetät `POST` ja `DELETE` pyynnöt verkko-Ohjelmointirajapinnan, avulla `BootstrapContext` hakemiseen tunnuksen kirjautuminen. Olemme toteutettu luontitoiminto puolestasi. Voit kokeilla viimeistely poistotoiminto `TasksController.cs`.

## <a name="run-the-sample-app"></a>Suorita malli-sovellus

Lopuksi muodosta ja suorita sovellus. Rekisteröidy ja kirjaudu ja kirjautunut sisään käyttäjän tehtävien luominen. Kirjaudu ulos ja kirjaudu sisään eri käyttäjänä. Luo kyseisen käyttäjän tehtävät. Huomaa, kuinka tehtävät ovat tallennetut käyttäjäkohtainen-Ohjelmointirajapinnan, koska Ohjelmointirajapinnan poimii käyttäjän tunnistetiedot tunnuksen se saa.

Viitteen, valmis malli [on annettu .zip-tiedostona](https://github.com/AzureADQuickStarts/B2C-WebApp-WebAPI-OpenIDConnect-DotNet/archive/complete.zip). Voit myös Kloonaa se GitHub kohteesta:

```git clone --branch complete https://github.com/AzureADQuickStarts/B2C-WebApp-WebAPI-OpenIDConnect-DotNet.git```

<!--

## Next steps

You can now move on to more advanced B2C topics. You might try:

[Call a web API from a web app]()

[Customize the UX for a B2C app]()

-->
