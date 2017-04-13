<properties
    pageTitle="Lähettää push-ilmoitukset Azure ilmoituksen keskittimet Windows Phone | Microsoft Azure"
    description="Tässä opetusohjelmassa opit käyttämään Azure ilmoituksen keskittimet push-ilmoitukset Windows Phone 8: n tai Windows Phone 8.1 Silverlight-sovellukseen."
    services="notification-hubs"
    documentationCenter="windows"
    keywords="Push-ilmoituksen, push ilmoituksen windows Phonen push"
    authors="ysxu"
    manager="erikre"
    editor="erikre"/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows-phone"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/03/2016"
    ms.author="yuaxu"/>

# <a name="sending-push-notifications-with-azure-notification-hubs-on-windows-phone"></a>Lähettää push-ilmoitukset Azure ilmoituksen keskittimet Windows Phone

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

##<a name="overview"></a>Yleiskatsaus

> [AZURE.NOTE] Jotta voit suorittaa tässä opetusohjelmassa, on oltava aktiivinen Azure-tili. Jos sinulla ei ole tiliä, voit luoda ilmainen kokeiluversio tili vain muutaman minuutin. Lisätietoja on artikkelissa [Azure maksuttoman kokeiluversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-windows-phone-get-started%2F).

Tässä opetusohjelmassa näytetään, miten push-ilmoitukset lähetetään Windows Phone 8: n tai Windows Phone 8.1 Silverlight-sovelluksen Azure ilmoituksen keskittimet avulla. Jos kohdennat Windows Phone 8.1 (-Silverlight), viittaa [Windows yleinen](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) versio.
Tässä opetusohjelmassa voit luoda tyhjän Windows Phone 8-sovellusta, joka vastaanottaa push-ilmoitukset käyttämällä Microsoftin Push ilmoituksen Service (MPNS). Kun olet valmis, pystyt käyttämään yleislähetys push-ilmoituksia, jotka kaikki laitteet, joissa sovelluksen ilmoitus-toiminnossa.

> [AZURE.NOTE] Ilmoitus keskittimet Windows Phone-SDK ei tue Windowsin Push-ilmoituksen Service (WNS) käyttäminen Windows Phone 8.1 Silverlight-sovellukset. Jos haluat käyttää WNS (sijaan MPNS) Windows Phone 8.1 Silverlight-sovellukset, noudata [Ilmoituksen keskittimet - opetusohjelma Windows Phone Silverlight], joka käyttää REST API.

Opetusohjelman esitellään yksinkertainen lähetyksen skenaarion käyttämällä ilmoituksen keskittimet.

##<a name="prerequisites"></a>Edellytykset

Tässä opetusohjelmassa edellyttää seuraavaa:

+ [Visual Studio 2012 Express for Windows Phone]- tai sitä uudempi versio.

Noudata tässä opetusohjelmassa on kaikki muut ilmoituksen keskittimet opetusohjelmat for Windows Phone 8-sovellukset.

##<a name="create-your-notification-hub"></a>Luo ilmoitus-toiminnossa

