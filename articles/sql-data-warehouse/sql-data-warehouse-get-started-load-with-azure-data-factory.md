<properties
   pageTitle="Lataa tiedot ja Azure Data Factory | Microsoft Azure"
   description="Lisätietoja Azure Data Factory tietojen lataaminen"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="twounder"
   manager="barbkess"
   editor=""
   tags="azure-sql-data-warehouse"/>
<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="10/13/2016"
   ms.author="mausher;barbkess"/>

# <a name="load-data-with-azure-data-factory"></a>Lataa tiedot ja Azure Data Factory 

> [AZURE.SELECTOR]
- [Redgate](sql-data-warehouse-load-with-redgate.md)  
- [Tietoja Factory](sql-data-warehouse-get-started-load-with-azure-data-factory.md)  
- [PolyBase](sql-data-warehouse-get-started-load-with-polybase.md)  
- [BCP](sql-data-warehouse-load-with-bcp.md)  

Tässä opetusohjelmassa näytetään, miten voit luoda putkijohto Azure Data Factory Siirry Azure Blob-objektien tallennustilaan tietojen Azure SQL-tietovarasto. Seuraavat vaiheet menettelet seuraavasti:

+ Määritä Azure-tallennustilan Blob mallitiedot.
+ Yhdistä Azure Data Factory resurssit.
+ Voit siirtää tietoja BLOB Storage SQL-tietovarasto putkijohto luominen

>[AZURE.VIDEO loading-azure-sql-data-warehouse-with-azure-data-factory]


## <a name="before-you-begin"></a>Ennen aloittamista

Tutustu Azure Data Factory-artikkelissa [Azure Data Factory esittely][].

### <a name="create-or-identify-resources"></a>Luo tai Määritä resurssit

Ennen kuin aloitat Tässä opetusohjelmassa, sinun on on seuraavissa resursseissa:

   + **Azure-tallennustilan Blob**: Tässä opetusohjelmassa käyttää Azure Blob-objektien tallennustilaan tietolähteenä Azure Data Factory putkijohto ja siten on oltava jokin käytettävissä oleva mallitiedot. Jos sitä ei ole vielä ole, katso, miten [Luo tallennustilan tili][].

   + **SQL-tietovarasto**: Tässä opetusohjelmassa siirtää tiedot Azure Blob-objektien tallennustilaan SQL-tietovarasto ja näin annettava tietojen varasto verkossa, joka on ladattu AdventureWorksDW mallitiedoilla. Jos sinulla ei ole tietovarasto, katso kuinka voit [valmistella jokin][Create a SQL Data Warehouse]. Jos olet data warehouse mutta et valmisteleminen se esimerkkitiedot, voit [ladata manuaalisesti][Load sample data into SQL Data Warehouse].

   + **Azure Data Factory**: Azure Data Factory päättää todellisen kuormituksen ja näin sinun pitää vielä ole tiliä, joiden avulla voit luoda tietojen siirtämistä putkijohto. Jos sitä ei ole vielä ole, Opi luomaan vaihe 1 [Azure Data Factory (tietojen Factory Editor) käytön aloittaminen][].

   + **AZCopy**: tarvitset AZCopy esimerkkitiedot kopioiminen Azure-tallennustilan Blob paikallisen asiakkaalle. Katso asennusohjeet, [AZCopy ohjeissa][].

## <a name="step-1-copy-sample-data-to-azure-storage-blob"></a>Vaihe 1: Kopioi esimerkkitiedot Azure Blob-objektien tallennustilaan

Kun olet valmis osat, olet valmis mallitiedot kopioitavaksi Azure-tallennustilan Blob.

1. [Lataa mallitiedot][]. Nämä tiedot Lisää toinen kolmen vuoden myyntitiedot AdventureWorksDW mallitiedot.

2. AZCopy tällä komennolla voit kopioida tietoja kolmen vuoden Azure-tallennustilan Blob.

    ````
    AzCopy /Source:<Sample Data Location>  /Dest:https://<storage account>.blob.core.windows.net/<container name> /DestKey:<storage key> /Pattern:FactInternetSales.csv
    ````


