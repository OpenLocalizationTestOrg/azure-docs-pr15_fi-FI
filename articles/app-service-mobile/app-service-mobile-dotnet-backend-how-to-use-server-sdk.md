<properties
    pageTitle=".NET palvelimeen SDK Mobile-sovellusten käsittelemisestä | Azure sovelluksen-palvelu"
    description="Lue, miten .NET palvelimeen SDK toimimaan Azure palvelun mobiilisovellukset."
    keywords="sovelluksen palvelu, azure app palvelu, mobiilisovelluksen, mobiilipalvelu skaalaus-skaalattava-sovelluksen käyttöönotto azure sovellusten käyttöönoton"
    services="app-service\mobile"
    documentationCenter=""
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-multiple"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="work-with-the-net-backend-server-sdk-for-azure-mobile-apps"></a>.NET palvelimeen SDK Azure Mobile-sovellusten käyttäminen

[AZURE.INCLUDE [app-service-mobile-selector-server-sdk](../../includes/app-service-mobile-selector-server-sdk.md)]

Tässä ohjeaiheessa esitellään .NET palvelimeen SDK-skenaariota Azure App palvelun Mobile-sovellusten käyttämisestä. Azure Mobile Apps SDK avulla voit käsitellä mobiilisovellukset ASP.NET-sovelluksesta.

>[AZURE.TIP] [.NET server Azure-mobiilisovellukset SDK] [ 2] on GitHub Avaa lähde. Säilö sisältää kaikki lähdekoodin, mukaan lukien koko palvelimen SDK yksikön testi-ohjelmistopaketissa ja jotkin otoksen projektit.

## <a name="reference-documentation"></a>Oppaat

Oppaat palvelimen SDK sijaitsee seuraavassa: [Azure Mobile sovellusten .NET viittaus][1].

## <a name="create-app"></a>Toimintaohje: taustassa .NET Mobile-sovelluksen luominen

Jos aloitat uuden projektin, voit luoda sovelluksen Service-sovellus [Azure portal] tai Visual Studio. Voit suorittaa sovellusten palvelusovellus paikallisesti tai julkaista projektin oman pilvipohjainen sovellus palvelun mobiilisovelluksessa.  

Jos olet lisäämässä mobile ominaisuuksia aiemmin luotu projekti on [ladata ja alusta SDK](#install-sdk) -osassa.

### <a name="create-a-net-backend-using-the-azure-portal"></a>Luo .NET taustassa Azure-portaalissa

Voit luoda mobile App Service-taustatietokannan joko noudattamalla [pikaopas opetusohjelma] [ 3] tai seuraavasti:

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service-classic](../../includes/app-service-mobile-dotnet-backend-create-new-service-classic.md)]

_Aloittaminen_ -sivu **taulukon luominen API**-kohdassa Valitse **C#** **Taustajärjestelmä kieli**. Valitse **Lataa**, Pura pakattu projektitiedostot paikalliseen tietokoneeseen ja avaa ratkaisun Visual Studiossa.

### <a name="create-a-net-backend-using-visual-studio-2013-and-visual-studio-2015"></a>Luo Visual Studio 2013 ja Visual Studio 2015 .NET taustassa

Asenna [Azure SDK.NET] [ 4] (versio 2.9.0 tai uudempi) Azure-mobiilisovellukset projektin luominen Visual Studio. Kun olet asentanut SDK, luoda ASP.NET-sovellukseen seuraavasti:

1. Avaa **Uusi projekti** -valintaikkuna ( *tiedostosta* > **Uusi** > **projektin...**).
2. **Mallit** > **Visual C#**ja valitse **WWW**.
3. **ASP.NET Web-sovelluksen**valitseminen
4. Täytä projektin nimeä. Valitse **OK**.
5. Valitse _ASP.NET 4.5.2 malleja_, valitse **Azure Mobile-sovelluksesta**. Tarkista **Host pilveen** mobile taustassa luominen, johon voit julkaista projektin pilveen.
6. Valitse **OK**.

## <a name="install-sdk"></a>Toimintaohje: Lataa ja SDK valmistelu

SDK on käytettävissä [NuGet.org]. Tämä paketti sisältää tarvitse käyttäminen SDK perustoiminnot. Alustaa SDK, sinun täytyy suorittaa toimintoja **HttpConfiguration** objektin.

### <a name="install-the-sdk"></a>Asenna SDK

Asenna SDK, palvelimen projektin Visual Studiossa hiiren kakkospainikkeella, valitse **Hallitse NuGet-paketti**, Etsi [Microsoft.Azure.Mobile.Server] -paketti ja valitse sitten **Asenna**.

###<a name="server-project-setup"></a>Alustaa server project

