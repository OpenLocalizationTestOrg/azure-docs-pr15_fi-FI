<properties
    pageTitle="Luo HBase klustereiden Virtual verkossa | Microsoft Azure"
    description="Aloita käyttäminen Azure Hdinsightiin HBase. Opettele luomaan HDInsight HBase klustereiden Azure Virtual verkossa."
    keywords=""
    services="hdinsight,virtual-network"
    documentationCenter=""
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/18/2016"
   ms.author="jgao"/>

# <a name="create-hbase-clusters-in-azure-virtual-network"></a>Luo HBase klustereiden Azure Virtual verkossa 

Tietokantakyselyjen luominen Azure Hdinsightiin HBase klustereiden [Azure Virtual Network][1].

VPN-integroinnin HBase klustereiden voidaan ottaa käyttöön virtual samassa verkossa kuin sovelluksia niin, että sovellukset voivat viestiä HBase suoraan. Etuja:

- HBase-klusterin, joka mahdollistaa HBase Java etäproseduurikutsu viestintä solmut verkkosovelluksen suora yhteys Soita API (RPC).
- Parannettu suorituskyky pyytämällä tietoliikenteestä ei siirry useiden yhdyskäytävien ja kuormituksen tasoitusmääritykset päälle.
- Mahdollisuus käsitellä luottamuksellisia tietoja turvallisempi tavalla ilman paljastaa julkisen päätepiste.

###<a name="prerequisites"></a>Edellytykset
Ennen kuin aloitat Tässä opetusohjelmassa, sinun on oltava seuraavasti:

