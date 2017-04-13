<properties
    pageTitle="Esimerkki MyDriving Azure IoT: pikaopas | Microsoft Azure"
    description="Aloittaminen sovellusta, joka on siitä, miten arkkitehti IoT järjestelmän käyttämällä Microsoft Azure, esimerkiksi Stream Analytics koneen oppiminen ja tapahtuman keskittimet täydellinen esittely."
    services=""
    documentationCenter=".net"
    suite=""
    authors="harikmenon"
    manager="douge"/>

<tags
    ms.service="multiple"
    ms.workload="tbd"
    ms.tgt_pltfrm="ibiza"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="03/25/2016"
    ms.author="harikm"/>

# <a name="mydriving-iot-system-quick-start"></a>MyDriving IoT järjestelmän: n pika-aloitusopas

MyDriving on järjestelmä, joka osoittaa suunnitteluun ja toteuttamiseen tyypillinen [Asioita Internet](iot-suite-overview.md) (IoT)-ratkaisun, joka kerää telemetriatietojen laitteilla, käsittelee pilveen tiedot ja konepohjaisten oppimistekniikoiden antamaan mukautuvat vastauksen koskee. Esittely kirjaa tietoja automatkoilla, matkapuhelin ja sovittimen, joka kerää tietoja auton ohjausobjektin järjestelmästä tietoja käyttäen. Se käyttää näitä tietoja, voit antaa palautetta ajo tyylisi verrattuna muille käyttäjille.

MyDriving reaali tarkoituksena on aloittamista IoT-ratkaisun luomiseen. Mutta ennen sen oletetaan, että olet käyttäminen MyDriving-sovelluksen itse--testi käyttäjän tiimimme jäsenenä. Näin sisäänrakennetun sovelluksen ja järjestelmän takana kuluttaja, ennen kuin delve arkkitehtuuri kyselyjä. Se myös esitellään HockeyApp hallinnan sovellus alfa ja beeta-jaot hienoja voi testata käyttäjät.

## <a name="use-the-mobile-experience"></a>Käytä käyttötuntuman mobiililaitteella

Voit käyttää MyDriving sovellus, jos sinulla on Android-, iOS- ja Windows 10-laitteeseen.

### <a name="android-and-windows-10-mobile-installation"></a>Android- ja Windows 10 Mobile-asennuksen

Laitteen:

1.  Salli sovellusten kehittämisen seuraavasti:

    -   **Android: Asetuksissa** > **tietoturva**Salli sovellusten **tuntemattomista**lähteistä.

    -   Windows 10: N **asetukset** > **päivitykset** > **Varten kehittäjät** **kehittäjätilan**määrittämisestä.

