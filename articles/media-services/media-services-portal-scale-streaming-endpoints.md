<properties
    pageTitle=" Skaalaa streaming päätepisteet Azure-portaalissa | Microsoft Azure"
    description="Tässä opetusohjelmassa käydään läpi vaiheet skaalaus streaming päätepisteet Azure-portaalissa."
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


# <a name="scale-streaming-endpoints-with-the-azure-portal"></a>Skaalaa streaming päätepisteet Azure-portaalissa

##<a name="overview"></a>Yleiskatsaus

> [AZURE.NOTE] Jotta voit suorittaa tässä opetusohjelmassa, sinun on Azure-tili. Lisätietoja on artikkelissa [Azure maksuttoman kokeiluversion](https://azure.microsoft.com/pricing/free-trial/). 

Kun käsittelet jokin tilanteissa toimittaa video mukautuvat bittitaajuus streaming kautta asiakkaat Azure Media-palveluita. Media-palveluiden tukee seuraavia mukautuvat bittitaajuus streaming tekniikoita: HTTP Live Streaming (HLS), tasainen Streaming, MPEG VIIVA ja HDS (varten vain Adobe PrimeTime/Access asiakirjamalleja).

Media-palvelut on dynaaminen pakkaus, jonka avulla voit pitää mukautuvat bittitaajuus MP4 koodattu sisällön streaming tukemat Media Services (MPEG VIIVA, HLS, tasainen Streaming, HDS) juuri perustuvan, mukaan eikä sinun tarvitse tallentaa valmiiksi pakattu versioiden kaikkien näiden streaming muotoja.

Jos haluat hyödyntää Dynaaminen pakkaus, seuraavasti:

- Koodata mezzanine (lähde)-tiedoston tuominen säädön bittitaajuus MP4-tiedostot (koodauksen vaiheet ovat esitellään jäljempänä tässä opetusohjelmassa).  
- Luo vähintään yksi streaming yksikkö *tietovirta päätepisteen* , joista aiot toimituksen sisältöä. Alla olevia ohjeita noudattamalla streaming yksiköiden määrän muuttaminen.

Dynaaminen pakkaus tarvitset vain tallentamiseen ja maksaa yhden Tallennusmuoto tiedostot sekä Media Services luominen ja yhteyshenkilönä oikea vastaus asiakaskoneesta pyyntöjen perusteella.

Lisäksi voit hallita Streaming päätepisteen-palvelun kapasiteetti käsittelemään säätämällä streaming yksiköt kasvava kaistanleveyden tarpeisiin. On suositeltavaa varata vähintään yksi aikayksikön tuotantoympäristössä sovellukset. Streaming yksiköt antaa sinulle sekä erillinen juniin kapasiteetin, joka voi ostaa kerrallaan 200 Mbps ja muita toimintoja, mitä toimintoja, mukaan lukien: [Dynaaminen pakkaus](media-services-dynamic-packaging-overview.md), CDN integrointi ja lisämäärityksiä. Lisätietoja on artikkelissa [Hallitse streaming päätepisteet Azure-portaalissa](media-services-portal-manage-streaming-endpoints.md).

## <a name="scale-streaming-endpoints"></a>Streaming päätepisteet asteikko

Voit luoda ja muuttaa streaming varattu yksiköiden määrän, toimi seuraavasti:

1. Valitse [Azure portal](https://portal.azure.com/)Azure Media Services-tilin.
2. Valitse **asetukset** -ikkunassa **Virtautetun median päätepisteet**.
3. Valitse streaming päätepiste, jonka haluat skaalata. 
4. Liukusäätimen avulla streaming yksiköiden määrän määrittäminen
 
![Päätepisteen Streaming](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints3.png)

Ottaa huomioon seuraavat asiat:

- Kaikki uudet streaming yksiköt kohdistus voi kestää noin 20 minuutin ajan suorittamiseen. 
- Tällä hetkellä siirtymällä mikä tahansa positiivinen arvosta streaming yksiköt takaisin ei mitään, voit poistaa käytöstä ylöspäin tunnin streaming tarvittaessa.
- 24 tunnin määritettyjen yksikköjen suurinta numeroa käytetään laskeminen kustannukset. Lisätietoja hinnat tiedot on artikkelissa [Media Services hinnat tiedot](http://go.microsoft.com/fwlink/?LinkId=275107).

##<a name="next-steps"></a>Seuraavat vaiheet

Tarkista oppimispolut Media-palveluita.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Palautteen antaminen

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


