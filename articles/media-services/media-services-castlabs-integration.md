<properties 
    pageTitle="CastLabs avulla voit pitää Widevine käyttöoikeuksien Azure Media Services | Microsoft Azure" 
    description="Tässä artikkelissa kuvataan, miten voit Azure Media Services (AMS) aikana muodossa, joka on dynaamisesti salattu AMS PlayReady ja Widevine DRMs. PlayReady käyttöoikeuden tulee Media Services PlayReady käyttöoikeuspalvelimen ja Widevine käyttöoikeuden toimitetaan castLabs käyttöoikeuspalvelimen mukaan." 
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
    ms.date="09/26/2016"  
    ms.author="Mingfeiy;willzhan;Juliako"/>


#<a name="using-castlabs-to-deliver-widevine-licenses-to-azure-media-services"></a>CastLabs avulla voit pitää Widevine käyttöoikeuksien Azure Media-palveluita

> [AZURE.SELECTOR]
- [Axinom](media-services-axinom-integration.md)
- [castLabs](media-services-castlabs-integration.md)

##<a name="overview"></a>Yleiskatsaus

Tässä artikkelissa kuvataan, miten voit Azure Media Services (AMS) aikana muodossa, joka on dynaamisesti salattu AMS PlayReady ja Widevine DRMs. PlayReady käyttöoikeuden tulee Media Services PlayReady käyttöoikeuspalvelimen ja Widevine käyttöoikeuden toimitetaan **castLabs** käyttöoikeuspalvelimen mukaan.

