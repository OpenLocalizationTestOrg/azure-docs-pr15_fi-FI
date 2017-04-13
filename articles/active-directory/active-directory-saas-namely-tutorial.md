<properties
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi eli | Microsoft Azure"
    description="Opettele määrittämään kertakirjautumisen Azure Active Directoryn välillä eli."
    services="active-directory"
    documentationCenter=""
    authors="jeevansd"
    manager="prasannas"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-namely"></a>Opetusohjelma: Azure Active Directory-integrointi eli

Tässä opetusohjelmassa tavoitteena on, kuinka voit integroida eli Azure Active Directory (Azure AD).

Azure AD eli integraation avulla voit seuraavat edut: 

- Voit hallita Azure AD, joilla on oikeus eli 
- Voit ottaa automaattisesti saat kirjautunut sisään, eli käyttäjien (kertakirjautumisen) niiden Azure AD-tilien kanssa
- Voit hallita asiakkaiden yhdessä keskitetyssä sijainnissa - Azure perinteinen portal

Jos haluat tietää tarkempia tietoja SaaS sovelluksen integrointi Azure AD-kohdassa [mitä sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Edellytykset 

Määritä Azure AD integrointi, voit edellyttää seuraavia:

- Azure AD-tilaus
- Eli single-merkki käytössä-tilauksessa


> [AZURE.NOTE] Voit esikatsella vaiheet Tässä opetusohjelmassa, Emme suosittele käyttämällä tuotantoympäristössä.


Voit esikatsella vaiheet Tässä opetusohjelmassa-noudattamalla näitä suosituksia:

- Älä käytä tuotantoympäristössä, ellei se on tarpeen.
- Jos sinulla ei ole kokeiluversion Azure AD-ympäristössä, voit hankkia yksi kuukausi kokeiluversion [tähän](https://azure.microsoft.com/pricing/free-trial/). 

 
## <a name="scenario-description"></a>Skenaarion kuvaus
Tässä opetusohjelmassa tavoitteena on, jotta voit testata Azure AD kertakirjautumisen testiympäristössä. 

Tässä opetusohjelmassa kuvatut skenaarion kuuluu kaksi tärkeimmät rakenneosien:

1. Eli lisääminen valikoimasta 
2. Yksittäinen määrittäminen ja testaus Azure AD-on


## <a name="adding-namely-from-the-gallery"></a>Eli lisääminen valikoimasta
Voit määrittää integrointi eli suoraan Azure AD-haluat lisätä eli valikoiman hallitun SaaS sovellusluettelo.

**Jos haluat lisätä eli valikoimasta, toimi seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen portal**, valitse vasemmassa siirtymisruudussa. 

    ![Active Directory][1]

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset][2]

4. Valitse sivun alareunassa **Lisää** .

    ![Sovellukset][3]

5. Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Sovellukset][4]

6. Kirjoita hakuruutuun **eli**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-namely-tutorial/tutorial_namely_01.png)

7. Valitse tulosruudussa **eli**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-namely-tutorial/tutorial_namely_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yksittäinen määrittäminen ja testaus Azure AD-on
Tässä osassa tavoitteena on noudattamalla voit määrittää ja testata Azure AD kertakirjautumisen kanssa nimeltä "Britta Simon" testikäyttäjän eli perusteella.

Kertakirjautumisen toimimaan Azure AD on tiedettävä vastaavaan käyttäjän eli Azure AD käyttäjä. Toisin sanoen Azure AD-käyttäjä ja Aiheeseen liittyvät-linkki suhteen eli on vahvistettava.

Linkki suhde laaditaan määrittäminen **käyttäjänimi** arvo eli Azure AD **käyttäjänimi** arvoksi.
 
Määritä ja testaa Azure AD kertakirjautumisen kanssa, sinun on tehtävä seuraavat hakukyselyn:

