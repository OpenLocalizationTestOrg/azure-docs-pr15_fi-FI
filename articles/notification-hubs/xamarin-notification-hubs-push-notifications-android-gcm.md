<properties
    pageTitle="Aloita ilmoituksen keskittimet Xamarin.Android sovellusten | Microsoft Azure"
    description="Tässä opetusohjelmassa opit käyttämään Azure ilmoituksen keskittimet push-ilmoitukset lähetetään Xamarin Android-sovelluksen."
    authors="ysxu"
    manager="erikre"
    editor=""
    services="notification-hubs"
    documentationCenter="xamarin"/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-android"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

# <a name="get-started-with-notification-hubs-with-xamarin-for-android"></a>Aloita ilmoituksen lihavoituna Xamarin androidille

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

##<a name="overview"></a>Yleiskatsaus

Tässä opetusohjelmassa näytetään, miten push-ilmoitukset lähetetään Xamarin.Android sovellukseen Azure ilmoituksen keskittimet avulla.
Luo tyhjä Xamarin.Android-sovellusta, joka vastaanottaa push-ilmoitukset Google Cloud Messaging (GCM) avulla. Kun olet valmis, pystyt käyttämään yleislähetys push-ilmoituksia, jotka kaikki laitteet, joissa sovelluksen ilmoitus-toiminnossa. Valmis-koodi on käyttää [sovelluksen NotificationHubs] [ GitHub] malli.

Tässä opetusohjelmassa kuvataan yksinkertainen lähetyksen skenaarion käyttämällä ilmoituksen keskittimet.


## <a name="before-you-begin"></a>Ennen aloittamista

[AZURE.INCLUDE [notification-hubs-hero-slug](../../includes/notification-hubs-hero-slug.md)]

Tässä opetusohjelmassa valmiit koodi löytyy GitHub [tähän](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/Xamarin/GetStartedXamarinAndroid).



##<a name="prerequisites"></a>Edellytykset

Tässä opetusohjelmassa edellyttää seuraavaa:

