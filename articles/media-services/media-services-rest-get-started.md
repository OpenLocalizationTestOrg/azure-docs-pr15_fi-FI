<properties 
    pageTitle="Aloita välittää sisällön käyttäminen muiden pyydettäessä | Microsoft Azure" 
    description="Tässä opetusohjelmassa käydään läpi vaiheet käyttöönoton sovelluksen on demand-sisällön toimittamisen käyttämällä REST API Azure Media-palvelujen kanssa." 
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
    ms.date="10/11/2016" 
    ms.author="juliako"/>

#<a name="get-started-with-delivering-content-on-demand-using-rest"></a>Välittää sisällön käyttäminen muiden pyydettäessä käytön aloittaminen 

[AZURE.INCLUDE [media-services-selector-get-started](../../includes/media-services-selector-get-started.md)]


>[AZURE.NOTE]
> Jotta voit suorittaa tässä opetusohjelmassa, sinun on Azure-tili. Lisätietoja on artikkelissa [Azure maksuttoman kokeiluversion](/pricing/free-trial/?WT.mc_id=A261C142F). 


Tämä pikaopas käydään läpi vaiheet käyttöönoton videotilauspalveluiden (VoD) sisällön toimitus-sovellus, joka käyttää Azure Media Services (AMS) REST API. 

Opetusohjelman esitellään basic Media Services-työnkulku ja yleisimmät ohjelmoinnin objektit ja Media-palveluiden kehittämiseen tarvittavat tehtävät. Opetusohjelman päätyttyä osaat käyttää tai Lataa asteittain, joka on ladattu, koodattu ja ladata media mallitiedosto.  

## <a name="prerequisites"></a>Edellytykset
Seuraavat edellytykset kehittämisen REST API Media-palvelun käynnistäminen edellyttää.

