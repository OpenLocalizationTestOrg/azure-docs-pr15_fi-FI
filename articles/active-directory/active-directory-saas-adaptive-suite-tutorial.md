<properties 
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi mukautuvat Suite | Microsoft Azure"
    description="Opettele käyttämään mukautuvat Suite Azure Active Directory-hakemistosta käyttöön kertakirjautumisen, automaattinen valmistelu ja lisää!" 
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

#<a name="tutorial-azure-active-directory-integration-with-adaptive-suite"></a>Opetusohjelma: Mukautuvat Suite Azure Active Directory-integrointi

Tässä opetusohjelmassa tavoitteena on näyttämään Azure ja mukautuvat Suite integrointi.  
Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   Mukautuvat Suite vuokraajan

Kun olet suorittanut Tässä opetusohjelmassa, olet määrittänyt mukautuvat ohjelmistopakettiin Azure AD-käyttäjien saa oikeuden kertakirjauksen mukautuvat Suite yrityksesi sivuston (palveluntarjoajan aloittanut allekirjoitus-) tai käyttämällä [Access-paneelin esittely](active-directory-saas-access-panel-introduction.md)-sovellukseen.

Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1.  Sovelluksen integrointi mukautuvat Suite ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Määrittäminen käyttäjän valmistelu
4.  Käyttäjien määrittäminen

![Skenaario] (./media/active-directory-saas-adaptive-suite-tutorial/IC805637.png "Skenaario")
##<a name="enabling-the-application-integration-for-adaptive-suite"></a>Sovelluksen integrointi mukautuvat Suite ottaminen käyttöön

Tässä osassa tavoitteena on Jäsennys, miten voit ottaa käyttöön sovelluksen integrointi mukautuvat Suite.

###<a name="to-enable-the-application-integration-for-adaptive-suite-perform-the-following-steps"></a>Ottaa käyttöön sovelluksen integroinnin mukautuvat Suite, toimi seuraavasti:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-adaptive-suite-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-adaptive-suite-tutorial/IC700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-adaptive-suite-tutorial/IC749321.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-adaptive-suite-tutorial/IC749322.png "Lisää sovellus-gallerry")

6.  Kirjoita **hakuruutuun** **Mukautuvat Suite**.

    ![Sovelluksen valikoima] (./media/active-directory-saas-adaptive-suite-tutorial/IC805638.png "Sovelluksen valikoima")

7.  Valitse tulosruudussa **Mukautuvat Suite**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![Mukautuvat Suite] (./media/active-directory-saas-adaptive-suite-tutorial/IC805639.png "Mukautuvat Suite")
##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen

Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjien todentamiseen mukautuvat ohjelmistopakettiin niiden tilillä, käyttämällä SAML-protokollan perusteella federation Azure AD.  
Kertakirjautumisen mukautuvat Suite määrittäminen edellyttää, että allekirjoitus arvon noutaa varmenne.  
Jos et ole tottunut näin, katso, [miten voit hakea varmenteen allekirjoitus-arvon](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Valitse Azure perinteinen portaalissa integrointi **Mukautuvat Suite** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-adaptive-suite-tutorial/IC805640.png "Kertakirjautumisen määrittäminen")

2.  **Miten haluat kirjautua mukautuvat ohjelmistopakettiin käyttäjät** -sivulla **Microsoft Azure AD kertakirjautumisen**Valitse ja valitse sitten **Seuraava**.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-adaptive-suite-tutorial/IC805641.png "Kertakirjautumisen määrittäminen")

3.  Kirjoita **Määritä sovelluksen asetukset** -sivun **Vastaa URL** -tekstiruutuun URL-osoite käyttämällä seuraavaa kaavaa "*https://login.adaptiveinsights.com:443/samlsso/RlJFRVRSSUFMMTI3MTE =*", ja valitse sitten **Seuraava**.

    >[AZURE.NOTE] Saat tämän arvon mukautuvat Suite **SAML SSO asetukset** -sivulla.

    ![Sovelluksen asetusten määrittäminen] (./media/active-directory-saas-adaptive-suite-tutorial/IC805642.png "Sovelluksen asetusten määrittäminen")

4.  **Määritä kertakirjautumisen mukautuvat Suite osoitteessa** -sivulla Lataa sertifikaatti, valitse **Lataa**, ja Tallenna sertifikaattitiedosto paikallisesti tietokoneeseen.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-adaptive-suite-tutorial/IC805643.png "Kertakirjautumisen määrittäminen")

5.  Eri selainikkunassa Kirjaudu mukautuvat Suite yrityksesi sivuston järjestelmänvalvojan oikeuksilla.

