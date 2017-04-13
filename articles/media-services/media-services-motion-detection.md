<properties
    pageTitle="Tunnista liikkeet kanssa Azure Media Analytics | Microsoft Azure"
    description="Azure Media liikerata ilmaisimen media-suoritin (MP) avulla voit tunnistaa tehokkaasti osien halutut sisällä muussa pitkä ja uneventful video."
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
    ms.date="10/10/2016"  
    ms.author="milanga;juliako;"/>
 
# <a name="detect-motions-with-azure-media-analytics"></a>Tunnista liikkeet Azure Media-analyysin avulla

##<a name="overview"></a>Yleiskatsaus

**Azure Media liikerata ilmaisimen** media-suoritin (MP) avulla voit tunnistaa tehokkaasti osien halutut sisällä muussa pitkä ja uneventful video. Liikerata-tunnistus voidaan staattiset kameran videomateriaali-tunnistavan osien videon, jossa liikerata tapahtuu. Se luo metatietojen aikaleimat ja ympäröivä alue, jossa tapahtuma tapahtui sisältävän JSON-tiedoston.

Kohdistettu kohti suojauksen videovirtaa, tämä tekniikka on voivat luokitella liikerata haluamasi tapahtuma- ja tunnistettujen varjostusten ja valaistus muutokset. Voit muodosta suojausvaroitusten kameran syötteet ilman todennäköisyydellä roskapostia ja päättömät ole merkitystä tapahtumat, mutta ei voi poimia aikaa halutut erittäin pitkä valvonta videot.

**Azure Media liikerata ilmaisimen** MP ei tällä hetkellä esikatselu.

Tässä ohjeaiheessa tarkempia tietoja **Azure Media liikerata ilmaisimen** ja kerrotaan, miten voit käyttää sitä Media Services SDK .NET varten


##<a name="motion-detector-input-files"></a>Liikeradan ilmaisimen syötteen tiedostot

Videotiedostot. Tällä hetkellä tueta seuraavissa muodoissa: MP4 MOV ja WMV.

##<a name="task-configuration-preset"></a>Tehtävän määrittäminen (esiasetus)

Kun luot tehtävän **Azure Media liikerata ilmaisimen**, määritys esiasetus on määritettävä. 

###<a name="parameters"></a>Parametrit

Voit käyttää seuraavia parametreja:

Nimi|Asetukset|Kuvaus|Oletusarvo
---|---|---|---
sensitivityLevel|Merkkijono: "pieni", "Normaali", "suuri"|Määrittää, mitkä liikkeet suojaustaso raportoidaan. Muuta tämä, jos haluat säätää tunnistettujen määrä.|"medium"
frameSamplingValue|Positiivinen kokonaisluku|Määrittää, mitkä algoritmin taajuus suoritetaan. 1 on jokaisen kehys, 2 tarkoittaa kaikissa 2. kehys ja niin edelleen.|1
detectLightChange|Totuusarvo: "tosi", "false"|Määrittää, voivatko Vaalea muutoksista ilmoitetaan tulokset|"False"
mergeTimeThreshold|Xs ja aika: Ss<br/>Esimerkki: 00:00:03|Määrittää aikaikkunan liikerata tapahtumien missä 2 tapahtumien yhdistetyt ja 1 valmiiksi välillä.|00:00:00
detectionZones|Matriisin tunnistus alueet:<br/>-Tunnistus alue on matriisikaava 3 tai Lisää asioista<br/>-Kohta on x ja y coordinate 0 1.|Tässä artikkelissa kuvataan monikulmaisen tunnistus alueilla käytettävä luettelo.<br/>Tulokset ilmoitetaan kanssa vyöhykkeet kanssa ensin yhden on 'id-tunnusta: 0|Yksittäisen vyöhyke, joka kattaa koko kehys.

###<a name="json-example"></a>JSON-Esimerkki

    
    {
      'version': '1.0',
      'options': {
        'sensitivityLevel': 'medium',
        'frameSamplingValue': 1,
        'detectLightChange': 'False',
        "mergeTimeThreshold":
        '00:00:02',
        'detectionZones': [
          [
            {'x': 0, 'y': 0},
            {'x': 0.5, 'y': 0},
            {'x': 0, 'y': 1}
           ],
          [
            {'x': 0.3, 'y': 0.3},
            {'x': 0.55, 'y': 0.3},
            {'x': 0.8, 'y': 0.3},
            {'x': 0.8, 'y': 0.55},
            {'x': 0.8, 'y': 0.8},
            {'x': 0.55, 'y': 0.8},
            {'x': 0.3, 'y': 0.8},
            {'x': 0.3, 'y': 0.55}
          ]
        ]
      }
    }


