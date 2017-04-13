<properties
    pageTitle="Luo ensimmäinen tietojen factory (Resurssienhallinta-malli) | Microsoft Azure"
    description="Tässä opetusohjelmassa luot otoksen Azure Data Factory putkijohto Azure Resurssienhallinta-mallin avulla."
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
    ms.topic="hero-article"
    ms.date="10/12/2016"
    ms.author="spelluru"/>

# <a name="tutorial-build-your-first-azure-data-factory-using-azure-resource-manager-template"></a>Opetusohjelma: Luo ensimmäinen factory Azure tietojen Azure Resurssienhallinta-mallin avulla
> [AZURE.SELECTOR]
- [Yleiskatsaus ja edellytykset](data-factory-build-your-first-pipeline.md)
- [Azure portal](data-factory-build-your-first-pipeline-using-editor.md)
- [Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
- [PowerShellin](data-factory-build-your-first-pipeline-using-powershell.md)
- [Resurssienhallinta-malli](data-factory-build-your-first-pipeline-using-arm.md)
- [REST-OHJELMOINTIRAJAPINNALLA](data-factory-build-your-first-pipeline-using-rest-api.md)

Tässä artikkelissa käyttää Azure Resurssienhallinta-mallin ensimmäisen Azure tiedot-factory luomiseen.

## <a name="prerequisites"></a>Edellytykset
- Lue artikkeli [Opetusohjelma yleiskatsaus](data-factory-build-your-first-pipeline.md) ja **edellytyksenä** vaiheiden avulla.
- Noudattamalla ohjeita, [miten voit asentaa ja määrittää PowerShellin Azure](../powershell-install-configure.md) artikkelin PowerShellin Azure uusimman version asentaminen tietokoneeseen.
- Katso tietoja Azure Resurssienhallinta [Yhtä aikaa muiden kanssa Azure Resurssienhallinta malleja](../resource-group-authoring-templates.md) . 

## <a name="in-this-tutorial"></a>Tässä opetusohjelmassa
Kohteen | Kuvaus  
------ | ----------- 
Azure linkitetty tallennuspalvelu | Linkki Azure-tallennustilan tilin tiedot factory. Azure-tallennustilan tilin on tässä esimerkissä putkijohto syöttö- ja tiedot. 
HDInsight tarvittaessa linkitetyn-palvelu| Linkit tarvittaessa HDInsight klusterin tietojen factory. Klusterin luodaan automaattisesti, prosessi-tietoihin ja poistetaan, kun käsittely on valmis.
Azure Blob syötteen tietojoukko | Viittaa Azuren tallennustilaan linkitetty palvelu. Linkitetyn palvelun viittaa Azure-tallennustilan tilin ja Azure-Blob-objektien tietojoukkoa määrittää säilö, kansio ja tiedostonimi muistiin, joka sisältää syöttötiedot. 
Azure-Blob tulosteen tietojoukko | Viittaa Azuren tallennustilaan linkitetty palvelu. Linkitetyn palvelun viittaa Azure-tallennustilan tilin ja Azure-Blob-objektien tietojoukkoa määrittää säilö, kansio ja tiedostonimi muistiin, joka sisältää tiedot. 
Tietoja myyntijakso | Putkijohto on yksi tehtävän tyyppi HDInsightHive siinä käytetään syötteen tietojoukko ja tuottaa tulostus-tietojoukko.   

Tietoja factory voi olla yksi tai useampi putkistot. Putkijohto voi olla vähintään yksi toimintoja ei. Toiminnan kahdenlaisia: [tietojen siirtämistä toimintojen](data-factory-data-movement-activities.md) ja [tietojen muunnos](data-factory-data-transformation-activities.md). Tässä opetusohjelmassa Luo putkijohto ja yksi tehtävä (kopioi tehtävä).

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

Luo JSON-tiedosto nimeltä **ADFTutorialARM.json** seuraavat sisältöä **C:\ADFGetStarted** kansiossa:

    {
        "contentVersion": "1.0.0.0",
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "parameters": {
            "storageAccountName": { "type": "string", "metadata": { "description": "Name of the Azure storage account that contains the input/output data." } },
            "storageAccountKey": { "type": "securestring", "metadata": { "description": "Key for the Azure storage account." } },
            "blobContainer": { "type": "string", "metadata": { "description": "Name of the blob container in the Azure Storage account." } },
            "inputBlobFolder": { "type": "string", "metadata": { "description": "The folder in the blob container that has the input file." } },
            "inputBlobName": { "type": "string", "metadata": { "description": "Name of the input file/blob." } },
            "outputBlobFolder": { "type": "string", "metadata": { "description": "The folder in the blob container that will hold the transformed data." } },
            "hiveScriptFolder": { "type": "string", "metadata": { "description": "The folder in the blob container that contains the Hive query file." } },
            "hiveScriptFile": { "type": "string", "metadata": { "description": "Name of the hive query (HQL) file." } }
        },
        "variables": {
            "dataFactoryName": "[concat('HiveTransformDF', uniqueString(resourceGroup().id))]",
            "azureStorageLinkedServiceName": "AzureStorageLinkedService",
            "hdInsightOnDemandLinkedServiceName": "HDInsightOnDemandLinkedService",
            "blobInputDatasetName": "AzureBlobInput",
            "blobOutputDatasetName": "AzureBlobOutput",
            "pipelineName": "HiveTransformPipeline"
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
                "name": "[variables('hdInsightOnDemandLinkedServiceName')]",
                "dependsOn": [
                    "[variables('dataFactoryName')]",
                    "[variables('azureStorageLinkedServiceName')]"
                ],
                "apiVersion": "2015-10-01",
                "properties": {
                    "type": "HDInsightOnDemand",
                    "typeProperties": {
                        "clusterSize": 1,
                        "version": "3.2",
                        "timeToLive": "00:05:00",
                        "osType": "windows",
                        "linkedServiceName": "[variables('azureStorageLinkedServiceName')]"
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
                    "typeProperties": {
                        "fileName": "[parameters('inputBlobName')]",
                        "folderPath": "[concat(parameters('blobContainer'), '/', parameters('inputBlobFolder'))]",
                        "format": {
                            "type": "TextFormat",
                            "columnDelimiter": ","
                        }
                    },
                    "availability": {
                        "frequency": "Month",
                        "interval": 1
                    },
                    "external": true
                }
            },
            {
                "type": "datasets",
                "name": "[variables('blobOutputDatasetName')]",
                "dependsOn": [
                    "[variables('dataFactoryName')]",
                    "[variables('azureStorageLinkedServiceName')]"
                ],
                "apiVersion": "2015-10-01",
                "properties": {
                    "type": "AzureBlob",
                    "linkedServiceName": "[variables('azureStorageLinkedServiceName')]",
                    "typeProperties": {
                        "folderPath": "[concat(parameters('blobContainer'), '/', parameters('outputBlobFolder'))]",
                        "format": {
                            "type": "TextFormat",
                            "columnDelimiter": ","
                        }
                    },
                    "availability": {
                        "frequency": "Month",
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
                    "[variables('hdInsightOnDemandLinkedServiceName')]",
                    "[variables('blobInputDatasetName')]",
                    "[variables('blobOutputDatasetName')]"
                ],
                "apiVersion": "2015-10-01",
                "properties": {
                    "description": "Pipeline that transforms data using Hive script.",
                    "activities": [
                    {
                        "type": "HDInsightHive",
                        "typeProperties": {
                            "scriptPath": "[concat(parameters('blobContainer'), '/', parameters('hiveScriptFolder'), '/', parameters('hiveScriptFile'))]",
                            "scriptLinkedService": "[variables('azureStorageLinkedServiceName')]",
                            "defines": {
                                "inputtable": "[concat('wasb://', parameters('blobContainer'), '@', parameters('storageAccountName'), '.blob.core.windows.net/', parameters('inputBlobFolder'))]",
                                "partitionedtable": "[concat('wasb://', parameters('blobContainer'), '@', parameters('storageAccountName'), '.blob.core.windows.net/', parameters('outputBlobFolder'))]"
                            }
                        },
                        "inputs": [
                            {
                                "name": "[variables('blobInputDatasetName')]"
                            }
                        ],
                        "outputs": [
                            {
                                "name": "[variables('blobOutputDatasetName')]"
                            }
                        ],
                        "policy": {
                            "concurrency": 1,
                            "retry": 3
                        },
                        "scheduler": {
                            "frequency": "Month",
                            "interval": 1
                        },
                        "name": "RunSampleHiveActivity",
                        "linkedServiceName": "[variables('hdInsightOnDemandLinkedServiceName')]"
                    }
                    ],
                    "start": "2016-10-01T00:00:00Z",
                    "end": "2016-10-02T00:00:00Z",
                    "isPaused": false
                }
            }
            ]
        }
        ]
    }

