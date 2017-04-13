<properties
    pageTitle="Aloita Azure ilmoituksen keskittimet Kindle sovellusten | Microsoft Azure"
    description="Tässä opetusohjelmassa opit käyttämään Azure ilmoituksen keskittimet push-ilmoitukset lähetetään Kindle-sovellus."
    services="notification-hubs"
    documentationCenter=""
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-kindle"
    ms.devlang="Java"
    ms.topic="hero-article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

# <a name="get-started-with-notification-hubs-for-kindle-apps"></a>Ilmoitus keskittimet Kindle sovellusten aloittaminen

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

##<a name="overview"></a>Yleiskatsaus

Tässä opetusohjelmassa näytetään, miten push-ilmoitukset lähetetään Kindle sovellukseen Azure ilmoituksen keskittimet avulla.
Luo tyhjä Kindle-sovellusta, joka vastaanottaa push-ilmoitukset Amazon laitteen Messaging (ADM) avulla.

##<a name="prerequisites"></a>Edellytykset

Tässä opetusohjelmassa edellyttää seuraavaa:

+ Pyydä <a href="http://go.microsoft.com/fwlink/?LinkId=389797">Android sivuston</a>Android SDK (oletetaan, että käytät Pimennys).
+ <a href="https://developer.amazon.com/appsandservices/resources/development-tools/ide-tools/tech-docs/01-setting-up-your-development-environment">Asetusten määrittäminen Your kehitysympäristö</a> kehittäminen ympäristön määrittäminen Kindle noudattamalla.

##<a name="add-a-new-app-to-the-developer-portal"></a>Lisää uusi sovellus developer-portaalissa

1. Luo sovellus [Amazon developer-portaalissa].

    ![][0]

2. Kopioi **sovellusnäppäintä**.

    ![][1]

3. Portaalissa Napsauta sovelluksen nimeä ja valitse sitten **Laitteen Messaging** -välilehti.

    ![][2]

4. Valitse **Luo uusi suojausprofiilin**ja luo sitten uusi suojausprofiilin (esimerkiksi **TestAdm suojausprofiilin**). Valitse **Tallenna**.

    ![][3]

5. Valitse **Suojaus-profiileista** juuri luomasi suojausprofiilin tarkastelemiseen. Kopioi **Asiakastunnus** ja **Asiakkaan salaisen** arvot myöhempää käyttöä varten.

    ![][4]

## <a name="create-an-api-key"></a>Luo Ohjelmointirajapinnan avain

1. Avaa komentokehote järjestelmänvalvojan oikeuksilla.
2. Siirry Android SDK-kansioon.
3. Kirjoita seuraava komento:

        keytool -list -v -alias androiddebugkey -keystore ./debug.keystore

    ![][5]

4.  **Keystore** salasanan Kirjoita **android**.

5.  Kopioi **MD5** tunnisteen.
6.  Takaisin sisään developer-portaalissa **viestit** -välilehdessä **Android/Kindle** ja sovelluksen (esimerkiksi **com.sample.notificationhubtest**) ja **MD5** arvon paketin nimi **Luo Ohjelmointirajapinnan avain**.

## <a name="add-credentials-to-the-hub"></a>Tunnistetietojen lisääminen keskittimeen

Lisää-portaalissa asiakkaan salaisuus ja Ostajantunnus ilmoitus-toiminnossa **määrittäminen** -välilehteen.

## <a name="set-up-your-application"></a>Sovelluksen määrittäminen

> [AZURE.NOTE] Kun olet luomassa sovellus, käytä vähintään API tason 17.

Lisää ADM-kirjastojen Pimennys projektin:

1. Voit hankkia ADM kirjaston [lataaminen SDK]. Pura SDK zip-tiedosto.
2. Valitse Pimennys projektin hiiren kakkospainikkeella ja valitse sitten **Ominaisuudet**. Valitse **Java luominen polku** vasemmalla puolella ja valitse sitten yläreunassa **kirjastot **-välilehti. **Lisää ulkoisia Jar**ja valitse tiedosto `\SDK\Android\DeviceMessaging\lib\amazon-device-messaging-*.jar` hakemistosta, johon olet purkanut Amazon SDK-paketissa.
3. Lataa NotificationHubs Android SDK (linkki).
4. Paketin purkaminen ja vedä tiedosto `notification-hubs-sdk.jar` kyselyjä `libs` Pimennys kansioon.

