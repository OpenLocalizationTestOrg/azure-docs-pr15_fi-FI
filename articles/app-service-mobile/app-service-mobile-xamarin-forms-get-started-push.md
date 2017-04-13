<properties
    pageTitle="Push-ilmoitukset lisääminen Xamarin.Forms sovelluksen | Microsoft Azure"
    description="Lue, miten monen käyttöympäristön push-ilmoitukset lähetetään Xamarin.Forms sovelluksia Azure-palveluiden avulla."
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="ysxu"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/12/2016"
    ms.author="yuaxu"/>

# <a name="add-push-notifications-to-your-xamarinforms-app"></a>Push-ilmoitukset lisääminen Xamarin.Forms-sovellukseen

[AZURE.INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

##<a name="overview"></a>Yleiskatsaus

Tässä opetusohjelmassa Lisää push-ilmoitukset käyttöön kaikissa projekteissa tuloksena [Xamarin.Forms nopeasti Käynnistä-painiketta](app-service-mobile-xamarin-forms-get-started.md) , niin, että push-ilmoitus lähetetään kaikille Office kaikissa ympäristöissä asiakkaille aina, kun tietue lisätään.

Jos et käytä ladatut pikaopas server-projekti, sinun on push ilmoituksen tunniste-paketti. Lisätietoja on kohdassa [.NET palvelimeen Azure-mobiilisovellukset SDK käsitteleminen](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) .

##<a name="prerequisites"></a>Edellytykset

* IOS-on [Apple kehittäjä-ohjelman jäsenyyden](https://developer.apple.com/programs/ios/) ja fyysinen iOS-laitteen koska [iOS simulator ei tue push-ilmoitukset](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/iOS_Simulator_Guide/TestingontheiOSSimulator.html).

##<a name="configure-hub"></a>Määritä ilmoitus-toiminnossa

[AZURE.INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

##<a name="update-the-server-project-to-send-push-notifications"></a>Päivitä push-ilmoitukset lähetetään server-projekti

[AZURE.INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]


##<a name="optional-configure-and-run-the-android-project"></a>(Valinnainen) Määritä ja suorita Android projekti

Voit ottaa käyttöön push-ilmoitusten Xamarin.Forms Droid projektin Android suorittaminen


###<a name="enable-firebase-cloud-messaging-fcm"></a>Ota käyttöön Firebase Cloud Messaging (FCM)

[AZURE.INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

###<a name="configure-the-mobile-app-backend-to-send-push-requests-using-fcm"></a>Määritä Lähetä push-pyyntöjen FCM Mobile-sovelluksen taustaan

[AZURE.INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push.md)]

###<a name="add-push-notifications-to-the-android-project"></a>Voit lisätä Android projektiin push-ilmoitukset

Taustatietokannan, jolle FCM on määritetty emme voit lisätä osia ja koodit asiakas voi rekisteröidä FCM, Rekisteröidy push-ilmoitukset ja Azure ilmoituksen keskittimet mobiilisovelluksen Taustajärjestelmä kautta ja ilmoituksia.

1. **Droid** Projectissa Napsauta **osat** -kansiota hiiren kakkospainikkeella, valitsemalla **Hae lisää osia...**, Etsi **Google Cloud Messaging asiakas** -osa ja lisääminen projektiin. Tämä osa tukee Xamarin Android-projektin push-ilmoitukset.


2. Avaa projektitiedoston MainActivity.cs ja lisää seuraava lauseella tiedoston yläosassa:

        using Gcm.Client;

3. Lisää seuraava koodi **OnCreate** menetelmä **LoadApplication**puhelu jälkeen:

        try
        {
            // Check to ensure everything's setup right
            GcmClient.CheckDevice(this);
            GcmClient.CheckManifest(this);

            // Register for push notifications
            System.Diagnostics.Debug.WriteLine("Registering...");
            GcmClient.Register(this, PushHandlerBroadcastReceiver.SENDER_IDS);
        }
        catch (Java.Net.MalformedURLException)
        {
            CreateAndShowDialog("There was an error creating the client. Verify the URL.", "Error");
        }
        catch (Exception e)
        {
            CreateAndShowDialog(e.Message, "Error");
        }


4. Lisää uusi **CreateAndShowDialog** helper menetelmä seuraavasti:

        private void CreateAndShowDialog(String message, String title)
        {
            AlertDialog.Builder builder = new AlertDialog.Builder(this);

            builder.SetMessage (message);
            builder.SetTitle (title);
            builder.Create().Show ();
        }


5. Lisää seuraava koodi **MainActivity** luokkaan:

        // Create a new instance field for this activity.
        static MainActivity instance = null;

        // Return the current activity instance.
        public static MainActivity CurrentActivity
        {
            get
            {
                return instance;
            }
        }

    Tämä paljastaa **MainActivity** nykyisen esiintymän, joten voit suorittaa tärkeimmät Käyttöliittymän viestiketjun käyttöön.

6. Alusta `instance`, muuttujan **OnCreate** -menetelmä alkuun toimimalla seuraavasti.

        // Set the current instance of MainActivity.
        instance = this;

2. Lisää uusi luokka-tiedosto nimeltä **Droid** projektiin `GcmService.cs`, ja varmista, että seuraavat **käyttämällä** lauseet ovat näkyvissä tiedoston yläosassa:

        using Android.App;
        using Android.Content;
        using Android.Media;
        using Android.Support.V4.App;
        using Android.Util;
        using Gcm.Client;
        using Microsoft.WindowsAzure.MobileServices;
        using Newtonsoft.Json.Linq;
        using System;
        using System.Collections.Generic;
        using System.Diagnostics;
        using System.Text;


9. Lisää seuraavat käyttöoikeuspyynnöt tiedoston yläosassa **käyttämällä** lauseet jälkeen ja ennen **nimitilan** määritys.

        [assembly: Permission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "com.google.android.c2dm.permission.RECEIVE")]
        [assembly: UsesPermission(Name = "android.permission.INTERNET")]
        [assembly: UsesPermission(Name = "android.permission.WAKE_LOCK")]
        //GET_ACCOUNTS is only needed for android versions 4.0.3 and below
        [assembly: UsesPermission(Name = "android.permission.GET_ACCOUNTS")]

10. Lisää seuraavat luokan määritys nimitilan. 

        [BroadcastReceiver(Permission = Gcm.Client.Constants.PERMISSION_GCM_INTENTS)]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_MESSAGE }, Categories = new string[] { "@PACKAGE_NAME@" })]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_REGISTRATION_CALLBACK }, Categories = new string[] { "@PACKAGE_NAME@" })]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_LIBRARY_RETRY }, Categories = new string[] { "@PACKAGE_NAME@" })]
        public class PushHandlerBroadcastReceiver : GcmBroadcastReceiverBase<GcmService>
        {
            public static string[] SENDER_IDS = new string[] { "<PROJECT_NUMBER>" };
        }

    >[AZURE.NOTE]Korvaa **< PROJECT_NUMBER >** kirjoittamasi projektin puhelinnumero.   

