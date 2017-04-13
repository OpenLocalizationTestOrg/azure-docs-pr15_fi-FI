<properties 
    pageTitle="Tietojen siirtäminen OData lähteistä | Azure Data Factory" 
    description="Lisätietoja tietojen siirtäminen käyttämällä Azure Data Factory OData lähteistä." 
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
    ms.date="09/26/2016" 
    ms.author="jingwang"/>

# <a name="move-data-from-a-odata-source-using-azure-data-factory"></a>Siirrä tiedot OData-tietolähteen avulla Azure Data Factory
Tässä artikkelissa käsitellään käyttämisestä kopioi tehtävän Azure tietojen factory-Siirrä tiedot OData-lähteestä toiseen tietosäilö. Tässä artikkelissa perustuu [tietojen siirtämistä tehtävät](data-factory-data-movement-activities.md) -artikkelista, jossa näkyy yleiskatsaus tietojen siirto kopioi tehtävän ja tuettuja kaupan yhdistelmät.

> [AZURE.NOTE] Tämä OData yhdistimen tuki tietojen sekä pilvestä OData ja paikallisen OData lähteistä kopioimisesta. Jotta jälkimmäinen sinun täytyy asentaa Data Management Gateway. Lisätietoja Data Management Gateway on artikkelissa [paikallisen ja cloud tietojen siirtäminen](data-factory-move-data-between-onprem-and-cloud.md) artikkelissa.

## <a name="copy-data-wizard"></a>Ohjattu tietojen kopioiminen
Voit käyttää ohjattua tietojen kopioiminen on helpointa myyntijakso, kopioi tiedot OData-tietolähteen luominen. Katso [Opetusohjelma: Luo kopio ohjatulla putkijohto](data-factory-copy-data-wizard-tutorial.md) nopeasti käyttämällä ohjattua tietojen kopioiminen putkijohto luomisesta saat. 

