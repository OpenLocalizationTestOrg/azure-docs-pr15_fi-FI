<properties 
   pageTitle="StorSimple tilannevedoksen hallinnan MMC valikko toiminnot | Microsoft Azure"
   description="Tässä artikkelissa käsitellään käyttää vakio Microsoft Management Console (MMC)-valikosta toimintoja StorSimple tilannevedoksen hallinta."
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
   ms.date="04/25/2016"
   ms.author="v-sharos" />

# <a name="use-the-mmc-menu-actions-in-storsimple-snapshot-manager"></a>Käytä MMC valikko toiminnot StorSimple tilannevedoksen hallinta

## <a name="overview"></a>Yleiskatsaus

StorSimple tilannevedoksen hallinta-luettelossa kaikki toiminnon valikot ja kaikki **Toiminnot** -ruudusta variaatioita seuraavat toimet tulee näkyviin. 

- Näytä
- Tässä uudessa ikkunassa 
- Päivittäminen 
- Luettelon vieminen 
- Apua 

Nämä toiminnot ovat osa Microsoft Management Console (MMC) ja eivät ole vain StorSimple tilannevedoksen hallinnassa. Tässä opetusohjelmassa kuvataan nämä toiminnot ja kerrotaan, miten voit käyttää niitä eri StorSimple tilannevedoksen hallinta.

## <a name="view"></a>Näytä

Voit käyttää **asetus** , voit vaihtaa näkymää **tulokset** -ruudussa ja muuta console-ikkunan Näytä. 

#### <a name="to-change-the-results-pane-view"></a>Voit vaihtaa näkymää tulokset-ruutu

1. Napsauta työpöydän StorSimple tilannevedoksen hallinnan käynnistäminen kuvaketta.

2. **Laajuus** -ruudussa hiiren kakkospainikkeella mitä tahansa solmu tai Laajenna-solmu ja **tulosruudussa** kohdetta hiiren kakkospainikkeella ja valitse sitten **asetus** . 

3. Voit lisätä tai poistaa sarakkeita, jotka näkyvät **tulosruudussa** , valitse **Lisää tai Poista sarakkeet**. **Lisää tai Poista sarakkeet** -valintaikkuna tulee näkyviin.

    ![Lisää tai poista sarakkeita tulokset-ruudusta](./media/storsimple-snapshot-manager-mmc-menu/HCS_SSM_Add_remove_columns.png) 

4. Täytä lomake seuraavasti:

    - Valitse kohteiden **käytettävissä olevat** sarakkeet-luettelossa ja valitse **Lisää** ja lisää hänet **Näytettävät sarakkeet** -luettelosta. 

    - Valitse **Näytettävät sarakkeet** -luettelossa olevat kohteet ja valitse **Poista** , jos haluat poistaa luettelossa. 

    - Valitse **Näytettävät** sarakkeet-luettelosta haluamasi vaihtoehto ja valitse **Siirrä ylös** tai **Siirrä alas** Siirrä osa ylös tai alas luettelossa. 

    - Valitse työnkulkuun oletusarvon **tulokset** ruudun palauttaa **Palauta oletusarvot** . 

5. Kun olet valmis valintasi, valitse **OK**. 

#### <a name="to-change-the-console-window-view"></a>Voit vaihtaa näkymää console-ikkuna

1. Napsauta työpöydän StorSimple tilannevedoksen hallinnan käynnistäminen kuvaketta.

2. **Laajuus** -ruudussa hiiren kakkospainikkeella mitä tahansa solmu, valitse **Näytä**ja valitse sitten **Mukauta**. **Mukauta** -valintaikkuna tulee näkyviin.

    ![Console-ikkunan mukauttaminen](./media/storsimple-snapshot-manager-mmc-menu/HCS_SSM_Customize.png) 

3. Valitse tai poista valintaruutujen valinta, jos haluat näyttää tai piilottaa konsoli-ikkunan osat. Kun olet valmis valintasi, valitse **OK**.

## <a name="new-window-from-here"></a>Tässä uudessa ikkunassa

**Uusi ikkuna tästä** -vaihtoehdon avulla voit avata uuden konsoli-ikkunan.

#### <a name="to-open-a-new-console-window"></a>Avaa uusi konsoli-ikkuna

