<properties
    pageTitle="Azure Funktiot Twilio sidonta | Microsoft Azure"
    description="Tietoja Twilio sidontojen käyttäminen Azure-funktiot."
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
    ms.date="10/20/2016"
    ms.author="wesmc"/>

# <a name="azure-functions-twilio-output-binding"></a>Azure Funktiot Twilio tulosteen sidonta

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Tässä artikkelissa kerrotaan, kuinka voit määrittää ja käyttää Twilio sidontojen Azure-funktiot. 

[AZURE.INCLUDE [intro](../../includes/functions-bindings-intro.md)] 

Azure Funktiot tukee Twilio tulosteen sidontojen käyttöön oman Funktiot lähetetään tekstiviestien muutaman viivoilla koodin ja [Twilio](https://www.twilio.com/) -tili. 
 

## <a name="functionjson-for-azure-notification-hub-output-binding"></a>Azure ilmoituksen keskittimeen Function.JSON tulosteen sidonta

Function.json-tiedosto sisältää seuraavat ominaisuudet:

- `name`: Muuttujan nimi, jota käytetään tekstin Twilio tekstiviesti-funktio.
- `type`: on määritettävä *"twilioSms"*.
- `accountSid`: Tämä arvo on määritettävä sovelluksen-asetus, joka sisältää Twilio-tilin suojaustunnus nimeä.
- `authToken`: Tämä arvo on määritettävä sovelluksen-asetus, joka sisältää Twilio todennus-tunnuksen nimi.
- `to`: Tämä arvo on numero, joka lähetetään SMS teksti.
- `from`: Tämä arvo on puhelinnumero, SMS tekstin lähettänyt.
- `direction`: Määritä arvoksi *","*.
- `body`: Tämä arvo voidaan kiintolevyn koodin tekstin tekstiviesti, jos sinun ei tarvitse määrittää sen dynaamisesti funktion koodi. 

 
Esimerkki function.json:

    {
      "type": "twilioSms",
      "name": "message",
      "accountSid": "TwilioAccountSid",
      "authToken": "TwilioAuthToken",
      "to": "+1704XXXXXXX",
      "from": "+1425XXXXXXX",
      "direction": "out",
      "body": "Azure Functions Testing"
    }


## <a name="example-c-queue-trigger-with-twilio-output-binding"></a>Esimerkki C# jonon käynnistimen kanssa Twilio tulosteen sidonta

#### <a name="synchronous"></a>Synkronoitu

Tämän painikkeen esimerkkikoodi Azuren tallennustilaan-jonossa käynnistimen käyttää out parametria Tekstiviestin lähettäminen tilauksen tehneen asiakkaan.

    #r "Newtonsoft.Json"
    #r "Twilio.Api"

    using System;
    using Newtonsoft.Json;
    using Twilio;

    public static void Run(string myQueueItem, out SMSMessage message,  TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
    
        // In this example the queue item is a JSON string representing an order that contains the name of a 
        // customer and a mobile number to send text updates to.
        dynamic order = JsonConvert.DeserializeObject(myQueueItem);
        string msg = "Hello " + order.name + ", thank you for your order.";
    
        // Even if you want to use a hard coded message and number in the binding, you must at least 
        // initialize the SMSMessage variable.
        message = new SMSMessage();

        // A dynamic message can be set instead of the body in the output binding. In this example, we use 
        // the order information to personalize a text message to the mobile number provided for
        // order status updates.
        message.Body = msg;
        message.To = order.mobileNumber;
    }

#### <a name="asynchronous"></a>Asynkroninen

Tämä asynkroninen esimerkkikoodi Azuren tallennustilaan-jonossa käynnistimen lähettää tekstiviestin tilauksen tehneen asiakkaan.

    #r "Newtonsoft.Json"
    #r "Twilio.Api"
     
    using System;
    using Newtonsoft.Json;
    using Twilio;
    
    public static async Task Run(string myQueueItem, IAsyncCollector<SMSMessage> message,  TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");

        // In this example the queue item is a JSON string representing an order that contains the name of a 
        // customer and a mobile number to send text updates to.
        dynamic order = JsonConvert.DeserializeObject(myQueueItem);
        string msg = "Hello " + order.name + ", thank you for your order.";
    
        // Even if you want to use a hard coded message and number in the binding, you must at least 
        // initialize the SMSMessage variable.
        SMSMessage smsText = new SMSMessage();

        // A dynamic message can be set instead of the body in the output binding. In this example, we use 
        // the order information to personalize a text message to the mobile number provided for
        // order status updates.
        smsText.Body = msg;
        smsText.To = order.mobileNumber;
        
        await message.AddAsync(smsText);
    }


## <a name="example-nodejs-queue-trigger-with-twilio-output-binding"></a>Esimerkki Node.js jonossa käynnistimen kanssa Twilio tulosteen sidonta

Tässä esimerkissä Node.js lähettää tekstiviestin tilauksen tehneen asiakkaan.

    module.exports = function (context, myQueueItem) {
        context.log('Node.js queue trigger function processed work item', myQueueItem);
    
        // In this example the queue item is a JSON string representing an order that contains the name of a 
        // customer and a mobile number to send text updates to.
        var msg = "Hello " + myQueueItem.name + ", thank you for your order.";
    
        // Even if you want to use a hard coded message and number in the binding, you must at least 
        // initialize the message binding.
        context.bindings.message = {};
    
        // A dynamic message can be set instead of the body in the output binding. In this example, we use 
        // the order information to personalize a text message to the mobile number provided for
        // order status updates.
        context.bindings.message = {
            body : msg,
            to : myQueueItem.mobileNumber
        };
    
        context.done();
    };

## <a name="next-steps"></a>Seuraavat vaiheet

[AZURE.INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]
