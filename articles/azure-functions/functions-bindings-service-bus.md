<properties
    pageTitle="Azure Funktiot palvelun Bus käynnistimien ja sidontojen | Microsoft Azure"
    description="Tietoja Azure palvelun Bus käynnistimien ja sidontojen käyttämisestä Azure-funktiot."
    services="functions"
    documentationCenter="na"
    authors="christopheranderson"
    manager="erikre"
    editor=""
    tags=""
    keywords="Azure toimii, Funktiot, tapahtuman käsittelyn dynaaminen Laske serverless arkkitehtuuri"/>

<tags
    ms.service="functions"
    ms.devlang="multiple"
    ms.topic="reference"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="08/22/2016"
    ms.author="chrande; glenga"/>

# <a name="azure-functions-service-bus-triggers-and-bindings-for-queues-and-topics"></a>Azure Funktiot palvelun Bus käynnistimien ja sidontojen olevien ja aiheet

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Tässä artikkelissa kerrotaan määrittämisestä ja koodin Azure palvelun Bus käynnistimien ja sidontojen Azure-funktioissa. 

[AZURE.INCLUDE [intro](../../includes/functions-bindings-intro.md)] 

## <a id="sbtrigger"></a>Palvelun Bus jonossa tai aiheen käynnistin

#### <a name="functionjson"></a>Function.JSON

*Function.json* tiedosto määrittää seuraavat ominaisuudet.

- `name`Muuttujan nimi: käyttää funktiota koodissa jonossa tai jonossa tai aiheen viestin aihe. 
- `queueName`: N jonossa käynnistäminen vain lisääminen jonon nimi.
- `topicName`: N aiheen käynnistäminen vain lisääminen aiheen nimi.
- `subscriptionName`: N aiheen käynnistäminen vain tilauksen nimi.
- `connection`: Asetus, joka sisältää palvelun Bus yhteysmerkkijonon nimi. Yhteysmerkkijonon on oltava palvelun Bus nimitilan rajoituksetta tietyn jonon tai aihe. Jos yhteysmerkkijono ei hallita oikeudet, määrittää `accessRights` ominaisuus. Jos jätät `connection` tyhjä, käynnistintä tai sidonta toimivat funktio-sovellus, joka on määritetty AzureWebJobsServiceBus sovelluksen määrittämällä yhteysmerkkijonon palvelun Bus.
- `accessRights`Määrittää käytettävissä yhteysmerkkijonon käyttöoikeudet. Oletusarvo on `manage`. Jos haluat määrittää `listen` Jos käytät yhteysmerkkijonon, joka ei voi hallita käyttöoikeuksia. Käyttöoikeuksien hallinta muussa runtime kokeilemalla Funktiot ja virheiden suorittaa toimintoja, jotka edellyttävät.
- `type`: On määritettävä *serviceBusTrigger*.
- `direction`: On määritettävä *sisään*. 

Esimerkki *function.json* palvelun Bus jonon käynnistimen:

```json
{
  "bindings": [
    {
      "queueName": "testqueue",
      "connection": "MyServiceBusConnection",
      "name": "myQueueItem",
      "type": "serviceBusTrigger",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

#### <a name="c-code-example-that-processes-a-service-bus-queue-message"></a>C# Esimerkki koodista, joka käsittelee palvelun Bus jonon viestin

```csharp
public static void Run(string myQueueItem, TraceWriter log)
{
    log.Info($"C# ServiceBus queue trigger function processed message: {myQueueItem}");
}
```

#### <a name="f-code-example-that-processes-a-service-bus-queue-message"></a>F # Esimerkki koodista, joka käsittelee palvelun Bus jonon viestin

```fsharp
let Run(myQueueItem: string, log: TraceWriter) =
    log.Info(sprintf "F# ServiceBus queue trigger function processed message: %s" myQueueItem)
