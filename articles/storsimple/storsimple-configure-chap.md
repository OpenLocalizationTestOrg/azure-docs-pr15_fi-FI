<properties 
   pageTitle="Määritä CHAP StorSimple laitteeseesi | Microsoft Azure"
   description="Tässä artikkelissa käsitellään määrittäminen todennus Handshake Authentication Protocol (CHAP) StorSimple laitteessa."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="TBD"
   ms.date="08/17/2016"
   ms.author="alkohli" />

# <a name="configure-chap-for-your-storsimple-device"></a>Määritä CHAP StorSimple laitteeseesi

Tässä opetusohjelmassa kerrotaan, miten voit määrittää CHAP StorSimple laitteeseesi. Tämän artikkelin tarkat ohjeet koskevat StorSimple 8000 sarjan sekä StorSimple 1200 laitteet.

CHAP lyhenne todennus Handshake Authentication Protocol. On käyttämä palvelinten vahvistamaan remote asiakkaiden todennusmalli. Jaettu salasanan tai salaisuus perustuvan vahvistus. CHAP voi olla yksisuuntainen (yksisuuntainen) tai keskinäiset (kaksisuuntainen). Yksisuuntainen CHAP on, kun kohde todentaa käynnistäjä. Keskinäistä tai käänteisen CHAP edellyttää toisaalta, kohde todennetaan lähettäjä ja käynnistäjä todentaa kohde. Käynnistäjä todennus voidaan toteuttaa ilman kohde todennusta. Kuitenkin kohde todennus voidaan toteuttaa vain, jos käynnistäjä todennus käyttöön myös. 

Paras käytäntö on suositeltavaa, että käytät CHAP iSCSI tietoturvan.

>[AZURE.NOTE] Ota huomioon, että IP ei tällä hetkellä tueta StorSimple laitteet.

StorSimple laitteeseen CHAP-asetuksia voi määrittää seuraavilla tavoilla:

- Yksisuuntainen tai yksisuuntainen todennus

- Kaksisuuntaisen tai keskinäistä tai käänteisen todentaminen

Kaikissa tapauksissa portal-laite ja iSCSI-käynnistäjä Palvelinohjelmisto varten on määritettävä. Tässä määrityksessä yksityiskohtaiset ohjeet on kuvattu seuraavassa opetusohjelman.

## <a name="unidirectional-or-one-way-authentication"></a>Yksisuuntainen tai yksisuuntainen todennus

Yksisuuntainen todennus kohde todentaa lähettäjä. Todennus edellyttää, että CHAP käynnistäjä-asetusten määrittäminen StorSimple laitteen ja iSCSI käynnistäjä ohjelmiston isännän. StorSimple laite-ja Windows host yksityiskohtaiset ohjeet on kuvattu Seuraava.

#### <a name="to-configure-your-device-for-one-way-authentication"></a>Voit määrittää laitteesi yksisuuntainen todennusta varten

1. Valitse Azure perinteinen portal **laitteet** -sivulla **Määritä** -välilehti.

    ![CHAP käynnistäjä](./media/storsimple-configure-chap/IC740943.png)

2. Vieritä alaspäin tällä sivulla ja valitse **CHAP käynnistäjä** -kohdassa:
                                                    
    1. Anna CHAP käynnistäjä käyttäjänimi.

    2. Anna CHAP käynnistäjä salasanan.

         > [AZURE.IMPORTANT] CHAP-käyttäjänimen on oltava alle 233 merkkiä. CHAP Salasanassa on oltava 12 – 16 merkkiä. Windows-isännän käyttöoikeuksien tarkistusvirhe johtaa pidentää käyttäjänimi tai salasana.
    
    3. Vahvista salasana.

4. Valitse **Tallenna**. Vahvistussanoman näkyvät. Valitse **OK** , Tallenna muutokset.

#### <a name="to-configure-one-way-authentication-on-the-windows-host-server"></a>Windows host server yksisuuntainen käyttöoikeuksien määrittämistä varten

