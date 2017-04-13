<properties 
   pageTitle="Määritä HBase replikointi kahden palvelinkeskusten välillä | Microsoft Azure" 
   description="Opettele määrittämään HBase replikoinnin yli kaksi keskikohdan mukaan ja käytä tapauksissa klusterin replikoinnin." 
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
   ms.date="07/25/2016"
   ms.author="jgao"/>

# <a name="configure-hbase-geo-replication-in-hdinsight"></a>Määrittää HBase geo-replikoinnin Hdinsightiin

> [AZURE.SELECTOR]
- [Määritä VPN-yhteys](../hdinsight-hbase-geo-replication-configure-vnets.md)
- [DNS-asetusten määrittäminen](hdinsight-hbase-geo-replication-configure-dns.md)
- [Määrittää HBase replikoinnin.](hdinsight-hbase-geo-replication.md) 
 
Opettele määrittämään HBase replikoinnin yli kaksi keskikohdan mukaan. Jotkin käyttötapauksiin klusterin replikoinnin mukana:

- Varmuuskopiointi- ja tietojen palauttaminen
- Tietojen koostaminen
- Maantieteellisten tietojen jakelu
- Online-tietoihin nieltynä yhdistetty offline-tietojen analytics

Klusterin replikointi käyttää lähde push-menetelmää. HBase-klusterin voi olla lähde-kohteeseen, tai voit täyttää molemmat roolit samalla kertaa. Replikoinnin on asynkroninen ja lähinnä potentiaalisen yhdenmukaisuuden replikoinnin. Kun lähteen vastaanottaa sarakkeen tuoteperheeseen muokkauksen toistot sallittuja käytössä, kaikki kohde klustereiden muokkaukseen on välitetty. Kun tiedot on replikoida yhden klusterin toiseen, jotta replikoinnin silmukoita seurataan lähde-klusterin ja kaikki klustereiden, joka on jo käytetty tiedot. Lisätietoja on tässä opetusohjelmassa määrität lähde-replikoinnin.  Katso klusterin muiden topologioissa [Apache HBase opas](http://hbase.apache.org/book.html#_cluster_replication).

Tämä on sarjan kolmas osa:

- [Määritä VPN-yhteys kahden virtual verkon välillä][hdinsight-hbase-replication-vnet]
- [DNS-asetusten määrittäminen virtual-verkoissa][hdinsight-hbase-replication-dns]
- Määrittää HBase geo replikoinnin (Tässä opetusohjelmassa)

Seuraavassa kaaviossa on kuvattu kaksi virtual verkkojen ja [Määritä VPN-yhteys kahden virtual verkon välillä] on luotu verkkoyhteyden[ hdinsight-hbase-geo-replication-vnet] ja [Määrittää DNS-virtual verkkojen][hdinsight-hbase-replication-dns]: 

![HDInsight HBase replikoinnin virtual verkkokaavio][img-vnet-diagram]

## <a id="prerequisites"></a>Edellytykset

Ennen kuin aloitat Tässä opetusohjelmassa, sinun on oltava seuraavasti:

- **Azure-tilaus**. Katso [Hae Azure maksuttoman kokeiluversion](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- **Työasema Azure PowerShellin avulla**.

    PowerShell-komentosarjojen suorittamiseen PowerShellin Azure Suorita järjestelmänvalvojana ja suorittamisen käytännön asettaminen *RemoteSigned*. Katso määrittäminen suorituskäytäntöä cmdlet-komennolla.
    
    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

- **Kaksi Azure virtual verkon VPN-yhteyden ja määrittänyt DNS**.  Katso ohjeet [kahden Azure virtual verkon välillä VPN-yhteyden määrittäminen][hdinsight-hbase-replication-vnet], ja [Määrittää DNS-kaksi Azure virtual verkkojen välillä][hdinsight-hbase-replication-dns].


    Ennen kuin suoritat PowerShell-komentosarjojen, varmista, että olet muodostanut yhteyden Azure tilauksen seuraavat cmdlet-komennolla:

        Add-AzureAccount

    Jos sinulla on useita tilauksia Azure, seuraavan cmdlet-komennon avulla voit määrittää voimassa oleva tilaus:

        Select-AzureSubscription <AzureSubscriptionName>



## <a name="provision-hbase-clusters-in-hdinsight"></a>Valmistele HBase varausyksiköt Hdinsightiin

[Kaksi Azure virtual verkkojen välillä VPN-yhteyden]määrittäminen[hdinsight-hbase-replication-vnet], olet luonut virtual verkko Euroopan tietokeskuksen ja Yhdysvaltain tietokeskuksen virtual verkkoon. Kaksi virtual verkon muodostanut VPN-verkon kautta. Tässä istunnossa valmistella HBase-klusterin kussakin virtual verkot. Jäljempänä tässä opetusohjelmassa-maksat jokin HBase klustereiden replikoiminen HBase-klusterin.

Perinteinen Azure-portaalissa ei tue valmistelu HDInsight klustereiden kanssa mukautettuja asetuksia. Määritä esimerkiksi *hbase.replication* *Tosi*. Jos määrität arvon määritystiedostossa jälkeen klusteriin on valmisteltu, menetät asetus, kun klusterin on parhaillaan reimaged. Katso lisätietoja [säännöstä Hadoop varausyksiköt HDInsight][hdinsight-provision]. Jokin vaihtoehdoista valmistelu HDInsight klusteri mukautettuja asetuksia ja käyttää PowerShellin Azure.


**HBase-klusterin Contoso-VNet-EU valmistelu** 

1. Avaa Windows PowerShell ise: työaseman.
2. Määritä muuttujat komentosarja alussa ja suorittamalla komentosarja.

        # create hbase cluster with replication enabled
        
        $azureSubscriptionName = "[AzureSubscriptionName]"
        
        $hbaseClusterName = "Contoso-HBase-EU" # This is the HBase cluster name to be used.
        $hbaseClusterSize = 1   # You must provision a cluster that contains at least 3 nodes for high availability of the HBase services.
        $hadoopUserLogin = "[HadoopUserName]"
        $hadoopUserpw = "[HadoopUserPassword]"
        
        $vNetName = "Contoso-VNet-EU"  # This name must match your Europe virtual network name.
        $subNetName = 'Subnet-1' # This name must match your Europe virtual network subnet name.  The default name is "Subnet-1".
        
        $storageAccountName = 'ContosoStoreEU'  # The script will create this storage account for you.  The storage account name doesn't support hyphens. 
        $storageAccountName = $storageAccountName.ToLower() #Storage account name must be in lower case.
        $blobContainerName = $hbaseClusterName.ToLower()  #Use the cluster name as the default container name.
        
        #connect to your Azure subscription
        Add-AzureAccount 
        Select-AzureSubscription $azureSubscriptionName
        
        # Create a storage account used by the HBase cluster
        $location = Get-AzureVNetSite -VNetName $vNetName | %{$_.Location} # use the virtual network location
        New-AzureStorageAccount -StorageAccountName $storageAccountName -Location $location
        
        # Create a blob container used by the HBase cluster
        $storageAccountKey = Get-AzureStorageKey -StorageAccountName $storageAccountName | %{$_.Primary}
        $storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
        New-AzureStorageContainer -Name $blobContainerName -Context $storageContext
        
        # Create provision configuration object
        $hbaseConfigValues = new-object 'Microsoft.WindowsAzure.Management.HDInsight.Cmdlet.DataObjects.AzureHDInsightHBaseConfiguration'
            
        $hbaseConfigValues.Configuration = @{ "hbase.replication"="true" } #this modifies the hbase-site.xml file
        
        # retrieve vnet id based on vnetname
        $vNetID = Get-AzureVNetSite -VNetName $vNetName | %{$_.id}
        
        $config = New-AzureHDInsightClusterConfig `
                        -ClusterSizeInNodes $hbaseClusterSize `
                        -ClusterType HBase `
                        -VirtualNetworkId $vNetID `
                        -SubnetName $subNetName `
                    | Set-AzureHDInsightDefaultStorage `
                        -StorageAccountName $storageAccountName `
                        -StorageAccountKey $storageAccountKey `
                        -StorageContainerName $blobContainerName `
                    | Add-AzureHDInsightConfigValues `
                        -HBase $hbaseConfigValues
        
        # provision HDInsight cluster
        $hadoopUserPassword = ConvertTo-SecureString -String $hadoopUserpw -AsPlainText -Force     
        $credential = New-Object System.Management.Automation.PSCredential($hadoopUserLogin,$hadoopUserPassword)
        
        $config | New-AzureHDInsightCluster -Name $hbaseClusterName -Location $location -Credential $credential



**HBase-klusterin Contoso-VNet-fi-valmistelu** 

- Käytä samaa komentosarjaa seuraavin arvoin:

        $hbaseClusterName = "Contoso-HBase-US" # This is the HBase cluster name to be used.
        $vNetName = "Contoso-VNet-US"  # This name must match your Europe virtual network name.
        $storageAccountName = 'ContosoStoreUS'  

    Koska olet jo muodostanut yhteyden Azure-tili, sinun ei tarvitse suorittaa seuraavat comlets enää:

        Add-AzureAccount 
        Select-AzureSubscription $azureSubscriptionName




## <a name="configure-dns-conditional-forwarder"></a>Määritä DNS ehdollinen forwarder

[Määrittää]DNS-palvelimesta virtual verkkojen[hdinsight-hbase-replication-dns], olet määrittänyt DNS-palvelimet useita verkkoja. HBase klustereiden on toimialueesta liitteet. Niin haluat määrittää muita ehdollinen DNS-välittäjiä.

Ehdollinen forwarder määrittämiseen, sinun tarvitsee tietää kaksi HBase klustereiden toimialueen liitteitä. 

**Löydät kaksi HBase klustereiden toimialueliitteet**

1. RDP **Contoso-HBase-EU**kyselyjä.  Katso ohjeet [HDInsight käyttämällä perinteinen Azure-portaalin hallinta Hadoop varausyksiköt][hdinsight-manage-portal]. Se on todella headnode0 klusterin.
2. Avaa Windows PowerShell console tai komentorivi-ikkuna.
3. Suorita **ipconfig**ja kirjoita se muistiin **yhteyteen liittyvän DNS-liite**.
4. Älä sulje RDP-istunnon.  Sinun on sen myöhemmin, voit testata toimialuenimen selvitys.
5. Toista samat vaiheet kieliominaisuuksien **yhteyteen liittyvän DNS-liite** **Contoso-HBase-US**.


**Voit määrittää DNS-välittäjiä**
 
1.  RDP **Contoso-DNS-EU**kyselyjä. 
2.  Valitse vasemmassa alakulmassa Windows-näppäintä.
2.  Valitse **Valvontatyökalut**.
3.  Valitse **DNS**.
4.  Laajenna vasemmanpuoleisessa ruudussa **DSN**, **Contoso-DNS-EU**.
5.  **Ehdollinen välittäjiä**hiiren kakkospainikkeella ja valitse sitten **Uusi ehdollisen Forwarder**. 
5.  Anna seuraavat tiedot:
    - **DNS-toimialue**: Määritä DNS-liitteen Contoso-HBase-US. Esimerkki: Contoso-HBase-US.f5.internal.cloudapp.net.
    - **IP-osoitteet, master-palvelimet**: Kirjoita 10.2.0.4, joka on Contoso-DNS-USA käyttäjän IP-osoite. Varmista, että IP. DNS-palvelin voi olla eri IP-osoite.
6.  Paina **ENTER**-näppäintä ja valitse sitten **OK**.  Voit nyt voi ratkaista Contoso-DNS-EU Contoso-DNS-USA käyttäjän osoite.
7.  Toista vaiheet DNS ehdollinen forwarder lisääminen Contoso-DNS-US virtuaalikoneen seuraavin arvoin DNS-palvelu:
    - **DNS-toimialue**: Kirjoita Contoso-HBase-EU: n DNS-liitettä. 
    - **IP-osoitteet, master-palvelimet**: Kirjoita 10.2.0.4, joka on Contoso-DNS-EU: n IP-osoite.

**Voit testata toimialuenimen selvitys**

1. Siirtää Contoso-HBase-EU RDP-ikkunaan.
2. Avaa komentorivi-ikkuna.
3. Ping-komennon suorittaminen

        ping headnode0.[DNS suffix of Contoso-HBase-US]

    ICM-protokolla on otettu käyttöön HBase klustereiden työntekijän solmut

4. Älä sulje RDP-istunnon. Edelleen tarvitset sen myöhemmän version ominaisuudet opetusohjelman.
5. Toista samat vaiheet ping Contoso-HBase-EU Contoso-HBase-fi-headnode0.

>[AZURE.IMPORTANT] DNS on työskenneltävä, ennen kuin jatkat seuraavia ohjeita noudattamalla.

## <a name="enable-replication-between-hbase-tables"></a>Ota käyttöön replikoinnin HBase taulukoiden välille

Voit nyt luoda HBase esimerkkitaulukko, replikoinnin ottaminen käyttöön ja testaa se tietoja. Esimerkkitaulukko, jota käytät on kaksi saraketta perheille: oma ja Office. 

Tässä opetusohjelmassa tekee klusterin lähde ja kohde-klusterin kuin Yhdysvaltojen HBase-klusterin Europe HBase-klusterin.

Luo HBase taulukoiden samannimiset ja sarakkeen perheille, sekä lähde- ja klustereiden niin, että kohde-klusterin tietää tallennuspaikan se saa tiedot. Katso lisätietoja käyttämisestä HBase shell [HDInsight-HBase Apache käytön aloittaminen][hdinsight-hbase-get-started].

**Voit luoda HBase taulukon Contoso-HBase-EU**

1. Siirtää **Contoso-HBase-EU** RDP-ikkunaan.
2. Napsauta työpöydän **Hadoop komentoriviltä käsin**.
2. Siirry kansioon HBase pääkansion:

        cd %HBASE_HOME%\bin
3. Avaa HBase-liittymän:

        hbase shell
4. HBase-taulukon luominen:

        create 'Contacts', 'Personal', 'Office'
5. Älä sulje RDP-istunnon eikä Hadoop komentorivi-ikkuna. Edelleen tarvitset niitä myöhemmin-opetusohjelman.
    
**Voit luoda HBase taulukon Contoso-HBase-fi**

- Toista samat vaiheet saman taulukon luomisesta Contoso-HBase-US.


**Voit lisätä Contoso-HBase-US replikoinnin vertaisjärjestelmä**

1. Siirtää **Contso HBase_EU** RDP-ikkunaan.
2. HBase shell-ikkunasta Lisää kohde-klusterin (Contoso-HBase-fi) vertaisjärjestelmä, esimerkiksi:

        add_peer '1', 'zookeeper0.contoso-hbase-us.d4.internal.cloudapp.net,zookeeper1.contoso-hbase-us.d4.internal.cloudapp.net,zookeeper2.contoso-hbase-us.d4.internal.cloudapp.net:2181:/hbase'

    Toimialueen liite on otoksen, *contoso-hbase-us.d4.internal.cloudapp.net*. Sinun täytyy päivittää sen vastaamaan oman jälkiliitteen US HBase-klusterin. Varmista, että isäntänimet välillä ei ole välilyöntejä.

**Voit määrittää kunkin sarakkeen perheen replikoida lähde-klusterissa**

1. **Contso-HBase-EU** RDP-istunnon HBase shell-ikkunasta määrittää kunkin sarakkeen perheen replikoida:

        disable 'Contacts'
        alter 'Contacts', {NAME => 'Personal', REPLICATION_SCOPE => '1'}
        alter 'Contacts', {NAME => 'Office', REPLICATION_SCOPE => '1'}
        enable 'Contacts'

**Jos haluat Lataa HBase-taulukon tiedot joukkona**

Tietoja mallitiedosto on ladattu julkisen Azure-Blob-säilö, jossa on seuraava URL-osoite:

        wasbs://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt

Tiedoston sisällön:

        8396    Calvin Raji 230-555-0191    5415 San Gabriel Dr.
        16600   Karen Wu    646-555-0113    9265 La Paz
        4324    Karl Xie    508-555-0163    4912 La Vuelta
        16891   Jonathan Jackson    674-555-0110    40 Ellis St.
        3273    Miguel Miller   397-555-0155    6696 Anchor Drive
        3588    Osarumwense Agbonile    592-555-0152    1873 Lion Circle
        10272   Julia Lee   870-555-0110    3148 Rose Street
        4868    Jose Hayes  599-555-0171    793 Crawford Street
        4761    Caleb Alexander 670-555-0141    4775 Kentucky Dr.
        16443   Terry Chander   998-555-0171    771 Northridge Drive

Voit ladata saman tiedoston tuominen HBase-klusterin ja tuoda tiedot sieltä.

1. Siirtää **Contoso-HBase-EU** RDP-ikkunaan.
2. Napsauta työpöydän **Hadoop komentoriviltä käsin**.
3. Siirry kansioon HBase pääkansion:

        cd %HBASE_HOME%\bin

4. Lataa tiedot:

        hbase org.apache.hadoop.hbase.mapreduce.ImportTsv -Dimporttsv.columns="HBASE_ROW_KEY,Personal:Name, Personal:HomePhone, Office:Address" -Dimporttsv.bulk.output=/tmpOutput Contacts wasbs://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt

        hbase org.apache.hadoop.hbase.mapreduce.LoadIncrementalHFiles /tmpOutput Contacts


## <a name="verify-that-data-replication-is-taking-place"></a>Varmista, että tietojen replikointia on meneillään

Voit tarkistaa, että replikointi on meneillään skannaamalla taulukot sekä klustereiden ja HBase shell-komennot:

        Scan 'Contacts'


## <a name="next-steps"></a>Seuraavat vaiheet

Tässä opetusohjelmassa opit on yli kaksi palvelinkeskusten HBase replikoinnin määrittämisestä. Lisätietoja HDInsight ja HBase on seuraavissa artikkeleissa:

- [HDInsight-palvelun sivulle](https://azure.microsoft.com/services/hdinsight/)
- [HDInsight-asiakirjat](https://azure.microsoft.com/documentation/services/hdinsight/)
- [Apache HBase HDInsight-käytön aloittaminen][hdinsight-hbase-get-started]
- [HDInsight HBase yleiskatsaus][hdinsight-hbase-overview]
- [Valmistele HBase klustereiden Azure Virtual verkossa][hdinsight-hbase-provision-vnet]
- [Analysoi reaaliaikainen Twitter-markkinatunnelma HBase kanssa][hdinsight-hbase-twitter-sentiment]
- [Myrsky ja HDInsight (Hadoop) HBase tunnistimen tietojen analysoiminen][hdinsight-sensor-data]

[hdinsight-hbase-geo-replication-vnet]: hdinsight-hbase-geo-replication-configure-vnets.md
[hdinsight-hbase-geo-replication-dns]: ../hdinsight-hbase-geo-replication-configure-VNet.md


[img-vnet-diagram]: ./media/hdinsight-hbase-geo-replication/HDInsight.HBase.Replication.Network.diagram.png

[powershell-install]: powershell-install-configure.md
[hdinsight-hbase-get-started]: hdinsight-hbase-tutorial-get-started.md
[hdinsight-manage-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-hbase-replication-vnet]: hdinsight-hbase-geo-replication-configure-vnets.md
[hdinsight-hbase-replication-dns]: hdinsight-hbase-geo-replication-configure-dns.md
[hdinsight-hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md
[hdinsight-sensor-data]: hdinsight-storm-sensor-data-analysis.md
[hdinsight-hbase-overview]: hdinsight-hbase-overview.md
[hdinsight-hbase-provision-vnet]: hdinsight-hbase-provision-vnet.md
