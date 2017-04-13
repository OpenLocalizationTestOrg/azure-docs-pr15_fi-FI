<properties 
    pageTitle="Ajoneuvon telemetriatietojen analytics ratkaisu playbook: ratkaisun perinpohjaisesti käsittelevään artikkeliin | Microsoft Azure" 
    description="Cortana yritystieto-ominaisuuksien käyttäminen saada reaaliaikaisia ja ennakoivan havainnollistamisen ajoneuvon terveys- ja ajo-käyttöä." 
    services="machine-learning" 
    documentationCenter="" 
    authors="bradsev" 
    manager="jhubbard" 
    editor="cgronlun" />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/12/2016" 
    ms.author="bradsev" />


# <a name="vehicle-telemetry-analytics-solution-playbook-deep-dive-into-the-solution"></a>Ajoneuvon telemetriatietojen analytics ratkaisu playbook: ratkaisun perinpohjaisesti käsittelevään artikkeliin

Tässä **valikossa** linkit tämän playbook osiin: 

[AZURE.INCLUDE [cap-vehicle-telemetry-playbook-selector](../../includes/cap-vehicle-telemetry-playbook-selector.md)]

Tässä osassa työkaluja alaspäin kyselyjä edellisessä ratkaisu-arkkitehtuuri ja ohjeita ja osoittimet mukauttamiseen vaiheisiin. 

## <a name="data-sources"></a>Tietolähteet

Ratkaisu käyttää kahta eri tietolähteistä:

- **Simuloitu ajoneuvon signaalit ja diagnostiikan tietojoukko** ja 
- **ajoneuvon luettelo**

Ajoneuvon telemaattiset-simulator on tämän ratkaisun mukana. Se tietokoneesta kuuluu äänimerkki vianmääritystiedot ja signaalit vastaavat ajoneuvon tilaan ja ajo kaavaa tiettynä ajankohtana. Valitse [Ajoneuvon telemaattiset Simulator](http://go.microsoft.com/fwlink/?LinkId=717075) lataamaan **Ajoneuvon telemaattiset Simulator Visual Studio ratkaisun** mukautusten tarpeen mukaan. Ajoneuvon-luettelo sisältää viittauksen tietojoukon, jonka VIN mallin määritys.

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig2-vehicle-telematics-simulator.png)

*Kuva 2 – ajoneuvon telemaattiset Simulator*

Tämä on JSON-muotoiset tietojoukko, joka sisältää seuraavan rakenteen.

Sarakkeen | Kuvaus | Arvot 
 ------- | ----------- | --------- 
VIN | Ajoneuvon tunnus satunnaisluvun | Tämä on saatu 10 000 satunnaisesti luotu ajoneuvon tunnistenumerot perustyylisivujen luettelo.
Lämpötila ulkopuolelta | Jos ajoneuvon riippuvan sisennyksen ulkopuolelta lämpötila | Satunnaisluvun 0 – 100
Ohjelma lämpötila | Ajoneuvon ohjelma lämpötilan | Satunnaisluvun 0 – 500
Nopeus | Ohjelma nopeutta, jolla ajoneuvon riippuvan sisennyksen | Satunnaisluvun 0 – 100
Polttoaineen | Ajoneuvon polttoaineentaso | Satunnaisluvun 0 – 100 (osoittaa polttoaineen prosenttiarvo)
EngineOil | Ajoneuvon ohjelma oil taso | Satunnaisluvun 0 – 100 (osoittaa ohjelma oil prosenttiarvo)
Tire paine | Ajoneuvon tire paine | Satunnaisesti luotu luku 0 – 50 (osoittaa tire paine prosenttiarvo)
Matkamittarin | Ajoneuvon matkamittarin-lukeminen | Satunnaisluvun 0 – 200000 kohteesta
Accelerator_pedal_position | Ajoneuvon accelerator Polkimien sijainti | Satunnaisluvun 0 – 100 (osoittaa accelerator prosenttiarvo)
Parking_brake_status | Ilmaisee, onko ajoneuvon säilytyksessä tai ei | TOSI tai EPÄTOSI
Headlamp_status | Osoittaa, missä kohtaa täyttää on tai ei | TOSI tai EPÄTOSI
Brake_pedal_status | Ilmaisee, onko jarrun poljin painetaan tai ei | TOSI tai EPÄTOSI
Transmission_gear_position | Ajoneuvon siirto vaihde sijainti | Tilat: ensimmäinen, toinen, kolmas, neljäs viidennen, kuudennen, seitsemännessä, kahdeksannen
Ignition_status | Ilmaisee, onko ajoneuvon käynnissä vai pysäytetty | TOSI tai EPÄTOSI
Windshield_wiper_status | Ilmaisee, onko windshield pyyhin käytössä tai ei | TOSI tai EPÄTOSI
ITSEISARVO | Ilmaisee, onko itseisarvo on kytketty tai ei | TOSI tai EPÄTOSI
Aikaleima | Aikaleiman arvopiste luotaessa | Päivämäärä
Kaupunki | Ajoneuvon sijainti | 4 kaupunkien tätä ratkaisua: Bellevue, Redmond, Sammamish, Seattle


Ajoneuvon mallin viittaus tietojoukko sisältää VIN malli-määritys. 