1. Napsauta työpöydän StorSimple tilannevedoksen hallinnan käynnistäminen kuvaketta.

2. **Laajuus** -ruudussa hiiren kakkospainikkeella mitä tahansa solmu ja valitse sitten **Uusi ikkuna tästä**. 

    Näkyviin tulee uusi ikkuna, näkyy vain laajuuden, että olet valinnut. Esimerkiksi jos **Varmuuskopioinnin käytännöt** -solmun hiiren kakkospainikkeella, uudessa ikkunassa näkyy vain **Varmuuskopion käytännöt** -solmun **laajuus** -ruutu ja varmuuskopion käytäntöjä **tulosruudussa** luettelo. Esimerkki.

    ![Tässä uudessa ikkunassa](./media/storsimple-snapshot-manager-mmc-menu/HCS_SSM_NewWindow.png) 
 
## <a name="refresh"></a>Päivittäminen

**Päivitä** -toiminnon avulla voit päivittää konsoli-ikkuna.

#### <a name="to-update-the-console-window"></a>Jos haluat päivittää konsoli-ikkuna

1. Napsauta työpöydän StorSimple tilannevedoksen hallinnan käynnistäminen kuvaketta.

2. **Laajuus** -ruudussa hiiren kakkospainikkeella mitä tahansa solmu tai Laajenna-solmu ja **tulosruudussa** kohdetta hiiren kakkospainikkeella ja valitsemalla sitten **Päivitä**. 

## <a name="export-list"></a>Luettelon vieminen

**Vie luettelo** -toiminnon avulla voit tallentaa luettelon CSV (CSV). Voit viedä esimerkiksi varmuuskopioinnin käytännöt tai varmuuskopion luettelon luettelo. Voit tuoda CSV-tiedoston sitten laskentataulukkosovellusta analyysia varten.

#### <a name="to-save-a-list-in-a-comma-separated-value-csv-file"></a>Voit tallentaa luettelon CSV (CSV)-tiedosto

1. Napsauta työpöydän StorSimple tilannevedoksen hallinnan käynnistäminen kuvaketta. 

2. **Laajuus** -ruudussa hiiren kakkospainikkeella mitä tahansa solmu tai Laajenna-solmu ja **tulosruudussa** kohdetta hiiren kakkospainikkeella ja valitse sitten **Vie luettelo**. 

3. **Vie luettelo** -valintaikkuna. Täytä lomake seuraavasti: 

    1. CSV-tiedoston nimi **Tiedostonimi** -ruutuun tai valitse olevaa nuolta ja valitse avattavasta luettelosta.

    2. **Tallennusmuoto** -ruudussa olevaa nuolta ja valitse tyyppi avattavasta luettelosta.

    3. Jos haluat tallentaa vain valitut kohteet, valitse muutettavat rivit ja valitse sitten **Tallenna vain valitut rivit** -valintaruutu. Jos haluat tallentaa kaikki viedyt luettelot, poista **Tallenna vain valitut rivit** -valintaruudun valinta.

    4. Valitse **Tallenna**.

    ![Luettelon vieminen CSV-tiedostona](./media/storsimple-snapshot-manager-mmc-menu/HCS_SSM_Export_List.png) 
 
## <a name="help"></a>Apua

**Ohje** -valikosta avulla voit tarkastella käytettävissä online-ohje StorSimple tilannevedoksen johtajan ja MMC: HEN.

#### <a name="to-view-available-online-help"></a>Voit tarkastella käytettävissä online-ohje

1. Napsauta työpöydän StorSimple tilannevedoksen hallinnan käynnistäminen kuvaketta.

2. **Laajuus** -ruudussa hiiren kakkospainikkeella mitä tahansa solmu tai Laajenna-solmu ja **tulosruudussa** kohdetta hiiren kakkospainikkeella ja valitsemalla sitten **Ohje**. 

## <a name="next-steps"></a>Seuraavat vaiheet

- Lisätietoja [StorSimple tilannevedoksen hallinnan käyttöliittymä](storsimple-use-snapshot-manager.md).
- Lisätietoja [StorSimple tilannevedoksen hallinnan ammattimainen StorSimple-ratkaisun avulla](storsimple-snapshot-manager-admin.md).
