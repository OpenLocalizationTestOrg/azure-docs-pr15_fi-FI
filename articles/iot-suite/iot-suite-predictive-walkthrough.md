<properties
 pageTitle="Ennakoivan ylläpito ongelmatilanteita | Microsoft Azure"
 description="Azure IoT valmiiksi ennakoivan ylläpito-ratkaisun vaiheista."
 services=""
 suite="iot-suite"
 documentationCenter=""
 authors="aguilaaj"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-suite"
 ms.devlang="na"
 ms.topic="get-started-article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="08/17/2016"
 ms.author="araguila"/>

# <a name="predictive-maintenance-preconfigured-solution-walkthrough"></a>Ennakoivan ylläpito valmiiksi ratkaisu ongelmatilanteita

## <a name="introduction"></a>Johdanto

IoT Suite ennakoivan ylläpito valmiiksi ratkaisu on lopusta loppuun-ratkaisun, joka ennustaa piste, kun virhe on todennäköistä business-skenaarion. Voit käyttää esimääritettyjä ratkaisun itse toimintoja, kuten optimointi ylläpito. Ratkaisu yhdistää avaimen Azure IoT Suite-palveluja, kuten [Azure koneen Learning] [ lnk_machine_learning] työtilassa. Tämä työtila on kokeet, julkisen otoksen tietojoukon, ennustetaan jäljellä olevan hyötyä aika (RUL) ilma-moduulin perusteella. Ratkaisu toteuttaa IoT business-skenaarion täysin lähtökohtana voit suunnitella ja toteuttaa ratkaisun, joka vastaa oman yrityksen.

## <a name="logical-architecture"></a>Looginen arkkitehtuuri

Seuraavassa kaaviossa ympärille esimääritettyjä ratkaisu looginen osat:

![][img-architecture]

Sininen kohteet ovat Azure palvelut, jotka on valmisteltu sijainnissa, voit valita esimääritetyt ratkaisu valmistelu. Voit valmistella Yhdysvaltojen Itä, Pohjois Europe tai Itä-Aasian alueen esimääritettyjä ratkaisu.

Resurssien eivät ole käytettävissä mihin esimääritettyjä ratkaisu valmistelu alueilla. Kaavion oranssi kohteiden esittävät valmisteltu lähinnä käytettävissä alueen (Etelä keskitetyn US Europe Länsi tai Kaakkoisaasialaisten Aasian) annettu valitun alueen Azure-palveluita.

Vihreä kohde on Simuloitu laitteella, joka edustaa ilma-ohjelma. Saat lisätietoja näiden Simuloitu laitteiden seuraavassa osassa tietoja.

Harmaa kohteet vastaavat osat, jotka toteuttavat *laitteen hallinnan* ominaisuuksia. Ennakoivan valmiiksi ylläpito-ratkaisun nykyisessä versiossa ei valmistella nämä resurssit. Lisätietoja laitteen hallinta viitata [remote seuranta esimääritettyjä ratkaisu][lnk-remote-monitoring].

## <a name="simulated-devices"></a>Simuloitu laitteet

Valmiiksi määritetyt ratkaisu Simuloitu laitteen edustaa ilma-ohjelma. Ratkaisu on valmisteltu kaksi varustetut, jotka yhdistetään yhteen ilma. Kunkin ohjelma tietokoneesta kuuluu äänimerkki neljänlaisia telemetriatietojen: tunnistimen 9, tunnistimen 11 tai tunnistimen 14 tunnistimen 15 säätää tietokoneen Learning mallin jäljellä olevan hyötyä aika (RUL) Engine laskemiseen tarvittavat tiedot. Kunkin Simuloitu laitteen lähettää seuraavat telemetriatietojen viestit IoT-toiminnossa:

*Jakson määrä*. Jakson edustaa valmiit lento muuttujan pituinen 2 – 10 tuntia, jossa telemetriatietoja kirjataan lennon puolessa tunnissa.

*Telemetriatietojen*. On neljä anturit, jotka edustavat ohjelma määritteet. Anturit yhdistyvät toisten tietokantajärjestelmien nimeltä tunnistimen 9, tunnistimen 11, tunnistimen 14 ja tunnistimen 15. Nämä 4 anturit edustavat telemetriatietojen riittävästi hyödyllisiä tuloksia hakeminen RUL Machine Learning-malli. Tämän mallin luodaan julkisen tietojoukon, joka sisältää reaali ohjelma tunnistimen tietoja. Lisätietoja siitä, miten malli on luotu alkuperäinen tietojoukon, katso [Cortana Intelligence valikoima ennakoivan ylläpito mallin][lnk-cortana-analytics].

Simuloitu laitteet voivat käsitellä lähetetään IoT-toiminnosta seuraavista komennoista:

| Komento | Kuvaus |
|---------|-------------|
| StartTelemetry | Hallitsee sen tilaa.<br/>Käynnistää telemetriatietojen lähettäminen laitteeseen     |
| StopTelemetry  | Hallitsee sen tilaa.<br/>Lopettaa telemetriatietojen lähettäminen laitteeseen |

IoT toiminnosta on laitteen komento acknowledgment.

## <a name="azure-stream-analytics-job"></a>Azure Stream Analytics-työ

**Työ: Telemetriatietojen** toimii laitteen telemetriatietojen stream saapuvien versio kaksi lauseita käyttäen. Ensimmäinen valitsee kaikki telemetriatietojen laitteet ja lähettää nämä tiedot blob storage-kohtaa, johon se näyttää web App-sovelluksessa. Toinen lauseke laskee keskiarvon tunnistimen arvot yli kaksi minuuttia liukuva ikkunan ja lähettää nämä tiedot – tapahtumaa-toiminnossa on **Tapahtuman käsittelijä**.

