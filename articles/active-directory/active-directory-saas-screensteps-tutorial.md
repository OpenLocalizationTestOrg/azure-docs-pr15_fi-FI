<properties 
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi ScreenSteps | Microsoft Azure" 
    description="Opettele käyttämään ScreenSteps Azure Active Directory-hakemistosta käyttöön kertakirjautumisen, automaattinen valmistelu ja lisää!" 
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

#<a name="tutorial-azure-active-directory-integration-with-screensteps"></a>Opetusohjelma: ScreenSteps Azure Active Directory-integrointi
  
Tässä opetusohjelmassa tavoitteena on näyttämään Azure ja ScreenSteps integrointi.  
Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   ScreenSteps palvelutili
  
Kun olet suorittanut Tässä opetusohjelmassa, olet liittänyt ScreenSteps Azure AD-käyttäjien saa oikeuden kertakirjauksen ScreenSteps yrityksesi sivuston (palveluntarjoajan aloittanut allekirjoitus-) tai käyttämällä [Access-paneelin esittely](active-directory-saas-access-panel-introduction.md)-sovellukseen.
  
Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1.  ScreenSteps sovelluksen integrointi ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Määrittäminen käyttäjän valmistelu
4.  Käyttäjien määrittäminen

![Skenaario] (./media/active-directory-saas-screensteps-tutorial/IC778516.png "Skenaario")
##<a name="enabling-the-application-integration-for-screensteps"></a>ScreenSteps sovelluksen integrointi ottaminen käyttöön
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön ScreenSteps sovelluksen integrointi.

###<a name="to-enable-the-application-integration-for-screensteps-perform-the-following-steps"></a>ScreenSteps sovelluksen integrointi käyttöön toimimalla seuraavasti:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-screensteps-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-screensteps-tutorial/IC700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-screensteps-tutorial/IC749321.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-screensteps-tutorial/IC749322.png "Lisää sovellus-gallerry")

6.  Kirjoita **hakuruutuun** **ScreenSteps**.

    ![Sovelluksen valikoima] (./media/active-directory-saas-screensteps-tutorial/IC778517.png "Sovelluksen valikoima")

7.  Valitse tulosruudussa **ScreenSteps**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![ScreenSteps] (./media/active-directory-saas-screensteps-tutorial/IC778518.png "ScreenSteps")
##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjien todentamiseen ScreenSteps käyttämällä SAML-protokollan perusteella federation Azure AD-tilin kanssa.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Valitse Azure perinteinen portaalissa integrointi **ScreenSteps** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-screensteps-tutorial/IC778519.png "Määritä kertakirjautuminen")

2.  **Miten käyttäjät voivat kirjautua ScreenSteps haluat käyttää** sivulla Valitse **Microsoft Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-screensteps-tutorial/IC778520.png "Määritä kertakirjautuminen")

3.  Kirjoita **Määritä sovelluksen URL-osoite** -sivulla **ScreenSteps merkki-URL** -tekstiruutuun URL-osoite käyttämällä seuraavaa kaavaa "*https://\<vuokraajan nimi\>. ScreenSteps.com*", ja valitse sitten **Seuraava**.

    ![Sovelluksen URL-Osoitteen määrittäminen] (./media/active-directory-saas-screensteps-tutorial/IC778521.png "Sovelluksen URL-Osoitteen määrittäminen")

4.  Valitse **Määritä kertakirjautumisen osoitteessa ScreenSteps** -sivulla Lataa sertifikaatti, valitsemalla **Lataa**, ja Tallenna sertifikaattitiedosto tietokoneeseen.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-screensteps-tutorial/IC778522.png "Määritä kertakirjautuminen")

5.  Eri selainikkunassa Kirjaudu ScreenSteps yrityksesi sivuston järjestelmänvalvojan oikeuksilla.

6.  Valitse **tilinhallinta**.

    ![Tilinhallinta] (./media/active-directory-saas-screensteps-tutorial/IC778523.png "Tilinhallinta")

7.  Valitse **Remote todennusta**.

    ![Remote todennus] (./media/active-directory-saas-screensteps-tutorial/IC778524.png "Remote todennus")

