<properties 
   pageTitle="StorSimple teknisiä | Microsoft Azure"
   description="Tässä artikkelissa kuvataan tekniset vaatimukset ja lakisääteisiä standardeja yhteensopivuustiedot StorSimple laitteisto-komponentteja."
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
   ms.date="08/17/2016"
   ms.author="alkohli" />

# <a name="technical-specifications-and-compliance-for-the-storsimple-device"></a>Tekniset vaatimukset ja vaatimusten noudattaminen StorSimple-laite

## <a name="overview"></a>Yleiskatsaus

Microsoft Azure StorSimple laitteen laitteita noudattaa tekniset vaatimukset ja tässä artikkelissa kuvattujen lakisääteisiä standardeja. Tekniset vaatimukset kuvataan Power ja jäähdytys moduulit (PCMs), levyasemien, tallennustilaa ja liitteet. Yhteensopivuus-tietoja käsitellään kansainväliset standardit, suojaus ja päästöjen ja kaapeli avuksi.


## <a name="power-and-cooling-module-specifications"></a>Power ja jäähdytys moduulin määritykset  

StorSimple laitteessa on kaksi 100 240V kahden Puhalla, käytössä-yhteensopiva Power jäähdytys moduulit (PCMs). Tämä on tarpeettomia power määritys. Jos PCM epäonnistuu, laite säilyy toimimaan tavallisesti muissa PCM kunnes moduulin korvataan.  

EBOD kehyksen käyttävät 580 W-PCM ja ensisijainen kehyksen 764 W-PCM. Seuraavissa taulukoissa luetellaan PCMs liittyvä teknisiä.

| Määritys           | 580 W PCM (EBOD)                                    | 764 W PCM (perus)                                |
|------------------------ | --------------------------------------------------- | -------------------------------------------------- |
| Suurin tulosteen power    | 580 W                                               | 764                                                |
| Taajuus               | 50/60 Hz                                            | 50/60 Hz                                           |
| Jännite alueen valinta | Automaattinen välien: 90 – 264 V AC, 47/63 Hz               | Automaattinen välien: 90-264 V AC, 47/63 Hz               |
| Suurin inrush nykyisen  | 20 A                                                | 20 A                                               |
| Power kerroin korjaus. | > 95 % nimellisjännitteellä näytettävä                          | > 95 % nimellisjännitteellä näytettävä                         |
| Yliaaltojen               | Täyttää EN61000-3-2                                   | Täyttää EN61000-3-2                                  |
| Tulos                  | 5V valmius jännitteen @ 2.0 A                          | 5V valmius jännitteen @ 2.7 A                         |
|                         | + 5V @ 42 A                                          | + 5V @ 40 A                                         |
|                         | + 12V @ 38 A                                         | + 12V @ 38 A                                        |
| Oikotie vaihdettava           |  Kyllä                                                | Kyllä                                                |
| Valitsimet ja merkkivalot       | AC käyttöön/pois valitsin ja neljä Tilailmaisin merkkivalot     | AC käyttöön/pois valitsin ja kuusi Tilailmaisin merkkivalot     |
| Jäähdytys kehys       | Aksiaalinen jäähdytys tuulettimista muuttujan Tuulettimen nopeus-ohjausobjekti  | Aksiaalinen jäähdytys tuulettimista muuttujan Tuulettimen nopeus-ohjausobjekti |

 
## <a name="power-consumption-statistics"></a>Power kulutus tilastotiedot  

