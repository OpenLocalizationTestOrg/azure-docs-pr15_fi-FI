<properties
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi Flatter tiedostoja | Microsoft Azure"
    description="Opettele määrittämään kertakirjautumisen Azure Active Directory- ja Flatter tiedostojen välillä."
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
    ms.date="09/29/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-flatter-files"></a>Opetusohjelma: Flatter tiedostojen Azure Active Directory-integrointi

Tässä opetusohjelmassa tavoitteena on, kuinka voit integroida Flatter tiedostoja Azure Active Directory (Azure AD).  
Azure AD-integraation Flatter tiedostoja, jonka avulla seuraavat edut: 

- Voit hallita Azure AD, joilla on käyttöoikeus Flatter tiedostot 
- Voit ottaa käyttöön automaattisesti Hae kirjautunut sisään Flatter tiedostoja (kertakirjautumisen) käyttäjille heidän Azure AD-tilien kanssa
- Voit hallita asiakkaiden keskitetyn paikkaan – perinteinen Azure Active Directory-portaalissa

Jos haluat tietää tarkempia tietoja SaaS sovelluksen integrointi Azure AD-kohdassa [mitä sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Edellytykset 

Azure AD-integroinnin määrittäminen Flatter-tiedostoja, tarvitset seuraavat:

- Azure AD-tilaus
- Flatter tiedostot single-merkki käytössä-tilauksessa


> [AZURE.NOTE] Voit esikatsella vaiheet Tässä opetusohjelmassa, Emme suosittele käyttämällä tuotantoympäristössä.


Voit esikatsella vaiheet Tässä opetusohjelmassa-noudattamalla näitä suosituksia:

- Älä käytä tuotantoympäristössä, ellei se on tarpeen.
- Jos sinulla ei ole kokeiluversion Azure AD-ympäristössä, voit hankkia yksi kuukausi kokeiluversion [tähän](https://azure.microsoft.com/pricing/free-trial/). 

 
## <a name="scenario-description"></a>Skenaarion kuvaus
Tässä opetusohjelmassa tavoitteena on, jotta voit testata Azure AD kertakirjautumisen testiympäristössä.  
Tässä opetusohjelmassa kuvatut skenaarion kuuluu kaksi tärkeimmät rakenneosien:

1. Flatter tiedostojen lisääminen valikoimasta 
2. Yksittäinen määrittäminen ja testaus Azure AD-on


## <a name="adding-flatter-files-from-the-gallery"></a>Flatter tiedostojen lisääminen valikoimasta
Voit määrittää Flatter tiedostoja integroida Azure AD-haluat Flatter tiedostojen lisääminen valikoimasta hallitun SaaS sovellusluettelo.

**Voit lisätä Flatter tiedostoja valikoimasta toimimalla seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen portal**, valitse vasemmassa siirtymisruudussa. 

    ![Active Directory][1]

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset][2]

4. Valitse sivun alareunassa **Lisää** .

    ![Sovellukset][3]

5. Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Sovellukset][4]

6. Kirjoita hakuruutuun **Flatter tiedostot**.


    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_01.png)

7. Valitse tulokset-ruudun **Flatter**tiedostot ja valitse sitten **Valmis** , voit lisätä sovelluksen.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_500.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yksittäinen määrittäminen ja testaus Azure AD-on
Tässä osassa tavoitteena on noudattamalla voit määrittää ja testaa Azure AD kertakirjautumisen Flatter tiedostoja nimeltä "Britta Simon" testikäyttäjän perusteella.

Kertakirjautumisen toimimaan Azure AD on tiedettävä vastaavaan käyttäjän Flatter tiedostoja, Azure AD-käyttäjälle. Toisin sanoen linkki suhteen Azure AD-käyttäjä ja Aiheeseen liittyvät Flatter tiedostoissa on vahvistettava.  
Linkki-yhteys on muodostettu määrittämällä **käyttäjänimi** arvo arvoksi Flatter tiedostoja **käyttäjänimi** Azure AD.
 
Määrittäminen ja testaaminen Azure AD kertakirjautumisen Flatter-tiedostoja, sinun on tehtävä seuraavat hakukyselyn:

