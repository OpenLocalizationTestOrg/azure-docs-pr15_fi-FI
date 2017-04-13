<properties 
   pageTitle="StorSimple virtual laitteen järjestelmänvalvojan salasanan vaihtaminen | Microsoft Azure"
   description="Tässä artikkelissa käsitellään suorittaa Azure perinteinen portaalin ja StorSimple Virtual matriisi-web-Käyttöliittymää laitteen järjestelmänvalvojan salasanan vaihtaminen."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="06/17/2016"
   ms.author="alkohli" />

# <a name="change-the-storsimple-virtual-array-device-administrator-password"></a>StorSimple Virtual matriisin laitteen järjestelmänvalvojan salasanan vaihtaminen

## <a name="overview"></a>Yleiskatsaus

Kun Windows PowerShell-liittymän avulla voit käyttää StorSimple virtual laitetta, tarvitaan laitteen järjestelmänvalvoja-salasana. Kun StorSimple-laite on ensin valmisteltu ja käytön aloittaminen-oletusarvon salasana on *Salasana1*. Tietojen suojauksen oletusarvo-vanhenee, kirjaudu sisään ja muuta salasana tarvitaan ensimmäisen kerran.

Voit käyttää myös paikallisen sivuston Käyttöliittymä tai Azure perinteinen portaalin milloin tahansa, kun laite on otettu käyttöön tuotantoympäristössä laitteen järjestelmänvalvojan salasanan vaihtaminen. Tässä artikkelissa on kuvattu kaikki seuraavat toimenpiteet.

## <a name="use-the-azure-classic-portal-to-change-the-password"></a>Azure perinteinen-portaalin käyttäminen salasanan vaihtaminen

Seuraavien toimien laitteen järjestelmänvalvojan salasanan palvelun Azure perinteinen-portaalissa.

#### <a name="to-change-the-device-administrator-password-via-the-azure-classic-portal"></a>Azure perinteinen portaalin kautta laitteen järjestelmänvalvojan salasanan vaihtaminen

1. Valitse **laitteet**-portaalissa > **määrittäminen** laitteeseesi.

2. Vieritä **Laitteen järjestelmänvalvojan salasana** -osaan. Anna järjestelmänvalvojan salasana, joka sisältää 8 15 merkkiä. Salasanassa on oltava isoilla, pieni, numeerinen ja määräten merkkien yhdistelmiä.

3. Vahvista salasana.

4. Valitse sivun alareunassa **Tallenna** .

Laitteen järjestelmänvalvojan salasana nyt voi päivittää. Voit käyttää laitteeseen paikallisesti muokattu salasana.

## <a name="use-the-storsimple-virtual-array-web-ui-to-change-the-password"></a>Käytä StorSimple Virtual matriisi-web-Käyttöliittymää salasanan vaihtaminen

Seuraavien toimien laitteen järjestelmänvalvojan salasanan paikallisen sivuston Käyttöliittymän välityksellä.

#### <a name="to-change-the-device-administrator-password-via-the-local-web-ui"></a>Paikallisen sivuston Käyttöliittymä kautta laitteen järjestelmänvalvojan salasanan vaihtaminen

1. Valitse paikallisen sivuston Käyttöliittymä **ylläpito** > laitteen**salasanan muuttaminen** .

    ![Muuta Salasana1](./media/storsimple-ova-change-device-admin-password/image40.png)

2. Kirjoita **nykyinen salasana**.

3. Anna **Uusi salasana**. Salasana on oltava vähintään 8 merkkiä. Tiedostossa on oltava 3 / 4 seuraavasti: merkkejä isoja, pieni, numeeriset ja erikoismerkkejä.

    Huomaa, että salasana ei voi olla sama kuin viimeisen 24 salasanat.

3. Kirjoita salasana kirjoittamalla se uudelleen.

    ![Muuta Salasana2](./media/storsimple-ova-change-device-admin-password/image41.png)

4. Sivun alareunassa valitsemalla **Käytä**. Uusi salasana sitten otetaan käyttöön. Jos salasanan vaihtaminen ei onnistu, näet seuraavan virheen.

    ![Salasanavirhe](./media/storsimple-ova-change-device-admin-password/image42.png)

    Kun salasana on päivitetty, saat tiedon. Voit käyttää tätä muokattu salasanaa käyttämään laitteeseen paikallisesti.

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja [hallinnasta StorSimple Virtual matriisin](storsimple-ova-web-ui-admin.md).