##<a name="motion-detector-output-files"></a>Liikeradan ilmaisimen tiedostot

Liikeradan tunnistustyön palauttavat JSON tiedoston tulosteen kohteen joka kuvaa liikerata-ilmoitukset ja niiden luokkien sisällä video. Tiedosto sisältää tietoja alkamisaika ja kesto liikeradan havaita videon.

Liikeradan ilmaisimen Ohjelmointirajapinnan on ilmaisimet, kun on objekteja liikkeessä kiinteä taustan video (kuten valvonta video). Liikerata-ilmaisimen on koulutus vähentää vääriä hälytyksiä, kuten valaistus ja varjostus muutokset. Nykyisen rajoituksia algoritmeista ovat yöllä vision videoita, läpikuultava objekteja ja pieni objekteja.

###<a id="output_elements"></a>JSON kohdetiedosto osat

>[AZURE.NOTE]Uusimmassa versiossa tulosteen JSON-muoto on muutettu ja jakautumisen muuttaminen voi aiheuttaa joidenkin asiakkaiden.

Seuraavassa taulukossa on kuvattu JSON kohdetiedosto osat.

Osa|Kuvaus
---|---
Versio|Tämä tarkoittaa Video-Ohjelmointirajapinnan versio. Nykyinen versio on 2.
Aikajana|"Jakoviivat" sekunnissa videon.
Siirtymä|"Jakoviivat" aikaleimat aika siirtymä. Video-ohjelmointirajapinnan 1.0 versiossa tämä on aina 0. Tulevaisuudessa tilanteita, joissa tuetaan, tämä arvo voi muuttua.
Framerate|Kuvia sekunnissa videon.
Leveys, korkeus|Viittaa leveys ja korkeus kuvapisteinä videon.
Aloittaminen|Käynnistä-aikaleima "jakoviivat".
Kesto|Tapahtuman "jakoviivat" pituus.
Väli|Tekstien tapauksessa "jakoviivat" aikaväli.
Tapahtumat|Jokaisen tapahtuman fragmentti on havainnut sisällä, kesto liikerata.
Tyyppi|Nykyisessä versiossa tämä on aina "2" Yleinen liikerata. Tämän otsikon antaa Video ohjelmointirajapinnan joustavuutta luokittelemista varten liikeradan tulevissa versioissa.
RegionID|Edellä kuvatulla tämä on aina 0 tässä versiossa. Tämän tarran antaa Video API joustavasti liikerata etsiminen eri alueiden tulevissa versioissa.
Alueiden|Viittaa videon alue, jossa liikerata tärkeisiin. <br/><br/>-"tunnus" edustaa alue-alueen – tässä versiossa on vain yksi tunnus on 0. <br/>-"tyyppi" edustaa tärkeisiin liikerata alueen muodon. Tällä hetkellä tueta "suorakulmio" ja "monikulmio".<br/> Jos olet määrittänyt "suorakulmio", alue on mittoja X Y, leveys ja korkeus. X ja Y-koordinaatteja edustavat normitettu laajuuden 0.0-1.0 alueen ylemmässä vasemmalla olevasta XY-koordinaattien. Leveys ja korkeus edustavat normitettu laajuuden 0.0-1.0-alueen kokoa. Nykyisessä versiossa X, Y, leveys ja korkeus on aina kiinteä 0, 0 ja 1, 1. <br/>Jos olet määrittänyt "monikulmio", alue on mitat pisteinä. <br/>
Vaillinaiset lauseet|Metatietojen on lohkottu kutsutaan Vaillinaiset lauseet eri osiin. Kukin osa on alku, kesto, aikavälin numero ja tapahtuma(t). Fragmentti ei tapahtumien tarkoittaa sitä, että ei tunnistettu aikana, aloitusaika ja kesto.
Hakasulkeen|Kunkin hakasulje edustaa tapahtuman yksi aikaväli. Tyhjä hakasulkeita aikaväliä tarkoittaa, että liike ei ole tunnistettu.
sijainnit|Tapahtumat-kohdassa uusi tapahtuma on lueteltu sijainti, jossa liikkeen tapahtui. Tämä on tarkempi kuin tunnistus vyöhykkeisiin.

