<properties 
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi Picturepark | Microsoft Azure" 
    description="Opettele käyttämään Picturepark Azure Active Directory-hakemistosta käyttöön kertakirjautumisen, automaattinen valmistelu ja lisää!" 
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
    ms.date="09/26/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-picturepark"></a>Opetusohjelma: Picturepark Azure Active Directory-integrointi
  
Tässä opetusohjelmassa tavoitteena on näyttämään Azure ja Picturepark integrointi.  
Tässä opetusohjelmassa kuvatut tilanteessa oletetaan, että sinulla on jo seuraavat kohteet:

-   Kelvollinen Azure tilaus
-   Picturepark palvelutili
  
Kun olet suorittanut Tässä opetusohjelmassa, olet liittänyt Picturepark Azure AD-käyttäjien saa oikeuden kertakirjauksen Picturepark yrityksesi sivuston (palveluntarjoajan aloittanut allekirjoitus-) tai käyttämällä [Access-paneelin esittely](active-directory-saas-access-panel-introduction.md)-sovellukseen.
  
Tässä opetusohjelmassa kuvatut skenaarion kuuluu seuraavat hakukyselyn:

1.  Picturepark sovelluksen integrointi ottaminen käyttöön
2.  Kertakirjautumisen määrittäminen
3.  Määrittäminen käyttäjän valmistelu
4.  Käyttäjien määrittäminen

![Skenaario] (./media/active-directory-saas-picturepark-tutorial/IC795055.png "Skenaario")

##<a name="enabling-the-application-integration-for-picturepark"></a>Picturepark sovelluksen integrointi ottaminen käyttöön
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön Picturepark sovelluksen integrointi.

###<a name="to-enable-the-application-integration-for-picturepark-perform-the-following-steps"></a>Picturepark sovelluksen integrointi käyttöön toimimalla seuraavasti:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-picturepark-tutorial/IC700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-picturepark-tutorial/IC700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-picturepark-tutorial/IC749321.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-picturepark-tutorial/IC749322.png "Lisää sovellus-gallerry")

6.  Kirjoita **hakuruutuun** **Picturepark**.

    ![Sovelluksen valikoima] (./media/active-directory-saas-picturepark-tutorial/IC795056.png "Sovelluksen valikoima")

7.  Valitse tulosruudussa **Picturepark**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![Picturepark] (./media/active-directory-saas-picturepark-tutorial/IC795057.png "Picturepark")

##<a name="configuring-single-sign-on"></a>Kertakirjautumisen määrittäminen
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjien todentamiseen Picturepark käyttämällä SAML-protokollan perusteella federation Azure AD-tilin kanssa.  
Kertakirjausta varten Picturepark määrittäminen edellyttää, että allekirjoitus arvon noutaa varmenne.  
Jos et ole tottunut näin, katso, [miten voit hakea varmenteen allekirjoitus arvon](http://youtu.be/YKQF266SAxI)...

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Voit määrittää kertakirjautumisen toimimalla seuraavasti:

1.  Valitse Azure perinteinen portaalissa integrointi **Picturepark** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-picturepark-tutorial/IC795058.png "Kertakirjautumisen määrittäminen")

2.  **Miten käyttäjät voivat kirjautua Picturepark haluat käyttää** sivulla Valitse **Microsoft Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-picturepark-tutorial/IC795059.png "Kertakirjautumisen määrittäminen")

3.  **Määritä sovelluksen URL-osoite** -sivulla **Picturepark merkki-URL** -tekstiruutuun Kirjoita käyttämällä seuraavaa kaavaa "*http://company.picturepark.com*" URL-osoite ja valitse sitten **Seuraava**.

    ![Sovelluksen URL-Osoitteen määrittäminen] (./media/active-directory-saas-picturepark-tutorial/IC795060.png "Sovelluksen URL-Osoitteen määrittäminen")

4.  **Määritä kertakirjautumisen osoitteessa Picturepark** -sivulla Lataa sertifikaatti, valitse **Lataa**, ja Tallenna sertifikaattitiedosto paikallisesti tietokoneeseen.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-picturepark-tutorial/IC795061.png "Kertakirjautumisen määrittäminen")

5.  Eri selainikkunassa Kirjaudu Picturepark yrityksesi sivuston järjestelmänvalvojan oikeuksilla.

6.  Yläkulmassa työkalurivin **Valvontatyökalut**ja valitse sitten **Management Console**.

    ![Hallintakonsoli] (./media/active-directory-saas-picturepark-tutorial/IC795062.png "Hallintakonsoli")

