<properties
    pageTitle="Opetusohjelma: Microsoft Azure Cloud hallinta-portaalin Azure Active Directory-integrointi | Microsoft Azure"
    description="Opettele määrittämään kertakirjautumisen Azure Active Directory- ja Microsoft Azure Cloud hallinta-portaalin välillä."
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
    ms.date="08/16/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-cloud-management-portal-for-microsoft-azure"></a>Opetusohjelma: Microsoft Azure Cloud hallinta-portaalin Azure Active Directory-integrointi

Tässä opetusohjelmassa tavoitteena on, kuinka voit integroida Azure Active Directory (Azure AD) Microsoft Azure Cloud hallinta-portaalin.  
Microsoft Azure Cloud hallinta-portaalin integraation Azure AD, jonka avulla seuraavat edut:

- Voit hallita Azure AD, kenellä on oikeus Cloud hallinta-portaalin Microsoft Azure varten
- Voit ottaa käyttöön automaattisesti Hae kirjautunut käyttöön Microsoft Azure (kertakirjautumisen) Cloud hallinta-portaalin käyttäjille heidän Azure AD-tilien kanssa
- Voit hallita asiakkaiden yhdessä keskitetyssä sijainnissa - Azure perinteinen portal


Jos haluat tietää tarkempia tietoja SaaS sovelluksen integrointi Azure AD-kohdassa [mitä sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Edellytykset

Azure AD-integroinnin määrittäminen Microsoft Azure Cloud hallinta-portaalissa, tarvitset seuraavat:

- Azure AD-tilaus
- Cloud hallinta-portaalin Microsoft Azure single-kirjauduttaessa käytössä-tilauksessa


> [AZURE.NOTE] Voit esikatsella vaiheet Tässä opetusohjelmassa, Emme suosittele käyttämällä tuotantoympäristössä.


Voit esikatsella vaiheet Tässä opetusohjelmassa-noudattamalla näitä suosituksia:

- Älä käytä tuotantoympäristössä, ellei se on tarpeen.
- Jos sinulla ei ole kokeiluversion Azure AD-ympäristössä, voit hankkia yksi kuukausi kokeiluversion [tähän](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Skenaarion kuvaus
Tässä opetusohjelmassa tavoitteena on, jotta voit testata Azure AD kertakirjautumisen testiympäristössä.  
Tässä opetusohjelmassa kuvatut skenaarion kuuluu kaksi tärkeimmät rakenneosien:

1. Microsoft Azure Cloud hallinta-portaalin lisääminen valikoimasta
2. Yksittäinen määrittäminen ja testaus Azure AD-on


## <a name="adding-cloud-management-portal-for-microsoft-azure-from-the-gallery"></a>Microsoft Azure Cloud hallinta-portaalin lisääminen valikoimasta
Voit määrittää Microsoft Azure Cloud hallinta-portaalin integroida Azure AD-haluat lisätä Microsoft Azure Cloud hallinta-portaalin valikoimasta hallitun SaaS sovellusluettelo.

**Jos haluat lisätä Microsoft Azure Cloud hallinta-portaalin valikoimasta, toimi seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen portal**, valitse vasemmassa siirtymisruudussa. 

    ![Active Directory][1]

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset][2]

4. Valitse sivun alareunassa **Lisää** .

    ![Sovellukset][3]

5. Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Sovellukset][4]

6. Kirjoita hakuruutuun **Microsoft Azure Cloud hallinta-portaalin**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-newsignature-tutorial/tutorial_newsignature_01.png)

7. Valitse tulosruudussa **Microsoft Azure Cloud hallinta-portaalin**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-newsignature-tutorial/tutorial_newsignature_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yksittäinen määrittäminen ja testaus Azure AD-on
Tässä osassa tavoitteena on noudattamalla voit määrittää ja testaa Azure AD kertakirjautumisen Microsoft Azure nimeltä "Britta Simon" testi käyttäjään perustuva Cloud hallinta-portaalissa.

Kertakirjautumisen toimimaan Azure AD on tiedä, mikä on Microsoft Azure Cloud hallinta-portaalissa vastaavaan käyttäjän Azure AD-käyttäjälle. Toisin sanoen linkki suhteen Azure AD-käyttäjä ja Aiheeseen liittyvät Microsoft Azure Cloud hallinta-portaalissa on vahvistettava.  
Linkki-yhteys on muodostettu määrittämällä **käyttäjänimi** arvo arvoksi **käyttäjänimi** Microsoft Azure Cloud hallinta-portaalissa Azure AD.

