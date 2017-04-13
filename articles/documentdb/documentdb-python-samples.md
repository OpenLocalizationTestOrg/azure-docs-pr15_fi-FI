<properties 
    pageTitle="Esimerkkejä NoSQL Python DocumentDB | Microsoft Azure" 
    description="Etsi NoSQL Python esimerkkejä-github DocumentDB, mukaan lukien CRUD toimintojen JSON asiakirjojen NoSQL tietokantojen Yleiset toiminnot." 
    keywords="Python esimerkkejä"
    services="documentdb" 
    authors="moderakh" 
    manager="jhubbard" 
    editor="monicar" 
    documentationCenter="python"/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="04/18/2016" 
    ms.author="moderakh"/>


# <a name="documentdb-python-examples"></a>DocumentDB Python esimerkkejä

> [AZURE.SELECTOR]
- [.NET-esimerkkejä](documentdb-dotnet-samples.md)
- [Node.js esimerkkejä](documentdb-nodejs-samples.md)
- [Python esimerkkejä](documentdb-python-samples.md)
- [Azure koodin otoksen valikoima](https://azure.microsoft.com/documentation/samples/?service=documentdb)

Esimerkki ratkaisuja, jotka suorittavat CRUD-toimintojen ja muita yleisiä toimintoja Azure DocumentDB resurssien sisältyvät [azure-documentdb-python](https://github.com/Azure/azure-documentdb-python/tree/master/samples) GitHub säilö. Tässä artikkelissa:

- Linkit Python Esimerkki projektitiedoston tehtävät. 
- Linkkejä aiheeseen liittyvät ohjelmointirajapinnan viitata sisältöä.

**Edellytykset**

1. Sinun on käytettävä Python esimerkit Azure tili:
    - Voit [avata Azure-tili maksutta](https://azure.microsoft.com/pricing/free-trial/): Saat hyvitykset avulla voit kokeilla maksettu Azure services ja senkin jälkeen, kun niitä käytetään enintään voit pitää tilin ja käyttää vapaa Azure-palvelut, kuten sivustot. Luottokorttisi koskaan veloitetaan, ellet nimenomaisesti muuttaminen ja pyydä veloitetaan.
   - Voit [aktivoida Visual Studio tilaajan edut](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/): Your Visual Studio tilauksen käyttöösi hyvitykset joka kuukausi, jonka maksettu Azure-palveluiden avulla.
2. Sinun on myös [Python SDK-paketissa](documentdb-sdk-python.md). 

    > [AZURE.NOTE] Jokainen malli on itsenäinen, se itse määrittää ja tyhjentää jälkeen itse. Näin ollen mallit antaa [document_client useita kutsuja. CreateCollection](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html). Aina, kun se tehdään tilauksen laskutetaan on käyttö kohti luodaan sivustokokoelman suorituskyky-taso 1 tunti. 

## <a name="database-examples"></a>Tietokannan esimerkkejä

[DatabaseManagement](https://github.com/Azure/azure-documentdb-python/tree/master/samples/DatabaseManagement) projektin [Program.py](https://github.com/Azure/azure-documentdb-python/tree/master/samples/DatabaseManagement/Program.py) -tiedosto näyttää, miten voit suorittaa seuraavat tehtävät.

Tehtävä | API-viittaus
--- | ---
[Tietokannan luominen](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L65-L76) | [document_client. CreateDatabase](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html)
[Kyselyn tilin tietokantaan](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L49-L62) | [document_client. QueryDatabases](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html)
[Lue tietokannan tunnuksen mukaan](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L79-L96) | [document_client. ReadDatabase](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html)
[Luettelon tietokantojen tiliä varten](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L99-L110) | [document_client. ReadDatabases](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html)
[Tietokannan poistaminen](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L113-L126) | [document_client. DeleteDatabase](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html)

## <a name="collection-examples"></a>Sivustokokoelman esimerkkejä 

[CollectionManagement](https://github.com/Azure/azure-documentdb-python/tree/master/samples/CollectionManagement) projektin [Program.py](https://github.com/Azure/azure-documentdb-python/tree/master/samples/CollectionManagement/Program.py) -tiedosto näyttää, miten voit suorittaa seuraavat tehtävät.

Tehtävä | API-viittaus
--- | ---
[Luo kokoelma](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L84-L135) | [document_client. CreateCollection](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection)
[Lue luettelo tietokannan kaikki sivustokokoelmat](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L198-L225) | [document_client. ListCollections](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection)
[Hae sivustokokoelman tunnuksen mukaan](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L178-L195) | [document_client. ReadCollection](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection)
[Hae kokoelman suorituskyvyn taso](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L139-L161) | [DocumentQueryable.QueryOffers](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection)
[Muuta suorituskyvyn taso kokoelman](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L163-L175) | [document_client. ReplaceOffer](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection)
[Poista kokoelma](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L212-L225) | [document_client. DeleteCollection](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection)
