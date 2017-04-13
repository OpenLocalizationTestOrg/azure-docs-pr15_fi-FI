<properties 
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi Concur | Microsoft Azure" 
    description="Opettele käyttämään Concur Azure Active Directory-hakemistosta käyttöön kertakirjautumisen, automaattinen valmistelu ja lisää!" 
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

#<a name="tutorial-azure-active-directory-integration-with-concur"></a>Opetusohjelma: Concur Azure Active Directory-integrointi  


Tässä opetusohjelmassa tavoitteena on näyttämään Azure ja Concur integrointi.  
Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   Valitse Concur palvelutili

Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1.  Concur sovelluksen integrointi ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Määrittäminen käyttäjän valmistelu
4.  Käyttäjien määrittäminen

![Skenaario] (./media/active-directory-saas-concur-tutorial/IC769766.png "Skenaario")

>[AZURE.NOTE] SAML kautta liitetyn SSO Concur tilauksesi määritys on erillinen eli prioriteettitason, joka on otettava yhteyttä Concur suorittamiseen.

##<a name="enabling-the-application-integration-for-concur"></a>Concur sovelluksen integrointi ottaminen käyttöön

Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön Concur sovelluksen integrointi.

###<a name="to-enable-the-application-integration-for-concur-perform-the-following-steps"></a>Concur sovelluksen integrointi käyttöön toimimalla seuraavasti:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-concur-tutorial/IC700993.png "Active Directory")

2.  **Kansion** luettelosta, valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-concur-tutorial/IC700994.png "Sovellukset")

4.  Avaa **Sovelluksen Gallery**, **Lisää App**ja valitse sitten **Lisää sovellus tehtävään organisaatiossani, jos haluat käyttää**.

    ![Mitä haluat tehdä?] (./media/active-directory-saas-concur-tutorial/IC700995.png "Mitä haluat tehdä?")

5.  Kirjoita **hakuruutuun** **Concur**.

    ![Sovelluksen valikoima] (./media/active-directory-saas-concur-tutorial/IC721727.png "Sovelluksen valikoima")

6.  Valitse tulosruudussa **Concur**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![Isäntävaltion] (./media/active-directory-saas-concur-tutorial/IC721728.png "Isäntävaltion")
##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen

Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjien todentamiseen Concur käyttämällä SAML-protokollan perusteella federation Azure AD-tilin kanssa.

>[AZURE.NOTE] SAML kautta liitetyn SSO Concur tilauksesi määritys on erillinen eli prioriteettitason, joka on otettava yhteyttä Concur suorittamiseen.

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Valitse Azure perinteinen portaalissa integrointi **Concur **sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-concur-tutorial/IC769767.png "Määritä kertakirjautuminen")

2.  **Miten käyttäjät voivat kirjautua Concur haluat käyttää** sivulla Valitse **Microsoft Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-concur-tutorial/IC769768.png "Määritä kertakirjautuminen")

3.  **Määritä sovelluksen URL-osoite** -sivulla **Isäntävaltion merkki-URL** -tekstiruutuun Kirjoita concur vuokraajan kirjautumisen URL-osoite ja valitse sitten **Seuraava**: 

    ![Määritä kirjautuminen URL-osoite] (./media/active-directory-saas-concur-tutorial/IC769769.png "Määritä kirjautuminen URL-osoite")

4.  Valitse **Määritä kertakirjautumisen osoitteessa Concur** -sivulla toimimalla seuraavasti.

    ![Määritä kirjautuminen URL-osoite] (./media/active-directory-saas-concur-tutorial/IC769770.png "Määritä kirjautuminen URL-osoite")

    1.  Valitse Lataa metatiedot ja sitten safe datatiedoston tietokoneeseen.
    2.  Yhteyden määrittäminen SSO-vuokraajan Concur-tukeen.
    3.  Valitse Sign määritysten vahvistus ja valitse sitten **Valmis** **Määrittäminen kertakirjautuminen** -valintaikkunassa Sulje.  

    >[AZURE.NOTE] SAML kautta liitetyn SSO Concur tilauksesi määritys on erillinen eli prioriteettitason, joka on otettava yhteyttä Concur suorittamiseen.

##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu

Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön Active Directory-käyttäjätilien Concur valmistelu.

Käyttöön sovellukset kulut palvelussa on on oltava oikein ja käyttäminen WWW-palvelun Järjestelmänvalvoja-profiiliin. Älä lisää olemassa olevaan profiiliin järjestelmänvalvoja, voit käyttää T & E hallintatoimia WS järjestelmänvalvojan rooli.

