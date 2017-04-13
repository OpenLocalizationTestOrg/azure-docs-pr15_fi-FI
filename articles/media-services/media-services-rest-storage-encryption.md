<properties 
    pageTitle="Sisällön käyttäminen AMS REST API tallennustilan salauksella salaaminen" 
    description="Opettele salaa sisältöä käyttämällä AMS REST API tallennustilan salausta." 
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


#<a name="encrypting-your-content-with-storage-encryption-using-ams-rest-api"></a>Salaaminen sisältöä tallennustilan salauksella AMS REST-Ohjelmointirajapinnan käyttäminen

On erittäin suositeltavaa voit salata käyttää paikallisesti bittistä AES-256-salausta sisältöä ja lataa se sitten Azuren tallennustilaan kohtaa, johon se tallennetaan salataan muille käyttäjille.

Tässä artikkelissa on yleiskatsaus AMS tallennustilan salaus ja näytetään, miten voit ladata tallennustilan salattu sisältö:

- Luo sisällön avain.
- Luo amerikkalaisen. Määritä AssetCreationOption StorageEncryption kohteen luotaessa.

     Salatun kalusto on liitettävä sisältö avaimet.
- Linkki kohteen sisällön avain.  
- Määritä salaus liittyvien kohteiden AssetFile parametrit.
 
>[AZURE.NOTE]Jos haluat pitää tallennustilan salattuja resurssi, sinun on määritettävä kohteen toimituksen käytännön. Ennen kuin oman resurssi voidaan lähettää, streaming palvelimen poistaa tallennustilaa salaus ja virtauttaa määritetyn toimitus-käytännön avulla sisältöä. Lisätietoja on artikkelissa [Määrittäminen resurssi toimituksen käytännöt](media-services-rest-configure-asset-delivery-policy.md).


>[AZURE.NOTE] Kun käsittelet Media Services REST-Ohjelmointirajapinta, ottaa huomioon seuraavat asiat:
>
>Kohteiden Media-palveluita käyttäessään sinun on määritettävä HTTP-pyyntöjen tietyn otsikossa näkyvät kentät ja arvot. Lisätietoja on artikkelissa [Media Services REST API kehittäminen asetukset](media-services-rest-how-to-use.md).

>Kun https://media.windows.net muodostettu yhteys, saat 301 uudelleenohjaus, määrittämällä toisen Media palveluiden URI-tunnus. Sinun on tehtävä seuraavat suosikkiryhmän puhelut [yhteyden muodostaminen Media-palveluita käyttämällä REST API](media-services-rest-connect-programmatically.md)kuvattuja uusi URI-osoitteeseen. 

##<a name="storage-encryption-overview"></a>Tallennustilan salauksen yleiskatsaus 

