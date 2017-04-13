<properties
    pageTitle="Azure AD-B2C | Microsoft Azure"
    description="Miten voit luoda .NET-verkko-Ohjelmointirajapinnan Azure Active Directory B2C, suojattu OAuth 2.0 access tunnusten todennuksen avulla."
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
    ms.topic="hero-article"
    ms.date="07/22/2016"
    ms.author="dastrock"/>

# <a name="azure-active-directory-b2c-build-a-net-web-api"></a>Azure Active Directory-B2C: Muodosta .NET verkko-Ohjelmointirajapinnan

<!-- TODO [AZURE.INCLUDE [active-directory-b2c-devquickstarts-web-switcher](../../includes/active-directory-b2c-devquickstarts-web-switcher.md)]-->

Azure Active Directory (Azure AD) B2C, jossa voit suojata verkko-Ohjelmointirajapinnan OAuth 2.0 access tunnusten avulla. Näiden tunnusten Salli asiakkaan sovellus, joka todentaa ohjelmointirajapinnan Azure AD B2C avulla. Tässä artikkelissa näytetään, miten voit luoda .NET-mallin näkymä-ohjain (MVC) "tehtäväluettelo-Ohjelmointirajapinta, jonka avulla käyttäjät CRUD tehtäviin. Web-Ohjelmointirajapinnan on suojattu Azure AD B2C ja sallii vain todennetut käyttäjät voivat hallita niiden tehtäväluettelo.

## <a name="create-an-azure-ad-b2c-directory"></a>Azure AD B2C-kansion luominen

Ennen kuin voit käyttää Azure AD B2C, voit luoda hakemiston tai palveltavan. Kansio on kaikkien käyttäjien, sovellukset, ryhmät ja lisää säilö. Jos sitä ei ole vielä ole, ennen kuin voit [luoda hakemiston B2C](active-directory-b2c-get-started.md) edelleen tässä oppaassa.

## <a name="create-an-application"></a>Sovelluksen luominen

Seuraavaksi haluat luoda sovelluksen B2C hakemistossa. Näin pitää yhteyttä suojatusti sovelluksen tarvitsemiaan Azure AD-tietoja. Voit luoda sovelluksen noudattamalla [näitä ohjeita](active-directory-b2c-app-registration.md). Muista:

