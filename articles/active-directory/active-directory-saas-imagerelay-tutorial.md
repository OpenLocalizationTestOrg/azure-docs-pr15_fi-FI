<properties
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi ImageRelay | Microsoft Azure"
    description="Opettele määrittämään kertakirjautumisen Azure Active Directory- ja ImageRelay välillä."
    services="active-directory"
    documentationCenter=""
    authors="jeevansd"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-imagerelay"></a>Opetusohjelma: ImageRelay Azure Active Directory-integrointi

Tässä opetusohjelmassa tavoitteena on, kuinka voit integroida ImageRelay Azure Active Directory (Azure AD).  
Azure AD-integraation ImageRelay avulla voit seuraavat edut:

- Voit hallita Azure AD, jolla on pääsy ImageRelay
- Voit ottaa käyttöön automaattisesti Hae kirjautunut käyttöön ImageRelay (kertakirjautumisen) niiden Azure AD-tilien käyttäjille
- Voit hallita asiakkaiden yhdessä keskitetyssä sijainnissa - Azure perinteinen portal

Jos haluat tietää tarkempia tietoja SaaS sovelluksen integrointi Azure AD-kohdassa [mitä sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Edellytykset

Määrittää Azure AD-integroinnin ImageRelay, tarvitset seuraavat:

- Azure AD-tilaus
- Käytössä ImageRelay kertakirjautumisen tilaus


> [AZURE.NOTE] Voit esikatsella vaiheet Tässä opetusohjelmassa, Emme suosittele käyttämällä tuotantoympäristössä.


Voit esikatsella vaiheet Tässä opetusohjelmassa-noudattamalla näitä suosituksia:

- Älä käytä tuotantoympäristössä, ellei se on tarpeen.
- Jos sinulla ei ole kokeiluversion Azure AD-ympäristössä, voit hankkia yksi kuukausi kokeiluversion [tähän](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Skenaarion kuvaus
Tässä opetusohjelmassa tavoitteena on, jotta voit testata Azure AD kertakirjautumisen testiympäristössä.  
Tässä opetusohjelmassa kuvatut skenaarion kuuluu kaksi tärkeimmät rakenneosien:

1. ImageRelay lisääminen valikoimasta

2. Yksittäinen määrittäminen ja testaus Azure AD-on


## <a name="adding-imagerelay-from-the-gallery"></a>ImageRelay lisääminen valikoimasta
Voit määrittää ImageRelay integroida Azure AD-haluat lisätä ImageRelay valikoimasta hallitun SaaS sovellusluettelo.

**Jos haluat lisätä ImageRelay valikoimasta, toimi seuraavasti:**

1. Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa. 

    ![Active Directory][1]

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset][2]

4. Valitse sivun alareunassa **Lisää** .

    ![Sovellukset][3]

5. Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Sovellukset][4]

6. Kirjoita hakuruutuun **ImageRelay**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_01.png)

7. Valitse tulosruudussa **ImageRelay**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yksittäinen määrittäminen ja testaus Azure AD-on
Tässä osassa tavoitteena on noudattamalla voit määrittää ja testaa Azure AD kertakirjautumisen ImageRelay nimeltä "Britta Simon" testikäyttäjän perusteella.

Kertakirjautumisen toimimaan Azure AD on käyttäjätili, joka edustaa ImageRelay liittyvät käyttäjän.  Toisin sanoen suhteen Azure AD-käyttäjä ja Aiheeseen liittyvät ImageRelay-linkki on vahvistettava.  
Linkki-yhteys on muodostettu määrittämällä **käyttäjänimi** arvo arvoksi ImageRelay **käyttäjänimi** Azure AD.

Määrittäminen ja testaaminen Azure AD kertakirjautumisen ImageRelay kanssa, sinun on tehtävä seuraavat hakukyselyn:

