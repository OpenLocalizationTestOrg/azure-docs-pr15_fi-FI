<properties 
    pageTitle="Opetusohjelma: Kankkulan Azure Active Directory-integrointi: järjestelmien | Microsoft Azure" 
    description="Opettele käyttämään Kankkulan: järjestelmien Azure Active Directoryn käyttöön kertakirjautumisen, automaattinen valmistelu ja muiden kanssa!" 
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

#<a name="tutorial-azure-active-directory-integration-with-fm-systems"></a>Opetusohjelma: Kankkulan Azure Active Directory-integrointi: järjestelmät
  
Tässä opetusohjelmassa tavoitteena on näyttämään Azure ja FM:Systems integrointi.  
Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   Käytössä FM:Systems kertakirjautumisen tilaus
  
Kun olet suorittanut Tässä opetusohjelmassa, olet liittänyt FM:Systems Azure AD-käyttäjien saa oikeuden kertakirjauksen FM:Systems yrityksesi sivuston (palveluntarjoajan aloittanut allekirjoitus-) tai käyttämällä [Access-paneelin esittely](active-directory-saas-access-panel-introduction.md)-sovellukseen.
  
Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1.  FM:Systems sovelluksen integrointi ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Määrittäminen käyttäjän valmistelu
4.  Käyttäjien määrittäminen

![Skenaario] (./media/active-directory-saas-fm-systems-tutorial/IC795899.png "Skenaario")
##<a name="enabling-the-application-integration-for-fmsystems"></a>FM:Systems sovelluksen integrointi ottaminen käyttöön
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön FM:Systems sovelluksen integrointi.

###<a name="to-enable-the-application-integration-for-fmsystems-perform-the-following-steps"></a>FM:Systems sovelluksen integrointi käyttöön toimimalla seuraavasti:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-fm-systems-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-fm-systems-tutorial/IC700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-fm-systems-tutorial/IC749321.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-fm-systems-tutorial/IC749322.png "Lisää sovellus-gallerry")

6.  Kirjoita **hakuruutuun** **FM:Systems**.

    ![Sovelluksen valikoima] (./media/active-directory-saas-fm-systems-tutorial/IC795900.png "Sovelluksen valikoima")

7.  Valitse tulosruudussa **FM:Systems**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![Kankkulan: järjestelmien] (./media/active-directory-saas-fm-systems-tutorial/IC800213.png "Kankkulan: järjestelmien")
##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjien todentamiseen FM:Systems niiden tilillä Azure AD käyttämällä SAML-protokollan perusteella federation.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Valitse Azure perinteinen portaalissa integrointi **FM:Systems** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-fm-systems-tutorial/IC790810.png "Kertakirjautumisen määrittäminen")

2.  **Miten käyttäjät voivat kirjautua FM:Systems haluat käyttää** sivulla Valitse **Microsoft Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-fm-systems-tutorial/IC795901.png "Kertakirjautumisen määrittäminen")

3.  Tee **Määrittäminen sovelluksen URL-osoite** -sivulla seuraavat toimet:

    ![Sovelluksen URL-Osoitteen määrittäminen] (./media/active-directory-saas-fm-systems-tutorial/IC795902.png "Sovelluksen URL-Osoitteen määrittäminen")

    1.  Kirjoita **FM:Systems merkki-URL** -tekstiruutuun FM:Systems **Vastaa URL-osoite** (esimerkiksi: *https://dpr.fmshosted.com/fminteract/ConsumerService2.aspx*).  

        >[AZURE.WARNING] Saat tämän arvon oman Kankkulan: järjestelmien tekniseen tukeen.

    2.  Valitse **Seuraava**

4.  Valitse **Määritä kertakirjautumisen osoitteessa FM:Systems** -sivulla lataamaan metatietojen valitsemalla **Lataa metatietojen**, ja Tallenna metatiedot tietokoneeseen.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-fm-systems-tutorial/IC795903.png "Kertakirjautumisen määrittäminen")

5.  Metatietojen ladattu tiedosto lähetetään oman Kankkulan: järjestelmien tekniseen tukeen.

    >[AZURE.NOTE] Oman Kankkulan: Järjestelmien tukihenkilöstön on suoritettava todellinen SSO-määritys.
Näyttöön tulee ilmoitus, kun SSO on otettu käyttöön tilauksen.

6.  Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Valmis** **Määrittäminen kertakirjautuminen** -valintaikkunassa Sulje.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-fm-systems-tutorial/IC795904.png "Kertakirjautumisen määrittäminen")
##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu
  
Jos haluat ottaa käyttöön Azure AD-käyttäjät voivat kirjautua sisään FM:Systems, ne on valmisteltava FM:Systems kyselyjä.  
FM:Systems valmistelu on manuaalinen tehtävä.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Voit määrittää käyttäjän valmistelu toimimalla seuraavasti:

1.  Web-selainikkunassa Kirjaudu sisään FM:Systems yrityksesi sivuston järjestelmänvalvojan oikeuksilla.

2.  Siirry **järjestelmänvalvontaan \> suojauksen hallinta \> käyttäjien \> käyttäjäluettelon**.

    ![Järjestelmän hallinta] (./media/active-directory-saas-fm-systems-tutorial/IC795905.png "Järjestelmän hallinta")

3.  Valitse **Luo uusi käyttäjä**.

    ![Luo uusi käyttäjä] (./media/active-directory-saas-fm-systems-tutorial/IC795906.png "Luo uusi käyttäjä")

4.  Valitse **Luo käyttäjä** -osassa seuraavasti:

    ![Käyttäjän luominen] (./media/active-directory-saas-fm-systems-tutorial/IC795907.png "Käyttäjän luominen")

    1.  Kirjoita käyttäjänimi, salasana ja sen vahvistus, sähköpostiosoite ja kelvollinen Azure Active Directory-tilin haluat valmistelu kyselyjä liittyvät tekstiruutujen Työntekijätunnus.
    2.  Valitse **Seuraava**.

>[AZURE.NOTE] Voit käyttää mitä tahansa muita FM:Systems käyttäjän tilin luontityökalut tai ohjelmointirajapinnan myöntämä FM:Systems säännöstä AAD käyttäjätileille.

##<a name="assigning-users"></a>Käyttäjien määrittäminen
  
Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-fmsystems-perform-the-following-steps"></a>Jos haluat määrittää käyttäjiä FM:Systems, toimi seuraavasti:

1.  Testaa tilin luominen Azure perinteinen portaalissa.

2.  Valitse integration **FM:Systems **sovellus-sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-fm-systems-tutorial/IC795908.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Kyllä] (./media/active-directory-saas-fm-systems-tutorial/IC767830.png "Kyllä")
  
Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli. Katso lisätietoja Access-paneelin [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).