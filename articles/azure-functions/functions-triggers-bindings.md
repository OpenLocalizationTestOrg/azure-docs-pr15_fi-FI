<properties
    pageTitle="Azure Funktiot käynnistimien ja sidontojen | Microsoft Azure"
    description="Tietoja käynnistimien ja sidontojen käyttämisestä Azure-funktiot."
    services="functions"
    documentationCenter="na"
    authors="christopheranderson"
    manager="erikre"
    editor=""
    tags=""
    keywords="Azure Funktiot, funktioiden, käsittely, webhooks, dynaaminen suorittaminen, serverless arkkitehtuuri"/>

<tags
    ms.service="functions"
    ms.devlang="multiple"
    ms.topic="reference"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="10/27/2016"
    ms.author="chrande"/>

# <a name="azure-functions-triggers-and-bindings-developer-reference"></a>Azure Funktiot käynnistimien ja sidontojen Sovelluskehittäjän opas


Tämä artikkeli sisältää yleiset viittaus käynnistimien ja sidontojen. Se on sidonnan lisäasetusten ominaisuudet ja sidonta kaikenlaisten tukemat syntaksia.  

Jos etsit lisätietoja määrittäminen sekä coding tietynlaisia käynnistimen tai sidonta, haluat ehkä napsauttamalla jotakin käynnistimen tai sidontojen alla sijaan:

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)] 

