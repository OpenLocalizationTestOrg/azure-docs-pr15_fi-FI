<properties 
   pageTitle="StorSimple Virtual matriisin web Käyttöliittymän hallinta | Microsoft Azure"
   description="Tässä artikkelissa käsitellään laitteen hallintatehtävien kautta StorSimple Virtual matriisi-web-Käyttöliittymää."
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
   ms.date="04/07/2016"
   ms.author="alkohli" />

# <a name="use-the-web-ui-to-administer-your-storsimple-virtual-array"></a>Web-Käyttöliittymän avulla voit hallita StorSimple Virtual matriisin

![asennuksen prosessinkulku](./media/storsimple-ova-web-ui-admin/manage4.png)

## <a name="overview"></a>Yleiskatsaus

Tässä artikkelissa opetusohjelmat koskevat Microsoft Azure StorSimple Virtual matriisin (tunnetaan myös nimellä StorSimple paikallisen virtual laite) käynnissä maaliskuussa 2016 yleiseen käyttöön (GA)-versiossa. Tässä artikkelissa kuvataan monimutkaisia työnkulut ja hallintatehtäviä, joita voidaan suorittaa StorSimple Virtual matriisi. Voit hallita StorSimple hallintaa käyttämällä StorSimple Virtual matriisin service (jota kutsutaan portaalin Käyttöliittymän) Käyttöliittymä ja paikallisen sivuston Käyttöliittymä laitteen. Tässä artikkelissa keskitytään tehtävät, joita voi suorittaa web-Käyttöliittymää.

Tässä artikkelissa on opetusohjelmassa:

- Hae palvelun tiedot salausavaimen
- Web Käyttöliittymän asennuksen vianmääritys
- Luo lokitiedoston-paketti
- Sammuta tai Käynnistä laite uudelleen

## <a name="get-the-service-data-encryption-key"></a>Hae palvelun tiedot salausavaimen

Palvelun tiedot-salausavaimen luodaan, kun ensimmäisen laitteen rekisteröinnin StorSimple hallinta-palvelussa. Tämä avain on sitten pyytää muita laitteita rekisteröityä StorSimple hallintapalvelu palvelun rekisteröinti-näppäintä.

Jos olet hukannut palvelun tiedot salausavaimen ja haluat palauttaa, suorita seuraavat vaiheet paikallisen sivuston Käyttöliittymä laitteen rekisteröity palveluun.

#### <a name="to-get-the-service-data-encryption-key"></a>Palvelun tiedot salausavaimen hankkiminen

1. Muodosta yhteys paikalliseen web-Käyttöliittymä. Valitse **määritys** > **Cloud asetukset**.
  

2. Sivun alareunassa valitsemalla **Hae palvelun tiedot salausavaimen**. Avaimen tulee näkyviin. Kopioi ja Tallenna avain.
    
    ![Hae tiedot salausavaimen 1](./media/storsimple-ova-web-ui-admin/image27.png)
   


## <a name="troubleshoot-web-ui-setup-errors"></a>Web Käyttöliittymän asennuksen vianmääritys

Joissakin tapauksissa määrittäessäsi laitteesi paikallisen sivuston Käyttöliittymä, joita saattaa ilmetä virheitä. Ja tällaisia vianmääritys, voit suorittaa Diagnostiikka-testien.

#### <a name="to-run-the-diagnostic-tests"></a>Jos haluat suorittaa diagnostiikan testejä

1. Siirry paikallisen sivuston Käyttöliittymä **vianmääritys** > **Diagnostiikan testit**.

    ![Suorita diagnostiikka 1](./media/storsimple-ova-web-ui-admin/image29.png)

2. Valitse **Suorita Diagnostiikkatestit**sivun alareunaan. Tämä aloittaa testien vianmääritys mahdollisia ongelmia verkon, laite, web-välityspalvelimen, aika tai cloud asetukset. Saat ilmoituksen, että laitteesi käyttöjärjestelmänä on kokeet.

3. Kun testejä on valmis, tulokset näkyvät. Seuraavassa esimerkissä esitetään diagnostiikan tulokset. Huomaa, että WWW-välityspalvelimen ei ole määritetty tässä laitteessa ja vuoksi web välityspalvelimen testi ei toimi. Kaikki muut kokeet verkkoasetukset, DNS-palvelin ja aika-asetuksia ei onnistunut.

    ![Suorita diagnostiikka 2](./media/storsimple-ova-web-ui-admin/image30.png)

## <a name="generate-a-log-package"></a>Luo lokitiedoston-paketti

