<properties 
    pageTitle="Määritä resurssi toimituksen käytännöt .NET SDK | Microsoft Azure" 
    description="Tässä ohjeaiheessa esitellään eri kohteiden toimituksen käytäntöjen määrittäminen Azure Media Services .NET SDK." 
    services="media-services" 
    documentationCenter="" 
    authors="Mingfeiy" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="09/19/2016"
    ms.author="juliako;mingfeiy"/>

#<a name="configure-asset-delivery-policies-with-net-sdk"></a>.NET SDK ja kohteiden toimituksen käytäntöjen määrittäminen
[AZURE.INCLUDE [media-services-selector-asset-delivery-policy](../../includes/media-services-selector-asset-delivery-policy.md)]

##<a name="overview"></a>Yleiskatsaus

Jos aiot toimituksen salattu kalusto-jokin Media Services sisällön lähettämisen työnkulun vaiheet on määrittäminen resurssien toimituksen käytännöt. Kohteiden toimituksen käytännön kertoo Media Services siitä, miten haluat, että annetaan toimitetaan: kyselyjä mitä streaming protokolla olisi oman resurssi voidaan dynaamisesti pakattu (esimerkiksi MPEG VIIVA, HLS, tasainen Streaming, tai kaikki), onko haluat salata dynaamisesti oman resurssi ja miten (kirjekuori tai yleisten salaus).

Tässä ohjeaiheessa kerrotaan, miksi ja miten voit luoda ja määrittää resurssi toimituksen käytännöt.

>[AZURE.NOTE]On vähintään yksi asteikko (tunnetaan myös nimellä streaming yksikkö) on muista voi käyttää Dynaaminen pakkaus ja dynaaminen salauksen. Lisätietoja on artikkelissa [miten Media-palvelun](media-services-portal-manage-streaming-endpoints.md).
>
>Myös että resurssi on oltava mukautuvat bittitaajuus MP4s tai mukautuvat bittitaajuus tasainen Streaming-tiedostoja.

Voi käyttää eri käytännöt sama kohde. Voit esimerkiksi lisätä PlayReady salaus tasainen Streaming ja AES kirjekuori salaus MPEG VIIVA ja HLS. Protokollat, joka ei ole määritetty toimitus-käytäntö (esimerkiksi lisätä yhden käytännön, joka määrittää vain protokollaksi HLS) estetty streaming. Poikkeus tähän on, jos sinulla ei ole resurssi toimituksen käytäntö on määritetty lainkaan. Valitse sitten Poista sallittu kaikki protokollat.

Jos haluat pitää tallennustilan salattuja resurssi, sinun on määritettävä kohteen toimituksen käytännön. Ennen kuin oman resurssi voidaan lähettää, streaming palvelimen poistaa tallennustilaa salaus ja virtauttaa määritetyn toimitus-käytännön avulla sisältöä. Esimerkiksi pitää yhteyttä resurssi salattu salaus AES (Advanced Standard) kirjekuoren salausavaimen, Määritä **DynamicEnvelopeEncryption**käytännön tyyppi. Tallennustilan salauksen poistaminen ja poista annetaan virtauttaa, Määritä käytäntö tyypiksi **NoDynamicEncryption**. Esimerkkejä, jotka osoittavat, miten voit määrittää käytännön tällaisia noudattamalla.

Sen mukaan, miten kohteiden toimitus-käytännön määrittäminen voi dynaamisesti paketointi, dynaamisesti salaaminen ja virtauttaa seuraavista protokollista: tasainen Streaming, HLS, MPEG VIIVA ja HDS virtaa.

Seuraavassa luettelossa on muotoja, voit lähettää sujuvat, HLS, VIIVAN sekä HDS.

Tasainen Streaming:

{streaming päätepisteen nimi media-palveluiden tilin name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

HLS:

{streaming päätepisteen nimi media-palveluiden tilin name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

MPEG-VIIVA

{streaming päätepisteen nimi media-palveluiden tilin name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)

HDS

{streaming päätepisteen nimi media-palveluiden tilin name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=f4m-f4f)

Ohjeita siitä, miten voit julkaista sijoituksen ja luoda streaming URL-osoite on artikkelissa [luominen streaming URL-osoite](media-services-deliver-streaming-content.md).

##<a name="considerations"></a>Huomioon otettavia seikkoja

- Et voi poistaa AssetDeliveryPolicy liittyvä sijoituksen samalla, kun kyseinen resurssi on olemassa OnDemand (streaming) locator. Käytännön poistaminen kohteen ennen poistamista käytäntö on suositus.
- Streaming locator ei voi luoda tallennustilan salattuja resurssi, kun ei ole resurssi toimituksen käytäntö määritetään.  Jos kohteen ei tallennustilan salattu, järjestelmä avulla voit luoda locator ja virtauttaa Poista resurssi-toimituksen käytäntöä annetaan.
- Sinulla voi olla useita resurssi toimituksen käytäntöjä liittyvät yksittäinen resurssi, mutta voit määrittää vain yksi tapa käsitellä annetun AssetDeliveryProtocol.  Mikä tarkoittaa, jos yrität linkittää kaksi toimituksen käytännöt, jotka määrittävät AssetDeliveryProtocol.SmoothStreaming-protokolla, joka aiheuttaa virheen, koska järjestelmä tiedä haluamasi se käyttää, kun asiakas on tasainen Streaming pyynnön.
- Jos sinulla on aiemmin streaming locator sijoituksen, et voi linkittää uuden käytännön kohteen (Voit joko Poista olemassa olevaa käytäntöä-kohteen, tai päivittää liittyvän kohteen toimitus-käytännön).  Sinun on ensin poistettava streaming locator, säätää käytännöt ja luo streaming locator uudelleen.  Voit käyttää samaa locatorId streaming locator uudelleen, mutta voit varmistaa, joka ei aiheuttaa ongelmia asiakkaille, koska sisältöä voidaan säilyttää välimuistissa alkuperän tai edeltävät CDN.


##<a name="clear-asset-delivery-policy"></a>Poista resurssi toimituksen käytäntö

**ConfigureClearAssetDeliveryPolicy** seuraavasti määrittää koske dynaaminen salaus ja pitää virta jossakin seuraavista protokollista: MPEG VIIVA, HLS ja tasainen Streaming protokollat. Haluat ehkä käyttää tätä käytäntöä tallennustilan salattu varat.

Saat lisätietoja mitä arvoja voit määrittää AssetDeliveryPolicy luotaessa-kohdassa [tyypit käytetään määritettäessä AssetDeliveryPolicy](#types) .

staattinen julkisen void ConfigureClearAssetDeliveryPolicy (IAsset resurssi) {IAssetDeliveryPolicy käytännön = _context. AssetDeliveryPolicies.Create ("Poista käytäntö", AssetDeliveryPolicyType.NoDynamicEncryption, AssetDeliveryProtocol.HLS | AssetDeliveryProtocol.SmoothStreaming | AssetDeliveryProtocol.Dash, null);

resurssi. DeliveryPolicies.Add(policy); }

##<a name="dynamiccommonencryption-asset-delivery-policy"></a>DynamicCommonEncryption resurssi toimituksen käytäntö


Seuraava **CreateAssetDeliveryPolicy** menetelmä luo **AssetDeliveryPolicy** , joka on määritetty käyttämään dynaaminen yleisiä salaus (**DynamicCommonEncryption**) tasainen streaming protocol (muut protokollat estetään-streaming). Menetelmä on kaksi parametria: **resurssi** (resurssi, johon haluat lisätä toimitus-käytäntö) ja **IContentKey** (sisällön avain **CommonEncryption** -tyypin Lisätietoja on artikkelissa: [sisällön avaimen luomista](media-services-dotnet-create-contentkey.md#common_contentkey)).

Saat lisätietoja mitä arvoja voit määrittää AssetDeliveryPolicy luotaessa-kohdassa [tyypit käytetään määritettäessä AssetDeliveryPolicy](#types) .


staattinen julkisen void CreateAssetDeliveryPolicy (IAsset resurssi, IContentKey avain) {Uri acquisitionUrl = avain. GetKeyDeliveryUrl(ContentKeyDeliveryType.PlayReadyLicense);

Sanaston < AssetDeliveryPolicyConfigurationKey, merkkijono > assetDeliveryPolicyConfiguration uusi sanasto < AssetDeliveryPolicyConfigurationKey merkkijono > = {{AssetDeliveryPolicyConfigurationKey.PlayReadyLicenseAcquisitionUrl, acquisitionUrl.ToString()}};

        var assetDeliveryPolicy = _context.AssetDeliveryPolicies.Create(
                "AssetDeliveryPolicy",
            AssetDeliveryPolicyType.DynamicCommonEncryption,
            AssetDeliveryProtocol.SmoothStreaming,
            assetDeliveryPolicyConfiguration);

        // Add AssetDelivery Policy to the asset
        asset.DeliveryPolicies.Add(assetDeliveryPolicy);

        Console.WriteLine();
        Console.WriteLine("Adding Asset Delivery Policy: " +
            assetDeliveryPolicy.AssetDeliveryPolicyType);
    }

Azure Media-palveluiden avulla voit lisätä Widevine salausta. Seuraavassa esimerkissä PlayReady ja Widevine resurssi toimitus-käytäntö lisätään.


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
            {AssetDeliveryPolicyConfigurationKey.WidevineLicenseAcquisitionUrl, widevineUrl.ToString()}

        };

        var assetDeliveryPolicy = _context.AssetDeliveryPolicies.Create(
                "AssetDeliveryPolicy",
            AssetDeliveryPolicyType.DynamicCommonEncryption,
            AssetDeliveryProtocol.Dash,
            assetDeliveryPolicyConfiguration);


        // Add AssetDelivery Policy to the asset
        asset.DeliveryPolicies.Add(assetDeliveryPolicy);

    }

>[AZURE.NOTE]Kun salaamaan Widevine, vain olisi voi pitää käyttämällä VIIVA. Varmista, että voit määrittää VIIVAN resurssi toimitus-protokollaa.


##<a name="dynamicenvelopeencryption-asset-delivery-policy"></a>DynamicEnvelopeEncryption resurssi toimituksen käytäntö 

Seuraava **CreateAssetDeliveryPolicy** menetelmä luo **AssetDeliveryPolicy** , joka on määritetty dynaaminen kirjekuori salaus (**DynamicEnvelopeEncryption**) koskee tasainen Streaming, HLS ja VIIVAN protokollat (Jos päätät Määritä Jotkin protokollat, ne estetään streaming). Menetelmä on kaksi parametria: **resurssi** (resurssi, johon haluat lisätä toimitus-käytäntö) ja **IContentKey** ( **EnvelopeEncryption** tyyppi-sisällön avain Lisätietoja on artikkelissa: [sisällön avaimen luomista](media-services-dotnet-create-contentkey.md#envelope_contentkey)).


Saat lisätietoja mitä arvoja voit määrittää AssetDeliveryPolicy luotaessa-kohdassa [tyypit käytetään määritettäessä AssetDeliveryPolicy](#types) .   

    private static void CreateAssetDeliveryPolicy(IAsset asset, IContentKey key)
    {
        
        //  Get the Key Delivery Base Url by removing the Query parameter.  The Dynamic Encryption service will
        //  automatically add the correct key identifier to the url when it generates the Envelope encrypted content
        //  manifest.  Omitting the IV will also cause the Dynamice Encryption service to generate a deterministic
        //  IV for the content automatically.  By using the EnvelopeBaseKeyAcquisitionUrl and omitting the IV, this
        //  allows the AssetDelivery policy to be reused by more than one asset.
        //
        Uri keyAcquisitionUri = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.BaselineHttp);
        UriBuilder uriBuilder = new UriBuilder(keyAcquisitionUri);
        uriBuilder.Query = String.Empty;
        keyAcquisitionUri = uriBuilder.Uri;

        // The following policy configuration specifies: 
        //   key url that will have KID=<Guid> appended to the envelope and
        //   the Initialization Vector (IV) to use for the envelope encryption.
        Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
            new Dictionary<AssetDeliveryPolicyConfigurationKey, string> 
        {
            {AssetDeliveryPolicyConfigurationKey.EnvelopeBaseKeyAcquisitionUrl, keyAcquisitionUri.ToString()},
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
        Console.WriteLine("Adding Asset Delivery Policy: " + assetDeliveryPolicy.AssetDeliveryPolicyType);
    }


##<a id="types"></a>Käytetään määritettäessä AssetDeliveryPolicy tyypit

###<a id="AssetDeliveryProtocol"></a>AssetDeliveryProtocol 

    /// <summary>
    /// Delivery protocol for an asset delivery policy.
    /// </summary>
    [Flags]
    public enum AssetDeliveryProtocol
    {
        /// <summary>
        /// No protocols.
        /// </summary>
        None = 0x0,

        /// <summary>
        /// Smooth streaming protocol.
        /// </summary>
        SmoothStreaming = 0x1,

        /// <summary>
        /// MPEG Dynamic Adaptive Streaming over HTTP (DASH)
        /// </summary>
        Dash = 0x2,

        /// <summary>
        /// Apple HTTP Live Streaming protocol.
        /// </summary>
        HLS = 0x4,

        /// <summary>
        /// Adobe HTTP Dynamic Streaming (HDS)
        /// </summary>
        Hds = 0x8,

        /// <summary>
        /// Include all protocols.
        /// </summary>
        All = 0xFFFF
    }

###<a id="AssetDeliveryPolicyType"></a>AssetDeliveryPolicyType

    /// <summary>
    /// Policy type for dynamic encryption of assets.
    /// </summary>
    public enum AssetDeliveryPolicyType
    {
        /// <summary>
        /// Delivery Policy Type not set.  An invalid value.
        /// </summary>
        None,

        /// <summary>
        /// The Asset should not be delivered via this AssetDeliveryProtocol. 
        /// </summary>
        Blocked, 

        /// <summary>
        /// Do not apply dynamic encryption to the asset.
        /// </summary>
        /// 
        NoDynamicEncryption,  

        /// <summary>
        /// Apply Dynamic Envelope encryption.
        /// </summary>
        DynamicEnvelopeEncryption,

        /// <summary>
        /// Apply Dynamic Common encryption.
        /// </summary>
        DynamicCommonEncryption
    }

###<a id="ContentKeyDeliveryType"></a>ContentKeyDeliveryType

    /// <summary>
    /// Delivery method of the content key to the client.
    /// </summary>
    public enum ContentKeyDeliveryType
    {
        /// <summary>
        /// None.
        /// </summary>
        None = 0,

        /// <summary>
        /// Use PlayReady License acquistion protocol
        /// </summary>
        PlayReadyLicense = 1,

        /// <summary>
        /// Use MPEG Baseline HTTP key protocol.
        /// </summary>
        BaselineHttp = 2,

        /// <summary>
        /// Use Widevine License acquistion protocol
        ///
        </summary>
        Widevine = 3

    }

###<a id="AssetDeliveryPolicyConfigurationKey"></a>AssetDeliveryPolicyConfigurationKey

    /// <summary>
    /// Keys used to get specific configuration for an asset delivery policy.
    /// </summary>
    public enum AssetDeliveryPolicyConfigurationKey
    {
        /// <summary>
        /// No policies.
        /// </summary>
        None,

        /// <summary>
        /// Exact Envelope key URL.
        /// </summary>
        EnvelopeKeyAcquisitionUrl,

        /// <summary>
        /// Base key url that will have KID=<Guid> appended for Envelope.
        /// </summary>
        EnvelopeBaseKeyAcquisitionUrl,
        
        /// <summary>
        /// The initialization vector to use for envelope encryption in Base64 format.
        /// </summary>
        EnvelopeEncryptionIVAsBase64,

        /// <summary>
        /// The PlayReady License Acquisition Url to use for common encryption.
        /// </summary>
        PlayReadyLicenseAcquisitionUrl,

        /// <summary>
        /// The PlayReady Custom Attributes to add to the PlayReady Content Header
        /// </summary>
        PlayReadyCustomAttributes,

        /// <summary>
        /// The initialization vector to use for envelope encryption.
        /// </summary>
        EnvelopeEncryptionIV,

        /// <summary>
        /// Widevine DRM acquisition url
        /// </summary>
        WidevineLicenseAcquisitionUrl
    }

##<a name="media-services-learning-paths"></a>Media-palveluiden oppimispolut

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Palautteen antaminen

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