+ Windows Xamarin Visual Studiossa tai Xamarin Studio-Mac OS X. valmis asennusohjeet ovat [asennus ja asenna Visual Studio ja Xamarin](https://msdn.microsoft.com/library/mt613162.aspx).
+ Aktiivinen Google-tiliä
+ [Azure Component viestiliikenne]
+ [Google Cloud Messaging asiakas-osa]

Noudata tässä opetusohjelmassa on kaikki muut ilmoituksen keskittimet opetusohjelmat Xamarin.Android sovellusten.

> [AZURE.IMPORTANT] Jotta voit suorittaa tässä opetusohjelmassa, on oltava aktiivinen Azure-tili. Jos sinulla ei ole tiliä, voit luoda ilmainen kokeiluversio tili vain muutaman minuutin. Lisätietoja on artikkelissa [Azure maksuttoman kokeiluversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A9C9624B5&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fpartner-xamarin-notification-hubs-android-get-started%2F).

##<a name="enable-google-cloud-messaging"></a>Google Cloud Messaging ottaminen käyttöön

[AZURE.INCLUDE [mobile-services-enable-Google-cloud-messaging](../../includes/mobile-services-enable-google-cloud-messaging.md)]

##<a name="configure-your-notification-hub"></a>Määritä ilmoitus-toiminnossa

[AZURE.INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]


<ol start="7">
<li><p><b>Määritä</b> välilehti yläreunassa, olet hankkinut edellisessä osassa <b>Ohjelmointirajapinnan avain</b> arvo ja valitse sitten <b>Tallenna</b>.</p>
</li>
</ol>
&emsp;&emsp;![](./media/notification-hubs-android-get-started/notification-hub-configure-android.png)


Ilmoitus-toiminnossa on nyt määritetty toimimaan GCM ja sinulla on yhteysmerkkijonot sekä, Rekisteröi sovelluksen ilmoituksia ja lähettää push-ilmoitukset.

##<a name="connect-your-app-to-the-notification-hub"></a>Sovelluksen yhdistäminen ilmoitus-toiminnossa

###<a name="create-a-new-project"></a>Uuden projektin luominen

1. Xamarin Studiossa **Uuden ratkaisun**, valitse **Android-sovelluksen**ja valitse **Seuraava**.

    ![](./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-project1.png)

2. Kirjoita **Sovelluksen nimi** ja **tunnus**. Valitse **Kohde Plaforms** tukevat ja valitse **Seuraava** ja **Luo**.

    ![](./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-project2.png)


    Tämä luo uuden Android projektin.

2. Avaa projektin ominaisuuksien uuden projektin ratkaisu-näkymässä hiiren kakkospainikkeella ja valitsemalla **asetukset**. Valitse kohteen **Android-sovelluksen** **luominen** -osiosta.

    Varmista, että **pakettinimi** ensimmäinen kirjain on pieni.

    > [AZURE.IMPORTANT] Pakettinimi ensimmäinen kirjain on kirjoitettava pienellä. Muussa tapauksessa näyttöön tulee luettelo sovellusvirheet, kun rekisteröidyt **BroadcastReceiver** ja **IntentFilter** push-ilmoitukset alla.

    ![](./media/partner-xamarin-notification-hubs-android-get-started/notification-hub--xamarin-android-app-options.png)


3. Myös **Pienin Android-versio** ja määrittää toisen API tasolle.

4. **Kohteen Android-version** myös määrittää toisen API-versioon, jonka haluat kohdistaa (tason on oltava API 8 tai uudempi).

Valitse **OK** ja sulje projektin asetukset-valintaikkunassa.


###<a name="add-the-required-components-to-your-project"></a>Tarvittavat komponentit lisääminen projektiin

Google Cloud Messaging asiakkaan käytettävissä Xamarin osan säilössä yksinkertaistaa tukevat push-ilmoitukset Xamarin.Android.

1. Napsauta Xamarin.Android sovelluksen osat-kansiota hiiren kakkospainikkeella ja valitse **Hae lisää osia**.

2. Etsi **Azure Messaging** -komponentin ja sen lisääminen projektiin.

3. Etsi **Google Cloud Messaging asiakas** -osan ja sen lisääminen projektiin.


###<a name="set-up-notification-hubs-in-your-project"></a>Ilmoitus keskittimet projektin määrittäminen

1. Kerää Android-sovelluksen ja ilmoitus-toiminnossa seuraavat tiedot:

    - **GoogleProjectNumber**: Pyydä projektinumero-arvoksi sovelluksen Google Developer-portaalissa yleiskatsaus. Tämän arvon aiemmin muistiin tehdä, kun loit sovelluksen portaalissa.
    - **Kuuntele yhteysmerkkijonon**: [Azure perinteinen Portal]-koontinäytössä valitsemalla **Näytä yhteyden merkkijonoja**. Kopioi tämä arvo *DefaultListenSharedAccessSignature* yhteysmerkkijono.
    - **Pääsivuston nimi**: Tämä on oman [Azure perinteinen Portal]-toiminnossa nimi. Esimerkiksi *mynotificationhub2*.

    Luo Xamarin projektin **Constants.cs** luokka ja määritä seuraavat vakion arvot luokan. Paikkamerkkien tilalle arvot.

        public static class Constants
        {
            public const string SenderID = "<GoogleProjectNumber>"; // Google API Project Number
            public const string ListenConnectionString = "<Listen connection string>";
            public const string NotificationHubName = "<hub name>";
        }

2. Lisää seuraava lauseiden **MainActivity.cs**käyttämisestä:

        using Android.Util;
        using Gcm.Client;

3. Lisää esiintymä-muuttuja `MainActivity` luokka, jota käytetään näyttämään Suojausvaroitus-valintaikkunan, kun sovellus on käynnissä:

        public static MainActivity instance;


3. Luo **MainActivity** luokan seuraavalla tavalla:

        private void RegisterWithGCM()
        {
            // Check to ensure everything's set up right
            GcmClient.CheckDevice(this);
            GcmClient.CheckManifest(this);

            // Register for push notifications
            Log.Info("MainActivity", "Registering...");
            GcmClient.Register(this, Constants.SenderID);
        }

4. : `OnCreate` **MainActivity.cs**maksutavan alusta `instance` muuttujan ja puhelun `RegisterWithGCM`:

        protected override void OnCreate (Bundle bundle)
        {
            instance = this;

            base.OnCreate (bundle);

            // Set your view from the "main" layout resource
            SetContentView (Resource.Layout.Main);

            // Get your button from the layout resource,
            // and attach an event to it
            Button button = FindViewById<Button> (Resource.Id.myButton);

            RegisterWithGCM();
        }


4. Luo uusi luokka- **MyBroadcastReceiver**.

    > [AZURE.NOTE] Selkeät **BroadcastReceiver** luokan luominen alusta alkaen alla. Nopea vaihtoehto luomista manuaalisesti **MyBroadcastReceiver.cs** on kuitenkin [NotificationHubs näytteiden]ohjelmistopakettia otoksen Xamarin.Android projektin löytyvät **GcmService.cs** tiedostossa[GitHub]. Aloita sekä niihin voi olla monistaminen **GcmService.cs** ja luokan nimien muuttaminen.

5. Lisää seuraava lauseiden **MyBroadcastReceiver.cs** (viittaaminen osa ja kokoonpanon aiemmin lisäämäsi) käyttämisestä:

        using System.Collections.Generic;
        using System.Text;
        using Android.App;
        using Android.Content;
        using Android.Util;
        using Gcm.Client;
        using WindowsAzure.Messaging;

5. Lisää seuraavat käyttöoikeuspyynnöt välillä **käyttämällä** **nimitilan** määritys **MyBroadcastReceiver.cs**:

        [assembly: Permission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "com.google.android.c2dm.permission.RECEIVE")]

        //GET_ACCOUNTS is needed only for Android versions 4.0.3 and below
        [assembly: UsesPermission(Name = "android.permission.GET_ACCOUNTS")]
        [assembly: UsesPermission(Name = "android.permission.INTERNET")]
        [assembly: UsesPermission(Name = "android.permission.WAKE_LOCK")]

