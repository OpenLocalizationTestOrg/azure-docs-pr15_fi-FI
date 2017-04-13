<properties
   pageTitle="Windows-pohjaisesta Hadoop klustereiden luominen Azure Resurssienhallinta mallien avulla HDInsight | Microsoft Azure"
    description="Opettele luomaan klustereiden varten Azure Hdinsightiin Azure Resurssienhallinta mallien avulla."
   services="hdinsight"
   documentationCenter=""
   tags="azure-portal"
   authors="mumian"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/19/2016"
   ms.author="jgao"/>

# <a name="create-windows-based-hadoop-clusters-in-hdinsight-using-azure-resource-manager-templates"></a>Windows-pohjaisesta Hadoop klustereiden luominen HDInsight Azure Resurssienhallinta mallien käyttäminen

[AZURE.INCLUDE [selector](../../includes/hdinsight-selector-create-clusters.md)]

Opettele luomaan HDInsight klustereiden Azure Resurssienhallinta mallien avulla. Lisätietoja on artikkelissa [Azure Resurssienhallinta mallilla sovelluksen käyttöönotto](../resource-group-template-deploy.md). Klusterin muiden luontia varten työkaluja ja ominaisuuksia välilehti Valitse sivun ylälaidassa tai saat [klusterin luominen](hdinsight-provision-clusters.md#cluster-creation-methods).

##<a name="prerequisites"></a>Edellytykset:

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


Ennen kuin aloitat tämän artikkelin ohjeita, sinulla on oltava seuraavasti:

- [Azure tilauksen](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- Azure PowerShell tai Azure CLI

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-powershell-and-cli.md)] 

### <a name="access-control-requirements"></a>Access-ohjausobjektin vaatimukset

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="resource-manager-templates"></a>Resurssienhallinta-mallit

Resurssienhallinta-mallin luominen HDInsight klustereiden, niiden riippuvaiset resurssit (esimerkiksi tallennustilan oletustilin) ja muut resurssit (esimerkiksi Azure SQL-tietokannan käyttäminen Apache Sqoop) sovelluksen yhteen, koordinoidun-toiminnossa on helppoa. Malliin Määritä sovellukselle tarvittavat resurssit ja määritä käyttöönoton parametreja kirjaimilla eri ympäristöissä arvot. Mallin koostuu JSON ja lausekkeita, jonka avulla voit muodostaa käyttöönoton arvot.

