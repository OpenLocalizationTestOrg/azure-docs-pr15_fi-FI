<properties 
   pageTitle="Hallitse StorSimple tilannevedoksen hallinta | Microsoft Azure"
   description="Tässä artikkelissa kerrotaan yhteyden ja hallitse StorSimple StorSimple tilannevedoksen hallinnan MMC-laajennuksen avulla."
   services="storsimple"
   documentationCenter=""
   authors="SharS"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="04/18/2016"
   ms.author="v-sharos" />

# <a name="use-storsimple-snapshot-manager-to-connect-and-manage-storsimple-devices"></a>Muodosta ja hallitse StorSimple StorSimple tilannevedoksen hallinnan avulla

## <a name="overview"></a>Yleiskatsaus

StorSimple tilannevedoksen hallinta **laajuus** solmujen avulla voit tuodut StorSimple laitteen tiedot voit tarkistaa ja päivittää yhdistetyn tallennuslaitteet. Lisäksi Napsauta **laitteet** -solmu ja näet luettelon yhdistettyjen laitteiden ja vastaavan tilatiedot **tulosruudussa** .

![Yhdistettyjen laitteiden](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_connect_devices.png)

**Kuva 1: StorSimple tilannevedoksen Manager yhdistetty laite** 

Sen mukaan, **Näytä** -valinnat **tuloksia** -ruudussa näkyy kunkin laitteen seuraavat tiedot. (Lisätietoja näkymän Siirry [Näytä-valikosta](storsimple-use-snapshot-manager.md#view-menu).


| Tulokset-sarakkeessa  |Kuvaus          |
|:----------------|:--------------------| 
| Nimi            | Laitteen Azure perinteinen portaalissa määritetty nimi|
| Malli           | Laitteen mallin määrä|
| Versio         | Laitteeseen asennettu versio |
| Tila          | Onko laite ei käytettävissä |
| Viimeksi synkronoitu     | Päivämäärä ja kellonaika, kun laite on viimeksi synkronoitu |
| Sarjanumero      | Laitteen sarjanumero |
 
Jos **laitteiden** solmun **laajuus** -ruudussa hiiren kakkospainikkeella, voit valita seuraavista toimista:

- Lisää tai korvaa laite 
- Laite ja tarkista tuonti 
- Päivitä yhdistetyt laitteet 

Jos **laitteet** -solmu ja napsauta hiiren kakkospainikkeella **tulosruudussa** laitteen nimen, voit valita seuraavista toimista:

- Todennusta käyttävän laitteen 
- Laitteen tietojen tarkasteleminen 
- Päivitä laite 
- Poista laitteen määrittäminen 
- Laitteen salasanan muuttaminen

>[AZURE.NOTE] Kaikki nämä toiminnot ovat käytettävissä myös **Toiminnot** -ruudussa.
 
Tässä opetusohjelmassa kerrotaan, miten voit StorSimple tilannevedoksen hallinnan avulla voit yhdistää ja laitteiden hallinta ja suorita seuraavat toimet:

- Lisää tai korvaa laite 
- Laite ja tarkista tuonti 
- Päivitä yhdistetyt laitteet 
- Todennusta käyttävän laitteen 
- Laitteen tietojen tarkasteleminen 
- Yksittäisen laitteen päivittäminen 
- Poista laitteen määrittäminen 
- Vanhentuneen laitteen salasanan vaihtaminen
- Korvaa epäonnistui laite

>[AZURE.NOTE] Yleisiä tietoja StorSimple tilannevedoksen hallinta-käyttöliittymän avulla Siirry [StorSimple tilannevedoksen hallinnan käyttöliittymä](storsimple-use-snapshot-manager.md).


## <a name="add-or-replace-a-device"></a>Lisää tai korvaa laite

Seuraavien ohjeiden avulla voit lisätä tai korvata StorSimple laite.

#### <a name="to-add-or-replace-a-device"></a>Lisää tai korvaa laite

1. Napsauta työpöydän StorSimple tilannevedoksen hallinnan käynnistäminen kuvaketta.

2. **Laajuus** -ruudussa **laitteet** -solmun hiiren kakkospainikkeella ja valitse sitten **Määritä laite**. **Laitteen määrittäminen** -valintaikkuna.

    ![Määritä StorSimple-laite](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_config_device.png) 

3. Valitse **laite** avattavasta-ruudussa tai virtual-laite IP-osoite. 

4. Kirjoita **salasana** -ruutuun StorSimple tilannevedoksen hallinta salasanan, jonka loit laitteen Azure perinteinen-portaalissa. Valitse **OK**. StorSimple tilannevedoksen hallinta etsii laitteella, joka tunnistanut. 

    - Jos laite on käytettävissä, StorSimple tilannevedoksen hallinta Lisää yhteys. 

    - Jos laite ei ole käytettävissä mistä tahansa syystä, StorSimple tilannevedoksen hallinta palauttaa virhesanoman. Valitse **OK** ja sulje virhesanoma ja valitse sitten Sulje **Määritä laite** -valintaikkunassa **Peruuta** .

## <a name="connect-a-device-and-verify-imports"></a>Laite ja tarkista tuonti

Seuraavien ohjeiden avulla voit StorSimple laite ja Tarkista aiemmin aseman ryhmät, joihin on liitetty varmuuskopioiden on tuotu.

#### <a name="to-connect-a-device-and-verify-imports"></a>Laite ja tarkista tuonti

1. Muodosta yhteys laitteeseen StorSimple tilannevedoksen hallinta, noudata Lisää tai korvaa laite. Kun se muodostaa laitetta, StorSimple tilannevedoksen hallinta vastaa seuraavasti:

    - Jos laite ei ole käytettävissä mistä tahansa syystä, StorSimple tilannevedoksen hallinta palauttaa virhesanoman. 

   - Jos laite on käytettävissä, StorSimple tilannevedoksen hallinta Lisää yhteys. Kun valitset laitteen, se näkyy **tulosruudussa** ja tila-kenttä ilmaisee, että laite on **käytettävissä**. StorSimple tilannevedoksen hallinta tuo määritetty laitteen äänenvoimakkuuden ryhmät, jos aseman ryhmät on liitetty varmuuskopiot. Varmuuskopion käytännöt ei tuoda. Äänenvoimakkuuden ryhmät, joihin ei ole liitetty varmuuskopioiden ei tuoda.

2. Napsauta työpöydän StorSimple tilannevedoksen hallinnan käynnistäminen kuvaketta.

3. Alisolmun **laajuus** -ruudussa hiiren kakkospainikkeella ja valitse sitten **Näytä tai piilota tuonnin**.

    ![Valitse Näytä tai piilota tuonnin näyttö](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_Toggle_Imports_Display.png) 

4. **Näytä tai piilota tuonti** -valintaikkuna tulee näyttöön, jossa tuodut aseman ryhmiä ja varmuuskopiot. Valitse **OK**. 

Kun aseman ryhmät ja varmuuskopioiden on tuotu onnistuneesti, voit StorSimple tilannevedoksen hallinta voit hallita niitä samalla tavalla kuin aseman ryhmien ja varmuuskopiot, jotka olet luonut ja määrittänyt StorSimple tilannevedoksen hallinta hallinta. 

## <a name="refresh-connected-devices"></a>Päivitä yhdistetyt laitteet

Seuraavien ohjeiden avulla voit Synkronoi yhdistetyn StorSimple laitteet StorSimple tilannevedoksen hallinta.

####<a name="to-refresh-connected-devices"></a>Voit päivittää yhdistetyn laitteet

1. Napsauta työpöydän StorSimple tilannevedoksen hallinnan käynnistäminen kuvaketta.

2. **Laajuus** -ruudussa **laitteet**hiiren kakkospainikkeella ja valitse sitten **Päivitä laitteet**. Tämä Synkronoi yhdistettyjen laitteiden StorSimple tilannevedoksen hallinta niin, että voit tarkastella aseman ryhmiä ja varmuuskopioiden, mukaan lukien kaikki viimeisimmät lisäykset. 

    ![Päivitä StorSimple-laitteet](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_Refresh_devices.png)
 
**Päivitä laitteet** -toiminto hakee yhdistettyjen laitteiden ryhmistä äänenvoimakkuus ja liittyvän varmuuskopiot. Toisin kuin **uudelleen tietomääristä** **asemat** -solmu käytettävissä **Päivitä laitteet** ei palauta Varmuuskopioi rekisteri.

## <a name="authenticate-a-device"></a>Todennusta käyttävän laitteen

Seuraavien ohjeiden avulla voit todentaa StorSimple laitteen StorSimple tilannevedoksen hallinta.

#### <a name="to-authenticate-a-device"></a>Laitteen tarkistamiseen

1. Napsauta työpöydän StorSimple tilannevedoksen hallinnan käynnistäminen kuvaketta.

2. Valitse **laajuus** -ruudussa **laitteet**.

3. Valitse **tulokset** -ruudun laitteen nimeä hiiren kakkospainikkeella ja valitse sitten **Tarkista**.

4. **Tarkista käyttöoikeus** -valintaikkuna. Laitteen salasana ja valitse sitten **OK**.

    ![Todentaa valintaikkuna](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_Authenticate.png) 
 
## <a name="view-device-details"></a>Laitteen tietojen tarkasteleminen

Seuraavien ohjeiden avulla voit tarkastella StorSimple laite ja tarvittaessa synkronoida laitteen StorSimple tilannevedoksen hallinta.

#### <a name="to-view-and-resynchronize-device-details"></a>Voit tarkastella ja synkronoida laitteen tiedot

1. Napsauta työpöydän StorSimple tilannevedoksen hallinnan käynnistäminen kuvaketta.

2. Valitse **laajuus** -ruudussa **laitteet**.

3. Valitse **tulokset** -ruudun laitteen nimeä hiiren kakkospainikkeella ja valitse sitten **tiedot**. 

4- **Laitteen tiedot** -valintaikkuna tulee näkyviin. Tässä ruudussa näkyy nimi, malli, versio, sarjanumero, tila, kohde iSCSI koko nimi (IQN), ja viimeisen synkronoinnin päivämäärä ja kellonaika. 

   - Valitse Synkronoi laitteen **Synkronoi** .

   - Valitse **OK** tai **Peruuta** Sulje valintaikkuna.

    ![Laitteen tiedot](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_Device_details.png) 
 
## <a name="refresh-an-individual-device"></a>Yksittäisen laitteen päivittäminen

Noudattamalla seuraavia vaiheita voit synkronoida yksittäisen StorSimple laitteen StorSimple tilannevedoksen hallinta.

#### <a name="to-refresh-a-device"></a>Voit päivittää laite

1. Napsauta työpöydän StorSimple tilannevedoksen hallinnan käynnistäminen kuvaketta. 

2. Valitse **laajuus** -ruudussa **laitteet**. 

3. Valitse **tulokset** -ruudun laitteen nimeä hiiren kakkospainikkeella ja valitse sitten **Päivitä laite**. Tämä synkronoi laitteen StorSimple tilannevedoksen hallinta. 

## <a name="delete-a-device-configuration"></a>Poista laitteen määrittäminen

Seuraavien ohjeiden avulla voit poistaa yksittäisiä StorSimple-laitteen määrittäminen StorSimple tilannevedoksen Managerista.

#### <a name="to-delete-a-device-configuration"></a>Jos haluat poistaa laitteen määrittäminen

1. Napsauta työpöydän StorSimple tilannevedoksen hallinnan käynnistäminen kuvaketta. 

2. Valitse **laajuus** -ruudussa **laitteet**. 

3. Valitse **tulokset** -ruudun laitteen nimeä hiiren kakkospainikkeella ja valitse sitten **Poista**. 

4. Näkyviin tulee seuraava sanoma. Valitse **Kyllä** Poista määritykset tai valitsemalla Peruuta poistaminen **ei** .

    ![Poista laitteen määrittäminen](./media/storsimple-snapshot-manager-manage-devices/HCS_SSM_DeleteDevice.png)

## <a name="change-an-expired-device-password"></a>Vanhentuneen laitteen salasanan vaihtaminen

Sinun on annettava salasana tarkistamiseen StorSimple laitteen StorSimple tilannevedoksen hallinta. Määritä salasana, kun laite määrittäminen Windows PowerShell-liittymän avulla. Salasanan voi kuitenkin vanhentua. Tässä tapauksessa voit vaihtaa salasanan Azure perinteinen portaalin. Valitse laite on määritetty StorSimple tilannevedoksen hallinta, ennen kuin salasana vanhentunut, koska laitteen StorSimple tilannevedoksen hallinta on uudelleen todentamista. 

#### <a name="to-change-the-expired-password"></a>Vanhentuneen salasanan vaihtaminen

1. Azure perinteinen-portaalissa Aloita StorSimple hallintapalvelu.

2. Valitse **laitteet** > laitteen**määrittäminen** .

3. Vieritä StorSimple tilannevedoksen hallinta-osaan. Kirjoita salasana, jossa on 14 15 merkkiä. Varmista, että salasana on erilaiset isoja, pieni, numeerinen ja määräten merkit.

4. Kirjoita salasana kirjoittamalla se uudelleen.

5. Valitse sivun alareunassa **Tallenna** .

#### <a name="to-re-authenticate-the-device"></a>Tarkistamiseen laitteen uudelleen

1. Käynnistä StorSimple tilannevedoksen hallinta.

2. Valitse **laajuus** -ruudussa **laitteet**. **Tulokset** -ruudussa näkyy määritetty luettelo. 

3. Valitse laitteen hiiren kakkospainikkeella ja valitse sitten **Tarkista**.

4. **Tarkista** -ikkunassa Kirjoita uusi salasana. 

5. Valitse laitteen hiiren kakkospainikkeella ja valitse **Päivitä laite**. Tämä synkronoi laitteen StorSimple tilannevedoksen hallinta. 

## <a name="replace-a-failed-device"></a>Korvaa epäonnistui laite

Jos StorSimple laitteen epäonnistuu, ja se on korvattu valmiustilassa (automaattisesti) laitteen yhdistäminen uuteen laitteeseen ja tarkastella liittyvät varmuuskopiot seuraavien vaiheiden avulla.

#### <a name="to-connect-to-a-new-device-after-failover"></a>Muodostaa uusi laitteeseen automaattisesti jälkeen

1. Uuteen laitteeseen iSCSI-yhteyden asetukset uudelleen. Ohjeet, siirry "Vaihe 7: Ota, alusta ja Muotoile aseman"- [käyttöönotto paikallisen StorSimple laitteen](storsimple-deployment-walkthrough-u2.md). 

>[AZURE.NOTE] Jos uusi StorSimple laite on sama kuin vanhan IP-osoite, voit ehkä voi yhdistää vanha määritys. 

2. Microsoft StorSimple Management-palvelun pysäyttäminen:

    1. Aloita Serverin hallinta.

    2. Palvelimen hallinnan koontinäytössä **Työkalut** -valikon Valitse **palvelut**. 

    3. Valitse **Microsoft StorSimple hallintapalvelun** **palvelut** -ikkuna. 

    4. Valitse oikeanpuoleisen ruudun **Microsoft StorSimple hallintapalvelun**- **palvelun pysäyttäminen**. 

3. Poista vanha laitteeseen liittyvät määritykset tiedot: 

    1. Selaa Resurssienhallinnassa C:\ProgramData\Microsoft\StorSimple\BACatalog. 

    2. Poista BACatalog-kansiossa olevat tiedostot. 

4. Käynnistä Microsoft StorSimple Management-palvelu: 

    1. Palvelimen hallinnan koontinäytössä **Työkalut** -valikon Valitse **palvelut**. 

    2. Valitse **Microsoft StorSimple hallintapalvelun** **palvelut** -ikkuna. 

    3. Valitse oikeanpuoleisen ruudun **Microsoft StorSimple hallintapalvelun**- **palvelun käynnistäminen uudelleen**. 

5. Käynnistä StorSimple tilannevedoksen hallinta. 

6. Uusi StorSimple laitteen määrittämiseen vaiheiden vaihe 2: StorSimple laite [käyttöön StorSimple tilannevedoksen](storsimple-snapshot-manager-deployment.md)hallinnassa. 

7. Napsauta ylätason solmu (StorSimple tilannevedoksen hallinnan esimerkissä) **laajuus** -ruudussa hiiren kakkospainikkeella ja valitse sitten **Näytä tai piilota tuonnin**. 

8. Sanoma tulee näkyviin, kun tuodut aseman ryhmiä ja varmuuskopiot ovat näkyvissä StorSimple tilannevedoksen hallinta. Valitse **OK**. 

## <a name="next-steps"></a>Seuraavat vaiheet

- Lisätietoja käyttämisestä [StorSimple tilannevedoksen hallinta ammattimainen StorSimple ratkaisu](storsimple-snapshot-manager-admin.md).
- Lisätietoja käyttämisestä [StorSimple tilannevedoksen hallinta voit tarkastella ja asemat hallinta](storsimple-snapshot-manager-manage-volumes.md).

