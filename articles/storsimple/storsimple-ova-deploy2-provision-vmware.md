<properties
   pageTitle="Ottaa käyttöön StorSimple Virtual matriisi - VMware valmistelu"
   description="Tässä toisessa opetusohjelmassa StorSimple Virtual matriisin käyttöönoton sarjan liittyy valmistelu VMware virtual laitteeseen."
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
   ms.date="04/12/2016"
   ms.author="alkohli"/>


# <a name="deploy-storsimple-virtual-array---provision-a-virtual-array-in-vmware"></a>Ottaa käyttöön StorSimple Virtual matriisi - Virtual matriisi-VMware valmistelu

![](./media/storsimple-ova-deploy2-provision-vmware/vmware4.png)

## <a name="overview"></a>Yleiskatsaus 
Valmistelun Tässä opetusohjelmassa koskee StorSimple Virtual matriisien (tunnetaan myös nimellä StorSimple paikallisen virtual tai StorSimple virtual laitteet) käynnissä maaliskuussa 2016 yleiseen käyttöön (GA)-versiossa. Tässä opetusohjelmassa kerrotaan, miten voit valmistella ja yhdistäminen StorSimple Virtual matriisin host järjestelmässä käytössä VMware ESXi 5.5 ja yläpuolella. Tämä artikkeli koskee Azure perinteinen portal sekä Microsoft Azure Government Cloud StorSimple Virtual matriisien käyttöönotto.

Voit pitää olla järjestelmänvalvojan oikeudet valmistelu ja yhteyden muodostaminen näennäisen laitteeseen. Valmistelu ja ensimmäinen määrittäminen voi kestää noin 10 minuutin suorittamiseen.


## <a name="provisioning-prerequisites"></a>Valmistelu edellytykset

Tätä löydät edellytykset valmistelu virtual laitteeseen host järjestelmässä käytössä VMware ESXi 5.5 ja yläpuolella.

### <a name="for-the-storsimple-manager-service"></a>StorSimple hallinta-palveluun

Ennen kuin aloitat, varmista, että:

-   Olet tehnyt kaikki [valmisteleminen portaalin StorSimple Virtual matriisin](storsimple-ova-deploy1-portal-prep.md)vaiheet.

-   Olet ladannut virtual laitteen kuva, VMware Azure-portaalista. Lisätietoja on artikkelissa [Vaihe 3: lataa virtual laitteen kuva](storsimple-ova-deploy1-portal-prep.md#step-3-download-the-virtual-device-image).

### <a name="for-the-storsimple-virtual-device"></a>StorSimple virtual laitteen 

Ennen kuin otat virtual laitteen, varmista, että:

-   Sinulla on käynnissä Hyper-V Host (isäntä)-järjestelmän (2008 R2 tai uudempi), joka voi olla varaus käytettävä laite.

-   Host ()-isäntäjärjestelmä pystyy tälle valmistelu virtual laite on seuraavissa resursseissa:

    -   Vähintään 4 sydämiä.

    -   Vähintään 8 Gigatavun RAM-muistia.

    -   Yhden verkoston käyttöliittymä.

    -   500 gt virtual levyn Järjestelmätiedot.

### <a name="for-the-network-in-datacenter"></a>Verkossa, joten 

Ennen kuin aloitat, varmista, että:

-   Olet tarkistanut ottamaan StorSimple virtual laitteen verkko vaatimukset ja määrittänyt palvelinkeskuksen verkon mukaisesti. Lisätietoja on artikkelissa [StorSimple Virtual matriisin järjestelmävaatimukset](storsimple-ova-system-requirements.md).

## <a name="step-by-step-provisioning"></a>Vaiheittaiset ohjeet valmistelu 

Voit valmistella ja yhteyden muodostaminen näennäisen laitteeseen, tarvitset toimimalla seuraavasti:

1.  Varmista, että isäntä-järjestelmän riittävästi resursseja pienin virtual laitteen ehtojen mukaan.

2.  Valmistele oman hypervisor virtual laitteeseen.

3.  Käynnistä virtual laite ja Hae IP-osoite.

## <a name="step-1-ensure-host-system-meets-minimum-virtual-device-requirements"></a>Vaihe 1: Varmista host järjestelmän vastaa pienin virtual laite

Voit luoda virtuaalisen laitteen, tarvitset:

-   Host (isäntä)-järjestelmän käynnissä VMware ESXi Server 5.5 ja yllä käyttöoikeus.

-   VMware vSphere asiakas-järjestelmään hallittavan ESXi isäntä.

    -   Vähintään 4 sydämiä.

    -   Vähintään 8 Gigatavun RAM-muistia.

    -   Yhden verkoston liittymän yhteydessä verkkoon voi reitittää liikenteen Internet-yhteys. Pienin Internet-kaistanleveys on oltava 5 Mbps sallimaan optimaalisen toiminnan laitteen.

    -   500 gt virtual DVD-levyllä tiedoille.

## <a name="step-2-provision-a-virtual-device-in-hypervisor"></a>Vaihe 2: Valmistella hypervisor virtual laitteeseen

Seuraavien toimien valmistelu oman hypervisor virtual laitteeseen.

1.  Kopioi virtual laitteen kuva järjestelmään. Tämä on kuva, joita olet ladannut palvelun Azure perinteinen-portaalissa. 
    1.  Varmista, että tämä on uusin kuvatiedosto, joita olet ladannut. Jos latasit aikaisemmin kuvan, lataa se uudelleen, jos haluat varmistaa, käytössäsi ovat uusimmat kuva. Uusimman kuva on kaksi tiedostoja (sijaan yksi).
    2.  Pane merkille sijainti, johon kopioit kuvan, kun käytät tätä jäljempänä kuvatulla tavalla.

2.  Kirjaudu sisään vSphere käyttävä ESXi palvelimeen. Haluat luoda virtual tietokoneen järjestelmänvalvojan oikeudet.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image1.png)

