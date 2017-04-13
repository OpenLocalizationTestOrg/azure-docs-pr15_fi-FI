<properties 
   pageTitle="Hallitse Azure PowerShellin Azure tietojen järvi Analytics | Azure" 
   description="Opi hallitsemaan tietoja järvi Analytics työt, tietolähteiden, käyttäjät. " 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="edmacauley" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="05/16/2016"
   ms.author="edmaca"/>

# <a name="manage-azure-data-lake-analytics-using-azure-powershell"></a>Hallitse Azure tietojen järvi Analytics Azure PowerShellin avulla

[AZURE.INCLUDE [manage-selector](../../includes/data-lake-analytics-selector-manage.md)]

Opi hallitsemaan Azure tietojen järvi Analytics-tilit, tietolähteitä, käyttäjät ja työt Azure-PowerShellin avulla. Hallinta aiheita avulla muut työkalut näkyviin napsauttamalla yllä välilehden Valitse.

**Edellytykset**

Ennen kuin aloitat Tässä opetusohjelmassa, sinun on oltava seuraavasti:

- **Azure-tilaus**. Katso [Hae Azure maksuttoman kokeiluversion](https://azure.microsoft.com/pricing/free-trial/).


<!-- ################################ -->
<!-- ################################ -->


##<a name="install-azure-powershell-10-or-greater"></a>Asenna Azure PowerShell 1.0 tai uudempi

Valmistelevat osiosta, [Azure PowerShellin Azure resurssien hallinta](powershell-azure-resource-manager.md#prerequisites).
    
## <a name="manage-accounts"></a>Tilien hallinta

Ennen kuin suoritat tietojen järvi Analytics töitä, sinulla on oltava tietojen järvi Analytics-tili. Toisin kuin Azure Hdinsightiin ei maksat Analytics-tiliä, kun se ei ole käynnissä työn.  Voit maksaa vain kerran, kun se suoritetaan työn.  Lisätietoja on artikkelissa [Azure järvi Analytics yleiskatsaus](data-lake-analytics-overview.md).  

###<a name="create-accounts"></a>Tilien luominen

    $resourceGroupName = "<ResourceGroupName>"
    $dataLakeStoreName = "<DataLakeAccountName>"
    $dataLakeAnalyticsAccountName = "<DataLakeAnalyticsAccountName>"
    $location = "<Microsoft Data Center>"
    
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
        -Name $dataLakeAnalyticsAccountName `
        -ResourceGroupName $resourceGroupName `
        -Location $location `
        -DefaultDataLake $dataLakeStoreName
    
    Write-Host "The newly created Data Lake Analytics account ..."  -ForegroundColor Green
    Get-AzureRmDataLakeAnalyticsAccount `
        -ResourceGroupName $resourceGroupName `
        -Name $dataLakeAnalyticsAccountName  

Voit käyttää myös Azure resurssiryhmä-malli. Mallin luominen tietojen järvi Analytics-tili ja riippuvainen järvi tietosäilö on [Liite A](#appendix-a). Tallenna malli tiedostoon .json mallin avulla ja anna sille seuraavaa PowerShell-komentosarjaa avulla:


    $AzureSubscriptionID = "<Your Azure Subscription ID>"
    
    $ResourceGroupName = "<New Azure Resource Group Name>"
    $Location = "EAST US 2"
    $DefaultDataLakeStoreAccountName = "<New Data Lake Store Account Name>"
    $DataLakeAnalyticsAccountName = "<New Data Lake Analytics Account Name>"
    
    $DeploymentName = "MyDataLakeAnalyticsDeployment"
    $ARMTemplateFile = "E:\Tutorials\ADL\ARMTemplate\azuredeploy.json"  # update the Json template path 
    
    Login-AzureRmAccount
    
    Select-AzureRmSubscription -SubscriptionId $AzureSubscriptionID
    
    # Create the resource group
    New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Location
    
    # Create the Data Lake Analytics account with the default Data Lake Store account.
    $parameters = @{"adlAnalyticsName"=$DataLakeAnalyticsAccountName; "adlStoreName"=$DefaultDataLakeStoreAccountName}
    New-AzureRmResourceGroupDeployment -Name $DeploymentName -ResourceGroupName $ResourceGroupName -TemplateFile $ARMTemplateFile -TemplateParameterObject $parameters 

 
###<a name="list-account"></a>Luettelo-tili

Luettelon tietojen järvi Analytics tilit voimassa oleva tilaus

    Get-AzureRmDataLakeAnalyticsAccount
    
Tulos:

    Id         : /subscriptions/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/resourceGroups/learn1021rg/providers/Microsoft.DataLakeAnalytics/accounts/learn1021adla
    Location   : eastus2
    Name       : learn1021adla
    Properties : Microsoft.Azure.Management.DataLake.Analytics.Models.DataLakeAnalyticsAccountProperties
    Tags       : {}
    Type       : Microsoft.DataLakeAnalytics/accounts

Tietyn resurssiryhmän luettelon tietojen järvi Analytics tilit

    Get-AzureRmDataLakeAnalyticsAccount -ResourceGroupName $resourceGroupName

Tiettyjen tietojen järvi Analytics-tilin näyttäminen

    Get-AzureRmDataLakeAnalyticsAccount -Name $adlAnalyticsAccountName

Tiettyjen tietojen järvi Analytics-tilin olemassaolo testaaminen

    Test-AzureRmDataLakeAnalyticsAccount -Name $adlAnalyticsAccountName

Cmdlet-komento palauttaa **arvon TOSI** tai **Epätosi**.

###<a name="delete-data-lake-analytics-accounts"></a>Poista tietojen järvi Analytics-tilit

    $resourceGroupName = "<ResourceGroupName>"
    $dataLakeAnalyticsAccountName = "<DataLakeAnalyticsAccountName>"
    
    Remove-AzureRmDataLakeAnalyticsAccount -Name $dataLakeAnalyticsAccountName 

Poista tietojen järvi Analytics tili ei poista riippuvaiset järvi tietosäilö-tili. Seuraavassa esimerkissä poistaa tietoja järvi Analytics-tili ja järvi tietovaraston oletustilin

    $resourceGroupName = "<ResourceGroupName>"
    $dataLakeAnalyticsAccountName = "<DataLakeAnalyticsAccountName>"
    $dataLakeStoreName = (Get-AzureRmDataLakeAnalyticsAccount -ResourceGroupName $resourceGroupName -Name $dataLakeAnalyticAccountName).Properties.DefaultDataLakeAccount

    Remove-AzureRmDataLakeAnalyticsAccount -ResourceGroupName $resourceGroupName -Name $dataLakeAnalyticAccountName 
    Remove-AzureRmDataLakeStoreAccount -ResourceGroupName $resourceGroupName -Name $dataLakeStoreName

<!-- ################################ -->
<!-- ################################ -->
## <a name="manage-account-data-sources"></a>Tili-tietolähteiden hallinta

Tietoja järvi Analytics tukee tällä hetkellä seuraaviin tietolähteisiin:

- [Azure järvi tietosäilö](../data-lake-store/data-lake-store-overview.md)
- [Azure-tallennustilan](storage-introduction.md)

Kun luot Analytics-tilin, sinun on määritettävä Azure järvi tietosäilö tilin olevan tallennustilan oletustilin. Tietosäilö järvi oletustilin käytetään työn metatietojen ja työn valvontalokien tallentamiseen. Kun olet luonut Analytics-tiliä, voit lisätä järvi tietosäilö tilejä ja/tai Azure-tallennustilan tilin. 

### <a name="find-the-default-data-lake-store-account"></a>Etsi oletustilin järvi tietosäilö

    $resourceGroupName = "<ResourceGroupName>"
    $dataLakeAnalyticsAccountName = "<DataLakeAnalyticsAccountName>"
    $dataLakeStoreName = (Get-AzureRmDataLakeAnalyticsAccount -ResourceGroupName $resourceGroupName -Name $dataLakeAnalyticAccountName).Properties.DefaultDataLakeAccount


### <a name="add-additional-azure-blob-storage-accounts"></a>Lisää Azure Blob storage tilejä

    $resourceGroupName = "<ResourceGroupName>"
    $dataLakeAnalyticsAccountName = "<DataLakeAnalyticsAccountName>"
    $AzureStorageAccountName = "<AzureStorageAccountName>"
    $AzureStorageAccountKey = "<AzureStorageAccountKey>"
    
    Add-AzureRmDataLakeAnalyticsDataSource -ResourceGroupName $resourceGroupName -Account $dataLakeAnalyticAccountName -AzureBlob $AzureStorageAccountName -AccessKey $AzureStorageAccountKey

### <a name="add-additional-data-lake-store-accounts"></a>Lisää tilejä järvi tietosäilö

    $resourceGroupName = "<ResourceGroupName>"
    $dataLakeAnalyticsAccountName = "<DataLakeAnalyticsAccountName>"
    $AzureDataLakeName = "<DataLakeStoreName>"
    
    Add-AzureRmDataLakeAnalyticsDataSource -ResourceGroupName $resourceGroupName -Account $dataLakeAnalyticAccountName -DataLake $AzureDataLakeName 

### <a name="list-data-sources"></a>Luettele tietolähteet:

    $resourceGroupName = "<ResourceGroupName>"
    $dataLakeAnalyticsAccountName = "<DataLakeAnalyticsAccountName>"

    (Get-AzureRmDataLakeAnalyticsAccount -ResourceGroupName $resourceGroupName -Name $dataLakeAnalyticAccountName).Properties.DataLakeStoreAccounts
    (Get-AzureRmDataLakeAnalyticsAccount -ResourceGroupName $resourceGroupName -Name $dataLakeAnalyticAccountName).Properties.StorageAccounts
    


<!-- ################################ -->
<!-- ################################ -->
## <a name="manage-jobs"></a>Töiden hallinta

Sinulla on oltava tietojen järvi Analytics-tiliä, ennen kuin luot projektin.  Lisätietoja on artikkelissa [hallinta tietojen järvi Analytics-tilit](#manage-data-lake-analytics-accounts).

### <a name="list-jobs"></a>Luettelon työt

    $dataLakeAnalyticsAccountName = "<DataLakeAnalyticsAccountName>"
    
    Get-AzureRmDataLakeAnalyticsJob -Account $dataLakeAnalyticAccountName
    
    Get-AzureRmDataLakeAnalyticsJob -Account $dataLakeAnalyticAccountName -State Running, Queued
    #States: Accepted, Compiling, Ended, New, Paused, Queued, Running, Scheduling, Starting
    
    Get-AzureRmDataLakeAnalyticsJob -Account $dataLakeAnalyticAccountName -Result Cancelled
    #Results: Cancelled, Failed, None, Successed 
    
    Get-AzureRmDataLakeAnalyticsJob -Account $dataLakeAnalyticAccountName -Name <Job Name>
    Get-AzureRmDataLakeAnalyticsJob -Account $dataLakeAnalyticAccountName -Submitter <Job submitter>

    # List all jobs submitted on January 1 (local time)
    Get-AzureRmDataLakeAnalyticsJob -Account $dataLakeAnalyticAccountName `
        -SubmittedAfter "2015/01/01"
        -SubmittedBefore "2015/01/02"   

    # List all jobs that succeeded on January 1 after 2 pm (UTC time)
    Get-AzureRmDataLakeAnalyticsJob -Account $dataLakeAnalyticAccountName `
        -State Ended
        -Result Succeeded
        -SubmittedAfter "2015/01/01 2:00 PM -0"
        -SubmittedBefore "2015/01/02 -0"

    # List all jobs submitted in the past hour
    Get-AzureRmDataLakeAnalyticsJob -Account $dataLakeAnalyticAccountName `
        -SubmittedAfter (Get-Date).AddHours(-1)

### <a name="get-job-details"></a>Projektin näyttäminen

    $dataLakeAnalyticsAccountName = "<DataLakeAnalyticsAccountName>"
    Get-AzureRmDataLakeAnalyticsJob -Account $dataLakeAnalyticAccountName -JobID <Job ID>
    
### <a name="submit-jobs"></a>Lähettää työt

    $dataLakeAnalyticsAccountName = "<DataLakeAnalyticsAccountName>"

    #Pass script via path
    Submit-AzureRmDataLakeAnalyticsJob -Account $dataLakeAnalyticAccountName `
        -Name $jobName `
        -ScriptPath $scriptPath

    #Pass script contents
    Submit-AzureRmDataLakeAnalyticsJob -Account $dataLakeAnalyticAccountName `
        -Name $jobName `
        -Script $scriptContents

> [AZURE.NOTE] Työn prioriteettitason oletusarvo on 1 000 ja rinnakkaisuus projektin oletusarvo-aste on 1.


### <a name="cancel-jobs"></a>Peruuta työt

    Stop-AzureRmDataLakeAnalyticsJob -Account $dataLakeAnalyticAccountName `
        -JobID $jobID


## <a name="manage-catalog-items"></a>Luettelon hallitseminen

U-SQL-luettelon käytetään rakenteen tietoja ja -koodia, jotta ne voidaan jakaa U-SQL-komentosarjat. Luettelon mahdollistaa tietojen Azure tietojen järvi mahdollisuuksiin suurin suorituskyvyn. Lisätietoja on artikkelissa [Käytä U-SQL-luettelon](data-lake-analytics-use-u-sql-catalog.md).

###<a name="list-catalog-items"></a>Luettelon luettelokohteita

    #List databases
    Get-AzureRmDataLakeAnalyticsCatalogItem `
        -Account $adlAnalyticsAccountName `
        -ItemType Database
    
    
    
    #List tables
    Get-AzureRmDataLakeAnalyticsCatalogItem `
        -Account $adlAnalyticsAccountName `
        -ItemType Table `
        -Path "master.dbo"

###<a name="get-catalog-item-details"></a>Luettelokohteen näyttäminen 

    #Get a database
    Get-AzureRmDataLakeAnalyticsCatalogItem `
        -Account $adlAnalyticsAccountName `
        -ItemType Database `
        -Path "master"
    
    #Get a table
    Get-AzureRmDataLakeAnalyticsCatalogItem `
        -Account $adlAnalyticsAccountName `
        -ItemType Table `
        -Path "master.dbo.mytable"

###<a name="test-existence-of--catalog-item"></a>Luettelokohteen olemassaolo testaaminen

    Test-AzureRmDataLakeAnalyticsCatalogItem  `
        -Account $adlAnalyticsAccountName `
        -ItemType Database `
        -Path "master"

###<a name="create-catalog-secret"></a>Luo luettelo salaisuus
    New-AzureRmDataLakeAnalyticsCatalogSecret  `
            -Account $adlAnalyticsAccountName `
            -DatabaseName "master" `
            -Secret (Get-Credential -UserName "username" -Message "Enter the password")

### <a name="modify-catalog-secret"></a>Muokkaa luettelon salaisuus
    Set-AzureRmDataLakeAnalyticsCatalogSecret  `
            -Account $adlAnalyticsAccountName `
            -DatabaseName "master" `
            -Secret (Get-Credential -UserName "username" -Message "Enter the password")



###<a name="delete-catalog-secret"></a>Poista luettelon salaisuus
    Remove-AzureRmDataLakeAnalyticsCatalogSecret  `
            -Account $adlAnalyticsAccountName `
            -DatabaseName "master"


## <a name="use-azure-resource-manager-groups"></a>Käytä Azure Resurssienhallinta-ryhmät

Sovellusten yleensä tehty useita osia, kuten verkkosovellukseen, tietokannan, tietokantapalvelimeen, tallennustilan ja 3 osapuolen palvelujen. Azure resurssien hallinta (ARM) mahdollistaa ryhmänä Azure-resurssiryhmä nimitystä sovelluksen resurssien käsitteleminen. Voit käyttöön, Päivitä, valvoa tai poistaa kaikkien resurssien sovelluksen yhteen, koordinoidun toiminnossa. Mallin käyttäminen käyttöönottoa varten ja mallin toimii eri ympäristöissä, kuten testauksen testaus- ja. Voit selventää Laskutus organisaation tarkastelemalla koko ryhmän kootut kustannuksiin. Lisätietoja on artikkelissa [Azure Resurssienhallinta yleiskatsaus](../azure-resource-manager/resource-group-overview.md). 

Tietoja järvi Analytics-palvelu voi sisältää seuraavat osat:

- Azure tietojen järvi Analytics-tili
- Pakollinen oletustilin Azure järvi tietosäilö
- Muita Azure tietojen järvi tallennustilan tilit
- Azuren tallennustilaan tilejä

Voit luoda kaikki komponentit yhden ARM-ryhmässä, jotta ne on helpompi hallita.

![Azure tietojen järvi Analytics-tili ja tallennustilaa](./media/data-lake-analytics-manage-use-portal/data-lake-analytics-arm-structure.png)

Tietoja järvi Analytics-tili ja riippuvainen tallennustilan tilit on asetettava saman Azure tietokeskuksen.
ARM-ryhmän kuitenkaan voi sijaita eri tietokeskuksen.  

##<a name="see-also"></a>Katso myös 

- [Microsoft Azure tietojen järvi Analytics yleiskatsaus](data-lake-analytics-overview.md)
- [Aloita tietojen järvi Analytics Azure-portaalissa](data-lake-analytics-get-started-portal.md)
- [Hallitse Azure tietojen järvi Analytics Azure-portaalissa](data-lake-analytics-manage-use-portal.md)
- [Valvo ja Azure tietojen järvi Analytics työt Azure-portaalissa vianmääritys](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)

##<a name="appendix-a---data-lake-analytics-arm-template"></a>Liite A – tietojen järvi Analytics ARM-malli

Seuraavat ARM-malli voidaan ottaa käyttöön tietojen järvi Analytics- ja sen riippuvaiset järvi tietosäilö-tili.  Tallentaminen json-tiedostona ja soita mallin PowerShell-komentosarjaa avulla. Lisätietoja on artikkelissa [Azure Resurssienhallinta mallilla sovelluksen käyttöönotto](../resource-group-template-deploy.md#deploy-with-powershell) ja [yhtä aikaa muiden kanssa Azure Resurssienhallinta mallit](../resource-group-authoring-templates.md).

    {
      "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "adlAnalyticsName": {
          "type": "string",
          "metadata": {
            "description": "The name of the Data Lake Analytics account to create."
          }
        },
        "adlStoreName": {
          "type": "string",
          "metadata": {
            "description": "The name of the Data Lake Store account to create."
          }
        }
      },
      "resources": [
        {
          "name": "[parameters('adlStoreName')]",
          "type": "Microsoft.DataLakeStore/accounts",
          "location": "East US 2",
          "apiVersion": "2015-10-01-preview",
          "dependsOn": [ ],
          "tags": { }
        },
        {
          "name": "[parameters('adlAnalyticsName')]",
          "type": "Microsoft.DataLakeAnalytics/accounts",
          "location": "East US 2",
          "apiVersion": "2015-10-01-preview",
          "dependsOn": [ "[concat('Microsoft.DataLakeStore/accounts/',parameters('adlStoreName'))]" ],
          "tags": { },
          "properties": {
            "defaultDataLakeStoreAccount": "[parameters('adlStoreName')]",
            "dataLakeStoreAccounts": [
              { "name": "[parameters('adlStoreName')]" }
            ]
          }
        }
      ],
      "outputs": {
        "adlAnalyticsAccount": {
          "type": "object",
          "value": "[reference(resourceId('Microsoft.DataLakeAnalytics/accounts',parameters('adlAnalyticsName')))]"
        },
        "adlStoreAccount": {
          "type": "object",
          "value": "[reference(resourceId('Microsoft.DataLakeStore/accounts',parameters('adlStoreName')))]"
        }
      }
    }

