<properties 
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi Syncplicity | Microsoft Azure" 
    description="Opettele käyttämään Syncplicity Azure Active Directory-hakemistosta käyttöön kertakirjautumisen, automaattinen valmistelu ja lisää!" 
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

#<a name="tutorial-azure-active-directory-integration-with-syncplicity"></a>Opetusohjelma: Syncplicity Azure Active Directory-integrointi
  
Tässä opetusohjelmassa tavoitteena on näyttää, miten voit määrittää kertakirjautumisen Azure Active Directory (Azure AD) ja Syncplicity välillä.
  
Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   Syncplicity palvelutili
  
Kun olet suorittanut tämän opetusohjelman, Azure AD-käyttäjät, joille olet määrittää Accessin Syncplicity voivat yksittäinen merkki sovellukseen Syncplicity yrityksesi sivuston (palveluntarjoajan aloittanut allekirjoitus-) tai käyttämällä Azure AD Access-paneeli.

1.  Syncplicity sovelluksen integrointi ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Määrittäminen käyttäjän valmistelu
4.  Käyttäjien määrittäminen

![Skenaario] (./media/active-directory-saas-syncplicity-tutorial/IC769524.png "Skenaario")

##<a name="enabling-the-application-integration-for-syncplicity"></a>Syncplicity sovelluksen integrointi ottaminen käyttöön
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön Syncplicity sovelluksen integrointi.

###<a name="to-enable-the-application-integration-for-syncplicity-perform-the-following-steps"></a>Syncplicity sovelluksen integrointi käyttöön toimimalla seuraavasti:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-syncplicity-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-syncplicity-tutorial/IC700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-syncplicity-tutorial/IC749321.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-syncplicity-tutorial/IC749322.png "Lisää sovellus-gallerry")

6.  Kirjoita **hakuruutuun** **Syncplicity**.

    ![Syncplicity sovelluksen valikoima] (./media/active-directory-saas-syncplicity-tutorial/IC769532.png "Syncplicity sovelluksen valikoima")

7.  Valitse tulosruudussa **Syncplicity**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![Syncplicity] (./media/active-directory-saas-syncplicity-tutorial/IC769533.png "Syncplicity")

##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen
  
Tässä osassa kuvataan antaa käyttäjien todentamiseen Syncplicity niiden tilillä Azure Active Directory federation perusteella SAML-protokollan avulla.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Valitse Azure perinteinen portaalissa integrointi **Syncplicity** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-syncplicity-tutorial/IC769534.png "Määritä kertakirjautuminen")

2.  **Miten käyttäjät voivat kirjautua Syncplicity haluat käyttää** sivulla Valitse **Microsoft Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Microsoft Azure AD-Single Sign-On] (./media/active-directory-saas-syncplicity-tutorial/IC769535.png "Microsoft Azure AD-Single Sign-On")

3.  **Määrittää sovelluksen URL-osoite** -sivulla **Syncplicity merkki-URL** -tekstiruutuun URL-käyttäjät käyttävät Syncplicity sovelluksen kirjautumalla tyyppi valitsemalla **Seuraava**. 

    Sovelluksen URL-osoite on Syncplicity vuokraajan URL-osoite (esimerkiksi: *http://company.Syncplicity.com*):

    ![Sovelluksen URL-Osoitteen määrittäminen] (./media/active-directory-saas-syncplicity-tutorial/IC769536.png "Sovelluksen URL-Osoitteen määrittäminen")

4.  Valitse **Määritä kertakirjautumisen osoitteessa Syncplicity** -sivulla Lataa sertifikaatti, **Lataa**ja Tallenna sertifikaattitiedosto paikallisesti tietokoneeseen.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-syncplicity-tutorial/IC769543.png "Määritä kertakirjautuminen")

5.  Kirjaudu **Syncplicity** alihallintaan.

6.  Valitse ylä-valikossa **järjestelmänvalvoja**, valitse **asetukset**ja valitse sitten **Mukautettu toimialue ja kertakirjautuminen**.

    ![Syncplicity] (./media/active-directory-saas-syncplicity-tutorial/IC769545.png "Syncplicity")

