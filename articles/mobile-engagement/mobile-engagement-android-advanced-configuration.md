<properties
    pageTitle="Azure Mobile välitys Android SDK lisämäärityksiä"
    description="Tässä artikkelissa kuvataan lisämäärityksiä-asetuksia, kuten Android luettelon kanssa Azure Mobile välitys Android SDK-paketissa"
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
    ms.date="10/04/2016"
    ms.author="piyushjo;ricksal" />

# <a name="advanced-configuration-for-azure-mobile-engagement-android-sdk"></a>Azure Mobile välitys Android SDK lisämäärityksiä

> [AZURE.SELECTOR]
- [Yleinen Windows](mobile-engagement-windows-store-advanced-configuration.md)
- [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md)
- [iOS](mobile-engagement-ios-integrate-engagement.md)
- [Android-laitteeseen](mobile-engagement-android-advanced-configuration.md)

Tässä kuvataan määrittäminen Azure Mobile välitys Android-sovellusten asetuksia.

## <a name="prerequisites"></a>Edellytykset

[AZURE.INCLUDE [Prereqs](../../includes/mobile-engagement-android-prereqs.md)]

## <a name="permission-requirements"></a>Käyttöoikeus-vaatimukset
Jotkin asetukset vaativat tietyt käyttöoikeudet, jotka on lueteltu täällä viittaus ja sitoutuvat-ominaisuutta. Lisää nämä oikeudet projektin AndroidManifest.xml välittömästi ennen tai jälkeen `<application>` tunniste.

Käyttöoikeus-koodi on näyttää seuraavalta, jos täytät taulukosta, joka on tarvittavat käyttöoikeudet.

    <uses-permission android:name="android.permission.[specific permission]"/>


| Käyttöoikeudet | Kun sitä käytetään |
| ---------- | --------- |
| INTERNET | Pakollinen. Perus-raportointia varten |
| ACCESS_NETWORK_STATE | Pakollinen. Perus-raportointia varten |
| RECEIVE_BOOT_COMPLETED | Pakollinen. Näyttöleikkeen ilmoitukset keskelle laitteen uudelleenkäynnistyksen jälkeen |
| WAKE_LOCK | Suositeltava. Mahdollistaa tietojen keräämisen WiFi käytettäessä tai jos näyttö on poistettu käytöstä |
| VÄRINÄTOIMINNOT | Valinnainen. Mahdollistaa tärinä ilmoitukset saapuessa |
| DOWNLOAD_WITHOUT_NOTIFICATION | Valinnainen. Mahdollistaa Android laajemmin ilmoitus |
| WRITE_EXTERNAL_STORAGE | Valinnainen. Mahdollistaa Android laajemmin ilmoitus |
| ACCESS_COARSE_LOCATION | Valinnainen. Mahdollistaa reaaliaikaista sijaintia raportointi |
| ACCESS_FINE_LOCATION | Valinnainen. Mahdollistaa GPS-pohjainen sijainti raportointi |

Alkaen Android M, [joitakin käyttöoikeuksia hallitaan suorituksen aikana](mobile-engagement-android-location-reporting.md#Android-M-Permissions).

Jos käytössäsi on jo ``ACCESS_FINE_LOCATION``, valitse sinun ei tarvitse käyttää myös ``ACCESS_COARSE_LOCATION``.

## <a name="android-manifest-configuration-options"></a>Android luettelon kokoonpanoasetusten määrittäminen

### <a name="crash-report"></a>Kaatumisraportti

Käytöstä kaatumisen raportteja, lisää koodi välillä `<application>` ja `</application>` tunnisteet:

    <meta-data android:name="engagement:reportCrash" android:value="false"/>

### <a name="burst-threshold"></a>Purskeen kynnysarvo

Oletusarvon mukaan välitys raportit kirjaa reaaliaikaisesti. Jos sovelluksen raportin lokit vaihtelevat usein, on parempi puskurin lokit ja raportin ne kaikki kerralla säännöllisin väliajoin, perus (eli "#pursketila"). Voit tehdä tämän välillä-koodin lisääminen `<application>` ja `</application>` tunnisteet:

    <meta-data android:name="engagement:burstThreshold" android:value="{interval between too bursts (in milliseconds)}"/>

Purskeen tila on hieman kasvaa varauksen aika, mutta se on vaikutusta välitys näytöltä: kaikki istuntoa ja töiden kesto pyöristetään purskeen raja (siis istunnot ja töiden lyhyempi kuin purskeen raja-arvo ei ehkä näy). Purskeen raja-arvon pitäisi olla enempää kuin 30000 (30s).

### <a name="session-timeout"></a>Istunnon aikakatkaisu

 Voit lopettaa tehtävän **Aloitus** - tai **taaksepäin** -näppäimen painaminen, määrittämällä käyttämättömänä puhelin tai hyppäämällä toiseen sovellukseen. Oletusarvon mukaan istunnon lopetetaan 10 sekuntia viimeisen toimintansa päättymisen jälkeen. Tämä ei istunnon jaon aina, kun käyttäjä poistuu ja palaa sovelluksen nopeasti, joka voi tapahtuu, kun käyttäjä valitsee määrittäminen kuva-tarkistaa ilmoitus, jne. Voit muokata tätä parametria. Tee lisää koodi välillä `<application>` ja `</application>` tunnisteet:

    <meta-data android:name="engagement:sessionTimeout" android:value="{session timeout (in milliseconds)}"/>

## <a name="disable-log-reporting"></a>Lokitiedoston raportoinnin poistaminen käytöstä

### <a name="using-a-method-call"></a>Menetelmä puhelun

Jos halua enää halua lähettää lokit välitys, voit soittaa:

    EngagementAgent.getInstance(context).setEnabled(false);

Tämä kutsu on jatkuva: se käyttää jaettua asetustiedoston.

Jos välitys on valittuna, kun soitat tätä toimintoa, voi kestää minuutin kuluttua-palveluun, jos haluat lopettaa. Mutta se ei käynnistää palvelun lainkaan seuraavan kerran käynnistät sovelluksen.

Voit ottaa lokin raportoinnin uudelleen soittamalla kanssa samaan toimintoon `true`.

### <a name="integration-in-your-own-preferenceactivity"></a>Oman integrointi`PreferenceActivity`

Sen sijaan, että puhelut tätä toimintoa, voit myös integroida tämän asetuksen suoraan-nykyinen `PreferenceActivity`.

Voit määrittää välitys käyttämään asetustiedoston (ja haluamasi tila) `AndroidManifest.xml` tiedostoa, jonka `application meta-data`:

-   `engagement:agent:settings:name` Avaimen avulla jaettuun asetustiedoston nimi.
-   `engagement:agent:settings:mode` Avaimen avulla määrittää jaetun asetustiedoston tilassa. Käyttävät samaa kuin oman `PreferenceActivity`. Tila on siirrettävä lukuna: Jos käytössäsi on vakio lippujen yhdistelmä omassa koodissasi, tarkista kokonaisarvon.

Välitys aina käyttää `engagement:key` totuusarvo avain hallintaan asetus asetukset-tiedostossa.

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
