<properties 
   pageTitle="StorSimple tilannevedoksen hallinta aseman ryhmät | Microsoft Azure"
   description="Tässä artikkelissa käsitellään luoda ja hallita äänenvoimakkuuden StorSimple tilannevedoksen hallinnan MMC-laajennuksen avulla."
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

# <a name="use-storsimple-snapshot-manager-to-create-and-manage-volume-groups"></a>Luoda ja hallita äänenvoimakkuuden StorSimple tilannevedoksen hallinnan avulla

## <a name="overview"></a>Yleiskatsaus

Voit käyttää **Aseman ryhmät** -solmun **laajuus** -ruutu määrittää tietomääristä aseman ryhmiin, tarkastella aseman ryhmän tietoja, varmuuskopioinnin ja muokata asemaa ryhmiä. 

Avauksen ja vaihdon ryhmät ovat liittyvät asemat varmistaa varmuuskopiot ovat yhdenmukaisia sovelluksen jakavat. Lisätietoja on artikkelissa [asemat ja aseman ryhmät](storsimple-what-is-snapshot-manager.md#volumes-and-volume-groups) ja [Windowsin tilannevedospalvelu integrointi](storsimple-what-is-snapshot-manager.md#integration-with-windows-volume-shadow-copy-service).

>[AZURE.IMPORTANT] 
>
> * Kaikki asemat aseman ryhmän sijaittava yksittäisen cloud-palveluntarjoajalta.
> 
> * Kun määrität aseman ryhmät, ole yhdistelmiä klusterin jaetut asemat (CSVs) ja saman aseman ryhmän ei CSVs. StorSimple tilannevedoksen hallinta ei tue CSVs sekä CSVs saman tilannevedoksen.
 
![Avauksen ja vaihdon ryhmät solmu](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_Volume_groups.png)

**Kuva 1: StorSimple tilannevedoksen hallinta aseman ryhmät solmu** 

Tässä opetusohjelmassa kerrotaan StorSimple tilannevedoksen hallinnan käyttämisestä:

- Äänenvoimakkuuden ryhmien tietojen tarkasteleminen 
- Avauksen ja vaihdon ryhmän luominen
- Äänenvoimakkuuden ryhmän varmuuskopiointi
- Avauksen ja vaihdon ryhmän muokkaaminen
- Avauksen ja vaihdon ryhmän poistaminen

Kaikki nämä toiminnot ovat käytettävissä myös **Toiminnot** -ruudussa.
 
## <a name="view-volume-groups"></a>Avauksen ja vaihdon ryhmien tarkasteleminen

Jos valitset **Aseman ryhmät** -solmun, **tulokset** -ruudussa näkyy aseman kunkin ryhmän sarakkeen valintojen mukaan seuraavat tiedot. ( **Tulosruudussa** sarakkeet ovat määritettävissä. Napsauta hiiren kakkospainikkeella **asemat** -solmu, valitsemalla **Näytä**ja valitse sitten **Lisää tai Poista sarakkeet**.)

Tulokset-sarakkeessa | Kuvaus 
:--------------|:------------ 
Nimi           | **Nimi** -sarakkeessa äänenvoimakkuutta-ryhmän nimi.
Sovelluksen    | **Sovellukset** -sarakkeessa näkyy VSS kirjoittajat tällä hetkellä asennettu ja käytössä Windows-isännän määrä.
Valittu       | **Valittu** sarake näyttää asemat, jotka sisältyvät määrän äänenvoimakkuutta-ryhmässä. Nolla (0) ilmaisee, että sovellusta ei ole liittyy tietomääristä äänenvoimakkuutta-ryhmässä.
Tuotu       | **Tuotu** -sarakkeessa on tuotu tietomääristä määrä. Jos määritetty **Tosi**, tässä sarakkeessa ilmaisee, että aseman ryhmän on tuotu perinteinen Azure-portaalista ei ole luotu StorSimple tilannevedoksen hallinta.
 
>[AZURE.NOTE] StorSimple tilannevedoksen hallinta aseman ryhmät näkyvät myös Azure perinteinen portaalissa **Varmuuskopiointi käytännöt** -välilehdessä.
 
## <a name="create-a-volume-group"></a>Avauksen ja vaihdon ryhmän luominen

Seuraavien ohjeiden avulla voit luoda aseman ryhmän.

#### <a name="to-create-a-volume-group"></a>Avauksen ja vaihdon ryhmän luominen

1. Napsauta työpöydän StorSimple tilannevedoksen hallinnan käynnistäminen kuvaketta. 

2. **Laajuus** -ruudussa hiiren kakkospainikkeella **Aseman ryhmät**ja valitse sitten **Luo aseman ryhmä**. 

    ![Avauksen ja vaihdon ryhmän luominen](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_Create_volume_group.png)
 
    **Avauksen ja vaihdon ryhmän luominen** -valintaikkuna. 

    ![Luo aseman ryhmä-valintaikkuna](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_CreateVolumeGroup_dialog.png) 

3.  Anna seuraavat tiedot: 

    1. Kirjoita **nimi** -ruutuun uuden aseman ryhmän yksilöllinen nimi. 

    2. Valitse **sovellukset** -ruudussa asemat, jota olet lisäämässä aseman ryhmään liittyvät sovellukset. 

        Vain sovellukset, jotka käyttävät StorSimple asemat ja VSS kirjoittajat on otettu käyttöön niiden **sovellukset** -ruutuun luettelot. VSS writer on käytössä vain, jos kaikki asemat, että kirjoittajan tuntee StorSimple asemat. Jos sovelluksia-ruutu on tyhjä, ei ole sovellukset, jotka käyttävät Azure StorSimple asemat ja on tuettu VSS kirjoittajat asennetaan. (Tällä hetkellä Azure StorSimple tukee Microsoft Exchange- ja SQL Server.) Saat lisätietoja VSS kirjoittajat [Windows tilannevedospalvelu integrointi](storsimple-what-is-snapshot-manager.md#integration-with-windows-volume-shadow-copy-service).

        Jos valitset sovelluksen, kaikki siihen liittyvät asemat valitaan automaattisesti. Vastaavasti jos valitset tietyn sovelluksen liittyvät asemat-sovellus on automaattisesti valittuna **sovellukset** -ruutuun. 

    3. Valitse **asemat** -ruudussa StorSimple tietomääristä Lisää äänenvoimakkuutta-ryhmään. 

      - Voit lisätä yksittäisiä tai useita osioita asemat. (Useita osion asemat voidaan dynaamisiksi tai basic levyjä useita osioita.) Asema, joka sisältää useita osioita käsitellään yhtenä yksikkönä. Näin ollen jos lisäät vain yksi osiot aseman ryhmään, kaikki muut osiot lisätään automaattisesti aseman ryhmään samanaikaisesti. Kun olet lisännyt useita osion aseman äänenvoimakkuuden ryhmään, useita osion äänenvoimakkuuden säilyy käsitellään yhtenä yksikkönä.

      - Voit luoda tyhjän aseman ryhmiä määrittämällä kaikki asemat niihin ei ole. 

      - Klusterin jaetut asemat (CSVs) ja saman aseman ryhmän ei CSVs ole yhdistelmiä. StorSimple tilannevedoksen hallinta ei tue CSV asemat sekä-CSV asemat samaan tilannevedoksen. 

4. Valitse **OK** äänenvoimakkuutta-ryhmästä Tallenna.

## <a name="back-up-a-volume-group"></a>Äänenvoimakkuuden ryhmän varmuuskopiointi

Noudattamalla seuraavia vaiheita voit varmuuskopioida aseman ryhmän.

#### <a name="to-back-up-a-volume-group"></a>Voit varmuuskopioida äänenvoimakkuutta-ryhmä

1. Napsauta työpöydän StorSimple tilannevedoksen hallinnan käynnistäminen kuvaketta.

2. **Laajuus** -ruudussa Laajenna **Aseman ryhmät** -solmu, äänenvoimakkuuden ryhmän nimeä hiiren kakkospainikkeella ja valitse sitten **Ota varmuuskopiointi**. 

    ![Varmuuskopioi aseman ryhmän heti](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_Take_backup.png)

3. **Ota varmuuskopio** -valintaikkunassa valitse **Paikallisen tilannevedoksen** tai **Cloud tilannevedoksen**ja valitse sitten **Luo**. 

    ![Ota varmuuskopio-valintaikkunassa](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_TakeBackup_dialog.png) 

4. Varmista, että varmuuskopiointi on käynnissä, laajenna **Projektit** -solmu ja valitse sitten **käynnissä**. Varmuuskopion luettelossa pitäisi näkyä.

5. Näytä valmiit tilannevedoksen, **Varmuuskopioinnin luettelo** -solmu, ryhmänimen ja valitsemalla **Paikallisen tilannevedoksen** tai **Cloud tilannevedoksen**. Varmuuskopion näkyvät, jos se valmis. 

## <a name="edit-a-volume-group"></a>Avauksen ja vaihdon ryhmän muokkaaminen

Seuraavien ohjeiden avulla voit muokata asemaa ryhmän.

#### <a name="to-edit-a-volume-group"></a>Voit muokata asemaa ryhmän

1. Napsauta työpöydän StorSimple tilannevedoksen hallinnan käynnistäminen kuvaketta.

2. **Laajuus** -ruudussa Laajenna **Aseman ryhmät** -solmu, äänenvoimakkuuden ryhmän nimeä hiiren kakkospainikkeella ja valitse sitten **Muokkaa**. 

3. **Avauksen ja vaihdon ryhmän luominen **-valintaikkuna. Voit muuttaa **nimen**, **sovellusten**ja **asemat** -tapahtumat. 

4. Valitse **OK** , Tallenna muutokset.

## <a name="delete-a-volume-group"></a>Avauksen ja vaihdon ryhmän poistaminen

Seuraavien ohjeiden avulla voit poistaa aseman ryhmän. 

>[AZURE.WARNING] Tämä poistaa myös kaikki aseman ryhmään liittyvien varmuuskopiot.

#### <a name="to-delete-a-volume-group"></a>Avauksen ja vaihdon ryhmän poistaminen

1. Napsauta työpöydän StorSimple tilannevedoksen hallinnan käynnistäminen kuvaketta. 

2. **Laajuus** -ruudussa Laajenna **Aseman ryhmät** -solmu, äänenvoimakkuuden ryhmän nimeä hiiren kakkospainikkeella ja valitse sitten **Poista**. 

3. **Poista aseman ryhmän** -valintaikkuna. Kirjoita **Vahvista** tekstiruutuun ja valitse sitten **OK**. 

    Poistetun aseman ryhmän katoaa **tulosruudussa** -luettelosta ja kaikki varmuuskopiot, jotka liittyvät aseman ryhmän poistetaan ja varmuuskopion luettelon.

## <a name="next-steps"></a>Seuraavat vaiheet

- Lisätietoja käyttämisestä [StorSimple tilannevedoksen hallinta ammattimainen StorSimple ratkaisu](storsimple-snapshot-manager-admin.md).
- Lisätietoja käyttämisestä [StorSimple tilannevedoksen hallinta voit luoda ja hallita varmuuskopion käytännöt](storsimple-snapshot-manager-manage-backup-policies.md).
