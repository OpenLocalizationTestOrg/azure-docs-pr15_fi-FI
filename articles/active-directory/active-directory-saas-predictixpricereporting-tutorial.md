<properties
    pageTitle="Opetusohjelma: Azure Active Directory integrointi Predictix hinta raportoinnin | Microsoft Azure"
    description="Opettele määrittämään kertakirjautumisen Azure Active Directory- ja Predictix hinta raportointi välillä."
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
    ms.date="09/01/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-predictix-price-reporting"></a>Opetusohjelma: Azure Active Directory integrointi Predictix hinta raportointi

Tässä opetusohjelmassa kerrotaan, miten voit integroida Predictix hinta raportointi ja Azure Active Directory (Azure AD).

Integrointi Azure AD Predictix hinta raportointi, jonka avulla seuraavat edut:

- Voit hallita Azure AD, jolla on pääsy Predictix hinta raportointi
- Voit ottaa käyttöön automaattisesti Hae kirjautunut sisään Predictix hinta raportointiominaisuuksien (kertakirjautumisen) käyttäjille heidän Azure AD-tilien kanssa
- Voit hallita asiakkaiden yhdessä keskitetyssä sijainnissa - Azure perinteinen portal

Jos haluat tietää tarkempia tietoja SaaS sovelluksen integrointi Azure AD-kohdassa [mitä sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Edellytykset

Määrittää Azure AD-integroinnin Predictix hinta raportointi, tarvitset seuraavat:

- Azure AD-tilaus
- Predictix hinta raportoinnin single-merkki käytössä-tilauksessa


> [AZURE.NOTE] Voit esikatsella vaiheet Tässä opetusohjelmassa, Emme suosittele käyttämällä tuotantoympäristössä.


Voit esikatsella vaiheet Tässä opetusohjelmassa-noudattamalla näitä suosituksia:

- Älä käytä tuotantoympäristössä, ellei se on tarpeen.
- Jos sinulla ei ole kokeiluversion Azure AD-ympäristössä, voit hankkia yksi kuukausi kokeiluversion [tähän](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Skenaarion kuvaus
Tässä opetusohjelmassa testaat Azure AD kertakirjautumisen testiympäristössä.

Tässä opetusohjelmassa kuvatut skenaarion kuuluu kaksi tärkeimmät rakenneosien:

1. Predictix hinta raportoinnin lisääminen valikoimasta
2. Yksittäinen määrittäminen ja testaus Azure AD-on


## <a name="adding-predictix-price-reporting-from-the-gallery"></a>Predictix hinta raportoinnin lisääminen valikoimasta
Voit määrittää Predictix hinta raportoinnin integroida Azure AD-haluat lisätä Predictix hinta raportoinnin valikoimasta hallitun SaaS sovellusten luetteloon.

**Lisää Predictix hinta raportointi-valikoimasta, toimi seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen portal**, valitse vasemmassa siirtymisruudussa.

    ![Sovellukset][1]
2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset][2]

4. Valitse sivun alareunassa **Lisää** .

    ![Sovellukset][3]

5. Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Sovellukset][4]

6. Kirjoita hakuruutuun **Predictix hinta raportointia**.

    ![Sovellukset](./media/active-directory-saas-predictixpricereporting-tutorial/tutorial_predictixpricereporting_01.png)