Seuraavassa taulukossa on lueteltu StorSimple laitteen eri mallien tyypillinen power kulutustiedot (todellisia arvoja voi vaihdella julkaistu). 
 
 Ehdot | 240 V AC | 240 V AC | 240 V AC | 110 V AC | 110 V AC | 110 V AC 
 ---------- | -------- | -------- | -------- | -------- | -------- | -------- 
 Tuulettimista hidas, asemat käyttämättömänä | NOUSEVAT 1,45 A  |0.31 kW | 1057.76 BTU/hr | 3.19 A | 0.34 kW | 1160.13 BTU/hr 
 Hidas tuulettimista asemat käyttäminen | 1.54 A | Järjestelmä luo 0,33 kW | 1126.01 BTU/hr | 3.27 A | 0.36 kW | 1228.37 BTU/hr 
 Tuulettimista nopeasti asemat käyttämättömänä kaksi PSUs muistiin | 2.14 A | 0.49 kW  | 1671.95 BTU/hr | 4,99 A | 0.54 kW | 1842.56 BTU/hr 
 Nopeasti tuulettimista asemat käyttämättömänä, yksi PSU muistiin jokin käyttämättömänä | 2.05 A | 0.48 kW | 1637.83 BTU/hr | 4.58 A | 0,50 kW | 1706.07 BTU/hr 
 Tuulettimista nopeasti asemat käyttäisit, kaksi PSUs muistiin | 2.26 KOHDASSA A | 0,51 kW | 1740.19 BTU/hr | 4.95 A | 0.54 kW | 1842.56 BTU/hr 
 Nopea tuulettimista asemat käyttäisit, yksi PSU muistiin jokin käyttämättömänä | 2.14 A |0.49 kW | 1671.95 BTU/hr | 4.81 A  | 0.53 kW | 1808.44 BTU/hr 

## <a name="disk-drive-specifications"></a>Levyasema määritykset  

StorSimple laitteen tukee jopa 12 3,5 tuuman lomakkeen kerroin Serial liitetty SCSI (SAS)-levyasemien. Todellinen asemat voivat olla sekoitus SSD asemat (SSDs) tai kiintolevyn levyasemien (HDDs)-tuotteen asetuksista riippuen. 12 levyaseman paikkojen sijaitsevat 3 x 4-määritys kehyksen eteen. EBOD kehyksen avulla toisen 12 levyasemien lisätallennustilaa. Nämä ovat aina HDDs.  

## <a name="storage-specifications"></a>Tallennustilan määritykset
StorSimple laitteet on kiintolevyaseman levyasemien sekä SSD asemat 8100 ja 8600. Käytettävä kokonaiskapasiteetti 8100 ja 8600 ovat noin 15 TT ja 38 Teratavua. Seuraavassa taulukossa asiakirjat tietoja Suoritettaessa Kiintolevylle ja cloud kapasiteetin StorSimple ratkaisu kapasiteetin kontekstissa.

| Laitteen mallista / kapasiteetti                         | 8100                                                    | 8600                                                    |
|------------------------------------------------|---------------------------------------------------------|---------------------------------------------------------|
| Kiintolevyn määrä ohjaa (HDDs)              |   8                                                     |  19                                                     |
| Määrä SSD ohjaa (SSDs)            |   4                                                     |  5                                                      |
| Yksittäinen Kiintolevy kapasiteetti                            |   4 TT                                                  |  4 TT                                                   |
| Yksittäisen Suoritettaessa kapasiteetti                            |   400 GT                                                |  800 GT                                                 |
| Käyttämätöntä kapasiteettia                                 |   4 TT                                                  | 4 TT                                                    |
| Käytettävä Kiintolevy kapasiteetti                            | 14 TT                                                   | 36 TT                                                   |
| Käytettävä Suoritettaessa kapasiteetti                            | 800 GT                                                  | 2 TERATAVUA                                                    |
| Yhteensä käytettävissä olevaa kapasiteettia *                         | NOIN 15 TT                                                 | NOIN 38 TT                                                 |
| Suurin ratkaisu kapasiteetin (mukaan lukien cloud)    | 200 TT                                                  | 500 TT                                                  |


<sup> * </sup> -  *Käytettävä kokonaiskapasiteetti sisältää tiedot ja metatiedot buffers.* käytettävissä kapasiteetti

## <a name="enclosure-dimensions-and-weight-specifications"></a>Kehyksen dimensiot- ja leveys-määritykset  