VIN | Malli |
--------------|------------------
FHL3O1SA4IEHB4WU1 | Sedan |
8J0U8XCPRGW4Z3NQE | Hybrid |
WORG68Z2PLTNZDBI7 | Perheen porrasperä |
JTHMYHQTEPP4WBMRN | Sedan |
W9FTHG27LZN1YWO0Y | Hybrid |
MHTP9N792PHK08WJM | Perheen porrasperä |
EI4QXI2AXVQQING4I | Sedan |
5KKR2VB4WHQH97PF8 | Hybrid |
W9NSZ423XZHAONYXB | Perheen porrasperä |
26WJSGHX4MA5ROHNL | Muunnettavat |
GHLUB6ONKMOSI7E77 | Asema vaunun |
9C2RHVRVLMEJDBXLP | Järjestä Auto |
BRNHVMZOUJ6EOCP32 | Pieni SUV |
VCYVW0WUZNBTM594J | Urheiluauto |
HNVCE6YFZSA5M82NY | Normaali SUV |
4R30FOR7NUOBL05GJ | Asema vaunun |
WYNIIY42VKV6OQS1J | Suuri SUV |
8Y5QKG27QET1RBK7I | Suuri SUV |
DF6OX2WSRA6511BVG | Avoauto |
Z2EOZWZBXAEW3E60T | Sedan |
M4TV6IEALD5QDS3IR | Hybrid |
VHRA1Y2TGTA84F00H | Perheen porrasperä |
R0JAUHT1L1R3BIKI0 | Sedan |
9230C202Z60XX84AU | Hybrid |
T8DNDN5UDCWL7M72H | Perheen porrasperä |
4WPYRUZII5YV7YA42 | Sedan |
D1ZVY26UV2BFGHZNO | Hybrid |
XUF99EW9OIQOMV7Q7 | Perheen porrasperä
8OMCL3LGI7XNCC21U | Muunnettavat |
…….  |   |


### <a name="to-generate-simulated-data"></a>Luo Simuloitu tiedot
1.  Lataa tiedot simulator-paketti, napsauttamalla ajoneuvon telemaattiset Simulator solmun oikeassa yläkulmassa olevaa nuolta. Tallenna ja Pura tiedostot paikallisesti tietokoneeseen. ![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig3-vehicle-telemetry-blueprint.png)*Kuva 3 – ajoneuvon Telemetriatietojen Analytics ratkaisu Sinikopio*

2.  Paikallisessa tietokoneessa Siirry kansioon, johon purit ajoneuvon telemaattiset Simulator-paketti. ![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig4-vehicle-telematics-simulator-folder.png)*Kuva 4 – ajoneuvon telemaattiset Simulator-kansio*

3.  Suorita sovellus **CarEventGenerator.exe**.

### <a name="references"></a>Viittaukset

[Ajoneuvon telemaattiset Simulator Visual Studio-ratkaisuun](http://go.microsoft.com/fwlink/?LinkId=717075) 

[Azure tapahtumaa-toiminnossa](https://azure.microsoft.com/services/event-hubs/)

[Azure Data Factory](https://azure.microsoft.com/documentation/learning-paths/data-factory/)


## <a name="ingestion"></a>Nieltynä
Azure tapahtuman keskittimet Stream Analytics ja Data Factory yhdistelmät ovat hyödyntää ingest ajoneuvon signaalit, diagnostiikan tapahtumien ja reaaliaikaisten ja erä analytics. Kaikki komponentit on luotu ja määritetty osana ratkaisun käyttöönotto. 

### <a name="real-time-analysis"></a>Reaaliaikainen analyysi
Ajoneuvon telemaattiset Simulator luomat tapahtumat julkaistaan käyttämällä tapahtumaa-toiminnossa SDK tapahtumaa-toiminnossa. Virta Analytics-työ ingests näitä tapahtumia tapahtuma-toiminnosta ja käsittelee reaaliaikaisesti ajoneuvon analysoi tietoja. 

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig5-vehicle-telematics-event-hub-dashboard.png) 

*Kuva 5 - tapahtumaa-toiminnossa Raporttinäkymät-ikkunan*

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig6-vehicle-telematics-stream-analytics-job-processing-data.png) 

*Kuva 6 - tietovirta analytics työn tietojen käsittely*

Virta analytics työn;

- ingests tietojen tapahtuma-toiminnosta 
- suorittaa liitoksen yhdistämään ajoneuvon VIN vastaavan mallin viittaus-tiedoilla 
- jatkuu ne yhdeksi monipuolisia erä analytics Azure-blob-säiliö. 

Virta analytics-kyselyn avulla toistuvat tiedot Azure-blob varastoon. 

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig7-vehicle-telematics-stream-analytics-job-query-for-data-ingestion.png) 

*Kuva 7 – Stream analytics työn kyselyn erilaisille tiedot*

### <a name="batch-analysis"></a>Erän analyysi
Olemme luodaan myös muita määrä Simuloitu ajoneuvon signaalit ja tehokkaan erä analytics diagnostiikan tietojoukkoa. Tämä on tarpeen, jotta eräkäsittely hyvä edustavan tietojen voimakkuutta. Tätä varten on käyttävät nimeltä "PrepareSampleDataPipeline" putkijohto Azure Data Factory työnkulun Luo Simuloitu ajoneuvon signaalit ja diagnostiikan tietojoukko yksi vuosi eurolla. Valitse [Data Factory mukautetut aktiviteetit](http://go.microsoft.com/fwlink/?LinkId=717077) , lataa tiedot Factory mukautetun DotNet tehtävän Visual Studio-ratkaisun mukautusten tarpeen mukaan. 

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig8-vehicle-telematics-prepare-sample-data-for-batch-processing.png) 

