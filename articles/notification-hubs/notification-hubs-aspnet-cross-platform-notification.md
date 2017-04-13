<properties
    pageTitle="Office kaikissa ympäristöissä ilmoitukset lähetetään ilmoitus keskittimet (ASP.NET) käyttäjille"
    description="Opettele käyttämään lähettäminen yksittäisen pyynnön ympäristö ympäristöstä riippumattomalla tavalla ilmoitus, joka on tarkoitettu kaikissa ympäristöissä ilmoituksen keskittimet malleja."
    services="notification-hubs"
    documentationCenter=""
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/03/2016" 
    ms.author="yuaxu"/>

# <a name="send-cross-platform-notifications-to-users-with-notification-hubs"></a>Office kaikissa ympäristöissä ilmoitukset lähetetään käyttäjille, joilla ilmoituksen keskittimet


[Ilmoita käyttäjille, joilla ilmoituksen keskittimet]edellisessä opetusohjelmassa opit, miten push-ilmoitukset kaikki tietyn todennetun käyttäjän rekisteröityjen laitteisiin. Kyseisen opetusohjelmassa useita pyyntöjä edellyttämiä ilmoituksen lähettäminen kussakin tuetut asiakas-ympäristössä. Ilmoitus keskittimet tukee mallit, joten voit määrittää, miten tietyn laitteen haluaa ilmoituksia. Tämä yksinkertaistaa Office kaikissa ympäristöissä ilmoitusten lähettäminen. Tässä ohjeaiheessa kerrotaan, miten voit hyödyntää mallia lähettäminen yksittäisen pyynnön platform ympäristöstä riippumattomalla tavalla ilmoitus, joka on tarkoitettu kaikissa ympäristöissä. Yksityiskohtaisempia tietoja mallit-kohdassa [Azure ilmoituksen keskittimet yleiskatsaus][Templates].

> [AZURE.NOTE] Ilmoitus keskittimet avulla laitteeseen, jotta voit rekisteröidä useita malleja sama tunniste. Tässä tapauksessa kohdistamisen sama tunniste saapuvan sanoman tuottaa useita ilmoitukset toimitetaan laitteeseen, yksi jokaista mallia. Näin voit näyttää useita visuaaliset ilmoitukset, kuten molemmat merkki ja ilmoitusruudun Windows-kaupan sovelluksessa saman viestin.

Mikä Office kaikissa ympäristöissä kertaa mallien avulla seuraavasti:

1. Napsauta ratkaisunhallinnassa Visual Studiossa Laajenna **ohjaimet** -kansio ja valitse Avaa RegisterController.cs-tiedosto.

2. Etsi-koodin lohko, joka luo uuden rekisteröinti korvaa **Post** -menetelmää `switch` sisällöstä seuraava koodi:

        switch (deviceUpdate.Platform)
        {
            case "mpns":
                var toastTemplate = "<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
                    "<wp:Notification xmlns:wp=\"WPNotification\">" +
                       "<wp:Toast>" +
                            "<wp:Text1>$(message)</wp:Text1>" +
                       "</wp:Toast> " +
                    "</wp:Notification>";
                registration = new MpnsTemplateRegistrationDescription(deviceUpdate.Handle, toastTemplate);
                break;
            case "wns":
                toastTemplate = @"<toast><visual><binding template=""ToastText01""><text id=""1"">$(message)</text></binding></visual></toast>";
                registration = new WindowsTemplateRegistrationDescription(deviceUpdate.Handle, toastTemplate);
                break;
            case "apns":
                var alertTemplate = "{\"aps\":{\"alert\":\"$(message)\"}}";
                registration = new AppleTemplateRegistrationDescription(deviceUpdate.Handle, alertTemplate);
                break;
            case "gcm":
                var messageTemplate = "{\"data\":{\"message\":\"$(message)\"}}";
                registration = new GcmTemplateRegistrationDescription(deviceUpdate.Handle, messageTemplate);
                break;
            default:
                throw new HttpResponseException(HttpStatusCode.BadRequest);
        }

    Tämä koodi kutsuu käyttöjärjestelmäkohtaiset menetelmä luo malli-rekisteröinti alkuperäisen rekisteröinnin sijaan. Olemassa olevan merkintöjen on ei voi muokata, koska mallin-merkintöjä johdettu alkuperäisen merkintöjä.

3. Korvaa **ilmoitukset** -ohjain **sendNotification** menetelmä seuraava koodi:

        public async Task<HttpResponseMessage> Post()
        {
            var user = HttpContext.Current.User.Identity.Name;
            var userTag = "username:" + user;

            var notification = new Dictionary<string, string> { { "message", "Hello, " + user } };
            await Notifications.Instance.Hub.SendTemplateNotificationAsync(notification, userTag);

            return Request.CreateResponse(HttpStatusCode.OK);
        }

    Tämä koodi lähettää ilmoituksen kaikissa ympäristöissä samanaikaisesti ja eikä sinun tarvitse määrittää alkuperäiset tiedot. Ilmoitus keskittimet muodostaa ja toimittaa oikeat tiedot kaikkiin laitteisiin annettu _tunnisteen_ arvona rekisteröityjen mallien mukaisesti.

4. Julkaista projektisi WebApi taustatietokantaan uudelleen.

5. Suorita asiakkaan sovellus uudelleen ja varmista, että rekisteröinti onnistuu.

6. (Valinnainen) Asiakas-sovelluksen käyttöönotto toisen laitteen ja valitse Suorita sovellus.

    Huomaa, että ilmoitus näkyy laitteissa.

## <a name="next-steps"></a>Seuraavat vaiheet

Nyt kun olet tehnyt Tässä opetusohjelmassa, Lue lisää ilmoituksen keskittimet ja mallit, Näissä ohjeaiheissa:

+ **[Ilmoitus keskittimet avulla voit lähettää uutisia]** <br/>Esittelee toiseen skenaario mallien käyttäminen

+  **[Azure ilmoituksen keskittimet yleiskatsaus][Templates]**<br/>Ohjeaihe on tarkemmat tiedot malleja.


<!-- Anchors. -->

<!-- Images. -->




<!-- URLs. -->
[Push to users ASP.NET]: /manage/services/notification-hubs/notify-users-aspnet
[Push to users Mobile Services]: /manage/services/notification-hubs/notify-users/
[Visual Studio 2012 Express for Windows 8]: http://go.microsoft.com/fwlink/?LinkId=257546

[Ilmoitus keskittimet avulla voit lähettää uutisia]: notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md
[Azure Notification Hubs]: http://go.microsoft.com/fwlink/p/?LinkId=314257
[Ilmoita käyttäjille ilmoituksen keskittimet kanssa]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[Templates]: http://go.microsoft.com/fwlink/p/?LinkId=317339
[Notification Hub How to for Windows Store]: http://msdn.microsoft.com/library/windowsazure/jj927172.aspx
