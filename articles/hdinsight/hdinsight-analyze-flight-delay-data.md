<properties
    pageTitle="Hadoop HDInsight lento viive tietojen analysoiminen | Microsoft Azure"
    description="Opettele käyttämään HDInsight-klusterin luominen, rakenne suoritetaan, suorita Sqoop työn ja poista klusterin yhden Windows PowerShell-komentosarjaa."
    services="hdinsight"
    documentationCenter=""
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="jgao"/>

#<a name="analyze-flight-delay-data-by-using-hive-in-hdinsight"></a>Lentotietoihin viiveen analysoiminen käyttämällä HDInsight-rakenne

Rakenteen avulla töitä Hadoop MapReduce välityksellä SQL kaltaisessa komentosarjakielen kutsutaan * [HiveQL][hadoop-hiveql]*, joka voi suojata yhteenvetojen, kyselyt ja analysoiminen suurista tietomääristä.

> [AZURE.NOTE] Tässä asiakirjassa vaiheet pätevät Windows-pohjaisesta HDInsight-klusterin. Ohjeita, jotka toimivat Linux-klusteriin on kohdassa [Analysoi lento viive tiedot käyttämällä rakenne HDInsight (Linux)](hdinsight-analyze-flight-delay-data-linux.md).

Tietojen tallentaminen ja Laske erottaminen on yksi Azure Hdinsightiin merkittäviä etuja. HDInsight käyttää Azure-Blob-säiliö tietojen tallentamista varten. Normaali työ on kolme osaa:

1. **Tiedot tallennetaan Azure-Blob-säiliö**  SÄÄSUOJAUS esimerkiksi tietoja, tunnistimen tietojen, web-lokit ja tällöin viiveen lentotietoihin on tallennettu Azure-Blob-säiliö.
2. **Suorita työt.** Kun on aika käsitellä tietoja, suorita Windows PowerShell-komentosarjaa (tai asiakassovellus) voit luoda HDInsight-klusterin ja töiden poistaminen klusterin. Töiden Tallenna siirtää tietoja Azure-Blob-säiliö. Tiedot säilytetään klusterin poistamisen jälkeen. Tällä tavalla maksat vain mitä sinun on käytetty.
3. **Noutaa Azure-Blob-säiliö tulosteen**, tai tässä opetusohjelmassa Vie tiedot Azure SQL-tietokantaan.

Seuraavassa kaaviossa on kuvattu tässä opetusohjelmassa rakenne ja skenaariota:

![HDI. FlightDelays.flow][img-hdi-flightdelays-flow]

**Huomautus**: kaavion numerot vastaavat otsikot. **M** on lyhenne tärkeimmät prosessi. **A** lyhenne sisältö lisäyksen.

Opetusohjelman tärkeimmät osa näyttää, miten yhden Windows PowerShell-komentosarjaa avulla voit suorittaa seuraavia tehtäviä:

- Luo HDInsight-klusterin.
- Suorita rakenteen työn klusterin keskimääräisistä myöhästymisistä lentoasemien laskemiseen. Viive lentotietoihin tallennetaan Azure Blob storage tili.
- Suorita Sqoop työn rakenteen työn tulosteen vieminen Azure SQL-tietokantaan.
- Poista HDInsight-klusterin.

Liitteitä, valitse löytyvät ohjeita lataamisen Varausviive lentotietoihin, luodaan tai ladataan rakenteen kyselymerkkijonon ja valmisteleminen Sqoop työn Azure SQL-tietokantaan.

> [AZURE.NOTE] Tässä asiakirjassa vaiheet ovat Windows-pohjaisesta HDInsight klustereiden. Ohjeita, jotka toimivat Linux-klusteriin on kohdassa [Analysoi viive lentotietoihin käyttämällä HDInsight (Linux)-rakenne](hdinsight-analyze-flight-delay-data-linux.md)

###<a name="prerequisites"></a>Edellytykset

Ennen kuin aloitat Tässä opetusohjelmassa, sinulla on oltava seuraavat kohteet:

