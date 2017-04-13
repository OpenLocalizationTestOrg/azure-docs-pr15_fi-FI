<properties 
    pageTitle="Määrittäminen resurssi toimituksen käytännöt Media REST API: N avulla | Microsoft Azure" 
    description="Tässä ohjeaiheessa esitellään eri kohteiden toimituksen käytännöt Media Services REST API määrittämisestä." 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/19/2016"  
    ms.author="juliako"/>

#<a name="configuring-asset-delivery-policies"></a>Kohteiden toimituksen käytäntöjen määrittäminen

[AZURE.INCLUDE [media-services-selector-asset-delivery-policy](../../includes/media-services-selector-asset-delivery-policy.md)]

Jos aiot toimittaa dynaamisesti salattuja kalusto-jokin Media Services sisällön lähettämisen työnkulun vaiheet on määrittäminen resurssien toimituksen käytännöt. Kohteiden toimituksen käytännön kertoo Media Services siitä, miten haluat, että annetaan toimitetaan: kyselyjä mitä streaming protokolla olisi oman resurssi voidaan dynaamisesti pakattu (esimerkiksi MPEG VIIVA, HLS, tasainen Streaming, tai kaikki), onko haluat salata dynaamisesti oman resurssi ja miten (kirjekuori tai yleisten salaus).

Tässä ohjeaiheessa kerrotaan, miksi ja miten voit luoda ja määrittää resurssi toimituksen käytännöt.

>[AZURE.NOTE]On vähintään yksi asteikko (tunnetaan myös nimellä streaming yksikkö) on muista voi käyttää Dynaaminen pakkaus ja dynaaminen salauksen. Lisätietoja on artikkelissa [miten Media-palvelun](media-services-portal-manage-streaming-endpoints.md).
>
>Myös että resurssi on oltava mukautuvat bittitaajuus MP4s tai mukautuvat bittitaajuus tasainen Streaming-tiedostoja.

Voi käyttää eri käytännöt saman resurssi. Voit esimerkiksi lisätä PlayReady salaus tasainen Streaming ja AES kirjekuori salaus MPEG VIIVA ja HLS. Protokollat, joka ei ole määritetty toimitus-käytäntö (esimerkiksi lisätä yhden käytännön, joka määrittää vain protokollaksi HLS) estetty streaming. Poikkeus tähän on, jos sinulla ei ole resurssi toimituksen käytäntö on määritetty lainkaan. Valitse sitten Poista sallittu kaikki protokollat.

Jos haluat pitää tallennustilan salattuja resurssi, sinun on määritettävä kohteen toimituksen käytännön. Ennen kuin oman resurssi voidaan lähettää, streaming palvelimen poistaa tallennustilaa salaus ja virtauttaa määritetyn toimitus-käytännön avulla sisältöä. Esimerkiksi pitää yhteyttä resurssi salattu salaus AES (Advanced Standard) kirjekuoren salausavaimen, Määritä **DynamicEnvelopeEncryption**käytännön tyyppi. Tallennustilan salauksen poistaminen ja poista annetaan virtauttaa, Määritä käytäntö tyypiksi **NoDynamicEncryption**. Esimerkkejä, jotka osoittavat, miten voit määrittää käytännön tällaisia noudattamalla.

Sen mukaan, miten kohteiden toimitus-käytännön määrittäminen voi dynaamisesti paketointi, dynaamisesti salaa ja virtauttaa seuraavista protokollista: tasainen Streaming, HLS, MPEG VIIVA ja HDS virtaa.

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
- Jos sinulla on aiemmin streaming locator sijoituksen, ei voi linkittää uuden käytännön kohteen, Poista olemassa olevaa käytäntöä-kohteen tai päivitä liittyvän kohteen toimitus-käytännön.  Sinun on ensin poistettava streaming locator, säätää käytännöt ja luo streaming locator uudelleen.  Voit käyttää samaa locatorId streaming locator uudelleen, mutta voit varmistaa, joka ei aiheuttaa ongelmia asiakkaille, koska sisältöä voidaan säilyttää välimuistissa alkuperän tai edeltävät CDN.

>[AZURE.NOTE] Kun käsittelet Media Services REST-Ohjelmointirajapinta, ottaa huomioon seuraavat asiat:
>
>Kohteiden Media-palveluita käyttäessään sinun on määritettävä HTTP-pyyntöjen tietyn otsikossa näkyvät kentät ja arvot. Lisätietoja on artikkelissa [Media Services REST API kehittäminen asetukset](media-services-rest-how-to-use.md).

