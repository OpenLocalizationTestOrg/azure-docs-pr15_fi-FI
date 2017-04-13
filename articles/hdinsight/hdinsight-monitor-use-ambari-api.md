<properties
    pageTitle="Valvoa Hadoop varausyksiköt Ambari Ohjelmointirajapinnan käyttäminen HDInsight | Microsoft Azure"
    description="Käytä Apache Ambari API luominen, hallinta ja seuranta Hadoop klustereiden. Intuitiivisen operaattori työkalut ja ohjelmointirajapinnan piilottaa Hadoop monimutkaisuutta."
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"
    editor="cgronlun"
    manager="jhubbard"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="jgao"/>

# <a name="monitor-hadoop-clusters-in-hdinsight-using-the-ambari-api"></a>Näytön Hadoop varausyksiköt HDInsight Ambari Ohjelmointirajapinnan käyttäminen

Opettele valvoa HDInsight klustereiden Ambari API.

> [AZURE.NOTE] Tämän artikkelin tiedot on ensisijaisesti Windows-pohjaisesta HDInsight klustereiden, joka tarjoaa Ambari REST-Ohjelmointirajapinnalla vain luku-version. Katso Linux-pohjaiset klustereiden [hallinta Hadoop klustereiden Ambari avulla](hdinsight-hadoop-manage-ambari.md).

## <a name="what-is-ambari"></a>Mikä on Ambari?

[Apache Ambari] [ ambari-home] käytetään valmistelu, hallinta ja seuranta Apache Hadoop klustereiden. Sisältää intuitiivisen sivustokokoelman operaattori työkalut ja lukuisia API, joka piilottaa Hadoop-toiminnon klustereiden yksinkertaistaminen monimutkaisuutta. Saat lisätietoja API [Ambari API viittaus][ambari-api-reference]. 

HDInsight tukee tällä hetkellä vain Ambari seurannan ominaisuus. Ambari API 1.0 tue HDInsight versiot 3.0- ja 2.1 klustereiden. Tässä artikkelissa käsitellään käytettäessä Ambari API-HDInsight versio 3.1 ja 2.1 klustereiden. Kahden avaimen ero on, että jotkin osat ovat muuttuneet uusia ominaisuuksia (esimerkiksi projektin historia-palvelin) myötä. 

**Edellytykset**

Ennen kuin aloitat Tässä opetusohjelmassa, sinun on oltava seuraavasti:

- **Työasema Azure PowerShellin avulla**.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

- (Valinnainen) [cURL] [curl]. Voit asentaa sen Katso [cURL versioista ja ladattavia tiedostoja][curl-download].

    >[AZURE.NOTE] Kun komennolla kääntö Windowsissa, käytä lainausmerkkejä yhden lainausmerkit vaihtoehto arvojen sijaan.

- **Azure HDInsight-klusterin**. Saat ohjeet klusteri valmisteleminen [käyttäminen HDInsight] [ hdinsight-get-started] tai [säännöstä HDInsight klustereiden][hdinsight-provision]. Tarvitset käsitellään opetusohjelman seuraavat tiedot:

    Klusterin-ominaisuus|Azure PowerShell muuttujan nimi|Arvo|Kuvaus
    ---|---|---|---
    HDInsight-klusterinimi|$clusterName||HDInsight-klusterin nimi.
    Klusterin käyttäjänimi|$clusterUsername||Klusterin käyttäjänimi määritetty klusterin luontipäivämäärä.
    Klusterin salasana|$clusterPassword||Klusterin käyttäjän salasana.

    >[AZURE.NOTE] Fill-in-taulukon arvot. Tämä on hyötyä Tässä opetusohjelmassa loppuun.

## <a name="jump-start"></a>Käynnistä siirtyminen

Ambari avulla voit valvoa HDInsight klustereiden useilla tavoilla.

**Azure PowerShellin käyttäminen**

Seuraavassa on Azure PowerShell-komentosarjaa, MapReduce Työn seuranta tiedot *HDInsight 3.1-klusterin.*  Tärkein ero on on erotettu nämä tiedot kuitenkaan service (mieluummin kuin MapReduce).

    $clusterName = "<HDInsightClusterName>"
    $clusterUsername = "<HDInsightClusterUsername>"
    $clusterPassword = "<HDInsightClusterPassword>"

    $ambariUri = "https://$clusterName.azurehdinsight.net:443/ambari"
    $uriJobTracker = "$ambariUri/api/v1/clusters/$clusterName.azurehdinsight.net/services/yarn/components/resourcemanager"

    $passwd = ConvertTo-SecureString $clusterPassword -AsPlainText -Force
    $creds = New-Object System.Management.Automation.PSCredential ($clusterUsername, $passwd)

    $response = Invoke-RestMethod -Method Get -Uri $uriJobTracker -Credential $creds -OutVariable $OozieServerStatus

    $response.metrics.'yarn.queueMetrics'

Seuraavassa on tehostavia MapReduce Työn seuranta tiedot *HDInsight 2.1-klusterin*Azure PowerShell-komentosarjaa:

    $clusterName = "<HDInsightClusterName>"
    $clusterUsername = "<HDInsightClusterUsername>"
    $clusterPassword = "<HDInsightClusterPassword>"

    $ambariUri = "https://$clusterName.azurehdinsight.net:443/ambari"
    $uriJobTracker = "$ambariUri/api/v1/clusters/$clusterName.azurehdinsight.net/services/mapreduce/components/jobtracker"

    $passwd = ConvertTo-SecureString $clusterPassword -AsPlainText -Force
    $creds = New-Object System.Management.Automation.PSCredential ($clusterUsername, $passwd)

    $response = Invoke-RestMethod -Method Get -Uri $uriJobTracker -Credential $creds -OutVariable $OozieServerStatus

    $response.metrics.'mapred.JobTracker'

