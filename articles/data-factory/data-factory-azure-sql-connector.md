<properties 
    pageTitle="Tietojen siirtäminen tai Azure SQL-tietokanta | Microsoft Azure" 
    description="Opettele siirtämään tiedot ja Azure SQL-tietokanta Azure Data Factory." 
    services="data-factory" 
    documentationCenter="" 
    authors="linda33wj" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/20/2016" 
    ms.author="jingwang"/>

# <a name="move-data-to-and-from-azure-sql-database-using-azure-data-factory"></a>Tietojen siirtäminen ja sieltä pois Azure SQL-tietokanta Azure Data Factory
Tässä artikkelissa käsitellään käyttämisestä kopioi tehtävän Azure tietojen factory-Siirrä tiedot ja Azure SQL-tietokanta / toisen tietosäilö. Tässä artikkelissa perustuu [tietojen siirtämistä tehtävät](data-factory-data-movement-activities.md) -artikkelista, jossa näkyy yleiskatsaus tietojen siirto kopioi tehtävän ja tuettuja kaupan yhdistelmät. 

## <a name="supported-sources-and-sinks"></a>Tuetut tietolähteet ja poistumia
Katso [Tuetut tietojen stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) taulukon tiedot tallentaa kopion tehtävän lähteitä tai poistumia tukemat luettelo. Voit siirtää tietoja kaikki tuetut lähde-tietosäilö Azure SQL-tietokantaan tai Azure SQL-tietokanta kaikki tuetut käsittelytoiminto tietosäilölle.

## <a name="create-pipeline"></a>Myyntijakso luominen
Voit luoda putkijohto kopio-toiminto, joka siirtää ja Azure SQL-tietokannan tietojen käyttämällä eri Työkalut/ohjelmointirajapinnan kanssa.  

- Ohjattu kopiointi
- Azure portal
- Visual Studio
- Azure PowerShell
- .NET-OHJELMOINTIRAJAPINTA
- REST-OHJELMOINTIRAJAPINNALLA

[Kopioi tehtävän opetusohjelma](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) Saat vaiheittaiset ohjeet luominen putkijohto kopioi aktiviteettiin eri tavoin.   

## <a name="copy-data-wizard"></a>Ohjattu tietojen kopioiminen
Helpoin tapa luoda myyntijakso, kopioi tiedot ja Azure SQL-tietokanta on käyttämällä ohjattua Kopioi tiedot. Katso [Opetusohjelma: Luo kopio ohjatulla putkijohto](data-factory-copy-data-wizard-tutorial.md) nopeasti käyttämällä ohjattua tietojen kopioiminen putkijohto luomisesta saat. 