1. **[Azure AD määrittäminen Single Sign](#configuring-azure-ad-single-single-sign-on)** - käyttäjät voivat käyttää tätä toimintoa.
2. **[Käyttäjän testata luominen Azure AD](#creating-an-azure-ad-test-user)** - Testaa Azure AD kertakirjautumisen Britta Simon kanssa.
4. **[Luominen Testaa eli käyttäjän](#creating-a-namely-test-user)** - on tapahtumista Britta Simon-eli, jotka on linkitetty hän Azure AD esitys.
5. **[Käyttäjän testata määrittäminen Azure AD](#assigning-the-azure-ad-test-user)** - Britta Simon käyttämään Azure AD kertakirjautumisen käyttöön.
5. **[Testaus Single Sign](#testing-single-sign-on)** - voit tarkistaa, toimiiko määritykset.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD-Single Sign-On määrittäminen

Tässä osassa tavoitteena on Azure AD kertakirjautumisen Azure perinteinen portaalissa käyttöön ja määrittäminen kertakirjautumisen eli sovellus. 




**Voit määrittää Azure AD kertakirjautumisen kanssa eli toimimalla seuraavasti:**

1. Valitse Azure perinteinen portal-sivulla **eli** sovelluksen integrointi, **Määritä kertakirjautumisen** **Määrittäminen Single Sign** -valintaikkunan avaaminen.

    ![Kertakirjautumisen määrittäminen][6] 

2. **Miten haluat kirjautua sisään eli käyttäjät** -sivulla Valitse **Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.
 
    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-namely-tutorial/tutorial_namely_03.png) 

3. Valitse **Määritä sovelluksen asetukset** -sivulla toimimalla seuraavasti:.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-namely-tutorial/tutorial_namely_04.png) 

    a. Kirjoita **Merkki-URL** -tekstiruutuun käyttävät käyttäjät Kirjaudu eli sovelluksen URL-osoite (esimerkiksi: *https://fabrikam.Namely.com/*).

    b. Valitse **Seuraava**.
 
 
4. Tee **kertakirjautumisen, eli määrittäminen** -sivulla seuraavasti:

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-namely-tutorial/tutorial_namely_05.png) 

    a. Valitse **Lataa**ja tallenna sitten tiedosto tietokoneesta.

    b. Valitse **Seuraava**.


1. Toisessa selainikkunassa, kirjaudu sisään oman eli yrityksen sivuston järjestelmänvalvojan oikeuksilla.

1. Valitse sivun työkalurivillä **yrityksen**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-namely-tutorial/tutorial_namely_06.png) 

1. Valitse **asetukset** -välilehti.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-namely-tutorial/tutorial_namely_07.png) 


1. Valitse **SAML**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-namely-tutorial/tutorial_namely_08.png) 


1. Suorita **SAML asetukset** -sivun seuraavasti:

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-namely-tutorial/tutorial_namely_09.png) 

    a. Valitse **Ota käyttöön SAML**. 

    b. Azure perinteinen portaalissa sivun valintaikkunan **määrittäminen kertakirjautumisen, eli** **Sign-palvelun URL** -arvon kopioi ja liitä se sitten **tunnistetietojen toimittaja DDO url** -tekstiruutuun. 

    c-näppäinyhdistelmää. Avaa ladatut varmenteen Muistiossa, sisällön kopioiminen ja liittäminen **tunnistetietojen toimittaja varmenteen** tekstiruutu.    

    d. Valitse **Tallenna**.


6. Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Seuraava**. 

    ![Azure AD-Single Sign-On][10]

7. Valitse **Sign Vahvista** -sivulla **Valmis**.  

    ![Azure AD-Single Sign-On][11]




### <a name="creating-an-azure-ad-test-user"></a>Azure AD-testikäyttäjän luominen
Tässä osassa tavoitteena on luotava testikäyttäjän kutsutaan Britta Simon Azure perinteinen portaalissa.

![Luo Azure AD-käyttäjä][20]


**Voit luoda testikäyttäjän Azure AD-toimimalla seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen portal**, valitse vasemmassa siirtymisruudussa.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-namely-tutorial/create_aaduser_09.png)  

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Ylä-valikossa käyttäjäluettelon, näkyviin napsauttamalla **käyttäjät**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-namely-tutorial/create_aaduser_03.png) 
 
4. Avaa **Lisää käyttäjä** -valintaikkunan alareunassa työkalurivillä, valitse **Lisää käyttäjä**. 

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-namely-tutorial/create_aaduser_04.png) 

5. Tee valintaikkunassa **Kerro kyseisen käyttäjän tietoja** -sivulla seuraavasti: 

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-namely-tutorial/create_aaduser_05.png)  

    a. Käyttäjänä tyyppi Valitse uusi käyttäjä organisaatiossa.

    b. Kirjoita käyttäjänimi- **tekstiruutuun** **BrittaSimon**.

    c-näppäinyhdistelmää. Valitse **Seuraava**.

