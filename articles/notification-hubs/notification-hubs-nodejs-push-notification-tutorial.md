<properties
    pageTitle="Push-ilmoitusten Azure ilmoituksen keskittimet ja Node.js lähettäminen"
    description="Opettele käyttämään ilmoituksen keskittimet push-ilmoitukset lähetetään Node.js-sovelluksesta."
    keywords="Push-ilmoitus, push notifications,node.js push ios push"
    services="notification-hubs"
    documentationCenter="nodejs"
    authors="ysxu"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="yuaxu"/>

# <a name="sending-push-notifications-with-azure-notification-hubs-and-nodejs"></a>Push-ilmoitusten Azure ilmoituksen keskittimet ja Node.js lähettäminen
[AZURE.INCLUDE [notification-hubs-backend-how-to-selector](../../includes/notification-hubs-backend-how-to-selector.md)]

##<a name="overview"></a>Yleiskatsaus

> [AZURE.IMPORTANT] Jotta voit suorittaa tässä opetusohjelmassa, on oltava aktiivinen Azure-tili. Jos sinulla ei ole tiliä, voit luoda ilmainen kokeiluversio tili vain muutaman minuutin. Lisätietoja on artikkelissa [Azure maksuttoman kokeiluversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-nodejs-how-to-use-notification-hubs).

Tässä oppaassa kerrotaan, kuinka lähettämään suoraan Node.js sovelluksen push-ilmoitukset ja Azure ilmoituksen keskittimet avulla. 

Tilanteita, joissa kattaa ovat push-ilmoitukset lähetetään sovellusten seuraavissa ympäristöissä:

* Android-laitteeseen
* iOS
* Windows Phone
* Yleinen Windows-ympäristössä 

