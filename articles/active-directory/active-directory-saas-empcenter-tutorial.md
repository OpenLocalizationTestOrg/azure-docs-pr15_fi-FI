<properties 
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi EmpCenter | Microsoft Azure" 
    description="Opettele käyttämään EmpCenter Azure Active Directory-hakemistosta käyttöön kertakirjautumisen, automaattinen valmistelu ja lisää!" 
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
    ms.date="08/16/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-empcenter"></a>Opetusohjelma: EmpCenter Azure Active Directory-integrointi
  
Tässä opetusohjelmassa tavoitteena on näyttämään Azure ja EmpCenter integrointi.  
Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   Käytössä EmpCenter kertakirjautumisen tilaus
  
Kun olet suorittanut Tässä opetusohjelmassa, olet liittänyt EmpCenter Azure AD-käyttäjien saa oikeuden kertakirjauksen EmpCenter yrityksen sivuston (palveluntarjoajan aloittanut allekirjoitus-) tai käyttämällä [Access-paneelin esittely](active-directory-saas-access-panel-introduction.md)-sovellukseen.
  
Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1.  EmpCenter sovelluksen integrointi ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Määrittäminen käyttäjän valmistelu
4.  Käyttäjien määrittäminen

![Skenaario] (./media/active-directory-saas-empcenter-tutorial/IC802916.png "Skenaario")
##<a name="enabling-the-application-integration-for-empcenter"></a>EmpCenter sovelluksen integrointi ottaminen käyttöön
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön EmpCenter sovelluksen integrointi.

###<a name="to-enable-the-application-integration-for-empcenter-perform-the-following-steps"></a>EmpCenter sovelluksen integrointi käyttöön toimimalla seuraavasti:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-empcenter-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-empcenter-tutorial/IC700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-empcenter-tutorial/IC749321.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-empcenter-tutorial/IC749322.png "Lisää sovellus-gallerry")

6.  Kirjoita **hakuruutuun** **EmpCenter**.

    ![Sovelluksen valikoima] (./media/active-directory-saas-empcenter-tutorial/IC802917.png "Sovelluksen valikoima")

7.  Valitse tulosruudussa **EmpCenter**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![EmpCentral] (./media/active-directory-saas-empcenter-tutorial/IC802918.png "EmpCentral")
##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjien todentamiseen EmpCenter käyttämällä SAML-protokollan perusteella federation Azure AD-tilin kanssa.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Valitse Azure perinteinen portaalissa integrointi **EmpCenter** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-empcenter-tutorial/IC802919.png "Kertakirjautumisen määrittäminen")

2.  **Miten käyttäjät voivat kirjautua EmpCenter haluat käyttää** sivulla Valitse **Microsoft Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-empcenter-tutorial/IC802920.png "Kertakirjautumisen määrittäminen")

3.  Suorita **Sovellus asetusten määrittäminen** -sivulla seuraavasti:

    ![Sovelluksen asetusten määrittäminen] (./media/active-directory-saas-empcenter-tutorial/IC802921.png "Sovelluksen asetusten määrittäminen")

    1.  Kirjoita **Merkki-URL** -tekstiruutuun Sign EmpCenter sovelluksen käyttäjille käyttämä URL-osoite (esimerkiksi: *https://partner-authenticati.empcenter.com/workforce/SSO.do*).
    2.  Valitse **Seuraava**

4.  Valitse **Määritä kertakirjautumisen osoitteessa EmpCenter** -sivulla lataamaan metatietojen valitsemalla **Lataa metatietojen**, ja Tallenna tietokoneessa metatiedot-tiedostoon.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-empcenter-tutorial/IC802922.png "Kertakirjautumisen määrittäminen")

5.  Voit lähettää ladatut metatietojen tiedoston EmpCenter tukihenkilöstölle.

    >[AZURE.NOTE] EmpCenter tukihenkilöstö on suoritettava todellinen SSO-määritys.
Näyttöön tulee ilmoitus, kun SSO on otettu käyttöön tilauksen.

6.  Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Valmis** **Määrittäminen kertakirjautuminen** -valintaikkunassa Sulje.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-empcenter-tutorial/IC802923.png "Kertakirjautumisen määrittäminen")
##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu
  
Jos haluat ottaa käyttöön Azure AD-käyttäjät voivat kirjautua sisään EmpCenter, ne on valmisteltava EmpCenter kyselyjä.  
Kyseessä EmpCenter käyttäjätilit on luotava EmpCenter tukihenkilöstölle.

>[AZURE.NOTE] Voit käyttää mitä tahansa muita EmpCenter käyttäjän tilin luontityökalut tai ohjelmointirajapinnan myöntämä EmpCenter valmistelu Azure Active Directory käyttäjätilit.

##<a name="assigning-users"></a>Käyttäjien määrittäminen
  
Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-empcenter-perform-the-following-steps"></a>Jos haluat määrittää käyttäjiä EmpCenter, toimi seuraavasti:

1.  Testaa tilin luominen Azure perinteinen portaalissa.

2.  Valitse integration **EmpCenter **sovellus-sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-empcenter-tutorial/IC802924.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Kyllä] (./media/active-directory-saas-empcenter-tutorial/IC767830.png "Kyllä")
  
Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli. Katso lisätietoja Access-paneelin [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).