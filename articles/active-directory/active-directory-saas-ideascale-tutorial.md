<properties 
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi IdeaScale | Microsoft Azure" 
    description="Opettele käyttämään IdeaScale Azure Active Directory-hakemistosta käyttöön kertakirjautumisen, automaattinen valmistelu ja lisää!" 
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

#<a name="tutorial-azure-active-directory-integration-with-ideascale"></a>Opetusohjelma: IdeaScale Azure Active Directory-integrointi
  
Tässä opetusohjelmassa tavoitteena on näyttämään Azure ja IdeaScale integrointi.  
Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   Käytössä IdeaScale kertakirjautumisen tilaus
  
Kun olet suorittanut Tässä opetusohjelmassa, olet liittänyt IdeaScale Azure AD-käyttäjien saa oikeuden kertakirjauksen käyttämällä [Access-paneelin esittely](active-directory-saas-access-panel-introduction.md)-sovellukseen.
  
Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1.  IdeaScale sovelluksen integrointi ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Määrittäminen käyttäjän valmistelu
4.  Käyttäjien määrittäminen

![Skenaario] (./media/active-directory-saas-ideascale-tutorial/IC790838.png "Skenaario")
##<a name="enabling-the-application-integration-for-ideascale"></a>IdeaScale sovelluksen integrointi ottaminen käyttöön
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön IdeaScale sovelluksen integrointi.

###<a name="to-enable-the-application-integration-for-ideascale-perform-the-following-steps"></a>IdeaScale sovelluksen integrointi käyttöön toimimalla seuraavasti:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-ideascale-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-ideascale-tutorial/IC700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-ideascale-tutorial/IC749321.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-ideascale-tutorial/IC749322.png "Lisää sovellus-gallerry")

6.  Kirjoita **hakuruutuun** **IdeaScale**.

    ![Sovelluksen valikoima] (./media/active-directory-saas-ideascale-tutorial/IC790841.png "Sovelluksen valikoima")

7.  Valitse tulosruudussa **IdeaScale**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![IdeaScale] (./media/active-directory-saas-ideascale-tutorial/IC790842.png "IdeaScale")
##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjien todentamiseen IdeaScale käyttämällä SAML-protokollan perusteella federation Azure AD-tilin kanssa.  
Kertakirjautumisen varten IdeaScale määrittäminen edellyttää, että allekirjoitus arvon noutaa varmenne.  
Jos et ole tottunut näin, katso, [miten voit hakea varmenteen allekirjoitus-arvon](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Valitse Azure perinteinen portaalissa integrointi **IdeaScale** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-ideascale-tutorial/IC790843.png "Kertakirjautumisen määrittäminen")

2.  **Miten käyttäjät voivat kirjautua IdeaScale haluat käyttää** sivulla Valitse **Microsoft Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-ideascale-tutorial/IC790844.png "Kertakirjautumisen määrittäminen")

3.  Kirjoita **Määritä sovelluksen URL-osoite** -sivulla **IdeaScale merkki-URL** -tekstiruutuun käyttämä käyttäjien kirjautua IdeaScale sovelluksen URL-osoite (esimerkiksi: "*https://company.IdeaScale.com*"), ja valitse sitten **Seuraava**.

    ![Sovelluksen URL-Osoitteen määrittäminen] (./media/active-directory-saas-ideascale-tutorial/IC790845.png "Sovelluksen URL-Osoitteen määrittäminen")

4.  Valitse **Määritä kertakirjautumisen osoitteessa IdeaScale** -sivulla lataamaan metatietojen valitsemalla **Lataa metatietojen**, ja Tallenna paikallisesti tietokoneen metatiedot-tiedostoon.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-ideascale-tutorial/IC790846.png "Kertakirjautumisen määrittäminen")

5.  Eri selainikkunassa Kirjaudu IdeaScale yrityksesi sivuston järjestelmänvalvojan oikeuksilla.

6.  Valitse **yhteisön asetukset**.

    ![Yhteisön asetukset] (./media/active-directory-saas-ideascale-tutorial/IC790847.png "Yhteisön asetukset")