> [AZURE.NOTE] Voit etsiä Resurssienhallinta mallin toinen esimerkki Azure tietojen factory luomisesta [Opetusohjelma: Luo putkijohto kopioi aktiviteettiin Azure Resurssienhallinta-mallin avulla](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md).  

## <a name="parameters-json"></a>JSON parametrit 
Luo JSON-tiedosto nimeltä **ADFTutorialARM Parameters.json** , joka sisältää parametrit Azure Resurssienhallinta-mallin.  

> [AZURE.IMPORTANT] Määritä nimi ja Azure-tallennustilan tilin numeronäppäin **storageAccountName** ja **storageAccountKey** parametrit parametrin tässä tiedostossa. 

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "storageAccountName": {
                "value": "<Name of your Azure Storage account>"
            },
            "storageAccountKey": {
                "value": "<Key of your Azure Storage account>"
            },
            "blobContainer": {
                "value": "adfgetstarted"
            },
            "inputBlobFolder": {
                "value": "inputdata"
            },
            "inputBlobName": {
                "value": "input.log"
            },
            "outputBlobFolder": {
                "value": "partitioneddata"
            },
            "hiveScriptFolder": {
                "value": "script"
            },
            "hiveScriptFile": {
                "value": "partitionweblogs.hql"
            }
        }
    }

