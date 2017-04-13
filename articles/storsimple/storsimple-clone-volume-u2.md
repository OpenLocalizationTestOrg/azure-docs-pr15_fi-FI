<properties
   pageTitle="Kloonaa StorSimple äänenvoimakkuutta | Microsoft Azure"
   description="Kuvataan eri Kloonaa tyypit ja käyttää niitä, ja kerrotaan asettaminen Kloonaa yksittäisiä aseman varmuuskopion käyttämisestä."
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
   ms.date="07/27/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-clone-a-volume-update-2"></a>Tilavuus (päivitys 2) Kloonaa StorSimple hallinta-palvelun avulla

[AZURE.INCLUDE [storsimple-version-selector-clone-volume](../../includes/storsimple-version-selector-clone-volume.md)]

## <a name="overview"></a>Yleiskatsaus

StorSimple hallinta palvelun **Varmuuskopiotiedoston** -sivulla näkyvät kaikki, jotka on luotu manuaalisesti tai automaattisen varmuuskopioinnin otettaessa varmuuskopion joukot. Voit tämän sivun avulla luettelon kaikki varmuuskopion käytännön tai asema, valitse varmuuskopiot tai poistaa varmuuskopioita tai palauttaa tai Kloonaa aseman varmuuskopion avulla.

![Varmuuskopiotiedoston sivu](./media/storsimple-clone-volume-u2/backupCatalog.png)  

Tässä opetusohjelmassa kuvataan, miten voit käyttää asettaminen Kloonaa yksittäisiä aseman varmuuskopion. Artikkelissa kerrotaan myös *lyhytkestoisia* ja *pysyvä* kloonit välinen erotus.

