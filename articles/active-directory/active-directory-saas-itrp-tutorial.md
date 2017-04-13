<properties
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi ITRP | Microsoft Azure" 
    description="Opettele käyttämään ITRP Azure Active Directory-hakemistosta käyttöön kertakirjautumisen, automaattinen valmistelu ja lisää!" 
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
    ms.date="09/07/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-itrp"></a>Opetusohjelma: ITRP Azure Active Directory-integrointi
  
Tässä opetusohjelmassa tavoitteena on näyttämään Azure ja ITRP integrointi.  
Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   ITRP palvelutili
  
Kun olet suorittanut Tässä opetusohjelmassa, olet liittänyt ITRP Azure AD-käyttäjien saa oikeuden kertakirjauksen ITRP yrityksesi sivuston (palveluntarjoajan aloittanut allekirjoitus-) tai käyttämällä [Access-paneelin esittely](active-directory-saas-access-panel-introduction.md)-sovellukseen.
  
Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1.  ITRP sovelluksen integrointi ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Määrittäminen käyttäjän valmistelu
4.  Käyttäjien määrittäminen

![Skenaario] (./media/active-directory-saas-itrp-tutorial/IC775551.png "Skenaario")
##<a name="enabling-the-application-integration-for-itrp"></a>ITRP sovelluksen integrointi ottaminen käyttöön
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön ITRP sovelluksen integrointi.

###<a name="to-enable-the-application-integration-for-itrp-perform-the-following-steps"></a>ITRP sovelluksen integrointi käyttöön toimimalla seuraavasti:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-itrp-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-itrp-tutorial/IC700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-itrp-tutorial/IC749321.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-itrp-tutorial/IC749322.png "Lisää sovellus-gallerry")

6.  Kirjoita **hakuruutuun** **ITRP**.

    ![Sovelluksen valikoima] (./media/active-directory-saas-itrp-tutorial/IC775565.png "Sovelluksen valikoima")

7.  Valitse tulosruudussa **ITRP**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![ITRP] (./media/active-directory-saas-itrp-tutorial/IC775566.png "ITRP")
##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjien todentamiseen ITRP käyttämällä SAML-protokollan perusteella federation Azure AD-tilin kanssa.  
Kertakirjautumisen varten ITRP määrittäminen edellyttää, että allekirjoitus arvon noutaa varmenne.  
Jos et ole tottunut näin, katso, [miten voit hakea varmenteen allekirjoitus-arvon](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Valitse Azure perinteinen portaalissa integrointi **ITRP** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-itrp-tutorial/IC771709.png "Määritä kertakirjautuminen")

2.  **Miten käyttäjät voivat kirjautua ITRP haluat käyttää** sivulla Valitse **Microsoft Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-itrp-tutorial/IC775567.png "Kertakirjautumisen määrittäminen")

3.  Kirjoita **Määritä sovelluksen URL-osoite** -sivulla **ITRP merkki-URL** -tekstiruutuun URL-osoite käyttämällä seuraavaa kaavaa "*https://\<vuokraajan nimi\>. ITRP.com*", ja valitse sitten **Seuraava**.

    ![Sovelluksen URL-Osoitteen määrittäminen] (./media/active-directory-saas-itrp-tutorial/IC775568.png "Sovelluksen URL-Osoitteen määrittäminen")

4.  Valitse **Määritä kertakirjautumisen osoitteessa ITRP** -sivulla Lataa sertifikaatti, **Lataa**ja Tallenna sertifikaattitiedosto paikallisesti kuin **c:\\ITRP.cer**.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-itrp-tutorial/IC775569.png "Kertakirjautumisen määrittäminen")

5.  Eri selainikkunassa Kirjaudu ITRP yrityksen sivuston järjestelmänvalvojan oikeuksilla.

6.  Valitse **asetukset**-sivun työkalurivillä.

    ![ITRP] (./media/active-directory-saas-itrp-tutorial/IC775570.png "ITRP")

7.  Valitse vasemmassa siirtymisruudussa **Kertakirjautumisen**.

    ![Kertakirjautuminen] (./media/active-directory-saas-itrp-tutorial/IC775571.png "Kertakirjautuminen")

8.  Kertakirjautumisen määrittäminen-osassa seuraavasti:

    ![Kertakirjautuminen] (./media/active-directory-saas-itrp-tutorial/IC775572.png "Kertakirjautuminen")

    ![Kertakirjautuminen] (./media/active-directory-saas-itrp-tutorial/IC775573.png "Kertakirjautuminen")

    1.  Valitse **Ota käyttöön**.
    2.  Azure perinteinen portaalissa-sivulla **Määritä kertakirjautumisen osoitteessa ITRP** valintaikkunan Kopioi **Remote Kirjaudu URL-osoite** -arvo ja liitä se sitten **Remote Kirjaudu URL** -tekstiruutuun.
    3.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa ITRP** -valintasivun **SAML SSO URL-osoite** -arvo kopioi ja liitä se sitten **SAML SSO URL** -tekstiruutuun.
    4.  Viedyn varmenteen **allekirjoitusta** kopioi ja liitä se **Varmenteen tunnisteen** tekstiruutu.
        
        >[AZURE.TIP]Lisätietoja on artikkelissa [noutamisesta varmenteen allekirjoitusarvo](http://youtu.be/YKQF266SAxI)

    5.  Valitse **Tallenna**.

9.  Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Valmis** **Määrittäminen kertakirjautuminen** -valintaikkunassa Sulje.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-itrp-tutorial/IC775574.png "Kertakirjautumisen määrittäminen")
##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu
  
Jos haluat ottaa käyttöön Azure AD-käyttäjät voivat kirjautua sisään ITRP, ne on valmisteltava ITRP kyselyjä.  
ITRP valmistelu on manuaalinen tehtävä.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Valmistelu käyttäjätilit, toimi seuraavasti:

1.  Kirjaudu sisään **ITRP** alihallintaan.

2.  Valitse sivun työkalurivillä **tietueita**.

    ![Järjestelmänvalvoja] (./media/active-directory-saas-itrp-tutorial/IC775575.png "Järjestelmänvalvoja")

3.  Valitse avautuvan ponnahdusikkunan **henkilöt**.

    ![Henkilöt] (./media/active-directory-saas-itrp-tutorial/IC775587.png "Henkilöt")

4.  Valitse **Lisää uusi henkilö** ("+").

    ![Järjestelmänvalvoja] (./media/active-directory-saas-itrp-tutorial/IC775576.png "Järjestelmänvalvoja")

5.  Valitse Lisää uusi henkilö-valintaikkunassa seuraavasti:

    ![Käyttäjän] (./media/active-directory-saas-itrp-tutorial/IC775577.png "Käyttäjän")

    1.  Kirjoita **nimi**, kelvollinen AAD-tilin **sähköpostin** haluat säätää.
    2.  Valitse **Tallenna**.

>[AZURE.NOTE] Voit käyttää mitä tahansa muita ITRP käyttäjän tilin luontityökalut tai ohjelmointirajapinnan myöntämä ITRP säännöstä AAD käyttäjätileille.

##<a name="assigning-users"></a>Käyttäjien määrittäminen
  
Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-itrp-perform-the-following-steps"></a>Jos haluat määrittää käyttäjiä ITRP, toimi seuraavasti:

1.  Testaa tilin luominen Azure AD-portaalissa.

2.  Valitse integration **ITRP **sovellus-sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-itrp-tutorial/IC775588.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Kyllä] (./media/active-directory-saas-itrp-tutorial/IC767830.png "Kyllä")
  
Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli. Katso lisätietoja Access-paneelin [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).