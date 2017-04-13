<properties 
    pageTitle="Logiikan App määritelmien luominen | Microsoft Azure" 
    description="Opettele kirjoittamaan logiikan sovellusten JSON-määritys" 
    authors="jeffhollan" 
    manager="erikre" 
    editor="" 
    services="logic-apps" 
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/25/2016"
    ms.author="jehollan"/>
    
# <a name="author-logic-app-definitions"></a>Tekijä logiikan App määritelmät
Tässä ohjeaiheessa kerrotaan, miten voit käyttää [Azure logiikan sovellusten](app-service-logic-what-are-logic-apps.md) määritykset, joka on yksinkertainen ja määritettäviä JSON-kieli. Jos et ole antanut sitä vielä, tutustu [logiikan uuden sovelluksen luominen](app-service-logic-create-a-logic-app.md) ensin. Voit myös lukea [koko MSDN-määritys-kielen aineistoa](http://aka.ms/logicappsdocs).

## <a name="several-steps-that-repeat-over-a-list"></a>Useita vaiheita, toista luettelon

Voit hyödyntää toista kautta enintään 10 k matriisin kohteiden ja suorita kohteelle toiminto kunkin [foreach tyyppi](app-service-logic-loops-and-scopes.md) .

## <a name="a-failure-handling-step-if-something-goes-wrong"></a>Jos järjestelmässä vikaan virheen käsittely vaihe

Haluat yleisesti voi kirjoittaa *korjaus vaihe* – logiikkaa, joka suorittaa, jos **ja vain siinä tapauksessa**, vähintään yhtä puhelut epäonnistui. Tässä esimerkissä on jää tietoja monista eri paikoista, mutta kutsu epäonnistuu, jos haluan LÄHETTÄÄ viestiä toiseen sijaintiin, jotta voin etsiä kyseinen virhe myöhemmin:  

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
    },
    "triggers": {
        "manual": {
            "type": "manual"
        }
    },
    "actions": {
        "readData": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "http://myurl"
            }
        },
        "postToErrorMessageQueue": {
            "type": "ApiConnection",
            "inputs": "...",
            "runAfter": {
                "readData": ["Failed"]
            }
        }
    },
    "outputs": {}
}
```

Voit tehdä käyttäminen `runAfter` ominaisuuden avulla voit määrittää `postToErrorMessageQueue` Suorita vain `readData` on **epäonnistunut**.  Tämä myös voi olla luettelon mahdollisista arvoista, joten `runAfter` voi olla `["Succeeded", "Failed"]`.

Lopuksi voit nyt käsitellä virheen, koska emme enää merkitse Suorita **epäonnistui**. Kuten näet tähän, kyseisen esiintymän on **onnistui** vaikka askel epäonnistui, koska voin kirjoittamasi vaihe käsittelemään tämän virheen.

## <a name="two-or-more-steps-that-execute-in-parallel"></a>Kahden (tai useamman) vaiheet, joiden suorittaminen rinnakkain

On useita toimintojen suorittaminen rinnakkain `runAfter` ominaisuus on vastattava suorituksen aikana. 

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {},
    "triggers": {
        "manual": {
            "type": "manual"
        }
    },
    "actions": {
        "readData": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "http://myurl"
            }
        },
        "branch1": {
            "type": "Http",
            "inputs": "...",
            "runAfter": {
                "readData": ["Succeeded"]
            }
        },
        "branch2": {
            "type": "Http",
            "inputs": "...",
            "runAfter": {
                "readData": ["Succeeded"]
            }
        }
    },
    "outputs": {}
}
```

Kuten näet yllä molemmat olevassa esimerkissä `branch1` ja `branch2` on määritetty jälkeen `readData`. Tämän vuoksi molemmat nämä haaran suoritetaan rinnakkain:

![Rinnakkain](./media/app-service-logic-author-definitions/parallel.png)

Voit tarkastella sekä haaroja aikaleiman on sama kuin. 

## <a name="join-two-parallel-branches"></a>Yhdistä kaksi Rinnakkaiset haarat

