<properties
    pageTitle="Azure Funktiot ilmoituksen keskittimeen sidonta | Microsoft Azure"
    description="Osaavat käyttää Azure ilmoituksen keskittimeen sidonta Azure-funktiot."
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
    ms.date="10/27/2016"
    ms.author="wesmc"/>

# <a name="azure-functions-notification-hub-output-binding"></a>Azure Funktiot ilmoituksen keskittimeen tulosteen sidonta

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Tässä artikkelissa kerrotaan, miten ja koodin Azure ilmoituksen keskittimeen sidontojen Azure-funktioissa. 

[AZURE.INCLUDE [intro](../../includes/functions-bindings-intro.md)] 

Oman Funktiot lähettää määritetyn Azure-ilmoitus-toiminnossa käyttäminen muutamalla koodin rivit push-ilmoitukset. Kuitenkin Azure ilmoitus-toiminnossa on oltava määritettynä,-ympäristö ilmoitukset Services (PNS) haluat käyttää. Lisätietoja määrittäminen Azure-ilmoitus-toiminnosta ja rekisteröi-ilmoitusten vastaanottotilanteiden asiakassovellusten kehittämiseen ohjeaiheessa [käytön aloittaminen ilmoituksen keskittimet](../notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) ja sitten kohde asiakkaan ympäristö yläreunassa.

Voit lähettää ilmoituksia voi olla alkuperäisen ilmoituksia tai mallin ilmoitukset. Alkuperäisen ilmoitukset kohteen tietyn ilmoitus-ympäristö, määritykset `platform` tulosteen sidonta-ominaisuus. Mallin ilmoituksen avulla voidaan kohdistaa useiden ympäristöjen.   

## <a name="notification-hub-output-binding-properties"></a>Ilmoitus keskittimeen tulosteen sidontaominaisuudet

Function.json-tiedosto sisältää seuraavat ominaisuudet:

- `name`: Käyttää toiminnossa ilmoitusviestin funktion koodissa muuttujan nimi.
- `type`: on määritettävä *"notificationHub"*.
- `tagExpression`: Tunnisteen lausekkeiden avulla voit määrittää, että ilmoitukset toimitetaan laitteiden, joka on rekisteröity ilmoituksia, jotka vastaavat merkinnän lauseketta.  Lisätietoja on artikkelissa [Reititys ja tunnisteen lausekkeita](../notification-hubs/notification-hubs-tags-segment-push-message.md).
- `hubName`: Azure-portaalissa ilmoituksen keskittimeen-resurssin nimi.
- `connection`: Yhteysmerkkijonon tämä on oltava **Asetus** -yhteysmerkkijonon, ilmoitus-toiminnossa *DefaultFullSharedAccessSignature* arvo arvoksi.
- `direction`: Määritä arvoksi *","*. 
- `platform`: Ympäristö ominaisuus osoittaa, ilmoitus-ympäristö ilmoituksen kohteet. On oltava jokin seuraavista arvoista:
    - `template`: Tämä on oletusarvo-ympäristö, jos platform-ominaisuus on jätetty pois tulosteen sidonta. Mallien ilmoitukset voidaan kohdentamisen minkä tahansa ympäristö on määritetty Azure ilmoitus-toiminnossa. Lisätietoja mallien avulla yleinen lähettämään cross platform-ilmoituksia Azure-ilmoitus-toiminnosta on artikkelissa [malleja](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md).
    - `apns`: Apple Push-ilmoituspalvelu. Katso lisätietoja määrittämisestä-APN ja saat ilmoituksen Asiakas-sovellukseen, ilmoitus-toiminnossa [lähettäminen push-ilmoitukset ja Azure ilmoituksen keskittimet iOS-laitteille](../notification-hubs/notification-hubs-ios-apple-push-notification-apns-get-started.md) 
    - `adm`: [Amazon laitteen Messaging](https://developer.amazon.com/device-messaging). Katso lisätietoja ilmoitus-toiminnossa määrittäminen ADM ja saat ilmoituksen Kindle-sovelluksessa [Ilmoituksen keskittimet Kindle-sovellusten käytön aloittaminen](../notification-hubs/notification-hubs-kindle-amazon-adm-push-notification.md) 
    - `gcm`: [Google Cloud Messaging](https://developers.google.com/cloud-messaging/). Firebase Cloud Messaging, joka on GCM uuden version tuetaan myös. Katso lisätietoja määrittämisestä-ilmoitus-toiminnossa GCM/FCM ja saat ilmoituksen asiakkaan Android-sovelluksen [, Android-laitteeseen Azure ilmoituksen keskittimet lähettäminen push-ilmoitukset](../notification-hubs/notification-hubs-android-push-notification-google-fcm-get-started.md)
    - `wns`: [Windows Push-ilmoituksen Services](https://msdn.microsoft.com/en-us/windows/uwp/controls-and-patterns/tiles-and-notifications-windows-push-notification-services--wns--overview) kohdistaminen Windows-käyttöjärjestelmien työpöytäsovelluksiin. Windows Phone 8.1 ja uudempi versio tukee myös WNS. Lisätietoja määrittämisestä WNS ja saat ilmoituksen yleinen Windows Platform (UWP)-sovelluksessa, ilmoitus-toiminnossa artikkelissa [ilmoituksen keskittimet for Windowsin yleinen ympäristö sovellusten käytön aloittaminen](../notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md)
    - `mpns`: [Microsoftin Push-ilmoituspalvelu](https://msdn.microsoft.com/en-us/library/windows/apps/ff402558.aspx). Tässä ympäristössä tukee Windows Phone 8 ja aiemmissa Windows Phone-ympäristöissä. Katso lisätietoja määrittämisestä-MPNS ja saat ilmoituksen Windows Phone-sovelluksessa, ilmoitus-toiminnossa [lähettäminen push-ilmoitukset ja Azure ilmoituksen keskittimet Windows Phone-](../notification-hubs/notification-hubs-windows-mobile-push-notifications-mpns.md)
 
Esimerkki function.json:

    {
      "bindings": [
        {
          "name": "notification",
          "type": "notificationHub",
          "tagExpression": "",
          "hubName": "my-notification-hub",
          "connection": "MyHubConnectionString",
          "platform": "gcm",
          "direction": "out"
        }
      ],
      "disabled": false
    }

## <a name="notification-hub-connection-string-setup"></a>Ilmoitus-toiminnossa merkkijonon yhteysasetukset

Käyttämään ilmoituksen keskittimeen tulostus-sidonta, sinun on määritettävä valitsemalla yhteysmerkkijono. Näin voit tehdä *liittää* -välilehdessä valitsemalla ilmoitus-toiminnossa tai luoda uutta ryhmäsivustoa. 

Voit lisätä aiemmin keskittimeen yhteysmerkkijonon myös manuaalisesti lisäämällä yhteysmerkkijonon *DefaultFullSharedAccessSignature* ilmoitus-keskittimeen. Tämä yhteysmerkkijono sisältää funktion oikeudet lähettää ilmoitukset. *DefaultFullSharedAccessSignature* yhteyden merkkijonoarvo niitä voi käyttää ilmoituksen keskittimeen resurssin Azure-portaalissa tärkeimmät sivu **näppäimet** -painike. Voit lisätä oman keskittimeen yhteysmerkkijonon manuaalisesti noudattamalla seuraavia ohjeita: 

1. Azure-portaalin **funktion sovellus** -sivu, valitse **funktion App-asetukset > App palvelun asetukset**.

2. Valitse **Sovellusasetukset** **asetukset** -sivu.

3. Siirry **yhteyden merkkijonoja** -osioon ja *DefaultFullSharedAccessSignature* arvo ilmoitus-toiminnossa nimetty sisäänkäynnin lisääminen. Tyypin muuttaminen **Mukautettu**.
4. Viittaus tulosteen sidontojen yhteyden merkkijonon nimeä. Samalla tavalla kuin edellisessä esimerkissä käytetty **MyHubConnectionString** .

## <a name="native-notification-examples"></a>Alkuperäisen ilmoituksen esimerkkejä

#### <a name="apns-native-notifications-with-c-queue-triggers"></a>APN alkuperäisen ilmoitukset, joiden C# jonon Käynnistimet

Tässä esimerkissä näytetään opit käyttämään [Microsoft Azure ilmoituksen keskittimet kirjaston](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) määritettyjen Lähetä alkuperäisen APN-ilmoitus. 

    #r "Microsoft.Azure.NotificationHubs"
    #r "Newtonsoft.Json"
    
    using System;
    using Microsoft.Azure.NotificationHubs;
    using Newtonsoft.Json;
    
    public static async Task Run(string myQueueItem, IAsyncCollector<Notification> notification, TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
    
        // In this example the queue item is a new user to be processed in the form of a JSON string with 
        // a "name" value.
        //
        // The JSON format for a native APNS notification is ...
        // { "aps": { "alert": "notification message" }}  

        log.Info($"Sending APNS notification of a new user");   
        dynamic user = JsonConvert.DeserializeObject(myQueueItem);  
        string apnsNotificationPayload = "{\"aps\": {\"alert\": \"A new user wants to be added (" + 
                                            user.name + ")\" }}";
        log.Info($"{apnsNotificationPayload}");
        await notification.AddAsync(new AppleNotification(apnsNotificationPayload));        
    }

#### <a name="gcm-native-notifications-with-c-queue-triggers"></a>GCM alkuperäisen ilmoitukset, joiden C# jonon Käynnistimet

Tässä esimerkissä näytetään opit käyttämään [Microsoft Azure ilmoituksen keskittimet kirjaston](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) määritettyjen alkuperäisen GCM ilmoituksen lähettäminen. 

    #r "Microsoft.Azure.NotificationHubs"
    #r "Newtonsoft.Json"
    
    using System;
    using Microsoft.Azure.NotificationHubs;
    using Newtonsoft.Json;
    
    public static async Task Run(string myQueueItem, IAsyncCollector<Notification> notification, TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
    
        // In this example the queue item is a new user to be processed in the form of a JSON string with 
        // a "name" value.
        //
        // The JSON format for a native GCM notification is ...
        // { "data": { "message": "notification message" }}  

        log.Info($"Sending GCM notification of a new user");    
        dynamic user = JsonConvert.DeserializeObject(myQueueItem);  
        string gcmNotificationPayload = "{\"data\": {\"message\": \"A new user wants to be added (" + 
                                            user.name + ")\" }}";
        log.Info($"{gcmNotificationPayload}");
        await notification.AddAsync(new GcmNotification(gcmNotificationPayload));       
    }

#### <a name="wns-native-notifications-with-c-queue-triggers"></a>WNS alkuperäisen ilmoitukset, joiden C# jonon Käynnistimet

Tässä esimerkissä näytetään opit käyttämään [Microsoft Azure ilmoituksen keskittimet kirjaston](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) määritettyjen Lähetä alkuperäisen WNS ilmoitusruudun. 

    #r "Microsoft.Azure.NotificationHubs"
    #r "Newtonsoft.Json"
    
    using System;
    using Microsoft.Azure.NotificationHubs;
    using Newtonsoft.Json;
    
    public static async Task Run(string myQueueItem, IAsyncCollector<Notification> notification, TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
    
        // In this example the queue item is a new user to be processed in the form of a JSON string with 
        // a "name" value.
        //
        // The XML format for a native WNS toast notification is ...
        // <?xml version="1.0" encoding="utf-8"?>
        // <toast>
        //   <visual>
        //     <binding template="ToastText01">
        //       <text id="1">notification message</text>
        //     </binding>
        //   </visual>
        // </toast>

        log.Info($"Sending WNS toast notification of a new user");  
        dynamic user = JsonConvert.DeserializeObject(myQueueItem);  
        string wnsNotificationPayload = "<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
                                        "<toast><visual><binding template=\"ToastText01\">" +
                                            "<text id=\"1\">" + 
                                                "A new user wants to be added (" + user.name + ")" + 
                                            "</text>" +
                                        "</binding></visual></toast>";

        log.Info($"{wnsNotificationPayload}");
        await notification.AddAsync(new WindowsNotification(wnsNotificationPayload));       
    }


## <a name="template-notification-examples"></a>Mallin ilmoituksen esimerkkejä

#### <a name="template-example-for-nodejs-timer-triggers"></a>Mallin esimerkki Node.js ajastimen Käynnistimet 

Tässä esimerkissä lähettää ilmoituksen, joka sisältää [mallin rekisteröinti](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) `location` ja `message`.

    module.exports = function (context, myTimer) {
        var timeStamp = new Date().toISOString();
       
        if(myTimer.isPastDue)
        {
            context.log('Node.js is running late!');
        }
        context.log('Node.js timer trigger function ran!', timeStamp);  
        context.bindings.notification = {
            location: "Redmond",
            message: "Hello from Node!"
        };
        context.done();
    };

#### <a name="template-example-for-f-timer-triggers"></a>F # ajastimen käynnistimet malli-esimerkki

Tässä esimerkissä lähettää ilmoituksen, joka sisältää [mallin rekisteröinti](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) `location` ja `message`.

    let Run(myTimer: TimerInfo, notification: byref<IDictionary<string, string>>) =
        notification = dict [("location", "Redmond"); ("message", "Hello from F#!")]


#### <a name="template-example-using-an-out-parameter"></a>Mallin esimerkki out parametrin avulla 

Tässä esimerkissä lähettää ilmoituksen, joka sisältää [mallin rekisteröinti](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) `message` paikkamerkki mallissa.

    using System;
    using System.Threading.Tasks;
    using System.Collections.Generic;
     
    public static void Run(string myQueueItem,  out IDictionary<string, string> notification, TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
        notification = GetTemplateProperties(myQueueItem);
    }
     
    private static IDictionary<string, string> GetTemplateProperties(string message)
    {
        Dictionary<string, string> templateProperties = new Dictionary<string, string>();
        templateProperties["message"] = message;
        return templateProperties;
    }

#### <a name="template-example-with-asynchronous-function"></a>Mallin esimerkki asynkroninen-toiminnolla

Jos käytössäsi on asynkroninen koodi, parametreja, ei sallita. Käytä tässä tapauksessa `IAsyncCollector` palauttaa malli-ilmoitus. Seuraava koodi on esimerkki asynkroninen yllä olevan koodin. 

    using System;
    using System.Threading.Tasks;
    using System.Collections.Generic;
    
    public static async Task Run(string myQueueItem, IAsyncCollector<IDictionary<string,string>> notification, TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
    
        log.Info($"Sending Template Notification to Notification Hub");
        await notification.AddAsync(GetTemplateProperties(myQueueItem));    
    }
    
    private static IDictionary<string, string> GetTemplateProperties(string message)
    {
        Dictionary<string, string> templateProperties = new Dictionary<string, string>();
        templateProperties["user"] = "A new user wants to be added : " + message;
        return templateProperties;
    }

#### <a name="template-example-using-json"></a>JSON käyttämällä mallia-Esimerkki 

Tässä esimerkissä lähettää ilmoituksen, joka sisältää [mallin rekisteröinti](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) `message` paikkamerkki-mallissa käyttämällä kelvollinen JSON-merkkijono.

    using System;
     
    public static void Run(string myQueueItem,  out string notification, TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
        notification = "{\"message\":\"Hello from C#. Processed a queue item!\"}";
    }


#### <a name="template-example-using-notification-hubs-library-types"></a>Mallin esimerkkiä ilmoituksen keskittimet kirjaston tyypit

Tässä esimerkissä näytetään käyttämisestä määritettyjen [Microsoft Azure ilmoituksen keskittimet kirjastoon](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/). 


    #r "Microsoft.Azure.NotificationHubs"

    using System;
    using System.Threading.Tasks;
    using Microsoft.Azure.NotificationHubs;
     
    public static void Run(string myQueueItem,  out Notification notification, TraceWriter log)
    {
       log.Info($"C# Queue trigger function processed: {myQueueItem}");
       notification = GetTemplateNotification(myQueueItem);
    }

    private static TemplateNotification GetTemplateNotification(string message)
    {
        Dictionary<string, string> templateProperties = new Dictionary<string, string>();
        templateProperties["message"] = message;
        return new TemplateNotification(templateProperties);
    }

## <a name="next-steps"></a>Seuraavat vaiheet

[AZURE.INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]
