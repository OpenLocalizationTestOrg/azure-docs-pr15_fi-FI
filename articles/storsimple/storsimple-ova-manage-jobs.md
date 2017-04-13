<properties 
   pageTitle="Tarkastella ja hallita töitä StorSimple Virtual matriisi | Microsoft Azure"
   description="Tässä artikkelissa kuvataan StorSimple hallinnan palvelun Projektit-sivulla ja miten sitä käytetään seuraamaan uusimpia ja nykyisen työt StorSimple Virtual matriisin."
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
   ms.workload="na"
   ms.date="06/07/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-view-jobs-for-the-storsimple-virtual-array"></a>Näytä työt StorSimple Virtual matriisin StorSimple hallinta-palvelun avulla

## <a name="overview"></a>Yleiskatsaus

**Työt** -sivu sisältää yhden keskitetyn portaalin tarkasteleminen ja hallinta työt, jotka on aloitettu Virtual matriisien (tunnetaan myös nimellä paikallisen virtual laitteet), jotka ovat yhteydessä StorSimple hallintapalvelu. Voit tarkastella käynnissä, toteutuneita ja epäonnistui työt virtual useilla eri laitteilla. Tulokset esitetään taulukkomuodossa. 

![Työt-sivu](./media/storsimple-ova-manage-jobs/ovajobs1.png)

Löydät nopeasti projektit ovat kiinnostuneita suodattamalla kentät, kuten:

- **Tila** – voit etsiä kaikki käynnissä olevat, valmis tai epäonnistui työt.
- **Mistä ja mihin** – työt voidaan suodattaa päivämäärä ja aika-alueen mukaan.
- **Kirjoita** – työlaji voidaan kaikkien, varmuuskopion, Palauta, automaattisesti, lataa päivitykset tai asenna päivitykset.
- **Laitteiden** – työt tehdään tietyn laitteella palvelun yhteydessä. Suodatettu projektit ovat sitten taulukkomuotoinen seuraavien määritteiden perusteella:

    - **Kirjoita** – työlaji voidaan kaikkien, varmuuskopion, Palauta, automaattisesti, lataa päivitykset tai asenna päivitykset.

    - **Tila** – työt voi olla kaikki käynnissä, valmis, tai epäonnistui.

    - **Kohteen** – työt voidaan liittää asemaan, Jaa tai laite. 

    - **Laitteen** – laitteen nimi, jonka työ on käynnistetty.

    - **Valitse aloittaminen** – aika, kun työ on käynnistetty.

    - **Edistymisen** – käynnissä olevan projektin valmistumisen. Valmiin projektin tämä on aina oltava 100 %.

Töiden luettelo päivitetään 30 sekunnin välein.

## <a name="view-job-details"></a>Työn tarkasteleminen

Seuraavien toimien voidaan tarkastella mitään projektin tietoja.

#### <a name="to-view-job-details"></a>Voit tarkastella projektin tietoja.

1. Valitse **Projektit** -sivulla Näytä ovat kiinnostuneita käynnistämällä kyselyn suodattimilla työt. Voit etsiä valmis tai käynnissä olevat työt.

2. Valitse projektin työt taulukkomuotoinen luettelo.

3. Valitse **tiedot**-sivun alareunassa.

4. Valitse **tiedot** -valintaikkunassa voit tarkastella tila, tiedot ja tilastot. Seuraavassa kuvassa on esimerkki **Varmuuskopiointi projektin tiedot** -valintaikkunassa.
 
    ![Projektin tiedot-sivu](./media/storsimple-ova-manage-jobs/ovajobs2.png)

#### <a name="job-failures-when-the-virtual-machine-is-paused-in-the-hypervisor"></a>Kun virtuaalikoneen on keskeytetty hypervisor työn virheet

Kun työ on käynnissä StorSimple Virtual matriisin ja laitteen (virtual machine valmisteltu hypervisor) on keskeytetty yli 15 minuutin, työ epäonnistuu. Tämän vuoksi StorSimple Virtual matriisin ajankäytön parhaillaan synkronoitu Microsoft Azure-aika. Seuraavassa näyttökuvassa näkyy esimerkki palauttaminen työn virheisiin.

![Palauttaa työn virhe](./media/storsimple-ova-manage-jobs/restorejobfailure.png)

Nämä virheet koskevat varmuuskopiointi, palauttaminen tai Päivitä automaattisesti työt. Jos virtuaalikoneen on valmisteltu Hyper-V-koneen synkronoida aika myöhemmin oman hypervisor. Kun näin tapahtuu, voit käynnistää työtäsi. 

## <a name="next-steps"></a>Seuraavat vaiheet

[Opettele käyttämään paikallisen sivuston Käyttöliittymä ammattimainen StorSimple Virtual matriisin](storsimple-ova-web-ui-admin.md).
