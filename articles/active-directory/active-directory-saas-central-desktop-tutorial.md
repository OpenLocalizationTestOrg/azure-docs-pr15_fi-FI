<properties 
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi keskitetyn työpöydän | Microsoft Azure" 
    description="Opettele käyttämään keskitetyn työpöydän Azure Active Directory-hakemistosta käyttöön kertakirjautumisen, automaattinen valmistelu ja lisää!" 
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

#<a name="tutorial-azure-active-directory-integration-with-central-desktop"></a>Opetusohjelma: Keskitetty työpöydän Azure Active Directory-integrointi

Tässä opetusohjelmassa tavoitteena on näyttämään Azure ja keskitetyn työpöydän integrointi. Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   Käytössä-tilauksessa keskitetyn työpöydän kertakirjauksen / keskitetyn työpöydän vuokraaja

Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1.  Sovelluksen integrointi keskitetyn työpöydän ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Määrittäminen käyttäjän valmistelu
4.  Käyttäjien määrittäminen

![Skenaario] (./media/active-directory-saas-central-desktop-tutorial/IC769558.png "Skenaario")
##<a name="enabling-the-application-integration-for-central-desktop"></a>Sovelluksen integrointi keskitetyn työpöydän ottaminen käyttöön

Tässä osassa tavoitteena on Jäsennys, miten voit ottaa käyttöön sovelluksen integrointi: keskitetyn työpöytäversion.

###<a name="to-enable-the-application-integration-for-central-desktop-perform-the-following-steps"></a>Ottaa käyttöön sovelluksen integroinnin: keskitetyn työpöytäversion, toimi seuraavasti:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-central-desktop-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-central-desktop-tutorial/IC700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-central-desktop-tutorial/IC749321.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-central-desktop-tutorial/IC749322.png "Lisää sovellus-gallerry")

6.  Kirjoita **hakuruutuun**, **Keskitetty työpöytä**.

    ![Sovelluksen valikoima] (./media/active-directory-saas-central-desktop-tutorial/IC769559.png "Sovelluksen valikoima")

7.  Valitse tulokset-ruudun valitsemalla **Keskitetyn**ja valitse sitten **Valmis** , voit lisätä sovelluksen.

    ![Keskitetyn työpöydällä] (./media/active-directory-saas-central-desktop-tutorial/IC769560.png "Keskitetyn työpöydällä")
##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen

Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjien todentamiseen keskitetyn työpöydälle niiden tilillä, käyttämällä SAML-protokollan perusteella federation Azure AD.  
Tämän toimintosarjan osana on base-64-koodattu varmenteen lataaminen keskitetyn työpöydän alihallintaan.  
Jos et ole tottunut näin, katso, [miten voit muuntaa binaariluvun varmenteen tekstitiedostoon](http://youtu.be/PlgrzUZ-Y1o).



###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Valitse Azure perinteinen portaalissa integrointi **Keskitetyn työpöydän** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-central-desktop-tutorial/IC749323.png "Määritä kertakirjautuminen")

2.  **Miten haluat käyttäjien kirjautua keskitetyn työpöydälle** sivulla Valitse **Microsoft Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-central-desktop-tutorial/IC777628.png "Määritä kertakirjautuminen")

