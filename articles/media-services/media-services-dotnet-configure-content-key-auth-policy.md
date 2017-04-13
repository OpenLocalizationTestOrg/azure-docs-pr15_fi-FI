<properties 
    pageTitle="Sisällön avaimen luvan käytännön Media Services .NET SDK: N avulla | Microsoft Azure" 
    description="Opettele määrittämään todennus-käytäntö sisältö avaimen avulla Media Services .NET SDK." 
    services="media-services" 
    documentationCenter="" 
    authors="Mingfeiy" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/15/2016"
    ms.author="juliako;mingfeiy"/>



# <a name="dynamic-encryption-configure-content-key-authorization-policy"></a>Dynaaminen salaus: sisällön avaimen luvan käytännön määrittäminen

[AZURE.INCLUDE [media-services-selector-content-key-auth-policy](../../includes/media-services-selector-content-key-auth-policy.md)]

##<a name="overview"></a>Yleiskatsaus

Microsoft Azure Media-palveluiden avulla voit pitää MPEG-VIIVA, tasainen Streaming ja HTTP-Live-virtautetun median (HLS) virtaa salaus AES (Advanced Standard) (joko 128 bittistä salausavaimet) tai [Microsoft PlayReady DRM](https://www.microsoft.com/playready/overview/)suojattu. AMS avulla voit pitää VIIVA virtaa Widevine DRM salattu. Yleisiä salaus (ISO/IEC 23001 7 CENC) määrityksen kohden salattuja PlayReady ja Widevine.

Media-palveluiden sisältää myös **Avaimen/käyttöoikeuden toimituksen palvelun** josta asiakkaat voivat hankkia AES avaimet tai PlayReady/Widevine käyttöoikeuksia, jotta voit toistaa salattuja sisältöä.

Jos haluat salata sijoituksen Media-palveluiden, joudut salausavain (**CommonEncryption** tai **EnvelopeEncryption**) liittäminen resurssi (kuten kuvattu [seuraavassa](media-services-dotnet-create-contentkey.md)) ja määritettävä luvan käytännöt avaimen (Tässä artikkelissa kuvattua).

Kun stream pyytämän pelaaja-Media Services käyttää määritetyn avaimen salaamiseen dynaamisesti AES tai DRM salausta käyttämällä sisältöä. Purkaa virta player pyytää avain avaimen toimitus-palvelusta. Päättävän riippumatta siitä, onko käyttäjällä on oikeus hankkia avain palvelun arvioi luvan käytäntöjä, jonka olet määrittänyt näppäimen.

Media-palveluiden tukee useita tapoja käyttäjät, jotka avaimen pyytää todennustapa. Sisällön avaimen todennus-käytäntö voi olla yksi tai useampi käyttöoikeuksien rajoitukset: **Avaa** tai **Suojaustunnuksen** rajoitus. Suojaustunnuksen rajoitettu käytäntö on liitettävä mukaan suojattu suojaustunnuksen Service (STS) antanut tunnusta. Media-palveluiden tukee tunnusten **Yksinkertaisen Web tunnusten** ([SWT](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2))-muodossa ja **JSON Web suojaustunnuksen** ([JWT](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_3))-muodossa.

Media-palveluiden ei tarjoa suojatun suojaustunnuksen palvelut. Voit luoda mukautetun STS tai hyödyntää Microsoft Azure ACS ongelman tunnukset. Luoda määritetyn avaimen ja ongelman saatavat, jonka olet määrittänyt suojaustunnuksen rajoituksen määrittäminen (katso tässä artikkelissa kuvattuja) on allekirjoitettu tunnuksen on oltava määritettynä STS. Avaimen toimituksen Media palvelut palauttaa salausavaimen asiakkaalle, jos tunnus on voimassa ja tunnuksen saatavat vastaavat määritetty sisällön avain.

Lisätietoja on artikkelissa

[JWT suojaustunnuksen authenitcation](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/)

[Integroi Azure Media Services OWIN MVC perusteella app Azure Active Directory-hakemistosta ja rajoittaa sisällön avaimen toimittamisen JWT saatavat perusteella](http://www.gtrifonov.com/2015/01/24/mvc-owin-azure-media-services-ad-integration/).

[Käytä Azure ACS ongelman tunnukset](http://mingfeiy.com/acs-with-key-services).

###<a name="some-considerations-apply"></a>Käytä seikkoja:

- Ainakin yksi streaming varattu yksikkö on muista voi käyttää Dynaaminen pakkaus ja dynaaminen salauksen. Lisätietoja on artikkelissa [miten Media-palvelun](media-services-portal-manage-streaming-endpoints.md).
- Oman resurssi on oltava mukautuvat bittitaajuus MP4s tai mukautuvat bittitaajuus tasainen Streaming-tiedostoja. Lisätietoja on artikkelissa [Käytä sijoituksen](media-services-encode-asset.md).
- Lataa ja koodata oman varat **AssetCreationOptions.StorageEncrypted** -vaihtoehdon.
- Jos aiot käyttää useita sisältö näppäimiä, jotka tarvitsevat samaan käytännön määritys, on erittäin suositeltavaa yksittäisen todennus-käytännön luominen ja käyttää sitä uudelleen useita näppäimiä sisältö.
- Avaimen toimitus-palvelun välimuistiin 15 minuutin ContentKeyAuthorizationPolicy ja liittyvät objektit (käytäntöasetukset ja rajoituksia).  Jos luot ContentKeyAuthorizationPolicy ja määritä käyttämään "Tunnus" rajoitus, Testaa, ja Päivitä käytäntö "Avaa" rajoitus, kestää noin 15 minuuttia ennen kuin käytäntöä siirtyy käytännön "Avaa" versio.
- Jos lisäät tai päivittää oman resurssi toimituksen käytäntö, poista aiemmin locator (jos saatavilla) ja luo uusi locator.
- Tällä hetkellä voi salata streaming format HDS tai progressiivinen lataaminen.


##<a name="aes-128-dynamic-encryption"></a>AES-128 dynaaminen salaus

###<a name="open-restriction"></a>Avaa rajoitus

Avaa rajoituksen tarkoittaa, että järjestelmä toimittaa avain kaikkien sellaisten käyttäjien, niiden tekijöistä avaimen pyynnön. Tämä rajoitus voi olla hyötyä testausta varten.

Seuraava esimerkki luo Avaa todennus-käytäntö ja lisää sen sisällön avaimen.

staattinen julkisen void AddOpenAuthorizationPolicy (IContentKey contentKey) {/ / luominen ContentKeyAuthorizationPolicy Avaa rajoitetusti / / ja luvan käytännön IContentKeyAuthorizationPolicy käytännön = _context.
ContentKeyAuthorizationPolicies.
CreateAsync ("Avaa luvan käytäntö"). Tulos;

Luettelon<ContentKeyAuthorizationPolicyRestriction> rajoitukset = uusi luettelo<ContentKeyAuthorizationPolicyRestriction>();
    
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


###<a name="token-restriction"></a>Suojaustunnuksen rajoitus

Tässä osassa kuvataan, miten sisällön avaimen todennus-käytännön luominen ja liittää sen sisältöä avain. Todennus-käytäntö kuvataan, mitä luvan vaatimusten on täytyttävä määrittääksesi, jos käyttäjä on oikeus vastaanottaa key (esimerkiksi käytettäessä "vahvistus avain"-luettelo sisältää avain, joka on allekirjoitettu tunnus).

Jos haluat suojaustunnuksen rajoitus-asetusten määrittäminen käyttää XML tunnuksen luvan vaatimukset kuvaamiseen. Suojaustunnuksen rajoituksen määritysten XML on oltava XML-rakenne.

####<a id="schema"></a>Suojaustunnuksen rajoituksen rakenne
    
    <?xml version="1.0" encoding="utf-8"?>
    <xs:schema xmlns:tns="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/TokenRestrictionTemplate/v1" elementFormDefault="qualified" targetNamespace="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/TokenRestrictionTemplate/v1" xmlns:xs="http://www.w3.org/2001/XMLSchema">
      <xs:complexType name="TokenClaim">
        <xs:sequence>
          <xs:element name="ClaimType" nillable="true" type="xs:string" />
          <xs:element minOccurs="0" name="ClaimValue" nillable="true" type="xs:string" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="TokenClaim" nillable="true" type="tns:TokenClaim" />
      <xs:complexType name="TokenRestrictionTemplate">
        <xs:sequence>
          <xs:element minOccurs="0" name="AlternateVerificationKeys" nillable="true" type="tns:ArrayOfTokenVerificationKey" />
          <xs:element name="Audience" nillable="true" type="xs:anyURI" />
          <xs:element name="Issuer" nillable="true" type="xs:anyURI" />
          <xs:element name="PrimaryVerificationKey" nillable="true" type="tns:TokenVerificationKey" />
          <xs:element minOccurs="0" name="RequiredClaims" nillable="true" type="tns:ArrayOfTokenClaim" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="TokenRestrictionTemplate" nillable="true" type="tns:TokenRestrictionTemplate" />
      <xs:complexType name="ArrayOfTokenVerificationKey">
        <xs:sequence>
          <xs:element minOccurs="0" maxOccurs="unbounded" name="TokenVerificationKey" nillable="true" type="tns:TokenVerificationKey" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="ArrayOfTokenVerificationKey" nillable="true" type="tns:ArrayOfTokenVerificationKey" />
      <xs:complexType name="TokenVerificationKey">
        <xs:sequence />
      </xs:complexType>
      <xs:element name="TokenVerificationKey" nillable="true" type="tns:TokenVerificationKey" />
      <xs:complexType name="ArrayOfTokenClaim">
        <xs:sequence>
          <xs:element minOccurs="0" maxOccurs="unbounded" name="TokenClaim" nillable="true" type="tns:TokenClaim" />
        </xs:sequence>
      </xs:complexType>
      <xs:element name="ArrayOfTokenClaim" nillable="true" type="tns:ArrayOfTokenClaim" />
      <xs:complexType name="SymmetricVerificationKey">
        <xs:complexContent mixed="false">
          <xs:extension base="tns:TokenVerificationKey">
            <xs:sequence>
              <xs:element name="KeyValue" nillable="true" type="xs:base64Binary" />
            </xs:sequence>
          </xs:extension>
        </xs:complexContent>
      </xs:complexType>
      <xs:element name="SymmetricVerificationKey" nillable="true" type="tns:SymmetricVerificationKey" />
    </xs:schema>

Kun käytäntö määritetään **tunnuksen** rajoitettu, sinun on määritettävä ensisijainen** vahvistus-näppäintä**, **myöntäjä** ja **yleisön** parametrit. **Ensisijainen vahvistus avain **on avain, joka on allekirjoitettu tunnuksen, **myöntäjä** on suojattu suojaustunnuksen palvelu, joka ongelmat tunnuksen. **Käyttäjäryhmän** (kutsutaan joskus **laajuus**) kuvataan tunnuksen palveluita tai resurssin tunnus sallii käyttöoikeus. Media-palveluiden avaimen toimitus-palvelu tarkistaa tunnuksen kyseiset arvot vastaavat arvot mallissa. 

**Media Services SDK.NET**käytettäessä voit **TokenRestrictionTemplate** luokan rajoitus tunnuksen luomiseen.
Seuraavassa esimerkissä luodaan luvan käytännön suojaustunnuksen rajoituksen kanssa. Tässä esimerkissä asiakas on esittää tunnuksen, joka sisältää: Kirjautuminen key (VerificationKey), tunnuksen myöntäjän ja tarvittavat saatavat.
    
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

####<a id="test"></a>Testi-tunnus

Saat vahvistustunnuksen rajoitus, jota käytettiin avaimen luvan käytännön perusteella testi-tunnuksen, toimi seuraavasti.
    
    // Deserializes a string containing an Xml representation of a TokenRestrictionTemplate
    // back into a TokenRestrictionTemplate class instance.
    TokenRestrictionTemplate tokenTemplate =
        TokenRestrictionTemplateSerializer.Deserialize(tokenTemplateString);
    
    // Generate a test token based on the the data in the given TokenRestrictionTemplate.
    // Note, you need to pass the key id Guid because we specified 
    // TokenClaim.ContentKeyIdentifierClaim in during the creation of TokenRestrictionTemplate.
    Guid rawkey = EncryptionUtils.GetKeyIdAsGuid(key.Id);
    
    //The GenerateTestToken method returns the token without the word “Bearer” in front
    //so you have to add it in front of the token string. 
    string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate, null, rawkey);
    Console.WriteLine("The authorization token is:\nBearer {0}", testToken);
    Console.WriteLine();


##<a name="playready-dynamic-encryption"></a>PlayReady dynaaminen salaus 

Media-palveluiden avulla voit määrittää oikeudet ja rajoitukset, jota haluat käyttää PlayReady DRM runtime voidaan valvoa, kun käyttäjä yrittää toistaa suojattua sisältöä. 

Kun suojaat sisällön kanssa PlayReady toimea, sinun on määritettävä luvan käytännön on XML-merkkijono, joka määrittää [PlayReady käyttöoikeuden malli](media-services-playready-license-template-overview.md). Media Services SDK .NET- **PlayReadyLicenseResponseTemplate** ja **PlayReadyLicenseTemplate** luokkien auttaa sinua määrittää PlayReady käyttöoikeus-malli.

[Tässä ohjeaiheessa](media-services-protect-with-drm.md) esitellään salaaminen **PlayReady** ja **Widevine**sisältöä.

###<a name="open-restriction"></a>Avaa rajoitus
    
Avaa rajoitus tarkoittaa, että järjestelmä toimittaa avain kaikkien sellaisten käyttäjien, niiden tekijöistä avaimen pyynnön. Tämä rajoitus voi olla hyötyä testausta varten.

Seuraava esimerkki luo Avaa todennus-käytäntö ja lisää sen sisällön avaimen.

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
    
        // Configure PlayReady license template.
        string newLicenseTemplate = ConfigurePlayReadyLicenseTemplate();
    
        IContentKeyAuthorizationPolicyOption policyOption =
            _context.ContentKeyAuthorizationPolicyOptions.Create("",
                ContentKeyDeliveryType.PlayReadyLicense,
                    restrictions, newLicenseTemplate);
    
        IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                    ContentKeyAuthorizationPolicies.
                    CreateAsync("Deliver Common Content Key with no restrictions").
                    Result;
    
    
        contentKeyAuthorizationPolicy.Options.Add(policyOption);
    
        // Associate the content key authorization policy with the content key.
        contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
        contentKey = contentKey.UpdateAsync().Result;
    }

###<a name="token-restriction"></a>Suojaustunnuksen rajoitus

Jos haluat suojaustunnuksen rajoitus-asetusten määrittäminen käyttää XML tunnuksen luvan vaatimukset kuvaamiseen. Suojaustunnuksen rajoituksen määritysten XML on oltava XML-rakenteen [Tässä](#schema) osassa näytetään.
    
    public static string AddTokenRestrictedAuthorizationPolicy(IContentKey contentKey)
    {
        string tokenTemplateString = GenerateTokenRequirements();
    
        IContentKeyAuthorizationPolicy policy = _context.
                                ContentKeyAuthorizationPolicies.
                                CreateAsync("HLS token restricted authorization policy").Result;
    
        List<ContentKeyAuthorizationPolicyRestriction> restrictions = new List<ContentKeyAuthorizationPolicyRestriction>
        {
            new ContentKeyAuthorizationPolicyRestriction 
            { 
                Name = "Token Authorization Policy", 
                KeyRestrictionType = (int)ContentKeyRestrictionType.TokenRestricted,
                Requirements = tokenTemplateString, 
            }
        };
    
        // Configure PlayReady license template.
        string newLicenseTemplate = ConfigurePlayReadyLicenseTemplate();
    
        IContentKeyAuthorizationPolicyOption policyOption =
            _context.ContentKeyAuthorizationPolicyOptions.Create("Token option",
                ContentKeyDeliveryType.PlayReadyLicense,
                    restrictions, newLicenseTemplate);
    
        IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                    ContentKeyAuthorizationPolicies.
                    CreateAsync("Deliver Common Content Key with no restrictions").
                    Result;
                
        policy.Options.Add(policyOption);
    
        // Add ContentKeyAutorizationPolicy to ContentKey
        contentKeyAuthorizationPolicy.Options.Add(policyOption);
    
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


Testi-tunnuksen perusteella suojaustunnuksen rajoitus, jota käytettiin avaimen hyväksymistä käytännön asentamisesta [sisältö](#test) . 

##<a id="types"></a>Käytetään määritettäessä ContentKeyAuthorizationPolicy tyypit

###<a id="ContentKeyRestrictionType"></a>ContentKeyRestrictionType

    public enum ContentKeyRestrictionType
    {
        Open = 0,
        TokenRestricted = 1,
        IPRestricted = 2,
    }

###<a id="ContentKeyDeliveryType"></a>ContentKeyDeliveryType

    public enum ContentKeyDeliveryType
    {
      None = 0,
      PlayReadyLicense = 1,
      BaselineHttp = 2,
      Widevine = 3
    }

###<a id="TokenType"></a>TokenType

    public enum TokenType
    {
        Undefined = 0,
        SWT = 1,
        JWT = 2,
    }



##<a name="media-services-learning-paths"></a>Media-palveluiden oppimispolut

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Palautteen antaminen

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="next-step"></a>Seuraava vaihe
Nyt kun olet määrittänyt sisällön avain luvan käytäntö, siirry [resurssi toimituksen käytännön määrittäminen](media-services-dotnet-configure-asset-delivery-policy.md) aihetta.
 