- Tietoja siitä, miten voit tehdä Media Services REST-ohjelmointirajapinnalla. Lisätietoja on artikkelissa [media-palvelut-rest-yleiskatsaus](http://msdn.microsoft.com/library/azure/hh973616.aspx).
- Sovelluksen valittua, voit lähettää HTTP-pyynnöt ja vastaukset. Tässä opetusohjelmassa käytetään [Fiddler](http://www.telerik.com/download/fiddler). 

Tämä pikaopas näkyvät seuraavat toimet.

1.  Luo Media Services-tilin, (joko Azure portaalin).
2.  Määritä streaming päätepisteen (joko Azure portaalin).
1.  Muodosta yhteys REST-ohjelmointirajapinnalla Media Services-tilin.
1.  Luo uusi resurssi ja lataa videotiedoston REST-ohjelmointirajapinnalla.
1.  Määritä streaming yksiköt REST-ohjelmointirajapinnalla.
2.  Koodata lähdetiedosto mukautuvat bittitaajuus MP4-tiedostot REST-ohjelmointirajapinnalla joukkoon.
1.  Julkaise resurssi ja Hae streaming ja REST-ohjelmointirajapinnalla asteittain Lataa URL-osoitteet. 
1.  Toistaa sisältöä. 

## <a name="create-an-azure-media-services-account-using-the-azure-portal"></a>Azure-portaalissa Azure Media Services-tilin luominen

Tämän osan vaiheet näyttää, miten voit luoda AMS tilin.

1. Kirjaudu sisään osoitteessa [Azure portal](https://portal.azure.com/).
2. Valitse **+ Uusi** > **Media + CDN** > **Media-palveluita**.

    ![Media-palveluiden luominen](./media/media-services-portal-vod-get-started/media-services-new1.png)

3. Kirjoita **Luo MEDIA SERVICES-tilin** pakolliset arvot.

    ![Media-palveluiden luominen](./media/media-services-portal-vod-get-started/media-services-new3.png)
    
    1. Kirjoita **Tilin nimi**-kenttään uuden AMS tilin nimi. Media Services-tilin nimi on kaikki kirjaimiksi numerot tai kirjaimet ilman välilyöntejä, ja 3-24 merkkiä.
    2. Valitse tilauksen kesken eri Azure-tilauksissa, joihin sinulla on pääsy.
    
    2. **Resurssiryhmä**Valitse uusi tai aiemmin luotu resurssi.  Resurssiryhmä on kokoelma resursseja, jotka jakavat elinkaari, käyttöoikeudet ja käytännöt. Lisätietoja [tästä](azure-resource-manager/resource-group-overview.md#resource-groups).
    3. **Sijainti**Valitse maantieteellisen alueen käytetään tallentaa Media Services-tilin media ja metatiedot-tietueita. Tällä alueella käytetään Käsittele ja median lisääminen. Vain käytettävissä Media Services alueet näkyvät avattavan luetteloruudun. 
    
    3. **Tallennustilan tilin**Valitse tallennustilan tili antamaan Blob-objektien tallennustilaan mediasisällön Media Services-tililtä. Voit valita olemassa olevan tallennustilan tilin saman maantieteellisen alueen Media Services-tiliksi tai voit luoda tallennustilan tilin. Uusi tallennustilan-tili on luotu samalla alueella. Tallennustilan tilin nimi säännöt ovat samat kuin Media Services tilit.

        Lisätietoja tallennustilan [tähän](storage-introduction.md).

    4. Valitse **raporttinäkymät-ikkunan kiinnittäminen** Nähdäksesi tilin käyttöönoton etenemisen.
    
7. Valitse lomakkeen alareunassa **luominen** .

    Kun tili on luotu, tilaksi muuttuu **käynnissä**. 

    ![Media-palveluiden asetukset](./media/media-services-portal-vod-get-started/media-services-settings.png)

    Voit hallita tiliäsi AMS (esimerkiksi videoiden lataaminen, koodata varat, työn edistymistä) **asetukset** -ikkunaa.

## <a name="configure-streaming-endpoints-using-the-azure-portal"></a>Määritä streaming päätepisteet Azure-portaalissa

Kun käsittelet jokin tilanteissa toimittaa video mukautuvat bittitaajuus streaming kautta asiakkaat Azure Media-palveluita. Media-palveluiden tukee seuraavia mukautuvat bittitaajuus streaming tekniikoita: HTTP Live Streaming (HLS), tasainen Streaming, MPEG VIIVA ja HDS (varten vain Adobe PrimeTime/Access asiakirjamalleja).

Media-palvelut on dynaaminen pakkaus, jonka avulla voit pitää mukautuvat bittitaajuus MP4 koodattu sisällön streaming tukemat Media Services (MPEG VIIVA, HLS, tasainen Streaming, HDS) juuri perustuvan, mukaan eikä sinun tarvitse tallentaa valmiiksi pakattu versioiden kaikkien näiden streaming muotoja.

Jos haluat hyödyntää Dynaaminen pakkaus, seuraavasti:

- Koodata mezzanine (lähde)-tiedoston tuominen mukautuvat bittitaajuus MP4-tiedostot (koodauksen vaiheet ovat esitellään jäljempänä tässä opetusohjelmassa).  
- Luo vähintään yksi streaming yksikkö *tietovirta päätepisteen* , joista aiot toimituksen sisältöä. Alla olevia ohjeita noudattamalla streaming yksiköiden määrän muuttaminen.

Dynaaminen pakkaus, jossa tarvitset vain tallentamiseen ja maksaa yhden Tallennusmuoto tiedostoja ja Media-palveluiden muodostaa ja on oikea vastaus asiakaskoneesta pyyntöjen perusteella.

Voit luoda ja muuttaa streaming varattu yksiköiden määrän, toimi seuraavasti:


1. Valitse **asetukset** -ikkunassa **Virtautetun median päätepisteet**. 

2. Valitse streaming päätepisteen oletusarvo. 

    **Oletus-TIETOVIRTA PÄÄTEPISTEEN tiedot** -ikkuna.

3. Jos haluat määrittää streaming yksiköiden määrän, **Virtautetun median yksiköt** liukusäädintä.

    ![Streaming yksiköt](./media/media-services-portal-vod-get-started/media-services-streaming-units.png)

4. Tallenna muutokset valitsemalla **Tallenna** .

    >[AZURE.NOTE]Kaikki uudet yksiköt kohdistus voi kestää jopa 20 minuuttia.

## <a id="connect"></a>Yhteyden muodostaminen Media Services-tilin REST-ohjelmointirajapinnalla

Kaksi asiaa tarvittavat Azure Media-palveluita käyttäessään: annettu Azure Access ohjausobjektin Services (ACS)-ja Media-palveluiden URI itse access-tunnus. Voit käyttää muulla tavoin haluat luotaessa nämä pyynnöt, kunhan määrittää Otsikkoarvot, oikea ja siirtää käyttöoikeustietue-oikein soitettaessa Media Services.

Seuraavissa vaiheissa kuvataan yleisimmät työnkulku, kun Media-palveluiden yhdistäminen Media Services REST-Ohjelmointirajapinnalla avulla:

1. Hakeminen access-tunnuksen. 
2. Yhteyden muodostaminen Media-palveluiden URI.  

    Muista, että kun muodostettu yhteys https://media.windows.net, näyttöön tulee 301 uudelleenohjaus, määrittämällä toisen Media palveluiden URI-tunnus. Sinun on tehtävä seuraavat suosikkiryhmän puhelut uusi URI-osoitteeseen. Näyttöön saattaa tulla HTTP/1.1 200 vastauksen, joka sisältää ODATA-Ohjelmointirajapinnan metatietojen kuvaus.
3. Kirjaaminen myöhemmissä API puhelut uusi URL-osoite. 
    
    Jos jälkeen yrität muodostaa yhteyttä odottamasi esimerkiksi seuraavasti:
        
        HTTP/1.1 301 Moved Permanently
        Location: https://wamsbayclus001rest-hs.cloudapp.net/api/

    Pitäisi lähettää myöhemmin API puhelut https://wamsbayclus001rest-hs.cloudapp.net/api/.

###<a name="getting-an-access-token"></a>Access-tunnuksen saaminen

Voit käyttää Media Services suoraan REST-Ohjelmointirajapinta, access-tunnuksen noutaa ACS ja käyttää sitä jokaisen palvelun tehdä HTTP-pyynnön aikana. Muut edellytykset ei tarvitse ennen yhteyden muodostamista suoraan Media-palveluita.

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

Sinun on osoittanut jotakin tämän lisäajan, client_id ja client_secret arvot client_id ja client_secret vastaavat AccountName ja AccountKey-arvot. Nämä arvot toimitetaan sinulle Media-palveluissa, kun määrität tiliä. 

AccountKey Media Services-tilin on oltava URL-koodatun, kun käytät access-tunnuksen pyynnön client_secret arvona.

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
       "access_token":"http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421330840&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=uf69n82KlqZmkJDNxhJkOxpyIpA2HDyeGUTtSnq1vlE%3d",
       "expires_in":"21600",
       "scope":"urn:WindowsAzureMediaServices"
    }
    

>[AZURE.NOTE]
On suositeltavaa välimuistiin ulkoisen tallennustilan "access_token" ja "expires_in" arvot. Suojaustunnuksen tiedot myöhemmin voidaan noutaa tallennustilaa ja käyttää uudelleen Media Services REST API-puheluissa. Tämä on erityisen hyödyllisiä skenaariot, jossa tunnuksen voidaan turvallisesti jakaa useita prosesseja tai tietokoneiden välillä.

Varmista, että access-tunnuksen "expires_in" arvosta ja Päivitä REST API-puhelut uusi tunnusten tarpeen mukaan.

###<a name="connecting-to-the-media-services-uri"></a>Yhteyden muodostaminen Media-palveluiden URI

Pääkansio URI Media Services on https://media.windows.net/. Alun perin Muodosta yhteys tämän URI ja jos 301 uudelleenohjaus takaisin vastauksena, varmista seuraavat suosikkiryhmän puhelut uusi URI-osoitteeseen. Lisäksi-pyyntöjen käyttää ei mitään automaattinen uudelleenohjaus/seuraaminen logiikan. HTTP-käyttö ja laitokset pyyntö ei siirtää uuteen URI.

Pääkansio URI lataamisesta ja kohteiden tiedostojen lataaminen on https://yourstorageaccount.blob.core.windows.net/ tallennustilan tilin nimi on Media Services-tilin määrittäminen käytettyä sama.

Seuraavassa esimerkissä HTTP-pyyntö Media Services pääkansio URI (https://media.windows.net/). Pyynnön saa 301 uudelleenohjaus takaisin vastaus. Seuraavien pyynnön käyttää uuteen URI (https://wamsbayclus001rest-hs.cloudapp.net/api/).     

**HTTP-pyyntö**:
    
    GET https://media.windows.net/ HTTP/1.1
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421500579&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=ElVWXOnMVggFQl%2ft9vhdcv1qH1n%2fE8l3hRef4zPmrzg%3d
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
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421500579&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=ElVWXOnMVggFQl%2ft9vhdcv1qH1n%2fE8l3hRef4zPmrzg%3d
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
     


>[AZURE.NOTE] Tästä lähtien valitsemalla Uusi URI käytetään tässä opetusohjelmassa.

## <a id="upload"></a>Luo uusi resurssi ja lataa videotiedoston REST-ohjelmointirajapinnalla

Media-palveluja Lataa digitaalisten tiedostojen sijoituksen kyselyjä. **Kohteiden** kohde voi olla video, ääni, kuvia, pikkukuvan sivustokokoelmat, tekstin raidat tekstitys tiedostot (ja nämä tiedostot metatietoa.)  Kun tiedostot on ladattu palvelimeen kyselyjä kohteen, sisältöä tallennetaan Pilvipalvelun myöhempää käsittelyä ja streaming suojatusti. 

Arvot, jotka sinun on annettava sijoituksen luotaessa on resurssi luonnin asetukset. **Asetukset** -ominaisuus on numerointiarvo, joka kuvaa amerikkalaisen voidaan luoda haluamasi salausasetukset. Virheellinen arvo on jotakin arvoa ei ole tässä luettelossa arvojen yhdistelmän alla olevasta luettelosta:

 
- **Ei mitään** = **0** - salausta käytetään. Kun tämä vaihtoehto sisältöä ei ole suojattu siirrossa tai muiden tallennustilaan.
    Jos haluat pitää MP4 käyttämällä asteittain lataaminen, käytä tätä vaihtoehtoa. 
- **StorageEncrypted** = **1** - salaa käyttää paikallisesti bittistä AES-256-salausta Tyhjennä sisältö ja lataa se sitten Azuren tallennustilaan johon se on tallennettu salataan muille käyttäjille. Varat suojannut tallennustilan salaus automaattisesti salaamattomana ja sijoitetaan salattuja tiedostojärjestelmässä ennen koodaus ja halutessasi uudelleen salataan ennen lataamista takaisin kuin uuden tulostus-resurssi. Tallennustilan salauksen ensisijainen käyttötapaus on, kun haluat suojata laadukkaita mediatiedostojen salauksesta osoitteessa muille käyttäjille levyn.
- **CommonEncryptionProtected** = **2** – Käytä tätä vaihtoehtoa, jos lataat sisältöä, joka on jo salattu ja suojattu Yleiset-salausta tai PlayReady DRM (esimerkiksi tasainen Streaming suojattu with PlayReady DRM).
- **EnvelopeEncryptionProtected** = **4** – Käytä tätä vaihtoehtoa, jos lataat HLS AES salattu. Tiedostot on on koodattu ja salattu Transform hallinta.

### <a name="create-an-asset"></a>Sijoituksen luominen

Sijoituksen on useita tiedostotyypit ja Media-palveluja, kuten video, ääni, kuvia, pikkukuvan sivustokokoelmat, tekstin raidat ja tekstitys tiedostot objektien tietojoukkoa säilö. REST API-liittymän amerikkalaisen luominen edellyttää POST-pyynnön lähettäminen Media Services ja lisäämällä ominaisuutta koskevia tietoja oman resurssi pyynnön tekstissä.

Seuraavassa esimerkissä esitetään, miten voit luoda sijoituksen.

**HTTP-pyyntö**

    POST https://wamsbayclus001rest-hs.cloudapp.net/api/Assets HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    x-ms-client-request-id: c59de965-bc89-4295-9a57-75d897e5221e
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 45
    
    {"Name":"BigBuckBunny.mp4", "Options":"0"}
    

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
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Sun, 18 Jan 2015 22:06:40 GMT
        
    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Assets/@Element",
       "Id":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "State":0,
       "Created":"2015-01-18T22:06:40.6010903Z",
       "LastModified":"2015-01-18T22:06:40.6010903Z",
       "AlternateId":null,
       "Name":"BigBuckBunny2.mp4",
       "Options":0,
       "Uri":"https://storagetestaccount001.blob.core.windows.net/asset-9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1",
       "StorageAccountName":"storagetestaccount001"
    }
    
### <a name="create-an-assetfile"></a>Luo AssetFile

[AssetFile](http://msdn.microsoft.com/library/azure/hh974275.aspx) kohteen edustaa video- tai tiedosto, joka on tallennettu blob-säilö. Resurssi-tiedosto on aina liitetty sijoituksen ja sijoituksen voi olla yksi tai usea AssetFiles. Media-palveluiden Encoder tehtävän epäonnistuu, jos objektin resurssi tiedosto ei ole liitetty digitaalinen tiedoston blob-säilö.

Kun olet ladannut digitaalisen mediatiedoston blob säilöön, käytät **Yhdistä** HTTP-pyyntö Päivitä tiedot AssetFile tietoja media-tiedosto (kuten artikkelin). 

**HTTP-pyyntö**

    POST https://wamsbayclus001rest-hs.cloudapp.net/api/Files HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
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

Ennen lataamista tiedostoista, jotka Blob-objektien tallennustilaan kyselyjä, Määritä käytäntö oikeudet sijoituksen kirjoittaminen access. Saadakseen ne kirjaa AccessPolicies kohde-Määritä HTTP-pyynnön. Määritä DurationInMinutes arvon luotaessa tai 500 Sisäinen palvelinvirhe-virhesanoma tulee näyttöön vastauksen takaisin sisään. Saat lisätietoja AccessPolicies [AccessPolicy](http://msdn.microsoft.com/library/azure/hh974297.aspx).

Seuraavassa esimerkissä AccessPolicy luominen:
        
**HTTP-pyyntö**

    POST https://wamsbayclus001rest-hs.cloudapp.net/api/AccessPolicies HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 74
    
    {"Name":"NewUploadPolicy", "DurationInMinutes":"440", "Permissions":"2"} 

**HTTP-vastaus**

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
    X-Powered-By: ASP.NET
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

### <a name="get-the-upload-url"></a>Lataa URL-Osoitteen hankkiminen

Vastaanottamaan todellinen lataus-URL SAS Locator luominen Locators ohjelmat, joita haluat käyttää sijoituksen tiedostot Määritä alkamis- ja yhteyden endpoint tyyppi. Voit luoda useita Locator kohteet annetun AccessPolicy ja kohteiden pari käsittelemään toisen asiakkaan pyynnöt ja tarpeisiin. Jokainen nämä Locators käyttää aloitusajan arvo plus AccessPolicy DurationInMinutes arvo määrittää URL-Osoitteen avulla voidaan pituutta. Lisätietoja on artikkelissa [Locator](http://msdn.microsoft.com/library/azure/hh974308.aspx).


SAS URL-osoite on seuraavanlainen:

    {https://myaccount.blob.core.windows.net}/{asset name}/{video file name}?{SAS signature}

Käytä seikkoja:

- Ei voi olla enintään viisi yksilöllinen Locators liittyvät annetulle kohteiden samalla kertaa. Lisätietoja on artikkelissa Locator.
- Jos haluat ladata tiedostoja heti, sinun kannattaa asettaa aloitusajan arvo 5 minuutin ajan ennen nykyisen kellonajan. Tämä johtuu siitä voi olla kellon jakauman.vinous asiakaskoneeseen ja Media-palveluiden välillä. Lisäksi aloitusajan arvo on oltava DateTime-muodossa: YYYY-MM-DDTHH:mm:ssZ (esimerkiksi "2014-05-23T17:53:50Z").   
- Ehkä 30 – 40 sekunnin viive jälkeen Locator luodaan kun se on käytettävissä. Tämä ongelma koskee sekä SAS URL-osoite ja Origin Locators.

Seuraavassa esimerkissä luomisesta SAS URL-Locator määrittämä tyyppi-ominaisuuden pyyntö tekstissä ("1", SAS locator) ja "2" edelleen tarvittaessa-origin locator. Palauttaa **Path** -ominaisuus on URL-osoite, sinun on käytettävä Lataa tiedosto.
    
**HTTP-pyyntö**
    
    POST https://wamsbayclus001rest-hs.cloudapp.net/api/Locators HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=f7f09258-6753-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421640053&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=vlG%2fPYdFDMS1zKc36qcFVWnaNh07UCkhYj3B71%2fk1YA%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 178
    
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
    
    MERGE https://wamsbayclus001rest-hs.cloudapp.net/api/Files('nb%3Acid%3AUUID%3Af13a0137-0a62-9d4c-b3b9-ca944b5142c5') HTTP/1.1
    Content-Type: application/json
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421662918&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=utmoXXbm9Q7j4tW1yJuMVA3egRiQy5FPygwadkmPeaY%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
    
    {  
       "ContentFileSize":"1186540",
       "Id":"nb:cid:UUID:f13a0137-0a62-9d4c-b3b9-ca944b5142c5",
       "MimeType":"video/mp4",
       "Name":"BigBuckBunny.mp4",
       "ParentAssetId":"nb:cid:UUID:9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1"
    }


**HTTP-vastaus**

Jos onnistuu, seuraava kaava palauttaa: HTTP/1.1 204 ei sisältöä

## <a name="delete-the-locator-and-accesspolicy"></a>Poista Locator ja AccessPolicy 

**HTTP-pyyntö**


    DELETE https://wamsbayclus001rest-hs.cloudapp.net/api/Locators('nb%3Alid%3AUUID%3Aaf57bdd8-6751-4e84-b403-f3c140444b54') HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421662918&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=utmoXXbm9Q7j4tW1yJuMVA3egRiQy5FPygwadkmPeaY%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net

    
**HTTP-vastaus**

Jos onnistuu, palautetaan seuraavasti:

    HTTP/1.1 204 No Content 
    ...

**HTTP-pyyntö**

    DELETE https://wamsbayclus001rest-hs.cloudapp.net/api/AccessPolicies('nb%3Apid%3AUUID%3Abe0ac48d-af7d-4877-9d60-1805d68bffae') HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421662918&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=utmoXXbm9Q7j4tW1yJuMVA3egRiQy5FPygwadkmPeaY%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net

**HTTP-vastaus**

Jos onnistuu, palautetaan seuraavasti:

    HTTP/1.1 204 No Content 
    ...

 
## <a id="configure_streaming_units"></a>Määritä streaming yksiköt REST-ohjelmointirajapinnalla

Kun käsittelet jokin tilanteissa toimittaa mukautuvat bittitaajuus streaming asiakkaiden Azure Media-palveluita. Mukautuvat bittitaajuus tietovirta, jossa asiakkaan voit vaihtaa ylemmäs tai alemmas bittitaajuus muodossa kuin videon näkyy nykyinen kaistanleveys, suorittimen käyttö ja muiden tekijöiden perusteella. Media-palveluiden tukee seuraavia mukautuvat bittitaajuus streaming tekniikoita: HTTP Live Streaming (HLS), tasainen Streaming, MPEG VIIVA ja HDS (varten vain Adobe PrimeTime/Access asiakirjamalleja). 

Media-palvelut on dynaaminen pakkaus, jonka avulla voit pitää mukautuvat bittitaajuus MP4 tai tasainen Streaming koodattu sisältölähdettä streaming tukemat Media-palveluissa (MPEG VIIVA, HLS, tasainen Streaming, HDS) ilman, että sinun tarvitsee pakata huomioon nämä streaming muotoja. 

Jos haluat hyödyntää Dynaaminen pakkaus, seuraavasti:

- vähintään yksi streaming yksikkö **tietovirta päätepisteen **, joista voit taata aikana (Tässä osiossa esitettyjä) sisällön hakeminen
- koodata tai muuttaa oman mezzanine (lähde) tiedoston säädön bittitaajuus MP4-tiedostoja tai säädön bittitaajuus tasainen Streaming-tiedostoja (koodauksen vaiheet ovat esitellään jäljempänä tässä opetusohjelmassa), joukkoon  

Dynaaminen pakkaus, jossa tarvitset vain tallentamiseen ja maksaa yhden Tallennusmuoto tiedostoja ja Media-palveluiden muodostaa ja on oikea vastaus asiakaskoneesta pyyntöjen perusteella. 


>[AZURE.NOTE] Lisätietoja hinnat tiedot on artikkelissa [Media Services hinnat tiedot](http://go.microsoft.com/fwlink/?LinkId=275107).

Jos haluat muuttaa streaming varattu yksiköiden määrän, toimi seuraavasti:
    
### <a name="get-the-streaming-endpoint-you-want-to-update"></a>Hae päivitettävän streaming päätepiste

Esimerkiksi aloitetaan ensimmäisen streaming päätepisteen tilisi (sinulla on kaksi streaming päätepisteet tilan käynnissä yhtä aikaa.)

**HTTP-pyyntö**:

    GET https://wamsbayclus001rest-hs.cloudapp.net/api/StreamingEndpoints()?$top=1 HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json;odata=verbose
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421466122&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=TiKGEOTporft4pFGU24sSZRZk5GRAWszFXldl5NXAhY%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net

**HTTP-vastaus**
    
Jos onnistuu, palautetaan seuraavasti:
    
    HTTP/1.1 200 OK
    . . . 
    
### <a name="scale-the-streaming-endpoint"></a>Skaalaa streaming päätepiste
 
**HTTP-pyyntö**:

    POST https://wamsbayclus001rest-hs.cloudapp.net/api/StreamingEndpoints('nb:oid:UUID:cd57670d-cc1c-0f86-16d8-3ad478bf9486')/Scale HTTP/1.1
    Content-Type: application/json;odata=verbose
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json;odata=verbose
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421466122&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=TiKGEOTporft4pFGU24sSZRZk5GRAWszFXldl5NXAhY%3d
    x-ms-version: 2.11
    x-ms-client-request-id: 39f96c93-a4b1-43ce-b97e-b2aaa44ee2dd
    Host: wamsbayclus001rest-hs.cloudapp.net
    
    {"scaleUnits":1}

**HTTP-vastaus**

    HTTP/1.1 202 Accepted
    Cache-Control: no-cache
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: 39f96c93-a4b1-43ce-b97e-b2aaa44ee2dd
    request-id: 3c1ba1c7-281c-4b2d-a898-09cb70a3a424
    x-ms-request-id: 3c1ba1c7-281c-4b2d-a898-09cb70a3a424
    operation-id: nb:opid:UUID:1853bcbf-b71f-4ed5-a4c7-a581d4f45ae7
    X-Content-Type-Options: nosniff
    DataServiceVersion: 1.0;
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Fri, 16 Jan 2015 22:16:43 GMT
    Content-Length: 0

    
### <a id="long_running_op_status"></a>Pitkään suoritettavien toiminnon tilan tarkistaminen

Kaikki uudet yksiköt kohdistus suorittamiseen tarvittava noin 20 minuutin ajan. Toiminto Käytä **toimintojen** menetelmää tilan tarkistaminen ja määritä toiminnon tunnus. Toiminnon tunnus palautettiin **asteikko** -pyynnön vastauksen.

    operation-id: nb:opid:UUID:1853bcbf-b71f-4ed5-a4c7-a581d4f45ae7
 
**HTTP-pyyntö**:
    
    GET https://wamsbayclus001rest-hs.cloudapp.net/api/Operations('nb:opid:UUID:1853bcbf-b71f-4ed5-a4c7-a581d4f45ae7') HTTP/1.1
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json;odata=verbose
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421466122&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=TiKGEOTporft4pFGU24sSZRZk5GRAWszFXldl5NXAhY%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
    
**HTTP-vastaus**
    
    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 515
    Content-Type: application/json;odata=verbose;charset=utf-8
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: 829e1a89-3ec2-4836-a04d-802b5aeff5e8
    request-id: f7ae8a78-af8d-4881-b9ea-ca072cfe0b60
    x-ms-request-id: f7ae8a78-af8d-4881-b9ea-ca072cfe0b60
    X-Content-Type-Options: nosniff
    DataServiceVersion: 1.0;
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Fri, 16 Jan 2015 22:57:39 GMT
    
    {  
       "d":{  
          "__metadata":{  
             "id":"https://wamsbayclus001rest-hs.cloudapp.net/api/Operations('nb%3Aopid%3AUUID%3Acc339c28-6bba-4f7d-bee5-91ea4a0a907e')",
             "uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/Operations('nb%3Aopid%3AUUID%3Acc339c28-6bba-4f7d-bee5-91ea4a0a907e')",
             "type":"Microsoft.Cloud.Media.Vod.Rest.Data.Models.Operation"
          },
          "Id":"nb:opid:UUID:cc339c28-6bba-4f7d-bee5-91ea4a0a907e",
          "State":"Succeeded",
          "TargetEntityId":"nb:oid:UUID:cd57670d-cc1c-0f86-16d8-3ad478bf9486",
          "ErrorCode":null,
          "ErrorMessage":null
       }
    }


## <a id="encode"></a>Koodata lähdetiedosto joukkoon mukautuvat bittitaajuus MP4-tiedostot

Ingesting varat kyselyjä Media Services media voidaan koodata, transmuxed, vesileimattu, ja niin edelleen jälkeen ennen kuin se toimitetaan asiakkaille. Näitä toimia niin ja suorittamalla eri taustan roolin esiintymissä erinomainen suorituskyky ja käytettävyyden varmistamiseksi. Näitä toimintoja kutsutaan työt ja kunkin [projektin](http://msdn.microsoft.com/library/azure/hh974289.aspx) koostuu atomisia tehtäviin, jotka tehdä työtä resurssi-tiedoston avulla. 

Kun kerrottiin, kun käsittelet Azure Media jokin tilanteissa toimittaa mukautuvat bittitaajuus streaming asiakkaiden palvelut. Media-palveluiden dynaamisesti voit pakata mukautuvat bittitaajuus MP4 tiedostojoukon johonkin seuraavista muodoista: HTTP Live Streaming (HLS), tasainen Streaming, MPEG VIIVA ja HDS (varten vain Adobe PrimeTime/Access asiakirjamalleja). 

Jos haluat hyödyntää Dynaaminen pakkaus, seuraavasti:

- koodata tai muuttaa oman mezzanine (lähde) tiedoston mukautuvat bittitaajuus MP4-tiedostoja tai mukautuvat bittitaajuus tasainen Streaming tiedostoja joukkoon  
- vähintään yhden streaming yksikön streaming päätepisteen, joista voit taata aikana sisällön hakeminen 

Seuraavassa osassa kerrotaan, miten työn, joka sisältää koodauksen tehtävän luominen. Tehtävän määrittää koodauksen mezzanine-tiedoston mukautuvat bittitaajuus MP4s käyttämällä **Media Encoder vakio**joukkoon. Osassa näkyvät myös seuraamisesta työn käsittely käynnissä. Kun työ on valmis, sinun olisi voivat luoda locators, joita tarvitaan Accessin hankkiminen oman varat. 

### <a name="get-a-media-processor"></a>Hae media-suoritin

Media-palveluiden media-suoritin on osa, joka käsittelee käsittelyä tehtävän, kuten koodaus-tiedostomuodon muuntaminen salauksen tai salauksen mediasisällön. Tässä opetusohjelmassa näytetään koodauksen tehtävän tarkastellaan käyttämään Media Encoder vakio.

Seuraava koodi pyytää encoder tunnus. 

**HTTP-pyyntö**

    GET https://wamsbayclus001rest-hs.cloudapp.net/api/MediaProcessors()?$filter=Name%20eq%20'Media%20Encoder%20Standard' HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=f7f09258-6753-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421675491&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=9hUudHYnATpi5hN3cvTfgw%2bL4N3tL0fdsRnQnm6ZYIU%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
    

**HTTP-vastaus**

    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 273
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    request-id: 6beb04b4-55a7-480d-8aa8-e5c5d59ffa1f
    x-ms-request-id: 6beb04b4-55a7-480d-8aa8-e5c5d59ffa1f
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 19 Jan 2015 07:54:09 GMT
    
    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#MediaProcessors",
       "value":[  
          {  
             "Id":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
             "Description":"Media Encoder Standard",
             "Name":"Media Encoder Standard",
             "Sku":"",
             "Vendor":"Microsoft",
             "Version":"1.1"
          }
       ]
    }

### <a name="create-a-job"></a>Työn luominen

Kunkin projektin voi olla vähintään yksi tehtävä, jonka haluat suorittaa käsittely tyypin mukaan. REST-Ohjelmointirajapinnalla avulla voit luoda työt ja niihin liittyvät tehtävät jollakin seuraavista tavoista: tehtävät voidaan määritetyn tekstiin työn kohteiden tehtävät selaus-ominaisuutta, tai OData eräkäsittely kautta. Media Services SDK käyttää eräkäsittely. Tämän artikkelin esimerkkejä koodin luettavuuden tehtävät ovat kuitenkin määritetyn tekstiin. Lisätietoja eräkäsittely on artikkelissa [Open Data Protocol (OData) erän käsittely](http://www.odata.org/documentation/odata-version-3-0/batch-processing/).

Seuraavassa esimerkissä esitetään, kuinka voit luoda ja julkaista projektin yhden tehtävän asettaminen tietyn tarkkuutta ja laatu videon koodata. Seuraavat asiakirjat-osio sisältää kaikki [tehtävän Esiasetukset](http://msdn.microsoft.com/library/mt269960) Media Encoder vakio suoritin tukemat luettelo.  

**HTTP-pyyntö**
    
    POST https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Content-Type: application/json
    Accept: application/json;odata=verbose
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421675491&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=9hUudHYnATpi5hN3cvTfgw%2bL4N3tL0fdsRnQnm6ZYIU%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
    Content-Length: 482
    
    {  
       "Name":"NewTestJob",
       "InputMediaAssets":[  
          {  
             "__metadata":{  
                "uri":"https://wamsbayclus001rest-hs.net/api/Assets('nb%3Acid%3AUUID%3A9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1')"
             }
          }
       ],
       "Tasks":[  
          {  
             "Configuration":"H264 Adaptive Bitrate MP4 Set 720p",
             "MediaProcessorId":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
             "TaskBody":"<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset>
                <outputAsset>JobOutputAsset(0)</outputAsset></taskBody>"
          }
       ]
    }

**HTTP-vastaus**

Jos onnistuu, funktio palauttaa seuraavan vastauksen:

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 1215
    Content-Type: application/json;odata=verbose;charset=utf-8
    Location: https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')
    Server: Microsoft-IIS/8.5
    request-id: 532ac1ec-a475-4dce-b2d5-7c8ce94ac87c
    x-ms-request-id: 532ac1ec-a475-4dce-b2d5-7c8ce94ac87c
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 19 Jan 2015 08:04:35 GMT
        
    {  
       "d":{  
          "__metadata":{  
             "id":"https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')",
             "uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')",
             "type":"Microsoft.Cloud.Media.Vod.Rest.Data.Models.Job"
          },
          "Tasks":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')/Tasks"
             }
          },
          "OutputMediaAssets":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')/OutputMediaAssets"
             }
          },
          "InputMediaAssets":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A9bc8ff20-24fb-4fdb-9d7c-b04c7ee573a1')/InputMediaAssets"
             }
          },
          "Id":"nb:jid:UUID:71d2dd33-efdf-ec43-8ea1-136a110bd42c",
          "Name":"NewTestJob",
          "Created":"2015-01-19T08:04:34.3287057Z",
          "LastModified":"2015-01-19T08:04:34.3287057Z",
          "EndTime":null,
          "Priority":0,
          "RunningDuration":0,
          "StartTime":null,
          "State":0,
          "TemplateId":null,
          "JobNotificationSubscriptions":{  
             "__metadata":{  
                "type":"Collection(Microsoft.Cloud.Media.Vod.Rest.Data.Models.JobNotificationSubscription)"
             },
             "results":[  
    
             ]
          }
       }
    }


Liittyy Huomautus työn pyyntö muutama huomioon:

- TaskBody ominaisuudet käytettävä literal XML määrittää syötteen määrän tai tulosteen kalusteet, joiden avulla tehtävän. Tehtävän aihe sisältää XML-rakenteen määritys XML.
- TaskBody-määrityksessä kunkin sisempi arvo <inputAsset> ja <outputAsset> on määritettävä JobInputAsset(value) tai JobOutputAsset(value).
- Tehtävän voi olla useita tulosteen resurssit. Yksi JobOutputAsset(x) voi käyttää vain kerran kuin tulos projektin tehtävästä.
- Voit määrittää syötteen resurssi tehtävän JobInputAsset tai JobOutputAsset.
- Jakson on eivät muodosta tehtävät.
- Value-parametria, jotka siirtävät JobInputAsset tai JobOutputAsset edustaa indeksiarvon sijoituksen. Todellinen varat määritetään työn kohteen määritelmän InputMediaAssets ja OutputMediaAssets siirtyminen ominaisuudet. 

>[AZURE.NOTE] Koska Media Services perustuu OData v3, InputMediaAssets ja OutputMediaAssets siirtyminen ominaisuuden kokoelmien yksittäisiä kohteita, joihin viitataan kautta "__metadata: uri-nimi-arvo-pari. 

- InputMediaAssets yhdistää yhden tai useamman varat olet luonut Media Services-palveluissa. OutputMediaAssets luodaan järjestelmä. Viittaa aiemmin resurssi.
- OutputMediaAssets voi nimetä assetName-määritteen avulla. Jos tämä määritettä ei ole ja valitse OutputMediaAsset on jostakin tekstissä arvo <outputAsset> elementti on jälkiliitteellä projektin nimi-arvoa tai työn tunnus-arvon (siltä varalta, jossa Name-ominaisuutta ei ole määritetty). Esimerkiksi jos määrität assetName "Esimerkki" arvo, valitse OutputMediaAsset Name-ominaisuutta arvoksi "Esimerkki". Kuitenkin jos ei ole määrittänyt assetName arvo, mutta määrittänyt "NewJob" työn nimen, valitse OutputMediaAsset nimi olisi "JobOutputAsset (arvo) _NewJob". 

    Seuraavassa esimerkissä esitetään määrittämisestä assetName määrite:
    
        "<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset assetName=\"CustomOutputAssetName\">JobOutputAsset(0)</outputAsset></taskBody>"


- Voit ottaa tehtävän ketjutus seuraavasti:

    - Työn on oltava vähintään kaksi tehtävää
    - Asiakirjassa on oltava vähintään yksi tehtävä, jonka syötteen siirretään toisen projektin tehtävästä.

Lisätietoja on artikkelissa- [koodaus työn Media Services REST-ohjelmointirajapinnalla luominen](http://msdn.microsoft.com/library/azure/jj129574.aspx).

### <a name="monitor-processing-progress"></a>Näytön käsittely käynnissä

Tila-ominaisuuden avulla voit hakea työn tilan seuraavan esimerkin mukaisesti. 

**HTTP-pyyntö**

    GET https://wamsbayclus001rest-hs.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')/State HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=youraccountname&urn%3aSubscriptionId=zf84471d-2233-4e75-aa09-010f0fc0cf5b&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1336908022&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=RYXOraO6Z%2f7l9whWZQN%2bypeijgHwIk8XyikA01Kx1%2bk%3d
    Host: wamsbayclus001rest-hs.net
    Content-Length: 0


**HTTP-vastaus**

Jos onnistuu, funktio palauttaa seuraavan vastauksen:

    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 17
    Content-Type: application/json;odata=verbose;charset=utf-8
    Server: Microsoft-IIS/7.5
    x-ms-request-id: 01324d81-18e2-4493-a3e5-c6186209f0c8
    X-Content-Type-Options: nosniff
    DataServiceVersion: 1.0;
    X-AspNet-Version: 4.0.30319
    X-Powered-By: ASP.NET
    Date: Sun, 13 May 2012 05:16:53 GMT
    
    {"d":{"State":2}}


### <a name="cancel-a-job"></a>Työn peruuttaminen

Media-palveluiden avulla voit peruuttaa käynnissä olevat työt CancelJob-toimintoa. Puhelun palauttaa 400 virhekoodi Jos yrität työn peruuttaminen, kun sen tila on peruutettu, peruuttaminen, virhe tai valmis.

Seuraavassa esimerkissä esitetään, miten voit soittaa CancelJob.


**HTTP-pyyntö**


    GET https://wamsbayclus001rest-hs.net/API/CancelJob?jobid='nb%3ajid%3aUUID%3a71d2dd33-efdf-ec43-8ea1-136a110bd42c' HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.2
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=youraccountname&urn%3aSubscriptionId=2f84471d-b1ae-4e75-aa09-010f0fc0cf5b&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1336908753&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=kolAgnFfbQIeRv4lWxKSFa4USyjWXRm15Kd%2bNd5g8eA%3d
    Host: wamsbayclus001rest-hs.net


Jos onnistuu, 204 vastauksen koodi palautetaan kanssa viestin tekstiosaa.

>[AZURE.NOTE] Sinun täytyy URL-Osoitteen koodata työn tunnus (tavallisesti nb:jid:UUID: somevalue) kun kulkeva sen parametrina CancelJob.


### <a name="get-the-output-asset"></a>Hae tulosteen resurssi 

Seuraava koodi esitetään, kuinka voit pyytää tulosteen kohteen tunnus. 


**HTTP-pyyntö**

    GET https://wamsbayclus001rest-hs.cloudapp.net/api/Jobs('nb%3Ajid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')/OutputMediaAssets() HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    User-Agent: Microsoft ADO.NET Data Services
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=amstestaccount001&urn%3aSubscriptionId=z7f09258-2233-4ca2-b1ae-193798e2c9d8&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1421675491&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=9hUudHYnATpi5hN3cvTfgw%2bL4N3tL0fdsRnQnm6ZYIU%3d
    x-ms-version: 2.11
    Host: wamsbayclus001rest-hs.cloudapp.net
    

**HTTP-vastaus**

    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 445
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    request-id: 73cd605d-066a-46f1-8358-f4bd25a9220a
    x-ms-request-id: 73cd605d-066a-46f1-8358-f4bd25a9220a
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    X-Powered-By: ASP.NET
    Strict-Transport-Security: max-age=31536000; includeSubDomains
    Date: Mon, 19 Jan 2015 08:28:13 GMT
        
    {  
       "odata.metadata":"https://wamsbayclus001rest-hs.cloudapp.net/api/$metadata#Assets",
       "value":[  
          {  
             "Id":"nb:cid:UUID:71d2dd33-efdf-ec43-8ea1-136a110bd42c",
             "State":0,
             "Created":"2015-01-19T07:52:15.603",
             "LastModified":"2015-01-19T07:52:15.603",
             "AlternateId":null,
             "Name":"Multibitrate MP4s",
             "Options":0,
             "Uri":"https://storagetestaccount001.blob.core.windows.net/asset-71d2dd33-efdf-ec43-8ea1-136a110bd42c",
             "StorageAccountName":"storagetestaccount001"
          }
       ]
    }



## <a id="publish_get_urls"></a>Resurssi ja Hae streaming ja REST-ohjelmointirajapinnalla asteittain Lataa URL-osoitteet

Virtauttaa tai ladata sijoituksen, sinun on "julkaisemaan" locator luomalla. Locators tarjoavat pääsyn kohteen olevat tiedostot. Media-palveluiden tukee kahdentyyppisiä locators: OnDemandOrigin locators, mediavirtaa (kuten MPEG VIIVA, HLS tai tasainen Streaming) ja Access-allekirjoitus (SAS) locators käyttää ladattaessa mediatiedostot.

Kun olet luonut locators, voit luoda URL-osoitteet, joiden avulla virtauttaa tai ladata tiedostoja. 


Streaming URL-osoite tasainen Streaming on seuraavanlainen:

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

HLS streaming URL-osoite on seuraavanlainen:

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

MPEG VIIVA streaming URL-osoite on seuraavanlainen:

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)


Tiedostojen lataaminen käytetään SAS URL-osoite on seuraavanlainen:

    {blob container name}/{asset name}/{file name}/{SAS signature}

Tässä osassa esitellään voit suorittaa seuraavia tehtäviä tarpeen "julkaista oman varat".  

- Lukuoikeudet AccessPolicy luominen 
- SAS URL-Osoitteen luomisesta sisällön lataamisen 
- Streaming sisällön origin URL-osoitteen luominen 

###<a name="creating-the-accesspolicy-with-read-permission"></a>Lukuoikeudet AccessPolicy luominen

Ennen kuin yrität ladata tai streaming minkä tahansa mediasisältöä, ensin määrittää AccessPolicy lukuoikeudet kanssa ja luoda haluamasi Locator yritys, joka määrittää toimituksen järjestelmä haluat asiakkaiden käyttöön. Saat lisätietoja saatavilla ominaisuudet [AccessPolicy kohteen ominaisuudet](https://msdn.microsoft.com/library/azure/hh974297.aspx#accesspolicy_properties).

Seuraavassa esimerkissä esitetään määrittäminen AccessPolicy annetulle kohteiden luku käyttöoikeudet.

    POST https://wamsbayclus001rest-hs.net/API/AccessPolicies HTTP/1.1
    Content-Type: application/json
    Accept: application/json
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=youraccountname&urn%3aSubscriptionId=2f84471d-b1ae-4e75-aa09-010f0fc0cf5b&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1337067658&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=dithjGvlXR9HlyAf5DE99N5OCYkPAxsHIcsTSjm9%2fVE%3d
    Host: wamsbayclus001rest-hs.net
    Content-Length: 74
    Expect: 100-continue
    
    {"Name": "DownloadPolicy", "DurationInMinutes" : "300", "Permissions" : 1}

Jos onnistuu, 201 success koodi palautetaan kuvaava AccessPolicy kohde, jonka loit. Voit käyttää AccessPolicy sekä resurssi tunnus, jossa haluat pitää (esimerkiksi tulostus-resurssi) Locator kohteen luominen tiedosto kohteen tunnus.

>[AZURE.NOTE]
Tavallinen työnkulku on sama kuin tiedoston lataamisessa, kun ingesting omaisuuden (kuten on edellä tämän ohjeaiheen). Myös kuten ladata tiedostoja, jos olet (tai asiakkaat) on käyttää tiedostojasi heti, arvoksi aloitusajan arvo viisi minuuttia, ennen kuin nykyinen aika. Tämä toiminto on tarpeen, koska saattaa olla kellon jakauman.vinous asiakas- ja Media-palveluiden välillä. Aloitusajan arvon on oltava DateTime-muodossa: YYYY-MM-DDTHH:mm:ssZ (esimerkiksi "2014-05-23T17:53:50Z").


###<a name="creating-a-sas-url-for-downloading-content"></a>SAS URL-Osoitteen luomisesta sisällön lataamisen 

Seuraava koodi näkyy hankkiminen URL-osoite, jonka avulla voidaan Lataa mediatiedosto luodaan ja ladata aiemmin. AccessPolicy on artikkelissa käyttöoikeuksien määrittäminen ja Locator polku viittaa SAS lataamisen URL-Osoitteen.

    POST https://wamsbayclus001rest-hs.net/API/Locators HTTP/1.1
    Content-Type: application/json
    Accept: application/json
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=youraccountname&urn%3aSubscriptionId=zf84471d-b1ae-2233-aa09-010f0fc0cf5b&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1337067658&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=dithjGvlXR9HlyAf5DE99N5OCYkPAxsHIcsTSjm9%2fVE%3d
    Host: wamsbayclus001rest-hs.net
    Content-Length: 182
    Expect: 100-continue
    
    {"AccessPolicyId": "nb:pid:UUID:38c71dd0-44c5-4c5f-8418-08bb6fbf7bf8", "AssetId" : "nb:cid:UUID:71d2dd33-efdf-ec43-8ea1-136a110bd42c", "StartTime" : "2014-05-17T16:45:53", "Type":1}

Jos onnistuu, funktio palauttaa seuraavan vastauksen:

    
    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 1150
    Content-Type: application/json;odata=verbose;charset=utf-8
    Location: https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A8e5a821d-2194-4d00-8884-adf979856874')
    Server: Microsoft-IIS/7.5
    x-ms-request-id: 8cfd8556-3064-416a-97f2-67d9e35f0911
    X-Content-Type-Options: nosniff
    DataServiceVersion: 1.0;
    X-AspNet-Version: 4.0.30319
    X-Powered-By: ASP.NET
    Date: Mon, 14 May 2012 21:41:32 GMT
        
    {  
       "d":{  
          "__metadata":{  
             "id":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A8e5a821d-2194-4d00-8884-adf979856874')",
             "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A8e5a821d-2194-4d00-8884-adf979856874')",
             "type":"Microsoft.Cloud.Media.Vod.Rest.Data.Models.Locator"
          },
          "AccessPolicy":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A8e5a821d-2194-4d00-8884-adf979856874')/AccessPolicy"
             }
          },
          "Asset":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A71d2dd33-efdf-ec43-8ea1-136a110bd42c')/Asset"
             }
          },
          "Id":"nb:lid:UUID:8e5a821d-2194-4d00-8884-adf979856874",
          "ExpirationDateTime":"\/Date(1337049393000)\/",
          "Type":1,
          "Path":"https://storagetestaccount001.blob.core.windows.net/asset-71d2dd33-efdf-ec43-8ea1-136a110bd42c?st=2012-05-14T21%3A36%3A33Z&se=2012-05-15T02%3A36%3A33Z&sr=c&si=8e5a821d-2194-4d00-8884-adf979856874&sig=y75dViDpC5V8WutrXM%2B%2FGpR3uOtqmlISiNlHU1YUBOg%3D",
          "AccessPolicyId":"nb:pid:UUID:38c71dd0-44c5-4c5f-8418-08bb6fbf7bf8",
          "AssetId":"nb:cid:UUID:71d2dd33-efdf-ec43-8ea1-136a110bd42c",
          "StartTime":"\/Date(1337031393000)\/"
       }
    }


Palautetut **Path** -ominaisuus on SAS URL-osoite.

>[AZURE.NOTE]
Jos olet ladannut tallennustilan salattu sisältöä, voit on manuaalisesti salauksen ennen sen, tai tallennustilan salauksen MediaProcessor käsittely tehtävän tulosteen käsitellyt tiedostoja valintaruudun valinnan poistaminen OutputAsset ja ladata sitten kyseinen resurssi. Lisätietoja käsittely on artikkelissa luominen koodaus työn Media Services REST-ohjelmointirajapinnalla. Lisäksi SAS URL-Osoitteen Locators ei voi päivittää, kun ne on luotu. Et voi käyttää samaa Locator päivitetyt aloitusajan-arvolla. Tämä johtuu SAS URL-osoitteet luodaan haluamallasi tavalla. Jos haluat käyttää resurssi ladattavaksi jälkeen Locator on päättynyt, sinun on luotava uusi, jolla uuden aloitusajan.

###<a name="download-files"></a>Tiedostojen lataaminen

Kun olet AccessPolicy ja määritä Locator, voit ladata tiedostot Azure-tallennustilan REST API.  

>[AZURE.NOTE] Sinun on lisättävä Tiedostonimi tiedoston haluat ladata vastaanotettu edellisessä osassa Locator **polku** -arvoa. Esimerkiksi https://storagetestaccount001.blob.core.windows.net/asset-e7b02da4-5a69-40e7-a8db-e8f4f697aac0/BigBuckBunny.mp4? . . . 

Katso lisätietoja käsittelystä tallennustilan Azure-BLOB- [Blob-objektien palvelun REST API](http://msdn.microsoft.com/library/azure/dd135733.aspx).

Koodauksen työ, joita voit suorittaa aiemmissa (koodaus kyselyjä mukautuvat MP4 määrittäminen), tuloksena on useita, voit ladata asteittain MP4-tiedostoja. Esimerkki:    
    
    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z
    
    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_400kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z
    
    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_3400kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z
    
    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_2250kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z
    
    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_1500kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z
    
    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_1000kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z
    
    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z
    
    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_AAC_und_ch2_56kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z


### <a name="creating-a-streaming-url-for-streaming-content"></a>Streaming URL-Osoitteen luomisesta streaming sisältö


Seuraava koodi näytetään, miten voit luoda streaming URL-Locator:

    POST https://wamsbayclus001rest-hs/API/Locators HTTP/1.1
    Content-Type: application/json
    Accept: application/json
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.11
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=youraccountname&urn%3aSubscriptionId=2f84471d-b1ae-4e75-aa09-010f0fc0cf5b&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1337067658&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=dithjGvlXR9HlyAf5DE99N5OCYkPAxsHIcsTSjm9%2fVE%3d
    Host: wamsbayclus001rest-hs
    Content-Length: 182
    Expect: 100-continue
    
    {"AccessPolicyId": "nb:pid:UUID:38c71dd0-44c5-4c5f-8418-08bb6fbf7bf8", "AssetId" : "nb:cid:UUID:eb5540a2-116e-4d36-b084-7e9958f7f3c3", "StartTime" : "2014-05-17T16:45:53",, "Type":2}

Jos onnistuu, funktio palauttaa seuraavan vastauksen:

    HTTP/1.1 201 Created
    Cache-Control: no-cache
    Content-Length: 981
    Content-Type: application/json;odata=verbose;charset=utf-8
    Location: https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A52034bf6-dfae-4d83-aad3-3bd87dcb1a5d')
    Server: Microsoft-IIS/7.5
    x-ms-request-id: 2eac4158-fc78-4fa2-81ee-c9f582dc385f
    X-Content-Type-Options: nosniff
    DataServiceVersion: 1.0;
    X-AspNet-Version: 4.0.30319
    X-Powered-By: ASP.NET
    Date: Mon, 14 May 2012 21:41:39 GMT
        
    {  
       "d":{  
          "__metadata":{  
             "id":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A52034bf6-dfae-4d83-aad3-3bd87dcb1a5d')",
             "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A52034bf6-dfae-4d83-aad3-3bd87dcb1a5d')",
             "type":"Microsoft.Cloud.Media.Vod.Rest.Data.Models.Locator"
          },
          "AccessPolicy":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A52034bf6-dfae-4d83-aad3-3bd87dcb1a5d')/AccessPolicy"
             }
          },
          "Asset":{  
             "__deferred":{  
                "uri":"https://wamsbayclus001rest-hs.net/api/Locators('nb%3Alid%3AUUID%3A52034bf6-dfae-4d83-aad3-3bd87dcb1a5d')/Asset"
             }
          },
          "Id":"nb:lid:UUID:52034bf6-dfae-4d83-aad3-3bd87dcb1a5d",
          "ExpirationDateTime":"\/Date(1337049395000)\/",
          "Type":2,
          "Path":"http://wamsbayclus001rest-hs.net/52034bf6-dfae-4d83-aad3-3bd87dcb1a5d/",
          "AccessPolicyId":"nb:pid:UUID:38c71dd0-44c5-4c5f-8418-08bb6fbf7bf8",
          "AssetId":"nb:cid:UUID:eb5540a2-116e-4d36-b084-7e9958f7f3c3",
          "StartTime":"\/Date(1337031395000)\/"
       }
    }

