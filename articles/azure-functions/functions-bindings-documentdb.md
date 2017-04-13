<properties
    pageTitle="Azure Funktiot DocumentDB sidontojen | Microsoft Azure"
    description="Osaavat käyttää Azure DocumentDB sidontojen Azure-funktiot."
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

# <a name="azure-functions-documentdb-bindings"></a>Azure Funktiot DocumentDB sidontojen

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Tässä artikkelissa kerrotaan, miten ja koodin Azure DocumentDB sidontojen Azure-funktioissa. 

[AZURE.INCLUDE [intro](../../includes/functions-bindings-intro.md)] 

## <a id="docdbinput"></a>Azure DocumentDB syötteen sidonta

Syötteen sidontojen voit ladata tiedoston DocumentDB kokoelmasta ja välittää sen suoraan yhteyttä sidonta. Tiedostotunnisteen määritettävissä käynnistin, joka käynnistää funktion perusteella. C#-funktiossa tietueeseen tehdyt muutokset lähetetään automaattisesti takaisin kokoelman kun funktio poistuu onnistuneesti.

#### <a name="functionjson-for-documentdb-input-binding"></a>Function.JSON DocumentDB näytettävä sidontaa varten

*Function.json* -tiedosto sisältää seuraavat ominaisuudet:

- `name`: Funktio käyttää asiakirjan muuttujan nimi.
- `type`: on määritettävä "documentdb".
- `databaseName`Tietokanta: sisältävä tiedosto.
- `collectionName`Sivustokokoelman: sisältävä tiedosto.
- `id`: Tunnus asiakirjan hakemiseen. Tämä ominaisuus tukee sidontojen samalla tavalla kuin "{queueTrigger}", joka käyttää jonon viestin merkkijonoarvo asiakirjana ID-tunnuksellasi.
- `connection`: Merkkijonon tämä on oltava päätepiste DocumentDB tilin asetus-arvoksi. Jos valitset tilisi liittää-välilehti, uusi sovellus-asetus luodaan puolestasi, joka vie Seuraava lomake yourAccount_DOCUMENTDB nimellä. Jos haluat luoda manuaalisesti asetus, todellinen yhteysmerkkijonon tulee seuraavanlainen, AccountEndpoint =<Endpoint for your account>; AccountKey =<Your primary access key>;.
- "suunta: Määritä arvoksi *"tomaatti"*.

Esimerkki *function.json*:
 
    {
      "bindings": [
        {
          "name": "document",
          "type": "documentdb",
          "databaseName": "MyDatabase",
          "collectionName": "MyCollection",
          "id" : "{queueTrigger}",
          "connection": "MyAccount_DOCUMENTDB",     
          "direction": "in"
        }
      ],
      "disabled": false
    }

#### <a name="azure-documentdb-input-code-example-for-a-c-queue-trigger"></a>Azure DocumentDB syötteen koodi, esimerkiksi C#-jonossa käynnistimen
 
Käytä edellä olevassa esimerkissä function.json, DocumentDB näytettävä sidonta noutaa asiakirjan, joka vastaa merkkijonon jonon viestin tunnuksella ja välittää "document"-parametrin. Jos asiakirjassa ei löydy, "document"-parametrin arvo on nolla. Tiedosto päivitetään arvona teksti sitten, kun funktio poistuu.
 
    public static void Run(string myQueueItem, dynamic document)
    {   
        document.text = "This has changed.";
    }

#### <a name="azure-documentdb-input-code-example-for-an-f-queue-trigger"></a>Azure DocumentDB syötteen koodiesimerkki F # jonon käynnistin

Käytä edellä olevassa esimerkissä function.json, DocumentDB näytettävä sidonta noutaa asiakirjan, joka vastaa merkkijonon jonon viestin tunnuksella ja välittää "document"-parametrin. Jos asiakirjassa ei löydy, "document"-parametrin arvo on nolla. Tiedosto päivitetään arvona teksti sitten, kun funktio poistuu.

    open FSharp.Interop.Dynamic
    let Run(myQueueItem: string, document: obj) =
        document?text <- "This has changed."

Tarvitset `project.json` tiedosto, jonka määrittää NuGet `FSharp.Interop.Dynamic` ja `Dynamitey` pakettien paketin riippuvuudet, kuin tältä:

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

Tämä käyttää NuGet hakeaksesi riippuvuudet ja viittaavat niiden komentosarja.

#### <a name="azure-documentdb-input-code-example-for-a-nodejs-queue-trigger"></a>Azure DocumentDB syötteen koodiesimerkki Node.js jonossa käynnistimen
 
Käytä edellä olevassa esimerkissä function.json, DocumentDB näytettävä sidonta noutaa asiakirjan, joka vastaa merkkijonon jonon viestin tunnuksella ja välittää sen `documentIn` sidonta ominaisuus. Node.js funktioissa päivitetyt asiakirjoja ei lähetetä takaisin kokoelmasta. Voit siirtää syötteen sidonta suoraan sidonta DocumentDB-tulosteen nimetyn `documentOut` tukemaan päivityksiä. Koodin tässä esimerkissä päivittää syötteen asiakirjan text-ominaisuuteen, ja määrittää tulostus-asiakirjana.
 
    module.exports = function (context, input) {   
        context.bindings.documentOut = context.bindings.documentIn;
        context.bindings.documentOut.text = "This was updated!";
        context.done();
    };

## <a id="docdboutput"></a>Azure DocumentDB tulosteen sidontojen

