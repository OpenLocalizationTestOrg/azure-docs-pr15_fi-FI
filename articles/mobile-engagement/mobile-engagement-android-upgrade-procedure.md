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


#<a name="upgrade-procedures"></a>Päivityksen ohjeita

Jos jo on vanhempi versio, tutustu SDK on integroitu sovellukseen, sinun on ottaa huomioon seuraavat seikat, kun päivitetään SDK.

Saatat joutua noudata useita ohjeita, jos vastattu SDK useita versioita. Esimerkiksi jos voit siirtää 1.4.0 1.6.0, sinun on ensin noudattamalla "lähettäjä, 1.5.0 1.4.0" sitten "lähettäjä, 1.6.0 1.5.0" kuvatulla tavalla.

Jostakin päivität-versio sinulla on korvaa `mobile-engagement-VERSION.jar` uudella.

##<a name="from-420-to-421"></a>Valitse 4.2.0 4.2.1

Tässä vaiheessa voit tehdä todella SDK: ssa, se on saatavilla toimintoja integroida suojauksen parantaminen.

Lisää nyt `exported="false"` kaikki saatavilla toimintoihin.

Saatavilla toimintoja pitäisi nyt näyttää tältä että `AndroidManifest.xml`:

            <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementTextAnnouncementActivity" android:theme="@android:style/Theme.Light" android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="android.intent.category.DEFAULT" />
                <data android:mimeType="text/plain" />
              </intent-filter>
            </activity>
            <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity" android:theme="@android:style/Theme.Light" android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="android.intent.category.DEFAULT" />
                <data android:mimeType="text/html" />
              </intent-filter>
            </activity>
            <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementPollActivity" android:theme="@android:style/Theme.Light" android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.POLL"/>
                <category android:name="android.intent.category.DEFAULT" />
              </intent-filter>
            </activity>
            <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementLoadingActivity" android:theme="@android:style/Theme.Dialog" android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.LOADING"/>
                <category android:name="android.intent.category.DEFAULT"/>
              </intent-filter>
            </activity>

##<a name="from-400-to-410"></a>Valitse 4.0.0 4.1.0-sovelluksessa

SDK nyt kahvaa uudet käyttöoikeudet mallin Android m.