1. **[Azure AD määrittäminen Single Sign](#configuring-azure-ad-single-single-sign-on)** - käyttäjät voivat käyttää tätä toimintoa.
2. **[Käyttäjän testata luominen Azure AD](#creating-an-azure-ad-test-user)** - Testaa Azure AD kertakirjautumisen Britta Simon kanssa.
4. **[Luomisesta ImageRelay testata käyttäjän](#creating-a-userecho-test-user)** - tapahtumista Britta Simon ovat ImageRelay, jotka on linkitetty hän Azure AD esitys.
5. **[Käyttäjän testata määrittäminen Azure AD](#assigning-the-azure-ad-test-user)** - Britta Simon käyttämään Azure AD kertakirjautumisen käyttöön.
5. **[Testaus Single Sign](#testing-single-sign-on)** - voit tarkistaa, toimiiko määritykset.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD-Single Sign-On määrittäminen

Tässä osassa tavoitteena on Azure AD kertakirjautumisen Azure perinteinen portaalissa ottaminen käyttöön ja määrittää kertakirjautumisen ImageRelay-sovelluksessa.


**Määrittää Azure AD kertakirjautumisen ImageRelay, toimimalla seuraavasti:**

1. Valitse Azure perinteinen portaalissa integrointi **ImageRelay** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen Single Sign** -valintaikkunan avaaminen.

     ![Kertakirjautumisen määrittäminen][6] 

2. **Miten käyttäjät voivat kirjautua ImageRelay haluat käyttää** sivulla Valitse **Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_03.png) 

3. Valitse **Määritä sovelluksen asetukset** -sivulla toimimalla seuraavasti:

     ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_04.png) 

    a. Kirjoita **Merkki-URL** -tekstiruutuun käyttää käyttäjien kirjautua ImageRelay sovelluksen URL-osoite (esimerkiksi: *https://fabrikam.ImageRelay.com/*).

    b. Valitse **Seuraava**.

4. Valitse **Määritä kertakirjautumisen osoitteessa ImageRelay** -sivulla toimimalla seuraavasti:

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_05.png) 

    a. Valitse **Lataa**ja tallenna sitten tiedosto tietokoneesta.

    b. Valitse **Seuraava**.

5. Toisessa selainikkunassa Kirjaudu ImageRelay yrityksesi sivuston järjestelmänvalvojan oikeuksilla.

1. Valitse **käyttäjät ja käyttöoikeudet** työmäärää sivun työkalurivillä.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_06.png) 

1. Valitse **uuden käyttöoikeustason**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_08.png) 

1. Valitse **Yksittäinen merkki-asetuksia** , työmäärää **Tämä ryhmä voi vain Kirjautumisvirheitä aiheuttavien kertakirjautumisen kautta** -valintaruutu ja valitse sitten **Tallenna**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_09.png) 

1. Valitse **tilin asetukset**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_10.png) 

1. Siirry **Yksi merkki-asetuksia** työmäärää.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_11.png)

1. Suorita **SAML asetukset** -valintaikkunassa seuraavat toimet:

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_12.png)

    a. Azure perinteinen portaalissa **Sign-palvelun URL**kopioi ja liitä se sitten **Kirjautuminen URL** -tekstiruutuun.


    b. Azure perinteinen-portaalissa kopioi **Yhden Sign-Out palvelun URL-osoite**ja liitä se sitten **Kirjaudu ulos URL** -tekstiruutuun.

    c-näppäinyhdistelmää. **Tunnus nimimuotoa**, valitse **urn: oasis: nimet: tc: SAML:1.1:nameid-muoto: emailAddress**.

    
    d. Valitse **Palveluntarjoaja (kuva välitys)-pyynnöt sidonnan asetukset** **Kirjaa sidonta**.
   

    e. Valitse **x.509-varmenne**Valitse **Päivitys varmenne**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_17.png)

    f. Avaa ladatut varmenteen Muistiossa sisällön kopioiminen ja liittäminen x.509-varmenne tekstiruutu.
  
    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_18.png)

    g. Valitse **Oikeaan aikaan tapahtuva käyttäjän valmistelu** -osassa **Ota käyttöön oikeaan aikaan tapahtuva käyttäjän valmistelu**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_19.png)

    h. Valitse käyttöoikeudet-ryhmä (esimerkiksi **SSO Basic**) mikä on sallittua vain kautta kertakirjautumisen kirjautuminen.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_20.png)

    voin. Valitse **Tallenna**.

6. Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Seuraava**.

    ![Azure AD-Single Sign-On][10]

7. Valitse **Sign Vahvista** -sivulla **Valmis**.

    ![Azure AD-Single Sign-On][11]


### <a name="creating-an-azure-ad-test-user"></a>Azure AD-testikäyttäjän luominen
Tässä osassa tavoitteena on luotava testikäyttäjän kutsutaan Britta Simon Azure perinteinen portaalissa.

![Luo Azure AD-käyttäjä][20]

