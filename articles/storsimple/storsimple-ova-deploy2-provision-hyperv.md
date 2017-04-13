<properties
   pageTitle="Ottaa käyttöön StorSimple Virtual matriisi - säännöstä Hyper-V"
   description="Tässä toisessa opetusohjelmassa StorSimple Virtual matriisin käyttöönoton liittyy valmistelu Hyper-V virtual laitteeseen."
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
   ms.workload="NA"
   ms.date="10/11/2016"
   ms.author="alkohli"/>

# <a name="deploy-storsimple-virtual-array---provision-a-virtual-array-in-hyper-v"></a>Ottaa käyttöön StorSimple Virtual matriisi - valmistella Virtual matriisin Hyper-v

![](./media/storsimple-ova-deploy2-provision-hyperv/hyperv4.png)

## <a name="overview"></a>Yleiskatsaus

Valmistelun Tässä opetusohjelmassa koskee Microsoft Azure StorSimple Virtual matriisien (tunnetaan myös nimellä StorSimple paikallisen virtual tai StorSimple virtual laitteet) käynnissä maaliskuussa 2016 yleiseen käyttöön (GA)-versiossa. Tässä opetusohjelmassa kerrotaan, miten valmistelu StorSimple Virtual matriisin Hyper-V käytössä Windows Server 2012 R2, Windows Server 2012: ssa tai Windows Server 2008 R2 host järjestelmään. Tämä artikkeli koskee Azure perinteinen portal sekä Microsoft Azure Government Cloud StorSimple Virtual matriisien käyttöönotto.

Voit pitää olla järjestelmänvalvojan oikeudet valmistelu ja virtual laitteen määrittäminen. Valmistelu ja ensimmäinen määrittäminen voi kestää noin 10 minuutin suorittamiseen.


## <a name="provisioning-prerequisites"></a>Valmistelu edellytykset

Tätä löydät edellytykset valmistelu virtual host järjestelmän Hyper-V käytössä Windows Server 2012 R2, Windows Server 2012: ssa tai Windows Server 2008 R2-laitteella.

### <a name="for-the-storsimple-manager-service"></a>StorSimple hallinta-palveluun

Ennen kuin aloitat, varmista, että:

-   Olet tehnyt kaikki [valmisteleminen portaalin StorSimple Virtual matriisin](storsimple-ova-deploy1-portal-prep.md)vaiheet.

