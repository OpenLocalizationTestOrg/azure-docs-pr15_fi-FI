<properties 
    pageTitle="Tietoja live streaming luominen usean bittitaajuus virtaa .NET Azure Media-palveluiden avulla | Microsoft Azure" 
    description="Tässä opetusohjelmassa käydään läpi vaiheet, joka vastaanottaa yhden bittitaajuus live virta ja käyttää sitä usean bittitaajuus stream käyttämällä .NET SDK kanavan luominen." 
    services="media-services" 
    documentationCenter="" 
    authors="anilmur" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article"
    ms.date="10/12/2016"
    ms.author="juliako;anilmur"/>


#<a name="how-to-perform-live-streaming-using-azure-media-services-to-create-multi-bitrate-streams-with-net"></a>Tietoja live streaming luominen usean bittitaajuus virtaa .NET Azure Media-palveluiden avulla

> [AZURE.SELECTOR]
- [Portal](media-services-portal-creating-live-encoder-enabled-channel.md)
- [.NET](media-services-dotnet-creating-live-encoder-enabled-channel.md)
- [REST-OHJELMOINTIRAJAPINNALLA](https://msdn.microsoft.com/library/azure/dn783458.aspx)

>[AZURE.NOTE]
> Jotta voit suorittaa tässä opetusohjelmassa, sinun on Azure-tili. Lisätietoja on artikkelissa [Azure maksuttoman kokeiluversion](/pricing/free-trial/?WT.mc_id=A261C142F).

##<a name="overview"></a>Yleiskatsaus

Tässä opetusohjelmassa käydään läpi vaiheet, joka vastaanottaa yhden bittitaajuus live virta ja käyttää sitä usean bittitaajuus stream **kanavan** luominen.

Käsitteellinen liittyvät, jotka on otettu käyttöön live koodaus kanavien, katso lisätietoja [Live streaming luominen usean bittitaajuus virtaa Azure Media-palveluiden avulla](media-services-manage-live-encoder-enabled-channels.md).


##<a name="common-live-streaming-scenario"></a>Käytetty Live Streaming-vaihtoehto

Seuraavissa vaiheissa kuvataan tehtävien yhdistävää yhteistä live streaming sovellusten luominen.

>[AZURE.NOTE] Tällä hetkellä live tapahtuman max suositeltava kesto on kahdeksan tuntia. Ota yhteyttä amslived Microsoft.comin, jos haluat suorittaa kanavan pitkällä aikavälillä.

1. Kameraa muodostaa yhteyttä tietokoneeseen. Käynnistä ja määrittää paikallisen live encoder, joka voi tulostaa yhden bittitaajuus stream jollakin seuraavista protokollista: RTMP, tasainen Streaming tai RTP (MPEG-TS). Lisätietoja on artikkelissa [Azure Media Services RTMP tuki- ja Live koodauslaitteista](http://go.microsoft.com/fwlink/?LinkId=532824).

Tämän vaiheen myös voi suorittaa, kun olet luonut kanavan.

1. Luo ja Käynnistä kanavan.

1. Nouda kanavan ingest URL-osoite.

Ingest URL-Osoitetta käytetään live encoder virta lähettäminen kanava.

1. Noutaa kanavan esikatselu URL-osoite.

Tätä URL-Osoitteen avulla voit tarkistaa kanavan oikein lähetetään live stream.

2. Luo sijoituksen.
3. Jos haluat, voit salata dynaamisesti toiston aikana annetaan, toimi seuraavasti:
1. Luo sisällön avain.
1. Sisällön avain luvan käytännön määrittäminen.
1. Määritä resurssi toimituksen käytäntö (käyttämä Dynaaminen pakkaus ja dynaaminen salauksen).
3. Ohjelma luo ja määritä käyttämään resurssi, jonka loit.
1. Julkaise ohjelmaan liittyvä luomalla OnDemand locator resurssi.

Varmista, että ainakin yksi streaming varattu yksikön streaming päätepisteen, josta haluat sisällön.

1. Käynnistä ohjelma, kun olet valmis aloittamaan streaming ja arkistointi.
2. Voit myös live encoder voi tehdä Aloita mainos. Ilmoitus lisätään tulostus-muodossa.
1. Lopeta ohjelma aina, kun haluat lopettaa streaming ja arkistointi tapahtuma.
1. Poista ohjelma (ja halutessasi poistaa kohteen).

## <a name="what-youll-learn"></a>Opit

Tämän artikkelin avulla voit suorittaa kanavat ja ohjelmat Media Services .NET SDK eri toimintoja. Koska monet toiminnot ovat pitkään suoritettavien .NET-ohjelmointirajapinnan, joilla hallitaan pitkään suoritetut toimintojen avulla.

Aiheen näyttää, miten seuraavasti:

1. Luo ja Käynnistä kanavan. Pitkään suoritettavien ohjelmointirajapinnan käytetään.
1. Hae kanavat ingest (input) päätepiste. Tämä päätepiste on annettava encoder, voit lähettää yksittäisen bittitaajuus live stream.
1. Pyydä esikatselu päätepiste. Tämä päätepiste käytetään oman stream esikatselu.
1. Luo resurssi, jota käytetään sisällön tallennuspaikka. Kohteiden toimituksen käytännöt on määritettävä myös, kuten tässä esimerkissä.
1. Ohjelma luo ja määritä käyttämään resurssi, joka on luotu aiemmassa versiossa. Käynnistä ohjelma. Pitkään suoritettavien ohjelmointirajapinnan käytetään.
1. Luo locator, annetaan, jotta sisältö saa julkaista ja voidaan lähettää asiakkaat.
1. Näyttää tai piilottaa Kivitaulut. Aloittaa ja lopettaa ilmoituksia. Pitkään suoritettavien ohjelmointirajapinnan käytetään.
1. Puhdista kanavan ja kaikki niihin liittyvät resurssit.


##<a name="prerequisites"></a>Edellytykset

Seuraavassa on suorittamiseen tarvittava opetusohjelman.

- Jotta voit suorittaa tässä opetusohjelmassa, sinun on Azure-tili.

Jos sinulla ei ole tiliä, voit luoda ilmainen kokeiluversio tili vain muutaman minuutin. Lisätietoja on artikkelissa [Azure maksuttoman kokeiluversion](/pricing/free-trial/?WT.mc_id=A261C142F). Saat hyvitykset, jonka avulla voit kokeilla maksettu Azure services. Senkin jälkeen, kun hyvitykset on käytetty, voit säilyttää sen tilin ja käyttää Azure palveluiden ja -ominaisuuksia, kuten Azure App palvelun Web Apps-toiminnon.
- Media Services-tilin. Media Services-tilin luominen-kohdassa [Luo tili](media-services-portal-create-account.md).
- Visual Studio 2010 SP1 (Professional, Premium, Ultimate tai Express) tai uudempi versio.
- Sinun on käytettävä Media Services .NET SDK versio 3.2.0.0 tai uudempi versio.
- Verkkokamera ja encoder, voit lähettää yksittäisen bittitaajuus live muodossa.

##<a name="considerations"></a>Huomioon otettavia seikkoja

- Tällä hetkellä live tapahtuman max suositeltava kesto on kahdeksan tuntia. Ota yhteyttä amslived Microsoft.comin, jos haluat suorittaa kanavan pitkällä aikavälillä.
- Varmista, että ainakin yksi streaming varattu yksikön streaming päätepisteen, josta haluat sisällön.

##<a name="download-sample"></a>Lataa malli

Hae ja suorita otoksen [täältä](https://azure.microsoft.com/documentation/samples/media-services-dotnet-encode-live-stream-with-ams-clear/).


##<a name="set-up-for-development-with-media-services-sdk-for-net"></a>Media Services SDK kehitys, .NET määrittäminen

1. Luo Visual Studiossa console-sovellus.
1. Lisää Media Services SDK .NET käyttämällä Media Services NuGet paketin console-sovellukseen.

##<a name="connect-to-media-services"></a>Media-palveluiden yhdistäminen
Paras käytäntö kannattaa tallentaa Media Services nimi ja tilin avain app.config-tiedoston avulla.

>[AZURE.NOTE]Jos haluat etsiä nimen ja avaimen arvoja, siirry Azure-portaaliin ja valitse tilisi. Asetukset-ikkuna oikealla. Valitse asetukset-ikkunassa avaimet. Jokaisen tekstiruudun vieressä olevaa kuvaketta valitsemalla kopioi arvon järjestelmän Leikepöydälle.

AppSettings-osan lisääminen app.config-tiedostoa ja määritä Media Services tilin nimi ja tilin avainta.


    <?xml version="1.0"?>
    <configuration>
      <appSettings>
          <add key="MediaServicesAccountName" value="YouMediaServicesAccountName" />
          <add key="MediaServicesAccountKey" value="YouMediaServicesAccountKey" />
      </appSettings>
    </configuration>
     
    
##<a name="code-example"></a>Esimerkki

    using System;
    using System.Collections.Generic;
    using System.Configuration;
    using System.IO;
    using System.Linq;
    using System.Net;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using Microsoft.WindowsAzure.MediaServices.Client.DynamicEncryption;
    
    namespace EncodeLiveStreamWithAmsClear
    {
        class Program
        {
            private const string ChannelName = "channel001";
            private const string AssetlName = "asset001";
            private const string ProgramlName = "program001";
    
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
    
                IChannel channel = CreateAndStartChannel();
    
                // The channel's input endpoint:
                string ingestUrl = channel.Input.Endpoints.FirstOrDefault().Url.ToString();
    
                Console.WriteLine("Intest URL: {0}", ingestUrl);
    
    
                // Use the previewEndpoint to preview and verify 
                // that the input from the encoder is actually reaching the Channel. 
                string previewEndpoint = channel.Preview.Endpoints.FirstOrDefault().Url.ToString();
    
                Console.WriteLine("Preview URL: {0}", previewEndpoint);
    
                // When Live Encoding is enabled, you can now get a preview of the live feed as it reaches the Channel. 
                // This can be a valuable tool to check whether your live feed is actually reaching the Channel. 
                // The thumbnail is exposed via the same end-point as the Channel Preview URL.
                string thumbnailUri = new UriBuilder
                {
                    Scheme = Uri.UriSchemeHttps,
                    Host = channel.Preview.Endpoints.FirstOrDefault().Url.Host,
                    Path = "thumbnails/input.jpg"
                }.Uri.ToString();
    
                Console.WriteLine("Thumbain URL: {0}", thumbnailUri);
    
                // Once you previewed your stream and verified that it is flowing into your Channel, 
                // you can create an event by creating an Asset, Program, and Streaming Locator. 
                IAsset asset = CreateAndConfigureAsset();
    
                IProgram program = CreateAndStartProgram(channel, asset);
    
                ILocator locator = CreateLocatorForAsset(program.Asset, program.ArchiveWindowLength);
    
                // You can use slates and ads only if the channel type is Standard.  
                StartStopAdsSlates(channel);
    
                // Once you are done streaming, clean up your resources.
                Cleanup(channel);
    
            }
    
            public static IChannel CreateAndStartChannel()
            {
                var channelInput = CreateChannelInput();
                var channePreview = CreateChannelPreview();
                var channelEncoding = CreateChannelEncoding();
    
    
                ChannelCreationOptions options = new ChannelCreationOptions
                {
                    EncodingType = ChannelEncodingType.Standard,
                    Name = ChannelName,
                    Input = channelInput,
                    Preview = channePreview,
                    Encoding = channelEncoding
                };
    
                Log("Creating channel");
                IOperation channelCreateOperation = _context.Channels.SendCreateOperation(options);
                string channelId = TrackOperation(channelCreateOperation, "Channel create");
    
                IChannel channel = _context.Channels.Where(c => c.Id == channelId).FirstOrDefault();
    
                Log("Starting channel");
                var channelStartOperation = channel.SendStartOperation();
                TrackOperation(channelStartOperation, "Channel start");
    
                return channel;
            }
    
            /// <summary>
            /// Create channel input, used in channel creation options. 
            /// </summary>
            /// <returns></returns>
            private static ChannelInput CreateChannelInput()
            {
                return new ChannelInput
                {
                    StreamingProtocol = StreamingProtocol.RTPMPEG2TS,
                    AccessControl = new ChannelAccessControl
                    {
                        IPAllowList = new List<IPRange>
                        {
                            new IPRange
                            {
                                Name = "TestChannelInput001",
                                Address = IPAddress.Parse("0.0.0.0"),
                                SubnetPrefixLength = 0
                            }
                        }
                    }
                };
            }
    
            /// <summary>
            /// Create channel preview, used in channel creation options. 
            /// </summary>
            /// <returns></returns>
            private static ChannelPreview CreateChannelPreview()
            {
                return new ChannelPreview
                {
                    AccessControl = new ChannelAccessControl
                    {
                        IPAllowList = new List<IPRange>
                        {
                            new IPRange
                            {
                                Name = "TestChannelPreview001",
                                Address = IPAddress.Parse("0.0.0.0"),
                                SubnetPrefixLength = 0
                            }
                        }
                    }
                };
            }
    
            /// <summary>
            /// Create channel encoding, used in channel creation options. 
            /// </summary>
            /// <returns></returns>
            private static ChannelEncoding CreateChannelEncoding()
            {
                return new ChannelEncoding
                {
                    SystemPreset = "Default720p",
                    IgnoreCea708ClosedCaptions = false,
                    AdMarkerSource = AdMarkerSource.Api,
                    // You can only set audio if streaming protocol is set to StreamingProtocol.RTPMPEG2TS.
                    AudioStreams = new List<AudioStream> { new AudioStream { Index = 103, Language = "eng" } }.AsReadOnly()
                };
            }
    
            /// <summary>
            /// Create an asset and configure asset delivery policies.
            /// </summary>
            /// <returns></returns>
            public static IAsset CreateAndConfigureAsset()
            {
                IAsset asset = _context.Assets.Create(AssetlName, AssetCreationOptions.None);
    
                IAssetDeliveryPolicy policy =
                    _context.AssetDeliveryPolicies.Create("Clear Policy",
                    AssetDeliveryPolicyType.NoDynamicEncryption,
                    AssetDeliveryProtocol.HLS | AssetDeliveryProtocol.SmoothStreaming | AssetDeliveryProtocol.Dash, null);
    
                asset.DeliveryPolicies.Add(policy);
    
                return asset;
            }
    
            /// <summary>
            /// Create a Program on the Channel. You can have multiple Programs that overlap or are sequential;
            /// however each Program must have a unique name within your Media Services account.
            /// </summary>
            /// <param name="channel"></param>
            /// <param name="asset"></param>
            /// <returns></returns>
            public static IProgram CreateAndStartProgram(IChannel channel, IAsset asset)
            {
                IProgram program = channel.Programs.Create(ProgramlName, TimeSpan.FromHours(3), asset.Id);
                Log("Program created", program.Id);
    
                Log("Starting program");
                var programStartOperation = program.SendStartOperation();
                TrackOperation(programStartOperation, "Program start");
    
                return program;
            }
    
            /// <summary>
            /// Create locators in order to be able to publish and stream the video.
            /// </summary>
            /// <param name="asset"></param>
            /// <param name="ArchiveWindowLength"></param>
            /// <returns></returns>
            public static ILocator CreateLocatorForAsset(IAsset asset, TimeSpan ArchiveWindowLength)
            {
                // You cannot create a streaming locator using an AccessPolicy that includes write or delete permissions.            
                var locator = _context.Locators.CreateLocator
                    (
                        LocatorType.OnDemandOrigin,
                        asset,
                        _context.AccessPolicies.Create
                            (
                                "Live Stream Policy",
                                ArchiveWindowLength,
                                AccessPermissions.Read
                            )
                    );
    
                return locator;
            }
    
            /// <summary>
            /// Perform operations on slates.
            /// </summary>
            /// <param name="channel"></param>
            public static void StartStopAdsSlates(IChannel channel)
            {
                int cueId = new Random().Next(int.MaxValue);
                var path = Path.GetFullPath(Path.Combine(AppDomain.CurrentDomain.BaseDirectory, @"..\\..\\SlateJPG\\DefaultAzurePortalSlate.jpg"));
    
                Log("Creating asset");
                var slateAsset = _context.Assets.Create("Slate test asset " + DateTime.Now.ToString("yyyy-MM-dd HH-mm"), AssetCreationOptions.None);
                Log("Slate asset created", slateAsset.Id);
    
                Log("Uploading file");
                var assetFile = slateAsset.AssetFiles.Create("DefaultAzurePortalSlate.jpg");
                assetFile.Upload(path);
                assetFile.IsPrimary = true;
                assetFile.Update();
    
                Log("Showing slate");
                var showSlateOpeartion = channel.SendShowSlateOperation(TimeSpan.FromMinutes(1), slateAsset.Id);
                TrackOperation(showSlateOpeartion, "Show slate");
    
                Log("Hiding slate");
                var hideSlateOperation = channel.SendHideSlateOperation();
                TrackOperation(hideSlateOperation, "Hide slate");
    
                Log("Starting ad");
                var startAdOperation = channel.SendStartAdvertisementOperation(TimeSpan.FromMinutes(1), cueId, false);
                TrackOperation(startAdOperation, "Start ad");
    
                Log("Ending ad");
                var endAdOperation = channel.SendEndAdvertisementOperation(cueId);
                TrackOperation(endAdOperation, "End ad");
    
                Log("Deleting slate asset");
                slateAsset.Delete();
            }
    
            /// <summary>
            /// Clean up resources associated with the channel.
            /// </summary>
            /// <param name="channel"></param>
            public static void Cleanup(IChannel channel)
            {
                IAsset asset;
                if (channel != null)
                {
                    foreach (var program in channel.Programs)
                    {
                        asset = _context.Assets.Where(se => se.Id == program.AssetId)
                                                .FirstOrDefault();
    
                        Log("Stopping program");
                        var programStopOperation = program.SendStopOperation();
                        TrackOperation(programStopOperation, "Program stop");
    
                        program.Delete();
    
                        if (asset != null)
                        {
                            Log("Deleting locators");
                            foreach (var l in asset.Locators)
                                l.Delete();
    
                            Log("Deleting asset");
                            asset.Delete();
                        }
                    }
    
                    Log("Stopping channel");
                    var channelStopOperation = channel.SendStopOperation();
                    TrackOperation(channelStopOperation, "Channel stop");
    
                    Log("Deleting channel");
                    var channelDeleteOperation = channel.SendDeleteOperation();
                    TrackOperation(channelDeleteOperation, "Channel delete");
                }
            }
    
    
            /// <summary>
            /// Track long running operations.
            /// </summary>
            /// <param name="operation"></param>
            /// <param name="description"></param>
            /// <returns></returns>
            public static string TrackOperation(IOperation operation, string description)
            {
                string entityId = null;
                bool isCompleted = false;
    
                Log("starting to track ", null, operation.Id);
                while (isCompleted == false)
                {
                    operation = _context.Operations.GetOperation(operation.Id);
                    isCompleted = IsCompleted(operation, out entityId);
                    System.Threading.Thread.Sleep(TimeSpan.FromSeconds(30));
                }
                // If we got here, the operation succeeded.
                Log(description + " in completed", operation.TargetEntityId, operation.Id);
    
                return entityId;
            }
    
            /// <summary> 
            /// Checks if the operation has been completed. 
            /// If the operation succeeded, the created entity Id is returned in the out parameter.
            /// </summary> 
            /// <param name="operationId">The operation Id.</param> 
            /// <param name="channel">
            /// If the operation succeeded, 
            /// the entity Id associated with the sucessful operation is returned in the out parameter.</param>
            /// <returns>Returns false if the operation is still in progress; otherwise, true.</returns> 
            private static bool IsCompleted(IOperation operation, out string entityId)
            {
    
                bool completed = false;
    
                entityId = null;
    
                switch (operation.State)
                {
                    case OperationState.Failed:
                        // Handle the failure. 
                        // For example, throw an exception. 
                        // Use the following information in the exception: operationId, operation.ErrorMessage.
                        Log("operation failed", operation.TargetEntityId, operation.Id);
                        break;
                    case OperationState.Succeeded:
                        completed = true;
                        entityId = operation.TargetEntityId;
                        break;
                    case OperationState.InProgress:
                        completed = false;
                        Log("operation in progress", operation.TargetEntityId, operation.Id);
                        break;
                }
                return completed;
            }
    
    
            private static void Log(string action, string entityId = null, string operationId = null)
            {
                Console.WriteLine(
                    "{0,-21}{1,-51}{2,-51}{3,-51}",
                    DateTime.Now.ToString("yyyy'-'MM'-'dd HH':'mm':'ss"),
                    action,
                    entityId ?? string.Empty,
                    operationId ?? string.Empty);
            }
        }
    }   


##<a name="next-step"></a>Seuraava vaihe

Tarkista oppimispolut Media-palveluita.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Palautteen antaminen

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

### <a name="looking-for-something-else"></a>Etsitkö jotain muuta?

Jos ohjeaihe ei sisällä mitä olet odottanut, puuttuu jotain tai -jollakin muulla tavalla ei vastaa tarpeitasi, anna meille kanssasi palautteen Disqus säikeen alla.