> [AZURE.IMPORTANT] Saatat joutua erillisen parametrin JSON tiedostot kehitystä, testaus ja tuotannon ympäristöissä, joissa voit käyttää samaa tietojen Factory JSON-mallia. PowerShelliä komentosarjan avulla voit automatisoida käyttöönottaminen Data Factory kohteiden näissä ympäristöissä. 

## <a name="create-data-factory"></a>Tietoja factory luominen

1. **PowerShellin Azure** Käynnistä ja suorita seuraava komento: 
    - Suorita `Login-AzureRmAccount` ja Anna käyttäjänimi ja salasana, jonka avulla Kirjaudu Azure-portaaliin.  
    - Suorita `Get-AzureRmSubscription` voit tarkastella kaikkien tilausten tilin.
    - Suorita `Get-AzureRmSubscription -SubscriptionName <SUBSCRIPTION NAME> | Set-AzureRmContext` Valitse tilaus, jota haluat käyttää. Tämä tilaus on oltava sama kuin käytit Azure-portaalissa.
1. Suorita seuraava komento ottaa käyttöön Data Factory kohteiden Resurssienhallinta-mallin avulla vaiheessa 1 luomasi. 

        New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile C:\ADFGetStarted\ADFTutorialARM.json -TemplateParameterFile C:\ADFGetStarted\ADFTutorialARM-Parameters.json

## <a name="monitor-pipeline"></a>Näytön myyntijakso
 
