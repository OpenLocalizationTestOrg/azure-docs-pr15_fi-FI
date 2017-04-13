<properties
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi piirtoalustan LMS | Microsoft Azure" 
    description="Opettele käyttämään piirtoalustan LMS Azure Active Directory-hakemistosta käyttöön kertakirjautumisen, automaattinen valmistelu ja lisää!" 
    services="active-directory" 
    authors="jeevansd"  
    documentationCenter="na" 
    manager="femila"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="09/29/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-canvas-lms"></a>Opetusohjelma: Piirtoalustan LMS Azure Active Directory-integrointi

Tässä opetusohjelmassa tavoitteena on näyttämään Azure ja piirtoalustan integrointi.  
Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   Piirtoalustan palvelutili

Kun olet suorittanut Tässä opetusohjelmassa, olet määrittänyt piirtoalustaan Azure AD-käyttäjien saa oikeuden kertakirjauksen piirtoalustan yrityksesi sivuston (palveluntarjoajan aloittanut allekirjoitus-) tai käyttämällä [Access-paneelin esittely](active-directory-saas-access-panel-introduction.md)-sovellukseen.

Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1.  Piirtoalustan sovelluksen integrointi ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Määrittäminen käyttäjän valmistelu
4.  Käyttäjien määrittäminen

![Skenaario] (./media/active-directory-saas-canvas-lms-tutorial/IC775984.png "Skenaario")
##<a name="enabling-the-application-integration-for-canvas"></a>Piirtoalustan sovelluksen integrointi ottaminen käyttöön

Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön piirtoalustan sovelluksen integrointi.

###<a name="to-enable-the-application-integration-for-canvas-perform-the-following-steps"></a>Piirtoalustan sovelluksen integrointi käyttöön toimimalla seuraavasti:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-canvas-lms-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-canvas-lms-tutorial/IC700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-canvas-lms-tutorial/IC749321.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-canvas-lms-tutorial/IC749322.png "Lisää sovellus-gallerry")

6.  Kirjoita **hakuruutuun** **alusta**.

    ![Sovelluksen valikoima] (./media/active-directory-saas-canvas-lms-tutorial/IC775985.png "Sovelluksen valikoima")

7.  Valitse tulokset-ruudun Valitse **alusta**ja valitse sitten **Valmis** , voit lisätä sovelluksen.

    ![Piirtoalusta] (./media/active-directory-saas-canvas-lms-tutorial/IC775986.png "Piirtoalusta")
##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen

Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjien todentamiseen piirtoalustaan niiden tilillä, käyttämällä SAML-protokollan perusteella federation Azure AD.  
Kertakirjautumisen varten piirtoalustan määrittäminen edellyttää, että allekirjoitus arvon noutaa varmenne.  
Jos et ole tottunut näin, katso, [miten voit hakea varmenteen allekirjoitus arvon](http://youtu.be/YKQF266SAxI)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Valitse Azure perinteinen portaalissa integrointi **piirtoalustan** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-canvas-lms-tutorial/IC771709.png "Määritä kertakirjautuminen")

2.  **Miten haluat kirjautua piirtoalustaan käyttäjät** -sivulla **Microsoft Azure AD kertakirjautumisen**Valitse ja valitse sitten **Seuraava**.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-canvas-lms-tutorial/IC775987.png "Kertakirjautumisen määrittäminen")

3.  Kirjoita **Määrittäminen sovelluksen URL-osoite** -sivulla **Kirjaudu piirtoalustan URL-** -tekstiruutuun URL-osoite käyttämällä seuraavaa kaavaa `https://<tenant-name>.instructure.com`, ja valitse sitten **Seuraava**.

    ![Sovelluksen URL-Osoitteen määrittäminen] (./media/active-directory-saas-canvas-lms-tutorial/IC775988.png "Sovelluksen URL-Osoitteen määrittäminen")

4.  **Määritä kertakirjautumisen osoitteessa alusta** -sivulla Lataa sertifikaatti, valitse **Lataa**, ja Tallenna sertifikaattitiedosto paikallisesti tietokoneeseen.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-canvas-lms-tutorial/IC775989.png "Kertakirjautumisen määrittäminen")

5.  Eri selainikkunassa Kirjaudu piirtoalustan yrityksesi sivuston järjestelmänvalvojan oikeuksilla.

6.  Siirry **kursseja \> hallitut tilit \> Microsoft**.

    ![Piirtoalusta] (./media/active-directory-saas-canvas-lms-tutorial/IC775990.png "Piirtoalusta")

