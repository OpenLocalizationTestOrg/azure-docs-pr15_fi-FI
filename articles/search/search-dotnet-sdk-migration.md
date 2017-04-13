<properties
   pageTitle="Azure haun .NET SDK version 1.1-päivityksen | Microsoft Azure | Isännöityjen pilvipalvelussa haku"
   description="Azure haun .NET SDK-version 1.1 päivittäminen"
   services="search"
   documentationCenter=""
   authors="brjohnstmsft"
   manager="pablocas"
   editor=""/>

<tags
   ms.service="search"
   ms.devlang="dotnet"
   ms.workload="search"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.date="08/15/2016"
   ms.author="brjohnst"/>

# <a name="upgrading-to-the-azure-search-net-sdk-version-20-preview"></a>Päivityksen Azure haun .NET SDK versiota 2.0-esikatselu

Jos käytät version 1.1 tai vanhempi, [Azure haun .NET SDK](https://msdn.microsoft.com/library/azure/dn951165.aspx), tämän artikkelin avulla voit päivittää sovelluksen käyttämään seuraava pääversio, 2.0-esikatselu.

Tutustu yleisiä vaiheista esimerkkejä SDK [Azure hakutoiminnolla .NET-sovelluksesta](search-howto-dotnet-sdk.md).

Versiota 2.0-esikatselun Azure haun .NET SDK sisältää joitakin muutoksia aiemmista versioista. Nämä ovat useimmiten pieniä, joten koodia tarvitsevat vain vähän työmäärään. Katso [vaiheet päivittäminen](#UpgradeSteps) ohjeita siitä, miten voit muuttaa koodin käyttämään uutta SDK-versiota.

> [AZURE.NOTE] Jos käytät 0,13 esikatselu tai vanhempi versio, sinun kannattaa päivittää version 1.1 etunimen ja valitse Päivitä 2.0-esikatselu. Katso [Lisäys: vaiheet päivittämiseksi version 1.1](#UpgradeStepsV1) ohjeita.

<a name="WhatsNew"></a>
## <a name="whats-new-in-version-20-preview"></a>Mitä uusia ominaisuuksia versiossa 2.0-esikatselu

Versiota 2.0-esikatselu on Azure haun .NET SDK kohdentamisen Azure haun REST API, erityisesti 2015-02-28-esikatselu kokeiluversioon ensimmäinen versio. Näin monta esikatselu-ominaisuuksia Azure haun .NET-sovelluksesta, mukaan lukien seuraavat:

- [Mukautettu analyzers](https://aka.ms/customanalyzers)
- [Azure-Blob-säiliö](search-howto-indexing-azure-blob-storage.md) ja [Azure-taulukkotallennus](search-howto-indexing-azure-tables.md) indeksointitoiminto tuki
- Indeksointitoiminto mukauttaminen [kentän yhdistämismäärityksiä](search-indexer-field-mappings.md) kautta
- ETags tuki käyttöön turvallisten samanaikainen päivittäminen indeksin määritelmien ja Indeksoijilla tietolähteet
- .NET Core ja .NET kannettavat profiilin 111 tuki

<a name="UpgradeSteps"></a>
## <a name="steps-to-upgrade"></a>Ohjeita päivittäminen

Päivitä ensin, että NuGet viittaus `Microsoft.Azure.Search` käyttämällä joko NuGet pakettien hallinta-konsolin tai mukaan projektiviitteet hiiren kakkospainikkeella ja valitsemalla "Hallinta NuGet pakettien..." Visual Studiossa. Varmista, että otat ennakkoversion pakettien valitsemalla "Sisällytä Prerelease" NuGet "pakettien hallinta-ikkunassa Visual Studiossa tai käyttämällä `-IncludePrerelease` vaihtaminen, jos käytät NuGet paketin hallinta-konsolin.

Kun NuGet on ladattu uudet paketit ja niiden riippuvuudet, muodosta uudelleen projektin. Sen mukaan, koodisi rakenteesta se voi muodostaa onnistuneesti. Jos näin on, olet valmis!

Jos yhteyttä muodosta epäonnistuu, pitäisi näkyä muodosta-virhe, kuten jompikumpi seuraavista:

    Program.cs(31,45,31,86): error CS0266: Cannot implicitly convert type 'Microsoft.Azure.Search.ISearchIndexClient' to 'Microsoft.Azure.Search.SearchIndexClient'. An explicit conversion exists (are you missing a cast?)

Seuraavaksi voit korjata virheen muodosta. Lisätietoja [tärkeimmät muutokset versiota 2.0 - esikatselussa](#ListOfChanges) mikä aiheuttaa virheen ja sen korjaamisesta.

Voit nähdä muut muodosta varoitukset vanhentuneen menetelmät ja ominaisuudet. Varoitukset sisällytetään mitä voit käyttää sen sijaan, että poistetuista toiminnon ohjeita. Jos sovellus käyttää esimerkiksi `SearchRequestOptions.RequestId` ominaisuutta, voit saada varoituksen, jonka mukaan`"This property is deprecated. Please use ClientRequestId instead."`

Kun olet korjannut muodosta virheitä, voit tehdä muutoksia, jotta voit hyödyntää uusia toimintoja, jos haluat, että sovelluksesi. SDK: N uudet ominaisuudet yksityiskohtaisesti [versiota 2.0-Preview'n uudet ominaisuudet](#WhatsNew).

<a name="ListOfChanges"></a>
## <a name="breaking-changes-in-version-20-preview"></a>Tärkeimmät muutokset versiota 2.0-esikatselu

On vain yksi jakautumisen muuttaminen versiota 2.0-esikatselussa, joka saattaa edellyttää muutokset lisäksi muodostetaan sovelluksesi uudelleen.

### <a name="indexesgetclient-return-type"></a>Indexes.GetClient palautustyyppi

`Indexes.GetClient` Menetelmässä on uusi palautustyyppi. Aikaisemmin, se palauttaa `SearchIndexClient`, mutta tämä on vaihdettu `ISearchIndexClient` versiota 2.0-esikatselussa. Tämä on asiakkaiden, jotka haluavat mock tukemiseksi `GetClient` menetelmä yksikön testeissä palauttamalla mock soveltaminen `ISearchIndexClient`.

#### <a name="example"></a>Esimerkki

Jos koodi on seuraavanlainen:

```csharp
SearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");
```

Voit muuttaa sen tätä korjaa muodosta virheitä:

```csharp
ISearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");
```

## <a name="conclusion"></a>Tekemistä
Lisätietoja Azure haun .NET SDK käyttämisestä on artikkelissa Microsoftin viimeksi päivitetty [käyttövinkkejä](search-howto-dotnet-sdk.md).

Tervetulleita palautetta SDK:. Jos käytössä ilmenee ongelmia, vapaasti us kysy apua käyttämiseen [Azure Etsi MSDN-keskustelupalsta](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=azuresearch). Jos löydät ohjelmavirhe, voit jättää ongelma [Azure .NET SDK GitHub säilöön](https://github.com/Azure/azure-sdk-for-net/issues). Varmista, että ongelma-otsikko ja etuliite "haun SDK:".

Kiitos, että Azure-haun avulla.

<a name="UpgradeStepsV1"></a>
## <a name="appendix-steps-to-upgrade-to-version-11"></a>Lisäys: Päivitä version 1.1 vaiheet 

> [AZURE.NOTE] Tässä osassa koskee vain Azure haun .NET SDK-version 0,13 esikatselu tai sitä vanhemman version käyttäjille.

Päivitä ensin, että NuGet viittaus `Microsoft.Azure.Search` käyttämällä joko NuGet pakettien hallinta-konsolin tai mukaan projektiviitteet hiiren kakkospainikkeella ja valitsemalla "Hallinta NuGet pakettien..." Visual Studiossa.

Kun NuGet on ladattu uudet paketit ja niiden riippuvuudet, muodosta uudelleen projektin.

Jos käytössäsi on aiemmin versio 1.0.0-preview, 1.0.1-preview tai 1.0.2-preview, Luo pitäisi toimia ja olet valmis!

Jos käytit aiemmin versio 0.13.0-preview tai vanhempi, näkyviin tulee luominen virheitä seuraavalta:

    Program.cs(137,56,137,62): error CS0117: 'Microsoft.Azure.Search.Models.IndexBatch' does not contain a definition for 'Create'
    Program.cs(137,99,137,105): error CS0117: 'Microsoft.Azure.Search.Models.IndexAction' does not contain a definition for 'Create'
    Program.cs(146,41,146,54): error CS1061: 'Microsoft.Azure.Search.IndexBatchException' does not contain a definition for 'IndexResponse' and no extension method 'IndexResponse' accepting a first argument of type 'Microsoft.Azure.Search.IndexBatchException' could be found (are you missing a using directive or an assembly reference?)
    Program.cs(163,13,163,42): error CS0246: The type or namespace name 'DocumentSearchResponse' could not be found (are you missing a using directive or an assembly reference?)

Seuraavaksi muodosta virheiden korjaamiseksi yksi kerrallaan. Useimmat edellyttävät muuttaminen joitakin luokan ja menetelmä nimiä, jotka on nimetty uudelleen SDK: ssa. [Luettelon, kun version 1.1 muutokset](#ListOfChangesV1) sisältää luettelon nimen muutoksia.

Jos käytät mukautettuja luokkia malliin tiedostojen ja näiden luokkien on-nullable primitiivityyppejä ominaisuuksia (esimerkiksi `int` tai `bool` C#), on ohjelmavirhe korjaa SDK-paketissa, jotka sinun olisi otettava huomioon 1.1-versiossa. Saat lisätietoja [virheenkorjauksia 1.1-versiossa](#BugFixesV1) .

Lopuksi, kun olet korjannut muodosta virheitä, voit tehdä muutoksia, jotta voit hyödyntää uusia toimintoja, jos haluat, että sovelluksesi.

<a name="ListOfChangesV1"></a>
### <a name="list-of-breaking-changes-in-version-11"></a>Luettelon, kun muutoksia version 1.1

Seuraavassa luettelossa on tilattu mukaan todennäköisyys, että muutos vaikuttaa sovelluksen-koodin.

#### <a name="indexbatch-and-indexaction-changes"></a>IndexBatch ja IndexAction muutokset

`IndexBatch.Create`on nimetty `IndexBatch.New` ja ei ole enää `params` argumentti. Voit käyttää `IndexBatch.New` erissä, sekoitetaan erityyppiset toiminnot (yhdistää, poistaa, jne.). Lisäksi tavoilla uusi staattinen luomisen erissä missä kaikki toiminnot ovat samat: `Delete`, `Merge`, `MergeOrUpload`, ja `Upload`.

`IndexAction`ei enää ole julkisten konstruktoreja ja sen ominaisuuksia ei voi nyt. Käytettävä uusi staattiset menetelmät luomisen toiminnot eri tarkoituksiin: `Delete`, `Merge`, `MergeOrUpload`, ja `Upload`. `IndexAction.Create`on poistettu. Jos olet käyttänyt liikaa, joka kestää vain asiakirjan, varmista, että Käytä `Upload` sijaan.

##### <a name="example"></a>Esimerkki

Jos koodi on seuraavanlainen:

    var batch = IndexBatch.Create(documents.Select(doc => IndexAction.Create(doc)));
    indexClient.Documents.Index(batch);

Voit muuttaa sen tätä korjaa muodosta virheitä:

    var batch = IndexBatch.New(documents.Select(doc => IndexAction.Upload(doc)));
    indexClient.Documents.Index(batch);

Jos haluat, voit yksinkertaistaa sitä edelleen tähän:

    var batch = IndexBatch.Upload(documents);
    indexClient.Documents.Index(batch);

#### <a name="indexbatchexception-changes"></a>IndexBatchException muutokset

`IndexBatchException.IndexResponse` Ominaisuus on nimetty `IndexingResults`, ja sen tyyppi on nyt `IList<IndexingResult>`.

##### <a name="example"></a>Esimerkki

Jos koodi on seuraavanlainen:

    catch (IndexBatchException e)
    {
        Console.WriteLine(
            "Failed to index some of the documents: {0}",
            String.Join(", ", e.IndexResponse.Results.Where(r => !r.Succeeded).Select(r => r.Key)));
    }

Voit muuttaa sen tätä korjaa muodosta virheitä:

    catch (IndexBatchException e)
    {
        Console.WriteLine(
            "Failed to index some of the documents: {0}",
            String.Join(", ", e.IndexingResults.Where(r => !r.Succeeded).Select(r => r.Key)));
    }

<a name="OperationMethodChanges"></a>
#### <a name="operation-method-changes"></a>Toimintojen menetelmä muutokset

Menetelmä Osastollasi joukkona näkyvät kunkin toiminnon Azure haun .NET SDK synkronoidusti ja asynkroninen tulevien. Allekirjoitukset ja käyttötilejä, näiden menetelmä Osastollasi on muutettu version 1.1.

Esimerkiksi "Hae tilastot"-toimintoa SDK aiemmissa versioissa tarjoamia näiden allekirjoitusten:

In `IIndexOperations`:

    // Asynchronous operation with all parameters
    Task<IndexGetStatisticsResponse> GetStatisticsAsync(
        string indexName,
        CancellationToken cancellationToken);

In `IndexOperationsExtensions`:

    // Asynchronous operation with only required parameters
    public static Task<IndexGetStatisticsResponse> GetStatisticsAsync(
        this IIndexOperations operations,
        string indexName);

    // Synchronous operation with only required parameters
    public static IndexGetStatisticsResponse GetStatistics(
        this IIndexOperations operations,
        string indexName);

Menetelmäallekirjoitukset samaan toimintoon version 1.1 näyttää tältä:

In `IIndexesOperations`:

    // Asynchronous operation with lower-level HTTP features exposed
    Task<AzureOperationResponse<IndexGetStatisticsResult>> GetStatisticsWithHttpMessagesAsync(
        string indexName,
        SearchRequestOptions searchRequestOptions = default(SearchRequestOptions),
        Dictionary<string, List<string>> customHeaders = null,
        CancellationToken cancellationToken = default(CancellationToken));

In `IndexesOperationsExtensions`:

    // Simplified asynchronous operation
    public static Task<IndexGetStatisticsResult> GetStatisticsAsync(
        this IIndexesOperations operations,
        string indexName,
        SearchRequestOptions searchRequestOptions = default(SearchRequestOptions),
        CancellationToken cancellationToken = default(CancellationToken));

    // Simplified synchronous operation
    public static IndexGetStatisticsResult GetStatistics(
        this IIndexesOperations operations,
        string indexName,
        SearchRequestOptions searchRequestOptions = default(SearchRequestOptions));

Version 1.1 alkaen Azure haun .NET SDK järjestää toiminnon menetelmiä eri tavalla:

 - Valinnaisten parametrien nyt mallintaa oletukseksi parametrit mieluummin kuin muut menetelmä Osastollasi. Tämä pienentää menetelmä Osastollasi määrän joskus huomattavasti.
 - Tunniste-menetelmiä piilottaa nyt ylimääräiset tiedot HTTP soittajan paljon. Esimerkiksi SDK vanhempien versioiden palautti vastauksen, HTTP tilakoodi, jotka usein ei tarvitse tarkistaa, koska toiminnon menetelmiä palauttaa objektin `CloudException` , minkä tahansa tilakoodi, joka ilmaisee virheen. Uusi tunniste menetelmiä palauttaa vain mallin objekteista, jolloin sinun ei tarvitse poistaa rivityksen koodisi.
 - Jos taas tärkeä liittymät nyt paljastus tapoja, joiden avulla voit määrittää tarkemmin HTTP tasolla, jos tarvitse sitä. Voit nyt välittää sisällytetään pyynnöt ja uuden mukautetun HTTP-otsikoiden `AzureOperationResponse<T>` palautustyyppi tutustutaan suora pääsy `HttpRequestMessage` ja `HttpResponseMessage` toiminnon. `AzureOperationResponse`on määritetty `Microsoft.Rest.Azure` nimitilan ja korvaa `Hyak.Common.OperationResponse`.

#### <a name="scoringparameters-changes"></a>ScoringParameters muutokset

Uuden luokan nimi `ScoringParameter` on lisätty uusimmat SDK: ssa voit antaa näkyvissä profiilit pistemäärä haun kyselyn parametrit on helpompaa. Aiemmin `ScoringProfiles` -ominaisuuden `SearchParameters` luokka on kirjoitettu kuin `IList<string>`; Nyt on kirjoitettu kuin `IList<ScoringParameter>`.

##### <a name="example"></a>Esimerkki

Jos koodi on seuraavanlainen:

    var sp = new SearchParameters();
    sp.ScoringProfile = "jobsScoringFeatured";      // Use a scoring profile
    sp.ScoringParameters = new[] { "featuredParam-featured", "mapCenterParam-" + lon + "," + lat };

Voit muuttaa sen tätä korjaa muodosta virheitä: 

    var sp = new SearchParameters();
    sp.ScoringProfile = "jobsScoringFeatured";      // Use a scoring profile
    sp.ScoringParameters =
        new[]
        {
            new ScoringParameter("featuredParam", new[] { "featured" }),
            new ScoringParameter("mapCenterParam", GeographyPoint.Create(lat, lon))
        };

#### <a name="model-class-changes"></a>Mallin luokan muutokset

Allekirjoituksen vuoksi muuttuu kuvattu [toiminnon menetelmä muuttuu](#OperationMethodChanges), useita luokkia `Microsoft.Azure.Search.Models` nimitilan on nimetty uudelleen tai poistettu. Esimerkki:

 - `IndexDefinitionResponse`on korvattu`AzureOperationResponse<Index>`
 - `DocumentSearchResponse`on nimetty`DocumentSearchResult`
 - `IndexResult`on nimetty`IndexingResult`
 - `Documents.Count()`palauttaa nyt `long` ja sen sijaan, että tiedostojen-määrän`DocumentCountResponse`
 - `IndexGetStatisticsResponse`on nimetty`IndexGetStatisticsResult`
 - `IndexListResponse`on nimetty`IndexListResult`

Yhteenveto `OperationResponse`-johdetun luokat, jotka olivat vain, jos haluat rivittää malli-objekti on poistettu. Jäljellä olevat luokat on ollut niiden jälkiliite muuttui `Response` , `Result`.

##### <a name="example"></a>Esimerkki

Jos koodi on seuraavanlainen:

    IndexerGetStatusResponse statusResponse = null;

    try
    {
        statusResponse = _searchClient.Indexers.GetStatus(indexer.Name);
    }
    catch (Exception ex)
    {
        Console.WriteLine("Error polling for indexer status: {0}", ex.Message);
        return;
    }

    IndexerExecutionResult lastResult = statusResponse.ExecutionInfo.LastResult;

Voit muuttaa sen tätä korjaa muodosta virheitä:

    IndexerExecutionInfo status = null;

    try
    {
        status = _searchClient.Indexers.GetStatus(indexer.Name);
    }
    catch (Exception ex)
    {
        Console.WriteLine("Error polling for indexer status: {0}", ex.Message);
        return;
    }

    IndexerExecutionResult lastResult = status.LastResult;

##### <a name="response-classes-and-ienumerable"></a>Vastauksen luokat ja IEnumerable

Muut muutokset, jotka voivat vaikuttaa koodi on vastaus luokat, jotka pidä sivustokokoelmat enää toteuttaa `IEnumerable<T>`. Sen sijaan voit käyttää sivustokokoelman ominaisuuden suoraan. Esimerkiksi jos koodi on seuraavanlainen:

    DocumentSearchResponse<Hotel> response = indexClient.Documents.Search<Hotel>(searchText, sp);
    foreach (SearchResult<Hotel> result in response)
    {
        Console.WriteLine(result.Document);
    }

Voit muuttaa sen tätä korjaa muodosta virheitä:

    DocumentSearchResult<Hotel> response = indexClient.Documents.Search<Hotel>(searchText, sp);
    foreach (SearchResult<Hotel> result in response.Results)
    {
        Console.WriteLine(result.Document);
    }

##### <a name="important-note-for-web-applications"></a>Tärkeä huomautus verkkosovellusten

Jos sinulla on web-sovelluksen serializes `DocumentSearchResponse` suoraan lähettäminen hakutuloksissa selaimessa, sinun on muutettava koodi tai tulokset ei onnistu oikein. Esimerkiksi jos koodi on seuraavanlainen:

    public ActionResult Search(string q = "")
    {
        // If blank search, assume they want to search everything
        if (string.IsNullOrWhiteSpace(q))
            q = "*";

        return new JsonResult
        {
            JsonRequestBehavior = JsonRequestBehavior.AllowGet,
            Data = _featuresSearch.Search(q)
        };
    }

Voit muuttaa sen käytön `.Results` haun vastauksen korjata haun tulokset värien-ominaisuus:

    public ActionResult Search(string q = "")
    {
        // If blank search, assume they want to search everything
        if (string.IsNullOrWhiteSpace(q))
            q = "*";

        return new JsonResult
        {
            JsonRequestBehavior = JsonRequestBehavior.AllowGet,
            Data = _featuresSearch.Search(q).Results
        };
    }

Sinun on tarkistettava tällaisissa tapauksissa koodissa itse. **Kääntäjä ei varoittaa,** koska `JsonResult.Data` tyyppi on `object`.

#### <a name="cloudexception-changes"></a>CloudException muutokset

`CloudException` Luokka on siirretty `Hyak.Common` nimitilan `Microsoft.Rest.Azure` nimitilan. Myös sen `Error` ominaisuus on nimetty `Body`.

#### <a name="searchserviceclient-and-searchindexclient-changes"></a>SearchServiceClient ja SearchIndexClient muutokset

Tyyppi `Credentials` ominaisuus on muuttunut `SearchCredentials` sen perus-luokan `ServiceClientCredentials`. Jos haluat käyttää `SearchCredentials` , `SearchIndexClient` tai `SearchServiceClient`, käytä uuden `SearchCredentials` ominaisuus.

Aiemmissa versioissa SDK-paketissa `SearchServiceClient` ja `SearchIndexClient` konstruktoreja, joita noudatit oli `HttpClient` parametri. Nämä on korvattu konstruktoreja, jotka `HttpClientHandler` ja matriisin `DelegatingHandler` objekteja. Tämä on helppo asentaa mukautetun tiedostokäsittelijöitä esikäsittely pyyntöjen tarvittaessa.

Lopuksi, joita noudatit konstruktoreja `Uri` ja `SearchCredentials` ovat muuttuneet. Esimerkiksi jos koodi, joka näyttää tältä:

    var client =
        new SearchServiceClient(
            new SearchCredentials("abc123"),
            new Uri("http://myservice.search.windows.net"));

Voit muuttaa sen tätä korjaa muodosta virheitä:

    var client =
        new SearchServiceClient(
            new Uri("http://myservice.search.windows.net"),
            new SearchCredentials("abc123"));

Huomaa myös, että tunnistetiedot-parametrin tyyppi on muuttunut `ServiceClientCredentials`. Tämä on todennäköisesti vaikuta koodisi jälkeen `SearchCredentials` johdetaan `ServiceClientCredentials`.

#### <a name="passing-a-request-id"></a>Kulkeva pyynnön tunnus

SDK vanhempi versio, voit määrittää pyynnön tunnus `SearchServiceClient` tai `SearchIndexClient` , ja se lisätään jokaisen REST API-pyynnössä. Tästä on hyötyä search-palvelun ongelmien vianmääritykseen, jos tarvitset ottaa yhteyden tukeen. On kuitenkin hyötyä, voit määrittää yksilölliset pyyntöä, jonka tunnus jokaisen-toiminnon sijaan voit käyttää samaa tunnusta kaikki toiminnot. Tästä syystä `SetClientRequestId` maksutapojen `SearchServiceClient` ja `SearchIndexClient` on poistettu. Sen sijaan voit välittää pyynnön tunnus toiminnon kummassakin menetelmässä on valinnainen kautta `SearchRequestOptions` parametria.

> [AZURE.NOTE] SDK: n tulevissa versioissa on Lisää uusi järjestelmä määrittämisestä pyynnön tunnus yleisesti asiakkaan objekteja, joka vastaa muiden Azure SDK: T käyttämä lähestymistapa.

#### <a name="example"></a>Esimerkki

Jos sinulla on koodi, joka näyttää tältä:

    client.SetClientRequestId(Guid.NewGuid());
    ...
    long count = client.Documents.Count();

Voit muuttaa sen tätä korjaa muodosta virheitä:

    long count = client.Documents.Count(new SearchRequestOptions(requestId: Guid.NewGuid()));

#### <a name="interface-name-changes"></a>Käyttöliittymän nimenmuutoksia

Toiminnon ryhmien käyttöliittymän nimet kaikki uutta yhtenäistämistä vastaavan ominaisuuden nimillä:

 - Tyypin `ISearchServiceClient.Indexes` on nimetty uudelleen `IIndexOperations` , `IIndexesOperations`.
 - Tyypin `ISearchServiceClient.Indexers` on nimetty uudelleen `IIndexerOperations` , `IIndexersOperations`.
 - Tyypin `ISearchServiceClient.DataSources` on nimetty uudelleen `IDataSourceOperations` , `IDataSourcesOperations`.
 - Tyypin `ISearchIndexClient.Documents` on nimetty uudelleen `IDocumentOperations` , `IDocumentsOperations`.

Tämä muutos on todennäköisesti vaikuta koodi, paitsi jos olet luonut mocks testaus näistä liittymistä.

<a name="BugFixesV1"></a>
### <a name="bug-fixes-in-version-11"></a>Version 1.1 virheenkorjauksia

Tapahtui ohjelmavirhe liittyvistä Sarjatoiminto mukautetun mallin luokkien Azure haun .NET SDK aiemmissa versioissa. Virhe voi ilmetä, jos olet luonut mukautetun mallin luokan ominaisuus-nullable arvon tyyppi.

#### <a name="steps-to-reproduce"></a>Toistaminen

Luo mukautettu malli luokka-nullable arvo-tyypin ominaisuus. Lisää esimerkiksi julkisen `UnitCount` tyypin `int` sijaan `int?`.

Jos indeksoit tiedoston tyyppiä oletusarvo (esimerkiksi 0 `int`)-kentän arvo on nolla, Azure hakutoiminnolla. Jos etsit myöhemmin, että asiakirja `Search` kutsu palauttaa `JsonSerializationException` complaining, sitä ei voi muuntaa `null` , `int`.

Lisäksi suodattimet eivät ehkä toimi odotetulla tavalla, koska sen sijaan, että tarkoitettu arvo indeksi on kirjoitettu null.

#### <a name="fix-details"></a>Korjaa tiedot

Microsoft on korjattu ongelman SDK 1.1-version. Nyt, jos sinulla on mallin luokan tältä:

    public class Model
    {
        public string Key { get; set; }

        public int IntValue { get; set; }
    }

Voit määrittää ja `IntValue` 0, arvo on nyt oikein muuntaa sarjaksi 0-koneisiin ja tallennettu indeksin 0. Pyöreä kompastuminen toimii myös odotetulla tavalla.

Yksi mahdollinen ongelma pitää mielessä, tämän menetelmän: Jos käytät mallityyppi-nullable ominaisuus, sinun on **taata** indeksi ei asiakirjoja sisältävät null-arvon vastaavaan kenttään. Eikä SDK Azure haun REST-Ohjelmointirajapinta avulla voit käyttää tätä.

Tämä ei ole juuri hypoteettista huolta: Kuvitellaan tilanne, jossa uuden kentän lisääminen aiemmin luotua indeksiä, jonka tyyppi on `Edm.Int32`. Kun olet päivittänyt indeksin määrityksen, kaikki asiakirjat on uuden kentän null-arvon (koska kaikenlaisten on nullable Azure hakutoiminnossa). Jos käytät mallin luokan sitten kanssa ei-tyypin nullable `int` kentän ominaisuuden, näkyviin tulee `JsonSerializationException` tältä, kun yritetään noutaa tiedostoja:

    Error converting value {null} to type 'System.Int32'. Path 'IntValue'.

Tästä syystä Suosittelemme kuitenkin, että käytät mallin luokkien lajit parhaana käytäntönä.

Saat lisätietoja Tämä ohjelmavirhe ja korjaa [tämän ongelman GitHub](https://github.com/Azure/azure-sdk-for-net/issues/1063).