1.  Valitse vSphere-asiakasohjelman varaston-kohta vasemmasta ruudusta ESXi-palvelin.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image2.png)

1.  Ensin Lataa VMDK ESXi palvelimeen. Siirry oikeanpuoleisen ruudun **määritys** -välilehti. Valitse **laitteisto**- **tallennustilan**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image3.png)

1.  Valitse oikeanpuoleisen ruudun kohdasta **Datastores**datastore, johon haluat ladata VMDK. Datastore on oltava OS ja tietoja-levyjen tarpeeksi tilaa.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image4.png)

1.  Napsauta hiiren kakkospainikkeella ja valitse **Selaa Datastore**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image5.png)

1.  **Datastore** selainikkunassa tulee näkyviin.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image6.png)

1.  Valitse työkalurivillä ![](./media/storsimple-ova-deploy2-provision-vmware/image7.png) kuvaketta, jos haluat luoda uuden kansion. Määritä kansionimi ja paina se mieleen. Tämän kansionimi käyttää myöhemmin luotaessa virtual koneen (suositus parhaiden käytäntöjen mukaisesti). Valitse **OK**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image8.png)

1.  Uusi kansio näkyy vasemmassa ruudussa **Datastore selaimessa**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image9.png)

1.  Valitse latauksen kuvake ![](./media/storsimple-ova-deploy2-provision-vmware/image10.png) ja valitse sitten **Lataa tiedosto**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image11.png)

1.  Pitäisi nyt Selaa ja valitse VMDK tiedostot, jotka olet ladannut. On kaksi tiedostoa. Valitse ladattava tiedosto.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image12m.png)

1.  Valitse **Avaa**. Tämä käynnistää nyt määritetty datastore VMDK tiedoston lataus. Voi kestää useita minuutteja ladattava tiedosto.


1.  Kun lataus on valmis, näyttöön tulee kansio, jonka loit datastore-tiedosto. 

    ![](./media/storsimple-ova-deploy2-provision-vmware/image14.png)

    Sinun on nyt toinen VMDK tiedosto ladataan samaan datastore.

1.  Palaa vSphere asiakas-ikkunaan. ESXi-palvelimeen on valittuna napsauttamalla hiiren kakkospainikkeella ja valitse **Uusi Virtual Machine**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image15.png)

1.  **Luo uusi Virtual Machine** -ikkuna tulee näkyviin. Valitse **määritys** -sivulla **Mukautettu** -vaihtoehto. Valitse **Seuraava**.
    ![](./media/storsimple-ova-deploy2-provision-vmware/image16.png)

2.  Määritä **nimi ja sijainti** -sivulla virtuaalikoneen nimi. Tämä nimi tulee vastata aiemmin vaihe 8: ssa määrittämäsi kansionimi (suositeltava parhaita käytäntöjä).

    ![](./media/storsimple-ova-deploy2-provision-vmware/image17.png)

1.  Valitse **tallennustilan** -sivulla datastore, jota haluat käyttää valmistelu oman AM.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image18.png)

1.  Valitse **Virtuaalikoneen versio** -sivulla **virtuaalikoneen versio: 8**. Huomaa, että versiot 8-11 tuetaan.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image19.png)

1.  **Vieras-käyttöjärjestelmän** sivulla Valitse **Vieras-käyttöjärjestelmän** **Windows**. Valitse avattavasta luettelosta **Version** **Microsoft Windows Server 2012 (64-bittinen)**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image20.png)

