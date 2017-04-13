<properties 
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi TalentLMS | Microsoft Azure" 
    description="Opettele käyttämään TalentLMS Azure Active Directory-hakemistosta käyttöön kertakirjautumisen, automaattinen valmistelu ja lisää!" 
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

#<a name="tutorial-azure-active-directory-integration-with-talentlms"></a>Opetusohjelma: TalentLMS Azure Active Directory-integrointi
  
Tässä opetusohjelmassa tavoitteena on näyttämään Azure ja TalentLMS integrointi.  
Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   TalentLMS palvelutili
  
Kun olet suorittanut Tässä opetusohjelmassa, olet liittänyt TalentLMS Azure AD-käyttäjien saa oikeuden kertakirjauksen TalentLMS yrityksesi sivuston (palveluntarjoajan aloittanut allekirjoitus-) tai käyttämällä [Access-paneelin esittely](active-directory-saas-access-panel-introduction.md)-sovellukseen.
  
Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1.  TalentLMS sovelluksen integrointi ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Määrittäminen käyttäjän valmistelu
4.  Käyttäjien määrittäminen

![Skenaario] (./media/active-directory-saas-talentlms-tutorial/IC777289.png "Skenaario")

##<a name="enabling-the-application-integration-for-talentlms"></a>TalentLMS sovelluksen integrointi ottaminen käyttöön
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön TalentLMS sovelluksen integrointi.

###<a name="to-enable-the-application-integration-for-talentlms-perform-the-following-steps"></a>TalentLMS sovelluksen integrointi käyttöön toimimalla seuraavasti:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-talentlms-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-talentlms-tutorial/IC700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-talentlms-tutorial/IC749321.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-talentlms-tutorial/IC749322.png "Lisää sovellus-gallerry")

6.  Kirjoita **hakuruutuun** **TalentLMS**.

    ![Sovelluksen valikoima] (./media/active-directory-saas-talentlms-tutorial/IC777290.png "Sovelluksen valikoima")

7.  Valitse tulosruudussa **TalentLMS**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![TalentLMS] (./media/active-directory-saas-talentlms-tutorial/IC777291.png "TalentLMS")

##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjien todentamiseen TalentLMS käyttämällä SAML-protokollan perusteella federation Azure AD-tilin kanssa. .  
Kertakirjautumisen varten TalentLMS määrittäminen edellyttää, että allekirjoitus arvon noutaa varmenne.  
Jos et ole tottunut näin, katso, [miten voit hakea varmenteen allekirjoitus-arvon](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Valitse Azure perinteinen portaalissa integrointi **TalentLMS** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-talentlms-tutorial/IC777292.png "Määritä kertakirjautuminen")

2.  **Miten käyttäjät voivat kirjautua TalentLMS haluat käyttää** sivulla Valitse **Microsoft Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-talentlms-tutorial/IC777293.png "Määritä kertakirjautuminen")

3.  Kirjoita **Määritä sovelluksen URL-osoite** -sivulla **TalentLMS merkki-URL** -tekstiruutuun URL-osoite käyttämällä seuraavaa kaavaa "*https://\<vuokraajan nimi\>. TalentLMSapp.com*", ja valitse sitten **Seuraava**.

    ![Kirjaudu URL-osoite] (./media/active-directory-saas-talentlms-tutorial/IC777294.png "Kirjaudu URL-osoite")

4.  Valitse **Määritä kertakirjautumisen osoitteessa TalentLMS** -sivulla Lataa sertifikaatti, **Lataa**ja Tallenna sertifikaattitiedosto paikallisesti kuin **c:\\TalentLMS.cer**.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-talentlms-tutorial/IC777295.png "Kertakirjautumisen määrittäminen")

5.  Eri selainikkunassa Kirjaudu TalentLMS yrityksesi sivuston järjestelmänvalvojan oikeuksilla.

6.  Valitse **tili ja asetukset** -osassa **käyttäjät** -välilehti.

    ![Tili ja asetukset] (./media/active-directory-saas-talentlms-tutorial/IC777296.png "Tili ja asetukset")

7.  **Kertakirjautuminen (SSO)**,

8.  Kertakirjautumisen-osassa seuraavasti:

    ![Kertakirjautuminen] (./media/active-directory-saas-talentlms-tutorial/IC777297.png "Kertakirjautuminen")

    1.  Valitse **SAML 2.0** **Kertakirjautumista integroinnin tyyppi** -luettelosta.
    2.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa TalentLMS** -valintasivun **Tunnistetietojen Toimittajan tunnus** -arvon kopioi ja liitä se sitten **tunnistetietojen toimittaja (IdP)** -tekstiruutuun.
    3.  Viedyn varmenteen **allekirjoitusta** kopioi ja liitä se **Varmenteen tunnisteen** tekstiruutu.

        >[AZURE.TIP] Lisätietoja on artikkelissa [noutamisesta varmenteen allekirjoitusarvo](http://youtu.be/YKQF266SAxI)

    4.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa TalentLMS** -valintasivun **Remote sisäänkirjautumisen URL-osoite** -arvo kopioi ja liitä se sitten **Remote - kirjautuminen URL** -tekstiruutuun.
    5.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa TalentLMS** -valintasivun **Remote Kirjaudu URL-osoite** -arvo kopioi ja liitä se sitten **Remote Kirjaudu ulos URL** -tekstiruutuun.
    6.  Kirjoita **TargetedID** -tekstiruutuun **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name**
    7.  Kirjoita **Ensimmäinen nimi** -tekstiruutuun **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**
    8.  Kirjoita **Sukunimi** -tekstiruutuun **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**
    9.  Kirjoita **sähköpostiosoite** -tekstiruutuun **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**
    10. Valitse **Tallenna**.

9.  Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Valmis** **Määrittäminen kertakirjautuminen** -valintaikkunassa Sulje.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-talentlms-tutorial/IC777298.png "Kertakirjautumisen määrittäminen")

##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu
  
Jos haluat ottaa käyttöön Azure AD-käyttäjät voivat kirjautua sisään TalentLMS, ne on valmisteltava TalentLMS kyselyjä.  
TalentLMS valmistelu on manuaalinen tehtävä.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Valmistelu käyttäjätilit, toimi seuraavasti:

1.  Kirjaudu sisään **TalentLMS** alihallintaan.

2.  Valitse **käyttäjät**ja valitse sitten **Lisää käyttäjä**.

3.  Valitse **Lisää käyttäjä** -sivulla toimimalla seuraavasti:

    ![Käyttäjän lisääminen] (./media/active-directory-saas-talentlms-tutorial/IC777299.png "Käyttäjän lisääminen")

    1.  Kirjoita Azure AD-käyttäjätilin liittyvän määritteen arvot seuraavat tekstiruutujen: **Etunimi**, **Sukunimi**, **sähköpostiosoite**.
    2.  Valitse **Lisää käyttäjä**.

>[AZURE.NOTE] Voit käyttää mitä tahansa muita TalentLMS käyttäjän tilin luontityökalut tai ohjelmointirajapinnan myöntämä TalentLMS säännöstä AAD käyttäjätileille.

##<a name="assigning-users"></a>Käyttäjien määrittäminen
  
Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-talentlms-perform-the-following-steps"></a>Jos haluat määrittää käyttäjiä TalentLMS, toimi seuraavasti:

1.  Testaa tilin luominen Azure perinteinen portaalissa.

2.  Valitse integration **TalentLMS **sovellus-sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-talentlms-tutorial/IC777300.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Kyllä] (./media/active-directory-saas-talentlms-tutorial/IC767830.png "Kyllä")
  
Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli. Katso lisätietoja Access-paneelin [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).