-   Olet ladannut virtual laitteen kuva, Hyper-V Azure-portaalista. Lisätietoja on artikkelissa [Vaihe 3: lataa virtual laitteen kuva](storsimple-ova-deploy1-portal-prep.md#step-3-download-the-virtual-device-image).

    > [AZURE.IMPORTANT] StorSimple Virtual matriisin käytössä ohjelmiston voi käyttää vain Storsimple hallinta-palvelun kanssa.

### <a name="for-the-storsimple-virtual-device"></a>StorSimple virtual laitteen

Ennen kuin otat virtual laitteen, varmista, että:

-   Sinulla on käynnissä Hyper-V Windows Server 2008 R2 tai uudempi versio, joka voi olla host järjestelmän varaus käytettävä laite.

-   Host ()-isäntäjärjestelmä pystyy tälle valmistelu virtual laite on seuraavissa resursseissa:

    -   Vähintään 4 sydämiä.

    -   Vähintään 8 Gigatavun RAM-muistia.

    -   Yhden verkoston käyttöliittymä.

    -   500 gt virtual levyn Järjestelmätiedot.

### <a name="for-the-network-in-the-datacenter"></a>Verkon joten

Ennen kuin aloitat, tarkista verkko edellytykset StorSimple virtual laitteen käyttöönotto ja määrittäminen palvelinkeskuksen verkon asianmukaisesti. Lisätietoja on artikkelissa [StorSimple Virtual matriisin verkko vaatimuksia](storsimple-ova-system-requirements.md#networking-requirements).

## <a name="step-by-step-provisioning"></a>Vaiheittaiset ohjeet valmistelu

Voit valmistella ja yhteyden muodostaminen näennäisen laitteeseen, tarvitset toimimalla seuraavasti:

1.  Varmista, että isäntä-järjestelmän riittävästi resursseja pienin virtual laitteen ehtojen mukaan.

2.  Valmistele oman hypervisor virtual laitteeseen.

3.  Käynnistä virtual laite ja Hae IP-osoite.

Kaikki nämä vaiheet selitetään tarkemmin seuraavissa osissa.

## <a name="step-1-ensure-that-the-host-system-meets-minimum-virtual-device-requirements"></a>Vaihe 1: Varmista, että isäntä-järjestelmän täyttää pienin virtual laitteen vaatimukset

Voit luoda virtuaalisen laitteen, tarvitset:

-   Hyper-V-rooli on asennettu Windows Server 2012 R2, Windows Server 2012: ssa tai Windows Server 2008 R2 SP1.

-   Microsoft Hyper-V hallinta isäntään Microsoft Windows-asiakasohjelmassa.

Varmista pohjana laitteisto (Host ()-isäntäjärjestelmä)-, luot virtual laite ei pysty tälle virtual laite on seuraavissa resursseissa:

- Vähintään 4 sydämiä.
- Vähintään 8 Gigatavun RAM-muistia.
- Yhden verkoston käyttöliittymä.
- 500 gt virtual levyn Järjestelmätiedot.

## <a name="step-2-provision-a-virtual-device-in-hypervisor"></a>Vaihe 2: Valmistella hypervisor virtual laitteeseen

Seuraavien toimien valmistelu oman hypervisor laitteeseen.

#### <a name="to-provision-a-virtual-device"></a>Virtuaalinen laitteen valmistelu

1.  Windows Server-isännän kopioi virtual laitteen kuva paikalliseen asemaan. Tämä on kuva (Näennäiskiintolevyn tai VHDX), jotka olet ladannut palvelun Azure-portaalissa. Pane merkille sijainti, johon kopioit kuvan, kun käytät tätä jäljempänä kuvatulla tavalla.

2.  Avaa **Serverin hallinta**. Valitse oikeasta yläkulmasta **Työkalut** ja valitse **Hyper-V hallinta**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image1.png)

    Jos käytössäsi on Windows Server 2008 R2, Avaa Hyper-V hallinta. Valitse palvelimen hallinnassa **roolit > Hyper-V > Hyper-V Manager**.

1.  **Hyper-V hallinta**ja laajuus-ruudussa järjestelmä-solmu Avaa pikavalikkoa hiiren kakkospainikkeella ja valitse sitten **Uusi** > **virtuaalikoneen**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image2.png)

1.  Valitse ohjattu virtuaalikoneen **ennen kuin aloitat** -sivulla **Seuraava**.

1.  **Määritä nimi ja sijainti** sivulla Anna **nimi** virtual laitteeseesi. Valitse **Seuraava**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image4.png)

1.  **Määritä luonti** sivulla Valitse haluamasi laite kuva ja valitse sitten **Seuraava**. Tämä sivu ei tule näkyviin, jos käytät Windows Server 2008 R2.

    * Valitse **luonti 2** , jos latasit .vhdx kuva Windows Server 2012: n tai sitä uudemmalla versiolla.
    * Valitse **luonti-1** , jos latasit .vhd kuva Windows Server 2008 R2 tai myöhemmin.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image5.png)

1.  **Muistin määrittäminen** -sivulla Määritä **Käynnistys muistin** määrän vähintään **8192 Mt**, ei dynaaminen muistin ottaminen käyttöön ja valitse sitten **Seuraava**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image6.png)

1.  **Määritä verkko** -sivulla Määritä virtual valitsin, joka on yhteydessä Internetiin, ja valitse sitten **Seuraava**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image7.png)

1.  **Yhdistä virtual kiintolevyn** sivulla Käytä **aiemmin virtual kiintolevyn**, virtual laitteen kuva (.vhdx tai .vhd) sijainti ja valitse sitten **Seuraava**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image8m.png)

1.  Tarkista **Yhteenveto** ja valitse sitten Luo virtuaalikoneen **Valmis** . Mutta ei siirry eteenpäin vielä - haluat lisätä joitakin suorittimen Sydämiä ja toinen asema. 

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image9.png)

1.  Vähimmäisvaatimukset, sinun on 4 sydämiä. Voit lisätä virtual suorittimien valittuna **Hyper-V hallinta** -ikkunan oikeanpuoleisessa ruudussa kohdan **näennäiskoneiden**-luettelosta host järjestelmän Etsi juuri luomasi virtuaalikoneen. Valitse tietokonenimeä hiiren kakkospainikkeella ja valitse **asetukset**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image10.png)

1.  Valitse **asetukset** -sivun vasemmassa ruudussa **suoritin**. Määritä oikeanpuoleisessa ruudussa **virtual suorittimien määrän** 4 (tai useamman). Valitse **Käytä**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image11.png)

