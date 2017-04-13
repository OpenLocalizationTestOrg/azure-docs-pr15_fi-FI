<properties 
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi SCC elinkaari | Microsoft Azure" 
    description="Opettele käyttämään SCC elinkaari Azure Active Directory-hakemistosta käyttöön kertakirjautumisen, automaattinen valmistelu ja lisää!" 
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

#<a name="tutorial-azure-active-directory-integration-with-scc-lifecycle"></a>Opetusohjelma: SCC elinkaari Azure Active Directory-integrointi
  
Tässä opetusohjelmassa tavoitteena on näyttämään Azure ja SCC elinkaari integrointi.  
Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   Käytössä SCC elinkaari kertakirjautumisen tilaus
  
Kun olet suorittanut Tässä opetusohjelmassa, olet liittänyt SCC elinkaari Azure AD-käyttäjien saa oikeuden kertakirjauksen SCC elinkaari yrityksesi sivuston (palveluntarjoajan aloittanut allekirjoitus-) tai käyttämällä [Access-paneelin esittely](active-directory-saas-access-panel-introduction.md)-sovellukseen.
  
Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1.  SCC elinkaari sovelluksen integrointi ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Määrittäminen käyttäjän valmistelu
4.  Käyttäjien määrittäminen

![Skenaario] (./media/active-directory-saas-scc-lifecycle-tutorial/IC794120.png "Skenaario")
##<a name="enabling-the-application-integration-for-scc-lifecycle"></a>SCC elinkaari sovelluksen integrointi ottaminen käyttöön
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön SCC elinkaari sovelluksen integrointi.

###<a name="to-enable-the-application-integration-for-scc-lifecycle-perform-the-following-steps"></a>SCC elinkaari sovelluksen integrointi käyttöön toimimalla seuraavasti:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-scc-lifecycle-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-scc-lifecycle-tutorial/IC700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-scc-lifecycle-tutorial/IC749321.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-scc-lifecycle-tutorial/IC749322.png "Lisää sovellus-gallerry")

6.  Kirjoita **hakuruutuun** **SCC elinkaari**.

    ![Sovelluksen valikoima] (./media/active-directory-saas-scc-lifecycle-tutorial/IC794121.png "Sovelluksen valikoima")

7.  Valitse tulosruudussa **SCC elinkaari**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![SCC elinkaari] (./media/active-directory-saas-scc-lifecycle-tutorial/IC795082.png "SCC elinkaari")
##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjien todentamiseen SCC elinkaari käyttämällä SAML-protokollan perusteella federation Azure AD-tilin kanssa.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Valitse Azure perinteinen portaalissa integrointi **SCC elinkaari** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-scc-lifecycle-tutorial/IC794122.png "Kertakirjautumisen määrittäminen")

2.  **Miten käyttäjät voivat kirjautua SCC elinkaari haluat käyttää** sivulla Valitse **Microsoft Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-scc-lifecycle-tutorial/IC794123.png "Kertakirjautumisen määrittäminen")

3.  **Määritä sovelluksen URL-osoite** -sivulla **Merkki-URL** -tekstiruutuun käyttää käyttäjien kirjautua käyttämällä seuraavaa kaavaa "*https://bs1.scc.com/lc7/welcome/customer/PICTtest.aspx*" SCC elinkaari sovelluksen URL-osoite ja valitse sitten **Seuraava**.

    ![Sovelluksen URL-Osoitteen määrittäminen] (./media/active-directory-saas-scc-lifecycle-tutorial/IC794124.png "Sovelluksen URL-Osoitteen määrittäminen")

4.  Valitse **Määritä kertakirjautumisen osoitteessa SCC elinkaari** -sivulla valitsemalla **Lataa metatiedot**ja Tallenna paikallisesti tietokoneen tiedoston metatiedot.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-scc-lifecycle-tutorial/IC795083.png "Kertakirjautumisen määrittäminen")

5.  Lähetä edelleen metatietojen kyseinen tiedosto SCC elinkaari-tukeen.

    >[AZURE.NOTE]Kertakirjautumisen pitää ottaa käyttöön SCC elinkaari-tukeen.

6.  Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Valmis** **Määrittäminen kertakirjautuminen** -valintaikkunassa Sulje.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-scc-lifecycle-tutorial/IC794125.png "Kertakirjautumisen määrittäminen")
##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu
  
Jotta voit ottaa käyttöön Azure AD-käyttäjät voivat kirjautua sisään SCC elinkaari, ne on valmisteltava kyselyjä SCC elinkaari.
  
Ei ei toimi, voit määrittää käyttäjän valmisteleminen SCC elinkaari.  
Kun määritetty käyttäjä yrittää kirjautua SCC elinkaari, SCC elinkaari-tilin luodaan automaattisesti tarvittaessa.

>[AZURE.NOTE]Voit käyttää mitä tahansa muita SCC elinkaari käyttäjän tilin luontityökalut tai ohjelmointirajapinnan myöntämä SCC elinkaari säännöstä AAD käyttäjätileille.

##<a name="assigning-users"></a>Käyttäjien määrittäminen
  
Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-scc-lifecycle-perform-the-following-steps"></a>Jos haluat määrittää käyttäjiä SCC elinkaari, toimi seuraavasti:

1.  Testaa tilin luominen Azure perinteinen portaalissa.

2.  Valitse integration **SCC elinkaari **-sovellus-sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-scc-lifecycle-tutorial/IC794126.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Kyllä] (./media/active-directory-saas-scc-lifecycle-tutorial/IC767830.png "Kyllä")
  
Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli. Katso lisätietoja Access-paneelin [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).