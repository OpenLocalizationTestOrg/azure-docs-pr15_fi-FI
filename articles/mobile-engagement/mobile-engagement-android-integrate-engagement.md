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

#<a name="how-to-integrate-engagement-on-android"></a>Miten voit integroida Android-välitys

> [AZURE.SELECTOR]
- [Windowsin Universal](mobile-engagement-windows-store-integrate-engagement.md)
- [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md)
- [iOS](mobile-engagement-ios-integrate-engagement.md)
- [Android-laitteeseen](mobile-engagement-android-integrate-engagement.md)

Tässä kuvataan helpoin tapa Aktivoi välitys 's Analytics seurantaan ja valvontaan Funktiot Android-sovelluksessa.

> [AZURE.IMPORTANT] Pienin Android SDK-Ohjelmointirajapinnan-tason on oltava 10 tai uudempi (Android 2.3.3 tai uudempi).

Seuraavat vaiheet ovat riittävän Aktivoi lokit tarpeen laskea tilastoja kaikki käyttäjät, istunnot, aktiviteetteja, kaatuu ja Technicals raportti. Lokit tarpeen laskea muiden tilastotietoja, kuten tapahtumia, virheitä ja töiden raportti on tehtävä manuaalisesti välitys-Ohjelmointirajapinnan käyttäminen (katso [käyttämisestä Lisäasetukset välitys Mobile-Ohjelmointirajapinnan Android tunnisteita](mobile-engagement-android-use-engagement-api.md) , koska nämä tilasto on sovellusta riippuvaisten.

##<a name="embed-the-engagement-sdk-and-service-into-your-android-project"></a>Välitys SDK ja palvelun upottaminen Android Project-ohjelmaan

Lataa Android SDK [tähän](https://aka.ms/vq9mfn) Get `mobile-engagement-VERSION.jar` ja sijoittaa ne `libs` kansion Android projektin (luominen kirjastoon-kansioon, jos se ei ole vielä).

> [AZURE.IMPORTANT]
> Voit luoda oman sovelluspaketin ProGuard kanssa, jos haluat säilyttää jotkin luokat. Voit käyttää seuraavia määritysten koodikatkelman:
>
>
            -keep public class * extends android.os.IInterface
            -keep class com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity$EngagementReachContentJS {
            <methods>;
            }

Määritä välitys yhteysmerkkijonon soittamalla käynnistys-toiminnossa seuraavalla tavalla:

            EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
            engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
            EngagementAgent.getInstance(this).init(engagementConfiguration);

Sovelluksen yhteysmerkkijonon näkyy Azure-portaalissa.

-   Jos puuttuu, Lisää seuraavat Android käyttöoikeudet (ennen `<application>` tunniste):

            <uses-permission android:name="android.permission.INTERNET"/>
            <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>

-   Lisää seuraavan osan (välillä `<application>` ja `</application>` tunnisteet):

            <service
              android:name="com.microsoft.azure.engagement.service.EngagementService"
              android:exported="false"
              android:label="<Your application name>Service"
              android:process=":Engagement"/>

-   Muuta `<Your application name>` sovelluksen nimen mukaan.

> [AZURE.TIP] `android:label` Määritteen avulla voit valita välitys-palvelun nimi, ne näkyvät loppukäyttäjien "Servicesiä" näytössä puhelimeensa. On suositeltavaa, voit määrittää tämän määritteen `"<Your application name>Service"` (kuten `"AcmeFunGameService"`).

Määrittämällä `android:process` määrite varmistat, että välitys-palvelun suoritetaan omassa prosessissa (käynnissä samoja ohjeita välitys, kun sovellus tekee pää/Käyttöliittymän viestiketjun mahdollisesti vähemmän vastaa).

> [AZURE.NOTE] Voit lisätä koodia `Application.onCreate()` ja muiden sovellusten takaisinkutsuja suorittavat kaikki sovelluksesi kannalta, mukaan lukien välitys-palvelun. Se on ehkä ei-toivottuja puoli tehosteita (kuten tarpeettomat muistin kohdistukset ja viestiketjuissa siirtyminen prosessi, kaksoiskappaleiden lähetyksen vastaanottajia tai palveluja välitys).

Jos voit ohittaa `Application.onCreate()`, on suositeltavaa, Lisää seuraavat koodikatkelman alussa oman `Application.onCreate()` funktiota:

             public void onCreate()
             {
               if (EngagementAgentUtils.isInDedicatedEngagementProcess(this))
                 return;

               ... Your code...
             }

Voit tehdä saman myös `Application.onTerminate()`, `Application.onLowMemory()` ja `Application.onConfigurationChanged(...)`.

Voit myös laajentaa `EngagementApplication` sijaan laajentaminen `Application`: takaisinkutsu `Application.onCreate()` onko prosessi-valintaruutu ja puhelut `Application.onApplicationProcessCreate()` vain, jos nykyinen prosessi ei ole välitys isännöintipalvelu yksi, muut takaisinkutsuja sovelletaan samoja sääntöjä.

##<a name="basic-reporting"></a>Tavallinen raportointi

### <a name="recommended-method-overload-your-activity-classes"></a>Suositeltu tapa: ylikuormituksen oman `Activity` luokat

Kaikki käyttäjät, istunnot, aktiviteetteja, kaatuu ja teknisistä tiedoista laskemiseen välitys vaatii lokit raportin aktivoimista sinulla on tehdä kaikki oman `*Activity` aliraportti luokat perivät vastaavan `Engagement*Activity` luokat (esimerkiksi Jos vanha toimiessasi ulottuu `ListActivity`, tee se ulottuu `EngagementListActivity`).

**Ilman välitys:**

            package com.company.myapp;

            import android.app.Activity;
            import android.os.Bundle;

            public class MyApp extends Activity
            {
              @Override
              public void onCreate(Bundle savedInstanceState)
              {
                super.onCreate(savedInstanceState);
                setContentView(R.layout.main);
              }
            }

**Välitys: kanssa**

            package com.company.myapp;

            import com.microsoft.azure.engagement.activity.EngagementActivity;
            import android.os.Bundle;

            public class MyApp extends EngagementActivity
            {
              @Override
              public void onCreate(Bundle savedInstanceState)
              {
                super.onCreate(savedInstanceState);
                setContentView(R.layout.main);
              }
            }

> [AZURE.IMPORTANT] Käytettäessä `EngagementListActivity` tai `EngagementExpandableListActivity`, varmista, että jokin kutsu `requestWindowFeature(...);` tehdään ennen kutsun `super.onCreate(...);`, muuten kaatumisen tapahtuu.

Näitä luokkia voit etsiä `src` -kansiossa ja kopioida ne projektiin. Luokkia on myös **JavaDoc**.

### <a name="alternate-method-call-startactivity-and-endactivity-manually"></a>Vaihtoehtoinen menetelmä: soita `startActivity()` ja `endActivity()` manuaalisesti

Jos et halua ylikuormittaa tai ei onnistu oman `Activity` luokkia, voit aloittaa ja lopettaa toimintojen soittamalla sijaan `EngagementAgent`'s menetelmiä suoraan.

> [AZURE.IMPORTANT] Android SDK koskaan soittaa `endActivity()` menetelmä myös silloin, kun sovellus suljetaan (Android-sovellukset on todella koskaan suljettu). Joten se on *erittäin* suositeltavaa Soita `startActivity()` menetelmä `onResume` takaisinsoitto *All* toimintosi ja `endActivity()` menetelmä `onPause()` takaisinsoitto *All* toimintojen. Tämä on ainoa tapa toiseen ja varmistamalla, että istunnot ei vuotamaan. Jos istunnon vuotamaan, välitys-palvelun katkaisee koskaan välitys Taustajärjestelmä (koska palvelun edelleen liitettynä, kunhan istunnon odottaa).

Tässä on esimerkki:

            public class MyActivity extends Some3rdPartyActivity
            {
              @Override
              protected void onResume()
              {
                super.onResume();
                String activityNameOnEngagement = EngagementAgentUtils.buildEngagementActivityName(getClass()); // Uses short class name and removes "Activity" at the end.
                EngagementAgent.getInstance(this).startActivity(this, activityNameOnEngagement, null);
              }

              @Override
              protected void onPause()
              {
                super.onPause();
                EngagementAgent.getInstance(this).endActivity();
              }
            }

Tässä esimerkissä muistuttaa `EngagementActivity` luokan ja sen muunnokset, joiden lähdekoodi on säädetty `src` kansio.

##<a name="test"></a>Testi

Nyt Tarkista integrointi suorittamalla mobile-sovellus emulaattorin tai laitteessa ja varmistetaan, että se rekisteröi istunnon valvonta-välilehdessä.

Seuraavat osiot ovat valinnaisia.

##<a name="location-reporting"></a>Sijainti raportointi

Sijaintien raportoitava halutessasi sinun on lisättävä muutaman rivin määritys (välillä `<application>` ja `</application>` tunnisteet).

### <a name="lazy-area-location-reporting"></a>Kuitata alueen sijainti raportointi

Kuitata alueen sijainti reporting avulla voit raportoida maa-, alue- ja laitteisiin liittyvät paikka. Tämän tyyppistä sijainti raportoinnin käyttää vain verkkosijainnit (perustuu solun tunnus tai WIFI). Laitteen-alueella ilmoitetaan enintään kerran istunnossa. GPS on käyttänyt ja näin ollen tällaista sijainti raportti on hyvin vähän (ei haluat sanoa ei) varauksen vaikutus.

Raportoidun alueet käytetään laskemiseen maantieteelliset tilastotietoja käyttäjille, istunnot, tapahtumia ja virheet. Niitä voi käyttää myös Reach Kampanjat ehto.

Käyttöön kuitata alueen sijainti, voit tehdä sen käyttämällä määritysten kerrottiin annettavien ohjeiden mukaisesti:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setLazyAreaLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

Sinun on myös lisättävä seuraavat oikeudet, jos puuttuu:

            <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>

Tai voit jatkaa ``ACCESS_FINE_LOCATION`` Jos jo käytät sitä sovelluksen.

### <a name="real-time-location-reporting"></a>Reaaliaikainen sijainti raportointi

Reaaliaikainen sijainti raportointi avulla raportoimaan leveyttä ja laitteisiin liittyvät pituutta. Oletusarvon mukaan tällaista sijainti raportoinnin käyttää vain verkkosijainnit (perustuu solun tunnus tai WIFI) ja raportoinnin on käytettävissä vain, kun sovellus suoritetaan edustalla (eli istunnossa).

Reaaliaikainen sijainnit eivät *ole* käyttää laskemiseen tilastotiedot. Vain niiden tarkoitus on sallittua käyttää reaaliajassa geo-aitaus \<Reach osallistujat-geofencing\> Reach Kampanjat ehto.

Reaaliaikainen sijainti raportoinnin käyttöön voit tehdä sen käyttämällä kerrottiin prosessin myöhemmässä vaiheessa määritykset:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

Sinun on myös lisättävä seuraavat oikeudet, jos puuttuu:

            <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>

Tai voit jatkaa ``ACCESS_FINE_LOCATION`` Jos jo käytät sitä sovelluksen.

#### <a name="gps-based-reporting"></a>GPS perusteella raportointi

Käyttää oletusarvon mukaan reaaliaikainen sijainti raportointi vain mukaan verkkosijainnit. Voit ottaa mukaan GPS sijainneista (jotka ovat paljon tarkemmin), käytä kokoonpano-objekti:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    engagementConfiguration.setFineRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

Sinun on myös lisättävä seuraavat oikeudet, jos puuttuu:

            <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>

#### <a name="background-reporting"></a>Taustan raportointi

Oletusarvon mukaan reaaliaikainen sijainti raportointi on aktiivinen vain, kun sovellus suoritetaan edustalla (eli istunnossa). Käyttöön myös taustalla, käytä kokoonpano-objekti:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    engagementConfiguration.setBackgroundRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

> [AZURE.NOTE] Kun sovellus toimii taustalla, vain verkon perusteella sijainnit raportoidaan, vaikka GPS käytössä.

Taustakuvan sijainti raportin lopetetaan, jos käyttäjä käynnistyy laitteensa, voit lisätä tämän sen uudelleen automaattisesti tietokoneen käynnistyksen yhteydessä:

            <receiver android:name="com.microsoft.azure.engagement.EngagementLocationBootReceiver"
               android:exported="false">
               <intent-filter>
                  <action android:name="android.intent.action.BOOT_COMPLETED" />
               </intent-filter>
            </receiver>

Sinun on myös lisättävä seuraavat oikeudet, jos puuttuu:

            <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />

### <a name="android-m-permissions"></a>Android M-käyttöoikeudet

Android M alkaen joitakin käyttöoikeuksia hallitaan suorituksen ja tarvitsee käyttäjän hyväksynnän.

Runtimen käyttöoikeudet poistetaan käytöstä oletusarvoisesti uuden sovelluksen asentaminen Jos laadit Android vaatii tason 23. Muussa tapauksessa se otetaan käyttöön oletusarvoisesti.

Käyttäjän voi ottaa käyttöön/poistaa käytöstä käyttöoikeudet periytyvät laitteen asetukset-valikko. Järjestelmävalikon käyttöoikeudet käytöstä keskeyttää taustaprosessit-sovelluksen, tämä on järjestelmän toimintaa, joten ei ole vaikutusta, mahdollisuus saada push taustalla.

Mobile välitys kontekstissa suorituksen hyväksyntää vaativien käyttöoikeudet ovat:

- `ACCESS_COARSE_LOCATION`
- `ACCESS_FINE_LOCATION`
- `WRITE_EXTERNAL_STORAGE`(vain, kun kohdistus tämä taso Android API 23)

Ulkoinen tallennustilan käytetään vain Reach ylemmän tason ominaisuus. Jos löydät kysyy käyttäjät voi häiritä nämä käyttöoikeudet, voit poistaa sen Jos olet käyttänyt sitä vain Mobile välitys mutta kustannuksella ylemmän tason poistaminen käytöstä.

Sijainti-ominaisuudet kannattaa pyytää oikeutta käyttäjän vakio Järjestelmä-valintaikkunan. Jos käyttäjä hyväksyy, onko annettava ``EngagementAgent`` ottaa huomioon, että muuta reaaliajassa (muussa muutoksen käsitellään käyttäjän käynnistää sovelluksen seuraavan kerran).

Tässä on esimerkki koodi sovelluksen toimintaa avulla pyytää käyttöoikeuksia ja tulos eteenpäin, jos se on positiivinen, ``EngagementAgent``:

    @Override
    public void onCreate(Bundle savedInstanceState)
    {
      /* Other code... */

      /* Request permissions */
      requestPermissions();
    }

    @TargetApi(Build.VERSION_CODES.M)
    private void requestPermissions()
    {
      /* Avoid crashing if not on Android M */
      if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.M)
      {
        /*
         * Request location permission, but this won't explain why it is needed to the user.
         * The standard Android documentation explains with more details how to display a rationale activity to explain the user why the permission is needed in your application.
         * Putting COARSE vs FINE has no impact here, they are part of the same group for runtime permission management.
         */
        if (checkSelfPermission(android.Manifest.permission.ACCESS_FINE_LOCATION) != PackageManager.PERMISSION_GRANTED)
          requestPermissions(new String[] { android.Manifest.permission.ACCESS_FINE_LOCATION }, 0);

        /* Only if you want to keep features using external storage */
        if (checkSelfPermission(android.Manifest.permission.WRITE_EXTERNAL_STORAGE) != PackageManager.PERMISSION_GRANTED)
          requestPermissions(new String[] { android.Manifest.permission.WRITE_EXTERNAL_STORAGE }, 1);
      }
    }

    @Override
    public void onRequestPermissionsResult(int requestCode, String[] permissions, int[] grantResults)
    {
      /* Only a positive location permission update requires engagement agent refresh, hence the request code matching from above function */
      if (requestCode == 0 && grantResults[0] == PackageManager.PERMISSION_GRANTED)
        getEngagementAgent().refreshPermissions();
    }

##<a name="advanced-reporting"></a>Laajennettu raportointi

Voit myös halutessasi sovelluksen tiettyjä tapahtumia, virheet ja töiden raportoimaan sinun on käytettävä välitys Ohjelmointirajapinnan kautta menetelmät `EngagementAgent` luokka. Objektin tämän luokan voi olla retreived soittamalla `EngagementAgent.getInstance()` staattinen menetelmä.

Välitys Ohjelmointirajapinnan avulla voit käyttää kaikkia välitys 's lisäominaisuuksia ja miten yksityiskohtaisesti välitys Ohjelmointirajapinnan käyttäminen Android (sekä teknisten Valitse `EngagementAgent` luokan).

##<a name="advanced-configuration-in-androidmanifestxml"></a>Määritysten Lisäasetukset (-AndroidManifest.xml)

### <a name="wake-locks"></a>Aktivoi lukitukset

Jos haluat varmistaa, että tilastotiedot lähetetään reaaliajassa Wifi käytettäessä tai näytön ei ole käytössä, Lisää seuraava valinnainen käyttöoikeuksia:

            <uses-permission android:name="android.permission.WAKE_LOCK"/>

### <a name="crash-report"></a>Kaatumisraportti

Jos haluat poistaa käytöstä kaatumisen raportteja, Lisää tämä (välillä `<application>` ja `</application>` tunnisteet):

            <meta-data android:name="engagement:reportCrash" android:value="false"/>

### <a name="burst-threshold"></a>Purskeen kynnysarvo

Oletusarvon mukaan välitys raportit kirjaa reaaliaikaisesti. Jos sovellus ilmoittaa lokit hyvin usein, on parempi puskurin lokit ja raportoida ne kaikki kerralla Normaali aika perus (eli "purskeen tilassa"). Tee Lisää tämä (välillä `<application>` ja `</application>` tunnisteet):

            <meta-data android:name="engagement:burstThreshold" android:value="{interval between too bursts (in milliseconds)}"/>

Purskeen tila on hieman Suurenna varauksen aika, mutta se on vaikutusta välitys näytöltä: kaikki istuntoa ja töiden kesto pyöristetään purskeen raja (siis istunnot ja töiden lyhyempi kuin purskeen raja-arvo ei ehkä näy). On suositeltavaa käyttää burst-kynnysarvo enää-kuin 30000 (30s).

### <a name="session-timeout"></a>Istunnon aikakatkaisu

Oletusarvon mukaan istunto on päättynyt 10s (joka tapahtuu yleensä painamalla Home- tai Edellinen-näppäintä, voit määrittää puhelimen käyttämättömänä tai hyppäämällä toiseen sovellukseen) viimeisen toimintansa päättymisen jälkeen. Näin voit välttää istunnon jaon kunkin, kun käyttäjä Lopeta ja palaa sovelluksen nopeasti (joka ilmaantui kun kuva-, nosta hän tarkistaa ilmoitus, jne.). Voit muokata tätä parametria. Tee Lisää tämä (välillä `<application>` ja `</application>` tunnisteet):

            <meta-data android:name="engagement:sessionTimeout" android:value="{session timeout (in milliseconds)}"/>

##<a name="disable-log-reporting"></a>Lokitiedoston raportoinnin poistaminen käytöstä

### <a name="using-a-method-call"></a>Menetelmä puhelun

Jos halua enää halua lähettää lokit välitys, voit soittaa:

            EngagementAgent.getInstance(context).setEnabled(false);

Tämä kutsu on jatkuva: se käyttää jaettua asetustiedoston.

Jos välitys on valittuna, kun soitat tätä toimintoa, voi kestää minuutin-palveluun, jos haluat lopettaa. Mutta se ei käynnistää palvelun lainkaan seuraavan kerran käynnistät sovelluksen.

Voit ottaa lokin raportoinnin uudelleen soittamalla kanssa samaan toimintoon `true`.

### <a name="integration-in-your-own-preferenceactivity"></a>Oman integrointi`PreferenceActivity`

Sen sijaan, että puhelut tätä toimintoa, voit myös integroida tämän asetuksen suoraan-nykyinen `PreferenceActivity`.

Voit määrittää välitys käyttämään asetustiedoston (ja haluamasi tila) `AndroidManifest.xml` tiedostoa, jonka `application meta-data`:

-   `engagement:agent:settings:name` Avaimen avulla jaettuun asetustiedoston nimi.
-   `engagement:agent:settings:mode` Avaimen avulla määrittää jaetun asetustiedoston tilassa, sinun on käytettävä samaa tilassa kuin oman `PreferenceActivity`. Tila on siirrettävä lukuna: Jos käytössäsi on vakio lippujen yhdistelmä omassa koodissasi, tarkista kokonaisarvon.

Käytä aina välitys `engagement:key` totuusarvo avain asetustiedoston hallintaan tämän asetuksen.

Seuraavassa esimerkissä `AndroidManifest.xml` näyttää oletusarvot:

            <application>
                [...]
                <meta-data
                  android:name="engagement:agent:settings:name"
                  android:value="engagement.agent" />
                <meta-data
                  android:name="engagement:agent:settings:mode"
                  android:value="0" />

Voit lisätä `CheckBoxPreference` preference rakenne muistuttavan seuraavat:

            <CheckBoxPreference
              android:key="engagement:enabled"
              android:defaultValue="true"
              android:title="Use Engagement"
              android:summaryOn="Engagement is enabled."
              android:summaryOff="Engagement is disabled." />

<!-- URLs. -->
[Device API]: http://go.microsoft.com/?linkid=9876094
