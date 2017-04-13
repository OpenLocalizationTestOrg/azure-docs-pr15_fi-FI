<properties 
    pageTitle="Seurannan ilmaisimet StorSimple | Microsoft Azure" 
    description="Tässä artikkelissa kuvataan light – siirto diodeja (merkkivalot) ja äänimerkin hälytykset voit valvoa StorSimple laitteen tila."
    services="storsimple"
    documentationCenter="NA"
    authors="alkohli"
    manager="carmonm"
    editor="" />
 <tags 
    ms.service="storsimple"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"
    ms.workload="TBD"
    ms.date="08/18/2016"
    ms.author="alkohli" />

# <a name="use-storsimple-monitoring-indicators-to-manage-your-device"></a>Seuranta ilmaisimet StorSimple avulla voit hallita laitetta   

## <a name="overview"></a>Yleiskatsaus

StorSimple laitteen sisältää light – siirto diodeja (merkkivalot) ja hälytykset, että voit valvoa moduulit ja StorSimple laitteen yleistä tilaa. Seurannan ilmaisimet löytyy laitteen ensisijainen kehys ja EBOD kehyksen laitteita. Seurannan ilmaisimet voi olla merkkivalot tai äänimerkin hälytykset.

On kolme LED-tilaa käytetään sen osoittamiseen moduulin tila: vihreä, punainen-keltainen tai punainen-keltainen, vihreä vilkkuvat.  

- Vihreä merkkivalot edustavat kunnossa toiminta-tilaan.  
- Vilkkuvat vihreä ja punainen-keltainen merkkivalot edustavat vähäinen tilanteita, joissa voi edellyttää käyttäjältä esiintymistä.  
- Punainen-keltainen merkkivalot osoittavat, että esitä-moduulin kriittisen virheen.  

Jäljempänä tässä artikkelissa kuvataan eri seurantaa ilmaisin-merkkivalot, niiden sijainnit StorSimple laitteeseen, laitteen tila LED tilat ja kaikki liittyvät äänimerkin hälytykset perusteella.

## <a name="front-panel-indicator-leds"></a>Paneeli ilmaisin merkkivalot

Paneeli, eli *toimintojen Ohjauspaneelin* tai *ops Ohjauspaneelin*näyttää kaikki moduulit kooste tilan järjestelmä. Paneeli on sama kuin ensisijaisen StorSimple ja EBOD kehys ja on esitelty alla.  

   ![Laitteen paneeli][1]
 
Paneeli sisältää seuraavien ilmaisimien osalta:  

