<properties
    pageTitle="Käyttämällä .NET SDK Azure haun indeksin luominen | Microsoft Azure | Isännöityjen pilvipalvelussa haku"
    description="Indeksin luominen koodissa käyttämällä Azure haun .NET SDK-paketissa."
    services="search"
    documentationCenter=""
    authors="brjohnstmsft"
    manager="jhubbard"
    editor=""
    tags="azure-portal"/>

<tags
    ms.service="search"
    ms.devlang="dotnet"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="brjohnst"/>

# <a name="create-an-azure-search-index-using-the-net-sdk"></a>Käyttämällä .NET SDK Azure haun indeksin luominen
> [AZURE.SELECTOR]
- [Yleiskatsaus](search-what-is-an-index.md)
- [Portal](search-create-index-portal.md)
- [.NET](search-create-index-dotnet.md)
- [MUILLE KÄYTTÄJILLE](search-create-index-rest-api.md)


Tässä artikkelissa opastaa luominen käyttämällä [Azure haun .NET SDK](https://msdn.microsoft.com/library/azure/dn951165.aspx)Azure haun [indeksi](https://msdn.microsoft.com/library/azure/dn798941.aspx) .

Ennen tämän oppaan seuraavan ja indeksin, sinulla on oltava jo [luotu Azure-hakupalvelun](search-create-service-portal.md).

Huomaa, että kaikki tämän artikkelin Esimerkki koodi kirjoitetaan C#. Voit etsiä koko tietolähteen [GitHub](http://aka.ms/search-dotnet-howto)-koodiin.

## <a name="i-identify-your-azure-search-services-admin-api-key"></a>Voin. Määritä Azure Search-palvelun järjestelmänvalvoja api-näppäin
Nyt kun olet valmisteltu Azure-hakupalvelun, olet melkein valmis käyttämällä .NET SDK palvelupäätepiste pyynnöt antamaan. Sinun on ensin jokin järjestelmänvalvojan api-näppäimiä, joka on luotu hakupalvelun voit valmistelun yhteydessä. .NET SDK Lähetä tätä api-näppäintä jokaisen pyynnön palvelussa. Salli kohti pyynnön välein, lähettää pyynnön ja -palvelu, joka käsittelee sen sovelluksen välillä on kelvollinen avain vahvistetaan.

1. Etsi oman service api-näppäimiä sinun on kirjauduttava [Azure-portaalissa](https://portal.azure.com/)
2. Siirry Azure Search-palvelun sivu
3. Valitse "Näppäimet"-kuvake

Palvelussa on *järjestelmänvalvojan näppäimet* ja *kysely-näppäimiä*.

  - Ensisijaisen ja toissijaisen *järjestelmänvalvojan näppäimet* myöntää kaikki toiminnot, mukaan lukien pätevyys hallita palvelua, luominen ja poistaminen indeksit Indeksoijilla ja tietolähteiden täydet oikeudet. On kaksi näppäimet, jotta voit edelleen käyttää toissijaisen avaimen, jos uudelleen perusavain ja päinvastoin.
  - *Kyselyn näppäimet* myöntää indeksit ja tiedostojen käyttöoikeus vain luku- ja jaetaan yleensä asiakassovelluksiin, joka antaa search-pyyntöjen.

Hakemiston luominen yhdistämiskyselyssä, voit käyttää joko ensisijaisen tai toissijaisen järjestelmänvalvojan-näppäintä.

<a name="CreateSearchServiceClient"></a>
## <a name="ii-create-an-instance-of-the-searchserviceclient-class"></a>II. SearchServiceClient-luokan luominen
Aloita käyttäminen Azure haun .NET SDK-paketissa, sinun on luoda esiintymää `SearchServiceClient` luokka. Tämä luokka on useita konstruktoreja. Haluamasi kestää search-palvelunimi ja `SearchCredentials` parametreiksi objekti. `SearchCredentials`rivittää api-näppäintä.

Seuraava koodi luo uuden `SearchServiceClient` käyttämällä arvoja search-palvelunimi ja api-näppäintä, jotka on tallennettu sovelluksen määritystiedosto (`app.config` tai `web.config`):

```csharp
string searchServiceName = ConfigurationManager.AppSettings["SearchServiceName"];
string adminApiKey = ConfigurationManager.AppSettings["SearchServiceAdminApiKey"];

SearchServiceClient serviceClient = new SearchServiceClient(searchServiceName, new SearchCredentials(adminApiKey));
```

`SearchServiceClient`on `Indexes` ominaisuus. Tämä ominaisuus on kaikki menetelmät, sinun täytyy luoda luettelon, Päivitä tai poista Azure-hakuindeksin.

> [AZURE.NOTE] `SearchServiceClient` Luokka hallitsee search-palvelun yhteydet. Avaamatta liikaa yhteyksiä olisi yrität jakaa yksittäisen esiintymän `SearchServiceClient` sovelluksen Jos mahdollista. Sen menetelmiä ovat viestiketjun sopivaa tällaisten jakamisen ottaminen käyttöön.

<a name="DefineIndex"></a>
## <a name="iii-define-your-azure-search-index-using-the-index-class"></a>III. Määritä Azure haku-hakemisto-avulla `Index` luokan
Yksittäisen kutsu `Indexes.Create` menetelmä luo indeksi. Tämä menetelmä kestää parametrina `Index` objekti, joka määrittää Azure-hakuindeksin. Sinun on luotava `Index` objekti ja alusta seuraavasti:

1. Määrittää `Name` -ominaisuuden `Index` objektin indeksi nimeä.
2. Määritä `Fields` -ominaisuuden `Index` matriisin objektin `Field` objekteja. Kunkin `Field` objektien määrittää kentän toimintaa indeksi. Voit kirjoittaa konstruktorin tietotyyppi (tai analyzer merkkijonon kenttien) sekä kentän nimi. Voit myös määrittää muita ominaisuuksia, kuten `IsSearchable`, `IsFilterable`jne.

On tärkeää, että pidät Etsi käyttäjä kokemus ja business tarpeitasi mielessä suunniteltaessa indeksi kuin kunkin `Field` on määritettävä [haluamasi ominaisuudet](https://msdn.microsoft.com/library/azure/dn798941.aspx). Kentät, jotka koskevat nämä ominaisuudet määrittää, mitkä Etsi ominaisuuksia (suodattamista, faceting, lajittelua tekstimuotoisen etsimisen, jne.). Minkä tahansa ominaisuudelle ei ole määritetty `Field` luokan oletusarvo on vastaava hakutoiminnon käytöstä, ellet nimenomaisesti käyttöön.

Tässä esimerkissä on olet nimeltä indeksi "Hotellit" ja Microsoftin kentät määritellään seuraavasti:

```csharp
var definition = new Index()
{
    Name = "hotels",
    Fields = new[]
    {
        new Field("hotelId", DataType.String)                       { IsKey = true, IsFilterable = true },
        new Field("baseRate", DataType.Double)                      { IsFilterable = true, IsSortable = true, IsFacetable = true },
        new Field("description", DataType.String)                   { IsSearchable = true },
        new Field("description_fr", AnalyzerName.FrLucene),
        new Field("hotelName", DataType.String)                     { IsSearchable = true, IsFilterable = true, IsSortable = true },
        new Field("category", DataType.String)                      { IsSearchable = true, IsFilterable = true, IsSortable = true, IsFacetable = true },
        new Field("tags", DataType.Collection(DataType.String))     { IsSearchable = true, IsFilterable = true, IsFacetable = true },
        new Field("parkingIncluded", DataType.Boolean)              { IsFilterable = true, IsFacetable = true },
        new Field("smokingAllowed", DataType.Boolean)               { IsFilterable = true, IsFacetable = true },
        new Field("lastRenovationDate", DataType.DateTimeOffset)    { IsFilterable = true, IsSortable = true, IsFacetable = true },
        new Field("rating", DataType.Int32)                         { IsFilterable = true, IsSortable = true, IsFacetable = true },
        new Field("location", DataType.GeographyPoint)              { IsFilterable = true, IsSortable = true }
    }
};
```

Microsoft on valinnut huolellisesti ominaisuuden arvot kullekin `Field` kuinka on mielestäsi niitä käytetään sovelluksen perusteella. Esimerkiksi on todennäköistä, että hotellit hakeminen henkilöt kiinnostunut avainsana hakutuloksia, valitse `description` -kentässä, jotta olemme haku on otettu käyttöön teksti-kenttään määrittämällä `IsSearchable` , `true`.

Huomautus täsmälleen yhden indeksi-tyypin kentässä `DataType.String` on oltava _avain_ -kentässä määritettyyn määrittämällä `IsKey` , `true` (katso `hotelId` edellä olevassa esimerkissä).

Yllä indeksin määrityksen käyttää mukautettuja kieli-analyzer varten `description_fr` kenttää, koska se on tarkoitus tallentaa Ranskan tekstiä. Katso [tukevat aiheessa MSDN-sivuston kieli](https://msdn.microsoft.com/library/azure/dn879793.aspx) sekä vastaavan [blogimerkintä](https://azure.microsoft.com/blog/language-support-in-azure-search/) lisätietoja kielen analyzers.

> [AZURE.NOTE]  Huomaa, että välittämällä `AnalyzerName.FrLucene` konstruktorissa, `Field` tulee automaattisesti tyypin `DataType.String` ja on `IsSearchable` asettaminen `true`.

## <a name="iv-create-the-index"></a>IV. Hakemiston luominen
Nyt kun olet luonut valmisteltu `Index` objekti, voit luoda hakemiston yksinkertaisesti soittamalla `Indexes.Create` -että `SearchServiceClient` objekti:

```csharp
serviceClient.Indexes.Create(definition);
```

Onnistuneiden pyynnön menetelmä palauttaa normaalisti. Jos näkyvissä on virheellinen parametri kuten pyynnön ongelma, menetelmä palauttaa `CloudException`.

Kun enää tarvitse indeksin ja haluat poistaa sen, vain Soita `Indexes.Delete` menetelmän oman `SearchServiceClient`. Tämä on esimerkiksi kuinka on poistettava "Hotellit" indeksi:

```csharp
serviceClient.Indexes.Delete("hotels");
```

> [AZURE.NOTE] Tämän artikkelin esimerkkikoodi käyttää Azure haun .NET SDK synkronisen menetelmiä yksinkertaisuuden. On suositeltavaa, että käytät omia sovelluksia asynkronisten menetelmien pitämään ne skaalattava ja vastaa. Kuten edellä olevassa esimerkissä voit käyttää `CreateAsync` ja `DeleteAsync` sijaan `Create` ja `Delete`.

## <a name="next"></a>Seuraava
Kun olet luonut Azure-hakuindeksin, voi haluat [ladata sisältöä hakemiston kyselyjä](search-what-is-data-import.md) , voit ryhtyä tietojen etsimistä.