Voit liittyä kaksi toiminnot, jotka on asetettu suorittamaan rinnakkain lisäämällä kohteita `runAfter` ominaisuutta vastaava yläpuolella.

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-04-01-preview/workflowdefinition.json#",
    "actions": {
        "readData": {
            "inputs": {
                "method": "GET",
                "uri": "http://myurl"
            },
            "runAfter": {},
            "type": "Http"
        },
        "branch1": {
            "inputs": {
                "method": "GET",
                "uri": "http://myurl"
            },
            "runAfter": {
                "readData": [
                    "Succeeded"
                ]
            },
            "type": "Http"
        },
        "branch2": {
            "inputs": {
                "method": "GET",
                "uri": "http://myurl"
            },
            "runAfter": {
                "readData": [
                    "Succeeded"
                ]
            },
            "type": "Http"
        },
        "join": {
            "inputs": {
                "method": "GET",
                "uri": "http://myurl"
            },
            "runAfter": {
                "branch1": [
                    "Succeeded"
                ],
                "branch2": [
                    "Succeeded"
                ]
            },
            "type": "Http"
        }
    },
    "contentVersion": "1.0.0.0",
    "outputs": {},
    "parameters": {},
    "triggers": {
        "manual": {
            "inputs": {
                "schema": {}
            },
            "kind": "Http",
            "type": "Request"
        }
    }
}
```

![Rinnakkain](./media/app-service-logic-author-definitions/join.png)

## <a name="mapping-items-in-a-list-to-some-different-configuration"></a>Luettelon kohteiden linkittäminen joitakin eri määritys

Seuraavaksi oletetaan, että haluamme auttaa kokonaan eri sisällön ominaisuuden arvon mukaan. Voit luoda kartan arvojen kohteisiin parametrina:  

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "specialCategories": {
            "defaultValue": ["science", "google", "microsoft", "robots", "NSA"],
            "type": "Array"
        },
        "destinationMap": {
            "defaultValue": {
                "science": "http://www.nasa.gov",
                "microsoft": "https://www.microsoft.com/en-us/default.aspx",
                "google": "https://www.google.com",
                "robots": "https://en.wikipedia.org/wiki/Robot",
                "NSA": "https://www.nsa.gov/"
            },
            "type": "Object"
        }
    },
    "triggers": {
        "manual": {
            "type": "manual"
        }
    },
    "actions": {
        "getArticles": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "https://ajax.googleapis.com/ajax/services/feed/load?v=1.0&q=http://feeds.wired.com/wired/index"
            },
            "conditions": []
        },
        "getSpecialPage": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "@parameters('destinationMap')[first(intersection(item().categories, parameters('specialCategories')))]"
            },
            "conditions": [{
                "expression": "@greater(length(intersection(item().categories, parameters('specialCategories'))), 0)"
            }],
            "forEach": "@body('getArticles').responseData.feed.entries"
        }
    }
}
```

Tässä tapauksessa emme ensin saat luettelon käytettävissä artikkelit ja sitten toinen vaihe etsii kartan, luokka, joka on määritetty parametrina, minkä URL-Osoitteen hankkiminen sisällön perusteella. 

