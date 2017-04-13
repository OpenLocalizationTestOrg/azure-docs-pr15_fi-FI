<properties
    pageTitle="Aloita blob storage ja Visual Studio yhteydessä services (WebJob projektit) | Microsoft Azure"
    description="Pikaviestien käyttäminen Blob-objektien tallennustilaan WebJob projekteissa, kun yhteys Azure tallennustilan Visual Studiossa yhdistetyt palvelut."
    services="storage"
    documentationCenter=""
    authors="TomArcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="storage"
    ms.workload="web"
    ms.tgt_pltfrm="vs-getting-started"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/18/2016"
    ms.author="tarcher"/>

# <a name="get-started-with-azure-blob-storage-and-visual-studio-connected-services-webjob-projects"></a>Aloita Azure Blob storage ja Visual Studio yhteydessä services (WebJob projektit)

[AZURE.INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Yleiskatsaus

Tässä artikkelissa on C# MALLIKOODEJA, jotka näyttävät käynnistämisestä prosessin Azure-blob luodaan tai päivitetään. MALLIKOODEJA Käytä [WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk.md) -versiota 1.x. Kun lisäät tallennustilan tilin WebJob projektin Visual Studio **Lisää yhdistetyt palvelut** -valintaikkunan, vastaava Azure-tallennustilan NuGet paketti asennetaan, .NET viitteet on lisätty projektiin ja yhteyden merkkijonot tallennustilan tilin päivitetään App.config-tiedostoa.



## <a name="how-to-trigger-a-function-when-a-blob-is-created-or-updated"></a>Funktion käynnistämisestä blob luodaan tai päivitetään

Tässä osassa esitellään käyttämisestä **BlobTrigger** -määrite.

 **Huomautus:** WebJobs SDK tarkistaa lokitiedostojen uusia tai muutettuja BLOB odottaminen. Tämä toimenpide on myös hidas; funktion ehkä ole käynnistetään useita minuutteja tai enää blob luomisen jälkeen.  Jos sovellus on käyttänyt BLOB heti, suositeltu tapa on jonon viestin luominen, kun luot blob ja käytä [QueueTrigger](../app-service-web/websites-dotnet-webjobs-sdk-storage-queues-how-to.md#trigger) -määritettä **BlobTrigger** -määrite, joka käsittelee blob-funktion sijasta.

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

## <a name="types-that-you-can-bind-to-blobs"></a>Tyypit, jotka voit sitoa BLOB-objektit

Voit käyttää seuraavia **BlobTrigger** määrite:

* **merkkijono**
* **TextReader-kohteesta**
* **Muodossa**
* **ICloudBlob**
* **CloudBlockBlob**
* **CloudPageBlob**
* Muuntyyppisten poistaa [ICloudBlobStreamBinder](#getting-serialized-blob-content-by-using-icloudblobstreambinder) mukaan

Jos haluat käsitellä Azure-tallennustilan tilin, voit lisätä **CloudStorageAccount** parametrin myös menetelmän allekirjoitus.

## <a name="getting-text-blob-content-by-binding-to-string"></a>Merkkijonon sitominen tekstin blob sisällön hakeminen

Jos teksti-BLOB pitäisi olla **BlobTrigger** voi käyttää **kyselymerkkijonon** parametrin. Seuraava koodi malli sitoo tekstin Blob-objektien **kyselymerkkijonon** parametrin **logMessage**. Funktio käyttää parametrin kirjoittaminen blob sisällön WebJobs SDK Raporttinäkymät-ikkunan.

        public static void WriteLog([BlobTrigger("input/{name}")] string logMessage,
            string name,
            TextWriter logger)
        {
             logger.WriteLine("Blob name: {0}", name);
             logger.WriteLine("Content:");
             logger.WriteLine(logMessage);
        }

## <a name="getting-serialized-blob-content-by-using-icloudblobstreambinder"></a>Muuntaa käytön sarjaksi blob sisällön ICloudBlobStreamBinder avulla

Seuraava koodi malli käyttää luokka, joka toteuttaa **ICloudBlobStreamBinder** käyttöön **BlobTrigger** -määritteen blob sitoa **WebImage** tyyppi.

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

**WebImage** sidonta-koodi on annettu **WebImageBinder** -luokka, joka johdetaan **ICloudBlobStreamBinder**.

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

## <a name="how-to-handle-poison-blobs"></a>Käsittelemisestä torjuttujen BLOB-objektit

Kun **BlobTrigger** -funktio ei toimi, SDK kutsuu sen uudelleen, siltä varalta, että virhe on aiheutunut lyhytkestoisia virhe. Jos virheen syynä on sisällön blob-funktio epäonnistuu aina, kun se yrittää käsitellä blob. Oletusarvon mukaan SDK kutsuu funktion enintään 5 luvulla annetun Blob-objektien. Jos viidennen yritä epäonnistuu, SDK Lisää viestiin nimeltä *webjobs blobtrigger-kosketusmyrkky*jonossa.

Uudelleenyritykset enimmäismäärä on määritettävä. [MaxDequeueCount](../app-service-web/websites-dotnet-webjobs-sdk-storage-queues-how-to.md#configqueue) samaa asetusta käytetään torjuttujen Blob-objektien käsittelyn ja torjuttujen jonon viestin käsittely.

Torjuttujen BLOB jonon viestin on JSON-objekti, joka sisältää seuraavat ominaisuudet:

* FunctionId (muodossa *{WebJob name}*. Toiminnot. *{Funktion nimen}*, esimerkiksi: WebJob1.Functions.CopyBlob)
* BlobType ("BlockBlob" tai "PageBlob")
* ContainerName
* BlobName
* ETag (blob versio-tunniste, esimerkiksi: "0x8D1DC6E70A277EF")

Seuraava koodi-esimerkki **CopyBlob** -funktio on koodi, joka määrittää, se voi epäonnistua aina, kun se on nimeltään. Kun SDK soittaa uudelleenyritykset enimmäismäärä, viesti on luotu torjuttujen blob-jonossa ja **LogPoisonBlob** -funktio käsittelee kyseisen viestin.

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

SDK deserializes automaattisesti JSON-viesti. Näin **PoisonBlobMessage** luokan:

        public class PoisonBlobMessage
        {
            public string FunctionId { get; set; }
            public string BlobType { get; set; }
            public string ContainerName { get; set; }
            public string BlobName { get; set; }
            public string ETag { get; set; }
        }

### <a name="blob-polling-algorithm"></a>Blob-objektien kysely algoritmin

WebJobs SDK tarkistaa kaikki säilöt määrittämää **BlobTrigger** määritteet sovellusten käynnistyksen yhteydessä. Suuri tallennustilan tilin tämä tarkistus voi kestää jonkin aikaa, joten voi olla hetken, ennen kuin uusi BLOB löytyy ja **BlobTrigger** toiminnot suoritetaan.

Voit tunnistaa uusia tai muutettuja BLOB-objektit, kun sovellus käynnistyy, SDK lukee säännöllisesti blob storage lokit. Blob-lokit ovat ellei ja Hae fyysisesti vain kirjoitettu 10 minuutin välein tai, joten voi olla merkittäviä viiveen jälkeen blob luodaan tai päivitetään ennen vastaavan **BlobTrigger** -funktio suorittaa.

Tällä poikkeuksen BLOB-objektit, jotka luot **Blob** -määritteen avulla. Kun WebJobs SDK Luo uusi Blob-objektien, se välittää uusi Blob-objektien heti **BlobTrigger** vastaavia tehtäviä. Tämän vuoksi halutessasi blob-syötteiden ja tulostaa yhdistettyjen SDK voit käsitellä niitä tehokkaasti. Mutta jos haluat vähän viive käynnissä oman Blob-objektien käsittelyn BLOB-objektit, jotka luodaan tai päivitetään muulla-Funktiot, on suositeltavaa käyttää **QueueTrigger** **BlobTrigger**sijaan.

### <a name="blob-receipts"></a>Blob-objektien kuittaukset

WebJobs SDK varmistetaan, että ei ole **BlobTrigger** -funktio saa kutsua useammin kuin kerran saman uusi tai päivitetty blob. Se tekee yllä *blob kuittaukset* jotta voit tarkistaa, onko tietyllä blob-versio on käsitelty.

Blob-objektien kuittaukset tallennetaan määrittämää AzureWebJobsStorage yhteysmerkkijonoa Azure-tallennustilan tilin *azure-webjobs-isännät* -säilö. Blob-kuittaus on seuraavat tiedot:

* Funktio, joka on kutsuttu Blob-objektien ("*{WebJob name}*. Toiminnot. *{Funktion nimen}*", esimerkiksi:"WebJob1.Functions.CopyBlob")
* Säilön nimi
* Blob-tyyppi ("BlockBlob" tai "PageBlob")
* Blob-nimi
* ETag (blob versio-tunniste, esimerkiksi: "0x8D1DC6E70A277EF")

Jos haluat aloittaa uudelleen, blob, voit poistaa kyseisen blob blob-kuittausta manuaalisesti *azure-webjobs-isäntien* säilöstä.

## <a name="related-topics-covered-by-the-queues-article"></a>Aiheeseen liittyvät olevien artikkelin aiheita

Lisätietoja käsittelemisestä Blob-objektien käsittelyn jonon sanoman käynnistämän tai WebJobs SDK ei koske blob-skenaariot käsittelyn Katso, [miten Azure jonon tallennusvälineiden WebJobs SDK: N kanssa](../app-service-web/websites-dotnet-webjobs-sdk-storage-queues-how-to.md).

Aiheeseen liittyvät artikkelin aiheita ovat seuraavat:

* Asynkroninen Funktiot
* Useita kertoja
* Kaksivaiheista sulkeminen
* Käytä funktiota tekstissä WebJobs SDK määritteet
* Määrittää SDK yhteyden merkkijonot koodi.
* Määritä arvot WebJobs SDK konstruktori parametrit-koodi
* Määritä **MaxDequeueCount** torjuttujen Blob-objektien käsittelyn.
* Funktion käynnistäminen manuaalisesti
* Kirjoita lokit

## <a name="next-steps"></a>Seuraavat vaiheet

Tässä artikkelissa on tarjonnut MALLIKOODEJA, jotka esittävät yleisiä tilanteita, joissa käyttäminen Azure BLOB käsittelemisestä. Saat lisätietoja Azure WebJobs ja WebJobs SDK käyttäminen [Azure WebJobs dokumentaatioresurssit](http://go.microsoft.com/fwlink/?linkid=390226).
