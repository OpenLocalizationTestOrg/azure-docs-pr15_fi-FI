<properties 
    pageTitle="Suojaa sisällöstä Apple FairPlay ja/tai Microsoft PlayReady oman HLS | Microsoft Azure" 
    description="Tässä ohjeaiheessa on yleiskuvaus ja kerrotaan, miten voit salata dynaamisesti HTTP Live Streaming (HLS) sisällön kanssa Apple FairPlay Azure Media-palveluiden avulla. Se näyttää myös käyttämisestä Media-palveluiden käyttöoikeus toimitus-palvelun aikana FairPlay käyttöoikeuksien asiakkaille." 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/27/2016"
    ms.author="juliako"/>

# <a name="protect-your-hls-content-with-apple-fairplay-andor-microsoft-playready"></a>Oman sisällön Apple FairPlay/tai Microsoft PlayReady HLS suojaaminen

Azure Media-palveluiden avulla voit salata dynaamisesti HTTP Live Streaming (HLS) sisällön käyttäminen seuraavissa muodoissa:  

- **AES-128 kirjekuori clear-näppäin** 

    Koko lohko salataan **AES-128 CBC** -tilassa. Tuetaan virta salauksen grafiikkatiedostomuotoja iOS- ja OS x toisto-ohjelman mukaan. Lisätietoja [Tässä artikkelissa](media-services-protect-with-aes128.md).

- **Apple FairPlay** 

    Yksittäisten ääni- ja mallit salataan **AES-128 CBC** -tilassa. **FairPlay Streaming** (FPS) on integroitu laite-käyttöjärjestelmissä alkuperäisen iOS- ja Apple TV-tukeen. OS x: ssä Safari mahdollistaa FPS käytössä salattu Media laajennukset (EME)-liittymän tuki.
- **Microsoft PlayReady**

Seuraava kuva esittää **HLS + FairPlay ja/tai PlayReady dynaaminen salaus** työnkulun.

![FairPlay suojaaminen](./media/media-services-content-protection-overview/media-services-content-protection-with-fairplay.png)

Tässä ohjeaiheessa kerrotaan, miten Azure Media-palveluiden avulla voit salata dynaamisesti HLS sisällön Apple FairPlay kanssa. Se näyttää myös käyttämisestä Media-palveluiden käyttöoikeus toimitus-palvelun aikana FairPlay käyttöoikeuksien asiakkaille.

>[AZURE.NOTE] Jos haluat salata HLS sisällön PlayReady kanssa, sinun on yleisiä sisällön avaimen luominen ja liittää sen oman resurssi. Haluat myös sisällön avain luvan käytännön määrittäminen [käyttämällä PlayReady dynaaminen yhteistä salaus](media-services-protect-with-drm.md) ohjeaiheessa kuvatulla tavalla.

    
## <a name="requirements-and-considerations"></a>Vaatimukset ja huomioon otettavia seikkoja

