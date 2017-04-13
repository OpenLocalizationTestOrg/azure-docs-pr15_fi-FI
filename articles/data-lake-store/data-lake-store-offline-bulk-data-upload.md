<properties
   pageTitle="Lataa suuria tietomääriä tietojen järvi säilöön offline-tilassa menetelmillä | Microsoft Azure"
   description="Tietojen kopioiminen Azure-tallennustilan BLOB järvi tietovaraston AdlCopy-työkalun avulla"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/07/2016"
   ms.author="nitinme"/>

# <a name="use-azure-import-export-service-for-offline-copy-of-data-to-data-lake-store"></a>Azure Tuo Vie-palvelun käyttäminen offline-kopio tietojen järvi tietosäilö

Tässä artikkelissa lisätietoja kopioimisesta tietojoukkoja erittäin suuri (> 200 gt) Azure tietojen järvi säilöön offline-kopio tavoilla, kuten [Azure Tuo/Vie-palvelu](../storage/storage-import-export-service.md). Tarkemmin sanottuna tiedostoa, jota käytetään esimerkkinä tässä artikkelissa on 339,420,860,416 tavua eli noin 319 gt levylle. Soita oletetaan, että tämä tiedosto 319GB.tsv.

Azure Tuo/Vie-palvelun avulla voit siirtää suojatusti suuria tietomääriä Azure-blob-säiliö toimitus kiintolevyaseman levyasemien Azure tietojen käyttöoikeudet.

## <a name="prerequisites"></a>Edellytykset

Ennen kuin aloitat tämän artikkelin, sinulla on oltava seuraavasti:

