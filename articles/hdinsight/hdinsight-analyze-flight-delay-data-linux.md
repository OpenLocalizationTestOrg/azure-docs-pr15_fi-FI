<properties 
    pageTitle="Rakenne-Linux-pohjaiset HDInsight lennon viive tietojen analysoiminen | Microsoft Azure" 
    description="Opettele käyttämään rakenteen Linux-pohjaiset HDInsight-lentojen tietojen analysointia ja sitten Vie tiedot käyttämällä Sqoop SQL-tietokantaan." 
    services="hdinsight" 
    documentationCenter="" 
    authors="Blackmist" 
    manager="jhubbard" 
    editor="cgronlun"
    tags="azure-portal"/>

<tags 
    ms.service="hdinsight" 
    ms.workload="big-data" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/11/2016" 
    ms.author="larryfr"/>

#<a name="analyze-flight-delay-data-by-using-hive-in-hdinsight"></a>Lentotietoihin viiveen analysoiminen käyttämällä HDInsight-rakenne

Lue, miten voit analysoida viive lentotietoihin rakenteen käyttäminen Linux-pohjaiset HDInsight sitten Vie tiedot käyttämällä käyttämällä Sqoop Azure SQL-tietokanta.

> [AZURE.NOTE] Kun tämän asiakirjan yksittäiset osat voidaan käyttää Windows-pohjaisesta HDInsight varausyksiköt (Python ja esimerkiksi rakenne), jossa monessa vaiheessa määräytyvät Linux-pohjaiset klustereihin. Katso kohdasta, jotka toimivat Windows-pohjaisesta klusterin [Analysoi viive lentotietoihin käyttämällä HDInsight-rakenne](hdinsight-analyze-flight-delay-data.md)

###<a name="prerequisites"></a>Edellytykset

Ennen kuin aloitat Tässä opetusohjelmassa, sinun on oltava seuraavasti:

- **Azure-tilaus**. Katso [Hae Azure maksuttoman kokeiluversion](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- __HDInsight-klusterin__. Saat ohjeet uuden Linux-pohjaiset HDInsight-klusterin luominen [Hadoop Linux HDInsight-rakenne ja käytön aloittaminen](hdinsight-hadoop-linux-tutorial-get-started.md) .

- __Azure SQL-tietokanta__. Voit käyttää kohde tietosäilö Azure SQL-tietokantaan. Jos sinulla ei ole jo SQL-tietokantaan, katso lisätietoja artikkelista [SQL-tietokanta-opetusohjelma: SQL-tietokannan luominen minuutteina](../sql-database/sql-database-get-started.md).

- __Azure CLI__. Jos et ole asentanut Azure-CLI, saat lisää ohjeita [Asenna ja Määritä Azure-CLI](../xplat-cli-install.md) .

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]


##<a name="download-the-flight-data"></a>Lataa lentotietoihin

1. Siirry [oheis- ja tekniikalla hallinta-Bureau kuljetus tilastojen][rita-website].
2. Valitse-sivulla seuraavat arvot:

  	| Nimi | Arvo |
  	| ---- | ---- |
  	| Suodatus vuoden | 2013 |
  	| Suodattaa piste | Tammikuussa |
  	| Kentät | Vuosi-FlightDate, UniqueCarrier, Carrier, FlightNum, OriginAirportID, Origin, OriginCityName, OriginState, DestAirportID, Dest, DestCityName, DestState, DepDelayMinutes, ArrDelay, ArrDelayMinutes, CarrierDelay, WeatherDelay, NASDelay, SecurityDelay, LateAircraftDelay. Poista kaikki muut kentät |

3. Valitse **Lataa**. 

##<a name="upload-the-data"></a>Lataa tiedot

1. Käytä seuraavaa komentoa zip-tiedoston lataaminen HDInsight-klusterin pään solmu:

        scp FILENAME.csv USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:

    Korvaa zip-tiedoston nimi __tiedoston nimi__ . Korvaa HDInsight-klusterin SSH-kirjautumisen __käyttäjänimi__ . Korvaa CLUSTERNAME HDInsight-klusterin nimen.
    
    > [AZURE.NOTE] Jos käytät salasanaa SSH kirjautumisen, voit pyydetään salasanaa. Jos olet käyttänyt julkisella avaimella, voit joutua käyttämään `-i` parametri ja määritä polku vastaava yksityinen avain. Esimerkiksi `scp -i ~/.ssh/id_rsa FILENAME.csv USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:`.