- **Azure-tilaus**. Katso [Hae Azure maksuttoman kokeiluversion](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- **Työasema Azure PowerShellin avulla**.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

**Tässä opetusohjelmassa käytetyt tiedostot**

Tässä opetusohjelmassa käyttää liittäminen lentoyhtiön lentotietoihin [tutkimus ja innovatiivista tekniikka hallinta, kuljetus tilastotiedot Bureau]tai RITA aikatauluissa suorituskyvyn [rita-website]. Kopioi tiedot on ladattu Azure-Blob-säilytykseen julkisen Blob-käyttöoikeudet. PowerShell-komentosarjaa osan kopioi tiedot julkisen blob-säilö yhteyttä klusterin oletusarvon blob-säilö. HiveQL komentosarja kopioidaan saman Blob-säilöön. Jos haluat tietoja get/Lataa tallennustilan-tilin tiedot, ja miten voit luoda ja lataa HiveQL-komentosarjatiedosto, katso [Liite A](#appendix-a) ja [B lisäys](#appendix-b).

Seuraavassa taulukossa on lueteltu tässä opetusohjelmassa käytetyt tiedostot:

<table border="1">
<tr><th>Tiedostot</th><th>Kuvaus</th></tr>
<tr><td>wasbs://flightdelay@hditutorialdata.blob.core.windows.net/flightdelays.hql</td><td>Rakenne-projektin käyttämän HiveQL komentosarjatiedosto. Tämä komentosarja on ladattu Azure Blob storage tilille, jolla yleisölle. <a href="#appendix-b">Lisäys B</a> on valmistelu ja tiedoston lataaminen Azure Blob storage tiliisi.</td></tr>
<tr><td>wasbs://flightdelay@hditutorialdata.blob.core.windows.net/2013Data</td><td>Rakenne-työ syöttötiedot. Tiedot on ladattu Azure Blob storage tilille, jolla yleisölle. <a href="#appendix-a">Lisäys A</a> on ohjeet tietojen hakeminen ja lataaminen oman Azure Blob storage tilin tiedot.</td></tr>
<tr><td>\tutorials\flightdelays\output</td><td>Rakenne-työ tulosteen polku. Oletussäilö käytetään tulosteen tietojen tallentamista varten.</td></tr>
<tr><td>\tutorials\flightdelays\jobstatus</td><td>Valitse Oletussäilö rakenteen projektin tila-kansio.</td></tr>
</table>


##<a name="create-cluster-and-run-hivesqoop-jobs"></a>Klusterin luominen ja suorittaminen rakenne ja Sqoop työt

Hadoop MapReduce on eräkäsittely. Useimmat kannattava suorittaa rakenteen työ on työn klusterin luominen ja poistaminen työn, kun työ on valmis. Seuraavaa komentosarjaa kattaa koko prosessia. Katso lisätietoja HDInsight-klusterin luominen ja rakenteen töitä [luominen Hadoop varausyksiköt HDInsight] [ hdinsight-provision] ja [Käytä rakenne ja HDInsight][hdinsight-use-hive].

**Suorittaa PowerShellin Azure rakenne-kyselyt**

1. Azure SQL-tietokantaan ja Sqoop työn tulostukseen-taulukon luominen ohjeita avulla [Lisäys C-näppäinyhdistelmää](#appendix-c).
3. Avaa Windows PowerShell ise: ja suorita seuraavaa komentosarjaa:

        $subscriptionID = "<Azure Subscription ID>"
        $nameToken = "<Enter an Alias>" 
        
        ###########################################
        # You must configure the follwing variables
        # for an existing Azure SQL Database
        ###########################################
        $existingSqlDatabaseServerName = "<Azure SQL Database Server>"
        $existingSqlDatabaseLogin = "<Azure SQL Database Server Login>"
        $existingSqlDatabasePassword = "<Azure SQL Database Server login password>"
        $existingSqlDatabaseName = "<Azure SQL Database name>"
        
        $localFolder = "E:\Tutorials\Downloads\" # A temp location for copying files. 
        $azcopyPath = "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy" # depends on the version, the folder can be different
        
        ###########################################
        # (Optional) configure the following variables
        ###########################################
        
        $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")

        $resourceGroupName = $namePrefix + "rg"
        $location = "EAST US 2"
        
        $HDInsightClusterName = $namePrefix + "hdi"
        $httpUserName = "admin"
        $httpPassword = "<Enter the Password>"
        
        $defaultStorageAccountName = $namePrefix + "store"
        $defaultBlobContainerName = $HDInsightClusterName # use the cluster name
        
        $existingSqlDatabaseTableName = "AvgDelays"
        $sqlDatabaseConnectionString = "jdbc:sqlserver://$existingSqlDatabaseServerName.database.windows.net;user=$existingSqlDatabaseLogin@$existingSqlDatabaseServerName;password=$existingSqlDatabaseLogin;database=$existingSqlDatabaseName"
        
        $hqlScriptFile = "/tutorials/flightdelays/flightdelays.hql" 
        
        $jobStatusFolder = "/tutorials/flightdelays/jobstatus"
        
        ###########################################
        # Login
        ###########################################
        try{
            $acct = Get-AzureRmSubscription
        }
        catch{
            Login-AzureRmAccount
        }
        Select-AzureRmSubscription -SubscriptionID $subscriptionID
        
        ###########################################
        # Create a new HDInsight cluster
        ###########################################
        
        # Create ARM group
        New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
        
        # Create the default storage account
        New-AzureRmStorageAccount -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName -Location $location -Type Standard_LRS
        
        # Create the default Blob container
        $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
        $defaultStorageAccountContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccountName -StorageAccountKey $defaultStorageAccountKey 
        New-AzureStorageContainer -Name $defaultBlobContainerName -Context $defaultStorageAccountContext 
        
        # Create the HDInsight cluster
        $pw = ConvertTo-SecureString -String $httpPassword -AsPlainText -Force
        $httpCredential = New-Object System.Management.Automation.PSCredential($httpUserName,$pw)
        
        New-AzureRmHDInsightCluster `
            -ResourceGroupName $resourceGroupName `
            -ClusterName $HDInsightClusterName `
            -Location $location `
            -ClusterType Hadoop `
            -OSType Windows `
            -ClusterSizeInNodes 2 `
            -HttpCredential $httpCredential `
            -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
            -DefaultStorageAccountKey $defaultStorageAccountKey `
            -DefaultStorageContainer $existingDefaultBlobContainerName 
        
        
        ###########################################
        # Prepare the HiveQL script and source data
        ###########################################
        
        # Create the temp location  
        New-Item -Path $localFolder -ItemType Directory -Force 
        
        # Download the sample file from Azure Blob storage
        $context = New-AzureStorageContext -StorageAccountName "hditutorialdata" -Anonymous
        $blobs = Get-AzureStorageBlob -Container "flightdelay" -Context $context
        #$blobs | Get-AzureStorageBlobContent -Context $context -Destination $localFolder
        
        # Upload data to default container
        
        $azcopycmd = "cmd.exe /C '$azcopyPath\azcopy.exe' /S /Source:'$localFolder' /Dest:'https://$defaultStorageAccountName.blob.core.windows.net/$defaultBlobContainerName/tutorials/flightdelays' /DestKey:$defaultStorageAccountKey"
        
        Invoke-Expression -Command:$azcopycmd
        
        ###########################################
        # Submit the Hive job
        ###########################################
        Use-AzureRmHDInsightCluster -ClusterName $HDInsightClusterName -HttpCredential $httpCredential
        $response = Invoke-AzureRmHDInsightHiveJob `
                        -Files $hqlScriptFile `
                        -DefaultContainer $defaultBlobContainerName `
                        -DefaultStorageAccountName $defaultStorageAccountName `
                        -DefaultStorageAccountKey $defaultStorageAccountKey `
                        -StatusFolder $jobStatusFolder 
        
        write-Host $response
        
        
        ###########################################
        # Submit the Sqoop job
        ###########################################
        $exportDir = "wasbs://$defaultBlobContainerName@$defaultStorageAccountName.blob.core.windows.net/tutorials/flightdelays/output"
        
        $sqoopDef = New-AzureRmHDInsightSqoopJobDefinition `
                        -Command "export --connect $sqlDatabaseConnectionString --table $sqlDatabaseTableName --export-dir $exportDir --fields-terminated-by \001 "
        $sqoopJob = Start-AzureRmHDInsightJob `
                        -ResourceGroupName $resourceGroupName `
                        -ClusterName $hdinsightClusterName `
                        -HttpCredential $httpCredential `
                        -JobDefinition $sqoopDef #-Debug -Verbose
        
        Wait-AzureRmHDInsightJob `
            -ResourceGroupName $resourceGroupName `
            -ClusterName $HDInsightClusterName `
            -HttpCredential $httpCredential `
            -WaitTimeoutInSeconds 3600 `
            -Job $sqoopJob.JobId
        
        
        Get-AzureRmHDInsightJobOutput `
            -ResourceGroupName $resourceGroupName `
            -ClusterName $hdinsightClusterName `
            -HttpCredential $httpCredential `
            -DefaultContainer $existingDefaultBlobContainerName `
            -DefaultStorageAccountName $defaultStorageAccountName `
            -DefaultStorageAccountKey $defaultStorageAccountKey `
            -JobId $sqoopJob.JobId `
            -DisplayOutputType StandardError
        
        ###########################################
        # Delete the cluster
        ###########################################
        Remove-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName

6. Yhteyden muodostaminen SQL-tietokantaan ja katso keskimääräinen lennot AvgDelays taulukon kaupungin perusteella:

    ![HDI. FlightDelays.AvgDelays.Dataset][image-hdi-flightdelays-avgdelays-dataset]



---
##<a id="appendix-a"></a>Liite A – Lataa lento viive tietojen Azure-Blob-säiliö
Datatiedosto ja (katso [Lisäys B](#appendix-b)) HiveQL komentosarjan tiedostojen lataaminen palvelimeen edellyttää suunnitella hyvin. Datatiedostot ja ennen HDInsight-klusterin luomalla ja suorittamalla rakenteen työn HiveQL-tiedoston tallentaminen on. Sinulla on kaksi vaihtoehtoa:

- **Käytä samalle Azuren tallennustilaan-tilille, jota käytetään käyttöjärjestelmän mukaan HDInsight-klusterin.** Koska HDInsight-klusterin on tallennustilan tilin pikanäppäin, ei tarvitse tehdä muita muutoksia.
- **Käytä Azure-tallennustilan tilin tiedostojärjestelmästä HDInsight klusterin oletusarvon.** Jos näin on, sinun on muokattava Windows PowerShell-komentosarjaa, [Luo HDInsight-klusterin ja suorita rakenne ja Sqoop työt](#runjob) linkitettävän tallennustilan tilin nimellä ylimääräisen tallennustilan tilin luominen-osa. Katso ohjeet [luominen Hadoop varausyksiköt HDInsight][hdinsight-provision]. Valitse HDInsight-klusterin tietää tallennustilan tilin pikanäppäin.

>[AZURE.NOTE] Datatiedoston Blob storage polku on vaikeaa koodattu komentosarjatiedosto HiveQL. Se on päivitettävä vastaavasti.

**Voit ladata lentotietoihin**

1. Siirry [oheis- ja tekniikalla hallinta-Bureau kuljetus tilastojen][rita-website].
2. Valitse-sivulla seuraavat arvot:

    <table border="1">
    <tr><th>Nimi</th><th>Arvo</th></tr>
    <tr><td>Suodatus vuoden</td><td>2013 </td></tr>
    <tr><td>Suodattaa piste</td><td>Tammikuussa</td></tr>
    <tr><td>Kentät</td><td>*Vuoden*, *FlightDate*, *UniqueCarrier*, *Carrier*, *FlightNum*, *OriginAirportID*, *Origin*, *OriginCityName*, *OriginState*, *DestAirportID*, *Dest*, *DestCityName*, *DestState*, *DepDelayMinutes*, *ArrDelay*, *ArrDelayMinutes*, *CarrierDelay*, *WeatherDelay*, *NASDelay*, *SecurityDelay*, *LateAircraftDelay* (Tyhjennä kaikki kentät)</td></tr>
    </table>

3. Valitse **Lataa**.
4. Pura tiedostosta **C:\Tutorials\FlightDelay\2013Data** -kansioon. Kukin tiedosto on CSV-tiedostoon ja noin 60 Gigatavun kokoisia.
5.  Anna tiedostolle nimi kuukauden sen sisältämät tiedot. Esimerkiksi tammikuussa tiedot sisältävän tiedoston nimeksi *January.csv*.
6. Toista vaiheet 2 ja 5 lataamaan tiedoston kunkin 2013 12 kuukautta. Tarvitset vähintään yhden tiedoston opetusohjelman.  

**Voit ladata viive lentotietoihin Azure-Blob-säiliö**

1. Valmistele parametrit:

    <table border="1">
    <tr><th>Muuttujan nimi</th><th>Huomautuksia</th></tr>
    <tr><td>$storageAccountName</td><td>Azure-tallennustilan tilin kohtaa, johon haluat ladata tiedot.</td></tr>
    <tr><td>$blobContainerName</td><td>Blob-säilö kohtaa, johon haluat ladata tiedot.</td></tr>
    </table>
2. Avaa Azure PowerShell ise:.
3. Seuraavat komentosarjan liittäminen komentosarjan-ruudussa:

        [CmdletBinding()]
        Param(

            [Parameter(Mandatory=$True,
                       HelpMessage="Enter the Azure storage account name for creating a new HDInsight cluster. If the account doesn't exist, the script will create one.")]
            [String]$storageAccountName,

            [Parameter(Mandatory=$True,
                       HelpMessage="Enter the Azure blob container name for creating a new HDInsight cluster. If not specified, the HDInsight cluster name will be used.")]
            [String]$blobContainerName
        )

        #Region - Variables
        $localFolder = "C:\Tutorials\FlightDelay\2013Data"  # The source folder
        $destFolder = "tutorials/flightdelay/2013data"     #The blob name prefix for the files to be uploaded
        #EndRegion
        
        #Region - Connect to Azure subscription
        Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
        try{Get-AzureRmContext}
        catch{Login-AzureRmAccount}
        #EndRegion
        
        #Region - Validate user input
        Write-Host "`nValidating the Azure Storage account and the Blob container..." -ForegroundColor Green
        # Validate the Storage account
        if (-not (Get-AzureRmStorageAccount|Where-Object{$_.StorageAccountName -eq $storageAccountName}))
        {
            Write-Host "The storage account, $storageAccountName, doesn't exist." -ForegroundColor Red
            exit
        }
        else{
            $resourceGroupName = (Get-AzureRmStorageAccount|Where-Object{$_.StorageAccountName -eq $storageAccountName}).ResourceGroupName
        }
        
        # Validate the container
        $storageAccountKey = (Get-AzureRmStorageAccountKey -StorageAccountName $storageAccountName -ResourceGroupName $resourceGroupName)[0].Value
        $storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
        
        if (-not (Get-AzureStorageContainer -Context $storageContext |Where-Object{$_.Name -eq $blobContainerName}))
        {
            Write-Host "The Blob container, $blobContainerName, doesn't exist" -ForegroundColor Red
            Exit
        }
        #EngRegion
        
        #Region - Copy the file from local workstation to Azure Blob storage  
        if (test-path -Path $localFolder)
        {
            foreach ($item in Get-ChildItem -Path $localFolder){
                $fileName = "$localFolder\$item"
                $blobName = "$destFolder/$item"
        
                Write-Host "Copying $fileName to $blobName" -ForegroundColor Green
        
                Set-AzureStorageBlobContent -File $fileName -Container $blobContainerName -Blob $blobName -Context $storageContext
            }
        }
        else
        {
            Write-Host "The source folder on the workstation doesn't exist" -ForegroundColor Red
        }
        
        # List the uploaded files on HDInsight
        Get-AzureStorageBlob -Container $blobContainerName  -Context $storageContext -Prefix $destFolder
        #EndRegion




4. Painamalla **F5** komentosarjan suorittaminen.

Jos haluat käyttää toista menetelmää tiedostojen lataamisesta, varmista, että tiedoston koko on opetusohjelmat/flightdelay/tiedot. Tiedostojen avaaminen syntaksi on seuraava:

    wasbs://<ContainerName>@<StorageAccountName>.blob.core.windows.net/tutorials/flightdelay/data

Polun opetusohjelmat/flightdelay/tiedot ovat loit silloin, kun olet ladannut tiedostot näennäiskansio. Varmista, että on yksi vuoden jokaiselle kuukaudelle 12 tiedostot.

>[AZURE.NOTE] Sinun on päivitettävä rakenteen kyselyn lukea uuteen sijaintiin.

> Sinun on määritettävä joko julkisen tai sitoa tallennustilan tilin HDInsight-klusterin säilö-käyttöoikeus. Muussa tapauksessa rakenteen kyselymerkkijonon eivät pysty käyttämään datatiedostot.

---
##<a id="appendix-b"></a>Lisäys B – luo ja lataa HiveQL-komentosarja

Azure PowerShellin avulla voit suorittaa useita HiveQL lauseet yksi kerrallaan tai pakata HiveQL-lauseen komentosarja-tiedostoon. Tässä osassa näytetään, miten voit luoda HiveQL komentosarjan ja Lataa komentosarja Azure-Blob-säiliö Azure PowerShell-toiminnolla. Rakenne edellyttää tallennetaan Azure-Blob-säiliö HiveQL-komentosarjoja.

HiveQL komentosarja suorittaa seuraavasti:

1. **Pudota delays_raw taulukon**, tapauksessa taulukossa jo olemassa.
2. **Luo delays_raw ulkoisen rakennetaulukko** osoittamalla Blob-tallennuspaikka lento viive-tiedostoja. Tämä kysely määrittää, että kentät on erotettu toisistaan "," ja että rivien lopetetaan "\n". Tämä aiheuttaa virheen, kun kenttäarvojen sisältää pilkkuja, koska rakennetta ei voida erottaa pilkku, joka on kenttäerotin ja yksi, joka on osa kentän arvon (joka on peräisin kenttäarvojen\_Kaupunki\_nimi ja DEST\_Kaupunki\_nimi). Osoitteen tämä kysely luo TEMP sarakkeet sisältää tietoja, jotka on väärin jakamista sarakkeisiin.  
3. **Pudota viiveitä taulukon**, tapauksessa taulukossa jo olemassa.
4. **Luo viiveitä taulukon**. Kannattaa lisätä Puhdista tiedot ennen lisäkäsittelyä. Tämä kysely luo uuden taulukon, *viiveitä*delays_raw-taulukosta. Huomautus TEMP-sarakkeet (kuten edellä mainittiin) ei kopioida ja että **alimerkkijono** -funktiota käytetään lainausmerkkejä poistaminen tiedot.
5. **Laske keskiarvo sää viive ja ryhmien tulokset postitoimipaikka.** Se myös siirtää Blob-objektien tallennustilaan tulokset. Huomaa, että kysely poistaa heittomerkit tiedot ja sulkee pois rivejä, jossa **weather_delay** arvo on null. Tämä on tarpeen, koska Sqoop, käytetään jäljempänä tässä opetusohjelmassa ei käsittele kyseiset arvot tilanteen oletusarvoisesti.

Katso täysi luettelo HiveQL komentoja [Rakenne Data Definition Language][hadoop-hiveql]. Kunkin HiveQL-komento on lopetettava puolipisteellä.

**Voit luoda HiveQL-komentosarjatiedosto**

1. Valmistele parametrit:

    <table border="1">
    <tr><th>Muuttujan nimi</th><th>Huomautuksia</th></tr>
    <tr><td>$storageAccountName</td><td>Azure-tallennustilan tilin kohtaa, johon haluat ladata HiveQL komentosarja.</td></tr>
    <tr><td>$blobContainerName</td><td>Kohtaa, johon haluat ladata HiveQL komentosarja Blob-säilö.</td></tr>
    </table>
2. Avaa Azure PowerShell ise:.

3. Kopioi ja liitä seuraava komentosarja script-ruudussa:

        [CmdletBinding()]
        Param(

            # Azure Blob storage variables
            [Parameter(Mandatory=$True,
                       HelpMessage="Enter the Azure storage account name for creating a new HDInsight cluster. If the account doesn't exist, the script will create one.")]
            [String]$storageAccountName,

            [Parameter(Mandatory=$True,
                       HelpMessage="Enter the Azure blob container name for creating a new HDInsight cluster. If not specified, the HDInsight cluster name will be used.")]
            [String]$blobContainerName
        )

        #region - Define variables
        # Treat all errors as terminating
        $ErrorActionPreference = "Stop"
        
        # The HiveQL script file is exported as this file before it's uploaded to Blob storage
        $hqlLocalFileName = "e:\tutorials\flightdelay\flightdelays.hql"
        
        # The HiveQL script file will be uploaded to Blob storage as this blob name
        $hqlBlobName = "tutorials/flightdelay/flightdelays.hql"
        
        # These two constants are used by the HiveQL script file
        #$srcDataFolder = "tutorials/flightdelay/data"
        $dstDataFolder = "/tutorials/flightdelay/output"
        #endregion

        #Region - Connect to Azure subscription
        Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
        try{Get-AzureRmContext}
        catch{Login-AzureRmAccount}
        #EndRegion
        
        #Region - Validate user input
        Write-Host "`nValidating the Azure Storage account and the Blob container..." -ForegroundColor Green
        # Validate the Storage account
        if (-not (Get-AzureRmStorageAccount|Where-Object{$_.StorageAccountName -eq $storageAccountName}))
        {
            Write-Host "The storage account, $storageAccountName, doesn't exist." -ForegroundColor Red
            exit
        }
        else{
            $resourceGroupName = (Get-AzureRmStorageAccount|Where-Object{$_.StorageAccountName -eq $storageAccountName}).ResourceGroupName
        }
        
        # Validate the container
        $storageAccountKey = (Get-AzureRmStorageAccountKey -StorageAccountName $storageAccountName -ResourceGroupName $resourceGroupName)[0].Value
        $storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
        
        if (-not (Get-AzureStorageContainer -Context $storageContext |Where-Object{$_.Name -eq $blobContainerName}))
        {
            Write-Host "The Blob container, $blobContainerName, doesn't exist" -ForegroundColor Red
            Exit
        }
        #EngRegion
        
        #region - Validate the file and file path
        
        # Check if a file with the same file name already exists on the workstation
        Write-Host "`nvalidating the folder structure on the workstation for saving the HQL script file ..."  -ForegroundColor Green
        if (test-path $hqlLocalFileName){
        
            $isDelete = Read-Host 'The file, ' $hqlLocalFileName ', exists.  Do you want to overwirte it? (Y/N)'
        
            if ($isDelete.ToLower() -ne "y")
            {
                Exit
            }
        }
        
        # Create the folder if it doesn't exist
        $folder = split-path $hqlLocalFileName
        if (-not (test-path $folder))
        {
            Write-Host "`nCreating folder, $folder ..." -ForegroundColor Green
        
            new-item $folder -ItemType directory  
        }
        #end region
        
        #region - Write the Hive script into a local file
        Write-Host "`nWriting the Hive script into a file on your workstation ..." `
                    -ForegroundColor Green
        
        $hqlDropDelaysRaw = "DROP TABLE delays_raw;"
        
        $hqlCreateDelaysRaw = "CREATE EXTERNAL TABLE delays_raw (" +
                "YEAR string, " +
                "FL_DATE string, " +
                "UNIQUE_CARRIER string, " +
                "CARRIER string, " +
                "FL_NUM string, " +
                "ORIGIN_AIRPORT_ID string, " +
                "ORIGIN string, " +
                "ORIGIN_CITY_NAME string, " +
                "ORIGIN_CITY_NAME_TEMP string, " +
                "ORIGIN_STATE_ABR string, " +
                "DEST_AIRPORT_ID string, " +
                "DEST string, " +
                "DEST_CITY_NAME string, " +
                "DEST_CITY_NAME_TEMP string, " +
                "DEST_STATE_ABR string, " +
                "DEP_DELAY_NEW float, " +
                "ARR_DELAY_NEW float, " +
                "CARRIER_DELAY float, " +
                "WEATHER_DELAY float, " +
                "NAS_DELAY float, " +
                "SECURITY_DELAY float, " +
                "LATE_AIRCRAFT_DELAY float) " +
            "ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' " +
            "LINES TERMINATED BY '\n' " +
            "STORED AS TEXTFILE " +
            "LOCATION 'wasbs://flightdelay@hditutorialdata.blob.core.windows.net/2013Data';"
        
        $hqlDropDelays = "DROP TABLE delays;"
        
        $hqlCreateDelays = "CREATE TABLE delays AS " +
            "SELECT YEAR AS year, " +
                "FL_DATE AS flight_date, " +
                "substring(UNIQUE_CARRIER, 2, length(UNIQUE_CARRIER) -1) AS unique_carrier, " +
                "substring(CARRIER, 2, length(CARRIER) -1) AS carrier, " +
                "substring(FL_NUM, 2, length(FL_NUM) -1) AS flight_num, " +
                "ORIGIN_AIRPORT_ID AS origin_airport_id, " +
                "substring(ORIGIN, 2, length(ORIGIN) -1) AS origin_airport_code, " +
                "substring(ORIGIN_CITY_NAME, 2) AS origin_city_name, " +
                "substring(ORIGIN_STATE_ABR, 2, length(ORIGIN_STATE_ABR) -1)  AS origin_state_abr, " +
                "DEST_AIRPORT_ID AS dest_airport_id, " +
                "substring(DEST, 2, length(DEST) -1) AS dest_airport_code, " +
                "substring(DEST_CITY_NAME,2) AS dest_city_name, " +
                "substring(DEST_STATE_ABR, 2, length(DEST_STATE_ABR) -1) AS dest_state_abr, " +
                "DEP_DELAY_NEW AS dep_delay_new, " +
                "ARR_DELAY_NEW AS arr_delay_new, " +
                "CARRIER_DELAY AS carrier_delay, " +
                "WEATHER_DELAY AS weather_delay, " +
                "NAS_DELAY AS nas_delay, " +
                "SECURITY_DELAY AS security_delay, " +
                "LATE_AIRCRAFT_DELAY AS late_aircraft_delay " +
            "FROM delays_raw;"
        
        $hqlInsertLocal = "INSERT OVERWRITE DIRECTORY '$dstDataFolder' " +
            "SELECT regexp_replace(origin_city_name, '''', ''), " +
                "avg(weather_delay) " +
            "FROM delays " +
            "WHERE weather_delay IS NOT NULL " +
            "GROUP BY origin_city_name;"
        
        $hqlScript = $hqlDropDelaysRaw + $hqlCreateDelaysRaw + $hqlDropDelays + $hqlCreateDelays + $hqlInsertLocal
        
        $hqlScript | Out-File $hqlLocalFileName -Encoding ascii -Force
        #endregion
        
        #region - Upload the Hive script to the default Blob container
        Write-Host "`nUploading the Hive script to the default Blob container ..." -ForegroundColor Green
        
        # Create a storage context object
        $storageAccountKey = (Get-AzureRmStorageAccountKey -StorageAccountName $storageAccountName -ResourceGroupName $resourceGroupName)[0].Value
        $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
        
        # Upload the file from local workstation to Blob storage
        Set-AzureStorageBlobContent -File $hqlLocalFileName -Container $blobContainerName -Blob $hqlBlobName -Context $destContext
        #endregion
        
        Write-host "`nEnd of the PowerShell script" -ForegroundColor Green

    Seuraavassa on käytetty komentosarja muuttujat:

    - **$hqlLocalFileName** - komentosarja tallentaa HiveQL-komentosarjatiedosto paikallisesti ennen lataaminen Blob-objektien tallennustilaan. Tämä on tiedostonimi. Oletusarvo on <u>C:\tutorials\flightdelay\flightdelays.hql</u>.
    - **$hqlBlobName** - tämä on HiveQL komentosarjan blob tiedostonimen Azure-Blob-objektien tallennustilaan. Oletusarvo on tutorials/flightdelay/flightdelays.hql. Koska tiedoston kirjoitetaan suoraan Azure-Blob-säiliö, ei ole "/" blob-nimen alussa. Jos haluat käyttää tiedostoa Blob-objektien tallennustilaan, tarvitset lisää "/" tiedostonimen alussa.
    - **$srcDataFolder** ja **$dstDataFolder** - = "opetusohjelmat-flightdelay-tiedot" = "opetusohjelmat-flightdelay-tulostus"


---
##<a id="appendix-c"></a>Lisäys C - valmisteleminen Azure SQL-tietokantaan Sqoop työn tulos
**Valmistele SQL-tietokanta (Yhdistä tämä Sqoop komentosarja)**

1. Valmistele parametrit:

    <table border="1">
    <tr><th>Muuttujan nimi</th><th>Huomautuksia</th></tr>
    <tr><td>$sqlDatabaseServerName</td><td>Azure SQL-tietokantapalvelimen nimi. Kirjoita mitään voit luoda uuden palvelimen.</td></tr>
    <tr><td>$sqlDatabaseUsername</td><td>Azure SQL-tietokantapalvelimen käyttäjätunnus. Jos $sqlDatabaseServerName on olemassa olevan palvelimen, kirjaudu sisään ja kirjautumissalasana käytetään palvelimen todennustavat. Muussa tapauksessa niitä käytetään luomaan uuden palvelimen.</td></tr>
    <tr><td>$sqlDatabasePassword</td><td>Azure SQL-tietokantapalvelimen kirjautumissalasana.</td></tr>
    <tr><td>$sqlDatabaseLocation</td><td>Tätä arvoa käytetään vain silloin, kun luot uuden Azure tietokantapalvelimeen.</td></tr>
    <tr><td>$sqlDatabaseName</td><td>SQL-tietokanta on käytettävä Sqoop työn AvgDelays-taulukon luominen. Jätä se tyhjäksi Luo kutsutaan HDISqoop tietokanta. Taulukkonimi Sqoop työn tulos on AvgDelays. </td></tr>
    </table>
2. Avaa Azure PowerShell ise:.
3. Kopioi ja liitä seuraava komentosarja script-ruudussa:

        [CmdletBinding()]
        Param(
        
            # Azure Resource group variables
            [Parameter(Mandatory=$True,
                    HelpMessage="Enter the Azure resource group name. It will be created if it doesn't exist.")]
            [String]$resourceGroupName,
        
            # SQL database server variables
            [Parameter(Mandatory=$True,
                    HelpMessage="Enter the Azure SQL Database Server Name. It will be created if it doesn't exist.")]
            [String]$sqlDatabaseServer, 
        
            [Parameter(Mandatory=$True,
                    HelpMessage="Enter the Azure SQL Database admin user.")]
            [String]$sqlDatabaseLogin,
        
            [Parameter(Mandatory=$True,
                    HelpMessage="Enter the Azure SQL Database admin user password.")]
            [String]$sqlDatabasePassword,
        
            [Parameter(Mandatory=$True,
                    HelpMessage="Enter the region to create the Database in.")]
            [String]$sqlDatabaseLocation,   #For example, West US.
        
            # SQL database variables
            [Parameter(Mandatory=$True,
                    HelpMessage="Enter the database name. It will be created if it doesn't exist.")]
            [String]$sqlDatabaseName # specify the database name if you have one created. Otherwise use "" to have the script create one for you.
        )
        
        # Treat all errors as terminating
        $ErrorActionPreference = "Stop"
        
        #region - Constants and variables
        
        # IP address REST service used for retrieving external IP address and creating firewall rules
        [String]$ipAddressRestService = "http://bot.whatismyipaddress.com"
        [String]$fireWallRuleName = "FlightDelay"
        
        # SQL database variables
        [String]$sqlDatabaseMaxSizeGB = 10
        
        #SQL query string for creating AvgDelays table
        [String]$sqlDatabaseTableName = "AvgDelays"
        [String]$sqlCreateAvgDelaysTable = " CREATE TABLE [dbo].[$sqlDatabaseTableName](
                    [origin_city_name] [nvarchar](50) NOT NULL,
                    [weather_delay] float,
                CONSTRAINT [PK_$sqlDatabaseTableName] PRIMARY KEY CLUSTERED
                (
                    [origin_city_name] ASC
                )
                )"
        #endregion
        
        #Region - Connect to Azure subscription
        Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
        try{Get-AzureRmContext}
        catch{Login-AzureRmAccount}
        #EndRegion
        
        #region - Create and validate Azure resouce group
        try{
            Get-AzureRmResourceGroup -Name $resourceGroupName
        }
        catch{
            New-AzureRmResourceGroup -Name $resourceGroupName -Location $sqlDatabaseLocation
        }
        
        #EndRegion
        
        #region - Create and validate Azure SQL database server
        try{
            Get-AzureRmSqlServer -ServerName $sqlDatabaseServer -ResourceGroupName $resourceGroupName}
        catch{
            Write-Host "`nCreating SQL Database server ..."  -ForegroundColor Green
        
            $sqlDatabasePW = ConvertTo-SecureString -String $sqlDatabasePassword -AsPlainText -Force
            $credential = New-Object System.Management.Automation.PSCredential($sqlDatabaseLogin,$sqlDatabasePW)
        
            $sqlDatabaseServer = (New-AzureRmSqlServer -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -SqlAdministratorCredentials $credential -Location $sqlDatabaseLocation).ServerName
            Write-Host "`tThe new SQL database server name is $sqlDatabaseServer." -ForegroundColor Cyan
        
            Write-Host "`nCreating firewall rule, $fireWallRuleName ..." -ForegroundColor Green
            $workstationIPAddress = Invoke-RestMethod $ipAddressRestService
            New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -FirewallRuleName "$fireWallRuleName-workstation" -StartIpAddress $workstationIPAddress -EndIpAddress $workstationIPAddress
        
            #To allow other Azure services to access the server add a firewall rule and set both the StartIpAddress and EndIpAddress to 0.0.0.0. Note that this allows Azure traffic from any Azure subscription to access the server.
            New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -FirewallRuleName "$fireWallRuleName-Azureservices" -StartIpAddress "0.0.0.0" -EndIpAddress "0.0.0.0"
        }
        
        #endregion
        
        #region - Create and validate Azure SQL database
        
        try {
            Get-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -DatabaseName $sqlDatabaseName
        }
        catch {
            Write-Host "`nCreating SQL Database, $sqlDatabaseName ..."  -ForegroundColor Green
            New-AzureRMSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -DatabaseName $sqlDatabaseName -Edition "Standard" -RequestedServiceObjectiveName "S1"
        }
        
        #endregion
        
        #region -  Execute an SQL command to create the AvgDelays table
        
        Write-Host "`nCreating SQL Database table ..."  -ForegroundColor Green
        $conn = New-Object System.Data.SqlClient.SqlConnection
        $conn.ConnectionString = "Data Source=$sqlDatabaseServer.database.windows.net;Initial Catalog=$sqlDatabaseName;User ID=$sqlDatabaseLogin;Password=$sqlDatabasePassword;Encrypt=true;Trusted_Connection=false;"
        $conn.open()
        $cmd = New-Object System.Data.SqlClient.SqlCommand
        $cmd.connection = $conn
        $cmd.commandtext = $sqlCreateAvgDelaysTable
        $cmd.executenonquery()
        
        $conn.close()
        
        Write-host "`nEnd of the PowerShell script" -ForegroundColor Green

    >[AZURE.NOTE] Komentosarja käyttää representational tilan siirto (REST) palvelun, http://bot.whatismyipaddress.com, voit hakea ulkoisia IP-osoite. IP-osoitetta käytetään SQL-tietokantaan palomuurisäännön luominen.  

    Seuraavassa on joitakin muuttujaa käytetään komentosarja:

    - **$ipAddressRestService** - oletusarvo on http://bot.whatismyipaddress.com. Julkinen IP-osoite REST-palvelun tehostavia ulkoinen IP-osoite on. Voit käyttää muita palveluita, voit halutessasi. Ulkoisen IP-osoitteen noutaa palvelun kautta käytetään Azure SQL-tietokantapalvelimen palomuurisäännön luominen niin, että voit käyttää tietokannan työaseman (käyttämällä Windows PowerShell-komentosarjaa).
    - **$fireWallRuleName** – tämä on Azure SQL-tietokantaan palomuurin säännön nimi. Oletusnimi on <u>FlightDelay</u>. Voit muuttaa sitä, jos haluat.
    - **$sqlDatabaseMaxSizeGB** - tätä arvoa käytetään vain silloin, kun luot uuden Azure SQL-tietokantaan. Oletusarvo on 10 Gigatavua. 10 Gigatavua riittää Tässä opetusohjelmassa.
    - **$sqlDatabaseName** - tätä arvoa käytetään vain silloin, kun luot uuden Azure SQL-tietokantaan. Oletusarvo on HDISqoop. Jos nimeät, sinun on päivitettävä Sqoop Windows PowerShell-komentosarjaa vastaavasti.

4. Painamalla **F5** komentosarjan suorittaminen.
5. Vahvista komentosarjan tulos. Varmista, että komentosarja suoritettiin onnistuneesti.

##<a id="nextsteps"></a>Seuraavat vaiheet
Nyt ymmärrät tiedoston lataaminen Azure-Blob-säiliö, miten täytä rakenteen taulukon avulla tiedot Azure-Blob-säiliö, rakenne kyselyjen suorittaminen ja tietojen vieminen Azure SQL-tietokantaan HDFS Sqoop avulla. Lisätietoja on seuraavissa artikkeleissa:

* [HDInsight käytön aloittaminen][hdinsight-get-started]
* [Rakenteen käyttäminen Hdinsightiin][hdinsight-use-hive]
* [Oozie käyttäminen Hdinsightiin][hdinsight-use-oozie]
* [Sqoop käyttäminen Hdinsightiin][hdinsight-use-sqoop]
* [Possu käyttäminen Hdinsightiin][hdinsight-use-pig]
* [Kehitä Java MapReduce ohjelmien Hdinsightiin][hdinsight-develop-mapreduce]



[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/


[rita-website]: http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time
[powershell-install-configure]: powershell-install-configure.md

[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-develop-mapreduce]: hdinsight-develop-deploy-java-mapreduce-linux.md

[hadoop-hiveql]: https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL
[hadoop-shell-commands]: http://hadoop.apache.org/docs/r0.18.3/hdfs_shell.html

[technetwiki-hive-error]: http://social.technet.microsoft.com/wiki/contents/articles/23047.hdinsight-hive-error-unable-to-rename.aspx

[image-hdi-flightdelays-avgdelays-dataset]: ./media/hdinsight-analyze-flight-delay-data/HDI.FlightDelays.AvgDelays.DataSet.png
[img-hdi-flightdelays-run-hive-job-output]: ./media/hdinsight-analyze-flight-delay-data/HDI.FlightDelays.RunHiveJob.Output.png
[img-hdi-flightdelays-flow]: ./media/hdinsight-analyze-flight-delay-data/HDI.FlightDelays.Flow.png
