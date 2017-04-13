 <properties
    pageTitle="Hadoop - Hadoop HDInsight-ominaisuudet esittely? | Microsoft Azure"
    description="Tutustu sovelluksen Hadoop, eri aikajaksoille suuri tietojenkäsittely ja analysointia ja Hadoop ekosysteemissä osat HDInsight-pilveen."
    keywords="suuri Tietoanalyysi, hadoop, mikä on hadoop, hadoop-tekniikat, hadoop ekosysteemiin esittely"
    services="hdinsight"
    documentationCenter=""
    authors="cjgronlund"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/21/2016"
   ms.author="cgronlun"/>


# <a name="an-introduction-to-the-hadoop-ecosystem-on-azure-hdinsight"></a>Johdanto Azure Hdinsightiin Hadoop-ekosysteemiin

 Tässä artikkelissa esitellään Hadoop Azure Hdinsightiin, sen ekosysteemiin ja big datasta. Lisätietoja Hadoop-osista, yleisiä termejä ja tilanteita, joissa suuri Tietoanalyysi.

## <a name="what-is-hadoop-on-hdinsight"></a>Hadoop HDInsight-ominaisuudet

Hadoop viittaa Avaa lähde-ohjelmisto, joka on jaettu käsittelyn, tallentaminen kehys ekosysteemiin ja analysointia suurten tietojoukkojen klustereiden tietokoneiden käyttöön. Azure Hdinsightiin tekee Hadoop-osat- **Hortonworks tietojen ympäristö (HDP)** jakauman pilvipalvelussa, ja ottaa käyttöön ja valmistelee hallitun klustereiden hyvin luotettavuutta ja käytettävyyttä.  

