<properties
    pageTitle="Analysoi mediatiedostojen Azure-portaalissa | Microsoft Azure"
    description="Tässä ohjeaiheessa kerrotaan, miten käsittelemään mediatiedostojen Azure-portaalissa Media Analytics media-suorittimet (MP) kanssa."
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


# <a name="analyze-your-media-using-the-azure-portal"></a>Analysoi mediatiedostojen Azure-portaalissa

> [AZURE.NOTE] Jotta voit suorittaa tässä opetusohjelmassa, sinun on Azure-tili. Lisätietoja on artikkelissa [Azure maksuttoman kokeiluversion](https://azure.microsoft.com/pricing/free-trial/). 

## <a name="overview"></a>Yleiskatsaus

Azure Media-palveluiden Analytics on kokoelma, puhe ja vision komponentit (yrityksen mittakaava, yhteensopivuuden, suojaus ja yleisen reach), jotka helpottavat organisaatiot ja yritykset saada suoritettavia havainnollistamisen niiden videon tiedostoista. Katso tarkempia yleiskatsaus Azure Media Services Analytics [tämän](media-services-analytics-overview.md) ohjeaiheen. 

Tässä ohjeaiheessa kerrotaan, miten käsittelemään mediatiedostojen Azure-portaalissa Media Analytics media-suorittimet (MP) kanssa. Media Analytics MP tuottaa MP4-tiedostoja tai JSON-tiedostoja. Jos media-suoritin MP4-tiedoston, voit ladata tiedoston asteittain. Jos media-suoritin JSON-tiedoston, tiedosto voi ladata Azure-blob-säiliö. 

## <a name="choose-an-asset-that-you-want-to-analyze"></a>Valitse resurssi, jonka haluat analysoida 
 
1. Valitse [Azure portal](https://portal.azure.com/)Azure Media Services-tilin.
2. Valitse **asetukset** -ikkunassa **resurssit**.  
.
    ![Analysoi videot](./media/media-services-portal-analyze/media-services-portal-analyze001.png)

2. Valitse resurssi, jotka haluat analysoida ja valitse **Analysoi** -painike.
        
    ![Analysoi videot](./media/media-services-portal-analyze/media-services-portal-analyze002.png)

3. Valitse suoritin **prosessin media kohteiden kanssa Media Analytics** -ikkunassa. 

    Loput on artikkelissa kerrotaan, miksi ja Opi käyttämään suoritin. 
   
4. Painamalla **luominen** alusta työn.

## <a name="azure-media-indexer"></a>Azure Media indeksointitoiminto

**Azure Media indeksointitoiminto** media-suoritin avulla voit tehdä mediatiedostojen ja sisällön etsittävän sekä luoda suljettu tekstitys raidat. Tämä osat antaa joitakin asetuksia, jotka voit määrittää tämän MP-tietoja.

![Analysoi videot](./media/media-services-portal-analyze/media-services-portal-analyze003.png)

### <a name="language"></a>Kieli

Luonnollisen kielen tunnistetaan multimediatiedoston. Esimerkiksi englanti ja Espanjassa. 

### <a name="captions"></a>Kuvatekstit

Voit valita, kuvateksti-muodossa, joka luodaan sisältöä. Indeksoinnin työn luoda tekstityksen tiedostoja seuraavissa muodoissa:  

- **SAAME**
- **TTML**
- **WebVTT**

Suljettu otsikko (kopio) Nämä tiedostomuodot voidaan tehdä ääni- ja videotiedostoista helppokäyttöisempiä kuuleminen disability kärsivät ihmiset.

### <a name="aib-file"></a>AIB-tiedosto

Valitse tämä asetus, jos haluat luoda mukautetun SQL Server-IFilter käytettäväksi äänen indeksi-Blob-tiedostoon. Lisätietoja on artikkelissa [Tämä](https://azure.microsoft.com/blog/using-aib-files-with-azure-media-indexer-and-sql-server/) blogiin.

### <a name="keywords"></a>Avainsanat

Valitse tämä asetus, jos haluat luoda avainsanat XML-tiedosto. Tiedoston nimi sisältää avainsanoja poimittujen puhe sisältöä, korkojakso ja siirtymä tiedot.

### <a name="job-name"></a>Työn nimi

Kutsumanimi, jolla voit tunnistaa työn. [Tässä](media-services-portal-check-job-progress.md) artikkelissa kuvataan, miten voit seurata projektin etenemistä. 

### <a name="output-file"></a>Kohdetiedosto

Kutsumanimi, jolla voit tunnistaa tulosteen sisältö. 

## <a name="azure-media-hyperlapse"></a>Azure Media Hyperlapse

Azure Media Hyperlapse on MP, joka luo tasainen aika lakkasi olemasta voimassa videot ensimmäinen henkilö tai toiminnon kameran sisällöstä.  Lisätietoja on artikkelissa [tämän](media-services-hyperlapse-content.md) ohjeaiheen. Tämä osat antaa joitakin asetuksia, jotka voit määrittää tämän MP-tietoja.

![Analysoi videot](./media/media-services-portal-analyze/media-services-portal-analyze004.png)

### <a name="speed"></a>Nopeus 

Määrittää nopeus, jota käytetään nopeuttaa syötteen video. Tulos on vakiintunut ja aikaa lakkasi olemasta voimassa kuvakäännöksen syöttö-video.

### <a name="job-name"></a>Työn nimi

Kutsumanimi, jolla voit tunnistaa työn. [Tässä](media-services-portal-check-job-progress.md) artikkelissa kuvataan, miten voit seurata projektin etenemistä. 

### <a name="output-file"></a>Kohdetiedosto

Kutsumanimi, jolla voit tunnistaa tulosteen sisältö. 

## <a name="azure-media-face-detector"></a>Azure Media hymiö tunnistus

**Azure Media hymiö ilmaisimen** media-suoritin (MP) avulla voit laskea ja seurata liikkeitä mitta jopa yleisön osallistumisen ja reaktion kasvojen lausekkeiden kautta. Tämä palvelu on kaksi ominaisuudet: 

- **Hymiö tunnistus**

    Hymiö tunnistaminen löytää, ja seuraa ihmisten naamoja videon kuluessa. Useita hymiöiden voidaan havaita ja sen jälkeen voi seurata, kun ne siirtyvät ympärille, JSON-tiedoston palautettu aika ja sijainti metatietojen kanssa. Seuranta-aikana se yrittää antaa yhdenmukaisia tunnus saman hymiö samalla, kun henkilö on siirtyminen näytössä, vaikka ne ovat silmiä tai jätä lyhyesti kehys.

    >[AZURE.NOTE]Tämä services Suorita kasvojen tunnistuksen. Henkilö, joka poistuu kehyksen tai tulee silmiä varten liian pitkä kiinnitetään uuden tunnuksen, kun ne palauttavat.

- **Tunteet tunnistus**
    
    Tunteet tunnistus on valinnainen osa hymiö tunnistus Media suorittimen, joka palauttaa analyysi useita tunnesiteisiin määritteissä naamoja havaita, mukaan lukien onnea, sadness, jäänyt, anger ja lisää. 

![Analysoi videot](./media/media-services-portal-analyze/media-services-portal-analyze005.png)

### <a name="detection-mode"></a>Tunnistamisen tilassa

Jokin seuraavista tiloista voidaan käyttää suoritin:

- hymiö tunnistus
- kohti hymiö tunteet tunnistus
- kooste tunteet tunnistus

### <a name="job-name"></a>Työn nimi

Kutsumanimi, jolla voit tunnistaa työn. [Tässä](media-services-portal-check-job-progress.md) artikkelissa kuvataan, miten voit seurata projektin etenemistä. 

### <a name="output-file"></a>Kohdetiedosto

Kutsumanimi, jolla voit tunnistaa tulosteen sisältö. 

## <a name="azure-media-motion-detector"></a>Azure Media liikerata tunnistus

**Azure Media liikerata ilmaisimen** media-suoritin (MP) avulla voit tunnistaa tehokkaasti osien halutut sisällä muussa pitkä ja uneventful video. Liikerata-tunnistus voidaan staattiset kameran videomateriaali-tunnistavan osien videon, jossa liikerata tapahtuu. Se luo metatietojen aikaleimat ja ympäröivä alue, jossa tapahtuma tapahtui sisältävän JSON-tiedoston.

Kohdistettu kohti suojauksen videovirtaa, tämä tekniikka on voivat luokitella liikerata haluamasi tapahtuma- ja tunnistettujen varjostusten ja valaistus muutokset. Voit muodosta suojausvaroitusten kameran syötteet ilman todennäköisyydellä roskapostia ja päättömät ole merkitystä tapahtumat, mutta ei voi poimia aikaa halutut erittäin pitkä valvonta videot.

![Analysoi videot](./media/media-services-portal-analyze/media-services-portal-analyze006.png)

## <a name="azure-media-video-thumbnails"></a>Azure Media Video pikkukuvat

Suorittimen avulla voit luoda yhteenvetojen pitkä videoita valitsemalla kiinnostavat katkelmat automaattisesti lähde-video. Tästä on hyötyä, kun haluat lisätä pikaisesti toiminta pitkä video. Yksityiskohtaisia ohjeita ja esimerkkejä artikkelissa [Käytä Azure Media Video pikkukuvat Video yhteenvedon luominen](media-services-video-summarization.md)

![Analysoi videot](./media/media-services-portal-analyze/media-services-portal-analyze008.png)

### <a name="job-name"></a>Työn nimi

Kutsumanimi, jolla voit tunnistaa työn. [Tässä](media-services-portal-check-job-progress.md) artikkelissa kuvataan, miten voit seurata projektin etenemistä. 

### <a name="output-file"></a>Kohdetiedosto

Kutsumanimi, jolla voit tunnistaa tulosteen sisältö. 


##<a name="next-steps"></a>Seuraavat vaiheet

Näytä mediapalvelut oppimispolut.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Palautteen antaminen

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


