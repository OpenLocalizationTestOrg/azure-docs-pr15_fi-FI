<properties
    pageTitle="Pääset alkuun käyttämällä Baidu Azure ilmoituksen keskittimet | Microsoft Azure"
    description="Tässä opetusohjelmassa kerrotaan, miten Azure ilmoituksen keskittimet avulla Android-laitteiden Baidu käyttämällä push-ilmoitukset."
    services="notification-hubs"
    documentationCenter="android"
    authors="ysxu"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.devlang="java"
    ms.topic="hero-article"
    ms.tgt_pltfrm="mobile-baidu"
    ms.workload="mobile"
    ms.date="08/19/2016"
    ms.author="yuaxu"/>

# <a name="get-started-with-notification-hubs-using-baidu"></a>Ilmoitus keskittimet käyttämällä Baidu käytön aloittaminen

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

##<a name="overview"></a>Yleiskatsaus

Baidu cloud push on Kiinan pilvipalvelussa, joiden avulla voit lähettää mobiililaitteiden push-ilmoitukset. Tämä palvelu on erityisen hyödyllinen Kiinassa välittää push-ilmoitukset Android missä monimutkaisia eri app tallentaa ja push-palveluihin, Android-laitteille, jotka eivät ole yleensä yhteydessä GCM (Google Cloud Messaging) käytettävyyttä lisäksi vuoksi.

##<a name="prerequisites"></a>Edellytykset

Tässä opetusohjelmassa edellyttää seuraavaa:

+ Android SDK (oletetaan, että käytät Pimennys), jonka voit ladata <a href="http://go.microsoft.com/fwlink/?LinkId=389797">Android-sivusto</a>
+ [Mobile-palvelujen Android SDK-paketissa]
+ [Baidu Push Android SDK-paketissa]

>[AZURE.NOTE] Jotta voit suorittaa tässä opetusohjelmassa, on oltava aktiivinen Azure-tili. Jos sinulla ei ole tiliä, voit luoda ilmainen kokeiluversio tili vain muutaman minuutin. Lisätietoja on artikkelissa [Azure maksuttoman kokeiluversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-baidu-get-started%2F).


##<a name="create-a-baidu-account"></a>Baidu-tilin luominen

Jos haluat käyttää Baidu, on oltava Baidu-tili. Jos olet jo luonut yksi, kirjaudu sisään [Baidu-portaaliin] ja siirry seuraavaan vaiheeseen. Muussa tapauksessa Katso alla olevia ohjeita siitä, miten voit luoda Baidu tilin.  

1. [Baidu-portaaliin] ja valitse**登录**(**Sisäänkirjautuminen**)-linkki. Valitse Aloita tilin rekisteröiminen**立即注册**.

    ![][1]

2. Kirjoita tarvittavat tiedot – puhelin/sähköpostin osoite ja salasanan vahvistus koodi, ja valitse **kirjautumisen**.

    ![][2]

3. Voit lähetetään sähköpostitse, että olet kirjoittanut linkin Baidu tilin aktivoimiseen sähköpostiosoitteeseen.

    ![][3]

4. Sähköpostitilin kirjautuminen, Avaa Baidu aktivointi sähköposti ja sitten aktivointilinkki Baidu tilin aktivoimiseen.

    ![][4]

Kun sinulla on aktivoitu Baidu-tili, kirjaudu sisään [Baidu portal].

##<a name="register-as-a-baidu-developer"></a>Rekisteröi Baidu developer

1. Kun olet kirjautunut sisään [Baidu portal], valitse**更多 >>** (**Lisää**).

    ![][5]

2. Vieritä alaspäin**站长与开发者服务 (Sivustovastaavan ja Developer palvelut)** -osassa ja valitse**百度开放云平台**(**Baidu Avaa cloud-ympäristössä**).

    ![][6]

