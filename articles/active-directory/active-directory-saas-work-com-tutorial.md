<properties 
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi Work.com | Microsoft Azure" 
    description="Opettele käyttämään Work.com Azure Active Directory-hakemistosta käyttöön kertakirjautumisen, automaattinen valmistelu ja lisää!." 
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

#<a name="tutorial-azure-active-directory-integration-with-workcom"></a>Opetusohjelma: Work.com Azure Active Directory-integrointi
  
Tässä opetusohjelmassa tavoitteena on näyttämään Azure ja Work.com integrointi.  
Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   Käytössä Work.com kertakirjautumisen tilaus
  
Kun olet suorittanut tämän opetusohjelman, AAD käyttäjät, joille olet määrittää Accessin Work.com voivat yksittäinen merkki Work.com yrityksesi sivuston (palveluntarjoajan aloittanut allekirjoitus-) tai käyttämällä [Access-paneelin esittely](active-directory-saas-access-panel-introduction.md)-sovellukseen.
  
Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1.  Work.com sovelluksen integrointi ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Määrittäminen käyttäjän valmistelu
4.  Käyttäjien määrittäminen

![Skenaario] (./media/active-directory-saas-work-com-tutorial/IC794105.png "Skenaario")

##<a name="enabling-the-application-integration-for-workcom"></a>Work.com sovelluksen integrointi ottaminen käyttöön
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön Work.com sovelluksen integrointi.

###<a name="to-enable-the-application-integration-for-workcom-perform-the-following-steps"></a>Work.com sovelluksen integrointi käyttöön toimimalla seuraavasti:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-work-com-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-work-com-tutorial/IC700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-work-com-tutorial/IC749321.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-work-com-tutorial/IC749322.png "Lisää sovellus-gallerry")

6.  Kirjoita **hakuruutuun** **Work.com**.

    ![Sovelluksen valikoima] (./media/active-directory-saas-work-com-tutorial/IC794106.png "Sovelluksen valikoima")

7.  Valitse tulosruudussa **Work.com**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![Work.com] (./media/active-directory-saas-work-com-tutorial/IC794107.png "Work.com")

##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjien todentamiseen Work.com käyttämällä SAML-protokollan perusteella federation Azure AD-tilin kanssa.  
Tämän toimintosarjan osana tarvitaan varmenteen lataaminen Work.com.com.

>[AZURE.NOTE] Kertakirjautumisen määrittämiseen haluat asennuksen Work.com mukautetun toimialuenimen vielä. Sinun täytyy vähintään toimialuenimen määrittäminen, testaus toimialuenimi ja koko organisaation käyttöön.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Kirjaudu sisään Work.com asiakasympäristöön järjestelmänvalvojana.

2.  Siirry **asetukset**.

    ![Asetukset] (./media/active-directory-saas-work-com-tutorial/IC794108.png "Asetukset")

3.  Valitse vasemman siirtymisruudun **hallinta** -kohdassa Laajenna liittyvän osa **Toimialuehallinta** ja valitse sitten Avaa **Oma toimialue** -sivun **Oma toimialue** . 

    ![Toimialueen omistajuutta] (./media/active-directory-saas-work-com-tutorial/IC767825.png "Toimialueen omistajuutta")

4.  Jos haluat varmistaa, että toimialueesi on määritetty oikein, varmista, että se on "**Vaiheessa 4 käyttöön käyttäjien**" ja tarkista "**My Domain Settings**".

    ![Doman käyttöön käyttäjään] (./media/active-directory-saas-work-com-tutorial/IC784377.png "Doman käyttöön käyttäjään")

5.  Azure perinteinen portaalin kirjautuminen eri selainikkunassa.

6.  Valitse integration **Work.com **sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-work-com-tutorial/IC794109.png "Kertakirjautumisen määrittäminen")

7.  **Miten käyttäjät voivat kirjautua Work.com haluat käyttää** sivulla Valitse **Microsoft Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-work-com-tutorial/IC794110.png "Kertakirjautumisen määrittäminen")

8.  Kirjoita **Määritä sovelluksen URL-osoite** -sivulla **Work.com merkki-URL** -tekstiruutuun käyttämä käyttäjien kirjautua Work.com sovelluksen URL-osoite (esimerkiksi: " *http://company.my.salesforce.com*"), ja valitse sitten **Seuraava**: 

    ![Sovelluksen URL-Osoitteen määrittäminen] (./media/active-directory-saas-work-com-tutorial/IC794111.png "Sovelluksen URL-Osoitteen määrittäminen")

9.  **Määritä kertakirjautumisen osoitteessa Work.com** -sivulla Lataa sertifikaatti, valitse **Lataa**, ja Tallenna sertifikaattitiedosto paikallisesti tietokoneeseen.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-work-com-tutorial/IC794112.png "Kertakirjautumisen määrittäminen")

10. Kirjaudu sisään Work.com alihallintaan.

11. Siirry **asetukset**.

    ![Asetukset] (./media/active-directory-saas-work-com-tutorial/IC794108.png "Asetukset")

12. Laajenna **Turvaominaisuudet** -valikko ja valitse sitten **Sign-asetukset**.

    ![Kertakirjauksen asetukset] (./media/active-directory-saas-work-com-tutorial/IC794113.png "Kertakirjauksen asetukset")

13. Valitse **Sign-On asetukset** -sivulla toimimalla seuraavasti:

    ![SAML-käytössä] (./media/active-directory-saas-work-com-tutorial/IC781026.png "SAML-käytössä")

    1.  Valitse **SAML käytössä**.
    2.  Valitse **Uusi**.

