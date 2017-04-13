<properties 
    pageTitle="Opetusohjelma: Uuden Relic Azure Active Directory-integrointi | Microsoft Azure" 
    description="Opettele käyttämään uuden Relic Azure Active Directory-hakemistosta käyttöön kertakirjautumisen, automaattinen valmistelu ja lisää!" 
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

#<a name="tutorial-azure-active-directory-integration-with-new-relic"></a>Opetusohjelma: Uuden Relic Azure Active Directory-integrointi
  
Tässä opetusohjelmassa tavoitteena on näyttää, miten voit määrittää kertakirjautumisen Azure Active Directory- ja uusi Relic välillä.
  
Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   Käytössä uusi Relic kertakirjautumisen tilaus
  
Kun olet suorittanut Tässä opetusohjelmassa, olet määrittänyt uudet Relic Azure Active Directory-käyttäjät saa oikeuden kertakirjautumisen käyttämällä AAD Access-paneeli.

1.  Uusi Relic sovelluksen integrointi ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Määrittäminen käyttäjän valmistelu
4.  Käyttäjien määrittäminen

![Skenaario] (./media/active-directory-saas-new-relic-tutorial/IC797030.png "Skenaario")
##<a name="enabling-the-application-integration-for-new-relic"></a>Uusi Relic sovelluksen integrointi ottaminen käyttöön
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön uuden Relic sovelluksen integrointi.

###<a name="to-enable-the-application-integration-for-new-relic-perform-the-following-steps"></a>Uusi Relic sovelluksen integrointi käyttöön toimimalla seuraavasti:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-new-relic-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-new-relic-tutorial/IC700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-new-relic-tutorial/IC749321.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-new-relic-tutorial/IC749322.png "Lisää sovellus-gallerry")

6.  Kirjoita **Etsi-ruutuun** **Uuden Relic**.

    ![Sovelluksen valikoima] (./media/active-directory-saas-new-relic-tutorial/IC797031.png "Sovelluksen valikoima")

7.  Tulosruudussa Valitse **Uusi Relic**ja valitse sitten **Valmis** , voit lisätä sovelluksen.

    ![Uusi Relic] (./media/active-directory-saas-new-relic-tutorial/IC797032.png "Uusi Relic")
##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen
  
Tässä osassa kuvataan antaa käyttäjien todentamiseen uusi Relic niiden tilillä Azure Active Directory federation perusteella SAML-protokollan avulla.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Valitse Azure perinteinen portaalissa integrointi **Relic uusi** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-new-relic-tutorial/IC769534.png "Määritä kertakirjautuminen")

2.  **Miten käyttäjät voivat kirjautua uuden Relic haluat käyttää** sivulla Valitse **Microsoft Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-new-relic-tutorial/IC797033.png "Kertakirjautumisen määrittäminen")

3.  **Määritä sovelluksen URL-osoite** -sivulla **Uusi Relic merkki-URL** -tekstiruutuun käyttää käyttäjien kirjautua uuden Relic sovelluksen URL-osoite ja valitse sitten **Seuraava**. 

    Sovelluksen URL-osoite on uusi Relic vuokraajan URL-osoite (esimerkiksi: *https://rpm.newrelic.com*):

    ![Sovelluksen URL-Osoitteen määrittäminen] (./media/active-directory-saas-new-relic-tutorial/IC797034.png "Sovelluksen URL-Osoitteen määrittäminen")

4.  Valitse **Määritä kertakirjautumisen osoitteessa uusi Relic** -sivulla Lataa sertifikaatti, **Lataa**ja Tallenna sertifikaattitiedosto paikallisesti tietokoneeseen.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-new-relic-tutorial/IC797035.png "Kertakirjautumisen määrittäminen")

5.  Eri selainikkunassa Kirjaudu **Uusi Relic** yritys-sivustoon järjestelmänvalvojana.

6.  Valitse ylä-valikossa **Tiliasetukset**.

    ![Tilin asetukset] (./media/active-directory-saas-new-relic-tutorial/IC797036.png "Tilin asetukset")

7.  Valitse **Suojaus- ja** -välilehti ja valitse sitten **kertakirjautuminen** -välilehti.

    ![Kertakirjautuminen] (./media/active-directory-saas-new-relic-tutorial/IC797037.png "Kertakirjautuminen")

8.  Suorita SAML-sivulla seuraavat toimet:

    ![SAML] (./media/active-directory-saas-new-relic-tutorial/IC797038.png "SAML")

    1.  Valitse **Tiedosto** lataaminen ladatut Azure Active Directory-sertifikaatti.
    2.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa uusi Relic** -sivulla **Remote sisäänkirjautumisen URL-osoite** -arvo kopioi ja liitä se sitten **Remote kirjautuminen URL** -tekstiruutuun.
    3.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa uusi Relic** -sivulla **Remote Kirjaudu URL-osoite** -arvo kopioi ja liitä se sitten **Kirjaudu ulos purkamisen URL** -tekstiruutuun.
    4.  Valitse **Tallenna muutokset**.

9.  Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Valmis** **Määrittäminen kertakirjautuminen** -valintaikkunassa Sulje.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-new-relic-tutorial/IC797039.png "Kertakirjautumisen määrittäminen")
##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu
  
Jotta voit kirjautua uuden Relic Azure Active Directory käyttäjät, ne on valmisteltava tuominen uuteen Relic.  
Jos kyseessä on uusi Relic valmistelu on manuaalinen tehtävä.

###<a name="to-provision-a-user-account-to-new-relic-perform-the-following-steps"></a>Valmistelu uusi Relic käyttäjätili, toimi seuraavasti:

1.  Kirjaudu sisään **Uuden Relic** yrityksesi sivuston järjestelmänvalvojana.

2.  Valitse ylä-valikossa **Tiliasetukset**.

    ![Tilin asetukset] (./media/active-directory-saas-new-relic-tutorial/IC797040.png "Tilin asetukset")

3.  Valitse vasemmalla puolella **tili** -ruudussa **Yhteenveto**ja valitse sitten **Lisää käyttäjä**.

    ![Tilin asetukset] (./media/active-directory-saas-new-relic-tutorial/IC797041.png "Tilin asetukset")

4.  Valitse **Aktiiviset käyttäjät** -valintaikkunassa seuraavasti:

    ![Aktiiviset käyttäjät] (./media/active-directory-saas-new-relic-tutorial/IC797042.png "Aktiiviset käyttäjät")

    1.  Kirjoita **sähköpostiosoite** -tekstiruutuun haluat säätää kelvollinen Azure Active Directory-käyttäjän sähköpostiosoite.
    2.  Valitse **käyttäjän** **rooli** .
    3.  Valitse **Lisää käyttäjä**.

>[AZURE.NOTE]Voit käyttää mitä tahansa muita uusia Relic käyttäjän tilin luontityökalut tai ohjelmointirajapinnan myöntämä uusi Relic säännöstä AAD käyttäjätileille.

##<a name="assigning-users"></a>Käyttäjien määrittäminen
  
Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-new-relic-perform-the-following-steps"></a>Jos haluat määrittää käyttäjät uuteen Relic, toimi seuraavasti:

1.  Testaa tilin luominen Azure perinteinen portaalissa.

2.  Valitse **Uusi Relic** sovelluksen integrointi-sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-new-relic-tutorial/IC797043.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Kyllä] (./media/active-directory-saas-new-relic-tutorial/IC767830.png "Kyllä")
  
Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli. Katso lisätietoja Access-paneelin [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).




