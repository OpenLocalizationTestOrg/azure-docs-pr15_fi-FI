<properties 
    pageTitle="Luo ContentKeys .NET" 
    description="Opettele luomaan sisältö näppäimet, joissa on suojattu käyttö resurssit." 
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
    ms.date="09/26/2016"
    ms.author="juliako"/>


#<a name="create-contentkeys-with-net"></a>Luo ContentKeys .NET

> [AZURE.SELECTOR]
- [MUILLE KÄYTTÄJILLE](media-services-rest-create-contentkey.md)
- [.NET](media-services-dotnet-create-contentkey.md)

Media-palveluiden avulla voit luomisesta ja pitämisestä salattuja resurssit. **ContentKey** on suojattu käyttö **resurssi**s. 

Kun luot uuden resurssi (esimerkiksi ennen kuin voit [ladata tiedostoja](media-services-dotnet-upload-files.md)), voit määrittää seuraavat salausasetukset: **StorageEncrypted**, **CommonEncryptionProtected**tai **EnvelopeEncryptionProtected**. 

Kun pidät varat asiakkaiden, voit tehdä seuraavat kaksi salausta varten mainitun [dynaamisesti salattuja resurssien määrittäminen](media-services-dotnet-configure-asset-delivery-policy.md) : **DynamicEnvelopeEncryption** tai **DynamicCommonEncryption**.

Salatun kalusto on liitettävä **ContentKey**s-näppäinyhdistelmää. Tässä artikkelissa kerrotaan, miten sisällön avaimen luomiseen.

>[AZURE.NOTE] Luotaessa uusia **StorageEncrypted** resurssi, käyttämällä Media Services .NET SDK- **ContentKey** luodaan automaattisesti ja linkittää kohteen.

##<a name="contentkeytype"></a>ContentKeyType

Yksi arvoista, sinun on määritettävä milloin luominen sisältö avain on avaimen sisältötyypin. Valitse jokin seuraavista arvoista. 

    public enum ContentKeyType
    {
        /// <summary>
        /// Specifies a content key for common encryption.
        /// </summary>
        /// <remarks>This is the default value.</remarks>
        CommonEncryption = 0,

        /// <summary>
        /// Specifies a content key for storage encryption.
        /// </summary>
        StorageEncryption = 1,

        /// <summary>
        /// Specifies a content key for configuration encryption.
        /// </summary>
        ConfigurationEncryption = 2,

        /// <summary>
        /// Specifies a content key for Envelope encryption.  Only used internally.
        /// </summary>
        EnvelopeEncryption = 4
    }

##<a id="envelope_contentkey"></a>Luo kirjekuori ContentKey

Seuraavat koodikatkelman Luo sisällön näppäintä kirjekuori salaus tyyppi. Se sitten liittää avain määritetyn kohteen.

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

        asset.ContentKeys.Add(key);

        return key;
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

Kutsu

    IContentKey key = CreateEnvelopeTypeContentKey(encryptedsset);



##<a id="common_contentkey"></a>Luo yleisiä ContentKey    

Seuraavat koodikatkelman Luo sisällön näppäintä yleisiä salaus tyyppi. Se sitten liittää avain määritetyn kohteen.

    static public IContentKey CreateCommonTypeContentKey(IAsset asset)
    {
        // Create common encryption content key
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
Kutsu

    IContentKey key = CreateCommonTypeContentKey(encryptedsset); 


##<a name="media-services-learning-paths"></a>Media-palveluiden oppimispolut

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Palautteen antaminen

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