>Kun https://media.windows.net muodostettu yhteys, saat 301 uudelleenohjaus, määrittämällä toisen Media palveluiden URI-tunnus. Sinun on tehtävä seuraavat suosikkiryhmän puhelut [yhteyden muodostaminen Media-palveluita käyttämällä REST API](media-services-rest-connect-programmatically.md)kuvattuja uusi URI-osoitteeseen.


##<a name="clear-asset-delivery-policy"></a>Poista resurssi toimituksen käytäntö

###<a id="create_asset_delivery_policy"></a>Kohteiden toimitus-käytännön luominen
HTTP-pyyntö luo resurssi toimituksen käytännön, joka määrittää koske dynaaminen salaus ja pitää virta jossakin seuraavista protokollista: MPEG VIIVA, HLS ja tasainen Streaming protokollat. 

Saat lisätietoja mitä arvoja voit määrittää AssetDeliveryPolicy luotaessa-kohdassa [tyypit käytetään määritettäessä AssetDeliveryPolicy](#types) .   


Pyyntö:
      
    POST https://media.windows.net/api/AssetDeliveryPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amsaccount1&urn%3aSubscriptionId=zbbef702-e769-2233-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423397827&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=Szo6lbJAvL3dyecAeVmyAnzv3mGzfUNClR5shk9Ivbk%3d
    x-ms-version: 2.11
    x-ms-client-request-id: 4651882c-d7ad-4d5e-86ab-f07f47dcb41e
    Host: media.windows.net
    
    {"Name":"Clear Policy",
    "AssetDeliveryProtocol":7,
    "AssetDeliveryPolicyType":2,
    "AssetDeliveryConfiguration":null}

Vastaus:
    
    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 363
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://media.windows.net/api/AssetDeliveryPolicies('nb%3Aadpid%3AUUID%3A92b0f6ba-3c9f-49b6-a5fa-2a8703b04ecd')
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: 4651882c-d7ad-4d5e-86ab-f07f47dcb41e
    request-id: 6aedbf93-4bc2-4586-8845-fd45590136af
    x-ms-request-id: 6aedbf93-4bc2-4586-8845-fd45590136af
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sun, 08 Feb 2015 06:21:27 GMT
    
    {"odata.metadata":"https://media.windows.net/api/$metadata#AssetDeliveryPolicies/@Element",
    "Id":"nb:adpid:UUID:92b0f6ba-3c9f-49b6-a5fa-2a8703b04ecd",
    "Name":"Clear Policy",
    "AssetDeliveryProtocol":7,
    "AssetDeliveryPolicyType":2,
    "AssetDeliveryConfiguration":null,
    "Created":"2015-02-08T06:21:27.6908329Z",
    "LastModified":"2015-02-08T06:21:27.6908329Z"}
    
###<a id="link_asset_with_asset_delivery_policy"></a>Linkki resurssi resurssi toimituksen käytäntö

HTTP-pyyntö linkittää määritetyn kohteen resurssi toimituksen käytännön.

Pyyntö:

    POST https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3A86933344-9539-4d0c-be7d-f842458693e0')/$links/DeliveryPolicies HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Content-Type: application/json
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amsaccount1&urn%3aSubscriptionId=zbbef702-e769-3344-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423397827&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=Szo6lbJAvL3dyecAeVmyAnzv3mGzfUNClR5shk9Ivbk%3d
    x-ms-version: 2.11
    x-ms-client-request-id: 56d2763f-6e72-419d-ba3c-685f6db97e81
    Host: media.windows.net
    
    {"uri":"https://media.windows.net/api/AssetDeliveryPolicies('nb%3Aadpid%3AUUID%3A92b0f6ba-3c9f-49b6-a5fa-2a8703b04ecd')"}

Vastaus:

    HTTP/1.1 204 No Content


##<a name="dynamicenvelopeencryption-asset-delivery-policy"></a>DynamicEnvelopeEncryption resurssi toimituksen käytäntö 

###<a name="create-content-key-of-the-envelopeencryption-type-and-link-it-to-the-asset"></a>Luo sisällön avain EnvelopeEncryption tyypin ja linkittää sen kohteen

Määritettäessä DynamicEnvelopeEncryption toimituksen käytännön haluat Varmista, että kohteiden linkittäminen sisällön näppäintä EnvelopeEncryption tyyppi. Lisätietoja on artikkelissa: [sisällön avaimen luomista](media-services-rest-create-contentkey.md)).


###<a id="get_delivery_url"></a>Hae toimituksen URL-osoite

Toimituksen URL-Osoitteen hankkiminen edellisessä vaiheessa luotu sisällön avaimen määritetyn toimitustapa. Asiakas käyttää palautetut URL-osoite on pyydettävä AES-näppäintä tai PlayReady käyttöoikeuden tilauksen toistamisen suojattua sisältöä.