- **Azure-tilaus**. Katso [Hae Azure maksuttoman kokeiluversion](https://azure.microsoft.com/pricing/free-trial/).

- **Azure-tallennustilan tilin**.

- **Azure tietojen järvi Analytics-tili (valinnainen)** – Katso [aloittaminen Azure tietojen järvi Analytics](../data-lake-analytics/data-lake-analytics-get-started-portal.md) ohjeet järvi tietosäilö-tilin luominen.


## <a name="preparing-the-data"></a>Tietojen valmisteleminen

Ennen kuin käytät Tuo/Vie-palvelua, emme katkaista datatiedosto on siirretty **kyselyjä, jotka ovat vähemmän kuin 200 gt kopiot** kokoinen. Tämä johtuu siitä tuontityökalu ei toimi tiedostojen yli 200 gt: N kanssa. Noudata tämän Tässä opetusohjelmassa on jaettu tiedosto joukkojen tavuina 100 Gigatavua. Voit tehdä helposti käyttämällä [Cygwin](https://cygwin.com/install.html). Cygwin tukee Linux-komento, jonka avulla käyttäjät voivat tehdä tämän helposti. Tässä tapauksessa emme Käytä seuraavaa komentoa.

    split -b 100m 319GB.tsv

Jaa-toiminto luo tiedostot nimillä.

    319GB.tsv-part-aa

    319GB.tsv-part-ab

    319GB.tsv-part-ac

    319GB.tsv-part-ad

## <a name="get-disks-ready-with-data"></a>Käyttäjien levyjen tiedoilla

Noudata osoitteessa [Azure Tuo/Vie-palvelun avulla](../storage/storage-import-export-service.md) ( **valmisteleminen asemat** -osa) valmisteleminen oman kiintolevyillä. Seuraavassa on yleinen kulku valmistelemisesta asemaa.

1. Hankitaan kiintolevy, joka täyttää vaatimukset, jota käytetään Auzre Tuo/Vie palvelu.

2. Määritä Azure-tallennustilan tilin mistä tiedot on kopioitu kerran palvelun käyttäjätietojen Azure tietokeskuksen on toimitettu.

3. Käytä [Azure Tuo/Vie työkalun](http://go.microsoft.com/fwlink/?LinkID=301900&clcid=0x409)komentoriviltä apuohjelma. Seuraavassa on esimerkki koodikatkelman työkalun käyttämisestä.

    ````
    WAImportExport PrepImport /sk:<StorageAccountKey> /t: <TargetDriveLetter> /format /encrypt /logdir:e:\myexportimportjob\logdir /j:e:\myexportimportjob\journal1.jrn /id:myexportimportjob /srcdir:F:\demo\ExImContainer /dstdir:importcontainer/vf1/
    ````
    Saat enemmän otoksen katkelmat **Azure Tuo/Vie-työkalun**käyttämisestä [Azure Tuo/Vie-palvelun avulla](../storage/storage-import-export-service.md) .

4. Yllä olevan komennon Luo Päivyrin tiedoston määritetyssä sijainnissa. Luo tuontityö [Azure perinteinen Portal](https://manage.windowsazure.com)käytetään Päivyri tämän tiedoston uudelleen.

## <a name="create-an-import-job"></a>Tuo projektin luominen

Voit nyt luoda tuontityö, käyttämällä [Azure Tuo/Vie-palvelun avulla](../storage/storage-import-export-service.md) ohjeita (kohdassa **luo tuontityön** ). Tuo työlle ja muut tiedot myös antaa luomasi valmisteltaessa levyasemien Päivyri-tiedosto.

## <a name="physically-ship-the-disks"></a>Toimita fyysisesti levyjen

Toimita nyt Azure tietokeskuksen, jossa tiedot kopioidaan päälle Azure-tallennustilan BLOB antamasi luotaessa Tuontityön levyjen fyysisesti. Myös luotaessa työ, jos olet valinnut antamaan seurantatiedot myöhemmin, voit nyt siirtyä takaisin tuo työtäsi ja päivittää seurantanumeron.

## <a name="copy-data-from-azure-storage-blobs-to-azure-data-lake-store"></a>Tietojen kopioiminen Azure-tallennustilan BLOB Azure järvi tietosäilö

Kun tuonti työn tilana näkyy valmis, voit tarkistaa, onko tiedot ovat käytettävissä Azure-tallennustilan BLOB oli määritetty. Voit sitten tiedot Siirry Azure järvi tietovaraston tallennustilan Azure-BLOB usealla eri tavalla. Saat kaikki käytettävissä olevat vaihtoehdot tietojen lataaminen, [Ingesting tietojen järvi säilöön](data-lake-store-data-scenarios.md#ingest-data-into-data-lake-store).

Tässä osassa on antaa sinulle JSON määritelmiä, joiden avulla voit luoda Azure Data Factory-myyntijakso tietoja voi kopioida. Voit käyttää näitä [Azure portal](../data-factory/data-factory-copy-activity-tutorial-using-azure-portal.md) tai [Visual Studio](../data-factory/data-factory-copy-activity-tutorial-using-visual-studio.md) tai [PowerShellin Azure](../data-factory/data-factory-copy-activity-tutorial-using-powershell.md)JSON-määrityksiä.

### <a name="source-linked-service-azure-storage-blob"></a>Tietolähteen linkitetty Service (tallennustilan Azure-Blob)

````
{
    "name": "AzureStorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "description": "",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
    }
}
````

### <a name="target-linked-service-azure-data-lake-store"></a>Linkitetty kohde Service (Azure Lake Tietosäilölle)

````
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "description": "",
        "typeProperties": {
            "authorization": "<Click 'Authorize' to allow this data factory and the activities it runs to access this Data Lake Store with your access rights>",
            "dataLakeStoreUri": "https://<adls_account_name>.azuredatalakestore.net/webhdfs/v1",
            "sessionId": "<OAuth session id from the OAuth authorization session. Each session id is unique and may only be used once>"
        }
    }
}
````
### <a name="input-data-set"></a>Kirjoita tietojoukon
````
{
    "name": "InputDataSet",
    "properties": {
        "published": false,
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "importcontainer/vf1/"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {}
    }
}
````
### <a name="output-data-set"></a>Tulostus-tietojoukko
````
{
"name": "OutputDataSet",
"properties": {
  "published": false,
  "type": "AzureDataLakeStore",
  "linkedServiceName": "AzureDataLakeStoreLinkedService",
  "typeProperties": {
    "folderPath": "/importeddatafeb8job/"
    },
  "availability": {
    "frequency": "Hour",
    "interval": 1
    }
  }
}
````
### <a name="pipeline-copy-activity"></a>Myyntijakso (kopioi tehtävä)
````
{
    "name": "CopyImportedData",
    "properties": {
        "description": "Pipeline with copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "sink": {
                        "type": "AzureDataLakeStoreSink",
                        "copyBehavior": "PreserveHierarchy",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "InputDataSet"
                    }
                ],
                "outputs": [
                    {
                        "name": "OutputDataSet"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "AzureBlobtoDataLake",
                "description": "Copy Activity"
            }
        ],
        "start": "2016-02-08T22:00:00Z",
        "end": "2016-02-08T23:00:00Z",
        "isPaused": false,
        "pipelineMode": "Scheduled"
    }
}
````
Lisätietoja tietojen siirtäminen Azure Blob-objektien tallennustilaan ja Azure järvi tietovaraston Azure Data Factory avulla on artikkelissa [Siirry Azure tallennustilan Blob-objektien käyttäminen Azure Data Factory Azure tietojen järvi Store tiedot](../data-factory/data-factory-azure-datalake-connector.md#sample-copy-data-from-azure-blob-to-azure-data-lake-store).

## <a name="reconstruct-the-data-files-in-azure-data-lake-store"></a>Uudistaa Azure järvi tietovaraston datatiedostot

Olemme aloittaminen tiedostolle, joka oli 319 gt kokoisia ja rikki sitä pienemmäksi tiedostoihin niin, että voidaan siirtää Azure Tuo/Vie-palvelun avulla. Nyt, kun tiedot on Azure järvi tietosäilö, emme voi reconstrcut alkuperäiseen kokoonsa napsauttamalla tiedostoa. Voit käyttää seuraavia PowerShellin Azure cmldts tekemään niin.

````
# Login to our account
Login-AzureRmAccount

# List your subscriptions
Get-AzureRmSubscription

# Switch to the subscription you want to work with
Set-AzureRmContext –SubscriptionId
Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.DataLakeStore"

# Join  the files
Join-AzureRmDataLakeStoreItem -AccountName "<adls_account_name" -Paths "/importeddatafeb8job/319GB.tsv-part-aa","/importeddatafeb8job/319GB.tsv-part-ab", "/importeddatafeb8job/319GB.tsv-part-ac", "/importeddatafeb8job/319GB.tsv-part-ad" -Destination "/importeddatafeb8job/MergedFile.csv”
````

## <a name="next-steps"></a>Seuraavat vaiheet

- [Tietojen suojaaminen järvi säilössä](data-lake-store-secure-data.md)
- [Azure tietojen järvi Analytics käyttäminen järvi tietosäilö](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Azure Hdinsightiin käyttäminen järvi tietosäilö](data-lake-store-hdinsight-hadoop-use-portal.md)