>[AZURE.NOTE] 
>
>Paikallisesti kiinnitetty äänenvoimakkuus kopioitu kuin Porrastettu äänenvoimakkuutta. Jos tarvitset kloonatun aseman paikallisesti kiinnitetty, voit muuntaa Kloonaa paikallisesti kiinnitetty aseman jälkeen Kloonaustoiminto on suoritettu. Lisätietoja Porrastettu aseman muuntaminen paikallisesti kiinnitetty äänenvoimakkuutta Siirry [vaihtaminen asematyyppi](storsimple-manage-volumes-u2.md#change-the-volume-type).
>
>Jos yrität muuntaa kloonatun aseman-tasoisen paikallisesti kiinnitetyt heti, kun kloonaamalla (kun se on edelleen lyhytkestoisia Kloonaa), jos haluat muuntaminen epäonnistuu ja seuraava virhesanoma:
>
>`Unable to modify the usage type for volume {0}. This can happen if the volume being modified is a transient clone and hasn’t been made permanent. Take a cloud snapshot of this volume and then retry the modify operation.` 
>
>Tämä virhe vastaanotetaan vain, jos ovat kloonaamalla voin jonkin toisen laitteen. Voit muuntaa kiinnitetyt paikallisesti Jos muunnat lyhytkestoisia Kloonaa ensin pysyvä Kloonaa äänenvoimakkuutta. Muunnetaan pysyvästi Kloonaa lyhytkestoisia Kloonaa sen kuvata pilveen.

## <a name="create-a-clone-of-a-volume"></a>Luo Kloonaa aseman

Voit luoda Kloonaa samaan laitteeseen, toiseen laitteeseen tai jopa virtual machine käyttämällä paikallista tai cloud tilannevedoksen.

#### <a name="to-clone-a-volume"></a>Jos haluat Kloonaa aseman

1. Palvelun StorSimple hallinta-sivulla **varmuuskopioinnin luettelo** -välilehti ja valitse varmuuskopion.

2. Laajenna asetukseksi Näytä liittyvät asemat varmuuskopion. Valitse ja valitse aseman varmuuskopion joukosta.

     ![Kloonaa aseman](./media/storsimple-clone-volume-u2/CloneVol.png) 

3. Valitse Aloita kloonaamalla valitun aseman **Kloonaa** .

4. Valitse Kloonaa aseman ohjatussa **Määritä nimi ja sijainti**-kohdassa:

  1. Määritä target laite. Tämä on sijainti, johon Kloonaa luodaan. Voit valita samaan laitteeseen tai määrittää toisen laitteen. Jos valitset cloud palveluntarjoajia liittyvän aseman (ei Azure), kohde-laitteeseen pudotusvalikosta näytetään ainoastaan fyysisiä laitteita. Cloud palveluntarjoajia virtual laitteessa liittyvän aseman ei voi kopioida.

        >[AZURE.NOTE] Varmista, että Kloonaa edellyttämän kapasiteetin on pienempi kuin käytettävissä kohde laitteeseen kapasiteetti.

  2. Määrittää, että Kloonaa yksilöllinen nimi. Nimen on oltava 3 ja 127 merkkiä. 
    
        >[AZURE.NOTE] **Äänenvoimakkuuden Kloonaa nimellä** -kenttä on **Tiered** vaikka ovat kloonaamalla paikallisesti kiinnitetty äänenvoimakkuutta. Et voi muuttaa tätä asetusta; kuitenkin kloonatun aseman paikallisesti kiinnitetty sekä tarvittaessa voit muuntaa Kloonaa paikallisesti kiinnitetty aseman luotuasi onnistuneesti Kloonaa. Lisätietoja Porrastettu aseman muuntaminen paikallisesti kiinnitetty äänenvoimakkuutta Siirry [vaihtaminen asematyyppi](storsimple-manage-volumes-u2.md#change-the-volume-type).

        ![Ohjattu Kloonaa 1](./media/storsimple-clone-volume-u2/clone1.png) 

  3. Napsauttamalla nuolta ![nuolen kuvaketta](./media/storsimple-clone-volume-u2/HCS_ArrowIcon.png) Jatka seuraavalle sivulle.

5. Valitse **Määritä isännät, jotka käyttävät tätä asemaa**:

  1. Määritä on access control-tietue (ACR) Kloonaa. Voit lisätä uuden ACR tai valitse aiemmin luotua luetteloa.

        ![Ohjattu Kloonaa 2](./media/storsimple-clone-volume-u2/clone2.png) 

  2. Valitse Tarkista-kuvake ![tarkistuksen kuvake](./media/storsimple-clone-volume-u2/HCS_CheckIcon.png)toiminnon suorittamiseksi.

6. Kloonaa työn käynnistetään ja saat ilmoituksen, kun Kloonaa on luotu. Valitse **Näytä työn** seurannassa Kloonaa työn **työt** -sivu. Seuraava sanoma tulee näkyviin, kun Kloonaa työ on valmis:

    ![Kloonaa viesti](./media/storsimple-clone-volume-u2/CloneMsg.png) 

7. Kloonaa työn jälkeen:

  1. Siirry sivulle, **laitteet** ja valitse **Aseman säilöt** -välilehti. 
  2. Valitse aseman säilö, joka on liitetty lähdeasema, joka Voit monistaa. Asemat-luettelossa pitäisi näkyä, joka on luotu vain Kloonaa.

>[AZURE.NOTE] Seuranta- ja oletusarvoisesti varmuuskopio poistetaan käytöstä automaattisesti kloonatun asemassa.

Kloonaa, joka on luotu tällä tavalla on lyhytkestoisia Kloonaa. Saat lisätietoja Kloonaa tyypeistä [lyhytkestoisia ja pysyvä kloonit](#transient-vs.-permanent-clones).

Tämä Kloonaa on nyt säännöllisesti äänenvoimakkuus ja toiminnon, joka on mahdollista asemassa ole käytettävissä Kloonaa. Haluat määrittää tämän voimakkuutta varmuuskopiot.

## <a name="transient-vs-permanent-clones"></a>Pysyvä kloonit ja lyhytaikainen

Lyhytkestoisia kloonit luodaan vain silloin, kun ovat kloonaamalla toiseen laitteeseen. Voit kopioida määritetyn aseman varmuuskopiosta määrittää toisen laitteen, hallita StorSimple hallinta. Lyhytkestoisia Kloonaa saa alkuperäisen äänenvoimakkuuden viittauksia tietoihin ja käyttää tietojen lukeminen ja kirjoittaminen kohde laitteeseen paikallisesti. 

Kun viet lyhytkestoisia Kloonaa cloud tilannevedoksen, tuloksena Kloonaa on *pysyvä* Kloonaa. Tämän prosessin aikana kopion tiedot luodaan pilveen ja kopioi nämä tiedot aika määräytyy kokoa tiedot ja Azure viiveitä (tämä on Azure Azure-kopio). Toiminto voi kestää viikon ajalta päivää. Lyhytkestoisia Kloonaa tulee pysyvästi Kloonaa tällä tavalla, mutta se ei ole aseman alkuperäiset tiedot, se on kopioitu-viittauksia. 

## <a name="scenarios-for-transient-and-permanent-clones"></a>Tilanteita, joissa lyhytkestoisia ja pysyvä kloonit

Seuraavissa osissa on esimerkki tilanteissa, joissa lyhytkestoisia ja pysyvä kloonit voidaan.

### <a name="item-level-recovery-with-a-transient-clone"></a>Kohdetason hyödyntämistä lyhytkestoisia Kloonaa

Sinun on yksi vuosi-vanha Microsoft PowerPoint-esitys-Tiedosto Palauta. Järjestelmänvalvoja määrittää tietyn varmuuskopiointi-että ja suodattaa äänenvoimakkuutta. Järjestelmänvalvoja kloonit äänenvoimakkuuden, paikantaa tiedoston, jossa hakua ja antaa sinulle. Tässä skenaariossa lyhytkestoisia Kloonaa käytetään. 
 
![Video käytettävissä](./media/storsimple-clone-volume-u2/Video_icon.png) **Video käytettävissä**

Katso video, jossa esitellään, miten voit käyttää Kloonaa ja palauttaa Palauta poistettuja tiedostoja StorSimple ominaisuuksia, napsauta [tätä](https://azure.microsoft.com/documentation/videos/storsimple-recover-deleted-files-with-storsimple/).

### <a name="testing-in-the-production-environment-with-a-permanent-clone"></a>Pysyvä Kloonaa kanssa tuotantoympäristössä testaaminen

Haluat tarkistaa testauksen ohjelmavirhe tuotantoympäristössä. Voit luoda Kloonaa aseman tuotantoympäristössä ja tee tämä Kloonaa riippumaton kloonatun aseman luominen cloud tilannevedoksen. Tässä skenaariossa pysyvä Kloonaa käytetään.  

## <a name="next-steps"></a>Seuraavat vaiheet
- Lisätietoja palauttaminen [StorSimple aseman varmuuskopion joukosta](storsimple-restore-from-backup-set-u2.md).

- Lue ohjeet [ammattimainen StorSimple laitteen StorSimple hallinta-palvelun käyttöä](storsimple-manager-service-administration.md)varten.

 