Kaksi kohdetta, voit kiinnittää huomiota tähän: [`intersection()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#intersection) -funktiota käytetään, tarkista, vastaako luokkaan yksi määritetty tunnetut luokka. Kun olemme saada luokka, on se, tuoda kartan avulla hakasulkeisiin kohde: `parameters[...]`. 

## <a name="working-with-strings"></a>Merkkijonojen käsittelemisestä

Useilla toimintoja, joiden avulla voidaan käsitellä merkkijono. Voit esimerkki, jossa on merkkijono, joka siirtää järjestelmän haluamme, mutta emme ole varma, että merkkien koodaus käsitellään oikein. Yksi vaihtoehto on base64 koodata merkkijono. Kuitenkin välttää vuotamaan URL-osoitteen tässä tapauksessa korvaa muutaman merkin. 

Haluamme alimerkkijonon myös tilauksen nimeä, koska ensin 5 merkkiä ei käytetä.

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "order": {
            "defaultValue": {
                "quantity": 10,
                "id": "myorder1",
                "orderer": "NAME=Stèphén__Šīçiłianö"
            },
            "type": "Object"
        }
    },
    "triggers": {
        "manual": {
            "type": "manual"
        }
    },
    "actions": {
        "order": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "http://www.example.com/?id=@{replace(replace(base64(substring(parameters('order').orderer,5,sub(length(parameters('order').orderer), 5) )),'+','-') ,'/' ,'_' )}"
            }
        }
    },
    "outputs": {}
}
```

Toimi inside out:

1. Hae [`length()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#length) orderer nimen merkkien kokonaismäärän Tämä palauttaa takaisin

2. Vähentää 5 (koska haluamme lyhyempi merkkijono)

3. Todellisuudessa vievät [`substring()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#substring) . Aluksi indeksissä `5` ja valitse loput merkkijonon.

4. Muuntaa tämän alimerkkijono, [`base64()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#base64) merkkijono

5. [`replace()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#replace)kaikki `+` merkkien kanssa`-`

6. [`replace()`](https://msdn.microsoft.com/library/azure/mt643789.aspx#replace)kaikki `/` merkkien kanssa`_`

## <a name="working-with-date-times"></a>Päivämäärän kellonaikojen käsitteleminen

Päivämäärä-aikoja voivat olla hyödyllisiä, erityisesti silloin, kun yrität tuoda tietoja tietolähteestä, joka ei tue luonnollisesti **Käynnistimet**.  Voit myös selvittää, kuinka kauan vaiheista kestää päivämäärä kertaa. 

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "order": {
            "defaultValue": {
                "quantity": 10,
                "id": "myorder1"
            },
            "type": "Object"
        }
    },
    "triggers": {
        "manual": {
            "type": "manual"
        }
    },
    "actions": {
        "order": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "http://www.example.com/?id=@{parameters('order').id}"
            }
        },
        "timingWarning": {
            "actions" {
                "type": "Http",
                "inputs": {
                    "method": "GET",
                    "uri": "http://www.example.com/?recordLongOrderTime=@{parameters('order').id}&currentTime=@{utcNow('r')}"
                },
                "runAfter": {}
            }
            "expression": "@less(actions('order').startTime,addseconds(utcNow(),-1))"
        }
    },
    "outputs": {}
}
```

Tässä esimerkissä on puretaan `startTime` edellisessä vaiheessa. Olemme ovat käytön nykyisen kellonajan ja vähentämällä sekunnin:[`addseconds(..., -1)`](https://msdn.microsoft.com/library/azure/mt643789.aspx#addseconds) (Voit käyttää muita aikayksiköiden kuten `minutes` tai `hours`). Lopuksi että voit verrata nämä kaksi arvoa. Jos ensimmäinen on pienempi kuin toinen ja sitten sanoma tarkoittaa enemmän kuin yksi sekunti on kulunut tilaus on ensimmäinen kohta. 

Huomaa myös, että Microsoft käyttää merkkijonon formatters päivämäärien muotoilu: käytetään kyselymerkkijonon [`utcnow('r')`](https://msdn.microsoft.com/library/azure/mt643789.aspx#utcnow) saat RFC1123. Kaikki päivämäärä [on kuvattu MSDN-](https://msdn.microsoft.com/library/azure/mt643789.aspx#utcnow)muotoilun. 

## <a name="using-deployment-time-parameters-for-different-environments"></a>Käyttöönoton / aika-parametreilla eri ympäristöissä

On yhteinen on käyttöönottoprosessin aikana joissa kehitysympäristö, väliaikaisen ympäristön ja sitten tuotantoympäristössä. Kaikkien näiden saattaa haluat sama määritys, mutta käyttää eri tietokantoja, kuten. Voit vastaavasti sama määritys käyttää useita eri alueiden suuren käytettävyyden, mutta haluat logiikan app esiintymien jutella kyseisen alueen tietokannan. 

Huomaa, että se eroaa ottaen parametrien *runtime*, kannattaa käyttää `trigger()` toimi kuten edellä korostettuina. 

Voit aloittaa hyvin simplistic määritelmä seuraavanlainen:

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "uri": {
            "type": "string"
        }
    },
    "triggers": {
        "manual": {
            "type": "manual"
        }
    },
    "actions": {
        "readData": {
            "type": "Http",
            "inputs": {
                "method": "GET",
                "uri": "@parameters('uri')"
            }
        }
    },
    "outputs": {}
}
```

Valitse todellinen- `PUT` pyytää logiikan sovelluksen, voit antaa parametrin `uri`. Huomaa, että niin ei ole enää oletusarvon tämä parametri on käytössä pakollinen logiikan sovelluksen tiedot:

```
{
    "properties": {},
        "definition": {
          // Use the definition from above here
        },
        "parameters": {
            "connection": {
                "value": "https://my.connection.that.is.per.enviornment"
            }
        }
    },
    "location": "westus"
}
``` 

Kunkin ympäristössä, valitse Anna eri arvo `connection` parametria. 

On kaikkien luomisen ja ylläpidon logiikan sovellukset on asetukset [REST API-ohjeissa](https://msdn.microsoft.com/library/azure/mt643787.aspx) . 
