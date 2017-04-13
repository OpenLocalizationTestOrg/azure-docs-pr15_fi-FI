<properties 
   pageTitle="Päivitysten asentaminen StorSimple Virtual matriisin | Microsoft Azure"
   description="Kerrotaan, miten voit käyttää StorSimple Virtual matriisi-web-Käyttöliittymää päivitysten portaalia ja korjaus-menetelmällä"
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
   ms.date="09/07/2016"
   ms.author="alkohli" />

# <a name="install-updates-on-your-storsimple-virtual-array"></a>Päivitysten asentaminen StorSimple Virtual matriisin

## <a name="overview"></a>Yleiskatsaus

Tässä artikkelissa kuvataan toimet, joita tarvitaan päivitysten asentaminen StorSimple Virtual matriisin kautta paikallisen sivuston Käyttöliittymä ja Azure perinteinen portaalin kautta. Haluat käyttää ohjelmistopäivitykset tai korjausten StorSimple Virtual matriisin pitäminen ajan tasalla. 

Ota huomioon, päivitys tai korjaus asennetaan laitteen käynnistyy. Koska, että StorSimple Virtual matriisi on yksi solmu laitetta, minkä tahansa i/o käynnissä keskeytyy ja laitteen ilmenee käyttökatkot. 

Ennen kuin otat käyttöön päivitys, suosittelemme, että tallentumaan asemat- tai offline-tilassa osakkeet isännän ensimmäisen ja valitse haluamasi laite. Tämä pienentää tietovirheitä mahdollisuutta.

> [AZURE.IMPORTANT] Jos käytössäsi on päivitys 0,1 tai GA ohjelmiston versio, asenna päivitys 0,3 käytettävä paikallisen sivuston Käyttöliittymän välityksellä korjaus-menetelmää. Jos käytössäsi on päivitys 0,2, on suositeltavaa, että asennat päivitykset Azure perinteinen portaalin kautta.

## <a name="use-the-local-web-ui"></a>Käytä paikallisen sivuston Käyttöliittymä 
 
On kaksi vaihetta paikallisen sivuston Käyttöliittymä käytettäessä:

- Lataa päivitys tai korjaus
- Päivitys tai korjaus

### <a name="download-the-update-or-the-hotfix"></a>Lataa päivitys tai korjaus

Seuraavien toimien ohjelmistopäivityksen ladataan Microsoft Update-luettelosta.

#### <a name="to-download-the-update-or-the-hotfix"></a>Lataa päivitys tai korjaus

1. Käynnistä Internet Explorer ja siirry [http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).

2. Jos käytät Microsoft Update-luettelon tämän tietokoneen ensimmäistä kertaa, valitse **asentaminen** , kun ohjelma pyytää asentamaan Microsoft Update-luettelosta lisäosa.
  
3. Kirjoita Microsoft Update-luettelosta hakuruutuun haluat ladata korjaus tietämyskannan (kt) määrä. Kirjoita **3182061** päivityksen 0,3 ja valitse sitten **Etsi**.

    Korjaus-luettelossa näkyy esimerkiksi **StorSimple Virtual matriisin päivityksen 0,3**.

    ![Etsi-luettelon](./media/storsimple-ova-install-update-01/download1.png)

4. Valitse **Lisää**. Päivityksen lisätään koriin.

5. Valitse **Näytä kori**.

6. Valitse **Lataa**. Määritä tai **Selaa** paikallisen kohtaa, johon haluat näkyvän lataamisen. Päivitykset ladataan määritettyyn sijaintiin ja sijoittaa alikansion, jolla on sama nimi kuin päivitys. Kansion voi kopioida myös jaettuun verkkoresurssiin, joka on tavoitettavissa laitteesta.

7. Avaamalla kansion, kopioidut, näkyviin tulee Microsoft Update erillinen-pakettitiedosto `WindowsTH-KB3011067-x64`. Tätä tiedostoa käytetään asentaa päivitys tai korjaus.


### <a name="install-the-update-or-the-hotfix"></a>Päivitys tai korjaus

Päivitys -asennusta ennen Varmista, että sinulla on päivitys tai korjaus ladattu joko paikallisesti isännässä tai käytettävissä jaettuun verkon kautta. 

Käytä tätä tapaa päivitysten asentaminen laitteessa GA tai päivittää 0,1 ohjelmistoversiot. Tässä ohjeessa kerrotaan pienempi kuin 2 minuuttia. Seuraavien toimien asentaa päivitys tai korjaus.


#### <a name="to-install-the-update-or-the-hotfix"></a>Asenna päivitys tai korjaus

1. Siirry Käyttöliittymän paikallisen sivuston **ylläpito** > **Ohjelmistopäivitys**.

    ![Päivitä laite](./media/storsimple-ova-install-update-01/update1m.png)

2. Kirjoita **Päivitä tiedostopolku**, päivitys tai korjaus tiedostonimi. Voit myös selata päivitys tai korjaus asennus-tiedostoon, jos jaettuun verkkoresurssiin. Valitse **Käytä**.

    ![Päivitä laite](./media/storsimple-ova-install-update-01/update2m.png)

3.  Näkyviin tulee ilmoitus. Määritetty tämä on yksi solmu laite, kun päivitys on otettu käyttöön, laite käynnistyy ja on käyttökatkot. Valitse Tarkista-kuvake.

    ![Päivitä laite](./media/storsimple-ova-install-update-01/update3m.png)

4. Päivityksen käynnistyy. Kun laite on päivitetty, se käynnistyy. Paikallinen Käyttöliittymän ei ole käytettävissä tämän kesto.

    ![Päivitä laite](./media/storsimple-ova-install-update-01/update5m.png)

5. Uudelleenkäynnistyksen on valmis, kun avaat **Kirjaudu sisään** -sivulla. Voit varmistaa, että Laiteohjelmisto on päivitetty, paikallisen sivuston Käyttöliittymä, siirry **ylläpito** > **Ohjelmistopäivitys**. Näytössä ohjelmiston version on oltava **10.0.0.0.0.10288.0** päivityksen 0,3.

    > [AZURE.NOTE] Olemme raportin ohjelmistoversiot hieman eri tavalla paikallisen sivuston Käyttöliittymä ja Azure perinteinen portaalin. Esimerkiksi paikallisen sivuston Käyttöliittymä raportoi **10.0.0.0.0.10288** ja Azure perinteinen portaalin raporteissa **10.0.10288.0** saman version. 

    ![Päivitä laite](./media/storsimple-ova-install-update-01/update6m.png)





## <a name="use-the-azure-classic-portal"></a>Perinteinen Azure-portaalin käyttäminen

Jos käynnissä päivityksen 0,2, on suositeltavaa, että asennat päivitykset Azure perinteinen portaalin kautta. Portaalin toimintosarja edellyttää, että käyttäjä voi tarkistaa, lataa ja asenna päivitykset. Tässä ohjeessa kerrotaan 7 minuuttia suorittamiseen. Seuraavien toimien asentaa päivitys tai korjaus.

[AZURE.INCLUDE [storsimple-ova-install-update-via-portal](../../includes/storsimple-ova-install-update-via-portal.md)]

Kun asennus on valmis (sellaisena kuin tilan 100 %), siirry osoitteeseen **laitteiden > ylläpito > ohjelmistopäivitykset**. Näytössä ohjelmiston version on oltava 10.0.10288.0.

![Päivitä laite](./media/storsimple-ova-install-update-01/azupdate12m.png)

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja [hallinnasta StorSimple Virtual matriisin](storsimple-ova-web-ui-admin.md).