Seuraavissa esimerkeissä otoksen JSON määritykset, jonka avulla voit luoda putkijohto [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) tai [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) tai [PowerShellin Azure](data-factory-copy-activity-tutorial-using-powershell.md). Ne näyttävät tietojen kopioimisesta ja sieltä pois Azure SQL-tietokanta ja Azure-Blob-objektien tallennustilaan. Tietoja voi kuitenkin kopioidun **suoraan** mistä tahansa lähteestä johonkin seuraavista poistumia määritetyllä tavalla [tähän](data-factory-data-movement-activities.md#supported-data-stores) kopio-toimintojen käyttäminen Azure Data Factory.

## <a name="sample-copy-data-from-azure-sql-database-to-azure-blob"></a>Esimerkki: Kopioi tiedot Azure-Blob Azure SQL-tietokanta

Sama määrittää Data Factory seuraavia kohteita:

1. Linkitetyn palvelu [AzureSqlDatabase](#azure-sql-linked-service-properties)tyyppi.
2. Linkitetyn palvelu [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties)tyyppi. 
3. Syötteen [tietojoukko](data-factory-create-datasets.md) tyypin [AzureSqlTable](#azure-sql-dataset-type-properties). 
4. Tulosteen [tietojoukko](data-factory-create-datasets.md) tyypin [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4. [Myyntijakso](data-factory-create-pipelines.md) kopioi aktiviteettiin, joka käyttää [SqlSource](#azure-sql-copy-activity-type-properties) ja [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties).

Otosten kopioi aikasarjalle tiedot (hourly, päivittäin, jne.) Azure SQL-tietokannan taulukkoon blob tunnissa. Käyttää näitä esimerkkejä JSON-ominaisuuksia on kuvattu osia seuraavat mallit.  

**Azure linkitetty SQL-palvelu**

    {
      "name": "AzureSqlLinkedService",
      "properties": {
        "type": "AzureSqlDatabase",
        "typeProperties": {
          "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
        }
      }
    }

On linkitetty tämän palvelun tukemat ominaisuudet luettelon [Linkitetyn Azure SQL-palvelu](#azure-sql-linked-service-properties) -osassa. 

**Azure Blob storage linkitetty-palvelu**

    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

On artikkelissa [Azure-Blob-objektien](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties) luettelon linkitetyn palvelun tukemat ominaisuudet. 

**Azure SQL-syötteen tietojoukko**

Otosten oletetaan, että olet luonut taulukon "MyTable" Azure SQL- ja se sisältää sarake nimeltä "timestampcolumn" sarjan Aikatietojen. 

Asetus "ulkoinen": "true" ilmoittaa Azure Data Factory-palvelun dataset on ulkoiset tiedot-factory ja tietojen factory toimintaa ei tuottamat.

    {
      "name": "AzureSqlInput",
      "properties": {
        "type": "AzureSqlTable",
        "linkedServiceName": "AzureSqlLinkedService",
        "typeProperties": {
          "tableName": "MyTable"
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

Kohdassa [Azure SQL-tietojoukko tyypin ominaisuudet](#azure-sql-dataset-type-properties) luettelon tietojoukko tällaista tukemat ominaisuudet.  

**Azure-Blob tulosteen tietojoukko**

Tietoja kirjoitetaan uusia Blob-objektien tunnissa (taajuus: tunti, väli: 1). Kansiopolun blob arvioidaan dynaamisesti sektoria, käsitellään alkamispäivää perusteella. Kansiopolku käytetään vuoden, kuukauden, päivän ja alkamisajan tuntia osat. 

    {
      "name": "AzureBlobOutput",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}/",
          "partitionedBy": [
            {
              "name": "Year",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "yyyy"
              }
            },
            {
              "name": "Month",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "MM"
              }
            },
            {
              "name": "Day",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "dd"
              }
            },
            {
              "name": "Hour",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "HH"
              }
            }
          ],
          "format": {
            "type": "TextFormat",
            "columnDelimiter": "\t",
            "rowDelimiter": "\n"
          }
        },
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }

Kohdassa [Azure-Blob-objektien tietojoukkoa ominaisuudet](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties) luettelon tietojoukko tällaista tukemat ominaisuudet.  

**Kopioi aktiviteettiin putkijohto**

Putkisto sisältää kopio-toiminto, joka on määritetty käyttämään syöttö- ja tietojoukkoja ja on suunniteltu toimimaan tunnissa. Myyntijakso JSON määritelmä- **lähteen** tyyppi on määritetty **SqlSource** ja **käsittelytoiminto** tyyppi-asetukseksi on määritetty **BlobSink**. SQL-kyselyn **SqlReaderQuery** -ominaisuuden valitsee edellisen tunnin kopioitavat tiedot.

    {  
        "name":"SamplePipeline",
        "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline for copy activity",
        "activities":[  
          {
            "name": "AzureSQLtoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [
              {
                "name": "AzureSQLInput"
              }
            ],
            "outputs": [
              {
                "name": "AzureBlobOutput"
              }
            ],
            "typeProperties": {
              "source": {
                "type": "SqlSource",
                "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
              },
              "sink": {
                "type": "BlobSink"
              }
            },
           "scheduler": {
              "frequency": "Hour",
              "interval": 1
            },
            "policy": {
              "concurrency": 1,
              "executionPriorityOrder": "OldestFirst",
              "retry": 0,
              "timeout": "01:00:00"
            }
          }
         ]
       }
    }

Esimerkissä **sqlReaderQuery** on määritetty SqlSource. Kopioi tehtävän suorittaa kysely vastaan Azure SQL-tietokannan lähde, saat tiedot. Voit myös määrittää tallennetun toimintosarjan määrittämällä **sqlReaderStoredProcedureName** ja **storedProcedureParameters** (Jos tallennettu toimintosarja käyttää parametreja).

Jos et määritä sqlReaderQuery tai sqlReaderStoredProcedureName-tietojoukko JSON rakenne-osassa määritettyjä sarakkeita käytetään kyselyn suorittamalla Azure SQL-tietokanta. Esimerkki: `select column1, column2 from mytable`. Jos tietojoukko määritystä ei ole rakenteen, kaikki sarakkeet valitaan taulukosta. 


Katso [Sql-lähde](#sqlsource) -osio ja [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) SqlSource ja BlobSink tukemat ominaisuudet luettelo. 


## <a name="sample-copy-data-from-azure-blob-to-azure-sql-database"></a>Esimerkki: Kopioi tiedot Azure-Blob Azure SQL-tietokantaan

Otosten määrittää Data Factory seuraavia kohteita:  

1.  Linkitetyn palvelu [AzureSqlDatabase](data-factory-azure-sql-connector.md#azure-sql-linked-service-properties)tyyppi.
2.  Linkitetyn palvelu [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties)tyyppi.
3.  Syötteen [tietojoukko](data-factory-create-datasets.md) tyypin [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4.  Tulosteen [tietojoukko](data-factory-create-datasets.md) tyypin [AzureSqlTable](data-factory-azure-sql-connector.md#azure-sql-dataset-type-properties).
4.  [Myyntijakso](data-factory-create-pipelines.md) kopioi aktiviteettiin, joka käyttää [BlobSource](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) ja [SqlSink](data-factory-azure-sql-connector.md#azure-sql-copy-activity-type-properties).

Esimerkki kopiot aikasarjalle tiedot (hourly, päivittäin, jne.) azuren blob-taulukkoon Azure SQL-tietokannan tunnissa. Käyttää näitä esimerkkejä JSON-ominaisuuksia on kuvattu osia seuraavat mallit. 


**Azure linkitetty SQL-palvelu**
    
    {
      "name": "AzureSqlLinkedService",
      "properties": {
        "type": "AzureSqlDatabase",
        "typeProperties": {
          "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
        }
      }
    }

On linkitetty tämän palvelun tukemat ominaisuudet luettelon [Linkitetyn Azure SQL-palvelu](#azure-sql-linked-service-properties) -osassa. 

**Azure Blob storage linkitetty-palvelu**

    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

On artikkelissa [Azure-Blob-objektien](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties) luettelon linkitetyn palvelun tukemat ominaisuudet.

**Azure Blob syötteen tietojoukko**

Tiedot on noudettu uusi blob-tunnissa (taajuus: tunti, väli: 1). Blob kansion polku ja tiedostonimi dynaamisesti arvioidaan, käsitellään sektoria alkamispäivää perusteella. Kansiopolku käyttävät vuoden, kuukauden ja päivän aloitusaika-osa ja tiedostonimi alkamisajan tunti-osa. "ulkoinen": "true" asetus ilmoittaa Data Factory-palvelun tässä taulukossa on ulkoiset tiedot-factory ja tietojen factory toimintaa ei tuottamat.

    {
      "name": "AzureBlobInput",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/",
          "fileName": "{Hour}.csv",
          "partitionedBy": [
            {
              "name": "Year",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "yyyy"
              }
            },
            {
              "name": "Month",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "MM"
              }
            },
            {
              "name": "Day",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "dd"
              }
            },
            {
              "name": "Hour",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "HH"
              }
            }
          ],
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

Kohdassa [Azure-Blob-objektien tietojoukkoa ominaisuudet](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties) luettelon tietojoukko tällaista tukemat ominaisuudet.

**Azure SQL-tulostus-tietojoukko**

Otosten kopioi "MyTable" Azure SQL-taulukon tietoja. Luo taulukon sama määrä sarakkeita SQL Azure Blob CSV-tiedosto sisältää haluamallasi tavalla. Uusien rivien lisätään taulukon tunnissa. 

    {
      "name": "AzureSqlOutput",
      "properties": {
        "type": "AzureSqlTable",
        "linkedServiceName": "AzureSqlLinkedService",
        "typeProperties": {
          "tableName": "MyOutputTable"
        },
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }

Kohdassa [Azure SQL-tietojoukko tyypin ominaisuudet](#azure-sql-dataset-type-properties) luettelon tietojoukko tällaista tukemat ominaisuudet.

**Kopioi aktiviteettiin putkijohto**

Putkisto sisältää kopio-toiminto, joka on määritetty käyttämään syöttö- ja tietojoukkoja ja on suunniteltu toimimaan tunnissa. Myyntijakso JSON määritelmä- **lähteen** tyyppi on määritetty **BlobSource** ja **käsittelytoiminto** tyyppi-asetukseksi on määritetty **SqlSink**.

    {  
        "name":"SamplePipeline",
        "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline with copy activity",
        "activities":[  
          {
            "name": "AzureBlobtoSQL",
            "description": "Copy Activity",
            "type": "Copy",
            "inputs": [
              {
                "name": "AzureBlobInput"
              }
            ],
            "outputs": [
              {
                "name": "AzureSqlOutput"
              }
            ],
            "typeProperties": {
              "source": {
                "type": "BlobSource",
                "blobColumnSeparators": ","
              },
              "sink": {
                "type": "SqlSink"
              }
            },
           "scheduler": {
              "frequency": "Hour",
              "interval": 1
            },
            "policy": {
              "concurrency": 1,
              "executionPriorityOrder": "OldestFirst",
              "retry": 0,
              "timeout": "01:00:00"
            }
          }
          ]
       }
    }

Katso [BlobSource](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) ja [Sql-käsittelytoiminto](#sqlsink) osion tukemien SqlSink ja BlobSource luettelo. 


## <a name="azure-sql-linked-service-properties"></a>Azure linkitetty SQL-palveluominaisuudet
-Mallit ovat käyttäneet linkitetyn palvelun tyypin **AzureSqlDatabase** voit linkittää tiedot factory Azure SQL-tietokantaan. Seuraavassa taulukossa on kuvaus JSON elementtien tietyn linkitetty Azure SQL-palveluun.

| Ominaisuus | Kuvaus | Pakollinen |
| -------- | ----------- | -------- |
| tyyppi | Type-ominaisuus on määritettävä: **AzureSqlDatabase** | Kyllä |
| connectionString | Määritä tiedot, joita tarvitaan connectionString-ominaisuuden Azure SQL-tietokanta-esiintymään. | Kyllä |

> [AZURE.NOTE] Määritä [Azure SQL-tietokantaan palomuurin](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure) sallimaan [Azure Services-palvelimen käyttämiseen](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure)tietokantapalvelimen. Lisäksi jos Azure SQL-tietokantaan tietojen kopioiminen ovat ulkopuolelta Azure myös tietojen factory Gatewayn paikallisen tietolähteistä, määritä oikea IP-osoitealueita koneen, joka lähettää tietoja Azure SQL-tietokantaan. 

## <a name="azure-sql-dataset-type-properties"></a>Azure SQL-tietojoukko tyypin ominaisuudet
-Mallit ovat käyttäneet tietojoukko tyypin **AzureSqlTable** edustavan Azure SQL-tietokannan taulukkoon. 

Täydellinen luettelo osien ja ominaisuudet, jotka ovat käytettävissä määrittäminen tietojoukkoja [luominen tietojoukkoja](data-factory-create-datasets.md) on artikkelissa. Osia, kuten rakenne, käytettävyys ja tietojoukko JSON käytännön muistuttavat tyypeissä tietojoukko (Azure SQL Azure-blob, Azuren taulukkojen jne.). 

TypeProperties-osassa on eri mistäkin tietojoukko, ja se antaa tietoja tietojen tietovaraston sijainti. Tyypin **AzureSqlTable** tietojoukon **typeProperties** osiossa on seuraavat ominaisuudet:

| Ominaisuus | Kuvaus | Pakollinen |
| -------- | ----------- | -------- |
| taulukon nimi | Taulukko, joka viittaa linkitetyistä Azure SQL-tietokanta-esiintymän nimi. | Kyllä |

## <a name="azure-sql-copy-activity-type-properties"></a>Azure SQL kopioi tehtävän tyypin ominaisuudet
Täydellinen luettelo osat ja ominaisuudet, jotka ovat käytettävissä toiminnan määrittäminen [Putkistot luomisesta](data-factory-create-pipelines.md) on artikkelissa. Ominaisuudet, kuten nimi, kuvaus sekä syöttö- ja taulukot ja käytäntö ovat käytettävissä kaikki tehtävälajit.

> [AZURE.NOTE] Kopioi tehtävän tulee vain yhden syötteen ja tuottaa vain yksi tulos.

Tehtävän **typeProperties** -osassa käytettävissä olevat ominaisuudet vaihtelevat toisaalta kunkin tehtävätyyppi. Kopioi tehtävälle ne vaihtelevat sen mukaan, lähteiden ja poistumia. 

Jos siirrät tiedot Azure SQL-tietokantaan, voit määrittää **SqlSource**kopioi tehtävä lähteen tyyppi. Vastaavasti, jos tiedot ovat Siirry Azure SQL-tietokantaan ja määrität käsittelytoiminto tyyppi **SqlSink**kopioi tehtävä. Tässä osassa on luettelo SqlSource ja SqlSink tukemat ominaisuudet. 

### <a name="sqlsource"></a>SqlSource

Kopioi toiminnossa lähteen tyyppi **SqlSource**on seuraavat ominaisuudet ovat käytettävissä **typeProperties** -osassa:

| Ominaisuus | Kuvaus | Sallittu arvo | Pakollinen |
| -------- | ----------- | -------------- | -------- |
| sqlReaderQuery | Mukautetun kyselyn avulla voit lukea tietoja. | SQL-kyselymerkkijonon. Esimerkki: `select * from MyTable`.  | Ei |
| sqlReaderStoredProcedureName | Tallennetun toimintosarjan, joka lukee tietoja lähdetaulukosta nimi. | Tallennetun toimintosarjan nimen. | Ei |
| storedProcedureParameters | Tallennetun toimintosarjan parametrit. | Nimen ja arvon tietoparin. Nimiä ja parametrit kirjainkoko on vastattava nimet ja kirjainkoko tallennetun toimintosarjan parametrit. | Ei |

Jos **sqlReaderQuery** on määritetty SqlSource, kopioi tehtävän suorittaa kysely vastaan Azure SQL-tietokannan lähde, saat tiedot. Voit myös määrittää tallennetun toimintosarjan määrittämällä **sqlReaderStoredProcedureName** ja **storedProcedureParameters** (Jos tallennettu toimintosarja käyttää parametreja). 

Jos et määritä sqlReaderQuery tai sqlReaderStoredProcedureName, tietojoukko JSON rakenne-osassa määritettyjä sarakkeita käytetään kyselyn muodostaminen (`select column1, column2 from mytable`) voit suorittaa Azure SQL-tietokanta. Jos tietojoukko määritystä ei ole rakenteen, kaikki sarakkeet valitaan taulukosta. 

> [AZURE.NOTE] Kun käytät **sqlReaderStoredProcedureName**, haluat edelleen Määritä **taulukon nimi** -ominaisuuden arvo tietojoukko JSON. Ei ole vahvistukset suorittaa tämän taulukon vastaan vaikka. 

### <a name="sqlsource-example"></a>SqlSource Esimerkki

    "source": {
        "type": "SqlSource",
        "sqlReaderStoredProcedureName": "CopyTestSrcStoredProcedureWithParameters",
        "storedProcedureParameters": {
            "stringData": { "value": "str3" },
            "id": { "value": "$$Text.Format('{0:yyyy}', SliceStart)", "type": "Int"}
        }
    }

**Tallennettu toimintosarja-määritys:** 

    CREATE PROCEDURE CopyTestSrcStoredProcedureWithParameters
    (
        @stringData varchar(20),
        @id int
    )
    AS
    SET NOCOUNT ON;
    BEGIN
         select *
         from dbo.UnitTestSrcTable
         where dbo.UnitTestSrcTable.stringData != stringData
        and dbo.UnitTestSrcTable.id != id
    END
    GO


### <a name="sqlsink"></a>SqlSink 

**SqlSink** tukee seuraavia ominaisuuksia:

| Ominaisuus | Kuvaus | Sallittu arvo | Pakollinen |
| -------- | ----------- | -------------- | -------- |
| writeBatchTimeout | Erän Lisää toiminnon suorittaminen loppuun, ennen kuin se aikakatkaistaan odotusaika. | aikajakson<br/><br/> Esimerkki: "00: 30:00" (30 minuuttia). | Ei | 
| writeBatchSize | Lisää tiedot SQL-taulukkoon, kun puskurikoko saavuttaa writeBatchSize. | Kokonaisluku (rivien määrä)| Ei (oletus: 10000)
| sqlWriterCleanupScript | Määritä kysely kopioi tehtävän suorittamiseen siten, että Siivotaan tiedot tietyn muotoilu. Lisätietoja on artikkelissa [toistettavissa osa](#repeatability-during-copy). | Kysely-lause.  | Ei |
| sliceIdentifierColumnName | Määritä kopioi tehtävän muodostetaan automaattisesti sektoria tunnus, jota käytetään tiettyjen muotoilu, kun uudelleen tietojen tyhjennys täyttää sarakkeen nimi. Lisätietoja on artikkelissa [toistettavissa osa](#repeatability-during-copy). | Sarake, jonka tietotyypin binary(32) sarakkeen nimi. | Ei |
| sqlWriterStoredProcedureName | Tallennetun toimintosarjan nimen, upserts (päivitykset ja lisää) tiedot kohdetaulukkoon. | Tallennetun toimintosarjan nimen. | Ei |
| storedProcedureParameters | Tallennetun toimintosarjan parametrit. | Nimen ja arvon tietoparin. Nimiä ja parametrit kirjainkoko on vastattava nimet ja kirjainkoko tallennetun toimintosarjan parametrit. | Ei | 
| sqlWriterTableType | Määritä taulukon tyyppi, jota käytetään tallennetun toimintosarjan nimen. Kopioi tehtävän mahdollistaa siirretään tietojen temp taulukko, jossa on taulukon tällaista käytettävissä. Tallennettu toimintosarja koodi sitten yhdistää tiedot kopioidaan olemassa olevien tietojen kanssa. | Kirjoita taulukkonimi. | Ei |

#### <a name="sqlsink-example"></a>SqlSink Esimerkki

    "sink": {
        "type": "SqlSink",
        "writeBatchSize": 1000000,
        "writeBatchTimeout": "00:05:00",
        "sqlWriterStoredProcedureName": "CopyTestStoredProcedureWithParameters",
        "sqlWriterTableType": "CopyTestTableType",
        "storedProcedureParameters": {
            "id": { "value": "1", "type": "Int" },
            "stringData": { "value": "str1" },
            "decimalData": { "value": "1", "type": "Decimal" }
        }
    }

## <a name="identity-columns-in-the-target-database"></a>Käyttäjätietojen kohdetietokannan sarakkeissa
Tässä osassa on esimerkki tietojen kopioiminen ilman tunnussarake lähdetaulukkoon kohdetaulukko tunnussarake kanssa. 

**Lähdetaulukko:** 

    create table dbo.SourceTbl
    (
           name varchar(100),
           age int
    )

**Kohdetaulukko:**

    create table dbo.TargetTbl
    (
           id int identity(1,1),
           name varchar(100),
           age int
    )


Huomaa, että kohdetaulukkoon on tunnussarake. 

**Tietojoukon JSON määritys**

    {
        "name": "SampleSource",
        "properties": {
            "type": " SqlServerTable",
            "linkedServiceName": "TestIdentitySQL",
            "typeProperties": {
                "tableName": "SourceTbl"
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true,
            "policy": {}
        }
    }

**Kohde tietojoukko JSON määritys**

    {
        "name": "SampleTarget",
        "properties": {
            "structure": [
                { "name": "name" },
                { "name": "age" }
            ],
            "type": "AzureSqlTable",
            "linkedServiceName": "TestIdentitySQLSource",
            "typeProperties": {
                "tableName": "TargetTbl"
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": false,
            "policy": {}
        }   
    }


Huomaa, että lähde- ja taulukon on eri rakenne (kohde on toisen sarakkeen käyttäjätietojen kanssa). Tässä skenaariossa haluat määrittää **rakenne** -ominaisuuden kohde tietojoukko määritelmä, joka ei sisällä identity-sarake. 

Tämän jälkeen voit yhdistää tietolähteen tietojoukon sarakkeet kohde-tietojoukko sarakkeisiin. Esimerkki Katso [sarakkeen yhdistäminen mallit](#column-mapping-samples) -osa. 

[AZURE.INCLUDE [data-factory-type-repeatability-for-sql-sources](../../includes/data-factory-type-repeatability-for-sql-sources.md)] 

[AZURE.INCLUDE [data-factory-sql-invoke-stored-procedure](../../includes/data-factory-sql-invoke-stored-procedure.md)]
 

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

### <a name="type-mapping-for-sql-server--azure-sql-database"></a>Kirjoita SQL Serverin ja Azure SQL-tietokannan yhdistäminen

Mainitun [tietojen siirtämistä toimintoja](data-factory-data-movement-activities.md) artikkelissa kopioi tehtävän suorittaa Automaattinen tyypin muunnoksia lähteen tyyppiä vastaamaan allas tyypit seuraavista 2 vaihe vaiheelta kanssa:

1. Muuntaminen alkuperäisen tietolähteen tyypit .NET-tyyppi
2. .NET tyypistä muuntaminen alkuperäisen käsittelytoiminto tyyppi

Kun siirrät tietoja & Azure SQL-SQL server-Sybase seuraavat määritykset käytetään SQL-tyypistä .NET tyyppi ja päinvastoin.

Määritys on sama kuin ADO.NET SQL Serverin tietojen tyypin yhdistäminen.

| SQL Server-tietokantamoduuli tyyppi | .NET framework-tyyppi |
| ------------------------------- | ------------------- |
| bigint | Int64 |
| binaaritiedosto | Byte] |
| Bit | Totuusarvo |
| merkki | Merkkijono, merkki] |
| päivämäärä | Päivämäärä ja aika |
| Päivämäärä ja aika | Päivämäärä ja aika |
| datetime2 | Päivämäärä ja aika |
| DateTimeOffset-arvoa | DateTimeOffset-arvoa |
| Desimaaliluku | Desimaaliluku |
| FILESTREAM määrite (varbinary(max)) | Byte] |
| Liukuluku | Kaksinkertainen |
| Kuva | Byte] | 
| kokonaisluku | Int32 | 
| rahaa | Desimaaliluku |
| nchar | Merkkijono, merkki] |
| ntext | Merkkijono, merkki] |
| numeerinen | Desimaaliluku |
| nvarchar | Merkkijono, merkki] |
| Todellinen | Yksittäisen |
| ROWVERSION | Byte] |
| smalldatetime | Päivämäärä ja aika |
| smallint | Int16 |
| Smallmoney | Desimaaliluku | 
| sql_variant | Objektin * |
| teksti | Merkkijono, merkki] |
| aika | Aikajakson |
| aikaleima | Byte] |
| tinyint | Tavu |
| Uniqueidentifier | GUID-tunnus |
| Varbinary |  Byte] |
| varchar | Merkkijono, merkki] |
| XML | XML |


[AZURE.INCLUDE [data-factory-type-conversion-sample](../../includes/data-factory-type-conversion-sample.md)]

[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

## <a name="performance-and-tuning"></a>Suorituskyky ja säätäminen  
Lisätietoja on artikkelissa [Kopioi tehtävän suorituskyvyn & säätäminen opas](data-factory-copy-activity-performance.md) lisätietoja tärkeimmät seikat, vaikutus suorituskykyä tietojen siirto (kopioi tehtävä) Azure Data Factory ja voidaan optimoida sen usealla tavalla.