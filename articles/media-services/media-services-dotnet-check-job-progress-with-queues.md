<properties 
    pageTitle="Azure-jonossa tallennustilan avulla voit valvoa Media Services-työ ilmoitukset, joiden .NET | Microsoft Azure" 
    description="Opettele käyttämään Azure jonon tallennustilan seurannassa Media Services työn ilmoitukset. Koodi-malli on kirjoitettu C# ja käyttää Media Services SDK .NET." 
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
    ms.date="08/19/2016"   
    ms.author="juliako"/>

# <a name="use-azure-queue-storage-to-monitor-media-services-job-notifications-with-net"></a>Azure-jonossa tallennustilan avulla voit valvoa Media Services-työ ilmoitukset, joiden .NET

Kun suoritat työt, tarvitset usein vielä työn edistymistä. Voit tarkistaa edistymisen Azure jonon tallennustilan avulla voit valvoa Media Services työn ilmoitukset (Tässä ohjeaiheessa kuvatulla) tai määrittäminen StateChanged tapahtumakäsittelijä ( [Tässä](media-services-check-job-progress.md) ohjeaiheessa kuvatulla.  

## <a name="use-azure-queue-storage-to-monitor-media-services-job-notifications"></a>Azure-jonossa tallennustilan avulla voit valvoa Media Services työn ilmoitukset

Microsoft Azure Media Services pystyy pitää ilmoitukset [Azure jono tallennustilan](../storage-dotnet-how-to-use-queues.md#what-is) käsiteltäessä media työt. Tässä ohjeaiheessa esitellään nämä ilmoitukset hakemisesta jonon tallennustilan.

Viestit toimitetaan jonon tallennustilan voi käyttää mitä tahansa maailmanlaajuisesti. Azure-jonossa messaging arkkitehtuuri on luotettava ja erittäin skaalattava. Kyselyt jonon tallennustilan suositellaan päälle jollakin toisella tavalla. 

Yksi käytetty vaihtoehto kuunnella Media-palveluiden ilmoitukset on, jos kehittävät sisällön hallintajärjestelmän, jotka on suoritettava tehtävä joitakin muita koodauksen työn jälkeen on valmis (esimerkiksi käynnistimen seuraavan vaiheen työnkulun tai julkaista sisältöä). 

###<a name="considerations"></a>Huomioon otettavia seikkoja

Harkitse seuraavien asetusten Media Services-sovellusten, jotka käyttävät Azure tallennustilan jonon kehittämisessä.

- Olevien palvelu ei tarjoa takaa ensimmäisen-in-first-out (FIFO) vyöhykkeen toimitusta. Lisätietoja on artikkelissa [Azure olevien ja Azure palvelun Bus olevien verrattuna ja Contrasted](https://msdn.microsoft.com/library/azure/hh767287.aspx).
- Azure tallennustilan olevien ei ole push-palvelun; Sinulla lisääminen jonossa. 
- Voit määrittää minkä tahansa olevien määrän. Lisätietoja on artikkelissa [Jono palvelun REST API](https://msdn.microsoft.com/library/azure/dd179363.aspx).
- Azure tallennustilan olevien on joitakin rajoituksia ja tiedot, jotka on kuvattu seuraavassa artikkelissa: [Azure olevien ja Azure palvelun Bus olevien verrattuna ja Contrasted](https://msdn.microsoft.com/library/azure/hh767287.aspx).

###<a name="code-example"></a>Esimerkki

Tässä osassa koodiesimerkki toimii seuraavasti:

1. Määrittää, joka yhdistää ilmoituksen viestimuodon **EncodingJobMessage** -luokka. Koodin deserializes liittyvät ongelmat jonossa objekteiksi **EncodingJobMessage** tyyppi.
1. Lataa Media Services ja tallennustilaa tilitietojen app.config-tiedostoa. Voit käyttää tätä tietoa **CloudMediaContext** ja **CloudQueue** -objektien luomiseen.
1. Luo jonon, joka vastaanottaa ilmoituksen koodauksen projektin tietoja.
1. Luo ilmoitus loppupisteen merkki, joka on yhdistetty jonossa.
1. Liittää ilmoituksen lopuksi työn ja lähettää koodauksen työn. Voit määrittää useita liitetty työn ilmoituksen päätepisteestä.
1. Tässä esimerkissä emme vain kiinnostunut lopullinen hyötyä työn käsittelyn, niin että välittää **NotificationJobState.FinalStatesOnly** **AddNew** -menetelmä. 
        
        job.JobNotificationSubscriptions.AddNew(NotificationJobState.FinalStatesOnly, _notificationEndPoint);
1. Jos NotificationJobState.All onnistuu, odotat olisi kaikki tilan muuttaminen ilmoitusten saaminen: jonossa -> ajoitettu -> käsittely -> valmis. Kuitenkin aiemmin mainittuja Azure-tallennustilan olevien palvelun takaa järjestetyn toimitusta. Voit käyttää aikaleima ominaisuuden (määritettynä EncodingJobMessage alla olevassa esimerkissä) tilauksen viesteihin. On mahdollista saat kaksoiskappaleiden ilmoitukset. (Määritetty EncodingJobMessage-tyypin) ETag-ominaisuuden avulla voit etsiä kaksoiskappaleita. On myös mahdollista, että jotkin tilan muuttaminen ilmoitukset ohitetaan. 
1. Odottaa työn saat tilaan valitsemalla jonossa 10 sekunnin välein. Poistaa viestejä, kun ne on käsitelty.
1. Poistaa jonossa ja ilmoituksen loppupisteen merkki.

>[AZURE.NOTE]Suositeltu tapaa valvoa projektin tila on kuunteleminen ilmoitukset, kuten seuraavassa esimerkissä.
>
>Vaihtoehtoisesti voit voi tarkistaa projektin tila **IJob.State** -ominaisuuden avulla.  Tietoja projektin valmistumista ilmoitus voi tulla ennen sen valtion **IJob** asetuksena on **Valmis**. **IJob.State** -ominaisuuden kuvastaa vähäisiä viiveellä tarkkoja tila.

    
    using System;
    using System.Linq;
    using System.Configuration;
    using System.IO;
    using System.Text;
    using System.Threading;
    using System.Threading.Tasks;
    using System.Collections.Generic;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using System.Web;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Auth;
    using Microsoft.WindowsAzure.Storage.Queue;
    using System.Runtime.Serialization.Json;
    
    namespace JobNotification
    {
        public class EncodingJobMessage
        {
            // MessageVersion is used for version control. 
            public String MessageVersion { get; set; }
        
            // Type of the event. Valid values are 
            // JobStateChange and NotificationEndpointRegistration.
            public String EventType { get; set; }
        
            // ETag is used to help the customer detect if 
            // the message is a duplicate of another message previously sent.
            public String ETag { get; set; }
        
            // Time of occurrence of the event.
            public String TimeStamp { get; set; }
    
            // Collection of values specific to the event.
    
            // For the JobStateChange event the values are:
            //     JobId - Id of the Job that triggered the notification.
            //     NewState- The new state of the Job. Valid values are:
            //          Scheduled, Processing, Canceling, Cancelled, Error, Finished
            //     OldState- The old state of the Job. Valid values are:
            //          Scheduled, Processing, Canceling, Cancelled, Error, Finished
    
            // For the NotificationEndpointRegistration event the values are:
            //     NotificationEndpointId- Id of the NotificationEndpoint 
            //          that triggered the notification.
            //     State- The state of the Endpoint. 
            //          Valid values are: Registered and Unregistered.
    
            public IDictionary<string, object> Properties { get; set; }
        }
    
        class Program
        {
            private static CloudMediaContext _context = null;
            private static CloudQueue _queue = null;
            private static INotificationEndPoint _notificationEndPoint = null;
    
            private static readonly string _singleInputMp4Path =
                Path.GetFullPath(@"C:\supportFiles\multifile\BigBuckBunny.mp4");
    
            static void Main(string[] args)
            {
                // Get the values from app.config file.
                string mediaServicesAccountName = ConfigurationManager.AppSettings["MediaServicesAccountName"];
                string mediaServicesAccountKey = ConfigurationManager.AppSettings["MediaServicesAccountKey"];
                string storageConnectionString = ConfigurationManager.AppSettings["StorageConnectionString"];
    
    
                string endPointAddress = Guid.NewGuid().ToString();
    
                // Create the context. 
                _context = new CloudMediaContext(mediaServicesAccountName, mediaServicesAccountKey);
    
                // Create the queue that will be receiving the notification messages.
                _queue = CreateQueue(storageConnectionString, endPointAddress);
    
                // Create the notification point that is mapped to the queue.
                _notificationEndPoint = 
                        _context.NotificationEndPoints.Create(
                        Guid.NewGuid().ToString(), NotificationEndPointType.AzureQueue, endPointAddress);
    
    
                if (_notificationEndPoint != null)
                {
                    IJob job = SubmitEncodingJobWithNotificationEndPoint(_singleInputMp4Path);
                    WaitForJobToReachedFinishedState(job.Id);
                }
    
                // Clean up.
                _queue.Delete();      
                _notificationEndPoint.Delete();
           }
    
    
            static public CloudQueue CreateQueue(string storageAccountConnectionString, string endPointAddress)
            {
                CloudStorageAccount storageAccount = CloudStorageAccount.Parse(storageAccountConnectionString);
    
                // Create the queue client
                CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    
                // Retrieve a reference to a queue
                CloudQueue queue = queueClient.GetQueueReference(endPointAddress);
    
                // Create the queue if it doesn't already exist
                queue.CreateIfNotExists();
    
                return queue;
            }
     
    
            public static IJob SubmitEncodingJobWithNotificationEndPoint(string inputMediaFilePath)
            {
                // Declare a new job.
                IJob job = _context.Jobs.Create("My MP4 to Smooth Streaming encoding job");
    
                //Create an encrypted asset and upload the mp4. 
                IAsset asset = CreateAssetAndUploadSingleFile(AssetCreationOptions.StorageEncrypted, 
                    inputMediaFilePath);
    
                // Get a media processor reference, and pass to it the name of the 
                // processor to use for the specific task.
                IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");
    
                // Create a task with the conversion details, using a configuration file. 
                ITask task = job.Tasks.AddNew("My encoding Task",
                    processor,
                    "H264 Multiple Bitrate 720p",
                    Microsoft.WindowsAzure.MediaServices.Client.TaskOptions.None);
    
                // Specify the input asset to be encoded.
                task.InputAssets.Add(asset);
    
                // Add an output asset to contain the results of the job.
                task.OutputAssets.AddNew("Output asset",
                    AssetCreationOptions.None);
    
                // Add a notification point to the job. You can add multiple notification points.  
                job.JobNotificationSubscriptions.AddNew(NotificationJobState.FinalStatesOnly, 
                    _notificationEndPoint);
    
                job.Submit();
    
                return job;
            }
    
            public static void WaitForJobToReachedFinishedState(string jobId)
            {
                int expectedState = (int)JobState.Finished;
                int timeOutInSeconds = 600;
    
                bool jobReachedExpectedState = false;
                DateTime startTime = DateTime.Now;
                int jobState = -1;
    
                while (!jobReachedExpectedState)
                {
                    // Specify how often you want to get messages from the queue.
                    Thread.Sleep(TimeSpan.FromSeconds(10));
    
                    foreach (var message in _queue.GetMessages(10))
                    {
                        using (Stream stream = new MemoryStream(message.AsBytes))
                        {
                            DataContractJsonSerializerSettings settings = new DataContractJsonSerializerSettings();
                            settings.UseSimpleDictionaryFormat = true;
                            DataContractJsonSerializer ser = new DataContractJsonSerializer(typeof(EncodingJobMessage), settings);
                            EncodingJobMessage encodingJobMsg = (EncodingJobMessage)ser.ReadObject(stream);
    
                            Console.WriteLine();
    
                            // Display the message information.
                            Console.WriteLine("EventType: {0}", encodingJobMsg.EventType);
                            Console.WriteLine("MessageVersion: {0}", encodingJobMsg.MessageVersion);
                            Console.WriteLine("ETag: {0}", encodingJobMsg.ETag);
                            Console.WriteLine("TimeStamp: {0}", encodingJobMsg.TimeStamp);
                            foreach (var property in encodingJobMsg.Properties)
                            {
                                Console.WriteLine("    {0}: {1}", property.Key, property.Value);
                            }
    
                            // We are only interested in messages 
                            // where EventType is "JobStateChange".
                            if (encodingJobMsg.EventType == "JobStateChange")
                            {
                                string JobId = (String)encodingJobMsg.Properties.Where(j => j.Key == "JobId").FirstOrDefault().Value;
                                if (JobId == jobId)
                                {
                                    string oldJobStateStr = (String)encodingJobMsg.Properties.
                                                                Where(j => j.Key == "OldState").FirstOrDefault().Value;
                                    string newJobStateStr = (String)encodingJobMsg.Properties.
                                                                Where(j => j.Key == "NewState").FirstOrDefault().Value;
    
                                    JobState oldJobState = (JobState)Enum.Parse(typeof(JobState), oldJobStateStr);
                                    JobState newJobState = (JobState)Enum.Parse(typeof(JobState), newJobStateStr);
    
                                    if (newJobState == (JobState)expectedState)
                                    {
                                        Console.WriteLine("job with Id: {0} reached expected state: {1}", 
                                            jobId, newJobState);
                                        jobReachedExpectedState = true;
                                        break;
                                    }
                                }
                            }
                        }
                        // Delete the message after we've read it.
                        _queue.DeleteMessage(message);
                    }
    
                    // Wait until timeout
                    TimeSpan timeDiff = DateTime.Now - startTime;
                    bool timedOut = (timeDiff.TotalSeconds > timeOutInSeconds);
                    if (timedOut)
                    {
                        Console.WriteLine(@"Timeout for checking job notification messages, 
                                            latest found state ='{0}', wait time = {1} secs",
                            jobState,
                            timeDiff.TotalSeconds);
    
                        throw new TimeoutException();
                    }
                }
            }
       
            static private IAsset CreateAssetAndUploadSingleFile(AssetCreationOptions assetCreationOptions, string singleFilePath)
            {
                var asset = _context.Assets.Create("UploadSingleFile_" + DateTime.UtcNow.ToString(), 
                    assetCreationOptions);
    
                var fileName = Path.GetFileName(singleFilePath);
    
                var assetFile = asset.AssetFiles.Create(fileName);
    
                Console.WriteLine("Created assetFile {0}", assetFile.Name);
                Console.WriteLine("Upload {0}", assetFile.Name);
    
                assetFile.Upload(singleFilePath);
                Console.WriteLine("Done uploading of {0}", assetFile.Name);
    
                return asset;
            }
    
            static private IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
            {
                var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
                    ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();
    
                if (processor == null)
                    throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));
    
                return processor;
            }
        }
    }

Yllä olevassa esimerkissä valmistettu seuraava tulos. Voit arvot vaihtelevat.
    
    Created assetFile BigBuckBunny.mp4
    Upload BigBuckBunny.mp4
    Done uploading of BigBuckBunny.mp4
    
    EventType: NotificationEndPointRegistration
    MessageVersion: 1.0
    ETag: e0238957a9b25bdf3351a88e57978d6a81a84527fad03bc23861dbe28ab293f6
    TimeStamp: 2013-05-14T20:22:37
        NotificationEndPointId: nb:nepid:UUID:d6af9412-2488-45b2-ba1f-6e0ade6dbc27
        State: Registered
        Name: dde957b2-006e-41f2-9869-a978870ac620
        Created: 2013-05-14T20:22:35
    
    EventType: JobStateChange
    MessageVersion: 1.0
    ETag: 4e381f37c2d844bde06ace650310284d6928b1e50101d82d1b56220cfcb6076c
    TimeStamp: 2013-05-14T20:24:40
        JobId: nb:jid:UUID:526291de-f166-be47-b62a-11ffe6d4be54
        JobName: My MP4 to Smooth Streaming encoding job
        NewState: Finished
        OldState: Processing
        AccountName: westeuropewamsaccount
    job with Id: nb:jid:UUID:526291de-f166-be47-b62a-11ffe6d4be54 reached expected 
    State: Finished
    

## <a name="next-step"></a>Seuraava vaihe

Tarkista Media Services oppimispolut

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Palautteen antaminen

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
