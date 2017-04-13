<properties 
    pageTitle="Siirrä tiedot Teradata | Azure Data Factory" 
    description="Lisätietoja Teradata-yhdistin Data Factory-palveluun, jolla voit siirtää tietoja Teradata-tietokanta" 
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

# <a name="move-data-from-teradata-using-azure-data-factory"></a>Siirrä tiedot käyttämällä Azure Data Factory Teradata

Tässä artikkelissa käsitellään käyttämisestä kopioi tehtävän Azure tietojen factory-Siirrä tiedot toiseen tietojen Teradata-kaupasta. Tässä artikkelissa perustuu [tietojen siirtämistä tehtävät](data-factory-data-movement-activities.md) -artikkelista, jossa näkyy yleiskatsaus tietojen siirto kopioi tehtävän ja tuettuja kaupan yhdistelmät.

Tietoja factory tukee yhteyden muodostaminen paikalliseen Teradata lähteet Data Management Gateway kautta. Lisätietoja Data Management Gateway ja vaiheittaiset ohjeet yhdyskäytävän määrittäminen [paikallista sijainnit ja cloud siirtäminen tietojen](data-factory-move-data-between-onprem-and-cloud.md) artikkelista. 

> [AZURE.NOTE]
Yhdyskäytävän tarvitaan, vaikka Teradata isännöidään Azure-IaaS AM. Voit asentaa yhdyskäytävän saman IaaS AM, kuin tietovaraston tai eri AM, kunhan yhdyskäytävän muodostaa yhteyden tietokantaan. 

Tietoja factory tukee vain siirtäminen tiedot Teradata muiden tietojen stores, ei muiden tietojen myymälät Teradata.

## <a name="installation"></a>Asennus 

