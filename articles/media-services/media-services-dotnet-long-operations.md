<properties 
    pageTitle="Kyselyt pitkään käynnissä olevia toimintoja | Microsoft Azure" 
    description="Tässä ohjeaiheessa esitellään, miten lisääminen pitkään käynnissä olevia toimintoja." 
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
    ms.date="09/26/2016" 
    ms.author="juliako"/>


#<a name="delivering-live-streaming-with-azure-media-services"></a>Välittää Live Streaming Azure Media-palvelujen kanssa

##<a name="overview"></a>Yleiskatsaus

Microsoft Azure Media Services tarjoaa API, joka lähettää pyyntöjä Media Services alkavan toiminnot (esimerkiksi: Luo, aloittaa, lopettaa tai kanavan poistaminen). Nämä toiminnot ovat pitkään suoritettavien.

Media Services .NET SDK sisältää API, Lähetä pyyntö ja odota, kunnes toiminto on valmis (sisäisesti API ovat kyselyt toiminnon edistyminen joitakin väliajoin). Kun soitat kanavan esimerkiksi. Kutsumista, menetelmä palauttaa kanavan aloittamisen jälkeen. Voit myös käyttää asynkroninen versio: odotettava kanava. StartAsync() (tehtävien asynkroninen kuviota, lisätietoja [NAPAUTTAMALLA](https://msdn.microsoft.com/library/hh873175(v=vs.110).aspx)). API, toiminto-Lähetyspyyntö ja sitten kyselyn tilan, kunnes toiminto on valmis kutsutaan "kysely menetelmät". Näistä tavoista (erityisesti asynkroninen-versio) suositellaan rich client-sovellusten ja/tai tilallisten palvelut.

On skenaariot, jossa sovelluksen ei odota pitkään käytössä http-pyynnön ja haluaa äänestyksen järjestäminen toiminnon edistymisen manuaalisesti. Tyypillinen esimerkki olisi tilattomien WWW-palvelun käyttäminen selaimessa: kun selain pyytää Luo kanava-web-palvelu aloittaa pitkään käynnissä toiminto ja palauttaa toiminnon tunnus selaimeen. Selaimessa voi pyytää WWW-palvelun avulla toiminnon tilan perusteella on. Media Services .NET SDK sisältää API, joka sopii Tämä skenaario. Nämä API kutsutaan "kysely menetelmät".
"Kysely menetelmät" on seuraavassa nimeämismuoto: Lähetä*OperationName*toiminnon (esimerkiksi SendCreateOperation). Lähetä*OperationName*toiminnon menetelmiä palauttaa **IOperation** objektia. palautettu objekti sisältää tiedot, joiden avulla voidaan seurata toiminto. Lähetä*OperationName*OperationAsync menetelmiä palauttaa **tehtävän<IOperation>**.

Seuraavat luokat tukee tällä hetkellä, kysely menetelmistä: **kanavaa**, **StreamingEndpoint**ja **ohjelma**.

Lisääminen toiminnon tilan, käytä **OperationBaseCollection** luokan **GetOperation** -menetelmää. Seuraavassa väleihin avulla voit tarkistaa toiminnon tilan: **kanavan** ja **StreamingEndpoint** toimintoja, käytä 30 sekuntia. Käytä **ohjelman** toimintoja, 10 sekuntia.


##<a name="example"></a>Esimerkki

Seuraava esimerkki määrittää **ChannelOperations**-luokka. Tämä luokan määritys voi olla aloituspiste web service-luokan-määritys. Seuraavissa esimerkeissä käytetään yksinkertaisuuden, menetelmät asynkroninen versioissa.

Esimerkki myös näytetään, miten asiakas voi käyttää tähän luokkaan.

###<a name="channeloperations-class-definition"></a>ChannelOperations luokan määritys

    /// <summary> 
    /// The ChannelOperations class only implements 
    /// the Channel’s creation operation. 
    /// </summary> 
    public class ChannelOperations
    {
        // Read values from the App.config file.
        private static readonly string _mediaServicesAccountName =
            ConfigurationManager.AppSettings["MediaServicesAccountName"];
        private static readonly string _mediaServicesAccountKey =
            ConfigurationManager.AppSettings["MediaServicesAccountKey"];
    
        // Field for service context.
        private static CloudMediaContext _context = null;
        private static MediaServicesCredentials _cachedCredentials = null;
    
        public ChannelOperations()
        {
                _cachedCredentials = new MediaServicesCredentials(_mediaServicesAccountName,
                    _mediaServicesAccountKey);
    
                _context = new CloudMediaContext(_cachedCredentials);    }
    
        /// <summary>  
        /// Initiates the creation of a new channel.  
        /// </summary>  
        /// <param name="channelName">Name to be given to the new channel</param>  
        /// <returns>  
        /// Operation Id for the long running operation being executed by Media Services. 
        /// Use this operation Id to poll for the channel creation status. 
        /// </returns> 
        public string StartChannelCreation(string channelName)
        {
            var operation = _context.Channels.SendCreateOperation(
                new ChannelCreationOptions
                {
                    Name = channelName,
                    Input = CreateChannelInput(),
                    Preview = CreateChannelPreview(),
                    Output = CreateChannelOutput()
                });
    
            return operation.Id;
        }
    
        /// <summary> 
        /// Checks if the operation has been completed. 
        /// If the operation succeeded, the created channel Id is returned in the out parameter.
        /// </summary> 
        /// <param name="operationId">The operation Id.</param> 
        /// <param name="channel">
        /// If the operation succeeded, 
        /// the created channel Id is returned in the out parameter.</param>
        /// <returns>Returns false if the operation is still in progress; otherwise, true.</returns> 
        public bool IsCompleted(string operationId, out string channelId)
        {
            IOperation operation = _context.Operations.GetOperation(operationId);
            bool completed = false;
    
            channelId = null;
    
            switch (operation.State)
            {
                case OperationState.Failed:
                    // Handle the failure. 
                    // For example, throw an exception. 
                    // Use the following information in the exception: operationId, operation.ErrorMessage.
                    break;
                case OperationState.Succeeded:
                    completed = true;
                    channelId = operation.TargetEntityId;
                    break;
                case OperationState.InProgress:
                    completed = false;
                    break;
            }
            return completed;
        }
    
    
        private static ChannelInput CreateChannelInput()
        {
            return new ChannelInput
            {
                StreamingProtocol = StreamingProtocol.RTMP,
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
    
        private static ChannelOutput CreateChannelOutput()
        {
            return new ChannelOutput
            {
                Hls = new ChannelOutputHls { FragmentsPerSegment = 1 }
            };
        }
    }

###<a name="the-client-code"></a>Asiakas-koodi

    ChannelOperations channelOperations = new ChannelOperations();
    string opId = channelOperations.StartChannelCreation("MyChannel001");
    
    string channelId = null;
    bool isCompleted = false;
    
    while (isCompleted == false)
    {
        System.Threading.Thread.Sleep(TimeSpan.FromSeconds(30));
        isCompleted = channelOperations.IsCompleted(opId, out channelId);
    }
    
    // If we got here, we should have the newly created channel id.
    Console.WriteLine(channelId);
 


##<a name="media-services-learning-paths"></a>Media-palveluiden oppimispolut

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Palautteen antaminen

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