7. Tulokset-ruudussa Valitse **Predictix hinta raportointi**ja valitse sitten **Valmis** , voit lisätä sovelluksen.

    ![Sovellukset](./media/active-directory-saas-predictixpricereporting-tutorial/tutorial_predictixpricereporting_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yksittäinen määrittäminen ja testaus Azure AD-on
Tässä osassa Määritä ja testaa Azure AD kertakirjautumisen Predictix hinta raportoinnin nimeltä "Britta Simon" testikäyttäjän perusteella.

Kertakirjautumisen toimimaan Azure AD on tiedettävä käyttäjälle on vastaavaan käyttäjän Predictix hinta raportoinnin Azure AD. Toisin sanoen linkki Azure AD-käyttäjä ja Aiheeseen liittyvät Predictix hinta raportoinnissa suhteen on vahvistettava.

Linkki-yhteys on muodostettu määrittämällä arvo **käyttäjänimi** **käyttäjänimi** -Predictix hinta raportoinnin arvona Azure AD.

Määrittäminen ja testaaminen Azure AD kertakirjautumisen Predictix hinta ilmoittaa, sinun on tehtävä seuraavat hakukyselyn:

1. **[Azure AD määrittäminen Single Sign](#configuring-azure-ad-single-sign-on)** - käyttäjät voivat käyttää tätä toimintoa.
2. **[Käyttäjän testata luominen Azure AD](#creating-an-azure-ad-test-user)** - Testaa Azure AD kertakirjautumisen Britta Simon kanssa.
3. **[Käyttäjän luominen Predictix hinta raportoinnin testata](#creating-a-predictix-price-reporting-test-user)** - antaaksesi tapahtumista Britta Simon Predictix hinta raportointi, jotka on linkitetty hän Azure AD-esittäminen.
4. **[Käyttäjän testata määrittäminen Azure AD](#assigning-the-azure-ad-test-user)** - Britta Simon käyttämään Azure AD kertakirjautumisen käyttöön.
5. **[Testaus Single Sign](#testing-single-sign-on)** - voit tarkistaa, toimiiko määritykset.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD-Single Sign-On määrittäminen

Tässä osassa Azure AD kertakirjautumisen perinteinen portaalissa ottaminen käyttöön ja määritä kertakirjautumisen Predictix hinta raportointi-sovelluksessa.


**Määrittää Azure AD kertakirjautumisen Predictix hinta raportointi, toimimalla seuraavasti:**

1. Valitse perinteinen-portaalissa **Predictix hinta raportoinnin** sovellus integrointi-sivulla **Määritä kertakirjautumisen** **Määrittäminen Single Sign** -valintaikkunan avaaminen.
     
    ![Kertakirjautumisen määrittäminen][6] 

2. **Miten käyttäjät voivat kirjautua Predictix hinta raportoinnin haluat käyttää** sivulla Valitse **Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-predictixpricereporting-tutorial/tutorial_predictixpricereporting_03.png) 

3. Valitse **Määritä sovelluksen asetukset** -sivulla toimimalla seuraavasti:

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-predictixpricereporting-tutorial/tutorial_predictixpricereporting_04.png) 

    a. Kirjoita **Merkki-URL** -tekstiruutuun URL-osoite käyttämä käyttäjille Sign Predictix hinta raportointi-sovellukseen käyttämällä seuraavaa kaavaa:`https://<company name-pricing>.predictix.com/sso/request`
    
    b. Valitse **Seuraava**
 
4. Valitse **Määritä kertakirjautumisen osoitteessa Predictix hinta raportointi** -sivulla toimimalla seuraavasti:

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-predictixpricereporting-tutorial/tutorial_predictixpricereporting_05.png)

    a. Valitse **Lataa**ja tallenna sitten tiedosto tietokoneesta.

    b. Valitse **Seuraava**.


5. Saat määritetty sovelluksen SSO-Predictix hinta raportoinnin tukeen ja antaa niille seuraavasti:

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

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-predictixpricereporting-tutorial/create_aaduser_09.png) 

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Ylä-valikossa käyttäjäluettelon, näkyviin napsauttamalla **käyttäjät**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-predictixpricereporting-tutorial/create_aaduser_03.png) 

4. Avaa **Lisää käyttäjä** -valintaikkunan alareunassa työkalurivillä, valitse **Lisää käyttäjä**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-predictixpricereporting-tutorial/create_aaduser_04.png) 