Data Management Gatewayn muodostaminen Teradata-tietokantaan sinun täytyy asentaa [.NET Teradata-tietopalvelu](http://go.microsoft.com/fwlink/?LinkId=278886) saman järjestelmässä Data Management Gateway on.

> [AZURE.NOTE] Artikkelissa [vianmääritys yhdyskäytävän ongelmien](data-factory-data-management-gateway.md#troubleshoot-gateway-issues) vianmääritys yhteyden/yhdyskäytävän vihjeitä liittyvät ongelmat. 

## <a name="copy-data-wizard"></a>Ohjattu tietojen kopioiminen
Helpoin tapa luoda myyntijakso, kopioi tiedot Teradata johonkin seuraavista tuetuista käsittelytoiminto tietoja tallennetuista tiedoista on käyttämällä ohjattua Kopioi tiedot. Katso [Opetusohjelma: Luo kopio ohjatulla putkijohto](data-factory-copy-data-wizard-tutorial.md) nopeasti käyttämällä ohjattua tietojen kopioiminen putkijohto luomisesta saat. 

Seuraavassa esimerkissä on esimerkki JSON määritykset, jonka avulla voit luoda putkijohto [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) tai [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) tai [PowerShellin Azure](data-factory-copy-activity-tutorial-using-powershell.md). Ne näyttävät tietojen kopioiminen Azure-Blob-säiliö Teradata. Tietoja voi kuitenkin kopioida johonkin seuraavista poistumia määritetyllä tavalla [tähän](data-factory-data-movement-activities.md#supported-data-stores) kopio-toimintojen käyttäminen Azure Data Factory.   

### <a name="sample-copy-data-from-teradata-to-azure-blob"></a>Esimerkki: Tietojen kopioiminen Teradata Azure-Blob

Tässä esimerkissä tietojen kopioiminen Azure-Blob-objektien tallennustilaan Teradata-tietokantaan. Tietoja voi kuitenkin kopioidun **suoraan** johonkin seuraavista poistumia määritetyllä tavalla [tähän](data-factory-data-movement-activities.md#supported-data-stores) kopio-toimintojen käyttäminen Azure Data Factory.  
 
Malli on tietoja factory-kohteet:

1.  Linkitetyn palvelu [OnPremisesTeradata](data-factory-onprem-teradata-connector.md#teradata-linked-service-properties)tyyppi.
2.  Linkitetyn palvelu [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties)tyyppi.
3.  Syötteen [tietojoukko](data-factory-create-datasets.md) tyypin [RelationalTable](data-factory-onprem-teradata-connector.md#teradata-dataset-type-properties).
4.  Tulosteen [tietojoukko](data-factory-create-datasets.md) tyypin [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties). 
4.  [Myyntijakso](data-factory-create-pipelines.md) kopioi aktiviteettiin, joka käyttää [RelationalSource](data-factory-onprem-teradata-connector.md#teradata-copy-activity-type-properties) ja [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties).

Otosten kopioi tiedot kyselyn tuloksen Teradata-tietokanta-blob tunnissa. Käyttää näitä esimerkkejä JSON-ominaisuuksia on kuvattu osia seuraavat mallit. 

Ensimmäisessä vaiheessa asennusohjelma data management gateway. Ohjeet ovat [paikallisen sijainnit ja cloud siirtäminen tietojen](data-factory-move-data-between-onprem-and-cloud.md) -artikkelissa.

**Linkitetty Teradata-palvelu:**

    {
        "name": "OnPremTeradataLinkedService",
        "properties": {
            "type": "OnPremisesTeradata",
            "typeProperties": {
                "server": "<server>",
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


**Teradata syötteen tietojoukko:**

Otosten oletetaan, että olet luonut taulukon "MyTable" Teradata ja se sisältää sarake nimeltä "aikaleima" sarjan Aikatietojen.

Asetus "ulkoinen": TOSI ilmoittaa Data Factory-palvelu, että taulukko on ulkoiset tiedot-factory ja tietojen factory toimintaa ei tuottamat.

    {
        "name": "TeradataDataSet",
        "properties": {
            "published": false,
            "type": "RelationalTable",
            "linkedServiceName": "OnPremTeradataLinkedService",
            "typeProperties": {
            },
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
        "name": "AzureBlobTeradataDataSet",
        "properties": {
            "published": false,
            "location": {
                "type": "AzureBlobLocation",
                "folderPath": "mycontainer/teradata/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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
                ],
                "linkedServiceName": "AzureStorageLinkedService"
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }


**Kopioi aktiviteettiin putkijohto:**

Putkisto sisältää kopio-toiminto, joka on määritetty käyttämään syöttö- ja tietojoukkoja ja on suunniteltu toimimaan kerran tunnissa. Myyntijakso JSON määritelmä- **lähteen** tyyppi on määritetty **RelationalSource** ja **käsittelytoiminto** tyyppi-asetukseksi on määritetty **BlobSink**. SQL-kyselyn, **kysely** -ominaisuuden valitsee edellisen tunnin kopioitavat tiedot.

    {
        "name": "CopyTeradataToBlob",
        "properties": {
            "description": "pipeline for copy activity",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "RelationalSource",
                            "query": "$$Text.Format('select * from MyTable where timestamp >= \\'{0:yyyy-MM-ddTHH:mm:ss}\\' AND timestamp < \\'{1:yyyy-MM-ddTHH:mm:ss}\\'', SliceStart, SliceEnd)"
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "TeradataDataSet"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobTeradataDataSet"
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
                    "name": "TeradataToBlob"
                }
            ],
            "start": "2014-06-01T18:00:00Z",
            "end": "2014-06-01T19:00:00Z",
            "isPaused": false
        }
    }


## <a name="teradata-linked-service-properties"></a>Linkitetty Teradata-palveluominaisuudet

Seuraavassa taulukossa on kuvaus JSON elementtien tietyn linkitetty Teradata-palveluun. 

Ominaisuus | Kuvaus | Pakollinen
-------- | ----------- | --------
tyyppi | Type-ominaisuus on määritettävä: **OnPremisesTeradata** | Kyllä
palvelin | Teradata-palvelimen nimi. | Kyllä
authenticationType | Yhteyden muodostaminen Teradata-tietokantaan käyttöoikeuden tyyppi. Mahdolliset arvot ovat: anonyymi, Basic ja Windows. | Kyllä
käyttäjänimi | Määritä käyttäjänimi, jos käytössäsi on Basic- tai Windows-todennusta. | Ei 
salasana | Määritä käyttäjänimi määritetyn käyttäjätilin salasanan. | Ei 
gatewayName | Data Factory-palvelun avulla yhteyden muodostaminen paikalliseen Teradata-tietokantaan, yhdyskäytävän nimeä. | Kyllä

Saat lisätietoja määrittäminen paikallista Teradata-tietolähteen tunnistetiedot [määrittää käyttöoikeudet ja suojaus](data-factory-move-data-between-onprem-and-cloud.md#set-credentials-and-security) .

## <a name="teradata-dataset-type-properties"></a>Teradata tietojoukko ominaisuudet

Täydellinen luettelo osien ja ominaisuudet, jotka ovat käytettävissä määrittäminen tietojoukkoja [luominen tietojoukkoja](data-factory-create-datasets.md) on artikkelissa. Osia, kuten rakenne, käytettävyys ja tietojoukko JSON käytännön muistuttavat tyypeissä tietojoukko (Azure SQL Azure-blob, Azuren taulukkojen jne.).

**TypeProperties** -osassa on eri mistäkin tietojoukko, ja se antaa tietoja tietojen tietovaraston sijainti. Ei tällä hetkellä ei ole tuettu Teradata-tietojoukko ominaisuudet. 


## <a name="teradata-copy-activity-type-properties"></a>Teradata kopioi tehtävän ominaisuudet

Täydellinen luettelo osat ja ominaisuudet, jotka ovat käytettävissä toiminnan määrittäminen [Putkistot luomisesta](data-factory-create-pipelines.md) on artikkelissa. Ominaisuudet, kuten nimi, kuvaus sekä syöttö- ja taulukot ja käytännöt ovat käytettävissä kaikki tehtävälajit. 

Tehtävän typeProperties-osassa käytettävissä olevat ominaisuudet vaihtelevat toisaalta kunkin tehtävätyyppi. Kopioi tehtävälle ne vaihtelevat sen mukaan, lähteiden ja poistumia.

Kun lähteen tyyppi **RelationalSource** (joka sisältää Teradata) on seuraavat ominaisuudet ovat käytettävissä **typeProperties** -osassa:

Ominaisuus | Kuvaus | Sallittu arvo | Pakollinen
-------- | ----------- | -------------- | --------
kyselyn | Mukautetun kyselyn avulla voit lukea tietoja. | SQL-kyselymerkkijonon. Esimerkki: Valitse *-MyTable. | Kyllä

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## <a name="type-mapping-for-teradata"></a>Teradata-tyypin yhdistäminen

[Tietojen siirtämistä toiminnot](data-factory-data-movement-activities.md) on artikkelissa mainittu kopioi tehtävän suorittaa Automaattinen tyypin muunnoksia lähteen tyyppiä vastaamaan allas tyypit seuraavista 2 vaihe vaiheelta kanssa:

1. Muuntaminen alkuperäisen tietolähteen tyypit .NET-tyyppi
2. .NET tyypistä muuntaminen alkuperäisen käsittelytoiminto tyyppi

Liikkuva tietojen Teradata seuraavat määritykset käytetään tyyppi on .NET Teradata-tyypistä.

Teradata-tietokanta-tyyppi | .NET framework-tyyppi
----------------- | ---------------------------
Merkki | Merkkijono
CLOB | Merkkijono
Kuva | Merkkijono
VarChar | Merkkijono
VarGraphic | Merkkijono
BLOB | Byte]
Tavu | Byte]
VarByte | Byte]
BigInt | Int64
ByteInt | Int16
Desimaaliluku | Desimaaliluku
Kaksinkertainen | Kaksinkertainen
Kokonaisluku | Int32
Numero | Kaksinkertainen
SmallInt | Int16
Päivämäärä | Päivämäärä ja aika
Aika | Aikajakson
Kellonajan ja aikavyöhykkeen | Merkkijono
Aikaleima | Päivämäärä ja aika
Aikaleima ja aikavyöhyke | DateTimeOffset-arvoa
Aikavälin päivä | Aikajakson
Aikavälin päivä tunti | Aikajakson
Minuutti aikavälin päivä | Aikajakson
Toisen päivän väli | Aikajakson
Aikavälin tunti | Aikajakson
Minuutti tunnin väli | Aikajakson
Toisen tunnin väli | Aikajakson
Aikavälin minuutti | Aikajakson
Toisen minuutin väli | Aikajakson
Aikavälin toisen | Aikajakson
Aikavälin vuosi | Merkkijono
Aikavälin vuoden, kuukauden aktivoiminen | Merkkijono
Aikavälin kuukausi | Merkkijono
Period(Date) | Merkkijono
Period(Time) | Merkkijono
Kausi (aikavyöhykkeen aikaa) | Merkkijono
Period(TimeStamp) | Merkkijono
Kausi (aikaleima aikavyöhykkeen kanssa) | Merkkijono
XML | Merkkijono

[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-type-repeatability-for-relational-sources](../../includes/data-factory-type-repeatability-for-relational-sources.md)]

## <a name="performance-and-tuning"></a>Suorituskyky ja säätäminen  
Lisätietoja on artikkelissa [Kopioi tehtävän suorituskyvyn & säätäminen opas](data-factory-copy-activity-performance.md) lisätietoja tärkeimmät seikat, vaikutus suorituskykyä tietojen siirto (kopioi tehtävä) Azure Data Factory ja voidaan optimoida sen usealla tavalla.