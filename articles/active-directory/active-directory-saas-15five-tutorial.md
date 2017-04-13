<properties 
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi 15Five | Microsoft Azure" 
    description="Opettele käyttämään 15Five Azure Active Directory-hakemistosta käyttöön kertakirjautumisen, automaattinen valmistelu ja lisää!" 
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

#<a name="tutorial-azure-active-directory-integration-with-15five"></a>Opetusohjelma: 15Five Azure Active Directory-integrointi

Tässä opetusohjelmassa tavoitteena on näyttämään Azure ja 15Five integrointi. Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   Käytössä 15Five kertakirjautumisen tilaus

Kun olet suorittanut Tässä opetusohjelmassa, olet liittänyt 15Five Azure AD-käyttäjien saa oikeuden kertakirjauksen 15Five yrityksesi sivuston (palveluntarjoajan aloittanut allekirjoitus-) tai käyttämällä [Access-paneelin esittely](active-directory-saas-access-panel-introduction.md)-sovellukseen.

Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1.  15Five sovelluksen integrointi ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Määrittäminen käyttäjän valmistelu
4.  Käyttäjien määrittäminen

![Skenaario] (./media/active-directory-saas-15five-tutorial/IC784667.png "Skenaario")
##<a name="enabling-the-application-integration-for-15five"></a>15Five sovelluksen integrointi ottaminen käyttöön

Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön 15Five sovelluksen integrointi.

###<a name="to-enable-the-application-integration-for-15five-perform-the-following-steps"></a>15Five sovelluksen integrointi käyttöön toimimalla seuraavasti:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-15five-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-15five-tutorial/IC700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-15five-tutorial/IC749321.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-15five-tutorial/IC749322.png "Lisää sovellus-gallerry")

6.  Kirjoita **hakuruutuun** **15Five**.

    ![Sovelluksen valikoima] (./media/active-directory-saas-15five-tutorial/IC784668.png "Sovelluksen valikoima")

7.  Valitse tulosruudussa **15Five**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![15Five] (./media/active-directory-saas-15five-tutorial/IC784669.png "15Five")
##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen

Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjien todentamiseen 15Five niiden tilillä Azure AD käyttämällä SAML-protokollan perusteella federation.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Valitse Azure perinteinen portaalissa integrointi **15Five** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-15five-tutorial/IC784670.png "Määritä kertakirjautuminen")

2.  **Miten käyttäjät voivat kirjautua 15Five haluat käyttää** sivulla Valitse **Microsoft Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-15five-tutorial/IC784671.png "Määritä kertakirjautuminen")

3.  **Määrittää sovelluksen URL-osoite** -sivulla **15Five merkki-URL** -tekstiruutuun Kirjoita käyttämällä seuraavaa kaavaa "*https://company.15Five.com*" URL-osoite ja valitse sitten **Seuraava**.

    ![Sovelluksen URL-Osoitteen määrittäminen] (./media/active-directory-saas-15five-tutorial/IC784672.png "Sovelluksen URL-Osoitteen määrittäminen")

4.  Valitse **Määritä kertakirjautumisen osoitteessa 15Five** -sivulla **Lataa metatiedot**ja eteenpäin 15Five tukihenkilöstön metatietojen tiedoston.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-15five-tutorial/IC784673.png "Määritä kertakirjautuminen")

    >[AZURE.NOTE] Kertakirjautumisen täytyy ottaa käyttöön 15Five-tukeen.

5.  Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Valmis** **Määrittäminen kertakirjautuminen** -valintaikkunassa Sulje.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-15five-tutorial/IC784674.png "Määritä kertakirjautuminen")
##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu

Jos haluat ottaa käyttöön Azure AD-käyttäjät voivat kirjautua sisään 15Five, ne on valmisteltava 15Five kyselyjä.  
15Five valmistelu on manuaalinen tehtävä.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Voit määrittää käyttäjän valmistelu toimimalla seuraavasti:

1.  Kirjaudu sisään **15Five** yrityksesi sivuston järjestelmänvalvojana.

2.  Siirry **hallita yrityksen**.

    ![Yrityksen hallinta] (./media/active-directory-saas-15five-tutorial/IC784675.png "Yrityksen hallinta")

3.  Siirry **henkilöiden \> Lisää henkilöitä**.

    ![Henkilöt] (./media/active-directory-saas-15five-tutorial/IC784676.png "Henkilöt")

4.  Lisää uusi henkilö-osassa seuraavasti:

    ![Lisää uusi henkilö] (./media/active-directory-saas-15five-tutorial/IC784677.png "Lisää uusi henkilö")

    1.  Kirjoita **Etunimi**, **Sukunimi**, **otsikon**, kelvollinen Azure Active Directory-tilin haluat säännöstä kyselyjä liittyvät tekstiruutujen **sähköpostiosoite** .
    2.  Valitse **Valmis**.

    >[AZURE.NOTE] Azure AD-tilin omistaja saavat sähköpostiviestin, mukaan lukien linkin Vahvista tili, ennen kuin se on aktiivinen.

>[AZURE.NOTE] Voit käyttää mitä tahansa muita 15Five käyttäjän tilin luontityökalut tai ohjelmointirajapinnan myöntämä 15Five säännöstä AAD käyttäjätileille.

##<a name="assigning-users"></a>Käyttäjien määrittäminen

Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-15five-perform-the-following-steps"></a>Jos haluat määrittää käyttäjiä 15Five, toimi seuraavasti:

1.  Testaa tilin luominen Azure perinteinen portaalissa.

2.  Valitse integration **15Five **sovellus-sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-15five-tutorial/IC784678.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Kyllä] (./media/active-directory-saas-15five-tutorial/IC767830.png "Kyllä")

Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli. Katso lisätietoja Access-paneelin [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).
