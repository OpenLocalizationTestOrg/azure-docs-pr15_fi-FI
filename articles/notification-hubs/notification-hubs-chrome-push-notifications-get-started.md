<properties
    pageTitle="Push-ilmoitukset lähetetään Chrome-sovellusten kanssa Azure ilmoituksen keskittimet | Microsoft Azure"
    description="Opettele käyttämään Azure ilmoituksen keskittimet push-ilmoitukset lähetetään Chrome-sovellukseen."
    services="notification-hubs"
    keywords="Mobilen push-ilmoitukset, push-ilmoitukset push-ilmoitus, chrome push-ilmoitukset"
    documentationCenter=""
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-chrome"
    ms.devlang="JavaScript"
    ms.topic="hero-article"
    ms.date="10/03/2016"
    ms.author="yuaxu"/>

# <a name="send-push-notifications-to-chrome-apps-with-azure-notification-hubs"></a>Push-ilmoitukset lähetetään Chrome-sovellusten käyttäminen Azure ilmoituksen keskittimet kanssa

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

Tässä ohjeaiheessa esitellään, miten push-ilmoitukset lähetetään Chrome-sovellukseen, joka näytetään Google Chrome-selaimessa puitteissa Azure ilmoituksen keskittimet avulla. Tässä opetusohjelmassa luodaan Chrome-sovellusta, joka vastaanottaa push-ilmoitukset [Google Cloud Messaging (GCM)](https://developers.google.com/cloud-messaging/)avulla. 

>[AZURE.NOTE] Jotta voit suorittaa tässä opetusohjelmassa, on oltava aktiivinen Azure-tili. Jos sinulla ei ole tiliä, voit luoda ilmainen kokeiluversio tili vain muutaman minuutin. Lisätietoja on artikkelissa [Azure maksuttoman kokeiluversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%notification-hubs-chrome-get-started%2F).

Opetusohjelman esitellään push-ilmoitukset käyttöön näin:

* [Google Cloud Messaging ottaminen käyttöön](#register)
* [Määritä ilmoitus-toiminnossa](#configure-hub)
* [Chrome App yhdistäminen ilmoitus-toiminnossa](#connect-app)
* [Lähetä Chrome App push-ilmoitus](#send)
* [Lisätoimintoja ja ominaisuudet](#next-steps)

>[AZURE.NOTE] Chrome app push-ilmoitukset eivät ole yleinen-selaimen ilmoitukset – ne ovat selaimen laajennettavuus malli (Katso lisätietoja [Chrome-sovellusten yleiskatsaus] ). Työpöydän selaimen lisäksi Chrome sovellusten suorittaa Mobile (Android- ja iOS-) Apache Cordova avulla. Katso lisätietoja [Mobile Chrome-sovellukset] .

GCM ja Azure ilmoituksen keskittimet määrittämisestä on sama kuin määrittäminen Android-, koska [Google Cloud Messaging Chrome] on vähennetty ja saman GCM tukee nyt Android-laitteissa ja Chrome esiintymät.

##<a id="register"></a>Google Cloud Messaging ottaminen käyttöön

1. Siirry [Google Cloud Console] -sivustoon, kirjaudu sisään Google-tilin tunnistetiedot ja valitse sitten **Luo projekti** -painiketta. **Projektinimi**, ja valitse sitten **Luo** -painiketta.

    ![Google Cloud konsoli - projektin luominen][1]

2. **Projektinumero** muistiin projektin juuri luomasi **Projektit** -sivulla. Käytetään tämä **GCM Lähettäjätunnus** Chrome-sovelluksessa voit rekisteröidä GCM.

    ![Google Cloud Console - projektinumero][2]

3. Vasemmassa ruudussa **ohjelmointirajapinnan & auth**, valitse ja sitten alaspäin ja valitse Ota käyttöön **Google Cloud Messaging androidille**. Sinun ei tarvitse ottaa käyttöön **Google Cloud Messaging Chrome**.

    ![Google Cloud Console - palvelimen avain][3]

4. Valitse vasemmanpuoleisessa ruudussa **tunnistetiedot** > **Luo uusi avain** > **Palvelimen avain** > **luominen**.

    ![Google Cloud Console - tunnistetiedot][4]

5. Pane merkille palvelimen **Ohjelmointirajapinnan avain**. Voit määrittää tämä ilmoitus-toiminnossa seuraavaksi, jotta se voi lähettää GCM push-ilmoitukset.

    ![Google Cloud Console - Ohjelmointirajapinnan avain][5]

##<a id="configure-hub"></a>Määritä ilmoitus-toiminnossa

[AZURE.INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]


&emsp;&emsp;6. Valitse **asetukset** -sivu Valitse **Ilmoituksen Services** ja **Google (GCM)**. Kirjoita Ohjelmointirajapinnan avain ja Tallenna.

&emsp;&emsp;![Azure ilmoituksen keskittimet - Google (GCM)](./media/notification-hubs-android-get-started/notification-hubs-gcm-api.png)

##<a id="connect-app"></a>Chrome App yhdistäminen ilmoitus-toiminnossa

Ilmoitus-toiminnossa on nyt määritetty toimimaan GCM ja sinulla on yhteysmerkkijonot rekisteröinnissä, kun sovellus sekä vastaanottamista ja lähettämistä push-ilmoitukset. LK

###<a name="create-a-new-chrome-app"></a>Uuden Chrome-sovelluksen luominen

Alla esimerkki [Chrome App GCM otoksen] perusteella, ja käyttää suositeltu tapaa Chrome-sovelluksen luominen. Olemme korostaa Azure ilmoituksen porttiin erityisesti liittyviä ohjeita. 

>[AZURE.NOTE] On suositeltavaa ladata lähde- [Chrome App ilmoituksen keskittimeen otoksen]Chrome sovelluksen.

Chrome App luodaan JavaScript kautta, ja voit käyttää mitään ensisijainen word editors luomiseen sen. Alla on miltä Chrome sovelluksen näyttää.

![Google Chrome App][15]

1. Luo kansio ja anna sille nimi `ChromePushApp`. Nimi on haluamaansa – Jos nimeät sen jotain muuta, varmista, että haluat korvata polku tarvittavat koodi-osioon.

2. Lataa [crypto js kirjaston] toisessa vaiheessa luomasi kansion. Tämä kirjasto-kansio sisältää kaksi alikansiot: `components` ja `rollups`.

3. Luo `manifest.json` tiedosto. Tiedostojen tiedosto, joka sisältää sovelluksen metatiedot ja useimmat varmuuskopioidaan kaikki Chrome-sovellusta, kaikki käyttöoikeudet, jotka myönnetään-sovellukseen, kun käyttäjä asentaa sen.

        {
          "name": "NH-GCM Notifications",
          "description": "Chrome platform app.",
          "manifest_version": 2,
          "version": "0.1",
          "app": {
            "background": {
              "scripts": ["background.js"]
            }
          },
          "permissions": ["gcm", "storage", "notifications", "https://*.servicebus.windows.net/*"],
          "icons": { "128": "gcm_128.png" }
        }

    Ilmoitus `permissions` elementti, joka määrittää, että tämä Chrome App voivat push-ilmoitusten vastaanottaminen GCM. Siinä on myös määritettävä missä Chrome App tekee muut puhelun rekisteröidä Azure-ilmoituksen keskittimet URI.
    Esimerkki Microsoftin ‑sovellus käyttää myös äänitiedoston kuvakkeen `gcm_128.png`, löytyvät tietolähde, jota käytetään uudelleen alkuperäiseen GCM otosten-palvelussa. Voit korvata sen kuvan, joka sopii [kuvake ehdot](https://developer.chrome.com/apps/manifest/icons).

4. Luo tiedosto nimeltä `background.js` se seuraavalla koodilla:

        // Returns a new notification ID used in the notification.
        function getNotificationId() {
          var id = Math.floor(Math.random() * 9007199254740992) + 1;
          return id.toString();
        }

        function messageReceived(message) {
          // A message is an object with a data property that
          // consists of key-value pairs.

          // Concatenate all key-value pairs to form a display string.
          var messageString = "";
          for (var key in message.data) {
            if (messageString != "")
              messageString += ", "
            messageString += key + ":" + message.data[key];
          }
          console.log("Message received: " + messageString);

          // Pop up a notification to show the GCM message.
          chrome.notifications.create(getNotificationId(), {
            title: 'GCM Message',
            iconUrl: 'gcm_128.png',
            type: 'basic',
            message: messageString
          }, function() {});
        }

        var registerWindowCreated = false;

        function firstTimeRegistration() {
          chrome.storage.local.get("registered", function(result) {

            registerWindowCreated = true;
            chrome.app.window.create(
              "register.html",
              {  width: 520,
                 height: 500,
                 frame: 'chrome'
              },
              function(appWin) {}
            );
          });
        }

        // Set up a listener for GCM message event.
        chrome.gcm.onMessage.addListener(messageReceived);

        // Set up listeners to trigger the first-time registration.
        chrome.runtime.onInstalled.addListener(firstTimeRegistration);
        chrome.runtime.onStartup.addListener(firstTimeRegistration);

    Tämä on tiedosto, joka tulee Chrome-sovellusikkunan HTML (**register.html**) ja määrittää myös käsittelijä **messageReceived** käsittelemään saapuvia push-ilmoitus.

5. Luo tiedosto nimeltä `register.html` – Tämä määrittää Chrome-sovelluksen Käyttöliittymässä. 

   >[AZURE.NOTE] Tässä esimerkissä käytetään **CryptoJS v3.1.2**. Jos latasit toisen version kirjastoon, varmista, että haluat korvata oikein versio `src` polku.

        <html>

        <head>
        <title>GCM Registration</title>
        <script src="register.js"></script>
        <script src="CryptoJS v3.1.2/rollups/hmac-sha256.js"></script>
        <script src="CryptoJS v3.1.2/components/enc-base64-min.js"></script>
        </head>

        <body>

        Sender ID:<br/><input id="senderId" type="TEXT" size="20"><br/>
        <button id="registerWithGCM">Register with GCM</button>
        <br/>
        <br/>
        <br/>

        Notification Hub Name:<br/><input id="hubName" type="TEXT" style="width:400px"><br/><br/>
        Connection String:<br/><textarea id="connectionString" type="TEXT" style="width:400px;height:60px"></textarea>

        <br/>

        <button id="registerWithNH" disabled="true">Register with Azure Notification Hubs</button>

        <br/>
        <br/>

        <textarea id="console" type="TEXT" readonly style="width:500px;height:200px;background-color:#e5e5e5;padding:5px"></textarea>

        </body>

        </html>

6. Luo tiedosto nimeltä `register.js` koodilla. Tämä tiedosto määrittää takana komentosarja `register.html`. Chrome-sovellukset eivät salli tekstiin suorittamisen, joten sinun on luotava erillinen varmuuskopioiminen komentosarjan, että Käyttöliittymän.

        var registrationId = "";
        var hubName        = "", connectionString = "";
        var originalUri    = "", targetUri = "", endpoint = "", sasKeyName = "", sasKeyValue = "", sasToken = "";

        window.onload = function() {
           document.getElementById("registerWithGCM").onclick = registerWithGCM;  
           document.getElementById("registerWithNH").onclick = registerWithNH;
           updateLog("You have not registered yet. Please provider sender ID and register with GCM and then with  Notification Hubs.");
        }

        function updateLog(status) {
          currentStatus = document.getElementById("console").innerHTML;
          if (currentStatus != "") {
            currentStatus = currentStatus + "\n\n";
          }

          document.getElementById("console").innerHTML = currentStatus  + status;
        }

        function registerWithGCM() {
          var senderId = document.getElementById("senderId").value.trim();
          chrome.gcm.register([senderId], registerCallback);

          // Prevent register button from being clicked again before the registration finishes.
          document.getElementById("registerWithGCM").disabled = true;
        }

        function registerCallback(regId) {
          registrationId = regId;
          document.getElementById("registerWithGCM").disabled = false;

          if (chrome.runtime.lastError) {
            // When the registration fails, handle the error and retry the
            // registration later.
            updateLog("Registration failed: " + chrome.runtime.lastError.message);
            return;
          }

          updateLog("Registration with GCM succeeded.");
          document.getElementById("registerWithNH").disabled = false;

          // Mark that the first-time registration is done.
          chrome.storage.local.set({registered: true});
        }

        function registerWithNH() {
          hubName = document.getElementById("hubName").value.trim();
          connectionString = document.getElementById("connectionString").value.trim();
          splitConnectionString();
          generateSaSToken();
          sendNHRegistrationRequest();
        }

        // From http://msdn.microsoft.com/library/dn495627.aspx
        function splitConnectionString()
        {
          var parts = connectionString.split(';');
          if (parts.length != 3)
          throw "Error parsing connection string";

          parts.forEach(function(part) {
            if (part.indexOf('Endpoint') == 0) {
            endpoint = 'https' + part.substring(11);
            } else if (part.indexOf('SharedAccessKeyName') == 0) {
            sasKeyName = part.substring(20);
            } else if (part.indexOf('SharedAccessKey') == 0) {
            sasKeyValue = part.substring(16);
            }
          });

          originalUri = endpoint + hubName;
        }

        function generateSaSToken()
        {
          targetUri = encodeURIComponent(originalUri.toLowerCase()).toLowerCase();
          var expiresInMins = 10; // 10 minute expiration

          // Set expiration in seconds.
          var expireOnDate = new Date();
          expireOnDate.setMinutes(expireOnDate.getMinutes() + expiresInMins);
          var expires = Date.UTC(expireOnDate.getUTCFullYear(), expireOnDate
            .getUTCMonth(), expireOnDate.getUTCDate(), expireOnDate
            .getUTCHours(), expireOnDate.getUTCMinutes(), expireOnDate
            .getUTCSeconds()) / 1000;
          var tosign = targetUri + '\n' + expires;

          // Using CryptoJS.
          var signature = CryptoJS.HmacSHA256(tosign, sasKeyValue);
          var base64signature = signature.toString(CryptoJS.enc.Base64);
          var base64UriEncoded = encodeURIComponent(base64signature);

          // Construct authorization string.
          sasToken = "SharedAccessSignature sr=" + targetUri + "&sig="
                          + base64UriEncoded + "&se=" + expires + "&skn=" + sasKeyName;
        }

        function sendNHRegistrationRequest()
        {
          var registrationPayload =
          "<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
          "<entry xmlns=\"http://www.w3.org/2005/Atom\">" +
              "<content type=\"application/xml\">" +
                  "<GcmRegistrationDescription xmlns:i=\"http://www.w3.org/2001/XMLSchema-instance\" xmlns=\"http://schemas.microsoft.com/netservices/2010/10/servicebus/connect\">" +
                      "<GcmRegistrationId>{GCMRegistrationId}</GcmRegistrationId>" +
                  "</GcmRegistrationDescription>" +
              "</content>" +
          "</entry>";

          // Update the payload with the registration ID obtained earlier.
          registrationPayload = registrationPayload.replace("{GCMRegistrationId}", registrationId);

          var url = originalUri + "/registrations/?api-version=2014-09";
          var client = new XMLHttpRequest();

          client.onload = function () {
            if (client.readyState == 4) {
              if (client.status == 200) {
                updateLog("Notification Hub Registration succesful!");
                updateLog(client.responseText);
              } else {
                updateLog("Notification Hub Registration did not succeed!");
                updateLog("HTTP Status: " + client.status + " : " + client.statusText);
                updateLog("HTTP Response: " + "\n" + client.responseText);
              }
            }
          };

          client.onerror = function () {
                updateLog("ERROR - Notification Hub Registration did not succeed!");
          }

          client.open("POST", url, true);
          client.setRequestHeader("Content-Type", "application/atom+xml;type=entry;charset=utf-8");
          client.setRequestHeader("Authorization", sasToken);
          client.setRequestHeader("x-ms-version", "2014-09");

          try {
              client.send(registrationPayload);
          }
          catch(err) {
              updateLog(err.message);
          }
        }

    Yllä komentosarja on seuraavassa tärkeimmät parametrit:
    - **window.OnLoad** määrittää painikkeen napsautuksella tapahtumat kaksi painiketta Käyttöliittymässä. Yksi Rekisteröi GCM ja toinen käyttää Rekisteröintitunnusta, joka palautetaan kanssa GCM Azure ilmoituksen keskittimet rekisteröitymään rekisteröinnin jälkeen.
    - **updateLog** on toiminto, joka pystyy käsittelemään yksinkertainen kirjaaminen ominaisuuksia.
    - **registerWithGCM** on ensimmäinen painike napsauttamalla käsittelijä, joka tekee `chrome.gcm.register` kutsu GCM rekisteröidä Chrome App nykyisen esiintymän.
    - **registerCallback** on takaisinkutsutoiminto, jota kutsutaan, kun GCM rekisteröinti kutsu palauttaa.
    - **registerWithNH** on painiketta napsauttamalla toinen käsittelytoiminto, jonka ilmoituksen keskittimet Rekisteröi. Se saa `hubName` ja `connectionString` (joka on määritetty käyttäjälle) ja crafts ilmoituksen keskittimet rekisteröinti REST API-kutsu.
    - **splitConnectionString** ja **generateSaSToken** ovat apuohjelmia, jotka edustavat SaS suojaustunnuksen luomisen yhteydessä, jota käytetään kaikissa REST API-puheluissa JavaScript-käyttöönotto. Lisätietoja on artikkelissa [Yleisiä käsitteitä](http://msdn.microsoft.com/library/dn495627.aspx).
    - **sendNHRegistrationRequest** on toiminto, joka tekee Soita HTTP-REST Azure ilmoituksen porttiin.
    - **registrationPayload** määrittää rekisteröinti XML-tiedot. Lisätietoja on artikkelissa [Luo rekisteröinti NH REST API]. Päivitämme Tunnuksella ei ja mikä on saatu GCM.
    - **asiakas** on **XMLHttpRequest** , jonka avulla HTTP POST pyydettävä esiintymä. Huomaa, että Microsoft update `Authorization` otsikkoon `sasToken`. Puhelun onnistumiseen Rekisteröi Chrome App tämä esiintymä Azure ilmoituksen keskittimet.


Yleisen kansiorakenteen, projektin pitäisi muistuttaa:     ![Google Chrome App – kansiorakenne][21]

###<a name="set-up-and-test-your-chrome-app"></a>Määrittäminen ja testaus Chrome App

1. Avaa Chrome-selaimessa. Avaa **Chrome-laajennukset** ja aktivoi **kehittäjätilassa**.

    ![Google Chrome - Ota kehittäjätilassa][16]

2. Valitse **Lataa purettu tunniste** ja Selaa kansioon, jossa tiedostot luotiin. Voit myös halutessasi **Chrome-sovellukset ja laajennukset-kehitystyökalun**. Tämä työkalu on Chrome sovellus sellaisenaan (Chrome Web Storesta asennettu) sekä annetaan muistin lisäominaisuuksia Chrome App kehittäminen.

    ![Google Chrome - ladata purettu tunniste][17]

3. Jos Chrome-sovellus on luotu ilman virheitä, sinun tulee näkyviin Chrome-sovelluksesi näkyviin.

    ![Google Chrome - Chrome App-näyttö][18]

4. Kirjoita **Projektinumero** , jonka olet saanut aiemmin **Google Cloud konsolin** lähettäjä-ID ja valitse **rekisteröidä GCM**. Sanoma tulee näkyviin **rekisteröinti GCM onnistui.**

    ![Google Chrome - Chrome App mukauttaminen][19]

5. Kirjoita **Ilmoituksen keskittimeen nimi** ja **DefaultListenSharedAccessSignature** , joka saadaan-portaalista aiemmin ja sitten **Azure ilmoituksen toiminnossa voit rekisteröidä**. Sanoma tulee näkyviin **ilmoituksen keskittimeen rekisteröinnin onnistuneen!** ja rekisteröinti-vastauksen, joka sisältää Azure ilmoituksen keskittimet rekisteröinnin tietoja tunnus.

    ![Google Chrome - Määritä ilmoituksen keskittimeen tiedot][20]  

##<a name="send"></a>Lähetä ilmoitus Chrome-sovellukseen

Testausta varten lähetämme konsoli Chrome push-ilmoitukset käyttämällä .NET-sovellus. 

>[AZURE.NOTE] Voit lähettää push-ilmoitukset ilmoituksen keskittimet minkä tahansa taustasta Microsoftin julkisessa <a href="http://msdn.microsoft.com/library/windowsazure/dn223264.aspx">REST-liittymän</a>kautta. Tutustu Microsoftin [ohjeissa portal](https://azure.microsoft.com/documentation/services/notification-hubs/) Office kaikissa ympäristöissä esimerkkejä.

1. Valitse Visual Studion **Tiedosto** -valikosta **Uusi** ja valitsemalla sitten **Projekti**. **Visual C#** **Windows** ja **Console-sovellus**ja valitse sitten **OK**.  Tämä luo uuden konsolin sovelluksen projektin.

2. Valitse **Työkalut** -valikossa **Kirjaston pakettien hallinta** ja valitse sitten **Paketin hallinta-konsolin**. Tämä näyttää pakkauksen hallinta-konsolin.

3. Konsoli-ikkunassa Suorita seuraava komento:

        Install-Package Microsoft.Azure.NotificationHubs

    Tämä lisää Azure palvelun Bus SDK <a href="http://nuget.org/packages/  WindowsAzure.ServiceBus/">WindowsAzure.ServiceBus NuGet paketin</a>viittaus.

4. Avaa `Program.cs` ja lisää seuraava `using` lause:

        using Microsoft.Azure.NotificationHubs;

5. Valitse `Program` luokan, Lisää seuraavalla tavalla:

        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            String message = "{\"data\":{\"message\":\"Hello Chrome from Azure Notification Hubs\"}}";
            await hub.SendGcmNativeNotificationAsync(message);
        }

    Varmista, että korvaa `<hub name>` paikkamerkki ja nimi, joka tulee näkyviin ilmoitus-toiminnossa sivu [portal](https://portal.azure.com) -ilmoitus-toiminnossa. Korvaa myös yhteyden merkkijonon paikkamerkki kutsutaan yhteysmerkkijonon `DefaultFullSharedAccessSignature` ilmoituksen keskittimeen määritykset-osasta saamasi.

    >[AZURE.NOTE] Varmista, että yhteysmerkkijonoa käyttämällä **täydet** oikeudet, ei **Kuuntele** access. **Kuuntele** access-yhteysmerkkijonon Myönnä käyttöoikeuksia push-ilmoitukset lähetetään.

5. Lisää seuraavat puhelut `Main` menetelmää:

         SendNotificationAsync();
         Console.ReadLine();
         
6. Varmista, että Chrome on käynnissä ja console-sovelluksen käyttämiseen.

7. Pitäisi näkyä seuraava ilmoitus ponnahdusikkuna työpöydälle.

    ![Google Chrome - ilmoitus][13]

8. Voit tarkistaa kaikki ilmoitukset myös käyttämällä tehtäväpalkissa Chrome ilmoitukset-ikkunassa (Windows) kun Chrome on käynnissä.

    ![Google Chrome - ilmoitukset-luettelo][14]

>[AZURE.NOTE] Sinun ei tarvitse Chrome App suorittaminen tai avoinna selaimessa (mutta Chrome-selaimessa itse on oltava käynnissä). Saat myös kootut haluat nähdä kaikki ilmoitukset ilmoitukset Chrome-ikkunassa.

## <a name="next-steps"> </a>Seuraavat vaiheet

Lisätietoja ilmoituksen keskittimet [ilmoituksen keskittimet]yleiskatsaus.

Voit kohdistaa tietyille käyttäjille viitata [Azure ilmoituksen keskittimet Ilmoita käyttäjille] opetusohjelman. 

Jos haluat määritetään käyttäjien korko ryhmiin, voit noudattaa [Azure ilmoituksen keskittimet uutisia] opetusohjelman.

<!-- Images. -->
[1]: ./media/notification-hubs-chrome-get-started/GoogleConsoleCreateProject.PNG
[2]: ./media/notification-hubs-chrome-get-started/GoogleProjectNumber.png
[3]: ./media/notification-hubs-chrome-get-started/EnableGCM.png
[4]: ./media/notification-hubs-chrome-get-started/CreateServerKey.png
[5]: ./media/notification-hubs-chrome-get-started/ServerKey.png
[6]: ./media/notification-hubs-chrome-get-started/CreateNH.png
[7]: ./media/notification-hubs-chrome-get-started/NHNamespace.png
[8]: ./media/notification-hubs-chrome-get-started/NamespaceConfigure.png
[9]: ./media/notification-hubs-chrome-get-started/NHConfigure.png
[10]: ./media/notification-hubs-chrome-get-started/NHConfigureGCM.png
[11]: ./media/notification-hubs-chrome-get-started/NHDashboard.png
[12]: ./media/notification-hubs-chrome-get-started/NHConnString.png
[13]: ./media/notification-hubs-chrome-get-started/ChromeNotification.png
[14]: ./media/notification-hubs-chrome-get-started/ChromeNotificationWindow.png
[15]: ./media/notification-hubs-chrome-get-started/ChromeApp.png
[16]: ./media/notification-hubs-chrome-get-started/ChromeExtensions.png
[17]: ./media/notification-hubs-chrome-get-started/ChromeLoadExtension.png
[18]: ./media/notification-hubs-chrome-get-started/ChromeAppLoaded.png
[19]: ./media/notification-hubs-chrome-get-started/ChromeAppGCM.png
[20]: ./media/notification-hubs-chrome-get-started/ChromeAppNH.png
[21]: ./media/notification-hubs-chrome-get-started/FinalFolderView.png

<!-- URLs. -->
[Chrome App ilmoituksen keskittimeen malli]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/PushToChromeApps
[Google Cloud konsoli]: http://cloud.google.com/console
[Azure Classic Portal]: https://manage.windowsazure.com/
[Ilmoitus keskittimet yleiskatsaus]: notification-hubs-push-notification-overview.md
[Chrome-sovellusten yleiskatsaus]: https://developer.chrome.com/apps/about_apps
[Chrome App GCM malli]: https://github.com/GoogleChrome/chrome-app-samples/tree/master/samples/gcm-notifications
[Installable Web Apps]: https://developers.google.com/chrome/apps/docs/
[Chrome-sovellusten Mobile]: https://developer.chrome.com/apps/chrome_apps_on_mobile
[Luo rekisteröinti NH REST-Ohjelmointirajapinnalla]: http://msdn.microsoft.com/library/azure/dn223265.aspx
[salauksen js-kirjasto]: http://code.google.com/p/crypto-js/
[GCM with Chrome Apps]: https://developer.chrome.com/apps/cloudMessaging
[Google Cloud Messaging Chromessa]: https://developer.chrome.com/apps/cloudMessagingV1
[Azure ilmoituksen keskittimet Ilmoita käyttäjille]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[Uutisia Azure ilmoituksen keskittimet]: notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md
