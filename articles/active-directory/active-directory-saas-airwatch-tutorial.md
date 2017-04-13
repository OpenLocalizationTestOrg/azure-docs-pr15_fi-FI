<properties 
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi AirWatch | Microsoft Azure" 
    description="Opettele käyttämään AirWatch Azure Active Directory-hakemistosta käyttöön kertakirjautumisen, automaattinen valmistelu ja lisää!" 
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

#<a name="tutorial-azure-active-directory-integration-with-airwatch"></a>Opetusohjelma: AirWatch Azure Active Directory-integrointi

Tässä opetusohjelmassa tavoitteena on näyttämään Azure ja AirWatch integrointi.  
Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   Käytössä AirWatch kertakirjautumisen tilaus

Kun olet suorittanut Tässä opetusohjelmassa, olet liittänyt AirWatch Azure AD-käyttäjien saa oikeuden kertakirjauksen AirWatch yrityksesi sivuston (palveluntarjoajan aloittanut allekirjoitus-) tai käyttämällä [Access-paneelin esittely](active-directory-saas-access-panel-introduction.md)-sovellukseen.

Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1.  AirWatch sovelluksen integrointi ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Määrittäminen käyttäjän valmistelu
4.  Käyttäjien määrittäminen

![AirWatch] (./media/active-directory-saas-airwatch-tutorial/IC791913.png "AirWatch")
##<a name="enabling-the-application-integration-for-airwatch"></a>AirWatch sovelluksen integrointi ottaminen käyttöön

Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön AirWatch sovelluksen integrointi.

###<a name="to-enable-the-application-integration-for-airwatch-perform-the-following-steps"></a>AirWatch sovelluksen integrointi käyttöön toimimalla seuraavasti:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-airwatch-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-airwatch-tutorial/IC700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-airwatch-tutorial/IC749321.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-airwatch-tutorial/IC749322.png "Lisää sovellus-gallerry")

6.  Kirjoita **hakuruutuun** **AirWatch**.

    ![Sovelluksen valikoima] (./media/active-directory-saas-airwatch-tutorial/IC791914.png "Sovelluksen valikoima")

7.  Valitse tulosruudussa **AirWatch**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![AirWatch] (./media/active-directory-saas-airwatch-tutorial/IC791915.png "AirWatch")
##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen

Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjien todentamiseen AirWatch käyttämällä SAML-protokollan perusteella federation Azure AD-tilin kanssa.  
Tämän toimintosarjan osana on base-64-koodattu sertifikaattitiedosto luomiseen.  
Jos et ole tottunut näin, katso, [miten voit muuntaa binaariluvun varmenteen tekstitiedostoon](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Valitse Azure perinteinen portaalissa integrointi **AirWatch** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-airwatch-tutorial/IC791916.png "Kertakirjautumisen määrittäminen")

2.  **Miten käyttäjät voivat kirjautua AirWatch haluat käyttää** sivulla Valitse **Microsoft Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-airwatch-tutorial/IC791917.png "Kertakirjautumisen määrittäminen")

3.  Kirjoita **Määrittäminen sovelluksen URL-osoite** -sivulla **AirWatch merkki-URL** -tekstiruutuun käyttää käyttäjien kirjautua AirWatch sovelluksen URL-osoite (esimerkiksi: "*https:// companycode.awmdm.com/AirWatch/Login?gid=companycode*"), ja valitse sitten **Seuraava**.

    ![Sovelluksen URL-Osoitteen määrittäminen] (./media/active-directory-saas-airwatch-tutorial/IC791918.png "Sovelluksen URL-Osoitteen määrittäminen")

4.  Valitse **Määritä kertakirjautumisen osoitteessa AirWatch** -sivulla valitsemalla **Lataa**ja Tallenna sertifikaattitiedosto tietokoneeseen.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-airwatch-tutorial/IC791919.png "Kertakirjautumisen määrittäminen")

5.  Eri selainikkunassa Kirjaudu AirWatch yrityksesi sivuston järjestelmänvalvojan oikeuksilla.

6.  Valitse vasemmassa siirtymisruudussa **tilit**ja valitse sitten **Järjestelmänvalvojat**.

    ![Järjestelmänvalvojat] (./media/active-directory-saas-airwatch-tutorial/IC791920.png "Järjestelmänvalvojat")

7.  Laajenna **asetukset** -valikko ja valitse sitten **Hakemistopalvelujen**.

    ![Asetukset] (./media/active-directory-saas-airwatch-tutorial/IC791921.png "Asetukset")

8.  Valitse **käyttäjä** -välilehti **Base DN** -noudetun, Kirjoita toimialuenimi ja valitse sitten **Tallenna**.

    ![Käyttäjän] (./media/active-directory-saas-airwatch-tutorial/IC791922.png "Käyttäjän")