Määritä URL-osoitteen hankkiminen HTTP-pyyntö tekstiosaan. Jos suojaat sisällön PlayReady, jossa pyynnön Media Services PlayReady käyttöoikeuden hankkiminen URL-Osoitteen, käyttämällä 1 keyDeliveryType: {"keyDeliveryType": 1}. Jos suojaat kirjekuori-salauksella sisältöä, pyydä avaimen hankintaa URL-osoite määrittämällä 2 keyDeliveryType: {"keyDeliveryType": 2}.

Pyyntö:
    
    POST https://media.windows.net/api/ContentKeys('nb:kid:UUID:dc88f996-2859-4cf7-a279-c52a9d6b2f04')/GetKeyDeliveryUrl HTTP/1.1
    Content-Type: application/json
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amsaccount1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423452029&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=IEXV06e3drSIN5naFRBdhJZCbfEqQbFZsGSIGmawhEo%3d
    x-ms-version: 2.11
    x-ms-client-request-id: 569d4b7c-a446-4edc-b77c-9fb686083dd8
    Host: media.windows.net
    Content-Length: 21
    
    {"keyDeliveryType":2}

Vastaus:
    
    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 198
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: 569d4b7c-a446-4edc-b77c-9fb686083dd8
    request-id: d26f65d2-fe65-4136-8fcf-31545be68377
    x-ms-request-id: d26f65d2-fe65-4136-8fcf-31545be68377
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sun, 08 Feb 2015 21:42:30 GMT
    
    {"odata.metadata":"media.windows.net/api/$metadata#Edm.String","value":"https://amsaccount1.keydelivery.mediaservices.windows.net/?KID=dc88f996-2859-4cf7-a279-c52a9d6b2f04"}


###<a name="create-asset-delivery-policy"></a>Kohteiden toimitus-käytännön luominen

HTTP-pyyntö luo **AssetDeliveryPolicy** , joka on määritetty dynaaminen kirjekuori salaus (**DynamicEnvelopeEncryption**) koskee **HLS** -protokolla (Tässä esimerkissä muut protokollat estetään-streaming). 