6.  Valitse **järjestelmänvalvoja**.

    ![Järjestelmänvalvoja] (./media/active-directory-saas-adaptive-suite-tutorial/IC805644.png "Järjestelmänvalvoja")

7.  Valitse **käyttäjien ja roolien** -osassa **SAML SSO-asetusten hallinta**.

    ![SAML SSO-asetusten hallinta] (./media/active-directory-saas-adaptive-suite-tutorial/IC805645.png "SAML SSO-asetusten hallinta")

8.  Suorita **SAML SSO asetukset** -sivun seuraavasti:

    ![SAML SSO-asetukset] (./media/active-directory-saas-adaptive-suite-tutorial/IC805646.png "SAML SSO-asetukset")

    1.  Kirjoita nimi kokoonpanon **tunnistetietojen tarjoajan nimi** -tekstiruutuun.
    2.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa mukautuvat Suite** -valintasivun **Kohteen tunnus** -arvon kopioi ja liitä se sitten **tunnistetietojen toimittaja kohteen tunnus** -tekstiruutuun.
    3.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa mukautuvat Suite** -valintasivun **SAML SSO URL-osoite** -arvo kopioi ja liitä se sitten **tunnistetietojen toimittaja SSO URL** -tekstiruutuun.
    4.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa mukautuvat Suite** -valintasivun **SAML SSO URL-osoite** -arvo kopioi ja liitä se sitten **Mukautettu Kirjaudu URL** -tekstiruutuun.
    5.  Voit ladata ladatut varmenteen, valitsemalla **Valitse tiedosto**.
    6.  **SAML-käyttäjätunnuksella**Valitse **käyttäjän mukautuvat havainnollistamisen käyttäjänimi**.
    7.  **SAML tunnus sijaintiin**Valitse **Aihe NameID käyttäjätunnus**.
    8.  **SAML NameID muodossa**Valitse **sähköpostiosoite**.
    9.  Valitse **Ota käyttöön SAML** **Salli SAML SSO ja suorat mukautuvat havainnollistamisen Kirjaudu sisään**.
    10. Valitse **Tallenna**.

9.  Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Valmis** **Määrittäminen kertakirjautuminen** -valintaikkunassa Sulje.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-adaptive-suite-tutorial/IC805647.png "Kertakirjautumisen määrittäminen")
##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu

Jotta voit kirjautua mukautuvat Suite Azure AD-käyttäjien käyttöön, ne on valmisteltava mukautuvat Suite kyselyjä.  
Säätö-ohjelmistopaketti valmistelu on manuaalinen tehtävä.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Voit määrittää käyttäjän valmistelu toimimalla seuraavasti:

1.  Kirjaudu sisään **Mukautuvat Suite** yrityksesi sivuston järjestelmänvalvojan oikeuksilla.

2.  Valitse **järjestelmänvalvoja**.

    ![Järjestelmänvalvoja] (./media/active-directory-saas-adaptive-suite-tutorial/IC805644.png "Järjestelmänvalvoja")

3.  Valitse **käyttäjät ja roolit** -kohdassa **Lisää käyttäjä**.

    ![Käyttäjän lisääminen] (./media/active-directory-saas-adaptive-suite-tutorial/IC805648.png "Käyttäjän lisääminen")

4.  Valitse **Uusi käyttäjä** -osassa seuraavasti:

    ![Lähetä] (./media/active-directory-saas-adaptive-suite-tutorial/IC805649.png "Lähetä")

    1.  Kirjoita **nimi**, **Kirjaudu sisään**, **sähköpostin**, haluat säännöstä kyselyjä liittyvät tekstiruutujen kelvollinen Azure Active Directory-käyttäjän **salasanan** .
    2.  Valitse **rooli**.
    3.  Valitse **Lähetä**.

>[AZURE.NOTE] Voit käyttää mitä tahansa muita mukautuvat Suite käyttäjän tilin luontityökalut tai ohjelmointirajapinnan myöntämä mukautuvat Suite säännöstä AAD käyttäjätileille.

##<a name="assigning-users"></a>Käyttäjien määrittäminen

Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-adaptive-suite-perform-the-following-steps"></a>Jos haluat määrittää käyttäjiä mukautuvat Suite, toimi seuraavasti:

1.  Testaa tilin luominen Azure perinteinen portaalissa.

2.  Valitse **Mukautuvat Suite **sovelluksen integrointi-sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-adaptive-suite-tutorial/IC805650.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Kyllä] (./media/active-directory-saas-adaptive-suite-tutorial/IC767830.png "Kyllä")

Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli. Katso lisätietoja Access-paneelin [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).
