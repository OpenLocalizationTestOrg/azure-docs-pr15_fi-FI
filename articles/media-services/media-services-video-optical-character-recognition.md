<properties
    pageTitle="Muuntaa tekstin sisällön videotiedostoja digitaalisen tekstiksi Azure Media Analytics avulla | Microsoft Azure"
    description="Azure Media Analytics OCR (optisen merkkien tunnistuksen) avulla voit muuntaa videotiedostoja sisällön tekstissä digitaalisen tekstiä voi muokata, etsittävän.  Näin voit automatisoida kuvaava metatietojen purkaminen videon signaalin mediatiedostot."
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
    ms.author="juliako"/>
 
#<a name="use-azure-media-analytics-to-convert-text-content-in-video-files-into-digital-text"></a>Azure Media-analyysin avulla voit muuntaa videotiedostoja sisällön tekstissä digitaalisen tekstiksi 

##<a name="overview"></a>Yleiskatsaus

Jos haluat poimia tekstiä videotiedostot ja luo muokattavaksi, etsittävän digitaalisen tekstiä, käytä Azure Media Analytics OCR (optisen merkkien tunnistuksen). Azure Media suorittimen havaitsee sinun videotiedostoja tekstisisältöön ja luo tekstitiedostoista käyttöä varten. OCR: N avulla voit automatisoida kuvaava metatietojen purkaminen videon signaalin mediatiedostot.

Käytettäessä yhdessä hakukone, voit helposti indeksoida mediatiedostojen tekstin mukaan, ja parantaa sisältöä löydettävyys. Tämä on erittäin hyödyllinen erittäin tekstimuotoinen video, kuten videotallenteet tai näyttökuvan-diaesityksen esityksen. Azure OCR Media-suoritin on tarkoitettu digitaalisen teksti.

**Azure Media OCR** media-suoritin ei tällä hetkellä esikatselu.