## <a name="step-2-connect-resources-to-azure-data-factory"></a>Vaihe 2: Yhdistäminen resurssien Azure Data Factory

Voit luoda tietojen siirtämiseen tietovarasto SQL Azure-blob-säiliö Azure Data Factory-myyntijakso nyt, kun tiedot on määritetty.

Aluksi [Azure portal][] Avaa ja valitse tiedot-factory vasemmanpuoleisesta valikosta.

### <a name="step-21-create-linked-service"></a>Vaihe 2.1: Luo linkitetyn palvelu

Linkki Azure-tallennustilan tilin ja SQL-tietovarasto tietojen factory.  

1. Ensin aloittaa rekisteröitymisen valitsemalla tiedot factory "Linkitetyn Services"-osa ja valitse sitten "uudet tiedot tallennetaan." Valitse nimi, voit rekisteröidä azure tallennustilan kohdassa Valitse Azure-tallennustilan tyypiksi ja kirjoita sitten tilin nimi ja tili-näppäintä.

2. Voit rekisteröidä SQL-tietovarasto siirtyä kohtaan tekijä- ja ota käyttöön, valitse uusi tietosäilö ja "Azure SQL-tietovarasto". Kopioi ja Liitä tämä malli ja täytä tietyt tiedot.

    ```JSON
    {
        "name": "<Linked Service Name>",
        "properties": {
            "description": "",
            "type": "AzureSqlDW",
            "typeProperties": {
                 "connectionString": "Data Source=tcp:<server name>.database.windows.net,1433;Initial Catalog=<server name>;Integrated Security=False;User ID=<user>@<servername>;Password=<password>;Connect Timeout=30;Encrypt=True"
             }
        }
    }
    ```

### <a name="step-22-define-the-dataset"></a>Vaihe 2.2: Määritä tietojoukko

Kun olet luonut linkitetyt palveluja, on on määritettävä tietojoukkojen.  Tämä tarkoittaa tähän tiedot, jotka siirretään-tallennustilan tietojen varastoon rakenteen määrittäminen.  Voit lukea lisää luomisesta

1. Aloita siirtymällä tietojen factory tekijä- ja ota käyttöön-osassa.

2. Valitse 'Uusi tietojoukko' ja "Azure-Blob-säiliö" Voit linkittää tiedot factory-tallennustilan.  Voit käyttää komentosarjan, joka määrittää tietojen Azure-Blob-säiliö alla:

    ```JSON
    {
        "name": "<Dataset Name>",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "<linked storage name>",
            "typeProperties": {
                "folderPath": "<containter name>",
                "fileName": "FactInternetSales.csv",
                "format": {
                "type": "TextFormat",
                "columnDelimiter": ",",
                "rowDelimiter": "\n"
                }
            },
            "external": true,
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "externalData": {
                    "retryInterval": "00:01:00",
                    "retryTimeout": "00:10:00",
                    "maximumRetry": 3
                }
            }
        }
    }
    ```

3. Nyt on määritettävä Microsoftin tietojoukko SQL-tietovarasto. Olemme Aloita samalla tavalla, napsauttamalla 'Uusi tietojoukko' ja "Azure SQL-tietovarasto".

    ```JSON
    {
        "name": "DWDataset",
        "properties": {
            "type": "AzureSqlDWTable",
            "linkedServiceName": "AzureSqlDWLinkedService",
            "typeProperties": {
                "tableName": "FactInternetSales"
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }
    ```

## <a name="step-3-create-and-run-your-pipeline"></a>Vaihe 3: Luominen ja suorittaminen oman myyntijakso

Lopuksi on määrittäminen ja suorita putkijohto Azure Data Factory.  Tämä on toiminto, joka todellisten tietojen siirto on valmis.  Voit etsiä koko näkymän toiminnot, jotka voivat tehdä SQL-tietovarasto ja Azure Data Factory [tähän][Move data to and from Azure SQL Data Warehouse using Azure Data Factory].

