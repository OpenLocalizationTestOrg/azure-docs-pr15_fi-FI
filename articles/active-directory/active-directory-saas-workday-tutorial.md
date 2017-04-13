<properties 
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi työpäivä | Microsoft Azure" 
    description="Opettele käyttämään työpäivä Azure Active Directory-hakemistosta käyttöön kertakirjautumisen, automaattinen valmistelu ja lisää!." 
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

#<a name="tutorial-azure-active-directory-integration-with-workday"></a>Opetusohjelma: Työpäivä Azure Active Directory-integrointi
  
Tässä opetusohjelmassa tavoitteena on näyttämään Azure ja työpäivä integrointi. Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   Työpäivä-palvelutili
  
Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1.  Työpäivä-sovelluksen integrointi ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Määrittäminen käyttäjän valmistelu
4.  Määrittäminen käyttäjän valmistelu

![Skenaario] (./media/active-directory-saas-workday-tutorial/IC782919.png "Skenaario")

##<a name="enabling-the-application-integration-for-workday"></a>Työpäivä-sovelluksen integrointi ottaminen käyttöön
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön Salesforce-sovelluksen integrointi.

###<a name="to-enable-the-application-integration-for-workday-perform-the-following-steps"></a>Työpäivä-sovelluksen integrointi käyttöön toimimalla seuraavasti:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-workday-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-workday-tutorial/IC700994.png "Sovellukset")

4.  Avaa **Sovellus valikoima**, **Lisää App**ja valitse sitten **Lisää sovellus tehtävään organisaatiossani, jos haluat käyttää**.

    ![Mitä haluat tehdä?] (./media/active-directory-saas-workday-tutorial/IC700995.png "Mitä haluat tehdä?")

5.  Kirjoita **hakuruutuun** **työpäivät**.

    ![Työpäivä] (./media/active-directory-saas-workday-tutorial/IC701021.png "Työpäivä")

6.  Valitse tulosruudussa **työpäivä**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![Työpäivä] (./media/active-directory-saas-workday-tutorial/IC701022.png "Työpäivä")

##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjien todentamiseen työpäivä käyttämällä SAML-protokollan perusteella federation Azure AD-tilin kanssa.  
Kun nämä toimet, sinun on base-64-koodattu varmenteen luominen.  
Jos et ole tottunut näin, katso, [miten voit muuntaa binaariluvun varmenteen tekstitiedostoon](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Valitse integration **työpäivä** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-workday-tutorial/IC782920.png "Määritä kertakirjautuminen")

2.  **Miten käyttäjät voivat kirjautua työpäivä haluat käyttää** sivulla Valitse **Microsoft Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-workday-tutorial/IC782921.png "Määritä kertakirjautuminen")

3.  **Määritä sovelluksen URL-osoite** -sivulla suorittamalla seuraavat vaiheet ja valitse sitten **Seuraava**.

    ![Sovelluksen URL-Osoitteen määrittäminen] (./media/active-directory-saas-workday-tutorial/IC782957.png "Sovelluksen URL-Osoitteen määrittäminen")

    a. Kirjoita **Merkki-URL** -tekstiruutuun URL-osoite käyttää käyttäjien kirjautua työpäivä käyttämällä seuraavaa kaavaa:`https://impl.workday.com/<tenant>/login-saml2.htmld`

    b.  Kirjoita **Työpäivä vastaa URL** -tekstiruutuun työpäivä vastaa URL-osoite käyttämällä seuraavaa kaavaa:`https://impl.workday.com/<tenant>/login-saml.htmld`

    >[AZURE.NOTE]Vastaa URL-osoite on oltava aliraportti toimialue (esimerkiksi: www-wd2, wd3, wd3 toteutuksen, wd5, wd5 toteutuksen). 
    >Käyttämällä esimerkiksi "*http://www.myworkday.com*" toimii mutta "*http://myworkday.com*" ei tue. 
 
