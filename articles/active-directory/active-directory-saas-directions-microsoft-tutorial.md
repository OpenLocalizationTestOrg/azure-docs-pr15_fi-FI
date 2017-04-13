<properties 
    pageTitle="Opetusohjelma: Ohjeet Microsoft Azure Active Directory-integrointi | Microsoft Azure" 
    description="Opettele käyttämään ohjeita voit ottaa käyttöön kertakirjautumisen, automaattinen valmistelu ja muut Microsoft Azure Active Directory-hakemistosta!" 
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

#<a name="tutorial-azure-active-directory-integration-with-directions-on-microsoft"></a>Opetusohjelma: Ohjeet Microsoft Azure Active Directory-integrointi

Tässä opetusohjelmassa tavoitteena on Microsoft Azure Active Directory- ja ohjeita integrointi näyttäminen.  
Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   Ohjeita Microsoft-tilauksessa

Jos sinulla ei ole liitetty ohjeita vielä Microsoft-tilauksessa, sähköpostin pyyntö "*service@DirectionsOnMicrosoft.com*".

Kun olet suorittanut Tässä opetusohjelmassa, olet liittänyt ohjeet Microsoft Azure Active Directory-käyttäjät saa oikeuden kertakirjauksen käyttämällä single Sign-sovellukseen.

Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1.  Saat ohjeet Microsoft application-integraatio ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Määrittäminen käyttäjän valmistelu
4.  Käyttäjien määrittäminen

![Skenaario] (./media/active-directory-saas-directions-microsoft-tutorial/IC786877.png "Skenaario")
##<a name="enabling-the-application-integration-for-directions-on-microsoft"></a>Saat ohjeet Microsoft application-integraatio ottaminen käyttöön

Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön saat ohjeet Microsoft application-integraatio.

###<a name="to-enable-the-application-integration-for-directions-on-microsoft-perform-the-following-steps"></a>Saat ohjeet Microsoft-sovelluksen-integroinnin valitseminen käyttöön, toimi seuraavasti:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-directions-microsoft-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-directions-microsoft-tutorial/IC700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-directions-microsoft-tutorial/IC749321.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-directions-microsoft-tutorial/IC749322.png "Lisää sovellus-gallerry")

6.  Kirjoita **hakuruutuun** **Microsoft ohjeet**.

    ![Sovelluksen valikoima] (./media/active-directory-saas-directions-microsoft-tutorial/IC786878.png "Sovelluksen valikoima")

7.  Tulosruudussa Valitse **Microsoft ohjeet**ja valitse sitten **Valmis** , voit lisätä sovelluksen.

    ![Skenaario] (./media/active-directory-saas-directions-microsoft-tutorial/IC793922.png "Skenaario")
##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen

Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjien todentamiseen ohjeet Microsoft Azure AD käyttämällä SAML-protokollan perusteella federation niiden tilillä.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Valitse Azure perinteinen portaalissa integrointi **ohjeita Microsoftin** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Ottaa käyttöön kertakirjautumisen] (./media/active-directory-saas-directions-microsoft-tutorial/IC786879.png "Ottaa käyttöön kertakirjautumisen")

2.  **Miten käyttäjät voivat kirjautua Microsoft ohjeet haluat käyttää** sivulla Valitse **Microsoft Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Microsoft Azure AD Singel Sign-On] (./media/active-directory-saas-directions-microsoft-tutorial/IC786880.png "Microsoft Azure AD Singel Sign-On")

3.  **Määritä sovelluksen URL-osoite** -sivulla merkki-URL-tekstiruutuun Kirjoita **https://www.directionsonmicrosoft.com/user/login**ja valitse sitten **Seuraava**.

    ![Sovelluksen URL-Osoitteen määrittäminen] (./media/active-directory-saas-directions-microsoft-tutorial/IC786881.png "Sovelluksen URL-Osoitteen määrittäminen")

4.  **Määritä kertakirjautumisen osoitteessa Microsoft ohjeet** -sivulle valitsemalla **Lataa metatiedot**ja Tallenna paikallisesti tietokoneen metatiedot-tiedostoon.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-directions-microsoft-tutorial/IC786882.png "Kertakirjautumisen määrittäminen")

5.  Lähettää tiedoston metatietojen ohjeita Microsoftin tukeen, (*service@DirectionsOnMicrosoft.com*). Voit ottaa käyttöön ohjeita Microsoftin tekniseen tukeen, Etsi liitetyt sivuston jäsenyys, yritystietojen sisällyttäminen sähköpostin.

    >[AZURE.NOTE] Kertakirjautumisen suuntiin Microsoft täytyy ottaa käyttöön Microsoft tukihenkilöstön ohjeet.
Saat ilmoituksen, kun kertakirjautumisen on otettu käyttöön.

6.  Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Valmis** **Määrittäminen kertakirjautuminen** -valintaikkunassa Sulje.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-directions-microsoft-tutorial/IC786883.png "Kertakirjautumisen määrittäminen")
##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu

Ei ei toimi, voit määrittää käyttäjän valmistelu ohjeet Microsoft.  
Kun määritetty käyttäjä yrittää kirjautua Microsoft access-paneelin käyttäminen ohjeet, ohjeita Microsoft tarkistaa, onko käyttäjän olemassa. Jos ei ole käyttäjätiliä käytettävissä vielä, se luodaan automaattisesti ohjeet Microsoft mukaan.
##<a name="assigning-users"></a>Käyttäjien määrittäminen

Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-directions-on-microsoft-perform-the-following-steps"></a>Jos haluat määrittää käyttäjät Microsoft ohjeet, toimi seuraavasti:

1.  Testaa tilin luominen Azure perinteinen portaalissa.

2.  Valitse **ohjeet Microsoft **application integration-sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-directions-microsoft-tutorial/IC786884.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Kyllä] (./media/active-directory-saas-directions-microsoft-tutorial/IC767830.png "Kyllä")
