<properties 
    pageTitle="Siirrä tiedot MySQL | Azure Data Factory" 
    description="Lisätietoja tietojen siirtäminen MySQL-tietokannasta Azure Data Factory." 
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

# <a name="move-data-from-mysql-using-azure-data-factory"></a>Siirrä tiedot-MySQL Azure Data Factory käyttäminen

Tässä artikkelissa käsitellään käyttämisestä kopioi tehtävän Azure tietojen factory-Siirrä tiedot toiseen tietoihin MySQL-kaupasta. Tässä artikkelissa perustuu [tietojen siirtämistä tehtävät](data-factory-data-movement-activities.md) -artikkelista, jossa näkyy yleiskatsaus tietojen siirto kopioi tehtävän ja tuettuja kaupan yhdistelmät.

Tietoja Factory-palvelu tukee yhteyden muodostaminen paikalliseen MySQL sisältölähteiden käyttämällä Data Management Gateway. Lisätietoja Data Management Gateway ja vaiheittaiset ohjeet yhdyskäytävän määrittäminen [paikallista sijainnit ja cloud siirtäminen tietojen](data-factory-move-data-between-onprem-and-cloud.md) artikkelista. 

> [AZURE.NOTE] Yhdyskäytävän tarvitaan, vaikka MySQL-tietokantaan isännöidään Azure IaaS virtuaalikoneen (AM). Voit asentaa yhdyskäytävän kuin tietovaraston saman AM tai eri AM, kunhan yhdyskäytävän muodostaa yhteyden tietokantaan.  

Tietoja factory tukee tällä hetkellä vain siirtäminen tiedot MySQL ja muiden tietojen stores, mutta et tietojen siirtäminen MySQL tietoja muiden tallennetuista tiedoista.

