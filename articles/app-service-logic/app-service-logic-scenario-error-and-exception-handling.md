<properties
    pageTitle="Kirjaaminen ja virheiden käsittelyä koskevat logiikan sovelluksissa | Microsoft Azure"
    description="Näytä lisäasetukset virheen käsittely ja logiikka sovelluksilla kirjaamisen käytännön Käyttötapaus"
    keywords=""
    services="logic-apps"
    authors="hedidin"
    manager="anneta"
    editor=""
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/29/2016"
    ms.author="b-hoedid"/>

# <a name="logging-and-error-handling-in-logic-apps"></a>Kirjaaminen ja virheiden käsittelyä koskevat logiikan-sovelluksissa

Tässä artikkelissa kuvataan, miten voit laajentaa logiikan-sovellus tukee paremmin poikkeuksen käsittely. Käytännön Käyttötapaus ja tutustu vastaus kysymykseen, "Logiikan sovellusten tukeeko poikkeuksen ja Virheenkäsittely?"

>[AZURE.NOTE]Nykyisen version Microsoft Azure App palvelun logiikan sovellukset-ominaisuus on vakio malli toiminnon vastaukset.
>Tämä sisältää sekä sisäinen vahvistus virhe vastaukset palautetaan API-sovelluksen.

## <a name="overview-of-the-use-case-and-scenario"></a>Käyttötapaus ja skenaarion yhteenveto

