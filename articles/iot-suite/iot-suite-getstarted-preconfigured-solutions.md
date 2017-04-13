<properties
    pageTitle="Aloita esimääritetyt ratkaisut | Microsoft Azure"
    description="Katso tämä opetusohjelma, jotta Azure IoT Suite valmiiksi-ratkaisun käyttöönotto."
    services=""
    suite="iot-suite"
    documentationCenter=""
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-suite"
     ms.devlang="na"
     ms.topic="hero-article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="08/16/2016"
     ms.author="dobett"/>

# <a name="tutorial-get-started-with-the-preconfigured-solutions"></a>Opetusohjelma: Käytön aloittaminen esimääritetyt ratkaisut

## <a name="introduction"></a>Johdanto

Azure IoT Suite [esimääritetyt ratkaisut] [ lnk-preconfigured-solutions] yhdistää lopusta loppuun-ratkaisuja, jotka toteuttavat yleisiä IoT business tilanteita, joissa Azure IoT palveluihin. *Remote seuranta* valmiiksi-ratkaisun muodostaa yhteyden ja valvoo laitteet. Voit käyttää ratkaisun virta laitteilla tietojen analysointiin ja parantaa liiketoiminnan tulosten tekemällä vastaaminen automaattisesti kyseisen stream tietojen prosessit.

Tässä opetusohjelmassa näytetään, miten valmistelu remote seurantaa esimääritettyjä ratkaisu. Se myös esitellään remote seuranta-ratkaisun ominaisuuksiin. Voit käyttää Monet näistä toiminnoista, ottaa käyttöön sekä esimääritettyjä ratkaisu ratkaisu Raporttinäkymät-ikkunan avulla:

![Ratkaisu Raporttinäkymät-ikkunan valmiiksi Remote seuranta][img-dashboard]

Jotta voit suorittaa tässä opetusohjelmassa, sinun on aktiivinen Azure-tilaus.

> [AZURE.NOTE]  Jos sinulla ei ole tiliä, voit luoda ilmainen kokeiluversio tili vain muutaman minuutin. Lisätietoja on artikkelissa [Azure maksuttoman kokeiluversion][lnk_free_trial].

