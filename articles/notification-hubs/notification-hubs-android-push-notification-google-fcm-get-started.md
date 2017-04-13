<properties
    pageTitle="Push-ilmoitukset lähetetään Azure ilmoituksen keskittimet ja Firebase Cloud Messaging Android | Microsoft Azure"
    description="Tässä opetusohjelmassa opit käyttämään Azure ilmoituksen keskittimet ja Firebase Cloud Messaging Android-laitteiden push-ilmoitukset."
    services="notification-hubs"
    documentationCenter="android"
    keywords="Push-ilmoitukset, push-ilmoituksen, android push-ilmoituksen, fcm, firebase cloud viestintä"
    authors="ysxu"
    manager="erikre"
    editor=""/>
<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="hero-article"
    ms.date="07/14/2016"
    ms.author="yuaxu"/>

# <a name="sending-push-notifications-to-android-with-azure-notification-hubs"></a>Push-ilmoitukset lähetetään Android-laitteeseen Azure ilmoituksen keskittimet

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

##<a name="overview"></a>Yleiskatsaus

> [AZURE.IMPORTANT] Tässä ohjeaiheessa esitellään push-ilmoitukset ja Google Firebase Cloud Messaging (FCM). Jos käytössäsi on edelleen Google Cloud Messaging (GCM), katso [lähettäminen push-ilmoitukset, Android-laitteeseen Azure ilmoituksen keskittimet ja GCM](notification-hubs-android-push-notification-google-gcm-get-started.md).

Tässä opetusohjelmassa näytetään, miten Azure ilmoituksen keskittimet ja Firebase Cloud Messaging push-ilmoitukset lähetetään Android-sovelluksen avulla.
Luo tyhjä Android-sovelluksen, joka vastaanottaa push-ilmoitukset Firebase Cloud Messaging (FCM) avulla.



[AZURE.INCLUDE [notification-hubs-hero-slug](../../includes/notification-hubs-hero-slug.md)]