.NET Taustajärjestelmä palvelimen projektin on valmisteltu samalla ASP.NET muihin projekteihin sisällyttämällä OWIN-käynnistys-luokka. Varmista, että olet Viitattu NuGet paketin `Microsoft.Owin.Host.SystemWeb`. Jos haluat lisätä tähän luokkaan Visual Studiossa, palvelimen projektin hiiren kakkospainikkeella ja valitse **Lisää** > 
**Uusi kohde**ja valitse **Web** > **Yleiset** > **OWIN käynnistys-luokka**.  Luokka on muodostettu seuraava määrite:

    [assembly: OwinStartup(typeof(YourServiceName.YourStartupClassName))]

Valitse `Configuration()` OWIN-käynnistys luokan menetelmä voit määrittää Azure-mobiilisovellukset-ympäristö käyttämällä **HttpConfiguration** objektin.
Seuraavassa esimerkissä alustaa server-projekti ei ole lisätty ominaisuuksia:

    // in OWIN startup class
    public void Configuration(IAppBuilder app)
    {
        HttpConfiguration config = new HttpConfiguration();

        new MobileAppConfiguration()
            // no added features
            .ApplyTo(config);

        app.UseWebApi(config);
    }

Voit ottaa käyttöön yksittäisiä ominaisuuksia, on kutsu tunniste menetelmiä **MobileAppConfiguration** objektin ennen **ApplyTo**kutsumista. Esimerkiksi seuraava koodi Lisää oletusarvo-tiet kaikki API-ohjaimet, joilla on määrite `[MobileAppController]` aikana:

    new MobileAppConfiguration()
        .MapApiControllers()
        .ApplyTo(config);

Azure-portaalista server-pikaopas puhelujen **UseDefaultConfiguration()**. Tämä vastaa seuraavat asetukset:

        new MobileAppConfiguration()
            .AddMobileAppHomeController()             // from the Home package
            .MapApiControllers()
            .AddTables(                               // from the Tables package
                new MobileAppTableConfiguration()
                    .MapTableControllers()
                    .AddEntityFramework()             // from the Entity package
                )
            .AddPushNotifications()                   // from the Notifications package
            .MapLegacyCrossDomainController()         // from the CrossDomain package
            .ApplyTo(config);

Tunniste menetelmiä ovat seuraavat:

* `AddMobileAppHomeController()`sisältää Azure-mobiilisovellukset oletuskotisivu.
* `MapApiControllers()`mahdollistaa mukautetun API ominaisuuksia WebAPI ohjaimet päätteinen kanssa `[MobileAppController]` määrite.
* `AddTables()`sisältää määritystä `/tables` taulukon ohjaimet päätepisteet.
* `AddTablesWithEntityFramework()`on lyhyt-käden yhdistämistä varten `/tables` käyttämällä kohteen Framework päätepisteet perusteella ohjaimet.
* `AddPushNotifications()`on yksinkertainen menetelmä rekisteröiminen laitteiden ilmoituksen keskittimien.
* `MapLegacyCrossDomainController()`on vakio CORS otsikot paikallista kehittämistä.

### <a name="sdk-extensions"></a>SDK-laajennukset

Seuraavat NuGet perustuva tunniste-paketit on eri mobile ominaisuudet, joita voidaan käyttää sovelluksen. Tunnisteet käyttöön alustaminen käyttämällä **MobileAppConfiguration** objektia.

- [Microsoft.Azure.Mobile.Server.Quickstart] tukee mobiilisovellukset perusasetukset. Lisätä työnkulkuun soittamalla alustaminen **UseDefaultConfiguration** tunniste-menetelmää. Tämän tunnisteen sisältää seuraavat tunnisteet: ilmoitukset, todennus, kohteen, taulukoita, toimialueiden ja Home-paketit. Tämän paketin käyttämä Mobile sovellusten pikaopas käytettävissä Azure-portaalissa.

- [Microsoft.Azure.Mobile.Server.Home](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Home/) 
   toteuttaa oletusarvo *mobile-sovellus on asennettu ja sivun* sivuston pääkansion. Lisää työnkulkuun kutsumista   **AddMobileAppHomeController** tunniste.

- [Microsoft.Azure.Mobile.Server.Tables](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Tables/) 
   sisältää luokat tietojen käsitteleminen ja määrittää ylöspäin tietojen putkijohto. Lisää työnkulkuun kutsumista **AddTables** tunniste.

- [Microsoft.Azure.Mobile.Server.Entity](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Entity/) 
   mahdollistaa kohteen Framework käyttää tietoja SQL-tietokantaan. Lisää työnkulkuun kutsumista **AddTablesWithEntityFramework** tunniste.

- [Microsoft.Azure.Mobile.Server.Authentication] mahdollistaa todennuksen tai määrittää ylöspäin tunnusten vahvistamiseen käytetty OWIN middleware. Lisää työnkulkuun soittamalla **AddAppServiceAuthentication**  
   ja **IAppBuilder**. **UseAppServiceAuthentication** tunniste menetelmät.

