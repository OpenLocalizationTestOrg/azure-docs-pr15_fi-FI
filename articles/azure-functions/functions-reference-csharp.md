<properties
    pageTitle="Azure Funktiot Sovelluskehittäjän opas | Microsoft Azure"
    description="Osaavat kehittää Azure-funktioiden käyttäminen C#."
    services="functions"
    documentationCenter="na"
    authors="christopheranderson"
    manager="erikre"
    editor=""
    tags=""
    keywords="Azure Funktiot, funktioiden, käsittely, webhooks, dynaaminen suorittaminen, serverless arkkitehtuuri"/>

<tags
    ms.service="functions"
    ms.devlang="dotnet"
    ms.topic="reference"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="05/13/2016"
    ms.author="chrande"/>

# <a name="azure-functions-c-developer-reference"></a>Azure Funktiot C# Sovelluskehittäjän opas

> [AZURE.SELECTOR]
- [C#-komentosarja](../articles/azure-functions/functions-reference-csharp.md)
- [Komentosarjan F #](../articles/azure-functions/functions-reference-fsharp.md)
- [Node.js](../articles/azure-functions/functions-reference-node.md)
 
Azure Funktiot C#-toiminta perustuu Azure WebJobs SDK-paketissa. Tietoja jatkuu funktion C# menetelmä argumentit kautta. Argumenttien nimet on määritetty `function.json`, ovat valmiiksi määritetyillä nimillä kohteita, kuten funktion lokin ja peruutus tunnusten käyttäminen.

Tässä artikkelissa oletetaan, että olet jo lukenut [Azure Funktiot Sovelluskehittäjän opas](functions-reference.md).

## <a name="how-csx-works"></a>.Csx toiminta

`.csx` Muodossa tallennettuja kirjoittaa vähemmän "asiakirjan osien uudelleen käyttäminen" ja keskittyä vain C# funktiota kirjoitettaessa. Azure-Funktiot, voit vain sisältyy minkä tahansa kokoonpanon viittauksia ja nimitilan on määrittäminen yläreunassa tavalliseen tapaan ja sen sijaan, että rivitys kaikki nimitilan ja luokan, voit vain määrittää oman `Run` menetelmää. Jos haluat lisätä kaikki luokat, esimerkiksi määrittää POCO objekteja, voit lisätä luokan sisällä samaa tiedostoa.

## <a name="binding-to-arguments"></a>Sidonta argumentit

Eri sidontojen on sidottu C#-funktion kautta `name` *function.json* määritys-ominaisuutta. Kunkin sidonta on oma tuetut tyypit, jotka on kuvattu sidonta; kohden esimerkiksi blob käynnistimen tukee merkkijonon, POCO tai useita muita. Voit käyttää tyyppi, joka sopii tarvittaessa. 

```csharp
public static void Run(string myBlob, out MyClass myQueueItem)
{
    log.Verbose($"C# Blob trigger function processed: {myBlob}");
    myQueueItem = new MyClass() { Id = "myid" };
}

public class MyClass
{
    public string Id { get; set; }
}
```

## <a name="logging"></a>Lokiin kirjaaminen

Voit kirjautua tulosteen streaming lokeista C#, `TraceWriter` kirjoitettu argumentti. On suositeltavaa, että voit nimetä sen `log`. Suosittelemme, että voit välttää `Console.Write` Azure-funktioissa.

```csharp
public static void Run(string myBlob, TraceWriter log)
{
    log.Info($"C# Blob trigger function processed: {myBlob}");
}
```

## <a name="async"></a>Asynkroninen

Voit tehdä toiminnon asynkroninen `async` avainsana ja palaa `Task` objekti.

```csharp
public async static Task ProcessQueueMessageAsync(
        string blobName, 
        Stream blobInput,
        Stream blobOutput)
    {
        await blobInput.CopyToAsync(blobOutput, 4096, token);
    }
```

## <a name="cancellation-token"></a>Peruutus-tunnus

Joissakin tapauksissa saatat joutua toimintoja, jotka ovat luottamuksellisia suljetaan. Kannattaa aina kirjoittaa koodia, jossa voit käsitellä kaatuu, kun tapauksissa haluamaasi käsittelemään kaksivaiheista Sammuta pyytää, voit määrittää [`CancellationToken`](https://msdn.microsoft.com/library/system.threading.cancellationtoken.aspx) kirjoitettu argumentti.  A `CancellationToken` toimitetaan host Sammuta käynnistyy. 

```csharp
public async static Task ProcessQueueMessageAsyncCancellationToken(
        string blobName, 
        Stream blobInput,
        Stream blobOutput,
        CancellationToken token)
    {
        await blobInput.CopyToAsync(blobOutput, 4096, token);
    }
```

## <a name="importing-namespaces"></a>Nimitilan tuominen

Jos haluat tuoda nimitilan, voit tehdä niin kuin tavanomainen, ja `using` lause.

```csharp
using System.Net;
using System.Threading.Tasks;

public static Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
```

Seuraavat nimitilan tuodaan automaattisesti, ja ne ovat valinnaisia:

* `System`
* `System.Collections.Generic`
* `System.IO`
* `System.Linq`
* `System.Net.Http`
* `System.Threading.Tasks`
* `Microsoft.Azure.WebJobs`
* `Microsoft.Azure.WebJobs.Host`.

## <a name="referencing-external-assemblies"></a>Ulkoinen kokoonpanot viittaaminen

Framework kokoonpanoille lisääminen viittaukset avulla `#r "AssemblyName"` direktiivin.

```csharp
#r "System.Web.Http"

using System.Net;
using System.Net.Http;
using System.Threading.Tasks;

public static Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
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
* `Microsoft.AspNEt.WebHooks.Common`
* `Microsoft.Azure.NotificationHubs`

Jos haluat viitata yksityinen kokoonpano, voit ladata kokoonpanotiedosto kyselyjä `bin` suhteessa oman-funktio ja viittaus avulla tiedoston nimi (esimerkiksi kansioon  `#r "MyAssembly.dll"`). Tiedostojen lataaminen funktion kansioon, Lisätietoja on seuraavassa osassa paketin hallintaa.

## <a name="package-management"></a>Paketin hallinta

Käyttämään NuGet pakettien C#-funktion avulla *project.json* tiedoston lataaminen toiminnon kansion funktion app tiedostojärjestelmässä. Tässä on esimerkki *project.json* tiedostosta, joka lisää Microsoft.ProjectOxford.Face versio 1.1.0 viittaus:

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

.NET Framework 4.6 tuetaan, joten varmista *project.json* tiedoston määrittää `net46` tässä esitetyllä tavalla.

Kun lataat *project.json* tiedoston, suorituksen saa paketit ja lisää automaattisesti paketin kokoonpanon viittauksia. Sinun ei tarvitse lisätä `#r "AssemblyName"` direktiivejä. Voit lisätä pakollinen `using` *run.csx* tiedoston käyttämään NuGet-pakettien määritelty tyypit-lauseet.


### <a name="how-to-upload-a-projectjson-file"></a>Project.json tiedoston lataaminen

1. Aloita varmistamalla, että funktion sovellus on käynnissä, voit tehdä sen avaamalla funktion Azure-portaalissa. 

    Tämä myös antaa käyttöoikeuden streaming lokit jossa paketin asennuksen tulosteen näkyvät. 

2. Lataa project.json tiedosto, käytä [Azure-Funktiot developer reference aiheen](functions-reference.md#fileupdate) **päivittämisestä funktioiden sovelluksen tiedostot** -kohdan kuvatuilla tavoilla. 

3. Kun *project.json* -tiedosto on ladattu, näet seuraavan esimerkin-funktion tulos on streaming loki:

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

Pääset ympäristömuuttuja tai sovelluksen arvo on käyttää `System.Environment.GetEnvironmentVariable`, seuraava koodi esimerkin mukaisesti:

```csharp
public static void Run(TimerInfo myTimer, TraceWriter log)
{
    log.Info($"C# Timer trigger function executed at: {DateTime.Now}");
    log.Info(GetEnvironmentVariable("AzureWebJobsStorage"));
    log.Info(GetEnvironmentVariable("WEBSITE_SITE_NAME"));
}

public static string GetEnvironmentVariable(string name)
{
    return name + ": " + 
        System.Environment.GetEnvironmentVariable(name, EnvironmentVariableTarget.Process);
}
```

## <a name="reusing-csx-code"></a>Uudelleen .csx koodi

Voit käyttää luokat ja menetelmät *run.csx* tiedoston muiden *.csx* -tiedostoissa. Saadakseen ne käyttää `#load` direktiivejä *run.csx* -tiedostossa, kuten seuraavassa esimerkissä.

Esimerkki *run.csx*:

```csharp
#load "mylogger.csx"

public static void Run(TimerInfo myTimer, TraceWriter log)
{
    log.Verbose($"Log by run.csx: {DateTime.Now}"); 
    MyLogger(log, $"Log by MyLogger: {DateTime.Now}");
}
```

Esimerkki *mylogger.csx*:

```csharp
public static void MyLogger(TraceWriter log, string logtext)
{
    log.Verbose(logtext); 
}
```

Voit käyttää suhteellinen polku, jossa `#load` direktiivin:

* `#load "mylogger.csx"`Lataa tiedoston funktio-kansiossa.

* `#load "loadedfiles\mylogger.csx"`Lataa-funktion kansion kansiossa sijaitsevaan tiedostoon.

* `#load "..\shared\mylogger.csx"`Lataa alapuolella *wwwroot*kansioon samalla tasolla funktion kansiona, eli tiedoston.
 
`#load` Direktiivi toimii vain *.csx* (C# komentosarja)-tiedostojen kanssa, ei *.cs* tiedostot. 

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja on seuraavissa resursseissa:

* [Azure Funktiot Sovelluskehittäjän opas](functions-reference.md)
* [Azure Funktiot F # Sovelluskehittäjän opas](functions-reference-fsharp.md)
* [Azure Funktiot NodeJS Sovelluskehittäjän opas](functions-reference-node.md)
* [Azure Funktiot käynnistimien ja sidontojen](functions-triggers-bindings.md)