11. Korvaa tyhjä **GcmService** luokan seuraava koodi, joka käyttää uuden lähetyksen vastaanottaja:

         [Service]
         public class GcmService : GcmServiceBase
         {
             public static string RegistrationID { get; private set; }

             public GcmService()
                 : base(PushHandlerBroadcastReceiver.SENDER_IDS){}
         }


12. Lisää seuraava koodi, joka ohittaa **OnRegistered** tapahtumakäsittelijä ja toteuttaa **rekisteröidä** menetelmän **GcmService** luokkaan.

        protected override void OnRegistered(Context context, string registrationId)
        {
            Log.Verbose("PushHandlerBroadcastReceiver", "GCM Registered: " + registrationId);
            RegistrationID = registrationId;

            var push = TodoItemManager.DefaultManager.CurrentClient.GetPush();

            MainActivity.CurrentActivity.RunOnUiThread(() => Register(push, null));
        }

        public async void Register(Microsoft.WindowsAzure.MobileServices.Push push, IEnumerable<string> tags)
        {
            try
            {
                const string templateBodyGCM = "{\"data\":{\"message\":\"$(messageParam)\"}}";

                JObject templates = new JObject();
                templates["genericMessage"] = new JObject
                {
                    {"body", templateBodyGCM}
                };

                await push.RegisterAsync(RegistrationID, templates);
                Log.Info("Push Installation Id", push.InstallationId.ToString());
            }
            catch (Exception ex)
            {
                System.Diagnostics.Debug.WriteLine(ex.Message);
                Debugger.Break();
            }
        }

        Note that this code uses the `messageParam` parameter in the template registration. 