- Sisällytä **verkkosovellukseen** tai **Verkko-Ohjelmointirajapinnan** sovellus.
- Käytä **uudelleenohjata uniform resource identifier** `https://localhost:44316/` web-sovelluksen. Tämä on koodin tässä esimerkissä web app-asiakasohjelman oletussijainti.
- Kopioi **tunnus** , jolla määritetään sovelluksen. Tarvitset niitä myöhemmin.

 [AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Luoda oman käytäntöjä

Azure AD B2C jokaisen käyttäjäkokemuksen on määritetty [käytännön](active-directory-b2c-reference-policies.md)avulla. Asiakkaan koodi tässä esimerkissä on kolme tunnistetietojen kokemukset: Rekisteröidy, kirjaudu sisään ja Muokkaa profiilia. Sinun on kunkin tyyppiselle käytännön luomisesta, [käytännön viitata artikkelissa](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy)kuvatulla tavalla. Kun olet luonut kolme käytäntöjä, muista:

- Valitse **Käyttäjätunnus kirjautuminen** tai **sähköpostin kirjautuminen** tunnistetietojen toimittajat-sivu.
- Valitse kirjautumisen käytännön **Näyttönimi** ja ilmoittautuminen määritteet.
- Valitse sovelluksen saatavat jokaisen käytännön **Näyttönimi** ja **Objektitunnus** saatavat. Voit valita muita saatavat.
- Kopioi kunkin käytännön **nimen** , sen luomisen jälkeen. Tarvitset seuraavat käytännön nimet myöhemmin.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Kun olet luonut kolme käytäntöjä, voit ryhtyä sovelluksen luominen.

## <a name="download-the-code"></a>Lataa koodi

Tämän opetusohjelman [määritetään GitHub](https://github.com/AzureADQuickStarts/B2C-WebAPI-DotNet)koodi. Voit luoda otosten kuitenkin, [Lataa rakenne projektin .zip-tiedostona](https://github.com/AzureADQuickStarts/B2C-WebAPI-DotNet/archive/skeleton.zip). Voit myös kopioida rakenne:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-WebAPI-DotNet.git
```

Valmis-sovellus on myös [saatavilla .zip-tiedostona](https://github.com/AzureADQuickStarts/B2C-WebAPI-DotNet/archive/complete.zip) tai `complete` saman säilö haaraa.

Kun olet ladannut sample code, Avaa Visual Studio .sln tiedosto avulla pääset alkuun. Ratkaisutiedosto sisältää kaksi projektia: `TaskWebApp` ja `TaskService`. `TaskWebApp`on MVC-web-sovellus, jonka käyttäjä käsittelee. `TaskService`on sovelluksen taustatietokantaan verkko-Ohjelmointirajapinnan, joka tallentaa kunkin käyttäjän Tehtävät-luettelo.

## <a name="configure-the-task-web-app"></a>Tehtävän web App-sovelluksen määrittäminen

Kun käyttäjä toimii kanssa `TaskWebApp`, asiakkaan lähettää pyynnöt Azure AD ja saa takaisin tunnuksia, joita voidaan käyttää Soita `TaskService` verkko-Ohjelmointirajapinnan. Käyttäjän kirjautuminen ja hakea tunnuksia, sinun on annettava `TaskWebApp` tietoja sovelluksen kanssa. - `TaskWebApp` Avaa project `web.config` projektin ylimmällä tiedoston ja korvaa arvot `<appSettings>` osan.  Voit jättää `AadInstance`, `RedirectUri`, ja `TaskServiceUrl` arvot muodossa-on.

```
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
    <add key="api:TaskServiceUrl" value="https://localhost:44332/" />
  </appSettings>
```

Tässä artikkelissa ei käsitellä rakennuksen `TaskWebApp` asiakas.  Lisätietoja muodostaminen Azure AD B2C-sovelluksessa on artikkelissa [Microsoftin .NET web app-opas](active-directory-b2c-devquickstarts-web-dotnet.md).

## <a name="secure-the-api"></a>Suojattu Ohjelmointirajapinnan

Kun sinulla on jokin muu sähköpostiohjelma, joka kutsuu Ohjelmointirajapinnan käyttäjien puolesta, voit suojata `TaskService` käyttämällä OAuth 2.0 haltijan-tunnukset. Oman API voit hyväksyä ja vahvista tunnusten .NET (OWIN) kirjaston Microsoftin Avaa Web-käyttöliittymän avulla.

### <a name="install-owin"></a>Asenna OWIN
Aloita asentamalla OWIN OAuth-todennus myyntijakso:

```
PM> Install-Package Microsoft.Owin.Security.OAuth -ProjectName TaskService
PM> Install-Package Microsoft.Owin.Security.Jwt -ProjectName TaskService
PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TaskService
```

### <a name="enter-your-b2c-details"></a>Kirjoita B2C tiedot
Avaa `web.config` ylimmällä tiedoston `TaskService` project ja korvaa arvot `<appSettings>` osa. Nämä arvot käytetään koko API ja OWIN-kirjasto.  Voit jättää `AadInstance` arvo ei muutu.

```
  <appSettings>
    <add key="webpages:Version" value="3.0.0.0" />
    <add key="webpages:Enabled" value="false" />
    <add key="ClientValidationEnabled" value="true" />
    <add key="UnobtrusiveJavaScriptEnabled" value="true" />
    <add key="ida:AadInstance" value="https://login.microsoftonline.com/{0}/v2.0/.well-known/openid-configuration?p={1}" />
    <add key="ida:Tenant" value="fabrikamb2c.onmicrosoft.com" />
    <add key="ida:ClientId" value="90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6" />
    <add key="ida:SignUpPolicyId" value="b2c_1_sign_up" />
    <add key="ida:SignInPolicyId" value="b2c_1_sign_in" />
    <add key="ida:UserProfilePolicyId" value="b2c_1_edit_profile" />
  </appSettings>
```

### <a name="add-an-owin-startup-class"></a>Lisää OWIN-käynnistys-luokka
Lisää OWIN käynnistys-luokka, `TaskService` projektin nimi `Startup.cs`.  Projektin hiiren kakkospainikkeella, valitse **Lisää** ja **Uuden kohteen**ja Hae OWIN.


```C#
// Startup.cs

// Change the class declaration to "public partial class Startup" - we’ve already implemented part of this class for you in another file.
public partial class Startup
{
    // The OWIN middleware will invoke this method when the app starts
    public void Configuration(IAppBuilder app)
    {
        ConfigureAuth(app);
    }
}
```

### <a name="configure-oauth-20-authentication"></a>Määritä OAuth 2.0-todennus
Avaa tiedosto `App_Start\Startup.Auth.cs`, ja `ConfigureAuth(...)` menetelmää:

```C#
// App_Start\Startup.Auth.cs

public partial class Startup
{
    // These values are pulled from web.config
    public static string aadInstance = ConfigurationManager.AppSettings["ida:AadInstance"];
    public static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];
    public static string clientId = ConfigurationManager.AppSettings["ida:ClientId"];
    public static string signUpPolicy = ConfigurationManager.AppSettings["ida:SignUpPolicyId"];
    public static string signInPolicy = ConfigurationManager.AppSettings["ida:SignInPolicyId"];
    public static string editProfilePolicy = ConfigurationManager.AppSettings["ida:UserProfilePolicyId"];

    public void ConfigureAuth(IAppBuilder app)
    {   
        app.UseOAuthBearerAuthentication(CreateBearerOptionsFromPolicy(signUpPolicy));
        app.UseOAuthBearerAuthentication(CreateBearerOptionsFromPolicy(signInPolicy));
        app.UseOAuthBearerAuthentication(CreateBearerOptionsFromPolicy(editProfilePolicy));
    }

    public OAuthBearerAuthenticationOptions CreateBearerOptionsFromPolicy(string policy)
    {
        TokenValidationParameters tvps = new TokenValidationParameters
        {
            // This is where you specify that your API only accepts tokens from its own clients
            ValidAudience = clientId,
            AuthenticationType = policy,
        };

        return new OAuthBearerAuthenticationOptions
        {
            // This SecurityTokenProvider fetches the Azure AD B2C metadata & signing keys from the OpenIDConnect metadata endpoint
            AccessTokenFormat = new JwtFormat(tvps, new OpenIdConnectCachingSecurityTokenProvider(String.Format(aadInstance, tenant, policy))),
        };
    }
}
```

### <a name="secure-the-task-controller"></a>Suojattu tehtävä-ohjain
Kun sovellus on määritetty käyttämään OAuth 2.0-todennusta, voit suojata verkko-Ohjelmointirajapinnan lisäämällä `[Authorize]` tunnisteen tehtävän valvojalle. Tämä on ohjauskoneen, jossa kaikki tehtävät-luettelon käytöstä tapahtuu, jotta suojattava koko ohjauskoneen luokan tasolla. Voit lisätä `[Authorize]` tunnisteen monipuolisempaa toiminnoista.

```C#
// Controllers\TasksController.cs

[Authorize]
public class TasksController : ApiController
{
    ...
}
```

### <a name="get-user-information-from-the-token"></a>Pyydä käyttäjätiedot tunnus
`TasksController`tallentaa tietokannan kunkin tehtävän, joiden on liitetty käyttäjän "" Tehtävän omistavan tehtävät. Omistaja tunnistetaan käyttäjän **Objektitunnus**. (Tämän vuoksi lisäämiseen Objektitunnus sovelluksena tarvittavia varaa kaikista oman käytännöt.)

```C#
// Controllers\TasksController.cs

public IEnumerable<Models.Task> Get()
{
    string owner = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier").Value;
    IEnumerable<Models.Task> userTasks = db.Tasks.Where(t => t.owner == owner);
    return userTasks;
}
```

## <a name="run-the-sample-app"></a>Suorita malli-sovellus

Lopuksi luominen ja suorittaminen molemmat `TaskWebApp` ja `TaskService`. Tilaa sovelluksen käyttämällä sähköpostiosoite tai käyttäjänimi. Luo joitakin tehtäviä käyttäjän Tehtävät-luettelon ja huomaat, miten ne säilyvät-Ohjelmointirajapinnan senkin jälkeen, kun Lopeta ja Käynnistä uudelleen asiakas.

## <a name="edit-your-policies"></a>Muokkaa yhteyttä käytännöt

Jälkeen on suojattu Ohjelmointirajapinnan Azure AD B2C avulla, voit kokeilla sinua sovelluksen käytännöt ja tarkastella tehosteita (tai ei ole niiden)-Ohjelmointirajapinnan. Voit käsitellä sovelluksen saatavat toimintaperiaatteiden ja muuttaa käyttäjätietoja, joka on käytettävissä verkko-Ohjelmointirajapinnan. Saatavat, kun lisäät ovat käytettävissäsi .NET MVC sivustosi API `ClaimsPrincipal` objekti, tämän artikkelin ohjeiden mukaisesti.

<!--

## Next steps

You can now move onto more advanced B2C topics. You may try:

[Call a web API from a web app]()

[Customize the UX of your B2C app]()

-->
