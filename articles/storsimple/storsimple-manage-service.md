<properties 
   pageTitle="Ottaa käyttöön StorSimple hallintapalveluun | Microsoft Azure"
   description="Kerrotaan, miten voit luoda ja poistaa Azure perinteinen portaalissa StorSimple hallintapalvelu ja kerrotaan, miten voit hallita palvelua rekisteröinti-näppäintä."
   services="storsimple"
   documentationCenter=""
   authors="SharS"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="05/24/2016"
   ms.author="v-sharos" />

# <a name="deploy-the-storsimple-manager-service"></a>Ottaa käyttöön StorSimple hallintapalvelu

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

Tässä opetusohjelmassa kerrotaan, miten näiden tehtävien suorittamiseen.

## <a name="create-a-service"></a>Luo palvelu

**Nopea luominen** -vaihtoehdon avulla voit luoda StorSimple hallintapalveluun, jos haluat ottaa käyttöön StorSimple laitteen. Jos haluat luoda palvelu, on:

- Enterprise Agreement-sopimus-tilaus
- Tilin aktiivinen Microsoft Azure-tallennustilan
- Laskutustiedot, jota käytetään käyttöoikeushallinta

Voit myös luoda tallennustilan oletustilin luodessasi palvelun.

Yksittäistä voit hallita useita laitteita. Laite ei voi kuitenkin olla palveluihin. Suuri yritys voi olla useita palveluesiintymiä eri tilaukset, organisaatiot tai jopa käyttöönoton sijainnit-käyttöä varten. Huomaa, että erillistä StorSimple hallintapalvelu StorSimple 8000 sarjan laitteet ja StorSimple Virtual matriisin esiintymät.

Seuraavien toimien palvelun luomiseen.

[AZURE.INCLUDE [storsimple-create-new-service](../../includes/storsimple-create-new-service.md)]

## <a name="delete-a-service"></a>Palvelun poistaminen

Ennen kuin poistat palvelu, varmista, että ei ole yhdistetty laitteiden käytön yhteydessä. Jos palvelu on käytössä, poistaa yhdistettyjen laitteiden. Aktivointi-toiminnon palvelimen laite ja palvelun välinen yhteys, mutta säilyttää pilveen laitteen tiedot. 

[AZURE.IMPORTANT] Kun palvelu on poistettu, toimintoa ei voi kumota. Kaikissa laitteissa, joissa on käyttämällä palvelua on oltava factory Palauta, ennen kuin sitä voidaan käyttää toisen palvelun kanssa. Tässä skenaariossa paikallisen laitteen sekä määrityskohde-tiedot, menetetään.

Seuraavien toimien Poista palvelu.

### <a name="to-delete-a-service"></a>Jos haluat poistaa palvelun

1. Valitse **StorSimple hallinta** -sivulla palvelun, josta haluat poistaa.

1. Valitse sivun alareunassa **Poista** .

1. Valitse **Kyllä** , jos vahvistus-ilmoituksen. Voi kestää muutaman minuutin kuluttua-palvelun poistamista.

## <a name="get-the-service-registration-key"></a>Palvelun rekisteröinti Key-tunnuksen hankkiminen

Kun olet luonut palvelu, sinun on rekisteröidä StorSimple laitteen-palveluun. Rekisteröi ensimmäisen StorSimple laitteen, tarvitset palvelun rekisteröinti-näppäintä. Voit rekisteröidä muita laitteita StorSimple-palvelussa, tarvitset rekisteröinti-näppäintä ja palvelun tiedot salausavaimen (joka on luotu ensimmäisen laitteen rekisteröinnin yhteydessä). Saat lisätietoja palvelun tiedot salausavaimen [StorSimple suojaus](storsimple-security.md). Saat muodostamalla yhteyden **Rekisteröinti avain** **palvelut** -sivulla rekisteröinti-näppäintä.

Seuraavien toimien saat palvelun rekisteröinti-näppäintä.

[AZURE.INCLUDE [storsimple-get-service-registration-key](../../includes/storsimple-get-service-registration-key.md)]

Ota palvelun rekisteröinti avainta turvalliseen paikkaan. Tarvitset tätä näppäintä sekä palvelun tiedot salausavaimen, voit rekisteröidä muita laitteita tämän palvelun avulla. Tarvitset saatuaan palvelun rekisteröinti avainta määrittäminen Windows PowerShellin kautta laitteen StorSimple liittymän.

Lisätietoja rekisteröinti avaimeen käyttämisestä on artikkelissa [Vaihe 3: Määritä ja rekisteröi laite Windows PowerShellin kautta StorSimple](storsimple-deployment-walkthrough.md#step-2-configure-and-register-the-device-through-windows-powershell-for-storsimple).

## <a name="regenerate-the-service-registration-key"></a>Palvelun rekisteröinti avain uudelleen

Tarvitset palvelun rekisteröinti näppäintä uudelleen, jos sinun on suoritettava avaimen kierron tai palvelun järjestelmänvalvojat luettelo on muuttunut. Kun avain uudelleen, uusi avain käytetään vain seuraavien laitteiden rekisteröitymisestä. Laitteet, jotka on jo rekisteröity ovat ei vaikuta tällä menetelmällä.

Seuraavien toimien palvelun rekisteröinti näppäintä uudelleen.

### <a name="to-regenerate-the-service-registration-key"></a>Palvelun rekisteröinti avain uudelleen

1. Valitse **StorSimple hallinta** -sivulla **Rekisteröinti-näppäintä**.

1. Valitse **Palvelun rekisteröinti avainta** -valintaikkunan **uudelleen**.

1. Näyttöön tulee vahvistussanoma. Valitse **OK** uudistaminen Jatka.

1. Uusi palvelu rekisteröinti avain tulee näkyviin.

1. Kopioi tämä avain ja tallenna se rekisteröitymisestä uusia laitteita tämän palvelun avulla.

1. Valitse Tarkista-kuvake ![Tarkistuksen kuvake](./media/storsimple-manage-service/HCS_CheckIcon.png) Sulje valintaikkuna.


## <a name="next-steps"></a>Seuraavat vaiheet

- Lisätietoja [StorSimple käyttöönoton käsitellä](storsimple-deployment-walkthrough.md).

- Lisätietoja [StorSimple tallennustilan tilin hallinta](storsimple-manage-storage-accounts.md).

- Lue lisää siitä, miten käytön [hallinnasta StorSimple laitteen StorSimple hallintapalvelu](storsimple-manager-service-administration.md).

 
