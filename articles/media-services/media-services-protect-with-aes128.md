<properties
    pageTitle="AES-128 dynaaminen salaus ja avaimen toimitus-palvelun avulla | Microsoft Azure"
    description="Microsoft Azure Media-palveluiden avulla voit pitää salattu AES 128 bittistä salausavaimet sisältöä. Media-palvelut on myös avaimen toimitus-palvelun, joka toimittaa salausavaimet valtuutettujen käyttäjien. Tässä ohjeaiheessa esitellään dynaamisesti salaa AES-128 ja avaimen toimitus-palvelun käytöstä."
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
    ms.date="10/24/2016"
    ms.author="juliako"/>

#<a name="using-aes-128-dynamic-encryption-and-key-delivery-service"></a>AES-128 dynaaminen salaus ja avaimen toimitus-palvelun avulla

> [AZURE.SELECTOR]
- [.NET](media-services-protect-with-aes128.md)
- [Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
- [PHP](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)

##<a name="overview"></a>Yleiskatsaus

>[AZURE.NOTE] Katso [tämän](https://channel9.msdn.com/Shows/Azure-Friday/Azure-Media-Services-Protecting-your-Media-Content-with-AES-Encryption) videon opit suojaaminen että Media sisällöstä AES-salausta.

Microsoft Azure Media-palveluiden avulla voit pitää Http-Live-Streaming (HLS) ja tasainen virtaa salattu ja salauksen AES (Advanced Standard) (joko 128 bittistä salausavaimet). Media-palvelut on myös avaimen toimitus-palvelun, joka toimittaa salausavaimet valtuutettujen käyttäjien. Jos haluat salata sijoituksen Media-palveluiden, joudut salausavain liittäminen kohteen ja määritettävä luvan käytännöt näppäimen. Kun stream pyytämän pelaaja-Media Services käyttää määritetyn avaimen salaamiseen dynaamisesti sisältösi käyttää AES-salausta. Purkaa virta player pyytää avain avaimen toimitus-palvelusta. Päättää, onko käyttäjällä oikeuksia hankkia avain-palvelun arvioi luvan käytäntöjä, jonka olet määrittänyt näppäimen.

Media-palveluiden tukee useita tapoja käyttäjät, jotka avaimen pyytää todennustapa. Sisällön avaimen todennus-käytäntö voi olla vähintään yksi käyttöoikeuksien rajoitukset: Avaa tai tunnussanoma rajoitus. Suojaustunnuksen rajoitettu käytäntö on liitettävä mukaan suojattu suojaustunnuksen Service (STS) antanut tunnusta. Media-palveluiden tukee tunnusten [Yksinkertaisen Web tunnusten](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2) (SWT)-muodossa ja [JSON Web suojaustunnuksen](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_3) (JWT)-muodossa. Lisätietoja on artikkelissa [sisällön avain luvan käytännön määrittäminen](media-services-protect-with-aes128.md#configure_key_auth_policy).

Haluat hyödyntää dynaaminen salaus on resurssi, joka sisältää usean bittitaajuus MP4 tiedostojen tai usean bittitaajuus tasainen Streaming lähdetiedostot. Haluat myös, (kuvattu tämän artikkelin) annetaan toimitus-käytännön määrittäminen. Sitten määritetyssä streaming URL-osoitteen mukaan edelleen tarvittaessa Streaming palvelimen varmistat, että olet valinnut protokolla toimitetaan virta. Tuloksena tarvitset vain tallentamiseen ja maksaa yhden Tallennusmuoto tiedostot ja Media-palvelut luominen ja yhteyshenkilönä oikea vastaus asiakaskoneesta pyyntöjen perusteella.

Tässä ohjeaiheessa olla hyötyä kehittäjät, jotka työskentelevät sovellukset, jotka toimittavat suojattujen mediatiedostojen. Aiheen näytetään, miten tärkeimmät toimitus-palvelun määrittäminen luvan käytäntöjen kanssa niin, että vain valtuutettujen asiakkaiden saada salausavaimet. Se näyttää myös käyttämisestä dynaaminen salausta.

>[AZURE.NOTE]Aloita käyttäminen dynaamisen salausta, sinun täytyy saada vähintään yksi asteikko (tunnetaan myös nimellä streaming yksikkö). Lisätietoja on artikkelissa [miten Media-palvelun](media-services-portal-manage-streaming-endpoints.md).

##<a name="aes-128-dynamic-encryption-and-key-delivery-service-workflow"></a>AES-128 dynaaminen salaus ja avaimen toimituksen palvelun työnkulku

Seuraavassa on Yleiset vaiheet, sinun on suoritettava salaamiseen oman resurssien AES käyttämällä Media Services avaimen palvelua ja käyttää myös dynaaminen salausta.

1. [Luo sijoituksen ja lataa tiedostot kohteen](media-services-protect-with-aes128.md#create_asset).
1. [Käytä kohteen sisältävän tiedoston mukautuvat bittitaajuus MP4 määrittäminen](media-services-protect-with-aes128.md#encode_asset).
1. [Luo sisällön avain ja liittää sen koodattu kohteiden kanssa](media-services-protect-with-aes128.md#create_contentkey). Media-palveluiden sisällön avain sisältää kohteen salausavaimen.
1. [Sisällön avain luvan käytännön määrittäminen](media-services-protect-with-aes128.md#configure_key_auth_policy). Sisällön avaimen todennus-käytäntö on voit määrittää ja täyttää asiakkaan järjestyksessä sisällön näppäimen toimitetaan asiakkaalle.
1. [Määritä amerikkalaisen toimituksen käytäntöä](media-services-protect-with-aes128.md#configure_asset_delivery_policy). Toimituksen käytännön määritys sisältyy: hankintaa URL-osoite ja alustus Vector (IV) (AES 128 vaatii saman IV toimitettavien, kun ja salauksen) toimitus-protokolla (kuten MPEG VIIVA, HLS, HDS, tasainen Streaming tai kaikki), dynaaminen salaus (esimerkiksi kirjekuori tai dynaaminen salausta) tyypin.

Eri voitu koske kutakin saman resurssi-protokollaa. Voit esimerkiksi lisätä PlayReady salaus tasainen/VIIVA ja AES kirjekuorta HLS. Protokollat, joka ei ole määritetty toimitus-käytäntö (esimerkiksi lisätä yhden käytännön, joka määrittää vain protokollaksi HLS) estetty streaming. Poikkeus tähän on, jos sinulla ei ole resurssi toimituksen käytäntö on määritetty lainkaan. Valitse sitten Poista sallittu kaikki protokollat.

1. [Luo OnDemand locator](media-services-protect-with-aes128.md#create_locator) jotta saat streaming URL-osoite.

Aihe näkyy myös [siitä, miten asiakassovellus pyytää näppäintä avaimen toimitus-palvelusta](media-services-protect-with-aes128.md#client_request).

Etsii valmis .NET [Esimerkki](media-services-protect-with-aes128.md#example) ohjeaiheen lopussa.

Seuraavassa kuvassa näytetään edellä kuvatun työnkulun. Tähän tunnuksen käytetään todennusta varten.

![AES-128 suojaaminen](./media/media-services-content-protection-overview/media-services-content-protection-with-aes.png)

Tässä ohjeaiheessa muiden antaa etsimistä, koodiesimerkkejä ja linkkejä ohjeaiheisiin, kuinka saavuttamiseksi kuvattuun tehtävät.

##<a name="current-limitations"></a>Nykyisen rajoitukset

Jos lisäät tai päivittää oman resurssi toimituksen käytäntö, poista aiemmin locator (jos saatavilla) ja luo uusi locator.

##<a id="create_asset"></a>Luo sijoituksen ja ladata tiedostoja kohteen esittely

Hallitse koodata ja virtauttaa videoita, jotta lataa ensin sisällön tuominen Microsoft Azure Media Services. Kun ladattu palvelimeen, sisällön tallennetaan Pilvipalvelun lisäkäsittelyä ja streaming suojatusti. 

Lisätietoja on kohdassa [Lataa tiedostoja Media Services-tilille](media-services-dotnet-upload-files.md).

##<a id="encode_asset"></a>Koodata MP4 määrittäminen mukautuvat bittitaajuus tiedoston sisältävä resurssi

Dynaaminen salauksella tarvitset on luotava resurssi, joka sisältää usean bittitaajuus MP4 tiedostojen tai usean bittitaajuus tasainen Streaming lähdetiedostot. Sitten määritetyssä muodossa, luettelo tai fragmentti pyynnön edelleen tarvittaessa Streaming perusteella palvelimen varmistat, että näyttöön tulee virta valitsemasi protokolla. Tuloksena tarvitset vain tallentamiseen ja maksaa yhden Tallennusmuoto tiedostot ja Media-palvelut luominen ja yhteyshenkilönä oikea vastaus asiakaskoneesta pyyntöjen perusteella. Lisätietoja on aiheessa [Dynaaminen pakkaus yleiskatsaus](media-services-dynamic-packaging-overview.md) .

Ohjeita siitä, miten voit salata, katso, [miten voit salata käyttämällä Media Encoder vakio resurssi](media-services-dotnet-encode-with-media-encoder-standard.md).

##<a id="create_contentkey"></a>Luo sisällön avain ja liittää sen koodattu resurssi

Media-palveluiden sisällön avain sisältää avaimen, jonka haluat salata omaisuuden kanssa.

Lisätietoja on Katso [Luo sisällön avain](media-services-dotnet-create-contentkey.md).

##<a id="configure_key_auth_policy"></a>Sisällön avain luvan käytännön määrittäminen

Media-palveluiden tukee useita tapoja käyttäjät, jotka avaimen pyytää todennustapa. Sisällön avaimen todennus-käytäntö on määritetty sinä ja täyttää (player)-asiakasohjelman järjestyksessä toimitetaan asiakas-näppäimen. Sisällön avaimen todennus-käytäntö voi olla yksi tai useampi käyttöoikeuksien rajoitukset: Avaa, tunnussanoma rajoitus tai IP-rajoituksia.

Lisätietoja on artikkelissa [Sisällön avain luvan käytännön määrittäminen](media-services-dotnet-configure-content-key-auth-policy.md).

##<a id="configure_asset_delivery_policy"></a>Kohteiden toimituksen käytännön määrittäminen 

Määritä, että annetaan toimitus-käytäntö. Joitakin toimintoja, jotka resurssi toimituksen käytännön määritys sisältyy:

- Avaimen hankintaa URL-osoite. 
- Alustus Vector (IV) käyttämään kirjekuori-salausta. AES 128 on oltava sama IV toimitettavien, kun ja salauksen. 
- Kohteiden toimituksen protokolla (esimerkiksi MPEG VIIVA, HLS, HDS, tasainen Streaming tai kaikki).
- Dynaaminen salaus (esimerkiksi AES kirjekuori) tyypin tai dynaaminen salausta. 

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

##<a id="client_request"></a>Miten asiakkaan pyytää näppäintä avaimen toimitus-palvelusta?

Edellisessä vaiheessa että URL-Osoitetta, joka osoittaa tiedostojen tiedostoon. Asiakkaan on tarvittavat tiedot poimiminen streaming luettelon tiedostot Siirrä pyyntö avaimen toimitus-palveluun.

###<a name="manifest-files"></a>Luettelo tiedostoista

Asiakkaan täytyy purkaa URL-osoite (joka sisältää myös sisällön avaimen tunnus (lapset)) arvo luettelon tiedostosta. Asiakkaan yrittää sitten Hae salausavaimen avaimen toimitus-palvelusta. Asiakas on myös Pura IV arvo ja käytä sitä salauksen virta. Seuraavassa koodikatkelman <Protection> osan tasainen Streaming-luettelo.

    <Protection>
      <ProtectionHeader SystemID="B47B251A-2409-4B42-958E-08DBAE7B4EE9">
        <ContentProtection xmlns:sea="urn:mpeg:dash:schema:sea:2012" schemeIdUri="urn:mpeg:dash:sea:2012">
          <sea:SegmentEncryption schemeIdUri="urn:mpeg:dash:sea:aes128-cbc:2013"/>
          <sea:KeySystem keySystemUri="urn:mpeg:dash:sea:keysys:http:2013"/>
          <sea:CryptoPeriod IV="0xD7D7D7D7D7D7D7D7D7D7D7D7D7D7D7D7" 
                            keyUriTemplate="https://wamsbayclus001kd-hs.cloudapp.net/HlsHandler.ashx?
                                            kid=da3813af-55e6-48e7-aa9f-a4d6031f7b4d"/>
        </ContentProtection>
      </ProtectionHeader>
    </Protection>

Kyseessä HLS pääkansio-luettelo on jaettu segmentin tiedostoihin. 

Pääkansio-luettelo on esimerkiksi: http://test001.origin.mediaservices.windows.net/8bfe7d6f-34e3-4d1a-b289-3e48a8762490/BigBuckBunny.ism/manifest(format=m3u8-aapl) ja se on segmentin tiedostonimet luettelo.
    
    . . . 
    #EXT-X-STREAM-INF:PROGRAM-ID=1,BANDWIDTH=630133,RESOLUTION=424x240,CODECS="avc1.4d4015,mp4a.40.2",AUDIO="audio"
    QualityLevels(514369)/Manifest(video,format=m3u8-aapl)
    #EXT-X-STREAM-INF:PROGRAM-ID=1,BANDWIDTH=965441,RESOLUTION=636x356,CODECS="avc1.4d401e,mp4a.40.2",AUDIO="audio"
    QualityLevels(842459)/Manifest(video,format=m3u8-aapl)
    …

Jos jokin segmentin Avaa tekstieditorissa (esimerkiksi http://test001.origin.mediaservices.windows.net/8bfe7d6f-34e3-4d1a-b289-3e48a8762490/BigBuckBunny.ism/QualityLevels (514369)/Manifest(video,format=m3u8-aapl), se pitäisi näkyä #EXT X-näppäintä, joka ilmaisee, että tiedosto on salattu.
    
    #EXTM3U
    #EXT-X-VERSION:4
    #EXT-X-ALLOW-CACHE:NO
    #EXT-X-MEDIA-SEQUENCE:0
    #EXT-X-TARGETDURATION:9
    #EXT-X-KEY:METHOD=AES-128,
    URI="https://wamsbayclus001kd-hs.cloudapp.net/HlsHandler.ashx?
         kid=da3813af-55e6-48e7-aa9f-a4d6031f7b4d",
            IV=0XD7D7D7D7D7D7D7D7D7D7D7D7D7D7D7D7
    #EXT-X-PROGRAM-DATE-TIME:1970-01-01T00:00:00.000+00:00
    #EXTINF:8.425708,no-desc
    Fragments(video=0,format=m3u8-aapl)
    #EXT-X-ENDLIST
    
###<a name="request-the-key-from-the-key-delivery-service"></a>Pyydä avain avaimen toimitus-palvelusta

Seuraava koodi kerrotaan, miten voit lähettää pyynnön Media Services avaimen toimitus-palveluun käyttämällä avaimen toimituksen Uri (joka on poimittujen luettelo) ja tunnus (Tässä ohjeaiheessa ei keskustella yksinkertaisen Web tunnusten hakemisesta suojatun suojaustunnuksen Service).

    private byte[] GetDeliveryKey(Uri keyDeliveryUri, string token)
    {
        HttpWebRequest request = (HttpWebRequest)WebRequest.Create(keyDeliveryUri);
                
        request.Method = "POST";
        request.ContentType = "text/xml";
        if (!string.IsNullOrEmpty(token))
        {
            request.Headers[AuthorizationHeader] = token;
        }
        request.ContentLength = 0;
    
        var response = request.GetResponse();
     
        var stream = response.GetResponseStream();
        if (stream == null)
        {
            throw new NullReferenceException("Response stream is null");
        }
    
        var buffer = new byte[256];
        var length = 0;
        while (stream.CanRead && length <= buffer.Length)
        {
            var nexByte = stream.ReadByte();
            if (nexByte == -1)
            {
                break;
            }
            buffer[length] = (byte)nexByte;
            length++;
        }
        response.Close();
    
        // AES keys must be exactly 16 bytes (128 bits).
        var key = new byte[length];
        Array.Copy(buffer, key, length);
        return key;
    }
    
##<a id="example"></a>Esimerkki

1. Luo uusi Console-projekti.
1. NuGet avulla voit asentaa ja lisätä Azure Media Services .NET SDK-laajennusten. Asennuksessa, asentaa Media Services .NET SDK myös ja Lisää kaikki tarvittavat riippuvuudet.
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

1. Korvaa tämän osan esitetyn koodin koodin Program.cs-tiedostossa.
    
    Varmista, että päivitettävä muuttujat osoittamaan kansiot, jossa syötteen tiedostot sijaitsevat.
            
        
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
        
        namespace AESDynamicEncryptionAndKeyDeliverySvc
        {
            class Program
            {
                // Read values from the App.config file.
                private static readonly string _mediaServicesAccountName =
                    ConfigurationManager.AppSettings["MediaServicesAccountName"];
                private static readonly string _mediaServicesAccountKey =
                    ConfigurationManager.AppSettings["MediaServicesAccountKey"];
        
                // A Uri describing the issuer of the token.  
                // Must match the value in the token for the token to be considered valid.
                private static readonly Uri _sampleIssuer =
                    new Uri(ConfigurationManager.AppSettings["Issuer"]);
                // The Audience or Scope of the token.  
                // Must match the value in the token for the token to be considered valid.
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
                    // Used the chached credentials to create CloudMediaContext.
                    _context = new CloudMediaContext(_cachedCredentials);
        
                    bool tokenRestriction = false;
                    string tokenTemplateString = null;
        
                    IAsset asset = UploadFileAndCreateAsset(_singleMP4File);
                    Console.WriteLine("Uploaded asset: {0}", asset.Id);
        
                    IAsset encodedAsset = EncodeToAdaptiveBitrateMP4Set(asset);
                    Console.WriteLine("Encoded asset: {0}", encodedAsset.Id);
        
                    IContentKey key = CreateEnvelopeTypeContentKey(encodedAsset);
                    Console.WriteLine("Created key {0} for the asset {1} ", key.Id, encodedAsset.Id);
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
        
                        // Generate a test token based on the data in the given TokenRestrictionTemplate.
                        // Note, you need to pass the key id Guid because we specified 
                        // TokenClaim.ContentKeyIdentifierClaim in during the creation of TokenRestrictionTemplate.
                        Guid rawkey = EncryptionUtils.GetKeyIdAsGuid(key.Id);
        
                        //The GenerateTestToken method returns the token without the word “Bearer” in front
                        //so you have to add it in front of the token string. 
                        string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate, null, rawkey);
                        Console.WriteLine("The authorization token is:\nBearer {0}", testToken);
                        Console.WriteLine();
                    }
        
                    // You can use the bit.ly/aesplayer Flash player to test the URL 
                    // (with open authorization policy). 
                    // Paste the URL and click the Update button to play the video. 
                    //
                    string URL = GetStreamingOriginLocator(encodedAsset);
                    Console.WriteLine("Smooth Streaming Url: {0}/manifest", URL);
                    Console.WriteLine();
                    Console.WriteLine("HLS Url: {0}/manifest(format=m3u8-aapl)", URL);
                    Console.WriteLine();
        
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
                    IAsset inputAsset = _context.Assets.Create(assetName, AssetCreationOptions.StorageEncrypted);
        
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
        
                static public IAsset EncodeToAdaptiveBitrateMP4Set(IAsset asset)
                {
                    // Declare a new job.
                    IJob job = _context.Jobs.Create("Media Encoder Standard Job");
                    // Get a media processor reference, and pass to it the name of the 
                    // processor to use for the specific task.
                    IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");
        
                    // Create a task with the encoding details, using a string preset.
                    // In this case "H264 Multiple Bitrate 720p" preset is used.
                    ITask task = job.Tasks.AddNew("My encoding task",
                        processor,
                        "H264 Multiple Bitrate 720p",
                        TaskOptions.None);
        
                    // Specify the input asset to be encoded.
                    task.InputAssets.Add(asset);
                    // Add an output asset to contain the results of the job. 
                    // This output is specified as AssetCreationOptions.None, which 
                    // means the output asset is not encrypted. 
                    task.OutputAssets.AddNew("Output asset",
                        AssetCreationOptions.StorageEncrypted);
        
                    job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
                    job.Submit();
                    job.GetExecutionProgressTask(CancellationToken.None).Wait();
        
                    return job.OutputMediaAssets[0];
                }
        
                private static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
                {
                    var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
                    ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();
        
                    if (processor == null)
                        throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));
        
                    return processor;
                }
        
                static public IContentKey CreateEnvelopeTypeContentKey(IAsset asset)
                {
                    // Create envelope encryption content key
                    Guid keyId = Guid.NewGuid();
                    byte[] contentKey = GetRandomBuffer(16);
        
                    IContentKey key = _context.ContentKeys.Create(
                                            keyId,
                                            contentKey,
                                            "ContentKey",
                                            ContentKeyType.EnvelopeEncryption);
        
                    // Associate the key with the asset.
                    asset.ContentKeys.Add(key);
        
                    return key;
                }
        
                static public void AddOpenAuthorizationPolicy(IContentKey contentKey)
                {
                    // Create ContentKeyAuthorizationPolicy with Open restrictions 
                    // and create authorization policy             
                    IContentKeyAuthorizationPolicy policy = _context.
                                            ContentKeyAuthorizationPolicies.
                                            CreateAsync("Open Authorization Policy").Result;
        
                    List<ContentKeyAuthorizationPolicyRestriction> restrictions =
                        new List<ContentKeyAuthorizationPolicyRestriction>();
        
                    ContentKeyAuthorizationPolicyRestriction restriction =
                        new ContentKeyAuthorizationPolicyRestriction
                        {
                            Name = "HLS Open Authorization Policy",
                            KeyRestrictionType = (int)ContentKeyRestrictionType.Open,
                            Requirements = null // no requirements needed for HLS
                                };
        
                    restrictions.Add(restriction);
        
                    IContentKeyAuthorizationPolicyOption policyOption =
                        _context.ContentKeyAuthorizationPolicyOptions.Create(
                        "policy",
                        ContentKeyDeliveryType.BaselineHttp,
                        restrictions,
                        "");
        
                    policy.Options.Add(policyOption);
        
                    // Add ContentKeyAutorizationPolicy to ContentKey
                    contentKey.AuthorizationPolicyId = policy.Id;
                    IContentKey updatedKey = contentKey.UpdateAsync().Result;
                    Console.WriteLine("Adding Key to Asset: Key ID is " + updatedKey.Id);
                }
        
                public static string AddTokenRestrictedAuthorizationPolicy(IContentKey contentKey)
                {
                    string tokenTemplateString = GenerateTokenRequirements();
        
                    IContentKeyAuthorizationPolicy policy = _context.
                                            ContentKeyAuthorizationPolicies.
                                            CreateAsync("HLS token restricted authorization policy").Result;
        
                    List<ContentKeyAuthorizationPolicyRestriction> restrictions =
                            new List<ContentKeyAuthorizationPolicyRestriction>();
        
                    ContentKeyAuthorizationPolicyRestriction restriction =
                            new ContentKeyAuthorizationPolicyRestriction
                            {
                                Name = "Token Authorization Policy",
                                KeyRestrictionType = (int)ContentKeyRestrictionType.TokenRestricted,
                                Requirements = tokenTemplateString
                            };
        
                    restrictions.Add(restriction);
        
                    //You could have multiple options 
                    IContentKeyAuthorizationPolicyOption policyOption =
                        _context.ContentKeyAuthorizationPolicyOptions.Create(
                            "Token option for HLS",
                            ContentKeyDeliveryType.BaselineHttp,
                            restrictions,
                            null  // no key delivery data is needed for HLS
                            );
        
                    policy.Options.Add(policyOption);
        
                    // Add ContentKeyAutorizationPolicy to ContentKey
                    contentKey.AuthorizationPolicyId = policy.Id;
                    IContentKey updatedKey = contentKey.UpdateAsync().Result;
                    Console.WriteLine("Adding Key to Asset: Key ID is " + updatedKey.Id);
        
                    return tokenTemplateString;
                }
        
                static public void CreateAssetDeliveryPolicy(IAsset asset, IContentKey key)
                {
                    Uri keyAcquisitionUri = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.BaselineHttp);
        
                    string envelopeEncryptionIV = Convert.ToBase64String(GetRandomBuffer(16));
            
                    // When configuring delivery policy, you can choose to associate it
                    // with a key acquisition URL that has a KID appended or
                    // or a key acquisition URL that does not have a KID appended  
                    // in which case a content key can be reused. 

                    // EnvelopeKeyAcquisitionUrl:  contains a key ID in the key URL.
                    // EnvelopeBaseKeyAcquisitionUrl:  the URL does not contains a key ID

                    // The following policy configuration specifies: 
                    // key url that will have KID=<Guid> appended to the envelope and
                    // the Initialization Vector (IV) to use for the envelope encryption.
                    
                    Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
                        new Dictionary<AssetDeliveryPolicyConfigurationKey, string>
                    {
                                {AssetDeliveryPolicyConfigurationKey.EnvelopeKeyAcquisitionUrl, keyAcquisitionUri.ToString()}
                    };
        
                    IAssetDeliveryPolicy assetDeliveryPolicy =
                        _context.AssetDeliveryPolicies.Create(
                                    "AssetDeliveryPolicy",
                                    AssetDeliveryPolicyType.DynamicEnvelopeEncryption,
                                    AssetDeliveryProtocol.SmoothStreaming | AssetDeliveryProtocol.HLS | AssetDeliveryProtocol.Dash,
                                    assetDeliveryPolicyConfiguration);
        
                    // Add AssetDelivery Policy to the asset
                    asset.DeliveryPolicies.Add(assetDeliveryPolicy);
                    Console.WriteLine();
                    Console.WriteLine("Adding Asset Delivery Policy: " +
                        assetDeliveryPolicy.AssetDeliveryPolicyType);
                }
        
                static public string GetStreamingOriginLocator(IAsset asset)
                {
        
                    // Get a reference to the streaming manifest file from the  
                    // collection of files in the asset. 
        
                    var assetFile = asset.AssetFiles.Where(f => f.Name.ToLower().
                                                EndsWith(".ism")).
                                                FirstOrDefault();
        
                    // Create a 30-day readonly access policy. 
                    // You cannot create a streaming locator using an AccessPolicy that includes write or delete permissions.            
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
        
                static private void JobStateChanged(object sender, JobStateChangedEventArgs e)
                {
                    Console.WriteLine(string.Format("{0}\n  State: {1}\n  Time: {2}\n\n",
                        ((IJob)sender).Name,
                        e.CurrentState,
                        DateTime.UtcNow.ToString(@"yyyy_M_d__hh_mm_ss")));
                }
        
                static private byte[] GetRandomBuffer(int size)
                {
                    byte[] randomBytes = new byte[size];
                    using (RNGCryptoServiceProvider rng = new RNGCryptoServiceProvider())
                    {
                        rng.GetBytes(randomBytes);
                    }
        
                    return randomBytes;
                }
            }
        }


##<a name="media-services-learning-paths"></a>Media-palveluiden oppimispolut

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Palautteen antaminen

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