- **Azure-tilaus**. Katso [Hae Azure maksuttoman kokeiluversion](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- **Työasema Azure PowerShellin avulla**. Katso [asentaminen ja käyttäminen PowerShellin Azure](https://azure.microsoft.com/documentation/videos/install-and-use-azure-powershell/). 

## <a name="create-hbase-cluster-into-virtual-network"></a>Luo virtuaalisia verkostoon HBase klusteri

Tässä osassa luodaan Linux-pohjaiset HBase klusterin Azure virtual verkko [Azure Resurssienhallinta mallin](../resource-group-template-deploy.md)riippuvaiset Azure-tallennustilan tilin kanssa. Muut klusterin luominen menetelmistä ja tietoja asetukset-kohdassa [Luo HDInsight klustereiden](hdinsight-hadoop-provision-linux-clusters.md). Lisätietoja mallin avulla voit luoda Hadoop varausyksiköt HDInsight-kohdassa [Luo Hadoop varausyksiköt HDInsight Azure Resurssienhallinta mallien käyttäminen](hdinsight-hadoop-create-windows-clusters-arm-templates.md)

> [AZURE.NOTE] Jotkin ominaisuudet on koodattu malliin. Esimerkki:
>
> * __Sijainti__: itä US 2
> * __Cluster-versio: 3.4
> * __Klusterin työntekijä solmu Laske__: 4
> * __Tallennustilan oletustili__: &lt;klusterinimi > tallentaminen
> * __Virtual verkkonimi__: &lt;klusterinimi >-vnet
> * __Virtual verkko-osoitetilaa__: 10.0.0.0/16
> * __Aliverkon nimi__: oletusarvo
> * __Aliverkon osoitealueita__: 10.0.0.0/24
>
> &lt;Klusterin nimi > korvaa antaisit mallia käytettäessä klusterinimeä.

1. Valitse Avaa malli Azure-portaalissa seuraavassa kuvassa. Malli sijaitsee julkisen blob-säilö. 

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-hbase-cluster-in-vnet.json" target="_blank"><img src="https://acom.azurecomcdn.net/80C57D/cdn/mediahandler/docarticles/dpsmedia-prod/azure.microsoft.com/en-us/documentation/articles/hdinsight-hbase-tutorial-get-started-linux/20160201111850/deploy-to-azure.png" alt="Deploy to Azure"></a>

2. - **Mukautettu käyttöönottoa** sivu Anna seuraavat tiedot:

    - **Tilaus**: Valitse Azure tilauksen avulla luodaan HDInsight-klusterin, riippuvaiset tallennustilan tilin ja Azure virtual verkkoon.
    - **Resurssiryhmä**: **Luo uusi**ja määritä uusi resurssi ryhmänimi.
    - **Sijainti**: Valitse resurssiryhmän sijainti.
    - **ClusterName**: nimi, jonka aiot luoda Hadoop-klusterin.
    - **Klusterin käyttäjätunnus ja salasana**: Kirjautuminen oletusnimi on **järjestelmänvalvoja**.
    - **SSH käyttäjänimi ja salasana**: oletusarvo-käyttäjänimi on **sshuser**.  Voit nimetä sen uudelleen. 
    - **Voin hyväksy käyttöoikeussopimuksen ehdot edellä mainittujen**: (Valitse)
    

3. Valitse **Osta**. Kestää noin 20 minuutin klusterin luomiseen. Kun klusterin on luotu, voit valita Avaa portaalissa klusterin-sivu.

Kun olet suorittanut opetusohjelman, haluat ehkä poistaa klusterin. HDInsight, jossa tiedot on tallennettu Azure-tallennustilan, joten voit poistaa klusterin turvallisesti, kun se ei ole käytössä. Myös perittävän HDInsight-klusterin, vaikka se ei ole käytössä. Koska klusterin kulut on monta kertaa enemmän kuin tallennustilan kulut, kannattaa taloudellisen poistaa klustereiden, kun he eivät ole käytössä. Poistoprosessi klusterin ohjeita on artikkelissa [Hallitse Hadoop varausyksiköt HDInsight mukaan Azure-portaalissa](hdinsight-administer-use-management-portal.md#delete-clusters).

Voit aloittaa työskentelyn uuden HBase-klusterin [käyttäminen HBase kanssa Hadoop HDInsight](hdinsight-hbase-tutorial-get-started.md)-kohdan ohjeita.

## <a name="connect-to-the-hbase-cluster-using-hbase-java-rpc-apis"></a>Yhteyden muodostaminen HBase klusterin HBase Java RPC ohjelmointirajapinnan käyttäminen

1.  Luo infrastruktuuri service (IaaS) virtual koneen Azure virtual samassa verkossa ja saman aliverkon. Ohjeita uuden IaaS virtual koneen luomisesta on artikkelissa [Create virtuaalikoneen käytössä Windows Server](../virtual-machines/virtual-machines-windows-hero-tutorial.md). Kun noudattamalla tässä asiakirjassa, sinun on käytettävä verkon määritysten seuraavasti:

    - __Virtual verkon__: &lt;klusterinimi >-vnet
    - __Aliverkon__: oletusarvo

    > [AZURE.IMPORTANT] Korvaa &lt;klusterinimi > nimellä käytit, kun HDInsight-klusterin luominen edellä kuvatut vaiheet.

    Nämä arvot määrittäminen käyttämään virtual samassa verkossa ja aliverkon HDInsight-klusterin virtuaalikoneen. Näin ne suoraan keskenään.

2.  Kun HBase etäkäyttö Java-sovelluksen avulla, sinun on käytettävä täydellinen toimialuenimi (FQDN). Voit tarkistaa tämän saa HBase klusterin yhteyteen liittyvän DNS-liitettä. Saadakseen ne jollakin seuraavista tavoista:

    * Soittaminen Ambari selaimen avulla:
    
        Siirry https://&lt;ClusterName >.azurehdinsight.net/api/v1/clusters/&lt;ClusterName > / hosts?minimal_response = true. Poistaa JSON-tiedosto, jonka DNS-liitteet.

    * Käytä Ambari sivustoa:

        1. Siirry https://&lt;ClusterName >. azurehdinsight.net.
        2. Valitse yläreunan valikosta **isännät** .

    * Käytä kääntö REST-puheluissa:

            curl -u <username>:<password> -k https://<clustername>.azurehdinsight.net/ambari/api/v1/clusters/<clustername>.azurehdinsight.net/services/hbase/components/hbrest

        Etsi JavaScript Object Notation (JSON) tiedot on palautettu, "host_name" tapahtuma. Tämä sisältää klusterin solmujen täydellinen toimialuenimi. Esimerkki:

            ...
            "host_name": "wordkernode0.<clustername>.b1.cloudapp.net
            ...

        Toimialueen nimi alkaa klusterinimeä osa on DNS-liitettä. Esimerkiksi mycluster.b1.cloudapp.net.

    * Azure PowerShellin käyttäminen
    
        Käytä seuraavaa Azure PowerShell-komentosarjaa rekisteröidä **Get-ClusterDetail** -funktiota, joka voidaan palauttaa DNS-liite:

            function Get-ClusterDetail(
                [String]
                [Parameter( Position=0, Mandatory=$true )]
                $ClusterDnsName,
                [String]
                [Parameter( Position=1, Mandatory=$true )]
                $Username,
                [String]
                [Parameter( Position=2, Mandatory=$true )]
                $Password,
                [String]
                [Parameter( Position=3, Mandatory=$true )]
                $PropertyName
                )
            {
            <#
                .SYNOPSIS
                 Displays information to facilitate an HDInsight cluster-to-cluster scenario within the same virtual network.
                .Description
                 This command shows the following 4 properties of an HDInsight cluster:
                 1. ZookeeperQuorum (supports only HBase type cluster)
                    Shows the value of HBase property "hbase.zookeeper.quorum".
                 2. ZookeeperClientPort (supports only HBase type cluster)
                    Shows the value of HBase property "hbase.zookeeper.property.clientPort".
                 3. HBaseRestServers (supports only HBase type cluster)
                    Shows a list of host FQDNs that run the HBase REST server.
                 4. FQDNSuffix (supports all cluster types)
                    Shows the FQDN suffix of hosts in the cluster.
                .EXAMPLE
                 Get-ClusterDetail -ClusterDnsName {clusterDnsName} -Username {username} -Password {password} -PropertyName ZookeeperQuorum
                 This command shows the value of HBase property "hbase.zookeeper.quorum".
                .EXAMPLE
                 Get-ClusterDetail -ClusterDnsName {clusterDnsName} -Username {username} -Password {password} -PropertyName ZookeeperClientPort
                 This command shows the value of HBase property "hbase.zookeeper.property.clientPort".
                .EXAMPLE
                 Get-ClusterDetail -ClusterDnsName {clusterDnsName} -Username {username} -Password {password} -PropertyName HBaseRestServers
                 This command shows a list of host FQDNs that run the HBase REST server.
                .EXAMPLE
                 Get-ClusterDetail -ClusterDnsName {clusterDnsName} -Username {username} -Password {password} -PropertyName FQDNSuffix
                 This command shows the FQDN suffix of hosts in the cluster.
            #>

                $DnsSuffix = ".azurehdinsight.net"

                $ClusterFQDN = $ClusterDnsName + $DnsSuffix
                $webclient = new-object System.Net.WebClient
                $webclient.Credentials = new-object System.Net.NetworkCredential($Username, $Password)

                if($PropertyName -eq "ZookeeperQuorum")
                {
                    $Url = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/configurations?type=hbase-site&tag=default&fields=items/properties/hbase.zookeeper.quorum"
                    $Response = $webclient.DownloadString($Url)
                    $JsonObject = $Response | ConvertFrom-Json
                    Write-host $JsonObject.items[0].properties.'hbase.zookeeper.quorum'
                }
                if($PropertyName -eq "ZookeeperClientPort")
                {
                    $Url = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/configurations?type=hbase-site&tag=default&fields=items/properties/hbase.zookeeper.property.clientPort"
                    $Response = $webclient.DownloadString($Url)
                    $JsonObject = $Response | ConvertFrom-Json
                    Write-host $JsonObject.items[0].properties.'hbase.zookeeper.property.clientPort'
                }
                if($PropertyName -eq "HBaseRestServers")
                {
                    $Url1 = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/configurations?type=hbase-site&tag=default&fields=items/properties/hbase.rest.port"
                    $Response1 = $webclient.DownloadString($Url1)
                    $JsonObject1 = $Response1 | ConvertFrom-Json
                    $PortNumber = $JsonObject1.items[0].properties.'hbase.rest.port'

                    $Url2 = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/services/hbase/components/hbrest"
                    $Response2 = $webclient.DownloadString($Url2)
                    $JsonObject2 = $Response2 | ConvertFrom-Json
                    foreach ($host_component in $JsonObject2.host_components)
                    {
                        $ConnectionString = $host_component.HostRoles.host_name + ":" + $PortNumber
                        Write-host $ConnectionString
                    }
                }
                if($PropertyName -eq "FQDNSuffix")
                {
                    $Url = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/services/YARN/components/RESOURCEMANAGER"
                    $Response = $webclient.DownloadString($Url)
                    $JsonObject = $Response | ConvertFrom-Json
                    $FQDN = $JsonObject.host_components[0].HostRoles.host_name
                    $pos = $FQDN.IndexOf(".")
                    $Suffix = $FQDN.Substring($pos + 1)
                    Write-host $Suffix
                }
            }

        Kun olet suorittanut Azure PowerShell-komentosarjaa, käyttämällä seuraavaa komentoa DNS-liite **Get-ClusterDetail** -funktiolla. Määritä HDInsight HBase klusterin nimesi, järjestelmänvalvojan nimi ja järjestelmänvalvojan salasanan, jos tällä komennolla.

            Get-ClusterDetail -ClusterDnsName <yourclustername> -PropertyName FQDNSuffix -Username <clusteradmin> -Password <clusteradminpassword>

        Tämä palauttaa DNS-liitettä. Esimerkiksi **yourclustername.b4.internal.cloudapp.net**.

    * Käytä RDP
    
        Voit myös käyttää etätyöpöydän muodostaa HBase klusterin (yhteys muodostetaan pään solmun) ja suorita **ipconfig** komentoriviltä, voit hankkia DNS-liitettä. Remote Desktop Protocol (RDP) ottaminen käyttöön ja yhteyden muodostaminen klusterin avulla RDP-ohjeet ovat artikkelissa [Hadoop hallinta-portaalissa Azure Hdinsightiin varausyksiköt][hdinsight-admin-portal].
        
        ![hdinsight.hbase.DNS.surffix][img-dns-surffix]


<!--
3.  Change the primary DNS suffix configuration of the virtual machine. This enables the virtual machine to automatically resolve the host name of the HBase cluster without explicit specification of the suffix. For example, the *workernode0* host name will be correctly resolved to workernode0 of the HBase cluster.

    To make the configuration change:

    1. RDP into the virtual machine.
    2. Open **Local Group Policy Editor**. The executable is gpedit.msc.
    3. Expand **Computer Configuration**, expand **Administrative Templates**, expand **Network**, and then click **DNS Client**.
    - Set **Primary DNS Suffix** to the value obtained in step 2:

        ![hdinsight.hbase.primary.dns.suffix][img-primary-dns-suffix]
    4. Click **OK**.
    5. Reboot the virtual machine.
-->

Voit varmistaa, että virtuaalikoneen voivat viestiä HBase klusterin, komennolla `ping headnode0.<dns suffix>` virtual tietokoneesta. Esimerkiksi ping headnode0.mycluster.b1.cloudapp.net.

Voit käyttää näitä tietoja Java-sovelluksessa, voit noudattaa ohjeita voit luoda sovelluksen [Maven-testi avulla voit luoda Java-sovelluksia, jotka käyttävät HBase HDInsight (Hadoop) kanssa](hdinsight-hbase-build-java-maven.md) . Jos haluat muodostaa yhteyden etäpalvelimeen HBase sovellus, Muokkaa tässä esimerkissä FQDN käytettävät Zookeeper **hbase site.xml** -tiedostoa. Esimerkki:

    <property>
        <name>hbase.zookeeper.quorum</name>
        <value>zookeeper0.<dns suffix>,zookeeper1.<dns suffix>,zookeeper2.<dns suffix></value>
    </property>

> [AZURE.NOTE] Lisätietoja Azure-nimenselvitys virtual verkot, sekä omia DNS-palvelin on [Nimi tarkkuus (DNS)](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).

##<a name="next-steps"></a>Seuraavat vaiheet

Tässä opetusohjelmassa opit HBase-klusterin luominen. Lisätietoja on artikkeleissa:

- [HDInsight käytön aloittaminen](hdinsight-hadoop-linux-tutorial-get-started.md)
- [Määritä HBase replikoinnin Hdinsightiin](hdinsight-hbase-geo-replication.md)
- [Luo Hadoop klustereiden Hdinsightiin](hdinsight-provision-clusters.md)
- [Aloita käyttäminen Hadoop HDInsight HBase](hdinsight-hbase-tutorial-get-started.md)
- [Analysoi Twitter markkinatunnelma HBase HDInsight kanssa](hdinsight-hbase-analyze-twitter-sentiment.md)
- [VPN-yleiskatsaus][vnet-overview]


[1]: http://azure.microsoft.com/services/virtual-network/
[2]: http://technet.microsoft.com/library/ee176961.aspx
[3]: http://technet.microsoft.com/library/hh847889.aspx

[hbase-get-started]: hdinsight-hbase-tutorial-get-started.md
[hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md
[vnet-overview]: ../virtual-network/virtual-networks-overview.md
[vm-create]: ../virtual-machines/virtual-machines-windows-hero-tutorial.md

[azure-portal]: https://portal.azure.com
[azure-create-storageaccount]: ../storage-create-storage-account.md
[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md#rdp

[hdinsight-powershell-reference]: https://msdn.microsoft.com/library/dn858087.aspx


[twitter-streaming-api]: https://dev.twitter.com/docs/streaming-apis
[twitter-statuses-filter]: https://dev.twitter.com/docs/api/1.1/post/statuses/filter


[powershell-install]: powershell-install-configure.md


[hdinsight-customize-cluster]: hdinsight-hadoop-customize-cluster.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-storage-powershell]: ../hdinsight-hadoop-use-blob-storage.md#powershell
[hdinsight-analyze-flight-delay-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-storage]: ../hdinsight-hadoop-use-blob-storage.md
[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-power-query]: hdinsight-connect-excel-power-query.md
[hdinsight-hive-odbc]: hdinsight-connect-excel-hive-ODBC-driver.md
[hdinsight-hbase-replication-dns]: hdinsight-hbase-geo-replication-configure-DNS.md

[img-dns-surffix]: ./media/hdinsight-hbase-provision-vnet/DNSSuffix.png
[img-primary-dns-suffix]: ./media/hdinsight-hbase-provision-vnet/PrimaryDNSSuffix.png
[img-provision-cluster-page1]: ./media/hdinsight-hbase-provision-vnet/hbasewizard1.png "Uuden HBase-klusterin tietojen valmisteleminen"
[img-provision-cluster-page5]: ./media/hdinsight-hbase-provision-vnet/hbasewizard5.png "Voit mukauttaa HBase-klusterin komentosarja-toiminnon avulla"

[azure-preview-portal]: https://portal.azure.com
