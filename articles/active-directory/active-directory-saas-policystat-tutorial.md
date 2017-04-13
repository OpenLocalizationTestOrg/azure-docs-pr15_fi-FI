<properties 
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi PolicyStat | Microsoft Azure" 
    description="Opettele käyttämään PolicyStat Azure Active Directory-hakemistosta käyttöön kertakirjautumisen, automaattinen valmistelu ja lisää!" 
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

#<a name="tutorial-azure-active-directory-integration-with-policystat"></a>Opetusohjelma: PolicyStat Azure Active Directory-integrointi
  
Tässä opetusohjelmassa tavoitteena on näyttämään Azure ja PolicyStat integrointi.  
Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   PolicyStat palvelutili
  
Kun olet suorittanut Tässä opetusohjelmassa, olet liittänyt PolicyStat Azure AD-käyttäjien saa oikeuden kertakirjauksen PolicyStat yrityksesi sivuston (palveluntarjoajan aloittanut allekirjoitus-) tai käyttämällä [Access-paneelin esittely](active-directory-saas-access-panel-introduction.md)-sovellukseen.
  
Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1.  PolicyStat sovelluksen integrointi ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Määrittäminen käyttäjän valmistelu
4.  Käyttäjien määrittäminen

![Skenaario] (./media/active-directory-saas-policystat-tutorial/IC808662.png "Skenaario")
##<a name="enabling-the-application-integration-for-policystat"></a>PolicyStat sovelluksen integrointi ottaminen käyttöön
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön PolicyStat sovelluksen integrointi.

###<a name="to-enable-the-application-integration-for-policystat-perform-the-following-steps"></a>PolicyStat sovelluksen integrointi käyttöön toimimalla seuraavasti:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-policystat-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-policystat-tutorial/IC700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-policystat-tutorial/IC749321.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-policystat-tutorial/IC749322.png "Lisää sovellus-gallerry")

6.  Kirjoita **hakuruutuun** **PolicyStat**.

    ![Sovelluksen valikoima] (./media/active-directory-saas-policystat-tutorial/IC808627.png "Sovelluksen valikoima")

7.  Valitse tulosruudussa **PolicyStat**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![PolicyStat] (./media/active-directory-saas-policystat-tutorial/IC810430.png "PolicyStat")
##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjien todentamiseen PolicyStat käyttämällä SAML-protokollan perusteella federation Azure AD-tilin kanssa.  
PolicyStat sovelluksen odottaa SAML vahvistukset tietyssä muodossa, jossa pyydetään Lisää mukautettujen määritteiden yhdistämismäärityksiä **saml suojaustunnuksen määritteet** -määrityksiin.  
Seuraavassa näyttökuvassa näkyy esimerkki tämän.

![Määritteet] (./media/active-directory-saas-policystat-tutorial/IC808628.png "Määritteet")

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Valitse Azure perinteinen portaalissa integrointi **PolicyStat** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-policystat-tutorial/IC808629.png "Kertakirjautumisen määrittäminen")

2.  **Miten käyttäjät voivat kirjautua PolicyStat haluat käyttää** sivulla Valitse **Microsoft Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-policystat-tutorial/IC808630.png "Kertakirjautumisen määrittäminen")

3.  Kirjoita **Määrittää sovelluksen asetukset** -sivun **Merkki-URL** -tekstiruutuun Sign URL-Osoitteen PolicyStat sovelluksen käyttäjille käyttämä URL-osoite (esimerkiksi: *"https://demo-azure.policystat.com"*), ja valitse sitten **Seuraava**.

    ![Sovelluksen asetusten määrittäminen] (./media/active-directory-saas-policystat-tutorial/IC808631.png "Sovelluksen asetusten määrittäminen")

4.  Valitse **Määritä kertakirjautumisen osoitteessa PolicyStat** -sivulla valitsemalla **Lataa metatiedot**ja Tallenna tietokoneessa metatiedot-tiedostoon.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-policystat-tutorial/IC808632.png "Kertakirjautumisen määrittäminen")

5.  Eri selainikkunassa Kirjaudu PolicyStat yrityksesi sivuston järjestelmänvalvojan oikeuksilla.

