<properties
    pageTitle="Kopioi tehtävän suorituskyky ja säätäminen opas | Microsoft Azure"
    description="Lisätietoja avaimen tekijät, jotka vaikuttaa suorituskykyyn tietojen siirto-Azure Data Factory, kun käytät Kopioi tehtävän."
    services="data-factory"
    documentationCenter=""
    authors="linda33wj"
    manager="jhubbard"
    editor="monicar"/>

<tags
    ms.service="data-factory"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="jingwang"/>


# <a name="copy-activity-performance-and-tuning-guide"></a>Kopioi tehtävän suorituskyky ja säätäminen opas
Azure tietojen Factory kopioi tehtävän toimittaa ladataan ratkaisu pääosin turvallinen, luotettava ja tehokas tietojen. Se mahdollistaa tietojen teratavua lähimpään kymmeneen kopio päivittäin yli cloud monipuolisia erilaisia, ja paikallisen tietojen stores. Blazing nopea tietojen lataaminen suorituskyky on avain, jotta voit keskittyä core "big datasta"-ongelman: kehittyneen analyysin ratkaisujen etkä saa laaja havainnollistamisen kaikki tiedoista.

Azure sisältää yrityksen arvosanojen joukko tietojen tallennustilan ja tietojen varasto ratkaisuja ja kopioi tehtävä on erittäin optimoitu tietojen ladataan versio, joka on helppo määritetään. Vain yksittäistä kopiota aktiviteettiin voit tehostaa:

- Tietoja ladataan **Azure SQL-tietovarasto** **1.2 GBps** etsiminen
- Tietoja ladataan **1,0 GBps** **Azure-Blob-säiliö**
- Tietojen lataaminen **Azure järvi tietovaraston** **1,0 GBps** etsiminen


Tässä artikkelissa:

