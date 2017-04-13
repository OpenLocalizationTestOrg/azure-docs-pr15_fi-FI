<properties 
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi Veracode | Microsoft Azure" 
    description="Opettele käyttämään Veracode Azure Active Directory-hakemistosta käyttöön kertakirjautumisen, automaattinen valmistelu ja lisää!" 
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

#<a name="tutorial-azure-active-directory-integration-with-veracode"></a>Opetusohjelma: Veracode Azure Active Directory-integrointi
  
Tässä opetusohjelmassa tavoitteena on näyttämään Azure ja Veracode integrointi. Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   Käytössä Veracode kertakirjautumisen tilaus
  
Kun olet suorittanut Tässä opetusohjelmassa, olet liittänyt Veracode Azure AD-käyttäjien saa oikeuden kertakirjauksen käyttämällä [Access-paneelin esittely](active-directory-saas-access-panel-introduction.md)-sovellukseen.
  
Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1.  Veracode sovelluksen integrointi ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Määrittäminen käyttäjän valmistelu
4.  Käyttäjien määrittäminen

![Skenaario] (./media/active-directory-saas-veracode-tutorial/IC802903.png "Skenaario")

##<a name="enabling-the-application-integration-for-veracode"></a>Veracode sovelluksen integrointi ottaminen käyttöön
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön Veracode sovelluksen integrointi.

###<a name="to-enable-the-application-integration-for-veracode-perform-the-following-steps"></a>Veracode sovelluksen integrointi käyttöön toimimalla seuraavasti:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-veracode-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-veracode-tutorial/IC700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-veracode-tutorial/IC749321.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-veracode-tutorial/IC749322.png "Lisää sovellus-gallerry")

6.  Kirjoita **hakuruutuun** **Veracode**.

    ![Sovelluksen valikoima] (./media/active-directory-saas-veracode-tutorial/IC802904.png "Sovelluksen valikoima")

7.  Valitse tulosruudussa **Veracode**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![Veracode] (./media/active-directory-saas-veracode-tutorial/IC802905.png "Veracode")

##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjien todentamiseen Veracode käyttämällä SAML-protokollan perusteella federation Azure AD-tilin kanssa.  
Veracode sovelluksen odottaa SAML vahvistukset tietyssä muodossa, jossa pyydetään Lisää mukautettujen määritteiden yhdistämismäärityksiä **saml suojaustunnuksen määritteet** -määrityksiin.  
Seuraavassa näyttökuvassa näkyy esimerkki tämän.

![Määritteet] (./media/active-directory-saas-veracode-tutorial/IC802906.png "Määritteet")

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Valitse Azure perinteinen portaalissa integrointi **Veracode** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-veracode-tutorial/IC802907.png "Kertakirjautumisen määrittäminen")

2.  **Miten käyttäjät voivat kirjautua Veracode haluat käyttää** sivulla Valitse **Microsoft Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-veracode-tutorial/IC802908.png "Kertakirjautumisen määrittäminen")

3.  **Määritä sovelluksen asetukset** -sivulle valitsemalla **Seuraava**.

    ![Sovelluksen asetusten määrittäminen] (./media/active-directory-saas-veracode-tutorial/IC802909.png "Sovelluksen asetusten määrittäminen")

4.  **Määritä kertakirjautumisen osoitteessa Veracode** -sivulla Lataa sertifikaatti, valitse **Lataa**, ja Tallenna sertifikaattitiedosto paikallisesti tietokoneeseen.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-veracode-tutorial/IC802910.png "Kertakirjautumisen määrittäminen")

5.  Eri selainikkunassa Kirjaudu Veracode yrityksesi sivuston järjestelmänvalvojan oikeuksilla.

6.  Valitse ylä-valikossa **asetukset**ja valitse sitten **järjestelmänvalvoja**.

    ![Hallinta] (./media/active-directory-saas-veracode-tutorial/IC802911.png "Hallinta")

7.  Valitse **SAML** -välilehti.

