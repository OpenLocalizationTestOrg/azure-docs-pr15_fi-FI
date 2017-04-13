<properties 
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi Brightspace mukaan Desire2Learn | Microsoft Azure" 
    description="Opettele käyttämään Brightspace mukaan Desire2Learn Azure Active Directory-hakemistosta käyttöön kertakirjautumisen, automaattinen valmistelu ja lisää!" 
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

#<a name="tutorial-azure-active-directory-integration-with-brightspace-by-desire2learn"></a>Opetusohjelma: Brightspace mukaan Desire2Learn Azure Active Directory-integrointi

Tässä opetusohjelmassa tavoitteena on näyttämään Azure ja Brightspace Desire2Learn mukaan.  
Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   Brightspace mukaan Desire2Learn kertakirjautumisen käytössä tilaus

Kun olet suorittanut Tässä opetusohjelmassa, olet liittänyt Brightspace mukaan Desire2Learn Azure AD-käyttäjien saa oikeuden kertakirjauksen sovellukseen, että Brightspace Desire2Learn yrityksen sivuston (palveluntarjoajan aloittanut allekirjoitus-) tai käyttämällä [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).

Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1.  Brightspace mukaan Desire2Learn sovelluksen integrointi ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Määrittäminen käyttäjän valmistelu
4.  Käyttäjien määrittäminen

![Skenaario] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798957.png "Skenaario")
##<a name="enabling-the-application-integration-for-brightspace-by-desire2learn"></a>Brightspace mukaan Desire2Learn sovelluksen integrointi ottaminen käyttöön

Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön Brightspace mukaan Desire2Learn sovelluksen integrointi.

###<a name="to-enable-the-application-integration-for-brightspace-by-desire2learn-perform-the-following-steps"></a>Brightspace mukaan Desire2Learn sovelluksen integrointi käyttöön toimimalla seuraavasti:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC749321.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC749322.png "Lisää sovellus-gallerry")

6.  Kirjoita **hakuruutuun** **Brightspace Desire2Learn mukaan**.

    ![Apllication valikoima] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798958.png "Apllication valikoima")

7.  Valitse tulosruudussa **Brightspace Desire2Learn mukaan**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![Brightspace Desire2Learn mukaan] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC799321.png "Brightspace Desire2Learn mukaan")
##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen

Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjien todentamiseen Brightspace Desire2Learn mukaan käyttämällä SAML-protokollan perusteella federation Azure AD-tilin kanssa.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Valitse Azure perinteinen portaalissa integrointi **Brightspace mukaan Desire2Learn** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798959.png "Kertakirjautumisen määrittäminen")

2.  **Miten käyttäjät voivat kirjautua Brightspace Desire2Learn mukaan haluat käyttää** sivulla Valitse **Microsoft Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798960.png "Kertakirjautumisen määrittäminen")

3.  Tee **Määrittäminen sovelluksen URL-osoite** -sivulla seuraavat toimet:

    ![Sovelluksen URL-Osoitteen määrittäminen] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798961.png "Sovelluksen URL-Osoitteen määrittäminen")

    1.  Kirjoita **Merkki-URL** -tekstiruutuun käyttää käyttäjien oman **Brightspace mukaan Desire2Learn** kirjautumalla URL-osoite (esimerkiksi: *https://partnershowcase.desire2learn.com/Shibboleth.sso/Login?entityID=https://sts.windows-ppe.net/5caf9349-fd93-4a74-b064-0070f65bfb49/&target=https%3A%2F%2Fpartnershowcase.desire2learn.com%2Fd2l%2FshibbolethSSO%2Faspinfo.asp*).
    2.  Valitse **Seuraava**

4.  Valitse **Määritä kertakirjautumisen osoitteessa Brightspace Desire2Learn mukaan** -sivulla lataamaan metatietojen valitsemalla **Lataa metatietojen**, ja Tallenna metatiedot tietokoneeseen.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798962.png "Kertakirjautumisen määrittäminen")

5.  Lähettää ladatut metatieto-tiedosto kohdassa Brightspace Desire2Learn tukihenkilöstön.

    >[AZURE.NOTE] Oman Brightspace mukaan Desire2Learn tukihenkilöstön on suoritettava todellinen SSO-määritys.
Näyttöön tulee ilmoitus, kun SSO on otettu käyttöön tilauksen.

6.  Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Valmis** **Määrittäminen kertakirjautuminen** -valintaikkunassa Sulje.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798963.png "Kertakirjautumisen määrittäminen")
##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu

Jotta voit kirjautua Brightspace mukaan Desire2Learn Azure AD-käyttäjien käyttöön, jos ne valmisteltu yhdeksi Brightspace Desire2Learn mukaan.  
Käyttäjätilit on kyseessä Brightspace Desire2Learn mukaan, voi luoda oman Brightspace Desire2Learn tukihenkilöstön mukaan.

>[AZURE.NOTE] Voit käyttää muita Brightspace Desire2Learn käyttäjän tilin luontityökalut tai ohjelmointirajapinnan myöntämä Brightspace mukaan Desire2Learn valmistelu Azure Active Directory käyttäjätilit.

##<a name="assigning-users"></a>Käyttäjien määrittäminen

Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-brightspace-by-desire2learn-perform-the-following-steps"></a>Jos haluat määrittää käyttäjiä Brightspace Desire2Learn mukaan, toimi seuraavasti:

1.  Testaa tilin luominen Azure perinteinen portaalissa.

2.  Valitse **Brightspace mukaan Desire2Learn **sovelluksen integrointi-sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC798964.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Kyllä] (./media/active-directory-saas-brightspace-desire2learn-tutorial/IC767830.png "Kyllä")

Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli. Katso lisätietoja Access-paneelin [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).
