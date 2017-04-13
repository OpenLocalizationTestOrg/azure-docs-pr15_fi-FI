<properties
   pageTitle="Optimoi HDInsight nopeammin suoritus rakenne-kyselyissä | Microsoft Azure"
   description="Opettele optimoimaan rakenteen kyselyjen Hadoop HDInsight varten."
   services="hdinsight"
   documentationCenter=""
   authors="rashimg"
   manager="mwinkle"
   editor="cgronlun"
   tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="07/28/2015"
   ms.author="rashimg"/>


# <a name="optimize-hive-queries-for-hadoop-in-hdinsight"></a>Rakenne-kyselyiden optimointi HDInsight Hadoop

Oletusarvon mukaan Hadoop klustereiden ei ole optimoitu suorituskyvyn. Tässä artikkelissa käsitellään muutama yleisimmistä rakenteen suorituskyvyn optimointia menetelmistä, voit käyttää sekä kyselyt.

##<a name="scale-out-worker-nodes"></a>Skaalaa työntekijä solmujen ulos

Klusterin työntekijä solmujen määrän voidaan hyödyntää enemmän mappers ja suorita rinnakkain pienennyslaitteita varten. Voit suurentaa asteikkoa HDInsight kahdella tavalla:

- Säännöstä aikaan voit määrittää Azure-portaalissa, PowerShellin Azure tai rivin Office kaikissa ympäristöissä komentoliittymä työntekijä solmujen määrän.  Lisätietoja on artikkelissa [säännöstä HDInsight klustereiden](hdinsight-provision-clusters.md). Seuraavassa näytössä näyttää työntekijän solmu määritykset Azure-portaalissa:

    ![scaleout_1][image-hdi-optimize-hive-scaleout_1]

- Suorituksen aikana voit myös skaalata ulos klusterin ilman luotaessa jokin. Tämä on alla.
![scaleout_1][image-hdi-optimize-hive-scaleout_2]