*Kuva 8 – mallitietojen valmisteleminen erä käsittely työnkululle*

Mukautetun SYÖTTÖ-.net koostuu putkijohto aktiviteetin Näytä seuraavassa:

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig9-vehicle-telematics-prepare-sample-data-pipeline.png) 

*Kuva 9 – PrepareSampleDataPipeline*

Kun putkijohto suoritetaan onnistuneesti ja "RawCarEventsTable" tietojoukko merkitty "Valmis" yksivuotisen nykyarvo Simuloitu ajoneuvon signaalit ja diagnostiikan tiedot on valmistettu. Näet seuraavat kansiot ja tallennustilaa tilisi "connectedcar" säilössä luodun tiedoston:

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig10-vehicle-telematics-prepare-sample-data-pipeline-output.png) 

*Kuva 10 – PrepareSampleDataPipeline tulostus*

### <a name="references"></a>Viittaukset

[Azure tapahtuman keskittimeen SDK erilaisille muodossa](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)

[Azure Data Factory tietojen siirtämistä ominaisuuksia](../data-factory/data-factory-data-movement-activities.md)
[Azure tietojen Factory DotNet tehtävä](../data-factory/data-factory-use-custom-activities.md)

[Azure tietojen Factory DotNet tehtävän visual Studiossa ratkaisu valmistelemiseksi mallitiedot](http://go.microsoft.com/fwlink/?LinkId=717077) 


## <a name="partition-the-dataset"></a>Osion tietojoukko

Raaka puolirakenteisia ajoneuvon signaalit ja diagnostiikan tietojoukko osioitu tietojen valmistelu vaiheessa vuosi, kuukausi-muotoon. Tämä jakaminen korottaa tehokkaampaa kyselyt ja skaalattava pitkään tallennustilan ottamalla vika valittavia yhden blob-tilistä toiseen kuin ensimmäistä kertaa tiliä täyttyy. 

>[AZURE.NOTE] Ratkaisu tämä vaihe koskee vain eräkäsittely.

Syötteen ja tulosteen tietoja tietojen hallinta:

- **Siirtää tietoja** (lukee *PartitionedCarEventsTable*) on säilytettävä pitkän ajanjakson asiakkaan "tietojen järvi" tietojen foundational / "rawest" lomakkeena. 
- Tämä myyntijakso **syöttötiedot** yleensä poistetaan, kun tiedot on luotettavaa tietoa, – se vain tallennetaan (osioitu) paremmin myöhempää käyttöä varten.

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig11-vehicle-telematics-partition-car-events-workflow.png)

*Kuva 11 – osion auton tapahtumien työnkulku*

Perustietoja on osioitu rakenne HDInsight-toimintojen käyttäminen "PartitionCarEventsPipeline". Vuoden vaiheessa 1 luotu mallitiedot on osioitu vuoden ja kuukauden mukaan. Osioiden avulla luodaan ajoneuvon signaalit ja diagnostiikkatiedot vuoden jokaiselle kuukaudelle (yhteensä 12 osiot). 

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig12-vehicle-telematics-partition-car-events-pipeline.png)

*Kuva 12 - PartitionCarEventsPipeline*

Seuraava rakenne-komentosarja nimeltä "partitioncarevents.hql" käytetään jakaminen ja ladattu zip-\demo\src\connectedcar\scripts"-kansiossa. 


    SET hive.exec.dynamic.partition=true;
    SET hive.exec.dynamic.partition.mode = nonstrict;
    set hive.cli.print.header=true;

    DROP TABLE IF EXISTS RawCarEvents; 
    CREATE EXTERNAL TABLE RawCarEvents 
    (
                vin                             string,
                model                           string,
                timestamp                       string,
                outsidetemperature              string,
                enginetemperature               string,
                speed                           string,
                fuel                            string,
                engineoil                       string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position      string,
                parking_brake_status            string,
                headlamp_status                 string,
                brake_pedal_status              string,
                transmission_gear_position      string,
                ignition_status                 string,
                windshield_wiper_status         string,
                abs                             string,
                gendate                         string
                
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:RAWINPUT}'; 

    DROP TABLE IF EXISTS PartitionedCarEvents; 
    CREATE EXTERNAL TABLE PartitionedCarEvents 
    (
                vin                             string,
                model                           string,
                timestamp                       string,
                outsidetemperature              string,
                enginetemperature               string,
                speed                           string,
                fuel                            string,
                engineoil                       string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position      string,
                parking_brake_status            string,
                headlamp_status                 string,
                brake_pedal_status              string,
                transmission_gear_position      string,
                ignition_status                 string,
                windshield_wiper_status         string,
                abs                             string,
                gendate                         string
    ) partitioned by (YearNo int, MonthNo int) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:PARTITIONEDOUTPUT}';

    DROP TABLE IF EXISTS Stage_RawCarEvents; 
    CREATE TABLE IF NOT EXISTS Stage_RawCarEvents 
    (
                vin                             string,
                model                           string,
                timestamp                       string,
                outsidetemperature              string,
                enginetemperature               string,
                speed                           string,
                fuel                            string,
                engineoil                       string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position      string,
                parking_brake_status            string,
                headlamp_status                 string,
                brake_pedal_status              string,
                transmission_gear_position      string,
                ignition_status                 string,
                windshield_wiper_status         string,
                abs                             string,
                gendate                         string,
                YearNo                          int,
                MonthNo                         int) 
    ROW FORMAT delimited fields terminated by ',' LINES TERMINATED BY '10';

    INSERT OVERWRITE TABLE Stage_RawCarEvents
    SELECT
        vin,            
        model,
        timestamp,
        outsidetemperature,
        enginetemperature,
        speed,
        fuel,
        engineoil,
        tirepressure,
        odometer,
        city,
        accelerator_pedal_position,
        parking_brake_status,
        headlamp_status,
        brake_pedal_status,
        transmission_gear_position,
        ignition_status,
        windshield_wiper_status,
        abs,
        gendate,
        Year(gendate),
        Month(gendate)

    FROM RawCarEvents WHERE Year(gendate) = ${hiveconf:Year} AND Month(gendate) = ${hiveconf:Month}; 

    INSERT OVERWRITE TABLE PartitionedCarEvents PARTITION(YearNo, MonthNo) 
    SELECT
        vin,            
        model,
        timestamp,
        outsidetemperature,
        enginetemperature,
        speed,
        fuel,
        engineoil,
        tirepressure,
        odometer,
        city,
        accelerator_pedal_position,
        parking_brake_status,
        headlamp_status,
        brake_pedal_status,
        transmission_gear_position,
        ignition_status,
        windshield_wiper_status,
        abs,
        gendate,
        YearNo,
        MonthNo
    FROM Stage_RawCarEvents WHERE YearNo = ${hiveconf:Year} AND MonthNo = ${hiveconf:Month};