Lisätietoja ilmoituksen keskittimet on kohdassa [Vaiheisiin](#next) .

##<a name="what-are-notification-hubs"></a>Mitkä ovat ilmoituksen keskittimet?

Azure ilmoituksen keskittimet avulla voit helposti käytettävällä, ympäristön, skaalattava infrastruktuurin push-ilmoitukset lähetetään mobiililaitteisiin. Lisätietoja palvelu-infrastruktuuria on artikkelissa [Azure ilmoituksen keskittimet](http://msdn.microsoft.com/library/windowsazure/jj927170.aspx) -sivu.

##<a name="create-a-nodejs-application"></a>Node.js-sovelluksen luominen

Tässä opetusohjelmassa ensimmäiseksi Luo uusi tyhjä Node.js-sovellus. Ohjeita Node.js sovelluksen luomisesta on artikkelissa [Luo ja ota käyttöön Node.js-sovelluksen avulla Azure sivuston][nodejswebsite], [Node.js pilvipalvelussa] [ Node.js Cloud Service] Windows PowerShellin tai [-sivuston kanssa WebMatrix].

##<a name="configure-your-application-to-use-notification-hubs"></a>Määrittää sovelluksesi käyttämään ilmoituksen keskittimet

Käyttämään Azure ilmoituksen keskittimet, sinun on ladattava ja Node.js [azure-paketti](https://www.npmjs.com/package/azure), joka sisältää valmiita helper kirjastoja, joissa viestintä push ilmoituksen muiden palveluiden kanssa.

### <a name="use-node-package-manager-npm-to-obtain-the-package"></a>Hanki paketin solmu paketin hallinta (NPM) avulla

1.  Käytä esimerkiksi **PowerShell** (Windows), **terminaalissa** (Mac) tai (Linux) **Bash** käyttöliittymä ja Selaa kansioon, jossa tyhjä sovellus on luotu.

2.  Kirjoita **npm asentaa azure sb** komentorivi-ikkuna.

3.  Voit suorittaa manuaalisesti, varmista, että **ls** tai **dir** -komento **solmu\_moduulit** kansio on luotu. Kyseiseen kansioon Etsi **azure** -paketti, joka sisältää kirjastoja, sinun täytyy käyttää ilmoitus-toiminnossa.

>[AZURE.NOTE] Saat lisätietoja asentamisesta NPM virallinen [NPM blogiin](http://blog.npmjs.org/post/85484771375/how-to-install-npm). 

### <a name="import-the-module"></a>Tuo moduuli

Sovelluksen **server.js** -tiedoston ylhäältä tekstieditorissa, Lisää seuraava:

    var azure = require('azure');

### <a name="setup-an-azure-notification-hub-connection"></a>Azure ilmoituksen keskittimeen-yhteyden määrittäminen

**NotificationHubService** objektin voit käsitellä ilmoituksen keskittimet. Seuraava koodi Luo nimetty **hubname**nofication-toiminnossa **NotificationHubService** -objekti. Lisää tietue **server.js** , tiedoston yläosassa tuo azure moduuli-lausekkeen jälkeen:

    var notificationHubService = azure.createNotificationHubService('hubname','connectionstring');

Yhteyden **connectionstring** arvo saadaan [Azure Portal] suorittamalla seuraavat vaiheet:

1. Valitse vasemmassa siirtymisruudussa valitsemalla **Selaa**.

2. Valitse **Ilmoitus keskittimet**ja Etsi keskittimeen, jota haluat käyttää näytteen. Voit viitata [Windows Store aloittaminen opetusohjelma](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) Jos tarvitset apua uusi ilmoitus-toiminnossa.

3. Valitse **asetukset**.

4. Napsauta **käytäntöjen**. Näet sekä käyttää jaettua ja koko yhteyden merkkijonoja.

![Azure Portal - ilmoituksen keskittimet](./media/notification-hubs-nodejs-how-to-use-notification-hubs/notification-hubs-portal.png)

> [AZURE.NOTE] Voit myös hakea yhteysmerkkijonoa [Azure käyttöliittymä (Azure CLI)](../xplat-cli-install.md) [PowerShellin Azure](../powershell-install-configure.md) tai **azure sb nimitilan Näytä** -komento mukana **Get-AzureSbNamespace** cmdlet-komennolla.

##<a name="general-architecture"></a>Yleiset arkkitehtuuri

**NotificationHubService** objektin paljastaa seuraavat objektin esiintymät push-ilmoitukset lähetetään määrättyjä laitteita ja sovelluksia:

* **Android** - **GcmService** -objekti, joka on saatavilla kohdassa **notificationHubService.gcm** käyttäminen
* **iOS** - **ApnsService** -objekti, joka on käytettävissä osoitteessa **notificationHubService.apns** käyttäminen
* **Windows Phone** - **MpnsService** -objekti, joka on saatavilla kohdassa **notificationHubService.mpns** käyttäminen
* **Yleinen Windows-ympäristön** - **WnsService** -objekti, joka on saatavilla kohdassa **notificationHubService.wns** käyttäminen

### <a name="how-to-send-push-notifications-to-android-applications"></a>Toimintaohje: Lähetä push-ilmoitukset Android-sovellukset

**GcmService** -objekti sisältää **Lähetä** menetelmä, jota voidaan käyttää Android-sovellukset push-ilmoitukset lähetetään. **Lähetä** -menetelmä hyväksyy seuraavat parametrit:

* **Tunnisteet** - tunniste-tunniste. Jos tunnistetta ei ole annettu, ilmoitus lähetetään al asiakkaille.
* **Tiedot** - viestin JSON tai raaka merkkijonon tiedot.
* **Takaisinsoitto** - takaisinsoitto-funktiota.

Katso lisätietoja paketti-muodon, [Käyttöönoton GCM palvelimen](http://developer.android.com/google/gcm/server.html#payload) asiakirjan **tiedot** -osaan.

Seuraava koodi käyttää **NotificationHubService** näyttämiä **GcmService** esiintymän push ilmoituksen lähettäminen kaikille rekisteröityä asiakkaille.

    var payload = {
      data: {
        message: 'Hello!'
      }
    };
    notificationHubService.gcm.send(null, payload, function(error){
      if(!error){
        //notification sent
      }
    });

### <a name="how-to-send-push-notifications-to-ios-applications"></a>Toimintaohje: Lähetä push-ilmoitukset iOS-sovellukset

Yllä olevien ohjeiden Android-sovellusten kanssa samalla **ApnsService** objekti sisältää **Lähetä** menetelmä, jonka avulla voidaan lähettää push-ilmoitukset iOS-sovellukset. **Lähetä** -menetelmä hyväksyy seuraavat parametrit:

* **Tunnisteet** - tunniste-tunniste. Jos tunnistetta ei ole annettu, kaikkiin lähetetään ilmoitus.
* **Tiedot** - viestin JSON tai merkkijonon tiedot.
* **Takaisinsoitto** - takaisinsoitto-funktiota.

Katso lisätietoja paketti-muodossa, [paikallisen ja Push-ilmoituksen Programming Guide](http://developer.apple.com/library/ios/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/ApplePushService/ApplePushService.html) -artikkelin **Ilmoituksen tiedot** -osio.

Seuraava koodi käyttää **NotificationHubService** näyttämiä **ApnsService** esiintymän ilmoitussanoma lähettäminen kaikkiin:

    var payload={
        alert: 'Hello!'
      };
    notificationHubService.apns.send(null, payload, function(error){
      if(!error){
        // notification sent
      }
    });

### <a name="how-to-send-push-notifications-to-windows-phone-applications"></a>Toimintaohje: Lähetä push-ilmoitukset Windows Phone-sovellukset

**MpnsService** -objekti sisältää **Lähetä** menetelmä, jota voidaan käyttää push-ilmoitusten lähettäminen Windows Phone-sovellukset. **Lähetä** -menetelmä hyväksyy seuraavat parametrit:

* **Tunnisteet** - tunniste-tunniste. Jos tunnistetta ei ole annettu, kaikkiin lähetetään ilmoitus.
* **Tiedot** - viestin XML-tiedot.
* **TargetName**  -  `toast` ilmoitukseen ilmoituksia. `token`ruutu-ilmoituksia.
* **NotificationClass** - ilmoituksen prioriteetti. [Push-ilmoitukset palvelimesta](http://msdn.microsoft.com/library/hh221551.aspx) asiakirjan kelvolliset arvot kohdassa **HTTP ylätunnisteen osat** .
* **Asetukset** - valinnaisia otsikot.
* **Takaisinsoitto** - takaisinsoitto-funktiota.

Kelvollinen **TargetName**, **NotificationClass** ja otsikon vaihtoehtojen luetteloon Tutustu [Push-ilmoitukset-palvelimesta](http://msdn.microsoft.com/library/hh221551.aspx) -sivu.

Seuraava näyte koodi käyttää **NotificationHubService** näyttämiä **MpnsService** esiintymän lähettää push ilmoitusruudun:

    var payload = '<?xml version="1.0" encoding="utf-8"?><wp:Notification xmlns:wp="WPNotification"><wp:Toast><wp:Text1>string</wp:Text1><wp:Text2>string</wp:Text2></wp:Toast></wp:Notification>';
    notificationHubService.mpns.send(null, payload, 'toast', 22, function(error){
      if(!error){
        //notification sent
      }
    });

### <a name="how-to-send-push-notifications-to-universal-windows-platform-uwp-applications"></a>Toimintaohje: Lähetä push-ilmoitukset yleinen Windows Platform (UWP)-sovellukset

**WnsService** -objekti sisältää **Lähetä** menetelmä, jonka avulla voidaan lähettää yleinen Windows-ympäristön sovellusten push-ilmoitukset.  **Lähetä** -menetelmä hyväksyy seuraavat parametrit:

* **Tunnisteet** - tunniste-tunniste. Jos tunnistetta ei ole annettu, ilmoituksessa, joka lähetetään kaikille rekisteröityä asiakkaille.
* **Tiedot** - sanoman XML-tiedot.
* **Tyyppi** - ilmoitustyypin.
* **Asetukset** - valinnaisia otsikot.
* **Takaisinsoitto** - takaisinsoitto-funktiota.

Katso luettelo Kelvolliset tyypit ja pyynnön otsikot on [Push-ilmoituksen palvelun pyynnön ja vastauksen ylä](http://msdn.microsoft.com/library/windows/apps/hh465435.aspx).

Seuraava koodi käyttää **NotificationHubService** näyttämiä **WnsService** esiintymän push ilmoitusruudun lähettäminen UWP sovelluksen:

    var payload = '<toast><visual><binding template="ToastText01"><text id="1">Hello!</text></binding></visual></toast>';
    notificationHubService.wns.send(null, payload , 'wns/toast', function(error){
      if(!error){
        // notification sent
      }
    });

## <a name="next-steps"></a>Seuraavat vaiheet

Yllä otoksen katkelmat avulla voit helposti luoda palvelun infrastruktuurin push-ilmoitukset toimitetaan monenlaisia laitteita. Nyt kun olet oppinut perusteet ilmoituksen keskittimet käyttäminen node.js, näistä linkeistä saat lisätietoja siitä, miten voit laajentaa seuraavia ominaisuuksia.

-   Saat [Azure ilmoituksen keskittimet](https://msdn.microsoft.com/library/azure/jj927170.aspx)MSDN-viittaus.
-   Lisätietoja [Azure SDK solmun] säilö GitHub näytteiden ja säännöt.

  [Solmun Azure SDK]: https://github.com/WindowsAzure/azure-sdk-for-node
  [Next Steps]: #nextsteps
  [What are Service Bus Topics and Subscriptions?]: #what-are-service-bus-topics
  [Create a Service Namespace]: #create-a-service-namespace
  [Obtain the Default Management Credentials for the Namespace]: #obtain-default-credentials
  [Create a Node.js Application]: #Create_a_Nodejs_Application
  [Configure Your Application to Use Service Bus]: #Configure_Your_Application_to_Use_Service_Bus
  [How to: Create a Topic]: #How_to_Create_a_Topic
  [How to: Create Subscriptions]: #How_to_Create_Subscriptions
  [How to: Send Messages to a Topic]: #How_to_Send_Messages_to_a_Topic
  [How to: Receive Messages from a Subscription]: #How_to_Receive_Messages_from_a_Subscription
  [How to: Handle Application Crashes and Unreadable Messages]: #How_to_Handle_Application_Crashes_and_Unreadable_Messages
  [How to: Delete Topics and Subscriptions]: #How_to_Delete_Topics_and_Subscriptions
  [1]: #Next_Steps
  [Topic Concepts]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-topics-01.png
  [Azure Classic Portal]: http://manage.windowsazure.com
  [image]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-03.png
  [2]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-04.png
  [3]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-05.png
  [4]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-06.png
  [5]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-07.png
  [SqlFilter.SqlExpression]: http://msdn.microsoft.com/library/windowsazure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx
  [Azure Service Bus Notification Hubs]: http://msdn.microsoft.com/library/windowsazure/jj927170.aspx
  [SqlFilter]: http://msdn.microsoft.com/library/windowsazure/microsoft.servicebus.messaging.sqlfilter.aspx
  [Web-sivuston WebMatrix]: /develop/nodejs/tutorials/web-site-with-webmatrix/
  [Node.js Cloud Service]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
[Previous Management Portal]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/previous-portal.png
  [nodejswebsite]: /develop/nodejs/tutorials/create-a-website-(mac)/
  [Node.js Cloud Service with Storage]: /develop/nodejs/tutorials/web-app-with-storage/
  [Node.js Web Application with Storage]: /develop/nodejs/tutorials/web-site-with-storage/
  [Azure Portal]: https://portal.azure.com
