<properties 
   pageTitle="StorSimple tilannevedoksen hallinta varmuuskopiotiedoston | Microsoft Azure"
   description="Tässä artikkelissa käsitellään avulla voit tarkastella ja hallita varmuuskopion luettelon StorSimple tilannevedoksen hallinnan MMC-laajennus."
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
   ms.date="04/26/2016"
   ms.author="v-sharos" />

# <a name="use-storsimple-snapshot-manager-to-manage-the-backup-catalog"></a>Käytä StorSimple tilannevedoksen hallinnan varmuuskopion luettelon hallinta

## <a name="overview"></a>Yleiskatsaus

Ensisijainen StorSimple tilannevedoksen hallinta-funktio on, jotta voit luoda StorSimple tietomääristä sovelluksen johdonmukaisia varmuuskopioita tilannevedoksia muodossa. Tilannevedoksia näkyvät sitten nimeltä *varmuuskopiotiedoston*XML-tiedoston. Varmuuskopion luettelon järjestää tilannevedoksia aseman ryhmä ja napsauta paikallisen tilannevedoksen tai cloud tilannevedoksen. 

Tässä opetusohjelmassa kuvataan, miten voit käyttää **Varmuuskopioinnin luettelo** -solmu tekemällä seuraavat toimet:

- Palauttaa aseman 
- Kloonaa äänenvoimakkuutta tai asema-ryhmä 
- Poista varmuuskopiointi 
- Tiedoston palauttaminen
- Storsimple tilannevedoksen Manager-tietokannan palauttaminen

Voit tarkastella varmuuskopion luettelon laajentamalla **laajuus** -ruudun **Varmuuskopiotiedoston** -solmu ja laajentaminen äänenvoimakkuutta-ryhmä.

- Jos valitset ryhmänimen, **tulokset** -ruutu näyttää paikallisen tilannevedoksia ja cloud tilannevedoksia käytettävissä aseman ryhmän määrän. 

- Jos valitset **Paikallisen tilannevedoksen** tai **Cloud tilannevedoksen**, **tulokset** -ruudussa näkyy kunkin varmuuskopion tilannevedoksen (mukaan **Näyttöasetukset)** seuraavat tiedot: 

    - **Nimi** – tilannevedoksen otettiin aika. 

    - **Kirjoita** – onko paikallisen tilannevedoksen tai cloud tilannevedoksen. 

    - **Omistajan** – sisällön omistaja. 

    - **Käytettävissä olevat** – onko tilannevedoksen tällä hetkellä käytettävissä. **Tosi** näkyy, että tilannevedoksen on käytettävissä ja voit halutessasi palauttaa ne; **Epätosi** ilmaisee, että tilannevedoksen ei ole enää käytettävissä. 

    - **Tuotu** – onko varmuuskopioinnin on tuotu. **True** ilmaisee, että varmuuskopiointi on tuotu StorSimple hallinta-palvelusta laite on määritetty StorSimple tilannevedoksen hallinta; aikaa **Epätosi** tarkoittaa, että se ei tuoda, mutta on luotu StorSimple tilannevedoksen hallinnan avulla. (Voit helposti tunnistaa tuodut äänenvoimakkuutta-ryhmän koska siihen on lisätty, joka kertoo laite, josta aseman ryhmä on tuotu.)

    ![Varmuuskopiotiedoston](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Backup_catalog.png)

- Jos **Paikallisen tilannevedoksen** tai **Cloud tilannevedoksen**laajentaminen ja valitse sitten yksittäisiä tilannevedoksen nimi, **tulokset** -ruutu näyttää tilannevedoksen, jonka valitsit seuraavat tiedot:

    - **Nimi** – merkittyä kirjain äänenvoimakkuutta. 

    - **Paikallinen nimi** – paikallisen nimen asema (jos saatavilla). 

    - **Laitteen** – laitteen nimi, jonka äänenvoimakkuuden sijaitsee. 

    - **Käytettävissä olevat** – onko tilannevedoksen tällä hetkellä käytettävissä. **Tosi** näkyy, että tilannevedoksen on käytettävissä ja voit halutessasi palauttaa ne; **Epätosi** ilmaisee, että tilannevedoksen ei ole enää käytettävissä. 