3. Valitse seuraavalla sivulla**开发者服务**(**Developer Services**) oikeassa yläkulmassa.

    ![][7]

4. Valitse seuraavalla sivulla oikeassa yläkulmassa valikosta**注册开发者**(**Rekisteröity kehittäjät**).

    ![][8]

5. Kirjoita oma nimi, kuvaus ja matkapuhelinnumero vahvistus tekstiviestin viestien ja valitse sitten**送验证码**(**Lähetä tarkistuskoodi**). Huomaa, että kansainvälisen numeron, sinun on maakoodi, kirjoita arvo sulkeisiin. Esimerkiksi Yhdysvalloissa numerolle, se on **(1) 1234567890**.

    ![][9]

6. Pitäisi sitten tulla vahvistus luvuksi tekstiviestin seuraavan esimerkin mukaisesti:

    ![][10]

7. Kirjoita vahvistus numero**验证码**(**vahvistuskoodi**) viestin.

8. Suorita developer rekisteröinnin hyväksymällä Baidu-sopimus ja valitsemalla**提交**(**Lähetä**). Näet seuraavan sivun onnistumiseen rekisteröintiä:

    ![][11]

##<a name="create-a-baidu-cloud-push-project"></a>Baidu cloud push projektin luominen

Kun luot projektin Baidu cloud push, näyttöön tulee Sovellustunnus-Ohjelmointirajapinnan avain ja salausavaimen.

1. Kun olet kirjautunut sisään [Baidu portal], valitse**更多 >>** (**Lisää**).

    ![][5]

2. Vieritä alaspäin**站长与开发者服务**(**Sivustovastaavan ja Developer palvelut**)-osassa ja valitse**百度开放云平台**(**Baidu Avaa cloud-ympäristössä**).

    ![][6]

3. Valitse seuraavalla sivulla**开发者服务**(**Developer Services**) oikeassa yläkulmassa.

    ![][7]

4. Valitse seuraavalla sivulla**云推送**(**Cloud Push**)**云服务**(**Pilvipalveluihin**)-osiosta.

    ![][12]

5. Kun olet rekisteröity kehittäjä, näet**管理控制台**(**Hallintakonsoli**) yläreunan valikosta. Valitse**开发者服务管理**(**kehittäjät palvelun hallinta**).

    ![][13]

6. Valitse seuraavalla sivulla**创建工程**(**Luo projekti**).

    ![][14]

7. Kirjoita sovelluksen nimi ja valitse**创建**(**Luo**).

    ![][15]

8. Yhteydessä luomisen onnistumisen Baidu cloud push projektin näet sivun **sovelluksen tunnus**, **Ohjelmointirajapinnan avain**ja **Salainen avain**. Pane merkille Ohjelmointirajapinnan avain ja salausavaimen, jossa Käytämme myöhemmin.

    ![][16]

9. Määritä projektin push-ilmoitukset valitsemalla**云推送**(**Cloud Push**) vasemmassa ruudussa.

    ![][31]

10. Valitse seuraavalla sivulla**推送设置**(**Push-asetukset**)-painiketta.

    ![][32]  

11. Määritys-sivulla Lisää pakettinimi, sinun Android projektin**应用包名**(**sovelluspaketin**)-kentän käyttäminen, ja valitse sitten**保存设置**(**Tallenna**).  

    ![][33]

Näet**保存成功!** (**tallennettiin!**) viestin.

##<a name="configure-your-notification-hub"></a>Määritä ilmoitus-toiminnossa

1. Kirjautuminen [Perinteinen Azure-portaaliin]ja valitse sitten **+ Uusi** näytön alareunassa.

2. Valitse **Sovelluksen palvelut**, **Palvelun Bus**, **Ilmoitus-toiminnosta**ja valitse **Nopea luominen**.

3. Anna sille nimi **Ilmoitus-toiminnosta**, valitse **alue** ja **Namespace** , johon tämä ilmoitus-toiminnossa luodaan, ja valitse sitten **Luo uusi ilmoitus-toiminnossa**.  

    ![][17]

