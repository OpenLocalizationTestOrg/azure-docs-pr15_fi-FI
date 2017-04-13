<properties 
    pageTitle="Sisällön yleiskuvaus suojaaminen | Microsoft Azure" 
    description="Tässä artikkelissa antaa yleiskuvaus sisällön suojauksen Media-palvelujen kanssa." 
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
    ms.date="09/27/2016" 
    ms.author="juliako"/>

#<a name="protecting-content-overview"></a>Sisällön yleiskuvaus suojaaminen


Microsoft Azure Media Services-palvelujen avulla voit suojata mediatiedostojen se jättää tietokoneeseen tallentaminen, käsittely ja toimittaminen ajasta. Media-palveluiden avulla voit pitää salattu dynaamisesti salaus AES (Advanced Standard) (joko 128 bittistä salausavaimet) tai mitä tahansa pää DRMs suorien ja tarvittaessa sisältöä: Microsoft PlayReady Google Widevine ja Apple FairPlay. Media-palvelut sekä välittää AES näppäimet ja DRM palvelu (PlayReady, Widevine ja FairPlay) käyttöoikeuksia valtuutettujen asiakkaille. 

Seuraavassa kuvassa näytetään, joka tukee AMS sisällön suojauksen työnkulut. 

![PlayReady suojaaminen](./media/media-services-content-protection-overview/media-services-content-protection-with-multi-drm.png)

>[AZURE.NOTE]Jos dynaaminen salausta, sinun täytyy saada vähintään yhden streaming varattu yksikön streaming päätepisteen, josta haluat lähettää salatun sisällön.