Seuraavissa taulukoissa luetellaan eri kehyksen määritykset, dimensiot-ja leveys.  

### <a name="enclosure-dimensions"></a>Kehyksen mitat
Seuraavassa taulukossa on lueteltu millimetreiksi ja tuumaa kehyksen mitat.

|Kehys |Millimetreiksi |Tuumaa |
|----------|------------|-------| 
| Korkeus |87.9 | 3.46 |
| Yli yläkiinnityslaipassa leveys | 483 | 19.02 |
| Koko leipätekstin kehyksen leveys | 443 | 17.44 |
| Syvyys-eteen yläkiinnityslaipassa kehyksen leipätekstin pään | 577 | 22.72 |
| Syvyys-toimintojen Ohjauspaneelin kauimpana pään kehyksen avulla | 630.5 | 24.82 |
| Syvyys-yläkiinnityslaipassa kauimpana pään kehyksen avulla |   603 | 23.74 |

### <a name="enclosure-weight"></a>Kehyksen leveys  

Määritysten mukaan täydessä ensisijainen kehyksen voit arvioida-21 33 kgs ja edellyttää, että kaksoisnapsautat kaksi henkilöä. 
 
| Kehys | Weight (paino) |
|-----------|--------| 
| Suurin sallittu paino (vaihtelee määritykset) |30 kg – 33 kg |
| Tyhjä (ei ole asennettu asemat) |21 – 23 kg |

## <a name="enclosure-environment-specifications"></a>Kehyksen ympäristön määritykset  

Tässä osassa on luettelo kehys-ympäristöön liittyvät määritykset. Lämpötila, kosteus, korkeus, sähköiskuja, tärinä, suunta, suojaus ja sähkömagneettinen yhteensopivuus (EMC) sisältyvät tähän luokkaan.  

### <a name="temperature-and-humidity"></a>Lämpötilan ja kosteuden

| Kehys        | Lämpötilan alue  | Ympäristön suhteellinen kosteus | Suurin wet valo   |
|------------------|----------------------------|---------------------------|--------------------|
| Toiminnassa      | 5-35° C (41° F - 95° F)    | 20 % - 80 % ei-kondenssikattilat- | 28 OC (82° F)        |
| Muut toiminnassa  | – 40-70° C (40 ASTETTA F - 158° F) | 5 – 100 % ei-kondenssikattilat  | 29 OC (84° F)        |

### <a name="airflow-altitude-shock-vibration-orientation-safety-and-emc"></a>Ilmavirralle, korkeus, sähköiskuja, tärinä, suunta, suojaus ja EMC
 
| Kehys          | Toiminnallisia määritykset                                                |
|--------------------|---------------------------------------------------------------------------| 
| Ilmavirralle            | Järjestelmän ilmavirralle on edessä ja takana. Järjestelmä on käytettävä low-pressure, taka-pakokaasun asennuksen kanssa. Teline ovien ja esteistä luomia vastapaine korkeintaan 5 pascaleina (0,5 mm veden mittari). |
| Korkeus-toiminnassa  | -30 metriä, 3045 metriä (10 000 jalat alta-100) enintään 5 oC edellä jalat 7000 varaustiedoista luokitellut lämpötila kanssa. |
| Korkeus toiminnassa  | -305 metriä, 12,192 metriä (40 000 jalat alta kirjanpitoarvoksi-1 000) |
| Sähköiskuja toiminnassa  | 5g 10 ms ½ sini |
| Sähköiskuja, ei ole toiminnassa  | 30g 10 ms ½ sini |
| Tärinä toiminnassa  | 0.21g RMS 5 500 Hz satunnaisia |
| Tärinä toiminnassa  | 1.04g RMS 2 200 Hz satunnaisia |
| Tärinä siirtäminen  | 3g 2 200 Hz sini |
| Suunta ja käyttöönotto  | 19. "Teline asennustapa (2 EIA yksiköt) |
| Teline kiskot  | Sovita vähintään 700 mm (31.50 tuumaa) syvyys suksitelineiden standardien kanssa IEC 297 |
| Turvallisuus- ja hyväksynnät.  |   CE ja UL EN 61000-3, IEC 61000-3-UL 61000-3 |
| EMC  | EN55022 (CISPR - A), FCC A |