## <a name="event-processor"></a>Tapahtuman suoritin

**Tapahtuman käsittelijä** ottaa keskimääräinen tunnistimen arvot valmiit jakson. Se siirtoja arvot API, joka paljastaa koneen Learning koulutus malli moduulin RUL laskemiseen.

## <a name="azure-machine-learning"></a>Azure koneen oppiminen

Lisätietoja siitä, miten malli on luotu alkuperäinen tietojoukon, katso [Cortana liiketoimintatietojen valikoima ennakoivan ylläpito mallin][lnk-cortana-analytics].

## <a name="lets-start-walking"></a>Seuraavissa ohjeissa ominaisuussäilöjen

Tässä osassa esitellään ratkaisun osat, kuvaa tarkoitettu Käyttötapaus ja sisältää esimerkkejä.

### <a name="predictive-maintenance-dashboard"></a>Ennakoivan ylläpito Raporttinäkymät-ikkunan

WWW-sovelluksessa sivu käyttää PowerBI JavaScript-ohjausobjekteja (katso [PowerBI tehosteiden säilöön][lnk-powerbi]) voi esittää:

- Blob-objektien tallennustilaan Stream Analytics-töiden tiedot.
- RUL ja jakson määrä, ilma ohjelma kohden.

### <a name="observing-the-behavior-of-the-cloud-solution"></a>Noudattaen cloud-ratkaisun toimintaa

Siirry Azure-portaalissa ratkaisu-niminen haluat tarkastella valmistellun resursseja resurssiryhmän.

![][img-resource-group]

Kun valmistelu esimääritettyjä ratkaisu, saat tietokoneen Learning työtilan viittaavan linkin sähköpostitse. Voit myös siirtyä koneen Learning työtila- [azureiotsuite.com] [ lnk-azureiotsuite] valmistellun ratkaisu kun se on **valmiina** tila-sivu.

![][img-machine-learning]

Ratkaisu-portaalissa näet otosten on valmisteltu neljä Simuloitu laitteiden edustavan kaksi ilma kaksi varustetut ilma, joissa neljä anturit kohden. Kun siirryt ensin ratkaisu-portaaliin, sen on pysäytetty.

![][img-simulation-stopped]

Valitse **Käynnistä simuloinnissa** simuloinnissa, jossa näet tunnistimen historiatiedot, RUL, jaksot, ja RUL historia täyttäminen koontinäyttö alkavan.

![][img-simulation-running]

Kun RUL on pienempi kuin 160 (valittu esittelyä varten haluamaansa kynnysarvo), ratkaisu-portaalin näyttää varoitussymboli RUL näyttö-kohdan vieressä ja korostaa keltaisella ilma-ohjelma. Ilmoitus siitä, miten RUL-arvoilla on yleinen Yleiset alaspäin trendin, mutta yleensä pomppiva ylös tai alas. Tämä ongelma hakutulosten erilaisten jakson pituudet ja mallin tarkkuutta.

![][img-simulation-warning]

Koko simuloinnissa kestää noin 35 minuuttia 148 jaksot suorittamiseen. Molempien ohjelmien viesti raja-arvo on 8 minuuttia ja 160 RUL raja-arvon täyttyy ensimmäistä kertaa on noin 5 minuuttia.

Sen suoritetaan läpi koko tietojoukko 148 jaksot ja kummallakin lopullinen RUL ja jakson arvojen.

Voit lopettaa simuloinnissa milloin tahansa, mutta valitsemalla **Käynnistä simuloinnissa** replays simuloinnissa alusta DataSet.

## <a name="next-steps"></a>Seuraavat vaiheet

Kun olet suorittanut valmiiksi ennakoivan ylläpito-ratkaisun, haluat ehkä muuttaa sitä, katso [ohjeet mukauttamisesta esimääritetyt ratkaisut][lnk-customize].

[IoT Suite - kohdassa Näytä lisäasetukset - ennakoivan ylläpito](http://social.technet.microsoft.com/wiki/contents/articles/33527.iot-suite-under-the-hood-predictive-maintenance.aspx) TechNet-blogimerkinnässä on valmiiksi ennakoivan ylläpito-ratkaisun muita yksityiskohdat.

Voit myös selata joitakin muita ominaisuuksia ja toimintoja IoT Suite valmiiksi ratkaisuja:

- [Usein kysyttyjä kysymyksiä IoT Suite][lnk-faq]
- [IoT suojaus on alusta alkaen][lnk-security-groundup]


[img-architecture]: media/iot-suite-predictive-walkthrough/architecture.png
[img-resource-group]: media/iot-suite-predictive-walkthrough/resource-group.png
[img-machine-learning]: media/iot-suite-predictive-walkthrough/machine-learning.png
[img-simulation-stopped]: media/iot-suite-predictive-walkthrough/simulation-stopped.png
[img-simulation-running]: media/iot-suite-predictive-walkthrough/simulation-running.png
[img-simulation-warning]: media/iot-suite-predictive-walkthrough/simulation-warning.png

[lnk-powerbi]: https://www.github.com/Microsoft/PowerBI-visuals
[lnk_machine_learning]: https://azure.microsoft.com/services/machine-learning/
[lnk-remote-monitoring]: iot-suite-remote-monitoring-sample-walkthrough.md
[lnk-cortana-analytics]: http://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-Template-3
[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk-customize]: iot-suite-guidance-on-customizing-preconfigured-solutions.md
[lnk-faq]: iot-suite-faq.md
[lnk-security-groundup]: securing-iot-ground-up.md
