<properties
    pageTitle="Dynaaminen pakkaus yleiskatsaus | Microsoft Azure"
    description="Aiheen avulla ja Dynaaminen pakkaus yleiskatsaus."
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
    ms.date="10/24/2016" 
    ms.author="juliako"/>


# <a name="dynamic-packaging"></a>Dynaaminen pakkaus

##<a name="overview"></a>Yleiskatsaus

Microsoft Azure Media Services voidaan pitää monta lähde tiedoston mediatiedostoja, mediavirtaa muotoilut- ja sisällön suojauksen muotoilee yhteyden useisiin asiakkaan tekniikoita (esimerkiksi iOS XBOX, Silverlight, Windows 8). Näiden asiakkaiden tietoja eri protokollia, kuten iOS edellyttää HTTP Live Streaming (HLS) V4-muodossa ja Silverlightin ja Xbox vaatii tasainen Streaming. Jos sinulla on joukko mukautuvat bittitaajuus (bittitaajuus usean) MP4 (ISO Base 14496 – 12) mediatiedostojen tai tiedostojoukon mukautuvat bittitaajuus tasainen Streaming, jonka haluat yhteyshenkilönä asiakkaat, jotka ymmärtää MPEG VIIVA, HLS tai tasainen Streaming olisi voit hyödyntää Media Services Dynaaminen pakkaus.

Dynaaminen pakkaus on on resurssi, joka sisältää mukautuvat bittitaajuus MP4-tiedostot tai mukautuvat bittitaajuus tasainen Streaming tiedostojen luomiseen. Sitten määritetyssä muodossa, luettelo tai fragmentti pyynnön edelleen tarvittaessa Streaming perusteella palvelimen varmistat, että näyttöön tulee virta valitsemasi protokolla. Tuloksena tarvitset vain tallentamiseen ja maksaa yhden Tallennusmuoto tiedostot ja Media-palvelut luominen ja yhteyshenkilönä oikea vastaus asiakaskoneesta pyyntöjen perusteella.

Seuraavassa kaaviossa on esitetty perinteinen koodaus- ja staattinen pakkaus työnkulun.

![Staattinen koodaus](./media/media-services-dynamic-packaging-overview/media-services-static-packaging.png)

Seuraavassa kaaviossa on esitetty Dynaaminen pakkaus työnkulun.

![Dynaaminen koodaus](./media/media-services-dynamic-packaging-overview/media-services-dynamic-packaging.png)


>[AZURE.NOTE]Dynaaminen pakkaus hyödyntää, sinun täytyy saada vähintään yhden tarvittaessa streaming yksikön streaming päätepisteen, josta aiot toimituksen sisältöä. Katso lisätietoja, [miten asteikko Media Services](media-services-portal-manage-streaming-endpoints.md).

##<a name="common-scenario"></a>Käytetty vaihtoehto

1. Lataa syötteen äänitiedoston (jota kutsutaan mezzanine-tiedosto). Esimerkiksi H.264, MP4 tai WMV (for tuetut tiedostomuodot Katso [Muodot tukemat Media Encoder vakio](media-services-media-encoder-standard-formats.md)luettelo.

1. Koodata H.264 MP4 mukautuvat bittitaajuus tietojoukkojen mezzanine-tiedostossa.

1. Julkaise resurssi, joka sisältää MP4 määrittäminen luomalla edelleen tarvittaessa Locator mukautuvat bittitaajuus.

1. Luoda ja käyttää sisältöä streaming URL-osoitteet.


##<a name="preparing-assets-for-dynamic-streaming"></a>Dynaaminen streaming varat valmisteleminen

Dynaaminen streaming oman resurssi valmisteleminen on kaksi vaihtoehtoa:

1. [Perusmuodon tiedoston lataaminen](media-services-dotnet-upload-files.md).
2. [Käytä Media Encoder vakio encoder tuottamaan H.264 MP4 mukautuvat bittitaajuus joukot](media-services-dotnet-encode-with-media-encoder-standard.md).
3. [Virta sisältöä](media-services-deliver-content-overview.md).


##<a id="unsupported_formats"></a>Muodot, joita ei tueta Dynaaminen pakkaus

Dynaaminen pakkaus ei tue tietolähteen tiedostomuodoista.

- Dolby digital mp4-tiedostot.
- Dolby digital tasainen tiedostot.

##<a name="media-services-learning-paths"></a>Media-palveluiden oppimispolut

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Palautteen antaminen

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