1. **[Azure AD määrittäminen Single Sign](#configuring-azure-ad-single-single-sign-on)** - käyttäjät voivat käyttää tätä toimintoa.
2. **[Käyttäjän testata luominen Azure AD](#creating-an-azure-ad-test-user)** - Testaa Azure AD kertakirjautumisen Britta Simon kanssa.
4. **[Flatter tiedostojen luominen testata käyttäjän](#creating-a-halogen-software-test-user)** - on tapahtumista Britta Simon Flatter tiedostoissa, jotka on linkitetty hän Azure AD esitys.
5. **[Käyttäjän testata määrittäminen Azure AD](#assigning-the-azure-ad-test-user)** - Britta Simon käyttämään Azure AD kertakirjautumisen käyttöön.
5. **[Testaus Single Sign](#testing-single-sign-on)** - voit tarkistaa, toimiiko määritykset.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD kertakirjautumisen määrittäminen

Tässä osassa tavoitteena on Azure AD kertakirjautumisen perinteinen Azure AD-portaalissa ottaminen käyttöön ja määrittää kertakirjautumisen Flatter tiedostot-sovelluksessa. Tämän toimintosarjan osana on base-64-koodattu sertifikaattitiedosto luomiseen. Jos et ole tottunut näin, katso, [miten voit muuntaa binaariluvun varmenteen tekstitiedostoon](http://youtu.be/PlgrzUZ-Y1o).

Määrittäminen kertakirjautumisen Flatter tiedostoja, sinun on rekisteröity toimialue. Jos sinulla ei ole rekisteröity toimialue vielä, yhteyshenkilön Flatter tiedostojen tekniseen tukeen kautta [support@flatterfiles.com](mailto:support@flatterfiles.com).  



**Jos haluat määrittää Azure AD kertakirjautumisen Flatter-tiedostoja, toimimalla seuraavasti:**

1. Azure AD perinteinen-portaalissa, integrointi **Flatter tiedostoja** sovellus-sivulla valitsemalla **Määritä kertakirjautumisen** **Määrittäminen Single Sign** -valintaikkunan avaaminen.

    ![Kertakirjautumisen määrittäminen][6] 

2. **Miten käyttäjät voivat kirjautua Flatter tiedostoja haluat käyttää** sivulla Valitse **Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.
 
    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_02.png) 

3. Valitse **Määritä sovelluksen asetukset** -sivulla **Seuraava**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_03.png) 

    > [AZURE.NOTE] Flatter tiedostot käyttää kaikkien asiakkaiden SSO kirjautuminen URL-Osoitetta: [https://www.flatterfiles.com/site/login/sso/](https://www.flatterfiles.com/site/login/sso/).
.
 
 
4. Valitse **Määritä kertakirjautumisen Flatter tiedostot** -sivulla toimimalla seuraavasti:

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_04.png)  

    a. Valitse **Lataa**ja tallenna sitten tiedosto tietokoneesta.

    b. Valitse **Seuraava**.


1. Kirjaudu sisään järjestelmänvalvojana Flatter tiedostot-sovellukseen.

2. Valitse koontinäyttö. 

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_05.png)  



2. Valitse **asetukset**ja tee **yrityksen** -välilehdessä seuraavasti: 

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_06.png)  

    a. Valitse **Käytä SAML 2.0 todennusta varten**.

    b. Valitse **Määritä SAML**.



2. **SAML Configuration** -valintaikkunassa seuraavasti: 

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_08.png)  

    a. Kirjoita rekisteröidyn toimialueesi Domain-tekstiruutuun.

    > [AZURE.NOTE] Jos sinulla ei ole rekisteröity toimialue vielä, yhteyshenkilön Flatter tiedostojen tekniseen tukeen kautta [support@flatterfiles.com](mailto:support@flatterfiles.com).
    
    b. Azure-perinteinen-portaaliin, valitse yksittäinen Määritä Sign-on Flatter tiedostot-valintaikkunan copt yhden Sign-palvelun URL-osoite ja liitä se sitten tunnistetietojen toimittaja URL-tekstiruutuun.

    c-näppäinyhdistelmää.  Luo **base-64-koodattu** tiedosto ladattu varmennetta.  

    >[AZURE.TIP] Lisätietoja on artikkelissa [muuntaa binaariluvun varmenteen tekstitiedostoon](http://youtu.be/PlgrzUZ-Y1o)

    d.  Avaa base-64-koodattu varmennetta Muistiossa, kopioi Leikepöydän sisällön se ja liitä se **FlatterFiles tunnistetietojen toimittaja varmenne** -tekstiruutuun.

    e. Valitse **Päivitä**.

6. Valitse perinteinen Azure AD-portaalissa Valitse Sign määritysten vahvistus ja valitse sitten **Seuraava**. 

    ![Azure AD-Single Sign-On][10]

7. Valitse **Sign Vahvista** -sivulla **Valmis**.  

    ![Azure AD-Single Sign-On][11]




### <a name="creating-an-azure-ad-test-user"></a>Azure AD-testikäyttäjän luominen
Tässä osassa tavoitteena on luotava testikäyttäjän kutsutaan Britta Simon Azure perinteinen portaalissa.

![Luo Azure AD-käyttäjä][20]

**Voit luoda testikäyttäjän Azure AD-toimimalla seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen portal**, valitse vasemmassa siirtymisruudussa.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_09.png) 

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Ylä-valikossa käyttäjäluettelon, näkyviin napsauttamalla **käyttäjät**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_03.png) 
 
