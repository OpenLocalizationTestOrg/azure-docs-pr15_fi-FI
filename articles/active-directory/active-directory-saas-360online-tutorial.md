<properties
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi 360° | Microsoft Azure"
    description="Opettele määrittämään kertakirjautumisen Azure Active Directory ja 360° välillä."
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


# <a name="tutorial-azure-active-directory-integration-with-360-online"></a>Opetusohjelma: 360° Azure Active Directory-integrointi


Tässä opetusohjelmassa tavoitteena on, kuinka voit integroida 360° Azure Active Directory (Azure AD).

Paikallisen ympäristön integroinnissa 360° online-palvelun kanssa Azure AD löytyy seuraavat edut:

- Voit hallita Azure AD, joilla on 360° verkkopalvelimen käyttöoikeus
- Voit ottaa käyttöön automaattisesti Hae kirjautunut sisään 360 asteen Online (kertakirjautumisen) niiden Azure AD-tilien käyttäjille
- Voit hallita asiakkaiden yhdessä keskitetyssä sijainnissa - Azure perinteinen portal

Jos haluat tietää tarkempia tietoja SaaS sovelluksen integrointi Azure AD-kohdassa [mitä sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Edellytykset

Määrittää Azure AD-integroinnin 360°, tarvitset seuraavat:

- Azure AD-tilaus
- 360° Online-vuokraajan


> [AZURE.NOTE] Voit esikatsella vaiheet Tässä opetusohjelmassa, Emme suosittele käyttämällä tuotantoympäristössä.


Voit esikatsella vaiheet Tässä opetusohjelmassa-noudattamalla näitä suosituksia:

- Älä käytä tuotantoympäristössä, ellei se on tarpeen.
- Jos sinulla ei ole kokeiluversion Azure AD-ympäristössä, voit hankkia yksi kuukausi kokeiluversion [tähän](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Skenaarion kuvaus
Tässä opetusohjelmassa tavoitteena on, jotta voit testata Azure AD kertakirjautumisen testiympäristössä. 

Tässä opetusohjelmassa kuvatut skenaarion kuuluu kaksi tärkeimmät rakenneosien:

1. 360° lisääminen valikoimasta
2. Yksittäinen määrittäminen ja testaus Azure AD-on



## <a name="adding-360-online-from-the-gallery"></a>360° lisääminen valikoimasta
Voit määrittää 360° integroida Azure AD-haluat lisätä 360° valikoimasta hallitun SaaS sovellusluettelo.


**Jos haluat lisätä 360° valikoimasta, toimi seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen portal**, valitse vasemmassa siirtymisruudussa.

    ![Active Directory][1]

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset][2]

4. Valitse sivun alareunassa **Lisää** .

    ![Sovellukset][3]

5. Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Sovellukset][4]

6. Kirjoita hakuruutuun **360 °**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-360online-tutorial/tutorial_360online_01.png)

