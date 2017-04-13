<properties 
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi Learningpool | Microsoft Azure" 
    description="Opettele käyttämään Learningpool Azure Active Directory-hakemistosta käyttöön kertakirjautumisen, automaattinen valmistelu ja lisää!" 
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

#<a name="tutorial-azure-active-directory-integration-with-learningpool"></a>Opetusohjelma: Learningpool Azure Active Directory-integrointi
  
Tässä opetusohjelmassa tavoitteena on näyttämään Azure ja Learningpool integrointi.  
Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   Käytössä Learningpool kertakirjautumisen tilaus
  
Kun olet suorittanut Tässä opetusohjelmassa, olet liittänyt Learningpool Azure AD-käyttäjien saa oikeuden kertakirjauksen Learningpool yrityksesi sivuston (palveluntarjoajan aloittanut allekirjoitus-) tai käyttämällä [Access-paneelin esittely](active-directory-saas-access-panel-introduction.md)-sovellukseen.
  
Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1.  Learningpool sovelluksen integrointi ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Määrittäminen käyttäjän valmistelu
4.  Käyttäjien määrittäminen

![Skenaario] (./media/active-directory-saas-learningpool-tutorial/IC791166.png "Skenaario")
##<a name="enabling-the-application-integration-for-learningpool"></a>Learningpool sovelluksen integrointi ottaminen käyttöön
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön Learningpool sovelluksen integrointi.

###<a name="to-enable-the-application-integration-for-learningpool-perform-the-following-steps"></a>Learningpool sovelluksen integrointi käyttöön toimimalla seuraavasti:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-learningpool-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-learningpool-tutorial/IC700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-learningpool-tutorial/IC749321.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-learningpool-tutorial/IC749322.png "Lisää sovellus-gallerry")

6.  Kirjoita **hakuruutuun** **Learningpool**.

    ![Sovelluksen valikoima] (./media/active-directory-saas-learningpool-tutorial/IC795073.png "Sovelluksen valikoima")

7.  Valitse tulosruudussa **Learningpool**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![Learningpool] (./media/active-directory-saas-learningpool-tutorial/IC809577.png "Learningpool")
##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjien todentamiseen Learningpool käyttämällä SAML-protokollan perusteella federation Azure AD-tilin kanssa.
  
Learningpool sovelluksen odottaa SAML vahvistukset tietyssä muodossa, jossa pyydetään Lisää mukautettujen määritteiden yhdistämismäärityksiä **saml suojaustunnuksen määritteet** -määrityksiin.  
Seuraavassa näyttökuvassa näkyy esimerkki tämän.

![SAML-tunnuksen määritteet] (./media/active-directory-saas-learningpool-tutorial/IC795074.png "SAML-tunnuksen määritteet")

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Valitse Azure perinteinen portaalissa **Learningpool** sovelluksen integrointi-sivun yläreunassa valikosta **SAML suojaustunnuksen määritteet** -valintaikkunan avaaminen **määritteet** .

    ![Määritteet] (./media/active-directory-saas-learningpool-tutorial/IC795075.png "Määritteet")

2.  Lisää pakollinen määrite yhdistämismääritykset, toimi seuraavasti:

    ###  

  	|Määritteen nimi                |Määritteen arvo            |
  	|------------------------------|---------------------------|

     urn: oid:1.2.840.113556.1.4.221 | User.UserPrincipalName
  	|-------------------------------|--------------------------|  
     urn: oid:2.5.4.42|User.givenName   
  	|urn: oid:0.9.2342.19200300.100.1.3|User.Mail
  	|urn: oid:2.5.4.4|User.surname

    1.  Tietoja kullekin riville edellä olevan taulukon Valitse **Lisää käyttäjä-määrite**.
    2.  Kirjoita määritteen nimi näkyy kyseisen rivin **Määritteen nimi** -tekstiruutuun.
    3.  **Määritteen arvo** -luettelosta kyseisen rivin määritteen arvo.
    4.  Valitse **Valmis**.

3.  Valitse **Ota muutokset käyttöön**.

4.  Selaimessa Valitse **takaisin** **Pika-aloitus** -valintaikkunan avaaminen uudelleen.

5.  Valitse **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Määritä Singel Sign-On] (./media/active-directory-saas-learningpool-tutorial/IC795076.png "Määritä Singel Sign-On")

6.  **Miten käyttäjät voivat kirjautua Learningpool haluat käyttää** sivulla Valitse **Microsoft Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-learningpool-tutorial/IC795077.png "Kertakirjautumisen määrittäminen")

7.  Kirjoita **Määritä sovelluksen URL-osoite** -sivulla **Learningpool merkki-URL** -tekstiruutuun käyttämä käyttäjien kirjautua Learningpool sovelluksen URL-osoite (esimerkiksi: https://parliament.preview.learningpool.com/auth/shibboleth/index.php), ja valitse sitten **Seuraava**.

    ![Sovelluksen URL-Osoitteen määrittäminen] (./media/active-directory-saas-learningpool-tutorial/IC795078.png "Sovelluksen URL-Osoitteen määrittäminen")

8.  Valitse **Määritä kertakirjautumisen osoitteessa Learningpool** -sivulla lataamaan metatietojen valitsemalla **Lataa metatietojen**, ja Tallenna sertifikaattitiedosto paikallisesti tietokoneeseen.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-learningpool-tutorial/IC795079.png "Kertakirjautumisen määrittäminen")

9.  Lähetä edelleen metatietojen tiedoston Learningpool tukihenkilöstölle.

    >[AZURE.NOTE]Kertakirjautumisen pitää ottaa käyttöön Learningpool-tukeen.

10. Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Valmis** **Määrittäminen kertakirjautuminen** -valintaikkunassa Sulje.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-learningpool-tutorial/IC795080.png "Kertakirjautumisen määrittäminen")
##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu
  
Jos haluat ottaa käyttöön Azure AD-käyttäjät voivat kirjautua sisään Learningpool, ne on valmisteltava Learningpool kyselyjä.
  
Ei ei toimi, voit määrittää käyttäjän Learningpool valmistelu.  
Käyttäjien on luotava Learningpool tukihenkilöstölle.

>[AZURE.NOTE]Voit käyttää mitä tahansa muita Learningpool käyttäjän tilin luontityökalut tai ohjelmointirajapinnan myöntämä Learningpool säännöstä AAD käyttäjätileille.

##<a name="assigning-users"></a>Käyttäjien määrittäminen
  
Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-learningpool-perform-the-following-steps"></a>Jos haluat määrittää käyttäjiä Learningpool, toimi seuraavasti:

1.  Testaa tilin luominen Azure perinteinen portaalissa.

2.  Valitse integration **Learningpool **sovellus-sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-learningpool-tutorial/IC795081.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Kyllä] (./media/active-directory-saas-learningpool-tutorial/IC767830.png "Kyllä")
  
Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli. Katso lisätietoja Access-paneelin [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).