<properties
    pageTitle="Tietojen lataaminen Azure hakutoiminnossa | Microsoft Azure | Isännöityjen pilvipalvelussa haku"
    description="Tietoja tietojen lataaminen indeksin Azure hakutoiminnolla."
    services="search"
    documentationCenter=""
    authors="ashmaka"
    manager="jhubbard"
    editor=""
    tags=""/>

<tags
    ms.service="search"
    ms.devlang="NA"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="ashmaka"/>

# <a name="upload-data-to-azure-search"></a>Tietojen lataaminen Azure haku
> [AZURE.SELECTOR]
- [Yleiskatsaus](search-what-is-data-import.md)
- [.NET](search-import-data-dotnet.md)
- [MUILLE KÄYTTÄJILLE](search-import-data-rest-api.md)


## <a name="data-upload-models-in-azure-search"></a>Tietojen lataaminen Azure hakutoiminnossa mallit
Täytä Azure etsinnän indeksiin tietoluettelon kahdella eri tavalla. Ensimmäinen vaihtoehto on manuaalisesti valitseminen tietojen Azure-haun [REST API](search-import-data-rest-api.md) -tai [.NET SDK](search-import-data-dotnet.md)indeksiin. Toinen vaihtoehto on Azure-hakuindeksin [Valitse tuetut tietolähteet](search-indexer-overview.md) ja Azure haku noutaa tiedot automaattisesti search-palvelun avulla.

Tämän oppaan vain kattavat käyttämisestä (joka tuetaan vain [REST API](search-import-data-rest-api.md) ja [.NET SDK](search-import-data-dotnet.md)-) tietojen siirto push-mallin, mutta voit silti Lue lisää alla salaus puretaan mallin.

### <a name="push-data-to-an-index"></a>Indeksin Push tiedot

Tämän menetelmän viittaa tietojen lähettäminen Azure hakuun, jotta se voidaan etsiä ohjelmallisesti. On erittäin pienen viive vaatimukset sovellusten (esimerkiksi jos etsit on toiminnot on dynaaminen varaston tietokantojen käyttäminen synkronoinnissa), push-malli on ainoa vaihtoehto.

Voit käyttää [REST API](https://msdn.microsoft.com/library/azure/dn798930.aspx) tai [.NET SDK](search-import-data-dotnet.md) indeksin push tietoihin. Tällä hetkellä ennen tietojen portaalin kautta työkalu ei tue.

Tämä vaihtoehto on joustavampaa kuin salaus puretaan mallin, koska voit ladata tiedostoja yksitellen tai erissä (enintään 1 000 erä tai 16 Megatavua kohti, kumpi raja toteutuu ensin). Push-mallin avulla voit ladata tiedostoja Azure Etsi riippumatta siitä, joissa tiedot ovat.

### <a name="pull-data-into-an-index"></a>Voit noutaa tietoja indeksin

Salaus puretaan mallin käy läpi tuetut tietolähteet ja lataa tiedot Azure haun indeksiin automaattisesti puolestasi. Seuraamalla muutokset ja poistaa aiemmin luotuja tiedostoja lisäksi tunnista uusia asiakirjoja Indeksoijilla poistaa hallinta aktiivisesti indeksi tietoja ei tarvitse.

Azure hakutoiminnossa tämä ominaisuus on toteutettu *Indeksoijilla*, tällä hetkellä käytettävissä [Blob storage (esikatselu)](search-howto-indexing-azure-blob-storage.md)- [DocumentDB](http://aka.ms/documentdb-search-indexer) [Azure SQL-tietokantaan ja SQL Server Azure VMs](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers-2015-02-28.md)kautta.

[Azure-portaalin](search-import-data-portal.md) sekä kuin [REST API](https://msdn.microsoft.com/library/azure/dn946891.aspx)näkyvät indeksointitoiminto-toimintoja.
