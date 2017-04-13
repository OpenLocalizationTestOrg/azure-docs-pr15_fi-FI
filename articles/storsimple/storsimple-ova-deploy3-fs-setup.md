<properties
   pageTitle="Ottaa käyttöön StorSimple Virtual taulukko 3 - tiedostopalvelimeen virtual laitteen määrittäminen"
   description="StorSimple Virtual matriisin käyttöönoton kolmas opetusohjelma kehottaa määrittää virtual laitteen tiedoston palvelimeksi."
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
   ms.date="05/26/2016"
   ms.author="alkohli"/>

# <a name="deploy-storsimple-virtual-array---set-up-as-file-server"></a>Ottaa käyttöön StorSimple Virtual matriisi - määrittäminen määrittäminen tiedoston palvelimeksi

![](./media/storsimple-ova-deploy3-fs-setup/fileserver4.png)

## <a name="introduction"></a>Johdanto 

Tämä artikkeli koskee Microsoft Azure StorSimple Virtual matriisi (tunnetaan myös nimellä StorSimple paikallisen virtual laitteen tai StorSimple virtual laite) käynnissä maaliskuussa 2016 yleiseen käyttöön (GA)-versioon. Tässä artikkelissa käsitellään suorittaa ensimmäisen määrityskerran, rekisteröidä StorSimple tiedostopalvelimeen, laite-asennuksen ja luoda ja muodostaa yhteyden SMB osakkeet. Tämä on sarjan viimeinen käsittelevään käyttöönoton opetusohjelmat pakollinen ottamaan kokonaan virtual matriisi kuin tiedostopalvelimeen tai iSCSI palvelimelle.

Asennus ja määritys-prosessi voi kestää noin 10 minuutin suorittamiseen.


## <a name="setup-prerequisites"></a>Asetukset

Ennen kuin määrittäminen ja StorSimple virtual laitteen määrittäminen, varmista, että:

-   Sinulla on valmisteltu virtual laitteen ja yhdistetty [säännöstä Hyper - v StorSimple Virtual matriisi](storsimple-ova-deploy2-provision-hyperv.md) tai [säännöstä StorSimple Virtual matriisi-VMware](storsimple-ova-deploy2-provision-vmware.md)esitetyllä tavalla.

