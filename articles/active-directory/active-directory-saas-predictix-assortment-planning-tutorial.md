<properties
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi Predictix valikoiman suunnittelulla | Microsoft Azure"
    description="Opettele määrittämään kertakirjautumisen Azure Active Directory- ja Predictix valikoiman suunnittelu välillä."
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
    ms.date="10/14/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-predictix-assortment-planning"></a>Opetusohjelma: Azure Active Directory integrointi Predictix valikoiman suunnittelu

Tässä opetusohjelmassa kerrotaan, miten voit integroida Azure Active Directory (Azure AD) Predictix valikoiman suunnittelu.

Integrointi Azure AD Predictix valikoiman suunnittelu, jonka avulla seuraavat edut:

- Voit hallita Azure AD, jolla on pääsy Predictix valikoiman suunnittelu
- Voit ottaa käyttöön automaattisesti Hae kirjautunut sisään Predictix valikoiman suunnittelun (kertakirjautumisen) käyttäjille heidän Azure AD-tilien kanssa
- Voit hallita asiakkaiden yhdessä keskitetyssä sijainnissa - Azure perinteinen portal

Jos haluat tietää tarkempia tietoja SaaS sovelluksen integrointi Azure AD-kohdassa [mitä sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Edellytykset

Määrittää Azure AD-integroinnin Predictix valikoiman suunnittelu, tarvitset seuraavat:

- Azure AD-tilaus
- Predictix valikoiman suunnittelu single-merkki käytössä-tilauksessa


> [AZURE.NOTE] Voit esikatsella vaiheet Tässä opetusohjelmassa, Emme suosittele käyttämällä tuotantoympäristössä.


Voit esikatsella vaiheet Tässä opetusohjelmassa-noudattamalla näitä suosituksia:

- Älä käytä tuotantoympäristössä, ellei se on tarpeen.
- Jos sinulla ei ole kokeiluversion Azure AD-ympäristössä, voit hankkia yksi kuukausi kokeiluversion [tähän](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Skenaarion kuvaus
Tässä opetusohjelmassa testaat Microsoft Azure AD kertakirjautumisen testiympäristössä.

Tässä opetusohjelmassa kuvatut skenaarion kuuluu kaksi tärkeimmät rakenneosien:

1. Lisääminen valikoimasta Predictix valikoiman suunnittelu
2. Määrittäminen ja testaus Microsoft Azure AD Single Sign-On


## <a name="adding-predictix-assortment-planning-from-the-gallery"></a>Lisääminen valikoimasta Predictix valikoiman suunnittelu
Voit määrittää Predictix valikoiman suunnittelemisesta integroida Azure AD-haluat lisääminen Predictix valikoiman suunnittelu valikoimasta hallitun SaaS sovellusluettelo.

**Jos haluat lisätä valikoimasta Predictix valikoiman suunnittelu, toimi seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen portal**, valitse vasemmassa siirtymisruudussa.

    ![Active Directory][1]
2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset][2]

4. Valitse sivun alareunassa **Lisää** .

    ![Sovellukset][3]

5. Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Sovellukset][4]

6. Kirjoita hakuruutuun **Predictix valikoiman suunnittelu**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_predictixassortmentplanning_01.png)

7. Valitse tulosruudussa **Predictix valikoiman suunnittelu**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_predictixassortmentplanning_02.png)


##  <a name="configuring-and-testing-microsoft-azure-ad-single-sign-on"></a>Määrittäminen ja testaus Microsoft Azure AD Single Sign-On
Tässä osassa Määritä ja testaa Microsoft Azure AD kertakirjautumisen Predictix valikoiman suunnittelulla perusteella testikäyttäjän nimeltä "Britta Simon".

Kertakirjautumisen toimimaan Azure AD on tiedettävä käyttäjälle on vastaavaan käyttäjän Predictix valikoiman suunnittelussa Azure AD. Toisin sanoen linkki suhteen Azure AD-käyttäjä ja Aiheeseen liittyvät Predictix valikoiman suunnittelussa on vahvistettava.

