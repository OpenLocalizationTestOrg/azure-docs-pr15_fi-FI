<properties 
    pageTitle="Tietojen siirtäminen tai Azure-taulukosta | Microsoft Azure" 
    description="Opettele siirtämään tiedot ja käyttämällä Azure Data Factory Azure-taulukkotallennus." 
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
    ms.date="09/13/2016" 
    ms.author="jingwang"/>

# <a name="move-data-to-and-from-azure-table-using-azure-data-factory"></a>Siirrä tiedot ja Azure-taulukosta Azure Data Factory

Tässä artikkelissa käsitellään käyttämisestä kopioi tehtävän Azure tietojen factory-Siirry tai taulukon tietojen Azure / toisen tietosäilö. Tässä artikkelissa perustuu [tietojen siirtämistä tehtävät](data-factory-data-movement-activities.md) -artikkelista, jossa näkyy yleiskatsaus tietojen siirto ja tuettuja kaupan yhdistelmiä kopioi aktiviteettiin.

## <a name="copy-data-wizard"></a>Ohjattu tietojen kopioiminen
Helpoin tapa luoda myyntijakso, kopioi tiedot ja Azure-taulukkotallennus on käyttämällä ohjattua Kopioi tiedot. Katso [Opetusohjelma: Luo kopio ohjatulla putkijohto](data-factory-copy-data-wizard-tutorial.md) nopeasti käyttämällä ohjattua tietojen kopioiminen putkijohto luomisesta saat. 