4. Valitse nimitilan, johon olet luonut ilmoitus-toiminnosta ja valitse sitten **Ilmoituksen keskittimet** yläreunassa.

    ![][18]

5. Valitse ilmoitus-toiminto, jonka loit ja valitse sitten **Määritä** yläreunan valikosta.

    ![][19]

6. Siirry **baidu ilmoitusasetusten määrittäminen** -osioon ja kirjoita Ohjelmointirajapinnan avain ja salausavaimen, jotka olet hankkinut Baidu konsolin aiemmin Baidu cloud push projektin. Valitse **Tallenna**.

    ![][20]

7. Valitse ilmoitus-toiminnossa yläreunassa **koontinäyttö** -välilehti ja valitse sitten **Näytä yhteysmerkkijonon**.

    ![][21]

8. Pane merkille **DefaultListenSharedAccessSignature** ja **DefaultFullSharedAccessSignature** **Access yhteyden tiedot** -ikkunasta.

    ![][22]

##<a name="connect-your-app-to-the-notification-hub"></a>Sovelluksen yhdistäminen ilmoitus-toiminnossa

1. Luo uuden Android projektin Pimennys ADT (**tiedoston** > **Uusi** > **Android-sovelluksen projektin**).

    ![][23]

2. Kirjoita **Sovelluksen nimi** ja varmista, että **Pienin pakollinen SDK** -versio on asetettu **API 16: Android 4.1**.

    ![][24]

3. Valitse **Seuraava** ja jatka seuraamalla ohjatun toiminnon, kunnes **Luo toiminta** -ikkuna. Varmista, että **Tyhjä tehtävä** on valittuna ja valitse lopuksi **Valmis** , jos haluat luoda uuden Android-sovelluksen.

    ![][25]

4. Varmista, että **Projektin luominen kohde** on asetettu oikein.

    ![][26]