6. Muuta **MyBroadcastReceiver.cs** **MyBroadcastReceiver** luokan vastaamaan seuraavasti:

        [BroadcastReceiver(Permission=Gcm.Client.Constants.PERMISSION_GCM_INTENTS)]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_MESSAGE },
            Categories = new string[] { "@PACKAGE_NAME@" })]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_REGISTRATION_CALLBACK },
            Categories = new string[] { "@PACKAGE_NAME@" })]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_LIBRARY_RETRY },
            Categories = new string[] { "@PACKAGE_NAME@" })]
        public class MyBroadcastReceiver : GcmBroadcastReceiverBase<PushHandlerService>
        {
            public static string[] SENDER_IDS = new string[] { Constants.SenderID };

            public const string TAG = "MyBroadcastReceiver-GCM";
        }

7. Voit lisätä toisen luokan **MyBroadcastReceiver.cs** nimeltä **PushHandlerService**, jotka ovat peräisin **GcmServiceBase**. Varmista, että Käytä **Service** -määritteen luokkaan:

        [Service] // Must use the service tag
        public class PushHandlerService : GcmServiceBase
        {
            public static string RegistrationID { get; private set; }
            private NotificationHub Hub { get; set; }

            public PushHandlerService() : base(Constants.SenderID)
            {
                Log.Info(MyBroadcastReceiver.TAG, "PushHandlerService() constructor");
            }
        }


8. **GcmServiceBase** toteuttaa menetelmiä **OnRegistered()**, **OnUnRegistered()**, **OnMessage()**, **OnRecoverableError()**ja **OnError()**. Microsoftin **PushHandlerService** käyttöönotto-luokka on ohittaa näistä tavoista ja näistä menetelmistä Kyselysäännön vastauksena käyttäminen ilmoitus-toiminnossa.


9. Ohittaa **PushHandlerService** **OnRegistered()** menetelmä käyttämällä seuraava koodi:

        protected override void OnRegistered(Context context, string registrationId)
        {
            Log.Verbose(MyBroadcastReceiver.TAG, "GCM Registered: " + registrationId);
            RegistrationID = registrationId;

            createNotification("PushHandlerService-GCM Registered...",
                                "The device has been Registered!");

            Hub = new NotificationHub(Constants.NotificationHubName, Constants.ListenConnectionString,
                                        context);
            try
            {
                Hub.UnregisterAll(registrationId);
            }
            catch (Exception ex)
            {
                Log.Error(MyBroadcastReceiver.TAG, ex.Message);
            }

            //var tags = new List<string>() { "falcons" }; // create tags if you want
            var tags = new List<string>() {};

            try
            {
                var hubRegistration = Hub.Register(registrationId, tags.ToArray());
            }
            catch (Exception ex)
            {
                Log.Error(MyBroadcastReceiver.TAG, ex.Message);
            }
        }

    > [AZURE.NOTE] Yllä **OnRegistered()** koodissa Huomaa mahdollisuus määrittää tekstiviesti kanavan Rekisteröidy tunnisteet.

