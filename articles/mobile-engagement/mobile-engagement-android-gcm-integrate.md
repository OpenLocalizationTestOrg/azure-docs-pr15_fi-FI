<properties
    pageTitle="Azure Mobile välitys Android SDK-integrointi"
    description="Uusimmat päivitykset ja Azure Mobile välitys Android SDK ohjeet"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="piyushjo" />

#<a name="how-to-integrate-gcm-with-mobile-engagement"></a>Voit integroida GCM Mobile välitys

> [AZURE.IMPORTANT] Sinun on noudatettava integrointi kuvatulla tavalla miten voit integroida välitys Android asiakirjan ennen seuraavan tässä oppaassa.
>
> Tämä asiakirja on hyödyllinen vain, jos olet jo integroituna Reach moduulin ja aiot push Google Play-laitteet. Voit integroida Reach Kampanjat sovelluksessa, lue ensin integroida välitys vastausosoitteen Android.

##<a name="introduction"></a>Johdanto

Sovelluksen lähetettävä integrointi GCM avulla.

Miten SDK aina GCM paketteja sisältävät `azme` avaimen tieto-objekti. Näin käytät GCM toiseen tarkoitukseen sovelluksen, voit suodattaa työntää avaimen perusteella.

> [AZURE.IMPORTANT] Vain laitteet, joissa Android 2.2 tai yläpuolella on asennettu ja onko Google Google Play taustan yhteydessä käytössä voivat sijaita käyttämällä GCM; Voit kuitenkin integroida koodi turvallisesti laitteissa, joita ei tueta (se käyttää tätä ominaisuutta vain).

##<a name="create-a-google-cloud-messaging-project-with-api-key"></a>Kun Ohjelmointirajapinnan avain Google Cloud Messaging projektin luominen

[AZURE.INCLUDE [mobile-engagement-enable-Google-cloud-messaging](../../includes/mobile-engagement-enable-google-cloud-messaging.md)]

##<a name="sdk-integration"></a>SDK-integrointi

### <a name="managing-device-registrations"></a>Laitteen merkintöjen hallinta

Kunkin laitteen rekisteröinti-komento on lähettäminen Google-palvelimet, muuten niitä ei voi siirtyä.

Laitteen voi myös unregister GCM ilmoitusten (laite ei ole rekisteröity automaattisesti, jos sovellus poistetaan).

Jos et käytä [Google Play-SDK] tai ei ole jo lähetät rekisteröinnin käyttötarkoitukseen itse, voit tehdä välitys Rekisteröi laite automaattisesti puolestasi.

Voit ottaa tämän, Lisää seuraavat oman `AndroidManifest.xml` tiedoston sisäpuolella `<application/>` tunniste:

            <!-- If only 1 sender, don't forget the \n, otherwise it will be parsed as a negative number... -->
            <meta-data android:name="engagement:gcm:sender" android:value="<Your Google Project Number>\n" />

### <a name="communicate-registration-id-to-the-engagement-push-service-and-receive-notifications"></a>Viestintä tunnuksella välitys Push-palveluun ja ilmoitusten vastaanottaminen

Jotta viestintä välitys Push-palveluun laitteen rekisteröinti-tunnus ja sen ilmoituksia, Lisää seuraavat oman `AndroidManifest.xml` tiedoston sisäpuolella `<application/>` tunniste (myös silloin, kun hallitset laitteen merkintöjen itse):

            <receiver android:name="com.microsoft.azure.engagement.gcm.EngagementGCMEnabler"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.intent.action.APPID_GOT" />
              </intent-filter>
            </receiver>

            <receiver android:name="com.microsoft.azure.engagement.gcm.EngagementGCMReceiver" android:permission="com.google.android.c2dm.permission.SEND">
              <intent-filter>
                <action android:name="com.google.android.c2dm.intent.REGISTRATION" />
                <action android:name="com.google.android.c2dm.intent.RECEIVE" />
                <category android:name="<your_package_name>" />
              </intent-filter>
            </receiver>

Varmista seuraavat oikeudet ovat oman `AndroidManifest.xml` (kun `</application>` tunniste).

            <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
            <uses-permission android:name="<your_package_name>.permission.C2D_MESSAGE" />
            <permission android:name="<your_package_name>.permission.C2D_MESSAGE" android:protectionLevel="signature" />

##<a name="grant-mobile-engagement-access-to-your-gcm-api-key"></a>Mobiili välitys käyttöoikeuden myöntäminen GCM-Ohjelmointirajapinnan avain

Noudata [Tässä oppaassa](mobile-engagement-android-get-started.md#grant-mobile-engagement-access-to-your-gcm-api-key) Mobile välitys käyttöoikeuden myöntäminen GCM-Ohjelmointirajapinnan avain.

[Google Play SDK-paketissa]:https://developers.google.com/cloud-messaging/android/start