Muokkaa sovelluksen-luettelo tukemaan ADM:

1. Voit lisätä Amazon nimitilan luettelon pääelementti:


        xmlns:amazon="http://schemas.amazon.com/apk/res/android"

2. Lisää ensimmäinen elementti kohdassa tiedostojen osan käyttöoikeudet. Korvaa paketti, joka on luotu sovelluksen **[Oma PAKETTINIMI]** .

        <permission
         android:name="[YOUR PACKAGE NAME].permission.RECEIVE_ADM_MESSAGE"
         android:protectionLevel="signature" />

        <uses-permission android:name="android.permission.INTERNET"/>

        <uses-permission android:name="[YOUR PACKAGE NAME].permission.RECEIVE_ADM_MESSAGE" />

        <!-- This permission allows your app access to receive push notifications
        from ADM. -->
        <uses-permission android:name="com.amazon.device.messaging.permission.RECEIVE" />

        <!-- ADM uses WAKE_LOCK to keep the processor from sleeping when a message is received. -->
        <uses-permission android:name="android.permission.WAKE_LOCK" />

3. Lisää sovellus-osan ensimmäisen alikohteen Seuraava elementti. Muista **[YOUR palvelun nimi]** korvaa oman ADM viestin käsittely, jonka luot (mukaan lukien paketin) seuraavan osion nimeä ja korvaa pakettinimi, jonka loit sovelluksen **[Oma PAKETTINIMI]** .

        <amazon:enable-feature
              android:name="com.amazon.device.messaging"
                     android:required="true"/>
        <service
            android:name="[YOUR SERVICE NAME]"
            android:exported="false" />

        <receiver
            android:name="[YOUR SERVICE NAME]$Receiver" />

            <!-- This permission ensures that only ADM can send your app registration broadcasts. -->
            android:permission="com.amazon.device.messaging.permission.SEND" >

            <!-- To interact with ADM, your app must listen for the following intents. -->
            <intent-filter>
          <action android:name="com.amazon.device.messaging.intent.REGISTRATION" />
          <action android:name="com.amazon.device.messaging.intent.RECEIVE" />

          <!-- Replace the name in the category tag with your app's package name. -->
          <category android:name="[YOUR PACKAGE NAME]" />
            </intent-filter>
        </receiver>

## <a name="create-your-adm-message-handler"></a>Luo ADM-viestin käsittely

1. Luo uusi luokka, jotka perivät `com.amazon.device.messaging.ADMMessageHandlerBase` ja anna sille nimi `MyADMMessageHandler`, seuraavassa kuvassa osoitetulla tavalla:

    ![][6]

2. Lisää seuraava `import` lauseet:

        import android.app.NotificationManager;
        import android.app.PendingIntent;
        import android.content.Context;
        import android.content.Intent;
        import android.support.v4.app.NotificationCompat;
        import com.amazon.device.messaging.ADMMessageReceiver;
        import com.microsoft.windowsazure.messaging.NotificationHub

