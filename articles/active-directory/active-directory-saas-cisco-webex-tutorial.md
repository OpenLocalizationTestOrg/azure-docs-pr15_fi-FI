<properties 
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi Cisco Webex | Microsoft Azure" 
    description="Opettele käyttämään Cisco Webex Azure Active Directory-hakemistosta käyttöön kertakirjautumisen, automaattinen valmistelu ja lisää!" 
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

#<a name="tutorial-azure-active-directory-integration-with-cisco-webex"></a>Opetusohjelma: Cisco Webex Azure Active Directory-integrointi

Tässä opetusohjelmassa tavoitteena on näyttämään Azure ja Cisco Webex integrointi.  
Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   Cisco Webex palvelutili

Kun olet suorittanut Tässä opetusohjelmassa, olet liittänyt Cisco Webex Azure AD-käyttäjien saa oikeuden kertakirjauksen sovellukseen Cisco Webex yrityksesi sivuston (palveluntarjoajan aloittanut allekirjoitus-) tai käyttämällä [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).

Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1.  Cisco Webex sovelluksen integrointi ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Määrittäminen käyttäjän valmistelu
4.  Käyttäjien määrittäminen

![Skenaario] (./media/active-directory-saas-cisco-webex-tutorial/IC777614.png "Skenaario")
##<a name="enabling-the-application-integration-for-cisco-webex"></a>Cisco Webex sovelluksen integrointi ottaminen käyttöön

Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön Cisco Webex sovelluksen integrointi.

###<a name="to-enable-the-application-integration-for-cisco-webex-perform-the-following-steps"></a>Cisco Webex sovelluksen integrointi käyttöön toimimalla seuraavasti:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-cisco-webex-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-cisco-webex-tutorial/IC700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-cisco-webex-tutorial/IC749321.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-cisco-webex-tutorial/IC749322.png "Lisää sovellus-gallerry")

6.  Kirjoita **hakuruutuun** **Cisco Webex**.

    ![Sovelluksen valikoima] (./media/active-directory-saas-cisco-webex-tutorial/IC777615.png "Sovelluksen valikoima")

7.  Valitse tulosruudussa **Cisco Webex**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![Cisco Webex] (./media/active-directory-saas-cisco-webex-tutorial/IC777616.png "Cisco Webex")
##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen

Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjien todentamiseen Cisco Webex käyttämällä SAML-protokollan perusteella federation Azure AD-tilin kanssa.  
Kun nämä toimet, sinun on base-64-koodattu varmenteen luominen.  
Jos et ole tottunut näin, katso, [miten voit muuntaa binaariluvun varmenteen tekstitiedostoon](http://youtu.be/PlgrzUZ-Y1o)

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Valitse Azure perinteinen portaalissa integrointi **Cisco Webex** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-cisco-webex-tutorial/IC777617.png "Määritä kertakirjautuminen")

2.  **Miten käyttäjät voivat kirjautua Cisco Webex haluat käyttää** sivulla Valitse **Microsoft Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-cisco-webex-tutorial/IC777618.png "Määritä kertakirjautuminen")

