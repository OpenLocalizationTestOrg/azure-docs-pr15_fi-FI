<properties 
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi TOPdesk - suojatun | Microsoft Azure"
    description="Opettele käyttämään TOPdesk - suojatun Azure Active Directoryn käyttöön kertakirjautumisen, automaattinen valmistelu ja muiden kanssa!." 
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

#<a name="tutorial-azure-active-directory-integration-with-topdesk---secure"></a>Opetusohjelma: TOPdesk - suojatun Azure Active Directory-integrointi
  
Tässä opetusohjelmassa tavoitteena on näyttämään Azure ja TOPdesk - suojatun integrointi.  
Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   A TOPdesk - suojatun kertakirjautumisen käytössä tilaus
  
Kun olet suorittanut Tässä opetusohjelmassa, olet määrittänyt TOPdesk - suojatun Azure AD-käyttäjien saa oikeuden kertakirjauksen sovellukseen, että TOPdesk - suojatun yrityksen sivuston (palveluntarjoajan aloittanut allekirjoitus-) tai käyttämällä [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).
  
Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1.  TOPdesk - suojatun sovelluksen integrointi ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Määrittäminen käyttäjän valmistelu
4.  Käyttäjien määrittäminen

![Skenaario] (./media/active-directory-saas-topdesk-secure-tutorial/IC790596.png "Skenaario")

##<a name="enabling-the-application-integration-for-topdesk---secure"></a>TOPdesk - suojatun sovelluksen integrointi ottaminen käyttöön
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön TOPdesk - suojatun sovelluksen integrointi.

###<a name="to-enable-the-application-integration-for-topdesk---secure-perform-the-following-steps"></a>TOPdesk - sovelluksen-integroinnin valitseminen käyttöön suojattu, toimi seuraavasti:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-topdesk-secure-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-topdesk-secure-tutorial/IC700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-topdesk-secure-tutorial/IC749321.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-topdesk-secure-tutorial/IC749322.png "Lisää sovellus-gallerry")

6.  Kirjoita **hakuruutuun** **TOPdesk - suojattu**.

    ![Sovelluksen valikoima] (./media/active-directory-saas-topdesk-secure-tutorial/IC790597.png "Sovelluksen valikoima")

7.  Valitse tulosruudussa **TOPdesk - suojatun**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![TOPdesk - suojatun] (./media/active-directory-saas-topdesk-secure-tutorial/IC791933.png "TOPdesk - suojatun")

##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen
  
Tässä osassa on jäsentäminen ottamisesta käyttöön käyttäjien todentamiseen TOPdesk - tilin käyttämällä SAML-protokollan perusteella federation Azure AD-suojaaminen.  
Määritettäessä kertakirjautumisen varten TOPdesk - suojatun edellyttää, että logon kuvaketta tiedoston lataaminen. Saat kuvaketta tiedoston-yhteyden TOPdesk-tukeen.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Kirjaudu **TOPdesk - suojatun** yrityksesi sivuston järjestelmänvalvojan oikeuksilla.

2.  Valitse **TOPdesk** -valikossa **asetukset**.

    ![Asetukset] (./media/active-directory-saas-topdesk-secure-tutorial/IC790598.png "Asetukset")

3.  Valitse **Kirjaudu sisään-asetukset**.

    ![Kirjaudu sisään-asetukset] (./media/active-directory-saas-topdesk-secure-tutorial/IC790599.png "Kirjaudu sisään-asetukset")

4.  Laajenna **Kirjautuminen asetukset** -valikko ja valitse sitten **Yleiset**.

    ![Yleiset] (./media/active-directory-saas-topdesk-secure-tutorial/IC790600.png "Yleiset")

5.  **SAML-kirjautuminen** Tietolähdemääritykset-osasta **suojatun** -osassa seuraavasti:

    ![Tekniset asetukset] (./media/active-directory-saas-topdesk-secure-tutorial/IC790855.png "Tekniset asetukset")

    1.  Valitsemalla **Lataa** Lataa julkisen metatieto-tiedosto ja tallenna se paikallisesti tietokoneesta.
    2.  Avaa metatieto-tiedosto ja Etsi **AssertionConsumerService** -solmu.
        ![Vahvistus kuluttaja-palvelu] (./media/active-directory-saas-topdesk-secure-tutorial/IC790856.png "Vahvistus kuluttaja-palvelu")
    3.  Kopioi **AssertionConsumerService** -arvo.  

        >[AZURE.NOTE] Sinun on jäljempänä tässä opetusohjelmassa- **Määrittäminen sovelluksen URL-osoite** -kohtaan arvo.

6.  Eri selainikkunassa Kirjaudu **Azure perinteinen portaaliin** järjestelmänvalvojana.

7.  Valitse **TOPdesk - suojatun** sovelluksen integrointi-sivulla **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-topdesk-secure-tutorial/IC790602.png "Kertakirjautumisen määrittäminen")

8.  **Miten käyttäjät voivat kirjautua TOPdesk - suojatun haluat käyttää** sivulla Valitse **Microsoft Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-topdesk-secure-tutorial/IC790603.png "Kertakirjautumisen määrittäminen")