3. Lisää seuraava koodi, jonka olet luonut luokan. Muista korvattavat keskittimeen nimi ja yhteysmerkkijonon (kuunnella):

        public static final int NOTIFICATION_ID = 1;
        private NotificationManager mNotificationManager;
        NotificationCompat.Builder builder;
        private static NotificationHub hub;
        public static NotificationHub getNotificationHub(Context context) {
            Log.v("com.wa.hellokindlefire", "getNotificationHub");
            if (hub == null) {
                hub = new NotificationHub("[hub name]", "[listen connection string]", context);
            }
            return hub;
        }

        public MyADMMessageHandler() {
                super("MyADMMessageHandler");
            }

            public static class Receiver extends ADMMessageReceiver
            {
                public Receiver()
                {
                    super(MyADMMessageHandler.class);
                }
            }

            private void sendNotification(String msg) {
                Context ctx = getApplicationContext();

             mNotificationManager = (NotificationManager)
                    ctx.getSystemService(Context.NOTIFICATION_SERVICE);

            PendingIntent contentIntent = PendingIntent.getActivity(ctx, 0,
                new Intent(ctx, MainActivity.class), 0);

            NotificationCompat.Builder mBuilder =
                new NotificationCompat.Builder(ctx)
                .setSmallIcon(R.mipmap.ic_launcher)
                .setContentTitle("Notification Hub Demo")
                .setStyle(new NotificationCompat.BigTextStyle()
                         .bigText(msg))
                .setContentText(msg);

            mBuilder.setContentIntent(contentIntent);
            mNotificationManager.notify(NOTIFICATION_ID, mBuilder.build());
        }

4. Lisää seuraava koodi `OnMessage()` menetelmää:

        String nhMessage = intent.getExtras().getString("msg");
        sendNotification(nhMessage);

5. Lisää seuraava koodi `OnRegistered` menetelmää:

            try {
        getNotificationHub(getApplicationContext()).register(registrationId);
            } catch (Exception e) {
        Log.e("[your package name]", "Fail onRegister: " + e.getMessage(), e);
            }

6.  Lisää seuraava koodi `OnUnregistered` menetelmää:

            try {
                getNotificationHub(getApplicationContext()).unregister();
            } catch (Exception e) {
                Log.e("[your package name]", "Fail onUnregister: " + e.getMessage(), e);
            }

7. Valitse `MainActivity` -menetelmä lisää tuonti-lause:

        import com.amazon.device.messaging.ADM;

8. Lisää seuraava koodi lopussa `OnCreate` menetelmää:

        final ADM adm = new ADM(this);
        if (adm.getRegistrationId() == null)
        {
           adm.startRegister();
        } else {
            new AsyncTask() {
                  @Override
                  protected Object doInBackground(Object... params) {
                     try {                       MyADMMessageHandler.getNotificationHub(getApplicationContext()).register(adm.getRegistrationId());
                     } catch (Exception e) {
                         Log.e("com.wa.hellokindlefire", "Failed registration with hub", e);
                         return e;
                     }
                     return null;
                 }
               }.execute(null, null, null);
        }

## <a name="add-your-api-key-to-your-app"></a>Sovelluksen lisääminen Ohjelmointirajapinnan avain

1. Luo Pimennys-tiedosto nimeltä **api_key.txt** projektin directory varoihin.
2. Avaa tiedosto ja kopioi sitten API-näppäintä, jotka olet luonut Amazon developer-portaalissa.

## <a name="run-the-app"></a>Suorita sovellus

1. Aloita emulaattori.
2. Emulaattori, valitse sipaisemalla yläreunassa ja valitsemalla **asetukset**ja sitten **Oma tili** ja rekisteröidä Amazon-tilille.
3. Suorita Pimennys-sovellus.

> [AZURE.NOTE] Jos ongelma ilmenee, tarkista emulaattorin (tai laite). Aika-arvon on oltava tarkka. Voit muuttaa Kindle emulaattori aikaa, Android SDK-ympäristö Työkalut hakemistosta suorittamalla seuraava komento:

        adb shell  date -s "yyyymmdd.hhmmss"

## <a name="send-a-message"></a>Viestin lähettäminen

Viestin lähettäminen käyttämällä .NET:

        static void Main(string[] args)
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("[conn string]", "[hub name]");

            hub.SendAdmNativeNotificationAsync("{\"data\":{\"msg\" : \"Hello from .NET!\"}}").Wait();
        }

![][7]

<!-- URLs. -->
[Amazon developer-portaalissa]: https://developer.amazon.com/home.html
[Lataa SDK]: https://developer.amazon.com/public/resources/development-tools/sdk

[0]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal1.png
[1]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal2.png
[2]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal3.png
[3]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal4.png
[4]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal5.png
[5]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-cmd-window.png
[6]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-new-java-class.png
[7]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-notification.png
