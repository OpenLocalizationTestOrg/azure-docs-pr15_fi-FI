<properties
    pageTitle="Käyttää PlayReady ja/tai Widevine dynaaminen yleisiä salausta | Microsoft Azure"
    description="Microsoft Azure Media-palveluiden avulla voit pitää MPEG-VIIVA, tasainen Streaming ja Http-Live-virtautetun median (HLS) virtaa Microsoft PlayReady DRM suojattu. Se myös mahdollistaa toimituksen salattu Widevine DRM VIIVA. Tässä ohjeaiheessa esitellään, miten voit salata dynaamisesti PlayReady ja Widevine DRM."
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
    ms.topic="get-started-article" 
    ms.date="09/27/2016"
    ms.author="juliako"/>


#<a name="using-playready-andor-widevine-dynamic-common-encryption"></a>Käyttää PlayReady ja/tai Widevine dynaaminen yleisiä salausta

> [AZURE.SELECTOR]
- [.NET](media-services-protect-with-drm.md)
- [Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
- [PHP](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)

Microsoft Azure Media-palveluiden avulla voit pitää MPEG-VIIVA, tasainen Streaming ja HTTP-Live-virtautetun median (HLS) virtaa [Microsoft PlayReady DRM](https://www.microsoft.com/playready/overview/)suojattu. Voit myös salattujen VIIVA virtaa toimitetaan Widevine DRM käyttöoikeuksia. Yleisiä salaus (ISO/IEC 23001 7 CENC) määrityksen kohden salattuja PlayReady ja Widevine. Voit käyttää [AMS.NET SDK](https://www.nuget.org/packages/windowsazure.mediaservices/) (alkaa numerosta versio 3.5.1) tai REST API määrittäminen käyttämään Widevine oman AssetDeliveryConfiguration.

Media-palveluiden tarjoaa palvelun välittää PlayReady ja Widevine DRM käyttöoikeudet. Media-palveluiden sisältää myös API, joiden avulla voit määrittää oikeudet ja rajoitukset, jota haluat käyttää PlayReady tai Widevine DRM runtime voidaan valvoa, kun käyttäjä toistaa suojatun sisällön. Kun käyttäjä pyytää DRM suojatun sisällön, player-sovellus pyytää käyttöoikeuden AMS käyttöoikeus-palvelusta. AMS käyttöoikeuden palveluun antaa käyttöoikeuden toisto-ohjelman, jos se on sallittua. PlayReady tai Widevine-käyttöoikeus sisältää salauksen avainta, jota voidaan käyttää asiakkaan Playerin salaus ja käyttää sisältöä.


Voit käyttää myös seuraavat AMS kumppanien avulla voit pitää Widevine käyttöoikeudet: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/) [castLabs](http://castlabs.com/company/partners/azure/). Lisätietoja on artikkelissa: [Axinom](media-services-axinom-integration.md) ja [castLabs](media-services-castlabs-integration.md).

Media-palveluiden tukee useita tapoja sallimisesta käyttäjät, jotka avaimen pyytää. Sisällön avaimen todennus-käytäntö voi olla vähintään yksi käyttöoikeuksien rajoitukset: Avaa tai tunnussanoma rajoitus. Suojaustunnuksen rajoitettu käytäntö on liitettävä mukaan suojattu suojaustunnuksen Service (STS) antanut tunnusta. Media-palveluiden tukee tunnusten [Yksinkertaisen Web tunnusten](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2) (SWT)-muodossa ja [JSON Web suojaustunnuksen](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_3) (JWT)-muodossa. Lisätietoja on artikkelissa sisällön avain luvan käytännön määrittäminen.

Haluat hyödyntää dynaaminen salaus on resurssi, joka sisältää usean bittitaajuus MP4 tiedostojen tai usean bittitaajuus tasainen Streaming lähdetiedostot. Haluat myös määrittää toimituksen käytäntöjä, annetaan (kuvattu tämän artikkelin). Sitten määritetyssä streaming URL-osoitteen mukaan edelleen tarvittaessa Streaming palvelimen varmistat, että olet valinnut protokolla toimitetaan virta. Tuloksena tarvitset vain tallentamiseen ja maksaa yhden Tallennusmuoto tiedostoja ja Media-palveluiden luominen ja yhteyshenkilönä tarvittavat kunkin asiakaskoneesta pyynnön mukaisesti HTTP-vastaus.

Tässä ohjeaiheessa olla hyötyä kehittäjät, jotka työskentelevät sovellukset, jotka toimittavat media useita DRMs, kuten PlayReady ja Widevine suojattu. Ohjeaiheessa esitellään, miten PlayReady käyttöoikeuden toimitus-palvelun määrittäminen luvan käytäntöjen kanssa niin, että vain valtuutettujen asiakkaiden saada PlayReady tai Widevine käyttöoikeuksia. Se näyttää myös käyttämisestä dynaaminen salaus salaus PlayReady tai Widevine DRM VIIVAN päällä.

>[AZURE.NOTE]Aloita käyttäminen dynaamisen salausta, sinun täytyy saada vähintään yksi asteikko (tunnetaan myös nimellä streaming yksikkö). Lisätietoja on artikkelissa [miten Media-palvelun](media-services-portal-manage-streaming-endpoints.md).


##<a name="download-sample"></a>Lataa malli

Voit ladata otosten [täältä](https://github.com/Azure-Samples/media-services-dotnet-dynamic-encryption-with-drm)tässä artikkelissa kuvatulla tavalla.

##<a name="configuring-dynamic-common-encryption-and-drm-license-delivery-services"></a>Dynaaminen yleisiä salaus ja DRM käyttöoikeuden toimituksen palveluiden määrittämisestä

Seuraavassa on Yleiset vaiheet, sinun on suoritettava Kun suojaat oman resurssien PlayReady käyttämällä Media-palveluiden käyttöoikeus palvelua ja käyttää myös dynaaminen salausta.

1. Luo sijoituksen ja ladata tiedostoja kohteen esittely.
1. Koodata MP4 määrittäminen säädön bittitaajuus tiedoston sisältävä resurssi.
1. Luo sisällön avain ja liittää sen koodattu resurssi. Media-palveluiden sisällön avain sisältää kohteen salausavaimen.
1. Sisällön avain luvan käytännön määrittäminen. Sisällön avaimen todennus-käytäntö on voit määrittää ja täyttää asiakkaan järjestyksessä sisällön näppäimen toimitetaan asiakkaalle.

Sisällön avaimen luvan käytännön luotaessa sinun on määritettävä seuraavat: menetelmä (PlayReady tai Widevine) toimitusrajoitukset (Avaa tai suojaustunnuksen) ja tietoja avaimen toimituksen tyyppi, joka määrittää, miten avain toimitetaan asiakkaan ([PlayReady](media-services-playready-license-template-overview.md) tai [Widevine](media-services-widevine-license-template-overview.md) -käyttöoikeuden malli).
1. Määritä, annetaan toimitus-käytäntö. Toimituksen käytännön määritys sisältyy: toimitus-protokolla (kuten MPEG VIIVA, HLS, HDS, tasainen Streaming tai kaikki), dynaaminen salaus (esimerkiksi yhteistä salaus) PlayReady tai Widevine käyttöoikeuden hankkiminen URL-osoite tyyppi.

Eri voitu koske kutakin saman resurssi-protokollaa. Voit esimerkiksi lisätä PlayReady salaus tasainen/VIIVA ja AES kirjekuorta HLS. Protokollat, joka ei ole määritetty toimitus-käytäntö (esimerkiksi lisätä yhden käytännön, joka määrittää vain protokollaksi HLS) estetty streaming. Poikkeus tähän on, jos sinulla ei ole resurssi toimituksen käytäntö on määritetty lainkaan. Valitse sitten Poista sallittu kaikki protokollat.
1. Luo OnDemand locator, jotta saat streaming URL-osoite.

Etsii valmis .NET-Esimerkki ohjeaiheen lopussa.

Seuraavassa kuvassa näytetään edellä kuvatun työnkulun. Tähän tunnuksen käytetään todennusta varten.

![PlayReady suojaaminen](./media/media-services-content-protection-overview/media-services-content-protection-with-drm.png)

Tässä ohjeaiheessa muiden antaa etsimistä, koodiesimerkkejä ja linkkejä ohjeaiheisiin, kuinka saavuttamiseksi kuvattuun tehtävät.

##<a name="current-limitations"></a>Nykyisen rajoitukset

Jos Lisää tai päivitä resurssi-toimitus-käytäntö, poista liittyvät locator (jos saatavilla) ja luo uusi locator.

Rajoitukset, kun salaamaan Widevine Azure Media-palvelujen kanssa: tällä hetkellä sisällön useita näppäimiä ei tueta.

##<a name="create-an-asset-and-upload-files-into-the-asset"></a>Luo sijoituksen ja ladata tiedostoja kohteen esittely

Hallitse koodata ja virtauttaa videoita, jotta lataa ensin sisällön tuominen Microsoft Azure Media Services. Kun ladattu palvelimeen, sisällön tallennetaan Pilvipalvelun lisäkäsittelyä ja streaming suojatusti.

Lisätietoja on kohdassa [Lataa tiedostoja Media Services-tilille](media-services-dotnet-upload-files.md).

##<a name="encode-the-asset-containing-the-file-to-the-adaptive-bitrate-mp4-set"></a>Koodata MP4 määrittäminen mukautuvat bittitaajuus tiedoston sisältävä resurssi

Dynaaminen salauksella tarvitset on luotava resurssi, joka sisältää usean bittitaajuus MP4 tiedostojen tai usean bittitaajuus tasainen Streaming lähdetiedostot. Sitten luettelo ja osa-pyynnössä määritetyn muodon mukaan edelleen tarvittaessa Streaming palvelimen varmistat, että näyttöön tulee virta valitsemasi protokolla. Tuloksena tarvitset vain tallentamiseen ja maksaa yhden Tallennusmuoto tiedostot ja Media-palvelut luominen ja yhteyshenkilönä oikea vastaus asiakaskoneesta pyyntöjen perusteella. Lisätietoja on aiheessa [Dynaaminen pakkaus yleiskatsaus](media-services-dynamic-packaging-overview.md) .

Ohjeita siitä, miten voit salata, katso, [miten voit salata käyttämällä Media Encoder vakio resurssi](media-services-dotnet-encode-with-media-encoder-standard.md).


##<a id="create_contentkey"></a>Luo sisällön avain ja liittää sen koodattu resurssi

Media-palveluiden sisällön avain sisältää avaimen, jonka haluat salata omaisuuden kanssa.

Lisätietoja on Katso [Luo sisällön avain](media-services-dotnet-create-contentkey.md).


##<a id="configure_key_auth_policy"></a>Sisällön avain luvan käytännön määrittäminen

Media-palveluiden tukee useita tapoja käyttäjät, jotka avaimen pyytää todennustapa. Sisällön avaimen todennus-käytäntö on määritetty sinä ja täyttää (player)-asiakasohjelman järjestyksessä toimitetaan asiakas-näppäimen. Sisällön avaimen todennus-käytäntö voi olla vähintään yksi käyttöoikeuksien rajoitukset: Avaa tai tunnussanoma rajoitus.

Lisätietoja on artikkelissa [Sisällön avain luvan käytännön määrittäminen](media-services-dotnet-configure-content-key-auth-policy.md#playready-dynamic-encryption).

##<a id="configure_asset_delivery_policy"></a>Kohteiden toimituksen käytännön määrittäminen 

Määritä, että annetaan toimitus-käytäntö. Joitakin toimintoja, jotka resurssi toimituksen käytännön määritys sisältyy:

- DRM käyttöoikeuden hankkiminen URL-osoite. 
- Kohteiden toimituksen protokolla (esimerkiksi MPEG VIIVA, HLS, HDS, tasainen Streaming tai kaikki). 
- Dynaaminen salaus (eli tässä tapauksessa yleisiä salaus) tyyppi. 

Saat yksityiskohtaiset tiedot [määrittäminen resurssi toimituksen käytännön ](media-services-rest-configure-asset-delivery-policy.md).

##<a id="create_locator"></a>Luo OnDemand streaming locator, jotta saat streaming URL-osoite

Haluat antaa käyttäjän sujuvat, VIIVAN tai HLS streaming URL-osoite.

>[AZURE.NOTE]Jos lisäät tai päivittää oman resurssi toimituksen käytäntö, poista aiemmin locator (jos saatavilla) ja luo uusi locator.

Ohjeita siitä, miten voit julkaista sijoituksen ja luoda streaming URL-osoite on artikkelissa [luominen streaming URL-osoite](media-services-deliver-streaming-content.md).

##<a name="get-a-test-token"></a>Hae testi tunnus

Pyydä testi suojaustunnuksen suojaustunnuksen rajoitus, jota käytettiin avaimen luvan käytännön perusteella.

    // Deserializes a string containing an Xml representation of a TokenRestrictionTemplate
    // back into a TokenRestrictionTemplate class instance.
    TokenRestrictionTemplate tokenTemplate = 
        TokenRestrictionTemplateSerializer.Deserialize(tokenTemplateString);
    
    // Generate a test token based on the data in the given TokenRestrictionTemplate.
    //The GenerateTestToken method returns the token without the word “Bearer” in front
    //so you have to add it in front of the token string. 
    string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate);
    Console.WriteLine("The authorization token is:\nBearer {0}", testToken);

    
Voit käyttää [AMS Playerin](http://amsplayer.azurewebsites.net/azuremediaplayer.html) Testaa yhteyttä muodossa.

##<a id="example"></a>Esimerkki


Seuraavassa esimerkissä on toimintoja, jotka otettiin Azure Media Services SDK .net-Version 3.5.2 (erityisesti mahdollisuus Widevine käyttöoikeuden mallin määrittäminen ja Widevine käyttöoikeuden pyytämisestä Azure Media Services). Asenna paketti käytettiin Nuget paketti-komennon:

    PM> Install-Package windowsazure.mediaservices -Version 3.5.2


1. Luo uusi Console-projekti.
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
    
    Varmista, että päivitettävä muuttujat osoittamaan kansiot, jossa syötteen tiedostot sijaitsevat.
        
        using System;
        using System.Collections.Generic;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using System.Threading;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using Microsoft.WindowsAzure.MediaServices.Client.ContentKeyAuthorization;
        using Microsoft.WindowsAzure.MediaServices.Client.DynamicEncryption;
        using Microsoft.WindowsAzure.MediaServices.Client.Widevine;
        using Newtonsoft.Json;
        
        namespace DynamicEncryptionWithDRM
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
        
                    IContentKey key = CreateCommonTypeContentKey(encodedAsset);
                    Console.WriteLine("Created key {0} for the asset {1} ", key.Id, encodedAsset.Id);
                    Console.WriteLine("PlayReady License Key delivery URL: {0}", key.GetKeyDeliveryUrl(ContentKeyDeliveryType.PlayReadyLicense));
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
        
                    // You can use the http://amsplayer.azurewebsites.net/azuremediaplayer.html player to test streams.
                    // Note that DASH works on IE 11 (via PlayReady), Edge (via PlayReady), Chrome (via Widevine).
                     
                    string url = GetStreamingOriginLocator(encodedAsset);
                    Console.WriteLine("Encrypted DASH URL: {0}/manifest(format=mpd-time-csf)", url);
        
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
        
        
                static public IContentKey CreateCommonTypeContentKey(IAsset asset)
                {
                    
                    Guid keyId = Guid.NewGuid();
                    byte[] contentKey = GetRandomBuffer(16);
        
                    IContentKey key = _context.ContentKeys.Create(
                                            keyId,
                                            contentKey,
                                            "ContentKey",
                                            ContentKeyType.CommonEncryption);
        
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
        
                    // Configure PlayReady and Widevine license templates.
                    string PlayReadyLicenseTemplate = ConfigurePlayReadyLicenseTemplate();
        
                    string WidevineLicenseTemplate = ConfigureWidevineLicenseTemplate();
        
                    IContentKeyAuthorizationPolicyOption PlayReadyPolicy =
                        _context.ContentKeyAuthorizationPolicyOptions.Create("",
                            ContentKeyDeliveryType.PlayReadyLicense,
                                restrictions, PlayReadyLicenseTemplate);
        
                    IContentKeyAuthorizationPolicyOption WidevinePolicy =
                        _context.ContentKeyAuthorizationPolicyOptions.Create("", 
                            ContentKeyDeliveryType.Widevine, 
                            restrictions, WidevineLicenseTemplate);
        
                    IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                                ContentKeyAuthorizationPolicies.
                                CreateAsync("Deliver Common Content Key with no restrictions").
                                Result;
        
        
                    contentKeyAuthorizationPolicy.Options.Add(PlayReadyPolicy);
                    contentKeyAuthorizationPolicy.Options.Add(WidevinePolicy);
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
        
                    // Configure PlayReady and Widevine license templates.
                    string PlayReadyLicenseTemplate = ConfigurePlayReadyLicenseTemplate();
        
                    string WidevineLicenseTemplate = ConfigureWidevineLicenseTemplate();
        
                    IContentKeyAuthorizationPolicyOption PlayReadyPolicy =
                        _context.ContentKeyAuthorizationPolicyOptions.Create("Token option",
                            ContentKeyDeliveryType.PlayReadyLicense,
                                restrictions, PlayReadyLicenseTemplate);
        
                    IContentKeyAuthorizationPolicyOption WidevinePolicy =
                        _context.ContentKeyAuthorizationPolicyOptions.Create("Token option",
                            ContentKeyDeliveryType.Widevine,
                                restrictions, WidevineLicenseTemplate);
        
                    IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                                ContentKeyAuthorizationPolicies.
                                CreateAsync("Deliver Common Content Key with token restrictions").
                                Result;
        
                    contentKeyAuthorizationPolicy.Options.Add(PlayReadyPolicy);
                    contentKeyAuthorizationPolicy.Options.Add(WidevinePolicy);
        
                    // Associate the content key authorization policy with the content key
                    contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
                    contentKey = contentKey.UpdateAsync().Result;
        
                    return tokenTemplateString;
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
        
                static private string ConfigurePlayReadyLicenseTemplate()
                {
                    // The following code configures PlayReady License Template using .NET classes
                    // and returns the XML string.
        
                    //The PlayReadyLicenseResponseTemplate class represents the template for the response sent back to the end user. 
                    //It contains a field for a custom data string between the license server and the application 
                    //(may be useful for custom app logic) as well as a list of one or more license templates.
                    PlayReadyLicenseResponseTemplate responseTemplate = new PlayReadyLicenseResponseTemplate();
        
                    // The PlayReadyLicenseTemplate class represents a license template for creating PlayReady licenses
                    // to be returned to the end users. 
                    //It contains the data on the content key in the license and any rights or restrictions to be 
                    //enforced by the PlayReady DRM runtime when using the content key.
                    PlayReadyLicenseTemplate licenseTemplate = new PlayReadyLicenseTemplate();
                    //Configure whether the license is persistent (saved in persistent storage on the client) 
                    //or non-persistent (only held in memory while the player is using the license).  
                    licenseTemplate.LicenseType = PlayReadyLicenseType.Nonpersistent;
        
                    // AllowTestDevices controls whether test devices can use the license or not.  
                    // If true, the MinimumSecurityLevel property of the license
                    // is set to 150.  If false (the default), the MinimumSecurityLevel property of the license is set to 2000.
                    licenseTemplate.AllowTestDevices = true;
        
                    // You can also configure the Play Right in the PlayReady license by using the PlayReadyPlayRight class. 
                    // It grants the user the ability to playback the content subject to the zero or more restrictions 
                    // configured in the license and on the PlayRight itself (for playback specific policy). 
                    // Much of the policy on the PlayRight has to do with output restrictions 
                    // which control the types of outputs that the content can be played over and 
                    // any restrictions that must be put in place when using a given output.
                    // For example, if the DigitalVideoOnlyContentRestriction is enabled, 
                    //then the DRM runtime will only allow the video to be displayed over digital outputs 
                    //(analog video outputs won’t be allowed to pass the content).
        
                    //IMPORTANT: These types of restrictions can be very powerful but can also affect the consumer experience. 
                    // If the output protections are configured too restrictive, 
                    // the content might be unplayable on some clients. For more information, see the PlayReady Compliance Rules document.
        
                    // For example:
                    //licenseTemplate.PlayRight.AgcAndColorStripeRestriction = new AgcAndColorStripeRestriction(1);
        
                    responseTemplate.LicenseTemplates.Add(licenseTemplate);
        
                    return MediaServicesLicenseTemplateSerializer.Serialize(responseTemplate);
                }
        
        
                private static string ConfigureWidevineLicenseTemplate()
                {
                    var template = new WidevineMessage
                    {
                        allowed_track_types = AllowedTrackTypes.SD_HD,
                        content_key_specs = new[]
                        {
                            new ContentKeySpecs
                            {
                                required_output_protection = new RequiredOutputProtection { hdcp = Hdcp.HDCP_NONE},
                                security_level = 1,
                                track_type = "SD"
                            }
                        },
                        policy_overrides = new
                        {
                            can_play = true,
                            can_persist = true,
                            can_renew = false
                        }
                    };
        
                    string configuration = JsonConvert.SerializeObject(template);
                    return configuration;
                }
        
                static public void CreateAssetDeliveryPolicy(IAsset asset, IContentKey key)
                {
                    // Get the PlayReady license service URL.
                    Uri acquisitionUrl = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.PlayReadyLicense);
                
                    // GetKeyDeliveryUrl for Widevine attaches the KID to the URL.
                    // For example: https://amsaccount1.keydelivery.mediaservices.windows.net/Widevine/?KID=268a6dcb-18c8-4648-8c95-f46429e4927c.  
                    // The WidevineBaseLicenseAcquisitionUrl (used below) also tells Dynamaic Encryption 
                    // to append /? KID =< keyId > to the end of the url when creating the manifest.
                    // As a result Widevine license acquisition URL will have KID appended twice, 
                    // so we need to remove the KID that in the URL when we call GetKeyDeliveryUrl.
            
                    Uri widevineUrl = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.Widevine);
                    UriBuilder uriBuilder = new UriBuilder(widevineUrl);
                    uriBuilder.Query = String.Empty;
                    widevineUrl = uriBuilder.Uri;
        
                    Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
                        new Dictionary<AssetDeliveryPolicyConfigurationKey, string>
                        {
                            {AssetDeliveryPolicyConfigurationKey.PlayReadyLicenseAcquisitionUrl, acquisitionUrl.ToString()},
                            {AssetDeliveryPolicyConfigurationKey.WidevineBaseLicenseAcquisitionUrl, widevineUrl.ToString()}
        
                        };
        
                    // In this case we only specify Dash streaming protocol in the delivery policy,
                    // All other protocols will be blocked from streaming.
                    var assetDeliveryPolicy = _context.AssetDeliveryPolicies.Create(
                            "AssetDeliveryPolicy",
                        AssetDeliveryPolicyType.DynamicCommonEncryption,
                        AssetDeliveryProtocol.Dash,
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


## <a name="next-step"></a>Seuraava vaihe

Tarkista oppimispolut Media-palveluita.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Palautteen antaminen

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="see-also"></a>Katso myös

[CENC usean DRM ja käyttöoikeuksien hallinta](media-services-cenc-with-multidrm-access-control.md)

[Määritä Widevine pakkaus AMS](http://mingfeiy.com/how-to-configure-widevine-packaging-with-azure-media-services)

[Sähköposteja Google Widevine käyttöoikeuden toimituksen services Azure Media Services-palveluissa](https://azure.microsoft.com/blog/announcing-general-availability-of-google-widevine-license-services/)
