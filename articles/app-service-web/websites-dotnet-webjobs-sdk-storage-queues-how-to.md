<properties 
    pageTitle="Voit käyttää Azure jonon tallennustilan WebJobs SDK-paketissa" 
    description="Opettele käyttämään Azure jonon tallennustilan WebJobs SDK: N kanssa. Luo ja poista olevien; Lisää, vilkaista, Hae ja poista sanomien ja paljon muuta." 
    services="app-service\web, storage" 
    documentationCenter=".net" 
    authors="tdykstra" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="06/01/2016" 
    ms.author="tdykstra"/>

# <a name="how-to-use-azure-queue-storage-with-the-webjobs-sdk"></a>Voit käyttää Azure jonon tallennustilan WebJobs SDK-paketissa

## <a name="overview"></a>Yleiskatsaus

Tässä oppaassa on C# MALLIKOODEJA, joka näyttää, miten käyttävät Azure WebJobs SDK 1.x Azure jonon tallennustilan-palvelussa.

Oppaan oletetaan, että tiedät, [kuinka voit luoda WebJob projektin Visual Studiossa yhteyden merkkijonoja, jotka osoittavat tallennustilan tilin](websites-dotnet-webjobs-sdk-get-started.md#configure-storage) tai [useiden tallennustilan tilit](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/MultipleStorageAccountsEndToEndTests.cs).

Useimmat koodikatkelmat Näytä vain Funktiot, ei ole, luo koodi `JobHost` objektin, kuten tässä esimerkissä:

        static void Main(string[] args)
        {
            JobHost host = new JobHost();
            host.RunAndBlock();
        }
        
Oppaassa on seuraavissa ohjeaiheissa:

-   [Funktion käynnistämisestä jonon viestin saapuessa](#trigger)
    - Merkkijono sanomien
    - POCO sanomien
    - Asynkroninen Funktiot
    - QueueTrigger määrite toimii tyypit
    - Kysely-menetelmällä
    - Useita kertoja
    - Rinnakkaisia suorittaminen
    - Hae jonossa tai jonon viestin metatiedot
    - Kaksivaiheista sulkeminen
-   [Jonon viestin luominen jonon viestin käsiteltäessä](#createqueue)
    - Merkkijono sanomien
    - POCO sanomien
    - Luo useita viestejä tai asynkroninen Funktiot
    - Jonon määrite toimii tyypit
    - Käytä funktiota tekstissä WebJobs SDK määritteet
-   [Miten lukemiseen ja kirjoittamiseen BLOB jonon viestin käsiteltäessä](#blobs)
    - Merkkijono sanomien
    - POCO sanomien
    - Blob-määrite toimii tyypit
-   [Kokoukseen sanomien käsitteleminen](#poison)
    - Torjuttujen viestien automaattinen käsittely
    - Manuaalinen torjuttujen viestin käsittely
-   [Kokoonpanoasetusten määrittäminen](#config)
    - Määritä SDK yhteyden merkkijonot koodi
    - QueueTrigger asetusten määrittäminen
    - Määritä arvot WebJobs SDK konstruktori parametrit-koodi
-   [Käynnistämisestä funktio manuaalisesti](#manual)
-   [Viestin kirjoittaminen lokit](#logs) 
-   [Miten voit käsitellä virheitä ja määrittää aikakatkaisu](#errors)
-   [Seuraavat vaiheet](#nextsteps)

## <a id="trigger"></a>Funktion käynnistämisestä jonon viestin saapuessa

Jos haluat kirjoittaa funktion, joka WebJobs SDK kutsuu jonon viestin saapuessa, käytä `QueueTrigger` määrite. Määritteen konstruktoria kestää parametrin, joka määrittää jonon lisääminen nimi. Voit myös [määrittää jonon nimen dynaamisesti](#config).

### <a name="string-queue-messages"></a>Merkkijono sanomien

Seuraavassa esimerkissä jonossa sisältää merkkijonon, viestin niin `QueueTrigger` otetaan käyttöön kyselymerkkijonon parametrin `logMessage` sisältää jonon viestin sisältöä. -Funktion [kirjoittaa log viestin koontinäyttö](#logs).
 

        public static void ProcessQueueMessage([QueueTrigger("logqueue")] string logMessage, TextWriter logger)
        {
            logger.WriteLine(logMessage);
        }

Paitsi useimpien yritysten `string`-parametri voi olla DBCS-matriisi `CloudQueueMessage` objekti tai POCO, joka määritetään.

### <a name="poco-plain-old-clr-objecthttpenwikipediaorgwikiplainoldclrobject-queue-messages"></a>POCO [(Normaali vanha CLR-objekti](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) jonon viestit

Seuraavassa esimerkissä jonon viesti sisältää JSON varten `BlobInformation` objekti, joka sisältää `BlobName` ominaisuus. SDK deserializes automaattisesti objekti.

        public static void WriteLogPOCO([QueueTrigger("logqueue")] BlobInformation blobInfo, TextWriter logger)
        {
            logger.WriteLine("Queue message refers to blob: " + blobInfo.BlobName);
        }

SDK käyttää [Newtonsoft.Json NuGet paketin](http://www.nuget.org/packages/Newtonsoft.Json) onnistu ja poistaa viestit. Jos luot sanomien ohjelmassa, joka ei käytä WebJobs SDK-paketissa, voit kirjoittaa koodin, kuten seuraavassa esimerkissä POCO jonossa viestin, joka on SDK: ssa voidaan jäsentää luominen. 

        BlobInformation blobInfo = new BlobInformation() { BlobName = "log.txt" };
        var queueMessage = new CloudQueueMessage(JsonConvert.SerializeObject(blobInfo));
        logQueue.AddMessage(queueMessage);

### <a name="async-functions"></a>Asynkroninen Funktiot

Seuraavat asynkroninen funktion [kirjoittaa lokia koontinäyttö](#logs).

        public async static Task ProcessQueueMessageAsync([QueueTrigger("logqueue")] string logMessage, TextWriter logger)
        {
            await logger.WriteLineAsync(logMessage);
        }

Asynkroninen Funktiot saattaa kestää [peruutus-tunnuksen](http://www.asp.net/mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4#CancelToken), joka kopioi blob seuraavan esimerkin mukaisesti. (Kuvaus `queueTrigger` paikkamerkki- [BLOB](#blobs) -osiossa.)

        public async static Task ProcessQueueMessageAsyncCancellationToken(
            [QueueTrigger("blobcopyqueue")] string blobName, 
            [Blob("textblobs/{queueTrigger}",FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{queueTrigger}-new",FileAccess.Write)] Stream blobOutput,
            CancellationToken token)
        {
            await blobInput.CopyToAsync(blobOutput, 4096, token);
        }

### <a id="qtattributetypes"></a>QueueTrigger määrite toimii tyypit

Voit käyttää `QueueTrigger` seuraavien kanssa:

* `string`
* POCO kuin JSON tyyppi
* `byte[]`
* `CloudQueueMessage`

### <a id="polling"></a>Kysely-menetelmällä

SDK toteuttaa satunnainen eksponentiaalisen Edellinen käytöstä algoritmin vähentää vapaa-jonossa kyselyt tallennustilan kustannukset-tehoste.  Kun viesti löytyy, SDK odottaa kahden sekunnin ajan ja sen jälkeen tarkistaa toisen viestin; Kun sanomaa ei löydy Odota noin neljän sekunnin ajan ennen kuin yrität uudelleen. Epäonnistui Symbol jonon-ilmoitus, kun odotusaika säilyy niin, että, kunnes ohjausobjekti suurin odotusaika, jonka oletusarvo on minuutin kuluttua. [Suurin odotusaika on määritettävä](#config).

### <a id="instances"></a>Useita kertoja

Jos web-sovellus toimii eri esiintymissä, jatkuva WebJob suoritetaan jokaiseen tietokoneeseen ja Jokaisessa koneessa käynnistimien Odota ja yritä suorittaa toimintoja. WebJobs SDK jonon käynnistin estää funktion automaattisesti käsittelemästä jonon viestin useita kertoja; funktio ei ole kirjoitetaan idempotent. Jos haluat varmistaa, että vain yhden esiintymän kuitenkin funktio toimii myös silloin, kun on useita host web-sovellusten esiintymiä, voit käyttää `Singleton` määrite. 

### <a id="parallel"></a>Rinnakkaisia suorittaminen

Jos sinulla on useita eri olevien Kuuntele funktioita, SDK soittavat ne rinnakkain, kun viestejä vastaanotetaan samanaikaisesti. 

Sama koskee, kun useita viestejä vastaanotetaan yhden jonon. Oletusarvon mukaan SDK saa 16 sanomien erän kerrallaan ja käynnistää haluamasi funktio käsittelee ne rinnakkain. [Erän koko on määritettävä](#config). Kun käsitellään numero syötetään puolen erän koko, SDK saa muuta erää ja aloittaa käsittelyn viestit. Tämän vuoksi samanaikainen viestit käsitellään funktiota kohden enimmäismäärä on yksi ja puoli kertaa erän koko. Tämä rajoitus koskee erikseen jokaiselle funktio, joka on `QueueTrigger` määrite. 

Jos et halua rinnakkain suorittamisen yksi jono vastaanotetut viestit, voit määrittää erän koko 1. Katso myös [Azure WebJobs SDK 1.1.0 RTM](/blog/azure-webjobs-sdk-1-1-0-rtm/) **käsittely tarkemmin** .

### <a id="queuemetadata"></a>Hae jonossa tai jonon viestin metatiedot

Voit hankkia lisäämällä parametreja menetelmän allekirjoitus viestin seuraavat ominaisuudet:

* `DateTimeOffset`expirationTime
* `DateTimeOffset`insertionTime
* `DateTimeOffset`nextVisibleTime
* `string`queueTrigger (sisältää viestiteksti)
* `string`tunnus
* `string`popReceipt
* `int`dequeueCount

Jos haluat käsitellä Azure tallennustilan API, voit lisätä `CloudStorageAccount` parametria.

Seuraavassa esimerkissä kirjoitetaan kaikki metatiedot INFO-sovelluksen lokiin. Esimerkissä logMessage ja queueTrigger sisältää jonon viestin sisältöä.

        public static void WriteLog([QueueTrigger("logqueue")] string logMessage,
            DateTimeOffset expirationTime,
            DateTimeOffset insertionTime,
            DateTimeOffset nextVisibleTime,
            string id,
            string popReceipt,
            int dequeueCount,
            string queueTrigger,
            CloudStorageAccount cloudStorageAccount,
            TextWriter logger)
        {
            logger.WriteLine(
                "logMessage={0}\n" +
            "expirationTime={1}\ninsertionTime={2}\n" +
                "nextVisibleTime={3}\n" +
                "id={4}\npopReceipt={5}\ndequeueCount={6}\n" +
                "queue endpoint={7} queueTrigger={8}",
                logMessage, expirationTime,
                insertionTime,
                nextVisibleTime, id,
                popReceipt, dequeueCount,
                cloudStorageAccount.QueueEndpoint,
                queueTrigger);
        }

Tässä on esimerkki lokia sample code kirjoittama:

        logMessage=Hello world!
        expirationTime=10/14/2014 10:31:04 PM +00:00
        insertionTime=10/7/2014 10:31:04 PM +00:00
        nextVisibleTime=10/7/2014 10:41:23 PM +00:00
        id=262e49cd-26d3-4303-ae88-33baf8796d91
        popReceipt=AgAAAAMAAAAAAAAAfc9H0n/izwE=
        dequeueCount=1
        queue endpoint=https://contosoads.queue.core.windows.net/
        queueTrigger=Hello world!

### <a id="graceful"></a>Kaksivaiheista sulkeminen

Funktion, joka suoritetaan jatkuva WebJob hyväksyä `CancellationToken` parametri, joka mahdollistaa käyttöjärjestelmän funktio ilmoittaa, kun WebJob on lopetetaan. Tämä ilmoitus avulla voit varmistaa, funktio ei lopeta odottamatta niin, että jättää tiedot epäyhtenäiseen tilaan.

Seuraavassa esimerkissä esitetään tarkistaminen pian funktion WebJob lopettamista varten.

    public static void GracefulShutdownDemo(
                [QueueTrigger("inputqueue")] string inputText,
                TextWriter logger,
                CancellationToken token)
    {
        for (int i = 0; i < 100; i++)
        {
            if (token.IsCancellationRequested)
            {
                logger.WriteLine("Function was cancelled at iteration {0}", i);
                break;
            }
            Thread.Sleep(1000);
            logger.WriteLine("Normal processing for queue message={0}", inputText);
        }
    }

**Huomautus:** Koontinäytön ehkä ole oikein Näytä tila ja tulosteen toiminnot, jotka on suljettu.
 
Lisätietoja on artikkelissa [WebJobs kaksivaiheista Sammuta](http://blog.amitapple.com/post/2014/05/webjobs-graceful-shutdown/#.VCt1GXl0wpR).   

## <a id="createqueue"></a>Jonon viestin luominen jonon viestin käsiteltäessä

Jos haluat kirjoittaa funktion, joka luo uuden jonon viestin, käytä `Queue` määrite. Kuten `QueueTrigger`, voit välittää jonon nimen merkkijonona tai voit [määrittää jonon nimen dynaamisesti](#config).

### <a name="string-queue-messages"></a>Merkkijono sanomien

Asynkroninen koodi-näyte Luo uuden jonon viestin jonossa nimeltä "outputqueue", jolla on sama sisältö vastaanotetut nimeltä "inputqueue" jonossa jonon viestinä. (Asynkroninen funktioiden käyttäminen `IAsyncCollector<T>` tässä osassa myöhemmin esitetyllä tavalla.)


        public static void CreateQueueMessage(
            [QueueTrigger("inputqueue")] string queueMessage,
            [Queue("outputqueue")] out string outputQueueMessage )
        {
            outputQueueMessage = queueMessage;
        }
  
### <a name="poco-plain-old-clr-objecthttpenwikipediaorgwikiplainoldclrobject-queue-messages"></a>POCO [(Normaali vanha CLR-objekti](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) jonon viestit

Välittää jonon viestin, joka sisältää merkkijonon sijaan POCO luominen POCO tyyppi kuin tulostusparametri `Queue` Määritekonstruktorilla.
 
        public static void CreateQueueMessage(
            [QueueTrigger("inputqueue")] BlobInformation blobInfoInput,
            [Queue("outputqueue")] out BlobInformation blobInfoOutput )
        {
            blobInfoOutput = blobInfoInput;
        }

SDK serializes automaattisesti JSON objekti. Jonon viestin luodaan aina, vaikka objekti on null.

### <a name="create-multiple-messages-or-in-async-functions"></a>Luo useita viestejä tai asynkroninen Funktiot

Jos haluat luoda useita viestejä, tehdä tulosteen jonossa parametrin tyypin `ICollector<T>` tai `IAsyncCollector<T>`, kuten seuraavassa esimerkissä.

        public static void CreateQueueMessages(
            [QueueTrigger("inputqueue")] string queueMessage,
            [Queue("outputqueue")] ICollector<string> outputQueueMessage,
            TextWriter logger)
        {
            logger.WriteLine("Creating 2 messages in outputqueue");
            outputQueueMessage.Add(queueMessage + "1");
            outputQueueMessage.Add(queueMessage + "2");
        }

Jonon jokaisen viestin luodaan heti kun `Add` menetelmää kutsutaan.

### <a name="types-that-the-queue-attribute-works-with"></a>Tiedostotyypit, joka toimii jonon-määrite

Voit käyttää `Queue` parametrin seuraavanlaisia määrite:

* `out string`(Luo jonon viesti, jos parametriarvo on muu kuin null, kun funktio päättyy)
* `out byte[]`(works, kuten `string`) 
* `out CloudQueueMessage`(works, kuten `string`) 
* `out POCO`(liittymätyypillä, Luo viestin null-objektin ja jos parametri on tyhjä, kun funktio päättyy)
* `ICollector`
* `IAsyncCollector`
* `CloudQueue`(luomisen viestiä, joissa manuaalisesti suoraan Azure-tallennustilan API)

### <a id="ibinder"></a>Käytä funktiota tekstissä WebJobs SDK määritteet

Jos sinun täytyy suorittaa joitakin työn funktion ennen käyttää WebJobs SDK-määrite, kuten `Queue`, `Blob`, tai `Table`, voit käyttää `IBinder` käyttöliittymän.

Seuraavassa esimerkissä tulee syötteen jonon viestin ja luo uusi viesti, jossa tulostus-jonossa samaa sisältöä. Tulosteen jonon nimi on määritetty koodilla funktion tekstiosaan.

        public static void CreateQueueMessage(
            [QueueTrigger("inputqueue")] string queueMessage,
            IBinder binder)
        {
            string outputQueueName = "outputqueue" + DateTime.Now.Month.ToString();
            QueueAttribute queueAttribute = new QueueAttribute(outputQueueName);
            CloudQueue outputQueue = binder.Bind<CloudQueue>(queueAttribute);
            outputQueue.AddMessage(new CloudQueueMessage(queueMessage));
        }

`IBinder` Käyttöliittymän voidaan käyttää myös `Table` ja `Blob` määritteet.

## <a id="blobs"></a>Miten lukemiseen ja kirjoittamiseen BLOB-objektit ja taulukot jonon viestin käsiteltäessä

`Blob` Ja `Table` määritteet mahdollistavat lukemiseen ja kirjoittamiseen BLOB-objektit ja taulukot. Tässä osassa mallit koskevat BLOB-objektit. Katso, [miten voit käyttää Azure-blob-säiliö ja WebJobs SDK](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md)MALLIKOODEJA, jotka näyttävät käynnistämisestä prosessit BLOB-objektit luodaan tai päivitetään, ja MALLIKOODEJA, lukeminen ja kirjoittaminen taulukot, tutustu [käyttämään Azure-taulukkotallennus WebJobs SDK: N kanssa](websites-dotnet-webjobs-sdk-storage-tables-how-to.md).

### <a name="string-queue-messages-triggering-blob-operations"></a>Merkkijono sanomien käynnistävä blob-toiminnot

Jonon viestiin, joka sisältää merkkijonon `queueTrigger` on käytettävissä olevat paikkamerkki `Blob` -määritteen `blobPath` parametri, joka sisältää viestin sisältöä. 

Seuraavassa esimerkissä `Stream` lukemiseen ja kirjoittamiseen Blob-objektien. Jonon viesti on textblobs säilö sijaitsee Blob-objektien nimi. Kopion Blob-objektien kanssa "-uusi" liitetään nimi luodaan samaan säilö. 

        public static void ProcessQueueMessage(
            [QueueTrigger("blobcopyqueue")] string blobName, 
            [Blob("textblobs/{queueTrigger}",FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{queueTrigger}-new",FileAccess.Write)] Stream blobOutput)
        {
            blobInput.CopyTo(blobOutput, 4096);
        }

`Blob` Määritteen konstruktorin tulevat `blobPath` parametri, joka määrittää säilö ja blob-nimi. Saat lisätietoja tämän paikkamerkin [käyttämisestä Azure-blob-säiliö WebJobs SDK: N kanssa](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md) 

Kun määrite decorates `Stream` objektin toiseen konstruktorin parametri ilmaisee `FileAccess` tilassa luku, kirjoita tai luku-/ kirjoitusoikeudet. 

Seuraavassa esimerkissä `CloudBlockBlob` blob poistettavan objektin. Jonon viesti on blob nimi.

        public static void DeleteBlob(
            [QueueTrigger("deleteblobqueue")] string blobName,
            [Blob("textblobs/{queueTrigger}")] CloudBlockBlob blobToDelete)
        {
            blobToDelete.Delete();
        }

### <a id="pocoblobs"></a>POCO [(Normaali vanha CLR-objekti](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) jonon viestit

POCO, tallennettujen JSON jonon viestissä, voit käyttää paikkamerkit, joita nimen objektin ominaisuudet `Queue` -määritteen `blobPath` parametria. Voit käyttää myös [jonon metatietojen ominaisuuksien nimiä](#queuemetadata) paikkamerkkeinä. 

Seuraavassa esimerkissä kopioi blob uusi blob tunnistetta. Jonon viesti on `BlobInformation` objekti, joka sisältää `BlobName` ja `BlobNameWithoutExtension` ominaisuudet. Ominaisuutta käytetään paikkamerkkeinä blob polku `Blob` määritteet. 
 
        public static void CopyBlobPOCO(
            [QueueTrigger("copyblobqueue")] BlobInformation blobInfo,
            [Blob("textblobs/{BlobName}", FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{BlobNameWithoutExtension}.txt", FileAccess.Write)] Stream blobOutput)
        {
            blobInput.CopyTo(blobOutput, 4096);
        }

SDK käyttää [Newtonsoft.Json NuGet paketin](http://www.nuget.org/packages/Newtonsoft.Json) onnistu ja poistaa viestit. Jos luot sanomien ohjelmassa, joka ei käytä WebJobs SDK-paketissa, voit kirjoittaa koodin, kuten seuraavassa esimerkissä POCO jonossa viestin, joka on SDK: ssa voidaan jäsentää luominen.

        BlobInformation blobInfo = new BlobInformation() { BlobName = "boot.log", BlobNameWithoutExtension = "boot" };
        var queueMessage = new CloudQueueMessage(JsonConvert.SerializeObject(blobInfo));
        logQueue.AddMessage(queueMessage);

Jos haluat tehdä joitakin työt-funktion ennen blob sitominen objektin, voit käyttää määrite funktion [aiemmin jonon-määrite, kuten](#ibinder)tekstiosaan.

### <a id="blobattributetypes"></a>Voit käyttää Blob-määrite, jonka tyypit
 
`Blob` Määrite voidaan käyttää seuraavia:

* `Stream`(Lue ja kirjoita, määritetty FileAccess konstruktori parametrilla)
* `TextReader`
* `TextWriter`
* `string`(luku)
* `out string`(Kirjoita; Luo blob vain, jos Merkkijonoparametri on muut kuin null, kun funktio palauttaa)
* POCO (luku)
* ulos POCO (Kirjoita; aina Luo blob, Luo tyhjä objektina, jos POCO-parametri on nolla, kun funktio palauttaa)
* `CloudBlobStream`(Kirjoita)
* `ICloudBlob`(luku- tai kirjoitusoikeudet)
* `CloudBlockBlob`(luku- tai kirjoitusoikeudet) 
* `CloudPageBlob`(luku- tai kirjoitusoikeudet) 

## <a id="poison"></a>Kokoukseen sanomien käsitteleminen

Viestit, joiden sisältö aiheuttaa funktio voi epäonnistua kutsutaan *sanomien*. Kun funktio epäonnistuu, jonossa viesti ei poisteta ja tekstin on noudettu uudelleen aiheuttaa toistettava jakson. SDK automaattisesti voit keskeyttää jakson jälkeen Iteraatioita rajoitettu määrä tai voit tehdä sen manuaalisesti.

### <a name="automatic-poison-message-handling"></a>Torjuttujen viestien automaattinen käsittely

SDK soittaa funktion enintään 5 luvulla käsittelemään jonossa viestin. Jos viidennen yritä epäonnistuu, viesti siirretään torjuttujen jonossa. [Uudelleenyritykset enimmäismäärä on määritettävä](#config). 

Torjuttujen jonossa nimeltä *{originalqueuename}*-torjuttujen. Voit kirjoittaa funktion prosessin viesteihin torjuttujen jonossa kirjaaminen ne tai lähettämällä ilmoituksen, että manuaalinen huomiota tarvita. 

Seuraavassa esimerkissä `CopyBlob` funktion epäonnistuu, kun jonossa viesti sisältää Blob-objektien, jota ei ole nimi. Tällöin viesti siirretään copyblobqueue jonossa copyblobqueue kosketusmyrkky jonossa. `ProcessPoisonMessage` Kirjaa torjuttujen viestin.

        public static void CopyBlob(
            [QueueTrigger("copyblobqueue")] string blobName,
            [Blob("textblobs/{queueTrigger}", FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{queueTrigger}-new", FileAccess.Write)] Stream blobOutput)
        {
            blobInput.CopyTo(blobOutput, 4096);
        }
        
        public static void ProcessPoisonMessage(
            [QueueTrigger("copyblobqueue-poison")] string blobName, TextWriter logger)
        {
            logger.WriteLine("Failed to copy blob, name=" + blobName);
        }

Seuraavassa kuvassa konsolin tulosteen näiden funktioiden torjuttujen viestin käsittelyn yhteydessä.

![Konsolin tulosteen torjuttujen viestin käsittely](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/poison.png)

### <a name="manual-poison-message-handling"></a>Manuaalinen torjuttujen viestin käsittely

Saat kuinka monta kertaa, viestiin on noudettu käsittelyä varten lisäämällä `int` parametri `dequeueCount` funktion avulla. Voit tarkistaa dequeue Laske-funktio ja suorita oman torjuttujen viestin käsittely kun määrä on suurempi kuin kynnysarvo, kuten seuraavassa esimerkissä.

        public static void CopyBlob(
            [QueueTrigger("copyblobqueue")] string blobName, int dequeueCount,
            [Blob("textblobs/{queueTrigger}", FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{queueTrigger}-new", FileAccess.Write)] Stream blobOutput,
            TextWriter logger)
        {
            if (dequeueCount > 3)
            {
                logger.WriteLine("Failed to copy blob, name=" + blobName);
            }
            else
            {
            blobInput.CopyTo(blobOutput, 4096);
            }
        }

## <a id="config"></a>Kokoonpanoasetusten määrittäminen

Voit käyttää `JobHostConfiguration` lajin seuraavat asetukset:

* Määrittää SDK yhteyden merkkijonot koodi.
* Määritä `QueueTrigger` asetuksia, kuten enintään jonosta poistamisen määrä.
* Pyydä jonon nimet määritys.

### <a id="setconnstr"></a>Määritä SDK yhteyden merkkijonot koodi

Määrittäminen SDK yhteyden merkkijonot koodin avulla voit käyttää omia yhteyden merkkijonon nimiä tiedostojen tai ympäristömuuttujat seuraavan esimerkin mukaisesti.

        static void Main(string[] args)
        {
            var _storageConn = ConfigurationManager
                .ConnectionStrings["MyStorageConnection"].ConnectionString;
        
            var _dashboardConn = ConfigurationManager
                .ConnectionStrings["MyDashboardConnection"].ConnectionString;
        
            var _serviceBusConn = ConfigurationManager
                .ConnectionStrings["MyServiceBusConnection"].ConnectionString;
        
            JobHostConfiguration config = new JobHostConfiguration();
            config.StorageConnectionString = _storageConn;
            config.DashboardConnectionString = _dashboardConn;
            config.ServiceBusConnectionString = _serviceBusConn;
            JobHost host = new JobHost(config);
            host.RunAndBlock();
        }

### <a id="configqueue"></a>QueueTrigger asetusten määrittäminen

Voit määrittää seuraavat asetukset, jotka koskevat jonon viestin käsittely:

- On noudettu samanaikaisesti, joka suoritetaan rinnakkain niiden sanomien sallittu enimmäismäärä (oletusarvo on 16).
- Enimmäismäärä ennen jonon viesti on lähetetty torjuttujen jonon uudelleenyritykset (oletusarvo on 5).
- Suurin odotusaika ennen kysely uudelleen, kun jono on tyhjä (oletusarvo on 1 minuutti).

Seuraavassa esimerkissä esitetään, miten voit määrittää nämä asetukset:

        static void Main(string[] args)
        {
            JobHostConfiguration config = new JobHostConfiguration();
            config.Queues.BatchSize = 8;
            config.Queues.MaxDequeueCount = 4;
            config.Queues.MaxPollingInterval = TimeSpan.FromSeconds(15);
            JobHost host = new JobHost(config);
            host.RunAndBlock();
        }

### <a id="setnamesincode"></a>Määritä arvot WebJobs SDK konstruktori parametrit-koodi

Joskus haluat määrittää jonon nimi, blob nimi tai säilö tai taulukon nimen koodi koodata eikä se. Esimerkiksi voit määrittää jonon nimi `QueueTrigger` määritysten tiedoston tai ympäristön muuttujana. 

Voit tehdä, lähi- `NameResolver` objektia `JobHostConfiguration` tyyppi. Erityistä paikkamerkit uhkaavat WebJobs SDK määritteen konstruktorin parametrien etumerkki prosenttia (%) Sisällytä ja `NameResolver` koodi määrittää todellisten arvojen näiden paikkamerkit sijasta.

Oletetaan esimerkiksi, jota haluat käyttää logqueuetest testiympäristössä ja yksi nimetty logqueueprod tuotannon jonossa. Sen sijaan, että koodattu jonossa, nimi, jonka haluat täydennys nimi `appSettings` kokoelma, joka on todellinen jonon nimi. Jos `appSettings` avain on logqueue, funktion näyttää samalta kuin seuraavassa esimerkissä.

        public static void WriteLog([QueueTrigger("%logqueue%")] string logMessage)
        {
            Console.WriteLine(logMessage);
        }

Oman `NameResolver` luokan sitten saatu jonon nimen `appSettings` seuraavan esimerkin mukaisesti:

        public class QueueNameResolver : INameResolver
        {
            public string Resolve(string name)
            {
                return ConfigurationManager.AppSettings[name].ToString();
            }
        }

Voit siirtää `NameResolver` luokka `JobHost` objektin, kuten seuraavassa esimerkissä.

        static void Main(string[] args)
        {
            JobHostConfiguration config = new JobHostConfiguration();
            config.NameResolver = new QueueNameResolver();
            JobHost host = new JobHost(config);
            host.RunAndBlock();
        }
 
**Huomautus:** Jonossa, taulukon ja Blob-objektien nimet selvitetään aina, kun funktio kutsutaan, mutta blob-säilö nimet selvitetään vain, kun sovellus käynnistyy. Blob-säilö nimeä ei voi muuttaa työn ollessa käynnissä. 

## <a id="manual"></a>Käynnistämisestä funktio manuaalisesti

Käynnistettävän funktion manuaalisesti, käytä `Call` tai `CallAsync` -menetelmä `JobHost` objekti ja `NoAutomaticTrigger` määrite-funktion, kuten seuraavassa esimerkissä. 

        public class Program
        {
            static void Main(string[] args)
            {
                JobHost host = new JobHost();
                host.Call(typeof(Program).GetMethod("CreateQueueMessage"), new { value = "Hello world!" });
            }
        
            [NoAutomaticTrigger]
            public static void CreateQueueMessage(
                TextWriter logger, 
                string value, 
                [Queue("outputqueue")] out string message)
            {
                message = value;
                logger.WriteLine("Creating queue message: ", message);
            }
        }

## <a id="logs"></a>Viestin kirjoittaminen lokit

Koontinäytön näkyy lokit kahdessa paikassa: sivulle, jossa WebJob ja tietyn WebJob kutsu sivu. 

![Lokien WebJob sivu](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/dashboardapplogs.png)

![Funktion kutsu sivun lokit](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/dashboardlogs.png)

Tulosteen konsolin menetelmiä, joita voit soittaa funktion- tai siinä `Main()` menetelmä tulee näkyviin WebJob raporttinäkymä-sivulle, ei tietyn menetelmän käynnistyksessä sivu. Tulosteen TextWriter objektia, jonka saat parametrin menetelmä allekirjoitukseen näkyy menetelmän käynnistyksessä raporttinäkymä-sivulle.

Konsolin tuloste ei voi linkittää tiettyyn kutsun, koska konsolin on yksi threaded monta työtehtävien on käynnissä yhtä aikaa. Minkä vuoksi SDK sisältää jokaisen funktion kutsu yksilöllinen log writer-objektin mukana.

Voit kirjoittaa [sovelluksen jäljityksen lokitiedostot](web-sites-dotnet-troubleshoot-visual-studio.md#logsoverview) `Console.Out` (Luo lokit tiedot merkitty) ja `Console.Error` (Luo lokit Merkitse virheeksi). Löytämiseen on käyttää [jäljitys tai TraceSource](http://blogs.msdn.com/b/mcsuksoldev/archive/2014/09/04/adding-trace-to-azure-web-sites-and-web-jobs.aspx), joka sisältää lisäksi tiedot ja virheen yksityiskohtainen varoitus ja kriittinen tasot. Sovelluksen jäljityksen lokitiedostot näkyvät web app kirjautuvat lokitiedostoihin, Azure-taulukoiden tai Azure-BLOB sen mukaan, miten voit määrittää Azure koodiin. On TOSI, jos kaikki konsolin tulosteen, viimeisimmän 100 sovelluksen lokit näkyvät myös raporttinäkymä-sivulle WebJob eikä funktion kutsu sivua varten. 

Konsolin tulos näkyy vain, jos ohjelma on käynnissä Azure-WebJob, ei, jos ohjelma on käynnissä paikallisesti Raporttinäkymät-ikkunan tai muussa ympäristössä.

Poista käytöstä Raporttinäkymät-ikkunan kirjaus käyttöön high siirtonopeuden skenaarioita. Oletusarvon mukaan SDK kirjoittaa lokit tallennustilan ja tässä aktiviteetti voi heikentää suorituskykyä viestejä paljon käsiteltäessä. Poista kirjaaminen käytöstä määrittämällä Raporttinäkymät-ikkunan yhteysmerkkijono Null, kuten seuraavassa esimerkissä.

        JobHostConfiguration config = new JobHostConfiguration();       
        config.DashboardConnectionString = "";        
        JobHost host = new JobHost(config);
        host.RunAndBlock();

Seuraavassa esimerkissä esitetään, kirjoittaa lokit usealla eri tavalla:

        public static void WriteLog(
            [QueueTrigger("logqueue")] string logMessage,
            TextWriter logger)
        {
            Console.WriteLine("Console.Write - " + logMessage);
            Console.Out.WriteLine("Console.Out - " + logMessage);
            Console.Error.WriteLine("Console.Error - " + logMessage);
            logger.WriteLine("TextWriter - " + logMessage);
        }

WebJobs SDK koontinäytön, tuloste `TextWriter` objekti näkyy kun voit avata tietyn sivun kutsu-funktiota, ja valitse **Näytä tai piilota tulos**:

![Valitse funktio-kutsun linkki](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/dashboardinvocations.png)

![Funktion kutsu sivun lokit](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/dashboardlogs.png)

WebJobs SDK-koontinäytön viimeisimmän 100 riviä konsolin tulosteen Näytä ylös, siirry sivulle, WebJob (ei käytössä toiminnon käynnistäminen) ja valitse **Näytä tai piilota tulos**.
 
![Valitse Näytä tai piilota-tulostus](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/dashboardapplogs.png)

Jatkuva WebJob-sovelluksen lokit näkyvät/tietojen/työt/jatkuva /*{webjobname}*/job_log.txt web app-tiedostojärjestelmässä.

        [09/26/2014 21:01:13 > 491e54: INFO] Console.Write - Hello world!
        [09/26/2014 21:01:13 > 491e54: ERR ] Console.Error - Hello world!
        [09/26/2014 21:01:13 > 491e54: INFO] Console.Out - Hello world!

Azure-blob-sovelluksen lokit näyttävät tältä: 2014-09-26T21:01:13,Information,contosoadsnew,491e54,635473620738373502,0,17404,17,Console.Write - Hei maailma!, 2014-09-26T21:01:13,Error,contosoadsnew,491e54,635473620738373502,0,17404,19,Console.Error - Hei!, 2014-09-26T21:01:13,Information,contosoadsnew,491e54,635473620738529920,0,17404,17,Console.Out - Hei maailma!,

Ja Azure-taulukon `Console.Out` ja `Console.Error` lokit näyttää tältä:

![Taulukon tiedot kirjautuminen](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/tableinfo.png)

![Taulukon virheloki](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/tableerror.png)

Jos haluat laajennus oman lokin, katso [Tässä esimerkissä](http://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/MiscOperations/Program.cs).

## <a id="errors"></a>Miten voit käsitellä virheitä ja määrittää aikakatkaisu

WebJobs SDK sisältää myös [aikakatkaisu](http://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/MiscOperations/Functions.cs) , joiden avulla voit saada funktioon peruutetaan, jos määrite ei täytä tietyn ajanjakson ajan kuluessa. Jos haluat korottaa ilmoituksen, kun liikaa virheitä tapahtuu määritetyn ajanjakson aikana, voit käyttää ja `ErrorTrigger` määrite. Tässä on [Esimerkki ErrorTrigger](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Error-Monitoring).

```
public static void ErrorMonitor(
[ErrorTrigger("00:01:00", 1)] TraceFilter filter, TextWriter log,
[SendGrid(
    To = "admin@emailaddress.com",
    Subject = "Error!")]
 SendGridMessage message)
{
    // log last 5 detailed errors to the Dashboard
   log.WriteLine(filter.GetDetailedMessage(5));
   message.Text = filter.GetDetailedMessage(1);
}
```

Voit myös dynaamisesti käytöstä ja ottaa käyttöön funktiot ohjausobjektiin, onko ne voidaan käynnistää, käyttämällä määritysten valitsin, joka voi olla asetus tai ympäristön muuttujan nimi. Sample code-kohdassa `Disable` [WebJobs SDK näytteiden](https://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/MiscOperations/Functions.cs)säilössä määrite.

## <a id="nextsteps"></a>Seuraavat vaiheet

Tässä oppaassa on tarjonnut MALLIKOODEJA, jotka esittävät yleisiä tilanteita, joissa käyttäminen Azure olevien käsittelemisestä. Saat lisätietoja Azure WebJobs ja WebJobs SDK käyttäminen [Azure WebJobs suositellaan resurssit](http://go.microsoft.com/fwlink/?linkid=390226).
 
