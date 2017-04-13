<properties 
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi Clever | Microsoft Azure" 
    description="Opettele käyttämään Clever Azure Active Directory-hakemistosta käyttöön kertakirjautumisen, automaattinen valmistelu ja lisää!" 
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

#<a name="tutorial-azure-active-directory-integration-with-clever"></a>Opetusohjelma: Clever Azure Active Directory-integrointi

Tässä opetusohjelmassa tavoitteena on näyttämään Azure ja Clever integrointi. Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   Kekseliäs palvelutili

Kun olet suorittanut Tässä opetusohjelmassa, olet liittänyt Clever Azure AD-käyttäjien saa oikeuden kertakirjauksen kekseliäs yrityksesi sivuston (palveluntarjoajan aloittanut allekirjoitus-) tai käyttämällä [Access-paneelin esittely](active-directory-saas-access-panel-introduction.md)-sovellukseen.

Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1.  Clever sovelluksen integrointi ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Määrittäminen käyttäjän valmistelu
4.  Käyttäjien määrittäminen

![Skenaario] (./media/active-directory-saas-clever-tutorial/IC798977.png "Skenaario")
##<a name="enabling-the-application-integration-for-clever"></a>Clever sovelluksen integrointi ottaminen käyttöön

Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön Clever sovelluksen integrointi.

###<a name="to-enable-the-application-integration-for-clever-perform-the-following-steps"></a>Clever sovelluksen integrointi käyttöön toimimalla seuraavasti:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-clever-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-clever-tutorial/IC700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-clever-tutorial/IC749321.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-clever-tutorial/IC749322.png "Lisää sovellus-gallerry")

6.  Kirjoita **hakuruutuun** **Clever**.

    ![Sovelluksen valikoima] (./media/active-directory-saas-clever-tutorial/IC798978.png "Sovelluksen valikoima")

7.  Valitse tulosruudussa **Clever**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![Kekseliäs] (./media/active-directory-saas-clever-tutorial/IC798979.png "Kekseliäs")
##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen

Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjien todentamiseen Clever käyttämällä SAML-protokollan perusteella federation Azure AD-tilin kanssa.  
Kekseliäs sovelluksesi odottaa SAML vahvistukset tietyssä muodossa, jossa pyydetään Lisää mukautettujen määritteiden yhdistämismäärityksiä **saml suojaustunnuksen määritteet** -määrityksiin.  
Seuraavassa näyttökuvassa näkyy esimerkki tämän.

![Määritteet] (./media/active-directory-saas-clever-tutorial/IC798980.png "Määritteet")

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Valitse Azure perinteinen portaalissa integrointi **Clever** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-clever-tutorial/IC784682.png "Kertakirjautumisen määrittäminen")

2.  **Miten käyttäjät voivat kirjautua Clever haluat käyttää** sivulla Valitse **Microsoft Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-clever-tutorial/IC798981.png "Kertakirjautumisen määrittäminen")

3.  Kirjoita **Määritä sovelluksen URL-osoite** -sivulla **Kekseliäs merkki-URL** -tekstiruutuun Sign kekseliäs sovelluksen käyttäjille käyttämä URL-osoite (esimerkiksi: *https://clever.com/in/azsandbox*), ja valitse sitten **Seuraava**.

    ![Sovelluksen URL-Osoitteen määrittäminen] (./media/active-directory-saas-clever-tutorial/IC798982.png "Sovelluksen URL-Osoitteen määrittäminen")

4.  Valitse **Määritä kertakirjautumisen osoitteessa Clever** -sivulla lataamaan metatietojen valitsemalla **Lataa metatietojen**, ja Tallenna paikallisesti tietokoneen metatiedot-tiedostoon.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-clever-tutorial/IC798983.png "Kertakirjautumisen määrittäminen")

5.  Eri selainikkunassa Kirjaudu kekseliäs yrityksesi sivuston järjestelmänvalvojan oikeuksilla.

6.  Valitse työkaluriviltä **Pikaviestien kirjautuminen**.

    ![Pikaviestien kirjautuminen] (./media/active-directory-saas-clever-tutorial/IC798984.png "Pikaviestien kirjautuminen")

7.  Tee **Pikaviestien Kirjaudu sisään** -sivulla seuraavasti:

    ![Pikaviestien kirjautuminen] (./media/active-directory-saas-clever-tutorial/IC798985.png "Pikaviestien kirjautuminen")

    1.  Kirjoita **Login URL-osoite**.  

        >[AZURE.NOTE] **Kirjaudu sisään URL-osoite** on mukautettu arvo.
Saat todellinen arvo kekseliäs tukihenkilöstölle.

    2.  **Käyttäjätietojen järjestelmän**Valitse **ADFS**.
    3.  Valitse **Tallenna**.

8.  Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Valmis** **Määrittäminen kertakirjautuminen** -valintaikkunassa Sulje.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-clever-tutorial/IC798986.png "Kertakirjautumisen määrittäminen")

9.  Valitse ylä-valikossa **määritteet** **SAML suojaustunnuksen määritteet** -valintaikkunan avaaminen.

    ![Määritteet] (./media/active-directory-saas-clever-tutorial/IC795920.png "Määritteet")

10. Lisää pakollinen määrite yhdistämismääritykset, toimi seuraavasti:

    ![SAML-tunnuksen määritteet] (./media/active-directory-saas-clever-tutorial/IC795921.png "SAML-tunnuksen määritteet")

  	|Määritteen nimi|Määritteen arvo|
  	|---|---|
  	|clever.Student.Credentials.District\_käyttäjänimi|User.UserPrincipalName|

    1.  Tietoja kullekin riville edellä olevan taulukon Valitse **Lisää käyttäjä-määrite**.
    2.  Kirjoita määritteen nimi näkyy kyseisen rivin **Määritteen nimi** -tekstiruutuun.
    3.  Valitse rivin määritteen arvo **Määritteen arvo** -tekstiruutuun.
    4.  Valitse **Valmis**.

11. Valitse **Ota muutokset käyttöön**.

##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu

Jos haluat ottaa käyttöön Azure AD-käyttäjät voivat kirjautua sisään Clever, ne on valmisteltava Clever kyselyjä.  
Clever valmistelu on manuaalinen tehtävä on suoritettava kekseliäs tuki.

>[AZURE.NOTE] Voit käyttää mitä tahansa muita kekseliäs käyttäjän tilin luominen työkaluja tai ohjelmointirajapinnan myöntämä Clever säännöstä AAD käyttäjätileille.

##<a name="assigning-users"></a>Käyttäjien määrittäminen

Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-clever-perform-the-following-steps"></a>Jos haluat määrittää käyttäjiä Clever, toimi seuraavasti:

1.  Testaa tilin luominen Azure perinteinen portaalissa.

2.  Valitse integration **Clever **sovellus-sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-clever-tutorial/IC798987.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Kyllä] (./media/active-directory-saas-clever-tutorial/IC767830.png "Kyllä")

Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli. Katso lisätietoja Access-paneelin [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).