5. Suorita valintaikkunan **Kerro kyseisen käyttäjän tietoja** -sivulla seuraavat toimet:  ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-predictixpricereporting-tutorial/create_aaduser_05.png) 

    a. Käyttäjänä tyyppi Valitse uusi käyttäjä organisaatiossa.

    b. Kirjoita käyttäjänimi- **tekstiruutuun** **BrittaSimon**.

    c-näppäinyhdistelmää. Valitse **Seuraava**.

6.  Suorita valintaikkunan **Käyttäjäprofiili** -sivulla seuraavat toimet: ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-predictixpricereporting-tutorial/create_aaduser_06.png) 

    a. Kirjoita **Ensimmäinen nimi** -tekstiruutuun **Britta**.  

    b. Kirjoita **Sukunimi** -tekstiruutuun tyyppi- **Simon**.

    c-näppäinyhdistelmää. Kirjoita **Näyttönimi** -tekstiruutuun **Britta Simon**.

    d. Valitse **rooli** -luettelosta **käyttäjä**.

    e. Valitse **Seuraava**.

7. Valitse **Hae tilapäinen salasana** -valintaikkunan sivulla **Luo**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-predictixpricereporting-tutorial/create_aaduser_07.png) 

8. **Hae tilapäinen salasana** -valintaikkunan sivulla toimimalla seuraavasti:

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-predictixpricereporting-tutorial/create_aaduser_08.png) 

    a. Kirjoita **Uusi salasana**arvo muistiin.

    b. Valitse **Valmis**.   



### <a name="creating-an-predictix-price-reporting-test-user"></a>Predictix hinta raportointi-testikäyttäjän luominen

Tässä osassa nimeltä Britta Simon Predictix hinta raportoinnissa käyttäjän luominen. Toimi tukihenkilöstöösi Predictix hinta raportoinnin voit lisätä käyttäjiä Predictix hinta raportointi-ympäristössä.


### <a name="assigning-the-azure-ad-test-user"></a>Azure AD-testikäyttäjän määrittäminen

Tässä osassa voit ottaa käyttöön Britta Simon käyttämään Azure kertakirjautumisen hänen käyttöoikeuksien myöntäminen Predictix hinta raportointiin.

![Määrittää käyttäjälle][200] 

**Jos haluat määrittää Britta Simon Predictix hinta raportointi, toimi seuraavasti:**

1. Perinteinen-portaalissa Avaa sovellukset-näkymässä kansio-näkymässä, valitse **sovellukset** yläreunan-valikossa.

    ![Määrittää käyttäjälle][201] 

2. Valitse sovellukset-luettelosta **Predictix hinta raportointia**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-predictixpricereporting-tutorial/tutorial_predictixpricereporting_50.png) 

3. Valitse ylä-valikossa Valitse **käyttäjiä**.

    ![Määrittää käyttäjälle][203]

4. Valitse käyttäjät-luettelosta **Britta Simon**.

5. Napsauta työkalurivin alaosassa **määrittäminen**.

    ![Määrittää käyttäjälle][205]


### <a name="testing-single-sign-on"></a>Kertakirjautumisen testaaminen

Tässä osassa voit testata Azure AD yksittäisen Sign-asetusten käyttäminen Access-paneeli.

Access-ruudussa Predictix hinta raportointi-ruutua napsauttaessasi voit olisi Hae automaattisesti kirjautunut sisään Predictix hinta raportointi-sovellukseen.


## <a name="additional-resources"></a>Lisäresursseja

* [Luettelo siitä, miten voit integroida SaaS sovellusten Azure Active Directory-opetusohjelmat](active-directory-saas-tutorial-list.md)
* [Mikä on sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-predictixpricereporting-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-predictixpricereporting-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-predictixpricereporting-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-predictixpricereporting-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-predictixpricereporting-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-predictixpricereporting-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-predictixpricereporting-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-predictixpricereporting-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-predictixpricereporting-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-predictixpricereporting-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-predictixpricereporting-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-predictixpricereporting-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-predictixpricereporting-tutorial/tutorial_general_205.png
