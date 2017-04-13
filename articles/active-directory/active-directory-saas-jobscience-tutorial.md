<properties 
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi Jobscience | Microsoft Azure" 
    description="Opettele käyttämään Jobscience Azure Active Directory-hakemistosta käyttöön kertakirjautumisen, automaattinen valmistelu ja lisää!" 
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

#<a name="tutorial-azure-active-directory-integration-with-jobscience"></a>Opetusohjelma: Jobscience Azure Active Directory-integrointi
  
Tässä opetusohjelmassa tavoitteena on näyttämään Azure ja Jobscience integrointi.  
Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   Tilauksen Jobscience Single Sign-On otettu käyttöön
  
Kun olet suorittanut Tässä opetusohjelmassa, olet liittänyt Jobscience Azure AD-käyttäjien saa oikeuden kertakirjauksen Jobscience yrityksesi sivuston (palveluntarjoajan aloittanut allekirjoitus-) tai käyttämällä [Access-paneelin esittely](active-directory-saas-access-panel-introduction.md)-sovellukseen.
  
Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1.  Jobscience sovelluksen integrointi ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Määrittäminen käyttäjän valmistelu
4.  Käyttäjien määrittäminen

![Skenaario] (./media/active-directory-saas-jobscience-tutorial/IC784341.png "Skenaario")
##<a name="enabling-the-application-integration-for-jobscience"></a>Jobscience sovelluksen integrointi ottaminen käyttöön
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön Jobscience sovelluksen integrointi.

###<a name="to-enable-the-application-integration-for-jobscience-perform-the-following-steps"></a>Jobscience sovelluksen integrointi käyttöön toimimalla seuraavasti:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-jobscience-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-jobscience-tutorial/IC700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-jobscience-tutorial/IC749321.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-jobscience-tutorial/IC749322.png "Lisää sovellus-gallerry")

6.  Kirjoita **hakuruutuun** **jobscience**.

    ![Sovelluksen valikoima] (./media/active-directory-saas-jobscience-tutorial/IC784342.png "Sovelluksen valikoima")

7.  Valitse tulosruudussa **Jobscience**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![Jobscience] (./media/active-directory-saas-jobscience-tutorial/IC784357.png "Jobscience")
##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjien todentamiseen Jobscience käyttämällä SAML-protokollan perusteella federation Azure AD-tilin kanssa.  
Kertakirjautumisen varten Jobscience määrittäminen edellyttää, että allekirjoitus arvon noutaa varmenne.  
Jos et ole tottunut näin, katso, [miten voit hakea varmenteen allekirjoitus-arvon](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Kirjaudu sisään Jobscience yrityksesi sivuston järjestelmänvalvojan oikeuksilla.

2.  Siirry **asetukset**.

    ![Asetukset] (./media/active-directory-saas-jobscience-tutorial/IC784358.png "Asetukset")

3.  Valitse vasemman siirtymisruudun **hallinta** -kohdassa Laajenna liittyvän osa **Toimialuehallinta** ja valitse sitten Avaa **Oma toimialue** -sivun **Oma toimialue** . 

    ![Toimialueen omistajuutta] (./media/active-directory-saas-jobscience-tutorial/IC767825.png "Toimialueen omistajuutta")

4.  Jos haluat varmistaa, että toimialueesi on määritetty oikein, varmista, että se on "**Vaiheessa 4 käyttöön käyttäjien**" ja tarkista "**My Domain Settings**".

    ![Doman käyttöön käyttäjään] (./media/active-directory-saas-jobscience-tutorial/IC784377.png "Doman käyttöön käyttäjään")

5.  Azure perinteinen portaalin kirjautuminen eri selainikkunassa.

6.  Valitse integration **Jobscience** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-jobscience-tutorial/IC784360.png "Kertakirjautumisen määrittäminen")

7.  **Miten käyttäjät voivat kirjautua Jobscience haluat käyttää** sivulla Valitse **Microsoft Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-jobscience-tutorial/IC784361.png "Kertakirjautumisen määrittäminen")

8.  **Määrittää sovelluksen URL-osoite** -sivulla **Jobscience merkki-URL** -tekstiruutuun Kirjoita käyttämällä seuraavaa kaavaa "*http://company.my.salesforce.com*" URL-osoite ja valitse sitten **Seuraava**.

    ![Sovelluksen URL-Osoitteen määrittäminen] (./media/active-directory-saas-jobscience-tutorial/IC784362.png "Sovelluksen URL-Osoitteen määrittäminen")

9.  **Määritä kertakirjautumisen osoitteessa Jobscience** -sivulla Lataa sertifikaatti, valitse **Lataa**, ja Tallenna sertifikaattitiedosto paikallisesti tietokoneeseen.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-jobscience-tutorial/IC784363.png "Kertakirjautumisen määrittäminen")

10. Yrityksen Jobscience-sivustossa **Turvaominaisuudet**ja sitten **Sign-asetukset**.

    ![Turvaominaisuudet] (./media/active-directory-saas-jobscience-tutorial/IC784364.png "Turvaominaisuudet")

11. Valitse **Sign-On asetukset** -kohdassa toimimalla seuraavasti:

    ![Kertakirjauksen asetukset] (./media/active-directory-saas-jobscience-tutorial/IC781026.png "Kertakirjauksen asetukset")

    1.  Valitse **SAML käytössä**.
    2.  Valitse **Uusi**.