## <a name="installation"></a>Asennus 
Data Management Gatewayn muodostaminen MySQL-tietokantaan sinun täytyy asentaa [MySQL yhdistimen tai verkon 6.6.5 Microsoft Windowsin](http://go.microsoft.com/fwlink/?LinkId=278885) saman järjestelmässä Data Management Gateway on.

> [AZURE.NOTE] Artikkelissa [vianmääritys yhdyskäytävän ongelmien](data-factory-data-management-gateway.md#troubleshoot-gateway-issues) vianmääritys yhteyden/yhdyskäytävän vihjeitä liittyvät ongelmat. 

## <a name="copy-data-wizard"></a>Ohjattu tietojen kopioiminen
Helpoin tapa luoda myyntijakso, kopioi tiedot MySQL-tietokantaan johonkin seuraavista tuetuista käsittelytoiminto tietoja tallennetuista tiedoista on käyttämällä ohjattua Kopioi tiedot. Katso [Opetusohjelma: Luo kopio ohjatulla putkijohto](data-factory-copy-data-wizard-tutorial.md) nopeasti käyttämällä ohjattua tietojen kopioiminen putkijohto luomisesta saat. 

Seuraavassa esimerkissä on esimerkki JSON määritelmiä, joiden avulla voit luoda putkijohto. Valmis vaiheittaisissa ohjeissa on vaiheittaiset ohjeet [paikallisen sijainnit ja cloud siirtäminen tietojen](data-factory-move-data-between-onprem-and-cloud.md) artikkelista. Tietoja voi kopioida johonkin seuraavista poistumia määritetyllä tavalla [tähän](data-factory-data-movement-activities.md#supported-data-stores) kopio-toimintojen käyttäminen Azure Data Factory.   

## <a name="sample-copy-data-from-mysql-to-azure-blob"></a>Esimerkki: Tietojen kopioiminen MySQL Azure-Blob
Tässä esimerkissä esitetään, kuinka haluat kopioida tiedot Azure-Blob-objektien tallennustilaan paikallisen MySQL-tietokannasta. Tietoja voi kuitenkin kopioidun **suoraan** johonkin seuraavista poistumia määritetyllä tavalla [tähän](data-factory-data-movement-activities.md#supported-data-stores) kopio-toimintojen käyttäminen Azure Data Factory.  

> [AZURE.IMPORTANT] Tässä esimerkissä on JSON katkelmat. Vaiheittaiset ohjeet tietojen factory luomiseen ei sisällä. [Paikallisen sijainnit ja cloud siirtäminen tietojen](data-factory-move-data-between-onprem-and-cloud.md) artikkelista vaiheittaiset ohjeet. 
 
Malli on tietoja factory-kohteet:

1.  Linkitetyn palvelu [OnPremisesMySql](data-factory-onprem-mysql-connector.md#mysql-linked-service-properties)tyyppi.
2.  Linkitetyn palvelu [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties)tyyppi.
3.  Syötteen [tietojoukko](data-factory-create-datasets.md) tyypin [RelationalTable](data-factory-onprem-mysql-connector.md#mysql-dataset-type-properties).
4.  Tulosteen [tietojoukko](data-factory-create-datasets.md) tyypin [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4.  [Myyntijakso](data-factory-create-pipelines.md) kopioi aktiviteettiin, joka käyttää [RelationalSource](data-factory-onprem-mysql-connector.md#mysql-copy-activity-type-properties) ja [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties).

Otosten kopioi tiedot kyselyn tuloksen MySQL-tietokantaan blob kerran tunnissa. Käyttää näitä esimerkkejä JSON-ominaisuuksia on kuvattu osia seuraavat mallit. 

Ensimmäisessä vaiheessa asennusohjelma data management gateway. Ohjeet ovat [paikallisen sijainnit ja cloud siirtäminen tietojen](data-factory-move-data-between-onprem-and-cloud.md) -artikkelissa. 

**Linkitetty MySQL-palvelu**

    {
      "name": "OnPremMySqlLinkedService",
      "properties": {
        "type": "OnPremisesMySql",
        "typeProperties": {
          "server": "<server name>",
          "database": "<database name>",
          "schema": "<schema name>",
          "authenticationType": "<authentication type>",
          "userName": "<user name>",
          "password": "<password>",
          "gatewayName": "<gateway>"
        }
      }
    }

**Azure linkitetty tallennuspalvelu**

    {
      "name": "AzureStorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

**Syötteen MySQL-tietojoukko**

Otosten oletetaan, että olet luonut taulukon "MyTable" MySQL ja se sisältää sarake nimeltä "timestampcolumn" sarjan Aikatietojen.

Asetus "ulkoinen": "true" ilmoittaa Data Factory-palvelun, että taulukossa on ulkoiset tiedot-factory ja tietojen factory toimintaa ei tuottamat.
    
    {
        "name": "MySqlDataSet",
        "properties": {
            "published": false,
            "type": "RelationalTable",
            "linkedServiceName": "OnPremMySqlLinkedService",
            "typeProperties": {},
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true,
            "policy": {
                "externalData": {
                    "retryInterval": "00:01:00",
                    "retryTimeout": "00:10:00",
                    "maximumRetry": 3
                }
            }
        }
    }



**Azure-Blob tulosteen tietojoukko**

Tietoja kirjoitetaan uusia Blob-objektien tunnissa (taajuus: tunti, väli: 1). Kansiopolun blob arvioidaan dynaamisesti sektoria, käsitellään alkamispäivää perusteella. Kansiopolku käytetään vuoden, kuukauden, päivän ja alkamisajan tuntia osat.

    {
        "name": "AzureBlobMySqlDataSet",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/mysql/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
                "format": {
                    "type": "TextFormat",
                    "rowDelimiter": "\n",
                    "columnDelimiter": "\t"
                },
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
                ]
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }



**Kopioi aktiviteettiin putkijohto**

Putkisto sisältää kopio-toiminto, joka on määritetty käyttämään syöttö- ja tietojoukkoja ja on suunniteltu toimimaan tunnissa. Myyntijakso JSON määritelmä- **lähteen** tyyppi on määritetty **RelationalSource** ja **käsittelytoiminto** tyyppi-asetukseksi on määritetty **BlobSink**. SQL-kyselyn, **kysely** -ominaisuuden valitsee edellisen tunnin kopioitavat tiedot.
    
    {
        "name": "CopyMySqlToBlob",
        "properties": {
            "description": "pipeline for copy activity",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "RelationalSource",
                            "query": "$$Text.Format('select * from MyTable where timestamp >= \\'{0:yyyy-MM-ddTHH:mm:ss}\\' AND timestamp < \\'{1:yyyy-MM-ddTHH:mm:ss}\\'', WindowStart, WindowEnd)"
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "MySqlDataSet"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobMySqlDataSet"
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
                    "name": "MySqlToBlob"
                }
            ],
            "start": "2014-06-01T18:00:00Z",
            "end": "2014-06-01T19:00:00Z"
        }
    }



## <a name="mysql-linked-service-properties"></a>Linkitetty MySQL-palveluominaisuudet

Seuraavassa taulukossa on kuvaus JSON elementtien tietyn linkitetty MySQL-palveluun.

| Ominaisuus | Kuvaus | Pakollinen |
| -------- | ----------- | -------- | 
| tyyppi | Type-ominaisuus on määritettävä: **OnPremisesMySql** | Kyllä |
| palvelin | MySQL-palvelimen nimi. | Kyllä |
| tietokannan | MySQL-tietokantaan nimi. | Kyllä | 
| rakenne  | Tietokannan rakenteen nimi. | Ei | 
| authenticationType | Yhteyden muodostaminen MySQL-tietokantaan käyttöoikeuden tyyppi. Mahdolliset arvot ovat: anonyymi, Basic ja Windows. | Kyllä | 
| käyttäjänimi | Määritä käyttäjänimi, jos käytössäsi on Basic- tai Windows-todennusta. | Ei | 
| salasana | Määritä käyttäjänimi määritetyn käyttäjätilin salasanan. | Ei | 
| gatewayName | Data Factory-palvelun avulla yhteyden muodostaminen paikalliseen MySQL-tietokantaan, yhdyskäytävän nimeä. | Kyllä |

Saat lisätietoja määrittäminen paikallista MySQL-tietolähteen tunnistetiedot [määrittää käyttöoikeudet ja suojaus](data-factory-move-data-between-onprem-and-cloud.md#set-credentials-and-security) .

## <a name="mysql-dataset-type-properties"></a>MySQL tietojoukko ominaisuudet

Täydellinen luettelo osien ja ominaisuudet, jotka ovat käytettävissä määrittäminen tietojoukkoja [luominen tietojoukkoja](data-factory-create-datasets.md) on artikkelissa. Osia, kuten rakenne, käytettävyys ja tietojoukko JSON käytännön muistuttavat tyypeissä tietojoukko (Azure SQL Azure-blob, Azuren taulukkojen jne.).

**TypeProperties** -osassa on eri mistäkin tietojoukko, ja se antaa tietoja tietojen tietovaraston sijainti. Tyypin **RelationalTable** (joka sisältää MySQL-tietojoukko) tietojoukko typeProperties osiossa on seuraavat ominaisuudet

| Ominaisuus | Kuvaus | Pakollinen |
| -------- | ----------- | -------- |
| taulukon nimi | Taulukko, joka viittaa linkitetyistä MySQL-tietokantaan esiintymän nimi. | Ei (Jos **RelationalSource** **kysely** on määritetty) | 

## <a name="mysql-copy-activity-type-properties"></a>MySQL kopioi tehtävän ominaisuudet

Täydellinen luettelo osat ja ominaisuudet, jotka ovat käytettävissä toiminnan määrittäminen [Putkistot luomisesta](data-factory-create-pipelines.md) on artikkelissa. Ominaisuudet, kuten nimi, kuvaus ja syöttö- ja taulukot ovat käytännöt ovat käytettävissä kaikki tehtävälajit. 

Tehtävän **typeProperties** -osassa käytettävissä olevat ominaisuudet vaihtelevat toisaalta kunkin tehtävätyyppi. Kopioi tehtävälle ne vaihtelevat sen mukaan, lähteiden ja poistumia.

Kopioi tehtävän lähteen tyyppi **RelationalSource** (joka sisältää MySQL) on seuraavat ominaisuudet ovat käytettävissä typeProperties-osassa:

| Ominaisuus | Kuvaus | Sallittu arvo | Pakollinen |
| -------- | ----------- | -------------- | -------- |
| kyselyn | Mukautetun kyselyn avulla voit lukea tietoja. | SQL-kyselymerkkijonon. Esimerkki: Valitse *-MyTable. | Ei (Jos **taulukon nimi** , **tietojoukko** on määritetty) | 

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

### <a name="type-mapping-for-mysql"></a>MySQL yhdistäminen tyyppi

[Tietojen siirtämistä toiminnot](data-factory-data-movement-activities.md) on artikkelissa mainittu kopioi tehtävän suorittaa Automaattinen tyypin muunnoksia lähteen tyyppiä vastaamaan allas tyypit seuraavista kaksivaiheinen lähestymistapa kanssa:

1. Muuntaminen alkuperäisen tietolähteen tyypit .NET-tyyppi
2. .NET tyypistä muuntaminen alkuperäisen käsittelytoiminto tyyppi

Kun siirrät tietoja MySQL-seuraavat määritykset käytetään MySQL eri .NET tyypit.

| MySQL-tietokantaan tyyppi | .NET framework-tyyppi |
| ------------------- | ------------------- | 
| bigint allekirjoittamat | Desimaaliluku |
| bigint | Int64 |
| Bit | Desimaaliluku |
| BLOB | Byte] |
| bool |  Totuusarvo | 
| merkki | Merkkijono |
| päivämäärä | Päivämäärä ja aika |
| Päivämäärä ja aika | Päivämäärä ja aika |
| desimaaliluku | Desimaaliluku |
| kaksinkertainen tarkkuus | Kaksinkertainen |
| Kaksinkertainen | Kaksinkertainen |
| luettelo | Merkkijono |
| liukuluku | Yksittäisen |
| INT allekirjoittamat | Int64 |
| kokonaisluku | Int32 |
| kokonaisluku allekirjoittamat | Int64 |
| kokonaisluku | Int32 | 
| pitkä varbinary | Byte] |
| pitkä varchar | Merkkijono |
| longblob | Byte] |
| LONGTEXT | Merkkijono | 
| mediumblob |  Byte] | 
| mediumint allekirjoittamat | Int64 |
| mediumint | Int32 | 
| mediumtext | Merkkijono |
| numeerinen | Desimaaliluku |
| Todellinen |  Kaksinkertainen |
| määrittäminen | Merkkijono |
| smallint allekirjoittamat | Int32 |
| smallint | Int16 |
| teksti | Merkkijono |
| aika | Aikajakson |
| aikaleima | Päivämäärä ja aika |
| tinyblob | Byte] |
| tinyint allekirjoittamat |  Int16 | 
| tinyint | Int16 |
| tinytext | Merkkijono | 
| varchar | Merkkijono | 
| vuoden | Kokonaisluku | 


[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-type-repeatability-for-relational-sources](../../includes/data-factory-type-repeatability-for-relational-sources.md)]

## <a name="performance-and-tuning"></a>Suorituskyky ja säätäminen  
Lisätietoja on artikkelissa [Kopioi tehtävän suorituskyvyn & säätäminen opas](data-factory-copy-activity-performance.md) lisätietoja tärkeimmät seikat, vaikutus suorituskykyä tietojen siirto (kopioi tehtävä) Azure Data Factory ja voidaan optimoida sen usealla tavalla.

