<properties
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi @Task| Microsoft Azure"
    description="Opettele määrittämään kertakirjautumisen Azure Active Directoryn välillä ja @Task."
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


# <a name="tutorial-azure-active-directory-integration-with-task"></a>Opetusohjelma: Azure Active Directory-integrointi@Task

Tässä opetusohjelmassa tavoitteena on kerrotaan, miten voit integroida @Task Azure Active Directory (Azure AD).  
Paikallisen ympäristön integroinnissa @Task kanssa Azure AD mahdollistaa seuraavat edut: 

- Voit hallita Azure AD, joilla on oikeus@Task
- Voit ottaa automaattisesti saat kirjautunut käyttöön käyttäjien @Task (kertakirjautumisen) niiden Azure AD-tilien kanssa
- Voit hallita asiakkaiden yhdessä keskitetyssä sijainnissa - Azure perinteinen Portal

Jos haluat tietää tarkempia tietoja SaaS sovelluksen integrointi Azure AD-kohdassa [mitä sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Edellytykset 

Voit määrittää Azure AD-integrointi @Task, tarvitset seuraavat:

- Azure AD-tilaus
- @Task Käytössä tilauksessa single-merkki


> [AZURE.NOTE] Voit esikatsella vaiheet Tässä opetusohjelmassa, Emme suosittele käyttämällä tuotantoympäristössä.


Voit esikatsella vaiheet Tässä opetusohjelmassa-noudattamalla näitä suosituksia:

- Älä käytä tuotantoympäristössä, ellei se on tarpeen.
- Jos sinulla ei ole kokeiluversion Azure AD-ympäristössä, voit hankkia yksi kuukausi kokeiluversion [tähän](https://azure.microsoft.com/pricing/free-trial/). 

 
## <a name="scenario-description"></a>Skenaarion kuvaus
Tässä opetusohjelmassa tavoitteena on, jotta voit testata Azure AD kertakirjautumisen testiympäristössä.  
Tässä opetusohjelmassa kuvatut skenaarion koostuu kolme tärkeimmät rakenneosien:

1. Lisäämällä @Task valikoimasta 
2. Yksittäinen määrittäminen ja testaus Azure AD-on


## <a name="adding-task-from-the-gallery"></a>Lisäämällä @Task valikoimasta
Voit määrittää integrointi @Task suoraan Azure AD, sinun on lisättävä @Task hallitun SaaS sovellusluettelo-valikoimasta.

**Jos haluat lisätä @Task valikoimasta, toimi seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen portal**, valitse vasemmassa siirtymisruudussa. 

    ![Active Directory][1] 

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset][2] 

4. Valitse sivun alareunassa **Lisää** .

    ![Sovellukset][3] 

5. Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Sovellukset][4] 

6. Kirjoita hakuruutuun **@Task**.

    ![Sovellukset][5] 

7. Valitse tulokset-ruudussa **@Task**, ja valitse sitten **Valmis** , voit lisätä sovelluksen.

    ![Sovellukset][30] 



##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yksittäinen määrittäminen ja testaus Azure AD-on

Tässä osassa tavoitteena on noudattamalla voit määrittää ja testaa Azure AD kertakirjautumisen kanssa @Task nimeltä "Britta Simon" testikäyttäjän perusteella.

Kertakirjautumisen toimimaan, saat Azure AD tarvitsee tietää, mitä vastaavaan käyttäjän @Task Azure AD-käyttäjälle on. Toisin sanoen linkki Azure AD-käyttäjä ja Aiheeseen liittyvät-välisen suhteen @Task on vahvistettava.   
Linkki-yhteys on muodostettu määrittämällä arvo **käyttäjänimi** **käyttäjänimi** arvoksi Azure AD @Task.
 
Määritä ja testaa Azure AD kertakirjautumisen kanssa @Task, sinun on suoritettava seuraavat hakukyselyn:

