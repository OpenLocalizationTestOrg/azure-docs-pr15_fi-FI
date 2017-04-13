<properties
    pageTitle="iOS Push-ilmoitukset, joiden ilmoituksen keskittimet Xamarin sovellusten | Microsoft Azure"
    description="Tässä opetusohjelmassa opit käyttämään Azure ilmoituksen keskittimet push-ilmoitukset lähetetään Xamarin iOS-sovelluksen."
    services="notification-hubs"
    keywords="iOS push-ilmoitukset, push viestien push-ilmoitukset, push-viesti"
    documentationCenter="xamarin"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

# <a name="ios-push-notifications-with-notification-hubs-for-xamarin-apps"></a>iOS Push-ilmoitukset, joiden ilmoituksen keskittimet Xamarin sovellusten

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

##<a name="overview"></a>Yleiskatsaus
> [AZURE.IMPORTANT] Jotta voit suorittaa tässä opetusohjelmassa, on oltava aktiivinen Azure-tili. Jos sinulla ei ole tiliä, voit luoda ilmainen kokeiluversio tili vain muutaman minuutin. Lisätietoja on artikkelissa [Azure maksuttoman kokeiluversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fpartner-xamarin-notification-hubs-ios-get-started).

Tässä opetusohjelmassa näytetään, miten push-ilmoitukset lähetetään iOS-sovellukseen Azure ilmoituksen keskittimet avulla.
Luo tyhjä Xamarin.iOS-sovellusta, joka vastaanottaa push-ilmoitukset [Apple Push-ilmoituspalvelu (APN)](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html)avulla. Kun olet valmis, pystyt käyttämään yleislähetys push-ilmoituksia, jotka kaikki laitteet, joissa sovelluksen ilmoitus-toiminnossa. Valmis-koodi on käyttää [sovelluksen NotificationHubs] [ GitHub] malli.

Tässä opetusohjelmassa kuvataan kanssa ilmoituksen keskittimet yksinkertainen push viestin yleislähetys-skenaario.

##<a name="prerequisites"></a>Edellytykset

Tässä opetusohjelmassa edellyttää seuraavaa:

+ [Xcode 6.0][Install Xcode]
+ IOS 7.0 (tai uudempi versio) yhteensopivan laitteen
+ iOS kehittäjä-ohjelman jäsenyys
+ [Xamarin Studio]

   > [AZURE.NOTE] Määritykset-vaatimusten vuoksi for iOS push-ilmoitukset, on käyttöönotto ja esikatsella fyysinen iOS-laite (iPhone tai iPad)-sovelluksen malli simulator sijaan.

Noudata tässä opetusohjelmassa on Xamarin iOS-sovellusten kaikkiin muihin ilmoituksen keskittimet opetusohjelmiin.


[AZURE.INCLUDE [Notification Hubs Enable Apple Push Notifications](../../includes/notification-hubs-enable-apple-push-notifications.md)]


##<a name="configure-your-notification-hub"></a>Määritä ilmoitus-toiminnossa

Tässä osassa esitellään luodaan uusi ilmoitus-toiminnosta ja käyttöoikeuksien määrittäminen käyttämällä **.p12** push-sertifikaatti, jonka loit APN kautta. Jos haluat käyttää ilmoitus-toiminto, jonka olet jo luonut, voit ohittaa vaiheeseen 5.

[AZURE.INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]


<ol start="7">
<li>
<p>Koska haluamme APN-yhteyden määrittäminen Azure-portaalissa, ilmoitus-toiminnossa asetukset, ande <b>Ilmoituksen</b>Services napsauttamalla ja valitse sitten luettelokohdan <b>Apple (APN)</b> . Jälkeen valitsemalla <b>Lataa sertifikaatti</b> ja valitse <b>.p12</b> -varmenne, jonka olet vienyt aiempaa versiota, sekä varmenteen salasanan.</p>
<p>Varmista, että valitset <b>eristetyn</b> tilan jälkeen, kun lähetät viestejä push-kehitysympäristö. Käytä <b>tuotannon</b> -asetusta vain, jos haluat lähettää käyttäjille, joiden ostamistasi kaupasta sovelluksen push-ilmoitukset.</p>
</li>
</ol>
&emsp;&emsp;![](./media/notification-hubs-ios-get-started/notification-hubs-apns.png)

&emsp;&emsp;![](./media/notification-hubs-ios-get-started/notification-hubs-sandbox.png)


Ilmoitus-toiminnossa on nyt määritetty APN-käyttöä varten, ja sinulla on yhteysmerkkijonot rekisteröidä sovelluksen ja Lähetä push-ilmoitukset.


##<a name="connect-your-app-to-the-notification-hub"></a>Sovelluksen yhdistäminen ilmoitus-toiminnossa

