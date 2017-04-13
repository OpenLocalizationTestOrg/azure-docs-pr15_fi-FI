<properties
   pageTitle="Logiikan sovellusten silmukoita, alueet ja Debatching | Microsoft Azure"
   description="Logiikan App silmukka, laajuuteen ja debatching käsitteitä"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="dwrede"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="05/14/2016"
   ms.author="jehollan"/>
   
# <a name="logic-apps-loops-scopes-and-debatching"></a>Logiikan sovellusten silmukoita, alueet ja Debatching
  
>[AZURE.NOTE] Tässä versiossa on artikkelissa koskee logiikan sovellusten 2016-04-01-esikatselu-rakenteen ja uudempi versio.  Käsitteitä vastaavat vanhat rakenteiden, mutta vaikutusalueet ovat vain käytettävissä tätä rakennetta ja uudempi versio.
  
## <a name="foreach-loop-and-arrays"></a>ForEach silmukan ja matriisien käyttöä
  
Logiikan sovellusten avulla voit ympäri tietoja ja suorittaa toiminnon kunkin kohteen.  Tämä on mahdollista kautta `foreach` toiminto.  Suunnittelussa, voit määrittää lisää silmukassa varten.  Kun olet valinnut taulukon haluat käydä päälle, voit aloittaa toimintojen lisäämisen.  Tällä hetkellä olet rajoitettu vain yhden toiminnon foreach silmukan kohti, mutta tämä rajoitus peruutettava seuraavan viikon aikana.  Kerran silmukassa voit aloittaa Määritä, mitä tapahtuu, jokainen matriisin arvo.

Jos käytät koodi-näkymässä, voit määrittää, silmukassa.  Tämä on esimerkki silmukassa, joka lähettää sähköpostia kunkin sähköpostiosoitetta, joka sisältää "microsoft.com" varten:

```
{
    "email_filter": {
        "type": "query",
        "inputs": {
            "from": "@triggerBody()['emails']",
            "where": "@contains(item()['email'], 'microsoft.com')
        }
    },
    "forEach_email": {
        "type": "foreach",
        "foreach": "@body('email_filter')",
        "actions": {
            "send_email": {
                "type": "ApiConnection",
                "inputs": {
                "body": {
                    "to": "@item()",
                    "from": "me@contoso.com",
                    "message": "Hello, thank you for ordering"
                }
                "host": {
                    "connection": {
                        "id": "@parameters('$connections')['office365']['connection']['id']"
                    }
                }
                }
            }
        },
        "runAfter":{
            "email_filter": [ "Succeeded" ]
        }
    }
}
```
  
  A `foreach` toiminto voi käydä over taulukoita enintään 5 000 riviä.  Iteraation suorittaa rinnakkain, joten voi olla tarpeen lisätä viestien jonon, virtaussäätimiä tarvittaessa.
  
## <a name="until-loop"></a>Silmukan asti
  
  Voit suorittaa toiminnon tai toimintojen sarjan, kunnes ehto täyttyy.  Tämän ominaisuuden useimmin käytetty vaihtoehto soittaa päätepisteen kunnes etsimääsi vastausta.  Suunnittelussa, voit määrittää lisää silmukan asti.  Kun olet lisännyt sisällä silmukan toiminnot, voit määrittää Lopeta ehto sekä silmukan rajoitukset.  Silmukka jaksojen välillä on minuutin viive.
  
  Jos käytät koodi-näkymässä, voit määrittää kunnes silmukka, kuten alla.  Tämä on esimerkki kutsumista HTTP-päätepisteen, ennen kuin vastaus tekstissä on arvo "Valmis".  Viimeistelee milloin joko 
  
  * HTTP-vastaus on tila on "Valmis"
  * Se on yrittänyt 1 tunti
  * Se on testitilaksi 100 kertaa
  
  ```
  {
      "until_successful":{
        "type": "until",
        "expression": "@equals(actions('http')['status'], 'Completed'),
        "limit": {
            "count": 100,
            "timeout": "PT1H"
        },
        "actions": {
            "create_resource": {
                "type": "http",
                "inputs": {
                    "url": "http://provisionRseource.com",
                    "body": {
                        "resourceId": "@triggerBody()"
                    }
                }
            }
        }
      }
  }
  ```
  
## <a name="spliton-and-debatching"></a>SplitOn ja Debatching

Joskus käynnistimen saattaa tulla matriisin kohteet, jotka haluat debatch ja Aloita työnkulku kohteen kohden.  Tämä onnistuu kautta `spliton` komento.  Oletusarvoisesti, jos käynnistimen swagger määrittää paketti, joka on matriisikaava `spliton` lisätään ja Käynnistä Suorita kohteen kohden.  SplitOn voidaan lisätä vain käynnistintä.  Näin voit manuaalisesti määritetty tai ohittaa määritelmä koodinäkymän.  Tällä hetkellä SplitOn voit debatch taulukoita enintään 5 000 kohdetta.  Ei voi olla `spliton` ja toteuttaa myös syncronous vastauksen mallia.  Mikä tahansa työnkulku-kohdat, jotka on `response` toiminto lisäksi `spliton` suorittaa asyncronously ja Lähetä heti `202 Accepted` vastaus.  

Koodi-näkymässä voidaan määrittää SplitOn seuraavan esimerkin mukaisesti.  Tämä recieves matriisin kohteiden ja debatches jokaiselle riville.

```
{
    "myDebatchTrigger": {
        "type": "Http",
        "inputs": {
            "url": "http://getNewCustomers",
        },
        "recurrence": {
            "frequency": "Second",
            "interval": 15
        },
        "spliton": "@triggerBody()['rows']"
    }
}
```

## <a name="scopes"></a>Käyttöalueet

Ei ryhmitellä joukko ehtorivien käyttöalue toimia.  Tämä on erityisen hyödyllinen toteuttamisesta poikkeuksen käsittely.  Suunnittelussa voit lisätä uuden vaikutusalueen ja aloittaa se sisältyy minkä tahansa toimintojen lisäämisen.  Käyttöalueen koodi-näkymässä, kuten jompikumpi seuraavista:


```
{
    "myScope": {
        "type": "scope",
        "actions": {
            "call_bing": {
                "type": "http",
                "inputs": {
                    "url": "http://www.bing.com"
                }
            }
        }
    }
}
```
