<properties
    pageTitle="Yhdistämällä DocumentDB Azure hakutuloksia hyödyntämällä Indeksoijilla | Microsoft Azure"
    description="Tämän artikkelin avulla opit käyttämään Azure haun indeksointitoiminto kanssa DocumentDB tietolähteenä."
    services="documentdb"
    documentationCenter=""
    authors="dennyglee"
    manager="jhubbard"
    editor="mimig"/>

<tags
    ms.service="documentdb"
    ms.devlang="rest-api"
    ms.topic="article"
    ms.tgt_pltfrm="NA"
    ms.workload="data-services"
    ms.date="07/08/2016"
    ms.author="denlee"/>

#<a name="connecting-documentdb-with-azure-search-using-indexers"></a>Yhdistämällä DocumentDB Azure hakutuloksia hyödyntämällä Indeksoijilla

Jos haluat toteuttamisesta hyvien haun kokemukset DocumentDB tietojen päälle, käytä Azure haun indeksointitoiminto DocumentDB! Tässä artikkelissa esitellään, miten voit integroida Azure DocumentDB Azure haulla eikä sinun tarvitse säilyttää indeksoinnin infrastruktuurin koodia!

Voit määrittää tämän on [asennuksen Azure haku-tiliä](../search/search-create-service-portal.md) (sinun ei tarvitse päivittää Normaali haku) ja soita sitten [Azure haun REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx) DocumentDB **tietolähteen** ja **indeksointitoiminto** tietolähteen luomiseen.