- Seuraavassa on tarvittavat käytettäessä AMS aikana salattu FairPlay HLS ja pitää FairPlay käyttöoikeudet.

    - Azure tili. Lisätietoja on artikkelissa [Azure maksuttoman kokeiluversion](/pricing/free-trial/?WT.mc_id=A261C142F).
    - Media Services-tilin. Media Services-tilin luominen-kohdassa [Luo tili](media-services-portal-create-account.md).
    - Tilaa [Apple kehittäminen-ohjelman](https://developer.apple.com/)kanssa.
    - Apple edellyttää sisällön omistaja hankkiminen [käyttöönottopaketti](https://developer.apple.com/contact/fps/). Maininta voit toteuttaa jo KSM (avain suojauksen moduuli) Azure Media-palvelujen kanssa ja pyydät lopullinen FPS paketin pyynnön. Ole lopullinen FPS paketin Luo sertifikaatin ja KYSY, joiden aiot käyttää määrittämään FairPlay Hanki ohjeita. 

    - Azure Media Services .NET SDK-version **3.6.0** tai uudempi versio.

- AMS avaimen toimituksen reunassa on määritettävä seuraavat kohdat:
    - **Sovelluksen varmenne (AC)** - .pfx-tiedosto, joka sisältää yksityinen avain. Tämä tiedosto luodaan asiakkaan ja salasana salataan saman asiakkaan. 
        
        Kun asiakkaan määrittää avaimen toimituksen käytäntö, ne on määritettävä salasana ja .pfx base64-muodossa.

        Seuraavat vaiheet kerrotaan, kuinka voit luoda pfx varmenteen FairPlay.
        
        1. Asenna OpenSSL https://slproweb.com/products/Win32OpenSSL.html
        
            Siirry kansioon, jossa FairPlay varmenne ja muita tiedostoja Apple toimittaman ovat.
        
        2. Komentorivin cer muuntaminen pem:
        
            "C:\OpenSSL-Win32\bin\openssl.exe" x509-ilmoittaa der-kohdassa fairplay.cer-fairplay out.pem ulos
        
        3. Komentorivi pem muuntaminen pfx ja yksityinen avain (pfx-tiedoston salasanan pyydetään sitten OpenSSL mukaan).
        
            "C:\OpenSSL-Win32\bin\openssl.exe" pkcs12-Vie - kohdassa fairplay out.pfx-inkey privatekey.pem-fairplay out.pem - passin file:privatekey-pem-pass.txt: 
        
    - **Sovelluksen varmenteen salasana** - asiakkaan salasanan luominen .pfx-tiedosto.
    - **Sovelluksen varmenteen Salasanatunnus** - asiakkaan täytyy ladata samalla siitä, miten ne ladata muita AMS näppäimet ja **ContentKeyType.FairPlayPfxPassword** luettelo arvon käyttäminen salasana. Tuloksena he saavat AMS tunnus tämä on tarvitsemansa sisällä avaimen toimitustapa käytännön avulla.
    - **iv** – 16 tavua satunnaisen arvon on oltava samat iv resurssi toimituksen käytännössä. Asiakas IV Luo ja sijoittaa sen molemmista sijainneista: kohteiden toimituksen käytännön ja avaimen avulla toimituksen käytännön asetus. 
    - **ASK** - KYSY (sovellusnäppäintä salaisuus) vastaanotetaan Apple Developer-portaalissa sertifikaatin luomisessa. Kehittäminen kunkin ryhmän saavat yksilöllinen KYSY. Tallenna kopio KYSY ja tallenna se turvallisessa paikassa. Sinun on määritettävä KYSY FairPlayAsk myöhemmin Azure Media-palveluihin. 
    -  **PYYDÄ tunnus** - saadaan, kun asiakkaan latauksia AMS tuominen Kysy. Asiakkaan täytyy ladata Kysy **ContentKeyType.FairPlayASk** luettelo arvon avulla. Tulos on AMS tunnus palautetaan ja tämä on mitä käytetään, kun avaimen toimitus-käytännön asetus.

- FPS asiakaspuolen on määritettävä seuraavat kohdat:
    - **Sovelluksen varmenne (AC)** -.cer/.der tiedoston julkisella avaimella, jossa OS käyttää joidenkin tietojen salaamiseen. AMS tarvitsee tietää tietoja, koska sitä edellytä toisto-ohjelman. Avaimen toimitus-palvelun purkaa käyttämällä vastaava yksityinen avain.

- Toistamisen FairPlay salattuja stream tarvitset hakee reaali KYSY ensimmäisen ja luo sitten reaali varmenne. Prosessin Luo kaikki 3 osat:

    -  .der, 
    -  .pfx ja 
    -  .pfx salasana.
 
- Asiakkaat, jotka tukevat HLS **AES-128 CBC** salauksella: iOS OS X-Apple TV Safari.

##<a name="steps-for-configuring-fairplay-dynamic-encryption-and-license-delivery-services"></a>Ohjeet FairPlay dynaaminen salaus- ja käyttöoikeustiedostojen toimituksen palveluiden määrittäminen

Seuraavassa on Yleiset vaiheet, sinun on suoritettava Kun suojaat oman resurssien FairPlay käyttämällä Media-palveluiden käyttöoikeus palvelua ja käyttää myös dynaaminen salausta.

1. Luo sijoituksen ja ladata tiedostoja kohteen esittely. 
1. Koodata MP4 määrittäminen säädön bittitaajuus tiedoston sisältävä resurssi.
1. Luo sisällön avain ja liittää sen koodattu resurssi.  
1. Sisällön avain luvan käytännön määrittäminen. Sisällön avaimen luvan käytännön luotaessa sinun on määritettävä seuraavasti: 
    
    - Toimitustapa (eli tässä tapauksessa FairPlay), 
    - FairPlay käytännön asetusten. Lisätietoja FairPlay määrittämisestä on artikkelissa ConfigureFairPlayPolicyOptions() menetelmä alla olevassa esimerkissä.
    
        >[AZURE.NOTE] Yleensä haluat ehkä määrittää FairPlay käytäntöasetukset vain kerran, koska käytössäsi on vain yksi joukko sertifikaatin ja KYSY.
-(Avaa tai tunnus) - rajoituksia ja avaimen toimituksen tyyppi, joka määrittää, miten avain toimitetaan asiakkaan tietoja. 
    
2. Kohteiden toimituksen käytännön määrittäminen. Toimituksen käytännön määritys sisältyy: 

    - toimitus-protokolla (HLS) 
    - Dynaaminen salaus (yhteiset CBC salaus)-tyyppi 
    - käyttöoikeuden hankkiminen URL-osoite. 
    
    >[AZURE.NOTE]Jos haluat pitää muodossa, joka on suojattu FairPlay + toisen DRM, sinun on määritettävä erillinen toimituksen käytännöt 
    >
    >- Yksi IAssetDeliveryPolicy määrittää VIIVAN CENC (PlayReady + WideVine) ja tasainen PlayReady kanssa. 
    >- Toisen IAssetDeliveryPolicy FairPlay määrittämiseksi HLS

1. Luo OnDemand-locator streaming URL-Osoitteen hankkiminen.

##<a name="using-fairplay-key-delivery-by-playerclient-apps"></a>FairPlay avaimen toimituksen käyttämällä Playerin/asiakassovellukset

Asiakkaat voivat kehittää player-sovellusten käyttäminen iOS SDK. Jotta voit toistaa FairPlay sisältöä asiakkaiden on Toteuta käyttöoikeuden exchange-protokollaa. Käyttöoikeuden exchange-protokollaa ei ole määritetty Apple mukaan. On enintään kunkin sovelluksen lähettäminen avaimen toimituksen pyynnöt. AMS FairPlay avaimen toimitus-palveluiden odottaa SPC tulee www-form-url koodattu kirjaa viestinä seuraavassa muodossa: 

    spc=<Base64 encoded SPC>

>[AZURE.NOTE] Azure Media Player ei tue FairPlay toiston ulos ruutuun. Asiakkaiden on hankittava otoksen Playerin Apple Kehitystyökalut-tilin avulla opit FairPlay toiston MAC OS x. 
 
##<a name="streaming-urls"></a>Streaming URL-osoitteet

Jos yhteyttä resurssi on salattu useita DRM, kannattaa käyttää salaus-tunniste streaming URL-osoitteen: (Muotoile = 'm3u8-aapl' salaus = "xxx").

Ottaa huomioon seuraavat asiat:

- Vain nolla tai yksi salaustyyppi voidaan määrittää.
- Salauksen tyyppi ei tarvitse erikseen URL-osoite, jos vain yksi salaus käytetty kohteen.
- Salauksen tyyppi on kirjainkokoa.
- Salauksen seuraavanlaisia voidaan määrittää:  
    - **cenc**: yleisiä salaus (Playready tai Widevine)
    - **cbcs aapl**: Fairplay
    - **CBC**: Kirjekuori AES-salausta.


##<a name="net-example"></a>.NET-Esimerkki


Seuraavassa esimerkissä on toimintoja, jotka otettiin Azure Media Services SDK .net-Version 3.6.0 (voi käyttää Azure Media Services aikana salattu FairPlay sisältöä). Asenna paketti käytettiin Nuget paketti-komennon:

    PM> Install-Package windowsazure.mediaservices -Version 3.6.0


1. Luo Console-projekti.
1. NuGet avulla voit asentaa ja lisätä Azure Media Services .NET SDK.
2. Lisää lisäresurssit: System.Configuration.
2. Lisää määritystiedosto, joka sisältää tilin nimi ja tiedot:
    
        <?xml version="1.0" encoding="utf-8"?>
        <configuration>
            <startup> 
                <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5" />
            </startup>
              <appSettings>
            
                <add key="MediaServicesAccountName" value="AccountName"/>
                <add key="MediaServicesAccountKey" value="AccountKey"/>
            
                <add key="Issuer" value="http://testacs.com"/>
                <add key="Audience" value="urn:test"/>
              </appSettings>
        </configuration>

1. Vähintään yhden streaming yksikön streaming päätepisteen, josta aiot toimituksen sisällön hakeminen Lisätietoja on artikkelissa: [määrittää streaming päätepisteet](media-services-dotnet-get-started.md#configure-streaming-endpoint-using-the-portal).

1. Korvaa tämän osan esitetyn koodin koodin Program.cs-tiedostossa.
            
        
        using System;
        using System.Collections.Generic;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using System.Threading;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using Microsoft.WindowsAzure.MediaServices.Client.ContentKeyAuthorization;
        using Microsoft.WindowsAzure.MediaServices.Client.DynamicEncryption;
        using Microsoft.WindowsAzure.MediaServices.Client.FairPlay;
        using Newtonsoft.Json;
        using System.Security.Cryptography.X509Certificates;
        
        namespace DynamicEncryptionWithFairPlay
        {
            class Program
            {
                // Read values from the App.config file.
                private static readonly string _mediaServicesAccountName =
                    ConfigurationManager.AppSettings["MediaServicesAccountName"];
                private static readonly string _mediaServicesAccountKey =
                    ConfigurationManager.AppSettings["MediaServicesAccountKey"];
        
                private static readonly Uri _sampleIssuer =
                    new Uri(ConfigurationManager.AppSettings["Issuer"]);
                private static readonly Uri _sampleAudience =
                    new Uri(ConfigurationManager.AppSettings["Audience"]);
        
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
                    // Used the cached credentials to create CloudMediaContext.
                    _context = new CloudMediaContext(_cachedCredentials);
        
                    bool tokenRestriction = false;
                    string tokenTemplateString = null;
        
                    IAsset asset = UploadFileAndCreateAsset(_singleMP4File);
                    Console.WriteLine("Uploaded asset: {0}", asset.Id);
        
                    IAsset encodedAsset = EncodeToAdaptiveBitrateMP4Set(asset);
                    Console.WriteLine("Encoded asset: {0}", encodedAsset.Id);
        
                    IContentKey key = CreateCommonCBCTypeContentKey(encodedAsset);
                    Console.WriteLine("Created key {0} for the asset {1} ", key.Id, encodedAsset.Id);
                    Console.WriteLine("FairPlay License Key delivery URL: {0}", key.GetKeyDeliveryUrl(ContentKeyDeliveryType.FairPlay));
                    Console.WriteLine();
        
                    if (tokenRestriction)
                        tokenTemplateString = AddTokenRestrictedAuthorizationPolicy(key);
                    else
                        AddOpenAuthorizationPolicy(key);
        
                    Console.WriteLine("Added authorization policy: {0}", key.AuthorizationPolicyId);
                    Console.WriteLine();
        
                    CreateAssetDeliveryPolicy(encodedAsset, key);
                    Console.WriteLine("Created asset delivery policy. \n");
                    Console.WriteLine();
        
                    if (tokenRestriction && !String.IsNullOrEmpty(tokenTemplateString))
                    {
                        // Deserializes a string containing an Xml representation of a TokenRestrictionTemplate
                        // back into a TokenRestrictionTemplate class instance.
                        TokenRestrictionTemplate tokenTemplate =
                            TokenRestrictionTemplateSerializer.Deserialize(tokenTemplateString);
        
                        // Generate a test token based on the the data in the given TokenRestrictionTemplate.
                        // Note, you need to pass the key id Guid because we specified 
                        // TokenClaim.ContentKeyIdentifierClaim in during the creation of TokenRestrictionTemplate.
                        Guid rawkey = EncryptionUtils.GetKeyIdAsGuid(key.Id);
                        string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate, null, rawkey,
                                                                                DateTime.UtcNow.AddDays(365));
                        Console.WriteLine("The authorization token is:\nBearer {0}", testToken);
                        Console.WriteLine();
                    }
        
                    string url = GetStreamingOriginLocator(encodedAsset);
                    Console.WriteLine("Encrypted HLS URL: {0}/manifest(format=m3u8-aapl)", url);
        
                    Console.ReadLine();
                }
        
                static public IAsset UploadFileAndCreateAsset(string singleFilePath)
                {
                    if (!File.Exists(singleFilePath))
                    {
                        Console.WriteLine("File does not exist.");
                        return null;
                    }
        
                    var assetName = Path.GetFileNameWithoutExtension(singleFilePath);
                    IAsset inputAsset = _context.Assets.Create(assetName, AssetCreationOptions.None);
        
                    var assetFile = inputAsset.AssetFiles.Create(Path.GetFileName(singleFilePath));
        
                    Console.WriteLine("Created assetFile {0}", assetFile.Name);
        
                    var policy = _context.AccessPolicies.Create(
                                            assetName,
                                            TimeSpan.FromDays(30),
                                            AccessPermissions.Write | AccessPermissions.List);
        
                    var locator = _context.Locators.CreateLocator(LocatorType.Sas, inputAsset, policy);
        
                    Console.WriteLine("Upload {0}", assetFile.Name);
        
                    assetFile.Upload(singleFilePath);
                    Console.WriteLine("Done uploading {0}", assetFile.Name);
        
                    locator.Delete();
                    policy.Delete();
        
                    return inputAsset;
                }
        
                static public IAsset EncodeToAdaptiveBitrateMP4Set(IAsset inputAsset)
                {
                    var encodingPreset = "H264 Multiple Bitrate 720p";
        
                    IJob job = _context.Jobs.Create(String.Format("Encoding into Mp4 {0} to {1}",
                                            inputAsset.Name,
                                            encodingPreset));
        
                    var mediaProcessors =
                        _context.MediaProcessors.Where(p => p.Name.Contains("Media Encoder Standard")).ToList();
        
                    var latestMediaProcessor =
                        mediaProcessors.OrderBy(mp => new Version(mp.Version)).LastOrDefault();
        
                    ITask encodeTask = job.Tasks.AddNew("Encoding", latestMediaProcessor, encodingPreset, TaskOptions.None);
                    encodeTask.InputAssets.Add(inputAsset);
                    encodeTask.OutputAssets.AddNew(String.Format("{0} as {1}", inputAsset.Name, encodingPreset),    AssetCreationOptions.StorageEncrypted);
        
                    job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
                    job.Submit();
                    job.GetExecutionProgressTask(CancellationToken.None).Wait();
        
                    return job.OutputMediaAssets[0];
                }
        
                static public IContentKey CreateCommonCBCTypeContentKey(IAsset asset)
                {
                    // Create HLS SAMPLE AES encryption content key
                    Guid keyId = Guid.NewGuid();
                    byte[] contentKey = GetRandomBuffer(16);
        
                    IContentKey key = _context.ContentKeys.Create(
                                            keyId,
                                            contentKey,
                                            "ContentKey",
                                            ContentKeyType.CommonEncryptionCbcs);
        
                    // Associate the key with the asset.
                    asset.ContentKeys.Add(key);
        
                    return key;
                }
        
        
                static public void AddOpenAuthorizationPolicy(IContentKey contentKey)
                {
                    // Create ContentKeyAuthorizationPolicy with Open restrictions 
                    // and create authorization policy          
        
                    List<ContentKeyAuthorizationPolicyRestriction> restrictions = new List<ContentKeyAuthorizationPolicyRestriction>
                            {
                                new ContentKeyAuthorizationPolicyRestriction
                                {
                                    Name = "Open",
                                    KeyRestrictionType = (int)ContentKeyRestrictionType.Open,
                                    Requirements = null
                                }
                            };
        
        
                    // Configure FairPlay policy option.
                    string FairPlayConfiguration = ConfigureFairPlayPolicyOptions();
        
                    IContentKeyAuthorizationPolicyOption FairPlayPolicy =
                        _context.ContentKeyAuthorizationPolicyOptions.Create("",
                        ContentKeyDeliveryType.FairPlay,
                        restrictions,
                        FairPlayConfiguration);
        
        
                    IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                                ContentKeyAuthorizationPolicies.
                                CreateAsync("Deliver Common CBC Content Key with no restrictions").
                                Result;
        
                    contentKeyAuthorizationPolicy.Options.Add(FairPlayPolicy);
        
                    // Associate the content key authorization policy with the content key.
                    contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
                    contentKey = contentKey.UpdateAsync().Result;
                }
        
                public static string AddTokenRestrictedAuthorizationPolicy(IContentKey contentKey)
                {
                    string tokenTemplateString = GenerateTokenRequirements();
        
                    List<ContentKeyAuthorizationPolicyRestriction> restrictions = new List<ContentKeyAuthorizationPolicyRestriction>
                            {
                                new ContentKeyAuthorizationPolicyRestriction
                                {
                                    Name = "Token Authorization Policy",
                                    KeyRestrictionType = (int)ContentKeyRestrictionType.TokenRestricted,
                                    Requirements = tokenTemplateString,
                                }
                            };
        
                    // Configure FairPlay policy option.
                    string FairPlayConfiguration = ConfigureFairPlayPolicyOptions();
        
        
                    IContentKeyAuthorizationPolicyOption FairPlayPolicy =
                        _context.ContentKeyAuthorizationPolicyOptions.Create("Token option",
                               ContentKeyDeliveryType.FairPlay,
                               restrictions,
                               FairPlayConfiguration);
        
                    IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                                ContentKeyAuthorizationPolicies.
                                CreateAsync("Deliver Common CBC Content Key with token restrictions").
                                Result;
        
                    contentKeyAuthorizationPolicy.Options.Add(FairPlayPolicy);
        
                    // Associate the content key authorization policy with the content key
                    contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
                    contentKey = contentKey.UpdateAsync().Result;
        
                    return tokenTemplateString;
                }
        
                private static string ConfigureFairPlayPolicyOptions()
                {
                    // For testing you can provide all zeroes for ASK bytes together with the cert from Apple FPS SDK. 
                    // However, for production you must use a real ASK from Apple bound to a real prod certificate.
                    byte[] askBytes = Guid.NewGuid().ToByteArray();
                    var askId = Guid.NewGuid();
                    // Key delivery retrieves askKey by askId and uses this key to generate the response.
                    IContentKey askKey = _context.ContentKeys.Create(
                                            askId,
                                            askBytes,
                                            "askKey",
                                            ContentKeyType.FairPlayASk);
        
                    //Customer password for creating the .pfx file.
                    string pfxPassword = "<customer password for creating the .pfx file>";
                    // Key delivery retrieves pfxPasswordKey by pfxPasswordId and uses this key to generate the response.
                    var pfxPasswordId = Guid.NewGuid();
                    byte[] pfxPasswordBytes = System.Text.Encoding.UTF8.GetBytes(pfxPassword);
                    IContentKey pfxPasswordKey = _context.ContentKeys.Create(
                                            pfxPasswordId,
                                            pfxPasswordBytes,
                                            "pfxPasswordKey",
                                            ContentKeyType.FairPlayPfxPassword);
        
                    // iv - 16 bytes random value, must match the iv in the asset delivery policy.
                    byte[] iv = Guid.NewGuid().ToByteArray();
        
                    //Specify the .pfx file created by the customer.
                    var appCert = new X509Certificate2("path to the .pfx file created by the customer", pfxPassword, X509KeyStorageFlags.Exportable);
        
                    string FairPlayConfiguration =
                        Microsoft.WindowsAzure.MediaServices.Client.FairPlay.FairPlayConfiguration.CreateSerializedFairPlayOptionConfiguration(
                            appCert,
                            pfxPassword,
                            pfxPasswordId,
                            askId,
                            iv);
        
                    return FairPlayConfiguration;
                }
        
                static private string GenerateTokenRequirements()
                {
                    TokenRestrictionTemplate template = new TokenRestrictionTemplate(TokenType.SWT);
        
                    template.PrimaryVerificationKey = new SymmetricVerificationKey();
                    template.AlternateVerificationKeys.Add(new SymmetricVerificationKey());
                    template.Audience = _sampleAudience.ToString();
                    template.Issuer = _sampleIssuer.ToString();
                    template.RequiredClaims.Add(TokenClaim.ContentKeyIdentifierClaim);
        
                    return TokenRestrictionTemplateSerializer.Serialize(template);
                }
        
                static public void CreateAssetDeliveryPolicy(IAsset asset, IContentKey key)
                {
                    var kdPolicy = _context.ContentKeyAuthorizationPolicies.Where(p => p.Id == key.AuthorizationPolicyId).Single();
        
                    var kdOption = kdPolicy.Options.Single(o => o.KeyDeliveryType == ContentKeyDeliveryType.FairPlay);
        
                    FairPlayConfiguration configFP = JsonConvert.DeserializeObject<FairPlayConfiguration>(kdOption.KeyDeliveryConfiguration);
        
                    // Get the FairPlay license service URL.
                    Uri acquisitionUrl = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.FairPlay);
        
                    // The reason the below code replaces "https://" with "skd://" is because
                    // in the IOS player sample code which you obtained in Apple developer account, 
                    // the player only recognizes a Key URL that starts with skd://. 
                    // However, if you are using a customized player, 
                    // you can choose whatever protocol you want. 
                    // For example, "https". 

                    Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
                        new Dictionary<AssetDeliveryPolicyConfigurationKey, string>
                        {
                            {AssetDeliveryPolicyConfigurationKey.FairPlayLicenseAcquisitionUrl, acquisitionUrl.ToString().Replace("https://", "skd://")},
                            {AssetDeliveryPolicyConfigurationKey.CommonEncryptionIVForCbcs, configFP.ContentEncryptionIV}
                        };
        
                    var assetDeliveryPolicy = _context.AssetDeliveryPolicies.Create(
                            "AssetDeliveryPolicy",
                        AssetDeliveryPolicyType.DynamicCommonEncryptionCbcs,
                        AssetDeliveryProtocol.HLS,
                        assetDeliveryPolicyConfiguration);
        
                    // Add AssetDelivery Policy to the asset
                    asset.DeliveryPolicies.Add(assetDeliveryPolicy);
        
                }
        
        
                /// <summary>
                /// Gets the streaming origin locator.
                /// </summary>
                /// <param name="assets"></param>
                /// <returns></returns>
                static public string GetStreamingOriginLocator(IAsset asset)
                {
        
                    // Get a reference to the streaming manifest file from the  
                    // collection of files in the asset. 
        
                    var assetFile = asset.AssetFiles.Where(f => f.Name.ToLower().
                                                 EndsWith(".ism")).
                                                 FirstOrDefault();
        
                    // Create a 30-day readonly access policy. 
                    IAccessPolicy policy = _context.AccessPolicies.Create("Streaming policy",
                        TimeSpan.FromDays(30),
                        AccessPermissions.Read);
        
                    // Create a locator to the streaming content on an origin. 
                    ILocator originLocator = _context.Locators.CreateLocator(LocatorType.OnDemandOrigin, asset,
                        policy,
                        DateTime.UtcNow.AddMinutes(-5));
        
                    // Create a URL to the manifest file. 
                    return originLocator.Path + assetFile.Name;
                }
        
                static private void JobStateChanged(object sender, JobStateChangedEventArgs e)
                {
                    Console.WriteLine(string.Format("{0}\n  State: {1}\n  Time: {2}\n\n",
                        ((IJob)sender).Name,
                        e.CurrentState,
                        DateTime.UtcNow.ToString(@"yyyy_M_d__hh_mm_ss")));
                }
        
                static private byte[] GetRandomBuffer(int length)
                {
                    var returnValue = new byte[length];
        
                    using (var rng =
                        new System.Security.Cryptography.RNGCryptoServiceProvider())
                    {
                        rng.GetBytes(returnValue);
                    }
        
                    return returnValue;
                }
            }
        }


##<a name="next-steps-media-services-learning-paths"></a>Seuraavat vaiheet: Media-palveluiden oppimispolut

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Palautteen antaminen

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