```

#### <a name="nodejs-code-example-that-processes-a-service-bus-queue-message"></a>Käsittelee palvelun Bus jonon viestin node.js koodi tässä esimerkissä

```javascript
module.exports = function(context, myQueueItem) {
    context.log('Node.js ServiceBus queue trigger function processed message', myQueueItem);
    context.done();
};
```

#### <a name="supported-types"></a>Tuetut tiedostotyypit

Palvelun Bus jonon viesti voi poistaa johonkin seuraavista:

* Objektin (JSON)
* merkkijono
* DBCS-matriisi 
* `BrokeredMessage`(C#) 

#### <a id="sbpeeklock"></a>PeekLock toiminta

Funktiot runtime vastaanottaa viestin `PeekLock` tila ja puhelut `Complete` tulee näyttöön, jos funktio on suoritettu onnistuneesti tai puhelut `Abandon` Jos funktio epäonnistuu. Jos funktio suorittaa yli `PeekLock` aikakatkaisu, Lukitse uusitaan automaattisesti.

#### <a id="sbpoison"></a>Torjuttujen sanomien käsittely

Palvelun Bus ei omassa torjuttujen sanomien käsitteleminen, joka ei voi hallita tai määritetty Azure Funktiot määritysten tai koodi. 

#### <a id="sbsinglethread"></a>Yhden popierių

Oletusarvoisesti Funktiot runtime käsittelee useita sanomien samanaikaisesti. Ohjaamaan käsittelemään kerrallaan vain yhteen jonossa tai aiheen viestin runtime määrittäminen `serviceBus.maxConcurrrentCalls` 1 *host.json* -tiedostossa. Lisätietoja *host.json* tiedoston Developer artikkelissa ja [host.json](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json) WebJobs.Script säilöön wikisivun-kohdassa [Kansiorakenne](functions-reference.md#folder-structure) .

## <a id="sboutput"></a>Palvelun Bus jonossa tai aiheen tulosteen sidonta

#### <a name="functionjson"></a>Function.JSON

*Function.json* tiedosto määrittää seuraavat ominaisuudet.

- `name`Muuttujan nimi: käyttää funktiota koodissa jonossa tai jonon viestin. 
- `queueName`: N jonossa käynnistäminen vain lisääminen jonon nimi.
- `topicName`: N aiheen käynnistäminen vain lisääminen aiheen nimi.
- `subscriptionName`: N aiheen käynnistäminen vain tilauksen nimi.
- `connection`: Sama kuin palvelun Bus käynnistäminen.
- `accessRights`Määrittää käytettävissä yhteysmerkkijonon käyttöoikeudet. Oletusarvo on `manage`. Jos haluat määrittää `send` Jos käytät yhteysmerkkijonon, joka ei voi hallita käyttöoikeuksia. Muussa runtime kokeilemalla Funktiot ja virheiden suorittaa toimintoja, jotka edellyttävät hallinta oikeudet, kuten olevien luominen.
- `type`: On määritettävä *serviceBus*.
- `direction`: On määritettävä *ulos*. 

Esimerkki *function.json* kirjoittaa palvelun Bus sanomien ajastin käynnistimen avulla:

```JSON
{
  "bindings": [
    {
      "schedule": "0/15 * * * * *",
      "name": "myTimer",
      "runsOnStartup": true,
      "type": "timerTrigger",
      "direction": "in"
    },
    {
      "name": "outputSbQueue",
      "type": "serviceBus",
      "queueName": "testqueue",
      "connection": "MyServiceBusConnection",
      "direction": "out"
    }
  ],
  "disabled": false
}
``` 

#### <a name="supported-types"></a>Tuetut tiedostotyypit

Azure funktioita voit luoda jokin seuraavista raporttityypeistä palvelun Bus jonon viestin.

* Objektin (aina Luo JSON viestin, Luo viesti null-objektia, jos arvo on null, kun funktio päättyy)
* merkkijono (Luo viesti, jos arvo on muut kuin null, kun funktio päättyy)
* DBCS-matriisin (toiminta merkkijono) 
* `BrokeredMessage`(C#-toiminta merkkijono)

Luonut useita viestejä C#-funktion, voit käyttää `ICollector<T>` tai `IAsyncCollector<T>`. Viestin luodaan, kun soitat `Add` menetelmää.

#### <a name="c-code-examples-that-create-service-bus-queue-messages"></a>C# koodiesimerkkejä, jotka luovat palvelun Bus sanomien

```csharp
public static void Run(TimerInfo myTimer, TraceWriter log, out string outputSbQueue)
{
    string message = $"Service Bus queue message created at: {DateTime.Now}";
    log.Info(message); 
    outputSbQueue = message;
}
```

```csharp
public static void Run(TimerInfo myTimer, TraceWriter log, ICollector<string> outputSbQueue)
{
    string message = $"Service Bus queue message created at: {DateTime.Now}";
    log.Info(message); 
    outputSbQueue.Add("1 " + message);
    outputSbQueue.Add("2 " + message);
}
```

#### <a name="f-code-example-that-creates-a-service-bus-queue-message"></a>F # Esimerkki koodista, joka luo palvelun Bus jonon viestin

```fsharp
let Run(myTimer: TimerInfo, log: TraceWriter, outputSbQueue: byref<string>) =
    let message = sprintf "Service Bus queue message created at: %s" (DateTime.Now.ToString())
    log.Info(message)
    outputSbQueue = message
```

#### <a name="nodejs-code-example-that-creates-a-service-bus-queue-message"></a>Luo viestin palvelun Bus jonon node.js koodi tässä esimerkissä

```javascript
module.exports = function (context, myTimer) {
    var timeStamp = new Date().toISOString();
    
    if(myTimer.isPastDue)
    {
        context.log('Node.js is running late!');
    }
    var message = 'Service Bus queue message created at ' + timeStamp;
    context.log(message);   
    context.bindings.outputSbQueueMsg = message;
    context.done();
};
```

## <a name="next-steps"></a>Seuraavat vaiheet

[AZURE.INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)] 
