<properties 
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi Intacct | Microsoft Azure" 
    description="Opettele käyttämään Intacct Azure Active Directory-hakemistosta käyttöön kertakirjautumisen, automaattinen valmistelu ja lisää!" 
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

#<a name="tutorial-azure-active-directory-integration-with-intacct"></a>Opetusohjelma: Intacct Azure Active Directory-integrointi
  
Tässä opetusohjelmassa tavoitteena on näyttämään Azure ja Intacct integrointi.  
Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   Intacct palvelutili
  
Kun olet suorittanut Tässä opetusohjelmassa, olet liittänyt Intacct Azure AD-käyttäjien saa oikeuden kertakirjauksen Intacct yrityksesi sivuston (palveluntarjoajan aloittanut allekirjoitus-) tai käyttämällä [Access-paneelin esittely](active-directory-saas-access-panel-introduction.md)-sovellukseen.
  
Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1.  Intacct sovelluksen integrointi ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Määrittäminen käyttäjän valmistelu
4.  Käyttäjien määrittäminen

![Skenaario] (./media/active-directory-saas-intacct-tutorial/IC790030.png "Skenaario")
##<a name="enabling-the-application-integration-for-intacct"></a>Intacct sovelluksen integrointi ottaminen käyttöön
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön Intacct sovelluksen integrointi.

###<a name="to-enable-the-application-integration-for-intacct-perform-the-following-steps"></a>Intacct sovelluksen integrointi käyttöön toimimalla seuraavasti:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-intacct-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-intacct-tutorial/IC700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-intacct-tutorial/IC749321.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-intacct-tutorial/IC749322.png "Lisää sovellus-gallerry")

6.  Kirjoita **hakuruutuun** **Intacct**.

    ![Sovelluksen valikoima] (./media/active-directory-saas-intacct-tutorial/IC790031.png "Sovelluksen valikoima")

7.  Valitse tulosruudussa **Intacct**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![Intacct] (./media/active-directory-saas-intacct-tutorial/IC790032.png "Intacct")
##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjien todentamiseen Intacct käyttämällä SAML-protokollan perusteella federation Azure AD-tilin kanssa.  
Tämän toimintosarjan osana on base-64-koodattu sertifikaattitiedosto luomiseen.  
Jos et ole tottunut näin, katso, [miten voit muuntaa binaariluvun varmenteen tekstitiedostoon](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Valitse Azure perinteinen portaalissa integrointi **Intacct** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-intacct-tutorial/IC790033.png "Kertakirjautumisen määrittäminen")

2.  **Miten käyttäjät voivat kirjautua Intacct haluat käyttää** sivulla Valitse **Microsoft Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-intacct-tutorial/IC790034.png "Kertakirjautumisen määrittäminen")

3.  **Määritä sovelluksen URL-osoite** -sivulla **Intacct merkki-URL** -tekstiruutuun Kirjoita käyttämällä seuraavaa kaavaa "*https://Intacct.com/company*" URL-osoite ja valitse sitten **Seuraava**.

    ![Sovelluksen URL-Osoitteen määrittäminen] (./media/active-directory-saas-intacct-tutorial/IC790035.png "Sovelluksen URL-Osoitteen määrittäminen")

4.  Valitse **Määritä kertakirjautumisen osoitteessa Intacct** -sivulla valitsemalla **Lataa**ja Tallenna sertifikaattitiedosto tietokoneeseen.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-intacct-tutorial/IC790036.png "Kertakirjautumisen määrittäminen")

5.  Eri selainikkunassa Kirjaudu Intacct yrityksen sivuston järjestelmänvalvojan oikeuksilla.

6.  **Yritys** -välilehti ja valitse sitten **Yrityksen tiedot**.

    ![Yrityksen] (./media/active-directory-saas-intacct-tutorial/IC790037.png "Yrityksen")