Isäntävaltion konsulteille tai asiakas-järjestelmänvalvojan on luotava erillinen WWW-palvelun järjestelmänvalvoja-profiilin ja asiakas-järjestelmänvalvoja on käytettävä tämän profiilin Web Services-järjestelmänvalvojan-funktioita (esimerkiksi ottaminen käyttöön sovellukset). Nämä profiilit on säilytettävä erillään asiakkaan järjestelmänvalvojan päivittäinen T & E järjestelmänvalvojaprofiilin (T & E järjestelmänvalvojan profiilin olisi ei ole WSAdmin rooli).

Kun luot profiilin, jota käytetään sovelluksen ottaminen käyttöön, kirjoittaa käyttäjäprofiilin kenttien asiakkaan järjestelmänvalvojan nimi. Tämä määrittää omistajuus profiiliin. Kun profiilia on luotu, asiakkaan on kirjauduttava sisään profiilia napsauttamalla kumppanin sovelluksen sisällä verkkopalvelut-valikosta "*käyttöön*"-painiketta.

Seuraavista syistä tämä toiminto ei pitäisi tehdä ne käyttävät Normaali T & E hallinta profiilin.

1.  Asiakas on oltava haluamasi vaihtoehto valitsee "*Kyllä*"-valintaikkunan-ikkunasta, joka tulee näkyviin, kun sovellus on otettu käyttöön. Valitsemalla hyväksyy sen asiakas on valmis kumppanin sovelluksen tietoihinsa, joten kumppanin et voi napsauttaa, että Kyllä-painiketta.
2.  Jos asiakas-järjestelmänvalvoja, joka on ottanut käyttöön sovelluksen käyttämällä T & E-järjestelmänvalvojan profiilin jättää yrityksen (tuloksena on parhaillaan käytöstä profiilin)-sovellusten käyttöön käyttämällä, että profiilin eivät toimi, ennen kuin sovellus otetaan käyttöön toisen aktiivinen WS Järjestelmänvalvoja-profiiliin. Tämän vuoksi on määritetty luoda eri WS järjestelmänvalvojan profiileja.
3.  Jos järjestelmänvalvoja jättää yrityksen, WS Järjestelmänvalvoja-profiiliin määritetty nimi voi muuttaa korvaava-järjestelmänvalvojan tarvittaessa ilman vaikuttavat käytössä sovelluksen koska profiilin ei tarvitse poistaa käytöstä

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Voit määrittää käyttäjän valmistelu toimimalla seuraavasti:

1.  Kirjaudu sisään **Concur** alihallintaan.

2.  Valitse **hallinta** -valikosta **Internet-palvelut**.

    ![Concur vuokraajan] (./media/active-directory-saas-concur-tutorial/IC721729.png "Concur vuokraajan")

3.  Valitse vasemmanpuoleisessa **Verkkopalvelut** -ruudun **Käyttöön kumppanin-sovellusta**.

    ![Kumppanin-sovellus] (./media/active-directory-saas-concur-tutorial/IC721730.png "Kumppanin-sovellus")

4.  **Ottaa käyttöön sovelluksen** tehtäväluettelosta Valitse **Azure Active Directory**ja valitse sitten **Ota käyttöön**.

    ![Microsoft Azure Active Directory] (./media/active-directory-saas-concur-tutorial/IC721731.png "Microsoft Azure Active Directory")

5.  Valitse **Kyllä** **Vahvista toiminto** -valintaikkunassa Sulje.

    ![Vahvista toiminto] (./media/active-directory-saas-concur-tutorial/IC721732.png "Vahvista toiminto")

6.  Azure perinteinen portaalissa sovellusten luettelosta **Concur** , voit avata **Concur** valintasivun.

7.  Avaa **Määritä käyttäjän valmistelu** valintasivun, valitsemalla **Määritä käyttäjän valmistelu**.

8.  Kirjoita käyttäjänimi ja salasana Concur järjestelmänvalvojan ja valitse sitten **Seuraava**.

9.  Lopeta määritystä, valitse **Vahvista** -sivulla valitsemalla **Valmis** .

Voit nyt luoda Testaa tilin, odota 10 minuuttia ja varmista, että tili on synkronoitu Concur.
##<a name="assigning-users"></a>Käyttäjien määrittäminen

Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-concur-perform-the-following-steps"></a>Jos haluat määrittää käyttäjiä Concur, toimi seuraavasti:

1.  Testaa tilin luominen Azure perinteinen portaalissa.

2.  Valitse integration **Concur **sovellus-sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-concur-tutorial/IC769771.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Kyllä] (./media/active-directory-saas-concur-tutorial/IC767830.png "Kyllä")

Pitäisi nyt Odota 10 minuuttia ja varmista, että tili on synkronoitu Concur.

Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli. Katso lisätietoja Access-paneelin [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).