1.  Vähimmäisvaatimukset, myös haluat lisätä 500 gt virtual tietojen levyn. Valitse **asetukset** -sivulla:

    1.  Valitse vasemmanpuoleisessa ruudussa **SCSI-ohjain**.
    2.  Valitse oikeanpuoleisen ruudun **Kiintolevyn,** ja valitse **Lisää**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image12.png)

1.  **Kiintolevy** sivulla Valitse **Virtual kiintolevy** -vaihtoehto ja valitse **Uusi**. **Virtuaalinen kiintolevyn ohjattu**käynnistyy.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image13.png)

1.  Valitse ohjattu Virtual kiintolevyn **ennen kuin aloitat** -sivulla **Seuraava**.

1.  **Valitse Levyn muoto-sivu**hyväksy oletusasetus **VHDX** muoto. Valitse **Seuraava**. Tässä näytössä ei näy, jos käytössäsi on Windows Server 2012 R2 tai Windows Server 2008 R2.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image15.png)

1.  **Valitse levyn tyyppi-sivulla**määrittäminen virtual kiintolevyn tyyppi **dynaamisesti laajentaminen** (suositus). Jos valitset **kiinteän kokoinen** levyn, se toimii myös, mutta joudut ehkä odottamaan kauan. On suositeltavaa, että et käytä **Differencing** -vaihtoehto. Valitse **Seuraava**. Huomaa, että **dynaamisesti laajentaminen** on oletusarvoisesti Windows Server 2012 R2: n ja Windows Server 2012. Windows Server 2008 R2 oletusarvo on **kiinteän kokoinen**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image16.png)

1.  **Määritä nimi ja sijainti** -sivulla antaa tietoja levy **nimi** sekä **sijainti** (Voit selata yhteen). Valitse **Seuraava**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image17.png)

1.  **Levyn määrittäminen** -sivulla valitsemalla **Luo uusi tyhjä virtual kiintolevy** ja määritä koko **500 Gigatavua** (tai useamman). Voit valmistella suurempi DVD-levyllä aina, 500 gt: N ollessa vähimmäisvaatimukset. Huomaa, että ei voi laajentaa tai pienentää levyn kerran valmistelun yhteydessä. Lisätietoja koon levyn valmistelu on Tarkista [parhaat käytännöt](storsimple-ova-best-practices.md#configuration-best-practices) -tiedoston koon muuttaminen-osaa. Valitse **Seuraava**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image18.png)

1.  Valitse **Yhteenveto** -sivulla Tarkista virtual tietojen levytilaa tiedot ja jos täyty, valitse **Valmis** luomaan levy. Ohjattu toiminto päättyy ja virtual kiintolevyn lisätään tietokoneeseesi.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image19.png)

2.  Voit palauttaa- **asetusten** sivulle. Valitse **asetukset** -sivulla Sulje ja palaa Hyper-V hallintaikkunan **OK** .

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image20.png)

## <a name="step-3-start-the-virtual-device-and-get-the-ip"></a>Vaihe 3: Käynnistä virtual laite ja Hae IP

Seuraavien toimien virtual laitteen ja muodostaa siihen yhteyden.

#### <a name="to-start-the-virtual-device"></a>Aloita virtual laite

1.  Käynnistä virtual laite.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image21.png)

1.  Sen jälkeen, kun laite on käynnissä, valitse laite, napsauta hiiren kakkospainikkeella ja valitse **Yhdistä**.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image22.png)

1.  Saatat joutua odottamaan 5-10 minuutin laite on valmis. Tila-sanoma tulee näkyviin konsolin edistyminen ilmaista. Kun laite on valmis, siirry kohtaan **toiminto**. Paina `Ctrl + Alt + Delete` kirjautua virtual laite. Oletuskäyttäjä on *StorSimpleAdmin* ja oletusarvon salasana on *Salasana1*.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image23.png)

1.  Tietoturvasyistä laitteen järjestelmänvalvojan salasanan vanhenee ensimmäisen lokista. Voit pyydetään salasanan vaihtaminen.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image24.png)

    Kirjoita salasana, joka sisältää vähintään 8 merkkiä. Salasana on täytettävä ainakin 3, 4 seuraavien ehtojen mukaan ulos: merkkejä isoja, pieni, numeeriset ja erikoismerkkejä. Kirjoita salasana kirjoittamalla se uudelleen. Saat ilmoituksen, että salasana on vaihdettu.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image25.png)