*Kuva 13 – PartitionConnectedCarEvents rakenne-komentosarja*

Kun putkijohto on suoritettu onnistuneesti, näet-tallennustilan tilin "connectedcar" säilössä luotu seuraavat osiot.

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig14-vehicle-telematics-partitioned-output.png)

*Kuva 14 - osioitua tulostus*

Tiedot on nyt optimoitu, entistä helpommin hallittaviin ja valmis lisäämään monipuolisia erä havainnollistamisen lisäkäsittelyä varten. 

## <a name="data-analysis"></a>Tietojen analysointi

Tässä osassa näet, miten voit yhdistää Azure Stream Analytics, Azure koneen Learning, Azure Data Factory ja Azure Hdinsightista RTF advanced tunnin ajoneuvon terveyteen ja ajo-käyttöä varten. Seuraavat kolme jaksoissa on:

1.  **Koneen Learning**: Tämä tästä sisältää tiedot, joista on kehitetty tätä ratkaisua ennustaa edellyttävän ylläpidon ylläpito ja edellyttävän markkinoilta poistettujen turvallisuutta ongelmien vuoksi poikkeavuuksista tunnistus kokeen.
2.  **Reaaliaikainen analyysi**: Tämä tästä sisältää koskevia reaaliaikaisia analytics käyttämällä Stream Analytics-kyselykielen ja operationalizing tietokoneen learning kokeen reaaliajassa mukautetun sovelluksen avulla.
3.  **Erän analyysi**: Tämä tästä on tietoja koskevat muodonmuutoksen ja Azure Hdinsightiin ja Azure koneen Learning operationalized mukaan Azure Data Factory erä tietojen käsittely.

### <a name="machine-learning"></a>Koneen oppiminen

Tavoitteenamme tähän on ennustaa ajoneuvot, jotka edellyttävät ylläpito tai peruuttaminen nummet tiettyjä tilastoja perusteella. Olemme tehdä seuraavat oletukset

- Jos jokin seuraavista kolmesta ehdot ovat tosia, ajoneuvot tarvitaan **ylläpidon ylläpito**:
    - Tire on pieni
    - Ohjelma oil on alhainen
    - Ohjelma lämpötila on suuri

- Jos jokin seuraavista ehdoista täyty, ajoneuvot ehkä **ongelman** ja vaatia **peruuttaminen**:
    - Ohjelma lämpötila on suuri, mutta ulkopuolelta lämpötila on pieni
    - Ohjelma lämpötila on pieni, mutta ulkopuolelta lämpötila on suuri

Edellisen tarpeiden perusteella olemme luonut kaksi erillistä mallien esiintyvien poikkeamia, yksi ajoneuvon ylläpito tunnistamista varten ja toinen ajoneuvon peruuttaminen tunnistamista varten. Sekä näiden mallien valmiin lainan osa analyysin (KYS) algoritmin käytetään poikkeavuuksista tunnistamista varten. 

**Ylläpito tunnistus malli**

Jos jokin seuraavista kolmesta ilmaisimet - tire paine, ohjelma oil tai ohjelma lämpötila - täyttää vastaaviin kunto-ylläpito tunnistus mallin raportit poikkeavuuksista. Tuloksena on vain otettava huomioon nämä kolme muuttujaa muodostamisessa mallin. Azure koneen Learning-Microsoftin kokeessa ensin Käytämme **Valitse sarakkeet-tietojoukko** moduuli Pura nämä kolme muuttujaa. Seuraavaksi Käytämme KYS perustuva poikkeavuuksista tunnistus moduulin luonnissa poikkeavuuksista tunnistus-malli. 

