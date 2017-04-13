<properties 
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi Adobe EchoSign | Microsoft Azure" 
    description="Opettele käyttämään Adobe EchoSign Azure Active Directory-hakemistosta käyttöön kertakirjautumisen, automaattinen valmistelu ja lisää!" 
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

#<a name="tutorial-azure-active-directory-integration-with-adobe-echosign"></a>Opetusohjelma: Adobe EchoSign Azure Active Directory-integrointi

Tässä opetusohjelmassa tavoitteena on näyttämään Azure ja Adobe EchoSign integrointi.  
Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   Adobe-EchoSign, jossa on yksi Kirjaudu käytössä-tilauksessa

Kun olet suorittanut Tässä opetusohjelmassa, olet liittänyt Adobe EchoSign Azure AD-käyttäjien saa oikeuden kertakirjauksen Adobe EchoSign yrityksesi sivuston (palveluntarjoajan aloittanut allekirjoitus-) tai käyttämällä [Access-paneelin esittely](active-directory-saas-access-panel-introduction.md)-sovellukseen.

Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1.  Adobe EchoSign sovelluksen integrointi ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Määrittäminen käyttäjän valmistelu
4.  Käyttäjien määrittäminen

![Skenaario] (./media/active-directory-saas-adobe-echosign-tutorial/IC789511.png "Skenaario")
##<a name="enabling-the-application-integration-for-adobe-echosign"></a>Adobe EchoSign sovelluksen integrointi ottaminen käyttöön

Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön Adobe EchoSign sovelluksen integrointi.

###<a name="to-enable-the-application-integration-for-adobe-echosign-perform-the-following-steps"></a>Adobe EchoSign sovelluksen integrointi käyttöön toimimalla seuraavasti:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-adobe-echosign-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-adobe-echosign-tutorial/IC700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-adobe-echosign-tutorial/IC749321.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-adobe-echosign-tutorial/IC749322.png "Lisää sovellus-gallerry")

6.  Kirjoita **hakuruutuun** **Adobe EchoSign**.

    ![Sovelluksen valikoima] (./media/active-directory-saas-adobe-echosign-tutorial/IC789514.png "Sovelluksen valikoima")

7.  Valitse tulosruudussa **Adobe EchoSign**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![Adobe EchoSign] (./media/active-directory-saas-adobe-echosign-tutorial/IC789515.png "Adobe EchoSign")
##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen

Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjien todentamiseen Adobe EchoSign käyttämällä SAML-protokollan perusteella federation Azure AD-tilin kanssa.  
Tämän toimintosarjan osana on base-64-koodattu sertifikaattitiedosto luomiseen.  
Jos et ole tottunut näin, katso, [miten voit muuntaa binaariluvun varmenteen tekstitiedostoon](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Valitse Azure perinteinen portaalissa integrointi **Adobe EchoSign** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-adobe-echosign-tutorial/IC789516.png "Kertakirjautumisen määrittäminen")

2.  **Miten käyttäjät voivat kirjautua Adobe EchoSign haluat käyttää** sivulla Valitse **Microsoft Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-adobe-echosign-tutorial/IC789517.png "Kertakirjautumisen määrittäminen")

3.  **Määritä sovelluksen URL-osoite** -sivulla **Adobe EchoSign merkki-URL** -tekstiruutuun Kirjoita käyttämällä seuraavaa kaavaa "*https://company.echosign.com/*" URL-osoite ja valitse sitten **Seuraava**.

    ![Sovelluksen URL-Osoitteen määrittäminen] (./media/active-directory-saas-adobe-echosign-tutorial/IC789518.png "Sovelluksen URL-Osoitteen määrittäminen")

4.  Valitse **Määritä kertakirjautumisen osoitteessa Adobe EchoSign** -sivulla valitsemalla **Lataa**ja Tallenna sertifikaattitiedosto tietokoneeseen.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-adobe-echosign-tutorial/IC789519.png "Kertakirjautumisen määrittäminen")

5.  Eri selainikkunassa Kirjaudu Adobe EchoSign yrityksesi sivuston järjestelmänvalvojan oikeuksilla.

6.  Valitse ylä-valikossa **tilin**ja valitse sitten, **SAML asetukset** siirtymisruudun vasemmasta die **Tilin asetukset**-kohdassa.

    ![Tilin] (./media/active-directory-saas-adobe-echosign-tutorial/IC789520.png "Tilin")