**Voit luoda testikäyttäjän Azure AD-toimimalla seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen portal**, valitse vasemmassa siirtymisruudussa.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-imagerelay-tutorial/create_aaduser_09.png) 

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Ylä-valikossa käyttäjäluettelon, näkyviin napsauttamalla **käyttäjät**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-imagerelay-tutorial/create_aaduser_03.png) 

4. Avaa **Lisää käyttäjä** -valintaikkunan alareunassa työkalurivillä, valitse **Lisää käyttäjä**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-imagerelay-tutorial/create_aaduser_04.png) 

5. Tee valintaikkunassa **Kerro kyseisen käyttäjän tietoja** -sivulla seuraavasti:

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-imagerelay-tutorial/create_aaduser_05.png) 

    a. Käyttäjänä tyyppi Valitse uusi käyttäjä organisaatiossa.

    b. Kirjoita käyttäjänimi- **tekstiruutuun** **BrittaSimon**.

    c-näppäinyhdistelmää. Valitse **Seuraava**.

6.  Suorita **Käyttäjäprofiili** -sivulla seuraavat toimet:

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-imagerelay-tutorial/create_aaduser_06.png) 

    a. Kirjoita **Ensimmäinen nimi** -tekstiruutuun **Britta**.  

    b. Kirjoita **Sukunimi** -tekstiruutuun tyyppi- **Simon**.

    c-näppäinyhdistelmää. Kirjoita **Näyttönimi** -tekstiruutuun **Britta Simon**.

    d. Valitse **rooli** -luettelosta **käyttäjä**.

    e. Valitse **Seuraava**.

7. Valitse **Hae tilapäinen salasana** -valintaikkunan sivulla **Luo**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-imagerelay-tutorial/create_aaduser_07.png) 

8. **Hae tilapäinen salasana** -valintaikkunan sivulla toimimalla seuraavasti:

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-imagerelay-tutorial/create_aaduser_08.png) 

    a. Kirjoita **Uusi salasana**arvo muistiin.

    b. Valitse **Valmis**.   



### <a name="creating-a-imagerelay-test-user"></a>ImageRelay testikäyttäjän luominen

Tässä osassa tavoitteena on nimeltään Britta Simon ImageRelay käyttäjän luominen.

**Luo kutsutaan Britta Simon ImageRelay käyttäjä, toimi seuraavasti:**

1. Kertakirjauksen ImageRelay yrityksen sivustoon järjestelmänvalvojana.

1. Siirry **käyttäjät ja käyttöoikeudet** ja valitse **Luo SSO-käyttäjä**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_21.png) 

1. **Sähköpostin**, **Etunimi**, **Sukunimi** ja **yrityksen** käyttäjän haluat valmistelu ja käyttöoikeudet-ryhmä (esimerkiksi SSO Basic) on ryhmä, johon vain kautta kertakirjautumisen kirjautumaan.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_22.png) 

1. Valitse **Luo**.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD-testikäyttäjän määrittäminen

Tässä osassa tavoitteena on Britta Simon käyttämään Azure kertakirjautumisen hänen käyttöoikeuden myöntämisestä ImageRelay ottaminen käyttöön.

![Määrittää käyttäjälle][200] 

**Jos haluat määrittää Britta Simon ImageRelay, toimi seuraavasti:**

1. Azure perinteinen-portaalissa Avaa sovellukset-näkymässä kansio-näkymässä, valitse **sovellukset** yläreunan-valikossa.

    ![Määrittää käyttäjälle][201] 

2. Valitse sovellukset-luettelosta **ImageRelay**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-imagerelay-tutorial/tutorial_imagerelay_23.png) 

1. Valitse ylä-valikossa Valitse **käyttäjiä**.

    ![Määrittää käyttäjälle][203] 

1. Valitse käyttäjät-luettelosta **Britta Simon**.

2. Napsauta työkalurivin alaosassa **määrittäminen**.

    ![Määrittää käyttäjälle][205]


### <a name="testing-single-sign-on"></a>Kertakirjautumisen testaaminen

Tässä osassa tavoitteena on Testaa Azure AD yksittäisen Sign-asetusten käyttäminen Access-paneeli.  
Access-ruudussa ImageRelay-ruutua napsauttaessasi sinun pitäisi saada automaattisesti kirjautunut-on ImageRelay sovelluksen.


## <a name="additional-resources"></a>Lisäresursseja

* [Luettelo siitä, miten voit integroida SaaS sovellusten Azure Active Directory-opetusohjelmat](active-directory-saas-tutorial-list.md)
* [Mikä on sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-imagerelay-tutorial/tutorial_general_205.png