Seuraavat Tarinan on tämän artikkelin käyttötapaus.
Tunnettujen terveydenhuollon organisaatiossa käytetään meitä kehittämään Azure ratkaisu, joka loisi Potilas portal käyttämällä Microsoft Dynamics CRM Online. Ne tarvittavat Lähetä tapaamisen Dynamics CRM Online Potilas portaalin ja Salesforce välillä olevat tietueet.  Microsoft on pyytää käytetään kaikkien Potilas tietueiden [HL7 FHIR](http://www.hl7.org/implement/standards/fhir/) -standardia.

Projektin oli kaksi tärkeintä vaatimukset:  

 -  Menetelmä Kirjaudu tietueet, jotka on lähetetty Dynamics CRM Online-portaalista
 -  Tapa tarkastella työnkulun sisällä tapahtuneista virheistä


## <a name="how-we-solved-the-problem"></a>Miten on ratkaistu ongelma

>[AZURE.TIP] Voit tarkastella projektin korkean tason videon [Integrointi käyttäjäryhmä]-(http://www.integrationusergroup.com/do-logic-apps-support-error-handling/ "Integroinnin käyttäjäryhmä").

Microsoft valitsi [Azure DocumentDB](https://azure.microsoft.com/services/documentdb/ "Azure DocumentDB") nimellä (DocumentDB viittaa tietueiden tiedostoina) log- ja virhetietoja tietueet säilö. Logiikan sovellukset on vakio malli kaikki vastaukset, Microsoft ei olisi mukautetun rakenteen luomista. Emme voi luoda API apusovellus **ja **kyselyn** ** virhe ja kirjaudu tietueita. Emme voi määrittää rakenteen myös kunkin API-sovelluksen.  

Toisen vaatimus oli Tyhjennä tietueet tietyn päivämäärän jälkeen. DocumentDB on nimeltään [elinaika](https://azure.microsoft.com/blog/documentdb-now-supports-time-to-live-ttl/ "elinaika") (TTL), jota sallittu että Määritä **elinaika** -arvoksi kunkin tietueen tai sivustokokoelman ominaisuus. Tämä on poistettu ei tarvitse poistaa manuaalisesti DocumentDB tietueet.

### <a name="creation-of-the-logic-app"></a>Logiikan-sovelluksen luominen

Ensimmäiseksi on logiikan-sovelluksen luominen ja lataa se suunnittelussa. Tässä esimerkissä on käytössä ylä-ja alatason logiikan sovellukset. Oletetaan, että on jo luonut ylemmän tason ja olet luomassa yhteen lapsen logiikan-sovellukseen.

Koska tarkastellaan on logging tietueen tulossa ulos Dynamics CRM Online-aloitetaan yläreunassa. Käytä pyynnön käynnistimen, koska päätason logiikan sovelluksen käynnistää tämän lapsen annettava.

> [AZURE.IMPORTANT] Viimeistele Tässä opetusohjelmassa, sinun on DocumentDB tietokannan ja kaksi sivustokokoelmat (kirjaaminen ja virheet).

### <a name="logic-app-trigger"></a>Logiikan sovelluksen käynnistin

Esimerkissä on käytössä pyynnön käynnistimen seuraavan esimerkin mukaisesti.

```` json
"triggers": {
        "request": {
          "type": "request",
          "kind": "http",
          "inputs": {
            "schema": {
              "properties": {
                "CRMid": {
                  "type": "string"
                },
                "recordType": {
                  "type": "string"
                },
                "salesforceID": {
                  "type": "string"
                },
                "update": {
                  "type": "boolean"
                }
              },
              "required": [
                "CRMid",
                "recordType",
                "salesforceID",
                "update"
              ],
              "type": "object"
            }
          }
        }
      },

````


### <a name="steps"></a>Vaiheet

Kirjaudu Potilas tietueen lähteen (pyyntö) Dynamics CRM Online-portaalista annettava.

1. Uuden tapaamistietueen noutaminen Dynamics CRM Online annettava.
    CRM tulevat käynnistimen avulla meille **CRM PatentId**, **tietuetyyppi**, **Uusi tai päivitetty tietue** (uusi tai päivitä totuusarvo), ja **SalesforceId**. **SalesforceId** voi olla tyhjä, koska sitä käytetään vain päivityksen.
    Olemme saavat CRM-tietue käyttämällä CRM **PatientID** ja **Tietuetyyppi**.
1. Seuraavaksi annettava Lisää Microsoftin DocumentDB API-sovellus **InsertLogEntry** toiminnon esitetty seuraavassa esitetyllä tavalla.


#### <a name="insert-log-entry-designer-view"></a>Lisää log syöttönäkymä suunnittelu

![Lisää lokitapahtuman](./media/app-service-logic-scenario-error-and-exception-handling/lognewpatient.png)

#### <a name="insert-error-entry-designer-view"></a>Lisää virhe syöttönäkymä suunnittelu
![Lisää lokitapahtuman](./media/app-service-logic-scenario-error-and-exception-handling/insertlogentry.png)

#### <a name="check-for-create-record-failure"></a>Tarkista Luo tietue virhe

![Ehto](./media/app-service-logic-scenario-error-and-exception-handling/condition.png)


## <a name="logic-app-source-code"></a>Logiikan sovelluksen-lähdekoodi

>[AZURE.NOTE]  Seuraavassa on esimerkkejä, joiden vain. Tässä opetusohjelmassa perustuu toteutus olevan tuotannon, koska arvon **Lähde-solmu** ei ehkä näy ominaisuudet, jotka liittyvät tapaamisen ajoittaminen.

### <a name="logging"></a>Lokiin kirjaaminen
Seuraava logiikan app koodin malli näkyy käsittelemisestä kirjaaminen.

#### <a name="log-entry"></a>Lokitapahtuman
Tämä on logiikan app lähdekoodin lisääminen lokitiedosto.

``` json
"InsertLogEntry": {
        "metadata": {
        "apiDefinitionUrl": "https://.../swagger/docs/v1",
        "swaggerSource": "website"
        },
        "type": "Http",
        "inputs": {
        "body": {
            "date": "@{outputs('Gets_NewPatientRecord')['headers']['Date']}",
            "operation": "New Patient",
            "patientId": "@{triggerBody()['CRMid']}",
            "providerId": "@{triggerBody()['providerID']}",
            "source": "@{outputs('Gets_NewPatientRecord')['headers']}"
        },
        "method": "post",
        "uri": "https://.../api/Log"
        },
        "runAfter":    {
            "Gets_NewPatientecord": ["Succeeded"]
        }
}
```

#### <a name="log-request"></a>Log-pyyntö

Tämä on julkaistu API-sovelluksen log pyynnön viestin.

``` json
    {
    "uri": "https://.../api/Log",
    "method": "post",
    "body": {
        "date": "Fri, 10 Jun 2016 22:31:56 GMT",
        "operation": "New Patient",
        "patientId": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0",
        "providerId": "",
        "source": "{\"Pragma\":\"no-cache\",\"x-ms-request-id\":\"e750c9a9-bd48-44c4-bbba-1688b6f8a132\",\"OData-Version\":\"4.0\",\"Cache-Control\":\"no-cache\",\"Date\":\"Fri, 10 Jun 2016 22:31:56 GMT\",\"Set-Cookie\":\"ARRAffinity=785f4334b5e64d2db0b84edcc1b84f1bf37319679aefce206b51510e56fd9770;Path=/;Domain=127.0.0.1\",\"Server\":\"Microsoft-IIS/8.0,Microsoft-HTTPAPI/2.0\",\"X-AspNet-Version\":\"4.0.30319\",\"X-Powered-By\":\"ASP.NET\",\"Content-Length\":\"1935\",\"Content-Type\":\"application/json; odata.metadata=minimal; odata.streaming=true\",\"Expires\":\"-1\"}"
        }
    }

```


#### <a name="log-response"></a>Kirjaa vastaus

Tämä on lokin vastausviesti API-sovelluksen.

``` json
{
    "statusCode": 200,
    "headers": {
        "Pragma": "no-cache",
        "Cache-Control": "no-cache",
        "Date": "Fri, 10 Jun 2016 22:32:17 GMT",
        "Server": "Microsoft-IIS/8.0",
        "X-AspNet-Version": "4.0.30319",
        "X-Powered-By": "ASP.NET",
        "Content-Length": "964",
        "Content-Type": "application/json; charset=utf-8",
        "Expires": "-1"
    },
    "body": {
        "ttl": 2592000,
        "id": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0_1465597937",
        "_rid": "XngRAOT6IQEHAAAAAAAAAA==",
        "_self": "dbs/XngRAA==/colls/XngRAOT6IQE=/docs/XngRAOT6IQEHAAAAAAAAAA==/",
        "_ts": 1465597936,
        "_etag": "\"0400fc2f-0000-0000-0000-575b3ff00000\"",
        "patientID": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0",
        "timestamp": "2016-06-10T22:31:56Z",
        "source": "{\"Pragma\":\"no-cache\",\"x-ms-request-id\":\"e750c9a9-bd48-44c4-bbba-1688b6f8a132\",\"OData-Version\":\"4.0\",\"Cache-Control\":\"no-cache\",\"Date\":\"Fri, 10 Jun 2016 22:31:56 GMT\",\"Set-Cookie\":\"ARRAffinity=785f4334b5e64d2db0b84edcc1b84f1bf37319679aefce206b51510e56fd9770;Path=/;Domain=127.0.0.1\",\"Server\":\"Microsoft-IIS/8.0,Microsoft-HTTPAPI/2.0\",\"X-AspNet-Version\":\"4.0.30319\",\"X-Powered-By\":\"ASP.NET\",\"Content-Length\":\"1935\",\"Content-Type\":\"application/json; odata.metadata=minimal; odata.streaming=true\",\"Expires\":\"-1\"}",
        "operation": "New Patient",
        "salesforceId": "",
        "expired": false
    }
}

```

Nyt katsotaan Virheenkäsittely vaiheet.


### <a name="error-handling"></a>Virheenkäsittely

Seuraava logiikan sovellusten koodi-malli näkyy, kuinka voit ottaa Virheenkäsittely.

#### <a name="create-error-record"></a>Virhetietueen luominen

Tämä on logiikan sovellusten lähdekoodi Virhetietueen luomisessa.

``` json
"actions": {
    "CreateErrorRecord": {
        "metadata": {
        "apiDefinitionUrl": "https://.../swagger/docs/v1",
        "swaggerSource": "website"
        },
        "type": "Http",
        "inputs": {
        "body": {
            "action": "New_Patient",
            "isError": true,
            "crmId": "@{triggerBody()['CRMid']}",
            "patientID": "@{triggerBody()['CRMid']}",
            "message": "@{body('Create_NewPatientRecord')['message']}",
            "providerId": "@{triggerBody()['providerId']}",
            "severity": 4,
            "source": "@{actions('Create_NewPatientRecord')['inputs']['body']}",
            "statusCode": "@{int(outputs('Create_NewPatientRecord')['statusCode'])}",
            "salesforceId": "",
            "update": false
        },
        "method": "post",
        "uri": "https://.../api/CrMtoSfError"
        },
        "runAfter":
        {
            "Create_NewPatientRecord": ["Failed" ]
        }
    }
}          
```

#### <a name="insert-error-into-documentdb--request"></a>Lisäysvirhe yhdeksi DocumentDB--pyytäminen

``` json

{
    "uri": "https://.../api/CrMtoSfError",
    "method": "post",
    "body": {
        "action": "New_Patient",
        "isError": true,
        "crmId": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0",
        "patientId": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0",
        "message": "Salesforce failed to complete task: Message: duplicate value found: Account_ID_MED__c duplicates value on record with id: 001U000001c83gK",
        "providerId": "",
        "severity": 4,
        "salesforceId": "",
        "update": false,
        "source": "{\"Account_Class_vod__c\":\"PRAC\",\"Account_Status_MED__c\":\"I\",\"CRM_HUB_ID__c\":\"6b115f6d-a7ee-e511-80f5-3863bb2eb2d0\",\"Credentials_vod__c\",\"DTC_ID_MED__c\":\"\",\"Fax\":\"\",\"FirstName\":\"A\",\"Gender_vod__c\":\"\",\"IMS_ID__c\":\"\",\"LastName\":\"BAILEY\",\"MasterID_mp__c\":\"\",\"C_ID_MED__c\":\"851588\",\"Middle_vod__c\":\"\",\"NPI_vod__c\":\"\",\"PDRP_MED__c\":false,\"PersonDoNotCall\":false,\"PersonEmail\":\"\",\"PersonHasOptedOutOfEmail\":false,\"PersonHasOptedOutOfFax\":false,\"PersonMobilePhone\":\"\",\"Phone\":\"\",\"Practicing_Specialty__c\":\"FM - FAMILY MEDICINE\",\"Primary_City__c\":\"\",\"Primary_State__c\":\"\",\"Primary_Street_Line2__c\":\"\",\"Primary_Street__c\":\"\",\"Primary_Zip__c\":\"\",\"RecordTypeId\":\"012U0000000JaPWIA0\",\"Request_Date__c\":\"2016-06-10T22:31:55.9647467Z\",\"ONY_ID__c\":\"\",\"Specialty_1_vod__c\":\"\",\"Suffix_vod__c\":\"\",\"Website\":\"\"}",
        "statusCode": "400"
    }
}
```

#### <a name="insert-error-into-documentdb--response"></a>Virhe lisääminen DocumentDB--vastaus


``` json
{
    "statusCode": 200,
    "headers": {
        "Pragma": "no-cache",
        "Cache-Control": "no-cache",
        "Date": "Fri, 10 Jun 2016 22:31:57 GMT",
        "Server": "Microsoft-IIS/8.0",
        "X-AspNet-Version": "4.0.30319",
        "X-Powered-By": "ASP.NET",
        "Content-Length": "1561",
        "Content-Type": "application/json; charset=utf-8",
        "Expires": "-1"
    },
    "body": {
        "id": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0-1465597917",
        "_rid": "sQx2APhVzAA8AAAAAAAAAA==",
        "_self": "dbs/sQx2AA==/colls/sQx2APhVzAA=/docs/sQx2APhVzAA8AAAAAAAAAA==/",
        "_ts": 1465597912,
        "_etag": "\"0c00eaac-0000-0000-0000-575b3fdc0000\"",
        "prescriberId": "6b115f6d-a7ee-e511-80f5-3863bb2eb2d0",
        "timestamp": "2016-06-10T22:31:57.3651027Z",
        "action": "New_Patient",
        "salesforceId": "",
        "update": false,
        "body": "CRM failed to complete task: Message: duplicate value found: CRM_HUB_ID__c duplicates value on record with id: 001U000001c83gK",
        "source": "{\"Account_Class_vod__c\":\"PRAC\",\"Account_Status_MED__c\":\"I\",\"CRM_HUB_ID__c\":\"6b115f6d-a7ee-e511-80f5-3863bb2eb2d0\",\"Credentials_vod__c\":\"DO - Degree level is DO\",\"DTC_ID_MED__c\":\"\",\"Fax\":\"\",\"FirstName\":\"A\",\"Gender_vod__c\":\"\",\"IMS_ID__c\":\"\",\"LastName\":\"BAILEY\",\"MterID_mp__c\":\"\",\"Medicis_ID_MED__c\":\"851588\",\"Middle_vod__c\":\"\",\"NPI_vod__c\":\"\",\"PDRP_MED__c\":false,\"PersonDoNotCall\":false,\"PersonEmail\":\"\",\"PersonHasOptedOutOfEmail\":false,\"PersonHasOptedOutOfFax\":false,\"PersonMobilePhone\":\"\",\"Phone\":\"\",\"Practicing_Specialty__c\":\"FM - FAMILY MEDICINE\",\"Primary_City__c\":\"\",\"Primary_State__c\":\"\",\"Primary_Street_Line2__c\":\"\",\"Primary_Street__c\":\"\",\"Primary_Zip__c\":\"\",\"RecordTypeId\":\"012U0000000JaPWIA0\",\"Request_Date__c\":\"2016-06-10T22:31:55.9647467Z\",\"XXXXXXX\":\"\",\"Specialty_1_vod__c\":\"\",\"Suffix_vod__c\":\"\",\"Website\":\"\"}",
        "code": 400,
        "errors": null,
        "isError": true,
        "severity": 4,
        "notes": null,
        "resolved": 0
        }
}
```

#### <a name="salesforce-error-response"></a>Salesforce-virhe vastausta

``` json
{
    "statusCode": 400,
    "headers": {
        "Pragma": "no-cache",
        "x-ms-request-id": "3e8e4884-288e-4633-972c-8271b2cc912c",
        "X-Content-Type-Options": "nosniff",
        "Cache-Control": "no-cache",
        "Date": "Fri, 10 Jun 2016 22:31:56 GMT",
        "Set-Cookie": "ARRAffinity=785f4334b5e64d2db0b84edcc1b84f1bf37319679aefce206b51510e56fd9770;Path=/;Domain=127.0.0.1",
        "Server": "Microsoft-IIS/8.0,Microsoft-HTTPAPI/2.0",
        "X-AspNet-Version": "4.0.30319",
        "X-Powered-By": "ASP.NET",
        "Content-Length": "205",
        "Content-Type": "application/json; charset=utf-8",
        "Expires": "-1"
    },
    "body": {
        "status": 400,
        "message": "Salesforce failed to complete task: Message: duplicate value found: Account_ID_MED__c duplicates value on record with id: 001U000001c83gK",
        "source": "Salesforce.Common",
        "errors": []
    }
}

```

### <a name="returning-the-response-back-to-the-parent-logic-app"></a>Vastauksen ylätason logiikan sovellus, joka palauttaa

Kun olet vastauksen, voit siirtää takaisin ylemmän tason logiikan sovellus.

#### <a name="return-success-response-to-the-parent-logic-app"></a>Palauttaa onnistumisen vastaus ylemmän tason logiikan-sovellukseen

``` json
"SuccessResponse": {
    "runAfter":
        {
            "UpdateNew_CRMPatientResponse": ["Succeeded"]
        },
    "inputs": {
        "body": {
            "status": "Success"
    },
    "headers": {
    "   Content-type": "application/json",
        "x-ms-date": "@utcnow()"
    },
    "statusCode": 200
    },
    "type": "Response"
}
```

#### <a name="return-error-response-to-the-parent-logic-app"></a>Palauttaa virhevastaus ylemmän tason logiikan-sovellukseen

``` json
"ErrorResponse": {
    "runAfter":
        {
            "Create_NewPatientRecord": ["Failed"]
        },
    "inputs": {
        "body": {
            "status": "BadRequest"
        },
        "headers": {
            "Content-type": "application/json",
            "x-ms-date": "@utcnow()"
        },
        "statusCode": 400
    },
    "type": "Response"
}

```


## <a name="documentdb-repository-and-portal"></a>DocumentDB tietovaraston ja portal

Microsoftin ratkaisun lisätty [DocumentDB](https://azure.microsoft.com/services/documentdb)muita ominaisuuksia.

### <a name="error-management-portal"></a>Virhe hallinta-portaalin

Jos haluat tarkastella virheitä, voit luoda MVC web-sovelluksen virhe tietueiden DocumentDB näytettävä. **Luettelon**, **tiedot**-, **Muokkaa**ja **Poista** -toiminnot sisälly uusimpaan versioon.

> [AZURE.NOTE]Muokkaa toiminnon: DocumentDB onko korvaa koko asiakirjan.
> **Luettelo** -ja **Tietonäkymät** näkyvät tietueet on vain mallit. Ne eivät ole todellinen Potilas tapaamistietueita.

Seuraavassa on esimerkkejä meidän MVC sovelluksen tiedot on luotu edellä kuvattuja lähestymistapa.

#### <a name="error-management-list"></a>Virhe hallinta luettelossa

![Virheluetteloa](./media/app-service-logic-scenario-error-and-exception-handling/errorlist.png)

#### <a name="error-management-detail-view"></a>Virhe hallinta tietonäkymästä

![Virheen yksityiskohtaiset tiedot](./media/app-service-logic-scenario-error-and-exception-handling/errordetails.png)

### <a name="log-management-portal"></a>Lokitiedoston hallinta-portaalin

Voit tarkastella lokeja, luomaamme myös MVC web app-sovelluksessa.  Seuraavassa on esimerkkejä meidän MVC sovelluksen tiedot on luotu edellä kuvattuja lähestymistapa.

#### <a name="sample-log-detail-view"></a>Esimerkki log tietonäkymästä

![Lokitiedoston tietonäkymästä](./media/app-service-logic-scenario-error-and-exception-handling/samplelogdetail.png)

### <a name="api-app-details"></a>API sovelluksen tiedot

#### <a name="logic-apps-exception-management-api"></a>Logiikan sovellusten poikkeuksen hallinta Ohjelmointirajapinta

Tutustu Avaa lähde logiikan sovellusten poikkeuksen hallinta API app sisältää seuraavat ominaisuudet.

On kaksi ohjaimet:

- **ErrorController** Lisää DocumentDB kokoelman virhe-tietue (asiakirja).
- **LogController** Lisää DocumentDB kokoelman log-tietue (asiakirja).

> [AZURE.TIP] Molemmat ohjaimet käyttää `async Task<dynamic>` toimintoja. Näin toimintojen ratkaistava suorituksen, jotta voit luoda DocumentDB rakenteen toiminnon tekstiosaan.

DocumentDB Jokaisella tiedostolla on oltava yksilöllinen tunnus. Esimerkissä on käytössä `PatientId` ja lisäämällä aikaleima, joka on muunnettu Unix aikaleima-arvon (double). Olemme katkaise sen Poista murto-arvo.

Voit tarkastella tämän virheen ohjauskoneen API [-GitHub](https://github.com/HEDIDIN/LogicAppsExceptionManagementApi/blob/master/Logic App Exception Management API/Controllers/ErrorController.cs)lähdekoodia.

Olemme puhelun Ohjelmointirajapinnan logiikan-sovelluksen käyttämällä seuraavaa syntaksia.

``` json
 "actions": {
        "CreateErrorRecord": {
          "metadata": {
            "apiDefinitionUrl": "https://.../swagger/docs/v1",
            "swaggerSource": "website"
          },
          "type": "Http",
          "inputs": {
            "body": {
              "action": "New_Patient",
              "isError": true,
              "crmId": "@{triggerBody()['CRMid']}",
              "prescriberId": "@{triggerBody()['CRMid']}",
              "message": "@{body('Create_NewPatientRecord')['message']}",
              "salesforceId": "@{triggerBody()['salesforceID']}",
              "severity": 4,
              "source": "@{actions('Create_NewPatientRecord')['inputs']['body']}",
              "statusCode": "@{int(outputs('Create_NewPatientRecord')['statusCode'])}",
              "update": false
            },
            "method": "post",
            "uri": "https://.../api/CrMtoSfError"
          },
          "runAfter": {
              "Create_NewPatientRecord": ["Failed"]
            }
        }
 }
```

Edellisen koodin otoksessa lauseke etsii *Create_NewPatientRecord* tilan **epäonnistui**.

## <a name="summary"></a>Yhteenveto

- Voit helposti toteuttaa kirjaaminen ja virheiden käsittelyä koskevat logiikan-sovelluksessa.
- Voit käyttää DocumentDB säilö log- ja virhetietoja tietueiden (tiedostot).
- Voit luoda log- ja virhetietoja tietueiden näyttämisen portaalin MVC.

### <a name="source-code"></a>Lähdekoodin
Lähdekoodin logiikan sovellusten poikkeuksen hallinnan API-sovellus on käytettävissä tämän [GitHub säilöön](https://github.com/HEDIDIN/LogicAppsExceptionManagementApi "Logiikan sovelluksen poikkeuksen hallinnan API").


## <a name="next-steps"></a>Seuraavat vaiheet
- [Näytä Lisää logiikan sovellusten esimerkkejä ja skenaariot](app-service-logic-examples-and-scenarios.md)
- [Lisätietoja Valvontatyökalut logiikan-sovellukset](app-service-logic-monitor-your-logic-apps.md)
- [Logiikan sovelluksen automaattisen käyttöönoton mallin luominen](app-service-logic-create-deploy-template.md)