Saat lisätietoja mitä arvoja voit määrittää AssetDeliveryPolicy luotaessa-kohdassa [tyypit käytetään määritettäessä AssetDeliveryPolicy](#types) .   

Pyyntö:

    POST https://media.windows.net/api/AssetDeliveryPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amsaccount1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423480651&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=T2FG3tIV0e2ETzxQ6RDWxWAsAzuy3ez2ruXPhrBe62Y%3d
    x-ms-version: 2.11
    x-ms-client-request-id: fff319f6-71dd-4f6c-af27-b675c0066fa7
    Host: media.windows.net
    
    {"Name":"AssetDeliveryPolicy","AssetDeliveryProtocol":4,"AssetDeliveryPolicyType":3,"AssetDeliveryConfiguration":"[{\"Key\":2,\"Value\":\"https:\\/\\/amsaccount1.keydelivery.mediaservices.windows.net\\/\"}]"}

    
Vastaus:
    
    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 460
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: media.windows.net/api/AssetDeliveryPolicies('nb%3Aadpid%3AUUID%3Aec9b994e-672c-4a5b-8490-a464eeb7964b')
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: fff319f6-71dd-4f6c-af27-b675c0066fa7
    request-id: c2a1ac0e-9644-474f-b38f-b9541c3a7c5f
    x-ms-request-id: c2a1ac0e-9644-474f-b38f-b9541c3a7c5f
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 09 Feb 2015 05:24:38 GMT
    
    {"odata.metadata":"media.windows.net/api/$metadata#AssetDeliveryPolicies/@Element","Id":"nb:adpid:UUID:ec9b994e-672c-4a5b-8490-a464eeb7964b","Name":"AssetDeliveryPolicy","AssetDeliveryProtocol":4,"AssetDeliveryPolicyType":3,"AssetDeliveryConfiguration":"[{\"Key\":2,\"Value\":\"https:\\/\\/amsaccount1.keydelivery.mediaservices.windows.net\\/\"}]","Created":"2015-02-09T05:24:38.9167436Z","LastModified":"2015-02-09T05:24:38.9167436Z"}


###<a name="link-asset-with-asset-delivery-policy"></a>Linkki resurssi resurssi toimituksen käytäntö

Katso [linkki resurssi resurssi toimituksen käytäntö](#link_asset_with_asset_delivery_policy)

##<a name="dynamiccommonencryption-asset-delivery-policy"></a>DynamicCommonEncryption resurssi toimituksen käytäntö 

###<a name="create-content-key-of-the-commonencryption-type-and-link-it-to-the-asset"></a>Sisällön avaimen CommonEncryption tyypin luominen ja linkittää sen kohteen

Määritettäessä DynamicCommonEncryption toimituksen käytännön haluat Varmista, että kohteiden linkittäminen sisältö näppäintä CommonEncryption tyyppi. Lisätietoja on artikkelissa: [sisällön avaimen luomista](media-services-rest-create-contentkey.md)).


###<a name="get-delivery-url"></a>Hae toimituksen URL-osoite

Toimituksen URL-Osoitteen hankkiminen PlayReady toimitustapa edellisessä vaiheessa luotu sisältöä tärkeimmistä. Asiakas käyttää palautetut URL-osoite on pyydettävä PlayReady järjestyksessä toistamisen suojatun sisällön käyttöoikeuden. Lisätietoja [Saat toimituksen URL-osoite](#get_delivery_url).

###<a name="create-asset-delivery-policy"></a>Kohteiden toimitus-käytännön luominen

HTTP-pyyntö luo **AssetDeliveryPolicy** , joka on määritetty dynaaminen yhteistä salaus (**DynamicCommonEncryption**) koskee **Tasainen Streaming** -protokolla (Tässä esimerkissä muut protokollat estetään-streaming). 

Saat lisätietoja mitä arvoja voit määrittää AssetDeliveryPolicy luotaessa-kohdassa [tyypit käytetään määritettäessä AssetDeliveryPolicy](#types) .   


Pyyntö:

    POST https://media.windows.net/api/AssetDeliveryPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amsaccount1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423480651&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=T2FG3tIV0e2ETzxQ6RDWxWAsAzuy3ez2ruXPhrBe62Y%3d
    x-ms-version: 2.11
    x-ms-client-request-id: fff319f6-71dd-4f6c-af27-b675c0066fa7
    Host: media.windows.net
    
    {"Name":"AssetDeliveryPolicy","AssetDeliveryProtocol":1,"AssetDeliveryPolicyType":4,"AssetDeliveryConfiguration":"[{\"Key\":2,\"Value\":\"https:\\/\\/amsaccount1.keydelivery.mediaservices.windows.net\/PlayReady\/"}]"}


Jos haluat suojata sisältöä käyttämällä Widevine DRM, Päivitä AssetDeliveryConfiguration arvoista WidevineLicenseAcquisitionUrl (joka on 7 arvo) ja määritä käyttöoikeuden toimitus-palvelun URL-osoite. Voit käyttää seuraavia AMS kumppanien avulla voit pitää Widevine käyttöoikeudet: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/) [castLabs](http://castlabs.com/company/partners/azure/).

Esimerkki: 
 
    
    {"Name":"AssetDeliveryPolicy","AssetDeliveryProtocol":2,"AssetDeliveryPolicyType":4,"AssetDeliveryConfiguration":"[{\"Key\":7,\"Value\":\"https:\\/\\/example.net\/WidevineLicenseAcquisition\/"}]"}

>[AZURE.NOTE]Kun salaamaan Widevine, vain olisi voi pitää käyttämällä VIIVA. Varmista, että VIIVA (2) on määritettävä resurssi toimitus-protokollaa.
  
###<a name="link-asset-with-asset-delivery-policy"></a>Linkki resurssi resurssi toimituksen käytäntö

Katso [linkki resurssi resurssi toimituksen käytäntö](#link_asset_with_asset_delivery_policy)


##<a id="types"></a>Käytetään määritettäessä AssetDeliveryPolicy tyypit

###<a name="assetdeliveryprotocol"></a>AssetDeliveryProtocol 

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

###<a name="assetdeliverypolicytype"></a>AssetDeliveryPolicyType

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

###<a name="contentkeydeliverytype"></a>ContentKeyDeliveryType


    /// <summary>
    /// Delivery method of the content key to the client.
    ///
    </summary>
    public enum ContentKeyDeliveryType
    {
        /// <summary>
        /// None.
        ///
        </summary>
        None = 0,

        /// <summary>
        /// Use PlayReady License acquistion protocol
        ///
        </summary>
        PlayReadyLicense = 1,

        /// <summary>
        /// Use MPEG Baseline HTTP key protocol.
        ///
        </summary>
        BaselineHttp = 2,

        /// <summary>
        /// Use Widevine License acquistion protocol
        ///
        </summary>
        Widevine = 3

    }


###<a name="assetdeliverypolicyconfigurationkey"></a>AssetDeliveryPolicyConfigurationKey

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
