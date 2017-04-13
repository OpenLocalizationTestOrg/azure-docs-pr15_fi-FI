<properties 
   pageTitle="Ottaa käyttöön StorSimple tilannevedoksen hallinta | Microsoft Azure"
   description="Lue, miten voit ladata ja asentaa StorSimple tilannevedoksen hallinta, MMC laajennuksen StorSimple tietojen suojauksen ja varmuuskopiointi ominaisuuksien hallinnasta."
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
   ms.date="05/24/2016"
   ms.author="v-sharos" />

# <a name="deploy-the-storsimple-snapshot-manager-mmc-snap-in"></a>StorSimple tilannevedoksen hallinnan MMC ottaa

## <a name="overview"></a>Yleiskatsaus

StorSimple tilannevedoksen hallinta on Microsoft Management Console (MMC)-laajennus, joka helpottaa tietojen suojaaminen ja varmuuskopion hallinta Microsoft Azure StorSimple ympäristössä. StorSimple tilannevedoksen hallinnan avulla voit hallita Microsoft Azure StorSimple paikallisen ja cloud tallennustilan kuin, jos se on täysin integroitu tallennustilan-järjestelmä näin yksinkertaistaminen varmuuskopioiminen ja palauttaminen ja pienentää kustannuksia. 

Tässä opetusohjelmassa kuvataan määritykset-vaatimusten sekä ohjeet asentaminen ja poistaminen päivittäminen StorSimple tilannevedoksen hallinta.

>[AZURE.NOTE] 
>
>- StorSimple tilannevedoksen hallinnan avulla et voi hallita Microsoft Azure StorSimple Virtual matriisien (tunnetaan myös nimellä StorSimple paikallisen virtual laitteet).
>
>- Jos aiot asentaa StorSimple Update 2 StorSimple laitteen, muista ladata uusimman version StorSimple tilannevedoksen hallinta ja asenna se **ennen kuin asennat StorSimple päivityksen 2**. Uusimman version StorSimple tilannevedoksen hallinta on yhteensopiva aiempien versioiden ja kaikkien julkaistun versioiden Microsoft Azure StorSimple toimii. Jos käytössäsi on aiemmasta versiosta StorSimple tilannevedoksen hallinta, sinun on päivittää sen (sinun ei tarvitse poistaa edellisen version ennen uuden version asentamista).

## <a name="storsimple-snapshot-manager-installation"></a>Asennuksen StorSimple tilannevedoksen hallinta

StorSimple tilannevedoksen hallinta voidaan asentaa tietokoneissa, joissa on käytössä Windows Server 2008 R2 SP1, Windows Server 2012: ssa tai Windows Server 2012 R2-käyttöjärjestelmä. Windows 2008 R2-palvelimissa on asennettava myös Windows Server 2008: n SP1 ja Windows Management Framework 3.0. 

Ennen kuin asennat tai päivität StorSimple tilannevedoksen hallinta-laajennus Microsoft Management Console (MMC), varmista, että Microsoft Azure StorSimple laitteen ja host palvelin on määritetty oikein. 

## <a name="configure-prerequisites"></a>Määritä edellytykset

Seuraavat vaiheet on yleiskatsaus määritystehtävät, sinun on suoritettava ennen kuin asennat StorSimple tilannevedoksen hallinta. Valmis Microsoft Azure StorSimple määritys ja määrittämisestä on artikkelissa, mukaan lukien järjestelmävaatimukset ja vaiheittaiset ohjeet ovat artikkelissa [paikallisen StorSimple laitteen käyttöönotto](storsimple-deployment-walkthrough.md).

