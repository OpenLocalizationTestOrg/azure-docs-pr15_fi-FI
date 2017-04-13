<properties
    pageTitle="Tunnistaa hymiö ja tunteet kanssa Azure Media Analytics | Microsoft Azure"
    description="Tässä ohjeaiheessa kerrotaan, miten esiintyvien hymiöiden ja tunteet Azure Media Analytics kanssa."
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
    ms.date="09/26/2016"   
    ms.author="milanga;juliako;"/>
 
#<a name="detect-face-and-emotion-with-azure-media-analytics"></a>Tunnistaa hymiö ja tunteet Azure Media-analyysin avulla

##<a name="overview"></a>Yleiskatsaus

**Azure Media hymiö ilmaisimen** media-suoritin (MP) avulla voit laskea ja seurata liikkeitä mitta jopa yleisön osallistumisen ja reaktion kasvojen lausekkeiden kautta. Tämä palvelu on kaksi ominaisuudet: 

- **Hymiö tunnistus**

    Hymiö tunnistaminen löytää, ja seuraa ihmisten naamoja videon kuluessa. Useita hymiöiden voidaan havaita ja sen jälkeen voi seurata, kun ne siirtyvät ympärille, JSON-tiedoston palautettu aika ja sijainti metatietojen kanssa. Seuranta-aikana se yrittää antaa yhdenmukaisia tunnus saman hymiö samalla, kun henkilö on siirtyminen näytössä, vaikka ne ovat silmiä tai jätä lyhyesti kehys.

    >[AZURE.NOTE]Tämä services Suorita kasvojen tunnistuksen. Henkilö, joka poistuu kehyksen tai tulee silmiä varten liian pitkä kiinnitetään uuden tunnuksen, kun ne palauttavat.

- **Tunteet tunnistus**
    
    Tunteet tunnistus on valinnainen osa hymiö tunnistus Media suorittimen, joka palauttaa analyysi useita tunnesiteisiin määritteissä naamoja havaita, mukaan lukien onnea, sadness, jäänyt, anger ja lisää. 

**Azure Media hymiö ilmaisimen** MP ei tällä hetkellä esikatselu.

Tässä ohjeaiheessa tarkempia tietoja **Azure Media hymiö ilmaisimen** ja kerrotaan, miten voit käyttää sitä Media Services SDK .NET varten.

##<a name="face-detector-input-files"></a>Yleisölle tarkoitetun ilmaisimen syötteen tiedostot

Videotiedostot. Tällä hetkellä tueta seuraavissa muodoissa: MP4 MOV ja WMV.

##<a name="face-detector-output-files"></a>Yleisölle tarkoitetun ilmaisimen tiedostot

Hymiö tunnistaminen ja seuranta-Ohjelmointirajapinnan on suuren tarkkuuden hymiö sijainti tunnistaminen ja seuranta, joka tunnistaa 64 ihmisten naamoja video. Paljastavaa naamoja tiettyjen parhaat tulokset, puoli naamoja ja pieni naamoja (pienempi tai yhtä suuri 24 x 24 kuvapistettä) ei ehkä ole tarkasti.

Havaittiin ja seurattavaksi naamoja palautetaan koordinaatit (vasen, ylä, leveys ja korkeus) kanssa, joka ilmaisee naamoja kuvapistettä sekä hymiö tunnus luvun, joka ilmaisee, että yksittäiset seurannan kuvan sijainti. Hymiö tunnusnumerot ovat voi enää vaihtamaan tilanteissa, kun paljastavaa hymiö kadonneita tai päällekkäinen kehyksessä, tuloksena on joitakin henkilöiden hakeminen varattu useita tunnukset.

###<a id="output_elements"></a>JSON kohdetiedosto osat

Hymiö-tunnistaminen ja seurannan toimintoa tuloste tulos sisältää tietyn tiedoston JSON-muodossa naamoja metatietoja.

Hymiö-tunnistaminen ja seuranta JSON sisältää määritteet:

Osa|Kuvaus
---|---
Versio|Tämä tarkoittaa Video-Ohjelmointirajapinnan versio.
Aikajana|"Jakoviivat" sekunnissa videon.
Siirtymä|Tämä on aikaleimat aika siirtymä. Video-ohjelmointirajapinnan 1.0 versiossa tämä on aina 0. Tulevaisuudessa tilanteita, joissa tuetaan, tämä arvo voi muuttua.
Framerate|Kuvia sekunnissa videon.
Vaillinaiset lauseet|Metatietojen on lohkottu kutsutaan Vaillinaiset lauseet eri osiin. Kukin osa on alku, kesto, aikavälin numero ja tapahtuma(t).
Aloittaminen|'Jakoviivat' ensimmäinen tapahtuman aloitusaika.
Kesto|Osan "jakoviivat" pituus.
Väli|Tapahtuma-tekstien osan "jakoviivat" aikaväli.
Tapahtumat|Kuhunkin tapahtumaan sisältää naamoja havaita ja seurata sisällä, kesto. Matriisin tapahtumien matriisi on. Ulomman matriisin edustaa yhtä aikaväli. Sisempi taulukko koostuu 0 tai tapahtumien, joka on tapahtunut senhetkinen ajoissa. Tyhjä hakasulje [-] tarkoittaa naamoja ei tunnisteta.
TUNNUS| Teksti, jota seurataan tunnus. Tämän numeron vahingossa saattavat muuttua, jos hymiö tulee tunnistamattoman. Tietyn henkilön on oltava sama tunnus yleinen videon koko, mutta tämä voidaan taata tunnistus algoritmin (eston jne.) rajoitusten takia
X-, Y|Vasemmasta yläkulmasta X ja Y-koordinaattien, rajoita 0.0-1.0 normitettu skaalaus-ruudussa hymiö. <br/>-X ja Y koordinaatit ovat suhteellisen vaakasuuntaiseksi aina, joten jos sinulla on pysty video (tai ylösalaisin, kyseessä iOS), sinun on Transponoi koordinaatteja vastaavasti.
Leveys, korkeus|Leveyttä ja korkeutta rajoita 0.0-1.0 normitettu skaalaus-ruudussa hymiö.
facesDetected|Tämä löytyy JSON tulokset lopussa ja naamoja, algoritmin havaitut videon määrä on yhteenveto. Koska tunnukset voivat vaihtaa vahingossa, jos hymiö tulee tunnistamattoman (kuten hymiö siirtyy näytössä näyttää poissa käytöstä), tämän numeron eri aina suuri naamoja videon tosi määrän.

Hymiö ilmaisimen käyttää tekniikoita pirstoutuminen (jossa metatiedot voidaan jakaa-aikapohjaisten näkyvissä ja voit ladata vain tarvitsemasi) ja Segmentointi (jossa tapahtumat ovat jakaa siltä varalta, että he saavat liian suuri). Jotkin yksinkertaisia laskutoimituksia avulla voit muuntaa tiedot. Jos tapahtuman aloitettua 6300 (jakoviivat), jossa aikajanaa 2997 (jakoviivat/sec) ja sitten 29.97 (kehyksiä/s) framerate:

- Aloita/aika-asteikon = 2.1 sekuntia
- Sekunnit (Framerate/aika-asteikko) x = 63 kehykset

Alla on esimerkki kyselyjä JSON näyttämisestä kohti hymiö tunnistaminen ja seuranta kehyksen muoto:
    
    var faceDetectionResultJsonString = operationResult.ProcessingResult;
    var faceDetecionTracking = 
         JsonConvert.DeserializeObject<FaceDetectionResult>(faceDetectionResultJsonString, settings);


##<a name="face-detection-input-and-output-example"></a>Yleisölle tarkoitetun tunnistus syötteen ja tulosteen Esimerkki

###<a name="input-video"></a>Syötteen video