6.  Suorita **Käyttäjäprofiili** -sivulla seuraavat toimet: 

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-namely-tutorial/create_aaduser_06.png) 
 
    a. Kirjoita **Ensimmäinen nimi** -tekstiruutuun **Britta**.  

    b. Kirjoita **Sukunimi** -tekstiruutuun tyyppi- **Simon**.

    c-näppäinyhdistelmää. Kirjoita **Näyttönimi** -tekstiruutuun **Britta Simon**.

    d. Valitse **rooli** -luettelosta **käyttäjä**.
    e. Valitse **Seuraava**.

7. Valitse **Hae tilapäinen salasana** -valintaikkunan sivulla **Luo**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-namely-tutorial/create_aaduser_07.png) 
 
8. **Hae tilapäinen salasana** -valintaikkunan sivulla toimimalla seuraavasti:

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-namely-tutorial/create_aaduser_08.png) 
  
    a. Kirjoita **Uusi salasana**arvo muistiin.

    b. Valitse **Valmis**.   

  
 
### <a name="creating-a-namely-test-user"></a>Luominen Testaa eli käyttäjä

Tässä osassa tavoitteena on eli Britta Simon eli käyttäjän luominen.

**Luo eli Britta Simon eli käyttäjä, toimi seuraavasti:**

1. Kertakirjauksen käyttöön oman eli yrityksen sivuston järjestelmänvalvojan oikeuksilla.

1. Valitse **henkilöt**-sivun työkalurivillä.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-namely-tutorial/tutorial_namely_10.png) 

1. Valitse **Hakemisto** -välilehti.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-namely-tutorial/tutorial_namely_11.png) 

1. Valitse **Lisää uusi henkilö**.



1. Valitse **Lisää uusi henkilö** -valintaikkunassa seuraavasti:

    a. Kirjoita **Ensimmäinen nimi** -tekstiruutuun **Britta**.

    b. Kirjoita **Simon** **Sukunimi** -tekstiruutuun.

    c-näppäinyhdistelmää. Kirjoita **sähköpostiosoite** -tekstiruutuun Azure perinteinen portaalissa Britta henkilön sähköpostiosoite.

    d. Valitse **Tallenna**.





### <a name="assigning-the-azure-ad-test-user"></a>Azure AD-testikäyttäjän määrittäminen

Tässä osassa tavoitteena on Britta Simon käyttämään Azure kertakirjautumisen hänen käyttöoikeuksien myöntäminen eli ottaminen käyttöön.

![Määrittää käyttäjälle][200] 

**Jos haluat määrittää Britta Simon avulla, toimi seuraavasti:**

1. Azure perinteinen-portaalissa Avaa sovellukset-näkymässä kansio-näkymässä, valitse **sovellukset** yläreunan-valikossa.

    ![Määrittää käyttäjälle][201] 

2. Valitse sovellukset-luettelosta **eli**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-namely-tutorial/tutorial_namely_50.png) 

1. Valitse ylä-valikossa Valitse **käyttäjiä**.

    ![Määrittää käyttäjälle][203] 

1. Valitse käyttäjät-luettelosta **Britta Simon**.

2. Napsauta työkalurivin alaosassa **määrittäminen**.

    ![Määrittää käyttäjälle][205]



### <a name="testing-single-sign-on"></a>Kertakirjautumisen testaaminen

Tässä osassa tavoitteena on Testaa Azure AD yksittäisen Sign-asetusten käyttäminen Access-paneeli.

Kun napsautat ruutu on korostettuna eli Access-ruudussa, sinun kannattaa Hae automaattisesti kirjautunut käyttöön eli sovelluksen.


## <a name="additional-resources"></a>Lisäresursseja

* [Luettelo siitä, miten voit integroida SaaS sovellusten Azure Active Directory-opetusohjelmat](active-directory-saas-tutorial-list.md)
* [Mikä on sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-namely-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-namely-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-namely-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-namely-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-namely-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-namely-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-namely-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-namely-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-namely-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-namely-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-namely-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-namely-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-namely-tutorial/tutorial_general_205.png