13. Lisää seuraava koodi, joka sisältää **OnMessage**: 

        protected override void OnMessage(Context context, Intent intent)
        {
            Log.Info("PushHandlerBroadcastReceiver", "GCM Message Received!");

            var msg = new StringBuilder();

            if (intent != null && intent.Extras != null)
            {
                foreach (var key in intent.Extras.KeySet())
                    msg.AppendLine(key + "=" + intent.Extras.Get(key).ToString());
            }

            //Store the message
            var prefs = GetSharedPreferences(context.PackageName, FileCreationMode.Private);
            var edit = prefs.Edit();
            edit.PutString("last_msg", msg.ToString());
            edit.Commit();

            string message = intent.Extras.GetString("message");
            if (!string.IsNullOrEmpty(message))
            {
                createNotification("New todo item!", "Todo item: " + message);
                return;
            }

            string msg2 = intent.Extras.GetString("msg");
            if (!string.IsNullOrEmpty(msg2))
            {
                createNotification("New hub message!", msg2);
                return;
            }

            createNotification("Unknown message details", msg.ToString());
        }

        void createNotification(string title, string desc)
        {
            //Create notification
            var notificationManager = GetSystemService(Context.NotificationService) as NotificationManager;

            //Create an intent to show ui
            var uiIntent = new Intent(this, typeof(MainActivity));

            //Use Notification Builder
            NotificationCompat.Builder builder = new NotificationCompat.Builder(this);

            //Create the notification
            //we use the pending intent, passing our ui intent over which will get called
            //when the notification is tapped.
            var notification = builder.SetContentIntent(PendingIntent.GetActivity(this, 0, uiIntent, 0))
                    .SetSmallIcon(Android.Resource.Drawable.SymActionEmail)
                    .SetTicker(title)
                    .SetContentTitle(title)
                    .SetContentText(desc)

                    //Set the notification sound
                    .SetSound(RingtoneManager.GetDefaultUri(RingtoneType.Notification))

                    //Auto cancel will remove the notification once the user touches it
                    .SetAutoCancel(true).Build();

            //Show the notification
            notificationManager.Notify(1, notification);
        }

    Tämä käsittelee saapuvia ilmoitukset ja lähetä ne näytetään ilmoitus-hallinta.

14. **GcmServiceBase** pidentämiseksi sinun on otettava käyttöön **OnUnRegistered** ja **VirheenYhteydessä** käsittelijä menetelmät, voit tehdä sen seuraavasti:

        protected override void OnUnRegistered(Context context, string registrationId)
        {
            Log.Error("PushHandlerBroadcastReceiver", "Unregistered RegisterationId : " + registrationId);
        }

        protected override void OnError(Context context, string errorId)
        {
            Log.Error("PushHandlerBroadcastReceiver", "GCM Error: " + errorId);
        }

Olet nyt valmiina testi push-ilmoitukset Android-laitteessa tai emulaattori-sovelluksessa.

###<a name="test-push-notifications-in-your-android-app"></a>Testaa push-ilmoitukset Android-sovelluksessa

Kaksi ensimmäistä vaihetta vaaditaan vain, kun testaus emulointiohjelma.

1. Varmista, että ottaminen käyttöön tai virheenkorjaus virtual laitteessa, joka on määritetty kohde, Google-ohjelmointirajapinnan Android Virtual laitteen (AVD) Managerissa tuoteluokkaryhmien.

2. Google-tilin lisääminen Android-laitteen **sovelluksia**valitsemalla > **asetukset** > **Lisää tili**ja valitse seuraa kehotuksia käyttämään aiemmin Google-tilin lisääminen laitteeseen, jotta voit luoda uuden.

1. Visual Studiossa tai Xamarin Studio **Droid** projektin napsauttamalla hiiren kakkospainikkeella ja valitse **Aseta käynnistys projektin**.

2. Painamalla muodosta projektin ja Käynnistä sovellus Android-laitteen tai emulaattorin **Suorita** -painiketta.

3. -Sovellukseen, kirjoita tehtävän ja valitse sitten plus (**+**) kuvaketta.

4. Varmista, että ilmoituksen vastaanotetaan, kun kohde on lisätty.


##<a name="optional-configure-and-run-the-ios-project"></a>(Valinnainen) Määritä ja suorita iOS-projekti

Tässä osassa on käynnissä iOS-laitteiden Xamarin iOS-projekti. Voit ohittaa tämän osan, jos käsittelet ei iOS-laitteita.

[AZURE.INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]


####<a name="configure-the-notification-hub-for-apns"></a>Ilmoitus-toiminnossa määrittäminen APN