Lisätietoja eri näennäiskoneiden tukemat Hdinsightista on artikkelissa [HDInsight hinnat](https://azure.microsoft.com/pricing/details/hdinsight/).

##<a name="enable-tez"></a>Ota käyttöön Tez

[Apache Tez](http://hortonworks.com/hadoop/tez/) on vaihtoehtoisia suorittamisen-ohjelma MapReduce-moduulin:

![tez_1][image-hdi-optimize-hive-tez_1]


Tez on nopeampaa, koska:

- Suorittaa yhteen työn MapReduce Engine ohjataan asykliset Graph (DAG), DAG, joka ilmaistaan edellyttää, että kukin ehtojoukko mappers noudatettava joukkona pienennyslaitteita varten. Tämä aiheuttaa useita MapReduce työt kehrätty kunkin rakenteen kyselyn. Tez ei ole tällaisia rajoitus ja käsitellä monimutkaisia DAG kuin yhden työn siten pienentämisen työn käynnistys katseltavan.
- **Tarpeettomien Avoids kirjoittaa** Vuoksi useita töitä parhaillaan kehrätty sama rakenne kyselyn MapReduce-ohjelma kunkin projektin tulos kirjoitetaan HDFS, keskitaso tiedoille. Koska Tez minimoi töiden kunkin rakenne-kyselyn on voit välttää tarpeettomat kirjoittaminen.
- **Minimizes pysäyttämisestä ehkäiseminen** Tez paremmin voi pienentää pysäyttämisestä viive vähentämällä mappers käynnistämiseen tarvitaan määrän ja parantaa myös optimointi koko.
- **Säilöjen käyttää uudelleen** Aina, kun mahdollista Tez on säilöjen, jotta viive vuoksi säilöjen käynnistäminen pienenee uudelleen.
- **Jatkuva optimointi tekniikat** Perinteisesti optimointi määritetty kääntäminen vaiheen aikana. Kuitenkin lisätietoja syötteiden on käytettävissä, jotka sallivat paremmin optimointi suorituksen aikana. Tez käyttää jatkuva optimointi tekniikoita, jonka avulla muita kyselyjä runtime vaiheen suunnitelman optimointi.

Lisätietoja näistä käsitteistä, napsauttamalla [tätä](http://hortonworks.com/hadoop/tez/)

Voit tehdä kyselyn rakenne Tez etuliitteiden kyselyn alla-asetus käyttöön:

    set hive.execution.engine=tez;

For Windows-pohjaisesta HDInsight klustereiden Tez on otettava käyttöön säännöstä aikaa. Seuraavassa on esimerkki PowerShellin Azure komentosarjan valmistelu Hadoop-klusterin Tez käytössä:

[AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

    $clusterName = "[HDInsightClusterName]"
    $location = "[AzureDataCenter]" #i.e. West US
    $dataNodes = 32 # number of worker nodes in the cluster

    $defaultStorageAccountName = "[DefaultStorageAccountName]"
    $defaultStorageContainerName = "[DefaultBlobContainerName]"
    $defaultStorageAccountKey = $defaultStorageAccountKey = Get-AzureStorageKey $defaultStorageAccountName.ToLower() | %{ $_.Primary }

    $hdiUserName = "[HTTPUserName]"
    $hdiPassword = "[HTTPUserPassword]"

    $hdiSecurePassword = ConvertTo-SecureString $hdiPassword -AsPlainText -Force
    $hdiCredential = New-Object System.Management.Automation.PSCredential($hdiUserName, $hdiSecurePassword)

    $hiveConfig = new-object 'Microsoft.WindowsAzure.Management.HDInsight.Cmdlet.DataObjects.AzureHDInsightHiveConfiguration'
    $hiveConfig.Configuration = @{ "hive.execution.engine"="tez" }

    New-AzureHDInsightClusterConfig -ClusterSizeInNodes $dataNodes -HeadNodeVMSize Standard_D14 -DataNodeVMSize Standard_D14 |
    Set-AzureHDInsightDefaultStorage -StorageAccountName "$defaultStorageAccountName.blob.core.windows.net" -StorageAccountKey $defaultStorageAccountKey -StorageContainerName $defaultStorageContainerName |
    Add-AzureHDInsightConfigValues -Hive $hiveConfig |
    New-AzureHDInsightCluster -Name $clusterName -Location $location -Credential $hdiCredential

    
> [AZURE.NOTE] Linux-pohjaiset HDInsight klustereiden on oletusarvoisesti käytössä Tez.
    

## <a name="hive-partitioning"></a>Rakenne jakaminen

I/o-toiminto on pää suorituskyvyn pullonkaulana käynnissä rakenteen kyselyt. Suorituskykyä voi parantaa Jos korjattava voi lukea tietojen määrä vähenee. Oletusarvon mukaan rakenteen kyselyjen tarkistamaan rakenteen kokonaisia taulukoita. Tämä on hyvä kyselyjen, esimerkiksi tarkistaa taulukon kuitenkin kyselyitä, jotka on vain vähän tietoja (kuten kyselyt, kun suodatus) tarkistamaan, tämä luo tarpeettomia katseltavan. Rakenteen kyselyjen käyttämään rakenteen taulukoiden tietojen tarvittavat summa rakenteen jakaminen avulla.

Rakenteen jakaminen on toteutettu järjestämällä perustietoja uusi kansioihin kunkin osion on oma hakemisto - kohtaa, johon käyttäjän määrittämä osio. Seuraavassa kaaviossa on kuvattu jakaminen rakennetaulukko *vuosi*-sarakkeen mukaan. Uusi kansio luodaan vuosittain.

![jakaminen][image-hdi-optimize-hive-partitioning_1]

Osioinnin seikkoja:

- **Eivät ole alapuolella osio** - sarakkeiden kanssa vain muutaman arvon jakaminen voi estää muutamaa osiot. Esimerkiksi jakaminen-sukupuoli Luo vain kaksi osiota luodaan (Mies ja nainen), näin vain pienentäminen viive enintään puolet.

- **Ei perusteettomasti osio** - muut hyvin voimakas, valitse osion luomisesta sarake, jossa on yksilöllinen arvo (kuten käyttäjätunnus) aiheuttaa useita osioita ilmenneet kuormitus paljon klusterin namenode, kun se on käsitellään kansioiden suuria määriä.

- **Vältä tietojen jakauman.vinous** - Valitse key-tuotetunnuksen osioinnin viisaasti niin, että kaikki osiot ovat myös kokoa. Esimerkki on jakaminen *tilassa* saattaa aiheuttaa Kalifornian on melkein 30 kohdassa tietueiden määrän, jotka Vermont vuoksi populaation ero x.

Voit luoda osion taulukon käyttämällä *Osioitu By* -lause:

    CREATE TABLE lineitem_part
        (L_ORDERKEY INT, L_PARTKEY INT, L_SUPPKEY INT,L_LINENUMBER INT,
         L_QUANTITY DOUBLE, L_EXTENDEDPRICE DOUBLE, L_DISCOUNT DOUBLE,
         L_TAX DOUBLE, L_RETURNFLAG STRING, L_LINESTATUS STRING,
         L_SHIPDATE_PS STRING, L_COMMITDATE STRING, L_RECEIPTDATE        STRING, L_SHIPINSTRUCT STRING, L_SHIPMODE STRING,
         L_COMMENT STRING)
    PARTITIONED BY(L_SHIPDATE STRING)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    STORED AS TEXTFILE;

Kun osioitua taulukko luodaan, voit joko luoda jakaminen staattinen tai dynaaminen jakaminen.

- **Staattinen jakaminen** tarkoittaa, että sinulla on jo sharded tietoja tarvittavat kansioissa voidaan esittää rakenteen osioita manuaalisesti hakemiston sijainnin perusteella. Tämä näkyy alla koodikatkelman.

        INSERT OVERWRITE TABLE lineitem_part
        PARTITION (L_SHIPDATE = ‘5/23/1996 12:00:00 AM’)
        SELECT * FROM lineitem 
        WHERE lineitem.L_SHIPDATE = ‘5/23/1996 12:00:00 AM’

        ALTER TABLE lineitem_part ADD PARTITION (L_SHIPDATE = ‘5/23/1996 12:00:00 AM’))
        LOCATION ‘wasbs://sampledata@ignitedemo.blob.core.windows.net/partitions/5_23_1996/'

- **Dynaaminen jakaminen** tarkoittaa, että haluat rakenteen Luo osioita automaattisesti puolestasi. On jo luonut osioinnin taulukon väliaikaisen taulukossa, emme tarvitsee on Lisää osioitua taulukkoon alla kuvatulla tavalla:

        SET hive.exec.dynamic.partition = true;
        SET hive.exec.dynamic.partition.mode = nonstrict;
        INSERT INTO TABLE lineitem_part
        PARTITION (L_SHIPDATE)
        SELECT L_ORDERKEY as L_ORDERKEY, L_PARTKEY as L_PARTKEY , 
             L_SUPPKEY as L_SUPPKEY, L_LINENUMBER as L_LINENUMBER,
             L_QUANTITY as L_QUANTITY, L_EXTENDEDPRICE as L_EXTENDEDPRICE,
             L_DISCOUNT as L_DISCOUNT, L_TAX as L_TAX, L_RETURNFLAG as       L_RETURNFLAG, L_LINESTATUS as L_LINESTATUS, L_SHIPDATE as       L_SHIPDATE_PS, L_COMMITDATE as L_COMMITDATE, L_RECEIPTDATE as   L_RECEIPTDATE, L_SHIPINSTRUCT as L_SHIPINSTRUCT, L_SHIPMODE as      L_SHIPMODE, L_COMMENT as L_COMMENT, L_SHIPDATE as L_SHIPDATE FROM lineitem;

Lisätietoja on artikkelissa [Osioitu taulukot](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL#LanguageManualDDL-PartitionedTables).

##<a name="use-the-orcfile-format"></a>Käytä ORCFile muotoa

Rakenteen tukee eri tiedostomuodossa. Esimerkki:

- **Teksti**: Tämä on oletustiedostomuodon ja useimmissa skenaarioissa toimii
- **Avro**: toimii hyvin yhteensopivuus-skenaariot
- **ORC/tilat**: parhaiten suorituskyky

ORC (optimoitu rivin Sarakemuoto)-muoto on erittäin tehokkaasti rakenteen tietojen tallentamiseen. Verrattuna muihin tiedostomuotoihin, ORC on seuraavia etuja:

- monimutkainen tyypit, mukaan lukien päivämäärä ja aika ja monimutkaisia ja puolirakenteisia tuki
- enintään 70 % pakkaus
- indeksit jokaisen 10 000 rivit, jotka mahdollistavat rivien ohittaminen
- merkittäviä avattavan suorituksenaikainen suorittamisen

ORC muoto käyttöön ensin luot taulukon lauseen *tallennetut kuin ORC*kanssa:

    CREATE TABLE lineitem_orc_part
        (L_ORDERKEY INT, L_PARTKEY INT,L_SUPPKEY INT, L_LINENUMBER INT,
         L_QUANTITY DOUBLE, L_EXTENDEDPRICE DOUBLE, L_DISCOUNT DOUBLE,
         L_TAX DOUBLE, L_RETURNFLAG STRING, L_LINESTATUS STRING,
         L_SHIPDATE_PS STRING, L_COMMITDATE STRING, L_RECEIPTDATE STRING,
         L_SHIPINSTRUCT STRING, L_SHIPMODE STRING, L_COMMENT     STRING)
    PARTITIONED BY(L_SHIPDATE STRING)
    STORED AS ORC;

Seuraavaksi voit lisätä tietoja taulukkoon ORC väliaikaisen taulukossa. Esimerkki:

    INSERT INTO TABLE lineitem_orc
    SELECT L_ORDERKEY as L_ORDERKEY, 
           L_PARTKEY as L_PARTKEY , 
           L_SUPPKEY as L_SUPPKEY,
           L_LINENUMBER as L_LINENUMBER,
           L_QUANTITY as L_QUANTITY, 
           L_EXTENDEDPRICE as L_EXTENDEDPRICE,
           L_DISCOUNT as L_DISCOUNT,
           L_TAX as L_TAX,
           L_RETURNFLAG as L_RETURNFLAG,
           L_LINESTATUS as L_LINESTATUS,
           L_SHIPDATE as L_SHIPDATE,
           L_COMMITDATE as L_COMMITDATE,
           L_RECEIPTDATE as L_RECEIPTDATE, 
           L_SHIPINSTRUCT as L_SHIPINSTRUCT,
           L_SHIPMODE as L_SHIPMODE,
           L_COMMENT as L_COMMENT
    FROM lineitem;

Voit lukea lisää ORC muotoilu [tähän](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+ORC).

##<a name="vectorization"></a>Vectorization

Vectorization avulla käyttänyt 1 024 rivit yhdessä sijaan käsittelyn yhden rivin kerrallaan erän rakenne. Tämä tarkoittaa, että yksinkertaiset toiminnot suoritetaan nopeammin, koska vähemmän sisäinen koodi on suoritettava.

Voit ottaa käyttöön vectorization etuliite rakenteen kyselyn seuraavat asetukset:

    set hive.vectorized.execution.enabled = true;

Lisätietoja on artikkelissa [Vectorized kyselyn suorittamisen](https://cwiki.apache.org/confluence/display/Hive/Vectorized+Query+Execution).


##<a name="other-optimization-methods"></a>Muita tapoja optimointi

On useita optimointi menetelmiä, harkitse, esimerkiksi:

- **Rakenne bucketing:** tekniikka, jonka avulla määritetään ja klusterin suurten tietojoukkojen kyselyn suorituskyvyn parantamiseksi.
- **Liittyä optimointi:** optimointi rakenne on kyselyn suorituksen suunnittelun liitokset tehokkuuden parantamiseksi ja vähentää edellyttää käyttäjän vihjeet. Lisätietoja on artikkelissa [liittyä optimointi](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+JoinOptimization#LanguageManualJoinOptimization-JoinOptimization).
- **Suurenna pienennyslaitteita varten**

##<a id="nextsteps"></a>Seuraavat vaiheet
Tässä artikkelissa on opit eri Yleiset rakenteen kyselyn optimointia vaihtoehtoja. Lisätietoja on seuraavissa artikkeleissa:

- [Käytä HDInsight Apache-rakenne](hdinsight-use-hive.md)
- [Lentotietoihin viiveen analysoiminen käyttämällä HDInsight-rakenne](hdinsight-analyze-flight-delay-data.md)
- [Voit analysoida Twitter-tietoja käyttämällä HDInsight-rakenne](hdinsight-analyze-twitter-data.md)
- [Kyselyn rakenne-konsolin käyttäminen Hadoop HDInsight tunnistimen tietojen analysointi](hdinsight-hive-analyze-sensor-data.md)
- [Rakenteen käyttäminen HDInsight lokit verkkosivuilta analysointia varten](hdinsight-hive-analyze-website-log.md)


[image-hdi-optimize-hive-scaleout_1]: ./media/hdinsight-hadoop-optimize-hive-query/scaleout_1.png
[image-hdi-optimize-hive-scaleout_2]: ./media/hdinsight-hadoop-optimize-hive-query/scaleout_2.png
[image-hdi-optimize-hive-tez_1]: ./media/hdinsight-hadoop-optimize-hive-query/tez_1.png
[image-hdi-optimize-hive-partitioning_1]: ./media/hdinsight-hadoop-optimize-hive-query/partitioning_1.png