Linkki-yhteys on muodostettu määrittämällä **käyttäjänimi** arvo arvoksi **käyttäjänimi** Predictix valikoiman suunnittelussa Azure AD.

Määrittäminen ja testaaminen Microsoft Azure AD kertakirjautumisen Predictix valikoiman suunnittelulla, sinun on tehtävä seuraavat hakukyselyn:

1. **[Asetusten määrittäminen Microsoft Azure AD Single Sign](#configuring-azure-ad-single-sign-on)** - käyttäjät voivat käyttää tätä toimintoa.
2. **[Käyttäjän testata luominen Azure AD](#creating-an-azure-ad-test-user)** - Testaa Microsoft Azure AD kertakirjautumisen Britta Simon kanssa.
3. **[Luodaan Predictix valikoiman suunnittelu testikäyttäjän](#creating-a-predictix-price-reporting-test-user)** - on tapahtumista Britta Simon Predictix valikoiman suunnittelussa, jotka on linkitetty hän Azure AD esitys.
4. **[Käyttäjän testata määrittäminen Azure AD](#assigning-the-azure-ad-test-user)** - Britta Simon käyttämään Microsoft Azure AD kertakirjautumisen käyttöön.
5. **[Testaus Single Sign](#testing-single-sign-on)** - voit tarkistaa, toimiiko määritykset.

### <a name="configuring-microsoft-azure-ad-single-sign-on"></a>Microsoft Azure AD-Single Sign-On määrittäminen

Tässä osassa Ota käyttöön Microsoft Azure AD kertakirjautumisen perinteinen portaalissa ja määritä kertakirjautumisen Predictix valikoiman suunnittelu-sovelluksessa.


**Voit määrittää Microsoft Azure AD kertakirjautumisen Predictix valikoiman suunnittelulla toimimalla seuraavasti:**

1. Valitse perinteinen-portaalissa sivulla **Predictix valikoiman suunnittelu** sovelluksen integrointi, **Määritä kertakirjautumisen** **Määrittäminen Single Sign** -valintaikkunan avaaminen.
     
    ![Kertakirjautumisen määrittäminen][6] 

2. **Miten käyttäjät voivat kirjautua Predictix valikoiman suunnittelu haluat käyttää** sivulla Valitse **Microsoft Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_predictixassortmentplanning_03.png) 

3. Valitse **Määritä sovelluksen asetukset** -sivulla toimimalla seuraavasti:

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_predictixassortmentplanning_04.png) 

    a. Kirjoita **Merkki-URL** -tekstiruutuun URL-osoite käyttämä käyttäjille Sign Predictix valikoiman suunnittelu-sovellukseen käyttämällä seuraavaa kaavaa: **https://\<yrityksen nimi hinnat\>.ap.predictix.com/sso/request**.
    
    b. Valitse **Seuraava**
 
4. Valitse **Määritä kertakirjautumisen osoitteessa Predictix valikoiman suunnittelu** -sivulla toimimalla seuraavasti:

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_predictixassortmentplanning_05.png)

    a. Valitse **Lataa**ja tallenna sitten tiedosto tietokoneesta.

    b. Valitse **Seuraava**.


5. Saat määritetty sovelluksen SSO-Predictix valikoiman suunnittelu tukeen ja antaa niille seuraavasti:

    • Ladatut varmenne

    • **Kohteen tunnus**

    • **SAML SSO URL-osoite**

    • **Yksittäisen Kirjaudu ulos palvelun URL-osoite**

6. Perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Seuraava**.
    
    ![Azure AD-Single Sign-On][10]

7. Valitse **Sign Vahvista** -sivulla **Valmis**.  
 
    ![Azure AD-Single Sign-On][11]


### <a name="creating-an-azure-ad-test-user"></a>Azure AD-testikäyttäjän luominen
Tässä osassa voit luoda testikäyttäjän kutsutaan Britta Simon perinteinen portaalissa.


![Luo Azure AD-käyttäjä][20]

**Voit luoda testikäyttäjän Azure AD-toimimalla seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen portal**, valitse vasemmassa siirtymisruudussa.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-predictix-assortment-planning-tutorial/create_aaduser_09.png) 

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Ylä-valikossa käyttäjäluettelon, näkyviin napsauttamalla **käyttäjät**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-predictix-assortment-planning-tutorial/create_aaduser_03.png) 

