<properties 
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi RightAnswers | Microsoft Azure" 
    description="Opettele käyttämään RightAnswers Azure Active Directory-hakemistosta käyttöön kertakirjautumisen, automaattinen valmistelu ja lisää!" 
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
    ms.date="09/26/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-rightanswers"></a>Opetusohjelma: RightAnswers Azure Active Directory-integrointi
  
Tässä opetusohjelmassa tavoitteena on näyttämään Azure ja RightAnswers integrointi. Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   Käytössä RightAnswers kertakirjautumisen tilaus
  
Kun olet suorittanut Tässä opetusohjelmassa, olet liittänyt RightAnswers Azure AD-käyttäjien saa oikeuden kertakirjauksen käyttämällä [Access-paneelin esittely](active-directory-saas-access-panel-introduction.md)-sovellukseen.
  
Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1.  RightAnswers sovelluksen integrointi ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Määrittäminen käyttäjän valmistelu
4.  Käyttäjien määrittäminen

![Skenaario] (./media/active-directory-saas-rightanswers-tutorial/IC802925.png "Skenaario")
##<a name="enabling-the-application-integration-for-rightanswers"></a>RightAnswers sovelluksen integrointi ottaminen käyttöön
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön RightAnswers sovelluksen integrointi.

###<a name="to-enable-the-application-integration-for-rightanswers-perform-the-following-steps"></a>RightAnswers sovelluksen integrointi käyttöön toimimalla seuraavasti:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-rightanswers-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-rightanswers-tutorial/IC700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-rightanswers-tutorial/IC749321.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-rightanswers-tutorial/IC749322.png "Lisää sovellus-gallerry")

6.  Kirjoita **hakuruutuun** **RightAnswers**.

    ![Sovelluksen valikoima] (./media/active-directory-saas-rightanswers-tutorial/IC802926.png "Sovelluksen valikoima")

7.  Valitse tulosruudussa **RightAnswers**ja valitse **Valmis** , voit lisätä sovelluksen.
##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjien todentamiseen RightAnswers käyttämällä SAML-protokollan perusteella federation Azure AD-tilin kanssa.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Valitse Azure perinteinen portaalissa integrointi **RightAnswers** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-rightanswers-tutorial/IC802927.png "Kertakirjautumisen määrittäminen")

2.  **Miten käyttäjät voivat kirjautua RightAnswers haluat käyttää** sivulla Valitse **Microsoft Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-rightanswers-tutorial/IC802928.png "Kertakirjautumisen määrittäminen")

3.  Kirjoita **Määrittää sovelluksen asetukset** -sivun **Merkki-URL** -tekstiruutuun Sign RightAnswers sovelluksen käyttäjille käyttämä URL-osoite (esimerkiksi: *https://fortify.rightanswers.com/portal/ss/*), ja valitse sitten **Seuraava**.

    ![Sovelluksen asetusten määrittäminen] (./media/active-directory-saas-rightanswers-tutorial/IC802929.png "Sovelluksen asetusten määrittäminen")

4.  Valitse **Määritä kertakirjautumisen osoitteessa RightAnswers** -sivulla lataamaan metatietojen valitsemalla **Lataa metatietojen**, ja Tallenna paikallisesti tietokoneen metatiedot-tiedostoon.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-rightanswers-tutorial/IC802930.png "Kertakirjautumisen määrittäminen")

5.  Voit lähettää ladatut metatietojen tiedoston RightAnswers tukihenkilöstölle.

    >[AZURE.NOTE] RightAnswers tukihenkilöstö on suoritettava todellinen SSO-määritys.
Näyttöön tulee ilmoitus, kun SSO on otettu käyttöön tilauksen.

6.  Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Valmis** **Määrittäminen kertakirjautuminen** -valintaikkunassa Sulje.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-rightanswers-tutorial/IC802931.png "Kertakirjautumisen määrittäminen")
##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu
  
Jos haluat ottaa käyttöön Azure AD-käyttäjät voivat kirjautua sisään RightAnswers, ne on valmisteltava RightAnswers kyselyjä.  
RightAnswers valmistelu on Automaattinen tehtävä.  
Ei, ei toimi.
  
Käyttäjät luodaan automaattisesti tarvittaessa ensimmäinen yksittäisen Sign yritys aikana.

>[AZURE.NOTE]Voit käyttää mitä tahansa muita RightAnswers käyttäjän tilin luontityökalut tai ohjelmointirajapinnan myöntämä RightAnswers säännöstä AAD käyttäjätileille.

##<a name="assigning-users"></a>Käyttäjien määrittäminen
  
Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-rightanswers-perform-the-following-steps"></a>Jos haluat määrittää käyttäjiä RightAnswers, toimi seuraavasti:

1.  Testaa tilin luominen Azure perinteinen portaalissa.

2.  Valitse integration **RightAnswers **sovellus-sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-rightanswers-tutorial/IC802932.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Kyllä] (./media/active-directory-saas-rightanswers-tutorial/IC767830.png "Kyllä")
  
Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli. Katso lisätietoja Access-paneelin [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).