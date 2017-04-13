<properties 
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi BlueJeans | Microsoft Azure" 
    description="Opettele käyttämään BlueJeans Azure Active Directory-hakemistosta käyttöön kertakirjautumisen, automaattinen valmistelu ja lisää!" 
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

#<a name="tutorial-azure-ad-integration-with-bluejeans"></a>Opetusohjelma: BlueJeans Azure AD-integrointi

Tässä opetusohjelmassa tavoitteena on näyttämään Azure ja BlueJeans integrointi.  
Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   Käytössä BlueJeans kertakirjautumisen tilaus

Kun olet suorittanut Tässä opetusohjelmassa, olet liittänyt BlueJeans Azure AD-käyttäjien saa oikeuden kertakirjauksen BlueJeans yrityksen sivuston (palveluntarjoajan aloittanut allekirjoitus-) tai käyttämällä [Access-paneelin esittely](active-directory-saas-access-panel-introduction.md)-sovellukseen.

Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1.  BlueJeans sovelluksen integrointi ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Määrittäminen käyttäjän valmistelu
4.  Käyttäjien määrittäminen

![Skenaario] (./media/active-directory-saas-bluejeans-tutorial/IC785860.png "Skenaario")
##<a name="enabling-the-application-integration-for-bluejeans"></a>BlueJeans sovelluksen integrointi ottaminen käyttöön

Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön BlueJeans sovelluksen integrointi.

###<a name="to-enable-the-application-integration-for-bluejeans-perform-the-following-steps"></a>BlueJeans sovelluksen integrointi käyttöön toimimalla seuraavasti:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-bluejeans-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-bluejeans-tutorial/IC700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-bluejeans-tutorial/IC749321.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-bluejeans-tutorial/IC749322.png "Lisää sovellus-gallerry")

6.  Kirjoita **hakuruutuun** **BlueJeans**.

    ![Sovelluksen valikoima] (./media/active-directory-saas-bluejeans-tutorial/IC785861.png "Sovelluksen valikoima")

7.  Valitse tulosruudussa **BlueJeans**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![BlueJeans] (./media/active-directory-saas-bluejeans-tutorial/IC785862.png "BlueJeans")
##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen

Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjien todentamiseen BlueJeans käyttämällä SAML-protokollan perusteella federation Azure AD-tilin kanssa.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Valitse Azure perinteinen portaalissa integrointi **BlueJeans** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-bluejeans-tutorial/IC785863.png "Määritä kertakirjautuminen")

2.  **Miten käyttäjät voivat kirjautua BlueJeans haluat käyttää** sivulla Valitse **Microsoft Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-bluejeans-tutorial/IC785864.png "Kertakirjautumisen määrittäminen")

3.  **Määritä sovelluksen URL-osoite** -sivulla **BlueJeans merkki-URL** -tekstiruutuun Kirjoita käyttämällä seuraavaa kaavaa "*https://company.BlueJeans.com*" URL-osoite ja valitse sitten **Seuraava**.

    ![Sovelluksen URL-Osoitteen määrittäminen] (./media/active-directory-saas-bluejeans-tutorial/IC785865.png "Sovelluksen URL-Osoitteen määrittäminen")

4.  Valitse **Määritä kertakirjautumisen osoitteessa BlueJeans** -sivulla Lataa sertifikaatti, valitsemalla **Lataa**, ja Tallenna sertifikaattitiedosto tietokoneeseen.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-bluejeans-tutorial/IC785866.png "Kertakirjautumisen määrittäminen")

5.  Eri selainikkunassa Kirjaudu **BlueJeans** yrityksesi sivuston järjestelmänvalvojan oikeuksilla.

6.  Siirry **JÄRJESTELMÄNVALVOJAN \> ryhmäasetusten \> suojauksen**.

    ![Järjestelmänvalvoja] (./media/active-directory-saas-bluejeans-tutorial/IC785868.png "Järjestelmänvalvoja")

