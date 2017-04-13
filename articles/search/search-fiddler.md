<properties
    pageTitle="Arvioi ja testaa Azure haun REST API Fiddler avulla | Microsoft Azure | Isännöityjen pilvipalvelussa haku"
    description="Käytä Fiddler koodin vapaa lähestymistavan Azure haun käytettävyyden tarkistaminen ja kokeilla REST API."
    services="search"
    documentationCenter=""
    authors="HeidiSteen"
    manager="mblythe"
    editor=""/>

<tags
    ms.service="search"
    ms.devlang="rest-api"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="10/17/2016"
    ms.author="heidist"/>

# <a name="use-fiddler-to-evaluate-and-test-azure-search-rest-apis"></a>Fiddler avulla voit arvioida ja Azure haun REST API testaaminen
> [AZURE.SELECTOR]
- [Yleiskatsaus](search-query-overview.md)
- [Erikoishaku](search-explorer.md)
- [Fiddler](search-fiddler.md)
- [.NET](search-query-dotnet.md)
- [MUILLE KÄYTTÄJILLE](search-query-rest-api.md)

Tässä artikkelissa kerrotaan, miten voit käyttää Fiddler [Telerik ladata ilmaiseksi](http://www.telerik.com/fiddler), ongelma HTTP pyynnöt ja Näytä vastaukset käyttämällä Azure haun REST-Ohjelmointirajapinta, eikä sinun tarvitse kirjoittaa koodia. Azure haku on täysin rajoitetun, isännöidään haun pilvipalvelussa Microsoft Azure-helposti ohjelmoitava .NET ja REST API. Azure-hakupalvelun REST API on kuvattu [MSDN](https://msdn.microsoft.com/library/azure/dn798935.aspx)-sivuston.

Seuraavissa vaiheissa indeksin luominen, tiedostojen lataaminen, kysely indeksi ja kyselyn palvelutiedot järjestelmä.

Näiden vaiheiden suorittamiseen tarvitset Azure-hakupalvelun ja `api-key`. Katso ohjeet aloittamaan [luominen Azure Search-palvelun portaalissa](search-create-service-portal.md) .

## <a name="create-an-index"></a>Indeksin luominen

1. Aloita Fiddler. **Tiedosto** -valikosta käytöstä **Siepata liikenne** Piilota ylimääräiset HTTP-toiminto, joka ei vaikuta siihen, nykyisen tehtävän.

3. **Tunnus** -välilehdessä voit muotoilla asentaa pyynnön, joka näyttää seuraavalta ruudulta-koodin.

    ![][1]

2. Valitse **Aseta**.

3. Kirjoita URL-osoite, joka määrittää URL-osoite ja pyynnön määritteitä api-versio. Kannattaa pitää mielessä muutama osoittimet:
   + Määritä HTTPS etuliitteen.
   + Pyyntö-määrite on "/ indeksit/hotellit". Tämä kertoo haun indeksin nimeltä 'hotellit' luomiseen.
   + API-versio on pieni, määritettyä "? api-version 2015-02-28 =". API versioita ovat tärkeitä, sillä Azure haun ottaa käyttöön päivitykset säännöllisesti. Harvinaisten joissakin tapauksissa service-päivitys voivat esitellä Ohjelmointirajapinnan jakautumisen muuttaminen. Tästä syystä Azure haku edellyttää on api-versio kunkin pyynnön niin, että käytössä on yli yhtä käytetään täydet oikeudet.

    Seuraavassa esimerkissä pitäisi muistuttaa täydellinen URL-osoite.

            https://my-app.search.windows.net/indexes/hotels?api-version=2015-02-28

4.  Määritä pyynnön otsikko-ohjelmointirajapinnan avain ja host tilalle arvoja, jotka ovat kelvollisia palvelun.

            User-Agent: Fiddler
            host: my-app.search.windows.net
            content-type: application/json
            api-key: 1111222233334444

5.  Liitä pyytää leipätekstiin kentistä, jotka muodostavat indeksin määrityksen.
            
             {
            "name": "hotels",  
            "fields": [
              {"name": "hotelId", "type": "Edm.String", "key":true, "searchable": false},
              {"name": "baseRate", "type": "Edm.Double"},
              {"name": "description", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false},
              {"name": "hotelName", "type": "Edm.String"},
              {"name": "category", "type": "Edm.String"},
              {"name": "tags", "type": "Collection(Edm.String)"},
              {"name": "parkingIncluded", "type": "Edm.Boolean"},
              {"name": "smokingAllowed", "type": "Edm.Boolean"},
              {"name": "lastRenovationDate", "type": "Edm.DateTimeOffset"},
              {"name": "rating", "type": "Edm.Int32"},
              {"name": "location", "type": "Edm.GeographyPoint"}
             ]
            }

6.  Valitse **Suorita**.

Muutaman sekunnin pitäisi näkyä HTTP 201-vastauksen istunnon-luettelosta, joka ilmaisee indeksin luominen onnistui.

Jos HTTP 504, Tarkista URL-osoite määrittää HTTPS. Jos näet HTTP 400 tai 404, tarkista pyynnön tekstiin vahvistamiseksi virheitä ei kopioiminen ja liittäminen. HTTP-403 ilmaisee yleensä ongelma api-näppäinyhdistelmää (virheellinen avain tai syntaksi ongelma miten api-avain on määritetty).

## <a name="load-documents"></a>Tiedostojen lataaminen

**Tunnus** -välilehden asiakirjoja pyyntö näyttää seuraavalta. Pyynnön leipätekstissä 4 hotellit haun tiedot.

   ![][2]

1. Valitse **JULKAISE**.

2.  Kirjoita URL-osoite, joka alkaa HTTPS-palvelun URL-Osoitteesi, suuntanumeroa ja sen jälkeen "/indexes/ <'indexname ' >/docs/indeksi? api-versio 2015-02-28 =". Seuraavassa esimerkissä pitäisi muistuttaa täydellinen URL-osoite.

            https://my-app.search.windows.net/indexes/hotels/docs/index?api-version=2015-02-28

3.  Pyynnön otsikossa on oltava sama kuin ennen. Muista, että voit korvata ohjelmointirajapinnan avain ja host arvoja, jotka ovat kelvollisia palvelusi.

            User-Agent: Fiddler
            host: my-app.search.windows.net
            content-type: application/json
            api-key: 1111222233334444

4.  Pyydä tekstissä esiintyy neljä asiakirjojen lisääminen hotellit indeksi.

            {
            "value": [
            {
                "@search.action": "upload",
                "hotelId": "1",
                "baseRate": 199.0,
                "description": "Best hotel in town",
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
                "@search.action": "upload",
                "hotelId": "3",
                "baseRate": 279.99,
                "description": "Surprisingly expensive",
                "hotelName": "Dew Drop Inn",
                "category": "Bed and Breakfast",
                "tags": ["charming", "quaint"],
                "parkingIncluded": true,
                "smokingAllowed": false,
                "lastRenovationDate": null,
                "rating": 4,
                "location": { "type": "Point", "coordinates": [-122.33207, 47.60621] }
              },
              {
                "@search.action": "upload",
                "hotelId": "4",
                "baseRate": 220.00,
                "description": "This could be the one",
                "hotelName": "A Hotel for Everyone",
                "category": "Basic hotel",
                "tags": ["pool", "wifi"],
                "parkingIncluded": true,
                "smokingAllowed": false,
                "lastRenovationDate": null,
                "rating": 4,
                "location": { "type": "Point", "coordinates": [-122.12151, 47.67399] }
              }
             ]
            }

8.  Valitse **Suorita**.

Muutaman sekunnin pitäisi näkyä HTTP 200-vastauksen istunnon luettelosta. Tämä ilmaisee asiakirjat on luotu. Jos 207, vähintään yksi tiedosto ei voi ladata. Jos saat 404, sinun on syntaksivirhe ylä-tai pyynnön.

## <a name="query-the-index"></a>Kyselyn indeksi

Nyt kun indeksin ja tiedostoja ladataan, voit myöntää kyselyitä, jotka perustuvat ne.  **Tunnus** -välilehden **HAE** -komento, joka tekee palvelusi näyttävät seuraavat näyttökuvan.

   ![][3]

1.  Valitse **HAE**.

2.  Kirjoita URL-osoite, joka alkaa HTTPS-palvelun URL-Osoitteesi, suuntanumeroa ja sen jälkeen "/indexes/ <'indexname ' > / docs?", sitten kyselyparametrit. Esimerkiksi seuraava URL-Esimerkki isäntänimi tilalle sellainen, joka on voimassa palvelusi käyttäminen.
    
            https://my-app.search.windows.net/indexes/hotels/docs?search=motel&facet=category&facet=rating,values:1|2|3|4|5&api-version=2015-02-28

    Tämä kysely termi "oppaalla" etsii ja noutaa luokitusten pinta luokat.

3.  Pyynnön otsikossa on oltava sama kuin ennen. Muista, että voit korvata ohjelmointirajapinnan avain ja host arvoja, jotka ovat kelvollisia palvelusi.

            User-Agent: Fiddler
            host: my-app.search.windows.net
            content-type: application/json
            api-key: 1111222233334444

Vastauksen koodin pitäisi olla 200 ja vastaus tulosteen pitäisi muistuttaa seuraavista näyttökuvan.

   ![][4]

Seuraava esimerkkikysely on MSDN- [hakuindeksin toiminnon (Azure haun API)](http://msdn.microsoft.com/library/dn798927.aspx) . Monissa tämän artikkelin Esimerkki kyselyt sisältää välilyöntejä, jotka eivät ole sallittuja Fiddler. Korvaa kunkin tilaa ja + merkki ennen kyselyn liittäminen merkkijono, ennen kuin yrität Fiddler kyselyn.

**Ennen kuin välilyöntejä ovat seuraavasti:**

        GET /indexes/hotels/docs?search=*&$orderby=lastRenovationDate desc&api-version=2015-02-28

**Kun välilyöntejä korvataan +:**

        GET /indexes/hotels/docs?search=*&$orderby=lastRenovationDate+desc&api-version=2015-02-28

## <a name="query-the-system"></a>Kyselyn järjestelmä

Voit myös kyselyn asiakirjan määrät ja tallennustilaa kulutus järjestelmä. **Tunnus** -välilehdessä pyyntö näyttää seuraavankaltaiselta ja vastaus palauttaa tiedostoja ja paljon tilaa määrän laskeminen.

 ![][5]

1.  Valitse **HAE**.

2.  Kirjoita URL-osoite, joka sisältää palvelun URL-ja sen jälkeen "/ indeksit/hotellit/tilasto? api-version 2015-02-28 =":

            https://my-app.search.windows.net/indexes/hotels/stats?api-version=2015-02-28

3.  Määritä pyynnön otsikko-ohjelmointirajapinnan avain ja host tilalle arvoja, jotka ovat kelvollisia palvelun.

            User-Agent: Fiddler
            host: my-app.search.windows.net
            content-type: application/json
            api-key: 1111222233334444

4.  Jätä pyynnön tekstiin tyhjäksi.

5.  Valitse **Suorita**. Raportissa pitäisi näkyä HTTP 200-tilakoodin istunnon luettelosta. Valitse komento kirjattu tapahtuma.

6.  **Tarkastajien** -välilehti, valitse **otsikot** -välilehti ja valitse sitten JSON-muoto. Raportissa pitäisi näkyä asiakirjan Laske- ja tallennustilaa koon (kilotavuina).

## <a name="next-steps"></a>Seuraavat vaiheet

[Hallitse Search-palvelun Azure-](search-manage.md) artikkelissa koodittomia lähestymistapa hallinta ja Azure-haun avulla.


<!--Image References-->
[1]: ./media/search-fiddler/AzureSearch_Fiddler1_PutIndex.png
[2]: ./media/search-fiddler/AzureSearch_Fiddler2_PostDocs.png
[3]: ./media/search-fiddler/AzureSearch_Fiddler3_Query.png
[4]: ./media/search-fiddler/AzureSearch_Fiddler4_QueryResults.png
[5]: ./media/search-fiddler/AzureSearch_Fiddler5_QueryStats.png
