<properties
    pageTitle="Azure Mobile välitys Android SDK-integrointi"
    description="Uusimmat päivitykset ja Azure Mobile välitys Android SDK ohjeet"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="dwrede"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/19/2016"
    ms.author="piyushjo" />


#<a name="how-to-integrate-adm-with-engagement"></a>Voit integroida ADM välitys

> [AZURE.IMPORTANT] Sinun on noudatettava integrointi kuvatulla tavalla miten voit integroida välitys Android asiakirjan ennen seuraavan tässä oppaassa.
>
> Tämä asiakirja on hyödyllinen vain, jos olet jo integroituna Reach moduulin ja aiot push Amazon laitteet. Voit integroida Reach Kampanjat sovelluksessa, lue ensin integroida välitys vastausosoitteen Android.

##<a name="introduction"></a>Johdanto

Miten kun kohdistamisen Amazon Android-laitteille sovellusta integrointi ADM avulla.

Miten SDK aina ADM paketteja sisältävät `azme` avaimen tieto-objekti. Näin käytät ADM toiseen tarkoitukseen sovelluksen, voit suodattaa työntää avaimen perusteella.

> [AZURE.IMPORTANT] Vain Amazon Kindle laitteet, joissa Android versio 4.0.3 tai edellä tukemat Amazon laitteen Messaging; Voit kuitenkin integroida koodin turvallisesti muissa laitteissa.

##<a name="sign-up-to-adm"></a>Rekisteröidy ADM

Jos et ole vielä tehnyt, sinun on otettava ADM Amazon-tililläsi.

Menettelyn yksityiskohtainen osoitteessa: [<https://developer.amazon.com/sdk/adm/credentials.html>].

Yhteydessä menettelyä on valmis, näyttöön tulee:

-   OAuth välitys voivan push-laitteet tunnistetiedot (asiakas-tunnus ja asiakkaan salaisuus).
-   API-näppäintä, joka on integroitu sovellus.

##<a name="sdk-integration"></a>SDK-integrointi

### <a name="managing-device-registrations"></a>Laitteen merkintöjen hallinta

Kunkin laitteen rekisteröinti-komento on lähettäminen ADM-palvelimet, muuten niitä ei voi siirtyä.

Jos käytät jo [ADM asiakkaan kirjasto]ja on jo [integroitu ADM] voit siirtyä suoraan android-sdk-adm-vastaanottamiseen.

Jos ADM on integroitu ei vielä ole, välitys on yksinkertaisin tapa ottaa käyttöön sovelluksen:

Muokkaa yhteyttä `AndroidManifest.xml` tiedosto:

-   Lisää Amazon nimitila tiedoston alkaa seuraavasti:

        <?xml version="1.0" encoding="utf-8"?>
        <manifest xmlns:android="http://schemas.android.com/apk/res/android"
                  xmlns:amazon="http://schemas.amazon.com/apk/res/android"

-   Sisään `<application/>` tunnisteen, Lisää sisältö:

        <amazon:enable-feature
           android:name="com.amazon.device.messaging"
           android:required="false"/>

        <meta-data android:name="engagement:adm:register" android:value="true" />

-   Kun olet lisännyt amazon-tunniste, muodosta-virhe saattaa olla, jos projektin luominen kohde on Android 2.1 alapuolella. Sinun on käytettävä **Android 2.1 +** muodosta kohde (ei hätää, voit silti määrittää `minSdkVersion` määritetty 4).
-   Integroi ADM-Ohjelmointirajapinnan avain sijoituksen kuin seuraavassa [seuraavasti].

Noudata seuraavan osat.

### <a name="communicate-registration-id-to-the-engagement-push-service-and-receive-notifications"></a>Viestintä tunnuksella välitys Push-palveluun ja ilmoitusten vastaanottaminen

Jotta viestintä välitys Push-palveluun laitteen rekisteröinti-tunnus ja sen ilmoituksia, Lisää seuraavat oman `AndroidManifest.xml` tiedoston sisäpuolella `<application/>` tunniste (vaikka käyttäisit ADM ilman välitys):

        <receiver android:name="com.microsoft.azure.engagement.adm.EngagementADMEnabler"
          android:exported="false">
          <intent-filter>
            <action android:name="com.microsoft.azure.engagement.intent.action.APPID_GOT"/>
          </intent-filter>
        </receiver>

         <receiver android:name="com.microsoft.azure.engagement.adm.EngagementADMReceiver"
           android:permission="com.amazon.device.messaging.permission.SEND">
          <intent-filter>
            <action android:name="com.amazon.device.messaging.intent.REGISTRATION"/>
            <action android:name="com.amazon.device.messaging.intent.RECEIVE"/>
            <category android:name="<your_package_name>"/>
          </intent-filter>
        </receiver>   

Varmista seuraavat oikeudet ovat oman `AndroidManifest.xml` (ennen `</application>` tunniste).

        <uses-permission android:name="android.permission.WAKE_LOCK"/>
        <uses-permission android:name="com.amazon.device.messaging.permission.RECEIVE"/>
        <uses-permission android:name="<your_package_name>.permission.RECEIVE_ADM_MESSAGE"/>
        <permission android:name="<your_package_name>.permission.RECEIVE_ADM_MESSAGE" android:protectionLevel="signature"/>

##<a name="grant-engagement-oauth-credentials"></a>Myönnä välitys OAuth tunnistetiedot

Lähetä OAuth tunnistetiedot (Asiakastunnus ja asiakkaan salaisuus) välitys-portaalissa.

[< https://developer.amazon.com/sdk/adm/credentials.html>]:https://developer.amazon.com/sdk/adm/credentials.html
[ADM asiakkaan kirjastoon]:https://developer.amazon.com/sdk/adm/setup.html
[integroitu ADM]:https://developer.amazon.com/sdk/adm/integrating-app.html
[Tämä toiminto]:https://developer.amazon.com/sdk/adm/integrating-app.html#Asset
