<properties
    pageTitle="Luo ja lataa tiedot rakenteen taulukoiksi Blob-objektien tallennustilaan | Microsoft Azure"
    description="Rakenne-taulukoiden luominen ja Blob-objektien rakenne taulukoiden tietojen lataaminen"
    services="machine-learning,storage"
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
    ms.date="09/14/2016"
    ms.author="bradsev" />


#<a name="create-and-load-data-into-hive-tables-from-azure-blob-storage"></a>Luo ja lataa tiedot rakenteen taulukoiksi Azure-blob-säiliö

Tässä ohjeartikkelissa esitellään yleinen rakenne-kyselyjä, jotka rakenteen taulukoiden luominen ja tietojen lataaminen Azure-blob-säiliö. Jotkin ohjeet toimitetaan myös jakaminen rakenteen taulukoita ja käyttämisestä optimoitu rivin Sarakemuoto (ORC) voit parantaa kyselyn suorituskykyä muotoilu.

Tässä **valikossa** linkkejä ohjeaiheisiin, joissa kerrotaan, kuinka voit ingest tietojen tuominen kohde-ympäristössä, jossa tiedot voidaan tallentaa ja käsitellä aikana-työryhmän tiedot tiede prosessi (TDSP).

[AZURE.INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]


## <a name="prerequisites"></a>Edellytykset
Tässä artikkelissa oletetaan, että sinulla on:

* Luoda Azure-tallennustilan tilin. Jos tarvitset ohjeita, katso [Lisätietoja Azure-tallennustilan tilit](../storage/storage-create-storage-account.md). 
* Valmisteltu mukautetun Hadoop-klusterin HDInsight-palvelussa.  Jos tarvitset ohjeita, katso [mukauttaminen Azure Hdinsightiin Hadoop klustereiden kehittyneen analyysin varten](machine-learning-data-science-customize-hadoop-cluster.md).
* Käytössä etäkäyttöpalvelimen klusterin kirjautuneena ja avata Hadoop komentorivin konsolin. Jos tarvitset ohjeita, on artikkelissa [Head solmu, Hadoop klusterin](machine-learning-data-science-customize-hadoop-cluster.md#headnode).

## <a name="upload-data-to-azure-blob-storage"></a>Tietojen lataaminen Azure-blob-säiliö
Jos olet luonut Azure virtuaalikoneen noudattamalla seuraavia ohjeita [Azure virtual-tietokoneen kehittyneen analyysin määrittäminen](machine-learning-data-science-setup-virtual-machine.md), tämä komentosarjatiedosto olisi on ladattu *C:\\käyttäjien\\\<käyttäjänimi\>\\asiakirjojen\\tietojen tiede komentosarjojen* hakemiston virtuaalikoneen. Nämä rakenne-kyselyt edellyttävät vain Kytke Omat tiedot rakenteen ja Azure-blob storage määritys on valmis lähettämistä varten tarvittavat kentät.

Oletetaan, että rakenne-taulukoiden tiedot **pakkaamattomat** sarakemuodossa ja tiedot on ladattu oletusarvo (tai Lisää)-tallennustilan tilin käyttämän Hadoop-klusterin säilö.

Jos haluat harjoitella **Kokousesitelmän taksin työmatkan tietoja**, on:

- **Lataa** 24 [Kokousesitelmän taksin työmatkan](http://www.andresmh.com/nyctaxitrips) datatiedostot (12 työmatkan tiedostoja ja 12 maksun tiedostot)
- **Pura** kaikki tiedostot .csv-tiedostoon ja valitse sitten
- **Lataa** ne Azure-tallennustilan tilin, joka on luotu menetelmällä oletusarvo (tai asianmukaiset säilö) noudatat [mukauttaminen Azure Hdinsightiin Hadoop klustereiden Advanced Analytics-prosessi-ja tekniikka](machine-learning-data-science-customize-hadoop-cluster.md) . Prosessin .csv-tiedostojen lataaminen tallennustilan tilin oletusarvo-säilö löytyy tämän [sivun](machine-learning-data-science-process-hive-walkthrough.md#upload).


## <a name="submit"></a>Voit lähettää rakenne-kyselyt

Rakenne-kyselyt voi lähettää käyttämällä:

1. [Lähetä headnode Hadoop-klusterin kyselyjen rakenteen kautta Hadoop komentoriviltä](#headnode)
2. [Lähetä rakenteen kyselyjen rakenne-editorissa](#hive-editor)
3. [Lähetä rakenteen kyselyt ja Azure PowerShell-komennot](#ps)

Rakenne on SQL kaltaisessa. Jos tunnet SQL, saatat huomata [SQL käyttäjien Cheat taulukon rakenne](http://hortonworks.com/wp-content/uploads/2013/05/hql_cheat_sheet.pdf) hyödyllisiä.

Kyselyn rakenne lähettäessään voit myös määrittää tulosteen rakenteen kyselyjen kohde, onko kyseessä näytössä tai paikallinen tiedosto pään solmun tai Azure-blob.


###<a name="headnode"></a>1. lähettää headnode Hadoop-klusterin kyselyjen rakenteen kautta Hadoop komentoriviltä

Jos kyselyn rakenne on monimutkainen, lähettämistä suoraan pään solmun Hadoop-klusterin yleensä liidejä, voit ottaa nopeammin kuin lähettämistä rakenne muokkaajan tai Azure PowerShell-komentosarjojen kanssa.

Hadoop-klusterin pään solmu kirjautuminen, Avaa Hadoop komentoriviltä pään solmun työpöydällä, ja kirjoita kehote `cd %hive_home%\bin`.

Sinulla voi lähettää rakenteen kyselyjen Hadoop komentoriviltä kolmella tavalla:

* suoraan
* .hql tiedostojen käyttäminen
* Rakenne-komento, konsolissa

#### <a name="submit-hive-queries-directly-in-hadoop-command-line"></a>Lähetä rakenteen kyselyjen suoraan Hadoop komentorivi.

Voit suorittaa komennon, kuten `hive -e "<your hive query>;` esittää yksinkertaisen rakenteen kyselyjen suoraan Hadoop komentorivi. Tässä on esimerkki, jossa punainen ruutu ympärille komento, joka lähettää kyselyn rakenne ja vihreä ruutu ympärille tuloste kyselyn rakenne.

![Työtilan luominen](./media/machine-learning-data-science-move-hive-tables/run-hive-queries-1.png)

#### <a name="submit-hive-queries-in-hql-files"></a>Lähetä .hql tiedostojen rakenteen kyselyissä

Kun kyselyn rakenne on monimutkaisempi ja useita viivoja, kyselyissä komentorivin tai rakenne-Komentokonsolin muokkaaminen ei ole käytännön. Löytämiseen on rakenne-kyselyt tallennetaan .hql tiedoston paikallisessa kansiossa pään solmun Hadoop-klusterin pään solmu tekstieditorissa avulla. Valitse rakenne-kyselyn .hql-tiedostossa voi lähettää käyttämällä `-f` argumentti seuraavasti:

    hive -f "<path to the .hql file>"

![Työtilan luominen](./media/machine-learning-data-science-move-hive-tables/run-hive-queries-3.png)


**Piilota edistymisen tila näytön Tulosta rakenne-kyselyt**

Oletusarvon mukaan kun kyselyn rakenne on lähetetty Hadoop komentorivin kartan/Pienennä työn edistymistä tulostetaan näytön. Jos et halua näytön Tulosta kartta ja vähennä projektin edistymisestä, voit käyttää argumentti `-S` ("S" isoilla kirjaimilla) komennossa rivi seuraavasti:

    hive -S -f "<path to the .hql file>"
    hive -S -e "<Hive queries>"

#### <a name="submit-hive-queries-in-hive-command-console"></a>Lähetä rakenteen kyselyjen rakenteen komentorivillä.

Voit kirjoittaa rakenne-komento-konsolin myös ensin suorittamalla komennon `hive` Hadoop komentorivi- ja lähetä sitten rakenne kyselyjen rakenteen komentorivillä. Tässä on esimerkki. Tässä esimerkissä kaksi punaiset ruudut Korosta Kirjoita rakenne-komento-konsolin komentojen ja rakenne-kyselyn lähetetty rakenteen komentorivillä tarpeen mukaan. Vihreä ruutu korostaa tulosteen rakenteen kyselystä.

![Työtilan luominen](./media/machine-learning-data-science-move-hive-tables/run-hive-queries-2.png)

Edellisessä kohdassa käytettyä tulosteen suoraan näytön rakenne kyselyn tuloksia. Voit myös kirjoittaa tulosteen paikalliseen tiedostoon pään solmun tai Azure-blob. Voit sitten analysoida rakenteen kyselyjen tulosteen muita työkaluja.

**Tulosteen paikallisen tiedoston rakenteen kyselyn tuloksia.**

Siirtää rakenteen kyselytulokset paikallisessa kansiossa pään solmun sinulla Lähetä rakenteen kysely Hadoop komentoriviltä seuraavasti:

    hive -e "<hive query>" > <local path in the head node>

Seuraavassa esimerkissä rakenteen kyselyn tulos on kirjoitettu tiedostoon `hivequeryoutput.txt` Directory `C:\apps\temp`.

![Työtilan luominen](./media/machine-learning-data-science-move-hive-tables/output-hive-results-1.png)

**Azure-blob tulosteen rakenteen kyselytulokset**

Voit myös siirtää Azure blob, Hadoop-klusterin oletusarvo-säilön rakenteen kyselytulokset. Tämä rakenne kysely on seuraavanlainen:

    insert overwrite directory wasb:///<directory within the default container> <select clause from ...>

Seuraavassa esimerkissä rakenteen kyselyn tulos on kirjoitettu blob-kansioon `queryoutputdir` Hadoop-klusterin oletusarvon säilöön. Tähän riittää antamaan kansionimen ilman blob-nimeä. Virhe ilmenee, jos antaisit hakemistosta ja Blob-objektien nimet, kuten `wasb:///queryoutputdir/queryoutput.txt`.

![Työtilan luominen](./media/machine-learning-data-science-move-hive-tables/output-hive-results-2.png)

Jos avaat Azure-tallennustilan Resurssienhallinnassa Hadoop-klusterin oletusarvo-säilö, näet tulosteen rakenteen kyselyn seuraavassa kuvassa esitetyllä tavalla. Voit käyttää suodatinta (korostettiin punainen ruutu) hakea vain Blob-objektien nimissä määritetyn kirjaimilla.

![Työtilan luominen](./media/machine-learning-data-science-move-hive-tables/output-hive-results-3.png)

###<a name="hive-editor"></a>2. lähettää rakenteen kyselyjen rakenne-editorissa

Voit käyttää myös kyselyn Console (rakenne-editori) kirjoittamalla URL-Osoitteen lomakkeen *https://&#60; Hadoop-klusterinimi >.azurehdinsight.net/Home/HiveEditor* selaimeen. Sinun on oltava kirjautunut sisään Katso tämä konsoli ja näin sinun pitää Hadoop klusterin tunnistetiedot tähän.

###<a name="ps"></a>3. Lähetä rakenteen kyselyt ja Azure PowerShell-komennot

PowerShellin avulla voit myös lähettää rakenteen kyselyt. Lisätietoja on kohdassa [Lähetä rakenne työt PowerShellin avulla](../hdinsight/hdinsight-submit-hadoop-jobs-programmatically.md#hive-powershell).


## <a name="create-tables"></a>Tietokannan rakenne ja taulukoiden luominen

Rakenne-kyselyjen jaettuja [Github säilöön](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_create_db_tbls_load_data_generic.hql) , ja sen voi ladata sieltä.

Seuraavassa on rakenne-kysely, joka luo taulukon rakennetta.

    create database if not exists <database name>;
    CREATE EXTERNAL TABLE if not exists <database name>.<table name>
    (
        field1 string,
        field2 int,
        field3 float,
        field4 double,
        ...,
        fieldN string
    )
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>' lines terminated by '<line separator>'
    STORED AS TEXTFILE LOCATION '<storage location>' TBLPROPERTIES("skip.header.line.count"="1");

Seuraavassa on kenttiä, jotka haluat laajennus ja määrityksiä kuvauksia:

- **& #60; tietokannan nimi >**:, jonka haluat luoda tietokannan nimi. Jos haluat käyttää oletusarvon tietokantaa, kyselyn *... tietokannan luominen* voi jättää pois.
- **& #60; taulukkonimi >**: taulukon, jonka haluat luoda määritetyn tietokannan nimi. Jos haluat käyttää oletusarvo-tietokanta, taulukko voidaan käsiteltäväksi suoraan *& #60; taulukkonimi >* ilman & #60; tietokannan nimi >.
- **& #60; kenttäerotin >**: erotin, joka erottaa datatiedostoon ladattava rakenne-taulukon kentät.
- **& #60; rivin erotin >**: erotin, joka erottaa datatiedostoon rivit.
- **& #60; tallennuspaikka >**: Azure tallennustilan sijainti, johon haluat tallentaa tiedot rakenteen taulukoiden. Jos et määritä *sijainti & #60; tallennuspaikka >*, tietokanta ja taulukot on tallennettu *rakenne ja varasto/* oletusarvoisesti rakenne-klusterin oletusarvon säilön hakemistoon. Jos haluat määrittää tallennussijainnin tallennuspaikan on oltava oletusarvo-säilön tietokanta ja taulukot. Tämä sijainti on tarkoitettu sijainnin suhteessa oletusarvo-säilö muoto klusterin *' wasb: / / / & #60; kansiota 1 > /'* tai *' wasb: / / / & #60; kansiota 1 > / & #60; kansion 2 > /'*jne. Kun kysely suoritetaan, suhteellinen kansioita luodaan oletusarvon säilöön.
- **TBLPROPERTIES("Skip.Header.Line.Count"="1")**: Jos tiedosto on otsikkorivi, sinun on lisättävä tämän ominaisuuden **lopussa** *Luo taulukko* -kyselyn. Muussa tapauksessa otsikkorivi ladataan tietueeksi taulukkoon. Jos tiedosto ei ole otsikkorivi, määritysten jättää pois kyselyssä.

## <a name="load-data"></a>Lataa tiedot rakenteen taulukoihin
Seuraavassa on rakenne-kysely, joka lataa tiedot taulukkoon rakenne.

    LOAD DATA INPATH '<path to blob data>' INTO TABLE <database name>.<table name>;

- **& #60; blob-tietojen polku >**: Jos ladattava taulukko rakenne blob-tiedosto on HDInsight Hadoop-klusterin oletusarvo-säilössä *& #60; blob-tietojen polku >* pitäisi olla muodossa *' wasb: / / / & #60; directory tähän säilöön > / & #60; blob tiedostonimi >'*. Blob-tiedoston voi olla myös muita säilössä HDInsight Hadoop-klusterin. Tässä tapauksessa *& #60; blob-tietojen polku >* pitäisi olla muodossa *' wasb: / / & #60; säilö name>@&#60;storage tilin nimi >.blob.core.windows.net/ & #60; blob tiedostonimi >'*.

    >[AZURE.NOTE] Ladattava rakennetaulukko blob-tietojen on oltava oletus- tai Lisää säilö-tallennustilan tilin Hadoop-klusterin. Muussa tapauksessa kyselyn *Lataaminen* epäonnistuu complaining, että se ei voi käyttää tietoja.


## <a name="partition-orc"></a>Laajennettu aiheita: osioitu taulukon ja kaupan rakenteen tietojen ORC

Jos tiedot on suuri, taulukon jakaminen on kätevä kyselyitä, jotka on vain muutama osioiden taulukon tarkistamaan. Esimerkiksi on järkevää osion lokitiedot sivuston päivämäärän mukaan.

Lisäksi jakaminen rakenteen taulukot, on myös hyödyllisiin rakenne-tiedot tallennetaan optimoitu rivin Sarakemuoto (ORC)-muodossa. Lisätietoja ORC muotoilu on artikkelissa <a href="https://cwiki.apache.org/confluence/display/Hive/LanguageManual+ORC#LanguageManualORC-ORCFiles" target="_blank">käyttämällä ORC tiedostoja parantaa suorituskykyä, kun rakenne on lukeminen, kirjoittaminen, ja tietojen käsittelemistä</a>.

### <a name="partitioned-table"></a>Osioitu taulukko
Seuraavassa on rakenne-kysely, joka luo osioitua taulukon ja lataa tiedot siihen.

    CREATE EXTERNAL TABLE IF NOT EXISTS <database name>.<table name>
    (field1 string,
    ...
    fieldN string
    )
    PARTITIONED BY (<partitionfieldname> vartype) ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>'
         lines terminated by '<line separator>' TBLPROPERTIES("skip.header.line.count"="1");
    LOAD DATA INPATH '<path to the source file>' INTO TABLE <database name>.<partitioned table name>
        PARTITION (<partitionfieldname>=<partitionfieldvalue>);

Kun kysely suoritetaan osioitua taulukot, kannattaa lisätä ehtoa osion **alkuun** `where` kuin tämä lause on parannettu tehon haun merkittävästi.

    select
        field1, field2, ..., fieldN
    from <database name>.<partitioned table name>
    where <partitionfieldname>=<partitionfieldvalue> and ...;

### <a name="orc"></a>Rakenne-tiedot tallennetaan ORC muoto

Ei suoraan ladata tietoja Blob-objektien tallennustilaan rakenteen taulukoiksi, joka on tallennettu ORC-muodossa. Seuraavat vaiheet, jotka sinun on suoritettava, jotta voit ladata tiedot Azure BLOB ORC-muodossa tallennettu rakenne-taulukoihin.

Luoda Blob-objektien tallennustilaan ulkoisen taulukon **TALLENNETTU AS TEXTFILE** ja lataa tiedot taulukkoon.

        CREATE EXTERNAL TABLE IF NOT EXISTS <database name>.<external textfile table name>
        (
            field1 string,
            field2 int,
            ...
            fieldN date
        )
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>'
            lines terminated by '<line separator>' STORED AS TEXTFILE
            LOCATION 'wasb:///<directory in Azure blob>' TBLPROPERTIES("skip.header.line.count"="1");

        LOAD DATA INPATH '<path to the source file>' INTO TABLE <database name>.<table name>;

Luo sisäinen taulukko vaiheessa 1, jossa saman kenttäerotin ulkoisen taulukon samaan rakenteeseen ja rakenne-tiedot tallennetaan ORC-muodossa.

        CREATE TABLE IF NOT EXISTS <database name>.<ORC table name>
        (
            field1 string,
            field2 int,
            ...
            fieldN date
        )
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '<field separator>' STORED AS ORC;

Valitse vaiheessa 1 ulkoisen taulukon tiedot ja lisää ORC-taulukkoon

        INSERT OVERWRITE TABLE <database name>.<ORC table name>
            SELECT * FROM <database name>.<external textfile table name>;

>[AZURE.NOTE] Jos TEXTFILE taulukon *& #60; tietokannan nimi >. & #60; ulkoiset textfile taulukkonimi >* on osioita, vaihe 3 `SELECT * FROM <database name>.<external textfile table name>` -komento valitsee osion muuttujan palautetut tietojoukon kenttänä. Lisäät kuvan *& #60; tietokannan nimi >. & #60; ORC taulukkonimi >* epäonnistuu, koska *& #60; tietokannan nimi >. & #60; ORC taulukkonimi >* ei ole osion muuttujan taulukkorakennetta kenttänä. Tässä tapauksessa sinun on valittava erityisesti kentät, jotka lisätään *& #60; tietokannan nimi >. & #60; ORC taulukkonimi >* seuraavasti:

        INSERT OVERWRITE TABLE <database name>.<ORC table name> PARTITION (<partition variable>=<partition value>)
           SELECT field1, field2, ..., fieldN
           FROM <database name>.<external textfile table name>
           WHERE <partition variable>=<partition value>;

Se on turvallista pudota *& #60; ulkoiset textfile taulukkonimi >* kun käyttämällä seuraavan kyselyn jälkeen kaikki tiedot on lisätty *& #60; tietokannan nimi >. & #60; ORC taulukkonimi >*:

        DROP TABLE IF EXISTS <database name>.<external textfile table name>;

Kun näiden ohjeiden tulisi olla taulukko, jossa tiedot ryhtyä käyttämään ORC-muodossa.  
