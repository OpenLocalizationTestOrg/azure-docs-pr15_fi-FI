<properties 
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi Boomi | Microsoft Azure" 
    description="Opettele käyttämään Boomi Azure Active Directory-hakemistosta käyttöön kertakirjautumisen, automaattinen valmistelu ja lisää!" 
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

#<a name="tutorial-azure-active-directory-integration-with-boomi"></a>Opetusohjelma: Boomi Azure Active Directory-integrointi

Tässä opetusohjelmassa tavoitteena on näyttämään Azure ja Boomi integrointi.  
Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   Käytössä Boomi kertakirjautumisen tilaus

Kun olet suorittanut Tässä opetusohjelmassa, olet liittänyt Boomi Azure AD-käyttäjien saa oikeuden kertakirjauksen Boomi yrityksesi sivuston (palveluntarjoajan aloittanut allekirjoitus-) tai käyttämällä [Access-paneelin esittely](active-directory-saas-access-panel-introduction.md)-sovellukseen.

Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1.  Boomi sovelluksen integrointi ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Määrittäminen käyttäjän valmistelu
4.  Käyttäjien määrittäminen

![Skenaario] (./media/active-directory-saas-boomi-tutorial/IC791134.png "Skenaario")
##<a name="enabling-the-application-integration-for-boomi"></a>Boomi sovelluksen integrointi ottaminen käyttöön

Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön Boomi sovelluksen integrointi.

###<a name="to-enable-the-application-integration-for-boomi-perform-the-following-steps"></a>Boomi sovelluksen integrointi käyttöön toimimalla seuraavasti:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-boomi-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-boomi-tutorial/IC700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-boomi-tutorial/IC749321.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-boomi-tutorial/IC749322.png "Lisää sovellus-gallerry")

6.  Kirjoita **hakuruutuun** **Boomi**.

    ![Sovelluksen valikoima] (./media/active-directory-saas-boomi-tutorial/IC790822.png "Sovelluksen valikoima")

7.  Valitse tulosruudussa **Boomi**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![Boomi] (./media/active-directory-saas-boomi-tutorial/IC790823.png "Boomi")
##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen

Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjien todentamiseen Boomi käyttämällä SAML-protokollan perusteella federation Azure AD-tilin kanssa.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Valitse Azure perinteinen portaalissa integrointi **Boomi** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-boomi-tutorial/IC790824.png "Kertakirjautumisen määrittäminen")

2.  **Miten käyttäjät voivat kirjautua Boomi haluat käyttää** sivulla Valitse **Microsoft Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-boomi-tutorial/IC790825.png "Kertakirjautumisen määrittäminen")

3.  Kirjoita **Määritä sovelluksen URL-osoite** -sivulla **Boomi vastaa URL** -tekstiruutuun **Boomi AtomSphere sisäänkirjautumisen URL-osoite** (esimerkiksi: "*https://platform.boomi.com/sso/AccountName/saml*"), ja valitse sitten **Seuraava**.

    ![Sovelluksen URL-Osoitteen määrittäminen] (./media/active-directory-saas-boomi-tutorial/IC790826.png "Sovelluksen URL-Osoitteen määrittäminen")

4.  **Määritä kertakirjautumisen osoitteessa Boomi** -sivulla Lataa sertifikaatti, valitse **Lataa**, ja Tallenna sertifikaattitiedosto paikallisesti tietokoneeseen.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-boomi-tutorial/IC790827.png "Kertakirjautumisen määrittäminen")

5.  Eri selainikkunassa Kirjaudu Boomi yrityksesi sivuston järjestelmänvalvojan oikeuksilla.

6.  Valitse yläreunassa olevalla työkalurivillä, yrityksen nimi, ja valitse sitten **asetukset**.

    ![Asetukset] (./media/active-directory-saas-boomi-tutorial/IC790828.png "Asetukset")

7.  Valitse **SSO-asetukset**.

    ![SSO-asetukset] (./media/active-directory-saas-boomi-tutorial/IC790829.png "SSO-asetukset")

8.  **Sign-On asetukset** -osassa seuraavasti:

    ![Sign-asetukset] (./media/active-directory-saas-boomi-tutorial/IC790830.png "Sign-asetukset")

    1.  Valitse **Ota käyttöön SAML kertakirjautumisen**.
    2.  Valitse **Tuo**, lataa ladatut varmenne.
    3.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa Boomi** -valintasivun **Remote sisäänkirjautumisen URL-osoite** -arvo kopioi ja liitä se sitten **Tunnistetietojen toimittaja sisäänkirjautumisen URL-osoite** -tekstiruutuun.
    4.  **Federation tunnus sijaintia**Valitse **Federation tunnus on NameID osan aihe**.
    5.  Valitse **Tallenna**.

9.  Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Valmis** **Määrittäminen kertakirjautuminen** -valintaikkunassa Sulje.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-boomi-tutorial/IC775560.png "Kertakirjautumisen määrittäminen")
##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu

Jos haluat ottaa käyttöön Azure AD-käyttäjät voivat kirjautua sisään Boomi, ne on valmisteltava Boomi kyselyjä.  
Boomi valmistelu on manuaalinen tehtävä.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Voit määrittää käyttäjän valmistelu toimimalla seuraavasti:

1.  Kirjaudu sisään **Boomi** yrityksesi sivuston järjestelmänvalvojana.

2.  Siirry **Käyttäjähallinta \> käyttäjien**.

    ![Käyttäjät] (./media/active-directory-saas-boomi-tutorial/IC790831.png "Käyttäjät")

3.  Valitse **Lisää käyttäjä**.

    ![Käyttäjän lisääminen] (./media/active-directory-saas-boomi-tutorial/IC790832.png "Käyttäjän lisääminen")

4.  Valitse **Lisää liiketoiminta-roolien** -sivulla toimimalla seuraavasti:

    ![Käyttäjän lisääminen] (./media/active-directory-saas-boomi-tutorial/IC790833.png "Käyttäjän lisääminen")

    1.  Kirjoita **Etunimi**, **Sukunimi** ja **sähköpostin** kelvollinen AAD tilin haluat säännöstä liittyvät tekstiruutujen kyselyjä.
    2.  Valitse OK.

>[AZURE.NOTE] Voit käyttää mitä tahansa muita Boomi käyttäjän tilin luontityökalut tai ohjelmointirajapinnan myöntämä Boomi säännöstä AAD käyttäjätileille.

##<a name="assigning-users"></a>Käyttäjien määrittäminen

Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-boomi-perform-the-following-steps"></a>Jos haluat määrittää käyttäjiä Boomi, toimi seuraavasti:

1.  Testaa tilin luominen Azure perinteinen portaalissa.

2.  Valitse integration **Boomi **sovellus-sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-boomi-tutorial/IC790834.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Kyllä] (./media/active-directory-saas-boomi-tutorial/IC767830.png "Kyllä")

Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli. Katso lisätietoja Access-paneelin [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).