1. Vaimenna-painiketta
2. Power-ilmaisin LED (vihreä ja punainen-keltainen)
3. Moduulin virheen ilmaisin JOHTANUT (Valitse punainen-keltainen/OFF)
4. (Valitse punainen-keltainen/OFF JOHTANUT loogisen virheen ilmaisin
5. Yksikön tunnus-näyttö  

Suurin ero paneeli merkkivalot laitteen ja mainittujen EBOD kehyksen on LED-näytössä näkyvän **Järjestelmän yksikön tunnusluvun** . Oletusarvon yksikön tunnus näkyy laitteessa on **00**EBOD kehyksen näytössä oletusarvon yksikön tunnus on **01**. Voit nopeasti erottaa toisistaan laitteen ja EBOD kehys, kun laite on otettu käyttöön. Jos laite on poistettu käytöstä, käytä [ottaminen käyttöön uuden laitteen](storsimple-turn-device-on-or-off.md#turn-on-a-new-device) tiedot laitteen erillään EBOD kehys.  

## <a name="front-panel-led-status"></a>Paneeli LED tila  

Seuraavan taulukon avulla voit tunnistaa merkkivalot-paneeli laitteen tai EBOD kehys merkkinä tila.  

|Järjestelmän virta | Moduulin vika | Loogisen vika | Hälytys | Tila|
|-------------|---------------|-----------------|-------|-------|
|Punainen-keltainen | POISTAMINEN KÄYTÖSTÄ     | POISTAMINEN KÄYTÖSTÄ | PUUTTUU | AC power hävitty, käsittelee varmuuskopion power tai AC power ja ohjauskoneen moduulit on poistettu.|
|Vihreä | KÄYTÖSSÄ | KÄYTÖSSÄ | PUUTTUU | OPS Ohjauspaneelin virta (5s) Testaa tila|
|Vihreä | POISTAMINEN KÄYTÖSTÄ | POISTAMINEN KÄYTÖSTÄ | PUUTTUU | Kaikki funktiot hyvä virta|
|Vihreä | KÄYTÖSSÄ |PUUTTUU | PCM vika merkkivalot, tuuletin vika merkkivalot | Minkä tahansa PCM vika, vika, tai yksityisessä lämpötilan tuuletin|
| Vihreä | KÄYTÖSSÄ | PUUTTUU | I/o-moduulin merkkivalot  | Mikä tahansa ohjauskoneen moduuli-virhe|
| Vihreä | KÄYTÖSSÄ | PUUTTUU | PUUTTUU | Kehyksen logiikan virhe|
| Vihreä | Flash | PUUTTUU | Moduulin tila LED ohjauskoneen moduuli. PCM vika merkkivalot, tuuletin vika merkkivalot | Tuntematon ohjauskoneen moduulityyppi on asennettu, I2C bus virhe-moduulin tärkeä tuotteen tiedot (VPD) määritysten virhe |

## <a name="power-cooling-module-pcm-indicator-leds"></a>Power jäähdytys moduuli (PCM) ilmaisin merkkivalot   

Power jäähdyttimen moduuli (PCM) ilmaisin merkkivalot löytyy ensisijainen kehyksen tai EBOD kehyksen takapuolella PCM kunkin moduulin käyttöön. Tässä ohjeaiheessa kerrotaan, miten seuraavat merkkivalot avulla voit valvoa StorSimple laitteen tila.  

- PCM merkkivalot ensisijainen kehyksen varten
- PCM merkkivalot EBOD kehyksen varten

## <a name="pcm-leds-for-the-primary-enclosure"></a>PCM merkkivalot ensisijainen kehyksen varten  

StorSimple laitteessa on 764W PCM moduuli muita varauksen kanssa. Seuraavassa kuvassa näkyy laitteen LED-paneeli.  

   ![Valitse ensisijainen kehyksen merkkivalot PCM][2]

LED selite:

1. AC power virhe
2. Tuulettimen virhe
3. Varauksen virhe
4. PCM OK
5. Toimialueen Ohjauskoneen virhe
6. Hyvä varauksen  

Tila PCM merkitään LED-paneeli. Laitteen PCM LED-ruutu on kuusi merkkivalot. Neljä nämä merkkivalot Näytä power toimitus- ja tuulettimen tila. Jäljellä olevat kaksi merkkivalot osoittavat PCM varmuuskopion varauksen-moduulissa tila. Seuraavissa taulukoissa avulla voit selvittää PCM tilan.  

### <a name="pcm-indicator-leds-for-power-supply-and-fan"></a>PCM ilmaisin merkkivalot power toimitus- ja tuuletin
| Tila | PCM OK (vihreä) | AC fail (ruskeankeltaista) | Tuulettimen fail (ruskeankeltaista) | Toimialueen Ohjauskoneen fail (ruskeankeltaista) |
|--------|----------------|-----------------------|------------------|----------------------|
| Ei ole AC esitysmuotoa (kehyksen) | POISTAMINEN KÄYTÖSTÄ | POISTAMINEN KÄYTÖSTÄ | POISTAMINEN KÄYTÖSTÄ | POISTAMINEN KÄYTÖSTÄ|
| Ei ole AC power (vain tämä PCM) | POISTAMINEN KÄYTÖSTÄ | KÄYTÖSSÄ | POISTAMINEN KÄYTÖSTÄ | KÄYTÖSSÄ |
| AC esittää PCM päällä - OK     | KÄYTÖSSÄ | POISTAMINEN KÄYTÖSTÄ | POISTAMINEN KÄYTÖSTÄ | POISTAMINEN KÄYTÖSTÄ |
| PCM fail (tuuletin virheiden) | POISTAMINEN KÄYTÖSTÄ | POISTAMINEN KÄYTÖSTÄ | KÄYTÖSSÄ | PUUTTUU |
| PCM vika (päälle amp päälle jännite päälle nykyinen) | POISTAMINEN KÄYTÖSTÄ | KÄYTÖSSÄ | KÄYTÖSSÄ | KÄYTÖSSÄ |
| PCM (tuuletin poikkeama ulos) | KÄYTÖSSÄ | POISTAMINEN KÄYTÖSTÄ | POISTAMINEN KÄYTÖSTÄ | KÄYTÖSSÄ |
| Valmiustilassa | Vilkkuvat | POISTAMINEN KÄYTÖSTÄ | POISTAMINEN KÄYTÖSTÄ | POISTAMINEN KÄYTÖSTÄ |
| PCM laitteisto-ohjelmiston lataaminen | POISTAMINEN KÄYTÖSTÄ | Vilkkuvat | Vilkkuvat | Vilkkuvat |

### <a name="pcm-indicator-leds-for-the-backup-battery"></a>Varmuuskopion varauksen ilmaisin PCM merkkivalot  

| Tila | Varauksen hyvä (vihreä) | Varauksen vika (keltainen) |
|--------|----------------------|-----------------------|
| Varauksen ei käytössä | POISTAMINEN KÄYTÖSTÄ | POISTAMINEN KÄYTÖSTÄ |
| Esitä ja veloitettujen varauksen | KÄYTÖSSÄ | POISTAMINEN KÄYTÖSTÄ |
| Varauksen charging- tai vastuuvapauden | Vilkkuvat | POISTAMINEN KÄYTÖSTÄ |
| Varauksen "pehmeä" vika (palautettavissa) | POISTAMINEN KÄYTÖSTÄ | Vilkkuvat |
| Varauksen "" vika (käsittelylle) | POISTAMINEN KÄYTÖSTÄ | KÄYTÖSSÄ |
| Varauksen disarmed | Vilkkuvat | POISTAMINEN KÄYTÖSTÄ |

## <a name="pcm-leds-for-the-ebod-enclosure"></a>PCM merkkivalot EBOD kehyksen varten  

EBOD kehys on 580W PCM ja ei ole muita varauksen. EBOD kehyksen PCM-ruudun on ilmaisin merkkivalot power tarvikkeiden ja tuulettimen. Seuraavassa kuvassa näkyy nämä merkkivalot.

   ![Valitse EBOD kehyksen merkkivalot PCM][3] 
 
Seuraavan taulukon avulla voit selvittää PCM tilan.  

| Tila | PCM OK (vihreä) | AC fail (ruskeankeltaista) | Tuulettimen fail (ruskeankeltaista) | Toimialueen Ohjauskoneen fail (ruskeankeltaista) |
|--------|---------------|------------------------|------------------|----------------------|
| Ei ole AC esitysmuotoa (kehyksen) | POISTAMINEN KÄYTÖSTÄ | POISTAMINEN KÄYTÖSTÄ | POISTAMINEN KÄYTÖSTÄ | POISTAMINEN KÄYTÖSTÄ |
| Ei ole AC power (vain tämä PCM) | POISTAMINEN KÄYTÖSTÄ | KÄYTÖSSÄ | POISTAMINEN KÄYTÖSTÄ | KÄYTÖSSÄ |
| AC esittää PCM edelleen – OK | KÄYTÖSSÄ | POISTAMINEN KÄYTÖSTÄ | POISTAMINEN KÄYTÖSTÄ | POISTAMINEN KÄYTÖSTÄ |
| PCM fail (tuuletin virheiden) | POISTAMINEN KÄYTÖSTÄ | POISTAMINEN KÄYTÖSTÄ | KÄYTÖSSÄ | X |
| PCM vika (päälle amp jännite päälle nykyisen päälle | POISTAMINEN KÄYTÖSTÄ | KÄYTÖSSÄ | KÄYTÖSSÄ | KÄYTÖSSÄ |
| PCM (tuuletin poikkeama ulos) | KÄYTÖSSÄ | POISTAMINEN KÄYTÖSTÄ | POISTAMINEN KÄYTÖSTÄ | KÄYTÖSSÄ |
| Valmiustilassa malli | Vilkkuvat | POISTAMINEN KÄYTÖSTÄ | POISTAMINEN KÄYTÖSTÄ | POISTAMINEN KÄYTÖSTÄ |
| PCM laitteisto-ohjelmiston lataaminen | POISTAMINEN KÄYTÖSTÄ | Vilkkuvat | Vilkkuvat | Vilkkuvat |

## <a name="controller-module-indicator-leds"></a>Ohjaimen moduulin ilmaisin merkkivalot  

StorSimple laite on merkkivalot ensisijainen ohjain ja EBOD ohjauskoneen moduulit.   

### <a name="monitoring-leds-for-the-primary-controller"></a>Seurannan merkkivalot ensisijainen-ohjain
Seuraavassa kuvassa auttaa tunnistamaan ensisijainen ohjaimen merkkivalot. (Kaikkien sen osien on lueteltu tuki suunta.)  

   ![Seurannan merkkivalot - ensisijainen ohjain][4]
 
Seuraavan taulukon avulla voit selvittää, onko ohjauskoneen moduulin toimii oikein.  

### <a name="controller-indicator-leds"></a>Ohjaimen ilmaisin merkkivalot  

| LED | Kuvaus                                                                            
|---- | ----------- |
| Tunnus-LED (sininen) | Ilmaisee, että moduulin tunnistetaan. Jos sininen LED on vilkkuvaa käynnissä ohjaimen, ohjain on aktiivinen ohjauskoneen ja toinen on valmiustilassa ohjauskoneen. Lisätietoja on kohdassa [Identify aktiivinen ohjauskoneen laitteessasi](storsimple-controller-replacement.md#identify-the-active-controller-on-your-device). |
| Vian LED (keltainen) | Ilmaisee ohjaimen virheen.        
| OK-LED (vihreä) | Tasaisen vihreä tarkoittaa, että ohjain OK. Vihreä vilkkuvat osoittaa ohjauskoneen VPD kokoonpanovirhe. |
| Tehtävän SAS merkkivalot (vihreä) | Tasaisen vihreä ilmaisee yhteyttä ei ole valitun tehtävän. Vihreä vilkkuvat osoittaa yhteys on meneillään. |
| Ethernet tila merkkivalot | Oikealla puolella näkyy linkki tai verkon toiminta: aktiivinen (vilkkuvat vihreä) (tasaisen vihreä) linkin verkon toiminnan. Vasemmalla puolella näkyy verkon nopeuden: (keltainen) 1000 Mt/s ja (ei käytössä) (vihreä) 100 Mt/s 10 Mt/s. Osa-mallista riippuen valo ehkä vilkkuvat, vaikka verkkoliittymän ei ole otettu käyttöön. |
| Kirjaa merkkivalot | Osoittaa käynnistyksen edistymistä, kun ohjain on käytössä. Jos StorSimple laite ei toimi käynnistyy, tämä LED avulla Microsoft Support tunnistaa käynnistyksen yhteydessä, jolla virhe kohtaan. |

>[AZURE.IMPORTANT] 
Jos vika LED palaa on ohjauskoneen moduuli, jolla voi ratkaista käynnistämällä ohjaimen ongelma. Jos ohjaimen uudelleenkäynnistys ei korjaa ongelmaa, ota yhteyttä Microsoft Support.  


### <a name="monitoring-leds-for-the-ebod-ebod-enclosure"></a>Seurannan merkkivalot EBOD (EBOD kehyksen)  

6 gt/s SAS EBOD ohjaimet on merkkivalot, jotka ilmaisevat sen tila, kuten seuraavassa kuvassa.  

  ![Seurannan merkkivalot - EBOD kehys][5]

Seuraavan taulukon avulla voit määrittää, toimiiko EBOD ohjauskoneen moduulin normaalisti.  

### <a name="ebod-controller-module-indicator-leds"></a>EBOD ohjauskoneen moduulin ilmaisin merkkivalot  

|Tila | I/o-moduulin OK (vihreä) | I/o moduuliin Virhepuuanalyysikaavion (keltainen) | Host portin tehtävän (vihreä) |
|-------|----------------------|-------------------------------|----------------------------|
| Moduulin ohjauskoneen OK | KÄYTÖSSÄ | POISTAMINEN KÄYTÖSTÄ | - |
| Ohjaimen moduuliin Virhepuuanalyysikaavion | POISTAMINEN KÄYTÖSTÄ | KÄYTÖSSÄ | - |
| Liity ulkoinen isäntä portti | - | - | POISTAMINEN KÄYTÖSTÄ |
| Ulkoinen isäntä portin yhteyden – aktiviteettia | - | - | KÄYTÖSSÄ |
| Ulkoinen isäntä portin yhteys - tehtävä | - | - | Vilkkuvat |
| Moduulin metatietojen virhe | Vilkkuvat | - | - |

## <a name="disk-drive-indicator-leds-for-the-primary-enclosure-and-ebod-enclosure"></a>Ensisijainen kehys ja EBOD kehyksen ilmaisin levyaseman merkkivalot

StorSimple laitteessa on ensisijainen kehys ja EBOD kehyksen levyasemien. Kunkin levyaseman sisältää seuranta ilmaisin merkkivalot tässä kohdassa kuvatulla tavalla. 

Asema-tila näkyy levyasemien, vihreä LED ja punainen-keltainen-LED liitetty asema carrier moduulit edessä. Seuraavassa kuvassa näkyy nämä merkkivalot.

  ![Levyasema merkkivalot][6]
 
Seuraavan taulukon avulla voit määrittää kunkin levyaseman, joka puolestaan vaikuttaa yleinen paneeli LED tila tilan.  

### <a name="disk-drive-indicator-leds-for-the-ebod-enclosure"></a>EBOD kehyksen ilmaisin levyaseman merkkivalot  

| Tila | Tehtävän OK LED (vihreä) | Vian LED (punainen-keltainen) | Liitetty ops Ohjauspaneelin LED |
|-------|--------------------------|----------------------|-------------------------|
| Asema ei ole asennettu | POISTAMINEN KÄYTÖSTÄ | POISTAMINEN KÄYTÖSTÄ | Ei mitään |
| Asema asennettu ja toiminnassa | Vilkkuvat aktiviteettiin käytössä/poissa käytöstä | X | Ei mitään |
| SCSI kehyksen Services (SES) laitteen tunnistetietojen määrittäminen | KÄYTÖSSÄ | Vilkkuvat sekunnin-/ 1 toinen käytöstä | Ei mitään |
| SES laitteen vika bitti | KÄYTÖSSÄ | KÄYTÖSSÄ | Loogisen vika (punainen) |
| Power ohjausobjektin vikaantuminen | POISTAMINEN KÄYTÖSTÄ | KÄYTÖSSÄ | Moduulin vika (punainen) |

## <a name="audible-alarms"></a>Äänimerkin hälytykset  

StorSimple laite on ensisijainen kehyksen tai EBOD kehyksen liittyvää äänimerkin hälytykset. Hälytys sijaitsee paneeli (tunnetaan myös nimellä ops-paneeli), sekä liitteet. Äänimerkin hälytys osoittaa, kun vika ehto ei sisällä tietoja. Seuraavat ehdot Aktivoi hälytyksen:  

- Tuulettimen vika tai virhe
- Jännite alueen ulkopuolella
- Päälle tai lämpötilan ehdon täyttyessä
- Thermal ylivuoto
- Järjestelmän vika
- Loogisen vika
- Power toimituksen vika
- Potenssiin jäähdytys moduuli (PCM) poistaminen  

Seuraavassa taulukossa on kuvattu eri hälytys tilat.  

### <a name="alarm-states"></a>Hälytys tilat  

| Hälytyksen tila | Toiminto | Toiminto, jossa Vaimenna-painike painettuna |
|------------|---------|---------------------------------|
| S0 | Normaaliin tilaan: hiljainen | Äänimerkki kahdesti |
| S1 | Vian tila: sekunnin-/ 1 toinen käytöstä | Siirtymän S2 tai S3 (katso Huomautuksia) |
| S2 | Muistuta tila: katkonainen äänimerkki | Ei mitään |
| S3 | Mykistetystä tila: hiljainen | Ei mitään |
| S4 | Kriittinen vika tila: jatkuva hälytys | Ei käytettävissä: vaimennus ei ole käytössä |

> [AZURE.NOTE] 

>  - Hälytys tilassa S1, jos et paina Vaimenna kahden minuutin kuluessa tilan automaattisesti siirtymät S2 tai S3.  
>  - S4 jäsenvaltioiden hälytys S1 palaa S0, kun vika ehto ole valittu.  
>  - Kriittinen vika tilan S4 voidaan kirjoittaa muiden tilasta.  

Voit vaimentaa äänimerkin hälytys painamalla vaimennuspainike ops-paneeli. Automaattinen vaimennus ilmenee kahden minuutin kuluttua silloin Vaimenna Vaihda käytetään ei manuaalisesti. Kun hälytyksen vaimennetaan, säilyvät äänen lyhyt katkonainen piippauksia osoittamassa, että ongelma on yhä olemassa kanssa. Hälytyksen on automaattista, kun kaikki ongelmat ole valittuina.  

Seuraavassa taulukossa on kuvattu eri hälytys ehdot.  

### <a name="alarm-conditions"></a>Hälytys ehdot  

| Tila | Vakavuus | Hälytys | OPS-paneeli LED |
|--------|---------|--------|----------------|
| PCM ilmoitus – Ohjauskoneen power yksittäisen PCM menetys | Virhe – ei tappio arvojen | S1 | Moduulin vika|
|PCM ilmoitus – Ohjauskoneen power yksittäisen PCM menetys | Virhe – tappio arvojen | S1 | Moduulin vika |
| PCM tuuletin epäonnistuvat | Virhe – tappio arvojen | S1 | Moduulin vika |
| Käytössä moduuli havaitsi virheen PCM | Virhe | S1 | Moduulin vika |
| Poistaa PCM | Kokoonpanovirhe | Ei mitään | Moduulin vika |
| Kehyksen kokoonpanovirhe | Virhe – kriittinen | S1 | Moduulin vika |
| Pieni varoitus lämpötilan ilmoitus | Varoitus | S1 | Moduulin vika |
| Suuri varoitus lämpötilan ilmoitus | Varoitus | S1 | Moduulin vika |
| Lämpötila hälytys päällä | Virhe – kriittinen | S1 | Moduulin vika |
| I2C bus virhe | Virhe – tappio arvojen | S1 | Moduulin vika |
| OPS-paneeli virhe (I2C) | Virhe – kriittinen     | S1 | Moduulin vika |
| Virhe | Virhe – kriittinen | S1 | Moduulin vika |
| Käytössä käyttöliittymän moduuliin Virhepuuanalyysikaavion | Virhe – kriittinen | S1 | Moduulin vika |
| Käytössä käyttöliittymän moduulin vika – ei ole toiminnassa moduulit jäljellä | Virhe – kriittinen | S4 | Moduulin vika |
| Poistettu käytössä käyttöliittymän moduuli | Varoitus | Ei mitään | Moduulin vika |
| Asema power ohjausobjektin vika | Varoitus – asema power menetetä | S1 | Moduulin vika |
| Asema power ohjausobjektin vika | Virhe – kriittinen; asema power menetys | S1 | Moduulin vika |
| Levyasema poistettu | Varoitus | Ei mitään | Moduulin vika |
| Riittämätön power käytettävissä | Varoitus | ei mitään | Moduulin vika |

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja [StorSimple laitteet ja tila](storsimple-monitor-hardware-status.md).

[1]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE01.png
[2]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE02.png
[3]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE03.png
[4]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE04.png
[5]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE05.png
[6]: ./media/storsimple-monitoring-indicators/storsimple-monitoring-indicators-IMAGE06.png

 