[Lisää Video](http://ampdemo.azureedge.net/azuremediaplayer.html?url=https%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fc8834d9f-0b49-4b38-bcaf-ece2746f1972%2FMicrosoft%20Convergence%202015%20%20Keynote%20Highlights.ism%2Fmanifest&amp;autoplay=false)

###<a name="task-configuration-preset"></a>Tehtävän määrittäminen (esiasetus)

Kun luot tehtävän **Azure Media hymiö ilmaisimen**, määritys esiasetus on määritettävä. Seuraavat määritykset esiasetus on vain hymiö tunnistamista varten.

    {"version":"1.0"}

###<a name="json-output"></a>JSON-tulostus

Seuraavassa esimerkissä JSON tulosteen on katkaistu.

    {
    "version": 1,
    "timescale": 30000,
    "offset": 0,
    "framerate": 29.97,
    "width": 1280,
    "height": 720,
    "fragments": [
        {
        "start": 0,
        "duration": 60060
        },
        {
        "start": 60060,
        "duration": 60060,
        "interval": 1001,
        "events": [
            [
            {
                "id": 0,
                "x": 0.519531,
                "y": 0.180556,
                "width": 0.0867188,
                "height": 0.154167
            }
            ],
            [
            {
                "id": 0,
                "x": 0.517969,
                "y": 0.181944,
                "width": 0.0867188,
                "height": 0.154167
            }
            ],
            [
            {
                "id": 0,
                "x": 0.517187,
                "y": 0.183333,
                "width": 0.0851562,
                "height": 0.151389
            }
            ],

        . . . 

##<a name="emotion-detection-input-and-output-example"></a>Tunteet tunnistus syötteen ja tulosteen Esimerkki


###<a name="input-video"></a>Syötteen video

[Lisää Video](http://ampdemo.azureedge.net/azuremediaplayer.html?url=https%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fc8834d9f-0b49-4b38-bcaf-ece2746f1972%2FMicrosoft%20Convergence%202015%20%20Keynote%20Highlights.ism%2Fmanifest&amp;autoplay=false)

###<a name="task-configuration-preset"></a>Tehtävän määrittäminen (esiasetus)

Kun luot tehtävän **Azure Media hymiö ilmaisimen**, määritys esiasetus on määritettävä. Seuraavat määritykset esiasetus määrittää luominen JSON tunteet tunnistus perusteella.
    
    {
      "version": "1.0",
      "options": {
        "aggregateEmotionWindowMs": "987",
        "mode": "aggregateEmotion",
        "aggregateEmotionIntervalMs": "342"
      }
    }


####<a name="attribute-descriptions"></a>Määritteen kuvaukset

Määritteen nimi|Kuvaus
---|---
Tila|Naamoja: Vain yleisölle tarkoitetun tunnistus <br/>AggregateEmotion: Kaikki naamoja kehyksessä palauttaa keskiarvo tunteet arvot.
AggregateEmotionWindowMs|Käytä, jos AggregateEmotion tila on valittuna. Määrittää videon kasvattamisesta kunkin kooste tuloksen millisekunteina.
AggregateEmotionIntervalMs|Käytä, jos AggregateEmotion tila on valittuna. Määrittää tuottamaan tuloksia tietoja usein.

####<a name="aggregate-defaults"></a>Kooste oletusarvot

Alla on suositeltavaa kooste-ikkuna ja aikavälin asetusten arvot. AggregateEmotionWindowMs on oltava AggregateEmotionIntervalMs yli.

   |Oletukset (s)|Max(s)|MIN(s)
---|---|---|---
AggregateEmotionWindowMs|0,5|2|0,25
AggregateEmotionIntervalMs|0,5|1|0,25

###<a name="json-output"></a>JSON-tulostus

Tulos on kooste (katkaistu) tunteet JSON:
 
    
    {
     "version": 1,
     "timescale": 30000,
     "offset": 0,
     "framerate": 29.97,
     "width": 1280,
     "height": 720,
     "fragments": [
       {
         "start": 0,
         "duration": 60060,
         "interval": 15015,
         "events": [
           [
             {
               "windowFaceDistribution": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               }
             }
           ],
           [
             {
               "windowFaceDistribution": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               }
             }
           ],
           [
             {
               "windowFaceDistribution": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               }
             }
           ],
           [
             {
               "windowFaceDistribution": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               }
             }
           ]
         ]
       },
       {
         "start": 60060,
         "duration": 60060,
         "interval": 15015,
         "events": [
           [
             {
               "windowFaceDistribution": {
                 "neutral": 1,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0.688541,
                 "happiness": 0.0586323,
                 "surprise": 0.227184,
                 "sadness": 0.00945675,
                 "anger": 0.00592107,
                 "disgust": 0.00154993,
                 "fear": 0.00450447,
                 "contempt": 0.0042109
               }
             }
           ],
           [
             {
               "windowFaceDistribution": {
                 "neutral": 1,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,


##<a name="limitations"></a>Rajoitukset

- Tuetut syötteen videomuodot ovat MP4, MOV ja WMV.
- Havaittavissa hymiö arvoalueiden on 24 x 24, 2048 x 2 048 kuvapistettä. Tämän alueen ulkopuolella naamoja ei tunnisteta.
- Kunkin videopuheluihin palauttaa naamoja enimmäismäärä on 64.
- Jotkin naamoja ei tunnisteta vuoksi teknisten haasteiden; esimerkiksi suuren hymiö kulmista (aiheuttaa otsikko) ja suuri eston. Paljastavaa ja lähellä-paljastavaa naamoja on parhaat tulokset.


## <a name="sample-code"></a>Esimerkki koodi

Seuraavat ohjelma näyttää Toimintaohje:

1. Luo sijoituksen ja Lataa mediatiedosto kohteen kyselyjä.
1. Luo projektin hymiö tunnistus tehtävän kokoonpanotiedosto, joka sisältää seuraavat json esiasetus perusteella. 
                    
        {
            "version": "1.0"
        }

1. Lataa tulosteen JSON-tiedostot. 
         
        using System;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using System.Threading;
        using System.Threading.Tasks;
        
        namespace FaceDetection
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
        
                    // Run the FaceDetection job.
                    var asset = RunFaceDetectionJob(@"C:\supportFiles\FaceDetection\BigBuckBunny.mp4",
                                                @"C:\supportFiles\FaceDetection\config.json");
        
                    // Download the job output asset.
                    DownloadAsset(asset, @"C:\supportFiles\FaceDetection\Output");
                }
        
                static IAsset RunFaceDetectionJob(string inputMediaFilePath, string configurationFile)
                {
                    // Create an asset and upload the input media file to storage.
                    IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                        "My Face Detection Input Asset",
                        AssetCreationOptions.None);
        
                    // Declare a new job.
                    IJob job = _context.Jobs.Create("My Face Detection Job");
        
                    // Get a reference to Azure Media Face Detector.
                    string MediaProcessorName = "Azure Media Face Detector";
        
                    var processor = GetLatestMediaProcessorByName(MediaProcessorName);
        
                    // Read configuration from the specified file.
                    string configuration = File.ReadAllText(configurationFile);
        
                    // Create a task with the encoding details, using a string preset.
                    ITask task = job.Tasks.AddNew("My Face Detection Task",
                        processor,
                        configuration,
                        TaskOptions.None);
        
                    // Specify the input asset.
                    task.InputAssets.Add(asset);
        
                    // Add an output asset to contain the results of the job.
                    task.OutputAssets.AddNew("My Face Detectoion Output Asset", AssetCreationOptions.None);
        
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


##<a name="media-services-learning-paths"></a>Media-palveluiden oppimispolut

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Palautteen antaminen

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="related-links"></a>Aiheeseen liittyvät linkit

[Azure Media Services-palveluiden Analytics yleiskatsaus](media-services-analytics-overview.md)

[Azure Media Analytics esittelyt](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)