1.  **Suorittimessa** sivulla muuta **virtual sockets määrä** ja **sydämiä kohti virtual socket määrä** niin, että **sydämiä määrä** on 4 (tai useamman). Valitse **Seuraava**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image21.png)

1.  Määritä 8 Gigatavun (tai useamman) RAM-muistia **muistin** -sivulla. Valitse **Seuraava**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image22.png)

1.  Valitse **Verkko** -sivun Määritä verkkoliittymät määrä. Vähimmäisvaatimukset on yksi verkkokäyttöliittymä.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image23.png)

1.  **SCSI-ohjain** sivulla Hyväksy oletusasetukset **LSI logiikan SAS ohjauskoneen**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image24.png)

1.  Valitse **Valitse levy** -sivulla **Käytä olemassa olevaa virtual levyä**. Valitse **Seuraava**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image25.png)

1.  **Valitse aiemmin luotu levy** -sivulla **Levyn polku**-valitsemalla **Selaa**. **Selaa Datastores** -valintaikkuna avautuu. Siirry sijaintiin, johon olet ladannut VMDK. Näet nyt datastore vain yhden tiedoston, kun kaksi tiedostot, jotka olet ladannut alun perin on yhdistetty. Valitse tiedosto ja valitse **OK**. Valitse **Seuraava**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image26.png)

1.  Valitse **Lisäasetukset** -sivulla Hyväksy oletusasetukset ja valitse **Seuraava**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image27.png)

1.  Tarkista **Valmis valmis** -sivulla kaikki uudet virtuaalikoneen liittyvät asetukset. Tarkista **ennen valmistumista virtuaalikoneen-asetusten muokkaaminen**. Valitse **Jatka**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image28.png)

1.  Etsi laitteisto **Näennäiskoneiden ominaisuudet** -sivun **laitteisto** -välilehti. Valitse **uusi kiintolevy**. Valitse **Lisää**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image29.png)

1.  Tämä näyttää **Lisää laite** -ikkuna. **Laitteen tyyppi** -sivulla, **Valitse haluamasi laite, johon haluat lisätä**-, valitse **kiintolevy** ja valitse **Seuraava**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image30.png)

1.  Valitse **Valitse levy** -sivulla **Luo uusi virtual levy**. Valitse **Seuraava**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image31.png)

1.  **Luo DVD-levyllä** sivulla 500 Gigatavua (tai useamman) **Levyn koon** muuttaminen. Valitse **Levyn valmistelu**, **Ohut säännöstä**. Valitse **Seuraava**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image32.png)

1.  Valitse **Lisäasetukset** -sivulla Hyväksy oletusasetukset.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image33.png)

1.  Tarkista **Valmis valmis** -sivulla levyn asetukset. Valitse **Valmis**.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image34.png)

1.  Voit nyt palauttaa virtuaalikoneen ominaisuudet-sivulle. Uuden kiintolevyn lisätään virtuaalikoneen. Valitse **Valmis**.
  
    ![](./media/storsimple-ova-deploy2-provision-vmware/image35.png)

2.  Siirry virtuaalikoneen valittuna oikeanpuoleisen ruudun, jossa **Yhteenveto** -välilehti. Tarkista virtuaalikoneen asetuksia.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image36.png)

Virtuaalikoneen nyt valmistelun yhteydessä. Seuraavaksi power laitteessa ja IP-osoite.

## <a name="step-3-start-the-virtual-device-and-get-the-ip"></a>Vaihe 3: Käynnistä virtual laite ja Hae IP

Seuraavien toimien virtual laitteen ja muodostaa siihen yhteyden.

#### <a name="to-start-the-virtual-device"></a>Aloita virtual laite

1.  Käynnistä virtual laite. Vasemmassa ruudussa vSphere määritysten hallinta-kohdassa Valitse laite ja tuo pikavalikko näkyviin napsauttamalla hiiren kakkospainiketta. Valitse **Power** ja valitse sitten **virta**. Virtuaalikoneen olisi power. Voit tarkastella vSphere asiakkaan ala **Viimeisimmät tehtävät** -ruudussa.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image37.png)

1.  Asennuksen tehtävät kestää muutaman minuutin suorittamiseen. Kun laite on käynnissä, siirry **Console** -välilehti. Lähetä Ctrl + Alt + Delete kirjautua laitteeseen. Voit vaihtoehtoisesti Vie kohdistin console-ikkunassa ja painamalla Ctrl + Alt + Insert. Oletuskäyttäjä on *StorSimpleAdmin* ja oletusarvon salasana on *Salasana1*.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image38.png)

1.  Tietoturvasyistä laitteen järjestelmänvalvojan salasanan vanhenee ensimmäisen lokista. Voit pyydetään salasanan vaihtaminen.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image39.png)

