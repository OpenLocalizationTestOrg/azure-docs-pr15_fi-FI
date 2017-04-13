<properties 
    pageTitle="Siirrä tiedot käyttämällä Data Factory Amazon Redshift | Microsoft Azure" 
    description="Voit siirtää tietoja käyttämällä Azure Data Factory Amazon Redshift tietoja." 
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
    ms.date="08/23/2016" 
    ms.author="jingwang"/>

# <a name="move-data-from-amazon-redshift-using-azure-data-factory"></a>Siirrä tiedot-Amazon Redshift Azure Data Factory käyttäminen

Tässä artikkelissa käsitellään käyttämisestä kopioi tehtävän Azure tietojen factory-Siirrä tiedot toiseen tietoihin Redshift Amazon-kaupasta. Tässä artikkelissa perustuu [tietojen siirtämistä tehtävät](data-factory-data-movement-activities.md) -artikkelista, jossa näkyy yleiskatsaus tietojen siirto kopioi tehtävän sekä lähde/käsittelytoiminto tietojen stores luettelo.  

Tietoja factory tukee tällä hetkellä vain siirtäminen tiedot Amazon Redshift ja muiden tietojen stores, mutta et tietojen siirtäminen Amazon Redshift tietoja muiden tallennetuista tiedoista.

## <a name="prerequisites"></a>Edellytykset

