<properties 
   pageTitle="Pääset alkuun käyttämällä PowerShellin Azure Azure tietojen järvi Analytics | Azure" 
   description="Opettele Luo järvi tietosäilö-tili, tietoja järvi Analytics työn U SQL Azure-PowerShellin avulla ja lähettää työn. " 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="edmacauley" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="09/21/2016"
   ms.author="edmaca"/>

# <a name="tutorial-get-started-with-azure-data-lake-analytics-using-azure-powershell"></a>Opetusohjelma: Azure PowerShellin Azure tietojen järvi Analytics käytön aloittaminen

[AZURE.INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

Opettele PowerShellin Azure avulla voit luoda Azure tietojen järvi Analytics-tilit ja määrittää tietojen järvi Analytics työt [U](data-lake-analytics-u-sql-get-started.md)SQL lähettää tietoja järvi analyyttisten tilit työt. Lisätietoja tietojen järvi Analytics artikkelissa [Azure tietojen järvi Analytics yleiskatsaus](data-lake-analytics-overview.md).

Tässä opetusohjelmassa kehität työn, joka lukee välilehti erotetut arvot (TSV)-tiedosto ja muuntaa sen Luetteloerottimella erotetut arvot (CSV)-tiedostoon. Siirry saman opetusohjelman käyttämällä muita tuetuilla työkaluilla kautta valitsemalla ylälaidassa tämän osion välilehtiä.

##<a name="prerequisites"></a>Edellytykset

Ennen kuin aloitat Tässä opetusohjelmassa, sinun on oltava seuraavasti:

- **Azure-tilaus**. Katso [Hae Azure maksuttoman kokeiluversion](https://azure.microsoft.com/pricing/free-trial/).
- **Työasema Azure PowerShellin avulla**. Katso, [miten voit asentaa ja määrittää PowerShellin Azure](../powershell-install-configure.md).
    
##<a name="create-data-lake-analytics-account"></a>Tietoja järvi Analytics-tilin luominen

Sinulla on oltava tietojen järvi Analytics-tili, ennen kuin voit suorittaa töitä. Tietoja järvi Analytics-tilin luominen, sinun on määritettävä seuraavasti:

- **Azure resurssiryhmä**: A tietojen järvi Analytics-tili on luotava Azure resurssiryhmä kuluessa. [Azure Resurssienhallinta](../azure-resource-manager/resource-group-overview.md) mahdollistaa sovelluksesi ryhmänä resurssien käsitteleminen. Voit ottaa käyttöön, Päivitä tai poista kaikki resurssit sovelluksen yhteen, koordinoidun toiminnossa.  

    Luettelointi tilauksen resurssiryhmät:
    
        Get-AzureRmResourceGroup
    
    Luo uusi resurssiryhmä seuraavasti:

        New-AzureRmResourceGroup `
            -Name "<Your resource group name>" `
            -Location "<Azure Data Center>" # For example, "East US 2"

- **Tietoja järvi Analytics tilin nimi**
- **Sijainti**: jokin Azure tietojen keskikohdan mukaan, joka tukee tietojen järvi Analytics.
- **Oletusarvoinen tietojen järvi tilin**: tietojen järvi Analytics-tileille on tietoja järvi oletustilin.

    Luo uusi tietojen järvi seuraavasti:

        New-AzureRmDataLakeStoreAccount `
            -ResourceGroupName "<Your Azure resource group name>" `
            -Name "<Your Data Lake account name>" `
            -Location "<Azure Data Center>"  # For example, "East US 2"

    > [AZURE.NOTE] Tietoja järvi tilin nimen on oltava vain pieniä kirjaimia ja numeroita.



**Tietoja järvi Analytics-tilin luominen**

1. Avaa PowerShell ise: Windows-työasemaan.
2. Suorita seuraava komentosarja:

        $resourceGroupName = "<ResourceGroupName>"
        $dataLakeStoreName = "<DataLakeAccountName>"
        $dataLakeAnalyticsName = "<DataLakeAnalyticsAccountName>"
        $location = "East US 2"
        
        Write-Host "Create a resource group ..." -ForegroundColor Green
        New-AzureRmResourceGroup `
            -Name  $resourceGroupName `
            -Location $location
        
        Write-Host "Create a Data Lake account ..."  -ForegroundColor Green
        New-AzureRmDataLakeStoreAccount `
            -ResourceGroupName $resourceGroupName `
            -Name $dataLakeStoreName `
            -Location $location 
        
        Write-Host "Create a Data Lake Analytics account ..."  -ForegroundColor Green
        New-AzureRmDataLakeAnalyticsAccount `
            -Name $dataLakeAnalyticsName `
            -ResourceGroupName $resourceGroupName `
            -Location $location `
            -DefaultDataLake $dataLakeStoreName
        
        Write-Host "The newly created Data Lake Analytics account ..."  -ForegroundColor Green
        Get-AzureRmDataLakeAnalyticsAccount `
            -ResourceGroupName $resourceGroupName `
            -Name $dataLakeAnalyticsName  

##<a name="upload-data-to-data-lake"></a>Tietojen lataaminen tietojen järvi

Tässä opetusohjelmassa käsittelee joitakin haun lokitiedot.  Etsi lokissa voidaan tallentaa tiedot järvi kaupan tai Azure-Blob-säiliö. 

Esimerkki haku lokitiedostoon on kopioitu julkisten Azure-Blob-säilö. Käytä seuraavaa PowerShell-komentosarjaa Lataa tiedosto työasemaan ja lataa tiedosto järvi tietovaraston oletustilin tietojen järvi Analytics-tilisi.

    $dataLakeStoreName = "<The default Data Lake Store account name>"
    
    $localFolder = "C:\Tutorials\Downloads\" # A temp location for the file. 
    $storageAccount = "adltutorials"  # Don't modify this value.
    $container = "adls-sample-data"  #Don't modify this value.

    # Create the temp location  
    New-Item -Path $localFolder -ItemType Directory -Force 

    # Download the sample file from Azure Blob storage
    $context = New-AzureStorageContext -StorageAccountName $storageAccount -Anonymous
    $blobs = Azure\Get-AzureStorageBlob -Container $container -Context $context
    $blobs | Get-AzureStorageBlobContent -Context $context -Destination $localFolder

    # Upload the file to the default Data Lake Store account    
    Import-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path $localFolder"SearchLog.tsv" -Destination "/Samples/Data/SearchLog.tsv"

Seuraavaa PowerShell-komentosarjaa näkyy järvi tietovaraston oletusnimen tietojen järvi Analytics-tilin hankkiminen:


    $resourceGroupName = "<ResourceGroupName>"
    $dataLakeAnalyticsName = "<DataLakeAnalyticsAccountName>"
    $dataLakeStoreName = (Get-AzureRmDataLakeAnalyticsAccount -ResourceGroupName $resourceGroupName -Name $dataLakeAnalyticsName).Properties.DefaultDataLakeAccount
    echo $dataLakeStoreName

>[AZURE.NOTE] Azure-portaalin tarjoaa käyttöliittymän mallidatatiedostot kopioiminen järvi tietovaraston oletustilin. Ohjeita on artikkelissa [Azure tietojen järvi Analytics Azure-portaalissa käytön aloittaminen](data-lake-analytics-get-started-portal.md#upload-data-to-the-default-data-lake-store-account).

Tietoja järvi Analytics käyttää Azure-Blob-säiliö.  Tietojen lataaminen Azure-Blob-säiliö, artikkelissa [Azure PowerShellin Azure-tallennustilan kanssa](../storage/storage-powershell-guide-full.md).

##<a name="submit-data-lake-analytics-jobs"></a>Lähettää tietoja järvi Analytics työt

Tietoja järvi Analytics työt kirjoitetaan U SQL-kielen. Lisätietoja U-SQL-kohdassa [U SQL-kielen käytön aloittaminen](data-lake-analytics-u-sql-get-started.md) - ja [U-SQL-kieliohje](http://go.microsoft.com/fwlink/?LinkId=691348).

**Tietoja järvi Analytics työn komentosarjan luominen**

- Luo tekstitiedosto seuraavat U-SQL-komentosarjan ja Tallenna tekstitiedosto työasemaan:

        @searchlog =
            EXTRACT UserId          int,
                    Start           DateTime,
                    Region          string,
                    Query           string,
                    Duration        int?,
                    Urls            string,
                    ClickedUrls     string
            FROM "/Samples/Data/SearchLog.tsv"
            USING Extractors.Tsv();
        
        OUTPUT @searchlog   
            TO "/Output/SearchLog-from-Data-Lake.csv"
        USING Outputters.Csv();

    Tämä U-SQL-komentosarja lukee käyttämällä **Extractors.Tsv()**lähdetiedosto tiedot ja luo csv-tiedoston **Outputters.Csv()**. 
    
    Älä muokkaa kaksi polkua, paitsi jos lähdetiedosto kopioiminen toiseen paikkaan.  Tietojen järvi Analytics Luo tulostus-kansioon, jos se ei ole.
    
    On helpompaa käyttämään suhteelliset polut oletusarvon tietojen järvi tilit tallennettuja tiedostoja. Voit käyttää myös suoria polkuja.  Esimerkki 
    
        adl://<Data LakeStorageAccountName>.azuredatalakestore.net:443/Samples/Data/SearchLog.tsv
        
    Sinun on käytettävä absoluuttiset polut linkitetyt tallennustilan asiakkaat tiedostoja.  Liitetyn Azure-tallennustilan tilin tallennettuja tiedostoja syntaksi on seuraava:
    
        wasb://<BlobContainerName>@<StorageAccountName>.blob.core.windows.net/Samples/Data/SearchLog.tsv

    >[AZURE.NOTE] Azure Blob-säilö julkisen BLOB tai julkinen säilöjen käyttöoikeudet eivät ole tuettuja.    
    
    
**Voit lähettää työ**

1. Avaa PowerShell ise: Windows-työasemaan.
2. Suorita seuraava komentosarja:

        $dataLakeAnalyticsName = "<DataLakeAnalyticsAccountName>"
        $usqlScript = "c:\tutorials\data-lake-analytics\copyFile.usql"
        
        $job = Submit-AzureRmDataLakeAnalyticsJob -Name "convertTSVtoCSV" -AccountName $dataLakeAnalyticsName –ScriptPath $usqlScript 

        Wait-AdlJob -Account $dataLakeAnalyticsName -JobId $job.JobId

        Get-AzureRmDataLakeAnalyticsJob -AccountName $dataLakeAnalyticsName -JobId $job.JobId

    Komentosarjan U-SQL-komentosarjatiedosto tallennetaan c:\tutorials\data-lake-analytics\copyFile.usql. Päivitä tiedostopolku vastaavasti.
 
Kun työ on valmis, voit käyttämällä seuraavat Cmdlet-komentoja tiedosto ja lataa tiedosto:
    
    $resourceGroupName = "<Resource Group Name>"
    $dataLakeAnalyticName = "<Data Lake Analytic Account Name>"
    $destFile = "C:\tutorials\data-lake-analytics\SearchLog-from-Data-Lake.csv"
    
    $dataLakeStoreName = (Get-AzureRmDataLakeAnalyticsAccount -ResourceGroupName $resourceGroupName -Name $dataLakeAnalyticName).Properties.DefaultDataLakeAccount
    
    Get-AzureRmDataLakeStoreChildItem -AccountName $dataLakeStoreName -path "/Output"
    
    Export-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path "/Output/SearchLog-from-Data-Lake.csv" -Destination $destFile

## <a name="see-also"></a>Katso myös

- Saat saman opetusohjelman muiden työkaluilla valitsemalla välilehden valitsimet-sivulla.
- Monimutkaisen kyselyn on ohjeartikkelissa [Analysoi sivuston lokit Azure tietojen järvi Analytics avulla](data-lake-analytics-analyze-weblogs.md).
- Aloita U SQL kehityssovellusten ohjeaiheessa [kehittää U-SQL-komentosarjojen käyttäminen tietojen järvi Tools for Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).
- U-SQL-kohdassa [Azure tietojen järvi Analytics U-SQL-kielen käytön aloittaminen](data-lake-analytics-u-sql-get-started.md).
- Katso hallintatehtäviä, [Hallita Azure tietojen järvi Analytics Azure-portaalissa](data-lake-analytics-manage-use-portal.md).
- Saat yleiskatsauksen tietojen järvi Analytics-artikkelissa [Azure tietojen järvi Analytics yleiskatsaus](data-lake-analytics-overview.md).

