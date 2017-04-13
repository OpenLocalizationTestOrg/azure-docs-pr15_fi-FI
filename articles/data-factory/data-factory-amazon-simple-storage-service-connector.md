<properties 
    pageTitle="Siirrä tiedot Amazon yksinkertainen Tallennuspalvelu käyttämällä Data Factory | Microsoft Azure" 
    description="Lisätietoja siirtäminen tiedot käyttämällä Azure Data Factory Amazon yksinkertainen Tallennuspalvelu (S3)." 
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
    ms.date="10/24/2016" 
    ms.author="jingwang"/>

# <a name="move-data-from-amazon-simple-storage-service-using-azure-data-factory"></a>Siirrä tiedot-Amazon yksinkertainen Tallennuspalvelu Azure Data Factory käyttäminen

Tässä artikkelissa käsitellään käyttämisestä kopioi tehtävän Azure tietojen factory-siirtää tietoja Amazon yksinkertainen tallennustilan-palvelusta (S3) toisen tietojen tallentamista. Tässä artikkelissa muodostaa [tietojen siirtämistä toimintoja](data-factory-data-movement-activities.md) artikkelissa, joka esittää tietojen siirto yleiskatsaus ja tuettujen lähde/käsittelytoiminto tietojen stores luettelo kopioi aktiviteettiin.  

Tietoja factory tukee tällä hetkellä vain siirtäminen tiedot Amazon S3 ja muiden tietojen stores, mutta et tietojen siirtäminen Amazon S3 tietoja muiden tallennetuista tiedoista.

## <a name="required-permissions"></a>Tarvittavat käyttöoikeudet

Tietojen kopioiminen Amazon S3, varmista, että sinulle on annettu alla mainitut oikeudet:

- **S3:GetObject** ja **s3:GetObjectVersion** Amazon S3 objektin toimintoja varten
- **S3:ListBucket** ja **s3:ListAllMyBuckets** (käytetään vain Ohjattu kopiointi) Amazon S3 aikajakson toimintoja varten 