Seuraavissa esimerkeissä otoksen JSON määritykset, jonka avulla voit luoda putkijohto [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) tai [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) tai [PowerShellin Azure](data-factory-copy-activity-tutorial-using-powershell.md). Ne näyttävät tietojen kopioimisesta Azure-taulukkotallennus ja päinvastoin Azure-Blob-objektien tietokanta. Tietoja voi kuitenkin kopioidun **suoraan** mistä tahansa lähteestä johonkin seuraavista poistumia määritetyllä tavalla [tähän](data-factory-data-movement-activities.md#supported-data-stores) kopio-toimintojen käyttäminen Azure Data Factory.

## <a name="sample-copy-data-from-azure-table-to-azure-blob"></a>Esimerkki: Kopioimalla tiedot Azure-Blob Azure-taulukosta

Seuraavassa esimerkissä:

1.  Linkitetyn palvelu tyypin [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties) (käytetään taulukon & blob).
2.  Syötteen [tietojoukko](data-factory-create-datasets.md) tyypin [Azuren](#azure-table-dataset-type-properties).
3.  Tulosteen [tietojoukko](data-factory-create-datasets.md) tyypin [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties). 
3.  [Myyntijakso](data-factory-create-pipelines.md) kopioi aktiviteettiin, joka käyttää [AzureTableSource](#azure-table-copy-activity-type-properties) ja [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties). 

Otosten kopioi tiedot, jotka kuuluvat Azure-taulukosta, blob oletusarvo-osion tunnissa. Käyttää näitä esimerkkejä JSON-ominaisuuksia on kuvattu osia seuraavat mallit.

**Azure-tallennustilan linkitetty palvelu:**

    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

Azure Data Factory tukee kahdentyyppisiä Azuren tallennustilaan linkitetty palveluita: **AzureStorage** ja **AzureStorageSas**. Voit määrittää yhteysmerkkijonon, joka sisältää-tilin ensimmäisen vaiheen ja uudempi kohdalla voit määrittää jaetun Access allekirjoitus (SAS) Uri. Katso [Linkitetyn Services](#linked-services) osassa.  

**Azure taulukon syötteen tietojoukko:**

Otosten oletetaan, että olet luonut taulukon "MyTable" Azure-taulukosta.
 
Asetus "ulkoinen": "true" ilmoittaa Data Factory-palvelun dataset on ulkoiset tiedot-factory ja tietojen factory toimintaa ei tuottamat.

    {
      "name": "AzureTableInput",
      "properties": {
        "type": "AzureTable",
        "linkedServiceName": "StorageLinkedService",
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

**Azure-Blob tulosteen tietojoukko:**

Tietoja kirjoitetaan uusia Blob-objektien tunnissa (taajuus: tunti, väli: 1). Kansiopolun blob arvioidaan dynaamisesti sektoria, käsitellään alkamispäivää perusteella. Kansiopolku käytetään vuoden, kuukauden, päivän ja alkamisajan tuntia osat. 

    {
      "name": "AzureBlobOutput",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

**Putkijohto kopio-toimintoa:**

Putkisto sisältää kopio-toiminto, joka on määritetty käyttämään syöttö- ja tietojoukkoja ja on suunniteltu toimimaan tunnissa. Myyntijakso JSON määritelmä- **lähteen** tyyppi on määritetty **AzureTableSource** ja **käsittelytoiminto** tyyppi-asetukseksi on määritetty **BlobSink**. SQL-kysely, joka on määritetty **AzureTableSourceQuery** -ominaisuuden valitsee tiedot oletusarvo-osiosta tunnissa kopioi.

    {  
        "name":"SamplePipeline",
        "properties":{  
            "start":"2014-06-01T18:00:00",
            "end":"2014-06-01T19:00:00",
            "description":"pipeline for copy activity",
            "activities":[  
                {
                    "name": "AzureTabletoBlob",
                    "description": "copy activity",
                    "type": "Copy",
                    "inputs": [
                        {
                            "name": "AzureTableInput"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobOutput"
                        }
                    ],
                    "typeProperties": {
                        "source": {
                            "type": "AzureTableSource",
                            "AzureTableSourceQuery": "PartitionKey eq 'DefaultPartitionKey'"
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

## <a name="sample-copy-data-from-azure-blob-to-azure-table"></a>Esimerkki: Tietojen kopioiminen Azure-Blob Azure-taulukosta

Seuraavassa esimerkissä:

1.  Linkitetyn palvelun tyypin [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties) (käytetään taulukon & blob)
3.  Syötteen [tietojoukko](data-factory-create-datasets.md) tyypin [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4.  Tulosteen [tietojoukko](data-factory-create-datasets.md) tyypin [Azuren](#azure-table-dataset-type-properties). 
4.  [Myyntijakso](data-factory-create-pipelines.md) kopioi aktiviteettiin, joka käyttää [BlobSource](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) ja [AzureTableSink](#azure-table-copy-activity-type-properties). 


Esimerkki kopiot aikasarjalle tietojen tuominen Azure blob, Azure taulukon hourly. Käyttää näitä esimerkkejä JSON-ominaisuuksia on kuvattu osia seuraavat mallit.

**Azuren tallennustila (sekä Azure-taulukosta ja Blob) linkitetty palvelu:**

    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

Azure Data Factory tukee kahdentyyppisiä Azuren tallennustilaan linkitetty palveluita: **AzureStorage** ja **AzureStorageSas**. Voit määrittää yhteysmerkkijonon, joka sisältää-tilin ensimmäisen vaiheen ja uudempi kohdalla voit määrittää jaetun Access allekirjoitus (SAS) Uri. Katso [Linkitetyn Services](#linked-services) osassa. 

**Azure Blob syötteen tietojoukko:**

Tiedot on noudettu uusi blob-tunnissa (taajuus: tunti, väli: 1). Blob kansion polku ja tiedostonimi dynaamisesti arvioidaan, käsitellään sektoria alkamispäivää perusteella. Kansiopolku käyttävät vuoden, kuukauden ja päivän aloitusaika-osa ja tiedostonimi alkamisajan tunti-osa. "ulkoinen": "true" asetus ilmoittaa Data Factory-palvelun dataset on ulkoiset tiedot-factory ja tietojen factory toimintaa ei tuottamat.
    
    {
      "name": "AzureBlobInput",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}",
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

**Azuren taulukkojen tulosteen tietojoukko:**

Otosten kopioi Azure-taulukosta "MyTable"-taulukon tietoja. Luo Azure taulukko sama määrä sarakkeita sisältävän Blob CSV-tiedoston haluamallasi tavalla. Uusien rivien lisätään taulukon tunnissa. 

    {
      "name": "AzureTableOutput",
      "properties": {
        "type": "AzureTable",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "tableName": "MyOutputTable"
        },
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }

**Putkijohto kopio-toimintoa:**

Putkisto sisältää kopio-toiminto, joka on määritetty käyttämään syöttö- ja tietojoukkoja ja on suunniteltu toimimaan tunnissa. Myyntijakso JSON määritelmä- **lähteen** tyyppi on määritetty **BlobSource** ja **käsittelytoiminto** tyyppi-asetukseksi on määritetty **AzureTableSink**. 


    {  
        "name":"SamplePipeline",
        "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline with copy activity",
        "activities":[  
          {
            "name": "AzureBlobtoTable",
            "description": "Copy Activity",
            "type": "Copy",
            "inputs": [
              {
                "name": "AzureBlobInput"
              }
            ],
            "outputs": [
              {
                "name": "AzureTableOutput"
              }
            ],
            "typeProperties": {
              "source": {
                "type": "BlobSource"
              },
              "sink": {
                "type": "AzureTableSink",
                "writeBatchSize": 100,
                "writeBatchTimeout": "01:00:00"
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

## <a name="linked-services"></a>Linkitetyn palvelut
Voit linkittää Azure-blob-säiliö Azure tietojen factory linkitetyn palvelujen kahdenlaisia. Ne ovat: **AzureStorage** linkitetty palvelu ja **AzureStorageSas** linkitetty palvelu. Azure-tallennustilan linkitetty-palvelu sisältää tietoja factory yleisen käytön Azure-tallennustilan. Olisi Azure-tallennustilan Suojaussidosten (jaettu Access allekirjoitus) linkitetty-palvelu sisältää tietoja factory rajoitettu, aika-sidottu käytön Azure-tallennustilan. On nämä kaksi linkitetyn palveluiden välillä ei ole muita eroja. Valitse linkitetty palvelu, joka tarpeisiisi. Seuraavissa osissa on nämä kaksi linkitetyn Services enemmän tietoja.

[AZURE.INCLUDE [data-factory-azure-storage-linked-services](../../includes/data-factory-azure-storage-linked-services.md)]

## <a name="azure-table-dataset-type-properties"></a>Azure taulukon tietojoukko ominaisuudet

Täydellinen luettelo osien ja ominaisuudet, jotka ovat käytettävissä määrittäminen tietojoukkoja [luominen tietojoukkoja](data-factory-create-datasets.md) on artikkelissa. Osia, kuten rakenne, käytettävyys ja tietojoukko JSON käytännön muistuttavat tyypeissä tietojoukko (Azure SQL Azure-blob, Azuren taulukkojen jne.).

TypeProperties-osassa on eri mistäkin tietojoukko, ja se antaa tietoja tietojen tietovaraston sijainti. **Azuren** tyypin tietojoukon **typeProperties** osiossa on seuraavat ominaisuudet.

| Ominaisuus | Kuvaus | Pakollinen |
| -------- | ----------- | -------- |
| taulukon nimi | Taulukko, joka viittaa linkitetyistä Azure taulukon tietokannan esiintymän nimi. | Kyllä. Kun taulukon nimi on määritetty ilman azureTableSourceQuery, kaikki tietueet taulukosta kopioidaan kohteeseen. Jos azureTableSourceQuery on määritetty, tietueet, jotka täyttävät kyselyn taulukosta kopioidaan kohteeseen. |

### <a name="schema-by-data-factory"></a>Rakenteen mukaan Data Factory
Rakenteen tiedot stores, kuten Azure-taulukosta Data Factory-palvelun johtaa rakenteen jollakin seuraavista tavoista:

1.  Jos määrität **rakenne** -ominaisuuden avulla tietojoukko määrityksessä olevien tietojen rakennetta, Data Factory-palvelun merkkien rakenteen kuin rakenne. Tässä tapauksessa rivi ei sisällä sarakkeen arvon, jos arvo on null annetaan sen.
2. Jos et määritä olevien tietojen rakennetta **rakenne** -ominaisuuden avulla tietojoukko määrityksessä, Data Factory johtaa rakenteen käyttämällä ensimmäisen rivin tiedot. Tässä tapauksessa jos ensimmäinen rivi ei sisällä koko rakenteen, joitakin sarakkeita on vastaamatta jääneiden kopiointi läpäisseet.

Tämän vuoksi rakenteen vapauttaa tietolähteiden paras käytäntö on Määritä olevien tietojen **rakennetta** -ominaisuuden avulla rakennetta.

## <a name="azure-table-copy-activity-type-properties"></a>Azure taulukon kopioi tehtävän ominaisuudet

Täydellinen luettelo osat ja ominaisuudet, jotka ovat käytettävissä toiminnan määrittäminen [Putkistot luomisesta](data-factory-create-pipelines.md) on artikkelissa. Ominaisuudet, kuten nimi, kuvaus sekä syöttö- ja tietojoukkoja ja käytännöt ovat käytettävissä kaikki tehtävälajit. 

Tehtävän typeProperties-osassa käytettävissä olevat ominaisuudet vaihtelevat toisaalta kunkin tehtävätyyppi. Kopioi tehtävälle ne vaihtelevat sen mukaan, lähteiden ja poistumia.

**AzureTableSource** tukee seuraavia ominaisuuksia typeProperties-osassa:

Ominaisuus | Kuvaus | Sallittu arvo | Pakollinen
-------- | ----------- | -------------- | -------- 
azureTableSourceQuery | Mukautetun kyselyn avulla voit lukea tietoja. | Azuren taulukkojen kyselymerkkijonon. Esimerkkejä seuraavan osion. | Ei. Kun taulukon nimi on määritetty ilman azureTableSourceQuery, kaikki tietueet taulukosta kopioidaan kohteeseen. Jos azureTableSourceQuery on määritetty, tietueet, jotka täyttävät kyselyn taulukosta kopioidaan kohteeseen.
azureTableSourceIgnoreTableNotFound | Määritä, onko swallow lukuun ottamatta taulukossa ei ole. | TOSI<br/>EPÄTOSI | Ei |

### <a name="azuretablesourcequery-examples"></a>azureTableSourceQuery esimerkkejä

Jos Azure taulukon sarake on merkkijono tyyppiä: 

    azureTableSourceQuery": "$$Text.Format('PartitionKey ge \\'{0:yyyyMMddHH00_0000}\\' and PartitionKey le \\'{0:yyyyMMddHH00_9999}\\'', SliceStart)"

Jos Azure taulukon sarake on päivämäärä ja aika: 

    "azureTableSourceQuery": "$$Text.Format('DeploymentEndTime gt datetime\\'{0:yyyy-MM-ddTHH:mm:ssZ}\\' and DeploymentEndTime le datetime\\'{1:yyyy-MM-ddTHH:mm:ssZ}\\'', SliceStart, SliceEnd)"


**AzureTableSink** tukee seuraavia ominaisuuksia typeProperties-osassa:


Ominaisuus | Kuvaus | Sallittu arvo | Pakollinen  
-------- | ----------- | -------------- | -------- 
azureTableDefaultPartitionKeyValue | Osion avaimen oletusarvo, joka voidaan käyttää käsittelytoiminto. | Merkkijonoarvo. | Ei 
azureTablePartitionKeyName | Määritä sarake, jonka arvoja käytetään osion näppäimet. Jos ei määritetä, AzureTableDefaultPartitionKeyValue käytetään osio-näppäintä. | Sarakkeen nimi. | Ei |
azureTableRowKeyName | Määritä sarake, jonka sarakkeen arvoja käytetään rivin avaimeksi. Jos ei määritetä, käytä GUID-tunnus kullekin riville. | Sarakkeen nimi. | Ei  
azureTableInsertType | Tietojen lisääminen Azuren taulukkojen tila.<br/><br/>Tämä ominaisuus ohjaa, onko olemassa olevien rivien tulosteen taulukkoon vastaavaa osiota ja rivi on niiden arvojen korvataan tai yhdistetty. <br/><br/>Lisätietoja näiden asetusten (Yhdistä ja korvaa) toiminnasta on kohdassa [Lisää tai Yhdistä kohde](https://msdn.microsoft.com/library/azure/hh452241.aspx) ja [Lisää tai korvaa kohteen](https://msdn.microsoft.com/library/azure/hh452242.aspx) ohjeita. <br/><br> Tämä asetus koskee rivin tasolla, ei taulukon tasata, ja kumpaakaan asetus poistaa tulosteen taulukon rivit, jotka eivät ole syöte. | Yhdistä (oletus)<br/>Korvaa | Ei 
writeBatchSize | Lisää tietoja Azure-taulukkoon, kun writeBatchSize tai writeBatchTimeout viesti. | Kokonaisluku (rivien määrä)| Ei (oletus: 10000) 
writeBatchTimeout | Lisää tietoja Azure taulukkoon, kun writeBatchSize tai writeBatchTimeout viesti | aikajakson<br/><br/>Esimerkki: "00:20:00" (20 minuuttia) | Ei (oletusarvo on aikakatkaisun tallennustilan asiakkaan oletusarvo arvo 90 sekunti)

### <a name="azuretablepartitionkeyname"></a>azureTablePartitionKeyName
Yhdistä lähdesarake käyttämällä translator JSON-ominaisuuden, ennen kuin voit käyttää kohdesarakkeen azureTablePartitionKeyName kohdesarakkeen.

Seuraavassa esimerkissä on yhdistetty lähdesarakkeen DivisionID kohdesarakkeen: DivisionID.  

    "translator": {
        "type": "TabularTranslator",
        "columnMappings": "DivisionID: DivisionID, FirstName: FirstName, LastName: LastName"
    } 

DivisionID on määritetty osio-näppäintä. 

    "sink": {
        "type": "AzureTableSink",
        "azureTablePartitionKeyName": "DivisionID",
        "writeBatchSize": 100,
        "writeBatchTimeout": "01:00:00"
    }


[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

### <a name="type-mapping-for-azure-table"></a>Azuren taulukkojen yhdistäminen tyyppi

[Tietojen siirtämistä toiminnot](data-factory-data-movement-activities.md) on artikkelissa mainittu kopioi tehtävän suorittaa Automaattinen tyypin muunnoksia lähteen tyyppiä vastaamaan allas tyypit seuraavista kaksivaiheinen lähestymistapa kanssa.

1. Muuntaminen alkuperäisen tietolähteen tyypit .NET-tyyppi
2. .NET tyypistä muuntaminen alkuperäisen käsittelytoiminto tyyppi

Kun tietojen siirtämistä & Azure-taulukosta, seuraavat [yhdistämismääritykset määrittämiä Azure-taulukosta palvelun](https://msdn.microsoft.com/library/azure/dd179338.aspx) käytetään Azure taulukon OData-tyypeistä .NET tyyppi ja päinvastoin. 

| OData-tietotyyppi | .NET-tyyppi | Tiedot |
| --------------- | --------- | ------- |
| Edm.Binary | Byte] | Matriisin tavua enintään 64 Kilotavua. |
| Edm.Boolean | bool | Totuusarvo. |
| Edm.DateTime | Päivämäärä ja aika | 64-bittinen arvo, joka kuin koordinoidun yleisajan (UTC-ajan). Tuetut DateTime-alueen alkaa 12:00 keskiyöhön, tammikuussa 1 1601 A.D. (ENSIMMÄISEN PÄIVÄN KESKIYÖSTÄ LÄHTIEN), UTC-AIKA. Alueen päättyy joulukuun 31 9999. |
| Edm.Double | Kaksinkertainen | 64-bittinen irrallisen pisteen arvon. |
| Edm.Guid | GUID-tunnus | 128 bittistä GUID-tunnus. |
| Edm.Int32 | Int32 tai kokonaisluku | 32-bittisen kokonaisluvun. |
| Edm.Int64 | Int64 tai pitkä | 64-bittisen kokonaisluvun. |
| Edm.String | Merkkijono | UTF-16-koodattu arvo. Merkkijonoarvoa voi olla enintään 64 Kilotavua. |

### <a name="type-conversion-sample"></a>Kirjoita muuntaminen malli

Seuraavassa esimerkissä on kopioimalla tiedot Azure-Blob Azure-taulukosta laji muunnosten kanssa. 

Oletetaan, että Blob-objektien tietojoukkoa on CSV-muodossa ja se sisältää kolme saraketta. Jonkin niistä on datetime-sarake, jossa mukautetun datetime-muodossa, käyttämällä viikonpäivän lyhennetty Ranskan nimet. 

Määritä seuraavat määritelmät sarakkeille sekä Blob lähde-tietojoukko.
    
    {
        "name": " AzureBlobInput",
        "properties":
        {
             "structure": 
              [
                    { "name": "userid", "type": "Int64"},
                    { "name": "name", "type": "String"},
                    { "name": "lastlogindate", "type": "Datetime", "culture": "fr-fr", "format": "ddd-MM-YYYY"}
              ],
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/myfolder",
                "fileName":"myfile.csv",
                "format":
                {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                }
            },
            "external": true,
            "availability":
            {
                "frequency": "Hour",
                "interval": 1,
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

Valita tyyppi-määritys Azure taulukon OData tyypistä .NET tyyppi, määrität taulukon Azure-taulukosta seuraavat rakenteen kanssa. 

**Azure taulukko rakenne:**

Sarakkeen nimi | Tyyppi
----------- | --------
käyttäjätunnus | Edm.Int64
Nimi | Edm.String 
lastlogindate | Edm.DateTime

Määritä seuraavaksi Azure-taulukosta-tietojoukko seuraavasti. Ei tarvitse määrittää "rakenteen-osassa tyyppi tiedoilla, koska tiedot on jo määritetty pohjana tietovaraston.

    {
      "name": "AzureTableOutput",
      "properties": {
        "type": "AzureTable",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "tableName": "MyOutputTable"
        },
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }

Tässä tapauksessa Data Factory automaattisesti tietotyyppimuunnosten myös Datetime-kenttää, joiden avulla "f f" maa-asetus, kun tietojen siirtäminen Azure-taulukosta Blob-objektien mukautetun datetime-muodossa.



[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

## <a name="performance-and-tuning"></a>Suorituskyky ja säätäminen  
Lisätietoja tärkeimmät seikat, tietojen siirto (kopioi tehtävä) Azure Data Factory ja eri tapoja optimoida sen vaikutus toimintakykyä on artikkelissa [Kopioi tehtävän suorituskyvyn & säätäminen opas](data-factory-copy-activity-performance.md).







