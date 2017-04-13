<properties 
    pageTitle="Uuden mallin versio 2015-08-01-esikatselu" 
    description="Opettele kirjoittamaan JSON määrittelyn logiikan sovelluksia uusin versio" 
    authors="stepsic-microsoft-com" 
    manager="dwrede" 
    editor="" 
    services="logic-apps" 
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/31/2016"
    ms.author="stepsic"/>
    
# <a name="new-schema-version-2015-08-01-preview"></a>Uuden mallin versio 2015-08-01-esikatselu

Uusi malli ja API logiikan sovellusten versio on parannuksia, mikä parantaa luotettavuutta ja helpottaminen käyttäminen logiikan sovelluksia. On 4 keskeisiä eroja:

1. **APIApp** toiminnon tyyppi on päivitetty uusi **APIConnection** toiminnon tyyppi.
2. **Toista** on nimetty **Foreach**.
3. **HTTP-kuuntelutoiminnon** API-sovellusta ei enää tarvita.
4. Uuden rakenteen käyttää kutsumista lapsen työnkulkuja.

## <a name="1-moving-to-api-connections"></a>1. siirtäminen API yhteydet

Suurimmista muutos on, että et enää tarvitse Azure-tilaukseen käyttämään API-Ohjelmointirajapinnan Apps-sovellusten käyttöön. Voit käyttää ohjelmointirajapinnan 2 tavoilla:
* Hallitun Ohjelmointirajapinnan
* Oman mukautetun Web API

Kaikkien näiden käsitellään hieman eri tavalla, koska niiden hallinta ja isännöinnissä mallit ovat erilaiset. Tämän mallin etu on on rajoitettu resursseille, jotka on otettu enää resurssi-ryhmässä. 

### <a name="managed-apis"></a>Hallitun ohjelmointirajapinnan

Useilla API, joka hallitsee Microsoft puolestasi, kuten Office 365: ssä, Salesforce, Twitter-FTP... Joitain näistä hallitun Ohjelmointirajapinnan voidaan käyttää-on, kuten Bing kääntää, samalla, kun taas joissakin on käytettävä määritys. Tässä määrityksessä kutsutaan *yhteys*.

Esimerkiksi kun käytät Office 365: n, haluat muodostaa yhteyden, joka sisältää Office 365-tunnuksen. Tunnuksessa tallennettu ja päivittää niin, että logiikan sovelluksen aina soittaa Office 365-Ohjelmointirajapinnan suojatusti. Vaihtoehtoisesti, jos haluat muodostaa yhteyden SQL- tai FTP-palvelimeen, haluat muodostaa yhteyden, jossa on yhteysmerkkijonon. 

Määritys sisältyy nämä toiminnot kutsutaan `APIConnection`. Tässä on esimerkki yhteys, joka soittaa Office 365: ssä ja Lähetä sähköpostiviesti:

```
{
    "actions": {
        "Send_Email": {
            "type": "ApiConnection",
            "inputs": {
                "host": {
                    "api": {
                        "runtimeUrl": "https://msmanaged-na.azure-apim.net/apim/office365"
                    },
                    "connection": {
                        "name": "@parameters('$connections')['shared_office365']['connectionId']"
                    }
                },
                "method": "post",
                "body": {
                    "Subject": "Reminder",
                    "Body": "Don't forget!",
                    "To": "me@contoso.com"
                },
                "path": "/Mail"
            }
        }
    }
}
```

Syötteiden osa, joka on yksilöivä API yhteydet on `host` objekti. Tämä on kaksiosainen: `api` ja `connection`.

`api` On URL-osoite, johon, hallittu Ohjelmointirajapinta nykyisessä runtime. Voit tarkastella kaikkia käytettävissä hallitun ohjelmointirajapinnan puolestasi soittamalla `GET https://management.azure.com/subscriptions/{subid}/providers/Microsoft.Web/managedApis/?api-version=2015-08-01-preview`.

Kun käytät Ohjelmointirajapinta, voi tai ei ehkä ole määritetty **yhteyttä parametrit** . Jos se ei ole **yhteyttä** ei tarvita. Jos näin on, tarvitse muodostaa yhteyden. Kun luot yhteyden se on valitsemasi nimi ja sitten, että viittaus `connection` objektin sisällä `host` objekti. Yhteyden luominen resurssiryhmä, Soita:

