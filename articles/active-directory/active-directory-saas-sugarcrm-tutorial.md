<properties 
    pageTitle="Opetusohjelma: Azure Active Directory integrointi integrointi SugarCRM | Microsoft Azure" 
    description="Opettele käyttämään SugarCRM Azure Active Directory-hakemistosta käyttöön kertakirjautumisen, automaattinen valmistelu ja lisää!" 
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
    ms.date="09/11/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-integration-with-sugarcrm"></a>Opetusohjelma: Azure Active Directory integrointi SugarCRM integrointi
  
Tässä opetusohjelmassa tavoitteena on näyttämään Azure ja sokerin CRM integrointi.  
Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   Käytössä sokerin CRM kertakirjautumisen tilaus
  
Kun olet suorittanut Tässä opetusohjelmassa, olet liittänyt sokerin CRM Azure AD-käyttäjien saa oikeuden kertakirjauksen sokerin CRM yrityksen sivuston (palveluntarjoajan aloittanut allekirjoitus-) tai käyttämällä [Access-paneelin esittely](active-directory-saas-access-panel-introduction.md)-sovellukseen.
  
Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1.  Sovelluksen integrointi sokerin CRM ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Määrittäminen käyttäjän valmistelu
4.  Käyttäjien määrittäminen

![Skenaario] (./media/active-directory-saas-sugarcrm-tutorial/IC795881.png "Skenaario")

##<a name="enabling-the-application-integration-for-sugar-crm"></a>Sovelluksen integrointi sokerin CRM ottaminen käyttöön
  
Tässä osassa tavoitteena on Jäsennys, miten voit ottaa käyttöön sovelluksen integrointi sokerin CRM.

###<a name="to-enable-the-application-integration-for-sugar-crm-perform-the-following-steps"></a>Ottaa käyttöön sovelluksen integroinnin sokerin CRM, toimi seuraavasti:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-sugarcrm-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-sugarcrm-tutorial/IC700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-sugarcrm-tutorial/IC749321.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-sugarcrm-tutorial/IC749322.png "Lisää sovellus-gallerry")

6.  Kirjoita **hakuruutuun** **Sokerin CRM**.

    ![Sovelluksen valikoima] (./media/active-directory-saas-sugarcrm-tutorial/IC795882.png "Sovelluksen valikoima")

7.  Valitse tulosruudussa **Sokerin CRM**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![Sokerin CRM] (./media/active-directory-saas-sugarcrm-tutorial/IC795883.png "Sokerin CRM")

##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen
  
Tässä osassa tavoitteena on jäsentäminen siitä, miten käyttäjät voivat todentaa sokerin CRM käyttämällä SAML-protokollan perusteella federation Azure AD-tilin kanssa.  
Tämän toimintosarjan osana on base-64-koodattu varmenteen lataaminen sokerin CRM alihallintaan.  
Jos et ole tottunut näin, katso, [miten voit muuntaa binaariluvun varmenteen tekstitiedostoon](http://youtu.be/PlgrzUZ-Y1o)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Valitse Azure perinteinen portaalissa integrointi **Sokerin CRM** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-sugarcrm-tutorial/IC795884.png "Kertakirjautumisen määrittäminen")

2.  **Miten käyttäjät voivat kirjautua sokerin CRM haluat käyttää** sivulla Valitse **Microsoft Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-sugarcrm-tutorial/IC795885.png "Kertakirjautumisen määrittäminen")

3.  Kirjoita **Määritä sovelluksen URL-osoite** -sivulla **Sokerin CRM merkki-URL** -tekstiruutuun Sign sokerin CRM sovelluksen käyttäjille käyttämä URL-osoite (esimerkiksi: "*http://company.sugarondemand.com*", ja valitse sitten **Seuraava**.

    ![Sovelluksen URL-Osoitteen määrittäminen] (./media/active-directory-saas-sugarcrm-tutorial/IC795886.png "Sovelluksen URL-Osoitteen määrittäminen")

4.  Valitse **Määritä kertakirjautumisen osoitteessa sokerin CRM** -sivulla Lataa sertifikaatti, valitsemalla **Lataa**, ja Tallenna sertifikaattitiedosto tietokoneeseen.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-sugarcrm-tutorial/IC796918.png "Kertakirjautumisen määrittäminen")

5.  Eri selainikkunassa Kirjaudu sokerin CRM yrityksesi sivuston järjestelmänvalvojan oikeuksilla.

6.  Valitse **järjestelmänvalvoja**.

    ![Järjestelmänvalvoja] (./media/active-directory-saas-sugarcrm-tutorial/IC795888.png "Järjestelmänvalvoja")

