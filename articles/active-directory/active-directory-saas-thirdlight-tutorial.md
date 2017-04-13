<properties 
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi Thirdlight | Microsoft Azure" 
    description="Opettele käyttämään Thirdlight Azure Active Directory-hakemistosta käyttöön kertakirjautumisen, automaattinen valmistelu ja lisää!" 
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

#<a name="tutorial-azure-active-directory-integration-with-thirdlight"></a>Opetusohjelma: Thirdlight Azure Active Directory-integrointi
  
Tässä opetusohjelmassa tavoitteena on näyttämään Azure ja Thirdlight integrointi.  
Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   Käytössä Thirdlight kertakirjautumisen tilaus
  
Kun olet suorittanut Tässä opetusohjelmassa, olet liittänyt Thirdlight Azure AD-käyttäjien saa oikeuden kertakirjauksen Thirdlight yrityksesi sivuston (palveluntarjoajan aloittanut allekirjoitus-) tai käyttämällä [Access-paneelin esittely](active-directory-saas-access-panel-introduction.md)-sovellukseen.
  
Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1.  Thirdlight sovelluksen integrointi ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Määrittäminen käyttäjän valmistelu
4.  Käyttäjien määrittäminen

![Skenaario] (./media/active-directory-saas-thirdlight-tutorial/IC805836.png "Skenaario")

##<a name="enabling-the-application-integration-for-thirdlight"></a>Thirdlight sovelluksen integrointi ottaminen käyttöön
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön Thirdlight sovelluksen integrointi.

###<a name="to-enable-the-application-integration-for-thirdlight-perform-the-following-steps"></a>Thirdlight sovelluksen integrointi käyttöön toimimalla seuraavasti:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-thirdlight-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-thirdlight-tutorial/IC700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-thirdlight-tutorial/IC749321.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-thirdlight-tutorial/IC749322.png "Lisää sovellus-gallerry")

6.  Kirjoita **hakuruutuun** **Thirdlight**.

    ![Sovelluksen valikoima] (./media/active-directory-saas-thirdlight-tutorial/IC805837.png "Sovelluksen valikoima")

7.  Valitse tulosruudussa **Thirdlight**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![ThirdLight] (./media/active-directory-saas-thirdlight-tutorial/IC805838.png "ThirdLight")

##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjien todentamiseen Thirdlight käyttämällä SAML-protokollan perusteella federation Azure AD-tilin kanssa.  
Kertakirjautumisen varten Thirdlight määrittäminen edellyttää, että allekirjoitus arvon noutaa varmenne.  
Jos et ole tottunut näin, katso, [miten voit hakea varmenteen allekirjoitus-arvon](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Valitse Azure perinteinen portaalissa integrointi **Thirdlight** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-thirdlight-tutorial/IC805839.png "Kertakirjautumisen määrittäminen")

2.  **Miten käyttäjät voivat kirjautua Thirdlight haluat käyttää** sivulla Valitse **Microsoft Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-thirdlight-tutorial/IC805840.png "Kertakirjautumisen määrittäminen")

3.  Kirjoita **Määrittäminen sovelluksen URL-osoite** -sivulla **Thirdlight merkki-URL** -tekstiruutuun käyttää käyttäjien kirjautua Thirdlight sovelluksen URL-osoite (esimerkiksi: "*http://azuresso2.thirdlight.com/*"), ja valitse sitten **Seuraava**.

    ![Sovelluksen URL-Osoitteen määrittäminen] (./media/active-directory-saas-thirdlight-tutorial/IC805841.png "Sovelluksen URL-Osoitteen määrittäminen")

4.  Valitse **Määritä kertakirjautumisen osoitteessa Thirdlight** -sivulla lataamaan metatietojen valitsemalla **Lataa metatietojen**, ja Tallenna paikallisesti tietokoneen metatiedot-tiedostoon.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-thirdlight-tutorial/IC805842.png "Kertakirjautumisen määrittäminen")

5.  Eri selainikkunassa Kirjaudu Thirdlight yrityksesi sivuston järjestelmänvalvojan oikeuksilla.

6.  Siirry **määritysten \> järjestelmänvalvontaan**, ja valitse sitten **SAML2**.

    ![Järjestelmän hallinta] (./media/active-directory-saas-thirdlight-tutorial/IC805843.png "Järjestelmän hallinta")

7.  SAML2 määritykset-osasta toimimalla seuraavasti:

    ![SAML kertakirjautuminen] (./media/active-directory-saas-thirdlight-tutorial/IC805844.png "SAML kertakirjautuminen")

    1.  Valitse **Ota käyttöön SAML2 kertakirjautumisen**.
    2.  **Tietolähteeseen yhdistetyn IdP metatietoja**Valitse **Lataa IdP metatietojen XML: stä**.
    3.  Avaa ladatut metatieto-tiedosto, sisällön kopiointi ja liittäminen **IdP metatietojen XML** -tekstiruutu.
    4.  Valitse **Tallenna SAML2 asetukset**.

8.  Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Valmis** **Määrittäminen kertakirjautuminen** -valintaikkunassa Sulje.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-thirdlight-tutorial/IC805845.png "Kertakirjautumisen määrittäminen")

##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu
  
Jos haluat ottaa käyttöön Azure AD-käyttäjät voivat kirjautua sisään Thirdlight, ne on valmisteltava Thirdlight kyselyjä.  
Thirdlight valmistelu on manuaalinen tehtävä.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Voit määrittää käyttäjän valmistelu toimimalla seuraavasti:

1.  Kirjaudu sisään **Thirdlight** yrityksesi sivuston järjestelmänvalvojan oikeuksilla.

2.  Siirry **käyttäjät** -välilehti.

3.  Valitse **käyttäjät ja ryhmät**.

4.  Napsauta **Lisää uusi käyttäjä** -painiketta.

5.  Kirjoita **käyttäjänimi, nimen tai kuvauksen, sähköpostin, valitse esiasetus tai uusia jäseniä ryhmään** AAD tilille, jonka haluat säätää.

6.  Valitse **Luo**.

>[AZURE.NOTE] Voit käyttää mitä tahansa muita Thirdlight käyttäjän tilin luontityökalut tai ohjelmointirajapinnan myöntämä Thirdlight säännöstä AAD käyttäjätileille.

##<a name="assigning-users"></a>Käyttäjien määrittäminen
  
Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-thirdlight-perform-the-following-steps"></a>Jos haluat määrittää käyttäjiä Thirdlight, toimi seuraavasti:

1.  Testaa tilin luominen Azure perinteinen portaalissa.

2.  Valitse integration **Thirdlight **sovellus-sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-thirdlight-tutorial/IC805846.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Kyllä] (./media/active-directory-saas-thirdlight-tutorial/IC767830.png "Kyllä")
  
Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli. Katso lisätietoja Access-paneelin [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).