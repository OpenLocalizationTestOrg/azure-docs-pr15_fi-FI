<properties 
    pageTitle="Tietojen siirtäminen tai DocumentDB | Microsoft Azure" 
    description="Lue, miten tietojen siirtäminen tai käyttämällä Azure Data Factory Azure DocumentDB kokoelmasta" 
    services="data-factory, documentdb" 
    documentationCenter="" 
    authors="linda33wj" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="multiple" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016" 
    ms.author="jingwang"/>

# <a name="move-data-to-and-from-documentdb-using-azure-data-factory"></a>Tietojen siirtäminen ja sieltä pois DocumentDB Azure Data Factory käyttäminen

Tässä artikkelissa käsitellään käyttämisestä kopioi tehtävän Azure tietojen factory-Azure DocumentDB tiedot toiseen tiedoista tallentaa ja Siirrä tiedot DocumentDB toiseen tietosäilö. Tässä artikkelissa perustuu [tietojen siirtämistä tehtävät](data-factory-data-movement-activities.md) -artikkelista, jossa näkyy yleiskatsaus tietojen siirto kopioi tehtävän ja tuettuja kaupan yhdistelmät.

Seuraavat esimerkit näyttää, miten haluat kopioida tiedot Azure DocumentDB ja päinvastoin Azure-Blob-objektien tallennustilaan. Tietoja voi kuitenkin kopioidun **suoraan** mistä tahansa lähteestä johonkin seuraavista poistumia määritetyllä tavalla [tähän](data-factory-data-movement-activities.md#supported-data-stores) kopio-toimintojen käyttäminen Azure Data Factory.  

> [AZURE.NOTE] Valitse-paikallisen/Azure IaaS tietojen kopioiminen tiedot tallentaa Azure DocumentDB ja päinvastoin ovat tuettuja Data Management Gateway versio 2.1 ja yläpuolella.

## <a name="sample-copy-data-from-documentdb-to-azure-blob"></a>Esimerkki: Tietojen kopioiminen DocumentDB Azure-Blob

Esimerkki alla on esitetty:

1. Linkitetyn palvelu [DocumentDb](#azure-documentdb-linked-service-properties)tyyppi.
2. Linkitetyn palvelu [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties)tyyppi. 
3. Syötteen [tietojoukko](data-factory-create-datasets.md) tyypin [DocumentDbCollection](#azure-documentdb-dataset-type-properties). 
4. Tulosteen [tietojoukko](data-factory-create-datasets.md) tyypin [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4. [Myyntijakso](data-factory-create-pipelines.md) kopioi aktiviteettiin, joka käyttää [DocumentDbCollectionSource](#azure-documentdb-copy-activity-type-properties) ja [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties).

Otosten kopioi Azure DocumentDB tiedot Azure-Blob. Käyttää näitä esimerkkejä JSON-ominaisuuksia on kuvattu osia seuraavat mallit.

**Azure linkitetty DocumentDB-palvelu:**

    {
      "name": "DocumentDbLinkedService",
      "properties": {
        "type": "DocumentDb",
        "typeProperties": {
          "connectionString": "AccountEndpoint=<EndpointUrl>;AccountKey=<AccessKey>;Database=<Database>"
        }
      }
    }

**Azure-Blob-säiliö linkitetty palvelu:**

    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

**Azure asiakirjan DB syötteen tietojoukko:**

Otosten oletetaan, että kokoelma nimeltä **henkilön** Azure DocumentDB-tietokannassa.
 
Asetus "ulkoinen": "true" ja määrittämällä, että taulukossa on ulkoiset tiedot-factory ja tuottamat tiedot factory toimintaa ei externalData käytäntöjä koskevaan materiaaliin Azure Data Factory-palvelun.

    {
      "name": "PersonDocumentDbTable",
      "properties": {
        "type": "DocumentDbCollection",
        "linkedServiceName": "DocumentDbLinkedService",
        "typeProperties": {
          "collectionName": "Person"
        },
        "external": true,
        "availability": {
          "frequency": "Day",
          "interval": 1
        }
      }
    }


**Azure-Blob tulosteen tietojoukko:**

Tiedot kopioidaan uuden Blob-objektien, mieti tietyn datetime tunti rakeisuuden kanssa Blob-objektien polulla tunnissa.

    {
      "name": "PersonBlobTableOut",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "docdb",
          "format": {
            "type": "TextFormat",
            "columnDelimiter": ",",
            "nullValue": "NULL"
          }
        },
        "availability": {
          "frequency": "Day",
          "interval": 1
        }
      }
    }

Esimerkki JSON asiakirjan henkilön sivustokokoelman DocumentDB tietokannan: 

    {
      "PersonId": 2,
      "Name": {
        "First": "Jane",
        "Middle": "",
        "Last": "Doe"
      }
    }

DocumentDB tukee kyseltäessä asiakirjojen SQL, kuten syntaksi välityksellä hierarkkisia JSON-tiedostoja. 

Esimerkki: Valitse Person.PersonId, Person.Name.First AS Etunimi Person.Name.Middle MiddleName, Person.Name.Last AS Sukunimi HENKILÖLTÄ

Seuraavat myyntijakso kopioi henkilön sivustokokoelman DocumentDB tietokannan tietojen Azure-blob. Kopioi tehtävän syötteen osa ja tulos on määritetty tietojoukkoja.  
    
    {
      "name": "DocDbToBlobPipeline",
      "properties": {
        "activities": [
          {
            "type": "Copy",
            "typeProperties": {
              "source": {
                "type": "DocumentDbCollectionSource",
                "query": "SELECT Person.Id, Person.Name.First AS FirstName, Person.Name.Middle as MiddleName, Person.Name.Last AS LastName FROM Person",
                "nestingSeparator": "."
              },
              "sink": {
                "type": "BlobSink",
                "blobWriterAddHeader": true,
                "writeBatchSize": 1000,
                "writeBatchTimeout": "00:00:59"
              }
            },
            "inputs": [
              {
                "name": "PersonDocumentDbTable"
              }
            ],
            "outputs": [
              {
                "name": "PersonBlobTableOut"
              }
            ],
            "policy": {
              "concurrency": 1
            },
            "name": "CopyFromDocDbToBlob"
          }
        ],
        "start": "2015-04-01T00:00:00Z",
        "end": "2015-04-02T00:00:00Z"
      }
    }

## <a name="sample-copy-data-from-azure-blob-to-azure-documentdb"></a>Esimerkki: Tietojen kopioiminen Azure-Blob Azure DocumentDB

Esimerkki alla on esitetty:

1. Linkitetyn palvelu [DocumentDb](#azure-documentdb-linked-service-properties)tyyppi.
2. Linkitetyn palvelu [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties)tyyppi.
3. Syötteen [tietojoukko](data-factory-create-datasets.md) tyypin [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4. Tulosteen [tietojoukko](data-factory-create-datasets.md) tyypin [DocumentDbCollection](#azure-documentdb-dataset-type-properties). 
4. [Myyntijakso](data-factory-create-pipelines.md) kopioi aktiviteettiin, joka käyttää [BlobSource](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) ja [DocumentDbCollectionSink](#azure-documentdb-copy-activity-type-properties).


Otosten kopioi tiedot Azure blob Azure DocumentDB. Käyttää näitä esimerkkejä JSON-ominaisuuksia on kuvattu osia seuraavat mallit.

**Azure-Blob-säiliö linkitetty palvelu:**
    
    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

**Azure linkitetty DocumentDB-palvelu:**
    
    {
      "name": "DocumentDbLinkedService",
      "properties": {
        "type": "DocumentDb",
        "typeProperties": {
          "connectionString": "AccountEndpoint=<EndpointUrl>;AccountKey=<AccessKey>;Database=<Database>"
        }
      }
    }

**Azure Blob syötteen tietojoukko:**

    {
      "name": "PersonBlobTableIn",
      "properties": {
        "structure": [
          {
            "name": "Id",
            "type": "Int"
          },
          {
            "name": "FirstName",
            "type": "String"
          },
          {
            "name": "MiddleName",
            "type": "String"
          },
          {
            "name": "LastName",
            "type": "String"
          }
        ],
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "fileName": "input.csv",
          "folderPath": "docdb",
          "format": {
            "type": "TextFormat",
            "columnDelimiter": ",",
            "nullValue": "NULL"
          }
        },
        "external": true,
        "availability": {
          "frequency": "Day",
          "interval": 1
        }
      }
    }

**Azure DocumentDB tulosteen tietojoukko:**

Otosten kopioi tiedot nimeltä "henkilö-kokoelmaan.

    {
      "name": "PersonDocumentDbTableOut",
      "properties": {
        "structure": [
          {
            "name": "Id",
            "type": "Int"
          },
          {
            "name": "Name.First",
            "type": "String"
          },
          {
            "name": "Name.Middle",
            "type": "String"
          },
          {
            "name": "Name.Last",
            "type": "String"
          }
        ],
        "type": "DocumentDbCollection",
        "linkedServiceName": "DocumentDbLinkedService",
        "typeProperties": {
          "collectionName": "Person"
        },
        "availability": {
          "frequency": "Day",
          "interval": 1
        }
      }
    }

Seuraavat myyntijakso kopioi tiedot Azure-Blob DocumentDB henkilö-kokoelmaan. Kopioi tehtävän syötteen osa ja tulos on määritetty tietojoukkoja. 
    
    {
      "name": "BlobToDocDbPipeline",
      "properties": {
        "activities": [
          {
            "type": "Copy",
            "typeProperties": {
              "source": {
                "type": "BlobSource"
              },
              "sink": {
                "type": "DocumentDbCollectionSink",
                "nestingSeparator": ".",
                "writeBatchSize": 2,
                "writeBatchTimeout": "00:00:00"
              }
              "translator": {
                  "type": "TabularTranslator",
                  "ColumnMappings": "FirstName: Name.First, MiddleName: Name.Middle, LastName: Name.Last, BusinessEntityID: BusinessEntityID, PersonType: PersonType, NameStyle: NameStyle, Title: Title, Suffix: Suffix, EmailPromotion: EmailPromotion, rowguid: rowguid, ModifiedDate: ModifiedDate"
              }
            },
            "inputs": [
              {
                "name": "PersonBlobTableIn"
              }
            ],
            "outputs": [
              {
                "name": "PersonDocumentDbTableOut"
              }
            ],
            "policy": {
              "concurrency": 1
            },
            "name": "CopyFromBlobToDocDb"
          }
        ],
        "start": "2015-04-14T00:00:00Z",
        "end": "2015-04-15T00:00:00Z"
      }
    }
 
Jos otoksen blob-syöte on 

    1,John,,Doe

Valitse tuloste JSON DocumentDB on kuin:

    {
      "Id": 1,
      "Name": {
        "First": "John",
        "Middle": null,
        "Last": "Doe"
      },
      "id": "a5e8595c-62ec-4554-a118-3940f4ff70b6"
    }
    
DocumentDB on NoSQL säilö JSON asiakirjojen, jossa sisäkkäisiä rakenteita sallitaan. Azure Data Factory käyttäjä osoittamaan kautta **nestingSeparator**, joka on hierarkia-. " Tässä esimerkissä. Erotin, kopioi tehtävän luo kolme lasten osat "Nimi" objektia ensin, toisen nimen ja sukunimen mukaan "Name.First", "Name.Middle" ja "Name.Last" taulukkomääritys.

## <a name="azure-documentdb-linked-service-properties"></a>Azure DocumentDB linkitetyn palvelun ominaisuudet

Seuraavassa taulukossa on kuvaus JSON elementtien tietyn Azure DocumentDB linkitetty-palveluun. 

| **Ominaisuus** | **Kuvaus** | **Pakollinen** |
| -------- | ----------- | --------- |
| tyyppi | Type-ominaisuus on määritettävä: **DocumentDb** | Kyllä |
| connectionString | Määritä Azure DocumentDB tietokantayhteyden muodostamisessa tarvittavat tiedot. | Kyllä |

## <a name="azure-documentdb-dataset-type-properties"></a>Azure DocumentDB tietojoukko ominaisuudet

Täydellinen luettelo osien ja ominaisuudet, jotka ovat käytettävissä määrittäminen tietojoukkoja tutustumaan [luominen tietojoukkoja](data-factory-create-datasets.md) artikkelin. Osia, kuten rakenne, käytettävyys ja tietojoukko JSON käytännön muistuttavat tyypeissä tietojoukko (Azure SQL Azure-blob, Azuren taulukkojen jne.).
 
TypeProperties-osassa on eri mistäkin tietojoukko, ja se antaa tietoja tietojen tietovaraston sijainti. Tyypin **DocumentDbCollection** tietojoukon typeProperties osiossa on seuraavat ominaisuudet.

| **Ominaisuus** | **Kuvaus** | **Pakollinen** |
| -------- | ----------- | -------- |
| collectionName | DocumentDB asiakirjan sivustokokoelman nimi. | Kyllä |


Esimerkki:

    {
      "name": "PersonDocumentDbTable",
      "properties": {
        "type": "DocumentDbCollection",
        "linkedServiceName": "DocumentDbLinkedService",
        "typeProperties": {
          "collectionName": "Person"
        },
        "external": true,
        "availability": {
          "frequency": "Day",
          "interval": 1
        }
      }
    }

### <a name="schema-by-data-factory"></a>Rakenteen mukaan Data Factory
Rakenteen tiedot stores, kuten DocumentDB Data Factory-palvelun johtaa rakenteen jollakin seuraavista tavoista:  

1.  Jos määrität **rakenne** -ominaisuuden avulla tietojoukko määrityksessä olevien tietojen rakennetta, Data Factory-palvelun merkkien rakenteen kuin rakenne. Tässä tapauksessa rivi ei sisällä sarakkeen arvon, jos arvo on null toimitetaan sen.
2.  Jos et määritä **rakenne** -ominaisuuden avulla tietojoukko määrityksessä olevien tietojen rakennetta, Data Factory-palvelun johtaa rakenteen käyttämällä ensimmäisen rivin tiedot. Tässä tapauksessa jos ensimmäinen rivi ei sisällä koko mallia, joitakin sarakkeita on saatu kopioimista puuttuu.

Tämän vuoksi rakenteen vapauttaa tietolähteiden paras käytäntö on Määritä olevien tietojen **rakennetta** -ominaisuuden avulla rakennetta.

## <a name="azure-documentdb-copy-activity-type-properties"></a>Azure DocumentDB kopioi tehtävän ominaisuudet

Täydellinen luettelo osat ja ominaisuudet, jotka ovat käytettävissä toiminnan määrittäminen tutustumaan [Putkistot luomisesta](data-factory-create-pipelines.md) on artikkelissa. Ominaisuudet, kuten nimi, kuvaus sekä syöttö- ja taulukot ja käytäntö ovat käytettävissä kaikki tehtävälajit.
 
**Huomautus:** Kopioi tehtävän tulee vain yhden syötteen ja tuottaa vain yksi tulos.

Tehtävän typeProperties-osassa käytettävissä olevat ominaisuudet vaihtelevat toisaalta kunkin tehtävätyypin ja jos kopioi tehtävän ne vaihtelevat sen mukaan, lähteiden ja poistumia.

Jos kopioi tehtävän lähteen tyyppi **DocumentDbCollectionSource** on seuraavat ominaisuudet ovat käytettävissä **typeProperties** -osassa:

| **Ominaisuus** | **Kuvaus** | **Sallittu arvo** | **Pakollinen** |
| ------------ | --------------- | ------------------ | ------------ |
| kyselyn | Määritä kysely voit lukea tietoja. | Kyselyn tukemat DocumentDB merkkijono. <br/><br/>Esimerkki:`SELECT c.BusinessEntityID, c.PersonType, c.NameStyle, c.Title, c.Name.First AS FirstName, c.Name.Last AS LastName, c.Suffix, c.EmailPromotion FROM c WHERE c.ModifiedDate > \"2009-01-01T00:00:00\"` | Ei <br/><br/>Jos ei määritetä, SQL-lause, joka suoritetaan:`select <columns defined in structure> from mycollection` 
| nestingSeparator | Osoittamassa, että asiakirja on upotettu erikoismerkin lisääminen | Mitä tahansa merkkiä. <br/><br/>DocumentDB on NoSQL säilö JSON asiakirjojen, jossa sisäkkäisiä rakenteita sallitaan. Azure Data Factory käyttäjä osoittamaan kautta nestingSeparator, joka on hierarkia-. " Edellä olevassa esimerkissä. Erotin, kopioi tehtävän luo kolme lasten osat "Nimi" objektia ensin, toisen nimen ja sukunimen "Name.First", "Name.Middle" ja "Name.Last" taulukkomäärityksen mukaan. | Ei

**DocumentDbCollectionSink** tukee seuraavia ominaisuuksia:

| **Ominaisuus** | **Kuvaus** | **Sallittu arvo** | **Pakollinen** |
| -------- | ----------- | -------------- | -------- |
| nestingSeparator | Erikoismerkki osoittamaan sisäkkäisiä asiakirjan lähdesarakkeen nimi tarvita. <br/><br/>Kuten edellä: `Name.First` tulosteesta taulukon tuottaa seuraavat JSON rakenne, DocumentDB asiakirjassa:<br/><br/>"Nimi": {}<br/>  "Ensimmäisen": "John"<br/>}, | Merkki, jota käytetään erottamaan sisäkkäisiä tasoja.<br/><br/>Oletusarvo on `.` (piste). | Merkki, jota käytetään erottamaan sisäkkäisiä tasoja. <br/><br/>Oletusarvo on `.` (piste). | Ei | 
| writeBatchSize | Rinnakkaisia pyyntöjen DocumentDB palveluun asiakirjojen luomiseen määrä.<br/><br/>Voit hienosäätää suorituskyvyn, kun kopioit tietoja tai DocumentDB tämän ominaisuuden avulla. Voit odottaa parempi suorituskyky voit suurentaa writeBatchSize, koska enemmän rinnakkain pyyntöjä DocumentDB lähetetään. Mutta sinun on välttämiseksi rajoittaminen, jotka voivat palauttaa virhesanoma: "Pyytää korko on suuri".<br/><br/>Rajoittimen päättänyt useista tekijöistä, mukaan lukien koon asiakirjojen asiakirjoissa kausien määrä, indeksointi käytännön kohdesivustokokoelmaan jne. Kopioi toimintoja, voit käyttää paremmin sivustokokoelman (esimerkiksi S3) on eniten käytettävissä siirtonopeuden (2 500 pyynnön yksiköt ja toisen). | Kokonaisluku | Ei (oletus: 10000) |
| writeBatchTimeout | Toiminto on valmis, ennen kuin se aikakatkaistaan odotusaika. | aikajakson<br/><br/> Esimerkki: "00: 30:00" (30 minuuttia). | Ei |
 
## <a name="appendix"></a>Lisäys
1. **Kysymys:**  
    onko olemassa olevien tietueiden kopioi tehtävän tuki päivitys?

    **Vastaus:**  
    ei.

2. **Kysymys:**  
    mistä uudelleen DocumentDB käsittelevät kopion jo kopioitu tietueet?

    **Vastaus:**  
    jos tietueella on "Tunnus"-kenttä ja kopiointi yrittää lisätä tietueen, jolla on sama tunnus, kopiointi ilmoittaa virheestä.  
 
3. **Kysymys:** 
    Data Factory tue [alueen tai hash-pohjaisten tietojen jakaminen](https://azure.microsoft.com/documentation/articles/documentdb-partition-data/)? 

    **Vastaus:** 
    ei. 
4. **Kysymys:** 
    useamman kuin yhden taulukon DocumentDB sivustokokoelman määrittää?
    
    **Vastaus:** 
    ei. Vain yksi sivustokokoelman voidaan määrittää tällä hetkellä.
     
## <a name="performance-and-tuning"></a>Suorituskyky ja säätäminen  
Lisätietoja on artikkelissa [Kopioi tehtävän suorituskyvyn & säätäminen opas](data-factory-copy-activity-performance.md) lisätietoja tärkeimmät seikat, vaikutus suorituskykyä tietojen siirto (kopioi tehtävä) Azure Data Factory ja voidaan optimoida sen usealla tavalla.