3.  **Määritä sovelluksen URL-osoite** -sivulla suorittamalla seuraavat vaiheet ja valitse sitten **Seuraava**.

    ![Sovelluksen URL-Osoitteen määrittäminen] (./media/active-directory-saas-cisco-webex-tutorial/IC777619.png "Sovelluksen URL-Osoitteen määrittäminen")

    1.  Kirjoita **Hyvinkin muuttaa mielesi URL** -tekstiruutuun Cisco Webex vuokraajan URL-osoite (esimerkiksi: *http://contoso.webex.com*).
    2.  Kirjoita **Cisco Webex vastaa URL** -tekstiruutuun **Cisco Webex AssertionConsumerService URL-osoite** (esimerkiksi: *https://company.webex.com/dispatcher/SAML2AuthService?siteurl=company*).

4.  Valitse **Määritä kertakirjautumisen osoitteessa Cisco Webex** -sivulla Lataa sertifikaatti, valitsemalla **Lataa**, ja Tallenna sertifikaattitiedosto tietokoneeseen.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-cisco-webex-tutorial/IC777620.png "Määritä kertakirjautuminen")

5.  Eri selainikkunassa Kirjaudu Cisco Webex yrityksesi sivuston järjestelmänvalvojan oikeuksilla.

6.  Valitse ylä-valikosta **Sivustonhallinta**.

    ![Sivustonhallinta] (./media/active-directory-saas-cisco-webex-tutorial/IC777621.png "Sivustonhallinta")

7.  Valitse **Sivuston hallinta** -osassa **SSO-määritys**.

    ![SSO-määritys] (./media/active-directory-saas-cisco-webex-tutorial/IC777622.png "SSO-määritys")

8.  Liitetty SSO-määritys Web-osassa seuraavasti:

    ![Liitetty SSO-määritys] (./media/active-directory-saas-cisco-webex-tutorial/IC777623.png "Liitetty SSO-määritys")

    1.  Valitse **SAML 2.0** **Federation protokolla** -luettelosta.
    2.  Luo **Base-64-koodattu** tiedosto ladattu varmennetta.  

        >[AZURE.TIP] Lisätietoja on artikkelissa [muuntaa binaariluvun varmenteen tekstitiedostoon](http://youtu.be/PlgrzUZ-Y1o)

    3.  Avaa base-64-koodattu varmennetta Muistiossa ja kopioimalla sen sisällön.
    4.  **Tuo SAML**metatiedot ja liitä base-64-koodattu varmennetta.
    5.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa Cisco Webex** -valintasivun **Myöntäjä URL-osoite** -arvo kopioi ja liitä se sitten **myöntäjä SAML (IdP tunnus)** -tekstiruutuun.
    6.  Azure perinteinen portaalissa-sivulla **Määritä kertakirjautumisen osoitteessa Cisco Webex** valintaikkunan Kopioi **Remote sisäänkirjautumisen URL-osoite** -arvo ja liitä se **Asiakkaan SSO palvelun kirjautuminen URL** -tekstiruutuun.
    7.  Valitse **sähköpostiosoite** **NameID muoto** -luettelosta.
    8.  Kirjoita **Urn: oasis: nimet: tc: SAML:2.0:ac:classes:Password** **AuthnContextClassRef** -tekstiruutuun.
    9.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa Cisco Webex** -valintasivun **Remote Kirjaudu URL-osoite** -arvo kopioi ja liitä se sitten **Asiakkaan SSO palvelun Kirjaudu URL** -tekstiruutuun.
    10. Valitse **Päivitä**.

9.  Azure perinteinen portaalissa-sivulla **Määritä kertakirjautumisen osoitteessa Cisco Webex** valintaikkuna Valitse Sign määritysten vahvistus ja valitse sitten **Valmis**.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-cisco-webex-tutorial/IC777624.png "Määritä kertakirjautuminen")
##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu

Jotta voit kirjautua Cisco Webex Azure AD-käyttäjien käyttöön, ne on valmisteltava Cisco Webex kyselyjä.  
Cisco Webex valmistelu on manuaalinen tehtävä.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Valmistelu käyttäjätilit, toimi seuraavasti:

1.  Kirjaudu sisään **Cisco Webex** alihallintaan.

2.  Siirry **käyttäjien hallinta \> Lisää käyttäjä**.

    ![Lisää käyttäjiä] (./media/active-directory-saas-cisco-webex-tutorial/IC777625.png "Lisää käyttäjiä")

3.  Lisää käyttäjä-osassa, toimi seuraavasti:

    ![Lisää käyttäjä] (./media/active-directory-saas-cisco-webex-tutorial/IC777626.png "Lisää käyttäjä")

    1.  Valitse **Tilin tyyppi**- **isäntä**.
    2.  Kirjoita aiemmin luodun Azure AD-käyttäjän tiedot seuraavat tekstiruutujen: ** **Etunimi, Sukunimi**, käyttäjänimi**, **sähköpostiosoite**, **salasana**, **Vahvista salasana**.
    3.  Valitse **Lisää**.

>[AZURE.NOTE] Voit käyttää mitä tahansa muita Cisco Webex käyttäjän tilin luontityökalut tai ohjelmointirajapinnan myöntämä Cisco Webex säännöstä AAD käyttäjätileille.

##<a name="assigning-users"></a>Käyttäjien määrittäminen

Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-cisco-webex-perform-the-following-steps"></a>Jos haluat määrittää käyttäjiä Cisco Webex, toimi seuraavasti:

1.  Testaa tilin luominen Azure perinteinen portaalissa.

2.  Valitse integration **Cisco Webex **sovellus-sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-cisco-webex-tutorial/IC777627.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Kyllä] (./media/active-directory-saas-cisco-webex-tutorial/IC767830.png "Kyllä")

Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli. Katso lisätietoja Access-paneelin [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).
