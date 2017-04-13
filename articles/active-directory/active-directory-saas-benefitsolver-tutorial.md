<properties 
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi Benefitsolver | Microsoft Azure"
    description="Opettele käyttämään Benefitsolver Azure Active Directory-hakemistosta käyttöön kertakirjautumisen, automaattinen valmistelu ja lisää!" 
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
    ms.date="10/10/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-benefitsolver"></a>Opetusohjelma: Benefitsolver Azure Active Directory-integrointi

Tässä opetusohjelmassa tavoitteena on näyttämään Azure ja Benefitsolver integrointi.  
Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   Käytössä Benefitsolver kertakirjautumisen tilaus

Kun olet suorittanut Tässä opetusohjelmassa, olet liittänyt Benefitsolver Azure AD-käyttäjien saa oikeuden kertakirjauksen käyttämällä [Access-paneelin esittely](active-directory-saas-access-panel-introduction.md)-sovellukseen.

Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1.  Benefitsolver sovelluksen integrointi ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Määrittäminen käyttäjän valmistelu
4.  Käyttäjien määrittäminen

![Skenaario] (./media/active-directory-saas-benefitsolver-tutorial/IC804820.png "Skenaario")
##<a name="enabling-the-application-integration-for-benefitsolver"></a>Benefitsolver sovelluksen integrointi ottaminen käyttöön

Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön Benefitsolver sovelluksen integrointi.

###<a name="to-enable-the-application-integration-for-benefitsolver-perform-the-following-steps"></a>Benefitsolver sovelluksen integrointi käyttöön toimimalla seuraavasti:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-benefitsolver-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-benefitsolver-tutorial/IC700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-benefitsolver-tutorial/IC749321.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-benefitsolver-tutorial/IC749322.png "Lisää sovellus-gallerry")

6.  Kirjoita **hakuruutuun** **Benefitsolver**.

    ![Sovelluksen valikoima] (./media/active-directory-saas-benefitsolver-tutorial/IC804821.png "Sovelluksen valikoima")

7.  Valitse tulosruudussa **Benefitsolver**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![Benefitssolver] (./media/active-directory-saas-benefitsolver-tutorial/IC804822.png "Benefitssolver")
##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen

Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjien todentamiseen Benefitsolver käyttämällä SAML-protokollan perusteella federation Azure AD-tilin kanssa.  
Benefitsolver sovelluksen odottaa SAML vahvistukset tietyssä muodossa, jossa pyydetään Lisää mukautettujen määritteiden yhdistämismäärityksiä **saml suojaustunnuksen määritteet** -määrityksiin.  
Seuraavassa näyttökuvassa näkyy esimerkki tämän.

![Määritteet] (./media/active-directory-saas-benefitsolver-tutorial/IC804823.png "Määritteet")

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Valitse Azure perinteinen portaalissa integrointi **Benefitsolver** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-benefitsolver-tutorial/IC804824.png "Kertakirjautumisen määrittäminen")

2.  **Miten käyttäjät voivat kirjautua Benefitsolver haluat käyttää** sivulla Valitse **Microsoft Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-benefitsolver-tutorial/IC804825.png "Kertakirjautumisen määrittäminen")

3.  **Määritä sovelluksen asetukset** -sivulla toimimalla seuraavasti:

    ![Sovelluksen asetusten määrittäminen] (./media/active-directory-saas-benefitsolver-tutorial/IC804826.png "Sovelluksen asetusten määrittäminen")

    1.  Kirjoita **Merkki-URL** -tekstiruutuun **http://azure.benefitsolver.com**.
    2.  Kirjoita **https://www.benefitsolver.com/benefits/BenefitSolverView?page_name=single_signon_saml** **Vastaa URL** -tekstiruutuun.  


    3.  Valitse **Seuraava**.

4.  Valitse **Määritä kertakirjautumisen osoitteessa Benefitsolver** -sivulla lataamaan metatietojen valitsemalla **Lataa metatietojen**, ja Tallenna paikallisesti tietokoneen metatiedot-tiedostoon.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-benefitsolver-tutorial/IC804827.png "Kertakirjautumisen määrittäminen")

5.  Voit lähettää ladatut metatietojen tiedoston Benefitsolver tukihenkilöstölle.

    >[AZURE.NOTE] Benefitsolver tukihenkilöstö on suoritettava todellinen SSO-määritys.
Näyttöön tulee ilmoitus, kun SSO on otettu käyttöön tilauksen.

6.  Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Valmis** **Määrittäminen kertakirjautuminen** -valintaikkunassa Sulje.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-benefitsolver-tutorial/IC804828.png "Kertakirjautumisen määrittäminen")

7.  Valitse ylä-valikossa **määritteet** **SAML suojaustunnuksen määritteet** -valintaikkunan avaaminen.

    ![Määritteet] (./media/active-directory-saas-benefitsolver-tutorial/IC795920.png "Määritteet")

8.  Lisää pakollinen määrite yhdistämismääritykset, toimi seuraavasti:

    ![Määritteet] (./media/active-directory-saas-benefitsolver-tutorial/IC804823.png "Määritteet")

  	|Määritteen nimi|Määritteen arvo|
  	|---|---|
  	|ClientID|Tarvitset tätä arvoa käyttämistä Benefitsolver tukihenkilöstölle.|
  	|ClientKey|Tarvitset tätä arvoa käyttämistä Benefitsolver tukihenkilöstölle.|
  	|LogoutURL|Tarvitset tätä arvoa käyttämistä Benefitsolver tukihenkilöstölle.|
  	|Työntekijätunnus|Tarvitset tätä arvoa käyttämistä Benefitsolver tukihenkilöstölle.|

    1.  Tietoja kullekin riville edellä olevan taulukon Valitse **Lisää käyttäjä-määrite**.
    2.  Kirjoita määritteen nimi näkyy kyseisen rivin **Määritteen nimi** -tekstiruutuun.
    3.  Valitse rivin määritteen arvo **Määritteen arvo** -tekstiruutuun.
    4.  Valitse **Valmis**.

9.  Valitse **Ota muutokset käyttöön**.

##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu

Jos haluat ottaa käyttöön Azure AD-käyttäjät voivat kirjautua sisään Benefitsolver, ne on valmisteltava Benefitsolver kyselyjä.  
Benefitsolver, kyseessä työntekijöiden tiedot on täytetty laskenta-tiedoston HRIS järjestelmästä (yleensä yö)-sovelluksessa.  

>[AZURE.NOTE] Voit käyttää mitä tahansa muita Benefitsolver käyttäjän tilin luontityökalut tai ohjelmointirajapinnan myöntämä Benefitsolver säännöstä AAD käyttäjätileille.

##<a name="assigning-users"></a>Käyttäjien määrittäminen

Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-benefitsolver-perform-the-following-steps"></a>Jos haluat määrittää käyttäjiä Benefitsolver, toimi seuraavasti:

1.  Testaa tilin luominen Azure perinteinen portaalissa.

2.  Valitse integration **Benefitsolver **sovellus-sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-benefitsolver-tutorial/IC804829.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Kyllä] (./media/active-directory-saas-benefitsolver-tutorial/IC767830.png "Kyllä")

Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli. Katso lisätietoja Access-paneelin [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).
