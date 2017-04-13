<properties 
    pageTitle="Azure Media Services käsitteitä | Microsoft Azure" 
    description="Tässä artikkelissa on yleiskatsaus Azure Media Services käsitteitä" 
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

#<a name="azure-media-services-concepts"></a>Azure Media Services käsitteitä 

Tässä ohjeaiheessa on yleiskuvaus tärkeimmistä Media Services-käsitteistä.

##<a id="assets"></a>Kalusto- ja tallennustilaa

###<a name="assets"></a>Resurssit

[Resurssi](https://msdn.microsoft.com/library/azure/hh974277.aspx) on digitaalinen tiedostot (mukaan lukien video, ääni, kuvia, pikkukuvan sivustokokoelmat, tekstin raidat ja tekstitys tiedostot) ja nämä tiedostot metatietoa. Kun digitaalinen tiedostoja ladataan sijoituksen kyselyjä, ne voidaan Media-palveluissa koodaus ja streaming työnkulkuja.

Sijoituksen on nyt yhdistetty blob-säilö Azure-tallennustilan tilin ja kohteen tiedostot tallennetaan kyseisen säilön BLOB.

Mitä mediasisältöä, voit ladata ja tallentaa sijoituksen valitsemisessa ottaa huomioon seuraavat asiat:

- Sijoituksen pitäisi olla vain yksi, yksilöllinen esiintymä mediasisällön. Esimerkiksi yhden muokkaaminen TV-jakson, elokuva tai mainos.
- Sijoituksen ei saa sisältää useita käännöksiä tai muokkaukset audiovisuaalisen tiedosto. Esimerkki amerikkalaisen virheellisestä käyttö yritetään tallentaa useita TV jakson, ilmoitus tai useita kameran kulmista yksittäisen tuotannon amerikkalaisen sisällä. Useiden käännöksiä tai muokkaukset audiovisuaalisen tiedosto tallennetaan sijoituksen voi aiheuttaa vaikeuksia lähettämistä koodauksen työt streaming ja suojaaminen myöhemmin työnkulussa resurssi toimittamisen.  

###<a name="asset-file"></a>Resurssi-tiedosto 
[AssetFile](https://msdn.microsoft.com/library/azure/hh974275.aspx) edustaa on todellinen video- tai tiedosto, joka on tallennettu blob-säilö. Resurssi-tiedosto on aina liitetty sijoituksen ja sijoituksen voi olla yksi tai useita tiedostoja. Media-palveluiden Encoder tehtävän epäonnistuu, jos objektin resurssi tiedosto ei ole liitetty digitaalinen tiedoston blob-säilö.

**AssetFile** esiintymän ja todellinen mediatiedosto on kaksi erillistä objektia. AssetFile esiintymää on metatietoa mediatiedosto, mediatiedosto on todellinen mediasisältöä.

Sinun ei yrittää blob säilöjen, luomat Media Services käyttämättä Media Service API sisällön muuttaminen.

###<a name="asset-encryption-options"></a>Kohteiden salausasetukset

Sen mukaan, minkä tyyppistä sisältöä haluat ladata, tallentaa ja pitää Media Services on eri salausasetukset, voit valita.

**Ei mitään** Käytetään salausta. Tämä on oletusarvo. Huomaa, että kun tämä vaihtoehto sisältöä ei ole suojattu siirrossa tai muiden tallennustilaan.

Jos haluat pitää MP4 käyttämällä asteittain lataaminen, käytä tätä vaihtoehtoa, voit ladata sisältöä.

**StorageEncrypted** – tämän asetuksen avulla voit salata käyttää paikallisesti bittistä AES 256-salausta Tyhjennä sisältö ja lataa se sitten Azuren tallennustilaan johon se on tallennettu salataan muille käyttäjille. Varat suojannut tallennustilan salaus automaattisesti salaamattomana ja sijoitetaan salattuja tiedostojärjestelmässä ennen koodaus ja halutessasi uudelleen salataan ennen lataamista takaisin kuin uuden tulostus-resurssi. Tallennustilan salauksen ensisijainen käyttötapaus on, kun haluat laadukkaita mediatiedostojen suojaaminen salauksesta osoitteessa muille käyttäjille levyn. 

Näyttää tallennustilan salattuja resurssi, jotta sinun on määritettävä kohteen toimituksen käytäntö, jotta Media Services tietää, miten haluat pitää sisältöä. Ennen kuin oman resurssi voidaan lähettää, streaming palvelimen poistaa tallennustilaa salaus ja virtauttaa sisällön (esimerkiksi AES, PlayReady tai salausta) määritetyn toimitus-käytännön avulla. 

**CommonEncryptionProtected** – Käytä tätä vaihtoehtoa, jos haluat salata (tai Lataa jo salattu) sisällöstä Yleiset-salausta tai PlayReady DRM (esimerkiksi tasainen Streaming suojattu PlayReady DRM).

**EnvelopeEncryptionProtected** – Käytä tätä vaihtoehtoa, jos haluat suojata (tai Lataa jo suojattu) HTTP Live Streaming (HLS) salattu ja salauksen AES Advanced Standard (). Huomaa, että lataat jo salattu AES HLS, jos se on salattu Transform hallinta.

###<a name="access-policy"></a>Käyttöoikeuskäytäntö 

[AccessPolicy](https://msdn.microsoft.com/library/azure/hh974297.aspx) määrittää käyttöoikeuksia (kuten luku, kirjoittaminen ja luettelon) ja keston access sijoituksen. Yleensä siirtää AccessPolicy-objektin URL-osoite, valitse käytetään käyttämään amerikkalaisen tiedostoille.


###<a name="blob-container"></a>BLOB-säilö

Blob-säilö on joukko BLOB ryhmittely. Blob-objektien säilöjen käytetään Media Services reuna-kohdan käyttöoikeuksien hallinta ja kalusto-locators jaettu Access allekirjoitus (SAS). Azure-tallennustilan tilin voi olla blob säilöjen rajoittamaton määrä. Säilön voi tallentaa BLOB rajoittamaton määrä.

>[AZURE.NOTE]Sinun ei yrittää blob säilöjen, luomat Media Services käyttämättä Media Service API sisällön muuttaminen.

###<a id="locators"></a>Locators

[Locator](https://msdn.microsoft.com/library/azure/hh974308.aspx)s on aloituskohtaa käyttämään sijoituksen tiedostoille. Käyttöoikeuskäytäntö käytetään käyttöoikeudet ja että asiakas käyttää annetulle kohteiden kesto. Locators voi olla yksi yhteys access-käytäntö, jonka siten, että eri locators tarjota eri alkamis- ja yhteystyypit eri asiakkaiden käytettäessä kaikki oikeuksien ja keston asetukset; kuitenkin vuoksi jaettujen käyttöoikeuksien käytännön rajoittaminen, määrittää Azure liittyviä palveluja, ei voi olla enintään viisi yksilöllinen locators liittyvät annetulle kohteiden samalla kertaa. 

Media-palveluiden tukee kahdentyyppisiä locators: OnDemandOrigin locators, median (kuten MPEG VIIVA, HLS tai tasainen Streaming) ja lataa asteittain media ja Suojaussidosten URL-Osoitteen locators käytettävä käytetään lataaminen media tiedostojen to\from Azure-tallennustilan. 

Huomaa, että luettelon käyttöoikeuksia (AccessPermissions.List) ei voi käyttää OrDemandOrigin locator luotaessa. 

###<a name="storage-account"></a>Tallennustilan tilin

Kaikki pääsy Azuren tallennustilaan tehdään tallennustilan tilin kautta. Media palvelutilin yhdistää yhden tai useamman tallennustilan tilit. Tilin voi olla säilöjen, rajoittamaton määrä, kunhan niiden yhteenlaskettu koko on alle 500 TT tallennustilaa tilin kohden.  Media-palvelut on SDK tason sillä, jotta voit hallita useita tallennustilan ja lataa saldo oman varat jakautumisen näissä tileissä lataamisen aikana, joka perustuu arvot tai satunnaisia jakauman. Lisätietoja on artikkelissa [Azure-tallennustilan](https://msdn.microsoft.com/library/azure/dn767951.aspx)käsitteleminen. 

##<a name="jobs-and-tasks"></a>Projektien ja tehtävien

[Työn](https://msdn.microsoft.com/library/azure/hh974289.aspx) käytetään yleensä prosessi (esimerkiksi indeksoida tai koodata) yhden äänen/videon esitykseen. Jos useiden videoiden käsitellään luoda jokaisen videon koodattava projektille.

Työn on suoritettava käsittely metatietoa. Kunkin projektin sisältää vähintään yhden [tehtävän](https://msdn.microsoft.com/library/azure/hh974286.aspx)s, jotka määrittävät atomisia käsittely-tehtävän sen syötteen varat tulosteen varat ja media-suoritin sen liittyvät asetukset. Projektin tehtäviä voit ketjuttaa, jossa yksi tehtävä tulostus-resurssi annetaan kuin syötteen kohteiden seuraavaan tehtävään. Tällä tavalla yhden työn voivat sisältää kaikki tarvittavat media esityksen käsittely.

##<a id="encoding"></a>Koodaus

Azure Media Services tarjoaa useita tapoja media pilveen koodaus.

Kun alkuvaiheessa Media-palvelujen kanssa, on tärkeää pakkauksenhallinta ja tiedostomuotoja eroista.
Pakkauksenhallinta on ohjelmisto, joka toteuttaa pakkauksen purkamisen tai algoritmeista, tiedostomuodot ovat säilöt, joissa on pakattu video.

Media-palvelut on dynaaminen pakkaus, jonka avulla voit pitää mukautuvat bittitaajuus MP4 tai tasainen Streaming koodattu sisältölähdettä streaming tukemat Media-palveluissa (MPEG VIIVA, HLS, tasainen Streaming, HDS) eikä sinun tarvitse uudelleen pakkaaminen huomioon nämä streaming muotoja.

Jos haluat hyödyntää [Dynaaminen pakkaus](media-services-dynamic-packaging-overview.md), seuraavasti:

- Mezzanine (lähde)-tiedoston koodata mukautuvat bittitaajuus MP4-tiedostoja tai mukautuvat bittitaajuus tasainen Streaming-tiedostoja (koodauksen vaiheet ovat esitellään jäljempänä tässä opetusohjelmassa) joukkoon.
- Vähintään yhden tarvittaessa streaming yksikön streaming päätepisteen, josta aiot toimituksen sisällön hakeminen Katso lisätietoja, [Voit tarvittaessa Streaming varattu aikayksikön](media-services-portal-manage-streaming-endpoints.md).

Media-palveluiden tukee seuraavia demand koodauslaitteista, joka on kuvattu artikkelissa:

- [Media Encoder vakio](media-services-encode-asset.md#media-encoder-standard)
- [Media Encoder Premium työnkulku](media-services-encode-asset.md#media-encoder-premium-workflow)

Lisätietoja tuetuista koodauslaitteista artikkelissa [koodauslaitteista](media-services-encode-asset.md).


##<a name="live-streaming"></a>Live Streaming

Azure Media Services kanavan edustaa putkijohto streaming sisällön käsittelyyn. Kanavan saa live syötteen virtaa jollakin seuraavista tavoista:

- Live paikallisen-encoder lähettää kanavan usean bittitaajuus RTMP tai tasainen Streaming (pirstoutuneet MP4). Voit käyttää seuraavia live koodauslaitteista, joka siirtää usean bittitaajuus tasainen Streaming: alkuaine, Envivion, Cisco. Seuraavat live koodauslaitteista tulosteen RTMP: Adobe Flash Live, Telestream Wirecast ja Tricaster transcoders. Nieltäväksi virtaa kulkevat kanavat ilman mitään lisäkäsittelyä. Kun pyydetty, Media Services toimittaa virta asiakkaille.

- Yksittäisen bittitaajuus stream (jollakin seuraavista muodoista: RTP (MPEG-TS))-RTMP tai tasainen Streaming (pirstoutuneet MP4)) lähetetään kanavaa, jotka on otettu käyttöön suorittamiseen live koodaus Media-palvelujen kanssa. Kanavan suorittaa sitten live koodaus saapuvien yksittäisen bittitaajuus virta usean-bittitaajuus (mukautuvat) videovirtaa. Kun pyydetty, Media Services toimittaa virta asiakkaille.

###<a name="channel"></a>Kanavan

Media-palveluiden [kanavan](https://msdn.microsoft.com/library/azure/dn783458.aspx)s vastaavat streaming sisällön käsittelyyn. Kanavan tarjoaa syötteen päätepisteen (ingest URL-osoite), valitse annat live transcoder. Kanavan vastaanottaa reaaliaikaisia syötteen virtaa live transcoder ja helpottaa käytettävissä streaming vähintään yksi StreamingEndpoints kautta. Kanavien tarjoavat myös esikatselu-päätepisteen (esikatselu URL-osoite), jonka avulla esikatselu ja Vahvista oman stream ennen lisäkäsittelyä ja toimituksen.

Voit avata ingest ja Esikatsele URL-Osoitteen kanavan luodessasi. Saat nämä URL-osoitteet-kanavan ei ole aloitettu-tilassa olevan. Kun olet valmis aloittamaan valitseminen live transcoder-tietojen tuominen kanavan, kanavan on oltava käynnissä. Kun live transcoder käynnistyy ingesting tietoja, voit esikatsella oman muodossa.

Media Services-tileille voi olla useita kanavat, useita ohjelmia ja useita StreamingEndpoints. Kaistanleveys ja suojaus-tarpeiden mukaan StreamingEndpoint services voi nimettävä vähintään yksi kanavat. Mikä tahansa StreamingEndpoint voivat hyödyntää kanavan.


###<a name="program"></a>Ohjelma

[Ohjelman](https://msdn.microsoft.com/library/azure/dn783463.aspx) avulla voit hallita julkaisu- ja live stream osia tallennustilaan. Kanavien hallinta ohjelmat. Kanavan ja ohjelma yhteyden muistuttaa perinteinen media, jossa kanavan on vakio stream sisällön ja ohjelma on määritetty kyseisen kanavan joitakin ajoitetun tapahtuma.
Voit määrittää ohjelman tallennetun sisällön säilytettävien määrittämällä **ArchiveWindowLength** -ominaisuuden tuntimäärä. Tämän arvon voi määrittää enintään 25 tuntia vähintään viiteen minuuttiin.

ArchiveWindowLength määrittää myös ajan asiakkaiden enimmäismäärän voit haku taaksepäin nykyinen reaaliaikainen merkistä lähtien. Ohjelmat voidaan suorittaa tietyn ajanjakson päälle, mutta sisältöä, joka on välin takana ikkunan pituus jatkuvasti poistetaan. Tämän ominaisuuden arvoksi myös määrittää, kuinka kauan asiakkaan luettelot voi kasvaa.

Jokainen ohjelma on liitetty sijoituksen. Voit julkaista ohjelma on luotava locator, liitetty annetaan. Ottaa tämän locator avulla voit luoda streaming URL-osoite, voit tarjota asiakkaat.

Kanavan tukee jopa kolme samanaikaisesti käynnissä olevat ohjelmat, joten voit luoda useita arkistot samalla saapuvat muodossa. Näin voit julkaista ja arkistoida tapahtuman eri osia tarpeen mukaan. Esimerkiksi business vaatimus on arkistoon ohjelman 6 tuntia, mutta vain viimeisten 10 minuutin yleislähetys. Tämän vuoksi haluat luoda kahden samanaikaisesti käynnissä olevat ohjelmat. Yksi ohjelma on määritetty arkistoon tapahtuman 6 tuntia, mutta ohjelma ei ole julkaistu. Toinen ohjelma on määritetty arkistoida 10 minuutin ja ohjelma on julkaistu.


Lisätietoja on artikkelissa:

- [Kanavien, jotka voivat suorittaa Live koodaus Azure Media-palvelujen kanssa käsitteleminen](media-services-manage-live-encoder-enabled-channels.md)
- [Kanavien, joka vastaanottaa usean bittitaajuus Live Stream paikallisen koodauslaitteista käsitteleminen](media-services-live-streaming-with-onprem-encoders.md)
- [Kiintiöiden ja rajoitukset](media-services-quotas-and-limitations.md).

##<a name="protecting-content"></a>Sisällön suojaaminen

###<a name="dynamic-encryption"></a>Dynaaminen salaus

Azure Media-palveluiden avulla voit suojata mediatiedostojen se jättää tietokoneeseen tallentaminen, käsittely ja toimittaminen ajasta. Media-palveluiden avulla voit pitää salattu dynaamisesti salaus AES (Advanced Standard) (joko 128 bittistä salausavaimet) ja yleisiä salauksen (CENC) käyttämällä PlayReady ja/tai Widevine DRM sisältöä. Media-palveluiden tarjoaa myös palvelun välittää AES näppäimet ja PlayReady käyttöoikeuksien valtuutettujen asiakkaille.

Tällä hetkellä voit salata streaming seuraavissa muodoissa: HLS, MPEG VIIVA ja tasainen Streaming. Et voi salata streaming format HDS tai progressiivinen lataaminen.

Jos haluat salata amerikkalaisen Media-palveluiden, joudut salausavain (CommonEncryption tai EnvelopeEncryption) liittäminen oman resurssi ja määritettävä luvan käytännöt näppäimen.

Jos haluat käyttää tallennustilan salattuja resurssi, sinun on määritettävä kohteen toimituksen käytännön haluat määrittää, miten haluat pitää yhteyttä resurssi.

Kun stream pyytämän pelaaja-Media Services käyttää määritetyn avaimen salaamiseen dynaamisesti kirjekuori salaus (ja AES) tai yleisten salausta (PlayReady tai käyttöä Widevine) sisällön. Purkaa virta player pyytää avain avaimen toimitus-palvelusta. Päättää, onko käyttäjällä oikeuksia hankkia avain-palvelun arvioi luvan käytäntöjä, jonka olet määrittänyt näppäimen.


###<a name="token-restriction"></a>Suojaustunnuksen rajoitus

Sisällön avaimen todennus-käytäntö voi olla yksi tai useampi käyttöoikeuksien rajoitukset: Avaa, tunnussanoma rajoitus tai IP-rajoituksia. Suojaustunnuksen rajoitettu käytäntö on liitettävä mukaan suojattu suojaustunnuksen Service (STS) antanut tunnusta. Media-palveluiden tukee tunnusten yksinkertaisen Web tunnusten (SWT)-muodossa ja JSON Web suojaustunnuksen (JWT)-muodossa. Media-palveluiden ei tarjoa suojatun suojaustunnuksen palvelut. Voit luoda mukautetun STS tai hyödyntää Microsoft Azure ACS ongelman tunnukset. Luoda määritetyn avaimen ja ongelman saatavat, jonka olet määrittänyt suojaustunnuksen rajoituksen määritys on allekirjoitettu tunnuksen on oltava määritettynä STS. Avaimen toimituksen Media palvelut palauttaa pyydettyjen key (tai käyttöoikeus) asiakas, jos tunnus on kelvollinen ja saatavat suojaustunnuksen vastaavat toisiaan ne määritetty key (tai käyttöoikeus).

Kun käytäntö määritetään tunnuksen rajoitettu, ensisijainen vahvistus-näppäintä, myöntäjä ja yleisön parametrit on määritettävä. Ensisijainen vahvistus-avain sisältää avain, joka on allekirjoitettu tunnuksen, myöntäjä on suojattu suojaustunnuksen palvelu, joka ongelmat tunnuksen. (Kutsutaan joskus laajuus) yleisön kuvataan tunnuksen palveluita tai resurssin tunnus sallii käyttöoikeus. Media-palveluiden avaimen toimitus-palvelu tarkistaa tunnuksen kyseiset arvot vastaavat arvot mallissa.

Lisätietoja on seuraavissa artikkeleissa:

[Suojaa sisällön yleiskuvaus](media-services-content-protection-overview.md)
[Suojaa kanssa AES-128](media-services-protect-with-aes128.md)
[Suojaa DRM kanssa](media-services-protect-with-drm.md)

##<a name="delivering"></a>Välittää

###<a id="dynamic_packaging"></a>Dynaaminen pakkaus

Kun käsittelet Media Services on suositeltavaa koodata mezzanine tiedostojen siirtäminen mukautuvat bittitaajuus MP4 määrittäminen ja muuntaa joukon haluamasi [Dynaaminen pakkaus](media-services-dynamic-packaging-overview.md)-muodossa.


###<a name="streaming-endpoint"></a>Päätepisteen Streaming

StreamingEndpoint edustaa streaming-palvelu, joka voi toimittaa sisältöä suoraan Playerin asiakassovellus, tai voit sisällön toimituksen verkon (CDN) edelleen jakauman (Azure Media Services on nyt Azure CDN integrointi.) Lähtevän stream StreamingEndpoint-palvelusta voi olla live stream tai videon Media Services-tilin kohteiden pyydettäessä. Lisäksi voit hallita StreamingEndpoint-palvelun kapasiteetti käsittelemään kasvava kaistanleveyden tarpeiden säätämällä aikayksikön (tunnetaan myös nimellä streaming yksiköt). On suositeltavaa varata vähintään yksi aikayksikön tuotantoympäristössä sovellukset. Aikayksikön antaa sinulle erillinen juniin kapasiteetin, joka voi ostaa 200 Mbps kokoisissa ja muita toimintoja, joka sisältää tällä hetkellä käytössä Dynaaminen pakkaus.

On suositeltavaa Dynaaminen pakkaus ja dynaaminen salausta. Jos haluat käyttää näitä ominaisuuksia, on oltava vähintään yksi streaming yksikkö päätepiste, jonka aiot käyttää. Lisätietoja on artikkelissa [Skaalaus streaming yksiköt](media-services-portal-manage-streaming-endpoints.md).

Oletusarvon mukaan käytössä voi olla enintään 2 streaming Media Services-tilin päätepisteet. Voit pyytää suurempi raja-kohdassa [kiintiöiden ja rajoitukset](media-services-quotas-and-limitations.md).

On vain laskuttaa oman StreamingEndpoint ollessa käynnissä tila.

###<a name="asset-delivery-policy"></a>Kohteiden toimituksen käytäntö

Seuraavien vaiheittaisten ohjeiden Media Services sisällön lähettämisen työnkulun on määrittäminen [resurssien toimituksen käytännöt ](https://msdn.microsoft.com/library/azure/dn799055.aspx), jotka haluat voidaan lähettää. Kohteiden toimituksen käytännön kertoo Media Services siitä, miten haluat, että annetaan toimitetaan: kyselyjä mitä streaming protokolla olisi oman resurssi voidaan dynaamisesti pakattu (esimerkiksi MPEG VIIVA, HLS, tasainen Streaming, tai kaikki), onko haluat salata dynaamisesti oman resurssi ja miten (kirjekuori tai yleisten salaus).

Jos sinulla on tallennustilan salattuja resurssi, ennen kuin oman resurssi voidaan lähettää, streaming palvelimen poistaa tallennustilaa salaus ja virtauttaa määritetyn toimitus-käytännön avulla sisältöä. Esimerkiksi pitää yhteyttä resurssi salattu salaus AES (Advanced Standard) salausavaimen, Määritä DynamicEnvelopeEncryption käytännön tyyppi. Tallennustilan salauksen poistaminen ja poista annetaan virtauttaa, Määritä käytäntö tyypiksi NoDynamicEncryption.

###<a name="progressive-download"></a>Nousevan lataaminen

Nousevan Lataa avulla voit aloittaa toistamisessa, ennen kuin koko tiedosto on ladattu. Voit ladata vain asteittain MP4-tiedosto.

Huomaa, että salattuja kalusto purkaa niiden ladattavissa asteittain halutessasi.

Antaa käyttäjien asteittain Lataa URL-osoitteet, ensin luotava OnDemandOrigin locator. Luominen locator avulla voit polun kohteen. Tarvitset sitten liittäminen MP4-tiedoston nimi. Esimerkki:

http://amstest1.Streaming.mediaservices.Windows.NET/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4

###<a name="streaming-urls"></a>Streaming URL-osoitteet

Streaming sisältöä asiakkaille. Voit antaa käyttäjien streaming URL-osoitteet, ensin on luotava OnDemandOrigin locator. Luominen locator avulla voit polun resurssi, jossa haluat käyttää sisältöä. Kuitenkin haluat, että voit käyttää tätä sisältöä Muokkaa Tämä polku. Muodostaa streaming luettelon tiedoston täydellinen URL-osoite on KETJUTA locator polku arvo ja luettelo (filename.ism) tiedostonimi. Liitä sitten locator polku /Manifest ja oikeassa muodossa (tarvittaessa).

Voit myös käyttää sisältöä SSL-yhteyttä. Toiminto, varmista, että streaming URL-osoitteet alkavat HTTPS.

Huomaa, että voit vain virtauttaa SSL-yhteyden kautta, jos streaming päätepisteen, josta pidät sisältöä on luotu 10. syyskuussa 2014 jälkeen. Jos luotu jälkeen syyskuu 10 streaming päätepisteet perustuvat streaming URL-osoitteet, URL-osoite sisältää "streaming.mediaservices.windows.net" (uusi muoto). Streaming URL-osoitteet, jotka sisältävät "origin.mediaservices.windows.net" (vanha format) eivät tue SSL. Jos haluat, että voit käyttää SSL: n URL-osoite on vanha muodossa, Luo uusi streaming päätepiste. URL-osoitteet, Luo uusi streaming päätepiste perusteella avulla voit lähettää sisältöä SSL-yhteyden kautta.

Seuraavassa luettelossa kuvataan eri streaming muodot ja esimerkkejä:

- Tasainen Streaming

{streaming päätepisteen nimi media-palveluiden tilin name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8b70-203e86b0df58/BigBuckBunny.ISM/manifest


- MPEG-VIIVA

{streaming päätepisteen nimi media-palveluiden tilin name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8b70-203e86b0df58/BigBuckBunny.ISM/manifest(Format=MPD-Time-CSF)



- Apple HTTP Live Streaming (HLS) V4

{streaming päätepisteen nimi media-palveluiden tilin name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8b70-203e86b0df58/BigBuckBunny.ISM/manifest(Format=m3u8-aapl)



- Apple HTTP Live Streaming (HLS) V3

{streaming päätepisteen nimi media-palveluiden tilin name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl-v3)

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8b70-203e86b0df58/BigBuckBunny.ISM/manifest(Format=m3u8-aapl-v3)

- HDS (for Adobe PrimeTime/Access asiakirjamalleja vain)

{streaming päätepisteen nimi media-palveluiden tilin name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=f4m-f4f)

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8b70-203e86b0df58/BigBuckBunny.ISM/manifest(Format=f4m-f4f)


##<a name="media-services-learning-paths"></a>Media-palveluiden oppimispolut

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Palautteen antaminen

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