Lyhennys osan analysointi (KYS) on muodostettu koneen learning, joka voidaan lisätä ominaisuudet ja luokittelu poikkeavuuksista tunnistus-tekniikkaa. KYS muuntaa joukon sisältävä mahdollisesti Korreloidun muuttujat kutsutaan tärkeimmät osat arvojoukon kirjainkoko. KYS perustuva mallinnus avaimen verrata on projektitietojen alemman kolmiulotteisen tilaa sivulle, josta ominaisuudet ja poikkeamia helpommin tunnistaa.
 
Kunkin uuden Syötteen tunnistus mallin poikkeavuuksista ilmaisimen laskee ensin, eigenvectors sen ennuste ja laskee sitten normitettu jälleenrakennus-virhe. Normitetun tämä virhe on poikkeavuuksista kirjainarvosanan. Mitä suurempi virhe, Lisää erheellisiin esiintymä on. 

Tunnistus ylläpito-ongelman kunkin tietueen voidaan pitää vaiheessa 3 kolmiulotteisen välilyönnillä määritysten tire paine, ohjelma oil ja ohjelma lämpötilan koordinaatit. Ottaa nämä poikkeamia Microsoft project 3 kolmiulotteisen tilaa alkuperäisiä tietoja käyttämällä KYS 2 kolmiulotteisen tilaan. Näin ollen on määrittää parametrin osien käyttäminen KYS 2. Tämä parametri toistetaan on tärkeä rooli soveltamisesta KYS perustuva poikkeavuuksista tunnistus. Käyttämällä KYS ulkonevien tietojen, jälkeen voit tarkastellaan näitä poikkeamia helpommin.

**Peruuttaa poikkeavuuksista tunnistus malli** Peruuttaminen poikkeavuuksista tunnistus mallissa Käytämme tietojoukko ja KYS perustuva poikkeavuuksista sarakkeiden valitseminen tunnistus moduulit samalla tavalla kuin. Olemme tarkemmin sanottuna ensin Pura kolme muuttujaa - ohjelma-lämpötilan, ulkopuolelta lämpötilan ja nopeus - **Tietojoukko Valitse sarakkeet** -moduulin. On myös lisätä nopeus-muuttuja, koska ohjelma lämpötila yleensä on vaikutus nopeuden. Seuraavaksi Käytämme KYS perustuva poikkeavuuksista tunnistus moduulin Project 3 kolmiulotteisen tilaa tiedot 2 kolmiulotteisen tilaan. Peruuttaminen-ehto täyttyy ja niin ajoneuvon vaatii peruuttaminen, kun ohjelma lämpötilan ja ulkopuolelta lämpötilan vastaavuussuhteet erittäin heikentää. KYS perustuva poikkeavuuksista tunnistus algoritmin avulla on siepata poikkeamia KYS suorituksen jälkeen. 

Kun koulutus joko mallin annettava käytettävä Normaali tietoja, joka ei edellytä kuin Syötetiedot, joiden kouluttaminen KYS perustuva poikkeavuuksista tunnistus mallin ylläpito- tai Peruuta. Tulosmalli kokeen Käytämme koulutetun poikkeavuuksista tunnistus mallin esiintyvien riippumatta siitä, onko ajoneuvon edellyttää ylläpito tai Peruuta. 


### <a name="real-time-analysis"></a>Reaaliaikainen analyysi

Saat kaikki tärkeät ajoneuvon kuten ajoneuvon polttoaineen määrä, ohjelma lämpötilan, matkamittarin lukeminen, tire paine, ohjelma oil taso ja muiden parametrien keskiarvo käytetään seuraavia Stream Analytics SQL-kysely. Keskiarvojen käytetään tunnistaa poikkeamia, antaa ilmoitukset- ja määrittää tietyn alueen ylläpitämä ajoneuvot yleinen kunto ehdot ja yhdistää ne sitten demografia. 

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig15-vehicle-telematics-stream-analytics-query-for-real-time-processing.png)

Kuva 15 – Stream analytics kyselyn reaaliaikainen käsittelyä varten

Kaikki keskiarvo lasketaan 3 sekunnin TumblingWindow päälle. Esimerkissä on käytössä TubmlingWindow tällöin, koska ikkunat ja jatkuvat aikavälien tarvitsee. 

