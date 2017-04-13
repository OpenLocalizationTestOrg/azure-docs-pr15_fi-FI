<properties
    pageTitle="Tekstin Analytics API 2-Version päivittäminen | Microsoft Azure"
    description="Azure Konepohjaisten Oppimistekniikoiden tekstin Analytics - Version 2 päivittäminen"
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

# <a name="upgrading-to-version-2-of-the-text-analytics-api"></a>Tekstin Analytics API 2-Version päivittäminen #

Tämän oppaan opastaa prosessi, jossa [Ensimmäinen versio Ohjelmointirajapinnan](../machine-learning/machine-learning-apps-text-analytics.md) käyttämään toisen version koodisi päivittäminen. 

Jos ole käyttänyt Ohjelmointirajapinnan ja haluat lisätietoja, voit **[Lisätietoja Ohjelmointirajapinnan tähän](//go.microsoft.com/fwlink/?LinkID=759711)** tai **[Noudata pika-aloitusopas](//go.microsoft.com/fwlink/?LinkID=760860)**. Tekniset tiedot-kohdassa **[API-määritys](//go.microsoft.com/fwlink/?LinkID=759346)**.

### <a name="part-1-get-a-new-key"></a>Osa 1. Hae uusi avain ###

Sinun on ensin uusi Ohjelmointirajapinnan avain käyttämistä **Azure-portaalissa**:

1. Siirry käyttää [Cortana liiketoimintatietojen Gallery](//gallery.cortanaintelligence.com/MachineLearningAPI/Text-Analytics-2)tekstin Analytics-palvelu. Tässä kohdassa löydät myös linkkejä asiakirjat ja koodi-mallit.

1. Valitse **Rekisteröidy**. Tätä linkkiä vievät sinut Azure hallinta-portaaliin, jossa voit rekisteröidä palvelun.

1. Valitse suunnitelma. Voit valita **vapaa taso 5 000 tapahtumia ja kuukauden**. On ilmainen suunnitelma, sinun ei peri palvelun käyttämisen. Tarvitset Azure-tilaukseen kirjautuminen. 

1. Sen jälkeen, kun kirjaudut tekstin Analytics, sinun annettava **Ohjelmointirajapinnan avain**. Kopioi tämä avain, sillä sinun on se API-palveluita käytettäessä.

### <a name="part-2-update-the-headers"></a>Osa 2. Päivitä otsikot ###

Päivitä lähetetyn Otsikkoarvot alla kuvatulla tavalla. Huomautus tiliavain tiliavain ei enää koodattu.

**Versio 1**

    Authorization: Basic base64encode(<your Data Market account key>)
    Accept: application/json

**Version 2**

    Content-Type: application/json
    Accept: application/json
    Ocp-Apim-Subscription-Key: <your Azure Portal account key>


### <a name="part-3-update-the-base-url"></a>Osa 3. Perusosoitteen päivittäminen ###

**Versio 1**

    https://api.datamarket.azure.com/data.ashx/amla/text-analytics/v1/

**Version 2**

    https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/

### <a name="part-4a-update-the-formats-for-sentiment-key-phrases-and-languages"></a>Osa 4 a. Päivitä muodot markkinatunnelma, avaimen lausekkeet ja kielet ###

#### <a name="endpoints"></a>Päätepisteet ####

HAE päätepisteet on nyt vähennetty, jotta kaikki syötteen jätettävä siten, että viestin. Päivitä päätepisteet alla niistä.

| |Yhden loppupisteen versio 1|Versio 1 erä päätepiste|Version 2 päätepiste|
|---|---|---|---|
|Puhelutyyppi|HAE|KIRJAA|KIRJAA|
|Markkinatunnelma|```GetSentiment```|```GetSentimentBatch```|```sentiment```|
|Avaimen lausekkeet|```GetKeyPhrases```|```GetKeyPhrasesBatch```|```keyPhrases```|
|Kielet|```GetLanguage```|```GetLanguageBatch```|```languages```|

#### <a name="input-formats"></a>Syötteen muotoja ####

Huomautus vain Kirjaa muotoilun nyt hyväksytään, jotta alustat olisi käyttäjältä, joka käyttää aiemmin yksittäisiä asiakirjoja päätepisteet vastaavasti. Syötteiden eivät ole kirjainkoko on merkitsevä.

**Versio 1 (erä)**

    {
      "Inputs": [
        {
          "Id": "string",
          "Text": "string"
        }
      ]
    }

**Version 2**

    {
      "documents": [
        {
          "id": "string",
          "text": "string"
        }
      ]
    }

#### <a name="output-from-sentiment"></a>Markkinatunnelma tulosteen ####

**Versio 1**

    {
      "SentimentBatch":[{
        "Id":"string",
        "Score":"double"
      }],
      "Errors" : [{
        "Id":"string",
        "Message":"string"
      }]
    }

**Version 2**

    {
      "documents":[{
        "id":"string",
        "score":"double"
      }],
      "errors" : [{
        "id":"string",
        "message":"string"
      }]
    }

#### <a name="output-from-key-phrases"></a>Tulosteen avaimen lausekkeet ####

**Versio 1**

    {
      "KeyPhrasesBatch":[{
        "Id":"string",
        "KeyPhrases":["string"]
      }],
      "Errors" : [{
        "Id":"string",
        "Message":"string"
      }]
    }

**Version 2**

    {
      "documents":[{
        "id":"string",
        "keyPhrases":["string"]
      }],
      "errors" : [{
        "id":"string",
        "message":"string"
      }]
    }

#### <a name="output-from-languages"></a>Tulosteen kielet ####


**Versio 1**

    {
      "LanguageBatch":[{
        "id":"string",
        "detectedLanguages": [{
          "Score":"double"
          "Name":"string",
          "Iso6391Name":"string"
        }]
      }],
      "Errors" : [{
        "Id":"string",
        "Message":"string"
      }]
    }

**Version 2**

    {
      "documents":[{
        "id":"string",
        "detectedLanguages": [{
          "score":"double"
          "name":"string",
          "iso6391Name":"string"
        }]
      }],
      "errors" : [{
        "id":"string",
        "message":"string"
      }]
    }


### <a name="part-4b-update-the-formats-for-topics"></a>Osa 4. Päivitä ohjeita muodot ###

#### <a name="endpoints"></a>Päätepisteet ####

| |Päätepisteen versio 1 | Version 2 päätepiste|
|---|---|---|
|Lähetä aiheen tunnistaminen (JULKAISE)|```StartTopicDetection```|```topics```|
|Nouda aiheen tulokset (HAE)|```GetTopicDetectionResult?JobId=<jobId>```|```operations/<operationId>```|

#### <a name="input-formats"></a>Syötteen muotoja ####

**Versio 1**

    {
      "StopWords": [
        "string"
      ],
      "StopPhrases": [
        "string"
      ], 
      "Inputs": [
        {
          "Id": "string",
          "Text": "string"
        }
      ]
    }

**Version 2**

    {
      "stopWords": [
        "string"
      ],
      "stopPhrases": [
        "string"
      ],
      "documents": [
        {
          "id": "string",
          "text": "string"
        }
      ]
    }

#### <a name="submission-results"></a>Lähettämisen tulokset ####

**Versio 1 (JULKAISE)**

Aiemmin, kun työ on valmis, voit näytetä seuraava JSON-tulos, jossa työn tunnus liitetään hakeaksesi tulosteen URL-osoite.

    {
        "odata.metadata":"<url>",
        "JobId":"<JobId>"
    }

**Version 2 (JULKAISE)**

Otsikon arvo nyt sisältää vastausta seuraavasti, jossa `operation-location` käytetään päätepisteen lisääminen tulosten:

    'operation-location': 'https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/operations/<operationId>'

#### <a name="operation-results"></a>Toiminnon tulokset ####

**Versio 1 (HAE)**

    {
      "TopicInfo" : [{
        "TopicId" : "string"
        "Score" : "double"
        "KeyPhrase" : "string"
      }],
      "TopicAssignment" : [{
        "Id" : "string",
        "TopicId" : "string",
        "Distance" : "double"
      }],
      "Errors" : [{
        "Id":"string",
        "Message":"string"
      }]
    }

**Version 2 (HAE)**

Kuten edellä **säännöllisesti kyselyn tulos** (minuutin välein on ehdotettu aika) saakka tulosteen palautetaan. 

Kun aiheet Ohjelmointirajapinnan on valmis, tila luku `succeeded` palautetaan. Tämä sisältää sitten tulos tulokset alla muodossa:

    {
        "status": "succeeded",
        "createdDateTime": "string",
        "operationType": "topics",
        "processingResult": {
            "topics" : [{
            "id" : "string"
            "score" : "double"
            "keyPhrase" : "string"
          }],
          "topicAssignments" : [{
            "topicId" : "string",
            "documentId" : "string",
            "distance" : "double"
          }],
          "errors" : [{
              "id":"string",
              "message":"string"
          }]
        }
    }

### <a name="part-5-test-it"></a>Osa 5. Testaa se! ###

Pitäisi nyt olla hyvä huomioida missä! Testaa koodin pieni otoksen varmistaa, että voit käsitellä tietoja onnistuneesti.