7. Valitse tulosruudussa **360 °**ja valitse **Valmis** , voit lisätä sovelluksen.
 
    ![Sovellukset](./media/active-directory-saas-360online-tutorial/tutorial_360online_06.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yksittäinen määrittäminen ja testaus Azure AD-on
Tässä osassa tavoitteena on noudattamalla voit määrittää ja testaa Azure AD kertakirjautumisen 360 ° Online perusteella nimeltä "Britta Simon" testikäyttäjän.

Kertakirjautumisen toimimaan Azure AD on tietää, mitä vastaavaan käyttäjän Online Azure AD-käyttäjälle on 360°. Toisin sanoen linkki suhteen Azure AD-käyttäjä ja Aiheeseen liittyvät 360 ° Online on vahvistettava.


Määrittäminen ja testaaminen Azure AD kertakirjautumisen 360 °, sinun on tehtävä seuraavat hakukyselyn:

1. **[Azure AD määrittäminen Single Sign](#configuring-azure-ad-single-sign-on)** - käyttäjät voivat käyttää tätä toimintoa.
2. **[Käyttäjän testata luominen Azure AD](#creating-an-azure-ad-test-user)** - Testaa Azure AD kertakirjautumisen Britta Simon kanssa.
4. **[Käyttäjän luominen verkossa 360 ° testata](#creating-a-360-online-test-user)** - on vastaavaan, Britta Simon 360 ° Online, jotka on linkitetty hän Azure AD-esittäminen.
5. **[Käyttäjän testata määrittäminen Azure AD](#assigning-the-azure-ad-test-user)** - Britta Simon käyttämään Azure AD kertakirjautumisen käyttöön.
5. **[Testaus Single Sign](#testing-single-sign-on)** - voit tarkistaa, toimiiko määritykset.


### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD-Single Sign-On määrittäminen

Tässä osassa tavoitteena on Azure AD kertakirjautumisen Azure perinteinen portaalissa käyttöön ja määrittäminen kertakirjautumisen 360° Onlinea.

**Voit määrittää Azure AD kertakirjautumisen 360 ° toimimalla seuraavasti:**


1. Azure perinteinen portal-integroinnin **360 °** sovellus-sivulla valitsemalla **Määritä kertakirjautumisen** **Määrittäminen Single Sign** -valintaikkunan avaaminen.

    ![Kertakirjautumisen määrittäminen][13] 

2. **Miten haluat kirjautua sisään 360 ° käyttäjät** -sivulla Valitse **Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-360online-tutorial/tutorial_360online_03.png) 

3. Valitse **Määritä sovelluksen URL-osoite** -sivulla suorittamalla seuraavat vaiheet ja valitse sitten **Seuraava**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-360online-tutorial/tutorial_360online_04.png)

    a. Kirjoita **Merkki-URL** -tekstiruutuun URL-osoite käyttämä käyttäjille Sign 360 asteen Onlinea käyttämällä seuraavaa kaavaa:`https://<company name>.public360online.com`

    b. Valitse **Seuraava**

4. Valitse **Määritä sovelluksen URL-osoite** -sivulla suorittamalla seuraavat vaiheet ja valitse sitten **Seuraava**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-360online-tutorial/tutorial_360online_05.png) 

    a. Valitse **Lataa metatiedot**ja tallenna se tietokoneeseen.

    b. Valitse **Seuraava**.


5. Saat määritetty sovelluksen SSO-yhteyden kautta 360° Online tukeen [360online@software-innovation.com](mailto:360online@software-innovation.com) ja liittää ladatut metatietojen tiedoston sähköpostisi.

6. Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Seuraava**.

    ![Azure AD-Single Sign-On][10]

7. Valitse **Sign Vahvista** -sivulla **Valmis**.  

    ![Azure AD-Single Sign-On][11]



### <a name="creating-an-azure-ad-test-user"></a>Azure AD-testikäyttäjän luominen
Tässä osassa tavoitteena on luotava testikäyttäjän kutsutaan Britta Simon Azure perinteinen portaalissa.


![Luo Azure AD-käyttäjä][20]

**Voit luoda testikäyttäjän Azure AD-toimimalla seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen portal**, valitse vasemmassa siirtymisruudussa.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-360online-tutorial/create_aaduser_09.png) 

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Ylä-valikossa käyttäjäluettelon, näkyviin napsauttamalla **käyttäjät**.
 
    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-360online-tutorial/create_aaduser_03.png) 


4. Avaa **Lisää käyttäjä** -valintaikkunan alareunassa työkalurivillä, valitse **Lisää käyttäjä**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-360online-tutorial/create_aaduser_04.png)

5. Tee valintaikkunassa **Kerro kyseisen käyttäjän tietoja** -sivulla seuraavasti:

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-360online-tutorial/create_aaduser_05.png) 

    a. **Käyttäjän tyyppi**Valitse **Uusi käyttäjä organisaatiossa**.

    b. Kirjoita **Käyttäjänimi** -tekstiruutuun **BrittaSimon**.

    c-näppäinyhdistelmää. Valitse **Seuraava**.