## <a name="restore-a-volume"></a>Palauttaa aseman

Seuraavien ohjeiden avulla voit palauttaa aseman varmuuskopiosta.

#### <a name="prerequisites"></a>Edellytykset

Jos et ole vielä tehnyt aseman ja aseman ryhmän luomista ja poista äänenvoimakkuutta. Oletusarvon mukaan StorSimple tilannevedoksen hallinta Varmuuskopioi aseman ennen riittää se poistetaan. Tämän peruslevyä voit estää tietojen menettämisen, jos äänenvoimakkuuden poistetaan vahingossa tai jos tiedot vaativat palauttaa jostakin syystä. 

StorSimple tilannevedoksen hallinta näyttää seuraavan sanoman, kun se luo varo varmuuskopiointi.

![Automaattinen tilannevedoksen viesti](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Automatic_snap.png) 

>[AZURE.IMPORTANT]Et voi poistaa asema, joka on osa ryhmää äänenvoimakkuutta. Poista-vaihtoehto ei ole käytettävissä. <br>

#### <a name="to-restore-a-volume"></a>Jos haluat palauttaa aseman

1. Napsauta työpöydän StorSimple tilannevedoksen hallinnan käynnistäminen kuvaketta. 

2. **Laajuus** -ruudussa Laajenna **Varmuuskopioinnin luettelo** -solmu, laajenna äänenvoimakkuutta-ryhmä ja valitse sitten **Paikallisen tilannevedoksia** tai **Cloud tilannevedoksia**. Varmuuskopion tilannevedoksia luettelo tulee näkyviin **tulosruudussa** . 

3. Etsi varmuuskopion, jonka haluat palauttaa, napsauta hiiren kakkospainikkeella ja valitse sitten **Palauta**. 

    ![Varmuuskopiotiedoston palauttaminen](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Restore_BU_catalog.png) 

4. Valitse vahvistussivullavalmis Tarkista tiedot, kirjoita **Vahvista**ja valitse sitten **OK**. StorSimple tilannevedoksen hallinta käyttää varmuuskopion palauttaminen. 

    ![Vahvistussanoman palauttaminen](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Restore_volume_msg.png) 

5. Voit valvoa palautustoiminto, kun se suoritetaan. **Laajuus** -ruudussa Laajenna **Projektit** -solmu ja valitse sitten **käynnissä**. Projektin tiedot näkyvät **tulosruudussa** . Kun palautustyön on valmis, työn tiedot siirretään **viimeisen 24 tuntia** -luettelosta.

## <a name="clone-a-volume-or-volume-group"></a>Kloonaa äänenvoimakkuutta tai asema-ryhmä

Seuraavien ohjeiden avulla voit luoda äänenvoimakkuutta tai asema ryhmän kaksoiskappale (Kloonaa).

#### <a name="to-clone-a-volume-or-volume-group"></a>Jos haluat Kloonaa äänenvoimakkuutta tai asema-ryhmä

1. Napsauta työpöydän StorSimple tilannevedoksen hallinnan käynnistäminen kuvaketta.

2. **Laajuus** -ruudussa Laajenna **Varmuuskopioinnin luettelo** -solmu ja laajentaa ryhmän aseman **Cloud tilannevedoksia**. Varmuuskopioiden luettelo tulee näkyviin **tulosruudussa** .

3. Etsi äänenvoimakkuutta tai aseman ryhmä, johon haluat kopioida, äänenvoimakkuutta tai asema ryhmänimeä hiiren kakkospainikkeella ja valitse **Kloonaa**. **Kloonaa Cloud tilannevedos** -valintaikkuna.

    ![Kloonaa tilannevedoksen pilveen](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Clone.png) 

4. Viimeistele **Kloonaa Cloud tilannevedos** -valintaikkunassa seuraavasti: 

    1. Kirjoita **nimi** -tekstiruutuun kloonatun äänenvoimakkuuden nimi. Tämä nimi näkyy **asemat** -solmu. 

    2. (Valinnainen) Valitse **asema**ja valitse sitten avattavasta luettelosta kirjaimen. 

    3. (Valinnainen) valitsemalla **Kansion (NTFS)**, ja kirjoittaa kansiopolun tai valitse Selaa ja valitse kansion sijainti. 

    4. Valitse **Luo**.