## <a name="international-standards-compliance"></a>Kansainvälisten vaatimusten noudattaminen
Microsoft Azure StorSimple laitteen noudattaa kansainväliset seuraavat vaatimukset:  

- CE - EN 60950-1  
- CB raportin IEC 60950-1  
- UL ja cUL UL 60950-1  

## <a name="safety-compliance"></a>Turvallisuus yhteensopivuus  

Microsoft Azure StorSimple laitteen täyttää seuraavat turvallisuuden luokitusten:  

- Tuotteen tyyppihyväksynnän: UL cUL CE  
- Turvallisuus yhteensopivuus: UL 60950, IEC 60950-60950 fi  

## <a name="emc-compliance"></a>EMC yhteensopivuus 

Microsoft Azure StorSimple laitteen täyttää seuraavat EMC luokitukset.  

### <a name="emissions"></a>Päästöt

Laite on EMC yhteensopiva suoritettu ja säteilyn tasoissa.  

- Suoritettu päästöjen rajoittaa tasot: Kongressin 47 osa 15B luokan A EN55022 luokan A CISPR luokan A  
- Säteilyn päästöjen rajoittaa tasot: Kongressin 47 osa 15B luokan A EN55022 luokan A CISPR luokan A   

### <a name="harmonics-and-flicker"></a>Yliaaltojen ja flickeristä  

Laitteen mukainen EN61000-3-2/3.  

### <a name="immunity-limit-levels"></a>Rajoita tehotasot  
Laitteen noudattaa EN55024.  

## <a name="ac-power-cord-compliance"></a>AC power johto yhteensopivuus
  
Laajennuksen ja valmis power johto kokoonpano on täytettävä maa, johon laitteen käytetään varten standardit ja niiden on oltava turvallisuutta hyväksynnät, joka on hyväksyttävä maassa. Seuraavissa taulukoissa luetellaan Europe ja USA: n mukaisia.  

### <a name="ac-power-cords---usa-must-be-nrtl-listed"></a>AC power langat - USA: ssa (NRTL luettelossa on oltava)

| Osa       | Määritys                                                     |
| --------------- | ----------------------------------------------------------------- | 
| Virtajohto tyyppi       | AV- tai SVT 18 AWG pienin, 3 johdin 2.0 metriä enimmäispituus |
| Laajennuksen            | NEMA 5-15P lentokonetyypille tyyppi liitteen laajennuksen luokitellut 120 V 10 A; tai IEC 320 C14, 250 V, 10 A |
| Socket          | IEC 320 C-13 250 V 10 A                                         |

### <a name="ac-power-cords---europe"></a>AC power langat - Europe

| Osa       | Määritys                                                     |
| --------------- | ----------------------------------------------------------------- | 
| Virtajohto tyyppi       | Yhdenmukaistettu, H05 VVF-3G1.0                                         |
| Socket          | IEC 320 C-13 250 V 10 A                                         |

## <a name="supported-network-cables"></a>Tuetut verkkokaapelit  

10 GbE verkkoliittymät, tietojen 2 ja 3 tietoja, katso [luettelo tuetuista verkkokaapelit ja moduulit](storsimple-supported-hardware-for-10-gbe-network-interfaces.md).

## <a name="next-steps"></a>Seuraavat vaiheet

Olet nyt valmis ottamaan yhteyttä palvelinkeskuksen StorSimple laitteeseen. Lisätietoja on artikkelissa [paikallisen laitteen käyttöönotto](storsimple-deployment-walkthrough-u2.md).  
