<properties 
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi ClickTime | Microsoft Azure" 
    description="Opettele käyttämään ClickTime Azure Active Directory-hakemistosta käyttöön kertakirjautumisen, automaattinen valmistelu ja lisää!" 
    services="active-directory" 
    authors="jeevansd"
    documentationCenter="na" 
    manager="femila" />
<tags
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="08/16/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-clicktime"></a>Opetusohjelma: ClickTime Azure Active Directory-integrointi

Tässä opetusohjelmassa kerrotaan, miten voit integroida ClickTime Azure Active Directory (Azure AD).

Azure AD-integraation ClickTime avulla voit seuraavat edut:

- Voit hallita Azure AD, jolla on pääsy ClickTime
- Voit ottaa käyttöön automaattisesti Hae kirjautunut käyttöön ClickTime (kertakirjautumisen) niiden Azure AD-tilien käyttäjille
- Voit hallita asiakkaiden yhdessä keskitetyssä sijainnissa - Azure perinteinen portal

Jos haluat tietää tarkempia tietoja SaaS sovelluksen integrointi Azure AD-kohdassa [mitä sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Edellytykset

Määrittää Azure AD-integroinnin ClickTime, tarvitset seuraavat:

- Azure AD-tilaus
- ClickTime single-merkki käytössä-tilauksessa


> [AZURE.NOTE] Voit esikatsella vaiheet Tässä opetusohjelmassa, Emme suosittele käyttämällä tuotantoympäristössä.


Voit esikatsella vaiheet Tässä opetusohjelmassa-noudattamalla näitä suosituksia:

- Älä käytä tuotantoympäristössä, ellei se on tarpeen.
- Jos sinulla ei ole kokeiluversion Azure AD-ympäristössä, voit hankkia yksi kuukausi kokeiluversion [tähän](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Skenaarion kuvaus
Tässä opetusohjelmassa testaat Azure AD kertakirjautumisen testiympäristössä.

Tässä opetusohjelmassa kuvatut skenaarion kuuluu kaksi tärkeimmät rakenneosien:

1. ClickTime lisääminen valikoimasta
2. Yksittäinen määrittäminen ja testaus Azure AD-on

##<a name="adding-clicktime-from-the-gallery"></a>ClickTime lisääminen valikoimasta

Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön ClickTime sovelluksen integrointi.

###<a name="to-enable-the-application-integration-for-clicktime-perform-the-following-steps"></a>ClickTime sovelluksen integrointi käyttöön toimimalla seuraavasti:

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-clicktime-tutorial/tic700993.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-clicktime-tutorial/tic700994.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-clicktime-tutorial/tic749321.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-clicktime-tutorial/tic749322.png "Lisää sovellus-gallerry")

6.  Kirjoita **hakuruutuun** **ClickTime**.

    ![Sovelluksen valikoima] (./media/active-directory-saas-clicktime-tutorial/tic777275.png "Sovelluksen valikoima")

7.  Valitse tulosruudussa **ClickTime**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![ClickTime] (./media/active-directory-saas-clicktime-tutorial/tic777276.png "ClickTime")

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yksittäinen määrittäminen ja testaus Azure AD-on
Tässä osassa Määritä ja testaa Azure AD kertakirjautumisen ClickTime nimeltä "Britta Simon" testikäyttäjän perusteella.

Kertakirjautumisen toimimaan Azure AD on tiedettävä käyttäjälle on ClickTime vastaavaan käyttäjän Azure AD. Toisin sanoen suhteen Azure AD-käyttäjä ja Aiheeseen liittyvät ClickTime-linkki on vahvistettava.

Linkki-yhteys on muodostettu määrittämällä **käyttäjänimi** arvo arvoksi ClickTime **käyttäjänimi** Azure AD.

Määrittäminen ja testaaminen Azure AD kertakirjautumisen ClickTime kanssa, sinun on tehtävä seuraavat hakukyselyn:

1. **[Azure AD määrittäminen Single Sign](#configuring-azure-ad-single-sign-on)** - käyttäjät voivat käyttää tätä toimintoa.
2. **[Käyttäjän testata luominen Azure AD](#creating-an-azure-ad-test-user)** - Testaa Azure AD kertakirjautumisen Britta Simon kanssa.
3. **[Luomisesta ClickTime testata käyttäjän](#creating-a-clicktime-test-user)** - tapahtumista Britta Simon ovat ClickTime, jotka on linkitetty hän Azure AD esitys.
4. **[Käyttäjän testata määrittäminen Azure AD](#assigning-the-azure-ad-test-user)** - Britta Simon käyttämään Azure AD kertakirjautumisen käyttöön.
5. **[Testaus Single Sign](#testing-single-sign-on)** - voit tarkistaa, toimiiko määritykset.


### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD kertakirjautumisen määrittäminen

Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjien todentamiseen ClickTime käyttämällä SAML-protokollan perusteella federation Azure AD-tilin kanssa.  


>[AZURE.IMPORTANT] Jos haluat voi määrittää kertakirjautumisen ClickTime vuokraajan, ota ensin ClickTime tekniseen tukeen ja Hae tämä toiminto on käytössä.

**Määrittää Azure AD kertakirjautumisen ClickTime, toimimalla seuraavasti:**

1.  Valitse Azure perinteinen portaalissa integrointi **ClickTime** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Ottaa käyttöön kertakirjautumisen] (./media/active-directory-saas-clicktime-tutorial/tic777277.png "Ottaa käyttöön kertakirjautumisen")

2.  **Miten käyttäjät voivat kirjautua ClickTime haluat käyttää** sivulla Valitse **Microsoft Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-clicktime-tutorial/tic777278.png "Määritä kertakirjautuminen")

3. Valitse **Määritä sovelluksen asetukset** -sivulla toimimalla seuraavasti:

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-clicktime-tutorial/tic777286.png) 

    a. Kirjoita **IdentifierL** -tekstiruutuun URL-osoite, käyttämällä seuraavaa kaavaa: **https://app.clicktime.com/sp/**
    
    b. Kirjoita **Vastaus URL** -tekstiruutuun URL-osoite, käyttämällä seuraavaa kaavaa: **https://app.clicktime.com/Login/**

    c-näppäinyhdistelmää. Valitse **Seuraava**

4.  Valitse **Määritä kertakirjautumisen osoitteessa ClickTime** -sivulla Lataa sertifikaatti, valitsemalla **Lataa**, ja Tallenna sertifikaattitiedosto tietokoneeseen.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-clicktime-tutorial/tic777279.png "Määritä kertakirjautuminen")

4.  Eri selainikkunassa Kirjaudu ClickTime yrityksesi sivuston järjestelmänvalvojan oikeuksilla.

5.  Yläkulmassa työkalurivin **asetukset**ja valitse sitten **Suojausasetukset**.

6.  **Sign-asetusten** määritys-osassa seuraavasti:

    ![Tietoturva-asetukset] (./media/active-directory-saas-clicktime-tutorial/tic777280.png "Tietoturva-asetukset")

    a.  Valitse **Salli** -kirjautuminen käyttäminen **Azure AD**Single Sign-On (SSO).
    
    b.  Azure perinteinen portaalissa-sivulla **Määritä kertakirjautumisen osoitteessa ClickTime** valintaikkunan **Sign-palvelun URL** -arvo kopioi ja liitä se sitten **Tunnistetietojen toimittaja päätepisteen** tekstiruutu.

    c-näppäinyhdistelmää.  Avaa base-64-koodattu varmenteen **Muistiossa**, sisällön kopioiminen ja liittäminen **X.509-varmenne** -tekstiruutuun.
    
    d.  Valitse **Tallenna**.

7.  Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Valmis** **Määrittäminen kertakirjautuminen** -valintaikkunassa Sulje.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-clicktime-tutorial/tic777281.png "Määritä kertakirjautuminen")

##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu

Jos haluat ottaa käyttöön Azure AD-käyttäjät voivat kirjautua sisään ClickTime, ne on valmisteltava ClickTime kyselyjä.  
ClickTime valmistelu on manuaalinen tehtävä.

###<a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Valmistelu käyttäjätilit, toimi seuraavasti:

1.  Kirjaudu sisään **ClickTime** alihallintaan.

2.  Yläkulmassa työkalurivin **yrityksen**ja valitse **ihmiset**.

    ![Henkilöt] (./media/active-directory-saas-clicktime-tutorial/tic777282.png "Henkilöt")

3.  Valitse **Lisää henkilö**.

    ![Lisää henkilö] (./media/active-directory-saas-clicktime-tutorial/tic777283.png "Lisää henkilö")

4.  Uuden henkilön-osassa seuraavasti:

    ![Henkilöt] (./media/active-directory-saas-clicktime-tutorial/tic777284.png "Henkilöt")

    a.  Kirjoita **sähköpostiosoite** -tekstiruutuun Azure AD-tilisi sähköpostiosoite.
    
    b.  Kirjoita **koko nimi** -tekstiruutuun Azure AD-tilisi nimi.  

    >[AZURE.NOTE] Jos haluat, voit määrittää uuden henkilön objektin lisäominaisuuksia.

    c-näppäinyhdistelmää.  Valitse **Tallenna**.

>[AZURE.NOTE] Voit käyttää mitä tahansa muita ClickTime käyttäjän tilin luontityökalut tai ohjelmointirajapinnan myöntämä ClickTime valmistelu Azure AD-käyttäjätilejä.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD-testikäyttäjän määrittäminen

Tässä osassa voit ottaa käyttöön Britta Simon käyttämään Azure kertakirjautumisen hänen käyttöoikeuksien myöntämistä ClickTime.

![Määrittää käyttäjälle][200]

Voit esikatsella kokoonpanosi-haluat myöntää Azure AD-käyttäjille sallitaan avulla määrittämällä sen sovelluksen käyttöoikeus.

**Jos haluat määrittää Britta Simon ClickTime, suorita seuraavat vaiheet**

1. Perinteinen-portaalissa Avaa sovellukset-näkymässä kansio-näkymässä, valitse **sovellukset** yläreunan-valikossa.

    ![Määrittää käyttäjälle][201] 

2. Valitse sovellukset-luettelosta **ClickTime**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-clicktime-tutorial/tutorial_clicktime_50.png) 

3. Valitse ylä-valikossa Valitse **käyttäjiä**.

    ![Määrittää käyttäjälle][203]

4. Valitse käyttäjät-luettelosta **Britta Simon**.

5. Napsauta työkalurivin alaosassa **määrittäminen**.

    ![Määrittää käyttäjälle][205]

## <a name="testing-single-sign-on"></a>Kertakirjautumisen testaaminen
Tässä osassa voit testata Azure AD yksittäisen Sign-asetusten käyttäminen Access-paneeli.

Access-ruudussa ClickTime-ruutua napsauttaessasi sinun pitäisi saada automaattisesti kirjautunut-on ClickTime sovelluksen.


## <a name="additional-resources"></a>Lisäresursseja

* [Luettelo siitä, miten voit integroida SaaS sovellusten Azure Active Directory-opetusohjelmat](active-directory-saas-tutorial-list.md)
* [Mikä on sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[200]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_203.png
[205]: ./media/active-directory-saas-clicktime-tutorial/tutorial_general_205.png