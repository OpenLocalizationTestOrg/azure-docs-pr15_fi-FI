<properties
    pageTitle="Hyperlapse mediatiedostot kanssa Azure Media Hyperlapse | Microsoft Azure"
    description="Azure Media Hyperlapse Luo tasainen aika lakkasi olemasta voimassa videot ensimmäinen henkilö tai toiminnon kameran sisältöä. Tässä ohjeaiheessa kerrotaan, miten Media indeksointitoiminto."
    services="media-services"
    documentationCenter=""
    authors="asolanki"
    manager="johndeu"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/19/2016"  
    ms.author="adsolank"/>


# <a name="hyperlapse-media-files-with-azure-media-hyperlapse"></a>Hyperlapse mediatiedostot Azure Media Hyperlapse kanssa

Azure Media Hyperlapse on Media suoritin (MP), joka luo tasainen aika lakkasi olemasta voimassa videot ensimmäinen henkilö tai toiminnon kameran sisällöstä.  Pilvipohjainen rinnakkaisten [Microsoft Researchin työpöydän Hyperlapse Pro](http://aka.ms/hyperlapse)ja puhelimitse välitettävä Hyperlapse Mobile Microsoft Hyperlapse varten Azure Media-palveluita käyttämällä vaakasuunnassa skaalata ja parallelize Azure Media Services Media käsittely-ympäristö valtaviin asteikon joukkosähköposti Hyperlapse käsittely.

>[AZURE.IMPORTANT]Microsoft Hyperlapse on suunniteltu toimivat parhaiten ensimmäisen henkilön sisällön siirtäminen kamera.  Vaikka edelleen kameran videomateriaali voi edelleenkään toimi, ei voi taata suorituskykyä ja Azure Media Hyperlapse Media-suoritin laatuun muiden sisältötyypeille.  Lue lisää Microsoft Hyperlapse Azure Media Services ja tarkastele Esimerkki joissakin videoissa, Kuittaa ulos [Johdanto blogimerkintä](http://aka.ms/azurehyperlapseblog) julkisen esikatselu.

Työn kestää kuin Azure Media Hyperlapse syötteen MP4, MOV tai WMV resurssi äänitiedoston sekä kokoonpanotiedosto, joka määrittää, mitkä kehykset videon pitäisi olla aika lakkasi olemasta voimassa ja mitä nopeus (kuten ensimmäisen 10 000 kehykset 2 x).  Tulos on vakiintunut ja aikaa lakkasi olemasta voimassa kuvakäännöksen syöttö-video.

Katso Azure Media Hyperlapse uusimmat päivitykset, [Media Services blogit](https://azure.microsoft.com/blog/topics/media-services/).

## <a name="hyperlapse-an-asset"></a>Hyperlapse amerikkalaisen

Sinun on ensin haluamasi syötteen tiedosto ladataan Azure Media-palveluita.  Lisätietoja liittyvistä lataamisesta ja hallita sisältöä käsitteistä, lue [sisällönhallinta-artikkelissa](media-services-portal-vod-get-started.md).

###  <a id="configuration"></a>Määritysten esiasetus Hyperlapse varten

Kun sisältöä on Media Services-tilin, sinun on muodostaa määritysten esiasetus.  Seuraavassa taulukossa mainitaan käyttäjän määrittämät kentät.

 Kenttä | Kuvaus
-------|-------------
StartFrame|Kehys, jossa Microsoft Hyperlapse käsittely alkaa.
NumFrames|Voit käsitellä kehysten määrä
Nopeus|Kerroin, jota käytetään nopeuttaa syötteen video.

Seuraavassa on esimerkki yhteensopiva määritystiedoston XML-ja JSON:

**XML-esiasetus:**

    <?xml version="1.0" encoding="utf-16"?>
    <Preset xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">
        <Sources>
            <Source StartFrame="0" NumFrames="10000" />
        </Sources>
        <Options>
            <Speed>12</Speed>
        </Options>
    </Preset>

**JSON esiasetus:**

    {
        "Version":1.0,
        "Sources": [
            {
                "StartFrame":0,
                "NumFrames":2147483647
            }
        ],
        "Options": {
            "Speed":1,
            "Stabilize":false
        }
    }

###  <a id="sample_code"></a>Microsoft Hyperlapse AMS .NET SDK kanssa

Seuraavalla tavalla Lataa mediatiedosto kuin sijoituksen ja luo työn Azure Media Hyperlapse Media-suoritin.

> [AZURE.NOTE] On jo alueen nimi "kontekstin" toimimaan tämän koodin CloudMediaContext.  Lisätietoja, lue [sisällönhallinta-artikkelissa](media-services-dotnet-get-started.md).

> [AZURE.NOTE] "HyperConfig" merkkijono-argumentti on tarkoitus olla yhteensopiva määrityksen ennalta määritetty JSON tai XML yllä olevien ohjeiden mukaisesti.

staattinen bool RunHyperlapseJob (merkkijonoa, tulosteen merkkijonon, merkkijonon hyperConfig) {/ / resurssi Luo tiedostosta IAsset resurssi = kontekstissa. Omaisuus. CreateAssetAndUploadSingleFile (syöttö-"Omat Hyperlapse syöte"-AssetCreationOptions.None);

käyttää esiintymät Azure Media Hyperlapse MP IMediaProcessor mp = kontekstissa. MediaProcessors. GetLatestMediaProcessorByName ("Azure Media Hyperlapse");

Luo projekti Hyperlapse tehtävän IJob työhön = kontekstissa. Työt. Luo (String.Format ("Hyperlapse {0}"-syötteen));

Jos (String.IsNullOrEmpty(hyperConfig)) {/ / config ei voi olla tyhjä palauttaa false;}

hyperConfig = File.ReadAllText(hyperConfig);

ITask hyperlapseTask = työn. Tasks.AddNew ("Hyperlapse tehtävän", VO, hyperConfig, TaskOptions.None); hyperlapseTask.InputAssets.Add(asset); hyperlapseTask.OutputAssets.AddNew ("Hyperlapse tulosteesta", AssetCreationOptions.None).


työn. Submit();

Luo edistymisen tulostaminen ja kyselyt tehtävät tehtävän progressPrintTask = uusi Task(() = > {

IJob jobQuery = null. Tee {var progressContext = kontekstin; jobQuery = progressContext.Jobs. Jos (j = > j.Id == työn. Tunnus). First(); Console.WriteLine (merkkijono. Format ("\t {1} \t {0} {2}", DateTime.Now, jobQuery.State, jobQuery.Tasks[0]. Käynnissä)). Thread.Sleep(10000); } aikana (jobQuery.State! = JobState.Finished & & jobQuery.State! = JobState.Error & & jobQuery.State! = JobState.Canceled); }); progressPrintTask.Start();

            Task progressJobTask = job.GetExecutionProgressTask(
                                                 CancellationToken.None);
            progressJobTask.Wait();

            // If job state is Error, the event handling
            // method for job progress should log errors.  Here we check
            // for error state and exit if needed.
            if (job.State == JobState.Error)
            {
                ErrorDetail error = job.Tasks.First().ErrorDetails.First();
                Console.WriteLine(string.Format("Error: {0}. {1}",
                                                error.Code,
                                                error.Message));  
                return false;                  
            }

        DownloadAsset(job.OutputMediaAssets.First(), output);
        return true;
    }

    static void DownloadAsset(IAsset asset, string outputDirectory)
    {
        foreach (IAssetFile file in asset.AssetFiles)
        {
            file.Download(Path.Combine(outputDirectory, file.Name));
        }
    }


    static IAsset CreateAssetAndUploadSingleFile(string filePath, string assetName, AssetCreationOptions options)
    {
        IAsset asset = context.Assets.Create(assetName, options);

        var assetFile = asset.AssetFiles.Create(Path.GetFileName(filePath));
        assetFile.Upload(filePath);

        return asset;
    }

    static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
    {
        var processor = context.MediaProcessors
        .Where(p => p.Name == mediaProcessorName)
        .ToList()
        .OrderBy(p => new Version(p.Version))
        .LastOrDefault();

        if (processor == null)
            throw new ArgumentException(string.Format("Unknown media processor",
                                                       mediaProcessorName));

        return processor;
    }

### <a id="file_types"></a>Tuetut tiedostotyypit

- MP4
- MOV
- WMV



##<a name="media-services-learning-paths"></a>Media-palveluiden oppimispolut

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Palautteen antaminen

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="related-links"></a>Aiheeseen liittyvät linkit

[Azure Media Services-palveluiden Analytics yleiskatsaus](media-services-analytics-overview.md)

[Azure Media Analytics esittelyt](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)