1. **[Azure AD määrittäminen Single Sign](#configuring-azure-ad-single-single-sign-on)** - käyttäjät voivat käyttää tätä toimintoa.
2. **[Käyttäjän testata luominen Azure AD](#creating-an-azure-ad-test-user)** - Testaa Azure AD kertakirjautumisen Britta Simon kanssa.
4. **[Luomisesta @Tasktest käyttäjän](#creating-a-halogen-software-test-user) ** - on tapahtumista Britta Simon @Taskthat on linkitetty hän Azure AD esitys.
5. **[Käyttäjän testata määrittäminen Azure AD](#assigning-the-azure-ad-test-user)** - Britta Simon käyttämään Azure AD kertakirjautumisen käyttöön.
5. **[Testaus Single Sign](#testing-single-sign-on)** - voit tarkistaa, toimiiko määritykset.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD-Single Sign-On määrittäminen

Tässä osassa on Azure AD kertakirjautumisen Azure perinteinen portaalissa ottaminen käyttöön ja määrittää single Sign-että @Task sovelluksen.

**Voit määrittää Azure AD kertakirjautumisen kanssa @Task, toimimalla seuraavasti:**

1. Azure perinteinen-portaalissa Valitse **@Task** sovelluksen integrointi-sivulla **Määritä kertakirjautumisen** **Määrittäminen Single Sign** -valintaikkunan avaaminen.

    ![Kertakirjautumisen määrittäminen][6] 

2. Valitse **miten Haluatko, että käyttäjät voivat kirjautua sisään @Task ** sivun, valitse **Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Azure AD-Single Sign-On][7] 

3. Valitse **Määritä sovelluksen asetukset** -sivulla toimimalla seuraavasti:

    ![Sovelluksen asetusten määrittäminen][8] 
 
     a. Kirjoita **Merkki-URL** -tekstiruutuun URL-osoite kertakirjauksen käyttöön käyttäjien käyttää omaa @Task sovellus (esimerkiksi:*https://<Tenant name>.attask ondemand.com*).

     b. Valitse **Seuraava**.

4. Valitse **määrittäminen kertakirjautumisen osoitteessa @Task ** sivun, valitse **Lataa metatietoja**, tallentaminen paikallisesti tietokoneen metatiedot-tiedostoon ja valitse sitten **Seuraava**.

    ![Mikä on Azure AD Connect][9] 



1. Kertakirjauksen käyttöön oman @Task yrityksen sivuston järjestelmänvalvojana.

2. Siirry **Kertakirjauksen määritykset**.


1. Valitse **Single Sign** -valintaikkunassa suorittamalla seuraavat vaiheet

    ![Kertakirjautumisen määrittäminen][23]

    a. Valitse **tyyppi**- **SAML 2.0**.

    b. Valitse **palveluntarjoajan tunnus**.

    c-näppäinyhdistelmää. Azure perinteinen-portaalissa kopioi **Remote sisäänkirjautumisen URL-osoite**ja liitä se sitten **Kirjautuminen portaalin URL** -tekstiruutuun.

    d. Azure perinteinen-portaalissa kopioi **Yhden Sign-Out palvelun URL-osoite**ja liitä se sitten **Sign-Out URL** -tekstiruutuun.

    e. Azure perinteinen-portaalissa kopioi **Muuta salasana URL-osoite**ja liitä se sitten **Vaihda salasana URL** -tekstiruutuun.

    f. Valitse **Tallenna**.

6. Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Seuraava**. 

    ![Mikä on Azure AD Connect][10]

7. Valitse **Sign Vahvista** -sivulla **Valmis**.  

    ![Mikä on Azure AD Connect][11]




### <a name="creating-an-azure-ad-test-user"></a>Azure AD-testikäyttäjän luominen
Tässä osassa tavoitteena on luotava testikäyttäjän kutsutaan Britta Simon Azure perinteinen portaalissa.  

![Luo Azure AD-käyttäjä][20]

**Voit luoda testikäyttäjän Azure AD-toimimalla seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen portal**, valitse vasemmassa siirtymisruudussa.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-attask-tutorial/create_aaduser_02.png) 

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Ylä-valikossa käyttäjäluettelon, näkyviin napsauttamalla **käyttäjät**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-attask-tutorial/create_aaduser_03.png) 
 
4. Avaa **Lisää käyttäjä** -valintaikkunan alareunassa työkalurivillä, valitse **Lisää käyttäjä**. 

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-attask-tutorial/create_aaduser_04.png) 

5. Tee valintaikkunassa **Kerro kyseisen käyttäjän tietoja** -sivulla seuraavasti: 

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-attask-tutorial/create_aaduser_05.png) 

    a. Käyttäjänä tyyppi Valitse uusi käyttäjä organisaatiossa.

    b. Kirjoita käyttäjänimi- **tekstiruutuun** **BrittaSimon**.

    c-näppäinyhdistelmää. Valitse **Seuraava**.

6.  Suorita **Käyttäjäprofiili** -sivulla seuraavat toimet: 

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-attask-tutorial/create_aaduser_06.png) 
 
    a. Kirjoita **Ensimmäinen nimi** -tekstiruutuun **Britta**.  

    b. Kirjoita **Sukunimi** -tekstiruutuun tyyppi- **Simon**.

    c-näppäinyhdistelmää. Kirjoita **Näyttönimi** -tekstiruutuun **Britta Simon**.

    d. Valitse **rooli** -luettelosta **käyttäjä**.
    e. Valitse **Seuraava**.

