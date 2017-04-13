<properties 
    pageTitle="Laajennettu käytettäessä Media Encoder Standard-koodaus" 
    description="Tässä ohjeaiheessa esitellään tietoja Lisäasetukset koodaus mukauttamalla Media Encoder vakio tehtävän esiasetuksia. Aihe näkyy käyttämisestä Media Services .NET SDK koodauksen tehtävän ja projektin luomiseen. Se myös näyttää, miten toimittamaan mukautetun Esiasetukset koodauksen projektille." 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/25/2016"   
    ms.author="juliako"/>


#<a name="advanced-encoding-with-media-encoder-standard"></a>Laajennettu käytettäessä Media Encoder Standard-koodaus

##<a name="overview"></a>Yleiskatsaus

Tässä ohjeaiheessa kerrotaan, miten tehtäviä Lisäasetukset koodauksen Media Encoder vakio. Aihe näkyy [käyttämisestä .NET koodauksen tehtävän ja työ, joka suorittaa tämän tehtävän](media-services-custom-mes-presets-with-dotnet.md#encoding_with_dotnet). Se näyttää myös siitä, miten toimittamaan mukautetun Esiasetukset koodauksen tehtävään. Kuvaus elementtejä, jotka käyttävät Esiasetukset-kohdassa [tämän asiakirjan](https://msdn.microsoft.com/library/mt269962.aspx). 

Mukautetun Esiasetukset, suorita seuraavat toimet koodaus on osoittanut:

- [Luo pienoiskuvat](media-services-custom-mes-presets-with-dotnet.md#thumbnails)
- [Videon (näyttöleikkeen)](media-services-custom-mes-presets-with-dotnet.md#trim_video)
- [Luo kyselynopeustiedot](media-services-custom-mes-presets-with-dotnet.md#overlay)
- [Lisää hiljainen ääniraidan, kun syöte on ilman ääntä](media-services-custom-mes-presets-with-dotnet.md#silent_audio)
- [Automaattinen varaustiedoista lomituksen poistaminen käytöstä](media-services-custom-mes-presets-with-dotnet.md#deinterlacing)
- [Pelkän Esiasetukset](media-services-custom-mes-presets-with-dotnet.md#audio_only)
- [KETJUTA kahden tai useamman videotiedostot](media-services-custom-mes-presets-with-dotnet.md#concatenate)
- [Rajaa videoiden katselusta Media Encoder vakio](media-services-custom-mes-presets-with-dotnet.md#crop)
- [Lisää video seuraa, kun syöte on videota ei](media-services-custom-mes-presets-with-dotnet.md#no_video)
- [Videon kiertäminen](media-services-custom-mes-presets-with-dotnet.md#rotate_video)

##<a id="encoding_with_dotnet"></a>Media-palveluiden .NET SDK-koodaus

Seuraava koodiesimerkki käyttää Media Services .NET SDK voit suorittaa seuraavia tehtäviä:

- Luo koodauksen työ.
- Viittaaminen Media Encoder vakio encoder.
- Lataa mukautetut XML- tai JSON esiasetus. Voit tallentaa XML tai JSON (esimerkiksi [XML](media-services-custom-mes-presets-with-dotnet.md#xml) - tai tiedosto ja käytä seuraava koodi [JSON](media-services-custom-mes-presets-with-dotnet.md#json) -tiedoston lataaminen.

        // Load the XML (or JSON) from the local file.
        string configuration = File.ReadAllText(fileName);  
- Lisää koodauksen tehtävän työn. 
- Määritä koodattava syötteen resurssi.
- Luo tulostus-resurssi, joka sisältää koodattu resurssi.
- Lisää tapahtumakäsittelijän tarkistamaan työn edistymistä.
- Lähetä työn.
    
        using System;
        using System.Collections.Generic;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using System.Net;
        using System.Security.Cryptography;
        using System.Text;
        using System.Threading.Tasks;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using Newtonsoft.Json.Linq;
        using System.Threading;
        using Microsoft.WindowsAzure.MediaServices.Client.ContentKeyAuthorization;
        using Microsoft.WindowsAzure.MediaServices.Client.DynamicEncryption;
        using System.Web;
        using System.Globalization;
        
        namespace CustomizeMESPresests
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
        
                private static readonly string _mediaFiles =
                    Path.GetFullPath(@"../..\Media");
        
                private static readonly string _singleMP4File =
                    Path.Combine(_mediaFiles, @"BigBuckBunny.mp4");
        
                static void Main(string[] args)
                {
                    // Create and cache the Media Services credentials in a static class variable.
                    _cachedCredentials = new MediaServicesCredentials(
                                    _mediaServicesAccountName,
                                    _mediaServicesAccountKey);
                    // Used the chached credentials to create CloudMediaContext.
                    _context = new CloudMediaContext(_cachedCredentials);
        
                    // Get an uploaded asset.
                    var asset = _context.Assets.FirstOrDefault();
        
                    // Encode and generate the output using custom presets.
                    EncodeToAdaptiveBitrateMP4Set(asset);
        
                    Console.ReadLine();
                }
        
                static public IAsset EncodeToAdaptiveBitrateMP4Set(IAsset asset)
                {
                    // Declare a new job.
                    IJob job = _context.Jobs.Create("Media Encoder Standard Job");
                    // Get a media processor reference, and pass to it the name of the 
                    // processor to use for the specific task.
                    IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");
                
        
                    // Load the XML (or JSON) from the local file.
                    string configuration = File.ReadAllText("CustomPreset_JSON.json");
                
                    // Create a task
                    ITask task = job.Tasks.AddNew("Media Encoder Standard encoding task",
                        processor,
                        configuration,
                        TaskOptions.None);
                
                    // Specify the input asset to be encoded.
                    task.InputAssets.Add(asset);
                    // Add an output asset to contain the results of the job. 
                    // This output is specified as AssetCreationOptions.None, which 
                    // means the output asset is not encrypted. 
                    task.OutputAssets.AddNew("Output asset",
                        AssetCreationOptions.None);
                
                    job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
                    job.Submit();
                    job.GetExecutionProgressTask(CancellationToken.None).Wait();
                
                    return job.OutputMediaAssets[0];
                }
        
                static public IAsset UploadMediaFilesFromFolder(string folderPath)
                {
                    IAsset asset = _context.Assets.CreateFromFolder(folderPath, AssetCreationOptions.None);
        
                    foreach (var af in asset.AssetFiles)
                    {
                        // The following code assumes 
                        // you have an input folder with one MP4 and one overlay image file.
                        if (af.Name.Contains(".mp4"))
                            af.IsPrimary = true;
                        else
                            af.IsPrimary = false;
        
                        af.Update();
                    }
        
                    return asset;
                }
        
        
                static public IAsset EncodeWithOverlay(IAsset assetSource, string customPresetFileName)
                {
                    // Declare a new job.
                    IJob job = _context.Jobs.Create("Media Encoder Standard Job");
                    // Get a media processor reference, and pass to it the name of the 
                    // processor to use for the specific task.
                    IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");
        
                    // Load the XML (or JSON) from the local file.
                    string configuration = File.ReadAllText(customPresetFileName);
        
                    // Create a task
                    ITask task = job.Tasks.AddNew("Media Encoder Standard encoding task",
                        processor,
                        configuration,
                        TaskOptions.None);
        
                    // Specify the input assets to be encoded.
                    // This asset contains a source file and an overlay file.
                    task.InputAssets.Add(assetSource);
        
                    // Add an output asset to contain the results of the job. 
                    task.OutputAssets.AddNew("Output asset",
                        AssetCreationOptions.None);
        
                    job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
                    job.Submit();
                    job.GetExecutionProgressTask(CancellationToken.None).Wait();
        
                    return job.OutputMediaAssets[0];
                }
        

                private static void JobStateChanged(object sender, JobStateChangedEventArgs e)
                {
                    Console.WriteLine("Job state changed event:");
                    Console.WriteLine("  Previous state: " + e.PreviousState);
                    Console.WriteLine("  Current state: " + e.CurrentState);
                    switch (e.CurrentState)
                    {
                        case JobState.Finished:
                            Console.WriteLine();
                            Console.WriteLine("Job is finished. Please wait while local tasks or downloads complete...");
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
                            break;
                        default:
                            break;
                    }
                }
        
        
                private static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
                {
                    var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
                    ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();
        
                    if (processor == null)
                        throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));
        
                    return processor;
                }
        
            }
        }


##<a name="support-for-relative-sizes"></a>Suhteellinen koot tuki

Pikkukuvat sen luotaessa sinun ei tarvitse määrittää aina tulosteen leveys ja korkeus kuvapisteinä. Voit määrittää ne prosenttien alueen [1 %,..., 100 %: n].

###<a name="json-preset"></a>JSON esiasetus 
    
    "Width": "100%",
    "Height": "100%"

###<a name="xml-preset"></a>XML-esiasetus
    
    <Width>100%</Width>
    <Height>100%</Height>
    
##<a id="thumbnails"></a>Luo pikkukuvat

Tässä osassa esitellään mukauttamisesta esiasetus, joka luo pikkukuvat. Esiasetus alla on tietoja siitä, miten haluat koodata oman tiedoston sekä pikkukuvat luomiseen tarvittavia tietoja. Voit siirtää jokin seuraavista Markkinatalousasemaa Esiasetukset kuvata [tähän](https://msdn.microsoft.com/library/mt269960.aspx) ja lisää koodi, joka luo pikkukuvat.  

>[AZURE.NOTE]TOSI, jos olet yksittäisen bittitaajuus, video-koodaus voi määrittää vain seuraavat valmiiksi määritettyä voi **SceneChangeDetection** -asetus. Jos usean bittitaajuus videon koodaus ja **SceneChangeDetection** arvoksi true, encoder palauttaa virheen.  


Lisätietoja rakenne on [Tässä](https://msdn.microsoft.com/library/mt269962.aspx) ohjeaiheessa.

Varmista, että [huomioon otettavia seikkoja](media-services-custom-mes-presets-with-dotnet.md#considerations) -osiossa.

###<a id="json"></a>JSON esiasetus


    {
      "Version": 1.0,
      "Codecs": [
        {
          "KeyFrameInterval": "00:00:02",
          "SceneChangeDetection": "true",
          "H264Layers": [
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 4500,
              "MaxBitrate": 4500,
              "BufferWindow": "00:00:05",
              "Width": 1280,
              "Height": 720,
              "ReferenceFrames": 3,
              "EntropyMode": "Cabac",
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
       
            }
          ],
          "Type": "H264Video"
        },
        {
          "JpgLayers": [
            {
              "Quality": 90,
              "Type": "JpgLayer",
              "Width": 640,
              "Height": 360
            }
          ],
          "Start": "{Best}",
          "Type": "JpgImage"
        },
        {
          "PngLayers": [
            {
              "Type": "PngLayer",
              "Width": 640,
              "Height": 360,
            }
          ],
          "Start": "00:00:01",
          "Step": "00:00:10",
          "Range": "00:00:58",
          "Type": "PngImage"
        },
        {
          "BmpLayers": [
            {
              "Type": "BmpLayer",
              "Width": 640,
              "Height": 360
            }
          ],
          "Start": "10%",
          "Step": "10%",
          "Range": "90%",
          "Type": "BmpImage"
        },
        {
          "Channels": 2,
          "SamplingRate": 48000,
          "Bitrate": 128,
          "Type": "AACAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_{Index}{Extension}",
          "Format": {
            "Type": "JpgFormat"
          }
        },
        {
          "FileName": "{Basename}_{Index}{Extension}",
          "Format": {
            "Type": "PngFormat"
          }
        },
        {
          "FileName": "{Basename}_{Index}{Extension}",
          "Format": {
            "Type": "BmpFormat"
          }
        },
        {
          "FileName": "{Basename}_{Width}x{Height}_{VideoBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }


###<a id="xml"></a>XML-esiasetus


    <?xml version="1.0" encoding="utf-16"?>
    <Preset xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">
      <Encoding>
        <H264Video>
          <KeyFrameInterval>00:00:02</KeyFrameInterval>
          <SceneChangeDetection>true</SceneChangeDetection>
          <H264Layers>
            <H264Layer>
              <Bitrate>4500</Bitrate>
              <Width>1280</Width>
              <Height>720</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>4500</MaxBitrate>
            </H264Layer>
          </H264Layers>
        </H264Video>
        <AACAudio>
          <Profile>AACLC</Profile>
          <Channels>2</Channels>
          <SamplingRate>48000</SamplingRate>
          <Bitrate>128</Bitrate>
        </AACAudio>
        <JpgImage Start="{Best}">
          <JpgLayers>
            <JpgLayer>
              <Width>640</Width>
              <Height>360</Height>
              <Quality>90</Quality>
            </JpgLayer>
          </JpgLayers>
        </JpgImage>
        <BmpImage Start="10%" Step="10%" Range="90%">
          <BmpLayers>
            <BmpLayer>
              <Width>640</Width>
              <Height>360</Height>
            </BmpLayer>
          </BmpLayers>
        </BmpImage>
        <PngImage Start="00:00:01" Step="00:00:10" Range="00:00:58">
          <PngLayers>
            <PngLayer>
              <Width>640</Width>
              <Height>360</Height>
            </PngLayer>
          </PngLayers>
        </PngImage>
      </Encoding>
      <Outputs>
        <Output FileName="{Basename}_{Width}x{Height}_{VideoBitrate}.mp4">
          <MP4Format />
        </Output>
        <Output FileName="{Basename}_{Index}{Extension}">
          <JpgFormat />
        </Output>
        <Output FileName="{Basename}_{Index}{Extension}">
          <BmpFormat />
        </Output>
        <Output FileName="{Basename}_{Index}{Extension}">
          <PngFormat />
        </Output>
      </Outputs>
    </Preset>

###<a name="considerations"></a>Huomioon otettavia seikkoja

Ottaa huomioon seuraavat asiat:

- Käynnistä/vaihe tai alue eksplisiittinen aikaleimat käyttö olettaa, että lähde vähintään yksi minuutti.
- JPG/Png/BmpImage osat on Start-vaiheessa sekä Yhteysmerkkijonon määritteet alueen – ne voidaan tulkita:

    - Kehyksen numero, jos ne ovat negatiivinen kokonaisluvut, esimerkiksi "Käynnistä": "120"
    - Suhteellinen lähde kesto, jos niin % jälkiliitteeksi, esimerkiksi "Käynnistä-:"15 %", tai
    - Jos ss valuuttamäärän aikaleima... Muotoile, esimerkiksi "Käynnistä-:" 00: 01:00 "

    Voit sekoita ja vastaa Ota muotoilun tavalliseen tapaan.
    
    Lisäksi Käynnistä tukee myös erikois makroa: {Parhaat}, joka yrittää selvittää sisältö Huomautus ensimmäinen "kiinnostava" kuva: (vaihe ja alueen ohitetaan, kun Käynnistä on määritetty {parhaiten})
    
    - Oletukset: Käynnistä: {Parhaat}
- Siirtomuoto on kuitenkin nimenomaisesti kunkin kuvatiedosto: Jpg/Png/BmpFormat. Kun se on käytettävissä, Markkinatalousasemaa vastaa JpgVideo JpgFormat ja niin edelleen. OutputFormat esitellään uusi kuva pakkauksenhallinta tietyn makron: {Index}, mikä on oltava esittää (kerran ja vain kerran) kuva siirtomuodossa.

##<a id="trim_video"></a>Videon (näyttöleikkeen)

Tässä osassa puheen encoder Esiasetukset ClipArt-tai syötteen videon, missä syötteen niin kutsutut mezzanine tiedoston tai tarvittaessa tiedoston muokkaaminen. Encoder voidaan myös ClipArt-ja trim resurssi, joka oteta talteen, tai arkistoida live virta- – tämä tiedot ovat käytettävissä [Tässä](https://azure.microsoft.com/blog/sub-clipping-and-live-archive-extraction-with-media-encoder-standard/)blogissa.

Voit leikata videoita, voit siirtää jokin seuraavista Markkinatalousasemaa Esiasetukset kuvata [tähän](https://msdn.microsoft.com/library/mt269960.aspx) ja muokata **lähteet** -osan (Katso kuvaa alla). Aloitusajan arvo on vastattava suora aikaleimat syötteen videon. Esimerkiksi jos syötteen videon ensimmäinen kuva on aikaleimaa 12:00:10.000, sitten aloitusajan on oltava vähintään 12:00:10.000 ja suurempia. Seuraavassa esimerkissä on oletetaan, että syötteen videon on ensimmäinen aikaleima nolla. **Lähteiden** sijoitetaan esiasetus alussa. 
 
###<a id="json"></a>JSON esiasetus
    
    {
      "Version": 1.0,
      "Sources": [
        {
          "StartTime": "00:00:04",
          "Duration": "00:00:16"
        }
      ],
      "Codecs": [
        {
          "KeyFrameInterval": "00:00:02",
          "StretchMode": "AutoSize",
          "H264Layers": [
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 3400,
              "MaxBitrate": 3400,
              "BufferWindow": "00:00:05",
              "Width": 1280,
              "Height": 720,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 2250,
              "MaxBitrate": 2250,
              "BufferWindow": "00:00:05",
              "Width": 960,
              "Height": 540,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 1500,
              "MaxBitrate": 1500,
              "BufferWindow": "00:00:05",
              "Width": 960,
              "Height": 540,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 1000,
              "MaxBitrate": 1000,
              "BufferWindow": "00:00:05",
              "Width": 640,
              "Height": 360,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 650,
              "MaxBitrate": 650,
              "BufferWindow": "00:00:05",
              "Width": 640,
              "Height": 360,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 400,
              "MaxBitrate": 400,
              "BufferWindow": "00:00:05",
              "Width": 320,
              "Height": 180,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            }
          ],
          "Type": "H264Video"
        },
        {
          "Profile": "AACLC",
          "Channels": 2,
          "SamplingRate": 48000,
          "Bitrate": 128,
          "Type": "AACAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_{Width}x{Height}_{VideoBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    } 

###<a name="xml-preset"></a>XML-esiasetus
    
Voit leikata videoita, voit siirtää jokin seuraavista Markkinatalousasemaa Esiasetukset kuvata [tähän](https://msdn.microsoft.com/library/mt269960.aspx) ja muokata **lähteet** -osan (Katso kuvaa alla).

    <?xml version="1.0" encoding="utf-16"?>
    <Preset xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">
      <Sources>
        <Source StartTime="PT4S" Duration="PT14S"/>
      </Sources>
      <Encoding>
        <H264Video>
          <KeyFrameInterval>00:00:02</KeyFrameInterval>
          <H264Layers>
            <H264Layer>
              <Bitrate>3400</Bitrate>
              <Width>1280</Width>
              <Height>720</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>3400</MaxBitrate>
            </H264Layer>
            <H264Layer>
              <Bitrate>2250</Bitrate>
              <Width>960</Width>
              <Height>540</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>2250</MaxBitrate>
            </H264Layer>
            <H264Layer>
              <Bitrate>1500</Bitrate>
              <Width>960</Width>
              <Height>540</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>1500</MaxBitrate>
            </H264Layer>
            <H264Layer>
              <Bitrate>1000</Bitrate>
              <Width>640</Width>
              <Height>360</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>1000</MaxBitrate>
            </H264Layer>
            <H264Layer>
              <Bitrate>650</Bitrate>
              <Width>640</Width>
              <Height>360</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>650</MaxBitrate>
            </H264Layer>
            <H264Layer>
              <Bitrate>400</Bitrate>
              <Width>320</Width>
              <Height>180</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>3</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cabac</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>400</MaxBitrate>
            </H264Layer>
          </H264Layers>
        </H264Video>
        <AACAudio>
          <Profile>AACLC</Profile>
          <Channels>2</Channels>
          <SamplingRate>48000</SamplingRate>
          <Bitrate>128</Bitrate>
        </AACAudio>
      </Encoding>
      <Outputs>
        <Output FileName="{Basename}_{Width}x{Height}_{VideoBitrate}.mp4">
          <MP4Format />
        </Output>
      </Outputs>
    </Preset>

##<a id="overlay"></a>Luo kyselynopeustiedot

Media Encoder vakio avulla voit sivulle aiemmin luodun videon kuvan päälle. Tällä hetkellä tueta seuraavissa muodoissa: png, jpg, gif- ja bmp. Jäljempänä esiasetus on basic Esimerkki videon päällekkäin.

Lisäksi määrittäminen valmiiksi määritetyn tiedoston, sinun on myös antaa Media Services tietää minkä kohteen tiedosto on kerros-kuva ja tiedoston on videon tietolähde sivulle, johon haluat kuvan päälle. Videotiedoston on oltava **ensisijainen** tiedosto. 

Yllä olevassa esimerkissä .NET määrittää kahden funktion: **UploadMediaFilesFromFolder** ja **EncodeWithOverlay**. UploadMediaFilesFromFolder-funktio Lataa tiedostot-kansion (esimerkiksi BigBuckBunny.mp4 ja Image001.png) ja asettaa mp4-kohteen ensisijainen-tiedosto. **EncodeWithOverlay** -funktiossa käytetään mukautettuja valmiiksi määritettyä tiedosto, joka on välitetty siihen (esimerkiksi esiasetus, joka seuraa) luo koodauksen tehtävä. 

>[AZURE.NOTE]Nykyinen rajoitukset:
>
>Kerros läpikuultavuus-asetusta ei tueta.
>
>Videon lähdetiedoston ja kerros kuvatiedoston on oltava sama resurssi- ja videotiedostoa on määritettävä kohteen ensisijainen tiedostona.

###<a name="json-preset"></a>JSON esiasetus
    
    {
      "Version": 1.0,
      "Sources": [
        {
          "Streams": [],
          "Filters": {
            "VideoOverlay": {
              "Position": {
                "X": 100,
                "Y": 100,
                "Width": 100,
                "Height": 50
              },
              "AudioGainLevel": 0.0,
              "MediaParams": [
                {
                  "OverlayLoopCount": 1
                },
                {
                  "IsOverlay": true,
                  "OverlayLoopCount": 1,
                  "InputLoop": true
                }
              ],
              "Source": "Image001.png",
              "Clip": {
                "Duration": "00:00:05"
              },
              "FadeInDuration": {
                "Duration": "00:00:01"
              },
              "FadeOutDuration": {
                "StartTime": "00:00:03",
                "Duration": "00:00:04"
              }
            }
          },
          "Pad": true
        }
      ],
      "Codecs": [
        {
          "KeyFrameInterval": "00:00:02",
          "H264Layers": [
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 1045,
              "MaxBitrate": 1045,
              "BufferWindow": "00:00:05",
              "ReferenceFrames": 3,
              "EntropyMode": "Cavlc",
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "Width": "640",
              "Height": "360",
              "FrameRate": "0/1"
            }
          ],
          "Type": "H264Video"
        },
        {
          "Type": "CopyAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}{Extension}",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }


###<a name="xml-preset"></a>XML-esiasetus
    
    <?xml version="1.0" encoding="utf-16"?>
    <Preset xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">
      <Sources>
        <Source>
          <Streams />
          <Filters>
            <VideoOverlay>
              <Source>Image001.png</Source>
              <Clip Duration="PT5S" />
              <FadeInDuration Duration="PT1S" />
              <FadeOutDuration StartTime="PT3S" Duration="PT4S" />
              <Position X="100" Y="100" Width="100" Height="50" />
              <Opacity>0</Opacity>
              <AudioGainLevel>0</AudioGainLevel>
              <MediaParams>
                <MediaParam>
                  <IsOverlay>false</IsOverlay>
                  <OverlayLoopCount>1</OverlayLoopCount>
                  <InputLoop>false</InputLoop>
                </MediaParam>
                <MediaParam>
                  <IsOverlay>true</IsOverlay>
                  <OverlayLoopCount>1</OverlayLoopCount>
                  <InputLoop>true</InputLoop>
                </MediaParam>
              </MediaParams>
            </VideoOverlay>
          </Filters>
          <Pad>true</Pad>
        </Source>
      </Sources>
      <Encoding>
        <H264Video>
          <KeyFrameInterval>00:00:02</KeyFrameInterval>
          <H264Layers>
            <H264Layer>
              <Bitrate>1045</Bitrate>
              <Width>640</Width>
              <Height>360</Height>
              <FrameRate>0/1</FrameRate>
              <Profile>Auto</Profile>
              <Level>auto</Level>
              <BFrames>0</BFrames>
              <ReferenceFrames>3</ReferenceFrames>
              <Slices>0</Slices>
              <AdaptiveBFrame>true</AdaptiveBFrame>
              <EntropyMode>Cavlc</EntropyMode>
              <BufferWindow>00:00:05</BufferWindow>
              <MaxBitrate>1045</MaxBitrate>
            </H264Layer>
          </H264Layers>
        </H264Video>
        <CopyAudio />
      </Encoding>
      <Outputs>
        <Output FileName="{Basename}{Extension}">
          <MP4Format />
        </Output>
      </Outputs>
    </Preset>


##<a id="silent_audio"></a>Lisää hiljainen ääniraidan, kun syöte on ilman ääntä

Oletusarvon mukaan Jos lähetät encoder, joka sisältää vain video ja ei ole ääni syötteen sitten tulosteen resurssi sisältää tiedostot, jotka sisältävät vain videon tiedot. Jotkin pelaajat ei ehkä käsittelemään esimerkiksi tulostus virtaa. Tämän asetuksen avulla voit pakottaa encoder hiljainen ääniraidan lisääminen skenaarion tulos.

Voit pakottaa encoder tuottamaan resurssi, joka sisältää hiljainen ääniraidan, kun syöte on ilman ääntä, Määritä "InsertSilenceIfNoAudio"-arvo.

Voit tehdä mitään Markkinatalousasemaa Esiasetukset kuvata [tähän](https://msdn.microsoft.com/library/mt269960.aspx)ja tee seuraavat muutokset:

###<a name="json-preset"></a>JSON esiasetus

    {
      "Channels": 2,
      "SamplingRate": 44100,
      "Bitrate": 96,
      "Type": "AACAudio",
      "Condition": "InsertSilenceIfNoAudio"
    }

###<a name="xml-preset"></a>XML-esiasetus

    <AACAudio Condition="InsertSilenceIfNoAudio">
      <Channels>2</Channels>
      <SamplingRate>44100</SamplingRate>
      <Bitrate>96</Bitrate>
    </AACAudio>

##<a id="deinterlacing"></a>Automaattinen varaustiedoista lomituksen poistaminen käytöstä

Asiakkaiden ei tarvitse tehdä mitään, jos ne on automaattisesti varaustiedoista lomitettu interlace sisältöä, kuten. Kun automaattinen varaustiedoista lomituksen on käytössä (oletus) Markkinatalousasemaa ei automaattinen tunnistus lomitettu kehysten ja interlaces kehykset merkitty lomitettu vain varaustiedoista.

Voit poistaa automaattisen varaustiedoista lomituksen käytöstä. Tämä vaihtoehto ei ole suositeltavaa.

###<a name="json-preset"></a>JSON esiasetus
    
    "Sources": [
    {
     "Filters": {
        "Deinterlace": {
          "Mode": "Off"
        }
      },
    }
    ]

###<a name="xml-preset"></a>XML-esiasetus
    
    <Sources>
    <Source>
      <Filters>
        <Deinterlace>
          <Mode>Off</Mode>
        </Deinterlace>
      </Filters>
    </Source>
    </Sources>


##<a id="audio_only"></a>Pelkän Esiasetukset

Tässä osassa esitellään kaksi pelkän Markkinatalousasemaa Esiasetukset: AAC ääni- ja AAC laatu hyvä ääni.

###<a name="aac-audio"></a>AAC ääni 

    {
      "Version": 1.0,
      "Codecs": [
        {
          "Profile": "AACLC",
          "Channels": 2,
          "SamplingRate": 48000,
          "Bitrate": 128,
          "Type": "AACAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_AAC_{AudioBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }

###<a name="aac-good-quality-audio"></a>AAC laadukkaan ääni

    {
      "Version": 1.0,
      "Codecs": [
        {
          "Profile": "AACLC",
          "Channels": 2,
          "SamplingRate": 48000,
          "Bitrate": 192,
          "Type": "AACAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_AAC_{AudioBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }

##<a id="concatenate"></a>KETJUTA kahden tai useamman videotiedostot

Seuraavassa esimerkissä kuvataan, miten voit luoda esiasetus käyttäisit kahden tai useamman videotiedostot. Useimmin käytetty vaihtoehto on, kun haluat lisätä ylä- tai perävaunua tärkeimmät video. Käyttötarkoitus on, kun muokataan yhdessä videotiedostojen jakaminen ominaisuudet (näytön tarkkuus, kehyksen korko, ääniraidan Laske jne.). Sinun pitäisi huolehtia halua sekoita videot eri kehyksessä korkojen tai ääniraitojen eri määrän.

###<a name="requirements-and-considerations"></a>Vaatimukset ja huomioon otettavia seikkoja

- Syötteen videot tulee olla vain yksi ääniraidan.
- Kaikki syötteen videot pitäisi olla sama kehys-korvaus.
- Täytyy ladata videoiden esittely erillisessä kalusto ja määritä videot kunkin resurssi ensisijainen tiedostona.
- Sinun tarvitsee tietää videoiden toistaminen.
- Valmiiksi määritettyä Esimerkeissä oletetaan, että syötteen videot aloittaminen aikaleima nolla. Haluat muokata aloitusajan arvot, jos videot on eri aloitus aikaleima on yleensä live arkistot tapauksessa.
- JSON-esiasetus on eksplisiittinen viittauksia syötteen varat aihetunnus arvot.
- Sample code oletetaan, että JSON esiasetus on tallennettu paikalliseen tiedostoon, kuten "C:\supportFiles\preset.json". Se myös olettaa kaksi varat on luotu kaksi videotiedostoja lataamalla ja tiedät voimassa oleva aihetunnus arvot.
- Koodikatkelman ja JSON esiasetus Esimerkki ketjuttaa kaksi videotiedostot. Voit laajentaa sen enemmän kuin kaksi videot mukaan:

    1. Soittavan tehtävän. InputAssets.Add() toistuvasti, jos haluat lisätä Lisää videoita, järjestyksessä.
    2. "Lähteistä" elementtiin JSON, että vastaavat Muokkaa lisäämällä siihen merkintöjä, samassa järjestyksessä. 


###<a name="net-code"></a>.NET-koodi

    
    IAsset asset1 = _context.Assets.Where(asset => asset.Id == "nb:cid:UUID:606db602-efd7-4436-97b4-c0b867ba195b").FirstOrDefault();
    IAsset asset2 = _context.Assets.Where(asset => asset.Id == "nb:cid:UUID:a7e2b90f-0565-4a94-87fe-0a9fa07b9c7e").FirstOrDefault();
    
    // Declare a new job.
    IJob job = _context.Jobs.Create("Media Encoder Standard Job for Concatenating Videos");
    // Get a media processor reference, and pass to it the name of the 
    // processor to use for the specific task.
    IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");
    
    // Load the XML (or JSON) from the local file.
    string configuration = File.ReadAllText(@"c:\supportFiles\preset.json");
    
    // Create a task
    ITask task = job.Tasks.AddNew("Media Encoder Standard encoding task",
        processor,
        configuration,
        TaskOptions.None);
    
    // Specify the input videos to be concatenated (in order).
    task.InputAssets.Add(asset1);
    task.InputAssets.Add(asset2);
    // Add an output asset to contain the results of the job. 
    // This output is specified as AssetCreationOptions.None, which 
    // means the output asset is not encrypted. 
    task.OutputAssets.AddNew("Output asset",
        AssetCreationOptions.None);
    
    job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
    job.Submit();
    job.GetExecutionProgressTask(CancellationToken.None).Wait();

###<a name="json-preset"></a>JSON esiasetus

Päivitä mukautetun ennalta määritetty tunnukset kohteita, jotka haluat yhdistää, ja jokaisen videon oikea aika-osiossa.

    {
      "Version": 1.0,
      "Sources": [
        {
          "AssetID": "606db602-efd7-4436-97b4-c0b867ba195b",
          "StartTime": "00:00:01",
          "Duration": "00:00:15"
        },
        {
          "AssetID": "a7e2b90f-0565-4a94-87fe-0a9fa07b9c7e",
          "StartTime": "00:00:02",
          "Duration": "00:00:05"
        }
      ],
      "Codecs": [
        {
          "KeyFrameInterval": "00:00:02",
          "SceneChangeDetection": true,
          "H264Layers": [
            {
              "Level": "auto",
              "Bitrate": 1800,
              "MaxBitrate": 1800,
              "BufferWindow": "00:00:05",
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "Width": "640",
              "Height": "360",
              "FrameRate": "0/1"
            }
          ],
          "Type": "H264Video"
        },
        {
          "Channels": 2,
          "SamplingRate": 48000,
          "Bitrate": 128,
          "Type": "AACAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_{Width}x{Height}_{VideoBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }

##<a id="crop"></a>Rajaa videoiden katselusta Media Encoder vakio

[Rajaa videoiden katselusta Media Encoder vakio](media-services-crop-video.md) aiheessa.

##<a id="no_video"></a>Lisää video seuraa, kun syöte on videota ei

Jos lähetät encoder, joka sisältää vain ääni- ja ei video syötteen sitten tulosteen resurssi sisältää oletusarvoisesti vain ääni tietoja sisältävien tiedostojen. Jotkin pelaajat, mukaan lukien Azure Media Player-ohjelmassa (katso [tämän](https://feedback.azure.com/forums/169396-azure-media-services/suggestions/8082468-audio-only-scenarios)) ei ehkä käsittelemään tällaisten virtaa. Tämän asetuksen avulla voit pakottaa encoder Mustavalkoinen videon Jäljitä lisääminen skenaarion tulos. 

>[AZURE.NOTE]Lisää video tulostus-Jäljitä encoder pakottaminen suurentaa tulosteen resurssi- ja siten koodauksen tehtävän kertyneet kustannukset. Suorita testit voit varmistaa, että voimassa oleva kasvu on vain vähän vaikutus kuukausittain maksuja.

### <a name="inserting-video-at-only-the-lowest-bitrate"></a>Videon lisääminen pienin bittitaajuus etsiminen

Oletetaan, että käytät useita bittitaajuus koodaus esiasetus kuten ["H264 useita bittitaajuus 720p"](https://msdn.microsoft.com/library/mt269960.aspx) koodata koko syötteen luettelon varten tietovirta, joka sisältää videotiedostot sekä pelkän tiedostot. Syöte on ei videota voit halutessasi tässä skenaariossa Pakota encoder, kun haluat lisätä vain pienin bittitaajuus verrattuna lisääminen video, joka tulosteen bittitaajuus Mustavalkoinen videon seuraa. Tätä haluat määrittää "InsertBlackIfNoVideoBottomLayerOnly"-merkinnän.

Voit tehdä mitään Markkinatalousasemaa Esiasetukset kuvata [tähän](https://msdn.microsoft.com/library/mt269960.aspx)ja tee seuraavat muutokset:

#### <a name="json-preset"></a>JSON esiasetus

    {
          "KeyFrameInterval": "00:00:02",
          "StretchMode": "AutoSize",
          "Condition": "InsertBlackIfNoVideoBottomLayerOnly",
          "H264Layers": [
          …
          ]
    }

#### <a name="xml-preset"></a>XML-esiasetus

    <KeyFrameInterval>00:00:02</KeyFrameInterval>
    <StretchMode>AutoSize</StretchMode>
    <Condition>InsertBlackIfNoVideoBottomLayerOnly</Condition>

### <a name="inserting-video-at-all-output-bitrates"></a>Videon lisääminen lainkaan tulosteen bitrates

Oletetaan, että käytät useita bittitaajuus koodaus esiasetus kuten ["H264 useita bittitaajuus 720p](https://msdn.microsoft.com/library/mt269960.aspx) koodata koko syötteen luettelon varten tietovirta, joka sisältää videotiedostot sekä pelkän tiedostot. Syöte on ei video voit halutessasi tässä skenaariossa voit pakottaa Lisää Mustavalkoinen videon Jäljitä lainkaan tulosteen bitrates encoder. Näin varmistat, että tulostus varat ovat kaikki yhdenmukaisemmat, jotka koskevat videon raidat ja ääniraitojen. Tätä haluat määrittää "InsertBlackIfNoVideo"-merkinnän.

Voit tehdä mitään Markkinatalousasemaa Esiasetukset kuvata [tähän](https://msdn.microsoft.com/library/mt269960.aspx)ja tee seuraavat muutokset:

#### <a name="json-preset"></a>JSON esiasetus

    {
          "KeyFrameInterval": "00:00:02",
          "StretchMode": "AutoSize",
          "Condition": "InsertBlackIfNoVideo",
          "H264Layers": [
          …
          ]
    }

#### <a name="xml-preset"></a>XML-esiasetus
    
    <KeyFrameInterval>00:00:02</KeyFrameInterval>
    <StretchMode>AutoSize</StretchMode>
    <Condition>InsertBlackIfNoVideo</Condition>

##<a id="rotate_video"></a>Videon kiertäminen

[Media Encoder Standard](media-services-dotnet-encode-with-media-encoder-standard.md) tukee kiertoa 0/90/180/270 kulmista mukaan. Oletusarvoisen toimintatavan on "Automaattinen", jossa se yrittää tunnistaa kierto-metatiedot saapuvien videotiedostoa ja sen. Sisällytä seuraavat **tietolähteiden** elementti, hiirellä yhtä suorakulmion Esiasetukset määritetyn [tähän](http://msdn.microsoft.com/library/azure/mt269960.aspx):

### <a name="json-preset"></a>JSON esiasetus

    "Sources": [
    {
      "Streams": [],
      "Filters": {
        "Rotation": "90"
      }
    }
    ],
    "Codecs": [
    
    ...
### <a name="xml-preset"></a>XML-esiasetus

    <Sources>
        <Source>
          <Streams />
          <Filters>
            <Rotation>90</Rotation>
          </Filters>
        </Source>
    </Sources>

[Tässä](https://msdn.microsoft.com/library/azure/mt269962.aspx#PreserveResolutionAfterRotation) ohjeaiheessa on Katso myös-encoder tulkitseminen esiasetus, leveys ja korkeus asetuksia kun kierto korvauksen käynnistyy.

Voit ohittaa kierto metatietojen tarvittaessa video Input encoder osoittamaan arvon "0". 


##<a name="media-services-learning-paths"></a>Media-palveluiden oppimispolut

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Palautteen antaminen

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="see-also"></a>Katso myös 

[Media Services-palveluiden koodauksen yleiskatsaus](media-services-encode-asset.md)
