<properties
   pageTitle="Analysoida ja prosessin JSON tiedostoja yhdessä HDInsight-rakenne | Microsoft Azure"
   description="Lue, miten voit käyttää JSON asiakirjoja ja analysointia käyttämällä HDInsight-rakenne."
   services="hdinsight"
   documentationCenter=""
   authors="rashimg"
   manager="mwinkle"
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="06/22/2015"
   ms.author="rashimg"/>


# <a name="process-and-analyze-json-documents-using-hive-in-hdinsight"></a>Käsittele ja analysoi JSON-tiedostojen HDInsight-rakenne

Opettele käsittelemään ja analysoida JSON tiedostot HDInsight-rakenne. JSON-asiakirja, jota käytetään opetusohjelman

    {
        "StudentId": "trgfg-5454-fdfdg-4346",
        "Grade": 7,
        "StudentDetails": [
            {
                "FirstName": "Peggy",
                "LastName": "Williams",
                "YearJoined": 2012
            }
        ],
        "StudentClassCollection": [
            {
                "ClassId": "89084343",
                "ClassParticipation": "Satisfied",
                "ClassParticipationRank": "High",
                "Score": 93,
                "PerformedActivity": false
            },
            {
                "ClassId": "78547522",
                "ClassParticipation": "NotSatisfied",
                "ClassParticipationRank": "None",
                "Score": 74,
                "PerformedActivity": false
            },
            {
                "ClassId": "78675563",
                "ClassParticipation": "Satisfied",
                "ClassParticipationRank": "Low",
                "Score": 83,
                "PerformedActivity": true
            }
        ]
    }

Tiedoston tukikäytännöistä wasbs://processjson@hditutorialdata.blob.core.windows.net/. Katso lisätietoja Azure-Blob-säiliö käyttämisestä HDInsight, [Käytä HDFS-yhteensopiva Azure Blob-objektien tallennustilaan Hadoop HDInsight kanssa](hdinsight-hadoop-use-blob-storage.md). Voit kopioida tiedoston yhteyttä klusterin oletusarvon säilön halutessasi.

Tässä opetusohjelmassa käytetään rakenne-konsolin.  Katso ohjeet avaaminen rakenne-konsolin [Käyttäminen rakenne ja Hadoop-HDInsight etätyöpöydän kanssa](hdinsight-hadoop-use-hive-remote-desktop.md).

##<a name="flatten-json-documents"></a>Litistä JSON asiakirjat

Seuraavassa osassa luetellut tavat edellyttävät JSON asiakirjan yhdelle riville. On niin Litistä merkkijonon JSON asiakirjan. Jos JSON-asiakirjassa on jo käytössä, voit ohittaa tämän vaiheen ja siirtyä suoraan seuraavan osion analysoiminen JSON tietojen.

    DROP TABLE IF EXISTS StudentsRaw;
    CREATE EXTERNAL TABLE StudentsRaw (textcol string) STORED AS TEXTFILE LOCATION "wasb://processjson@hditutorialdata.blob.core.windows.net/";

    DROP TABLE IF EXISTS StudentsOneLine;
    CREATE EXTERNAL TABLE StudentsOneLine
    (
      json_body string
    )
    STORED AS TEXTFILE LOCATION '/json/students';

    INSERT OVERWRITE TABLE StudentsOneLine
    SELECT CONCAT_WS(' ',COLLECT_LIST(textcol)) AS singlelineJSON
          FROM (SELECT INPUT__FILE__NAME,BLOCK__OFFSET__INSIDE__FILE, textcol FROM StudentsRaw DISTRIBUTE BY INPUT__FILE__NAME SORT BY BLOCK__OFFSET__INSIDE__FILE) x
          GROUP BY INPUT__FILE__NAME;

    SELECT * FROM StudentsOneLine

Raaka JSON-tiedosto sijaitsee **wasbs://processjson@hditutorialdata.blob.core.windows.net/**. *StudentsRaw* rakennetaulukko asioista raaka yhdistelmävisualisoinnin litistetty JSON-tiedostoon.