8.  Valitse **Luo todennus päätepiste**.

    ![Remote todennus] (./media/active-directory-saas-screensteps-tutorial/IC778525.png "Remote todennus")

9.  **Luo päätepisteen todennus** -osassa seuraavasti:

    ![Luo päätepisteen todennus] (./media/active-directory-saas-screensteps-tutorial/IC778526.png "Luo päätepisteen todennus")

    1.  Kirjoita **otsikko** -tekstiruutuun otsikko.
    2.  **Tila** -luettelosta **SAML**.
    3.  Valitse **Luo**.

10. **Todennus päätepiste** -osassa seuraavasti:

    ![Päätepisteen Remote todennus] (./media/active-directory-saas-screensteps-tutorial/IC778527.png "Päätepisteen Remote todennus")

    1.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa ScreenSteps** -sivulla **Remote sisäänkirjautumisen URL-osoite** -arvo kopioi ja liitä se sitten **Remote kirjautuminen URL** -tekstiruutuun.
    2.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa ScreenSteps** -sivulla **Remote Kirjaudu URL-osoite** -arvo kopioi ja liitä se sitten **Kirjaudu ulos URL** -tekstiruutuun.
    3.  **Valitse tiedosto**ja lataa sitten ladattu varmenne.
    4.  Valitse **Päivitä**.

11. Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Valmis** **Määrittäminen kertakirjautuminen** -valintaikkunassa Sulje.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-screensteps-tutorial/IC778542.png "Määritä kertakirjautuminen")
##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu
  
Jos haluat ottaa käyttöön Azure AD-käyttäjät voivat kirjautua sisään **ScreenSteps**, ne on valmisteltava **ScreenSteps**kyselyjä.  
**ScreenSteps**valmistelu on manuaalinen tehtävä.

###<a name="to-provision-a-user-account-to-screensteps-perform-the-following-steps"></a>Valmistelu ScreenSteps käyttäjätili, toimi seuraavasti:

1.  Kirjaudu sisään **ScreenSteps** alihallintaan.

2.  Valitse **tilinhallinta**.

    ![Tilinhallinta] (./media/active-directory-saas-screensteps-tutorial/IC778523.png "Tilinhallinta")

3.  Valitse **käyttäjiä**.

    ![Käyttäjät] (./media/active-directory-saas-screensteps-tutorial/IC778544.png "Käyttäjät")

4.  Valitse **Luo käyttäjä**.

    ![Kaikki käyttäjät] (./media/active-directory-saas-screensteps-tutorial/IC778545.png "Kaikki käyttäjät")

5.  **Käyttäjärooli** tehtäväluettelosta Valitse käyttäjän rooli.

6.  Kirjoita käyttäjärooli-osassa haluat säännöstä kyselyjä liittyvät tekstiruutujen kelvollinen AAD tilin**"Etunimi**, **Sukunimi**, **sähköpostin**, **Kirjaudu sisään**, **salasana** ja **Salasanan vahvistus"**.

    ![Uusi käyttäjä] (./media/active-directory-saas-screensteps-tutorial/IC778546.png "Uusi käyttäjä")

7.  Ryhmät-kohdassa Valitse **Todennus-ryhmä (SAML)**ja valitse sitten **Luo käyttäjä**.

    ![Ryhmät] (./media/active-directory-saas-screensteps-tutorial/IC778547.png "Ryhmät")

>[AZURE.NOTE]Voit käyttää mitä tahansa muita ScreenSteps käyttäjän tilin luontityökalut tai ohjelmointirajapinnan myöntämä ScreenSteps säännöstä AAD käyttäjätileille.

##<a name="assigning-users"></a>Käyttäjien määrittäminen
  
Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-screensteps-perform-the-following-steps"></a>Jos haluat määrittää käyttäjiä ScreenSteps, toimi seuraavasti:

1.  Testaa tilin luominen Azure perinteinen portaalissa.

2.  Valitse integration **ScreenSteps **sovellus-sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-screensteps-tutorial/IC773094.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-screensteps-tutorial/IC778548.png "Käyttäjien määrittäminen")
  
Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli. Katso lisätietoja Access-paneelin [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).