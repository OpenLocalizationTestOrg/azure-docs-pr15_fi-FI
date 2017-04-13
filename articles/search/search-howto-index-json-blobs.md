<properties
pageTitle="Indeksoinnin JSON BLOB Azure haun blob indeksointitoiminto kanssa"
description="Indeksoinnin JSON BLOB Azure haun blob indeksointitoiminto kanssa"
services="search"
documentationCenter=""
authors="chaosrealm"
manager="pablocas"
editor="" />

<tags
ms.service="search"
ms.devlang="rest-api"
ms.workload="search" ms.topic="article"  
ms.tgt_pltfrm="na"
ms.date="07/26/2016"
ms.author="eugenesh" />

# <a name="indexing-json-blobs-with-azure-search-blob-indexer"></a>Indeksoinnin JSON BLOB Azure haun blob indeksointitoiminto kanssa 

Tässä artikkelissa kerrotaan, miten voit määrittää Azure haun blob indeksointitoiminto jäsennetyn sisällön noutamiseen BLOB-objektit, jotka sisältävät JSON.

## <a name="scenarios"></a>Skenaariot

Oletusarvon mukaan [Azure haun blob indeksointitoiminto](search-howto-indexing-azure-blob-storage.md) jäsentää kuin yhden lohkon tekstin JSON BLOB-objektit. Usein haluat säilyttää JSON-asiakirjojen rakenteen. Valita esimerkiksi JSON-asiakirja 

    { 
        "article" : {
            "text" : "A hopefully useful article explaining how to parse JSON blobs",
            "datePublished" : "2016-04-13" 
            "tags" : [ "search", "storage", "howto" ]    
        }
    }

haluat ehkä jäsentää Azure haku-asiakirjaan, "teksti", "datePublished" ja "tunnisteet-kentät.

Voit myös silloin, kun yhteyttä BLOB sisältävät **JSON objektien matriisi**, voit halutessasi kunkin luomisella erillisenä tiedostona Azure haun matriisin elementti. Esimerkiksi annettu Blob-objektien kanssa tämän JSON:  

    [
        { "id" : "1", "text" : "example 1" },
        { "id" : "2", "text" : "example 2" },
        { "id" : "3", "text" : "example 3" }
    ]

Voit lisätä Azure etsinnän indeksiin 3 erillistä asiakirjaa, jonka molemmissa "tunnus" ja "teksti" kentät. 

> [AZURE.IMPORTANT] Tämä toiminto on tällä hetkellä esikatselu. Se on käytettävissä vain REST-Ohjelmointirajapinnalla versio **2015-02-28-esikatselu**. Ota muista, esikatsella ohjelmointirajapinnan on tarkoitettu testaus ja arviointi ja ei voi käyttää tuotantoympäristössä. 

## <a name="setting-up-json-indexing"></a>JSON indeksoinnin määrittäminen

Indeksoida JSON BLOB-objektit, Määritä `parsingMode` määritysten parametri `json` (luoda hakemiston kunkin blob yhtenä asiakirjana) tai `jsonArray` (Jos että BLOB sisältävät JSON-matriisi): 

    {
      "name" : "my-json-indexer",
      ... other indexer properties
      "parameters" : { "configuration" : { "parsingMode" : "json" | "jsonArray" } }
    }

Tarvittaessa voit valita kohde-hakuindeksin täyttämiseen käytetyn JSON lähdeasiakirjan ominaisuudet **kentän yhdistämismäärityksiä** avulla.  Tämä on kuvattu alla. 