7.  Vasemman reunan siirtymisruudussa Valitse **todennus**ja valitse sitten **Lisää uusi SAML Config**.

    ![Todennus] (./media/active-directory-saas-canvas-lms-tutorial/IC775991.png "Todennus")

8.  Tee nykyinen integrointi-sivulla seuraavat toimet:

    ![Nykyinen integrointi] (./media/active-directory-saas-canvas-lms-tutorial/IC775992.png "Nykyinen integrointi")

    1.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa alusta** -valintasivun **Kohteen tunnus** -arvon kopioi ja liitä se sitten **IdP kohteen tunnus** -tekstiruutuun.
    2.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa alusta** -valintasivun **Remote sisäänkirjautumisen URL-osoite** -arvo kopioi ja liitä se sitten **Log-URL** -tekstiruutuun.
    3.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa alusta** -valintasivun **Remote sisäänkirjautumisen URL-osoite** -arvo kopioi ja liitä se sitten **Kirjaudu ulos URL** -tekstiruutuun.
    4.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa alusta** -valintasivun **Muuta salasana URL-osoite** -arvo kopioi ja liitä se sitten **Muuta salasana linkki** -tekstiruutuun.
    5.  Viedyn varmenteen **allekirjoitusta** kopioi ja liitä se **Varmenteen tunnisteen** tekstiruutu.  

        >[AZURE.TIP] Lisätietoja on artikkelissa [noutamisesta varmenteen allekirjoitusarvo](http://youtu.be/YKQF266SAxI)

    6.  **Kirjaudu sisään-määrite** -luettelosta **NameID**.
    7.  **Tunniste-muoto** -luettelosta **emailAddress**.
    8.  Valitse **Tallenna todennusasetukset**.

9.  Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Valmis** **Määrittäminen kertakirjautuminen** -valintaikkunassa Sulje.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-canvas-lms-tutorial/IC775993.png "Kertakirjautumisen määrittäminen")
##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu

Jotta voit kirjautua piirtoalustan Azure AD-käyttäjien käyttöön, ne on valmisteltava kyselyjä alusta.  
Piirtoalusta valmistelu on manuaalinen tehtävä.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Valmistelu käyttäjätilit, toimi seuraavasti:

1.  Kirjaudu sisään **piirtoalustan** alihallintaan.

2.  Siirry **kursseja \> hallitut tilit \> Microsoft**.

    ![Piirtoalusta] (./media/active-directory-saas-canvas-lms-tutorial/IC775990.png "Piirtoalusta")

3.  Valitse **käyttäjiä**.

    ![Käyttäjät] (./media/active-directory-saas-canvas-lms-tutorial/IC775995.png "Käyttäjät")

4.  Valitse **Lisää uusi käyttäjä**.

    ![Käyttäjät] (./media/active-directory-saas-canvas-lms-tutorial/IC775996.png "Käyttäjät")

5.  Lisää uusi käyttäjä valintasivun toimi seuraavasti:

    ![Käyttäjän lisääminen] (./media/active-directory-saas-canvas-lms-tutorial/IC775997.png "Käyttäjän lisääminen")

    1.  Kirjoita **KokoNimi** -tekstiruutuun käyttäjän nimi.
    2.  Kirjoita **sähköpostiosoite** -tekstiruutuun käyttäjän sähköpostiosoite.
    3.  Kirjoita **Kirjaudu sisään** -tekstiruutuun käyttäjän Azure AD-sähköpostiosoite.
    4.  Valitse **Sähköposti tämän tilin luominen käyttäjää**.
    5.  Valitse **Lisää käyttäjä**.

>[AZURE.NOTE] Voit käyttää mitä tahansa muita piirtoalustan käyttäjän tilin luontityökalut tai ohjelmointirajapinnan myöntämä piirtoalustan säännöstä AAD käyttäjätileille.

##<a name="assigning-users"></a>Käyttäjien määrittäminen

Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-canvas-perform-the-following-steps"></a>Jos haluat määrittää käyttäjiä alusta, toimi seuraavasti:

1.  Testaa tilin luominen Azure perinteinen portaalissa.

2.  Valitse **alusta **sovelluksen integrointi-sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-canvas-lms-tutorial/IC775998.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Kyllä] (./media/active-directory-saas-canvas-lms-tutorial/IC767830.png "Kyllä")

Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli. Katso lisätietoja Access-paneelin [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).
