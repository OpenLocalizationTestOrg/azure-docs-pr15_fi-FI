<properties
    pageTitle="Hadoop saatavuuden klusterit HDInsight | Microsoft Azure"
    description="HDInsight ottaa käyttöön laajasti käytettävissä ja luotettava klustereiden muita pään solmu kanssa."
    services="hdinsight"
    tags="azure-portal"
    editor="cgronlun"
    manager="jhubbard"
    authors="mumian"
    documentationCenter=""/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/21/2016"
    ms.author="jgao"/>


#<a name="availability-and-reliability-of-windows-based-hadoop-clusters-in-hdinsight"></a>Käytettävyys ja Windows-pohjaisesta Hadoop varausyksiköt HDInsight luotettavuutta


>[AZURE.NOTE] Tässä asiakirjassa vaiheet ovat Windows-pohjaisesta HDInsight klustereiden. Jos käytössäsi on Linux-pohjaiset klusterin, saat Linux koskevien tietojen [käytettävyys ja luotettavuutta Linux-pohjaiset Hadoop varausyksiköt Hdinsightista](hdinsight-high-availability-linux.md) .

HDInsight avulla asiakkaat voivat ottaa klusterin tyypit, eri analytics työmääriä eri. Klusterin tarjota tänään ovat Hadoop klustereiden kyselyn ja analyysi työmääriä, HBase klustereiden NoSQL työmääriä, ja myrsky klustereiden reaaliaikainen käsittely työmääriä varten. Tietyn klusterin tyyppi-alueella on eri rooleja eri solmujen. Esimerkki:



- Hadoop klustereiden varten Hdinsightista on otettu käyttöön ja kaksi rooleja:
    - Pää-solmu (2 solmujen)
    - Tietoja solmu (vähintään 1 solmu)

- Kolme roolien HBase klustereiden varten Hdinsightista on otettu käyttöön:
    - Pää-palvelimet (2 solmujen)
    - Alueen servers (vähintään 1 solmu)
    - Perustyyli/Zookeeper solmujen (3 solmujen)

- Kolme roolien myrsky klustereiden varten Hdinsightista on otettu käyttöön:
    - Nimbus solmujen (2 solmujen)
    - Valvojan servers (vähintään 1 solmu)
    - Zookeeper solmujen (3 solmujen)

Vakio käyttöotot Hadoop klustereiden on yleensä yksittäinen pään solmu. HDInsight poistaa yksittäisen vaiheessa virheen kanssa lisäämällä toisen pään solmu /-HEAD palvelin/Nimbus solmu käytettävyyden ja hallita työmääriä palvelua luotettavuutta. Nämä pään solmujen/head palvelinten/Nimbus solmut on suunniteltu hallinta työntekijä solmujen virheen ongelmitta, mutta kaikki perusmuodon palveluita pään solmun katkokset aiheuttaa klusterin lakkaavat toimimasta.


