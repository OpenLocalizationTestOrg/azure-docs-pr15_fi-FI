<properties 
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi Lucidchart | Microsoft Azure" 
    description="Opettele käyttämään Lucidchart Azure Active Directory-hakemistosta käyttöön kertakirjautumisen, automaattinen valmistelu ja lisää!" 
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

#<a name="tutorial-azure-active-directory-integration-with-lucidchart"></a>Opetusohjelma: Lucidchart Azure Active Directory-integrointi
  
Tässä opetusohjelmassa tavoitteena on näyttämään Azure ja Lucidchart integrointi.  
Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   Käytössä Lucidchart kertakirjautumisen tilaus
  
Kun olet suorittanut Tässä opetusohjelmassa, olet liittänyt Lucidchart Azure AD-käyttäjien saa oikeuden kertakirjauksen Lucidchart yrityksesi sivuston (palveluntarjoajan aloittanut allekirjoitus-) tai käyttämällä [Access-paneelin esittely](active-directory-saas-access-panel-introduction.md)-sovellukseen.
  
Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1.  Lucidchart sovelluksen integrointi ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Määrittäminen käyttäjän valmistelu
4.  Käyttäjien määrittäminen

![Skenaario] (./media/active-directory-saas-lucidchart-tutorial/IC791183.png "Skenaario")
##<a name="enabling-the-application-integration-for-lucidchart"></a>Lucidchart sovelluksen integrointi ottaminen käyttöön
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön Lucidchart sovelluksen integrointi.

###<a name="to-enable-the-application-integration-for-lucidchart-perform-the-following-steps"></a>Lucidchart sovelluksen integrointi käyttöön toimimalla seuraavasti:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-lucidchart-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-lucidchart-tutorial/IC700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-lucidchart-tutorial/IC749321.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-lucidchart-tutorial/IC749322.png "Lisää sovellus-gallerry")

6.  Kirjoita **hakuruutuun** **Lucidchart**.

    ![Sovelluksen valikoima] (./media/active-directory-saas-lucidchart-tutorial/IC791184.png "Sovelluksen valikoima")

7.  Valitse tulosruudussa **Lucidchart**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![Lucidchart] (./media/active-directory-saas-lucidchart-tutorial/IC791185.png "Lucidchart")
##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjien todentamiseen Lucidchart käyttämällä SAML-protokollan perusteella federation Azure AD-tilin kanssa.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Valitse Azure perinteinen portaalissa integrointi **Lucidchart** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-lucidchart-tutorial/IC791186.png "Kertakirjautumisen määrittäminen")

2.  **Miten käyttäjät voivat kirjautua Lucidchart haluat käyttää** sivulla Valitse **Microsoft Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-lucidchart-tutorial/IC791187.png "Kertakirjautumisen määrittäminen")

3.  Kirjoita **Määritä sovelluksen URL-osoite** -sivulla **Lucidchart merkki-URL** -tekstiruutuun käyttämä käyttäjien kirjautua Lucidchart sovelluksen URL-osoite (esimerkiksi: "*https://chart2.office.lucidchart.com/saml/sso/azure*"), ja valitse sitten **Seuraava**.

    ![Sovelluksen URL-Osoitteen määrittäminen] (./media/active-directory-saas-lucidchart-tutorial/IC791188.png "Sovelluksen URL-Osoitteen määrittäminen")

4.  **Määritä kertakirjautumisen osoitteessa Lucidchart** -sivulla lataamaan metatietojen valitsemalla **Lataa metatietojen**, ja Tallenna tiedosto paikallisesti tietokoneeseen.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-lucidchart-tutorial/IC791189.png "Kertakirjautumisen määrittäminen")

5.  Eri selainikkunassa Kirjaudu Lucidchart yrityksesi sivuston järjestelmänvalvojan oikeuksilla.

6.  Valitse ylä-valikossa **ryhmän**.

    ![Ryhmän] (./media/active-directory-saas-lucidchart-tutorial/IC791190.png "Ryhmän")

7.  Valitse **sovelluksen \> hallinta SAML**.

    ![SAML hallinta] (./media/active-directory-saas-lucidchart-tutorial/IC791191.png "SAML hallinta")

8.  Suorita **SAML todennusasetukset** -sivulla seuraavat toimet:

    1.  Valitse **Ota käyttöön SAML todennus**ja valitse sitten **Valinnainen**.
        ![SAML todennusasetukset] (./media/active-directory-saas-lucidchart-tutorial/IC791192.png "SAML todennusasetukset")
    2.  Kirjoita **toimialue** -tekstiruutuun toimialueen nimi ja valitse sitten **Muuta varmennetta**.
        ![Muuta varmennetta] (./media/active-directory-saas-lucidchart-tutorial/IC791193.png "Muuta varmennetta")
    3.  Avaa ladatut metatieto-tiedosto, kopioi sisältö ja liitä se sitten **Lataa metatiedot** -tekstiruutu.
        ![Lataa metatiedot] (./media/active-directory-saas-lucidchart-tutorial/IC791194.png "Lataa metatiedot")
    4.  Valitse **Lisää automaattisesti uuden ryhmän käyttäjän**ja valitse sitten **Tallenna muutokset**.
        ![Tallenna muutokset] (./media/active-directory-saas-lucidchart-tutorial/IC791195.png "Tallenna muutokset")

9.  Valitse Sign määritysten vahvistus ja valitse sitten **Valmis** **Määrittäminen kertakirjautuminen** -valintaikkunassa Sulje.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-lucidchart-tutorial/IC791196.png "Kertakirjautumisen määrittäminen")
##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu
  
Ei ei toimi, voit määrittää käyttäjän valmistelu Lucidchart.  
Kun määritetty käyttäjä yrittää kirjautua käyttämällä access-paneelin Lucidchart, Lucidchart tarkistaa, onko käyttäjän olemassa.  
Jos ei ole käyttäjätiliä käytettävissä vielä, se luodaan automaattisesti Lucidchart mukaan.
##<a name="assigning-users"></a>Käyttäjien määrittäminen
  
Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-lucidchart-perform-the-following-steps"></a>Jos haluat määrittää käyttäjiä Lucidchart, toimi seuraavasti:

1.  Testaa tilin luominen Azure perinteinen portaalissa.

2.  Valitse integration **Lucidchart **sovellus-sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-lucidchart-tutorial/IC791197.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Kyllä] (./media/active-directory-saas-lucidchart-tutorial/IC767830.png "Kyllä")
  
Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli. Katso lisätietoja Access-paneelin [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).