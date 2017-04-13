<properties
    pageTitle="GEO läheisyydessä push-ilmoitukset Azure ilmoituksen keskittimet ja Bing paikkatietojen tietoja | Microsoft Azure"
    description="Tässä opetusohjelmassa kerrotaan ohjeet sijaintiin perustuva push-ilmoitukset Azure ilmoituksen keskittimet ja Bing paikkatietojen tiedoilla."
    services="notification-hubs"
    documentationCenter="windows"
    keywords="Push-ilmoitus, push-ilmoitus"
    authors="dend"
    manager="yuaxu"
    editor="dend"/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows-phone"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="05/31/2016"
    ms.author="dendeli"/>
    
# <a name="geo-fenced-push-notifications-with-azure-notification-hubs-and-bing-spatial-data"></a>Azure ilmoituksen keskittimet ja Bing paikkatietojen tietoja GEO läheisyydessä push-ilmoitukset
 
 > [AZURE.NOTE] Jotta voit suorittaa tässä opetusohjelmassa, on oltava aktiivinen Azure-tili. Jos sinulla ei ole tiliä, voit luoda ilmainen kokeiluversio tili vain muutaman minuutin. Lisätietoja on artikkelissa [Azure maksuttoman kokeiluversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02).

Tässä opetusohjelmassa kerrotaan ohjeet sijaintiin perustuva push-ilmoitukset Azure ilmoituksen keskittimet ja Bing paikkatietojen tietoja, hyödyntää yleinen Windows-ympäristön-sovelluksesta.

##<a name="prerequisites"></a>Edellytykset
Keskustelupalstoja tarvitset Varmista, että sinulla on kaikki ohjelmistojen ja palvelun vaatimat:

* [Visual Studio 2015 päivitys 1](https://www.visualstudio.com/en-us/downloads/download-visual-studio-vs.aspx) tai uudempi ([Yhteisön Edition](https://go.microsoft.com/fwlink/?LinkId=691978&clcid=0x409) tällöin toimi sekä). 
* [Azure SDK](https://azure.microsoft.com/downloads/)uusimman version. 
* [Bing Maps keskihajonta Center-tili](https://www.bingmapsportal.com/) (Voit luoda sen maksutta ja liittäminen Microsoft-tili). 

##<a name="getting-started"></a>Käytön aloittaminen

Aloitetaan luomalla projektin. Visual Studiossa Käynnistä uuden projektin tyyppi **Tyhjästä sovelluksesta (Universal Windows)**.

![](./media/notification-hubs-geofence/notification-hubs-create-blank-app.png)

Projektin luominen on valmis, kun sinulla on oltava sovelluksessa johdinsarjan. Nyt kaikki geo aitaus infrastruktuurin määritetään. Koska tarkastellaan käyttää tätä Bing-palveluja, on julkinen REST API-päätepiste, joka pystyy kysely tiettyyn kohtaan kehykset:

    http://spatial.virtualearth.net/REST/v1/data/
    
Sinun on määritettävä seuraavat parametrit ennen kuin se toimii:

* **Tietolähteen tunnus** ja **Tietolähteen nimi** – Bing Maps-Ohjelmointirajapinnan tietolähteet sisältävät eri bucketed metatietoja, kuten sijainnit ja yrityksen aukioloajat. Voit lukea lisää tietoja tähän. 
* **Kohteen nimi** – kohde, jota haluat käyttää lähtökohtana ilmoituksen. 
* **Bing Maps-Ohjelmointirajapinnan avain** – avain, jotka olet hankkinut aiemmin luodessasi Bing keskihajonta Center-tiliisi.
 
Tee ohjaamme asetusten oletetaan, että kunkin yllä olevista osista.

##<a name="setting-up-the-data-source"></a>Tietolähteen määrittäminen

Voit tehdä sen Bing Maps keskihajonta keskelle. Napsauta **tietolähteiden** yläreunan siirtymispalkissa ja valitsemalla **Hallitse tietolähteet**.

![](./media/notification-hubs-geofence/bing-maps-manage-data.png)

Jos et ole käyttänyt Bing Maps-Ohjelmointirajapinnan ennen, todennäköisesti siellä ei ole kaikki tietolähteet on käytössä, jotta voit luoda uuden valitsemalla Lataa tiedot tietolähteeseen. Varmista, että kaikki tarvittavat kentät täytetään:

![](./media/notification-hubs-geofence/bing-maps-create-data.png)

Saatat miettiä – tiedoston ominaisuudet ja mitä tulee voit olla lataamisen? Tämän testin tarkoituksiin Microsoft käyttää vain putkien perustuva otosten, kehysten alue, San Franciscon waterfront:

    Bing Spatial Data Services, 1.0, TestBoundaries
    EntityID(Edm.String,primaryKey)|Name(Edm.String)|Longitude(Edm.Double)|Latitude(Edm.Double)|Boundary(Edm.Geography)
    1|SanFranciscoPier|||POLYGON ((-122.389825 37.776598,-122.389438 37.773087,-122.381885 37.771849,-122.382186 37.777022,-122.389825 37.776598))
    
Edellä edustaa tämän kohteen:

![](./media/notification-hubs-geofence/bing-maps-geofence.png)

Riittää, että kopioida ja liittää yllä olevan merkkijonon uuteen tiedostoon ja tallentaminen **NotificationHubsGeofence.pipe**, ja lataa Bing keskihajonta keskelle.

>[AZURE.NOTE]Sinulta saatetaan pyytää määrittämään uusi avain **avaimen** , joka ei ole sama kuin **Kyselyn avain**. Ainoastaan luoda uuden tunnuksen – koontinäyttö ja Päivitä tietolähteen Lataa sivulla.

Kun olet ladannut datatiedosto, sinun on varmistaaksesi, että julkaiset tietolähteen. 

Siirry **Hallita tietolähteitä**, aivan kuten on ollut yli, Etsi tietolähde luettelosta ja valitse **Julkaise** **Toiminnot** -sarakkeessa. Kohdassa hieman, näkyy tietolähteen **Julkaistu tietolähteet** -välilehti:

![](./media/notification-hubs-geofence/bing-maps-published-data.png)

Jos valitset **Muokkaa**, osaat näet yhdellä silmäyksellä tietoja sijainneista on otettu käyttöön sen:

![](./media/notification-hubs-geofence/bing-maps-data-details.png)

Tässä vaiheessa portaalin ei välttämättä näy sinua, että luomaasi – annettava on määritetty sijainti on oikea lähellä vahvistus geofence reunat.

Nyt käytössäsi on tietolähteen vaatimukset. Saat tietoja pyyntö URL-osoitteen API, puhelun Bing Maps keskihajonta keskelle- **tietolähteet** ja valitse **Tietolähteen tiedot**.

![](./media/notification-hubs-geofence/bing-maps-data-info.png)

**Kyselyn URL-osoite** on emme ole tähän jälkeen. Tämä on päätepiste vastaan, joka on suorittaa kyselyitä, voit tarkistaa, onko laite on tällä hetkellä sijainti rajojen vai ei. Jos haluat suorittaa tämän valintaruudun, riittää, että annettava suorittaa GET puhelun vastaan kyselyn URL-osoite, joka on lisätty seuraavat parametreilla:

    ?spatialFilter=intersects(%27POINT%20LONGITUDE%20LATITUDE)%27)&$format=json&key=QUERY_KEY

Näin voit alkamispäivämäärän kohde osoittamalla, joka on hankkia laitteesta ja Bing Maps automaattisesti suorittaa laskutoimituksia onko geofence sisällä. Kun suoritat selaimessa (tai kääntö) – pyynnön, saat vakio JSON-vastaus:

![](./media/notification-hubs-geofence/bing-maps-json.png)

Tämä vastaus tapahtuu vain, kun piste on todella nimettyjen rajojen. Jos se ei ole, näkyviin tulee tyhjä **tulokset** aikajakson:

![](./media/notification-hubs-geofence/bing-maps-nores.png)

##<a name="setting-up-the-uwp-application"></a>UWP-sovelluksen määrittäminen

Nyt kun on valmis tietolähteen, emme aloittaa tiedostojesi käytön UWP sovellus, jossa on bootstrapped aiemmin.

Keskustelupalstoja emme on otettava sijainti palveluita tämän sovelluksen. Voit tehdä tämän kaksoisnapsauttamalla `Package.appxmanifest` tiedosto, napsauta **Ratkaisunhallinnassa**.

![](./media/notification-hubs-geofence/vs-package-manifest.png)

Paketin ominaisuudet-välilehti, joka avataan vain, valitse **Ominaisuudet** ja varmista, että olet valinnut **sijainnin**:

![](./media/notification-hubs-geofence/vs-package-location.png)

Sijainti-ominaisuuksien ilmoitettu Luo uusi kansio nimeltä ratkaisu `Core`, ja lisätä sen nimi uuden tiedoston `LocationHelper.cs`:

![](./media/notification-hubs-geofence/vs-location-helper.png)

`LocationHelper` Itse luokka on melko basic tässä vaiheessa – se on, jotta hankkia järjestelmän API kautta käyttäjän sijainti:

    using System;
    using System.Threading.Tasks;
    using Windows.Devices.Geolocation;

    namespace NotificationHubs.Geofence.Core
    {
        public class LocationHelper
        {
            private static readonly uint AppDesiredAccuracyInMeters = 10;

            public async static Task<Geoposition> GetCurrentLocation()
            {
                var accessStatus = await Geolocator.RequestAccessAsync();
                switch (accessStatus)
                {
                    case GeolocationAccessStatus.Allowed:
                        {
                            Geolocator geolocator = new Geolocator { DesiredAccuracyInMeters = AppDesiredAccuracyInMeters };

                            return await geolocator.GetGeopositionAsync();
                        }
                    default:
                        {
                            return null;
                        }
                }
            }

        }
    }

Voit lukea lisää hakeminen käyttäjän UWP sovelluksissa virallinen [MSDN-tiedoston](https://msdn.microsoft.com/library/windows/apps/mt219698.aspx)sijainti.

Voit tarkistaa, että sijainti hankintaa toimii oikein, Avaa koodin tärkeimmät sivun reunaan (`MainPage.xaml.cs`). Luo uusi tapahtumakäsittelijä `Loaded` tapahtuman `MainPage` konstruktori:

    public MainPage()
    {
        this.InitializeComponent();
        this.Loaded += MainPage_Loaded;
    }

Tapahtumakäsittelijä soveltaminen on seuraavanlainen:

    private async void MainPage_Loaded(object sender, RoutedEventArgs e)
    {
        var location = await LocationHelper.GetCurrentLocation();

        if (location != null)
        {
            Debug.WriteLine(string.Concat(location.Coordinate.Longitude,
                " ", location.Coordinate.Latitude));
        }
    }

Huomaa, että olemme määrätä käsittelijä asynkroninen koska `GetCurrentLocation` on awaitable ja näin ollen vaatii, joka suoritetaan asynkroninen kontekstissa. Lisäksi tietyissä tilanteissa on ehkä lopputulos null sijainti (esimerkiksi palvelut eivät ole käytettävissä sijainti tai sovellus on estetty käyttöoikeudet access sijaintiin), koska annettava Varmista, että se käsitellään oikein null-valintaruutu.

Suorita sovellus. Varmista, että sijainti käyttö:

![](./media/notification-hubs-geofence/notification-hubs-location-access.png)

Kun sovellus käynnistyy, pitäisi onnistua Nähdäksesi koordinaatteja **kohde** -ikkunassa:

![](./media/notification-hubs-geofence/notification-hubs-location-output.png)

Nyt tiedät, että sijainti hankintaa works – vapaasti poistamalla testi tapahtumakäsittelijän ladattu, koska Microsoft ei käytä sitä enää.

Seuraavaksi kerää sijainti muutokset. Mennään takaisin kohdalla, `LocationHelper` luokka ja lisätä tapahtumakäsittely `PositionChanged`:

    geolocator.PositionChanged += Geolocator_PositionChanged;

Käyttöönoton näytetään sijainnin koordinaatit **kohde** -ikkunassa:

    private static async void Geolocator_PositionChanged(Geolocator sender, PositionChangedEventArgs args)
    {
        await CoreApplication.MainView.CoreWindow.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
        {
            Debug.WriteLine(string.Concat(args.Position.Coordinate.Longitude, " ", args.Position.Coordinate.Latitude));
        });
    }

##<a name="setting-up-the-backend"></a>Taustaan määrittäminen

Lataa [.NET Taustajärjestelmä otoksen GitHub](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/NotifyUsers). Kun lataus on valmis, Avaa `NotifyUsers` kansio ja sen jälkeen – `NotifyUsers.sln` tiedosto.

Määritä `AppBackend` **Käynnistys projektin** project ja käynnistä se.

![](./media/notification-hubs-geofence/vs-startup-project.png)

Projekti on jo määritetty push-ilmoitusten lähettäminen laitteiden, niin että on tehtävä kaksi asiaa – yhteystietokuvan oikean yhteyden merkkijono, ilmoitus-toiminnossa ja lisää rajan tunnus lähettää ilmoituksen vain silloin, kun käyttäjä ei sisällä geofence.

Yhteysmerkkijonossa paikallaan `Models` Avaa kansion `Notifications.cs`. `NotificationHubClient.CreateClientFromConnectionString` Funktion pitäisi sisältää tietoja oman ilmoitus-toiminto, jonka saat [Azure-portaali](https://portal.azure.com) (Katso sisällä **asetukset** **Käytäntöjen** -sivu). Tallenna päivitetty määritystiedostoa.

Nyt annettava Bing Maps-Ohjelmointirajapinnan tuloksen mallin luominen. Helpoin tapa tehdä on, napsauta hiiren kakkospainikkeella `Models` kansioon, **Lisää** > **luokka**. Anna sille nimi `GeofenceBoundary.cs`. Saapuvia kopioi JSON API-vastauksen, joka on kerrottu ensimmäisen osan ja Visual Studio käytössä **Muokkaa** > **Liitä määräten** > **Liitä JSON luokiksi**. 

Sen mukaan, varmistamme, että objekti poistaa täsmälleen samalla tavalla kuin se on tarkoitettu. Luokan tulosjoukko pitäisi muistuttaa näin:

    namespace AppBackend.Models
    {
        public class Rootobject
        {
            public D d { get; set; }
        }

        public class D
        {
            public string __copyright { get; set; }
            public Result[] results { get; set; }
        }

        public class Result
        {
            public __Metadata __metadata { get; set; }
            public string EntityID { get; set; }
            public string Name { get; set; }
            public float Longitude { get; set; }
            public float Latitude { get; set; }
            public string Boundary { get; set; }
            public string Confidence { get; set; }
            public string Locality { get; set; }
            public string AddressLine { get; set; }
            public string AdminDistrict { get; set; }
            public string CountryRegion { get; set; }
            public string PostalCode { get; set; }
        }

        public class __Metadata
        {
            public string uri { get; set; }
        }
    }

Avaa seuraavaksi `Controllers`  >  `NotificationsController.cs`. Annettava säätää tehosteasetuksilla kirjaa puhelun kohde pituutta ja leveyttä-tiliin. Kohdalla, lisää kaksi merkkijonoa funktion – allekirjoituksen `latitude` ja `longitude`.

    public async Task<HttpResponseMessage> Post(string pns, [FromBody]string message, string to_tag, string latitude, string longitude)

Luoda uuden luokan nimi projektin `ApiHelper.cs` – Käytämme se tarkistaa Bing yhdistäminen ja valitse reunan risteykset. Ota käyttöön `IsPointWithinBounds` -funktio, seuraavasti:

    public class ApiHelper
    {
        public static readonly string ApiEndpoint = "{YOUR_QUERY_ENDPOINT}?spatialFilter=intersects(%27POINT%20({0}%20{1})%27)&$format=json&key={2}";
        public static readonly string ApiKey = "{YOUR_API_KEY}";

        public static bool IsPointWithinBounds(string longitude,string latitude)
        {
            var json = new WebClient().DownloadString(string.Format(ApiEndpoint, longitude, latitude, ApiKey));
            var result = JsonConvert.DeserializeObject<Rootobject>(json);
            if (result.d.results != null && result.d.results.Count() > 0)
            {
                return true;
            }
            else
            {
                return false;
            }
        }
    }

>[AZURE.NOTE] Varmista, että korvaa API päätepisteen kyselyn URL-osoite, jotka olet hankkinut aiemmin Centeristä Bing keskihajonta (Luo Ohjelmointirajapinnan avain). 

Jos kyselyn tuloksia, joka tarkoittaa, että kohdan on geofence, rajojen niin palautettavien `true`. Jos luettelossa on tuloksia, Bing kehottaa meille, että kohta on haku-kehyksen ulkopuolella, joten palautettavien `false`.

Siirry takaisin `NotificationsController.cs`, tarkista oikea ennen switch-lauseen luominen:

    if (ApiHelper.IsPointWithinBounds(longitude, latitude))
    {
        switch (pns.ToLower())
        {
            case "wns":
                //// Windows 8.1 / Windows Phone 8.1
                var toast = @"<toast><visual><binding template=""ToastText01""><text id=""1"">" +
                            "From " + user + ": " + message + "</text></binding></visual></toast>";
                outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, userTag);

                // Windows 10 specific Action Center support
                toast = @"<toast><visual><binding template=""ToastGeneric""><text id=""1"">" +
                            "From " + user + ": " + message + "</text></binding></visual></toast>";
                outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, userTag);

                break;
        }
    }

Näin ilmoituksen vain lähettää, kun piste on rajojen sisällä.

##<a name="testing-push-notifications-in-the-uwp-app"></a>Push-ilmoitukset testaaminen UWP-sovelluksessa

Palaaminen UWP-sovellus on nyt pitäisi Testaa ilmoitukset. Sisällä `LocationHelper` luokan, Luo uusi funktio – `SendLocationToBackend`:

    public static async Task SendLocationToBackend(string pns, string userTag, string message, string latitude, string longitude)
    {
        var POST_URL = "http://localhost:8741/api/notifications?pns=" +
            pns + "&to_tag=" + userTag + "&latitude=" + latitude + "&longitude=" + longitude;

        using (var httpClient = new HttpClient())
        {
            try
            {
                await httpClient.PostAsync(POST_URL, new StringContent("\"" + message + "\"",
                    System.Text.Encoding.UTF8, "application/json"));
            }
            catch (Exception ex)
            {
                Debug.WriteLine(ex.Message);
            }
        }
    }

>[AZURE.NOTE] Vaihda `POST_URL` käyttöön web-sovelluksen, joka on luotu edellisessä osassa haluamaasi kohtaan. Nyt voit suorittaa paikallisesti, mutta kun työstät käyttöönotto julkinen versio, sinun on isännöitävä ulkoisen tarjoajan.

Oletetaan, että, varmista, että olemme Rekisteröi push-ilmoitukset UWP-sovellukseen. Visual Studiossa, valitse **projektin** > **tallentaa** > **Liitä-kaupan sovellus**.

![](./media/notification-hubs-geofence/vs-associate-with-store.png)

Kun olet kirjautunut sisään Kehitystyökalut-tilillesi, varmista, että voit valita olemassa olevan sovelluksen tai luoda uuden ja liittää siihen paketin. 

Siirry keskihajonta Centeriin ja Avaa sovellus juuri luomasi. Valitse **palvelut** > **Push-ilmoitukset** > **Live Services-sivustossa**.

![](./media/notification-hubs-geofence/ms-live-services.png)

Sivuston Ota seuraavat seikat huomioon **Sovelluksen salaisuus** ja **Paketin SUOJAUSTUNNUS**. Tarvitset sekä Azure-portaalissa – Avaa ilmoitus-toiminnosta, valitse **asetukset** > **Ilmoituksen Services** > **Windows (WNS)** ja kirjoita tiedot pakolliset kentät.

![](./media/notification-hubs-geofence/notification-hubs-wns.png)

Valitse **Tallenna**.

Napsauta **Ratkaisunhallinnassa** **viittaukset** hiiren kakkospainikkeella ja valitse **NuGet pakettien hallinta**. Lisää viittaus **Microsoft Azure palvelun Bus hallitun kirjasto** – Etsi yksinkertaisesti annettava `WindowsAzure.Messaging.Managed` ja sen lisääminen projektiin.

![](./media/notification-hubs-geofence/vs-nuget.png)

Testausta varten, voit luoda `MainPage_Loaded` tapahtumakäsittelijä uudelleen, tätä koodikatkelman lisääminen:

    var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

    var hub = new NotificationHub("HUB_NAME", "HUB_LISTEN_CONNECTION_STRING");
    var result = await hub.RegisterNativeAsync(channel.Uri);

    // Displays the registration ID so you know it was successful
    if (result.RegistrationId != null)
    {
        Debug.WriteLine("Reg successful.");
    }

Edellä Rekisteröi sovelluksen ilmoitus-toiminnossa. Olet valmis! 

Valitse `LocationHelper`, sisäpuolella `Geolocator_PositionChanged` käsittelijä, voit lisätä osan testi-koodi, joka sijoitetaan pakottamalla sisällä geofence sijainti:

    await LocationHelper.SendLocationToBackend("wns", "TEST_USER", "TEST", "37.7746", "-122.3858");

Koska kopiointia ovat ei kulkeva reaali koordinaatteja (jotka eivät välttämättä ole rajojen sisällä tällä hetkellä) ja käytä ennalta määritettyjä testin, näemme päivityksen näy ilmoituksen:

![](./media/notification-hubs-geofence/notification-hubs-test-notification.png)

##<a name="whats-next"></a>Mitä seuraavaksi?

Muutama vaiheet, voit joutua muuttamaan noudattamalla edellä varmistaaksesi, että ratkaisu on tuotannon käyttövalmiin lisäksi.

Keskustelupalstoja haluat ehkä varmistaa, että geofences dynaaminen. Tämä vaatii muutamia ylimääräisiä käsitteleminen Bing-Ohjelmointirajapinnan jotta voi ladata uusia rajojen sisällä aiemmasta tietolähteestä. Lue lisätietoja aiheesta [Bing paikkatietojen Data Services API-ohjeissa](https://msdn.microsoft.com/library/ff701734.aspx) .

Se, kun käsittelet varmistamiseksi toimituksen tehdään oikean osallistujille, haluat ehkä kohdistaa ne kautta [tunnisteita](notification-hubs-tags-segment-push-message.md).

Yllä ratkaisu kuvataan tilanne, jossa voi olla erilaisia kohdeympäristö, joten emme ole rajoittaa geofencing järjestelmän kielikohtaiset ominaisuudet. Toisaalta, yleinen Windows-ympäristö on esiintyvien [geofences oikean ulos,-valmiilla](https://msdn.microsoft.com/windows/uwp/maps-and-location/set-up-a-geofence)ominaisuuksia.

Lisätietoja koskeva ilmoitus keskittimet ominaisuuksia Tutustu Microsoftin [asiakirjat-portaalissa](https://azure.microsoft.com/documentation/services/notification-hubs/).