Log-paketin koostuu kaikki asianmukaiset lokit, joka auttaa Microsoft Support laitteen kaikki vianmääritykseen. Tässä versiossa log-paketin voidaan luoda paikallisen sivuston Käyttöliittymän välityksellä.

#### <a name="to-generate-the-log-package"></a>Log-paketin luomiseen

1. Siirry paikallisen sivuston Käyttöliittymä **vianmääritys** > **järjestelmän lokitiedot**.

    ![Luo lokitiedoston 1-paketti](./media/storsimple-ova-web-ui-admin/image31.png)

2. Valitse **Luo lokitiedoston paketin**sivun alareunaan. Järjestelmälokit ohjelmistopaketin luodaan. Tämä kestää muutaman minuutin.

    ![Luo lokitiedoston pakettia 2](./media/storsimple-ova-web-ui-admin/image32.png)

    Saat ilmoituksen, kun paketti on luotu ja sivu päivitetään ilmaisemaan, kun paketti on luotu päivämäärän ja kellonajan.

    ![Luo lokitiedoston paketin 3](./media/storsimple-ova-web-ui-admin/image33.png)

3. Valitse **Lataa log-paketti**. ZIP-paketin ladataan järjestelmään.

    ![Luo lokitiedoston paketin 4](./media/storsimple-ova-web-ui-admin/image34.png)

4. Voit ladatut log-paketin purkaminen ja tarkastella järjestelmän lokitiedostot.

## <a name="shut-down-and-restart-your-device"></a>Sulje ja Käynnistä laite uudelleen

Voit Sammuta tai käyttämällä paikallisen sivuston Käyttöliittymä virtual laitteen uudelleen. Microsoft suosittelee, ennen kuin käynnistät, ottaa asemat tai osakkeet offline-tilassa isännän ja laitteen. Tämä minimoi tietovirheitä mahdollisuutta. 

#### <a name="to-shut-down-your-virtual-device"></a>Virtuaalinen laitteen Sammuta

1. Siirry Käyttöliittymän paikallisen sivuston **ylläpito** > **Power asetukset**.

2. Valitse sivun alareunassa **Sammuta**.

    ![laitteen Sammuta 1](./media/storsimple-ova-web-ui-admin/image36.png)

3. Näyttöön tulee varoitus, jossa ilmoitetaan, että laitteen Sammuta keskeyttää minkä tahansa IO, joka on käynnissä, tuloksena käyttökatkot. Valitse Tarkista-kuvake ![Tarkista kuvake](./media/storsimple-ova-web-ui-admin/image3.png).

    ![laitteen Sammuta varoitus](./media/storsimple-ova-web-ui-admin/image37.png)

    Saat ilmoituksen Sammuta on aloitettu.

    ![laitteen Sammuta käytön aloittaminen](./media/storsimple-ova-web-ui-admin/image38.png)

    Laitteen suljetaan. Jos haluat aloittaa laitteessa, sinun on tehdä Hyper-V Manager kautta.

#### <a name="to-restart-your-virtual-device"></a>Aloita virtual laitteen uudelleen

1. Siirry Käyttöliittymän paikallisen sivuston **ylläpito** > **Power asetukset**.

2. Valitse sivun alareunassa **uudelleen**.

    ![Käynnistä laite uudelleen](./media/storsimple-ova-web-ui-admin/image36.png)

3. Näyttöön tulee varoitus, jossa ilmoitetaan, että laitteen käynnistäminen uudelleen keskeyttää minkä tahansa IOs, joka on käynnissä, tuloksena käyttökatkot. Valitse Tarkista-kuvake ![Tarkista kuvake](./media/storsimple-ova-web-ui-admin/image3.png).

    ![Käynnistä varoitus](./media/storsimple-ova-web-ui-admin/image37.png)

    Saat ilmoituksen, että uudelleenkäynnistyksen on aloitettu.

    ![aloitetaan uudelleen](./media/storsimple-ova-web-ui-admin/image39.png)

    Kun uudelleenkäynnistyksen on käynnissä, menetät yhteyden Käyttöliittymän. Voit valvoa uudelleenkäynnistyksen päivittämällä Käyttöliittymän säännöllisin väliajoin. Vaihtoehtoisesti voit valvoa Hyper-V Manager kautta laitteen uudelleen-tila.

## <a name="next-steps"></a>Seuraavat vaiheet

Lue ohjeet [voi hallita laitetta StorSimple hallinta-palvelun käyttöä](storsimple-manager-service-administration.md)varten.
