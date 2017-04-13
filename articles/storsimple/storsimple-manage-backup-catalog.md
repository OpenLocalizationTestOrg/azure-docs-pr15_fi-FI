<properties 
   pageTitle="Hallitse StorSimple varmuuskopion luettelon | Microsoft Azure"
   description="Tässä artikkelissa kuvataan luettelo ja valitse Poista varmuuskopion joukot aseman StorSimple hallinnan palvelun varmuuskopiotiedoston sivun avulla."
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
   ms.date="04/28/2016"
   ms.author="v-sharos" />

# <a name="use-the-storsimple-manager-service-to-manage-your-backup-catalog"></a>Hallitse varmuuskopion luettelon StorSimple hallinta-palvelun avulla

## <a name="overview"></a>Yleiskatsaus

StorSimple hallinta palvelun **Varmuuskopiotiedoston** -sivulla näkyvät kaikki, jotka on luotu manuaalisen tai ajoitetun varmuuskopioinnin otettaessa varmuuskopion joukot. Voit tämän sivun avulla luettelon kaikki varmuuskopion käytännön tai asema, valitse varmuuskopiot tai poistaa varmuuskopioita tai palauttaa tai Kloonaa aseman varmuuskopion avulla.

Tässä opetusohjelmassa kerrotaan, kuinka luettelo ja valitse Poista varmuuskopion. Lisätietoja laitteen palauttaminen varmuuskopiosta, siirry [palauttaa laitteen varmuuskopion joukosta](storsimple-restore-from-backup-set.md). Lisätietoja Kloonaa asema, siirry [Kloonaa StorSimple äänenvoimakkuutta](storsimple-clone-volume.md).

![Varmuuskopiotiedoston](./media/storsimple-manage-backup-catalog/backupcatalog.png) 

**Varmuuskopiotiedoston** -sivulla on kyselyn rajaaminen varmuuskopioinnin määrittäminen valinta. Voit suodattaa varmuuskopion joukot, jotka on haettu, seuraavien parametrien perusteella:

- **Laitteen** – laite, joka on luotu varmuuskopioinnin määrittäminen.

- **Varmuuskopion tai asema** – varmuuskopion käytännön tai tältä liittyvän aseman.

- **Mistä ja mihin** – päivämäärä ja aika-alueen varmuuskopioinnin määrittäminen luonnin yhteydessä.

Suodatettu varmuuskopion joukot ovat sitten taulukkomuotoinen seuraavien määritteiden perusteella:

- **Nimi** – varmuuskopion käytännön tai varmuuskopioinnin määrittäminen liittyvän aseman nimi.

- **Koon** – todellinen koko varmuuskopioinnin määrittäminen.

- **Luotu** – päivämäärä ja kellonaika, jolloin varmuuskopioista on luotu. 

- **Kirjoita** – varmuuskopiointi joukot voidaan paikallisen tilannevedoksia tai cloud tilannevedoksia. Paikallisen tilannevedoksen on kaikki paikallisesti tallennetut laitteen aseman tietojen varmuuskopioinnin taas cloud tilannevedoksen viittaa aseman tietojen pilveen asuvat varmuuskopion. Paikallinen tilannevedoksia on nopeuttamiseksi, olisi cloud tilannevedoksia valitaan tietojen vikasietoisuudelle.

- **Aloitetaan mukaan** – varmuuskopioista voidaan aloittaa automaattisesti aikataulun tai manuaalisesti käyttäjä. Voit tehdä varmuuskopion käytännön varmuuskopioinnin. Vaihtoehtoisesti voit ottaa manuaalinen varmuuskopiointi **tehdä varmuuskopio** -asetus.

## <a name="list-backup-sets-for-a-volume"></a>Asema määrittää luettelon varmuuskopiointi
 
Seuraavien vaiheiden luettelon kaikki aseman varmuuskopiot.

#### <a name="to-list-backup-sets"></a>Luettelon varmuuskopion tiedostojoukot

