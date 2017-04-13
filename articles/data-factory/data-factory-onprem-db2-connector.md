<properties 
    pageTitle="Siirrä tiedot DB2 | Azure Data Factory" 
    description="Lisätietoja on artikkelissa tietojen siirtäminen DB2-tietokannasta Azure Data Factory" 
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
    ms.date="09/06/2016" 
    ms.author="jingwang"/>

# <a name="move-data-from-db2-using-azure-data-factory"></a>Siirrä tiedot DB2 käyttämällä Azure Data Factory
Tässä artikkelissa käsitellään käyttämisestä kopioi tehtävän Azure tietojen factory-Siirrä tiedot toiseen tietoihin DB2-kaupasta. Tässä artikkelissa perustuu [tietojen siirtämistä tehtävät](data-factory-data-movement-activities.md) -artikkelista, jossa näkyy yleiskatsaus tietojen siirto kopioi tehtävän ja tuettuja kaupan yhdistelmät.

Tietoja factory tukee yhteyden muodostaminen paikalliseen DB2 lähteistä käyttämällä Data Management Gateway. Lisätietoja Data Management Gateway ja vaiheittaiset ohjeet yhdyskäytävän määrittäminen [paikallista sijainnit ja cloud siirtäminen tietojen](data-factory-move-data-between-onprem-and-cloud.md) artikkelista. 

> [AZURE.NOTE]
Yhdyskäytävän avulla voit muodostaa yhteyden DB2, vaikka sitä isännöidään Azure IaaS VMs. Jos yrität muodostaa esiintymään DB2 ylläpidettävä cloud, voit asentaa yhdyskäytävän esiintymä-IaaS AM.

Tietoja factory tukee tällä hetkellä vain siirtäminen tiedot DB2 muiden tietojen stores, ei muiden tietojen myymälät DB2. 

## <a name="installation"></a>Asennus 

Data Management Gateway on valmiita DB2-ohjain, joka tukee seuraavasti: 

- SQLAM 9 / 10 / 11
- LUW (Linux, Unix-Windows) DB2
- Z/OS ja DB2 for DB2 voin (eli kuin / 400)

Tämän vuoksi et enää tarvitse asentaminen manuaalisesti ohjaimet, kun tietojen kopioiminen DB2.

