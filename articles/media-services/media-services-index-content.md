<properties
    pageTitle="Indeksoinnin mediatiedostojen Azure Media indeksointitoiminto kanssa"
    description="Azure Media indeksointitoiminto avulla voit tehdä mediatiedostojen sisällöstä haettavissa olevia ja luo teksti-tallenne suljettu tekstitys ja avainsanat. Tässä ohjeaiheessa kerrotaan, miten Media indeksointitoiminto."
    services="media-services"
    documentationCenter=""
    authors="Asolanki"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/12/2016"   
    ms.author="adsolank;juliako;johndeu"/>


# <a name="indexing-media-files-with-azure-media-indexer"></a>Indeksoinnin mediatiedostojen Azure Media indeksointitoiminto kanssa


Azure Media indeksointitoiminto avulla voit tehdä mediatiedostojen sisällöstä haettavissa olevia ja luo teksti-tallenne suljettu tekstitys ja avainsanat. Voit käsitellä yksi mediatiedosto tai useita erän mediatiedostot.  

>[AZURE.IMPORTANT] Kun sisällön indeksointi, varmista, että Käytä mediatiedostoja, jotka on hyvin selkeät puheeksi (ilman taustamusiikkia, ääni, tehosteiden ja mikrofonin hiss). Esimerkkejä sisällön: kokouksiin, luentoja tai esityksiä. Seuraavia sisältö ei ehkä sovellu indeksoinnin: elokuvia, TV-ohjelmat, mikä tahansa erilaiset ääni- ja äänitehosteiden, tallennettuja huonosti taustaäänet (hiss) sisällön.


Indeksoinnin työn voivat luoda seuraavat tulostaa:

- Suljettu kuvateksti tiedostoja seuraavissa muodoissa: **SAAME**, **TTML**ja **WebVTT**.

    Tunnisteen Recognizability, mitkä tulosten indeksoinnin työn perusteella miten tunnistettavan lähteen videon puhe on tekstitys tiedostoissa.  Voit käyttää Recognizability arvon näytön tulosteen tiedostojen käytettävyyttä. Pieni pistemäärän tarkoittaa äänenlaatu on heikko indeksoinnin tuloksia.
