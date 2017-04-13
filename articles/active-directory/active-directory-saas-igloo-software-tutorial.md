<properties 
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi Igloo ohjelmiston | Microsoft Azure" 
    description="Opettele käyttämään Igloo ohjelmiston Azure Active Directory-hakemistosta käyttöön kertakirjautumisen, automaattinen valmistelu ja lisää!" 
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
    ms.date="10/20/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-igloo-software"></a>Opetusohjelma: Igloo ohjelmiston Azure Active Directory-integrointi
  
Tässä opetusohjelmassa tavoitteena on näyttämään Azure ja Igloo ohjelmiston integrointi.  
Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   [Igloo ohjelmisto](http://www.igloosoftware.com/) -kertakirjauksen, käytössä-tilauksessa
  
Kun olet suorittanut Tässä opetusohjelmassa, olet määrittänyt Igloo-ohjelmistoa Azure AD-käyttäjien saa oikeuden kertakirjauksen Igloo ohjelmiston yrityksesi sivuston (palveluntarjoajan aloittanut allekirjoitus-) tai käyttämällä [Access-paneelin esittely](active-directory-saas-access-panel-introduction.md)-sovellukseen.
  
Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1.  Sovelluksen integrointi Igloo ohjelmiston ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Määrittäminen käyttäjän valmistelu
4.  Käyttäjien määrittäminen

![Skenaario] (./media/active-directory-saas-igloo-software-tutorial/IC783961.png "Skenaario")
##<a name="enabling-the-application-integration-for-igloo-software"></a>Sovelluksen integrointi Igloo ohjelmiston ottaminen käyttöön
  
Tässä osassa tavoitteena on Jäsennys, miten voit ottaa käyttöön sovelluksen integrointi Igloo ohjelmisto.

###<a name="to-enable-the-application-integration-for-igloo-software-perform-the-following-steps"></a>Ottaa käyttöön sovelluksen integroinnin Igloo ohjelmisto, toimi seuraavasti:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-igloo-software-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-igloo-software-tutorial/IC700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-igloo-software-tutorial/IC749321.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-igloo-software-tutorial/IC749322.png "Lisää sovellus-gallerry")

6.  Kirjoita **hakuruutuun** **Igloo ohjelmiston**.

    ![Sovelluksen valikoima] (./media/active-directory-saas-igloo-software-tutorial/IC783962.png "Sovelluksen valikoima")

7.  Valitse tulosruudussa **Igloo ohjelmisto**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![Igloo] (./media/active-directory-saas-igloo-software-tutorial/IC783963.png "Igloo")
##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjien todentamiseen Igloo ohjelmisto käyttämällä SAML-protokollan perusteella federation Azure AD-tilin kanssa.  
Tämän toimintosarjan osana on base-64-koodattu varmenteen lataaminen keskitetyn työpöydän alihallintaan.  
Jos et ole tottunut näin, katso, [miten voit muuntaa binaariluvun varmenteen tekstitiedostoon](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Valitse Azure perinteinen portaalissa **Igloo ohjelmisto** sovelluksen integrointi-sivun **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-igloo-software-tutorial/IC783964.png "Kertakirjautumisen määrittäminen")

2.  **Miten käyttäjät voivat kirjautua Igloo ohjelmiston haluat käyttää** sivulla Valitse **Microsoft Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Microsoft Azure AD-Single Sign-On] (./media/active-directory-saas-igloo-software-tutorial/IC783965.png "Microsoft Azure AD-Single Sign-On")

3.  **Määritä sovelluksen URL-osoite** -sivulla **Igloo ohjelmiston merkki-URL** -tekstiruutuun Kirjoita käyttämällä seuraavaa kaavaa "*https://company.igloocommunities.com/?signin*" URL-osoite ja valitse sitten **Seuraava**.

    ![Sovelluksen URL-Osoitteen määrittäminen] (./media/active-directory-saas-igloo-software-tutorial/IC773625.png "Sovelluksen URL-Osoitteen määrittäminen")

4.  **Määritä kertakirjautumisen Igloo-ohjelma** -sivulla Lataa sertifikaatti, valitse **Lataa**, ja Tallenna sertifikaattitiedosto paikallisesti tietokoneeseen.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-igloo-software-tutorial/IC783966.png "Kertakirjautumisen määrittäminen")

5.  Eri selainikkunassa Kirjaudu Igloo ohjelmisto yrityksesi sivuston järjestelmänvalvojan oikeuksilla.

6.  Valitse **Ohjauspaneeli**.

    ![Ohjauspaneeli] (./media/active-directory-saas-igloo-software-tutorial/IC799949.png "Ohjauspaneeli")