- Jos tiedot ovat Siirry paikallisen-tietovarasto ja myönnä Data Management Gateway (Käytä IP-osoite koneen) Amazon Redshift klusterin käyttöoikeus. Saat ohjeita [Hyväksy käyttöoikeus klusterin](http://docs.aws.amazon.com/redshift/latest/gsg/rs-gsg-authorize-cluster-access.html) . 
- Jos tiedot ovat Siirry Azure tietovaraston ja saat Laske IP-osoitealueet (mukaan lukien SQL-alueet) käyttämän Microsoft Azure tietojen keskikohdan mukaan [Azure Data Centerissä IP-alueita](https://www.microsoft.com/download/details.aspx?id=41653) . 

## <a name="copy-data-wizard"></a>Ohjattu tietojen kopioiminen
Helpoin tapa luoda myyntijakso, kopioi tiedot Amazon Redshift on käyttämällä ohjattua Kopioi tiedot. Katso [Opetusohjelma: Luo kopio ohjatulla putkijohto](data-factory-copy-data-wizard-tutorial.md) nopeasti käyttämällä ohjattua tietojen kopioiminen putkijohto luomisesta saat. 

Seuraavassa esimerkissä on esimerkki JSON määritykset, jonka avulla voit luoda putkijohto [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) tai [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) tai [PowerShellin Azure](data-factory-copy-activity-tutorial-using-powershell.md). Sen avulla voit kopioida tietoja Amazon Redshift Azure-Blob-objektien tallennustilaan. Tietoja voi kopioida kuitenkin johonkin seuraavista poistumia määritetyllä tavalla [tähän](data-factory-data-movement-activities.md#supported-data-stores).

## <a name="sample-copy-data-from-amazon-redshift-to-azure-blob"></a>Esimerkki: Tietojen kopioiminen Amazon Redshift Azure-Blob
Tässä esimerkissä esitetään, kuinka haluat kopioida tiedot Azure-Blob-objektien tallennustilaan Amazon Redshift-tietokannasta. Tietoja voi kuitenkin kopioidun **suoraan** johonkin seuraavista poistumia määritetyllä tavalla [tähän](data-factory-data-movement-activities.md#supported-data-stores) kopio-toimintojen käyttäminen Azure Data Factory.  
 
Malli on tietoja factory-kohteet:

- Linkitetyn palvelu [AmazonRedshift](#linked-service-properties)tyyppi.
- Linkitetyn palvelu [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties)tyyppi.
- Syötteen [tietojoukko](data-factory-create-datasets.md) tyypin [RelationalTable](#dataset-type-properties).
- Tulosteen [tietojoukko](data-factory-create-datasets.md) tyypin [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
- [Myyntijakso](data-factory-create-pipelines.md) kopioi aktiviteettiin, joka käyttää [RelationalSource](#copy-activity-type-properties) ja [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties).

Otosten kopioi tiedot kyselyn tuloksen Amazon Redshift-blob tunnissa. Käyttää näitä esimerkkejä JSON-ominaisuuksia on kuvattu osia seuraavat mallit. 

**Amazon Redshift linkitetty-palvelu**

    {
        "name": "AmazonRedshiftLinkedService",
        "properties":
        {
            "type": "AmazonRedshift",
            "typeProperties":
            {
                "server": "< The IP address or host name of the Amazon Redshift server >",
                "port": <The number of the TCP port that the Amazon Redshift server uses to listen for client connections.>,
                "database": "<The database name of the Amazon Redshift database>",
                "username": "<username>",
                "password": "<password>"
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

**Amazon Redshift näytettävä tietojoukko**

Määrittäminen **"ulkoinen": TOSI** ilmoittaa Data Factory-palvelun dataset on ulkoiset tiedot-factory ja tietojen factory toimintaa ei tuottamat. Määrittää tämän ominaisuuden arvoksi true syötteen tietojoukko, joka ei ole esitetty putkijohto toimintaa.

    {
        "name": "AmazonRedshiftInputDataset",
        "properties": {
            "type": "RelationalTable",
            "linkedServiceName": "AmazonRedshiftLinkedService",
            "typeProperties": {
                "tableName": "<Table name>"
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true
        }
    }


**Azure-Blob tulosteen tietojoukko**

Tietoja kirjoitetaan uusia Blob-objektien tunnissa (taajuus: tunti, väli: 1). Kansiopolun blob arvioidaan dynaamisesti sektoria, käsitellään alkamispäivää perusteella. Kansiopolku käytetään vuoden, kuukauden, päivän ja alkamisajan tuntia osat.

    {
        "name": "AzureBlobOutputDataSet",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/fromamazonredshift/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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
        "name": "CopyAmazonRedshiftToBlob",
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
                            "name": "AmazonRedshiftInputDataset"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobOutputDataSet"
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
                    "name": "AmazonRedshiftToBlob"
                }
            ],
            "start": "2014-06-01T18:00:00Z",
            "end": "2014-06-01T19:00:00Z"
        }
    }



## <a name="linked-service-properties"></a>Linkitetyn palveluominaisuudet

Seuraavassa taulukossa on kuvaus JSON elementtien tietyn Amazon Redshift linkitetty-palveluun.

| Ominaisuus | Kuvaus | Pakollinen |
| -------- | ----------- | -------- | 
| tyyppi | Type-ominaisuus on määritettävä: **AmazonRedshift**. | Kyllä |
| palvelin | IP-osoite tai host Amazon Redshift palvelimen nimi. | Kyllä |
| Port (portti) | Amazon Redshift palvelimen käyttämän kuunteleminen asiakasyhteydet TCP-portin määrä. | Ei, oletusarvo: 5439 |
| tietokannan | Amazon Redshift tietokannan nimi. | Kyllä |
| käyttäjänimi | Käyttäjä, jolla on pääsy tietokantaan nimi.| Kyllä |
| salasana | Käyttäjätilin salasanan.| Kyllä |


## <a name="dataset-type-properties"></a>Tietojoukon ominaisuudet

Täydellinen luettelo osien ja ominaisuudet, jotka ovat käytettävissä määrittäminen tietojoukkoja [luominen tietojoukkoja](data-factory-create-datasets.md) on artikkelissa. Osia, kuten rakenne, käytettävyys ja käytännön muistuttavat tyypeissä tietojoukko (Azure SQL Azure-blob, Azuren taulukkojen jne.).

**TypeProperties** -osassa on eri mistäkin tietojoukko, ja se antaa tietoja tietojen tietovaraston sijainti. Tyypin **RelationalTable** (joka sisältää Amazon Redshift tietojoukko) tietojoukko typeProperties osiossa on seuraavat ominaisuudet

| Ominaisuus | Kuvaus | Pakollinen |
| -------- | ----------- | -------- |
| taulukon nimi | Taulukko, joka viittaa linkitetyistä Amazon Redshift tietokannan nimi. | Ei (Jos **RelationalSource** **kysely** on määritetty) | 

## <a name="copy-activity-type-properties"></a>Kopioi tehtävän ominaisuudet

Täydellinen luettelo osat ja ominaisuudet, jotka ovat käytettävissä toiminnan määrittäminen [Putkistot luomisesta](data-factory-create-pipelines.md) on artikkelissa. Ominaisuudet, kuten nimi, kuvaus sekä syöttö- ja taulukot ja käytännöt ovat käytettävissä kaikki tehtävälajit. 

Tehtävän **typeProperties** -osassa käytettävissä olevat ominaisuudet vaihtelevat toisaalta kunkin tehtävätyyppi. Kopioi tehtävälle ne vaihtelevat sen mukaan, lähteiden ja poistumia.

Kopioi tehtävän lähteen tyyppi **RelationalSource** (joka sisältää Amazon Redshift) on seuraavat ominaisuudet ovat käytettävissä typeProperties-osassa:

| Ominaisuus | Kuvaus | Sallittu arvo | Pakollinen |
| -------- | ----------- | -------------- | -------- |
| kyselyn | Mukautetun kyselyn avulla voit lukea tietoja. | SQL-kyselymerkkijonon. Esimerkki: Valitse *-MyTable. | Ei (Jos **taulukon nimi** , **tietojoukko** on määritetty) | 

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

### <a name="type-mapping-for-amazon-redshift"></a>Amazon Redshift yhdistäminen tyyppi

[Tietojen siirtämistä toiminnot](data-factory-data-movement-activities.md) on artikkelissa mainittu kopioi tehtävän suorittaa Automaattinen tyypin muunnoksia lähteen tyyppiä vastaamaan allas tyypit seuraavista kaksivaiheinen lähestymistapa kanssa:

1. Muuntaminen alkuperäisen tietolähteen tyypit .NET-tyyppi
2. .NET tyypistä muuntaminen alkuperäisen käsittelytoiminto tyyppi

Kun siirrät tietoja Amazon Redshift, seuraavat määritykset käytetään Amazon Redshift eri .NET tyypit.

Amazon Redshift tyyppi | .NET-pohjaiset tyyppi
-------------------- | ---------------
SMALLINT | Int16
KOKONAISLUKU | Int32
BIGINT | Int64
DESIMAALILUKU | Desimaaliluku
TODELLINEN | Yksittäisen
KAKSINKERTAINEN TARKKUUS | Kaksinkertainen
TOTUUSARVO | Merkkijono
MERKKI | Merkkijono
VARCHAR | Merkkijono
PÄIVÄMÄÄRÄ | Päivämäärä ja aika
AIKALEIMA | Päivämäärä ja aika
TEKSTI | Merkkijono



[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-type-repeatability-for-relational-sources](../../includes/data-factory-type-repeatability-for-relational-sources.md)]

## <a name="performance-and-tuning"></a>Suorituskyky ja säätäminen  
Lisätietoja on artikkelissa [Kopioi tehtävän suorituskyvyn & säätäminen opas](data-factory-copy-activity-performance.md) lisätietoja tärkeimmät seikat, vaikutus suorituskykyä tietojen siirto (kopioi tehtävä) Azure Data Factory ja voidaan optimoida sen usealla tavalla.

## <a name="next-steps"></a>Seuraavat vaiheet
Seuraavissa artikkeleissa: 
- [Kopioi tehtävän opetusohjelma](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) vaiheittaiset ohjeet luomiseen putkijohto kopio-tehtävän. 