2. Kun lataus on valmis, muodostaa yhteyttä klusterin SSH avulla:

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
        
    Lisätietoja Linux-pohjaiset HDInsight SSH käyttämisestä on seuraavissa artikkeleissa:
    
    * [Linux-pohjaiset Hadoop HDInsight Linux, Unix tai OS X-SSH käyttäminen](hdinsight-hadoop-linux-use-ssh-unix.md)

    * [SSH käyttäminen Linux-pohjaiset Hadoop-HDInsight Windows](hdinsight-hadoop-linux-use-ssh-windows.md)
    
3. Kun yhteys on muodostettu, käyttämällä seuraavaa purkaminen .zip-tiedosto:

        unzip FILENAME.zip
    
    Tämä purkaa .csv-tiedosto, joka on suunnilleen 60 Megatavun kokoisia.
    
4. Seuraavat avulla voit luoda uuden kansion WASB (HDInsight käyttämän jaettujen tietojen-kauppa) ja kopioi tiedosto:

    hdfs dfs - mkdir -p /tutorials/flightdelays/data hdfs dfs-valitseminen FILENAME.csv/opetusohjelmat/flightdelays/tietojen /
    
##<a name="create-and-run-the-hiveql"></a>Luoda ja suorittaa HiveQL

Seuraavien vaiheiden avulla voit tuoda tietoja CSV-tiedoston rakenne-taulukon __viiveet__.

1. Voit luoda ja muokata tiedosto nimeltä __flightdelays.hql__käyttämällä seuraavaa:

        nano flightdelays.hql
        
    Käyttää tämän tiedoston sisällön seuraavasti:
    
        DROP TABLE delays_raw;
        -- Creates an external table over the csv file
        CREATE EXTERNAL TABLE delays_raw (
            YEAR string,
            FL_DATE string,
            UNIQUE_CARRIER string,
            CARRIER string,
            FL_NUM string,
            ORIGIN_AIRPORT_ID string,
            ORIGIN string,
            ORIGIN_CITY_NAME string,
            ORIGIN_CITY_NAME_TEMP string,
            ORIGIN_STATE_ABR string,
            DEST_AIRPORT_ID string,
            DEST string,
            DEST_CITY_NAME string,
            DEST_CITY_NAME_TEMP string,
            DEST_STATE_ABR string,
            DEP_DELAY_NEW float,
            ARR_DELAY_NEW float,
            CARRIER_DELAY float,
            WEATHER_DELAY float,
            NAS_DELAY float,
            SECURITY_DELAY float,
            LATE_AIRCRAFT_DELAY float)
        -- The following lines describe the format and location of the file
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ','
        LINES TERMINATED BY '\n'
        STORED AS TEXTFILE
        LOCATION '/tutorials/flightdelays/data';
        
        -- Drop the delays table if it exists
        DROP TABLE delays;
        -- Create the delays table and populate it with data
        -- pulled in from the CSV file (via the external table defined previously)
        CREATE TABLE delays AS
        SELECT YEAR AS year,
            FL_DATE AS flight_date,
            substring(UNIQUE_CARRIER, 2, length(UNIQUE_CARRIER) -1) AS unique_carrier,
            substring(CARRIER, 2, length(CARRIER) -1) AS carrier,
            substring(FL_NUM, 2, length(FL_NUM) -1) AS flight_num,
            ORIGIN_AIRPORT_ID AS origin_airport_id,
            substring(ORIGIN, 2, length(ORIGIN) -1) AS origin_airport_code,
            substring(ORIGIN_CITY_NAME, 2) AS origin_city_name,
            substring(ORIGIN_STATE_ABR, 2, length(ORIGIN_STATE_ABR) -1)  AS origin_state_abr,
            DEST_AIRPORT_ID AS dest_airport_id,
            substring(DEST, 2, length(DEST) -1) AS dest_airport_code,
            substring(DEST_CITY_NAME,2) AS dest_city_name,
            substring(DEST_STATE_ABR, 2, length(DEST_STATE_ABR) -1) AS dest_state_abr,
            DEP_DELAY_NEW AS dep_delay_new,
            ARR_DELAY_NEW AS arr_delay_new,
            CARRIER_DELAY AS carrier_delay,
            WEATHER_DELAY AS weather_delay,
            NAS_DELAY AS nas_delay,
            SECURITY_DELAY AS security_delay,
            LATE_AIRCRAFT_DELAY AS late_aircraft_delay
        FROM delays_raw;
        
