<properties 
   pageTitle="StorSimple salasanojen | Microsoft Azure" 
   description="Tässä artikkelissa käsitellään StorSimple tilannevedoksen hallinta ja laitteen järjestelmänvalvoja salasanojen StorSimple hallinta-palvelun avulla." 
   services="storsimple" 
   documentationCenter="NA" 
   authors="alkohli" 
   manager="carmonm" 
   editor=""/>

<tags
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD" 
   ms.date="08/17/2016"
   ms.author="alkohli"/>

# <a name="use-the-storsimple-manager-service-to-change-your-storsimple-passwords"></a>StorSimple salasanojen StorSimple hallinta-palvelun avulla

## <a name="overview"></a>Yleiskatsaus 

Azure perinteinen portaalin **Määritä** sivun sisältää kaikki laitteen parametrit, voit määrittää uudelleen StorSimple laitteessa, joka hallitsee StorSimple hallintapalvelu. Tässä opetusohjelmassa kerrotaan, kuinka voit tehdä **Määritä** sivun laitteen järjestelmänvalvoja tai StorSimple tilannevedoksen hallinta salasanan muuttaminen.

## <a name="change-the-device-administrator-password"></a>Laitteen järjestelmänvalvojan salasanan vaihtaminen

Kun Windows PowerShell-liittymän avulla voit käyttää StorSimple laitetta, tarvitaan laitteen järjestelmänvalvoja-salasana. Kun ensimmäinen StorSimple laite on rekisteröity palvelu-liittymän oletusarvon salasana on *Salasana1*. Tietojen suojaukseen tarvitaan tämän salasanan rekisteröintiprosessi loppuun. Voi lopettaa rekisteröinnin loppuun muuttamatta salasana. Lisätietoja on artikkelissa [Vaihe 3: Määritä ja rekisteröi laite Windows PowerShellin kautta StorSimple](storsimple-deployment-walkthrough-u2.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple).

Salasanaa, jolla on ensin määritettävä Windows PowerShell-liittymän kautta rekisteröinnin yhteydessä voi muuttaa Azure perinteinen portaalin kautta. Seuraavien toimien laitteen järjestelmänvalvojan salasanan vaihtaminen.

#### <a name="to-change-the-device-administrator-password"></a>Laitteen järjestelmänvalvojan salasanan vaihtaminen

1. Valitse perinteinen-portaalissa **laitteet** > **määrittäminen** laitteeseesi.

2. Vieritä **Laitteen järjestelmänvalvojan salasana** -osaan. Anna järjestelmänvalvojan salasana, joka sisältää 8 15 merkkiä. Salasanassa on oltava yhdistelmän 3 tai useita merkkejä isoja, pieni, numeeriset ja erikoismerkkejä.

3. Vahvista salasana.

4. Valitse sivun alareunassa **Tallenna** .

Laitteen järjestelmänvalvojan salasana nyt voi päivittää. Voit käyttää Windows PowerShell-liittymän muokattu salasana.

## <a name="change-the-storsimple-snapshot-manager-password"></a>StorSimple tilannevedoksen hallinta salasanan vaihtaminen

StorSimple tilannevedoksen hallinta ohjelmiston sijaitsee Windows-isännän ja avulla järjestelmänvalvojat voivat hallita varmuuskopioiden StorSimple laitteen paikallinen lomakkeessa ja cloud tilannevedoksia.

Kun määrität laitteen StorSimple tilannevedoksen hallinta, voit pyydetään laitteen IP-osoite ja salasana tallennustilan laitteen tarkistamiseen. Salasana on määritetty ensin Windows PowerShell-liittymän kautta. Lisätietoja on artikkelissa [Vaihe 3: Määritä ja rekisteröi laite Windows PowerShellin kautta StorSimple](storsimple-deployment-walkthrough-u2.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple).

Salasanaa, jolla on ensin määritettävä Windows PowerShell-liittymän kautta rekisteröinnin yhteydessä voi muuttaa perinteinen portaalin kautta. Seuraavien toimien StorSimple tilannevedoksen hallinta salasanan vaihtaminen.

#### <a name="to-change-the-storsimple-snapshot-manager-password"></a>StorSimple tilannevedoksen hallinta salasanan vaihtaminen

1. Valitse perinteinen-portaalissa **laitteet** > **määrittäminen** laitteeseesi.

2. Vieritä **StorSimple tilannevedoksen hallinta** -osaan. Kirjoita salasana, jossa on 14 tai 15 merkkiä. Varmista, että salasana on yhdistelmän 3 tai useita merkkejä isoja, pieni, numeeriset ja erikoismerkkejä.

3. Vahvista salasana.

4. Valitse sivun alareunassa **Tallenna** .

StorSimple tilannevedoksen hallinta salasana nyt voi päivittää.
 

## <a name="next-steps"></a>Seuraavat vaiheet

- Lisätietoja [StorSimple suojaus](storsimple-security.md).

- Lisätietoja [laitteen asetusten muokkaaminen](storsimple-modify-device-config.md).

- Lisätietoja [ammattimainen StorSimple laitteen StorSimple hallinta-palvelun avulla](storsimple-manager-service-administration.md).