#### <a name="create-a-new-project"></a>Uuden projektin luominen

1. Xamarin Studiossa, Luo uusi iOS-projekti ja valitse **Yhdistetyn API** > **Yhden näkymä-sovelluksen** malli.

    ![Xamarin Studio – Valitse sovelluksen tyyppi][31]

2. Lisää Azure Messaging-komponentin viittaus. Ratkaisu-näkymässä hiiren kakkospainikkeella projektin **osat** -kansiota ja valitse **Hae lisää osia**. Etsi **Azure Messaging** -komponentin ja osan lisääminen projektiin.

3. Lisää seuraavat **AppDelegate.cs**lauseella:

        using WindowsAzure.Messaging;

4. Määritellä **SBNotificationHub**esiintymä:

        private SBNotificationHub Hub { get; set; }

5. Luo **Constants.cs** luokan seuraavat muuttujat:

        // Azure app-specific connection string and hub path
        public const string ConnectionString = "<Azure connection string>";
        public const string NotificationHubPath = "<Azure hub path>";


6. Päivitä **AppDelegate.cs** **FinishedLaunching()** vastaamaan seuraavasti:

        public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
        {
            if (UIDevice.CurrentDevice.CheckSystemVersion (8, 0)) {
                var pushSettings = UIUserNotificationSettings.GetSettingsForTypes (
                       UIUserNotificationType.Alert | UIUserNotificationType.Badge | UIUserNotificationType.Sound,
                       new NSSet ());

                UIApplication.SharedApplication.RegisterUserNotificationSettings (pushSettings);
                UIApplication.SharedApplication.RegisterForRemoteNotifications ();
            } else {
                UIRemoteNotificationType notificationTypes = UIRemoteNotificationType.Alert | UIRemoteNotificationType.Badge | UIRemoteNotificationType.Sound;
                UIApplication.SharedApplication.RegisterForRemoteNotificationTypes (notificationTypes);
            }

            return true;
        }

7. Ohita **AppDelegate.cs** **RegisteredForRemoteNotifications()** menetelmää:

        public override void RegisteredForRemoteNotifications(UIApplication application, NSData deviceToken)
        {
            Hub = new SBNotificationHub(Constants.ConnectionString, Constants.NotificationHubPath);

            Hub.UnregisterAllAsync (deviceToken, (error) => {
                if (error != null)
                {
                    Console.WriteLine("Error calling Unregister: {0}", error.ToString());
                    return;
                }

                NSSet tags = null; // create tags if you want
                Hub.RegisterNativeAsync(deviceToken, tags, (errorCallback) => {
                    if (errorCallback != null)
                        Console.WriteLine("RegisterNativeAsync error: " + errorCallback.ToString());
                });
            });
        }

8. Ohita **AppDelegate.cs** **ReceivedRemoteNotification()** menetelmää:

        public override void ReceivedRemoteNotification(UIApplication application, NSDictionary userInfo)
        {
            ProcessNotification(userInfo, false);
        }

9. Luo **AppDelegate.cs** **ProcessNotification()** seuraavasti:

        void ProcessNotification(NSDictionary options, bool fromFinishedLaunching)
        {
            // Check to see if the dictionary has the aps key.  This is the notification payload you would have sent
            if (null != options && options.ContainsKey(new NSString("aps")))
            {
                //Get the aps dictionary
                NSDictionary aps = options.ObjectForKey(new NSString("aps")) as NSDictionary;

                string alert = string.Empty;

                //Extract the alert text
                // NOTE: If you're using the simple alert by just specifying
                // "  aps:{alert:"alert msg here"}  ", this will work fine.
                // But if you're using a complex alert with Localization keys, etc.,
                // your "alert" object from the aps dictionary will be another NSDictionary.
                // Basically the JSON gets dumped right into a NSDictionary,
                // so keep that in mind.
                if (aps.ContainsKey(new NSString("alert")))
                    alert = (aps [new NSString("alert")] as NSString).ToString();

                //If this came from the ReceivedRemoteNotification while the app was running,
                // we of course need to manually process things like the sound, badge, and alert.
                if (!fromFinishedLaunching)
                {
                    //Manually show an alert
                    if (!string.IsNullOrEmpty(alert))
                    {
                        UIAlertView avAlert = new UIAlertView("Notification", alert, null, "OK", null);
                        avAlert.Show();
                    }
                }
            }
        }

    > [AZURE.NOTE] Voit ohittaa **FailedToRegisterForRemoteNotifications()** käsittelemään tilanteissa, kuten ei verkkoyhteyttä. Tämä on erityisen tärkeää, jossa käyttäjä aloittaa sovelluksen offline-tilassa (kuten lentokone) ja haluat käsitellä push messaging skenaarioita koskevat sovelluksen.