[ZooKeeper](http://zookeeper.apache.org/ ) solmujen (ZKs) on lisätty ja käytetään Täytemerkki osavaltioiden pään solmujen ja haluat varmistaa, että työntekijä solmujen ja yhdyskäytävien (GWs) tiedät, milloin ei onnistu päälle toissijainen pään solmu (pää Node1), kun aktiivinen pään solmu (pää Node0) poistuu käytöstä.

![Kaavio HDInsight Hadoop-käyttöönoton erittäin luotettavaa pään solmujen.](./media/hdinsight-high-availability/hadoop.high.availability.architecture.diagram.png)




## <a name="check-active-head-node-service-status"></a>Aktiivinen pään solmu palvelun tilan tarkistaminen
Voit selvittää, mitkä pään solmu on aktiivinen ja tarkistaa pään solmun käynnissä olevat palvelujen tila, Hadoop-klusterin täytyy muodostaa Remote Desktop Protocol (RDP) avulla. RDP-ohjeita on kohdassa [hallinta Hadoop varausyksiköt HDInsight mukaan Azure-portaalissa](hdinsight-administer-use-management-portal.md#connect-to-hdinsight-clusters-by-using-rdp). Kun olet muodostanut yhteyden klusterin, kaksoisnapsauttamalla työpöydällä hankkiminen pään solmun tilan Namenode **Hadoop palvelun tavoitettavissa** -kuvake, Jobtracker, Templeton, Oozieservice, Metastore ja Hiveserver2 palvelut ovat käynnissä tai HDI 3.0, Namenode, Resurssienhallinta, historia-palvelimeen, Templeton, Oozieservice, Metastore ja Hiveserver2 Services.

![](./media/hdinsight-high-availability/Hadoop.Service.Availability.Status.png)

Aktiivinen pään solmu on näyttökuvan, *headnode0*.

## <a name="access-log-files-on-the-secondary-head-node"></a>Access-lokitiedostojen toisen pään solmun

Työn käyttämään toissijainen pään solmu kirjautuu silloin, kun se on tullut selaaminen JobTracker Käyttöliittymän aktiivinen pään solmu toimii edelleen kuin ensisijainen aktiivinen solmu. Jos haluat käyttää JobTracker, täytyy muodostaa Hadoop-klusterin käyttämällä RDP edellisessä kohdassa kuvatulla tavalla. Kun siirtää etäälle klusterin kyselyjä, kaksoisnapsauttamalla työpöydällä **Hadoop nimi solmu tila** -kuvaketta ja valitse Hae Valitse Toissijainen pään solmu hakemiston **NameNode lokitiedot** .

![](./media/hdinsight-high-availability/Hadoop.Head.Node.Log.Files.png)


## <a name="configure-head-node-size"></a>Pään solmun koon määrittäminen
Pään solmujen kohdistetaan suuri näennäiskoneiden (VMs) kuin oletusarvoisesti. Tämä koko on riittävä useimmissa Hadoop työt suorittaa klusterin hallintaa varten. Mutta tilanteita, joissa voi vaatia suuria VMs pään solmujen. Esimerkki on, jos klusterin on paljon pieni Oozie töiden hallinta.

Suurien VMs voi määrittää käyttämällä Azure PowerShellin cmdlet-komennot tai HDInsight SDK-paketissa.

Luominen ja valmisteleminen klusterin Azure PowerShell-toiminnolla on kuvattu [HDInsight hallinta PowerShellin avulla](hdinsight-administer-use-powershell.md). Suuria pään solmun määritys edellyttää lisäämällä `-HeadNodeVMSize ExtraLarge` parametri `New-AzureRmHDInsightcluster` käyttää tätä cmdlet-komento.

    # Create a new HDInsight cluster in Azure PowerShell
    # Configured with an ExtraLarge head-node VM
    New-AzureRmHDInsightCluster `
                -ResourceGroupName $resourceGroupName `
                -ClusterName $clusterName ` 
                -Location $location `
                -HeadNodeVMSize ExtraLarge `
                -DefaultStorageAccountName "$storageAccountName.blob.core.windows.net" `
                -DefaultStorageAccountKey $storageAccountKey `
                -DefaultStorageContainerName $containerName  `
                -ClusterSizeInNodes $clusterNodes

SDK-paketissa Tarinan on samanlainen. Luominen ja valmisteleminen klusterin käyttämällä SDK on kuvattu [HDInsight .NET SDK: N avulla](hdinsight-provision-clusters.md#sdk). Suuria pään solmun määritys edellyttää lisäämällä `HeadNodeSize = NodeVMSize.ExtraLarge` parametri `ClusterCreateParameters()` koodi-menetelmää.

    # Create a new HDInsight cluster with the HDInsight SDK
    # Configured with an ExtraLarge head-node VM
    ClusterCreateParameters clusterInfo = new ClusterCreateParameters()
    {
        Name = clustername,
        Location = location,
        HeadNodeSize = NodeVMSize.ExtraLarge,
        DefaultStorageAccountName = storageaccountname,
        DefaultStorageAccountKey = storageaccountkey,
        DefaultStorageContainer = containername,
        UserName = username,
        Password = password,
        ClusterSizeInNodes = clustersize
    };


## <a name="next-steps"></a>Seuraavat vaiheet

- [Apache ZooKeeper](http://zookeeper.apache.org/ )
- [Yhteyden muodostaminen HDInsight klustereiden RDP käyttäminen](hdinsight-administer-use-management-portal.md#rdp)
- [HDInsight .NET SDK: N avulla](hdinsight-provision-clusters.md#sdk)
