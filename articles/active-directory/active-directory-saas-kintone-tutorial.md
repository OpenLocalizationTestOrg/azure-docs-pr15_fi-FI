<properties 
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi Kintone | Microsoft Azure" 
    description="Opettele käyttämään Kintone Azure Active Directory-hakemistosta käyttöön kertakirjautumisen, automaattinen valmistelu ja lisää!" 
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
    ms.date="09/01/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-kintone"></a>Opetusohjelma: Kintone Azure Active Directory-integrointi
  
Tässä opetusohjelmassa tavoitteena on näyttämään Azure ja Kintone integrointi.  
Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   Käytössä Kintone kertakirjautumisen tilaus
  
Kun olet suorittanut Tässä opetusohjelmassa, olet liittänyt Kintone Azure AD-käyttäjien saa oikeuden kertakirjauksen Kintone yrityksesi sivuston (palveluntarjoajan aloittanut allekirjoitus-) tai käyttämällä [Access-paneelin esittely](active-directory-saas-access-panel-introduction.md)-sovellukseen.
  
Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1.  Kintone sovelluksen integrointi ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Määrittäminen käyttäjän valmistelu
4.  Käyttäjien määrittäminen

![Skenaario] (./media/active-directory-saas-kintone-tutorial/IC785859.png "Skenaario")
##<a name="enabling-the-application-integration-for-kintone"></a>Kintone sovelluksen integrointi ottaminen käyttöön
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön Kintone sovelluksen integrointi.

###<a name="to-enable-the-application-integration-for-kintone-perform-the-following-steps"></a>Kintone sovelluksen integrointi käyttöön toimimalla seuraavasti:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-kintone-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-kintone-tutorial/IC700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-kintone-tutorial/IC749321.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-kintone-tutorial/IC749322.png "Lisää sovellus-gallerry")

6.  Kirjoita **hakuruutuun** **Kintone**.

    ![Sovelluksen valikoima] (./media/active-directory-saas-kintone-tutorial/IC785867.png "Sovelluksen valikoima")

7.  Valitse tulosruudussa **Kintone**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![Kintone] (./media/active-directory-saas-kintone-tutorial/IC785871.png "Kintone")
##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjien todentamiseen Kintone käyttämällä SAML-protokollan perusteella federation Azure AD-tilin kanssa.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Valitse Azure perinteinen portaalissa integrointi **Kintone** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-kintone-tutorial/IC785872.png "Kertakirjautumisen määrittäminen")

2.  **Miten käyttäjät voivat kirjautua Kintone haluat käyttää** sivulla Valitse **Microsoft Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-kintone-tutorial/IC785873.png "Kertakirjautumisen määrittäminen")

3.  **Määritä sovelluksen URL-osoite** -sivulla **Kintone merkki-URL** -tekstiruutuun Kirjoita käyttämällä seuraavaa kaavaa "*https://company.kintone.com*" URL-osoite ja valitse sitten **Seuraava**.

    ![Sovelluksen URL-Osoitteen määrittäminen] (./media/active-directory-saas-kintone-tutorial/IC785875.png "Sovelluksen URL-Osoitteen määrittäminen")

4.  Valitse **Määritä kertakirjautumisen osoitteessa Kintone** -sivulla Lataa sertifikaatti, valitsemalla **Lataa**, ja Tallenna sertifikaattitiedosto tietokoneeseen.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-kintone-tutorial/IC785878.png "Kertakirjautumisen määrittäminen")

5.  Eri selainikkunassa Kirjaudu **Kintone** yrityksesi sivuston järjestelmänvalvojan oikeuksilla.

6.  Valitse **asetukset**.

    ![Asetukset] (./media/active-directory-saas-kintone-tutorial/IC785879.png "Asetukset")

7.  Valitse **käyttäjät ja järjestelmänvalvoja**.

    ![Käyttäjät ja järjestelmän hallinta] (./media/active-directory-saas-kintone-tutorial/IC785880.png "Käyttäjät ja järjestelmän hallinta")

