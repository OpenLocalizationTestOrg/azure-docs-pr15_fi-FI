<properties 
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi TOPdesk - julkinen | Microsoft Azure" 
    description="Opettele käyttämään TOPdesk - julkinen Azure Active Directoryn käyttöön kertakirjautumisen, automaattinen valmistelu ja muiden kanssa!" 
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

#<a name="tutorial-azure-directory-integration-with-topdesk---public"></a>Opetusohjelma: TOPdesk - julkinen Azure Directory-integrointi

Tässä opetusohjelmassa tavoitteena on näyttämään Azure ja TOPdesk - julkinen integrointi.  
Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   A TOPdesk - julkinen kertakirjautumisen käytössä tilaus
  
Kun olet suorittanut tämän opetusohjelman, Azure AD-käyttäjien liittämäsi TOPdesk – julkisen saa oikeuden kertakirjauksen osoitteessa oman yrityksen julkisen sivuston (palveluntarjoajan aloittanut allekirjoitus-) tai käyttämällä [Johdanto Access-paneelin](active-directory-saas-access-panel-introduction.md)TOPdesk - sovellukseen.
  
Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1.  TOPdesk - julkinen sovelluksen integrointi ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Määrittäminen käyttäjän valmistelu
4.  Käyttäjien määrittäminen

![Skenaario] (./media/active-directory-saas-topdesk-public-tutorial/IC790613.png "Skenaario")

##<a name="enabling-the-application-integration-for-topdesk---public"></a>TOPdesk - julkinen sovelluksen integrointi ottaminen käyttöön
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön TOPdesk - julkinen sovelluksen integrointi.

###<a name="to-enable-the-application-integration-for-topdesk---public-perform-the-following-steps"></a>TOPdesk - sovelluksen-integroinnin valitseminen käyttöön julkinen, toimi seuraavasti:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-topdesk-public-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-topdesk-public-tutorial/IC700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-topdesk-public-tutorial/IC749321.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-topdesk-public-tutorial/IC749322.png "Lisää sovellus-gallerry")

6.  Kirjoita **hakuruutuun** **TOPdesk - julkinen**.

    ![Sovelluksen valikoima] (./media/active-directory-saas-topdesk-public-tutorial/IC790614.png "Sovelluksen valikoima")

7.  Valitse tulokset-ruudun Valitse **TOPdesk - julkinen**ja valitse sitten **Valmis** , voit lisätä sovelluksen.

    ![TOPdesk julkisen] (./media/active-directory-saas-topdesk-public-tutorial/IC791317.png "TOPdesk julkisen")

##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjien todentamiseen TOPdesk - ja niiden tilin käyttämällä SAML-protokollan perusteella federation Azure AD-julkinen.  
Määritettäessä kertakirjautumisen TOPdesk - julkinen, edellyttää, että logon kuvaketta tiedoston lataaminen. Saat kuvaketta tiedoston-yhteyden TOPdesk-tukeen.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Kirjaudu yrityksesi **TOPdesk - julkisen** sivuston järjestelmänvalvojan oikeuksilla.

2.  Valitse **TOPdesk** -valikossa **asetukset**.

    ![Asetukset] (./media/active-directory-saas-topdesk-public-tutorial/IC790598.png "Asetukset")

3.  Valitse **Kirjaudu sisään-asetukset**.

    ![Kirjaudu sisään-asetukset] (./media/active-directory-saas-topdesk-public-tutorial/IC790599.png "Kirjaudu sisään-asetukset")

4.  Laajenna **Kirjautuminen asetukset** -valikko ja valitse sitten **Yleiset**.

    ![Yleiset] (./media/active-directory-saas-topdesk-public-tutorial/IC790600.png "Yleiset")

5.  **SAML-kirjautuminen** Tietolähdemääritykset-osasta **julkinen** -osassa seuraavasti:

    ![Tekniset asetukset] (./media/active-directory-saas-topdesk-public-tutorial/IC790601.png "Tekniset asetukset")

    1.  Valitsemalla **Lataa** Lataa julkisen metatieto-tiedosto ja tallenna se paikallisesti tietokoneesta.
    2.  Avaa metatieto-tiedosto ja Etsi **AssertionConsumerService** -solmu.
        ![AssertionConsumerService] (./media/active-directory-saas-topdesk-public-tutorial/IC790619.png "AssertionConsumerService")
    3.  Kopioi **AssertionConsumerService** -arvo.  

        >[AZURE.NOTE] Sinun on jäljempänä tässä opetusohjelmassa- **Määrittäminen sovelluksen URL-osoite** -kohtaan arvo.

6.  Eri selainikkunassa Kirjaudu **Azure perinteinen portaaliin** järjestelmänvalvojana.

7.  Valitse **TOPdesk - julkinen** sovelluksen integrointi-sivulla **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-topdesk-public-tutorial/IC790620.png "Kertakirjautumisen määrittäminen")

8.  **Miten käyttäjät voivat kirjautua TOPdesk - julkinen haluat käyttää** sivulla Valitse **Microsoft Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-topdesk-public-tutorial/IC790621.png "Kertakirjautumisen määrittäminen")