6.  Valitse **järjestelmänvalvoja** -välilehteä ja valitse sitten **Sign-On määritys** vasemmanpuoleisessa siirtymisruudussa.

    ![Järjestelmänvalvoja-valikosta] (./media/active-directory-saas-policystat-tutorial/IC808633.png "Järjestelmänvalvoja-valikosta")

7.  Valitse **asetukset** -osiossa **ottaminen käyttöön yhden Sign-integrointi**.

    ![Sign-määritys] (./media/active-directory-saas-policystat-tutorial/IC808634.png "Sign-määritys")

8.  Valitse **Määritä määritteet**ja suorita sitten **Määritä määritteet** -osassa seuraavasti:

    ![Sign-määritys] (./media/active-directory-saas-policystat-tutorial/IC808635.png "Sign-määritys")

    1.  Kirjoita **Käyttäjänimi-määrite** -tekstiruutuun **uid**.
    2.  Kirjoita **Etunimi** **Etunimi-määrite** -tekstiruutuun.
    3.  Kirjoita **Sukunimi** **Viimeisen Name-määrite** -tekstiruutuun.
    4.  Kirjoita **sähköpostiosoite** **Sähköposti-määrite** -tekstiruutuun.
    5.  Valitse **Tallenna muutokset**.

9.  Valitse **Your IDP metatietojen**ja suorita sitten **Your IDP metatiedot** -osassa seuraavasti:

    ![Sign-määritys] (./media/active-directory-saas-policystat-tutorial/IC808635.png "Sign-määritys")

    1.  Ladatun metatiedot-tiedoston avaaminen, sisällön kopioiminen ja liittäminen **Your Identity tarjoajan metatiedot** -tekstiruutu
    2.  Valitse **Tallenna muutokset**.

10. Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Valmis** **Määrittäminen kertakirjautuminen** -valintaikkunassa Sulje.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-policystat-tutorial/IC771723.png "Kertakirjautumisen määrittäminen")

11. 12. Valitse ylä-valikossa **määritteet** **SAML suojaustunnuksen määritteet** -valintaikkunan avaaminen.

    ![Määritteet] (./media/active-directory-saas-policystat-tutorial/IC795920.png "Määritteet")

13. Lisää pakollinen määrite yhdistämismääritykset, toimi seuraavasti:

    ![Määritteet] (./media/active-directory-saas-policystat-tutorial/IC804823.png "Määritteet")

    1.  Valitse **Lisää käyttäjä-määrite**.
    2.  Kirjoita **uid** **Määritteen nimi** -tekstiruutuun.
    3.  Valitse **ExtractMailPrefix()** **Määritteen arvo** -tekstiruutuun.
    4.  Valitse **Sähköposti** -luettelosta **User.mail**.
    5.  Valitse **Valmis**.
##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu
  
Jos haluat ottaa käyttöön Azure AD-käyttäjät voivat kirjautua sisään PolicyStat, ne on valmisteltava PolicyStat kyselyjä.  
PolicyStat tukee vain kerran käyttäjän valmistelu. Tämä tarkoittaa sitä, sinun ei tarvitse lisätä käyttäjiä manuaalisesti PolicyStat.  
Käyttäjien Hae lisätty automaattisesti niiden ensimmäisen kirjaudut kertakirjauksen kautta.

>[AZURE.NOTE]Voit käyttää mitä tahansa muita PolicyStat käyttäjän tilin luontityökalut tai ohjelmointirajapinnan myöntämä PolicyStat säännöstä AAD käyttäjätileille.

##<a name="assigning-users"></a>Käyttäjien määrittäminen
  
Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-policystat-perform-the-following-steps"></a>Jos haluat määrittää käyttäjiä PolicyStat, toimi seuraavasti:

1.  Testaa tilin luominen Azure perinteinen portaalissa.

2.  Valitse integration **PolicyStat **sovellus-sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-policystat-tutorial/IC808636.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Kyllä] (./media/active-directory-saas-policystat-tutorial/IC767830.png "Kyllä")
  
Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli. Katso lisätietoja Access-paneelin [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).