Seuraavassa on esimerkki JSON tulostus

    {
      "version": 2,
      "timescale": 23976,
      "offset": 0,
      "framerate": 24,
      "width": 1280,
      "height": 720,
      "regions": [
        {
          "id": 0,
          "type": "polygon",
          "points": [{'x': 0, 'y': 0},
            {'x': 0.5, 'y': 0},
            {'x': 0, 'y': 1}]
        }
      ],
      "fragments": [
        {
          "start": 0,
          "duration": 226765
        },
        {
          "start": 226765,
          "duration": 47952,
          "interval": 999,
          "events": [
            [
              {
                "type": 2,
                "typeName": "motion",
                "locations": [
                  {
                    "x": 0.004184,
                    "y": 0.007463,
                    "width": 0.991667,
                    "height": 0.985185
                  }
                ],
                "regionId": 0
              }
            ],
    
    …
##<a name="limitations"></a>Rajoitukset

- Tuetut syötteen videomuodot ovat MP4, MOV ja WMV.
- Liikerata-tunnistus on tarkoitettu paikallaan taustan videot. Algoritmin keskitytään pienentämisestä vääriä hälytyksiä, kuten valaistus muutokset ja varjostusta.
- Jotkin liike ei tunnisteta vuoksi teknisten haasteiden; esimerkiksi yöllä vision videoita, läpikuultava objekteja ja pieni objekteja.


## <a name="sample-code"></a>Esimerkki koodi

Seuraavat ohjelma näyttää Toimintaohje:

1. Luo sijoituksen ja Lataa mediatiedosto kohteen kyselyjä.
1. Luo projektin videokuvan liike tunnistus tehtävän kokoonpanotiedosto, joka sisältää seuraavat json esiasetus perusteella. 
                    
        {
          'Version': '1.0',
          'Options': {
            'SensitivityLevel': 'medium',
            'FrameSamplingValue': 1,
            'DetectLightChange': 'False',
            "MergeTimeThreshold":
            '00:00:02',
            'DetectionZones': [
              [
                {'x': 0, 'y': 0},
                {'x': 0.5, 'y': 0},
                {'x': 0, 'y': 1}
               ],
              [
                {'x': 0.3, 'y': 0.3},
                {'x': 0.55, 'y': 0.3},
                {'x': 0.8, 'y': 0.3},
                {'x': 0.8, 'y': 0.55},
                {'x': 0.8, 'y': 0.8},
                {'x': 0.55, 'y': 0.8},
                {'x': 0.3, 'y': 0.8},
                {'x': 0.3, 'y': 0.55}
              ]
            ]
          }
        }

1. Lataa tulosteen JSON-tiedostot. 
         
        using System;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using System.Threading;
        using System.Threading.Tasks;
        
        namespace VideoMotionDetection
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
        
                    // Run the VideoMotionDetection job.
                    var asset = RunVideoMotionDetectionJob(@"C:\supportFiles\VideoMotionDetection\BigBuckBunny.mp4",
                                                @"C:\supportFiles\VideoMotionDetection\config.json");
        
                    // Download the job output asset.
                    DownloadAsset(asset, @"C:\supportFiles\VideoMotionDetection\Output");
                }
        
                static IAsset RunVideoMotionDetectionJob(string inputMediaFilePath, string configurationFile)
                {
                    // Create an asset and upload the input media file to storage.
                    IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                        "My Video Motion Detection Input Asset",
                        AssetCreationOptions.None);
        
                    // Declare a new job.
                    IJob job = _context.Jobs.Create("My Video Motion Detection Job");
        
                    // Get a reference to Azure Media Motion Detector.
                    string MediaProcessorName = "Azure Media Motion Detector";
        
                    var processor = GetLatestMediaProcessorByName(MediaProcessorName);
        
                    // Read configuration from the specified file.
                    string configuration = File.ReadAllText(configurationFile);
        
                    // Create a task with the encoding details, using a string preset.
                    ITask task = job.Tasks.AddNew("My Video Motion Detection Task",
                        processor,
                        configuration,
                        TaskOptions.None);
        
                    // Specify the input asset.
                    task.InputAssets.Add(asset);
        
                    // Add an output asset to contain the results of the job.
                    task.OutputAssets.AddNew("My Video Motion Detectoion Output Asset", AssetCreationOptions.None);
        
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
[Azure Media Services liikerata ilmaisimen-blogi](https://azure.microsoft.com/blog/motion-detector-update/)

[Azure Media Services-palveluiden Analytics yleiskatsaus](media-services-analytics-overview.md)

[Azure Media Analytics esittelyt](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)
