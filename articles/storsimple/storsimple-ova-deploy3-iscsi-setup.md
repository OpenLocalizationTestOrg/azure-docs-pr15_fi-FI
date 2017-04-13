<properties 
   pageTitle="StorSimple Virtual matriisin iSCSI palvelinasetukset | Microsoft Azure"
   description="Tässä artikkelissa käsitellään suorittaa ensimmäisen määrityskerran ja rekisteröi StorSimple iSCSI palvelimen laitteen asennuksen."
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
   ms.date="07/18/2016"
   ms.author="alkohli" />


# <a name="deploy-storsimple-virtual-array--set-up-your-virtual-device-as-an-iscsi-server"></a>Ottaa käyttöön StorSimple Virtual matriisin – iSCSI palvelimeksi virtual laitteen määrittäminen

![iSCSI asennuksen prosessinkulku](./media/storsimple-ova-deploy3-iscsi-setup/iscsi4.png)

## <a name="overview"></a>Yleiskatsaus

Tässä opetusohjelmassa käyttöönoton koskee Microsoft Azure StorSimple Virtual matriisi (tunnetaan myös nimellä StorSimple paikallisen virtual laitteen tai StorSimple virtual laite) käynnissä maaliskuussa 2016 yleiseen käyttöön (GA)-versiossa. Tässä opetusohjelmassa kerrotaan, miten suorittaa ensimmäisen määrityskerran rekisteröidä StorSimple iSCSI palvelimellesi, laitteen asennuksen, ja sitten luominen, ota käyttöön, alusta ja alusta asemat StorSimple virtual laitteen iSCSI palvelimeen. StorSimple määrittämisestä on artikkelissa tämän artikkelin koskee vain StorSimple Virtual matriiseja. 

Kuvattujen toimintojen kestää noin 30 minuuttia tähän suorittamiseen 1 tunti. Julkaistu tämän artikkelin tiedot koskevat vain StorSimple Virtual matriiseja.

## <a name="setup-prerequisites"></a>Asetukset

Ennen kuin määrittäminen ja StorSimple virtual laitteen määrittäminen, varmista, että:

- Sinulla on valmisteltu virtual laitteen ja yhdistetty kuvatulla tavalla [Käyttöönotto StorSimple Virtual matriisi - säännöstä virtual matriisin Hyper - v](storsimple-ova-deploy2-provision-hyperv.md) tai [Ottaa käyttöön StorSimple Virtual matriisi - säännöstä virtual matriisi-VMware](storsimple-ova-deploy2-provision-vmware.md).