4.  Valitse **Määritä kertakirjautumisen osoitteessa työpäivä** -sivulla Lataa sertifikaatti, valitsemalla **Lataa**, ja Tallenna sertifikaattitiedosto tietokoneeseen.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-workday-tutorial/IC782922.png "Määritä kertakirjautuminen")

5.  Eri selainikkunassa Kirjaudu työpäivä yrityksesi sivuston järjestelmänvalvojan oikeuksilla.

6.  Siirry **valikon \> Workbench**.

    ![Workbench] (./media/active-directory-saas-workday-tutorial/IC782923.png "Workbench")

7.  Valitse **tilinhallinta**.

    ![Tilinhallinta] (./media/active-directory-saas-workday-tutorial/IC782924.png "Tilinhallinta")

8.  Siirry **Muokkaa vuokraajan asetukset – suojaus**.

    ![Muokkaa vuokraajan suojaus] (./media/active-directory-saas-workday-tutorial/IC782925.png "Muokkaa vuokraajan suojaus")

9.  **Uudelleenohjaus URL** -osassa seuraavasti:

    ![Uudelleenohjaus URL-osoitteet] (./media/active-directory-saas-workday-tutorial/IC7829581.png "Uudelleenohjaus URL-osoitteet")

    a. Valitse **Lisää rivi**.

    b. Kirjoita **Login Uudelleenohjaus URL-osoite** -tekstiruutu ja **Matkapuhelin ohjaa URL** -tekstiruutuun Azure perinteinen portaalin **Määrittäminen sovelluksen URL-osoite** -sivulla antamasi **Työpäivä vuokraajan URL-osoite** .
    
    c-näppäinyhdistelmää. Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa työpäivä** -valintasivun kopioi **Yhden Sign-Out palvelun URL-osoite**ja liitä se sitten **Kirjaudu Uudelleenohjaus URL** -tekstiruutuun.

    d.  Kirjoita **ympäristön** -tekstiruutuun ympäristön nimi.  


    >[AZURE.NOTE] Ympäristön-määritteen arvon on yhdistetty vuokraajan URL-osoite-arvo:
    >
    >-   Jos toimialuenimen työpäivä-vuokraajan URL-osoite alkaa toteutuksen (esimerkiksi: *https://impl.workday.com/\<vuokraajan\>/login-saml2.htmld*)- **ympäristössä** -määrite on määritettävä käyttöönotto.
    >-   Jos toimialuenimi alkaa jotain muuta, joudut Ota työpäivä saat **ympäristön** vastaavan arvon.

10. **SAML-asetukset** -osassa seuraavasti:

    ![SAML-asetukset] (./media/active-directory-saas-workday-tutorial/IC782926.png "SAML-asetukset")

    a.  Valitse **Ota käyttöön SAML todennusta**.

    b.  Valitse **Lisää rivi**.

11. SAML-tunnistetietojen toimittajat-osassa seuraavasti:

    ![SAML-tunnistetietojen toimittajat] (./media/active-directory-saas-workday-tutorial/IC7829271.png "SAML-tunnistetietojen toimittajat")

    a. Kirjoita jäsenyyden tarjoajan nimi-tekstiruutuun tarjoajan nimi (esimerkiksi: *SPInitiatedSSO*).

    b. Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa työpäivä** -valintasivun **Tunnistetietojen Toimittajan tunnus** -arvo kopioi ja liitä se sitten **myöntäjä** -tekstiruutu.

    c-näppäinyhdistelmää. Valitse **Ota käyttöön työpäivä Initialted Kirjaudu ulos**.

    d. Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa työpäivä** -valintasivun **Yksittäisen Sign-Out palvelun URL-osoite** -arvo kopioi ja liitä se sitten **Kirjaudu ulos pyyntö URL** -tekstiruutuun.


    e. Valitse **Tunnistetietojen toimittaja julkisen avaimen varmenne**ja valitse sitten **Luo**. 

    ![Luo] (./media/active-directory-saas-workday-tutorial/IC782928.png "Luo")

    f. Valitse **luominen x509 julkisella avaimella**. 
        
    ![Luo] (./media/active-directory-saas-workday-tutorial/IC782929.png "Luo")


1. **Näytä x509 julkinen avain** -osassa seuraavasti: 

    ![Näytä x509 julkinen avain] (./media/active-directory-saas-workday-tutorial/IC782930.png "Näytä x509 julkinen avain") 

    a. Kirjoita **nimi** -tekstiruutuun sertifikaatin nimi (esimerkiksi: *Henkilönsuojaimien\_SP*).
        
    b. Kirjoita **Voimassa-** -tekstiruutuun voimassa olevien määritteen arvo varmennetta.
    
    c-näppäinyhdistelmää.  Kirjoita **Voimassa,** -tekstiruutuun kelvollinen määritteen arvo varmennetta.
        
    >[AZURE.NOTE] Saat virheellinen päivämäärä- ja voimassaolon päivämäärään ladatut varmenteen kaksoisnapsauttamalla sitä. Päivämäärät näkyvät **tiedot** -välilehdessä.

    d. Luo **Base-64-koodattu** tiedosto ladattu varmennetta.  

    >[AZURE.TIP] Lisätietoja on artikkelissa [muuntaa binaariluvun varmenteen tekstitiedostoon](http://youtu.be/PlgrzUZ-Y1o)

    e.  Avaa base-64-koodattu varmennetta Muistiossa ja kopioimalla sen sisällön.
    
    f.  Liitä Leikepöydän sisältö **varmenne** -tekstiruutuun.
    
    g.  Valitse **OK**.

12.  Suorita seuraavat vaiheet: 

    ![SSO-määritys] (./media/active-directory-saas-workday-tutorial/IC7829351111.png "SSO-määritys")

    a.  Ota käyttöön **x509 yksityinen avain pari**.

    b.  Kirjoita **http://www.workday.com** **Palveluntarjoajan tunnus** -tekstiruutuun.

    c-näppäinyhdistelmää.  Valitse **Ota aloittanut SP SAML todennusta**.

    d.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa työpäivä** -valintasivun **Sign-palvelun URL** -arvon kopioi ja liitä se sitten **IdP SSO-palvelun URL** -tekstiruutuun.
     
    e. Valitse **ei Kovera SP käynnistämä todennuspyynnön**.

    f. Valitse **Pyydä allekirjoituksen todentamismenetelmän** **SHA256**. 
        
    ![Käyttöoikeuksien pyytäminen allekirjoituksen-menetelmän] (./media/active-directory-saas-workday-tutorial/IC782932.png "Käyttöoikeuksien pyytäminen allekirjoituksen-menetelmän") 
 
    g. Valitse **OK**. 
        
    ![OK] (./media/active-directory-saas-workday-tutorial/IC782933.png "OK")

12. Azure perinteinen portaalissa, **Määritä kertakirjautumisen osoitteessa työpäivä** -sivulle valitsemalla **Seuraava**. 

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-workday-tutorial/IC782934.png "Määritä kertakirjautuminen")

13. Valitse **Sign Vahvista** -sivulla **Valmis**. 

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-workday-tutorial/IC782935111.png "Määritä kertakirjautuminen")



##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu
  
Saat testikäyttäjän valmisteltu kyselyjä työpäivä-haluat työpäivä-tukeen.  
Työpäivä-tukeen Luo käyttäjä.

##<a name="assigning-users"></a>Käyttäjien määrittäminen
  
Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-workday-perform-the-following-steps"></a>Jos haluat määrittää käyttäjiä työpäivä, toimi seuraavasti:

1.  Testaa tilin luominen Azure perinteinen portaalissa.

2.  Valitse integration **työpäivä **sovellus-sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-workday-tutorial/IC782935.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Kyllä] (./media/active-directory-saas-workday-tutorial/IC767830.png "Kyllä")
  
Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli. Katso lisätietoja Access-paneelin [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).