10. Ohittaa **PushHandlerService** **OnMessage** menetelmä käyttämällä seuraava koodi:

        protected override void OnMessage(Context context, Intent intent)
        {
            Log.Info(MyBroadcastReceiver.TAG, "GCM Message Received!");

            var msg = new StringBuilder();

            if (intent != null && intent.Extras != null)
            {
                foreach (var key in intent.Extras.KeySet())
                    msg.AppendLine(key + "=" + intent.Extras.Get(key).ToString());
            }

            string messageText = intent.Extras.GetString("message");
            if (!string.IsNullOrEmpty (messageText))
            {
                createNotification ("New hub message!", messageText);
            }
            else
            {
                createNotification ("Unknown message details", msg.ToString ());
            }
        }

11. Ilmoittaa käyttäjille, kun ilmoituksen vastaanotetaan lisätä **PushHandlerService** **createNotification** ja **dialogNotify** seuraavista tavoista.

    >[AZURE.NOTE] Ilmoitus rakenne Android versiossa 5.0 ja uudempi edustaa merkittäviä lähtevien, aiempiin versioihin verrattuna. Voit kokeilla toimintoa Android 5.0 tai uudempi versio, jos sovellus on käynnissä, jotta saat ilmoituksen. Lisätietoja on artikkelissa [Android ilmoitukset](http://go.microsoft.com/fwlink/?LinkId=615880).

        void createNotification(string title, string desc)
        {
            //Create notification
            var notificationManager = GetSystemService(Context.NotificationService) as NotificationManager;

            //Create an intent to show UI
            var uiIntent = new Intent(this, typeof(MainActivity));

            //Create the notification
            var notification = new Notification(Android.Resource.Drawable.SymActionEmail, title);

            //Auto-cancel will remove the notification once the user touches it
            notification.Flags = NotificationFlags.AutoCancel;

            //Set the notification info
            //we use the pending intent, passing our ui intent over, which will get called
            //when the notification is tapped.
            notification.SetLatestEventInfo(this, title, desc, PendingIntent.GetActivity(this, 0, uiIntent, 0));

            //Show the notification
            notificationManager.Notify(1, notification);
            dialogNotify (title, desc);
        }

        protected void dialogNotify(String title, String message)
        {

            MainActivity.instance.RunOnUiThread(() => {
                AlertDialog.Builder dlg = new AlertDialog.Builder(MainActivity.instance);
                AlertDialog alert = dlg.Create();
                alert.SetTitle(title);
                alert.SetButton("Ok", delegate {
                    alert.Dismiss();
                });
                alert.SetMessage(message);
                alert.Show();
            });
        }


12. Ohita abstraktit jäsenet **OnUnRegistered()**, **OnRecoverableError()**ja **OnError()** niin, että koodisi kääntää:

        protected override void OnUnRegistered(Context context, string registrationId)
        {
            Log.Verbose(MyBroadcastReceiver.TAG, "GCM Unregistered: " + registrationId);

            createNotification("GCM Unregistered...", "The device has been unregistered!");
        }

        protected override bool OnRecoverableError(Context context, string errorId)
        {
            Log.Warn(MyBroadcastReceiver.TAG, "Recoverable Error: " + errorId);

            return base.OnRecoverableError (context, errorId);
        }

        protected override void OnError(Context context, string errorId)
        {
            Log.Error(MyBroadcastReceiver.TAG, "GCM Error: " + errorId);
        }


##<a name="run-your-app-in-the-emulator"></a>Suorita sovellus emulaattori

Jos suoritat tämän sovelluksen emulaattori, varmista, että käytössäsi Android Virtual laitteen (AVD), joka tukee Google-ohjelmointirajapinnan.

> [AZURE.IMPORTANT] Jotta push-ilmoitusten vastaanottaminen on määritettävä Google-tilin Virtual Android-laitteessa. (Emulaattori, valitse **asetukset** ja valitse **Lisää tili**.) Varmista, että emulaattori on yhteydessä Internetiin.

>[AZURE.NOTE] Ilmoitus rakenne Android versiossa 5.0 ja uudempi edustaa merkittäviä lähtevien, aiempiin versioihin verrattuna. Lisätietoja on artikkelissa [Android ilmoitukset](http://go.microsoft.com/fwlink/?LinkId=615880).


1. Valitse **Työkalut**- **Avaa Android-emulaattorin hallinta**, valitse laite ja valitse sitten **Muokkaa**.

    ![][18]

2. Valitse **Google-ohjelmointirajapinnan** **kohde**ja valitse sitten **OK**.

    ![][19]

3. Valitse ylin työkaluriviltä valitsemalla **Suorita**ja valitse sitten sovellus. Tämä käynnistää emulaattori ja suorittaa sovellus.

  Sovelluksen *registrationId* hakee GCM ja rekisteröi ilmoitus-toiminnossa.

##<a name="send-notifications-from-your-backend"></a>Ilmoitukset lähetetään oman taustasta


Voit testata ilmoitusten vastaanottaminen sovelluksen lähettämällä ilmoitukset [Azure perinteinen Portal] ilmoitus-toiminnossa Virheenkorjaus-välilehdessä alla olevassa kuvassa esitetyllä tavalla.

![][30]


Push-ilmoitukset lähetetään tavallisesti Taustajärjestelmä palvelun kuten Mobile-palveluihin tai ASP.NET yhteensopiva kirjaston kautta. Voit käyttää myös REST-Ohjelmointirajapinnalla suoraan, jos haluat lähettää ilmoitukset, jos kirjasto ei ole käytettävissä käyttäjän Taustajärjestelmä.

Seuraavassa on luettelo muiden opetusohjelmat, haluat ehkä tarkastella lähettämiseen ilmoitukset:

- ASP.NET: Katso [Käyttäjille push-ilmoitukset käyttöön ilmoituksen keskittimiä].
- Azure ilmoituksen keskittimet Java SDK: Katso [käyttämisestä ilmoituksen keskittimet Java-](notification-hubs-java-push-notification-tutorial.md) ilmoitusten lähettäminen Java. Tämä on testattu-Pimennys Android kehittämiseen.
- PHP: Katso, [miten voit käyttää ilmoituksen keskittimet PHP kohteesta](notification-hubs-php-push-notification-tutorial.md).


Opetusohjelman seuraava jaksoissa lähetettävien ilmoitusten .NET console-sovelluksen avulla tai käyttämällä mobiilipalvelu solmu komentosarjan kautta.

####<a name="optional-send-notifications-by-using-a-net-app"></a>(Valinnainen) Ilmoitukset lähetetään, .NET-sovelluksen avulla

Tässä osassa lähetämme ilmoitukset .NET console-sovelluksen avulla

1. Luo uusi Visual C# console-sovellus:

    ![][20]

2. Visual Studiossa Valitse **Työkalut**ja **NuGet paketin**hallinta **Paketin hallinta-konsolin**.

    Tämä näyttää pakettien hallinta-konsolin Visual Studiossa.

3. Pakettien hallinta konsoli-ikkunassa määrittää projektiin uuden console-sovelluksen **oletusarvon projektin** ja suorita sitten konsoli-ikkunassa seuraava komento:

        Install-Package Microsoft.Azure.NotificationHubs

    Tämä lisää <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification keskittimet NuGet paketin</a>Azure-ilmoituksen keskittimet SDK viittaus.

    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-package-manager.png)

4. Avaa Program.cs-tiedosto ja lisää seuraava `using` lause:

        using Microsoft.Azure.NotificationHubs;

5. Valitse oman `Program` luokan, Lisää seuraava menetelmä. Päivitä paikkamerkkitekstin *DefaultFullSharedAccessSignature* yhteyden merkkijono ja keskittimeen nimesi [Azure perinteinen Portal].

        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            await hub.SendGcmNativeNotificationAsync("{ \"data\" : {\"message\":\"Hello from Azure!\"}}");
        }

