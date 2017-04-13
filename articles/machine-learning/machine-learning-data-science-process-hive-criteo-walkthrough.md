<properties
    pageTitle="Ryhmän tietojen tiede prosessin käytössä: käyttämällä HDInsight Hadoop klusterit 1 TT Criteo dataset | Microsoft Azure"
    description="Ryhmän tietojen tiede prosessia käyttämällä lopusta loppuun-skenaarion, joissa HDInsight Hadoop-klusterin voivat laatia ja kehittää mallin käyttäminen (1 TT) yleiseen käyttöön suuri tietojoukko"
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
    ms.date="09/13/2016"
    ms.author="bradsev" />

# <a name="the-team-data-science-process-in-action---using-azure-hdinsight-hadoop-clusters-on-a-1-tb-dataset"></a>Toiminto - käyttämällä Azure Hdinsightiin Hadoop klustereiden 1 TT dataset-ryhmän tietojen tiede-prosessi

Tätä vaiheittaista on osoitettava käyttäminen lopusta loppuun-skenaario kanssa luodun [Azure Hdinsightiin Hadoop-klusterin](https://azure.microsoft.com/services/hdinsight/) ryhmän tietojen tiede prosessin tallentamiseen, tutustu ominaisuus lähdekoodiksi ja alas-mallitiedot olevista julkisista [Criteo](http://labs.criteo.com/downloads/download-terabyte-click-logs/) tietojoukkoja. Azure koneen Learning avulla binaarinen luokitus-mallin luominen Tämä tiedoille. On myös näyttää, miten voit julkaista jokin näistä malleista verkkopalvelun.

On myös mahdollista suorittaa tehtäviä, tätä vaiheittaista esitettyjä IPython muistikirjan avulla. Käyttäjät, jotka haluat Kokeile tätä tapaa kysyttävä aiheen [Criteo ongelmatilanteita rakenne ODBC-yhteyden avulla](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-hive-walkthrough-criteo.ipynb) .


## <a name="dataset"></a>Criteo tietojoukko kuvaus

Criteo tiedot napsauttamalla tekstinsyöttö tietojoukko, joka on suunnilleen 370 Gigatavua pakattu gzip TSV tiedostoja (~1.3TB puretaan), jossa 4.3 miljardin tietueita. Se otetaan 24 päivän käytettäviksi [Criteo](http://labs.criteo.com/downloads/download-terabyte-click-logs/)tiedot. Tietoja tutkijoiden asiasta olemme on purettuna US kokeilla käytettävissä olevia tietoja.

Jokainen tässä tietojoukko tietue sisältää 40 sarakkeet:

- ensimmäisessä sarakkeessa on otsikko-sarakkeessa, joka ilmaisee, voiko käyttäjä napsauttaa **lisääminen** (arvo 1) tai valitse jokin (0-arvo)
- Seuraavaksi 13 sarakkeet ovat numeroita, ja
- viimeinen 26 on categorical sarakkeet

Sarakkeet ovat anonymized ja käytetään numeroidut nimet: "Sar1" (sarakkeelle otsikko), "Col40" (for categorical viimeisen sarakkeen).            

Näin ote kaksi huomioita (rivejä) tämä tietojoukko-20 ensimmäistä saraketta:

    Col1    Col2    Col3    Col4    Col5    Col6    Col7    Col8    Col9    Col10   Col11   Col12   Col13   Col14   Col15           Col16           Col17           Col18           Col19       Col20

    0       40      42      2       54      3       0       0       2       16      0       1       4448    4       1acfe1ee        1b2ff61f        2e8b2631        6faef306        c6fc10d3    6fcd6dcb           
    0               24              27      5               0       2       1               3       10064           9a8cb066        7a06385f        417e6103        2170fc56        acf676aa    6fcd6dcb                      

Tämä tietojoukko numeerinen ja categorical-sarakkeet on puuttuu arvoa. Olemme kuvaavat yksinkertainen tapa puuttuvien arvojen käsittely. Lisätietoja tiedot ovat tarkasteltavia kun ne tallennetaan rakenteen taulukoiksi.

**Määritys:** *Käyttöaste (tuot.sol./KUORM.RYH):* Tämä on tiedot hiiren napsautuksella. Tämä Criteo tietojoukko tuot.sol./KUORM.RYH on noin 3,3 % tai 0.033.

## <a name="mltasks"></a>Esimerkkejä tekstinsyöttö tehtävät
Kahden otoksen tekstinsyöttö ongelmia on lisätietoa kohdassa tätä vaiheittaista:

1. **Binaarinen luokitus**: ennustaa, voiko käyttäjä napsauttaa Lisää:
    - Luokan 0: Ei napsauttamalla
    - Luokka 1: Valitse

2. **Regressiosuoran**: ennustaa ad-pika-käyttäjän ominaisuuksia todennäköisyyden.


## <a name="setup"></a>Määritä määrittäminen HDInsight Hadoop klusteri tietojen tiede varten

**Huomautus:** Tämä on yleensä **järjestelmänvalvoja** -tehtävä.

Määritä Azure tietojen tiede ympäristön ennakoivan analytics-ratkaisujen kanssa HDInsight varausyksiköt kolme vaihetta:

1. [Luo tili tallennustilan](../storage/storage-create-storage-account.md): tallennustilan tilin voidaan tallentaa Azure-Blob-objektien tallennustilaan. HDInsight klustereissa käytetyt tiedot tallennetaan tähän.

2. [Mukauta Azure Hdinsightiin Hadoop klustereiden saat tietoja tiede](machine-learning-data-science-customize-hadoop-cluster.md): Tämä vaihe luo Azure Hdinsightiin Hadoop-klusterin 64-bittinen Anaconda Python 2.7 kaikki solmut asennettuina. Viimeistele HDInsight-klusterin mukautettaessa on kaksi tärkeää vaihetta (kuvattu tämän artikkelin).

    * Jos linkität tallennustilan tilin luotu HDInsight-klusterin vaiheen 1 luomisen yhteydessä. Tallennustilan tilin käytetään tietojen voi käsitellä sisällä klusterin käyttäminen.

    * Sinun on otettava etäkäyttöpalvelimen klusterin pään solmu sen luomisen jälkeen. Muista määrittämiäsi (muun kuin sen luomisen osoitteessa klusterin määritetylle) etäkäyttöpalvelimen tunnistetiedot: uudelleen käyttöön tekemällä seuraavat toimet.

3. [Luo Azure ML työtila](machine-learning-create-workspace.md): Tämä Azure Konepohjaisten Oppimistekniikoiden työtilan käytetään etsimisen koneen learning mallien jälkeen aloitustiedot tarkasteluun ja esimerkkejä HDInsight-klusterin alaspäin.

## <a name="getdata"></a>Hae ja julkisen tietolähteen tarjoaman

[Criteo](http://labs.criteo.com/downloads/download-terabyte-click-logs/) tietojoukko niitä voi käyttää napsauttamalla linkkiä, hyväksy käyttöehdot ja nimen antaminen. Mitä tämä näyttää tilannevedoksen on mukana:

![Hyväksy Criteo ehdot](./media/machine-learning-data-science-process-hive-criteo-walkthrough/hLxfI2E.png)

Valitsemalla **Lataa Jatka** lisätietoja dataset ja sen käytettävyyttä.

Tiedot sijaitsevat julkisen [Azure blob storage](../storage/storage-dotnet-how-to-use-blobs.md) sijainnissa: wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/. "wasb" viittaa Azure-Blob-objektien tallennuspaikka. 

1. Tätä yleisen Blob-objektien tallennustilaan tietojen koostuu kolmesta alikansiot wsp tietojen.

    1. Alikansion *raaka/Laske/* sisältää tiedot – päivästä ensimmäisen 21 päivän\_00 päivään\_20
    2. Alikansion *raaka/junassa/* yhden päivän tietojen koostuu päivän\_21
    3. Alikansion *raaka/testi/* kuuluu kaksi päivää, päivän\_22 ja päivän\_23

2. Käyttäjille, joiden haluat aloittaa raaka gzip tiedoilla, ovat myös käytettävissä pääkansion *raaka /* kuin day_NN.gz, johon NN sijoitetaan 00 – 23.

Vaihtoehtoinen menetelmä käyttää ja tutkiminen, ja tiedoista, joka ei edellytä mitään paikallisen lataukset selitetään tarkemmin jäljempänä tätä vaiheittaista kun rakenteen taulukot luodaan mallin.

## <a name="login"></a>Klusterin headnode kirjautuminen

Kirjautua sisään klusterin headnode [Azure portal](https://ms.portal.azure.com) avulla voit paikantaa klusterin. Vasemmalla HDInsight elephant-kuvaketta ja kaksoisnapsauta sitten yhteyttä klusterin nimen. Siirry **määritys** -välilehti, kaksoisnapsauta sivun alareunaan Yhdistä-kuvaketta ja anna tunnistetiedot pyydettäessä etäkäyttö. Tämä avaa klusterin headnode.

Näin tyypilliset ensimmäisen lokiin-klusterin headnode näyttää seuraavalta:

![Kirjaudu sisään klusteri](./media/machine-learning-data-science-process-hive-criteo-walkthrough/Yys9Vvm.png)


Vasemmassa reunassa näemme "Hadoop komentorivin", joka on Microsoftin workhorse tietojen tarkasteluun. Näkymässä näkyy myös kaksi hyödyllisiä URL - "Hadoop kuitenkaan tila" ja "Hadoop nimi solmu". Kuitenkaan tila URL-osoite näyttää työn edistymistä ja nimi solmun URL-Osoitteen avulla tiedot klusterin määritysten.

Nyt syy on määritetty ja valmis aloittamaan vaiheittaista alkuosa: tietojen tarkasteluun käyttämällä rakenne ja tietojen hakeminen Azure koneen Learning valmis.

## <a name="hive-db-tables"></a>Tietokannan rakenne ja taulukoiden luominen

Microsoftin Criteo tietojoukko rakenteen taulukoiden luominen, Avaa ***Hadoop komentoriviltä*** pään solmun työpöydällä ja syötä rakenteen hakemiston kirjoittamalla komento

    cd %hive_home%\bin

>[AZURE.NOTE] Suorittaa kaikki rakenne-komennot tätä vaiheittaista rakenteen roskakorista / kansio tulee näkyviin. Tämä on hoitamaan polku ongelmat automaattisesti. Käytämme termejä "Rakenteen directory kehote", "rakenteen bin / directory kehote"- ja "Hadoop komentorivin" tarkoittavat.

>[AZURE.NOTE]  Mikä tahansa rakenne-kyselyn suorittaminen jokin aina käyttää seuraavia komentoja:

        cd %hive_home%\bin
        hive

Kun rakenteen STAA tulee, jossa "rakenne >"Kirjaudu, riittää, että Leikkaa ja liitä se suorittaa kyselyn.

Seuraava koodi luo tietokannan "criteo" ja luo sitten 4 taulukot:


* päivien päivänä luodun *taulukon luonnissa laskee* \_00 päivään\_20,
* päivänä luodun *taulukon junassa tietojoukko käyttää* \_21 ja
* kaksi *taulukoiden käyttäminen testi tietojoukkoja* päivän rakennettu\_22 ja päivän\_23 tarpeen mukaan.

On jaettu Microsoftin testi-tietojoukko kaksi eri taulukkoa koska jokin päivät on vapaapäivä ja haluat selvittää, jos mallin tunnistaa holiday ja loma erot käyttöaste kohteesta.

Komentosarjan [otoksen & #95; rakenne & #95; luominen & #95; criteo & #95; tietokannan & #95; ja & #95;tables.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_create_criteo_database_and_tables.hql) tässä kentässä näkyy asiasta:

    CREATE DATABASE IF NOT EXISTS criteo;
    DROP TABLE IF EXISTS criteo.criteo_count;
    CREATE TABLE criteo.criteo_count (
    col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    LINES TERMINATED BY '\n'
    STORED AS TEXTFILE LOCATION 'wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/count';

    DROP TABLE IF EXISTS criteo.criteo_train;
    CREATE TABLE criteo.criteo_train (
    col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    LINES TERMINATED BY '\n'
    STORED AS TEXTFILE LOCATION 'wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/train';

    DROP TABLE IF EXISTS criteo.criteo_test_day_22;
    CREATE TABLE criteo.criteo_test_day_22 (
    col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    LINES TERMINATED BY '\n'
    STORED AS TEXTFILE LOCATION 'wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/test/day_22';

    DROP TABLE IF EXISTS criteo.criteo_test_day_23;
    CREATE TABLE criteo.criteo_test_day_23 (
    col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    LINES TERMINATED BY '\n'
    STORED AS TEXTFILE LOCATION 'wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/test/day_23';

On Huomaa, että nämä taulukot ovat ulkoisen on vain osoittamalla Azure Blob Storage (wasb) sijainnit.

**Suorittaa minkä tahansa rakenne kysely, joka on nyt mainita kahdella eri tavalla.**

1. **Komentorivin rakenteen STAA käyttäminen**: ensimmäinen on "rakenne-komento ja kopioida ja liittää kyselyn rakenne-STAA komentorivin. Tällöin toimi:

        cd %hive_home%\bin
        hive

    Nyt osoitteessa komentorivin STAA leikkaaminen ja liittäminen kyselyn käynnistää sen.

2. **Tallentamalla tiedoston kyselyjä ja suoritettaessa komentoa**: toinen on raportin tallennus kyselyt .hql-tiedostoon ([otoksen & #95; rakenne & #95; luominen & #95; criteo & #95; tietokannan & #95; ja & #95;tables.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_create_criteo_database_and_tables.hql)) ja antamalla sitten kyselyn suorittaminen seuraavan komennon:

        hive -f C:\temp\sample_hive_create_criteo_database_and_tables.hql


### <a name="confirm-database-and-table-creation"></a>Vahvista tietokannan ja taulukon luominen

Seuraavaksi on Vahvista tietokannan rakenteen roskakorista seuraavalla komennolla luonti / directory kehote:

        hive -e "show databases;"

Näin:

        criteo
        default
        Time taken: 1.25 seconds, Fetched: 2 row(s)

Tämä tarkoittaa oleva luominen uuteen tietokantaan "criteo".

Luomaasi taulukot näkyviin tähän komennon roskakorista rakenne on yksinkertaisesti antaa / directory kehote:

        hive -e "show tables in criteo;"

Näemme, valitse seuraava tulos:

        criteo_count
        criteo_test_day_22
        criteo_test_day_23
        criteo_train
        Time taken: 1.437 seconds, Fetched: 4 row(s)

##<a name="exploration"></a>Tietojen etsintä-rakenne

Nyt olemme valmis tekemään joitakin perustiedot tarkasteluun-rakenne. Olemme Aloita kappalemäärän junassa esimerkkejä ja testaa arvotaulukoita.

### <a name="number-of-train-examples"></a>Esimerkkejä junassa määrä

[Esimerkki & #95; rakenne & #95; Laske & #95; junassa & #95; taulukon & #95;examples.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_count_train_table_examples.hql) sisällön näkyvät seuraavassa:

        SELECT COUNT(*) FROM criteo.criteo_train;

Antaa tulokseksi:

        192215183
        Time taken: 264.154 seconds, Fetched: 1 row(s)

Vaihtoehtoisesti jokin voi myös antaa seuraava komento rakenteen roskakorista / directory kehote:

        hive -f C:\temp\sample_hive_count_criteo_train_table_examples.hql

### <a name="number-of-test-examples-in-the-two-test-datasets"></a>Testaa esimerkeissä kaksi testi tietojoukkoja määrä

On nyt kaksi testi tietojoukkoja esimerkkejä määrän laskeminen. Sisällön [otoksen & #95; rakenne & #95; Laske & #95; criteo & #95; Testaa & #95; päivä & #95; 22 & #95; taulukon & #95;examples.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_count_criteo_test_day_22_table_examples.hql) ovat seuraavat:

        SELECT COUNT(*) FROM criteo.criteo_test_day_22;

Antaa tulokseksi:

        189747893
        Time taken: 267.968 seconds, Fetched: 1 row(s)

Tavalliseen tapaan, emme voi myös soittaa komentosarja rakenteen roskakorista / hakemiston varmenteiden komento kehote mukaan:

        hive -f C:\temp\sample_hive_count_criteo_test_day_22_table_examples.hql

Lopuksi on tutkia testi esimerkkejä päivän perusteella testi tietojoukossa määrä\_23.

Toiminto-komento on vain kuvatun samalla (Lisätietoja [otoksen & #95; rakenne & #95; Laske & #95; criteo & #95; Testaa & #95; päivä & #95; 23 & #95;examples.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_count_criteo_test_day_23_examples.hql)):

        SELECT COUNT(*) FROM criteo.criteo_test_day_23;

Näin:

        178274637
        Time taken: 253.089 seconds, Fetched: 1 row(s)

### <a name="label-distribution-in-the-train-dataset"></a>Selitteen jakautumisen junassa-tietojoukko

Otsikko-jakauman junassa tietojoukossa on halutut. Jos haluat nähdä, emme esittämään sisällön [otoksen & #95; rakenne & #95; criteo & #95; otsikko & #95; jako & #95; junassa & #95;table.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_label_distribution_train_table.hql):

        SELECT Col1, COUNT(*) AS CT FROM criteo.criteo_train GROUP BY Col1;

Antaa tulokseksi otsikko jakauman:

        1       6292903
        0       185922280
        Time taken: 459.435 seconds, Fetched: 2 row(s)

Huomaa, että positiivinen tarra prosentti noin 3,3 % (johdonmukaisia verrattuna alkuperäisen tietojoukko).

### <a name="histogram-distributions-of-some-numeric-variables-in-the-train-dataset"></a>Histogrammi jaot joitakin numeerisia muuttujien junassa-tietojoukko

Käytämme rakenne on alkuperäinen "Histogrammi\_numeerinen"-funktio selvittää, miltä jakautumisen numeerinen muuttujat näyttää. Seuraavassa on [Esimerkki & #95; rakenne & #95; criteo #95; Histogrammi & #95;numeric.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_histogram_numeric.hql)sisältö:

        SELECT CAST(hist.x as int) as bin_center, CAST(hist.y as bigint) as bin_height FROM
            (SELECT
            histogram_numeric(col2, 20) as col2_hist
            FROM
            criteo.criteo_train
            ) a
            LATERAL VIEW explode(col2_hist) exploded_table as hist;

Tämä tuottaa seuraavasti:

        26      155878415
        2606    92753
        6755    22086
        11202   6922
        14432   4163
        17815   2488
        21072   1901
        24113   1283
        27429   1225
        30818   906
        34512   723
        38026   387
        41007   290
        43417   312
        45797   571
        49819   428
        53505   328
        56853   527
        61004   160
        65510   3446
        Time taken: 317.851 seconds, Fetched: 20 row(s)

SIVUTTAIN NÄKYMÄ - hajottaa yhdistelmän rakenteen palvelee tuottaa kaltaisessa SQL-tuotosta tavallista luettelon asemesta. Huomaa, että tämän taulukon ensimmäinen sarake vastaa bin keskelle ja toinen bin korkojakso.

### <a name="approximate-percentiles-of-some-numeric-variables-in-the-train-dataset"></a>Lähes arvona joitakin numeerisia muuttujien junassa-tietojoukko

On myös numeerinen muuttujat korko lähes arvona laskenta. Rakenne on alkuperäisen "prosenttipiste\_noin" toimii näin us. Sisällön [otoksen & #95; rakenne & #95; criteo & #95; lähes & #95;percentiles.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_approximate_percentiles.hql) ovat:

        SELECT MIN(Col2) AS Col2_min, PERCENTILE_APPROX(Col2, 0.1) AS Col2_01, PERCENTILE_APPROX(Col2, 0.3) AS Col2_03, PERCENTILE_APPROX(Col2, 0.5) AS Col2_median, PERCENTILE_APPROX(Col2, 0.8) AS Col2_08, MAX(Col2) AS Col2_max FROM criteo.criteo_train;

Antaa tulokseksi:

        1.0     2.1418600917169246      2.1418600917169246    6.21887086390288 27.53454893115633       65535.0
        Time taken: 564.953 seconds, Fetched: 1 row(s)

Olemme remark, PROSENTTIPISTE jakautumisen liittyy läheisesti mikä tahansa numeerinen muuttuja Histogrammi jakautumisen yleensä.        

### <a name="find-number-of-unique-values-for-some-categorical-columns-in-the-train-dataset"></a>Ainutkertaisten arvojen määrän etsiminen joitakin categorical sarakkeita junassa-tietojoukko

Jatkuvaa tietojen tarkasteluun, nyt etsiä, jotkin categorical sarakkeet, ne käyttää yksilöllisten arvojen määrän. Voit tehdä tämän emme esittämään sisällön [otoksen & #95; rakenne & #95; criteo & #95; yksilöllinen & #95; arvot ja #95;categoricals.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_unique_values_categoricals.hql):

        SELECT COUNT(DISTINCT(Col15)) AS num_uniques FROM criteo.criteo_train;

Antaa tulokseksi:

        19011825
        Time taken: 448.116 seconds, Fetched: 1 row(s)

Olemme Huomaa, että Col15 on yksilöllisiä arvoja, 19M! Naïve menetelmillä, kuten "yhden kuuman koodaus" koodata kyseisten suuri kolmiulotteisen categorical muuttujien on mahdotonta. Erityisesti on kerrotaan ja osoittaa kutsutaan käyttöönottamiseen ongelman tarkoitettu tehokkaasti [Oppiminen ja laskee](http://blogs.technet.com/b/machinelearning/archive/2015/02/17/big-learning-made-easy-with-counts.aspx) tehokkaita ja tehokkaat tekniikka.

Olemme Lopeta aliraportti sisältö katsomalla joitakin muita categorical sarakkeiden sekä ainutkertaisten arvojen määrän. Sisällön [otoksen & #95; rakenne & #95; criteo & #95; yksilöllinen & #95 arvot ja #95 useita; & #95;categoricals.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_unique_values_multiple_categoricals.hql) ovat:

        SELECT COUNT(DISTINCT(Col16)), COUNT(DISTINCT(Col17)),
        COUNT(DISTINCT(Col18), COUNT(DISTINCT(Col19), COUNT(DISTINCT(Col20))
        FROM criteo.criteo_train;

Antaa tulokseksi:

        30935   15200   7349    20067   3
        Time taken: 1933.883 seconds, Fetched: 1 row(s)

Uudelleen näemme, että Col20, lukuun ottamatta kaikkien muiden sarakkeiden on monta yksilölliset arvot.

### <a name="co-occurence-counts-of-pairs-of-categorical-variables-in-the-train-dataset"></a>Muiden esiintymä laskee paria categorical muuttujien junassa-tietojoukko

Paria categorical muuttujien työtovereiden esiintymä-arvo on myös halutut. Tämä määritettävissä-koodin avulla [otoksen & #95; rakenne & #95; criteo & #95; pisteparin & #95; categorical & #95;counts.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_paired_categorical_counts.hql):

        SELECT Col15, Col16, COUNT(*) AS paired_count FROM criteo.criteo_train GROUP BY Col15, Col16 ORDER BY paired_count DESC LIMIT 15;

Olemme käänteisen määrät lajittelu niiden esiintymä ja tarkista tässä tapauksessa ensimmäiset 15. Näin us:

        ad98e872        cea68cd3        8964458
        ad98e872        3dbb483e        8444762
        ad98e872        43ced263        3082503
        ad98e872        420acc05        2694489
        ad98e872        ac4c5591        2559535
        ad98e872        fb1e95da        2227216
        ad98e872        8af1edc8        1794955
        ad98e872        e56937ee        1643550
        ad98e872        d1fade1c        1348719
        ad98e872        977b4431        1115528
        e5f3fd8d        a15d1051        959252
        ad98e872        dd86c04a        872975
        349b3fec        a52ef97d        821062
        e5f3fd8d        a0aaffa6        792250
        265366bf        6f5c7c41        782142
        Time taken: 560.22 seconds, Fetched: 15 row(s)

## <a name="downsample"></a>Esimerkki varten Azure koneen Learning tietojoukkoja alaspäin

Ottaa tietojoukkoja tarkasteltavia ja osoittaa emme voi miten tällaista kaikki muuttujat (mukaan lukien kombinaatioiden) on nyt otoksen alaspäin tarkasteluun tietojoukkojen, jotta voimme kehittää Azure koneen Learning mallit. Peruuttaminen, joka on keskittyä ongelma: valita Esimerkki määritteet (Sarake2 - Col40 arvot ominaisuus) joukko, emme ennustaa Jos Sarake1 on 0 (ei napsauttamalla) tai 1 (napsauttamalla).

Esimerkki Microsoftin junassa alas ja testaa tietojoukkoja 1 % alkuperäisestä koosta, emme Käytä rakenne on alkuperäisen Funktiot RAND()-funktiota. Seuraava komentosarja [otoksen & #95; rakenne & #95; criteo & #95; erotuskykyä & #95; junassa & #95;dataset.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_downsample_train_dataset.hql) toimii näin junassa tietojoukko:

        CREATE TABLE criteo.criteo_train_downsample_1perc (
        col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
        LINES TERMINATED BY '\n'
        STORED AS TEXTFILE;

        ---Now downsample and store in this table

        INSERT OVERWRITE TABLE criteo.criteo_train_downsample_1perc SELECT * FROM criteo.criteo_train WHERE RAND() <= 0.01;

Antaa tulokseksi:

        Time taken: 12.22 seconds
        Time taken: 298.98 seconds

Komentosarjan [otoksen & #95; rakenne & #95; criteo & #95; erotuskykyä & #95; Testaa & #95; päivä & #95; 22 & #95;dataset.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_downsample_test_day_22_dataset.hql) tekee sen testitiedot, päivän\_22:

        --- Now for test data (day_22)

        CREATE TABLE criteo.criteo_test_day_22_downsample_1perc (
        col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
        LINES TERMINATED BY '\n'
        STORED AS TEXTFILE;

        INSERT OVERWRITE TABLE criteo.criteo_test_day_22_downsample_1perc SELECT * FROM criteo.criteo_test_day_22 WHERE RAND() <= 0.01;

Antaa tulokseksi:

        Time taken: 1.22 seconds
        Time taken: 317.66 seconds


Lopuksi komentosarja [otoksen & #95; rakenne & #95; criteo & #95; erotuskykyä & #95; Testaa & #95; päivä & #95; 23 & #95;dataset.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_downsample_test_day_23_dataset.hql) tekee sen testitiedot, päivän\_23:

        --- Finally test data day_23
        CREATE TABLE criteo.criteo_test_day_23_downsample_1perc (
        col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 srical feature; tring)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
        LINES TERMINATED BY '\n'
        STORED AS TEXTFILE;

        INSERT OVERWRITE TABLE criteo.criteo_test_day_23_downsample_1perc SELECT * FROM criteo.criteo_test_day_23 WHERE RAND() <= 0.01;

Antaa tulokseksi:

        Time taken: 1.86 seconds
        Time taken: 300.02 seconds

Tällä emme valmis käyttämällä Microsoftin alaspäin näytteeksi junassa ja testaa tietojoukkoja etsimisen Azure koneen Learning mallit.

Ei lopullinen tärkeä osa, ennen kuin olemme Siirry Azure koneen Learning, joka on koskee Laske taulukko. Seuraavan sisäinen osion käsiteltävät aiheet tämä joitakin yksityiskohtaiset tiedot.

##<a name="count"></a>Laske taulukko lyhyt keskusteluun

Olemme tuli, useita categorical muuttujia on erittäin suuri dimensionaalisuus. Tutustu ongelmatilanteita on esitä tehokkaita tekniikkaa, jota kutsutaan [Oppiminen ja laskee](http://blogs.technet.com/b/machinelearning/archive/2015/02/17/big-learning-made-easy-with-counts.aspx) koodata muuttujia tehokasta, tehokkaat tavalla. Saat lisätietoja tällä menetelmällä on linkistä.

**Huomautus:** Tätä vaiheittaista olemme tietotasoja tuottaa compact esitykset suuri kolmiulotteisen categorical ominaisuuksien Laske taulukoiden avulla. Tämä ei ole ainoa tapa koodata categorical ominaisuuksista. Lisätietoja muita menetelmiä kiinnostunut käyttäjien kuitata ulos [yhden-kuuman-koodaus](http://en.wikipedia.org/wiki/One-hot) ja [ominaisuus hajautuksessa](http://en.wikipedia.org/wiki/Feature_hashing).

Laske taulukoiden luominen Laske tietoihin, Microsoft käyttää tietoja kansion raaka/määrä. Mallinnus-osassa on näyttää käyttäjät muodostaminen categorical tarkoitettuja alusta asti Laske nämä taulukot tai vaihtoehtoisesti valmiita Laske taulukko käytettävät niiden explorations. Mitä seuraavasti: kun viitataan "valmiita Laske taulukot", tärkeää tehdä Laske taulukoista, annamme. Voit käyttää näitä taulukoiden tarkat ohjeet toimitetaan seuraavan osion.

## <a name="aml"></a>Luo malli, joka sisältää Azure koneen oppiminen

Tutustu muodostus Azure koneen Learning-mallin seuraavasti:

1. [Tietojen noutaminen rakenteen taulukoiden Azure koneen oppiminen](#step1)
2. [Luo kokeen: puhdistaminen tiedot, valitse paremmin katselemalla ja featurize Laske taulukoita](#step2)
3. [Mallin kouluttaminen](#step3)
4. [Pisteet testitiedot-malli](#step4)
5. [Mallin arvioiminen](#step5)
6. [WWW-palvelun avulla voidaan kulutettu mallin julkaiseminen](#step6)

Nyt olemme valmis luomaan Azure koneen Learning Studiossa mallit. Tutustu alaspäin näytteeksi tiedot tallentuvat klusterin taulukoiden rakennetta. Azure koneen Learning **Tietojen tuominen** -moduulin avulla lukea tiedoista. Tämän klusterin tallennustilan tilin käyttöä tunnistetiedot toimitetaan loppuosa.

### <a name="step1"></a>Vaihe 1: Tietojen hakeminen rakenteen taulukoiden tuominen Azure koneen Koulujen tietojen tuominen-moduulin ja valitse se Koulujen kokeen tietokoneelle

Aloita valitsemalla **+ Uusi** -> **KOKEEN** -> **Tyhjä kokeen**. -Kohdasta vasemmassa yläkulmassa **haku** -ruudussa Hae "tuontitiedot". Vedä ja pudota voin kokeen alusta (näytön keskelle-osa) **Tietojen tuominen** -moduulin käyttää moduulin tietojen käyttöä.

Tämä on esimerkiksi näyttää **Tietojen** noudettaessa rakenne-taulukon tietoja:

![Tietojen tuominen saa tiedot](./media/machine-learning-data-science-process-hive-criteo-walkthrough/i3zRaoj.png)

**Tietojen tuominen** -moduulin, joka annetaan kuvan parametrien arvot ovat vain arvot, sinun on annettava Lajittele esimerkkejä. Seuraavassa on muutamia yleisiä ohjeita täyttäminen parametrin määrittäminen **Tuontitiedot** -moduuli.

1. Valitse **tietolähteen** "rakenne-kysely
2. **Kyselyn rakenne** -ruutuun yksinkertaisen SELECT * FROM < oman\_tietokannan\_name.your\_taulukon\_nimi >-on tarpeeksi.
3. **Hcatalog palvelimen URI**: Jos yhteyttä klusterin on "abc" ja valitse tämä on vain: https://abc.azurehdinsight.net
4. **Hadoop käyttäjätilisi nimi**: valinnut klusterin saadaan aikaan käyttäjänimi. (Ei etäkäyttöpalvelimen käyttäjänimi!)
5. **Hadoop käyttäjätilin salasanan**: valinnut klusterin saadaan aikaan käyttäjänimen salasana. (Ei etäkäyttöpalvelimen salasana!)
6. **Siirtää tietoja sijainti**: Valitse "Azure"
7. **Azure-tallennustilan tilin nimi**: tallennustilan klusterin liittyvää tiliä
8. **Azure-tallennustilan tilin avain**: avain-tallennustilan tilin liittyvän klusterin.
9. **Azure säilön nimi**: Jos klusterinimi on "abc" ja valitse yksinkertaisesti "abc", on yleensä.


Kun **Tietojen tuonti** on valmis (näkyy vihreä jakoviivojen-moduulin) tietojen käytön, voit tallentaa tiedot tietojoukko (valittua nimellä). Mitä tämä näyttää seuraavanlaiselta:

![Tietojen tuominen tietojen tallentaminen](./media/machine-learning-data-science-process-hive-criteo-walkthrough/oxM73Np.png)

Napsauta hiiren kakkospainikkeella **Tuontitiedot** -moduulin tulostusporttiin. Näyttää tietoruudussa **tietojoukon tallentaminen** -asetus ja **Visualisoi** -asetus. **Visualisoi** -vaihtoehto, jos napsautetaan, näyttää tiedot, kanssa, joka sopii joitakin yhteenveto tilastoja oikeanpuoleisessa 100 riviä. Vain tallentaa tiedot, valitse **Tallenna nimellä-tietojoukko** ja noudata ohjeita.

Valitse tallennettu tietojoukko käytettäväksi koneen learning kokeessa, Etsi **seuraavassa kuvassa näkyvät hakuruudun** tietojoukkoja. Kirjoita sitten nimi, annoit osittain voit käyttää sitä ja vetämällä dataset tärkeimmät Ohjauspaneelin tietojoukko. Pudottaminen sivulle tärkeimmät Ohjauspaneelin valitsee sen koneen learning mallinnus käytettäviksi.

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/cl5tpGw.png)

>[AZURE.NOTE] Toimi samoin junassa ja testaa tietojoukkoja. Muista myös, voit käyttää tietokannan nimi ja taulukoiden nimet, jotka annoit tähän tarkoitukseen. Kuvassa käytettävät arvot ovat tuloskorteissa kuvassa purposes.* *

### <a name="step2"></a>Vaihe 2: Luo yksinkertaisia kokeen Azure koneen Learning ennustaa napsautuksella / ei ole hiiren napsautuksella

Tutustu Azure ML kokeen näyttää seuraavanlaiselta:

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/xRpVfrY.png)

On nyt tutkia tämän kokeen tärkeimmät osat. Muistutukseksi annettava vetämällä Microsoftin tallennetun junassa ja testaa tietojoukkoja voin kokeilla Microsoftin piirtoalustan ensin.

#### <a name="clean-missing-data"></a>Puhdista puuttuvat tiedot

**Puuttuvien tietojen SIIVOA** -moduulin onko nimessä ehdottaa: se puhdistaa puuttuu tietoja tavoilla, jotka voivat olla käyttäjän määrittämä. Etkö löytänyt kyseiseen moduuliin, näemme näin:

![SIIVOA puuttuvat tiedot](./media/machine-learning-data-science-process-hive-criteo-walkthrough/0ycXod6.png)

Tässä kohdassa Microsoft valitsi korvaa kaikki puuttuvat arvot 0. Muut asetukset sekä, jotka voi nähdä katsomalla moduulin avattavista luetteloista on.

#### <a name="feature-engineering-on-the-data"></a>Tekniset tiedot-ominaisuus

Voi olla miljoonia yksilöllisiä arvoja on paljon categorical ominaisuuksia. Naïve tavoilla, kuten yksi kuuman koodaus käytettyjen suuri kolmiulotteisen categorical toiminnot on täysin muuttuu. Tätä vaiheittaista on osoitettava käyttämisestä Laske toiminnot käyttämällä valmiita Azure koneen Learning moduulit suuri kolmiulotteisen categorical muuttujia compact esityksiä luomiseen. Lopeta-tulos on mallin pienemmäksi, koulutus nopeammin ja suorituskyvyn mittarit, jotka ovat aivan kuin käyttävät muita tekniikoita.

##### <a name="building-counting-transforms"></a>Laske muunnokset rakennus

Muodostaa Laske ominaisuuksia Käytämme **Luominen laskien Transform** moduulin käytettävissä oleva Azure koneen Learning. Moduulin näyttää seuraavanlaiselta:


![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/e0eqKtZ.png)
![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/OdDN0vw.png)


**Tärkeä huomautus** : on Kirjoita **sarakkeiden määrä** -ruutuun sellaiset sarakkeet, että haluat suorittaa laskelmia. Yleensä nämä ovat (kuten edellä) suuri kolmiulotteisen categorical sarakkeet. Alussa, on mainittu Criteo tietojoukko on 26 categorical sarakkeet: stä Col15 Col40. Tässä on laskea niiden kaikkien ja anna niiden indeksien (-15 erotettuina esitetyllä 40).

Moduulin käyttöä MapReduce-tilassa (vastaava on paljon), annettava käyttöoikeuksien Hdinsightiin Hadoop-klusterin (tähän tarkoitukseen voidaan käyttää ominaisuutta tarkasteluun varten) ja sen tunnistetiedot. Edellisen luvut osoittavat, mitä täytettyjen arvot näyttävät (kuva verrattuna kaavaan kannalta oman Käyttötapaus annettujen arvojen korvaaminen).

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/05IqySf.png)

Edellä olevassa kuvassa on näyttää, miten syötteen blob-sijainnin. Tämä sijainti on varattu etsimisen Laske taulukoiden tiedot.


Kun moduuli on päättynyt, emme tallenna muunnos myöhemmin napsauttamalla moduulin ja **Tallenna nimellä-muunnos** -vaihtoehdon valitseminen:

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/IcVgvHR.png)

Tutustu yllä koe-arkkitehtuuri tietojoukko "ytransform2" vastaa täsmälleen tallennetun Laske muunto. Tämän kokeen loput oletetaan lukija **Luominen laskien Transform** moduuli käytettyyn tietoja luomiseen määrät ja voit käyttää näitä laskee Luo Laske ominaisuuksien käyttöönotto junassa ja testaa tietojoukkoja.

##### <a name="choosing-what-count-features-to-include-as-part-of-the-train-and-test-datasets"></a>Mitä Laske-välilehdessä voit jättää junassa ja testaa tietojoukkojen valitseminen

On valmis transform laskeminen, kun käyttäjä voi valita, mitkä toiminnot niiden junassa sisällyttäminen ja testaa **Muokata Laske taulukko parametrit** -moduulin tietojoukkoja. Olemme Näytä vain tämän moduulin tähän täydellisiä, mutta yksinkertaisuuden et itse asiassa Käytä Microsoftin kokeessa.

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/PfCHkVg.png)

Tässä tapauksessa näkevät, kun Microsoft on valinnut käyttää vain log-todennäköisyydellä ja Ohita Edellinen sarakkeen käytöstä. On myös määrittää parametrit roskaa bin raja-arvon, esimerkiksi kuinka monta Pseudo edellisen esimerkkejä Lisää tasoitus ja käytetäänkö minkä tahansa Laplacian vähentämiseksi vai ei. Kaikki näihin ovat lisäominaisuudet ja se on huomattava oletusarvot on hyvä aloittaa käyttäjille, jotka ole aiemmin käyttänyt tällaista ominaisuuden luominen.

##### <a name="data-transformation-before-generating-the-count-features"></a>Muunnetaan ennen luomista Laske-ominaisuudet

Nyt on tietoja muodonmuutoksen Microsoftin junassa kohtaan keskittyä ja tarkista tietoja ennen luodaan todella Laske ominaisuuksia. Huomaa, että on kaksi **R-komentosarjan suorittaminen** moduulit, ennen kuin emme käytä Laske-muunnos tietojamme.

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/aF59wbc.png)

Näin ensimmäisen R komentosarja:

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/3hkIoMx.png)


Tämä R-komentosarja on Microsoftin sarakkeiden nimeä "Col40" "Sar1" nimet. Tämä johtuu siitä Laske-muunnos odottaa nimet tässä muodossa.

Toinen R-komentosarja Microsoft saldo kesken positiivisia ja negatiivisia luokat (luokkien 1 ja 0 vastaavasti) erotuskykyä vähennetään negatiivinen luokan mukaan. R komentosarja Seuraavassa esitetään, kuinka voit seuraavasti:

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/91wvcwN.png)

Käytämme tämän yksinkertaisen R-komentosarjan "pos\_neg\_suhde", Määritä tasapaino positiivisia ja negatiivisia luokat. Tämä on tärkeää tehdä koska parantaminen luokan ristiriidan yleensä on suorituskyky luokitus ongelmia luokan jakauman missä Vino (peruuttaminen, että tässä tapauksessa on 3,3 % positiivinen luokka ja negatiivinen luokan 96.7 %).

##### <a name="applying-the-count-transformation-on-our-data"></a>Käyttämällä Laske-muunnos-tietojamme

Microsoft-käyttää **Käyttää muunnos** -moduulissa, käytä Laske-muunnoksia Microsoftin junassa ja testaa tietojoukkoja. Moduuli tulee yksi syötteeksi tallennettu Laske-Muunna- ja muut syötteenä junassa tai testi-tietojoukkoja, ja palauttaa tiedot Laske ominaisuuksilla. Se näkyy tässä:

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/xnQvsYf.png)

##### <a name="an-excerpt-of-what-the-count-features-look-like"></a>Näyttää ote mitä Laske-ominaisuuksia

On instructive esikatsella tässä tapauksessa Laske-ominaisuuksia. Seuraavassa on Näytä ote nämä:

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/FO1nNfw.png)

Tämä ote on Näytä, että sarakkeille, jotka on laskettu, emme Hae määrät ja kirjaudu todennäköisyydellä lisäksi kaikki asianmukaiset backoffs.

On nyt valmis käyttämällä näitä muunnettua tietojoukkoja Azure koneen Learning-mallin luominen. Seuraavan osion on näyttää, miten tämän voi tehdä.

#### <a name="azure-machine-learning-model-building"></a>Azure koneen Learning mallin luominen

##### <a name="choice-of-learner"></a>Paremmin katselemalla valinta

Ensin annettava valita paremmin katselemalla. Tarkastellaan käyttää kahden luokan tehosti Päätöspuun Microsoftin paremmin katselemalla. Seuraavassa on tämän paremmin katselemalla oletusasetusten:

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/bH3ST2z.png)

Tutustu kokeen osalta tarkastellaan Valitse oletusarvot. Olemme Huomaa, että oletusarvot ovat yleensä kuvaava ja erinomainen tapa saada nopeasti perusaikataulujen suorituskykyyn. Voit parantaa suorituskykyä sweeping parametreja, jos halua, kun olet perusaikataulun.

#### <a name="train-the-model"></a>Mallin kouluttaminen

Koulutusta, emme kutsua ainoastaan **Junassa mallin** moduuli. Kahta syötettä siihen on kaksi luokan tehosti Päätöspuun paremmin katselemalla ja tutustu junassa tietojoukko. Tämä on mukana:

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/2bZDZTy.png)


#### <a name="score-the-model"></a>Mallin pisteet

Kun olemme koulutetun mallin, emme valmis testi dataset-sija ja arvioida suorituskykyä. Olemme seuraavasti seuraavassa kuvassa, sekä **Arvioimaan malli** -moduulin **Pistemäärän malli** -moduulin avulla:

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/fydcv6u.png)


### <a name="step5"></a>Vaihe 5: Arvioi malli

Lopuksi haluamme mallin suorituskyvyn analysoiminen. Kaksi luokan (binaarinen) luokitus ongelmien hyvä toimenpide on yleensä AUC. Visualisoimiseen tämä on yhdistää **Arvioi malli** -moduulin **Pistemäärän mallin** moduulin tämän. **Visualisoi** valitsemalla **Arvioi malli** -moduulin tuottaa kuvan muistuttavan seuraavat:

![Moduulin BDT mallin arvioiminen](./media/machine-learning-data-science-process-hive-criteo-walkthrough/0Tl0cdg.png)

Binaarinen (tai kaksi luokan) luokitus ongelmia, hyvä mitta tekstinsyöttö tarkkuus on alueen kohdassa käyrän (AUC). Valitse loppuosa Näytä tutustu tämän mallin avulla Microsoftin testi-tietojoukko tulokset. Saat tämän hiiren kakkospainikkeella **Arvioi mallin** moduuli ja valitse sitten **Visualisoi**tulostusporttiin.

![Arvioi mallin moduulin visualisointi](./media/machine-learning-data-science-process-hive-criteo-walkthrough/IRfc7fH.png)

### <a name="step6"></a>Vaihe 6: Julkaise mallin Web-palvelu
Mahdollisuus Azure koneen Learning-mallin julkaiseminen verkkopalvelut helposti vähintään on hyödyllinen ominaisuus, jolla varmistetaan käytettävissä. Sen jälkeen, kuka tahansa voi soittaa WWW-palvelun kanssa, että he tarvitsevat ennusteiden varten ja WWW-palvelun käytetään mallin palauttamiseen näiden ennusteiden syöttötiedot.

Voit tehdä tämän olemme tallentamista Microsoftin koulutetun mallin koulutetun mallin objektina. Tämä tapahtuu napsauttamalla **Junassa malli** -moduulin ja **koulutettua mallin tallentaminen** -vaihtoehdon.

Seuraavaksi annettava luominen syötteen ja tulosteen Microsoftin WWW-palvelun portit:

* syötteen portin Vie tiedot samassa muodossa kuin tiedot, joita tarvitaan ennusteiden varten
* tulostus-portin palauttaa tulos otsikot ja todennäköisyys.

#### <a name="select-a-few-rows-of-data-for-the-input-port"></a>Valitse joitakin tietorivejä syötteen portin

On helppo **Käyttää SQL-muunnos** -moduulin avulla voit valita vain 10 rivien yhteyshenkilönä syötteen portin tiedot. Valitse vain nämä tietoriviä Microsoftin input portin kuvassa SQL-kyselyn avulla:

![Syötteen portin tiedot](./media/machine-learning-data-science-process-hive-criteo-walkthrough/XqVtSxu.png)

#### <a name="web-service"></a>Web-palvelu
Microsoft on nyt valmis suorittamaan pieni koe, jonka avulla voidaan julkaista Microsoftin WWW-palvelun.

#### <a name="generate-input-data-for-webservice"></a>Luo verkkopalvelu syöttötiedot

Zeroth vaiheessa jälkeen Laske taulukko on suuri, emme kestää muutaman rivin testitiedot ja luo siirtää tietoja siitä Laske ominaisuuksilla. Tämä Microsoftin verkkopalvelu syöttötiedot muoto voi toimia. Tämä on mukana:

![Luo BDT kenttään annettavat tiedot](./media/machine-learning-data-science-process-hive-criteo-walkthrough/OEJMmst.png)

>[AZURE.NOTE] Syötteen tietomuodon Käytämme nyt **Laske Featurizer** moduulin tulos. Kun tämän kokeen on päättynyt, tallentaa tulosteen **Laske Featurizer** moduulista tietojoukko. Tämä tietojoukko käytetään verkkopalvelu syötteen tietoja.

#### <a name="scoring-experiment-for-publishing-webservice"></a>Näkyvissä kokeen pistemäärä julkaisun verkkopalvelu varten

Olemme ensin Näytä tämä näyttää tältä. Olennaiset rakenne on **Tulos mallin** moduuli, jolla hyväksyy Microsoftin koulutetun malli-objekti ja muutaman rivin kenttään annettavat tiedot, jotka on luotu **Laske Featurizer** -moduulin edelliset vaiheet. Käytämme "Valitse sarakkeet-tietojoukko" projektin Scored otsikot ja mahdollisuuden tuloksen todennäköisyyden.

![Valitse sarakkeet-tietojoukko](./media/machine-learning-data-science-process-hive-criteo-walkthrough/kRHrIbe.png)

Huomaa, miten **Tietojoukko Valitse sarakkeet** -moduulin voi käyttää '' tietojen suodattaminen Valitse tietojoukko. Olemme tähän sisällön näyttäminen:

![Valitse tietojoukko moduulissa sarakkeisiin suodattaminen](./media/machine-learning-data-science-process-hive-criteo-walkthrough/oVUJC9K.png)

Voit saada portit ja sininen syötteen, voit valitsemalla **valmisteleminen verkkopalvelu** oikeassa alareunassa. Käytössä tämän kokeen myös pystyy julkaisemaan web-palvelu: Valitse **JULKAISE WWW-palvelun** kuvaketta oikeassa alakulmassa kuvassa:

![Julkaise Web-palvelu](./media/machine-learning-data-science-process-hive-criteo-walkthrough/WO0nens.png)


Kun verkkopalvelu on julkaistu, emme Hae ohjataan sivulle, joka näyttää näin:

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/YKzxAA5.png)

Näemme verkkopalvelujen kaksi linkit vasemmalla puolella:

* **PYYNNÖN ja VASTAUKSEN** palvelu (tai RRS) on tarkoitettu yhden ennusteiden ja on olemme hyödyntää tämän workshop.
* **ERÄKÄSITTELY** Service (BES) käytetään erä ennusteiden ja edellyttää, että syöttötiedot käyttää ennusteiden Azure-Blob-säiliö sijaitsevat.

Kun napsautat linkkiä **PYYNNÖN ja VASTAUKSEN** kestää meihin yhteyttä-sivulle, joka antaa us valmiiksi säilyke C#, python ja r-koodi Tämä koodi voidaan kätevästi, puheluiden soittamiseen verkkopalvelu. Huomaa, että Ohjelmointirajapinnan avain tällä sivulla on käytettävä todennusta varten.

On helppo kopioida python koodi päälle IPython muistikirjaan uusi solu.

Seuraavassa on Näytä python koodin segmentin oikea API-näppäimen kanssa.

![Python koodi](./media/machine-learning-data-science-process-hive-criteo-walkthrough/f8N4L4g.png)


Huomaa, että on korvattu oletusarvo-Ohjelmointirajapinnan avain Microsoftin verkkopalvelujen Ohjelmointirajapinnan avain. Valitsemalla **Suorita** solun IPython muistikirjan tuottaa seuraavan vastauksen:

![IPython vastaus](./media/machine-learning-data-science-process-hive-criteo-walkthrough/KSxmia2.png)

Näemme, että kaksi testi esimerkkejä on kysytyt (Valitse JSON puitteissa python komentosarja), on vastauksia takaisin lomake "Scored otsikoita, Scored todennäköisyys liittyy". Huomaa, että tässä tapauksessa Microsoft valitsi oletusarvot, jotka valmiiksi purkitettu koodi on (0 on kaikkien numeeristen sarakkeiden ja merkkijonon "arvon" kaikki categorical sarakkeet).

Microsoftin lopusta loppuun ongelmatilanteita esittävä käsittelemään suurissa tietojoukko käyttämällä Azure koneen Learning on päättynyt. Olemme teratavun tietojen aloittaminen, rakennettava tekstinsyöttö mallin ja ottaa käyttöön pilveen WWW-palvelulle.
