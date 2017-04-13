<properties
    pageTitle="Azure ilmoituksen keskittimet suojatun Push"
    description="Lue, miten lähettäminen Azure suojatun push-ilmoitukset. MALLIKOODEJA kirjoitettu C# .NET-Ohjelmointirajapinnan käyttäminen."
    documentationCenter="windows"
    authors="ysxu"
    manager="erikre"
    editor=""
    services="notification-hubs"/>

<tags
    ms.service="notification-hubs" 
    ms.workload="mobile"
    ms.tgt_pltfrm="windows"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

#<a name="azure-notification-hubs-secure-push"></a>Azure ilmoituksen keskittimet suojatun Push

> [AZURE.SELECTOR]
- [Windowsin Universal](notification-hubs-aspnet-backend-windows-dotnet-wns-secure-push-notification.md)
- [iOS](notification-hubs-aspnet-backend-ios-push-apple-apns-secure-notification.md)
- [Android-laitteeseen](notification-hubs-aspnet-backend-android-secure-google-gcm-push-notification.md)


##<a name="overview"></a>Yleiskatsaus

Push-ilmoituksen tuki Microsoft Azure avulla voit käyttää helposti käytettävällä, useita, skaalata ulos push-infrastruktuuria, mikä vähentää huomattavasti yksinkertaistaa push-ilmoitukset mobile ympäristöjen kuluttaja- ja enterprise-sovellusten käyttöönoton.

Vuoksi säädösten tai suojauksen rajoitukset, joskus sovelluksen halutessasi lisätä jotain ilmoituksessa, joka voidaan lähettää vakio push-ilmoituksen infrastruktuurin kautta. Tässä opetusohjelmassa kerrotaan, miten saavuttamiseksi on sama kuvaus lähettämällä luottamuksellisia tietoja asiakaslaitteen ja sovelluksen Taustajärjestelmä välillä suojatun, todennetun-yhteyden kautta.

Korkean tason kulun on seuraavanlainen:

1. -Sovelluksen taustatietokantaan:
    - Säilöjen suojatun paketti-taustatietokannan.
    - Lähettää ilmoituksen tunnus (suojattu mitään tietoja ei lähetetä) laite.
2. Sovelluksen laitteeseen, kun saat ilmoituksen:
    - Laitteen yhteystietojen pyytää suojatun sisällön taustatietokannan.
    - Sovellus voidaan näyttää tiedot ilmoituksen laitteeseen.

On tärkeää muistaa, että edellisen kulun (ja tässä opetusohjelmassa) oletetaan, että laitteen tallentaa todennus-tunnuksen paikalliseen säilöön sen jälkeen, kun käyttäjä kirjautuu. Tämä takaa laitteen voit hakea ilmoituksen suojattu tietojen käyttäminen tunnuksessa kokonaan sujuvan kokemuksen. Jos sovellusta ei Tallenna todennus tunnusten laitteeseen tai näiden tunnusten on päättynyt, laite-sovelluksen, kun saat ilmoituksen pitäisi näkyä yleinen ilmoituksen käyttäjältä, Käynnistä sovellus. Valitse sovellus todentaa käyttäjän sekä näkyy ilmoitus-paketti.

Suojattu Push Tässä opetusohjelmassa näytetään push-ilmoituksen lähettäminen suojatusti. Opetusohjelman muodostaa [Ilmoita käyttäjille](notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md) , opetusohjelma, jotta on suoritettava vaiheet ensin, että opetusohjelmassa.

> [AZURE.NOTE] Tässä opetusohjelmassa oletetaan, että olet luonut ja määrittänyt ilmoitus-toiminnossa kuvatulla tavalla [Ilmoituksen keskittimet (Windows-kaupan) käytön aloittaminen](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md).
Huomaa, että Windows Phone 8.1 edellyttää (ei ole Windows Phone) Windows-tunnistetiedot, että taustatehtävät eivät toimi Windows Phone 8.0 tai Silverlight 8.1. Windows-kaupan sovelluksia, voit saada ilmoituksia kautta taustatehtävän vain, jos sovellus on käytössä Lukitse näytön (Valitse valintaruutu Appmanifest).

