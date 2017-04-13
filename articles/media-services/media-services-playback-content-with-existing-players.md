<properties 
    pageTitle="Toisto sisältöä | Microsoft Azure" 
    description="Tässä artikkelissa on lueteltu aiemmin pelaajat, että voit käyttää toisto sisältöä." 
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
    ms.date="10/12/2016" 
    ms.author="juliako"/>


#<a name="playing-your-content-with-existing-players"></a>Sisällön kanssa aiemmin pelaajat toistaminen

Azure Media Services tukee monet Suositut streaming muodot, kuten tasainen Streaming, HTTP Live Streaming ja MPEG-viiva. Tämä aihe ohjaa aiemmin pelaajat, joiden avulla voit testata oman virtaa.

>[AZURE.NOTE]Toista dynaamisesti pakattu tai dynaamisesti salatun sisällön, varmista, että vähintään yksi streaming yksikön hakeminen streaming päätepiste, josta aiot toimittaa sisältöä. Lisätietoja skaalaus streaming yksiköt-kohdassa: [miten streaming yksiköt](media-services-portal-manage-streaming-endpoints.md).

### <a name="the-azure-portal-media-services-content-player"></a>Azure portaalin Media Services sisällön player

**Azure** -portaalissa on sisällön toisto-ohjelman, joiden avulla voit testata videon.

Valitse haluamasi videon (Varmista, että se on [julkaistu](media-services-portal-publish.md)) ja valitse portaalin alareunassa **Toista** -painiketta.

Käytä seikkoja:

- **Palvelujen sisällön MEDIASOITINTA** toistaa streaming päätepisteen oletusarvon. Jos haluat toistaa muun kuin oletusarvoisen streaming päätepiste toiseen soitin avulla. Esimerkiksi [Azure Media Player-ohjelmassa](http://amsplayer.azurewebsites.net/azuremediaplayer.html).


![AMSPlayer][AMSPlayer]

###<a name="azure-media-player"></a>Azure Media Player-ohjelmassa

[Azure Media Playerin](http://amsplayer.azurewebsites.net/azuremediaplayer.html) avulla toisto sisältöä (Tyhjennä tai suojattu) kaikista seuraavissa muodoissa:

- Tasainen Streaming
- MPEG-VIIVA
- HLS
- Nousevan MP4


###<a name="flash-player"></a>Flash Player

####<a name="aes-encrypted-with-token"></a>AES salattu tunnuksen kanssa

[http://aestoken.azurewebsites.NET](http://aestoken.azurewebsites.net)

###<a name="silverlight-players"></a>Silverlight-soittimia

####<a name="monitoring"></a>Seuranta

[http://smf.cloudapp.NET/healthmonitor](http://smf.cloudapp.net/healthmonitor)

####<a name="playready-with-token"></a>PlayReady tunnuksen kanssa

[http://sltoken.azurewebsites.NET](http://sltoken.azurewebsites.net)

### <a name="dash-players"></a>VIIVAN pelaajat

[http://dashplayer.azurewebsites.NET](http://dashplayer.azurewebsites.net)

[http://dashif.org](http://dashif.org)

###<a name="other"></a>Muut

Voit testata HLS URL-osoitteet, voit käyttää myös seuraavasti:

- IOS-laitteessa **safari** tai
- Windowsin **3ivx HLS Player** .

##<a name="developing-video-players"></a>Videon pelaajat kehittäminen

Lisätietoja siitä, kuinka voit tehdä omia pelaajat on artikkelissa [kehitysmaiden videon pelaajat](media-services-develop-video-players.md)




##<a name="media-services-learning-paths"></a>Media-palveluiden oppimispolut

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Palautteen antaminen

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


[AMSPlayer]: ./media/media-services-playback-content-with-existing-players/media-services-portal-player.png
