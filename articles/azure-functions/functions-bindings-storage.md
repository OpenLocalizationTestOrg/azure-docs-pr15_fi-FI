<properties
    pageTitle="Azure Funktiot käynnistimien ja Azure-tallennustilan sidontojen | Microsoft Azure"
    description="Tietoja Azuren tallennustilaan käynnistimien ja sidontojen käyttämisestä Azure-funktiot."
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
    ms.author="chrande"/>

# <a name="azure-functions-triggers-and-bindings-for-azure-storage"></a>Azure Funktiot käynnistimien ja Azure-tallennustilan sidontojen

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Tässä artikkelissa kerrotaan määrittämisestä ja koodin Azuren tallennustilaan käynnistimien ja sidontojen Azure-funktioissa. 

[AZURE.INCLUDE [intro](../../includes/functions-bindings-intro.md)] 

## <a id="storagequeuetrigger"></a>Azure tallennustilan jonon käynnistin

#### <a name="functionjson-for-storage-queue-trigger"></a>tallennustilan jonon käynnistimen Function.JSON

*Function.json* tiedosto määrittää seuraavat ominaisuudet.

- `name`Muuttujan nimi: käyttää funktiota koodissa jonossa tai jono-viestin. 
- `queueName`: Jonossa lisääminen nimi. Katso jonon nimeämiskäytäntö säännöt [nimeäminen olevien ja metatiedot](https://msdn.microsoft.com/library/dd179349.aspx).
- `connection`: Asetus, joka sisältää tallennustilan yhteysmerkkijonon nimi. Jos jätät `connection` toimivat tyhjä, käynnistin funktio-sovellus, joka on määritetty AzureWebJobsStorage sovelluksen määrittämällä yhteysmerkkijonon tallennustilan.
- `type`: On määritettävä *queueTrigger*.
- `direction`: On määritettävä *sisään*. 

Esimerkki *function.json* tallennustilan jonon käynnistimen:

```json
{
    "disabled": false,
    "bindings": [
        {
            "name": "myQueueItem",
            "queueName": "myqueue-items",
            "connection":"",
            "type": "queueTrigger",
            "direction": "in"
        }
    ]
}
```

#### <a name="queue-trigger-supported-types"></a>Jonon käynnistimen tuetut tyypit

Jonon viesti voi poistaa johonkin seuraavista:

* Objektin (JSON)
* Merkkijono
* DBCS-matriisi 
* `CloudQueueMessage`(C#) 

#### <a name="queue-trigger-metadata"></a>Jonon käynnistimen metatiedot

Voit hankkia jonon metatiedot-funktion avulla nämä muuttujien nimissä:

* expirationTime
* insertionTime
* nextVisibleTime
* tunnus
* popReceipt
* dequeueCount
* queueTrigger (hakemiseen jonon viestiteksti merkkijonona erikseen)

C# koodin esimerkin hakee ja lokit jonon metatietojen:

```csharp
public static void Run(string myQueueItem, 
    DateTimeOffset expirationTime, 
    DateTimeOffset insertionTime, 
    DateTimeOffset nextVisibleTime,
    string queueTrigger,
    string id,
    string popReceipt,
    int dequeueCount,
    TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}\n" +
        $"queueTrigger={queueTrigger}\n" +
        $"expirationTime={expirationTime}\n" +
        $"insertionTime={insertionTime}\n" +
        $"nextVisibleTime={nextVisibleTime}\n" +
        $"id={id}\n" +
        $"popReceipt={popReceipt}\n" + 
        $"dequeueCount={dequeueCount}");
}
```

#### <a name="handling-poison-queue-messages"></a>Torjuttujen sanomien käsitteleminen

Viestit, joiden sisältö aiheuttaa funktio voi epäonnistua kutsutaan *sanomien*. Kun funktio epäonnistuu, jonossa viesti ei poisteta ja tekstin on noudettu uudelleen aiheuttaa toistettava jakson. SDK automaattisesti voit keskeyttää jakson jälkeen Iteraatioita rajoitettu määrä tai voit tehdä sen manuaalisesti.

SDK soittaa funktion enintään 5 luvulla käsittelemään jonossa viestin. Jos viidennen yritä epäonnistuu, viesti siirretään torjuttujen jonossa.

Torjuttujen jonossa nimeltä *{originalqueuename}*-torjuttujen. Voit kirjoittaa funktion prosessin viesteihin torjuttujen jonossa kirjaaminen ne tai lähettämällä ilmoituksen, että manuaalinen huomiota tarvita. 

Jos haluat käsitellä sanomien manuaalisesti, voit hankkia kuinka monta kertaa, viestiin on noudettu käsittelyä varten valitsemalla `dequeueCount`.

## <a id="storagequeueoutput"></a>Azure tallennustilan jonon tulosteen sidonta

#### <a name="functionjson-for-storage-queue-output-binding"></a>tallennustilan jonon Function.JSON tulosteen sidonta

*Function.json* tiedosto määrittää seuraavat ominaisuudet.

- `name`Muuttujan nimi: käyttää funktiota koodissa jonossa tai jono-viestin. 
- `queueName`: Jonossa nimi. Katso jonon nimeämiskäytäntö säännöt [nimeäminen olevien ja metatiedot](https://msdn.microsoft.com/library/dd179349.aspx).
- `connection`: Asetus, joka sisältää tallennustilan yhteysmerkkijonon nimi. Jos jätät `connection` toimivat tyhjä, käynnistin funktio-sovellus, joka on määritetty AzureWebJobsStorage sovelluksen määrittämällä yhteysmerkkijonon tallennustilan.
- `type`: On määritettävä *jono*.
- `direction`: On määritettävä *ulos*. 

Esimerkki *function.json* tallennustilan jonon tulosteen sidonta, joka käyttää jonon käynnistin ja kirjoittaa jonon sanoman:

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnection",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "myQueue",
      "queueName": "samples-workitems-out",
      "connection": "MyStorageConnection",
      "type": "queue",
      "direction": "out"
    }
  ],
  "disabled": false
}
``` 

#### <a name="queue-output-binding-supported-types"></a>Jonon tulosteen sidonta Tuetut tiedostotyypit

`queue` Sidonta voi muuntaa sarjaksi seuraavanlaisia jonon viestiin:

* Objektin (`out T` C#, Luo viestin null-objektia, jos parametri on tyhjä, kun funktio päättyy)
* Merkkijono (`out string` C#, Luo jonon viestin, jos parametriarvo on muu kuin null, kun funktio päättyy)
* DBCS-matriisin (`out byte[]` C# toimii, kuten merkkijono) 
* `out CloudQueueMessage`(C#-toiminta merkkijono) 

C# voit myös sitoa `ICollector<T>` tai `IAsyncCollector<T>` missä `T` on yksi tuetut tietotyypit.

#### <a name="queue-output-binding-code-examples"></a>Jonon tulosteen sidonta koodiesimerkkejä

C# koodin esimerkin kirjoittaa syötteen jonon kustakin viestistä yksittäisen tulosteen jonon viestin.

```csharp
public static void Run(string myQueueItem, out string myOutputQueueItem, TraceWriter log)
{
    myOutputQueueItem = myQueueItem + "(next step)";
}
```

C# koodin esimerkin kirjoittaa useita viestejä käyttämällä `ICollector<T>` (käytä `IAsyncCollector<T>` asynkronisen-funktion):

```csharp
public static void Run(string myQueueItem, ICollector<string> myQueue, TraceWriter log)
{
    myQueue.Add(myQueueItem + "(step 1)");
    myQueue.Add(myQueueItem + "(step 2)");
}
```

## <a id="storageblobtrigger"></a>Azure tallennustilan blob käynnistin

#### <a name="functionjson-for-storage-blob-trigger"></a>tallennustilan blob käynnistimen Function.JSON

*Function.json* tiedosto määrittää seuraavat ominaisuudet.

- `name`Muuttujan nimi: käyttää funktiota koodissa blob. 
- `path`: Polkua, joka määrittää säilön seurannassa ja voit myös blob nimi määräytyy.
- `connection`: Asetus, joka sisältää tallennustilan yhteysmerkkijonon nimi. Jos jätät `connection` toimivat tyhjä, käynnistin funktio-sovellus, joka on määritetty AzureWebJobsStorage sovelluksen määrittämällä yhteysmerkkijonon tallennustilan.
- `type`: On määritettävä *blobTrigger*.
- `direction`: On määritettävä *sisään*.

Esimerkki *function.json* tallennustilan blob käynnistimen, joka seuraa BLOB-objektit, jotka lisätään näytteiden workitems säilö:

```json
{
    "disabled": false,
    "bindings": [
        {
            "name": "myBlob",
            "type": "blobTrigger",
            "direction": "in",
            "path": "samples-workitems",
            "connection":""
        }
    ]
}
```

#### <a name="blob-trigger-supported-types"></a>Blob-objektien käynnistimen tuetut tyypit

Blob voi poistaa mitään solmu tai C#-funktioissa seuraavanlaisia:

* Objektin (JSON)
* Merkkijono

C#-funktioissa voit myös sitoa jokin seuraavista:

* `TextReader`
* `Stream`
* `ICloudBlob`
* `CloudBlockBlob`
* `CloudPageBlob`
* `CloudBlobContainer`
* `CloudBlobDirectory`
* `IEnumerable<CloudBlockBlob>`
* `IEnumerable<CloudPageBlob>`
* Muuntyyppisten poistaa [ICloudBlobStreamBinder](../app-service-web/websites-dotnet-webjobs-sdk-storage-blobs-how-to.md#icbsb) mukaan 

#### <a name="blob-trigger-c-code-example"></a>Blob-objektien käynnistimen C# koodiesimerkki

C# koodin esimerkin kirjaa kunkin blob, joka on lisätty valvottu säilön sisältöä.

```csharp
public static void Run(string myBlob, TraceWriter log)
{
    log.Info($"C# Blob trigger function processed: {myBlob}");
}
```

#### <a name="blob-trigger-name-patterns"></a>Blob-objektien käynnistimen nimi kuviot

Voit määrittää-Blob-objektien nimi kuviota `path` ominaisuus. Esimerkki:

```json
"path": "input/original-{name}",
```

Tämä polku etsii nimeltä *Alkuperäisen Blob1.txt* *syötteen* säilö ja arvon blob `name` muuttuja-funktio on `Blob1`.

Toinen esimerkki:

```json
"path": "input/{blobname}.{blobextension}",
```

Tämä polku etsiä myös *Alkuperäisen Blob1.txt*ja arvon blob `blobname` ja `blobextension` koodista muuttujat on *Alkuperäinen Blob1* ja *txt*.

Voit rajoittaa BLOB-objektit, joka käynnistää toiminnon määrittämällä kiinteän arvona tiedostotunniste kuvion tyypit. Jos määrität `path` */esimerkit / {nimi} .png*, voit vain *.png* BLOB- *objektit* -säilössä käynnistää toiminnon.

Jos haluat Määritä Blob-objektien nimet, jotka on aaltosulkeiden nimi nimi-kaksoisnapsauttamalla aaltosulkeet. Jos haluat etsiä *kuvia* , joiden nimet tältä säilössä BLOB esimerkiksi:

        {20140101}-soundfile.mp3

Käytä tätä varten `path` ominaisuus:

        images/{{20140101}}-{name}

Esimerkissä `name` muuttujan arvo olisi *soundfile.mp3*. 

#### <a name="blob-receipts"></a>Blob-objektien kuittaukset

Azure-Funktiot runtime varmistetaan, että blob ei käynnistystoiminto kutsutaan useammin kuin kerran, samalla uusi tai päivitetty blob varten. Se tekee yllä *blob kuittaukset* jotta voit tarkistaa, onko tietyllä blob-versio on käsitelty.

Blob-objektien kuittaukset tallennetaan määrittämää AzureWebJobsStorage yhteysmerkkijonoa Azure-tallennustilan tilin *azure-webjobs-isännät* -säilö. Blob-kuittaus on seuraavat tiedot:

* Funktio, joka on kutsuttu Blob-objektien ("*{funktion sovelluksen nimi}*. Toiminnot. *{funktion nimen}*", esimerkiksi:"functionsf74b96f7. Functions.CopyBlob")
* Säilön nimi
* Blob-tyyppi ("BlockBlob" tai "PageBlob")
* Blob-nimi
* ETag (blob versio-tunniste, esimerkiksi: "0x8D1DC6E70A277EF")

Jos haluat aloittaa uudelleen, blob, voit poistaa kyseisen blob blob-kuittausta manuaalisesti *azure-webjobs-isäntien* säilöstä.

#### <a name="handling-poison-blobs"></a>Käsittely torjuttujen BLOB-objektit

Kun blob käynnistystoiminto epäonnistuu, SDK kutsuu sen uudelleen, siltä varalta, että virhe on aiheutunut lyhytkestoisia virhe. Jos virheen syynä on sisällön blob-funktio epäonnistuu aina, kun se yrittää käsitellä blob. Oletusarvon mukaan SDK kutsuu funktion enintään 5 luvulla annetun Blob-objektien. Jos viidennen yritä epäonnistuu, SDK Lisää viestiin nimeltä *webjobs blobtrigger-kosketusmyrkky*jonossa.

Torjuttujen BLOB jonon viestin on JSON-objekti, joka sisältää seuraavat ominaisuudet:

* FunctionId (-muoto *{funktion sovelluksen nimi}*. Toiminnot. *{funktion nimen}*)
* BlobType ("BlockBlob" tai "PageBlob")
* ContainerName
* BlobName
* ETag (blob versio-tunniste, esimerkiksi: "0x8D1DC6E70A277EF")

#### <a name="blob-polling-for-large-containers"></a>Kysely, suuri säilöjen BLOB

Jos blob-säilö, joka käynnistin seuranta on yli 10 000 BLOB-Funktiot runtime tarkistukset lokitiedostot kuuntelemaan uusia tai muutettuja BLOB-objektit. Tämä toimenpide ei ole reaaliaikainen; funktion ehkä ole käynnistetään useita minuutteja tai enää blob luomisen jälkeen. Lisäksi [lokit luodaan "parhaat tehokkuutta" tallennustilan](https://msdn.microsoft.com/library/azure/hh343262.aspx) perusteella; ei ei takaa, että kaikki tapahtumat tallennetaan. Tietyissä tilanteissa saattaa vastattu lokitiedot. Jos suuri säilöjen blob käynnistimet nopeuden ja luotettavuuden rajoituksista ei hyväksytä sovelluksen, suositeltava tapa on jonon viestin luominen, kun luot blob ja käsitellä blob sijaan blob käynnistimen jonon käynnistimen avulla.
 
## <a id="storageblobbindings"></a>Tallennustilan Azure-blob syötteen ja tulosteen sidontojen

#### <a name="functionjson-for-a-storage-blob-input-or-output-binding"></a>tallennustilan Blob-objektien Function.JSON syötteen tai tulosteen sidonta

*Function.json* tiedosto määrittää seuraavat ominaisuudet.

- `name`Muuttujan nimi: käyttää funktiota koodissa blob. 
- `path`: Polkua, joka määrittää säilö-Blob-objektien lukeminen tai tiedostoon, Blob-objektien ja halutessasi blob nimi määräytyy.
- `connection`: Asetus, joka sisältää tallennustilan yhteysmerkkijonon nimi. Jos jätät `connection` toimivat tyhjä, sidonta funktio-sovellus, joka on määritetty AzureWebJobsStorage sovelluksen määrittämällä yhteysmerkkijonon tallennustilan.
- `type`: On määritettävä *blob*.
- `direction`: Määritä *sisään* tai *ulos*. 

Esimerkki *function.json* tallennuksessa blob-syötteen tai tulosteen sidonta, kopioiminen blob jonon käynnistimen avulla:

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnection",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "myInputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}",
      "connection": "MyStorageConnection",
      "direction": "in"
    },
    {
      "name": "myOutputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}-Copy",
      "connection": "MyStorageConnection",
      "direction": "out"
    }
  ],
  "disabled": false
}
``` 