14. **SAML yksittäisen kertakirjauksen asetukset** -osassa seuraavasti:

    ![SAML yksittäisen Sign-asetus] (./media/active-directory-saas-work-com-tutorial/IC794114.png "SAML yksittäisen Sign-asetus")

    1.  Kirjoita **nimi** -tekstiruutuun kokoonpanosi nimi.  

        >[AZURE.NOTE] Tarjoamalla arvon **nimi** täytetään **API nimi** -tekstiruutuun.

    2.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa Work.com** -valintasivun kopioi **Myöntäjä URL-osoite** -arvo ja liitä se sitten **myöntäjä** -tekstiruutuun.
    3.  Voit ladata ladatut varmenteen, valitsemalla **Selaa**.
    4.  Kirjoita **https://salesforce-work.com** **Kohteen tunnus** -tekstiruutuun.
    5.  **SAML tunnuksen tyyppiä**Valitse **vahvistus sisältää käyttäjän objektista Federation-tunnuksen**.
    6.  **SAML tunnistetietojen sijaintia**Valitse **tunnistetiedot on NameIdentfier osan aihe-lause**.
    7.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa Work.com** -valintasivun **Remote sisäänkirjautumisen URL-osoite** -arvo kopioi ja liitä se sitten **Tunnistetietojen toimittaja sisäänkirjautumisen URL-osoite** -tekstiruutuun.
    8.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa Work.com** -valintasivun **Remote Kirjaudu URL-osoite** -arvo kopioi ja liitä se sitten **Jäsenyyden tarjoajan Kirjaudu URL-Osoitteen** tekstiruutu.
    9.  **Palveluntarjoajan aloittanut pyytää sidonta**valitsemalla **HTTP viesti**.
    10. Valitse **Tallenna**.

15. Valitse Work.com perinteinen-portaalin, valitse vasemmassa siirtymisruudussa Laajenna liittyvän osa **Toimialuehallinta** ja valitse sitten Avaa **Oma toimialue** -sivun **Oma toimialue** . 

    ![Toimialueen omistajuutta] (./media/active-directory-saas-work-com-tutorial/IC794115.png "Toimialueen omistajuutta")

16. Valitse **Oma toimialue** -sivulla **Kirjaudu sisään-sivun ulkoasu** -osassa valitsemalla **Muokkaa**.

    ![Kirjaudu sisään-sivun mukauttaminen] (./media/active-directory-saas-work-com-tutorial/IC767826.png "Kirjaudu sisään-sivun mukauttaminen")

17. **Kirjaudu sisään-sivun ulkoasu** -sivulla **Todentamispalvelu** -osassa **SAML SSO asetukset** nimi näkyy. Valitse se ja valitse sitten **Tallenna**.

    ![Kirjaudu sisään-sivun mukauttaminen] (./media/active-directory-saas-work-com-tutorial/IC784366.png "Kirjaudu sisään-sivun mukauttaminen")

18. Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Valmis** **Määrittäminen kertakirjautuminen** -valintaikkunassa Sulje.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-work-com-tutorial/IC794116.png "Kertakirjautumisen määrittäminen")

##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu
  
Azure Active Directory käyttäjät voivat kirjautua ne on valmisteltava Work.com avulla.  
Work.com valmistelu on manuaalinen tehtävä.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Voit määrittää käyttäjän valmistelu toimimalla seuraavasti:

1.  Kirjaudu Work.com yrityksesi sivuston järjestelmänvalvojan oikeuksilla.

2.  Siirry **asetukset**.

    ![Asetukset] (./media/active-directory-saas-work-com-tutorial/IC794108.png "Asetukset")

3.  Siirry **käyttäjien hallinta \> käyttäjien**.

    ![Käyttäjien hallinta] (./media/active-directory-saas-work-com-tutorial/IC784369.png "Käyttäjien hallinta")

4.  Valitse **Uusi käyttäjä**.

    ![Kaikki käyttäjät] (./media/active-directory-saas-work-com-tutorial/IC794117.png "Kaikki käyttäjät")

5.  Käyttäjän muokkaaminen-osassa seuraavasti:

    ![Käyttäjän muokkaaminen] (./media/active-directory-saas-work-com-tutorial/IC794118.png "Käyttäjän muokkaaminen")

    1.  Kirjoita **Sukunimi**, **Alias**, **sähköpostin**, **käyttäjänimi** ja **lempinimien** määritteet haluat säännöstä kyselyjä liittyvät tekstiruutujen kelvollinen Azure Active Directory-tili.
    2.  Valitse **rooli**, **käyttöoikeus** ja **profiilin**.
    3.  Valitse **Tallenna**.  

        >[AZURE.NOTE] Azure Active Directory-tilin omistaja saavat sähköpostiviestin, mukaan lukien linkin Vahvista tili, ennen kuin se on aktiivinen.

>[AZURE.NOTE] Voit käyttää mitä tahansa muita Work.com käyttäjän tilin luontityökalut tai ohjelmointirajapinnan myöntämä Work.com säännöstä AAD käyttäjätileille.

##<a name="assigning-users"></a>Käyttäjien määrittäminen
  
Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-workcom-perform-the-following-steps"></a>Jos haluat määrittää käyttäjiä Work.com, toimi seuraavasti:

1.  Testaa tilin luominen Azure perinteinen portaalissa.

2.  Valitse integration Work.com sovellus-sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-work-com-tutorial/IC794119.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Kyllä] (./media/active-directory-saas-work-com-tutorial/IC767830.png "Kyllä")
  
Pitäisi nyt Odota 10 minuuttia ja varmista, että tili on synkronoitu Work.com.com.
  
Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli. Katso lisätietoja Access-paneelin [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).