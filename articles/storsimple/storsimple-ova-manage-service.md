<properties 
   pageTitle="Käyttöönotto StorSimple hallintapalvelu StorSimple virtual matriisin | Microsoft Azure"
   description="Kerrotaan, miten voit luoda ja poistaa Azure perinteinen portaalissa StorSimple hallintapalvelu ja kerrotaan, miten voit hallita palvelua rekisteröinti-näppäintä."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="05/19/2016"
   ms.author="alkohli" />

# <a name="deploy-the-storsimple-manager-service-for-storsimple-virtual-array"></a>Ottaa käyttöön StorSimple Virtual matriisin StorSimple hallintapalvelu

## <a name="overview"></a>Yleiskatsaus

StorSimple hallintapalvelu käytetään Microsoft Azure ja muodostaa yhteyden StorSimple useilla eri laitteilla. Kun olet luonut palvelu, voit sen laitteiden hallinta-portaalista Microsoft Azure perinteinen selaimessa. Voit valvoa yhteen-, Keski sijainnista, siten pienentämisen hallinnollisten rasitteiden StorSimple hallintapalveluun liitettyjen laitteiden.

StorSimple hallinta-aloitussivu on lueteltu kaikki StorSimple hallinnan palvelut, joiden avulla voit hallita StorSimple tallennuslaitteet. Kunkin StorSimple hallinta-palvelun esitetään StorSimple hallinta-sivulla seuraavat tiedot:

- **Nimi** : nimi, joka on määritetty StorSimple hallintapalveluun, kun se on luotu. Palvelun nimessä ei voi muuttaa sen luomisen jälkeen.

- **Tila** – palvelu, joka voi olla **aktiivinen**, **luominen**tai **online-palvelun**tila.

- **Sijainti** : maantieteellinen sijainti, johon StorSimple laitteen otetaan käyttöön.

- **Tilauksen** – laskutuksen tilauksesta, joka on liitetty palvelussa.

Yleisiä tehtäviä, joita voi käyttää StorSimple hallinta-sivulla ovat seuraavat:

- Luo palvelu
- Palvelun poistaminen
- Palvelun rekisteröinti Key-tunnuksen hankkiminen
- Palvelun rekisteröinti avain uudelleen

Tässä opetusohjelmassa kerrotaan, miten näiden tehtävien suorittamiseen. Tässä artikkelissa tietoja koskee vain StorSimple Virtual matriiseja. Lisätietoja StorSimple 8000 sarjan Siirry [StorSimple hallintapalvelu käyttöönotto](storsimple-manage-service.md).

## <a name="create-a-service"></a>Luo palvelu

**Nopea luominen** -vaihtoehdon avulla voit luoda StorSimple hallintapalveluun, jos haluat ottaa käyttöön StorSimple laitteen. Jos haluat luoda palvelu, on:

- Enterprise Agreement-sopimus-tilaus
- Tilin aktiivinen Microsoft Azure-tallennustilan
- Laskutustiedot, jota käytetään käyttöoikeushallinta

Voit myös luoda tallennustilan oletustilin luodessasi palvelun.

Yksittäistä voit hallita useita laitteita. Laite ei voi kuitenkin olla palveluihin. Suuri yritys voi olla useita palveluesiintymiä eri tilaukset, organisaatiot tai jopa käyttöönoton sijainnit-käyttöä varten.  

> [AZURE.NOTE] Sinun on eri esiintymien StorSimple hallintapalvelu StorSimple 8000 sarjan laitteet ja StorSimple Virtual matriiseja.

Seuraavien toimien palvelun luomiseen.

[AZURE.INCLUDE [storsimple-ova-create-new-service](../../includes/storsimple-ova-create-new-service.md)]

## <a name="delete-a-service"></a>Palvelun poistaminen

Ennen kuin poistat palvelu, varmista, että ei ole yhdistetty laitteiden käytön yhteydessä. Jos palvelu on käytössä, poistaa yhdistettyjen laitteiden. Aktivointi-toiminnon palvelimen laite ja palvelun välinen yhteys, mutta säilyttää pilveen laitteen tiedot. 

> [AZURE.IMPORTANT] Kun palvelu on poistettu, toimintoa ei voi kumota. 

Seuraavien toimien Poista palvelu.

### <a name="to-delete-a-service"></a>Jos haluat poistaa palvelun

1. Valitse **StorSimple hallinta** -sivulla palvelun, josta haluat poistaa.

1. Valitse sivun alareunassa **Poista** .

1. Valitse **Kyllä** , jos vahvistus-ilmoituksen. Voi kestää muutaman minuutin kuluttua-palvelun poistamista.

## <a name="get-the-service-registration-key"></a>Palvelun rekisteröinti Key-tunnuksen hankkiminen

Kun olet luonut palvelu, sinun on rekisteröidä StorSimple laitteen-palveluun. Rekisteröi ensimmäisen StorSimple laitteen, tarvitset palvelun rekisteröinti-näppäintä. Voit rekisteröidä muita laitteita StorSimple-palvelussa, tarvitset rekisteröinti-näppäintä ja palvelun tiedot salausavaimen (joka on luotu ensimmäisen laitteen rekisteröinnin yhteydessä). Saat lisätietoja palvelun tiedot salausavaimen [Hae palvelun tiedot salausavaimen paikallisen sivuston Käyttöliittymä](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key). 

Seuraavien toimien saat palvelun rekisteröinti-näppäintä.

[AZURE.INCLUDE [storsimple-ova-get-service-registration-key](../../includes/storsimple-ova-get-service-registration-key.md)]

Ota palvelun rekisteröinti avainta turvalliseen paikkaan. Tarvitset tätä näppäintä sekä palvelun tiedot salausavaimen, voit rekisteröidä muita laitteita tämän palvelun avulla. Tarvitset saatuaan palvelun rekisteröinti avainta määrittäminen Windows PowerShellin kautta laitteen StorSimple liittymän.

## <a name="regenerate-the-service-registration-key"></a>Palvelun rekisteröinti avain uudelleen

Tarvitset palvelun rekisteröinti näppäintä uudelleen, jos sinun on suoritettava avaimen kierron tai palvelun järjestelmänvalvojat luettelo on muuttunut. Kun avain uudelleen, uusi avain käytetään vain seuraavien laitteiden rekisteröitymisestä. Laitteet, jotka on jo rekisteröity ovat ei vaikuta tällä menetelmällä.

Seuraavien toimien palvelun rekisteröinti näppäintä uudelleen.

### <a name="to-regenerate-the-service-registration-key"></a>Palvelun rekisteröinti avain uudelleen

1. Valitse **StorSimple hallinta** -sivulla **Rekisteröinti-näppäintä**.

1. Valitse **Palvelun rekisteröinti avainta** -valintaikkunan **uudelleen**.

1. Näyttöön tulee vahvistussanoma. Valitse **OK** uudistaminen Jatka.

1. Uusi palvelu rekisteröinti avain tulee näkyviin.

1. Kopioi tämä avain ja tallenna se rekisteröitymisestä uusia laitteita tämän palvelun avulla.

1. Valitse Tarkista-kuvake ![Tarkistuksen kuvake](./media/storsimple-ova-manage-service/image7.png) Sulje valintaikkuna.


## <a name="next-steps"></a>Seuraavat vaiheet

- Lue, miten [pääset alkuun](storsimple-ova-deploy1-portal-prep.md) StorSimple virtual matriisi.
    
- Lisätietoja hallinnasta [StorSimple laitteen](storsimple-ova-web-ui-admin.md).

 
