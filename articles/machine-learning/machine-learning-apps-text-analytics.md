<properties
    pageTitle="Koneen Learning API: Tekstin Analytics | Microsoft Azure"
    description="Microsoftin koneen Learning tekstin Analytics API voidaan rakenteeton markkinatunnelma analysis, avaimen lause poiminnan, kielen tunnistaminen ja aiheen tunnistus tekstin analysointia varten."
    services="machine-learning"
    documentationCenter=""
    authors="onewth"
    manager="jhubbard"
    editor="cgronlun"/> 

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="onewth"/>


# <a name="machine-learning-apis-text-analytics-for-sentiment-key-phrase-extraction-language-detection-and-topic-detection"></a>Koneen Learning API: Tekstin analysointitietoja Markkinatunnelma, Key lause poiminnan, kielen tunnistaminen ja aiheen tunnistus

>[AZURE.NOTE] Tässä oppaassa on Ohjelmointirajapinnan versio 1. Version 2 [**viitata tässä asiakirjassa**](../cognitive-services/cognitive-services-text-analytics-quick-start.md). 2-versio on nyt tämän API Ensisijainen versio.

## <a name="overview"></a>Yleiskatsaus

Tekstin Analytics-Ohjelmointirajapinnan on luotu Azure koneen Learning tekstin analytics- [Verkkopalvelut](https://datamarket.azure.com/dataset/amla/text-analytics) kuuluu. Ohjelmointirajapinnan voidaan rakenteeton tekstiä, kuten markkinatunnelma analyysi, avaimen lause poiminnan, kielen tunnistaminen ja aiheen tunnistus analysointia varten. Mitään koulutustietoja ei tarvita käyttämään tämän API: tuoda vain tekstitietoja. Tämä API käyttää Lisäasetukset luonnollisen kielen käsittelyn tekniikoita aikana parhaiten luokan ennusteiden.

Näet [esittely sivuston](https://text-analytics-demo.azurewebsites.net/), saat myös näkyviin [Esimerkkejä](https://text-analytics-demo.azurewebsites.net/Home/SampleCode) siitä, miten voit toteuttaa tekstin analytics C#- ja Python tekstin analytics-toiminto.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)] 

---

## <a name="sentiment-analysis"></a>Markkinatunnelma analyysi

Ohjelmointirajapinnan palauttaa lukuarvojen väliltä 0 ja 1. Tulosten lähellä 1 osoittavat positiivinen markkinatunnelma, kun tulosten lähellä 0 osoittaa negatiivinen markkinatunnelma. Markkinatunnelma tulos on luotu käyttämällä luokittelun avulla. Valitsimen syötteen ominaisuuksien Sisällytä n-g, puhe osan tunnisteet ja Wordin embeddings luotu ominaisuuksia. Englannin kieli ei tällä hetkellä vain tuettu.
 
## <a name="key-phrase-extraction"></a>Avaimen lause poiminnan

Ohjelmointirajapinnan palauttaa luettelon merkkijonojen osoittavat avaimen mainittavat asiat, valitse Lisää teksti. Olemme lomakkeen-Microsoft Officen kehittyneitä luonnollisen kielen käsittely-työkalujen avulla. Englannin kieli ei tällä hetkellä vain tuettu.

## <a name="language-detection"></a>Kielen tunnistaminen

Ohjelmointirajapinnan palauttaa olemassa kieli- ja lukuarvojen väliltä 0 ja 1. 1 lähellä pisteet ilmaisevat 100 %: n varmuudella tunnistettujen kieli on TOSI. Korkeintaan 120 kielet ovat tuettuja.

## <a name="topic-detection"></a>Aiheen tunnistus

Tämä on äskettäin API, mitä ylä aiheiden luettelo palauttaa lähetetty tekstin tietueita. Aiheen yhdistetään avaimen lause, joka voi olla yksi tai useita liittyviä sanat. Tämä API edellyttää vähintään 100 tekstin tietueita voidaan esittää, mutta on suunniteltu tunnistamaan aiheet satoja tuhansia tietueiden välillä. Huomaa, että tämä API kulut 1 tapahtuman teksti-tietuetta, jotka on lähetetty kohden. Ohjelmointirajapinnan on suunniteltu toimimaan hyvin lyhyt ja ihmisten kirjoitettujen tekstiä, kuten tarkistaa ja käyttäjän palautetta varten.

---

## <a name="api-definition"></a>API määritys

### <a name="headers"></a>Otsikot

Varmista oikea otsikot sisällyttäminen pyyntö, jonka pitäisi näyttää seuraavalta:

    Authorization: Basic <creds>
    Accept: application/json
               
    Where <creds> = ConvertToBase64(“AccountKey:” + yourActualAccountKey);  

Löydät tiliavain [Azure Data Market](https://datamarket.azure.com/account/keys)-tililtä. Huomaa, että tällä hetkellä vain JSON hyväksytään syöttö- ja muotoja. XML ei tueta.

---

## <a name="single-response-apis"></a>Yksittäisen vastauksen API

### <a name="getsentiment"></a>GetSentiment

**URL-OSOITE** 

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetSentiment

**Esimerkki pyyntö**

Kutsussa on pyydetään markkinatunnelma analyysi lause "Hei-maailman":

    GET https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetSentiment?Text=hello+world

Tämä palauttaa vastausta seuraavasti:

    {
      "odata.metadata":"https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/$metadata",
        "Score":1.0
    }

---

### <a name="getkeyphrases"></a>GetKeyPhrases

**URL-OSOITE**

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetKeyPhrases

**Esimerkki pyyntö**

Alla kutsussa on pyydetään avaimen lausekkeet löydy teksti "On oikean hotellista yhteydenpitoon, yksilöllinen décor ja helpossa muodossa henkilökunnan":

    GET https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetKeyPhrases?
    Text=It+was+a+wonderful+hotel+to+stay+at,+with+unique+decor+and+friendly+staff

Tämä palauttaa vastausta seuraavasti:

    {
      "odata.metadata":"https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/$metadata",
      "KeyPhrases":[
        "wonderful hotel",
        "unique decor",
        "friendly staff"
      ]
    }
 
---

### <a name="getlanguage"></a>GetLanguage

**URL-OSOITE**

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetLanguage

**Esimerkki pyyntö**

GET-puhelu on alla emme pyydetään varten tekstin *Hei maailma* avaimen lausekkeet markkinatunnelma varten

    GET https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetLanguages?
    Text=Hello+World

Tämä palauttaa vastausta seuraavasti:

    {
      "UnknownLanguage": false,
      "DetectedLanguages": [{
        "Name": "English",
        "Iso6391Name": "en",
        "Score": 1.0
      }]
    }

**Valinnaisten parametrien**

`NumberOfLanguagesToDetect`on valinnainen parametri. Oletusarvo on 1.

---

## <a name="batch-apis"></a>Erän API

Tekstin Analytics-palvelun avulla voit tehdä markkinatunnelma ja avain-lause uuttoja erä-tilassa. Huomaa, että tietueen tulos laskee yhteen tapahtumana. Esimerkkinä, jos pyydät markkinatunnelma 1000 tietueiden yhden puhelun 1000 tapahtumat vähennetään.

Huomaa, että järjestelmään tunnukset järjestelmän palauttama tunnukset. Web-palvelu ei tarkista, että näiden tunnusten ovat yksilöllisiä. Vahvista yksilöllisyyden soittajan vastuu on. 


### <a name="getsentimentbatch"></a>GetSentimentBatch

**URL-OSOITE** 

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetSentimentBatch

**Esimerkki pyyntö**

Kirjaa puhelu emme pyydetään lauseiden oikeudet omistaa lausekkeista "Hei-maailman", "Hei tapahtuman nimenä maailman" ja "Hei oma maailman" pyynnön tekstiosaan varten:

    POST https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetSentimentBatch 

Pyyntö leipäteksti:

    {"Inputs":
    [
        {"Id":"1","Text":"hello world"},
        {"Id":"2","Text":"hello foo world"},
        {"Id":"3","Text":"hello my world"},
    ]}

Saat alla vastauksen tulosten tekstin tunnukset liittyvä luettelo:

    {
      "odata.metadata":"<url>", 
      "SentimentBatch":
      [
        {"Score":0.9549767,"Id":"1"},
        {"Score":0.7767222,"Id":"2"},
        {"Score":0.8988889,"Id":"3"}
      ],  
      "Errors":[]
    }


---

### <a name="getkeyphrasesbatch"></a>GetKeyPhrasesBatch

**URL-OSOITE**

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetKeyPhrasesBatch

**Esimerkki pyyntö**

Tässä esimerkissä on pyydetään lauseiden oikeudet omistaa, seuraavat tekstit avaimen lausekkeet luettelo: 

* "On oikean hotellista yhteydenpitoon, yksilöllinen décor ja helpossa muodossa henkilökunnan"
* "On näyttävien muodosta-kokoukseen, ja hyvin kiinnostavat käytön"
* "Liikenne on terrible, voin käytetyt kolmas tunti airport siirtymällä"

Tämä on esitetty päätepisteen kirjaa kutsu nimellä:

    POST https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetKeyPhrasesBatch

Pyyntö leipäteksti:

    {"Inputs":
    [
        {"Id":"1","Text":"It was a wonderful hotel to stay at, with unique decor and friendly staff"},
        {"Id":"2","Text":"It was an amazing build conference, with very interesting talks"},
        {"Id":"3","Text":"The traffic was terrible, I spent three hours going to the airport"}
    ]}

Saat alla vastauksen avaimen lauseita liittyvän tekstin tunnukset luettelo:

    { "odata.metadata":"<url>",
        "KeyPhrasesBatch":
        [
           {"KeyPhrases":["unique decor","friendly staff","wonderful hotel"],"Id":"1"},
           {"KeyPhrases":["amazing build conference","interesting talks"],"Id":"2"},
           {"KeyPhrases":["hours","traffic","airport"],"Id":"3" }
        ],
        "Errors":[]
    }

---

### GetLanguageBatch

In the POST call below, we are requesting language detection for two text inputs:

    POST https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetLanguageBatch

Request body:

    {
      "Inputs": [
        {"Text": "hello world", "Id": "1"},
        {"Text": "c'est la vie", "Id": "2"}
      ]
    }

This returns the following response, where English is detected in the first input and French in the second input:

    {
       "LanguageBatch": [{
         "Id": "1",
         "DetectedLanguages": [{
            "Name": "englanti",
            "Iso6391Name": "en",
            "Score": 1.0
         }],
         "UnknownLanguage": false
       },
       {
         "Id": "2",
         "DetectedLanguages": [{
            "Name": "ranska",
            "Iso6391Name": "fr",
            "Score": 1.0
         }],
         "UnknownLanguage": false
       }],
       "Errors": []
    }

---

## <a name="topic-detection-apis"></a>Aiheen tunnistus API

Tämä on äskettäin API, mitä ylä aiheiden luettelo palauttaa lähetetty tekstin tietueet. Aiheen yhdistetään avaimen lause, joka voi olla yksi tai useita liittyviä sanat. Huomaa, että tämä API kulut 1 tapahtuman teksti-tietuetta, jotka on lähetetty kohden.

Tämä API edellyttää vähintään 100 tekstin tietueita voidaan esittää, mutta on suunniteltu tunnistamaan aiheet satoja tuhansia tietueiden välillä.


### <a name="topics--submit-job"></a>Ohjeita – Lähetä työ

**URL-OSOITE**

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/StartTopicDetection

**Esimerkki pyyntö**


Kirjaa puhelun alla emme pyydetään 100 artikkeleita, jossa ensimmäinen ja viimeinen syötteen artikkelit ovat näkyvissä ja kaksi StopPhrases sisältyvät joukko aiheet.

    POST https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/StartTopicDetection HTTP/1.1

Pyyntö leipäteksti:

    {"Inputs":[
        {"Id":"1","Text":"I loved the food at this restaurant"},
        ...,
        {"Id":"100","Text":"I hated the decor"}
    ],
    "StopPhrases":[
        "restaurant", “visitor"
    ]}

Alla olevassa vastauksen työn tunnus hakeminen lähetetyn työn:

    {
        "odata.metadata":"<url>",
        "JobId":"<JobId>"
    }

Yksittäisen sanan tai useita word-lausekkeet, jotka ei olisi palautetaan aiheiden luettelo. Voidaan suodattamaan hyvin yleisiä ohjeita. Esimerkiksi tietojoukko tietoja hotellista tarkistukset, "hotellista" ja "hostel" voi olla järkevää Lopeta lauseita.  

### <a name="topics--poll-for-job-results"></a>Ohjeita – työn kyselyn tulokset

**URL-OSOITE**

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetTopicDetectionResult

**Esimerkki pyyntö**

Välittää JobId palautti 'Lähetä työn' vaiheesta hakeaksesi tuloksia. On suositeltavaa kutsua tämän päätepisteen minuutin välein kunnes tila = 'Valmis' vastauksessa. Kestää noin 10 minuutin työn tai pidempi työt, joilla on monia tietueisiin tuhansia.

    GET https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/GetTopicDetectionResult?JobId=<JobId>


Kun se käsittelee vastaus on seuraavasti:

    {
        "odata.metadata":"<url>",
        "Status":"Running",
        "TopicInfo":[],
        "TopicAssignment":[],
        "Errors":[]
    }


Ohjelmointirajapinnan palauttaa tulosteen JSON-muodossa, valitse Seuraava kaava:

    {
        "odata.metadata":"<url>",
        "Status":"Finished",
        "TopicInfo":[
        {
            "TopicId":"ed00480e-f0a0-41b3-8fe4-07c1593f4afd",
            "Score":8.0,
            "KeyPhrase":"food"
        },
        ...
        {
            "TopicId":"a5ca3f1a-fdb1-4f02-8f1b-89f2f626d692",
            "Score":6.0,
            "KeyPhrase":"decor"
            }
        ],
        "TopicAssignment":[
        {
            "Id":"1",
            "TopicId":"ed00480e-f0a0-41b3-8fe4-07c1593f4afd",
            "Distance":0.7809
        },
        ...
        {
            "Id":"100",
            "TopicId":"a5ca3f1a-fdb1-4f02-8f1b-89f2f626d692",
            "Distance":0.8034
        }
        ],
        "Errors":[]


Vastauksen jokaisen osan ominaisuuksia ovat seuraavat:

**TopicInfo ominaisuudet**

| Avain | Kuvaus |
|:-----|:----|
| TopicId | Aihe yksilöllinen. |
| Tulos | Aiheen myönnetyt tietueiden määrää. |
| KeyPhrase | Luoda yhteenveto sana tai lause aihe. Voi olla 1 tai useita sanoja. |

**TopicAssignment ominaisuudet**

| Avain | Kuvaus |
|:-----|:----|
| Tunnus | Tietueen tunnus. Vastaa tehtävässä syötteen sisältyvät tunnus. |
| TopicId | Aihetunnus, johon tietue on määritetty. |
| Etäisyys | Luottamusväli, että tietueen kuuluu aihetta. Etäisyys lähemmäksi nolla näkyy suurempi LUOTTAMUSVÄLI. |
