<properties 
    pageTitle="Käyttämällä kumppanien pitää Widevine käyttöoikeuksien Azure Media Services | Microsoft Azure" 
    description="Tässä artikkelissa kuvataan, miten voit Azure Media Services (AMS) aikana muodossa, joka on dynaamisesti salattu AMS PlayReady ja Widevine DRMs. PlayReady käyttöoikeuden tulee Media Services PlayReady käyttöoikeuspalvelimen ja Widevine käyttöoikeuden toimitetaan castLabs käyttöoikeuspalvelimen mukaan." 
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
    ms.date="09/26/2016"  
    ms.author="juliako"/>

#<a name="using-partners-to-deliver-widevine-licenses-to-azure-media-services"></a>Käyttämällä kumppanien pitää Widevine käyttöoikeuksien Azure Media-palveluita

##<a name="overview"></a>Yleiskatsaus

Microsoft Azure Media Services-palvelujen avulla voit toimittaa MPEG-VIIVA suojattu Widevine DRM, joka on salattu yleisiä salaus (CENC) määrityksen kohden.

Media Services .NET SDK versio 3.5.2 alkaen Media Services-palvelujen avulla voit määrittää Widevine käyttöoikeuden mallin ja saada Widevine käyttöoikeudet. Voit käyttää myös seuraavat AMS kumppanien avulla voit pitää Widevine käyttöoikeudet: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/) [castLabs](http://castlabs.com/company/partners/azure/).

##<a name="castlabs"></a>castLabs

Voit pitää Widevine käyttöoikeuksien [castLabs](http://castlabs.com/company/partners/azure/) . Lisätietoja on artikkelissa [käyttäminen castLabs aikana DRM käyttöoikeuksia Azure Media-palveluita](media-services-castlabs-integration.md)

##<a name="axinom"></a>Axinom

Voit pitää Widevine käyttöoikeuksien [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/) . Lisätietoja on artikkelissa [Aikana DRM käyttöoikeuksia Azure Media-palveluita käyttämällä Axinom](media-services-axinom-integration.md)


##<a name="media-services-learning-paths"></a>Media-palveluiden oppimispolut

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Palautteen antaminen

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="see-also"></a>Katso myös

[Käyttää PlayReady ja/tai Widevine dynaaminen yhteistä salausta](media-services-protect-with-drm.md)

[Mingfei's-blogi](https://azure.microsoft.com/blog/azure-media-services-adds-google-widevine-packaging-for-delivering-multi-drm-stream/)

