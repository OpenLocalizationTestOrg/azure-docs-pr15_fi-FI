<properties 
    pageTitle="Resurssien hallinnan käyttö mallit Data Factory | Microsoft Azure" 
    description="Opi luomaan ja Azure Resurssienhallinta mallien avulla voit luoda Data Factory kohteita." 
    services="data-factory" 
    documentationCenter="" 
    authors="sharonlo101" 
    manager="jhubbard" 
    editor=""/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/24/2016" 
    ms.author="shlo"/>

# <a name="use-templates-to-create-azure-data-factory-entities"></a>Voit luoda Azure Data Factory kohteiden mallien avulla

## <a name="overview"></a>Yleiskatsaus
Tietojen integrointi käyttötarkoitukseen Azure Data Factory saattaa olla itse ilmenevän vian uudelleen samoissa eri ympäristöissä tai käyttöönoton saman tehtävä repetitively saman ratkaisun yli. Mallien avulla voit toteuttaa ja hallita tilanteista helposti. Azure Data Factory mallit soveltuvat erinomaisesti tilanteita, joissa hyötykäyttöä ja toiston.
 
Harkitse tilanteeseen, jossa organisaatiossa on 10 tuotantolaitosta kaikkialla maailmassa. Lokit-kustakin on tallennettu eri paikallisen SQL Server-tietokantaan. Yrityksen haluaa muodostaa yhden tietovarasto itsenäisen analytics Pilvipalvelun. Se myös haluaa saman logiikan, mutta eri käyttömahdollisuudet kehitystä, Testaa ja tuotannon ympäristössä. 

Tässä tapauksessa tehtävän on toistettava saman ympäristössä, mutta eri arvoilla yli 10 tietojen tehtaan kunkin tehdas varten. **Toiston** ei sisällä tietoja. Templating tätä yleisen (putkistot, ottaa saman toimintojen tietoja kunkin factory) työnkulku otetaan avulla, mutta käyttää erillisen parametrin tiedoston kunkin tehdas.

Lisäksi kuin organisaation haluaa ottaa käyttöön useita kertoja eri ympäristöissä eri nämä 10 tietojen tehtaan, malleja voi käyttää tätä **hyötykäyttöä** välityksellä erillisen parametritiedostot kehitystä, Testaa ja tuotannon ympäristössä.