AMS tallennustilan salaus koskee koko tiedoston **AES tuot.sol./KUORM.RYH** tilassa salausta.  AES TUOT.SOL./KUORM.RYH tila on eston salaus, joka voidaan salata pituudeltaan tiedot ilman täyttöä tarvetta. Se toimii salaamalla laskuri-lohko ja AES algoritmin ja sitten XOR-tekeminen AES tulosteen tiedoilla salata tai purkaa.  Laskuri-lohko käytetään muodostetaan kopioimalla InitializationVector arvo laskuri-arvo 0 – 7 tavuina ja tavua 8-15 laskuri-arvo arvoksi 0. 16 tavua laskuri-lohko tavua 15 8 (eli vähiten merkitsevää tavua) käytetään yksinkertainen 64-bittinen Etumerkitön kokonaisluku, joka arvo kasvaa yhdellä kunkin myöhemmin tietolohko käsitteleminen ja on käytettävissä verkon tavu järjestyksessä. Huomaa, että, jos tämä kokonaisluku saavuttaa kasvavat palauttaa suurimman arvon (0xFFFFFFFFFFFFFFFF) estä laskuri nolla (tavua 8-15) vaikuttamatta muihin 64 bittiä, laskuri (eli tavua 0 – 7).   Säilyttää AES TUOT.SOL./KUORM.RYH tilassa salaus suojaus, jotta annetun avaimen-tunniste kunkin sisällön avaimen InitializationVector arvon on oltava yksilöllinen kaikille tuotaville tiedostoille ja tiedostojen on oltava pienempi kuin 2 ^ 64 lohkot pituus.  Tämä on varmistettava laskuri-arvoa käytetään aina uudelleen annetun avaimella. Saat lisätietoja tuot.sol./KUORM.RYH-tilasta [tämän wikisivun](https://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#CTR) (wiki-artikkelissa käytetään termiä "Nonce-tiedot" sen sijaan, että "InitializationVector").

**Tallennustilan salauksen** avulla voit salata käyttää paikallisesti bittistä AES-256-salausta Tyhjennä sisältö ja lataa se sitten Azuren tallennustilaan johon se on tallennettu salataan muille käyttäjille. Varat suojannut tallennustilan salaus automaattisesti salaamattomana ja sijoitetaan salattuja tiedostojärjestelmässä ennen koodaus ja halutessasi uudelleen salataan ennen lataamista takaisin kuin uuden tulostus-resurssi. Tallennustilan salauksen ensisijainen käyttötapaus on, kun haluat laadukkaita mediatiedostojen suojaaminen salauksesta osoitteessa muille käyttäjille levyn.

Näyttää tallennustilan salattuja resurssi, jotta sinun on määritettävä kohteen toimituksen käytäntö, jotta Media Services tietää, miten haluat pitää sisältöä. Ennen kuin oman resurssi voidaan lähettää, streaming palvelimen poistaa tallennustilaa salaus ja virtauttaa sisällön (esimerkiksi AES, Yleiset salauksen tai salausta) määritetyn toimitus-käytännön avulla.

##<a name="create-contentkeys-used-for-encryption"></a>Luo ContentKeys käytetään salausta

Salatun varat on liitettävä tallennustilan salausavaimen. Sinun on luotava sisällön avaimen, jota käytetään salausta, ennen kuin resurssi-tiedostojen luominen. Tässä osassa kuvataan, miten sisällön avaimen luomiseen.

Seuraavassa on yleisohjeita luonnissa, joihin resurssit, jonka haluat salata liittää sisällön näppäimet. 

1. Tallennustilan salauksen luo satunnaisesti 32-tavuinen AES avain. 

    Tämä on sisällön näppäintä resurssi mikä tarkoittaa, että kaikki tiedostot, jotka liittyvät kyseinen resurssi on käytettävä samaa sisältöä avainta salauksen aikana. 
2.  Soita [GetProtectionKeyId](https://msdn.microsoft.com/library/azure/jj683097.aspx#getprotectionkeyid) ja [GetProtectionKey](https://msdn.microsoft.com/library/azure/jj683097.aspx#getprotectionkey) menetelmiä saat oikeat X.509-varmenne, jota käytetään salaamaan sisällön avain.
3.  Salaa sisällön key-tuotetunnuksen X.509-varmenne julkisella avaimella. 

    Media Services .NET SDK käyttävä RSA OAEP, kun teet salaus.  Voit tarkastella .NET-esimerkki [funktiota EncryptSymmetricKeyData](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.FileEncryption/EncryptionUtils.cs).
4.  Luo tarkistussumman arvo lasketaan avaimen tunnisteen ja sisällön avaimen avulla. Seuraavassa esimerkissä .NET laskee tarkistussumma avaimen tunnisteen ja poista sisällön avain GUID-tunnus-osan avulla.
    

        public static string CalculateChecksum(byte[] contentKey, Guid keyId)
        {
            const int ChecksumLength = 8;
            const int KeyIdLength = 16;

            byte[] encryptedKeyId = null;

            // Checksum is computed by AES-ECB encrypting the KID
            // with the content key.
            using (AesCryptoServiceProvider rijndael = new AesCryptoServiceProvider())
            {
                rijndael.Mode = CipherMode.ECB;
                rijndael.Key = contentKey;
                rijndael.Padding = PaddingMode.None;

                ICryptoTransform encryptor = rijndael.CreateEncryptor();
                encryptedKeyId = new byte[KeyIdLength];
                encryptor.TransformBlock(keyId.ToByteArray(), 0, KeyIdLength, encryptedKeyId, 0);
            }

            byte[] retVal = new byte[ChecksumLength];
            Array.Copy(encryptedKeyId, retVal, ChecksumLength);

            return Convert.ToBase64String(retVal);
        }


5. Luo sisällön avain **EncryptedContentKey** (muunnettu base64-koodatun merkkijonon), **ProtectionKeyId**, **ProtectionKeyType**, **ContentKeyType**ja **tarkistussumma** arvot vastaanottamasi edellä kuvatut vaiheet.

    
    Tallennustilan salauksen seuraavat ominaisuudet otetaan pyynnön tekstissä.
     
    Pyydä leipäteksti-ominaisuus   | Kuvaus
    ---|---
    Tunnus | ContentKey-tunnus, joka on Luo molemmista seuraavassa muodossa "nb:kid:UUID:<NEW GUID>".
    ContentKeyType | Tämä on avaimen sisältötyypin sisällön avaimeen kokonaislukuna. Olemme välittää tallennustilan salauksen arvon 1.
    EncryptedContentKey | Luodaan uusi sisältö avaimen arvon, joka on 256-bittinen (32 Byte Character Set)-arvo. Avain on salattu tallennustilan salaus X.509-varmenne, joka on noutaminen Microsoft Azure Media Services suorittamalla HTTP GET-pyyntö GetProtectionKeyId ja GetProtectionKey menetelmistä. Esimerkkinä, katso seuraava .NET-koodi: **EncryptSymmetricKeyData** menetelmää määritetty [tähän](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.FileEncryption/EncryptionUtils.cs).
    ProtectionKeyId | Tämä on tallennustilan salaus X.509-varmenne, jota käytettiin sekä sisällön avain salataan suojauksen avaimen tunnus.
    ProtectionKeyType | Tämä on salaustyyppi, jota käytettiin sisällön avaimen salaamiseen suojaus-näppäintä. Tämä arvo on StorageEncryption(1) esimerkissä.
    Tarkistussumma |MD5 lasketun tarkistussumman sisällön näppäimen. Se on laskettu salaamalla sisällön tunnus sisältö-näppäimen kanssa. Esimerkkikoodi esitellään, miten tarkistussumman laskemiseen.
    

###<a name="retrieve-the-protectionkeyid"></a>Hae ProtectionKeyId 
 

Seuraavassa esimerkissä esitetään noutamisesta ProtectionKeyId varmenteen allekirjoitusta, sinun on käytettävä sisältö avaimen salaamiseen varmennetta. Tee tämä vaihe ja varmista, että sinulla on jo käytettävän sertifikaatin käyttämääsi laitteeseen.


Pyyntö:
    
    
    GET https://media.windows.net/api/GetProtectionKeyId?contentKeyType=0 HTTP/1.1
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=juliakoams1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423034908&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=7eSLe1GHnxgilr3F2FPCGxdL2%2bwy%2f39XhMPGY9IizfU%3d
    x-ms-version: 2.11
    Host: media.windows.net
    

Vastaus:
    
    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 139
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    request-id: 2b6aa7a4-3a09-4b08-b581-26b55667f817
    x-ms-request-id: 2b6aa7a4-3a09-4b08-b581-26b55667f817
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Wed, 04 Feb 2015 02:42:52 GMT
    
    {"odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Edm.String","value":"7D9BB04D9D0A4A24800CADBFEF232689E048F69C"}

###<a name="retrieve-the-protectionkey-for-the-protectionkeyid"></a>ProtectionKey noutaminen ProtectionKeyId

Seuraavassa esimerkissä esitetään noutamisesta X.509-varmenne ProtectionKeyId käyttämällä olet saanut edellisessä vaiheessa.

Pyyntö:
        
    GET https://media.windows.net/api/GetProtectionKey?ProtectionKeyId='7D9BB04D9D0A4A24800CADBFEF232689E048F69C' HTTP/1.1
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=juliakoams1&urn%3aSubscriptionId=zbbef702-e769-2233-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423141026&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=lDBz5YXKiWe5L7eXOHsLHc9kKEUcUiFJvrNFFSksgkM%3d
    x-ms-version: 2.11
    x-ms-client-request-id: 78d1247a-58d7-40e5-96cc-70ff0dfa7382
    Host: media.windows.net
    


Vastaus:
    
    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 1227
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: 78d1247a-58d7-40e5-96cc-70ff0dfa7382
    request-id: 1523e8f3-8ed2-40fe-8a9a-5d81eb572cc8
    x-ms-request-id: 1523e8f3-8ed2-40fe-8a9a-5d81eb572cc8
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Thu, 05 Feb 2015 07:52:30 GMT
    
    {"odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Edm.String",
    "value":"MIIDSTCCAjGgAwIBAgIQqf92wku/HLJGCbMAU8GEnDANBgkqhkiG9w0BAQQFADAuMSwwKgYDVQQDEyN3YW1zYmx1cmVnMDAxZW5jcnlwdGFsbHNlY3JldHMtY2VydDAeFw0xMjA1MjkwNzAwMDBaFw0zMjA1MjkwNzAwMDBaMC4xLDAqBgNVBAMTI3dhbXNibHVyZWcwMDFlbmNyeXB0YWxsc2VjcmV0cy1jZXJ0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAzR0SEbXefvUjb9wCUfkEiKtGQ5Gc328qFPrhMjSo+YHe0AVviZ9YaxPPb0m1AaaRV4dqWpST2+JtDhLOmGpWmmA60tbATJDdmRzKi2eYAyhhE76MgJgL3myCQLP42jDusWXWSMabui3/tMDQs+zfi1sJ4Ch/lm5EvksYsu6o8sCv29VRwxfDLJPBy2NlbV4GbWz5Qxp2tAmHoROnfaRhwp6WIbquk69tEtu2U50CpPN2goLAqx2PpXAqA+prxCZYGTHqfmFJEKtZHhizVBTFPGS3ncfnQC9QIEwFbPw6E5PO5yNaB68radWsp5uvDg33G1i8IT39GstMW6zaaG7cNQIDAQABo2MwYTBfBgNVHQEEWDBWgBCOGT2hPhsvQioZimw8M+jOoTAwLjEsMCoGA1UEAxMjd2Ftc2JsdXJlZzAwMWVuY3J5cHRhbGxzZWNyZXRzLWNlcnSCEKn/dsJLvxyyRgmzAFPBhJwwDQYJKoZIhvcNAQEEBQADggEBABcrQPma2ekNS3Wc5wGXL/aHyQaQRwFGymnUJ+VR8jVUZaC/U/f6lR98eTlwycjVwRL7D15BfClGEHw66QdHejaViJCjbEIJJ3p2c9fzBKhjLhzB3VVNiLIaH6RSI1bMPd2eddSCqhDIn3VBN605GcYXMzhYp+YA6g9+YMNeS1b+LxX3fqixMQIxSHOLFZ1G/H2xfNawv0VikH3djNui3EKT1w/8aRkUv/AAV0b3rYkP/jA1I0CPn0XFk7STYoiJ3gJoKq9EMXhit+Iwfz0sMkfhWG12/XO+TAWqsK1ZxEjuC9OzrY7pFnNxs4Mu4S8iinehduSpY+9mDd3dHynNwT4="}

### <a name="create-the-content-key"></a>Sisällön avaimen luominen 

Kun olet noutaa X.509-varmenne ja salata sisällön key-tuotetunnuksen, luoda **ContentKey** kohteen ja määrittää sen ominaisuusarvoihin vastaavasti julkisen avaimen avulla.

Yksi arvoista, sinun on määritettävä milloin luominen sisältö avain on tyyppi. Tallennustilan salausta, jos arvo on "1". 

Seuraavassa esimerkissä esitetään, miten voit luoda **ContentKey** **ContentKeyType** omaavien tallennustilan salauksen ("1") ja **ProtectionKeyType** arvoksi "0" osoittaa, että suojaa avaimen tunnus on X.509-varmenne allekirjoitus.  


Pyyntö

    POST https://media.windows.net/api/ContentKeys HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=juliakoams1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423034908&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=7eSLe1GHnxgilr3F2FPCGxdL2%2bwy%2f39XhMPGY9IizfU%3d
    x-ms-version: 2.11
    Host: media.windows.net
    {
    "Name":"ContentKey",
    "ProtectionKeyId":"7D9BB04D9D0A4A24800CADBFEF232689E048F69C", 
    "ContentKeyType":"1", 
    "ProtectionKeyType":"0",
    "EncryptedContentKey":"your encrypted content key",
    "Checksum":"calculated checksum"
    }


Vastaus:
    
    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 777
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Location: https://media.windows.net/api/ContentKeys('nb%3Akid%3AUUID%3A9c8ea9c6-52bd-4232-8a43-8e43d8564a99')
    Server: Microsoft-IIS/8.5
    request-id: 76e85e0f-5cf1-44cb-b689-b3455888682c
    x-ms-request-id: 76e85e0f-5cf1-44cb-b689-b3455888682c
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Wed, 04 Feb 2015 02:37:46 GMT
    
    {"odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#ContentKeys/@Element",
    "Id":"nb:kid:UUID:9c8ea9c6-52bd-4232-8a43-8e43d8564a99","Created":"2015-02-04T02:37:46.9684379Z",
    "LastModified":"2015-02-04T02:37:46.9684379Z",
    "ContentKeyType":1,
    "EncryptedContentKey":"your encrypted content key",
    "Name":"ContentKey",
    "ProtectionKeyId":"7D9BB04D9D0A4A24800CADBFEF232689E048F69C",
    "ProtectionKeyType":0,
    "Checksum":"calculated checksum"}

## <a name="create-an-asset"></a>Sijoituksen luominen

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
    
    {"Name":"BigBuckBunny" "Options":1}


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
       "Options":1,
       "Uri":"https://storagetestaccount001.blob.core.windows.net/asset-9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "StorageAccountName":"storagetestaccount001"
    }
    
##<a name="associate-the-contentkey-with-an-asset"></a>Liitä ContentKey sijoituksen

Kun olet luonut ContentKey, liittää sen oman resurssi $links-toiminnon seuraavan esimerkin mukaisesti:
    
Pyyntö:
    
    POST https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3Afbd7ce05-1087-401b-aaae-29f16383c801')/$links/ContentKeys HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Content-Type: application/json
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=juliakoams1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423141026&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=lDBz5YXKiWe5L7eXOHsLHc9kKEUcUiFJvrNFFSksgkM%3d
    x-ms-version: 2.11
    Host: media.windows.net

    
    {"uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/ContentKeys('nb%3Akid%3AUUID%3A01e6ea36-2285-4562-91f1-82c45736047c')"}

Vastaus:

    HTTP/1.1 204 No Content 

##<a name="create-an-assetfile"></a>Luo AssetFile

[AssetFile](http://msdn.microsoft.com/library/azure/hh974275.aspx) kohteen edustaa video- tai tiedosto, joka on tallennettu blob-säilö. Resurssi-tiedosto on aina liitetty amerikkalaisen ja amerikkalaisen voi olla yksi tai usea resurssi-tiedostoja. Media-palveluiden Encoder tehtävän epäonnistuu, jos objektin resurssi tiedosto ei ole liitetty digitaalinen tiedoston blob-säilö.

Huomaa, että **AssetFile** esiintymän ja todellinen mediatiedosto ovat kaksi erillistä objektia. AssetFile esiintymää on metatietoa mediatiedosto, mediatiedosto on todellinen mediasisältöä.

Kun olet ladannut digitaalisen mediatiedoston blob säilöön, Päivitä AssetFile tietoja siitä, että media-tiedosto (ei näy tässä ohjeaiheessa) **Yhdistä** HTTP-pyynnön avulla. 

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
       "IsEncrypted":"true",
       "EncryptionScheme" : "StorageEncryption", 
       "EncryptionVersion" : "1.0",       
       "EncryptionKeyId" : "nb:kid:UUID:32e6efaf-5fba-4538-b115-9d1cefe43510",
       "InitializationVector" : "397304628502661816</d:InitializationVector",
       "Options":0,
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
       "EncryptionVersion": "1.0",
       "EncryptionScheme": "StorageEncryption",
       "IsEncrypted":true,
       "EncryptionKeyId":"nb:kid:UUID:32e6efaf-5fba-4538-b115-9d1cefe43510",
       "InitializationVector":"397304628502661816</d:InitializationVector",
       "IsPrimary":false,
       "LastModified":"2015-01-19T00:34:08.1934137Z",
       "Created":"2015-01-19T00:34:08.1934137Z",
       "MimeType":"video/mp4",
       "ContentChecksum":null
    }