9.  Tee **Määrittäminen sovelluksen URL-osoite** -sivulla seuraavat toimet:

    ![Sovelluksen URL-Osoitteen määrittäminen] (./media/active-directory-saas-topdesk-secure-tutorial/IC790604.png "Sovelluksen URL-Osoitteen määrittäminen")

    1.  Kirjoita **TOPdesk - suojatun merkki-URL** -tekstiruutuun käyttävät käyttäjät pääse kirjautumaan sisään TOPdesk - sovelluksen suojaus URL-osoite (esimerkiksi: "*https://qssolutions.topdesk.net*").
    2.  Liitä **TOPdesk – Public vastaa URL** -tekstiruutuun **TOPdesk - suojatun AssertionConsumerService URL-osoite** (esimerkiksi: "*https://qssolutions.topdesk.net/tas/public/login/saml*")
    3.  Valitse **Seuraava**.

10. **Määritä kertakirjautumisen osoitteessa TOPdesk - suojatun** -sivulla lataamaan metatieto-tiedosto valitsemalla **Lataa metatietojen**, ja tallenna sitten tiedosto paikallisesti tietokoneeseen.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-topdesk-secure-tutorial/IC790605.png "Kertakirjautumisen määrittäminen")

11. Luo sertifikaattitiedosto, toimi seuraavasti:

    ![Varmenne] (./media/active-directory-saas-topdesk-secure-tutorial/IC790606.png "Varmenne")

    1.  Avaa ladatut metatieto-tiedosto.
    2.  Laajenna **RoleDescriptor** solmu **xsi: type** , jonka **fed: ApplicationServiceType**.
    3.  Kopioi **x 509** -solmu arvo.
    4.  Tallenna kopioidun **x 509** arvon paikallisesti tietokoneeseen, tiedostossa.

12. Valitse TOPdesk - suojatun yrityksen sivuston **TOPdesk** -valikosta **asetukset**.

    ![Asetukset] (./media/active-directory-saas-topdesk-secure-tutorial/IC790598.png "Asetukset")

13. Valitse **Kirjaudu sisään-asetukset**.

    ![Kirjaudu sisään-asetukset] (./media/active-directory-saas-topdesk-secure-tutorial/IC790599.png "Kirjaudu sisään-asetukset")

14. Laajenna **Kirjautuminen asetukset** -valikko ja valitse sitten **Yleiset**.

    ![Yleiset] (./media/active-directory-saas-topdesk-secure-tutorial/IC790600.png "Yleiset")

15. **Julkiseen** osaan Valitse **Lisää**.

    ![Lisää] (./media/active-directory-saas-topdesk-secure-tutorial/IC790607.png "Lisää")

16. Suorita **SAML määritysten avustaja** -sivulla seuraavat toimet:

    ![SAML määritysten avustaja] (./media/active-directory-saas-topdesk-secure-tutorial/IC790608.png "SAML määritysten avustaja")

    1.  Voit ladata ladatut metatiedot-tiedoston **Federation metatiedot**-kohdassa valitsemalla **Selaa**.
    2.  **Lataa, sertifikaattitiedosto **Varmenne (RSA)**, valitse Selaa.**
    3.  Olet saanut TOPdesk-tukitiimiltä **Logo-kuvaketta**, valitse logotiedoston lataaminen valitsemalla **Selaa**.
    4.  Kirjoita **käyttäjän nimimäärite** -tekstiruutuun **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.
    5.  Kirjoita **Näyttönimi** -tekstiruutuun kokoonpanosi nimi.
    6.  Valitse **Tallenna**.

17. Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Valmis** **Määrittäminen kertakirjautuminen** -valintaikkunassa Sulje.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-topdesk-secure-tutorial/IC790609.png "Kertakirjautumisen määrittäminen")

##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu
  
Jotta Azure AD-käyttäjät voivat kirjautua TOPdesk - turvallinen, ne on valmisteltava TOPdesk - suojatun kyselyjä.  
TOPdesk - turvallinen, valmistelu on manuaalinen tehtävä.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Voit määrittää käyttäjän valmistelu toimimalla seuraavasti:

1.  Kirjaudu **TOPdesk - suojatun** yrityksesi sivuston järjestelmänvalvojana.

2.  Valitse ylä-valikossa **TOPdesk \> uusi \> tukitiedostojen \> operaattori**.

    ![Operaattori] (./media/active-directory-saas-topdesk-secure-tutorial/IC790610.png "Operaattori")

3.  Valitse **Uusi operaattori** -valintaikkunassa seuraavasti:

    ![Uusi operaattori] (./media/active-directory-saas-topdesk-secure-tutorial/IC790611.png "Uusi operaattori")

    1.  Valitse Yleiset-välilehti.
    2.  Kirjoita **Yleiset** -osaan **Sukunimi** -tekstiruutuun sukunimen haluat säätää kelvollinen Azure Active Directory-tili.
    3.  Valitse **sivuston** tilin **sijainti** -osio.
    4.  Kirjoita käyttäjän kirjautumisnimi **TOPdesk kirjautuminen** osan **Kirjautuminen nimi** -tekstiruutuun.
    5.  Valitse **Tallenna**.

>[AZURE.NOTE] Voit käyttää mitä tahansa muita TOPdesk - suojatun käyttäjän tilin luontityökalut tai API-TOPdesk - suojatun valmistelu AAD käyttäjätilien myöntämä.

##<a name="assigning-users"></a>Käyttäjien määrittäminen
  
Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-topdesk---secure-perform-the-following-steps"></a>Voit määrittää käyttäjät TOPdesk - suojatun, toimimalla seuraavasti:

1.  Testaa tilin luominen Azure perinteinen portaalissa.

2.  Valitse **TOPdesk - suojatun **sovelluksen integrointi-sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-topdesk-secure-tutorial/IC790612.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Kyllä] (./media/active-directory-saas-topdesk-secure-tutorial/IC767830.png "Kyllä")
  
Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli. Katso lisätietoja Access-paneelin [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).