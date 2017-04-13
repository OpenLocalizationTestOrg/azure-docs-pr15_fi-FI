<properties 
   pageTitle="StorSimple aseman palauttaminen varmuuskopiosta | Microsoft Azure"
   description="Tässä artikkelissa kuvataan StorSimple hallinnan palvelun varmuuskopiotiedoston sivun avulla voit palauttaa StorSimple aseman varmuuskopion."
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

# <a name="restore-a-storsimple-volume-from-a-backup-set-update-2"></a>Palauttaa StorSimple aseman varmuuskopioinnin määrittäminen (päivitys 2)

[AZURE.INCLUDE [storsimple-version-selector-restore-from-backup](../../includes/storsimple-version-selector-restore-from-backup.md)]

## <a name="overview"></a>Yleiskatsaus

**Varmuuskopiotiedoston** -sivulla näkyvät kaikki, jotka on luotu manuaalisesti tai automaattisen varmuuskopioinnin otettaessa varmuuskopion joukot. Voit tämän sivun avulla luettelon kaikki varmuuskopion käytännön tai asema, valitse varmuuskopiot tai poistaa varmuuskopioita tai palauttaa tai Kloonaa aseman varmuuskopion avulla.

 ![Voit varmuuskopioida luettelo sivulle](./media/storsimple-restore-from-backup-set-u2/restore.png)

Tässä opetusohjelmassa kerrotaan, miten voit palauttaa laitteen varmuuskopion **Varmuuskopiotiedoston** sivun avulla.

Voit palauttaa paikallisesta äänenvoimakkuutta tai cloud varmuuskopioinnin. Kummassakin tapauksessa palautustoiminto näyttää äänenvoimakkuuden verkossa välittömästi, kun tiedot on ladattu taustalla. 

Ennen kuin käynnistät palautustyön, sinun olisi otettava huomioon seuraavasti:

- **Sinun on otettava offline-tilassa äänenvoimakkuuden** – Ota offline-tilassa äänenvoimakkuuden isännän ja laite, ennen kuin voit aloittaa palautustoiminto. Vaikka palautustoiminto yhdistää automaattisesti äänenvoimakkuuden verkossa laitteeseen, sinun on tuotava laitteen verkossa manuaalisesti isännän. Voit tuoda äänenvoimakkuuden verkossa isännän heti, kun ääni on online laitteessa. (Sinun ei tarvitse odottaa, kunnes palautus on valmis.) Ohjeet Siirry [otetaan tilavuus offline-tilassa](storsimple-manage-volumes-u2.md#take-a-volume-offline).

- **Avauksen ja vaihdon tyyppi palautuksen jälkeen** – poistetut asemat palautetaan tilannevedoksen; lajin perusteella Toisin sanoen asemat, jotka olivat paikallisesti kiinnitetyt palautetaan kuin paikallisesti kiinnitetty asemat ja asemat, jotka olivat tasoisen palautetaan kuin Porrastettu asemat.

    Olemassa olevan levyasemien nykyinen käyttö tyypin äänenvoimakkuuden ohittaa tyyppi, joka on tallennettu tilannevedoksen. Esimerkiksi jos palautat aseman tilannevedoksen, joka on tehty, kun asematyyppi on tasoisen ja että asematyyppi on nyt paikallisesti kiinnitetty (vuoksi muuntotoiminto, joka on tehty), valitse äänenvoimakkuuden palautetaan kuin paikallisesti kiinnitetty äänenvoimakkuutta. Vastaavasti jos aiemmin luotua paikallisesti kiinnitetty asemaa on laajennettu ja myöhemmin palauttaa vanhoja tilannevedoksen, joka suoritetaan, kun äänenvoimakkuuden on pienempi, palautettu äänenvoimakkuuden Säilytä nykyinen laajennettu koko.

    Ei voi muuntaa aseman Porrastettu aseman kiinnitetty paikalliseen asemaan tai Porrastettu aseman paikallisesti kiinnitetty aseman, kun äänenvoimakkuuden palautetaan. Odota, kunnes palautus on valmis, ja valitse äänenvoimakkuuden voidaan muuntaa jonkin muun. Lisätietoja muuntamisesta asema Siirry [vaihtaminen asematyyppi](storsimple-manage-volumes-u2.md#change-the-volume-type). 

- **Asemakoko näkyvät palautettu äänenvoimakkuuden** – tämä on otettava huomioon, jos palautat paikallisesti kiinnitetty asema, joka on poistettu (koska paikallisesti kiinnitetty tietomääristä täysin valmistelun yhteydessä). Varmista, että sinulla on tarpeeksi tilaa, ennen kuin yrität palauttaa paikallisesti kiinnitetty asema, joka poistettiin. 

- **Ei voi laajentaa äänenvoimakkuutta, kun palautetaan parhaillaan** – Odota, kunnes palautus on valmis, ennen kuin yrität Laajenna äänenvoimakkuutta. Lisätietoja laajentaminen asema Siirry [Muokkaa aseman](storsimple-manage-volumes-u2.md#modify-a-volume).

- **Voit suorittaa varmuuskopioinnin aikana palautat paikalliseen asemaan** – ohjeita Tutustu [StorSimple-hallintapalveluun, varmuuskopion käytäntöjen hallinta](storsimple-manage-backup-policies.md).

- **Voit peruuttaa palautustyön** – Jos peruutat palautustyön ja sitten äänenvoimakkuuden peruutetaan tilaan, jossa se oli, ennen kuin olet aloittanut palautustoiminto. Ohjeet Siirry [työn peruuttaminen](storsimple-manage-jobs-u2.md#cancel-a-job).

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

Voit palauttaa StorSimple äänenvoimakkuutta tietyn varmuuskopiosta **Varmuuskopiotiedoston** -sivulla. Ota kuitenkin huomioon, kyseisen palauttaminen aseman palautetaan tilaan, jossa se oli varmuuskopioinnin otettaessa äänenvoimakkuutta. Tiedot, jotka on lisätty jälkeen varmuuskopiointia, menetetään.

> [AZURE.WARNING] Varmuuskopiosta palauttaminen korvaa aiemmin luodut asemat varmuuskopiosta. Tämä voi hävitä tietoja, jotka on kirjoitettu, kun varmuuskopio on tehty.

### <a name="to-restore-your-volume"></a>Jos haluat palauttaa äänenvoimakkuutta

1. Valitse StorSimple hallinta-palvelu‑sivulla **varmuuskopioinnin luettelo** -välilehti.

    ![Varmuuskopiotiedoston](./media/storsimple-restore-from-backup-set-u2/restore.png)

2. Valitse varmuuskopioinnin määrittäminen seuraavasti:
  1. Valitse haluamasi laite.
  2. Valitse avattavasta luettelosta, jonka haluat valita varmuuskopioinnin äänenvoimakkuutta tai varmuuskopiointi käytäntö.
  3. Määritä aikaväli.
  4. Valitse Tarkista-kuvake ![Tarkista kuvake](./media/storsimple-restore-from-backup-set-u2/HCS_CheckIcon.png) Jos haluat suorittaa kyselyn.
 
    Varmuuskopioista liittyvät valitun aseman tai varmuuskopion käytännön pitäisi näkyä varmuuskopion joukot luettelo.

3. Laajenna asetukseksi Näytä liittyvät asemat varmuuskopion. Asemat on otettava offline-tilaan isännän ja laitteen, ennen kuin voit palauttaa ne. Käyttää tietomääristä **Aseman säilöt** -sivulla ja noudata sitten ohjeita siirtää offline-tilaan [otetaan tilavuus offline-tilassa](storsimple-manage-volumes-u2.md#take-a-volume-offline) .

    > [AZURE.IMPORTANT] Varmista, että suorittamasi tietomääristä offline-tilassa isännän ensin laitteessa Aloita tietomääristä offline-tilassa. Jos et tee isännän tietomääristä offline-tilassa, se voi aiheuttaa vahingoittumismahdollisuus.

4. Siirry takaisin **Varmuuskopiotiedoston** -välilehti ja valitse varmuuskopion.

5. Valitse sivun alareunassa **Palauta** .

6. Voit pyydetään vahvistamaan. Tarkista palauttaminen tiedot ja valitse vahvistus-valintaruutu.

    ![Vahvistus-sivu](./media/storsimple-restore-from-backup-set-u2/ConfirmRestore.png)

7. Valitse tarkistuksen kuvake ![kuvakkeesta](./media/storsimple-restore-from-backup-set-u2/HCS_CheckIcon.png). Tämä aloittaa palautustyön, jonka voit tarkastaa muodostamalla yhteyden **työt** -sivu. 

8. Kun palautus on valmis, voit varmistaa, että asemat sisällön korvataan tietomääristä varmuuskopiosta.

![Video käytettävissä](./media/storsimple-restore-from-backup-set-u2/Video_icon.png) **Video käytettävissä**

Katso video, jossa esitellään, miten voit käyttää Kloonaa ja palauttaa Palauta poistettuja tiedostoja StorSimple ominaisuuksia, napsauta [tätä](https://azure.microsoft.com/documentation/videos/storsimple-recover-deleted-files-with-storsimple/).

## <a name="if-the-restore-fails"></a>Jos palauttaminen ei onnistu

Saat ilmoituksen, jos palautustoiminto epäonnistuu jostakin syystä. Jos näin tapahtuu, Päivitä varmuuskopion varmistamaan, että varmuuskopiointi on edelleen voimassa. Jos palautat pilvestä varmuuskopiointi on kelvollinen, valitse yhteysongelmat aiheuttavat ongelman. 

Palautustoiminnon suorittaminen kestää isännän äänenvoimakkuuden offline-tilassa ja yritä uudelleen palautustoiminto. Huomaa, että aseman tietoihin, jotka olivat suorittaa palautuksen yhteydessä muutokset menetetään.

## <a name="next-steps"></a>Seuraavat vaiheet

- Lue, miten voit [hallita StorSimple asemat](storsimple-manage-volumes-u2.md).

- Lue ohjeet [ammattimainen StorSimple laitteen StorSimple hallinta-palvelun käyttöä](storsimple-manager-service-administration.md)varten.