[AZURE.INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="6">
<li><p>Valitse <b>Ilmoitus Services</b> osan ( <i>asetukset</i>), <b>Windows Phone (MPNS)</b> valitsemalla ja valitse sitten <b>Ota käyttöön Todentamattomille push</b> -valintaruutu.</p>
</li>
</ol>

&emsp;&emsp;![Azure Portal – Ota käyttöön unathenticated push-ilmoitukset](./media/notification-hubs-windows-phone-get-started/azure-portal-unauth.png)

Oman toiminnosta on nyt luonut ja määrittänyt lähettää Todentamattomille Windows Phone-ilmoitus.

> [AZURE.NOTE] Tässä opetusohjelmassa käytetään MPNS Todentamattomille-tilassa. MPNS Todentamattomille tilassa sisältyy rajoitukset ilmoitukset, voit lähettää kanava. Ilmoitus keskittimet tukee [MPNS todennettu tilassa](http://msdn.microsoft.com/library/windowsphone/develop/ff941099.aspx) niin, että voit ladata varmennetta.

##<a name="connecting-your-app-to-the-notification-hub"></a>Sovelluksen muodostamisesta ilmoitus-toiminnossa

1. Visual Studiossa Luo uusi Windows Phone 8-sovellus.

    ![Visual Studio - uusi projekti - Windows Phone-sovelluksen][13]

    Visual Studio 2013: n päivitys 2 tai uudempi versio voit luoda sen sijaan Windows Phone Silverlight-sovelluksen.

    ![Visual Studio - uusi projekti - tyhjä - sovellus Windows Phone Silverlight][11]

2. Visual Studiossa ratkaisun hiiren kakkospainikkeella ja valitse sitten **NuGet pakettien hallinta**.

    Tämä avaa **NuGet pakettien hallinta** -valintaikkunan.

3. Etsi `WindowsAzure.Messaging.Managed` ja valitse **Asenna**ja hyväksy käyttöehdot.

    ![Visual Studio - NuGet pakettien hallinta][20]

    Tämä Lataa, asentaa ja Lisää viittaus Azure viestintä-kirjastoon Windowsin <a href="http://nuget.org/packages/WindowsAzure.Messaging.Managed/">WindowsAzure.Messaging.Managed NuGet paketti</a>.

4. Avaa tiedosto App.xaml.cs ja lisää seuraava `using` lauseet:

        using Microsoft.Phone.Notification;
        using Microsoft.WindowsAzure.Messaging;

5. Lisää seuraava koodi **Application_Launching** menetelmä App.xaml.cs yläreunassa:

        var channel = HttpNotificationChannel.Find("MyPushChannel");
        if (channel == null)
        {
            channel = new HttpNotificationChannel("MyPushChannel");
            channel.Open();
            channel.BindToShellToast();
        }

        channel.ChannelUriUpdated += new EventHandler<NotificationChannelUriEventArgs>(async (o, args) =>
        {
            var hub = new NotificationHub("<hub name>", "<connection string>");
            var result = await hub.RegisterNativeAsync(args.ChannelUri.ToString());

            System.Windows.Deployment.Current.Dispatcher.BeginInvoke(() =>
            {
                MessageBox.Show("Registration :" + result.RegistrationId, "Registered", MessageBoxButton.OK);
            });
        });

    >[AZURE.NOTE] Arvo **MyPushChannel** on indeksin, jota käytetään haku [HttpNotificationChannel](https://msdn.microsoft.com/library/windows/apps/microsoft.phone.notification.httpnotificationchannel.aspx) kokoelman aiemmin luotu kanava. Jos sellaista ei ole käytettävissä, luo uuden tapahtuman nimi.
    
    Varmista, että keskittimeen ja kutsua **DefaultListenSharedAccessSignature** , joka on saatu edellisessä osassa yhteysmerkkijonon nimen.
    Tämä koodi sovelluksen kanavan URI hakee MPNS ja rekisteröi sitten kyseisen kanavan URI ilmoitus-toiminnossa. Se myös takaa, että kanavan URI on rekisteröity ilmoitus-toiminnossa aina, kun sovellus käynnistetään.

    >[AZURE.NOTE]Tässä opetusohjelmassa lähettää ilmoituksen ilmoitukseen laitteeseen. Kun lähetät ruutu ilmoituksen, Soita sen sijaan **BindToShellTile** menetelmä kanavalla. Tuki sekä ilmoitukseen ja ilmoitukset-ruutu, **BindToShellTile** ja **BindToShellToast**.

6. Napsauta ratkaisunhallinnassa, laajenna **Ominaisuudet**, Avaa `WMAppManifest.xml` tiedoston, valitse **Ominaisuudet** -välilehti ja varmista, että **ID_CAP_PUSH_NOTIFICATION** -ominaisuus on valittuna.

    ![Visual Studio - Windows Phone-sovelluksen ominaisuudet][14]

    Näin varmistat, että sovelluksesi voi vastaanottaa push-ilmoitukset. Push-ilmoituksen lähettäminen sovellus yrittää epäonnistuu ilman sitä.

7. Paina `F5` Suorita sovellus-näppäintä.

    Rekisteröity-viesti näkyy-sovelluksessa.

8. Sovelluksen sulkeminen  

   >[AZURE.NOTE] Saat push ilmoitusruudun, sovellus ei saa olla käynnissä etualalla.

##<a name="send-push-notifications-from-your-backend"></a>Lähettäminen käyttäjän Taustajärjestelmä push-ilmoitukset

Voit lähettää push-ilmoitusten avulla ilmoituksen keskittimet minkä tahansa taustasta julkisessa <a href="http://msdn.microsoft.com/library/windowsazure/dn223264.aspx">REST-liittymän</a>kautta. Tässä opetusohjelmassa Lähetä push-ilmoitukset .NET console-sovelluksesta. 

Esimerkki siitä, miten push-ilmoitusten lähettäminen ASP.NET WebAPI-taustatietokannan, joka on integroitu ilmoituksen keskittimet on artikkelissa [Azure ilmoituksen keskittimet Ilmoita käyttäjille, joilla .NET Taustajärjestelmä](./notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md).  

Esimerkki lähettäminen [REST API](https://msdn.microsoft.com/library/azure/dn223264.aspx)käyttämällä push-ilmoitukset Tutustu [käyttämisestä ilmoituksen keskittimet Java-](./notification-hubs-java-push-notification-tutorial.md) ja [Opi käyttämään ilmoituksen keskittimet PHP](./notification-hubs-php-push-notification-tutorial.md).

1. Ratkaisun hiiren kakkospainikkeella, valitse **Lisää** ja **Uuden projektin...**, valitse **Visual C#**-kohdassa **Windows** ja **Console-sovelluksen**, ja valitsemalla **OK**.

    ![Visual Studio - uusi projekti - konsolisovelluksen][6]

    Tämä lisää uuden Visual C# console-sovelluksen ratkaisuun. Voit myös tehdä tämän erilliset ratkaisu.

4. Valitse **Työkalut**ja **kirjaston paketin**hallinta **Paketin hallinta-konsolin**.

    Tämä näyttää pakkauksen hallinta-konsolin.

5.  **Pakettien hallinta konsoli** -ikkunassa määrittää projektiin uuden console-sovelluksen **oletusarvon projektin** ja suorita sitten konsoli-ikkunassa seuraava komento:

        Install-Package Microsoft.Azure.NotificationHubs

    Tämä lisää <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification keskittimet NuGet paketin</a>Azure-ilmoituksen keskittimet SDK viittaus.

6. Avaa `Program.cs` tiedoston ja lisää seuraava `using` lause:

        using Microsoft.Azure.NotificationHubs;

6. Valitse `Program` luokan, Lisää seuraavalla tavalla:

        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient
                .CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            string toast = "<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
                "<wp:Notification xmlns:wp=\"WPNotification\">" +
                   "<wp:Toast>" +
                        "<wp:Text1>Hello from a .NET App!</wp:Text1>" +
                   "</wp:Toast> " +
                "</wp:Notification>";
            await hub.SendMpnsNativeNotificationAsync(toast);
        }

    Varmista, että korvaa `<hub name>` ilmoitus-keskittimeen, joka näkyy portaalissa nimen paikkamerkki. Korvaa myös yhteyden merkkijonon paikkamerkki yhteysmerkkijonon kutsutaan **DefaultFullSharedAccessSignature** , joka on saatu osan "Määrittäminen ilmoitus-toiminnossa."

    >[AZURE.NOTE]Varmista, että yhteysmerkkijonoa käyttämällä **täydet** oikeudet, ei **Kuuntele** access. Kuuntele access-merkkijono ei ole oikeutta lähettää push-ilmoitukset.

4. Lisää seuraava rivi-että `Main` menetelmää:

         SendNotificationAsync();
         Console.ReadLine();

5. Oman Windows Phone-emulaattorin käynnissä ja sovellus suljetaan, määrittää konsolin sovelluksen projektin kuin oletusarvoinen käynnistys-projekti ja paina `F5` Suorita sovellus-näppäintä.

    Saat push ilmoitusruudun. Napauttamalla ilmoitukseen ilmoituspalkissa Lataa sovellus.

Löydät kaikki mahdolliset paketteja MSDN-sivuston [ilmoitukseen luettelon] ja [vierekkäin luettelon] ohjeaiheista.

##<a name="next-steps"></a>Seuraavat vaiheet

Tämä esimerkki lähettää push-ilmoitukset kaikkien Windows Phone 8-laitteiden. 

Kohteen tietyille käyttäjille, jotta voit viitata [Käyttäjille push-ilmoitukset käyttöön ilmoituksen keskittimiä] opetusohjelman. 

Jos haluat määritetään käyttäjien korko ryhmiin, voit lukea [Käytä ilmoituksen keskittimet lähettää uutisia]. 

Lisätietoja ilmoituksen keskittimet käyttämisestä [Ilmoituksen keskittimet ohjeita].



<!-- Images. -->
[6]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-console-app.png
[7]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-from-portal.png
[8]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-from-portal2.png
[9]: ./media/notification-hubs-windows-phone-get-started/notification-hub-select-from-portal.png
[10]: ./media/notification-hubs-windows-phone-get-started/notification-hub-select-from-portal2.png
[11]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-wp-silverlight-app.png
[12]: ./media/notification-hubs-windows-phone-get-started/notification-hub-connection-strings.png

[13]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-wp-app.png
[14]: ./media/notification-hubs-windows-phone-get-started/mobile-app-enable-push-wp8.png
[15]: ./media/notification-hubs-windows-phone-get-started/notification-hub-pushauth.png
[20]: ./media/notification-hubs-windows-phone-get-started/notification-hub-windows-universal-app-install-package.png
[213]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-console-app.png





<!-- URLs. -->
[Visual Studio 2012 Express for Windows Phone]: https://go.microsoft.com/fwLink/p/?LinkID=268374
[Ilmoitus keskittimet ohjeet]: http://msdn.microsoft.com/library/jj927170.aspx
[MPNS authenticated mode]: http://msdn.microsoft.com/library/windowsphone/develop/ff941099(v=vs.105).aspx
[Ilmoitus keskittimet avulla käyttäjille push-ilmoitukset]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[Ilmoitus keskittimet avulla voit lähettää uutisia]: notification-hubs-windows-phone-push-xplat-segmented-mpns-notification.md
[saapuvan luettelo]: http://msdn.microsoft.com/library/windowsphone/develop/jj662938(v=vs.105).aspx
[ruutu-luettelo]: http://msdn.microsoft.com/library/windowsphone/develop/hh202948(v=vs.105).aspx
[Ilmoitus keskittimet - Windows Phone Silverlight-opetusohjelma]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/PushToSLPhoneApp