- Avainsana-tiedosto (XML).
- Äänen indeksoinnin blob-tiedostoon (AIB) käyttää SQL Serverin kanssa.

    Lisätietoja on artikkelissa [Käyttämällä AIB tiedostojen Azure Media indeksointitoiminto ja SQL Serverissä](https://azure.microsoft.com/blog/2014/11/03/using-aib-files-with-azure-media-indexer-and-sql-server/).


Tässä ohjeaiheessa esitellään luomisesta Indeksointitöiden **indeksoinnin sijoituksen** ja **Indeksoi useita tiedostoja**.

Katso Azure Media indeksointitoiminto päivityksiä [Media Services blogit](#preset).

## <a name="using-configuration-and-manifest-files-for-indexing-tasks"></a>Indeksoinnin tehtävien määritys ja luettelo-tiedostojen käyttäminen

Voit määrittää lisätietoja indeksoinnin tehtäviin tehtävän määrityksen avulla. Voit esimerkiksi määrittää metatietojen käytettävä media-tiedosto. Metatiedot käytetään laajentaaksesi sen sanaston kieli-ohjelma ja parantaa huomattavasti puheentunnistuksen tarkkuutta.  Olet myös voi määrittää haluttu tulostiedostoja.

Voit käsitellä useita mediatiedostojen myös kerralla luettelon-tiedoston avulla.

Lisätietoja on artikkelissa [Tehtävän valmiiksi Azure Media indeksointitoiminto varten](https://msdn.microsoft.com/library/dn783454.aspx).

## <a name="index-an-asset"></a>Sijoituksen indeksi

Seuraavalla tavalla Lataa mediatiedosto kuin sijoituksen ja luo työn indeksoida kohteen.

Huomaa, että jos kokoonpanotiedosto ei ole määritetty, mediatiedosto indeksoidaan kaikki oletusasetuksilla.

    static bool RunIndexingJob(string inputMediaFilePath, string outputFolder, string configurationFile = "")
    {
        // Create an asset and upload the input media file to storage.
        IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
            "My Indexing Input Asset",
            AssetCreationOptions.None);

        // Declare a new job.
        IJob job = _context.Jobs.Create("My Indexing Job");

        // Get a reference to the Azure Media Indexer.
        string MediaProcessorName = "Azure Media Indexer";
        IMediaProcessor processor = GetLatestMediaProcessorByName(MediaProcessorName);

        // Read configuration from file if specified.
        string configuration = string.IsNullOrEmpty(configurationFile) ? "" : File.ReadAllText(configurationFile);

        // Create a task with the encoding details, using a string preset.
        ITask task = job.Tasks.AddNew("My Indexing Task",
            processor,
            configuration,
            TaskOptions.None);

        // Specify the input asset to be indexed.
        task.InputAssets.Add(asset);

        // Add an output asset to contain the results of the job.
        task.OutputAssets.AddNew("My Indexing Output Asset", AssetCreationOptions.None);

        // Use the following event handler to check job progress.  
        job.StateChanged += new EventHandler<JobStateChangedEventArgs>(StateChanged);

        // Launch the job.
        job.Submit();

        // Check job execution and wait for job to finish.
        Task progressJobTask = job.GetExecutionProgressTask(CancellationToken.None);
        progressJobTask.Wait();

        // If job state is Error, the event handling
        // method for job progress should log errors.  Here we check
        // for error state and exit if needed.
        if (job.State == JobState.Error)
        {
            Console.WriteLine("Exiting method due to job error.");
            return false;
        }

        // Download the job outputs.
        DownloadAsset(task.OutputAssets.First(), outputFolder);

        return true;
    }

    static IAsset CreateAssetAndUploadSingleFile(string filePath, string assetName, AssetCreationOptions options)
    {
        IAsset asset = _context.Assets.Create(assetName, options);

        var assetFile = asset.AssetFiles.Create(Path.GetFileName(filePath));
        assetFile.Upload(filePath);

        return asset;
    }

    static void DownloadAsset(IAsset asset, string outputDirectory)
    {
        foreach (IAssetFile file in asset.AssetFiles)
        {
            file.Download(Path.Combine(outputDirectory, file.Name));
        }
    }

    static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
    {
        var processor = _context.MediaProcessors
        .Where(p => p.Name == mediaProcessorName)
        .ToList()
        .OrderBy(p => new Version(p.Version))
        .LastOrDefault();

        if (processor == null)
            throw new ArgumentException(string.Format("Unknown media processor",
                                                       mediaProcessorName));

        return processor;
    }  
<!-- __ -->
### <a id="output_files"></a>Tulostus-tiedostot

Indeksoinnin työn luo oletusarvoisesti seuraavat tulostus-tiedostot. Tiedostot tallennetaan tulosteen ensimmäisen kohteen.

Kun kyseessä on useampi kuin yksi syötteen mediatiedosto, indeksointitoiminto Luo luettelon nimeltä "JobResult.txt" työn, tulostaa tiedoston. Kunkin syötteenä mediatiedosto, tuloksena olevat AIB, SAAME, TTML, WebVTT ja avainsana tiedostot ovat peräkkäin numeroitu ja nimeltä "Tunnus" avulla.

Tiedostonimi | Kuvaus
----------|------------
__InputFileName.aib__ | Indeksoinnin blob äänitiedosto. <br /><br /> Indeksoinnin Blob (AIB) äänitiedosto on binaaritiedosto, joka voidaan etsiä Microsoft SQL Server koko tekstin haun avulla.  AIB-tiedosto on tehokkaampaa kuin yksinkertainen kuvateksti-tiedostoja, koska se sisältää vaihtoehtojen jokaisen sanan salliminen paljon monipuolisemman haun avulla. <br/> <br/>Se edellyttää tietokoneen käynnissä olevat Microsoft SQL server 2008: n tai uudemman indeksointitoiminto SQL-lisäosan asentamista. Haun AIB käyttämällä Microsoft SQL server koko tekstin haun avulla tarkempi hakutulosten kuin etsiminen WAMI luoma tekstitys tiedostot. Tämä johtuu siitä AIB sisältää sanan vaihtoehtoja mitä äänen samalla olisi tekstitys tiedostoissa suurin LUOTTAMUSVÄLI word, kunkin osion äänen. Jos puhuttujen sanojen hakeminen on upmost tärkeys, on suositeltavaa AIB-yhdessä käytettäväksi Microsoft SQL Server.<br/><br/> Voit ladata lisäosan valitsemalla <a href="http://aka.ms/indexersql">Azure Media indeksointitoiminto SQL-lisäosa</a>. <br/><br/>On myös mahdollista käyttämiseen muut hakukoneet, kuten Apache Lucene/Solr indeksoida yksinkertaisesti videon tekstitys ja avainsana XML-tiedostoja, mutta tuloksena yhtä tarkkoja hakutuloksissa.
__InputFileName.smi__<br />__InputFileName.ttml__<br />__InputFileName.vtt__ |Suljettu otsikko (kopio)-tiedostoja SAAME, TTML ja WebVTT muodossa.<br/><br/>Ne voidaan tehdä ääni- ja videotiedostoista helppokäyttöisempiä kuuleminen disability kärsivät ihmiset.<br/><br/>Suljettu kuvateksti-tiedostoissa nimeltään <b>Recognizability</b> mitä tulosten indeksoinnin työn perusteella miten tunnistettavan videon lähde puhe on tunnisteen.  Voit käyttää <b>Recognizability</b> arvon näytön tulosteen tiedostojen käytettävyyttä. Pieni pistemäärän tarkoittaa äänenlaatu on heikko indeksoinnin tuloksia.
__InputFileName.kw.xml<br />InputFileName.info__ |Avainsanojen ja tiedot tiedostot. <br/><br/>Avainsanojen tiedosto on XML-tiedostoon, joka sisältää avainsanoja poimittujen puhe sisältöä, korkojakso ja siirtymä tiedot. <br/><br/>Tiedot-tiedosto on vain teksti-tiedosto, jossa on kunkin termin tunnistettu hajautetun tiedot. Ensimmäisen rivin on erityisen, ja se sisältää Recognizability kirjainarvosanan. Seuraavien kukin rivi on sarkaineroteltuun luettelon seuraavat tiedot: Käynnistä ajat, päättymisaika, sana tai lause, LUOTTAMUSVÄLI. Aikaa on esitetty sekuntia ja luottamus esitetään lukuna väliltä 0 – 1. <br/><br/>Esimerkki rivin: "1,20 nousevat 1,45 word 0.67" <br/><br/>Nämä tiedostot voidaan käyttää tarkoituksiin määrälle, puhe analyysin suorittamiseen tai käsiksi hakukoneet, kuten Bing, Google tai Microsoft SharePoint tekemään mediatiedostojen auttaa tai jopa käyttää useita mainosten aikana.
__JobResult.txt__ |Tulosteen luettelo, vain silloin, kun indeksoinnin useita tiedostoja, joka sisältää seuraavat tiedot:<br/><br/><table border="1"><tr><th>Lähdetiedosto</th><th>Alias</th><th>MediaLength</th><th>Virhe</th></tr><tr><td>a.mp4</td><td>Media_1</td><td>300</td><td>0</td></tr><tr><td>b.mp4</td><td>Media_2</td><td>0</td><td>3000</td></tr><tr><td>c.mp4</td><td>Media_3</td><td>600</td><td>0</td></tr></table><br/>



Jos kaikki syötteen mediatiedostot indeksoidaan onnistuneesti, indeksoinnin työ epäonnistuu virhekoodi 4000. Lisätietoja on artikkelissa [virhekoodit](#error_codes).

## <a name="index-multiple-files"></a>Indeksi useita tiedostoja

Seuraavalla tavalla latauksia useita mediatiedostojen kuin sijoituksen ja luo työn indeksoida kaikki erän nämä tiedostot.

Tiedostojen tiedoston, jonka .lst-tunniste on luotu ja lataus kohteen kyselyjä. Tiedostojen tiedosto sisältää kaikki resurssi-tiedostojen luettelon. Lisätietoja on artikkelissa [Tehtävän valmiiksi Azure Media indeksointitoiminto varten](https://msdn.microsoft.com/library/dn783454.aspx).

    static bool RunBatchIndexingJob(string[] inputMediaFiles, string outputFolder)
    {
        // Create an asset and upload to storage.
        IAsset asset = CreateAssetAndUploadMultipleFiles(inputMediaFiles,
            "My Indexing Input Asset - Batch Mode",
            AssetCreationOptions.None);

        // Create a manifest file that contains all the asset file names and upload to storage.
        string manifestFile = "input.lst";            
        File.WriteAllLines(manifestFile, asset.AssetFiles.Select(f => f.Name).ToArray());
        var assetFile = asset.AssetFiles.Create(Path.GetFileName(manifestFile));
        assetFile.Upload(manifestFile);

        // Declare a new job.
        IJob job = _context.Jobs.Create("My Indexing Job - Batch Mode");

        // Get a reference to the Azure Media Indexer.
        string MediaProcessorName = "Azure Media Indexer";
        IMediaProcessor processor = GetLatestMediaProcessorByName(MediaProcessorName);

        // Read configuration.
        string configuration = File.ReadAllText("batch.config");

        // Create a task with the encoding details, using a string preset.
        ITask task = job.Tasks.AddNew("My Indexing Task - Batch Mode",
            processor,
            configuration,
            TaskOptions.None);

        // Specify the input asset to be indexed.
        task.InputAssets.Add(asset);

        // Add an output asset to contain the results of the job.
        task.OutputAssets.AddNew("My Indexing Output Asset - Batch Mode", AssetCreationOptions.None);

        // Use the following event handler to check job progress.  
        job.StateChanged += new EventHandler<JobStateChangedEventArgs>(StateChanged);

        // Launch the job.
        job.Submit();

        // Check job execution and wait for job to finish.
        Task progressJobTask = job.GetExecutionProgressTask(CancellationToken.None);
        progressJobTask.Wait();

        // If job state is Error, the event handling
        // method for job progress should log errors.  Here we check
        // for error state and exit if needed.
        if (job.State == JobState.Error)
        {
            Console.WriteLine("Exiting method due to job error.");
            return false;
        }

        // Download the job outputs.
        DownloadAsset(task.OutputAssets.First(), outputFolder);

        return true;
    }

    private static IAsset CreateAssetAndUploadMultipleFiles(string[] filePaths, string assetName, AssetCreationOptions options)
    {
        IAsset asset = _context.Assets.Create(assetName, options);

        foreach (string filePath in filePaths)
        {
            var assetFile = asset.AssetFiles.Create(Path.GetFileName(filePath));
            assetFile.Upload(filePath);
        }

        return asset;
    }

### <a name="partially-succeeded-job"></a>Osittain onnistui työ

Jos kaikki syötteen mediatiedostot indeksoidaan onnistuneesti, indeksoinnin työ epäonnistuu virhekoodi 4000. Lisätietoja on artikkelissa [virhekoodit](#error_codes).


Luodaan samaan tulostus (kuten onnistui työt). Voit viitata luettelon kohdetiedosto nähdäksesi, mitkä syötteen tiedostot ovat epäonnistui sarakkeen virhearvoja mukaan. Syötteen tiedostoissa, jotka epäonnistui, tuloksena olevat AIB, SAAME, TTML, WebVTT ja avainsana tiedostoja ei luoda.

### <a id="preset"></a>Tehtävän esiasetus Azure Media indeksointitoiminto varten

From Azure Media indeksointitoiminto käsittelyn voi mukauttaa antamalla valinnainen tehtävästä rinnalla tehtävän valmiiksi.  Seuraavassa kuvataan näiden määritysten xml muodossa.

Nimi | Vaadi | Kuvaus
----|----|---
__syöte__ | EPÄTOSI | Resurssi-tiedostot, jotka haluat luoda hakemiston.</p><p>Azure Media indeksointitoiminto tukee media tiedostomuodot: MP4, WMV, MP3, M4A, WMA, AAC, WAV.</p><p>Voit määrittää tiedostonimi (s) siihen **input** -elementtiä **nimi** tai **luettelo** -määrite (Katso kuvaa alla). Jos et määritä resurssi-tiedoston, josta indeksi-ensisijainen tiedosto kerätään. Jos ensisijainen resurssi-tiedostoa ei ole määritetty, ensimmäisen tiedoston syötteen resurssi on indeksoitu.</p><p>Määritä resurssi tiedoston nimeä, toimi:<br />`<input name="TestFile.wmv">`<br /><br />Voit myös indeksoida useita resurssi tiedostoja samalla kertaa (enintään 10). Toiminto:<br /><br /><ol class="ordered"><li><p>Luo tekstitiedosto (luettelo-tiedosto) ja anna sille .lst-tunniste. </p></li><li><p>Lisätä kaikkien kohteiden tiedostonimien luettelo syötteen kohteiden luettelon tiedostoon. </p></li><li><p>Lisää kohteen (Lataa) thanifest tiedosto.  </p></li><li><p>Määritä tiedostojen tiedoston nimi syöte määrite.<br />`<input list="input.lst">`</li></ol><br /><br />Huomautus: Jos lisäät yli 10 tiedostojen luettelon tiedoston, indeksoinnin työ epäonnistuu 2006 virhekoodi.
__Metatietojen__ | EPÄTOSI | Käytettävän sanaston mukauttamista määritetyn resurssi-tiedostot-metatiedot.  Hyödyllinen tunnistamaan normaalista sanaston sanat, kuten erisnimien indeksointitoiminto valmistelemaan.<br />`<metadata key="..." value="..."/>` <br /><br />Voit antaa esimääritetyt __avaimet__ __arvot__ . Tällä hetkellä tueta seuraavia pikanäppäimiä:<br /><br />"otsikko" ja "kuvaus" - käytetään säätää tehosteasetuksilla kielen sanaston mukauttamista työtäsi malli ja parantaa puheentunnistuksen tarkkuutta.  Arvot Määritä Internet-hakujen contextually tarvittaviin tiedostoja, käyttämällä sisältöä ihmisistä sisäinen sanaston indeksointi tehtävän kesto.<br />`<metadata key="title" value="[Title of the media file]" />`<br />`<metadata key="description" value="[Description of the media file] />"`
__ominaisuudet__ <br /><br /> Lisätty versio 1.2. Ainoa tuettu toiminto on tällä hetkellä puheentunnistuksen ("ASR").| EPÄTOSI | Puheentunnistus-toiminto on näppäimet seuraavat asetukset:<table><tr><th><p>Avain</p></th>     <th><p>Kuvaus</p></th><th><p>Esimerkkiarvo</p></th></tr><tr><td><p>Kieli</p></td><td><p>Luonnollisen kielen tunnistetaan multimediatiedoston.</p></td><td><p>Englanti, espanja</p></td></tr><tr><td><p>CaptionFormats</p></td><td><p>puolipisteillä erotettu luettelo haluamasi otsikko siirtomuodoista (jos saatavilla)</p></td><td><p>ttml saame; webvtt</p></td></tr><tr><td><p>GenerateAIB</p></td><td><p>Totuusarvo merkinnän määrittäminen, onko AIB tiedostoa tarvitaan (käytettäväksi SQL Serverin ja asiakkaan indeksointitoiminto IFilter).  Lisätietoja on artikkelissa <a href="http://azure.microsoft.com/blog/2014/11/03/using-aib-files-with-azure-media-indexer-and-sql-server/">Käyttämällä AIB tiedostojen Azure Media indeksointitoiminto ja SQL Serverissä</a>.</p></td><td><p>TOSI. EPÄTOSI</p></td></tr><tr><td><p>GenerateKeywords</p></td><td><p>Totuusarvo merkinnän määrittäminen, onko avainsana XML-tiedosto on pakollinen.</p></td><td><p>TOSI. EPÄTOSI. </p></td></tr><tr><td><p>ForceFullCaption</p></td><td><p>Totuusarvo merkinnän määrittäminen pakottaa koko kuvatekstit (riippumatta luottamustaso) vai ei.  </p><p>Oletusarvo on EPÄTOSI, jolloin sanat ja ilmaisut, joka on pienempi kuin 50 % luottamustaso pois lopullinen kuvateksti-tulostus ja korvataan pistettä ("...").  Kolme pistettä on hyötyä kuvateksti laadunvalvontaa ja valvonta.</p></td><td><p>TOSI. EPÄTOSI. </p></td></tr></table>

### <a id="error_codes"></a>Virhekoodit

Virheen, jos Azure Media indeksointitoiminto raportin takaisin jompikumpi seuraavista virhekoodit:

Koodi | Nimi | Mahdollisia syitä
-----|------|------------------
2000: ssa | Virheellinen määritys | Virheellinen määritys
2001 | Virheellinen syötteen resurssit | Puuttuva syötteen varat tai tyhjä resurssi.
2002 | Luettelo ei kelpaa | Luettelo on tyhjä tai luettelo sisältää virheellisiä kohteita.
2003 | Lataa mediatiedosto epäonnistui | Tiedostojen tiedoston virheellinen URL-osoite.
2004 | Protokolla, joita ei tueta | Protokolla media URL-osoitteita ei tueta.
2005 | Tiedostotyyppiä | Median tiedostotyyppi ei tue.
2006 | Liian monta syötteen tiedostot | Kirjoita luettelo on yli 10 tiedostoja.
3000 | Toistaa mediatiedoston epäonnistui | Ei tueta media-pakkauksenhallinnan <br/>tai<br/> Vioittuneet mediatiedosto <br/>tai<br/> Ei ole äänivirtaa syötteen Media.
4000 | Erän indeksoinnin osittain onnistui | Median tiedostoja ei voi indeksoida. Lisätietoja on artikkelissa <a href="#output_files">tiedostot</a>.
muut | Sisäisiä virheitä | Ota yhteyttä tukihenkilöstöön. indexer@microsoft.com


## <a id="supported_languages"></a>Tukemat kielet

Tällä hetkellä tueta englannin ja Espanjan kielellä. Lisätietoja on artikkelissa [v1.2 release blogimerkinnän](https://azure.microsoft.com/blog/2015/04/13/azure-media-indexer-spanish-v1-2/).


##<a name="media-services-learning-paths"></a>Media-palveluiden oppimispolut

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Palautteen antaminen

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


## <a name="related-links"></a>Aiheeseen liittyvät linkit

[Azure Media Services-palveluiden Analytics yleiskatsaus](media-services-analytics-overview.md)

[Azure Media indeksointitoiminto ja SQL Server AIB tiedostojen käyttäminen](https://azure.microsoft.com/blog/2014/11/03/using-aib-files-with-azure-media-indexer-and-sql-server/)

[Indeksoinnin mediatiedostojen ja Azure Media indeksointitoiminto 2 esikatselu](media-services-process-content-with-indexer2.md)