Apache Hadoop on suuri tietojen käsittely alkuperäiseen Avaa lähde-projektiin. Seuraavat oli liittyvän ohjelmiston ja osa Hadoop-tekniikat, mukaan lukien Apache rakenne, Apache HBase, Apache ohjattu ja monista muista apuohjelmista kehittäminen. [Yleistä HDInsight Hadoop-ekosysteemiin](#overview) lisätietoja.

## <a name="what-is-big-data"></a>Mikä on big datasta?

Big datasta kuvataan minkä tahansa suuren leipätekstiin digitaaliset tiedot, Twitteriin, syötteen tietoja sivuston selaaminen asiakkaan ja Ostot teollisuuden laitteita tunnistimen tiedot tekstiä. Big datasta voi olla historiallisten (eli tallennetut tiedot) tai reaaliaikaista (eli virtauttaa suoraan lähde). Big datasta kerätään koskaan escalating tietomääristä on yhä suurempi nopeudet ja laajentaminen erilaisia muotoiluja.

Jos suuri tiedot suoritettavia Liiketoimintatieto-tai tietoja, keräämiseen asianmukaisten tietojen ja oikean kysymyksiä. Sinun on myös Varmista, että tiedot ovat käytettävissä, puhdistaa, analysoida ja esittää sitten kätevästi. Jossa big datasta analyysi Hadoop HDInsight voivat auttaa on.

## <a name="overview"></a>HDInsight Hadoop-ekosysteemiin yleiskatsaus

Hdinsightista on suuri Tietoanalyysi nopeasti laajennettavassa Apache Hadoop-tekniikat cloud jakauman Microsoft Azure. Se sisältää käyttöotot Apache ohjattu, HBase, myrsky, Possu, rakenne, Sqoop, Oozie, Ambari ja niin edelleen. HDInsight integroituu myös liiketoimintatietojen (BI) työkaluja kuten Power BI-, Excel-, SQL Server Analysis Services- ja SQL Server Reporting Services.

### <a name="hadoop-hbase-spark-storm-and-customized-clusters"></a>Hadoop, HBase, ohjattu, myrsky ja mukautetun klustereiden

Hdinsightista on Apache Hadoop, ohjattu, HBase tai myrsky Klusterimääritykset. Vaihtoehtoisesti voit [mukauttaa klustereiden komentosarjan toimintojen](hdinsight-hadoop-customize-cluster-linux.md).

* **Hadoop**: luotettavat tiedot tallennustila [HDFS](#hdfs)ja yksinkertainen [MapReduce](#mapreduce) ohjelmoinnin mallin käsittelemään ja analysoida tietoja rinnakkain.

* **<a target="_blank" href="http://spark.apache.org/">Apache ohjattu</a>**: rinnakkainen käsittely kehys, joka tukee ladatun käsittelyn edistää suuri tietojen analysointi-sovellusten suorituskykyä tiedostojen toimii SQL-tietovirta tiedot, ja tietokoneen learning. Katso [yleiskatsaus: mikä on Apache ohjattu HDInsight-?](hdinsight-apache-spark-overview.md)

* **<a target="_blank" href="http://hbase.apache.org/">HBase</a>**: Hadoop, joka tarjoaa paljon rakenteeton ja puolirakenteisia tietoja - mahdollisesti rivien miljardeja RAM ja vahva yhdenmukaisuuden rakennettu NoSQL tietokannan kertaa miljoonia sarakkeet. Artikkelissa [Yleistä HBase-Hdinsightista](hdinsight-hbase-overview.md).

* **<a  target="_blank" href="https://storm.incubator.apache.org/">Apache myrsky</a>**: hajautettu, reaaliaikainen laskenta-järjestelmän käsittelyyn suuri virtaa tiedot nopeasti. Hallitut HDInsight-klusterin tarjota myrsky. Artikkelissa [Analysoi reaaliaikainen tunnistimen tietojen myrsky ja Hadoop avulla](hdinsight-storm-sensor-data-analysis.md).

### <a name="example-customization-scripts"></a>Esimerkki mukauttaminen komentosarjat

Komentosarjatoiminnot ovat komentosarjoja tähän tarkoitukseen suorituksen aikana klusteri valmisteleminen ja avulla voidaan asentaa lisäosat klusterin. Linux-pohjaiset klustereiden ne ovat Bash komentosarjoja.

Seuraavassa esimerkissä komentosarjat ovat HDInsight-saamiesi:

* [Sävy](hdinsight-hadoop-hue-linux.md): A käytetään ottaa yhteyttä klusterin verkkosovellusten joukko. Vain Linux klustereiden.

* [Giraph](hdinsight-hadoop-giraph-install-linux.md): Graph käsittelyn mallin asioita tai henkilöiden väliset yhteydet.

* [R](hdinsight-hadoop-r-scripts-linux.md): Avaa lähde ja ympäristön tilastollinen tietojenkäsittely käyttää tietokoneen learning.

* [Solr](hdinsight-hadoop-solr-install-linux.md): yrityksen asteikko haku-ympäristö, joka sallii koko tekstin Etsi tietoja.

Lisätietoja kehittämisestä komentosarja-toiminnot on artikkelissa [kehitys HDInsight komentosarja-toiminnon](hdinsight-hadoop-script-actions-linux.md).


## <a name="what-are-the-hadoop-components-and-utilities"></a>Mitä Hadoop-osat ja apuohjelmia?

Seuraavat osat ja apuohjelmat sisällytetään HDInsight klustereiden.

* **[Ambari](#ambari)**: klusteri valmisteleminen, hallinta, seuranta ja apuohjelmat.

* **[Avro](#avro)** (Microsoft .NET kirjastoa varten Avro): tietojen Sarjatoiminto Microsoft .NET-ympäristössä.

* **[Rakenne ja HCatalog](#hive)**: Structured Query Language (SQL)-kyselyt ja taulukon ja tallennustilaa hallinta kerroksen.

* **[Mahout](#mahout)**: skaalattava tietokoneeseen Koulujen sovellukset.

* **[MapReduce](#mapreduce)**: vanha raamit Hadoop distributed käsittely ja Resurssienhallinnan. Katso [kuitenkaan](#yarn), seuraavan sukupolven resurssin framework.

* **[Oozie](#oozie)**: työnkulun hallinta.

* **[Pori](#phoenix)**: relaatiotietokannasta kerroksen HBase päälle.

* **[Possu](#pig)**: yksinkertaisempi komentosarjan MapReduce muunnoksia varten.

* **[Sqoop](#sqoop)**: tietojen tuonti ja vienti.

* **[Tez](#tez)**: mahdollistaa tietojen paljon prosessit toimimaan tehokkaasti asteikko.

* **[Kuitenkaan](#yarn)**: Hadoop core kirjasto- ja MapReduce ohjelmiston framework seuraavan sukupolven osana.

* **[ZooKeeper](#zookeeper)**: yhteensovittamisesta hajautettu järjestelmien prosesseista.

> [AZURE.NOTE] Lisätietoja tiettyihin osiin ja versiotiedot on [Hadoop osat, versiotiedot, ja HDInsight-palveluja][component-versioning]

### <a name="ambari"></a>Ambari

Apache Ambari on valmistelu, hallinta ja seuranta Apache Hadoop klustereiden. Sisältää intuitiivisen sivustokokoelman operaattori työkalut ja lukuisia API, joka piilottaa Hadoop-toiminnon klustereiden yksinkertaistaminen monimutkaisuutta. Klustereiden antaa molemmat Linux-pohjaiset HDInsight Ambari web-Käyttöliittymän ja Ambari REST-Ohjelmointirajapinnalla samalla, kun Windows-pohjaisesta klustereiden antaa alijoukkoa REST-Ohjelmointirajapinnalla. Ambari näkymän HDInsight klustereiden Salli laajennus Käyttöliittymän ominaisuuksia.

Katso [hallinta HDInsight klustereiden käyttämällä Ambari](hdinsight-hadoop-manage-ambari.md) (vain Linux), [näytön Hadoop varausyksiköt HDInsight Ambari Ohjelmointirajapinnan käyttäminen](hdinsight-monitor-use-ambari-api.md)ja <a target="_blank" href="https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md">Apache Ambari API-viittaus</a>.

### <a name="avro"></a>Avro (Microsoft .NET Avro kirjasto)

Avro Microsoft .NET-kirjastosta toteuttaa Sarjatoiminto Microsoft .NET-ympäristön Apache Avro compact binaaritietoja Interchange Format-muoto. Se käyttää <a target="_blank" href="http://www.json.org/">JavaScript Object Notation (JSON)</a> Määritä kieli-ympäristöstä riippumattomalla tavalla rakenteen, joka vahinkosuhdetta kielen yhteensopivuus, eli tietoja muuntaa sarjaksi yhden kielen voi lukea toisessa. Muodon yksityiskohtaiset tiedot löytyvät < kohde = _ "tyhjä" Hyperlinkkiviittaus = "http://avro.apache.org/docs/current/spec.html" > Apache Avro määrityksen</a>.
Avro tiedostojen muoto tukee hajautettu MapReduce ohjelmoinnin malli. Tiedostot ovat "jaettavia", mikä tarkoittaa, voit etsiä jotakin tiedoston kohtaa ja aloittaa lukeminen tietyn estosta. Kieliominaisuuksien käyttöönottamisesta on artikkelissa [Serialize tietojen Avro Microsoft .NET-kirjastoon](hdinsight-dotnet-avro-serialization.md).


### <a name="hdfs"></a>HDFS

Hajautettu tiedostojärjestelmän, joka on MapReduce ja, kuitenkaan Hadoop ekosysteemissä core on Hadoop Distributed tiedoston järjestelmän (HDFS). HDFS on Hadoop klustereiden HDInsight-standardin tiedostojärjestelmässä.

### <a name="hive"></a>Rakenne ja HCatalog

<a target="_blank" href="http://hive.apache.org/">Apache rakenne</a> on data warehouse ohjelmisto rakennettu Hadoop, jonka avulla voit kyselyn ja hallinta suuria tietojoukkoja hajautettu tallennustilaan kutsutaan HiveQL kaltaisessa SQL-kielen avulla. Rakenteen Possu, kuten on MapReduce päälle. Suoritettaessa rakenteen kääntää kyselyjen MapReduce työt sarjaksi. Rakenne on käsitteellisesti vähintään relaatiotietokannasta hallintajärjestelmään Possu ja on siksi Lisää jäsenneltyjen tietojen käyttämistä varten. Erimuotoisia tietoja Possu on paras vaihtoehto. Tutustu [rakenteen Hadoop HDInsight kanssa](hdinsight-use-hive.md).

<a target="_blank" href="https://cwiki.apache.org/confluence/display/Hive/HCatalog/">Apache HCatalog</a> on Hadoop, joka esittää käyttäjälle tietojen relaatio näkymän taulukko- ja tallennustilaa hallinta taso. HCatalog voit lukea ja kirjoittaa tiedostoja missä tahansa muodossa, jossa voi kirjoittaa rakenne SerDe (deserializer Sarjatoiminto).

### <a name="mahout"></a>Mahout

<a target="_blank" href="https://mahout.apache.org/">Apache Mahout</a> on konepohjaisten oppimistekniikoiden algoritmit käynnissä olevat Hadoop skaalattava valikoiman. Käytä periaatteet tilastotiedot, tietokoneen learning sovellusten opeta järjestelmien lisätietoja tiedoista ja käyttää aiempia tulosten määrittääkseen tulevissa toiminta. [Luo elokuvan suosituksia käyttämällä Mahout Hadoop-](hdinsight-mahout.md)kohdassa.

### <a name="mapreduce"></a>MapReduce
MapReduce on vanha ohjelmiston raamit Hadoop sovellusten erän prosessin suurten tietojoukkojen rinnakkain kirjoittaminen. MapReduce työn jakaa tietoja on paljon ja järjestää tietoja avain-arvo-pareiksi käsittelyä varten.

[On Hadoop seuraavan sukupolven resurssien hallinta ja sovelluksen kehys- ja MapReduce 2.0 kutsutaan.](#yarn) MapReduce työt suorittaa kuitenkaan.

Saat lisätietoja MapReduce <a target="_blank" href="http://wiki.apache.org/hadoop/MapReduce">MapReduce</a> Hadoop-wikisivun.

### <a name="oozie"></a>Oozie
<a target="_blank" href="http://oozie.apache.org/">Apache Oozie</a> on työnkulun yhteensovittamisesta järjestelmä, joka hallitsee Hadoop työt. Se on integroitu Hadoop-pino ja tukee Hadoop työt MapReduce, Possu, rakenne ja Sqoop. Myös se voidaan ajoittaa työt tietyn järjestelmään, kuten Java-ohjelmia tai shell-komentosarjoja. Katso [Käytä Oozie Hadoop kanssa](hdinsight-use-oozie.md).

### <a name="phoenix"></a>Pori
<a  target="_blank" href="http://phoenix.apache.org/">Apache Pori</a> ylittäneen relaatiotietokannasta kerroksen HBase. Pori sisältää JDBC ohjaimen, jonka avulla käyttäjät voivat kysely ja hallita SQL-taulukot suoraan. Pori kääntää kyselyt ja muut raportit yhdeksi alkuperäisen NoSQL API-puhelut - sijaan käyttämällä MapReduce - käyttöönotto näin nopeampaa sovellusten NoSQL stores. Katso [Käytä Apache Pori ja SQuirreL HBase klustereiden kanssa](hdinsight-hbase-phoenix-squirrel.md).


### <a name="pig"></a>Possu
<a  target="_blank" href="http://pig.apache.org/">Apache Possu</a> on korkean tason ympäristö, jonka avulla voit suorittaa on paljon monimutkaisia MapReduce muunnoksia kutsutaan Possu latinalaisen simple komentosarjakielen avulla. Possu kääntää Possu latinalainen-komentosarjoja, jotta ne suorittaa Hadoop. Voit luoda käyttäjän määrittämiä funktioita (UDF) Laajenna Possu latinalainen. Katso [Hadoop Possu käyttöä](hdinsight-use-pig.md).

### <a name="sqoop"></a>Sqoop
<a  target="_blank" href="http://sqoop.apache.org/">Apache Sqoop</a> on työkalu, että siirrot joukkosähköposti Hadoop ja relaatiotietokannat tällaisten SQL tietojen tai muiden jäsenneltyjen tietojen stores mahdollisimman tehokkaasti. Katso [Käytä Sqoop Hadoop kanssa](hdinsight-use-sqoop.md).

### <a name="tez"></a>Tez
<a  target="_blank" href="http://tez.apache.org/">Apache Tez</a> on rakennettu Hadoop kuitenkaan, joka suorittaa monimutkaisia, asykliset kaavioiden Yleiset Tiedonkäsittelyn sovelluksen-framework. Se on joustavampi ja tehokas seuraaja MapReduce puitteissa, joka mahdollistaa tietojen paljon prosesseja, kuten rakennetta, voit suorittaa tehokkaammin asteikko. Katso ["Käytä Apache Tez parannettu suorituskyky" Käytä rakenne ja HiveQL](hdinsight-use-hive.md#usetez).

### <a name="yarn"></a>KUITENKAAN
Apache on seuraavan sukupolven MapReduce (MapReduce 2.0 tai MRv2) ja tukee tietojen käsittely skenaariot lisäksi MapReduce eräkäsittely suurempi skaalattavuus ja reaaliaikainen käsittely. KUITENKAAN on Resurssienhallinta ja jaetun sovelluksen kehys. MapReduce työt suorittaa kuitenkaan.

Lisätietoja kuitenkaan on artikkelissa <a target="_blank" href="http://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html">Apache Hadoop NextGen MapReduce (kuitenkaan)</a>.


### <a name="zookeeper"></a>ZooKeeper
<a  target="_blank" href="http://zookeeper.apache.org/">Apache ZooKeeper</a> koordinoi prosessit suurten hajautettu järjestelmien jaetun hierarkkisia nimitilan tietojen rekisteri (znodes) avulla. Znodes sisältävät tarvitaan järjestämiseen prosessit metatietoja jonkin verran: tila, sijainti, määrittäminen ja niin edelleen.

## <a name="programming-languages-on-hdinsight"></a>Ohjelmoinnin kielten Hdinsightiin

HDInsight klustereiden--Hadoop-HBase, myrsky ja ohjattu klustereiden--Tukipuhelinnumero ohjelmoinnin kielet, mutta jotkin ei enää asenneta oletusarvoisesti. Kirjastojen moduulit tai pakettien asennu oletusarvoisesti Asenna osa komentosarja-toiminnon avulla. Katso [komentosarjan toiminnon kehitys HDInsight](hdinsight-hadoop-script-actions-linux.md).

### <a name="default-programming-language-support"></a>Oletusarvoinen ohjelmointi kielten tuki

Oletusarvon mukaan HDInsight klusterit tuki:

* Java

* Python

Lisää kieliä voidaan asentaa komentosarja-toimintojen käyttäminen: [komentosarjan toiminnon kehitys HDInsight](hdinsight-hadoop-script-actions-linux.md).

### <a name="java-virtual-machine-jvm-languages"></a>Java virtuaalikoneen (JVM) kielet

Useita kieliä kuin Java suorittamista Java virtual machine (JVM); käynnissä joitakin näiden kielten voi kuitenkin säätää lisäosat klusterin asennettuina.

HDInsight klustereiden tuetaan JVM perustuva seuraavilla kielillä:

* Clojure

* Jython (Python Java)

* Scala

### <a name="hadoop-specific-languages"></a>Hadoop-kohtaisia kielet

HDInsight klustereiden tukee seuraavia kieliä, jotka koskevat Hadoop ekosysteemiin:

* Possu latinalainen Possu töiden

* Rakenteen töiden ja SparkSQL HiveQL


## <a name="advantage"></a>Hadoop pilveen edut

Osana Azure cloud ekosysteemissä Hadoop HDInsight on hyötyä niistä:

* Automaattinen valmisteleminen Hadoop klustereiden. HDInsight klustereiden on helpompi luoda kuin Hadoop klustereiden määrittämisestä manuaalisesti. Lisätietoja on artikkelissa [säännöstä Hadoop varausyksiköt Hdinsightista](hdinsight-hadoop-provision-linux-clusters.md).

* Tila-ja-Nykyaikainen Hadoop osat. Lisätietoja on artikkelissa [Hadoop osat, versiotiedot, ja palveluja HDInsight-][component-versioning].

* Suuren käytettävyyden ja luotettavuuden klustereiden. Lisätietoja [käytettävyys ja luotettavuutta Hadoop varausyksiköt Hdinsightista](hdinsight-high-availability-linux.md) .

* Tehokas ja taloudellisia tietojen tallentaminen ja Azure-Blob-säiliö, Hadoop-yhteensopiva-vaihtoehto. [Käytä Azure-Blob-säiliö kanssa Hadoop HDInsight](hdinsight-hadoop-use-blob-storage.md) lisätietoja.

* Integrointi Azure muita palveluja, mukaan lukien [Web Apps-sovellusten](https://azure.microsoft.com/documentation/services/app-service/web/) ja [SQL-tietokantaan](https://azure.microsoft.com/documentation/services/sql-database/).

* Muita AM kokoja ja käynnissä HDInsight klustereiden tietotyypeistä. [Hadoop osat, versiotiedot,] ja palveluja HDInsight-[ component-versioning] lisätietoja.

* Klusterin skaalaus. Klusterin skaalaus avulla voit muuttaa käynnissä HDInsight-klusterin solmujen määrän eikä sinun tarvitse poistaa tai luoda sen uudelleen.

* Virtuaalinen verkkotuki. HDInsight klustereiden voidaan eristystaso cloud resurssien tai hybrid tilanteissa, joissa on linkki pilveen resurssien verrattuna kaavaan liittyviin oman palvelinkeskuksen tukemaan Azure Virtual verkoston kanssa.

* Alhainen merkinnän kustannukset. Aloittaa [maksuttoman kokeiluversion käyttäjäksi](https://azure.microsoft.com/free/)tai kuulla [HDInsight hinnoittelutiedot](https://azure.microsoft.com/pricing/details/hdinsight/).

Jos haluat lisätietoja Hadoop HDInsight-edut, Lisätietoja on [HDInsight Azure ominaisuussivulla][marketing-page].

## <a name="hdinsight-standard-and-hdinsight-premium"></a>HDInsight vakio- ja HDInsight Premium

Hdinsightista on big datasta cloud tarjouksia vakio- ja Premium luokan. HDInsight Standard sisältää yrityksen asteikko-klusterin joita organisaatiot voivat käyttää niiden big datasta työmääriä suorittamiseen. HDInsight Premium, joka perustuu sekä annetaan advanced analytical ja tietoturvaominaisuudet HDInsight-klusterin. Lisätietoja on artikkelissa [Azure Hdinsightiin Premium](hdinsight-component-versioning.md#hdinsight-standard-and-hdinsight-premium)


## <a id="resources"></a>Lisätietoja suuri tietojen analysointi ja Hadoop HDInsight koulutusresursseja

Muodosta tämän johdanto Hadoop cloud ja big datasta analysointi resurssit alla.


### <a name="hadoop-documentation-for-hdinsight"></a>Hadoop ohjeissa Hdinsightiin

* [HDInsight ohjeissa](https://azure.microsoft.com/documentation/services/hdinsight/): ohjeet sivun Hadoop-Azure Hdinsightiin sekä linkit artikkeleja, videoita ja muita resursseja.

* [Hadoop HDInsight käytön aloittaminen](hdinsight-hadoop-linux-tutorial-get-started.md): Pika-aloitus-opetusohjelma HDInsight Hadoop klustereiden valmistelu ja käynnissä otoksen rakenteen kyselyt.

* [Aloita ohjattu HDInsight-](hdinsight-apache-spark-jupyter-spark-sql.md): quick start opetusohjelma Ohjattu klusterin luomalla ja suorittamalla vuorovaikutteinen ohjattu SQL-kyselyjä.

* [Käytä R palvelimelle HDInsight](hdinsight-hadoop-r-server-get-started.md): HDInsight Premiumin R Serverin käytön aloittaminen.

* [Säännöstä HDInsight klustereiden](hdinsight-hadoop-provision-linux-clusters.md): Microsoft Azure portal, Azure CLI tai PowerShellin Azure Hdinsightiin Hadoop klusterissa-palvelun valmistelua koskeva.


### <a name="apache-hadoop"></a>Apache Hadoop

* <a target="_blank" href="http://hadoop.apache.org/">Apache Hadoop</a>: Lisätietoja Apache Hadoop-ohjelmiston kirjaston framework, joka sallii on paljon hajautettu käsittely klustereiden tietokoneiden välillä.

* <a target="_blank" href="http://hadoop.apache.org/docs/r1.0.4/hdfs_design.html">HDFS</a>: Lisätietoja arkkitehtuuri ja rakenteen tarkistaminen Hadoop Distributed File System, ensisijainen tallennustilan, jota käytetään Hadoop-sovelluksissa.

* <a target="_blank" href="http://hadoop.apache.org/docs/r1.2.1/mapred_tutorial.html">MapReduce opetusohjelma</a>: Lisätietoja ohjelmoinnin framework Hadoop-sovelluksia, jotka käsitellä nopeasti suuria määriä tietoa rinnakkain-suuri klustereiden Laske solmujen kirjoittamista varten.


### <a name="microsoft-business-intelligence"></a>Microsoftin liiketoimintatietojen

Tutut (BI) yritystietotyökalujen – kuten Excel, PowerPivot, SQL Server Analysis Services ja SQL Server Reporting Services - hakea, analysoida ja raporttitietojen integroitu Hdinsightiin Power Query-apuohjelman tai Microsoft-rakenne-ODBC-ohjain.

Nämä Liiketoimintatieto-työkaluista auttaa suuri tietojen analysointiin:

* [Hadoop Power Query muodostaa Excel](hdinsight-connect-excel-power-query.md): opettele muodostamaan Excel Azure-tallennustilan tilin, joka tallentaa HDInsight-klusterin käyttämällä Microsoft Power Query for Excelin liittyviä tietoja. Windows-työasemaan pakollinen. Windows - tai Linux-pohjaiset klusterin toimii.

* [Connect Excel to Microsoft-rakenne-ODBC-ohjaimella Hadoop Excelin](hdinsight-connect-excel-hive-odbc-driver.md): Lisätietoja tietojen tuomisesta Hdinsightista Microsoft-rakenne-ODBC-ohjaimella. Windows-työasemaan pakollinen. Windows - tai Linux-pohjaiset klusterin toimii.

* [Microsoft Cloud-ympäristössä](http://www.microsoft.com/server-cloud/solutions/business-intelligence/default.aspx): tutustu Power BI Office 365: n, Lataa SQL Server-kokeiluversio ja määrittäminen SharePoint Server 2013 ja SQL Server BI.

* [SQL Server Analysis Services](http://msdn.microsoft.com/library/hh231701.aspx).

* [SQL Server Reporting Servicesin](http://msdn.microsoft.com/library/ms159106.aspx).




[marketing-page]: https://azure.microsoft.com/services/hdinsight/
[component-versioning]: hdinsight-component-versioning.md
[zookeeper]: http://zookeeper.apache.org/