1.  Kirjoita salasana, joka sisältää vähintään 8 merkkiä. Salasanassa on oltava 3 poissa 4 nämä vaatimukset: merkkejä isoja, pieni, numeeriset ja erikoismerkkejä. Kirjoita salasana kirjoittamalla se uudelleen. Saat ilmoituksen, että salasana on vaihdettu.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image40.png)

1.  Kun salasana on vaihdettu, virtual laitteen voi käynnistää uudelleen. Odota uudelleenkäynnistyksen suorittamiseen. Windows PowerShell console laitteen voi näyttää sekä tilanneilmaisin.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image41.png)

1.  6-8 vaiheet koskevat vain ympäristössä, jossa ei ole DHCP määrittäminen käynnistettäessä. Jos olet DHCP ympäristössä, Ohita nämä vaiheet ja siirry vaiheeseen 9. Jos käynnistetty määrittäminen laitteeseesi ei DHCP ympäristössä, seuraava näyttö tulee näkyviin. 

    ![](./media/storsimple-ova-deploy2-provision-vmware/image42m.png)

    Sinun on nyt verkon määrittäminen.

1.  Käytä `Get-HcsIpAddress` -komennon luettelosta verkkoliittymät virtual laitteen käytössä. Jos laite on otettu käyttöön yhden verkoston käyttöliittymä, määritetty liittymän oletusnimi on `Ethernet`.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image43m.png)

1.  Käytä `Set-HcsIpAddress` cmdlet-komento, Määritä verkko. Esimerkki on alla:


    `Set-HcsIpAddress –Name Ethernet –IpAddress 10.161.22.90 –Netmask 255.255.255.0 –Gateway 10.161.22.1`

    ![](./media/storsimple-ova-deploy2-provision-vmware/image44.png)

1.  Kun ensimmäinen asennus on valmis ja laite on käynnistetty ylös, näet laitteen ilmoituspalkin teksti. Pane merkille IP-osoite ja hallita laitteen ilmoituspalkin teksti näkyy URL-osoite. IP-osoitteen avulla WWW-Käyttöliittymää virtual laitteesi yhteyden ja paikallisen asennuksen ja rekisteröintiä.

    ![](./media/storsimple-ova-deploy2-provision-vmware/image45.png)


1. (Valinnainen) Suorita tätä vaihetta, vain, jos otat laitteesi Government pilveen. Yhdysvallat FIPS Federal Information Processing Standard ()-tilassa nyt kokoustyötilaa laitteessasi. FIPS 140-standardi määrittää salauksen algoritmit US Federal luottamuksellisten tietojen suojaamista government tietokonejärjestelmien hyväksynyt käyttöä varten.
    1. FIPS-tila käyttöön ajamalla seuraavan cmdlet-komennon:
        
        `Enter-HcsFIPSMode`

    2. Käynnistä laite uudelleen, kun olet ottanut FIPS-tilassa niin, että salauksen vahvistukset tulee voimaan.

        > [AZURE.NOTE] Voit ottaa käyttöön tai laitteen FIPS-tilan poistaminen käytöstä. Vaihteleva FIPS ja FIPS tilan laite ei tue.


Jos laite ei täytä vähintään määritykset, näet virheen ilmoituspalkin teksti (kuten alla). Sinun on muokattava laitteen määrityksiä niin, että se on riittävästi resursseja vähimmäisvaatimukset. Voit käynnistää uudelleen ja muodostaa yhteyden laitteeseen. Pienin määritykset-vaatimusten viitata [Vaihe 1: Varmista, että isäntä-järjestelmän täyttää pienin virtual laitteen vaatimukset](#step-1-ensure-host-system-meets-minimum-virtual-device-requirements).

![](./media/storsimple-ova-deploy2-provision-vmware/image46.png)

Jos jokin muu virhe tuloste paikallisen WWW-Käyttöliittymää käyttäen sen ensimmäisen määrityksen aikana, tarkista seuraavat työnkulut [hallinta StorSimple Virtual matriisin](storsimple-ova-web-ui-admin.md)käyttämisen paikallisen sivuston Käyttöliittymä.

-   [Web-Käyttöliittymän asennuksen](storsimple-ova-web-ui-admin.md#troubleshoot-web-ui-setup-errors)vianmääritys diagnostiikan testien suorittaminen

-   [Luo lokitiedoston paketin ja Näytä lokitiedostot](storsimple-ova-web-ui-admin.md#generate-a-log-package)...

## <a name="next-steps"></a>Seuraavat vaiheet

-   [Määritetty StorSimple Virtual matriisin tiedostopalvelimeen](storsimple-ova-deploy3-fs-setup.md)

-   [Määritetty StorSimple Virtual matriisin iSCSI-palvelin](storsimple-ova-deploy3-iscsi-setup.md)

