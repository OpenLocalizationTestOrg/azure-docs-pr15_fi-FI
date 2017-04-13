<properties 
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi ralli ohjelmiston | Microsoft Azure" 
    description="Opettele käyttämään Rally ohjelmiston Azure Active Directory-hakemistosta käyttöön kertakirjautumisen, automaattinen valmistelu ja lisää!" 
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
    ms.date="09/26/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-rally-software"></a>Opetusohjelma: Ralli ohjelmiston Azure Active Directory-integrointi
  
Tässä opetusohjelmassa tavoitteena on näyttämään Azure ja Rally ohjelmiston integrointi.  
Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   Rally ohjelmisto palvelutili
  
Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1.  Sovelluksen integrointi Rally ohjelmiston ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Määrittäminen käyttäjän valmistelu
4.  Käyttäjien määrittäminen

![Skenaario] (./media/active-directory-saas-rally-software-tutorial/IC769525.png "Skenaario")
##<a name="enabling-the-application-integration-for-rally-software"></a>Sovelluksen integrointi Rally ohjelmiston ottaminen käyttöön
  
Tässä osassa tavoitteena on Jäsennys, miten voit ottaa käyttöön sovelluksen integrointi Rally-ohjelmiston.

###<a name="to-enable-the-application-integration-for-rally-software-perform-the-following-steps"></a>Ottaa käyttöön sovelluksen integroinnin Rally ohjelmisto, toimi seuraavasti:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-rally-software-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-rally-software-tutorial/IC700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-rally-software-tutorial/IC749321.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-rally-software-tutorial/IC749322.png "Lisää sovellus-gallerry")

6.  Kirjoita **hakuruutuun** **ralli**.

    ![Sovelluksen valikoima] (./media/active-directory-saas-rally-software-tutorial/IC769526.png "Sovelluksen valikoima")

7.  Valitse tulokset-ruudun Valitse **Rally ohjelmiston**ja valitse sitten **Valmis** , voit lisätä sovelluksen.

    ![Rally ohjelmisto] (./media/active-directory-saas-rally-software-tutorial/IC769527.png "Rally ohjelmisto")
##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjien todentamiseen Rally ohjelmisto käyttämällä SAML-protokollan perusteella federation Azure AD-tilin kanssa.  
Tämän toimintosarjan osana tarvitaan varmenteen lataaminen Rally ohjelmiston.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Valitse Azure perinteinen portaalissa **Rally ohjelmisto **sovelluksen integrointi-sivun **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-rally-software-tutorial/IC749323.png "Määritä kertakirjautuminen")

2.  **Miten Haluaisitko kirjautumaan ralli käyttäjät** -sivulla **Microsoft Azure AD kertakirjautumisen**Valitse ja valitse sitten **Seuraava**.

    ![Microsoft Azure AD-Single Sign-On] (./media/active-directory-saas-rally-software-tutorial/IC769528.png "Microsoft Azure AD-Single Sign-On")

3.  Kirjoita **Määritä sovelluksen URL-osoite** -sivulla **Rally ohjelmisto vuokraajan URL** -tekstiruutuun URL-osoite käyttämällä seuraavaa kaavaa "*https://\<vuokraajan nimi\>. rally.com*", ja valitse sitten **Seuraava**.

    ![Sovelluksen URL-Osoitteen määrittäminen] (./media/active-directory-saas-rally-software-tutorial/IC769529.png "Sovelluksen URL-Osoitteen määrittäminen")

4.  Valitse **Määritä kertakirjautumisen osoitteessa ralli** -sivulla Lataa metatiedot ja tallenna se tietokoneeseen.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-rally-software-tutorial/IC769530.png "Määritä kertakirjautuminen")

5.  Kirjaudu sisään **Rally ohjelmiston** alihallintaan.

6.  Yläkulmassa työkalurivin **asetukset**ja valitse sitten **tilaus**.

    ![Tilauksen] (./media/active-directory-saas-rally-software-tutorial/IC769531.png "Tilauksen")

7.  Valitse oikeassa yläkulmassa työkalurivin **toiminto** -painike ja valitse sitten **Muokkaa tilauksen**.

8.  Valitse **tilaus** -sivulla suorittamalla seuraavat vaiheet ja valitse sitten **Tallenna ja sulje**:

    ![Todennus] (./media/active-directory-saas-rally-software-tutorial/IC769542.png "Todennus")

    1.  Valitse todennus avattava **ralli tai SSO-todennus**
    2.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen Rally-ohjelma** -valintasivun **Tunnistetietojen Toimittajan tunnus** -arvo kopioi ja liitä se sitten **Tunnistetietojen toimittaja URL** -tekstiruutuun
    3.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen Rally-ohjelma** -valintasivun kopioi **Remote Kirjaudu URL-osoite** -arvo.

9.  Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Valmis** **Määrittäminen kertakirjautuminen** -valintaikkunassa Sulje.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-rally-software-tutorial/IC769547.png "Määritä kertakirjautuminen")
##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu
  
AAD käyttäjät voivat kirjautua ne on valmisteltava käyttämällä Azure Active Directory-käyttäjätunnuksen Rally ohjelmisto-sovellukseen.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Voit määrittää käyttäjän valmistelu toimimalla seuraavasti:

1.  Kirjaudu sisään Rally ohjelmiston alihallintaan.

2.  Siirry **asennuksen \> käyttäjien**, ja valitse sitten **+ Lisää uusi**.

    ![Käyttäjät] (./media/active-directory-saas-rally-software-tutorial/IC781039.png "Käyttäjät")

3.  Kirjoita uuden käyttäjän tekstiruutu ja valitse sitten **Lisää tiedot**.

4.  Valitse **Luo käyttäjä** -osassa seuraavasti:

    ![Käyttäjän luominen] (./media/active-directory-saas-rally-software-tutorial/IC781040.png "Käyttäjän luominen")

    1.  Kirjoita **Käyttäjänimi** -tekstiruutuun haluat säätää Azure AD-käyttäjän nimi.
    2.  Kirjoita **Sähköpostiosoite** -tekstiruutuun haluat säätää Azure AD-käyttäjän sähköpostiosoite.
    3.  Valitse **Tallenna ja sulje**.

>[AZURE.NOTE]Voit käyttää mitä tahansa muita Rally ohjelmiston käyttäjän tilin luontityökalut tai ohjelmointirajapinnan myöntämä Rally ohjelmiston säännöstä AAD käyttäjätileille.

##<a name="assigning-users"></a>Käyttäjien määrittäminen
  
Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-rally-software-perform-the-following-steps"></a>Jos haluat määrittää käyttäjiä Rally ohjelmiston, toimi seuraavasti:

1.  Testaa tilin luominen Azure perinteinen portaalissa.

2.  Valitse **Rally ohjelmiston** sovelluksen integrointi-sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-rally-software-tutorial/IC769548.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Kyllä] (./media/active-directory-saas-rally-software-tutorial/IC767830.png "Kyllä")
  
Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli. Katso lisätietoja Access-paneelin [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).