4. Avaa **Lisää käyttäjä** -valintaikkunan alareunassa työkalurivillä, valitse **Lisää käyttäjä**. 

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_04.png) 

5. Tee valintaikkunassa **Kerro kyseisen käyttäjän tietoja** -sivulla seuraavasti: 

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_05.png)  

    a. Käyttäjänä tyyppi Valitse uusi käyttäjä organisaatiossa.

    b. Kirjoita käyttäjänimi- **tekstiruutuun** **BrittaSimon**.

    c-näppäinyhdistelmää. Valitse **Seuraava**.

6.  Suorita **Käyttäjäprofiili** -sivulla seuraavat toimet: 

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_06.png) 
 
    a. Kirjoita **Ensimmäinen nimi** -tekstiruutuun **Britta**.  

    b. Kirjoita **Sukunimi** -tekstiruutuun tyyppi- **Simon**.

    c-näppäinyhdistelmää. Kirjoita **Näyttönimi** -tekstiruutuun **Britta Simon**.

    d. Valitse **rooli** -luettelosta **käyttäjä**.
    e. Valitse **Seuraava**.

7. Valitse **Hae tilapäinen salasana** -valintaikkunan sivulla **Luo**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_07.png) 
 
8. **Hae tilapäinen salasana** -valintaikkunan sivulla toimimalla seuraavasti:

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_08.png) 
  
    a. Kirjoita **Uusi salasana**arvo muistiin.

    b. Valitse **Valmis**.   

  
 
### <a name="creating-a-flatter-files-test-user"></a>Flatter tiedostot-testikäyttäjän luominen

Tässä osassa tavoitteena on nimeltään Britta Simon Flatter tiedostoja käyttäjän luominen.

**Luo käyttäjä kutsutaan Britta Simon Flatter tiedostoja, toimi seuraavasti:**

1. Kirjaudu **Flatter tiedostoja** yrityksesi sivuston järjestelmänvalvojana.

2. Vasemman reunan siirtymisruudussa **asetukset**ja valitse sitten käyttäjät- **välilehti**.

    ![Cfreate Flatter tiedostot-käyttäjä](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_09.png)

3. Valitse **Lisää käyttäjä**. 

4. Valitse **Lisää käyttäjä** -valintaikkunassa seuraavasti:

    ![Cfreate Flatter tiedostot-käyttäjä](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_10.png)

    a. Kirjoita **Ensimmäinen nimi** -tekstiruutuun **Britta**.

    b. Kirjoita **Simon** **Sukunimi** -tekstiruutuun. 

    c-näppäinyhdistelmää. Kirjoita **Sähköpostiosoite** -tekstiruutuun Azure perinteinen portaalissa Britta henkilön sähköpostiosoite.

    d. Valitse **Lähetä**.   


### <a name="assigning-the-azure-ad-test-user"></a>Azure AD-testikäyttäjän määrittäminen

Tässä osassa tavoitteena on ottaminen käyttöön Britta Simon käyttämään Azure kertakirjautumisen hänen käyttöoikeuden myöntämisestä Flatter tiedostot.

![Määrittää käyttäjälle][200] 

**Jos haluat määrittää Britta Simon Flatter tiedostoja, toimi seuraavasti:**

1. Azure perinteinen-portaalissa Avaa sovellukset-näkymässä kansio-näkymässä, valitse **sovellukset** yläreunan-valikossa.

    ![Määrittää käyttäjälle][201] 

2. Valitse sovellukset-luettelosta **Flatter tiedostot**.

    ![Määrittää käyttäjälle](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_11.png) 

1. Valitse ylä-valikossa Valitse **käyttäjiä**.

    ![Määrittää käyttäjälle][203] 

1. Valitse käyttäjät-luettelosta **Britta Simon**.

2. Napsauta työkalurivin alaosassa **määrittäminen**.

    ![Määrittää käyttäjälle][205]



### <a name="testing-single-sign-on"></a>Kertakirjautumisen testaaminen

Tässä osassa tavoitteena on Testaa Azure AD yksittäisen Sign-asetusten käyttäminen Access-paneeli.  
Access-ruudussa Flatter tiedostot-ruutua napsauttaessasi voit olisi Hae automaattisesti kirjautunut sisään Flatter tiedostot-sovellukseen.


## <a name="additional-resources"></a>Lisäresursseja

* [Luettelo siitä, miten voit integroida SaaS sovellusten Azure Active Directory-opetusohjelmat](active-directory-saas-tutorial-list.md)
* [Mikä on sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_205.png






