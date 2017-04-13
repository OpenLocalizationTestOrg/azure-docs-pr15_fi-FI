<properties 
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi e Builder | Microsoft Azure" 
    description="Opettele käyttämään e muodostin Azure Active Directory-hakemistosta käyttöön kertakirjautumisen, automaattinen valmistelu ja lisää!" 
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

#<a name="tutorial-azure-active-directory-integration-with-e-builder"></a>Opetusohjelma: E muodostin Azure Active Directory-integrointi
  
Tässä opetusohjelmassa tavoitteena on näyttämään Azure ja e muodostin.  
Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   E Builder-vuokraajan
  
Kun olet suorittanut Tässä opetusohjelmassa, olet määrittänyt e Builder Azure AD-käyttäjien saa oikeuden kertakirjauksen e muodostin yrityksesi sivuston (service provider aloittanut allekirjoitus-) tai käyttämällä [Access-paneelin esittely](active-directory-saas-access-panel-introduction.md)-sovellukseen.
  
Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1.  Sovelluksen integrointi e Builderin ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Määrittäminen käyttäjän valmistelu
4.  Käyttäjien määrittäminen

![Skenaario] (./media/active-directory-saas-e-builder-tutorial/IC777378.png "Skenaario")
##<a name="enabling-the-application-integration-for-e-builder"></a>Sovelluksen integrointi e Builderin ottaminen käyttöön
  
Tässä osassa tavoitteena on Jäsennys, miten voit ottaa käyttöön sovelluksen integrointi e Builderin.

###<a name="to-enable-the-application-integration-for-e-builder-perform-the-following-steps"></a>Ottaa käyttöön sovelluksen integroinnin e Builderin, toimi seuraavasti:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-e-builder-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-e-builder-tutorial/IC700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-e-builder-tutorial/IC749321.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-e-builder-tutorial/IC749322.png "Lisää sovellus-gallerry")

6.  Kirjoita **hakuruutuun**, **E-muodostin**.

    ![Sovelluksen valikoima] (./media/active-directory-saas-e-builder-tutorial/IC777379.png "Sovelluksen valikoima")

7.  Valitse tulokset-ruudun Valitse **E muodostin**ja valitse sitten **Valmis** , voit lisätä sovelluksen.

    ![E muodostin] (./media/active-directory-saas-e-builder-tutorial/IC777380.png "E muodostin")
##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjien todentamiseen e-muodostin ja niiden tilin käyttämällä SAML-protokollan perusteella federation Azure AD.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Valitse Azure perinteinen portaalissa integrointi **E muodostin** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-e-builder-tutorial/IC777381.png "Määritä kertakirjautuminen")

2.  **Miten käyttäjät voivat kirjautua e muodostin haluat käyttää** sivulla Valitse **Microsoft Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-e-builder-tutorial/IC777382.png "Määritä kertakirjautuminen")

3.  Kirjoita **Määritä sovelluksen URL-osoite** -sivulla **e muodostin kirjautuminen URL** -tekstiruutuun URL-osoite käyttämällä seuraavaa kaavaa "*https://\<vuokraajan nimi\>.e Builder.com*", ja valitse sitten **Seuraava**.

    ![Sovelluksen URL-Osoitteen määrittäminen] (./media/active-directory-saas-e-builder-tutorial/IC777383.png "Sovelluksen URL-Osoitteen määrittäminen")

4.  Valitse **Määritä kertakirjautumisen osoitteessa e muodostin** -sivulla Lataa metatietojen, valitse **Lataa metatiedot**ja sitten tiedot tiedosto paikallisesti nimellä **c:\\e BuilderMetaData.xml**.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-e-builder-tutorial/IC777384.png "Määritä kertakirjautuminen")

5.  Välitä metatietojen tiedoston e Builder tekniseen tukeen. Tuen työryhmän tarpeiden määrittää kertakirjautumisen puolestasi.

6.  Valitse Sign määritysten vahvistus ja valitse sitten **Valmis** **Määrittäminen kertakirjautuminen** -valintaikkunassa Sulje.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-e-builder-tutorial/IC777385.png "Määritä kertakirjautuminen")
##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu
  
Ei ei toimi, voit määrittää käyttäjän valmistelu e muodostin.  
Kun määritetty käyttäjä yrittää kirjautua muodostimen käyttäminen access-paneelin e, e-muodostin tarkistaa käyttäjä on.  
Jos ei ole käyttäjätiliä käytettävissä vielä, se luodaan automaattisesti e muodostimen avulla.
##<a name="assigning-users"></a>Käyttäjien määrittäminen
  
Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-e-builder-perform-the-following-steps"></a>Jos haluat määrittää käyttäjiä e muodostin, toimi seuraavasti:

1.  Testaa tilin luominen Azure perinteinen portaalissa.

2.  Valitse integration **e Builder **-sovellus-sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-e-builder-tutorial/IC777386.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Kyllä] (./media/active-directory-saas-e-builder-tutorial/IC767830.png "Kyllä")
  
Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli. Katso lisätietoja Access-paneelin [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).