7.  Valitse **todennus**ja valitse sitten **tunnistetietojen toimittajat**.

    ![Todennus] (./media/active-directory-saas-picturepark-tutorial/IC795063.png "Todennus")

8.  **Tunnistetietojen toimittaja määritykset** -osasta toimimalla seuraavasti:

    ![Tunnistetietojen toimittaja määritys] (./media/active-directory-saas-picturepark-tutorial/IC795064.png "Tunnistetietojen toimittaja määritys")

    1.  Valitse **Lisää**.
    2.  Kirjoita kokoonpanosi nimi.
    3.  Valitse **Aseta oletukseksi**.
    4.  Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa Picturepark** -valintasivun **SAML SSO URL-osoite** -arvo kopioi ja liitä se sitten **Myöntäjä URI** -tekstiruutuun.
    5.  Kopioi **allekirjoitusta** viedyn varmenteen ja liitä se sitten **Luotetut myöntäjä napin Tulosta** -tekstiruutuun.  

        >[AZURE.TIP]Lisätietoja on artikkelissa [noutamisesta varmenteen allekirjoitusarvo](http://youtu.be/YKQF266SAxI)

    6.  Valitse **JoinDefaultUsersGroup**.
    7.  Voit määrittää **Emailaddress** -määritteen **Varaa** tekstiruutuun kirjoittamalla **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.
        ![Määritys] (./media/active-directory-saas-picturepark-tutorial/IC795065.png "Määritys")
    8.  Valitse **Tallenna**.

9.  Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Valmis** **Määrittäminen kertakirjautuminen** -valintaikkunassa Sulje.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-picturepark-tutorial/IC795066.png "Kertakirjautumisen määrittäminen")

##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu
  
Jos haluat ottaa käyttöön Azure AD-käyttäjät voivat kirjautua sisään Picturepark, ne on valmisteltava Picturepark kyselyjä.  
Picturepark valmistelu on manuaalinen tehtävä.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Valmistelu käyttäjätilit, toimi seuraavasti:

1.  Kirjaudu sisään **Picturepark** alihallintaan.

2.  Yläkulmassa työkalurivin **Valvontatyökalut**ja valitse sitten **käyttäjät**.

    ![Käyttäjät] (./media/active-directory-saas-picturepark-tutorial/IC795067.png "Käyttäjät")

3.  Valitse **käyttäjien yleiskatsaus** -välilehdessä **Uusi**.

    ![Käyttäjien hallinta] (./media/active-directory-saas-picturepark-tutorial/IC795068.png "Käyttäjien hallinta")

4.  Valitse **Luo käyttäjä** -valintaikkunassa seuraavasti:

    ![Käyttäjän luominen] (./media/active-directory-saas-picturepark-tutorial/IC795069.png "Käyttäjän luominen")

    1.  Tyyppi: **sähköpostiosoite**, **salasana**, **Vahvista salasana**, **Etunimi**, **Sukunimi**, **yrityksen**, **maan**, **ZIP-**, **Kaupunki** kelvollinen Azure Active Directory-käyttäjän haluat objektityyppi säännöstä liittyvät tekstiruutujen kyselyjä.
    2.  Valitse **kieli**.
    3.  Valitse **Luo**.

>[AZURE.NOTE]Voit käyttää mitä tahansa muita Picturepark käyttäjän tilin luontityökalut tai ohjelmointirajapinnan myöntämä Picturepark säännöstä AAD käyttäjätileille.

##<a name="assigning-users"></a>Käyttäjien määrittäminen
  
Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

###<a name="to-assign-users-to-picturepark-perform-the-following-steps"></a>Jos haluat määrittää käyttäjiä Picturepark, toimi seuraavasti:

1.  Testaa tilin luominen Azure perinteinen portaalissa.

2.  Valitse integration **Picturepark **sovellus-sivulla **määritettävä käyttäjille**.

    ![Käyttäjien määrittäminen] (./media/active-directory-saas-picturepark-tutorial/IC795070.png "Käyttäjien määrittäminen")

3.  Valitse Testaa käyttäjä, valitse **Määritä**ja sitten Vahvista valitsemalla **Kyllä** tehtävääsi.

    ![Kyllä] (./media/active-directory-saas-picturepark-tutorial/IC767830.png "Kyllä")
  
Jos haluat testata yksittäisen Sign-asetuksia, Avaa Access-paneeli. Katso lisätietoja Access-paneelin [Johdanto Access-paneeli](active-directory-saas-access-panel-introduction.md).