```
PUT https://management.azure.com/subscriptions/{subid}/resourceGroups/{rgname}/providers/Microsoft.Web/connections/{name}?api-version=2015-08-01-preview
```

Kanssa jotakin seuraavista:


```
{
  "properties": {
    "api": {
      "id": "/subscriptions/{subid}/providers/Microsoft.Web/managedApis/azureblob"
    },
    "parameterValues" : {
        "accountName" : "{The name of the storage account -- the set of parameters is different for each API}"
    }
  },
  "location" : "{Logic app's location}"
}
```

### <a name="deploying-managed-apis-in-an-azure-resource-manager-template"></a>Käyttöönotto hallitun ohjelmointirajapinnan mallissa Azure resurssien hallinta

Voit luoda koko sovelluksen ARM-mallissa, kunhan se ei edellytä vuorovaikutteinen kirjautuminen. Jos se vaatii kirjautumisen, voit määrittää kaikki ARM-mallin avulla, mutta on edelleen käy sallivat yhteydet. 

```
    "resources": [{
        "apiVersion": "2015-08-01-preview",
        "name": "azureblob",
        "type": "Microsoft.Web/connections",
        "location": "[resourceGroup().location]",
        "properties": {
            "api": {
                "id": "[concat(subscription().id,'/providers/Microsoft.Web/locations/westus/managedApis/azureblob')]"
            },
            "parameterValues": {
                "accountName": "[parameters('storageAccountName')]",
                "accessKey": "[parameters('storageAccountKey')]"
            }
        }
    }, {
        "type": "Microsoft.Logic/workflows",
        "apiVersion": "2015-08-01-preview",
        "name": "[parameters('logicAppName')]",
        "location": "[resourceGroup().location]",
        "dependsOn": [
            "[resourceId('Microsoft.Web/connections', 'azureblob')]"
        ],
        "properties": {
            "sku": {
                "name": "[parameters('sku')]",
                "plan": {
                    "id": "[concat(resourceGroup().id, '/providers/Microsoft.Web/serverfarms/',parameters('svcPlanName'))]"
                }
            },
            "definition": {
                "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2015-08-01-preview/workflowdefinition.json#",
                "actions": {
                    "Create_file": {
                        "type": "apiconnection",
                        "inputs": {
                            "host": {
                                "api": {
                                    "runtimeUrl": "https://logic-apis-westus.azure-apim.net/apim/azureblob"
                                },
                                "connection": {
                                    "name": "@parameters('$connections')['azureblob']['connectionId']"
                                }
                            },
                            "method": "post",
                            "queries": {
                                "folderPath": "[concat('/',parameters('containerName'))]",
                                "name": "helloworld.txt"
                            },
                            "body": "@decodeDataUri('data:,Hello+world!')",
                            "path": "/datasets/default/files"
                        },
                        "conditions": []
                    }
                },
                "contentVersion": "1.0.0.0",
                "outputs": {},
                "parameters": {
                    "$connections": {
                        "defaultValue": {},
                        "type": "Object"
                    }
                },
                "triggers": {
                    "recurrence": {
                        "type": "Recurrence",
                        "recurrence": {
                            "frequency": "Day",
                            "interval": 1
                        }
                    }
                }
            },
            "parameters": {
                "$connections": {
                    "value": {
                        "azureblob": {
                            "connectionId": "[concat(resourceGroup().id,'/providers/Microsoft.Web/connections/azureblob')]",
                            "connectionName": "azureblob",
                            "id": "[concat(subscription().id,'/providers/Microsoft.Web/locations/westus/managedApis/azureblob')]"
                        }

                    }
                }
            }
        }
    }]
```

Näet tämän esimerkin, että yhteydet on vain Normaali resursseja, jotka asuvat resurssi-ryhmässä. Käytettävissä olevat tilauksen managedAPIs ne viittaavat.

### <a name="your-custom-web-apis"></a>Mukautetut verkko-ohjelmointirajapinnan