9.  Tee **Määrittäminen sovelluksen URL-osoite** -sivulla seuraavat toimet:

    ![Sovelluksen URL-Osoitteen määrittäminen] (./media/active-directory-saas-topdesk-public-tutorial/IC790622.png "Sovelluksen URL-Osoitteen määrittäminen")

    1.  Kirjoita **TOPdesk - julkinen merkki-URL** -tekstiruutuun käytettävä käyttäjien kirjautua TOPdesk - sovelluksen julkisen URL-osoite (esimerkiksi: "*https://qssolutions.topdesk.net*").
    2.  Liitä **TOPdesk – Public vastaa URL** -tekstiruutuun **TOPdesk - julkinen AssertionConsumerService URL-osoite** (esimerkiksi: "*https://qssolutions.topdesk.net/tas/public/login/saml*")
    3.  Valitse **Seuraava**.

10. **Määritä kertakirjautumisen osoitteessa TOPdesk - julkinen** -sivulla lataamaan metatieto-tiedosto valitsemalla **Lataa metatietojen**, ja tallenna sitten tiedosto paikallisesti tietokoneeseen.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-topdesk-public-tutorial/IC790623.png "Kertakirjautumisen määrittäminen")

11. Luo sertifikaattitiedosto, toimi seuraavasti:

    ![Varmenne] (./media/active-directory-saas-topdesk-public-tutorial/IC790606.png "Varmenne")

    1.  Avaa ladatut metatieto-tiedosto.
    2.  Laajenna **RoleDescriptor** solmu **xsi: type** , jonka **fed: ApplicationServiceType**.
    3.  Kopioi **x 509** -solmu arvo.
    4.  Tallenna kopioidun **x 509** arvon paikallisesti tietokoneeseen, tiedostossa.

12. Valitse TOPdesk - yrityksen julkisen sivuston **TOPdesk** -valikosta **asetukset**.

    ![Asetukset] (./media/active-directory-saas-topdesk-public-tutorial/IC790598.png "Asetukset")

13. Valitse **Kirjaudu sisään-asetukset**.

    ![Kirjaudu sisään-asetukset] (./media/active-directory-saas-topdesk-public-tutorial/IC790599.png "Kirjaudu sisään-asetukset")

14. Laajenna **Kirjautuminen asetukset** -valikko ja valitse sitten **Yleiset**.

    ![Yleiset] (./media/active-directory-saas-topdesk-public-tutorial/IC790600.png "Yleiset")

15. **Julkiseen** osaan Valitse **Lisää**.

    ![SAML-kirjautuminen] (./media/active-directory-saas-topdesk-public-tutorial/IC790625.png "SAML-kirjautuminen")

16. Suorita **SAML määritysten avustaja** -sivulla seuraavat toimet:

    ![SAML määritysten avustaja] (./media/active-directory-saas-topdesk-public-tutorial/IC790608.png "SAML määritysten avustaja")

    1.  Voit ladata ladatut metatiedot-tiedoston **Federation metatiedot**-kohdassa valitsemalla **Selaa**.
    2.  **Lataa, sertifikaattitiedosto **Varmenne (RSA)**, valitse Selaa.**
    3.  Olet saanut TOPdesk-tukitiimiltä **Logo-kuvaketta**, valitse logotiedoston lataaminen valitsemalla **Selaa**.
    4.  Kirjoita **käyttäjän nimimäärite** -tekstiruutuun **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.
    5.  Kirjoita **Näyttönimi** -tekstiruutuun kokoonpanosi nimi.
    6.  Valitse **Tallenna**.

17. Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Valmis** **Määrittäminen kertakirjautuminen** -valintaikkunassa Sulje.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-topdesk-public-tutorial/IC790627.png "Kertakirjautumisen määrittäminen")

##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu
  
Jotta Azure AD-käyttäjät voivat kirjautua TOPdesk - julkinen on valmisteltu TOPdesk - julkinen kyselyjä.  
Jos kyseessä on TOPdesk - julkinen valmistelu on manuaalinen tehtävä.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Voit määrittää käyttäjän valmistelu toimimalla seuraavasti:

1.  Kirjaudu yrityksesi **TOPdesk - julkisen** sivuston järjestelmänvalvojana.

2.  Valitse ylä-valikossa **TOPdesk \> uusi \> tukitiedostojen \> henkilön**.

    ![Henkilö] (./media/active-directory-saas-topdesk-public-tutorial/IC790628.png "Henkilö")

3.  Valitse uusi henkilö-valintaikkunassa seuraavasti:

    ![Uuden henkilön] (./media/active-directory-saas-topdesk-public-tutorial/IC790629.png "Uuden henkilön")

    1.  Valitse Yleiset-välilehti.
    2.  Kirjoita kelvollinen Azure Active Directory-tilin haluat säätää sukunimen Sukunimi-tekstiruutuun.
    3.  Valitse **sivuston** tilin.
    4.  Valitse **Tallenna**.

>[AZURE.NOTE] Voit käyttää mitä tahansa muita TOPdesk - julkiset tilin luontityökalut tai ohjelmointirajapinnan myöntämä TOPdesk - julkinen, jos haluat säätää AAD käyttäjätilit.

##<a name="assigning-users"></a>Käyttäjien määrittäminen
  
Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-topdesk---public-perform-the-following-steps"></a>Voit määrittää käyttäjät TOPdesk - julkinen, toimimalla seuraavasti:

1.  Testaa tilin luominen Azure perinteinen portaalissa.

2.  Valitse **TOPdesk - julkinen **sovelluksen integrointi-sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-topdesk-public-tutorial/IC790630.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Kyllä] (./media/active-directory-saas-topdesk-public-tutorial/IC767830.png "Kyllä")
  
Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli. Katso lisätietoja Access-paneelin [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).