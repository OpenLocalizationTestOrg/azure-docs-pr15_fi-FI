<properties
    pageTitle="  Azure-portaalissa sisällön julkaiseminen | Microsoft Azure"
    description="Tässä opetusohjelmassa käydään läpi vaiheet julkaiseminen sisällön Azure-portaalissa."
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
    ms.date="10/24/2016"
    ms.author="juliako"/>

# <a name="publish-content-with-the-azure-portal"></a>Julkaista sisältöä Azure-portaalissa

> [AZURE.SELECTOR]
- [Portal](media-services-portal-publish.md)
- [.NET](media-services-deliver-streaming-content.md)
- [MUILLE KÄYTTÄJILLE](media-services-rest-deliver-streaming-content.md)

## <a name="overview"></a>Yleiskatsaus

> [AZURE.NOTE] Jotta voit suorittaa tässä opetusohjelmassa, sinun on Azure-tili. Lisätietoja on artikkelissa [Azure maksuttoman kokeiluversion](https://azure.microsoft.com/pricing/free-trial/). 

Voit antaa käyttäjän URL-osoite, jonka avulla voidaan käyttää tai ladata sisältöä, sinun on "julkaiseminen" oman resurssi locator luomalla. Locators on pääsy kohteen olevat tiedostot. Media-palveluiden tukee kahdentyyppisiä locators: 

- Streaming (OnDemandOrigin) locators, käytetään mukautuvat toistamisen (esimerkiksi stream MPEG VIIVA, HLS tai tasainen Streaming). Voit luoda streaming locator oman resurssi on oltava .ism-tiedosto. 
- Nousevan (SAS)-locators, käytetään lähettämisen kautta asteittain Lataa video.


Streaming URL-osoite on muotoa ja sen avulla voit toistaa tasainen Streaming resurssit.

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

Muodostaa streaming URL-osoite HLS liittäminen (Muotoile = m3u8 aapl) URL-osoitteeseen.

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

Muodostaa streaming URL-Osoitteen MPEG-VIIVA liittäminen (Muotoile = mpd-aika-csf) URL-osoitteeseen.

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)

SAS URL-osoite on muotoa.

    {blob container name}/{asset name}/{file name}/{SAS signature}

Lisätietoja on artikkelissa [toimenpide yleiskatsaus](media-services-deliver-content-overview.md).

>[AZURE.NOTE] Jos olet käyttänyt portaalin luominen locators ennen maaliskuussa 2015, locators kahden vuoden vanhenemispäivämäärä kanssa on luotu.  

Päivitä vanhenemispäivämäärä, locator käyttämällä [REST](http://msdn.microsoft.com/library/azure/hh974308.aspx#update_a_locator ) - tai [.NET](http://go.microsoft.com/fwlink/?LinkID=533259) -ohjelmointirajapinnan. Huomaa, että kun päivität SAS locator vanhenemispäivää, URL-osoite muuttuu.

### <a name="to-use-the-portal-to-publish-an-asset"></a>Julkaise sijoituksen portaalin avulla

Portaalin avulla voit julkaista sijoituksen, toimi seuraavasti:

1. Valitse [Azure portal](https://portal.azure.com/)Azure Media Services-tilin.
1. Valitse **asetukset** > **resurssit**.
1. Valitse resurssi, jonka haluat julkaista.
1. Valitse **Julkaise** -painikkeen.
1. Valitse locator.
2. Paina **Lisää**.

    ![Julkaiseminen](./media/media-services-portal-vod-get-started/media-services-publish1.png)

URL-osoite lisätään **Julkaistu URL-osoitteiden**luettelon.

## <a name="play-content-from-the-portal"></a>Sisällön portaalista toistaminen

Azure-portaalissa on sisällön toisto-ohjelman, joiden avulla voit testata videon.

Valitse haluamasi video ja valitse sitten **Toista** -painiketta.

![Julkaiseminen](./media/media-services-portal-vod-get-started/media-services-play.png)

Käytä seikkoja:

- Varmista, että video on julkaistu.
- Tämä **Media Playerin** toistaa streaming päätepisteen oletusarvon. Jos haluat toistaa muun kuin oletusarvoisen streaming päätepiste, valitse toisen toisto ja kopioi URL-osoite. Esimerkiksi [Azure mediasoitinta palvelut](http://amsplayer.azurewebsites.net/azuremediaplayer.html).
- Streaming päätepisteen, josta ovat streaming on oltava käynnissä.  
- Lähettää streaming päätepisteen-, Lisää vähintään yksi streaming yksikkö. Lisätietoja on artikkelissa [tämän](media-services-portal-scale-streaming-endpoints.md) ohjeaiheen.   

##<a name="next-steps"></a>Seuraavat vaiheet

Tarkista oppimispolut Media-palveluita.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Palautteen antaminen

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


