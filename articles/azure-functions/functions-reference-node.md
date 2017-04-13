<properties
    pageTitle="Azure Funktiot NodeJS Sovelluskehittäjän opas | Microsoft Azure"
    description="Voit tehdä Azure-funktioiden käyttäminen NodeJS osaavat."
    services="functions"
    documentationCenter="na"
    authors="christopheranderson"
    manager="erikre"
    editor=""
    tags=""
    keywords="Azure Funktiot, funktioiden, käsittely, webhooks, dynaaminen suorittaminen, serverless arkkitehtuuri"/>

<tags
    ms.service="functions"
    ms.devlang="nodejs"
    ms.topic="reference"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="05/13/2016"
    ms.author="chrande"/>

# <a name="azure-functions-nodejs-developer-reference"></a>Azure Funktiot NodeJS Sovelluskehittäjän opas

> [AZURE.SELECTOR]
- [C#-komentosarja](../articles/azure-functions/functions-reference-csharp.md)
- [Komentosarjan F #](../articles/azure-functions/functions-reference-fsharp.md)
- [Node.js](../articles/azure-functions/functions-reference-node.md)

Azure-funktioiden solmu ja JavaScript-toiminto on helppo Vie funktiota, joka on välitetty `context` objektin yhteydessä suoritettavan ja kautta sidontojen tietojen lähettäminen ja vastaanottaminen.

Tässä artikkelissa oletetaan, että olet jo lukenut [Azure Funktiot Sovelluskehittäjän opas](functions-reference.md).

## <a name="exporting-a-function"></a>Funktion vieminen

Kaikki JavaScript toiminnot on vietävä yksittäinen `function` kautta `module.exports` Runtime Etsi funktio ja se toimii puhelimessasi. Tämä funktio on aina oltava `context` objekti.

```javascript
// You must include a context, but other arguments are optional
module.exports = function(context) {
    // Additional inputs can be accessed by the arguments property
    if(arguments.length === 4) {
        context.log('This function has 4 inputs');
    }
};
// or you can include additional inputs in your arguments
module.exports = function(context, myTrigger, myInput, myOtherInput) {
    // function logic goes here :)
};
```

Sidontojen, `direction === "in"` siirretään pitkin funktion argumentteina, eli voit käyttää [`arguments`](https://msdn.microsoft.com/library/87dw3w1k.aspx) käsittelemään dynaamisesti uusi syötteiden (esimerkiksi käyttämällä `arguments.length` käydä kaikkien syötteiden kautta). Tämä toiminto on erittäin käteviä, jos sinulla on vain käynnistimen kanssa syötteitä ei muita kuin voit käyttää käynnistimen tietojen laadukkaampi ilman viittaava oman `context` objekti.

Argumenttien on aina funktioon pitkin siinä järjestyksessä, ne sijaitsevat *function.json*, vaikka et määritä viennin-lauseessa. Jos sinulla on esimerkiksi `function(context, a, b)` ja muuta sen asetukseksi `function(context, a)`, voivat käyttää nykyistä arvoa `b` funktion koodissa viittaamalla `arguments[3]`.

Kaikki sidontojen riippumatta suunta on myös kulkenut pitkin `context` objektin (Katso alla oleva kuva). 

## <a name="context-object"></a>kontekstiobjekti

Suoritettavan käyttää `context` objektin välittää funktion tietoja ja mahdollistavat suorituksen yhteydessä.

Kontekstiobjekti on aina ensimmäinen parametri funktiolle ja aina sisällytetään, koska se on menetelmät kuten `context.done` ja `context.log` joka vaaditaan käyttöä oikein suorituksen. Voit nimetä objektin haluat luoda (eli `ctx` tai `c`).

```javascript
// You must include a context, but other arguments are optional
module.exports = function(context) {
    // function logic goes here :)
};
```

## <a name="contextbindings"></a>context.Bindings

`context.bindings` Objekti kerää kaikki syöttö- ja tiedot. Tiedot on lisätty sivulle `context.bindings` objektin kautta `name` sidonta-ominaisuus. Esimerkiksi alan seuraavan sidonta määritelmän *function.json*, voit käyttää kautta jonon sisältöä `context.bindings.myInput`. 

```json
    {
        "type":"queue",
        "direction":"in",
        "name":"myInput"
        ...
    }
```

```javascript
// myInput contains the input data which may have properties such as "name"
var author = context.bindings.myInput.name;
// Similarly, you can set your output data
context.bindings.myOutput = { 
        some_text: 'hello world', 
        a_number: 1 };
```

## `context.done([err],[propertyBag])`

`context.done` Funktio ilmoittaa suorituksen, että olet valmis käynnissä. Tämä on tärkeää kutsu, kun enää tarvitse funktion; Jos et, suorituksen tietävät edelleen koskaan funktion valmis. 

`context.done` Toiminnon avulla voit välittää takaisin käyttäjän määrittämän virheen suorituksen sekä ominaisuuden kantolaukku ominaisuudet, jotka korvaavat ominaisuudet käyttöön `context.bindings` objekti.

```javascript
// Even though we set myOutput to have:
//  -> text: hello world, number: 123
context.bindings.myOutput = { text: 'hello world', number: 123 };
// If we pass an object to the done function...
context.done(null, { myOutput: { text: 'hello there, world', noNumber: true }});
// the done method will overwrite the myOutput binding to be: 
//  -> text: hello there, world, noNumber: true
```

## <a name="contextlogmessage"></a>context.log(Message)

`context.log` Menetelmällä voit tulosteen log lausekkeita, jotka vastaavuussuhteet yhdessä kirjaaminen tarkoituksiin. Jos käytät `console.log`, viestit näkyvät vain prosessin tason kirjaaminen, joka ei ole yhtä hyödyllisiä.

```javascript
/* You can use context.log to log output specific to this 
function. You can access your bindings via context.bindings */
context.log({hello: 'world'}); // logs: { 'hello': 'world' } 
```

`context.log` Menetelmä tukee solmu [util.format menetelmä](https://nodejs.org/api/util.html#util_util_format_format) tukevassa parametrin muodossa. Näin on esimerkiksi koodi tältä:

```javascript
context.log('Node.js HTTP trigger function processed a request. RequestUri=' + req.originalUrl);
context.log('Request Headers = ' + JSON.stringify(req.headers));
```

Voit kirjoittaa tältä:

```javascript
context.log('Node.js HTTP trigger function processed a request. RequestUri=%s', req.originalUrl);
context.log('Request Headers = ', JSON.stringify(req.headers));
```

## <a name="http-triggers-contextreq-and-contextres"></a>HTTP-käynnistimet: context.req ja context.res

Kyseessä HTTP käynnistimien, koska se on esimerkiksi tavallisen kuvion käyttää `req` ja `res` on päättänyt HTTP pyynnön ja vastauksen objektien avulla on helppo käyttää niitä kontekstin objektin sijaan voit käyttää täydellinen pakottaminen `context.bindings.name` kuvio.

```javascript
// You can access your http request off of the context ...
if(context.req.body.emoji === ':pizza:') context.log('Yay!');
// and also set your http response
context.res = { status: 202, body: 'You successfully ordered more coffee!' };   
```

## <a name="node-version--package-management"></a>Solmu-versio ja paketin hallinta

Solmu-versio on lukittu `5.9.1`. Olemme olet tutkiminen lisääminen versioiden tuki ja tekeminen voidaan määrittää.

Voit sisällyttää pakettien funktion lataamalla *package.json* tiedoston oman funktion kansioon funktion app tiedostojärjestelmässä. Tiedoston lataaminen ohjeita on artikkelissa [Azure-Funktiot developer reference aiheen](functions-reference.md#fileupdate) **päivittämisestä funktioiden sovelluksen tiedostot** -osa. 

Voit myös käyttää `npm install` -funktion app palvelujen ohjauksen hallinta (Kudu) rivin komentoliittymä:

1. Siirry: `https://<function_app_name>.scm.azurewebsites.net`.

2. Valitse **Virheenkorjaus konsolin > CMD**.

3. Siirry `D:\home\site\wwwroot\<function_name>`.

4. Suorita `npm install`.

Kun tarvitset paketit on asennettu, voit tuoda funktion tavallista tavoilla (eli kautta `require('packagename')`)

```javascript
// Import the underscore.js library
var _ = require('underscore');
var version = process.version; // version === 'v5.9.1'

module.exports = function(context) {
    // Using our imported underscore.js library
    var matched_names = _
        .where(context.bindings.myInput.names, {first: 'Carla'});
```

## <a name="environment-variables"></a>Ympäristömuuttujat

Pääset ympäristömuuttuja tai sovelluksen arvo on käyttää `process.env`, seuraava koodi esimerkin mukaisesti:

```javascript
module.exports = function (context, myTimer) {
    var timeStamp = new Date().toISOString();
    
    context.log('Node.js timer trigger function ran!', timeStamp);   
    context.log(GetEnvironmentVariable("AzureWebJobsStorage"));
    context.log(GetEnvironmentVariable("WEBSITE_SITE_NAME"));
    
    context.done();
};

function GetEnvironmentVariable(name)
{
    return name + ": " + process.env[name];
}
```

## <a name="typescriptcoffeescript-support"></a>Kirjoituskoneella/CoffeeScript tuki

Ei ole vielä minkä tahansa tuotetuki automaattinen-käännetään kirjoituskoneella/CoffeeScript kautta Runtimen, niin, jotka haluat kaikki on käsiteltävä ulkopuolella runtime-käyttöönoton aikana. 

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja on seuraavissa resursseissa:

* [Azure Funktiot Sovelluskehittäjän opas](functions-reference.md)
* [Azure Funktiot C# Sovelluskehittäjän opas](functions-reference-csharp.md)
* [Azure Funktiot F # Sovelluskehittäjän opas](functions-reference-fsharp.md)
* [Azure Funktiot käynnistimien ja sidontojen](functions-triggers-bindings.md)
