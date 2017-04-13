<properties 
    pageTitle="Media-palveluiden julkaisutiedot | Microsoft Azure" 
    description="Media Services-palveluiden julkaisutiedot" 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="media" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="09/19/2016"
    ms.author="juliako"/>

# <a name="azure-media-services-release-notes"></a>Azure Media-palveluiden julkaisutiedot

Nämä julkaisutiedot yhteenvedon muutokset aiempien versioiden ja tunnetut ongelmat.

>[AZURE.NOTE] Haluamme kuulla asiakkaidemme ja ongelmien korjaaminen, jotka vaikuttavat voit keskittyä. Raportoi ongelmasta tai esittää kysymyksiä, voit julkaista [Azure Media Services MSDN-keskustelupalsta].


##<a id="issues"></a>Tällä hetkellä tunnetut ongelmat

### <a id="general_issues"></a>Media Services-palveluiden yleiset ongelmat

Ongelma|Kuvaus
---|---
Yleisiä HTTP-otsikot eivät kuulu REST-Ohjelmointirajapinnalla.|Jos kehität Media Services-sovellusten avulla REST-Ohjelmointirajapinta, löydät, kenttiä yleisiä HTTP-otsikko (mukaan lukien asiakas-PYYNNÖN-ID PYYNNÖN tunnus ja PALAA-asiakas-PYYNNÖN-tunnus) ei tueta. Otsikot lisätään tulevien päivitys.
URL-koodausta ei ole sallittua.|Media Services käyttää IAssetFile.Name-ominaisuuden arvon luotaessa URL-osoitteet streaming sisällön (esimerkiksi http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) Tästä syystä URL-koodausta ei sallita. Ominaisuuden **nimi** ei saa olla seuraavia [prosentti-koodaus-varattu merkit](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters): !*'();:@&=+$,/?%#[]". Lisäksi voi olla vain yksi '. " Saat tunnistetta.
ListBlobs menetelmä, joka on osa Azure-tallennustilan SDK versio 3.x epäonnistuu.|Media Services luo SAS URL-osoitteet [2012-02 – 12](http://msdn.microsoft.com/library/azure/dn592123.aspx) -version perusteella. Jos haluat käyttää Azure-tallennustilan SDK luettelon BLOB-blob-säilö, käytä [CloudBlobContainer.ListBlobs](http://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.cloudblobcontainer.listblobs.aspx) menetelmää, joka on osa Azure-tallennustilan SDK versio 2.x. ListBlobs menetelmä, joka on osa Azure-tallennustilan SDK-version 3.x epäonnistuu.
Media-palveluiden rajoittimen järjestelmä rajoittaa resurssien käyttö-sovelluksista, joihin liiallinen pyydettävä-palveluun. Palvelun voi palauttaa palvelu ei ole käytettävissä (503) HTTP-tilakoodin.|Lisätietoja on artikkelissa [Azure Media-palveluiden virhekoodit](http://msdn.microsoft.com/library/azure/dn168949.aspx) aiheen 503 HTTP-tilakoodin kuvaus.
Kun kyselyt kohteita ei palauttaa yhtä aikaa, koska julkisen muiden v2 rajoittaa kyselyn tuloksia 1000 tulokset 1000 kohteiden enimmäismäärä. | Sinun on käytettävä **Ohita** ja **toteuttaa** (.NET) / **ylös** (REST) muodossa, joka on kuvattu [.NET tässä esimerkissä](media-services-dotnet-manage-entities.md#enumerating-through-large-collections-of-entities) ja [REST API tässä esimerkissä](media-services-rest-manage-entities.md#enumerating-through-large-collections-of-entities). 
Joissakin palveluissa voit huomata toista tunniste-ongelma tasainen Streaming-luettelossa.|Lisätietoja on artikkelissa [sisältö](media-services-deliver-content-overview.md#known-issues) .
Azure Media Services .NET SDK-objekteja ei voi muuntaa sarjaksi, ja tuloksena eivät toimi Azure välimuistiin.|Jos yrität muuntaa sarjaksi SDK AssetCollection-objektin lisääminen Azure välimuistiin, ilmenee poikkeus.
Koodauksen työt epäonnistua viestin merkkijonolla "vaihe: DownloadFile. Koodi: System.NullReferenceException ".|Tyypillinen koodauksen työnkulku on syötteen videon tiedostot lataaminen syötteen resurssi ja lähettää vähintään yksi koodauksen työt, kyseisen syötteen annetaan, muuttamatta edelleen, kirjoita resurssi. Kuitenkin jos muokkaat syötteen resurssi (esimerkiksi mukaan lisääminen/poistaminen tai nimeäminen uudelleen kohteen tiedostot), valitse seuraavat työt voi epäonnistua DownloadFile virhe. Menetelmä on syötteen resurssi ja lataa uudelleen syötteen tiedostot, uusi resurssi. 

##<a id="rest_version_history"></a>REST API versiohistoria

Media Services REST API-versiohistoria tietoja on artikkelissa [Azure Media Services REST API-viittaus].

##<a id="july_changes16"></a>Heinäkuussa 2016-versiossa

###<a name="updates-to-manifest-file-ism-generated-by-encoding-tasks"></a>Luettelon tiedoston päivityksiä (*. ISM) luoma koodaus tehtävät

Kun koodauksen tehtävä on lähetetty Media Encoder vakio- tai Azure Media Encoder, koodauksen tehtävän Luo [streaming luettelon tiedosto](media-services-deliver-content-overview.md) (* .ism) tiedoston tulosteen resurssi. Palvelun uusimman version, jossa on päivitetty streaming luettelon tiedoston syntaksi.

>[AZURE.NOTE]Streaming luettelo (.ism)-tiedoston syntaksi on varattu sisäiseen käyttöön ja sitä saatetaan muuttaa tulevissa versioissa. Älä muokkaa tai käsitellä tämän tiedoston sisällön.

###<a name="a-new-client-manifest-ismc-file-is-generated-in-the-output-asset-when-an-encoding-task-outputs-one-or-more-mp4-files"></a>Uusi asiakas-luettelo (*. ISMC)-tiedosto luodaan tulosteen resurssi, kun koodauksen tehtävän tulostaa MP4-tiedostot

Lähtien palvelun uusin koodauksen tehtävän, joka tuottaa yhden päättymisen jälkeen lisää tiedostoja MP4-tulosteen resurssi myös sisältävät streaming asiakas-luettelo (*.ismc)-tiedosto. .Ismc tiedoston parantaa suorituskykyä dynaaminen streaming. 

>[AZURE.NOTE]Asiakas-luettelo (.ismc)-tiedoston syntaksi on varattu sisäiseen käyttöön ja sitä saatetaan muuttaa tulevissa versioissa. Älä muokkaa tai käsitellä tämän tiedoston sisällön.

Lisätietoja on artikkelissa [Tämä](https://blogs.msdn.microsoft.com/randomnumber/2016/07/08/encoder-changes-within-azure-media-services-now-create-ismc-file/) blogiin.

### <a name="known-issues"></a>Tunnetut ongelmat

Joissakin palveluissa voi tulla aross toista tunniste-ongelma tasainen Streaming-luettelossa. Lisätietoja on artikkelissa [sisältö](media-services-deliver-content-overview.md#known-issues) .

##<a id="apr_changes16"></a>Huhtikuussa 2016-versiossa

### <a name="azure-media-analytics"></a>Azure Media Analytics

Azure Media Servces otetaan käyttöön Azure Media Analytics tehokkaita videon liiketoimintatietojen. Lisätietoja on artikkelissa [Azure Media Services Analytics yleiskatsaus](media-services-analytics-overview.md).

### <a name="apple-fairplay-preview"></a>Apple FairPlay (ennakkoversio)

Azure Media-palveluiden avulla voit salata dynaamisesti oman HTTP Live Streaming (HLS) sisällöstä Apple FairPlay nyt. Voit käyttää myös AMS käyttöoikeuden toimituksen palvelun aikana FairPlay käyttöoikeuksien asiakkaille. Katso tarkempia tietoja, [Käytä Azure Media Services lähettää oman HLS sisällön suojatun Apple FairPlay kanssa ](media-services-protect-hls-with-fairplay.md).
  
##<a id="feb_changes16"></a>Helmikuussa 2016-versiossa

Azure Media Services SDK .NET (3.5.3) uusin versio sisältää Widevine liittyvät ohjelmavirhe korjaa. Ongelma on: AssetDeliveryPolicy ei voi käyttää uudelleen salattu Widevine useita kohteita. Ohjelmavirhe korjaa osana seuraava ominaisuus on lisätty SDK: **WidevineBaseLicenseAcquisitionUrl**.
    
    Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
        new Dictionary<AssetDeliveryPolicyConfigurationKey, string>
    {
        {AssetDeliveryPolicyConfigurationKey.WidevineBaseLicenseAcquisitionUrl,"http://testurl"},
        
    };

##<a id="jan_changes_16"></a>Tammikuussa 2016-versiossa

Koodaus varatut resurssit nimetä uudelleen, voit pienentää sekaannusta Encoder nimillä.

Basic, Vakio ja Premium koodauksen varattu yksiköt nimetään S1, S2, ja S3 varatut resurssit, tarpeen mukaan.  Asiakkaat, jotka käyttävät Basic koodaus RUs tänään tulee näkyviin S1 otsikkona Azure-portaalissa (ja laskussa), kun vakio ja Premium näkevät otsikot S2 ja S3 tarpeen mukaan. 

##<a id="dec_changes_15"></a>Joulukuussa 2015 julkaisu

Azure SDK-ryhmän julkaistu uusi versio [Azure SDK PHP](http://github.com/Azure/azure-sdk-for-php) -paketti, joka sisältää päivitykset ja uudet ominaisuudet Microsoft Azure Media Services. Erityisesti Azure Media SDK PHP tukee nyt uusimmat [sisällön suojauksen](media-services-content-protection-overview.md) ominaisuudet: dynaaminen salaus AES ja DRM (PlayReady ja Widevine) kanssa ja ilman tunnuksen rajoitus. Se tukee myös skaalaus [Koodaus yksiköt](media-services-dotnet-encoding-units.md).

Lisätietoja on artikkelissa:

- [Microsoft Azure Media Services SDK PHP](http://southworks.com/blog/2015/12/09/new-microsoft-azure-media-services-sdk-for-php-release-available-with-new-features-and-samples/) -blogissa.
- Seuraavat [MALLIKOODEJA](http://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices) , joiden avulla pääset nopeasti alkuun:
    - **vodworkflow_aes.php**: Tämä on PHP-tiedosto, joka näyttää, miten AES-128 dynaaminen salaus ja avain toimitus-palvelun. Se perustuu [Tässä](media-services-protect-with-aes128.md) artikkelissa kerrotaan .NET-malli.
    - **vodworkflow_aes.php**: Tämä on PHP-tiedosto, joka näyttää, miten PlayReady dynaaminen salaus ja käyttöoikeus toimitus-palvelun. Se perustuu [Tässä](media-services-protect-with-drm.md) artikkelissa kerrotaan .NET-malli.
    - **scale_encoding_units.php**: Tämä on PHP-tiedosto, joka näyttää, miten skaalata koodauksen varattu yksikkö.


##<a id="nov_changes_15"></a>Marraskuun 2015 julkaisu

Azure Media-palvelut on nyt Google Widevine käyttöoikeuden toimituksen palvelun pilveen. Lisätietoja on viitata [Tämä ilmoitus-blogi](https://azure.microsoft.com/blog/announcing-google-widevine-license-delivery-services-public-preview-in-azure-media-services/). Katso myös [Tässä opetusohjelmassa](media-services-protect-with-drm.md) ja [GitHub säilöön](http://github.com/Azure-Samples/media-services-dotnet-dynamic-encryption-with-drm). 

Huomaa, että Widevine käyttöoikeuden toimituksen palveluja Azure Media Microsoft esikatselussa. Katso lisätietoja [tämän blogin](https://azure.microsoft.com/blog/announcing-google-widevine-license-delivery-services-public-preview-in-azure-media-services/).

##<a id="oct_changes_15"></a>Lokakuussa 2015 julkaisu

Azure Media Services (AMS) on nyt käytettävissä seuraavat tiedot keskikohdan mukaan: Brasilia Etelä, Intia Länsi, Intia Etelä ja Intia keskitetyn. Voit nyt luoda [Media palvelutilejä](media-services-portal-create-account.md) Azure perinteinen-portaalin käyttäminen ja suorittaa erilaisia tehtäviä, joka on kuvattu [seuraavassa](https://azure.microsoft.com/documentation/services/media-services/). Kuitenkin Live koodaus ei ole otettu käyttöön näiden tietojen keskikohdan mukaan. Lisäksi ei kaikenlaisia koodaus varatut resurssit ovat käytettävissä näiden tietojen keskikohdan mukaan.

- Brasilia Etelä: Vain vakio- ja Basic koodaus varatut resurssit ovat käytettävissä
- Intia Länsi, Intia Etelä ja Intia keskitetyn: vain Basic koodaus varatut resurssit ovat käytettävissä


##<a id="september_changes_15"></a>Syyskuussa 2015 julkaisu 

- AMS on nyt voi suojata videotilauspalveluiden (VOD)- ja Live virtaa Widevine moduulit DRM tekniikalla. Voit käyttää toimituksen palvelut-kumppanien avulla voit pitää Widevine käyttöoikeudet: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/) [castLabs](http://castlabs.com/company/partners/azure/). Lisätietoja on artikkelissa [tämä blogiin](https://azure.microsoft.com/blog/azure-media-services-adds-google-widevine-packaging-for-delivering-multi-drm-stream/).

    Voit käyttää [AMS.NET SDK](https://www.nuget.org/packages/windowsazure.mediaservices/) (alkaa numerosta versio 3.5.1) tai REST API määrittäminen käyttämään Widevine oman AssetDeliveryConfiguration.  

- AMS lisätty tuki Apple ProRes videot. Voit nyt ladata QuickTime videot lähdetiedostot, jotka käyttävät Apple ProRes tai muita pakkauksenhallintoja. Lisätietoja on artikkelissa [tämä blogiin](https://azure.microsoft.com/blog/announcing-support-for-apple-prores-videos-in-azure-media-services/).

- Nyt voit Media Encoder vakio tekemään leikkauksen välisumma- ja live arkisto poiminnan. Lisätietoja on artikkelissa [tämä blogiin](https://azure.microsoft.com/blog/sub-clipping-and-live-archive-extraction-with-media-encoder-standard/).

- Tehtiin suodatus seuraavat päivitykset: 

    - Voit nyt käyttää pelkän suodattimella Apple HTTP Live Streaming (HLS)-muodossa. Tämän päivityksen avulla voit poistaa pelkän Jäljitä määrittämällä (pelkän EPÄTOSI) URL-osoitteen.
    - Oman varat suodattimet määritettäessä mahdollisuus yhdistää useita on nyt yksi URL-osoite (3) suodattimet.

    Katso lisätietoja [tämän](https://azure.microsoft.com/blog/azure-media-services-release-dynamic-manifest-composition-remove-hls-audio-only-track-and-hls-i-frame-track-support/) blogin.

- AMS tukee nyt voin-kehyksiä HLS v4. I-kehyksen tuki optimoi eteenpäin siirtyminen-ja taaksepäin. Oletusarvon mukaan kaikki HLS v4 tulostaa sisältävät voin-kehyksen soittoluettelo (EXT-X-I-FRAME-STREAM-INF).
 
    Katso lisätietoja [tämän](https://azure.microsoft.com/blog/azure-media-services-release-dynamic-manifest-composition-remove-hls-audio-only-track-and-hls-i-frame-track-support/) blogin.

##<a id="august_changes_15"></a>Elokuussa 2015 julkaisu

- Azure Media Services SDK Java V0.8.0 release ja uusi mallit ovat nyt käytettävissä. Lisätietoja on artikkelissa:

    - [Blogimerkintä](http://southworks.com/blog/2015/08/25/microsoft-azure-media-services-sdk-for-java-v0-8-0-released-and-new-samples-available/)
    - [Java esimerkit säilöön](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
- Azure Media Player-päivityksen usean äänivirtaa tukeen. Lisätietoja on artikkelissa:
    - [Blogimerkintä](https://azure.microsoft.com/blog/2015/08/13/azure-media-player-update-with-multi-audio-stream-support/)

##<a id="july_changes_15"></a>Heinäkuussa 2015 julkaisu

- Julkaisu Media Encoder standardin yleiseen käyttöön. Lisätietoja [rajaamisesta on blogikirjoituksessa](https://azure.microsoft.com/blog/2015/07/16/announcing-the-general-availability-of-media-encoder-standard/).

    [Tässä](http://go.microsoft.com/fwlink/?LinkId=618336) osassa kuvataan Esiasetukset käyttää Media Encoder vakio. Huomaa, että kun esiasetus käytön 4k käyttää Hanki **Premium** varattu yksikkötyyppi. Katso lisätietoja, [miten asteikko koodaus](media-services-scale-media-processing-overview.md).
- Reaaliaikainen reaaliaikainen kuvatekstit Azure Media Services ja toisto-ohjelman kanssa. Lisätietoja [rajaamisesta on blogikirjoituksessa](https://azure.microsoft.com/blog/2015/07/08/live-real-time-captions-with-azure-media-services-and-player/)

###<a name="media-services-net-sdk-updates"></a>Media Services-palveluiden .NET SDK päivitykset

Azure Media Services .NET SDK on nyt versio 3.4.0.0. Tässä versiossa on lisätty seuraavat ominaisuudet:  

- Live arkisto toteutettu tuki. Huomaa, että et voi ladata resurssi, joka sisältää live arkisto.
- Dynaamisia suodattimia toteutettu tuki.
- Toteutuksen toimintoja, joiden avulla käyttäjät voivat pitää tallennustilan säiliön poistettaessa resurssi.
- Virheenkorjauksia liittyvät yrittää kanavien käytännöt.
- Käytössä **Media Encoder Premium työnkulun**.

##<a id="june_changes_15"></a>Kesäkuussa 2015 julkaisu

###<a name="media-services-net-sdk-updates"></a>Media Services-palveluiden .NET SDK päivitykset

Azure Media Services .NET SDK on nyt versio 3.3.0.0. Tässä versiossa on lisätty seuraavat ominaisuudet:  

- OpenId yhteyden etsiminen määritys-tuki
- käsittelyn näppäimet palauttaminen tunnistetietojen toimittaja reunassa tuen. 

Jos käytössäsi on identity-palvelu, joka paljastaa OpenID yhteyden etsiminen asiakirjan (tavalla seuraavat palvelut: Azure Active Directory, Google, Salesforce), pyydät Azure Media Services hankkiminen allekirjoitetun näppäimet vahvistamisen JWT tunnuksen OpenID-yhteyden etsiminen määritys. 

Lisätietoja on artikkelissa [Käyttämällä Json Web pikanäppäimet-OpenID yhteyden etsiminen määritys JWT suojaustunnuksen todennus Azure Media Services-käyttöä varten](http://gtrifonov.com/2015/06/07/using-json-web-keys-from-openid-connect-discovery-spec-to-work-with-jwt-token-authentication-in-azure-media-services/).


##<a id="may_changes_15"></a>Vapauta toukokuu 2015

Julkaisu seuraavat uudet ominaisuudet:

- [Esikatselun Live koodaus Media-palvelujen kanssa](media-services-manage-live-encoder-enabled-channels.md)
- [Dynaaminen luettelo](media-services-dynamic-manifest-overview.md)
- [Azure Media Hyperlapse media suoritin esikatselu](https://azure.microsoft.com/blog/?p=286281&preview=1&_ppp=61e1a0b3db)

##<a id="april_changes_15"></a>Huhtikuussa 2015 julkaisu

 ###<a name="general-media-services-updates"></a>Yleinen median Services päivitykset

- [Julkaisu Azure Media Player-ohjelmassa](https://azure.microsoft.com/blog/2015/04/15/announcing-azure-media-player/).
- Media Services muiden 2.10, kanavat, joille on määritetty ingest RTMP-protokolla, aloittaen luodaan ensisijainen ja toissijainen ingest URL-osoitteet. Lisätietoja on artikkelissa [kanavan ingest määritykset](media-services-live-streaming-with-onprem-encoders.md#channel_input)
- Azure Media indeksointitoiminto päivitykset
- Englannin kielen tuki
- Uusien määritysten xml-muoto

Katso lisätietoja [tämän blogin](https://azure.microsoft.com/blog/2015/04/13/azure-media-indexer-spanish-v1-2/).
###<a name="media-services-net-sdk-updates"></a>Media Services-palveluiden .NET SDK päivitykset

Azure Media Services .NET SDK on nyt versio 3.2.0.0.

Seuraavassa on joitakin asiakkaan vastakkaisten päivitykset:

- **Muuta purkaa**: muuttaa merkkijonon tyypin pitää **TokenRestrictionTemplate.Issuer** ja **TokenRestrictionTemplate.Audience** .
- Mukautettu luomiseen liittyvät päivitykset uudelleen käytännöt.
- Virheenkorjauksia liittyvien tiedostojen lataamisesta ja lataaminen.
- **MediaServicesCredentials** luokan hyväksyy ensisijainen ja toissijainen access ohjausobjektin päätepiste todennetaan.



##<a id="march_changes_15"></a>Maaliskuussa 2015 julkaisu

### <a name="general-media-services-updates"></a>Yleinen median Services päivitykset

- Media-palvelut on nyt Azure CDN integrointi. Tukemaan integrointi **CdnEnabled** -ominaisuus on lisätty **StreamingEndpoint**.  **CdnEnabled** voidaan käyttää REST API versio 2.9 alkaen (Lisätietoja on artikkelissa [StreamingEndpoint](https://msdn.microsoft.com/library/azure/dn783468.aspx)).  **CdnEnabled** voidaan käyttää .NET SDK versio 3.1.0.2 alkaen (Lisätietoja on artikkelissa [StreamingEndpoint](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mediaservices.client.istreamingendpoint(v=azure.10).aspx)).
- **Media Encoder Premium työnkulun**ilmoitus. Lisätietoja on artikkelissa [Esittely Premium koodaus Azure Media-palveluita](https://azure.microsoft.com/blog/2015/03/05/introducing-premium-encoding-in-azure-media-services/).
 


##<a id="february_changes_15"></a>Helmikuussa 2015 julkaisu

### <a name="general-media-services-updates"></a>Yleinen median Services päivitykset

Media Services REST API on nyt versio 2.9. Aloita tällä versiolla, voit ottaa Azure CDN integrointi streaming päätepisteet. Lisätietoja on artikkelissa [StreamingEndpoint](https://msdn.microsoft.com/library/dn783468.aspx).

##<a id="january_changes_15"></a>Tammikuussa 2015 julkaisu

### <a name="general-media-services-updates"></a>Yleinen median Services päivitykset

Ilmoitus, yleinen käytettävyys (GA) sisällön dynaamisen salauksella suojauksen. Lisätietoja on artikkelissa [Azure Media Services parantaa streaming arvopaperille, jonka DRM Yleiset käytettävyys-tekniikkaa](https://azure.microsoft.com/blog/2015/01/29/azure-media-services-enhances-streaming-security-with-general-availability-of-drm-technology/).

###<a name="media-services-net-sdk-updates"></a>Media Services-palveluiden .NET SDK päivitykset

Azure Media Services .NET SDK on nyt versio 3.1.0.1.

Tässä versiossa, jossa lukee Microsoft.WindowsAzure.MediaServices.Client.ContentKeyAuthorization.TokenRestrictionTemplate oletuskonstruktoria sellaisena kuin se on vanhentunut. Uusi konstruktoria kestää TokenType argumenttina.

    TokenRestrictionTemplate template = new TokenRestrictionTemplate(TokenType.SWT);


##<a id="december_changes_14"></a>Joulukuussa 2014: n julkaisu

###<a name="general-media-services-updates"></a>Yleinen median Services päivitykset

- Jotkin päivitykset ja uusista ominaisuuksista on lisätty Azure indeksointitoiminto Media-suoritin. Lisätietoja on artikkelissa [Azure Media indeksointitoiminto versio 1.1.6.7 julkaisutiedot](https://azure.microsoft.com/blog/2014/12/03/azure-media-indexer-version-1-1-6-7-release-notes/).
- Lisätty uudella REST-Ohjelmointirajapinnalla, jonka avulla voit päivittää koodaus varatut resurssit: [EncodingReservedUnitType muiden kanssa](http://msdn.microsoft.com/library/azure/dn859236.aspx).
- Lisätty CORS tue avaimen toimitus-palveluun.
- Suorituskykyä on parannettu, kyselyt luvan käytäntöasetukset oli valmis.
- Kiinan tietokeskuksen [Avain toimituksen URL-osoite](http://msdn.microsoft.com/library/azure/ef4dfeeb-48ae-4596-ab28-44d6b36d8769#get_delivery_service_url) on nyt asiakasta kohti (aivan kuten muita tietoja keskikohdan mukaan).
- Lisätty HLS automaattinen kohde kesto. Kun teet live-tietovirta, HLS aina on pakattu dynaamisesti. Media-palveluiden laskee oletusarvoisesti automaattisesti HLS segmentin pakkaus suhde (FragmentsPerSegment) perusteella kutsutaan myös ryhmä, kuvat – GOP, joka on saatu Live encoder Avainkuva väli (KeyFrameInterval). Lisätietoja on artikkelissa [Azure Media Services Live Streaming käsitteleminen].
 
###<a name="media-services-net-sdk-updates"></a>Media Services-palveluiden .NET SDK päivitykset

- [Azure Media Services.NET SDK](http://www.nuget.org/packages/windowsazure.mediaservices/) on nyt 3.1.0.0 versio.
- Päivittää .net SDK-riippuvuus .NET 4.5 Framework.
- Lisätty uusi API, jonka avulla voit päivittää koodauksen varatut resurssit. Lisätietoja on artikkelissa [päivittäminen varattu tyyppi ja lisääntyvien koodaus RUs käyttämällä .NET](http://msdn.microsoft.com/library/azure/jj129582.aspx).
- Lisätty JWT (JSON Web suojaustunnuksen) tukevat suojaustunnuksen todennusta varten. Lisätietoja on artikkelissa [JWT suojaustunnuksen todennus Azure Media Services-palveluissa ja dynaaminen salausta](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/).
- Lisätty suhteellinen siirtymät BeginDate ja ExpirationDate PlayReady käyttöoikeuden mallissa.


##<a id="november_changes_14"></a>Marraskuussa 2014: n julkaisu

- Media-palveluiden avulla nyt ingest live tasainen Streaming (FMP4) sisällön SSL-yhteyden. Voit ingest SSL-yhteyden kautta, varmista, että päivittämään ingest URL-Osoitteeseen HTTPS.  Jos haluat lisätietoja live streaming on artikkelissa [Azure Media Services Live Streaming käsitteleminen].
- Huomaa, että tällä hetkellä voit ei ingest live RTMP-tietovirta SSL-yhteyttä.
- Voit myös käyttää sisältöä SSL-yhteyttä. Toiminto, varmista, että streaming URL-osoitteet alkavat HTTPS.
- Huomaa, että voit vain virtauttaa SSL-yhteyden kautta, jos streaming päätepisteen, josta pidät sisältöä on luotu 10. syyskuussa 2014 jälkeen. Jos luotu jälkeen syyskuu 10 streaming päätepisteet perustuvat streaming URL-osoitteet, URL-osoite sisältää "streaming.mediaservices.windows.net" (uusi muoto). Streaming URL-osoitteet, jotka sisältävät "origin.mediaservices.windows.net" (vanha format) eivät tue SSL. Jos haluat, että voit käyttää SSL: n, [Luo uusi streaming päätepiste](media-services-portal-manage-streaming-endpoints.md)URL-osoite on vanha-muodossa. URL-osoitteet, Luo uusi streaming päätepiste perusteella avulla voit lähettää sisältöä SSL-yhteyden kautta.

##<a id="october_changes_14"></a>Lokakuussa 2014: n julkaisu

### <a id="new_encoder_release"></a>Media Encoder Release palvelut

Julkaisu Media Services Azure Media Encoder uuden version. Kanssa viimeistään Azure Media Encoder vain perittävän tulosteen gigatavun, mutta muuten uusi encoder on yhteensopiva edellisen encoder ominaisuus. Lisätietoja [Media Services hinnoittelu tiedot]).

### <a id="oct_sdk"></a>Media-palveluiden .NET SDK-paketissa 

Media Services SDK .NET-laajennukset on nyt versio 2.0.0.3.

3.0.0.8 versio on nyt .NET Media Services SDK-paketissa.

Seuraavat muutokset on tehty:

- Optimointi uudelleen käytännön luokkiin.
- Http-pyyntö otsikoiden lisääminen käyttäjäagentin merkkijonon.
- Lisätään nuget palauttaminen muodosta vaihe.
- Ongelmien skenaarion testien käyttämään x509 varmenteen säilöstä.
- Asetusten vahvistaminen päivitettäessä kanavan ja toistamisen loppuun.
 

### <a name="new-github-repository-to-host-media-services-samples"></a>Uusi GitHub säilön host Media Services-esimerkkejä

Esimerkkejä, joiden on ohjeaiheessa [Azure Media Services näytteiden GitHub säilöön](https://github.com/Azure/Azure-Media-Services-Samples).


##<a id="september_changes_14"></a>Syyskuussa 2014: n julkaisu

Media-palveluiden REST-metatiedot on nyt versioon 2.7. Lisätietoja muiden uusimmista päivityksistä on artikkelissa [Azure Media Services REST API-viittaus].

Media Services SDK .NET on nyt 3.0.0.7-versio
 
### <a id="sept_14_breaking_changes"></a>Tärkeimmät muutokset

* [StreamingEndpoint] **origin** annettiin uusi nimi.
* Muuta oletustoiminnan käytettäessä **Azure perinteinen Portal** koodata ja julkaista MP4-tiedostot.

Aiemmin, kun julkaiseminen yhden tiedoston MP4 videon resurssi SAS URL-Osoitteen Azure perinteinen portaalin avulla luodaan (SAS URL-osoitteet avulla voit ladata videon blob-säiliö). Tällä hetkellä käytettäessä Azure perinteinen portaalin koodata ja julkaista videon yhden tiedoston MP4-resurssi luotu URL-osoite osoittaa Media Azure-palveluiden streaming päätepiste.  Tämä muutos ei vaikuta MP4-videoita, jotka on ladattu Media Services suoraan ja julkaisemisen ilman koodattu Azure Media-palveluissa.

Sinulla on tällä hetkellä ongelman ratkaisemiseen vaihtoehdoista.

* Ota streaming yksiköt ja virtauttaa streaming tasainen-esityksenä .mp4-resurssi Dynaaminen pakkaus avulla.

* Luo ja lataa (tai toistaa asteittain) .mp4 SAS URL-osoite. Lisätietoja SAS locator luomisesta on kohdassa [Välittää sisällön].


### <a id="sept_14_GA_changes"></a>Uudet ominaisuudet ja skenaariot, jotka ovat osa GA julkaisemisesta

* **Indeksointitoiminto Media suoritin**. Katso lisätietoja [Indeksoinnin mediatiedostojen Azure Media indeksointitoiminto kanssa].

* [StreamingEndpoint] -kohteen avulla voit lisätä mukautettuja toimialuenimiä (isäntä) nyt.

    Mukautettu toimialuenimi, jota käytetään Media Services streaming päätepisteen nimi sinun on mukautettu isäntänimet lisääminen streaming päätepiste. Lisää mukautettu isäntänimet Media Services REST API tai .NET SDK avulla.
    
    Ottaa huomioon seuraavat asiat:
    
    * Sinulla on oltava mukautetun toimialuenimen omistajuuden.
    
    * Toimialuenimen omistajuuden on tarkistettava Azure Media-palveluissa. Vahvista toimialueen, Luo CNAME-tietue, joka yhdistää <MediaServicesAccountId>.<parent domain> verifydns. < mediaservices-dns-vyöhyke >. 
    
    * Sinun on luotava toisen CNAME-tietue, joka yhdistää mukautetun isäntänimi (esimerkiksi sports.contoso.com) että Media Services StreamingEndpont's isäntänimi (esimerkiksi amstest.streaming.mediaservices.windows.net).


    Lisätietoja on artikkelissa [StreamingEndpoint] artikkelissa **CustomHostNames** -ominaisuus.

### <a id="sept_14_preview_changes"></a>Uudet ominaisuudet ja skenaariot, jotka ovat osa public preview-versio

* Reaaliaikainen esikatselu Streaming. Lisätietoja on artikkelissa [Azure Media Services Live Streaming käsitteleminen].

* Avaimen toimitus-palvelu. Lisätietoja on artikkelissa [käyttämällä AES-128 dynaaminen salaus ja avain toimituksen palvelu].

* Dynaaminen AES-salausta. Lisätietoja on artikkelissa [käyttämällä AES-128 dynaamisen salaus ja avain toimituksen palvelu].

* PlayReady käyttöoikeuden toimitus-palvelu. Lisätietoja on artikkelissa [käyttämällä PlayReady dynaaminen salaus ja käyttöoikeus toimitus-palvelun].

* PlayReady dynaaminen salausta. Lisätietoja on artikkelissa [käyttämällä PlayReady dynaaminen salaus ja käyttöoikeus toimitus-palvelun].

* Media Services-palveluiden PlayReady käyttöoikeuden malli. Lisätietoja on artikkelissa [Media Services PlayReady käyttöoikeuden mallin yleiskatsaus].

* Resurssien Streaming tallennustilan salattu. Lisätietoja on artikkelissa [Streaming tallennustilan salattu sisältö].

##<a id="august_changes_14"></a>Elokuussa 2014: n julkaisu

Kun sijoituksen koodata, tulostus-resurssi on valmistettu valmistumisen koodauksen työn. Tässä versiossa, kunnes Azure Media Services Encoder valmistettu tulosteen varat metatietoa. Syötteen varat metatietoa tuottaa myös alkaen tässä versiossa encoder. Lisätietoja on artikkeleissa [Syötteen metatietojen] ja [Tulosteen metatiedot] .


##<a id="july_changes_14"></a>Heinäkuussa 2014: n julkaisu

Seuraavat virheenkorjauksia tehtiin Azure Media Services Packagerin ja salaus:

* Vain ääni toistetaan takaisin, kun transmuxing live arkisto-resurssi, HTTP Live Streaming – tämä on korjattu ja nyt ääni ja video toistetaan.

* Kun pakkaaminen HTTP Live Streaming ja AES 128 bittistä kirjekuori salausta resurssi-pakattu virtaa ei voi toistaa takaisin Android-laitteille – Tämä ohjelmavirhe on vahvistettu ja pakattu virta toistaa Android-laitteisiin, jotka tukevat HTTP Live Streaming.

##<a id="may_changes_14"></a>Toukokuu 2014: n julkaisu

### <a id="may_14_changes"></a>Yleinen median Services päivitykset

Voit nyt tehdä stream HTTP Live Streaming (HLS) v3 [Dynaaminen pakkaus] . Virtauttaa HLS v3, lisätä seuraavanlainen origin locator polku: * .ism/manifest(format=m3u8-aapl-v3). Lisätietoja on artikkelissa [Nick Drouin blogi].

Dynaaminen pakkaus nyt myös tukee välittää HLS (v3 ja v4) salattu PlayReady perusteella tasainen Streaming nimipalvelimet salattu PlayReady. Lisätietoja siitä, miten voit salata tasainen Streaming PlayReady on artikkelissa [Protecting tasainen virta PlayReady kanssa].

### <a name="may_14_donnet_changes"></a>Media Services-palveluiden .NET SDK päivitykset

Seuraavat parannukset sisältyvät Media Services .NET SDK 3.0.0.5-versiota:

* Parempi nopeuden ja toimintakykyyn mediasisältöä lataamisesta ja lataamista varten.

* Yritä logiikka ja lyhytaikainen poikkeuksen käsittely parannukset: 

    * Lyhytkestoisia virhe tunnistaminen ja yritä logiikan on parannettu poikkeukset, jotka aiheutuvat kyselyt, muutosten tallentaminen, lataamalla ja tiedostojen lataaminen. 
    
    * Kun kuulu Web poikkeukset (esimerkiksi aikana ACS suojaustunnuksen pyyntö), huomaat, että vakava virheitä epäonnistuvat nopeammin nyt.

Lisätietoja on artikkelissa [Uudelleen funktioiden .NET Media Services SDK-paketissa].

##<a id="april_changes_14"></a>Huhtikuussa 2014 Encoder: n julkaisu

### <a name="april_14_enocer_changes"></a>Palveluiden Media Encoder päivitykset

* Tukee nyt ingesting AVI tiedostot kirjoitti saattaa laakso EDIUS nonlinear-editorin, jossa video on sipaise pakattu saattaa laakso menot/HQX pakkauksenhallinta. Lisätietoja on artikkelissa [Saattaa laakso ilmoittaa EDIUS 7 Streaming – pilveen].

* Tukee nyt määrittäminen Media Encoder luomat tiedostot nimeämiskäytäntö. Lisätietoja on artikkelissa [Hallinta Media palvelun Encoder tulosteen tiedostonimet].

* Videon tai äänen päällekkäiset lisätty tuki. Lisätietoja on artikkelissa [Luominen päällekkäiset].

* Tukee nyt stitching yhdessä videon useita osia. Lisätietoja on artikkelissa [Stitching videon osia].

* Kiinteä, jossa ääni on koodattu MPEG-1 Audio layer 3 (eli MP3) kanssa MP4s koodauksen muuntamisen liittyvät ohjelmavirhe.


##<a id="jan_feb_changes_14"></a>Tammikuu/helmikuussa 2014 julkaistut päivitykset

### <a name="jan_fab_14_donnet_changes"></a>Azure Media Services-palveluiden .NET SDK 3.0.0.1, 3.0.0.2 ja 3.0.0.3

3.0.0.1 ja 3.0.0.2 tehdyt muutokset ovat seuraavat:

* Kiinteä ongelmat liittyvät LINQ kyselyt, joilla lajittelu lauseet käyttö.

* Jaa testi ratkaisuja [GitHub] yksikkö-pohjainen testien ja tilannekohtaisia testit.

Lisätietoja muutoksista: [Azure Media Services .NET SDK 3.0.0.1 ja 3.0.0.2 julkaisut].

Seuraavat muutokset on tehty 3.0.0.3:

* Päivitetyn Azure-tallennustilan riippuvuudet, jos haluat käyttää 3.0.3.0. 

* Kiinteä 3.0 ongelman yhteensopivuus aiempien versioiden kanssa. *.* versiot. 


##<a id="december_changes_13"></a>Joulukuussa 2013-versiossa

### <a name="dec_13_donnet_changes"></a>Azure Media Services-palveluiden .NET SDK 3.0.0.0

>[AZURE.NOTE] 3.0.x.x versioiden eivät ole yhteensopivia 2.4.x.x kuuluvan.

Media Services SDK uusin versio on nyt 3.0.0.0. Voit ladata uusimman paketin Nuget tai pyydä bittien [GitHub].

Voit käyttää [Azure Active Directory Access ohjausobjektin Service (ACS)] tunnusten Media Services SDK versio 3.0.0.0 alkaen. Lisätietoja on artikkelissa [yhteyden muodostaminen .NET Media-palveluiden SDK Media-palvelun] kohdassa "Uudelleen Access ohjausobjektin palvelun tunnusten".

### <a name="dec_13_donnet_ext_changes"></a>Azure Media Services-palveluiden .NET SDK laajennukset 2.0.0.0

Azure Media Services .NET SDK-laajennukset on tunniste menetelmien ja helper toimintoja, jotka yksinkertaistaa koodin ja helpottaa kehittää Azure Media-palvelujen kanssa. Saat uusimman bittien [Azure Media Services .NET SDK-laajennusten].

##<a id="november_changes_13"></a>Marraskuussa 2013-versiossa

### <a name="nov_13_donnet_changes"></a>Azure Media Services-palveluiden .NET SDK-muutokset

Aloita tällä versiolla, .NET Media-palveluiden SDK käsittelee lyhytkestoisia vika virheitä, joita voi ilmetä, kun puheluiden soittamiseen Media Services REST API-kerrokseen.

##<a id="august_changes_13"></a>Elokuussa 2013-versiossa

### <a name="aug_13_powershell_changes"></a>Azure Sdk-työkaluja sisältyvät Media Services PowerShellin cmdlet-komennot

Seuraavat Media-palveluiden PowerShell cmdlet-komennot sisältyvät nyt [azure-sdk-Työkalut].

* Hae AzureMediaServices 

    Esimerkiksi `Get-AzureMediaServicesAccount`.

* Uusi AzureMediaServicesAccount 

    Esimerkiksi `New-AzureMediaServicesAccount -Name “MediaAccountName” -Location “Region” -StorageAccountName “StorageAccountName”`.

* Uusi AzureMediaServicesKey 

    Esimerkiksi `New-AzureMediaServicesKey -Name “MediaAccountName” -KeyType Secondary -Force`.

* Poista AzureMediaServicesAccount 

    Esimerkiksi `Remove-AzureMediaServicesAccount -Name “MediaAccountName” -Force`.

##<a id="june_changes_13"></a>Kesäkuu 2013-versiossa

### <a name="june_13_general_changes"></a>Azure Media Services muutokset

Tässä osassa muutokset ovat kesäkuussa 2013 Media Services-versioiden sisältyvät päivitykset.

* Mahdollisuus linkittää Media palvelutilin useita tallennustilan tilit. 

    StorageAccount
    
    Asset.StorageAccountName ja Asset.StorageAccount

* Mahdollisuus päivittää Job.Priority. 

* Ilmoitus liittyvien kohteiden ja ominaisuudet: 

    JobNotificationSubscription
    
    NotificationEndPoint
    
    Työn

* Asset.Uri 

* Locator.Name 

### <a name="june_13_dotnet_changes"></a>Azure Media Services .NET SDK-muutokset

Seuraavat muutokset sisältyvät kesäkuussa 2013: n Media Services SDK julkaisut. Uusimman Media Services SDK on käytettävissä GitHub.

* Kuvalla 2.3.0.0 alkaen linkittämällä useita tallennustilan Media Services SDK tukee tilejä Media Services-tilin. Seuraavat API tukevat tätä ominaisuutta:
    
    IStorageAccount tyyppi.
    
    Microsoft.WindowsAzure.MediaServices.Client.CloudMediaContext.StorageAccounts-ominaisuus.
    
    StorageAccount-ominaisuus.
    
    StorageAccountName-ominaisuus.
    
    Lisätietoja on artikkelissa [Hallinta Services mediasisältöä tallennustilan useiden eri tilien välillä].

* Ilmoitus, jotka liittyvät API. Voit kuunnella Azure jonon tallennustilan ilmoitukset on versio 2.2.0.0 alkaen. Lisätietoja on artikkelissa, [Käsittely Media Services työn ilmoitukset].
    
    Microsoft.WindowsAzure.MediaServices.Client.IJob.JobNotificationSubscriptions-ominaisuus.
    
    Microsoft.WindowsAzure.MediaServices.Client.INotificationEndPoint tyyppi.
    
    Microsoft.WindowsAzure.MediaServices.Client.IJobNotificationSubscription tyyppi.
    
    Microsoft.WindowsAzure.MediaServices.Client.NotificationEndPointCollection tyyppi.
    
    Microsoft.WindowsAzure.MediaServices.Client.NotificationEndPointType tyyppi.
    
    Microsoft.WindowsAzure.MediaServices.Client.NotificationJobState tyyppi.

* Riippuvuus Azure tallennustilan asiakkaan SDK 2.0 (Microsoft.WindowsAzure.StorageClient.dll).

* Riippuvuus OData 5.5 (Microsoft.Data.OData.dll).


##<a id="december_changes_12"></a>Joulukuuta 2012-versiossa

### <a name="dec_12_dotnet_changes"></a>Azure Media Services .NET SDK-muutokset

* IntelliSense: Lisätä erilaisia puuttuu Intellisense ohjeissa.

* Microsoft.Practices.TransientFaultHandling.Core: Kiinteät ongelma, jossa SDK oli edelleen riippuvuuden tämän kokoonpanon vanhaa versiota. SDK viittaa nyt tämän kokoonpanon 5.1.1209.1 versio.

Ratkaisuja marraskuussa 2012: n SDK havaitut ongelmat:

* IAsset.Locators.Count: Tämä Laske nyt oikein ilmoitetaan uudet IAsset liittymät kun kaikki locators on poistettu.

* IAssetFile.ContentFileSize: Tämä on nyt oikein arvo jälkeen lataus IAssetFile.Upload(filepath) mukaan.

* IAssetFile.ContentFileSize: Tämän ominaisuuden voit määrittää nyt, resurssi-tiedostoa luotaessa. Se oli aiemmin vain luku-tilassa.

* IAssetFile.Upload(filepath): Korjata ongelman missä synkronisen Lataa tämä menetelmä on yliheittävä seuraavan virheen, kun useiden tiedostojen lataaminen kohteen. Virhe: "palvelimeen ei onnistunut tarkistamiseen pyynnön. Varmista, että Authorization-otsikko arvo on muotoiltu oikein myös allekirjoitus."

* IAssetFile.UploadAsync: Kiinteät ongelma, jossa enintään 5 tiedostojen voitu ladata samanaikaisesti.

* IAssetFile.UploadProgressChanged: SDK nyt myöntämä tapahtuman.

* IAssetFile.DownloadAsync (merkkijono, BlobTransferClient, ILocator, CancellationToken): Tämä menetelmä liikaa on nyt käytettävissä.

* IAssetFile.DownloadAsync: Kiinteät ongelma, jossa enintään 5 tiedostojen voitu ladata samanaikaisesti.

* IAssetFile.Delete(): Kiinteät ongelma, jossa puheluja Poista voivat palauttaa poikkeuksen jos tiedostoa ei ladattu IAssetFile varten.

* Töiden: Kiinteät ongelma jossa ketjutus "MP4 tasainen virtaa tehtävään" ja "PlayReady suojaus tehtävän" työmallin ei luoda tehtäviä ollenkaan.

* EncryptionUtils.GetCertificateFromStore(): Tämä menetelmä ilmoittaa enää tyhjä viittaus poikkeuksen vuoksi virheet, etsiminen varmennetta varmenteen määritysongelmat perusteella.

##<a id="november_changes_12"></a>Marraskuussa 2012-versiossa

Tässä osassa muutokset on marraskuun 2012 (versio 2.0.0.0) sisältyvät päivitykset SDK. Nämä muutokset voi vaatia SDK Vapauta muokattu tai kirjoitettava uudelleen kesäkuussa 2012 esikatselu kirjoitettu koodia.

* Resurssit
    
    IAsset.Create(assetName) on vain resurssi luomista funktio. IAsset.Create Lataa tiedostoja ei enää menetelmäkutsu osana. Käytä IAssetFile ladataan.
    
    IAsset.Publish-menetelmällä ja AssetState.Publish numerointi-arvo on poistettu Services SDK-paketissa. Tunnus, jolla on riippuvainen tämän arvon on oltava uudelleen kirjoitettujen.

* FileInfo

    Tämä luokka on poistettu ja korvattu IAssetFile.

    IAssetFiles

    IAssetFile korvaa FileInfo, ja se toimii eri tavalla. Vahvistaa IAssetFiles kohdetta, ja sen käyttämällä joko Media Services SDK tai Azure-tallennustilan SDK tiedostojen-lataamisen jälkeen, kun sitä käytetään. Seuraavat IAssetFile.Upload Osastollasi voidaan käyttää:

    * IAssetFile.Upload(filePath): Synkronisen menetelmän, joka estää säikeen ja suositellaan vain, kun yksittäinen tiedosto ladataan.
    
    * IAssetFile.UploadAsync (tiedostopolku, blobTransferClient, locator, cancellationToken): asynkroninen menetelmä. Tämä on ensisijainen Lataa järjestelmä. 

    Tunnetut ohjelmavirhe: avulla cancellationToken varmasti Peruuta lataaminen; kuitenkin tehtävien peruutus-tila voi olla mikä tahansa tilojen määrä. On oikein suodattaa ja käsitellä poikkeukset.

* Locators
    
    Origin kielikohtaiset versiot on poistettu. SAS kielikohtaiset konteksti. Locators.CreateSasLocator (resurssi, accessPolicy) merkitään poistetuista tai poistaa sen GA. Kohdassa Locators uudet toiminnot-kohdassa päivitetyt tiedoista.


##<a id="june_changes_12"></a>Kesäkuussa 2012 Preview-versio

Seuraavat toiminnot on SDK marraskuun ‑version uudet ominaisuudet.

* Kohteiden poistaminen

    IAsset, IAssetFile, ILocator, IAccessPolicy, IContentKey objektit poistetaan nyt objektin tasolla, eli IObject.Delete(), sen sijaan, että edellyttävän Poista kokoelmassa, joka on cloudMediaContext.ObjCollection.Delete(objInstance).

* Locators

    Locators on nyt luotava CreateLocator-menetelmällä ja käytä LocatorType.SAS tai LocatorType.OnDemandOrigin luettelo arvoja argumenttina locator tietynlaisia haluat luoda.

    Uusia ominaisuuksia on lisätty Locators hankkia käytettävä URI sisällölle on helpompaa. Tämä Locators uudelleensuunnittelu on tarkoitus tulevien kolmannen osapuolen laajennettavuus joustavampaa ja kasvattaa helpottaminen käyttäminen media asiakassovellukset.

* Asynkroninen menetelmä tuki

    Kaikki tavat asynkroninen tuki on lisätty.


##<a name="media-services-learning-paths"></a>Media-palveluiden oppimispolut

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Palautteen antaminen

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


<!-- Anchors. -->

<!-- Images. -->

<!--- URLs. --->
[Azure Media Services-palveluiden MSDN-keskustelupalsta]: http://social.msdn.microsoft.com/forums/azure/home?forum=MediaServices
[Azure Media Services-palveluiden REST API-viittaus]: http://msdn.microsoft.com/library/azure/hh973617.aspx 
[Media Services-palveluiden hinnoittelutiedot]: http://azure.microsoft.com/pricing/details/media-services/
[Lisää metatiedot]: http://msdn.microsoft.com/library/azure/dn783120.aspx
[Tulosteen metatiedot]: http://msdn.microsoft.com/library/azure/dn783217.aspx
[Välittää sisältö]: http://msdn.microsoft.com/library/azure/hh973618.aspx
[Indeksoinnin mediatiedostojen Azure Media indeksointitoiminto kanssa]: http://msdn.microsoft.com/library/azure/dn783455.aspx
[StreamingEndpoint]: http://msdn.microsoft.com/library/azure/dn783468.aspx
[Toimimasta Azure Media-palvelujen kanssa Live Streaming]: http://msdn.microsoft.com/library/azure/dn783466.aspx
[AES-128 dynaaminen salaus ja avaimen toimitus-palvelun avulla]: http://msdn.microsoft.com/library/azure/dn783457.aspx
[PlayReady dynaaminen salaus ja käyttöoikeus toimitus-palvelun avulla]: http://msdn.microsoft.com/library/azure/dn783467.aspx
[Preview features]: http://azure.microsoft.com/services/preview/
[Media Services-palveluiden PlayReady käyttöoikeuden mallin yleiskatsaus]: http://msdn.microsoft.com/library/azure/dn783459.aspx
[Tallennustilan Streaming salattu sisältö]: http://msdn.microsoft.com/library/azure/dn783451.aspx
[Azure Classic Portal]: https://manage.windowsazure.com
[Dynaaminen pakkaus]: http://msdn.microsoft.com/library/azure/jj889436.aspx
[Nick Drouin-blogi]: http://blog-ndrouin.azurewebsites.net/hls-v3-new-old-thing/
[Tasainen virtaa, jolla PlayReady suojaaminen]: http://msdn.microsoft.com/library/azure/dn189154.aspx
[Yritä Media Services SDK funktioiden .NET varten]: http://msdn.microsoft.com/library/azure/dn745650.aspx
[Saattaa laakso ilmoittaa EDIUS 7 Streaming pilven kautta]: http://www.streamingmedia.com/Producer/Articles/ReadArticle.aspx?ArticleID=96351&utm_source=dlvr.it&utm_medium=twitter
[Palvelun ohjaaminen Media Encoder tulosteen tiedostonimet]: http://msdn.microsoft.com/library/azure/dn303341.aspx
[Päällekkäiset luominen]: http://msdn.microsoft.com/library/azure/dn640496.aspx
[Stitching videon osia]: http://msdn.microsoft.com/library/azure/dn640504.aspx
[Azure Media Services .NET SDK 3.0.0.1 ja 3.0.0.2-versioihin]: http://www.gtrifonov.com/2014/02/07/windows-azure-media-services-.net-sdk-3.0.0.2-release/
[Azure Active Directory Access Control-palvelu (ACS)]: http://msdn.microsoft.com/library/hh147631.aspx
[Yhteyden muodostaminen .NET Media-palveluihin SDK Media-palveluihin]: http://msdn.microsoft.com/library/azure/jj129571.aspx
[Azure Media Services-palveluiden .NET SDK-laajennukset]: https://github.com/Azure/azure-sdk-for-media-services-extensions/tree/dev
[Azure-sdk-Työkalut]: https://github.com/Azure/azure-sdk-tools
[GitHub]: https://github.com/Azure/azure-sdk-for-media-services
[Resurssien hallinta Media Services tallennustilan useiden eri tilien välillä]: http://msdn.microsoft.com/library/azure/dn271889.aspx
[Käsittely Media Services-työ-ilmoitukset]: http://msdn.microsoft.com/library/azure/dn261241.aspx
 
