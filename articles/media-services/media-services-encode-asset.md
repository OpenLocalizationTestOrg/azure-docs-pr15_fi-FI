<properties 
    pageTitle="Yleiskatsaus ja vertailu demand media koodauslaitteista Azure | Microsoft Azure" 
    description="Tässä ohjeaiheessa antaa yleiskatsaus ja vertailu demand media koodauslaitteista Azure." 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
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

#<a name="overview-and-comparison-of-azure-on-demand-media-encoders"></a>Yleiskatsaus ja vertailu demand media koodauslaitteista Azure

##<a name="encoding-overview"></a>Koodauksen yleiskatsaus

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

Tässä artikkelissa kerrotaan lyhyesti pyydettäessä media koodauslaitteista sekä linkkejä artikkeleihin, joissa on tarkempia tietoja. Tässä aiheessa on myös koodauslaitteista vertailussa.

Huomaa, että oletusarvoisesti Media Services-tileille voi olla yksi aktiivisen koodauksen tehtävän kerrallaan. Voit varata koodauksen yksiköiden, jotta voit on käynnissä samanaikaisesti, yksi jokaista koodauksen varattu yksikköä ostat useita koodauksen tehtäviä. Lisätietoja on artikkelissa [Skaalaus koodaus yksiköt](media-services-scale-media-processing-overview.md).

##<a name="media-encoder-standard"></a>Media Encoder vakio

###<a name="how-to-use"></a>Käyttäminen

[Voit salata kanssa Media Encoder vakio](media-services-dotnet-encode-with-media-encoder-standard.md)

###<a name="formats"></a>Muodot

[Muotoilut ja pakkauksenhallinnat](media-services-media-encoder-standard-formats.md)

###<a name="presets"></a>Esiasetukset

Media Encoder vakio määritetään Käytä jotakin seuraavista encoder Esiasetukset kuvattu [seuraavassa](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409).

###<a name="input-and-output-metadata"></a>Syöttö- ja metatiedot

