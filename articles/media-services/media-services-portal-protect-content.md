<properties 
    pageTitle="Azure-portaalissa sisällön suojauksen käytäntöjen määrittäminen | Microsoft Azure" 
    description="Tässä artikkelissa kerrotaan, miten sisällön suojauksen käytäntöjen määrittäminen Azure portaalin avulla. Artikkelin näkyvät myös ottamisesta käyttöön oman varat dynaaminen salausta." 
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

# <a name="configuring-content-protection-policies-using-the-azure-portal"></a>Azure-portaalissa sisällön suojauksen käytäntöjen määrittäminen

> [AZURE.NOTE] Jotta voit suorittaa tässä opetusohjelmassa, sinun on Azure-tili. Lisätietoja on artikkelissa [Azure maksuttoman kokeiluversion](https://azure.microsoft.com/pricing/free-trial/).

## <a name="overview"></a>Yleiskatsaus

Microsoft Azure Media Services (AMS) avulla voit suojata mediatiedostojen se jättää tietokoneeseen tallentaminen, käsittely ja toimittaminen ajasta. Media-palveluiden avulla voit pitää sisällön salattu dynaamisesti ja salauksen AES (Advanced Standard) (joko 128 bittistä salausavaimet), Yleiset salaus (CENC) PlayReady ja/tai Widevine DRM ja Apple FairPlay. 

AMS tarjoaa palvelun välittää DRM käyttöoikeuksia ja AES Poista näppäimet valtuutettujen asiakkaille. Azure portaalin avulla voit luoda yksi **avain/käyttöoikeuden luvan käytännön** kaikentyyppisissä salausta varten.

Tässä artikkelissa kerrotaan, miten voit määrittää sisällön suojauksen käytännöt Azure-portaalissa. Artikkelin näkyvät myös ottamisesta käyttöön oman varat dynaaminen salausta.

> [AZURE.NOTE]  Jos olet käyttänyt Azure perinteinen portaalin avulla voit luoda suojaus käytäntöjä, käytännöt ei näy [Azure portal](https://portal.azure.com/). Kuitenkin kaikki vanhan käytännöt yhä. Voit tarkastella niitä Azure Media Services .NET SDK tai [Azure-Media-palvelut-Explorer](https://github.com/Azure/Azure-Media-Services-Explorer/releases) -työkalun avulla (käytännöt näkyviin-kohteen hiiren kakkospainikkeella > Näytä tiedot (F4) -> Valitse sisällön avaimet välilehdessä -> Valitse avaimen). 
> 
> Jos haluat salata oman resurssi käyttämällä uusia käytäntöjä, määrittää niitä Azure-portaalissa, valitse Tallenna ja ottaminen uudelleen käyttöön dynaaminen salausta. 

## <a name="start-configuring-content-protection"></a>Käynnistä sisällön suojauksen määrittäminen

Portaalin avulla voit aloittaa määrittäminen sisällön suojauksen, yleinen AMS tiliisi, toimi seuraavasti:

1. Valitse [Azure portal](https://portal.azure.com/)Azure Media Services-tilin.
2. Valitse **asetukset** > **sisällön suojauksen**.

![Sisällön suojaaminen](./media/media-services-portal-content-protection/media-services-content-protection001.png)
 

## <a name="keylicense-authorization-policy"></a>Avaimen/käyttöoikeuden luvan käytäntö

AMS tukee useita tapoja käyttäjät, jotka avain tai -käyttöoikeuksia pyytää todennustapa. Sisällön avaimen todennus-käytäntö on voit määrittää ja täytettävä asiakkaan järjestyksessä avain/käyttöoikeuden delived asiakkaalle. Sisällön avaimen todennus-käytäntö voi olla yksi tai useampi käyttöoikeuksien rajoitukset: **Avaa** tai **Suojaustunnuksen** rajoitus.

Azure portaalin avulla voit luoda yksi **avain/käyttöoikeuden luvan käytännön** kaikentyyppisissä salausta varten.

###<a name="open"></a>Avaa 

Avaa rajoituksia tarkoittaa, että järjestelmä pitää avain kaikkien sellaisten käyttäjien, niiden tekijöistä avaimen pyynnön. Tämä rajoitus voi olla hyötyä testaus. 

### <a name="token"></a>Suojaustunnuksen

Suojaustunnuksen rajoitettu käytäntö on liitettävä mukaan suojattu suojaustunnuksen Service (STS) antanut tunnusta. Media-palveluiden tukee tunnusten yksinkertaisen Web tunnusten (SWT)-muodossa ja JSON Web suojaustunnuksen (JWT)-muodossa. Media-palveluiden ei tarjoa suojatun suojaustunnuksen palvelut. Voit luoda mukautetun STS tai hyödyntää Microsoft Azure ACS ongelman tunnukset. Luoda määritetyn avaimen ja ongelman saatavat, jonka olet määrittänyt suojaustunnuksen rajoituksen määritys on allekirjoitettu tunnuksen on oltava määritettynä STS. Avaimen toimituksen Media palvelut palauttaa pyydettyjen key (tai käyttöoikeus) asiakas, jos tunnus on kelvollinen ja saatavat suojaustunnuksen vastaavat toisiaan ne määritetty key (tai käyttöoikeus).

Kun käytäntö määritetään tunnuksen rajoitettu, ensisijainen vahvistus-näppäintä, myöntäjä ja yleisön parametrit on määritettävä. Ensisijainen vahvistus-avain sisältää avain, joka on allekirjoitettu tunnuksen, myöntäjä on suojattu suojaustunnuksen palvelu, joka ongelmat tunnuksen. (Kutsutaan joskus laajuus) yleisön kuvataan tunnuksen palveluita tai resurssin tunnus sallii käyttöoikeus. Media-palveluiden avaimen toimitus-palvelu tarkistaa tunnuksen kyseiset arvot vastaavat arvot mallissa.

![Sisällön suojaaminen](./media/media-services-portal-content-protection/media-services-content-protection002.png)

## <a name="playready-rights-template"></a>PlayReady malli

Lisätietoja PlayReady haluamasi malli artikkelissa [Media Services PlayReady käyttöoikeuden mallin yleiskatsaus](media-services-playready-license-template-overview.md).

### <a name="non-persistent"></a>Muut pysyvä

Jos määrität käyttöoikeuden kuin tilapäisten, se vain pidetään muistissa Playerin on käyttäessä käyttöoikeus.  

![Sisällön suojaaminen](./media/media-services-portal-content-protection/media-services-content-protection003.png)

### <a name="persistent"></a>Jatkuva

Jos määrität käyttöoikeuden kuin pysyvä, se tallennetaan asiakkaan pysyvän säilön.

![Sisällön suojaaminen](./media/media-services-portal-content-protection/media-services-content-protection004.png)

## <a name="widevine-rights-template"></a>Widevine malli

Yksityiskohtaisia tietoja Widevine haluamasi malli on artikkelissa [Widevine käyttöoikeuden mallin yleiskatsaus](media-services-widevine-license-template-overview.md).

### <a name="basic"></a>Perustoiminnot

Kun valitset **Basic**-mallin luodaan kaikki oletusasetusten arvot.

### <a name="advanced"></a>Lisäasetukset

Katso [tämän](media-services-widevine-license-template-overview.md) ohjeaiheen tarkan tietoja Widevine määrityksiä advance-vaihtoehtoa.

![Sisällön suojaaminen](./media/media-services-portal-content-protection/media-services-content-protection005.png)

## <a name="fairplay-configuration"></a>FairPlay määritys

FairPlay salaus käyttöön, sinun täytyy kirjoittaa sovelluksen varmenteen ja sovelluksen salaisuus Key (KYSY) FairPlay määritysvaihtoehto kautta. [Tässä](media-services-protect-hls-with-fairplay.md) artikkelissa lisätietoja FairPlay määritys ja vaatimuksia.

![Sisällön suojaaminen](./media/media-services-portal-content-protection/media-services-content-protection006.png)

## <a name="apply-dynamic-encryption-to-your-asset"></a>Dynaaminen salaa oman resurssi

Jos haluat hyödyntää dynaaminen salausta, seuraavasti:

- Koodata lähdetiedoston mukautuvat bittitaajuus MP4-tiedostot joukkoon.
- Vähintään yhden tarvittaessa streaming yksikön streaming päätepisteen, joista voit taata aikana sisällön hakeminen Lisätietoja on artikkelissa [miten tarvittaessa streaming varatut resurssit](media-services-portal-manage-streaming-endpoints.md).

### <a name="select-an-asset-that-you-want-to-encrypt"></a>Valitse resurssi, jonka haluat salata

Voit tarkastella kaikkia kohteita valitsemalla **asetukset** > **resurssit**.

![Sisällön suojaaminen](./media/media-services-portal-content-protection/media-services-content-protection007.png)

### <a name="encrypt-with-aes-or-drm"></a>Salaa AES tai DRM

Kun painat **Salaa** sijoituksen, näyttöön tulee kaksi vaihtoehtoa: **AES** tai **DRM**. 

#### <a name="aes"></a>AES

Tyhjennä salausavaimen otetaan käyttöön kaikissa protokollista AES: tasainen Streaming, HLS ja MPEG-VIIVA.

![Sisällön suojaaminen](./media/media-services-portal-content-protection/media-services-content-protection008.png)

#### <a name="drm"></a>DRM

Kun valitset DRM-välilehti, näyttöön tulee erilaisia vaihtoehtoja sisällön suojauksen käytäntöjen (joka on määrittänyt mennessä) + streaming protokollat joukko.

- **PlayReady ja Widevine viivoilla MPEG** - dynaamisesti salaa MPEG-VIIVA-tietovirta PlayReady ja Widevine DRMs.
- **PlayReady ja MPEG-VIIVA + FairPlay kanssa HLS-Widevine** - dynaamisesti salaa voit MPEG-VIIVA stream PlayReady ja Widevine DRMs. Myös salaa HLS-virtaa FairPlay kanssa.
- **PlayReady osallistuvan tasainen Streaming, HLS ja MPEG-VIIVA** - dynaamisesti salaa tasainen tietovirta, HLS, MPEG-VIIVA virtaa PlayReady DRM kanssa.
- **Widevine osallistuvan MPEG-VIIVA** - dynaamisesti salaa voit MPEG-VIIVA Widevine DRM kanssa.
- **FairPlay osallistuvan HLS** - dynaamisesti salaa HLS-tietovirta FairPlay kanssa.

FairPlay salaus käyttöön, sinun täytyy kirjoittaa sovelluksen varmenteen ja sovelluksen salaisuus Key (KYSY) – FairPlay määritysvaihtoehto sisällön suojauksen asetukset-sivu.

![Sisällön suojaaminen](./media/media-services-portal-content-protection/media-services-content-protection009.png)

Kun olet määrittänyt salauksen asetukset, paina **Käytä**.

##<a name="next-steps"></a>Seuraavat vaiheet

Tarkista oppimispolut Media-palveluita.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Palautteen antaminen

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]