6.  Suorita **Käyttäjäprofiili** -sivulla seuraavat toimet:

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-360online-tutorial/create_aaduser_06.png) 

    a. Kirjoita **Ensimmäinen nimi** -tekstiruutuun **Britta**.  

    b. Kirjoita **Sukunimi** -tekstiruutuun tyyppi- **Simon**.

    c-näppäinyhdistelmää. Kirjoita **Näyttönimi** -tekstiruutuun **Britta Simon**.

    d. Valitse **rooli** -luettelosta **käyttäjä**.

    e. Valitse **Seuraava**.

7. Valitse **Hae tilapäinen salasana** -valintaikkunan sivulla **Luo**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-360online-tutorial/create_aaduser_07.png) 


8. **Hae tilapäinen salasana** -valintaikkunan sivulla toimimalla seuraavasti:
 
    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-360online-tutorial/create_aaduser_08.png) 

    a. Kirjoita **Uusi salasana**arvo muistiin.

    b. Valitse **Valmis**.   




### <a name="creating-a-360-online-test-user"></a>360° Online testikäyttäjän luominen

Tässä osassa tavoitteena on nimeltään Britta Simon 360° käyttäjän luominen. 

Saat käyttäjän 360° luonut Online-haluat tukihenkilöstöltä 360° Online kautta [360online@software-innovation.com](mailto:360online@software-innovation.com).


### <a name="assigning-the-azure-ad-test-user"></a>Azure AD-testikäyttäjän määrittäminen

Tässä osassa tavoitteena on Britta Simon käyttämään Azure kertakirjautumisen hänen 360° käyttöoikeuksien myöntäminen ottaminen käyttöön.

![Määrittää käyttäjälle][200] 

**Jos haluat määrittää Britta Simon 360°, toimi seuraavasti:**

1. Azure perinteinen-portaalissa Avaa sovellukset-näkymässä kansio-näkymässä, valitse **sovellukset** yläreunan-valikossa.
 
    ![Määrittää käyttäjälle][201] 


2. Valitse sovellukset-luettelosta **360 °**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-360online-tutorial/tutorial_360online_50.png) 


1. Valitse ylä-valikossa Valitse **käyttäjiä**.

    ![Määrittää käyttäjälle][203]

1. Valitse käyttäjät-luettelosta **Britta Simon**.

2. Napsauta työkalurivin alaosassa **määrittäminen**.

    ![Määrittää käyttäjälle][205]



### <a name="testing-single-sign-on"></a>Kertakirjautumisen testaaminen

Tässä osassa tavoitteena on Testaa Azure AD yksittäisen Sign-asetusten käyttäminen Access-paneeli.

Kun napsautat verkossa 360° ruutu Access-ruudussa, sinun kannattaa Hae automaattisesti kirjautunut käyttöön 360° Onlinea.


## <a name="additional-resources"></a>Lisäresursseja

* [Luettelo siitä, miten voit integroida SaaS sovellusten Azure Active Directory-opetusohjelmat](active-directory-saas-tutorial-list.md)
* [Mikä on sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta?](active-directory-appssoaccess-whatis.md)







<!--Image references-->

[1]: ./media/active-directory-saas-360online-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-360online-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-360online-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-360online-tutorial/tutorial_general_04.png

[10]: ./media/active-directory-saas-360online-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-360online-tutorial/tutorial_general_07.png
[12]: ./media/active-directory-saas-360online-tutorial/tutorial_general_08.png
[13]: ./media/active-directory-saas-360online-tutorial/tutorial_general_09.png
[20]: ./media/active-directory-saas-360online-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-360online-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-360online-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-360online-tutorial/tutorial_general_203.png
[205]: ./media/active-directory-saas-360online-tutorial/tutorial_general_205.png
