<properties
    pageTitle="Yleisölle tarkoitetun redaction kanssa Azure media analytics | Microsoft Azure"
    description="Tässä ohjeaiheessa kerrotaan, miten redact naamoja Azure media analytics kanssa."
    services="media-services"
    documentationCenter=""
    authors="juliako"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/12/2016"   
    ms.author="juliako;"/>
 
#<a name="face-redaction-with-azure-media-analytics"></a>Hymiö redaction Azure media-analyysin avulla

##<a name="overview"></a>Yleiskatsaus

**Azure Media Redactor** on on [Azure Media Analytics](media-services-analytics-overview.md) media-suoritin (MP), joka tarjoaa skaalattava hymiö redaction pilveen. Hymiö redaction avulla voit muokata videon jotta pehmennystä valittujen henkilöiden sivua muodostaa. Haluat ehkä käyttää hymiö redaction palvelua julkisen turvallisuus- ja uutisia media tilanteissa. Muutaman minuutin, joka sisältää useita naamoja videomateriaali voi kestää tuntia redact manuaalisesti, mutta tämän palvelun avulla hymiö redaction prosessi edellyttää muutaman helpon vaiheen. Lisätietoja on artikkelissa [Tämä](https://azure.microsoft.com/blog/azure-media-redactor/) blogiin.

Tässä ohjeaiheessa tarkempia tietoja **Azure Media Redactor** ja kerrotaan, miten voit käyttää sitä Media Services SDK .NET varten.

**Azure Media Redactor** MP ei tällä hetkellä esikatselu.

## <a name="face-redaction-modes"></a>Hymiö redaction tilat

Kasvojen redaction toimii tunnistaminen naamoja jokaisen videon kehys ja seuraamalla hymiö objekti sekä eteenpäin ja taaksepäin aika-, niin, että sama henkilö voi pehmennetty muiden kulmista. Automaattinen redaction on monimutkainen ja ei ole aina haluamasi tulosteen tästä syystä Media Analytics tuottaa 100 %: n tarjoaa usealla tavalla, voit muokata lopputulos.

Täysin automaattinen tila on kaksi pass työnkulun joka sallii valinnan/de-selection löytyneen naamoja kautta tunnusten luettelo on. Käyttää myös, jotta satunnaisia kohti kehyksen muutoksia MP metatietojen tiedoston JSON-muodossa. Tämä työnkulku on jaettu **Analysoi** ja **Redact** . Voit yhdistää kaksi tilat yhden kerralla, joka toimii sekä tehtävät yksi työ; Tässä tilassa kutsutaan **yhdistetyn**.

###<a name="combined-mode"></a>Yhdistetyn tila

Tämä tuottaa redacted mp4 automaattisesti ilman mitään manuaalinen syöte.

Vaihe|Tiedostonimi|Huomautuksia
---|---|---
Syötteen resurssi|foo.bar|Videon WMV, MOV tai MP4-muodossa
Kirjoita config|Töiden määritysten esiasetus|{"version": "1.0',"asetukset": {"-tilassa: yhdistetty"}}
Tulosteen resurssi|foo_redacted.mp4|Videon blurring käyttöön

####<a name="input-example"></a>Kirjoita esimerkiksi:

[Tässä videossa](http://ampdemo.azureedge.net/?url=http%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fed99001d-72ee-4f91-9fc0-cd530d0adbbc%2FDancing.mp4)

####<a name="output-example"></a>Tulostusesimerkki:

[Tässä videossa](http://ampdemo.azureedge.net/?url=http%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fc6608001-e5da-429b-9ec8-d69d8f3bfc79%2Fdance_redacted.mp4)

###<a name="analyze-mode"></a>Analysoi tila

**Analysoi** kaksi pass työnkulun vaiheen tulee video input ja tuottaa JSON-tiedosto, jonka hymiö sijainnit ja kunkin olemassa hymiö jpg-kuvia.

Vaihe|Tiedostonimi|Huomautuksia
---|---|----
Syötteen resurssi|foo.bar|Videon WMV ja MPV MP4-muodossa
Kirjoita config|Töiden määritysten esiasetus|{"version": "1.0',"asetukset": {"-tilassa: analysoi"}}
Tulosteen resurssi|foo_annotations.JSON|Huomautus tiedot hymiö sijainneista JSON-muodossa. Tämä voi muokata käyttäjä voi muokata blurring rajoita ruudut. Katso esimerkki alla.
Tulosteen resurssi|foo_thumb%06d.jpg [foo_thumb000001.jpg, foo_thumb000002.jpg]|Rajatut jpg kunkin havaita hymiö, jossa numero ilmaisee näkyvät labelId

####<a name="output-example"></a>Tulostusesimerkki:

    {
      "version": 1,
      "timescale": 50,
      "offset": 0,
      "framerate": 25.0,
      "width": 1280,
      "height": 720,
      "fragments": [
        {
          "start": 0,
          "duration": 2,
          "interval": 2,
          "events": [
            [  
              {
                "id": 1,
                "x": 0.306415737,
                "y": 0.03199235,
                "width": 0.15357475,
                "height": 0.322126418
              },
              {
                "id": 2,
                "x": 0.5625317,
                "y": 0.0868245438,
                "width": 0.149155334,
                "height": 0.355517566
              }
            ]
          ]
        },

… katkaistu


###<a name="redact-mode"></a>Redact tila

Työnkulun toinen vaihe kestää on useita syötteitä, jotka on yhdistetty yhdeksi yksittäinen resurssi.

Tämä sisältää tunnukset pehmennystä, alkuperäisen videon ja huomautukset JSON luettelo. Tässä tilassa käyttää huomautukset blurring video Input.

Analysoi-pass tulosteen Sisällytä alkuperäisen videon. Videon on ladattu Redact tilassa tehtävän syötteen kohteiden siirtäminen ja valittu ensisijainen tiedosto.

Vaihe|Tiedostonimi|Huomautuksia
---|---|---
Syötteen resurssi|foo.bar|Video WMV ja MPV MP4-muodossa. Saman videokuvan kuin vaiheessa 1.
Syötteen resurssi|foo_annotations.JSON|Huomautuksia metatietojen tiedoston vaihe, valinnainen muutoksia.
Syötteen resurssi|foo_IDList.txt (valinnainen)|Valinnainen uuden rivin erotettu luettelo hymiö redact tunnukset. Jos tyhjä, tämä Blur-solun arvo kaikki naamoja.
Kirjoita config|Töiden määritysten esiasetus|{"version": "1.0',"asetukset": {"-tilassa: redact"}}
Tulosteen resurssi|foo_redacted.mp4|Videon blurring käytetty huomautusten perusteella

####<a name="example-output"></a>Esimerkki tulostus

Tämä on valittu yksi tunnuksella IDList tulosteen.

[Tässä videossa](http://ampdemo.azureedge.net/?url=http%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fad6e24a2-4f9c-46ee-9fa7-bf05e20d19ac%2Fdance_redacted1.mp4)

##<a name="attribute-descriptions"></a>Määritteen kuvaukset

Redaction MP tarjoaa suuren tarkkuuden hymiö sijainti tunnistaminen ja seuranta, joka tunnistaa 64 ihmisten naamoja videokehyksen. Paljastavaa naamoja antaa parhaat tulokset, puoli naamoja ja pieni naamoja (pienempi tai yhtä suuri 24 x 24 kuvapistettä) on hankalaa.

Havaittiin ja seurattavaksi naamoja palautetaan kanssa, joka ilmaisee naamoja sekä hymiö sijainnin koordinaatit ID-tunnus, joka ilmaisee, että yksittäiset seuranta. Hymiö tunnusnumerot ovat voi enää vaihtamaan tilanteissa, kun paljastavaa hymiö kadonneita tai päällekkäinen kehyksessä, tuloksena on joitakin henkilöiden hakeminen varattu useita tunnukset.

Etsimistä määritteiden on aiheessa [tunnistaa hymiö ja tunteet Azure Media Analytics kanssa](media-services-face-and-emotion-detection.md) .

## <a name="sample-code"></a>Esimerkki koodi

Seuraavat ohjelma näyttää Toimintaohje:

1. Luo sijoituksen ja Lataa mediatiedosto kohteen kyselyjä.
1. Luo projektin hymiö redaction tehtävän kokoonpanotiedosto, joka sisältää seuraavat json esiasetus perusteella. 
                    
        {'version':'1.0', 'options': {'mode':'combined'}}

1. Lataa tulosteen JSON-tiedostoja. 
         
        using System;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using System.Threading;
        using System.Threading.Tasks;
        
        namespace FaceRedaction
        {
            class Program
            {
                // Read values from the App.config file.
                private static readonly string _mediaServicesAccountName =
                    ConfigurationManager.AppSettings["MediaServicesAccountName"];
                private static readonly string _mediaServicesAccountKey =
                    ConfigurationManager.AppSettings["MediaServicesAccountKey"];
        
                // Field for service context.
                private static CloudMediaContext _context = null;
                private static MediaServicesCredentials _cachedCredentials = null;
        
                static void Main(string[] args)
                {
        
                    // Create and cache the Media Services credentials in a static class variable.
                    _cachedCredentials = new MediaServicesCredentials(
                                    _mediaServicesAccountName,
                                    _mediaServicesAccountKey);
                    // Used the cached credentials to create CloudMediaContext.
                    _context = new CloudMediaContext(_cachedCredentials);
        
                    // Run the FaceRedaction job.
                    var asset = RunFaceRedactionJob(@"C:\supportFiles\FaceRedaction\SomeFootage.mp4",
                                                @"C:\supportFiles\FaceRedaction\config.json");
        
                    // Download the job output asset.
                    DownloadAsset(asset, @"C:\supportFiles\FaceRedaction\Output");
                }
        
                static IAsset RunFaceRedactionJob(string inputMediaFilePath, string configurationFile)
                {
                    // Create an asset and upload the input media file to storage.
                    IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                        "My Face Redaction Input Asset",
                        AssetCreationOptions.None);
        
                    // Declare a new job.
                    IJob job = _context.Jobs.Create("My Face Redaction Job");
        
                    // Get a reference to Azure Media Redactor.
                    string MediaProcessorName = "Azure Media Redactor";
        
                    var processor = GetLatestMediaProcessorByName(MediaProcessorName);
        
                    // Read configuration from the specified file.
                    string configuration = File.ReadAllText(configurationFile);
        
                    // Create a task with the encoding details, using a string preset.
                    ITask task = job.Tasks.AddNew("My Face Redaction Task",
                        processor,
                        configuration,
                        TaskOptions.None);
        
                    // Specify the input asset.
                    task.InputAssets.Add(asset);
        
                    // Add an output asset to contain the results of the job.
                    task.OutputAssets.AddNew("My Face Redaction Output Asset", AssetCreationOptions.None);
        
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
                        ErrorDetail error = job.Tasks.First().ErrorDetails.First();
                        Console.WriteLine(string.Format("Error: {0}. {1}",
                                                        error.Code,
                                                        error.Message));
                        return null;
                    }
        
                    return job.OutputMediaAssets[0];
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
        
                static private void StateChanged(object sender, JobStateChangedEventArgs e)
                {
                    Console.WriteLine("Job state changed event:");
                    Console.WriteLine("  Previous state: " + e.PreviousState);
                    Console.WriteLine("  Current state: " + e.CurrentState);
        
                    switch (e.CurrentState)
                    {
                        case JobState.Finished:
                            Console.WriteLine();
                            Console.WriteLine("Job is finished.");
                            Console.WriteLine();
                            break;
                        case JobState.Canceling:
                        case JobState.Queued:
                        case JobState.Scheduled:
                        case JobState.Processing:
                            Console.WriteLine("Please wait...\n");
                            break;
                        case JobState.Canceled:
                        case JobState.Error:
                            // Cast sender as a job.
                            IJob job = (IJob)sender;
                            // Display or log error details as needed.
                            // LogJobStop(job.Id);
                            break;
                        default:
                            break;
                    }
                }
        
            }
        }


##<a name="next-step"></a>Seuraava vaihe

Tarkista oppimispolut Media-palveluita.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Palautteen antaminen

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="related-links"></a>Aiheeseen liittyvät linkit

[Azure Media Services-palveluiden Analytics yleiskatsaus](media-services-analytics-overview.md)

[Azure Media Analytics esittelyt](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)
