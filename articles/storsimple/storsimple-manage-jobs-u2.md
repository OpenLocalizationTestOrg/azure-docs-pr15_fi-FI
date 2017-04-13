<properties 
   pageTitle="Tarkastella ja hallita töitä StorSimple | Microsoft Azure"
   description="Tässä artikkelissa kuvataan StorSimple hallinnan palvelun Projektit-sivulla ja miten sitä käytetään seuraamaan viimeisimmät, nykyisen ja ajoitetun varmuuskopioinnin työt."
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
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-view-and-manage-storsimple-jobs-update-2"></a>StorSimple hallinta-palvelun avulla voit tarkastella ja hallita töitä StorSimple (päivitys 2)

[AZURE.INCLUDE [storsimple-version-selector-manage-jobs](../../includes/storsimple-version-selector-manage-jobs.md)]

## <a name="overview"></a>Yleiskatsaus

**Työt** -sivu sisältää yhden keskitetyn portaalin tarkasteleminen ja hallinta työt, jotka on aloitettu laitteissa yhteydessä StorSimple hallintapalvelu. Voit tarkastella ajoitetun, käynnissä, valmis, peruutettu ja epäonnistui työt useilla eri laitteilla. Tulokset esitetään taulukkomuodossa. 

![Työt-sivu](./media/storsimple-manage-jobs-u2/jobs.png)

Löydät nopeasti projektit ovat kiinnostuneita suodattamalla kentät, kuten:

- **Tila** – työt voi käyttää, valmis, peruutettu, epäonnistui, peruuttaa tai valmiin virheitä.
- **Mistä ja mihin** – työt voidaan suodattaa päivämäärä ja aika-alueen mukaan.
- **Kirjoita** – työlaji voi olla varmuuskopion, manuaalisesti, Palauta, Kloonaa laitteen automaattisesti Luo paikallisesti kiinnitetty asema, muokata asemaa, päivittäminen ja paketti tai virtual laitteen käyttöönotto.

- **Laitteiden** – työt tehdään tiettyjä laitteessa palvelun yhteydessä.
Suodatettu projektit ovat sitten taulukkomuotoinen seuraavien määritteiden perusteella:

    - **Kirjoita** – varmuuskopion, manuaalisesti, Palauta, Kloonaa laitteen automaattisesti Luo paikallisesti kiinnitetty asema, muokata asemaa, päivittäminen ja paketti tai virtual laitteen käyttöönotto.

    - **Tila** – käynnissä, valmis, peruutettu, epäonnistui, peruuttaa tai valmiin virheitä.

    - **Kohteen** – työt voidaan liittää asema, varmuuskopion käytännön tai laitteeseen. Kloonaa työ on liitetty asema, esimerkiksi ajoitetun varmuuskopiointityön on liitetty varmuuskopion käytännön. Laitteen työn luodaan palauttaminen (DR) tai palautustyön.

    - **Laitteen** – laitteen nimi, jonka työ on käynnistetty.

    - **Valitse aloittaminen** – aika, kun työ on käynnistetty.

    - **Edistymisen** – käynnissä olevan projektin valmistumisen. Valmiin projektin tämä on aina oltava 100 %.

Töiden luettelo päivitetään 30 sekunnin välein.

Voit suorittaa seuraavat projektiin liittyvät toiminnot tällä sivulla:

- Työn tarkasteleminen

- Työn peruuttaminen

## <a name="view-job-details"></a>Työn tarkasteleminen

Seuraavien toimien voidaan tarkastella mitään projektin tietoja.

#### <a name="to-view-job-details"></a>Voit tarkastella projektin tietoja.

1. Valitse **Projektit** -sivulla Näytä ovat kiinnostuneita käynnistämällä kyselyn suodattimilla työt. Voit etsiä valmis, käytössä tai peruuttaa työt.

2. Valitse projektin.

3. Valitse **tiedot**-sivun alareunassa.

4. Voit tarkastella **Varmuuskopiointi projektin tiedot** -valintaikkunassa tila, tiedot, tilastot ja tietojen tilastot.
 
    ![Projektin tiedot-sivu](./media/storsimple-manage-jobs-u2/JobDetails.png)

## <a name="cancel-a-job"></a>Työn peruuttaminen

Seuraavien toimien peruuttamaan käynnissä olevan projektin.

>[AZURE.NOTE] Jotkin töitä, kuten muokkaaminen aseman aseman tyypin muuttaminen tai asema, laajentaminen ei voi peruuttaa.

### <a name="to-cancel-a-job"></a>Voit peruuttaa työn

1. Valitse **Projektit** -sivulla Näytä käynnissä olevat työt, jonka haluat peruuttaa käynnistämällä kyselyn suodattimilla.

1. Valitse projekti.

1. Valitse sivun alareunassa **Peruuta**.

1. Kun ohjelma pyytää vahvistusta, valitse **Kyllä**.

Tämä työ on nyt peruutettu.

## <a name="next-steps"></a>Seuraavat vaiheet

- Lisätietoja [StorSimple-varmuuskopioinnin käytäntöjen](storsimple-manage-backup-policies.md)hallinta.

- Lue ohjeet [ammattimainen StorSimple laitteen StorSimple hallinta-palvelun käyttöä](storsimple-manager-service-administration.md)varten.