1.  Kun salasana on vaihdettu, virtual laite saattaa uudelleen. Odota, Käynnistä laite.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image26.png)

    Windows PowerShell console laitteen näkyvät sekä tilanneilmaisin.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image27.png)

1.  6-8 vaiheet koskevat vain ympäristössä, jossa ei ole DHCP määrittäminen käynnistettäessä. Jos olet DHCP ympäristössä, Ohita nämä vaiheet ja siirry vaiheeseen 9. Jos käynnistetty määrittäminen laitteeseesi ei DHCP ympäristössä, seuraava näyttö tulee näkyviin.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image28m.png)

    Sinun on nyt verkon määrittäminen.

1.  Käytä `Get-HcsIpAddress` -komennon luettelosta verkkoliittymät virtual laitteen käytössä. Jos laite on otettu käyttöön yhden verkoston käyttöliittymä, määritetty liittymän oletusnimi on `Ethernet`.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image29m.png)

1.  Käytä `Set-HcsIpAddress` cmdlet-komento, Määritä verkko. Esimerkki on alla:

    `Set-HcsIpAddress –Name Ethernet –IpAddress 10.161.22.90 –Netmask 255.255.255.0 –Gateway 10.161.22.1`

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image30.png)

1.  Kun ensimmäinen asennus on valmis ja laite on käynnistetty ylös, näet laitteen ilmoituspalkin teksti. Pane merkille IP-osoite ja hallita laitteen ilmoituspalkin teksti näkyy URL-osoite. IP-osoitteen avulla WWW-Käyttöliittymää virtual laitteesi yhteyden ja paikallisen asennuksen ja rekisteröintiä.

    ![](./media/storsimple-ova-deploy2-provision-hyperv/image31m.png)



1. (Valinnainen) Suorita tätä vaihetta, vain, jos otat laitteesi Government pilveen. Yhdysvallat FIPS Federal Information Processing Standard ()-tilassa nyt kokoustyötilaa laitteessasi. FIPS 140-standardi määrittää salauksen algoritmit US Federal luottamuksellisten tietojen suojaamista government tietokonejärjestelmien hyväksynyt käyttöä varten.
    1. FIPS-tila käyttöön ajamalla seuraavan cmdlet-komennon:

        `Enter-HcsFIPSMode`

    2. Käynnistä laite uudelleen, kun olet ottanut FIPS-tilassa niin, että salauksen vahvistukset tulee voimaan.

        > [AZURE.NOTE] Voit ottaa käyttöön tai laitteen FIPS-tilan poistaminen käytöstä. Vaihteleva FIPS ja FIPS tilan laite ei tue.

Jos laite ei täytä vähintään määritykset, näet virheen ilmoituspalkin teksti (kuten alla). Sinun on muokattava laitteen määrityksiä niin, että se on riittävästi resursseja vähimmäisvaatimukset. Voit käynnistää uudelleen ja muodostaa yhteyden laitteeseen. Pienin määritykset-vaatimusten viitata [Vaihe 1: Varmista, että isäntä-järjestelmän täyttää pienin virtual laitteen vaatimukset](#step-1-ensure-that-the-host-system-meets-minimum-virtual-device-requirements).

![](./media/storsimple-ova-deploy2-provision-hyperv/image32.png)

Jos jokin muu virhe tuloste paikallisen WWW-Käyttöliittymää käyttäen sen ensimmäisen määrityksen aikana, tarkista seuraavat työnkulut [hallinta StorSimple Virtual matriisin](storsimple-ova-web-ui-admin.md)käyttämisen paikallisen sivuston Käyttöliittymä.

-   [Web-Käyttöliittymän asennuksen](storsimple-ova-web-ui-admin.md#troubleshoot-web-ui-setup-errors)vianmääritys diagnostiikan testien suorittaminen

-   [Luo lokitiedoston paketin ja Näytä lokitiedostot](storsimple-ova-web-ui-admin.md#generate-a-log-package).

![videokuvake](./media/storsimple-ova-deploy2-provision-hyperv/video_icon.png)  **Video käytettävissä**

Katso video ja katso, miten voit valmistella StorSimple Virtual matriisin Hyper-v.

> [AZURE.VIDEO create-a-storsimple-virtual-array]

## <a name="next-steps"></a>Seuraavat vaiheet

-   [Määritetty StorSimple Virtual matriisin tiedostopalvelimeen](storsimple-ova-deploy3-fs-setup.md)

-   [Määritetty StorSimple Virtual matriisin iSCSI-palvelin](storsimple-ova-deploy3-iscsi-setup.md)