8.  Valitse **järjestelmänvalvontaan \> suojauksen** valitsemalla **Kirjaudu sisään**.

    ![Kirjautuminen] (./media/active-directory-saas-kintone-tutorial/IC785881.png "Kirjautuminen")

9.  Valitse **Ota käyttöön SAML todennusta**.

    ![SAML-todennus] (./media/active-directory-saas-kintone-tutorial/IC785882.png "SAML-todennus")

10. SAML todentaminen ‑osassa toimi seuraavasti:

    ![SAML-todennus] (./media/active-directory-saas-kintone-tutorial/IC785883.png "SAML-todennus")

    1.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa Kintone** -valintasivun **Remote sisäänkirjautumisen URL-osoite** -arvo kopioi ja liitä se sitten **Kirjautuminen URL** -tekstiruutuun.
    2.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa Kintone** -valintasivun **Remote Kirjaudu URL-osoite** -arvo kopioi ja liitä se sitten **Kirjaudu ulos URL** -tekstiruutuun.
    3.  Valitse Lataa ladatut varmenteen **Selaa** .
    4.  Valitse **Tallenna**.

11. Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Valmis** **Määrittäminen kertakirjautuminen** -valintaikkunassa Sulje.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-kintone-tutorial/IC785884.png "Kertakirjautumisen määrittäminen")
##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu
  
Jos haluat ottaa käyttöön Azure AD-käyttäjät voivat kirjautua sisään Kintone, ne on valmisteltava Kintone kyselyjä.  
Kintone valmistelu on manuaalinen tehtävä.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Valmistelu käyttäjätilit, toimi seuraavasti:

1.  Kirjaudu sisään **Kintone** yrityksesi sivuston järjestelmänvalvojan oikeuksilla.

2.  Valitse **asetus**.

    ![Asetukset] (./media/active-directory-saas-kintone-tutorial/IC785879.png "Asetukset")

3.  Valitse **käyttäjät ja järjestelmänvalvoja**.

    ![Käyttäjä ja järjestelmänvalvoja] (./media/active-directory-saas-kintone-tutorial/IC785880.png "Käyttäjä ja järjestelmänvalvoja")

4.  Valitse **Käyttäjien hallinta**-kohdassa **Osastot ja käyttäjille**.

    ![Osasto- ja käyttäjille] (./media/active-directory-saas-kintone-tutorial/IC785888.png "Osasto- ja käyttäjille")

5.  Valitse **Uusi käyttäjä**.

    ![Uudet käyttäjät] (./media/active-directory-saas-kintone-tutorial/IC785889.png "Uudet käyttäjät")

6.  Valitse **Uusi käyttäjä** -osassa seuraavasti:

    ![Uudet käyttäjät] (./media/active-directory-saas-kintone-tutorial/IC785890.png "Uudet käyttäjät")

    1.  Kirjoita **Näyttönimi**, **Käyttäjänimi**, **Uusi salasana**, **Vahvista salasana**, **Sähköpostiosoite** ja muut tiedot, jos haluat säännöstä kyselyjä liittyvät texboxes kelvollinen AAD tilin.
    2.  Valitse **Tallenna**.

>[AZURE.NOTE] Voit käyttää mitä tahansa muita Kintone käyttäjän tilin luontityökalut tai ohjelmointirajapinnan myöntämä Kintone säännöstä AAD käyttäjätileille.

##<a name="assigning-users"></a>Käyttäjien määrittäminen
  
Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-kintone-perform-the-following-steps"></a>Jos haluat määrittää käyttäjiä Kintone, toimi seuraavasti:

1.  Testaa tilin luominen Azure perinteinen portaalissa.

2.  Valitse integration **Kintone **sovellus-sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-kintone-tutorial/IC785891.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Kyllä] (./media/active-directory-saas-kintone-tutorial/IC767830.png "Kyllä")
  
Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli. Katso lisätietoja Access-paneelin [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).