[AZURE.INCLUDE [notification-hubs-aspnet-backend-securepush](../../includes/notification-hubs-aspnet-backend-securepush.md)]

## <a name="modify-the-windows-phone-project"></a>Muokkaa Windows Phone-projekti

1. **NotifyUserWindowsPhone** Projectissa lisätä seuraava koodi App.xaml.cs rekisteröidä push-tausta-tehtävän. Lisää seuraava rivi koodin lopussa `OnLaunched()` menetelmää:

        RegisterBackgroundTask();

2. Napsauta edelleen App.xaml.cs, Lisää seuraava koodi heti, kun `OnLaunched()` menetelmää:

        private async void RegisterBackgroundTask()
        {
            if (!Windows.ApplicationModel.Background.BackgroundTaskRegistration.AllTasks.Any(i => i.Value.Name == "PushBackgroundTask"))
            {
                var result = await BackgroundExecutionManager.RequestAccessAsync();
                var builder = new BackgroundTaskBuilder();

                builder.Name = "PushBackgroundTask";
                builder.TaskEntryPoint = typeof(PushBackgroundComponent.PushBackgroundTask).FullName;
                builder.SetTrigger(new Windows.ApplicationModel.Background.PushNotificationTrigger());
                BackgroundTaskRegistration task = builder.Register();
            }
        }

3. Lisää seuraava `using` lauseet App.xaml.cs tiedoston yläosassa:

        using Windows.Networking.PushNotifications;
        using Windows.ApplicationModel.Background;

4. Valitse Visual Studio **Tiedosto** -valikosta **Tallenna kaikki**.

## <a name="create-the-push-background-component"></a>Push-tausta-osan luominen

Seuraavaksi voit luoda push-tausta-komponentin.

1. Napsauta ratkaisunhallinnassa Napsauta ylätason solmu (Tässä tapauksessa**ratkaisu SecurePush** )-ratkaisun, valitsemalla **Lisää**valitse sitten **Uusi projekti**.

2. Laajenna **Kaupan sovellukset**, ja valitse **Windows Phone**-sovellukset ja valitse sitten **Windows osa (Windows Phone)**. Projektin **PushBackgroundComponent**nimeä ja valitse **OK** , jos haluat luoda projektin.

    ![][12]

3. Napsauta ratkaisunhallinnassa **PushBackgroundComponent (Windows Phone 8.1)** projektin, napsauta hiiren kakkospainikkeella ja valitse **Lisää**ja valitse sitten **luokan**. Nimeä uusi luokka **PushBackgroundTask.cs**. Valitse **Lisää** luokan luomiseen.

4. Korvaa seuraava koodi, korvaaminen paikkamerkin koko sisällön **PushBackgroundComponent** nimitilan määrityksen `{back-end endpoint}` saatu otettaessa käyttöön oman taustatietokantaan taustatietokantaan päätepisteissä:

        public sealed class Notification
            {
                public int Id { get; set; }
                public string Payload { get; set; }
                public bool Read { get; set; }
            }

            public sealed class PushBackgroundTask : IBackgroundTask
            {
                private string GET_URL = "{back-end endpoint}/api/notifications/";

                async void IBackgroundTask.Run(IBackgroundTaskInstance taskInstance)
                {
                    // Store the content received from the notification so it can be retrieved from the UI.
                    RawNotification raw = (RawNotification)taskInstance.TriggerDetails;
                    var notificationId = raw.Content;

                    // retrieve content
                    BackgroundTaskDeferral deferral = taskInstance.GetDeferral();
                    var httpClient = new HttpClient();
                    var settings = ApplicationData.Current.LocalSettings.Values;
                    httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Basic", (string)settings["AuthenticationToken"]);

                    var notificationString = await httpClient.GetStringAsync(GET_URL + notificationId);

                    var notification = JsonConvert.DeserializeObject<Notification>(notificationString);

                    ShowToast(notification);

                    deferral.Complete();
                }

                private void ShowToast(Notification notification)
                {
                    ToastTemplateType toastTemplate = ToastTemplateType.ToastText01;
                    XmlDocument toastXml = ToastNotificationManager.GetTemplateContent(toastTemplate);
                    XmlNodeList toastTextElements = toastXml.GetElementsByTagName("text");
                    toastTextElements[0].AppendChild(toastXml.CreateTextNode(notification.Payload));
                    ToastNotification toast = new ToastNotification(toastXml);
                    ToastNotificationManager.CreateToastNotifier().Show(toast);
                }
            }