Lisätietoja Azure Stream Analytics kaikki "Windowing" ominaisuudet, valitse [Windowing (Azure Stream Analytics)](https://msdn.microsoft.com/library/azure/dn835019.aspx).

**Reaaliaikainen Ennustaminen**

Sovellus on ratkaisun operationalize reaaliajassa tietokoneen learning-mallin mukana. Tämän sovelluksen nimeltä "RealTimeDashboardApp" on luonut ja määrittänyt osana ratkaisun käyttöönotto. Sovelluksen suorittaa seuraavasti:

1.  Seuraa missä muodossa Analytics julkaisee tapahtumat kuvion jatkuvasti tapahtumaa-toiminnossa esiintymään. ![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig16-vehicle-telematics-stream-analytics-query-for-publishing.png)*Kuva 16 – Stream analytics kyselyn tietojen julkaiseminen tulos tapahtumaa-toiminnossa esiintymä* 

2.  Jokaisen tapahtuman, joka vastaanottaa tämän sovelluksen: 

    - Käsittelee tiedot käyttämällä tietokoneen Learning – vastaus näkyvissä pistemäärä (RRS) päätepiste. RRS päätepisteen automaattisesti julkaistaan käyttöönoton osana.
    - RRS tulos on julkaistu käyttämällä push API PowerBI tietojoukko.

Tämä kaava on myös koskevat skenaariot, johon haluat integroida Business rivi (LoB)-sovelluksen kanssa reaaliajassa analytics-työnkulku tilanteita, joissa esimerkiksi ilmoitukset, ilmoituksia ja viestintä.

Valitse Lataa mukautusten RealtimeDashboardApp Visual Studio-ratkaisun [RealtimeDashboardApp Lataa](http://go.microsoft.com/fwlink/?LinkId=717078) . 

**Suorittaa reaaliaikaisen Dashboard-sovellus**

1.  Verkkokaavio-näkymän PowerBI-solmu ja valitse Ominaisuudet-ruudussa "Lataa reaaliaikaisen Raporttinäkymät-ikkunan sovelluksen"-linkki. ![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig17-vehicle-telematics-powerbi-dashboard-setup.png)*Kuva 17 – PowerBI Raporttinäkymät-ikkunan määritysohjeet ovat*
2.  Pura ja tallentaa paikallisesti ![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig18-vehicle-telematics-realtimedashboardapp-folder.png) *Kuva 18 – RealtimeDashboardApp kansio*
3.  Suorita sovellus RealtimeDashboardApp.exe
4.  Anna kelvollinen Power BI-tunnistetiedot, kirjaudu sisään ja sitten Hyväksy ![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig19a-vehicle-telematics-realtimedashboardapp-sign-in-to-powerbi.png)![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig19b-vehicle-telematics-realtimedashboardapp-sign-in-to-powerbi.png) 

*Kuva 19 – RealtimeDashboardApp: PowerBI kirjautuminen*

>[AZURE.NOTE] Jos haluat tyhjentää PowerBI-tietojoukko, suorita "flushdata"-parametrin RealtimeDashboardApp: 

    RealtimeDashboardApp.exe -flushdata

### <a name="batch-analysis"></a>Erän analyysi

Tavoite tähän on näyttää, miten Contoso ominaisuudet käyttämällä Azure laskentatoimintoja menettelytapoja big datasta ja myönnä monipuolisia tiedot ja kuvio, käyttö toiminta ja ajoneuvon kunto. Näin voit:

- Asiakkaan parempana ja tekemällä siitä halvempaa antamalla havainnollistamisen ajo-käyttöä ja polttoaineen tehokas ajo toiminnat
- Kun muutoksista tietoja asiakkaat ja niiden ajo patters ohjaavat liiketoimintapäätösten ja antamaan parhaiten luokan tuotteet ja palvelut

Tämä ratkaisu on ovat kohdistamisen seuraavat arvot:

1.  **Kohdetoimialueen ajo toiminta**: tunnistaa trendi-mallit, sijainnit, ajo ehdot ja lisäämään tietoja, valitse kohdetoimialueen ajo kuviot vuoden ajan. Contoson ominaisuudet voi käyttää näitä tietoja markkinointikampanjoita, ajo uusia mukautettuja ominaisuuksia ja käyttö-pohjainen vakuutus.
2.  **Polttoaineen tehokas ajo toiminta**: tunnistaa trendi-mallit, sijainnit, ajo ehdot ja vuoden lisäämään tietoja polttoaineen tehokas ajo kuviot-aika. Contoson ominaisuudet voi käyttää näitä tietoja markkinointi kampanjat, ajo uusista ominaisuuksista ja ohjaimet ennakoiva raportoinnin kustannusten voimassa ja ympäristön helpossa muodossa ajo käyttöä. 
3.  **Peruuttaa mallit**: tunnistaa edellyttävän markkinoilta poistettujen mukaan operationalizing poikkeavuuksista tunnistus-konepohjaisten oppimistekniikoiden kokeen mallit

Seuraavassa esitetty kaikkien nämä arvot yksityiskohdat


**Kohdetoimialueen ajo-kaava**

Osioitu ajoneuvon signaalit ja diagnostiikkatiedot käsitellään myyntijakso nimeltä "AggresiveDrivingPatternPipeline" rakenteen avulla voit määrittää mallit, sijainti, ajoneuvon, ajo-olosuhteissa ja muut parametrit, jotka on poistettu käytöstä käytetään kohdetoimialueen ajo kuvio.

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig20-vehicle-telematics-aggressive-driving-pattern.png) 
*Kuva 20 – Aggressiivinen ajo kuvio-työnkulku*

Rakenne-komentosarjan nimeltä "aggresivedriving.hql" käytettäviä analysoiminen kohdetoimialueen ajo ehto-kaava sijaitsee "\demo\src\connectedcar\scripts"-kansioon ladatut zip. 

    DROP TABLE IF EXISTS PartitionedCarEvents; 
    CREATE EXTERNAL TABLE PartitionedCarEvents
    (
                vin                             string,
                model                           string,
                timestamp                       string,
                outsidetemperature              string,
                enginetemperature               string,
                speed                           string,
                fuel                            string,
                engineoil                       string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position      string,
                parking_brake_status            string,
                headlamp_status                 string,
                brake_pedal_status              string,
                transmission_gear_position      string,
                ignition_status                 string,
                windshield_wiper_status         string,
                abs                             string,
                gendate                         string
                                
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:PARTITIONEDINPUT}';

    DROP TABLE IF EXISTS CarEventsAggresive; 
    CREATE EXTERNAL TABLE CarEventsAggresive
    (
                vin                         string, 
                model                       string,
                timestamp                   string,
                city                        string,
                speed                       string,
                transmission_gear_position  string,
                brake_pedal_status          string,
                Year                        string,
                Month                       string
                                
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:AGGRESIVEOUTPUT}';



    INSERT OVERWRITE TABLE CarEventsAggresive
    select
    vin,
    model,
    timestamp,
    city,
    speed,
    transmission_gear_position,
    brake_pedal_status,
    "${hiveconf:Year}" as Year,
    "${hiveconf:Month}" as Month
    from PartitionedCarEvents
    where transmission_gear_position IN ('fourth', 'fifth', 'sixth', 'seventh', 'eight') AND brake_pedal_status = '1' AND speed >= '50'

*Kuva 21 – Aggressiivinen ajo kuvion rakenteen kysely*

Tiedostossa käytetään ajoneuvon siirto vaihde sijainti, jarrun Polkimien tila ja nopeuden yhdistelmä esiintyvien reckless/kohdetoimialueen ajo toiminta jarrutus kuvio on nopea perusteella. 

Kun putkijohto on suoritettu onnistuneesti, näet-tallennustilan tilin "connectedcar" säilössä luotu seuraavat osiot.

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig22-vehicle-telematics-aggressive-driving-pattern-output.png) 

