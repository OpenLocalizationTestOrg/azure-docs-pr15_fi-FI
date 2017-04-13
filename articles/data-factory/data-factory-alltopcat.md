<properties
    pageTitle="Kaikki aiheet Data Factory-palvelun | Microsoft Azure"
    description="Taulukon kaikista aiheista Azure-palvelun nimi Data Factory järjestelmässä olevat http://azure.microsoft.com/documentation/articles/ ja otsikko ja kuvaus."
    services="data-factory"
    documentationCenter=""
    authors="spelluru"
    manager="jhubbard"
    editor="MightyPen"/>

<tags
    ms.service="data-factory"
    ms.workload="data-factory"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/05/2016"
    ms.author="spelluru"/>


# <a name="all-topics-for-azure-data-factory-service"></a>Kaikki aiheet Azure Data Factory-palveluun

Tässä artikkelissa on lueteltu jokaisen artikkelista, johon koskee Azure **Data Factory** -palvelun. Voit hakea avainsanojen tämä sivu nykyiseen halutut aiheet **Ctrl + F**, avulla.




## <a name="new"></a>Uusi

| &nbsp; | Otsikko | Kuvaus |
| --: | :-- | :-- |
| 1 | [Siirrä tiedot-Amazon Redshift Azure Data Factory käyttäminen](data-factory-amazon-redshift-connector.md) | Voit siirtää tietoja käyttämällä Azure Data Factory Amazon Redshift tietoja. |
| 2 | [Siirrä tiedot-Amazon yksinkertainen Tallennuspalvelu Azure Data Factory käyttäminen](data-factory-amazon-simple-storage-service-connector.md) | Lisätietoja siirtäminen tiedot käyttämällä Azure Data Factory Amazon yksinkertainen Tallennuspalvelu (S3). |
| 3 | [Azure tietojen Factory - Ohjattu kopiointi](data-factory-azure-copy-wizard.md) | Lue lisää tietojen kopioiminen tietolähteiden poistumia ohjatun tietojen Factory Azure kopioi avulla. |
| 4 | [Opetusohjelma: Luo ensimmäinen factory Azure tietojen käyttäminen tietojen Factory REST-Ohjelmointirajapinnalla](data-factory-build-your-first-pipeline-using-rest-api.md) | Tässä opetusohjelmassa luominen käyttämällä tietojen Factory REST API otoksen Azure Data Factory putkijohto. |
| 5 | [Opetusohjelma: Luo putkijohto kopioi aktiviteettiin .NET-Ohjelmointirajapinnan käyttäminen](data-factory-copy-activity-tutorial-using-dotnet-api.md) | Tässä opetusohjelmassa luot Azure Data Factory-myyntijakso kopioi aktiviteettiin käyttämällä .NET-Ohjelmointirajapinnan. |
| 6 | [Opetusohjelma: Luo putkijohto kopioi aktiviteettiin REST-Ohjelmointirajapinnan käyttäminen](data-factory-copy-activity-tutorial-using-rest-api.md) | Tässä opetusohjelmassa luot Azure Data Factory-myyntijakso kopioi aktiviteettiin käyttämällä REST API. |
| 7 | [Ohjattu tietojen Factory kopio](data-factory-copy-wizard.md) | Lue lisää tietojen kopioiminen tietolähteiden poistumia ohjattu Factory kopiointi avulla. |
| 8 | [Data Management Gateway](data-factory-data-management-gateway.md) | Tietojen siirtäminen paikallisen käyttöön ja määrittää yhdyskäytävän tiedot. Azure Data Factory Data Management Gatewayn avulla voit siirtää tietoja. |
| 9 | [Tietojen siirtäminen paikalliseen Cassandra tietokannasta käyttämällä Azure Data Factory](data-factory-onprem-cassandra-connector.md) | Lisätietoja tietojen siirtäminen paikalliseen Cassandra tietokannasta käyttämällä Azure Data Factory. |
| 10 | [Siirrä tiedot-MongoDB Azure Data Factory käyttäminen](data-factory-on-premises-mongodb-connector.md) | Lisätietoja tietojen siirtäminen MongoDB-tietokannasta Azure Data Factory. |
| 11 | [Siirrä tiedot Salesforce Azure Data Factory avulla](data-factory-salesforce-connector.md) | Voit siirtää tietoja Salesforce käyttämällä Azure Data Factory tietoja. |