5. Kun kloonausohjelmistoja prosessi on valmis, kloonatun äänenvoimakkuus on alusta. Käynnistä Serverin hallinta ja käynnistä sitten Levynhallinta. Lisätietoja on artikkelissa [käyttöön asemat](storsimple-snapshot-manager-manage-volumes.md#mount-volumes). Kun alustaa, äänenvoimakkuuden kohdasta **asemat** -solmu **laajuus** -ruudussa. Jos ei ole näkyvissä luettelossa äänenvoimakkuuden, Päivitä luettelo asemat ( **asemat** -solmu hiiren kakkospainikkeella ja valitsemalla sitten **Päivitä**).

## <a name="delete-a-backup"></a>Poista varmuuskopiointi

Poista tilannevedoksen varmuuskopion luettelon seuraavien ohjeiden avulla. 

>[AZURE.NOTE]Tilannevedoksen poistetaan varmuuskopioidut tilannevedoksen liittyviä tietoja. Kuitenkin pilvestä tietojen poistaminen voi kestää jonkin aikaa.<br>
 
#### <a name="to-delete-a-backup"></a>Jos haluat poistaa varmuuskopiointi

1. Napsauta työpöydän StorSimple tilannevedoksen hallinnan käynnistäminen kuvaketta.

2. **Laajuus** -ruudussa Laajenna **Varmuuskopioinnin luettelo** -solmu, laajenna äänenvoimakkuutta-ryhmä ja valitse sitten **Paikallisen tilannevedoksia** tai **Cloud tilannevedoksia**. Tilannevedoksia luettelo tulee näkyviin **tulosruudussa** . 

3. Napsauta hiiren kakkospainikkeella tilannevedoksen, jonka haluat poistaa, ja valitse sitten **Poista**.

4. Kun näyttöön tulee vahvistussanoma tulee näkyviin, valitse **OK**. 

## <a name="recover-a-file"></a>Tiedoston palauttaminen

Jos tiedosto poistetaan vahingossa asema, voit palauttaa tiedoston haetaan tilannevedoksen, joka päivämäärät valmiiksi poistamisen, Kloonaa aseman tilannevedoksen avulla ja kopioimalla tiedoston kloonatun äänenvoimakkuuden alkuperäisen äänenvoimakkuuden.

#### <a name="prerequisites"></a>Edellytykset

Ennen kuin aloitat, varmista, että nykyinen varmuuskopion aseman ryhmän. Poista jonkin aseman kyseisen ryhmän tietomääristä tallennetun tiedoston. Lopuksi poistetun tiedoston palauttaminen varmuuskopiosta seuraavien vaiheiden avulla. 

#### <a name="to-recover-a-deleted-file"></a>Poistetun tiedoston palauttaminen

1. Napsauta työpöydän StorSimple tilannevedoksen hallinta-kuvaketta. StorSimple tilannevedoksen hallinnan console-ikkuna tulee näkyviin. 

2. **Laajuus** -ruudussa Laajenna **Varmuuskopioinnin luettelo** -solmu ja tilannevedoksen, joka sisältää poistettu tiedosto selaamalla. Valitse yleensä tilannevedoksen, joka on luotu ennen poistamista. 

3. Etsi asema, johon haluat kopioida, napsauta hiiren kakkospainikkeella ja valitse **Kloonaa**. **Kloonaa Cloud tilannevedos** -valintaikkuna.

    ![Kloonaa tilannevedoksen pilveen](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Clone.png) 

4. Viimeistele **Kloonaa Cloud tilannevedos** -valintaikkunassa seuraavasti: 

   1. Kirjoita **nimi** -tekstiruutuun kloonatun äänenvoimakkuuden nimi. Tämä nimi näkyy **asemat** -solmu. 

   2. (Valinnainen) Valitse **asema**ja valitse sitten avattavasta luettelosta kirjaimen. 

   3. (Valinnainen) Valitsemalla **Kansion (NTFS)**, ja kirjoittaa kansiopolun tai valitse **Selaa** ja valitse sitten kansion sijainti. 

   4. Valitse **Luo**. 

5. Kun kloonausohjelmistoja prosessi on valmis, kloonatun äänenvoimakkuus on alusta. Käynnistä Serverin hallinta ja käynnistä sitten Levynhallinta. Lisätietoja on artikkelissa [käyttöön asemat](storsimple-snapshot-manager-manage-volumes.md#mount-volumes). Kun alustaa, äänenvoimakkuuden kohdasta **asemat** -solmu **laajuus** -ruudussa. 

    Jos ei ole näkyvissä luettelossa äänenvoimakkuuden, Päivitä luettelo asemat ( **asemat** -solmu hiiren kakkospainikkeella ja valitsemalla sitten **Päivitä**).

6. Avaa NTFS kansio, joka sisältää kloonatun äänenvoimakkuuden, laajenna **asemat** -solmu ja avaa sitten kloonatun äänenvoimakkuutta. Etsi jaettava tiedosto, jonka haluat palauttaa, ja kopioi se ensisijainen äänenvoimakkuutta.

7. Kun palauttaa tiedoston, voit poistaa NTFS-kansio, joka sisältää kloonatun äänenvoimakkuutta.

## <a name="restore-the-storsimple-snapshot-manager-database"></a>StorSimple tilannevedoksen Manager-tietokannan palauttaminen

StorSimple tilannevedoksen Manager-tietokanta kannattaa varmuuskopioida säännöllisesti tietokoneessa. Jos huono tapahtuu tai isäntätietokoneen epäonnistuu jostakin syystä, voit palauttaa sen sitten varmuuskopiosta. Tietokannan varmuuskopiointi luodaan manuaalisesti.

#### <a name="to-back-up-and-restore-the-database"></a>Tietokannan palauttaminen ja varmuuskopiointi

1. Microsoft StorSimple Management-palvelun pysäyttäminen:

    1. Aloita Serverin hallinta.

    2. Palvelimen hallinnan koontinäytössä **Työkalut** -valikon Valitse **palvelut**.

    3. Valitse **Microsoft StorSimple hallintapalvelun** **palvelut** -ikkuna.

    4. Valitse oikeanpuoleisen ruudun **Microsoft StorSimple hallintapalvelun**- **palvelun pysäyttäminen**.

2. Siirry isäntätietokoneen C:\ProgramData\Microsoft\StorSimple\BACatalog. 

    >[AZURE.NOTE] ProgramData on piilotettu kansio.
 
3. Etsi luettelon XML-tiedosto, kopioi tiedosto ja Tallenna kopio turvallisessa paikassa tai pilveen. Jos isännän epäonnistuu, voit palauttaa varmuuskopion käytäntöjä, jotka olet luonut StorSimple tilannevedoksen hallinta varmuuskopiotiedoston avulla.

    ![Azure StorSimple varmuuskopiotiedoston tiedosto](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_bacatalog.png)

4. Käynnistä Microsoft StorSimple Management-palvelu: 

    1. Palvelimen hallinnan koontinäytössä **Työkalut** -valikon Valitse **palvelut**.
    
    2. Valitse **Microsoft StorSimple hallintapalvelun** **palvelut** -ikkuna.

    3. Valitse oikeanpuoleisen ruudun **Microsoft StorSimple hallintapalvelun**- **palvelun käynnistäminen uudelleen**.

5. Siirry isäntätietokoneen C:\ProgramData\Microsoft\StorSimple\BACatalog. 

6. Poista luettelon XML-tiedosto ja korvaa se, jonka loit varmuuskopio. 

7. Valitse Käynnistä StorSimple tilannevedoksen hallinta työpöydän StorSimple tilannevedoksen hallinta-kuvake. 

## <a name="next-steps"></a>Seuraavat vaiheet

- Lisätietoja [StorSimple tilannevedoksen hallinnan ammattimainen StorSimple-ratkaisun avulla](storsimple-snapshot-manager-admin.md).
- Lisätietoja [StorSimple tilannevedoksen hallinta tehtävät ja työnkulkuja](storsimple-snapshot-manager-admin.md#storsimple-snapshot-manager-tasks-and-workflows).
