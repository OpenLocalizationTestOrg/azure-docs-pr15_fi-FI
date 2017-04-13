<properties
    pageTitle="Lisää Apache Cordova-sovelluksessa, jossa Azure Mobiilisovellusten Push-ilmoitukset | Azure sovelluksen-palvelu"
    description="Opettele Azure Mobile-sovellusten avulla voit lähettää push-ilmoitukset Apache Cordova-sovellukseen."
    services="app-service\mobile"
    documentationCenter="javascript"
    manager="erikre"
    editor=""
    authors="ysxu"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-html"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="yuaxu"/>

# <a name="add-push-notifications-to-your-apache-cordova-app"></a>Push-ilmoitukset lisääminen Apache Cordova-sovellukseen

[AZURE.INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>Yleiskatsaus

Tässä opetusohjelmassa Lisää push-ilmoitukset [Apache Cordova nopeasti Käynnistä] projektin niin, että push-ilmoitus lähetetään laitteen aina, kun tietue lisätään.

Jos et käytä ladatut pikaopas server-projekti, sinun on push ilmoituksen tunniste-paketti. Lisätietoja on kohdassa [.NET palvelimeen Azure-mobiilisovellukset SDK käsitteleminen](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) .

##<a name="prerequisites"></a>Edellytykset

Tässä opetusohjelmassa kerrotaan Apache Cordova sovelluksen kehittänyt Visual Studio 2015, joka suoritetaan Google Android emulaattorin, Android-laitteessa, Windows-laitteessa ja iOS-laitteen kanssa.

Jotta voit suorittaa tässä opetusohjelmassa, sinun on:

* Tietokone, jossa on [Visual Studio yhteisön 2015] tai uudempi versio.
* [Visual Studio Tools for Apache Cordova].
* [Aktiivinen Azure-tili](https://azure.microsoft.com/pricing/free-trial/).
* Valmiin [Apache Cordova pikaopas] projektin.
* (Android) On [Google-tilin] varmennettu sähköpostiosoite.
* (iOS) Apple kehittäjä-ohjelman jäsenyys- ja iOS-laitteessa (iOS Simulator ei tue push).
* (Windows) Windows-kaupan Kehitystyökalut-tili ja Windows 10-laitteeseen.

##<a name="configure-hub"></a>Määritä ilmoitus-toiminnossa

[AZURE.INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

[Katso tämä video näyttää tämän osan vaiheet](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-3-Create-azure-notification-hub)

##<a name="update-the-server-project-to-send-push-notifications"></a>Päivitä push-ilmoitukset lähetetään server-projekti

[AZURE.INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

##<a name="add-push-to-app"></a>Push-ilmoitusten vastaanottotilanteiden Cordova sovelluksen muokkaaminen

Sinun on tehtävä että Apache Cordova sovelluksen projektin on valmis käsittelemään push-ilmoitukset asentamalla Cordova push-laajennus plus käyttöjärjestelmäkohtaiset push palvelut.

#### <a name="update-the-cordova-version-in-your-project"></a>Päivitä projektin Cordova-versio.

On suositeltavaa päivittää asiakkaan projektin Cordova 6.1.1 Jos projektissa on määritetty on vanhempi versio. Päivitä projekti-hiiren kakkospainikkeella config.xml Avaa määritysten suunnittelutyökalu. Valitse ympäristöjen-välilehti ja valitse 6.1.1 **Cordova CLI** tekstiruutuun.

Valitsemalla **Luo**ja sitten **Luo ratkaisun** päivittää projektin.

#### <a name="install-the-push-plugin"></a>Push-laajennuksen asentaminen

Apache Cordova sovellukset eivät käsittele grafiikkatiedostomuotoja laite- tai verkko-ominaisuuksia.  Nämä ominaisuudet ovat myöntämä laajennukset, jotka on julkaistu [npm](https://www.npmjs.com/) tai GitHub.  `phonegap-plugin-push` Laajennuksen käytetään käsitellään verkon push-ilmoitukset.

Voit asentaa push-laajennus jollakin seuraavista tavoista:

**Komentoriviltä seuraavasti:**

Suorita seuraava komento:

    cordova plugin add phonegap-plugin-push

**Valitse Visual Studion:**

1.  Napsauta ratkaisunhallinnassa, Avaa `config.xml` tiedosto napsauttamalla **laajennukset** > **Mukautettu**, valitse **Git** asennuslähteeseen ja kirjoita sitten `https://github.com/phonegap/phonegap-plugin-push` lähteenä.

    ![](./media/app-service-mobile-cordova-get-started-push/add-push-plugin.png)

2.  Valitse asennuslähteeseen vieressä olevaa nuolta.

3. **SENDER_ID**, jos sinulla on jo numeerinen project-ID Google Developer Console-projekti, voit lisätä sen tähän. Muussa tapauksessa paikkamerkinarvo, 777777, kuten ja jos kohdennat Android voit päivittää myöhemmin config.xml arvo.

4. Valitse **Lisää**.

Push-laajennus on asennettu.

####<a name="install-the-device-plugin"></a>Asenna laajennus laite

Toimi push-laajennuksen asentamisessa, mutta huomaat laitteen laajennus Core laajennukset-luettelossa (Valitse **laajennukset** > **Core** löytääksesi sen). Tarvitset laajennus hankkiminen käyttöympäristön nimi (`device.platform`).

#### <a name="register-your-device-for-push-on-start-up"></a>Push-pysäyttämisestä laitteen rekisteröiminen

Olemme aluksi sisältää mahdollisimman vähän lisäkoodin androidille. Olemme myöhemmin, julkaista joitakin pieniä muutoksia Suorita iOS-tai Windows 10: ssä.

1. Lisää **registerForPushNotifications** puhelun aikana takaisinkutsu kirjautumisen tai **onDeviceReady** menetelmä alareunassa:

        // Login to the service.
        client.login('google')
            .then(function () {
                // Create a table reference
                todoItemTable = client.getTable('todoitem');

                // Refresh the todoItems
                refreshDisplay();

                // Wire up the UI Event Handler for the Add Item
                $('#add-item').submit(addItemHandler);
                $('#refresh').on('click', refreshDisplay);

                    // Added to register for push notifications.
                registerForPushNotifications();

            }, handleError);

    Tässä esimerkissä näkyy kutsumista **registerForPushNotifications** , kun todennus on muodostettu, jossa suositellaan, kun käyttämällä push-ilmoitukset ja todennusta-sovelluksen.

2. Lisää uusi **registerForPushNotifications** menetelmä seuraavasti:

        // Register for Push Notifications. Requires that phonegap-plugin-push be installed.
        var pushRegistration = null;
        function registerForPushNotifications() {
          pushRegistration = PushNotification.init({
              android: { senderID: 'Your_Project_ID' },
              ios: { alert: 'true', badge: 'true', sound: 'true' },
              wns: {}
          });

        // Handle the registration event.
        pushRegistration.on('registration', function (data) {
          // Get the native platform of the device.
          var platform = device.platform;
          // Get the handle returned during registration.
          var handle = data.registrationId;
          // Set the device-specific message template.
          if (platform == 'android' || platform == 'Android') {
              // Register for GCM notifications.
              client.push.register('gcm', handle, {
                  mytemplate: { body: { data: { message: "{$(messageParam)}" } } }
              });
          } else if (device.platform === 'iOS') {
              // Register for notifications.            
              client.push.register('apns', handle, {
                  mytemplate: { body: { aps: { alert: "{$(messageParam)}" } } }
              });
          } else if (device.platform === 'windows') {
              // Register for WNS notifications.
              client.push.register('wns', handle, {
                  myTemplate: {
                      body: '<toast><visual><binding template="ToastText01"><text id="1">$(messageParam)</text></binding></visual></toast>',
                      headers: { 'X-WNS-Type': 'wns/toast' } }
              });
          }
        });

        pushRegistration.on('notification', function (data, d2) {
          alert('Push Received: ' + data.message);
        });

        pushRegistration.on('error', handleError);
        }

3. (Android) Korvaa yllä koodin `Your_Project_ID` project numeerisen tunnus, kun sovellus [Google Developer konsolissa].

## <a name="optional-configure-and-run-the-app-on-android"></a>(Valinnainen) Määritä ja suorita Android-sovellus

Voit ottaa käyttöön push-ilmoitusten Android suorittaminen

####<a name="enable-gcm"></a>Ota käyttöön Firebase Cloud viestintä

Koska Microsoft on alun perin kohdistamisen Google Android-ympäristö, sinun on otettava Firebase Cloud Messaging. Vastaavasti on kohdistamisen Microsoft Windows-laitteet, jos WNS tuki käyttöön.

[AZURE.INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

####<a name="configure-backend"></a>Määritä Lähetä push-pyyntöjen FCM Mobile-sovelluksen taustaan

[AZURE.INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push.md)]

####<a name="configure-your-cordova-app-for-android"></a>Määritä Cordova sovelluksen androidille

Cordova-sovelluksen, Avaa config.xml ja korvaa `Your_Project_ID` project numeerisen tunnus, kun sovellus [Google Developer konsolissa].

        <plugin name="phonegap-plugin-push" version="1.7.1" src="https://github.com/phonegap/phonegap-plugin-push.git">
            <variable name="SENDER_ID" value="Your_Project_ID" />
        </plugin>

Avaa index.js ja Päivitä koodi, jota käytetään numeerinen projektin käyttämällä.

        pushRegistration = PushNotification.init({
            android: { senderID: 'Your_Project_ID' },
            ios: { alert: 'true', badge: 'true', sound: 'true' },
            wns: {}
        });

####<a name="configure-device"></a>USB virheenkorjaus for Android-laitteen määrittäminen

Ennen kuin voit ottaa käyttöön sovelluksen Android-laitteeseen, sinun on otettava käyttöön USB virheenkorjaus.  Android-puhelimessa toimimalla seuraavasti:

1. Valitse **asetukset** > **tietoja puhelinnumero**ja valitse sitten **koontiversion numero** asti kehittäjätilassa on käytössä (noin 7 kertaa).

2. **Takaisin asetuksissa** > **Developer asetukset** käyttöön **USB virheenkorjaus**ja valitse Android-puhelimessa muodostaa yhteyttä kehittäminen tietokoneen USB-kaapelilla.

Testattu tämä käyttämällä Google Nexuksen 5 X laitteeseen, jossa on Android 6.0 (vaahtokarkki).  Tapoja, joilla on kuitenkin yleisiä yli Moderni Android julkaistu versio.

#### <a name="install-google-play-services"></a>Asenna Google Play-palvelut

Push-laajennus on riippuvainen Android Google Play-palvelujen push-ilmoitukset.  

1.  **Visual Studio**, valitse **Työkalut** > **Android** > **Android SDK hallinta**-Laajenna **Lisäominaisuudet** -kansio ja valitse valintaruutu, varmista, että kaikki seuraavat SDK: T on asennettu.
    * Android 2.3 tai uudempi versio
    * Google-tietovarasto version 27 tai uudempi versio
    * Google Play-palvelujen 9.0.2 tai uudempi versio

2.  Valitse **Asenna pakettien** ja odota, viimeistele asennus.

Nykyisen pakollinen kirjastot on lueteltu [phonegap-laajennus-push asennusohjeet].

#### <a name="test-push-notifications-in-the-app-on-android"></a>Testaa push-ilmoitukset Android-sovelluksessa

Voit nyt testi push-ilmoitukset sovellusta ja lisäämällä nimikkeet TodoItem taulukossa. Tee tämä samaan laitteeseen tai toisen laitteen, kun käytössäsi on sama Taustajärjestelmä. Testaa Cordova sovelluksen Android-ympäristössä jollakin seuraavista tavoista:

- **Fyysinen laitteella:**  
Liittää Android-laitteen kehittäminen tietokoneen USB-kaapelilla.  Sen sijaan, että **Google Android emulaattorin**Valitse **laite**. Visual Studio otetaan käyttöön laitteen sovelluksen ja se toimii puhelimessasi.  Voit käsitellä sitten sovelluksen laitteeseen.  
Kehittäminen käyttäjäkokemuksen parantamiseksi.  Jaettuja näyttöjä sovelluksista, kuten [Mobizen] auttaa sinua kehittäminen Android-sovelluksen projisoiminen tietokoneeseesi Android näytön voin selaimen mukaan.

- **Android emulointiohjelma:**  
On määrityksen lisävaiheet, kun käytössä emulointiohjelma.

    Varmista, että ottaminen käyttöön tai virheenkorjaus virtual laitteessa, joka on määritetty kohde, Google-ohjelmointirajapinnan Android Virtual laitteen (AVD) Managerissa tuoteluokkaryhmien.

    ![](./media/app-service-mobile-cordova-get-started-push/google-apis-avd-settings.png)

    Jos haluat käyttää nopeampaa x86 emulaattorin, [Asenna HAXM ohjain](https://taco.visualstudio.com/en-us/docs/run-app-apache/#HAXM) ja määritä emulaattori käyttää sitä.

    Google-tilin lisääminen Android-laitteen **sovelluksia**valitsemalla > **asetukset** > **Lisää tili**ja valitse seuraa ohjeita voit lisätä aiemmin Google-tili (Suosittelemme aiemmin tilillä sen sijaan, että luot uuden) laitteeseen.

    ![](./media/app-service-mobile-cordova-get-started-push/add-google-account.png)

    Suorita ennen kuin sovellus todolist ja lisää uuden todo kohteen. Tällä hetkellä ilmoituskuvake näkyy ilmaisinalueella. Voit avata ilmoituksen laatikko ilmoituksen koko tekstin tarkasteleminen.

    ![](./media/app-service-mobile-cordova-get-started-push/android-notifications.png)

##<a name="optional-configure-and-run-on-ios"></a>(Valinnainen) Määritä ja suorita iOS

Tässä osassa on käynnissä Cordova projektin iOS-laitteisiin. Voit ohittaa tämän osan, jos käsittelet ei iOS-laitteita.

####<a name="install-and-run-the-ios-remotebuild-agent-on-a-mac-or-cloud-service"></a>Asentaminen ja suorittaminen iOS remotebuild agentti Mac- tai cloud-palvelusta

Ennen kuin voit avata Cordova-sovellus iOS Visual Studiossa, siirry [iOS määritysohjeet](http://taco.visualstudio.com/en-us/docs/ios-guide/) asentaminen ja suorittaminen remotebuild-agentti vaiheita.

Varmista, että voit luoda iOS-sovellukseen. Muodosta iOS Visual Studio tarvittavat toimenpiteet-palvelupaketin määritysopas. Jos sinulla ei ole Mac-tietokoneeseen, voit luoda iOS remotebuild-agentti käyttäminen palvelun kuten MacInCloud. Katso lisätietojen saamiseksi [suorittamalla iOS-sovelluksen pilveen](http://taco.visualstudio.com/en-us/docs/build_ios_cloud/).

>[AZURE.NOTE] IOS push-laajennuksen käyttämiseen tarvitaan XCode 7 tai uudempi.

####<a name="find-the-id-to-use-as-your-app-id"></a>Etsi Tunnusten käyttöasetusten App ID-tunnuksena

Ennen kuin voit rekisteröidä sovelluksen push-ilmoitukset, Avaa config.xml Cordova-sovelluksessa, Etsi `id` widget-osaan arvo ja kopioimalla sen myöhempää käyttöä varten. Seuraavat XML-tunnus on `io.cordova.myapp7777777`.

        <widget defaultlocale="en-US" id="io.cordova.myapp7777777"
        version="1.0.0" windows-packageVersion="1.1.0.0" xmlns="http://www.w3.org/ns/widgets"
            xmlns:cdv="http://cordova.apache.org/ns/1.0" xmlns:vs="http://schemas.microsoft.com/appx/2014/htmlapps">

Myöhemmin käyttää tämän tunnisteen, kun luot sovelluksen tunnus Applen developer-portaalissa. (Jos luominen eri Sovellustunnus developer-portaalissa ja haluat käyttää, sinun on tehtävä muutama lisätoimenpide jäljempänä tässä opetusohjelmassa, voit muuttaa config.xml tämä tunnus. Widget-osan tunnus on vastattava Sovellustunnus developer-portaalissa.)

####<a name="register-the-app-for-push-notifications-on-apples-developer-portal"></a>Rekisteröi sovelluksen push-ilmoitukset Applen developer-portaalissa

[AZURE.INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]

[Katso tämä video, jossa vastaavat vaiheet](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-5-Set-up-apns-for-push)

####<a name="configure-azure-to-send-push-notifications"></a>Azure lähettää push-ilmoitusten määrittäminen

[AZURE.INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

####<a name="verify-that-your-app-id-matches-your-cordova-app"></a>Tarkista, että Sovellustunnus vastaa Cordova sovelluksen

Jos olet luonut jo Apple Kehitystyökalut-tilisi Sovellustunnus vastaa tunnus config.xml widget-elementin, voit ohittaa tämän vaiheen. Kuitenkin jos tunnukset eivät vastaa, tee seuraavat toimet:

1. Poista projektin ympäristöjen-kansio.

2. Poista projektin laajennukset-kansio.

3. Poista projektin node_modules-kansio.

4. Päivitä config.xml käyttämään Sovellustunnus, jonka loit Apple Kehitystyökalut-tilisi widget-elementin id-määrite.

5. Muodosta uudelleen projektin.

#####<a name="test-push-notifications-in-your-ios-app"></a>Testaa push-ilmoitukset iOS-sovelluksessa

1. Visual Studiossa Varmista, että **iOS** käyttöönoton kohteeksi on valittuna ja valitse sitten **laitteen** toimimaan yhdistetyn iOS-laitteessa.

    Voit suorittaa iOS-laitteessa kytketty tietokoneeseen iTunesin avulla. IOS Simulator ei tue push-ilmoitukset.

2. Painamalla **F5-näppäintä** ja **Suorita** -painike Visual Studiossa voivat laatia projektin ja Käynnistä sovellus iOS-laitteessa ja valitse sitten **OK** Hyväksy push-ilmoitukset.

    >[AZURE.NOTE] Sinun on hyväksyttävä push-ilmoitukset erikseen sovelluksestasi. Pyyntö tapahtuu vain silloin, kun sovellus käynnistyy ensimmäisen kerran.

3. -Sovellukseen, kirjoita tehtävän ja valitse sitten plus (+) kuvaketta.

4. Varmista, että ilmoitus on vastaanotettu ja valitse sitten OK hylkää ilmoituksessa.

##<a name="optional-configure-and-run-on-windows"></a>(Valinnainen) Määritä ja suorita Windows

Tässä osassa on käynnissä Apache Cordova sovelluksen projektin Windows 10-laitteisiin (PhoneGap push-laajennus tuetaan Windows 10: ssä). Voit ohittaa tämän osan Jos eivät toimi Windows-laitteiden kanssa.

####<a name="register-your-windows-app-for-push-notifications-with-wns"></a>Voit rekisteröidä WNS Windows-sovellus push-ilmoitukset

Voit käyttää Visual Studiossa kaupan asetukset valitsemalla Windows kohde ratkaisu ympäristöjen tehtäväluettelosta, kuten **Windows x64** tai **Windows-x86** (välttää **Windows AnyCPU** push-ilmoitukset).

[AZURE.INCLUDE [app-service-mobile-register-wns](../../includes/app-service-mobile-register-wns.md)]

[Katso tämä video, jossa vastaavat vaiheet](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-6-Set-up-wns-for-push)

####<a name="configure-the-notification-hub-for-wns"></a>Ilmoitus-toiminnossa määrittäminen WNS

[AZURE.INCLUDE [app-service-mobile-configure-wns](../../includes/app-service-mobile-configure-wns.md)]

####<a name="configure-your-cordova-app-to-support-windows-push-notifications"></a>Cordova sovelluksen tukemaan Windows push-ilmoitusten määrittäminen

Avaa määritysten suunnittelu (hiiren kakkospainikkeen config.xml ja valitse **Näkymän suunnittelu**), valitse **Windows** -välilehti ja valitse **Windows 10: ssä** , **Windows kohde**-versiossa.

>[AZURE.NOTE] Jos käytät Cordova version ennen Cordova 5.1.1 (6.1.1 suositeltava), sinun on määritettävä ilmoitukseen pystyvät merkinnän myös config.xml tosi.

Tukemaan push ilmoitukset oletuskansioon (virheenkorjaus) muodostaa, Avaa build.json tiedosto. Kopioi "release" määritykset virheenkorjaus-määrityksiin.

        "windows": {
            "release": {
                "packageCertificateKeyFile": "res\\native\\windows\\CordovaApp.pfx",
                "publisherId": "CN=yourpublisherID"
            }
        }

Päivityksen jälkeen yllä olevaa koodia pitäisi näyttää tältä.

    "windows": {
        "release": {
            "packageCertificateKeyFile": "res\\native\\windows\\CordovaApp.pfx",
            "publisherId": "CN=yourpublisherID"
            },
        "debug": {
            "packageCertificateKeyFile": "res\\native\\windows\\CordovaApp.pfx",
            "publisherId": "CN=yourpublisherID"
            }
        }

Muodosta sovellus ja varmista, että sinulla on virheitä. Asiakkaan sovelluksen pitäisi nyt Rekisteröidy Mobile-sovelluksen taustasta ilmoitukset. Toista jokaisen Windows projektin ratkaisu-sisältö.

####<a name="test-push-notifications-in-your-windows-app"></a>Testaa push-ilmoitukset Windows-sovelluksessa

Visual Studiossa Varmista, että Windows-ympäristö on valittuna käyttöönoton kohteeksi, kuten **Windows x64** tai **Windows-x86**. Suorita sovellus isännöinnin Visual Studio Windows 10-käyttöjärjestelmää, valitse **Paikallinen kone**.

Paina voivat laatia projektin ja Käynnistä sovellus Suorita-painiketta.

-Sovellukseen, kirjoita uusi todoitem nimi ja valitse sitten plus (+) lisää sen kuvaketta.

Varmista, että ilmoituksen vastaanotetaan, kun kohde on lisätty.

##<a name="next-steps"></a>Seuraavat vaiheet

* Lisätietoja [Ilmoituksen keskittimet] lisätietoja push-ilmoitukset.
* Jos et ole vielä tehnyt edelleen opetusohjelman [Lisääminen käyttöoikeuksien] mukaan Apache Cordova-sovellukseen.

Opettele käyttämään SDK: T.

* [Apache Cordova SDK-paketissa]
* [ASP.NET-palvelimen SDK-paketissa]
* [Node.js Server SDK-paketissa]

<!-- URLs -->
[Käyttöoikeuksien lisääminen]: app-service-mobile-cordova-get-started-users.md
[Apache Cordova pika-aloitusopas]: app-service-mobile-cordova-get-started.md
[authentication]: app-service-mobile-cordova-get-started-users.md
[Work with the .NET backend server SDK for Azure Mobile Apps]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Google-tiliä]: http://go.microsoft.com/fwlink/p/?LinkId=268302
[Google-Developer konsoli]: https://console.developers.google.com/home/dashboard
[phonegap-laajennus-push-asennusohjeet]: https://github.com/phonegap/phonegap-plugin-push/blob/master/docs/INSTALLATION.md
[Mobizen]: https://www.mobizen.com/
[Visual Studio yhteisön 2015]: http://www.visualstudio.com/
[Visual Studio Tools for Apache Cordova]: https://www.visualstudio.com/en-us/features/cordova-vs.aspx
[Ilmoitus keskittimet]: ../notification-hubs/notification-hubs-push-notification-overview.md
[Apache Cordova SDK-paketissa]: app-service-mobile-cordova-how-to-use-client-library.md
[ASP.NET-palvelimen SDK-paketissa]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Node.js Server SDK-paketissa]: app-service-mobile-node-backend-how-to-use-server-sdk.md
