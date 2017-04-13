<properties
    pageTitle="Opetusohjelma: Luo Resurssienhallinta mallilla putkijohto | Microsoft Azure"
    description="Tässä opetusohjelmassa luot Azure Data Factory-myyntijakso kopioi aktiviteettiin Azure Resurssienhallinta-mallin avulla."
    services="data-factory"
    documentationCenter=""
    authors="spelluru"
    manager="jhubbard"
    editor="monicar"/>

<tags
    ms.service="data-factory"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/10/2016"
    ms.author="spelluru"/>

# <a name="tutorial-create-a-pipeline-with-copy-activity-using-azure-resource-manager-template"></a>Opetusohjelma: Luo putkijohto kopioi aktiviteettiin Azure Resurssienhallinta-mallin avulla
> [AZURE.SELECTOR]
- [Yleiskatsaus ja edellytykset](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
- [Ohjattu kopiointi](data-factory-copy-data-wizard-tutorial.md)
- [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md)
- [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
- [PowerShellin](data-factory-copy-activity-tutorial-using-powershell.md)
- [Azure Resurssienhallinta-malli](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
- [REST-OHJELMOINTIRAJAPINNALLA](data-factory-copy-activity-tutorial-using-rest-api.md)
- [.NET-OHJELMOINTIRAJAPINTA](data-factory-copy-activity-tutorial-using-dotnet-api.md)


Tässä opetusohjelmassa näytetään, miten voit luoda ja seurata Azure tiedot-factory, Azure Resurssienhallinta-mallin avulla. Tietoja factory putkijohto kopioi tiedot Azure-Blob-säiliö Azure SQL-tietokantaan.

## <a name="prerequisites"></a>Edellytykset
- [Opetusohjelma – yleiskatsaus ja edellytykset](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) läpi ja **edellytyksenä** vaiheiden avulla.
- Noudattamalla ohjeita, [miten voit asentaa ja määrittää PowerShellin Azure](../powershell-install-configure.md) artikkelin PowerShellin Azure uusimman version asentaminen tietokoneeseen. Tässä opetusohjelmassa käytetään PowerShell käyttöön Data Factory kohteita. 
- (valinnainen) Katso tietoja Azure Resurssienhallinta [Yhtä aikaa muiden kanssa Azure Resurssienhallinta malleja](../resource-group-authoring-templates.md) .


## <a name="in-this-tutorial"></a>Tässä opetusohjelmassa

Tässä opetusohjelmassa luominen tietojen factory Data Factory-objektien kanssa:

Kohteen | Kuvaus  
------ | ----------- 
Azure linkitetty tallennuspalvelu | Linkki Azure-tallennustilan tilin tiedot factory. Azure-tallennustila on tietovaraston tietolähteen ja Azure SQL-tietokanta on käsittelytoiminto tietovaraston kopioi tehtävän opetusohjelman. Määrittää tallennustilan-tili, joka sisältää syöttötiedot kopioi tehtävän. 
Azure SQL-tietokantaan, jotka on linkitetty-palvelu| Linkittää tiedot factory Azure SQL-tietokantaan. Määrittää Azure SQL-tietokantaan, joka sisältää kopion tehtävän tiedot. 
Azure Blob syötteen tietojoukko | Viittaa Azuren tallennustilaan linkitetty palvelu. Linkitetyn palvelun viittaa Azure-tallennustilan tilin ja Azure-Blob-objektien tietojoukkoa määrittää säilö, kansio ja tiedostonimi muistiin, joka sisältää syöttötiedot. 
Azure SQL-tulostus-tietojoukko | Viittaa linkitetty Azure SQL-palvelu. Linkitetty Azure SQL-palvelu viittaa Azure SQL-palvelimeen ja Azure SQL-tietojoukko määrittää nimen, taulukko, joka sisältää tiedot. 
Tietoja myyntijakso | Putkijohto on yksi tehtävä tyypin kopioiminen, jossa otetaan syötteeksi Azure-blob-tietojoukko ja Azure SQL-tietojoukko kuin tulos. Kopioi tehtävän kopioi tiedot Azure Blob-objektien Azure SQL-tietokannan taulukkoon.  

Tietoja factory voi olla yksi tai useampi putkistot. Putkijohto voi olla vähintään yksi toimintoja ei. Toiminnan kahdenlaisia: [tietojen siirtämistä toimintojen](data-factory-data-movement-activities.md) ja [tietojen muunnos](data-factory-data-transformation-activities.md). Tässä opetusohjelmassa Luo putkijohto ja yksi tehtävä (kopioi tehtävä).

![Azuren Blob-objektien kopioiminen Azure SQL-tietokanta](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/CopyBlob2SqlDiagram.png) 

Seuraavassa osassa on valmis Resurssienhallinta-mallin Data Factory kohteiden määrittäminen niin, että voit nopeasti läpi opetusohjelman ja Testaa malli. Selvittääksesi, miten Data Factory kullekin objektille on määritetty, katso [Data Factory kohteiden mallin](#data-factory-entities-in-the-template) osa.

## <a name="data-factory-json-template"></a>Factory JSON tietomalli
Ylimmän tason Resurssienhallinta mallin määrittäminen tietojen factory on: 

    {
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": { ...
        },
        "variables": { ...
        },
        "resources": [
            {
                "name": "[parameters('dataFactoryName')]",
                "apiVersion": "[variables('apiVersion')]",
                "type": "Microsoft.DataFactory/datafactories",
                "location": "westus",
                "resources": [
                    { ... },
                    { ... },
                    { ... },
                    { ... }
                ]
            }
        ]
    }

Luo JSON-tiedosto nimeltä **ADFCopyTutorialARM.json** seuraavat sisältöä **C:\ADFGetStarted** kansiossa:


    {
        "contentVersion": "1.0.0.0",
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "parameters": {
          "storageAccountName": { "type": "string", "metadata": { "description": "Name of the Azure storage account that contains the data to be copied." } },
          "storageAccountKey": { "type": "securestring", "metadata": { "description": "Key for the Azure storage account." } },
          "sourceBlobContainer": { "type": "string", "metadata": { "description": "Name of the blob container in the Azure Storage account." } },
          "sourceBlobName": { "type": "string", "metadata": { "description": "Name of the blob in the container that has the data to be copied to Azure SQL Database table" } },
          "sqlServerName": { "type": "string", "metadata": { "description": "Name of the Azure SQL Server that will hold the output/copied data." } },
          "databaseName": { "type": "string", "metadata": { "description": "Name of the Azure SQL Database in the Azure SQL server." } },
          "sqlServerUserName": { "type": "string", "metadata": { "description": "Name of the user that has access to the Azure SQL server." } },
          "sqlServerPassword": { "type": "securestring", "metadata": { "description": "Password for the user." } },
          "targetSQLTable": { "type": "string", "metadata": { "description": "Table in the Azure SQL Database that will hold the copied data." } 
          } 
        },
        "variables": {
          "dataFactoryName": "[concat('AzureBlobToAzureSQLDatabaseDF', uniqueString(resourceGroup().id))]",
          "azureSqlLinkedServiceName": "AzureSqlLinkedService",
          "azureStorageLinkedServiceName": "AzureStorageLinkedService",
          "blobInputDatasetName": "BlobInputDataset",
          "sqlOutputDatasetName": "SQLOutputDataset",
          "pipelineName": "Blob2SQLPipeline"
        },
        "resources": [
          {
            "name": "[variables('dataFactoryName')]",
            "apiVersion": "2015-10-01",
            "type": "Microsoft.DataFactory/datafactories",
            "location": "West US",
            "resources": [
              {
                "type": "linkedservices",
                "name": "[variables('azureStorageLinkedServiceName')]",
                "dependsOn": [
                  "[variables('dataFactoryName')]"
                ],
                "apiVersion": "2015-10-01",
                "properties": {
                  "type": "AzureStorage",
                  "description": "Azure Storage linked service",
                  "typeProperties": {
                    "connectionString": "[concat('DefaultEndpointsProtocol=https;AccountName=',parameters('storageAccountName'),';AccountKey=',parameters('storageAccountKey'))]"
                  }
                }
              },
              {
                "type": "linkedservices",
                "name": "[variables('azureSqlLinkedServiceName')]",
                "dependsOn": [
                  "[variables('dataFactoryName')]"
                ],
                "apiVersion": "2015-10-01",
                "properties": {
                  "type": "AzureSqlDatabase",
                  "description": "Azure SQL linked service",
                  "typeProperties": {
                    "connectionString": "[concat('Server=tcp:',parameters('sqlServerName'),'.database.windows.net,1433;Database=', parameters('databaseName'), ';User ID=',parameters('sqlServerUserName'),';Password=',parameters('sqlServerPassword'),';Trusted_Connection=False;Encrypt=True;Connection Timeout=30')]"
                  }
                }
              },
              {
                "type": "datasets",
                "name": "[variables('blobInputDatasetName')]",
                "dependsOn": [
                  "[variables('dataFactoryName')]",
                  "[variables('azureStorageLinkedServiceName')]"
                ],
                "apiVersion": "2015-10-01",
                "properties": {
                  "type": "AzureBlob",
                  "linkedServiceName": "[variables('azureStorageLinkedServiceName')]",
                  "structure": [
                    {
                      "name": "Column0",
                      "type": "String"
                    },
                    {
                      "name": "Column1",
                      "type": "String"
                    }
                  ],
                  "typeProperties": {
                    "folderPath": "[concat(parameters('sourceBlobContainer'), '/')]",
                    "fileName": "[parameters('sourceBlobName')]",
                    "format": {
                      "type": "TextFormat",
                      "columnDelimiter": ","
                    }
                  },
                  "availability": {
                    "frequency": "Day",
                    "interval": 1
                  },
                  "external": true
                }
              },
              {
                "type": "datasets",
                "name": "[variables('sqlOutputDatasetName')]",
                "dependsOn": [
                  "[variables('dataFactoryName')]",
                  "[variables('azureSqlLinkedServiceName')]"
                ],
                "apiVersion": "2015-10-01",
                "properties": {
                  "type": "AzureSqlTable",
                  "linkedServiceName": "[variables('azureSqlLinkedServiceName')]",
                  "structure": [
                    {
                      "name": "FirstName",
                      "type": "String"
                    },
                    {
                      "name": "LastName",
                      "type": "String"
                    }
                  ],
                  "typeProperties": {
                    "tableName": "[parameters('targetSQLTable')]"
                  },
                  "availability": {
                    "frequency": "Day",
                    "interval": 1
                  }
                }
              },
              {
                "type": "datapipelines",
                "name": "[variables('pipelineName')]",
                "dependsOn": [
                  "[variables('dataFactoryName')]",
                  "[variables('azureStorageLinkedServiceName')]",
                  "[variables('azureSqlLinkedServiceName')]",
                  "[variables('blobInputDatasetName')]",
                  "[variables('sqlOutputDatasetName')]"
                ],
                "apiVersion": "2015-10-01",
                "properties": {
                  "activities": [
                    {
                      "name": "CopyFromAzureBlobToAzureSQL",
                      "description": "Copy data frm Azure blob to Azure SQL",
                      "type": "Copy",
                      "inputs": [
                        {
                          "name": "[variables('blobInputDatasetName')]"
                        }
                      ],
                      "outputs": [
                        {
                          "name": "[variables('sqlOutputDatasetName')]"
                        }
                      ],
                      "typeProperties": {
                        "source": {
                          "type": "BlobSource"
                        },
                        "sink": {
                          "type": "SqlSink",
                          "sqlWriterCleanupScript": "$$Text.Format('DELETE FROM {0}', 'emp')"
                        },
                        "translator": {
                          "type": "TabularTranslator",
                          "columnMappings": "Column0:FirstName,Column1:LastName"
                        }
                      },
                      "Policy": {
                        "concurrency": 1,
                        "executionPriorityOrder": "NewestFirst",
                        "retry": 3,
                        "timeout": "01:00:00"
                      }
                    }
                  ],
                  "start": "2016-10-02T00:00:00Z",
                  "end": "2016-10-03T00:00:00Z"
                }
              }
            ]
          }
        ]
      }

## <a name="parameters-json"></a>JSON parametrit 
Luo JSON-tiedosto nimeltä **ADFCopyTutorialARM Parameters.json** , joka sisältää parametrit Azure Resurssienhallinta-mallin. 

> [AZURE.IMPORTANT] Määritä nimi ja Azure-tallennustilan tilin numeronäppäin **storageAccountName** ja **storageAccountKey** parametrit.  

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
        "contentVersion": "1.0.0.0",
        "parameters": { 
            "storageAccountName": { "value": "<Name of the Azure storage account>"  },
            "storageAccountKey": {
                "value": "<Key for the Azure storage account>"
            },
            "sourceBlobContainer": { "value": "adftutorial" },
            "sourceBlobName": { "value": "emp.txt" },
            "sqlServerName": { "value": "<Name of the Azure SQL server>" },
            "databaseName": { "value": "<Name of the Azure SQL database>" },
            "sqlServerUserName": { "value": "<Name of the user who has access to the Azure SQL database>" },
            "sqlServerPassword": { "value": "<password for the user>" },
            "targetSQLTable": { "value": "emp" }
        }
    }

> [AZURE.IMPORTANT] Saatat joutua erillisen parametrin JSON tiedostot kehitystä, testaus ja tuotannon ympäristöissä, joissa voit käyttää samaa tietojen Factory JSON-mallia. PowerShelliä komentosarjan avulla voit automatisoida käyttöönottaminen Data Factory kohteiden näissä ympäristöissä.  

## <a name="create-data-factory"></a>Tietoja factory luominen
1. **PowerShellin Azure** Käynnistä ja suorita seuraava komento:
    - Suorita `Login-AzureRmAccount` ja Anna käyttäjänimi ja salasana, jonka avulla Kirjaudu Azure-portaaliin.  
    - Suorita `Get-AzureRmSubscription` voit tarkastella kaikkien tilausten tilin.
    - Suorita `Get-AzureRmSubscription -SubscriptionName <SUBSCRIPTION NAME> | Set-AzureRmContext` Valitse tilaus, jota haluat käyttää. 
2. Suorita seuraava komento ottaa käyttöön Data Factory kohteiden Resurssienhallinta-mallin avulla vaiheessa 1 luomasi.

        New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile C:\ADFGetStarted\ADFCopyTutorialARM.json -TemplateParameterFile C:\ADFGetStarted\ADFCopyTutorialARM-Parameters.json

## <a name="monitor-pipeline"></a>Näytön myyntijakso
1. Kirjaudu sisään käyttämällä Azure tiliä [Azure portal](https://portal.azure.com) .
2. Valitse **tietojen tehtaan** vasemmalla olevasta valikosta (tai) valitsemalla **Lisää palveluja** ja valitse **tietojen tehtaan** **liiketoimintatietojen + ANALYTICS** -luokassa.

    ![Tehtaan Tietoasetukset](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/data-factories-menu.png)
3. **Tietoja tehtaan** -sivulla Etsi ja etsi tietoja factory. 

    ![Etsi tietoja factory](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/search-for-data-factory.png)  
4. Valitse Azure tietojen factory. Näet tietojen factory kotisivulle.

    ![Tietoja factory kotisivu](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/data-factory-home-page.png)  
5. Napsauta **kaavion** ruutua tietojen factory kaavion näkymää.

    ![Verkkokaavio-näkymän tiedot factory.](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/data-factory-diagram-view.png)
6. Kaavionäkymässä Kaksoisnapsauta tietojoukko **SQLOutputDataset**. Näet sektoria tilaa. Kun kopiointi on valmis, tila arvoksi **Valmis**.

    ![Tulosteen sektoria Valmis-tilaan](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/output-slice-ready.png)
7. Kun sektori on **Valmis** -tilaan, tarkista, että tiedot on kopioitu **emp** juuri Azure SQL-tietokantaan.

Saat ohjeet Azure portaalin näiden avulla voit valvoa putkijohto ja tietojoukkoja olet luonut Tässä opetusohjelmassa [näytön tietojoukkoja ja myyntijakso](data-factory-monitor-manage-pipelines.md) .

Voit käyttää myös näyttö ja hallita sovelluksen tietojen putkistot seurannassa. Katso [näyttö ja hallita Azure Data Factory putkistot seuranta-sovelluksella](data-factory-monitor-manage-app.md) Lisätietoja sovelluksen käyttämisestä.


## <a name="data-factory-entities-in-the-template"></a>Tietoja Factory yksiköt-malli

### <a name="define-data-factory"></a>Määritä tiedot factory
Tietoja factory Määritä hallinta mallin mukaisesti seuraavassa esimerkissä:  

    "resources": [
    {
        "name": "[variables('dataFactoryName')]",
        "apiVersion": "2015-10-01",
        "type": "Microsoft.DataFactory/datafactories",
        "location": "West US"
    }

DataFactoryName määritellään seuraavasti: 
      
    "dataFactoryName": "[concat('AzureBlobToAzureSQLDatabaseDF', uniqueString(resourceGroup().id))]"

Se on yksilöllinen merkkijono, joka perustuu resurssin tunnus.  

### <a name="defining-data-factory-entities"></a>Data Factory kohteiden määrittäminen
Seuraavat tiedot Factory-kohteet on määritetty JSON-mallissa: 

1. [Azure linkitetty tallennuspalvelu](#azure-storage-linked-service)
2. [Azure linkitetty SQL-palvelu](#azure-sql-database-linked-service)
3. [Azure-blob-tietojoukko](#azure-blob-dataset)
4. [Azure SQL-tietojoukko](#azure-sql-dataset)
5. [Tietoja putkijohto ja kopioi tehtävä](#data-pipeline)

#### <a name="azure-storage-linked-service"></a>Azure linkitetty tallennuspalvelu
Tässä osassa Määritä nimi ja Azure-tallennustilan tilin avain. Katso lisätietoja JSON-ominaisuuksia, joilla voidaan määrittää Azuren tallennustilaan linkitetty-palvelun [Azuren tallennustilaan linkitetty palvelu](data-factory-azure-blob-connector.md#azure-storage-linked-service) . 

    {
        "type": "linkedservices",
        "name": "[variables('azureStorageLinkedServiceName')]",
        "dependsOn": [
            "[variables('dataFactoryName')]"
        ],
        "apiVersion": "2015-10-01",
        "properties": {
            "type": "AzureStorage",
            "description": "Azure Storage linked service",
            "typeProperties": {
                "connectionString": "[concat('DefaultEndpointsProtocol=https;AccountName=',parameters('storageAccountName'),';AccountKey=',parameters('storageAccountKey'))]"
            }
        }
    }

ConnectionString käyttää storageAccountName ja storageAccountKey parametreja. Näiden avulla määritystiedostoa välitetty parametrien arvot. Määritelmän käyttää myös muuttujat: azureStroageLinkedService ja dataFactoryName määritetty malli. 
    
#### <a name="azure-sql-database-linked-service"></a>Azure SQL-tietokantaan, jotka on linkitetty-palvelu
Tässä osassa Määritä Azure SQL-palvelimen nimi, tietokannan nimi, käyttäjänimi ja salasana. Saat tietoja JSON-ominaisuuksia, joilla voidaan määrittää linkitetty Azure SQL-palvelun [Azure SQL-linkitetty palvelun](data-factory-azure-sql-connector.md#azure-sql-linked-service-properties) .  

    {
        "type": "linkedservices",
        "name": "[variables('azureSqlLinkedServiceName')]",
        "dependsOn": [
          "[variables('dataFactoryName')]"
        ],
        "apiVersion": "2015-10-01",
        "properties": {
            "type": "AzureSqlDatabase",
            "description": "Azure SQL linked service",
            "typeProperties": {
                "connectionString": "[concat('Server=tcp:',parameters('sqlServerName'),'.database.windows.net,1433;Database=', parameters('databaseName'), ';User ID=',parameters('sqlServerUserName'),';Password=',parameters('sqlServerPassword'),';Trusted_Connection=False;Encrypt=True;Connection Timeout=30')]"
            }
        }
    }

ConnectionString käyttää SQL-palvelimen nimi, nimi tai sqlServerUserName sqlServerPassword parametreja, jonka arvot on välitetty määritystiedoston avulla. Määritelmän käyttää myös seuraavat muuttujat mallista: azureSqlLinkedServiceName, dataFactoryName.

#### <a name="azure-blob-dataset"></a>Azure-blob-tietojoukko
Voit määrittää blob-säilö, kansion tai tiedoston, joka sisältää syöttötiedot nimet. Katso lisätietoja JSON-ominaisuuksia, joilla voidaan määrittää Azure-Blob-objektien tietojoukkoa [Azure-Blob-objektien tietojoukkoa ominaisuudet](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties) . 


    {
        "type": "datasets",
        "name": "[variables('blobInputDatasetName')]",
        "dependsOn": [
          "[variables('dataFactoryName')]",
          "[variables('azureStorageLinkedServiceName')]"
        ],
        "apiVersion": "2015-10-01",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "[variables('azureStorageLinkedServiceName')]",
            "structure": [
            {
                "name": "Column0",
                "type": "String"
            },
            {
                "name": "Column1",
                "type": "String"
            }
            ],
            "typeProperties": {
                "folderPath": "[concat(parameters('sourceBlobContainer'), '/')]",
                "fileName": "[parameters('sourceBlobName')]",
                "format": {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                }
            },
            "availability": {
                "frequency": "Day",
                "interval": 1
            },
            "external": true
        }
    }

#### <a name="azure-sql-dataset"></a>Azure SQL-tietojoukko
Taulukon nimeä, Määritä Azure SQL-tietokantaan, joka sisältää Azure-Blob-säiliö kopioidut tiedot. Katso lisätietoja JSON-ominaisuuksia, joilla voidaan määrittää Azure SQL-tietojoukko [Azure SQL-tietojoukko ominaisuudet](data-factory-azure-sql-connector.md#azure-sql-dataset-type-properties) . 

    {
        "type": "datasets",
        "name": "[variables('sqlOutputDatasetName')]",
        "dependsOn": [
            "[variables('dataFactoryName')]",
            "[variables('azureSqlLinkedServiceName')]"
        ],
        "apiVersion": "2015-10-01",
        "properties": {
            "type": "AzureSqlTable",
            "linkedServiceName": "[variables('azureSqlLinkedServiceName')]",
            "structure": [
            {
                "name": "FirstName",
                "type": "String"
            },
            {
                "name": "LastName",
                "type": "String"
            }
            ],
            "typeProperties": {
                "tableName": "[parameters('targetSQLTable')]"
            },
            "availability": {
                "frequency": "Day",
                "interval": 1
            }
        }
    }

#### <a name="data-pipeline"></a>Tietoja myyntijakso
Voit määrittää, kopioi tiedot Azuren Blob-objektien tietojoukkoa Azure SQL-tietojoukko putkijohto. Katso [Myyntijakso JSON](data-factory-create-pipelines.md#pipeline-json) JSON elementit määritetään putkijohto tässä esimerkissä kuvauksia. 

    {
        "type": "datapipelines",
        "name": "[variables('pipelineName')]",
        "dependsOn": [
            "[variables('dataFactoryName')]",
            "[variables('azureStorageLinkedServiceName')]",
            "[variables('azureSqlLinkedServiceName')]",
            "[variables('blobInputDatasetName')]",
            "[variables('sqlOutputDatasetName')]"
        ],
        "apiVersion": "2015-10-01",
        "properties": {
            "activities": [
            {
                "name": "CopyFromAzureBlobToAzureSQL",
                "description": "Copy data frm Azure blob to Azure SQL",
                "type": "Copy",
                "inputs": [
                {
                    "name": "[variables('blobInputDatasetName')]"
                }
                ],
                "outputs": [
                {
                    "name": "[variables('sqlOutputDatasetName')]"
                }
                ],
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "sink": {
                        "type": "SqlSink",
                        "sqlWriterCleanupScript": "$$Text.Format('DELETE FROM {0}', 'emp')"
                    },
                    "translator": {
                        "type": "TabularTranslator",
                        "columnMappings": "Column0:FirstName,Column1:LastName"
                    }
                },
                "Policy": {
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 3,
                    "timeout": "01:00:00"
                }
            }
            ],
            "start": "2016-10-02T00:00:00Z",
            "end": "2016-10-03T00:00:00Z"
        }
    }

## <a name="reuse-the-template"></a>Mallin käyttäminen uudelleen 
Opetusohjelman luomasi mallin määrittäminen Data Factory yksiköt ja mallin kulkeva parametrien arvot. Putkijohto kopioi Azure-tallennustilan tilin tiedot Azure SQL-tietokantaan määritettyä parametrit. Saman mallin avulla voit ottaa Data Factory kohteiden eri ympäristöissä, parametrin tiedoston kussakin ympäristössä ja käyttää sitä, että ympäristö käyttöönoton yhteydessä.     

Esimerkki:  

    New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFCopyTutorialARM.json -TemplateParameterFile ADFCopyTutorialARM-Parameters-Dev.json

    New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFCopyTutorialARM.json -TemplateParameterFile ADFCopyTutorialARM-Parameters-Test.json

    New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFCopyTutorialARM.json -TemplateParameterFile ADFCopyTutorialARM-Parameters-Production.json

Huomaa, että ensimmäinen komento käyttää parametri tiedosto kehitysympäristö testiympäristössä toinen ja kolmas yksi vastaavat tuotantoympäristössä.  

Voit myös käyttää malleja toistuvia tehtäviä. Esimerkiksi haluat luoda monta tietojen tehtaan vähintään yksi putkistot, jotka toteuttavat saman logiikan, mutta tietojen kunkin factory käyttää eri Azure tallennustilan ja Azure SQL-tietokanta-tilit. Tässä skenaariossa voit samaan malliin saman ympäristön (keskihajonta, Testaa tai tuotannon) eri parametrin tiedostojen luominen tietojen tehtaan.   

