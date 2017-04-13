<properties 
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi kanssa Aha! | Microsoft Azure" 
    description="Opettele käyttämään Aha! Azure Active Directory, jos haluat ottaa käyttöön kertakirjautumisen automaattinen valmistelu ja paljon muuta." 
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

#<a name="tutorial-azure-active-directory-integration-with-aha"></a>Opetusohjelma: Azure Active Directory-integrointi kanssa Aha!

Tässä opetusohjelmassa tavoitteena on Näytä Azure ja Aha integrointi!  
Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   Aha! kertakirjautumisen käytössä tilaus

Kun olet suorittanut tämän opetusohjelman, Azure AD-käyttäjiä on määritetty Aha! saa oikeuden kertakirjauksen sovellukseen on kohdassa Aha! yrityksen sivuston (service provider aloittanut allekirjoitus-) tai käyttämällä [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).

Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1.  Ottaminen käyttöön Aha sovelluksen integrointi!
2.  Kertakirjautumisen määrittäminen
3.  Määrittäminen käyttäjän valmistelu
4.  Käyttäjien määrittäminen

![Skenaario] (./media/active-directory-saas-aha-tutorial/IC798944.png "Skenaario")
##<a name="enabling-the-application-integration-for-aha"></a>Ottaminen käyttöön Aha sovelluksen integrointi!

Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön Aha sovelluksen integrointi!.

###<a name="to-enable-the-application-integration-for-aha-perform-the-following-steps"></a>Aha sovelluksen-integroinnin valitseminen käyttöön!, toimi seuraavasti:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-aha-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-aha-tutorial/IC700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-aha-tutorial/IC749321.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-aha-tutorial/IC749322.png "Lisää sovellus-gallerry")

6.  Kirjoita **hakuruutuun** **Aha!**.

    ![Sovelluksen valikoima] (./media/active-directory-saas-aha-tutorial/IC798945.png "Sovelluksen valikoima")

7.  Valitse tulokset-ruudussa **Aha!**, ja valitse sitten **Valmis** , voit lisätä sovelluksen.

    ![Aha!] (./media/active-directory-saas-aha-tutorial/IC802746.png "Aha!")
##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen

Tässä osassa on jäsentäminen ottamisesta käyttöön käyttäjien todentamiseen Aha! niiden käyttämällä SAML-protokollan perusteella federation Azure AD-tilillä.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Azure perinteinen-portaalissa Valitse **Aha!** sovelluksen integrointi-sivulla **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-aha-tutorial/IC798946.png "Kertakirjautumisen määrittäminen")

2.  Valitse **miten Haluatko, että käyttäjät voivat kirjautua Aha!** sivun, valitse **Microsoft Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-aha-tutorial/IC798947.png "Kertakirjautumisen määrittäminen")

3.  **Määritä sovelluksen URL-osoite** -sivulla **Aha! Merkki-URL-Osoitteen** tekstiruutu, kirjoita URL-Osoitetta käytetään käyttäjien kertakirjauksen käyttöön oman Aha! Sovelluksen (esimerkiksi: "*https://company.aha.io/session/new*"), ja valitse sitten **Seuraava**.

    ![Sovelluksen URL-Osoitteen määrittäminen] (./media/active-directory-saas-aha-tutorial/IC798948.png "Sovelluksen URL-Osoitteen määrittäminen")

4.  Valitse **osoitteessa Aha kertakirjautumisen määrittäminen!** sivu, lataa metatieto-tiedosto, valitse **Lataa metatiedot**ja valitse Tallenna paikallisesti tietokoneen metatiedot-tiedostoon.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-aha-tutorial/IC798949.png "Kertakirjautumisen määrittäminen")

5.  Kirjaudu eri selainikkunassa oman Aha! yrityksen sivuston järjestelmänvalvojan oikeuksilla.

6.  Valitse ylä-valikossa **asetukset**.

    ![Asetukset] (./media/active-directory-saas-aha-tutorial/IC798950.png "Asetukset")

7.  Valitse **tili**.

    ![Profiili] (./media/active-directory-saas-aha-tutorial/IC798951.png "Profiili")

8.  Valitse **Suojaus- ja kertakirjautuminen**.

    ![Tietoturva ja kertakirjautuminen] (./media/active-directory-saas-aha-tutorial/IC798952.png "Tietoturva ja kertakirjautuminen")

9.  Valitse **SAML2.0**osan **Kertakirjautumisen** **Tunnistetietojen toimittaja**.

    ![Tietoturva ja kertakirjautuminen] (./media/active-directory-saas-aha-tutorial/IC798953.png "Tietoturva ja kertakirjautuminen")

10. Valitse **Kertakirjautumisen** määritys-sivulla toimimalla seuraavasti:

    ![Kertakirjautuminen] (./media/active-directory-saas-aha-tutorial/IC798954.png "Kertakirjautuminen")

    1.  Kirjoita **nimi** -tekstiruutuun kokoonpanosi nimi.
    2.  **Määritä avulla**Valitse **Metatieto-tiedosto**.
    3.  Voit ladata ladatut metatieto-tiedosto valitsemalla **Selaa**.
    4.  Valitse **Päivitä**.

11. Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Valmis** **Määrittäminen kertakirjautuminen** -valintaikkunassa Sulje.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-aha-tutorial/IC798955.png "Kertakirjautumisen määrittäminen")
##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu

Jotta Azure AD-käyttäjät voivat kirjautua Aha!, ne on valmisteltava kyselyjä Aha!.  
Kyseessä Aha!, valmistelu on Automaattinen tehtävä.  
Ei, ei toimi.
  
Käyttäjät luodaan automaattisesti tarvittaessa ensimmäinen yksittäisen Sign yritys aikana.

>[AZURE.NOTE] Voit käyttää muita Aha! käyttäjän tilin luontityökalut tai Aha myöntämä API! valmistelu AAD käyttäjätilit.

##<a name="assigning-users"></a>Käyttäjien määrittäminen

Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-aha-perform-the-following-steps"></a>Voit määrittää käyttäjät Aha!, toimi seuraavasti:

1.  Testaa tilin luominen Azure perinteinen portaalissa.

2.  Valitse **Aha!** sovelluksen integrointi-sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-aha-tutorial/IC798956.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Kyllä] (./media/active-directory-saas-aha-tutorial/IC767830.png "Kyllä")

Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli. Katso lisätietoja Access-paneelin [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).