7.  Valitse **Suojaus** -välilehti ja valitse sitten **Muokkaa**.

    ![Tietoturva] (./media/active-directory-saas-intacct-tutorial/IC790038.png "Tietoturva")

8.  **Kertakirjautuminen (SSO)** -osassa seuraavasti:

    ![Kertakirjautuminen] (./media/active-directory-saas-intacct-tutorial/IC790039.png "Kertakirjautuminen")

    1.  Valitse **Kertakirjauksen käyttöön**.
    2.  **Käyttäjätietojen tarjoajan tyyppiä**Valitse **SAML 2.0**.
    3.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa Intacct** -valintasivun **Myöntäjä URL-osoite** -arvo kopioi ja liitä se sitten **Myöntäjä URL** -tekstiruutuun.
    4.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa Intacct** -valintasivun **Remote sisäänkirjautumisen URL-osoite** -arvo kopioi ja liitä se sitten **Kirjautuminen URL** -tekstiruutuun.
    5.  Luo **base-64-koodattu** tiedosto ladattu varmennetta.
        
        >[AZURE.TIP]Lisätietoja on artikkelissa [muuntaa binaariluvun varmenteen tekstitiedostoon](http://youtu.be/PlgrzUZ-Y1o)

    6.  Avaa base-64-koodattu varmennetta Muistiossa, kopioi se sisällön Leikepöydän ja liitä se **varmenne** -tekstiruutu
    7.  Valitse **Tallenna**.

9.  Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Valmis** **Määrittäminen kertakirjautuminen** -valintaikkunassa Sulje.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-intacct-tutorial/IC790040.png "Kertakirjautumisen määrittäminen")
##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu
  
Jos haluat ottaa käyttöön Azure AD-käyttäjät voivat kirjautua sisään Intacct, ne on valmisteltava Intacct kyselyjä.  
Intacct valmistelu on manuaalinen tehtävä.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Valmistelu käyttäjätilit, toimi seuraavasti:

1.  Kirjaudu sisään **Intacct** alihallintaan.

2.  **Yritys** -välilehti ja valitse sitten **käyttäjät**.

    ![Käyttäjät] (./media/active-directory-saas-intacct-tutorial/IC790041.png "Käyttäjät")

3.  Valitse **Lisää** -välilehti

    ![Lisää] (./media/active-directory-saas-intacct-tutorial/IC790042.png "Lisää")

4.  **Käyttäjätiedot** -osassa seuraavasti:

    ![Käyttäjätiedot] (./media/active-directory-saas-intacct-tutorial/IC790043.png "Käyttäjätiedot")

    1.  Kirjoita **Käyttäjätunnus**, **Sukunimi**, **Etunimi**, **sähköpostiosoitteen**, **otsikko** ja **puhelimen** Azure AD-tilin haluat säännöstä liittyvät tekstiruutujen kyselyjä.
    2.  Valitse tilin Azure AD-haluat säätää **järjestelmänvalvojan oikeuksia** .
    3.  Valitse **Tallenna**.
        
        >[AZURE.NOTE] AAD tilin omistaja vastaanottaa sähköpostia ja johtavan linkin Vahvista tilin, ennen kuin se on aktiivinen.

>[AZURE.NOTE] Voit käyttää mitä tahansa muita Intacct käyttäjän tilin luontityökalut tai ohjelmointirajapinnan myöntämä Intacct säännöstä AAD käyttäjätileille.

##<a name="assigning-users"></a>Käyttäjien määrittäminen
  
Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-intacct-perform-the-following-steps"></a>Jos haluat määrittää käyttäjiä Intacct, toimi seuraavasti:

1.  Testaa tilin luominen Azure perinteinen portaalissa.

2.  Valitse integration **Intacct **sovellus-sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-intacct-tutorial/IC790044.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Kyllä] (./media/active-directory-saas-intacct-tutorial/IC767830.png "Kyllä")
  
Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli. Katso lisätietoja Access-paneelin [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).