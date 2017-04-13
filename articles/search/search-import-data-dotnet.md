<properties
    pageTitle="Tietojen lataaminen Azure hakutuloksia hyödyntämällä .NET SDK | Microsoft Azure | Isännöityjen pilvipalvelussa haku"
    description="Tietoja tietojen lataaminen hakemiston Azure hakutuloksia hyödyntämällä .NET SDK."
    services="search"
    documentationCenter=""
    authors="brjohnstmsft"
    manager="jhubbard"
    editor=""
    tags=""/>

<tags
    ms.service="search"
    ms.devlang="dotnet"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="brjohnst"/>

# <a name="upload-data-to-azure-search-using-the-net-sdk"></a>Tietojen lataaminen Azure hakutuloksia hyödyntämällä .NET SDK
> [AZURE.SELECTOR]
- [Yleiskatsaus](search-what-is-data-import.md)
- [.NET](search-import-data-dotnet.md)
- [MUILLE KÄYTTÄJILLE](search-import-data-rest-api.md)

Tässä artikkelissa kerrotaan, miten [Azure haun .NET SDK](https://msdn.microsoft.com/library/azure/dn951165.aspx) avulla voit tuoda tietoja Azure-hakuindeksin.

Ennen aloittamista tätä vaiheittaista on jo [luotu Azure-hakuindeksin](search-what-is-an-index.md). Tässä artikkelissa oletetaan, että olet jo luonut myös `SearchServiceClient` objektin [luominen käyttämällä .NET SDK Azure haun indeksin](search-create-index-dotnet.md#CreateSearchServiceClient)esitetyllä tavalla.

Huomaa, että kaikki tämän artikkelin Esimerkki koodi kirjoitetaan C#. Voit etsiä koko tietolähteen [GitHub](http://aka.ms/search-dotnet-howto)-koodiin.

Jotta push asiakirjoja sisään käyttämällä .NET SDK indeksi, sinun on:

  1. Luo `SearchIndexClient` muodostaa yhteyttä hakuindeksin objekti.
  2. Luo `IndexBatch` sisältävät asiakirjat voi lisätä, muokata ja poistaa.
  3. Soita `Documents.Index` maksutavan oman `SearchIndexClient` Lähetä `IndexBatch` tekemäsi hakuindeksin.

## <a name="i-create-an-instance-of-the-searchindexclient-class"></a>Voin. SearchIndexClient-luokan luominen
Voit tuoda tietoja käyttämällä Azure haun .NET SDK indeksi, sinun on luoda esiintymää `SearchIndexClient` luokka. Voit luoda tämän esiintymän itse, mutta se on helpompaa, jos sinulla on jo `SearchServiceClient` esiintymän Soita sen `Indexes.GetClient` menetelmää. Esimerkiksi näin miten hankkia `SearchIndexClient` nimeltä "Hotellit" indeksin `SearchServiceClient` nimeltä `serviceClient`:

```csharp
SearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");
```

> [AZURE.NOTE] Tavallinen etsintä-sovelluksessa indeksien hallinnan ja populaation käsittelee erillisenä kyselyjen haku. `Indexes.GetClient`tietoja on helppo täyttää indeksin, koska se tallentaa voit antaa toisen ongelmia `SearchCredentials`. Se tekee tämä välittämällä järjestelmänvalvoja-näppäintä, jonka avulla luodaan `SearchServiceClient` uuteen `SearchIndexClient`. Sovelluksen, joka suorittaa kyselyjä-osassa on kuitenkin luoda `SearchIndexClient` suoraan niin, että voit välittää kyselyn näppäimen sijaan järjestelmänvalvoja-näppäintä. Tämä on [käyttöoikeuksien myöntäminen tarpeen mukaan](https://en.wikipedia.org/wiki/Principle_of_least_privilege) ja avulla voit tehdä sovelluksesta turvallisemman. Löydät lisätietoja järjestelmänvalvojan näppäimet ja [Azure-haun REST-Ohjelmointirajapinta viittaus MSDN-](https://msdn.microsoft.com/library/azure/dn798935.aspx)näppäimiä kyselyn.

`SearchIndexClient`on `Documents` ominaisuus. Tämä ominaisuus on kaikki menetelmät, sinun täytyy lisääminen, muokkaaminen, poistaminen tai kyselyn indeksi tiedostoista.

## <a name="ii-decide-which-indexing-action-to-use"></a>II. Päätä, mitkä indeksoinnin toiminnon käyttäminen
Voit tuoda tietoja käyttämällä .NET SDK, tarvitset paketin määrittäminen tietojen `IndexBatch` objekti. `IndexBatch` Kapseloi kokoelma `IndexAction` objektit, joista jokaisella on asiakirjan ja ominaisuus, joka kertoo Azure haun mitä toiminto tiedostoa (Lataa, Yhdistä, poista jne.). Sen mukaan, mitä alapuolella toiminnot valitset, vain tietyt kentät on oltava kunkin asiakirjan:

Toiminto | Kuvaus | Tarvittavat kunkin asiakirjan kentät | Huomautuksia
--- | --- | --- | ---
`Upload` | `Upload` Toiminto on sama kuin "upsert" kohtaa, johon tiedosto lisätään, jos se on uusi ja päivittää tai korvata jos se on olemassa. | avaimen sekä muita kenttiä, jonka haluat määrittää | Korvattaessa päivittäminen tai aiemmin luodun tiedoston kentän, joka ei ole määritetty pyyntö on sen-kentän arvoksi `null`. Tämä tapahtuu myös silloin, kun kenttä on aiemmin määritetty muut kuin null-arvoa.
`Merge` | Päivittää määritettyihin kenttiin aiemmin luotu tiedosto. Jos tiedosto ei ole indeksin, yhdistäminen epäonnistuu. | avaimen sekä muita kenttiä, jonka haluat määrittää | Määrittämäsi yhdistämiseen kentän korvaa olemassa olevan kentän asiakirjassa. Tämä vaihtoehto sisältää-tyypin kenttiä `DataType.Collection(DataType.String)`. Jos asiakirjassa on kentän esimerkiksi `tags` arvona `["budget"]` ja arvona yhdistämisen suorittaminen `["economy", "pool"]` varten `tags`, viimeinen arvo `tags` kenttä on `["economy", "pool"]`. Se ei ole `["budget", "economy", "pool"]`.
`MergeOrUpload` | Tämä toiminto toimii kuten `Merge` indeksissä aiemmin luodun asiakirjan tietyn-näppäimen kanssa. Jos tiedosto ei ole, se toimii kuten `Upload` ja uuden asiakirjan. | avaimen sekä muita kenttiä, jonka haluat määrittää | -
`Delete` | Poistaa määritetyn asiakirjan indeksi. | vain avain | Kaikki kentät, voit määrittää muut kuin avaimen kentän ohitetaan. Jos haluat poistaa yksittäisen kentän asiakirjasta, käytä `Merge` sijaan ja yksinkertaisesti kentän arvoksi erikseen null.

Toiminto, jota haluat käyttää eri staattinen menetelmillä voit määrittää `IndexBatch` ja `IndexAction` luokkien mukaisesti seuraavaan osaan.

## <a name="iii-construct-your-indexbatch"></a>III. Muodostaa yhteyttä IndexBatch
Nyt kun tiedät, mitkä toiminnot suorittavat tiedostojen, olet valmis, voit muodostaa `IndexBatch`. Alla olevassa esimerkissä esitetään, kuinka voit luoda erää muutamalla eri toimintojen. Huomaa, että tämän esimerkin käyttää mukautetun luokan kutsutaan `Hotel` , joka yhdistää "Hotellit" hakemiston asiakirjaan.

```csharp
var actions =
    new IndexAction<Hotel>[]
    {
        IndexAction.Upload(
            new Hotel()
            {
                HotelId = "1",
                BaseRate = 199.0,
                Description = "Best hotel in town",
                DescriptionFr = "Meilleur hôtel en ville",
                HotelName = "Fancy Stay",
                Category = "Luxury",
                Tags = new[] { "pool", "view", "wifi", "concierge" },
                ParkingIncluded = false,
                SmokingAllowed = false,
                LastRenovationDate = new DateTimeOffset(2010, 6, 27, 0, 0, 0, TimeSpan.Zero),
                Rating = 5,
                Location = GeographyPoint.Create(47.678581, -122.131577)
            }),
        IndexAction.Upload(
            new Hotel()
            {
                HotelId = "2",
                BaseRate = 79.99,
                Description = "Cheapest hotel in town",
                DescriptionFr = "Hôtel le moins cher en ville",
                HotelName = "Roach Motel",
                Category = "Budget",
                Tags = new[] { "motel", "budget" },
                ParkingIncluded = true,
                SmokingAllowed = true,
                LastRenovationDate = new DateTimeOffset(1982, 4, 28, 0, 0, 0, TimeSpan.Zero),
                Rating = 1,
                Location = GeographyPoint.Create(49.678581, -122.131577)
            }),
        IndexAction.MergeOrUpload(
            new Hotel()
            {
                HotelId = "3",
                BaseRate = 129.99,
                Description = "Close to town hall and the river"
            }),
        IndexAction.Delete(new Hotel() { HotelId = "6" })
    };

var batch = IndexBatch.New(actions);
```

Tässä tapauksessa emme käyttävät `Upload`, `MergeOrUpload`, ja `Delete` Microsoftin haun toimenpiteitä, kutsua menetelmillä määritetyllä tavalla `IndexAction` luokka.

Oletetaan, että tämän esimerkin "Hotellit" indeksin lisätään jo asiakirjojen määrä. Huomaa, miten Microsoft ei tarvitse määrittää kaikki mahdolliset asiakirjakentät käytettäessä `MergeOrUpload` ja miten on vain määritetty asiakirjan key (`HotelId`) käytettäessä `Delete`.

Huomaa, että voit lisätä enintään 1 000 tiedostoja vain yhden indeksoinnin pyynnön.

> [AZURE.NOTE] Tässä esimerkissä on ovat käyttö eri toimintoja eri asiakirjoja. Jos haluat määrittää kaikkien asiakirjojen osalta, sen sijaan, että puhelut samassa toimintojen suorittamiseen `IndexBatch.New`, voit käyttää muut staattista tavat, `IndexBatch`. Voit esimerkiksi luoda erissä soittamalla `IndexBatch.Merge`, `IndexBatch.MergeOrUpload`, tai `IndexBatch.Delete`. Näistä tavoista kestää kokoelma asiakirjoja (objektit tyypin `Hotel` tässä esimerkissä) sen sijaan, että `IndexAction` objekteja.

## <a name="iv-import-data-to-the-index"></a>IV. Tietojen tuominen indeksi
Nyt kun olet luonut valmisteltu `IndexBatch` objekti, voit lähettää sen hakemiston soittamalla `Documents.Index` -että `SearchIndexClient` objekti. Seuraavassa esimerkissä esitetään, miten voit soittaa `Index`, sekä joillekin lisävaiheita, sinun on suoritettava:

```csharp
try
{
    indexClient.Documents.Index(batch);
}
catch (IndexBatchException e)
{
    // Sometimes when your Search service is under load, indexing will fail for some of the documents in
    // the batch. Depending on your application, you can take compensating actions like delaying and
    // retrying. For this simple demo, we just log the failed document keys and continue.
    Console.WriteLine(
        "Failed to index some of the documents: {0}",
        String.Join(", ", e.IndexingResults.Where(r => !r.Succeeded).Select(r => r.Key)));
}

Console.WriteLine("Waiting for documents to be indexed...\n");
Thread.Sleep(2000);
```

Huomautus `try` / `catch` ympärillä puhelu `Index` menetelmää. Todellisen estäminen käsittelee on tärkeää virhe tapaukseen indeksoinnin. Jos Azure-hakupalvelun indeksoida joitakin asiakirjoja, valitse haluamasi erä `IndexBatchException` mukaan ilmenee `Documents.Index`. Näin voi käydä, jos tiedostot ovat indeksoinnin palvelun ollessa kuormitettu. **On suositeltavaa käsittely nimenomaisesti tässä tapauksessa koodisi.** Viivästää ja yritä sitten uudelleen indeksointi epäonnistui Poistettavat tiedostot tai voit kirjautua ja jatka kuin otosten ei, tai voit tehdä jotain muuta mukaan sovelluksen tietojen kelpoisuus mukaan.

Lopuksi koodi viiveitä kahden sekunnin ajan yllä olevassa esimerkissä. Indeksoinnin tapahtuu asynkronisesti Azure Search-palvelun, joten sovelluksen malli on oltava odottamaan hetken varmistaa, että tiedostot ovat käytettävissä etsimistä varten. Viiveet tältä ovat yleensä vain tarvittavat esittelyt, Testaa ja otoksen sovellukset.

<a name="HotelClass"></a>
### <a name="how-the-net-sdk-handles-documents"></a>Miten .NET SDK käsittelee asiakirjat

Mutta miten Azure haun .NET SDK on voi ladata palveluun, käyttäjän määrittämän luokan, esiintymät `Hotel` indeksiin. Jotta kyseisen kysymykseen katsotaan `Hotel` luokka, joka liittää indeksi rakenteeseen määritetty [käyttämällä .NET SDK Azure haun indeksin](search-create-index-dotnet.md#DefineIndex)luominen:

```csharp
[SerializePropertyNamesAsCamelCase]
public partial class Hotel
{
    public string HotelId { get; set; }

    public double? BaseRate { get; set; }

    public string Description { get; set; }

    [JsonProperty("description_fr")]
    public string DescriptionFr { get; set; }

    public string HotelName { get; set; }

    public string Category { get; set; }

    public string[] Tags { get; set; }

    public bool? ParkingIncluded { get; set; }

    public bool? SmokingAllowed { get; set; }

    public DateTimeOffset? LastRenovationDate { get; set; }

    public int? Rating { get; set; }

    public GeographyPoint Location { get; set; }

    // ToString() method omitted for brevity...
}
```

Huomaa, että huomaat on kunkin julkisen ominaisuuden `Hotel` vastaa kentän indeksin määrityksen, mutta tärkeitä yksi ero: kunkin kentän nimi kirjaimella alkavaan pienet ("kamelin tapauksessa"), kun kunkin julkisen ominaisuuden nimi `Hotel` alkaa isoilla kirjaimilla kirjaimena ("Pascal tapaus"). Tämä on käytetty vaihtoehto .NET-sovelluksissa, jotka suorittavat tietojen sidonta kohde rakenteen missä sovelluksen kehittäjä ohjausobjektin ulkopuolella. Sen sijaan, että liittyvän .NET nimeäminen ohjeet tekemällä ominaisuuden nimet kamelin kirjaimia, voit selvittää yhdistämään ominaisuuksien nimet automaattisesti kamelin-tapaus SDK `[SerializePropertyNamesAsCamelCase]` määrite.

> [AZURE.NOTE] Azure haun .NET SDK käyttää [NewtonSoft JSON.NET](http://www.newtonsoft.com/json/help/html/Introduction.htm) kirjaston onnistu ja poistaa mukautetun mallin objektit ja sieltä pois JSON. Voit mukauttaa tätä Sarjatoiminto tarvittaessa. Löydät lisätietoja [Upgrading to Azure haun .NET SDK version 1.1](search-dotnet-sdk-migration.md#WhatsNew). Esimerkki tämä on `[JsonProperty]` määrite `DescriptionFr` yllä sample code-ominaisuus.

Tietoja toisen tärkeintä `Hotel` luokan ovat tietotyyppien julkiset ominaisuudet. Näiden ominaisuuksien .NET-tietotyypit yhdistetään niiden vastaavat kenttätyyppien hakemiston määrityksessä. Esimerkiksi `Category` merkkijono ominaisuus vastaa `category` kenttä, jonka tyyppi on `DataType.String`. On samanlainen tyyppi yhdistämismääritykset `bool?` ja `DataType.Boolean`, `DateTimeOffset?` ja `DataType.DateTimeOffset`jne. Tietyn sääntöjä tyyppi yhdistämistä varten on kuvattu kanssa `Documents.Get` [MSDN](https://msdn.microsoft.com/library/azure/dn931291.aspx)-menetelmää.

Tämä mahdollisuutta käyttää omia luokkia tiedostojen toimii molempiin suuntiin; Voit hakea hakutulokset ja poistaa ne valintasi tyyppi automaattisesti SDK on [seuraavassa artikkelissa](search-query-dotnet.md)kuvatulla.

> [AZURE.NOTE] Azure haun .NET SDK tukee myös dynaamisesti kirjoitettu tiedostojen käyttäminen `Document` luokka, joka on kenttien nimet-kenttien arvojen avain/arvo määritystä. Tästä on hyötyä tilanteissa, jos et tiedä suunnittelun aikana indeksi-rakenne tai olisi tällainen sitoa tietyn mallin luokat. Kaikki menetelmät SDK: ssa, jotka liittyvät asiakirjat on Osastollasi, jotka toimivat `Document` luokan sekä erittäin kirjoitettu Osastollasi, jotka vievät yleisen tyypin. Vain jälkimmäisessä voi käyttää tämän artikkelin Esimerkki-koodi. Voit tarkistaa, lisää tietoja `Document` [MSDN-](https://msdn.microsoft.com/library/azure/microsoft.azure.search.models.document.aspx)luokka.

**Tietoja tietotyypeistä tärkeä huomautus**

Kun suunnittelet omia mallin luokkia Azure-hakuindeksin yhdistäminen, on suositeltavaa määritteleminen ominaisuudet, joiden arvo on esimerkiksi `bool` ja `int` voi olla nullable (esimerkiksi `bool?` sijaan `bool`). Jos käytät-nullable ominaisuutta, sinun on **taata** indeksi-asiakirjoja ei sisällä null-arvon vastaavan kentän. Eikä SDK Azure Search-palvelun avulla voit käyttää tätä.

Tämä ei ole juuri hypoteettista huolta: Kuvitellaan tilanne, jossa uuden kentän lisääminen aiemmin luotua indeksiä, jonka tyyppi on `DataType.Int32`. Kun olet päivittänyt indeksin määrityksen, kaikki asiakirjat on uuden kentän null-arvon (koska kaikenlaisten on nullable Azure hakutoiminnossa). Jos käytät mallin luokan sitten kanssa ei-tyypin nullable `int` kentän ominaisuuden, näkyviin tulee `JsonSerializationException` tältä, kun yritetään noutaa tiedostoja:

    Error converting value {null} to type 'System.Int32'. Path 'IntValue'.

Tästä syystä suosittelemme, että käytät mallin luokkien lajit parhaana käytäntönä.

## <a name="next"></a>Seuraava
Kun täyttää Azure-hakuindeksin ovat valmiina aloittamaan varmenteiden kyselyitä, voit etsiä tiedostoja. [Kyselyn Your Azure-hakuindeksin](search-query-overview.md) lisätietoja.
