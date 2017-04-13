<properties
    pageTitle="Esimerkkejä NoSQL Node.js DocumentDB | Microsoft Azure"
    description="Etsi NoSQL Node.js esimerkkejä-github DocumentDB, mukaan lukien CRUD toimintojen JSON asiakirjojen NoSQL tietokantojen Yleiset toiminnot."
    keywords="node.js esimerkkejä"
    services="documentdb"
    authors="moderakh"
    manager="jhubbard"
    editor="monicar"
    documentationCenter="nodejs"/>

<tags
    ms.service="documentdb"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="moderakh"/>


# <a name="documentdb-nodejs-examples"></a>DocumentDB Node.js esimerkkejä

> [AZURE.SELECTOR]
- [.NET-esimerkkejä](documentdb-dotnet-samples.md)
- [Node.js esimerkkejä](documentdb-nodejs-samples.md)
- [Python esimerkkejä](documentdb-python-samples.md)
- [Azure koodin otoksen valikoima](https://azure.microsoft.com/documentation/samples/?service=documentdb)

Esimerkki ratkaisuja, jotka suorittavat CRUD-toimintojen ja muita yleisiä toimintoja Azure DocumentDB resurssien sisältyvät [azure-documentdb-nodejs](https://github.com/Azure/azure-documentdb-node/tree/master/samples) GitHub säilö. Tässä artikkelissa:

- Linkit Node.js Esimerkki projektitiedoston tehtävät.
- Linkkejä aiheeseen liittyvät ohjelmointirajapinnan viitata sisältöä.

**Edellytykset**

1. Sinun on käytettävä Node.js esimerkit Azure tili:
    - Voit [avata Azure-tili maksutta](https://azure.microsoft.com/pricing/free-trial/): Saat hyvitykset avulla voit kokeilla maksettu Azure services ja senkin jälkeen, kun niitä käytetään enintään voit pitää tilin ja käyttää vapaa Azure-palvelut, kuten sivustot. Luottokorttisi koskaan veloitetaan, ellet nimenomaisesti muuttaminen ja pyydä veloitetaan.
   - Voit [aktivoida Visual Studio tilaajan edut](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/): Your Visual Studio tilauksen käyttöösi hyvitykset joka kuukausi, jonka maksettu Azure-palveluiden avulla.
2. Sinun on myös [Node.js SDK-paketissa](documentdb-sdk-node.md).

    > [AZURE.NOTE] Jokainen malli on itsenäinen, se itse määrittää ja tyhjentää jälkeen itse. Näin ollen mallit antaa [DocumentClient.createCollection](http://azure.github.io/azure-documentdb-node/DocumentClient.html#createCollection)useita kutsuja. Aina, kun se tehdään tilauksen laskutetaan on käyttö kohti luodaan sivustokokoelman suorituskyky-taso 1 tunti.

## <a name="database-examples"></a>Tietokannan esimerkkejä

[DatabaseManagement](https://github.com/Azure/azure-documentdb-node/tree/master/samples/DatabaseManagement) projektin [app.js](https://github.com/Azure/azure-documentdb-node/blob/master/samples/DatabaseManagement/app.js) -tiedosto näyttää, miten voit suorittaa seuraavat tehtävät.

Tehtävä | API-viittaus
--- | ---
[Tietokannan luominen](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DatabaseManagement/app.js#L121-L131) | [DocumentClient.createDatabase](http://azure.github.io/azure-documentdb-node/DocumentClient.html#createDatabase)
[Kyselyn tilin tietokantaan](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DatabaseManagement/app.js#L146-L171) | [DocumentClient.queryDatabases](http://azure.github.io/azure-documentdb-node/DocumentClient.html#queryDatabases)
[Lue tietokannan tunnuksen mukaan](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DatabaseManagement/app.js#L89-L99) | [DocumentClient.readDatabase](http://azure.github.io/azure-documentdb-node/DocumentClient.html#readDatabase)
[Luettelon tietokantojen tiliä varten](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DatabaseManagement/app.js#L111-L119) | [DocumentClient.readDatabases](http://azure.github.io/azure-documentdb-node/DocumentClient.html#readDatabase)
[Tietokannan poistaminen](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DatabaseManagement/app.js#L133-L144) | [DocumentClient.deleteDatabase](http://azure.github.io/azure-documentdb-node/DocumentClient.html#deleteDatabase)

## <a name="collection-examples"></a>Sivustokokoelman esimerkkejä

[CollectionManagement](https://github.com/Azure/azure-documentdb-node/tree/master/samples/CollectionManagement) projektin [app.js](https://github.com/Azure/azure-documentdb-node/blob/master/samples/CollectionManagement/app.js) -tiedosto näyttää, miten voit suorittaa seuraavat tehtävät.

Tehtävä | API-viittaus
--- | ---
[Luo kokoelma](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.CollectionManagement/app.js#L97-L118) | [DocumentClient.createCollection](http://azure.github.io/azure-documentdb-node/DocumentClient.html#createCollection)
[Lue luettelo tietokannan kaikki sivustokokoelmat](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.CollectionManagement/app.js#L120-L130) | [DocumentClient.readCollections](http://azure.github.io/azure-documentdb-node/DocumentClient.html#readCollections)
[Voit saada kokoelma _self](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.CollectionManagement/app.js#L132-L141) | [DocumentClient.readCollection](http://azure.github.io/azure-documentdb-node/DocumentClient.html#readCollection)
[Hae sivustokokoelman tunnuksen mukaan](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.CollectionManagement/app.js#L143-L156) | [DocumentClient.readCollection](http://azure.github.io/azure-documentdb-node/DocumentClient.html#readCollection)
[Hae kokoelman suorituskyvyn taso](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.CollectionManagement/app.js#L158-L186) | [DocumentQueryable.queryOffers](http://azure.github.io/azure-documentdb-node/DocumentClient.html#queryOffers)
[Muuta suorituskyvyn taso kokoelman](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.CollectionManagement/app.js#L188-L202) | [DocumentClient.replaceOffer](http://azure.github.io/azure-documentdb-node/DocumentClient.html#replaceOffer)
[Poista kokoelma](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.CollectionManagement/app.js#L204-L215) | [DocumentClient.deleteCollection](http://azure.github.io/azure-documentdb-node/DocumentClient.html#deleteCollection)

## <a name="document-examples"></a>Asiakirjan esimerkkejä

[DocumentManagement](https://github.com/Azure/azure-documentdb-node/tree/master/samples/DocumentManagement) projektin [app.js](https://github.com/Azure/azure-documentdb-node/blob/master/samples/DocumentManagement/app.js) -tiedosto näyttää, miten voit suorittaa seuraavat tehtävät.

Tehtävä | API-viittaus
--- | ---
[Tiedostojen luominen](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DocumentManagement/app.js#L153-L177) | [DocumentClient.createDocument](http://azure.github.io/azure-documentdb-node/DocumentClient.html#createDocument)
[Syötteen kokoelman asiakirjan lukeminen](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DocumentManagement/app.js#L179-L189) | [DocumentClient.readDocument](http://azure.github.io/azure-documentdb-node/DocumentClient.html#readDocument)
[Tunnuksen mukaan asiakirjan lukeminen](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DocumentManagement/app.js#L191-L201) | [DocumentClient.readDocument](http://azure.github.io/azure-documentdb-node/DocumentClient.html#readDocument)
[Asiakirjan lukeminen vain, jos tiedosto on muuttunut](https://github.com/Azure/azure-documentdb-node/blob/0778eadea7abb2af41e8c22a239dc872c584f421/samples/DocumentManagement/app.js#L79-L107) | [DocumentClient.readDocument](http://azure.github.io/azure-documentdb-node/DocumentClient.html#readDocument)<br/>[RequestOptions.accessCondition](http://azure.github.io/azure-documentdb-node/global.html#RequestOptions)
[Kyselyn asiakirjoille](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DocumentManagement/app.js#L82-L110) | [DocumentClient.queryDocuments](http://azure.github.io/azure-documentdb-node/DocumentClient.html#queryDocuments)
[Korvaa tiedoston](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DocumentManagement/app.js#L112-L119) |  [DocumentClient.replaceDocument](http://azure.github.io/azure-documentdb-node/DocumentClient.html#replaceDocument)
[Korvaa asiakirjan ehdollinen ETag-valintaruutu](https://github.com/Azure/azure-documentdb-node/blob/0778eadea7abb2af41e8c22a239dc872c584f421/samples/DocumentManagement/app.js#L147-L164) |  [DocumentClient.replaceDocument](http://azure.github.io/azure-documentdb-node/DocumentClient.html#replaceDocument)<br/>[RequestOptions.accessCondition](http://azure.github.io/azure-documentdb-node/global.html#RequestOptions)
[Tiedoston poistaminen](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.DocumentManagement/app.js#L122-L133) | [DocumentClient.deleteDocument](http://azure.github.io/azure-documentdb-node/DocumentClient.html#deleteDocument)

## <a name="indexing-examples"></a>Esimerkkejä indeksointi

[IndexManagement](https://github.com/Azure/azure-documentdb-node/tree/master/samples/IndexManagement) projektin [app.js](https://github.com/Azure/azure-documentdb-node/blob/master/samples/IndexManagement/app.js) -tiedosto näyttää, miten voit suorittaa seuraavat tehtävät.

Tehtävä | API-viittaus
--- | ---
Luo kokoelma oletusarvon indeksoinnin avulla | [DocumentClient.createDocument](http://azure.github.io/azure-documentdb-node/DocumentClient.html)
[Voit tarkastella tiettyä asiakirjaa indeksoi manuaalisesti](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.IndexManagement/app.js#L185-L238) | [indexingDirective: Sisällytä"](http://azure.github.io/azure-documentdb-node/global.html#indexingDirective)
[Voit tarkastella tiettyä asiakirjaa pois hakemiston manuaalisesti](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.IndexManagement/app.js#L120-L183) | [RequestOptions.indexingDirective](http://azure.github.io/azure-documentdb-node/global.html#RequestOptions)
[Käytä viive-indeksointi joukkona tuontia tai lukea paksu sivustokokoelmat](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.IndexManagement/app.js#L240-L269) | [IndexingMode.Lazy](http://azure.github.io/azure-documentdb-node/global.html#IndexingMode)
[Indeksoinnin asiakirjan tiettyjen polkujen sisällyttäminen](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.IndexManagement/app.js#L433-L444) | [IndexingPolicy.IncludedPaths](http://azure.github.io/azure-documentdb-node/global.html#IndexingPolicy)
[Indeksoinnin tiettyjen polkujen pois](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.IndexManagement/app.js#L427-L450) | [ExcludedPath](http://azure.github.io/azure-documentdb-node/global.html#IndexingPolicy)
[Salli tarkistuksen merkkijonon reitillä alue-toiminnon aikana](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.IndexManagement/app.js#L271-L347)| [ExcludedPath.EnableScanInQuery](http://azure.github.io/azure-documentdb-node/global.html#FeedOptions)
[Merkkijono reitillä alueen indeksin luominen](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.IndexManagement/app.js#L349-L425) | [DocumentClient.queryDocument](http://azure.github.io/azure-documentdb-node/DocumentClient.html#queryDocument)
[Luo kokoelma oletusarvon indexPolicy ja valitse Päivitä verkossa](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.IndexManagement/app.js#L519-L614) | [DocumentClient.createCollection](http://azure.github.io/azure-documentdb-node/DocumentClient.html#createCollection)<br> [DocumentClient.replaceCollection#replaceCollection](http://azure.github.io/azure-documentdb-node/DocumentClient.html)

Saat lisätietoja indeksoinnin [DocumentDB indeksoinnin käytännöt](documentdb-indexing-policies.md).

## <a name="server-side-programming-examples"></a>Palvelinpuolen ohjelmoinnin esimerkkejä

[ServerSideScripts](https://github.com/Azure/azure-documentdb-node/tree/master/samples/ServerSideScripts) projektin [app.js](https://github.com/Azure/azure-documentdb-node/blob/master/samples/ServerSideScripts/app.js) -tiedosto näyttää, miten voit suorittaa seuraavat tehtävät.

Tehtävä | API-viittaus
--- | ---
[Luo tallennettu toimintosarja](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.ServerSideScripts/app.js#L44-L71) | [DocumentClient.createStoredProcedure](http://azure.github.io/azure-documentdb-node/DocumentClient.html#createStoredProcedure)
[Tallennetun toimintosarjan suorittaminen](https://github.com/Azure/azure-documentdb-node/blob/ef53e5f6707a5dc45920fb6ad54d9c7e008a6c18/samples/DocumentDB.Samples.ServerSideScripts/app.js#L73-L90) | [DocumentClient.executeStoredProcedure](http://azure.github.io/azure-documentdb-node/DocumentClient.html#executeStoredProcedure)

Saat lisätietoja palvelinpuolen ohjelmoinnin [DocumentDB palvelinpuolen ohjelmoinnin: tallennettujen toimintosarjojen, tietokannan käynnistimien ja UDF](documentdb-programming.md).

## <a name="partitioning-examples"></a>Esimerkkejä jakaminen

[Osioimisen](https://github.com/Azure/azure-documentdb-node/tree/master/samples/Partitioning) projektin [app.js](https://github.com/Azure/azure-documentdb-node/blob/master/samples/Partitioning/app.js) -tiedosto näyttää, miten voit suorittaa seuraavat tehtävät.

Tehtävä | API-viittaus
--- | ---
[Käytä HashPartitionResolver](https://github.com/Azure/azure-documentdb-node/blob/ce0fc3c4e70b0279091a1e03620a668d93a14fc2/samples/Partitioning/app.js#L53-L103) | [HashPartitionResolver](http://azure.github.io/azure-documentdb-node/HashPartitionResolver.html)

Jos haluat lisätietoja DocumentDB tietojen jakaminen artikkelissa [osio ja skaalausta tietojen DocumentDB](documentdb-partition-data.md).