9.  Valitse **palvelin** -välilehti.

    ![Palvelin] (./media/active-directory-saas-airwatch-tutorial/IC791923.png "Palvelin")

10. Suorita seuraavat vaiheet:

    ![Lataa] (./media/active-directory-saas-airwatch-tutorial/IC791924.png "Lataa")

    1.  **Hakemistotyyppi**-kohdassa **ei mitään**.
    2.  Valitse **Käytä SAML todennusta varten**.
    3.  Voit ladata ladatut varmenteen, valitsemalla **Lataa**.

11. **Pyydä** -osassa seuraavasti:

    ![Pyydä] (./media/active-directory-saas-airwatch-tutorial/IC791925.png "Pyydä")

    1.  **Pyynnön sidonta tyyppi**valitsemalla **viesti**.
    2.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa Airwatch** -valintasivun **Sign-palvelun URL** -arvo kopioi ja liitä se sitten **Tunnistetietojen toimittaja yhteen Kirjaudu-URL** -tekstiruutuun.
    3.  **NameID muodossa**Valitse **Sähköpostiosoite**.
    4.  Valitse **Tallenna**.

12. Valitse **käyttäjä** -välilehti uudelleen.

    ![Käyttäjän] (./media/active-directory-saas-airwatch-tutorial/IC791926.png "Käyttäjän")

13. **Määritteen** -osassa seuraavasti:

    ![Määrite] (./media/active-directory-saas-airwatch-tutorial/IC791927.png "Määrite")

    1.  Kirjoita **http://schemas.microsoft.com/identity/claims/objectidentifier** **Objektitunnus** -tekstiruutuun.
    2.  Kirjoita **käyttäjänimi** -tekstiruutuun **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.
    3.  Kirjoita **Näyttönimi** -tekstiruutuun **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.
    4.  Kirjoita **Ensimmäinen nimi** -tekstiruutuun **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.
    5.  Kirjoita **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname** **Sukunimi** -tekstiruutuun.
    6.  Kirjoita **sähköpostiosoite** -tekstiruutuun **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.
    7.  Valitse **Tallenna**.

14. Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Valmis** **Määrittäminen kertakirjautuminen** -valintaikkunassa Sulje.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-airwatch-tutorial/IC791928.png "Kertakirjautumisen määrittäminen")
##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu

Jos haluat ottaa käyttöön Azure AD-käyttäjät voivat kirjautua sisään AirWatch, ne on valmisteltava AirWatch kyselyjä.  
AirWatch valmistelu on manuaalinen tehtävä.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Valmistelu käyttäjätilit, toimi seuraavasti:

1.  Kirjaudu sisään **AirWatch** yrityksesi sivuston järjestelmänvalvojana.

2.  Siirtymisruudun vasemmassa reunassa **tilit**ja valitse sitten **käyttäjät**.

    ![Käyttäjät] (./media/active-directory-saas-airwatch-tutorial/IC791929.png "Käyttäjät")

3.  Valitse **käyttäjät** -valikossa **Luettelonäkymän**ja valitse sitten **Lisää \> Lisää käyttäjä**.

    ![Käyttäjän lisääminen] (./media/active-directory-saas-airwatch-tutorial/IC791930.png "Käyttäjän lisääminen")

4.  Valitse **Lisää / Muokkaa käyttäjää** -valintaikkunassa seuraavasti:

    ![Käyttäjän lisääminen] (./media/active-directory-saas-airwatch-tutorial/IC791931.png "Käyttäjän lisääminen")

    1.  Kirjoita **käyttäjänimi**, **salasana**, **Vahvista salasana**, **Etunimi**, **Sukunimi**, kelvollinen Azure Active Directory-tilin haluat säännöstä kyselyjä liittyvät tekstiruutujen **Sähköpostiosoite** .
    2.  Valitse **Tallenna**.

>[AZURE.NOTE] Voit käyttää mitä tahansa muita AirWatch käyttäjän tilin luontityökalut tai ohjelmointirajapinnan myöntämä AirWatch säännöstä AAD käyttäjätileille.

##<a name="assigning-users"></a>Käyttäjien määrittäminen

Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-airwatch-perform-the-following-steps"></a>Jos haluat määrittää käyttäjiä AirWatch, toimi seuraavasti:

1.  Testaa tilin luominen Azure perinteinen portaalissa.

2.  Valitse integration **AirWatch **sovellus-sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-airwatch-tutorial/IC791932.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Kyllä] (./media/active-directory-saas-airwatch-tutorial/IC767830.png "Kyllä")

Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli. Katso lisätietoja Access-paneelin [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).
