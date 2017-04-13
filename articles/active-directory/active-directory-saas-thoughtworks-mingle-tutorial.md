<properties 
    pageTitle="Opetusohjelma: Azure Active Directory integrointi Thoughtworks Mingle | Microsoft Azure" 
    description="Opettele käyttämään Thoughtworks Mingle Azure Active Directory-hakemistosta käyttöön kertakirjautumisen, automaattinen valmistelu ja lisää!" 
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
    ms.date="09/11/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-thoughtworks-mingle"></a>Opetusohjelma: Azure Active Directory integrointi Thoughtworks Mingle
  
Tässä opetusohjelmassa tavoitteena on näyttämään Azure ja Thoughtworks Mingle integrointi.  
Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   Thoughtworks Mingle palvelutili
  
Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1.  -Sovelluksen integrointi Thoughtworks Mingle ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Määrittäminen käyttäjän valmistelu
4.  Käyttäjien määrittäminen

![Skenaario] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785150.png "Skenaario")

##<a name="enabling-the-application-integration-for-thoughtworks-mingle"></a>-Sovelluksen integrointi Thoughtworks Mingle ottaminen käyttöön
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön sovelluksen integrointi Thoughtworks Mingle.

###<a name="to-enable-the-application-integration-for-thoughtworks-mingle-perform-the-following-steps"></a>Voit ottaa käyttöön sovelluksen-integrointia varten Thoughtworks Mingle toimimalla seuraavasti:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC749321.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC749322.png "Lisää sovellus-gallerry")

6.  Kirjoita **hakuruutuun**, **thoughtworks mingle**.

    ![Sovelluksen valikoima] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785151.png "Sovelluksen valikoima")

7.  Valitse tulosruudussa **Thoughtworks Mingle**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![Thoughtworks Mingle] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785152.png "Thoughtworks Mingle")

##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjien todentamiseen Thoughtworks Mingle niiden tilillä Azure AD federation perusteella SAML-protokollan avulla.  
Tämän toimintosarjan osana tarvitaan Thoughtworks Mingle varmenteen lataaminen.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Valitse Azure perinteinen portal-sivulla **Thoughtworks Mingle **sovelluksen integrointi, **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785153.png "Määritä kertakirjautuminen")

2.  **Miten käyttäjät voivat kirjautua Thoughtworks Mingle haluat käyttää** sivulla Valitse **Microsoft Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785154.png "Määritä kertakirjautuminen")

3.  **Määritä sovelluksen URL-osoite** -sivulla **Thoughtworks Mingle vuokraajan URL** -tekstiruutuun Kirjoita käyttämällä seuraavaa kaavaa "*http://company.mingle.thoughtworks.com*" URL-osoite ja valitse sitten **Seuraava**.

    ![Sovelluksen URL-Osoitteen määrittäminen] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785155.png "Sovelluksen URL-Osoitteen määrittäminen")

4.  Valitse **Määritä kertakirjautumisen osoitteessa Thoughtworks Mingle** -sivulla Lataa metatiedot ja tallenna se tietokoneeseen.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785156.png "Määritä kertakirjautuminen")

5.  Kirjaudu **Thoughtworks Mingle** yritys-sivustoon järjestelmänvalvojana.

6.  Valitse **järjestelmänvalvoja** -välilehteä ja valitse **SSO Config**.

    ![SSO Config] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785157.png "SSO Config")

7.  **SSO Config** -osassa seuraavasti:

    ![SSO Config] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785158.png "SSO Config")

    1.  Voit ladata metatieto-tiedosto valitsemalla **Valitse tiedosto**.
    2.  Valitse **Tallenna muutokset**.

8.  Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Valmis** **Määrittäminen kertakirjautuminen** -valintaikkunassa Sulje.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785159.png "Määritä kertakirjautuminen")

##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu
  
AAD käyttäjät voivat kirjautua ne on valmisteltava käyttämällä Azure Active Directory-käyttäjätunnuksen Thoughtworks Mingle-sovellukseen.  
Thoughtworks Mingle valmistelu on manuaalinen tehtävä.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Voit määrittää käyttäjän valmistelu toimimalla seuraavasti:

1.  Kirjaudu Thoughtworks Mingle yritys-sivustoon järjestelmänvalvojana.

2.  Valitse **profiili**.

    ![Ensimmäinen projektin] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785160.png "Ensimmäinen projektin")

3.  Valitse **järjestelmänvalvoja** -välilehteä ja valitse sitten **käyttäjät**.

    ![Käyttäjät] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785161.png "Käyttäjät")

4.  Valitse **Uusi käyttäjä**.

    ![Uusi käyttäjä] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785162.png "Uusi käyttäjä")

5.  Valitse **Uusi käyttäjä** -sivulla toimimalla seuraavasti:

    ![Uusi käyttäjä] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785163.png "Uusi käyttäjä")

    1.  Kirjoita **kirjautumisnimi**, **Näyttönimi**, **Valitse salasana**, **Vahvista salasana** kelvollinen AAD tilin haluat säännöstä liittyvät tekstiruutujen kyselyjä.
    2.  **Käyttäjätyyppi**Valitse **täydet käyttöoikeudet**.
    3.  Valitse **Luo profiilia**.

>[AZURE.NOTE] Voit käyttää mitä tahansa muita Thoughtworks Mingle käyttäjän tilin luontityökalut tai ohjelmointirajapinnan myöntämä Thoughtworks Mingle säännöstä AAD käyttäjätileille.

##<a name="assigning-users"></a>Käyttäjien määrittäminen
  
Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-thoughtworks-mingle-perform-the-following-steps"></a>Jos haluat määrittää käyttäjiä Thoughtworks Mingle, toimi seuraavasti:

1.  Testaa tilin luominen Azure perinteinen portaalissa.

2.  **Thoughtworks Mingle** sovelluksen integrointi, napsauta sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC785164.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Kyllä] (./media/active-directory-saas-thoughtworks-mingle-tutorial/IC767830.png "Kyllä")
  
Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli. Katso lisätietoja Access-paneelin [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).