6. Lisää seuraavat rivit **Main** -menetelmää:

         SendNotificationAsync();
         Console.ReadLine();

7. Suorita sovellus F5-näppäintä. Saat ilmoituksen-sovelluksessa.

    ![][21]

####<a name="optional-send-notifications-by-using-a-mobile-service"></a>(Valinnainen) Ilmoitukset lähetetään käyttämällä mobile-palvelu

1. Noudata [Mobile palveluiden käytön aloittaminen].

1. Kirjautuminen [Perinteinen Azure-portaaliin]ja valitse mobile-palvelu.

2. Valitse sivun **ajoitus** -välilehti.

    ![][22]

3. Luo uuden ajoitetun työn, Lisää nimi ja valitse **pyydettäessä**.

    ![][23]

4. Kun työ on luotu, valitse työn nimi. Valitse yläpalkissa **komentosarja** -välilehti.

5. Lisää seuraavaa komentosarjaa ajoitus-funktion sisällä. Varmista, että Korvaa paikkamerkit ilmoitus-toiminnossa nimesi ja yhteysmerkkijonon *DefaultFullSharedAccessSignature* , jotka olet hankkinut aiemmin varten. Valitse **Tallenna**.

        var azure = require('azure');
        var notificationHubService = azure.createNotificationHubService('<hub name>', '<connection string>');
        notificationHubService.gcm.send(null,'{"data":{"message" : "Hello from Mobile Services!"}}',
          function (error)
          {
            if (!error) {
               console.warn("Notification successful");
            }
            else
            {
              console.warn("Notification failed" + error);
            }
          }
        );

