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
   ms.date="08/17/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-clone-a-volume"></a>Kloonaa aseman StorSimple hallinta-palvelun avulla

[AZURE.INCLUDE [storsimple-version-selector-clone-volume](../../includes/storsimple-version-selector-clone-volume.md)]

## <a name="overview"></a>Yleiskatsaus

StorSimple hallinta palvelun **Varmuuskopiotiedoston** -sivulla näkyvät kaikki, jotka on luotu manuaalisesti tai automaattisen varmuuskopioinnin otettaessa varmuuskopion joukot. Voit tämän sivun avulla luettelon kaikki varmuuskopion käytännön tai asema, valitse varmuuskopiot tai poistaa varmuuskopioita tai palauttaa tai Kloonaa aseman varmuuskopion avulla.

![Varmuuskopiotiedoston sivu](./media/storsimple-clone-volume/HCS_BackupCatalog.png)  

Tässä opetusohjelmassa kuvataan, miten voit käyttää asettaminen Kloonaa yksittäisiä aseman varmuuskopion. Artikkelissa kerrotaan myös *lyhytkestoisia* ja *pysyvä* kloonit välinen erotus. 

## <a name="create-a-clone-of-a-volume"></a>Luo Kloonaa aseman

Voit luoda Kloonaa samaan laitteeseen, toiseen laitteeseen tai jopa virtual machine paikallisen tai tilannevedoksen pilven avulla.

#### <a name="to-clone-a-volume"></a>Jos haluat Kloonaa aseman

1. Palvelun StorSimple hallinta-sivulla **varmuuskopioinnin luettelo** -välilehti ja valitse varmuuskopion.

2. Laajenna asetukseksi Näytä liittyvät asemat varmuuskopion. Valitse ja valitse aseman varmuuskopion joukosta.

     ![Kloonaa aseman](./media/storsimple-clone-volume/HCS_Clone.png) 

3. Valitse Aloita kloonaamalla valitun aseman **Kloonaa** .

4. Valitse Kloonaa aseman ohjatussa **Määritä nimi ja sijainti**-kohdassa:

  1. Määritä target laite. Tämä on sijainti, johon Kloonaa luodaan. Voit valita samaan laitteeseen tai määrittää toisen laitteen. Jos valitset cloud palveluntarjoajia liittyvän aseman (ei Azure), kohde-laitteeseen pudotusvalikosta näytetään ainoastaan fyysisiä laitteita. Cloud palveluntarjoajia virtual laitteessa liittyvän aseman ei voi kopioida.

        >  [AZURE.NOTE] Varmista, että Kloonaa edellyttämän kapasiteetin on pienempi kuin käytettävissä kohde laitteeseen kapasiteetti.
  2. Määrittää, että Kloonaa yksilöllinen nimi. Nimen on oltava 3 ja 127 merkkiä.
  3. Napsauttamalla nuolta ![nuolen kuvaketta](./media/storsimple-clone-volume/HCS_ArrowIcon.png) Jatka seuraavalle sivulle.

5. Valitse **Määritä isännät, jotka käyttävät tätä asemaa**:

  1. Määritä on access control-tietue (ACR) Kloonaa. Voit lisätä uuden ACR tai valitse aiemmin luotua luetteloa.
  2. Valitse Tarkista-kuvake ![tarkistuksen kuvake](./media/storsimple-clone-volume/HCS_CheckIcon.png)toiminnon suorittamiseksi.

6. Kloonaa työn käynnistetään ja saat ilmoituksen, kun Kloonaa on luotu. Valitse **Näytä työn** seurannassa Kloonaa työn **työt** -sivu.

7. Kloonaa työn jälkeen:

  1. Siirry sivulle, **laitteet** ja valitse **Aseman säilöt** -välilehti. 
  2. Valitse aseman säilö, joka on liitetty lähdeasema, joka Voit monistaa. Asemat-luettelossa pitäisi näkyä, joka on luotu vain Kloonaa.

>[AZURE.NOTE] Seuranta- ja oletusarvoisesti varmuuskopio poistetaan käytöstä automaattisesti kloonatun asemassa.

Kloonaa, joka on luotu tällä tavalla on lyhytkestoisia Kloonaa. Saat lisätietoja Kloonaa tyypeistä [lyhytkestoisia ja pysyvä kloonit](#transient-vs.-permanent-clones).

Tämä Kloonaa on nyt säännöllisesti äänenvoimakkuus ja toiminnon, joka on mahdollista asemassa ole käytettävissä Kloonaa. Haluat määrittää tämän voimakkuutta varmuuskopiot.

## <a name="transient-vs-permanent-clones"></a>Pysyvä kloonit ja lyhytaikainen

Lyhytkestoisia ja pysyvä kloonit luodaan vain silloin, kun ovat kloonaamalla voin jonkin toisen laitteen. Voit kopioida määritetyn aseman varmuuskopiosta määrittää toisen laitteen. Kloonaa, joka on luotu tällä tavalla on *lyhytkestoisia* Kloonaa. Lyhytkestoisia Kloonaa on alkuperäisen äänenvoimakkuuden viittauksia ja käyttävät asemaan lukemaan kirjoitettaessa paikallisesti. 

Kun viet lyhytkestoisia Kloonaa cloud tilannevedoksen, tuloksena Kloonaa on *pysyvä* Kloonaa. Pysyvä Kloonaa on erillinen, mutta se ei ole alkuperäisen äänenvoimakkuuden, se on kopioitu-viittauksia.  

## <a name="scenarios-for-transient-and-permanent-clones"></a>Tilanteita, joissa lyhytkestoisia ja pysyvä kloonit

Seuraavissa osissa on esimerkki tilanteissa, joissa lyhytkestoisia ja pysyvä kloonit voidaan.

### <a name="item-level-recovery-with-a-transient-clone"></a>Kohdetason hyödyntämistä lyhytkestoisia Kloonaa

Sinun on yksi vuosi-vanha Microsoft PowerPoint-esitys-Tiedosto Palauta. Järjestelmänvalvoja määrittää tietyn varmuuskopiointi-että ja suodattaa äänenvoimakkuutta. Järjestelmänvalvoja kloonit äänenvoimakkuuden, paikantaa tiedoston, jossa hakua ja antaa sinulle. Tässä skenaariossa lyhytkestoisia Kloonaa käytetään. 
 
![Video käytettävissä](./media/storsimple-clone-volume/Video_icon.png) **Video käytettävissä**

Katso video, jossa esitellään, miten voit käyttää Kloonaa ja palauttaa Palauta poistettuja tiedostoja StorSimple ominaisuuksia, napsauta [tätä](https://azure.microsoft.com/documentation/videos/storsimple-recover-deleted-files-with-storsimple/).

### <a name="testing-in-the-production-environment-with-a-permanent-clone"></a>Pysyvä Kloonaa kanssa tuotantoympäristössä testaaminen

Haluat tarkistaa testauksen ohjelmavirhe tuotantoympäristössä. Voit luoda Kloonaa aseman tuotantoympäristössä tehokkaasta tämän Kloonaa cloud tilannevedoksen. Kloonatun äänenvoimakkuus on nyt riippumatta. Tässä skenaariossa pysyvä Kloonaa käytetään.

## <a name="next-steps"></a>Seuraavat vaiheet
- Lisätietoja palauttaminen [StorSimple aseman varmuuskopion joukosta](storsimple-restore-from-backup-set.md).

- Lue ohjeet [ammattimainen StorSimple laitteen StorSimple hallinta-palvelun käyttöä](storsimple-manager-service-administration.md)varten.

 