7.  Siirry **suojauksen \> yksittäinen rekisteröityminen asetukset**.

    ![Yksittäisen rekisteröityminen asetukset] (./media/active-directory-saas-ideascale-tutorial/IC790848.png "Yksittäisen rekisteröityminen asetukset")

8.  **Yhden rekisteröityminen tyyppi**Valitse **SAML 2.0**.

    ![Yksittäisen rekisteröityminen tyyppi] (./media/active-directory-saas-ideascale-tutorial/IC790849.png "Yksittäisen rekisteröityminen tyyppi")

9.  Valitse **Yksittäinen rekisteröityminen asetukset** -valintaikkunassa seuraavasti:

    ![Yksittäisen rekisteröityminen asetukset] (./media/active-directory-saas-ideascale-tutorial/IC790850.png "Yksittäisen rekisteröityminen asetukset")

    1.  Azure perinteinen portaalissa-sivulla **Määritä kertakirjautumisen osoitteessa IdeaScale** valintaikkunan Kopioi **Kohteen tunnus** -arvon ja liitä se sitten **SAML IdP kohteen tunnus** -tekstiruutuun.
    2.  Metatietojen ladatun tiedoston sisällön kopioiminen ja liittäminen **SAML IdP metatiedot** -tekstiruutuun.
    3.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa IdeaScale** -valintasivun **Remote Kirjaudu URL-osoite** -arvo kopioi ja liitä se sitten **Kirjaudu ulos Success URL** -tekstiruutuun.
    4.  Valitse **Tallenna muutokset**.

10. Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Valmis** **Määrittäminen kertakirjautuminen** -valintaikkunassa Sulje.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-ideascale-tutorial/IC790851.png "Kertakirjautumisen määrittäminen")
##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu
  
Jos haluat ottaa käyttöön Azure AD-käyttäjät voivat kirjautua sisään IdeaScale, ne on valmisteltava IdeaScale kyselyjä.  
IdeaScale valmistelu on manuaalinen tehtävä.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Voit määrittää käyttäjän valmistelu toimimalla seuraavasti:

1.  Kirjaudu sisään **IdeaScale** yrityksesi sivuston järjestelmänvalvojana.

2.  Valitse **yhteisön asetukset**.

    ![Yhteisön asetukset] (./media/active-directory-saas-ideascale-tutorial/IC790847.png "Yhteisön asetukset")

3.  Siirry **perusasetukset \> jäsenten hallinta**.

4.  Valitse **Lisää jäsen**.

    ![Jäsenten hallinta] (./media/active-directory-saas-ideascale-tutorial/IC790852.png "Jäsenten hallinta")

5.  Lisää uusi jäsen-kohdassa toimimalla seuraavasti:

    ![Lisää uusi jäsen] (./media/active-directory-saas-ideascale-tutorial/IC790853.png "Lisää uusi jäsen")

    1.  Kirjoita sähköpostiosoite, jos haluat säätää kelvollinen AAD tilin **Sähköpostiosoitteet** -tekstiruutuun.
    2.  Valitse **Tallenna muutokset**.

    >[AZURE.NOTE] Azure Active Directory-tilin omistaja saavat sähköpostiviestin linkin Vahvista tili, ennen kuin se on aktiivinen.

>[AZURE.NOTE] Voit käyttää mitä tahansa muita IdeaScale käyttäjän tilin luontityökalut tai ohjelmointirajapinnan myöntämä IdeaScale säännöstä AAD käyttäjätileille.

##<a name="assigning-users"></a>Käyttäjien määrittäminen
  
Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-ideascale-perform-the-following-steps"></a>Jos haluat määrittää käyttäjiä IdeaScale, toimi seuraavasti:

1.  Testaa tilin luominen Azure perinteinen portaalissa.

2.  Valitse integration **IdeaScale **sovellus-sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-ideascale-tutorial/IC790854.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Kyllä] (./media/active-directory-saas-ideascale-tutorial/IC767830.png "Kyllä")
  
Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli. Katso lisätietoja Access-paneelin [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).