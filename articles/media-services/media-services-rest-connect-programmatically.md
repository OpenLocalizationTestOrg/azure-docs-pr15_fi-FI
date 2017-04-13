<properties 
    pageTitle="Yhteyden muodostaminen Media Services-tilin REST API | Microsoft Azure" 
    description="Tässä ohjeaiheessa kerrotaan, miten Media Services uisng REST API yhdistäminen." 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="09/26/2016"  
    ms.author="juliako"/>


# <a name="connecting-to-media-services-account-using-media-services-rest-api"></a>Yhteyden muodostaminen Media Services-tilin Media Services REST-Ohjelmointirajapinnalla

> [AZURE.SELECTOR]
- [.NET](media-services-dotnet-connect-programmatically.md)
- [MUILLE KÄYTTÄJILLE](media-services-rest-connect-programmatically.md)

Tässä ohjeaiheessa kerrotaan ohjelmallisesti yhteyden muodostaminen Microsoft Azure Media Services hankkiminen, kun ovat ohjelmointi Media Services REST-ohjelmointirajapinnalla.

Kaksi asiaa tarvitaan Microsoft Azure Media Services käytettäessä: annettu Azure Access ohjausobjektin Services (ACS)-ja Media-palveluiden URI itse access-tunnus. Voit käyttää muulla tavoin haluat luotaessa nämä pyynnöt, kunhan määrittää Otsikkoarvot, oikea ja siirtää käyttöoikeustietue-oikein soitettaessa Media Services.

Seuraavissa vaiheissa kuvataan yleisimmät työnkulku, kun Media-palveluiden yhdistäminen Media Services REST-Ohjelmointirajapinnalla avulla:

1. Access-tunnuksen saaminen 
2. Yhteyden muodostaminen Media-palveluiden URI 

    >[AZURE.NOTE] Kun https://media.windows.net muodostettu yhteys, saat 301 uudelleenohjaus, määrittämällä toisen Media palveluiden URI-tunnus. Sinun on tehtävä seuraavat suosikkiryhmän puhelut uusi URI-osoitteeseen.
Näyttöön saattaa tulla HTTP/1.1 200 vastauksen, joka sisältää ODATA-Ohjelmointirajapinnan metatietojen kuvaus.

3. Kirjaa myöhemmin API puhelut uusi URL-osoite. 

    Jos jälkeen yrität muodostaa yhteyttä odottamasi esimerkiksi seuraavasti:

        HTTP/1.1 301 Moved Permanently
        Location: https://wamsbayclus001rest-hs.cloudapp.net/api/

    Pitäisi lähettää myöhemmin API puhelut https://wamsbayclus001rest-hs.cloudapp.net/api/.

##<a name="access-control-address"></a>Accessin osoitteen

Media-palveluiden käyttö ohjausobjektin osoite on https://wamsprodglobal001acs.accesscontrol.windows.net Pohjois Kiinan alue, johon se on https://wamsprodglobal001acs.accesscontrol.chinacloudapi.cn lukuun ottamatta.

##<a name="getting-an-access-token"></a>Access-tunnuksen saaminen

Voit käyttää Media Services suoraan REST-Ohjelmointirajapinta, access-tunnuksen noutaa ACS ja käyttää sitä jokaisen palvelun tehdä HTTP-pyynnön aikana. Tunnuksessa on samanlainen kuin muiden tunnusten myöntämä ACS access vaateita, jotka on annettu HTTP-pyynnön ja OAuth v2-protokollan avulla otsikon perusteella. Muut edellytykset ei tarvitse ennen yhteyden muodostamista suoraan Media-palveluita.

Seuraavassa esimerkissä HTTP pyynnön ylä- ja leipätekstin tunnusta hakemiseen käytetty.

**Otsikko**:

    POST https://wamsprodglobal001acs.accesscontrol.windows.net/v2/OAuth2-13 HTTP/1.1
    Content-Type: application/x-www-form-urlencoded
    Host: wamsprodglobal001acs.accesscontrol.windows.net
    Content-Length: 120
    Expect: 100-continue
    Connection: Keep-Alive
    Accept: application/json

    
**Leipäteksti**:

Sinun täytyy todista jotakin tämän lisäajan, client_id ja client_secret arvot client_id ja client_secret vastaavat AccountName ja AccountKey-arvot. Nämä arvot toimitetaan sinulle Media-palveluissa, kun määrität tiliä. 