- [Microsoft.Azure.Mobile.Server.Notifications] ottaa käyttöön push-ilmoitukset ja määrittää push-rekisteröinnin päätepiste. Lisää työnkulkuun kutsumista **AddPushNotifications** tunniste.

- [Microsoft.Azure.Mobile.Server.CrossDomain](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.CrossDomain/) 
   Luo ohjauskoneen, joka on tietoja vanhat selaimet Mobile-sovellukset. Lisää työnkulkuun kutsumista   **MapLegacyCrossDomainController** tunniste.

- [Microsoft.Azure.Mobile.Server.Login] AppServiceLoginHandler.CreateToken() kirjoitustuen, joka on mukautettu tarkistus skenaariot käytettyä staattinen menetelmän avulla.   

## <a name="publish-server-project"></a>Toimintaohje: julkaiseminen server project

Tässä osassa näytetään, miten voit julkaista Visual Studio .NET Taustajärjestelmä projektin. Voit myös ottaa Taustajärjestelmä projektin käyttämällä Git tai mitä tahansa muita tapoja kattaa [Azure App palvelun käyttöönoton ohjeet](../app-service-web/web-sites-deploy.md).

1. Visual Studiossa uudelleen projektin palauttaminen NuGet paketit.

2. Napsauta ratkaisunhallinnassa Napsauta hiiren kakkospainikkeella projektin, valitse **Julkaise**. Voit julkaista ensimmäistä kertaa, sinun täytyy määrittää julkaisun profiilin. Kun sinulla on jo määritetty profiili, voit napsauttamalla sitä ja valitse **Julkaise**.

2. Jos sinua pyydetään valitsemaan Julkaise kohteeseen, valitse **Microsoft Azure-sovelluksen palvelun** > **Seuraava**, valitse (tarvittaessa) Kirjautumisvirheitä aiheuttavien Azure tunnistetiedot. 
   Visual Studio-lataukset ja tallentaa turvallisesti yhteyttä suoraan Azure Julkaisuasetukset.

    ![](./media/app-service-mobile-dotnet-backend-how-to-use-server-sdk/publish-wizard-1.png)

3. Valitse **tilaus**, valitse **Resurssilaji** **-näkymästä**, laajenna **Mobile-sovelluksen**, ja valitse Mobile-sovelluksen Taustajärjestelmä ja valitse sitten **OK**.

    ![](./media/app-service-mobile-dotnet-backend-how-to-use-server-sdk/publish-wizard-2.png)

4. Julkaise profiilitiedot ja valitse **Julkaise**.

    ![](./media/app-service-mobile-dotnet-backend-how-to-use-server-sdk/publish-wizard-3.png)

    Kun Mobile-sovelluksen Taustajärjestelmä on julkaistu, näet, jotka osoittavat onnistumista siirtymissivu.

    ![](./media/app-service-mobile-dotnet-backend-how-to-use-server-sdk/publish-success.png)

##<a name="define-table-controller"></a>Toimintaohje: Määritä taulukko-ohjain

Määritä taulukon ohjauskoneen, voit näyttää SQL-taulukkoon mobiilisovellukset.  Taulukon ohjauskoneen määrittämisestä on kolme vaihetta:

1. Tietojen siirtäminen objektin (DTO)-luokan luominen
2. Määrittää taulukkoviittauksen Mobile DbContext-luokka.
3. Luo taulukko-ohjain.

Tietojen siirtäminen objektin (DTO) on tavallinen C#-objekti, jotka perivät `EntityData`.  Esimerkki:

    public class TodoItem : EntityData
    {
        public string Text { get; set; }
        public bool Complete {get; set;}
    }

DTO käytetään määrittämään taulukon SQL-tietokantaan.  Voit luoda tietokantaan-merkinnän lisääminen `DbSet<>` käytät DbContext-ominaisuutta.  Azure-mobiilisovellukset projektin malliin, DbContext kutsutaan `Models\MobileServiceContext.cs`:

    public class MobileServiceContext : DbContext
    {
        private const string connectionStringName = "Name=MS_TableConnectionString";

        public MobileServiceContext() : base(connectionStringName)
        {

        }

        public DbSet<TodoItem> TodoItems { get; set; }

        protected override void OnModelCreating(DbModelBuilder modelBuilder)
        {
            modelBuilder.Conventions.Add(
                new AttributeToColumnAnnotationConvention<TableColumnAttribute, string>(
                    "ServiceColumnTable", (property, attributes) => attributes.Single().ColumnType.ToString()));
        }
    }

Jos sinulla on asennettu Azure SDK-paketissa, voit nyt luoda mallin taulukon ohjaimen seuraavasti:

1. Napsauta ohjaimet-kansiota hiiren kakkospainikkeella ja valitse **Lisää** > **ohjauskoneen...**.
2. Valitse **Azure Mobile sovellusten taulukon Controller** -vaihtoehto ja valitse sitten **Lisää**.
3. **Lisää Controller** -valintaikkunassa:
    * Valitse uusi DTO **mallin luokan** avattavasta valikosta.
    * Valitse **DbContext** avattavasta valikosta Mobile palvelun DbContext-luokka.
    * Nimi ohjauskoneen on luonut puolestasi.
4. Valitse **Lisää**.

Pikaopas server-projekti on esimerkki Yksinkertainen **TodoItemController**.

### <a name="how-to-adjust-the-table-paging-size"></a>Toimintaohje: taulukon sivutus koon säätäminen

Oletusarvon mukaan Azure-mobiilisovellukset palauttaa pyynnön 50 tietueita.  Sivutuksen varmistaa, että asiakas vaadi niiden Käyttöliittymän viestiketjun eikä palvelimelta liian kauan varmistaa, että saman hyvän käyttökokemuksen. Taulukon sivutus koon muuttamiseen Suurenna "sallittu kyselyn koko" palvelinpuolen ja asiakaspuolen sivun koko palvelinpuolen "sallittu kyselyn koko" sovitetaan avulla `EnableQuery` määrite:

    [EnableQuery(PageSize = 500)]

Varmista, PageSize on sama tai suurempi kuin asiakkaan.  Lisätietoja tietyn asiakkaan ohjeet ohjeissa asiakkaan sivun koon muuttamisesta.

## <a name="how-to-define-a-custom-api-controller"></a>Toimintaohje: Määritä mukautettu API-ohjain

Mukautettu API-ohjain tarjoaa eniten perustoiminnot Mobile-sovelluksen Taustajärjestelmä paljastaa päätepisteen. Voit rekisteröidä mobile kielikohtaiset API-valvonta on [MobileAppController]-määritteen avulla. `MobileAppController` Määrite Rekisteröi reitin, määrittää Mobile-sovellusten JSON Sarjatoiminto ja ottaa käyttöön [asiakas-version tarkistaminen](app-service-mobile-client-and-server-versioning.md).

1. Visual Studiossa, napsauta ohjaimet-kansiota hiiren kakkospainikkeella ja valitse sitten **Lisää** > **ohjauskoneen**, valitse **Verkko-Ohjelmointirajapinnan 2-ohjain&mdash;tyhjä** ja sitten **Lisää**.

2. Anna **nimi ohjauskoneen**, kuten `CustomController`, ja valitse sitten **Lisää**.

3. Uuden ohjaimen luokan tiedoston, Lisää seuraava lauseella:

        using Microsoft.Azure.Mobile.Server.Config;

4. Käytä **[MobileAppController]** -määrite API ohjauskoneen luokan-määritys seuraavan esimerkin mukaisesti:

        [MobileAppController]
        public class CustomController : ApiController
        {
              //...
        }

4. Lisää App_Start/Startup.MobileApp.cs tiedoston puhelun **MapApiControllers** tunniste tapaan, kuten seuraavassa esimerkissä:

        new MobileAppConfiguration()
            .MapApiControllers()
            .ApplyTo(config);

Voit käyttää myös `UseDefaultConfiguration()` tunniste menetelmän asemesta `MapApiControllers()`. Ohjauskoneen, jossa ei ole käytetty **MobileAppControllerAttribute** edelleen voivat käyttää asiakkaat, mutta sen voi ei voi oikein asiakkaita käyttämällä minkä tahansa Mobile-sovelluksen asiakkaan SDK.

## <a name="how-to-work-with-authentication"></a>Toimintaohje: todennus käsitteleminen

Azure Mobile-sovellusten käyttää sovelluksen todentaminen / luvan suojaamiseen mobile Taustajärjestelmä.  Tässä osassa näytetään, miten voit suorittaa seuraavat todennus liittyvät tehtävät .NET Taustajärjestelmä palvelimen projektin:

+ [Toimintaohje: käyttöoikeuksien lisääminen server-projekti](#add-auth)
+ [Toimintaohje: Käytä mukautetun todennusta sovelluksen](#custom-auth)
+ [Toimintaohje: noutaa todennetun käyttäjätiedot](#user-info)
+ [Toimintaohje: valtuutettujen käyttäjien tietojen käytön rajoittaminen](#authorize)

### <a name="add-auth"></a>Toimintaohje: käyttöoikeuksien lisääminen server-projekti

Voit lisätä todennus palvelimen projektiin laajentaminen **MobileAppConfiguration** -objekti ja määrittämällä OWIN middleware. Kun asennat [Microsoft.Azure.Mobile.Server.Quickstart] paketin ja kutsua **UseDefaultConfiguration** tunniste-menetelmää, voit siirtyä vaiheeseen 3.

1. Visual Studiossa Asenna [Microsoft.Azure.Mobile.Server.Authentication] -paketti.

2. Startup.cs project-tiedostoon Lisää seuraava rivi koodin alkuun, **määritys** -menetelmää:

        app.UseAppServiceAuthentication(config);

    Tämä OWIN middleware osa tarkistaa tunnusten myöntämä App palvelun liitetty yhdyskäytävä.

3. Lisää `[Authorize]` määrite ohjaimen tai menetelmä, joka edellyttää käyttöoikeuden tarkistusta. 

Lisätietoja siitä, miten tarkistamiseen mobiilisovellukset Taustajärjestelmä asiakkaiden on artikkelissa [Lisää todennusta sovelluksen](app-service-mobile-ios-get-started-users.md).

### <a name="custom-auth"></a>Toimintaohje: Käytä mukautetun todennusta sovelluksen

Jos et halua käyttää sovelluksen palvelun todennus/todennus-palveluista, voit toteuttaa kirjautuminen-järjestelmän. Asenna [Microsoft.Azure.Mobile.Server.Login] paketti todennus suojaustunnuksen luonti helpottamiseksi.  Anna oman koodin vahvistaminen käyttäjän tunnistetietoja. Voit tarkistaa esimerkiksi vastaan suolattu ja hajautettujen salasanat tietokannassa. Alla olevasta esimerkistä `isValidAssertion()` menetelmää (muualla määritetty) on vastuussa tarkistukset.

Mukautettu tarkistus on määritetty luomalla ApiController ja paljastaa `register` ja `login` toiminnot. Asiakas kerää tietoja käyttäjältä mukautetun Käyttöliittymän avulla.  Tiedot lähetetään sitten ohjelmointirajapinnan vakio HTTP POST puhelun. Kun palvelin vahvistaa vahvistus-tunnus annetaan avulla `AppServiceLoginHandler.CreateToken()` menetelmää.  ApiController **olisi ei** käytössä `[MobileAppController]` määrite. 

Esimerkki `login` toiminto:

        public IHttpActionResult Post([FromBody] JObject assertion)
        {
            if (isValidAssertion(assertion)) // user-defined function, checks against a database
            {
                JwtSecurityToken token = AppServiceLoginHandler.CreateToken(new Claim[] { new Claim(JwtRegisteredClaimNames.Sub, assertion["username"]) },
                    mySigningKey,
                    myAppURL,
                    myAppURL,
                    TimeSpan.FromHours(24) );
                return Ok(new LoginResult()
                {
                    AuthenticationToken = token.RawData,
                    User = new LoginResultUser() { UserId = userName.ToString() }
                });
            }
            else // user assertion was not valid
            {
                return this.Request.CreateUnauthorizedResponse();
            }
        }

Edellisessä esimerkissä LoginResult ja LoginResultUser eivät voi sarjoittaa objekteja paljastaa tarvittavat ominaisuudet. Asiakkaan odottaa kirjautuminen vastaukset palautetaan JSON objekteina lomakkeen:

        {
            "authenticationToken": "<token>",
            "user": {
                "userId": "<userId>"
            }
        }

`AppServiceLoginHandler.CreateToken()` Menetelmä sisältää _yleisön_ ja _myöntäjä_ -parametri. Molemmat parametrit on määritetty URL-osoite sovellus-pääkansio, käyttämällä HTTPS-malli. Määritä vastaavasti _secretKey_ olevan sovelluksen arvo käyttäjän kirjautuminen avain. Jakelu allekirjoitetun avain on jokin muu sähköpostiohjelma kuin sitä voidaan käyttää pääse tekemään näppäimet ja tekeytyminen käyttäjät. Voit hankkia allekirjoitetun näppäintä samalla, kun ylläpidettävä viittaamalla App Service _sivuston\_AUTH\_kirjautuminen\_AVAIMEN_ ympäristömuuttuja. Paikallinen muistin kontekstissa tarvittaessa noutaa ja tallentaa sen sovelluksen-asetus [paikallisen virheenkorjaus todennusta](#local-debug) osan ohjeiden mukaisesti.

Lähetetyn tunnus voi olla myös muut vaateet ja määräajan päivämäärä.  Vähimmäistukitiedot myönnetty tunniste on sisällettävä aihe (**sub**)-ryhmän.

Voit tukea vakio asiakkaan `loginAsync()` menetelmän ylikuormituksen todennus reitin.  Jos asiakas kutsuu `client.loginAsync('custom');` kirjautua sisään reitti on oltava `/.auth/login/custom`.  Voit määrittää reittiä mukautettu tarkistus ohjauskoneen käyttämällä `MapHttpRoute()`:

    config.Routes.MapHttpRoute("custom", ".auth/login/custom", new { controller = "CustomAuth" });

>[AZURE.TIP] Käyttämällä `loginAsync()` lähestymistapa varmistaa todennus-tunnuksen on liitetty jokaisen palvelun myöhemmin kutsu.

###<a name="user-info"></a>Toimintaohje: noutaa todennetun käyttäjätiedot

Kun käyttäjä todennetaan sovellus-palvelu, voit käyttää .NET Taustajärjestelmä koodin määritetty Käyttäjätunnus- ja muut tiedot. Käyttäjätiedot voidaan luvan päätöksentekoa taustaan varten. Seuraava koodi hakee pyyntö liittyvä Käyttäjätunnus:

    // Get the SID of the current user.
    var claimsPrincipal = this.User as ClaimsPrincipal;
    string sid = claimsPrincipal.FindFirst(ClaimTypes.NameIdentifier).Value;

SUOJAUSTUNNUS johdetaan toimittajakohtaiset Käyttäjätunnus ja on staattinen tietyn käyttäjän ja kirjaudu sisään tarjoajan varten.  SUOJAUSTUNNUS on tarkistusvirheen tunnusten null.

Sovelluksen palvelun voit myös pyytää yksittäisiä väitteitä kirjautuminen toimittajalta. Kunkin tunnistetietojen toimittaja voit antaa lisätietoja käyttäjätietojen SDK-palvelun avulla.  Esimerkiksi voit ystävien lisätietoja Facebook-kaavio-Ohjelmointirajapinnan.  Voit määrittää saatavat, jossa pyydetään Azure-portaalissa toimittaja-sivu. Jotkin saatavat tarvitaan lisämäärityksiä tunnistetietojen toimittajaan.

Seuraava koodi kutsuu Saat kirjautumistiedot, jotka sisältävät tarpeen Facebook-kaavio-Ohjelmointirajapinnan pyynnöt käyttöoikeustietue **GetAppServiceIdentityAsync** tunniste-menetelmää:

    // Get the credentials for the logged-in user.
    var credentials =
        await this.User
        .GetAppServiceIdentityAsync<FacebookCredentials>(this.Request);

    if (credentials.Provider == "Facebook")
    {
        // Create a query string with the Facebook access token.
        var fbRequestUrl = "https://graph.facebook.com/me/feed?access_token="
            + credentials.AccessToken;

        // Create an HttpClient request.
        var client = new System.Net.Http.HttpClient();

        // Request the current user info from Facebook.
        var resp = await client.GetAsync(fbRequestUrl);
        resp.EnsureSuccessStatusCode();

        // Do something here with the Facebook user information.
        var fbInfo = await resp.Content.ReadAsStringAsync();
    }

Lisää using tietosuojatiedot `System.Security.Principal` antamaan **GetAppServiceIdentityAsync** tunniste-menetelmää.

### <a name="authorize"></a>Toimintaohje: valtuutettujen käyttäjien tietojen käytön rajoittaminen

Edellisessä osassa on osoittanut noutamisesta todennetun käyttäjän Käyttäjätunnus. Voit rajoittaa tietojen ja muita resursseja tämän arvon perusteella. Käyttäjätunnus-sarakkeen lisääminen taulukkoon ja kyselytulokset mukaan Käyttäjätunnus suodatus on helppo tapa rajoittaa palautettujen tietoja vain valtuutettujen käyttäjien. Seuraava koodi palauttaa tietorivit vasta, kun SUOJAUSTUNNUS vastaa TodoItem taulukon käyttäjätunnus-sarakkeen arvo:

    // Get the SID of the current user.
    var claimsPrincipal = this.User as ClaimsPrincipal;
    string sid = claimsPrincipal.FindFirst(ClaimTypes.NameIdentifier).Value;

    // Only return data rows that belong to the current user.
    return Query().Where(t => t.UserId == sid);

`Query()` Menetelmä palauttaa `IQueryable` , joka voi käsitellä LINQ käsittelemään suodatus.

## <a name="how-to-add-push-notifications-to-a-server-project"></a>Toimintaohje: Lisää push ilmoitukset palvelimen lisääminen projektiin

Lisää push-ilmoitukset palvelimen projektin laajentaminen **MobileAppConfiguration** -objekti ja luomalla ilmoituksen keskittimet asiakas.

1. Visual Studiossa, server project hiiren kakkospainikkeella ja valitse **NuGet pakettien hallinta**, Etsi `Microsoft.Azure.Mobile.Server.Notifications`, valitse **Asenna**. 

2. Näin toistamalla voit asentaa `Microsoft.Azure.NotificationHubs` paketin, joka sisältää ilmoituksen keskittimet asiakas-kirjasto.

3. Valitse App_Start/Startup.MobileApp.cs ja lisää **AddPushNotifications()** tunniste menetelmä puhelun aikana:

        new MobileAppConfiguration()
            // other features...
            .AddPushNotifications()
            .ApplyTo(config);

4. Lisää seuraava koodi, joka luo ilmoituksen keskittimet asiakkaan:

        // Get the settings for the server project.
        HttpConfiguration config = this.Configuration;
        MobileAppSettingsDictionary settings =
            config.GetMobileAppSettingsProvider().GetMobileAppSettings();

        // Get the Notification Hubs credentials for the Mobile App.
        string notificationHubName = settings.NotificationHubName;
        string notificationHubConnection = settings
            .Connections[MobileAppSettingsKeys.NotificationHubConnectionString].ConnectionString;

        // Create a new Notification Hub client.
        NotificationHubClient hub = NotificationHubClient
            .CreateClientFromConnectionString(notificationHubConnection, notificationHubName);

Voit nyt ilmoituksen keskittimet asiakkaan push-ilmoitusten lähettäminen rekisteröity laitteet. Lisätietoja on artikkelissa [Lisää push-ilmoituksia, jotka sovelluksen](app-service-mobile-ios-get-started-push.md). Saat lisätietoja ilmoituksen keskittimet artikkelissa [Ilmoituksen keskittimet yleiskatsaus](../notification-hubs/notification-hubs-push-notification-overview.md).

##<a name="tags"></a>Toimintaohje: Ota käyttöön kohdistettu push-ohjauskoodien käyttäminen

Ilmoitus keskittimet voit kohdennettujen ilmoitukset lähetetään tietyn merkintöjen tunnisteiden avulla. Useita tunnisteita luodaan automaattisesti:

* Asennus-tunnus yksilöi tietyn laitteen.
* Todennettu SUOJAUSTUNNUS perusteella käyttäjätunnus tunnistaa tietyn käyttäjän.

Asennuksen tunnus voi käyttää **MobileServiceClient** **installationId** -ominaisuutta.  Seuraavassa esimerkissä esitetään, miten tunnisteen lisääminen-ilmoituksen keskittimet tietyn asennuksen tunnusta asennuksen avulla:

    hub.PatchInstallation("my-installation-id", new[]
    {
        new PartialUpdateOperation
        {
            Operation = UpdateOperationType.Add,
            Path = "/tags",
            Value = "{my-tag}"
        }
    });

Mahdolliset tunnisteet toimittamaan asiakkaan aikana push-ilmoitusten rekisteröinnissä ohitetaan mukaan taustaan asennuksen luotaessa. Asiakasohjelma tunnisteiden lisääminen asennuksen, sinun on luotava mukautettu API, joka lisää edellisen kaavan tunnisteet. 

Katso [asiakkaan lisätty push ilmoituksen tunnisteet] [ 5] palvelun mobiilisovellukset valmiit pikaopas otoksen esimerkki.

##<a name="push-user"></a>Toimintaohje: todennetun käyttäjän Lähetä push-ilmoitukset

Todennetun käyttäjän Rekisteröi push-ilmoituksia, kun käyttäjä tunnus tunnisteen lisätään automaattisesti rekisteröinnin. Käyttämällä tätä tunnistetta, voit lähettää push-ilmoitukset rekisteröity kyseisen henkilön kaikissa laitteissa. Seuraava koodi saa pyynnön käyttäjän SUOJAUSTUNNUS ja lähettää ilmoituksen mallin push jokaisen laitteen rekisteröinti kyseisen henkilön:

    // Get the current user SID and create a tag for the current user.
    var claimsPrincipal = this.User as ClaimsPrincipal;
    string sid = claimsPrincipal.FindFirst(ClaimTypes.NameIdentifier).Value;
    string userTag = "_UserId:" + sid;

    // Build a dictionary for the template with the item message text.
    var notification = new Dictionary<string, string> { { "message", item.Text } };

    // Send a template notification to the user ID.
    await hub.SendTemplateNotificationAsync(notification, userTag);

Kun rekisteröiminen todennetut asiakaskoneelta push-ilmoitukset, varmista, että tarkistus on valmis ennen kuin yrität rekisteröinti. Lisätietoja on artikkelissa [Push-käyttäjille] [ 6] palvelun mobiilisovellukset valmiit pikaopas otoksen .NET Taustajärjestelmä varten.

## <a name="how-to-debug-and-troubleshoot-the-net-server-sdk"></a>Toimintaohje: virheenkorjaus ja .NET Server SDK vianmääritys

Azure sovelluksen-palvelu sisältää useita virheenkorjaus ja vianmääritys ASP.NET-sovellusten avulla:

- [Seurannan Azure sovelluksen-palvelu](../app-service-web/web-sites-monitor.md)
- [Ota Diagnostiikan kirjaus Azure sovelluksen-palvelussa](../app-service-web/web-sites-enable-diagnostic-log.md)
- [Azure sovelluksen-palvelun Visual Studiossa vianmääritys](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md)

### <a name="logging"></a>Lokiin kirjaaminen

Voit kirjoittaa sovelluksen palvelun vianmäärityslokit käyttämällä vakio ASP.NET-jäljitys kirjoittamista. Ennen kuin voit kirjoittaa lokit, sinun on otettava diagnostiikka Mobile-sovelluksen Taustajärjestelmä.

Voit ottaa käyttöön diagnostiikka ja kirjoittaa lokit:

1. Noudata [ottamisesta käyttöön diagnostiikka](../app-service-web/web-sites-enable-diagnostic-log.md#enablediag).

2. Lisää seuraavat koodin tiedostossa-lauseella:

        using System.Web.Http.Tracing;

3. Luo jäljitys-writer kirjoittamaan .NET taustasta vianmäärityslokit, seuraavasti:

        ITraceWriter traceWriter = this.Configuration.Services.GetTraceWriter();
        traceWriter.Info("Hello, World");

4. Julkaista projektisi palvelimen sekä käyttää Mobile-sovelluksen Taustajärjestelmä suorittamaan lokikirjaus koodin polun.

5. Lataa ja arvioi lokit-ohjeiden mukaisesti [Toimintaohje: lataa lokit](../app-service-web/web-sites-enable-diagnostic-log.md#download).

### <a name="local-debug"></a>Paikallinen virheenkorjaus todennusta

Voit suorittaa sovelluksen paikallisesti, voit esikatsella muutoksia ennen niiden julkaisemista pilveen. Paina *F5* -Visual Studio useimmat Azure Mobile-sovellusten backends. Välillä on kuitenkin joitakin muita huomioon otettavia seikkoja käytettäessä todennusta.

Sinulla on oltava pilvipohjainen mobiilisovelluksessa kanssa App palvelun todennus/luvan määritetty ja asiakkaan on oltava vaihtoehtoinen kirjautuminen host määritettyä cloud päätepiste. Ohjeissa asiakkaan ympäristö varten tarvittavat vaiheet.

Varmista, että kannettavan Taustajärjestelmä [Microsoft.Azure.Mobile.Server.Authentication] asennettuna. Lisää sitten seuraavat sovelluksen OWIN käynnistys luokan jälkeen `MobileAppConfiguration` on käytössä oman `HttpConfiguration`:

        app.UseAppServiceAuthentication(new AppServiceAuthenticationOptions()
        {
            SigningKey = ConfigurationManager.AppSettings["authSigningKey"],
            ValidAudiences = new[] { ConfigurationManager.AppSettings["authAudience"] },
            ValidIssuers = new[] { ConfigurationManager.AppSettings["authIssuer"] },
            TokenHandler = config.GetAppServiceTokenHandler()
        });

Edellisessä esimerkissä kannattaa määrittää _authAudience_ ja _authIssuer_ sovellusasetukset Web.config-tiedostossa kunkin voidaan URL-osoite sovellus-pääkansio, käyttämällä HTTPS-malli. Määritä vastaavasti _authSigningKey_ olevan sovelluksen arvo käyttäjän kirjautuminen avain. Voit hankkia allekirjoitetun avainta:

1. Siirry [Azure portal] -sovellus 
2. Valitse **Työkalut**- **Kudu**, **Siirry**.
3. Napsauta **ympäristön**Kudu hallinta-sivustossa.
4. Etsi arvo _sivuston\_AUTH\_kirjautuminen\_AVAIMEN_. 

Käytä paikallisen sovelluksen config _authSigningKey_ -parametrin allekirjoitetun-näppäintä.  Mobiili Taustajärjestelmä on nyt varustettu Vahvista tunnusten suoritettaessa paikallisesti, jotka asiakas hakee tunnuksen pilvipohjainen päätepisteestä.

[1]: https://msdn.microsoft.com/library/azure/dn961176.aspx
[2]: https://github.com/Azure/azure-mobile-apps-net-server
[3]: app-service-mobile-ios-get-started.md
[4]: https://azure.microsoft.com/downloads/
[5]: https://github.com/Azure-Samples/app-service-mobile-dotnet-backend-quickstart/blob/master/README.md#client-added-push-notification-tags
[6]: https://github.com/Azure-Samples/app-service-mobile-dotnet-backend-quickstart/blob/master/README.md#push-to-users
[Azure portal]: https://portal.azure.com
[NuGet.org]: http://www.nuget.org/
[Microsoft.Azure.Mobile.Server]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/
[Microsoft.Azure.Mobile.Server.Quickstart]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Quickstart/
[Microsoft.Azure.Mobile.Server.Authentication]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Authentication/
[Microsoft.Azure.Mobile.Server.Login]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Login/
[Microsoft.Azure.Mobile.Server.Notifications]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Notifications/
[MapHttpAttributeRoutes]: https://msdn.microsoft.com/library/dn479134(v=vs.118).aspx