1. Käynnistä Windows Host (isäntä)-palvelimen iSCSI-käynnistäjä.

2. **ISCSI käynnistäjä ominaisuudet** -ikkunassa toimi seuraavasti:
                                                    
    1. Valitse **Etsintä** -välilehti.

        ![iSCSI käynnistäjä ominaisuudet](./media/storsimple-configure-chap/IC740944.png)

    2. Valitse, **Tutustu Portal**.

3. **Tutustu kohde Portal** -valintaikkunassa:
                                                    
    1. Määritä laitteen IP-osoite.

    3. Valitse **Lisäasetukset**.

        ![Tutustu kohde-portaalissa](./media/storsimple-configure-chap/IC740945.png)

4. **Lisäasetukset** -valintaikkunassa:
                                                    
    1. Valitse **Ota käyttöön CHAP Kirjaudu sisään** -valintaruutu.

    2. Anna käyttäjänimi, joka on valittu CHAP käynnistäjä perinteinen portaalissa **nimi** -kenttään.

    3. **Kohteen salaisuus** -kenttään annettava salasana, joka on valittu CHAP käynnistäjä perinteinen-portaalissa.

    4. Valitse **OK**.

        ![Yleiset asetukset](./media/storsimple-configure-chap/IC740946.png)

5. Laitteen tila pitäisi näkyä **yhdistetty**muodossa **iSCSI käynnistäjä ominaisuudet** -ikkunassa **kohteet** -välilehdessä. Jos käytössäsi on StorSimple 1200 laitetta, valitse jokaisen aseman otetaan käyttöön kuin iSCSI-kohteen alla kuvatulla tavalla. Näin ollen vaiheet 3 – 4 on toistettava jokaisen aseman.

    ![Asemat, jotka on otettu käyttöön erillinen kohteeksi](./media/storsimple-configure-chap/chap4.png)

    > [AZURE.IMPORTANT] Jos muutat iSCSI nimen, uusi iSCSI-istunnot käytetään uusi nimi. Uudet asetukset ei käytetä nykyiset istunnot, ennen kuin kirjaudut ulos ja kirjaudu uudelleen.