1. Valitse StorSimple hallinta-palvelu‑sivulla **varmuuskopioinnin luettelo** -välilehti.

2. Suodata valinnat seuraavasti:

    1. Valitse haluamasi laite.

    2. Valitse avattavasta luettelosta asema, voit tarkastella sitä vastaava varmuuskopioista.

    3. Määritä aikaväli.

    4. Valitse Tarkista-kuvake ![Tarkistuksen kuvake](./media/storsimple-manage-backup-catalog/HCS_CheckIcon.png) Jos haluat suorittaa kyselyn.
 
    Valitun aseman liittyvät varmuuskopiot pitäisi näkyä varmuuskopion joukot luettelo.

## <a name="select-a-backup-set"></a>Valitse varmuuskopioinnin määrittäminen

Seuraavien vaiheiden Valitse äänenvoimakkuutta tai varmuuskopion käytännön määrittäminen varmuuskopion.

#### <a name="to-select-a-backup-set"></a>Valitse varmuuskopioinnin määrittäminen

1. Valitse StorSimple hallinta-palvelu‑sivulla **varmuuskopioinnin luettelo** -välilehti.

2. Suodata valinnat seuraavasti:

    1. Valitse haluamasi laite.

    2. Valitse avattavasta luettelosta, jonka haluat valita varmuuskopioinnin äänenvoimakkuutta tai varmuuskopiointi käytäntö.

    3. Määritä aikaväli.

    4. Valitse Tarkista-kuvake ![Tarkistuksen kuvake](./media/storsimple-manage-backup-catalog/HCS_CheckIcon.png) Jos haluat suorittaa kyselyn.

    Varmuuskopioista liittyvät valitun aseman tai varmuuskopion käytännön pitäisi näkyä varmuuskopion joukot luettelo.

3. Valitse ja laajenna varmuuskopion. **Palauttaa** ja **poistaa** vaihtoehdot näkyvät sivun alareunassa. Voit suorittaa jommankumman näistä toiminnoista, jotka valitsit varmuuskopioinnin määrittäminen.

## <a name="delete-a-backup-set"></a>Poista varmuuskopioinnin määrittäminen

Poista varmuuskopion, kun et halua enää säilyttää sen liittyviä tietoja. Seuraavien toimien poistaa varmuuskopion.

#### <a name="to-delete-a-backup-set"></a>Jos haluat poistaa varmuuskopioinnin määrittäminen

1. Palvelun StorSimple hallinta-sivulla Valitse **varmuuskopioinnin luettelo-välilehti**.

2. Suodata valinnat seuraavasti:

    1. Valitse haluamasi laite.

    2. Valitse avattavasta luettelosta, jonka haluat valita varmuuskopioinnin äänenvoimakkuutta tai varmuuskopiointi käytäntö.

    3. Määritä aikaväli.

    4. Valitse Tarkista-kuvake ![Tarkistuksen kuvake](./media/storsimple-manage-backup-catalog/HCS_CheckIcon.png) Jos haluat suorittaa kyselyn.

    Varmuuskopioista liittyvät valitun aseman tai varmuuskopion käytännön pitäisi näkyä varmuuskopion joukot luettelo.

3. Valitse ja laajenna varmuuskopion. **Palauttaa** ja **poistaa** vaihtoehdot näkyvät sivun alareunassa. Valitse **Poista**.

4. Saat ilmoituksen, kun poisto on käynnissä ja kun se on suoritettu loppuun. Kun poisto on valmis, voit päivittää kyselyn tällä sivulla. Poistetun varmuuskopioinnin määrittäminen se ei enää näy varmuuskopion joukot-luettelossa.

## <a name="next-steps"></a>Seuraavat vaiheet

- Tietoja [varmuuskopion luettelon Palauta varmuuskopio joukosta laitteen](storsimple-restore-from-backup-set.md)käyttämisestä.

- Lue ohjeet [ammattimainen StorSimple laitteen StorSimple hallinta-palvelun käyttöä](storsimple-manager-service-administration.md)varten.
