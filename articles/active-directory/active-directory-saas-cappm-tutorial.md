<properties
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi CA PPM | Microsoft Azure"
    description="Opettele määrittämään kertakirjautumisen Azure Active Directory- ja CA PPM välillä."
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
    ms.date="10/10/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-ca-ppm"></a>Opetusohjelma: CA PPM Azure Active Directory-integrointi

Tässä opetusohjelmassa kerrotaan, miten voit integroida CA PPM Azure Active Directory (Azure AD).

Azure AD-integraation CA PPM, jonka avulla seuraavat edut:

- Voit hallita Azure AD, kenellä on oikeus CA PPM
- Voit ottaa käyttöön automaattisesti Hae kirjautunut käyttöön CA PPM (kertakirjautumisen) käyttäjille heidän Azure AD-tilien kanssa
- Voit hallita asiakkaiden yhdessä keskitetyssä sijainnissa - Azure perinteinen portal

Jos haluat tietää tarkempia tietoja SaaS sovelluksen integrointi Azure AD-kohdassa [mitä sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Edellytykset

Määrittää Azure AD-integroinnin CA PPM, tarvitset seuraavat:

- Azure AD-tilaus
- CA PPM single-merkki käytössä-tilauksessa


> [AZURE.NOTE] Voit esikatsella vaiheet Tässä opetusohjelmassa, Emme suosittele käyttämällä tuotantoympäristössä.


Voit esikatsella vaiheet Tässä opetusohjelmassa-noudattamalla näitä suosituksia:

- Älä käytä tuotantoympäristössä, ellei se on tarpeen.
- Jos sinulla ei ole kokeiluversion Azure AD-ympäristössä, voit hankkia yksi kuukausi kokeiluversion [tähän](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Skenaarion kuvaus
Tässä opetusohjelmassa testaat Azure AD kertakirjautumisen testiympäristössä.

Tässä opetusohjelmassa kuvatut skenaarion kuuluu kaksi tärkeimmät rakenneosien:

1. CA PPM lisääminen valikoimasta
2. Yksittäinen määrittäminen ja testaus Azure AD-on


## <a name="adding-ca-ppm-from-the-gallery"></a>CA PPM lisääminen valikoimasta
Voit määrittää CA PPM integroida Azure AD-haluat lisätä CA PPM valikoimasta hallitun SaaS sovellusluettelo.

**Lisää CA PPM valikoimasta, toimi seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen portal**, valitse vasemmassa siirtymisruudussa.

    ![Active Directory][1]
2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset][2]

4. Valitse sivun alareunassa **Lisää** .

    ![Sovellukset][3]

5. Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Sovellukset][4]

6. Kirjoita hakuruutuun **CA PPM**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-cappm-tutorial/tutorial_cappm_01.png)
7. Valitse tulosruudussa **CA PPM**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-cappm-tutorial/tutorial_cappm_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yksittäinen määrittäminen ja testaus Azure AD-on
Tässä osassa Määritä ja testaa Azure AD kertakirjautumisen CA PPM nimeltä "Britta Simon" testikäyttäjän perusteella.

Kertakirjautumisen toimimaan Azure AD on tiedettävä käyttäjälle on CA PPM vastaavaan käyttäjän Azure AD. Toisin sanoen suhteen Azure AD-käyttäjä ja Aiheeseen liittyvät CA PPM-linkki on vahvistettava.

Linkki-yhteys on muodostettu määrittämällä **käyttäjänimi** arvo arvoksi CA PPM **käyttäjänimi** Azure AD.

Määrittäminen ja testaaminen Azure AD kertakirjautumisen CA ppm: llä, sinun on tehtävä seuraavat hakukyselyn:

1. **[Azure AD määrittäminen Single Sign](#configuring-azure-ad-single-sign-on)** - käyttäjät voivat käyttää tätä toimintoa.
2. **[Käyttäjän testata luominen Azure AD](#creating-an-azure-ad-test-user)** - Testaa Azure AD kertakirjautumisen Britta Simon kanssa.
3. **[Käyttäjän luominen CA PPM testata](#creating-an-ca-ppm-test-user)** - on tapahtumista Britta Simon CA PPM, jotka on linkitetty hän Azure AD-esittäminen.
4. **[Käyttäjän testata määrittäminen Azure AD](#assigning-the-azure-ad-test-user)** - Britta Simon käyttämään Azure AD kertakirjautumisen käyttöön.
5. **[Testaus Single Sign](#testing-single-sign-on)** - voit tarkistaa, toimiiko määritykset.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD-Single Sign-On määrittäminen

Tässä osassa Azure AD kertakirjautumisen perinteinen portaalissa ottaminen käyttöön ja määritä kertakirjautumisen CA PPM-sovelluksessa.


**Määrittää Azure AD kertakirjautumisen CA PPM, toimimalla seuraavasti:**

1. Valitse perinteinen-portaalissa integrointi **CA PPM** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen Single Sign** -valintaikkunan avaaminen.
     
    ![Kertakirjautumisen määrittäminen][6] 

2. **Miten käyttäjät voivat kirjautua CA PPM haluat käyttää** sivulla Valitse **Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-cappm-tutorial/tutorial_cappm_03.png) 

3. Valitse **Määritä sovelluksen asetukset** -sivulla toimimalla seuraavasti:

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-cappm-tutorial/tutorial_cappm_04.png) 

    a. Kirjoita **tunnus** -tekstiruutuun URL-osoite käyttäjien Sign CA PPM-sovellukseen käyttämällä seuraavaa kaavaa: **https://ca.ondemand.saml.20.post.\< yrityksen nimi\>**.
    
    b. Kirjoita **Vastaus URL** -tekstiruutuun **https://fedsso.ondemand.ca.com/affwebservices/public/saml2assertionconsumer**

    c-näppäinyhdistelmää. Valitse **Seuraava**
 
4. Valitse **Määritä kertakirjautumisen osoitteessa CA PPM** -sivulla toimimalla seuraavasti:

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-cappm-tutorial/tutorial_cappm_05.png)

    a. Valitse **Lataa**ja tallenna sitten tiedosto tietokoneesta.

    b. Valitse **Seuraava**.


5. Saat määritetty sovelluksen SSO- [CA tekniseltä](mailto:catechnicalsupport@ca.com) tuelta ja antaa niille seuraavasti:

    • Ladatut varmenne

    • **Kohteen tunnus**


6. Perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Seuraava**.
    
    ![Azure AD-Single Sign-On][10]

7. Valitse **Sign Vahvista** -sivulla **Valmis**.  
 
    ![Azure AD-Single Sign-On][11]


### <a name="creating-an-azure-ad-test-user"></a>Azure AD-testikäyttäjän luominen
Tässä osassa voit luoda testikäyttäjän kutsutaan Britta Simon perinteinen portaalissa.


![Luo Azure AD-käyttäjä][20]

**Voit luoda testikäyttäjän Azure AD-toimimalla seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen portal**, valitse vasemmassa siirtymisruudussa.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-cappm-tutorial/create_aaduser_09.png) 

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Ylä-valikossa käyttäjäluettelon, näkyviin napsauttamalla **käyttäjät**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-cappm-tutorial/create_aaduser_03.png) 

