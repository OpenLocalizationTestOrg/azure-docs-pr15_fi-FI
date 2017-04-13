<properties 
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi ArcGIS | Microsoft Azure" 
    description="Opettele käyttämään ArcGIS Azure Active Directory-hakemistosta käyttöön kertakirjautumisen, automaattinen valmistelu ja lisää!" 
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

#<a name="tutorial-azure-active-directory-integration-with-arcgis"></a>Opetusohjelma: ArcGIS Azure Active Directory-integrointi

Tässä opetusohjelmassa tavoitteena on näyttämään Azure ja ArcGIS integrointi. Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   Käytössä ArcGIS kertakirjautumisen tilaus

Kun olet suorittanut Tässä opetusohjelmassa, olet liittänyt ArcGIS Azure AD-käyttäjien saa oikeuden kertakirjauksen ArcGIS yrityksesi sivuston (palveluntarjoajan aloittanut allekirjoitus-) tai käyttämällä [Access-paneelin esittely](active-directory-saas-access-panel-introduction.md)-sovellukseen.

Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1.  ArcGIS sovelluksen integrointi ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Määrittäminen käyttäjän valmistelu
4.  Käyttäjien määrittäminen

![Skenaario] (./media/active-directory-saas-arcgis-tutorial/IC784735.png "Skenaario")
##<a name="enabling-the-application-integration-for-arcgis"></a>ArcGIS sovelluksen integrointi ottaminen käyttöön

Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön ArcGIS sovelluksen integrointi.

###<a name="to-enable-the-application-integration-for-arcgis-perform-the-following-steps"></a>ArcGIS sovelluksen integrointi käyttöön toimimalla seuraavasti:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-arcgis-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-arcgis-tutorial/IC700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-arcgis-tutorial/IC749321.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-arcgis-tutorial/IC749322.png "Lisää sovellus-gallerry")

6.  Kirjoita **hakuruutuun** **ArcGIS**.

    ![Applcation valikoima] (./media/active-directory-saas-arcgis-tutorial/IC784736.png "Applcation valikoima")

7.  Valitse tulosruudussa **ArcGIS**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![ArcGIS] (./media/active-directory-saas-arcgis-tutorial/IC784737.png "ArcGIS")
##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen

Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjien todentamiseen ArcGIS käyttämällä SAML-protokollan perusteella federation Azure AD-tilin kanssa.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Valitse Azure perinteinen portaalissa integrointi **ArcGIS** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-arcgis-tutorial/IC784738.png "Kertakirjautumisen määrittäminen")

2.  **Miten käyttäjät voivat kirjautua ArcGIS haluat käyttää** sivulla Valitse **Microsoft Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-arcgis-tutorial/IC784739.png "Kertakirjautumisen määrittäminen")

3.  **Määrittää sovelluksen URL-osoite** -sivulla **ArcGIS merkki-URL** -tekstiruutuun URL-Osoitteen tyyppi käyttävät käyttäjät Kirjaudu sisään käyttämällä seuraavaa kaavaa "*https://company.maps.arcgis.com*" ja valitse sitten **Seuraava**.

    ![Sovelluksen URL-Osoitteen määrittäminen] (./media/active-directory-saas-arcgis-tutorial/IC784740.png "Sovelluksen URL-Osoitteen määrittäminen")

4.  Valitse **Määritä kertakirjautumisen osoitteessa ArcGIS** -sivulla valitsemalla **Lataa metatiedot**ja Tallenna paikallisesti tietokoneen metatiedot-tiedostoon.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-arcgis-tutorial/IC784741.png "Kertakirjautumisen määrittäminen")

5.  Eri selainikkunassa Kirjaudu ArcGIS yrityksesi sivuston järjestelmänvalvojan oikeuksilla.

6.  Valitse **Muokkaa asetuksia**.

    ![Asetusten muokkaaminen] (./media/active-directory-saas-arcgis-tutorial/IC784742.png "Asetusten muokkaaminen")

7.  Valitse **Suojaus**.

    ![Tietoturva] (./media/active-directory-saas-arcgis-tutorial/IC784743.png "Tietoturva")

8.  Valitse **Yrityksen kirjautumiset**-kohdassa **Määrittäminen tunnistetietojen toimittaja**.

    ![Yrityksen kirjautumiset] (./media/active-directory-saas-arcgis-tutorial/IC784744.png "Yrityksen kirjautumiset")

9.  **Määritä tunnistetietojen toimittaja** määritys-sivulla toimimalla seuraavasti:

    ![Määritä tunnistetietojen toimittaja] (./media/active-directory-saas-arcgis-tutorial/IC784745.png "Määritä tunnistetietojen toimittaja")

    1.  Kirjoita nimi-tekstiruutuun organisaation nimeä.
    2.  **Enterprise-tunnistetietojen toimittaja-metatiedot toimitetaan käyttäen**Valitse **Samanniminen tiedosto**.
    3.  Voit ladata ladatut metatieto-tiedosto valitsemalla **Valitse tiedosto**.
    4.  Valitse **Määritä tunnistetietojen toimittaja**.

10. Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Valmis** **Määrittäminen kertakirjautuminen** -valintaikkunassa Sulje.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-arcgis-tutorial/IC784746.png "Kertakirjautumisen määrittäminen")
##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu

Jos haluat ottaa käyttöön Azure AD-käyttäjät voivat kirjautua sisään ArcGIS, ne on valmisteltava ArcGIS kyselyjä.  
ArcGIS valmistelu on manuaalinen tehtävä.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Voit määrittää käyttäjän valmistelu toimimalla seuraavasti:

1.  Kirjaudu sisään **ArcGIS** alihallintaan.

2.  Valitse **Kutsu jäsenet**.

    ![Kutsu jäsenet] (./media/active-directory-saas-arcgis-tutorial/IC784747.png "Kutsu jäsenet")

3.  Valitse **Lisää jäseniä automaattisesti, jota ei lähetetä sähköpostia**ja valitse sitten **Seuraava**.

    ![Jäsenten lisääminen automaattisesti] (./media/active-directory-saas-arcgis-tutorial/IC784748.png "Jäsenten lisääminen automaattisesti")

4.  Valitse **jäsenet** -sivulta toimimalla seuraavasti:

    ![Lisää ja tarkistus] (./media/active-directory-saas-arcgis-tutorial/IC784749.png "Lisää ja tarkistus")

    1.  Kirjoita **Etunimi**, **Sukunimi** ja **sähköpostin** kelvollinen AAD tilin haluat säätää.
    2.  Valitse **Lisää ja tarkastele**.

5.  Tarkista annetut tiedot ja valitse sitten **Lisää jäseniä**.

    ![Lisää jäsen] (./media/active-directory-saas-arcgis-tutorial/IC784750.png "Lisää jäsen")

>[AZURE.NOTE] Voit käyttää mitä tahansa muita ArcGIS käyttäjän tilin luontityökalut tai ohjelmointirajapinnan myöntämä ArcGIS säännöstä AAD käyttäjätileille.

##<a name="assigning-users"></a>Käyttäjien määrittäminen

Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-arcgis-perform-the-following-steps"></a>Jos haluat määrittää käyttäjiä ArcGIS, toimi seuraavasti:

1.  Testaa tilin luominen Azure perinteinen portaalissa.

2.  Valitse integration **ArcGIS **sovellus-sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-arcgis-tutorial/IC784751.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Kyllä] (./media/active-directory-saas-arcgis-tutorial/IC767830.png "Kyllä")

Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli. Katso lisätietoja Access-paneelin [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).
