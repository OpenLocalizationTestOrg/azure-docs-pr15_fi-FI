<properties
    pageTitle="Rekisteröinnin hallinta"
    description="Tässä ohjeaiheessa kerrotaan, miten voit rekisteröidä laitteiden ilmoituksen keskittimet jotta push-ilmoitusten vastaanottaminen."
    services="notification-hubs"
    documentationCenter=".net"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-multiple"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

# <a name="registration-management"></a>Rekisteröinnin hallinta

##<a name="overview"></a>Yleiskatsaus

Tässä ohjeaiheessa kerrotaan, miten voit rekisteröidä laitteiden ilmoituksen keskittimet jotta push-ilmoitusten vastaanottaminen. Aiheen merkintöjen ylätasolla kuvaa ja valitse esitellään kaksi tärkeimmät kuviot rekisteröitymisestä laitteet: rekisteröiminen laitteesta suoraan, ilmoitus-toiminnosta ja rekisteröityvät sovelluksen Taustajärjestelmä kautta. 


##<a name="what-is-device-registration"></a>Mikä on laitteen rekisteröinti

Ilmoitus-toiminnossa laitteen rekisteröinti tehdään **rekisteröinti** tai **asennuksen**avulla.

#### <a name="registrations"></a>Merkintöjen
Rekisteröinti liittää Platform ilmoituksen Service (PNS) kahvaa laitteen tunnisteet ja mahdollisesti mallina. PNS kahvaa voi olla ChannelURI, laite tunnuksen tai GCM tunnuksella. Ilmoitusten reitittämiseen laitteen kahvoja oikea joukko käytetään tunnisteita. Lisätietoja on artikkelissa [Reititys ja tunnisteen lausekkeet](notification-hubs-tags-segment-push-message.md). Malleja käytetään toteuttamisesta rekisteröinti muunnos. Lisätietoja on artikkelissa [malleja](notification-hubs-templates-cross-platform-push-messages.md).

