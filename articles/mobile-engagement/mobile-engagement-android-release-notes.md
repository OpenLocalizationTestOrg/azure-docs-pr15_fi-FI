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
    ms.date="08/10/2016"
    ms.author="piyushjo" />

# <a name="release-notes"></a>Julkaisutiedot

## <a name="423-08102016"></a>4.2.3 (08/10/2016)

- Ei lisää WIFI lock.
- Korjaa tehtävä soitettaessa getDeviceId ennen alusta (ohjelmavirhe 4.2.0 käyttöön).

## <a name="422-05172016"></a>4.2.2 (05/17/2016)

- Vakauteen.

## <a name="421-05102016"></a>4.2.1 (05/10/2016)

- Suojaus: Poista käytöstä web näkymän paikallisen tiedoston Accessin.
- Suojaus: Poista `EngagementPreferenceActivity` luokka, joka vanhentuneen ja suojaamaton `PreferenceActivity` luokka.
- Suojaus: reach toiminnot on nyt kuvattu käyttämään `exported="false"`, tämä lippu voidaan myös SDK aiemmissa versioissa.

## <a name="420-03112016"></a>4.2.0 (03/11/2016)

- MIT nyt alaisen SDK.
- Salli SDK alustus aikaa, joka määrittää mukautetun laitteen tunnus.

## <a name="415-02012016"></a>4.1.5 (02/01/2016)

- Vakauteen.

## <a name="414-01262016"></a>4.1.4 (01/26/2016)

- Vakauteen.

## <a name="413-1292015"></a>4.1.3 (12/9/2015)

- Vakauteen.

## <a name="412-11252015"></a>4.1.2 (11/25/2015)

- Vakauteen.

## <a name="411-11042015"></a>4.1.1 (11/04/2015)

- Vakauteen.

## <a name="410-08252015"></a>4.1.0 (08/25/2015)

- Käsittele uuden käyttöoikeudet mallin Android m.
- Nyt määrittää sijainnin ominaisuuksia suorituksen sijaan `AndroidManifest.xml`.
- Korjaa käyttöoikeuksia ohjelmavirhe: Jos käytät `ACCESS_FINE_LOCATION`, valitse `ACCESS_COARSE_LOCATION` ei enää tarvita.
- Vakauteen.

## <a name="400-07062015"></a>4.0.0 (07-06-2015)

-   Sisäinen protokolla muutokset, jotta analyysin ja push luotettava.
-   Alkuperäisen push (GCM/ADM) nyt myös käytetään sovellusten ilmoitukset niin, että sinun on määritettävä kaikenlaisia push markkinointikampanjan alkuperäisen push tunnistetietojen.
-   Korjaa ylemmän tason ilmoitus: siirretyt näytössä vain 10s niiden ollessa.
-   Korjaa virhe web-näkymässä: linkin napsauttaminen on myös suoritetaan toiminnon URL-Osoitteen.
-   Korjaa harvinaisissa kaatumisen liittyvät paikallisen tallennusvälineiden hallinta.
-   Korjaa dynaamisen merkkijonon hallinta.
-   Päivitä käyttöoikeussopimus.

## <a name="300-02172015"></a>3.0.0 (02-17-2015)

-   Ensimmäinen versio Azure Mobile välitys
-   sovelluksen tunnus määritys on korvattu merkkijonon yhteysmäärityksen.
-   Poistaa Ohjelmointirajapinnan lähettää ja vastaanottaa haluamaansa XMPP viestien haluamaansa XMPP kohteita.
-   Poistaa API voit lähettää ja vastaanottaa viestejä laitteiden välillä.
-   Parannettu suojaus.
-   Google Play- ja SmartAd seurantaa poistetaan.
