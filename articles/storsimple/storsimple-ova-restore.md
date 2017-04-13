<properties
   pageTitle="Palauttaminen varmuuskopiosta StorSimple Virtual matriisin"
   description="Lisätietoja palauttaminen varmuuskopiosta StorSimple Virtual matriisin."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor=""/>

<tags
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="06/07/2016"
   ms.author="alkohli"/>

# <a name="restore-from-a-backup-of-your-storsimple-virtual-array"></a>Palauttaminen varmuuskopiosta StorSimple Virtual matriisin

## <a name="overview"></a>Yleiskatsaus 

Tämä artikkeli koskee Microsoft Azure StorSimple Virtual matriisi (tunnetaan myös nimellä StorSimple paikallisen virtual laitteen tai StorSimple virtual laite) käynnissä maaliskuussa 2016 yleiseen käyttöön (GA)-versiossa tai sitä uudempaan versioon. Tässä artikkelissa kuvataan vaiheittaiset palauttaminen osakkeet tai StorSimple Virtual matriisin asemista varmuuskopion joukosta. Artikkelin tiedot myös toiminnasta Kohdetason palautus StorSimple Virtual matriisin, joka on määritetty tiedostopalvelimeen.


## <a name="restore-shares-from-a-backup-set"></a>Palauttaa osakkeet varmuuskopioinnin määrittäminen