Jos käytät sijainti ominaisuuksia tai ylemmän tason ilmoitukset, lue [Tämä osa](mobile-engagement-android-integrate-engagement.md#android-m-permissions).

Lisäksi uuden käyttöoikeusmallin sovellus tukee nyt määrittäminen sijainti ominaisuuksia suorituksen aikana.
Edelleen yhteensopivuuden, sijainnin tiedostojen parametreilla, mutta se on nyt poistettu. Jos haluat käyttää runtime määritykset, Poista seuraavat osiot oman ``AndroidManifest.xml``:

    <meta-data
      android:name="engagement:locationReport:lazyArea"
      android:value="true"/>
    <meta-data
      android:name="engagement:locationReport:realTime"
      android:value="true"/>
    <meta-data
      android:name="engagement:locationReport:realTime:background"
      android:value="true"/>
    <meta-data
      android:name="engagement:locationReport:realTime:fine"
      android:value="true"/>

ja lue [päivitetty näin](mobile-engagement-android-integrate-engagement.md#location-reporting) käyttämään runtime määritys.

##<a name="from-300-to-400"></a>Valitse 3.0.0 4.0.0

### <a name="native-push"></a>Alkuperäisen push

Alkuperäisen push (GCM/ADM) nyt myös käytetään sovellusten ilmoitukset niin, että sinun on määritettävä kaikenlaisia push markkinointikampanjan alkuperäisen push tunnistetietojen.

Jos ei ole jo Ota noudattamalla [näitä ohjeita](mobile-engagement-android-integrate-engagement-reach.md#native-push).

### <a name="androidmanifestxml"></a>AndroidManifest.xml

Saatavilla integrointi on muokattu ``AndroidManifest.xml``.

Korvaa näin:

    <receiver
      android:name="com.microsoft.azure.engagement.reach.EngagementReachReceiver"
      android:exported="false">
      <intent-filter>
        <action android:name="android.intent.action.BOOT_COMPLETED"/>
        <action android:name="com.microsoft.azure.engagement.intent.action.AGENT_CREATED"/>
        <action android:name="com.microsoft.azure.engagement.intent.action.MESSAGE"/>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.ACTION_NOTIFICATION"/>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.EXIT_NOTIFICATION"/>
        <action android:name="android.intent.action.DOWNLOAD_COMPLETE"/>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.DOWNLOAD_TIMEOUT"/>
      </intent-filter>
    </receiver>

Mukaan

    <receiver
      android:name="com.microsoft.azure.engagement.reach.EngagementReachReceiver"
      android:exported="false">
      <intent-filter>
        <action android:name="android.intent.action.BOOT_COMPLETED"/>
        <action android:name="com.microsoft.azure.engagement.intent.action.AGENT_CREATED"/>
        <action android:name="com.microsoft.azure.engagement.intent.action.MESSAGE"/>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.ACTION_NOTIFICATION"/>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.EXIT_NOTIFICATION"/>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.DOWNLOAD_TIMEOUT"/>
      </intent-filter>
    </receiver>
    <receiver android:name="com.microsoft.azure.engagement.reach.EngagementReachDownloadReceiver">
      <intent-filter>
        <action android:name="android.intent.action.DOWNLOAD_COMPLETE"/>
      </intent-filter>
    </receiver>

Ei mahdollisesti ladataan näyttöön nyt kun napsautat ilmoituksen (ja tekstin/sivuston sisällön) tai kyselyn.
Sinun on lisättävä tämä näiden Kampanjat 4.0.0 käyttäminen:

    <activity
      android:name="com.microsoft.azure.engagement.reach.activity.EngagementLoadingActivity"
      android:theme="@android:style/Theme.Dialog">
      <intent-filter>
        <action android:name="com.microsoft.azure.engagement.reach.intent.action.LOADING"/>
        <category android:name="android.intent.category.DEFAULT"/>
      </intent-filter>
    </activity>

### <a name="resources"></a>Resurssit

Upottaa uuden `res/layout/engagement_loading.xml` tiedoston projektiin.

##<a name="from-240-to-300"></a>Valitse 2.4.0 3.0.0

Seuraavassa kerrotaan, miten SDK-integroinnin siirtäminen tarjoaman Capptain SAS sovellukseen Azure Mobile välitys tarjoaa Capptain palvelun. Jos olet siirtymässä aiemmasta versiosta, tilanteeseesi, Capptain web-sivuston siirtäminen 2.4.0 ensin ja tee seuraavat toimet.

>[AZURE.IMPORTANT] Capptain ja Mobile välitys eivät ole samoja palveluja ja alla kuvatulla tavalla korostaa vain siirtäminen asiakas-sovellus. Siirtyminen SDK-sovelluksessa ei siirretä tietojen Capptain-palvelimien Mobile välitys-palvelimiin.

### <a name="jar-file"></a>PURKKI tiedosto

Korvaa `capptain.jar` mukaan `mobile-engagement-VERSION.jar` -että `libs` kansio.

### <a name="resource-files"></a>Resurssitiedostojen

Jokaisen resurssitiedosto, joka toimittaa (etuliite mukaan `capptain_`) on korvataan uudet jäsenet tervetulleiksi (etuliite `engagement_`).

Jos kyseiset tiedostot on mukautettu, sinun on tekemäsi mukautukset, valitse uusia tiedostoja, **kaikki resurssitiedostoja tunnukset on nimetty**uudelleen käyttöön.

### <a name="application-id"></a>Tunnus

Nyt välitys käyttää yhteysmerkkijonon SDK-tunnukset, kuten tunniste määrittämiseen.

Sinun on käytettävä `EngagementAgent.init` menetelmä avain toimiessasi tältä:

            EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
            engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
            EngagementAgent.getInstance(this).init(engagementConfiguration);

Sovelluksen yhteysmerkkijonon näkyy Azure-portaalissa.

Poista kaikki kutsu `CapptainAgent.configure` kuin `EngagementAgent.init` Tämä menetelmä korvaa.

`appId` Ei enää voi määrittää käyttämällä `AndroidManifest.xml`.

Poista tämän osan oman `AndroidManifest.xml` jos se on:

            <meta-data android:name="capptain:appId" android:value="<YOUR_APPID>"/>

### <a name="java-api"></a>Java-Ohjelmointirajapinta

Jokaisen kutsun, tutustu SDK minkä tahansa Java-luokan ei ole sallittua; esimerkiksi `CapptainAgent.getInstance(this)` on nimettävä `EngagementAgent.getInstance(this)`, `extends CapptainActivity` on nimettävä `extends EngagementActivity` jne.

Jos oletusarvoinen agentti asetustiedostot on yhdistetty, tiedoston oletusnimeä on nyt `engagement.agent` eikä avain `engagement:agent`.

Web-ilmoitukset luotaessa Javascript-kansion on nyt `engagementReachContent`.

### <a name="androidmanifestxml"></a>AndroidManifest.xml

Paljon muutoksia on tapahtunut siellä, palvelu ei ole enää jaettu ja paljon vastaanottajia eivät ole enää voi viedä.

Palvelun ilmoituksen on nyt yksinkertainen; Poista käyttötarkoitusta suodattimen ja kaikki metatiedon sisällä, ja lisää `exportable=false`.

Sekä kaikki nimetään uudelleen käyttämään välitys.

Nyt näyttää:

            <service
              android:name="com.microsoft.azure.engagement.service.EngagementService"
              android:exported="false"
              android:label="<Your application name>Service"
              android:process=":Engagement"/>

Kun haluat ottaa käyttöön testi lokit-metatiedot-sovelluksen tunnisteen nyt siirretään ja on nimetty uudelleen:

            <application>
            
              <meta-data android:name="engagement:log:test" android:value="true" />
            
              <service/>
            
            </application>

Kaikki muut metatiedon vain nimetty uudelleen, toimi (kurssin nimeä vain käytät niistä) täydellisestä luettelosta:

            <meta-data
              android:name="engagement:reportCrash"
              android:value="true"/>
            <meta-data
              android:name="engagement:sessionTimeout"
              android:value="10000"/>
            <meta-data
              android:name="engagement:burstThreshold"
              android:value="0"/>
            <meta-data
              android:name="engagement:connection:delay"
              android:value="0"/>
            <meta-data
              android:name="engagement:locationReport:lazyArea"
              android:value="false"/>
            <meta-data
              android:name="engagement:locationReport:realTime"
              android:value="false"/>
            <meta-data
              android:name="engagement:locationReport:realTime:background"
              android:value="false"/>
            <meta-data
              android:name="engagement:locationReport:realTime:fine"
              android:value="false"/>
            <meta-data
              android:name="engagement:agent:settings:name"
              android:value="engagement.agent"/>
            <meta-data
              android:name="engagement:agent:settings:mode"
              android:value="0"/>
            <meta-data
              android:name="engagement:gcm:sender"
              android:value="<YOUR_PROJECT_NUMBER>\n"/>
            <meta-data
              android:name="engagement:adm:register"
              android:value="true"/>
            <meta-data
              android:name="engagement:reach:notification:icon"
              android:value="<DRAWABLE_NAME_WITHOUT_EXTENSION>"/>
            
            <activity android:name="SomeActivityWithoutReachOverlay">
              <meta-data
                android:name="engagement:notification:overlay"
                android:value="false"/>
            </activity>

Google Play- ja SmartAd seuranta on poistettu sinulla on poistettava tämä ilman korvaavan SDK:

            <meta-data 
                android:name="capptain:track:installReferrerForwardList"
                android:value="com.class1,com.class2"/>
            <meta-data
                android:name="capptain:track:adservers"
                android:value="smartad" />

Saatavilla toimintojen ilmoitetaan nyt tältä:

            <activity
              android:name="com.microsoft.azure.engagement.reach.activity.EngagementTextAnnouncementActivity"
              android:theme="@android:style/Theme.Light">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="android.intent.category.DEFAULT"/>
                <data android:mimeType="text/plain"/>
              </intent-filter>
            </activity>
            <activity
              android:name="com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity"
              android:theme="@android:style/Theme.Light">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="android.intent.category.DEFAULT"/>
                <data android:mimeType="text/html"/>
              </intent-filter>
            </activity>
            <activity
              android:name="com.microsoft.azure.engagement.reach.activity.EngagementPollActivity"
              android:theme="@android:style/Theme.Light">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.POLL"/>
                <category android:name="android.intent.category.DEFAULT"/>
              </intent-filter>
            </activity>
            
Jos sinulla on saatavilla mukautetut aktiviteetit, sinun on muutettava vain käyttötarkoitusta toiminnot vastaamaan joko `com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT` tai `com.microsoft.azure.engagement.reach.intent.action.POLL`.

Lähetyksen vastaanottajia on nimetty uudelleen sekä nyt lisätä `exported=false`. Näin vastaanottajia uusi määritystä (, kurssin nimeä vain käytät niistä) täysi luettelo:

            <receiver android:name="com.microsoft.azure.engagement.reach.EngagementReachReceiver"
              android:exported="false">
              <intent-filter>
                <action android:name="android.intent.action.BOOT_COMPLETED"/>
                <action android:name="com.microsoft.azure.engagement.intent.action.AGENT_CREATED"/>
                <action android:name="com.microsoft.azure.engagement.intent.action.MESSAGE"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ACTION_NOTIFICATION"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.EXIT_NOTIFICATION"/>
                <action android:name="android.intent.action.DOWNLOAD_COMPLETE"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.DOWNLOAD_TIMEOUT"/>
              </intent-filter>
            </receiver>
            
            <receiver android:name="com.microsoft.azure.engagement.gcm.EngagementGCMEnabler"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.intent.action.APPID_GOT" />
              </intent-filter>
            </receiver>
            
            <receiver
              android:name="com.microsoft.azure.engagement.gcm.EngagementGCMReceiver"
              android:permission="com.google.android.c2dm.permission.SEND">
              <intent-filter>
                <action android:name="com.google.android.c2dm.intent.REGISTRATION"/>
                <action android:name="com.google.android.c2dm.intent.RECEIVE"/>
                <category android:name="<your_package_name>"/>
              </intent-filter>
            </receiver>
            
            <receiver android:name="com.microsoft.azure.engagement.adm.EngagementADMEnabler"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.intent.action.APPID_GOT"/>
              </intent-filter>
            </receiver>
            
            <receiver
              android:name="com.microsoft.azure.engagement.adm.EngagementADMReceiver"
              android:permission="com.amazon.device.messaging.permission.SEND">
              <intent-filter>
                <action android:name="com.amazon.device.messaging.intent.REGISTRATION"/>
                <action android:name="com.amazon.device.messaging.intent.RECEIVE"/>
                <category android:name="<your_package_name>"/>
              </intent-filter>
            </receiver>
            
            <receiver android:name="<your_sub_class_of_com.microsoft.azure.engagement.reach.EngagementReachDataPushReceiver>"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.DATA_PUSH" />
              </intent-filter>
            </receiver>
            
            <receiver android:name="com.microsoft.azure.engagement.EngagementLocationBootReceiver"
               android:exported="false">
               <intent-filter>
                  <action android:name="android.intent.action.BOOT_COMPLETED" />
               </intent-filter>
            </receiver>
            
            <receiver android:name="<your_sub_class_of_com.microsoft.azure.engagement.EngagementConnectionReceiver.java>"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.intent.action.CONNECTED"/>
                <action android:name="com.microsoft.azure.engagement.intent.action.DISCONNECTED"/>
              </intent-filter>
            </receiver>
            
            <receiver
              android:name="<your_sub_class_of_com.microsoft.azure.engagement.EngagementMessageReceiver.java>"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.MESSAGE"/>
              </intent-filter>
            </receiver>

Seurannan vastaanottaja on poistettu, joten sinun on poistettava sisältö:

          <receiver android:name="com.ubikod.capptain.android.sdk.track.CapptainTrackReceiver">
            <intent-filter>
              <action android:name="com.ubikod.capptain.intent.action.APPID_GOT" />
              <!-- possibly <action android:name="com.android.vending.INSTALL_REFERRER" /> -->
            </intent-filter>
          </receiver>

Huomaa, että käyttöympäristösi lähetyksen vastaanottaja **EngagementMessageReceiver** koskevan ilmoituksen on muuttunut `AndroidManifest.xml`. Tämä johtuu siitä Ohjelmointirajapinnan haluamaansa XMPP viestien poistaminen haluamaansa XMPP yksiköt ja Lähetä ja voit lähettää ja vastaanottaa viestejä laitteiden välisten Ohjelmointirajapinnan on poistettu. Näin ollen on myös seuraavat takaisinkutsuja poistaminen **EngagementMessageReceiver** käyttöönotto.

            protected void onDeviceMessageReceived(android.content.Context context, java.lang.String deviceId, java.lang.String payload)

ja

            protected void onXMPPMessageReceived(android.content.Context context, android.os.Bundle message)

Poista kaikki kutsu **EngagementAgent** varten:

            sendMessageToDevice(java.lang.String deviceId, java.lang.String payload, java.lang.String packageName)

ja

            sendXMPPMessage(android.os.Bundle msg)

### <a name="proguard"></a>Proguard

Proguard määritys voi vaikuttaa mukaan korostetusti, kuten nyt näyttöä sääntöjä:

            -dontwarn android.**
            -keep class android.support.v4.** { *; }
            
            -keep public class * extends android.os.IInterface
            -keep class com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity$EngagementReachContentJS {
              <methods>;
            }
 