7.  Valitse **Kirjaudu sisään asetukset** **jäsenyys** -välilehteä.

    ![Kirjaudu sisään asetukset] (./media/active-directory-saas-igloo-software-tutorial/IC783968.png "Kirjaudu sisään asetukset")

8.  Valitse SAML Tietolähdemääritykset-osasta **Määrittäminen SAML todennusta**.

    ![SAML-määritys] (./media/active-directory-saas-igloo-software-tutorial/IC783969.png "SAML-määritys")

9.  **Yleisten määritysten** -osassa seuraavasti:

    ![Yleisten määritysten] (./media/active-directory-saas-igloo-software-tutorial/IC783970.png "Yleisten määritysten")

    1.  Kirjoita **Nimi** -tekstiruutuun kokoonpanosi mukautettu nimi.
    2.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen Igloo ohjelma** valintaikkunan-sivulla kopioi **Remote sisäänkirjautumisen URL-osoite** -arvo ja liitä se sitten **IdP kirjautuminen URL** -tekstiruutuun.
    3.  Azure perinteinen portaalissa-sivulla **Määritä kertakirjautumisen Igloo ohjelma** valintaikkunan **Remote Kirjaudu URL-osoite** -arvo kopioi ja liitä se sitten **IdP Kirjaudu URL** -tekstiruutuun.
    4.  **Kirjaudu ulos vastauksen ja pyynnön HTTP tyyppi**valitsemalla **viesti**.
    5.  Luo tekstitiedosto ladatut varmenteesta.
        
        >[AZURE.TIP]Lisätietoja on artikkelissa [muuntaa binaariluvun varmenteen tekstitiedostoon](http://youtu.be/PlgrzUZ-Y1o)

    6.  Ensimmäisen rivin ja viimeisen rivin poistaminen tekstin tiedostoversion varmennetta, jäljellä olevan varmenteen tekstin kopioiminen ja liittäminen **Julkisen varmenne** -tekstiruutuun.

10. **Vastauksen ja käyttöoikeuksien määritykset**toimi seuraavasti:

    ![Vastauksen ja todennuksen määrittäminen] (./media/active-directory-saas-igloo-software-tutorial/IC783971.png "Vastauksen ja todennuksen määrittäminen")

    1.  Valitse **Microsoft ADFS**- **Tunnistetietojen toimittaja**.
    2.  **Tunnuksen tyyppiä**Valitse **Sähköpostiosoite**.
    3.  Kirjoita **sähköpostiosoite** **Sähköposti-määrite** -tekstiruutuun.
    4.  Kirjoita **Etunimi-määrite** -tekstiruutuun **givenname**.
    5.  Kirjoita **Sukunimi** **Viimeisen Name-määrite** -tekstiruutuun.

11. Preform Viimeistele määritys seuraavasti:

    ![Kirjaudu sisään käyttäjän luominen] (./media/active-directory-saas-igloo-software-tutorial/IC783972.png "Kirjaudu sisään käyttäjän luominen")

    1.  **Käyttäjän luominen kirjautumissivulla**Valitse **Luo uusi käyttäjä, kun synkronointi-sivustossa**.
    2.  **Kirjaudu sisään asetukset**Valitse **"Kirjautuminen" näytön käyttäminen SAML-painiketta**.
    3.  Valitse **Tallenna**.

12. Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Valmis** **Määrittäminen kertakirjautuminen** -valintaikkunassa Sulje.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-igloo-software-tutorial/IC783973.png "Kertakirjautumisen määrittäminen")
##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu
  
Ei ei toimi, voit määrittää käyttäjän valmistelu Igloo-ohjelmistoa.  
Kun määritetty käyttäjä yrittää kirjautua Igloo ohjelmistojen käyttäminen access-paneeli, Igloo ohjelmiston tarkistaa käyttäjä on.  
Jos ei ole käyttäjätiliä käytettävissä vielä, se luodaan automaattisesti Igloo ohjelmisto.
##<a name="assigning-users"></a>Käyttäjien määrittäminen
  
Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-igloo-software-perform-the-following-steps"></a>Jos haluat määrittää käyttäjiä Igloo ohjelmiston, toimi seuraavasti:

1.  Testaa tilin luominen Azure perinteinen portaalissa.

2.  Valitse **Igloo ohjelmiston **sovelluksen integrointi-sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-igloo-software-tutorial/IC783974.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Kyllä] (./media/active-directory-saas-igloo-software-tutorial/IC767830.png "Kyllä")
  
Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli. Katso lisätietoja Access-paneelin [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).