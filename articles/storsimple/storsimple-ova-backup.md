<properties 
   pageTitle="StorSimple Virtual matriisin varmuuskopion opetusohjelma | Microsoft Azure"
   description="Tässä artikkelissa käsitellään varmuuskopioida StorSimple Virtual matriisin osakkeet ja asemat."
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
   ms.date="06/07/2016"
   ms.author="alkohli" />

# <a name="back-up-your-storsimple-virtual-array"></a>StorSimple Virtual matriisin varmuuskopiointi

## <a name="overview"></a>Yleiskatsaus 

Tässä opetusohjelmassa koskee Microsoft Azure StorSimple Virtual matriisi (tunnetaan myös nimellä StorSimple paikallisen virtual laitteen tai StorSimple virtual laite) käynnissä maaliskuussa 2016 yleiseen käyttöön (GA)-versiossa tai sitä uudemmissa versioissa.

StorSimple Virtual matriisi on hybrid cloud paikallisen virtual tallennusväline, joka on määritetty tiedostopalvelimeen tai iSCSI-palvelimeen. Se varmuuskopioita, varmuuskopioista palauttaminen ja suorittaa laitteen automaattisesti tarvittaessa palauttaminen. Kun määritetty tiedostopalvelimessa, se sallii Kohdetason palauttaminen. Tässä opetusohjelmassa kuvataan ajoitetun ja manuaaliset varmuuskopiot StorSimple Virtual matriisin luominen käyttämällä Azure perinteinen portaalin tai StorSimple web-Käyttöliittymä.


## <a name="back-up-shares-and-volumes"></a>Osakkeet ja asemat varmuuskopiointi

Varmuuskopioiden ajankohta suojausta, parantaa palauttaminen ja minimoida palauttaminen ajat osakkeet ja asemat. Voit varmuuskopioida Jaa tai asema StorSimple laitteen kahdella tavalla: **ajoitettu** tai **Manuaalinen**. Seuraavissa osissa käsitellään eri tavat.

> [AZURE.NOTE] Tässä versiossa ajoitetun varmuuskopiot luodaan oletuskäytäntö, joka suoritetaan päivittäin määritettynä ajankohtana ja varmuuskopioi osakkeet tai asemat laitteeseen. Ei voida luoda mukautettuja käytäntöjä ajoitettujen varmuuskopioiden tällä hetkellä.

## <a name="change-the-backup-schedule"></a>Varmuuskopion aikataulun muuttaminen

StorSimple virtual laitteessasi on oletusarvoisesti varmuuskopio käytännön, joka alkaa päivän (22:30) määritetyllä aikavälillä ja varmuuskopioi osakkeet tai asemat laitteeseen kerran päivässä. Voit muuttaa aika, jolla ei voi muuttaa varmuuskopion käynnistyy, mutta korkojakso ja säilytyskäytäntöjä (joka määrittää säilytettävien varmuuskopioiden määrän). Nämä varmuuskopioinnin aikana koko virtual laite varmuuskopioidaan; Tästä syystä suosittelemme ajoittaa nämä varmuuskopiot myöhemmin.

Seuraavien toimien [Azure perinteinen portaalissa](https://manage.windowsazure.com/) voit muuttaa oletusarvon varmuuskopion aloitusaika.

#### <a name="to-change-the-start-time-for-the-default-backup-policy"></a>Jos haluat muuttaa alkamisajan määrittäminen varmuuskopion oletuskäytäntö

1. Siirry laitteen **määritys** -välilehti.

2. Määritä **varmuuskopioinnin** -osassa aloitusaika varmuuskopioinnin.

3. Valitse **Tallenna**.

### <a name="take-a-manual-backup"></a>Ota manuaalinen varmuuskopiointi

Lisäksi ajoitetun varmuuskopioinnin voi tehdä manuaalisen (tarvittaessa) varmuuskopion milloin tahansa.

#### <a name="to-create-a-manual-on-demand-backup"></a>Luo varmuuskopio Manuaalinen (tarvittaessa)

1. Siirry **osakkeet** - tai **asemat** -välilehti.

2. Valitse **Varmuuskopiointi kaikki**sivun alareunaan. Voit pyydetään tarkistamaan, että haluat tehdä varmuuskopion nyt. Valitse tarkistuksen kuvake ![kuvakkeesta](./media/storsimple-ova-backup/image3.png) varmuuskopioinnin Jatka.

    ![varmuuskopion vahvistus](./media/storsimple-ova-backup/image4.png)

    Saat ilmoituksen, että varmuuskopiointityön käynnistyy.

    ![aloitetaan varmuuskopiointi](./media/storsimple-ova-backup/image5.png)

    Saat ilmoituksen, että projekti on luotu.

    ![luotu varmuuskopiointi](./media/storsimple-ova-backup/image7.png)

3. Projektin edistymisen seurantaan, valitse **Näytä työ**.

4. Kun varmuuskopiointityön on valmis, Avaa **varmuuskopioinnin luettelo** -välilehti. Raportissa pitäisi näkyä valmiit varmuuskopion.

    ![Valmiit varmuuskopiointi](./media/storsimple-ova-backup/image8.png)

5. Määritä suodatinvalinnat sopiva laite, varmuuskopion käytännön ja aikavälin ja valitse sitten valintaruutu-kuvake ![Tarkista kuvake](./media/storsimple-ova-backup/image3.png).

    Varmuuskopion pitäisi näkyä luettelossa varmuuskopion siirtämistä, joka näkyy luettelossa.

## <a name="view-existing-backups"></a>Tarkastele varmuuskopioon

Seuraavien toimien Azure perinteinen portaalissa voit tarkastella varmuuskopioon.

#### <a name="to-view-existing-backups"></a>Jos haluat tarkastella varmuuskopioon

1. Valitse StorSimple hallinta-palvelu‑sivulla **varmuuskopioinnin luettelo** -välilehti.

2. Valitse varmuuskopioinnin määrittäminen seuraavasti:

    1. Valitse laite.

    2. Valitse Jaa tai voimakkuutta varmuuskopion, josta haluat valita avattavasta luettelosta.

    3. Määritä aikaväli.

    4. Valitse tarkistuksen kuvake ![](./media/storsimple-ova-backup/image3.png) tämän kyselyn.

    Varmuuskopioista liittyvät valitun Jaa tai asema pitäisi näkyä varmuuskopion joukot luettelo.

![video_icon](./media/storsimple-ova-backup/video_icon.png) **Video käytettävissä**

Katso video ja katso, miten voit luoda osakkeet, osakkeet varmuuskopiointi ja palauttaminen StorSimple Virtual taulukon tiedot.

> [AZURE.VIDEO use-the-storsimple-virtual-array]

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja [hallinnasta StorSimple Virtual matriisin](storsimple-ova-web-ui-admin.md).
