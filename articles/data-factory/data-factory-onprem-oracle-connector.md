<properties 
    pageTitle="Tietojen siirtäminen tai käyttämällä Data Factory Oracle | Microsoft Azure" 
    description="Lue, miten voit siirtää tietoja tai Oracle-tietokantaan, joka on paikallisen Azure Data Factory avulla." 
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

# <a name="move-data-tofrom-on-premises-oracle-using-azure-data-factory"></a>Siirrä tiedot ja paikallisen Oracle Azure Data Factory käyttäminen 

Tässä artikkelissa kuvataan, miten voit tietojen factory kopioi tehtävän tiedot voidaan siirtää ja Oracle /, toinen tietojen tallentamista. Tässä artikkelissa perustuu [tietojen siirtämistä tehtävät](data-factory-data-movement-activities.md) -artikkelista, jossa näkyy yleiskatsaus tietojen siirto kopioi tehtävän ja tuettuja kaupan yhdistelmät.

## <a name="installation"></a>Asennus 
Azure Data Factory-palvelua voi yhdistää paikallisen Oracle-tietokantaan sinun on asennettava seuraavat osat: 

- Data Management Gatewayn saman tietokannan isännöivän tietokoneen tai erillisellä koneella välttää resurssien tietokannassa pikaluistelukilpailuissa. Data Management Gateway on asiakas-agentti, joka yhdistää käyttöön paikallisten tietolähteiden pilvipalveluihin turvallisemman ja hallitun tavalla. Lisätietoja Data Management Gateway on artikkelissa [paikallisen ja cloud tietojen siirtäminen](data-factory-move-data-between-onprem-and-cloud.md) artikkelissa. 
- .NET Frameworkin Oracle-tietopalvelu. Tämän osan sisältyy [Oracle tietojen käytön Windowsin osat](http://www.oracle.com/technetwork/topics/dotnet/downloads/). Asenna tarvittavat versio (32-tai 64-bittinen) host tietokoneeseen, johon on asennettu yhdyskäytävä. [Oracle Data Provider .NET 12.1](http://docs.oracle.com/database/121/ODPNT/InstallSystemRequirements.htm#ODPNT149) käyttää Oracle-tietokantaan 10 g 2 tai uudempi versio.

    Jos valitset "XCopy asennuksen", noudata ohjeita readme.htm. Suosittelemme, että valitset installer käyttöliittymän (ei-XCopy jokin). 
 
    Asennettuasi palveluntarjoajan tietokoneeseen käyttämällä palveluiden sovelluksen (tai) Data Management Gatewayn määritysten hallinta Data Management Gateway Host (isäntä)-palvelun käynnistäminen uudelleen.  

> [AZURE.NOTE] Artikkelissa [vianmääritys yhdyskäytävän ongelmien](data-factory-data-management-gateway.md#troubleshoot-gateway-issues) vianmääritys yhteyden/yhdyskäytävän vihjeitä liittyvät ongelmat. 

## <a name="copy-data-wizard"></a>Ohjattu tietojen kopioiminen
Helpoin tapa luoda putkijohto, joka kopioi tiedot tai Oracle-tietokantaan johonkin seuraavista tuetuista käsittelytoiminto tietoja tallennetuista tiedoista on käyttämällä ohjattua Kopioi tiedot. Katso [Opetusohjelma: Luo kopio ohjatulla putkijohto](data-factory-copy-data-wizard-tutorial.md) nopeasti käyttämällä ohjattua tietojen kopioiminen putkijohto luomisesta saat. 

Seuraavassa esimerkissä on esimerkki JSON määritykset, jonka avulla voit luoda putkijohto [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) tai [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) tai [PowerShellin Azure](data-factory-copy-activity-tutorial-using-powershell.md). Ne näyttävät kopioiminen tiedot / / from Azure-Blob-säiliö Oracle-tietokantaan. Tietoja voi kuitenkin kopioida johonkin seuraavista poistumia määritetyllä tavalla [tähän](data-factory-data-movement-activities.md#supported-data-stores) kopio-toimintojen käyttäminen Azure Data Factory.   

## <a name="sample-copy-data-from-oracle-to-azure-blob"></a>Esimerkki: Tietojen kopioiminen Oracle Azure-Blob
Tässä esimerkissä esitetään, kuinka haluat kopioida tiedot Azure-Blob-objektien tallennustilaan paikallisen Oracle-tietokannasta. Tietoja voi kuitenkin kopioidun **suoraan** johonkin seuraavista poistumia määritetyllä tavalla [tähän](data-factory-data-movement-activities.md#supported-data-stores) kopio-toimintojen käyttäminen Azure Data Factory.  
 
Malli on tietoja factory-kohteet:

1.  Linkitetyn palvelu [OnPremisesOracle](data-factory-onprem-oracle-connector.md#oracle-linked-service-properties)tyyppi.
2.  Linkitetyn palvelu [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties)tyyppi.
3.  Syötteen [tietojoukko](data-factory-create-datasets.md) tyypin [OracleTable](data-factory-onprem-oracle-connector.md#oracle-dataset-type-properties). 
4.  Tulosteen [tietojoukko](data-factory-create-datasets.md) tyypin [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
5.  [Myyntijakso](data-factory-create-pipelines.md) kopioi aktiviteettiin, joka käyttää [OracleSource](data-factory-onprem-oracle-connector.md#oracle-copy-activity-type-properties) lähde- ja [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) käsittelytoiminto.

Otosten kopioi tiedot taulukkoon paikallisen Oracle-tietokantaan blob kerran tunnissa. Lisätietoja eri ominaisuuksia, joilla otoksessa mallit seuraavien osien ohjeissa.

**Linkitetty Oracle-palvelu:**

    {
      "name": "OnPremisesOracleLinkedService",
      "properties": {
        "type": "OnPremisesOracle",
        "typeProperties": {
          "ConnectionString": "data source=<data source>;User Id=<User Id>;Password=<Password>;",
          "gatewayName": "<gateway name>"
        }
      }
    }

**Azure-Blob-säiliö linkitetty palvelu:**

    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<Account key>"
        }
      }
    }

**Oracle syötteen tietojoukko:**

Otosten oletetaan, että olet luonut taulukon "MyTable" Oracle ja se sisältää sarake nimeltä "timestampcolumn" sarjan Aikatietojen. 

Asetus "ulkoinen": "true" ilmoittaa Data Factory-palvelun dataset on ulkoiset tiedot-factory ja tietojen factory toimintaa ei tuottamat.

    {
        "name": "OracleInput",
        "properties": {
            "type": "OracleTable",
            "linkedServiceName": "OnPremisesOracleLinkedService",
            "typeProperties": {
                "tableName": "MyTable"
            },
               "external": true,
            "availability": {
                "offset": "01:00:00",
                "interval": "1",
                "anchorDateTime": "2014-02-27T12:00:00",
                "frequency": "Hour"
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

Tietoja kirjoitetaan uusia Blob-objektien tunnissa (taajuus: tunti, väli: 1). Blob kansion polku ja tiedostonimi dynaamisesti arvioidaan, käsitellään sektoria alkamispäivää perusteella. Kansiopolku käytetään vuoden, kuukauden, päivän ja alkamisajan tuntia osat.
    
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


**Kopioi aktiviteettiin putkijohto:**

Putkisto sisältää kopio-toiminto, joka on määritetty käyttämään syöttö- ja tietojoukkoja ja on suunniteltu toimimaan kerran tunnissa. Myyntijakso JSON määritelmä- **lähteen** tyyppi on määritetty **OracleSource** ja **käsittelytoiminto** tyyppi-asetukseksi on määritetty **BlobSink**.  SQL-kysely, joka on määritetty **oracleReaderQuery** -ominaisuuden valitsee edellisen tunnin kopioitavat tiedot.

    
    {  
        "name":"SamplePipeline",
        "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline for copy activity",
        "activities":[  
          {
            "name": "OracletoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [
              {
                "name": " OracleInput"
              }
            ],
            "outputs": [
              {
                "name": "AzureBlobOutput"
              }
            ],
            "typeProperties": {
              "source": {
                "type": "OracleSource",
                "oracleReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
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


Sinun on muutettava kyselymerkkijonon mukaan, miten päivämäärät on määritetty Oracle-tietokantaan. Jos saat seuraavan virhesanoman: 

    Message=Operation failed in Oracle Database with the following error: 'ORA-01861: literal does not match format string'.,Source=,''Type=Oracle.DataAccess.Client.OracleException,Message=ORA-01861: literal does not match format string,Source=Oracle Data Provider for .NET,'.

Joudut ehkä muokata kyselyä, kuten seuraavassa esimerkissä (to_date-funktion käyttäminen):

    "oracleReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= to_date(\\'{0:MM-dd-yyyy HH:mm}\\',\\'MM/DD/YYYY HH24:MI\\')  AND timestampcolumn < to_date(\\'{1:MM-dd-yyyy HH:mm}\\',\\'MM/DD/YYYY HH24:MI\\') ', WindowStart, WindowEnd)"

## <a name="sample-copy-data-from-azure-blob-to-oracle"></a>Esimerkki: Tietojen kopioiminen Azure-Blob Oracle
Tässä esimerkissä esitetään, kuinka voit kopioida tietoja Azure-Blob-objektien tallennustilaan paikallisen Oracle-tietokantaan. Tietoja voi kuitenkin kopioidun **suoraan** mistä tahansa lähteistä määritetyllä tavalla [tähän](data-factory-data-movement-activities.md#supported-data-stores) kopio-toimintojen käyttäminen Azure Data Factory.  
 
Malli on tietoja factory-kohteet:

1.  Linkitetyn palvelu [OnPremisesOracle](data-factory-onprem-oracle-connector.md#oracle-linked-service-properties)tyyppi.
2.  Linkitetyn palvelu [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties)tyyppi.
3.  Syötteen [tietojoukko](data-factory-create-datasets.md) tyypin [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4.  Tulosteen [tietojoukko](data-factory-create-datasets.md) tyypin [OracleTable](data-factory-onprem-oracle-connector.md#oracle-dataset-type-properties). 
5.  [Myyntijakso](data-factory-create-pipelines.md) kopioi aktiviteettiin, joka käyttää [BlobSource](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) sellaisena kuin käsittelytoiminto [OracleSink](data-factory-onprem-oracle-connector.md#oracle-copy-activity-type-properties) lähde.

Otosten kopioi tiedot blob paikallisen Oracle-tietokantaan taulukkoon tunnissa. Lisätietoja eri ominaisuuksia, joilla otoksessa mallit seuraavien osien ohjeissa.

**Linkitetty Oracle-palvelu:**

    {
      "name": "OnPremisesOracleLinkedService",
      "properties": {
        "type": "OnPremisesOracle",
        "typeProperties": {
          "ConnectionString": "data source=<data source>;User Id=<User Id>;Password=<Password>;",
          "gatewayName": "<gateway name>"
        }
      }
    }

**Azure-Blob-säiliö linkitetty palvelu:**

    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<Account key>"
        }
      }
    }

**Azure Blob syötteen tietojoukko**

Tiedot on noudettu uusi blob-tunnissa (taajuus: tunti, väli: 1). Blob kansion polku ja tiedostonimi dynaamisesti arvioidaan, käsitellään sektoria alkamispäivää perusteella. Kansiopolku käyttävät vuoden, kuukauden ja päivän aloitusaika-osa ja tiedostonimi alkamisajan tunti-osa. "ulkoinen": "true" asetus ilmoittaa Data Factory-palvelun tässä taulukossa on ulkoiset tiedot-factory ja tietojen factory toimintaa ei tuottamat.

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

**Oracle tulosteen tietojoukko:**

Otosten oletetaan, että olet luonut taulukon "MyTable" Oracle. Luo taulukon Oracle on yhtä monta saraketta sisältämään Blob CSV-tiedoston haluamallasi tavalla. Uusien rivien lisätään taulukon tunnissa.

    {
        "name": "OracleOutput",
        "properties": {
            "type": "OracleTable",
            "linkedServiceName": "OnPremisesOracleLinkedService",
            "typeProperties": {
                "tableName": "MyTable"
            },
            "availability": {
                "frequency": "Hour",
                "interval": "1"
            }
        }
    }


**Kopioi aktiviteettiin putkijohto:**

Putkisto sisältää kopio-toiminto, joka on määritetty käyttämään syöttö- ja tietojoukkoja ja on suunniteltu toimimaan tunnissa. Myyntijakso JSON määritelmä- **lähteen** tyyppi on määritetty **BlobSource** ja **käsittelytoiminto** tyyppi-asetukseksi on määritetty **OracleSink**.  

    
    {  
        "name":"SamplePipeline",
        "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline with copy activity",
        "activities":[  
          {
            "name": "AzureBlobtoOracle",
            "description": "Copy Activity",
            "type": "Copy",
            "inputs": [
              {
                "name": "AzureBlobInput"
              }
            ],
            "outputs": [
              {
                "name": "OracleOutput"
              }
            ],
            "typeProperties": {
              "source": {
                "type": "BlobSource"
              },
              "sink": {
                "type": "OracleSink"
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


## <a name="oracle-linked-service-properties"></a>Linkitetty Oracle-palveluominaisuudet

Seuraavassa taulukossa on kuvaus JSON elementtien tietyn linkitetty Oracle-palveluun. 

Ominaisuus | Kuvaus | Pakollinen
-------- | ----------- | --------
tyyppi | Type-ominaisuus on määritettävä: **OnPremisesOracle** | Kyllä
connectionString | Määritä Oracle-tietokantaan esiintymään connectionString-ominaisuuden tarvittavat tiedot. | Kyllä 
gatewayName | Yhdyskäytävän nimeä, jota käytetään yhteyden muodostaminen paikalliseen Oracle-palvelin | Kyllä

Saat lisätietoja määrittäminen paikallista Oracle-tietolähteen tunnistetiedot [määrittää käyttöoikeudet ja suojaus](data-factory-move-data-between-onprem-and-cloud.md#set-credentials-and-security) .
## <a name="oracle-dataset-type-properties"></a>Oracle tietojoukko ominaisuudet

Täydellinen luettelo osien ja ominaisuudet, jotka ovat käytettävissä määrittäminen tietojoukkoja [luominen tietojoukkoja](data-factory-create-datasets.md) on artikkelissa. Osia, kuten rakenne, käytettävyys ja tietojoukko JSON käytännön muistuttavat tyypeissä tietojoukko (Oracle-Azure-blob, Azuren taulukkojen jne.).
 
TypeProperties-osassa on eri mistäkin tietojoukko, ja se antaa tietoja tietojen tietovaraston sijainti. Tyypin OracleTable tietojoukon typeProperties osiossa on seuraavat ominaisuudet:

Ominaisuus | Kuvaus | Pakollinen
-------- | ----------- | --------
taulukon nimi | Oracle-tietokantaan, joka viittaa linkitetyn palvelun taulukon nimi. | Ei (Jos **oracleReaderQuery** **OracleSource** on määritetty)

## <a name="oracle-copy-activity-type-properties"></a>Oracle kopioi tehtävän ominaisuudet

Täydellinen luettelo osat ja ominaisuudet, jotka ovat käytettävissä toiminnan määrittäminen [Putkistot luomisesta](data-factory-create-pipelines.md) on artikkelissa. Ominaisuudet, kuten nimi, kuvaus sekä syöttö- ja taulukot ja käytäntö ovat käytettävissä kaikki tehtävälajit. 

> [AZURE.NOTE]
Kopioi tehtävän tulee vain yhden syötteen ja tuottaa vain yksi tulos.

Tehtävän typeProperties-osassa käytettävissä olevat ominaisuudet vaihtelevat toisaalta kunkin tehtävätyyppi. Kopioi tehtävälle ne vaihtelevat sen mukaan, lähteiden ja poistumia.

### <a name="oraclesource"></a>OracleSource
Kopioi toiminnossa lähteen tyyppi **OracleSource** on seuraavat ominaisuudet ovat käytettävissä **typeProperties** -osassa:

Ominaisuus | Kuvaus |Sallittu arvo | Pakollinen
-------- | ----------- | ------------- | --------
oracleReaderQuery | Mukautetun kyselyn avulla voit lukea tietoja. | SQL-kyselymerkkijonon. Esimerkki: Valitse *MyTable- <br/> <br/>Jos ei määritetä, SQL-lause, joka suoritetaan: Valitse* -MyTable | Ei (Jos **taulukon nimi** , **tietojoukko** on määritetty)

### <a name="oraclesink"></a>OracleSink
**OracleSink** tukee seuraavia ominaisuuksia:

Ominaisuus | Kuvaus | Sallittu arvo | Pakollinen
-------- | ----------- | -------------- | --------
writeBatchTimeout | Erän Lisää toiminnon suorittaminen loppuun, ennen kuin se aikakatkaistaan odotusaika. | aikajakson<br/><br/> Esimerkki: 00:30:00 (30 minuuttia). | Ei
writeBatchSize | Lisää tiedot SQL-taulukkoon, kun puskurikoko saavuttaa writeBatchSize.   | Kokonaisluku (rivien määrä)| Ei (oletus: 10000)  
sqlWriterCleanupScript | Määritä kysely kopioi tehtävän suorittamiseen siten, että Siivotaan tiedot tietyn muotoilu. | Kysely-lause. | Ei
sliceIdentifierColumnName | Määritä kopioi tehtävän muodostetaan automaattisesti sektoria tunnus, jota käytetään tiettyjen muotoilu, kun uudelleen tietojen tyhjennys täyttää sarakkeen nimi. | Sarake, jonka tietotyypin binary(32) sarakkeen nimi. | Ei


[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

### <a name="type-mapping-for-oracle"></a>Oracle yhdistäminen tyyppi

Mainitun [tietojen siirtämistä toimintoja](data-factory-data-movement-activities.md) artikkelissa kopioi tehtävän suorittaa Automaattinen tyypin muunnoksia lähteen tyyppiä vastaamaan allas tyypit seuraavista 2 vaihe vaiheelta kanssa:

1. Muuntaminen alkuperäisen tietolähteen tyypit .NET-tyyppi
2. .NET tyypistä muuntaminen alkuperäisen käsittelytoiminto tyyppi

Kun tietojen siirtämisestä Oracle seuraavat määritykset käytetään Oracle-tietotyyppi .NET tyyppi ja päinvastoin.

Oracle-tietotyyppi | .NET framework-tietotyyppi
---------------- | ------------------------
BFILE | Byte]
BLOB | Byte]
MERKKI | Merkkijono
CLOB | Merkkijono
PÄIVÄMÄÄRÄ | Päivämäärä ja aika
LIUKULUKU | Desimaaliluku
KOKONAISLUKU | Desimaaliluku
AIKAVÄLIN VUODEN, KUUKAUDEN AKTIVOIMINEN | Int32
TOISEN PÄIVÄN VÄLI | Aikajakson
PITKÄ | Merkkijono
PITKÄ RAAKA | Byte]
NCHAR | Merkkijono
NCLOB | Merkkijono
NUMERO | Desimaaliluku
NVARCHAR2 | Merkkijono
RAAKA | Byte]
ROWID | Merkkijono
AIKALEIMA | Päivämäärä ja aika
AIKALEIMA PAIKALLISEN AIKAVYÖHYKKEEN KANSSA | Päivämäärä ja aika
AIKALEIMA JA AIKAVYÖHYKE | Päivämäärä ja aika
ETUMERKITÖN KOKONAISLUKU | Numero
VARCHAR2 | Merkkijono
XML | Merkkijono

## <a name="troubleshooting-tips"></a>Vianmääritysvihjeitä

**Ongelma:** Näyttöön tulee seuraava **virhesanoma**: kopioi tehtävän täyttää virheelliset parametrit: "UnknownParameterName", yksityiskohtaiset viesti: ei löydy pyydetty .net Framework-tietopalvelu. Se ei ehkä ole asennettu".  

**Mahdollisia syitä:**

1. .NET Framework Data Provider for Oracle ei ole asennettu.
2. .NET Framework Data Provider for Oracle on asennettu .NET Framework 2.0 ja .NET Framework 4.0-kansioita ei löydy. 

**Tarkkuus Vaihtoehtoinen menetelmä:**

1. Jos ole vielä asentanut .NET Provider for Oracle, [Asenna se](http://www.oracle.com/technetwork/topics/dotnet/downloads/) ja yritä uudelleen skenaariota. 
2. Jos saat virheilmoituksen jopa palveluntarjoajan asentamisen jälkeen, toimi seuraavasti: 
    1. Avaa tietokoneen config, .NET 2.0-kansiosta: <system disk>: \Windows\Microsoft.NET\Framework64\v2.0.50727\CONFIG\machine.config.
    2. Etsi **.NET Frameworkin Oracle-tietopalvelu**ja sinulla pitäisi olla löydät merkinnän, kuten seuraava näyte unwn, on seuraavissa Esimerkki under **system.data** -> **DbProviderFactories**:
            “<add name="Oracle Data Provider for .NET" invariant="Oracle.DataAccess.Client" description="Oracle Data Provider for .NET" type="Oracle.DataAccess.Client.OracleClientFactory, Oracle.DataAccess, Version=2.112.3.0, Culture=neutral, PublicKeyToken=89b483f429c47342" />”
2.  Kopioi tämän arvon v4.0 seuraavaan kansioon machine.config-tiedostoon: <system disk>: \Windows\Microsoft.NET\Framework64\v4.0.30319\Config\machine.config hiiren kakkospainikkeella ja muuta 4.xxx.x.x-versiota.
3.  Asenna "< ODP.NET asennuspolku > \11.2.0\client_1\odp.net\bin\4\Oracle.DataAccess.dll" kokoonpanovälimuistiin (GAC) suorittamalla `gacutil /i [provider path]`.



[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]


## <a name="performance-and-tuning"></a>Suorituskyky ja säätäminen  
Lisätietoja on artikkelissa [Kopioi tehtävän suorituskyvyn & säätäminen opas](data-factory-copy-activity-performance.md) lisätietoja tärkeimmät seikat, vaikutus suorituskykyä tietojen siirto (kopioi tehtävä) Azure Data Factory ja voidaan optimoida sen usealla tavalla.
