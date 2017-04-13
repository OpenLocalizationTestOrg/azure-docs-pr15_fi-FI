<properties
   pageTitle="Azure haun palvelun REST API-versio 2015-02-28-esikatselu | Microsoft Azure | Azure Search-esikatselu Ohjelmointirajapinta"
   description="Azure haun palvelun REST API-versio 2015-02-28-esikatselu on koe toimintoja, kuten luonnollisen kielen Analyzers ja moreLikeThis hakuja."
   services="search"
   documentationCenter="na"
   authors="brjohnstmsft"
   manager="pablocas"
   editor=""/>

<tags
   ms.service="search"
   ms.devlang="rest-api"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="search"
   ms.date="09/07/2016"
   ms.author="brjohnst"/>

# <a name="azure-search-service-rest-api-version-2015-02-28-preview"></a>Azure Search-palvelun REST API: Versio 2015-02-28-esikatselu

Tässä artikkelissa on ohjeessa `api-version=2015-02-28-Preview`. Esikatselu laajentaa yleisesti saatavilla nykyversio [api-version = 2015-02-28](https://msdn.microsoft.com/library/dn798935.aspx), antamalla koe seuraavat toiminnot:

- `moreLikeThis`kyselyn parametri [Etsi asiakirjoista](#SearchDocs) API. Se löytää muita asiakirjoja, jotka liittyvät toiseen tiettyyn tiedostoon.

Muutamia muita osia `2015-02-28-Preview` REST API on kuvattu erikseen. Näitä ovat:

- [Näkyvissä profiilit pistemäärä](search-api-scoring-profiles-2015-02-28-preview.md)
- [Indeksoijilla](search-api-indexers-2015-02-28-preview.md)

Azure Search-palvelu on käytettävissä on useita versioita. Tutustu [Search-palvelun versiotietojen](http://msdn.microsoft.com/library/azure/dn864560.aspx) lisätietoja.

## <a name="apis-in-this-document"></a>API tässä asiakirjassa

Azure haun service API tukee kahta URL-Osoitteen muodot API toimille: yksinkertaisen ja OData (Katso lisätietoja [OData (Azure haun API) tuki](http://msdn.microsoft.com/library/azure/dn798932.aspx) ). Seuraavassa luettelossa on yksinkertainen syntaksia.

[Indeksin luominen](#CreateIndex)

    POST /indexes?api-version=2015-02-28-Preview

[Hakemiston päivittäminen](#UpdateIndex)

    PUT /indexes/[index name]?api-version=2015-02-28-Preview

[Hae hakemisto](#GetIndex)

    GET /indexes/[index name]?api-version=2015-02-28-Preview

[Luettelon indeksit](#ListIndexes)

    GET /indexes?api-version=2015-02-28-Preview

[Hae tilastot](#GetIndexStats)

    GET /indexes/[index name]/stats?api-version=2015-02-28-Preview

[Testaa analysoiminen](#TestAnalyzer)

    POST /indexes/[index name]/analyze?api-version=2015-02-28-Preview

[Indeksin poistaminen](#DeleteIndex)

    DELETE /indexes/[index name]?api-version=2015-02-28-Preview

[Lisätä, poistaa, ja indeksin tietojen päivittäminen](#AddOrUpdateDocuments)

    POST /indexes/[index name]/docs/index?api-version=2015-02-28-Preview

[Etsi asiakirjoista](#SearchDocs)

    GET /indexes/[index name]/docs?[query parameters]
    POST /indexes/[index name]/docs/search?api-version=2015-02-28-Preview

[Haku-asiakirja](#LookupAPI)

     GET /indexes/[index name]/docs/[key]?[query parameters]

[Laske-asiakirjat](#CountDocs)

    GET /indexes/[index name]/docs/$count?api-version=2015-02-28-Preview

[Ehdotukset](#Suggestions)

    GET /indexes/[index name]/docs/suggest?[query parameters]
    POST /indexes/[index name]/docs/suggest?api-version=2015-02-28-Preview

________________________________________
<a name="IndexOps"></a>
## <a name="index-operations"></a>Indeksi-toiminnot

Voit luoda ja hallita indeksit Azure Search-palvelun kautta yksinkertainen vastaan annettu indeksin resurssin pyyntöjen (POST, GET-HYLLYTETTY, poista). Jos haluat luoda hakemiston, JULKAISET JSON asiakirjan, joka kuvaa indeksi-rakenne. Rakenne määrittää kentät, indeksi, niiden tietotyypit ja miten niitä voi käyttää (esimerkiksi teksti-haut, suodattimia, lajittelun ja faceting). Se määrittää myös määrittää hakemiston toimintaa tulosmalli profiilit, suggesters ja määritteet.

Seuraavassa esimerkissä on etsiminen hotellista tiedot kielet määritelty kuvaus-kentän kanssa käytetään rakenne on kuva. Huomaa, kuinka määritteet hallita sitä, miten kenttää käytetään. Esimerkiksi `hotelId` käytetään asiakirjan avaimeksi (`"key": true`), eikä se sisälly koko tekstin haut (`"searchable": false`).

    {
    "name": "hotels",  
    "fields": [
      {"name": "hotelId", "type": "Edm.String", "key": true, "searchable": false},
      {"name": "baseRate", "type": "Edm.Double"},
      {"name": "description", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false},
      {"name": "description_fr", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false, "analyzer": "fr.lucene"},
      {"name": "hotelName", "type": "Edm.String"},
      {"name": "category", "type": "Edm.String"},
      {"name": "tags", "type": "Collection(Edm.String)"},
      {"name": "parkingIncluded", "type": "Edm.Boolean"},
      {"name": "smokingAllowed", "type": "Edm.Boolean"},
      {"name": "lastRenovationDate", "type": "Edm.DateTimeOffset"},
      {"name": "rating", "type": "Edm.Int32"},
      {"name": "location", "type": "Edm.GeographyPoint"}
     ],
     "suggesters": [
      {
       "name": "sg",
       "searchMode": "analyzingInfixMatching",
       "sourceFields": ["hotelName"]
      }
     ]
    }

Kun indeksi on luotu, voit ladata tiedostot, jotka täytä indeksi. [Lisää tai päivitä asiakirjojen](#AddOrUpdateDocuments) saat tämän seuraavaan vaiheeseen.

Katso video johdanto Azure haun indeksoinnista [kanavan 9 Cloud kattavat jakson Azure-haun](http://go.microsoft.com/fwlink/p/?LinkId=511509).


<a name="CreateIndex"></a>
## <a name="create-index"></a>Indeksin luominen

Indeksi on ensisijaisina järjestäminen ja hakeminen asiakirjojen Azure-haku, miten taulukon järjestää tietokannan tietueita samalla. Kunkin indeksi on kokoelma asiakirjoja, että kaikki indeksi rakenteen (kenttien nimet, tietotyypit ja ominaisuudet) mukainen, mutta indeksit määrittää myös muita rakenteita (suggesters, tulosmalli-profiileista ja CORS asetukset), jotka määrittävät, Etsi muita ominaisuuksia.

Voit luoda uuden indeksin avulla HTTP POST tai käyttöön pyynnön Azure Search-palvelun. Pyynnön teksti on JSON-rakenne, joka määrittää indeksi- ja määrityspalveluja tiedot.

    POST https://[service name].search.windows.net/indexes?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

Voit vaihtoehtoisesti käyttää HYLLYTETTY ja määritä URI indeksinimi. Jos indeksi ei ole, se luodaan.

    PUT https://[search service url]/indexes/[index name]?api-version=[api-version]

Hakemiston luominen määrittää tiedostoja tallennettu ja Etsi toimissa rakenne. Täyttää indeksi on erillinen toiminto. Tässä vaiheessa voit [indeksointitoiminto](https://msdn.microsoft.com/library/azure/mt183328.aspx) (käytettävissä tietolähteiden) tai [Lisää, Päivitä, tai poista asiakirjat](https://msdn.microsoft.com/library/azure/dn798930.aspx) -toimintoa. Käänteinen indeksi muodostetaan, kun asiakirjat on kirjattu.

**Huomautus**: indeksit sallittu enimmäismäärä vaihtelee hinnoittelu taso. Vapaa-palvelun avulla 3 indeksit. Vakio-palvelun avulla 50 indeksit hakupalvelun kohden. Katso lisätietoja [rajat ja rajoitukset](http://msdn.microsoft.com/library/azure/dn798934.aspx) .

**Pyyntö**

HTTPS tarvitaan kaikkia palvelupyyntöjä. **Create Index** -pyyntö on rakennettava viestin tai HYLLYTETTY-menetelmällä. Käytettäessä kirjaa, sinun on määritettävä Indeksinimi sekä indeksin rakenteen määritys pyynnön tekstissä. Indeksinimi on HYLLYTETTY, URL-Osoitteen osa. Jos indeksi ei ole valittu, se luodaan. Jos se on jo olemassa, se päivitetään uuden määritelmän.

Indeksinimi on kirjoitettava pienillä kirjaimilla, aloittaminen kirjaimen tai numeron, on tai ei ole vinoviivaa pistettä ja voi olla pienempi kuin 128: aa merkkiä. Käynnistyksen Indeksinimi kirjaimen tai numeron jälkeen loput nimi voi sisältää kirjain, numero ja katkoviivat, kunhan viivat eivät ole peräkkäisiä.

`api-version` Tarvitaan. [Search-palvelun versiotietojen](http://msdn.microsoft.com/library/azure/dn864560.aspx) saat luettelon käytettävissä olevista versioista.

**Pyydä otsikot**

Seuraavassa luettelossa kuvataan pakolliset ja valinnaiset pyynnön otsikot.

- `Content-Type`: Pakollinen. Määritä tähän`application/json`
- `api-key`: Pakollinen. `api-key` Käytetään
- todentaa Search-palvelun pyynnön. Merkkijonoarvo, yksilöllisiä palvelusi on. **Create Index** -pyyntö on sisällettävä `api-key` otsikon määrittää järjestelmänvalvojan key-tuotetunnuksen (eikä kyselyn avain).

Sinun on myös muodostaa pyyntö URL-Osoitteen palvelunimi. Saat sekä palvelunimi ja `api-key` service-koontinäytössä Azure-portaalissa. Katso Ohje sivuissa Siirtyminen [luominen Azure Search-palvelun portaalissa](search-create-service-portal.md) .

<a name="RequestData"></a>
**Leipäteksti syntaksi**

Pyynnön leipätekstissä rakenteen määritys, joka sisältää luettelon tietokenttiä sisällä tiedostoja, jotka syötettävä tämän indeksin, tietotyyppejä, määritteet sekä valinnainen luettelo näkyvissä pistemäärä profiilit, joita käytetään sija vastaavia asiakirjoja kyselyn yhteydessä.

Huomaa, että VIESTIIN pyytää, on määritettävä Indeksinimi pyynnön tekstissä.

Voi olla vain yhden kentän indeksin. Sen on oltava merkkijono. Tässä kentässä edustaa kunkin asiakirjan hakemiston tallennettuna yksilöllinen.

Indeksin tärkeimmät osat ovat seuraavat:

- `name`
- `fields`joka syötettävä tämän indeksin, kuten nimi, tietotyyppi ja ominaisuuksia, jotka määrittävät sallitut toiminnot-kenttään.
- `suggesters`käytetään automaattisen täydennyksen tai täydentävä kyselyissä.
- `scoringProfiles`käytettävä mukautettu haku pistemäärän luokitus. [Lisää tulosmalli-profiileista](https://msdn.microsoft.com/library/azure/dn798928.aspx) lisätietoja.
- `analyzers`, `charFilters`, `tokenizers`, `tokenFilters` avulla määritetään, miten tiedostoja ja kysely on jaettu Indeksoitavalla/etsittävän tunnusten. [Azure haun analyyseja](https://aka.ms//azsanalysis) lisätietoja.
- `defaultScoringProfile`voidaan korvata näkyvissä pistemäärä toiminnan oletusarvo.
- `corsOptions`Jos haluat, että rajat origin kyselyitä, jotka perustuvat indeksi.

Pyyntö paketti jäsentämiseen syntaksi on seuraava. Esimerkki pyyntö on annettu edelleen artikkelista.

    {
      "name": (optional on PUT; required on POST) "name_of_index",
      "fields": [
        {
          "name": "name_of_field",
          "type": "Edm.String | Collection(Edm.String) | Edm.Int32 | Edm.Int64 | Edm.Double | Edm.Boolean | Edm.DateTimeOffset | Edm.GeographyPoint",
          "searchable": true (default where applicable) | false (only Edm.String and Collection(Edm.String) fields can be searchable),
          "filterable": true (default) | false,
          "sortable": true (default where applicable) | false (Collection(Edm.String) fields cannot be sortable),
          "facetable": true (default where applicable) | false (Edm.GeographyPoint fields cannot be facetable),
          "key": true | false (default, only Edm.String fields can be keys),
          "retrievable": true (default) | false,              
          "analyzer": "name of the analyzer used for search and indexing", (only if 'searchAnalyzer' and 'indexAnalyzer' are not set)
          "searchAnalyzer": "name of the search analyzer", (only if 'indexAnalyzer' is set and 'analyzer' is not set)
          "indexAnalyzer": "name of the indexing analyzer" (only if 'searchAnalyzer' is set and 'analyzer' is not set)
        }
      ],
      "suggesters": [
        {
          "name": "name of suggester",
          "searchMode": "analyzingInfixMatching" (other modes may be added in the future),
          "sourceFields": ["field1", "field2", ...]
        }
      ],
      "scoringProfiles": [
        {
          "name": "name of scoring profile",
          "text": (optional, only applies to searchable fields) {
            "weights": {
              "searchable_field_name": relative_weight_value (positive numbers),
              ...
            }
          },
          "functions": (optional) [
            {
              "type": "magnitude | freshness | distance | tag",
              "boost": # (positive number used as multiplier for raw score != 1),
              "fieldName": "...",
              "interpolation": "constant | linear (default) | quadratic | logarithmic",
              "magnitude": {
                "boostingRangeStart": #,
                "boostingRangeEnd": #,
                "constantBoostBeyondRange": true | false (default)
              },
              "freshness": {
                "boostingDuration": "..." (value representing timespan leading to now over which boosting occurs)
              },
              "distance": {
                "referencePointParameter": "...", (parameter to be passed in queries to use as reference location, see "scoringParameter" for syntax details)
                "boostingDistance": # (the distance in kilometers from the reference location where the boosting range ends)
              },
              "tag": {
                "tagsParameter": "..." (parameter to be passed in queries to specify list of tags to compare against target field, see "scoringParameter" for syntax details)
              }
            }
          ],
          "functionAggregation": (optional, applies only when functions are specified)
            "sum (default) | average | minimum | maximum | firstMatching"
        }
      ],
      "analyzers":(optional)[ ... ],
      "charFilters":(optional)[ ... ],
      "tokenizers":(optional)[ ... ],
      "tokenFilters":(optional)[ ... ],
      "defaultScoringProfile": (optional) "...",
      "corsOptions": (optional) {
        "allowedOrigins": ["*"] | ["origin_1", "origin_2", ...],
        "maxAgeInSeconds": (optional) max_age_in_seconds (non-negative integer)
      }
    }

**Indeksimääritteet**

Seuraavien määritteiden voidaan määrittää hakemiston luomiseen. Lisätietoja tulosmalli ja tulosmalli-profiileista on artikkelissa [Lisää tulosmalli profiilit](https://msdn.microsoft.com/library/azure/dn798928.aspx).

`name`-Asettaa kentän nimi.

`type`-Asettaa kentän tietotyyppi. Katso luettelo tuetuista tyypeistä [Tuetut tietotyypit](#DataTypes) .

`searchable`-Merkitsee kentän koko tekstin haun pysty. Tämä tarkoittaa, että se tehdään analyysi, kuten word uusinta indeksoinnin aikana. Jos käytössä `searchable` kentän arvoa, kuten "sataako day", sisäisesti se jaetaan yksittäisiä tunnusten "sataako" ja "päivämäärä". Näin koko tekstin etsii näitä ehtoja. Tyypin kenttiä `Edm.String` tai `Collection(Edm.String)` ovat `searchable` oletusarvoisesti. Muita kenttiä ei voi olla `searchable`.

  - **Huomautus**: `searchable` kentät tarjoaman lisätilaa, indeksi, koska Azure haun tallentaa tokenized versio määrittäminen teksti-kenttäarvo. Jos haluat tallentaa tilaa indeksi ja sinun ei tarvitse kentän sisällytetään haut, Määritä `searchable` , `false`.

`filterable`-Mahdollistaa valitsinta kenttä `$filter` kyselyt. `filterable`ei ole sama kuin `searchable` -merkkijonoja käsittelytavan. Tyypin kenttiä `Edm.String` tai `Collection(Edm.String)` , jotka ovat `filterable` niille ei suoriteta word uusinta niin vertailuja ovat vain tarkan vastaa. Jos määrität tällaista kenttää esimerkiksi `f` "sataako päivä-, `$filter=f eq 'sunny'` etsii yhtään osumaa, mutta `$filter=f eq 'sunny day'` on. Kaikki kentät ovat `filterable` oletusarvoisesti.

`sortable`-Järjestelmä lajittelee tulokset oletusarvoisesti tuloksen mukaan, mutta monet kokemukset-käyttäjät haluavat asiakirjoissa kenttien lajittelu. Tyypin kenttiä `Collection(Edm.String)` ei voi olla `sortable`. Muut kentät ovat `sortable` oletusarvoisesti.

`facetable`-Esityksen, joka sisältää määrä (esimerkiksi digitaaliset kamerat ja katso osumia brändiä, megapikseliä mukaan, hinta, jne.) luokittain hakutulokset tavanomaisesta käytöstä. Tämä asetus ei voi käyttää tyypin kenttiä `Edm.GeographyPoint`. Muut kentät ovat `facetable` oletusarvoisesti.

  - **Huomautus**:-tyypin kenttiä `Edm.String` , jotka ovat `filterable`, `sortable`, tai `facetable` voi olla enintään 32 kt pituisissa. Tämä johtuu siitä yhden hakusanan käsitellään kyseisiä kenttiä ja Azure haun termin enimmäispituus on 32 Kilotavua. Jos haluat tallentaa enemmän tekstiä kuin tämä yhden merkkijonon kenttään, sinun on määritetty `filterable`, `sortable`, ja `facetable` , `false` indeksi-määritys.

  - **Huomautus**: Jos kenttä on mikään yllä määritteiden asettaminen `true` (`searchable`, `filterable`, `sortable`, tai`facetable`) kentän tehokkaasti jätetään pois indeksissä. Tämä asetus on hyödyllinen, jos kenttä ei ole käytetty kyselyt, mutta hakutuloksissa tarvitaan. Kyseisiä kenttiä lukuun ottamatta indeksistä parantaa suorituskykyä.

`key`-Merkitsee kenttä sisältävät asiakirjat hakemiston yksilölliset tunnukset. Yksi kenttä on valittava `key` kentän ja sen on oltava tyyppiä `Edm.String`. Avainkenttien avulla voidaan etsiä asiakirjoja suoraan [Haku Ohjelmointirajapinnan](#LookupAPI)kautta.

`retrievable`-Määrittää, voivatko kentän funktio voi palauttaa hakutuloksen.  Tästä on hyötyä, kun haluat (esimerkiksi reunusten)-kentän käyttäminen suodattimena, lajittelun ja näkyvissä pistemäärä järjestelmä, mutta et halua olevan näkyvissä käyttäjälle kentän. Tämän määritteen on oltava `true` varten `key` kentät.

`analyzer`-Asettaa käytettävän kentän etsiminen haun ja indeksoinnin ajankohdan analyzer nimi. Katso sallittujen arvojoukon [Analyzers](https://msdn.microsoft.com/library/mt605304.aspx). Tämä vaihtoehto voidaan käyttää vain `searchable` kentät ja sitä ei voi määrittää kanssa joko `searchAnalyzer` tai `indexAnalyzer`.  Kun analysointitoiminto valitaan, sitä ei voi muuttaa kentän.

`searchAnalyzer`-Asettaa analyzer käytetään Etsi milloin kentän nimi. Katso sallittujen arvojoukon [Analyzers](https://msdn.microsoft.com/library/mt605304.aspx). Tämä vaihtoehto voidaan käyttää vain `searchable` kentät. Se on määritettävä yhdessä `indexAnalyzer` ja sitä ei voi määrittää yhdessä `analyzer` vaihtoehto. Tämä analyzer voidaan päivittää aiemmin luotuun kenttään.

`indexAnalyzer`-Asettaa analyzer käytetään indeksoinnin milloin kentän nimi. Katso sallittujen arvojoukon [Analyzers](https://msdn.microsoft.com/library/mt605304.aspx). Tämä vaihtoehto voidaan käyttää vain `searchable` kentät. Se on määritettävä yhdessä `searchAnalyzer` ja sitä ei voi määrittää yhdessä `analyzer` vaihtoehto. Kun analysointitoiminto valitaan, sitä ei voi muuttaa kentän.

`suggesters`-Määrittää haun tila ja kentät, jotka ovat ehdotuksia sisällön lähteen. Katso lisätietoja [Suggesters](#Suggesters) .

`scoringProfiles`-Määrittää mukautetun tulosmalli toiminnat, joiden avulla voit vaikuttaa kohteet näkyvät suurempi hakutuloksissa. Tulosmalli profiilit koostuvat kentän leveydet ja funktioita. Saat lisätietoja määritteitä, joita käytetään tulosmalli profiilin [Lisää näkyvissä pistemäärä profiilit](https://msdn.microsoft.com/library/azure/dn798928.aspx) .

<!-- This is a standalone topic in MSDN -->
<a name="LanguageSupport"></a>
**Kielten tuki**

Hakukenttiä niille analyysi, suurin liittyy usein word uusinta, tekstin normalisointi ja suodattaa pois ehdot. Oletusarvon mukaan hakukenttiä Azure hakutoiminnossa analysoidaan [Apache Lucene vakio analyzer](http://lucene.apache.org/core/4_9_0/analyzers-common/index.html) joka jakaa tekstin["Unicode-teksti Segmentointi"](http://unicode.org/reports/tr29/) sääntöjen osat. Lisäksi vakio analyzer muuntaa kaikki seuraavat merkit pieniksi kirjaimiksi lomakkeeseen. Indeksoidut asiakirjat ja hakusanojen käymällä läpi analyysin aikana indeksoinnin ja kyselyjen käsittelyä.

Azure haun tukee useilla kielillä. Kullekin kielelle edellyttää normaalista teksti-analyzer joka muodostaa ominaispiirteet valitsemallasi kielellä. Azure haku on kahdentyyppisiä analyzers:

- 35 analyzers palautettua Lucene mukaan.
- 50 analyzers palautettua omistusoikeuksia Microsoft luonnollisen kielen käsittelyn tekniikka, jolla Office-ja Bing mukaan.

Kehittäjät voivat mieluummin Lucene tutun, yksinkertainen, Avaa lähde-ratkaisun. Lucene analyzers on nopeampaa, mutta Microsoft analyzers on Lisäasetukset-ominaisuuksia, kuten hakusanat, word decompounding (kielillä esimerkiksi saksa, Tanska, hollanti, Ruotsi, Norja, Viro, valmis, unkari, slovakki) ja kohteen tunnistuksen (URL-osoitteet, sähköpostit, päivämääriä, lukuja). Jos mahdollista Suorita Microsoft ja Lucene analyzers päättää, kumpi mahtuu vertailuja.

***Miten ne vertailu***

Englannin kielen Lucene-analyzer laajentaa vakio analyzer. Se poistaa omistus (lopussa on) sanat, koskee perusmuodon [portteri perusmuodon algoritmin](http://tartarus.org/~martin/PorterStemmer/)mukaan sekä poistaa [lopettaa sanoja](http://en.wikipedia.org/wiki/Stop_words)englannin kielen.

Vertailu Microsoft analyzer suorittaa hakusanat sijaan perusmuodon. Se tarkoittaa sitä, se voi käsitellä inflected ja epäsäännöllinen sanamuodot paljon paremmin mitä tulokset useita hakutuloksissa (Katso moduuli 7 [Azure haun MVA](http://www.microsoftvirtualacademy.com/training-courses/adding-microsoft-azure-search-to-your-websites-and-apps) esityksen lisätietoja).

Indeksoinnin Microsoft analyzers kanssa on keskimäärin kahdesta kolme kertaa hitaammin Lucene vastaavat, kielen mukaan. Etsi suorituskyky olisi ei kohdistuu koko keskimäärin kyselyjen.

***Määritys***

Indeksin määrityksen kaikista kentistä, voit määrittää `analyzer` -ominaisuuden arvoksi analyzer-nimi, joka määrittää, mitkä kieli- ja toimittaja. Saman analyzer otetaan käyttöön, kun indeksoinnin ja kentän haku.
Esimerkiksi voi olla eri kentissä englannin, Ranskan ja Espanjan hotellista kuvaukset siellä olevia rinnakkais samaan hakemistoon. ['SearchFields' kysely parametrien](#SearchQueryParameters) avulla voit määrittää kielikohtaiset kenttä, jonka haluat etsiä vastaan kyselyissä. Jos haluat tarkistaa kyselyn esimerkkejä, jotka sisältävät `analyzer` [Etsi tiedostot](#SearchDocs)-ominaisuutta. 

***Analyzer-luettelo***

Alla on tukemat kielet ja Lucene ja Microsoft analyzer nimien luettelosta.

<table style="font-size:12">
    <tr>
        <th>Kieli</th>
        <th>Microsoft analyzer nimi</th>
        <th>Lucene analyzer nimi</th>
    </tr>
    <tr>
        <td>arabia</td>
        <td>ar.Microsoft</td>
        <td>ar.lucene</td>      
    </tr>
    <tr>
        <td>armenia</td>
        <td></td>
        <td>Hy.lucene</td>
    </tr>
    <tr>
        <td>Bengali</td>
        <td>bn.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>baski</td>
        <td></td>
        <td>EU.lucene</td>
    </tr>
    <tr>
        <td>bulgaria</td>
        <td>BG.Microsoft</td>
        <td>BG.lucene</td>
    </tr>
    <tr>
        <td>katalaani</td>
        <td>CA.Microsoft</td>
        <td>CA.lucene</td>          
    </tr>
    <tr>
        <td>yksinkertaistettu kiina</td>
        <td>ZH Hans.microsoft</td>
        <td>ZH Hans.lucene</td>     
    </tr>
    <tr>
        <td>perinteinen kiina</td>
        <td>ZH Hant.microsoft</td>
        <td>ZH Hant.lucene</td>     
    <tr>
    <tr>
        <td>kroaatti</td>
        <td>HR.Microsoft</td>
        <td/></td>
    </tr>
    <tr>
        <td>tšekki</td>
        <td>CS.Microsoft</td>
        <td>CS.lucene</td>      
    </tr>    
    <tr>
        <td>tanska</td>
        <td>da.Microsoft</td>
        <td>da.lucene</td>      
    </tr>    
    <tr>
        <td>hollanti</td>
        <td>NL.Microsoft</td>
        <td>NL.lucene</td>  
    </tr>    
    <tr>
        <td>englanti</td>        
        <td>en.Microsoft</td>
        <td>en.lucene</td>      
    </tr>
    <tr>
        <td>viro</td>
        <td>Et.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>suomi</td>
        <td>fi.Microsoft</td>
        <td>fi.lucene</td>      
    </tr>    
    <tr>
        <td>ranska</td>
        <td>FR.Microsoft</td>
        <td>FR.lucene</td>      
    </tr>
    <tr>
        <td>galego</td>
        <td></td>
        <td>GL.lucene</td>      
    </tr>
    <tr>
        <td>saksa</td>
        <td>de.Microsoft</td>
        <td>de.lucene</td>      
    </tr>
    <tr>
        <td>kreikka</td>
        <td>El.Microsoft</td>
        <td>El.lucene</td>      
    </tr>
    <tr>
        <td>gudzarati</td>
        <td>Gu.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>heprea</td>
        <td>he.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>hindi</td>
        <td>HI.Microsoft</td>
        <td>HI.lucene</td>      
    </tr>
    <tr>
        <td>unkari</td>      
        <td>HU.Microsoft</td>
        <td>HU.lucene</td>
    </tr>
    <tr>
        <td>islanti</td>
        <td>is.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Indonesia (Bahasa)</td>
        <td>ID.Microsoft</td>
        <td>ID.lucene</td>      
    </tr>
    <tr>
        <td>irlanti</td>
        <td></td>
        <td>GA.lucene</td>
    </tr>
    <tr>
        <td>italia</td>
        <td>IT.Microsoft</td>
        <td>IT.lucene</td>      
    </tr>
    <tr>
        <td>japani</td>
        <td>ja.Microsoft</td>
        <td>ja.lucene</td>
        
    </tr>
    <tr>
        <td>kannada</td>
        <td>ka.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>korea</td>
        <td>KO.Microsoft</td>
        <td>KO.lucene</td>
    </tr>
    <tr>
        <td>latvia</td>        
        <td>LV.Microsoft</td>
        <td>LV.lucene</td>  
    </tr>
    <tr>
        <td>liettua</td>
        <td>lt.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>malajalam</td>
        <td>ml.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Malaiji (latinalainen)</td>
        <td>MS.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>marathi</td>
        <td>MR.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>norja</td>
        <td>NB.Microsoft</td>
        <td>No.lucene</td>      
    </tr>
    <tr>
        <td>persia</td>
        <td></td>
        <td>FA.lucene</td>      
    </tr>
    <tr>
        <td>puola</td>
        <td>PL.Microsoft</td>
        <td>PL.lucene</td>      
    </tr>
    <tr>
        <td>Portugali (Brasilia)</td>
        <td>pt Br.microsoft</td>
        <td>pt Br.lucene</td>       
    </tr>
    <tr>
        <td>Portugali (Portugali)</td>
        <td>pt Pt.microsoft</td>        
        <td>pt Pt.lucene</td>
    </tr>
    <tr>
        <td>punjabi</td>
        <td>Pa.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>romania</td>
        <td>ro.Microsoft</td>
        <td>ro.lucene</td>
    </tr>
    <tr>
        <td>venäjä</td>
        <td>RU.Microsoft</td>
        <td>RU.lucene</td>  
    </tr>
    <tr>
        <td>serbia (kyrillinen)</td>
        <td>SR cyrillic.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>Serbia (latinalainen)</td>
        <td>SR latin.microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>slovakki</td>
        <td>SK.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>sloveeni</td>
        <td>SL.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>espanja</td>
        <td>ES.Microsoft</td>
        <td>ES.lucene</td>
    </tr>
    <tr>
        <td>ruotsi</td>
        <td>SV.Microsoft</td>
        <td>SV.lucene</td>
    </tr>

    <tr>
        <td>tamili</td>
        <td>ta.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>telugu</td>
        <td>Te.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>thai</td>
        <td>TH.Microsoft</td>
        <td>TH.lucene</td>
    </tr>
    <tr>
        <td>turkki</td>
        <td>TR.Microsoft</td>
        <td>TR.lucene</td>      
    </tr>
    <tr>
        <td>ukraina</td>
        <td>UK.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>urdu</td>
        <td>Your.Microsoft</td>
        <td></td>
    </tr>
    <tr>
        <td>vietnam</td>
        <td>VI.Microsoft</td>
        <td></td>
    </tr>
    <td colspan="3">Lisäksi Azure-haun avulla kielen ympäristöstä riippumattomalla tavalla analyzer käyttömahdollisuudet</td>
    <tr>
        <td>Vakio ASCII taitto</td>
        <td>standardasciifolding.lucene</td>
        <td>
        <ul>
            <li>Unicode-teksti Segmentointi (vakio Tokenizer)</li>
            <li>ASCII kokoontaitettavalla Suodatin - muuntaa Unicode-merkkejä, jotka eivät kuulu ensin 127 ASCII merkkiyhdistelmän ASCII vastaavat kyselyjä. Tästä on hyötyä poistaminen diakriittiset merkit.</li>
        </ul>
        </td>
    </tr>
</table>

Kaikki analyzers kohdassa <i>lucene</i> nimet ovat tarjoaa [Apache Lucene kielen analyzers](http://lucene.apache.org/core/4_9_0/analyzers-common/overview-summary.html). Lisätietoja suodattimen taitto ASCII löytyy [tähän](http://lucene.apache.org/core/4_9_0/analyzers-common/org/apache/lucene/analysis/miscellaneous/ASCIIFoldingFilter.html).

**Suggesters**

A `suggester` määrittää, mitkä kentät indeksin käytetään tukemaan automaattisen täydennyksen hauissa. Osittainen hakumerkkijono lähetetään yleensä [Ehdotuksia API](#Suggestions) samalla, kun käyttäjä on kirjoittamalla hakukyselyn ja Ohjelmointirajapinnan palauttaa joukon ehdotettu lauseita. Suggester, voit määrittää hakemiston määrittää kentät, jotka käytetään luonnissa täydentävä hakutoiminto ehdot. Lisätietoja [Suggesters](#Suggesters) määritys.

**Näkyvissä profiilit pistemäärä**

A `scoringProfile` määrittää mukautetun tulosmalli toiminnat, joiden avulla voit vaikuttaa kohteet näkyvät suurempi hakutuloksista. Tulosmalli profiilit koostuvat kentän leveydet ja funktioita. Voit käyttää niitä määrittää profiilin kyselymerkkijonon nimen mukaan.

Oletusarvoisesti näkyvissä pistemäärä profiilin toimii Laske haku-tulos jokaisen kohteen tulosjoukon taustalla. Voit käyttää sisäisiä, nimeämättömiä tulosmalli profiilin. Voit myös määrittää `defaultScoringProfile` kutsuttu käyttäminen mukautettujen profiilin oletusarvon mukaan aina, kun mukautetun profiilin ei ole määritetty kyselymerkkijonon.

[Lisää tulosmalli-profiileista hakuindeksin (Azure haun palvelun REST API)](search-api-scoring-profiles-2015-02-28-preview.md) lisätietoja.

**CORS asetukset**

Asiakkaan Javascript voi kutsua minkä tahansa API oletusarvoisesti, koska selaimen estää kaikki rajat origin pyynnöt. Ottaa käyttöön CORS (rajat Origin resurssien jakaminen) määrittämällä `corsOptions` määrite sallimaan rajat origin kyselyjä indeksi. Huomautus vain kyselyn tietoturvasyistä tuki API CORS. CORS voidaan määrittää seuraavat asetukset:

- `allowedOrigins`(pakollinen): Tämä on alkuperistä, indeksi käyttöoikeus myönnetään luettelo. Tämä tarkoittaa, että kaikki Javascript-koodia served alkuperistä seuraavasti: näiden on oikeus kyselyn indeksi (olettaen että se sisältää oikeat Ohjelmointirajapinnan avain). Kunkin alkuperän on yleensä lomakkeen `protocol://fully-qualified-domain-name:port` vaikka portti usein jätetään pois. Katso lisätietoja [Tässä artikkelissa](http://go.microsoft.com/fwlink/?LinkId=330822) .
 - Jos haluat käyttää kaikkia alkuperistä, Sisällytä `*` yhtenä kohteena `allowedOrigins` matriisi. Huomaa, että **se ei ole suositeltavaa harjoituksen haun palveluille.** Kuitenkaan voi olla hyötyä kehittäminen tai vianmääritystä varten.
- `maxAgeInSeconds`(valinnainen): selaimet tätä arvoa käytetään määritettäessä kesto (sekunteina) välimuistin CORS kuvatiedostomuodot vastauksiin. Tämä on oltava negatiivinen kokonaisluku. Mitä suurempi arvo on, parempi suorituskyky, mutta sitä kauemmin kestää CORS käytäntöjen muutokset tulevat voimaan. Jos se ei ole määritetty, käytetään oletusarvon kesto on 5 minuuttia.

<a name="CreateUpdateIndexExample"></a>
**Pyydä leipäteksti-Esimerkki**

    {
      "name": "hotels",  
      "fields": [
        {"name": "hotelId", "type": "Edm.String", "key": true, "searchable": false},
        {"name": "baseRate", "type": "Edm.Double"},
        {"name": "description", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false},
        {"name": "description_fr", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false, "analyzer": "fr.lucene"},
        {"name": "hotelName", "type": "Edm.String"},
        {"name": "category", "type": "Edm.String"},
        {"name": "tags", "type": "Collection(Edm.String)"},
        {"name": "parkingIncluded", "type": "Edm.Boolean"},
        {"name": "smokingAllowed", "type": "Edm.Boolean"},
        {"name": "lastRenovationDate", "type": "Edm.DateTimeOffset"},
        {"name": "rating", "type": "Edm.Int32"},
        {"name": "location", "type": "Edm.GeographyPoint"}
      ],
      "suggesters": [
        {
          "name": "sg",
          "searchMode": "analyzingInfixMatching",
          "sourceFields": ["hotelName"]
        }
      ]
    }

**Vastaus**

Onnistuneiden pyynnön: "201 luotu".

Vastauksen teksti sisältää oletusarvoisesti JSON indeksin määrityksen, joka on luotu. Jos `Prefer` pyynnön otsikossa on määritetty `return=minimal`, vastaus tekstissä on tyhjä ja success tilakoodin päivitetään "204 sisältöä ei ole" "201 luotu" sijaan. Tämä pätee riippumatta siitä, onko hakemiston luomiseen käytetty HYLLYTETTY tai viestin.

**Huomautuksia**

Tällä hetkellä on rajoitettu tuki indeksi rakenteen päivitykset. Rakenteen päivitykset, jotka vaativat uudelleenindeksointi kuten kenttätyypit eivät ole tuettuja. Aiemmin luotuja kenttiä ei voi muuttaa tai poistaa, vaikka uusia kenttiä voidaan lisätä aiemmin luotua indeksiä milloin tahansa. Kun uusi kenttä lisätään, kaikki aiemmin luotuja tiedostoja indeksissä on kentän null-arvon automaattisesti. Ei lisää tallennustilaa kulutettu, kunnes indeksi on lisätty uusia asiakirjoja.

<a name="Suggesters"></a>
## <a name="suggesters"></a>Suggesters

Azure hakutoiminnossa ehdotukset-ominaisuus on täydentävä tai automaattisen täydennyksen kyselyn ominaisuuksien mahdollisen vastauksen osittaisen merkkijono-syötteiden haku-ruutuun kirjoitettu hakusanojen luettelon. Olet luultavasti huomannut kyselyehdotukset kaupallisen Internetin hakukoneet käytettäessä: ".NET" kirjoittaminen Bing tuottaa ehdot luettelo ".NET 4.5", ".NET Framework 3.5" ja niin edelleen. Search-palvelun REST API käytettäessä toteuttaminen ehdotuksia mukautettu Azure Etsi sovellus edellyttää seuraavaa:

- Ota käyttöön ehdotuksia lisätään **suggester** rakentaminen indeksi, Anna nimi, Etsi-tilassa ja luettelon kentistä, jonka täydentävä käynnistetään. Esimerkiksi jos määrität "Kaupunki" lähdekenttä, kirjoittamalla "Kirj" osittaisen hakumerkkijono johtaa "Seattle", "Merenrannalla" ja "Seatac" (kaikkien kolmen ovat todellinen kaupunkien nimien) tarjota kyselyehdotukset käyttäjälle.

- Käynnistää ehdotuksia soittamalla [Ehdotuksia API](#Suggestions) sovelluksen koodissa. Osittainen hakumerkkijono lähetetään yleensä palvelun samalla, kun käyttäjä on kirjoittamalla hakukyselyn ja tämä API palauttaa joukon ehdotettu lauseita.

Tässä artikkelissa kerrotaan, miten voit määrittää **suggester**. Lue myös [Ehdotuksia API](#Suggestions) lisätietoja siitä, miten suggester käytetään.

**Käyttö**

`Suggesters`Luo indeksi- ja työ parhaiten, kun ehdotetaan tiettyjen asiakirjojen sijaan irtoaa termejä tai lauseita. Paras candidate-kentät ovat otsikoita, nimet ja muut suhteellisen lyhyen lauseita, jotka voit tunnistaa kohteen. Vähemmän tehokas ovat toistuvat kentät, kuten luokat ja tunnisteet tai pitkissä kenttien, kuten kuvaukset tai kommentteja kentät.

Osana indeksin määrityksen, voit lisätä yhden suggester, `suggesters` sivustokokoelman. Ominaisuudet, jotka määrittävät suggester ovat seuraavat:

- `name`: Suggester nimi. Suggester nimeä käytetään silloin, kun kutsumista `suggest` API.
- `searchMode`Strategia: etsitään candidate lauseita. Tällä hetkellä tueta vain tila on `analyzingInfixMatching`, joka suorittaa joustavia vastaavat lausekkeista alussa tai lauseiden keskelle.
- `sourceFields`: Vähintään yksi kentät, jotka ovat ehdotuksia sisällön lähteen luettelo. Vain-tyypin kenttiä `Edm.String` ja `Collection(Edm.String)` voi olla ehdotuksia lähteet. Vain kentät, joilla ei ole määritetty mukautettuja kieli-analyzer voidaan.

**Suggester Esimerkki**

Hakemiston suggester kuuluu. Voi olla vain yksi suggester `suggesters` nykyinen versio rinnalla kentät-sivustokokoelman sivustokokoelman ja `scoringProfiles`.

        {
          "name": "hotels",
          "fields": [
             . . .
           ],
          "suggesters": [
            {
            "name": "sg",
            "searchMode": "analyzingInfixMatching",
            "sourceFields: ["hotelName", "category"]
            }
          ],
          "scoringProfiles": [
             . . .
          ]
        }

> [AZURE.NOTE]  Jos olet käyttänyt Azure haun julkisen preview-versiosta `suggesters` Korvaa vanhat totuusarvo-ominaisuuden (`"suggestions": false`), joka tuettu etuliite ehdotuksia vain lyhyet merkkijonot (3-25 merkkiä). Sen Korvaava `suggesters`, tukee sisämerkintäinen vastaavat, joka etsii ehtoja vastaavat paremmin poikkeama hakumerkkijono virheet, alussa tai kentän sisältöä, keskelle. Yleisesti saatavilla versiosta lähtien, tämä on nyt vain toteuttamiseen ehdotuksia API. Vanhempi `suggestions` ominaisuus, joka otettiin käyttöön `api-version=2014-07-31-Preview` työkirja versio toimii, mutta ei ole toiminnassa- `2015-02-28` tai uudempi versio Azure haku.

<a name="UpdateIndex"></a>
## <a name="update-index"></a>Hakemiston päivittäminen

Voit päivittää aiemmin luotua indeksiä Azure hakutuloksia hyödyntämällä HTTP valitseminen pyynnön kuluessa. Päivitykset voivat olla uusia kenttiä lisääminen aiemmin luotuun rakenteeseen, muokkaamalla CORS asetukset ja muokkaamalla tulosmalli profiilit. Lisätietoja on kohdassa [Lisää näkyvissä pistemäärä profiilit](https://msdn.microsoft.com/library/azure/dn798928.aspx) . Voit myös määrittää hakemiston päivittäminen pyydettäessä URI nimi:

    PUT https://[search service url]/indexes/[index name]?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

**Tärkeä:** Indeksi rakenteen päivitykset tuki on rajoitettu toiminnoista, joita ei edellytä hakuindeksin muodostetaan uudelleen. Rakenteen päivitykset, jotka vaativat uudelleenindeksointi, kuten kenttätyyppejä, eivät ole tuettuja. Uudet kentät voidaan lisätä milloin tahansa, mutta ei voi muuttaa tai poistaa aiemmin luodut kentät. Sama koskee `suggesters`. Uudet kentät voidaan lisätä kentät lisätään suggester aikaa, mutta kenttiä ei voi poistaa `suggesters` ja aiemmin luotuja kenttiä ei voi lisätä `suggesters`.

Kun lisäät uuden kentän indeksin, kaikki aiemmin luotuja tiedostoja indeksissä on kentän null-arvon automaattisesti. Ei lisää tallennustilaa kulutettu, kunnes indeksi on lisätty uusia asiakirjoja.

**Pyyntö**

HTTPS tarvitaan kaikkia palvelupyyntöjä. **Päivitä hakemisto** -pyyntö muodostetaan käyttämällä HTTP valitseminen. Indeksinimi on HYLLYTETTY, URL-Osoitteen osa. Jos indeksi ei ole valittu, se luodaan. Jos indeksi on jo luotu, se päivitetään uuden määritelmän.

Indeksinimi on kirjoitettava pienillä kirjaimilla, aloittaminen kirjaimen tai numeron, on tai ei ole vinoviivaa pistettä ja voi olla pienempi kuin 128: aa merkkiä. Käynnistyksen Indeksinimi kirjaimen tai numeron jälkeen loput nimi voi sisältää kirjain, numero ja katkoviivat, kunhan viivat eivät ole peräkkäisiä.

`api-version=[string]`(pakollinen). Preview-versiosta on `api-version=2015-02-28-Preview`. Katso [Search-palvelun versiotietojen](http://msdn.microsoft.com/library/azure/dn864560.aspx) tiedot ja vaihtoehtoiset versiot.

**Pyydä otsikot**

Seuraavassa luettelossa kuvataan pakolliset ja valinnaiset pyynnön otsikot.

- `Content-Type`: Pakollinen. Määritä tähän`application/json`
- `api-key`: Pakollinen. `api-key` Todennetaan Search-palvelun pyynnön. Merkkijonoarvo, yksilöllisiä palvelusi on. **Päivitä hakemisto** -pyyntö on sisällettävä `api-key` otsikon määrittää järjestelmänvalvojan key-tuotetunnuksen (eikä kyselyn avain).

Sinun on myös muodostaa pyyntö URL-Osoitteen palvelunimi. Saat palvelunimi ja `api-key` service-koontinäytössä Azure-portaalissa. Katso Ohje sivuissa Siirtyminen [luominen Azure Search-palvelun portaalissa](search-create-service-portal.md) .

**Leipäteksti syntaksi**

Kun päivität aiemmin luotua indeksiä, tekstissä on sisällettävä alkuperäisen rakenteen määritys- ja lisäät uusia kenttiä sekä muokattu tulosmalli profiilit-suggesters ja CORS asetukset-mahdollisesti. Jos muokkaat ei tulosmalli-profiileista ja CORS asetukset, sinun on lisättävä alkuperäiset-hakemiston luontipäivämäärä. Yleensä paras kuvion päivitykset voidaan noutaa indeksin määrityksen GET kanssa, muokkaa sitä ja valitse päivittää sen käyttöön.

Hakemiston luomiseen käytetyt rakenteen syntaksi uudestaan tähän helpottamiseksi. Saat lisätietoja [Indeksin luominen](#CreateIndex) .

    {
      "name": (optional) "name_of_index",
      "fields": [
        {
          "name": "name_of_field",
          "type": "Edm.String | Collection(Edm.String) | Edm.Int32 | Edm.Int64 | Edm.Double | Edm.Boolean | Edm.DateTimeOffset | Edm.GeographyPoint",
          "searchable": true (default where applicable) | false (only Edm.String and Collection(Edm.String) fields can be searchable),
          "filterable": true (default) | false,
          "sortable": true (default where applicable) | false (Collection(Edm.String) fields cannot be sortable),
          "facetable": true (default where applicable) | false (Edm.GeographyPoint fields cannot be facetable),
          "key": true | false (default, only Edm.String fields can be keys),
          "retrievable": true (default) | false, 
          "analyzer": "name of the analyzer used for search and indexing", (only if 'searchAnalyzer' and 'indexAnalyzer' are not set)
          "searchAnalyzer": "name of the search analyzer", (only if 'indexAnalyzer' is set and 'analyzer' is not set)
          "indexAnalyzer": "name of the indexing analyzer" (only if 'searchAnalyzer' is set and 'analyzer' is not set)
        }
      ],
      "suggesters": [
        {
          "name": "name of suggester",
          "searchMode": "analyzingInfixMatching" (other modes may be added in the future),
          "sourceFields": ["field1", "field2", ...]
        }
      ],
      "scoringProfiles": [
        {
          "name": "name of scoring profile",
          "text": (optional, only applies to searchable fields) {
            "weights": {
              "searchable_field_name": relative_weight_value (positive numbers),
              ...
            }
          },
          "functions": (optional) [
            {
              "type": "magnitude | freshness | distance | tag",
              "boost": # (positive number used as multiplier for raw score != 1),
              "fieldName": "...",
              "interpolation": "constant | linear (default) | quadratic | logarithmic",
              "magnitude": {
                "boostingRangeStart": #,
                "boostingRangeEnd": #,
                "constantBoostBeyondRange": true | false (default)
              },
              "freshness": {
                "boostingDuration": "..." (value representing timespan leading to now over which boosting occurs)
              },
              "distance": {
                "referencePointParameter": "...", (parameter to be passed in queries to use as reference location, see "scoringParameter" for syntax details)
                "boostingDistance": # (the distance in kilometers from the reference location where the boosting range ends)
              },
              "tag": {
                "tagsParameter": "..." (parameter to be passed in queries to specify list of tags to compare against target field, see "scoringParameter" for syntax details)
              }
            }
          ],
          "functionAggregation": (optional, applies only when functions are specified)
            "sum (default) | average | minimum | maximum | firstMatching"
        }
      ],
      "analyzers":(optional)[ ... ],
      "charFilters":(optional)[ ... ],
      "tokenizers":(optional)[ ... ],
      "tokenFilters":(optional)[ ... ],
      "defaultScoringProfile": (optional) "...",
      "corsOptions": (optional) {
        "allowedOrigins": ["*"] | ["origin_1", "origin_2", ...],
        "maxAgeInSeconds": (optional) max_age_in_seconds (non-negative integer)
      }
    }


**Vastaus**

Onnistuneiden pyynnön: "204 sisältöä ei ole".

Oletusarvoisesti vastauksen teksti on tyhjä. Kuitenkin jos `Prefer` pyynnön otsikossa on määritetty `return=representation`-vastauksen teksti sisältää JSON indeksin määrityksen, joka on päivitetty. Tässä tapauksessa success tilakoodi on "200 OK".

**Päivitetään mukautetun analyzers indeksimääritys**

Kun analyzer, tokenizer, suojaustunnuksen suodattimen tai merkki-suodatin on määritetty, sitä ei voi muokata. Uusia voidaan lisätä aiemmin luotua indeksiä vain, jos `allowIndexDowntime` lippu on määritetty tosi indeksin päivitys-pyynnössä: 

`PUT https://[search service name].search.windows.net/indexes/[index name]?api-version=[api-version]&allowIndexDowntime=true`

Huomaa, että tämän toiminnon käyttöön indeksi offline-tilassa vähintään muutaman sekunnin, vuoksi, että indeksointi ja kyselyn pyynnöt epäonnistuu. Suorituskyky ja kirjoita käytettävyys indeksin voi olla alentunut useita minuutteja indeksin päivittämisen jälkeen tai pidempi erittäin suuri indeksit.

<a name="ListIndexes"></a>
## <a name="list-indexes"></a>Luettelon indeksit

**Luettelon indeksit** -toiminto palauttaa indeksit luettelon tällä hetkellä Azure Search-palvelun.

    GET https://[service name].search.windows.net/indexes?api-version=[api-version]
    api-key: [admin key]

**Pyyntö**

HTTPS tarvitaan kaikkia palvelupyyntöjä. **Luettelon indeksit** -pyyntö on rakennettava GET-menetelmällä.

`api-version=[string]`(pakollinen). Preview-versiosta on `api-version=2015-02-28-Preview`. Katso [Search-palvelun versiotietojen](http://msdn.microsoft.com/library/azure/dn864560.aspx) tiedot ja vaihtoehtoiset versiot.

**Pyydä otsikot**

Seuraavassa luettelossa kuvataan pakolliset ja valinnaiset pyynnön otsikot.

- `api-key`: Pakollinen. `api-key` Todennetaan Search-palvelun pyynnön. Merkkijonoarvo, yksilöllisiä palvelusi on. **Luettelon indeksit** -pyyntö on sisällettävä `api-key` asettaminen järjestelmänvalvoja-näppäintä (eikä kyselyn avain).

Sinun on myös muodostaa pyyntö URL-Osoitteen palvelunimi. Saat palvelunimi ja `api-key` service-koontinäytössä Azure-portaalissa. Katso Ohje sivuissa Siirtyminen [luominen Azure Search-palvelun portaalissa](search-create-service-portal.md) .

**Pyydä tekstissä**

Ei mitään.

**Vastaus**

Tilakoodin: 200 OK palautetaan onnistuneen vastauksen odottaminen.

Tässä on esimerkki-vastauksen tekstissä:

    {
      "value": [
        {
          "name": "Books",
          "fields": [
            {"name": "ISBN", ...},
            ...
          ]
        },
        {
          "name": "Games",
          ...
        },
        ...
      ]
    }

Huomaa, että voit suodattaa vastauksen alaspäin käyttämällä ominaisuudet. Esimerkiksi jos haluat vain nimiluettelon indeksin, käytä OData `$select` kyselyn vaihtoehto:

    GET /indexes?api-version=2015-02-28-Preview&$select=name

Tässä tapauksessa edellä olevassa esimerkissä vastaus näkyy seuraavasti:

    {
      "value": [
        {"name": "Books"},
        {"name": "Games"},
        ...
      ]
    }

Tämä on hyödyllinen tapa tallentaa kaistanleveyden, jos sinulla on paljon indeksit Search-palvelun.

<a name="GetIndex"></a>
## <a name="get-index"></a>Hae hakemisto

**Hae indeksi** -toiminto saa indeksin määrityksen Azure haku.

    GET https://[service name].search.windows.net/indexes/[index name]?api-version=[api-version]
    api-key: [admin key]

**Pyyntö**

HTTPS tarvitaan palvelupyyntöjä. **Hae indeksi** -pyyntö on rakennettava GET-menetelmällä.

[Indeksinimi] URI-pyynnössä määrittää, mitkä indeksi palauttaa indeksit-sivustokokoelman.

`api-version=[string]`(pakollinen). Preview-versiosta on `api-version=2015-02-28-Preview`. Katso [Search-palvelun versiotietojen](http://msdn.microsoft.com/library/azure/dn864560.aspx) tiedot ja vaihtoehtoiset versiot.

**Pyydä otsikot**

Seuraavassa luettelossa kuvataan pakolliset ja valinnaiset pyynnön otsikot.

- `api-key`: Mitä `api-key` todennetaan Search-palvelun pyynnön. Merkkijonoarvo, yksilöllisiä palvelusi on. **Hae indeksi** -pyyntö on sisällettävä `api-key` asettaminen järjestelmänvalvoja-näppäintä (eikä kyselyn avain).

Sinun on myös muodostaa pyyntö URL-Osoitteen palvelunimi. Saat palvelunimi ja `api-key` service-koontinäytössä Azure-portaalissa. Katso Ohje sivuissa Siirtyminen [luominen Azure Search-palvelun portaalissa](search-create-service-portal.md) .

**Pyydä tekstissä**

Ei mitään.

**Vastaus**

Tilakoodin: 200 OK palautetaan onnistuneen vastauksen odottaminen.

Katso esimerkkiä JSON [luominen ja päivittäminen indeksin](#CreateUpdateIndexExample) Esimerkki vastauksen salataan.

<a name="DeleteIndex"></a>
## <a name="delete-index"></a>Indeksin poistaminen

**Indeksin poistaminen** -toiminto poistaa Azure-hakupalvelun indeksin ja liittyvät asiakirjat. Saat Indeksinimi Azure-portaalissa palvelun Raporttinäkymät-ikkunan tai Ohjelmointirajapinnan. Katso lisätietoja [Luettelon indeksit](#ListIndexes) .

    DELETE https://[service name].search.windows.net/indexes/[index name]?api-version=[api-version]
    api-key: [admin key]

**Pyyntö**

HTTPS tarvitaan palvelupyyntöjä. **Indeksin poistaminen** pyyntö on rakennettava DELETE-menetelmällä.

[Indeksinimi] URI-pyynnössä määrittää mitä indeksin indeksit kokoelmasta.

`api-version=[string]`(pakollinen). Preview-versiosta on `api-version=2015-02-28-Preview`. Katso [Search-palvelun versiotietojen](http://msdn.microsoft.com/library/azure/dn864560.aspx) tiedot ja vaihtoehtoiset versiot.

**Pyydä otsikot**

Seuraavassa luettelossa kuvataan pakolliset ja valinnaiset pyynnön otsikot.

- `api-key`: Pakollinen. `api-key` Todennetaan Search-palvelun pyynnön. On merkkijonoarvo, yksilöllisiä palvelun URL-osoite. **Indeksin poistaminen** pyynnön on sisällettävä `api-key` otsikon määrittää järjestelmänvalvojan key-tuotetunnuksen (eikä kyselyn avain).

Sinun on myös muodostaa pyyntö URL-Osoitteen palvelunimi. Saat palvelunimi ja `api-key` service-koontinäytössä Azure-portaalissa. Katso Ohje sivuissa Siirtyminen [luominen Azure Search-palvelun portaalissa](search-create-service-portal.md) .

**Pyydä tekstissä**

Ei mitään.

**Vastaus**

Tilakoodin: 204 ei sisältöä palautetaan onnistuneen vastauksen odottaminen.

<a name="GetIndexStats"></a>
## <a name="get-index-statistics"></a>Hae tilastot

**Hae tilastot** -toiminto palauttaa Azure haun asiakirjan nykyisen hakemiston sekä tallennustilan määrä.

    GET https://[service name].search.windows.net/indexes/[index name]/stats?api-version=[api-version]
    api-key: [admin key]

> [AZURE.NOTE] Asiakirjan Laske- ja tallennustilaa koko tilastoja kerätään muutaman minuutin välein, ei reaaliajassa. Vuoksi tämä API palauttama tilastotiedot ei ilmaise muutokset aiheutuvat viimeisimmät indeksoinnin toimintoja.

**Pyyntö**

HTTPS tarvitaan kaikki palvelujen pyynnöt. **Hae tilastot** pyyntö on rakennettava GET-menetelmällä.

[Indeksinimi] URI-pyynnössä kertoo palvelu palauttaa indeksointitilastoja määritetty indeksi.

`api-version=[string]`(pakollinen). Preview-versiosta on `api-version=2015-02-28-Preview`. Katso [Search-palvelun versiotietojen](http://msdn.microsoft.com/library/azure/dn864560.aspx) tiedot ja vaihtoehtoiset versiot.


**Pyydä otsikot**

Seuraavassa luettelossa kuvataan pakolliset ja valinnaiset pyynnön otsikot.

- `api-key`: Mitä `api-key` todennetaan Search-palvelun pyynnön. Merkkijonoarvo, yksilöllisiä palvelusi on. **Hae tilastot** pyynnön on sisällettävä `api-key` asettaminen järjestelmänvalvoja-näppäintä (eikä kyselyn avain).

Sinun on myös muodostaa pyyntö URL-Osoitteen palvelunimi. Saat palvelunimi ja `api-key` service-koontinäytössä Azure-portaalissa. Katso Ohje sivuissa Siirtyminen [luominen Azure Search-palvelun portaalissa](search-create-service-portal.md) .

**Pyydä tekstissä**

Ei mitään.

**Vastaus**

Tilakoodin: 200 OK palautetaan onnistuneen vastauksen odottaminen.

Vastauksen teksti on seuraavanlainen:

    {
      "documentCount": number,
      "storageSize": number (size of the index in bytes)
    }

<a name="TestAnalyzer"></a>
## <a name="test-analyzer"></a>Testaa analysoiminen

**Analysoi API** näyttää, miten analyzer jakaa tekstin tunnusten.

    POST https://[service name].search.windows.net/indexes/[index name]/analyze?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

**Pyyntö**

HTTPS tarvitaan kaikki palvelujen pyynnöt. **Analysoi API** -pyyntö on rakennettava POST-menetelmällä.

`api-version=[string]`(pakollinen). Preview-versiosta on `api-version=2015-02-28-Preview`. Katso [Search-palvelun versiotietojen](http://msdn.microsoft.com/library/azure/dn864560.aspx) tiedot ja vaihtoehtoiset versiot.


**Pyydä otsikot**

Seuraavassa luettelossa kuvataan pakolliset ja valinnaiset pyynnön otsikot.

- `api-key`: Mitä `api-key` todennetaan Search-palvelun pyynnön. Merkkijonoarvo, yksilöllisiä palvelusi on. **Analysoi API** -pyyntö on sisällettävä `api-key` asettaminen järjestelmänvalvoja-näppäintä (eikä kyselyn avain).

Sinun on myös Indeksinimi ja palvelunimi muodostaa pyyntö URL-osoite. Saat palvelunimi ja `api-key` service-koontinäytössä Azure-portaalissa. Katso Ohje sivuissa Siirtyminen [luominen Azure Search-palvelun portaalissa](search-create-service-portal.md) .

**Pyydä tekstissä**

    {
      "text": "Text to analyze",
      "analyzer": "analyzer_name"
    }

tai

    {
      "text": "Text to analyze",
      "tokenizer": "tokenizer_name",
      "tokenFilters": (optional) [ "token_filter_name" ],
      "charFilters": (optional) [ "char_filter_name" ]
    }

`analyzer_name`, `tokenizer_name`, `token_filter_name` Ja `char_filter_name` on oltava kelvolliset nimet valmista tai mukautettua analyzers, tokenizers, suojaustunnuksen suodattimet ja hakemiston merkki suodattimet. Lisätietoja Sanastollinen analyysin prosessin artikkelissa [Azure haun analyyseja](https://aka.ms/azsanalysis).

**Vastaus**

Tilakoodin: 200 OK palautetaan onnistuneen vastauksen odottaminen.

Vastauksen teksti on seuraavanlainen:

    {
      "tokens": [
        {
          "token": string (token),
          "startOffset": number (index of the first character of the token),
          "endOffset": number (index of the last character of the token),
          "position": number (position of the token in the input text)
        },
        ...
      ]
    }

**Analysoi API-Esimerkki**

**Pyyntö**

    {
      "text": "Text to analyze",
      "analyzer": "standard"
    }

**Vastaus**

    {
      "tokens": [
        {
          "token": "text",
          "startOffset": 0,
          "endOffset": 4,
          "position": 0
        },
        {
          "token": "to",
          "startOffset": 5,
          "endOffset": 7,
          "position": 1
        },
        {
          "token": "analyze",
          "startOffset": 8,
          "endOffset": 15,
          "position": 2
        }
      ]
    }

________________________________________
<a name="DocOps"></a>
## <a name="document-operations"></a>Asiakirjan toiminnot

Azure hakutoiminnossa indeksin pilveen tallennettujen ja täytetty JSON-tiedostot, jotka olet ladannut palvelun avulla. Kaikista tiedostoista, jotka olet ladannut kuuluvat haun tietojen corpus. Kentät, joista osa on tokenized hakusanojen kyselyjä, kun ne on ladattu-tiedostoissa. `/docs` URL-Osoitteen segmenttiä Azure haun Ohjelmointirajapinta edustaa indeksin asiakirjojen kokoelmaa. Suorittaa esimerkiksi ladataan sivustokokoelman kaikki toiminnot, yhdistäminen ja poistaminen sekä kyselyt asiakirjojen Ota sijoittaa yhden indeksin kontekstissa niin URL-osoitteet, jossa avautuu aina nämä toiminnot `/indexes/[index name]/docs` annettu indeksin nimen.

Sovelluksen-koodi on luotava joko JSON tiedostojen lataaminen Azure haun tai [indeksointitoiminto](https://msdn.microsoft.com/library/dn946891.aspx) voit ladata tiedostoja, jos tietolähde on Azure SQL-tietokanta tai DocumentDB. Yleensä indeksit täytetään yksittäisen tietojoukko, joka saadaan.

Voit suunnitella tulisi ottaa yhteen asiakirjaan kunkin kohteen, jonka haluat etsiä. Elokuvan vuokraus-sovellus voi olla / elokuva asiakirjaan storefront sovellus voi olla yksi asiakirjan per tuote courseware-tiedoston online-sovellus voi olla yhden asiakirjan kurssin kohti, Oheistiedot yritykselle saattaa on yksi asiakirja kunkin academic paperin niiden säilöön, ja niin edelleen.

Asiakirjojen koostuvat yhden tai useamman kentän. Kenttiä voi sisältää tekstiä, joka on tokenized Azure haku hakusanojen kyselyjä sekä muita tokenized tai muut kuin tekstiarvot, joita voi käyttää suodattimia tai tulosmalli profiilit. Indeksi-rakenne määritetään nimet, tietotyypit ja tukevat kunkin kentän hakuominaisuudet. Toisen kentän indeksin kunkin rakenteen on nimettävä tunnuksen ja kunkin asiakirjan on oltava, joka yksilöi indeksissä asiakirjan tunnus-kentän arvo. Kaikki muut asiakirjakentät ovat valinnaisia ja käytetään oletusarvon null-arvon, jos vasemmalle määrittämätön. Huomaa, että null-arvoja ei oteta hakuindeksin tilaa.

Ennen kuin voit ladata tiedostoja, on olet jo luonut indeksi-palvelusta. Katso lisätietoja tätä ensimmäistä vaihetta [Indeksin luominen](#CreateIndex) .

<a name="AddOrUpdateDocuments"></a>
## <a name="add-update-or-delete-documents"></a>Lisää, Päivitä tai tiedostojen poistaminen

Voit ladata, indeksi on määritetty käyttämällä HTTP POST yhdistäminen, Yhdistä tai lataa tai poista tiedostoja. Paljon päivitykset jonottaminen asiakirjojen ylöspäin (1000 tiedostojen erää kohti) tai 16 Mt erä kohden on suositeltavaa.

    POST https://[service name].search.windows.net/indexes/[index name]/docs/index?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

**Pyyntö**

HTTPS tarvitaan kaikkia palvelupyyntöjä. Voit ladata, indeksi on määritetty käyttämällä HTTP POST yhdistäminen, Yhdistä tai lataa tai poista tiedostoja.

Pyyntö URI sisältää [Indeksinimi] määrittäminen mitä indeksi voivat lähettää tiedostoja. Voit julkaista vain yhden indeksin asiakirjojen kerrallaan.

`api-version=[string]`(pakollinen). Preview-versiosta on `api-version=2015-02-28-Preview`. Katso [Search-palvelun versiotietojen](http://msdn.microsoft.com/library/azure/dn864560.aspx) tiedot ja vaihtoehtoiset versiot.

**Pyydä otsikot**

Seuraavassa luettelossa kuvataan pakolliset ja valinnaiset pyynnön otsikot.

- `Content-Type`: Pakollinen. Määritä tähän`application/json`
- `api-key`: Pakollinen. `api-key` Todennetaan Search-palvelun pyynnön. Merkkijonoarvo, yksilöllisiä palvelusi on. **Lisää tiedostoja** pyynnön on sisällettävä `api-key` otsikon määrittää järjestelmänvalvojan key-tuotetunnuksen (eikä kyselyn avain).

Sinun on myös muodostaa pyyntö URL-Osoitteen palvelunimi. Saat palvelunimi ja `api-key` service-koontinäytössä Azure-portaalissa. Katso Ohje sivuissa Siirtyminen [luominen Azure Search-palvelun portaalissa](search-create-service-portal.md) .

**Pyydä tekstissä**

Pyynnön leipätekstissä vähintään yksi asiakirjoja voi indeksoida. Asiakirjojen tunnistetaan yksilöivä tunnus. Jokaiseen tiedostoon liittyy toiminnon: lataa, Yhdistä, mergeOrUpload tai poista. Lataa pyynnöt on oltava tiedoston tiedot joukkona avain/arvo-pareina.

    {
      "value": [
        {
          "@search.action": "upload (default) | merge | mergeOrUpload | delete",
          "key_field_name": "unique_key_of_document", (key/value pair for key field from index schema)
          "field_name": field_value (key/value pairs matching index schema)
            ...
        },
        ...
      ]
    }

> [AZURE.NOTE] Asiakirjan näppäimet voi sisältää vain kirjaimia, numeroita, katkoviivat ("-"), alaviivoja ("_") sekä yhtäläisyysmerkkiä ("="). Lisätietoja on artikkelissa [Naming säännöt](https://msdn.microsoft.com/library/azure/dn857353.aspx).

**Asiakirjatoimet**

- `upload`: Lataa toiminnon on samankaltainen kuin "upsert" mihin tiedosto lisätään, jos se on uusia ja päivittää tai korvata, jos se on olemassa. Huomaa, että kaikki kentät korvataan päivitys-muodossa.
- `merge`: Yhdistäminen päivittää määritettyihin kenttiin aiemmin luotu tiedosto. Jos sitä ei ole, yhdistäminen epäonnistuu. Määrittämäsi yhdistämiseen kentän korvaa olemassa olevan kentän asiakirjassa. Tämä vaihtoehto sisältää-tyypin kenttiä `Collection(Edm.String)`. Jos asiakirjassa on kentän "tunnisteet" arvona esimerkiksi `["budget"]` ja arvona yhdistämisen suorittaminen `["economy", "pool"]` "tunnisteiden", "tunnisteet"-kenttään viimeinen arvo on `["economy", "pool"]`. Se **ei** voi `["budget", "economy", "pool"]`.
- `mergeOrUpload`:-funktio vastaa `merge` indeksissä aiemmin luodun asiakirjan tietyn-näppäimen kanssa. Jos sitä ei ole se toimii kuten `upload` ja uuden asiakirjan.
- `delete`: Poistaminen poistaa määritetyn asiakirjan pois indeksistä. Huomaa, että kaikki kentät määritettyyn `delete` toiminto kuin avaimen kentän ohitetaan. Jos haluat poistaa yksittäisen kentän asiakirjasta, käytä `merge` sijaan ja yksinkertaisesti määrittää kentän erikseen, `null`.

**Vastaus**

Tilakoodin 200 palautetaan (OK) onnistuneen palaute, mikä tarkoittaa, että kaikki kohteet on indeksoitu onnistuneesti. Tämä ilmaistaan `status` ominaisuus on määritetty tosi, jotta kaikki kohteet, myös nimellä `statusCode` määritetään 201 (varten juuri lataamasi tiedostot) tai (yhdistettyjä tai poistettujen tiedostojen) 200 ominaisuus:

    {
      "value": [
        {
          "key": "unique_key_of_new_document",
          "status": true,
          "errorMessage": null,
          "statusCode": 201
        },
        {
          "key": "unique_key_of_merged_document",
          "status": true,
          "errorMessage": null,
          "statusCode": 200
        },
        {
          "key": "unique_key_of_deleted_document",
          "status": true,
          "errorMessage": null,
          "statusCode": 200
        }
      ]
    }  

Tilakoodin 207 (usean tila) palautetaan, kun vähintään yksi kohde ei onnistunut indeksoitu. Kohteet, joita ei ole indeksoitu on `status` -kentän arvoksi false. `errorMessage` Ja `statusCode` ominaisuudet kertovat indeksoinnin virheen syy:

    {
      "value": [
        {
          "key": "unique_key_of_document_1",
          "status": false,
          "errorMessage": "The search service is too busy to process this document. Please try again later.",
          "statusCode": 503
        },
        {
          "key": "unique_key_of_document_2",
          "status": false,
          "errorMessage": "Document not found.",
          "statusCode": 404
        },
        {
          "key": "unique_key_of_document_3",
          "status": false,
          "errorMessage": "Index is temporarily unavailable because it was updated with the 'allowIndexDowntime' flag set to 'true'. Please try again later.",
          "statusCode": 422
        }
      ]
    }  

Seuraavassa taulukossa kuvataan eri asiakirjan kohti tilakoodit, joka palautetaan vastauksessa. Huomaa, että jotkin osoittaa pyynnön itse ongelmia samalla, kun muut Määritä tilapäinen virhetilanteita. Jälkimmäinen viiveen jälkeen yritä uudelleen.

<table style="font-size:12">
    <tr>
        <th>Tilakoodin</th>
        <th>Merkitys</th>
        <th>Uudelleenyritettävä</th>
        <th>Huomautuksia</th>
    </tr>
    <tr>
        <td>200</td>
        <td>Asiakirjan onnistuneesti on muokattu tai poistettu.</td>
        <td>puuttuu</td>
        <td>Poista-toiminnot ovat <a href="https://en.wikipedia.org/wiki/Idempotence">idempotent</a>. Vaikka asiakirja-näppäin ei ole indeksin, yritetään poistotoiminnon, jonka avain johtaa 200 tilakoodi.</td>
    </tr>
    <tr>
        <td>201</td>
        <td>Asiakirja on luotu.</td>
        <td>puuttuu</td>
        <td></td>
    </tr>
    <tr>
        <td>400</td>
        <td>Asiakirjaan, joka estää indeksointi oli virhe.</td>
        <td>Ei</td>
        <td>Vastauksessa virhesanoma ilmoittaa Miksei vaan asiakirjan.</td>
    </tr>
    <tr>
        <td>404</td>
        <td>Asiakirjan ei voi yhdistää määritetyn avaimen ei ole indeksi.</td>
        <td>Ei</td>
        <td>Tämä virhe ei ilmetä latauksia, koska ne luoda uusia asiakirjoja, ja sitä ei tapahdu, poistaa, koska ne ovat <a href="https://en.wikipedia.org/wiki/Idempotence">idempotent</a>.</td>
    </tr>
    <tr>
        <td>409</td>
        <td>Version ristiriita ilmoittavaan indeksoida tiedostoa.</td>
        <td>Kyllä</td>
        <td>Näin voi käydä, kun yrität indeksoida samaa tiedostoa samanaikaisesti useita kertoja.</td>
    </tr>
    <tr>
        <td>422</td>
        <td>Indeksi on tilapäisesti poissa käytöstä, koska se on päivitetty "allowIndexDowntime" lippu "true".</td>
        <td>Kyllä</td>
        <td></td>
    </tr>
    <tr>
        <td>503</td>
        <td>Search-palvelun ole tilapäisesti käytettävissä mahdollisesti kuormitus vuoksi.</td>
        <td>Kyllä</td>
        <td>Koodi on odotettava, ennen kuin tässä tapauksessa tai voit julkisuuteen pidentäminen palvelu ei ole käytettävissä.</td>
    </tr>
</table> 

**Huomautus**: Jos asiakas-koodin kohtaa usein 207 vastauksen, tämä voi johtua siitä, että järjestelmä on kohdassa Lataa. Voit vahvistaa valitsemalla `statusCode` 503-ominaisuus. Jos näin on, on suositeltavaa ***rajoittimen indeksoinnin pyynnöt***. Muussa tapauksessa indeksoinnin liikenne ei subside, jos järjestelmä voi käynnistää hylkäämällä kaikki palvelupyynnöt 503 virheitä.

429 tilakoodin indeksiä kohti tiedostojen määrän kiintiö on ylittynyt. Luo uusi indeksi tai päivittäminen suurempi kapasiteetin rajoitukset.

**Esimerkki:**

    {
      "value": [
        {
          "@search.action": "upload",
          "hotelId": "1",
          "baseRate": 199.0,
          "description": "Best hotel in town",
          "description_fr": "Meilleur hôtel en ville",
          "hotelName": "Fancy Stay",
          "category": "Luxury",
          "tags": ["pool", "view", "wifi", "concierge"],
          "parkingIncluded": false,
          "smokingAllowed": false,
          "lastRenovationDate": "2010-06-27T00:00:00Z",
          "rating": 5,
          "location": { "type": "Point", "coordinates": [-122.131577, 47.678581] }
        },
        {
          "@search.action": "upload",
          "hotelId": "2",
          "baseRate": 79.99,
          "description": "Cheapest hotel in town",
          "description_fr": "Hôtel le moins cher en ville",
          "hotelName": "Roach Motel",
          "category": "Budget",
          "tags": ["motel", "budget"],
          "parkingIncluded": true,
          "smokingAllowed": true,
          "lastRenovationDate": "1982-04-28T00:00:00Z",
          "rating": 1,
          "location": { "type": "Point", "coordinates": [-122.131577, 49.678581] }
        },
        {
          "@search.action": "merge",
          "hotelId": "3",
          "baseRate": 279.99,
          "description": "Surprisingly expensive",
          "lastRenovationDate": null
        },
        {
          "@search.action": "delete",
          "hotelId": "4"
        }
      ]
    }
________________________________________
<a name="SearchDocs"></a>
## <a name="search-documents"></a>Etsi asiakirjoista

**Hakutoiminto, jossa** on myönnetty kuin GET tai POST pyyntö ja parametrit, jotka antavat ehtoja vastaavia asiakirjoja valitsemiseen.

    GET https://[service name].search.windows.net/indexes/[index name]/docs?[query parameters]
    api-key: [admin or query key]

    POST https://[service name].search.windows.net/indexes/[index name]/docs/search?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin or query key]

**Kirjaa käyttäminen GET sijaan**

Kun käytät HTTP GET Soita **haun** Ohjelmointirajapinta, sinun täytyy huomioon, että pyyntö URL-Osoitteen pituus voi olla enintään 8 KB. Tämä on tarpeeksi yleensä useimpien sovellusten. Jotkin sovellukset tuottaa kuitenkin erittäin suuri kyselyt tai OData-suodatinlausekkeiden. Näiden sovellusten käyttämällä HTTP POST on kätevä vaihtoehto, koska se sallii suurempi kuin GET kyselyjen ja suodattimet. Kirjaa ehtoja tai kyselyn lauseet on rajoittava tekijä ei raaka kyselyn pyynnön kokorajoitus viestin ollessa 16 Megatavun koon.

> [AZURE.NOTE] Vaikka kirjaa pyynnön kokorajoituksen on erittäin suuri, hakukyselyt ja suodatinlausekkeiden ei voi olla satunnaisesti monimutkaisia. [Lucene kyselysyntaksia](https://msdn.microsoft.com/library/mt589323.aspx) ja [OData-lausekkeiden syntaksista](https://msdn.microsoft.com/library/dn798921.aspx) Saat lisätietoja haun kyselyn ja Suodata monimutkaisuuden rajoitukset.

**Pyyntö**

HTTPS tarvitaan palvelupyyntöjä. **Etsi** -pyyntö on rakennettava GET tai POST tavoilla.

Pyynnön URI määrittää, mitkä kaikki tiedostot, jotka vastaavat parametrit kyselylle hakemisto. Parametrit on määritetty kyselymerkkijonon kyseessä GET-pyyntöjä ja pyynnössä kyseessä viestin tekstiosaan pyytää.

Paras käytäntö GET-pyyntöjä luotaessa muista tietyn kyselyparametrit [URL-Osoitteen koodata](https://msdn.microsoft.com/library/system.uri.escapedatastring.aspx) soitettaessa REST-Ohjelmointirajapinnalla suoraan. **Etsi** -toimintoja mukaan lukien:

- `$filter`
- `facet`
- `highlightPreTag`
- `highlightPostTag`
- `search`
- `moreLikeThis`

URL-koodausta suositellaan vain yllä kyselyparametrit. Jos olet vahingossa URL-Osoitteen koodata koko kyselymerkkijonon (kaikki jälkeen?)-pyynnöt vaihtuvat.

URL-koodaus on myös vain tarvittavat suoraan käyttämällä HAE REST-Ohjelmointirajapinnalla soitettaessa. Ei ole URL-koodaus on tarpeen soitettaessa **haun** avulla VIESTIIN tai käyttämällä [.NET asiakkaan kirjastoon](https://msdn.microsoft.com/library/dn951165.aspx), joka käsittelee URL-koodausta puolestasi.

<a name="SearchQueryParameters"></a>
**Kyselyparametrit**

**Etsi** hyväksyy useita parametreja, joiden kyselyehdot ja määritä myös haun toiminta. Voit antaa nämä parametrit URL-osoitteen kyselyn merkkijono soitettaessa **haun** kautta GET ja JSON ominaisuuksina pyynnön tekstissä soitettaessa **haun** kautta viesti. Joidenkin parametrien syntaksi on hieman erilainen GET ja kirjaa välillä. Nämä erot on merkitty tapauksen alla:

`search=[string]`(valinnainen) - Etsittävä teksti. Kaikki `searchable` kentät etsitään oletusarvoisesti, ellei `searchFields` on määritetty. Etsittäessä `searchable` kentät, itse etsittävä teksti on tokenized, jotta useita ehtoja voidaan erottaa toisistaan tyhjää aluetta (esimerkiksi: `search=hello world`). Vastaa mitä tahansa, käytä `*` (tämä on hyvä totuusarvo suodattimen kyselyjen). Pois jättäminen tämä parametri on samoin kuin se asettaminen `*`. Katso [Yksinkertaisen kyselyn syntaksi](https://msdn.microsoft.com/library/dn798920.aspx) haun syntaksista yksityiskohtia.

  - **Huomautus**: tuloksia voidaan joskus surprising, kun kysely suoritetaan päälle `searchable` kentät. Tokenizer sisältää logiikan käsittelemään tapauksissa yhteiset englantilaisen tekstin, kuten heittomerkit, pilkuilla numerot jne. Esimerkiksi `search=123,456` yksittäisen termin 123,456 sijaan yksittäisiä termejä 123 ja 456, koska pilkkuja käytetään tuhat erottimiksi suuren englanniksi vastaavat. Tästä syystä suosittelemme käyttää tyhjää aluetta välimerkit sijaan erottaa käyttöoikeussopimuksen ehdot `search` parametria.

`searchMode=any|all`(valinnainen, oletusarvo on `any`) - onko jonkin tai kaikki hakuehdot on vastannut vastineena asiakirjan laskemiseen.

`searchFields=[string]`(valinnainen) - CSV-kentässä määritetyn tekstin Etsi nimien luettelossa. Kohdekentät on merkitty `searchable`.

`queryType=simple|full`(valinnainen, oletusarvo on `simple`) - silloin, kun "perushaun" tekstin tulkitaan yksinkertaisen kyselyn kielellä, joka sallii merkkejä, kuten +, * ja "". Kyselyjen arvioidaan kaikkien hakukenttiä yli (tai kentät, joka on `searchFields`) oletusarvoisesti jokaiseen asiakirjaan. Kun kyselytyypin on määritetty `full` hakuteksti tulkitaan Lucene-kyselykieltä, joka sallii kentän kielikohtaiset ja painotettu hakujen avulla. [Yksinkertaisen kyselyn syntaksin](https://msdn.microsoft.com/library/dn798920.aspx) ja [Lucene kyselysyntaksia](https://msdn.microsoft.com/library/mt589323.aspx) saat yksityiskohtia, valitse haku-muodot. 
 
> [AZURE.NOTE] Lucene, koska $filter joka tarjoaa samankaltaisia toimintoja ei tueta kyselykielen haun alue.

`moreLikeThis=[key]`(valinnainen) **Tärkeä:** Tämä ominaisuus on käytettävissä vain `2015-02-28-Preview`. Tämä asetus ei voi käyttää kyselyä, joka sisältää tekstin haku-parametrin `search=[string]`. `moreLikeThis` Parametrin löytää tiedostot, jotka muistuttavat asiakirjan asiakirjan-näppäintä. Kun hakupyyntöä muodostetaan `moreLikeThis`, hakusanojen luettelo luodaan korkojakso ja rarity lähdetiedostossa ehtojen perusteella. Ehdot käytetään sitten pyynnön tekemiseen. Oletusarvon mukaan kaikkien sisältö `searchable` kentät pidetään, ellei `searchFields` rajoitetaan haettavat kentät.  

`$skip=#`(valinnainen) – voit ohittaa; hakutulosten määrää Ei voi olla suurempi kuin 100 000. Jos sinun tarvitsee tiedostojen skannaaminen järjestyksessä, etkä voi käyttää `$skip` vuoksi tästä rajoituksesta kannattaa käyttää `$orderby` kokonaan vyöhykkeen avaimen ja `$filter` alueen kyselyn sen sijaan.

> [AZURE.NOTE] **Haun** avulla kirjaa soitettaessa tämä parametri on nimeltään `skip` sijaan `$skip`.

`$top=#`(valinnainen) - hakutulosten noutamiseen lukumäärä. Tämä voi käyttää yhdessä `$skip` toteuttamisesta asiakkaan sivutus hakutulokset.

> [AZURE.NOTE] **Haun** avulla kirjaa soitettaessa tämä parametri on nimeltään `top` sijaan `$top`.

`$count=true|false`(valinnainen, oletusarvo on `false`)-määrittää hakeaksesi tuloksia kokonaismäärä. Tämä on kaikista tiedostoista, jotka vastaavat `search` ja `$filter` parametreja, ohittaa `$top` ja `$skip`. Tämä arvo `true` voivat vaikuttaa suorituskykyyn. Huomaa, että määrä, palautetaan arvion.

> [AZURE.NOTE] **Haun** avulla kirjaa soitettaessa tämä parametri on nimeltään `count` sijaan `$count`.

`$orderby=[string]`(valinnainen) - lausekeluettelon CSV-tulosten mukaan lajittelu. Kunkin lauseke voi olla kenttänimen tai puhelun `geo.distance()` funktio. Kunkin lausekkeen perään `asc` , merkitty Nouseva, ja `desc` osoittamaan laskeva. Oletusarvo on nousevaan järjestykseen. TIES jaetaan asiakirjojen vastine-tulosten mukaan. Jos `$orderby` on määritetty, lajittelujärjestys on laskeva asiakirjan vastine tuloksen mukaan. 32 lausekkeita enimmäismäärä on `$orderby`.

> [AZURE.NOTE] **Haun** avulla kirjaa soitettaessa tämä parametri on nimeltään `orderby` sijaan `$orderby`.

`$select=[string]`(valinnainen) - luettelon noutaminen CSV-kentistä. Jos Määrittämätön, kaikki kentät, jotka on merkitty enää rakenteessa käytettävissä sisällytetään. Voit pyytää kaikki kentät myös erikseen parametrin arvoksi `*`.

> [AZURE.NOTE] **Haun** avulla kirjaa soitettaessa tämä parametri on nimeltään `select` sijaan `$select`.

`facet=[string]`(nolla tai enemmän) - kentän pinta mukaan. Voit myös merkkijonon voi olla parametrit mukauttamiseen faceting valuuttamäärän CSV- `name:value` parit. Kelvollinen parametrit ovat seuraavat:

- `count`(max määrä voi olla ehdoin, oletusarvo on 10). Ei ole käytettävissä, mutta suuremmat arvot maksamaan vastaavan suorituskykyä huomattavasti, erityisesti, jos kohdistettua-kentässä on runsaasti yksilöllinen ehdot.
  - Esimerkki: `facet=category,count:5` saa viisi luokat pinta tulokset.  
  - **Huomautus**: Jos `count` parametri on pienempi kuin yksilöllinen kausien määrä, tulokset eivät välttämättä ole oikein. Tämä on faceting kyselyjen jakautuu shards tavalla. Lisääntyvien `count` kasvaa yleensä tarkkuutta termin laskelmia, mutta suorituskyvyn kustannus.
- `sort`(jokin `count` voit lajitella *laskevaan järjestykseen* , valitsemalla Laske- `-count` voit lajitella *nousevaan* määrä, mukaan `value` voit lajitella arvon mukaan *nousevaan* tai `-value` voit lajitella arvon mukaan *laskevaan järjestykseen* )
  - Esimerkki: `facet=category,count:3,sort:count` saa ensimmäiset kolme luokat pinta tulokset laskevaan järjestykseen kunkin kaupungin nimen tiedostojen määrän mukaan. Esimerkiksi jos yläreunan kolmeen luokkaan ovat budjetin, oppaalla ja Luxury, budjetin on 5 osumaa, oppaalla on 6 ja Luxury on 4, valitse uudelleenkäytettävät on järjestyksessä oppaalla, budjetin, Luxury.
  - Esimerkki: `facet=rating,sort:-value` tuottaa kaikki mahdolliset luokitukset laskevaan järjestykseen arvolla uudelleenkäytettävät. Esimerkiksi jos luokitukset ovat 1-5, uudelleenkäytettävät, järjestetään 5, 4, 3, 2, 1, riippumatta siitä, kuinka monta asiakirjaa vastaavat kunkin luokitus.
- `values`(putkien erotetun numeerinen tai `Edm.DateTimeOffset` arvojen dynaaminen arvojoukon pinta merkintä)
  - Esimerkki: `facet=baseRate,values:10|20` tuottaa kolme uudelleenkäytettävät: yksi perus korko 0 asti, mutta lukuun ottamatta enintään 10, yksi 10, mutta lukuun ottamatta 20 ja toinen 20 tai uudempi versio.
  - Esimerkki: `facet=lastRenovationDate,values:2010-02-01T00:00:00Z` tuottaa kaksi uudelleenkäytettävät: yksi hotellien renovated ennen helmikuussa 2010 ja toinen hotellit renovated helmikuu 1st, 2010 tai uudempi versio.
- `interval`(kokonaisluku väli on suurempi kuin 0, lukuja, tai `minute`, `hour`, `day`, `week`, `month`, `quarter`, `year` päivämääräarvot aika)
  - Esimerkki: `facet=baseRate,interval:100` tuottaa uudelleenkäytettävät perus alueita 100 koon perusteella. Esimerkiksi jos peruste korvaukset on kaikki $60 – $600, ole 0 – 100, 100 – 200, 200 – 300, 300-400, 400-500 ja 500 600 uudelleenkäytettävät.
  - Esimerkki: `facet=lastRenovationDate,interval:year` tuottaa yhden aikajakson vuotuiset kun hotellit on renovated.
- `timeoffset`([+-] hh: mm, [+-] hh: mm, tai [+-] [hh) `timeoffset` on valinnainen. Se voidaan yhdistää vain `interval` vaihtoehto, ja vain silloin, kun kentän tietotyyppi on käytetty `Edm.DateTimeOffset`. Arvon määrittäminen aika rajat määrittää UTC-aika-poikkeama-tiliin.
  - Esimerkki: `facet=lastRenovationDate,interval:day,timeoffset:-01:00` päivä reunaa, joka alkaa käyttää 01:00:00 UTC-aika (keskiyöhön kohde aikavyöhykkeen)
- **Huomautus**: `count` ja `sort` voidaan yhdistää samaan pinta määrityksen, mutta niitä ei voi yhdistää `interval` tai `values`, ja `interval` ja `values` ei voi yhdistää yhteen.
- **Huomautus**: aikavälin rakenteita päivämäärä aika on laskettu UTC-ajan perusteella, jos `timeoffset` ei ole määritetty. Esimerkki: varten `facet=lastRenovationDate,interval:day`, päivän reunan alkaa 00:00:00 UTC-aika. 

> [AZURE.NOTE] **Haun** avulla kirjaa soitettaessa tämä parametri on nimeltään `facets` sijaan `facet`. Myös voit määrittää sen JSON matriisina merkkijonoja, jossa kukin merkkijono on erillinen pinta-lauseke.

`$filter=[string]`(valinnainen) - rakenteellisia hakulausekkeen vakio OData-syntaksissa. [OData-lausekkeiden syntaksista](#ODataExpressionSyntax) saat tiedot OData-lauseke kieliopin, joka tukee Azure haun alijoukko.

> [AZURE.NOTE] **Haun** avulla kirjaa soitettaessa tämä parametri on nimeltään `filter` sijaan `$filter`.

`highlight=[string]`(valinnainen) - CSV-kentän nimien osumien käytettäviä korostaa. Vain `searchable` kenttiä voi käyttää Osumien korostaminen.

`highlightPreTag=[string]`(valinnainen, oletusarvo on `<em>`) - A-tunniste, jonka eteen haluat osumien korostukset merkkijono. On määritettävä `highlightPostTag`.

> [AZURE.NOTE] Kun kutsumista **haun** avulla GET-varatut merkit URL-osoitteessa on prosenttia-koodattava (esimerkiksi # sijaan 23 %).

`highlightPostTag=[string]`(valinnainen, oletusarvo on `</em>`)-merkkijono-tunniste, joka liittää osumien korostukset. On määritettävä `highlightPreTag`.

> [AZURE.NOTE] Kun kutsumista **haun** avulla GET-varatut merkit URL-osoitteessa on prosenttia-koodattava (esimerkiksi # sijaan 23 %).

`scoringProfile=[string]`(valinnainen) - arvioida tulosmalli profiilin nimi vastaa pisteet vastaavat asiakirjat, jotta voit lajitella tiedot.

`scoringParameter=[string]`Ilmaisee (nolla tai enemmän) - arvot kullekin parametrille määritetty tulosmalli-funktiossa (esimerkiksi `referencePointParameter`)-muodossa `name-value1,value2,...`.

- Esimerkiksi jos tulosmalli profiili määrittää funktion kanssa parametrin nimeltä "mylocation" kyselyn merkkijono-vaihtoehto olisi `&scoringParameter=mylocation--122.2,44.8`. Ensimmäinen viiva erottaa nimi arvo-luettelosta, kun toinen viiva on osa ensimmäisen arvon (Tässä esimerkissä pituutta).
- Tulosmalli parametrien, kuten tunnisteen kehittämällä, jotka voivat sisältää pilkuilla voit pääse kyseiset arvot-luettelossa käyttämällä puolilainausmerkkeihin. Jos arvot itse sisällä puolilainausmerkkeihin, niitä voi päästä tuplaamalla.
  - Esimerkiksi jos kehittämällä parametrin nimeltä "mytag" tunnisteen ja haluat edistää tunnisteessa arvot "Hei, O'Brien" ja "Keila"-kyselyn merkkijono-vaihtoehto on `&scoringParameter=mytag-'Hello, O''Brien',Smith`. Huomaa, että tarjoukset ovat vain pakolliset arvot, jotka sisältävät pilkkuja.

> [AZURE.NOTE] **Haun** avulla kirjaa soitettaessa tämä parametri on nimeltään `scoringParameters` sijaan `scoringParameter`. Lisäksi se määritetään JSON matriisina merkkijonoja, jossa kukin merkkijono on erillinen `name-values` pari.

`minimumCoverage`(valinnainen, oletusarvo on 100)-luvun väliltä 0 – 100 valmistumisprosentin ilmaiseminen poistettavan indeksin, jotta kysely ilmoitetaan onnistumisen hakukyselyn piiriin. Oletusarvon mukaan koko indeksi on oltava käytettävissä tai `Search` palauttavat tilakoodin HTTP 503. Jos määrität `minimumCoverage` ja `Search` on luotu, se palauttaa HTTP 200 ja Sisällytä `@search.coverage` arvo, joka sisältyy kyselyn indeksin valmistumisprosentin vastauksessa.

> [AZURE.NOTE] Tämän parametrin arvo arvo on pienempi kuin 100 voi olla hyötyä myös palveluille vain yksi replikaan haun käytettävyyden varmistaminen. Kuitenkin kaikki vastaavat tiedostot ovat taata hakutuloksista. Jos haku peruuttaminen on tulostusjälkeä tärkeämpi sovelluksen käytettävyys ja valitse kannattaa jättää `minimumCoverage` , sen oletusarvo on 100.

`api-version=[string]`(pakollinen). Preview-versiosta on `api-version=2015-02-28-Preview`. Katso [Search-palvelun versiotietojen](http://msdn.microsoft.com/library/azure/dn864560.aspx) tiedot ja vaihtoehtoiset versiot.

Huomautus: tätä toimintoa varten `api-version` on määritetty kyselyparametri riippumatta siitä, onko soitat GET tai POST **haun** URL-osoitteen.

**Pyydä otsikot**

Seuraavassa luettelossa kuvataan pakolliset ja valinnaiset pyynnön otsikot.

- `api-key`: Mitä `api-key` todennetaan Search-palvelun pyynnön. On merkkijonoarvo, yksilöllisiä palvelun URL-osoite. **Etsi** pyynnön voit määrittää järjestelmänvalvojan avain- tai kyselyn avain `api-key`.

Sinun on myös muodostaa pyyntö URL-Osoitteen palvelunimi. Saat palvelunimi ja `api-key` service-koontinäytössä Azure-portaalissa. Katso Ohje sivuissa Siirtyminen [luominen Azure Search-palvelun portaalissa](search-create-service-portal.md) .

**Pyydä tekstissä**

HAE varten: ei mitään.

Saat viestin:

    {
      "count": true | false (default),
      "facets": [ "facet_expression_1", "facet_expression_2", ... ],
      "filter": "odata_filter_expression",
      "highlight": "highlight_field_1, highlight_field_2, ...",
      "highlightPreTag": "pre_tag",
      "highlightPostTag": "post_tag",
      "minimumCoverage": # (% of index that must be covered to declare query successful; default 100),
      "moreLikeThis": "document_key" (mutually exclusive with "search" parameter),
      "orderby": "orderby_expression",
      "scoringParameters": [ "scoring_parameter_1", "scoring_parameter_2", ... ],
      "scoringProfile": "scoring_profile_name",
      "search": "simple_query_expression",
      "searchFields": "field_name_1, field_name_2, ...",
      "searchMode": "any" (default) | "all",
      "select": "field_name_1, field_name_2, ...",
      "skip": # (default 0),
      "top": #
    }

**Osittaisen haun vastaukset jatkaminen**

Joskus Azure haku ei voi palauttaa pyydettyjen tulokset yhteen Etsi vastaus. Tämä voi johtua eri syistä, esimerkiksi kun kysely pyytää liikaa tiedostoja määrittämällä ei `$top` tai arvo, joka määrittää `$top` , joka on liian suuri. Tässä tapauksessa Azure haku sisältää `@odata.nextLink` huomautuksen vastauksen tekstiosaan ja myös `@search.nextPageParameters` jos se on POST-pyynnön. Voit käyttää arvojen näistä merkeistä muodostetaan toiseen hakupyyntöä saat haun vastausta seuraavaan osaan. Tätä kutsutaan ***jatkumista*** alkuperäistä hakupyyntöä ja huomautukset yleensä kutsutaan ***jatkumista tunnusten***. Katso [alla olevasta esimerkistä](#SearchResponse) tiedot näistä merkeistä syntaksista ja jossa ne näkyvät vastauksen tekstissä. 

Syitä, miksi Azure haku saattaa tuottaa jatkumista tunnusten ovat käyttöönottokohtainen ja voivat muuttua. Tehokkaat asiakkaiden on aina oltava valmiina käsittelemään vähemmän asiakirjoja odotettua palautetaan ja jatkumista tunnuksen sisältyy Jatka haetaan asiakirjoja. Huomaa, että HTTP samaa menetelmää on käytettävä kuin alkuperäisen pyynnön ennen kuin jatkat. Esimerkiksi jos lähetit GET-pyynnössä, voit lähettää jatkumista pyynnöt käytettävä myös GET (ja samoin Kirjaa).

<a name="SearchResponse"></a>
**Vastaus**

Tilakoodin: 200 OK palautetaan onnistuneen vastauksen odottaminen.

    {
      "@odata.count": # (if $count=true was provided in the query),
      "@search.coverage": # (if minimumCoverage was provided in the query),
      "@search.facets": { (if faceting was specified in the query)
        "facet_field": [
          {
            "value": facet_entry_value (for non-range facets),
            "from": facet_entry_value (for range facets),
            "to": facet_entry_value (for range facets),
            "count": number_of_documents
          }
        ],
        ...
      },
      "@search.nextPageParameters": { (request body to fetch the next page of results if not all results could be returned in this response and Search was called with POST)
        "count": ... (value from request body if present),
        "facets": ... (value from request body if present),
        "filter": ... (value from request body if present),
        "highlight": ... (value from request body if present),
        "highlightPreTag": ... (value from request body if present),
        "highlightPostTag": ... (value from request body if present),
        "minimumCoverage": ... (value from request body if present),
        "moreLikeThis": ... (value from request body if present),
        "orderby": ... (value from request body if present),
        "scoringParameters": ... (value from request body if present),
        "scoringProfile": ... (value from request body if present),
        "search": ... (value from request body if present),
        "searchFields": ... (value from request body if present),
        "searchMode": ... (value from request body if present),
        "select": ... (value from request body if present),
        "skip": ... (page size plus value from request body if present),
        "top": ... (value from request body if present minus page size),
      },
      "value": [
        {
          "@search.score": document_score (if a text query was provided),
          "@search.highlights": {
            field_name: [ subset of text, ... ],
            ...
          },
          key_field_name: document_key,
          field_name: field_value (retrievable fields or specified projection),
          ...
        },
        ...
      ],
      "@odata.nextLink": (URL to fetch the next page of results if not all results could be returned in this response; Applies to both GET and POST)
    }

**Esimerkkejä:**

Löydät lisää esimerkkejä [OData lausekkeiden syntaksista Azure-kohdennetun haun](https://msdn.microsoft.com/library/azure/dn798921.aspx) sivulle.

1)  Etsi hakemiston lajiteltu laskevaan järjestykseen päivämäärän mukaan.


    HAE /indexes/hotels/docs? Etsi = * & $orderby = lastRenovationDate desc & api-version = 2015-02-28-esikatselu

    Kirjaa /indexes/hotels/docs/search? api-version 2015-02-28-esikatselu = {"Etsi": "*", "lajittelu": "lastRenovationDate desc"}

2)  Kohdistettua haun avainsanat ja noutaa rakenteita luokat, luokitus, tunnisteita, sekä tietueita, joiden tiettyjä alueita baseRate:


    HAE /indexes/hotels/docs? Etsi = testi & pinta = luokan & pinta = luokitus & pinta = tunnisteet ja pinta = baseRate-arvot: 80 | 150 | 220 & api-versio = 2015-02-28-esikatselu

    Kirjaa /indexes/hotels/docs/search? api-version = 2015-02-28-esikatselu {"Etsi": "testata", "rakenteita": ["luokka", "luokitus", "tunnisteita", "baseRate-arvot: 80 | 150 | 220"]}

3)  Suodattimen käyttäminen edellisen kohdistettua kyselytulosten tarkentamiseksi sen jälkeen, kun käyttäjä napsauttaa arviointi 3 ja luokan "Oppaalla":


    HAE /indexes/hotels/docs? Etsi = testi & pinta = tunnisteet ja pinta = baseRate-arvot: 80 | 150 | 220 & $filter = luokitus 3 ja luokan eq 'oppaalla"seuraa & api-version = 2015-02-28-esikatselu

    Kirjaa /indexes/hotels/docs/search? api-version = 2015-02-28-esikatselu {"Etsi": "testaaminen", "rakenteita": ["tunnisteita", "baseRate-arvot: 80 | 150 | 220"], "suodatin": "luokitus eq 3 ja luokan eq 'Oppaalla'"}

4) Määritä kohdistettua haun yläraja kyselyssä palautetut yksilöllinen ehdoin. Oletusarvo on 10, mutta voit suurentaa tai pienentää tämän arvon avulla `count` parametri `facet` määrite:


    HAE /indexes/hotels/docs? Etsi = testi & pinta = kaupunki, Laske: 5 & api-version = 2015-02-28-esikatselu

    Kirjaa /indexes/hotels/docs/search? api-versio 2015-02-28-esikatselu = {"Etsi": "testata", "rakenteita": ["kaupunki-, Laske: 5"]}

5)  Tiettyjen kenttien; avainsanat Esimerkiksi kielikohtaiset kentän:


    HAE /indexes/hotels/docs? Etsi = hôtel & searchFields = description_fr & api-version = 2015-02-28-esikatselu

    Kirjaa /indexes/hotels/docs/search? api-version 2015-02-28-esikatselu = {"Etsi": "hôtel", "searchFields": "description_fr"}

6) Hakuja monikenttäisen indeksin. Voit tallentaa ja kyselyn hakukenttiä useilla kielillä, kaikki samassa hakemistossa.  Jos englanniksi ja ranskaksi kuvaukset ole yhtä samaan asiakirjaan, voit palauttaa jonkin tai kaikki kyselyn tuloksissa:


    HAE /indexes/hotels/docs? Etsi = hotellista & searchFields = kuvaus, description_fr & api-version = 2015-02-28-esikatselu

    Kirjaa /indexes/hotels/docs/search? api-version 2015-02-28-esikatselu = {"Etsi": "hotellista", "searchFields": "kuvaus, description_fr"}

Huomaa, että voit vain kyselyn indeksi kerrallaan. Kullekin kielelle useita indeksejä ei luoda, paitsi, jos aiot kyselyn yksi kerrallaan.

7)  Sivutuksen - Get 1st sivun kohteiden (sivun koko on 10):


    HAE /indexes/hotels/docs? Etsi = * & $skip = 0 & $top = 10 & api-version = 2015-02-28-esikatselu

    Kirjaa /indexes/hotels/docs/search? api-version = 2015-02-28-esikatselu {"Etsi": "*", "Ohita": 0, "yläreunan": 10}

8)  Sivutuksen - Get 2. sivun kohteiden (sivun koko on 10):


    HAE /indexes/hotels/docs? Etsi = * & $skip = 10 & $top = 10 & api-version = 2015-02-28-esikatselu

    Kirjaa /indexes/hotels/docs/search? api-version = 2015-02-28-esikatselu {"Etsi": "*", "Ohita": 10, "yläreunan": 10}

9)  Hae tietyt kentät:


    HAE /indexes/hotels/docs? Etsi = * & $select = hotelName, kuvaus ja api-version = 2015-02-28-esikatselu

    Kirjaa /indexes/hotels/docs/search? api-version 2015-02-28-esikatselu = {"Etsi": "*", "Valitse": "hotelName kuvaus"}

10)  Hae asiakirjoja, jotka vastaavat tiettyjä Suodatuslauseke:


    HAE /indexes/hotels/docs? $filter = (baseRate sivu 60 ja baseRate lt 300) tai hotelName 'Upea pysy' seuraa & api-version = 2015-02-28-esikatselu

    Kirjaa /indexes/hotels/docs/search? api-version = 2015-02-28-esikatselu {"suodatin": "(baseRate sivu 60 ja baseRate lt 300) tai hotelName eq 'Upea pitää viestit'"}

11) Etsi indeksi- ja palaa Vaillinaiset lauseet ja osumien korostukset


    HAE /indexes/hotels/docs? Etsi = jotain ja Korosta = kuvaus & api-version = 2015-02-28-esikatselu

    Kirjaa /indexes/hotels/docs/search? api-version 2015-02-28-esikatselu = {"Etsi": "jotain", "korostaminen": "kuvaus"}

12) Avainsanat ja palauttaa tiedostoja lajiteltu kauemmaksi ulkopuolella viittauksen lähempänä sijainnista


    HAE /indexes/hotels/docs? Etsi = jotain & $orderby=geo.distance (sijainti-geography'POINT(-122.12315 47.88121) ") & api-version = 2015-02-28-esikatselu

    Kirjaa /indexes/hotels/docs/search? api-version = 2015-02-28-esikatselu {"Etsi": "jotain", "lajittelu": "geo.distance (sijainti-geography'POINT(-122.12315 47.88121)") "}

13) Etsiä olettaen on nimeltään "geo" tulosmalli profiilin hakemiston kaksi etäisyys tulosmalli toiminnoilla, yksi määrittäminen parametrin nimeltä "currentLocation"-parametrin nimeltä "lastLocation" määrittäminen


    HAE /indexes/hotels/docs? Etsi = jotain & scoringProfile = geo & scoringParameter = currentLocation--122.123,44.77233 & scoringParameter = lastLocation--121.499,44.2113 & api-versio = 2015-02-28-esikatselu

    Kirjaa /indexes/hotels/docs/search? api-version 2015-02-28-esikatselu = {"Etsi": "jotain", "scoringProfile": "geo", "scoringParameters": ["currentLocation--122.123,44.77233", "lastLocation--121.499,44.2113"]}

14) Voit etsiä tiedostoja [yksinkertaisen kyselyn](https://msdn.microsoft.com/library/dn798920.aspx)syntaksin hakemistoon. Tämä kysely palauttaa Hotellit, jossa hakukenttiä sisältää ehtoja "mukavuutta" ja "sijainnin" mutta ei "oppaalla":


    HAE /indexes/hotels/docs? Etsi = mukavuutta + sijainti-oppaalla & searchMode = all & api-version = 2015-02-28-esikatselu

    Kirjaa /indexes/hotels/docs/search? api-versio 2015-02-28-esikatselu = {"Etsi": "mukavuutta + sijainti-oppaalla", "searchMode": "kaikki"}

Huomaa, `searchMode=all` yläpuolella. Tämä parametri mukaan lukien ohittaa oletusarvo `searchMode=any`, varmistamiseksi, `-motel` tarkoittaa "AND NOT" sijaan "Tai NOT". Ilman `searchMode=all`, näkyviin tulee "Tai NOT" joka laajentaa sen sijaan, että rajoittaa hakutulosten ja näin voit intuitiivisena joitakin käyttäjille.

15) Voit etsiä tiedostoja [lucene kyselyn](https://msdn.microsoft.com/library/mt589323.aspx)syntaksin hakemistoon. Tämä kysely palauttaa Hotellit, joiden luokka-kenttä sisältää termi "budjetin" ja kaikki hakukenttiä, jotka sisältävät sanan "viimeksi renovated". Tiedostot, jotka sisältävät sanan "viimeksi renovated" luokittelu suurempi tuloksena termin tehon lisäys-arvon (3)

    HAE /indexes/hotels/docs?search = luokan: budjetin ja \"viimeksi renovated\"^ 3 ja searchMode = all & api-version = 2015-02-28-esikatselu & querytype = täysi

    Kirjaa /indexes/hotels/docs/search?api-version = 2015-02-28-esikatselu {"Etsi": "luokka: budjetin ja \"viimeksi renovated\"^ 3", "queryType": "koko", "searchMode": "kaikki"}

<a name="LookupAPI"></a>
## <a name="lookup-document"></a>Haku-asiakirja

**Haku-tiedoston** toiminto hakee asiakirjan Azure haku. Tästä on hyötyä, kun käyttäjä napsauttaa tietyn hakutulos ja haluat lisätietoja asiakirjan hakeminen.

    GET https://[service name].search.windows.net/indexes/[index name]/docs/[key]?[query parameters]
    api-key: [admin or query key]

**Pyyntö**

HTTPS tarvitaan palvelupyyntöjä. **Haku asiakirjan** pyyntö on rakennettava seuraavasti.

    GET /indexes/[index name]/docs/key?[query parameters]

Vaihtoehtoisesti voit avaimen haussa perinteinen OData-syntaksia:

    GET /indexes('[index name]')/docs('[key]')?[query parameters]

Pyyntö URI sisältää [Indeksinimi] ja [avain], määrittämällä asiakirjaa hakea mitä indeksistä. Saat vain yhteen asiakirjaan kerrallaan. **Haun** avulla voit hakea yksittäisen pyynnön useita tiedostoja.

**Kyselyparametrit**

`$select=[string]`(valinnainen) - luettelon noutaminen CSV-kentistä. Jos Määrittämätön tai arvoksi `*`, kaikki kentät, jotka on merkitty enää rakenteessa käytettävissä sisältyvät ennuste.

`api-version=[string]`(pakollinen). Preview-versiosta on `api-version=2015-02-28-Preview`. Katso [Search-palvelun versiotietojen](http://msdn.microsoft.com/library/azure/dn864560.aspx) tiedot ja vaihtoehtoiset versiot.

Huomautus: tätä toimintoa varten `api-version` on määritetty kyselyparametri.

**Pyydä otsikot**

Seuraavassa luettelossa kuvataan pakolliset ja valinnaiset pyynnön otsikot.

- `api-key`: Mitä `api-key` todennetaan Search-palvelun pyynnön. On merkkijonoarvo, yksilöllisiä palvelun URL-osoite. **Haku asiakirjan** pyynnön voit määrittää järjestelmänvalvojan avain- tai kyselyn avain `api-key`.

Sinun on myös muodostaa pyyntö URL-Osoitteen palvelunimi. Saat palvelunimi ja `api-key` service-koontinäytössä Azure-portaalissa. Katso Ohje sivuissa Siirtyminen [luominen Azure Search-palvelun portaalissa](search-create-service-portal.md) .

**Pyydä tekstissä**

Ei mitään.

**Vastaus**

Tilakoodin: 200 OK palautetaan onnistuneen vastauksen odottaminen.

    {
      field_name: field_value (fields matching the default or specified projection)
    }

**Esimerkki**

Haku tiedostoa, jonka avain "2"

    GET /indexes/hotels/docs/2?api-version=2015-02-28-Preview

Haku tiedostoa, jonka avain "3" OData syntaksin:

    GET /indexes('hotels')/docs('3')?api-version=2015-02-28-Preview

<a name="CountDocs"></a>
## <a name="count-documents"></a>Laske-asiakirjat

**Laske-tiedostot** -toiminnon hakee hakuindeksin tiedostojen määrän laskeminen. `$count` Syntaksi on osa OData-protokolla.

    GET https://[service name].search.windows.net/indexes/[index name]/docs/$count?api-version=[api-version]
    Accept: text/plain
    api-key: [admin or query key]

**Pyyntö**

HTTPS tarvitaan palvelupyyntöjä. **Laske asiakirjat** -pyyntö on rakennettava GET-menetelmällä.

[Indeksinimi] URI-pyynnössä kertoo palvelu palauttaa määritetyn indeksin docs-sivustokokoelman kaikkien kohteiden määrä.

`api-version=[string]`(pakollinen). Preview-versiosta on `api-version=2015-02-28-Preview`. Katso [Search-palvelun versiotietojen](http://msdn.microsoft.com/library/azure/dn864560.aspx) tiedot ja vaihtoehtoiset versiot.

**Pyydä otsikot**

Seuraavassa luettelossa kuvataan pakolliset ja valinnaiset pyynnön otsikot.

- `Accept`: Tämä arvo on määritettävä `text/plain`.
- `api-key`: Mitä `api-key` todennetaan Search-palvelun pyynnön. On merkkijonoarvo, yksilöllisiä palvelun URL-osoite. **Laske asiakirjojen** pyynnön voit määrittää järjestelmänvalvojan avain- tai kyselyn avain `api-key`.

Sinun on myös muodostaa pyyntö URL-Osoitteen palvelunimi. Saat palvelunimi ja `api-key` service-koontinäytössä Azure-portaalissa. Katso Ohje sivuissa Siirtyminen [luominen Azure Search-palvelun portaalissa](search-create-service-portal.md) .

**Pyydä tekstissä**

Ei mitään.

**Vastaus**

Tilakoodin: 200 OK palautetaan onnistuneen vastauksen odottaminen.

Vastauksen tekstissä esiintyy määrän arvo kokonaislukuna, joka on muotoiltu teksti.

<a name="Suggestions"></a>
## <a name="suggestions"></a>Ehdotukset

**Ehdotukset** -toiminto hakee ehdotuksia osittaisen haun syöte perusteella. Se yleensä käytetään Hakuruudut antamaan täydentävä ehdotuksia käyttäjät syöttävät hakusanojen.

Ehdotus pyynnöt pyritään ehdottaminen kohdeasiakirja, joten ehdotettu tekstin voi toistaa, jos useita candidate asiakirjoja vastaa syötteen sama haku. Voit käyttää `$select` (mukaan lukien asiakirjan avain) asiakirjan muiden kenttien hakemiseen niin, että näet, mitkä asiakirja on kunkin ehdotus lähde.

**Ehdotukset** -toiminto on myöntänyt kuin GET tai POST pyyntö.

    GET https://[service name].search.windows.net/indexes/[index name]/docs/suggest?[query parameters]
    api-key: [admin or query key]

    POST https://[service name].search.windows.net/indexes/[index name]/docs/suggest?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin or query key]

**Kirjaa käyttäminen GET sijaan**

Kun käytät HTTP GET Soita **ehdotuksia** API, sinun täytyy huomioon, että pyyntö URL-Osoitteen pituus voi olla enintään 8 KB. Tämä on tarpeeksi yleensä useimpien sovellusten. Jotkin sovellukset tuottaa kuitenkin hyvin suuria kyselyjä, erityisesti OData-suodatinlausekkeiden. Näiden sovellusten käyttämällä HTTP POST on kätevä vaihtoehto, koska se sallii suurempi kuin GET suodattimet. VIESTI, jossa suodatinta lauseet määrä on rajoittava tekijä ei raaka suodattimen merkkijonon pyynnön kokorajoitus viestin ollessa 16 Megatavun koon.

> [AZURE.NOTE] Vaikka kirjaa pyynnön kokorajoituksen on erittäin suuri, suodatinlausekkeiden ei voi olla satunnaisesti monimutkaisia. [OData lausekkeiden syntaksista](https://msdn.microsoft.com/library/dn798921.aspx) Saat lisätietoja suodattimen monimutkaisuus rajoitukset.

**Pyyntö**

HTTPS tarvitaan palvelupyyntöjä. **Ehdotukset** -pyyntö on rakennettava GET tai POST tavoilla.

Pyynnön URI määrittää kyselyyn indeksin nimi. Parametrien, kuten osittain syötteen hakusanoja, määritetään kyselymerkkijonon kyseessä GET-pyyntöjä ja pyynnössä kyseessä viestin tekstiosaan pyytää.

Paras käytäntö GET-pyyntöjä luotaessa muista tietyn kyselyparametrit [URL-Osoitteen koodata](https://msdn.microsoft.com/library/system.uri.escapedatastring.aspx) soitettaessa REST-Ohjelmointirajapinnalla suoraan. **Ehdotukset** toimintoja mukaan lukien:

- `$filter`
- `highlightPreTag`
- `highlightPostTag`
- `search`

URL-koodausta suositellaan vain yllä kyselyparametrit. Jos olet vahingossa URL-Osoitteen koodata koko kyselymerkkijonon (kaikki jälkeen?)-pyynnöt vaihtuvat.

URL-koodaus on myös vain tarvittavat suoraan käyttämällä HAE REST-Ohjelmointirajapinnalla soitettaessa. Ei ole URL-koodaus on tarpeen **ehdotusten** avulla kirjaa soitettaessa tai käyttämällä [.NET asiakkaan kirjastoon](https://msdn.microsoft.com/library/dn951165.aspx), joka käsittelee URL-koodausta puolestasi.

**Kyselyparametrit**

**Ehdotukset** hyväksyy useita parametreja, joiden kyselyehdot ja määritä myös haun toiminta. Voit antaa nämä parametrit URL-osoitteen kyselyn merkkijono soitettaessa **ehdotuksia** kautta GET ja JSON ominaisuuksina pyynnön tekstissä soitettaessa **ehdotuksia** kirjaa kautta. Joidenkin parametrien syntaksi on hieman erilainen GET ja kirjaa välillä. Nämä erot on merkitty tapauksen alla:

`search=[string]`-hakuteksti Ehdota kyselyjen avulla. On oltava vähintään 1 merkki ja enintään 100 merkkiä.

`highlightPreTag=[string]`(valinnainen) - merkkijono tunnisteen, Etsi osumia eteen. On määritettävä `highlightPostTag`.

> [AZURE.NOTE] Kun kutsumista **ehdotuksia** GET-varatut merkit URL-osoitteen käyttäminen on prosenttia-koodattava (esimerkiksi # sijaan 23 %).

`highlightPostTag=[string]`(valinnainen) - merkkijono tunniste, joka liittää Etsi osumia. On määritettävä `highlightPreTag`.

> [AZURE.NOTE] Kun kutsumista **ehdotuksia** GET-varatut merkit URL-osoitteen käyttäminen on prosenttia-koodattava (esimerkiksi # sijaan 23 %).

`suggesterName=[string]`-suggester mukaisesti nimi `suggesters` kokoelma, joka on osa indeksin määrityksen. A `suggester` määrittää kentät, jotka ovat skannattu ehdotettu kyselyn ehtojen perusteella. Katso lisätietoja [Suggesters](#Suggesters) .

`fuzzy=[boolean]`(valinnainen; oletusarvo on EPÄTOSI) – Kun tämä API TRUE etsii ehdotuksia vaikka etsittävän tekstin on korvaava tai puuttuu-merkki. Kun tämä tarjoaa paremman kokemus joissakin tilanteissa on osoitteessa suorituskyvyn, kustannusten epäselvältä ehdotus haut ovat hitaammin ja tarjoaman Lisää resursseja.

`searchFields=[string]`(valinnainen) - CSV-kentässä määritetyn etsittävä teksti Etsi nimien luettelossa. Kohdekentät on otettava käyttöön ehdotuksia.

`$top=#`(valinnainen; oletusarvo = 5)-ehdotusten hakemiseen määrä. On oltava luku väliltä 1 – 100.

> [AZURE.NOTE] **Ehdotukset** käyttämällä kirjaa soitettaessa tämä parametri on nimeltään `top` sijaan `$top`.

`$filter=[string]`(valinnainen) - ehdotukset huomioon lauseke, joka suodattaa tiedostoja.

> [AZURE.NOTE] **Ehdotukset** käyttämällä kirjaa soitettaessa tämä parametri on nimeltään `filter` sijaan `$filter`.

`$orderby=[string]`(valinnainen) - lausekeluettelon CSV-tulosten mukaan lajittelu. Kunkin lauseke voi olla kenttänimen tai puhelun `geo.distance()` funktio. Kunkin lausekkeen perään `asc` , merkitty Nouseva, ja `desc` osoittamaan laskeva. Oletusarvo on nousevaan järjestykseen. 32 lausekkeita enimmäismäärä on `$orderby`.

> [AZURE.NOTE] **Ehdotukset** käyttämällä kirjaa soitettaessa tämä parametri on nimeltään `orderby` sijaan `$orderby`.

`$select=[string]`(valinnainen) - luettelon noutaminen CSV-kentistä. Jos Määrittämätön, vain asiakirjan avain ja ehdotus teksti palautetaan. Voit pyytää kaikki kentät erikseen parametrin arvoksi `*`.

> [AZURE.NOTE] **Ehdotukset** käyttämällä kirjaa soitettaessa tämä parametri on nimeltään `select` sijaan `$select`.

`minimumCoverage`(valinnainen, oletusarvo on 80)-luvun väliltä 0 – 100 valmistumisprosentin ilmaiseminen poistettavan indeksin, jotta kysely ilmoitetaan onnistumisen ehdotuksia kyselyn piiriin. Oletusarvon mukaan vähintään 80 % indeksi on oltava käytettävissä tai `Suggest` palauttavat tilakoodin HTTP 503. Jos määrität `minimumCoverage` ja `Suggest` on luotu, se palauttaa HTTP 200 ja Sisällytä `@search.coverage` arvo, joka sisältyy kyselyn indeksin valmistumisprosentin vastauksessa.

> [AZURE.NOTE] Tämän parametrin arvo arvo on pienempi kuin 100 voi olla hyötyä myös palveluille vain yksi replikaan haun käytettävyyden varmistaminen. Kaikki vastaavat ehdotuksia on kuitenkin taata sijaittava tulokset. Jos peruuttaminen on tulostusjälkeä tärkeämpi sovelluksen käytettävyys ja valitse ei kannata alemmalle tasolle `minimumCoverage` alle 80 oletusarvoisella.

`api-version=[string]`(pakollinen). Preview-versiosta on `api-version=2015-02-28-Preview`. Katso [Search-palvelun versiotietojen](http://msdn.microsoft.com/library/azure/dn864560.aspx) tiedot ja vaihtoehtoiset versiot.

Huomautus: tätä toimintoa varten `api-version` on määritetty kyselyparametri riippumatta siitä, onko soitat **ehdotuksia** GET tai Kirjaa URL-osoitteen.

**Pyydä otsikot**

Seuraavassa luettelossa kuvataan pakolliset ja valinnaiset pyynnön otsikot

- `api-key`: Mitä `api-key` todennetaan Search-palvelun pyynnön. On merkkijonoarvo, yksilöllisiä palvelun URL-osoite. **Ehdotukset** pyynnön voit määrittää järjestelmänvalvojan avain- tai kyselyn key `api-key`.

Sinun on myös muodostaa pyyntö URL-Osoitteen palvelunimi. Saat palvelunimi ja `api-key` service-koontinäytössä Azure-portaalissa. Katso Ohje sivuissa Siirtyminen [luominen Azure Search-palvelun portaalissa](search-create-service-portal.md) .

**Pyydä tekstissä**

HAE varten: ei mitään.

Saat viestin:

    {
      "filter": "odata_filter_expression",
      "fuzzy": true | false (default),
      "highlightPreTag": "pre_tag",
      "highlightPostTag": "post_tag",
      "minimumCoverage": # (% of index that must be covered to declare query successful; default 80),
      "orderby": "orderby_expression",
      "search": "partial_search_input",
      "searchFields": "field_name_1, field_name_2, ...",
      "select": "field_name_1, field_name_2, ...",
      "suggesterName": "suggester_name",
      "top": # (default 5)
    }

**Vastaus**

Tilakoodin: 200 OK palautetaan onnistuneen vastauksen odottaminen.

    {
      "@search.coverage": # (if minimumCoverage was provided in the query),
      "value": [
        {
          "@search.text": "...",
          "<key field>": "..."
        },
        ...
      ]
    }

Jos ennuste-vaihtoehto on käytössä hakea ne sisältyisivät jokainen matriisin osa kentät:

    {
      "@search.coverage": # (if minimumCoverage was provided in the query),
      "value": [
        {
          "@search.text": "...",
          "<key field>": "..."
          <other projected data fields>
        },
        ...
      ]
    }

**Esimerkki**

Noutaa 5 ehdotukset missä osittaisen haun syöte "luksia"

    GET /indexes/hotels/docs/suggest?search=lux&$top=5&suggesterName=sg&api-version=2015-02-28-Preview

    POST /indexes/hotels/docs/suggest?api-version=2015-02-28-Preview
    {
      "search": "lux",
      "top": 5,
      "suggesterName": "sg"
    }