8.  Valitse **Organisaation SAML asetukset** -kohdassa toimimalla seuraavasti:

    ![Hallinta] (./media/active-directory-saas-veracode-tutorial/IC802912.png "Hallinta")

    1.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa Veracode** -valintasivun kopioi **Myöntäjä URL-osoite** -arvo ja liitä se **myöntäjä** -tekstiruutu
    2.  Voit ladata ladatut varmenteen, valitsemalla **Tiedosto**.
    3.  Valitse **Ota käyttöön automaattinen rekisteröinti**.

9.  Valitse **Itse rekisteröinnin asetukset** -kohdassa seuraavien toimien ja valitse sitten **Tallenna**:

    ![Hallinta] (./media/active-directory-saas-veracode-tutorial/IC802913.png "Hallinta")

    1.  Valitse **Uuden käyttäjän aktivointi** **Ei aktivoitava**.
    2.  **Käyttäjän tietojen päivittämisen**Valitse **Asetus Veracode käyttäjätiedot**.
    3.  **SAML-määritteen tiedot**Valitse seuraavasti:
        -   **Liiketoiminta-roolien**
        -   **Käytäntöjen valvoja**
        -   **Tarkistaja**
        -   **Suojauksen liidi**
        -   **Johdon**
        -   **Lähettäjä**
        -   **Tekijä**
        -   **Kaikki skannata tyypit**
        -   **Ryhmän jäsenyyksiä**
        -   **Oletusryhmä**

10. Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Valmis** **Määrittäminen kertakirjautuminen** -valintaikkunassa Sulje.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-veracode-tutorial/IC802914.png "Kertakirjautumisen määrittäminen")

11. Valitse ylä-valikossa **määritteet** **SAML suojaustunnuksen määritteet** -valintaikkunan avaaminen.

    ![Määritteet] (./media/active-directory-saas-veracode-tutorial/IC795920.png "Määritteet")

12. Lisää pakollinen määrite yhdistämismääritykset, toimi seuraavasti:

    ![Määritteet] (./media/active-directory-saas-veracode-tutorial/IC802906.png "Määritteet")

  	| Määritteen nimi | Määritteen arvo |
  	|:---------------|:----------------|
  	| Etunimi      | User.givenName  |
  	| Sukunimi       | User.surname    |
  	| sähköpostin          | User.Mail       |

    1.  Tietoja kullekin riville edellä olevan taulukon Valitse **Lisää käyttäjä-määrite**.
    
    2.  Kirjoita määritteen nimi näkyy kyseisen rivin **Määritteen nimi** -tekstiruutuun.

    3.  Valitse rivin määritteen arvo **Määritteen arvo** -tekstiruutuun.

    4.  Valitse **Valmis**.

13. Valitse **Ota muutokset käyttöön**.

##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu
  
Jos haluat ottaa käyttöön Azure AD-käyttäjät voivat kirjautua sisään Veracode, ne on valmisteltava Veracode kyselyjä.  
Veracode valmistelu on Automaattinen tehtävä.  
Ei, ei toimi...
  
Käyttäjät luodaan automaattisesti tarvittaessa ensimmäinen yksittäisen Sign yritys aikana.

>[AZURE.NOTE] Voit käyttää mitä tahansa muita Veracode käyttäjän tilin luontityökalut tai ohjelmointirajapinnan myöntämä Veracode säännöstä AAD käyttäjätileille.

##<a name="assigning-users"></a>Käyttäjien määrittäminen
  
Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-veracode-perform-the-following-steps"></a>Jos haluat määrittää käyttäjiä Veracode, toimi seuraavasti:

1.  Testaa tilin luominen Azure perinteinen portaalissa.

2.  Valitse integration **Veracode **sovellus-sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-veracode-tutorial/IC802915.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Kyllä] (./media/active-directory-saas-veracode-tutorial/IC767830.png "Kyllä")
  
Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli. Katso lisätietoja Access-paneelin [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).