5. Napsauta ratkaisunhallinnassa Napsauta **PushBackgroundComponent (Windows Phone 8.1)** -projekti ja valitse sitten **NuGet pakettien hallinta**.

6. Valitse vasemmalla puolella **Online**.

7. Kirjoita **hakuruutuun** **Http-asiakas**.

8. Valitse tulokset-luettelosta **Microsoft HTTP asiakkaan kirjastoihin**ja valitse sitten **Asenna**. Viimeistele asennus.

9. Kirjoita **Json.net**takaisin sisään NuGet **haku** -ruutuun. Asenna **Json.NET** paketti ja valitse Sulje NuGet pakettien hallinta-ikkuna.

10. Lisää seuraava `using` lauseet **PushBackgroundTask.cs** tiedoston yläosassa:

        using Windows.ApplicationModel.Background;
        using Windows.Networking.PushNotifications;
        using System.Net.Http;
        using Windows.Storage;
        using System.Net.Http.Headers;
        using Newtonsoft.Json;
        using Windows.UI.Notifications;
        using Windows.Data.Xml.Dom;

11. Napsauta ratkaisunhallinnassa **NotifyUserWindowsPhone (Windows Phone 8.1)** Projectissa **viittaukset**hiiren kakkospainikkeella ja valitse **Lisää viittaus...**. Viittaus hallinta-valintaikkunassa **PushBackgroundComponent**vieressä oleva valintaruutu ja valitse sitten **OK**.

12. Napsauta ratkaisunhallinnassa Kaksoisnapsauta **Package.appxmanifest** **NotifyUserWindowsPhone (Windows Phone 8.1)** -projekti. Määritä **ilmoitukset**, **Kyllä** **Ilmoitukseen pystyvät** .

    ![][3]

13. **Package.appxmanifest**Napsauta lähellä sivun **ilmoitukset** -valikosta. Valitse **Käytettävissä olevat ilmoitukset** avattavasta- **Taustaprosessit**ja valitse sitten **Lisää**.

14. Tarkista **Package.appxmanifest**, valitse **Ominaisuudet**- **Push-ilmoitus**.

15. Kirjoita **Package.appxmanifest**, valitse **Sovelluksen asetukset**- **PushBackgroundComponent.PushBackgroundTask** **Aloituskohtaa** -kenttään.

    ![][13]

16. Valitse **Tiedosto** -valikosta **Tallenna kaikki**.

## <a name="run-the-application"></a>Suorita sovellus

Suorita sovellus, toimi seuraavasti:

1. Visual Studion **AppBackend** verkko-Ohjelmointirajapinnan-sovelluksen käyttämiseen. ASP.NET-web-sivu tulee näkyviin.

2. Visual Studiossa Suorita **NotifyUserWindowsPhone (Windows Phone 8.1)** Windows Phone-sovellus. Windows Phone-emulaattorin suorittaa ja lataa sovellus automaattisesti.

3. Kirjoita **NotifyUserWindowsPhone** sovelluksen Käyttöliittymän, käyttäjänimi ja salasana. Nämä voi olla mikä tahansa merkkijono, mutta niiden on oltava sama arvo.

4. Käyttöliittymän **NotifyUserWindowsPhone** -sovelluksessa valitsemalla **Kirjaudu sisään ja rekisteröi**. Valitse **Lähetä push**.

[3]: ./media/notification-hubs-aspnet-backend-windows-dotnet-secure-push/notification-hubs-secure-push3.png
[12]: ./media/notification-hubs-aspnet-backend-windows-dotnet-secure-push/notification-hubs-secure-push12.png
[13]: ./media/notification-hubs-aspnet-backend-windows-dotnet-secure-push/notification-hubs-secure-push13.png