[AZURE.INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

Seuraavaksi määritetään Xamarin Studiossa tai Visual Studio iOS project-asetus.

[AZURE.INCLUDE [app-service-mobile-xamarin-ios-configure-project](../../includes/app-service-mobile-xamarin-ios-configure-project.md)]


####<a name="add-push-notifications-to-your-ios-app"></a>Push-ilmoitukset lisääminen iOS-sovellukseen

1. **IOS** Projectissa, Avaa AppDelegate.cs lisäämällä seuraavan **käyttäminen** lauseen kooditiedoston alkuun.

        using Newtonsoft.Json.Linq;

4. Lisää **AppDelegate** -luokan ohitus **RegisteredForRemoteNotifications** tapahtuman Rekisteröidy ilmoitukset:

        public override void RegisteredForRemoteNotifications(UIApplication application, 
            NSData deviceToken)
        {
            const string templateBodyAPNS = "{\"aps\":{\"alert\":\"$(messageParam)\"}}";

            JObject templates = new JObject();
            templates["genericMessage"] = new JObject
                {
                  {"body", templateBodyAPNS}
                };

            // Register for push with your mobile app
            Push push = TodoItemManager.DefaultManager.CurrentClient.GetPush();
            push.RegisterAsync(deviceToken, templates);
        }

5. Lisää seuraavat ohitus **DidReceivedRemoteNotification** tapahtumakäsittelijän myös **AppDelegate**:

        public override void DidReceiveRemoteNotification(UIApplication application, 
            NSDictionary userInfo, Action<UIBackgroundFetchResult> completionHandler)
        {
            NSDictionary aps = userInfo.ObjectForKey(new NSString("aps")) as NSDictionary;

            string alert = string.Empty;
            if (aps.ContainsKey(new NSString("alert")))
                alert = (aps[new NSString("alert")] as NSString).ToString();

            //show alert
            if (!string.IsNullOrEmpty(alert))
            {
                UIAlertView avAlert = new UIAlertView("Notification", alert, null, "OK", null);
                avAlert.Show();
            }
        }

    Tämä menetelmä käsittelee saapuvia ilmoitukset sovellus on käynnissä.

2. Lisää seuraava koodi **AppDelegate** -luokan **FinishedLaunching** tapaan: 

        // Register for push notifications.
        var settings = UIUserNotificationSettings.GetSettingsForTypes(
            UIUserNotificationType.Alert
            | UIUserNotificationType.Badge
            | UIUserNotificationType.Sound,
            new NSSet());

        UIApplication.SharedApplication.RegisterUserNotificationSettings(settings);
        UIApplication.SharedApplication.RegisterForRemoteNotifications();

    Näin remote-ilmoitusten tuki ja pyynnöt push rekisteröinti.

Sovellus on nyt päivitetty tukemaan push-ilmoitukset.

####<a name="test-push-notifications-in-your-ios-app"></a>Testaa push-ilmoitukset iOS-sovelluksessa

1. IOS-projektin napsauttamalla hiiren kakkospainikkeella ja valitse **Aseta StartPp projektin**.

2. Painamalla **F5-näppäintä** ja **Suorita** -painike Visual Studiossa voivat laatia projektin ja Käynnistä sovellus iOS-laitteessa ja valitse sitten **OK** Hyväksy push-ilmoitukset.

    > [AZURE.NOTE] Sinun on hyväksyttävä push-ilmoitukset erikseen sovelluksestasi. Pyyntö tapahtuu vain silloin, kun sovellus käynnistyy ensimmäisen kerran.

3. -Sovellukseen, kirjoita tehtävän ja valitse sitten plus (**+**) kuvaketta.

4. Varmista, että ilmoitus on vastaanotettu ja valitse sitten **OK** hylkää ilmoituksessa.


##<a name="optional-configure-and-run-the-windows-projects"></a>(Valinnainen) Määritä ja suorita Windows-projektit

Tässä osassa on Xamarin.Forms WinApp ja Windows-laitteiden WinPhone81 projektit. Näitä ohjeita tukea myös yleinen Windows Platform (UWP) projektit. Voit ohittaa tämän osan Jos eivät toimi Windows-laitteiden kanssa.


####<a name="register-your-windows-app-for-push-notifications-with-wns"></a>Voit rekisteröidä WNS Windows-sovellus push-ilmoitukset

[AZURE.INCLUDE [app-service-mobile-register-wns](../../includes/app-service-mobile-register-wns.md)]


####<a name="configure-the-notification-hub-for-wns"></a>Ilmoitus-toiminnossa määrittäminen WNS

[AZURE.INCLUDE [app-service-mobile-configure-wns](../../includes/app-service-mobile-configure-wns.md)]


####<a name="add-push-notifications-to-your-windows-app"></a>Push-ilmoitusten lisääminen Windows-sovellus

1. Visual Studiossa Avaa Windows projektin **App.xaml.cs** ja lisää seuraavat **käyttämällä** lauseet.

        using Newtonsoft.Json.Linq;
        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;
        using Windows.Networking.PushNotifications;
        using <your_TodoItemManager_portable_class_namespace>;

    Korvaa `<your_TodoItemManager_portable_class_namespace>` kannettavat projektin, joka sisältää nimitilan `TodoItemManager` luokka.
 

2. Lisää App.xaml.cs **InitNotificationsAsync** seuraavasti: 

        private async Task InitNotificationsAsync()
        {
            var channel = await PushNotificationChannelManager
                .CreatePushNotificationChannelForApplicationAsync();

            const string templateBodyWNS = 
                "<toast><visual><binding template=\"ToastText01\"><text id=\"1\">$(messageParam)</text></binding></visual></toast>";

            JObject headers = new JObject();
            headers["X-WNS-Type"] = "wns/toast";

            JObject templates = new JObject();
            templates["genericMessage"] = new JObject
            {
                {"body", templateBodyWNS},
                {"headers", headers} // Needed for WNS.
            };

            await TodoItemManager.DefaultManager.CurrentClient.GetPush()
                .RegisterAsync(channel.Uri, templates);
        }

    Tämä menetelmä saa ilmoituksen push-kanavan ja rekisteröi mallin mallin ilmoituksia ilmoitus-toiminnosta. Malli-ilmoitus, joka tukee *messageParam* lähetetään asiakas.

3. Päivitä **OnLaunched** tapahtuman käsittelijä menetelmä määritystä App.xaml.cs, lisäämällä `async` määrite, Lisää seuraava rivi koodin menetelmä lopussa: 

        await InitNotificationsAsync();

    Näin varmistat, että push-ilmoitusten rekisteröinnissä luodaan tai päivitetään aina, kun sovellus käynnistetään. On tärkeää Näin voit varmistaa, että WNS push-kanavan on aina aktiivinen.  

4. Napsauta ratkaisunhallinnassa Visual Studio **Package.appxmanifest** -tiedoston avaaminen ja määritä **Saapuvan pystyvät** **Kyllä** **ilmoitukset**-kohdassa.

5. Muodosta sovellus ja tarkista sinulla on virheitä.  Asiakkaan sovelluksen pitäisi nyt Rekisteröidy taustasta Mobile-sovelluksen malli-ilmoitukset. Toista jokaisen Windows projektin ratkaisu-sisältö.


####<a name="test-push-notifications-in-your-windows-app"></a>Testaa push-ilmoitukset Windows-sovelluksessa

1. Visual Studiossa Windows projektin napsauttamalla hiiren kakkospainikkeella ja valitse **Aseta käynnistys projektin**.

2. Paina voivat laatia projektin ja Käynnistä sovellus **Suorita** -painiketta.

3. -Sovellukseen, kirjoita uusi todoitem nimi ja valitse sitten plus (**+**) kuvaketta, jos haluat lisätä sen.

4. Varmista, että ilmoituksen vastaanotetaan, kun kohde on lisätty.

##<a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja push-ilmoitukset:

* [Push-ilmoituksen vianmääritys](../notification-hubs/notification-hubs-push-notification-fixer.md)  
On useita syitä, miksi ilmoitukset Hae pudotettu tai lopu laitteisiin. Tässä ohjeaiheessa esitellään, miten haluat analysoida ja push ilmoituksen virheet pääkansion syy voi selvittää. 

Harkitse jatkuvaa voin jompikumpi seuraavista opetusohjelmat:

* [Käyttöoikeuksien lisääminen sovelluksen](app-service-mobile-xamarin-forms-get-started-users.md)  
Opettele todennetaan Liittoutuminen sovelluksesi käyttäjät.

* [Sovelluksen offline-synkronoinnin ottaminen käyttöön](app-service-mobile-xamarin-forms-get-started-offline-data.md)  
  Opettele lisäämään offline-tuki käyttämällä Mobile-sovelluksen Taustajärjestelmä sovelluksen. Offline-synkronoinnin sallii loppukäyttäjien kanssa mobiilisovelluksessa&mdash;tarkasteleminen, lisääminen tai muokkaaminen tietojen&mdash;myös silloin, kun yhteyttä ei ole verkossa.

<!-- Images. -->

<!-- URLs. -->
[Install Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[Xcode]: https://go.microsoft.com/fwLink/?LinkID=266532
[apns object]: http://go.microsoft.com/fwlink/p/?LinkId=272333

