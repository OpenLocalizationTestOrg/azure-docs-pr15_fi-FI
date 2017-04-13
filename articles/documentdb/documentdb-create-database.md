<properties 
    pageTitle="Tietokannan luominen DocumentDB | Microsoft Azure" 
    description="Lue, miten voit luoda online-palvelun-portaalin käytön Azure DocumentDB blazing nopea ja maailmanlaajuisesti NoSQL tietokannan tietokannan." 
    keywords="tietokannan luominen" 
    services="documentdb" 
    authors="mimig1" 
    manager="jhubbard" 
    editor="monicar" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/17/2016" 
    ms.author="mimig"/>

# <a name="how-to-create-a-database-for-documentdb-using-the-azure-portal"></a>Tietokannan luominen DocumentDB Azure-portaalissa

Käyttämään Microsoft Azure DocumentDB on oltava [DocumentDB tili](documentdb-create-account.md), tietokannan, kokoelma ja asiakirjoja. Azure-portaalissa DocumentDB tietokannat nyt luodaan samaan aikaan kuin kokoelma luodaan. 

DocumentDB tietokanta ja sivustokokoelman luominen portaalin, katso, [miten voit luoda DocumentDB kokoelman Azure-portaalissa](documentdb-create-collection.md).

## <a name="other-ways-to-create-a-documentdb-database"></a>Muita tapoja DocumentDB tietokannan luominen

Tietokantoja ei tarvitse luoda-portaalissa, voit myös luoda ne [DocumentDB SDK: T](documentdb-sdk-dotnet.md) tai [REST API](https://msdn.microsoft.com/library/mt489072.aspx)avulla. Lisätietoja tietokantojen käyttäminen käyttämällä .NET SDK on esimerkkejä [.NET tietokannan](documentdb-dotnet-samples.md#database-examples). Lisätietoja tietokantojen käyttäminen käyttämällä Node.js SDK on esimerkkejä [Node.js tietokannan](documentdb-nodejs-samples.md#database-examples). 

## <a name="next-steps"></a>Seuraavat vaiheet

Kun tietokanta ja sivustokokoelman on luotu, voit [lisätä JSON tiedostot](documentdb-view-json-document-explorer.md) Resurssienhallinnassa asiakirjan portaalissa [tuoda asiakirjoja](documentdb-import-data.md) sivustokokoelman DocumentDB tietojen siirto-työkalun avulla tai käyttämällä jotakin [DocumentDB SDK: T](documentdb-sdk-dotnet.md) suorittaa CRUD. DocumentDB on .NET, Java, Python, Node.js ja JavaScript-Ohjelmointirajapinnan SDK: T. Katso .NET MALLIKOODEJA esittävä luominen, poistaminen, päivittää ja poistaa asiakirjoja, [.NET asiakirjan esimerkkejä](documentdb-dotnet-samples.md#document-examples). Lisätietoja tiedostojen käsitteleminen käyttämällä Node.js SDK on esimerkkejä [Node.js asiakirjan](documentdb-nodejs-samples.md#document-examples). 

Kun tiedostot ovat kokoelma, voit [DocumentDB SQL](documentdb-sql-query.md) suorittaa [kyselyjä](documentdb-sql-query.md#executing-sql-queries) vastaan tiedostojen käyttämällä [Kyselyn Explorer](documentdb-query-collections-query-explorer.md) -portaalissa, [REST API](https://msdn.microsoft.com/library/azure/dn781481.aspx)tai jokin [SDK: T](documentdb-sdk-dotnet.md). 