Löydät Amazon S3 permisions tiedot täydellisestä luettelosta [Käytännön määrittäminen käyttöoikeudet](http://docs.aws.amazon.com/AmazonS3/latest/dev/using-with-s3-actions.html).

## <a name="copy-data-wizard"></a>Ohjattu tietojen kopioiminen
Helpoin tapa luoda myyntijakso, kopioi tiedot Amazon S3 on käyttämällä ohjattua Kopioi tiedot. Katso [Opetusohjelma: Luo kopio ohjatulla putkijohto](data-factory-copy-data-wizard-tutorial.md) nopeasti käyttämällä ohjattua tietojen kopioiminen putkijohto luomisesta saat. 

Seuraavassa esimerkissä on esimerkki JSON määritykset, jonka avulla voit luoda putkijohto [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) tai [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) tai [PowerShellin Azure](data-factory-copy-activity-tutorial-using-powershell.md). Sen avulla voit kopioida tietoja Amazon S3 Azure-Blob-objektien tallennustilaan. Tietoja voi kopioida kuitenkin johonkin seuraavista poistumia määritetyllä tavalla [tähän](data-factory-data-movement-activities.md#supported-data-stores).

## <a name="sample-copy-data-from-amazon-s3-to-azure-blob"></a>Esimerkki: Tietojen kopioiminen Amazon S3 Azure-Blob
Tässä esimerkissä esitetään, kuinka haluat kopioida tiedot Amazon S3 Azure-Blob-objektien tallennustilaan. Tietoja voi kuitenkin kopioidun **suoraan** johonkin seuraavista poistumia määritetyllä tavalla [tähän](data-factory-data-movement-activities.md#supported-data-stores) kopio-toimintojen käyttäminen Azure Data Factory.  
 
Malli on tietoja factory-kohteet:

- Linkitetyn palvelu [AwsAccessKey](#linked-service-properties)tyyppi.
- Linkitetyn palvelu [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties)tyyppi.
- Syötteen [tietojoukko](data-factory-create-datasets.md) tyypin [AmazonS3](#dataset-type-properties).
- Tulosteen [tietojoukko](data-factory-create-datasets.md) tyypin [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
- [Myyntijakso](data-factory-create-pipelines.md) kopioi aktiviteettiin, joka käyttää [FileSystemSource](#copy-activity-type-properties) ja [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties).

Otosten kopioi tiedot Amazon S3 Azure-blob tunnissa. Käyttää näitä esimerkkejä JSON-ominaisuuksia on kuvattu osia seuraavat mallit. 

**Amazon S3 linkitetty-palvelu**

    {
        "name": "AmazonS3LinkedService",
        "properties": {
            "type": "AwsAccessKey",
            "typeProperties": {
                "accessKeyId": "<access key id>",
                "secretAccessKey": "<secret access key>"
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

**Amazon S3 syötteen tietojoukko**

Määrittäminen **"ulkoinen": TOSI** ilmoittaa Data Factory-palvelun dataset on ulkoiset tiedot-factory ja tietojen factory toimintaa ei tuottamat. Määrittää tämän ominaisuuden arvoksi true syötteen tietojoukko, joka ei ole esitetty putkijohto toimintaa.

    {
        "name": "AmazonS3InputDataset",
        "properties": {
            "type": "AmazonS3",
            "linkedServiceName": "AmazonS3LinkedService",
            "typeProperties": {
                "key": "testFolder/test.orc",
                "bucketName": "testbucket",
                "format": {
                    "type": "OrcFormat"
                }
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
                "folderPath": "mycontainer/fromamazons3/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

Putkisto sisältää kopio-toiminto, joka on määritetty käyttämään syöttö- ja tietojoukkoja ja on suunniteltu toimimaan tunnissa. Myyntijakso JSON määritelmä- **lähteen** tyyppi on määritetty **FileSystemSource** ja **käsittelytoiminto** tyyppi-asetukseksi on määritetty **BlobSink**. 
    
    {
        "name": "CopyAmazonS3ToBlob",
        "properties": {
            "description": "pipeline for copy activity",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "FileSystemSource",
                            "recursive": true
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "AmazonS3InputDataset"
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
                    "name": "AmazonS3ToBlob"
                }
            ],
            "start": "2014-08-08T18:00:00Z",
            "end": "2014-08-08T19:00:00Z"
        }
    }



## <a name="linked-service-properties"></a>Linkitetyn palveluominaisuudet

Seuraavassa taulukossa on kuvaus JSON elementtien tietyn Amazon S3 (**AwsAccessKey**) linkitetty palvelu.

| Ominaisuus | Kuvaus | Sallittu arvo | Pakollinen |
| -------- | ----------- | -------- | ------- |  
| accessKeyID | Salainen pikanäppäin tunnus. | merkkijono | Kyllä |
| secretAccessKey | Salainen pikanäppäin itse. | Salattu salainen merkkijono | Kyllä | 


## <a name="dataset-type-properties"></a>Tietojoukon ominaisuudet

Täydellinen luettelo osien ja ominaisuudet, jotka ovat käytettävissä määrittäminen tietojoukkoja [luominen tietojoukkoja](data-factory-create-datasets.md) on artikkelissa. Osia, kuten rakenne, käytettävyys ja käytännön muistuttavat tyypeissä tietojoukko (Azure SQL Azure-blob, Azuren taulukkojen jne.).

**TypeProperties** -osassa on eri mistäkin tietojoukko, ja se antaa tietoja tietojen tietovaraston sijainti. Tyypin **AmazonS3** (joka sisältää Amazon S3 tietojoukko) tietojoukko typeProperties osiossa on seuraavat ominaisuudet

| Ominaisuus | Kuvaus | Sallittu arvo | Pakollinen |
| -------- | ----------- | -------- | ------ | 
| bucketName | S3-ryhmän nimi. | Merkkijono | Kyllä |
| avain | S3 objektin avain. | Merkkijono | Ei | 
| etuliite | Etuliitteen S3 objekti-näppäintä. Objektit, joiden näppäimet Aloita tämä etuliite valitaan. Koskee vain, kun näppäintä on tyhjä. | Merkkijono | Ei | 
| versio | S3 objektina, jos S3 versiotiedot on otettu käyttöön versio. | Merkkijono | Ei |  
| Muotoile | Muotoile seuraavanlaisia tuetaan: **TekstinMuoto**, **AvroFormat**, **JsonFormat**, **OrcFormat**, **ParquetFormat**. Muotoile **tyyppi** -ominaisuuden arvoksi jokin näistä arvoista. Lue lisätietoja [Määrittäminen TekstinMuoto](#specifying-textformat), [Määrittämällä AvroFormat](#specifying-avroformat), [Määrittämällä JsonFormat](#specifying-jsonformat), [Määrittämällä OrcFormat](#specifying-orcformat)ja [Määrittämällä ParquetFormat](#specifying-parquetformat) osat. Jos haluat kopioida tiedostot muodossa-on välillä tiedostopohjaisia stores (binaarinen kopio), voit ohittaa sekä syöttö- ja tietojoukko määritelmät muoto-kohdassa.| Ei
| pakkaus | Määritä tyyppi ja tietojen pakkaus taso. Tuetut tiedostotyypit ovat: **GZip**, **Deflate**ja **BZip2** ja tuettujen tasot ovat: **Optimal** ja **nopein**. Tällä hetkellä pakkausasetuksia ei tue tietojen **AvroFormat** tai **OrcFormat**. Jos haluat lisätietoja, katso [pakkaamisen tuki](#compression-support) -osa.  | Ei |

> [AZURE.NOTE] bucketName ja näppäin määrittää sijainnin S3-objekti, jossa aikajakson on pääkansio säilö S3 objektit ja avain on S3 objektin koko polku.

### <a name="sample-dataset-with-prefix"></a>Esimerkki tietojoukko etuliitteellä

    {
        "name": "dataset-s3",
        "properties": {
            "type": "AmazonS3",
            "linkedServiceName": "link- testS3",
            "typeProperties": {
                "prefix": "testFolder/test",
                "bucketName": "testbucket",
                "format": {
                    "type": "OrcFormat"
                }
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true
        }
    }

### <a name="sample-data-set-with-version"></a>Esimerkki tietojoukon (versiolla)

    {
        "name": "dataset-s3",
        "properties": {
            "type": "AmazonS3",
            "linkedServiceName": "link- testS3",
            "typeProperties": {
                "key": "testFolder/test.orc",
                "bucketName": "testbucket",
                "version": "WBeMIxQkJczm0CJajYkHf0_k6LhBmkcL",
                "format": {
                    "type": "OrcFormat"
                }
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true
        }
    }


### <a name="dynamic-paths-for-s3"></a>Dynaaminen S3 polut

Otoksen Käytämme avain ja bucketName ominaisuuksien Amazon S3 tietojoukossa kiinteitä arvoja. 

    "key": "testFolder/test.orc",
    "bucketName": "testbucket",

Voit määrittää tietojen Factory laskeminen käyttämällä Järjestelmämuuttujat, kuten SliceStart avain ja bucketName dynaamisesti suorituksen aikana.

    "key": "$$Text.Format('{0:MM}/{0:dd}/test.orc', SliceStart)"
    "bucketName": "$$Text.Format('{0:yyyy}', SliceStart)"

Voit tehdä saman Amazon S3-tietojoukko etuliite-ominaisuuden. Katso [tietoja Factory-funktioita ja järjestelmämuuttujia](data-factory-functions-variables.md) tuetut Funktiot ja muuttujien luettelo. 


[AZURE.INCLUDE [data-factory-file-format](../../includes/data-factory-file-format.md)]
[AZURE.INCLUDE [data-factory-compression](../../includes/data-factory-compression.md)]


## <a name="copy-activity-type-properties"></a>Kopioi tehtävän ominaisuudet

Täydellinen luettelo osat ja ominaisuudet, jotka ovat käytettävissä toiminnan määrittäminen [Putkistot luomisesta](data-factory-create-pipelines.md) on artikkelissa. Ominaisuudet, kuten nimi, kuvaus sekä syöttö- ja taulukot ja käytännöt ovat käytettävissä kaikki tehtävälajit. 

Tehtävän **typeProperties** -osassa käytettävissä olevat ominaisuudet vaihtelevat toisaalta kunkin tehtävätyyppi. Kopioi tehtävälle ne vaihtelevat sen mukaan, lähteiden ja poistumia.

Kun kopioi tehtävän lähde on tyypin **FileSystemSource** (joka sisältää Amazon S3), seuraavat ominaisuudet ovat käytettävissä typeProperties-osassa:

| Ominaisuus | Kuvaus | Sallittu arvo | Pakollinen |
| -------- | ----------- | -------------- | -------- | 
| Rekursiiviset | Määrittää, onko rekursiivisesti, luettelon S3 hakemiston objektien. | TOSI tai EPÄTOSI | Ei | 

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-type-repeatability-for-relational-sources](../../includes/data-factory-type-repeatability-for-relational-sources.md)]

## <a name="performance-and-tuning"></a>Suorituskyky ja säätäminen  
Lisätietoja on artikkelissa [Kopioi tehtävän suorituskyvyn & säätäminen opas](data-factory-copy-activity-performance.md) lisätietoja tärkeimmät seikat, vaikutus suorituskykyä tietojen siirto (kopioi tehtävä) Azure Data Factory ja voidaan optimoida sen usealla tavalla.

## <a name="next-steps"></a>Seuraavat vaiheet
Seuraavissa artikkeleissa: 
- [Kopioi tehtävän opetusohjelma](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) vaiheittaiset ohjeet luomiseen putkijohto kopio-tehtävän. 