## <a name="updated-articles-data-factory"></a>Päivitetty artikkeleita Data Factory

Tässä osassa on luettelo artikkeleissa, joka on päivitetty viimeksi, jossa päivitys on suuri tai merkittäviä. Kunkin päivitetyn artikkelin karkea katkelma korotuksia lisätty teksti tulee näkyviin. Artikkelit on päivitetty **2016-10-05** **2016-08-22** päivämäärävälillä.

| &nbsp; | Artikkelissa | Päivitetty teksti, koodikatkelman | Päivitetty, kun |
| --: | :-- | :-- | :-- |
| 12 | [Azure tietojen Factory - .NET API muutosloki](data-factory-api-change-log.md) | Tässä artikkelissa on tietoja muutokset Azure tietojen Factory SDK tietyn versiossa. Voit etsiä uusimmat NuGet paketin Azure Data Factory tähän (https://www.nuget.org/packages/Microsoft.Azure.Management.DataFactories) **versio 4.11.0** ominaisuus lisäykset: / linkitetyistä seuraavanlaisia on lisätty: - OnPremisesMongoDbLinkedService (https://msdn.microsoft.com/library/mt765129.aspx) - AmazonRedshiftLinkedService (https://msdn.microsoft.com/library/mt765121.aspx) - AwsAccessKeyLinkedService (https://msdn.microsoft.com/library/mt765144.aspx) / tietojoukko seuraavanlaisia on lisätty: - MongoDbCollectionDataset (https://msdn.microsoft.com/library/mt765145.aspx) - AmazonS3Dataset (https://msdn.microsoft.com/library/mt765112.aspx) / kopioi lähde seuraavanlaisia on lisätty :-MongoDbSource (https://msdn.microsoft.com/en-US/library/mt765123.aspx) **versio 4.10.0** / seuraavia valinnaisia ominaisuuksia on lisätty TekstinMuoto:-suksien | 2016-09-22 |
| 13 | [Tietojen siirtäminen ja sieltä pois Azure Blob-objektien käyttäminen Azure Data Factory](data-factory-azure-blob-connector.md) |  / copyBehavior / määrittää kopio, kun lähde on BlobSource tai tiedostojärjestelmässä.  / **PreserveHierarchy:** säilyttää kohdekansio tiedoston hierarkiaa. Tietolähteen kansio lähdetiedoston suhteellinen polku on sama kuin kohde kansioon kohdetiedosto suhteellisen polun... BR /... BR /. **FlattenHierarchy:** lähde-kansiosta kaikki tiedostot ovat kohdekansio ensimmäisen tason. Kohde-tiedostonimessä on luotu automaattinen nimi. .br /... BR /. **MergeFiles: (oletus)** yhdistää yhden tiedoston kaikkien tiedostojen lähde-kansiosta. Jos tiedosto-Blob-objektien nimi on määritetty, yhdistetyt tiedostonimen olisi on määritetty nimi Muussa tapauksessa olisi automaattisesti luodut tiedostonimi.  / Ei / **BlobSource** tukee myös nämä kaksi ominaisuutta yhteensopivuus aiempien versioiden kanssa. / **treatEmptyAsNull**: määrittää, käsittele null tai tyhjä merkkijono kuin null-arvon. / **skipHeaderLineCount** - määrittää, kuinka monta riviä on ohitettu. Se on käytettävissä vain kun syötteen tietojoukko käyttää TekstinMuoto. Vastaavasti **BlobSink** tukee ila | 2016-09-28 |
| 14 | [Luo ennakoivan putkistot Azure koneen Learning tehtävien avulla](data-factory-azure-ml-batch-execution-activity.md) | **Web-palvelu edellyttää useita syötteiden** Jos WWW-palvelun kestää useita syötteiden, sijaan **webServiceInput** **webServiceInputs** -ominaisuuden avulla. Tietojoukkoja, joihin viitataan **webServiceInputs** on myös sisällytettävä tehtävä- **syötteitä**. Azure ML-kokeessa-web-palvelu syötteen ja portit ja yleiset parametrit on oletusnimet ("input1", "input2"), jotka voit mukauttaa. Käytät webServiceInputs, webServiceOutputs ja globalParameters asetukset nimet on oltava täsmälleen samat kokeet nimet. Voit tarkastella otoksen pyynnön paketti erä suorittamisen Ohjesivun Azure ML päätepisteelle vahvistamiseksi odotettu yhdistämismääritys.    {"nimi": "PredictivePipeline", "ominaisuudet": {"kuvaus": "Käytä AzureML mallin", "toiminnot": {"nimi": "MLActivity", "tyyppi": "AzureMLBatchExecution", "kuvaus": "analyysi tekstinsyöttö erän syöte", "syöttää": {"nimi": "inputDataset1"}, {"nimi": "inputDatase | 2016-09-13 |
| 15 | [Kopioi tehtävän suorituskyky ja säätäminen opas](data-factory-copy-activity-performance.md) | 1. **Muodosta perusaikataulun**. Suunnitteluvaiheessa Testaa yhteyttä myyntijakso käyttämällä Kopioi tehtävän edustavan tietojen otoksen perusteella. Tietoja tehdas viipaloiminen mallin (tiedot-factory-ajoitus-ja-execution.md time-series-datasets-and-data-slices) avulla voit rajoittaa tiedot, joiden kanssa teet yhteistyötä.  Kerää suoritusaika ja suorituskyvyn **Seuranta ja hallinta-sovellus**. Valitse **näytön ja hallitse** Factory tietojen lukeminen aloitussivulla. Valitse puunäkymässä **tulosteen tietojoukko**. Valitse Kopioi tehtävän suorittaminen **Toiminta Windows** -luettelossa. **Tehtävän Windows** on lueteltu kopioi tehtävän keston ja tiedot, jotka on kopioitu kokoa. Siirtonopeuden näkyy **tehtävän ikkunan**Resurssienhallinnassa. Saat lisätietoja sovelluksen ohjeaiheessa näyttö ja Azure Data Factory putkistot hallinta seuranta ja hallinta-sovellus (tiedot-factory-näyttö-hallinta-app.md) avulla.  ! Tehtävän suorittaminen (./media/data-factory-copy-activity-performance/mmapp-activity-run-details.pn tiedot | 2016-09-27 |
| 16 | [Opetusohjelma: Luo putkijohto kopioi aktiviteettiin Visual Studiossa](data-factory-copy-activity-tutorial-using-visual-studio.md) |   Huomaa seuraavat seikat:-tietojoukko **tyyppi** on määritetty **AzureBlob**.     - **linkedServiceName** on määritetty **AzureStorageLinkedService**. Olet luonut linkitetyt palvelua vaiheessa 2.     - **kansiopolku** on määritetty **adftutorial** säilö. Voit myös määrittää blob-kansioon käyttämällä **fileName** -ominaisuuden nimi. Koska määrittämättä blob nimi, kaikki Blob-objektien säilössä tiedot pidetään kenttään annettavat tiedot.    -muodon **tyyppi** on määritetty **TekstinMuoto** - on kaksi kenttää tekstin tiedoston ΓÇô **Etunimi** ja **Sukunimi** ΓÇô erotettu toisistaan pilkulla-merkki (**columnDelimiter**) - **käytettävyys** on määritetty **kerran tunnissa** (**korkojakso** on määritetty **Tunti** ja **väli** on määritetty **1**). Tämän vuoksi Data Factory etsitään syöttötiedot tunnissa blob-säilö (**adftutorial**) määrittämääsi pääkansioon.  Jos et määritä **Tiedostonimi** **syötteen** tietojoukko, kaikki tiedostot ja BLOB syötteen kansiosta (**kansiopolku**) ovat consid | 2016-09 – 29 |
| 17 | [Luo, seurata ja hallita Azure tietojen tehtaan tietojen Factory .NET SDK: N avulla](data-factory-create-data-factories-programmatically.md) | Huomautus tunnus ja salasana (asiakkaan salaisuus) ja käyttää sitä vaiheittaisissa ohjeissa. **Hae Azure tilauksen ja vuokraajan tunnukset** Jos sinulla ei ole asennettu tietokoneeseesi PowerShellin uusimman version, noudata asentaminen ja määrittäminen PowerShellin Azure (... / powershell-asennus-configure.md) artikkelissa asennusohjeet. 1. Käynnistä PowerShellin Azure ja suorittamalla seuraavan komennon 2. Suorita seuraava komento ja kirjoita käyttäjänimi ja salasana, jonka avulla Kirjaudu Azure-portaaliin.         Kirjautuminen AzureRmAccount Jos käytössäsi on vain yksi tähän tiliin liittyvät Azure tilaus, sinun ei tarvitse suorittaa seuraavat kaksi vaihetta. 3. Suorita seuraava komento voit tarkastella kaikkien tilausten tilin.       Get-AzureRmSubscription 4. Suorita seuraava komento, jota haluat käyttää tilaus. Korvaa **NameOfAzureSubscription** Azure tilauksen nimeä.       Hae AzureRmSubscription - SubscriptionName NameOfAzureSubscription / määrittäminen AzureRmCo | 2016-09 – 14 |
| 18 | [Putkistot ja Azure Data Factory toimintoja](data-factory-create-pipelines.md) |       "Käynnistä-:" 2016-07-12T00:00:00Z ","loppu":" 2016-07-13T00:00:00Z "}} Huomaa seuraavat seikat: / toiminnot-osassa on vain yksi tehtävä, jonka **tyyppi** on määritetty **kopio**. / Syöte tehtävän on määritetty **InputDataset** ja tehtävän tulos on määritetty **OutputDataset**. / Osassa **typeProperties** **BlobSource** on määritetty lähteen tyyppi ja **SqlSink** on määritetty käsittelytoiminto tyyppi. Valmis vaiheista luomisesta tähän myyntijakso-kohdassa opetusohjelma: tietojen kopioiminen Blob-objektien tallennustilaan SQL-tietokanta (data-factory-copy-data-from-azure-blob-storage-to-sql-database.md). **Esimerkki muunnos myyntijakso** Seuraava esimerkki putkijohto on yksi tehtävä tyypin **HDInsightHive** **Toiminnot** -osassa. Tässä esimerkissä HDInsight rakenteen tehtävän (tiedot-factory-rakenne-activity.md) muunnoksia suorittamalla komentosarja rakennetiedostoon Azure Hdinsightiin Hadoop-klusterissa Azure-Blob-säiliö tiedot.  {"nimi": "TransformPipeline", "p | 2016-09-27 |
| 19 | [Muuntaa Azure Data Factory tiedot](data-factory-data-transformation-activities.md) | Tietoja Factory tukee seuraavia tietoja muunnos toimintoja, jotka voidaan lisätä putkijohdot (tiedot-factory-luominen-pipelines.md) joko yksitellen tai ketjutettu kanssa toiseen tehtävään. .  AZURE. Huomautus vaiheittainen on vaiheittaiset ohjeet artikkelissa Create putkijohto ja rakenne-muunnos (data-factory-build-your-first-pipeline.md) artikkelissa. **Tehtävän HDInsight-rakenne** Data Factory putkijohto HDInsight-rakenne-tehtävän suorittaa omat kyselyt rakennetta tai tarvittaessa Windowsin tai Linux-pohjaiset HDInsight-klusterin. Artikkelissa rakenteen tehtävän (tiedot-factory-rakenne-activity.md) tässä aktiviteetti lisätietoja. **HDInsight Possu toiminta** Data Factory putkijohto HDInsight Possu aktiviteetti suorittaa omat kyselyt Possu tai tarvittaessa Windowsin tai Linux-pohjaiset HDInsight-klusterin. Artikkelissa Possu tehtävän (tiedot-factory-possu-activity.md) tässä aktiviteetti lisätietoja. **HDInsight MapReduce toiminta** Data Factory putkijohto HDInsight MapReduce aktiviteetti suorittaa MapReduce ohjelmat y | 2016-09-26 |
| 20 | [Tietoja Factory ajoittaminen ja suorittaminen](data-factory-scheduling-and-execution.md) | CopyActivity2 suoritetaan vain, jos CopyActivity1 on suoritettu onnistuneesti ja Dataset2 on käytettävissä. Tässä on esimerkki myyntijakso JSON: {"nimi": "ChainActivities", "ominaisuudet": {"kuvaus": "Suorita toiminnot järjestyksessä", "toiminnot": {"tyyppi": "Kopioi", "typeProperties": {"lähde-: {"tyyppi":"BlobSource"},"käsittelytoiminto": {"tyyppi":"BlobSink","copyBehavior":"PreserveHierarchy","writeBatchSize": 0,"writeBatchTimeout":" 00: 00:00 "}},"syötteiden": {"nimi":"Dataset1"},"tulostaa": {"nimi":"Dataset2"},"käytännön": {"aikakatkaisu":" 01: 00:00 "},"ajoituksen": {"korkojakso":"Tunnin"ja"väli": 1}" nimi ":"CopyFromBlob1ToBlob2","kuvaus":"Kopioiminen blob tiedot toiseen"}, {"tyyppi":"Kopioi","typeProperties": {"lähde-: {"tyyppi": "BlobSource"}, "käsittelytoiminto" : {"tyyppi": "BlobSink", "writeBatchSize": 0, "writeBatchTimeout": "00: 00:00"}}, ": | 2016-09-28 |





## <a name="tutorials"></a>Opetusohjelmat

| &nbsp; | Otsikko | Kuvaus |
| --: | :-- | :-- |
| 21 | [Opetusohjelma: Oman ensimmäisen myyntijakso prosessin tietoihin käyttämällä Hadoop-klusterin luominen](data-factory-build-your-first-pipeline.md) | Azure Data Factory Tässä opetusohjelmassa näytetään, miten voit luoda ja ajoittaa tietojen factory, joka käsittelee rakenteen komentosarjan käyttäminen Hadoop-klusterin tietoja. |
| 22 | [Opetusohjelma: Luo ensimmäinen factory Azure tietojen Azure Resurssienhallinta-mallin avulla](data-factory-build-your-first-pipeline-using-arm.md) | Tässä opetusohjelmassa luot otoksen Azure Data Factory putkijohto Azure Resurssienhallinta-mallin avulla. |
| 23 | [Opetusohjelma: Luo ensimmäinen factory Azure tietojen Azure-portaalissa](data-factory-build-your-first-pipeline-using-editor.md) | Tässä opetusohjelmassa luot otoksen Azure Data Factory putkijohto tiedot-Factory editorilla Azure-portaalissa. |
| 24 | [Opetusohjelma: Luo ensimmäinen factory Azure tietojen Azure PowerShellin avulla](data-factory-build-your-first-pipeline-using-powershell.md) | Tässä opetusohjelmassa luot otoksen Azure Data Factory putkijohto Azure PowerShellin avulla. |
| 25 | [Opetusohjelma: Muodosta Azure ensimmäisen tietojen factory Microsoft Visual Studiossa](data-factory-build-your-first-pipeline-using-vs.md) | Tässä opetusohjelmassa luominen Visual Studiossa otoksen Azure Data Factory putkijohto. |
| 26 | [Opetusohjelma: Luo putkijohto kopioi aktiviteettiin Azure-portaalissa](data-factory-copy-activity-tutorial-using-azure-portal.md) | Tässä opetusohjelmassa luot Azure Data Factory-myyntijakso kopioi aktiviteettiin editorilla tietojen Factory Azure-portaalissa. |
| 27 | [Opetusohjelma: Luo putkijohto kopioi aktiviteettiin Azure PowerShellin avulla](data-factory-copy-activity-tutorial-using-powershell.md) | Tässä opetusohjelmassa luot Azure Data Factory-myyntijakso kopioi aktiviteettiin Azure PowerShell-toiminnolla. |
| 28 | [Opetusohjelma: Luo putkijohto kopioi aktiviteettiin Visual Studiossa](data-factory-copy-activity-tutorial-using-visual-studio.md) | Tässä opetusohjelmassa luot Azure Data Factory-myyntijakso kopioi aktiviteettiin Visual Studion avulla. |
| 29 | [Tietojen kopioiminen SQL-tietokannan käyttäminen tietojen Factory Blob-objektien tallennustilaan](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) | Tässä opetusohjelmassa näytetään kopioi toimintojen käyttämisestä Azure Data Factory-myyntijakso tietojen kopioiminen Blob-objektien tallennustilaan SQL-tietokantaan. |
| 30 | [Opetusohjelma: Luo putkijohto ohjatulla Factory kopioiminen Kopioi aktiviteettiin](data-factory-copy-data-wizard-tutorial.md) | Tässä opetusohjelmassa luot Azure Data Factory-myyntijakso kopioi aktiviteettiin Data Factory tukemat kopioi ohjatun toiminnon avulla |



## <a name="data-movement"></a>Tietojen siirto

| &nbsp; | Otsikko | Kuvaus |
| --: | :-- | :-- |
| 31 | [Tietojen siirtäminen ja sieltä pois Azure Blob-objektien käyttäminen Azure Data Factory](data-factory-azure-blob-connector.md) | Opi kopioimaan Azure Data Factory-blob-tietoja. Käytä Microsoftin otoksen: tietojen kopioimisesta ja sieltä pois Azure-Blob-säiliö ja Azure SQL-tietokantaan. |
| 32 | [Tietojen siirtäminen ja sieltä pois Azure Lake Tietosäilölle Azure Data Factory käyttäminen](data-factory-azure-datalake-connector.md) | Lue, miten voit siirtää tietoja ja Azure Lake Tietosäilölle Azure Data Factory avulla |
| 33 | [Tietojen siirtäminen ja sieltä pois DocumentDB Azure Data Factory käyttäminen](data-factory-azure-documentdb-connector.md) | Lue, miten tietojen siirtäminen tai käyttämällä Azure Data Factory Azure DocumentDB kokoelmasta |
| 34 | [Tietojen siirtäminen ja sieltä pois Azure SQL-tietokanta Azure Data Factory](data-factory-azure-sql-connector.md) | Opettele siirtämään tiedot ja Azure SQL-tietokanta Azure Data Factory. |
| 35 | [Tietojen siirtäminen ja sieltä pois Azure SQL-tietovarasto Azure Data Factory käyttäminen](data-factory-azure-sql-data-warehouse-connector.md) | Voit siirtää tietoja, koska Azure SQL-tietovarasto Azure Data Factory käyttäminen |
| 36 | [Siirrä tiedot ja Azure-taulukosta Azure Data Factory](data-factory-azure-table-connector.md) | Opettele siirtämään tiedot ja käyttämällä Azure Data Factory Azure-taulukkotallennus. |
| 37 | [Kopioi tehtävän suorituskyky ja säätäminen opas](data-factory-copy-activity-performance.md) | Lisätietoja avaimen tekijät, jotka vaikuttaa suorituskykyyn tietojen siirto-Azure Data Factory, kun käytät Kopioi tehtävän. |
| 38 | [Tietojen siirtäminen käyttämällä Kopioi tehtävä](data-factory-data-movement-activities.md) | Lisätietoja tietojen siirto-tietojen Factory putkistot: tietojen siirtäminen cloud stores, sekä paikallisen säilön ja cloud kaupan välillä. Kopioi toimintojen käyttäminen |
| 39 | [Data Management Gateway julkaisutiedot](data-factory-gateway-release-notes.md) | Data Management Gatewayn tory julkaisutiedot |
| 40 | [Tietojen siirtäminen paikalliseen HDFS Azure Data Factory avulla](data-factory-hdfs-connector.md) | Voit siirtää tietoja käyttämällä Azure Data Factory paikallisen HDFS tietoja. |
| 41 | [Seurata ja hallita Azure Data Factory putkistot uusi seuranta ja hallinta-sovelluksen avulla](data-factory-monitor-manage-app.md) | Lue, miten voit seurata ja hallita Azure tietojen tehtaan ja putkistot seuranta ja hallinta-sovelluksen avulla. |
| 42 | [Tietojen siirtäminen paikallisesti ja Data Management Gatewayn pilveen](data-factory-move-data-between-onprem-and-cloud.md) | Tietojen siirtäminen paikallisen käyttöön ja määrittää yhdyskäytävän tiedot. Azure Data Factory Data Management Gatewayn avulla voit siirtää tietoja. |
| 43 | [Siirrä tiedot OData-tietolähteen avulla Azure Data Factory](data-factory-odata-connector.md) | Lisätietoja tietojen siirtäminen käyttämällä Azure Data Factory OData lähteistä. |
| 44 | [Siirry Azure Data Factory käyttäminen tietojen ODBC-tiedot stores](data-factory-odbc-connector.md) | Voit siirtää tietoja ODBC-tiedot stores käyttämällä Azure Data Factory tietoja. |
| 45 | [Siirrä tiedot DB2 käyttämällä Azure Data Factory](data-factory-onprem-db2-connector.md) | Lisätietoja on artikkelissa tietojen siirtäminen DB2-tietokannasta Azure Data Factory |
| 46 | [Tietojen siirtäminen ja sieltä pois paikallisen-tiedostojärjestelmässä Azure Data Factory avulla](data-factory-onprem-file-system-connector.md) | Lue, miten voit siirtää tietoja ja paikallisen tiedoston sähköpostijärjestelmään käyttämällä Azure Data Factory. |
| 47 | [Siirrä tiedot-MySQL Azure Data Factory käyttäminen](data-factory-onprem-mysql-connector.md) | Lisätietoja tietojen siirtäminen MySQL-tietokannasta Azure Data Factory. |
| 48 | [Siirrä tiedot ja paikallisen Oracle Azure Data Factory käyttäminen](data-factory-onprem-oracle-connector.md) | Lue, miten voit siirtää tietoja tai Oracle-tietokantaan, joka on paikallisen Azure Data Factory avulla. |
| 49 | [Siirrä tiedot käyttämällä Azure Data Factory PostgreSQL](data-factory-onprem-postgresql-connector.md) | Lisätietoja tietojen siirtäminen PostgreSQL-tietokannasta Azure Data Factory. |
| 50 | [Siirrä tiedot käyttämällä Azure Data Factory Sybase](data-factory-onprem-sybase-connector.md) | Lisätietoja tietojen siirtäminen Sybase-tietokannasta Azure Data Factory. |
| 51 | [Siirrä tiedot käyttämällä Azure Data Factory Teradata](data-factory-onprem-teradata-connector.md) | Lisätietoja Teradata-yhdistin Data Factory-palveluun, jolla voit siirtää tietoja Teradata-tietokanta |
| 52 | [Siirrä tiedot, ja SQL Server paikallisen tai IaaS (Azure AM) Azure Data Factory käyttäminen](data-factory-sqlserver-connector.md) | Lue lisätietoja siitä, miten voit siirtää tietoja, koska SQL Server-tietokantaan, joka on paikallisen tai käyttämällä Azure Data Factory Azure-AM. |
| 53 | [Siirrä tiedot käyttämällä Azure Data Factory Web taulukon lähde](data-factory-web-table-connector.md) | Voit siirtää tietoja paikallisen verkkosivua Azure Data Factory taulukon tietoja. |



## <a name="data-transformation"></a>Muuntaminen

| &nbsp; | Otsikko | Kuvaus |
| --: | :-- | :-- |
| 54 | [Luo ennakoivan putkistot Azure koneen Learning tehtävien avulla](data-factory-azure-ml-batch-execution-activity.md) | Kerrotaan, miten voit luoda luominen ennakoivan putkistot Azure Data Factory ja Azure koneen oppiminen |
| 55 | [Laske linkitetyn palvelut](data-factory-compute-linked-services.md) | Lue lisätietoja Laske enviornments, että voit tehdä Azure Data Factory putkistot-muunnos/prosessin tiedot. |
| 56 | [Käsittele suurissa tietojoukkoja Data Factory ja erä käyttäminen](data-factory-data-processing-using-batch.md) | Tässä artikkelissa käsitellään käsitellä Azure Data Factory-myyntijakso erittäin suuri tietomäärien Azure erän rinnakkain käsittely-ominaisuuksien avulla. |
| 57 | [Muuntaa Azure Data Factory tiedot](data-factory-data-transformation-activities.md) | Opettele transform Azure Data Factory Hadoop sekä tietokoneen oppiminen ja Azure tietojen järvi Analytics-tiedot tai prosessi. |
| 58 | [Hadoop Streaming tehtävä](data-factory-hadoop-streaming-activity.md) | Lue käyttämisestä Hadoop Streaming tehtävän Azure tietojen factory-toimimaan Hadoop Streaming ohjelmat-demand/your oman HDInsight-klusterin. |
| 59 | [Tehtävän rakenne](data-factory-hive-activity.md) | Lue käyttämisestä rakenne tehtävän Azure tietojen factory-toimimaan rakenteen kyselyt-demand/your oman HDInsight-klusterin. |
| 60 | [Käynnistää MapReduce ohjelmien tietojen Factory](data-factory-map-reduce.md) | Lue, miten voit käsitellä tietoja suorittamalla MapReduce ohjelmat Azure HDInsight-klusterissa Azure tietojen factory. |
| 61 | [Possu toiminta](data-factory-pig-activity.md) | Lue käyttämisestä Possu tehtävän Azure tietojen factory-suorittamaan Possu komentosarjoja-demand/your oman HDInsight-klusterin. |
| 62 | [Käynnistä ohjattu ohjelmien tietojen Factory](data-factory-spark.md) | Opettele käynnistää ohjattu ohjelmien käyttäminen MapReduce tehtävän Azure tiedot-factory. |
| 63 | [SQL Serverin tallennettu toimintosarja tehtävä](data-factory-stored-proc-activity.md) | Tietoja siitä, miten voit käyttää SQL Serverin tallennettu toimintosarja tehtävän käynnistää tallennetun toimintosarjan Azure SQL-tietokanta tai SQL Azure-tietovarasto Data Factory myyntijakso. |
| 64 | [Mukautettujen toimintojen käyttäminen Azure Data Factory-myyntijakso](data-factory-use-custom-activities.md) | Lue, miten niitä käytetään Azure Data Factory-putkijohto ja luo mukautettuja toimintoja. |
| 65 | [U-SQL-komentosarja suoritetaan Azure tietojen järvi Analytics Azure Data Factory](data-factory-usql-activity.md) | Lue, miten voit käsitellä tietoja suorittamalla U-SQL-komentosarjat Azure tietojen järvi Analytics Laske-palvelusta. |



## <a name="samples"></a>Esimerkkejä, joiden

| &nbsp; | Otsikko | Kuvaus |
| --: | :-- | :-- |
| 66 | [Azure tietojen Factory - objektit](data-factory-samples.md) | On esimerkkejä, joiden toimittaa tietoja Azure Data Factory-palvelussa. |



## <a name="use-cases"></a>Käyttötapauksiin

| &nbsp; | Otsikko | Kuvaus |
| --: | :-- | :-- |
| 67 | [Asiakkaan tapaustutkimukset](data-factory-customer-case-studies.md) | Lue lisätietoja siitä, miten joitakin asiakkaidemme olet käyttänyt Azure Data Factory. |
| 68 | [Käyttötapaus - asiakkaan profilointi](data-factory-customer-profiling-usecase.md) | Lue, miten Azure Data Factory käytti tietoihin perustuvien työnkulun luominen (myyntijakso), profiilin gaming asiakkaille. |
| 69 | [Käyttötapaus - tuotteen suositukset](data-factory-product-reco-usecase.md) | Lisätietoja toteutettu käyttämällä Azure Data Factory yhdessä muiden palveluiden käyttötapaus. |



## <a name="monitor-and-manage"></a>Seuranta ja hallinta

| &nbsp; | Otsikko | Kuvaus |
| --: | :-- | :-- |
| 70 | [Seurata ja hallita Azure Data Factory putkistot](data-factory-monitor-manage-pipelines.md) | Opettele käyttämään Azure-portaaliin ja PowerShellin Azure ja Azure tietojen tehtaan ja olet luonut putkistot hallinta. |



## <a name="sdk"></a>SDK-PAKETISSA

| &nbsp; | Otsikko | Kuvaus |
| --: | :-- | :-- |
| 71 | [Azure tietojen Factory - .NET API muutosloki](data-factory-api-change-log.md) | Tässä artikkelissa kuvataan uusimmat muutokset, lisäykset ominaisuus, virheenkorjauksia muille... .NET Ohjelmointirajapinta Azure Data Factory tietyn versiossa. |
| 72 | [Luo, seurata ja hallita Azure tietojen tehtaan tietojen Factory .NET SDK: N avulla](data-factory-create-data-factories-programmatically.md) | Opettele ohjelmallisesti luominen, valvoa ja tehtaan Azure tietojen hallinta tietojen Factory SDK avulla. |
| 73 | [Azure tietojen Factory Sovelluskehittäjän opas](data-factory-sdks.md) | Lisätietoja eri tapoja luoda ja seurata Azure tietojen tehtaan hallinta |



## <a name="miscellaneous"></a>Muut

| &nbsp; | Otsikko | Kuvaus |
| --: | :-- | :-- |
| 74 | [Azure tietojen Factory - usein kysytyt kysymykset](data-factory-faq.md) | Usein kysyttyjä kysymyksiä Azure Data Factory. |
| 75 | [Azure tietojen Factory - Funktiot ja Järjestelmämuuttujat](data-factory-functions-variables.md) | Sisältää luettelon Azure Data Factory Funktiot ja Järjestelmämuuttujat |
| 76 | [Azure tietojen Factory - säännöt nimeäminen](data-factory-naming-rules.md) | Tässä artikkelissa kuvataan Data Factory kohteiden nimeämiskäytäntö säännöt. |
| 77 | [Data Factory-ongelmien vianmääritys](data-factory-troubleshoot.md) | Lue, miten käyttämällä Azure Data Factory ongelmien vianmääritystä. |

