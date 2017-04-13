<properties 
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi Chromeriver | Microsoft Azure" 
    description="Opettele käyttämään Chromeriver Azure Active Directory-hakemistosta käyttöön kertakirjautumisen, automaattinen valmistelu ja lisää!" 
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


#<a name="tutorial-azure-active-directory-integration-with-chromeriver"></a>Opetusohjelma: Chromeriver Azure Active Directory-integrointi

Tässä opetusohjelmassa tavoitteena on näyttämään Azure ja Chromeriver integrointi.  
Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   Käytössä Chromeriver kertakirjautumisen tilaus

Kun olet suorittanut Tässä opetusohjelmassa, olet liittänyt Chromeriver Azure AD-käyttäjien saa oikeuden kertakirjauksen Chromeriver yrityksesi sivuston (palveluntarjoajan aloittanut allekirjoitus-) tai käyttämällä [Access-paneelin esittely](active-directory-saas-access-panel-introduction.md)-sovellukseen.

Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1.  Chromeriver sovelluksen integrointi ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Määrittäminen käyttäjän valmistelu
4.  Käyttäjien määrittäminen

![Skenaario] (./media/active-directory-saas-chromeriver-tutorial/IC802755.png "Skenaario")
##<a name="enabling-the-application-integration-for-chromeriver"></a>Chromeriver sovelluksen integrointi ottaminen käyttöön

Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön Chromeriver sovelluksen integrointi.

###<a name="to-enable-the-application-integration-for-chromeriver-perform-the-following-steps"></a>Chromeriver sovelluksen integrointi käyttöön toimimalla seuraavasti:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-chromeriver-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-chromeriver-tutorial/IC700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-chromeriver-tutorial/IC749321.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-chromeriver-tutorial/IC749322.png "Lisää sovellus-gallerry")

6.  Kirjoita **hakuruutuun** **Chromeriver**.

    ![Sovelluksen valikoima] (./media/active-directory-saas-chromeriver-tutorial/IC802756.png "Sovelluksen valikoima")

7.  Valitse tulosruudussa **Chromeriver**ja valitse **Valmis** , voit lisätä sovelluksen.
##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen

Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjien todentamiseen Chromeriver käyttämällä SAML-protokollan perusteella federation Azure AD-tilin kanssa.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Valitse Azure perinteinen portaalissa integrointi **Chromeriver** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-chromeriver-tutorial/IC802757.png "Kertakirjautumisen määrittäminen")

2.  **Miten käyttäjät voivat kirjautua Chromeriver haluat käyttää** sivulla Valitse **Microsoft Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-chromeriver-tutorial/IC802758.png "Kertakirjautumisen määrittäminen")

3.  Suorita **Sovellus asetusten määrittäminen** -sivulla seuraavasti:

    ![Sovelluksen asetusten määrittäminen] (./media/active-directory-saas-chromeriver-tutorial/IC802759.png "Sovelluksen asetusten määrittäminen")

    1.  Kirjoita **Vastaus URL** -tekstiruutuun Chromeriver **AssertionConsumerService URL-osoite** (esimerkiksi: *https://qa-app.chromeriver.com/login/sso/saml/consume?customerId=911*).  

        >[AZURE.NOTE] Saat tämän arvon Chromeriver tukihenkilöstölle.

    2.  Valitse **Seuraava**

4.  Valitse **Määritä kertakirjautumisen osoitteessa Chromeriver** -sivulla lataamaan metatietojen valitsemalla **Lataa metatietojen**, ja Tallenna tietokoneessa metatiedot-tiedostoon.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-chromeriver-tutorial/IC802760.png "Kertakirjautumisen määrittäminen")

5.  Voit lähettää ladatut metatietojen tiedoston Chromeriver tukihenkilöstölle.

    >[AZURE.NOTE]Chromeriver tukihenkilöstö on suoritettava todellinen SSO-määritys.  
    Näyttöön tulee ilmoitus, kun SSO on otettu käyttöön tilauksen.

6.  Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Valmis** **Määrittäminen kertakirjautuminen** -valintaikkunassa Sulje.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-chromeriver-tutorial/IC802761.png "Kertakirjautumisen määrittäminen")
##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu

Jos haluat ottaa käyttöön Azure AD-käyttäjät voivat kirjautua sisään Chromeriver, ne on valmisteltava Chromeriver kyselyjä.  
Kyseessä Chromeriver käyttäjätilit on luotava Chromeriver tukihenkilöstölle.

>[AZURE.NOTE] Voit käyttää mitä tahansa muita Chromeriver käyttäjän tilin luontityökalut tai ohjelmointirajapinnan myöntämä Chromeriver valmistelu Azure Active Directory käyttäjätilit.

##<a name="assigning-users"></a>Käyttäjien määrittäminen

Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-chromeriver-perform-the-following-steps"></a>Jos haluat määrittää käyttäjiä Chromeriver, toimi seuraavasti:

1.  Testaa tilin luominen Azure perinteinen portaalissa.

2.  Valitse integration **Chromeriver **sovellus-sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-chromeriver-tutorial/IC802762.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Kyllä] (./media/active-directory-saas-chromeriver-tutorial/IC767830.png "Kyllä")

Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli. Katso lisätietoja Access-paneelin [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).