Toistamisen streaming sisällön suojattu CENC (PlayReady ja/tai Widevine) voit käyttää [Azure Media Player-ohjelmassa](http://amsplayer.azurewebsites.net/azuremediaplayer.html). Katso lisätietoja [AMP asiakirjan](http://amp.azure.net/libs/amp/latest/docs/) .

Seuraavassa kaaviossa näytetään korkean tason Azure Media-palveluiden ja castLabs integroinnin arkkitehtuuri.

![integrointi](./media/media-services-castlabs-integration/media-services-castlabs-integration.png)

##<a name="typical-system-set-up"></a>Tyypillinen järjestelmän määrittäminen

- AMS tallennetaan mediasisältöä.
- Sisältö näppäimet näppäimen tunnukset tallennetaan castLabs ja AMS.
- castLabs ja AMS on suojaustunnuksen käyttöoikeuksien hallinta. Seuraavissa osissa käsitellään todennus tunnukset. 
- Kun asiakaspyynnöt stream video-sisällön dynaamisesti salataan **Yhteistä salaus** (CENC) ja pakattu dynaamisesti mukaan AMS tasainen Streaming ja VIIVA. Microsoft toimittaa myös PlayReady M2TS peruskoulun stream salauksen HLS streaming-protokollaa.
- PlayReady käyttöoikeuden haetaan AMS käyttöoikeuspalvelimen ja Widevine käyttöoikeuden haetaan castLabs käyttöoikeuspalvelimen. 
- Media Playerin automaattisesti päättää, mitä käyttöoikeuden hakeaksesi asiakkaan ympäristö ominaisuuksien perusteella. 

##<a name="authentication-token-generation-for-getting-a-license"></a>Käyttöoikeuksien käytön käyttöoikeuden tunnuksen luominen

CastLabs ja AMS tukevat JWT (JSON Web suojaustunnuksen) tunnuksen muoto, jota haluat antaa käyttöoikeuden. 

###<a name="jwt-token-in-ams"></a>AMS JWT tunnus 

Seuraavassa taulukossa on kuvattu AMS JWT tunnus. 

Myöntäjä|Myöntäjän merkkijonon valitusta suojatun suojaustunnuksen Service (STS)
---|---
Käyttäjäryhmälle|Käyttäjäryhmän käytetyt STS-merkkijono
Vaateita koskevat rajoitukset|Määritä vaateita koskevat rajoitukset
NotBefore|Käynnistä tunnuksen kelpoisuus
Vanhentuu|Lopeta tunnuksen kelpoisuus
SigningCredentials|Avain, joka on jaettu PlayReady käyttöoikeuspalvelimen, castLabs käyttöoikeuspalvelimen ja STS-kesken, se voi olla symmetrisen tai julkiseen avain.

###<a name="jwt-token-in-castlabs"></a>CastLabs JWT tunnus

Seuraavassa taulukossa on kuvattu castLabs JWT tunnus. 

Nimi|Kuvaus
---|---
optData|JSON merkkijonon, joka sisältää tietoja. 
CRT|JSON merkkijonon, joka sisältää kohteen, tietoja sen tiedot ja toistaminen käyttöoikeudet.
IAT|Kausi nykyisen datetime.
jti|Yksilöllinen tunnus (joka tunnuksen voidaan käyttää vain kerran castLabs järjestelmän) tunnuksessa tietoja.

##<a name="sample-solution-set-up"></a>Esimerkki-ratkaisun määrittäminen 

[Esimerkki ratkaisu](https://github.com/AzureMediaServicesSamples/CastlabsIntegration) sisältää kaksi projektia:

-   Console-sovellus, jonka avulla voidaan DRM rajoitusten asettaminen jo nieltäväksi omaisuuden PlayReady ja Widevine.
-   Web-sovelluksen kädet ulos tunnuksia, joka saattaa näkyä STS hyvin yksinkertaistettu versio.


Jos haluat käyttää console-sovellusta:

1.  Voit muuttaa app.config määrittäminen AMS tunnistetiedot, castLabs tunnistetiedot, STS-määritys ja jaetun avaimen.
2.  Lataa sijoituksen AMS kyselyjä.
3.  Pyydä UUID ladattujen kohteiden ja muuta rivin 32 Program.cs tiedostossa:

         var objIAsset = _context.Assets.Where(x => x.Id == "nb:cid:UUID:dac53a5d-1500-80bd-b864-f1e4b62594cf").FirstOrDefault();

4.  Käytä aihetunnus nimeämiseen kohteen castLabs järjestelmä (rivin 44 Program.cs-tiedosto).

    Sinun on määritettävä aihetunnus **castLabs**; se on oltava yksilöllinen aakkosnumeerinen merkkijono.

5.  Suorita ohjelma.


Voit käyttää Web-sovelluksen (STS) seuraavasti:

1.  Muuta web.config asennuksen castlabs myyjän STS-määritys-tunnus ja jaetun avaimen.
2.  Ota käyttöön Azure sivustot.
3.  Siirry sivustoon.

##<a name="playing-back-a-video"></a>Videon toistaminen

Voit salata yleisiä salauksella (PlayReady ja/tai Widevine) videon toisto- [Azure Media Player-ohjelmassa](http://amsplayer.azurewebsites.net/azuremediaplayer.html). Suoritettaessa console-sovelluksen sisällön Key-tunnus ja luettelon URL-osoite on echoed.

1.  Avaa uudessa välilehdessä ja käynnistää oman STS: http://[yourStsName].azurewebsites.net/api/token/assetid/[yourCastLabsAssetId]/contentkeyid/[thecontentkeyid].
2.  Siirry [Azure Media Player-ohjelmassa](http://amsplayer.azurewebsites.net/azuremediaplayer.html).
3.  Liitä streaming URL-osoite.
4.  Valitse **Lisäasetukset** -valintaruutu.
5.  Valitse **Suojaus** -valikko PlayReady ja/tai Widevine.
6.  Liitä tunnuksen, joka on saatu oman STS tunnus-tekstiruutuun. 
    
    CastLab käyttöoikeuspalvelimen ei tarvita "haltijan =" etuliite tunnuksen eteen. Niin Poista, ennen kuin lähetät tunnuksen.
7.  Päivitä toisto-ohjelman.
8.  Videon toistaminen olisi.


##<a name="media-services-learning-paths"></a>Media-palveluiden oppimispolut

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Palautteen antaminen

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
