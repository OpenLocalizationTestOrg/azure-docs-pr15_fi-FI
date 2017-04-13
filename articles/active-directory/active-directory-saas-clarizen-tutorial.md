<properties 
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi Clarizen | Microsoft Azure" 
    description="Opettele käyttämään Clarizen Azure Active Directory-hakemistosta käyttöön kertakirjautumisen, automaattinen valmistelu ja lisää!" 
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

#<a name="tutorial-azure-active-directory-integration-with-clarizen"></a>Opetusohjelma: Clarizen Azure Active Directory-integrointi

Tässä opetusohjelmassa tavoitteena on näyttämään Azure ja Clarizen integrointi.  
Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   Käytössä Clarizen kertakirjautumisen tilaus

Kun olet suorittanut Tässä opetusohjelmassa, olet liittänyt Clarizen Azure AD-käyttäjien saa oikeuden kertakirjauksen Clarizen yrityksesi sivuston (palveluntarjoajan aloittanut allekirjoitus-) tai käyttämällä [Access-paneelin esittely](active-directory-saas-access-panel-introduction.md)-sovellukseen.

Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1.  Clarizen sovelluksen integrointi ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Määrittäminen käyttäjän valmistelu
4.  Käyttäjien määrittäminen

![Skenaario] (./media/active-directory-saas-clarizen-tutorial/IC784679.png "Skenaario")
##<a name="enabling-the-application-integration-for-clarizen"></a>Clarizen sovelluksen integrointi ottaminen käyttöön

Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön Clarizen sovelluksen integrointi.

###<a name="to-enable-the-application-integration-for-clarizen-perform-the-following-steps"></a>Clarizen sovelluksen integrointi käyttöön toimimalla seuraavasti:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-clarizen-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-clarizen-tutorial/IC700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-clarizen-tutorial/IC749321.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-clarizen-tutorial/IC749322.png "Lisää sovellus-gallerry")

6.  Kirjoita **hakuruutuun** **Clarizen**.

    ![Sovelluksen valikoima] (./media/active-directory-saas-clarizen-tutorial/IC784680.png "Sovelluksen valikoima")

7.  Valitse tulosruudussa **Clarizen**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![Clarizen] (./media/active-directory-saas-clarizen-tutorial/IC784681.png "Clarizen")
##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen

Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjien todentamiseen Clarizen käyttämällä SAML-protokollan perusteella federation Azure AD-tilin kanssa.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Valitse Azure perinteinen portaalissa integrointi **Clarizen** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-clarizen-tutorial/IC784682.png "Kertakirjautumisen määrittäminen")

2.  **Miten käyttäjät voivat kirjautua Clarizen haluat käyttää** sivulla Valitse **Microsoft Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-clarizen-tutorial/IC784683.png "Kertakirjautumisen määrittäminen")

3.  Valitse **Määritä kertakirjautumisen osoitteessa Clarizen** -sivulla Lataa sertifikaatti, valitsemalla **Lataa**, ja Tallenna sertifikaattitiedosto tietokoneeseen.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-clarizen-tutorial/IC784684.png "Kertakirjautumisen määrittäminen")

4.  Eri selainikkunassa Kirjaudu sisään järjestelmänvalvojana **Clarizen** yrityksesi-sivuston (esimerkiksi: *https://app2.clarizen.com/Clarizen/Pages/Service/Login.aspx*).

5.  Valitse käyttäjänimi ja valitse sitten **asetukset**.

    ![Asetukset] (./media/active-directory-saas-clarizen-tutorial/IC784685.png "Asetukset")

6.  Valitse **Yleiset asetukset** -välilehti ja valitse **Liitetty todennusta**-kohdan vieressä **Muokkaa**.

    ![Yleiset asetukset] (./media/active-directory-saas-clarizen-tutorial/IC786906.png "Yleiset asetukset")

7.  Valitse **Liitetty todennus** -valintaikkunassa seuraavasti:

    ![Liitetty todennus] (./media/active-directory-saas-clarizen-tutorial/IC785892.png "Liitetty todennus")

    1.  Valitse **Lataa** lataaminen ladatut varmennetta.
    2.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa Clarizen** -valintasivun **Sign-palvelun URL** -arvon kopioi ja liitä se sitten **Kirjaudu sisään URL** -tekstiruutuun.
    3.  Azure-perinteinen portaalissa, valitse **Määritä yhden Clarizen, kirjaudu ulos** -sivulla kopioi **Yhden Sign-Out palvelun URL-osoite** -arvo ja liitä se **Sign-out URL** -tekstiruutuun.
    4.  Valitse **Käytä VIESTIIN**.
    5.  Valitse **Tallenna**.

8.  Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Valmis** **Määrittäminen kertakirjautuminen** -valintaikkunassa Sulje.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-clarizen-tutorial/IC784688.png "Kertakirjautumisen määrittäminen")
##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu

Jos haluat ottaa käyttöön Azure AD-käyttäjät voivat kirjautua sisään Clarizen, ne on valmisteltava Clarizen kyselyjä.  
Clarizen valmistelu on manuaalinen tehtävä.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Valmistelu käyttäjätilit, toimi seuraavasti:

1.  Kirjaudu sisään **Clarizen** yrityksesi sivuston järjestelmänvalvojan oikeuksilla.

2.  Valitse **henkilöt**.

    ![Henkilöt] (./media/active-directory-saas-clarizen-tutorial/IC784689.png "Henkilöt")

3.  Valitse **Kutsu käyttäjä**.

    ![Käyttäjien kutsuminen] (./media/active-directory-saas-clarizen-tutorial/IC784690.png "Käyttäjien kutsuminen")

4.  Valitse kutsu henkilöitä-sivulla toimimalla seuraavasti:

    ![Kutsu käyttäjiä] (./media/active-directory-saas-clarizen-tutorial/IC784691.png "Kutsu käyttäjiä")

    1.  Kirjoita sähköpostiosoite, jos haluat säätää kelvollinen Azure Active Directory-tilin **sähköpostiosoite** -tekstiruutuun.
    2.  Valitse **Kutsu**.

    >[AZURE.NOTE] Azure Active Directory-tilin omistaja vastaanottaa sähköpostia ja johtavan linkin Vahvista tilin, ennen kuin se on aktiivinen.

##<a name="assigning-users"></a>Käyttäjien määrittäminen

Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-clarizen-perform-the-following-steps"></a>Jos haluat määrittää käyttäjiä Clarizen, toimi seuraavasti:

1.  Testaa tilin luominen Azure perinteinen portaalissa.

2.  Valitse integration **Clarizen **sovellus-sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-clarizen-tutorial/IC784692.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Kyllä] (./media/active-directory-saas-clarizen-tutorial/IC767830.png "Kyllä")

Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli. Katso lisätietoja Access-paneelin [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).
