<properties
   pageTitle="Luo Linux-pohjaiset Hadoop klustereiden HDInsight Azure Resurssienhallinta mallien avulla | Microsoft Azure"
    description="Opettele luomaan klustereiden varten Azure Hdinsightiin Azure Azure Resurssienhallinta mallien avulla."
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
   ms.date="09/02/2016"
   ms.author="jgao"/>

# <a name="create-linux-based-hadoop-clusters-in-hdinsight-using-azure-resource-manager-templates"></a>Luo Linux-pohjaiset Hadoop klustereiden HDInsight Azure Resurssienhallinta mallien käyttäminen

[AZURE.INCLUDE [selector](../../includes/hdinsight-selector-create-clusters.md)]

Opettele luomaan HDInsight klustereiden Azure resurssin Manager(ARM) mallien avulla. Lisätietoja on artikkelissa [Azure Resurssienhallinta mallilla sovelluksen käyttöönotto](../resource-group-template-deploy.md). Klusterin muiden luontia varten työkaluja ja ominaisuuksia välilehti Valitse sivun ylälaidassa tai saat [klusterin luominen](hdinsight-provision-clusters.md#cluster-creation-methods).

##<a name="prerequisites"></a>Edellytykset:

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Ennen kuin aloitat tämän artikkelin ohjeita, sinulla on oltava seuraavasti:

- [Azure tilauksen](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- Azure PowerShell-ja/tai Azure CLI

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-powershell-and-cli.md)]

### <a name="access-control-requirements"></a>Access-ohjausobjektin vaatimukset

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="resource-manager-templates"></a>Resurssienhallinta-mallit

Resurssienhallinta-mallin luominen HDInsight klustereiden, niiden riippuvaiset resurssit (esimerkiksi tallennustilan oletustilin) ja muut resurssit (esimerkiksi Azure SQL-tietokannan käyttäminen Apache Sqoop) sovelluksen yhteen, koordinoidun-toiminnossa on helppoa. Malliin Määritä sovellukselle tarvittavat resurssit ja määritä käyttöönoton parametreja kirjaimilla eri ympäristöissä arvot. Mallin koostuu JSON ja lausekkeita, jonka avulla voit muodostaa käyttöönoton arvot.

