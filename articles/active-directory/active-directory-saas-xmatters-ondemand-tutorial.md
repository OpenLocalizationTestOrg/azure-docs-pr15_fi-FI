<properties 
    pageTitle="Opetusohjelma: Azure Active Directory integrointi xMatters OnDemand | Microsoft Azure"
    description="Opettele käyttämään xMatters OnDemand Azure Active Directory-hakemistosta käyttöön kertakirjautumisen, automaattinen valmistelu ja lisää!" 
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
    ms.date="09/09/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-xmatters-ondemand"></a>Opetusohjelma: Azure Active Directory-integrointi xMatters OnDemand kanssa
  
Tässä opetusohjelmassa tavoitteena on näyttämään Azure ja xMatters OnDemand integrointi. Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   XMatters OnDemand palvelutili
  
Kun olet suorittanut Tässä opetusohjelmassa, olet liittänyt xMatters OnDemand Azure AD-käyttäjien saa oikeuden kertakirjauksen xMatters OnDemand yrityksesi sivuston (palveluntarjoajan aloittanut allekirjoitus-) tai käyttämällä [Access-paneelin esittely](active-directory-saas-access-panel-introduction.md)-sovellukseen.
  
Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1.  XMatters OnDemand sovelluksen integrointi ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Määrittäminen käyttäjän valmistelu
4.  Käyttäjien määrittäminen

![Skenaario] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776788.png "Skenaario")

##<a name="enabling-the-application-integration-for-xmatters-ondemand"></a>XMatters OnDemand sovelluksen integrointi ottaminen käyttöön
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön xMatters OnDemand sovelluksen integrointi.

###<a name="to-enable-the-application-integration-for-xmatters-ondemand-perform-the-following-steps"></a>XMatters OnDemand sovelluksen integrointi käyttöön toimimalla seuraavasti:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC749321.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC749322.png "Lisää sovellus-gallerry")

6.  Kirjoita **hakuruutuun** **xMatters OnDemand**.

    ![Sovelluksen valikoima] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776789.png "Sovelluksen valikoima")

7.  Valitse tulosruudussa **XMatters OnDemand**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![xMatters OnDemand] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776790.png "xMatters OnDemand")

##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjien todentamiseen XMatters OnDemand käyttämällä SAML-protokollan perusteella federation Azure AD-tilin kanssa.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Valitse Azure perinteinen portaalissa integrointi **XMatters OnDemand** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776791.png "Määritä kertakirjautuminen")

2.  **Miten käyttäjät voivat kirjautua XMatters OnDemand haluat käyttää** sivulla Valitse **Microsoft Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776792.png "Määritä kertakirjautuminen")

3.  Tee **Määrittäminen sovelluksen URL-osoite** -sivulla seuraavat toimet:

    ![Sovelluksen URL-Osoitteen määrittäminen] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776793.png "Sovelluksen URL-Osoitteen määrittäminen")

    a. Kirjoita **XMatters OnDemand merkki-URL** -tekstiruutuun URL-osoite käyttämällä seuraavaa kaavaa:`https://<tenant-name>.XMattersOnDemandapp.com`

    b. Valitse **Seuraava**.


4.  Valitse **Määritä kertakirjautumisen osoitteessa XMatters OnDemand** -sivulla Lataa sertifikaatti, **Lataa**ja Tallenna sertifikaattitiedosto paikallisesti kuin **c:\\XMatters OnDemand.cer**.

    >[AZURE.IMPORTANT] Sinun täytyy edelleen sertifikaatin xMatters tukihenkilöstön avulla. Varmenne on ladattava xMatters tukihenkilöstön mukaan, ennen kuin viimeistelet Sign-määritys.

    ![Määritä yhden sisäänkirjautuminen] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776794.png "Määritä yhden sisäänkirjautuminen")

5.  Eri selainikkunassa Kirjaudu XMatters OnDemand yrityksesi sivuston järjestelmänvalvojan oikeuksilla.

6.  Työkalurivin yläkulmassa **järjestelmänvalvoja**ja valitse sitten **Yrityksen tiedot** vasemman reunan siirtymispalkissa.

    ![Järjestelmänvalvoja] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776795.png "Järjestelmänvalvoja")

7.  **SAML-määritys** -sivulla toimimalla seuraavasti:

    ![SAML-määritys] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776796.png "SAML-määritys")

    1.  Valitse **Ota käyttöön SAML**.
    2.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa XMatters OnDemand** -valintasivun **Tunnistetietojen Toimittajan tunnus** -arvon kopioi ja liitä se sitten **Tunnistetietojen Toimittajan tunnus** -tekstiruutuun.
    3.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa XMatters OnDemand** -valintasivun **Sign-palvelun URL** -arvon kopioi ja liitä ne **Yhteen merkki-URL** -tekstiruutuun.
    4.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa XMatters OnDemand** -valintasivun **Yksittäisen Sign-Out palvelun URL-osoite** -arvo kopioi ja liitä ne **Yhteen Kirjaudu URL** -tekstiruutuun.
    5.  Yrityksen tiedot-sivun yläreunassa Valitse **Tallenna muutokset**.
        ![Yrityksen tiedot] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776797.png "Yrityksen tiedot")

8.  Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Valmis** **Määrittäminen kertakirjautuminen** -valintaikkunassa Sulje.

    ![Määritä yhden sisäänkirjautuminen] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776798.png "Määritä yhden sisäänkirjautuminen")

##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu
  
Jos haluat ottaa käyttöön Azure AD-käyttäjät voivat kirjautua sisään XMatters OnDemand, ne on valmisteltava XMatters OnDemand kyselyjä.  
XMatters OnDemand valmistelu on manuaalinen tehtävä.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Valmistelu käyttäjätilit, toimi seuraavasti:

1.  Kirjaudu sisään **XMatters OnDemand** alihallintaan.

2.  Valitse **käyttäjät** -välilehti.

3.  Valitse **Lisää käyttäjä**.

    ![Käyttäjät] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC781048.png "Käyttäjät")

4.  Valitse **aktiivinen**.

5.  Valitse **Lisää käyttäjä** -osassa seuraavasti:

    ![Käyttäjän lisääminen] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC781049.png "Käyttäjän lisääminen")

    1.  Kirjoita **käyttäjätunnus**, **Etunimi**, **Sukunimi**, **sivuston** kelvollinen AAD tilin haluat säätää.
    2.  Valitse **Tallenna**.

>[AZURE.NOTE] Voit käyttää mitä tahansa muita XMatters OnDemand käyttäjän tilin luontityökalut tai ohjelmointirajapinnan myöntämä XMatters OnDemand säännöstä AAD käyttäjätileille.

##<a name="assigning-users"></a>Käyttäjien määrittäminen
  
Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-xmatters-ondemand-perform-the-following-steps"></a>Jos haluat määrittää käyttäjiä XMatters OnDemand, toimi seuraavasti:

1.  Testaa tilin luominen Azure perinteinen portaalissa.

2.  Valitse integration **XMatters OnDemand **sovellus-sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC776799.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Kyllä] (./media/active-directory-saas-xmatters-ondemand-tutorial/IC767830.png "Kyllä")
  
Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli. Katso lisätietoja Access-paneelin [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).