Määrittäminen ja testaaminen Azure AD kertakirjautumisen Microsoft Azure Cloud hallinta-portaalissa, sinun on tehtävä seuraavat hakukyselyn:

1. **[Azure AD määrittäminen Single Sign](#configuring-azure-ad-single-single-sign-on)** - käyttäjät voivat käyttää tätä toimintoa.
2. **[Käyttäjän testata luominen Azure AD](#creating-an-azure-ad-test-user)** - Testaa Azure AD kertakirjautumisen Britta Simon kanssa.
4. **[Käyttäjän luominen Microsoft Azure Cloud hallinta-portaali testata](#creating-a-newsignature-test-user)** - tapahtumista Britta Simon on Microsoft Azure kyseistä henkilöä hän Azure AD-esittäminen Cloud hallinta-portaalissa.
5. **[Käyttäjän testata määrittäminen Azure AD](#assigning-the-azure-ad-test-user)** - Britta Simon käyttämään Azure AD kertakirjautumisen käyttöön.
5. **[Testaus Single Sign](#testing-single-sign-on)** - voit tarkistaa, toimiiko määritykset.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD-Single Sign-On määrittäminen

Tässä osassa tavoitteena on Azure AD kertakirjautumisen Azure perinteinen portaalissa käyttöön ja määrittäminen kertakirjautumisen Cloud hallinta-portaalin Microsoft Azure-sovelluksen.



**Jos haluat määrittää Azure AD kertakirjautumisen Microsoft Azure Cloud hallinta-portaalissa, toimimalla seuraavasti:**

1. Valitse Azure perinteinen portaalissa integrointi **Microsoft Azure Cloud hallinta-portaalin** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen Single Sign** -valintaikkunan avaaminen.

    ![Kertakirjautumisen määrittäminen][6] 

2. **Miten haluat kirjautua sisään Microsoft Azure Cloud hallinta-portaalin käyttäjät** -sivulla Valitse **Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-newsignature-tutorial/tutorial_newsignature_03.png) 

3. Valitse **Määritä sovelluksen asetukset** -sivulla toimimalla seuraavasti:.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-newsignature-tutorial/tutorial_newsignature_04.png) 

    a. Kirjoita **Merkki-URL** -tekstiruutuun URL-osoite kertakirjauksen käyttöön Cloud hallinta-portaalin käyttäjille käyttämän Microsoft Azure-sovelluksen käyttämällä seuraavaa kaavaa:`https://portal.igcm.com/<instance name>`

    b. Valitse **Seuraava**.


4. Valitse **Määritä kertakirjautumisen osoitteessa Microsoft Azure Cloud hallinta-portaalin** -sivulla toimimalla seuraavasti:

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-newsignature-tutorial/tutorial_newsignature_05.png) 

    a. Valitse **Lataa**ja tallenna sitten tiedosto tietokoneesta.

    b. Valitse **Seuraava**.


5. Hanki SSO sovelluksen määritetty yhteyttä Cloud hallinta-portaalin Microsoft Azure tukeen [jczernuszka@newsignature.com](mailTo:jczernuszka@newsignature.com) ja liitä ladatut sertifikaattitiedosto sähköposti. Myös Anna myöntäjä URL-osoite, SAML SSO URL-osoite ja yhteen Kirjaudu ulos palvelun URL niin, että ne on määritetty SSO-integrointia varten.


6. Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Seuraava**.

    ![Azure AD-Single Sign-On][10]

7. Valitse **Sign Vahvista** -sivulla **Valmis**.  

    ![Azure AD-Single Sign-On][11]



### <a name="creating-an-azure-ad-test-user"></a>Azure AD-testikäyttäjän luominen
Tässä osassa tavoitteena on luotava testikäyttäjän kutsutaan Britta Simon Azure perinteinen portaalissa.  

![Luo Azure AD-käyttäjä][20]

**Voit luoda testikäyttäjän Azure AD-toimimalla seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen portal**, valitse vasemmassa siirtymisruudussa.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-newsignature-tutorial/create_aaduser_09.png) 

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Ylä-valikossa käyttäjäluettelon, näkyviin napsauttamalla **käyttäjät**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-newsignature-tutorial/create_aaduser_03.png) 

