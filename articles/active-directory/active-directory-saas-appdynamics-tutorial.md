<properties 
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi AppDynamics | Microsoft Azure" 
    description="Opettele käyttämään AppDynamics Azure Active Directory-hakemistosta käyttöön kertakirjautumisen, automaattinen valmistelu ja lisää!" 
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

#<a name="tutorial-azure-active-directory-integration-with-appdynamics"></a>Opetusohjelma: AppDynamics Azure Active Directory-integrointi

Tässä opetusohjelmassa tavoitteena on näyttämään Azure ja AppDynamics integrointi. Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   Käytössä AppDynamics kertakirjautumisen tilaus

Kun olet suorittanut Tässä opetusohjelmassa, olet liittänyt AppDynamics Azure AD-käyttäjien saa oikeuden kertakirjauksen AppDynamics yrityksen sivuston (palveluntarjoajan aloittanut allekirjoitus-) tai käyttämällä [Access-paneelin esittely](active-directory-saas-access-panel-introduction.md)-sovellukseen.

Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1.  AppDynamics sovelluksen integrointi ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Määrittäminen käyttäjän valmistelu
4.  Käyttäjien määrittäminen

![Skenaario] (./media/active-directory-saas-appdynamics-tutorial/IC790209.png "Skenaario")
##<a name="enabling-the-application-integration-for-appdynamics"></a>AppDynamics sovelluksen integrointi ottaminen käyttöön

Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön AppDynamics sovelluksen integrointi.

###<a name="to-enable-the-application-integration-for-appdynamics-perform-the-following-steps"></a>AppDynamics sovelluksen integrointi käyttöön toimimalla seuraavasti:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-appdynamics-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-appdynamics-tutorial/IC700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-appdynamics-tutorial/IC749321.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-appdynamics-tutorial/IC749322.png "Lisää sovellus-gallerry")

6.  Kirjoita **hakuruutuun** **AppDynamics**.

    ![Sovelluksen valikoima] (./media/active-directory-saas-appdynamics-tutorial/IC790210.png "Sovelluksen valikoima")

7.  Valitse tulosruudussa **AppDynamics**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![AppDynamics] (./media/active-directory-saas-appdynamics-tutorial/IC790211.png "AppDynamics")
##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen

Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjien todentamiseen AppDynamics käyttämällä SAML-protokollan perusteella federation Azure AD-tilin kanssa.  
Tämän toimintosarjan osana on base-64-koodattu sertifikaattitiedosto luomiseen.  
Jos et ole tottunut näin, katso, [miten voit muuntaa binaariluvun varmenteen tekstitiedostoon](http://youtu.be/PlgrzUZ-Y1o).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Valitse Azure perinteinen portaalissa integrointi **AppDynamics** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Määrittää yksittäisen rekisteröityminen] (./media/active-directory-saas-appdynamics-tutorial/IC790212.png "Määrittää yksittäisen rekisteröityminen")

2.  **Miten käyttäjät voivat kirjautua AppDynamics haluat käyttää** sivulla Valitse **Microsoft Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Määrittää yksittäisen rekisteröityminen] (./media/active-directory-saas-appdynamics-tutorial/IC790213.png "Määrittää yksittäisen rekisteröityminen")

3.  **Määritä sovelluksen URL-osoite** -sivulla **AppDynamics merkki-URL** -tekstiruutuun Kirjoita käyttäjien Sign AppDynamics ("*https://companyname.saas.appdynamics.com*"), voit käyttää URL-osoite ja valitse sitten **Seuraava**.

    ![Sovelluksen URL-Osoitteen määrittäminen] (./media/active-directory-saas-appdynamics-tutorial/IC790214.png "Sovelluksen URL-Osoitteen määrittäminen")

4.  Valitse **Määritä kertakirjautumisen osoitteessa AppDynamics** -sivulla Lataa sertifikaatti, valitsemalla **Lataa**, ja Tallenna sertifikaattitiedosto tietokoneeseen.

    ![Määrittää yksittäisen rekisteröityminen] (./media/active-directory-saas-appdynamics-tutorial/IC790215.png "Määrittää yksittäisen rekisteröityminen")

5.  Eri selainikkunassa Kirjaudu AppDynamics yrityksesi sivuston järjestelmänvalvojan oikeuksilla.