Tässä ohjeaiheessa tarkempia tietoja **Azure Media OCR** ja kerrotaan, miten voit käyttää sitä Media Services SDK .NET varten. Lisätietoja sekä esimerkkejä blogimerkinnässä [tätä](https://azure.microsoft.com/blog/announcing-video-ocr-public-preview-new-config/)painiketta.

##<a name="ocr-input-files"></a>Syötteen OCR-tiedostot

Videotiedostot. Tällä hetkellä tueta seuraavissa muodoissa: MP4 MOV ja WMV.

##<a name="task-configuration"></a>Tehtävän määrittäminen 

Tehtävän määrittäminen (esiasetus). Ennalta määritetty JSON tai XML määrityksen on määritettävä luotaessa tehtävän **Azure Media**OCR. 

###<a name="attribute-descriptions"></a>Määritteen kuvaukset

Määritteen nimi|Kuvaus
---|---
Kieli|(valinnainen) kuvaa, jonka haluat näyttää tekstin kielen. Jokin seuraavista: automaattinen tunnistaminen (oletus), arabia, ChineseSimplified, ChineseTraditional, tšekki Tanska, hollanti, englanti, Suomi, ranska, saksa, kreikka, unkari, italia, japani, korea, Norja, puola, portugali, Romania, venäjä, SerbianCyrillic, SerbianLatin, slovakki, espanja, Ruotsi, Turkki.
TextOrientation|(valinnainen) kuvaa, jonka haluat näyttää tekstin suuntaa.  "Vasen" tarkoittaa, että kaikki kirjaimet alkuun on osoitettu vasemmalta oikealle.  Oletusteksti (kuten, joka löytyy kirjan) voidaan kutsua "Asetukset" pystysuuntaisen.  Jokin seuraavista: automaattinen tunnistaminen (oletus), oikea, alas, vasemmalle.
TimeInterval|(valinnainen) kuvataan esimerkkejä korko.  Oletusarvo on 1/2 sekunnin välein.<br/>JSON-muoto – ss. SSS (oletus 00:00:00.500)<br/>XML-tiedostomuodon – W3C XSD kesto alkuperäisten (oletus PT0.5)
DetectRegions|(valinnainen) Matriisin alueita videokuvan, jolla havaitse tekstiä, joka määrittää DetectRegion objekteja.<br/>DetectRegion-objekti on valmistettu neljä seuraavista arvoista:<br/>Vasen – vasemmalle-reunuksesta pikseliä<br/>Ylä – ylä-reunuksesta pikseliä<br/>Leveys – leveyttä kuvapisteinä alue<br/>Korkeus – korkeus kuvapisteinä alue

####<a name="json-preset-example"></a>JSON esiasetus Esimerkki
        
    {
        'Version':'1.0', 
        'Options': 
        {
        'Language':'English', 
            'TimeInterval':'00:00:01.5',
            'DetectRegions': 
             [
                {'Left':'1','Top':'1','Width':'1','Height':'1'},
                {'Left':'2','Top':'2','Width':'2','Height':'2'}
             ],
            'TextOrientation':'Up'
        }
    }

####<a name="xml-preset-example"></a>Ennalta määritetty XML-esimerkki

    <?xml version=""1.0"" encoding=""utf-16""?>
    <VideoOcrPreset xmlns:xsi=""http://www.w3.org/2001/XMLSchema-instance"" xmlns:xsd=""http://www.w3.org/2001/XMLSchema"" Version=""1.0"" xmlns=""http://www.windowsazure.com/media/encoding/Preset/2014/03"">
      <Options>
         <Language>English</Language>
         <TimeInterval>PT1.5S</TimeInterval>
         <DetectRegions>
             <DetectRegion>
                   <Left>1</Left>
                   <Top>1</Top>
                   <Width>1</Width>
                   <Height>1</Height>
            </DetectRegion>
       </DetectRegions>
       <TextOrientation>Up</TextOrientation>
      </Options>
    </VideoOcrPreset>

##<a name="ocr-output-files"></a>OCR tiedostot

OCR media-suoritin tulos on JSON-tiedosto.

###<a name="elements-of-the-output-json-file"></a>JSON kohdetiedosto osat

Videon OCR tulos sisältää ajan Segmentoitu tietojen videoon kirjaimia.  Voit käyttää määritteitä, esimerkiksi kielen ja suunnan hone sisään tarkalleen siinä muodossa, että olet kiinnostunut analysoiminen sanoja. 

Tulos sisältää seuraavat ominaisuudet:

Osa|Kuvaus
---|---
Aikajana|"jakoviivat" videon sekunnissa
Siirtymä|aika poikkeama aikaleimat. Video-ohjelmointirajapinnan 1.0 versiossa tämä on aina 0.
Framerate|Kuvaa videon sekunnissa
leveys|leveys kuvapisteinä videon
korkeus|korkeus kuvapisteinä videon
Vaillinaiset lauseet|taulukko, johon metatiedot lohkottu videon aikapohjaisten näkyvissä
aloittaminen|Aloitusaika "jakoviivat"-osa
kesto|"jakoviivat"-osan pituus
väli|tietyn osan sisällä kuhunkin tapahtumaan väli
tapahtumat|taulukon, joka sisältää alueet
alue|objekti, joka edustaa havaita sanoja tai lauseita
kieli|havaittu alueella tekstin kielen mukaan
suunta|havaittu alueella tekstin suunta
rivit|matriisin havaita alueella tekstirivejä.
teksti|leipäteksti

###<a name="json-output-example"></a>JSON tulostus-Esimerkki

Seuraavassa esimerkissä tulos on Yleiset videon tietoja ja useita videon Vaillinaiset lauseet. Jokaisen videon fragmentti se sisältää jokaisen aluetta, joka tunnistaa OCR MP-kielellä ja sen tekstin suuntaa. Alue sisältää myös word jokaisen rivin tällä alueella rivin tekstiä, rivin sijainti ja tämän rivin jokaisen word-tiedot (word sisältöä, sijainti ja LUOTTAMUSVÄLI). Seuraavassa on esimerkki ja voin siirtää joitakin kommentit-riveillä.

    {
        "version": 1, 
        "timescale": 90000, 
        "offset": 0, 
        "framerate": 30, 
        "width": 640, 
        "height": 480,  // general video information
        "fragments": [
            {
                "start": 0, 
                "duration": 180000, 
                "interval": 90000,  // the time information about this fragment
                "events": [
                    [
                       { 
                            "region": { // the detected region array in this fragment 
                                "language": "English",  // region language
                                "orientation": "Up",  // text orientation
                                "lines": [  // line information array in this region, including the text and the position
                                    {
                                        "text": "One Two", 
                                        "left": 10, 
                                        "top": 10, 
                                        "right": 210, 
                                        "bottom": 110, 
                                        "word": [  // word information array in this line
                                            {
                                                "text": "One", 
                                                "left": 10, 
                                                "top": 10, 
                                                "right": 110, 
                                                "bottom": 110, 
                                                "confidence": 900
                                            }, 
                                            {
                                                "text": "Two", 
                                                "left": 110, 
                                                "top": 10, 
                                                "right": 210, 
                                                "bottom": 110, 
                                                "confidence": 910
                                            }
                                        ]
                                    }
                                ]
                            }
                        }
                    ]
                ]
            }
        ]
    }

## <a name="sample-code"></a>Esimerkki koodi

Seuraavat ohjelma näyttää Toimintaohje:

1. Luo sijoituksen ja Lataa mediatiedosto kohteen kyselyjä.
1. Luo projektin OCR määritysten/esiasetus-tiedosto.
1. Lataa tulosteen JSON-tiedostot. 
         
        using System;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using System.Threading;
        using System.Threading.Tasks;
        
        namespace OCR
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
        
                    // Run the OCR job.
                    var asset = RunOCRJob(@"C:\supportFiles\OCR\presentation.mp4",
                                                @"C:\supportFiles\OCR\config.json");
        
                    // Download the job output asset.
                    DownloadAsset(asset, @"C:\supportFiles\OCR\Output");
                }
        
                static IAsset RunOCRJob(string inputMediaFilePath, string configurationFile)
                {
                    // Create an asset and upload the input media file to storage.
                    IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                        "My OCR Input Asset",
                        AssetCreationOptions.None);
        
                    // Declare a new job.
                    IJob job = _context.Jobs.Create("My OCR Job");
        
                    // Get a reference to Azure Media OCR.
                    string MediaProcessorName = "Azure Media OCR";
        
                    var processor = GetLatestMediaProcessorByName(MediaProcessorName);
        
                    // Read configuration from the specified file.
                    string configuration = File.ReadAllText(configurationFile);
        
                    // Create a task with the encoding details, using a string preset.
                    ITask task = job.Tasks.AddNew("My OCR Task",
                        processor,
                        configuration,
                        TaskOptions.None);
        
                    // Specify the input asset.
                    task.InputAssets.Add(asset);
        
                    // Add an output asset to contain the results of the job.
                    task.OutputAssets.AddNew("My OCR Output Asset", AssetCreationOptions.None);
        
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
