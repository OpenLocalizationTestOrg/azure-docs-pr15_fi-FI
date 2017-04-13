<properties
    pageTitle="Raportoinnin Azure Mobile välitys Android SDK sijainti"
    description="Tässä artikkelissa käsitellään raportointi Azure Mobile välitys Android SDK sijainnin määrittäminen"
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
    ms.date="08/12/2016"
    ms.author="piyushjo;ricksal" />

# <a name="location-reporting-for-azure-mobile-engagement-android-sdk"></a>Raportoinnin Azure Mobile välitys Android SDK sijainti

> [AZURE.SELECTOR]
- [Android-laitteeseen](mobile-engagement-android-integrate-engagement.md)

Tässä ohjeaiheessa kerrotaan, miten voit tehdä Android-sovelluksen raportoinnin sijainti.

## <a name="prerequisites"></a>Edellytykset

[AZURE.INCLUDE [Prereqs](../../includes/mobile-engagement-android-prereqs.md)]

## <a name="location-reporting"></a>Sijainti raportointi

Sijaintien raportoitava halutessasi sinun on lisättävä muutaman rivin määritys (välillä `<application>` ja `</application>` tunnisteet).

### <a name="lazy-area-location-reporting"></a>Kuitata alueen sijainti raportointi

Kuitata alueen sijainti raportoinnin mahdollistaa raportoinnin maa-, alue- ja laitteisiin liittyvät paikka. Tämän tyyppistä sijainti raportoinnin käyttää vain verkkosijainnit (perustuu solun tunnus tai WIFI). Laitteen-alueella ilmoitetaan enintään kerran istunnossa. GPS on käyttänyt ja näin tällaista sijainti raportti on pieni vaikutus varauksen.

Raportoidun alueet käytetään laskemiseen maantieteelliset tilastotietoja käyttäjille, istuntojen, tapahtumia ja virheet. Niitä voi käyttää myös Reach Kampanjat ehto.

Voit ottaa käyttöön kuitata alueen sijainti raportteja käyttämällä määritysten kerrottiin annettavien ohjeiden mukaisesti:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setLazyAreaLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

Haluat myös määrittää sijainti-oikeudet. Tämä koodi käyttää ``COARSE`` käyttöoikeuksia:

    <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>

Jos sovellus edellyttää sitä, voit käyttää ``ACCESS_FINE_LOCATION`` sijaan.

### <a name="real-time-location-reporting"></a>Reaaliaikainen sijainti raportointi

Reaaliaikaista sijaintia raportointi mahdollistaa leveyttä ja laitteisiin liittyvät pituutta. Tämän tyyppistä sijainti raportoinnin käyttää oletusarvon mukaan vain verkkosijainnit, solun tunnus tai WIFI. Raportoinnin on käytettävissä vain, kun sovellus suoritetaan edustalla (esimerkiksi istunnossa).

Reaaliaikainen sijainnit eivät *ole* käyttää laskemiseen tilastotiedot. Vain niiden tarkoituksena on sallittua käyttää reaaliaikaisia geo-aitaus \<Reach käyttäjäryhmä-geofencing\> Reach Kampanjat ehto.

Jotta reporting reaaliaikaista sijaintia, jossa välitys yhteysmerkkijonon määrittää avain tehtävän koodin lisääminen. Tulos näyttää seuraavalta:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

        You also need to specify a location permission. This code uses ``COARSE`` permission:

            <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>

        If your app requires it, you can use ``ACCESS_FINE_LOCATION`` instead.

#### <a name="gps-based-reporting"></a>GPS perusteella raportointi

Käyttää oletusarvon mukaan reaaliaikaista sijaintia raportointi vain verkkopohjainen sijainnit. Voit ottaa GPS-pohjainen sijainneista ovat paljon tarkemmin, käytä kokoonpano-objekti:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    engagementConfiguration.setFineRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

Sinun on myös lisättävä seuraavat oikeudet, jos puuttuu:

    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>

#### <a name="background-reporting"></a>Taustan raportointi

Oletusarvon mukaan reaaliaikaista sijaintia raportointi on aktiivinen vain, kun sovellus suoritetaan edustalla (esimerkiksi istunnossa). Käyttöön myös taustalla, käytä tämän kokoonpano-objekti:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    engagementConfiguration.setBackgroundRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

> [AZURE.NOTE] Kun sovellus suoritetaan taustalla, vain verkkopohjainen sijainnit raportoidaan, vaikka GPS käytössä.

Jos käyttäjä käynnistyy niiden laitteen, pysäytetään taustan sijainti-raportti. Lisää tämä koodi, jos haluat sen uudelleen automaattisesti tietokoneen käynnistyksen yhteydessä.

    <receiver android:name="com.microsoft.azure.engagement.EngagementLocationBootReceiver"
           android:exported="false">
        <intent-filter>
            <action android:name="android.intent.action.BOOT_COMPLETED" />
        </intent-filter>
    </receiver>

Sinun on myös lisättävä seuraavat oikeudet, jos puuttuu:

    <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />

## <a name="android-m-permissions"></a>Android M-käyttöoikeudet

Android M alkaen joitakin käyttöoikeuksia hallitaan suorituksen ja on käyttäjän hyväksyntää.

Jos laadit Android vaatii tason 23, runtime-käyttöoikeudet poistettu käytöstä oletusarvoisesti uuden sovelluksen asentaminen. Muussa tapauksessa ne ovat oletusarvoisesti käytössä.

Voit voit ottaa käyttöön/poistaa käytöstä käyttöoikeudet periytyvät laitteen asetukset-valikko. Sovelluksen, joka on järjestelmän toimintaa ja ei ole vaikutusta, mahdollisuus saada push taustalla taustaprosessit keskeyttää järjestelmävalikon käyttöoikeudet käytöstä.

Mobile välitys sijainti raportoinnin yhteydessä suorituksen hyväksyntää vaativien käyttöoikeudet ovat:

- `ACCESS_COARSE_LOCATION`
- `ACCESS_FINE_LOCATION`

Pyydä oikeudet käyttäjän vakio Järjestelmä-valintaikkunassa. Jos käyttäjä hyväksyy ``EngagementAgent`` ottaa huomioon, että muuta reaaliajassa. Muussa tapauksessa muutos käsitellään käyttäjän käynnistää sovelluksen seuraavan kerran.

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
         * Request location permission, but this doesn't explain why it is needed to the user.
         * The standard Android documentation explains with more details how to display a rationale activity to explain the user why the permission is needed in your application.
         * Putting COARSE vs FINE has no impact here, they are part of the same group for runtime permission management.
         */
        if (checkSelfPermission(android.Manifest.permission.ACCESS_FINE_LOCATION) != PackageManager.PERMISSION_GRANTED)
          requestPermissions(new String[] { android.Manifest.permission.ACCESS_FINE_LOCATION }, 0);

      }
    }

    @Override
    public void onRequestPermissionsResult(int requestCode, String[] permissions, int[] grantResults)
    {
      /* Only a positive location permission update requires engagement agent refresh, hence the request code matching from above function */
      if (requestCode == 0 && grantResults[0] == PackageManager.PERMISSION_GRANTED)
        getEngagementAgent().refreshPermissions();
    }
