<properties 
    pageTitle="Media Encoder Premium työnkulun muotoja ja pakkauksenhallinnat | Microsoft Azure" 
    description="Tässä artikkelissa on yleiskatsaus Media Encoder Premium työnkulun muotoilut muotoilut ja pakkauksenhallinnat" 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="erik43" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016"    
    ms.author="juliako;anilmur"/>

#<a name="media-encoder-premium-workflow-formats-and-codecs"></a>Media Encoder Premium työnkulun muotoja ja pakkauksenhallinnat


>[AZURE.NOTE]Premium encoder kysymyksille sähköpostin mepd Microsoft.comin.
>
>Tässä aiheessa kuvatut Media Encoder Premium työnkulun media suoritin ei ole käytettävissä Kiinassa. 

Tämä asiakirja sisältää syötteen ja tulosteen tiedostomuodot ja **Media Encoder Premium työnkulun** encoder julkisen preview-versio tukemista pakkauksenhallinnoista luettelo.

[Media Encoder Premium Worflow syötteen muotoja ja pakkauksenhallinnat](#input_formats)

[Media Encoder Premium Worflow siirtomuodoista ja pakkauksenhallinnat](#output_formats)

**Media Encoder Premium työnkulku** tukee tekstitys [Tässä](#closed_captioning) osiossa esitettyjä. 


##<a id="input_formats"></a>Media Encoder Premium työnkulun Lisää muotoja ja pakkauksenhallinnat

Seuraavassa osassa on luettelo pakkauksenhallinnoista sekä tiedoston muotoiluja, jotka media-suoritin tukee syötteenä.

###<a name="input-containerfile-formats"></a>Lisää säilö/tiedostomuodot

- Adobe® Flash® F4V
- MXF/SMPTE 377M
- GXF
- MPEG-2 Transport virtaa
- MPEG-2-ohjelman virtaa
- MPEG-4/MP4
- Windows Media/ASF
- AVI (pakkaamattomat 8-bittinen/10 bittiä)

###<a name="input-video-codecs"></a>Kirjoita videosisällön pakkauksenhallinnat

- AVC 8-bittinen/10-bittinen ylöspäin 4:2:2, mukaan lukien AVCIntra
- Avid DNxHD (-MXF)
- DVCPro/DVCProHD (-MXF)
- JPEG2000
- MPEG-2 (422 profiilin ja korkean tason ylöspäin, mukaan lukien muuttujat, kuten XDCAM, XDCAM HD, XDCAM IMX, CableLabs® ja D10)
- MPEG-1
- Windows Media Video/VC-1

###<a name="input-audio-codecs"></a>Kirjoita äänisisällön pakkauksenhallinnat

- AES (SMPTE 331 M ja 302 M; AES3 2003)
- Dolby® E
- Digitaalinen Dolby® (asentaa uuden AC3)
- AAC (AAC-LC, AAC hän ja AAC-HEv2; enintään 5.1)
- MPEG-tason 2
- MP3 (MPEG-1 Audio Layer 3)
- Windows Media Audio
- WAV/PCM
 
##<a id="output_format"></a>Media Encoder Premium työnkulun siirtomuodoista ja pakkauksenhallinnat

Seuraavassa osassa on luettelo pakkauksenhallinnoista sekä tiedoston muotoiluja, jotka media suorittimen tulosteen sovelluksia tuetaan.

###<a name="output-containerfile-formats"></a>Säilön/tiedoston siirtomuotoja

- Adobe® Flash® F4V
- MXF (OP1a, XDCAM ja AS02)
- DPP (mukaan lukien AS11)
- GXF
- MPEG-4/MP4
- Windows Media/ASF
- AVI (pakkaamattomat 8-bittinen/10 bittiä)
- Smooth Streaming-tiedostomuoto (PIFF 1.3)
- MPEG-TS 


###<a name="output-video-codecs"></a>Tulosteen videosisällön pakkauksenhallinnat

- AVC (H.264; 8-bittisen; ylöspäin suuri profiilin tason 5.2; 4 K Ultra HD; AVC sisäisessä)
- Avid DNxHD (-MXF)
- DVCPro/DVCProHD (-MXF)
- MPEG-2 (422 profiilin ja korkean tason ylöspäin, mukaan lukien muuttujat, kuten XDCAM, XDCAM HD, XDCAM IMX, CableLabs® ja D10)
- MPEG-1
- Windows Media Video/VC-1
- JPEG esikatselukuvien luonti

###<a name="output-audio-codecs"></a>Tulosteen äänisisällön pakkauksenhallinnat

- AES (SMPTE 331 M ja 302 M; AES3 2003)
- Digitaalinen Dolby® (asentaa uuden AC3)
- Dolby® Digital Plus (E asentaa uuden AC3) 7.1 ylöspäin
- AAC (AAC-LC, AAC hän ja AAC-HEv2; enintään 5.1)
- MPEG-tason 2
- MP3 (MPEG-1 Audio Layer 3)
- Windows Media Audio

##<a id="closed_captioning"></a>Tekstityksen tuki

Valitse ingest, **Media Encoder Premium työnkulku** tukee:

1. SCC tiedostot
1. SMPTE TT tiedostot
1. CEA-608/CEA-708 – käyttäjän tietoina (SEI viestien H.264 peruskoulun virtaa, ATSC/53, SCTE20) tai toteutuu siihen liittyvät työt tietoina MXF/GXF-tiedostoissa
1. Jotka sisältävät STL alaotsikko tiedostot

Tulostus seuraavat asetukset ovat käytettävissä:

1. CEA-608 CEA 708 käännökseen
1. CEA-608/CEA-708 kulkevat (SEI viestien H.264 peruskoulun virtaa upotettuna tai suoritettujen MXF tiedostojen kuin siihen liittyvät työt tietojen)
1. SCC
1. SMPTE aikakatkaistu teksti (lähteestä CEA 608 SMPTE RP2052 kohti, mukaan lukien DFXP luominen)
1. SRT alaotsikko-tiedosto
1. DVB alaotsikko virtaa

Huomautus: kaikki edellä mainitut siirtomuodoista tuetaan toimittamista kautta streaming Azure Media Services-palveluissa.

##<a name="known-issues"></a>Tunnetut ongelmat

Jos syötteen video ei sisällä tekstitys, tulos resurssi edelleen sisältää tyhjän TTML tiedoston. 


##<a name="media-services-learning-paths"></a>Media-palveluiden oppimispolut

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Palautteen antaminen

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