3.  **Määritä sovelluksen URL-osoite** -sivulla suorittamalla seuraavat vaiheet ja valitse sitten **Seuraava**: 

    -   Kirjoita **Keskitetyn työpöydän merkki-URL** -tekstiruutuun keskitetyn työpöytä-vuokraajan URL-osoite (esimerkiksi: *http://contoso.centraldesktop.com*).
    -   Kirjoita keskitetyn työpöydän vastaa URL-tekstiruutuun keskitetyn työpöydän AssertionConsumerService URL-osoite (esimerkiksi: https://contoso.centraldesktop.com/saml2-assertion.php).

    >[AZURE.NOTE] Saat arvon keskitetyn työpöydän metatiedot (esimerkiksi: *http://contoso.centraldesktop.com*).

    ![Sovelluksen URL-Osoitteen määrittäminen] (./media/active-directory-saas-central-desktop-tutorial/IC769561.png "Sovelluksen URL-Osoitteen määrittäminen")

4.  **Määritä kertakirjautumisen osoitteessa keskitetyn työpöytä** -sivulla Lataa sertifikaatti, valitse **Lataa**, ja Tallenna sertifikaattitiedosto tietokoneeseen.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-central-desktop-tutorial/IC769562.png "Määritä kertakirjautuminen")

5.  Kirjaudu sisään **Keskitetyn työpöydän** alihallintaan.

6.  Valitse **asetukset**, valitse **Lisäasetukset**ja valitse sitten **Kertakirjautuminen**.

    ![Asennus – Lisäasetukset] (./media/active-directory-saas-central-desktop-tutorial/IC769563.png "Asennus – Lisäasetukset")

7.  Valitse **Yksittäinen merkki-asetukset** -sivun toimimalla seuraavasti:

    ![Kertakirjautuminen asetukset] (./media/active-directory-saas-central-desktop-tutorial/IC769564.png "Kertakirjautuminen asetukset")

    1.  Valitse **Ota käyttöön SAML v2 kertakirjautuminen**.
    2.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa keskitetyn työpöytä** -sivulla **Myöntäjä URL-osoite** -arvo kopioi ja liitä se sitten **SSO URL** -tekstiruutuun.
    3.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa keskitetyn työpöytä** -sivulla **Remote sisäänkirjautumisen URL-osoite** -arvo kopioi ja liitä se sitten **SSO-kirjautumisen URL** -tekstiruutuun.
    4.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa keskitetyn työpöytä** -sivulla kopioi **Yhden Sign-Out palvelun URL-osoite** -arvo ja liitä se sitten **SSO Kirjaudu URL** -tekstiruutuun.

8.  Valitse **Viestin allekirjoitus tarkistustapa** -osassa seuraavasti:

    ![Viestin allekirjoitus tarkistustapa] (./media/active-directory-saas-central-desktop-tutorial/IC769565.png "Viestin allekirjoitus tarkistustapa")

    1.  Valitse **varmenne**.
    2.  Valitse **RSH SHA256** **SSO-todistus** -luettelosta.
    3.  Luo tekstitiedosto ladatut varmenteen tekstitiedoston sisällön kopioiminen ja liittäminen **SSO sertifikaatin** -kenttä.  

        >[AZURE.TIP] Lisätietoja on artikkelissa [muuntaa binaariluvun varmenteen tekstitiedostoon](http://youtu.be/PlgrzUZ-Y1o)

    4.  Valitse **Näytä linkki SAMLv2-kirjautumissivulle**.

9.  Valitse **Päivitä**.

10. Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Valmis** **Määrittäminen kertakirjautuminen** -valintaikkunassa Sulje.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-central-desktop-tutorial/IC769566.png "Määritä kertakirjautuminen")
##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu

AAD käyttäjät voivat kirjautua ne on valmisteltava keskitetyn työpöytä-sovellukseen. Tässä osassa kuvataan, miten AAD Käyttäjätilien luominen keskitetyn työpöytä.

###<a name="to-provision-user-accounts-to-central-desktop"></a>Valmistelu käyttäjätilit keskitetyn työpöydän:

1.  Kirjaudu sisään keskitetyn työpöydän alihallintaan.

2.  Siirry **henkilöiden \> sisäisille jäsenille**.

3.  Valitse **Lisää sisäisille jäsenille**.

    ![Henkilöt] (./media/active-directory-saas-central-desktop-tutorial/IC781051.png "Henkilöt")

4.  Kirjoita **Sähköpostiosoite, uudet jäsenet** -tekstiruutuun AAD tilin haluat säätää, ja valitse sitten **Seuraava**.

    ![Uusien jäsenten sähköpostiosoitteet] (./media/active-directory-saas-central-desktop-tutorial/IC781052.png "Uusien jäsenten sähköpostiosoitteet")

5.  Valitse **Lisää sisäinen jäsenet**.

    ![Lisää sisäinen jäsen] (./media/active-directory-saas-central-desktop-tutorial/IC781053.png "Lisää sisäinen jäsen")

    >[AZURE.NOTE] Olet lisännyt käyttäjät saavat sähköpostiviestin, joka sisältää linkin vahvistus-kiinni tarvittaessa Aktivoi tili valitsemalla.

>[AZURE.NOTE] Voit käyttää mitä tahansa muita keskitetyn työpöydän käyttäjän tilin luontityökalut tai ohjelmointirajapinnan myöntämä keskitetyn työpöydän säännöstä AAD käyttäjätilejä

##<a name="assigning-users"></a>Käyttäjien määrittäminen

Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-central-desktop-perform-the-following-steps"></a>Jos haluat määrittää käyttäjät keskitetyn työpöytä, toimi seuraavasti:

1.  Testaa tilin luominen Azure perinteinen portaalissa.

2.  Valitse **Keskitetyn työpöydän** sovelluksen integrointi-sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-central-desktop-tutorial/IC769567.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Kyllä] (./media/active-directory-saas-central-desktop-tutorial/IC767830.png "Kyllä")

Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli. Katso lisätietoja Access-paneelin [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).
