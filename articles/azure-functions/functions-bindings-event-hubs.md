<properties
    pageTitle="Azure Funktiot tapahtumaa-toiminnossa sidontojen | Microsoft Azure"
    description="Osaavat käyttää Azure tapahtumaa-toiminnossa sidontojen Azure-funktiot."
    services="functions"
    documentationCenter="na"
    authors="wesmc7777"
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
    ms.date="10/17/2016"
    ms.author="wesmc"/>

# <a name="azure-functions-event-hub-bindings"></a>Azure Funktiot tapahtumaa-toiminnossa sidontojen

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Tässä artikkelissa kerrotaan määrittämisestä ja koodin [Azure tapahtumaa-toiminnossa](../event-hubs/event-hubs-overview.md) sidontojen Azure-funktioiden. Azure funktioiden tuki käynnistin ja tulosteen sidontojen Azure tapahtuman keskittimet.

[AZURE.INCLUDE [intro](../../includes/functions-bindings-intro.md)] 

Jos olet uusi Azure tapahtuman porttiin, artikkelissa [Azure tapahtumaa-toiminnossa yleiskatsaus](../event-hubs/event-hubs-overview.md).

## <a name="azure-event-hub-trigger-binding"></a>Azure tapahtumaa-toiminnossa sidonta käynnistäminen

Azure tapahtumaa-toiminnossa käynnistimen avulla voidaan lähettää tapahtuman keskittimeen tapahtuma-tietovirta tapahtumaan vastaaminen. Sinulla on lukuoikeudet määrittämään käynnistimen sidonta tapahtumaa-toiminnossa.

#### <a name="functionjson-for-event-hub-trigger-binding"></a>Tapahtuma-toiminnossa Function.JSON käynnistäminen sidonta

Azure tapahtumaa-toiminnossa käynnistimen *function.json* -tiedosto määrittää seuraavat ominaisuudet:

- `type`: On määritettävä *eventHubTrigger*.
- `name`Muuttujan nimi: käyttää funktiota koodissa tapahtuma-toiminnossa viestin. 
- `direction`: On määritettävä *sisään*. 
- `path`: Tapahtumaa-toiminnossa nimi.
- `consumerGroup`: Tämä on valinnainen ominaisuus, joka on käytettävä [kuluttaja ryhmän](../event-hubs-overview.md#consumer-groups) , jota käytetään tilata tapahtumien toiminnossa. Jos jätetään pois, `$Default` consumer käytetään. 
- `connection`: Asetus, joka sisältää nimitilan, joka sijaitsee tapahtumaa-toiminnossa voit yhteysmerkkijonon nimi. Kopioi tämä yhteysmerkkijonon napsauttamalla nimitilan, ei itse tapahtumaa-toiminnossa **Yhteystiedot** -painiketta.  Tämän yhteysmerkkijonon täytyy olla vähintään lukuoikeudet aktivoimaan käynnistintä.

        {
          "bindings": [
            {
              "type": "eventHubTrigger",
              "name": "myEventHubMessage",
              "direction": "in",
              "path": "MyEventHub",
              "connection": "myEventHubReadConnectionString"
            }
          ],
          "disabled": false
        }

#### <a name="azure-event-hub-trigger-c-example"></a>Azure tapahtumaa-toiminnossa käynnistimen C#-Esimerkki
 
Käytä edellä olevassa esimerkissä function.json, tapahtuman viestin tekstiosaan kirjataan käyttämällä annettua C# funktion alla:
 
    using System;
    
    public static void Run(string myEventHubMessage, TraceWriter log)
    {
        log.Info($"C# Event Hub trigger function processed a message: {myEventHubMessage}");
    }

#### <a name="azure-event-hub-trigger-f-example"></a>Azure tapahtumaa-toiminnossa käynnistimen F # Esimerkki

Käytä edellä olevassa esimerkissä function.json, tapahtuman viestin tekstiosaan kirjataan käyttämällä annettua F # funktion alla:

    let Run(myEventHubMessage: string, log: TraceWriter) =
        log.Info(sprintf "F# eventhub trigger function processed work item: %s" myEventHubMessage)

#### <a name="azure-event-hub-trigger-nodejs-example"></a>Azure tapahtumaa-toiminnossa käynnistimen Node.js Esimerkki
 
Käytä edellä olevassa esimerkissä function.json, tapahtuman viestin tekstiosaan kirjataan alla Node.js funktio-koodin avulla:
 
    module.exports = function (context, myEventHubMessage) {
        context.log('Node.js eventhub trigger function processed work item', myEventHubMessage);    
        context.done();
    };


## <a name="azure-event-hub-output-binding"></a>Azure tapahtumaa-toiminnossa tulosteen sidonta

Azure-tapahtumaa-toiminnossa tulosteen sidontaa käytetään kirjoittaminen tapahtumien tapahtuman keskittimeen tapahtuma-muodossa. Lähetä oikeuden tapahtumaa-toiminnossa tapahtumien kirjoittamaan on oltava. 

#### <a name="functionjson-for-event-hub-output-binding"></a>Tapahtuma-toiminnossa Function.JSON tulosteen sidonta

Azure-tapahtumaa-toiminnossa *function.json* tiedoston tulosteen sidonta määrittää seuraavat ominaisuudet:

- `type`: On määritettävä *eventHub*.
- `name`Muuttujan nimi: käyttää funktiota koodissa tapahtuma-toiminnossa viestin. 
- `path`: Tapahtumaa-toiminnossa nimi.
- `connection`: Asetus, joka sisältää nimitilan, joka sijaitsee tapahtumaa-toiminnossa voit yhteysmerkkijonon nimi. Kopioi tämä yhteysmerkkijonon napsauttamalla nimitilan, ei itse tapahtumaa-toiminnossa **Yhteystiedot** -painiketta.  Tämän yhteysmerkkijonon on oltava Lähetä oikeus lähettää viestin tapahtumaa-toiminnossa-muodossa.
- `direction`: On määritettävä *ulos*. 

        {
          "type": "eventHub",
          "name": "outputEventHubMessage",
          "path": "myeventhub",
          "connection": "MyEventHubSend",
          "direction": "out"
        }


#### <a name="azure-event-hub-c-code-example-for-output-binding"></a>Azure tapahtuman keskittimeen C# esimerkki tulosteen sidontaa varten
 
Seuraava C# Esimerkki funktiota koodi esitellään kirjoittaminen tapahtuman tapahtumaa-toiminnossa-tapahtuma-muodossa. Tässä esimerkissä edustaa tapahtumaa-toiminnossa tulosteen sidonta yllä käyttöön C#-ajastin käynnistintä.  
 
    using System;
    
    public static void Run(TimerInfo myTimer, out string outputEventHubMessage, TraceWriter log)
    {
        String msg = $"TimerTriggerCSharp1 executed at: {DateTime.Now}";
    
        log.Verbose(msg);   
        
        outputEventHubMessage = msg;
    }

#### <a name="azure-event-hub-f-code-example-for-output-binding"></a>Azure tapahtuman keskittimeen F # esimerkki tulosteen sidontaa varten

Seuraava F # Esimerkki funktiota koodi esitellään kirjoittaminen tapahtuman tapahtumaa-toiminnossa-tapahtuma-muodossa. Tässä esimerkissä edustaa tapahtumaa-toiminnossa tulosteen sidonta yllä käyttöön C#-ajastin käynnistintä.

    let Run(myTimer: TimerInfo, outputEventHubMessage: byref<string>, log: TraceWriter) =
        let msg = sprintf "TimerTriggerFSharp1 executed at: %s" DateTime.Now.ToString()
        log.Verbose(msg);
        outputEventHubMessage <- msg;

#### <a name="azure-event-hub-nodejs-code-example-for-output-binding"></a>Azure tapahtumaa-toiminnossa Node.js esimerkki tulosteen sidontaa varten
 
Seuraava Node.js Esimerkki funktiota koodi esitellään kirjoittaminen tapahtuman tapahtumaa-toiminnossa-tapahtuma-muodossa. Tässä esimerkissä edustaa tapahtumaa-toiminnossa tulosteen sidonta yllä käyttöön Node.js ajastin käynnistintä.  
 
    module.exports = function (context, myTimer) {
        var timeStamp = new Date().toISOString();
        
        if(myTimer.isPastDue)
        {
            context.log('TimerTriggerNodeJS1 is running late!');
        }

        context.log('TimerTriggerNodeJS1 function ran!', timeStamp);   
        
        context.bindings.outputEventHubMessage = "TimerTriggerNodeJS1 ran at : " + timeStamp;
    
        context.done();
    };

## <a name="next-steps"></a>Seuraavat vaiheet

[AZURE.INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]
