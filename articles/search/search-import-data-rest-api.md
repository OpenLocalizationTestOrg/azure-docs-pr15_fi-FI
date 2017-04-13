<properties
    pageTitle="Tietojen lataaminen Azure-haun REST-Ohjelmointirajapinnan käyttäminen | Microsoft Azure | Isännöityjen pilvipalvelussa haku"
    description="Tietoja tietojen lataaminen hakemiston Azure-haun REST-Ohjelmointirajapinnan käyttäminen."
    services="search"
    documentationCenter=""
    authors="ashmaka"
    manager="jhubbard"
    editor=""
    tags=""/>

<tags
    ms.service="search"
    ms.devlang="rest-api"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="ashmaka"/>

# <a name="upload-data-to-azure-search-using-the-rest-api"></a>Tietojen lataaminen Azure-haun REST-Ohjelmointirajapinnan käyttäminen
> [AZURE.SELECTOR]
- [Yleiskatsaus](search-what-is-data-import.md)
- [.NET](search-import-data-dotnet.md)
- [MUILLE KÄYTTÄJILLE](search-import-data-rest-api.md)

Tässä artikkelissa kerrotaan, miten [Azure-haun REST-Ohjelmointirajapinta](https://msdn.microsoft.com/library/azure/dn798935.aspx) avulla voit tuoda tietoja Azure-hakuindeksin.

Ennen aloittamista tätä vaiheittaista on jo [luotu Azure-hakuindeksin](search-what-is-an-index.md).

Jotta push tiedostojen siirtäminen REST-Ohjelmointirajapinnan käyttäminen indeksi-lähetät HTTP POST-pyynnön oman indeksi URL-Osoitteen päätepiste. HTTP-pyyntö leipätekstin teksti on JSON-objektit, joiden voidaan lisätä, muokata ja poistaa asiakirjat.

## <a name="i-identify-your-azure-search-services-admin-api-key"></a>Voin. Määritä Azure Search-palvelun järjestelmänvalvoja api-näppäin
Kun pyyntöjen REST-Ohjelmointirajapinnan käyttäminen palvelun vastaan, *kunkin* API pyynnön on sisällettävä api-näppäintä, joka on luotu hakupalvelun voit valmistelun yhteydessä. Salli kohti pyynnön välein, lähettää pyynnön ja -palvelu, joka käsittelee sen sovelluksen välillä on kelvollinen avain vahvistetaan.

1. Etsi oman service api-näppäimiä sinun on kirjauduttava [Azure-portaalissa](https://portal.azure.com/)
2. Siirry Azure Search-palvelun sivu
3. Valitse "Näppäimet"-kuvake

Palvelussa on *järjestelmänvalvojan näppäimet* ja *kysely-näppäimiä*.

  - Ensisijaisen ja toissijaisen *järjestelmänvalvojan näppäimet* myöntää kaikki toiminnot, mukaan lukien pätevyys hallita palvelua, luominen ja poistaminen indeksit Indeksoijilla ja tietolähteiden täydet oikeudet. On kaksi näppäimet, jotta voit edelleen käyttää toissijaisen avaimen, jos uudelleen perusavain ja päinvastoin.
  - *Kyselyn näppäimet* myöntää indeksit ja tiedostojen käyttöoikeus vain luku- ja jaetaan yleensä asiakassovelluksiin, joka antaa search-pyyntöjen.

Tietojen tuominen indeksin tarkoituksiin, voit käyttää joko ensisijaisen tai toissijaisen järjestelmänvalvojan-näppäintä.

## <a name="ii-decide-which-indexing-action-to-use"></a>II. Päättää, mitä indeksoinnin toiminnon käyttäminen
REST-Ohjelmointirajapinnalla käytettäessä voit antaa HTTP POST pyynnöt ja laitokset JSON-pyyntö Azure-in-hakuindeksin päätepisteen URL-osoitteeseen. HTTP-pyyntö tekstissä JSON-objekti sisältää yksittäisen JSON-matriisin nimeltä "arvo" sisältää JSON-objekteja, jotka haluat lisätä hakemiston, Päivitä tai poista tiedostot.

JSON-objekteja, "arvo"-matriisissa on asiakirjan voi indeksoida. Näiden objektien sisältää asiakirjan avaimen ja määrittää haluamasi indeksoinnin toiminnon (Lataa, Yhdistä, poista jne.). Sen mukaan, mitä alapuolella toiminnot valitset, vain tietyt kentät on oltava kunkin asiakirjan:

@search.action | Kuvaus | Tarvittavat kunkin asiakirjan kentät | Huomautuksia
--- | --- | --- | ---
`upload` | `upload` Toiminto on sama kuin "upsert" kohtaa, johon tiedosto lisätään, jos se on uusi ja päivittää tai korvata jos se on olemassa. | avaimen sekä muita kenttiä, jonka haluat määrittää | Korvattaessa päivittäminen tai aiemmin luodun tiedoston kentän, joka ei ole määritetty pyyntö on sen-kentän arvoksi `null`. Tämä tapahtuu myös silloin, kun kenttä on aiemmin määritetty muut kuin null-arvoa.
`merge` | Päivittää määritettyihin kenttiin aiemmin luotu tiedosto. Jos tiedosto ei ole indeksissä, yhdistäminen epäonnistuu. | avaimen sekä muita kenttiä, jonka haluat määrittää | Määrittämäsi yhdistämiseen kentän korvaa olemassa olevan kentän asiakirjassa. Tämä vaihtoehto sisältää-tyypin kenttiä `Collection(Edm.String)`. Jos asiakirjassa on kentän esimerkiksi `tags` arvona `["budget"]` ja arvona yhdistämisen suorittaminen `["economy", "pool"]` varten `tags`, viimeinen arvo `tags` kenttä on `["economy", "pool"]`. Se ei ole `["budget", "economy", "pool"]`.
`mergeOrUpload` | Tämä toiminto toimii kuten `merge` indeksissä aiemmin luodun asiakirjan tietyn-näppäimen kanssa. Jos tiedosto ei ole, se toimii kuten `upload` ja uuden asiakirjan. | avaimen sekä muita kenttiä, jonka haluat määrittää | -
`delete` | Poistaa määritetyn asiakirjan indeksi. | vain avain | Kaikki kentät, voit määrittää muut kuin avaimen kentän ohitetaan. Jos haluat poistaa yksittäisen kentän asiakirjasta, käytä `merge` sijaan ja yksinkertaisesti kentän arvoksi erikseen null.

## <a name="iii-construct-your-http-request-and-request-body"></a>III. Käyttää HTTP-pyynnön ja pyydä tekstissä
Nyt kun olet kerännyt tarvittavat kenttäarvojen indeksi-toiminnot, olet valmis muodostaa todellinen HTTP-pyynnön ja JSON pyynnön leipätekstin tiedot.

#### <a name="request-and-request-headers"></a>Pyyntö ja pyynnön otsikot
URL-osoite, tarvitset Anna palvelunimi, Indeksinimi ("Hotellit" tässä tapauksessa) sekä ERISNIMI API-versio (API nykyistä versiota ei `2015-02-28` ajankohtana julkaistaan tämä asiakirja). Voit määrittää `Content-Type` ja `api-key` pyytää otsikot. Jälkimmäisessä käyttää useita yhteyttä palvelun järjestelmänvalvoja-näppäimiä.

    POST https://[search service].search.windows.net/indexes/hotels/docs/index?api-version=2015-02-28
    Content-Type: application/json
    api-key: [admin key]

#### <a name="request-body"></a>Pyydä tekstissä

```JSON
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
            "@search.action": "mergeOrUpload",
            "hotelId": "3",
            "baseRate": 129.99,
            "description": "Close to town hall and the river"
        },
        {
            "@search.action": "delete",
            "hotelId": "6"
        }
    ]
}
```

Tässä tapauksessa emme käyttävät `upload`, `mergeOrUpload`, ja `delete` Microsoftin haun toimenpiteitä.

Oletetaan, että tämän esimerkin "Hotellit" indeksin lisätään jo asiakirjojen määrä. Huomaa, miten Microsoft ei tarvitse määrittää kaikki mahdolliset asiakirjakentät käytettäessä `mergeOrUpload` ja miten on vain määritetty asiakirjan key (`hotelId`) käytettäessä `delete`.

Huomaa myös, yksi indeksoinnin pyynnön, että voit sisällyttää vain enintään 1 000 asiakirjat (tai 16 Mt).

## <a name="iv-understand-your-http-response-code"></a>IV. HTTP-vastauksen koodin ymmärtäminen
#### <a name="200"></a>200
Kun olet lähettänyt onnistuneen indeksoinnin pyyntö HTTP-vastauksen tilakoodi saavat `200 OK`. HTTP-vastaus JSON leipätekstiin on seuraavasti:

```JSON
{
    "value": [
        {
            "key": "unique_key_of_document",
            "status": true,
            "errorMessage": null
        },
        ...
    ]
}
```

#### <a name="207"></a>207
Tilakoodin `207` palautetaan, jos vähintään yksi kohde ei onnistunut indeksoitu. HTTP-vastaus JSON leipätekstiin sisältää tietoja epäonnistua asiakirjat.

```JSON
{
    "value": [
        {
            "key": "unique_key_of_document",
            "status": false,
            "errorMessage": "The search service is too busy to process this document. Please try again later."
        },
        ...
    ]
}
```

> [AZURE.NOTE] Tämä tarkoittaa yleensä search-palvelun kuormitus on kurottavat kohta, johon palauttaa alkaa indeksoinnin pyynnöt `503` vastaukset. Syy tässä tapauksessa erittäin suositeltavaa, että asiakas-koodin takaisin käytöstä ja odotettava, ennen kuin. Näin järjestelmän jonkin aikaa palauttamaan, lisääntyvien mahdollisuuksia, jotka tulevat pyynnöt onnistuu. Yritetään nopeasti pyyntöjen vain pidentää tilanne.

#### <a name="429"></a>429
Tilakoodin `429` indeksiä kohti tiedostojen määrän kiintiö on ylitetty palautetaan.

#### <a name="503"></a>503
Tilakoodin `503` palautetaan, jos mikään pyynnön kohteita ei onnistunut indeksoitu. Tämä virhe tarkoittaa, että järjestelmä on kuormitettu pyyntö ei voi käsitellä tällä hetkellä.

> [AZURE.NOTE] Syy tässä tapauksessa erittäin suositeltavaa, että asiakas-koodin takaisin käytöstä ja odotettava, ennen kuin. Näin järjestelmän jonkin aikaa palauttamaan, lisääntyvien mahdollisuuksia, jotka tulevat pyynnöt onnistuu. Yritetään nopeasti pyyntöjen vain pidentää tilanne.

Lisätietoja Asiakirjatoimet ja onnistuminen ja vastauksia on artikkelissa [lisääminen, päivittäminen, tai poista asiakirjat](https://msdn.microsoft.com/library/azure/dn798930.aspx). Saat lisätietoja muiden HTTP-tilan koodit, joita voi palauttaa virheen, jos [HTTP-tilakoodin (Azure haku)](https://msdn.microsoft.com/library/azure/dn798925.aspx).

## <a name="next"></a>Seuraava
Kun täyttää Azure-hakuindeksin ovat valmiina aloittamaan varmenteiden kyselyitä, voit etsiä tiedostoja. [Kyselyn Your Azure-hakuindeksin](search-query-overview.md) lisätietoja.