Nämä artikkelit oletetaan, että olet lukenut [Azure Funktiot Sovelluskehittäjän opas](functions-reference.md)ja [C#](functions-reference-csharp.md), [F #](functions-reference-fsharp.md)tai [Node.js](functions-reference-node.md) developer-artikkeleissa.



## <a name="overview"></a>Yleiskatsaus

Käynnistimien ovat tapahtuman vastauksia käytetään käynnistettävän mukautettua koodia. Niiden ansiosta voit vastata tapahtumiin Azure ympäristö tai paikalliseen. Sidontojen edustavat yhdistää koodisi haluamasi käynnistimen tai liittyvän syötteen tai siirtää tietoja tarvittavat metatiedot.

Saat paremman käsityksen siitä, voit integroida Azure-funktion sovelluksen eri sidontojen-viitata seuraavassa taulukossa.

[AZURE.INCLUDE [dynamic compute](../../includes/functions-bindings.md)]  

Ymmärtämään Käynnistimet ja sidontojen yleensä oletetaan, että haluat suorittaa lisäkoodin prosessin uuden kohteen pudotettu Azuren tallennustilaan jonon kyselyjä. Azure Funktiot sisältää jonon Azure-käynnistimen tukee tätä. Tarvitset, seurannassa jonossa seuraavat tiedot:
 
- Tallennustilan tili, jos jonossa on olemassa.
- Jonon nimi.
- Muuttujan nimi, jota koodisi käyttäisi viitata uuden kohteen, joka poistettiin jonossa kyselyjä.  
 
Jonon käynnistimen sidonta sisältää Azure-funktion tiedot. Jokaisen funktion *function.json* -tiedosto sisältää kaikki liittyvät sidontojen.  Esimerkki *function.json* , joka sisältää jonon käynnistäminen sidonta. 

    {
      "bindings": [
        {
          "name": "myNewUserQueueItem",
          "type": "queueTrigger",
          "direction": "in",
          "queueName": "queue-newusers",
          "connection": "MY_STORAGE_ACCT_APP_SETTING"
        }
      ],
      "disabled": false
    }

Koodin voi lähettää erityyppisiä tulosteen mukaan uuden jonon kohteen käsittelyn. Haluat esimerkiksi kirjoittaa uuden tietueen Azuren tallennustilaan-taulukkoon.  Voit tehdä tämän, voit määrittää tulostus-sidonta Azuren tallennustilaan taulukoksi. Tässä on esimerkki *function.json* , joka sisältää tallennustilan taulukon tulosteen sidonnan peruskaavoja voidaan käyttää jonon käynnistintä. 


    {
      "bindings": [
        {
          "name": "myNewUserQueueItem",
          "type": "queueTrigger",
          "direction": "in",
          "queueName": "queue-newusers",
          "connection": "MY_STORAGE_ACCT_APP_SETTING"
        },
        {
          "type": "table",
          "name": "myNewUserTableBinding",
          "tableName": "newUserTable",
          "connection": "MY_TABLE_STORAGE_ACCT_APP_SETTING",
          "direction": "out"
        }
      ],
      "disabled": false
    }


Seuraava C#-funktio vastaa kyselyjä jonossa menettämistä uuden kohteen, ja kirjoittaa uuden käyttäjän tekstin Azuren tallennustilaan taulukkoon.

    #r "Newtonsoft.Json"

    using System;
    using Newtonsoft.Json;

    public static async Task Run(string myNewUserQueueItem, IAsyncCollector<Person> myNewUserTableBinding, 
                                    TraceWriter log)
    {
        // In this example the queue item is a JSON string representing an order that contains the name, 
        // address and mobile number of the new customer.
        dynamic order = JsonConvert.DeserializeObject(myNewUserQueueItem);
    
        await myNewUserTableBinding.AddAsync(
            new Person() { 
                PartitionKey = "Test", 
                RowKey = Guid.NewGuid().ToString(), 
                Name = order.name,
                Address = order.address,
                MobileNumber = order.mobileNumber }
            );
    }
    
    public class Person
    {
        public string PartitionKey { get; set; }
        public string RowKey { get; set; }
        public string Name { get; set; }
        public string Address { get; set; }
        public string MobileNumber { get; set; }
    }

Koodiesimerkkejä ja Azure sijainteihin, joita tuetaan tarkempia tietoja on artikkelissa [Azure Funktiot käynnistimien ja Azure-tallennustilan sidontojen](functions-bindings-storage.md).


Azure-portaalissa monimutkaisemman sidonta-ominaisuudet käyttöön napsauttamalla funktion **liittää** -välilehden **muokkaus** -vaihtoehto. Laajennettu editori voi muokata *function.json* suoraan Portalissa.

## <a name="dynamic-parameter-binding"></a>Dynaaminen parametrin sidonta 

Sen sijaan, että tulostus sidonta-ominaisuuksien määrittäminen staattinen määritys voidaan määrittää sitoa dynaamisesti tiedot, jotka ovat osa oman käynnistimen syötteen sidonta asetukset. Harkitse tilanne, jossa uusi tilaukset käsitellään Azuren tallennustilaan jonossa. Kunkin uuden jonon kohde on JSON merkkijonon, joka sisältää vähintään seuraavat ominaisuudet:

    {
      name : "Customer Name",
      address : "Customer's Address".
      mobileNumber : "Customer's mobile number."
    }

Voit haluta lähettäminen asiakkaalle tekstiviestin Twilio tilin käyttö päivityksen kuin tilauksen vastaanotettiin.  Voit määrittää `body` ja `to` oman Twilio kentän tulosteen sitoa dynaamisesti sidonta `name` ja `mobileNumber` , joka on osa syötteen seuraavasti.

    {
      "name": "myNewOrderItem",
      "type": "queueTrigger",
      "direction": "in",
      "queueName": "queue-newOrders",
      "connection": "orders_STORAGE"
    },
    {
      "type": "twilioSms",
      "name": "message",
      "accountSid": "TwilioAccountSid",
      "authToken": "TwilioAuthToken",
      "to": "{mobileNumber}",
      "from": "+XXXYYYZZZZ",
      "body": "Thank you {name}, your order was received",
      "direction": "out"
    },
 
Funktio-koodi on nyt vain alustaa tulostus-parametrin seuraavasti. Suorituksen aikana tulostus-ominaisuuksia sidottava haluamasi syöttötiedot.

C#

    // Even if you want to use a hard coded message and number in the binding, you must at least 
    // initialize the message variable.
    message = new SMSMessage();

    // When using dynamic parameter binding no need to set this in code.
    // message.body = msg;
    // message.to = myNewOrderItem.mobileNumber

Node.js

    context.bindings.message = {
        // When using dynamic parameter binding no need to set this in code.
        //body : msg,
        //to : myNewOrderItem.mobileNumber
    };




## <a name="random-guids"></a>Satunnainen GUID-tunnus

Azure funktiot on syntaksi luomiseen satunnaisia GUID oman sidontojen kanssa. Sidonta syntaksi kirjoittaa tulosteen uusi BLOB Azuren tallennustilaan säilössä yksilöllinen nimi: 

    {
      "type": "blob",
      "name": "blobOutput",
      "direction": "out",
      "path": "my-output-container/{rand-guid}"
    }



## <a name="returning-a-single-output"></a>Palauttaa yhden tulostus

Voit käyttää kohtaa, johon funktio-koodi palauttaa yksittäisen tulosteen tapauksissa tulostus-sidonta, jonka nimi `$return` säilyttää koodisi luonnollista funktion allekirjoitukseen. Tämä voi käyttää vain palautusarvo (C#, Node.js, F #) tukevia kieliä kanssa. Sidonta olisi samalla tavalla kuin seuraavassa blob tulosteen sidonta, jota käytetään jonon käynnistimen kanssa.

    {
      "bindings": [
        {
          "type": "queueTrigger",
          "name": "input",
          "direction": "in",
          "queueName": "test-input-node"
        },
        {
          "type": "blob",
          "name": "$return",
          "direction": "out",
          "path": "test-output-node/{id}"
        }
      ]
    }


Seuraavat C#-koodin palauttaa tulosteen Lisää luonnollisesti käyttämättä `out` parametrin funktion allekirjoituksessa.


    public static string Run(WorkItem input, TraceWriter log)
    {
        string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
        log.Info($"C# script processed queue message. Item={json}");
        return json;
    }

    // Async example
    public static Task<string> Run(WorkItem input, TraceWriter log)
    {
        string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
        log.Info($"C# script processed queue message. Item={json}");
        return json;
    }


Tämän saman menetelmän on osoitettu alla Node.js kanssa.

    module.exports = function (context, input) {
        var json = JSON.stringify(input);
        context.log('Node.js script processed queue message', json);
        context.done(null, json);
    }

F # Esimerkki jäljempänä.

    let Run(input: WorkItem, log: TraceWriter) =
        let json = String.Format("{{ \"id\": \"{0}\" }}", input.Id)   
        log.Info(sprintf "F# script processed queue message '%s'" json)
        json

Tämä voi myös useita tulosteen parametreilla määrittämällä yhden tulos, jossa `$return`.


## <a name="route-support"></a>Reititys-tuki

Oletusarvoisesti luodessasi HTTP käynnistimen tai WebHook-funktion funktio on käytettävissä reitin lomakkeen:

    http://<yourapp>.azurewebsites.net/api/<funcname> 

Voit mukauttaa tätä vaihtoehtoa, toimi käyttämällä valinnainen `route` HTTP-käynnistimen-ominaisuus on syötetty sidonta. Esimerkiksi seuraava *function.json* tiedosto määrittää `route` HTTP-käynnistimen ominaisuus:

    {
      "bindings": [
        {
          "type": "httpTrigger",
          "name": "req",
          "direction": "in",
          "methods": [ "get" ],
          "route": "products/{category:alpha}/{id:int?}"
        },
        {
          "type": "http",
          "name": "res",
          "direction": "out"
        }
      ]
    }

Käytä tätä määritystä, funktio on nyt käytettävissä seuraavassa reitin alkuperäisen reitin sijaan.

    http://<yourapp>.azurewebsites.net/api/products/electronics/357

Näin tukemaan kaksi parametria osoite-funktio-koodin `category` ja `id`. Voit käyttää minkä tahansa [Web API reitin rajoitetta](https://www.asp.net/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2#constraints) parametrit. Seuraava C# funktion koodi käyttää molempien parametrien.

    public static Task<HttpResponseMessage> Run(HttpRequestMessage request, string category, int? id, 
                                                    TraceWriter log)
    {
        if (id == null)
           return  req.CreateResponse(HttpStatusCode.OK, $"All {category} items were requested.");
        else
           return  req.CreateResponse(HttpStatusCode.OK, $"{category} item with id = {id} has been requested.");
    }

Seuraavassa on Node.js-funktion koodi, jota käytetään samat Reititysparametrit.

    module.exports = function (context, req) {

        var category = context.bindingData.category;
        var id = context.bindingData.id;
    
        if (!id) {
            context.res = {
                // status: 200, /* Defaults to 200 */
                body: "All " + category + " items were requested."
            };
        }
        else {
            context.res = {
                // status: 200, /* Defaults to 200 */
                body: category + " item with id = " + id + " was requested."
            };
        }
    
        context.done();
    } 

Oletusarvon mukaan kaikki funktion tiet etuliite *ohjelmointirajapinnan*kanssa. Voit myös mukauttaa tai poistaa etuliitettä käyttämällä `http.routePrefix` ominaisuuden *host.json* -tiedostossa. Seuraavassa esimerkissä poistaa *api* reitin etuliite tyhjä merkkijono käyttämällä etuliitteen *host.json* -tiedostossa.

    {
      "http": {
        "routePrefix": ""
      }
    }

Lisätietoja siitä, miten voit päivittää oman funktiosta on artikkelissa [funktio voi päivittää tiedostojen](functions-reference.md#fileupdate) *host.json* -tiedostoa. 

Funktio on nyt käytettävissä seuraavassa reitin lisäämällä nämä määritykset:

    http://<yourapp>.azurewebsites.net/products/electronics/357

Lisätietoja muita ominaisuuksia, voit määrittää *host.json* tiedostossa on artikkelissa [host.json viittaus](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json).





## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja on seuraavissa resursseissa:

* [Funktion testaaminen](functions-test-a-function.md)
* [Skaalaa-funktio](functions-scale.md)