#### <a name="installations"></a>Asennukset
Asennus on parannettu rekisteröinti, joka sisältää kantolaukku, push liittyvät ominaisuudet. On viimeisimpiä hallintatavan rekisteröiminen laitteet. Mutta se ei tue saavuttaman vielä asiakaspuolen .NET SDK ([Ilmoitus keskittimeen SDK Taustajärjestelmä toimintoja varten](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/)).  Tämä tarkoittaa sitä, jos asiakkaan itse laitteesta, voit on tukevat asennusten [Ilmoituksen keskittimet REST API](https://msdn.microsoft.com/library/mt621153.aspx) -menetelmän avulla. Jos käytössäsi on Taustajärjestelmä palvelu, on oltava käyttää [Ilmoituksen keskittimeen SDK Taustajärjestelmä toimintoja varten](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).

Seuraavassa on joitakin keskeisiä etuja asennukset:

* Luominen tai päivittäminen asennus on täysin idempotent. Voit niin uudelleen sen ilman, että kaikki kaksoiskappaleet merkintöjä.
* Asennus-malli on helppo tehdä yksittäisiä työntää – kohdistamisen laitetta. Järjestelmän tunnisteen **"$InstallationId: [installationId]"** lisätään automaattisesti kunkin asennuksen perusteella rekisteröinnin yhteydessä. Näin voit soittaa Lähetä kohdentamisen tietyn laitteen eikä sinun tarvitse tehdä mitään muita coding tämän tunnisteen.
* Asennusten myös avulla voit tehdä osittaisen rekisteröinti päivitykset. Osittaisen päivityksen asennuksen pyydetään [JSON korjaustiedoston standard](https://tools.ietf.org/html/rfc6902)käyttäminen korjaus-menetelmällä. Tämä on erityisen hyödyllinen, kun haluat päivittää tunnisteita voi tarkastella rekisteröinnin. Sinun ei tarvitse koko rekisteröinnin avattava ja lähetä sitten edellisen tunnisteet uudelleen.

Asennuksen voi olla seuraavat ominaisuudet. Täydellinen luettelo asennuksen ominaisuudet artikkeleissa, [luominen tai korvaa REST-ohjelmointirajapinnalla asennuksen](https://msdn.microsoft.com/library/azure/mt621153.aspx) ja [Asennuksen ominaisuudet](https://msdn.microsoft.com/library/azure/microsoft.azure.notificationhubs.installation_properties.aspx) .

    // Example installation format to show some supported properties
    {
        installationId: "",
        expirationTime: "",
        tags: [],
        platform: "",
        pushChannel: "",
        ………
        templates: {
            "templateName1" : {
                body: "",
                tags: [] },
            "templateName2" : {
                body: "",
                // Headers are for Windows Store only
                headers: {
                    "X-WNS-Type": "wns/tile" }
                tags: [] }
        },
        secondaryTiles: {
            "tileId1": {
                pushChannel: "",
                tags: [],
                templates: {
                    "otherTemplate": {
                        bodyTemplate: "",
                        headers: {
                            ... }
                        tags: [] }
                }
            }
        }
    }

 

On tärkeää muistaa, että merkintöjen ja laitokset oletusarvoisesti ole enää voimassa.

Merkintöjen ja laitokset on oltava kelvollinen PNS kahva kunkin laitteen/kanavan. Koska PNS kahvoja saadaan vain asiakas-sovelluksessa laitteeseen, yksi kuvio on rekisteröityä suoraan laitteen asiakas-sovelluksen kanssa. Toisaalta suojausasiat ja liittyvien tunnisteiden liiketoimintalogiikan saattaa edellyttää hallittavan laitteen rekisteröinnin app taustatietokannan. 

#### <a name="templates"></a>Mallit

Jos haluat käyttää [malleja](notification-hubs-templates-cross-platform-push-messages.md), laitteen asentamisen pidä myös kaikki mallit JSON että laitteeseen liittyvät muotoileminen (katso yllä malli). Mallin nimet kertovat kohteen eri malleja samaan laitteeseen.

Huomaa, että kunkin mallin nimi karttoja mallin tekstissä ja valinnainen joukko tunnisteet. Lisäksi eri ympäristöissä voi olla muita mallin ominaisuudet. Windows-kaupan (joko WNS) ja Windows Phone 8 (joko MPNS) otsikot joukko voi olla osa mallia. Kyseessä APN voit määrittää määräajan-ominaisuuden joko vakio tai mallia lauseke. Täydellinen luettelo asennuksen ominaisuudet ohjeaiheessa, [luominen tai korvaa asennuksen muiden kanssa](https://msdn.microsoft.com/library/azure/mt621153.aspx) .

#### <a name="secondary-tiles-for-windows-store-apps"></a>Toissijainen ruutuja Windows-kaupan sovelluksia varten

Windows-kaupan asiakassovellukset ilmoitukset lähetetään toissijainen ruudut on sama kuin lähettäminen ensisijainen. Tämä tuetaan myös asennuksia. Huomaa, että toissijainen ruutujen eri ChannelUri, jotka asiakas-sovellukseen SDK käsittelee läpinäkyvä.

SecondaryTiles sanaston käyttää samaa TileId, jota käytetään SecondaryTiles-objektin luominen Windows-kaupan sovellus.
Ensisijainen ChannelUri avulla ChannelUris toissijainen ruutujen muuttaa milloin tahansa. Säilyttää asennuksesta päivitetty ilmoitus-toiminnossa, jotta laite on päivitettävä ne toissijainen ruudut nykyisen ChannelUris kanssa.


##<a name="registration-management-from-the-device"></a>Laitteen rekisteröinnin hallintaan

Jos laitteen rekisteröinti hallitseminen asiakassovellukset, taustaan on vain vastuussa ilmoitusten lähettäminen. Asiakassovellukset PNS kahvoja pitäminen ajan tasalla ja rekisteröi tunnisteet. Seuraava kuva havainnollistaa tätä mallia.

![](./media/notification-hubs-registration-management/notification-hubs-registering-on-device.png)

Laitteen ensin PNS kahvaa hakee PNS ja valitse Rekisteröi ilmoitus-toiminnossa suoraan. Kun rekisteröinti on tarkistettu, sovelluksen Taustajärjestelmä lähettää ilmoituksen kohdistamisen rekisteröinnin. Saat lisätietoja ilmoitukset lähetetään [Reititys ja tunnisteen lausekkeet](notification-hubs-tags-segment-push-message.md).
Huomaa, että tässä tapauksessa voit käyttää vain kuuntelevat käyttöoikeudet ilmoituksen keskittimet laitteesta. Lisätietoja on artikkelissa [Suojaus](notification-hubs-push-notification-security.md).

Rekisteröiminen laitteesta on helpoin tapa, mutta se on joitakin haitoista.
Ensimmäinen haitta on, että asiakas-sovelluksen vain päivittää sen tunnisteet kun sovellus on valittuna. Esimerkiksi jos käyttäjällä on kaksi laitetta, jotka liittyvät urheilu ryhmiä, kun ensimmäinen laite Rekisteröi muita tunnisteen (esimerkiksi Seahawks) tunnisteet rekisteröidä, toisen laitteen saa ilmoituksia Seahawks kunnes toisen laitteen sovelluksen suoritetaan toisen kerran. Kun tunnisteet koskee useita laitteita, tunnisteet hallitseminen taustaan laajemmin on olisi vaihtoehto.
Toisen haitta rekisteröinnin hallinnan asiakas-sovelluksen on, koska sovellukset voivat olla hakkeroitu, tunnisteet rekisteröinti suojaaminen edellyttää ylimääräisiä tarkkaan kuvatulla tavalla osan "tunnisteen käyttäjätason suojauksen."



#### <a name="example-code-to-register-with-a-notification-hub-from-a-device-using-an-installation"></a>Ilmoitus-toiminnossa käyttämällä asennusta laitteesta rekisteröityä esimerkkikoodi 

Tällä hetkellä tätä tuetaan vain [Ilmoituksen keskittimet REST API](https://msdn.microsoft.com/library/mt621153.aspx)käyttäminen.

Voit käyttää myös KORJAUSTIEDOSTON menetelmällä päivityksessä asennuksen [JSON korjaustiedoston vakio](https://tools.ietf.org/html/rfc6902) .

    class DeviceInstallation
    {
        public string installationId { get; set; }
        public string platform { get; set; }
        public string pushChannel { get; set; }
        public string[] tags { get; set; }
    }

    private async Task<HttpStatusCode> CreateOrUpdateInstallationAsync(DeviceInstallation deviceInstallation,
         string hubName, string listenConnectionString)
    {
        if (deviceInstallation.installationId == null)
            return HttpStatusCode.BadRequest;

        // Parse connection string (https://msdn.microsoft.com/library/azure/dn495627.aspx)
        ConnectionStringUtility connectionSaSUtil = new ConnectionStringUtility(listenConnectionString);
        string hubResource = "installations/" + deviceInstallation.installationId + "?";
        string apiVersion = "api-version=2015-04";

        // Determine the targetUri that we will sign
        string uri = connectionSaSUtil.Endpoint + hubName + "/" + hubResource + apiVersion;

        //=== Generate SaS Security Token for Authorization header ===
        // See, https://msdn.microsoft.com/library/azure/dn495627.aspx
        string SasToken = connectionSaSUtil.getSaSToken(uri, 60);

        using (var httpClient = new HttpClient())
        {
            string json = JsonConvert.SerializeObject(deviceInstallation);

            httpClient.DefaultRequestHeaders.Add("Authorization", SasToken);

            var response = await httpClient.PutAsync(uri, new StringContent(json, System.Text.Encoding.UTF8, "application/json"));
            return response.StatusCode;
        }
    }

    var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

    string installationId = null;
    var settings = ApplicationData.Current.LocalSettings.Values;

    // If we have not stored a installation id in application data, create and store as application data.
    if (!settings.ContainsKey("__NHInstallationId"))
    {
        installationId = Guid.NewGuid().ToString();
        settings.Add("__NHInstallationId", installationId);
    }

    installationId = (string)settings["__NHInstallationId"];

    var deviceInstallation = new DeviceInstallation
    {
        installationId = installationId,
        platform = "wns",
        pushChannel = channel.Uri,
        //tags = tags.ToArray<string>()
    };

    var statusCode = await CreateOrUpdateInstallationAsync(deviceInstallation, 
                        "<HUBNAME>", "<SHARED LISTEN CONNECTION STRING>");

    if (statusCode != HttpStatusCode.Accepted)
    {
        var dialog = new MessageDialog(statusCode.ToString(), "Registration failed. Installation Id : " + installationId);
        dialog.Commands.Add(new UICommand("OK"));
        await dialog.ShowAsync();
    }
    else
    {
        var dialog = new MessageDialog("Registration successful using installation Id : " + installationId);
        dialog.Commands.Add(new UICommand("OK"));
        await dialog.ShowAsync();
    }

   

#### <a name="example-code-to-register-with-a-notification-hub-from-a-device-using-a-registration"></a>Ilmoitus-toiminnossa käyttävän laitteen rekisteröinti-rekisteröityä esimerkkikoodi


Näistä tavoista luominen tai päivittäminen, niitä kutsutaan laitteen rekisteröinti. Tämä tarkoittaa sitä, jotta voit päivittää kahvaa tai tunnisteita, on korvaa koko rekisteröinti. Muista, että merkintöjen on lyhytkestoisia, niin on oltava aina luotettava säilön kanssa tietyn laitteen korjattava nykyiset tunnisteet.


    // Initialize the Notification Hub
    NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString(listenConnString, hubName);

    // The Device id from the PNS
    var pushChannel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

    // If you are registering from the client itself, then store this registration id in device
    // storage. Then when the app starts, you can check if a registration id already exists or not before
    // creating.
    var settings = ApplicationData.Current.LocalSettings.Values;

    // If we have not stored a registration id in application data, store in application data.
    if (!settings.ContainsKey("__NHRegistrationId"))
    {
        // make sure there are no existing registrations for this push handle (used for iOS and Android)    
        string newRegistrationId = null;
        var registrations = await hub.GetRegistrationsByChannelAsync(pushChannel.Uri, 100);
        foreach (RegistrationDescription registration in registrations)
        {
            if (newRegistrationId == null)
            {
                newRegistrationId = registration.RegistrationId;
            }
            else
            {
                await hub.DeleteRegistrationAsync(registration);
            }
        }

        newRegistrationId = await hub.CreateRegistrationIdAsync();

        settings.Add("__NHRegistrationId", newRegistrationId);
    }
     
    string regId = (string)settings["__NHRegistrationId"];

    RegistrationDescription registration = new WindowsRegistrationDescription(pushChannel.Uri);
    registration.RegistrationId = regId;
    registration.Tags = new HashSet<string>(YourTags);

    try
    {
        await hub.CreateOrUpdateRegistrationAsync(registration);
    }
    catch (Microsoft.WindowsAzure.Messaging.RegistrationGoneException e)
    {
        settings.Remove("__NHRegistrationId");
    }


## <a name="registration-management-from-a-backend"></a>Taustassa rekisteröinti hallintaan

Merkintöjen hallitseminen taustaan edellyttää muita koodin kirjoittamista. Sovelluksen laitteesta on määritettävä päivitetyt PNS täyttökahvaa taustaan aina, kun sovellus käynnistyy (sekä tunnisteet ja malleja) ja taustaan täytyy päivittää tämän kahvan kohdalle ilmoitus-toiminnossa. Seuraavassa kuvassa on kuvattu tämän rakenne.

![](./media/notification-hubs-registration-management/notification-hubs-registering-on-backend.png)

Merkintöjen hallitseminen taustaan etuja ovat mahdollisuus merkintöjä, tunnisteiden muokkaamiseen, vaikka laitteeseen vastaavan sovellus ei ole käytössä ja tarkistamiseen asiakkaan sovelluksen ennen kuin lisäät sen rekisteröinti tunnisteen.


#### <a name="example-code-to-register-with-a-notification-hub-from-a-backend-using-an-installation"></a>Ilmoitus-toiminnossa taustasta, käyttämällä asennusta rekisteröityä esimerkkikoodi

Asiakaslaitteen edelleen saa sen PNS kahvaa ja ennen kuin tarvittavat ominaisuudet ja soittaa mukautetun API taustassa, voit suorittaa rekisteröinnin ja määritä tunnisteet jne. Taustaan voidaan hyödyntää [Ilmoituksen keskittimeen SDK Taustajärjestelmä toimintoja varten](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).

Voit käyttää myös KORJAUSTIEDOSTON menetelmällä päivityksessä asennuksen [JSON korjaustiedoston vakio](https://tools.ietf.org/html/rfc6902) .
 

    // Initialize the Notification Hub
    NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString(listenConnString, hubName);

    // Custom API on the backend
    public async Task<HttpResponseMessage> Put(DeviceInstallation deviceUpdate)
    {

        Installation installation = new Installation();
        installation.InstallationId = deviceUpdate.InstallationId;
        installation.PushChannel = deviceUpdate.Handle;
        installation.Tags = deviceUpdate.Tags;

        switch (deviceUpdate.Platform)
        {
            case "mpns":
                installation.Platform = NotificationPlatform.Mpns;
                break;
            case "wns":
                installation.Platform = NotificationPlatform.Wns;
                break;
            case "apns":
                installation.Platform = NotificationPlatform.Apns;
                break;
            case "gcm":
                installation.Platform = NotificationPlatform.Gcm;
                break;
            default:
                throw new HttpResponseException(HttpStatusCode.BadRequest);
        }


        // In the backend we can control if a user is allowed to add tags
        //installation.Tags = new List<string>(deviceUpdate.Tags);
        //installation.Tags.Add("username:" + username);

        await hub.CreateOrUpdateInstallationAsync(installation);

        return Request.CreateResponse(HttpStatusCode.OK);
    }


#### <a name="example-code-to-register-with-a-notification-hub-from-a-device-using-a-registration-id"></a>Ilmoitus-toiminnossa rekisteröinti tunnuksen avulla laitteesta rekisteröityä esimerkkikoodi

Sovelluksen lisääminen taustasta voit suorittaa CRUDS perustoimintojen rekisteröinnissä. Esimerkki:

    var hub = NotificationHubClient.CreateClientFromConnectionString("{connectionString}", "hubName");
            
    // create a registration description object of the correct type, e.g.
    var reg = new WindowsRegistrationDescription(channelUri, tags);

    // Create
    await hub.CreateRegistrationAsync(reg);

    // Get by id
    var r = await hub.GetRegistrationAsync<RegistrationDescription>("id");

    // update
    r.Tags.Add("myTag");

    // update on hub
    await hub.UpdateRegistrationAsync(r);

    // delete
    await hub.DeleteRegistrationAsync(r);


Taustaan on käsiteltävä samanaikainen rekisteröinti päivitykset välillä. Palvelun Bus on optimistisen ohjausobjektin rekisteröinti hallintaa varten. HTTP tasolla tämä on toteutettu rekisteröinti hallintatoiminnot ETag käyttö. Tämä ominaisuus on läpinäkyvä käyttämä Microsoft SDKs, joka palauttaa poikkeuksen, jos päivitys on hylätty samanaikainen syistä. Sovelluksen Taustajärjestelmä on vastuussa käsittelyyn poikkeukset ja yritetään päivityksen tarvittaessa.