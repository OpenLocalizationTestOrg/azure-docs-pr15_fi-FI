<properties 
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi Sciforma | Microsoft Azure" 
    description="Opettele käyttämään Sciforma Azure Active Directory-hakemistosta käyttöön kertakirjautumisen, automaattinen valmistelu ja lisää!" 
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

#<a name="tutorial-azure-ad-integration-with-sciforma"></a>Opetusohjelma: Sciforma Azure AD-integrointi
  
Tässä opetusohjelmassa tavoitteena on näyttämään Azure ja Sciforma integrointi.  
Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   Sciforma palvelutili
  
Kun olet suorittanut Tässä opetusohjelmassa, olet liittänyt Sciforma Azure AD-käyttäjien saa oikeuden kertakirjauksen Sciforma yrityksen sivuston (palveluntarjoajan aloittanut allekirjoitus-) tai käyttämällä [Access-paneelin esittely](active-directory-saas-access-panel-introduction.md)-sovellukseen.
  
Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1.  Sciforma sovelluksen integrointi ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Määrittäminen käyttäjän valmistelu
4.  Käyttäjien määrittäminen

![Skenaario] (./media/active-directory-saas-sciforma-tutorial/IC777369.png "Skenaario")
##<a name="enabling-the-application-integration-for-sciforma"></a>Sciforma sovelluksen integrointi ottaminen käyttöön
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön Sciforma sovelluksen integrointi.

###<a name="to-enable-the-application-integration-for-sciforma-perform-the-following-steps"></a>Sciforma sovelluksen integrointi käyttöön toimimalla seuraavasti:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-sciforma-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-sciforma-tutorial/IC700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-sciforma-tutorial/IC749321.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-sciforma-tutorial/IC749322.png "Lisää sovellus-gallerry")

6.  Kirjoita **hakuruutuun** **Sciforma**.

    ![Sovelluksen valikoima] (./media/active-directory-saas-sciforma-tutorial/IC777370.png "Sovelluksen valikoima")

7.  Valitse tulosruudussa **Sciforma**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![Sciforma] (./media/active-directory-saas-sciforma-tutorial/IC777371.png "Sciforma")
##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjien todentamiseen Sciforma käyttämällä SAML-protokollan perusteella federation Azure AD-tilin kanssa.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Valitse Azure perinteinen portaalissa integrointi **Sciforma** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-sciforma-tutorial/IC777372.png "Määritä kertakirjautuminen")

2.  **Miten käyttäjät voivat kirjautua Sciforma haluat käyttää** sivulla Valitse **Microsoft Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-sciforma-tutorial/IC777373.png "Määritä kertakirjautuminen")

3.  Kirjoita **Määritä sovelluksen URL-osoite** -sivulla **Sciforma merkki-URL** -tekstiruutuun URL-osoite käyttämällä seuraavaa kaavaa "*https://\<vuokraajan nimi\>. Sciforma.com*", ja valitse sitten **Seuraava**.

    ![Sovelluksen URL-Osoitteen määrittäminen] (./media/active-directory-saas-sciforma-tutorial/IC777374.png "Sovelluksen URL-Osoitteen määrittäminen")

4.  Valitse **Määritä kertakirjautumisen osoitteessa Sciforma** -sivulla Lataa metatietojen, valitse **Lataa metatiedot**ja sitten tiedot tiedosto paikallisesti nimellä **c:\\SciformaMetaData.xml**.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-sciforma-tutorial/IC777375.png "Määritä kertakirjautuminen")

5.  Välitä Sciforma metatietojen kyseinen tiedosto tekniseen tukeen. Tuen työryhmän tarpeiden määrittää kertakirjautumisen puolestasi.

6.  Valitse Sign määritysten vahvistus ja valitse sitten **Valmis** **Määrittäminen kertakirjautuminen** -valintaikkunassa Sulje.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-sciforma-tutorial/IC777376.png "Määritä kertakirjautuminen")
##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu
  
Ei ei toimi, voit määrittää käyttäjän valmistelu Sciforma.  
Kun määritetty käyttäjä yrittää kirjautua käyttämällä access-paneelin Sciforma, Sciforma tarkistaa, onko käyttäjän olemassa.  
Jos ei ole käyttäjätiliä käytettävissä vielä, se luodaan automaattisesti Sciforma mukaan.
##<a name="assigning-users"></a>Käyttäjien määrittäminen
  
Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-sciforma-perform-the-following-steps"></a>Jos haluat määrittää käyttäjiä Sciforma, toimi seuraavasti:

1.  Testaa tilin luominen Azure perinteinen portaalissa.

2.  Valitse integration **Sciforma **sovellus-sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-sciforma-tutorial/IC777377.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Kyllä] (./media/active-directory-saas-sciforma-tutorial/IC767830.png "Kyllä")
  
Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli. Katso lisätietoja Access-paneelin [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).