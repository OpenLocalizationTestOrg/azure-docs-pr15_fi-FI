<properties 
    pageTitle="Videosoittimen sovellusten kehittämiseen" 
    description="Aiheen linkkejä Playerin kehysten ja laajennukset, joiden avulla voit kehittää oman asiakassovellukset, jotka voivat käyttää streaming mediatiedostoja, kuten Media-palveluita." 
    authors="Juliako" 
    manager="erikre" 
    editor="" 
    services="media-services" 
    documentationCenter=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016"
    ms.author="juliako"/>


#<a name="develop-video-player-applications"></a>Videosoittimen sovellusten kehittämiseen

##<a name="overview"></a>Yleiskatsaus

Azure Media-palvelut on työkaluja, sinun on luotava dynaamisten Playerin asiakassovelluksissa useimmat ympäristöjen, mukaan lukien: iOS-laitteita, Android-laitteille, Windowsin, Windows Phone-, Xbox ja TV-ruutuihin. Tässä artikkelissa on linkkejä myös SDK: T ja Playerin kehyksiä, jotka voit kehittää oman asiakas-sovelluksia, jotka voivat käyttää streaming mediatiedostoja, kuten Azure Media Services.

##<a name="azure-media-player"></a>Azure Media Player-ohjelmassa

[Azure Media Player-ohjelmassa](http://aka.ms/ampinfo) on web-videosoittimen valmista toistaa mediasisältöä Microsoft Azure Media Servicesistä erilaisia selaimet ja laitteet. Azure Media Playerin käyttämällä yleisesti käytettyjen, kuten HTML5, Media lähde laajennukset (MSE) ja salattu Media laajennukset (EME) antamaan rikastetun mukautuvat streaming sisäänrakennetun. Kun nämä vaatimukset eivät ole käytettävissä laitteessa tai selaimessa, Azure Media Player käyttää Flash- ja Silverlight varakyselyjen tekniikka. Kehittäjät on yhdistetty JavaScript-liittymän, voit käyttää API toisto-tekniikka, jolla, riippumatta. Näin served Azure Media-palveluissa sisältöä, joka toistetaan arvoalueella wide-laitteet ja selaimet ilman mitään ylimääräisiä työmäärään.

Microsoft Azure Media Services sallii sisällön voi served VIIVA, tasainen Streaming ja HLS streaming muodot takaisin sisällön toistaminen. Azure Media Playerin otetaan huomioon nämä eri muodoissa, ja toistaa automaattisesti parhaat linkin ympäristö/selaimen-ominaisuuksien perusteella. Microsoft Azure Media Services mahdollistaa myös dynaaminen salauksen omaisuuden PlayReady salauksella tai AES-128 bitti kirjekuori-salausta. Azure Media Playerin mahdollistaa PlayReady salauksen ja AES-128 bitti salatun sisällön, kun määritetty oikein. 

Lisätietoja:

- [Azure Media Player-ohjelmassa](http://aka.ms/ampinfo)
- [Azure Media Player-asiakirjat](http://aka.ms/ampdocs) 
- [Azure Media Playerin käytön aloittaminen-blogi](https://azure.microsoft.com/blog/2015/04/15/announcing-azure-media-player/)
- [Pysyt ajan tasalla Azure Media Playerin päivään Rekisteröidy](http://aka.ms/ampsignup)
- [Lisää uusi ehdottaa ominaisuuksia, ideoita ja palaute](http://aka.ms/ampuservoice ) 


##<a name="other-tools-for-creating-player-applications"></a>Muita työkaluja Player-sovellusten luominen

Voit myös käyttää jokin seuraavista SDK: T:

- [Tasaiset Streaming asiakkaan SDK](http://www.iis.net/downloads/microsoft/smooth-streaming) 
- [Tasainen Streaming Windows-kaupan sovellus](media-services-build-smooth-streaming-apps.md)
- [Microsoft Media-ympäristöä: Playerin Framework](http://playerframework.codeplex.com/) 
- [HTML5 Soittimen Framework ohjeissa](http://playerframework.codeplex.com/wikipage?title=HTML5%20Player&referringTitle=Documentation) 
- [Microsoft tasainen Streaming laajennuksen OSMF varten](https://www.microsoft.com/download/details.aspx?id=36057) 
- [Käyttöoikeuksien myöntämistä Microsoft® tasainen Streaming asiakkaan sovittaminen Kit](http://aka.ms/sspk) 
- [XBOX videon sovellusten kehittämisen](http://xbox.create.msdn.com/) 
 

##<a name="advertising"></a>Mainonta

Azure Media Services tukee ad kohdistimen kautta Windows Media-ympäristössä: Playerin puitteissa. Windows 8, Silverlight, Windows Phone 8 ja iOS laitteisiin saatavilla olevia Playerin kehysten ad-tukeen. Kunkin Playerin framework on otoksen koodi, joka näytetään, miten voit toteuttaa toisto-ohjelmaa. On kolme erilaista, voit lisätä mediatiedostojen mainokset:

Lineaarinen – koko kehyksen mainos, joka pysäyttää tärkeimmät videon

Nonlinear – kerros Active Directory, jotka näkyvät, kun tärkeimmät video toistetaan, yleensä logon tai muita sijoittaa Playerin staattisen kuvan

Avustaja – Active Directory, jotka näkyvät Playerin ulkopuolella

Active Directory voi asettaa milloin tahansa tärkeimmät video aikajana. Toisto-ohjelman täytyy ilmoittaa, milloin toistetaan mainos ja mitkä mainosten toistamiseen. Tämä on valmis käyttämällä vakio XML-pohjaisia tiedostoja: Video Ad palvelun malli (VAST), digitaalisen videon useita Ad soittoluettelo (VMAP), Media abstraktit jaksotetaan malli (LAPPU) ja digitaalisen videon toisto-ohjelman Ad käyttöliittymän määritys (VPAID). SUURIA tiedostoja Määritä, mitä mainosten näyttämiseen. VMAP tiedostot Määritä toiminnon toistaminen eri mainosten ja suuria XML sisältää. LAPPU tiedostot on sarjan Active Directory, joka voi sisältää myös suuria XML tapa. VPAID tiedostot Määritä käyttöliittymään videon toisto-ohjelman ja ad tai ad-palvelin. Lisätietoja on artikkelissa [Lisääminen mainosten](https://msdn.microsoft.com/library/dn387398.aspx).

Lisätietoja suljettu Live streaming videoiden tekstitys ja mainosten tuki on artikkelissa [Tuetut suljettu tekstitys ja Ad kohdistimen standardit](https://msdn.microsoft.com/library/c49e0b4d-357e-4cca-95e5-2288924d1ff3#caption_ad).


##<a name="media-services-learning-paths"></a>Media-palveluiden oppimispolut

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Palautteen antaminen

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="see-also"></a>Katso myös

[MPEG-VIIVA mukautuvat videoiden upottaminen DASH.js HTML5-sovellukseen](media-services-embed-mpeg-dash-in-html5.md)

[GitHub dash.js säilöön](https://github.com/Dash-Industry-Forum/dash.js)
 
