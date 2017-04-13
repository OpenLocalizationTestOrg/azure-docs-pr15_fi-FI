<properties
pageTitle="Indeksoinnin CSV-BLOB-objektit ja Azure haun blob indeksointitoiminto | Microsoft Azure"
description="Opettele indeksoida CSV-BLOB Azure haulla"
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
ms.date="07/12/2016"
ms.author="eugenesh" />

# <a name="indexing-csv-blobs-with-azure-search-blob-indexer"></a>Indeksoinnin CSV-BLOB Azure haun blob indeksointitoiminto kanssa 

Oletusarvon mukaan [Azure haun blob indeksointitoiminto](search-howto-indexing-azure-blob-storage.md) jäsentää erotettu teksti BLOB kuin yhden lohkon tekstin. Kuitenkin ja BLOB-objektit, joka sisältää CSV-tiedot, haluat ehkä käsittele kunkin rivin Blob-objektien erillisenä tiedostona. Valita esimerkiksi seuraavat erotinmerkkejä: 

    id, datePublished, tags
    1, 2016-01-12, "azure-search,azure,cloud" 
    2, 2016-07-07, "cloud,mobile" 

haluat ehkä jäsentää 2 asiakirjoihin kunkin sisältävä "tunnus", "datePublished" ja "tunnisteet-kentät.

Tässä artikkelissa kerrotaan jäsentäminen CSV-BLOB haun Azure-Blob-objektien indeksointitoiminto kanssa. 

> [AZURE.IMPORTANT] Tämä toiminto on tällä hetkellä esikatselu. Se on käytettävissä vain REST-Ohjelmointirajapinnalla versio **2015-02-28-esikatselu**. Ota muista, esikatsella ohjelmointirajapinnan on tarkoitettu testaus ja arviointi ja ei voi käyttää tuotantoympäristössä. 

## <a name="setting-up-csv-indexing"></a>CSV-indeksoinnin määrittäminen

Indeksi CSV-BLOB-objektit, luominen tai päivittäminen indeksointitoiminto määritelmä `delimitedText` jäsennyksen tila:  

    {
      "name" : "my-csv-indexer",
      ... other indexer properties
      "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "firstLineContainsHeaders" : true } }
    }

Lisätietoja Ohjelmointirajapinnan indeksointitoiminto luominen Tutustu [Luominen indeksointitoiminto](search-api-indexers-2015-02-28-preview.md#create-indexer).

`firstLineContainsHeaders`Ilmaisee, että kunkin blob (muuta kuin tyhjää) ensimmäinen rivi sisältää otsikot.
Jos blob ei sisällä alkuperäisen otsikkorivi, otsikot määritettävä indeksointitoiminto määritykset: 

    "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "delimitedTextHeaders" : "id,datePublished,tags" } } 

Tällä hetkellä vain UTF-8-koodausta tuetaan. Lisäksi vain pilkku `','` merkin tuetaan erottimena. Jos tarvitset tukea muita koodausten tai erottimet, kerro meille [UserVoice](https://feedback.azure.com/forums/263029-azure-search)-sivuston.

> [AZURE.IMPORTANT] Jäsennyksen tilassa erotinmerkkejä käytettäessä Azure haun oletetaan olevan tietolähteen kaikki Blob-objektien CSV. Jos tarvitset tukea CSV- ja CSV BLOB-objektit erilaisten saman tietolähteen, kerro meille [UserVoice](https://feedback.azure.com/forums/263029-azure-search)-sivuston.

## <a name="request-examples"></a>Esimerkkejä pyytäminen

Laajennettujen kaikki yhdessä, seuraavassa on esimerkkejä valmiina tiedot. 

Tietolähde: 

    POST https://[service name].search.windows.net/datasources?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "my-blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "<my storage connection string>" },
        "container" : { "name" : "my-container", "query" : "<optional, my-folder>" }
    }   

Indeksointitoiminto:

    POST https://[service name].search.windows.net/indexers?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "my-csv-indexer",
      "dataSourceName" : "my-blob-datasource",
      "targetIndexName" : "my-target-index",
      "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "delimitedTextHeaders" : "id,datePublished,tags" } }
    }

## <a name="help-us-make-azure-search-better"></a>Auta meitä parantamaan Azure haun toimintaa

Jos sinulla on ehdottaa ominaisuuksia tai ideoita tehokkuutta, ota saavuttaminen meille [UserVoice-sivustossa](https://feedback.azure.com/forums/263029-azure-search/).