**Ennen kuin yrität palauttaa osakkeet, varmista, että sinulla on tarpeeksi tilaa laitteeseen tätä toimintoa varten.** Seuraavien toimien avulla palauttaminen varmuuskopiosta [Azure perinteinen portal](https://manage.windowsazure.com/).

#### <a name="to-restore-a-share"></a>Jos haluat palauttaa Jaa

1.  Siirry **varmuuskopion luettelon**. Suodattaa sopiva laite ja Etsi varmuuskopiot aikavälin. Valitse tarkistuksen kuvake ![](./media/storsimple-ova-restore/image1.png) kyselyn suorittaminen.


1.  Valitse varmuuskopioinnin joukot näkyviin luettelo ja valitse tietyn varmuuskopion. Laajenna varmuuskopioinnin Nähdäksesi eri osakkeet alla. Valitse ja valitse Jaa, jonka haluat palauttaa.

2.  Sivun alareunassa valitsemalla **Palauta uutena**.

3.  Tämä aloittaa **uuden Jaa palauttaa** ohjatun toiminnon. **Määritä nimi ja sijainti** -sivulla:


    1.  Vahvista tietolähteen laitteen nimi. Tämä on oltava laitteella, joka sisältää palautettavan Jaa. Laitteen valinta näkyy harmaana. Jos haluat valita eri lähde-laite, sinun on Sulje ohjattu toiminto ja valitse varmuuskopioinnin määrittäminen uudelleen.

    2.  Anna Jaa nimi. Jaa-nimen on oltava 3-127 merkkiä.

    3.  Tarkista kokoa, tyyppi ja jaa, joka palauttaa rooliin liittyvät oikeudet. Osaat muokata kautta Resurssienhallinnassa Jaa-ominaisuuksia, kun palautus on valmis.

    4.  Valitse tarkistuksen kuvake ![](./media/storsimple-ova-restore/image1.png).

        ![](./media/storsimple-ova-restore/image9.png)

1.  Kun palautustyön on valmis, Palauta käynnistyy ja näet toisen ilmoituksen. Voit valvoa edistymistä palauttaminen, valitsemalla **Näytä työ**. Tällöin näyttöön **työt** -sivu.

2.  Voit seurata edistymistä palautustyön. Kun palautus on 100 % valmiina, siirtyä takaisin laitteen **osakkeet** -sivu.

3.  Voit tarkastella jaettujen resurssien luetteloa uusi palautettu Jaa nyt laitteessasi. Huomaa, että palautus on valmis Jaa samaa tietotyyppiä. Porrastettu Jaa palautetaan kuin tasoisen ja paikallisesti kiinnitetty jaetuksi paikallisesti kiinnitetty jakaminen.

Olet valmis laitteen määrityksiä ja oppinut varmuuskopiointia tai palauta jaettuun. 


## <a name="restore-volumes-from-a-backup-set"></a>Palauttaa tietomääristä varmuuskopioinnin määrittäminen


Seuraavien toimien avulla palauttaminen varmuuskopiosta Azure perinteinen portaalissa. Palautustoiminto palauttaa varmuuskopion uuden aseman samaan virtual laitteeseen; Et voi palauttaa toiseen laitteeseen.

#### <a name="to-restore-a-volume"></a>Jos haluat palauttaa aseman

1.  Siirry **varmuuskopion luettelon**. Suodattaa sopiva laite ja Etsi varmuuskopiot aikavälin. Valitse tarkistuksen kuvake ![](./media/storsimple-ova-restore/image1.png) kyselyn suorittaminen.

2.  Valitse varmuuskopioinnin joukot näkyviin luettelo ja valitse tietyn varmuuskopion. Laajenna varmuuskopioinnin näet alla eri asemat. Valitse palautettavan äänenvoimakkuutta. 

5.  Sivun alareunassa valitsemalla **Palauta uutena**. **Palauttaa uusi asema** ohjattu toiminto käynnistyy.

1.  **Määritä nimi ja sijainti** -sivulla:


    1.  Vahvista tietolähteen laitteen nimi. Tämä on oltava laitteella, joka sisältää asema, jonka haluat palauttaa. Laitteen valinta ei ole käytettävissä. Jos haluat valita eri lähde-laite, sinun on Sulje ohjattu toiminto ja valitse varmuuskopioinnin määrittäminen uudelleen.

    2.  Anna levyn nimi levyn uudeksi palautetaan parhaillaan. Nimen on oltava 3-127 merkkiä.

    3.  Valitse napsauttamalla nuolta.

        ![](./media/storsimple-ova-restore/image12.png)

1.  **Määritä isännät, jotka käyttävät tätä asemaa** sivulla Valitse haluamasi ACRs avattavasta luettelosta.

    ![](./media/storsimple-ova-restore/image13.png)

1.  Valitse tarkistuksen kuvake ![](./media/storsimple-ova-restore/image1.png). Tämä aloittaa palautustyön ja näet seuraavan ilmoituksen, työ on käynnissä.

2.  Kun palautustyön on valmis, Palauta käynnistyy ja näet toisen ilmoituksen. Voit valvoa edistymistä palauttaminen, valitsemalla **Näytä työ**. Tällöin näyttöön **työt** -sivu.

3.  Voit seurata edistymistä palautustyön. Siirry takaisin laitteen **asemat** -sivulle.

4.  Nyt voit tarkastella uusi palautettu asema asemat luettelo laitteen. Huomaa, palautus on valmis aseman samaa tietotyyppiä. Porrastettu aseman palautetaan kuin tasoisen ja paikallisesti kiinnitetty aseman palautetaan kuin paikallisesti kiinnitetty äänenvoimakkuutta.

5.  Kun äänenvoimakkuuden näkyy online luettelon asemista, asema ei voi käyttää.  Päivitä isännässä iSCSI käynnistäjä tavoitteiden iSCSI käynnistäjä ominaisuudet-ikkunassa.  Uusi kohde, jossa on palautettu nimen pitäisi näkyä 'passiiviseksi'-tila-sarakkeessa.

6.  Valitse kohde ja valitse **Yhdistä**.   Kun kohde on liitetty käynnistäjä, tila vaihdetaan **yhteys on muodostettu**. 

7.  **Levynhallinta** -ikkunassa liitetyn asemat näkyvät seuraavassa kuvassa esitetyllä tavalla. Hiiren kakkospainikkeella havaitun äänenvoimakkuuden (Valitse levyn nimi), ja valitse sitten **online-tilassa**.

> [AZURE.IMPORTANT] Yritettäessä aseman tai jaettuun palauttaminen varmuuskopiosta Määritä palautustyön epäonnistuu, jos kohde äänenvoimakkuutta tai Jaa edelleen voi luoda portaalissa. On tärkeää, että Poista kohde-asema tai jakaa portaalissa Pienennä tulevien ongelmat johtuvat tämä elementti.

## <a name="item-level-recovery-ilr"></a>Kohdetason palauttaminen (ILR)

Tässä versiossa esitellään Kohdetason palauttaminen (ILR) määritetty tiedostopalvelimeen StorSimple virtual laitteessa. Ominaisuuden avulla voit tehdä hajautetun palautus tiedostojen ja kansioiden StorSimple laitteeseen osakkeet cloud varmuuskopion. Poistettujen tiedostojen voit noutaa viimeisimmät varmuuskopiot Omatoiminen mallin avulla.

Jokaisen Jaa on *.backups* kansio, joka sisältää uusimmat varmuuskopiot. Käyttäjä voi Siirry haluamaasi varmuuskopiointi liittyvien tiedostojen ja kansioiden kopioiminen varmuuskopion ja palauttaa ne. Näin järjestelmänvalvojat puhelut, tiedostojen palauttaminen varmuuskopioista.

1.  Suoritettaessa ILR voit tarkastella varmuuskopioiden Resurssienhallinnan avulla. Valitse tietyn Jaa, jota haluat tarkastella varmuuskopioinnin varten. Valitse Jaa, joka sisältää kaikki varmuuskopiot luoda *.backups* kansion tulee näkyviin. Laajenna *.backups* -kansio ja tarkastele varmuuskopiot. Kansio Valitse näkyy koko varmuuskopion hierarkian purettu-näkymään. Tässä näkymässä luodaan tarvittaessa ja yleensä kestää vain muutaman sekunnin luomiseen.

    Viimeksi 5 varmuuskopiot näkyvät tällä tavalla ja voidaan käyttää Kohdetason palauttamisen. 5 viimeisimmät varmuuskopiot ovat ajoitettu oletusarvo ja manuaalinen varmuuskopiot.

    
    -   **Ajoitettu varmuuskopioiden** nimennyt sinut &lt;laitteen nimi&gt;DailySchedule-VVVVKKPP-TTMMSS-UTC-aika.

    -   **Manuaalinen varmuuskopiointi** nimeltä Ad hoc-VVVVKKPP-TTMMSS-UTC-muodossa.
    
        ![](./media/storsimple-ova-restore/image14.png)

1.  Määritä varmuuskopion poistetun tiedoston uusimman version. Vaikka kansion nimessä on UTC-aikaleima yllä tapauksissa, kellonaika, jolla kansiossa on luotu on todellinen laite-aika, jolloin varmuuskopiointi alkaa. Kansion aikaleima avulla voit paikantaa ja tunnistaa varmuuskopioista.

2.  Etsi kansion tai tiedoston, jonka haluat palauttaa varmuuskopion, voit tunnistaa edellisessä vaiheessa. Huomautus Voit tarkastella vain tiedostot tai kansiot, joihin sinulla on oikeudet. Jos et pysty käyttämään tiettyjä tiedostoja tai kansioita, sinun on yhteyttä Jaa-järjestelmänvalvoja, joka käyttää Resurssienhallinnan avulla voit käyttää yksittäinen tiedosto tai kansio ja jaa muokkausoikeudet. Jaa-järjestelmänvalvoja voi sen sijaan, että sama käyttäjä käyttäjäryhmä suositellut paras käytäntö on.

3.  Kopioi tiedosto tai kansio tarvittavat Jaa StorSimple tiedosto palvelimeen.

![video_icon](./media/storsimple-ova-restore/video_icon.png) **Video käytettävissä**

Katso video ja katso, miten voit luoda osakkeet, osakkeet varmuuskopiointi ja palauttaminen StorSimple Virtual taulukon tiedot.

> [AZURE.VIDEO use-the-storsimple-virtual-array]

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja ammattimainen [Käyttäminen paikallisen sivuston Käyttöliittymä StorSimple Virtual matriisin](storsimple-ova-web-ui-admin.md).
