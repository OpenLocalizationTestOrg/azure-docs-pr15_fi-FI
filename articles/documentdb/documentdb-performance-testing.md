<properties 
    pageTitle="DocumentDB asteikko ja suorituskyvyn testaaminen | Microsoft Azure" 
    description="Opi suorittamaan asteikko ja suorituskyvyn testaaminen oikeilla Azure DocumentDB"
    keywords="Suorituskyvyn testaaminen"
    services="documentdb" 
    authors="arramac" 
    manager="jhubbard" 
    editor="" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/27/2016" 
    ms.author="arramac"/>

# <a name="performance-and-scale-testing-with-azure-documentdb"></a>Suorituskyky ja skaalattavuus testaaminen oikeilla Azure DocumentDB
Suorituskyvyn ja mittakaava testaaminen on sovellusten kehittämisen avaimen vaihe. Monta sovellusten tietokannan taso on yleinen suorituskyky ja skaalattavuus merkittäviä vaikutus, joten on tärkeä osa suorituskyvyn testauksen. [Azure DocumentDB](https://azure.microsoft.com/services/documentdb/) on purpose-built joustavasti asteikko ja ennakoitavissa suorituskyky ja sen vuoksi hienoa Sovita sovelluksissa, jotka on tehokas tietokannan taso. 

Tässä artikkelissa on sovelluskehittäjille käyttöönoton suorituskyvyn testi-ohjelmistopaketit DocumentDB niiden työmääriä varten tai arvioimisen DocumentDB tehokkaan sovelluksen skenaarioissa viittaus. Ensisijaisesti keskitytään erillään suorituskyvyn testaaminen tietokannan, mutta myös tuotannon sovellusten parhaat käytännöt.

Luettuasi tämän artikkelin osaat seuraaviin kysymyksiin:   

- Mistä löydän otoksen .NET-asiakassovellukseen suorituskyvyn testausta Azure DocumentDB varten? 
- Kuinka suuri siirtonopeuden tasot ja Azure DocumentDB saavuttamiseksi asiakas-sovelluksesta?

Aloita koodi, lataa projektin [DocumentDB suorituskyvyn testaaminen otoksen](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark)osoitteesta. 

> [AZURE.NOTE] Tämän sovelluksen lähinnä osoittamaan parhaita käytäntöjä, joka poimii paremmin mahdollisen hyödyn DocumentDB asiakaskoneiden on vähän. Tämä ei määritettiin osoittamaan palvelun, joka voi skaalata limitlessly huippu kapasiteetti.

Jos etsit asiakkaan määritysasetukset DocumentDB suorituskyvyssä on artikkelissa [DocumentDB vihjeitä tehokkuuden parantamiseksi](documentdb-performance-tips.md).

## <a name="run-the-performance-testing-application"></a>Suorita testaus sovellusten suorituskykyä
Nopein tapa aloittaa on Käännä ja suorita .NET otosten alla kuvatulla seuraavien vaiheiden kuvissa. Voit tarkastella lähdekoodin ja toteuttaa samanlaisia käyttömahdollisuudet oman asiakassovelluksiin.

**Vaihe 1:** Lataa projektin [DocumentDB suorituskyvyn testaaminen malli](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark)tai haarautuvat Github säilö.

**Vaihe 2:** Muokkaa asetuksia EndpointUrl, AuthorizationKey, CollectionThroughput ja DocumentTemplate (valinnainen) App.config.

> [AZURE.NOTE] Ennen valmistelu sivustokokoelmat suuren siirtonopeuden kanssa, tutustu [Hinnat sivun](https://azure.microsoft.com/pricing/details/documentdb/) kustannusarvioita sivustokokoelman kohden. DocumentDB laskut tallennustilan ja siirtonopeuden itsenäisesti-tunnissa, jotta voit tallentaa kustannukset poistamalla tai pienentäminen DocumentDB sivustokokoelmat siirtonopeuden testauksen jälkeen.

**Vaihe 3:** Käännä ja Suorita komentoriviltä console-sovellus. Pitäisi näkyä tulosteen, kuten jompikumpi seuraavista:

    Summary:
    ---------------------------------------------------------------------
    Endpoint: https://docdb-scale-demo.documents.azure.com:443/
    Collection : db.testdata at 50000 request units per second
    Document Template*: Player.json
    Degree of parallelism*: 500
    ---------------------------------------------------------------------

    DocumentDBBenchmark starting...
    Creating database db
    Creating collection testdata
    Creating metric collection metrics
    Retrying after sleeping for 00:03:34.1720000
    Starting Inserts with 500 tasks
    Inserted 661 docs @ 656 writes/s, 6860 RU/s (18B max monthly 1KB reads)
    Inserted 6505 docs @ 2668 writes/s, 27962 RU/s (72B max monthly 1KB reads)
    Inserted 11756 docs @ 3240 writes/s, 33957 RU/s (88B max monthly 1KB reads)
    Inserted 17076 docs @ 3590 writes/s, 37627 RU/s (98B max monthly 1KB reads)
    Inserted 22106 docs @ 3748 writes/s, 39281 RU/s (102B max monthly 1KB reads)
    Inserted 28430 docs @ 3902 writes/s, 40897 RU/s (106B max monthly 1KB reads)
    Inserted 33492 docs @ 3928 writes/s, 41168 RU/s (107B max monthly 1KB reads)
    Inserted 38392 docs @ 3963 writes/s, 41528 RU/s (108B max monthly 1KB reads)
    Inserted 43371 docs @ 4012 writes/s, 42051 RU/s (109B max monthly 1KB reads)
    Inserted 48477 docs @ 4035 writes/s, 42282 RU/s (110B max monthly 1KB reads)
    Inserted 53845 docs @ 4088 writes/s, 42845 RU/s (111B max monthly 1KB reads)
    Inserted 59267 docs @ 4138 writes/s, 43364 RU/s (112B max monthly 1KB reads)
    Inserted 64703 docs @ 4197 writes/s, 43981 RU/s (114B max monthly 1KB reads)
    Inserted 70428 docs @ 4216 writes/s, 44181 RU/s (115B max monthly 1KB reads)
    Inserted 75868 docs @ 4247 writes/s, 44505 RU/s (115B max monthly 1KB reads)
    Inserted 81571 docs @ 4280 writes/s, 44852 RU/s (116B max monthly 1KB reads)
    Inserted 86271 docs @ 4273 writes/s, 44783 RU/s (116B max monthly 1KB reads)
    Inserted 91993 docs @ 4299 writes/s, 45056 RU/s (117B max monthly 1KB reads)
    Inserted 97469 docs @ 4292 writes/s, 44984 RU/s (117B max monthly 1KB reads)
    Inserted 99736 docs @ 4192 writes/s, 43930 RU/s (114B max monthly 1KB reads)
    Inserted 99997 docs @ 4013 writes/s, 42051 RU/s (109B max monthly 1KB reads)
    Inserted 100000 docs @ 3846 writes/s, 40304 RU/s (104B max monthly 1KB reads)

    Summary:
    ---------------------------------------------------------------------
    Inserted 100000 docs @ 3834 writes/s, 40180 RU/s (104B max monthly 1KB reads)
    ---------------------------------------------------------------------
    DocumentDBBenchmark completed successfully.


**Vaiheessa 4 (tarvittaessa):** Työkalusta (RU/s) raportoitu siirtonopeuden on oltava samat tai suurempi kuin kokoelman valmistellun siirtonopeuden. Muussa tapauksessa lisääntyvien vähitellen DegreeOfParallelism voivat auttaa sinua saavuttaa rajoitus. Jos asiakas-sovelluksen siirtonopeuden ylätasangoilla avaamista useita samassa tai eri tietokoneissa sovellusten esiintymiä avulla voit saavuttaa valmistellun rajoitus eri eri esiintymien välillä. Jos tarvitset apua tässä vaiheessa, kirjoita kirjoittaa sähköpostiin askdocdb@microsoft.com tai täyttäminen tuki lippu.

Kun olet sovelluksen käytössä, voit kokeilla eri [indeksointi käytännöt](documentdb-indexing-policies.md) ja [yhdenmukaisuuden tasot](documentdb-consistency-levels.md) ymmärtää niiden vaikutus siirtonopeuden ja viive. Voit tarkastella lähdekoodin ja toteuttaa oman testi-ohjelmistopaketit tai tuotannon sovellusten samalla määrityksiä.

## <a name="next-steps"></a>Seuraavat vaiheet
Tässä artikkelissa on tarkastellut siitä, miten voit suorittaa suorituskyky ja skaalattavuus testaaminen oikeilla DocumentDB .NET konsolin sovellusta käytettäessä. Katso lisätietoja käsittelystä DocumentDB linkeistä.

* [DocumentDB suorituskyvyn testauksen malli](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark)
* [Asiakkaan asetukset DocumentDB suorituskyvyn parantamiseksi](documentdb-performance-tips.md)
* [Palvelinpuolen DocumentDB jakaminen](documentdb-partition-data.md)
* [DocumentDB sivustokokoelmat ja suorituskyvyn tasoon](documentdb-performance-levels.md)
* [DocumentDB .NET SDK MSDN: N ohjeet](https://msdn.microsoft.com/library/azure/dn948556.aspx)
* [DocumentDB .NET-objektit](https://github.com/Azure/azure-documentdb-net)
* [DocumentDB blogi-suorituskykyyn liittyviä vihjeitä](https://azure.microsoft.com/blog/2015/01/20/performance-tips-for-azure-documentdb-part-1-2/)
