<properties 
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi Zendesk | Microsoft Azure" 
    description="Opettele käyttämään Zendesk Azure Active Directory-hakemistosta käyttöön kertakirjautumisen, automaattinen valmistelu ja lisää!." 
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
    ms.date="09/09/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-zendesk"></a>Opetusohjelma: Zendesk Azure Active Directory-integrointi
  
Tässä opetusohjelmassa tavoitteena on näyttämään Azure ja Zendesk integrointi.  
Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   Zendesk palvelutili
  
Kun olet suorittanut Tässä opetusohjelmassa, olet liittänyt Zendesk Azure AD-käyttäjien saa oikeuden kertakirjauksen Zendesk yrityksesi sivuston (palveluntarjoajan aloittanut allekirjoitus-) tai käyttämällä [Access-paneelin esittely](active-directory-saas-access-panel-introduction.md)-sovellukseen.
  
Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1.  Zendesk sovelluksen integrointi ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Määrittäminen käyttäjän valmistelu
4.  Käyttäjien määrittäminen

![Skenaario] (./media/active-directory-saas-zendesk-tutorial/IC773083.png "Skenaario")

##<a name="enabling-the-application-integration-for-zendesk"></a>Zendesk sovelluksen integrointi ottaminen käyttöön
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön Zendesk sovelluksen integrointi.

###<a name="to-enable-the-application-integration-for-zendesk-perform-the-following-steps"></a>Zendesk sovelluksen integrointi käyttöön toimimalla seuraavasti:

1.  Valitse **Active Directory**Azure-hallinta-portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-zendesk-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-zendesk-tutorial/IC700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-zendesk-tutorial/IC749321.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-zendesk-tutorial/IC749322.png "Lisää sovellus-gallerry")

6.  Kirjoita **hakuruutuun** **Zendesk**.

    ![Sovelluksen valikoima] (./media/active-directory-saas-zendesk-tutorial/IC773084.png "Sovelluksen valikoima")

7.  Valitse tulosruudussa **Zendesk**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![Zendesk] (./media/active-directory-saas-zendesk-tutorial/IC773085.png "Zendesk")

##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjien todentamiseen Zendesk käyttämällä SAML-protokollan perusteella federation Azure AD-tilin kanssa.  
Kertakirjautumisen varten Zendesk määrittäminen edellyttää, että allekirjoitus arvon noutaa varmenne.  
Jos et ole tottunut näin, katso, [miten voit hakea varmenteen allekirjoitus-arvon](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Valitse Azure AD-portaalissa integrointi **Zendesk** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Kertakirjautuminen] (./media/active-directory-saas-zendesk-tutorial/IC773086.png "Kertakirjautuminen")

2.  **Miten käyttäjät voivat kirjautua Zendesk haluat käyttää** sivulla Valitse **Microsoft Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-zendesk-tutorial/IC773087.png "Määritä kertakirjautuminen")

3.  Tee **Määrittäminen sovelluksen URL-osoite** -sivulla seuraavat toimet:

    ![Sovelluksen URL-Osoitteen määrittäminen] (./media/active-directory-saas-zendesk-tutorial/IC773088.png "Sovelluksen URL-Osoitteen määrittäminen")
  
    a. Kirjoita **Zendesk merkki-URL** -tekstiruutuun URL-osoite käyttämällä seuraavaa kaavaa:`https://<tenant-name>.zendesk.com`

    b. Valitse **Seuraava**.



4.  Valitse **Määritä kertakirjautumisen osoitteessa Zendesk** -sivulla valitsemalla **Lataa**ja Tallenna sertifikaatin tiedoston paikallisesti että compiter.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-zendesk-tutorial/IC777534.png "Määritä kertakirjautuminen")

5.  Eri selainikkunassa Kirjaudu Zendesk yrityksesi sivuston järjestelmänvalvojan oikeuksilla.

6.  Valitse **järjestelmänvalvoja**.

7.  Valitse vasemmassa siirtymisruudussa **asetukset**ja valitse sitten **Suojaus**.

    ![Tietoturva] (./media/active-directory-saas-zendesk-tutorial/IC773089.png "Tietoturva")

8.  Valitse **Suojaus** -sivulla **järjestelmänvalvoja ja käyttäjäagenttien** -välilehti.

9.  Valitse **yksittäinen kertakirjautumisportaaliin (SSO) ja SAML**, ja valitse sitten **SAML**.

10. Azure AD-portaalissa, valitse **Määritä kertakirjautumisen osoitteessa Zendesk** -sivulla **SAML SSO URL-osoite** -arvo kopioi ja liitä se sitten **SAML SSO URL** -tekstiruutuun.

11. Azure AD-portaalissa, valitse **Määritä kertakirjautumisen osoitteessa Zendesk** -sivulla **Remote Kirjaudu URL-osoite** -arvo kopioi ja liitä se sitten **Remote Kirjaudu URL** -tekstiruutuun.

    ![Kertakirjautuminen] (./media/active-directory-saas-zendesk-tutorial/IC773090.png "Kertakirjautuminen")

12. Viedyn varmenteen **allekirjoitusta** kopioi ja liitä se **Varmenteen tunnisteen** tekstiruutu.

    >[AZURE.TIP] Lisätietoja on artikkelissa [noutamisesta varmenteen allekirjoitusarvo](http://youtu.be/YKQF266SAxI)

13. Valitse **Tallenna**.

14. Azure AD-portaalissa Valitse Sign määritysten vahvistus ja valitse sitten **Valmis** **Määrittäminen kertakirjautuminen** -valintaikkunassa Sulje.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-zendesk-tutorial/IC773093.png "Määritä kertakirjautuminen")

##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu
  
Jos haluat ottaa käyttöön Azure AD-käyttäjät voivat kirjautua sisään **Zendesk**, ne on valmisteltava **Zendesk**kyselyjä.  
**Zendesk**valmistelu on manuaalinen tehtävä.

###<a name="to-provision-a-user-account-to-zendesk-perform-the-following-steps"></a>Valmistelu Zendesk käyttäjätili, toimi seuraavasti:

1.  Kirjaudu sisään **Zendesk** alihallintaan.

2.  Valitse **Asiakas-luettelo** -välilehti.

3.  Valitse **käyttäjä** -välilehti ja sitten **Lisää**.

    ![Lisää käyttäjä] (./media/active-directory-saas-zendesk-tutorial/IC773632.png "Lisää käyttäjä")

4.  Kirjoita sähköpostiosoite, olemassa olevasta Azure AD-tilistä haluat säätää, ja valitse sitten **Tallenna**.

    ![Uusi käyttäjä] (./media/active-directory-saas-zendesk-tutorial/IC773633.png "Uusi käyttäjä")

>[AZURE.NOTE] Voit käyttää mitä tahansa muita Zendesk käyttäjän tilin luontityökalut tai ohjelmointirajapinnan myöntämä Zendesk säännöstä AAD käyttäjätileille.

##<a name="assigning-users"></a>Käyttäjien määrittäminen
  
Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-zendesk-perform-the-following-steps"></a>Jos haluat määrittää käyttäjiä Zendesk, toimi seuraavasti:

1.  Testaa tilin luominen Azure AD-portaalissa.

2.  Valitse integration **Zendesk **sovellus-sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-zendesk-tutorial/IC773094.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Kyllä] (./media/active-directory-saas-zendesk-tutorial/IC767830.png "Kyllä")
  
Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli. Katso lisätietoja Access-paneelin [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).