*StudentsOneLine* rakennetaulukko tallentaa tiedot polussa ** /json/opiskelijoiden/HDInsight käyttöjärjestelmän.

Lisää-lause täyttää StudentOneLine-taulukko, jonka litistetty JSON-tiedot.

SELECT-lauseen palauttaa vain 1 rivi.

Näin SELECT-lauseen tulos:

![Litistämistä JSON asiakirjan.][image-hdi-hivejson-flatten]

##<a name="analyze-json-documents-in-hive"></a>Analysoi JSON tiedostojen rakenne

Rakenne on kolme eri tapaa tehdä kyselyjä JSON tiedostoja:

- Käytä Aloita\_JSON\_OBJEKTIN UDF (käyttäjän määrittämät-funktio)
- Käytä JSON_TUPLE UDF
- Käytä mukautettuja SerDe
- Kirjoita omistat UDF Python tai muilla kielillä. Lue [artikkeli] [ hdinsight-python] suorittamisesta Python oman koodin rakenteen kanssa.

### <a name="use-the-getjsonobject-udf"></a>Käytä Aloita\_JSON_OBJECT UDF
Rakenne sisältää valmiita UDF nimeltä [Hae json-objekti](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-get_json_object) , joka suorittaa JSON kyselyn suorituksen aikana. Tämä menetelmä on kaksi argumenttia – taulukkonimi ja menetelmän nimi, joka on litistetty JSON-asiakirja ja JSON-kentän, joka on jäsentää. Katsotaan Esimerkki näet miten tämä UDF toimii.

Etunimi ja Sukunimi kukin opiskelija

    SELECT
      GET_JSON_OBJECT(StudentsOneLine.json_body,'$.StudentDetails.FirstName'),
      GET_JSON_OBJECT(StudentsOneLine.json_body,'$.StudentDetails.LastName')
    FROM StudentsOneLine;

Tässä on tulos, tämän kyselyn suorituksen konsoli-ikkunassa.

![get_json_object UDF][image-hdi-hivejson-getjsonobject]

Get-json_object UDF on joitakin rajoituksia.

- Koska kyselyn kunkin kentän edellyttää jäsentäminen uudelleen kyselyn, se vaikuttaa suorituskykyyn.
- HAE\_JSON_OBJECT() palauttaa matriisin käänteismatriisin string-esitys. Tämä muuntaminen rakenne-matriisi, sinun on hakasulkeita korvaaminen yleismerkkien avulla ' ["ja"] "ja soita Jaa matriisin saat myös.


Tämän vuoksi rakenteen wiki suosittelee json_tuple käyttämistä.  

### <a name="use-the-jsontuple-udf"></a>Käytä JSON_TUPLE UDF

