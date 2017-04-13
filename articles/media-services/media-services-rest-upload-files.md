<properties 
    pageTitle="Tiedostojen lataaminen käyttämällä muiden Media Services-tilille | Microsoft Azure" 
    description="Opettele mediasisältöä tuoda Media Services luomalla ja resurssien ladataan." 
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
    ms.date="09/19/2016"
    ms.author="juliako"/>


# <a name="upload-files-into-a-media-services-account-using-rest"></a>Tiedostojen lataaminen käyttämällä muiden Media Services-tilille

 > [AZURE.SELECTOR]
 - [.NET](media-services-dotnet-upload-files.md)
 - [MUILLE KÄYTTÄJILLE](media-services-rest-upload-files.md)
 - [Portal](media-services-portal-upload-files.md)

Media-palveluja Lataa digitaalisten tiedostojen sijoituksen kyselyjä. [Kohteiden](https://msdn.microsoft.com/library/azure/hh974277.aspx) kohde voi olla video, ääni, kuvia, pikkukuvan sivustokokoelmat, tekstin raidat tekstitys tiedostot (ja nämä tiedostot metatietoa.)  Kun tiedostot on ladattu palvelimeen kyselyjä kohteen, sisältöä tallennetaan Pilvipalvelun myöhempää käsittelyä ja streaming suojatusti. 

>[AZURE.NOTE]Kun valitset resurssi-tiedostonimi ottaa huomioon seuraavat asiat:
>
>- Media Services käyttää IAssetFile.Name-ominaisuuden arvon luotaessa URL-osoitteet streaming sisällön (esimerkiksi http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) Tästä syystä URL-koodausta ei sallita. Ominaisuuden **nimi** ei saa olla seuraavia [prosentti-koodaus-varattu merkit](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters): !*'();:@&=+$,/?%#[]". Lisäksi voi olla vain yksi ".' tiedostotunnisteen varten.
>
>- Nimen pituus ei pitäisi olla suurempi kuin 260 merkkiä.

Seuraavissa osissa on jaettu basic työnkulun ladataan kalusto:

- Sijoituksen luominen
- Salaa omaisuuden (valinnainen)
- Tiedoston lataaminen-blob-säiliö

AMS avulla voit ladata varat kerralla. Lisätietoja on artikkelissa [sisältö](media-services-rest-upload-files.md#upload_in_bulk) .

##<a name="upload-assets"></a>Lataa resurssit

###<a name="create-an-asset"></a>Sijoituksen luominen

>[AZURE.NOTE] Kun käsittelet Media Services REST-Ohjelmointirajapinta, ottaa huomioon seuraavat asiat:
>
>Kohteiden Media-palveluita käyttäessään sinun on määritettävä HTTP-pyyntöjen tietyn otsikossa näkyvät kentät ja arvot. Lisätietoja on artikkelissa [Media Services REST API kehittäminen asetukset](media-services-rest-how-to-use.md).

>Kun https://media.windows.net muodostettu yhteys, saat 301 uudelleenohjaus, määrittämällä toisen Media palveluiden URI-tunnus. Sinun on tehtävä seuraavat suosikkiryhmän puhelut [yhteyden muodostaminen Media-palveluita käyttämällä REST API](media-services-rest-connect-programmatically.md)kuvattuja uusi URI-osoitteeseen. 
 
Sijoituksen on useita tiedostotyypit ja Media-palveluja, kuten video, ääni, kuvia, pikkukuvan sivustokokoelmat, tekstin raidat ja tekstitys tiedostot objektien tietojoukkoa säilö. REST API-liittymän sijoituksen luominen edellyttää POST-pyynnön lähettäminen Media Services ja lisäämällä ominaisuutta koskevia tietoja oman resurssi pyynnön tekstissä.

Ominaisuudet, jotka voit määrittää sijoituksen luotaessa on **asetukset**. **Asetukset** on numerointiarvo, joka kuvaa sijoituksen voidaan luoda haluamasi salausasetukset. Kelvollinen arvo on jokin arvoista ei arvojen yhdistelmän alla olevasta luettelosta. 

- **Ei mitään** = **0**: salausta ei voi käyttää. Tämä on oletusarvo. Huomaa, että kun tämä vaihtoehto sisältöä ei ole suojattu salataanko siirrettävät tai muiden tallennustilaan.
    Jos haluat pitää MP4 käyttämällä asteittain lataaminen, käytä tätä vaihtoehtoa. 

- **StorageEncrypted** = **1**: Määritä, jos haluat salata AES 256 bittinen salauksella Lataa ja tallennustilaa tiedostojen.

    Jos yhteyttä resurssi on salattu tallennustilan, sinun on määritettävä resurssi toimituksen käytännön. Katso lisätietoja [määrittäminen resurssi toimituksen käytännön](media-services-rest-configure-asset-delivery-policy.md).

- **CommonEncryptionProtected** = **2**: Määritä, jos lataat tiedostoja, jotka on suojattu yleisiä salausmenetelmä (kuten PlayReady). 

- **EnvelopeEncryptionProtected** = **4**: Määritä, jos lataat HLS salattu AES-tiedostoja. Huomaa, että tiedostot on koodattu ja salattu Transform hallinta.

>[AZURE.NOTE]Oman resurssi käytetään salausta, jos luominen **ContentKey** ja linkittää sen oman resurssi ohjeiden mukaisesti seuraavissa ohjeaiheissa:[luomisesta ContentKey](media-services-rest-create-contentkey.md). Huomaa, että kun olet ladannut tiedostojen siirtäminen kohteen, sinun on päivitettävä **AssetFile** kohteen salauksen ominaisuudet arvoilla, onko **resurssi** salauksen aikana. Älä **Yhdistä** HTTP-pyynnön avulla. 


Seuraavassa esimerkissä esitetään, miten voit luoda sijoituksen.

**HTTP-pyyntö**

    POST https://media.windows.net/api/Assets HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-2233-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: media.windows.net
    
    {"Name":"BigBuckBunny.mp4"}
    

**HTTP-vastaus**

Jos onnistuu, palautetaan seuraavasti:
    
    HTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 452
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/Assets('nb%3Acid%3AUUID%3A9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1')
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: c59de965-bc89-4295-9a57-75d897e5221e
    request-id: e98be122-ae09-473a-8072-0ccd234a0657
    x-ms-request-id: e98be122-ae09-473a-8072-0ccd234a0657
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sun, 18 Jan 2015 22:06:40 GMT
    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Assets/@Element",
       "Id":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "State":0,
       "Created":"2015-01-18T22:06:40.6010903Z",
       "LastModified":"2015-01-18T22:06:40.6010903Z",
       "AlternateId":null,
       "Name":"BigBuckBunny.mp4",
       "Options":0,
       "Uri":"https://storagetestaccount001.blob.core.windows.net/asset-9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "StorageAccountName":"storagetestaccount001"
    }
    
###<a name="create-an-assetfile"></a>Luo AssetFile

[AssetFile](http://msdn.microsoft.com/library/azure/hh974275.aspx) kohteen edustaa video- tai tiedosto, joka on tallennettu blob-säilö. Resurssi-tiedosto on aina liitetty amerikkalaisen ja amerikkalaisen voi olla yksi tai usea resurssi-tiedostoja. Media-palveluiden Encoder tehtävän epäonnistuu, jos objektin resurssi tiedosto ei ole liitetty digitaalinen tiedoston blob-säilö.

Huomaa, että **AssetFile** esiintymän ja todellinen mediatiedosto ovat kaksi erillistä objektia. AssetFile esiintymää on metatietoa mediatiedosto, mediatiedosto on todellinen mediasisältöä.

Kun olet ladannut digitaalisen mediatiedoston blob säilöön, Päivitä tiedot AssetFile tietoja media-tiedosto (kuten artikkelin) **Yhdistä** HTTP-pyynnön avulla. 

**HTTP-pyyntö**

    POST https://media.windows.net/api/Files HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-4ca2-2233-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: media.windows.net
    Content-Length: 164
    
    {  
       "IsEncrypted":"false",
       "IsPrimary":"false",
       "MimeType":"video/mp4",
       "Name":"BigBuckBunny.mp4",
       "ParentAssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1"
    }


**HTTP-vastaus**

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 535
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/Files('nb%3Acid%3AUUID%3Af13a0137-0a62-9d4c-b3b9-ca944b5142c5')
    Server: Microsoft-IIS/8.5
    request-id: 98a30e2d-f379-4495-988e-0b79edc9b80e
    x-ms-request-id: 98a30e2d-f379-4495-988e-0b79edc9b80e
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 19 Jan 2015 00:34:07 GMT
    
    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Files/@Element",
       "Id":"nb:cid:UUID:f13a0137-0a62-9d4c-b3b9-ca944b5142c5",
       "Name":"BigBuckBunny.mp4",
       "ContentFileSize":"0",
       "ParentAssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "EncryptionVersion":null,
       "EncryptionScheme":null,
       "IsEncrypted":false,
       "EncryptionKeyId":null,
       "InitializationVector":null,
       "IsPrimary":false,
       "LastModified":"2015-01-19T00:34:08.1934137Z",
       "Created":"2015-01-19T00:34:08.1934137Z",
       "MimeType":"video/mp4",
       "ContentChecksum":null
    }


### <a name="creating-the-accesspolicy-with-write-permission"></a>Luomalla AccessPolicy kirjoitusoikeudet. 

Ennen lataamista tiedostoista, jotka Blob-objektien tallennustilaan kyselyjä, Määritä käytäntö oikeudet sijoituksen kirjoittaminen access. Saadakseen ne kirjaa AccessPolicies kohde-Määritä HTTP-pyynnön. Määritä DurationInMinutes arvon luotaessa tai saat 500 sisäisen palvelimen virhesanoma vastauksen takaisin sisään. Saat lisätietoja AccessPolicies [AccessPolicy](http://msdn.microsoft.com/library/azure/hh974297.aspx).

Seuraavassa esimerkissä AccessPolicy luominen:
        
**HTTP-pyyntö**

    POST https://media.windows.net/api/AccessPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-2233-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: media.windows.net
    
    {"Name":"NewUploadPolicy", "DurationInMinutes":"440", "Permissions":"2"} 

**HTTP-pyyntö**

    If successful, the following response is returned:
    
    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 312
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/AccessPolicies('nb%3Apid%3AUUID%3Abe0ac48d-af7d-4877-9d60-1805d68bffae')
    Server: Microsoft-IIS/8.5
    request-id: 74c74545-7e0a-4cd6-a440-c1c48074a970
    x-ms-request-id: 74c74545-7e0a-4cd6-a440-c1c48074a970
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sun, 18 Jan 2015 22:18:06 GMT

    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#AccessPolicies/@Element",
       "Id":"nb:pid:UUID:be0ac48d-af7d-4877-9d60-1805d68bffae",
       "Created":"2015-01-18T22:18:06.6370575Z",
       "LastModified":"2015-01-18T22:18:06.6370575Z",
       "Name":"NewUploadPolicy",
       "DurationInMinutes":440.0,
       "Permissions":2
    }

###<a name="get-the-upload-url"></a>Lataa URL-Osoitteen hankkiminen

Vastaanottamaan todellinen lataus-URL SAS Locator luominen Locators ohjelmat, joita haluat käyttää sijoituksen tiedostot Määritä alkamis- ja yhteyden endpoint tyyppi. Voit luoda useita Locator kohteet annetun AccessPolicy ja kohteiden pari käsittelemään toisen asiakkaan pyynnöt ja tarpeisiin. Kaikkien näiden Locators avulla voit määrittää URL-Osoitteen avulla voidaan pituutta aloitusajan arvo plus AccessPolicy DurationInMinutes-arvo. Lisätietoja on artikkelissa [Locator](http://msdn.microsoft.com/library/azure/hh974308.aspx).


SAS URL-osoite on seuraavanlainen:

    {https://myaccount.blob.core.windows.net}/{asset name}/{video file name}?{SAS signature}

Käytä seikkoja:

- Ei voi olla enintään viisi yksilöllinen Locators liittyvät annetulle kohteiden samalla kertaa. Lisätietoja on artikkelissa Locator.
- Jos haluat ladata tiedostoja heti, sinun kannattaa asettaa aloitusajan arvo 5 minuutin ajan ennen nykyisen kellonajan. Tämä johtuu siitä voi olla kellon jakauman.vinous asiakaskoneeseen ja Media-palveluiden välillä. Lisäksi aloitusajan arvo on oltava DateTime-muodossa: YYYY-MM-DDTHH:mm:ssZ (esimerkiksi "2014-05-23T17:53:50Z").   
- Ehkä 30 – 40 sekunnin viive jälkeen Locator luodaan kun se on käytettävissä. Tämä ongelma koskee sekä SAS URL-osoite ja Origin Locators.

Seuraavassa esimerkissä luomisesta SAS URL-Locator määrittämä tyyppi-ominaisuuden pyyntö tekstissä ("1", SAS locator) ja "2" edelleen tarvittaessa-origin locator. Palauttaa **Path** -ominaisuus on URL-osoite, sinun on käytettävä Lataa tiedosto.
    
**HTTP-pyyntö**
    
    POST https://media.windows.net/api/Locators HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-4ca2-2233-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: media.windows.net
    {  
       "AccessPolicyId":"nb:pid:UUID:be0ac48d-af7d-4877-9d60-1805d68bffae",
       "AssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "StartTime":"2015-02-18T16:45:53",
       "Type":1
    }


**HTTP-vastaus**

Jos onnistuu, funktio palauttaa seuraavan vastauksen:
        
    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 949
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/Locators('nb%3Alid%3AUUID%3Aaf57bdd8-6751-4e84-b403-f3c140444b54')
    Server: Microsoft-IIS/8.5
    request-id: 2adeb1f8-89c5-4cc8-aa4f-08cdfef33ae0
    x-ms-request-id: 2adeb1f8-89c5-4cc8-aa4f-08cdfef33ae0
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 19 Jan 2015 03:01:29 GMT
    
    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Locators/@Element",
       "Id":"nb:lid:UUID:af57bdd8-6751-4e84-b403-f3c140444b54",
       "ExpirationDateTime":"2015-02-19T00:05:53",
       "Type":1,
       "Path":"https://storagetestaccount001.blob.core.windows.net/asset-f438649c-313c-46e2-8d68-7d2550288247?sv=2012-02-12&sr=c&si=af57bdd8-6751-4e84-b403-f3c140444b54&sig=fE4btwEfZtVQFeC0Wh3Kwks2OFPQfzl5qTMW5YytiuY%3D&st=2015-02-18T16%3A45%3A53Z&se=2015-02-19T00%3A05%3A53Z",
       "BaseUri":"https://storagetestaccount001.blob.core.windows.net/asset-f438649c-313c-46e2-8d68-7d2550288247",
       "ContentAccessComponent":"?sv=2012-02-12&sr=c&si=af57bdd8-6751-4e84-b403-f3c140444b54&sig=fE4btwEfZtVQFeC0Wh3Kwks2OFPQfzl5qTMW5YytiuY%3D&st=2015-02-18T16%3A45%3A53Z&se=2015-02-19T00%3A05%3A53Z",
       "AccessPolicyId":"nb:pid:UUID:be0ac48d-af7d-4877-9d60-1805d68bffae",
       "AssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "StartTime":"2015-02-18T16:45:53",
       "Name":null
    }

### <a name="upload-a-file-into-a-blob-storage-container"></a>Tiedoston lataaminen blob storage säilöön
    
Kun olet AccessPolicy ja määritä Locator, todellinen tiedosto ladataan käyttämällä Azure-tallennustilan REST API Azure-Blob-säiliö-säilöön. Voit ladata sivun tai estää BLOB-objektit. 

>[AZURE.NOTE] Sinun on lisättävä Tiedostonimi tiedoston haluat ladata vastaanotettu edellisessä osassa Locator **polku** -arvoa. Esimerkiksi https://storagetestaccount001.blob.core.windows.net/asset-e7b02da4-5a69-40e7-a8db-e8f4f697aac0/BigBuckBunny.mp4? . . . 

Katso lisätietoja käsittelystä tallennustilan Azure-BLOB- [Blob-objektien palvelun REST API](http://msdn.microsoft.com/library/azure/dd135733.aspx).


### <a name="update-the-assetfile"></a>Päivitä AssetFile 

Nyt kun tiedosto on ladattu, Päivitä FileAsset koko (ja muut). Esimerkki:
    
    MERGE https://media.windows.net/api/Files('nb%3Acid%3AUUID%3Af13a0137-0a62-9d4c-b3b9-ca944b5142c5') HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-4ca2-2233-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421662918&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=utmoXXbm9Q7j4tW1yJuMVA3egRiQy5FPygwadkmPeaY%3d
    x-ms-version: 2.11
    Host: media.windows.net
    
    {  
       "ContentFileSize":"1186540",
       "Id":"nb:cid:UUID:f13a0137-0a62-9d4c-b3b9-ca944b5142c5",
       "MimeType":"video/mp4",
       "Name":"BigBuckBunny.mp4",
       "ParentAssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1"
    }


**HTTP-vastaus**

Jos onnistuu, seuraava kaava palauttaa: HTTP/1.1 204 ei sisältöä

### <a name="delete-the-locator-and-accesspolicy"></a>Poista Locator ja AccessPolicy 

**HTTP-pyyntö**


    DELETE https://media.windows.net/api/Locators('nb%3Alid%3AUUID%3Aaf57bdd8-6751-4e84-b403-f3c140444b54') HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-2233-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421662918&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=utmoXXbm9Q7j4tW1yJuMVA3egRiQy5FPygwadkmPeaY%3d
    x-ms-version: 2.11
    Host: media.windows.net

    
**HTTP-vastaus**

Jos onnistuu, palautetaan seuraavasti:

    HTTP/1.1 204 No Content 
    ...

**HTTP-pyyntö**

    DELETE https://media.windows.net/api/AccessPolicies('nb%3Apid%3AUUID%3Abe0ac48d-af7d-4877-9d60-1805d68bffae') HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-6753-2233-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421662918&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=utmoXXbm9Q7j4tW1yJuMVA3egRiQy5FPygwadkmPeaY%3d
    x-ms-version: 2.11
    Host: media.windows.net

**HTTP-vastaus**

Jos onnistuu, palautetaan seuraavasti:

    HTTP/1.1 204 No Content 
    ...

##<a id="upload_in_bulk"></a>Lataa varat kerralla

###<a name="create-the-ingestmanifest"></a>Luo IngestManifest

IngestManifest on joukko varat, resurssi-tiedostot ja tilastotiedot, jonka avulla voidaan määrittää joukon ingesting joukkona etenemisen säilö.


**HTTP-pyyntö**

    POST https:// media.windows.net/API/IngestManifests HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net
    Content-Length: 36
    Expect: 100-continue
    
    { "Name" : "ExampleManifestREST" }

###<a name="create-assets"></a>Luo resurssit

Ennen kuin luot IngestManifestAsset, sinun täytyy luoda resurssi, joka on valmis käyttämällä joukkona ingesting. Sijoituksen on useita tiedostotyypit ja Media-palveluja, kuten video, ääni, kuvia, pikkukuvan sivustokokoelmat, tekstin raidat ja tekstitys tiedostot objektien tietojoukkoa säilö. REST-Ohjelmointirajapinnalla sijoituksen luominen edellyttää HTTP POST-pyynnön lähettäminen Microsoft Azure Media Services ja lisäämällä ominaisuutta koskevia tietoja oman resurssi pyynnön tekstissä. Tässä esimerkissä käyttöomaisuus luodaan pyynnön teksti sisältyy StorageEncrption(1)-vaihtoehdon.

**HTTP-vastaus**

    POST https://media.windows.net/API/Assets HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net
    Content-Length: 55
    Expect: 100-continue
    
    { "Name" : "ExampleManifestREST_Asset", "Options" : 1 }

###<a name="create-the-ingestmanifestassets"></a>Luo IngestManifestAssets

IngestManifestAssets edustavat kalusto IngestManifest sisällä, joita käytetään joukkona ingesting. Linkki lähinnä kohteen luettelo. Azure Media-palveluiden seuraa sisäisesti perusteella IngestManifestFiles sivustokokoelman IngestManifestAsset liittyvän tiedoston lataamisen. Kun tiedostot on ladattu palvelimeen, kohteen on valmis. Voit luoda uuden IngestManifestAsset HTTP POST-pyynnön. Pyyntö tekstiosaan ovat IngestManifest-tunnus ja kohteen tunnus, joka IngestManifestAsset olisi linkki yhdessä for joukkona ingesting.

**HTTP-vastaus**

    POST https://media.windows.net/API/IngestManifestAssets HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net
    Content-Length: 152
    Expect: 100-continue
    { "ParentIngestManifestId" : "nb:mid:UUID:5c77f186-414f-8b48-8231-17f9264e2048", "Asset" : { "Id" : "nb:cid:UUID:b757929a-5a57-430b-b33e-c05c6cbef02e"}}


###<a name="create-the-ingestmanifestfiles-for-each-asset"></a>Luo IngestManifestFiles kunkin resurssi

IngestManifestFile edustaa todellinen videon tai äänen blob-objekti, joka voi ladata joukkona ingesting, annetaan osana. Salauksen, jotka liittyvät ominaisuudet eivät ole pakollisia, ellei kohteen Käytä salaus-asetusta. Tässä osassa käytetyn Esimerkki käsittelee käyttävän luominen IngestManifestFile StorageEncryption, aiemmin luotu annetaan.


**HTTP-vastaus**

    POST https://media.windows.net/API/IngestManifestFiles HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net
    Content-Length: 367
    Expect: 100-continue
    
    { "Name" : "REST_Example_File.wmv", "ParentIngestManifestId" : "nb:mid:UUID:5c77f186-414f-8b48-8231-17f9264e2048", "ParentIngestManifestAssetId" : "nb:maid:UUID:beed8531-9a03-9043-b1d8-6a6d1044cdda", "IsEncrypted" : "true", "EncryptionScheme" : "StorageEncryption", "EncryptionVersion" : "1.0", "EncryptionKeyId" : "nb:kid:UUID:32e6efaf-5fba-4538-b115-9d1cefe43510" }
    
###<a name="upload-the-files-to-blob-storage"></a>Lataa tiedostot Blob-objektien tallennustilaan

Voit käyttää minkä tahansa nopea asiakassovellus voi resurssi-tiedostojen lataamisesta blob storage säilöön Uri myöntämä IngestManifest BlobStorageUriForUpload-ominaisuus. Yksi huomattavat nopea Lataa palvelu on [Aspera pyydettäessä Azure-sovelluksen](http://go.microsoft.com/fwlink/?LinkId=272001).

###<a name="monitor-bulk-ingest-progress"></a>Näytön joukkona Ingest käynnissä

Voit valvoa joukko toimintoja IngestManifest ingesting mukaan kyselyt IngestManifest tilasto-ominaisuuden etenemisen. Ominaisuus on monimutkaisia [IngestManifestStatistics](https://msdn.microsoft.com/library/azure/jj853027.aspx)tyyppi. Lisääminen tilasto-ominaisuutta, Lähetä HTTP GET-pyyntö kulkeva IngestManifest ID-tunnuksellasi.
 

##<a name="create-contentkeys-used-for-encryption"></a>Luo ContentKeys käytetään salausta

Jos yhteyttä resurssi käyttävät salausta, sinun on luotava ContentKey, jota käytetään salausta, ennen kuin resurssi-tiedostojen luominen. Tallennustilan salauksen seuraavat ominaisuudet otetaan pyynnön tekstissä.
 
Pyydä leipäteksti-ominaisuus   | Kuvaus
---|---
Tunnus | ContentKey-tunnus, joka on Luo molemmista seuraavassa muodossa "nb:kid:UUID:<NEW GUID>".
ContentKeyType | Tämä on avaimen sisältötyypin sisällön avaimeen kokonaislukuna. Olemme välittää tallennustilan salauksen arvon 1.
EncryptedContentKey | Luodaan uusi sisältö avaimen arvon, joka on 256-bittinen (32 Byte Character Set)-arvo. Avain on salattu tallennustilan salaus X.509-varmenne, joka on noutaminen Microsoft Azure Media Services suorittamalla HTTP GET-pyyntö GetProtectionKeyId ja GetProtectionKey menetelmistä.
ProtectionKeyId | Tämä on tallennustilan salaus X.509-varmenne, jota käytettiin Microsoftin sisällön avain salataan suojauksen avaimen tunnus.
ProtectionKeyType | Tämä on salaustyyppi, jota käytettiin sisällön avaimen salaamiseen suojaus-näppäintä. Tämä arvo on StorageEncryption(1) esimerkissä.
Tarkistussumma |MD5 lasketun tarkistussumman sisältö-näppäimen. Se on laskettu salaamalla sisällön tunnus sisältö-näppäimen kanssa. Esimerkkikoodi esitellään, miten tarkistussumman laskemiseen.


**HTTP-vastaus**
    
    POST https://media.windows.net/api/ContentKeys HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net
    Content-Length: 572
    Expect: 100-continue
    
    {"Id" : "nb:kid:UUID:316d14d4-b603-4d90-b8db-0fede8aa48f8", "ContentKeyType" : 1, "EncryptedContentKey" : "Y4NPej7heOFa2vsd8ZEOcjjpu/qOq3RJ6GRfxa8CCwtAM83d6J2mKOeQFUmMyVXUSsBCCOdufmieTKi+hOUtNAbyNM4lY4AXI537b9GaY8oSeje0NGU8+QCOuf7jGdRac5B9uIk7WwD76RAJnqyep6U/OdvQV4RLvvZ9w7nO4bY8RHaUaLxC2u4aIRRaZtLu5rm8GKBPy87OzQVXNgnLM01I8s3Z4wJ3i7jXqkknDy4VkIyLBSQvIvUzxYHeNdMVWDmS+jPN9ScVmolUwGzH1A23td8UWFHOjTjXHLjNm5Yq+7MIOoaxeMlKPYXRFKofRY8Qh5o5tqvycSAJ9KUqfg==", "ProtectionKeyId" : "7D9BB04D9D0A4A24800CADBFEF232689E048F69C", "ProtectionKeyType" : 1, "Checksum" : "TfXtjCIlq1Y=" }

### <a name="link-the-contentkey-to-the-asset"></a>Linkki ContentKey kohteen

ContentKey on liitetty yhteen tai useampaan lähettämällä HTTP POST pyynnön. Seuraava pyyntö on esimerkki Esimerkki ContentKey linkittäminen Esimerkki Käyttöomaisuus ID-tunnuksellasi.

**HTTP-vastaus**
    
    POST https://media.windows.net/API/Assets('nb:cid:UUID:b3023475-09b4-4647-9d6d-6fc242822e68')/$links/ContentKeys HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net
    Content-Length: 113
    Expect: 100-continue
    
    { "uri": "https://media.windows.net/api/ContentKeys('nb%3Akid%3AUUID%3A32e6efaf-5fba-4538-b115-9d1cefe43510')"}

**HTTP-vastaus**

    GET https://media.windows.net/API/IngestManifests('nb:mid:UUID:5c77f186-414f-8b48-8231-17f9264e2048') HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=070500D0-F35C-4A5A-9249-485BBF4EC70B&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1334275521&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=GxdBb%2fmEyN7iHdNxbawawHRftLhPFFqxX1JZckuv3hY%3d
    Host: media.windows.net



##<a name="next-step"></a>Seuraava vaihe

Tarkista oppimispolut Media-palveluita.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Palautteen antaminen

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]



 
[How to Get a Media Processor]: media-services-get-media-processor.md
 