5. Lataa ilmoitus-keskittimet-0.4.jar tiedoston [Ilmoitus-keskittimet-Android-SDK-Bintray](https://bintray.com/microsoftazuremobile/SDK/Notification-Hubs-Android-SDK/0.4) **tiedostot** -välilehti. Tiedoston lisääminen **kirjastoon** kansion Pimennys projektin ja Päivitä *kirjastoa* -kansio.

6. Lataa ja unzip [Baidu Push Android SDK], Avaa **kirjastoa** -kansio ja kopioi sitten **pushservice x.y.z** purkki ja **armeabi** & **mips** kansiot Android-sovelluksen **kirjastoa** -kansiossa.

7. Avaa Android projektin **AndroidManifest.xml** -tiedosto ja lisää mukaan Baidu SDK tarvittavat oikeudet.

        <uses-permission android:name="android.permission.INTERNET" />
        <uses-permission android:name="android.permission.READ_PHONE_STATE" />
        <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
        <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
        <uses-permission android:name="android.permission.WRITE_SETTINGS" />
        <uses-permission android:name="android.permission.VIBRATE" />
        <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
        <uses-permission android:name="android.permission.DISABLE_KEYGUARD" />
        <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
        <uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
        <uses-permission android:name="android.permission.ACCESS_DOWNLOAD_MANAGER" />
        <uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION" />

8. Lisää **sovellus** -osa- **AndroidManifest.xml**, korvaaminen *yourprojectname* (esimerkiksi **com.example.BaiduTest**) **android: name** -ominaisuutta. Varmista, että tämä projektinimi vastaa jonkin, jonka määritit Baidu konsolissa.

        <application android:name="yourprojectname.DemoApplication"

9. Lisää sovellus-elementissä seuraavat määritysten jälkeen **. MainActivity** tehtävän osan korvaaminen *yourprojectname* (esimerkiksi **com.example.BaiduTest**):

        <receiver android:name="yourprojectname.MyPushMessageReceiver">
            <intent-filter>
                <action android:name="com.baidu.android.pushservice.action.MESSAGE" />
                <action android:name="com.baidu.android.pushservice.action.RECEIVE" />
                <action android:name="com.baidu.android.pushservice.action.notification.CLICK" />
            </intent-filter>
        </receiver>

        <receiver android:name="com.baidu.android.pushservice.PushServiceReceiver"
            android:process=":bdservice_v1">
            <intent-filter>
                <action android:name="android.intent.action.BOOT_COMPLETED" />
                <action android:name="android.net.conn.CONNECTIVITY_CHANGE" />
                <action android:name="com.baidu.android.pushservice.action.notification.SHOW" />
            </intent-filter>
        </receiver>

        <receiver android:name="com.baidu.android.pushservice.RegistrationReceiver"
            android:process=":bdservice_v1">
            <intent-filter>
                <action android:name="com.baidu.android.pushservice.action.METHOD" />
                <action android:name="com.baidu.android.pushservice.action.BIND_SYNC" />
            </intent-filter>
            <intent-filter>
                <action android:name="android.intent.action.PACKAGE_REMOVED"/>
                <data android:scheme="package" />
            </intent-filter>
        </receiver>

        <service
            android:name="com.baidu.android.pushservice.PushService"
            android:exported="true"
            android:process=":bdservice_v1"  >
            <intent-filter>
                <action android:name="com.baidu.android.pushservice.action.PUSH_SERVICE" />
            </intent-filter>
        </service>

9. Lisää uusi luokka nimeltä **ConfigurationSettings.java** projektiin.

    ![][28]

    ![][29]

10. Lisää siihen seuraava koodi:

        public class ConfigurationSettings {
                public static String API_KEY = "...";
                public static String NotificationHubName = "...";
                public static String NotificationHubConnectionString = "...";
            }

    Määritä **API_KEY** arvo ja hakea projektista Baidu cloud aiemmin **NotificationHubName** ilmoituksen keskittimeen nimesi perinteinen Azure-portaalista ja **NotificationHubConnectionString** kanssa DefaultListenSharedAccessSignature perinteinen Azure-portaalista.

11. Lisää uusi luokka, jota kutsutaan **DemoApplication.java**ja lisää seuraava koodi:

        import com.baidu.frontia.FrontiaApplication;

        public class DemoApplication extends FrontiaApplication {
            @Override
            public void onCreate() {
                super.onCreate();
            }
        }

12. Lisää toinen uuden luokan nimi **MyPushMessageReceiver.java**ja alla koodin lisääminen. Tämä on luokka, joka käsittelee push-ilmoitukset, jotka ovat saaneet Baidu push-palvelimesta.

        import java.util.List;
        import android.content.Context;
        import android.os.AsyncTask;
        import android.util.Log;
        import com.baidu.frontia.api.FrontiaPushMessageReceiver;
        import com.microsoft.windowsazure.messaging.NotificationHub;

        public class MyPushMessageReceiver extends FrontiaPushMessageReceiver {
            /** TAG to Log */
            public static NotificationHub hub = null;
            public static String mChannelId, mUserId;
            public static final String TAG = MyPushMessageReceiver.class
                    .getSimpleName();

            @Override
            public void onBind(Context context, int errorCode, String appid,
                    String userId, String channelId, String requestId) {
                String responseString = "onBind errorCode=" + errorCode + " appid="
                        + appid + " userId=" + userId + " channelId=" + channelId
                        + " requestId=" + requestId;
                Log.d(TAG, responseString);
                mChannelId = channelId;
                mUserId = userId;

                try {
                 if (hub == null) {
                        hub = new NotificationHub(
                                ConfigurationSettings.NotificationHubName,
                                ConfigurationSettings.NotificationHubConnectionString,
                                context);
                        Log.i(TAG, "Notification hub initialized");
                    }
                } catch (Exception e) {
                   Log.e(TAG, e.getMessage());
                }

                registerWithNotificationHubs();
            }

            private void registerWithNotificationHubs() {
               new AsyncTask<Void, Void, Void>() {
                  @Override
                  protected Void doInBackground(Void... params) {
                     try {
                         hub.registerBaidu(mUserId, mChannelId);
                         Log.i(TAG, "Registered with Notification Hub - '"
                                + ConfigurationSettings.NotificationHubName + "'"
                                + " with UserId - '"
                                + mUserId + "' and Channel Id - '"
                                + mChannelId + "'");
                     } catch (Exception e) {
                         Log.e(TAG, e.getMessage());
                     }
                     return null;
                 }
               }.execute(null, null, null);
            }

            @Override
            public void onSetTags(Context context, int errorCode,
                    List<String> sucessTags, List<String> failTags, String requestId) {
                String responseString = "onSetTags errorCode=" + errorCode
                        + " sucessTags=" + sucessTags + " failTags=" + failTags
                        + " requestId=" + requestId;
                Log.d(TAG, responseString);
            }

            @Override
            public void onDelTags(Context context, int errorCode,
                    List<String> sucessTags, List<String> failTags, String requestId) {
                String responseString = "onDelTags errorCode=" + errorCode
                        + " sucessTags=" + sucessTags + " failTags=" + failTags
                        + " requestId=" + requestId;
                Log.d(TAG, responseString);
            }

            @Override
            public void onListTags(Context context, int errorCode, List<String> tags,
                    String requestId) {
                String responseString = "onListTags errorCode=" + errorCode + " tags="
                        + tags;
                Log.d(TAG, responseString);
            }

            @Override
            public void onUnbind(Context context, int errorCode, String requestId) {
                String responseString = "onUnbind errorCode=" + errorCode
                        + " requestId = " + requestId;
                Log.d(TAG, responseString);
            }

            @Override
            public void onNotificationClicked(Context context, String title,
                    String description, String customContentString) {
                String notifyString = "title=\"" + title + "\" description=\""
                        + description + "\" customContent=" + customContentString;
                Log.d(TAG, notifyString);
            }

            @Override
            public void onMessage(Context context, String message,
                    String customContentString) {
                String messageString = "message=\"" + message + "\" customContentString=" + customContentString;
                Log.d(TAG, messageString);
            }
        }

13. Avaa **MainActivity.java**ja lisää seuraava teksti **onCreate** menetelmää:

            PushManager.startWork(getApplicationContext(),
                    PushConstants.LOGIN_TYPE_API_KEY, ConfigurationSettings.API_KEY);

14. Avaa seuraavat Tuontilauseet yläreunassa:

            import com.baidu.android.pushservice.PushConstants;
            import com.baidu.android.pushservice.PushManager;

##<a name="send-notifications-to-your-app"></a>Ilmoitukset lähetetään sovelluksen


Voit testata nopeasti ilmoitusten vastaanottaminen sovelluksen lähettämällä ilmoitukset [Azure-portaalin](https://portal.azure.com/) käyttäminen **Testata** Lähetä ilmoitus-toiminnossa alla olevassa kuvassa esitetyllä tavalla.

![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-test-send-wns.png)

Push-ilmoitukset lähetetään tavallisesti taustatietokantaan-palvelun, kuten Mobile-palveluihin tai ASP.NET-yhteensopiva kirjaston käyttöä. Voit käyttää myös REST-Ohjelmointirajapinnalla suoraan, jos haluat lähettää ilmoitukset, jos kirjasto ei ole käytettävissä käyttäjän taustatietokannan.

Tässä opetusohjelmassa on säilyttää yksinkertainen ja näytetään vain testaaminen asiakkaan sovelluksen lähettämällä ilmoitukset käyttämällä .NET SDK ilmoituksen keskittimien sijaan Taustajärjestelmä palvelu console-sovelluksessa. On suositeltavaa [Käyttäjille push-ilmoitukset käyttöön ilmoituksen keskittimiä](notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md) opetusohjelman lähettämiseen ilmoitukset ASP.NET-taustasta vaiheessa. Seuraavilla tavoilla voi kuitenkin käyttää ilmoitusten lähettäminen:

* **REST-liittymän**: Voit tukea ilmoituksen missä tahansa Taustajärjestelmä ympäristössä [REST-liittymän](http://msdn.microsoft.com/library/windowsazure/dn223264.aspx)avulla.

* **Microsoft Azure ilmoituksen keskittimet .NET SDK**: Nuget paketin Managerissa Visual Studiossa, suorita [Asennuksen paketin Microsoft.Azure.NotificationHubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).

* **Node.js** : [ilmoitus keskittimet-Node.js käyttämisestä](notification-hubs-nodejs-push-notification-tutorial.md).

* **Mobile-sovellukset**: Katso esimerkki siitä, miten ilmoitukset lähetetään Azure palvelun mobiilisovellukset-taustatietokannan, joka on integroitu ilmoituksen keskittimet, [Lisää push-ilmoituksia, jotka mobile-sovellus](../app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-push.md).

* **Java / PHP**: esimerkki siitä, miten voit lähettää ilmoituksia käyttämällä REST API-kohdassa "How to use Java/PHP-keskittimet ilmoitus" ([Java](notification-hubs-java-push-notification-tutorial.md) | [PHP](notification-hubs-php-push-notification-tutorial.md)).

##<a name="optional-send-notifications-from-a-net-console-app"></a>(Valinnainen) Ilmoitukset lähetetään .NET-konsolin-sovelluksesta.

Tässä osassa on Näytä lähettää ilmoituksen .NET konsolin sovellusta käytettäessä.

1. Luo uusi Visual C# console-sovellus:

    ![][30]

2. Pakettien hallinta konsoli-ikkunassa määrittää projektiin uuden console-sovelluksen **oletusarvon projektin** ja suorita sitten konsoli-ikkunassa seuraava komento:

        Install-Package Microsoft.Azure.NotificationHubs

    Tämä lisää <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification keskittimet NuGet paketin</a>Azure-ilmoituksen keskittimet SDK viittaus.

    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-package-manager.png)

3. Avaa tiedosto **Program.cs** ja lisää seuraava lauseella:

        using Microsoft.Azure.NotificationHubs;

4. Valitse oman `Program` luokan, Lisää seuraava menetelmä ja korvaa arvot, jotka sinulla on *DefaultFullSharedAccessSignatureSASConnectionString* ja *NotificationHubName* .

        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("DefaultFullSharedAccessSignatureSASConnectionString", "NotificationHubName");
            string message = "{\"title\":\"((Notification title))\",\"description\":\"Hello from Azure\"}";
            var result = await hub.SendBaiduNativeNotificationAsync(message);
        }

5. Lisää seuraavat rivit **Main** -menetelmää:

         SendNotificationAsync();
         Console.ReadLine();

##<a name="test-your-app"></a>Sovelluksen testaaminen

Voit esikatsella tämän sovelluksen kanssa todellinen puhelin-vain Yhdistä puhelin tietokoneeseen USB-kaapelilla. Tämä Lataa sovelluksen liitettyjä Phonen.

Testaa tämän sovelluksen kanssa emulaattorin Pimennys yläreunan työkalurivin **Suorita**ja valitse sitten sovellus. Tämä käynnistää emulaattori, ja valitse Lataa ja suorittaa sovellus.

Sovelluksen 'käyttäjätunnus' ja 'channelId' hakee Baidu Push-ilmoituspalvelu ja rekisteröi ilmoitus-toiminnossa.

Testaa ilmoituksen lähettäminen voit perinteinen Azure-portaalin virheenkorjaus-välilehti. Jos olet luonut Visual Studio .NET console-sovellus, painamalla F5-näppäintä Visual Studio sovelluksen käyttämiseen. Sovelluksen lähetetään ilmoitus, joka tulee näkyviin laitteen tai emulaattorin yläreunan ilmaisinalueella.


<!-- Images. -->
[1]: ./media/notification-hubs-baidu-get-started/BaiduRegistration.png
[2]: ./media/notification-hubs-baidu-get-started/BaiduRegistrationInput.png
[3]: ./media/notification-hubs-baidu-get-started/BaiduConfirmation.png
[4]: ./media/notification-hubs-baidu-get-started/BaiduActivationEmail.png
[5]: ./media/notification-hubs-baidu-get-started/BaiduRegistrationMore.png
[6]: ./media/notification-hubs-baidu-get-started/BaiduOpenCloudPlatform.png
[7]: ./media/notification-hubs-baidu-get-started/BaiduDeveloperServices.png
[8]: ./media/notification-hubs-baidu-get-started/BaiduDeveloperRegistration.png
[9]: ./media/notification-hubs-baidu-get-started/BaiduDevRegistrationInput.png
[10]: ./media/notification-hubs-baidu-get-started/BaiduDevRegistrationConfirmation.png
[11]: ./media/notification-hubs-baidu-get-started/BaiduDevConfirmationFinal.png
[12]: ./media/notification-hubs-baidu-get-started/BaiduCloudPush.png
[13]: ./media/notification-hubs-baidu-get-started/BaiduDevSvcMgmt.png
[14]: ./media/notification-hubs-baidu-get-started/BaiduCreateProject.png
[15]: ./media/notification-hubs-baidu-get-started/BaiduCreateProjectInput.png
[16]: ./media/notification-hubs-baidu-get-started/BaiduProjectKeys.png
[17]: ./media/notification-hubs-baidu-get-started/AzureNHCreation.png
[18]: ./media/notification-hubs-baidu-get-started/NotificationHubs.png
[19]: ./media/notification-hubs-baidu-get-started/NotificationHubsConfigure.png
[20]: ./media/notification-hubs-baidu-get-started/NotificationHubBaiduConfigure.png
[21]: ./media/notification-hubs-baidu-get-started/NotificationHubsConnectionStringView.png
[22]: ./media/notification-hubs-baidu-get-started/NotificationHubsConnectionString.png
[23]: ./media/notification-hubs-baidu-get-started/EclipseNewProject.png
[24]: ./media/notification-hubs-baidu-get-started/EclipseProjectCreation.png
[25]: ./media/notification-hubs-baidu-get-started/EclipseBlankActivity.png
[26]: ./media/notification-hubs-baidu-get-started/EclipseProjectBuildProperty.png
[27]: ./media/notification-hubs-baidu-get-started/EclipseBaiduReferences.png
[28]: ./media/notification-hubs-baidu-get-started/EclipseNewClass.png
[29]: ./media/notification-hubs-baidu-get-started/EclipseConfigSettingsClass.png
[30]: ./media/notification-hubs-baidu-get-started/ConsoleProject.png
[31]: ./media/notification-hubs-baidu-get-started/BaiduPushConfig1.png
[32]: ./media/notification-hubs-baidu-get-started/BaiduPushConfig2.png
[33]: ./media/notification-hubs-baidu-get-started/BaiduPushConfig3.png

<!-- URLs. -->
[Mobile-palvelujen Android SDK-paketissa]: https://go.microsoft.com/fwLink/?LinkID=280126&clcid=0x409
[Baidu Push Android SDK-paketissa]: http://developer.baidu.com/wiki/index.php?title=docs/cplat/push/sdk/clientsdk
[Azure perinteinen Portal]: https://manage.windowsazure.com/
[Baidu portal]: http://www.baidu.com/