10. Suorita sovellus laitteessasi.


## <a name="sending-push-notifications"></a>Push-ilmoitusten lähettäminen


Voit testata sovelluksen push-ilmoitusten vastaanottaminen alla olevassa kuvassa esitetyllä tavalla lähettämällä [Azure-portaalin] kautta **Testata Lähetä** ominaisuuksien **vianmääritys** työkaluryhmä-, ilmoitus-toiminnossa sivun oikeassa ilmoitukset.

![](./media/notification-hubs-ios-get-started/notification-hubs-test-send.png)

Push-ilmoitukset lähetetään tavallisesti taustatietokantaan-palvelun, kuten Mobile-palveluihin tai käyttämällä yhteensopiva kirjaston ASP.NET kautta. Voit käyttää myös REST-Ohjelmointirajapinnalla suoraan, jos haluat lähettää push-viestejä, jos kirjasto ei ole käytettävissä käyttämässäsi skenaariossa. 

Tässä opetusohjelmassa on säilyttää yksinkertainen ja vain kuvaavat testaaminen asiakkaan sovelluksen lähettämällä ilmoitukset käyttämällä .NET SDK ilmoituksen keskittimien sijaan Taustajärjestelmä palvelu console-sovelluksessa. On suositeltavaa [Käyttäjille push-ilmoitukset käyttöön ilmoituksen keskittimiä](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) opetusohjelman lähettämiseen ilmoitukset ASP.NET-taustasta vaiheessa. Seuraavilla tavoilla voi kuitenkin käyttää ilmoitusten lähettäminen:

* **REST-liittymän**: Voit tukea push-ilmoituksen missä tahansa Taustajärjestelmä ympäristössä [REST-liittymän](http://msdn.microsoft.com/library/windowsazure/dn223264.aspx)avulla.

* **Microsoft Azure ilmoituksen keskittimet .NET SDK**: Nuget paketin Managerissa Visual Studiossa, suorita [Asennuksen paketin Microsoft.Azure.NotificationHubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).

* **Node.js** : [ilmoitus keskittimet-Node.js käyttämisestä](notification-hubs-nodejs-push-notification-tutorial.md).

**Mobile-sovellukset**: Katso esimerkki siitä, miten ilmoitukset lähetetään Azure palvelun mobiilisovellukset-taustatietokannan, joka on integroitu ilmoituksen keskittimet, [Lisää push-ilmoituksia, jotka mobile-sovellus](../app-service-mobile/app-service-mobile-ios-get-started-push.md).

* **Java / PHP**: esimerkki push-ilmoitusten lähettäminen käyttämällä REST API-kohdassa "How to use Java/PHP-ilmoituksen keskittimet" ([Java](notification-hubs-java-push-notification-tutorial.md) | [PHP](notification-hubs-php-push-notification-tutorial.md)).


####<a name="optional-send-push-notifications-from-a-net-console-app"></a>(Valinnainen) Push-ilmoitusten lähettäminen .NET Console-sovellus

Tässä osassa lähetämme push-ilmoitukset yksinkertainen .NET konsolin sovellusta käytettäessä. Tässä esimerkissä tarkoitetaan olemme vaihtuu Windows kehitysympäristö, jossa on jo asennettu Visual Studio.

1. Visual Studiossa Luo uusi Visual C# console-sovellus:

    ![Visual Studio - uuden konsolisovelluksen luominen][213]

2. Visual Studiossa Valitse **Työkalut**ja **NuGet paketin**hallinta **Paketin hallinta-konsolin**.

    Paketin hallinta-konsolin pitäisi näkyä kiinnitetyn Visual Studio työtilan alareunaan.

3. Pakettien hallinta konsoli-ikkunassa määrittää projektiin uuden console-sovelluksen **oletusarvon projektin** ja suorita sitten konsoli-ikkunassa seuraava komento:

        Install-Package Microsoft.Azure.NotificationHubs

    Tämä lisää <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification keskittimet NuGet paketin</a>Azure-ilmoituksen keskittimet SDK viittaus.

    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-package-manager.png)


4. Avaa `Program.cs` tiedoston ja lisää seuraava `using` -lauseessa, varmistetaan, että Microsoft käyttää Azure luokat ja tärkeimmät luokka-funktioita:

        using Microsoft.Azure.NotificationHubs;

3. Valitse oman `Program` luokan, Lisää seuraava menetelmä (Älä unohda korvaa **yhteysmerkkijonon** ja **keskittimeen nimi**):

        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            var alert = "{\"aps\":{\"alert\":\"Hello from .NET!\"}}";
            await hub.SendAppleNativeNotificationAsync(alert);
        }

