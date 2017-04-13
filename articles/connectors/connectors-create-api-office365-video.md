<properties
pageTitle="Käytä Office 365 Video-yhdistin logiikan-sovelluksissa | Microsoft Azure"
description="Microsoft Azure-sovelluksen yrityksen logiikan sovellusten käyttämisestä Office 365 Video-yhdistin"
services=""    
documentationCenter=""     
authors="msftman"    
manager="erikre"    
editor=""
tags="connectors"/>

<tags
ms.service="multiple"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="na"
ms.date="05/18/2016"
ms.author="deonhe"/>

# <a name="get-started-with-the-office365-video-connector"></a>Käytön aloittaminen Office 365 Video-yhdistin
Muodostaa yhteyden Office 365 Video-Saat lisätietoja Office 365 videon, saat luettelon käytettävissä videoita ja paljon muuta. Office 365 Video-yhdistin voidaan käyttää:

- Logiikan sovellukset 

>[AZURE.NOTE] Tässä versiossa on artikkelissa koskee logiikan sovellusten 2015-08-01 – esikatselu rakenteen versio. Tämä yhdistin ei tueta aiemmissa rakenne-versioissa.

Office 365-video voit:

- Muodostaa yhteyttä business työnkulku, sinulla on Office 365 Video tietojen perusteella. 
- Käytä Toiminnot, jotka video-portaalin tilan, saat luettelon käytettävissä kaikki video-kanavan ja paljon muuta. Nämä toiminnot vastauksen saaminen ja tee tulosteen käytettävissä, jos haluat käyttää muuta vaihtoehtoa. Voit esimerkiksi Hae Bing-yhdistin avulla voit etsiä Office 365-videot ja tietojen tarkasteleminen, video Office 365 videon Connectorin avulla. Jos video ei vastaa tarpeitasi, voit julkaista tässä videossa Facebookista. 