6.  Yläkulmassa työkalurivin **asetukset**ja valitse sitten **hallinta**.

    ![Hallinta] (./media/active-directory-saas-appdynamics-tutorial/IC790216.png "Hallinta")

7.  Valitse **Käyttöoikeustarkistuspalvelun** -välilehti.

    ![Käyttöoikeustarkistuspalvelun] (./media/active-directory-saas-appdynamics-tutorial/IC790224.png "Käyttöoikeustarkistuspalvelun")

8.  **Käyttöoikeustarkistuspalvelun** -osassa seuraavasti:

    ![SAML-määritys] (./media/active-directory-saas-appdynamics-tutorial/IC790225.png "SAML-määritys")

    1.  Valitse **Käyttöoikeustarkistuspalvelun** **SAML**.
    2.  Azure perinteinen portaalissa-sivulla **Määritä kertakirjautumisen osoitteessa AppDynamics** valintaikkunan Kopioi **Remote sisäänkirjautumisen URL-osoite** -arvo ja liitä se sitten **Kirjautuminen URL** -tekstiruutuun.
    3.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa AppDynamics** -valintasivun **Remote Kirjaudu URL-osoite** -arvo kopioi ja liitä se sitten **Kirjaudu ulos URL** -tekstiruutuun.
    4.  Luo **base-64-koodattu** tiedosto ladattu varmennetta.  

        >[AZURE.TIP] Lisätietoja on artikkelissa [muuntaa binaariluvun varmenteen tekstitiedostoon](http://youtu.be/PlgrzUZ-Y1o)

    5.  Avaa base-64-koodattu varmennetta Muistiossa, kopioi se sisällön Leikepöydän ja liitä se **varmenne** -tekstiruutu
    6.  Valitse **Tallenna**.
        ![Tallenna] (./media/active-directory-saas-appdynamics-tutorial/IC777673.png "Tallenna")

9.  Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Valmis** **Määrittäminen kertakirjautuminen** -valintaikkunassa Sulje.

    ![Määrittää yksittäisen rekisteröityminen] (./media/active-directory-saas-appdynamics-tutorial/IC790226.png "Määrittää yksittäisen rekisteröityminen")
##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu

Jos haluat ottaa käyttöön Azure AD-käyttäjät voivat kirjautua sisään AppDynamics, ne on valmisteltava AppDynamics kyselyjä.  
AppDynamics valmistelu on manuaalinen tehtävä.

###<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Voit määrittää käyttäjän valmistelu toimimalla seuraavasti:

1.  Kirjaudu sisään AppDynamics yrityksesi sivuston järjestelmänvalvojan oikeuksilla.

2.  Siirry **käyttäjät**ja valitse sitten **+** Avaa **Luo käyttäjä** -valintaikkuna.

    ![Käyttäjät] (./media/active-directory-saas-appdynamics-tutorial/IC790229.png "Käyttäjät")

3.  Valitse **Luo käyttäjä** -osassa seuraavasti:

    ![Käyttäjän luominen] (./media/active-directory-saas-appdynamics-tutorial/IC790230.png "Käyttäjän luominen")

    1.  Kirjoita **käyttäjänimi**, **nimi**, **Sähköposti**, **Uusi salasana**, kelvollinen AAD-tilin haluat säännöstä kyselyjä liittyvät tekstiruutujen **Toista uutta salasanaa** .
    2.  Valitse **Tallenna**.

>[AZURE.NOTE] Voit käyttää mitä tahansa muita AppDynamics käyttäjän tilin luontityökalut tai ohjelmointirajapinnan myöntämä AppDynamics valmistelu Azure AD-käyttäjätilejä.

##<a name="assigning-users"></a>Käyttäjien määrittäminen

Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-appdynamics-perform-the-following-steps"></a>Jos haluat määrittää käyttäjiä AppDynamics, toimi seuraavasti:

1.  Testaa tilin luominen Azure perinteinen portaalissa.

2.  Valitse integration **AppDynamics **sovellus-sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-appdynamics-tutorial/IC790231.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Kyllä] (./media/active-directory-saas-appdynamics-tutorial/IC767830.png "Kyllä")

Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli. Katso lisätietoja Access-paneelin [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).
