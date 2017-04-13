<properties
    pageTitle=" Aloittaminen välittää sisällön Azure-portaalissa pyydettäessä | Microsoft Azure"
    description="Tässä opetusohjelmassa käydään läpi vaiheet käyttöönoton basic videotilauspalveluiden (VoD) sisällön toimittamisen palvelu Azure-portaalissa Azure Media Services (AMS)-sovelluksessa."
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
    ms.topic="get-started-article"
    ms.date="08/30/2016"
    ms.author="juliako"/>


# <a name="get-started-with-delivering-content-on-demand-using-the-azure-portal"></a>Välittää sisällön Azure-portaalissa pyydettäessä käytön aloittaminen

[AZURE.INCLUDE [media-services-selector-get-started](../../includes/media-services-selector-get-started.md)]

Tässä opetusohjelmassa käydään läpi vaiheet basic videotilauspalveluiden (VoD) sisällön toimittamisen palvelun käyttöönoton Azure-portaalissa Azure Media Services (AMS)-sovelluksessa.

> [AZURE.NOTE] Jotta voit suorittaa tässä opetusohjelmassa, sinun on Azure-tili. Lisätietoja on artikkelissa [Azure maksuttoman kokeiluversion](https://azure.microsoft.com/pricing/free-trial/). 

Tässä opetusohjelmassa sisältää seuraavat toimet:

1.  Luo tili Azure Media-palveluita.
2.  Määritä streaming päätepiste.
1.  Lataa videotiedoston.
1.  Koodata lähdetiedosto mukautuvat bittitaajuus MP4-tiedostot joukkoon.
1.  Julkaise resurssi ja Hae streaming ja asteittain Lataa URL-osoitteet.  
1.  Toistaa sisältöä.


## <a name="create-an-azure-media-services-account"></a>Azure Media Services-tilin luominen

Tämän osan vaiheet näyttää, miten voit luoda AMS tilin.

1. Kirjaudu sisään osoitteessa [Azure portal](https://portal.azure.com/).
2. Valitse **+ Uusi** > **Web + Mobile** > **Media-palveluita**.

    ![Media-palveluiden luominen](./media/media-services-portal-vod-get-started/media-services-new1.png)

3. Kirjoita **Luo MEDIA SERVICES-tilin** pakolliset arvot.

    ![Media-palveluiden luominen](./media/media-services-portal-vod-get-started/media-services-new3.png)
    
    1. Kirjoita **Tilin nimi**-kenttään uuden AMS tilin nimi. Media Services-tilin nimi on kaikki kirjaimiksi numerot tai kirjaimet ilman välilyöntejä, ja 3-24 merkkiä.
    2. Valitse tilauksen kesken eri Azure-tilauksissa, joihin sinulla on pääsy.
    
    2. **Resurssiryhmä**Valitse uusi tai aiemmin luotu resurssi.  Resurssiryhmä on kokoelma resursseja, jotka jakavat elinkaari, käyttöoikeudet ja käytännöt. Lisätietoja [tästä](azure-resource-manager/resource-group-overview.md#resource-groups).
    3. **Sijainti**Valitse maantieteellisen alueen käytetään tallentaa Media Services-tilin media ja metatiedot-tietueita. Tällä alueella käytetään Käsittele ja median lisääminen. Vain käytettävissä Media Services alueet näkyvät avattavan luetteloruudun. 
    
    3. **Tallennustilan tilin**Valitse tallennustilan tili antamaan Blob-objektien tallennustilaan mediasisällön Media Services-tililtä. Voit valita olemassa olevan tallennustilan tilin saman maantieteellisen alueen Media Services-tiliksi tai voit luoda tallennustilan tilin. Uusi tallennustilan-tili on luotu samalla alueella. Tallennustilan tilin nimi säännöt ovat samat kuin Media Services tilit.

        Lisätietoja tallennustilan [tähän](storage-introduction.md).

    4. Valitse **raporttinäkymät-ikkunan kiinnittäminen** Nähdäksesi tilin käyttöönoton etenemisen.
    
7. Valitse lomakkeen alareunassa **luominen** .

    Kun tili on luotu, tilaksi muuttuu **käynnissä**. 

    ![Media-palveluiden asetukset](./media/media-services-portal-vod-get-started/media-services-settings.png)

    Voit hallita tiliäsi AMS (esimerkiksi videoiden lataaminen, koodata varat, työn edistymistä) **asetukset** -ikkunaa.

## <a name="manage-keys"></a>Hallitse näppäimet

Tarvitset tilin nimi ja perusavaimen tiedot ohjelmallisesti Media Services-tilin.

1. Azure-portaalissa Valitse tilisi. 

    **Asetukset** -ikkuna oikealla. 

2. Valitse **asetukset** -ikkunassa **avaimet**. 

    **Hallitse näppäimet** windows näyttää tilin nimi ja ensisijainen ja toissijainen näppäimet tulee näkyviin. 
3. Paina kopioitavat arvot Kopioi-painike.
    
    ![Media Services-palveluiden näppäimet](./media/media-services-portal-vod-get-started/media-services-keys.png)

## <a name="configure-streaming-endpoints"></a>Määritä streaming päätepisteet

Kun käsittelet jokin tilanteissa toimittaa video mukautuvat bittitaajuus streaming kautta asiakkaat Azure Media-palveluita. Media-palveluiden tukee seuraavia mukautuvat bittitaajuus streaming tekniikoita: HTTP Live Streaming (HLS), tasainen Streaming, MPEG VIIVA ja HDS (varten vain Adobe PrimeTime/Access asiakirjamalleja).

Media-palvelut on dynaaminen pakkaus, jonka avulla voit pitää mukautuvat bittitaajuus MP4 koodattu sisällön streaming tukemat Media Services (MPEG VIIVA, HLS, tasainen Streaming, HDS) juuri perustuvan, mukaan eikä sinun tarvitse tallentaa valmiiksi pakattu versioiden kaikkien näiden streaming muotoja.

Jos haluat hyödyntää Dynaaminen pakkaus, seuraavasti:

- Koodata mezzanine (lähde)-tiedoston tuominen mukautuvat bittitaajuus MP4-tiedostot (koodauksen vaiheet ovat esitellään jäljempänä tässä opetusohjelmassa).  
- Luo vähintään yksi streaming yksikkö *tietovirta päätepisteen* , joista aiot toimituksen sisältöä. Alla olevia ohjeita noudattamalla streaming yksiköiden määrän muuttaminen.

Dynaaminen pakkaus, jossa tarvitset vain tallentamiseen ja maksaa yhden Tallennusmuoto tiedostoja ja Media-palveluiden muodostaa ja on oikea vastaus asiakaskoneesta pyyntöjen perusteella.

Voit luoda ja muuttaa streaming varattu yksiköiden määrän, toimi seuraavasti:


1. Valitse **asetukset** -ikkunassa **Virtautetun median päätepisteet**. 

2. Valitse streaming päätepisteen oletusarvo. 

    **Oletus-TIETOVIRTA PÄÄTEPISTEEN tiedot** -ikkuna.

3. Jos haluat määrittää streaming yksiköiden määrän, **Virtautetun median yksiköt** liukusäädintä.

    ![Streaming yksiköt](./media/media-services-portal-vod-get-started/media-services-streaming-units.png)

4. Tallenna muutokset valitsemalla **Tallenna** .

    >[AZURE.NOTE]Kaikki uudet yksiköt kohdistus voi kestää jopa 20 minuuttia.

## <a name="upload-files"></a>Tiedostojen lataaminen

Virta ja videot Azure Media-palveluiden avulla, sinun täytyy lähde-videoiden lataaminen, koodata ne yhdeksi useita bitrates ja julkaista tulos. Tässä osassa käsitellään ensimmäiseksi. 

1. Valitse **asetus** -ikkunasta **resurssit**.

    ![Tiedostojen lataaminen](./media/media-services-portal-vod-get-started/media-services-upload.png)

3. Napsauttamalla **Lataa** -painiketta.

    **Lataa video resurssi** -ikkuna tulee näkyviin.

    >[AZURE.NOTE] Ei ole tiedoston enimmäiskoko.
    
4. Selaa haluamasi video tietokoneesta, valitse se ja valitse OK.  

    Lataus alkaa, ja näet tiedostonimen edistymisen.  

Kun lataus on valmis, näkyviin tulee uusi resurssi **kalusto** -ikkunassa. 

## <a name="encode-assets"></a>Koodata resurssit

Kun käsittelet jokin tilanteissa toimittaa mukautuvat bittitaajuus streaming asiakkaiden Azure Media-palveluita. Media-palveluiden tukee seuraavia mukautuvat bittitaajuus streaming tekniikoita: HTTP Live Streaming (HLS), tasainen Streaming, MPEG VIIVA ja HDS (varten vain Adobe PrimeTime/Access asiakirjamalleja). Videoita valmisteleminen mukautuvat bittitaajuus streaming-haluat koodata lähteen videon usean bittitaajuus-tiedostoiksi. Käytettävä **Media Encoder vakio** encoder koodata videoita.  

Media-palvelut sekä Dynaaminen pakkaus, jonka avulla voit pitää usean bittitaajuus MP4s streaming seuraavissa muodoissa: MPEG VIIVA, HLS, tasainen Streaming tai HDS ilman, että sinun tarvitsee pakata huomioon nämä streaming muotoja. Dynaaminen pakkaus, jossa tarvitset vain tallentamiseen ja maksaa yhden Tallennusmuoto tiedostoja ja Media-palveluiden muodostaa ja on oikea vastaus asiakaskoneesta pyyntöjen perusteella.

Jos haluat hyödyntää Dynaaminen pakkaus, seuraavasti:

- Koodata lähdetiedoston usean bittitaajuus MP4-tiedostot (koodauksen vaiheet ovat jäljempänä tässä osassa esiteltyjä) joukkoon.
- Vähintään yhden streaming yksikön streaming päätepisteen, josta aiot toimituksen sisällön hakeminen Lisätietoja on artikkelissa [määrittäminen streaming päätepisteet](media-services-portal-vod-get-started.md#configure-streaming-endpoints). 

### <a name="to-use-the-portal-to-encode"></a>Koodata portaalin avulla

Tässä osassa kuvataan, miten voit koodata käytettäessä Media Encoder Standard sisältöä.

1.  Valitse **asetukset** -ikkunassa **resurssit**.  
2.  Valitse resurssi, jotka haluat koodata **kalusto** -ikkunassa.
3.  Valitse **Käytä** -painike.
4.  Valitse **Käytä amerikkalaisen** -ikkunassa "Media Encoder vakio-suoritin ja esiasetus. Esimerkiksi jos tiedät syötteen video on 1920 x 1080 kuvapisteen tarkkuutta, valitse-sovelluksella "H264 useita bittitaajuus 1080p" esiasetettuja. Lisätietoja Esiasetukset, katso [Tässä](https://msdn.microsoft.com/library/azure/mt269960.aspx) artikkelissa – on tärkeää Valitse esiasetus, joka sopii parhaiten syötettäsi video. Jos sinulla on videon pienitarkkuuksisessa näytössä (640 x 360) ja valitse sinun ei olisi käytettävä oletusarvo "H264 useita bittitaajuus 1080p" esiasetettuja.
    
    Helpottaa hallintaa varten on vaihtoehto muokata tulosteen kohteen nimi ja projektin nimeä.
        
    ![Koodata resurssit](./media/media-services-portal-vod-get-started/media-services-encode1.png)
5. Paina **luominen**.

### <a name="monitor-encoding-job-progress"></a>Koodauksen työn edistymistä

Koodauksen työn edistymistä, valitse **asetukset** (sivun yläosassa) ja valitse **työt**.

![Projektit](./media/media-services-portal-vod-get-started/media-services-jobs.png)

## <a name="publish-content"></a>Sisällön julkaiseminen

Voit antaa käyttäjän URL-osoite, jonka avulla voidaan käyttää tai ladata sisältöä, sinun on "julkaiseminen" oman resurssi locator luomalla. Locators tarjoavat pääsyn kohteen olevat tiedostot. Media-palveluiden tukee kahdentyyppisiä locators: 

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

>[AZURE.NOTE] Jos olet käyttänyt portaalin luominen locators ennen maaliskuussa 2015, locators, jossa on kahden vuoden vanhentumispäivä on luotu.  

Päivitä vanhenemispäivämäärä, locator käyttämällä [REST](http://msdn.microsoft.com/library/azure/hh974308.aspx#update_a_locator ) - tai [.NET](http://go.microsoft.com/fwlink/?LinkID=533259) -ohjelmointirajapinnan. Kun päivität SAS locator vanhenemispäivää, URL-osoite muuttuu.

### <a name="to-use-the-portal-to-publish-an-asset"></a>Julkaise sijoituksen portaalin avulla

Portaalin avulla voit julkaista sijoituksen, toimi seuraavasti:

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

##<a name="next-steps"></a>Seuraavat vaiheet

Tarkista oppimispolut Media-palveluita.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Palautteen antaminen

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