Tulos on seuraava:

![Jobtracker tulostus][img-jobtracker-output]

**Käytä kääntö**

Seuraavassa on esimerkki käytön klusterin tiedot käyttämällä kääntö:

    curl -u <username>:<password> -k https://<ClusterName>.azurehdinsight.net:443/ambari/api/v1/clusters/<ClusterName>.azurehdinsight.net

Tulos on seuraava:

    {"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/",
     "Clusters":{"cluster_name":"hdi0211v2.azurehdinsight.net","version":"2.1.3.0.432823"},
     "services"[
       {"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/services/hdfs",
        "ServiceInfo":{"cluster_name":"hdi0211v2.azurehdinsight.net","service_name":"hdfs"}},
       {"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/services/mapreduce",
        "ServiceInfo":{"cluster_name":"hdi0211v2.azurehdinsight.net","service_name":"mapreduce"}}],
     "hosts":[
       {"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/hosts/headnode0",
        "Hosts":{"cluster_name":"hdi0211v2.azurehdinsight.net",
                 "host_name":"headnode0"}},
       {"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/hosts/workernode0",
        "Hosts":{"cluster_name":"hdi0211v2.azurehdinsight.net",
                 "host_name":"headnode0.{ClusterDNS}.azurehdinsight.net"}}]}

**10/8/2014 julkaistut Vapauta**:

Käytettäessä Ambari päätepisteen "https://{clusterDns}.azurehdinsight.net/ambari/api/v1/clusters/{clusterDns}.azurehdinsight.net/services/{servicename}/components/{componentname}", *host_name* kentän palauttaa täydellinen toimialuenimi (FQDN) solmun isäntänimi sijaan. Tässä esimerkissä palauttaa ennen 10/8/2014 versio, riittää, että "**headnode0**". 10/8/2014, julkaisemisen jälkeen saat FQDN "**headnode0. {} ClusterDNS} .azurehdinsight .net**", kuten edellisessä esimerkissä. Tämä muutos on pakollinen helpottamiseksi tilanteissa, joissa useista klusterin tietotyypeistä (esimerkiksi HBase ja Hadoop) käyttöön virtual verkon (VNET). Tämä tapahtuu esimerkiksi käytettäessä taustatietokantaan ympäristö HBase Hadoop varten.

## <a name="ambari-monitoring-apis"></a>Ambari API seuranta

Seuraavassa taulukossa on lueteltu joitakin yleisimmät Ambari API puheluiden valvontaan. Saat lisätietoja Ohjelmointirajapinnan [Ambari API viittaus][ambari-api-reference].

Näytön API-kutsu|URI|Kuvaus
---|---|---
Hae klustereiden|`/api/v1/clusters`|
Näytä klusterin tiedot.|`/api/v1/clusters/<ClusterName>.azurehdinsight.net`|klustereiden, palvelujen isännät
Palveluiden|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services`|Palveluihin sisältyy: ään, mapreduce
Services hankkiminen.|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services/<ServiceName>`|
Hae palvelun osat|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services/<ServiceName>/components`|HDFS: namenode datanode<br/>MapReduce: jobtracker; tasktracker
Näytä osan tiedot.|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services/<ServiceName>/components/<ComponentName>`|ServiceComponentInfo Host (isäntä)-komponenttien, arvot
Hae isännät|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts`|headnode0, workernode0
Näytä host tiedot.|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts/<HostName>`|
Hae host osat|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts/<HostName>/host_components`|namenode, resourcemanager
Näytä Host (isäntä)-osan tiedot.|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts/<HostName>/host_components/<ComponentName>`|HostRoles-osan, host, arvot
Hae määritykset|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/configurations`|Config tyypit: core-sivusto, hdfs-sivusto, mapred-sivusto, sivuston rakenne
Pyydä kokoonpanotietoja.|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/configurations?type=<ConfigType>&tag=<VersionName>`|Config tyypit: core-sivusto, hdfs-sivusto, mapred-sivusto, sivuston rakenne


##<a name="next-steps"></a>Seuraavat vaiheet

Olet nyt kuullut käyttämisestä Ambari API puheluiden valvontaan. Lisätietoja on artikkeleissa:

- [Hallitse HDInsight klustereiden Azure-portaalissa][hdinsight-admin-portal]
- [PowerShellin Azure Hdinsightiin klustereiden hallinta][hdinsight-admin-powershell]
- [Hallitse HDInsight klustereiden käyttämällä käyttöliittymä][hdinsight-admin-cli]
- [HDInsight-asiakirjat][hdinsight-documentation]
- [HDInsight käytön aloittaminen][hdinsight-get-started]



[ambari-home]: http://ambari.apache.org/
[ambari-api-reference]: https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md

[curl]: http://curl.haxx.se
[curl-download]: http://curl.haxx.se/download.html

[microsoft-hadoop-SDK]: http://hadoopsdk.codeplex.com/wikipage?title=Ambari%20Monitoring%20Client

[powershell-install]: powershell-install-configure.md
[powershell-script]: http://technet.microsoft.com/library/ee176949.aspx

[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-admin-cli]: hdinsight-administer-use-command-line.md
[hdinsight-documentation]: /documentation/services/hdinsight/
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-provision]: hdinsight-provision-clusters.md

[img-jobtracker-output]: ./media/hdinsight-monitor-use-ambari-api/hdi.ambari.monitor.jobtracker.output.png