6. Valitse **Suorita kerran** alapalkissa. Saapuvan ilmoitus tulee näyttöön.

##<a name="next-steps"></a>Seuraavat vaiheet

Tämä esimerkki lähettää ilmoitukset kaikkien Android-laitteiden. Kohteen tietyille käyttäjille, jotta voit viitata opetusohjelman [Käytön ilmoituksen keskittimiä push-ilmoitukset käyttäjille]. Jos haluat määritetään käyttäjien korko ryhmiin, voit lukea [Käytä ilmoituksen keskittimet lähettää uutisia]. Lisätietoja käyttämisestä ilmoituksen keskittimet [Ilmoituksen keskittimet ohjeet] ja [Ilmoituksen keskittimet vaiheittaiset androidille].

<!-- Anchors. -->
[Enable Google Cloud Messaging]: #register
[Configure your Notification Hub]: #configure-hub
[Connecting your app to the Notification Hub]: #connecting-app
[Run your app with the emulator]: #run-app
[Send notifications from your back-end]: #send
[Next steps]:#next-steps

<!-- Images. -->

[11]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-configure-android.png

[13]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-app1.png
[15]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-app3.png

[18]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-android-app7.png
[19]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-android-app8.png

[20]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-console-app.png
[21]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-android-toast.png
[22]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-scheduler1.png
[23]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-scheduler2.png

[30]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hubs-debug-hub-gcm.png


<!-- URLs. -->
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[Mobiili-palveluiden käytön aloittaminen]: /develop/mobile/tutorials/get-started-xamarin-android/#create-new-service
[JavaScript and HTML]: /develop/mobile/tutorials/get-started-with-push-js

[Azure perinteinen Portal]: https://manage.windowsazure.com/
[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
[Ilmoitus keskittimet ohjeet]: http://msdn.microsoft.com/library/jj927170.aspx
[Ilmoitus keskittimet vaiheittaiset androidille]: http://msdn.microsoft.com/library/dn282661.aspx

[Ilmoitus keskittimet avulla käyttäjille push-ilmoitukset]: /manage/services/notification-hubs/notify-users-aspnet
[Ilmoitus keskittimet avulla voit lähettää uutisia]: /manage/services/notification-hubs/breaking-news-dotnet
[GCMClient Component page]: http://components.xamarin.com/view/GCMClient
[Xamarin.NotificationHub GitHub page]: https://github.com/SaschaDittmann/Xamarin.NotificationHub
[GitHub]: http://go.microsoft.com/fwlink/p/?LinkId=331329
[Google Cloud Messaging asiakas-osa]: http://components.xamarin.com/view/GCMClient/
[Azure Component viestiliikenne]: http://components.xamarin.com/view/azure-messaging