4. Avaa **Lisää käyttäjä** -valintaikkunan alareunassa työkalurivillä, valitse **Lisää käyttäjä**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-cappm-tutorial/create_aaduser_04.png) 

5. Suorita valintaikkunan **Kerro kyseisen käyttäjän tietoja** -sivulla seuraavat toimet:  ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-cappm-tutorial/create_aaduser_05.png) 

    a. Käyttäjänä tyyppi Valitse uusi käyttäjä organisaatiossa.

    b. Kirjoita käyttäjänimi- **tekstiruutuun** **BrittaSimon**.

    c-näppäinyhdistelmää. Valitse **Seuraava**.

6.  Suorita valintaikkunan **Käyttäjäprofiili** -sivulla seuraavat toimet: ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-cappm-tutorial/create_aaduser_06.png) 

    a. Kirjoita **Ensimmäinen nimi** -tekstiruutuun **Britta**.  

    b. Kirjoita **Sukunimi** -tekstiruutuun tyyppi- **Simon**.

    c-näppäinyhdistelmää. Kirjoita **Näyttönimi** -tekstiruutuun **Britta Simon**.

    d. Valitse **rooli** -luettelosta **käyttäjä**.

    e. Valitse **Seuraava**.

7. Valitse **Hae tilapäinen salasana** -valintaikkunan sivulla **Luo**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-cappm-tutorial/create_aaduser_07.png) 

8. **Hae tilapäinen salasana** -valintaikkunan sivulla toimimalla seuraavasti:

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-cappm-tutorial/create_aaduser_08.png) 

    a. Kirjoita **Uusi salasana**arvo muistiin.

    b. Valitse **Valmis**.   



### <a name="creating-an-ca-ppm-test-user"></a>CA PPM-testikäyttäjän luominen

Tässä osassa nimeltä Britta Simon CA PPM käyttäjän luominen. Toimi CA PPM tukihenkilöstöösi voit lisätä käyttäjiä CA PPM-ympäristössä.


### <a name="assigning-the-azure-ad-test-user"></a>Azure AD-testikäyttäjän määrittäminen

Tässä osassa voit ottaa käyttöön Britta Simon käyttämään Azure kertakirjautumisen hänen käyttöoikeuksien myöntämistä CA PPM.

![Määrittää käyttäjälle][200] 

**Jos haluat määrittää Britta Simon CA PPM, toimi seuraavasti:**

1. Perinteinen-portaalissa Avaa sovellukset-näkymässä kansio-näkymässä, valitse **sovellukset** yläreunan-valikossa.

    ![Määrittää käyttäjälle][201] 

2. Valitse sovellukset-luettelosta **CA PPM**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-cappm-tutorial/tutorial_cappm_50.png) 

3. Valitse ylä-valikossa Valitse **käyttäjiä**.

    ![Määrittää käyttäjälle][203]

4. Valitse käyttäjät-luettelosta **Britta Simon**.

5. Napsauta työkalurivin alaosassa **määrittäminen**.

    ![Määrittää käyttäjälle][205]


### <a name="testing-single-sign-on"></a>Kertakirjautumisen testaaminen

Tässä osassa voit testata Azure AD yksittäisen Sign-asetusten käyttäminen Access-paneeli.

Access-ruudussa CA PPM-ruutua napsauttaessasi voit olisi Hae automaattisesti kirjautunut sisään CA PPM-sovellukseen.


## <a name="additional-resources"></a>Lisäresursseja

* [Luettelo siitä, miten voit integroida SaaS sovellusten Azure Active Directory-opetusohjelmat](active-directory-saas-tutorial-list.md)
* [Mikä on sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-cappm-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cappm-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cappm-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cappm-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-cappm-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-cappm-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-cappm-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-cappm-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-cappm-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-cappm-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-cappm-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-cappm-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-cappm-tutorial/tutorial_general_205.png