[AZURE.INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

## <a name="view-the-solution-dashboard"></a>Ratkaisu Raporttinäkymät-ikkunan tarkasteleminen

Ratkaisu Raporttinäkymät-ikkunan avulla voit hallita käyttöön ratkaisu. Voit esimerkiksi tarkastella telemetriatietojen, lisää laitteita ja Määritä säännöt.

1.  Kun esimääritettyjä ratkaisu ruutu näkyy **valmiina**valmistelun on valmis, valitse remote seurantaa ratkaisu-portaalin avaaminen uudessa välilehdessä **Käynnistä** .

    ![Käynnistä esimääritettyjä ratkaisu][img-launch-solution]

2.  Ratkaisu-portaalin näyttää oletusarvoisesti *ratkaisu Raporttinäkymät-ikkunan*. Voit valita muissa näkymissä vasemmanpuoleisesta valikosta.

    ![Ratkaisu Raporttinäkymät-ikkunan valmiiksi Remote seuranta][img-dashboard]

Koontinäytön näkyvät seuraavat tiedot:

- Kartan näyttää kunkin laitteen ratkaisuun sijainnin. Kun suoritat ratkaisun, on neljä Simuloitu laitetta. Simuloitu laitteet on toteutettu Azure WebJobs ja ratkaisu käyttää Bing Maps-Ohjelmointirajapinnan esitystapa tiedot kartassa.
- **Telemetriatietojen historia** -paneelin piirretään kosteuden ja lämpötilan telemetriatietojen valitusta laitteesta lähes reaaliajassa ja näyttää koostetietoja, kuten suurin-, vähimmäis- ja keskiarvo kosteus.
- **Hälytys historia** -paneelin näkyvät viimeisimmät hälytys tapahtumat, kun telemetriatietojen arvo on ylitetty raja-arvon. Voit määrittää oman hälytykset lisäksi esimääritettyjä ratkaisun luoma esimerkkejä.

## <a name="view-the-device-list"></a>Laitteen luettelon tarkasteleminen

Laitteen luettelossa näkyvät kaikki rekisteröity laitteet ratkaisun. Voit tarkastella laitteen metatietojen muokkaaminen, Lisää tai poista laitteet ja lähettää komentoja laitteet.

1.  Valitse vasemmanpuoleisessa valikossa näyttämään tämä ratkaisu *laiteluettelo* **laitteet** .

    ![Laiteluettelo raporttinäkymät-ikkunassa][img-devicelist]

2.  Laite luettelosta näkyy, että neljä luotu valmistelu prosessin Simuloitu laitetta.

3.  Valitse laite, voit tarkastella laitteen laite-luettelossa.

    ![Laitteen tiedot raporttinäkymät-ikkunassa][img-devicedetails]

**Laitteen tiedot** -ruutu sisältää kolme osaa:

- **Toiminnot** -osassa luetellaan toiminnot, voit suorittaa laitteeseen. Jos poistat laitteen, ei enää ole sallittu telemetriatietojen lähettämiseen tai vastaanottamiseen komentoja. Jos poistat laitteen, voit sitten ottaa sen uudelleen. Voit lisätä laitteella, joka tuottaa hälytyksen, kun telemetriatietojen arvon kynnysarvo liittyvä sääntö. Voit myös lähettää komennon laitteeseen. Kun laite on ensin muodostaa, se kertoo ratkaisun vastata komentoja.
- **Laitteen ominaisuudet** -osassa on luettelo laitteen metatiedot. Jotkin metatiedot joltakin itse laitteen (kuten valmistajan) ja jotkin luodaan ratkaisu (kuten luotu aika). Voit muokata tästä laitteen-metatiedot.
- **Todennus näppäimet** -osassa on luettelo laitteen avulla voit todentaa ratkaisuun näppäimet.

## <a name="send-a-command-to-a-device"></a>Lähettää komennon laitteeseen

Laitteen tiedot-ruutu näyttää kaikki komennot, jotka tietyn laitteen tukee ja ottaa käyttöön, voit lähettää komennot laitteeseen. Kun laite käynnistyy ensimmäisen kerran, se lähettää tietoja komennoista, se tukee ratkaisu.

1.  Valitse **Komennot** valitun laitteen laitteen tiedot-ruudussa.

    ![Raporttinäkymät-ikkunan laite-komennot][img-devicecommands]

2.  Valitse **PingDevice** komento-luettelosta.

3.  Valitse **Lähetä-komennon**.

4.  Näet tilan komennon historia-komennosta.

    ![Raporttinäkymät-ikkunan komento-tila][img-pingcommand]

Ratkaisu seuraa jokaisen komennon se lähettää tila. Tulos on alun perin **odotetaan**. Kun laite ilmoittaa, että se suorittaa komento, tulos on määritetty **onnistui**.

## <a name="add-a-new-simulated-device"></a>Lisää uusi Simuloitu laite

Kun otat käyttöön esimääritettyjä ratkaisun, valmistella automaattisesti neljä otoksen laitetta, näet laite luettelosta. Seuraaviin laitteisiin ovat käynnissä Azure-WebJob *Simuloitu laitteet* . Simuloitu laitteissa voit kokeilla tarpeellisuus reaali fyysinen laitteita ilman esimääritettyjä ratkaisu on helppoa. Jos haluat muodostaa reaali laitteen ratkaisuun, katso [Yhdistä laitteesi kaukosäätimen seuranta esimääritettyjä ratkaisu] [ lnk-connect-rm] opetusohjelma.

Seuraavat vaiheet Näytä Simuloitu laitteen lisääminen ratkaisua:

1.  Siirry takaisin laite luettelosta.

2.  Valitse **+ Lisää laitetta** laitteen lisääminen vasemmassa alakulmassa.

    ![Lisää laitteen esimääritettyjä ratkaisu][img-adddevice]

3.  Valitse **Lisää uusi** **Simuloitu laite** -ruutu.

    ![Määritä uusi laitteen tiedot raporttinäkymät-ikkunassa][img-addnew]
    
    Fyysinen laite lisäksi luot uuden Simuloitu laitteen, voit lisätä myös jos haluat luoda **Mukautetun laitteen**. Lisätietoja yhteyden fyysisten laitteiden ratkaisuun, artikkelissa [etäyhteyden remote seuranta esimääritettyjä ratkaisu IoT ohjelmistopakettiin laitteen][lnk-connect-rm].

4.  Valitse **Haluan määrittää omaa laitteen-tunnus**ja kirjoita laitteelle tunnusnimi, kuten **mydevice_01**.

5.  Valitse **Luo**.

    ![Tallenna uusi laite][img-definedevice]

6. **Lisää Simuloitu laite**vaiheessa 3 valitsemalla palaa laite luettelosta **Valmis** .

7. Voit tarkastella **käytössä** laitteen laite luettelosta.

    ![Näytä uusi laite Valitse laite luettelosta][img-runningnew]

8. Voit myös tarkastella Simuloitu telemetriatietojen koontinäytössä uusi laitteesta:

    ![Näytä telemetriatietojen uusi laitteesta][img-runningnew-2]

## <a name="edit-the-device-metadata"></a>Laitteen metatietojen muokkaaminen

Kun laite muodostaa ensin ratkaisun, se lähettää sen metatietoja ratkaisu. Muokattaessa ratkaisu Raporttinäkymät-ikkunan kautta laitteen-metatiedot lähettää uusia metatietoarvojen laitteelle ja tallentaa uudet arvot ratkaisu DocumentDB-tietokantaan. Lisätietoja on artikkelissa [laitteen tunnistetietojen rekisterin ja DocumentDB][lnk-devicemetadata].

1.  Siirry takaisin laite luettelosta.

2.  Valitse uusi laitteesi **Laitteet-luettelosta**ja valitse sitten **Muokkaa** **Laitteen ominaisuuksien**muokkaaminen:

    ![Laitteen metatietojen muokkaaminen][img-editdevice]

3. Vieritä alaspäin ja muutosten tekeminen leveys- ja pituusastetiedot vales. Valitse **Tallenna muutokset laitteen rekisteriin**.

    ![Laitteen metatietojen muokkaaminen][img-editdevice2]

4. Siirry takaisin koontinäyttö-laitteen sijainti on muuttunut kartassa:

    ![Laitteen metatietojen muokkaaminen][img-editdevice3]

## <a name="add-a-rule-for-the-new-device"></a>Lisää uusi laitteen sääntö

Ei ole sääntöjä uuden laitteen juuri lisäämäsi. Tässä osassa voit lisätä säännön, joka tuottaa hälytyksen, kun uusi laite lähetti lämpötila ylittää 47 astetta. Ennen kuin aloitat, Huomaa, että uuteen laitteeseen koontinäytössä telemetriatietojen historiaa näkyy laitteen lämpötila eivät voi ylittää 45 astetta.

1.  Siirry takaisin laite luettelosta.

2.  Valitse uusi laitteesi **Laitteet-luettelosta**ja valitse sitten laitteen säännön **Lisää sääntö** .

3. Luo sääntö, joka käyttää **lämpötilan** tietokentän ja käyttää **AlarmTemp** tulos, kun lämpötila ylittää 47 astetta:

    ![Lisää laitteen sääntö][img-adddevicerule]

4. Valitse **Tallenna ja tarkastele sääntöjä** Tallenna muutokset.

5.  Napsauta **Komennot** uuteen laitteeseen laitteen tiedot-ruutu.

    ![Lisää laitteen sääntö][img-adddevicerule2]

6.  Komento-luettelosta **ChangeSetPointTemp** ja määritä **SetPointTemp** 45. Valitse **Lähetä-komennon**:

    ![Lisää laitteen sääntö][img-adddevicerule3]

7.  Siirry takaisin ratkaisu Raporttinäkymät-ikkunan. Lyhyt aika, kun uusi merkintä **Hälytys historia** -ruutu tulee näkyviin, kun uusi laite lähetti lämpötila ylittää 47 asteen raja-arvon:

    ![Lisää laitteen sääntö][img-adddevicerule4]

8. Voit tarkastella ja muokata kaikkia koontinäytön **säännöt** -sivun sääntöjä:

    ![Luettelon laitteen säännöt][img-rules]

9. Voi tarkastella ja muokata kaikkia toimintoja, jotka voidaan toteuttaa vastauksen säännön **Toiminnot** koontinäytön sivulla:

    ![Laitteen luettelotoiminnot][img-actions]

> [AZURE.NOTE] Se on mahdollista määrittää toiminnot, jotka voit lähettää sähköpostiviestin tai SMS vastauksena säännön tai integroida liiketoiminta-järjestelmän [Logiikan sovelluksen]kautta[lnk-logic-apps]. Lisätietoja on artikkelissa [yhteyden logiikan sovellus että Azure IoT Suite Remote seuranta valmiiksi ratkaisu][lnk-logicapptutorial].

## <a name="other-features"></a>Muut ominaisuudet

Käyttämällä ratkaisu-portaalissa, voit hakea tiettyjä ominaisuuksia, kuten mallin laitteiden:

![Laitteen etsiminen][img-search]

Voit poistaa käytöstä laitteen ja sen jälkeen, kun se on poistettu käytöstä, voit poistaa sen:

![Poista käytöstä ja poista laitteen][img-disable]

## <a name="behind-the-scenes"></a>Taustalla

Kun otat käyttöön esimääritettyjä ratkaisu, käyttöönottoprosessin Luo useita resursseja valitsit Azure tilaus. Voit tarkastella näiden resurssien Azure- [portaalin][lnk-portal]. Käyttöönottoprosessin Luo **resurssiryhmä** nimellä, voit valita esimääritetyt ratkaisun nimen perusteella:

![Valmiiksi määritetyt ratkaisu Azure-portaalissa][img-portal]

Voit tarkastella kullekin resurssille asetukset valitsemalla sen resursseja resurssiryhmän luettelo.

Voit myös tarkastella esimääritettyjä ratkaisun lähdekoodin. Remote seurantaa esimääritettyjä ratkaisu-lähdekoodi on [azure-iot-remote-seuranta] [ lnk-rmgithub] GitHub säilöön:

- **DeviceAdministration** -kansio on lähdekoodi koontinäytön.
- **Simulator** -kansio on Simuloitu laitteen lähdekoodi.
- **EventProcessor** kansio sisältää taustatietokantaan prosessi, joka käsittelee saapuvia telemetriatietojen lähdekoodi.

Kun olet valmis, voit poistaa valmiiksi määritettyjä ratkaisu Azure tilauksen [azureiotsuite.com] [ lnk-azureiotsuite] sivuston. Tämän sivuston avulla voit helposti poistaa kaikki resurssit, jotka on valmisteltu esimääritettyjä ratkaisun luonnin yhteydessä.

> [AZURE.NOTE] Voit varmistaa, että poistat kaikki liittyvät esimääritettyjä ratkaisu, poista se [azureiotsuite.com] [ lnk-azureiotsuite] sivuston ja ei poistaa resurssiryhmä-portaalissa.

## <a name="next-steps"></a>Seuraavat vaiheet

Nyt kun olet käyttöön valmiiksi toimimasta-ratkaisun, voit jatkaa aloittaminen IoT Suite lukemalla on seuraavissa artikkeleissa:

- [Ratkaisu ongelmatilanteita valmiiksi Remote seuranta][lnk-rm-walkthrough]
- [Laitteen yhdistäminen remote seurantaa esimääritettyjä ratkaisu][lnk-connect-rm]
- [Azureiotsuite.com oikeudet][lnk-permissions]

[img-launch-solution]: media/iot-suite-getstarted-preconfigured-solutions/launch.png
[img-dashboard]: media/iot-suite-getstarted-preconfigured-solutions/dashboard.png
[img-devicelist]: media/iot-suite-getstarted-preconfigured-solutions/devicelist.png
[img-devicedetails]: media/iot-suite-getstarted-preconfigured-solutions/devicedetails.png
[img-devicecommands]: media/iot-suite-getstarted-preconfigured-solutions/devicecommands.png
[img-pingcommand]: media/iot-suite-getstarted-preconfigured-solutions/pingcommand.png
[img-adddevice]: media/iot-suite-getstarted-preconfigured-solutions/adddevice.png
[img-addnew]: media/iot-suite-getstarted-preconfigured-solutions/addnew.png
[img-definedevice]: media/iot-suite-getstarted-preconfigured-solutions/definedevice.png
[img-runningnew]: media/iot-suite-getstarted-preconfigured-solutions/runningnew.png
[img-runningnew-2]: media/iot-suite-getstarted-preconfigured-solutions/runningnew2.png
[img-rules]: media/iot-suite-getstarted-preconfigured-solutions/rules.png
[img-editdevice]: media/iot-suite-getstarted-preconfigured-solutions/editdevice.png
[img-editdevice2]: media/iot-suite-getstarted-preconfigured-solutions/editdevice2.png
[img-editdevice3]: media/iot-suite-getstarted-preconfigured-solutions/editdevice3.png
[img-adddevicerule]: media/iot-suite-getstarted-preconfigured-solutions/addrule.png
[img-adddevicerule2]: media/iot-suite-getstarted-preconfigured-solutions/addrule2.png
[img-adddevicerule3]: media/iot-suite-getstarted-preconfigured-solutions/addrule3.png
[img-adddevicerule4]: media/iot-suite-getstarted-preconfigured-solutions/addrule4.png
[img-actions]: media/iot-suite-getstarted-preconfigured-solutions/actions.png
[img-portal]: media/iot-suite-getstarted-preconfigured-solutions/portal.png
[img-search]: media/iot-suite-getstarted-preconfigured-solutions/solutionportal_07.png
[img-disable]: media/iot-suite-getstarted-preconfigured-solutions/solutionportal_08.png

[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-preconfigured-solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk-azureiotsuite]: https://www.azureiotsuite.com
[lnk-logic-apps]: https://azure.microsoft.com/documentation/services/app-service/logic/
[lnk-portal]: http://portal.azure.com/
[lnk-rmgithub]: https://github.com/Azure/azure-iot-remote-monitoring
[lnk-devicemetadata]: iot-suite-what-are-preconfigured-solutions.md#device-identity-registry-and-documentdb
[lnk-logicapptutorial]: iot-suite-logic-apps-tutorial.md
[lnk-rm-walkthrough]: iot-suite-remote-monitoring-sample-walkthrough.md
[lnk-connect-rm]: iot-suite-connecting-devices.md
[lnk-permissions]: iot-suite-permissions.md