> [AZURE.NOTE] Artikkelissa [vianmääritys yhdyskäytävän ongelmien](data-factory-data-management-gateway.md#troubleshoot-gateway-issues) vianmääritys yhteyden/yhdyskäytävän vihjeitä liittyvät ongelmat. 

## <a name="copy-data-wizard"></a>Ohjattu tietojen kopioiminen
Helpoin tapa luoda myyntijakso, kopioi tiedot DB2-tietokantaan on käyttämällä ohjattua Kopioi tiedot. Katso [Opetusohjelma: Luo kopio ohjatulla putkijohto](data-factory-copy-data-wizard-tutorial.md) nopeasti käyttämällä ohjattua tietojen kopioiminen putkijohto luomisesta saat. 

Seuraavissa esimerkeissä otoksen JSON määritykset, jonka avulla voit luoda putkijohto [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) tai [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) tai [PowerShellin Azure](data-factory-copy-activity-tutorial-using-powershell.md). Ne näyttävät tietojen kopioiminen DB2-tietokantaan ja Azure-Blob-objektien tallennustilaan. Tietoja voi kuitenkin kopioida johonkin seuraavista poistumia määritetyllä tavalla [tähän](data-factory-data-movement-activities.md#supported-data-stores) kopio-toimintojen käyttäminen Azure Data Factory.

## <a name="sample-copy-data-from-db2-to-azure-blob"></a>Esimerkki: Tietojen kopioiminen DB2 Azure-Blob

Tässä esimerkissä tietojen kopioiminen Azure-Blob-objektien tallennustilaan paikallisen DB2-tietokantaan. Tietoja voi kuitenkin kopioidun **suoraan** johonkin seuraavista poistumia määritetyllä tavalla [tähän](data-factory-data-movement-activities.md#supported-data-stores) kopio-toimintojen käyttäminen Azure Data Factory.  
 
Malli on tietoja factory-kohteet:

1.  Linkitetyn palvelu [OnPremisesDb2](data-factory-onprem-db2-connector.md#db2-linked-service-properties)tyyppi.
2.  Linkitetyn palvelu [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties)tyyppi. 
3.  Syötteen [tietojoukko](data-factory-create-datasets.md) tyypin [RelationalTable](data-factory-onprem-db2-connector.md#db2-dataset-type-properties).
4.  Tulosteen [tietojoukko](data-factory-create-datasets.md) tyypin [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties). 
5.  [Myyntijakso](data-factory-create-pipelines.md) kopioi aktiviteettiin, joka käyttää [RelationalSource](data-factory-onprem-db2-connector.md#db2-copy-activity-type-properties) ja [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties). 

Otosten kopioi tiedot kyselyn tuloksen DB2-tietokantaan Azure-blob kerran tunnissa. Käyttää näitä esimerkkejä JSON-ominaisuuksia on kuvattu osia seuraavat mallit. 

Ensimmäisessä vaiheessa Asenna ja määritä data management Gatewayn. Ohjeet ovat [paikallisen sijainnit ja cloud siirtäminen tietojen](data-factory-move-data-between-onprem-and-cloud.md) artikkelin.

**Linkitetty DB2-palvelu:**

    {
        "name": "OnPremDb2LinkedService",
        "properties": {
            "type": "OnPremisesDb2",
            "typeProperties": {
                "server": "<server>",
                "database": "<database>",
                "schema": "<schema>",
                "authenticationType": "<authentication type>",
                "username": "<username>",
                "password": "<password>",
                "gatewayName": "<gatewayName>"
            }
        }
    }


**Azure-Blob-säiliö linkitetty palvelu:**

    {
        "name": "AzureStorageLinkedService",
        "properties": {
            "type": "AzureStorageLinkedService",
            "typeProperties": {
                "connectionString": "DefaultEndpointsProtocol=https;AccountName=<AccountName>;AccountKey=<AccountKey>"
            }
        }
    }

**DB2 syötteen tietojoukko:**

Otosten oletetaan, että olet luonut taulukon "MyTable" DB2 ja se sisältää sarake nimeltä "aikaleima" sarjan Aikatietojen.

Asetus "ulkoinen": TOSI ilmoittaa Data Factory-palvelu, että tämä tietojoukko on ulkoiset tiedot-factory ja tietojen factory toimintaa ei tuottamat. Huomaa, että **tyyppi** on määritetty **RelationalTable**. 

    {
        "name": "Db2DataSet",
        "properties": {
            "type": "RelationalTable",
            "linkedServiceName": "OnPremDb2LinkedService",
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


**Azure-Blob tulosteen tietojoukko:**

Tietoja kirjoitetaan uusia Blob-objektien tunnissa (taajuus: tunti, väli: 1). Kansiopolun blob arvioidaan dynaamisesti sektoria, käsitellään alkamispäivää perusteella. Kansiopolku käytetään vuoden, kuukauden, päivän ja alkamisajan tuntia osat.

    {
        "name": "AzureBlobDb2DataSet",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/db2/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

**Kopioi aktiviteettiin putkijohto:**

Putkisto sisältää kopio-toiminto, joka on määritetty käyttämään syöttö- ja tietojoukkoja ja on suunniteltu toimimaan tunnissa. Myyntijakso JSON määritelmä- **lähteen** tyyppi on määritetty **RelationalSource** ja **käsittelytoiminto** tyyppi-asetukseksi on määritetty **BlobSink**. SQL-kyselyn, **kysely** -ominaisuuden valitsee Tilaukset-taulukon tiedot.

    {
        "name": "CopyDb2ToBlob",
        "properties": {
            "description": "pipeline for copy activity",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "RelationalSource",
                            "query": "select * from \"Orders\""
                        },
                        "sink": {
                            "type": "BlobSink"
                        }
                    },
                    "inputs": [
                        {
                            "name": "Db2DataSet"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobDb2DataSet"
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
                    "name": "Db2ToBlob"
                }
            ],
            "start": "2014-06-01T18:00:00Z",
            "end": "2014-06-01T19:00:00Z"
        }
    }


## <a name="db2-linked-service-properties"></a>Linkitetty DB2 palveluominaisuudet

Seuraavassa taulukossa on kuvaus JSON elementtien tietyn linkitetty DB2-palveluun. 

| Ominaisuus | Kuvaus | Pakollinen |
| -------- | ----------- | -------- | 
| tyyppi | Type-ominaisuus on määritettävä: **OnPremisesDB2** | Kyllä |
| palvelin | DB2-palvelimen nimi. | Kyllä |
| tietokannan | DB2-tietokantaan nimi. | Kyllä |
| rakenne | Tietokannan rakenteen nimi. Rakenteen nimen kirjainkoko on merkitsevä. | Ei |
| authenticationType | Todentamistapa muodostaa DB2-tietokantaan. Mahdolliset arvot ovat: anonyymi, Basic ja Windows. | Kyllä |
| käyttäjänimi | Määritä käyttäjänimi, jos käytössäsi on Basic- tai Windows-todennusta. | Ei |
| salasana | Määritä käyttäjänimi määritetyn käyttäjätilin salasanan. | Ei |
| gatewayName | Data Factory-palvelun avulla yhteyden muodostaminen paikalliseen DB2-tietokantaan, yhdyskäytävän nimeä. | Kyllä |

Saat lisätietoja määrittäminen paikallista DB2-tietolähteen tunnistetiedot [määrittää käyttöoikeudet ja suojaus](data-factory-move-data-between-onprem-and-cloud.md#set-credentials-and-security) . 


## <a name="db2-dataset-type-properties"></a>DB2 tietojoukko ominaisuudet

Täydellinen luettelo osien ja ominaisuudet, jotka ovat käytettävissä määrittäminen tietojoukkoja [luominen tietojoukkoja](data-factory-create-datasets.md) on artikkelissa. Osia, kuten rakenne, käytettävyys ja tietojoukko JSON käytännön muistuttavat tyypeissä tietojoukko (Azure SQL Azure-blob, Azuren taulukkojen jne.).

TypeProperties-osassa on eri mistäkin tietojoukko, ja se antaa tietoja tietojen tietovaraston sijainti. Tyyppi (joka sisältää DB2 tietojoukko) RelationalTable tietojoukko typeProperties osiossa on seuraavat ominaisuudet.

| Ominaisuus | Kuvaus | Pakollinen |
| -------- | ----------- | -------- | 
| taulukon nimi | Taulukko, joka viittaa linkitetyistä DB2-tietokanta-esiintymän nimi. Taulukon nimi on merkitsevä. | Ei (Jos **RelationalSource** **kysely** on määritetty) |

## <a name="db2-copy-activity-type-properties"></a>DB2 kopioi tehtävän ominaisuudet

Täydellinen luettelo osat ja ominaisuudet, jotka ovat käytettävissä toiminnan määrittäminen [Putkistot luomisesta](data-factory-create-pipelines.md) on artikkelissa. Ominaisuudet, kuten nimi, kuvaus sekä syöttö- ja taulukot ja käytännöt ovat käytettävissä kaikki tehtävälajit. 

Tehtävän typeProperties-osassa käytettävissä olevat ominaisuudet vaihtelevat toisaalta kunkin tehtävätyyppi. Kopioi tehtävälle ne vaihtelevat sen mukaan, lähteiden ja poistumia.

Kopioi tehtävälle lähteen tyyppi **RelationalSource** (joka sisältää DB2) on seuraavat ominaisuudet ovat käytettävissä typeProperties-osassa:


| Ominaisuus | Kuvaus | Sallittu arvo | Pakollinen |
| -------- | ----------- | -------- | -------------- |
| kyselyn | Mukautetun kyselyn avulla voit lukea tietoja. | SQL-kyselymerkkijonon. Esimerkki: `"query": "select * from "MySchema"."MyTable""`. | Ei (Jos **taulukon nimi** , **tietojoukko** on määritetty)|

> [AZURE.NOTE] Rakenne ja taulukon nimien kirjainkoko on merkitsevä. Valitse nimet "" (lainausmerkki) kyselyssä.  

**Esimerkki:**

    "query": "select * from "DB2ADMIN"."Customers""


[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## <a name="type-mapping-for-db2"></a>Kirjoita DB2 yhdistäminen
[Tietojen siirtämistä toiminnot](data-factory-data-movement-activities.md) on artikkelissa mainittu kopioi tehtävän suorittaa Automaattinen tyypin muunnoksia lähteen tyyppiä vastaamaan allas tyypit seuraavista 2 vaihe vaiheelta kanssa:

1. Muuntaminen alkuperäisen tietolähteen tyypit .NET-tyyppi
2. .NET tyypistä muuntaminen alkuperäisen käsittelytoiminto tyyppi

Kun siirrät tietoja DB2, seuraavat määritykset käytetään DB2 tyypistä .NET tyyppi.

DB2-tietokantaan tyyppi | .NET framework-tyyppi 
----------------- | ------------------- 
SmallInt | Int16
Kokonaisluku | Int32
BigInt | Int64
Todellinen | Yksittäisen
Kaksinkertainen | Kaksinkertainen
Liukuluku | Kaksinkertainen
Desimaaliluku | Desimaaliluku
DecimalFloat | Desimaaliluku
Numeerinen | Desimaaliluku
Päivämäärä | Päivämäärä ja aika
Aika | Aikajakson
Aikaleima | Päivämäärä ja aika
XML | Byte]
Merkki | Merkkijono
VarChar | Merkkijono
LongVarChar | Merkkijono
DB2DynArray | Merkkijono
Binaarinen | Byte]
VarBinary | Byte]
LongVarBinary | Byte]
Kuva | Merkkijono
VarGraphic | Merkkijono
LongVarGraphic | Merkkijono
CLOB | Merkkijono
BLOB | Byte]
DbClob | Merkkijono
SmallInt | Int16
Kokonaisluku | Int32
BigInt | Int64
Todellinen | Yksittäisen
Kaksinkertainen | Kaksinkertainen
Liukuluku | Kaksinkertainen
Desimaaliluku | Desimaaliluku
DecimalFloat | Desimaaliluku
Numeerinen | Desimaaliluku
Päivämäärä | Päivämäärä ja aika
Aika | Aikajakson
Aikaleima | Päivämäärä ja aika
XML | Byte]
Merkki | Merkkijono


[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]


[AZURE.INCLUDE [data-factory-type-repeatability-for-relational-sources](../../includes/data-factory-type-repeatability-for-relational-sources.md)]

## <a name="performance-and-tuning"></a>Suorituskyky ja säätäminen  
Lisätietoja on artikkelissa [Kopioi tehtävän suorituskyvyn & säätäminen opas](data-factory-copy-activity-performance.md) lisätietoja tärkeimmät seikat, vaikutus suorituskykyä tietojen siirto (kopioi tehtävä) Azure Data Factory ja voidaan optimoida sen usealla tavalla.