## <a name="templating-with-azure-resource-manager"></a>Templating kanssa Azure resurssien hallinta
[Azure Resurssienhallinta mallit](../azure-resource-manager/resource-group-overview.md#template-deployment) ovat erinomainen tapa päästä Azure Data Factory templating. Resurssienhallinta mallit Määritä julkaisuinfrastruktuuri ja Azure-ratkaisun määrittäminen JSON-tiedoston avulla. Koska kaikki/useimmat Azure palveluiden käsitteleminen Azure Resurssienhallinta mallit, se laajasti voidaan hallita helposti kaikkien resurssien Azure kohteita. Saat lisätietoja Resurssienhallinta-mallien yleinen [yhtä aikaa muiden kanssa Azure Resurssienhallinta mallit](../resource-group-authoring-templates.md) näkyviin. 

## <a name="tutorials"></a>Opetusohjelmat
Katso vaiheittaiset ohjeet tietojen Factory kohteiden luominen Resurssienhallinta mallien avulla opetusohjelmassa:

- [Opetusohjelma: Luo myyntijakso, jos haluat kopioida tiedot Azure Resurssienhallinta-mallin avulla](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
- [Opetusohjelma: Luo myyntijakso prosessin tietoihin Azure Resurssienhallinta-mallin avulla](data-factory-build-your-first-pipeline.md)

## <a name="data-factory-templates-on-github"></a>Tietomallit Factory-Github
Tutustu Github seuraavat Azure pika-aloitusopas-mallit: 

- [Tiedot-factory tietojen kopioiminen Azure-Blob-säiliö Azure SQL-tietokannan luominen](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-blob-to-sql-copy)
- [Luo Data factory jossa Azure HDInsight-klusterissa aktiviteetit rakenne](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-hive-transformation)
- [Luo Data factory, tietojen kopioiminen Salesforce Azure-BLOB-objektit](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-salesforce-to-blob-copy)

Vapaasti jakaminen Azure Data Factory malleja osoitteessa [Azure Pika-aloitus](https://azure.microsoft.com/documentation/templates/). Viittaavat [osuus apuviiva](https://github.com/Azure/azure-quickstart-templates/tree/master/1-CONTRIBUTION-GUIDE) ja kehittää malleilla, jotka voidaan jakaa tätä säilöä kautta. 

Seuraavissa osissa on tietoja määrittämällä Data Factory resurssien Resurssienhallinta-malli. 

## <a name="defining-data-factory-resources-in-templates"></a>Mallien määrittäminen Data Factory-resurssit
Ylimmän tason mallin määrittäminen tietojen factory on:

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
        { "type": "linkedservices",
            ...
        },
        {"type": "datasets",
            ...
        },
        {"type": "dataPipelines",
            ...
        }
    }

### <a name="define-data-factory"></a>Määritä tiedot factory

Määritä tiedot-factory Resurssienhallinta mallissa seuraava näyte esitetyllä tavalla:

    "resources": [
    {
        "name": "[variables('<mydataFactoryName>')]",
        "apiVersion": "2015-10-01",
        "type": "Microsoft.DataFactory/datafactories",
        "location": "East US"
    }

DataFactoryName määritetään "muuttujat" seuraavasti:

    "dataFactoryName": "[concat('<myDataFactoryName>', uniqueString(resourceGroup().id))]",

### <a name="define-linked-services"></a>Linkitetyn palveluiden määrittäminen 
    
    "type": "linkedservices",
    "name": "[variables('<LinkedServiceName>')]",
    "apiVersion": "2015-10-01",
    "dependsOn": [ "[variables('<dataFactoryName>')]" ],
    "properties": {
        ...
    }


Saat tietoja haluat ottaa linkitetyn palvelun JSON ominaisuuksien [Linkitetyn Tallennuspalvelu](data-factory-azure-blob-connector.md#azure-storage-linked-service) tai [Laskea linkitetyn palvelut](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) . "DependsOn"-parametri ilmaisee vastaavien tietojen factory nimi. Esimerkki määrittäminen Azuren tallennustilaan linkitetyn palvelun näkyy seuraavan JSON määritelmän:

### <a name="define-datasets"></a>Määritä tietojoukkoja

    "type": "datasets",
    "name": "[variables('<myDatasetName>')]",
    "dependsOn": [
        "[variables('<dataFactoryName>')]",
        "[variables('<myDatasetLinkedServiceName>')]"
    ],
    "apiVersion": "2015-10-01",
    "properties": {
        ...
    }

Viittaavat [tuettu Microsoft Data](data-factory-data-movement-activities.md#supported-data-stores-and-formats) lisätietoja JSON ominaisuuksia haluat ottaa käyttöön tiettyjä tietojoukko tyyppi. Huomautus "dependsOn"-parametri ilmaisee nimi vastaavien tietojen factory ja tallennustilaa linkitetty palvelu. Esimerkki määrittäminen Azure-blob-säiliö tietojoukko tyyppi näkyy seuraavan JSON määritelmän:

    "type": "datasets",
    "name": "[variables('storageDataset')]",
    "dependsOn": [
        "[variables('dataFactoryName')]",
        "[variables('storageLinkedServiceName')]"
    ],
    "apiVersion": "2015-10-01",
    "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "[variables('storageLinkedServiceName')]",
    "typeProperties": {
        "folderPath": "[concat(parameters('sourceBlobContainer'), '/')]",
        "fileName": "[parameters('sourceBlobName')]",
        "format": {
            "type": "TextFormat"
        }
    },
    "availability": {
        "frequency": "Hour",
        "interval": 1
    }

### <a name="define-pipelines"></a>Määritä putkistot

    "type": "dataPipelines",
    "name": "[variables('<mypipelineName>')]",
    "dependsOn": [
        "[variables('<dataFactoryName>')]",
        "[variables('<inputDatasetLinkedServiceName>')]",
        "[variables('<outputDatasetLinkedServiceName>')]",
        "[variables('<inputDataset>')]",
        "[variables('<outputDataset>')]"
    ],
    "apiVersion": "2015-10-01",
    "properties": {
        activities: {
            ...
        }
    }

Katso lisätietoja tietyn putkijohto ja toiminnot, jotka haluat ottaa JSON ominaisuuksien [määrittäminen putkistot](data-factory-create-pipelines.md#pipeline-json) . Huomautus "dependsOn"-parametri ilmaisee tietojen factory nimi ja kaikki vastaavat linkitetty services tai tietojoukkoja. Esimerkki myyntijakso, kopioi tiedot Azure-Blob-säiliö Azure SQL-tietokanta näkyy seuraavat JSON koodikatkelman:

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
        "start": "2016-10-03T00:00:00Z",
        "end": "2016-10-04T00:00:00Z"

## <a name="parameterizing-data-factory-template"></a>Parametrointi Data Factory-malli
Valitse parametrointi, katso [parhaiden käytäntöjen Azure Resurssienhallinta-mallien luomiseen](../resource-manager-template-best-practices.md#parameters) artikkelissa parhaista käytännöistä. Yleensä parametrien käyttö olisi pienentää, etenkin jos muuttujaa voidaan käyttää sen sijaan. Anna vain parametrit seuraavissa tilanteissa:

- Asetukset vaihtelevat mukaan ympäristössä (Esimerkki: kehitystä, Testaa ja tuotannon)
- Tietoja (esimerkiksi salasanat)

Jos haluat tuoda tietoja [Azure avain](../key-vault/key-vault-get-started.md) säilöstä otettaessa Azure Data Factory kohteiden mallien avulla, Määritä **avaimen säilö** ja **salainen nimi** seuraavan esimerkin mukaisesti:

    "parameters": {
        "storageAccountKey": { 
            "reference": {
                "keyVault": {
                    "id":"/subscriptions/<subscriptionID>/resourceGroups/<resourceGroupName>/providers/Microsoft.KeyVault/vaults/<keyVaultName>",
                },
                "secretName": "<secretName>"
            }, 
        },
        ...
    }

> [AZURE.NOTE] Aiemmin luotuja tietoja tehtaan mallien vieminen tällä hetkellä ei vielä tueta, mutta on Works. 