Valitse tekijä- ja ota käyttöön-osassa 'Lisää komentoja' ja "Uusi myyntijakso".  Kun olet luonut putkijohto, voit käyttää koodi, joka siirtää tiedot data warehouse alla:

```JSON
{
    "name": "<Pipeline Name>",
    "properties": {
        "description": "<Description>",
        "activities": [
          {
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "BlobSource",
                    "skipHeaderLineCount": 1
                },
                "sink": {
                    "type": "SqlDWSink",
                    "writeBatchSize": 0,
                    "writeBatchTimeout": "00:00:10"
                }
            },
            "inputs": [
              {
                "name": "<Storage Dataset>"
              }
            ],
            "outputs": [
              {
                "name": "<Data Warehouse Dataset>"
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
            "name": "Sample Copy",
            "description": "Copy Activity"
          }
        ],
        "start": "<Date YYYY-MM-DD>",
        "end": "<Date YYYY-MM-DD>",
        "isPaused": false
    }
}
```

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja on käynnistettävä tarkasteleminen:

- [Azure Data Factory oppimispolku][].
- [Azure SQL-tiedot varasto yhdistin][]. Tämä on ensisijainen viittaus aihe Azure Data Factory käyttöä Azure SQL-tietovarasto.


Seuraavissa artikkeleissa on tarkkoja tietoja Azure Data Factory. He keskustelevat Azure SQL-tietokanta tai HDInsight, mutta tiedot koskevat myös Azure SQL-tietovarasto.

- [Opetusohjelma: Azure Data Factory aloittaminen][] Tämä on core opetusohjelman Azure Data Factory tietojen käsittelyä varten. Tässä opetusohjelmassa muodostetaan yhteyttä ensimmäisen myyntijakso, joka käyttää HDInsight Muunna ja analysoida web-lokit kuukausittain. Huomaa, että tässä opetusohjelmassa on kopio aktiviteettia.
- [Opetusohjelma: tietojen kopioiminen Azure Blob-objektien tallennustilaan Azure SQL-tietokanta][]. Tässä opetusohjelmassa Luo myyntijakso-Azure Data Factory tietojen kopioiminen Azure Blob-objektien tallennustilaan Azure SQL-tietokantaan.

<!--Image references-->

<!--Article references-->
[AZCopy dokumentaatio]: ../storage/storage-use-azcopy.md
[Azure SQL Data Warehouse Connector]: ../data-factory/data-factory-azure-sql-data-warehouse-connector.md
[BCP]: sql-data-warehouse-load-with-bcp.md
[Create a SQL Data Warehouse]: sql-data-warehouse-get-started-provision.md
[Tallennustilan tilin luominen]: ../storage/storage-create-storage-account.md#create-a-storage-account
[Data Factory]: sql-data-warehouse-get-started-load-with-azure-data-factory.md
[Azure Data Factory (tietojen Factory Editor) käytön aloittaminen]: ../data-factory/data-factory-build-your-first-pipeline-using-editor.md
[Azure Data Factory esittely]: ../data-factory/data-factory-introduction.md
[Load sample data into SQL Data Warehouse]: sql-data-warehouse-load-sample-databases.md
[Move data to and from Azure SQL Data Warehouse using Azure Data Factory]: ../data-factory/data-factory-azure-sql-data-warehouse-connector.md
[PolyBase]: sql-data-warehouse-get-started-load-with-polybase.md
[Opetusohjelma: Tietojen kopioiminen Azure Blob-objektien tallennustilaan Azure SQL-tietokanta]: ../data-factory/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md
[Opetusohjelma: Azure Data Factory käytön aloittaminen]: ../data-factory/data-factory-build-your-first-pipeline.md

<!--MSDN references-->

<!--Other Web references-->
[Azure Data Factory oppimispolku]: https://azure.microsoft.com/documentation/learning-paths/data-factory
[Azure portal]: https://portal.azure.com
[Lataa mallitiedot]: https://migrhoststorage.blob.core.windows.net/adfsample/FactInternetSales.csv
