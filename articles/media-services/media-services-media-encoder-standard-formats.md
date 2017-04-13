<properties 
    pageTitle="Media Encoder vakiomuotoa ja pakkauksenhallinnat" 
    description="Tässä artikkelissa on yleiskatsaus Media Encoder vakiomuotoa ja pakkauksenhallinta." 
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
    ms.date="10/10/2016"
    ms.author="juliako;anilmur"/>

#<a name="media-encoder-standard-formats-and-codecs"></a>Media Encoder vakiomuotoa ja pakkauksenhallinta


Tämä asiakirja on luettelo yleisimmistä tuominen ja vieminen tiedostomuodot, joita voi käyttää Media Encoder vakio.


##<a name="input-containerfile-formats"></a>Lisää säilö/tiedostomuodot

Tiedostomuodot (tiedostotunnisteet)|Tuettu
---|---|---|---
FLV (ja H.264 ja AAC pakkauksenhallinnat) (.flv)          |Kyllä 
MXF (.mxf)                  |Kyllä 
GXF (.gxf)                  |Kyllä 
MPEG2-PS, MPEG2-TS 3GP (.ts, .ps, .3gp, .3gpp, .mpg)   |Kyllä 
Windows Media Video (WMV) / ASF (.wmv, .asf) |Kyllä 
AVI (pakkaamattomat 8-bittinen/10 bittiä) (.avi)|Kyllä 
MP4 (.mp4, .m4a, .m4v) / ISMV (.isma, .ismv)|Kyllä 
[Microsoft Digital Video Recording(DVR-MS)](https://msdn.microsoft.com/library/windows/desktop/dd692984) (.dvr-ms) |Kyllä 
Matroska/WebM (.mkv)        |Kyllä 
AALTO/WAV (.wav) |Kyllä 
QuickTime (.mov) |Kyllä

>[AZURE.NOTE] On yllä usein havaittiin tiedostotunnisteiden luettelo. Media Encoder Standard tue monista muista (esimerkiksi: .m2ts, .mpeg2video ja .qt). Jos yrität salata tiedoston ja näyttöön tulee virhesanoma, jos muotoa ei tueta, anna palaute [tähän](https://feedback.azure.com/forums/169396-media-services/category/144411-encoding-and-processing/).

###<a name="audio-formats-in-input-containers"></a>Syötteen säilöjen ääni-muodot 

Media Encoder Standard tukee suorittamisesta syötteen säilöjen seuraavat äänitiedostomuodot:

- MXF, GXF ja QuickTime tiedostot, joilla on kanssa limitetty stereo audio raidat tai 5.1 näytteiden

tai

- MXF, GXF ja QuickTime-tiedostoja, jossa ääni suoritetaan kuin erillisessä PCM raidat, mutta kanavan määritystä (stereo ja 5.1) voit johdetaan tiedoston metatiedot

Huomaa, joka tukee eksplisiittinen/käyttäjän kanavan yhdistäminen edellyttäen lähitulevaisuudessa.


##<a name="input-video-codecs"></a>Kirjoita videosisällön pakkauksenhallinnat

Kirjoita videosisällön pakkauksenhallinnat|Tuettu
---|---|---|---
AVC 8-bittinen/10-bittinen ylöspäin 4:2:2, mukaan lukien AVCIntra   |8-bittinen 4:2:0 – 4 / 2:2 
Avid DNxHD (-MXF)                                 |Kyllä 
DVCPro/DVCProHD (-MXF)                            |Kyllä 
Digitaalisen videokameran (-AVI-tiedostot)                   |Kyllä
JPEG 2000: SSA                                           |Kyllä 
MPEG-2 (422 profiilin ja korkean tason ylöspäin, mukaan lukien muuttujat, kuten XDCAM, XDCAM HD, XDCAM IMX, CableLabs® ja D10)|Enintään 422 profiili 
MPEG-1                                              |Kyllä 
VC-1/WMV9                                           |Kyllä 
Canopus menot/HQX                                      |Ei 
MPEG-4, osa 2                                       |Kyllä 
[Theora](https://en.wikipedia.org/wiki/Theora)      |Kyllä 
Puretaan YUV420 tai mezzanine                   |Kyllä
Apple ProRes 422                                    |Kyllä
Apple ProRes 422 LT |Kyllä
Apple ProRes 422 menot |Kyllä
Apple ProRes välityspalvelin|Kyllä
Apple ProRes 4444 |Kyllä
Apple ProRes 4444 XQ |Kyllä



##<a name="input-audio-codecs"></a>Kirjoita äänisisällön pakkauksenhallinnat

Kirjoita äänisisällön pakkauksenhallinnat|Tuettu
---|---|---|---
AAC (AAC-LC, AAC hän ja AAC-HEv2; enintään 5.1)|Kyllä 
MPEG-tason 2|Kyllä 
MP3 (MPEG-1 Audio Layer 3)|Kyllä 
Windows Media Audio|Kyllä 
WAV/PCM|Kyllä 
[FLAC](https://en.wikipedia.org/wiki/FLAC)</a>|Kyllä 
[Opus](http://go.microsoft.com/fwlink/?LinkId=822667) |Kyllä 
[Vorbis](https://en.wikipedia.org/wiki/Vorbis)</a>|Kyllä 
AMR (mukautuvat usean kurssi)|Kyllä
AES (SMPTE 331 M ja 302 M; AES3 2003)        |Ei 
Dolby® E                                    |Ei 
Digitaalinen Dolby® (asentaa uuden AC3)                        |Ei 
Dolby® Digital Plus (E asentaa uuden AC3)                 |Ei 


##<a name="output-formats-and-codecs"></a>Siirtomuodoista ja pakkauksenhallinnat

Seuraavassa taulukossa on lueteltu pakkauksenhallinta ja tiedostomuotoja, joita tueta Vie.


Tiedostomuoto|Videon pakkauksenhallinta|Pakkauksenhallinnan
---|---|---
MP4 <br/><br/>(mukaan lukien usean bittitaajuus MP4 säilöjen) |H.264 (suuri, pää ja perusaikataulun profiilit)|AAC-LC, HE-AAC v1, HE-AAC v2 
MPEG2 TS |H.264 (suuri, pää ja perusaikataulun profiilit)|AAC-LC, HE-AAC v1, HE-AAC v2 



##<a name="media-services-learning-paths"></a>Media-palveluiden oppimispolut

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Palautteen antaminen

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="see-also"></a>Katso myös

[Koodaus tarvittaessa sisällön Azure Media-palvelujen kanssa](media-services-encode-asset.md)

[Voit salata kanssa Media Encoder vakio](media-services-dotnet-encode-with-media-encoder-standard.md)