7.  SAML-asetukset-osassa seuraavasti:

    ![SAML-asetukset] (./media/active-directory-saas-adobe-echosign-tutorial/IC789521.png "SAML-asetukset")

    1.  **SAML-tilassa**Valitse **SAML pakollinen**.
    2.  Valitse **Salli EchoSign tilin järjestelmänvalvojat voivat kirjautua sisään käyttämällä EchoSign tunnistetietoja**.
    3.  Valitse **Käyttäjän luominen**, **lisätä automaattisesti kautta SAML todennetut käyttäjät**.

8.  Siirrä, seuraavasti:

    ![SAML-asetukset] (./media/active-directory-saas-adobe-echosign-tutorial/IC789522.png "SAML-asetukset")

    1.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa Adobe EchoSign** -valintasivun kopioi **Kohteen tunnus** -arvon ja liitä se sitten **IdP kohteen tunnus** -tekstiruutuun.
    2.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa Adobe EchoSign** -valintasivun **Remote sisäänkirjautumisen URL-osoite** -arvo kopioi ja liitä se sitten **IdP kirjautuminen URL** -tekstiruutuun.
    3.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa Adobe EchoSign** -valintasivun **Remote Kirjaudu URL-osoite** -arvo kopioi ja liitä se sitten **IdP Kirjaudu URL** -tekstiruutuun.
    4.  Luo **base-64-koodattu** tiedosto ladattu varmennetta.  

        >[AZURE.TIP] Lisätietoja on artikkelissa [muuntaa binaariluvun varmenteen tekstitiedostoon](http://youtu.be/PlgrzUZ-Y1o) 
 5.  Avaa base-64-koodattu varmennetta Muistiossa, kopioi se sisällön Leikepöydän ja liitä se **IdP varmenteen** tekstiruudun 6.  Valitse **Tallenna muutokset**.

9.  Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Valmis** **Määrittäminen kertakirjautuminen** -valintaikkunassa Sulje.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-adobe-echosign-tutorial/IC789523.png "Kertakirjautumisen määrittäminen")
##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu

Jos haluat ottaa käyttöön Azure AD-käyttäjät voivat kirjautua sisään Adobe EchoSign, ne on valmisteltava Adobe EchoSign kyselyjä.  
Adobe EchoSign valmistelu on manuaalinen tehtävä.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Valmistelu käyttäjätilit, toimi seuraavasti:

1.  Kirjaudu sisään **Adobe EchoSign** yrityksesi sivuston järjestelmänvalvojana.

2.  Ylä-valikossa **tilin**siirtymisruudun vasemmasta die sitten **käyttäjät ja ryhmät**, ja valitse **Luo uusi käyttäjä**.

    ![Tilin] (./media/active-directory-saas-adobe-echosign-tutorial/IC789524.png "Tilin")

3.  **Luo uusi käyttäjä** -osassa seuraavasti:

    ![Käyttäjän luominen] (./media/active-directory-saas-adobe-echosign-tutorial/IC789525.png "Käyttäjän luominen")

    1.  Kirjoita **Sähköpostiosoite**, **Etunimi** ja **Sukunimi** kelvollinen AAD tilin haluat säännöstä liittyvät tekstiruutujen kyselyjä.
    2.  Valitse **Luo käyttäjä**.

        >[AZURE.NOTE] Azure Active Directory-tilin omistaja saavat sähköpostiviestin, joka sisältää linkin Vahvista tili, ennen kuin se on aktiivinen.

>[AZURE.NOTE] Voit käyttää mitä tahansa muita Adobe EchoSign käyttäjän tilin luontityökalut tai ohjelmointirajapinnan myöntämä Adobe EchoSign säännöstä AAD käyttäjätileille.

##<a name="assigning-users"></a>Käyttäjien määrittäminen

Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-adobe-echosign-perform-the-following-steps"></a>Jos haluat määrittää käyttäjiä Adobe EchoSign, toimi seuraavasti:

1.  Testaa tilin luominen Azure perinteinen portaalissa.

2.  Valitse integration **Adobe EchoSign **sovellus-sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-adobe-echosign-tutorial/IC789526.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Kyllä] (./media/active-directory-saas-adobe-echosign-tutorial/IC767830.png "Kyllä")

Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli. Katso lisätietoja Access-paneelin [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).
