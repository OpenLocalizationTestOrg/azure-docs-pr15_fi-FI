<properties
    pageTitle="Azure Funktiot HTTP- ja webhook sidontojen | Microsoft Azure"
    description="Osaavat käyttää HTTP- ja webhook käynnistimien ja sidontojen Azure-funktiot."
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
    ms.date="08/22/2016"
    ms.author="chrande"/>

# <a name="azure-functions-http-and-webhook-bindings"></a>Azure Funktiot HTTP- ja webhook sidontojen

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Tässä artikkelissa kerrotaan, kuinka voit määrittää ja koodi HTTP- ja webhook käynnistimien ja sidontojen Azure-funktioissa. 

[AZURE.INCLUDE [intro](../../includes/functions-bindings-intro.md)] 

## <a name="functionjson-for-http-and-webhook-bindings"></a>HTTP- ja webhook sidontojen Function.JSON

*Function.json* -tiedosto sisältää ominaisuuksia, jotka koskevat pyynnön ja vastauksen.

HTTP-pyyntö ominaisuudet:

- `name`: Käyttää funktiota koodissa request-objekti (tai pyynnön leipätekstin kyseessä Node.js funktiot) muuttujan nimi.
- `type`: On määritettävä *httpTrigger*.
- `direction`: On määritettävä *sisään*. 
- `webHookType`: Kelvolliset arvot ovat WebHook käynnistimien, *github*, *liukuma*ja *genericJson*. Käynnistin, joka ei ole WebHook-arvoksi tämän ominaisuuden HTTP tyhjän merkkijonon. Lisätietoja WebHooks on kohdassa [WebHook käynnistimien](#webhook-triggers) .
- `authLevel`: WebHook käynnistimien ei koske. Aseta arvoksi "toimi" edellyttämään API-näppäintä, "anonyymi" avattavan Ohjelmointirajapinnan avaimen vaatimus tai "järjestelmänvalvojan" edellyttävät perusmuodon Ohjelmointirajapinnan avain. Lisätietoja on alla [API-näppäimiä](#apikeys) .

HTTP-vastauksen ominaisuudet:

- `name`: Käyttää funktiota koodissa vastauksen objekti muuttujan nimi.
- `type`: On määritettävä *http*.
- `direction`: On määritettävä *ulos*. 
 
Esimerkki *function.json*:

```json
{
  "bindings": [
    {
      "webHookType": "",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in",
      "authLevel": "function"
    },
    {
      "name": "res",
      "type": "http",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

## <a name="webhook-triggers"></a>WebHook Käynnistimet

WebHook käynnistin on HTTP-käynnistimen, joka on tarkoitettu WebHooks seuraavat ominaisuudet:

* Tietyn WebHook toimittajien (tällä hetkellä GitHub ja liukuma tuetaan)-Funktiot runtime tarkistaa tarjoajan allekirjoitus.
* Node.js Funktiot funktioiden runtime tarjoaa pyynnön sijaan pyyntö objektin tekstissä. Ei ole C#-funktioiden, käsitellä, koska voit määrittää, mikä on annettu määrittämällä parametrityyppi. Jos määrität `HttpRequestMessage` saat request-objekti. Jos POCO Määritä Funktiot runtime yrittää jäsentää JSON-objektin tekstissä pyynnön täytä objektin ominaisuudet.
* Käynnistettävän WebHook funktion HTTP-pyyntö on sisällettävä API-näppäintä. WebHook HTTP käynnistimien tämä vaatimus on valinnainen.

Lisätietoja GitHub WebHook määrittämisestä on artikkelissa [GitHub Developer - luominen WebHooks](http://go.microsoft.com/fwlink/?LinkID=761099&clcid=0x409).

## <a name="url-to-trigger-the-function"></a>Käynnistää toiminnon URL-osoite

Käynnistettävän funktion, Lähetä HTTP-pyyntö URL-Osoitetta, joka on yhdistelmä funktion sovelluksen URL-osoite ja Funktionimi:

```
 https://{function app name}.azurewebsites.net/api/{function name} 
```

## <a name="api-keys"></a>API näppäimet

Oletusarvon mukaan Ohjelmointirajapinnan avain on oltava HTTP-pyynnön käynnistettävän HTTP- tai WebHook-funktiota. Avain voidaan lisätä nimetty kysely merkkijonomuuttujaan `code`, tai se voidaan lisätä `x-functions-key` HTTP-otsikkoa. WebHook funktioiden voit ilmaista, että Ohjelmointirajapinnan avain ei vaadita määrittämällä `authLevel` ominaisuuden arvoksi "anonyymi" *function.json* -tiedostossa.

Löydät API avainarvot tiedostojärjestelmän funktion sovelluksen *D:\home\data\Functions\secrets* -kansiossa.  Avaimen ja Toimintonäppäin määritetään *host.json* -tiedostossa, kuten tässä esimerkissä. 

```json
{
  "masterKey": "K6P2VxK6P2VxK6P2VxmuefWzd4ljqeOOZWpgDdHW269P2hb7OSJbDg==",
  "functionKey": "OBmXvc2K6P2VxK6P2VxK6P2VxVvCdB89gChyHbzwTS/YYGWWndAbmA=="
}
```

Toimintonäppäin- *host.json* avulla voidaan käynnistää funktiota, mutta eivät Käynnistä ei käytössä-funktio. Perustyyli-avain voidaan käyttää funktiota, käynnistettävän ja käynnistävän funktiota, vaikka se on poistettu käytöstä. Voit määrittää funktion edellyttämään määrittämällä perustyyli-näppäintä `authLevel` "Järjestelmänvalvoja"-ominaisuutta. 

Jos *tietoja* kansio sisältää JSON tiedosto on sama nimi kuin funktion `key` tiedosto-ominaisuuden avulla myös voidaan käynnistää toiminnon ja avaimeen toimii vain se viittaa-funktiolla. Esimerkiksi Ohjelmointirajapinnan avain nimeltä funktion `HttpTrigger` on määritetty *HttpTrigger.json* *tietoja* -kansiossa. Tässä on esimerkki:

```json
{
  "key":"0t04nmo37hmoir2rwk16skyb9xsug32pdo75oce9r4kg9zfrn93wn4cx0sxo4af0kdcz69a4i"
}
```

> [AZURE.NOTE] Kun olet määrittämässä WebHook käynnistimen, Älä jaa perustyyli-avain WebHook-palvelussa. Käytä avainta, joka toimii vain funktiota, joka käsittelee WebHook.  Perustyyli avainta voi käyttää mitä tahansa funktiota käynnistettävän käytöstä myös funktioita.

## <a name="example-c-code-for-an-http-trigger-function"></a>HTTP-käynnistystoiminto C#-esimerkkikoodi 

Etsii esimerkkikoodi `name` parametrin kyselymerkkijonon tai HTTP-pyyntö tekstiosaan.

```csharp
using System.Net;
using System.Threading.Tasks;

public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
{
    log.Info($"C# HTTP trigger function processed a request. RequestUri={req.RequestUri}");

    // parse query parameter
    string name = req.GetQueryNameValuePairs()
        .FirstOrDefault(q => string.Compare(q.Key, "name", true) == 0)
        .Value;

    // Get request body
    dynamic data = await req.Content.ReadAsAsync<object>();

    // Set name to query string or body data
    name = name ?? data?.name;

    return name == null
        ? req.CreateResponse(HttpStatusCode.BadRequest, "Please pass a name on the query string or in the request body")
        : req.CreateResponse(HttpStatusCode.OK, "Hello " + name);
}
```

## <a name="example-f-code-for-an-http-trigger-function"></a>F # esimerkkikoodi HTTP-käynnistystoiminto

Etsii esimerkkikoodi `name` parametrin kyselymerkkijonon tai HTTP-pyyntö tekstiosaan.

```fsharp
open System.Net
open System.Net.Http
open FSharp.Interop.Dynamic

let Run(req: HttpRequestMessage) =
    async {
        let q =
            req.GetQueryNameValuePairs()
                |> Seq.tryFind (fun kv -> kv.Key = "name")
        match q with
        | Some kv ->
            return req.CreateResponse(HttpStatusCode.OK, "Hello " + kv.Value)
        | None ->
            let! data = Async.AwaitTask(req.Content.ReadAsAsync<obj>())
            try
                return req.CreateResponse(HttpStatusCode.OK, "Hello " + data?name)
            with e ->
                return req.CreateErrorResponse(HttpStatusCode.BadRequest, "Please pass a name on the query string or in the request body")
    } |> Async.StartAsTask
```

Tarvitset `project.json` tiedostoa, johon haluat viitata NuGet `FSharp.Interop.Dynamic` ja `Dynamitey` laitekokonaisuuksien tältä:

```json
{
  "frameworks": {
    "net46": {
      "dependencies": {
        "Dynamitey": "1.0.2",
        "FSharp.Interop.Dynamic": "3.0.0"
      }
    }
  }
}
```

Tämä käyttää NuGet hakeaksesi riippuvuudet ja viittaavat niiden komentosarja.

## <a name="example-nodejs-code-for-an-http-trigger-function"></a>Node.js esimerkkikoodi HTTP-käynnistystoiminto 

Tässä esimerkissä koodin etsitään `name` parametrin kyselymerkkijonon tai HTTP-pyyntö tekstiosaan.

```javascript
module.exports = function(context, req) {
    context.log('Node.js HTTP trigger function processed a request. RequestUri=%s', req.originalUrl);

    if (req.query.name || (req.body && req.body.name)) {
        context.res = {
            // status: 200, /* Defaults to 200 */
            body: "Hello " + (req.query.name || req.body.name)
        };
    }
    else {
        context.res = {
            status: 400,
            body: "Please pass a name on the query string or in the request body"
        };
    }
    context.done();
};
```

## <a name="example-c-code-for-a-github-webhook-function"></a>Esimerkki C#-koodin GitHub WebHook-funktio 

Tässä esimerkissä koodin kirjaa GitHub ongelman kommentit.

```csharp
#r "Newtonsoft.Json"

using System;
using System.Net;
using System.Threading.Tasks;
using Newtonsoft.Json;

public static async Task<object> Run(HttpRequestMessage req, TraceWriter log)
{
    string jsonContent = await req.Content.ReadAsStringAsync();
    dynamic data = JsonConvert.DeserializeObject(jsonContent);

    log.Info($"WebHook was triggered! Comment: {data.comment.body}");

    return req.CreateResponse(HttpStatusCode.OK, new {
        body = $"New GitHub comment: {data.comment.body}"
    });
}
```

## <a name="example-f-code-for-a-github-webhook-function"></a>Esimerkki F #-koodin GitHub WebHook-funktio

Tässä esimerkissä koodin kirjaa GitHub ongelman kommentit.

```fsharp
open System.Net
open System.Net.Http
open FSharp.Interop.Dynamic
open Newtonsoft.Json

type Response = {
    body: string
}

let Run(req: HttpRequestMessage, log: TraceWriter) =
    async {
        let! content = req.Content.ReadAsStringAsync() |> Async.AwaitTask
        let data = content |> JsonConvert.DeserializeObject
        log.Info(sprintf "GitHub WebHook triggered! %s" data?comment?body)
        return req.CreateResponse(
            HttpStatusCode.OK,
            { body = sprintf "New GitHub comment: %s" data?comment?body })
    } |> Async.StartAsTask
```

## <a name="example-nodejs-code-for-a-github-webhook-function"></a>Node.js esimerkkikoodi GitHub WebHook-funktio 

Tässä esimerkissä koodin kirjaa GitHub ongelman kommentit.

```javascript
module.exports = function (context, data) {
    context.log('GitHub WebHook triggered!', data.comment.body);
    context.res = { body: 'New GitHub comment: ' + data.comment.body };
    context.done();
};
```

## <a name="next-steps"></a>Seuraavat vaiheet

[AZURE.INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)] 