Jos haluat lisätä toiminnon logiikan sovelluksissa, katso [logiikan-sovelluksen luominen](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Käynnistimet ja toiminnot

Office 365 Video-yhdistin on käytettävissä seuraavat toimet: Ei ole Käynnistimet.

| Käynnistimien | Toiminnot|
| --- | --- |
| Ei mitään | <ul><li>Tarkistaa, video-portaalin tila</li><li>Hae kaikki voivat tarkastella kanavat</li><li>Hae videon toiston URL-osoite Azure Media Services-luettelo</li><li>Voit purkaa videon pääsy haltijatunnukseen hankkiminen</li><li>Hakee tietoja tietyn Office 365 video</li><li>Office 365-videoita esittää kanavan luettelot</li></ul>

Yhdistimien tukevat tietojen JSON ja XML-muodossa. 

## <a name="create-a-connection-to-office365-video-connector"></a>Yhteyden muodostaminen Office 365 Video-yhdistin
Kun lisäät Tämä yhdistin logiikan sovelluksia, on kirjautuminen Office 365 Video-tilisi ja sallia logiikan sovellusten voit muodostaa yhteyden tiliisi.

>[AZURE.INCLUDE [Steps to create a connection to Office 365 Video](../../includes/connectors-create-api-office365video.md)]

Kun olet luonut yhteyden, Määritä Office 365 video ominaisuudet, kuten vuokraajan nimi tai kanavan tunnus. **REST API viittaus** tässä ohjeaiheessa kuvataan nämä ominaisuudet.

>[AZURE.TIP] Voit käyttää samaa Office 365 Video-yhteyteen logiikan muista sovelluksista.

## <a name="swagger-rest-api-reference"></a>Swagger REST API-viittaus
Koskee versiota: 1.0.

### <a name="checks-video-portal-status"></a>Tarkistaa, video-portaalin tila 
Tarkistaa videon portaalin tilaa nähdä, jos video palvelut otetaan käyttöön.  
```GET: /{tenant}/IsEnabled``` 

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|vuokraajan|merkkijono|Kyllä|polku|ei mitään|Hakemiston käyttäjän vuokraajan nimi on osa|


#### <a name="response"></a>Vastaus

|Nimi|Kuvaus|
|---|---|
|200|Toiminto onnistui|
|400|BadRequest|
|401|Ei valtuuksia|
|404|Ei löydy|
|500|Sisäinen palvelinvirhe|
|Oletusarvo|Epäonnistui.|



### <a name="get-all-viewable-channels"></a>Hae kaikki voivat tarkastella kanavat 
Hakee kaikki käyttäjällä on pääsy tarkasteleminen kanavat.  
```GET: /{tenant}/Channels``` 

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|vuokraajan|merkkijono|Kyllä|polku|ei mitään|Hakemiston käyttäjän vuokraajan nimi on osa|


#### <a name="response"></a>Vastaus

|Nimi|Kuvaus|
|---|---|
|200|Toiminto onnistui|
|400|BadRequest|
|401|Ei valtuuksia|
|404|Ei löydy|
|500|Sisäinen palvelinvirhe|
|Oletusarvo|Epäonnistui.|




### <a name="lists-all-the-office365-videos-present-in-a-channel"></a>Office 365-videoita esittää kanavan luettelot 
Office 365-videoita esittää kanavan luettelot.  
```GET: /{tenant}/Channels/{channelId}/Videos``` 

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|vuokraajan|merkkijono|Kyllä|polku|ei mitään|Hakemiston käyttäjän vuokraajan nimi on osa|
|channelId|merkkijono|Kyllä|polku|ei mitään|Kanavatunnus, josta on noudettavat videoita|


#### <a name="response"></a>Vastaus

|Nimi|Kuvaus|
|---|---|
|200|Toiminto onnistui|
|400|BadRequest|
|401|Ei valtuuksia|
|404|Ei löydy|
|500|Sisäinen palvelinvirhe|
|Oletusarvo|Epäonnistui.|




### <a name="gets-information-about-a-particular-office365-video"></a>Hakee tietoja tietyn Office 365 video 
Hakee tietoja tietyn Office 365 video.  
```GET: /{tenant}/Channels/{channelId}/Videos/{videoId}``` 

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|vuokraajan|merkkijono|Kyllä|polku|ei mitään|Hakemiston käyttäjän vuokraajan nimi on osa|
|channelId|merkkijono|Kyllä|polku|ei mitään|Kanavatunnus|
|videoId|merkkijono|Kyllä|polku|ei mitään|Videon tunnus|


#### <a name="response"></a>Vastaus

|Nimi|Kuvaus|
|---|---|
|200|Toiminto onnistui|
|400|BadRequest|
|401|Ei valtuuksia|
|404|Ei löydy|
|500|Sisäinen palvelinvirhe|
|Oletusarvo|Epäonnistui.|




### <a name="get-playback-url-of-the-azure-media-services-manifest-for-a-video"></a>Hae videon toiston URL-osoite Azure Media Services-luettelo 
Hanki videon toiston URL-osoite Azure Media Services-luettelo.  
```GET: /{tenant}/Channels/{channelId}/Videos/{videoId}/playbackurl``` 

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|vuokraajan|merkkijono|Kyllä|polku|ei mitään|Hakemiston käyttäjän vuokraajan nimi on osa|
|channelId|merkkijono|Kyllä|polku|ei mitään|Kanavatunnus|
|videoId|merkkijono|Kyllä|polku|ei mitään|Videon tunnus|
|streamingFormatType|merkkijono|Kyllä|kyselyn|ei mitään|Streaming format tyyppi. 1 - tietovirta tasainen tai MPEG-VIIVA. 0 - tietovirta HLS|


#### <a name="response"></a>Vastaus

|Nimi|Kuvaus|
|---|---|
|200|Toiminto onnistui|
|400|BadRequest|
|401|Ei valtuuksia|
|404|Ei löydy|
|500|Sisäinen palvelinvirhe|
|Oletusarvo|Epäonnistui.|




### <a name="get-the-bearer-token-to-get-access-to-decrypt-the-video"></a>Voit purkaa videon pääsy haltijatunnukseen hankkiminen 
Voit purkaa videon pääsy haltijatunnukseen hakeminen  
```GET: /{tenant}/Channels/{channelId}/Videos/{videoId}/token```

| Nimi| Tietotyyppi|Pakollinen|Sijaitsee|Oletusarvo|Kuvaus|
| ---|---|---|---|---|---|
|vuokraajan|merkkijono|Kyllä|polku|ei mitään|Hakemiston käyttäjän vuokraajan nimi on osa|
|channelId|merkkijono|Kyllä|polku|ei mitään|Kanavatunnus|
|videoId|merkkijono|Kyllä|polku|ei mitään|Videon tunnus|


#### <a name="response"></a>Vastaus

|Nimi|Kuvaus|
|---|---|
|200|Toiminto onnistui|
|400|BadRequest|
|401|Ei valtuuksia|
|404|Ei löydy|
|500|Sisäinen palvelinvirhe|
|Oletusarvo|Epäonnistui.|


## <a name="object-definitions"></a>Objektimääritykset

#### <a name="channel-channel-class"></a>Kanavan: Kanavan luokan

| Nimi | Tietotyyppi | Pakollinen|
|---|---|---|
|Tunnus|merkkijono|Ei|
|Otsikko|merkkijono|Ei|
|Kuvaus|merkkijono|Ei|


#### <a name="video"></a>Video 

| Nimi | Tietotyyppi |Pakollinen|
|---|---|---|
|Tunnus|merkkijono|Ei|
|Otsikko|merkkijono|Ei|
|Kuvaus|merkkijono|Ei|
|CreationDate|merkkijono|Ei|
|Omistaja|merkkijono|Ei|
|ThumbnailUrl|merkkijono|Ei|
|VideoUrl|merkkijono|Ei|
|VideoDuration|kokonaisluku|Ei|
|VideoProcessingStatus|kokonaisluku|Ei|
|ViewCount|kokonaisluku|Ei|


## <a name="next-steps"></a>Seuraavat vaiheet
[Logiikan-sovelluksen luominen](../app-service-logic/app-service-logic-create-a-logic-app.md).