- Sinun on StorSimple hallinta-palvelusta, jonka loit StorSimple virtual laitteiden hallinta palvelun rekisteröinti-näppäintä. Lisätietoja on artikkelissa **Vaihe 2: hankkia palvelun rekisteröinti avain** [käyttöön StorSimple Virtual](storsimple-ova-deploy1-portal-prep.md#step-2-get-the-service-registration-key)matriisissa - portaalin valmisteleminen.

- Jos kyseessä on toinen tai myöhempi virtual laitteella, joka on rekisteröiminen StorSimple hallinta-palvelussa, taulukossa pitäisi olla palvelun tiedot salausavaimen. Tämä avain on luotu, kun ensimmäinen laite on rekisteröitynyt tämän palvelun avulla. Jos tätä näppäintä on katkennut, katso **Hae palvelun tiedot salausavaimen** [hallinnasta StorSimple Virtual matriisin Web-Käyttöliittymän käyttöä](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key).

## <a name="step-by-step-setup"></a>Vaiheittaisesta määrittämisestä 

Seuraavien vaiheittaisten ohjeiden avulla voit StorSimple virtual laitteen määrittäminen:

-  [Vaihe 1: Asennuksen paikallisen sivuston Käyttöliittymä ja rekisteröi laite](#step-1-complete-the-local-web-ui-setup-and-register-your-device)
-  [Vaihe 2: Viimeistele tarvittavat laitteen määrittäminen](#step-2-complete-the-required-device-setup)
-  [Vaihe 3: Lisää aseman](#step-3-add-a-volume)
-  [Vaihe 4: Ota käyttöön, alusta ja muotoilla aseman](#step-4-mount-initialize-and-format-a-volume)  

## <a name="step-1-complete-the-local-web-ui-setup-and-register-your-device"></a>Vaihe 1: Asennuksen paikallisen sivuston Käyttöliittymä ja rekisteröi laite 

#### <a name="to-complete-the-setup-and-register-the-device"></a>Rekisteröi laite ja Viimeistele määritykset

1. Avaa selain ja yhdistäminen web-Käyttöliittymää kirjoittamalla:

    `https://<ip-address of network interface>`

    Käytä edellisessä vaiheessa yhteyden URL-Osoitetta. Näet virhesanoman, jossa ilmoitetaan, että on sivuston suojausvarmenteessa ongelma. Valitse **Jatka web-sivulle**.

    ![suojauksen varmenteen virhe](./media/storsimple-ova-deploy3-iscsi-setup/image3.png)

2. Kirjaudu web-Käyttöliittymää virtual laitteesi **StorSimpleAdmin**. Kirjoita laitteen järjestelmänvalvojan salasana, jotka olet muuttanut vaihe 3: Käynnistä virtual laite [Käyttöönotto StorSimple Virtual matriisi - säännöstä Hyper-V virtual laitteeseen](storsimple-ova-deploy2-provision-hyperv.md) tai [Ottaa käyttöön StorSimple Virtual matriisin - säännöstä VMware virtual laitteeseen](storsimple-ova-deploy2-provision-vmware.md).

    ![Kirjautumissivu](./media/storsimple-ova-deploy3-iscsi-setup/image4.png)

3. Siirryt **Aloitussivu** . Tällä sivulla kerrotaan eri asetukset, joita tarvitaan ja rekisteröidä virtual laitteen StorSimple hallinta-palveluun. Huomaa, että **verkkoasetukset**, **Web-välityspalvelimen**ja **aika-asetukset** ovat valinnaisia. **Laiteasetusten** ja **Cloud asetukset**ovat vain tarvittavat asetukset.

    ![Aloitussivu](./media/storsimple-ova-deploy3-iscsi-setup/image5.png)

4. Valitse **verkkoliittymät** **verkkoasetukset** -sivulla tiedot 0 määritetään automaattisesti puolestasi. Kunkin verkkoliittymän on määritetty oletusarvoisesti hakea IP-osoitteen automaattisesti (DHCP). Tämän vuoksi IP-osoite, aliverkon ja yhdyskäytävän automaattisesti määritetään (varten sekä IPv4- että IPv6).

    Suunnitellessasi ottamaan laitteen iSCSI-palvelimeksi (voit valmistella estä tallennustilan), on suositeltavaa **Hae IP-osoite automaattisesti** poistaminen käytöstä ja staattinen IP-osoitteiden määrittäminen.

    ![Verkkoasetukset-sivulle](./media/storsimple-ova-deploy3-iscsi-setup/image6.png)

    Jos olet lisännyt useita verkkoliittymän laitteen valmistelun aikana, voit määrittää ne tähän. Huomautus Voit määrittää verkkoliittymän IPv4 kuin vain tai IPv4- ja IPv6 nimellä. Vain IPv6-määrityksiä ei tueta.

5. DNS-palvelimet tarvitaan, koska niitä käytetään, kun laite yrittää pitää yhteyttä cloud-tallennustilan palveluntarjoajien tai jos se on määritetty tiedostopalvelimessa, ratkaise laitteen nimi. **DNS-palvelimet**-kohdassa **verkkoasetukset** -sivulla:

    1. Ensisijaisen ja toissijaisen DNS-palvelin määritetään automaattisesti. Jos haluat määrittää staattinen IP-osoitteet, voit määrittää DNS-palvelimet. Suuren käytettävyyden Suosittelemme, että määrität ensisijainen ja Toissijainen DNS-palvelin.

    2. Valitse **Käytä**. Tämä Käytä ja Tarkista verkkoasetukset.

6. **Laitteen asetukset** -sivulla:

    1. Määritä laitteen yksilöivä **nimi** . Tämä nimi voi olla 1-15 merkkiä, ja ne voivat sisältää kirjain, numeroita ja väliviivoja.

    2. **ISCSI server** -kuvaketta ![iSCSI palvelimen kuvake](./media/storsimple-ova-deploy3-iscsi-setup/image7.png) laite, jota olet luomassa **tyyppi** . ISCSI-palvelimen avulla voit säätää estä säilöön.

    3. Määrittää, haluatko laite on toimialueeseen liittymistä. Jos laite on iSCSI-palvelin, valitse toimialueeseen on valinnainen. Jos päätät Yhdistä iSCSI palvelimen toimialueeseen, valitse **Käytä**, odota asetuksia voi suojata ja siirry seuraavaan vaiheeseen.

        Jos haluat liittää laite toimialueelle. Kirjoita **toimialuenimi**ja valitse sitten **Käytä**.

        > [AZURE.NOTE] Jos liittyminen iSCSI palvelimen toimialueeseen, varmista, että virtual matriisi on oma organisaatioyksikkö (OU) Microsoft Azure Active Directory eivätkä ole ryhmäkäytännön objekteja (Ryhmäkäytäntöobjekti) on käytetty.

    5. Valintaikkuna tulee näkyviin. Kirjoita toimialueen tunnistetiedot määritetyssä muodossa. Valitse Tarkista-kuvake ![Tarkista kuvake](./media/storsimple-ova-deploy3-iscsi-setup/image15.png). Toimialueen tunnistetietoja tarkistetaan. Näyttöön tulee virhesanoma, jos tunnistetiedot ovat virheellisiä.

        ![tunnistetiedot](./media/storsimple-ova-deploy3-iscsi-setup/image8.png)

    6. Valitse **Käytä**. Tämä Käytä ja tarkista Laiteasetukset.
 
7. Web-välityspalvelimen määrittäminen (valinnainen). Web-välityspalvelimen määritys on valinnainen, mutta huomioon, että jos käytät web-välityspalvelimen, voit vain määrittää sen tähän.

    ![web-välityspalvelimen asetusten määrittäminen](./media/storsimple-ova-deploy3-iscsi-setup/image9.png)

    **Web-välityspalvelimen** -sivulla:

    1. Anna **välityspalvelimen WWW-URL-osoite** tässä muodossa: *http://host-IP osoite* tai *FDQN:Port numero*. Huomaa, että HTTPS URL-osoitteita ei tueta.

    2. Määritä **todennus** **Basic** tai **ei mitään**.

    3. Jos käytössäsi on todennus, sinun on myös **käyttäjänimeä** ja **salasanaa**.

    4. Valitse **Käytä**. Tämä Vahvista ja käyttää määritetyn web välityspalvelimen asetuksia.
 
8. Laitteen, kuten aikavyöhykkeen ja ensisijaisen ja toissijaisen NTP-palvelimien aika-asetusten määrittäminen (valinnainen). NTP-palvelimien tarvitaan, koska laite on aikaa, niin, että se todentamismenetelmä cloud palveluntarjoajia.

    ![Aika-asetukset](./media/storsimple-ova-deploy3-iscsi-setup/image10.png)

    **Aika-asetukset** -sivulla:

    1. Valitse avattavasta luettelosta **aikavyöhyke** , jossa laitteen otetaan käyttöön maantieteellisen sijainnin perusteella. Laitteen Oletusaikavyöhyke on pst-tiedoston. Laitteen käyttää tätä aikavyöhykkeen ajoitetun kaikki toiminnot.

    2. **Ensisijainen NTP server** laitteeseesi tai hyväksy time.windows.com oletusarvon. Varmista verkon sallii NTP-liikenne paikalliseen välittää Internet-yhteyttä palvelinkeskukseen.

    3. Voit myös määrittää **toissijaisen NTP server** laitteeseesi.

    4. Valitse **Käytä**. Tämä Vahvista ja käyttää määritetyt aika-asetuksia.

9. Laitteen cloud-asetusten määrittäminen. Tässä vaiheessa Viimeistele määritys paikallisen laitteen ja rekisteröi sitten laitteen StorSimple hallinta-palvelun kanssa.

    1. Kirjoita **palvelun rekisteröinti avainta** , jotka olet saanut- **Vaihe 2: hankkia palvelun rekisteröinti avain** [käyttöön StorSimple Virtual](storsimple-ova-deploy1-portal-prep.md#step-2-get-the-service-registration-key)matriisissa - portaalin valmisteleminen.

    2. Jos tämä ei ole ensimmäisen laitteella, joka on rekisteröimistä tämä palvelu, sinun on **palvelun tiedot salausavaimen**. Tämä avain tarvitaan muita laitteita rekisteröityä StorSimple hallintapalvelu palvelun rekisteröinti-näppäimen kanssa. Saat lisätietoja viitata saat [palvelun tiedot salausavaimen](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key) paikallisen sivuston Käyttöliittymä.

    3. Valitse **Rekisteröi**. Tämä käynnistää laite. Voit joutua odottamaan 2 – 3 minuuttia, ennen kuin laitteen rekisteröinti onnistuu. Laite on käynnistettävä uudelleen, kun siirryt Kirjaudu sisään-sivulla.

       ![Rekisteröi laite](./media/storsimple-ova-deploy3-iscsi-setup/image11.png)

10. Palaa perinteiseen Azure-portaaliin. Varmista **laitteet** -sivulla, että laite on muodostanut yhteyden palvelun etsimällä tila. Laitteen tila on oltava **aktiivinen**.

    ![Laitteet-sivu](./media/storsimple-ova-deploy3-iscsi-setup/image12.png)

## <a name="step-2-complete-the-required-device-setup"></a>Vaihe 2: Viimeistele tarvittavat laitteen määrittäminen

Suorittamaan StorSimple laitteen laite-määritys on:

- Valitse liitettävä laitteen tallennustila-tili.

- Valitse tiedot, jotka on lähetetty cloud salausasetukset.

Seuraavien toimien tarvittava laite-asennuksen viimeistelemiseen Azure perinteinen-portaalissa.

#### <a name="to-complete-the-minimum-device-setup"></a>Pienin laite-asennuksen viimeistelemiseen

1. Valitse **laitteet** -sivulla Valitse juuri luomasi laite. Tämä laite näkyvät **aktiiviseksi**. Napsauta nuolta laitteen nimen vieressä ja valitse sitten **Pika-aloitus**.

    ![Laitteet-sivu](./media/storsimple-ova-deploy3-iscsi-setup/image13.png)

2. Valitse Käynnistä ohjattu laitteen määrittäminen **Valmis laitteen asennus** .

    ![Ohjattu laitteen määrittäminen](./media/storsimple-ova-deploy3-iscsi-setup/image14.png)

3. Valitse Määritä laitteen ohjatun **Perusasetukset** -sivulla seuraavasti:

   1. Määritä tallennustilan-tili, jota käytetään laitteen kanssa. Voit valita avattavasta luettelosta käytössä olevan tallennustilan tilin tähän tilaukseen tai voit määrittää **Lisää** tili valittavana eri tilauksen.

   2. Määritä kaikille muille käyttäjille, jotka lähetetään pilveen tiedoissa salauksen asetukset. (StorSimple käyttää AES 256 salausta.) Salaa tiedot, valitse **Enable cloud tallennustilan salaus** -valintaruutu. Kirjoita cloud tallennustilan salausta, joka sisältää 32 merkkiä. Vahvista kirjoittamalla se sitten-näppäintä.

   3. Valitse Tarkista-kuvake ![Tarkista kuvake](./media/storsimple-ova-deploy3-iscsi-setup/image15.png).

    ![Perusasetukset](./media/storsimple-ova-deploy3-iscsi-setup/image16.png)

    Asetukset päivitetään. Kun asetukset on päivitetty, täydellinen laitteen asetukset-painike ei ole käytettävissä. Voit palauttaa laitteen **Pika-aloitus** -sivulle.                                                        

>[AZURE.NOTE]Voit muokata muita laiteasetusten milloin tahansa muodostamalla yhteyden **määrittäminen** -sivulla.

## <a name="step-3-add-a-volume"></a>Vaihe 3: Lisää aseman

Seuraavien toimien aseman luominen Azure perinteinen-portaalissa.

#### <a name="to-create-a-volume"></a>Aseman luominen

1. Valitse laitteen **Pika-aloitus** -sivulla **Lisää aseman**. Lisää äänenvoimakkuutta ohjattu toiminto käynnistyy.

2. Lisää äänenvoimakkuutta ohjatun toiminnon **Perusasetukset**-kohdassa seuraavasti:

    1. Lisää äänenvoimakkuutta yksilöllinen nimi. Nimen on oltava merkkijono, joka sisältää 3-127 merkkiä.

    2. Ongelman kuvaus aseman. Kuvaus tunnistaa aseman omistajat.

    3. Valitse käyttö aseman. Käyttötyyppi voi olla **Tiered äänenvoimakkuutta** tai **kiinnitetty paikallisesti äänenvoimakkuutta.** (**Tiered äänenvoimakkuus** on oletusarvo.) Valitse toiminnoista, jotka edellyttävät paikallisen oikeudet, pieni viiveet suurempia ja paremman suorituskyvyn, **paikallisesti kiinnitetyt** **äänenvoimakkuutta**. Jos kaikki muut tiedot, valitse **Tiered** **äänenvoimakkuutta**.

        Paikallisesti kiinnitetty äänenvoimakkuuden thickly valmisteltu ja varmistaa, että äänenvoimakkuuden ensisijainen tietojen pysyy laitteessa ja ei spill pilveen. Jos luot paikallisesti kiinnitetty äänenvoimakkuutta, laite tarkistaa paikallisen tasoa valmistelu pyydetty koon tilavuus vapaata tilaa. Paikallisesti kiinnitetty aseman luominen voi vaatia spilling olemassa olevat tiedot laitteesta pilvipalveluun ja luoda äänenvoimakkuuden aika voi olla pitkä. Kokonaisaika määräytyy valmistellun äänenvoimakkuuden käytettävissä kaistanleveys ja laitteen tiedot koon.

        Porrastettu aseman toisaalta levinneet valmisteltu ja voidaan luoda nopeasti. Kun luot Porrastettu äänenvoimakkuutta, noin 10 prosenttia tila on valmisteltu paikallinen ylätasolla ja 90 prosenttia tila on valmisteltu pilveen. Esimerkiksi jos 1 TT aseman valmistelun yhteydessä, 100 Gigatavua sijaitsi paikallisen tilaa ja 900 gt voi käyttää pilveen kun tietojen tasoa. Tämä tarkoittaa puolestaan on, jos olet käyttänyt kaikki paikallisen loppumassa laitteeseen, ei voi valmistella Porrastettu Jaa (koska 10 % ei ole käytettävissä).

    4. Määritä äänenvoimakkuutta valmistellun kapasiteetti. Huomaa, että määritetyn kapasiteetin on oltava pienempi kuin käytettävissä oleva kapasiteetti. Jos olet luomassa Porrastettu äänenvoimakkuutta, kokoa on oltava välillä 500 Gigatavua ja 5 Teratavua. Määritä paikallisesti kiinnitetty aseman äänenvoimakkuuden koko välillä 50 Gigatavun ja 500 Gigatavua. Käytä käytettävissä olevan kapasiteetin ohjeena valmistelu aseman. Jos käytettävissä paikallisten kapasiteetti on 0 gt, jota ei sallita valmistelu paikallisesti kiinnitetty tai Porrastettu määrän.

        ![Perusasetukset](./media/storsimple-ova-deploy3-iscsi-setup/image17.png)

    5. Napsauttamalla nuolta ![nuolikuvaketta](./media/storsimple-ova-deploy3-iscsi-setup/image18.png) Siirry seuraavalle sivulle.

3. Valitse **Lisäasetukset** -sivulla Lisää uusi access-ohjausobjektin tietue (ACR):

    1. Anna **nimi** , että ACR.

    2. Ilmoita **iSCSI käynnistäjä nimi**-kohdassa iSCSI koko nimi (IQN) Windows-isännän. Jos sinulla ei ole IQN, siirry [lisäys A: Get Windows Server host IQN](#appendix-a-get-the-iqn-of-a-windows-server-host).

    3. On suositeltavaa ottaa oletusarvon varmuuskopion valitsemalla **tämän aseman oletusarvon varmuuskopion Ota käyttöön** -valintaruutu. Oletus-varmuuskopiointi luo käytännön, joka suorittaa 22:30 päivittäin (laitteen aika)-palvelussa ja luo aseman cloud tilannevedoksen.

        ![Lisäasetukset](./media/storsimple-ova-deploy3-iscsi-setup/image19.png)

    4. Valitse Tarkista-kuvake ![Tarkista kuvake](./media/storsimple-ova-deploy3-iscsi-setup/image15.png). Työn aseman luominen käynnistyy. Näet seuraavankaltaiselta edistymisen viestin.

        ![edistymisen viesti](./media/storsimple-ova-deploy3-iscsi-setup/image20.png)

        Asema luodaan määritetyn asetusten kanssa. Oletusarvon mukaan seuranta- ja varmuuskopiointi on käytettävissään äänenvoimakkuutta.

    5. Vahvista äänenvoimakkuuden luotiin, siirry **asemat** -sivulle. Raportissa pitäisi näkyä luettelossa äänenvoimakkuutta.

        ![](./media/storsimple-ova-deploy3-iscsi-setup/image21.png)

## <a name="step-4-mount-initialize-and-format-a-volume"></a>Vaihe 4: Ota käyttöön, alusta ja muotoilla aseman

Seuraavien toimien avulla käyttöön, alusta ja muotoilla StorSimple-asemista Windows Server host.

#### <a name="to-mount-initialize-and-format-a-volume"></a>Ottaa käyttöön, alusta ja aseman alustaminen

1. Käynnistä Microsoft iSCSI-käynnistäjä.

2. Valitse **iSCSI käynnistäjä ominaisuudet** -ikkunassa **Etsintä** -välilehden **Tutustu Portal**.

    ![Tutustu portal](./media/storsimple-ova-deploy3-iscsi-setup/image22.png)

3. **Tutustu kohde Portal** -valintaikkunassa Anna iSCSI käyttävä verkkoliittymän IP-osoite ja valitse sitten **OK**.

    ![IP-osoite](./media/storsimple-ova-deploy3-iscsi-setup/image23.png)

4. Etsi **Discovered kohteiden** **iSCSI käynnistäjä ominaisuudet** -ikkunassa **kohteet** -välilehdessä. (Kunkin äänenvoimakkuus on havaitun target.) Laitteen tila pitäisi näkyä **passiiviseksi**.

    ![havaitun kohteet](./media/storsimple-ova-deploy3-iscsi-setup/image24.png)

5. Valitse Kohdelaite ja valitse **Yhdistä**. Kun laite on yhdistetty, tila vaihdetaan **yhteys on muodostettu**. (Katso lisätietoja käyttämisestä Microsoft iSCSI-käynnistäjä [asentaminen ja määrittäminen Microsoft iSCSI käynnistäjä] [1].

    ![Valitse Kohdelaite](./media/storsimple-ova-deploy3-iscsi-setup/image25.png)

6. Windows-isännän Paina Windows-näppäin + X ja valitse sitten **Suorita**.

7. Kirjoita **Suorita** -valintaikkunaan **Diskmgmt.msc**. Valitse **OK**ja **Levynhallinta** -valintaikkuna tulee näkyviin. Oikeanpuoleisessa ruudussa näkyy asemat isäntä.

8. **Levynhallinta** -ikkunassa liitetyn asemat näkyvät seuraavassa kuvassa esitetyllä tavalla. Hiiren kakkospainikkeella havaitun äänenvoimakkuuden (Valitse levyn nimi), ja valitse sitten **online-tilassa**.

    ![Levynhallinta](./media/storsimple-ova-deploy3-iscsi-setup/image26.png)

9. Napsauta hiiren kakkospainikkeella ja valitse **Alusta levy**.

    ![alusta levy 1](./media/storsimple-ova-deploy3-iscsi-setup/image27.png)

10. Valitse-valintaikkunassa valitse suorittamiseksi alusta ja valitse sitten **OK**.

    ![alusta levy 2](./media/storsimple-ova-deploy3-iscsi-setup/image28.png)

11. Uusi tavallinen asema ohjattu toiminto käynnistyy. Valitse levyn koko ja valitse sitten **Seuraava**.

    ![Ohjattu 1-aseman](./media/storsimple-ova-deploy3-iscsi-setup/image29.png)

12. Liittää kirjaimen asemaan ja valitse sitten **Seuraava**.

    ![Ohjattu 2-aseman](./media/storsimple-ova-deploy3-iscsi-setup/image30.png)

13. Kirjoita alusta asema parametrit. **Windows Server tuetaan vain NTFS.** Määritä Australia 64 kilotavua. Säätää äänenvoimakkuutta otsikko. Se on tämä nimi on sama kuin antamasi StorSimple virtual laitteen nimen suositellut parhaat käytännöt. Valitse **Seuraava**.

    ![Ohjattu 3-aseman](./media/storsimple-ova-deploy3-iscsi-setup/image31.png)

14. Tarkista äänenvoimakkuutta arvot ja valitse sitten **Valmis**.

    ![Ohjattu 4-aseman](./media/storsimple-ova-deploy3-iscsi-setup/image32.png)

    Asemat näkyvät **Online** **Levynhallinta** -sivulla.

    ![asemat verkossa](./media/storsimple-ova-deploy3-iscsi-setup/image33.png)

## <a name="next-steps"></a>Seuraavat vaiheet

Opettele käyttämään paikallisen sivuston Käyttöliittymä ammattimainen [StorSimple Virtual matriisin](storsimple-ova-web-ui-admin.md).

## <a name="appendix-a-get-the-iqn-of-a-windows-server-host"></a>A: lisäys Hanki Windows Server host IQN

Seuraavien toimien saat iSCSI koko nimi (IQN) Windows-isännän, jossa on käytössä Windows Server 2012.

#### <a name="to-get-the-iqn-of-a-windows-host"></a>Windows-isäntä IQN hankkiminen

1. Käynnistä Microsoft iSCSI-käynnistäjä Windows-isännän.

2. **ISCSI käynnistäjä ominaisuudet** -ikkunassa **määritys** -välilehti Valitse ja kopioi merkkijonon **Käynnistäjä nimi** -kentästä.

    ![iSCSI käynnistäjä ominaisuudet](./media/storsimple-ova-deploy3-iscsi-setup/image34.png)

2. Tallenna merkkijono.

<!--Reference link-->
[1]: https://technet.microsoft.com/library/ee338480(WS.10).aspx



