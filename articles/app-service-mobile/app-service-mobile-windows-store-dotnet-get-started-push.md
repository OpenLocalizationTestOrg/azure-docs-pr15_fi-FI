<properties
    pageTitle="Push-ilmoitukset lisääminen yleinen Windows Platform (UWP)-sovellukseen | Azure-mobiilisovellukset"
    description="Opettele käyttämään Azure palvelun mobiilisovellukset ja Azure ilmoituksen keskittimet push-ilmoitukset lähetetään yleinen Windows Platform (UWP)-sovellukseen."
    services="app-service\mobile,notification-hubs"
    documentationCenter="windows"
    authors="ysxu"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/12/2016"
    ms.author="yuaxu"/>

# <a name="add-push-notifications-to-your-windows-app"></a>Push-ilmoitukset lisääminen Windows-sovellus

[AZURE.INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

##<a name="overview"></a>Yleiskatsaus

Tässä opetusohjelmassa Lisää push-ilmoitukset [nopeasti Windowsin Käynnistä](app-service-mobile-windows-store-dotnet-get-started.md) -projektin niin, että push-ilmoitus lähetetään laitteen aina, kun tietue lisätään.

Jos et käytä ladatut pikaopas server-projekti, sinun on push ilmoituksen tunniste-paketti. Lisätietoja on kohdassa [.NET palvelimeen Azure-mobiilisovellukset SDK käsitteleminen](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) .

##<a name="configure-hub"></a>Määritä ilmoitus-toiminnossa

[AZURE.INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

##<a name="register-your-app-for-push-notifications"></a>Rekisteröi sovelluksen push-ilmoitukset

Haluat lähettää kun Windows-kaupan sovellus ja valitse Määritä palvelimen projektin integroiminen kanssa Windows ilmoituksen Services (WNS) lähettää push.

1. Napsauta hiiren kakkospainikkeella Visual Studio ratkaisunhallinnassa UWP sovelluksen projektin, valitse **Tallenna** > **Liittää App Store...**. 

    ![Windows-kaupan sovelluksen liittäminen](./media/app-service-mobile-windows-store-dotnet-get-started-push/notification-hub-associate-uwp-app.png)
    
2. Ohjatun Valitse **Seuraava**, kirjaudu sisään käyttämällä Microsoft-tiliä, kirjoita sovelluksesi nimi **Varaa uuden sovelluksen nimi**ja valitse sitten **Varaa**.

3. Kun sovellus rekisteröinti on onnistunut, uuden sovelluksen nimi, valitse **Seuraava**ja valitse sitten **Liitä**. Tämä lisää tarvittavat Windows-kaupan rekisteröintitietoja sovelluksen luettelo.  

7. Siirry [Windows keskihajonta Center](https://dev.windows.com/en-us/overview), kirjaudu sisään Microsoft-tiliä, valitse uusi sovellus rekisteröinti, **sovellusten**ja sitten Laajenna **palvelut** > **Push-ilmoitukset**. 

8. **Push-ilmoitukset** -sivulla **Microsoft Azure Mobile-palvelut**-kohdasta **Live Services-sivustossa** .

9. Rekisteröinti-sivulla Valitse **sovelluksen tietoja** ja **Paketin SUOJAUSTUNNUS**, avulla seuraavaksi voit määrittää mobiilisovelluksen Taustajärjestelmä arvon muistiin. 

    ![Windows-kaupan sovelluksen liittäminen](./media/app-service-mobile-windows-store-dotnet-get-started-push/app-service-mobile-uwp-app-push-auth.png)

    > [AZURE.IMPORTANT] Asiakkaan salaisuus ja pakkaus SUOJAUSTUNNUS ovat tärkeitä suojausvaltuudet. Älä jaa nämä arvot kenen kanssa tai jakaa ne sovelluksen. **Sovellustunnus** toiminta käytetään Microsoft Account käyttöoikeuksien määrittämiseen.

##<a name="configure-the-backend-to-send-push-notifications"></a>Push-ilmoitukset lähetetään Taustajärjestelmä määrittäminen

[AZURE.INCLUDE [app-service-mobile-configure-wns](../../includes/app-service-mobile-configure-wns.md)]

##<a id="update-service"></a>Päivitä palvelimeen lähetetään push-ilmoitukset

Seuraavia ohjeita, jotka vastaavat Taustajärjestelmä projektityypin&mdash; [.NET taustatietokannan](#dotnet) tai [Node.js Taustajärjestelmä](#nodejs).

### <a name="dotnet"></a>.NET Taustajärjestelmä projektin

1. Visual Studiossa hiiren kakkospainikkeella server project ja **NuGet pakettien hallinta**, Etsi Microsoft.Azure.NotificationHubs ja sitten **Asenna**. Tämä toiminto asentaa ilmoituksen keskittimet asiakas-kirjasto.

2. Laajenna **ohjaimet**, Avaa TodoItemController.cs ja lisää seuraava lauseiden käyttämisestä:

        using System.Collections.Generic;
        using Microsoft.Azure.NotificationHubs;
        using Microsoft.Azure.Mobile.Server.Config;

3. Lisää seuraava koodi **PostTodoItem** -menetelmä **InsertAsync**puhelu jälkeen:

        // Get the settings for the server project.
        HttpConfiguration config = this.Configuration;
        MobileAppSettingsDictionary settings =
            this.Configuration.GetMobileAppSettingsProvider().GetMobileAppSettings();

        // Get the Notification Hubs credentials for the Mobile App.
        string notificationHubName = settings.NotificationHubName;
        string notificationHubConnection = settings
            .Connections[MobileAppSettingsKeys.NotificationHubConnectionString].ConnectionString;

        // Create the notification hub client.
        NotificationHubClient hub = NotificationHubClient
            .CreateClientFromConnectionString(notificationHubConnection, notificationHubName);

        // Define a WNS payload
        var windowsToastPayload = @"<toast><visual><binding template=""ToastText01""><text id=""1"">"
                                + item.Text + @"</text></binding></visual></toast>";
        try
        {
            // Send the push notification.
            var result = await hub.SendWindowsNativeNotificationAsync(windowsToastPayload);

            // Write the success result to the logs.
            config.Services.GetTraceWriter().Info(result.State.ToString());
        }
        catch (System.Exception ex)
        {
            // Write the failure result to the logs.
            config.Services.GetTraceWriter()
                .Error(ex.Message, null, "Push.SendAsync Error");
        }

    Koodi kertoo lähettää push-ilmoituksen, kun uusi kohde on kohdistin ilmoitus-toiminnossa.

4. Jos haluat julkaista server project.

### <a name="nodejs"></a>Node.js Taustajärjestelmä projektin

1. Jos sinulla ei ole vielä lisätty [Lataa pikaopas projektin](app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart) tai muuten [online editorin Azure-portaalissa](app-service-mobile-node-backend-how-to-use-server-sdk.md#online-editor).

2. Korvaa aiemmin luotu koodi todoitem.js tiedoston seuraavasti:

        var azureMobileApps = require('azure-mobile-apps'),
        promises = require('azure-mobile-apps/src/utilities/promises'),
        logger = require('azure-mobile-apps/src/logger');

        var table = azureMobileApps.table();

        table.insert(function (context) {
        // For more information about the Notification Hubs JavaScript SDK,
        // see http://aka.ms/nodejshubs
        logger.info('Running TodoItem.insert');

        // Define the WNS payload that contains the new item Text.
        var payload = "<toast><visual><binding template=\ToastText01\><text id=\"1\">"
                                    + context.item.text + "</text></binding></visual></toast>";

        // Execute the insert.  The insert returns the results as a Promise,
        // Do the push as a post-execute action within the promise flow.
        return context.execute()
            .then(function (results) {
                // Only do the push if configured
                if (context.push) {
                    // Send a WNS native toast notification.
                    context.push.wns.sendToast(null, payload, function (error) {
                        if (error) {
                            logger.error('Error while sending push notification: ', error);
                        } else {
                            logger.info('Push notification sent successfully!');
                        }
                    });
                }
                // Don't forget to return the results from the context.execute()
                return results;
            })
            .catch(function (error) {
                logger.error('Error while running context.execute: ', error);
            });
        });

        module.exports = table;

    Tämä lähettää WNS ilmoitusruudun, joka sisältää item.text, kun uusi todo kohde on lisätty.

2. Kun muokkaat tiedostoa paikallisessa tietokoneessa, julkaista server project.

##<a id="update-app"></a>Sovelluksen lisääminen push-ilmoitukset

Seuraavaksi sovelluksen on rekisteröitävä push-ilmoitukset pysäyttämisestä. Kun todennus on käytössä, varmista, että käyttäjän merkkejä, ennen kuin yrität rekisteröidä push-ilmoitukset.

1. Avaa projektitiedoston **App.xaml.cs** ja lisää seuraava `using` lauseet:

        using System.Threading.Tasks;
        using Windows.Networking.PushNotifications;

2. Lisää **InitNotificationsAsync** menetelmän-määritys on saman tiedoston **sovelluksen** luokkaan:

        private async Task InitNotificationsAsync()
        {
            // Get a channel URI from WNS.
            var channel = await PushNotificationChannelManager
                .CreatePushNotificationChannelForApplicationAsync();

            // Register the channel URI with Notification Hubs.
            await App.MobileService.GetPush().RegisterAsync(channel.Uri);
        }

    Tämä koodi sovelluksen ChannelURI hakee WNS ja rekisteröi sitten kyseisen ChannelURI sovelluksen palvelun Mobile-sovelluksessa.

3. **App.xaml.cs** **OnLaunched** -tapahtumakäsittelijä yläreunassa Lisää **asynkroninen** -määrite menetelmän määritys ja seuraavat puhelu uusi **InitNotificationsAsync** menetelmä, kuten seuraavassa esimerkissä:

        protected async override void OnLaunched(LaunchActivatedEventArgs e)
        {
            await InitNotificationsAsync();

            // ...
        }

    Tämä takaa, että lyhytkestoinen ChannelURI on rekisteröity aina, kun sovellus käynnistetään.

4. Muodosta uudelleen UWP sovelluksen projektin. Sovellus on nyt valmis ilmoitukseen ilmoituksia.

##<a id="test"></a>Sovelluksen testi push-ilmoitukset

[AZURE.INCLUDE [app-service-mobile-windows-universal-test-push](../../includes/app-service-mobile-windows-universal-test-push.md)]


##<a id="more"></a>Seuraavat vaiheet

Lisätietoja push-ilmoitukset:

* [Miten voit käyttää hallitun asiakkaan Azure-mobiilisovellukset](app-service-mobile-dotnet-how-to-use-client-library.md#how-to-register-push-templates-to-send-cross-platform-notifications)  
Mallien avulla voit lähettää Office kaikissa ympäristöissä työntää lokalisoitu työntää joustavuutta. Opettele rekisteröidä malleja.

* [Push-ilmoituksen vianmääritys](../notification-hubs/notification-hubs-push-notification-fixer.md)  
On useita syitä, miksi ilmoitukset Hae pudotettu tai lopu laitteisiin. Tässä ohjeaiheessa esitellään, miten haluat analysoida ja push ilmoituksen virheet pääkansion syy voi selvittää. 

Harkitse jatkuvaa voin jompikumpi seuraavista opetusohjelmat:

+ [Käyttöoikeuksien lisääminen sovelluksen](app-service-mobile-windows-store-dotnet-get-started-users.md)  
  Opettele todennetaan Liittoutuminen sovelluksesi käyttäjät.

+ [Sovelluksen offline-synkronoinnin ottaminen käyttöön](app-service-mobile-windows-store-dotnet-get-started-offline-data.md)  
  Opettele lisäämään offline-tuki käyttämällä Mobile-sovelluksen Taustajärjestelmä sovelluksen. Offline-synkronoinnin sallii loppukäyttäjien kanssa mobiilisovelluksessa&mdash;tarkasteleminen, lisääminen tai muokkaaminen tietojen&mdash;myös silloin, kun yhteyttä ei ole verkossa.

<!-- Anchors. -->

<!-- URLs. -->
[Azure Portal]: https://portal.azure.com/

<!-- Images. -->