4. Lisää seuraavat rivit-että `Main` menetelmää:

         SendNotificationAsync();
         Console.ReadLine();

5. Suorita sovellus F5-näppäintä. Muutamassa pitäisi näkyä push ilmoitus näkyy laitteessa. Onko käytössäsi Wi-Fi- tai matkapuhelinverkon datayhteyden, varmista, että sinulla on aktiivinen internet-yhteys laitteeseen.

Löydät kaikki mahdolliset paketteja Apple [paikallisen ja Push-ilmoituksen Programming Guide].

####<a name="optional-send-notifications-from-a-mobile-service"></a>(Valinnainen) Ilmoitukset lähetetään mobiili-palvelusta

Tässä osassa lähetämme mobiilipalvelu solmu komentosarjan kautta käyttämällä push-ilmoitukset.

Voit lähettää ilmoituksen käyttämällä mobile-palvelu, noudattamalla [aloittaminen Mobile-palveluihin], ja valitse sitten:

1. Kirjautuminen [Perinteinen Azure-portaaliin]ja valitse mobile-palvelu.

2. Valitse sivun **ajoitus** -välilehti.

    ![Azure perinteinen Portal - ajoitus][215]

3. Luo uuden ajoitetun työn, Lisää nimi ja valitse **pyydettäessä**.

    ![Azure perinteinen Portal - Luo uusi projekti][216]

4. Kun työ on luotu, valitse työn nimi. Valitse yläpalkissa **komentosarja** -välilehti.

5. Lisää seuraavaa komentosarjaa ajoitus-funktion sisällä. Varmista, että Korvaa paikkamerkit ilmoitus-toiminnossa nimesi ja yhteysmerkkijonon *DefaultFullSharedAccessSignature* , jotka olet hankkinut aiemmin varten. Valitse **Tallenna**.

        var azure = require('azure');
        var notificationHubService = azure.createNotificationHubService('<Hubname>', '<SAS Full access >');
        notificationHubService.apns.send(
            null,
            {"aps":
                {
                "alert": "Hello from Mobile Services!"
                }
            },
            function (error)
            {
                if (!error) {
                    console.warn("Notification successful");
                }
            }
        );


6. Valitse **Suorita kerran** alapalkissa. Saat ilmoituksen laitteessasi.

##<a name="next-steps"></a>Seuraavat vaiheet

Tämä esimerkki lähettää push-ilmoitukset kaikki iOS-laitteisiin. Kohteen tietyille käyttäjille, jotta voit viitata opetusohjelman [Käytön ilmoituksen keskittimiä push-ilmoitukset käyttäjille]. Jos haluat määritetään käyttäjien korko ryhmiin, voit lukea [Käytä ilmoituksen keskittimet lähettää uutisia]. Lisätietoja käyttämisestä ilmoituksen keskittimet [Ilmoituksen keskittimet ohjeet] ja [Ilmoituksen keskittimet toimintaohjeet iOS].


<!-- Images. -->

[213]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-create-console-app.png

[215]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-scheduler1.png
[216]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-scheduler2.png


[31]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-create-ios-app.png




<!-- URLs. -->
[Mobile Services iOS SDK]: http://go.microsoft.com/fwLink/?LinkID=266533
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253

[Mobiili-palveluiden käytön aloittaminen]: /develop/mobile/tutorials/get-started-xamarin-ios
[Azure perinteinen Portal]: https://manage.windowsazure.com/
[Ilmoitus keskittimet ohjeet]: http://msdn.microsoft.com/library/jj927170.aspx
[Ilmoitus keskittimet toimintaohjeet iOS]: http://msdn.microsoft.com/library/jj927168.aspx
[Install Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[iOS Provisioning Portal]: http://go.microsoft.com/fwlink/p/?LinkId=272456

[Ilmoitus keskittimet avulla käyttäjille push-ilmoitukset]: /manage/services/notification-hubs/notify-users-aspnet
[Ilmoitus keskittimet avulla voit lähettää uutisia]: /manage/services/notification-hubs/breaking-news-dotnet

[Paikallinen ja Push-ilmoituksen ohjelmoinnin opas]: http://developer.apple.com/library/mac/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW1
[Apple Push Notification Service]: http://go.microsoft.com/fwlink/p/?LinkId=272584

[Azure Mobile Services Component]: http://components.xamarin.com/view/azure-mobile-services/
[GitHub]: http://go.microsoft.com/fwlink/p/?LinkId=331329
[Xamarin Studio]: http://xamarin.com/download
[WindowsAzure.Messaging]: https://github.com/infosupport/WindowsAzure.Messaging.iOS
[Azure Portal]: https://portal.azure.com