Toisen UDF rakenteen myöntämä kutsutaan [json_tuple](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-json_tuple) , joka suorittaa parempi vaihtoehto [get_ json _object](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF#LanguageManualUDF-get_json_object). Tämä menetelmä näppäimet ja JSON merkkijonon, ja palauttaa monikon arvojen yhteen-funktiolla. Seuraava kysely palauttaa student-tunnus ja allekirjoittajan JSON asiakirjasta:

    SELECT q1.StudentId, q1.Grade
      FROM StudentsOneLine jt
      LATERAL VIEW JSON_TUPLE(jt.json_body, 'StudentId', 'Grade') q1
        AS StudentId, Grade;

Tämä komentosarja rakenne-konsolissa tulos:

![json_tuple UDF][image-hdi-hivejson-jsontuple]

JSON\_MONIKON käyttää [radan viereen varattu näyttämisen](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+LateralView) syntaksi rakennetta, joka sallii json\_monikon virtual taulukon luominen lisäämällä alkuperäisen taulukon kunkin rivin UDT-funktiota.  Monimutkainen JSONs liian käytännöllisesti radan viereen varattu NÄKYMÄN toistuvien käytön vuoksi. Lisäksi JSON_TUPLE ei voi käsitellä sisäkkäisiä JSONs.


###<a name="use-custom-serde"></a>Käytä mukautettuja SerDe

SerDe on paras vaihtoehto jäsennyksen sisäkkäisiä JSON-asiakirjat, sen avulla voit määrittää JSON-rakenteen ja käyttää rakennetta jäsentää asiakirjat. Tässä opetusohjelmassa voit käyttää jotain suositumpi SerDe, joka on kehitetty [rcongiu](https://github.com/rcongiu).

**Voit käyttää mukautettuja SerDe seuraavasti:**

1. Asenna [Java SE Development Kit 7u55 JDK 1.7.0_55](http://www.oracle.com/technetwork/java/javase/downloads/java-archive-downloads-javase7-521261.html#jdk-7u55-oth-JPR). Valitse JDK Windows X64-version, jos aiot käyttää HDInsight Windows-käyttöönoton

    >[AZURE.WARNING] JDK 1.8 ei käsitellä tämän SerDe.

    Kun asennus on valmis, Lisää uusi käyttäjä-ympäristömuuttujan:

    1. Avaa Windows näytön **näkymä järjestelmän Lisäasetukset** .
    2. Valitse **Ympäristömuuttujat**.  
    3. Lisää uusi **JAVA_HOME** ympäristömuuttuja osoittamalla **C:\Program Files\Java\jdk1.7.0_55** tai kohdassa oman JDK on asennettu.

    ![JDK oikea config arvojen määrittäminen][image-hdi-hivejson-jdk]

2. Asenna [maven-testi 3.3.1](http://mirror.olnevhost.net/pub/apache/maven/maven-3/3.3.1/binaries/apache-maven-3.3.1-bin.zip)

    Bin-kansio lisääminen path siirtymällä Control Panel--> Muokkaa Järjestelmämuuttujat tilin Environment-muuttujat. Seuraavassa näyttökuvassa näkyy tähän.

    ![Maven-testi määrittäminen][image-hdi-hivejson-maven]

3. Kloonaa projektin [Rakenne-JSON-SerDe](https://github.com/sheetaldolas/Hive-JSON-Serde/tree/master) github-sivustosta. Voit tehdä tämän napsauttamalla "Lataa Zip-painikkeen alla olevassa näyttökuvassa esitetyllä tavalla.

    ![Projektin kloonaamalla][image-hdi-hivejson-serde]

4: Siirry kansioon, johon olet ladannut tämän paketin ja kirjoita "mvn package". Tämä luo tarvittavat purkki tiedostot, jotka voit sitten kopioida päälle klusterin.

5: Siirry kohdekansio pääkansio, johon olet ladannut paketti-kohdassa. Json-serde-1.1.9.9-Hive13-jar-with-dependencies.jar-tiedoston lataaminen pää-solmu yhteyttä klusterin. Voin ladata sen yleensä rakenteen binaarinen kansiossa: C:\apps\dist\hive-0.13.0.2.1.11.0-2316\bin tai kaltaisen.

6: Kirjoita rakenne-kehote "Lisää purkki /path/to/json-serde-1.1.9.9-Hive13-jar-with-dependencies.jar". Oma tapauksessa purkkiin on C:\apps\dist\hive-0.13.x\bin-kansioon, voin lisätä suoraan nimellä purkkiin, alla kuvatulla tavalla:

    add jar json-serde-1.1.9.9-Hive13-jar-with-dependencies.jar;

    ![Adding JAR to your project][image-hdi-hivejson-addjar]

Nyt olet valmis SerDe avulla voit suorittaa kyselyjä JSON-asiakirja.

Seuraava lauseke Luo taulukko, jossa määritetty rakenne

    DROP TABLE json_table;
    CREATE EXTERNAL TABLE json_table (
      StudentId string,
      Grade int,
      StudentDetails array<struct<
          FirstName:string,
          LastName:string,
          YearJoined:int
          >
      >,
      StudentClassCollection array<struct<
          ClassId:string,
          ClassParticipation:string,
          ClassParticipationRank:string,
          Score:int,
          PerformedActivity:boolean
          >
      >
    ) ROW FORMAT SERDE 'org.openx.data.jsonserde.JsonSerDe'
    LOCATION '/json/students';

Etunimi ja Sukunimi käänteisen luettelon

    SELECT StudentDetails.FirstName, StudentDetails.LastName FROM json_table;

Näin rakenne-konsolin tulos.

![Kyselyn SerDe 1][image-hdi-hivejson-serde_query1]

Voit laskea tulosten JSON asiakirjan

    SELECT SUM(scores)
    FROM json_table jt
      lateral view explode(jt.StudentClassCollection.Score) collection as scores;

Kysely yllä käyttää [radan viereen varattu näkymän leikkaaminen](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+LateralView) UDF Laajenna tulosten matriisin niin, että ne voidaan summata.

Rakenne-konsolin siirretään.

![Kyselyn SerDe 2][image-hdi-hivejson-serde_query2]

Voit etsiä mitä aiheet annetun opiskelija on tulos yli 80 pisteet valitsemalla  
      jt. StudentClassCollection.ClassId-json_table jt radan viereen varattu näkymän leikkaaminen (jt. Tuloksen kuin sivustokokoelman StudentClassCollection.Score) > 80, jossa pisteet

Yllä kysely palauttaa rakenteen matriisin toisin kuin get\_json\_objekti, joka palauttaa merkkijonon.

![Kyselyn SerDe 3][image-hdi-hivejson-serde_query3]

Jos haluat skil Vääränlainen JSON, valitse tämä SerDe [wikisivun](https://github.com/sheetaldolas/Hive-JSON-Serde/tree/master) artikkelissa kuvatulla tavalla voit toteuttaa, kirjoittamalla seuraava koodi:  

    ALTER TABLE json_table SET SERDEPROPERTIES ( "ignore.malformed.json" = "true");




##<a name="summary"></a>Yhteenveto
Yhteenveto rakenne, joka on valittava JSON-operaattorin tyyppi vaihtelee tilanteen. Jos sinulla on yksinkertainen JSON-asiakirja ja sinulla on vain yksi kenttä-– hakemiseen voit käytössä rakenne UDF\_json\_objekti. Jos sinulla on useampi kuin yksi näppäimet, jotta voit hakea voit käyttää json_tuple. Jos sisäkkäisiä asiakirja on kannattaa käyttää JSON-SerDe.

Muita aiheeseen liittyviä artikkeleja on ohjeaiheessa

- [Käyttäminen Hadoop HDInsight-rakenne ja HiveQL analysointiin Apache log4j mallitiedosto](hdinsight-use-hive.md)
- [Lentotietoihin viiveen analysoiminen käyttämällä HDInsight-rakenne](hdinsight-analyze-flight-delay-data.md)
- [Voit analysoida Twitter-tietoja käyttämällä HDInsight-rakenne](hdinsight-analyze-twitter-data.md)
- [Suorita Hadoop työn DocumentDB ja Hdinsightista](../documentdb/documentdb-run-hadoop-with-hdinsight.md)

[hdinsight-python]: hdinsight-python.md

[image-hdi-hivejson-flatten]: ./media/hdinsight-using-json-in-hive/flatten.png
[image-hdi-hivejson-getjsonobject]: ./media/hdinsight-using-json-in-hive/getjsonobject.png
[image-hdi-hivejson-jsontuple]: ./media/hdinsight-using-json-in-hive/jsontuple.png
[image-hdi-hivejson-jdk]: ./media/hdinsight-using-json-in-hive/jdk.png
[image-hdi-hivejson-maven]: ./media/hdinsight-using-json-in-hive/maven.png
[image-hdi-hivejson-serde]: ./media/hdinsight-using-json-in-hive/serde.png
[image-hdi-hivejson-addjar]: ./media/hdinsight-using-json-in-hive/addjar.png
[image-hdi-hivejson-serde_query1]: ./media/hdinsight-using-json-in-hive/serde_query1.png
[image-hdi-hivejson-serde_query2]: ./media/hdinsight-using-json-in-hive/serde_query2.png
[image-hdi-hivejson-serde_query3]: ./media/hdinsight-using-json-in-hive/serde_query3.png
[image-hdi-hivejson-serde_result]: ./media/hdinsight-using-json-in-hive/serde_result.png