#### <a name="blob-input-and-output-supported-types"></a>BLOB syöttö- ja Tuetut tiedostotyypit

`blob` Sidonta voi muuntaa sarjaksi tai poistaa Node.js tai C#-funktioissa seuraavanlaisia:

* Objektin (`out T` C#, tuloste blob: Luo blob null objektina, jos parametriarvo on null, kun funktio päättyy)
* Merkkijono (`out string` C#, tuloste blob: Luo blob vain, jos Merkkijonoparametri on muut kuin null, kun funktio palauttaa)

C# Funktiot voit sitoa seuraavanlaisia:

* `TextReader`(vain syöte)
* `TextWriter`(vain tulos)
* `Stream`
* `CloudBlobStream`(vain tulos)
* `ICloudBlob`
* `CloudBlockBlob` 
* `CloudPageBlob` 

#### <a name="blob-output-c-code-example"></a>Blob-objektien tulosteen C# koodiesimerkki

C# koodin esimerkin kopioi blob, jonka nimi on vastaanotettu jonon viestin.

```csharp
public static void Run(string myQueueItem, string myInputBlob, out string myOutputBlob, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    myOutputBlob = myInputBlob;
}
```

## <a id="storagetablesbindings"></a>Azure tallennustilan taulukoiden syötteen ja tulosteen sidontojen

#### <a name="functionjson-for-storage-tables"></a>tallennustilan taulukoiden Function.JSON

*Function.json* määrittää seuraavat ominaisuudet.

- `name`Muuttujan nimi: käyttää funktiota taulukon sidontaa varten. 
- `tableName`: Taulukon nimi.
- `partitionKey`ja `rowKey` : käytetään yhdessä lukemaan yhden kohteen C# tai solmu-funktion tai kirjoittaa yhden kohteen solmu-funktiossa.
- `take`Lue lisää solmu-funktion taulukon rivien: enimmäismäärä.
- `filter`: Lisää solmu-funktion taulukon OData suodatinlauseke.
- `connection`: Asetus, joka sisältää tallennustilan yhteysmerkkijonon nimi. Jos jätät `connection` toimivat tyhjä, sidonta funktio-sovellus, joka on määritetty AzureWebJobsStorage sovelluksen määrittämällä yhteysmerkkijonon tallennustilan.
- `type`: On määritettävä *taulukkoon*.
- `direction`: Määritä *sisään* tai *ulos*. 

Seuraavassa esimerkissä *function.json* käyttää jonon käynnistimen lukemaan yhden taulukkorivi. JSON on koodattu osion avainarvon ja määrittää, että rivin tunnusta peräisin jonon viestin.

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnection",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "personEntity",
      "type": "table",
      "tableName": "Person",
      "partitionKey": "Test",
      "rowKey": "{queueTrigger}",
      "connection": "MyStorageConnection",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

#### <a name="storage-tables-input-and-output-supported-types"></a>Tallennustilan taulukoiden syötteen ja tulosteen tuetut tyypit

`table` Sidonta voi muuntaa sarjaksi tai poistaa objekteja Node.js tai C#-funktiota. Objektit on RowKey ja PartitionKey ominaisuudet. 

C# Funktiot voit sitoa seuraavanlaisia:

* `T`Jos `T` toteuttaa`ITableEntity`
* `IQueryable<T>`(vain syöte)
* `ICollector<T>`(vain tulos)
* `IAsyncCollector<T>`(vain tulos)

#### <a name="storage-tables-binding-scenarios"></a>Tallennustilan taulukot sidonta-skenaariot

Taulukon sidonta tukee seuraavia vaihtoehtoja:

* Lue yksirivinen C#- tai solmu-funktiossa.

    Määritä `partitionKey` ja `rowKey`. `filter` Ja `take` ominaisuuksia ei käytetä tässä skenaariossa.

* Lue useita rivejä C#-funktiossa.

    Funktiot runtime on `IQueryable<T>` sidottu objekti. Kirjoita `T` on oltava johdettu `TableEntity` tai toteuta `ITableEntity`. `partitionKey`, `rowKey`, `filter`, Ja `take` ominaisuuksia ei käytetä tässä skenaariossa; Voit käyttää `IQueryable` objekti, voit tehdä suodatusta pakollinen. 

* Lue useita rivejä solmu-funktiossa.

    Määritä `filter` ja `take` ominaisuudet. Et määritä `partitionKey` tai `rowKey`.

* Kirjoita rivejä C#-funktiota.

    Funktiot runtime on `ICollector<T>` tai `IAsyncCollector<T>` sidottu taulukkoon, jossa `T` määrittää kohteita, johon haluat lisätä rakenteen. Kirjoita yleensä `T` johdetaan `TableEntity` tai toteuttaa `ITableEntity`, mutta siinä ei ole. `partitionKey`, `rowKey`, `filter`, Ja `take` ominaisuuksia ei käytetä tässä skenaariossa.

#### <a name="storage-tables-example-read-a-single-table-entity-in-c-or-node"></a>Tallennustilan taulukoiden Esimerkki: lukea yhden taulukon kohteen, C# tai solmu

C# koodin esimerkissä toimii esitettyjä lukemaan yhden taulukon kohteen edellisen *function.json* -tiedosto. Jonon viestissä on rivin avainarvon ja taulukon kohde on luku, joka on määritetty *run.csx* tiedoston tyypiksi. Sisältää tyypin `PartitionKey` ja `RowKey` ominaisuudet ja ei ole johdettu `TableEntity`. 

```csharp
public static void Run(string myQueueItem, Person personEntity, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    log.Info($"Name in Person entity: {personEntity.Name}");
}

public class Person
{
    public string PartitionKey { get; set; }
    public string RowKey { get; set; }
    public string Name { get; set; }
}
```

Seuraava F # koodiesimerkki toimii myös edellisen *function.json* tiedoston lukeminen yhden taulukon kohde.

```fsharp
[<CLIMutable>]
type Person = {
  PartitionKey: string
  RowKey: string
  Name: string
}

let Run(myQueueItem: string, personEntity: Person) =
    log.Info(sprintf "F# Queue trigger function processed: %s" myQueueItem)
    log.Info(sprintf "Name in Person entity: %s" personEntity.Name)
```

Solmun koodi-esimerkki toimii myös edellisen *function.json* tiedoston lukeminen yhden taulukon kohde.

```javascript
module.exports = function (context, myQueueItem) {
    context.log('Node.js queue trigger function processed work item', myQueueItem);
    context.log('Person entity name: ' + context.bindings.personEntity.Name);
    context.done();
};
```

#### <a name="storage-tables-example-read-multiple-table-entities-in-c"></a>Tallennustilan taulukoiden Esimerkki: lukea usean taulukon kohteiden c# 

Seuraavat *function.json* ja C#-koodiin osion avainta, joka on määritetty jonon viestin Esimerkki lukee kohteet.

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnection",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "tableBinding",
      "type": "table",
      "connection": "MyStorageConnection",
      "tableName": "Person",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

C#-koodin Lisää viittauksen Azure-tallennustilan SDK niin, että kohteen tyyppi voivat saada `TableEntity`.

```csharp
#r "Microsoft.WindowsAzure.Storage"
using Microsoft.WindowsAzure.Storage.Table;

public static void Run(string myQueueItem, IQueryable<Person> tableBinding, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    foreach (Person person in tableBinding.Where(p => p.PartitionKey == myQueueItem).ToList())
    {
        log.Info($"Name: {person.Name}");
    }
}

public class Person : TableEntity
{
    public string Name { get; set; }
}
``` 

#### <a name="storage-tables-example-create-table-entities-in-c"></a>Tallennustilan taulukoiden Esimerkki: taulukon kohteiden luominen C# 

Seuraava esimerkki *function.json* ja *run.csx* näkyy viestin kirjoittaminen taulukon kohteiden C#.

```json
{
  "bindings": [
    {
      "name": "input",
      "type": "manualTrigger",
      "direction": "in"
    },
    {
      "tableName": "Person",
      "connection": "MyStorageConnection",
      "name": "tableBinding",
      "type": "table",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

```csharp
public static void Run(string input, ICollector<Person> tableBinding, TraceWriter log)
{
    for (int i = 1; i < 10; i++)
        {
            log.Info($"Adding Person entity {i}");
            tableBinding.Add(
                new Person() { 
                    PartitionKey = "Test", 
                    RowKey = i.ToString(), 
                    Name = "Name" + i.ToString() }
                );
        }

}

public class Person
{
    public string PartitionKey { get; set; }
    public string RowKey { get; set; }
    public string Name { get; set; }
}

```

#### <a name="storage-tables-example-create-table-entities-in-f"></a>Tallennustilan taulukoiden Esimerkki: F taulukon kohteiden luominen#

Seuraava esimerkki *function.json* ja *run.fsx* näkyy viestin kirjoittaminen taulukon kohteiden F #.

```json
{
  "bindings": [
    {
      "name": "input",
      "type": "manualTrigger",
      "direction": "in"
    },
    {
      "tableName": "Person",
      "connection": "MyStorageConnection",
      "name": "tableBinding",
      "type": "table",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

```fsharp
[<CLIMutable>]
type Person = {
  PartitionKey: string
  RowKey: string
  Name: string
}

let Run(input: string, tableBinding: ICollector<Person>, log: TraceWriter) =
    for i = 1 to 10 do
        log.Info(sprintf "Adding Person entity %d" i)
        tableBinding.Add(
            { PartitionKey = "Test"
              RowKey = i.ToString()
              Name = "Name" + i.ToString() })
```

#### <a name="storage-tables-example-create-a-table-entity-in-node"></a>Tallennustilan taulukoiden Esimerkki: luoda taulukon kohteen solmu

Seuraava esimerkki *function.json* ja *run.csx* näkyy viestin kirjoittaminen taulukko-kohteen solmu.

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnection",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "tableName": "Person",
      "partitionKey": "Test",
      "rowKey": "{queueTrigger}",
      "connection": "MyStorageConnection",
      "name": "personEntity",
      "type": "table",
      "direction": "out"
    }
  ],
  "disabled": true
}
```

```javascript
module.exports = function (context, myQueueItem) {
    context.log('Node.js queue trigger function processed work item', myQueueItem);
    context.bindings.personEntity = {"Name": "Name" + myQueueItem }
    context.done();
};
```

## <a name="next-steps"></a>Seuraavat vaiheet

[AZURE.INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)] 