12. Valitse **SAML yksittäisen Sign-asetus muokkaaminen** -valintaikkunassa seuraavasti:

    ![SAML yksittäisen Sign-asetus] (./media/active-directory-saas-jobscience-tutorial/IC784365.png "SAML yksittäisen Sign-asetus")

    1.  Kirjoita **nimi** -tekstiruutuun kokoonpanosi nimi.
    2.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa Jobscience** valintaikkunan-sivulla kopioi **Myöntäjä URL-osoite** -arvo ja liitä se **myöntäjä** -tekstiruutu
    3.  Kirjoita **Kohteen tunnus** -tekstiruutuun **https://salesforce-jobscience.com**
    4.  Valitse **Selaa** lataaminen Azure AD-sertifikaatti.
    5.  **SAML tunnuksen tyyppiä**Valitse **vahvistus sisältää käyttäjän objektista Federation-tunnuksen**.
    6.  **SAML tunnistetietojen sijaintia**Valitse **tunnistetiedot on NameIdentfier osan aihe-lause**.
    7.  Kopioi **Remote sisäänkirjautumisen URL-osoite** -arvo Azure perinteinen portal-sivulla **Määritä kertakirjautumisen osoitteessa Jobscience** valintaikkunan ja liitä se sitten **Tunnistetietojen toimittaja sisäänkirjautumisen URL-osoite** -tekstiruutu
    8.  Kopioi **Remote Kirjaudu URL-osoite** -arvo Azure perinteinen portal-sivulla **Määritä kertakirjautumisen osoitteessa Jobscience** valintaikkunan ja liitä se sitten **Tunnistetietojen toimittaja Kirjaudu URL-osoite** -tekstiruutu
    9.  Valitse **Tallenna**.

13. Valitse vasemman siirtymisruudun **hallinta** -kohdassa Laajenna liittyvän osa **Toimialuehallinta** ja valitse sitten Avaa **Oma toimialue** -sivun **Oma toimialue** . 

    ![Toimialueen omistajuutta] (./media/active-directory-saas-jobscience-tutorial/IC767825.png "Toimialueen omistajuutta")

14. Valitse **Oma toimialue** -sivulla **Kirjaudu sisään-sivun ulkoasu** -osassa valitsemalla **Muokkaa**.

    ![Kirjaudu sisään-sivun mukauttaminen] (./media/active-directory-saas-jobscience-tutorial/IC767826.png "Kirjaudu sisään-sivun mukauttaminen")

15. **Kirjaudu sisään-sivun ulkoasu** -sivulla **Todentamispalvelu** -osassa **SAML SSO asetukset** nimi näkyy. Valitse se ja valitse sitten **Tallenna**.

    ![Kirjaudu sisään-sivun mukauttaminen] (./media/active-directory-saas-jobscience-tutorial/IC784366.png "Kirjaudu sisään-sivun mukauttaminen")

16. Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Valmis** **Määrittäminen kertakirjautuminen** -valintaikkunassa Sulje.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-jobscience-tutorial/IC784367.png "Kertakirjautumisen määrittäminen")
  
Voit muuttaa SP omaan kertakirjautuminen sisäänkirjautumisen URL-osoite valitsemalla **kertakirjautuminen asetukset** **Turvaominaisuudet** valikko-osassa.

![Turvaominaisuudet] (./media/active-directory-saas-jobscience-tutorial/IC784368.png "Turvaominaisuudet")
  
Valitse yllä olevassa vaiheessa luomasi SSO profiilia.  
Tällä sivulla näkyy yksi merkki URL-Osoitteen yrityksesi (kuten *https://companyname.my.salesforce.com?so=companyid*).
##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu
  
Jos haluat ottaa käyttöön Azure AD-käyttäjät voivat kirjautua sisään Jobscience, ne on valmisteltava Jobscience kyselyjä.  
Jobscience valmistelu on manuaalinen tehtävä.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Voit määrittää käyttäjän valmistelu toimimalla seuraavasti:

1.  Kirjaudu sisään **Jobscience** yrityksesi sivuston järjestelmänvalvojana.

2.  Määrittäminen

    ![Asetukset] (./media/active-directory-saas-jobscience-tutorial/IC784358.png "Asetukset")

3.  Siirry **käyttäjien hallinta \> käyttäjien**.

    ![Käyttäjät] (./media/active-directory-saas-jobscience-tutorial/IC784369.png "Käyttäjät")

4.  Valitse **Uusi käyttäjä**.

    ![Kaikki käyttäjät] (./media/active-directory-saas-jobscience-tutorial/IC784370.png "Kaikki käyttäjät")

5.  **Käyttäjän muokkaaminen** -valintaikkunassa seuraavasti:

    ![Käyttäjän muokkaaminen] (./media/active-directory-saas-jobscience-tutorial/IC784371.png "Käyttäjän muokkaaminen")

    1.  Kirjoita etunimi, Sukunimi, alias, sähköpostin, Azure AD-käyttäjän säännöstä kyselyjä liittyvät tekstiruutujen haluat käyttäjän nimi ja lempinimien ominaisuuksia.
    2.  Valitse **Tallenna**.

    >[AZURE.NOTE] Azure AD-tilin omistaja saavat sähköpostiviestin, joka sisältää linkin Vahvista tili, ennen kuin se on aktivoitu.

>[AZURE.NOTE] Voit käyttää mitä tahansa muita Jobscience käyttäjän tilin luontityökalut tai ohjelmointirajapinnan myöntämä Jobscience säännöstä AAD käyttäjätileille.

##<a name="assigning-users"></a>Käyttäjien määrittäminen
  
Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-jobscience-perform-the-following-steps"></a>Jos haluat määrittää käyttäjiä Jobscience, toimi seuraavasti:

1.  Testaa tilin luominen Azure perinteinen portaalissa.

2.  Valitse integration **Jobscience **sovellus-sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-jobscience-tutorial/IC784372.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Kyllä] (./media/active-directory-saas-jobscience-tutorial/IC767830.png "Kyllä")
  
Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli. Katso lisätietoja Access-paneelin [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).