Seuraavissa esimerkeissä otoksen JSON määritykset, jonka avulla voit luoda putkijohto [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) tai [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) tai [PowerShellin Azure](data-factory-copy-activity-tutorial-using-powershell.md). Ne näyttävät tietojen kopioiminen Azure-Blob-objektien tallennustilaan OData-lähde. Tietoja voi kuitenkin kopioida johonkin seuraavista poistumia määritetyllä tavalla [tähän](data-factory-data-movement-activities.md#supported-data-stores) kopio-toimintojen käyttäminen Azure Data Factory.


## <a name="sample-copy-data-from-odata-source-to-azure-blob"></a>Esimerkki: Kopioimalla tiedot OData lähteestä Azure-Blob

Tässä esimerkissä esitetään, kuinka haluat kopioida tiedot OData-lähteestä Azure-Blob-objektien tallennustilaan. Tietoja voi kuitenkin kopioidun **suoraan** johonkin seuraavista poistumia määritetyllä tavalla [tähän](data-factory-data-movement-activities.md#supported-data-stores) kopio-toimintojen käyttäminen Azure Data Factory.  
 
Malli on tietoja factory-kohteet:

1.  [OData](#odata-linked-service-properties)-tyypin linkitetyn palvelu.
2.  Linkitetyn palvelu [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties)tyyppi.
3.  Syötteen [tietojoukko](data-factory-create-datasets.md) tyypin [ODataResource](#odata-dataset-type-properties).
4.  Tulosteen [tietojoukko](data-factory-create-datasets.md) tyypin [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4.  [Myyntijakso](data-factory-create-pipelines.md) kopioi aktiviteettiin, joka käyttää [RelationalSource](#odata-copy-activity-type-properties) ja [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties).

Otosten kopioi tietojen kyselyt vastaan Azure-blob OData lähde tunnissa. Käyttää näitä esimerkkejä JSON-ominaisuuksia on kuvattu osia seuraavat mallit. 

**OData linkitetty palvelu** Tässä esimerkissä perustodentamista. Todennus, voit käyttää erilaista Katso [OData linkitetty palvelun](#odata-linked-service-properties) osa. 

    {
        "name": "ODataLinkedService",
        "properties": 
        {
            "type": "OData",
            "typeProperties": 
            {
                "url": "http://services.odata.org/OData/OData.svc",
               "authenticationType": "Anonymous"
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

**OData-syötteen tietojoukko**

Asetus "ulkoinen": "true" ilmoittaa Data Factory-palvelun dataset on ulkoiset tiedot-factory ja tietojen factory toimintaa ei tuottamat.
    
    {
        "name": "ODataDataset",
        "properties": 
        {
            "type": "ODataResource",
            "typeProperties": 
            {
                "path": "Products" 
            },
            "linkedServiceName": "ODataLinkedService",
            "structure": [],
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true,
            "policy": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3               
            }
        }
    }

**Polun** määrittämällä tietojoukossa määritys on valinnainen. 


**Azure-Blob tulosteen tietojoukko**

Tietoja kirjoitetaan uusia Blob-objektien tunnissa (taajuus: tunti, väli: 1). Kansiopolun blob arvioidaan dynaamisesti sektoria, käsitellään alkamispäivää perusteella. Kansiopolku käytetään vuoden, kuukauden, päivän ja alkamisajan tuntia osat.

    {
        "name": "AzureBlobODataDataSet",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/odata/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

Putkisto sisältää kopio-toiminto, joka on määritetty käyttämään syöttö- ja tietojoukkoja ja on suunniteltu toimimaan tunnissa. Myyntijakso JSON määritelmä- **lähteen** tyyppi on määritetty **RelationalSource** ja **käsittelytoiminto** tyyppi-asetukseksi on määritetty **BlobSink**. SQL-kyselyn, **kysely** -ominaisuuden valitsee (uusin) uusimmat tiedot OData-lähteestä.
    
    {
        "name": "CopyODataToBlob",
        "properties": {
            "description": "pipeline for copy activity",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "RelationalSource",
                            "query": "?$select=Name, Description&$top=5",
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "ODataDataSet"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobODataDataSet"
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
                    "name": "ODataToBlob"
                }
            ],
            "start": "2016-02-01T18:00:00Z",
            "end": "2016-02-03T19:00:00Z"
        }
    }


**Kyselyn** määrittäminen myyntijakso-määrityksessä on valinnainen. **URL-osoite** , jossa Data Factory-palvelua käytetään tietojen hakemiseen: URL-osoite (pakollinen) linkitetyn palvelussa + tietojoukko (valinnainen) + (valinnainen) myyntijakso kyselyn polku. 

## <a name="odata-linked-service-properties"></a>OData linkitetty palveluominaisuudet

Seuraavassa taulukossa on kuvaus JSON elementtien tietyn linkitetty OData-palveluun.

| Ominaisuus | Kuvaus | Pakollinen |
| -------- | ----------- | -------- | 
| tyyppi | Type-ominaisuus on määritettävä: **OData** | Kyllä |
| URL-osoite| OData-palvelun URL-osoite. | Kyllä |
| authenticationType | Yhteyden muodostaminen OData-tietolähteen käyttöoikeuden tyyppi. <br/><br/> Cloud OData mahdolliset arvot ovat anonyymi ja Basic. Mahdolliset arvot ovat paikallisen OData anonyymi, Basic ja Windows. | Kyllä | 
| käyttäjänimi | Määritä käyttäjänimi, jos käytössäsi on perustodentamista. | Kyllä (vain, jos käytössäsi on perustodentamista) | 
| salasana | Määritä käyttäjänimi määritetyn käyttäjätilin salasanan. | Kyllä (vain, jos käytössäsi on perustodentamista) | 
| gatewayName | Yhdyskäytävän, Data Factory-palvelun avulla yhteyden muodostaminen paikalliseen OData-palvelun nimi. Määritä vain, jos haluat kopioida tiedot paikallinen OData lähde. | Ei |

### <a name="using-basic-authentication"></a>Käyttöoikeuksien avulla

    {
        "name": "inputLinkedService",
        "properties": 
        {
            "type": "OData",
            "typeProperties": 
            {
               "url": "http://services.odata.org/OData/OData.svc",
               "authenticationType": "Basic",
                "username": "username",
               "password": "password"
           }
       }
    }

### <a name="using-anonymous-authentication"></a>Käyttämällä Anonyymi todentaminen
    
    {
        "name": "ODataLinkedService",
        "properties": 
        {
            "type": "OData",
            "typeProperties": 
            {
               "url": "http://services.odata.org/OData/OData.svc",
               "authenticationType": "Anonymous"
           }
       }
    }

### <a name="using-windows-authentication-accessing-on-premises-odata-source"></a>Windows-todennuksen käyttäminen paikallisen OData lähde

    {
        "name": "inputLinkedService",
        "properties": 
        {
            "type": "OData",
            "typeProperties": 
            {
               "url": "<endpoint of on-premises OData source e.g. Dynamics CRM>",
               "authenticationType": "Windows",
                "username": "domain\\user",
               "password": "password",
               "gatewayName": "mygateway"
           }
       }
    }



## <a name="odata-dataset-type-properties"></a>OData tietojoukko ominaisuudet

Täydellinen luettelo osien ja ominaisuudet, jotka ovat käytettävissä määrittäminen tietojoukkoja [luominen tietojoukkoja](data-factory-create-datasets.md) on artikkelissa. Osia, kuten rakenne, käytettävyys ja tietojoukko JSON käytännön muistuttavat tyypeissä tietojoukko (Azure SQL Azure-blob, Azuren taulukkojen jne.).

**TypeProperties** -osassa on eri mistäkin tietojoukko, ja se antaa tietoja tietojen tietovaraston sijainti. Tyypin **ODataResource** (joka sisältää OData-tietojoukko) tietojoukko typeProperties osiossa on seuraavat ominaisuudet

| Ominaisuus | Kuvaus | Pakollinen |
| -------- | ----------- | -------- |
| polku | OData-resurssin polku | Ei | 

## <a name="odata-copy-activity-type-properties"></a>OData kopioi tehtävän ominaisuudet

Täydellinen luettelo osat ja ominaisuudet, jotka ovat käytettävissä toiminnan määrittäminen [Putkistot luomisesta](data-factory-create-pipelines.md) on artikkelissa. Ominaisuudet, kuten nimi, kuvaus sekä syöttö- ja taulukot ja käytäntö ovat käytettävissä kaikki tehtävälajit. 

Tehtävän typeProperties-osassa käytettävissä olevat ominaisuudet vaihtelevat toisaalta kunkin tehtävätyyppi. Kopioi tehtävälle ne vaihtelevat sen mukaan, lähteiden ja poistumia.

Kun lähteen tyyppi **RelationalSource** (joka sisältää OData) on seuraavat ominaisuudet ovat käytettävissä typeProperties-osassa:

| Ominaisuus | Kuvaus | Esimerkki | Pakollinen |
| -------- | ----------- | -------------- | -------- |
| kyselyn | Mukautetun kyselyn avulla voit lukea tietoja. | "? $select = nimi, kuvaus ja $top = 5" | Ei | 

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

### <a name="type-mapping-for-odata"></a>OData-tyypin yhdistäminen

[Tietojen siirtämistä toiminnot](data-factory-data-movement-activities.md) on artikkelissa mainittu kopioi tehtävän suorittaa Automaattinen tyypin muunnoksia lähteen tyyppiä vastaamaan allas tyypit seuraavista 2 vaihe vaiheelta kanssa:

1. Muuntaminen alkuperäisen tietolähteen tyypit .NET-tyyppi
2. .NET tyypistä muuntaminen alkuperäisen käsittelytoiminto tyyppi

Kun siirtäminen tiedot OData-tiedot tallennetaan, OData-tietotyypit on yhdistetty .NET tyypit.


[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-type-repeatability-for-relational-sources](../../includes/data-factory-type-repeatability-for-relational-sources.md)]

## <a name="performance-and-tuning"></a>Suorituskyky ja säätäminen  
Lisätietoja on artikkelissa [Kopioi tehtävän suorituskyvyn & säätäminen opas](data-factory-copy-activity-performance.md) lisätietoja tärkeimmät seikat, vaikutus suorituskykyä tietojen siirto (kopioi tehtävä) Azure Data Factory ja voidaan optimoida sen usealla tavalla.