Jos käytät omaa API (tarkemmin sanottuna ei Microsoftin hallitsemaan niistä), sinun pitäisi käyttää sisäistä **HTTP** -toimintoa Soita ne. Ole paras mahdollinen kokemusta, jotta pitäisi näyttää swagger päätepisteen oman API. Tämä ottaa käyttöön logiikan app suunnittelija hahmontamiseen syötteiden ja tulostaa oman API. Ilman swagger suunnittelija vain saa oikeuden Näytä syötteiden ja tulostaa peittävä JSON-objekteina.

Tässä on esimerkki, jossa näkyy uusi `metadata.apiDefinitionUrl` ominaisuus:
```
{
   "actions": {
        "mycustomAPI": {
            "type": "http",
            "metadata" : {
              "apiDefinitionUrl" : "https://mysite.azurewebsites.net/api/apidef/"  
            },
            "inputs": {
                "uri": "https://mysite.azurewebsites.net/api/getsomedata",
                "method" : "GET"
            }
        }
    }
}
```

Jos isännöit verkko-Ohjelmointirajapinnan **App** palvelun sitten se näkyy automaattisesti-luettelo toiminnoista, jotka ovat käytettävissä suunnittelussa. Jos et, sinun on Liitä URL-osoitteen suoraan. Swagger päätepisteen on todentamaton ollakseni käytettävä sisältyy logiikan sovellusten suunnittelu (vaikka voi suojata itse Ohjelmointirajapinnan kanssa riippumatta menetelmiä tuetaan Swagger).

### <a name="using-your-already-deployed-api-apps-with-2015-08-01-preview"></a>Jo käyttöön API-sovellusten käyttäminen 2015-08-01-esikatselu

Jos olet aiemmin käyttöön API-sovelluksen, voit soittaa **HTTP** -toiminnon avulla.

Esimerkiksi jos käytät Dropbox-tiedostojen, olet saattanut seuraavankaltaiselta **2014 – 12-01 – Esikatsele** rakenne-versio-määritys

```
{
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2014-12-01-preview/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "/subscriptions/423db32d-...-b59f14c962f1/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector/token": {
            "defaultValue": "eyJ0eX...wCn90",
            "type": "String",
            "metadata": {
                "token": {
                    "name": "/subscriptions/423db32d-...-b59f14c962f1/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector/token"
                }
            }
        }
    },
    "actions": {
        "dropboxconnector": {
            "type": "ApiApp",
            "inputs": {
                "apiVersion": "2015-01-14",
                "host": {
                    "id": "/subscriptions/423db32d-...-b59f14c962f1/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector",
                    "gateway": "https://avdemo.azurewebsites.net"
                },
                "operation": "ListFiles",
                "parameters": {
                    "FolderPath": "/myfolder"
                },
                "authentication": {
                    "type": "Raw",
                    "scheme": "Zumo",
                    "parameter": "@parameters('/subscriptions/423db32d-...-b59f14c962f1/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector/token')"
                }
            }
        }
    }
}
```

Voit luoda vastaavat HTTP-toiminnon kuten alla (logiikan app määritelmä muutu parametrit-osassa):

```
{
    "actions": {
        "dropboxconnector": {
            "type": "Http",
            "metadata" : {
              "apiDefinitionUrl" : "https://avdemo.azurewebsites.net/api/service/apidef/dropboxconnector/?api-version=2015-01-14&format=swagger-2.0-standard"  
            },
            "inputs": {
                "uri": "https://avdemo.azurewebsites.net/api/service/invoke/dropboxconnector/ListFiles?api-version=2015-01-14",
                "method" : "POST",
                "body": {
                    "FolderPath": "/myfolder"
                },
                "authentication": {
                    "type": "Raw",
                    "scheme": "Zumo",
                    "parameter": "@parameters('/subscriptions/423db32d-...-b59f14c962f1/resourcegroups/avdemo/providers/Microsoft.AppService/apiapps/dropboxconnector/token')"
                }
            }
        }
    }
}
```

Ominaisuussäilöjen – näiden ominaisuuksien yksi kerrallaan:

| Toiminto-ominaisuus |  Kuvaus |
| --------------- | -----------  |
| `type` | `Http`Sijasta`APIapp` |
| `metadata.apiDefinitionUrl` | Jos haluat käyttää tätä toimintoa logiikan sovellusten suunnittelussa kannattaa sisällyttää metatietojen päätepiste. Tämä muodostetaan kohteesta:`{api app host.gateway}/api/service/apidef/{last segment of the api app host.id}/?api-version=2015-01-14&format=swagger-2.0-standard` |
| `inputs.uri` | Tämä muodostetaan kohteesta:`{api app host.gateway}/api/service/invoke/{last segment of the api app host.id}/{api app operation}?api-version=2015-01-14` |
| `inputs.method` | Aina`POST` |
| `inputs.body` | Sama kuin api sovelluksen parametrit | 
| `inputs.authentication` | Sama kuin api sovelluksen todennus |

Tämän menetelmän pitäisi toimia kaikki API-sovelluksen toimintoja. Valitse Jatka muista, että nämä edellisen API-sovellukset ei enää tueta ja sinun on siirrettävä yhteen kaksi muut haluamasi asetukset (joko hallitun Ohjelmointirajapinnan eli isännöinnin mukautettuja verkko-Ohjelmointirajapinnan).

## <a name="2-repeat-renamed-to-foreach"></a>2. nimetty Foreach toista

Olemme asiakaspalautteen paljon saaneet edellisen rakenne-version, **Toista** oli sekavaa ja ei ole oikein sieppaaminen, että se on todella silmukassa varten. Tuloksena on on nimetä uudelleen sen **Foreach**. Esimerkki:

```
{
    "actions": {
        "pingBing": {
            "type": "Http",
            "repeat": "@range(0,2)",
            "inputs": {
                "method": "GET",
                "uri": "https://www.bing.com/search?q=@{repeatItem()}"
            }
        }
    }
}
```

Nyt kirjoitettavat nimellä:

```
{
    "actions": {
        "pingBing": {
            "type": "Http",
            "foreach": "@range(0,2)",
            "inputs": {
                "method": "GET",
                "uri": "https://www.bing.com/search?q=@{item()}"
            }
        }
    }
}
```

Aiemmin funktion `@repeatItem()` käytettiin viittaa nykyisen kohteen parhaillaan iteroitava päälle. Tämä on helpotettu juuri `@item()`. 

### <a name="referencing-the-outputs-of-the-foreach"></a>Viittaaminen Foreach tulostus
Voit selkeyttää, **Foreach** toiminnot tulostaa ei voi rivittää objektin, jota kutsutaan **repeatItems**. Tämä tarkoittaa sitä, olisi edellä toista tulostus on:

```
{
    "repeatItems": [
        {
            "name": "pingBing",
            "inputs": {
                "uri": "https://www.bing.com/search?q=0",
                "method": "GET"
            },
            "outputs": {
                "headers": { },
                "body": "<!DOCTYPE html><html lang=\"en\" xml:lang=\"en\" xmlns=\"http://www.w3.org/1999/xhtml\" xmlns:Web=\"http://schemas.live.com/Web/\">...</html>"
            }
            "status": "Succeeded"
        }
    ]
}
```

Kun se on:

```
[
    {
        "name": "pingBing",
        "inputs": {
            "uri": "https://www.bing.com/search?q=0",
            "method": "GET"
        },
        "outputs": {
            "headers": { },
            "body": "<!DOCTYPE html><html lang=\"en\" xml:lang=\"en\" xmlns=\"http://www.w3.org/1999/xhtml\" xmlns:Web=\"http://schemas.live.com/Web/\">...</html>"
        }
        "status": "Succeeded"
    }
]
```

Viittaat nämä tulostus, sinun on saat toiminnon leipätekstiin Älä:

```
{
    "actions": {
        "secondAction" : {
            "type" : "Http",
            "repeat" : "@outputs('pingBing').repeatItems",
            "inputs" : {
                "method" : "POST",
                "uri" : "http://www.example.com",
                "body" : "@repeatItem().outputs.body"
            }
        }
    }
}
```

Nyt voit tehdä sen sijaan:

```
{
    "actions": {
        "secondAction" : {
            "type" : "Http",
            "foreach" : "@outputs('pingBing')",
            "inputs" : {
                "method" : "POST",
                "uri" : "http://www.example.com",
                "body" : "@item().outputs.body"
            }
        }
    }
}
```

Nämä muutokset funktiot `@repeatItem()`, `@repeatBody()` ja `@repeatOutputs()` poistetaan.

