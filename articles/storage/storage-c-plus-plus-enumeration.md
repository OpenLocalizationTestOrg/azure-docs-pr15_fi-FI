<properties
    pageTitle="Azure-tallennustilan resurssien Microsoft Azure-tallennustilan asiakas-kirjaston kanssa on lueteltu C++ | Microsoft Azure"
    description="Opettele käyttämään luettelon API C++ Microsoft Azure tallennustilan asiakkaan kirjaston luettelointi säilöjen BLOB-objektit, olevien taulukoita ja kohteita."
    documentationCenter=".net"
    services="storage"
    authors="dineshmurthy"
    manager="jahogg"
    editor="tysonn"/>
<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="dineshm"/>

# <a name="list-azure-storage-resources-in-c"></a>Azure-tallennustilan resurssit ovat c++

Luettelon toiminnot ovat kehittäminen skenaarioita kanssa Azuren tallennustilaan-näppäintä. Tässä artikkelissa käsitellään Luetteloi mahdollisimman tehokkaasti objektien luettelon API C++ tarkoitettu Microsoft Azure tallennustilan asiakas-kirjastossa käyttämällä Azure-tallennustilan.

>[AZURE.NOTE] Tämän oppaan kohteena Azure tallennustilan asiakkaan kirjasto C++ versio 2.x, joka on käytettävissä [NuGet](http://www.nuget.org/packages/wastorage) tai [GitHub](https://github.com/Azure/azure-storage-cpp)kautta.

Tallennustilan asiakkaan kirjasto sisältää erilaisia menetelmiä luettelo tai kysely objekteihin Azuren tallennustilaan. Tässä artikkelissa käsitellään seuraavissa tilanteissa:

-   Luettelon säilöt-tilissä
-   Luettelon BLOB-säilö tai virtual blob-kansio
-   Luettelossa olevien tilin
-   Luettelon taulukoiden tilin
-   Taulukon kohteiden kysely

Näitä näkyy eri Osastollasi käyttämällä eri skenaarioissa.

## <a name="asynchronous-versus-synchronous"></a>Asynkroninen ja synkronoitu

Tallennustilan asiakkaan kirjasto C++ rakentuu [C++ REST-kirjastoa](https://github.com/Microsoft/cpprestsdk), sillä salliminen tuetaan asynkronisten toimintojen avulla [pplx::task](http://microsoft.github.io/cpprestsdk/classpplx_1_1task.html). Esimerkki:

    pplx::task<list_blob_item_segment> list_blobs_segmented_async(continuation_token& token) const;

Synkronoitujen toimintojen rivitys vastaavan asynkronisten toimintojen:

    list_blob_item_segment list_blobs_segmented(const continuation_token& token) const
    {
        return list_blobs_segmented_async(token).get();
    }

Jos käytät useita säikeistysmallin sovelluksia ja palveluja, on suositeltavaa, että käytät asynkroninen ohjelmointirajapinnan suoraan sijaan säikeen Soita Synkronoi API, jotka vaikuttaa merkittävästi suorituskyvyn.

## <a name="segmented-listing"></a>Segmentoitu luettelo

Pilvitallennustilaa asteikon edellyttää Segmentoitu luettelo. Voit esimerkiksi on miljoona BLOB-objektit Azure-blob-säilö tai Azure-taulukosta miljardin kohteiden päälle. Niitä ei teoreettinen numeroita, mutta todellisia asiakkaiden käyttö tapauksissa.

Näin ollen hankalaa Luettele kaikki objektit yhteen vastausta. Sen sijaan voit lisätä objektien sivutus. Luettelon ohjelmointirajapinnan on *Segmentoitu* ylikuormittaa.

Segmentoitu luettelo-toiminnon vastaus sisältää:

-   <i>_segment</i>, joka sisältää luettelon API yhden puhelun tuloksia.
-   *continuation_token*, jotka siirretään seuraavan puhelun, jotta saat tulosten seuraavalle sivulle. Kun ei enää palauttaa tuloksia, jatkumista tunnus on null.

Esimerkiksi tyypillinen puhelun Luettele kaikki BLOB-säilö voi näyttää samalta kuin seuraavassa koodikatkelman. Koodi on käytettävissä sekä [näytteiden](https://github.com/Azure/azure-storage-cpp/blob/master/Microsoft.WindowsAzure.Storage/samples/BlobsGettingStarted/Application.cpp):

    // List blobs in the blob container
    azure::storage::continuation_token token;
    do
    {
        azure::storage::list_blob_item_segment segment = container.list_blobs_segmented(token);
        for (auto it = segment.results().cbegin(); it != segment.results().cend(); ++it)
    {
        if (it->is_blob())
        {
            process_blob(it->as_blob());
        }
        else
        {
            process_diretory(it->as_directory());
        }
    }

        token = segment.continuation_token();
    }
    while (!token.empty());

Huomaa, että sivulle palautettujen tulosten määrää voivat valvoa kunkin API liikaa-parametri- *max_results* esimerkiksi:

    list_blob_item_segment list_blobs_segmented(const utility::string_t& prefix, bool use_flat_blob_listing,
        blob_listing_details::values includes, int max_results, const continuation_token& token,
        const blob_request_options& options, operation_context context)

Jos *max_results* -parametria ei määritetä, korkeintaan 5 000 tulosten enimmäismäärä oletusarvon palautetaan yhdelle sivulle.

Huomaa, että kysely Azure-taulukkotallennus voi palauttaa tietueita ei ole tai vähemmän kuin arvo, jonka olet määrittänyt *max_results* parametrin tietueita vaikka jatkumista tunnuksen ei ole tyhjä. Yksi syy saattaa olla, että kysely ei onnistunut viiden sekunnin kuluttua. Kun jatkumista tunnuksen ei ole tyhjä, kysely olisi edelleen ja koodisi ei tule olettaa segmentin tulosten kokoa.

Useimmat skenaariot coding suositellut malli on Segmentoitu luettelo, joka sisältää luettelon tai kyselyt ja miten palvelun reagoi sivupyynnön eksplisiittinen etenemisen. Erityisen C++-sovelluksia ja palveluja varten alemman tason ohjausobjektin luettelon edistymisestä auttavat ohjausobjektin muistia ja suorituskykyä.

## <a name="greedy-listing"></a>Alho luettelo

Tallennustilan asiakkaan kirjasto c++: aa aiemmissa versioissa (versiot 0.5.0 esikatselu tai sitä vanhempi versio) levy-Segmentoitu etsimääsi ohjelmointirajapinnan taulukot ja olevien, kuten seuraavassa esimerkissä:

    std::vector<cloud_table> list_tables(const utility::string_t& prefix) const;
    std::vector<table_entity> execute_query(const table_query& query) const;
    std::vector<cloud_queue> list_queues() const;

Segmentoitu ohjelmointirajapinnan kääreisiin on toteutettu seuraavista tavoista. Segmentoitu luettelon kunkin vastauksen koodin tulokset liitetään vektorista ja palauttaa kaikki tulokset, kun koko säilöjen skannattujen.

Tämän menetelmän toimi, jos tallennustila-tili tai taulukko sisältää pieneen objektien. Kuitenkin objektien määrä kasvaa, jossa tarvittavaa muistia voi suurentaa ilman rajoitusta, koska kaikki tulokset olleet muistiin. Yksi luettelo saattaa kestää kauan aikaa, jona soittajan oli etenemisen mitään tietoja.

Nämä alho luettelon API SDK: ssa ei ole C#, Java tai JavaScript Node.js-ympäristössä. Vältä mahdolliset ongelmat nämä alho ohjelmointirajapinnan käyttäminen on poistanut hänet versiossa 0.6.0 esikatselu.

Jos koodi on kutsumista nämä alho API:

    std::vector<azure::storage::table_entity> entities = table.execute_query(query);
    for (auto it = entities.cbegin(); it != entities.cend(); ++it)
    {
        process_entity(*it);
    }

Valitse Muokkaa koodin käyttämään Segmentoitu luettelon API:

    azure::storage::continuation_token token;
    do
    {
        azure::storage::table_query_segment segment = table.execute_query_segmented(query, token);
        for (auto it = segment.results().cbegin(); it != segment.results().cend(); ++it)
        {
            process_entity(*it);
        }

        token = segment.continuation_token();
    } while (!token.empty());

Segmentin *max_results* -parametrin määrittämällä saldo pyynnöt numerot ja muistinkäytön täyttävän suorituskykyyn liittyviä tietoja sovelluksen välillä.

Lisäksi jos Segmentoitu luettelon ohjelmointirajapinnan on käytössä, mutta tiedot tallennetaan paikallisten sivustokokoelman "alho" tyylin, myös suositeltavaa refactor koodisi käsittelemään tiedot tallennetaan paikallisten sivustokokoelman huolellisesti, asteikko.

## <a name="lazy-listing"></a>Kuitata luettelo

Alho luettelon korotettuna mahdollisen ongelman, mutta se on kätevä, jos säilössä eivät ole liian monta objekteja.

Jos käytät myös C#- tai Oracle Java SDK: T, pitäisi tuttuja järjestelmäelementtien ohjelmoinnin malli, joka tarjoaa kuitata-tyylin luettelo, jossa tiettyjä osoitteesta tietoja haetaan vain tarvittaessa. C++-kyselymerkkijonon perustuva-mallissa on myös samaa menetelmää.

Tyypillinen kuitata luettelo API, käyttämällä **list_blobs** esimerkkinä näyttää seuraavanlaiselta:

    list_blob_item_iterator list_blobs() const;

Tyypillinen koodikatkelman, joka käyttää kuitata luettelon kuvion voi näyttää tältä:

    // List blobs in the blob container
    azure::storage::list_blob_item_iterator end_of_results;
    for (auto it = container.list_blobs(); it != end_of_results; ++it)
    {
        if (it->is_blob())
        {
            process_blob(it->as_blob());
        }
        else
        {
            process_directory(it->as_directory());
        }
    }

Huomaa, että kuitata luettelo on käytettävissä vain synkronoitu tila.

Verrattuna alho luettelon kuitata luettelon hakee tietoja vain silloin, kun se on tarpeen. Koskee, valitse se hakee tietoja Azure säilöstä vain silloin, kun seuraavan kyselymerkkijonon siirtyy seuraavaan osioon. Tämän vuoksi muistinkäytön ohjataan kanssa joka koon ja toiminto on nopea.

Version 2.2.0, sisällytetään laiskan luettelon API tallennustilan asiakkaan kirjaston C++.

## <a name="conclusion"></a>Tekemistä

Tässä artikkelissa on kerrottu eri Osastollasi luettelon API eri objektien tallennustila asiakkaan kirjaston C++ varten. Yhteenveto:

-   Asynkroninen ohjelmointirajapinnan on erittäin suositeltavaa useita säikeistysmallin tilanteissa.
-   Segmentoitu luettelon suositellaan useimmissa tilanteissa.
-   Kuitata luettelo on annettu kirjastossa helposti paketti synkronisen tilanteissa.
-   Alho luetteloa ei ole suositeltavaa, ja se on poistettu kirjastosta.

##<a name="next-steps"></a>Seuraavat vaiheet

Saat lisätietoja Azure-tallennustilan ja asiakkaan kirjaston C++, on seuraavissa resursseissa.

-   [Opi käyttämään C++-Blob-objektien tallennustilaan](storage-c-plus-plus-how-to-use-blobs.md)
-   [Opi käyttämään C++-taulukkotallennus](storage-c-plus-plus-how-to-use-tables.md)
-   [Opi käyttämään jonon tallennustilaa C++](storage-c-plus-plus-how-to-use-queues.md)
-   [Azure tallennustilan asiakkaan kirjasto C++ API ohjeita.](http://azure.github.io/azure-storage-cpp/)
-   [Azure-tallennustilan tiimin blogia](http://blogs.msdn.com/b/windowsazurestorage/)
-   [Azure-tallennustilan dokumentaatio](https://azure.microsoft.com/documentation/services/storage/)
