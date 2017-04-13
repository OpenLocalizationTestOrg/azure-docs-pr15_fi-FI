<properties
    pageTitle="Oman REST-Ohjelmointirajapinnan käyttäminen Azure-hakuindeksin kyselyn | Microsoft Azure | Isännöityjen pilvipalvelussa haku"
    description="Muodosta Azure hakutoiminnossa hakukyselyn ja Etsi parametreilla, suodattaminen ja lajitteleminen hakutulokset."
    services="search"
    documentationCenter=""
    manager="jhubbard"
    authors="ashmaka"
/>

<tags
    ms.service="search"
    ms.devlang="na"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="ashmaka"/>

# <a name="query-your-azure-search-index-using-the-rest-api"></a>Kyselyn Azure etsinnän indeksiin REST-Ohjelmointirajapinnan käyttäminen
> [AZURE.SELECTOR]
- [Yleiskatsaus](search-query-overview.md)
- [Portal](search-explorer.md)
- [.NET](search-query-dotnet.md)
- [MUILLE KÄYTTÄJILLE](search-query-rest-api.md)

Tässä artikkelissa kerrotaan, kuinka kyselyn indeksin [Azure-haun REST-Ohjelmointirajapinta](https://msdn.microsoft.com/library/azure/dn798935.aspx)käyttäminen.

Ennen aloittamista tätä vaiheittaista pitäisi olla jo [Azure-hakuindeksin luodaan](search-what-is-an-index.md) ja [täytetään sen tiedoilla](search-what-is-data-import.md).

## <a name="i-identify-your-azure-search-services-query-api-key"></a>Voin. Määritä Azure Search-palvelun kyselyn api-avain
Jokaisen hakutoiminto vastaan Azure haun REST-Ohjelmointirajapinta avaimen osa on *api-näppäintä* , joka on luotu palvelun voit valmistelun yhteydessä. Salli kohti pyynnön välein, lähettää pyynnön ja -palvelu, joka käsittelee sen sovelluksen välillä on kelvollinen avain vahvistetaan.

1. Etsi oman service api-näppäimiä sinun on kirjauduttava [Azure-portaalissa](https://portal.azure.com/)
2. Siirry Azure Search-palvelun sivu
3. Valitse "Näppäimet"-kuvake

Palvelussa on *järjestelmänvalvojan näppäimet* ja *kysely-näppäimiä*.

 - Ensisijaisen ja toissijaisen *järjestelmänvalvojan näppäimet* myöntää kaikki toiminnot, mukaan lukien pätevyys hallita palvelua, luominen ja poistaminen indeksit Indeksoijilla ja tietolähteiden täydet oikeudet. On kaksi näppäimet, jotta voit edelleen käyttää toissijaisen avaimen, jos uudelleen perusavain ja päinvastoin.
 - *Kyselyn näppäimet* myöntää indeksit ja tiedostojen käyttöoikeus vain luku- ja jaetaan yleensä asiakassovelluksiin, joka antaa search-pyyntöjen.

Kyselyt indeksin tarkoitetaan jollakin kysely-näppäimiä. Järjestelmänvalvoja-näppäimiä voidaan myös kyselyjen, mutta sinun on käytettävä kyselyn avain sovelluksen koodissa seuraavasti tämä paremmin [käyttöoikeuksien myöntäminen tarpeen mukaan](https://en.wikipedia.org/wiki/Principle_of_least_privilege).

## <a name="ii-formulate-your-query"></a>II. Kyselyn muodostetaan
Voit [etsiä REST-Ohjelmointirajapinnan käyttäminen indeksi](https://msdn.microsoft.com/library/azure/dn798927.aspx)kahdella eri tavalla. Yksi tapa on HTTP POST-pyynnön missä kyselyparametrit määritellään JSON-objektin pyynnön tekstissä. Muut tapa on HTTP GET-pyynnön missä kyselyparametrit määritetään sisällä pyyntö URL-osoite. Huomaa, että viesti on enemmän [katsottu rajoitukset](https://msdn.microsoft.com/library/azure/dn798927.aspx) kyselyparametrit kuin GET koon. Tästä syystä suosittelemme käyttämällä kirjaa, ellei sinulla ole erityiset tilanteet, joissa käyttämällä HAE olisi tarkoituksenmukaista.

Kirjaa ja HAE sinun on annettava *palvelunimi*, *Indeksinimi*ja ERISNIMI *API-versio* (API nykyistä versiota ei `2015-02-28` julkaisemisesta tämän asiakirjan aikaa) pyyntö URL-osoitteen. GET- *kyselymerkkijonon* URL-Osoitteen lopussa on annetaan kyselyparametrit. Alla on URL-muoto:

    https://[service name].search.windows.net/indexes/[index name]/docs?[query string]&api-version=2015-02-28

VIESTIN muoto on sama, mutta vain api-version merkkijono kyselyparametrit kanssa.



#### <a name="example-queries"></a>Esimerkki kyselyt

Seuraavassa on muutama Esimerkki kysely hakemiston nimeltä "Hotellit". Nämä kyselyt näkyvät GET-ja JULKAISE.

Hakea koko hakemistosta termin "budjetin" ja palauttaa vain `hotelName` kentän:

```
GET https://[service name].search.windows.net/indexes/hotels/docs?search=budget&$select=hotelName&api-version=2015-02-28

POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2015-02-28
{
    "search": "budget",
    "select": "hotelName"
}
```

Suodattimen etsiminen hotellit halvempia 150 euroa yöllä ja palauttaa indeksi `hotelId` ja `description`:

```
GET https://[service name].search.windows.net/indexes/hotels/docs?search=*&$filter=baseRate lt 150&$select=hotelId,description&api-version=2015-02-28

POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2015-02-28
{
    "search": "*",
    "filter": "baseRate lt 150",
    "select": "hotelId,description"
}
```

Hakea koko hakemistosta tilauksen tietyn kentän mukaan (`lastRenovationDate`) laskevaan järjestykseen kestää kaksi parasta tulokset, ja näyttää vain `hotelName` ja `lastRenovationDate`:

```
GET https://[service name].search.windows.net/indexes/hotels/docs?search=*&$top=2&$orderby=lastRenovationDate desc&$select=hotelName,lastRenovationDate&api-version=2015-02-28

POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2015-02-28
{
    "search": "*",
    "orderby": "lastRenovationDate desc",
    "select": "hotelName,lastRenovationDate",
    "top": 2
}
```

## <a name="iii-submit-your-http-request"></a>III. Lähetä HTTP-pyyntö
Nyt kun olet muotoilla kyselyn osana HTTP pyyntö URL-osoite (HAE) tai leipäteksti (varten Kirjaa), voit määrittää pyynnön otsikot ja lähettää kyselyn.

#### <a name="request-and-request-headers"></a>Pyyntö ja pyynnön otsikot
HAE tai Kirjaa kolmen kaksi pyynnön otsikot on määritettävä:
1. `api-key` Otsikko on määritettävä löytämäsi vaiheessa voin yllä kysely-näppäintä. Huomaa, että voit käyttää myös järjestelmänvalvoja-avain kuin `api-key` otsikkoa, mutta se on suositeltavaa, että voit käyttää kyselyn avain myöntää ainoastaan vain luku-indeksit ja tiedostojen käyttöoikeus.
2. `Accept` Otsikko on määritettävä `application/json`.
3. Saat viestin, `Content-Type` otsikko on myös määritettävä `application/json`.

Noudata seuraavia ohjeita voit hakea käyttämällä Azure haun REST-Ohjelmointirajapinta, käyttämällä yksinkertaisen kyselyn, joka hakee termin "oppaalla" "Hotellit" hakemistosta HTTP GET-pyynnössä:

```
GET https://[service name].search.windows.net/indexes/hotels/docs?search=motel&api-version=2015-02-28
Accept: application/json
api-key: [query key]
```

Tässä on esimerkki saman kyselyn tällä kertaa käyttäen HTTP POST:

```
POST https://[service name].search.windows.net/indexes/hotels/docs/search?api-version=2015-02-28
Content-Type: application/json
Accept: application/json
api-key: [query key]

{
    "search": "motel"
}
```

Onnistunut kysely aiheuttaa tilakoodin `200 OK` ja etsinnän tulokset palautetaan JSON vastauksen tekstissä. Seuraavassa on tietoja tulokset yllä kyselyn ulkoasua, kuten olettaen "Hotellit" hakemiston lisätään esimerkkitiedot [Azure-haun REST-Ohjelmointirajapinnan käyttäminen tietojen tuontia](search-import-data-rest-api.md) (Huomautus JSON muotoiltu selkeyttä varten).

```JSON
{
    "value": [
        {
            "@search.score": 0.59600675,
            "hotelId": "2",
            "baseRate": 79.99,
            "description": "Cheapest hotel in town",
            "description_fr": "Hôtel le moins cher en ville",
            "hotelName": "Roach Motel",
            "category": "Budget",
            "tags":["motel", "budget"],
            "parkingIncluded": true,
            "smokingAllowed": true,
            "lastRenovationDate": "1982-04-28T00:00:00Z",
            "rating": 1,
            "location": {
                "type": "Point",
                "coordinates": [-122.131577, 49.678581],
                "crs": {
                    "type":"name",
                    "properties": {
                        "name": "EPSG:4326"
                    }
                }
            }
        }
    ]
}
```

Katso lisätietoja seuraavasta [Etsi asiakirjoista](https://msdn.microsoft.com/library/azure/dn798927.aspx)"Vastauksen"-osa. Saat lisätietoja muiden HTTP-tilan koodit, joita voi palauttaa virheen, jos [HTTP-tilakoodin (Azure haku)](https://msdn.microsoft.com/library/azure/dn798925.aspx).