> [AZURE.IMPORTANT] Kun käytät `json` tai `jsonArray` jäsennyksen tilassa Azure haun olettaa, olevan tietolähteen kaikki Blob-objektien JSON. Jos tarvitset tukea JSON ja JSON BLOB-objektit erilaisten saman tietolähteen, kerro meille [UserVoice](https://feedback.azure.com/forums/263029-azure-search)-sivuston.

## <a name="using-field-mappings-to-build-search-documents"></a>Muodosta haun asiakirjojen kentän yhdistämismäärityksiä avulla 

Tällä hetkellä Azure haku ei voi indeksoida haluamaansa JSON asiakirjoja suoraan, sillä se tukee vain alkeistyyppi tietotyyppejä, merkkijonon matriisin ja GeoJSON pistettä. Voit kuitenkin JSON asiakirjan osaa, valitse ja "Nosta" ne ylimmän tason kenttiin, etsi asiakirjan **kenttien yhdistämismäärityksiä** avulla. Lisätietoja kentän yhdistämismäärityksiä perusteet on artikkelissa [Azure haun indeksointitoiminto kentän yhdistämismäärityksiä silta tietolähteet ja hakuindeksin erot](search-indexer-field-mappings.md).

Mielenkiinnon Esimerkki JSON voit: 

    { 
        "article" : {
            "text" : "A hopefully useful article explaining how to parse JSON blobs",
            "datePublished" : "2016-04-13" 
            "tags" : [ "search", "storage", "howto" ]    
        }
    }

Oletetaan, että sinulla on hakuindeksin, kun seuraavat kentät: `text` tyypin Edm.String, `date` Edm.DateTimeOffset, tyyppi ja `tags` Collection(Edm.String) tyypin. Haluat yhdistää oman JSON haluamaasi muotoon, käytä seuraavia kenttien yhdistämismääritykset: 

    "fieldMappings" : [ 
        { "sourceFieldName" : "/article/text", "targetFieldName" : "text" },
        { "sourceFieldName" : "/article/datePublished", "targetFieldName" : "date" },
        { "sourceFieldName" : "/article/tags", "targetFieldName" : "tags" }
    ]

Tietolähteen kenttänimet yhdistämismääritykset määritetään käyttämällä [JSON osoitin](http://tools.ietf.org/html/rfc6901) -merkintätapaa. Voit viitata JSON asiakirjan ylimmällä vinoviivalla aloittaminen ja valitse siirtyä eteenpäin vinoviivalla erotettua polun avulla halutun ominaisuuden (tasolla haluamaansa sisäkkäisiä). 

Voit myös katsoa yksittäisen taulukon osat nollasta indeksin avulla. Jos haluat valita edellä olevassa esimerkissä "tunnisteet" taulukon ensimmäinen elementti, käytä kenttien yhdistämismäärityksen tältä:

    { "sourceFieldName" : "/article/tags/0", "targetFieldName" : "firstTag" }

> [AZURE.NOTE] Jos kentän määritystä polku tietolähteen kentän nimen viittaa ominaisuus, joka ei ole JSON, tämä määritys ohitetaan ilman virhettä. Tämä tapahtuu niin, että sovellus tukee tiedostot eri rakenteen käyttäminen (joka on yleisiä Käyttötapaus). Koska ei ole kelpoisuuden, sinun täytyy huolehtia oman kentän määritystä kirjoitusvirheet välttämiseksi. 

Jos JSON tiedostojen sisältää vain yksinkertaisia ylimmän tason ominaisuudet, ei ehkä kentän yhdistämismäärityksiä ollenkaan. Esimerkiksi jos että JSON näyttää tältä, ylimmän tason ominaisuudet "teksti", "datePublished" ja "tunnisteet" suoraan yhdistetään hakuindeksin vastaaviin kenttiin: 
 
    { 
       "text" : "A hopefully useful article explaining how to parse JSON blobs",
       "datePublished" : "2016-04-13" 
       "tags" : [ "search", "storage", "howto" ]    
    }

## <a name="indexing-nested-json-arrays"></a>Indeksoinnin JSON sisäkkäisiä taulukoita

Jos haluat indeksoida matriisin JSON objekteja, mutta matriisin on upotettu johonkin asiakirjan? Voit valita, mitkä ominaisuus sisältää taulukon avulla `documentRoot` kokoonpano-ominaisuutta. Esimerkki: oman BLOB näyttää tältä: 

    { 
        "level1" : {
            "level2" : [
                { "id" : "1", "text" : "Use the documentRoot property" }, 
                { "id" : "2", "text" : "to pluck the array you want to index" },
                { "id" : "3", "text" : "even if it's nested inside the document" }  
            ]
        }
    } 

indeksi-"level2"-ominaisuuden matriisin määritysten avulla: 

    {
        "name" : "my-json-array-indexer",
        ... other indexer properties
        "parameters" : { "configuration" : { "parsingMode" : "jsonArray", "documentRoot" : "/level1/level2" } }
    }


## <a name="request-examples"></a>Esimerkkejä pyytäminen

Laajennettujen kaikki yhdessä, seuraavassa on esimerkkejä valmis paketteja. 

Tietolähde: 

    POST https://[service name].search.windows.net/datasources?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "my-blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "<my storage connection string>" },
        "container" : { "name" : "my-container", "query" : "optional, my-folder" }
    }   

Indeksointitoiminto:

    POST https://[service name].search.windows.net/indexers?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "my-json-indexer",
      "dataSourceName" : "my-blob-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" },
      "parameters" : { "configuration" : { "useJsonParser" : true } }, 
      "fieldMappings" : [ 
        { "sourceFieldName" : "/article/text", "targetFieldName" : "text" },
        { "sourceFieldName" : "/article/datePublished", "targetFieldName" : "date" },
        { "sourceFieldName" : "/article/tags", "targetFieldName" : "tags" }
      ]
    }

## <a name="help-us-make-azure-search-better"></a>Auta meitä parantamaan Azure haun toimintaa

Jos sinulla on ehdottaa ominaisuuksia tai ideoita tehokkuutta, ota saavuttaminen meille [UserVoice-sivustossa](https://feedback.azure.com/forums/263029-azure-search/).