*Kuva 22 – AggressiveDrivingPatternPipeline tulostus*


**Polttoaineen tehokas ajo-kaava**

Osioitu ajoneuvon signaalit ja diagnostiikkatiedot käsitellään myyntijakso nimeltä "FuelEfficientDrivingPatternPipeline". Rakenteen käytetään mallit, sijainti, ajoneuvon, ajo-olosuhteissa ja muita ominaisuuksia, jotka toimia polttoaineen tehokas ajo kuvio.

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig23-vehicle-telematics-fuel-efficient-driving-pattern.png) 

*Kuva 23 – polttoaineen tehokas ajo kuvion työnkulku*

Rakenne-komentosarjan nimeltä "fuelefficientdriving.hql" käytettäviä analysoiminen kohdetoimialueen ajo ehto-kaava sijaitsee "\demo\src\connectedcar\scripts"-kansioon ladatut zip. 

    DROP TABLE IF EXISTS PartitionedCarEvents; 
    CREATE EXTERNAL TABLE PartitionedCarEvents
    (
                vin                             string,
                model                           string,
                timestamp                       string,
                outsidetemperature              string,
                enginetemperature               string,
                speed                           string,
                fuel                            string,
                engineoil                       string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position      string,
                parking_brake_status            string,
                headlamp_status                 string,
                brake_pedal_status              string,
                transmission_gear_position      string,
                ignition_status                 string,
                windshield_wiper_status         string,
                abs                             string,
                gendate                         string
                                
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:PARTITIONEDINPUT}';

    DROP TABLE IF EXISTS FuelEfficientDriving; 
    CREATE EXTERNAL TABLE FuelEfficientDriving
    (
                vin                         string, 
                model                       string,
                city                        string,
                speed                       string,
                transmission_gear_position  string,                
                brake_pedal_status          string,            
                accelerator_pedal_position  string,                             
                Year                        string,
                Month                       string
                                
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:FUELEFFICIENTOUTPUT}';



    INSERT OVERWRITE TABLE FuelEfficientDriving
    select
    vin,
    model,
    city,
    speed,
    transmission_gear_position,
    brake_pedal_status,
    accelerator_pedal_position,
    "${hiveconf:Year}" as Year,
    "${hiveconf:Month}" as Month
    from PartitionedCarEvents
    where transmission_gear_position IN ('fourth', 'fifth', 'sixth', 'seventh', 'eight') AND parking_brake_status = '0' AND brake_pedal_status = '0' AND speed <= '60' AND accelerator_pedal_position >= '50'


*Kuva 24 – polttoaineen tehokkaan ajo kuvion rakenteen kyselyn*

Tiedostossa käytetään ajoneuvon siirto vaihde sijainnin yhdistelmä, jarrun Polkimien tila, nopeuden ja accelerator poljin sijainti esiintyvien polttoaineen tehokas ajo toiminta kiihtyvyyden jarrutukseen perusteella ja nopeuttaa kuviot. 

Kun putkijohto on suoritettu onnistuneesti, näet-tallennustilan tilin "connectedcar" säilössä luotu seuraavat osiot.

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig25-vehicle-telematics-fuel-efficient-driving-pattern-output.png) 

*Kuva 25 – FuelEfficientDrivingPatternPipeline tulostus*


**Ennusteiden peruuttaminen**

Konepohjaisten oppimistekniikoiden kokeen on valmisteltu ja julkaistaan verkkopalvelun osana ratkaisun käyttöönotto. Loppupisteen merkki näkyvissä pistemäärä erä on hyödyntää tässä työnkulussa factory linkitetty palveluna rekisteröity ja operationalized tietojen factory erä näkyvissä pistemäärä toimintojen avulla.

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig26-vehicle-telematics-machine-learning-endpoint.png) 

*Kuva 26 – Konepohjaisten oppimistekniikoiden päätepisteen rekisteröity linkitettyjen tietojen factory palvelu*

Rekisteröity linkitetyn palvelun käytetään DetectAnomalyPipeline sija poikkeavuuksista tunnistus mallin tiedot. 

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig27-vehicle-telematics-aml-batch-scoring.png) 

*Kuva 27 – tietojen factory Azure koneen Learning erä näkyvissä pistemäärä toimintaa* 