7.  Suorita **Single Sign-On (SSO)** -sivulla seuraavat toimet:

    ![Sign-On yksi \(SSO\)](./media/active-directory-saas-syncplicity-tutorial/IC769550.png "Single Sign-On \(SSO\)")

    1.  Kirjoita **Mukautettu toimialue** -tekstiruutuun toimialueen nimi.
    2.  Valitse **Sign-tila** **käytössä** .
    3.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa Syncplicity** -sivulla **Kohteen tunnus** -arvon kopioi ja liitä se sitten **Kohteen tunnus** -tekstiruutuun.
    4.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa Syncplicity** -sivulla **Sign-palvelun URL** -arvon kopioi ja liitä se sitten **Kirjaudu sisään sivun URL** -tekstiruutuun.
    5.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa Syncplicity** -sivulla **Remote Kirjaudu URL-osoite** -arvo kopioi ja liitä se sitten **Kirjaudu ulos sivun URL** -tekstiruutuun.
    6.  **Tunnistetietojen toimittaja varmenne** **Valitse tiedosto**ja lataa varmenteen olet ladannut perinteinen Azure-portaalista.
    7.  Valitse **Tallenna muutokset**.

8.  Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Valmis** **Määrittäminen kertakirjautuminen** -valintaikkunassa Sulje.

    ![Vahvistus] (./media/active-directory-saas-syncplicity-tutorial/IC769554.png "Vahvistus")

##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu
  
AAD käyttäjät voivat kirjautua ne on valmisteltava Syncplicity-sovellukseen. Tässä osassa kuvataan, miten AAD Käyttäjätilien luominen Syncplicity.

###<a name="to-provision-a-user-account-to-syncplicity-perform-the-following-steps"></a>Valmistelu Syncplicity käyttäjätili, toimi seuraavasti:

1.  Kirjaudu sisään **Syncplicity** asiakasympäristöön (esimerkiksi: *https://company.Syncplicity.com*).

2.  Valitse **järjestelmänvalvoja** ja valitse **Käyttäjätilit**.

3.  Valitse **Lisää käyttäjä**.

    ![Käyttäjien hallinta] (./media/active-directory-saas-syncplicity-tutorial/IC769764.png "Käyttäjien hallinta")

4.  Kirjoita **sähköpostiosoite** AAD tilin valmistelu, valitse **käyttäjä** **rooli**ja valitse sitten **Seuraava**.

    ![Tilitiedot] (./media/active-directory-saas-syncplicity-tutorial/IC769765.png "Tilitiedot")

    >[AZURE.NOTE] AAD tilin omistaja saavat sähköpostiviestin, mukaan lukien linkin Vahvista ja aktivoi tili.

5.  Valitse ryhmä yrityksessäsi, että uusi käyttäjä olisi jäseneksi, ja valitse sitten **Seuraava**.

    ![Ryhmäjäsenyys] (./media/active-directory-saas-syncplicity-tutorial/IC769772.png "Ryhmäjäsenyys")

    >[AZURE.NOTE] Jos ei ole lueteltu ryhmiä, valitse **Seuraava**.

6.  Valitse kansiot, jotka haluat sijoittaa käyttäjän tietokoneen Syncplicity's valvonnassa ja valitse sitten **Seuraava**.

    ![Syncplicity kansiot] (./media/active-directory-saas-syncplicity-tutorial/IC769773.png "Syncplicity kansiot")

>[AZURE.NOTE] Voit käyttää mitä tahansa muita Syncplicity käyttäjän tilin luontityökalut tai ohjelmointirajapinnan myöntämä Syncplicity säännöstä AAD käyttäjätileille.

##<a name="assigning-users"></a>Käyttäjien määrittäminen
  
Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-syncplicity-perform-the-following-steps"></a>Jos haluat määrittää käyttäjiä Syncplicity, toimi seuraavasti:

1.  Testaa tilin luominen Azure perinteinen portaalissa.

2.  Valitse integration **Syncplicity** sovellus-sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-syncplicity-tutorial/IC769557.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Kyllä] (./media/active-directory-saas-syncplicity-tutorial/IC767830.png "Kyllä")
  
Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli. Katso lisätietoja Access-paneelin [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).