2.  Liity beeta-testi tiimimme kirjautuminen tai kirjautumisessa, [HockeyApp](https://rink.hockeyapp.net). HockeyApp on helppo jakaa aikainen sovelluksen Testaa käyttäjät versioihin.

    Jos käytät Windows 10, käytä Edge-selaimella.

    Jos olit muodosta 2016 osallistuja, kirjaudu sisään Microsoft tilin sähköpostiosoitteella saman versio, että olet rekisteröitynyt kokouksen, käyttämällä jotakin Microsoft-painiketta. Olet jo rekisteröityneet palveluun HockeyApp kanssa.

    ![HockeyApp kirjautumisnäyttö](./media/iot-solution-get-started/image1.png)

3.  Lataa ja asenna sovellus täältä:

    -   [Android-laitteeseen](http://rink.io/spMyDrivingAndroid)

    -   [Windows 10: ssä](http://rink.io/spMyDrivingUWP)

    On kaksi osaa. Asenna varmenne **Luotetut henkilöt**. Asenna sovellus sitten.

*Sovelluksen käynnistäminen Windows 10 Mobile ongelmia?* Puhelimen voi olla päivitys tai kaksi takana. Varmista, että luet uusimmat päivitykset tai jos asennat:

 - [Microsoft.NET.Native.Framework.1.2.appx](https://download.hockeyapp.net/packages/win10/Microsoft.NET.Native.Framework.1.2.appx) 

 - [Microsoft.NET.Native.Runtime.1.1.appx](https://download.hockeyapp.net/packages/win10/Microsoft.NET.Native.Runtime.1.1.appx) 

 - [Microsoft.VCLibs.ARM.14.00.appx](https://download.hockeyapp.net/packages/win10/Microsoft.VCLibs.ARM.14.00.appx)


### <a name="ios-installation"></a>iOS-asennus

Jos voit osallistunut muodosta 2016, lataa sovellus HockeyApp testi-tiimimme jäsen:

1.  IOS-laitteessa Kirjaudu [HockeyApp](https://rink.hockeyapp.net).
    Käytä Microsoft-kirjautuminen painikkeiden ja kirjaudu sisään samalla Microsoft-tilin sähköpostiosoitteella, jonka olet rekisteröinyt kokoukseen. (Älä käytä sähköpostiosoite ja salasana-kenttiin.)

    ![HockeyApp kirjautumisnäyttö](./media/iot-solution-get-started/image1.png)

2.  Valitse HockeyApp Raporttinäkymät-ikkunan MyDriving ja lataa se.

3.  Määritä HockeyApp-beetaversion:

    a. Valitse **asetukset** > **Yleiset** > **profiilit ja Laitehallinnan.**

    b. Luotettava **Bittinen STADIUM-ohjelmistoa käyttäen GmbH** .

Jos et osallistu muodosta 2016, voit muodosta ja ota käyttöön sovelluksen itse:

1.   Lataa koodi [-GitHub].

2.   Muodosta ja ota käyttöön [Xamarin]avulla.

Etsi lisätietoja [MyDriving opas](http://aka.ms/mydrivingdocs).

## <a name="get-an-obd-adapter-optional"></a>Hae OBD sovittimen (valinnainen)

Tämä on osa, joka tekee reaali asioita Internet-järjestelmän! Voit käyttää sovelluksen ilman, mutta se on Lisää hauska kanssa reaali kohdetta ja ne eivät ole kallista.

Junassa diagnostiikka (OBD) on auton, joiden korjaamolle auton nopeuttavat ja vianmäärityksen pariton äänet ja varoitusvalosta ominaisuus. Ellei auton on hyvä antiquity, löydät socket johonkin käsimatkatavaroihin yleensä koontinäyttö-kohdassa olevaa takana. Oikea yhdistin voit saada moduulin suorituskyvyn mittarit ja tiettyjen säätäminen. OBD yhdistimen voi ostaa edullisemmin tavallista paikasta. Muodostaa käyttämällä Bluetooth- tai Wi-Fi-sovelluksen puhelimessa.

Tässä tapauksessa tutustutaan auton yhdistäminen pilveen. Suora yhteys OBD on puhelimeesi, mutta monipuolisimmat toimii välitys. Auton telemetriatietojen lähetetään suoraan MyDriving IoT-toiminnossa kohtaa, johon se käsitellään kirjautumaan tien trips ja arvioida ajo tyyli.

Voit liittää OBD-laitteen seuraavasti:

1.  Tarkista, että auton on OBD socket.

2.  Hanki OBD sovittimen:

    -   Jos käytät Android- ja Windows-puhelin, tarvitset Bluetooth käyttävä OBD II sovittimen. On käytetty [BAFX tuotteiden 34t5 Bluetooth OBDII-tarkistustyökalun].

    -   Jos käytät iOS-puhelin, sinun on Wi-Fi-käytössä OBD sovittimen. On käytetty [ScanTool OBDLink MX Wi-Fi-yhteys: OBD sovittimen/diagnostiikka skannerin].

3.  Noudata OBD sovittimen muodostaa puhelimen mukana toimitettavista. Ota huomioon seuraavat asiat:

    -   Puhelimen **asetukset** -sivulla on yhdistettävä Bluetooth-sovittimen.

    -   Wi-Fi-sovittimen on oltava osoite-alueen 192.168.xxx.xxx.

4.  Jos sinulla on useita autot, saat eri sovittimen kunkin (enintään kolme).

Jos sinulla ei ole OBD-sovittimen, sovellus edelleen lähettäminen sijainti ja nopeuden tietojen puhelimen GPS vastaanotin taustatietokantaan ja kysyy, haluatko simuloida OBD.

Voit tarkistaa, lisää siitä, miten sovellus käyttää OBD sovittimen tiedot ja erilaisiin OBD-laitteen luominen 2.1-kohdassa "IoT laitteissa," [MyDriving opas](http://aka.ms/mydrivingdocs).

## <a name="use-the-app"></a>Sovelluksen käyttäminen

Käynnistä sovellus. On alkuperäinen pikaopas, miten se toimii opastusta.

### <a name="track-your-trips"></a>Oman trips seuraaminen

Matkan valitsemalla tietue-painike (suuri punainen ympyrä näytön alareunassa) ja sitten uudelleen loppuun.

![Kuva työmatkan seurantaa varten tietue-painike](./media/iot-solution-get-started/image2.png)

Aina kun käynnistät matkan, jos ei ole OBD-laite, sinua pyydetään haluat käyttää simulator.

Matkan lopussa valitse Pysäytä-painike ja saat yhteenvedon.

![Esimerkki matkan yhteenveto](./media/iot-solution-get-started/image3.png)

### <a name="review-your-trips"></a>Tarkista, että trips

![Esimerkki edellisten työmatkan](./media/iot-solution-get-started/image4.png)

### <a name="review-your-profile"></a>Oman profiilin tarkasteleminen

![Esimerkki ajo-tyyli-profiili](./media/iot-solution-get-started/image5.png)

## <a name="send-us-your-test-feedback"></a>Testaa palautteen lähettäminen

Koska luomaamme MyDriving avulla jumpstart IoT-järjestelmiä, varmasti haluamme kuulla mielipiteesi siitä, miten se toimii. Kerro meille, jos:

- Käytössä ilmenee ongelmia tai haasteita.

- Tällä tunniste-kohdan, jotka tehdä lisää sopiva käyttämässäsi skenaariossa.

- Voit etsiä tehokkaampi tapa tehdä tiettyjen tarpeisiin.

- Sinulla on muita ehdotuksista, jotka MyDriving tai näitä ohjeita.

MyDriving sovelluksen itse, voit käyttää valmiita HockeyApp palaute-järjestelmä: iOS-ja Android-vain antaa puhelimen ravistellaan tai käytä **palaute** -valikkokomento. Tämä liittää näyttökuvan, automaattisesti niin, että olemme tietää, mitä olet puhuminen tietoja. Ja jos mikä tahansa unfortunate kaatuu, HockeyApp kerää kaatumisen lokit kerro niistä. Voit myös tarkastella [HockeyApp portal]palaute.

Voit myös tiedoston [GitHub ongelman]tai jätä kommentin alla (fi-yhteyttä edition).

Odotamme kuuleminen sinulta!

## <a name="next-steps"></a>Seuraavat vaiheet

-   Tutustu selvittääksesi, miten syy on suunniteltu ja sisäisten koko MyDriving järjestelmän [MyDriving opas](http://aka.ms/mydrivingdocs) .

-   [Luo ja ota käyttöön oman järjestelmän](iot-solution-build-system.md) käyttämällä Microsoftin Azure Resurssienhallinta-komentosarjoja. [MyDriving oppaan](http://aka.ms/mydrivingdocs) avulla voit myös alueet, jossa tehdä useimmat mukautukset.

  [Valitse GitHub]: https://github.com/Azure-Samples/MyDriving
  [Xamarin käyttäminen]: https://developer.xamarin.com/guides/ios/getting_started/installation/
  [BAFX tuotteiden 34t5 Bluetooth OBDII tarkistustyökalun]: http://www.amazon.com/gp/product/B005NLQAHS
  [ScanTool OBDLink MX Wi-Fi: OBD sovittimen/diagnostiikka skannerista]: http://www.amazon.com/gp/product/B00OCYXTYY/ref=s9_simh_gw_g263_i1_r?pf_rd_m=ATVPDKIKX0DER&pf_rd_s=desktop-2&pf_rd_r=1MWRMKXK4KK9VYMJ44MP
  [HockeyApp portal]: https://rink.hockeyapp.org
  [Valitse GitHub ongelma]: https://github.com/Azure-Samples/MyDriving/issues
