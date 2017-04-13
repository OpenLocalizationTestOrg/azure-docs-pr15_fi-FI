<properties
    pageTitle="Päivityksen Mobile-palvelujen Azure sovelluksen-palveluun"
    description="Opi päivittämään helposti Mobile-palvelut-sovellus App palvelun Mobile-sovellukseen"
    services="app-service\mobile"
    documentationCenter=""
    authors="adrianhall"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="upgrade-your-existing-net-azure-mobile-service-to-app-service"></a>Päivittää oman aiemmin .NET-Azure mobiilipalvelun sovelluksen-palveluun

Sovelluksen palvelun Mobile on uusi tapa käyttämällä Microsoft Azure mobiili-sovelluksia. Lisätietoja on artikkelissa [mitä mobiilisovellukset?].

Tässä artikkelissa käsitellään olemassa olevan .NET Taustajärjestelmä sovelluksen ksi uusi sovellus-palvelun Mobile-sovellusten Azure Mobile-palvelut. Samalla, kun olet suorittanut tämän päivityksen, voit jatkaa aiemmin Mobile-palvelut-sovelluksen toimimaan.   Jos haluat päivittää Node.js Taustajärjestelmä sovelluksen viitata [päivittäminen Node.js Mobile-palvelut](./app-service-mobile-node-backend-upgrading-from-mobile-services.md).

Mobiili taustassa päivityksen Azure sovelluksen-palveluun, on käyttää sovelluksen palvelun kaikkia ominaisuuksia ja mukaan [App palvelun hinnat], ei hinnat Mobile-palveluihin laskutettu.

##<a name="migrate-vs-upgrade"></a>Siirtää ja päivitys

[AZURE.INCLUDE [app-service-mobile-migrate-vs-upgrade](../../includes/app-service-mobile-migrate-vs-upgrade.md)]

>[AZURE.TIP] On suositeltavaa, että [siirto](app-service-mobile-migrating-from-mobile-services.md) ennen päivitystä loppuun. Näin voit sijoittaa sovelluksen molemmat versiot saman sovelluksen-palvelu-suunnittelu ja maksamaan ilman lisäkustannuksia.

###<a name="improvements-in-mobile-apps-net-server-sdk"></a>Mobile-sovellusten .NET server SDK-parannukset

Uusi [Mobile-sovellusten SDK](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/) päivittämisestä on seuraavat edut:

- Lisää joustavuutta NuGet riippuvuuksista. Isännöintipalvelu ympäristön sisältää enää omassa NuGet pakettien versiot, jotta voit käyttää vaihtoehtoinen yhteensopivista versioista. Jos luettelossa on uusi kriittinen bugfixes tai turvapäivitykset, jotka Mobile Server SDK tai riippuvuudet, on päivitettävä palvelun manuaalisesti.

