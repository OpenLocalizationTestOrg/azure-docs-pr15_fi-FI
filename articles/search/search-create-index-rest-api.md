<properties
    pageTitle="Luo REST-Ohjelmointirajapinnan käyttäminen Azure haun indeksi | Microsoft Azure | Isännöityjen pilvipalvelussa haku"
    description="Indeksin luominen koodissa käyttämällä Azure haun HTTP REST-Ohjelmointirajapinta."
    services="search"
    documentationCenter=""
    authors="ashmaka"
    manager="jhubbard"
    editor=""
    tags="azure-portal"/>

<tags
    ms.service="search"
    ms.devlang="rest-api"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="ashmaka"/>

# <a name="create-an-azure-search-index-using-the-rest-api"></a>REST-Ohjelmointirajapinnan käyttäminen Azure haun indeksin luominen
> [AZURE.SELECTOR]
- [Yleiskatsaus](search-what-is-an-index.md)
- [Portal](search-create-index-portal.md)
- [.NET](search-create-index-dotnet.md)
- [MUILLE KÄYTTÄJILLE](search-create-index-rest-api.md)


Tässä artikkelissa opastaa luominen käyttämällä Azure haun REST-Ohjelmointirajapinta Azure haun [indeksi](https://msdn.microsoft.com/library/azure/dn798941.aspx) .

Ennen tämän oppaan seuraavan ja indeksin, sinulla on oltava jo [luotu Azure-hakupalvelun](search-create-service-portal.md).

REST-Ohjelmointirajapinnan käyttäminen Azure haun indeksin luomiseen lähetät yksittäisen HTTP POST pyynnön Azure Search-palvelun URL-Osoitteen päätepiste. Indeksimääritys sisälsi pyynnön teksti on muotoiltu JSON sisältö.


## <a name="i-identify-your-azure-search-services-admin-api-key"></a>Voin. Määritä Azure Search-palvelun järjestelmänvalvoja api-näppäin
Nyt kun olet valmisteltu Azure-hakupalvelun, voit myöntää pyyntöjen vastaan yhteyttä palvelun URL-Osoitteen päätepisteen REST-Ohjelmointirajapinnan käyttäminen. Kuitenkin *kaikki* API pyynnöt täytyy sisältää api-näppäintä, joka on luotu hakupalvelun voit valmistelun yhteydessä. Salli kohti pyynnön välein, lähettää pyynnön ja -palvelu, joka käsittelee sen sovelluksen välillä on kelvollinen avain vahvistetaan.

1. Etsi oman service api-näppäimiä sinun on kirjauduttava [Azure-portaalissa](https://portal.azure.com/)
2. Siirry Azure Search-palvelun sivu
3. Valitse "Näppäimet"-kuvake

Palvelussa on *järjestelmänvalvojan näppäimet* ja *kysely-näppäimiä*.

 - Ensisijaisen ja toissijaisen *järjestelmänvalvojan näppäimet* myöntää kaikki toiminnot, mukaan lukien pätevyys hallita palvelua, luominen ja poistaminen indeksit Indeksoijilla ja tietolähteiden täydet oikeudet. On kaksi näppäimet, jotta voit edelleen käyttää toissijaisen avaimen, jos uudelleen perusavain ja päinvastoin.
 - *Kyselyn näppäimet* myöntää indeksit ja tiedostojen käyttöoikeus vain luku- ja jaetaan yleensä asiakassovelluksiin, joka antaa search-pyyntöjen.

Hakemiston luominen yhdistämiskyselyssä, voit käyttää joko ensisijaisen tai toissijaisen järjestelmänvalvojan-näppäintä.

## <a name="ii-define-your-azure-search-index-using-well-formed-json"></a>II. Määrittää, että käyttämällä muotoiltu JSON Azure hakuindeksin
Yksittäisen HTTP POST pyynnön palveluun Luo indeksi. HTTP POST-pyynnössä leipätekstiin sisältää yksittäisen JSON-objekti, joka määrittää Azure-hakuindeksin.

1. Ensimmäiseen ominaisuuteen, JSON-objekti on indeksi nimi.
2. JSON objektin toinen ominaisuus on nimetty JSON-matriisin `fields` , joka sisältää indeksi kunkin kentän erillinen JSON objekti. Näiden JSON-objektien sisältää useita nimen ja arvon tietoparin kunkin kentän attribuuteista, mukaan lukien "nimi", "tyyppi", jne.

On tärkeää, että pidät Etsi käyttäjä kokemus ja business tarpeitasi mielessä suunniteltaessa indeksi, kun kunkin kentän on määritetty [oikea määritteet](https://msdn.microsoft.com/library/azure/dn798941.aspx). Kentät, jotka koskevat nämä määritteet määrittää, mitkä Etsi ominaisuuksia (suodattamista, faceting, lajittelua tekstimuotoisen etsimisen, jne.). Mikä tahansa määrite ei määritetä oletusarvo on käyttöön vastaavan hakutoimintoa, ellet nimenomaisesti käytöstä.

Tässä esimerkissä on olet nimeltä indeksi "Hotellit" ja Microsoftin kentät määritellään seuraavasti:

```JSON
{
    "name": "hotels",  
    "fields": [
        {"name": "hotelId", "type": "Edm.String", "key": true, "searchable": false, "sortable": false, "facetable": false},
        {"name": "baseRate", "type": "Edm.Double"},
        {"name": "description", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false},
        {"name": "description_fr", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false, "analyzer": "fr.lucene"},
        {"name": "hotelName", "type": "Edm.String", "facetable": false},
        {"name": "category", "type": "Edm.String"},
        {"name": "tags", "type": "Collection(Edm.String)"},
        {"name": "parkingIncluded", "type": "Edm.Boolean", "sortable": false},
        {"name": "smokingAllowed", "type": "Edm.Boolean", "sortable": false},
        {"name": "lastRenovationDate", "type": "Edm.DateTimeOffset"},
        {"name": "rating", "type": "Edm.Int32"},
        {"name": "location", "type": "Edm.GeographyPoint"}
    ]
}
```

Microsoft on valinnut huolellisesti kunkin kentän mukaan, miten on mielestäsi niitä käytetään sovelluksen indeksimääritteet. Esimerkiksi `hotelId` on yksilöivä tunnus haussa hotellit todennäköisesti eivät tiedä, jotta päivitystoiminto poistuu käytöstä teksti-kenttään hakeminen määrittämällä ihmiset `searchable` , `false`, mikä säästää levytilaa hakemistoon.

Huomautus täsmälleen yhden indeksi-tyypin kentässä `Edm.String` on oltava nimettyjen 'avain'-kentässä.

Yllä indeksin määrityksen käyttää mukautettuja kieli-analyzer varten `description_fr` kenttää, koska se on tarkoitus tallentaa Ranskan tekstiä. Katso [tukevat aiheessa MSDN-sivuston kieli](https://msdn.microsoft.com/library/azure/dn879793.aspx) sekä vastaavan [blogimerkintä](https://azure.microsoft.com/blog/language-support-in-azure-search/) lisätietoja kielen analyzers.

## <a name="iii-issue-the-http-request"></a>III. HTTP-pyynnön
1. Käytä indeksimääritys pyynnön tekstinä, antaa HTTP POST-pyynnön Azure haun palvelun päätepisteen URL-osoite. URL-osoite, varmista, että voit käyttää palvelunimi isäntänimi ja siirtää oikeat `api-version` merkkijonon parametrina (API nykyistä versiota ei `2015-02-28` ajankohtana julkaistaan tämä asiakirja).
2. Pyyntö otsikot, Määritä `Content-Type` kuin `application/json`. Sinun on myös antaa yhteyttä palvelun järjestelmänvalvoja avainta, joka tunnistaa i vaihe `api-key` otsikko.


Sinun on annettava antaa alla pyynnön oma palvelun nimi ja ohjelmointirajapinnan avain:


    POST https://[service name].search.windows.net/indexes?api-version=2015-02-28
    Content-Type: application/json
    api-key: [api-key]


Onnistuneiden pyynnön pitäisi näkyä tilakoodin 201 (luotu). Lisätietoja indeksin REST-Ohjelmointirajapinnalla kautta käy [MSDN](https://msdn.microsoft.com/library/azure/dn798941.aspx)-Ohjelmointirajapinnan viittaus. Saat lisätietoja muiden HTTP-tilan koodit, joita voi palauttaa virheen, jos [HTTP-tilakoodin (Azure haku)](https://msdn.microsoft.com/library/azure/dn798925.aspx).

Kun enää tarvitse indeksin ja haluat poistaa sen, Lähetä HTTP poistaa pyynnön. Tämä on esimerkiksi kuinka on poistettava "Hotellit" indeksi:

    DELETE https://[service name].search.windows.net/indexes/hotels?api-version=2015-02-28
    api-key: [api-key]


## <a name="next"></a>Seuraava
Kun olet luonut Azure-hakuindeksin, voi haluat [ladata sisältöä hakemiston kyselyjä](search-what-is-data-import.md) , voit ryhtyä tietojen etsimistä.
