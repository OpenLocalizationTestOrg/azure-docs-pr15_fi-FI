<properties 
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi Zoho sähköpostin | Microsoft Azure" 
    description="Opettele käyttämään Zoho sähköpostin Azure Active Directory-hakemistosta käyttöön kertakirjautumisen, automaattinen valmistelu ja lisää!." 
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
    ms.author="markvi" />

#<a name="tutorial-azure-active-directory-integration-with-zoho-mail"></a>Opetusohjelma: Zoho sähköpostin Azure Active Directory-integrointi
  
Tässä opetusohjelmassa tavoitteena on näyttämään Azure ja Zoho sähköpostin integrointi.  
Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   Zoho sähköpostin palvelutili
  
Kun olet suorittanut Tässä opetusohjelmassa, olet liittänyt Zoho sähköpostin Azure AD-käyttäjien saa oikeuden kertakirjauksen Zoho sähköpostin yrityksesi sivuston (palveluntarjoajan aloittanut allekirjoitus-) tai käyttämällä [Access-paneelin esittely](active-directory-saas-access-panel-introduction.md)-sovellukseen.
  
Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1.  Sovelluksen integrointi Zoho sähköpostin ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Määrittäminen käyttäjän valmistelu
4.  Käyttäjien määrittäminen

![Skenaario] (./media/active-directory-saas-zoho-mail-tutorial/IC789600.png "Skenaario")

##<a name="enabling-the-application-integration-for-zoho-mail"></a>Sovelluksen integrointi Zoho sähköpostin ottaminen käyttöön
  
Tässä osassa tavoitteena on Jäsennys, miten voit ottaa käyttöön sovelluksen integrointi Zoho posti.

###<a name="to-enable-the-application-integration-for-zoho-mail-perform-the-following-steps"></a>Ottaa käyttöön sovelluksen integroinnin Zoho posti, toimi seuraavasti:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-zoho-mail-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-zoho-mail-tutorial/IC700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-zoho-mail-tutorial/IC749321.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-zoho-mail-tutorial/IC749322.png "Lisää sovellus-gallerry")

6.  Kirjoita **hakuruutuun** **Zoho sähköposti**.

    ![Sovelluksen valikoima] (./media/active-directory-saas-zoho-mail-tutorial/IC789601.png "Sovelluksen valikoima")

7.  Valitse tulokset-ruudun **Zoho**posti ja valitse sitten **Valmis** , voit lisätä sovelluksen.

    ![Zoho sähköposti] (./media/active-directory-saas-zoho-mail-tutorial/IC789602.png "Zoho sähköposti")

##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjien todentamiseen Zoho sähköpostin Azure AD käyttämällä SAML-protokollan perusteella federation niiden tilillä.  
Tämän toimintosarjan osana on base-64-koodattu sertifikaattitiedosto luomiseen.  
Jos et ole tottunut näin, katso, [miten voit muuntaa binaariluvun varmenteen tekstitiedostoon](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Valitse Azure perinteinen portaalissa **Zoho Mail** -sovellus integrointi-sivulla **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-zoho-mail-tutorial/IC789603.png "Kertakirjautumisen määrittäminen")

2.  **Miten käyttäjät voivat kirjautua Zoho sähköpostin haluat käyttää** sivulla Valitse **Microsoft Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-zoho-mail-tutorial/IC789604.png "Kertakirjautumisen määrittäminen")

3.  Tee **Määrittäminen sovelluksen URL-osoite** -sivulla seuraavat toimet:

    ![Sovelluksen URL-Osoitteen määrittäminen] (./media/active-directory-saas-zoho-mail-tutorial/IC789605.png "Sovelluksen URL-Osoitteen määrittäminen")

    a. Kirjoita **Zoho sähköpostin merkki-URL** -tekstiruutuun URL-osoite käyttämällä seuraavaa kaavaa:`http://<company name>.ZohoMail.com`

    b. Valitse **Seuraava**.


4.  Valitse **Määritä kertakirjautumisen osoitteessa Zoho posti** -sivulla valitsemalla **Lataa**ja Tallenna sertifikaattitiedosto tietokoneeseen.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-zoho-mail-tutorial/IC789606.png "Kertakirjautumisen määrittäminen")

5.  Eri selainikkunassa Kirjaudu Zoho sähköpostin yrityksesi sivuston järjestelmänvalvojan oikeuksilla.