Tässä ohjeaiheessa kerrotaan [ja termeistä](media-services-content-protection-overview.md) tietoja AMS sisällön suojaaminen kiinnostavat. Aihe sisältää myös [linkkejä](media-services-content-protection-overview.md#common-scenarios) ohjeaiheisiin, joka näyttää, miten sisällön suojauksen tehtävät saavuttamiseksi. 

##<a name="dynamic-encryption"></a>Dynaaminen salaus

Microsoft Azure Media Services-palvelujen avulla voit pitää salattu dynaamisesti AES Poista avaimen tai DRM salausta sisältöä: Microsoft PlayReady Google Widevine ja Apple FairPlay.

Tällä hetkellä voit salata streaming seuraavissa muodoissa: HLS, MPEG VIIVA ja tasainen Streaming. Et voi salata streaming format HDS tai progressiivinen lataaminen.

Media-palveluiden salaamiseen amerikkalaisen halutessasi joudut salausavain (CommonEncryption tai EnvelopeEncryption) liittäminen oman resurssi ja määritettävä luvan käytännöt näppäimen.

Haluat myös kohteen toimituksen käytännön määrittäminen. Jos haluat käyttää tallennustilan salattuja resurssi, varmista, että voit määrittää, miten haluat pitää resurssi toimituksen käytännön määrittäminen.

Kun stream pyytämän pelaaja-Media Services käyttää määritetyn avaimen salaamiseen dynaamisesti sisältöä AES Poista avaimen avulla tai DRM salausta. Purkaa virta player pyytää avain avaimen toimitus-palvelusta. Päättävän riippumatta siitä, onko käyttäjällä on oikeus hankkia avain palvelun arvioi luvan käytäntöjä, jonka olet määrittänyt näppäimen.

>[AZURE.NOTE]Dynaaminen salaus hyödyntää, sinun täytyy saada vähintään yhden tarvittaessa streaming yksikön streaming päätepisteen, josta aiot toimituksen salatun sisällön. Katso lisätietoja, [miten asteikko Media Services](media-services-portal-manage-streaming-endpoints.md).

##<a name="storage-encryption"></a>Tallennustilan salaus

Tallennustilan salauksen avulla voit salata käyttää paikallisesti bittistä AES 256-salausta Tyhjennä sisältö ja lataa se sitten Azuren tallennustilaan johon se on tallennettu salataan muille käyttäjille. Varat suojannut tallennustilan salaus automaattisesti salaamattomana ja sijoitetaan salattuja tiedostojärjestelmässä ennen koodaus ja halutessasi uudelleen salataan ennen lataamista takaisin kuin uuden tulostus-resurssi. Tallennustilan salauksen ensisijainen käyttötapaus on, kun haluat laadukkaita mediatiedostojen suojaaminen salauksesta osoitteessa muille käyttäjille levyn.

Näyttää tallennustilan salattuja resurssi, jotta sinun on määritettävä kohteen toimituksen käytäntö, jotta Media Services tietää, miten haluat pitää sisältöä. Ennen kuin oman resurssi voidaan lähettää, streaming palvelimen poistaa tallennustilaa salaus ja virtauttaa sisällön (esimerkiksi AES, Yleiset salauksen tai salausta) määritetyn toimitus-käytännön avulla.

## <a name="common-encryption-cenc"></a>Yleisiä salaus (CENC)

Yleisiä salausta käytetään, kun salaaminen kanssa PlayReady sisältöä tai / ja Widewine.

## <a name="using-cbcs-aapl-encryption"></a>Cbcs aapl salauksen avulla

Cbcs aapl käytetään, kun salaaminen sisällön FairPlay kanssa.

## <a name="envelope-encryption"></a>Kirjekuori-salaus 

Käytä tätä vaihtoehtoa, jos haluat suojata sisältöä AES-128 Tyhjennä avaimella. Jos haluat turvallisempi vaihtoehto, valitse tässä aiheessa lueteltuja jokin DRMs. 

##<a name="licenses-and-keys-delivery-service"></a>Käyttöoikeudet ja avaimet toimitus-palvelu

Media-palveluiden tarjoaa palvelun välittää DRM (PlayReady, Widevine, FairPlay) käyttöoikeuksien ja AES Poista näppäimet valtuutettujen asiakkaille. Voit käyttää [Azure portaalin](media-services-portal-protect-content.md), REST API tai Media Services SDK .NET määrittämiseen todennus ja käyttöoikeuksien käytännöt käyttöoikeuksia ja avaimet.

##<a name="token-restriction"></a>Suojaustunnuksen rajoitus

Sisällön avaimen todennus-käytäntö voi olla yksi tai useampi käyttöoikeuksien rajoitukset: Avaa tai tunnussanoma rajoitus. Suojaustunnuksen rajoitettu käytäntö on liitettävä mukaan suojattu suojaustunnuksen Service (STS) antanut tunnusta. Media-palveluiden tukee tunnusten yksinkertaisen Web tunnusten (SWT)-muodossa ja JSON Web suojaustunnuksen (JWT)-muodossa. Media-palveluiden ei tarjoa suojatun suojaustunnuksen palvelut. Voit luoda mukautetun STS tai hyödyntää Microsoft Azure ACS ongelman tunnukset. Luoda määritetyn avaimen ja ongelman saatavat, jonka olet määrittänyt suojaustunnuksen rajoitus määritys on allekirjoitettu tunnuksen on oltava määritettynä STS. Avaimen toimituksen Media palvelut palauttaa pyydettyjen key (tai käyttöoikeus) asiakas, jos tunnus on kelvollinen ja saatavat suojaustunnuksen vastaavat toisiaan ne määritetty key (tai käyttöoikeus).

Kun käytäntö määritetään tunnuksen rajoitettu, ensisijainen vahvistus-näppäintä, myöntäjä ja yleisön parametrit on määritettävä. Ensisijainen vahvistus-avain sisältää avain, joka on allekirjoitettu tunnuksen, myöntäjä on suojattu suojaustunnuksen palvelu, joka ongelmat tunnuksen. (Kutsutaan joskus laajuus) yleisön kuvataan tunnuksen palveluita tai resurssin tunnus sallii käyttöoikeus. Media-palveluiden avaimen toimitus-palvelu tarkistaa tunnuksen kyseiset arvot vastaavat arvot mallissa.

##<a name="streaming-urls"></a>Streaming URL-osoitteet

Jos yhteyttä resurssi on salattu useita DRM, kannattaa käyttää salaus-tunniste streaming URL: (muoto = 'm3u8-aapl' salaus = "xxx").

Ottaa huomioon seuraavat asiat:

- Vain nolla tai yksi salaustyyppi voidaan määrittää.
- Salauksen tyyppi ei tarvitse erikseen URL-osoite, jos vain yksi salaus käytetty kohteen.
- Salauksen tyyppi on kirjainkokoa.
- Salauksen seuraavanlaisia voidaan määrittää:  
    - **cenc**: yleisiä salaus (Playready tai Widevine)
    - **cbcs aapl**: Fairplay
    - **CBC**: Kirjekuori AES-salausta.

##<a name="common-scenarios"></a>Yleisiä tilanteita, joissa

Seuraavat ohjeaiheet näytetään, miten tallennustilan sisällön suojaaminen, pitää dynaamisesti salattuja streaming media, AMS avain/käyttöoikeuden toimitus-palvelun käyttöä

- [AES suojaaminen](media-services-protect-with-aes128.md) 
- [PlayReady ja/tai Widevine suojaaminen](media-services-protect-with-drm.md)
- [Virtauttaa HLS-sisällön suojatun/tai Apple FairPlay PlayReady](media-services-protect-hls-with-fairplay.md)

### <a name="additional-scenarios"></a>Lisää skenaarioita

- [Miten voit integroida Azure PlayReady käyttöoikeuden palvelun oman salaus/streaming-palvelimen kanssa](http://mingfeiy.com/integrate-azure-playready-license-service-encryptorstreaming-server).
- [CastLabs avulla voit pitää DRM käyttöoikeuksien Azure Media-palveluita](media-services-castlabs-integration.md)
 
##<a name="media-services-learning-paths"></a>Media-palveluiden oppimispolut

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Palautteen antaminen

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="related-links"></a>Aiheeseen liittyvät linkit

[Sähköposteja PlayReady ja dynaaminen AES-salausta Azure Media-palvelujen kanssa](http://mingfeiy.com/playready)

[Azure Media Services PlayReady käyttöoikeuden toimituksen hinnat kuvaus](http://mingfeiy.com/playready-pricing-explained-in-azure-media-services)

[Voit korjata AES varten salattu stream Azure Media Services-palveluissa](http://mingfeiy.com/debug-aes-encrypted-stream-azure-media-services)

[JWT suojaustunnuksen authenitcation](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/)

[Integroi Azure Media Services OWIN MVC perusteella app Azure Active Directory-hakemistosta ja rajoittaa sisällön avaimen toimittamisen JWT saatavat perusteella](http://www.gtrifonov.com/2015/01/24/mvc-owin-azure-media-services-ad-integration/).

[Käytä Azure ACS ongelman tunnukset](http://mingfeiy.com/acs-with-key-services).

[content-protection]: ./media/media-services-content-protection-overview/media-services-content-protection.png