Lisätietoja CHAP määrittäminen Windows Host (isäntä)-palvelimeen Siirry [muita huomioon otettavia seikkoja](#additional-considerations).


## <a name="bidirectional-or-mutual-authentication"></a>Kaksisuuntaisen tai keskinäiset todennus

Kaksisuuntainen todennuksessa kohde todentaa lähettäjä ja sitten käynnistäjä todentaa kohde. Tämä edellyttää, että käyttäjä laitteen ja iSCSI käynnistäjä ohjelmiston isännän CHAP käynnistäjä asetuksia sekä käänteisen CHAP asetusten määrittämistä varten. Seuraavassa kuvataan vaiheet laitteessa ja Windows-isännän kaksisuuntainen käyttöoikeuksien määrittämiseen.

#### <a name="to-configure-your-device-for-mutual-authentication"></a>Kaksisuuntainen käyttöoikeuksien laitteen määrittäminen

1. Valitse Azure perinteinen portal **laitteet** -sivulla **Määritä** -välilehti.

    ![CHAP kohde](./media/storsimple-configure-chap/IC740948.png)

2. Vieritä alaspäin tällä sivulla ja valitse **CHAP kohde** -kohdassa:
                                                    
    1. Anna **Käänteinen CHAP käyttäjänimi** laitteeseesi.

    2. Määritä **Käänteinen CHAP salasanan** laitteeseesi.

    3. Vahvista salasana.

3. **CHAP käynnistäjä** -osassa:
                                                
    1. Anna **käyttäjänimi** laitteeseesi.

    1. Anna **salasana** laitteeseesi.

    3. Vahvista salasana.

4. Valitse **Tallenna**. Vahvistussanoman näkyvät. Valitse **OK** , Tallenna muutokset.

#### <a name="to-configure-bidirectional-authentication-on-the-windows-host-server"></a>Kaksisuuntainen käyttöoikeuksien määrittämiseksi Windows server Host (isäntä)

1. Käynnistä Windows Host (isäntä)-palvelimen iSCSI-käynnistäjä.

2. Valitse **iSCSI käynnistäjä ominaisuudet** -ikkunassa **määritys** -välilehti.

3. Valitse **CHAP**.

4. **ISCSI käynnistäjä keskinäistä CHAP salaisuus** -valintaikkunassa:
                                                    
    1. Kirjoita **Käänteinen CHAP salasana** , jonka määritit Azure perinteinen-portaalissa.

    2. Valitse **OK**.

        ![iSCSI käynnistäjä keskinäistä CHAP-salaisuus](./media/storsimple-configure-chap/IC740949.png)

5. Valitse **kohteet** -välilehti.

6. Napsauta **Muodosta** -painiketta. 

7. Valitse **Yhdistä kohde** -valintaikkunassa **Lisäasetukset**.

8. **Lisäominaisuudet** -valintaikkunassa:
                                                    
    1. Valitse **Ota käyttöön CHAP Kirjaudu sisään** -valintaruutu.

    2. Anna käyttäjänimi, joka on valittu CHAP käynnistäjä perinteinen portaalissa **nimi** -kenttään.

    3. **Kohteen salaisuus** -kenttään annettava salasana, joka on valittu CHAP käynnistäjä perinteinen-portaalissa.

    4. Valitse **Suorita kaksisuuntainen käyttöoikeuksien** -valintaruutu.

        ![Kaksisuuntainen käyttöoikeuksien Lisäasetukset](./media/storsimple-configure-chap/IC740950.png)

    5. Valitse **OK** suorittamaan CHAP-määritys
     
Lisätietoja CHAP määrittäminen Windows Host (isäntä)-palvelimeen Siirry [muita huomioon otettavia seikkoja](#additional-considerations).

## <a name="additional-considerations"></a>Muita huomioon otettavia seikkoja

**Nopea yhdistäminen** -toiminto ei tue yhteydet, jotka on otettu käyttöön CHAP. Kun CHAP on otettu käyttöön, varmista, että käytät **Muodosta** -painiketta, joka on käytettävissä muodostaa kohde **kohteet** -välilehdessä.

![Yhteyden muodostaminen kohde](./media/storsimple-configure-chap/IC740947.png)

Valitse, jotka on jaettu **Yhdistä kohteeseen** -valintaikkunassa **Lisää tuttuja kohteiden luettelo tässä yhteydessä** -valintaruutu. Näin varmistat, että aina, kun tietokoneen uudelleen on yritettiin yhteyden palauttaminen iSCSI tuttuja kohteet.

## <a name="errors-during-configuration"></a>Virheet määritysten aikana

Jos CHAP määritys on virheellinen, et todennäköisesti **tunnistaminen epäonnistui** -virhesanoman.

## <a name="verification-of-chap-configuration"></a>CHAP määritysten tarkistaminen

Voit varmistaa, että CHAP käytetään noudata seuraavia ohjeita.

#### <a name="to-verify-your-chap-configuration"></a>Vahvista CHAP-määritys

1. Valitse **haluamasi kohteet**.

2. Valitse kohde, jonka otit todennusta.

3. Valitse **tiedot**.

    ![iSCSI käynnistäjä ominaisuudet tuttuja kohteet](./media/storsimple-configure-chap/IC740951.png)

4. Huomaa **Tuttuja kohteen tiedot** -valintaikkunassa tapahtuma **tarkistus** -kenttään. Jos määritykset onnistui, pitäisi näkyä versionumeron **CHAP**.

    ![Suosikkikansioiden kohteen tiedot](./media/storsimple-configure-chap/IC740952.png)

## <a name="next-steps"></a>Seuraavat vaiheet

- Lisätietoja [StorSimple suojaus](storsimple-security.md).

- Lisätietoja [ammattimainen StorSimple laitteen StorSimple hallinta-palvelun avulla](storsimple-manager-service-administration.md).
