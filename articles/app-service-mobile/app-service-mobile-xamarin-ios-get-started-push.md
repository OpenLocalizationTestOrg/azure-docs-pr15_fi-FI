<properties
    pageTitle="Push-ilmoitukset lisääminen Xamarin.iOS sovelluksen Azure-sovelluksen ominaisuudet"
    description="Opettele käyttämään Azure App palvelun lähettäminen Xamarin.iOS sovelluksen push-ilmoitukset"
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="ysxu"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/12/2016"
    ms.author="yuaxu"/>

# <a name="add-push-notifications-to-your-xamarinios-app"></a>Push-ilmoitukset lisääminen Xamarin.iOS-sovellukseen

[AZURE.INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

##<a name="overview"></a>Yleiskatsaus

Tässä opetusohjelmassa Lisää push-ilmoitukset [Xamarin.iOS nopeasti Käynnistä](app-service-mobile-xamarin-ios-get-started.md) projektin niin, että push-ilmoitus lähetetään laitteen aina, kun tietue lisätään.

Jos et käytä ladatut pikaopas server-projekti, sinun on push ilmoituksen tunniste-paketti. Lisätietoja on kohdassa [.NET palvelimeen Azure-mobiilisovellukset SDK käsitteleminen](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) .

##<a name="prerequisites"></a>Edellytykset

* Suorita [Xamarin.iOS pikaopas](app-service-mobile-xamarin-ios-get-started.md) opetusohjelman.

* Fyysinen iOS-laitteessa. IOS-simulator ei tue Push-ilmoitukset.

##<a name="register-the-app-for-push-notifications-on-apples-developer-portal"></a>Rekisteröi sovelluksen push-ilmoitukset Applen developer-portaalissa

[AZURE.INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]

##<a name="configure-your-mobile-app-to-send-push-notifications"></a>Mobile-sovelluksen lähettämään push-ilmoitusten määrittäminen

[AZURE.INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

##<a name="update-the-server-project-to-send-push-notifications"></a>Päivitä push-ilmoitukset lähetetään server-projekti

[AZURE.INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

##<a name="configure-your-xamarinios-project"></a>Määritä Xamarin.iOS-projekti

[AZURE.INCLUDE [app-service-mobile-xamarin-ios-configure-project](../../includes/app-service-mobile-xamarin-ios-configure-project.md)]

##<a name="add-push-notifications-to-your-app"></a>Sovelluksen lisääminen push-ilmoitukset

1. Lisää seuraavan ominaisuuden **QSTodoService**niin, että **AppDelegate** voi hankkia mobiilisovelluksen:

            public MobileServiceClient GetClient {
            get
            {
                return client;
            }
            private set
            {
                client = value;
            }
        }

1. Lisää seuraava `using` **AppDelegate.cs** tiedoston ylhäältä-lauseen.

        using Microsoft.WindowsAzure.MobileServices;
        using Newtonsoft.Json.Linq;

2. Ohittaa **FinishedLaunching** tapahtuman **AppDelegate**:

        public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
        {
            // registers for push for iOS8
            var settings = UIUserNotificationSettings.GetSettingsForTypes(
                UIUserNotificationType.Alert
                | UIUserNotificationType.Badge
                | UIUserNotificationType.Sound,
                new NSSet());

            UIApplication.SharedApplication.RegisterUserNotificationSettings(settings);
            UIApplication.SharedApplication.RegisterForRemoteNotifications();

            return true;
        }

3. Ohittaa saman tiedoston **RegisteredForRemoteNotifications** tapahtuma. Tässä on rekisteröiminen yksinkertainen malli-ilmoituksen, joka lähetetään yli kaikki tuetut käyttöympäristöt palvelin.

    Saat lisätietoja mallien ilmoituksen keskittimet [malleja](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md).


        public override void RegisteredForRemoteNotifications(UIApplication application, NSData deviceToken)
        {
            MobileServiceClient client = QSTodoService.DefaultService.GetClient;

            const string templateBodyAPNS = "{\"aps\":{\"alert\":\"$(messageParam)\"}}";

            JObject templates = new JObject();
            templates["genericMessage"] = new JObject
            {
                {"body", templateBodyAPNS}
            };

            // Register for push with your mobile app
            var push = client.GetPush();
            push.RegisterAsync(deviceToken, templates);
        }


4. Valitse Ohita **DidReceivedRemoteNotification** tapahtuman:

        public override void DidReceiveRemoteNotification (UIApplication application, NSDictionary userInfo, Action<UIBackgroundFetchResult> completionHandler)
        {
            NSDictionary aps = userInfo.ObjectForKey(new NSString("aps")) as NSDictionary;

            string alert = string.Empty;
            if (aps.ContainsKey(new NSString("alert")))
                alert = (aps [new NSString("alert")] as NSString).ToString();

            //show alert
            if (!string.IsNullOrEmpty(alert))
            {
                UIAlertView avAlert = new UIAlertView("Notification", alert, null, "OK", null);
                avAlert.Show();
            }
        }

Sovellus on nyt päivitetty tukemaan push-ilmoitukset.

## <a name="test"></a>Sovelluksen testi push-ilmoitukset

1. Painamalla muodosta projektin ja Käynnistä sovellus iOS-laitteessa voi **suorittaa** -painiketta ja valitse sitten **OK** Hyväksy push-ilmoitukset.

    > [AZURE.NOTE] Sinun on hyväksyttävä push-ilmoitukset erikseen sovelluksestasi. Pyyntö tapahtuu vain silloin, kun sovellus käynnistyy ensimmäisen kerran.

2. -Sovellukseen, kirjoita tehtävän ja valitse sitten plus (**+**) kuvaketta.

3. Varmista, että ilmoitus on vastaanotettu ja valitse sitten **OK** hylkää ilmoituksessa.

4. Toista vaihe 2 ja välittömästi Sulje sovellus ja valitse Tarkista, että ilmoituksen mukana.

Tässä opetusohjelmassa on suoritettu onnistuneesti.

<!-- Images. -->

<!-- URLs. -->