Koodauslaitteista näytettävä metatiedot on kuvattu [seuraavassa](http://msdn.microsoft.com/library/azure/dn783120.aspx).

Koodauslaitteista tulosteen metatiedot on kuvattu [seuraavassa](http://msdn.microsoft.com/library/azure/dn783217.aspx).

###<a name="generate-thumbnails"></a>Luo pikkukuvat

Lisätietoja [siitä, miten luodaan käyttämällä Media Encoder vakio pikkukuvat](media-services-custom-mes-presets-with-dotnet.md#thumbnails).

###<a name="trim-videos-clipping"></a>Rajaa videot (näyttöleikkeen)

Lisätietoja [rajaamisesta videoita Media Encoder vakio](media-services-custom-mes-presets-with-dotnet.md#trim_video).

###<a name="create-overlays"></a>Päällekkäiset luominen

Lisätietoja on artikkelissa [luominen käyttämällä Media Encoder vakio päällekkäiset](media-services-custom-mes-presets-with-dotnet.md#overlay).

###<a name="see-also"></a>Katso myös

[Media Services-blogi](https://azure.microsoft.com/blog/2015/07/16/announcing-the-general-availability-of-media-encoder-standard/)
 
##<a name="media-encoder-premium-workflow"></a>Media Encoder Premium työnkulku

###<a name="overview"></a>Yleiskatsaus

[Esittely Premium koodaus Azure Media-palvelut](https://azure.microsoft.com/blog/2015/03/05/introducing-premium-encoding-in-azure-media-services/)

###<a name="how-to-use"></a>Käyttäminen

Media Encoder Premium työnkulku on määritetty monimutkaisia työnkulkujen käyttämisen. Työnkulun tiedostot onnistunut ja päivittää [Työnkulun suunnittelu](media-services-workflow-designer.md) -työkalun avulla.

[Opi käyttämään Premium koodaus Azure Media-palvelut](https://azure.microsoft.com/blog/2015/03/06/how-to-use-premium-encoding-in-azure-media-services/)

###<a name="known-issues"></a>Tunnetut ongelmat

Jos syötteen video ei sisällä tekstitys, tulos resurssi edelleen sisältää tyhjän TTML tiedoston. 


##<a id="compare_encoders"></a>Vertaa koodauslaitteista

###<a id="billing"></a>Kunkin encoder käyttämä Laskutus-mittari

Media-suoritin nimi|Sovellettava hinnat|Huomautuksia
---|---|---
**Media Encoder vakio** |ENCODER|Tehtävät-koodaus veloitetaan tulosteen koon mukaan annetaan GBytes määritetty korko [tähän][1], ENCODER-sarakkeessa.
**Media Encoder Premium työnkulku** |PREMIUM ENCODER|Tehtävät-koodaus veloitetaan tulosteen koon mukaan annetaan GBytes määritetty korko [tähän][1], PREMIUM ENCODER-sarakkeessa.


Tässä osassa vertaa **Media Encoder vakio** - ja **Media Encoder Premium työnkulun**koodauksen ominaisuuksia.


###<a name="input-containerfile-formats"></a>Lisää säilö/tiedostomuodot

Lisää säilö/tiedostomuodot|Media Encoder vakio|Media Encoder Premium työnkulku
---|---|---
Adobe® Flash® F4V           |Kyllä|Kyllä
MXF/SMPTE 377M              |Kyllä|Kyllä
GXF                         |Kyllä|Kyllä
MPEG-2 Transport virtaa    |Kyllä|Kyllä
MPEG-2-ohjelman virtaa      |Kyllä|Kyllä
MPEG-4/MP4                  |Kyllä|Kyllä
Windows Media/ASF           |Kyllä|Kyllä
AVI (pakkaamattomat 8-bittinen/10 bittiä)|Kyllä|Kyllä
3GPP/3GPP2                  |Kyllä|Ei
Smooth Streaming-tiedostomuoto (PIFF 1.3)|Kyllä|Ei
[Microsoft Digital Video Recording(DVR-MS)](https://msdn.microsoft.com/library/windows/desktop/dd692984)|Kyllä|Ei
Matroska/WebM               |Kyllä|Ei
QuickTime (.mov) |Kyllä|Ei

###<a name="input-video-codecs"></a>Kirjoita videosisällön pakkauksenhallinnat

Kirjoita videosisällön pakkauksenhallinnat|Media Encoder vakio|Media Encoder Premium työnkulku
---|---|---
AVC 8-bittinen/10-bittinen ylöspäin 4:2:2, mukaan lukien AVCIntra   |8-bittinen 4:2:0 – 4 / 2:2|Kyllä
Avid DNxHD (-MXF)                                 |Kyllä|Kyllä
DVCPro/DVCProHD (-MXF)                            |Kyllä|Kyllä
JPEG2000                                            |Kyllä|Kyllä
MPEG-2 (422 profiilin ja korkean tason ylöspäin, mukaan lukien muuttujat, kuten XDCAM, XDCAM HD, XDCAM IMX, CableLabs® ja D10)|Enintään 422 profiili|Kyllä
MPEG-1                                              |Kyllä|Kyllä
Windows Media Video/VC-1                            |Kyllä|Kyllä
Canopus menot/HQX                                      |Ei|Ei
MPEG-4, osa 2                                       |Kyllä|Ei
[Theora](https://en.wikipedia.org/wiki/Theora)      |Kyllä|Ei
Apple ProRes 422    |Kyllä|Ei
Apple ProRes 422 LT |Kyllä|Ei
Apple ProRes 422 menot |Kyllä|Ei
Apple ProRes välityspalvelin|Kyllä|Ei
Apple ProRes 4444 |Kyllä|Ei
Apple ProRes 4444 XQ |Kyllä|Ei

###<a name="input-audio-codecs"></a>Kirjoita äänisisällön pakkauksenhallinnat

Kirjoita äänisisällön pakkauksenhallinnat|Media Encoder vakio|Media Encoder Premium työnkulku
---|---|---
AES (SMPTE 331 M ja 302 M; AES3 2003)        |Ei|Kyllä
Dolby® E                                    |Ei|Kyllä
Digitaalinen Dolby® (asentaa uuden AC3)                        |Ei|Kyllä
Dolby® Digital Plus (E asentaa uuden AC3)                 |Ei|Kyllä
AAC (AAC-LC, AAC hän ja AAC-HEv2; enintään 5.1)|Kyllä|Kyllä
MPEG-tason 2|Kyllä|Kyllä
MP3 (MPEG-1 Audio Layer 3)|Kyllä|Kyllä
Windows Media Audio|Kyllä|Kyllä
WAV/PCM|Kyllä|Kyllä
[FLAC](https://en.wikipedia.org/wiki/FLAC)</a>|Kyllä|Ei
[Opus](https://en.wikipedia.org/wiki/Opus_(audio_format)) |Kyllä|Ei
[Vorbis](https://en.wikipedia.org/wiki/Vorbis)</a>|Kyllä|Ei


###<a name="output-containerfile-formats"></a>Säilön/tiedoston siirtomuotoja

Säilön/tiedoston siirtomuotoja|Media Encoder vakio|Media Encoder Premium työnkulku
---|---|---
Adobe® Flash® F4V|Ei|Kyllä
MXF (OP1a, XDCAM ja AS02)|Ei|Kyllä
DPP (mukaan lukien AS11)|Ei|Kyllä
GXF|Ei|Kyllä
MPEG-4/MP4|Kyllä|Kyllä
MPEG-TS|Kyllä|Kyllä
Windows Media/ASF|Ei|Kyllä
AVI (pakkaamattomat 8-bittinen/10 bittiä)|Ei|Kyllä
Smooth Streaming-tiedostomuoto (PIFF 1.3)|Ei|Kyllä

###<a name="output-video-codecs"></a>Tulosteen videosisällön pakkauksenhallinnat

Tulosteen videosisällön pakkauksenhallinnat|Media Encoder vakio|Media Encoder Premium työnkulku
---|---|---
AVC (H.264; 8-bittisen; ylöspäin suuri profiilin tason 5.2; 4 K Ultra HD; AVC sisäisessä)|Vain 8-bittinen 4:2:0|Kyllä
Avid DNxHD (-MXF)|Ei|Kyllä
MPEG-2 (422 profiilin ja korkean tason ylöspäin, mukaan lukien muuttujat, kuten XDCAM, XDCAM HD, XDCAM IMX, CableLabs® ja D10)|Ei|Kyllä
MPEG-1|Ei|Kyllä
Windows Media Video/VC-1|Ei|Kyllä
JPEG esikatselukuvien luonti|Ei|Kyllä

###<a name="output-audio-codecs"></a>Tulosteen äänisisällön pakkauksenhallinnat

Tulosteen äänisisällön pakkauksenhallinnat|Media Encoder vakio|Media Encoder Premium työnkulku
---|---|---
AES (SMPTE 331 M ja 302 M; AES3 2003)|Ei|Kyllä
Digitaalinen Dolby® (asentaa uuden AC3)|Ei|Kyllä
Dolby® Digital Plus (E asentaa uuden AC3) 7.1 ylöspäin|Ei|Kyllä
AAC (AAC-LC, AAC hän ja AAC-HEv2; enintään 5.1)|Kyllä|Kyllä
MPEG-tason 2|Ei|Kyllä
MP3 (MPEG-1 Audio Layer 3)|Ei|Kyllä
Windows Media Audio|Ei|Kyllä


##<a name="error-codes"></a>Virhekoodit  

Seuraavassa taulukossa luetellaan virhekoodeja, joka voidaan palauttaa siltä varalta, että koodauksen tehtävän suorituksen aikana ilmeni virhe.  Saat virheen yksityiskohtaiset tiedot .NET-koodi-käyttää [ErrorDetails](http://msdn.microsoft.com/library/microsoft.windowsazure.mediaservices.client.errordetail.aspx) -luokka. Saat virheen yksityiskohtaiset tiedot REST-koodi, käytä [ErrorDetail](https://msdn.microsoft.com/library/jj853026.aspx) REST-Ohjelmointirajapinnalla.

ErrorDetail.Code|Sinulla on virhe
-----|-----------------------
Tuntematon| Tuntematon virhe tehtävän suorittaminen
ErrorDownloadingInputAssetMalformedContent|Virheiden luokka, joka kattaa virheiden ratkaiseminen lataaminen syötteen kohteiden, kuten virheelliset nimet, nolla tiedostot, joiden pituus, virheelliset muodot ja niin edelleen.
ErrorDownloadingInputAssetServiceFailure|Virheiden luokka, joka kattaa palvelun reunassa - esimerkissä verkko- tai virheet ladattaessa ongelmia.
ErrorParsingConfiguration|Luokan virheiden kohtaa, johon tehtävä <see cref="MediaTask.PrivateData"/> (määritys) ei ole kelvollinen, kuten määritystä ei ole kelvollinen järjestelmä esiasetus, tai se sisältää virheellisiä XML.
ErrorExecutingTaskMalformedContent|Tehtävä, jossa ongelmat sisällä syötteen mediatiedostojen aiheuttaa virheen suorituksen aikana virheitä luokka.
ErrorExecutingTaskUnsupportedFormat|Luokan virheitä, jossa media-suoritin ei voi käsitellä toimitettujen tiedostojen - media-muoto ei ole tuettu tai ei vastaa määritykset. Yritetään esimerkiksi tuottaa resurssi, joka sisältää vain videon pelkän tulosteen
ErrorProcessingTask|Muut virheet, media-suoritin kohtaa tehtävän käsiteltäessä, jotka ovat itsenäisten sisällön luokka.
ErrorUploadingOutputAsset|Luokan virheitä ladattaessa tulosteen resurssi
ErrorCancelingTask|Virheiden peittää epäonnistuu yritettäessä peruuttaa tehtävän luokka
TransientError|Luokan virheiden peittää lyhytkestoisia ongelmat (esimerkiksi. Azuren tallennustilaan väliaikaiset verkko ongelmat)


Jos tarvitset apua **Media Services** -tiimin lähettämän, Avaa [tukevat lippu](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade).



##<a name="media-services-learning-paths"></a>Media-palveluiden oppimispolut

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Palautteen antaminen

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="related-articles"></a>Aiheeseen liittyviä artikkeleita

- [Suorittaa Lisäasetukset koodauksen tehtäviä Media Encoder vakio Esiasetukset mukauttaminen](media-services-custom-mes-presets-with-dotnet.md)
- [Kiintiöiden ja rajoitukset](media-services-quotas-and-limitations.md)

 
<!--Reference links in article-->
[1]: http://azure.microsoft.com/pricing/details/media-services/
