<properties 
   pageTitle="StorSimple tilannevedoksen hallinta ja asemat | Microsoft Azure"
   description="Tässä artikkelissa käsitellään avulla StorSimple tilannevedoksen hallinnan MMC-laajennuksen, voit tarkastella ja asemat hallinta ja varmuuskopioinnin määrittäminen."
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="04/18/2016"
   ms.author="v-sharos" />

# <a name="use-storsimple-snapshot-manager-to-view-and-manage-volumes"></a>StorSimple tilannevedoksen hallinnan avulla voit tarkastella ja asemat hallinta

## <a name="overview"></a>Yleiskatsaus

Voit käyttää StorSimple tilannevedoksen hallinta **asemat** -solmu ( **laajuus** -ruutu) Valitse asemat ja tarkastella niitä koskevia tietoja. Asemat esitetään kuin asemista, jotka vastaavat liitetty isäntä asemat. **Asemat** -solmu näyttää asemat ja aseman tyypit, jotka tukevat StorSimple, mukaan lukien havaitsi iSCSI ja laitteen asemat. 

Lisätietoja tuetuista tietomääristä Siirry [tuki useita aseman tyyppejä](storsimple-what-is-snapshot-manager.md#support-for-multiple-volume-types).

![Äänenvoimakkuus-luettelosta tulosruudussa](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Volume_node.png)

**Asemat** -solmu avulla voit tarkistaa uudelleen tai poistaa asemat, kun StorSimple tilannevedoksen hallinta löytää ne. 

Tässä opetusohjelmassa kerrotaan, miten voit ottaa käyttöön, alusta, ja muotoilla asemat ja käytä StorSimple tilannevedoksen hallinta:

- Asemat tietojen tarkasteleminen 
- Poista tietomääristä
- Asemat uudelleen 
- Määrittää basic äänenvoimakkuutta ja takaisin ylöspäin
- Määritä dynaaminen peilattu asema ja takaisin ylöspäin

>[AZURE.NOTE] Kaikki **aseman** solmu toiminnot ovat käytettävissä myös **Toiminnot** -ruudussa.
 
## <a name="mount-volumes"></a>Ota käyttöön tietomääristä

Seuraavien ohjeiden avulla voit ottaa käyttöön, alusta ja muotoilla StorSimple asemat. Tässä menetelmässä Levynhallinta-järjestelmän-apuohjelma kiintolevyä ja vastaavan asemat tai osioiden hallintaan. Lisätietoja Disk Management Siirry [Disk Management](https://technet.microsoft.com/library/cc770943.aspx) Microsoft TechNet-sivustossa.

#### <a name="to-mount-volumes"></a>Ota käyttöön tietomääristä

1. Host (isäntä)-tietokoneessa Käynnistä Microsoft iSCSI-käynnistäjä.

2. Anna käyttöliittymän IP-osoitteiden kuin kohdeportaalin tai etsiminen IP-osoite, ja muodostaa yhteyden laitteeseen. Kun laite on yhdistetty, asemat voi käyttää Windows-järjestelmään. Lisätietoja Microsoft iSCSI-käynnistäjä, siirry kohtaan [asentaminen ja määrittäminen Microsoft iSCSI käynnistäjä]"yhteyden iSCSI-kohteen laitteeseen"[1].

3. Levynhallinnan käynnistäminen jollakin seuraavista vaihtoehdoista:

    - Kirjoita **Suorita** -valintaikkunaan Diskmgmt.msc.

    - Käynnistä Serverin hallinta, laajenna **tallennustilan** -solmu ja valitse **Disk Management**.

    - Käynnistä **Valvontatyökalut**, laajenna **Tietokoneenhallinta** -solmu ja valitse **Disk Management**. 

    >[AZURE.NOTE] Voit suorittaa Levynhallinnan käytettävä järjestelmänvalvojan oikeudet.
 
4. Tee asema(t) verkossa:

   1. Levyn hallinnassa Napsauta hiiren kakkospainikkeella minkä tahansa asemaa merkitty **offline-tilassa**.

   2. Valitse **Aktivoi levy uudelleen**. Kun levyn aktivoidaan levyn merkitään **Online** .

5. Alusta asema(t):

   1. Napsauta hiiren kakkospainikkeella havaitun asemat.

   2. Valitse-valikosta **Alusta levy**.

   3. Valitse **Alusta levy** -valintaikkunassa valitse levyjä, jotka haluat alusta ja valitse sitten **OK**.

6. Tavalliset asemat muotoileminen:

   1. Napsauta hiiren kakkospainikkeella asemaa, jonka haluat muotoilla.

   2. Valitse-valikosta **Uusi tavallinen asema**.

   3. Uusi tavallinen asema ohjatun toiminnon avulla voit muotoilla äänenvoimakkuuden:

      - Määritä Asemakoko.
      - Anna kirjaimen.
      - Valitse NTFS-tiedostojärjestelmää.
      - Määritä 64 Kilotavua kohdistus yksikön koko.
      - Voit suorittaa nopean muoto.

7. Voit muotoilla usean osion asemat. Siirry ohjeet "Osiot ja asemat" [käyttöönoton levyn](https://msdn.microsoft.com/library/dd163556.aspx)hallinta-osassa.

## <a name="view-information-about-your-volumes"></a>Jälkeen asemat tietojen tarkasteleminen

Seuraavien ohjeiden avulla voit tarkastella tietoja paikallisen ja Azure StorSimple asemat.

#### <a name="to-view-volume-information"></a>Voit tarkastella aseman tiedot

1. Napsauta työpöydän StorSimple tilannevedoksen hallinnan käynnistäminen kuvaketta. 

2. Valitse **laajuus** -ruudun **asemat** -solmu. Paikalliset ja käyttöön otettujen asemat, mukaan lukien kaikki Azure StorSimple asemat luettelo tulee näkyviin **tulosruudussa** . **Tulosruudussa** sarakkeet määritettäviä. ( **Asemat** -solmu hiiren kakkospainikkeella, valitse **näkymä**ja valitse sitten **Lisää tai Poista sarakkeet**.)

    ![Sarakkeiden määrittäminen](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_View_volumes.png)

    Tulokset-sarakkeessa | Kuvaus 
    :--------------|:-------------
    Nimi           | **Nimi** -sarakkeessa kunkin havaitun asemaan kirjain.
    Laite         | **Laitteen** -sarakkeessa laite on liitettynä isäntätietokoneen IP-osoite.
    Laitteen asemanimi | **Laitteen asemanimi** -sarakkeessa laitteen asema, johon kuuluu valitun aseman nimi. Tämä on määritetty kyseisen määritetyn aseman Azure perinteinen portaali nimen.
    Polkuja   | **Polkuja** -sarakkeessa näkyy polku äänenvoimakkuutta. Tämä on aseman kirjain tai asennustapa, joita ei voi käyttää isäntätietokoneen äänenvoimakkuutta.
 
## <a name="delete-a-volume"></a>Aseman poistaminen

Seuraavien ohjeiden avulla voit poistaa aseman StorSimple tilannevedoksen Managerista.

>[AZURE.NOTE] Et voi poistaa aseman, jos se on osa aseman jonkin ryhmän. (Poista-vaihtoehto ei ole käytettävissä, jotka ovat aseman ryhmän jäsenien tietomääristä.) Sinun on poistettava koko aseman-ryhmä, johon haluat poistaa asemaa.


#### <a name="to-delete-a-volume"></a>Voit poistaa aseman

1. Napsauta työpöydän StorSimple tilannevedoksen hallinnan käynnistäminen kuvaketta.

2. Valitse **laajuus** -ruudun **asemat** -solmu. 

3. Napsauta hiiren kakkospainikkeella **tulosruudussa** asema, jonka haluat poistaa.

4. Valitse-valikosta **Poista**. 

    ![Aseman poistaminen](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Delete_volume.png) 

5. **Poista asema** -valintaikkuna. Kirjoita **Vahvista** tekstiruutuun ja valitse sitten **OK**.

6. Oletusarvon mukaan StorSimple tilannevedoksen hallinta Varmuuskopioi aseman ennen poistamista. Tämä peruslevyä voit suojata voit tietojen menettämistä jatkossa, jos poisto on tahattoman. StorSimple tilannevedoksen hallinta **Automaattisen tilannevedoksen** edistymisen tulee näkyviin samalla, kun se Varmuuskopioi äänenvoimakkuutta. 

    ![Automaattinen tilannevedoksen viesti](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Automatic_snap.png) 

## <a name="rescan-volumes"></a>Asemat uudelleen

Noudattamalla seuraavia vaiheita voit tarkistaa asemat yhdistetty StorSimple tilannevedoksen hallinta.

#### <a name="to-rescan-the-volumes"></a>Voit tarkistaa asemat

1. Napsauta työpöydän StorSimple tilannevedoksen hallinnan käynnistäminen kuvaketta.

2. **Laajuus** -ruudussa **tietomääristä**hiiren kakkospainikkeella ja valitse sitten **uudelleen asemat**.

    ![Asemat uudelleen](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Rescan_volumes.png)
 
    Tämä toiminto Synkronoi aseman StorSimple tilannevedoksen hallinta. Tulokset näkyvät kaikki muutokset, kuten uusia tai poistetut asemat.

## <a name="configure-and-back-up-a-basic-volume"></a>Määritä ja basic aseman varmuuskopiointi

Seuraavien ohjeiden avulla voit määrittää basic aseman varmuuskopion ja joko Aloita varmuuskopiointi heti tai ajoitettujen varmuuskopioiden-käytännön luominen.

### <a name="prerequisites"></a>Edellytykset

Ennen aloittamista:

- Varmista, että StorSimple laitteen ja host tietokone on määritetty oikein. Saat lisätietoja Siirry [käyttöönotto paikallisen StorSimple laitteen](storsimple-deployment-walkthrough-u2.md).

- Asenna ja määritä StorSimple tilannevedoksen hallinta. Saat lisätietoja Siirry [Käyttöönotto StorSimple tilannevedoksen hallinta](storsimple-snapshot-manager-deployment.md).

#### <a name="to-configure-backup-of-a-basic-volume"></a>Voit määrittää basic aseman varmuuskopiointi

1. Luo tavallinen asema StorSimple laitteeseen.

2. Ottaa käyttöön, alusta ja muotoilla äänenvoimakkuuden kuvatulla tavalla [käyttöön asemat](#mount-volumes). 

3. Napsauta työpöydän StorSimple tilannevedoksen hallinta-kuvaketta. StorSimple tilannevedoksen hallinta-ikkuna. 

4. **Laajuus** -ruudussa hiiren kakkospainikkeella **asemat** -solmu ja valitse sitten **uudelleen asemat**. Kun tarkistus on valmis, luettelon asemista pitäisi näkyä **tulosruudussa** . 

5. **Tulokset** -ruudussa äänenvoimakkuuden hiiren kakkospainikkeella ja valitse sitten **Luo aseman ryhmä**. 

    ![Avauksen ja vaihdon ryhmän luominen](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Create_volume_group.png) 

6. **Luo aseman ryhmä** -valintaikkunassa aseman ryhmän nimi, Määritä asemat ja valitse sitten **OK**.

7. Laajenna **Aseman ryhmät** -solmun **laajuus** -ruudussa. Uuden aseman ryhmän pitäisi näkyä **Aseman ryhmät** -solmun. 

8. Napsauta hiiren kakkospainikkeella ryhmänimen.

    - Voit aloittaa vuorovaikutteinen (tarvittaessa) varmuuskopiointityön napsauttamalla **Tehdä varmuuskopion**. 

    - Ajoittaa automaattisen varmuuskopioinnin, valitse **Luo varmuuskopio käytännön**. **Yleiset** -sivulla luettelosta asema ryhmän. **Ajoitus** -sivulla Lisää aikataulutiedot. Kun olet valmis, valitse **OK**. 

9. Vahvista, että varmuuskopiointi on alkanut, laajenna **Projektit** -solmu **laajuus** -ruudussa ja valitse sitten **käytössä** -solmu. Käynnissä olevat työt näkyy **tulosruudussa** . 

## <a name="configure-and-back-up-a-dynamic-mirrored-volume"></a>Määritä ja dynaaminen peilattu asema varmuuskopiointi

Seuraavien vaiheiden määrittäminen dynaaminen peilattu asema varmuuskopio:

- Vaihe 1: Käytä Levynhallinta dynaaminen peilattu aseman luominen. 

- Vaihe 2: Käytä StorSimple tilannevedoksen hallinnan varmuuskopioinnin määrittäminen.

### <a name="prerequisites"></a>Edellytykset

Ennen aloittamista:

- Varmista, että StorSimple laitteen ja host tietokone on määritetty oikein. Saat lisätietoja Siirry [käyttöönotto paikallisen StorSimple laitteen](storsimple-deployment-walkthrough-u2.md).

- Asenna ja määritä StorSimple tilannevedoksen hallinta. Saat lisätietoja Siirry [Käyttöönotto StorSimple tilannevedoksen hallinta](storsimple-snapshot-manager-deployment.md).

- Määritä kaksi StorSimple laitteeseen. (Esimerkeissä käytettävissä asemat ovat **levyn 1** ja **2 levyn**.) 

### <a name="step-1-use-disk-management-to-create-a-dynamic-mirrored-volume"></a>Vaihe 1: Käytä Disk Management dynaaminen peilattu aseman luominen

Levynhallinta on tarkoitettu kiintolevyä ja asemat tai osioita, jotka ne sisältävät hallinta. Lisätietoja Disk Management Siirry [Disk Management](https://technet.microsoft.com/library/cc770943.aspx) Microsoft TechNet-sivustossa.

#### <a name="to-create-a-dynamic-mirrored-volume"></a>Dynaaminen peilattu aseman luominen

1. Levynhallinnan käynnistäminen jollakin seuraavista vaihtoehdoista: 

   - Avaa **Suorita** -valintaikkunaan, kirjoita **Diskmgmt.msc**ja paina Enter-näppäintä.

   - Käynnistä Serverin hallinta, laajenna **tallennustilan** -solmu ja valitse **Disk Management**. 

   - Käynnistä **Valvontatyökalut**, laajenna **Tietokoneenhallinta** -solmu ja valitse **Disk Management**. 

2. Varmista, että sinulla on kaksi asemaa käytettävissä StorSimple laitteeseen. (Esimerkissä käytettävissä asemat ovat **levyn 1** ja **2 levyn**.) 

3. Levynhallinta-ikkunassa alemmasta oikeanpuoleisessa sarakkeessa **Disk 1** hiiren kakkospainikkeella ja valitse **Uusi peilattu asema**. 

    ![Uusi peilattu asema](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_New_mirrored_volume.png) 

4. **Uusi peilattu asema** ohjatun toiminnon sivulla Valitse **Seuraava**.

5. **Valitse levyjen** -sivulla **levyn 2** Valitse **Valitut** -ruudun, valitse **Lisää**ja valitse sitten **Seuraava**. 

6. Valitse **Aseta aseman kirjain tai polku** -sivulla hyväksy oletusarvot ja valitse sitten **Seuraava**. 

7. Valitse **Muotoile äänenvoimakkuutta** -sivulla **Kohdistus yksikön fonttikoko** -ruudussa **64 kilotavua**. **Suorittaa nopean muodossa** -valintaruutu ja valitse sitten **Seuraava**. 

8. **Uusi peilattu asema Viimeistellään** sivulla Tarkista ja valitse sitten **Valmis**. 

9. Näyttöön tulee osoittamaan basic levyn muunnetaan dynaaminen levy. Valitse **Kyllä**.

    ![Dynaamisen levyn muuntaminen viesti](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Disk_management_msg.png) 

10. Levynhallinta Varmista, että levyn 1 ja 2 levyn näkyvät dynaaminen peilattu asema. (**Dynaaminen** pitäisi näkyä tila-sarakkeessa ja kapasiteetin palkin väri vaihdetaan punainen, joka ilmaisee peilattu asema.) 

    ![Peilattu hallinta dynaamisiksi](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Verify_dynamic_disks_2.png) 
 
### <a name="step-2-use-storsimple-snapshot-manager-to-configure-backup"></a>Vaihe 2: Käytä StorSimple tilannevedoksen hallinta varmuuskopioinnin määrittäminen

Määritä dynaaminen peilattu asema ja joko Aloita varmuuskopiointi heti tai ajoitettujen varmuuskopioiden-käytännön luominen noudattamalla seuraavia vaiheita.

#### <a name="to-configure-backup-of-a-dynamic-mirrored-volume"></a>Dynaaminen peilattu asema varmuuskopioinnin määrittäminen

1. Napsauta työpöydän StorSimple tilannevedoksen hallinta-kuvaketta. StorSimple tilannevedoksen hallinta-ikkuna. 

2. **Laajuus** -ruudussa hiiren kakkospainikkeella **asemat** -solmu ja valitse **uudelleen asemat**. Kun tarkistus on valmis, luettelon asemista pitäisi näkyä **tulosruudussa** . Dynaaminen peilattu asema on merkitty yhteen asemaan. 

3. **Tulokset** -ruudussa dynaaminen peilattu asema hiiren kakkospainikkeella ja valitse sitten **Luo aseman ryhmä**. 

4. **Luo aseman ryhmä** -valintaikkunassa aseman ryhmän nimi, Määritä dynaaminen peilattu asema tähän ryhmään ja valitse sitten **OK**. 

5. Laajenna **Aseman ryhmät** -solmun **laajuus** -ruudussa. Uuden aseman ryhmän pitäisi näkyä **Aseman ryhmät** -solmun. 

6. Napsauta hiiren kakkospainikkeella ryhmänimen. 

    - Voit aloittaa vuorovaikutteinen (tarvittaessa) varmuuskopiointityön napsauttamalla **Tehdä varmuuskopion**. 

    - Ajoittaa automaattisen varmuuskopioinnin, valitse **Luo varmuuskopio käytännön**. **Yleiset** -sivulla luettelosta asema-ryhmä. **Ajoitus** -sivulla Lisää aikataulutiedot. Kun olet valmis, valitse **OK**. 

7. Voit valvoa varmuuskopiointityön, kun se suoritetaan. **Laajuus** -ruudussa Laajenna **Projektit** -solmu ja valitse sitten **käytössä**-projektin tiedot näkyvät **tulosruudussa** . Varmuuskopiointityön päätyttyä, tiedot siirtyvät **viimeisen 24** tunnin projekti-luettelo. 

## <a name="next-steps"></a>Seuraavat vaiheet

- Lisätietoja käyttämisestä [StorSimple tilannevedoksen hallinta ammattimainen StorSimple ratkaisu](storsimple-snapshot-manager-admin.md).
- Lisätietoja käyttämisestä [StorSimple tilannevedoksen hallinta voit luoda ja hallita äänenvoimakkuutta](storsimple-snapshot-manager-manage-volume-groups.md).

<!--Reference links-->
[1]: https://msdn.microsoft.com/library/ee338480(v=ws.10).aspx
