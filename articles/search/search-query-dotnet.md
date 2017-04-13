<properties
    pageTitle="Käyttämällä .NET SDK oman Azure-hakuindeksin kyselyn | Microsoft Azure | Isännöityjen pilvipalvelussa haku"
    description="Muodosta Azure hakutoiminnossa hakukyselyn ja Etsi parametreilla, suodattaminen ja lajitteleminen hakutulokset."
    services="search"
    manager="jhubbard"
    documentationCenter=""
    authors="brjohnstmsft"
/>

<tags
    ms.service="search"
    ms.devlang="dotnet"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="brjohnst"/>

# <a name="query-your-azure-search-index-using-the-net-sdk"></a>Kyselyn lisääminen käyttämällä .NET SDK Azure hakuindeksin
> [AZURE.SELECTOR]
- [Yleiskatsaus](search-query-overview.md)
- [Portal](search-explorer.md)
- [.NET](search-query-dotnet.md)
- [MUILLE KÄYTTÄJILLE](search-query-rest-api.md)

Tässä artikkelissa kerrotaan, miten indeksin avulla [Azure haun .NET SDK](https://msdn.microsoft.com/library/azure/dn951165.aspx)kysely.

Ennen aloittamista tätä vaiheittaista pitäisi olla jo [Azure-hakuindeksin luodaan](search-what-is-an-index.md) ja [täytetään sen tiedoilla](search-what-is-data-import.md).

Huomaa, että kaikki tämän artikkelin Esimerkki koodi kirjoitetaan C#. Voit etsiä koko tietolähteen [GitHub](http://aka.ms/search-dotnet-howto)-koodiin.

## <a name="i-identify-your-azure-search-services-query-api-key"></a>Voin. Määritä Azure Search-palvelun kyselyn api-avain
Nyt kun olet luonut Azure-hakuindeksin, olet melkein valmis antamaan kyselyjen .NET SDK. Sinun on ensin jokin kyselyn api-näppäimiä, joka on luotu hakupalvelun voit valmistelun yhteydessä. .NET SDK Lähetä tätä api-näppäintä jokaisen pyynnön palvelussa. Salli kohti pyynnön välein, lähettää pyynnön ja -palvelu, joka käsittelee sen sovelluksen välillä on kelvollinen avain vahvistetaan.

1. Etsi oman service api-näppäimiä sinun on kirjauduttava [Azure-portaalissa](https://portal.azure.com/)
2. Siirry Azure Search-palvelun sivu
3. Valitse "Näppäimet"-kuvake

Palvelussa on *järjestelmänvalvojan näppäimet* ja *kysely-näppäimiä*.

  - Ensisijaisen ja toissijaisen *järjestelmänvalvojan näppäimet* myöntää kaikki toiminnot, mukaan lukien pätevyys hallita palvelua, luominen ja poistaminen indeksit Indeksoijilla ja tietolähteiden täydet oikeudet. On kaksi näppäimet, jotta voit edelleen käyttää toissijaisen avaimen, jos uudelleen perusavain ja päinvastoin.
  - *Kyselyn näppäimet* myöntää indeksit ja tiedostojen käyttöoikeus vain luku- ja jaetaan yleensä asiakassovelluksiin, joka antaa search-pyyntöjen.

Kyselyt indeksin tarkoitetaan jollakin kysely-näppäimiä. Järjestelmänvalvoja-näppäimiä voidaan myös kyselyjen, mutta sinun on käytettävä kyselyn avain sovelluksen koodissa seuraavasti tämä paremmin [käyttöoikeuksien myöntäminen tarpeen mukaan](https://en.wikipedia.org/wiki/Principle_of_least_privilege).

## <a name="ii-create-an-instance-of-the-searchindexclient-class"></a>II. SearchIndexClient-luokan luominen
Antamaan kyselyjen Azure haun .NET SDK tarvitset luoda esiintymää `SearchIndexClient` luokka. Tämä luokka on useita konstruktoreja. Haluamasi kestää search-palvelunimi, Indeksinimi ja `SearchCredentials` parametreiksi objekti. `SearchCredentials`rivittää api-näppäintä.

Seuraava koodi luo uuden `SearchIndexClient` (luodaan [käyttämällä .NET SDK Azure Search-indeksin](search-create-index-dotnet.md)) "Hotellit" indeksin käyttämällä arvoja search-palvelunimi ja api-näppäintä, jotka on tallennettu sovelluksen määritystiedosto (`app.config` tai `web.config`):

```csharp
string searchServiceName = ConfigurationManager.AppSettings["SearchServiceName"];
string queryApiKey = ConfigurationManager.AppSettings["SearchServiceQueryApiKey"];

SearchIndexClient indexClient = new SearchIndexClient(searchServiceName, "hotels", new SearchCredentials(queryApiKey));
```

`SearchIndexClient`on `Documents` ominaisuus. Tämä ominaisuus on kaikki menetelmät, sinun täytyy Azure-hakuindeksin kyselyn.

## <a name="iii-query-your-index"></a>III. Kyselyn indeksi
.NET SDK etsimällä sujuu yhtä helposti kuin soitat `Documents.Search` menetelmän oman `SearchIndexClient`. Tämä menetelmä kestää muutaman parametrit-etsittävän tekstin, mukaan lukien pystysuuntaiselle, jossa `SearchParameters` objektia, jonka avulla voit tarkentaa hakua kyselyn.

#### <a name="types-of-queries"></a>Kyselytyyppejä
Kaksi tärkeimmät [Kyselytyypit](search-query-overview.md#types-of-queries) käytät ovat `search` ja `filter`. A `search` kysely etsii _kaikki hakukenttiä, indeksi-_ vähintään yksi ehtojen perusteella. A `filter` kyselyn arvioi totuusarvolauseke kaikista _suodatettavia_ indeksin kenttien.

Hakujen ja suodattimet suoritetaan käyttämällä `Documents.Search` menetelmää. Hakukyselyn voidaan välittää `searchText` parametri, kun suodatin-lausekkeen voidaan välittää `Filter` -ominaisuuden `SearchParameters` luokan. Suodattamiseen ilman haun vain välittää `"*"` varten `searchText` parametria. Etsi ilman suodattaminen, jätä `Filter` ominaisuuden poistaminen, tai välitä `SearchParameters` esiintymän ollenkaan.

#### <a name="example-queries"></a>Esimerkki kyselyt

Seuraava esimerkki koodi näyttää "Hotellit"-indeksi, joka on määritetty luominen [käyttämällä .NET SDK Azure hakuindeksin](search-create-index-dotnet.md#DefineIndex)kyselyn muutamalla eri tavalla. Huomaa, että palautettu hakutulokset tiedostot ovat esiintymät `Hotel` luokka, joka on määritetty [Tietojen tuontia Azure hakutuloksia hyödyntämällä .NET SDK](search-import-data-dotnet.md#HotelClass). Sample code käytetään `WriteDocuments` tavan tulosteen konsolin hakutulokset. Tämä menetelmä on kuvattu seuraavassa osassa.

```csharp
SearchParameters parameters;
DocumentSearchResult<Hotel> results;

Console.WriteLine("Search the entire index for the term 'budget' and return only the hotelName field:\n");

parameters =
    new SearchParameters()
    {
        Select = new[] { "hotelName" }
    };

results = indexClient.Documents.Search<Hotel>("budget", parameters);

WriteDocuments(results);

Console.Write("Apply a filter to the index to find hotels cheaper than $150 per night, ");
Console.WriteLine("and return the hotelId and description:\n");

parameters =
    new SearchParameters()
    {
        Filter = "baseRate lt 150",
        Select = new[] { "hotelId", "description" }
    };

results = indexClient.Documents.Search<Hotel>("*", parameters);

WriteDocuments(results);

Console.Write("Search the entire index, order by a specific field (lastRenovationDate) ");
Console.Write("in descending order, take the top two results, and show only hotelName and ");
Console.WriteLine("lastRenovationDate:\n");

parameters =
    new SearchParameters()
    {
        OrderBy = new[] { "lastRenovationDate desc" },
        Select = new[] { "hotelName", "lastRenovationDate" },
        Top = 2
    };

results = indexClient.Documents.Search<Hotel>("*", parameters);

WriteDocuments(results);

Console.WriteLine("Search the entire index for the term 'motel':\n");

parameters = new SearchParameters();
results = indexClient.Documents.Search<Hotel>("motel", parameters);

WriteDocuments(results);
```

## <a name="iv-handle-search-results"></a>IV. Käsittele hakutulokset
`Documents.Search` Menetelmä palauttaa `DocumentSearchResult` objekti, joka sisältää kyselyn tulokset. Esimerkki edellisessä osassa käytetään menetelmän `WriteDocuments` siirrossa konsolin haun tulokset:

```csharp
private static void WriteDocuments(DocumentSearchResult<Hotel> searchResults)
{
    foreach (SearchResult<Hotel> result in searchResults.Results)
    {
        Console.WriteLine(result.Document);
    }

    Console.WriteLine();
}
```

Näin, miltä tulokset näyttävät kyselyjen edellisessä osassa olettaen "Hotellit" hakemiston lisätään [Tietojen tuontia Azure hakutuloksia hyödyntämällä .NET SDK](search-import-data-dotnet.md)esimerkkitiedot:

```
Search the entire index for the term 'budget' and return only the hotelName field:

Name: Roach Motel

Apply a filter to the index to find hotels cheaper than $150 per night, and return the hotelId and description:

ID: 2   Description: Cheapest hotel in town
ID: 3   Description: Close to town hall and the river

Search the entire index, order by a specific field (lastRenovationDate) in descending order, take the top two results, and show only hotelName and lastRenovationDate:

Name: Fancy Stay        Last renovated on: 6/27/2010 12:00:00 AM +00:00
Name: Roach Motel       Last renovated on: 4/28/1982 12:00:00 AM +00:00

Search the entire index for the term 'motel':

ID: 2   Base rate: 79.99        Description: Cheapest hotel in town     Description (French): Hôtel le moins cher en ville      Name: Roach Motel       Category: Budget        Tags: [motel, budget]   Parking included: yes   Smoking allowed: yes    Last renovated on: 4/28/1982 12:00:00 AM +00:00 Rating: 1/5     Location: Latitude 49.678581, longitude -122.131577

```

Yllä olevan otoksen koodin käyttää konsolin tulosteen hakutulokset. Sinun on myös hakutulokset näkyvät oman sovelluksen. Katso, miten hahmontamiseen hakutulosten ASP.NET MVC-pohjainen web-sovelluksen [käyttöön GitHub tässä esimerkissä](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetSample) .
