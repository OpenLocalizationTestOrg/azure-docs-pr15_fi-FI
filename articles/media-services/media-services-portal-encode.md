<properties
    pageTitle="Koodata resurssi käyttämällä Media Encoder vakio Azure-portaalissa | Microsoft Azure"
    description="Tässä opetusohjelmassa käydään läpi vaiheet koodaus resurssi käyttämällä Media Encoder vakio Azure-portaalissa."
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


# <a name="encode-an-asset-using-media-encoder-standard-with-the-azure-portal"></a>Koodata resurssi käyttämällä Media Encoder vakio Azure-portaalissa

> [AZURE.NOTE] Jotta voit suorittaa tässä opetusohjelmassa, sinun on Azure-tili. Lisätietoja on artikkelissa [Azure maksuttoman kokeiluversion](https://azure.microsoft.com/pricing/free-trial/). 

Kun käsittelet jokin tilanteissa toimittaa mukautuvat bittitaajuus streaming asiakkaiden Azure Media-palveluita. Media-palveluiden tukee seuraavia mukautuvat bittitaajuus streaming tekniikoita: HTTP Live Streaming (HLS), tasainen Streaming, MPEG VIIVA ja HDS (varten vain Adobe PrimeTime/Access asiakirjamalleja). Videoita valmisteleminen mukautuvat bittitaajuus streaming-haluat koodata lähteen videon usean bittitaajuus-tiedostoiksi. Käytettävä **Media Encoder vakio** encoder koodata videoita.  

Media-palvelut sekä Dynaaminen pakkaus, jonka avulla voit pitää usean bittitaajuus MP4s streaming seuraavissa muodoissa: MPEG VIIVA, HLS, tasainen Streaming tai HDS, eikä sinun tarvitse uudelleen pakkaaminen huomioon nämä streaming muotoja. Dynaaminen pakkaus tarvitset vain tallentamiseen ja maksaa yhden Tallennusmuoto tiedostot sekä Media Services luominen ja yhteyshenkilönä oikea vastaus asiakaskoneesta pyyntöjen perusteella.

Jos haluat hyödyntää Dynaaminen pakkaus, seuraavasti:

- Koodata lähdetiedoston usean bittitaajuus MP4-tiedostot (koodauksen vaiheet ovat jäljempänä tässä osassa esiteltyjä) joukkoon.
- Vähintään yhden streaming yksikön streaming päätepisteen, josta aiot toimituksen sisällön hakeminen Lisätietoja on artikkelissa [määrittäminen streaming päätepisteet](media-services-portal-vod-get-started.md#configure-streaming-endpoints). 

[Tässä](media-services-portal-scale-media-processing.md) ohjeaiheessa skaalata media käsittely.

## <a name="encode-with-the-azure-portal"></a>Koodata Azure-portaalissa

Tässä osassa kuvataan, miten voit koodata käytettäessä Media Encoder Standard sisältöä.

1.  Valitse [Azure portal](https://portal.azure.com/)Azure Media Services-tilin.
2.  Valitse **asetukset** -ikkunassa **resurssit**.  
2.  Valitse resurssi, jotka haluat koodata **kalusto** -ikkunassa.
3.  Valitse **Käytä** -painike.
4.  Valitse **Käytä amerikkalaisen** -ikkunassa "Media Encoder vakio-suoritin ja esiasetus. Esimerkiksi jos tiedät syötteen video on 1920 x 1080 kuvapisteen tarkkuutta, valitse-sovelluksella "H264 useita bittitaajuus 1080p" esiasetettuja. Lisätietoja Esiasetukset, katso [Tässä](https://msdn.microsoft.com/library/azure/mt269960.aspx) artikkelissa – on tärkeää Valitse esiasetus, joka sopii parhaiten syötettäsi video. Jos sinulla on videon pienitarkkuuksisessa näytössä (640 x 360) ja valitse sinun ei olisi käytettävä oletusarvo "H264 useita bittitaajuus 1080p" esiasetettuja.
    
    Helpottaa hallintaa varten on vaihtoehto muokata tulosteen kohteen nimi ja projektin nimeä.
        
    ![Koodata resurssit](./media/media-services-portal-vod-get-started/media-services-encode1.png)
5. Paina **luominen**.


##<a name="next-step"></a>Seuraava vaihe

Voit valvoa koodauksen työn edistymistä Azure-portaalissa [Tässä](media-services-portal-check-job-progress.md) artikkelissa kuvatulla tavalla.  

##<a name="media-services-learning-paths"></a>Media-palveluiden oppimispolut

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Palautteen antaminen

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