- [Suorituskyvyn viitenumerot](#performance-reference) tuetut lähde- ja käsittelytoiminto tiedot tallennetaan, miten voit suunnitella projektin;
- Ominaisuudet, joita voi edistää kopioi siirtonopeuden eri skenaarioissa, mukaan lukien [rinnakkain kopio](#parallel-copy), [cloud tietojen siirtämistä yksiköt](#cloud-data-movement-units)ja [Kopioi vaiheistettu](#staged-copy);
- [Suorituskyvyn säätäminen ohjeita](#performance-tuning-steps) siitä, miten voit paranna suorituskykyä ja avaimen tekijöistä, jotka voivat vaikuttaa suorituskykyyn kopio.

> [AZURE.NOTE] Jos et ole tottunut kopioi tehtävän parempi, katso ennen lukeminen tämän artikkelin [tietojen avulla kopioi tehtävän siirtäminen](data-factory-data-movement-activities.md) .

## <a name="performance-reference"></a>Suorituskyvyn viittaus

![Suorituskyvyn matriisi](./media/data-factory-copy-activity-performance/CopyPerfRef.png)

> [AZURE.NOTE] Voit tehostaa suurempi siirtonopeuden mukaan hyödyntäminen Lisää tietojen siirtämistä yksiköt (DMUs) kuin oletusarvoista suurin DMUs, joka on 8 cloud cloud kopioi tehtävän suorittaminen. Esimerkiksi 100 DMUs, jossa voit kopioida tietoja Azure-Blob-Azure järvi tietovaraston 1 gigatavu sekunnissa korko. [Cloud tietojen siirtämistä yksiköt](#cloud-data-movement-units) on kohdassa Lisätietoja tätä ominaisuutta. Ota pyytää lisää DMUs [Azure tukevat](https://azure.microsoft.com/support/) .

Määritä Points to Huomautus

- Siirtonopeuden lasketaan seuraavan kaavan avulla: [tiedon määrä luku lähteestä] / [kopioi tehtävän suorittaminen kesto].
- Suorituskyvyn viitenumerot taulukossa on mitattu käyttäen [TPC-H](http://www.tpc.org/tpch/) tietojoukon Suorita yksittäistä kopiota toimintaa.
- Kopioitavat cloud tietojen stores määrittäminen vertailu **cloudDataMovementUnits** 1 ja 4 (tai 8). **parallelCopies** ei ole määritetty. [Rinnakkaisia kopio](#parallel-copy) on kohdassa Lisätietoja näistä toiminnoista.
- Azure tietojen stores-lähde- ja käsittelytoiminto ovat samassa Azure alueella.
- Saat hybridi (paikallisen cloud tai cloud paikalliseen) tietojen siirto yhdyskäytävän yksittäisen esiintymän oli käynnissä tietokoneessa, joka on erillään paikallisen tietovaraston. Seuraavassa taulukossa näkyy määritykset. Kun yksi tehtävä on käynnissä yhdyskäytävässä, kopiointi kulutettu vain pienen osan testi tietokoneen suorittimen, muistin tai kaistanleveys.
    <table>
    <tr>
        <td>SUORITIN</td>
        <td>32-sydämiä 2.20 GHz Intel Xeon E5-2660 v2</td>
    </tr>
    <tr>
        <td>Muisti</td>
        <td>128 GT</td>
    </tr>
    <tr>
        <td>Verkon</td>
        <td>Internet-liittymän: 10 Gbps; intranet-liittymää: 40 Gbps</td>
    </tr>
    </table>

## <a name="parallel-copy"></a>Rinnakkaisia kopio
Voit lukea tietoja lähteestä tai kirjoittaa tietoja kohteen **kopio-aktiviteetin suorittaminen rinnakkain**. Tämä toiminto tehostaa kopiointi siirtonopeuden ja vähentää tietojen siirtämiseen kuluvaa aikaa.

Tämä asetus on erilainen kuin tehtävän määrityksessä **samanaikainen** -ominaisuutta. **Samanaikainen** -ominaisuus määrittää määrän **samanaikainen kopioi tehtävän suoritetaan** voit käsitellä tietoja eri toiminta Windows (1 AM 2: 00, 2 AM 3 AM, 4 AM AM 3 ja niin edelleen). Tämä ominaisuus on hyötyä, kun suoritat historiallisten kuormituksen. Rinnakkaisia kopioi ominaisuuksien koskee **yhden tehtävän suorittaminen**.

Seuraavassa esimerkkitilanne on esitetty. Seuraavassa esimerkissä useita sektorit-aiemman on käsiteltävä. Tietoja Factory suorittaa kunkin sektorin kopioi tehtävän (tehtävä-asennus) esiintymä:

- Ensimmäinen tehtävä-ikkunassa (1 AM 2: 00) tietojen sektoria == > tehtävän suorittaminen 1
- Toinen tehtävä-ikkunassa (2: 00 3-AM) tietojen sektoria == > tehtävän suorittaminen 2
- Toinen tehtävä-ikkunassa (3 AM 4-AM) tietojen sektoria == > tehtävän suorittaminen 3

Ja niin edelleen.

Tässä esimerkissä **samanaikainen** -arvoksi on määritetty 2, kun **tehtävä Suorita 1** ja **tehtävän 2** kopioi tietoja kahden tehtävän windows **samanaikaisesti** tietojen siirtämistä suorituskyvyn parantamiseksi. Kuitenkin jos tehtävän suorittaminen 1 on liitetty useita tiedostoja, tietojen siirto-palvelun kopioi tiedostot lähteestä kohdetiedosto yksi kerrallaan.

### <a name="parallelcopies"></a>parallelCopies
**ParallelCopies** -ominaisuuden avulla voit ilmaista, jonka haluat kopioida tehtävän käyttämään rinnakkaisuus. Voit ajatella tätä ominaisuutta, voit lukea lähde tai kirjoittaa rinnakkain käsittelytoiminto tietojen myymälät kopioi aktiviteetin viestiketjuissa siirtyminen enimmäismäärä.

Määrittää Data Factory kunkin Suorita kopio-toiminnon käyttäminen tietojen kopioimiseen lähteestä tietojen tallentamiseen ja kohde-tiedot tallennetaan rinnakkain kopioiden lukumäärä. Rinnakkaisia kopiot, se käyttää oletusmäärän määräytyy lähde- ja käsittelytoiminto, jota käytät tyyppi.  

Lähde- ja käsittelytoiminto |   Oletusarvoinen rinnakkain kopioiden määrä määräytyy palvelu
------------- | -------------------------------------------------
Tietoja siirretään toiseen tiedostopohjaisia stores (Blob-objektien tallennustilaan; Tietosäilö järvi; Amazon S3; paikallisen-tiedostojärjestelmässä; paikallisen-HDFS) | Väliltä 1 – 32. Määräytyy kokoa tiedostot ja cloud tietojen siirtämistä yksiköiden (DMUs) käytettävä tietoja siirretään toiseen kaksi cloud tietojen myymälät tai käyttää hybrid kopioi (Jos haluat kopioida tiedot tai paikallisen tietojen-kaupasta) yhdyskäytävän tietokoneen fyysinen määritys.
**Azure-taulukkotallennus tietovaraston tietolähteen tietoja** tietojen kopioiminen | 4
Kaikki muut lähde- ja käsittelytoiminto paria | 1

Yleensä oletustoiminnan tulisi ilmoittaa sinulle parhaiten siirtonopeuden. Voit määrittää kuormituksen tietokoneissa, jotka isännöivät tiedot tallennetaan, tai säätämällä kopioi suorituskyvyn, voit ohittaa oletusarvoa ja määrittää **parallelCopies** -ominaisuuden arvon. Arvon on oltava väliltä 1 – 32 (molemmat mukaan lukien). Suorituksen aikana parhaan suorituskyvyn, kopioi tehtävä käyttää arvo, joka on pienempi tai yhtä suuri kuin arvo, joka määritetään.

    "activities":[  
        {
            "name": "Sample copy activity",
            "description": "",
            "type": "Copy",
            "inputs": [{ "name": "InputDataset" }],
            "outputs": [{ "name": "OutputDataset" }],
            "typeProperties": {
                "source": {
                    "type": "BlobSource",
                },
                "sink": {
                    "type": "AzureDataLakeStoreSink"
                },
                "parallelCopies": 8
            }
        }
    ]

Määritä Points to Huomautus

- Kun kopioit tietoja tiedostopohjaisia stores välillä, rinnakkaisuus tapahtuu tiedoston tasolla. Ei ole paloittelun sisällä yhteen tiedostoon. Todellinen Kopiomäärä parallel data siirto-palvelu käyttää kopiointi suorituksen aikana ei ole suurempi kuin sinulla tiedostojen määrän. Jos kopioi toiminta on **mergeFile**, kopioi tehtävän et voi hyödyntää rinnakkaisuus.
- Kun määrität **parallelCopies** -ominaisuuden arvon, harkitse kuormituksen lisäys lähde- ja käsittelytoiminto tietojen stores ja yhdyskäytävän jos se on hybrid kopio. Näin tapahtuu, erityisesti jos on useita toimintoja tai saman toimintoja, jotka suorittaa saman tietovaraston samanaikainen suoritetaan. Jos huomaat, että tietosäilö tai yhdyskäytävä on täyttyminen kuormituksen, pienennä vapauttaa kuormituksen **parallelCopies** -arvoa.
- Kun kopioit tietoja, joita ei voi tallentaa, jotka ovat tiedostopohjaisia tiedostopohjaisia stores, tietojen siirto-palvelun ohittaa **parallelCopies** -ominaisuus. Vaikka rinnakkaisuus on määritetty, sitä ei käytetä tässä tapauksessa.

> [AZURE.NOTE] Voit käyttää **parallelCopies** -ominaisuutta, kun teet hybrid kopio käytettävä Data Management Gateway 1.11 tai uudempi versio.

### <a name="cloud-data-movement-units"></a>Cloud tietojen siirtämistä yksiköt
**Cloud tietojen siirtämistä yksikön (dmu tässä)** mittaa, joka edustaa Data Factory yhtenä yksikkönä potenssiin (suorittimen ja muistin verkon resurssivaraukset yhdistelmä). Dmu Tässä voidaan cloud cloud kopiointi, mutta ei hybrid kopio.

Data Factory käyttää oletusarvon mukaan yksi cloud dmu Tässä yksittäisen kopio-toiminnon suorittaminen. Voit ohittaa tämän oletuksena, Määritä **cloudDataMovementUnits** -ominaisuuden arvon seuraavasti. Saat lisätietoja suorituskyvyn taso voit saada, kun määrität tietyn kopioi lähde- ja käsittelytoiminto Lisää yksiköt-Hae [suorituskyvyn viittaus](#performance-reference).

    "activities":[  
        {
            "name": "Sample copy activity",
            "description": "",
            "type": "Copy",
            "inputs": [{ "name": "InputDataset" }],
            "outputs": [{ "name": "OutputDataset" }],
            "typeProperties": {
                "source": {
                    "type": "BlobSource",
                },
                "sink": {
                    "type": "AzureDataLakeStoreSink"
                },
                "cloudDataMovementUnits": 4
            }
        }
    ]

(Oletus)-1, 2, 4 ja 8 **Sallitut arvot** **cloudDataMovementUnits** -ominaisuus on. Kopiointi suorituksen aikana käyttämä **cloud DMUs tosiasiallinen määrä** on yhtä suuri kuin tai pienempi kuin määritetty arvo tietojen kuvion mukaan. 

> [AZURE.NOTE] Jos tarvitset lisää cloud DMUs suurempi siirtonopeuden, ota [Azure tukevat](https://azure.microsoft.com/support/). Asetusta 8 ja yllä toimii tällä hetkellä vain silloin, kun useita tiedostoja kopioi Blob-objektien tallennustilaan järvi tietosäilö tai Azure SQL-tietokanta-Blob-säiliö ja tiedostokoko on suurempi tai yhtä suuri 16 Mt yksitellen.

Nämä kaksi ominaisuuksien avulla paremmin ja parantaa tietojen siirto-siirtonopeuden on [otoksen käyttötapauksiin](#case-study-use-parallel-copy). Ei tarvitse määrittää **parallelCopies** hyödyntää oletustoiminnan. Jos määrität ja **parallelCopies** on liian pientä, useita cloud DMUs ehkä ole täysin käyttää.  

On **tärkeää** muistaa, että perittävän perusteella kopiointi kokonaisajan. Jos Kopioi projekti käyttää mukaasi tunnin yhden cloud yksikkö ja nyt kestää neljä cloud yksiköt 15 minuuttia, yleinen laskun pysyy lähes samat. Voit käyttää esimerkiksi neljä cloud yksiköt. Ensimmäinen cloud yksikkö käyttää 10 minuutin toinen tehtävä 10 minuutin, kolmas yksi 5 minuuttia ja neljännen yksi 5 minuuttia kaikki kopio-toiminnossa Suorita. Perittävän yhteensä kopioi (tietojen siirto) ajan, joka on 10 + 10 + 5 + 5 = 30 minuuttia. **ParallelCopies** käyttäminen ei vaikuta laskutus.

## <a name="staged-copy"></a>Vaiheistettu kopio
Kun kopioit tietoja lähde tietosäilö käsittelytoiminto tietosäilö, voit käyttää Blob-objektien tallennustilaan väliaikaisen väliaikaisen säilön. Väliaikaisen on hyötyä seuraavissa tapauksissa:

1.  Jos **haluat ingest tietojen tuominen SQL-tietovarasto kautta PolyBase tiedot sijaitsevat**. SQL-tietovarasto käyttää PolyBase suuren siirtonopeuden järjestelmä, jonka voit ladata suuria määriä tietoa SQL-tietovarasto. Tietolähteen tietojen on oltava Blob-objektien tallennustilaan, ja se on täytettävä lisäehtoja. Kun lataat tiedot kuin Blob-objektien tallennustilaan tietosäilö, voit ottaa tietojen kopioiminen väliaikaisen väliaikaisen Blob-objektien tallennustilaan kautta. Tässä tapauksessa Data Factory suorittaa tarvittavat tiedot muunnokset varmistaakseen, että se täyttää vaatimukset PolyBase. Valitse se käyttää PolyBase ladata tietoja SQL-tietovarasto. Lisätietoja on artikkelissa [Lataamaan tietojen tuominen SQL Azure-tietovarasto PolyBase käyttöä](data-factory-azure-sql-data-warehouse-connector.md#use-polybase-to-load-data-into-azure-sql-data-warehouse).
2.  **Joskus kestää hetken suorittamiseen hybrid-tietojen siirto (eli kopioi paikalliset tiedot välillä tallentaminen ja cloud tietojen tallentaa) kautta verkkoyhteys on hidas**. Suorituskyvyn parantamiseksi voit pakata tietojen paikallisen niin, että kuluu vähemmän aikaa tietojen siirtäminen väliaikaisen tietovaraston pilveen. Valitse väliaikaisen kaupan tiedot voit purkaa, ennen kuin lataat sen kohde-tietosäilö.
3.  **Et halua Avaa portit kuin portti 80 ja porttiin 443 palomuurissa yrityksen IT-käytäntöjä vuoksi**. Esimerkiksi kun kopioit tietoja paikallisen-tietovarasto Azure SQL-tietokanta-käsittelytoiminto tai SQL Azure-tietovarasto käsittelytoiminto, sinun on aktivoitava lähtevän TCP-tietoliikenteen porttia 1433 Windowsin palomuuri-ja yrityksen palomuuri. Tässä skenaariossa voit hyödyntää ensimmäisen kopioi tiedot yhdyskäytävä Blob storage väliaikaisen esiintymään HTTP tai HTTPS porttiin 443. Valitse Lataa tiedot SQL-tietokanta tai SQL-tietovarasto Blob storage väliaikaisesta. Tässä työnkulussa ei tarvitse ottaa käyttöön porttia 1433.

### <a name="how-staged-copy-works"></a>Kuinka vaiheistettu kopioi toiminta
Väliaikaisen ominaisuus aktivoidessasi ensin tiedot kopioidaan tietovaraston tietolähteen väliaikaisen tietovarasto (Tuo oman). Seuraavaksi tiedot kopioidaan väliaikaisen tietovaraston käsittelytoiminto tietovaraston. Tietoja Factory hallitsee automaattisesti kaksivaiheinen kulun puolestasi. Tietoja Factory myös tyhjentää tietojen tilapäinen väliaikaisen säilöstä tietojen siirto päätyttyä.

Cloud kopioi skenaariossa (lähde- ja käsittelytoiminto tiedot säilöt ovat pilvipalvelussa) yhdyskäytävä ei ole käytössä. Data Factory-palvelun suorittaa kopio-toimintoja.

![Kopioi vaiheistettu: Cloud-skenaario](media/data-factory-copy-activity-performance/staged-copy-cloud-scenario.png)

Hybrid kopioi skenaariossa (lähde on paikallisen ja käsittelytoiminto on pilvipalvelussa), yhdyskäytävän siirtää tietoja väliaikaisen tietovaraston tietolähteen tiedot-kaupasta. Tietojen Factory-palvelu siirtää tietoja väliaikaisen tietovaraston käsittelytoiminto tietovaraston. Tietojen kopioiminen paikallisen-tietovarasto kautta väliaikaisen cloud tietosäilö myös tukee käänteinen työnkulku.

![Kopioi vaiheistettu: Hybrid-skenaario](media/data-factory-copy-activity-performance/staged-copy-hybrid-scenario.png)

Aktivoidessasi tietojen siirto väliaikaisen säilön avulla voit määrittää, haluatko tiedot ennen tietojen siirtämistä väliaikainen tai väliaikaisen tietovaraston tietolähteen tietovaraston pakattu ja sitten puretaan ennen tietojen siirtämisestä väliaikainen tai väliaikaisen tietovaraston käsittelytoiminto tietojen kauppa.

Tällä hetkellä voi kopioida tietoja kaksi paikallisen tietojen stores käyttämällä väliaikaisen säilön välillä. Odotamme tämä asetus on käytettävissä pian.

### <a name="configuration"></a>Määritys
Kopioi-toiminnossa voit määrittää, haluatko tietojen vaiheistettu Blob-objektien tallennustilaan, ennen kuin lataat sen kohde tietosäilö **enableStaging** määritykset. Kun määrität **enableStaging** TOSI, Määritä seuraavassa taulukossa lueteltuja lisäominaisuudet. Jos sinulla ei ole, sinun on myös luominen Azure-tallennustilan tai tallennustilan jaetun vaiheet access allekirjoitus linkitetty palvelun.

Ominaisuus | Kuvaus | Oletusarvo | Pakollinen
--------- | ----------- | ------------ | --------
**enableStaging** | Määritä, jonka haluat kopioida tietoja kohdan väliaikaisen kaupan kautta. | EPÄTOSI | Ei
**linkedServiceName** | Määritä [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service) tai [AzureStorageSas](data-factory-azure-blob-connector.md#azure-storage-sas-linked-service) linkitetyn palvelun, joka viittaa tallennustilan esiintymän, käyttää väliaikaisen väliaikaisen säilön nimi. <br/><br/> Et voi käyttää tallennustilan jaettua käyttöä allekirjoituksella voit ladata tietoja SQL-tietovarasto PolyBase kautta. Voit käyttää sitä muissa tilanteissa. | PUUTTUU | Kyllä, kun **enableStaging** on TOSI
**polku** | Määritä Blob storage polku, jonka haluat sisältävät vaiheistettu tiedot. Jos et anna polku-palvelun Luo tilapäisten säilön. <br/><br/> Määritä polku, vain, jos käytät tallennustilan jaettua käyttöä allekirjoituksella tai asetat väliaikaiset tiedot tietyssä paikassa. | PUUTTUU | Ei
**enableCompression** | Määrittää, onko tietojen pitäisi pakkaus, ennen kuin se kopioidaan kohteeseen. Tämä asetus pienentää siirrettävän tietoja on paljon. | EPÄTOSI | Ei

Tässä on esimerkki määritelmä kopioi toiminnan ominaisuudet, jotka on kuvattu edellisessä taulukossa:

    "activities":[  
    {
        "name": "Sample copy activity",
        "type": "Copy",
        "inputs": [{ "name": "OnpremisesSQLServerInput" }],
        "outputs": [{ "name": "AzureSQLDBOutput" }],
        "typeProperties": {
            "source": {
                "type": "SqlSource",
            },
            "sink": {
                "type": "SqlSink"
            },
            "enableStaging": true,
            "stagingSettings": {
                "linkedServiceName": "MyStagingBlob",
                "path": "stagingcontainer/path",
                "enableCompression": true
            }
        }
    }
    ]


### <a name="billing-impact"></a>Laskutus vaikutus
Peritään mukaan seuraavalla tavalla: kopioi keston ja kopioi tyyppi. 

- Kun käytät väliaikaisen aikana (tietojen kopioiminen cloud tietosäilö toiseen cloud tietojen Store) cloud kopio, perittävän [summa kopioi keston vaiheet 1 ja 2] [cloud kopioi Yksikköhinta] x.
- Kun käytät väliaikaisen aikana (tietojen kopioiminen paikallisen tietojen Storesta cloud tietosäilö) hybrid kopio, perittävän [hybrid kopioi kesto] x [hybrid kopioi Yksikköhinta] + [cloud kopioi kesto] [cloud kopioi Yksikköhinta] x.


## <a name="performance-tuning-steps"></a>Suorituskyvyn säätäminen vaiheet
Suosittelemme kestää suorituskyvyn parantamisesta Data Factory-palvelun kopioi aktiviteettiin seuraavasti:

1.  **Muodosta perusaikataulun**. Suunnitteluvaiheessa Testaa yhteyttä myyntijakso käyttämällä Kopioi tehtävän edustavan tietojen otoksen perusteella. Voit käyttää tietojen Factory [viipaloiminen mallin](data-factory-scheduling-and-execution.md#time-series-datasets-and-data-slices) tiedot, joiden kanssa teet yhteistyötä määrän rajoittaminen.

    Kerää suoritusaika ja suorituskyvyn **Seuranta ja hallinta-sovellus**. Valitse **näytön ja hallitse** Factory tietojen lukeminen aloitussivulla. Valitse puunäkymässä **tulosteen tietojoukko**. Valitse Kopioi tehtävän suorittaminen **Toiminta Windows** -luettelossa. **Tehtävän Windows** on lueteltu kopioi tehtävän keston ja tiedot, jotka on kopioitu kokoa. Siirtonopeuden näkyy **tehtävän ikkunan**Resurssienhallinnassa. Lisätietoa sovelluksesta on artikkelissa [näyttö ja hallita Azure Data Factory putkistot seuranta ja hallinta-sovelluksen avulla](data-factory-monitor-manage-app.md).

    ![Tehtävän suorittaminen tiedot](./media/data-factory-copy-activity-performance/mmapp-activity-run-details.png)

    Artikkelin voit verrata suorituskyky ja kokoonpano käyttämässäsi skenaariossa kopioi tehtävän [suorituskyvyn viittaus](#performance-reference) Microsoftin tuloksia.
2. **Diagnosointi ja suorituskyvyn**. Jos huomaat suorituskyvyn ei vastaa odotuksiasi, sinun on tunnistaa performance pullonkaulojen. Tämän jälkeen voit parantaa suorituskykyä, poistaminen tai pienentäminen pullonkaulojen vaikutus. Suorituskyvyn vianmääritys täydellinen kuvaus on tämän artikkelin laajemmin, mutta seuraavassa on joitakin yleisiä asioita:
    - Suoritustoiminnot:
        - [Rinnakkaisia kopio](#parallel-copy)
        - [Cloud tietojen siirtämistä yksiköt](#cloud-data-movement-units)
        - [Vaiheistettu kopio](#staged-copy)   
    - [Lähde](#considerations-for-the-source)
    - [Käsittelytoiminto](#considerations-for-the-sink)
    - [Sarjatoiminto ja sarjoituksen](#considerations-for-serialization-and-deserialization)
    - [Pakkaus](#considerations-for-compression)
    - [Sarakkeiden yhdistäminen](#considerations-for-column-mapping)
    - [Data Management Gateway](#considerations-for-data-management-gateway)
    - [Muita huomioon otettavia seikkoja](#other-considerations)

3. **Laajenna koko tietojoukko määritykset**. Kun olet tyytyväinen suorittamisen tulokset ja suorituskyvyn, voit laajentaa määrityksen ja myyntijakso käyttöajan peittää koko tietojoukko.

## <a name="considerations-for-the-source"></a>Huomioitavaa lähde
### <a name="general"></a>Yleiset
Varmista, että pohjana tietovaraston ei täyttyminen muiden toiminnoista, jotka ovat käynnissä, tai sitä vastaan. 

Katso Microsoft tietojen stores [valvominen ja säätäminen ohjeita](#performance-reference) , jotka liittyvät Microsoft Data ja ymmärrät tietojen tallentaminen suorituskyvyn, vastaus kertaa pienentää ja suurentaa nopeus-Ohje.

Jos kopioit tietoja Blob-objektien tallennustilaan SQL-tietovarasto, kannattaa käyttää **PolyBase** parantaa suorituskykyä. [Käytä PolyBase lataamaan tietojen tuominen SQL Azure-tietovarasto](data-factory-azure-sql-data-warehouse-connector.md###use-polybase-to-load-data-into-azure-sql-data-warehouse) lisätietoja.


### <a name="file-based-data-stores"></a>Tiedosto-pohjaisten tietojen säilöt
*(Mukaan lukien Blob storage, järvi tietosäilö, Amazon S3, paikallisen tiedostojärjestelmien ja paikallisen HDFS)*

- **Keskimääräinen tiedostokoko ja tiedostojen määrä**: kopioi tehtävän siirtää datatiedoston yksi kerrallaan. Tietoja on siirrettävä saman verran yleinen siirtonopeuden on pienempi, jos tiedot koostuu usean pieni tiedoston muutaman suurten tiedostojen vuoksi käynnistyksen vaihe kunkin tiedoston sijaan. Sen vuoksi, jos mahdollista, yhdistää pieniä tiedostoja suurempia tiedostoja ja myönnä suurempi siirtonopeuden.
- **Tiedostomuoto ja pakkaus**: muita tapoja parantaa suorituskykyä on kohdissa [Sarjatoiminto sekä sarjoituksen](#considerations-for-serialization-and-deserialization) ja [huomioon otettavia seikkoja pakkaus](#considerations-for-compression) .
- **Paikallisen tiedostojärjestelmän** -tilanteeseen, jossa **Data Management Gateway** vaaditaan, [Huomioitavaa Data Management Gateway](#considerations-for-data-management-gateway) on kohdassa.

### <a name="relational-data-stores"></a>Relaatiotietoja säilöt
*(Mukaan lukien SQL-tietokantaan. SQL-tietovarasto; Amazon Redshift; SQL Server-tietokannat; ja Oracle-MySQL, DB2, Teradata, Sybase ja PostgreSQL-tietokantojen jne.)*

- **Tietoja kuvio**: Taulukkorakenteen kopio nopeus vaikuttaa. Suuri rivin koko avulla voit parantaa suorituskykyä, pieni rivin kokoa, voit kopioida tiedot yhtä paljon. Syynä on se, että tietokanta tehokkaammin voit hakea tietoja, jotka sisältävät vähemmän rivejä vähemmän erissä.
- **Kysely tai tallennettu toimintosarja**: optimoida kyselyn tai tallennetun toimintosarjan kopioi tehtävän lähteen hakeaksesi tietoja tehokkaammin määritettyyn logiikan.
- **Paikallisen relaatiotietokannasta**, kuten SQL Server tai Oracle-joka edellyttää **Data Management Gateway**- [Huomioitavaa Data Management Gateway](#considerations-on-data-management-gateway) -osiossa.

## <a name="considerations-for-the-sink"></a>Huomioitavaa käsittelytoiminto

### <a name="general"></a>Yleiset
Varmista, että pohjana tietovaraston ei täyttyminen muiden toiminnoista, jotka ovat käynnissä, tai sitä vastaan. 

Kohdassa Microsoft tietojen stores [valvominen ja säätäminen ohjeita](#performance-reference) , jotka koskevat tiedot stores. Nämä artikkelit auttavat lisätietoja tietojen myymälän suorituskyky ominaisuuksia sekä vastauksen kertaa pienentää ja suurentaa siirtonopeuden.

Jos kopioit tietoja **SQL-tietovarasto**- **Blob-objektien tallennustilaan** , kannattaa käyttää **PolyBase** parantaa suorituskykyä. [Käytä PolyBase lataamaan tietojen tuominen SQL Azure-tietovarasto](data-factory-azure-sql-data-warehouse-connector.md###use-polybase-to-load-data-into-azure-sql-data-warehouse) lisätietoja.


### <a name="file-based-data-stores"></a>Tiedosto-pohjaisten tietojen säilöt
*(Mukaan lukien Blob storage, järvi tietosäilö, Amazon S3, paikallisen tiedostojärjestelmien ja paikallisen HDFS)*

- **Kopioi toiminta**: Jos kopioit tietoja eri tiedostopohjaisia tietosäilö, kopioi tehtävä on kolme **copyBehavior** -ominaisuuden kautta. Se säilyttää hierarkian, objektiksi hierarkian tai yhdistää tiedostot. Säilyttäminen tai litistämistä hierarkian on pieni tai suorituskyvyn katseltavan, mutta yhdistetään tiedostoja aiheuttaa suorituskyvyn katseltavan niin, että.
- **Tiedostomuoto ja pakkaus**: kohdissa [Sarjatoiminto sekä sarjoituksen](#considerations-for-serialization-and-deserialization) ja [huomioon otettavia seikkoja pakkaamisen](#considerations-for-compression) enemmän tapoja parantaa suorituskykyä.
- **Blob-objektien tallennustilaan**: tällä hetkellä Blob storage tukee vain estää BLOB optimoitu tiedonsiirto ja siirtonopeuden.
- **Paikallisen tiedoston järjestelmien** tilanteessa, joissa on käytettävä **Data Management**Gatewayn [Huomioitavaa Data Management Gateway](#considerations-for-data-management-gateway) on kohdassa.

### <a name="relational-data-stores"></a>Relaatiotietoja säilöt
*(Mukaan lukien SQL-tietokantaan, SQL-tietovarasto, SQL Server-tietokannat ja Oracle-tietokannat)*

- **Kopioi toiminta**: mukaan **sqlSink**määrittämäsi ominaisuudet, kopioi tehtävän kirjoittaa tietoja kohdetietokannassa eri tavoilla.
    - Oletusarvon mukaan tietojen siirtämistä palvelu käyttää Lisää tiedot joukkona kopio-Ohjelmointirajapinnan Lisää tilaa, mikä tarjoaa parhaan suorituskyvyn.
    - Jos määrität tallennetun toimintosarjan käsittelytoiminto, tietokannan koskee yhden tietorivin aikaan joukkona kuormituksen sijaan. Suorituskyvyn pudottaa merkittävästi. Jos tietojoukko on suuri, kun se on mahdollista, harkitse siirtymistä **sqlWriterCleanupScript** -ominaisuuden avulla.
    - Jos määrität **sqlWriterCleanupScript** -ominaisuus suorittaa kopioi jokaiselle tehtävälle, käynnistää palvelua komentosarjan ja sitten joukkona kopioi Ohjelmointirajapinnan avulla voit lisätä tiedot. Korvaa koko taulukon uusimmat tiedot, voit määrittää komentosarjan, joka poistaa ensin kaikki tietueet ennen lähteestä uudet tiedot joukkona-lataamista.
- **Tietojen kuvio ja erä koko**:
    - Taulukkorakenteen vaikuttaa kopioi siirtonopeuden. Kopioitavat tiedot yhtä paljon suuri rivin koko antaa sinulle parempi suorituskyky kuin pieni rivin koko sillä tietokannan tehokkaammin voit vahvistaa vähemmän erissä tiedot.
    - Kopioi tehtävän Lisää tiedot erissä sarjaa. Voit määrittää, kuinka monta riviä erän **writeBatchSize** -ominaisuuden avulla. Jos tiedoissa on pieni rivit, voit määrittää **writeBatchSize** -ominaisuutta, jolla pienempi erä yleiskuluja ja uudempi versio siirtonopeuden hyötyvät suuremmat arvot. Jos tietojen rivin kokoa on suuri, varovainen, kun lisäät **writeBatchSize**. Suuri arvo saattaa johtaa aiheutuvat ylikuormituksen tietokannan kopion epäonnistui.
- SQL Server ja Oracle, johon **Data Management Gateway**on käytettävä, kuten **paikallisen relaatiotietokannasta** [Data Management Gateway Huomioitavaa](#considerations-for-data-management-gateway) kohdassa.


### <a name="nosql-stores"></a>NoSQL säilöt
*(Mukaan lukien taulukkotallennus ja Azure DocumentDB)*

- **Taulukkotallennus**:
    - **Osion**: kirjoittaminen limitetty osioiden huomattavasti heikentää suorituskykyä. Lähdetiedot lajitella osio-näppäintä, niin, että tiedot on lisätty tehokkaasti yhden osion toiselle tai Säädä logiikan kirjoittaa tiedot osio.
- Saat **DocumentDB**:
    - **Erän koko**: **writeBatchSize** -ominaisuus määrittää rinnakkain pyyntöjen määrä DocumentDB-palveluun, asiakirjojen luomiseen. Voit odottaa suorituskyvyn parantamiseksi voit suurentaa **writeBatchSize** , koska enemmän rinnakkain pyyntöjä lähetetään DocumentDB. Kuuntelemaan kuitenkin rajoitusta, kun kirjoitat DocumentDB (virhesanoma on "Pyytää korko on suuri"). Eri tekijät voivat aiheuttaa asiakirjat ja kohde-sivustokokoelman indeksoinnin käytännön määrän rajoittaminen mukaan lukien asiakirjan koko. Tavoitteet suurempi kopioi siirtonopeuden, kannattaa käyttää paremmin sivustokokoelman, esimerkiksi S3.

## <a name="considerations-for-serialization-and-deserialization"></a>Sarjatoiminto sekä sarjoituksen
Sarjatoiminto ja sarjoituksen voi ilmetä, kun syötteen tietojoukon tai tulosteen tietojoukon on. Tällä hetkellä kopioi tehtävän tukee Avro ja teksti (esimerkiksi CSV ja TSV) tietomuotojen.

**Kopioi toiminta**:

-   Kopioidaan tiedostoja tiedostopohjaisia Microsoft Data välillä:
    - Kun syöttö- ja tietojoukot molemmat on sama tai tiedosto ei ole muotoiluasetukset, tietojen siirto-palvelun suorittaa ilman Sarjatoiminto tai sarjoituksen binaarinen kopio. Näet suurempi siirtonopeuden, verrattuna tilanne, jossa lähde- ja käsittelytoiminto tiedoston muotoiluasetukset ovat eroavat toisistaan.
    - Kun syötteen ja tulosteen tietojoukot molemmat ovat teksti-muodossa ja vain koodaus tyyppi on erilainen, tietojen siirto-palvelu suorittaa vain koodauksen muuntaminen. Se ei tee mitään Sarjatoiminto ja sarjoituksen, joka aiheuttaa joidenkin suorituskyvyn yleisrasite verrattuna binaarinen kopio.
    - Kun syöttö- ja tietojoukot molemmat on eri tiedostomuodoissa tai eri määritykset, kuten erottimet, tietojen siirto-palvelun deserializes lähdetietojen virtauttaa, muuntaminen ja muuntaa sen sarjaksi olet määrittänyt tulostus-muotoon. Tämä toiminto tuloksena paljon merkittäviä yleisrasite verrattuna muissa tilanteissa.
- Kun olet kopioinut tiedostojen tietovarasto, jota ei ole tiedostopohjaisia (esimerkiksi tiedostopohjaisia kauppa relaatio Store), Sarjatoiminto tai sarjoituksen vaihe on pakollinen. Tässä vaiheessa aiheuttaa merkittäviä katseltavan.

**Tiedostomuoto**: tiedostomuodossa, voit valita voi olla vaikutusta kopioi suorituskykyä. Esimerkiksi Avro on compact binaarimuodossa, metatiedot tiedoilla. Se on laaja tuki Hadoop ekosysteemissä käsittelyn ja kyselyt. Avro on kuitenkin kalliimpi Sarjatoiminto ja sarjoituksen, joka johtaa alemman kopioi siirtonopeuden teksti-muotoon nähden. Tee valintasi tiedostomuodon käsittely kulun koko holistically. Missä muodossa tiedot alkavat on tallennettu lähde tietojen stores- tai purkaa ulkoiset järjestelmät; paras muoto tallennustilaa, analytical käsittely ja kyselyt; ja missä muodossa tiedot viedään kyselyjä tietojen marts raportointi-ja visualisointia varten. Joskus tiedostomuoto, joka on lainkaan, lue ja kirjoita suorituskyky voi olla hyvä vaihtoehto, kun mietit yleinen analyysin aikana.

## <a name="considerations-for-compression"></a>Huomioitavaa pakkaus
Kun syöttö- tai tietojoukon on tiedoston, voit määrittää kopioi tehtävän suorittamiseen pakkaamisen tai purku, kun se kirjoittaa tiedot kohteeseen. Valitessasi pakkaamisen teet sopivat i (i/o) ja Suoritin. Pakataan ylimääräisiä Laske resurssien tietoja kustannukset. Mutta, se vähentää verkon i/o ja tallennustilaa. Tietojen mukaan näkyviin voi tulla tehon lisäys-yleistä kopioi siirtonopeuden.

**Pakkauksenhallinnan**: kopioi tehtävän tukee gzip, bzip2 ja Deflate pakkaamisen tyyppejä. Azure Hdinsightiin voit käyttää kaikkia kolmea tietotyyppiä käsittelyä varten. Kunkin pakkauksenhallintaa on etuja. Esimerkiksi bzip2 on pienin kopioi siirtonopeuden, mutta saat parhaan rakenteen kyselyn suorituskyvyn bzip2, koska voit jakaa sen käsittelyä varten. GZip on eniten tasapainoinen ja niitä käytetään useimmin. Valitse pakkauksenhallinta lopusta loppuun-skenaario parhaiten sopiva.

**Tason**: Voit valita kunkin pakkauksenhallintaa kahdella tavalla: nopeimmin pakattu ja optimaalisesti pakattu. Nopeimmin pakattu vaihtoehto pakkaa tiedot mahdollisimman nopeasti, vaikka tiedosto ei pakata optimaalisesti. Optimaalisesti pakattu asetus käyttää enemmän aikaa pakkaus ja tuottaa vähän tietoja. Voit testata molemmat vaihtoehdot nähdäksesi, joka sisältää tapaus yleinen suorituskyvyn parantamiseksi.

**A huomiota**: kopioi suuria määriä tietoa paikallisen säilön ja pilveen, kannattaa käyttää väliaikainen Blob-objektien tallennustilaan pakkaamisen kanssa. Käyttämällä väliaikainen tallennustila on hyötyä, kun yrityksen verkossa ja Azure palvelujen kaistanleveys on rajoittava tekijä ja haluat syötteen tietojoukon ja tulosteen tietojoukon sekä pakkaamattomat muodossa. Tarkemmin voit jakaa yksittäistä kopiota tehtävän osiin kaksi kopioi toimintoja. Kopioi ensimmäiseen toimintoon kopioi lähteestä väliaikainen tai väliaikaisen blob pakattu lomakkeessa. Kopioi toinen tehtävä pakattujen tietojen kopioi väliaikaisen ja purkaa sitten samalla, kun se kirjoittaa käsittelytoiminto.

## <a name="considerations-for-column-mapping"></a>Huomioitavaa sarakkeen yhdistäminen
Voit määrittää **columnMappings** -ominaisuuden kopioi toimintaa kartan kaikki tai osan syötteen sarakkeiden tulostus-sarakkeisiin. Kun tietojen siirto-palvelu lukee tiedot lähteestä, se on suoritettava sarakkeen yhdistäminen tietoihin, ennen kuin se kirjoittaa tiedot käsittelytoiminto. Tämä ylimääräinen käsittely vähentää kopioi siirtonopeuden.

Jos lähdetiedot tallentaa on kyseltäväksi, esimerkiksi, jos se on suhteellinen säilö, kuten SQL-tietokanta tai SQL Server tai jos se on NoSQL kaupan, kuten taulukkotallennus tai DocumentDB, harkitse valitseminen sarakkeen suodattaminen ja uudelleenjärjestämisen logiikan sijaan sarakkeen yhdistäminen **kysely** -ominaisuus. Tällä tavalla ennuste ilmenee, kun tietojen siirto-palvelu lukee tietojen lähteen tiedot-kaupasta, jossa on paljon tehokkaampaa.

## <a name="considerations-for-data-management-gateway"></a>Data Management Gatewayn huomioon otettavia seikkoja
Katso yhdyskäytävän asetukset suosituksia [Data Management Gateway Huomioitavaa](data-factory-move-data-between-onprem-and-cloud.md#Considerations-for-using-Data-Management-Gateway).

**Yhdyskäytävän tietokoneen ympäristössä**: suosittelemme, että käytät isäntään Data Management Gateway Oma tietokone. Työkalujen, kuten PerfMon tutkia suorittimen ja muistin kaistanleveyden käytön aikana kopiointi yhdyskäytävän käyttämääsi laitteeseen. Jos suorittimen ja muistin kaistanleveys on pullonkaula, siirry tehokkaampia tietokoneeseen.

**Samanaikainen kopioi tehtävän suoritetaan**: Data Management Gateway yksittäisen esiintymän, voit käyttää useita kopioi tehtävä on suoritettu samaan aikaan tai samanaikaisesti. Samanaikainen töiden enimmäismäärä lasketaan yhdyskäytävän tietokoneen laitekokoonpanon perusteella. Kopion töitä on jonossa, kunnes ne kerätään yhdyskäytävän tai kunnes toinen työ aikakatkaistaan. Resurssiristiriita yhdyskäytävän tietokoneen välttämiseksi vaiheen kopioi tehtävän aikataulun määrää jonossa olevien töiden kopioi kerrallaan tai harkitse jakaminen Lataa useita yhdyskäytävän tietokoneisiin.


## <a name="other-considerations"></a>Muita huomioon otettavia seikkoja
Jos haluat kopioida tiedot on suuri, voit säätää organisaatiosi liiketoimintalogiikan edelleen osioon slicing järjestelmä käyttäminen tietojen Factory tiedot. Ajoita kopioi tehtävän suorittamiseen useammin tiedot koon pienentämiseksi Suorita kopioi jokaiselle tehtävälle.

Ole varovainen tietojoukot ja kopioi toimet, jotka Data Factory edellyttävät Connectorin saman tietovaraston samanaikaisesti. Monta samanaikainen kopioi työt ehkä rajoita tietosäilö ja johtaa eivät toimi oikein suorituskyvyn, Kopioi työn sisäinen uudelleenyritykset, ja joissakin tapauksissa suoritusvirheistä.

## <a name="sample-scenario-copy-from-an-on-premises-sql-server-to-blob-storage"></a>Esimerkkitilanne: kopioi paikallisen SQL Serveristä Blob-objektien tallennustilaan
**Skenaario**: putkijohto valmista tietojen kopioiminen paikallisen SQL Serveriä Blob-objektien tallennustilaan CSV-muodossa. Kopioi projekti nopeuttaa CSV-tiedostoja kannattaa pakata bzip2 muotoon.

**Numero- ja analyysi**: kopioi tehtävän siirtonopeuden on pienempi kuin 2 MBps, joka on heikentynyt suorituskyky ensisijainen.

**Suorituskyvyn analysointi ja säätäminen**: vianmääritys suorituskykyyn liittyvä ongelma katsotaan miten tiedot käsitellään ja siirtää.

1.  **Tietojen**: yhdyskäytävän avautuu SQL Server-yhteyden ja lähettää kyselyn. SQL Server vastaa lähettämällä tietovirta yhdyskäytävän intranetin kautta.
2.  **Serialize ja Tiivistä tiedot**: yhdyskäytävän serializes tietovirta CSV-muotoon ja pakkaa bzip2 stream tiedot.
3.  **Tietojen kirjoittaminen**: Gateway Lataa bzip2 virta Blob-objektien tallennustilaan Internetin kautta.

Kuten näet, tiedot on parhaillaan käsitellään ja siirtää streaming peräkkäisiä tavalla: SQL Server > Lähiverkon > yhdyskäytävän > WAN > Blob storage. **Yleistä suorituskykyä on gated pienin siirtonopeuden kautta putkijohto mukaan**.

![Tiedonkulun](./media/data-factory-copy-activity-performance/case-study-pic-1.png)

Suorituskyvyn pullonkaula voi aiheuttaa vähintään yksi seuraavat seikat:

-   **Tietolähteen**: SQL Server itse on pieni siirtonopeuden paksu Lataa vuoksi.
-   **Data Management Gatewayn**:
    -   **Lähiverkon**: yhdyskäytävän sijaitsee huomattavasti SQL Server-tietokoneeseen, ja se on hidas yhteys.
    -   **Yhdyskäytävän**: yhdyskäytävä on muutettu kuormituksen rajoituksia voit suorittaa seuraavat toimet:
        -   **Sarjatoiminto**: sarjoitettaessa CSV-tiedoston tietojen virta on hidas siirtonopeuden.
        -   **Pakkaamisen**: valitsit hidas pakkauksenhallintaa (esimerkiksi bzip2, joka on 2,8 MBps Core i7 kanssa).
    -   **WAN**: yrityksen verkossa ja Azure palvelujen välillä kaistanleveys on pieni (esimerkiksi T1 = 1,544 kbps; T2 = 6,312 kbps).
-   **Allas**: Blob-objektien tallennustilaan on pieni siirtonopeuden. (Tämä skenaario on epätodennäköistä, koska sen SLA takaa vähintään 60 Mbps.)

Tässä tapauksessa bzip2 tietojen pakkaus voi hidastaa koko putkijohto. Siirtyminen gzip pakkauksenhallintaa voi helpottaa tämän pullonkaula.


## <a name="sample-scenarios-use-parallel-copy"></a>Esimerkki skenaariot: rinnakkainen kopioimalla  

**Skenaario I:** Kopioi paikallisen tiedostojärjestelmän 1 000 1 Megatavun tiedostot Blob-objektien tallennustilaan.

**Analysointia ja suorituskyvyn säätö**: esimerkki, jos olet asentanut yhdyskäytävän Ristinmuotoinen core koneessa Data Factory käyttää 16 rinnakkain kopiota tiedostot siirretään tiedostojärjestelmän Blob-objektien tallennustilaan samanaikaisesti. Rinnakkaisia suorittaminen olisi johtaa hyvin siirtonopeuden. Voi erikseen määrittää rinnakkain kopioiden määrä. Kun kopioit useita pieniä tiedostoja, rinnakkain kopiot auttaa huomattavasti siirtonopeuden käyttämällä resurssien entistä tehokkaammin.

![Käyttötilanne 1](./media/data-factory-copy-activity-performance/scenario-1.png)

**Skenaario II**: 20 BLOB 500 megatavua kopioiminen tietojen järvi kaupan Analytics-Blob-säiliö ja paranna suorituskykyä.

**Analysointia ja suorituskyvyn säätö**: Tässä skenaariossa Data Factory kopioi tiedot Blob-objektien tallennustilaan järvi tietovaraston single-kopio (**parallelCopies** arvoksi 1) ja yksittäisen cloud tietojen siirtämistä yksiköt avulla. Huomaat siirtonopeuden lähellä, jotka esitetään [Suorituskyky viittaus-osan](#performance-reference).   

![Käyttötilanne 2](./media/data-factory-copy-activity-performance/scenario-2.png)

**Skenaario III**: yksittäisen tiedostokoko on suurempi kuin MBs kymmeniä ja kokonaismäärä on suuri.

**Analysointia ja suorituskyvyn ottaminen**: lisääntyvien **parallelCopies** ei tarkoita suorituskyvyn parantamiseksi Kopioi yhden cloud dmu-Tässä resurssien rajoitusten vuoksi. Määritä sen sijaan Lisää cloud DMUs saat lisää resursseja, joiden suorittaa tietojen siirto. Määritä **parallelCopies** -ominaisuuden arvoa. Tietoja Factory käsittelee rinnakkaisuus puolestasi. Tässä tapauksessa Jos määrität **cloudDataMovementUnits** 4, siirtonopeuden on noin neljä kertaa tapahtuu.

![Tapaus 3](./media/data-factory-copy-activity-performance/scenario-3.png)

## <a name="reference"></a>Viittaus
Seuraavassa on suorituskyvyn seurantaa ja viittaukset parantaminen joitakin tuetut tietoja tallennetuista tiedoista:

- Azure Storage (mukaan lukien Blob-säiliö ja taulukkotallennus): [Azuren tallennustilaan skaalattavuus kohteet](../storage/storage-scalability-targets.md) ja [Azure-tallennustilan suorituskyky ja skaalattavuus tarkistusluettelo](../storage//storage-performance-checklist.md)
- Azure SQL-tietokantaan: Voit [näytössä suorituskyky](../sql-database/sql-database-service-tiers.md#monitoring-performance) ja Tarkista tietokannan tapahtuman yksikkö (DTU) prosentteina
- Azure SQL-tietovarasto: Sen ominaisuuksien mitataan data warehouse mittayksikkö (DWUs); Katso [hallinta Laske virtaa Azure SQL-tietovarasto (yleiskatsaus)](../sql-data-warehouse/sql-data-warehouse-manage-compute-overview.md)
- Azure DocumentDB: [DocumentDB tasojen suorituskyky](../documentdb/documentdb-performance-levels.md)
- Paikallisen SQL Server: [näyttö ja hienosäätää suorituskyky](https://msdn.microsoft.com/library/ms189081.aspx)
- Paikallisen tiedoston palvelin: [suorituskyvyn parantaminen tiedostopalvelimet](https://msdn.microsoft.com/library/dn567661.aspx)