Tässä opetusohjelmassa valmiit koodi voi ladata GitHub [tähän](https://github.com/Azure/azure-notificationhubs-samples/tree/master/Android/GetStartedFirebase).


##<a name="prerequisites"></a>Edellytykset

> [AZURE.IMPORTANT] Jotta voit suorittaa tässä opetusohjelmassa, on oltava aktiivinen Azure-tili. Jos sinulla ei ole tiliä, voit luoda ilmainen kokeiluversio tili vain muutaman minuutin. Lisätietoja on artikkelissa [Azure maksuttoman kokeiluversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-android-get-started).

- Aktiivinen Azure tili edellä mainituista, lisäksi tässä opetusohjelmassa edellyttää [Android Studio](http://go.microsoft.com/fwlink/?LinkId=389797)uusimman version.
- Android 2.3 tai uudempi Firebase Cloud viestiliikenteen.
- Google-tietovarasto version 27 tai uudempi versio vaaditaan Firebase Cloud Messaging.
- Google Play-palvelujen 9.0.2 vähintään Firebase Cloud viestiliikenteen.
- Noudata tässä opetusohjelmassa on Android-sovellusten kaikkiin muihin ilmoituksen keskittimet opetusohjelmiin.


##<a name="create-a-new-android-studio-project"></a>Luo uusi Android Studio-projekti

1. Android Studiossa Käynnistä Android Studio uusi projekti.

    ![Android Studio - uusi projekti](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-android-studio-new-project.png)

2. Valitse **puhelimen ja Taulutietokoneen** lomakkeen kerroin ja **Pienin SDK-paketissa** , jonka haluat tukea. Valitse sitten **Seuraava**.

    ![Android Studio - projektin luominen työnkulun](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-android-studio-choose-form-factor.png)

3. Valitse **Tyhjä tehtävän** tärkeimmät tehtävän, valitse **Seuraava**ja valitse sitten **Valmis**.


##<a name="create-a-project-that-supports-firebase-cloud-messaging"></a>Luo projekti, joka tukee Firebase Cloud viestintä

[AZURE.INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]


##<a name="configure-a-new-notification-hub"></a>Määritä uusi ilmoitus-toiminnossa

[AZURE.INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

&emsp;&emsp;6. Valitse ilmoitus-toiminnossa **asetukset** -sivu Valitse **Ilmoituksen Services** ja **Google (GCM)**. Kirjoita kopioitu aiemmin [Firebase konsolin](https://firebase.google.com/console/) FCM server-näppäintä ja valitse **Tallenna**.

&emsp;&emsp;![Azure ilmoituksen keskittimet - Google (GCM)](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-gcm-api.png)

Ilmoitus-toiminnossa on nyt määritetty toimimaan Firebase Cloud Messagin ja sinulla on yhteysmerkkijonot sekä, Rekisteröi sovelluksen push-ilmoitusten lähettämisessä ja vastaanottamisessa.

##<a id="connecting-app"></a>Sovelluksen yhdistäminen ilmoitus-toiminnossa

###<a name="add-google-play-services-to-the-project"></a>Voit lisätä projektiin Google Play-palvelut

[AZURE.INCLUDE [Add Play Services](../../includes/notification-hubs-android-studio-add-google-play-services.md)]

###<a name="adding-azure-notification-hubs-libraries"></a>Azure ilmoituksen keskittimet kirjastojen lisääminen


1. Valitse `Build.Gradle` **app**tiedoston, Lisää seuraavat rivit **riippuvuudet** -osassa.

        compile 'com.microsoft.azure:notification-hubs-android-sdk:0.4@aar'
        compile 'com.microsoft.azure:azure-notifications-handler:1.0.1@aar'

2. Lisää seuraavat säilöön **riippuvuudet** osan jälkeen.

        repositories {
            maven {
                url "http://dl.bintray.com/microsoftazuremobile/SDK"
            }
        }

### <a name="updating-the-androidmanifestxml"></a>Päivitetään AndroidManifest.xml.


1. Tukemaan FCM olemme on pantava Esiintymätunnus kuuntelupalvelu koodia koon muuttamiseen tarkoitettu [hankkia rekisteröinti tunnusten](https://firebase.google.com/docs/cloud-messaging/android/client#sample-register) [Googlen FirebaseInstanceId Ohjelmointirajapinnan](https://firebase.google.com/docs/reference/android/com/google/firebase/iid/FirebaseInstanceId)käyttäminen. Tässä opetusohjelmassa on nimetä luokan `MyInstanceIDService`. 
 
    Lisää seuraavan Palvelumääritelmän sisäpuolella AndroidManifest.xml-tiedostoon `<application>` tunniste. 

        <service android:name=".MyInstanceIDService">
            <intent-filter>
                <action android:name="com.google.firebase.INSTANCE_ID_EVENT"/>
            </intent-filter>
        </service>



2. Kun sinulta on vastaanotettu meidän FCM rekisteröinti tunnuksen FirebaseInstanceId-Ohjelmointirajapinta, Käytämme sen rekisteröityä [Azure ilmoitus-toiminnossa](notification-hubs-push-notification-registration-management.md). Sovellus tukee rekisteröinnin taustan käyttämällä `IntentService` nimeltä `RegistrationIntentService`. Tämä palvelu myös on vastuussa päivittäminen Microsoftin FCM rekisteröinti tunnuksen.
 
    Lisää seuraavan Palvelumääritelmän sisäpuolella AndroidManifest.xml-tiedostoon `<application>` tunniste. 

        <service
            android:name=".RegistrationIntentService"
            android:exported="false">
        </service>



3. On myös määrittää vastaanottaja, saat ilmoituksen. Lisää seuraavan vastaanotin määritelmän sisäpuolella AndroidManifest.xml-tiedostoon `<application>` tunniste. Korvaa `<your package>` paikkamerkki ja todellinen paketin nimesi näkyy yläreunassa `AndroidManifest.xml` tiedosto.

        <receiver android:name="com.microsoft.windowsazure.notifications.NotificationsBroadcastReceiver"
            android:permission="com.google.android.c2dm.permission.SEND">
            <intent-filter>
                <action android:name="com.google.android.c2dm.intent.RECEIVE" />
                <category android:name="<your package name>" />
            </intent-filter>
        </receiver>



4. Lisää seuraava tarvittavat FCM liittyviä oikeuksia alla `</application>` tunniste. Varmista, että korvaa `<your package>` yläreunassa näkyy paketin nimi `AndroidManifest.xml` tiedosto.

    Saat lisätietoja näiden oikeuksien [Android GCM asiakas-sovelluksen asennuksen](https://developers.google.com/cloud-messaging/android/client#manifest) ja [Siirrä GCM asiakas-sovelluksen Android Firebase Cloud Messaging](https://developers.google.com/cloud-messaging/android/android-migrate-fcm#remove_the_permissions_required_by_gcm).

        <uses-permission android:name="android.permission.INTERNET"/>
        <uses-permission android:name="android.permission.GET_ACCOUNTS"/>
        <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />


### <a name="adding-code"></a>Koodin lisääminen


1. Laajenna Project-näkymä **sovelluksen** > **src** > **tärkeimmät** > **java**. Paketti-kansiosi **java**-kohdassa hiiren kakkospainikkeella, valitse **Uusi**ja valitse sitten **Java-luokan**. Lisää uusi luokka nimeltä `NotificationSettings`. 

    ![Android Studio - uusi Java-luokka](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hub-android-new-class.png)

    Varmista, että päivität nämä kolme seuraava koodi paikkamerkit `NotificationSettings` luokan:
    * **SenderId**: olet hankkinut aiemmassa [Firebase console](https://firebase.google.com/console/)-projektin asetusten **Cloud viestintä** -välilehdessä Lähettäjätunnus.
    * **HubListenConnectionString**: oman keskittimeen **DefaultListenAccessSignature** yhteysmerkkijono. Voit kopioida yhteysmerkkijonon, valitsemalla **Käytäntöjen** oman [Azure Portal]-toiminnossa **asetukset** -sivu.
    * **HubName**: Käytä omaa ilmoitus-keskittimeen, joka näkyy [Azure Portal]-toiminnossa-sivu.

    `NotificationSettings`koodi:

        public class NotificationSettings {
            public static String SenderId = "<Your project number>";
            public static String HubName = "<Your HubName>";
            public static String HubListenConnectionString = "<Enter your DefaultListenSharedAccessSignature connection string>";
        }

2. Edellä mainittujen ohjeiden lisääminen toisesta uuden luokan nimi `MyInstanceIDService`. Tämä on Microsoftin Esiintymätunnus listener palvelun käyttöönoton.

    Tämä luokan koodi soittavat Microsoftin `IntentService` taustalla [Päivitä FCM-tunnuksen](https://developers.google.com/instance-id/guides/android-implementation#refresh_tokens) .

        import android.content.Intent;
        import android.util.Log;
        import com.google.firebase.iid.FirebaseInstanceIdService;
        
        
        public class MyInstanceIDService extends FirebaseInstanceIdService {
        
            private static final String TAG = "MyInstanceIDService";
        
            @Override
            public void onTokenRefresh() {
        
                Log.d(TAG, "Refreshing GCM Registration Token");
        
                Intent intent = new Intent(this, RegistrationIntentService.class);
                startService(intent);
            }
        };


3. Toisen uuden luokan lisääminen projektiin nimeltä, `RegistrationIntentService`. Tämä on käyttöönoton Microsoftin `IntentService` , joka käsittelee [päivittäminen FCM tunnuksen](https://developers.google.com/instance-id/guides/android-implementation#refresh_tokens) ja [rekisteröimistä ilmoitus-toiminnossa](notification-hubs-push-notification-registration-management.md).

    Käyttää seuraava koodi tähän luokkaan.

        import android.app.IntentService;
        import android.content.Intent;
        import android.content.SharedPreferences;
        import android.preference.PreferenceManager;
        import android.util.Log;        
        import com.google.firebase.iid.FirebaseInstanceId;
        import com.microsoft.windowsazure.messaging.NotificationHub;
        
        public class RegistrationIntentService extends IntentService {
        
            private static final String TAG = "RegIntentService";
        
            private NotificationHub hub;
        
            public RegistrationIntentService() {
                super(TAG);
            }
        
            @Override
            protected void onHandleIntent(Intent intent) {
        
                SharedPreferences sharedPreferences = PreferenceManager.getDefaultSharedPreferences(this);
                String resultString = null;
                String regID = null;
                String storedToken = null;
        
                try {
                    String FCM_token = FirebaseInstanceId.getInstance().getToken();
                    Log.d(TAG, "FCM Registration Token: " + FCM_token);
        
                    // Storing the registration id that indicates whether the generated token has been
                    // sent to your server. If it is not stored, send the token to your server,
                    // otherwise your server should have already received the token.
                    if (((regID=sharedPreferences.getString("registrationID", null)) == null)){
        
                        NotificationHub hub = new NotificationHub(NotificationSettings.HubName,
                                NotificationSettings.HubListenConnectionString, this);
                        Log.d(TAG, "Attempting a new registration with NH using FCM token : " + FCM_token);
                        regID = hub.register(FCM_token).getRegistrationId();
        
                        // If you want to use tags...
                        // Refer to : https://azure.microsoft.com/en-us/documentation/articles/notification-hubs-routing-tag-expressions/
                        // regID = hub.register(token, "tag1,tag2").getRegistrationId();
        
                        resultString = "New NH Registration Successfully - RegId : " + regID;
                        Log.d(TAG, resultString);
        
                        sharedPreferences.edit().putString("registrationID", regID ).apply();
                        sharedPreferences.edit().putString("FCMtoken", FCM_token ).apply();
                    }
        
                    // Check if the token may have been compromised and needs refreshing.
                    else if ((storedToken=sharedPreferences.getString("FCMtoken", "")) != FCM_token) {
        
                        NotificationHub hub = new NotificationHub(NotificationSettings.HubName,
                                NotificationSettings.HubListenConnectionString, this);
                        Log.d(TAG, "NH Registration refreshing with token : " + FCM_token);
                        regID = hub.register(FCM_token).getRegistrationId();
        
                        // If you want to use tags...
                        // Refer to : https://azure.microsoft.com/en-us/documentation/articles/notification-hubs-routing-tag-expressions/
                        // regID = hub.register(token, "tag1,tag2").getRegistrationId();
        
                        resultString = "New NH Registration Successfully - RegId : " + regID;
                        Log.d(TAG, resultString);
        
                        sharedPreferences.edit().putString("registrationID", regID ).apply();
                        sharedPreferences.edit().putString("FCMtoken", FCM_token ).apply();
                    }
        
                    else {
                        resultString = "Previously Registered Successfully - RegId : " + regID;
                    }
                } catch (Exception e) {
                    Log.e(TAG, resultString="Failed to complete registration", e);
                    // If an exception happens while fetching the new token or updating our registration data
                    // on a third-party server, this ensures that we'll attempt the update at a later time.
                }
        
                // Notify UI that registration has completed.
                if (MainActivity.isVisible) {
                    MainActivity.mainActivity.ToastNotify(resultString);
                }
            }
        }


        
4. -Että `MainActivity` luokan, Lisää seuraava `import` lauseet luokan ilmoituksen yläpuolella.

        import com.google.android.gms.common.ConnectionResult;
        import com.google.android.gms.common.GoogleApiAvailability;
        import com.microsoft.windowsazure.notifications.NotificationsManager;
        import android.content.Intent;
        import android.util.Log;
        import android.widget.TextView;
        import android.widget.Toast;

5. Lisää seuraavat yksityinen jäsenet luokan yläreunassa. Käytämme näitä [Google toistaa palvelut, kuten suosittelemalla Google käytettävyyden tarkistaminen](https://developers.google.com/android/guides/setup#ensure_devices_have_the_google_play_services_apk).

        public static MainActivity mainActivity;
        public static Boolean isVisible = false;    
        private static final String TAG = "MainActivity";
        private static final int PLAY_SERVICES_RESOLUTION_REQUEST = 9000;

6. Valitse oman `MainActivity` luokan, Lisää seuraava menetelmä Google Play-palvelut käyttöön. 

        /**
         * Check the device to make sure it has the Google Play Services APK. If
         * it doesn't, display a dialog that allows users to download the APK from
         * the Google Play Store or enable it in the device's system settings.
         */
        private boolean checkPlayServices() {
            GoogleApiAvailability apiAvailability = GoogleApiAvailability.getInstance();
            int resultCode = apiAvailability.isGooglePlayServicesAvailable(this);
            if (resultCode != ConnectionResult.SUCCESS) {
                if (apiAvailability.isUserResolvableError(resultCode)) {
                    apiAvailability.getErrorDialog(this, resultCode, PLAY_SERVICES_RESOLUTION_REQUEST)
                            .show();
                } else {
                    Log.i(TAG, "This device is not supported by Google Play Services.");
                    ToastNotify("This device is not supported by Google Play Services.");
                    finish();
                }
                return false;
            }
            return true;
        }


7. Valitse oman `MainActivity` luokan, Lisää seuraava koodi, joka tarkistaa Google Play-palvelujen ennen kuin soitat oman `IntentService` hakee FCM rekisteröinti tunnuksen ja ilmoitus-toiminnossa voit rekisteröidä.

        public void registerWithNotificationHubs()
        {
            if (checkPlayServices()) {
                // Start IntentService to register this application with FCM.
                Intent intent = new Intent(this, RegistrationIntentService.class);
                startService(intent);
            }
        }


8. Valitse `OnCreate` menetelmää `MainActivity` luokan, Lisää seuraava koodi Aloita rekisteröityminen, kun tehtävä luodaan.

        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
    
            mainActivity = this;
            NotificationsManager.handleNotifications(this, NotificationSettings.SenderId, MyHandler.class);
            registerWithNotificationHubs();
        }


9. Lisää nämä muita tapoja `MainActivity` vahvistamiseksi sovelluksen sovelluksen tilan ja raportin tilaa.

        @Override
        protected void onStart() {
            super.onStart();
            isVisible = true;
        }
    
        @Override
        protected void onPause() {
            super.onPause();
            isVisible = false;
        }
    
        @Override
        protected void onResume() {
            super.onResume();
            isVisible = true;
        }
    
        @Override
        protected void onStop() {
            super.onStop();
            isVisible = false;
        }
    
        public void ToastNotify(final String notificationMessage) {
            runOnUiThread(new Runnable() {
                @Override
                public void run() {
                    Toast.makeText(MainActivity.this, notificationMessage, Toast.LENGTH_LONG).show();
                    TextView helloText = (TextView) findViewById(R.id.text_hello);
                    helloText.setText(notificationMessage);
                }
            });
        }


10. `ToastNotify` Menetelmässä *"Hei-maailman"* `TextView` ohjausobjektin raportin tilan ja ilmoitusten pysyvästi-sovelluksessa. Lisää activity_main.xml asettelun ohjaavat seuraava tunnus.

        android:id="@+id/text_hello"

11. Seuraavaksi lisäämme Microsoftin vastaanotin AndroidManifest.xml määritimme aliluokka. Toisen uuden luokan lisääminen projektiin nimeltä `MyHandler`.

12. Lisää seuraavat Tuontilauseet yläreunassa `MyHandler.java`:

        import android.app.NotificationManager;
        import android.app.PendingIntent;
        import android.content.Context;
        import android.content.Intent;
        import android.media.RingtoneManager;
        import android.net.Uri;
        import android.os.Bundle;
        import android.support.v4.app.NotificationCompat;
        import com.microsoft.windowsazure.notifications.NotificationsHandler;

13. Lisää seuraava koodi `MyHandler` luokan, jolloin aliluokka `com.microsoft.windowsazure.notifications.NotificationsHandler`.

    Tämä koodi ohittaa `OnReceive` menetelmä, jotta käsittelijä raportoi ilmoituksia, jotka ovat saaneet. Käsittelijä lähettää ilmoituksen Android-hallinnan push-ilmoituksen myös käyttämällä `sendNotification()` menetelmää. `sendNotification()` Menetelmä suoritetaan, kun sovellus ei ole käynnissä ja ilmoituksen vastaanotetaan.

        public class MyHandler extends NotificationsHandler {
            public static final int NOTIFICATION_ID = 1;
            private NotificationManager mNotificationManager;
            NotificationCompat.Builder builder;
            Context ctx;
        
            @Override
            public void onReceive(Context context, Bundle bundle) {
                ctx = context;
                String nhMessage = bundle.getString("message");
                sendNotification(nhMessage);
                if (MainActivity.isVisible) {
                    MainActivity.mainActivity.ToastNotify(nhMessage);
                }
            }
        
            private void sendNotification(String msg) {
        
                Intent intent = new Intent(ctx, MainActivity.class);
                intent.addFlags(Intent.FLAG_ACTIVITY_CLEAR_TOP);
        
                mNotificationManager = (NotificationManager)
                        ctx.getSystemService(Context.NOTIFICATION_SERVICE);
        
                PendingIntent contentIntent = PendingIntent.getActivity(ctx, 0,
                        intent, PendingIntent.FLAG_ONE_SHOT);
        
                Uri defaultSoundUri = RingtoneManager.getDefaultUri(RingtoneManager.TYPE_NOTIFICATION);
                NotificationCompat.Builder mBuilder =
                        new NotificationCompat.Builder(ctx)
                                .setSmallIcon(R.mipmap.ic_launcher)
                                .setContentTitle("Notification Hub Demo")
                                .setStyle(new NotificationCompat.BigTextStyle()
                                        .bigText(msg))
                                .setSound(defaultSoundUri)
                                .setContentText(msg);
        
                mBuilder.setContentIntent(contentIntent);
                mNotificationManager.notify(NOTIFICATION_ID, mBuilder.build());
            }
        }


14. Android Studiossa valikkorivillä, valitse **Muodosta** > **Uudelleen projektin** varmistaaksesi, että virheitä löytyvät koodisi.
15. Suorita sovellus laitteessasi ja vahvista se rekisteröi onnistuneesti ilmoitus-toiminnossa. 

    > [AZURE.NOTE] Ennen ensimmäisen julkaisun rekisteröinti saattaa epäonnistua `onTokenRefresh()` menetelmää esiintymän Id-palvelun kutsutaan. Päivitys olisi intiate onnistuneen merkitsemisestä ilmoitus-toiminnossa.

##<a name="sending-push-notifications"></a>Push-ilmoitusten lähettäminen

Voit testata sovelluksen push-ilmoitusten vastaanottaminen alla kuvatulla tavalla lähettämällä [Azure Portal] - keskittimeen, sivu **vianmääritys** -osan ulkoasua kautta.

![Lähetä ilmoitus Azure keskittimet - testi](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-test-send.png)

[AZURE.INCLUDE [notification-hubs-sending-notifications-from-the-portal](../../includes/notification-hubs-sending-notifications-from-the-portal.md)]

## <a name="optional-send-push-notifications-directly-from-the-app"></a>(Valinnainen) Push-ilmoitukset lähetetään suoraan sovelluksesta

>[AZURE.IMPORTANT] Tämä esimerkki ilmoitusten lähettäminen asiakas-sovelluksen annetaan käyttöön liittyviä ilmoitusta. Koska tämä edellyttää `DefaultFullSharedAccessSignature` voi olla asiakas-sovellukseen, se paljastaa ilmoitus-keskittimeen, että käyttäjä voi saada luvattoman ilmoitukset lähetetään asiakkaiden riskiin.

Tavallisesti lähettäisit ilmoitukset Taustajärjestelmä palvelimen avulla. Joissakin tapauksissa, haluat ehkä pysty lähettämään suoraan asiakassovellus push-ilmoitukset. Tässä osassa kerrotaan, miten voit lähettää ilmoituksia asiakkaasta käyttämällä [Azure ilmoituksen keskittimeen REST API](https://msdn.microsoft.com/library/azure/dn223264.aspx).

1. Laajenna Android Studio Project-näkymä **sovelluksen** > **src** > **tärkeimmät** > **tarkkuus** > **Asettelu**. Avaa `activity_main.xml` asettelun tiedoston, ja valitse **teksti** -välilehti tiedoston tekstin sisällön päivittäminen. Päivitä tiedosto seuraava koodi, joka lisää uusi `Button` ja `EditText` ohjausobjektit push ilmoitukset lähetetään ilmoitus-toiminnossa. Lisää tämä koodi ja juuri ennen `</RelativeLayout>`.

        <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/send_button"
        android:id="@+id/sendbutton"
        android:layout_centerVertical="true"
        android:layout_centerHorizontal="true"
        android:onClick="sendNotificationButtonOnClick" />

        <EditText
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/editTextNotificationMessage"
        android:layout_above="@+id/sendbutton"
        android:layout_centerHorizontal="true"
        android:layout_marginBottom="42dp"
        android:hint="@string/notification_message_hint" />

2. Laajenna Android Studio Project-näkymä **sovelluksen** > **src** > **tärkeimmät** > **tarkkuus** > **arvot**. Avaa `strings.xml` tiedoston ja lisää merkkijonoarvot, joihin viitataan uuden `Button` ja `EditText` ohjausobjektit. Lisää nämä tiedoston alareunassa juuri ennen `</resources>`.

        <string name="send_button">Send Notification</string>
        <string name="notification_message_hint">Enter notification message text</string>


3. Valitse oman `NotificationSetting.java` tiedoston, Lisää seuraava-asetukseksi `NotificationSettings` luokan.

    Päivitä `HubFullAccess` oman keskittimeen **DefaultFullSharedAccessSignature** yhteysmerkkijonon kanssa. Tämän yhteysmerkkijonon voi kopioida [Azure-portaali] valitsemalla **Käytäntöjen** , ilmoitus-toiminnossa **asetukset** -sivu.

        public static String HubFullAccess = "<Enter Your DefaultFullSharedAccessSignature Connection string>";

4. -Että `MainActivity.java` tiedoston, Lisää seuraava `import` lauseet edellä `MainActivity` luokan.

        import java.io.BufferedOutputStream;
        import java.io.BufferedReader;
        import java.io.InputStreamReader;
        import java.io.OutputStream;
        import java.net.HttpURLConnection;
        import java.net.URL;
        import java.net.URLEncoder;
        import javax.crypto.Mac;
        import javax.crypto.spec.SecretKeySpec;
        import android.util.Base64;
        import android.view.View;
        import android.widget.EditText;

6. Valitse oman `MainActivity.java` tiedoston, Lisää ensin ylälaitaan seuraavat jäsenet, `MainActivity` luokan.  

        private String HubEndpoint = null;
        private String HubSasKeyName = null;
        private String HubSasKeyValue = null;

6. Sinun on luotava ohjelmiston käytön allekirjoitus (SaS)-tunnuksen tarkistamiseen kirjaa pyyntö viestit lähetetään ilmoitus-toiminnossa. Tämä on valmis jakamaan avaimen yhteysmerkkijonon ja luomalla sitten SaS-tunnuksen, [Yleisiä käsitteitä](http://msdn.microsoft.com/library/azure/dn495627.aspx) REST API viittauksen mainittu. Seuraava koodi on esimerkki.

    Valitse `MainActivity.java`, Lisää seuraavaa menetelmää `MainActivity` luokan jäsentää yhteysmerkkijono.

        /**
         * Example code from http://msdn.microsoft.com/library/azure/dn495627.aspx
         * to parse the connection string so a SaS authentication token can be
         * constructed.
         *
         * @param connectionString This must be the DefaultFullSharedAccess connection
         *                         string for this example.
         */
        private void ParseConnectionString(String connectionString)
        {
            String[] parts = connectionString.split(";");
            if (parts.length != 3)
                throw new RuntimeException("Error parsing connection string: "
                        + connectionString);
    
            for (int i = 0; i < parts.length; i++) {
                if (parts[i].startsWith("Endpoint")) {
                    this.HubEndpoint = "https" + parts[i].substring(11);
                } else if (parts[i].startsWith("SharedAccessKeyName")) {
                    this.HubSasKeyName = parts[i].substring(20);
                } else if (parts[i].startsWith("SharedAccessKey")) {
                    this.HubSasKeyValue = parts[i].substring(16);
                }
            }
        }


7. - `MainActivity.java`, Lisää seuraavat menetelmä `MainActivity` luokan luoda SaS todennus tunnuksen.

        /**
         * Example code from http://msdn.microsoft.com/library/azure/dn495627.aspx to
         * construct a SaS token from the access key to authenticate a request.
         *
         * @param uri The unencoded resource URI string for this operation. The resource
         *            URI is the full URI of the Service Bus resource to which access is
         *            claimed. For example,
         *            "http://<namespace>.servicebus.windows.net/<hubName>"
         */
        private String generateSasToken(String uri) {
    
            String targetUri;
            String token = null;
            try {
                targetUri = URLEncoder
                        .encode(uri.toString().toLowerCase(), "UTF-8")
                        .toLowerCase();
    
                long expiresOnDate = System.currentTimeMillis();
                int expiresInMins = 60; // 1 hour
                expiresOnDate += expiresInMins * 60 * 1000;
                long expires = expiresOnDate / 1000;
                String toSign = targetUri + "\n" + expires;
    
                // Get an hmac_sha1 key from the raw key bytes
                byte[] keyBytes = HubSasKeyValue.getBytes("UTF-8");
                SecretKeySpec signingKey = new SecretKeySpec(keyBytes, "HmacSHA256");
    
                // Get an hmac_sha1 Mac instance and initialize with the signing key
                Mac mac = Mac.getInstance("HmacSHA256");
                mac.init(signingKey);
    
                // Compute the hmac on input data bytes
                byte[] rawHmac = mac.doFinal(toSign.getBytes("UTF-8"));
    
                // Using android.util.Base64 for Android Studio instead of
                // Apache commons codec
                String signature = URLEncoder.encode(
                        Base64.encodeToString(rawHmac, Base64.NO_WRAP).toString(), "UTF-8");
    
                // Construct authorization string
                token = "SharedAccessSignature sr=" + targetUri + "&sig="
                        + signature + "&se=" + expires + "&skn=" + HubSasKeyName;
            } catch (Exception e) {
                if (isVisible) {
                    ToastNotify("Exception Generating SaS : " + e.getMessage().toString());
                }
            }
    
            return token;
        }



8. Valitse `MainActivity.java`, Lisää seuraavaa menetelmää `MainActivity` luokan push-ilmoitusviestin lähettäminen valitsemalla käyttämällä valmiita REST-Ohjelmointirajapinnalla ja käsitellä **Lähetä ilmoitus** -painiketta napsauttamalla.

        /**
         * Send Notification button click handler. This method parses the
         * DefaultFullSharedAccess connection string and generates a SaS token. The
         * token is added to the Authorization header on the POST request to the
         * notification hub. The text in the editTextNotificationMessage control
         * is added as the JSON body for the request to add a GCM message to the hub.
         *
         * @param v
         */
        public void sendNotificationButtonOnClick(View v) {
            EditText notificationText = (EditText) findViewById(R.id.editTextNotificationMessage);
            final String json = "{\"data\":{\"message\":\"" + notificationText.getText().toString() + "\"}}";
    
            new Thread()
            {
                public void run()
                {
                    try
                    {
                        // Based on reference documentation...
                        // http://msdn.microsoft.com/library/azure/dn223273.aspx
                        ParseConnectionString(NotificationSettings.HubFullAccess);
                        URL url = new URL(HubEndpoint + NotificationSettings.HubName +
                                "/messages/?api-version=2015-01");
    
                        HttpURLConnection urlConnection = (HttpURLConnection)url.openConnection();
    
                        try {
                            // POST request
                            urlConnection.setDoOutput(true);
    
                            // Authenticate the POST request with the SaS token
                            urlConnection.setRequestProperty("Authorization", 
                                generateSasToken(url.toString()));
    
                            // Notification format should be GCM
                            urlConnection.setRequestProperty("ServiceBusNotification-Format", "gcm");
    
                            // Include any tags
                            // Example below targets 3 specific tags
                            // Refer to : https://azure.microsoft.com/en-us/documentation/articles/notification-hubs-routing-tag-expressions/
                            // urlConnection.setRequestProperty("ServiceBusNotification-Tags", 
                            //      "tag1 || tag2 || tag3");
    
                            // Send notification message
                            urlConnection.setFixedLengthStreamingMode(json.length());
                            OutputStream bodyStream = new BufferedOutputStream(urlConnection.getOutputStream());
                            bodyStream.write(json.getBytes());
                            bodyStream.close();
    
                            // Get reponse
                            urlConnection.connect();
                            int responseCode = urlConnection.getResponseCode();
                            if ((responseCode != 200) && (responseCode != 201)) {
                                BufferedReader br = new BufferedReader(new InputStreamReader((urlConnection.getErrorStream())));
                                String line;
                                StringBuilder builder = new StringBuilder("Send Notification returned " +
                                        responseCode + " : ")  ;
                                while ((line = br.readLine()) != null) {
                                    builder.append(line);
                                }

                                ToastNotify(builder.toString());
                            }
                        } finally {
                            urlConnection.disconnect();
                        }
                    }
                    catch(Exception e)
                    {
                        if (isVisible) {
                            ToastNotify("Exception Sending Notification : " + e.getMessage().toString());
                        }
                    }
                }
            }.start();
        }



##<a name="testing-your-app"></a>Sovelluksen testaaminen

####<a name="push-notifications-in-the-emulator"></a>Emulaattori Push-ilmoitukset

Jos haluat testata push-ilmoitukset emulaattorin sisällä, varmista, että emulaattorin kuva tukee, jonka valitsit, kun sovellus Google-Ohjelmointirajapinnan taso. Jos kuvaa ei tue alkuperäisen Google-ohjelmointirajapinnan, jonka päätyvät kanssa **palvelun\_ei\_käytettävissä** poikkeuksen.

Lisäksi edellä, varmista, että olet lisännyt oman käynnissä emulaattorin **asetukset**-kohdassa Google-tilisi > **tilit**. Muussa tapauksessa yrittää rekisteröidä GCM voi aiheuttaa **todennus\_epäonnistui** poikkeuksen.

####<a name="running-the-application"></a>Sovellus on käynnissä

1. Suorita sovellus ja huomaat Tunnuksella ilmoitetaan onnistuneen rekisteröinnin.

    ![Android - kanavan rekisteröinti testaaminen](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-android-studio-registered.png)

2. Kirjoita kaikki Android-laitteille, joka on rekisteröity valitsemalla lähetetään ilmoitus.

    ![Android - viestin lähettämistä testaaminen](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-android-studio-set-message.png)

3. Paina **Lähetä ilmoitus**. Laitteet, jossa sovellus on käynnissä näkyy `AlertDialog` push-ilmoitusviestin esiintymää. Laitteet, joka ei ole käynnissä sovelluksen, mutta aiemmin rekisteröidyn push-ilmoitukset saavat ilmoituksen Android ilmoituksen hallinta. Voit tarkastella niitä sipaisemalla alaspäin vasemmassa yläkulmassa.

    ![Android - ilmoitukset testaaminen](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-android-studio-received-message.png)

##<a name="next-steps"></a>Seuraavat vaiheet

On suositeltavaa [Käyttäjille push-ilmoitukset käyttöön ilmoituksen keskittimiä] opetusohjelman vaiheessa. Se kerrotaan, kuinka, ilmoitukset lähetetään ASP.NET-taustatietokannan, tunnisteiden avulla voit kohdistaa tietyille käyttäjille.

Jos haluat määritetään käyttäjien korko ryhmiin, katso [Käytä ilmoituksen keskittimet lähettää uutisia] opetusohjelman.

Lisätietoja ilmoituksen keskittimet yleisiä tietoja on artikkelissa Microsoftin [Ilmoitusta keskittimet ohjeita].

<!-- Images. -->



<!-- URLs. -->
[Get started with push notifications in Mobile Services]: ../mobile-services-javascript-backend-android-get-started-push.md  
[Mobile Services Android SDK]: https://go.microsoft.com/fwLink/?LinkID=280126&clcid=0x409
[Referencing a library project]: http://go.microsoft.com/fwlink/?LinkId=389800
[Azure Classic Portal]: https://manage.windowsazure.com/
[Ilmoitus keskittimet ohjeet]: notification-hubs-push-notification-overview.md
[Ilmoitus keskittimet avulla käyttäjille push-ilmoitukset]: notification-hubs-aspnet-backend-gcm-android-push-to-user-google-notification.md
[Ilmoitus keskittimet avulla voit lähettää uutisia]: notification-hubs-aspnet-backend-android-xplat-segmented-gcm-push-notification.md
[Azure Portal]: https://portal.azure.com