4. Avaa **Lisää käyttäjä** -valintaikkunan alareunassa työkalurivillä, valitse **Lisää käyttäjä**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-predictix-assortment-planning-tutorial/create_aaduser_04.png) 

5. Suorita valintaikkunan **Kerro kyseisen käyttäjän tietoja** -sivulla seuraavat toimet:  ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-predictix-assortment-planning-tutorial/create_aaduser_05.png) 

    a. Käyttäjänä tyyppi Valitse uusi käyttäjä organisaatiossa.

    b. Kirjoita käyttäjänimi- **tekstiruutuun** **BrittaSimon**.

    c-näppäinyhdistelmää. Valitse **Seuraava**.

6.  Suorita valintaikkunan **Käyttäjäprofiili** -sivulla seuraavat toimet: ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-predictix-assortment-planning-tutorial/create_aaduser_06.png) 

    a. Kirjoita **Ensimmäinen nimi** -tekstiruutuun **Britta**.  

    b. Kirjoita **Sukunimi** -tekstiruutuun tyyppi- **Simon**.

    c-näppäinyhdistelmää. Kirjoita **Näyttönimi** -tekstiruutuun **Britta Simon**.

    d. Valitse **rooli** -luettelosta **käyttäjä**.

    e. Valitse **Seuraava**.

7. Valitse **Hae tilapäinen salasana** -valintaikkunan sivulla **Luo**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-predictix-assortment-planning-tutorial/create_aaduser_07.png) 

8. **Hae tilapäinen salasana** -valintaikkunan sivulla toimimalla seuraavasti:

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-predictix-assortment-planning-tutorial/create_aaduser_08.png) 

    a. Kirjoita **Uusi salasana**arvo muistiin.

    b. Valitse **Valmis**.   



### <a name="creating-an-predictix-assortment-planning-test-user"></a>Predictix valikoiman suunnittelu-testikäyttäjän luominen

Tässä osassa nimeltä Britta Simon Predictix valikoiman suunnittelussa käyttäjän luominen. Toimi tukihenkilöstöösi Predictix valikoiman suunnittelu voit lisätä käyttäjiä Predictix valikoiman suunnittelu-ympäristössä.


### <a name="assigning-the-azure-ad-test-user"></a>Azure AD-testikäyttäjän määrittäminen

Tässä osassa voit ottaa käyttöön Britta Simon käyttämään Azure kertakirjautumisen hänen käyttöoikeuksien myöntämistä Predictix valikoiman suunnittelu.

![Määrittää käyttäjälle][200] 

**Jos haluat määrittää Britta Simon Predictix valikoiman suunnittelu, toimi seuraavasti:**

1. Perinteinen-portaalissa Avaa sovellukset-näkymässä kansio-näkymässä, valitse **sovellukset** yläreunan-valikossa.

    ![Määrittää käyttäjälle][201] 

2. Valitse sovellukset-luettelosta **Predictix valikoiman suunnittelu**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_predictixassortmentplanning_50.png) 

3. Valitse ylä-valikossa Valitse **käyttäjiä**.

    ![Määrittää käyttäjälle][203]

4. Valitse käyttäjät-luettelosta **Britta Simon**.

5. Napsauta työkalurivin alaosassa **määrittäminen**.

    ![Määrittää käyttäjälle][205]


### <a name="testing-single-sign-on"></a>Kertakirjautumisen testaaminen

Tässä osassa voit testata Microsoft Azure AD kertakirjautumisen asetusten käyttäminen Access-paneeli.

Access-ruudussa Predictix valikoiman suunnittelu-ruutua napsauttaessasi voit olisi Hae automaattisesti kirjautunut sisään Predictix valikoiman suunnittelu-sovellukseen.


## <a name="additional-resources"></a>Lisäresursseja

* [Luettelo siitä, miten voit integroida SaaS sovellusten Azure Active Directory-opetusohjelmat](active-directory-saas-tutorial-list.md)
* [Mikä on sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_general_205.png