Resurssienhallinta mallin HDInsight-klusterin ja riippuvainen Azure-tallennustilan tilin luominen löytyvät [Lisäys A](#appx-a-arm-template). [Resurssienhallinta tietty tunniste](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools) tai tekstieditorissa Office kaikissa ympäristöissä [VSCode](https://code.visualstudio.com/#alt-downloads) avulla voit tallentaa mallin työasemalle-tiedostoon. Opit soittaminen mallin usealla eri tavalla.

Saat lisätietoja Resurssienhallinta mallista

- [Tekijän Azure Resurssienhallinta-mallit](../resource-group-authoring-templates.md)
- [Sovelluksen Azure Resurssienhallinta mallin ottaminen käyttöön](../resource-group-template-deploy.md)

Saat selville JSON rakenteen tiettyjä osia, voit seurata seuraavasti:

1. Avaa [Azure portal](https://porta.azure.com) HDInsight-klusterin luominen.  Katso [luominen Linux-pohjaiset varausyksiköt HDInsight Azure-portaalissa](hdinsight-hadoop-create-linux-clusters-portal.md).
2. Määritä tarvittavat tiedot ja tarvitset JSON rakenteen osat.
3. Ennen kuin valitset **Luo**, valitse **automaatio asetukset** seuraavista näyttökuvan esitetyllä tavalla:

    ![HDInsight Hadoop Luo klusteri Resurssienhallinta-mallin automaatio Rakenneasetukset](./media/hdinsight-hadoop-create-linux-clusters-arm-templates/hdinsight-create-cluster-resource-manager-template-automation-option.png)

    Portaalin Luo Resurssienhallinta mallin perusasetusten pohjalta.
## <a name="deploy-with-powershell"></a>Ottaa käyttöön PowerShellin avulla

Seuraavassa Luo Linux-pohjaiset HDInsight-klusterin.

**Ottaa käyttöön klusterin Resurssienhallinta-mallin avulla**

1. Json-tiedosto tallennetaan [Liite A](#appx-a-arm-template) työasemaan. PowerShell-komentosarjaa tiedostonimi on *C:\HDITutorials-ARM\hdinsight-arm-template.json*.
2. Määritä parametrit ja muuttujien tarvittaessa.
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
        $parameters = @{clusterName="$hdinsightClusterName"}

        New-AzureRmResourceGroupDeployment `
            -Name $armDeploymentName `
            -ResourceGroupName $resourceGroupName `
            -TemplateFile $templateFile `
            -TemplateParameterObject $parameters

        # List cluster
        Get-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName 

    PowerShell-komentosarjaa määrittää vain klusterinimeä. Tallennustilan tilin nimi on englanninkielisissä mallissa. Pyydetään antamaan klusterin-käyttäjän salasanan (oletuskäyttäjänimi on *järjestelmänvalvoja*); ja (oletus SSH käyttäjänimi on *sshuser*) SSH käyttäjän salasana.  
    
Lisätietoja on artikkelissa [käyttöönotto PowerShellin avulla](../resource-group-template-deploy.md#deploy-with-powershell).

## <a name="deploy-with-azure-cli"></a>Azure CLI käyttöönotto

Seuraava esimerkki luo klusterin riippuvaiset tallennustilan tilin ja sen säilö soittamalla Resurssienhallinta-malliin:

    azure login
    azure config mode arm
    azure group create -n hdi1229rg -l "East US"
    azure group deployment create --resource-group "hdi1229rg" --name "hdi1229" --template-file "C:\HDITutorials-ARM\hdinsight-arm-template.json"
    
Voit pyydetään antamaan klusterinimeä ja klusterin käyttäjän salasanan (oletuskäyttäjänimi on *järjestelmänvalvoja*) (oletus SSH käyttäjänimi on *sshuser*) SSH käyttäjän salasana. Anna sitoutuvat parametrit:

    azure group deployment create --resource-group "hdi1229rg" --name "hdi1229" --template-file "c:\Tutorials\HDInsightARM\create-linux-based-hadoop-cluster-in-hdinsight.json" --parameters '{\"clusterName\":{\"value\":\"hdi1229\"},\"clusterLoginPassword\":{\"value\":\"Pass@word1\"},\"sshPassword\":{\"value\":\"Pass@word1\"}}'

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

Seuraavat Azure resurssinhallinta-malli luo Linux-pohjaiset Hadoop-klusterin riippuvaiset Azure-tallennustilan tilin kanssa. 

> [AZURE.NOTE] Malli sisältää kokoonpanotietoja rakenteen metastore ja Oozie metastore.  Poistaa osan tai määrittää osan ennen mallin avulla.

    {
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
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
            "description": "The password must be at least 10 characters in length and must contain at least one digit, one non-alphanumeric character, and one upper or lower case letter."
        }
        },
        "sshUserName": {
        "type": "string",
        "defaultValue": "sshuser",
        "metadata": {
            "description": "These credentials can be used to remotely access the cluster."
        }
        },
        "sshPassword": {
        "type": "securestring",
        "metadata": {
            "description": "The password must be at least 10 characters in length and must contain at least one digit, one non-alphanumeric character, and one upper or lower case letter."
        }
        },
        "location": {
        "type": "string",
        "defaultValue": "East US",
        "allowedValues": [
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
            "Australia East",
            "Australia Southeast"
        ],
        "metadata": {
            "description": "The location where all azure resources will be deployed."
        }
        },
        "clusterType": {
        "type": "string",
        "defaultValue": "hadoop",
        "allowedValues": [
            "hadoop",
            "hbase",
            "storm",
            "spark"
        ],
        "metadata": {
            "description": "The type of the HDInsight cluster to create."
        }
        },
        "clusterWorkerNodeCount": {
        "type": "int",
        "defaultValue": 2,
        "metadata": {
            "description": "The number of nodes in the HDInsight cluster."
        }
        }
    },
    "variables": {
        "defaultApiVersion": "2015-05-01-preview",
        "clusterApiVersion": "2015-03-01-preview",
        "clusterStorageAccountName": "[concat(parameters('clusterName'),'store')]"
    },
    "resources": [
        {
        "name": "[variables('clusterStorageAccountName')]",
        "type": "Microsoft.Storage/storageAccounts",
        "location": "[parameters('location')]",
        "apiVersion": "[variables('defaultApiVersion')]",
        "dependsOn": [ ],
        "tags": { },
        "properties": {
            "accountType": "Standard_LRS"
        }
        },
        {
        "name": "[parameters('clusterName')]",
        "type": "Microsoft.HDInsight/clusters",
        "location": "[parameters('location')]",
        "apiVersion": "[variables('clusterApiVersion')]",
        "dependsOn": [ "[concat('Microsoft.Storage/storageAccounts/',variables('clusterStorageAccountName'))]" ],
        "tags": {

        },
        "properties": {
            "clusterVersion": "3.4",
            "osType": "Linux",
            "tier": "standard",
            "clusterDefinition": {
            "kind": "[parameters('clusterType')]",
            "configurations": {
                "gateway": {
                "restAuthCredential.isEnabled": true,
                "restAuthCredential.username": "[parameters('clusterLoginUserName')]",
                "restAuthCredential.password": "[parameters('clusterLoginPassword')]"
                },
                "hive-site": {
                    "javax.jdo.option.ConnectionDriverName": "com.microsoft.sqlserver.jdbc.SQLServerDriver",
                    "javax.jdo.option.ConnectionURL": "jdbc:sqlserver://myadla0901dbserver.database.windows.net;database=myhive20160901;encrypt=true;trustServerCertificate=true;create=false;loginTimeout=300",
                    "javax.jdo.option.ConnectionUserName": "johndole",
                    "javax.jdo.option.ConnectionPassword": "myPassword$"
                },
                "hive-env": {
                    "hive_database": "Existing MSSQL Server database with SQL authentication",
                    "hive_database_name": "myhive20160901",
                    "hive_database_type": "mssql",
                    "hive_existing_mssql_server_database": "myhive20160901",
                    "hive_existing_mssql_server_host": "myadla0901dbserver.database.windows.net",
                    "hive_hostname": "myadla0901dbserver.database.windows.net"
                },
                "oozie-site": {
                    "oozie.service.JPAService.jdbc.driver": "com.microsoft.sqlserver.jdbc.SQLServerDriver",
                    "oozie.service.JPAService.jdbc.url": "jdbc:sqlserver://myadla0901dbserver.database.windows.net;database=myhive20160901;encrypt=true;trustServerCertificate=true;create=false;loginTimeout=300",
                    "oozie.service.JPAService.jdbc.username": "johndole",
                    "oozie.service.JPAService.jdbc.password": "myPassword$",
                    "oozie.db.schema.name": "oozie"
                },
                "oozie-env": {
                    "oozie_database": "Existing MSSQL Server database with SQL authentication",
                    "oozie_database_name": "myhive20160901",
                    "oozie_database_type": "mssql",
                    "oozie_existing_mssql_server_database": "myhive20160901",
                    "oozie_existing_mssql_server_host": "myadla0901dbserver.database.windows.net",
                    "oozie_hostname": "myadla0901dbserver.database.windows.net"
                }            
            }
            },
            "storageProfile": {
            "storageaccounts": [
                {
                "name": "[concat(variables('clusterStorageAccountName'),'.blob.core.windows.net')]",
                "isDefault": true,
                "container": "[parameters('clusterName')]",
                "key": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', variables('clusterStorageAccountName')), variables('defaultApiVersion')).key1]"
                }
            ]
            },
            "computeProfile": {
            "roles": [
                {
                "name": "headnode",
                "targetInstanceCount": "2",
                "hardwareProfile": {
                    "vmSize": "Standard_D3"
                },
                "osProfile": {
                    "linuxOperatingSystemProfile": {
                    "username": "[parameters('sshUserName')]",
                    "password": "[parameters('sshPassword')]"
                    }
                }
                },
                {
                "name": "workernode",
                "targetInstanceCount": "[parameters('clusterWorkerNodeCount')]",
                "hardwareProfile": {
                    "vmSize": "Standard_D3"
                },
                "osProfile": {
                    "linuxOperatingSystemProfile": {
                    "username": "[parameters('sshUserName')]",
                    "password": "[parameters('sshPassword')]"
                    }
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