## <a name="3-native-http-listener"></a>3. alkuperäisen HTTP-kuuntelutoiminnon 
HTTP-kuuntelutoiminnon-ominaisuuksia on nyt sisäisiä, joten sinun tarvitse enää HTTP Listener API-sovelluksen käyttöönotto. Lue lisätietoja [siitä, miten voi tehdä logiikan app päätepiste kutsuttava tähän tarkat tiedot](app-service-logic-http-endpoint.md). 

Nämä muutokset, funktio `@accessKeys()` poistetaan ja on korvattu `@listCallbackURL()` tarkoitetaan käytön päätepisteen (tarvittaessa)-funktio. Lisäksi nyt on määritettävä vähintään yksi käynnistin logiikan sovelluksen nyt. Jos haluat `/run` työnkulun, sinun on tuettava jotakin `manual`, `apiConnectionWebhook` tai `httpWebhook` käynnistimien. 

## <a name="4-calling-child-workflows"></a>4. lapsen työnkulut soitat

Aiemmin kutsumista lapsen työnkulut pakollinen työnkulun siirtymällä, käytön käyttöoikeustietue ja liittämiseen, että logiikan sovellus, jonka haluat kutsua lapsen määritelmän. Uusi rakenne-versiolla logiikan sovellukset ohjelma luo automaattisesti SAS suorituksen ali-työnkulun, mikä tarkoittaa, että sinun ei tarvitse mitään tietoja liittäminen määritelmän.  Tässä on esimerkki:

```
"mynestedwf" : {
    "type" : "workflow",
    "inputs" : {
        "host" : {
            "id" : "/subscriptions/xxxxyyyyzzz/resourceGroups/rg001/providers/Microsoft.Logic/mywf001",
            "triggerName" : "myendpointtrigger"
        },
        "queries" : {
            "extrafield" : "specialValue"
        },
        "headers" : {
            "x-ms-date" : "@utcnow()",
            "Content-type" : "application/json"
        },
        "body" : {
            "contentFieldOne" : "value100",
            "anotherField" : 10.001
        }
    },
    "conditions" : []
}
```

Toinen Käyttömukavuuden on on olla sivukomentojen alityönkulkujen täydet saapuvan pyynnön. Tämä tarkoittaa, että voit välittää parametrit *Kyselyt* -osassa ja *otsikot* -objekti ja että voit määrittää täysin koko teksti.

Lopuksi on Aliobjektin työnkulkuun tarvittavat muutokset. Tämän vuoksi ennen kuin voi vain soitat lapsen työnkulun suoraan. Nyt tarvitset ehkä määrittää käynnistimen päätepisteen ylemmän tason Soita työnkulun. Tämä tarkoittaa yleensä, Lisää, kirjoita **Manuaalinen** käynnistäminen ja käyttää sitä sitten ylemmän tason määrityksessä. Huomaa, että `host` ominaisuus on erityisesti `triggerName`, koska mitä käynnistimen keskustelunäkymässä on määritettävä aina.

## <a name="other-changes"></a>Muut muutokset

### <a name="new-queries-property"></a>Uusi kyselyt-ominaisuus
Kaikki tyypit tukevat uusi kutsutaan **Kyselyt**-syöte. Tämä on rakenteellisia objektin sijaan voit ottaa tarvitaan merkkijonon käsin.

### <a name="parse-function-renamed"></a>nimetty Parse()-funktio
Kun olemme pian lisäämiseen muita sisältötyyppejä `parse()` funktio on nimetty `json()`.

## <a name="coming-soon-enterprise-integration-apis"></a>Tulossa pian: yrityksen integrointi API
Tässä vaiheessa kohdassa kerralla Microsoft ei vielä on hallitun versioiden yrityksen integrointi API käytettävissä (esimerkiksi AS2). Nämä olla tulossa pian kuuluvat [ohjeet](http://www.zdnet.com/article/microsoft-outlines-its-cloud-and-server-integration-roadmap-for-2016/). Sillä että aiemmin käyttöön BizTalk ohjelmointirajapinnan HTTP-toiminnon avulla voit käyttää kuin yllä "Käyttämällä jo käyttöön API-sovellusten."