2. Tallenna tiedosto __Ctrl + X__ja __Y__ avulla.

3. Seuraavat avulla voit aloittaa rakenne ja suorita __flightdelays.hql__ -tiedosto:

        beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin -f flightdelays.hql
        
    > [AZURE.NOTE] Tässä esimerkissä `localhost` käytetään, koska olet muodostanut yhteyden pään solmun HDInsight-klusterin, joka on, jossa HiveServer2 on käynnissä.

4. Käytä seuraavaa komentoa Avaa vuorovaikutteisen Beeline-istunnon:

        beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin

5. Kun olet saanut `jdbc:hive2://localhost:10001/>` Kysy, käytä seuraavia tuodut lentotietoihin viive tietojen hakemiseen.

        INSERT OVERWRITE DIRECTORY '/tutorials/flightdelays/output'
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
        SELECT regexp_replace(origin_city_name, '''', ''),
            avg(weather_delay)
        FROM delays
        WHERE weather_delay IS NOT NULL
        GROUP BY origin_city_name;

    Tämä Hae kaupunkia, joissa listan sää viiveitä keskimääräinen viiveaika sekä luettelo ja tallentaa sen `/tutorials/flightdelays/output`. Myöhemmin Sqoop lukee tiedot kyseisestä sijainnista ja vieminen Azure SQL-tietokantaan.

6. Jos haluat poistua Beeline, kirjoita `!quit` tulee näkyviin.

## <a name="create-a-sql-database"></a>SQL-tietokannan luominen

Jos sinulla on jo SQL-tietokantaan, saat palvelimen nimi. Voit etsiä tämän [Azure-portaali](https://portal.azure.com) valitsemalla __SQL-tietokannat__ja sitten suodattaminen haluat käyttää tietokannan nimi. Palvelimen nimi näkyy __palvelin__ -sarakkeessa.

Jos sinulla ei ole SQL-tietokantaan, käytä tiedot [SQL-tietokanta-opetusohjelma: SQL-tietokannan luominen minuutteina](../sql-database/sql-database-get-started.md) luomaan. Sinun on käytettävä tietokanta palvelimen nimi Tallenna.

##<a name="create-a-sql-database-table"></a>SQL-tietokanta-taulukon luominen

> [AZURE.NOTE] Yhteyden muodostaminen SQL-tietokantaan, voit luoda taulukon monella tavalla. Seuraavat vaiheet käyttää [FreeTDS](http://www.freetds.org/) HDInsight-klusterin.

1. SSH avulla voit muodostaa yhteyden Linux-pohjaiset HDInsight-klusterin ja suorita seuraavat vaiheet SSH istunnosta.

3. Käytä asentamiseksi FreeTDS seuraava komento:

        sudo apt-get --assume-yes install freetds-dev freetds-bin

4. Kun FreeTDS on asennettu, seuraavalla komennolla voit muodostaa yhteyden SQL-tietokantapalvelimeen. Korvaa SQL-tietokantapalvelimen nimi __palvelimen nimi__ . Korvaa __adminLogin__ ja __adminPassword__ kirjautuminen SQL-tietokantaan. Korvaa tietokannan nimi __nimi__ .

        TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D <databaseName>

    Näyttöön tulee tulosteen seuraavankaltaiselta:

        locale is "en_US.UTF-8"
        locale charset is "UTF-8"
        using default charset "UTF-8"
        Default database being set to sqooptest
        1>

5. Milloin `1>` Kysy, kirjoita seuraavat rivit:

        CREATE TABLE [dbo].[delays](
        [origin_city_name] [nvarchar](50) NOT NULL,
        [weather_delay] float,
        CONSTRAINT [PK_delays] PRIMARY KEY CLUSTERED   
        ([origin_city_name] ASC))
        GO

    Kun `GO` lauseen yksikkö-edellisen lauseet lasketaan. Tämä vaihtoehto luo uuden taulukon nimeltä __viiveitä__liitetty indeksi (vaatii SQL-tietokantaan.)

    Voit varmistaa, että taulukossa on luotu käyttämällä seuraavaa:

        SELECT * FROM information_schema.tables
        GO

    Näyttöön tulee tulosteen seuraavankaltaiselta:

        TABLE_CATALOG   TABLE_SCHEMA    TABLE_NAME      TABLE_TYPE
        databaseName       dbo     delays      BASE TABLE

8. Kirjoita `exit` osoitteessa `1>` kehote Lopeta tsql-apuohjelma.
    
##<a name="export-data-with-sqoop"></a>Vie tiedot ja Sqoop

2. Seuraavalla komennolla voit varmistaa, että Sqoop voi nähdä SQL-tietokantaan:

        sqoop list-databases --connect jdbc:sqlserver://<serverName>.database.windows.net:1433 --username <adminLogin> --password <adminPassword>

    Tämä palautetaan tietokannoista, mukaan lukien tietokannan viiveitä juuri aiemmin luomasi luettelo.

3. Käytä seuraavaa komentoa tietojen vieminen mobiledata taulukon hivesampletable:

        sqoop export --connect 'jdbc:sqlserver://<serverName>.database.windows.net:1433;database=<databaseName>' --username <adminLogin> --password <adminPassword> --table 'delays' --export-dir 'wasbs:///tutorials/flightdelays/output' --fields-terminated-by '\t' -m 1

    Tämän tiedon Sqoop viiveitä, taulukon-tietokantayhteyden muodostamisessa SQL-tietokantaan ja tietojen vieminen wasbs: / / / opetusohjelmat/flightdelays/tulostus (on tallennettu aiemmin rakenne kyselyn tulos) viiveitä-taulukkoon.

4. Komennon jälkeen muodostaa yhteyden tietokantaan käyttämällä TSQL käyttämällä seuraavaa:

        TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D sqooptest

    Kun yhteys on muodostettu, varmista, että tiedot on viety mobiledata taulukkoon seuraavista väittämistä avulla:
    
        SELECT * FROM delays
        GO

    Raportissa pitäisi näkyä luettelo-taulukon tiedot. Kirjoita `exit` Lopeta tsql-apuohjelma.

##<a id="nextsteps"></a>Seuraavat vaiheet

Nyt ymmärrät tiedoston lataaminen Azure-Blob-säiliö, miten täytä rakenteen taulukon avulla tiedot Azure-Blob-säiliö, rakenne kyselyjen suorittaminen ja tietojen vieminen Azure SQL-tietokantaan HDFS Sqoop avulla. Lisätietoja on seuraavissa artikkeleissa:

* [HDInsight käytön aloittaminen][hdinsight-get-started]
* [Rakenteen käyttäminen Hdinsightiin][hdinsight-use-hive]
* [Oozie käyttäminen Hdinsightiin][hdinsight-use-oozie]
* [Sqoop käyttäminen Hdinsightiin][hdinsight-use-sqoop]
* [Possu käyttäminen Hdinsightiin][hdinsight-use-pig]
* [Kehitä Java MapReduce ohjelmien Hdinsightiin][hdinsight-develop-mapreduce]
* [Kehittää Python Hadoop streaming ohjelmat HDInsight varten][hdinsight-develop-streaming]



[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/


[rita-website]: http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time
[cindygross-hive-tables]: http://blogs.msdn.com/b/cindygross/archive/2013/02/06/hdinsight-hive-internal-and-external-tables-intro.aspx

[hdinsight-use-oozie]: hdinsight-use-oozie-linux-mac.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-use-sqoop]: hdinsight-use-sqoop-mac-linux.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-develop-streaming]: hdinsight-hadoop-streaming-python.md
[hdinsight-develop-mapreduce]: hdinsight-develop-deploy-java-mapreduce-linux.md

[hadoop-hiveql]: https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL

[technetwiki-hive-error]: http://social.technet.microsoft.com/wiki/contents/articles/23047.hdinsight-hive-error-unable-to-rename.aspx


 
