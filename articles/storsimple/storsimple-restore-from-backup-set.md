<properties 
   pageTitle="StorSimple aseman palauttaminen varmuuskopiosta | Microsoft Azure"
   description="Tässä artikkelissa kuvataan StorSimple hallinnan palvelun varmuuskopiotiedoston sivun avulla voit palauttaa StorSimple aseman varmuuskopion."
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
   ms.date="08/17/2016"
   ms.author="alkohli" />

# <a name="restore-a-storsimple-volume-from-a-backup-set"></a>Palauttaa StorSimple aseman varmuuskopioinnin määrittäminen

[AZURE.INCLUDE [storsimple-version-selector-restore-from-backup](../../includes/storsimple-version-selector-restore-from-backup.md)]

## <a name="overview"></a>Yleiskatsaus

**Varmuuskopiotiedoston** -sivulla näkyvät kaikki, jotka on luotu manuaalisesti tai automaattisen varmuuskopioinnin otettaessa varmuuskopion joukot. Voit tämän sivun avulla luettelon kaikki varmuuskopion käytännön tai asema, valitse varmuuskopiot tai poistaa varmuuskopioita tai palauttaa tai Kloonaa aseman varmuuskopion avulla.

 ![Voit varmuuskopioida luettelo sivulle](./media/storsimple-restore-from-backup-set/HCS_BackupCatalog.png)

Tässä opetusohjelmassa kerrotaan, miten voit palauttaa laitteen aseman varmuuskopion **Varmuuskopiotiedoston** sivun avulla.

## <a name="how-to-use-the-backup-catalog"></a>Varmuuskopion luettelon käyttäminen 

**Varmuuskopiotiedoston** -sivulla on kysely, joka avulla voit rajata varmuuskopioinnin määrittäminen valinta. Voit suodattaa varmuuskopion joukot, jotka on haettu seuraavien parametrien perusteella:

- **Laitteen** – laite, joka on luotu varmuuskopioinnin määrittäminen.
- **Varmuuskopiointi-käytäntö** tai **asema** – varmuuskopion käytännön tai tältä liittyvän aseman.
- **Mistä** - ja **ja** – päivämäärä ja aika-alueen varmuuskopioinnin määrittäminen luonnin yhteydessä.

Suodatettu varmuuskopion joukot ovat sitten taulukkomuotoinen seuraavien määritteiden perusteella:

- **Nimi** – varmuuskopion käytännön tai varmuuskopioinnin määrittäminen liittyvän aseman nimi.
- **Koon** – todellinen koko varmuuskopioinnin määrittäminen.
- **Valitse luotu** – päivämäärä ja kellonaika, jolloin varmuuskopioista on luotu. 
- **Kirjoita** – varmuuskopiointi joukot voidaan paikallisen tilannevedoksia tai cloud tilannevedoksia. Paikallisen tilannevedoksen on kaikki paikallisesti tallennetut laitteen aseman tietojen varmuuskopioinnin taas cloud tilannevedoksen viittaa aseman tietojen pilveen asuvat varmuuskopion. Paikallinen tilannevedoksia on nopeuttamiseksi, olisi cloud tilannevedoksia valitaan tietojen vikasietoisuudelle.
- **Löytäjä** – varmuuskopioista voidaan aloittaa automaattisesti aikataulun mukaan tai käyttäjän manuaalisesti. (Voit tehdä varmuuskopion käytännön varmuuskopioinnin. Vaihtoehtoisesti voit **tehdä varmuuskopio** -asetus tulevat vuorovaikutteinen varmuuskopiointi.)

## <a name="how-to-restore-your-storsimple-volume-from-a-backup"></a>StorSimple äänenvoimakkuutta palauttaminen varmuuskopiosta

Voit palauttaa StorSimple äänenvoimakkuutta tietyn varmuuskopiosta **Varmuuskopiotiedoston** -sivulla. 

> [AZURE.WARNING] Varmuuskopiosta palauttaminen korvaa aiemmin luodut asemat varmuuskopiosta. Tämä voi hävitä tietoja, jotka on kirjoitettu, kun varmuuskopio on tehty.

Ennen kuin käynnistät palauttaminen levyllä, varmista, että ääni on offline-tilassa. Sinun on otettava äänenvoimakkuuden offline-tilassa isännän ensimmäisen ja valitse haluamasi laite. [Ota offline-tilassa aseman](storsimple-manage-volumes.md#take-a-volume-offline)noudattamalla. Seuraavien toimien aseman palauttaminen varmuuskopion.

### <a name="to-restore-from-a-backup-set"></a>Varmuuskopion palauttaminen

1. Valitse StorSimple hallinta-palvelu‑sivulla **varmuuskopioinnin luettelo** -välilehti.

    ![Varmuuskopiotiedoston](./media/storsimple-restore-from-backup-set/HCS_Restore.png)

2. Valitse varmuuskopioinnin määrittäminen seuraavasti:
  1. Valitse haluamasi laite.
  2. Valitse avattavasta luettelosta, jonka haluat valita varmuuskopioinnin äänenvoimakkuutta tai varmuuskopiointi käytäntö.
  3. Määritä aikaväli.
  4. Valitse Tarkista-kuvake ![Tarkista kuvake](./media/storsimple-restore-from-backup-set/HCS_CheckIcon.png) Jos haluat suorittaa kyselyn.
 
    Varmuuskopioista liittyvät valitun aseman tai varmuuskopion käytännön pitäisi näkyä varmuuskopion joukot luettelo.

3. Laajenna asetukseksi Näytä liittyvät asemat varmuuskopion. Asemat on otettava offline-tilaan isännän ja laitteen, ennen kuin voit palauttaa ne. [Ota offline-tilassa aseman](storsimple-manage-volumes.md#take-a-volume-offline)noudattamalla.

    >  [AZURE.IMPORTANT] Varmista, että suorittamasi tietomääristä offline-tilassa isännän ensin laitteessa Aloita tietomääristä offline-tilassa. Jos et tee isännän tietomääristä offline-tilassa, se voi aiheuttaa vahingoittumismahdollisuus.

4. Valitse Varmuuskopioi. Valitse sivun alareunassa **Palauta** .

6. Voit pyydetään vahvistamaan. 

    ![Vahvistus-sivu](./media/storsimple-restore-from-backup-set/HCS_ConfirmRestore.png)

7. Tarkista Palauta tiedot ja valitse tarkistuksen kuvake ![kuvakkeesta](./media/storsimple-restore-from-backup-set/HCS_CheckIcon.png). Tämä aloittaa palautustyön, jonka voit tarkastaa muodostamalla yhteyden **työt** -sivu. 

8. Kun palautus on valmis, voit varmistaa, että asemat sisällön korvataan tietomääristä varmuuskopiosta.

![Video käytettävissä](./media/storsimple-restore-from-backup-set/Video_icon.png) **Video käytettävissä**

Katso video, jossa esitellään, miten voit käyttää Kloonaa ja palauttaa Palauta poistettuja tiedostoja StorSimple ominaisuuksia, napsauta [tätä](https://azure.microsoft.com/documentation/videos/storsimple-recover-deleted-files-with-storsimple/).

## <a name="next-steps"></a>Seuraavat vaiheet

- Lue, miten voit [hallita StorSimple asemat](storsimple-manage-volumes.md).

- Lue ohjeet [ammattimainen StorSimple laitteen StorSimple hallinta-palvelun käyttöä](storsimple-manager-service-administration.md)varten.