-   Sinun on StorSimple hallinta-palvelusta, jonka loit StorSimple virtual laitteiden hallinta palvelun rekisteröinti-näppäintä. Lisätietoja on artikkelissa [Vaihe 2: hankkia palvelun rekisteröinti avain](storsimple-ova-deploy1-portal-prep.md#step-2-get-the-service-registration-key) StorSimple Virtual matriisin.

-   Jos kyseessä on toinen tai myöhempi virtual laitteella, joka on rekisteröiminen StorSimple hallinta-palvelussa, taulukossa pitäisi olla palvelun tiedot salausavaimen. Tämä avain on luotu, kun ensimmäinen laite on rekisteröitynyt tämän palvelun avulla. Jos tämä avain on katkennut, katso StorSimple Virtual matriisin [Hae palvelun tiedot salausavaimen](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key) .

## <a name="step-by-step-setup"></a>Vaiheittaisesta määrittämisestä

Seuraavien vaiheittaisten ohjeiden avulla voit StorSimple virtual laitteen määrittäminen.

## <a name="step-1-complete-the-local-web-ui-setup-and-register-your-device"></a>Vaihe 1: Asennuksen paikallisen sivuston Käyttöliittymä ja rekisteröi laite 


#### <a name="to-complete-the-setup-and-register-the-device"></a>Rekisteröi laite ja Viimeistele määritykset

1.  Avaa selain ja muodosta yhteys paikalliseen web-Käyttöliittymä. Tyyppi: 

    `https://<ip-address of network interface>`

    Käytä edellisessä vaiheessa yhteyden URL-Osoitetta. Näyttöön tulee virhesanoma, joka ilmoittaa, että on sivuston suojausvarmenteessa ongelma. Valitse **Jatka tähän sivustoon**.

    ![](./media/storsimple-ova-deploy3-fs-setup/image2.png)

1.  Kirjaudu web-Käyttöliittymää virtual laitteesi **StorSimpleAdmin**. Kirjoita laitteen järjestelmänvalvojan salasana, jotka olet muuttanut vaihe 3: Käynnistä virtual laite [säännöstä Hyper - v StorSimple Virtual matriisi](storsimple-ova-deploy2-provision-hyperv.md) tai [säännöstä StorSimple Virtual matriisi-VMware](storsimple-ova-deploy2-provision-vmware.md).

    ![](./media/storsimple-ova-deploy3-fs-setup/image3.png)

1.  Siirryt **Aloitussivu** . Tällä sivulla kerrotaan eri asetukset, joita tarvitaan ja rekisteröidä virtual laitteen StorSimple hallinta-palveluun. Huomaa, että **verkkoasetukset**, **Web-välityspalvelimen**ja **aika-asetukset** ovat valinnaisia. **Laiteasetusten** ja **Cloud asetukset**ovat vain tarvittavat asetukset.

    ![](./media/storsimple-ova-deploy3-fs-setup/image4.png)

1.  Valitse **verkkoliittymät** **verkkoasetukset** -sivulla tiedot 0 määritetään automaattisesti puolestasi. Kunkin verkkoliittymän on määritetty oletusarvoisesti hakea IP-osoitteen automaattisesti (DHCP). Näin ollen IP-osoite, aliverkon ja yhdyskäytävän automaattisesti määritetään (varten sekä IPv4- että IPv6).

    ![](./media/storsimple-ova-deploy3-fs-setup/image5.png)

    Jos olet lisännyt useita verkkoliittymän laitteen valmistelun aikana, voit määrittää ne tähän. Huomautus Voit määrittää verkkoliittymän IPv4 kuin vain tai IPv4- ja IPv6 nimellä. Vain IPv6-määrityksiä ei tueta.

1.  DNS-palvelimet tarvitaan, koska niitä käytetään, kun tiedostopalvelimeen määritetty nimi laitteen yrittää pitää yhteyttä cloud-tallennustilan palveluntarjoajien tai laitteen ratkaisemiseksi. **Verkko-asetukset** -sivun **DNS-palvelimet**-kohdassa:

    1.  Ensisijaisen ja toissijaisen DNS-palvelin määritetään automaattisesti. Jos haluat määrittää staattinen IP-osoitteet, voit määrittää DNS-palvelimet. Suuren käytettävyyden Suosittelemme, että määrität ensisijainen ja Toissijainen DNS-palvelin.

    2.  Valitse **Käytä**. Tämä Käytä ja Tarkista verkkoasetukset.

2.  **Laitteen asetukset** -sivulla:

    1.  Määritä laitteen yksilöivä **nimi** . Tämä nimi voi olla 1-15 merkkiä, ja ne voivat sisältää kirjain, numeroita ja väliviivoja.

    2.  Valitse **tiedoston** kuvakkeen ![](./media/storsimple-ova-deploy3-fs-setup/image6.png) laite, jota olet luomassa **tyyppi** . Tiedostopalvelimeen avulla voit luoda jaettuja kansioita.

    3.  Kun laite on tiedostopalvelimessa, sinun on liittää laite toimialueelle. Kirjoita **toimialuenimi**.

    1.  Valitse **Käytä**.

2.  Valintaikkuna tulee näkyviin. Kirjoita toimialueen tunnistetiedot määritetyssä muodossa. Valitse Tarkista-kuvake. Toimialueen tunnistetietoja tarkistetaan. Näyttöön tulee virhesanoma, jos tunnistetiedot ovat virheellisiä.

    ![](./media/storsimple-ova-deploy3-fs-setup/image7.png)

1.  Valitse **Käytä**. Tämä Käytä ja tarkista Laiteasetukset.

    ![](./media/storsimple-ova-deploy3-fs-setup/image8.png)

    > [AZURE.NOTE]
    > 
    > Varmista, että virtual matriisi on oma organisaatioyksikkö (OU) Active Directoryn eivätkä ei ryhmäkäytännön objektit (Ryhmäkäytäntöobjekti) on käytetty tai peritään. Ryhmäkäytäntö voi asentaa sovellukset, kuten virustorjuntaohjelmaa StorSimple Virtual matriisi. Lisää ohjelmistojen ei tue ja saattaa johtaa vahingoittumismahdollisuus. 

1.  Web-välityspalvelimen määrittäminen (valinnainen). Web-välityspalvelimen määritys on valinnainen, mutta huomioon, että jos käytät web-välityspalvelimen, voit vain määrittää sen tähän.

    ![](./media/storsimple-ova-deploy3-fs-setup/image9.png)

    **Web-välityspalvelimen** -sivulla:

    1.  Anna **välityspalvelimen WWW-URL-osoite** tässä muodossa: *http://&lt;isännän IP-osoite tai FDQN&gt;: porttinumero*. Huomaa, että HTTPS URL-osoitteita ei tueta.

    2.  Määritä **todennus** **Basic** tai **ei mitään**.

    3.  Jos todennusta, sinun on myös **käyttäjänimeä** ja **salasanaa**.

    4.  Valitse **Käytä**. Tämä Vahvista ja käyttää määritetyn web välityspalvelimen asetuksia.

1.  Laitteen, kuten aikavyöhykkeen ja ensisijaisen ja toissijaisen NTP-palvelimien aika-asetusten määrittäminen (valinnainen). NTP-palvelimien tarvitaan, koska laite on aikaa, niin, että se todentamismenetelmä cloud palveluntarjoajia.

    ![](./media/storsimple-ova-deploy3-fs-setup/image10.png)

    **Aika-asetukset** -sivulla:

    1.  Valitse avattavasta luettelosta **aikavyöhyke** , jossa laitteen otetaan käyttöön maantieteellisen sijainnin perusteella. Laitteen Oletusaikavyöhyke on pst-tiedoston. Laitteen käyttää tätä aikavyöhykkeen ajoitetun kaikki toiminnot.

    2.  **Ensisijainen NTP server** laitteeseesi tai hyväksy time.windows.com oletusarvon. Varmista verkon sallii NTP-liikenne paikalliseen välittää Internet-yhteyttä palvelinkeskukseen.

    3.  Voit myös määrittää **toissijaisen NTP server** laitteeseesi.

    4.  Valitse **Käytä**. Tämä Vahvista ja käyttää määritetyt aika-asetuksia.

1.  Laitteen cloud-asetusten määrittäminen. Tässä vaiheessa Viimeistele määritys paikallisen laitteen ja rekisteröi sitten laitteen StorSimple hallinta-palvelun kanssa.

    1.  Kirjoita **palvelun rekisteröinti avainta** , jotka olet saanut- [Vaihe 2: hankkia palvelun rekisteröinti avain](storsimple-ova-deploy1-portal-prep.md#step-2-get-the-service-registration-key) StorSimple Virtual matriisin.

    2.  Ohita tämä vaihe, jos tämä on ensimmäinen laitteen rekisteröinti tämän palvelun avulla ja siirry seuraavaan vaiheeseen. Jos tämä ei ole ensimmäisen laitteella, joka on rekisteröimistä tämä palvelu, sinun on **palvelun tiedot salausavaimen**. Tämä avain tarvitaan muita laitteita rekisteröityä StorSimple hallintapalvelu palvelun rekisteröinti-näppäimen kanssa. Saat lisätietoja viitata hankkia [palvelun tiedot salausavaimen](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key) paikallisen sivuston Käyttöliittymä.

    3.  Valitse **Rekisteröi**. Tämä käynnistää laite. Voit joutua odottamaan 2 – 3 minuuttia, ennen kuin laitteen rekisteröinti onnistuu. Laite on käynnistettävä uudelleen, kun siirryt Kirjaudu sisään-sivulla.

        ![](./media/storsimple-ova-deploy3-fs-setup/image13.png)
    

1.  Palaa perinteiseen Azure-portaaliin. Varmista **laitteet** -sivulla, että laite on muodostanut yhteyden palvelun etsimällä tila. Laitteen tila on oltava **aktiivinen**.

![](./media/storsimple-ova-deploy3-fs-setup/image12.png)

## <a name="step-2-complete-the-required-device-setup"></a>Vaihe 2: Viimeistele tarvittavat laitteen määrittäminen

Suorittamaan StorSimple laitteen laite-määritys on:

-   Valitse liitettävä laitteen tallennustila-tili.

-   Valitse tiedot, jotka on lähetetty cloud salausasetukset.

Seuraavien toimien [Azure perinteinen portal](https://manage.windowsazure.com/) tarvittava laite-asennuksen viimeistelemiseen.

#### <a name="to-complete-the-minimum-device-setup"></a>Pienin laite-asennuksen viimeistelemiseen

1.  **Laitteet** -sivulla Valitse juuri luomasi laite. Tämä laite näkyvät **aktiiviseksi**. Laitteen nimen vastaan olevaa nuolta ja valitse sitten **Pika-aloitus**.

2.  Valitse Käynnistä ohjattu laitteen määrittäminen **Valmis laitteen asennus** .

3.  Valitse Määritä laitteen ohjatussa **Perusasetukset** -sivulla seuraavasti:

    1.  Määritä tallennustilan-tili, jota käytetään laitteen kanssa. Voit valita olemassa olevan tallennustilan tilin tähän tilaukseen avattavasta luettelosta tai määrittää **Lisää** tili valittavana eri tilauksen.

    2.  Määritä salausasetukset kaikki tiedot-palvelussa-muiden (AES-salausta), joka lähetetään pilveen. Salaa tiedot, tarkista **käyttöön cloud tallennustilan salausavaimen**yhdistelmäruutuun. Kirjoita cloud tallennustilan salausta, joka sisältää 32 merkkiä. Vahvista kirjoittamalla se sitten-näppäintä. 256-bittistä AES-avainta käytetään salausta käyttäjän määrittämä-näppäimen kanssa.

    3.  Valitse tarkistuksen kuvake ![](./media/storsimple-ova-deploy3-fs-setup/image15.png).

        ![](./media/storsimple-ova-deploy3-fs-setup/image16.png)

Asetukset päivitetään. Kun asetukset on päivitetty, täydellinen laitteen asetukset-painike näkyy harmaana. Voit palauttaa laitteen **Pika-aloitus** -sivulle.

 ![](./media/storsimple-ova-deploy3-fs-setup/image17.png)


> [AZURE.NOTE]                                                              
>
> Voit muokata muita laiteasetusten milloin tahansa muodostamalla yhteyden **määrittäminen** -sivulla.

## <a name="step-3-add-a-share"></a>Vaihe 3: Lisää jaettuun

Seuraavien toimien [Azure perinteinen portal](https://manage.windowsazure.com/) Luo jaettu kansio.

#### <a name="to-create-a-share"></a>Voit luoda jaetun

1.  Valitse laitteen **Pika-aloitus** -sivulla **Lisää jaettuun**. Jaa Ohjattu lisääminen käynnistyy.

    ![](./media/storsimple-ova-deploy3-fs-setup/image17.png)

1.  Valitse **Perusasetukset** -sivulla seuraavasti:

    1.  Määrittää, että Jaa yksilöllinen nimi. Nimen on oltava merkkijono, joka sisältää 3-127 merkkiä.

    2.  (Valinnainen) Ongelman kuvaus jaetun. Kuvaus tunnistaa Jaa omistajat.

    3.  Valitse Jaa käyttötyyppi. Käyttö-tyypin voi olla **Tiered** tai **paikallisesti kiinnitetty**, kanssa Porrastettu on oletusarvo. Valitse toiminnoista, jotka edellyttävät paikallisen oikeudet, pieni viiveet suurempia ja paremman suorituskyvyn, **paikallisesti kiinnitetyt** Jaa. Jos kaikki muut tiedot, valitse **Tiered** Jaa.

    Paikallisesti kiinnitetty Jaa thickly valmisteltu ja varmistaa, että Jaa ensisijainen tiedot ovat varmasti paikallisessa laitteelle ja ei spill pilveen. Porrastettu Jaa toisaalta on levinneet valmistelun yhteydessä. Kun luot Porrastettu Jaa, 10 prosenttia tila on valmisteltu paikallinen ylätasolla ja 90 prosenttia tila on valmisteltu pilveen. Esimerkiksi jos 1 TT aseman valmistelun yhteydessä, 100 Gigatavua sijaitsi paikallisen tilaa ja 900 gt voi käyttää pilveen kun tietojen tasoa. Tämä puolestaan tarkoittaa, että jos suoritat kaikki paikallisen loppumassa laitteessa, sinun ei voi valmistella Porrastettu Jaa.

1.  Määrittää, että Jaa valmistellun kapasiteetti. Huomaa, että määritetyn kapasiteetin on oltava pienempi kuin käytettävissä oleva kapasiteetti. Jos Porrastettu Jaa, Jaa kokoa on oltava välillä 500 Gigatavua ja 20 Teratavua. Määritä paikallisesti kiinnitetty Jaa jaa koko välillä 50 Gigatavun ja 2 Teratavua. Valmistele jaettuun ohjeena käytettävissä olevan kapasiteetin avulla. Jos käytettävissä olevan paikallisen kapasiteetin on 0 Gigatavua, valitse voit ei saavat valmistelu paikallisen tai Porrastettu.

    ![](./media/storsimple-ova-deploy3-fs-setup/image18.png)

1.  Napsauttamalla nuolta ![](./media/storsimple-ova-deploy3-fs-setup/image19.png) Siirry seuraavalle sivulle.

1.  Valitse **Lisäasetukset** -sivulla käyttöoikeuksien käyttäjä tai ryhmä, johon pääsevät tämän Jaa. Määrittää käyttäjän tai käyttäjäryhmän nimen *<john@contoso.com>* muodossa. On suositeltavaa, että Salli järjestelmänvalvojan oikeudet, voi käyttää näitä osakkeet käyttäjäryhmä (sijaan yksittäisen käyttäjän) avulla. Kun olet määrittänyt tähän käyttöoikeudet, voit sitten näiden käyttöoikeuksien muokkaaminen Resurssienhallinnan avulla.

    ![](./media/storsimple-ova-deploy3-fs-setup/image20.png)

1.  Valitse tarkistuksen kuvake ![](./media/storsimple-ova-deploy3-fs-setup/image21.png). Jaettuun luodaan määritetyn asetusten kanssa. Oletusarvon mukaan seuranta- ja varmuuskopiointi on käytettävissään Jaa.

## <a name="step-4-connect-to-the-share"></a>Vaihe 4: Yhdistäminen Jaa

Sinun on nyt muodostaa share(s), jonka loit edellisessä vaiheessa. Näiden vaiheiden suorittamista Windows Server-isännän.

#### <a name="to-connect-to-the-share"></a>Muodosta yhteys Jaa

1.  Paina ![](./media/storsimple-ova-deploy3-fs-setup/image22.png) + R. Kirjoita Suorita-ikkunassa * \\ * kuin polun korvaamalla *palvelimen nimi* laitteen nimi, jonka määritit tiedosto palvelimeen. Valitse **OK**.

    ![](./media/storsimple-ova-deploy3-fs-setup/image23.png)

2.  Tämä Avaa Resurssienhallinnassa ylöspäin. Pitäisi nyt olla näe jaettuja resursseja, jotka olet luonut kansioita. Valitse ja kaksoisnapsauta sisältöä Jaa (kansio).

    ![](./media/storsimple-ova-deploy3-fs-setup/image24.png)

3.  Voit lisätä tiedostoja nämä osakkeet ja varmuuskopioi.

![videokuvake](./media/storsimple-ova-deploy3-fs-setup/video_icon.png) **Video käytettävissä**

Katso video ja katso, miten voit määrittää ja rekisteröi StorSimple Virtual matriisin tiedostopalvelimeen.

> [AZURE.VIDEO configure-a-storsimple-virtual-array]

## <a name="next-steps"></a>Seuraavat vaiheet

Opettele käyttämään paikallisen sivuston Käyttöliittymä ammattimainen [StorSimple Virtual matriisin](storsimple-ova-web-ui-admin.md).