1.  Sen jälkeen kirjautumisesta [Azure portal](https://portal.azure.com/), valitse **Selaa** ja valitse **tietoja tehtaan**.
        ![Selaa -> tietojen tehtaan](./media/data-factory-build-your-first-pipeline-using-arm/BrowseDataFactories.png)
2.  Valitse tietojen factory (**TutorialFactoryARM**) luomasi **Tehtaan tiedot** -sivu.   
2.  Valitse **kaavio**, tietojen factory **Data Factory** -sivu.
        ![Kaavio-ruutu](./media/data-factory-build-your-first-pipeline-using-arm/DiagramTile.png)
4.  **Kaavionäkymä**näet, putkistot ja käyttää tässä opetusohjelmassa tietojoukkoja.
    
    ![Kaavionäkymä](./media/data-factory-build-your-first-pipeline-using-arm/DiagramView.png) 
8. Kaavionäkymässä Kaksoisnapsauta tietojoukko **AzureBlobOutput**. Näet, joka käsitellään parhaillaan sektoria.

    ![Tietojoukko](./media/data-factory-build-your-first-pipeline-using-arm/AzureBlobOutput.png)
9. Kun käsittely on valmis, näyttöön tulee sektoria **Valmis** -tilaan. Tarvittaessa-HDInsight-klusterin luominen yleensä kestää viime (noin 20 minuuttia). Tämän vuoksi toiminta voi kestää **noin 30 minuuttia** käsittelemään sektoria putkijohto.

    ![Tietojoukko](./media/data-factory-build-your-first-pipeline-using-arm/SliceReady.png) 
10. Kun sektoria on **valmiina** , tarkista **adfgetstarted** säilössä Blob-objektien tallennustilaan tulosteen tietojen **partitioneddata** -kansio.  

Saat ohjeet Azure portaalin näiden avulla voit valvoa putkijohto ja tietojoukkoja olet luonut Tässä opetusohjelmassa [näytön tietojoukkoja ja myyntijakso](data-factory-monitor-manage-pipelines.md) .

Voit käyttää myös näyttö ja hallita sovelluksen tietojen putkistot seurannassa. Katso [näyttö ja hallita Azure Data Factory putkistot seuranta-sovelluksella](data-factory-monitor-manage-app.md) Lisätietoja sovelluksen käyttämisestä. 

> [AZURE.IMPORTANT] Lähdetiedoston saa poistetaan, kun sektoria käsittely on onnistunut. Vuoksi halutessasi uudelleen sektoria tai tehdä opetusohjelman uudelleen input-tiedoston lataaminen (input.log) adfgetstarted säilön inputdata-kansio.

## <a name="data-factory-entities-in-the-template"></a>Tietoja Factory yksiköt-malli
### <a name="define-data-factory"></a>Määritä tiedot factory
Määritä tiedot-factory Resurssienhallinta mallissa seuraava näyte esitetyllä tavalla:  

    "resources": [
    {
        "name": "[variables('dataFactoryName')]",
        "apiVersion": "2015-10-01",
        "type": "Microsoft.DataFactory/datafactories",
        "location": "West US"
    }

DataFactoryName määritellään seuraavasti: 
      
      "dataFactoryName": "[concat('HiveTransformDF', uniqueString(resourceGroup().id))]",

Se on yksilöllinen merkkijono, joka perustuu resurssin tunnus.  

### <a name="defining-data-factory-entities"></a>Data Factory kohteiden määrittäminen
Seuraavat tiedot Factory-kohteet on määritetty JSON-mallissa: 

- [Azure linkitetty tallennuspalvelu](#azure-storage-linked-service)
- [HDInsight tarvittaessa linkitetyn-palvelu](#hdinsight-on-demand-linked-service)
- [Azure-blob-syötteen tietojoukko](#azure-blob-input-dataset)
- [Azure-blob-tulostus-tietojoukko](#azure-blob-output-dataset)
- [Tietoja putkijohto ja kopioi tehtävä](#data-pipeline)

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

**ConnectionString** käyttää storageAccountName ja storageAccountKey parametreja. Näiden avulla määritystiedostoa välitetty parametrien arvot. Määritelmän käyttää myös muuttujat: azureStroageLinkedService ja dataFactoryName määritetty malli. 
    
#### <a name="hdinsight-on-demand-linked-service"></a>HDInsight tarvittaessa linkitetyn-palvelu
Katso [Laske linkitetyn services](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) artikkelissa lisätietoja JSON-ominaisuuksia, joilla voidaan määrittää HDInsight tarvittaessa linkitetyn palvelu.  

      {
        "type": "linkedservices",
        "name": "[variables('hdInsightOnDemandLinkedServiceName')]",
        "dependsOn": [
          "[variables('dataFactoryName')]"
        ],
        "apiVersion": "2015-10-01",
        "properties": {
          "type": "HDInsightOnDemand",
          "typeProperties": {
            "clusterSize": 1,
            "version": "3.2",
            "timeToLive": "00:05:00",
            "osType": "windows",
            "linkedServiceName": "[variables('azureStorageLinkedServiceName')]"
          }
        }
      }

Huomaa seuraavat seikat: 

- Data Factory Luo **Windows-pohjaisesta** HDInsight-klusterin yllä JSON kanssa. Voit myös voi olla se **Linux-pohjaiset** HDInsight-klusterin luominen. Katso [Tarvittaessa HDInsight linkitetyn palvelun](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) tiedot. 
- Voit käyttää **omaa HDInsight-klusterin** tarvittaessa-HDInsight-klusterin sijaan. Katso [HDInsight linkitetyn palvelun](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) tiedot.
- HDInsight-klusterin Luo **oletusarvon säilö** blob-säiliö määrittämäsi JSON (**linkedServiceName**). HDInsight Poista tämä säilö klusterin poistamisen yhteydessä. Tämä on suunniteltu ominaisuus. Tarvittaessa linkitetty HDInsight-palveluun HDInsight, klusterin luodaan aina, kun sektoria on käsiteltävä, ellei aiemmin luodun klusterin (**timeToLive**) live ja poistetaan, kun käsittely on valmis.

    Lisää sektoreiden käsitellään, näet monta säilöjen Azuren Blob-objektien tallennustilaan. Jos ei tarvitse niitä töiden vianmääritystä varten, haluat ehkä poistaa ne tallennustilan pienentää. Nämä säilöjä nimet noudattavat kuviolla: "syöttö**yourdatafactoryname**-**linkedservicename**- datetimestamp". Poistaa Azure-Blob-objektien tallennustilaan säilöjen Työkalut [Exploreria tallennustilan](http://storageexplorer.com/) kuten avulla.

Katso [Tarvittaessa HDInsight linkitetyn palvelun](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) tiedot.



#### <a name="azure-blob-input-dataset"></a>Azure-blob-syötteen tietojoukko
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
          "typeProperties": {
            "fileName": "[parameters('inputBlobName')]",
            "folderPath": "[concat(parameters('blobContainer'), '/', parameters('inputBlobFolder'))]",
            "format": {
              "type": "TextFormat",
              "columnDelimiter": ","
            }
          },
          "availability": {
            "frequency": "Month",
            "interval": 1
          },
          "external": true
        }
      }

Tämä määritelmä käyttää parametrin mallin seuraavat parametrit: blobContainer, inputBlobFolder, ja inputBlobName. 

#### <a name="azure-blob-output-dataset"></a>Azure-Blob tulosteen tietojoukko
Voit määrittää blob-säilö ja kansio, joka sisältää tiedot nimet. Katso lisätietoja JSON-ominaisuuksia, joilla voidaan määrittää Azure-Blob-objektien tietojoukkoa [Azure-Blob-objektien tietojoukkoa ominaisuudet](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties) .  

      {
        "type": "datasets",
        "name": "[variables('blobOutputDatasetName')]",
        "dependsOn": [
          "[variables('dataFactoryName')]",
          "[variables('azureStorageLinkedServiceName')]"
        ],
        "apiVersion": "2015-10-01",
        "properties": {
          "type": "AzureBlob",
          "linkedServiceName": "[variables('azureStorageLinkedServiceName')]",
          "typeProperties": {
            "folderPath": "[concat(parameters('blobContainer'), '/', parameters('outputBlobFolder'))]",
            "format": {
              "type": "TextFormat",
              "columnDelimiter": ","
            }
          },
          "availability": {
            "frequency": "Month",
            "interval": 1
          }
        }
      }

Tämä määritelmä käyttää mallissa parametrin seuraavat parametrit: blobContainer ja outputBlobFolder. 

#### <a name="data-pipeline"></a>Tietoja myyntijakso
Voit määrittää, jotka muuntavat tiedot suorittamalla rakenteen komentosarja tarvittaessa-Azure Hdinsightiin klusterissa putkijohto. Katso [Myyntijakso JSON](data-factory-create-pipelines.md#pipeline-json) JSON elementit määritetään putkijohto tässä esimerkissä kuvauksia. 

    {
        "type": "datapipelines",
        "name": "[variables('pipelineName')]",
        "dependsOn": [
          "[variables('dataFactoryName')]",
          "[variables('azureStorageLinkedServiceName')]",
          "[variables('hdInsightOnDemandLinkedServiceName')]",
          "[variables('blobInputDatasetName')]",
          "[variables('blobOutputDatasetName')]"
        ],
        "apiVersion": "2015-10-01",
        "properties": {
          "description": "Pipeline that transforms data using Hive script.",
          "activities": [
            {
              "type": "HDInsightHive",
              "typeProperties": {
                "scriptPath": "[concat(parameters('blobContainer'), '/', parameters('hiveScriptFolder'), '/', parameters('hiveScriptFile'))]",
                "scriptLinkedService": "[variables('azureStorageLinkedServiceName')]",
                "defines": {
                  "inputtable": "[concat('wasb://', parameters('blobContainer'), '@', parameters('storageAccountName'), '.blob.core.windows.net/', parameters('inputBlobFolder'))]",
                  "partitionedtable": "[concat('wasb://', parameters('blobContainer'), '@', parameters('storageAccountName'), '.blob.core.windows.net/', parameters('outputBlobFolder'))]"
                }
              },
              "inputs": [
                {
                  "name": "[variables('blobInputDatasetName')]"
                }
              ],
              "outputs": [
                {
                  "name": "[variables('blobOutputDatasetName')]"
                }
              ],
              "policy": {
                "concurrency": 1,
                "retry": 3
              },
              "scheduler": {
                "frequency": "Month",
                "interval": 1
              },
              "name": "RunSampleHiveActivity",
              "linkedServiceName": "[variables('hdInsightOnDemandLinkedServiceName')]"
            }
          ],
          "start": "2016-10-01T00:00:00Z",
          "end": "2016-10-02T00:00:00Z",
          "isPaused": false
        }
      }

## <a name="reuse-the-template"></a>Mallin käyttäminen uudelleen 
Opetusohjelman luomasi mallin määrittäminen Data Factory yksiköt ja mallin kulkeva parametrien arvot. Saman mallin avulla voit ottaa Data Factory kohteiden eri ympäristöissä, parametrin tiedoston kussakin ympäristössä ja käyttää sitä, että ympäristö käyttöönoton yhteydessä.     

Esimerkki:  

    New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFTutorialARM.json -TemplateParameterFile ADFTutorialARM-Parameters-Dev.json

    New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFTutorialARM.json -TemplateParameterFile ADFTutorialARM-Parameters-Test.json

    New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFTutorialARM.json -TemplateParameterFile ADFTutorialARM-Parameters-Production.json

Huomaa, että ensimmäinen komento käyttää parametri tiedosto kehitysympäristö testiympäristössä toinen ja kolmas yksi vastaavat tuotantoympäristössä.  

Voit myös käyttää malleja toistuvia tehtäviä. Esimerkiksi haluat luoda monta tietojen tehtaan vähintään yksi putkistot, jotka toteuttavat saman logiikan, mutta tietojen kunkin factory käyttää eri Azure tallennustilan ja Azure SQL-tietokanta-tilit. Tässä skenaariossa voit samaan malliin saman ympäristön (keskihajonta, Testaa tai tuotannon) eri parametrin tiedostojen luominen tietojen tehtaan. 

## <a name="resource-manager-template-for-creating-a-gateway"></a>Luoda yhdyskäytävän Resurssienhallinta-malli
Tässä on esimerkki Resurssienhallinta mallin luoda loogisen yhdyskäytävän takana. Yhdyskäytävän asentaminen paikalliseen tietokoneeseen tai Azure IaaS AM ja rekisteröidä yhdyskäytävän avaimen avulla Data Factory-palveluun. Katso lisätietoja [paikallisen ja cloud tietojen siirtäminen](data-factory-move-data-between-onprem-and-cloud.md) .

    {
        "contentVersion": "1.0.0.0",
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "parameters": {
        },
        "variables": {
            "dataFactoryName":  "GatewayUsingArmDF",
            "apiVersion": "2015-10-01",
            "singleQuote": "'"
        },
        "resources": [
            {
                "name": "[variables('dataFactoryName')]",
                "apiVersion": "[variables('apiVersion')]",
                "type": "Microsoft.DataFactory/datafactories",
                "location": "eastus",
                "resources": [
                    {
                        "dependsOn": [ "[concat('Microsoft.DataFactory/dataFactories/', variables('dataFactoryName'))]" ],
                        "type": "gateways",
                        "apiVersion": "[variables('apiVersion')]",
                        "name": "GatewayUsingARM",
                        "properties": {
                            "description": "my gateway"
                        }
                    }            
                ]
            }
        ]
    }

Tämä malli luo nimeltä GatewayUsingArmDF nimeltä yhdyskäytävän tiedot-factory: GatewayUsingARM. 

## <a name="see-also"></a>Katso myös
| Aihe | Kuvaus |
| :---- | :---- |
| [Tietojen muunnos toiminnot](data-factory-data-transformation-activities.md) | Tässä artikkelissa on tietoja muunnos toimintoja (kuten HDInsight-rakenne-muunnos käytit Tässä opetusohjelmassa) luettelo tukemat Azure Data Factory. |
| [Ajoittamisen ja suorittaminen](data-factory-scheduling-and-execution.md) | Tässä artikkelissa kerrotaan Azure Data Factory-sovelluksen malli ajoittaminen ja suorittamisen ominaisuuksia. |
| [Putkistot](data-factory-create-pipelines.md) | Tässä artikkelissa auttaa sinua ymmärtämään putkistot ja Azure Data Factory ja kuinka niitä käytetään muodostaa skenaarion tai business loppuun tietoihin perustuvien työnkulkuja. |
| [Tietojoukkoja](data-factory-create-datasets.md) | Tässä artikkelissa auttaa sinua ymmärtämään Azure Data Factory tietojoukkoja.
| [Seurata ja hallita putkistot seuranta-sovelluksella](data-factory-monitor-manage-app.md) | Tässä artikkelissa kerrotaan, miten voit seurata ja hallita virheenkorjaus putkistot seuranta ja hallinta-sovelluksen käyttäminen. 

  