4. Avaa **Lisää käyttäjä** -valintaikkunan alareunassa työkalurivillä, valitse **Lisää käyttäjä**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-newsignature-tutorial/create_aaduser_04.png) 

5. Tee valintaikkunassa **Kerro kyseisen käyttäjän tietoja** -sivulla seuraavasti:

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-newsignature-tutorial/create_aaduser_05.png) 

    a. Käyttäjänä tyyppi Valitse uusi käyttäjä organisaatiossa.

    b. Kirjoita käyttäjänimi- **tekstiruutuun** **BrittaSimon**.

    c-näppäinyhdistelmää. Valitse **Seuraava**.

6.  Suorita **Käyttäjäprofiili** -sivulla seuraavat toimet:

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-newsignature-tutorial/create_aaduser_06.png) 

    a. Kirjoita **Ensimmäinen nimi** -tekstiruutuun **Britta**.  

    b. Kirjoita **Sukunimi** -tekstiruutuun tyyppi- **Simon**.

    c-näppäinyhdistelmää. Kirjoita **Näyttönimi** -tekstiruutuun **Britta Simon**.

    d. Valitse **rooli** -luettelosta **käyttäjä**.

    e. Valitse **Seuraava**.

7. Valitse **Hae tilapäinen salasana** -valintaikkunan sivulla **Luo**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-newsignature-tutorial/create_aaduser_07.png) 

8. **Hae tilapäinen salasana** -valintaikkunan sivulla toimimalla seuraavasti:

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-newsignature-tutorial/create_aaduser_08.png) 

    a. Kirjoita **Uusi salasana**arvo muistiin.

    b. Valitse **Valmis**.   



### <a name="creating-a-cloud-management-portal-for-microsoft-azure-test-user"></a>Cloud hallinta-portaalin Microsoft Azure testi käyttäjän luominen

Tässä osassa tavoitteena on kutsua Britta Simon Cloud hallinta-portaalissa Microsoft Azure käyttäjän luominen. Toimi Cloud hallinta-portaalissa Microsoft Azure tukeen, voit lisätä käyttäjiä Microsoft Azure-tilin Cloud hallinta-portaalissa. 


> [AZURE.NOTE]Jos haluat luoda luodun käyttäjän manuaalisesti, on Cloud hallinta-portaalin Pyydä Microsoft Azure-tukeen.


### <a name="assigning-the-azure-ad-test-user"></a>Azure AD-testikäyttäjän määrittäminen

Tässä osassa tavoitteena on Britta Simon käyttämään Azure kertakirjautumisen hänen käyttöoikeuksien myöntämistä Microsoft Azure Cloud hallinta-portaalin ottaminen käyttöön.

![Määrittää käyttäjälle][200] 

**Jos haluat määrittää Britta Simon Microsoft Azure Cloud hallinta-portaalin, toimi seuraavasti:**

1. Azure perinteinen-portaalissa Avaa sovellukset-näkymässä kansio-näkymässä, valitse **sovellukset** yläreunan-valikossa.

    ![Määrittää käyttäjälle][201] 

2. Valitse sovellukset-luettelosta **Microsoft Azure Cloud hallinta-portaalin**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-newsignature-tutorial/tutorial_newsignature_50.png) 

1. Valitse ylä-valikossa Valitse **käyttäjiä**.

    ![Määrittää käyttäjälle][203] 

1. Valitse käyttäjät-luettelosta **Britta Simon**.

2. Napsauta työkalurivin alaosassa **määrittäminen**.

    ![Määrittää käyttäjälle][205]



### <a name="testing-single-sign-on"></a>Kertakirjautumisen testaaminen

Tässä osassa tavoitteena on Testaa Azure AD yksittäisen Sign-asetusten käyttäminen Access-paneeli.  
Kun napsautat Microsoft Azure-ruudun Access-ruudussa Cloud hallinta-portaalissa, voit olisi Hae automaattisesti kirjautunut käyttöön Cloud hallinta-portaalin Microsoft Azure-sovelluksen.


## <a name="additional-resources"></a>Lisäresursseja

* [Luettelo siitä, miten voit integroida SaaS sovellusten Azure Active Directory-opetusohjelmat](active-directory-saas-tutorial-list.md)
* [Mikä on sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-newsignature-tutorial/tutorial_general_205.png
