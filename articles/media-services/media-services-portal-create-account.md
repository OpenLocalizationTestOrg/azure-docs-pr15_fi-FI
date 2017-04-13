<properties
    pageTitle=" Azure Media Services-tilin luominen Azure-portaalissa | Microsoft Azure"
    description="Tässä opetusohjelmassa käydään läpi vaiheet Azure Media Services-tilin luominen Azure-portaalissa."
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
    ms.date="10/24/2016"
    ms.author="juliako"/>


# <a name="create-an-azure-media-services-account-using-the-azure-portal"></a>Azure-portaalissa Azure Media Services-tilin luominen

> [AZURE.SELECTOR]
- [Portal](media-services-portal-create-account.md)
- [PowerShellin](media-services-manage-with-powershell.md)
- [MUILLE KÄYTTÄJILLE](http://msdn.microsoft.com/library/azure/dn194267.aspx)

> [AZURE.NOTE] Jotta voit suorittaa tässä opetusohjelmassa, sinun on Azure-tili. Lisätietoja on artikkelissa [Azure maksuttoman kokeiluversion](https://azure.microsoft.com/pricing/free-trial/). 

Azure-portaalissa on voi luoda nopeasti Azure Media Services (AMS) tili. Voit käyttää Media-palveluja, joiden avulla voit tallentaa, salata, koodata, hallinta ja virtauttaa Azure mediasisällön tilisi. Media Services-tilin luominen milloin voit myös luoda liittyvän tallennustilan tilin (tai Käytä aiemmin luotua) saman tietyn maantieteellisen alueen Media Services-tilinä.

Tässä artikkelissa kerrotaan joitakin yleisiä käsitteitä ja näyttää, miten Media Services-tilin luominen Azure-portaalissa.

## <a name="concepts"></a>Käsitteitä

Media-palveluiden käyttö edellyttää kahta liittyvät tiliä:

- Media Services-tilin. Tilin kautta pääset käsiksi pilvipohjainen Media-palvelut, jotka ovat käytettävissä Azure joukko. Media Services-tilin Tallenna todellinen mediasisältöä. Sen sijaan se tallentaa metatietoja mediasisältöä ja media töitä tilisi. Luo tili aikaa Voit valita käytettävissä Media Services-alue. Valitse alue on tietokeskuksen, joka tallentaa metatietojen tietueet tilissäsi.

    Käytettävissä olevat Media Services (AMS) alueiden ovat seuraavat: Pohjois Europe, Länsi Europe, Länsi US, Yhdysvaltojen Itä, varaaja Aasian, Itä-Aasian japani Länsi ja japani Itä. Media-palveluiden ei käytä affiniteetti ryhmät.
    
    AMS on nyt käytössä myös seuraavat tiedot keskikohdan mukaan: Brasilia Etelä, Intia Länsi, Intia Etelä ja Intia keskitetyn. Voit nyt Azure portaalin Media Service-tilien luominen ja kuvatut eri tehtävien suorittamiseen. Kuitenkin Live koodaus ei ole otettu käyttöön näiden tietojen keskikohdan mukaan. Lisäksi ei kaikenlaisia koodaus varatut resurssit ovat käytettävissä näiden tietojen keskikohdan mukaan.
    
    - Brasilia Etelä: Vain vakio- ja Basic koodaus varatut resurssit ovat käytettävissä.
    - Intia Länsi ja Intia Etelä: 

- Azure-tallennustilan tilin. Tallennustilan tilit on sijaittava saman maantieteellisen alueen Media Services-tilinä. Kun luot Media Services-tilin, voit valita käytössä olevan tallennustilan tilin joko samalla alueella tai voit luoda uuden tallennustilan tilin samalla alueella. Jos poistat Media Services-tilin, liittyvät tallennustilan tilisi blob ei poisteta.

## <a name="create-an-ams-account"></a>AMS-tilin luominen

Tämän osan vaiheet näyttää, miten voit luoda AMS tilin.

1. Kirjaudu sisään osoitteessa [Azure portal](https://portal.azure.com/).
2. Valitse **+ Uusi** > **Web + Mobile** > **Media-palveluita**.

    ![Media-palveluiden luominen](./media/media-services-portal-vod-get-started/media-services-new1.png)

3. Kirjoita **Luo MEDIA SERVICES-tilin** pakolliset arvot.

    ![Media-palveluiden luominen](./media/media-services-portal-vod-get-started/media-services-new3.png)
    
    1. Kirjoita **Tilin nimi**-kenttään uuden AMS tilin nimi. Media Services-tilin nimi on kaikki kirjaimiksi numerot tai kirjaimet ilman välilyöntejä, ja 3-24 merkkiä.
    2. Valitse tilauksen kesken eri Azure-tilauksissa, joihin sinulla on pääsy.
    
    2. **Resurssiryhmä**Valitse uusi tai aiemmin luotu resurssi.  Resurssiryhmä on kokoelma resursseja, jotka jakavat elinkaari, käyttöoikeudet ja käytännöt. Lisätietoja [tästä](azure-resource-manager/resource-group-overview.md#resource-groups).
    3. Valitse **sijainti**, maantieteellisen alueen, jota käytetään tallentaa Media Services-tilin media ja metatiedot-tietueita. Tällä alueella käytetään käsittelemään ja median lisääminen. Vain käytettävissä Media Services alueet näkyvät avattavan luetteloruudun. 
    
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

## <a name="next-steps"></a>Seuraavat vaiheet

Voit nyt ladata tiedostoja AMS-tilillesi. Lisätietoja on artikkelissa [tiedostojen lataaminen](media-services-portal-upload-files.md).

## <a name="media-services-learning-paths"></a>Media-palveluiden oppimispolut

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Palautteen antaminen

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


