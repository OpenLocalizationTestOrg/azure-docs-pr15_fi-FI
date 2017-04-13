<properties 
    pageTitle="Voit käyttää WebJobs SDK Azure-blob-säiliö" 
    description="Opettele käyttämään Azure-blob-säiliö WebJobs SDK: N kanssa. Käynnistää prosessin, kun uusi Blob-objektien näkyy säilön ja käsitellä "torjuttujen BLOB"." 
    services="app-service\web, storage" 
    documentationCenter=".net" 
    authors="tdykstra" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="06/01/2016" 
    ms.author="tdykstra"/>

# <a name="how-to-use-azure-blob-storage-with-the-webjobs-sdk"></a>Voit käyttää WebJobs SDK Azure-blob-säiliö

## <a name="overview"></a>Yleiskatsaus

Tässä oppaassa on C# MALLIKOODEJA, jotka näyttävät käynnistämisestä prosessin Azure-blob luodaan tai päivitetään. MALLIKOODEJA Käytä [WebJobs SDK](websites-dotnet-webjobs-sdk.md) -versiota 1.x.

Tutustu MALLIKOODEJA, joka näyttää, miten voit luoda BLOB- [Azure jonon tallennusvälineiden WebJobs SDK: N kanssa](websites-dotnet-webjobs-sdk-storage-queues-how-to.md). 
        
Oppaan oletetaan, että tiedät, [kuinka voit luoda WebJob projektin Visual Studiossa yhteyden merkkijonoja, jotka osoittavat tallennustilan tilin](websites-dotnet-webjobs-sdk-get-started.md) tai [useiden tallennustilan tilit](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/MultipleStorageAccountsEndToEndTests.cs).

## <a id="trigger"></a>Funktion käynnistämisestä blob luodaan tai päivitetään

Tässä osassa esitellään käyttämisestä `BlobTrigger` määrite. 