Lähettää tasainen Streaming origin URL-Osoitteen streaming media Player-ohjelmassa, voit lisätä ominaisuuden nimi tasainen Streaming luettelon tiedoston, ja sen jälkeen "/ luettelon" polku.

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest

Lähettää HLS liittäminen (Muotoile m3u8 aapl =) jälkeen "/ luettelon".

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest(format=m3u8-aapl)

Lähettää MPEG VIIVAN liittäminen (Muotoile mpd-aika-csf =) jälkeen "/ luettelon".

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest(format=mpd-time-csf)


## <a id="play"></a>Toistaa sisältöä  

Lähettää video [Azure mediasoitinta-palvelujen](http://amsplayer.azurewebsites.net/azuremediaplayer.html)avulla

Voit esikatsella asteittain Lataa-Liitä URL-osoite selaimen (esimerkiksi Internet Explorerin, Chromen, Safari).


##<a name="next-steps-media-services-learning-paths"></a>Seuraavat vaiheet: Media-palveluiden oppimispolut

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Palautteen antaminen

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="looking-for-something-else"></a>Etsitkö jotain muuta?

Jos ohjeaihe ei sisällä mitä olet odottanut, puuttuu jotain tai -jollakin muulla tavalla ei vastaa tarpeitasi, anna meille palautetta, käyttäen alla Disqus viestiketjun.