6.  Valitse **Ohjauspaneeli**.

    ![Ohjauspaneeli] (./media/active-directory-saas-zoho-mail-tutorial/IC789607.png "Ohjauspaneeli")

7.  Valitse **SAML todennus** -välilehti.

    ![SAML-todennus] (./media/active-directory-saas-zoho-mail-tutorial/IC789608.png "SAML-todennus")

8.  **SAML todennus tiedot** -kohdassa toimimalla seuraavasti:

    ![SAML-todennuksen tiedot] (./media/active-directory-saas-zoho-mail-tutorial/IC789609.png "SAML-todennuksen tiedot")

    1.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa Zoho posti** -valintasivun **Remote sisäänkirjautumisen URL-osoite** -arvo kopioi ja liitä se sitten **Kirjautuminen URL** -tekstiruutuun.
    2.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa Zoho posti** -valintasivun **Remote Kirjaudu URL-osoite** -arvo kopioi ja liitä se sitten **Kirjaudu ulos URL** -tekstiruutuun.
    3.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa Zoho posti** -valintasivun **Muuta salasana URL-osoite** -arvo kopioi ja liitä se sitten **Vaihda salasana URL** -tekstiruutuun.
    4.  Luo **base-64-koodattu** tiedosto ladattu varmennetta.  

        >[AZURE.TIP] Lisätietoja on artikkelissa [muuntaa binaariluvun varmenteen tekstitiedostoon](http://youtu.be/PlgrzUZ-Y1o)

    5.  Avaa base-64-koodattu varmennetta Muistiossa, kopioi Leikepöydän sisällön se ja liitä se **PublicKey** -tekstiruutuun.
    6.  **Algoritmin**Valitse **RSA**.
    7.  Valitse **OK**.

9.  Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Valmis** **Määrittäminen kertakirjautuminen** -valintaikkunassa Sulje.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-zoho-mail-tutorial/IC789610.png "Kertakirjautumisen määrittäminen")

##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu
  
Jos haluat ottaa käyttöön Azure AD-käyttäjät voivat kirjautua Zoho sähköpostiviestiin, ne on valmisteltava Zoho sähköpostiviestiin.  
Sähköpostin Zoho valmistelu on manuaalinen tehtävä.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Valmistelu käyttäjätilit, toimi seuraavasti:

1.  Kirjaudu sisään **Zoho sähköpostin** yrityksesi sivuston järjestelmänvalvojan oikeuksilla.

2.  Siirry **Ohjauspaneeli \> sähköposti- ja tiedostojen**.

3.  Siirry **käyttäjätiedot \> Lisää käyttäjä**.

    ![Käyttäjän lisääminen] (./media/active-directory-saas-zoho-mail-tutorial/IC789611.png "Käyttäjän lisääminen")

4.  Valitse **Lisää käyttäjiä** -valintaikkunassa seuraavasti:

    ![Käyttäjän lisääminen] (./media/active-directory-saas-zoho-mail-tutorial/IC789612.png "Käyttäjän lisääminen")

    1.  Kirjoita **Etunimi**, **Sukunimi**, **Sähköpostitunnus**, haluat säännöstä kyselyjä liittyvät tekstiruutujen kelvollinen Azure Active Directory-tilin **salasana** .
    2.  Valitse **OK**.  

        >[AZURE.NOTE] Azure Active Directory-tilin omistaja saavat sähköpostiviestin linkin Vahvista tili, ennen kuin se on aktiivinen.

>[AZURE.NOTE] Voit käyttää mitä tahansa muita Zoho sähköpostin käyttäjän tilin luontityökalut tai ohjelmointirajapinnan myöntämä Zoho sähköpostin säännöstä AAD käyttäjätileille.

##<a name="assigning-users"></a>Käyttäjien määrittäminen
  
Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-zoho-mail-perform-the-following-steps"></a>Jos haluat määrittää käyttäjiä Zoho posti, toimi seuraavasti:

1.  Testaa tilin luominen Azure perinteinen portaalissa.

2.  Valitse integration **Zoho Mail **-sovellus-sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-zoho-mail-tutorial/IC789613.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Kyllä] (./media/active-directory-saas-zoho-mail-tutorial/IC767830.png "Kyllä")
  
Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli. Katso lisätietoja Access-paneelin [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).