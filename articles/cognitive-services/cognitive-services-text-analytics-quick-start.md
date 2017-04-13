<properties
    pageTitle="Pikaopas: tietokoneen Learning tekstin Analytics ohjelmointirajapinnan | Microsoft Azure"
    description="Azure Konepohjaisten Oppimistekniikoiden tekstin Analytics - pikaopas"
    services="cognitive-services"
    documentationCenter=""
    authors="onewth"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="cognitive-services"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="onewth"/>

# <a name="getting-started-with-the-text-analytics-apis-to-detect-sentiment-key-phrases-topics-and-language"></a>Tekstin Analytics-ohjelmointirajapinnan esiintyvien markkinatunnelma, avaimen ilmaisut, aiheet ja kielen käytön aloittaminen

<a name="HOLTop"></a>

Tässä asiakirjassa kerrotaan kuinka, määrän palvelun tai sovelluksen käyttämään [Tekstin Analytics API](//go.microsoft.com/fwlink/?LinkID=759711).
Voit käyttää näitä API tunnistamiseen ja tekstin markkinatunnelma, avaimen ilmaisut, aiheet ja kieli. [Katso vuorovaikutteinen esittely käyttökokemuksesta napsauttamalla tätä.](//go.microsoft.com/fwlink/?LinkID=759712)

Tutustu API teknisten [API määritykset](//go.microsoft.com/fwlink/?LinkID=759346) .

Tässä oppaassa on API versio 2. Lisätietoja ohjelmointirajapinnan [tämän asiakirjan viitata](../machine-learning/machine-learning-apps-text-analytics.md)1-versiossa.

Tässä opetusohjelmassa loppuun mennessä osaat ohjelmallisesti tunnistaa:

- **Markkinatunnelma** - teksti on positiivinen tai negatiivinen?

- **Avaimen lauseita** - mitä henkilöitä yhteen artikkelin keskustella?

- **Aiheiden** - mitä henkilöitä keskustella monta artikkeleista?

- **Kielet** - kielen on tekstiä kirjoitetaan?

Huomaa, että tämä API kulut 1 tapahtuman lähetetyn asiakirjan kohden. Esimerkkinä, jos pyydät markkinatunnelma 1000 asiakirjojen yhden puhelun 1000 tapahtumat vähennetään.



<a name="Overview"></a>
## <a name="general-overview"></a>Yleisiä tietoja ##

Tämä asiakirja on vaiheittaiset ohjeet. Microsoftin tavoitteena on opastusta kouluttaminen malli ja osoita resursseja, jotta voit ladata sen tuotannon tarvittavat vaiheet. Tämä Harjoitus kestää noin 30 minuuttia.

Näiden tehtävien on muokkaajan ja soita RESTful päätepisteet värisinä kielellä.

Aloitetaan.

## <a name="task-1---signing-up-for-the-text-analytics-apis"></a>Tehtävä 1 - allekirjoitetun sinua tekstin Analytics-ohjelmointirajapinnan käyttäjäksi ####

Tässä tehtävässä voit tilaa tekstin analytics-palvelu.

1. Siirry [Azure Portal](//go.microsoft.com/fwlink/?LinkId=761108) **Kognitiiviset palvelut** ja varmista Ohjelmointirajapinta tyyppi on valittu **Teksti Analytics** .

1. Valitse suunnitelma. Voit valita **vapaa taso 5 000 tapahtumia ja kuukauden**. On ilmainen suunnitelma, sinun ei peri palvelun käyttämisen. Tarvitset Azure-tilaukseen kirjautuminen. 

1. Muita kenttiä ja luo tili.

1. Kun kirjaudut tekstin Analytics, Etsi **Ohjelmointirajapinnan avain**. Kopioi perusavaimen, sillä tarvitset sitä API-palveluita käytettäessä.


## <a name="task-2---detect-sentiment-key-phrases-and-languages"></a>Tehtävä 2 – tunnistaa markkinatunnelma, avaimen lausekkeet ja kielet ####

On helppo tunnistaa markkinatunnelma, avaimen lausekkeet ja kielillä teksti. Näkyviin tulee ohjelmallisesti saman tuloksen kuin [esittely kokemus](//go.microsoft.com/fwlink/?LinkID=759712) palauttaa.

>[AZURE.TIP] Markkinatunnelma analyysia varten on suositeltavaa tekstin jakaminen lauseiden. Tämä yleensä johtaa tarkkuus-markkinatunnelma ennusteiden.

Huomaa, että tukemat kielet ovat seuraavat:

| Toiminto | Tuettu koodit |
|:-----|:----|
| Markkinatunnelma | `en`(Englanniksi) `es` (Espanja) `fr` (Ranska) ja `pt` (Portugali) |
| Avaimen lausekkeet | `en`(Englanniksi) `es` (Espanja) `de` (Saksa) `ja` (japani) |


1. Sinun on määritetty otsikot seuraavasti. Huomaa, että JSON on tällä hetkellä vain hyväksyä syötteen muoto API. XML ei tueta.

        Ocp-Apim-Subscription-Key: <your API key>
        Content-Type: application/json
        Accept: application/json

1. Seuraavaksi voit muotoilla JSON oman syötteen rivit. Markkinatunnelma, avaimen lausekkeet ja language-muoto on sama. Huomaa, että kunkin tunnuksen on oltava yksilöllinen tunnus palautetaan järjestelmä. Yhden tiedoston, joka voidaan lähettää enimmäiskoko on 10 kt ja lähetetyn syötteen enimmäiskoko on 1 Megatavu. Enintään 1 000 asiakirjoja voi lähettää yhden käynnissä. Korko rajoittaminen on 100 puhelujen minuutissa korvaus - Microsoft suosittelee, että voit lähettää suuria määriä asiakirjoja yhteen käynnissä. Kieli on valinnainen parametri, joka on määritettävä Jos analysoiminen kuin englanninkielisessä teksti. Esimerkki syöte näkyy alla, johon valinnaisten parametrien `language` markkinatunnelma analysis tai avain lause poiminnan on mukana:

        {
            "documents": [
                {
                    "language": "en",
                    "id": "1",
                    "text": "First document"
                },
                ...
                {
                    "language": "en",
                    "id": "100",
                    "text": "Final document"
                }
            ]
        }

1. Puhelun soittaminen **Kirjaa** markkinatunnelma, avaimen lausekkeet ja kielen Input-järjestelmään. URL-osoitteet näyttävät seuraavasti:

        POST https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/sentiment
        POST https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/keyPhrases
        POST https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/languages

1. Puhelun palauttaa JSON muotoiltu tunnukset vastauksen ja havaita ominaisuudet. Esimerkki markkinatunnelma tulosteen alla (ja virheen yksityiskohtaiset tiedot pois). Markkinatunnelma pisteet väliltä 0 – 1 kuuluvat palautetaan kunkin asiakirjan:

        // Sentiment response
        {
            "documents": [
                {
                    "id": "1",
                    "score": "0.934"
                },
                ...
                {
                    "id": "100",
                    "score": "0.002"
                },
            ]
        }

        // Key phrases response
        {
            "documents": [
                {
                    "id": "1",
                    "keyPhrases": ["key phrase 1", ..., "key phrase n"]
                },
                ...
                {
                    "id": "100",
                    "keyPhrases": ["key phrase 1", ..., "key phrase n"]
                },
            ]
        }

        // Languages response
        {
            "documents": [
                {
                    "id": "1",
                    "detectedLanguages": [
                        {
                            "name": "English",
                            "iso6391Name": "en",
                            "score": "1"
                        }
                    ]
                },
                ...
                {
                    "id": "100",
                    "detectedLanguages": [
                        {
                            "name": "French",
                            "iso6391Name": "fr",
                            "score": "0.985"
                        }
                    ]
                }
            ]
        }


## <a name="task-3---detect-topics-in-a-corpus-of-text"></a>Tehtävä 3 – tunnistaa aiheet corpus tekstin ####

Tämä on äskettäin API, mitä ylä aiheiden luettelo palauttaa lähetetty tekstin tietueet. Aiheen yhdistetään avaimen lause, joka voi olla yksi tai useita liittyviä sanat. Ohjelmointirajapinnan on suunniteltu toimimaan hyvin lyhyt ja ihmisten kirjoitettujen tekstiä, kuten tarkistaa ja käyttäjän palautetta varten.

Tämä API edellyttää **vähintään 100 tekstin tietueiden** toimitettavat, mutta on suunniteltu tunnistamaan aiheet satoja tuhansia tietueiden välillä. Muussa kuin englanninkielisessä tietueita tai tietueiden alle 3 sanoilla hylätään ja näin ollen ei liitetä aiheisiin. Yhden tiedoston, joka voidaan lähettää enimmäiskoko on 30 Kilotavun aiheen tunnistaminen ja lähetetyn syötteen enimmäiskoko on 30 Megatavua. Aiheen tunnistus on rajoitettu 5 lähetysten 5 minuutin välein.

On kaksi muita **valinnaisia** syöteparametria, jotka auttavat parantamaan laatua, tulokset:

- **Lopeta sanat.**  Nämä sanat ja sulje lomakkeet (kuten monikot) jätetään pois aihetta tunnistus putkijohto. Käytä tätä, yleisiä sanoja (, esimerkiksi "ongelma", "Virhe" ja "käyttäjä" voi olla asiakas valitusten ohjelmistosta sopivat vaihtoehdot). Kukin merkkijono on oltava yhtä sanaa.
- **Lopeta lauseita** - näistä lausekkeista jätetään pois palautetun aiheiden luettelo. Tällä komennolla voit jättää yleisiä ohjeita, joita et halua näkevän tulokset. Esimerkiksi "Microsoft" ja "Azure" olisi sopivat vaihtoehdot ohjeita, jos haluat jättää pois. Merkkijonojen voi olla useita sanoja.

Tunnista aiheet teksti seuraavasti.

1. Voit muotoilla JSON-syöte. Tällä hetkellä voit pysäyttää sanojen määrittämiseen ja lopettaa lauseita.

        {
            "documents": [
                {
                    "id": "1",
                    "text": "First document"
                },
                ...
                {
                    "id": "100",
                    "text": "Final document"
                }
            ],
            "stopWords": [
                "issue", "error", "user"
            ],
            "stopPhrases": [
                "Microsoft", "Azure"
            ]
        }

1. Käytä samaa otsikot määritysten mukaisesti tehtävän 2, varmista **Kirjaa** kutsu aiheet päätepisteen:

        POST https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/topics

1. Tämä palauttaa `operation-location` ylätunnisteena vastauksen, joiden arvo on kyselyn tuloksena ohjeita URL-osoite:

        'operation-location': 'https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/operations/<operationId>'

1. Kysely palautettu `operation-location` säännöllisesti **HAE** pyynnön kanssa. Kun minuutissa suositellaan.

        GET https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/operations/<operationId>

1. Päätepisteen palauttaa vastauksen, mukaan lukien `{"status": "notstarted"}` ennen käsittelyä `{"status": "running"}` käsiteltäessä ja `{"status": "succeeded"}` tehtyä tulosteen kanssa. Voit sitten tarjoaman tulosteen olevan muotoa (Huomautus tiedot, kuten virhe muoto ja päivämäärät on jätetty pois tässä esimerkissä):

        {
            "status": "succeeded",
            "operationProcessingResult": {
                "topics": [
                    {
                        "id": "8b89dd7e-de2b-4a48-94c0-8e7844265196"
                        "score": "5"
                        "keyPhrase": "first topic name"
                    },
                    ...
                    {
                        "id": "359ed9cb-f793-4168-9cde-cd63d24e0d6d"
                        "score": "3"
                        "keyPhrase": "final topic name"
                    }
                ],
                "topicAssignments": [
                    {
                        "topicId": "8b89dd7e-de2b-4a48-94c0-8e7844265196",
                        "documentId": "1",
                        "distance": "0.354"
                    },
                    ...
                    {
                        "topicId": "359ed9cb-f793-4168-9cde-cd63d24e0d6d",
                        "documentId": "55",
                        "distance": "0.758"
                    },            
                ]
            }
        }

Huomaa, että aiheita onnistuneen vastaus `operations` päätepisteen saa seuraavan rakenteen:

    {
            "topics" : [{
                "id" : "string",
                "score" : "number",
                "keyPhrase" : "string"
            }],
            "topicAssignments" : [{
                "documentId" : "string",
                "topicId" : "string",
                "distance" : "number"
            }],
            "errors" : [{
                "id" : "string",
                "message" : "string"
            }]
        }

Tämä vastaus jokaisen osan selitykset ovat seuraavat:

**ohjeaiheet**

| Avain | Kuvaus |
|:-----|:----|
| tunnus | Aihe yksilöllinen. |
| tulos | Aiheen myönnetyt tiedostojen määrä. |
| keyPhrase | Luoda yhteenveto sana tai lause aihe. |

**topicAssignments**

| Avain | Kuvaus |
|:-----|:----|
| documentId | Asiakirjan tunnus. Vastaa tehtävässä syötteen sisältyvät tunnus. |
| topicId | Aihetunnus, johon asiakirja on määritetty. |
| etäisyys | Asiakirjan aihe liitos tulos väliltä 0 – 1. Pienet a etäisyys sija mitä aiheen liitos on. |

**virheet**

| Avain | Kuvaus |
|:-----|:----|
| tunnus | Lisää asiakirja virhe viittaa yksilöllinen. |
| viestin | Virhesanoma. |

## <a name="next-steps"></a>Seuraavat vaiheet ##

Onnittelen! Tekstin analytics käyttäminen tiedot on nyt valmis. Haluat ehkä nyt tarkastelevat Visualisoi tietosi-työkalun, kuten [Power BI](//powerbi.microsoft.com) käytöstä sekä automatisointi antamaan sinulle reaaliaikainen näkymän tekstin tietojen tietoja.

Miten tekstin Analytics-ominaisuuksia, kuten markkinatunnelma, voi käyttää osana robotti on ohjeartikkelissa robotti Framework sivustossa [Tunnesiteisiin robotti](http://docs.botframework.com/en-us/bot-intelligence/language/#example-emotional-bot) esimerkki.