On muutama vaihe, suorittaa tämän myyntijakso tietojen valmistelua varten niin, että se voidaan operationalized näkyvissä WWW-palvelun pistemäärä erän. 

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig28-vehicle-telematics-pipeline-predicting-recalls.png) 

*Kuva 28 – DetectAnomalyPipeline, ajoneuvot, jotka edellyttävät markkinoilta poistettujen ennakoimista* 

Kun lasketaan on valmis, HDInsight-aktiviteetin käytetään prosessin ja koota tietoja, jotka on luokiteltu kuin poikkeamia mallia, jonka todennäköisyys tulos 0,60 mukaan tai uudempi versio.

    DROP TABLE IF EXISTS CarEventsAnomaly; 
    CREATE EXTERNAL TABLE CarEventsAnomaly 
    (
                vin                         string,
                model                       string,
                gendate                     string,
                outsidetemperature          string,
                enginetemperature           string,
                speed                       string,
                fuel                        string,
                engineoil                   string,
                tirepressure                string,
                odometer                    string,
                city                        string,
                accelerator_pedal_position  string,
                parking_brake_status        string,
                headlamp_status             string,
                brake_pedal_status          string,
                transmission_gear_position  string,
                ignition_status             string,
                windshield_wiper_status     string,
                abs                         string,
                maintenanceLabel            string,
                maintenanceProbability      string,
                RecallLabel                 string,
                RecallProbability           string
                                
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:ANOMALYOUTPUT}';

    DROP TABLE IF EXISTS RecallModel; 
    CREATE EXTERNAL TABLE RecallModel 
    (

                vin                         string,
                model                       string,
                city                        string,
                outsidetemperature          string,
                enginetemperature           string,
                speed                       string,
                Year                        string,
                Month                       string              
                                
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:RECALLMODELOUTPUT}';

    INSERT OVERWRITE TABLE RecallModel
    select
    vin,
    model,
    city,
    outsidetemperature,
    enginetemperature,
    speed,
    "${hiveconf:Year}" as Year,
    "${hiveconf:Month}" as Month
    from CarEventsAnomaly
    where RecallLabel = '1' AND RecallProbability >= '0.60'


Kun putkijohto on suoritettu onnistuneesti, näet seuraavat osiot-tallennustilan tilin "connectedcar" säilössä luotu.

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig30-vehicle-telematics-detect-anamoly-pipeline-output.png) 

*Kuva 30 – kuva 30 – DetectAnomalyPipeline tulostus*


## <a name="publish"></a>Julkaiseminen

### <a name="real-time-analysis"></a>Reaaliaikainen analyysi

Yksi stream analytics työn kyselyt julkaisee tapahtumat tulos tapahtumaa-toiminnossa esiintymä. 

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig31-vehicle-telematics-stream-analytics-job-publishes-output-event-hub.png)

*Kuva 31 – Stream analytics työn julkaisee tulos tapahtumaa-toiminnossa esiintymä*

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig32-vehicle-telematics-stream-analytics-query-publish-output-event-hub.png)

*Kuva 32 – Stream analytics kyselyn tulos julkaiseminen tapahtumaa-toiminnossa esiintymä*

Tapahtumien vuosta kulutettu ratkaisun sisältämät RealTimeDashboardApp mukaan. Tämän sovelluksen hyödyntää reaaliaikaisia näkyvissä pistemäärä koneen Learning – vastaus web-palvelu ja julkaisee voimassa oleva tietojen kulutus PowerBI tietojoukkoa. 

### <a name="batch-analysis"></a>Erän analyysi

Siirtoerän ja reaaliaikainen käsittelyn tulokset julkaistaan Azure SQL-tietokantataulukot käyttöön. Azure SQL-palvelimeen, tietokanta ja taulukot luodaan automaattisesti asennuksen komentosarjan osana. 

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig33-vehicle-telematics-batch-processing-results-copy-to-data-mart.png)

*Kuva 33 – eräkäsittely tulokset kopioi mart työnkulkuun*

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig34-vehicle-telematics-stream-analytics-job-publishes-to-data-mart.png)

*Kuva 34 – analytics työn julkaisee tietoja mart muodossa*

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig35-vehicle-telematics-data-mart-setting-in-stream-analytics-job.png)

*Kuva 35 – tietojen mart stream analytics projektin määrittäminen*


## <a name="consume"></a>Kuluttavat

Power BI antaa tämän ratkaisun monipuolisia Raporttinäkymät-ikkunan reaaliaikaiset tiedot ja ennakoivan analytics visualisointeja. 

Lisätietoja sähköpostin määrittämisestä PowerBI-raporttien ja koontinäytön napsauttamalla tätä. Lopullinen raporttinäkymät-ikkuna näyttää seuraavanlaiselta:

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig36-vehicle-telematics-powerbi-dashboard.png)

*Kuva 36 - PowerBI Raporttinäkymät-ikkunan*

## <a name="summary"></a>Yhteenveto

Tässä asiakirjassa on yksityiskohtaiset alirakenteeseen ajoneuvon Telemetriatietojen Analytics-ratkaisun. Tämä esitellään lambda arkkitehtuuri malli reaaliaikainen ja erä analytics ennusteiden ja toiminnot. Tätä mallia koskee Käyttötapaukset, jotka edellyttävät kuuma polku (reaaliajassa) monien ja kylmän polku (erä) analysoinnin. 