Resurssienhallinta mallin HDInsight-klusterin ja seuraajasoluja Azure-tallennustilan tilin luominen löytyvät [Lisäys A](#appx-a-arm-template). Tekstieditorissa avulla voit tallentaa mallin työasemalle-tiedostoon. Opit soittaminen eri työkaluilla malli.

Saat lisätietoja Resurssienhallinta mallista

- [Tekijä Azure Resurssienhallinta-mallit](../resource-group-authoring-templates.md)
- [Sovelluksen Azure Resurssienhallinta mallin ottaminen käyttöön](../resource-group-template-deploy.md)


## <a name="deploy-with-powershell"></a>Ottaa käyttöön PowerShellin avulla

Seuraavassa Luo HDInsight-klusterin.

**Ottaa käyttöön klusterin Resurssienhallinta-mallin avulla**

1. Json-tiedosto tallennetaan [Liite A](#appx-a-arm-template) työasemaan.
2. Määritä parametrit tarvittaessa.
3. Suorita mallissa, käyttämällä seuraavaa PowerShell-komentosarjaa:

        ####################################
        # Set these variables
        ####################################
        #region - used for creating Azure service names
        $nameToken = "<Enter an Alias>" 
        $templateFile = "C:\HDITutorials-ARM\hdinsight-arm-template.json"
        #endregion

        ####################################
        # Service names and varialbes
        ####################################
        #region - service names
        $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")

        $resourceGroupName = $namePrefix + "rg"
        $hdinsightClusterName = $namePrefix + "hdi"
        $defaultStorageAccountName = $namePrefix + "store"
        $defaultBlobContainerName = $hdinsightClusterName

        $location = "East US 2"

        $armDeploymentName = $namePrefix
        #endregion

        ####################################
        # Connect to Azure
        ####################################
        #region - Connect to Azure subscription
        Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
        try{Get-AzureRmContext}
        catch{Login-AzureRmAccount}
        #endregion

        # Create a resource group
        New-AzureRmResourceGroup -Name $resourceGroupName -Location $Location

        # Create cluster and the dependent storage accounge
        $parameters = @{clusterName="$hdinsightClusterName";clusterStorageAccountName="$defaultStorageAccountName"}

        New-AzureRmResourceGroupDeployment `
            -Name $armDeploymentName `
            -ResourceGroupName $resourceGroupName `
            -TemplateFile $templateFile `
            -TemplateParameterObject $parameters

        # List cluster
        Get-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName 

    PowerShell-komentosarjaa määrittää vain klusterin ja tallennustilan tilin nimi.  Voit määrittää muita arvoja Resurssienhallinta-mallissa. 
    
Lisätietoja on artikkelissa [käyttöönotto PowerShellin avulla](../resource-group-template-deploy.md#deploy-with-powershell).

## <a name="deploy-with-azure-cli"></a>Azure CLI käyttöönotto

Seuraava esimerkki luo klusterin riippuvaiset tallennustilan tilin ja sen säilö soittamalla Resurssienhallinta-malliin:

    azure login
    azure config mode arm
    azure group create -n hdi1229rg -l "East US 2"
    azure group deployment create "hdi1229rg" "hdi1229" --template-file "C:\HDITutorials-ARM\hdinsight-arm-template.json" -p "{\"clusterName\":{\"value\":\"hdi1229win\"},\"clusterStorageAccountName\":{\"value\":\"hdi1229store\"},\"location\":{\"value\":\"East US 2\"},\"clusterLoginPassword\":{\"value\":\"Pass@word1\"}}"





## <a name="deploy-with-rest-api"></a>Ottaa käyttöön REST-ohjelmointirajapinnalla

Katso [REST-ohjelmointirajapinnalla käyttöönotto](../resource-group-template-deploy.md#deploy-with-the-rest-api).

## <a name="deploy-with-visual-studio"></a>Ottaa käyttöön Visual Studiossa

Visual Studiossa voit luoda resurssin yhteisessä projektissa ja ota se käyttöön Azure käyttöliittymässä. Resursseja projektia tyypin valitseminen ja resurssit lisätään automaattisesti Resurssienhallinta-malli. Projektin sisältää myös PowerShell-komentosarjaa ottaa mallin käyttöön.

Katso esittely käyttämällä Visual Studio resurssin ryhmiin, [luominen ja käyttöönotto Azure resurssiryhmät Visual Studio kautta](../vs-azure-tools-resource-groups-deployment-projects-create-deploy.md).

##<a name="next-steps"></a>Seuraavat vaiheet
Tässä artikkelissa on oppimiasi HDInsight-klusterin luominen eri tavoin. Lisätietoja on seuraavissa artikkeleissa:


- Katso esimerkki käyttöönotto resursseja .NET asiakkaan kirjaston kautta, [Ota käyttöön resurssien käyttämällä .NET-kirjastojen ja mallin](../virtual-machines/virtual-machines-windows-csharp-template.md).
- Katso tarkempia Esimerkki sovelluksen käyttöönotto- [säännöstä ja ota käyttöön microservices laadukkaampi Azure-](../app-service-web/app-service-deploy-complex-application-predictably.md).
- Käyttöönotto-ratkaisun eri ympäristöissä, artikkelissa [kehitys ja testaa ympäristöissä Microsoft Azure-tietokannassa](../solution-dev-test-environments.md).
- Lisätietoja Azure Resurssienhallinta-mallin osista on artikkelissa [julkaisu malleja](../resource-group-authoring-templates.md).
- Funktiot, voit käyttää Azure Resurssienhallinta-mallin luettelo on artikkelissa [mallin Funktiot](../resource-group-template-functions.md).



##<a name="appx-a-resource-manager-template"></a>Appx A: Resurssienhallinta-malli

Seuraavat Azure resurssinhallinta-malli luo Windows-pohjaisesta Hadoop-klusterin riippuvaiset Azure-tallennustilan tilin kanssa.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
    "parameters": {
        "location": {
        "type": "string",
        "defaultValue": "East US 2",
        "allowedValues": [
            "Central US",
            "North Europe",
            "East US",
            "East US 2",
            "North Central US",
            "South Central US",
            "West US",
            "North Europe",
            "West Europe",
            "East Asia",
            "Southeast Asia",
            "Japan East",
            "Japan West",
            "Brizil South",
            "Australia East",
            "Australia Southeast",
            "Central India"
        ],
        "metadata": {
            "description": "The location where all azure resources will be deployed."
        }
        },
        "clusterName": {
        "type": "string",
        "metadata": {
            "description": "The name of the HDInsight cluster to create."
        }
        },
        "clusterLoginUserName": {
        "type": "string",
        "defaultValue": "admin",
        "metadata": {
            "description": "These credentials can be used to submit jobs to the cluster and to log into cluster dashboards."
        }
        },
        "clusterLoginPassword": {
        "type": "securestring",
        "metadata": {
            "description": "The password for the cluster login."
        }
        },
        "clusterStorageAccountName": {
        "type": "string",
        "metadata": {
            "description": "The name of the storage account to be created and be used as the cluster's storage."
        }
        },
        "clusterStorageType": {
        "type": "string",
        "defaultValue": "Standard_LRS",
        "allowedValues": [
            "Standard_LRS",
            "Standard_GRS",
            "Standard_ZRS"
        ]
        },
        "clusterWorkerNodeCount": {
        "type": "int",
        "defaultValue": 4,
        "metadata": {
            "description": "The number of nodes in the HDInsight cluster."
        }
        }
    },
        "variables": {},
        "resources": [
            {
            "name": "[parameters('clusterStorageAccountName')]",
            "type": "Microsoft.Storage/storageAccounts",
            "location": "[parameters('location')]",
            "apiVersion": "2015-05-01-preview",
            "dependsOn": [],
            "tags": {},
            "properties": {
                "accountType": "[parameters('clusterStorageType')]"
            }
            },
            {
            "name": "[parameters('clusterName')]",
            "type": "Microsoft.HDInsight/clusters",
            "location": "[parameters('location')]",
            "apiVersion": "2015-03-01-preview",
            "dependsOn": [
                "[concat('Microsoft.Storage/storageAccounts/',parameters('clusterStorageAccountName'))]"
            ],
            "tags": {},
            "properties": {
                "clusterVersion": "3.2",
                "osType": "Windows",
                "clusterDefinition": {
                "kind": "hadoop",
                "configurations": {
                    "gateway": {
                    "restAuthCredential.isEnabled": true,
                    "restAuthCredential.username": "[parameters('clusterLoginUserName')]",
                    "restAuthCredential.password": "[parameters('clusterLoginPassword')]"
                    }
                }
                },
                "storageProfile": {
                "storageaccounts": [
                    {
                    "name": "[concat(parameters('clusterStorageAccountName'),'.blob.core.windows.net')]",
                    "isDefault": true,
                    "container": "[parameters('clusterName')]",
                    "key": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('clusterStorageAccountName')), '2015-05-01-preview').key1]"
                    }
                ]
                },
                "computeProfile": {
                "roles": [
                    {
                    "name": "headnode",
                    "targetInstanceCount": "1",
                    "hardwareProfile": {
                        "vmSize": "Large"
                    }
                    },
                    {
                    "name": "workernode",
                    "targetInstanceCount": "[parameters('clusterWorkerNodeCount')]",
                    "hardwareProfile": {
                        "vmSize": "Large"
                    }
                    }
                ]
                }
            }
            }
        ],
        "outputs": {
            "cluster": {
            "type": "object",
            "value": "[reference(resourceId('Microsoft.HDInsight/clusters',parameters('clusterName')))]"
            }
        }
    }

