<properties
    pageTitle="Ryhmän tietojen tiede prosessin käytössä: Käytä Hadoop klusterit | Microsoft Azure"
    description="Ryhmän tietojen tiede prosessia käyttämällä lopusta loppuun-skenaarion, joissa HDInsight Hadoop-klusterin voivat laatia ja kehittää käyttämällä yleisesti saatavilla tietojoukko mallin."
    services="machine-learning,hdinsight"
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
    ms.date="09/19/2016"
    ms.author="hangzh;bradsev" />


# <a name="the-team-data-science-process-in-action-using-hdinsight-hadoop-clusters"></a>Ryhmän tietojen tiede prosessin käytössä: HDInsight Hadoop klustereiden käyttäminen

Tätä vaiheittaista Käytämme [Ryhmän tietojen tiede prosessi (TDSP)](data-science-process-overview.md) avulla luodun [klusterin Azure Hdinsightiin Hadoop](https://azure.microsoft.com/services/hdinsight/) tallentamiseen, tutki ja ominaisuus lähdekoodiksi tietojen yleisesti saatavilla [Kokousesitelmän taksin Trips](http://www.andresmh.com/nyctaxitrips/) tietojoukko ja alas otoksen tiedot lopusta loppuun-skenaariossa. Mallien tiedot on luotu Azure koneen Learning käsittelemään binaarinen ja multiclass luokittelu ja regression ennakoivan tehtävät.

Katso vaiheittainen, jossa näkyy käsittelemisestä suurempi (1 teratavun) tietojoukkoa vastaava skenaario HDInsight Hadoop klustereiden käyttäminen tietojen käsittely- [Työryhmän tiedot tiede prosessi - käyttämällä Azure Hdinsightiin Hadoop klustereiden 1 TT dataset](machine-learning-data-science-process-hive-criteo-walkthrough.md).

On myös mahdollista suorittaa tehtäviä esitetään käyttämällä 1 TT tietojoukko ongelmatilanteita IPython muistikirjan avulla. Käyttäjät, jotka haluat Kokeile tätä tapaa kysyttävä aiheen [Criteo ongelmatilanteita rakenne ODBC-yhteyden avulla](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-hive-walkthrough-criteo.ipynb) .


## <a name="dataset"></a>Kokousesitelmän taksin Trips tietojoukko kuvaus

Kokousesitelmän taksin työmatkan tiedot on noin 20 gt: n pakattu pilkuilla erotetut arvot (CSV)-tiedostot (noin 48 gt puretaan), 173 miljoonaa yksittäisiä trips ja kuljetusmaksut maksettu kunkin työmatkan. Matkan kunkin tietueen sisältää nouto ja latauskirjaston sijainti ja aika, anonymized hakkeroida (ohjaimen) käyttöoikeuden numero ja medallion (taksin on yksilöllinen tunnus) numero. Tietoja kattaa kaikki trips vuoden 2013 ja annetaan seuraavat kaksi tietojoukkoja jokaiselle kuukaudelle:

1. 'Trip_data' CSV-mallitiedostot sisältävät työmatkan tiedot, kuten matkustajien, nouto ja dropoff pistettä, työmatkan keston ja työmatkan pituus määrän. Tässä on muutama Esimerkki tietue:

        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count,trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868

2. 'Trip_fare' CSV-mallitiedostot sisältävät tietoja maksettu kunkin työmatkan, kuten maksutyyppi, maksun summa, Lisämaksu ja verojen, vihjeet ja tietullit, maksun ja yhteensä maksetun summan. Tässä on muutama Esimerkki tietue:

        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

Yksilöivä liittymään työmatkan\_tiedot ja työmatkan\_maksun koostuu kentät: medallion, hakkeroida\_lisenssin ja nouto\_datetime.

Jos haluat saada kaikki tiedot liittyvät erityisesti matkan, on riittävä osallistuminen näppäimistä: "medallion", "yhteyden Bluetooth\_käyttöoikeuden" ja "nouto\_datetime".

Olemme kuvataan joitakin lisätietoja tiedot tallennetaan ne rakenteen taulukoiksi pian.

## <a name="mltasks"></a>Esimerkkejä tekstinsyöttö tehtävät
Kun lähellä määritettäessä, millaisia ennusteiden, josta haluat tehdä tietojen perusteella sen avulla voidaan selkeyttää tehtävät, jotka haluat sisällyttää prosessin.
Seuraavassa on kolme esimerkkiä tekstinsyöttö ongelmia, jotka on osoite tätä vaiheittaista, jonka muodostamisessa perustuu *Vihje\_summa*:

1. **Binaarinen luokitus**: ennustaa riippumatta siitä, onko Vihje maksettuun matkan, eli *Vihje\_summa* , joka on suurempi kuin 0 euroa on positiivinen esimerkissä, mutta *Vihje\_summa* $ 0 on negatiivinen esimerkki.

        Class 0 : tip_amount = $0
        Class 1 : tip_amount > $0

2. **Multiclass luokitus**: ennustaa Vihje summat maksettu matkan solualue. Jaetaan *Vihje\_summa* viisi palkkien tai luokat:

        Class 0 : tip_amount = $0
        Class 1 : tip_amount > $0 and tip_amount <= $5
        Class 2 : tip_amount > $5 and tip_amount <= $10
        Class 3 : tip_amount > $10 and tip_amount <= $20
        Class 4 : tip_amount > $20

3. **Regressiosuoran tehtävän**: ennustaa matkan maksettu Vihje määrää.  


## <a name="setup"></a>Määritä HDInsight Hadoop-klusterin kehittyneen analyysin varten

>[AZURE.NOTE] Tämä on yleensä **järjestelmänvalvoja** -tehtävä.

Voit määrittää Azure käyttöympäristön kehittyneen analyysin, joka on suojattu HDInsight-klusterin kolme vaihetta:

1. [Luo tili tallennustilan](../storage/storage-create-storage-account.md): tallennustilan tilin käytetään tietojen tallentamiseen Azure-Blob-objektien tallennustilaan. Käyttää HDInsight klustereissa tiedot sijaitsevat myös tässä.

2. [Mukauta Azure Hdinsightiin Hadoop klustereiden Advanced Analytics-prosessi-ja tekniikka](machine-learning-data-science-customize-hadoop-cluster.md). Tämä vaihe luo klusteri ja 64-bittinen Anaconda Python 2.7 asennettuihin kaikki solmut Azure Hdinsightiin Hadoop. Sisältää kaksi tärkeää vaihetta muistaa mukauttamisesta HDInsight-klusterin.

    * Muista HDInsight-klusterin vaiheen 1 luomisen sen luomisen tallennustilan asiakkaan linkittäminen. Tallennustilan tilin käytetään käyttämään tietoja, jotka käsitellään klusterin kuluessa.

    * Klusterin luomisen jälkeen Etäkäyttötoiminnon ottaminen pään solmun klusterin. Siirry **määritys** -välilehti ja valitse **Ota käyttöön Remote**. Tässä vaiheessa määrittää remote kirjautuminen käytettäviä käyttäjätietoja.

3. [Luo Azure Konepohjaisten Oppimistekniikoiden työtila](machine-learning-create-workspace.md): Tämä Azure Konepohjaisten Oppimistekniikoiden työtilan käytetään tietokoneen learning mallien luomiseen. Tehtävä on osoitettu, kun olet suorittanut aloitustiedot tarkasteluun ja esimerkkejä käyttämällä HDInsight-klusterin alas.

## <a name="getdata"></a>Julkisen tietolähteen tietojen noutaminen

>[AZURE.NOTE] Tämä on yleensä **järjestelmänvalvoja** -tehtävä.

Pääset [Kokousesitelmän taksin Trips](http://www.andresmh.com/nyctaxitrips/) tietojoukko julkisen sijainnista saa käyttää jokin [Siirrä tiedot ja sieltä pois Azure-Blob-säiliö](machine-learning-data-science-move-azure-blob.md) kuvatuilla tavoilla voit kopioida tiedot tietokoneeseesi.

Tässä on kuvaus käyttämisestä AzCopy voivat lähettää toisilleen tiedostoja, jotka sisältävät tietoja. Lataa ja asenna AzCopy noudattamalla ohjeita kohdassa [Käytön aloittaminen AzCopy komentorivivalitsimet-apuohjelma](../storage/storage-use-azcopy.md).

1. Komentokehote-ikkunassa antaa seuraavat AzCopy komennot ja *< path_to_data_folder >* tilalle haluamasi kohde:


        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:https://nyctaxitrips.blob.core.windows.net/data /Dest:<path_to_data_folder> /S

2. Kun kopio on valmis, yhteensä 24 .zip-tiedostoista on valittu data-kansion. Pura samaan kansioon ladatut tiedostot paikallisesta tietokoneesta. Pane merkille kansio, jossa pakkaamattomat tiedostot sijaitsevat. Tämä kansio nimitystä *< polku\_,\_unzipped_data\_tiedostojen\> * loppuosa on.


## <a name="upload"></a>Tietojen lataaminen Azure Hdinsightiin Hadoop-klusterin oletusarvo-säilö

>[AZURE.NOTE] Tämä on yleensä **järjestelmänvalvoja** -tehtävä.

Korvaa seuraavat parametrit AzCopy seuraavat komennot ja Hadoop-klusterin luotaessa määritetyt arvot ja unzipping datatiedostot.

* ***& #60; path_to_data_folder >*** hakemiston käyttämääsi laitteeseen, jotka sisältävät wsp datatiedostot (ja polku)  
* ***& #60; tallennustilan tilin nimi Hadoop-klusterin >*** tallennustilan HDInsight-klusterin liittyvää tiliä
* ***& #60; oletusarvon säilö Hadoop-klusterin >*** yhteyttä klusterin käyttää oletusarvon säilö. Huomautus oletusarvoisesti säilön nimi on yleensä sama nimi kuin klusterin itse. Jos klusterin kutsutaan "abc123.azurehdinsight.net"-oletusarvo-säilö on abc123.
* ***& #60; tallennustilan tilin avain >*** näppäintä yhteyttä klusterin käyttämä tallennustila-tili

Komentorivi tai Windows PowerShell-ikkunan käyttämääsi laitteeseen Suorita seuraavat kaksi AzCopy-komentoa.

Tämä komento lataa työmatkan tiedot Hadoop-klusterin oletusarvon säilön ***nyctaxitripraw*** hakemistossa.

        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:<path_to_unzipped_data_files> /Dest:https://<storage account name of Hadoop cluster>.blob.core.windows.net/<default container of Hadoop cluster>/nyctaxitripraw /DestKey:<storage account key> /S /Pattern:trip_data_*.csv

Tämä komento lataa maksun tiedot Hadoop-klusterin oletusarvon säilön ***nyctaxifareraw*** hakemistossa.

        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:<path_to_unzipped_data_files> /Dest:https://<storage account name of Hadoop cluster>.blob.core.windows.net/<default container of Hadoop cluster>/nyctaxifareraw /DestKey:<storage account key> /S /Pattern:trip_fare_*.csv

Tietojen pitäisi nyt Azure-Blob-säiliö ja valmis kulutettu HDInsight-klusterin kuluessa.

## <a name="#download-hql-files"></a>Hadoop-klusterin pään solmu on kirjauduttava ja sekä valmistelemaan kokeilevaa tietojen analysointia varten

>[AZURE.NOTE] Tämä on yleensä **järjestelmänvalvoja** -tehtävä.

Voit käyttää pää solmun klusterin kokeilevaa tietojen analysointiin ja esimerkkejä tietojen, alas, noudata seuraavia ohjeita [Access Head solmu, Hadoop klusterin](machine-learning-data-science-customize-hadoop-cluster.md#headnode)kuvatut.

Tätä vaiheittaista-ensisijaisesti Käytämme kirjoitettu [rakenne](https://hive.apache.org/), SQL kaltaisessa kyselykielen kyselyjen suorittamiseen alustava tietojen explorations. Rakenne-kyselyt tallennetaan .hql tiedostoja. Olemme sitten alas mallitiedot tämän käytettäviä Azure koneen Learning etsimisen mallit.

Klusteri valmisteleminen kokeilevaa tietojen analysointi-lataamisesta .hql-tiedostot, jotka sisältävät haluamasi rakenne-komentosarjat- [github](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts) paikallisessa kansiossa (C:\temp) pään solmun. Avaa **komentokehote** -klusterin pään solmu kuluessa ja antaa seuraavista komennoista:

    set script='https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/DataScienceProcess/DataScienceScripts/Download_DataScience_Scripts.ps1'

    @powershell -NoProfile -ExecutionPolicy unrestricted -Command "iex ((new-object net.webclient).DownloadString(%script%))"

Nämä kaksi komennot Lataa kaikki tarvittavat tätä vaiheittaista paikallisen hakemiston ***C:\temp & #92;*** pään solmu .hql-tiedostot.

## <a name="#hive-db-tables"></a>Tietokannan rakenne ja kuukauden osioinut taulukoiden luominen

>[AZURE.NOTE] Tämä on yleensä **järjestelmänvalvoja** -tehtävä.

On nyt valmis luomaan meidän Kokousesitelmän taksin tietojoukko taulukoiden rakennetta.
Hadoop-klusterin pää-solmun Avaa ***Hadoop komentoriviltä*** pään solmun työpöydällä ja kirjoita rakenteen hakemiston kirjoittamalla komento

    cd %hive_home%\bin

>[AZURE.NOTE] **Suorittaa kaikki rakenne-komennot tätä vaiheittaista yllä rakenteen roskakorista / kansio tulee näkyviin. Tämä huolehtia polku ongelmat automaattisesti. Käytämme termejä "Rakenteen directory kehote", "rakenteen bin / directory kehote"- ja "Hadoop komentorivillä" tarkoittavat tämän vaiheittaisen kuvauksen suorittamiseksi.**

Rakenne-kansion seuraavasti: Anna-Hadoop komentorivin pään solmun rakenne-kyselyn luomiseen rakenteen tietokanta ja taulukot, Lähetä seuraava komento:

    hive -f "C:\temp\sample_hive_create_db_and_tables.hql"

Näin sisällöstä ***C:\temp\sample\_rakenteen\_luominen\_db\_ja\_tables.hql*** tiedosto, joka luo rakenteen tietokannan ***nyctaxidb*** ja taulukoiden ***työmatkan*** ja ***maksun***.

    create database if not exists nyctaxidb;

    create external table if not exists nyctaxidb.trip
    (
        medallion string,
        hack_license string,
        vendor_id string,
        rate_code string,
        store_and_fwd_flag string,
        pickup_datetime string,
        dropoff_datetime string,
        passenger_count int,
        trip_time_in_secs double,
        trip_distance double,
        pickup_longitude double,
        pickup_latitude double,
        dropoff_longitude double,
        dropoff_latitude double)  
    PARTITIONED BY (month int)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' lines terminated by '\n'
    STORED AS TEXTFILE LOCATION 'wasb:///nyctaxidbdata/trip' TBLPROPERTIES('skip.header.line.count'='1');

    create external table if not exists nyctaxidb.fare
    (
        medallion string,
        hack_license string,
        vendor_id string,
        pickup_datetime string,
        payment_type string,
        fare_amount double,
        surcharge double,
        mta_tax double,
        tip_amount double,
        tolls_amount double,
        total_amount double)
    PARTITIONED BY (month int)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' lines terminated by '\n'
    STORED AS TEXTFILE LOCATION 'wasb:///nyctaxidbdata/fare' TBLPROPERTIES('skip.header.line.count'='1');

Rakenne-komentosarja luo kaksi taulukkoa:

* "työmatkan"-taulukko sisältää työmatkan tiedot kunkin kulun (ohjaimen tiedot, noudon aika, työmatkan etäisyys ja kertaa)
* "maksun"-taulukko sisältää maksun tiedot (maksun, Vihje summa, tietullien ja lisämaksut).

Jos tarvitset lisäapua näitä toimintosarjoja tai haluat tutkia vaihtoehtoinen niistä, on kohdassa [Lähetä rakenne kyselyjen suoraan-Hadoop komentorivillä ](machine-learning-data-science-move-hive-tables.md#submit).

## <a name="#load-data"></a>Tietojen lataaminen rakenteen taulukoihin osioihin

>[AZURE.NOTE] Tämä on yleensä **järjestelmänvalvoja** -tehtävä.

Kokousesitelmän taksin tietojoukko on luonnollinen jakaminen kuukausittain, jossa Käytämme käyttöön käsittelyn ja kyselyn nopeammin. Tietoja "työmatkan" ja "maksun"-rakenteen taulukot, kuukausi osioinut ladata PowerShell-komennot alla (käyttämällä **Hadoop komentoriviltä**rakenne-hakemistosta).

    for /L %i IN (1,1,12) DO (hive -hiveconf MONTH=%i -f "C:\temp\sample_hive_load_data_by_partitions.hql")

*Otoksen\_rakenteen\_ladata\_tietojen\_mukaan\_partitions.hql* -tiedosto sisältää seuraavat **ladata** komentoja.

    LOAD DATA INPATH 'wasb:///nyctaxitripraw/trip_data_${hiveconf:MONTH}.csv' INTO TABLE nyctaxidb.trip PARTITION (month=${hiveconf:MONTH});
    LOAD DATA INPATH 'wasb:///nyctaxifareraw/trip_fare_${hiveconf:MONTH}.csv' INTO TABLE nyctaxidb.fare PARTITION (month=${hiveconf:MONTH});

Huomautus Käytämme tähän tarkasteluun prosessin rakenteen kyselyjen määrä liittyä esittää vain yhden osion tai vain muutama osiot. Mutta nämä kyselyt voivat suorittaa yli koko tiedot.

### <a name="#show-db"></a>Näytä tietokantojen HDInsight Hadoop-klusterin

Näyttää HDInsight Hadoop-klusterin Hadoop komentorivi-ikkuna sisällä luotujen tietokantojen, suorita seuraava komento Hadoop komentoriviltä:

    hive -e "show databases;"

### <a name="#show-tables"></a>Näytä nyctaxidb tietokannan rakenne-taulukot

Jos haluat näyttää nyctaxidb tietokannan taulukot, suorita seuraava komento Hadoop komentorivin:

    hive -e "show tables in nyctaxidb;"

Voimme vahvistaa, että taulukot ovat osioitu lähettämällä alla oleva komento:

    hive -e "show partitions nyctaxidb.trip;"

Odotettu tulos on alla:

    month=1
    month=10
    month=11
    month=12
    month=2
    month=3
    month=4
    month=5
    month=6
    month=7
    month=8
    month=9
    Time taken: 2.075 seconds, Fetched: 12 row(s)

Voit vastaavasti varmistamme maksun taulukko on osioitu lähettämällä komennon alla:

    hive -e "show partitions nyctaxidb.fare;"

Odotettu tulos on alla:

    month=1
    month=10
    month=11
    month=12
    month=2
    month=3
    month=4
    month=5
    month=6
    month=7
    month=8
    month=9
    Time taken: 1.887 seconds, Fetched: 12 row(s)

## <a name="#explore-hive"></a>Tietojen tarkasteluun ja rakenne-ominaisuus-tekniikka

>[AZURE.NOTE] Tämä on yleensä **Tietojen Tiedemies** tehtävän.

Tietojen tarkasteluun ja ominaisuus suunnittelu ladattu rakenne-taulukoiden tietojen tehtävät voidaan toteuttaa rakenteen käyttäminen. Seuraavassa on esimerkkejä tehtäviä, että käyttöösi selkeät sisältö:

- Näytä top 10-tietueet kummassakin taulukossa.
- Tutki tietoja jaot erilaisten Windowsin muutaman kenttien.
- Tutki pituutta ja leveyttä kenttien tietojen laadun.
- Luo binaarinen ja multiclass luokittelu otsikot **Vihje\_summa**.
- Luo ominaisuuksia tietojenkäsittely suoraan työmatkan etäisyydet.

### <a name="exploration-view-the-top-10-records-in-table-trip"></a>Siihen tarkemmin: Tarkastella taulukon työmatkan top 10-tietueita

>[AZURE.NOTE] Tämä on yleensä **Tietojen Tiedemies** tehtävän.

Jos haluat nähdä, miltä tiedot näyttää, on tarkistaa 10 kunkin taulukon tietueita. Suorita seuraavat kaksi kyselyt erikseen rakenteen directory kehotteen Hadoop komentoriviltä konsolissa on rajattu hiusristikon sisään tietueet.

Saat top 10-tietueet "työmatkan"-taulukon ensimmäisen kuukauden:

    hive -e "select * from nyctaxidb.trip where month=1 limit 10;"

Ensimmäisen kuukauden saaminen taulukon ylimmät 10 tietuetta "maksun":

    hive -e "select * from nyctaxidb.fare where month=1 limit 10;"

Kannattaa usein käyttää Tallenna tietueet helposti tarkastella tiedostoon. Edellisessä kyselyssä pieni muuttamaan suorittaa näin:

    hive -e "select * from nyctaxidb.fare where month=1 limit 10;" > C:\temp\testoutput

### <a name="exploration-view-the-number-of-records-in-each-of-the-12-partitions"></a>Siihen tarkemmin: Tarkastella tietueiden määrän jokaisen 12

>[AZURE.NOTE] Tämä on yleensä **Tietojen Tiedemies** tehtävän.

Halutut seuraavasti trips määrä vaihtelee kalenterivuoden aikana. Kuukauden mukaan ryhmittely pystyy näet, miltä trips jakelu näyttää.

    hive -e "select month, count(*) from nyctaxidb.trip group by month;"

Näin us tulos:

    1       14776615
    2       13990176
    3       15749228
    4       15100468
    5       15285049
    6       14385456
    7       13823840
    8       12597109
    9       14107693
    10      15004556
    11      14388451
    12      13971118
    Time taken: 283.406 seconds, Fetched: 12 row(s)

Tässä ensimmäisessä sarakkeessa on kuukausi ja toinen on kyseisen kuukauden trips määrä.

Olemme laskea tietueiden kokonaismäärän Microsoftin työmatkan tietojoukon lähettämällä rakenteen directory komentoriville seuraava komento.

    hive -e "select count(*) from nyctaxidb.trip;"

Antaa tulokseksi:

    173179759
    Time taken: 284.017 seconds, Fetched: 1 row(s)

Kaltaisia työmatkan tietojoukon näkyvät komennot emme voi myöntää rakenteen kyselyjen rakenteen directory kehotteen maksun tietojoukon vahvistamiseen tietueiden määrän.

    hive -e "select month, count(*) from nyctaxidb.fare group by month;"

Näin us tulos:

    1       14776615
    2       13990176
    3       15749228
    4       15100468
    5       15285049
    6       14385456
    7       13823840
    8       12597109
    9       14107693
    10      15004556
    11      14388451
    12      13971118
    Time taken: 253.955 seconds, Fetched: 12 row(s)

Huomaa, että molemmat tietojoukot palautetaan täsmälleen sama määrä trips kuukaudessa. Tämä on ensimmäinen vahvistus tiedot on ladattu oikein.

Maksun tietojoukon tietueiden kokonaismäärän laskeminen voidaan toteuttaa komennolla alla rakenteen kansion seuraavasti:

    hive -e "select count(*) from nyctaxidb.fare;"

Antaa tulokseksi:

    173179759
    Time taken: 186.683 seconds, Fetched: 1 row(s)

Molempien taulukoiden kokonaismäärä on myös sama. Tämä on toinen kelpoisuuden tarkistaminen tiedot on ladattu oikein.

### <a name="exploration-trip-distribution-by-medallion"></a>Siihen tarkemmin: Matkan jakelu medallion mukaan

>[AZURE.NOTE] Tämä on yleensä **Tietojen Tiedemies** tehtävän.

Tässä esimerkissä määrittää medallion (taksin numerot) yli 100 trips tietyn aikajakson sisällä. Kyselyn hyötyä osioitua taulukon Accessista, koska se on vakautetaan osion muuttujan **kuukausi**. Kyselytulokset kirjoitetaan paikallinen tiedosto-queryoutput.tsv `C:\temp` pään solmun.

    hive -f "C:\temp\sample_hive_trip_count_by_medallion.hql" > C:\temp\queryoutput.tsv

Näin sisällön *otoksen\_rakenteen\_työmatkan\_Laske\_mukaan\_medallion.hql* tarkastus-tiedosto.

    SELECT medallion, COUNT(*) as med_count
    FROM nyctaxidb.fare
    WHERE month<=3
    GROUP BY medallion
    HAVING med_count > 100
    ORDER BY med_count desc;

Kokousesitelmän taksin tietojoukon medallion määrittää yksilölliset cab. Voit tarkastellaan mitä muutokseen ovat "varattu-pyytämällä mitkä tehty yli trips tietty määrä tietyn ajan kuluessa. Seuraavassa esimerkissä tunnistaa, joka on tehty useita lunastuspäivään trips ensimmäisen kolme kuukautta ja tallentaa kyselyn tulokset paikallisen tiedoston C:\temp\queryoutput.tsv muutokseen.

Näin sisällön *otoksen\_rakenteen\_työmatkan\_Laske\_mukaan\_medallion.hql* tarkastus-tiedosto.

    SELECT medallion, COUNT(*) as med_count
    FROM nyctaxidb.fare
    WHERE month<=3
    GROUP BY medallion
    HAVING med_count > 100
    ORDER BY med_count desc;

Rakenne-kansion seuraavasti: antamalla komennon alla:

    hive -f "C:\temp\sample_hive_trip_count_by_medallion.hql" > C:\temp\queryoutput.tsv

### <a name="exploration-trip-distribution-by-medallion-and-hacklicense"></a>Siihen tarkemmin: Medallion ja hack_license työmatkan jakauman.

>[AZURE.NOTE] Tämä on yleensä **Tietojen Tiedemies** tehtävän.

Kun tutkiminen tietojoukko usein haluat tarkastella Mää-esiintymien ryhmien arvojen määrän. Tässä osassa on esimerkki siitä, miten voit tehdä tämän muutokseen ja ohjaimet.

*Otoksen\_rakenteen\_työmatkan\_Laske\_mukaan\_medallion\_license.hql* tiedoston ryhmittelee maksun tietojoukon "medallion" ja "hack_license" ja palauttaa yhdistelmien määrät. Seuraavassa on sen sisällön.

    SELECT medallion, hack_license, COUNT(*) as trip_count
    FROM nyctaxidb.fare
    WHERE month=1
    GROUP BY medallion, hack_license
    HAVING trip_count > 100
    ORDER BY trip_count desc;

Tämä kysely palauttaa cab ja ohjainta kombinaatioiden trips laskeva määrän mukaan.

Suorita rakenteen hakemisto-kehote:

    hive -f "C:\temp\sample_hive_trip_count_by_medallion_license.hql" > C:\temp\queryoutput.tsv

Kyselytulokset kirjoitetaan paikallisen tiedoston C:\temp\queryoutput.tsv.

### <a name="exploration-assessing-data-quality-by-checking-for-invalid-longitudelatitude-records"></a>Siihen tarkemmin: Arvioidaan tietojen laatua virheellinen pituutta ja leveyttä tietueiden valitsemalla

>[AZURE.NOTE] Tämä on yleensä **Tietojen Tiedemies** tehtävän.

Kokeilevaa Tietoanalyysi yleisiä tavoitteena on rikkaruohojen, virheellinen tai virheelliset tietueet. Tässä osassa esimerkki määrittää, onko pituutta tai leveyttä kentät sisältävät arvon kaukana Kokousesitelmän alueen ulkopuolella. On todennäköistä, kuten tietueella on virheellisiä pituutta-leveyttä-arvot, haluamme poistamiseksi ne tiedot, joita käytetään mallinnus.

Näin sisällön *otoksen\_rakenteen\_laatua\_assessment.hql* tarkastus-tiedosto.

        SELECT COUNT(*) FROM nyctaxidb.trip
        WHERE month=1
        AND  (CAST(pickup_longitude AS float) NOT BETWEEN -90 AND -30
        OR    CAST(pickup_latitude AS float) NOT BETWEEN 30 AND 90
        OR    CAST(dropoff_longitude AS float) NOT BETWEEN -90 AND -30
        OR    CAST(dropoff_latitude AS float) NOT BETWEEN 30 AND 90);


Suorita rakenteen hakemisto-kehote:

    hive -S -f "C:\temp\sample_hive_quality_assessment.hql"

Tämä komento sisältyvät *-S* -argumentin estää tila-näytössä ulkoasun rakenne kartta ja vähennä työt. Tästä on hyötyä, sillä näytön tulostuksen rakenteen kyselyn tulosteen luettavuutta.

### <a name="exploration-binary-class-distributions-of-trip-tips"></a>Siihen tarkemmin: Binaarinen luokan jaot työmatkan vihjeiden

> [AZURE.NOTE] Tämä on yleensä **Tietojen Tiedemies** tehtävän.

[Esimerkkejä tekstinsyöttö tehtävät](machine-learning-data-science-process-hive-walkthrough.md#mltasks) -osassa kuvatut binaarinen luokitus ongelman on hyvä tietää, onko Vihje annettiin vai ei. Vihjeitä jakelu on binaarinen:

* Vihje annettu (luokan 1, vihje\_summa > $0)  
* ei ole Vihje (luokan 0, vihje\_summa = 0).

*Otoksen\_rakenteen\_Kallistettu\_frequencies.hql* tiedoston alla tekee tämän.

    SELECT tipped, COUNT(*) AS tip_freq
    FROM
    (
        SELECT if(tip_amount > 0, 1, 0) as tipped, tip_amount
        FROM nyctaxidb.fare
    )tc
    GROUP BY tipped;

Suorita rakenteen hakemisto-kehote:

    hive -f "C:\temp\sample_hive_tipped_frequencies.hql"


### <a name="exploration-class-distributions-in-the-multiclass-setting"></a>Siihen tarkemmin: Luokan jaot multiclass asetus

> [AZURE.NOTE] Tämä on yleensä **Tietojen Tiedemies** tehtävän.

[Esimerkkejä tekstinsyöttö tehtävät](machine-learning-data-science-process-hive-walkthrough.md#mltasks) -osassa kuvatut multiclass luokitus ongelman seuraavan tietojoukon myös kohdealueella luonnollinen luokittelu, jossa haluamme ennustaa annetaan vinkkejä määrää. Olemme määrittää Vihje alueita kyselyn palkkien avulla. Käytämme lukea luokan jaot eri Vihje alueita, *otoksen\_rakenteen\_vihje\_alueen\_frequencies.hql* tiedoston. Seuraavassa on sen sisällön.

    SELECT tip_class, COUNT(*) AS tip_freq
    FROM
    (
        SELECT if(tip_amount=0, 0,
            if(tip_amount>0 and tip_amount<=5, 1,
            if(tip_amount>5 and tip_amount<=10, 2,
            if(tip_amount>10 and tip_amount<=20, 3, 4)))) as tip_class, tip_amount
        FROM nyctaxidb.fare
    )tc
    GROUP BY tip_class;

Hadoop komentorivin konsolin varten suorittamalla seuraavan komennon:

    hive -f "C:\temp\sample_hive_tip_range_frequencies.hql"

### <a name="exploration-compute-direct-distance-between-two-longitude-latitude-locations"></a>Siihen tarkemmin: Laske pituutta leveyttä kumpaankin suoraan etäisyys

> [AZURE.NOTE] Tämä on yleensä **Tietojen Tiedemies** tehtävän.

Ottaa suoraan etäisyys mitataan pystyy Selvitä sen ja todellinen työmatkan etäisyys välinen erotus. Olemme motivoida toiminto valitsemalla että matkustaja voi olla epätodennäköistä Vihje jos ne selvittää, että ohjain on tarkoituksella tekemä ne pidempään reitin.

Todellinen työmatkan etäisyys ja kahden pituutta leveyttä pisteen ("hienoa ympyrä" etäisyys) välisen [etäisyyden Haversine](http://en.wikipedia.org/wiki/Haversine_formula) vertailu näkyviin Käytämme käytettävissä Trigonometriset funktiot sisällä rakenne, näin:

    set R=3959;
    set pi=radians(180);

    insert overwrite directory 'wasb:///queryoutputdir'

    select pickup_longitude, pickup_latitude, dropoff_longitude, dropoff_latitude, trip_distance, trip_time_in_secs,
    ${hiveconf:R}*2*2*atan((1-sqrt(1-pow(sin((dropoff_latitude-pickup_latitude)
     *${hiveconf:pi}/180/2),2)-cos(pickup_latitude*${hiveconf:pi}/180)
     *cos(dropoff_latitude*${hiveconf:pi}/180)*pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2)))
     /sqrt(pow(sin((dropoff_latitude-pickup_latitude)*${hiveconf:pi}/180/2),2)
     +cos(pickup_latitude*${hiveconf:pi}/180)*cos(dropoff_latitude*${hiveconf:pi}/180)*
     pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2))) as direct_distance
    from nyctaxidb.trip
    where month=1
    and pickup_longitude between -90 and -30
    and pickup_latitude between 30 and 90
    and dropoff_longitude between -90 and -30
    and dropoff_latitude between 30 and 90;

Yllä kyselyssä R on Mailia maapallon sädettä ja pii radiaania muunnetaan. Huomaa, että pituutta leveyttä pisteet ovat "suodatettu" Poista huomattavasti Kokousesitelmän alueella olevat arvot.

Tässä tapauksessa emme kirjoittaa Microsoftin tulokset kansio nimeltä "queryoutputdir". Alla olevassa komennot luo ensin tulosteen tähän kansioon ja suorittaa sitten rakenne-komento.

Suorita rakenteen hakemisto-kehote:

    hdfs dfs -mkdir wasb:///queryoutputdir

    hive -f "C:\temp\sample_hive_trip_direct_distance.hql"


Kyselytulokset kirjoitetaan 9 Azure BLOB ***queryoutputdir/000000\_0*** , ***queryoutputdir/000008\_0*** Hadoop-klusterin oletusarvo-säilössä.

Nähdäksesi yksittäisiä BLOB koon on suorittamalla seuraavan komennon rakenne kansion seuraavasti:

    hdfs dfs -ls wasb:///queryoutputdir

Jos haluat nähdä tietyn tiedoston sisällön, sano 000000\_0, Käytämme käyttäjän Hadoop `copyToLocal` komento, jota ei.

    hdfs dfs -copyToLocal wasb:///queryoutputdir/000000_0 C:\temp\tempfile

> [AZURE.WARNING] `copyToLocal`voi olla hyvin hidasta suuria tiedostoja ja ei suositella käytettäväksi heidän kanssaan.  

Avaimen etuna on nämä tiedot sijaitsevat Azure-blob on Azure koneen Learning [Tietojen] tietoja voi tutustumme[ import-data] moduuli.


## <a name="#downsample"></a>Esimerkki tietoja ja muodosta mallien Azure koneen Learning alaspäin

> [AZURE.NOTE] Tämä on yleensä **Tietojen Tiedemies** tehtävän.

Kokeilevaa tietojen analysointi-vaiheen jälkeen on nyt alas otoksen tietojen etsimisen Azure koneen Learning mallit. Tässä osassa on Näytä käyttämisestä kyselyssä rakenne alas otoksen tiedot, joita käytetään Valitse [Tietojen tuominen] [ import-data] Azure koneen Learning moduuli.

### <a name="down-sampling-the-data"></a>Alaspäin näyte tiedot

Seuraavassa on kaksi vaihetta. Ensin on liittyä **nyctaxidb.trip** ja **nyctaxidb.fare** taulukot näppäimistä, jotka ovat kaikki tietueet: "medallion", "yhteyden Bluetooth\_käyttöoikeutta"- ja "nouto\_datetime". Olemme luo sitten binaarinen luokitus otsikko **Kallistettu** ja usean luokan luokitus selitteen **Vihje\_luokan**.

Voit käyttää olevasta luettelosta näyte tietoja suoraan [Tietojen] [ import-data] moduulissa Azure koneen Learning on tarpeen sisäinen rakennetaulukko yllä kyselyn tulosten tallentaminen. Valitse loppuosa emme sisäinen rakenne-taulukon luominen ja sen sisältö liitetyn ja alaspäin näytteeksi tietojen täyttäminen.

Kyselyä sovelletaan vakio rakenteen funktioita suoraan tunnin päivän, viikon vuoden viikonpäivä (maanantai on 1 ja 7 on sunnuntai) "nouto\_datetime-kentän ja nouto- ja dropoff sijainnit suoraan etäisyys. Käyttäjät voivat viitata [LanguageManual UDF](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF) näiden funktioiden kattavaan luetteloon.

Kyselyn sitten alas esimerkit tiedot niin, että kyselytulokset mahtuvien Azure koneen Learning Studio. Vain noin 1 % alkuperäisen tietojoukko on tuotu Studio.

Seuraavassa on sisällön *otoksen\_rakenteen\_valmisteleminen\_varten\_aml\_full.hql* tiedosto, jonka tiedot valmistellaan Azure koneen Learning luomisesta mallin.

        set R = 3959;
        set pi=radians(180);

        create table if not exists nyctaxidb.nyctaxi_downsampled_dataset (

        medallion string,
        hack_license string,
        vendor_id string,
        rate_code string,
        store_and_fwd_flag string,
        pickup_datetime string,
        dropoff_datetime string,
        pickup_hour string,
        pickup_week string,
        weekday string,
        passenger_count int,
        trip_time_in_secs double,
        trip_distance double,
        pickup_longitude double,
        pickup_latitude double,
        dropoff_longitude double,
        dropoff_latitude double,
        direct_distance double,
        payment_type string,
        fare_amount double,
        surcharge double,
        mta_tax double,
        tip_amount double,
        tolls_amount double,
        total_amount double,
        tipped string,
        tip_class string
        )
        row format delimited fields terminated by ','
        lines terminated by '\n'
        stored as textfile;

        --- now insert contents of the join into the above internal table

        insert overwrite table nyctaxidb.nyctaxi_downsampled_dataset
        select
        t.medallion,
        t.hack_license,
        t.vendor_id,
        t.rate_code,
        t.store_and_fwd_flag,
        t.pickup_datetime,
        t.dropoff_datetime,
        hour(t.pickup_datetime) as pickup_hour,
        weekofyear(t.pickup_datetime) as pickup_week,
        from_unixtime(unix_timestamp(t.pickup_datetime, 'yyyy-MM-dd HH:mm:ss'),'u') as weekday,
        t.passenger_count,
        t.trip_time_in_secs,
        t.trip_distance,
        t.pickup_longitude,
        t.pickup_latitude,
        t.dropoff_longitude,
        t.dropoff_latitude,
        t.direct_distance,
        f.payment_type,
        f.fare_amount,
        f.surcharge,
        f.mta_tax,
        f.tip_amount,
        f.tolls_amount,
        f.total_amount,
        if(tip_amount>0,1,0) as tipped,
        if(tip_amount=0,0,
        if(tip_amount>0 and tip_amount<=5,1,
        if(tip_amount>5 and tip_amount<=10,2,
        if(tip_amount>10 and tip_amount<=20,3,4)))) as tip_class

        from
        (
        select
        medallion,
        hack_license,
        vendor_id,
        rate_code,
        store_and_fwd_flag,
        pickup_datetime,
        dropoff_datetime,
        passenger_count,
        trip_time_in_secs,
        trip_distance,
        pickup_longitude,
        pickup_latitude,
        dropoff_longitude,
        dropoff_latitude,
        ${hiveconf:R}*2*2*atan((1-sqrt(1-pow(sin((dropoff_latitude-pickup_latitude)
        *${hiveconf:pi}/180/2),2)-cos(pickup_latitude*${hiveconf:pi}/180)
        *cos(dropoff_latitude*${hiveconf:pi}/180)*pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2)))
        /sqrt(pow(sin((dropoff_latitude-pickup_latitude)*${hiveconf:pi}/180/2),2)
        +cos(pickup_latitude*${hiveconf:pi}/180)*cos(dropoff_latitude*${hiveconf:pi}/180)*pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2))) as direct_distance,
        rand() as sample_key

        from nyctaxidb.trip
        where pickup_latitude between 30 and 90
            and pickup_longitude between -90 and -30
            and dropoff_latitude between 30 and 90
            and dropoff_longitude between -90 and -30
        )t
        join
        (
        select
        medallion,
        hack_license,
        vendor_id,
        pickup_datetime,
        payment_type,
        fare_amount,
        surcharge,
        mta_tax,
        tip_amount,
        tolls_amount,
        total_amount
        from nyctaxidb.fare
        )f
        on t.medallion=f.medallion and t.hack_license=f.hack_license and t.pickup_datetime=f.pickup_datetime
        where t.sample_key<=0.01

Voit suorittaa tämän kyselyn rakenne directory kehote:

    hive -f "C:\temp\sample_hive_prepare_for_aml_full.hql"

Sisäinen taulukko "nyctaxidb.nyctaxi_downsampled_dataset" jota voi käyttää [Tuontitiedot] on nyt[ import-data] Azure koneen Learning moduuli. Lisäksi Microsoft voi käyttää tätä tietojoukko etsimisen koneen Learning mallit.  

### <a name="use-the-import-data-module-in-azure-machine-learning-to-access-the-down-sampled-data"></a>Tietojen tuominen-moduulin käyttäminen Azure koneen Learning alaspäin näytteeksi tietojen käyttämistä varten

Kuin edellytyksistä varmenteiden rakenteen kyselyjen [Tietojen] [ import-data] moduulissa Azure koneen opiskelun annettava access Azure Konepohjaisten Oppimistekniikoiden työtilan ja käyttöoikeus klusterin ja sen liittyvät tallennustilan tilin tunnistetiedot.

[Tuontitiedot] koskevat tiedot[ import-data] moduuli ja kirjoita parametrit:

**HCatalog palvelimen URI**: Jos klusterinimi on abc123 ja sitten tämä on vain: https://abc123.azurehdinsight.net

**Hadoop käyttäjätilisi nimi** : käyttäjänimi valinnut klusterin (**ei** etäkäyttöpalvelimen käyttäjänimi)

**Hadoop huolto tilin salasana** : valittu klusterin (**ei** etäkäyttöpalvelimen salasana) salasana

**Siirtää tietoja sijainti** : Tämä on päättänyt olla Azure.

**Azure-tallennustilan tilin nimi** : klusterin liittyvän oletusarvoisen-tallennustilan tilin nimi.

**Azure säilön nimi** : Tämä on klusterin säilö oletusnimi ja on yleensä sama kuin klusterinimeä. Klusterin nimeltä "abc123" Tämä on vain abc123.

> [AZURE.IMPORTANT] **Taulukko on haluat kyselyn [Tietojen] [ import-data] Azure koneen Learning moduulissa on oltava sisäinen taulukko.** Vihje Jos taulukon T tietokannan D.db on sisäinen taulukko määrittämiseksi on seuraavasti.

Rakenne-kansion seuraavasti: myöntää komento:

    hdfs dfs -ls wasb:///D.db/T

Jos taulukko on sisäinen taulukko ja se lisätään, sen sisältö on Näytä tähän. Voit selvittää, onko taulukon sisäinen taulukko on Explorerilla Azure-tallennustilan. Sen avulla voit siirtyä klusterin säilö oletusnimi ja Suodata taulukon nimi. Jos taulukon ja sen sisällön näy, tämä tarkoittaa, että se on sisäinen taulukko.

Seuraavassa on rakenne-kysely ja [Tuontitiedot] [ import-data] moduuli:

![](./media/machine-learning-data-science-process-hive-walkthrough/1eTYf52.png)

Microsoftin alas näytteeksi tiedot sijaitsevat oletusarvo-säilössä, koska Azure koneen Learning rakenteen kysely on kuvattu yksinkertainen, eikä on vain "Valitse *-nyctaxidb.nyctaxi\_erotuskykyä vähennetään\_tiedot".

Tietojoukon nyt voidaan käyttää lähtökohtana etsimisen koneen Learning mallit.

### <a name="mlmodel"></a>Luoda malleja Azure koneen oppiminen

Emme nyt voit jatkaa mallin luominen ja mallin käyttöönottoa [Azure koneen Learning](https://studio.azureml.net). Tiedot ovat valmiita us käyttäminen osoitteen edellä tekstinsyöttö ongelmat:

**1. binaarinen luokitus**: ennustaa onko Vihje maksettuun matkan.

**Paremmin katselemalla käytetään:** Kahden luokan logistista regressiota

a. Tämän ongelman Microsoftin kohde (tai luokan) otsikko on "Kallistettu". Tutustu alkuperäisen luettelon näyte tietojoukko on muutama saraketta, jotka ovat kohde vuodot luokitus-kokeen osalta. Erityisesti: vihje\_luokan, vihje\_summa ja summa\_summa paljastaa tietoja kohteen otsikkoa, joka ei ole käytettävissä osoitteessa testaaminen aika. Näiden sarakkeiden poistaminen on vastikkeen, [Valitse sarakkeet-tietojoukko] avulla[ select-columns] moduuli.

Alla tilannevedoksen näyttää Microsoftin kokeen ennustaa riippumatta siitä, onko Vihje maksettuun tietyn matkan.

![](./media/machine-learning-data-science-process-hive-walkthrough/QGxRz5A.png)

b. Tämän kokeen Microsoftin kohde otsikko jaot on noin 1:1.

Alla olevassa tilannevedoksen esittää Vihje jakautumisen luokan tarrojen binaarinen luokitus-ongelma.

![](./media/machine-learning-data-science-process-hive-walkthrough/9mM4jlD.png)

Olemme hankkia tuloksena 0.987 AUC alla olevassa kuvassa osoitetulla tavalla.

![](./media/machine-learning-data-science-process-hive-walkthrough/8JDT0F8.png)

**2. multiclass luokitus**: ennustaa solualue, Vihje summat maksettu työmatkan aiemmin määritettyä luokkien avulla.

**Paremmin katselemalla käytetään:** Multiclass logistista regressiota

a. Tämä ongelma Microsoftin kohde (tai luokan) selite on "Vihje\_luokan" joka voi kestää jonkin viisi arvot (0,1,2,3,4). Binaarinen luokitus tapaukselle, kuten on muutama sarakkeet, jotka ovat kohde vuodot tämän kokeen osalta. Erityisesti: Kallistettu, vihje\_, kokonaissumma\_summa paljastaa tietoja kohteen otsikkoa, joka ei ole käytettävissä osoitteessa testaaminen aika. Olemme Poista näiden sarakkeiden avulla, [Valitse sarakkeet-tietojoukko] [ select-columns] moduuli.

Näyttökuvan alla näkyy Microsoftin kokeen ennustaa mitä roskakorissa olevat vihje on todennäköisesti kuuluvat (luokan 0: Vihje = $0, luokan 1: > $0 ja Vihje Vihje < = $5, luokan 2: > $5 ja Vihje Vihje < = 10, luokan 3: > $10 ja Vihje Vihje < = $20, luokan 4: Vihje > $20)

![](./media/machine-learning-data-science-process-hive-walkthrough/5ztv0n0.png)

On nyt näkyä Microsoftin todellinen testi luokan jakauman näyttää tältä. Näemme, että luokan 0 ja 1 luokassa suojaukselle, muut luokat on harvinaisissa.

![](./media/machine-learning-data-science-process-hive-walkthrough/Vy1FUKa.png)

b. Saat tämän kokeen Käytämme sekaannusta matriisin Microsoftin tekstinsyöttö tarkkuudet tarkasteltavan. Tämä on alla.

![](./media/machine-learning-data-science-process-hive-walkthrough/cxFmErM.png)

Huomaa, että haittaohjelmien luokat-luokan Microsoftin tarkkuudet ollessa aivan hyvä malli ei tehdä "learning" hyvin harvoissa luokat.


**3. regression tehtävän**: ennustaa Vihje matkan maksettu määrä.

**Paremmin katselemalla käytetään:** Tehosti Päätöspuun

a. Tämä ongelma Microsoftin kohde (tai luokan) selite on "Vihje\_summa". Microsoftin kohde vuodot eivät tässä tapauksessa: Kallistettu, vihje\_luokan, yhteensä\_summa; Nämä muuttujat paljastaa tietoja Vihje summa, joka on yleensä ei ole käytettävissä osoitteessa testaaminen aika. Olemme Poista näiden sarakkeiden avulla, [Valitse sarakkeet-tietojoukko] [ select-columns] moduuli.

Tilannevedoksen belows näyttää Microsoftin kokeen ennustaa annetun Vihje määrää.

![](./media/machine-learning-data-science-process-hive-walkthrough/11TZWgV.png)

b. Regressiosuoran ongelmia emme mittaa Microsoftin tekstinsyöttö tarkkuudet katsomalla neliön virhe ennusteiden, korrelaatiokerroin ja tykkää. Olemme Näytä alla.

![](./media/machine-learning-data-science-process-hive-walkthrough/Jat9mrz.png)

Näemme, että tietoja korrelaatiokerroin on 0.709, mikä noin 71 % varianssi on selitetty Microsoftin mallin kertoimet mukaan.

> [AZURE.IMPORTANT] Jos haluat lisätietoja Azure koneen oppiminen ja miten sitä käytetään, tutustu [Mikä on tietokoneen Learning?](machine-learning-what-is-machine-learning.md). On erittäin hyödyllinen resurssi koneen Learning kokeet Azure koneen Learning useita käsitteleminen on [Cortana Liiketoimintatieto-valikoimassa](https://gallery.cortanaintelligence.com/). Valikoima kattaa asteikkoon kokeet, ja sisältää perusteellisesti johdannon Azure koneen Learning ominaisuudet-alueelle.

## <a name="license-information"></a>Käyttöoikeustiedot

Esimerkki tätä vaiheittaista ja sen mukana komentosarjoja on jaettu Microsoft MIT-käyttöoikeudet-kohdassa. Tarkista LICENSE.txt-tiedoston sample code GitHub lisätietoja hakemistossa.

## <a name="references"></a>Viittaukset

• [Andrés Monroy Kokousesitelmän taksin Trips lataussivulle](http://www.andresmh.com/nyctaxitrips/)  
• [FOILing Kokousesitelmän 's taksin työmatkan tietojen Chris Whong](http://chriswhong.com/open-data/foil_nyc_taxi/)   
• [Kokousesitelmän taksin ja Umpiauto eli Myyntipalkkio oheis- ja tilastot](https://www1.nyc.gov/html/tlc/html/about/statistics.shtml)


[2]: ./media/machine-learning-data-science-process-hive-walkthrough/output-hive-results-3.png
[11]: ./media/machine-learning-data-science-process-hive-walkthrough/hive-reader-properties.png
[12]: ./media/machine-learning-data-science-process-hive-walkthrough/binary-classification-training.png
[13]: ./media/machine-learning-data-science-process-hive-walkthrough/create-scoring-experiment.png
[14]: ./media/machine-learning-data-science-process-hive-walkthrough/binary-classification-scoring.png
[15]: ./media/machine-learning-data-science-process-hive-walkthrough/amlreader.png

<!-- Module References -->
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