Huomaa, että AccountKey Media Services-tilin on oltava URL-koodatun (katso [URL-koodausta](http://tools.ietf.org/html/rfc3986#section-2.1) , kun käytät access-tunnuksen pyynnön client_secret arvona.

    grant_type=client_credentials&client_id=ams_account_name&client_secret=URL_encoded_ams_account_key&scope=urn%3aWindowsAzureMediaServices


Esimerkki: 

    grant_type=client_credentials&client_id=amstestaccount001&client_secret=wUNbKhNj07oqjqU3Ah9R9f4kqTJ9avPpfe6Pk3YZ7ng%3d&scope=urn%3aWindowsAzureMediaServices


Seuraavassa esimerkissä esitetään, joka sisältää access HTTP-vastaus suojaustunnuksen vastauksen tekstissä.

    HTTP/1.1 200 OK
    Cache-Control: no-cache, no-store
    Pragma: no-cache
    Content-Type: application/json; charset=utf-8
    Expires: -1
    request-id: a65150f5-2784-4a01-a4b7-33456187ad83
    X-Content-Type-Options: nosniff
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Thu, 15 Jan 2015 08:07:20 GMT
    Content-Length: 670
    
    {  
       "token_type":"http://schemas.xmlsoap.org/ws/2009/11/swt-token-profile-1.0",
       "access_token":"http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f19258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421330840&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=uf69n82KlqZmkJDNxhJkOxpyIpA2HDyeGUTtSnq1vlE%3d",
       "expires_in":"21600",
       "scope":"urn:WindowsAzureMediaServices"
    }
    

>[AZURE.NOTE]
On suositeltavaa välimuistiin ulkoisen tallennustilan "access_token" ja "expires_in" arvot. Suojaustunnuksen tiedot myöhemmin voidaan noutaa tallennustilaa ja käyttää uudelleen Media Services REST API-puheluissa. Tämä on erityisen hyödyllisiä skenaariot, jossa tunnuksen voidaan turvallisesti jakaa useita prosesseja tai tietokoneiden välillä.

Varmista, että access-tunnuksen "expires_in" arvosta ja Päivitä REST API-puhelut uusi tunnusten tarpeen mukaan.

###<a name="connecting-to-the-media-services-uri"></a>Yhteyden muodostaminen Media-palveluiden URI

Pääkansio URI Media Services on https://media.windows.net/. Alun perin Muodosta yhteys tämän URI ja jos 301 uudelleenohjaus takaisin vastauksena, varmista seuraavat suosikkiryhmän puhelut uusi URI-osoitteeseen. Lisäksi-pyyntöjen käyttää ei mitään automaattinen uudelleenohjaus/seuraaminen logiikan. HTTP-käyttö ja laitokset pyyntö ei siirtää uuteen URI.

Huomaa, että pääkansio URI lataamisesta ja kohteiden tiedostojen lataaminen ei https://yourstorageaccount.blob.core.windows.net/ tallennustilan tilin nimi on Media Services-tilin määrittäminen käytettyä sama.

Seuraavassa esimerkissä HTTP-pyyntö Media Services pääkansio URI (https://media.windows.net/). Pyynnön saa 301 uudelleenohjaus takaisin vastaus. Seuraavien pyynnön käyttää uuteen URI (https://wamsbayclus001rest-hs.cloudapp.net/api/).     

**HTTP-pyyntö**:
    
    GET https://media.windows.net/ HTTP/1.1
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f19258-6753-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421500579&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=ElVWXOnMVggFQl%2ft9vhdcv1qH1n%2fE8l3hRef4zPmrzg%3d
    x-ms-version: 2.11
    Accept: application/json
    Host: media.windows.net


**HTTP-vastaus**:
    
    HTTP/1.1 301 Moved Permanently
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/
    Server: Microsoft-IIS/8.5
    request-id: 987d7652-497a-44e5-b815-f492e02aef97
    x-ms-request-id: 987d7652-497a-44e5-b815-f492e02aef97
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sat, 17 Jan 2015 07:44:53 GMT
    Content-Length: 164
    
    <html><head><title>Object moved</title></head><body>
    <h2>Object moved to <a href="https://wamsbayclus001rest-hs.cloudapp.net/api/">here</a>.</h2>
    </body></html>


**HTTP-pyyntö** (joko uuden URI):
            
    GET https://wamsbayclus001rest-hs.cloudapp.net/api/ HTTP/1.1
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f19258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421500579&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=ElVWXOnMVggFQl%2ft9vhdcv1qH1n%2fE8l3hRef4zPmrzg%3d
    x-ms-version: 2.11
    Accept: application/json
    Host: wamsbayclus001rest-hs.cloudapp.net


**HTTP-vastaus**:
    
    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 1250
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    request-id: 5f52ea9d-177e-48fe-9709-24953d19f84a
    x-ms-request-id: 5f52ea9d-177e-48fe-9709-24953d19f84a
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sat, 17 Jan 2015 07:44:52 GMT
    
    {"odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata","value":[{"name":"AccessPolicies","url":"AccessPolicies"},{"name":"Locators","url":"Locators"},{"name":"ContentKeys","url":"ContentKeys"},{"name":"ContentKeyAuthorizationPolicyOptions","url":"ContentKeyAuthorizationPolicyOptions"},{"name":"ContentKeyAuthorizationPolicies","url":"ContentKeyAuthorizationPolicies"},{"name":"Files","url":"Files"},{"name":"Assets","url":"Assets"},{"name":"AssetDeliveryPolicies","url":"AssetDeliveryPolicies"},{"name":"IngestManifestFiles","url":"IngestManifestFiles"},{"name":"IngestManifestAssets","url":"IngestManifestAssets"},{"name":"IngestManifests","url":"IngestManifests"},{"name":"StorageAccounts","url":"StorageAccounts"},{"name":"Tasks","url":"Tasks"},{"name":"NotificationEndPoints","url":"NotificationEndPoints"},{"name":"Jobs","url":"Jobs"},{"name":"TaskTemplates","url":"TaskTemplates"},{"name":"JobTemplates","url":"JobTemplates"},{"name":"MediaProcessors","url":"MediaProcessors"},{"name":"EncodingReservedUnitTypes","url":"EncodingReservedUnitTypes"},{"name":"Operations","url":"Operations"},{"name":"StreamingEndpoints","url":"StreamingEndpoints"},{"name":"Channels","url":"Channels"},{"name":"Programs","url":"Programs"}]}
     


>[AZURE.NOTE] Kun saat uuden URI, eli URI, jota käytetään Media-palveluiden kanssa kommunikoimiseen. 


##<a name="media-services-learning-paths"></a>Media-palveluiden oppimispolut

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Palautteen antaminen

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
