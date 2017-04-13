<properties 
   pageTitle="Access-ohjausobjektin tietueiden hallintaa StorSimple Virtual matriisin | Microsoft Azure"
   description="Tässä artikkelissa käsitellään hallinta Accessin ohjausobjektin tietueiden (ACRs) voit selvittää, mitkä isännät muodostaa StorSimple Virtual matriisin osioon."
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
   ms.date="05/03/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-manage-access-control-records-for-the-storsimple-virtual-array"></a>Access-ohjausobjektin tietueiden hallintaa StorSimple Virtual matriisin StorSimple hallinta-palvelun avulla 

## <a name="overview"></a>Yleiskatsaus

Accessin ohjausobjektin tietueiden (ACRs) avulla voit määrittää, mitkä isännät muodostaa StorSimple Virtual matriisin (tunnetaan myös nimellä StorSimple paikallisen virtual laite)-osioon. ACRs määritetään määritetyn aseman ja sisältää iSCSI koko nimet (IQNs) isännät. Kun isäntä yrittää aseman yhdistäminen, laite tarkistaa IQN nimen asemaan liittyvän ACR, ja jos vastine löytyy, valitse yhteys on muodostettu. **Määritä** sivun **Accessin ohjausobjektin tietueiden** -osa näyttää access ohjausobjektin tietueet vastaavan IQNs isäntien kanssa.

Tässä opetusohjelmassa kerrotaan seuraavat yhteiset ACR liittyviä tehtäviä:

- Hae IQN
- Access-ohjausobjektin tietueen lisääminen 
- Access-ohjausobjektin tietueen muokkaaminen 
- Access-ohjausobjektin tietueen poistaminen 

> [AZURE.IMPORTANT] 
> 
> - Kun määrität ACR levylle, huolehtia, että äänenvoimakkuuden ei samanaikaisesti Käytä useita ei ole liitetty isäntä, koska tämä saattaa vioittunut äänenvoimakkuutta. 
> - Poistettaessa ACR asemasta Varmista, että vastaavan host ei käytä äänenvoimakkuuden koska poistaminen saattaa johtaa luku-ja kirjoitusoikeudet häiriöt.

## <a name="get-the-iqn"></a>Hae IQN

Seuraavien toimien saat Windows-isäntä, jossa on käytössä Windows Server 2012 IQN.

[AZURE.INCLUDE [storsimple-get-iqn](../../includes/storsimple-get-iqn.md)]

## <a name="add-an-acr"></a>Lisää ACR

StorSimple hallinta palvelun **määritys** -sivulla voit lisätä ACRs. Yksi ACR yleensä liittämällä yksi osa.

Lisätietoja ACR liittäminen aseman Siirry [Lisää aseman](storsimple-ova-deploy3-iscsi-setup.md#step-3-add-a-volume).

>[AZURE.IMPORTANT] 
> 
>Kun määrität ACR levylle, huolehtia, että äänenvoimakkuuden ei samanaikaisesti Käytä useita ei ole liitetty isäntä, koska tämä saattaa vioittunut äänenvoimakkuutta.
 
Seuraavien toimien Lisää ACR.

#### <a name="to-add-an-acr"></a>Jos haluat lisätä ACR

1. Valitse service-aloitussivu palvelun, kaksoisnapsauta palvelunimi ja valitse **määritys** -välilehti.

    ![määritys-välilehti](./media/storsimple-ova-manage-acrs/acr1.png)

2. Anna **nimi** **Accessin ohjausobjektin tietueiden**taulukkomuotoinen pääkohde-että ACR varten.

3. Anna Windows-isännän IQN nimen kohdassa **iSCSI käynnistäjä nimi**. 

4. Valitse **Tallenna** sivun alalaidassa Tallenna juuri luomasi ACR. Näyttöön tulee seuraava vahvistussanoma.

    ![vahvistussanoman](./media/storsimple-ova-manage-acrs/acr2.png)

5. Valitse Tarkista-kuvake ![Tarkista kuvake](./media/storsimple-ova-manage-acrs/check-icon.png). Taulukkomuotoinen luettelo päivitetään vastaamaan tämä lisäys.

## <a name="edit-an-acr"></a>Muokkaa ACR

Käytät Muokkaa ACRs **määritys** -sivulla Azure perinteinen-portaalissa. 

> [AZURE.NOTE] Muokkaa vain ACRs, jotka eivät ole käytössä. Jos haluat muokata ACR liittyvät asema, joka on käytössä, voit ensin otettava äänenvoimakkuuden offline-tilassa.

Seuraavien toimien voit muokata ACR.

#### <a name="to-edit-an-acr"></a>Jos haluat muokata ACR

1. Valitse service-aloitussivu palvelun, kaksoisnapsauta palvelunimi ja valitse **määritys** -välilehti.

2. Access-ohjausobjektin tietueiden taulukkomuotoinen luettelo-hiiren osoitinta ACR, jota haluat muokata.

3. Anna uusi nimi ja/tai IQN ACR varten.

4. Valitse **Tallenna** sivun alalaidassa Tallenna muokattu ACR. Näyttöön tulee vahvistussanoma. 

5. Valitse Tarkista-kuvake ![Tarkista kuvake](./media/storsimple-ova-manage-acrs/check-icon.png). Taulukkomuotoinen luettelo päivitetään vastaamaan tämän muutoksen.

## <a name="delete-an-access-control-record"></a>Access-ohjausobjektin tietueen poistaminen

**Määritys** -sivulla Käytä Azure perinteinen portaalissa ACRs poistamiseen. 

> [AZURE.NOTE] 
> 
> - Sinun tulee poistaa vain ACRs, jotka eivät ole käytössä. Jos haluat poistaa ACR liittyvät asema, joka on käytössä, ensin otettava äänenvoimakkuuden offline-tilassa.
> - Poistettaessa ACR asemasta Varmista, että vastaavan host ei käytä äänenvoimakkuuden koska poistaminen saattaa johtaa luku-ja kirjoitusoikeudet häiriöt.

Seuraavien toimien poistaminen access-ohjausobjektin tietueen.

#### <a name="to-delete-an-access-control-record"></a>Jos haluat poistaa access-ohjausobjektin tietueen

1. Valitse service-aloitussivu palvelusi, kaksoisnapsauta palvelunimi ja valitse **määritys** -välilehti.

2. Access-ohjausobjektin tietueiden (ACRs) taulukkomuotoinen luettelo-päälle, jonka haluat poistaa ACR.

3. Poista-kuvaketta (**x**) tulevat näkyviin, kun valitset ACR erittäin oikeanpuoleisessa sarakkeessa. Valitse poistettava ACR **x** -kuvaketta. Näyttöön tulee seuraava vahvistussanoma.

    ![vahvistussanoman](./media/storsimple-ova-manage-acrs/acr3.png)

5. Valitse Tarkista-kuvake ![Tarkista kuvake](./media/storsimple-ova-manage-acrs/check-icon.png). Taulukkomuotoinen luettelo päivitetään vastaamaan poisto.

## <a name="next-steps"></a>Seuraavat vaiheet

- Lisätietoja [tietomääristä lisäämällä ja määrittämällä ACRs](storsimple-ova-deploy3-iscsi-setup.md#step-3-add-a-volume).
