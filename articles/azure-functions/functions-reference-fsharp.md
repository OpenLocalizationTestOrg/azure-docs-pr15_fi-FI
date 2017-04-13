<properties
    pageTitle="Azure Funktiot F # Sovelluskehittäjän opas | Microsoft Azure"
    description="Osaavat kehittää Azure-funktioiden käyttäminen F #."
    services="functions"
    documentationCenter="fsharp"
    authors="sylvanc"
    manager="jbronsk"
    editor=""
    tags=""
    keywords="Azure Funktiot, funktioiden, tapahtuman käsittelyn webhooks, dynaaminen suorittaminen, serverless arkkitehtuuri, F#"/>

<tags
    ms.service="functions"
    ms.devlang="fsharp"
    ms.topic="reference"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="09/09/2016"
    ms.author="syclebsc"/>

# <a name="azure-functions-f-developer-reference"></a>Azure Funktiot F # Sovelluskehittäjän opas

> [AZURE.SELECTOR]
- [C#-komentosarja](../articles/azure-functions/functions-reference-csharp.md)
- [Komentosarjan F #](../articles/azure-functions/functions-reference-fsharp.md)
- [Node.js](../articles/azure-functions/functions-reference-node.md)

F # Azure-funktioiden on käynnissä helposti pilveen pystysuuntaisena koodi tai-funktioiden,"ratkaisu. Tietoja jatkuu F # funktion argumenttien kautta. Argumenttien nimet on määritetty `function.json`, ovat valmiiksi määritetyillä nimillä kohteita, kuten funktion lokin ja peruutus tunnusten käyttäminen.

Tässä artikkelissa oletetaan, että olet jo lukenut [Azure Funktiot Sovelluskehittäjän opas](functions-reference.md).

## <a name="how-fsx-works"></a>.Fsx toiminta

`.fsx` Tiedosto on F #-komentosarjan. Se voidaan ajatella F # projektina, jotka sisältyvät yhteen tiedostoon. Tiedostossa on sekä koodi ohjelman (Tässä tapauksessa Azure-funktion) ja direktiivejä riippuvuudet hallintaan.

Kun käytät `.fsx` Azure-funktion yleisesti pakollinen kokoonpanot sisällytetään automaattisesti, joten voit keskittyä funktion sijaan "asiakirjan osien uudelleen käyttäminen"-koodi.

## <a name="binding-to-arguments"></a>Sidonta argumentit

Kunkin sidonta tukee joitakin määrittäminen argumenttien [Azure Funktiot käynnistimien ja sidontojen Sovelluskehittäjän opas](functions-triggers-bindings.md)esitetyllä tavalla. Esimerkiksi jokin blob käynnistimen tukee argumentin sidontojen on POCO, joka voidaan ilmaista F #-tietue. Esimerkki:

```fsharp
type Item = { Id: string }

let Run(blob: string, output: byref<Item>) =
    let item = { Id = "Some ID" }
    output <- item
```

F # Azure funktion tulevat yhtä tai useampaa argumenttia. Kun käsittelemme Azure Funktiot argumentit on lisätietoja _syötteen_ argumentit ja _tulosteen_ argumentit. Syötteen argumentti on täsmälleen se kuulostaa samoin kuin: Kirjoita F # Azure funktion. _Tulostus_ -argumentti on mutable tietojen tai `byref<>` argumentin, joka on tapa välittää tiedot takaisin _,_ että funktion.

Yllä olevassa esimerkissä `blob` on syötteen argumenttina ja `output` on tulostus-argumentti. Huomaa, että on käytetty `byref<>` varten `output` (ei tarvita, jos haluat lisätä `[<Out>]` Huomautus). Käyttämällä `byref<>` tyyppi avulla voit muuttaa mitä tietueen tai objektin argumentti viittaa funktion.

Kun F #-tietuetta käytetään syötteen tyyppi-tietueen määritys on merkittävä `[<CLIMutable>]` , jotta voit määrittää kentät asianmukaisesti ennen lähettämistä tietueen funktion Azure-Funktiot-framework. Näytä lisäasetukset `[<CLIMutable>]` luo tietueen ominaisuudet-valintaikkunassa hankintamenoperiaatteesta. Esimerkki:

```fsharp
[<CLIMutable>]
type TestObject =
    { SenderName : string
      Greeting : string }

let Run(req: TestObject, log: TraceWriter) =
    { req with Greeting = sprintf "Hello, %s" req.SenderName }
```

F #-luokan voidaan myös sekä sisään ja ulos argumentit. Luokka-ominaisuuksia on yleensä, getters ja hankintamenoperiaatteesta. Esimerkki:

```fsharp
type Item() =
    member val Id = "" with get,set
    member val Text = "" with get,set

let Run(input: string, item: byref<Item>) =
    let result = Item(Id = input, Text = "Hello from F#!")
    item <- result
```

## <a name="logging"></a>Lokiin kirjaaminen

Kirjaudu tulosteen oman [streaming lokit](../app-service-web/web-sites-streaming-logs-and-console.md) F #-funktion otettava argumentin tyyppi `TraceWriter`. Yhdenmukaisuuden, suosittelemme argumentti on nimeltään `log`. Esimerkki:

```fsharp
let Run(blob: string, output: byref<string>, log: TraceWriter) =
    log.Verbose(sprintf "F# Azure Function processed a blob: %s" blob)
    output <- input
```

## <a name="async"></a>Asynkroninen

`async` Työnkulkua voi käyttää, mutta tuloksena on palautettava `Task`. Tämän voi tehdä kanssa `Async.StartAsTask`, esimerkiksi:

```fsharp
let Run(req: HttpRequestMessage) =
    async {
        return new HttpResponseMessage(HttpStatusCode.OK)
    } |> Async.StartAsTask
```

## <a name="cancellation-token"></a>Peruutus-tunnus

Jos haluat käsitellä Sammuta tilanteen funktion, voit tarkastella sitä [`CancellationToken`](https://msdn.microsoft.com/library/system.threading.cancellationtoken.aspx) argumentti. Tämä voi yhdistää `async`, esimerkiksi:

```fsharp
let Run(req: HttpRequestMessage, token: CancellationToken)
    let f = async {
        do! Async.Sleep(10)
        return new HttpResponseMessage(HttpStatusCode.OK)
    }
    Async.StartAsTask(f, token)
```

## <a name="importing-namespaces"></a>Nimitilan tuominen

Nimitilan voi avata tavalliseen tapaan:

```fsharp
open System.Net
open System.Threading.Tasks

let Run(req: HttpRequestMessage, log: TraceWriter) =
    ...
```

Seuraavat nimitilan avataan automaattisesti:

* `System`
* `System.Collections.Generic`
* `System.IO`
* `System.Linq`
* `System.Net.Http`
* `System.Threading.Tasks`
* `Microsoft.Azure.WebJobs`
* `Microsoft.Azure.WebJobs.Host`.

## <a name="referencing-external-assemblies"></a>Ulkoinen kokoonpanot viittaaminen

Vastaavasti framework kokoonpanon viittaukset on lisätty `#r "AssemblyName"` direktiivin.

```fsharp
#r "System.Web.Http"

open System.Net
open System.Net.Http
open System.Threading.Tasks

let Run(req: HttpRequestMessage, log: TraceWriter) =
    ...
```

Seuraavat kokoonpanon lisätään automaattisesti Isännöintiympäristö Azure-funktioita:

* `mscorlib`,
* `System`
* `System.Core`
* `System.Xml`
* `System.Net.Http`
* `Microsoft.Azure.WebJobs`
* `Microsoft.Azure.WebJobs.Host`
* `Microsoft.Azure.WebJobs.Extensions`
* `System.Web.Http`
* `System.Net.Http.Formatting`.

Lisäksi seuraavat kokoonpanon ovat erityisiä toimivaltaisena ja voi viitata simplename (kuten `#r "AssemblyName"`):

* `Newtonsoft.Json`
* `Microsoft.WindowsAzure.Storage`
* `Microsoft.ServiceBus`
* `Microsoft.AspNet.WebHooks.Receivers`
* `Microsoft.AspNEt.WebHooks.Common`.

Jos haluat viitata yksityinen kokoonpano, voit ladata kokoonpanotiedosto kyselyjä `bin` suhteessa oman-funktio ja viittaus avulla tiedoston nimi (esimerkiksi kansioon  `#r "MyAssembly.dll"`). Tiedostojen lataaminen funktion kansioon, Lisätietoja on seuraavassa osassa paketin hallintaa.

## <a name="editor-prelude"></a>Editorin Prelude

Editorin, joka tukee F # kääntäjän palvelut eivät ole huomioon nimitilan tai kokoonpanoista, jotka Azure Funktiot sisältää automaattisesti. Näin ollen voi olla hyödyllistä prelude, jonka avulla löydät, että käytät kokoonpanon editorin ja avaa erikseen nimitilan. Esimerkki:

```fsharp
#if !COMPILED
#I "../../bin/Binaries/WebJobs.Script.Host"
#r "Microsoft.Azure.WebJobs.Host.dll"
#endif

open Sytem
open Microsoft.Azure.WebJobs.Host

let Run(blob: string, output: byref<string>, log: TraceWriter) =
    ...
```

Kun Azure-funktio suorittaa koodin, käsittelee lähteen kanssa `COMPILED` määritetty, joten editorin prelude ohitetaan.

## <a name="package-management"></a>Paketin hallinta

Käyttämään NuGet pakettien F #-funktion lisääminen `project.json` tiedosto toiminnon kansion funktion app tiedostojärjestelmässä. Esimerkki `project.json` tiedosto, joka lisää NuGet paketin viittaa `Microsoft.ProjectOxford.Face` versio 1.1.0:

```json
{
  "frameworks": {
    "net46":{
      "dependencies": {
        "Microsoft.ProjectOxford.Face": "1.1.0"
      }
    }
   }
}
```

.NET Framework 4.6 tuetaan, joten varmista, että `project.json` tiedosto määrittää `net46` tässä esitetyllä tavalla.

Kun lataat `project.json` tiedostoa, suorituksen saa paketit ja lisää automaattisesti paketin kokoonpanon viittauksia. Sinun ei tarvitse lisätä `#r "AssemblyName"` direktiivejä. Voit lisätä pakollinen `open` lauseet oman `.fsx` tiedosto.

Halutessasi voit sijoittaa automaattisesti viittaukset kokoonpanot editori-prelude oman editorin käsittelemisen F #: n Käännä palvelujen parantamiseen.

### <a name="how-to-add-a-projectjson-file-to-your-azure-function"></a>Voit lisätä `project.json` tiedoston Azure-funktio

1. Aloita varmistamalla, että funktion sovellus on käynnissä, voit tehdä sen avaamalla funktion Azure-portaalissa. Tämä myös antaa käyttöoikeuden streaming lokit jossa paketin asennuksen tulosteen näkyvät.

2. Lataaminen `project.json` tiedoston, käytä jotakin [päivittämisestä funktioiden sovelluksen tiedostot](functions-reference.md#fileupdate)kuvatuilla tavoilla. Jos käytössäsi on [Jatkuva käyttöönoton Azure-funktioiden](functions-continuous-deployment.md), voit lisätä `project.json` oman väliaikaisen haaran, jotta voit kokeilla sitä ennen sen lisäämistä käyttöönotto-haara-tiedosto.

3. Sen jälkeen `project.json` tiedosto lisätään, näet samalla tavalla kuin seuraavassa esimerkissä-funktion tulos on streaming loki:

```
2016-04-04T19:02:48.745 Restoring packages.
2016-04-04T19:02:48.745 Starting NuGet restore
2016-04-04T19:02:50.183 MSBuild auto-detection: using msbuild version '14.0' from 'D:\Program Files (x86)\MSBuild\14.0\bin'.
2016-04-04T19:02:50.261 Feeds used:
2016-04-04T19:02:50.261 C:\DWASFiles\Sites\facavalfunctest\LocalAppData\NuGet\Cache
2016-04-04T19:02:50.261 https://api.nuget.org/v3/index.json
2016-04-04T19:02:50.261
2016-04-04T19:02:50.511 Restoring packages for D:\home\site\wwwroot\HttpTriggerCSharp1\Project.json...
2016-04-04T19:02:52.800 Installing Newtonsoft.Json 6.0.8.
2016-04-04T19:02:52.800 Installing Microsoft.ProjectOxford.Face 1.1.0.
2016-04-04T19:02:57.095 All packages are compatible with .NETFramework,Version=v4.6.
2016-04-04T19:02:57.189
2016-04-04T19:02:57.189
2016-04-04T19:02:57.455 Packages restored.
```

## <a name="environment-variables"></a>Ympäristömuuttujat

Pääset ympäristömuuttuja tai sovelluksen arvo on käyttää `System.Environment.GetEnvironmentVariable`, esimerkiksi:

```fsharp
open System.Environment

let Run(timer: TimerInfo, log: TraceWriter) =
    log.Info("Storage = " + GetEnvironmentVariable("AzureWebJobsStorage"))
    log.Info("Site = " + GetEnvironmentVariable("WEBSITE_SITE_NAME"))
```

## <a name="reusing-fsx-code"></a>Uudelleen .fsx koodi

Voit käyttää koodia toisistaan `.fsx` tiedostojen käyttäminen `#load` direktiivin. Esimerkki:

`run.fsx`

```fsharp
#load "logger.fsx"

let Run(timer: TimerInfo, log: TraceWriter) =
    mylog log (sprintf "Timer: %s" DateTime.Now.ToString())
```

`logger.fsx`

```fsharp
let mylog(log: TraceWriter, text: string) =
    log.Verbose(text);
```

Polut tarjoaa `#load` direktiivi ovat sijainnin suhteessa oman `.fsx` tiedosto.

* `#load "logger.fsx"`Lataa tiedoston funktio-kansiossa.

* `#load "package\logger.fsx"`Lataa tiedosto sijaitsee `package` funktio-kansiossa.

* `#load "..\shared\mylogger.fsx"`Lataa tiedosto sijaitsee `shared` samalla tasolla funktion kansiona, eli kansion alapuolella `wwwroot`.

`#load` Direktiivin toimii vain `.fsx` (F # komentosarja)-tiedostoja, eikä `.fs` tiedostot.

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja on seuraavissa resursseissa:

* [F # opas](https://docs.microsoft.com/en-us/dotnet/articles/fsharp/index)
* [Azure Funktiot Sovelluskehittäjän opas](functions-reference.md)
* [Azure Funktiot C# Sovelluskehittäjän opas](functions-reference-csharp.md)
* [Azure Funktiot NodeJS Sovelluskehittäjän opas](functions-reference-node.md)
* [Azure Funktiot käynnistimien ja sidontojen](functions-triggers-bindings.md)
* [Azure toimii testaaminen](functions-test-a-function.md)
* [Azure toimii skaalaus](functions-scale.md)