> [AZURE.NOTE] WebJobs SDK tarkistaa lokitiedostojen uusia tai muutettuja BLOB odottaminen. Tämä toimenpide ei ole reaaliaikainen; funktion ehkä ole käynnistetään useita minuutteja tai enää blob luomisen jälkeen. Lisäksi [lokit luodaan "parhaat tehokkuutta" tallennustilan](https://msdn.microsoft.com/library/azure/hh343262.aspx) perusteella; ei ei takaa, että kaikki tapahtumat tallennetaan. Tietyissä tilanteissa saattaa vastattu lokitiedot. Jos blob käynnistimien nopeuden ja luotettavuuden rajoituksista ei hyväksytä sovelluksen, suositeltava tapa on jonon viestin luominen, kun luot blob ja käytä sen sijaan, että [QueueTrigger](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#trigger) -määritettä `BlobTrigger` funktiota, joka käsittelee blob-määrite.

### <a name="single-placeholder-for-blob-name-with-extension"></a>Blob-objektien nimen tunniste yksi paikkamerkki  

Seuraava koodi malli Kopioi teksti BLOB-objektit, jotka näkyvät *syötteen* säilö *tulosteen* säilöön:

        public static void CopyBlob([BlobTrigger("input/{name}")] TextReader input,
            [Blob("output/{name}")] out string output)
        {
            output = input.ReadToEnd();
        }

Määritteen konstruktoria kestää parametrin, joka määrittää säilön nimi ja blob-nimen paikkamerkki. Jos Blob-objektien nimeltä *Blob1.txt* luodaan *syötteen* säilö, funktio luo tässä esimerkissä Blob-objektien nimeltä *Blob1.txt* *tulostus* -säilössä. 

Voit määrittää nimen kuvion nimi Blob-objektien, paikkamerkkien seuraava koodi näyte esitetyllä tavalla:

        public static void CopyBlob([BlobTrigger("input/original-{name}")] TextReader input,
            [Blob("output/copy-{name}")] out string output)
        {
            output = input.ReadToEnd();
        }

Tämä koodi kopioi vain BLOB-objektit, joiden nimet alkavat merkkijonolla "alkuperäinen-". Esimerkiksi *Alkuperäisen Blob1.txt* *Näytettävä* säilössä kopioidaan *Kopioi Blob1.txt* *tulostus* -säilössä.

Jos haluat Määritä Blob-objektien nimet, jotka on aaltosulkeiden nimi nimi-kaksoisnapsauttamalla aaltosulkeet. Jos haluat etsiä *kuvia* , joiden nimet tältä säilössä BLOB esimerkiksi:

        {20140101}-soundfile.mp3

Käytä tätä kuvion varten:

        images/{{20140101}}-{name}

Esimerkissä *nimen* paikkamerkki-arvo on *soundfile.mp3*. 

### <a name="separate-blob-name-and-extension-placeholders"></a>Erillinen Blob-objektien nimi ja tunniste paikkamerkit

Seuraava koodi malli muuttuu tiedostotunniste, kun se kopioi BLOB-objektit, jotka näkyvät *tulosteen* säilöön *syötteen* säilö. Koodin kirjautuu *syötteen* Blob-objektien tunniste ja määrittää *tulosteen* Blob-objektien tunniste *.txt*.

        public static void CopyBlobToTxtFile([BlobTrigger("input/{name}.{ext}")] TextReader input,
            [Blob("output/{name}.txt")] out string output,
            string name,
            string ext,
            TextWriter logger)
        {
            logger.WriteLine("Blob name:" + name);
            logger.WriteLine("Blob extension:" + ext);
            output = input.ReadToEnd();
        }

## <a id="types"></a>Tyypit, jotka voit sitoa BLOB-objektit

Voit käyttää `BlobTrigger` seuraavanlaisia määrite:

* `string`
* `TextReader`
* `Stream`
* `ICloudBlob`
* `CloudBlockBlob`
* `CloudPageBlob`
* `CloudBlobContainer`
* `CloudBlobDirectory`
* `IEnumerable<CloudBlockBlob>`
* `IEnumerable<CloudPageBlob>`
* Muuntyyppisten poistaa [ICloudBlobStreamBinder](#icbsb) mukaan 

Jos haluat käsitellä Azure-tallennustilan tilin, voit lisätä `CloudStorageAccount` parametri menetelmän allekirjoitus.

Katso esimerkkejä [blob sidonta-koodin GitHub.com azure-webjobs-sdk-säilössä](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/BlobBindingEndToEndTests.cs).

## <a id="string"></a>Merkkijonon sitominen tekstin blob sisällön hakeminen

Jos teksti-BLOB pitäisi olla `BlobTrigger` voi suojata `string` parametri. Seuraava koodi malli sitoo tekstin Blob-objektien, `string` parametri `logMessage`. Funktio käyttää parametrin kirjoittaminen blob sisällön WebJobs SDK Raporttinäkymät-ikkunan. 
 
        public static void WriteLog([BlobTrigger("input/{name}")] string logMessage,
            string name, 
            TextWriter logger)
        {
             logger.WriteLine("Blob name: {0}", name);
             logger.WriteLine("Content:");
             logger.WriteLine(logMessage);
        }

## <a id="icbsb"></a>Muuntaa käytön sarjaksi blob sisällön ICloudBlobStreamBinder avulla

Seuraava koodi malli käyttää luokka, joka toteuttaa `ICloudBlobStreamBinder` käyttöön `BlobTrigger` määritteen Blob-objektien, jos haluat sitoa `WebImage` tyyppi.

        public static void WaterMark(
            [BlobTrigger("images3/{name}")] WebImage input,
            [Blob("images3-watermarked/{name}")] out WebImage output)
        {
            output = input.AddTextWatermark("WebJobs SDK", 
                horizontalAlign: "Center", verticalAlign: "Middle",
                fontSize: 48, opacity: 50);
        }
        public static void Resize(
            [BlobTrigger("images3-watermarked/{name}")] WebImage input,
            [Blob("images3-resized/{name}")] out WebImage output)
        {
            var width = 180;
            var height = Convert.ToInt32(input.Height * 180 / input.Width);
            output = input.Resize(width, height);
        }

`WebImage` Sidonta koodi on säädetty `WebImageBinder` luokka, joka johdetaan `ICloudBlobStreamBinder`.

        public class WebImageBinder : ICloudBlobStreamBinder<WebImage>
        {
            public Task<WebImage> ReadFromStreamAsync(Stream input, 
                System.Threading.CancellationToken cancellationToken)
            {
                return Task.FromResult<WebImage>(new WebImage(input));
            }
            public Task WriteToStreamAsync(WebImage value, Stream output,
                System.Threading.CancellationToken cancellationToken)
            {
                var bytes = value.GetBytes();
                return output.WriteAsync(bytes, 0, bytes.Length, cancellationToken);
            }
        }

## <a name="getting-the-blob-path-for-the-triggering-blob"></a>Hakeminen käynnistävän Blob-objektien blob-polku

Hanki säilö nimi ja Blob-objektien nimi, joka on saatu funktion Blob-objektien, Sisällytä `blobTrigger` merkkijono parametrin funktion allekirjoituksessa.

        public static void WriteLog([BlobTrigger("input/{name}")] string logMessage,
            string name,
            string blobTrigger,
            TextWriter logger)
        {
             logger.WriteLine("Full blob path: {0}", blobTrigger);
             logger.WriteLine("Content:");
             logger.WriteLine(logMessage);
        }


## <a id="poison"></a>Käsittelemisestä torjuttujen BLOB-objektit

Kun `BlobTrigger` funktio epäonnistuu SDK soittaa se uudelleen, niin virheen on aiheutunut lyhytkestoisia virhe. Jos virheen syynä on sisällön blob-funktio epäonnistuu aina, kun se yrittää käsitellä blob. Oletusarvon mukaan SDK kutsuu funktion enintään 5 luvulla annetun Blob-objektien. Jos viidennen yritä epäonnistuu, SDK Lisää viestiin nimeltä *webjobs blobtrigger-kosketusmyrkky*jonossa.

Uudelleenyritykset enimmäismäärä on määritettävä. [MaxDequeueCount](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#configqueue) samaa asetusta käytetään torjuttujen Blob-objektien käsittelyn ja torjuttujen jonon viestin käsittely. 

Torjuttujen BLOB jonon viestin on JSON-objekti, joka sisältää seuraavat ominaisuudet:

* FunctionId (muodossa *{WebJob name}*. Toiminnot. *{Funktion nimen}*, esimerkiksi: WebJob1.Functions.CopyBlob)
* BlobType ("BlockBlob" tai "PageBlob")
* ContainerName
* BlobName
* ETag (blob versio-tunniste, esimerkiksi: "0x8D1DC6E70A277EF")

Seuraava koodi-malli `CopyBlob` funktio on koodi, joka määrittää, se voi epäonnistua aina, kun se on nimeltään. Kun SDK soittaa uudelleenyritykset enimmäismäärä, viesti on luotu torjuttujen blob-jonossa ja viestin käsitellään `LogPoisonBlob` funktio. 

        public static void CopyBlob([BlobTrigger("input/{name}")] TextReader input,
            [Blob("textblobs/output-{name}")] out string output)
        {
            throw new Exception("Exception for testing poison blob handling");
            output = input.ReadToEnd();
        }
        
        public static void LogPoisonBlob(
        [QueueTrigger("webjobs-blobtrigger-poison")] PoisonBlobMessage message,
            TextWriter logger)
        {
            logger.WriteLine("FunctionId: {0}", message.FunctionId);
            logger.WriteLine("BlobType: {0}", message.BlobType);
            logger.WriteLine("ContainerName: {0}", message.ContainerName);
            logger.WriteLine("BlobName: {0}", message.BlobName);
            logger.WriteLine("ETag: {0}", message.ETag);
        }

SDK deserializes automaattisesti JSON-viesti. Näin `PoisonBlobMessage` luokan: 

        public class PoisonBlobMessage
        {
            public string FunctionId { get; set; }
            public string BlobType { get; set; }
            public string ContainerName { get; set; }
            public string BlobName { get; set; }
            public string ETag { get; set; }
        }

### <a id="polling"></a>Blob-objektien kysely algoritmin

WebJobs SDK tarkistaa kaikki säilöt määrittämää `BlobTrigger` määritteet sovellusten käynnistyksen yhteydessä. Suuri tallennustilan tilin tämä tarkistus voi kestää jonkin aikaa, joten se saattaa olla hetken, ennen kuin uusi BLOB löytyy ja `BlobTrigger` toiminnot suoritetaan.

Voit tunnistaa uusia tai muutettuja BLOB-objektit, kun sovellus käynnistyy, SDK lukee säännöllisesti blob storage lokit. Blob-lokit ovat ellei ja Hae fyysisesti vain kirjoitettu 10 minuutin välein tai, niin saattaa olla merkittäviä viiveen jälkeen blob luodaan tai päivitetään ennen vastaavan `BlobTrigger` funktio suoritetaan. 

Poikkeus BLOB-objektit, jotka on luotu, `Blob` määrite. Kun WebJobs SDK Luo uusi Blob-objektien, se välittää uusi Blob-objektien välittömästi kaikki vastaavat `BlobTrigger` funktiot. Tämän vuoksi halutessasi blob-syötteiden ja tulostaa yhdistettyjen SDK voit käsitellä niitä tehokkaasti. Mutta halutessasi käynnissä oman blob BLOB-objektit, jotka luodaan tai päivitetään muulla käsittelytoimintoihin pieni viive on suositeltavaa `QueueTrigger` sijaan `BlobTrigger`.

### <a id="receipts"></a>Blob-objektien kuittaukset

WebJobs SDK varmistetaan, joka ei ole `BlobTrigger` funktio saa kutsua samassa uusi tai päivitetty blob useammin kuin kerran. Se tekee yllä *blob kuittaukset* jotta voit tarkistaa, onko tietyllä blob-versio on käsitelty.

Blob-objektien kuittaukset tallennetaan määrittämää AzureWebJobsStorage yhteysmerkkijonoa Azure-tallennustilan tilin *azure-webjobs-isännät* -säilö. Blob-kuittaus on seuraavat tiedot:

* Funktio, joka on kutsuttu Blob-objektien ("*{WebJob name}*. Toiminnot. *{Funktion nimen}*", esimerkiksi:"WebJob1.Functions.CopyBlob")
* Säilön nimi
* Blob-tyyppi ("BlockBlob" tai "PageBlob")
* Blob-nimi
* ETag (blob versio-tunniste, esimerkiksi: "0x8D1DC6E70A277EF")

Jos haluat aloittaa uudelleen, blob, voit poistaa kyseisen blob blob-kuittausta manuaalisesti *azure-webjobs-isäntien* säilöstä.

## <a id="queues"></a>Aiheeseen liittyvät olevien artikkelin aiheita

Lisätietoja käsittelemisestä Blob-objektien käsittelyn jonon sanoman käynnistämän tai WebJobs SDK ei koske blob-skenaariot käsittelyn Katso, [miten Azure jonon tallennusvälineiden WebJobs SDK: N kanssa](websites-dotnet-webjobs-sdk-storage-queues-how-to.md). 

Aiheeseen liittyvät artikkelin aiheita ovat seuraavat:

* Asynkroninen Funktiot
* Useita kertoja
* Kaksivaiheista sulkeminen
* Käytä funktiota tekstissä WebJobs SDK määritteet
* Määrittää SDK yhteyden merkkijonot koodi.
* Määritä arvot WebJobs SDK konstruktori parametrit-koodi
* Määritä `MaxDequeueCount` torjuttujen Blob-objektien käsittelyä varten.
* Funktion käynnistäminen manuaalisesti
* Kirjoita lokit

## <a id="nextsteps"></a>Seuraavat vaiheet

Tässä oppaassa on tarjonnut MALLIKOODEJA, jotka esittävät yleisiä tilanteita, joissa käyttäminen Azure BLOB käsittelemisestä. Saat lisätietoja Azure WebJobs ja WebJobs SDK käyttäminen [Azure WebJobs suositellaan resurssit](http://go.microsoft.com/fwlink/?linkid=390226).
 