- Mobiili SDK-paketissa useampia sivuston. Voit määrittää erikseen, mitä ominaisuuksia ja tiet on määritetty oikein, kuten todennusta, taulukon ohjelmointirajapinnan ja push rekisteröinti päätepiste. Lisätietoja on artikkelissa [.NET-palvelimen SDK Azure Mobile-sovellusten käyttämisestä](app-service-mobile-net-upgrading-from-mobile-services.md#server-project-setup).

- Muut ASP.NET projektityypit ja tiet tuki. Voit nyt ylläpitää MVC ja verkko-Ohjelmointirajapinnan ohjaimet mobile Taustajärjestelmä projektin samassa projektissa.

- Uuden sovelluksen palvelun todennus ominaisuuksista, joiden avulla voit käyttää yleisiä todennuksen määrittäminen web ja mobiilisovellukset tuki.

##<a name="overview"></a>Perustiedot Päivitä yleiskatsaus

Monissa tapauksissa päivittäminen on helposti vaihtaminen uuteen mobiilisovellukset palvelimeen SDK ja uudelleen aina uutta Mobile-sovelluksen esiintymää koodi. On-kuitenkin joitakin tilanteita, jotka edellyttävät joitakin lisämäärityksiä, kuten todennuksen lisäasetukset skenaariot ja käsitteleminen ajoitetut työt. Kaikkien näiden käsitellään myöhemmin osat.

>[AZURE.TIP] On suositeltavaa, että lukeminen ja ymmärtäminen tämän ohjeaiheen loppuun kokonaan ennen kuin aloitat päivityksen. Kirjoita muistiin ominaisuuksia käytät joka ovat korostettuina alapuolella.

Mobile Services client SDK: T ovat **ei ole** yhteensopiva uuden mobiilisovellukset palvelimen SDK kanssa. Anna palvelun, kun sovellus jatkuvuuden, jotta voit tarjota tällä hetkellä julkaistun asiakkaiden sivuston olisi ei julkaista muutokset. Sen sijaan sinun on luotava uusi mobiilisovelluksen, joka on jo olemassa. Voit sijoittaa tämän sovelluksen saman sovelluksen palvelusopimus välttämiseksi sille taloudellisen tilanteen lisäkustannuksia.

Sen jälkeen sovellus kaksi versiota: yksi, joka pysyy muuttumattomana ja on julkaistu sovellusten luonnosta, ja toisen, jossa voit päivittää ja kohde uusi asiakas-versiossa. Voit siirtää ja Testaa yhteyttä tahdissa koodi, mutta varmista, että kaikki virheenkorjauksia teet Hae sovelletaan molempia. Kun huomaat, että haluamasi määrä luonnosta sovellukset on päivitetty uusimpaan versioon asiakas, voit poistaa alkuperäisen siirretyt sovelluksen kysymykset.

Koko jäsennyksen päivitysprosessin on seuraavanlainen:

1. Uuden Mobile-sovelluksen luominen
2. Päivitä projekti käyttämään uuden palvelimen SDK: T
3. Asiakassovellus uusi versio
4. (Valinnainen) Oman alkuperäisen siirretyt poisto

##<a name="mobile-app-version"></a>Luodaan toinen sovellusesiintymä
Päivittäminen ensimmäiseksi on luotava Mobile-sovelluksen resurssin, joka isännöi sovelluksen uuden version. Jos olet jo siirtänyt aiemmin mobiilipalvelu, haluat luoda tämän version samaan isännöintipalvelu osalta. Avaa [Azure-portaaliin] ja siirry siirretyt sovelluksen. Kirjoita muistiin sovelluksen-palvelu-palvelupaketti, se on käytössä.

Seuraavaksi luodaan toisen sovelluksen esiintymän [.NET Taustajärjestelmä luomisen ohjeita](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#create-app)noudattamalla. Kun näyttöön tulee kehote valita voit App palvelusopimus tai "isännöinnin suunnitelma" Valitse siirretyt sovelluksen palvelupaketti.

Todennäköisesti haluat käyttää saman tietokannan ja ilmoituksen keskittimeen samoin kuin Mobile-palvelut. Voit kopioida nämä arvot avaamalla [Azure portal] ja siirtyminen alkuperäinen-sovellukseen ja valitse sitten **asetukset** > **asetukset**. Valitse **Yhteyden merkkijonoja**, kopioi `MS_NotificationHubConnectionString` ja `MS_TableConnectionString`. Siirry uuden päivityksen sivustoosi ja liitä ne valitse Korvaa kaikki olemassa olevat arvot. Toista nämä vaiheet muissa sovellusasetukset app tarpeitasi. Käytä siirretyt palvelu, lue yhteyden merkkijonot ja sovelluksen asetusten **määrittäminen** -välilehden [Azure perinteinen portal]Mobile-palvelut-osan.

Sovelluksen ASP.NET-projektin kopioiminen ja julkaista sen uuteen sivustoon. Käytä asiakassovellus uusi URL-osoite päivitetty kopio, Vahvista, että kaikki toimii odotetulla tavalla.

## <a name="updating-the-server-project"></a>Päivittäminen server project

Mobile-sovellukset on uusi [Mobile-sovelluksen Server SDK] joka tarjoaa paljon samalla tavalla kuin runtime Mobile-palvelut. Poista ensin kaikki viittaukset Mobile-palvelut-paketit. NuGet paketin hallintaan ja etsiä `WindowsAzure.MobileServices.Backend`. Useimpien sovellusten tulee näkyviin useita pakettien tähän, kuten `WindowsAzure.MobileServices.Backend.Tables` ja `WindowsAzure.MobileServices.Backend.Entity`. Tässä tapauksessa aloitetaan pienin paketin riippuvuuden kohtaa, kuten `Entity`, ja poista se. Kun sinulta kysytään, ei poista kaikki riippuvaiset paketit. Toista nämä vaiheet, kunnes olet poistanut `WindowsAzure.MobileServices.Backend` itse.

Tässä vaiheessa sinun on projekti, joka viittaa enää Mobile Services SDK: T.

Lisää seuraavaksi viittaukset Mobile-sovellusten SDK. Tähän päivitykseen useimmat kehittäjät kannattaa voit ladata ja asentaa `Microsoft.Azure.Mobile.Server.Quickstart` pakata, kun tämä otetaan koko esitetty.

Ole aivan vähän kääntäjän virheet johtuvat erot SDK: T, mutta ne ovat helposti osoitteen ja muiden tässä osassa käsitellään.

### <a name="base-configuration"></a>Peruskokoonpano

Valitse WebApiConfig.cs, voit korvata:

        // Use this class to set configuration options for your mobile service
        ConfigOptions options = new ConfigOptions();

        // Use this class to set WebAPI configuration options
        HttpConfiguration config = ServiceConfig.Initialize(new ConfigBuilder(options));

kanssa

        HttpConfiguration config = new HttpConfiguration();
        new MobileAppConfiguration()
            .UseDefaultConfiguration()
        .ApplyTo(config);

>[AZURE.NOTE] Jos haluat lisätietoja uuden .NET-palvelimen SDK ja miten Lisää tai poista ominaisuuksia-sovellukset, katso [käyttämisestä .NET-palvelimen SDK] -aihetta.

Jos sovellus on todennus-ominaisuuksien avulla, sinun on myös rekisteröidä OWIN middleware. Tässä tapauksessa edellä määrityskoodi on siirtäminen uusi OWIN käynnistys-luokka.

1. Lisää NuGet paketti `Microsoft.Owin.Host.SystemWeb` jos se ei ole jo projektin.
2. Visual Studiossa, napsauta projektin hiiren kakkospainikkeella ja valitse **Lisää** -> **Uusi kohde**. Valitse **Web** -> **Yleiset** -> **OWIN käynnistys-luokka**.
3. Siirtää yläpuolella koodi MobileAppConfiguration `WebApiConfig.Register()` , `Configuration()` uusi käynnistys-luokka-menetelmää.

Varmista, `Configuration()` menetelmä päättyy:

        app.UseWebApi(config)
        app.UseAppServiceAuthentication(config);

On muita todentamiseen liittyvien muutoksia, jotka kuuluvat koko todennus-osassa.

### <a name="working-with-data"></a>Tietojen käsitteleminen

Matkapuhelin-palveluiden mobiilisovelluksen nimi served rakenteen oletusnimen kohteen Framework asetuksissa.

Varmista, että sinulla on viitattu kuin ennen, käytä seuraavat rakenteen määrittämisestä sovelluksen DbContext samaan rakenteeseen:

        string schema = System.Configuration.ConfigurationManager.AppSettings.Get("MS_MobileServiceName");

Varmista, että sinulla on MS_MobileServiceName, jos teet edellä mainituista. Voit myös lisätä toisen mallin nimi, jos sovelluksesi mukautettu tämä aiemmin.

### <a name="system-properties"></a>Järjestelmän ominaisuudet

#### <a name="naming"></a>Nimeäminen

Azure Mobile Services-palvelimen SDK-järjestelmän ominaisuudet sisältävät aina kaksinkertainen alaviivaa (`__`) etuliite ominaisuudet:

- __createdAt
- __updatedAt
- __deleted
- __version

Mobile Services client SDK: T on erityisen logiikan jäsentämiseen järjestelmän ominaisuudet tässä muodossa.

Azure-mobiilisovellukset järjestelmän ominaisuudet eivät enää ole Erikoismuotoilu ja seuraavat nimet on:

- createdAt
- updatedAt
- poistaa
- versio

Mobile-sovellusten asiakas SDK: T käyttää uusia ominaisuuksia nimet, jotta muutoksia ei tarvita asiakas-koodiin. Jos teet suoraan muiden puhelut palvelun sitten muutettava kyselyissä vastaavasti.

#### <a name="local-store"></a>Paikallinen säilö

Järjestelmän ominaisuuksien nimet muutokset tarkoittaa, että offline-synkronoinnin Mobile-palveluihin paikallinen tietokanta ei ole yhteensopiva Mobile-sovellusten kanssa. Jos mahdollista Vältä päivittää asiakasohjelman sovelluksia Mobile-palveluihin mobiilisovelluksiin sen jälkeen odottavia muutoksia on lähetetty palvelimeen. Tämän jälkeen päivitetty sovellus tulee käyttää uuden tietokannan tiedostonimi.

Jos asiakas-sovellus päivitetään Mobile Servicesistä mobiilisovelluksiin Internetissä on odottavia muutoksia offline-tilassa toiminnon jonossa, Valitse järjestelmätietokanta on päivitettävä käyttämään uusien sarakkeiden nimet. IOS-tämä onnistuu kevyt siirrot avulla voit muuttaa sarakkeiden nimet. Android-ja .NET hallitun asiakas Kirjoita mukautetun SQL objektin arvotaulukot sarakkeiden nimeäminen uudelleen.

IOS sinun on muutettava Core tiedot rakenteen tiedot-kohteiden vastaamaan seuraavasti. Huomaa, että ominaisuudet `createdAt`, `updatedAt` ja `version` ei enää ole `ms_` etuliite:

| Määrite |  Tyyppi   | Huomautus                                                 |
|---------- |  ------ | -----------------------------------------------------|
| tunnus        | Merkkijono, joka on merkitty pakolliseksi  | perusavaimen remote säilöön         |
| createdAt | Päivämäärä    | (valinnainen) vastaa createdAt järjestelmäominaisuus         |
| updatedAt | Päivämäärä    | (valinnainen) vastaa updatedAt järjestelmäominaisuus         |
| versio   | Merkkijono  | (valinnainen) käytetään esiintyvien ristiriidat-versioon kartat |

#### <a name="querying-system-properties"></a>Kyselyt järjestelmän ominaisuudet

Azure Mobile-palveluja, järjestelmä ei lähetetä oletusarvoisesti, mutta vain silloin, kun pyydetty käyttämällä kyselymerkkijonon `__systemProperties`. Sen sijaan Azure-mobiilisovellukset järjestelmän ominaisuudet ovat **aina valittuna** , koska ne ovat osa server SDK-objektimallia.

Tämä muutos vaikuttaa pääasiassa mukautetun toimialueen johtajien, kuten tunnisteet käyttöotot `MappedEntityDomainManager`. Matkapuhelin-palveluja, jos asiakastietokone pyytää koskaan järjestelmä-ominaisuuksia on mahdollista käyttämään `MappedEntityDomainManager` , joka ei yhdistetä todella kaikki ominaisuudet. Kuitenkin Azure Mobile-sovelluksissa yhdistämättömät ominaisuuksien aiheuttaa virheen GET-kyselyiden.

Voit ratkaista ongelman helpoin tapa on muokattava oman DTOs niin, että ne perivät `ITableData` sijaan `EntityData`. Lisää sitten `[NotMapped]` määrite kentät, jotka kannattaa jättää pois.

Esimerkiksi seuraavat määrittää `TodoItem` ei ole järjestelmän ominaisuudet:

    using System.ComponentModel.DataAnnotations.Schema;

    public class TodoItem : ITableData
    {
        public string Text { get; set; }

        public bool Complete { get; set; }

        public string Id { get; set; }

        [NotMapped]
        public DateTimeOffset? CreatedAt { get; set; }

        [NotMapped]
        public DateTimeOffset? UpdatedAt { get; set; }

        [NotMapped]
        public bool Deleted { get; set; }

        [NotMapped]
        public byte[] Version { get; set; }
    }

Huomautus: Jos virheet `NotMapped`, Lisää viittaus kokoonpanon `System.ComponentModel.DataAnnotations`.

### <a name="cors"></a>CORS

Mobile-palveluihin lisätään joitakin tuki CORS rivitys ASP.NET CORS-ratkaisun. Tämä rivitystä kerros on poistettu antaa kehittäjä tarkemmin, jotta [ASP.NET CORS tuki](http://www.asp.net/web-api/overview/security/enabling-cross-origin-requests-in-web-api)voidaan hyödyntää suoraan.

Huolta CORS käytettäessä pääaluetta on se, että `eTag` ja `Location` otsikot on sallittava tilauksen SDK: T-asiakasohjelman toimii oikein.

### <a name="push-notifications"></a>Push-ilmoitukset
Push-tärkeimmät kohde, joka saattaa olla puuttuu palvelimen SDK: n on PushRegistrationHandler-luokka. Merkintöjen käsitellään hieman eri tavalla Mobile-sovellusten ja tagless merkintöjen ovat oletusarvoisesti käytössä. Hallinta tunnisteiden voidaan toteuttaa käyttämällä mukautettuja API. Katso lisätietoja [rekisteröiminen tunnisteiden](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#tags) ohjeita.

### <a name="scheduled-jobs"></a>Ajoitetuissa
Ajoitetuissa ei valmiina Mobile-sovellusten, jotta olemassa olevat projektit, .NET-taustatietokannan sisältävät on päivitettävä erikseen. Yksi vaihtoehto on luoda ajoitetun [Työn Web] Mobile-sovelluksen koodi-sivustoissa. Voi myös ohjauskoneen, joka sisältää työn koodin määrittäminen ja määritä [Azure ajoituksen] osumien odotettu aikataulussa, päätepiste.

### <a name="miscellaneous-changes"></a>Muut muutokset
Kaikki ApiControllers, jossa käytetty mukaan mobiilisovellukseen on oltava nyt `[MobileAppController]` määrite. Tämä ei ole enää oletuksena niin, että muut ApiControllers Siirry vaikuta mobile formatters.

`ApiServices` Objekti ei ole enää SDK. Voit käyttää Mobile-sovelluksen asetukset, voit käyttää seuraavasti:

    MobileAppSettingsDictionary settings = this.Configuration.GetMobileAppSettingsProvider().GetMobileAppSettings();

Vastaavasti kirjaaminen tehdään nyt käyttämällä vakio ASP.NET jäljitys kirjoittaminen:

    ITraceWriter traceWriter = this.Configuration.Services.GetTraceWriter();
    traceWriter.Info("Hello, World");  

##<a name="authentication"></a>Todennus huomioon otettavia seikkoja

Käyttöoikeuksien osat Mobile-palveluihin siirretty kyselyjä App palvelun todennus/todennus-ominaisuus. Voit opetella käyttöönottoon tämän sivuston pääkohtiin lukemalla artikkeli [Lisää todennusta mobile-sovellus](app-service-mobile-ios-get-started-users.md) .

Joillakin palveluntarjoajilla, kuten AAD Facebook tai Google sinun pitäisi hyödyntää rekisteröinnin kopio-sovelluksesta. Tarvitset vain siirtyminen tunnistetietojen toimittaja-portaaliin ja Lisää uusi Uudelleenohjaus URL-osoite rekisteröinnin. Määritä sitten sovelluksen palvelun todennus/luvan Asiakastunnus ja salaisuus.

### <a name="controller-action-authorization"></a>Ohjaimen toiminto-todennus
Kaikki esiintymät `[AuthorizeLevel(AuthorizationLevel.User)]` määrite on nyt muutettava käyttämään vakio ASP.NET `[Authorize]` määrite. Lisäksi ohjaimet ovat nyt anonyymi oletusarvoisesti, kuten ASP.NET-sovelluksia.
Jos käytössäsi on jokin muu AuthorizeLevel vaihtoehto, kuten järjestelmänvalvojan tai sovelluksen, Huomaa, että ne on poistettu. Voit sen sijaan AuthorizationFilters määritetään jaettuja tietoja tai määrittää AAD Service-lyhennys käyttöön palvelun puhelut suojatusti.

### <a name="getting-additional-user-information"></a>Lisätietoja käyttäjän tietojen

Saat lisää käyttäjätiedot, kuten access tunnusten kautta `GetAppServiceIdentityAsync()` menetelmää:

        FacebookCredentials creds = await this.User.GetAppServiceIdentityAsync<FacebookCredentials>();

Lisäksi sovelluksen kestää riippuvuudet käyttäjätunnukset, kuten tallennetaan tietokannan, jos se on tärkeää Huomaa, että käyttäjätunnuksiin Mobile-palveluihin ja palvelun mobiilisovellukset ovat eri. Mobile Services Käyttäjätunnus, voivat käyttää nykyistä kautta. Kaikki ProviderCredentials aliluokkien on käyttäjätunnus-ominaisuus. Jatkamista niin ennen esimerkissä:

        string mobileServicesUserId = creds.Provider + ":" + creds.UserId;

Jos sovellus käyttää riippuvuuksia käyttäjätunnukset, on tärkeää, että voit hyödyntää saman rekisteröinnin Liittoutuminen Jos mahdollista. Käyttäjätunnukset on rajattu yleensä sovelluksen rekisteröinnin, jota käytettiin, jotta uusi rekisteröinti esittely voi luoda vastaavat käyttäjien tietonsa liittyviä ongelmia.

### <a name="custom-authentication"></a>Mukautettu tarkistus

Jos sovellus on mukautettu tarkistus-ratkaisun, haluat Varmista, että päivitetyn sivustoni on pääsyn. Ohjeiden uuden mukautetun todennustavaksi [.NET server SDK yleiskatsaus] integroida ratkaisu. Huomaa, että mukautettu tarkistus-osat ovat edelleen esikatselu.

##<a name="updating-clients"></a>Asiakasohjelmien
Kun olet toiminnallisia Mobile-sovelluksen-taustatietokannan, voit käsitellä uudella versiolla, joka käyttää sitä asiakassovellus. Asiakkaan SDK: T uusi versio sisältää myös Mobile-sovellusten ja samoin kuin server-päivitystä, poista kaikki viittaukset Mobile Services SDK: T ennen asennusta Mobile-sovellusten versiot tarvitset.

Tärkeimmät muutokset versioiden välillä on konstruktoreja eivät enää tarvitse sovellusnäppäimen. Voit nyt vain välittää Mobile-sovelluksen URL-osoite. Esimerkiksi .NET-asiakkaat- `MobileServiceClient` konstruktori on nyt:

        public static MobileServiceClient MobileService = new MobileServiceClient(
            "https://contoso.azurewebsites.net", // URL of the Mobile App
        );

Voit lukea asennuksesta uusi SDK: T ja uusi rakennetta alla olevia linkkejä kautta:

- [iOS versio 3.0.0 tai uudempi versio](app-service-mobile-ios-how-to-use-client-library.md)
- [.NET (Windows/Xamarin) versio 2.0.0 tai uudempi versio](app-service-mobile-dotnet-how-to-use-client-library.md)

Jos sovellus on käyttää push-ilmoitukset, muistiin eri ympäristöissä rekisteröinnin ohjeita, niin on joitakin muutoksia myös.

Kun olet valmis uuden asiakasohjelmaversion, kokeile päivitetyn palvelimen projektin vastaan. Voit vapauttaa asiakkaille sovelluksen uuden version tarkistettuaan, että se toimii. Myöhemmin, kun asiakkaille, sinulla on ollut mahdollisuus saada nämä päivitykset, voit poistaa sovelluksen Mobile-palvelut-versiossa. Tässä vaiheessa päivitit kokonaan Mobile-sovellusten uusimmat palvelinta SDK App palvelun Mobile ohjelmaan.

<!-- URLs. -->

[Azure portal]: https://portal.azure.com/
[Azure perinteinen portal]: https://manage.windowsazure.com/
[Mitkä ovat mobiilisovellukset?]: app-service-mobile-value-prop.md
[I already use web sites and mobile services – how does App Service help me?]: /en-us/documentation/articles/app-service-mobile-value-prop-migration-from-mobile-services
[Mobiili App-palvelin SDK-paketissa]: http://www.nuget.org/packages/microsoft.azure.mobile.server
[Create a Mobile App]: app-service-mobile-xamarin-ios-get-started.md
[Add push notifications to your mobile app]: app-service-mobile-xamarin-ios-get-started-push.md
[Add authentication to your mobile app]: app-service-mobile-xamarin-ios-get-started-users.md
[Azure ajoitus]: /en-us/documentation/services/scheduler/
[Web-työ]: ../app-service-web/websites-webjobs-resources.md
[.NET-palvelimen SDK käyttäminen]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Migrate from Mobile Services to an App Service Mobile App]: app-service-mobile-migrating-from-mobile-services.md
[Migrate your existing Mobile Service to App Service]: app-service-mobile-migrating-from-mobile-services.md
[Sovelluksen palvelun hinnat]: https://azure.microsoft.com/en-us/pricing/details/app-service/
[.NET server SDK yleiskatsaus]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