Tilauksen Lähetä pyyntöjä REST API vuorovaikutuksessa voit käyttää [Postman](https://www.getpostman.com/), [Fiddler](http://www.telerik.com/fiddler)tai jokin haluamallasi työkalu.


##<a id="Concepts"></a>Azure haun indeksointitoiminto käsitteitä

Azure haun tukee luominen ja hallinta tietojen tietolähteitä (mukaan lukien DocumentDB) ja Indeksoijilla, jotka toimivat näille tietolähteille.

**Tietolähteen** määrittää, mitkä tiedot on indeksoitava, tunnistetiedot, joita käyttää tietoja ja käytännöt tehokkaasti tiedot (kuten muokata tai poistaa asiakirjojen kokoelmaa sisällä) tehdyt muutokset tunnistetaan Azure haku on otettu käyttöön. Tietolähde on määritetty riippumaton resurssin niin, että se voidaan käyttää useita Indeksoijilla.

**Indeksointitoiminto** kuvataan, miten tiedot jatkuu tietolähteen kohde hakuindeksin. Kannattaa suunnitella yhden indeksointitoiminto jokaisen kohteen indeksi ja tietojen lähteen yhdistelmän luomisesta. Samalla, kun voi olla useita Indeksoijilla kirjoittaminen kyselyjä sama indeksi-indeksointitoiminto voit kirjoittaa vain yhden indeksin luominen. Indeksointitoiminto on käytettävä:

- Voit suorittaa tietojen täyttämiseen indeksin erikseen kopion.
- Synkronoi indeksin aikataulun tietolähteen muutokset. Aikataulun on osa indeksointitoiminto-määritys.
- Käynnistää tarvittaessa päivitykset indeksin tarpeen mukaan.

##<a id="CreateDataSource"></a>Vaihe 1: Tietolähteen luominen

Lähetä uusi tietolähde luominen Azure-Search-palvelun, pyynnön-otsikot mukaan lukien HTTP POST-pyynnön.

    POST https://[Search service name].search.windows.net/datasources?api-version=[api-version]
    Content-Type: application/json
    api-key: [Search service admin key]

`api-version` Tarvitaan. Kelvolliset arvot `2015-02-28` tai sitä uudempi versio. Käy [API versiot Azure hakutoiminnossa](../search/search-api-versions.md) näet kaikki tuetut haun Ohjelmointirajapinta versiot.

Pyynnön leipätekstissä tietolähteen määrityksen, joihin sisältyvät seuraavat kentät:

- **nimi**: Valitse mikä tahansa edustavan DocumentDB tietokannan nimi.

- **tyyppi**: Käytä `documentdb`.

- **tunnistetiedot**:

    - **connectionString**: pakollinen. Määritä yhteyden tiedot Azure DocumentDB tietokannan seuraavanlainen:`AccountEndpoint=<DocumentDB endpoint url>;AccountKey=<DocumentDB auth key>;Database=<DocumentDB database id>`

- **säilön**:

    - **nimi**: pakollinen. Määritä DocumentDB kokoelman voi indeksoida tunnus.

    - **kyselyn**: valinnainen. Voit määrittää kyselyn Litistä haluamaansa JSON-tiedoston tuominen tasainen rakenteen, joka Azure-haku voi indeksoida.

- **dataChangeDetectionPolicy**: valinnainen. Katso [tiedot muuttuvat tunnistus käytännön](#DataChangeDetectionPolicy) alla.

- **dataDeletionDetectionPolicy**: valinnainen. Katso [Tietojen poisto tunnistus käytännön](#DataDeletionDetectionPolicy) alla.

Alla [Esimerkki pyynnön tekstissä](#CreateDataSourceExample).

###<a id="DataChangeDetectionPolicy"></a>Tallentaa muuttuneita asiakirjoja

Tietojen muuttaminen tunnistus käytännön tarkoituksena on tunnistaa tehokkaasti muutetut tiedot kohteet. Ainoa tuettu käytäntöä ei tällä hetkellä `High Water Mark` käytännön avulla `_ts` on viimeksi muokattu aikaleima ominaisuuden myöntämä DocumentDB - joka on määritetty seuraavasti:

    {
        "@odata.type" : "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
        "highWaterMarkColumnName" : "_ts"
    }

Voit myös on lisättävä `_ts` ennuste- ja `WHERE` lauseen kyselyn. Esimerkki:

    SELECT s.id, s.Title, s.Abstract, s._ts FROM Sessions s WHERE s._ts >= @HighWaterMark


###<a id="DataDeletionDetectionPolicy"></a>Poistettujen tiedostojen sieppaus

Kun rivit on poistettu lähdetaulukosta, sinun tulee poistaa ne rivit hakuindeksin sekä. Tietojen poistaminen tunnistus käytännön tarkoituksena on tunnistaa tehokkaasti poistettujen tietojen kohteet. Ainoa tuettu käytäntöä ei tällä hetkellä `Soft Delete` käytännön (poisto on merkitty merkinnän tarkoitus), joka on määritetty seuraavasti:

    {
        "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
        "softDeleteColumnName" : "the property that specifies whether a document was deleted",
        "softDeleteMarkerValue" : "the value that identifies a document as deleted"
    }

> [AZURE.NOTE] Haluat sisällyttää softDeleteColumnName-ominaisuuden SELECT-lauseessa, jos käytössäsi on mukautettu ennuste.

###<a id="CreateDataSourceExample"></a>Pyydä leipäteksti-Esimerkki

Seuraava esimerkki luo tietolähde Mukautettu kysely ja käytännön vihjeiden avulla:

    {
        "name": "mydocdbdatasource",
        "type": "documentdb",
        "credentials": {
            "connectionString": "AccountEndpoint=https://myDocDbEndpoint.documents.azure.com;AccountKey=myDocDbAuthKey;Database=myDocDbDatabaseId"
        },
        "container": {
            "name": "myDocDbCollectionId",
            "query": "SELECT s.id, s.Title, s.Abstract, s._ts FROM Sessions s WHERE s._ts > @HighWaterMark"
        },
        "dataChangeDetectionPolicy": {
            "@odata.type": "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
            "highWaterMarkColumnName": "_ts"
        },
        "dataDeletionDetectionPolicy": {
            "@odata.type": "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
            "softDeleteColumnName": "isDeleted",
            "softDeleteMarkerValue": "true"
        }
    }

###<a name="response"></a>Vastaus

Saat HTTP 201 luotu vastausta, jos tietolähde on luotu.

##<a id="CreateIndex"></a>Vaihe 2: Hakemiston luominen

Luo kohde Azure-hakuindeksin, jos sitä ei vielä ole. Voit tehdä tämän [Azure Portal Käyttöliittymän](../search/search-create-index-portal.md) tai [Luoda hakemiston API](https://msdn.microsoft.com/library/azure/dn798941.aspx)käyttämällä.

    POST https://[Search service name].search.windows.net/indexes?api-version=[api-version]
    Content-Type: application/json
    api-key: [Search service admin key]


Varmista, että kohde indeksi rakenne on yhteensopiva JSON asiakirjoja rakennetta tai mukautettua kyselyä-ennuste tulosteen.

>[AZURE.NOTE] Osioitu loppu oletusarvon asiakirjan avain on DocumentDB's `_rid` -ominaisuutta, joka saa nimetä siten, että `rid` Azure hakutoiminnolla. Lisäksi DocumentDB's `_rid` arvot sisältää merkkejä, jotka ovat virheellisiä Azure haku-näppäimiä. Tämän vuoksi `_rid` arvot ovat Base64-koodattuja.

###<a name="figure-a-mapping-between-json-data-types-and-azure-search-data-types"></a>A: kuva Yhdistämisen JSON tietotyypit ja Azure haku-tietotyypit

| JSON-TIETOTYYPPI|   YHTEENSOPIVAT KOHDE INDEKSIN KENTTIEN LAJIT|
|---|---|
|Bool|Edm.Boolean, Edm.String|
|Numerot, jotka näyttävät kokonaisluvut|Edm.Int32, Edm.Int64, Edm.String|
|Puhelinnumerot, irrallinen pisteiden näköiseksi|Edm.Double, Edm.String|
|Merkkijono|Edm.String|
|Alkuperäisten matriisien kirjoittaa esimerkiksi "a", "b", "c" |Collection(EDM.String)|
|Merkkijonoja, jotka näyttävät päivämäärät| Edm.DateTimeOffset, Edm.String|
|GeoJSON objektien esimerkiksi {"tyyppi": "Kohdan", "koordinaatit": [pitkä lat]} | Edm.GeographyPoint |
|Muut JSON-objektit|PUUTTUU|

###<a id="CreateIndexExample"></a>Pyydä leipäteksti-Esimerkki

Seuraavassa esimerkissä luodaan hakemiston tunnuksen ja kuvaus-kentän kanssa:

    {
       "name": "mysearchindex",
       "fields": [{
         "name": "id",
         "type": "Edm.String",
         "key": true,
         "searchable": false
       }, {
         "name": "description",
         "type": "Edm.String",
         "filterable": false,
         "sortable": false,
         "facetable": false,
         "suggestions": true
       }]
     }

###<a name="response"></a>Vastaus

Saat HTTP 201 luotu vastausta, jos indeksin luominen onnistui.

##<a id="CreateIndexer"></a>Vaihe 3: Luo indeksointitoiminto

Voit luoda uuden indeksointitoiminto Azure Search-palvelun käyttämällä HTTP POST-pyynnön seuraavat otsikot.

    POST https://[Search service name].search.windows.net/indexers?api-version=[api-version]
    Content-Type: application/json
    api-key: [Search service admin key]

Pyynnön leipätekstissä indeksointitoiminto määritelmää, joihin sisältyvät seuraavat kentät:

- **nimi**: pakollinen. Indeksointitoiminto nimi.

- **Tietolähdeyhteyden**: pakollinen. Aiemmin luotuun tietolähteeseen nimi.

- **targetIndexName**: pakollinen. Aiemmin luodun indeksin nimi.

- **aikataulun**: valinnainen. Katso [indeksoinnin aikataulun](#IndexingSchedule) alla.

###<a id="IndexingSchedule"></a>Käynnissä Indeksoijilla aikataulun

Indeksointitoiminto voit myös määrittää aikataulun. Jos aikataulun ei sisällä tietoja, indeksointitoiminto suoritetaan säännöllisesti aikataulun mukaan. Aikataulun on määritteet:

- **aikavälin**: pakollinen. Kesto-arvo, joka määrittää välin tai kauden indeksointitoiminto suoritetaan. Pienin sallittu aikaväli on 5 minuuttia. Pisin haara on yksi päivä. Se on muotoiltu XSD "dayTimeDuration" arvo (rajoitettu alijoukko [ISO 8601 kesto](http://www.w3.org/TR/xmlschema11-2/#dayTimeDuration) -arvo). Tämä kaava on: `P(nD)(T(nH)(nM))`. Esimerkkejä: `PT15M` 15 minuutin välein `PT2H` , kahden tunnin välein.

- **aloitusajan**: pakollinen. UTC-ja, joka määrittää, milloin indeksointitoiminto aloitetaan käynnissä.

###<a id="CreateIndexerExample"></a>Pyydä leipäteksti-Esimerkki

Seuraavassa esimerkissä luodaan indeksointitoiminto, kopioi tiedot käyttämät sivustokokoelman `myDocDbDataSource` tietolähdettä `mySearchIndex` indeksi aikataulun, joka alkaa 1 tammi 2015 UTC-ajan ja suorittaa kerran tunnissa.

    {
        "name" : "mysearchindexer",
        "dataSourceName" : "mydocdbdatasource",
        "targetIndexName" : "mysearchindex",
        "schedule" : { "interval" : "PT1H", "startTime" : "2015-01-01T00:00:00Z" }
    }

###<a name="response"></a>Vastaus

Saat HTTP 201 luotu vastausta, jos indeksointitoiminto on luotu.

##<a id="RunIndexer"></a>Vaihe 4: Suorita indeksointitoiminto

Lisäksi on säännöllisesti aikataulun, indeksointitoiminto myös voidaan kutsua pyydettäessä lähettämällä HTTP POST-pyynnössä:

    POST https://[Search service name].search.windows.net/indexers/[indexer name]/run?api-version=[api-version]
    api-key: [Search service admin key]

###<a name="response"></a>Vastaus

Näyttöön tulee HTTP 202 hyväksytty vastausta, jos indeksointitoiminto on kutsuttu onnistuneesti.

##<a name="GetIndexerStatus"></a>Vaihe 5: Get indeksointitoiminnon tila

Voit myöntää HTTP GET-pyyntö hakea nykyisen tilan ja suorittamisen historia indeksointitoiminto:

    GET https://[Search service name].search.windows.net/indexers/[indexer name]/status?api-version=[api-version]
    api-key: [Search service admin key]

###<a name="response"></a>Vastaus

Näkyviin tulee vastaus tekstissä, joka sisältää tietoja yleinen indeksointitoiminto kunto tilan, viimeisen indeksointitoiminto kutsu sekä viimeisimmät indeksointitoiminto ohjelmarakennekaaviossa historiatiedot (Jos löytyy) sekä palauttaa HTTP 200 OK vastausta.

Vastauksen pitäisi näyttää seuraavankaltaiselta:

    {
        "status":"running",
        "lastResult": {
            "status":"success",
            "errorMessage":null,
            "startTime":"2014-11-26T03:37:18.853Z",
            "endTime":"2014-11-26T03:37:19.012Z",
            "errors":[],
            "itemsProcessed":11,
            "itemsFailed":0,
            "initialTrackingState":null,
            "finalTrackingState":null
         },
        "executionHistory":[ {
            "status":"success",
             "errorMessage":null,
            "startTime":"2014-11-26T03:37:18.853Z",
            "endTime":"2014-11-26T03:37:19.012Z",
            "errors":[],
            "itemsProcessed":11,
            "itemsFailed":0,
            "initialTrackingState":null,
            "finalTrackingState":null
        }]
    }

Suorittamisen historia on enintään 50 viimeisimmän valmiit suorituskerran, joka on lajiteltu käänteisessä aikajärjestyksessä (jotta uusimman suorittamisen on ensin vastauksessa).

##<a name="NextSteps"></a>Seuraavat vaiheet

Onnittelen! Opit on vain, miten voit integroida Azure DocumentDB indeksointitoiminto käytön DocumentDB Azure-haulla.

 - Lisätietoja siitä, miten Azure DocumentDB on artikkelissa [DocumentDB palvelu‑sivulla](https://azure.microsoft.com/services/documentdb/).

 - Miten lisätietoja Azure haku-kohdassa [haun palvelun sivulle](https://azure.microsoft.com/services/search/).