7. Valitse **Hae tilapäinen salasana** -valintaikkunan sivulla **Luo**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-attask-tutorial/create_aaduser_07.png) 
 
8. **Hae tilapäinen salasana** -valintaikkunan sivulla toimimalla seuraavasti:

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-attask-tutorial/create_aaduser_08.png) 
  
    a. Kirjoita **Uusi salasana**arvo muistiin.

    b. Valitse **Valmis**.   

  
 
### <a name="creating-an-task-test-user"></a>Luominen @Task testikäyttäjän

Tässä osassa on Luo eli Britta Simon käyttäjä @Task.


**Luo käyttäjä kutsutaan Britta Simon @Task, toimimalla seuraavasti:**

1. Kirjaudu sisään oman @Task yrityksen sivuston järjestelmänvalvojana.

2. Valitse ylä-valikossa Valitse **henkilöt**.

3. Valitse **uuden henkilön**. 

4. Valitse uusi henkilö-valintaikkunassa seuraavasti:

    ![Luo @Task testikäyttäjän][21] 

    a. Kirjoita **Ensimmäinen nimi** -tekstiruutuun "Britta".

    b. Kirjoita "Simon" **Sukunimi** -tekstiruutuun.

    c-näppäinyhdistelmää. Kirjoita **Sähköpostiosoite** -tekstiruutuun Azure Active Directoryn Britta Simonin sähköpostiosoite.

    d. Valitse **Lisää henkilö**.




### <a name="assigning-the-azure-ad-test-user"></a>Azure AD-testikäyttäjän määrittäminen

Tässä osassa tavoitteena on ottaminen käyttöön Britta Simon käyttämään Azure kertakirjautumisen hänen käyttöoikeuksien myöntäminen @Task.

![Määrittää käyttäjälle][200] 

**Määritä Britta Simon, @Task, toimimalla seuraavasti:**

1. Azure perinteinen-portaalissa Avaa sovellukset-näkymässä kansio-näkymässä, valitse **sovellukset** yläreunan-valikossa.

    ![Määrittää käyttäjälle][201] 

2. Valitse sovellukset-luettelosta **@Task**.

    ![Määrittää käyttäjälle][202] 

1. Valitse ylä-valikossa Valitse **käyttäjiä**.

    ![Määrittää käyttäjälle][203] 

1. Valitse käyttäjät-luettelosta **Britta Simon**.

2. Napsauta työkalurivin alaosassa **määrittäminen**.

    ![Määrittää käyttäjälle][205]



### <a name="testing-single-sign-on"></a>Kertakirjautumisen testaaminen

Tässä osassa tavoitteena on Testaa Azure AD yksittäisen Sign-asetusten käyttäminen Access-paneeli.  
Kun napsautat @Task vierekkäin Access-ruudussa pitäisi saada automaattisesti kirjautunut sisään, että @Task sovelluksen.


## <a name="additional-resources"></a>Lisäresursseja

* [Luettelo siitä, miten voit integroida SaaS sovellusten Azure Active Directory-opetusohjelmat](active-directory-saas-tutorial-list.md)
* [Mikä on sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-attask-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-attask-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-attask-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-attask-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_01.png
[30]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_02.png


[6]: ./media/active-directory-saas-attask-tutorial/tutorial_general_05.png
[7]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_03.png
[8]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_04.png
[9]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_05.png
[10]: ./media/active-directory-saas-attask-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-attask-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-attask-tutorial/tutorial_general_100.png
[21]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_08.png


[23]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_06.png

[200]: ./media/active-directory-saas-attask-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-attask-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-attask-tutorial/tutorial_attask_09.png
[203]: ./media/active-directory-saas-attask-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-attask-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-attask-tutorial/tutorial_general_205.png