7.  Valitse **hallinta** -kohdasta **Salasanojen hallinta**.

    ![Hallinta] (./media/active-directory-saas-sugarcrm-tutorial/IC795889.png "Hallinta")

8.  Valitse **Ota käyttöön SAML todennusta**.

    ![Hallinta] (./media/active-directory-saas-sugarcrm-tutorial/IC795890.png "Hallinta")

9.  **SAML todentaminen** ‑osassa toimi seuraavasti:

    ![SAML-todennus] (./media/active-directory-saas-sugarcrm-tutorial/IC795891.png "SAML-todennus")

    1.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa sokerin CRM** -valintasivun **Remote sisäänkirjautumisen URL-osoite** -arvo kopioi ja liitä se sitten **Kirjautuminen URL** -tekstiruutuun.
    2.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa sokerin CRM** -valintasivun **Remote sisäänkirjautumisen URL-osoite** -arvo kopioi ja liitä se sitten **Hitaissa URL** -tekstiruutuun.
    3.  Luo **Base-64-koodattu** tiedosto ladattu varmennetta.

        >[AZURE.TIP] Lisätietoja on artikkelissa [muuntaa binaariluvun varmenteen tekstitiedostoon](http://youtu.be/PlgrzUZ-Y1o)

    4.  Avaa base-64-koodattu varmennetta Muistiossa, kopioi se sisällön Leikepöydän ja liitä koko sertifikaatin **X.509-varmenne** tekstiruutu.
    5.  Valitse **Tallenna**.

10. Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa sokerin CRM** -valintasivun Valitse Sign määritysten vahvistus ja valitse sitten **Valmis**.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-sugarcrm-tutorial/IC796919.png "Kertakirjautumisen määrittäminen")

##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu
  
Jos haluat ottaa käyttöön Azure AD-käyttäjät voivat kirjautua sokerin CRM-järjestelmään, ne on valmisteltava sokerin CRM.  
Sokerin CRM valmistelu on manuaalinen tehtävä.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Valmistelu käyttäjätilit, toimi seuraavasti:

1.  Kirjaudu sisään **Sokerin CRM** yrityksesi sivuston järjestelmänvalvojana.

2.  Valitse **järjestelmänvalvoja**.

    ![Järjestelmänvalvoja] (./media/active-directory-saas-sugarcrm-tutorial/IC795888.png "Järjestelmänvalvoja")

3.  Valitse **hallinta** -kohdasta **Käyttäjien hallinta**.

    ![Hallinta] (./media/active-directory-saas-sugarcrm-tutorial/IC795893.png "Hallinta")

4.  Siirry **käyttäjien \> Luo uusi käyttäjä**.

    ![Luo uusi käyttäjä] (./media/active-directory-saas-sugarcrm-tutorial/IC795894.png "Luo uusi käyttäjä")

5.  **Käyttäjäprofiilin** -välilehden toimimalla seuraavasti:

    ![Uusi käyttäjä] (./media/active-directory-saas-sugarcrm-tutorial/IC795895.png "Uusi käyttäjä")

    1.  Kirjoita käyttäjänimi, viimeisen nimi ja sähköpostiosoite kelvollinen Azure Active Directory-käyttäjän liittyvät tekstiruutuja.

6.  Valitse **aktiivinen** **tila**.

7.  Valitse salasana-välilehden toimimalla seuraavasti:

    ![Uusi käyttäjä] (./media/active-directory-saas-sugarcrm-tutorial/IC795896.png "Uusi käyttäjä")

    1.  Kirjoita salasana liittyvät tekstiruutu.
    2.  Valitse **Tallenna**.

>[AZURE.NOTE] Voit käyttää mitä tahansa muita sokerin CRM käyttäjän tilin luontityökalut tai ohjelmointirajapinnan myöntämä sokerin CRM säännöstä AAD käyttäjätileille.

##<a name="assigning-users"></a>Käyttäjien määrittäminen
  
Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-sugar-crm-perform-the-following-steps"></a>Jos haluat määrittää käyttäjiä sokerin CRM, toimi seuraavasti:

1.  Testaa tilin luominen Azure perinteinen portaalissa.

2.  Valitse integration **Sokerin CRM** -sovellus-sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-sugarcrm-tutorial/IC795897.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Kyllä] (./media/active-directory-saas-sugarcrm-tutorial/IC767830.png "Kyllä")
  
Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli. Katso lisätietoja Access-paneelin [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).