Oman funktioita voit kirjoittaa JSON **Azure DocumentDB asiakirjan** Azure DocumentDB-tietokannan asiakirjojen tulostus sidonta. Lisätietoja Azure DocumentDB Tarkista [DocumentDB johdanto](../documentdb/documentdb-introduction.md) ja [opetusohjelman käytön aloittaminen](../documentdb/documentdb-get-started.md).

#### <a name="functionjson-for-documentdb-output-binding"></a>Function.JSON DocumentDB tulosteen sidonta

Function.json-tiedosto sisältää seuraavat ominaisuudet:

- `name`: Funktio käyttää uuden asiakirjan muuttujan nimi.
- `type`: on määritettävä *"documentdb"*.
- `databaseName`Tietokanta: sisältävän sivustokokoelman, johon uusi tiedosto luodaan.
- `collectionName`: Sivustokokoelman, jossa uusi tiedosto luodaan.
- `createIfNotExists`: Tämä on totuusarvo arvo, joka ilmaisee, onko kokoelman luodaan, jos sitä ei ole. Oletusarvo on *Epätosi*. Syy tämä on uusi sivustokokoelmat luodaan varattu siirtonopeuden, jonka on hinnat vaikutukset. Lisätietoja seuraavasta [hinnat sivun](https://azure.microsoft.com/pricing/details/documentdb/).
- `connection`: Merkkijonon tämä on oltava **Asetus** on määritetty päätepisteen DocumentDB tilissäsi. Jos valitset tilisi **liittää** -välilehti, uusi sovellus-asetus luodaan sinulle nimellä, joka vie ja `yourAccount_DOCUMENTDB`. Jos haluat luoda manuaalisesti asetus, todellinen yhteysmerkkijonon on otettava ja `AccountEndpoint=<Endpoint for your account>;AccountKey=<Your primary access key>;`. 
- `direction`: Määritä arvoksi *","*. 
 
Esimerkki function.json:

    {
      "bindings": [
        {
          "name": "document",
          "type": "documentdb",
          "databaseName": "MyDatabase",
          "collectionName": "MyCollection",
          "createIfNotExists": false,
          "connection": "MyAccount_DOCUMENTDB",
          "direction": "out"
        }
      ],
      "disabled": false
    }


#### <a name="azure-documentdb-output-code-example-for-a-nodejs-queue-trigger"></a>Azure DocumentDB tulosteen koodiesimerkki Node.js jonossa käynnistimen

    module.exports = function (context, input) {
       
        context.bindings.document = {
            text : "I'm running in a Node function! Data: '" + input + "'"
        }   
     
        context.done();
    };

Tuloksena oleva asiakirja:

    {
      "text": "I'm running in a Node function! Data: 'example queue data'",
      "id": "01a817fe-f582-4839-b30c-fb32574ff13f"
    }
 

#### <a name="azure-documentdb-output-code-example-for-an-f-queue-trigger"></a>Azure DocumentDB tulosteen koodiesimerkki F # jonon käynnistimen

    open FSharp.Interop.Dynamic
    let Run(myQueueItem: string, document: obj) =
        document?text <- (sprintf "I'm running in an F# function! %s" myQueueItem)

#### <a name="azure-documentdb-output-code-example-for-a-c-queue-trigger"></a>Azure DocumentDB tulosteen koodiesimerkki C#-jonossa käynnistimen


    using System;

    public static void Run(string myQueueItem, out object document, TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
       
        document = new {
            text = $"I'm running in a C# function! {myQueueItem}"
        };
    }


#### <a name="azure-documentdb-output-code-example-setting-file-name"></a>Azure DocumentDB tulosteen koodin Esimerkki asetus tiedostonimi

Jos haluat määrittää asiakirjan nimi-funktion, vain määrittää `id` arvo.  Esimerkki: JSON sisällön työntekijän poistettiin kyselyjä jonossa seuraavankaltaiselta:

    {
      "name" : "John Henry",
      "employeeId" : "123456",
      "address" : "A town nearby"
    }

Voit käyttää seuraavia C#-koodin jonon käynnistimen-funktiossa: 
    
    #r "Newtonsoft.Json"
    
    using System;
    using Newtonsoft.Json;
    using Newtonsoft.Json.Linq;
    
    public static void Run(string myQueueItem, out object employeeDocument, TraceWriter log)
    {
        log.Info($"C# Queue trigger function processed: {myQueueItem}");
        
        dynamic employee = JObject.Parse(myQueueItem);
        
        employeeDocument = new {
            id = employee.name + "-" + employee.employeeId,
            name = employee.name,
            employeeId = employee.employeeId,
            address = employee.address
        };
    }

Tai vastaava F #-koodi:

    open FSharp.Interop.Dynamic
    open Newtonsoft.Json

    type Employee = {
        id: string
        name: string
        employeeId: string
        address: string
    }

    let Run(myQueueItem: string, employeeDocument: byref<obj>, log: TraceWriter) =
        log.Info(sprintf "F# Queue trigger function processed: %s" myQueueItem)
        let employee = JObject.Parse(myQueueItem)
        employeeDocument <-
            { id = sprintf "%s-%s" employee?name employee?employeeId
              name = employee?name
              employeeId = employee?id
              address = employee?address }

Esimerkki tulos:

    {
      "id": "John Henry-123456",
      "name": "John Henry",
      "employeeId": "123456",
      "address": "A town nearby"
    }

## <a name="next-steps"></a>Seuraavat vaiheet

[AZURE.INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]