>[AZURE.IMPORTANT]Ennen kuin aloitat, tarkista [käyttöönoton määrityksen tarkistusluettelo](storsimple-deployment-walkthrough.md#deployment-configuration-checklist) ja ja [käyttöönotto paikallisen StorSimple laitteen](storsimple-deployment-walkthrough.md) [käyttöönoton edellytykset](storsimple-deployment-walkthrough.md#deployment-prerequisites) .
> <br>
 
### <a name="before-you-install-storsimple-snapshot-manager"></a>Ennen kuin asennat StorSimple tilannevedoksen hallinta

1. Pura, ottaa käyttöön ja muodosta Microsoft Azure StorSimple laitteen kuvatulla tavalla [Asenna StorSimple 8100 laitteen](storsimple-8100-hardware-installation.md) tai [Asenna StorSimple 8600 laitteen](storsimple-8600-hardware-installation.md).

2. Varmista, että host tietokone toimii jokin seuraavista:

    - Windows Server 2008 R2 (käytössä palvelimessa on Windows 2008 R2, sinun on asennettava myös Windows Server 2008: n SP1 ja Windows Management Framework 3.0)
    - Windows Server 2012
    - Windows Server 2012 R2
 
    StorSimple: laitteen virtual isäntä on oltava Microsoft Azure Virtual-Machine. 

3. Varmista, että kaikki Microsoft Azure StorSimple määritysten vaatimukset täyttyvät. Lisätietoja on Siirry [käyttöönoton edellytykset](storsimple-deployment-walkthrough.md#deployment-prerequisites).

4. Laitteen yhdistäminen isännän ja suorittaa sen ensimmäisen määrityksen. Lisätietoja on Siirry [käyttöönoton vaiheet](storsimple-deployment-walkthrough.md#deployment-steps).

5. Luoda tietomääristä laitteeseen varaaminen isännän ja varmista, että isäntä voit ottaa käyttöön ja käyttää niitä. StorSimple tilannevedoksen hallinta tukee tietomääristä seuraavanlaisia: 

    - Tavallinen tietomääristä
    - Tavalliset asemat
    - Dynaamiset asemat
    - Peilattu dynaamiset asemat (RAID-1)
    - Klusterin jaettu tietomääristä
 
    Lisätietoja luomisesta tietomääristä StorSimple laitteen tai StorSimple virtual laitteessa, siirry [Vaihe 6: aseman luominen](storsimple-deployment-walkthrough.md#step-6-create-a-volume),- [käyttöönotto paikallisen StorSimple laitteen](storsimple-deployment-walkthrough.md).

## <a name="install-a-new-storsimple-snapshot-manager"></a>Asenna uusi StorSimple tilannevedoksen hallinta

Ennen kuin asennat StorSimple tilannevedoksen hallinta, varmista, että asemat luomasi StorSimple laitteessa tai StorSimple, virtual laitteen otettu käyttöön, alustaa ja [Määritä edellytykset](#configure-prerequisites)kuvatun.

>[AZURE.IMPORTANT]
>
>- StorSimple: laitteen virtual isäntä on oltava Microsoft Azure Virtual-Machine. 
>
>- Isännän on oltava käynnissä, Windows 2008 R2, Windows Server 2012: ssa tai Windows Server 2012 R2. Jos palvelin on käynnissä Windows Server 2008 R2, sinun on asennettava myös Windows Server 2008: n SP1 ja Windows Management Framework 3.0.
>
>- Sinun on määritettävä iSCSI-yhteyden isännältä StorSimple laitteeseen, ennen kuin voit muodostaa laitteen StorSimple tilannevedoksen hallinta.

Viimeistele uuden asennuksen StorSimple tilannevedoksen hallinta seuraavasti. Jos asennat päivityksen, siirry [päivittäminen tai uudelleen StorSimple tilannevedoksen hallinta](#upgrade-or-reinstall-storsimple-snapshot-manager).

- Vaihe 1: Asenna StorSimple tilannevedoksen hallinta 
- Vaihe 2: Yhdistäminen StorSimple tilannevedoksen hallinta-laitteeseen 
- Vaihe 3: Vahvista laitteen yhteys 

###<a name="step-1-install-storsimple-snapshot-manager"></a>Vaihe 1: Asenna StorSimple tilannevedoksen hallinta

Seuraavien vaiheiden avulla voit asentaa StorSimple tilannevedoksen hallinta.

#### <a name="to-install-storsimple-snapshot-manager"></a>Asenna StorSimple tilannevedoksen hallinta

1. Lataa StorSimple tilannevedoksen hallinta-ohjelmiston (Siirry Microsoft Download Centeristä [StorSimple tilannevedoksen hallinta](https://www.microsoft.com/download/details.aspx?id=44220) ) ja tallenna se paikallisesti isännän.

2. Resurssienhallinnassa Napsauta hiiren kakkospainikkeella pakattu kansio ja valitse sitten **Pura kaikki**.

3. Kirjoita **Pura pakattu (Zipped) kansiot** -ikkunassa **Valitse kohde ja Pura tiedostot** -ruutuun tai polku, johon haluat purkaa tiedoston selaamalla. 

       >[AZURE.IMPORTANT] Asenna c-asema StorSimple tilannevedoksen hallinta.
 
4. Valitse **Näytä poimitun tiedostojen jälkeen** -valintaruutu ja valitse sitten **Pura**.

    ![Pura tiedostot-valintaikkuna](./media/storsimple-snapshot-manager-deployment/HCS_SSM_extract_files.png) 

4. Kun purkaminen on valmis, Avaa kohdekansioon. Kaksoisnapsauta sovelluksen asetukset-kuvaketta, joka näkyy kohdekansioon.

5. Kun **Asennus onnistu** sanoma tulee näkyviin, valitse **Sulje**. StorSimple tilannevedoksen hallinta-kuvake näkyy työpöydällä.

    ![työpöydän kuvakkeesta](./media/storsimple-snapshot-manager-deployment/HCS_SSM_desktop_icon.png) 

### <a name="step-2-connect-storsimple-snapshot-manager-to-a-device"></a>Vaihe 2: Yhdistäminen StorSimple tilannevedoksen hallinta-laitteeseen

Seuraavien vaiheiden avulla voit muodostaa yhteyden StorSimple laitteeseen StorSimple tilannevedoksen hallinta.

#### <a name="to-connect-storsimple-snapshot-manager-to-a-device"></a>Muodosta yhteys laitteen StorSimple tilannevedoksen hallinta

1. Napsauta työpöydän StorSimple tilannevedoksen hallinta-kuvaketta. StorSimple tilannevedoksen hallinta-ikkuna. Ikkunassa on **laajuus** -ruudussa, **tulokset** -ruutu ja **Toiminnot** -ruudusta. 

    ![StorSimple tilannevedoksen hallinnan käyttöliittymä](./media/storsimple-snapshot-manager-deployment/HCS_SSM_gui_panes.png) 

    - **Laajuus** -ruudussa (vasemmanpuoleisessa ruudussa) on järjestetty puurakenteeksi, solmujen luettelo. Voit laajentaa joidenkin solmujen Valitse näkymä tai solmun liittyvät tiedot. Valitse laajentaminen tai kutistaminen solmu napsauttamalla nuolta. Napsauta hiiren kakkospainikkeella kyseiselle kohteelle käytettävissä olevien toimintojen luettelo **laajuus** -ruudussa. 

    - **Tulokset** -ruudussa (keskimmäisessä ruudussa) sisältää yksityiskohtaiset tilatietoja solmu, näkymän tai tiedot, jotka valitsit **laajuus** -ruudussa.

    - **Toiminnot** -ruudusta luettelo toiminnoista, joita voit suorittaa solmu, näkymän tai tiedot, jotka valitsit **laajuus** -ruudussa.

    Kuvaus StorSimple tilannevedoksen hallinnan käyttöliittymä on artikkelissa [StorSimple tilannevedoksen hallinnan käyttöliittymä](storsimple-use-snapshot-manager.md).

2. **Laajuus** -ruudussa **laitteet** -solmun hiiren kakkospainikkeella ja valitse sitten **Määritä laite**. **Laitteen määrittäminen** -valintaikkuna.

    ![Laitteen määrittäminen](./media/storsimple-snapshot-manager-deployment/HCS_SSM_config_device.png) 

3. Valitse **laite** -luetteloruudussa IP-osoite Microsoft Azure StorSimple tai virtual avulla. Kirjoita **salasana** -ruutuun StorSimple tilannevedoksen hallinta salasanan, jonka loit laitteen Azure perinteinen-portaalissa. Valitse **OK**.

4. StorSimple tilannevedoksen hallinta etsii laitteella, joka tunnistanut. Jos laite on käytettävissä, StorSimple tilannevedoksen hallinta Lisää yhteys. Voit vahvistaa, että yhteys on lisätty [Varmista yhteys laitteeseen](#to-verify-the-connection) .

    Jos laite ei ole käytettävissä mistä tahansa syystä, StorSimple tilannevedoksen hallinta palauttaa virhesanoman. Valitse **OK** ja sulje virhesanoma ja valitse sitten Sulje **Määritä laite** -valintaikkunassa **Peruuta** .

5. Kun se yhdistää laitteen ja StorSimple tilannevedoksen hallinta tuo määritetty kyseisen laitteen äänenvoimakkuuden kunkin ryhmän, äänenvoimakkuuden ryhmän liittyy varmuuskopiot. Äänenvoimakkuuden ryhmät, joihin ei ole liitetty varmuuskopioiden ei tuoda. Lisäksi varmuuskopion käytäntöjä, jotka on luotu aseman ryhmän ei tuoda. Voit tarkastella tuotuja ryhmiä, napsauttamalla sitä hiiren kakkospainikkeella ylimmän **Aseman ryhmät** -solmun **laajuus** -ruudussa ja valitsemalla **Näytä tai piilota tuotu ryhmät**.

### <a name="step-3-verify-the-connection-to-the-device"></a>Vaihe 3: Vahvista laitteen yhteys

Seuraavien vaiheiden avulla voit Tarkista, että StorSimple laite on yhdistetty StorSimple tilannevedoksen hallinta.

#### <a name="to-verify-the-connection"></a>Tarkista yhteys

1. Napsauta **laitteet** -solmun **laajuus** -ruudussa.

    ![StorSimple tilannevedoksen hallinta laitteen tila](./media/storsimple-snapshot-manager-deployment/HCS_SSM_Device_status.png) 

2. Tarkista **tulokset** -ruudussa: 

   - Jos **käytettävissä** **tila** -sarakkeessa näkyy vihreä ilmaisin näkyy laite-kuvaketta, laite on yhdistetty. 

   - Jos laite-kuvake näkyy punainen ilmaisin ja **tila** -sarakkeessa näkyy ei käytettävissä, laite ei ole yhteydessä. 

   - Jos **tila** -sarakkeessa näkyy **päivittäminen** , on haetaan StorSimple tilannevedoksen hallinta aseman ryhmät ja yhdistetyn laitteen liittyvät varmuuskopiot.

## <a name="upgrade-or-reinstall-storsimple-snapshot-manager"></a>Päivitä tai asenna StorSimple tilannevedoksen hallinta

StorSimple tilannevedoksen hallinta on poistaa kokonaan, ennen kuin voit päivittää tai asentaa ohjelmiston uudelleen. 

Ennen uudelleenasentaminen StorSimple tilannevedoksen hallinnan takaisin isäntätietokoneen aiemmin StorSimple tilannevedoksen Manager-tietokanta. Tämä tallentaa varmuuskopion käytännöt- ja määrityspalveluja tiedot niin, että voit palauttaa tiedot helposti varmuuskopiosta.

Noudattamalla seuraavia ohjeita, jos päivität tai uudelleenasentaminen StorSimple tilannevedoksen hallinta:

- Vaihe 1: Poista StorSimple tilannevedoksen hallinta 
- Vaihe 2: StorSimple tilannevedoksen Manager-tietokannan varmuuskopioiminen 
- Vaihe 3: Asenna StorSimple tilannevedoksen hallinta ja tietokannan palauttaminen 

### <a name="step-1-uninstall-storsimple-snapshot-manager"></a>Vaihe 1: Poista StorSimple tilannevedoksen hallinta

Seuraavien vaiheiden avulla voit poistaa StorSimple tilannevedoksen hallinta.

#### <a name="to-uninstall-storsimple-snapshot-manager"></a>Jos haluat poistaa StorSimple tilannevedoksen hallinta

1. Host (isäntä)-tietokonetta Avaa **Ohjauspaneeli**, valitse **Ohjelmat**ja valitse **Ohjelmat ja toiminnot**.

2. Valitse vasemmanpuoleisessa ruudussa **Muuta sovellusta tai poista se**.

3. Napsauta hiiren kakkospainikkeella **StorSimple tilannevedoksen hallinta**ja valitse sitten **Poista**.

4. Tämä käynnistää StorSimple tilannevedoksen Managerin asennusohjelma. Valitse **Muokkaa asetukset**ja valitse sitten **Poista**.

    >[AZURE.NOTE] Jos luettelossa on käynnissä taustalla MMC prosessit, kuten StorSimple tilannevedoksen hallinta tai Disk Management asennuksen poistaminen ei onnistu ja näyttöön tulee sanoma, sulje MMC kaikki esiintymät, ennen kuin yrität Poista ohjelman asennus. Valitse **Sulje automaattisesti sovellukset ja yritä käynnistää ne asennuksen päätyttyä uudelleen**ja valitse sitten **OK**.
 
5. Asennuksen päätyttyä, **Asennus onnistu** sanoma tulee näkyviin. Valitse **Sulje**.

### <a name="step-2-back-up-the-storsimple-snapshot-manager-database"></a>Vaihe 2: StorSimple tilannevedoksen Manager-tietokannan varmuuskopioiminen

Seuraavien vaiheiden avulla voit luoda ja tallentaa kopion StorSimple tilannevedoksen Manager-tietokanta.

#### <a name="to-back-up-the-database"></a>Tietokannan varmuuskopioiminen

1. Microsoft StorSimple Management-palvelun pysäyttäminen:

   1. Aloita Serverin hallinta.

   2. Palvelimen hallinnan koontinäytössä **Työkalut** -valikon Valitse **palvelut**.

   3. Valitse **palvelut** -sivulla **Microsoft StorSimple hallintapalvelun**.

   4. Valitse oikeanpuoleisen ruudun **Microsoft StorSimple hallintapalvelun**- **palvelun pysäyttäminen**.

        ![StorSimple hallinta-palvelun pysäyttäminen](./media/storsimple-snapshot-manager-deployment/HCS_SSM_stop_service.png)

2. Siirry C:\ProgramData\Microsoft\StorSimple\BACatalog. 

    >[AZURE.NOTE] ProgramData on piilotettu kansio.

3. Etsi luettelon XML-tiedosto, kopioi tiedosto ja Tallenna kopio turvallisessa paikassa tai pilveen.

    ![StorSimple varmuuskopiotiedoston tiedosto](./media/storsimple-snapshot-manager-deployment/HCS_SSM_bacatalog.png)

4. Käynnistä Microsoft StorSimple Management-palvelu: 

    1. Palvelimen hallinnan koontinäytössä **Työkalut** -valikon Valitse **palvelut**.

    2. Valitse **palvelut** -sivulla **Microsoft StorSimple hallinnan Servic**e.

    3. Valitse oikeanpuoleisen ruudun **Microsoft StorSimple hallintapalvelun**- **palvelun käynnistäminen uudelleen**. 

### <a name="step-3-reinstall-storsimple-snapshot-manager-and-restore-the-database"></a>Vaihe 3: Asenna StorSimple tilannevedoksen hallinta ja tietokannan palauttaminen

StorSimple tilannevedoksen hallinta uudelleen noudattamalla ohjeita [Asenna uusi StorSimple tilannevedoksen hallinta](#install-a-new-storsimple-snapshot-manager). Tämän jälkeen StorSimple tilannevedoksen Manager-tietokannan palauttaminen seuraavien ohjeiden avulla.

#### <a name="to-restore-the-database"></a>Palauta tietokanta

1. Microsoft StorSimple Management-palvelun pysäyttäminen:

    1. Aloita Serverin hallinta.

    2. Palvelimen hallinnan koontinäytössä **Työkalut** -valikon Valitse **palvelut**.

    3. Valitse **palvelut** -sivulla **Microsoft StorSimple hallintapalvelun**.

    4. Valitse oikeanpuoleisen ruudun **Microsoft StorSimple hallintapalvelun**- **palvelun pysäyttäminen**.

2. Siirry C:\ProgramData\Microsoft\StorSimple\BACatalog. 

     >[AZURE.NOTE] ProgramData on piilotettu kansio.

3. Poista luettelon XML-tiedosto ja korvaa se aiemmin tallentamasi versio.

4. Käynnistä Microsoft StorSimple Management-palvelu: 

    1. Palvelimen hallinnan koontinäytössä **Työkalut** -valikon Valitse **palvelut**.

    2. Valitse **palvelut** -sivulla **Microsoft StorSimple hallintapalvelun**.

    3. Valitse oikeanpuoleisen ruudun **Microsoft StorSimple hallintapalvelun**- **palvelun käynnistäminen uudelleen**.

## <a name="next-steps"></a>Seuraavat vaiheet

- Katso lisätietoja StorSimple tilannevedoksen Managerin Siirry [Mikä on StorSimple tilannevedoksen hallinta?](storsimple-what-is-snapshot-manager.md).

- Lisätietoja StorSimple tilannevedoksen hallinnan-käyttöliittymä, siirry [StorSimple tilannevedoksen hallinnan käyttöliittymä](storsimple-use-snapshot-manager.md).

- Lisätietoja StorSimple tilannevedoksen hallinnan avulla, siirry [StorSimple tilannevedoksen hallinnan käyttö ammattimainen StorSimple ratkaisu](storsimple-snapshot-manager-admin.md).