7.  Valitse **Suojaus** -kohdassa toimimalla seuraavasti:

    ![SAML-kertakirjautuminen] (./media/active-directory-saas-bluejeans-tutorial/IC785869.png "SAML-kertakirjautuminen")

    1.  Valitse **SAML kertakirjautuminen**.
    2.  Valitse **Ota käyttöön automaattinen valmisteleminen**.

8.  Siirrä seuraavasti:

    ![Varmenteen polku] (./media/active-directory-saas-bluejeans-tutorial/IC785870.png "Varmenteen polku")

    1.  **Valitse tiedosto**ja lataa sitten ladattu varmenne.
    2.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa BlueJeans** -valintasivun **Remote sisäänkirjautumisen URL-osoite** -arvo kopioi ja liitä se sitten **Kirjautuminen URL** -tekstiruutuun.
    3.  Azure perinteinen portaalissa-sivulla **Määritä kertakirjautumisen osoitteessa BlueJeans** valintaikkuna **Muuta salasana URL-osoite** -arvo kopioi ja liitä se sitten **Salasana Muuta URL-osoite** -tekstiruutu.
    4.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa BlueJeans** -valintasivun **Remote Kirjaudu URL-osoite** -arvo kopioi ja liitä se sitten **Kirjaudu ulos URL** -tekstiruutuun.

9.  Siirrä seuraavasti:

    ![Tallenna muutokset] (./media/active-directory-saas-bluejeans-tutorial/IC785874.png "Tallenna muutokset")

    1.  Kirjoita **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name** **käyttäjätunnus** -tekstiruutuun.
    2.  Kirjoita **sähköpostiosoite** -tekstiruutuun **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name**.
    3.  Valitse **Tallenna muutokset**.

10. Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Valmis** **Määrittäminen kertakirjautuminen** -valintaikkunassa Sulje.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-bluejeans-tutorial/IC785876.png "Kertakirjautumisen määrittäminen")
##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu

Jos haluat ottaa käyttöön Azure AD-käyttäjät voivat kirjautua sisään BlueJeans, ne on valmisteltava BlueJeans kyselyjä.  
BlueJeans valmistelu on manuaalinen tehtävä.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Valmistelu käyttäjätilit, toimi seuraavasti:

1.  Kirjaudu sisään **BlueJeans** yrityksen sivuston järjestelmänvalvojan oikeuksilla.

2.  Siirry **JÄRJESTELMÄNVALVOJAN \> käyttäjien hallinta \> Lisää käyttäjä**.

    ![Järjestelmänvalvoja] (./media/active-directory-saas-bluejeans-tutorial/IC785877.png "Järjestelmänvalvoja")

    >[AZURE.IMPORTANT] **Lisää käyttäjä** -välilehti näkyy vain, jos **Suojaus-välilehti**, **Ota käyttöön automaattinen valmisteleminen** ei ole valittuna.

3.  Valitse **Lisää käyttäjä** -osassa seuraavasti:

    ![Käyttäjän lisääminen] (./media/active-directory-saas-bluejeans-tutorial/IC785886.png "Käyttäjän lisääminen")

    1.  Kirjoita **BlueJeans käyttäjänimi**, **sähköpostiosoitteen**, **BlueJeans kokouksen tunnus**, **Valvojan tunnuskoodi**, **Nimen**, **yrityksen** kelvollinen AAD-tili, jonka haluat valmistella liittyvät tekstiruutujen kyselyjä.
    2.  Valitse **Lisää käyttäjä**.

>[AZURE.NOTE] Voit käyttää mitä tahansa muita BlueJeans käyttäjän tilin luontityökalut tai ohjelmointirajapinnan myöntämä BlueJeans säännöstä AAD käyttäjätileille.

##<a name="assigning-users"></a>Käyttäjien määrittäminen

Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-bluejeans-perform-the-following-steps"></a>Jos haluat määrittää käyttäjiä BlueJeans, toimi seuraavasti:

1.  Testaa tilin luominen Azure perinteinen portaalissa.

2.  Valitse integration **BlueJeans **sovellus-sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-bluejeans-tutorial/IC785887.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Kyllä] (./media/active-directory-saas-bluejeans-tutorial/IC767830.png "Kyllä")

Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli. Katso lisätietoja Access-paneelin [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).
