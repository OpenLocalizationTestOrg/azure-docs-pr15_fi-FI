<properties
    pageTitle="Opetusohjelma: 23 Video Azure Active Directory-integrointi | Microsoft Azure"
    description="Opettele määrittämään kertakirjautumisen Azure Active Directory- ja 23 Video välillä."
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
    ms.date="10/24/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-23-video"></a>Opetusohjelma: 23 Video Azure Active Directory-integrointi

Tässä opetusohjelmassa tavoitteena on, kuinka voit integroida 23 Video Azure Active Directory (Azure AD).  
Paikallisen ympäristön integroinnissa 23 Azure AD videon löytyy seuraavat edut: 

- Voit hallita Azure AD, kenellä on oikeus 23 Video 
- Voit ottaa käyttöön käyttäjille automaattisesti Hae kirjautunut sisään 23 (kertakirjautumisen) videoon niiden Azure AD-tilien kanssa

Jos haluat tietää tarkempia tietoja SaaS sovelluksen integrointi Azure AD-kohdassa [mitä sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Edellytykset 

Azure AD-integroinnin määrittäminen 23 video, tarvitset seuraavat:

- Azure AD-tilaus
- 23 Video single-merkki käytössä-tilauksessa


> [AZURE.NOTE] Voit esikatsella vaiheet Tässä opetusohjelmassa, Emme suosittele käyttämällä tuotantoympäristössä.


Voit esikatsella vaiheet Tässä opetusohjelmassa-noudattamalla näitä suosituksia:

- Älä käytä tuotantoympäristössä, ellei se on tarpeen.
- Jos sinulla ei ole kokeiluversion Azure AD-ympäristössä, voit hankkia yksi kuukausi kokeiluversion [tähän](https://azure.microsoft.com/pricing/free-trial/). 

 
## <a name="scenario-description"></a>Skenaarion kuvaus
Tässä opetusohjelmassa tavoitteena on, jotta voit testata Azure AD kertakirjautumisen testiympäristössä.  
Tässä opetusohjelmassa kuvatut skenaarion kuuluu kaksi tärkeimmät rakenneosien:

1. 23 videon lisääminen valikoimasta 
2. Yksittäinen määrittäminen ja testaus Azure AD-on


## <a name="adding-23-video-from-the-gallery"></a>23 videon lisääminen valikoimasta
Voit määrittää 23 Video integroida Azure AD-haluat 23 videon lisääminen valikoimasta hallitun SaaS sovellusluettelo.

**Lisää 23 Video valikoimasta toimimalla seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen portal**, valitse vasemmassa siirtymisruudussa. 

    ![Active Directory][1]

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset][2]

4. Valitse sivun alareunassa **Lisää** .

    ![Sovellukset][3]

5. Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Sovellukset][4]

6. Kirjoita hakuruutuun **23 Video**.

    ![Sovellukset][5]

7. Valitse tulokset-ruudun Valitse **23 Video**ja valitse sitten **Valmis** , voit lisätä sovelluksen.

    ![Sovellukset][25]

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yksittäinen määrittäminen ja testaus Azure AD-on
Tässä osassa tavoitteena on noudattamalla voit määrittää ja testaa Azure AD kertakirjautumisen nimeltä "Britta Simon" testi käyttäjään perustuva 23 Video.

Kertakirjautumisen toimimaan Azure AD on tietää, mitkä asiat vastaavaan käyttäjän 23 videossa Azure AD-käyttäjälle. Toisin sanoen linkki suhteen Azure AD-käyttäjä ja Aiheeseen liittyvät 23 Video on vahvistettava.  
Linkki-yhteys on muodostettu määrittämällä **käyttäjänimi** arvo arvoksi **käyttäjänimi** 23 video Azure AD.
 
Määrittäminen ja testaaminen Azure AD kertakirjautumisen 23 video-sinun on tehtävä seuraavat hakukyselyn:

1. **[Azure AD määrittäminen Single Sign](#configuring-azure-ad-single-single-sign-on)** - käyttäjät voivat käyttää tätä toimintoa.
2. **[Käyttäjän testata luominen Azure AD](#creating-an-azure-ad-test-user)** - Testaa Azure AD kertakirjautumisen Britta Simon kanssa.
4. **[Käyttäjän luominen 23 videon testata](#creating-a-23-video-test-user)** - tapahtumista Britta Simon ovat 23 Video, joka on linkitetty hän Azure AD-esittäminen.
5. **[Käyttäjän testata määrittäminen Azure AD](#assigning-the-azure-ad-test-user)** - Britta Simon käyttämään Azure AD kertakirjautumisen käyttöön.
5. **[Testaus Single Sign](#testing-single-sign-on)** - voit tarkistaa, toimiiko määritykset.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD-Single Sign-On määrittäminen

Tässä osassa tavoitteena on Azure AD kertakirjautumisen Azure perinteinen portaalissa käyttöön ja määrittää kertakirjautumisen 23 Video-sovelluksessa.

**Voit määrittää Azure AD kertakirjautumisen 23 video toimimalla seuraavasti:**

1. Valitse Azure perinteinen portaalissa integrointi **23 Video** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen Single Sign** -valintaikkunan avaaminen.

    ![Kertakirjautumisen määrittäminen][6] 

2. **Miten Haluatko, että käyttäjät voivat kirjautua sisään 23 Video** -sivulla Valitse **Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Azure AD-Single Sign-On][7] 

3. Valitse **Määritä sovelluksen asetukset** -sivulla toimimalla seuraavasti:

    ![Azure AD-Single Sign-On][8] 
 
     a. Kirjoita **Vastaus URL** -tekstiruutuun URL-osoite käyttäjien Sign 23 Video sivustoon (esimerkiksi: *https://britta-simon.23Video.com/saml/login*).

     > [AZURE.NOTE] Active Directory-integrointi, käyttämällä SAML 2.0 on kaikkien 23 Video-käyttäjien käytettävissä. Ota yhteyttä tukeen osoitteessa [support@23company.com](mailto:support@23company.com) Jos tarvitset Aiheeseen liittyviä metatietoja.

     b. Valitse **Seuraava**.
 
4. Valitse **Määritä kertakirjautumisen 23 videota** -sivulla toimimalla seuraavasti:

    ![Azure AD-Single Sign-On][9] 

    a. Valitse Lataa sertifikaatti ja tallenna sitten tiedosto tietokoneesta.

    b. 23 Video-tukeen kautta [support@23company.com](mailto:support@23company.com), antaa niille ladatut varmenteen **Myöntäjä URL-osoite**, **Sign-palvelun URL**, **Yksi Sign-Out URL-osoite**ja pyytää heitä asetukset SSO 23 Video-sovellusta. 

    c-näppäinyhdistelmää. Valitse **Seuraava**.


6. Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Seuraava**. 

    ![Azure AD-Single Sign-On][10]

7. Valitse **Sign Vahvista** -sivulla **Valmis**.  
  
    ![Azure AD-Single Sign-On][11]




### <a name="creating-an-azure-ad-test-user"></a>Azure AD-testikäyttäjän luominen
Tässä osassa tavoitteena on luotava testikäyttäjän kutsutaan Britta Simon Azure perinteinen portaalissa.

![Luo Azure AD-käyttäjä][20]

**Voit luoda testikäyttäjän Azure AD-toimimalla seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen portal**, valitse vasemmassa siirtymisruudussa.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-23video-tutorial/create_aaduser_09.png)  

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Ylä-valikossa käyttäjäluettelon, näkyviin napsauttamalla **käyttäjät**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-23video-tutorial/create_aaduser_03.png) 
 
4. Avaa **Lisää käyttäjä** -valintaikkunan alareunassa työkalurivillä, valitse **Lisää käyttäjä**. 

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-23video-tutorial/create_aaduser_04.png) 

5. Tee valintaikkunassa **Kerro kyseisen käyttäjän tietoja** -sivulla seuraavasti: 

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-23video-tutorial/create_aaduser_05.png)  

    a. Käyttäjänä tyyppi Valitse uusi käyttäjä organisaatiossa.

    b. Kirjoita käyttäjänimi- **tekstiruutuun** **BrittaSimon**.

    c-näppäinyhdistelmää. Valitse **Seuraava**.

6.  Suorita **Käyttäjäprofiili** -sivulla seuraavat toimet: 

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-23video-tutorial/create_aaduser_06.png) 
 
    a. Kirjoita **Ensimmäinen nimi** -tekstiruutuun **Britta**.  

    b. Kirjoita **Sukunimi** -tekstiruutuun tyyppi- **Simon**.

    c-näppäinyhdistelmää. Kirjoita **Näyttönimi** -tekstiruutuun **Britta Simon**.

    d. Valitse **rooli** -luettelosta **käyttäjä**.
    e. Valitse **Seuraava**.

7. Valitse **Hae tilapäinen salasana** -valintaikkunan sivulla **Luo**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-23video-tutorial/create_aaduser_07.png) 
 
8. **Hae tilapäinen salasana** -valintaikkunan sivulla toimimalla seuraavasti:

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-23video-tutorial/create_aaduser_08.png) 
  
    a. Kirjoita **Uusi salasana**arvo muistiin.

    b. Valitse **Valmis**.   

  
 
### <a name="creating-a-23-video-test-user"></a>23 Video-testikäyttäjän luominen

Tässä osassa tavoitteena on nimeltään Britta Simon 23 Video käyttäjän luominen.

**Luo kutsutaan Britta Simon 23 Video käyttäjä, toimi seuraavasti:**

1. Kirjaudu 23 Video yrityksesi sivuston järjestelmänvalvojana.

1. Valitse **asetukset**.


2. Valitse **käyttäjät** -osiossa **Configure**.

    ![Määrittää käyttäjälle][400]

3. Valitse **Lisää uusi käyttäjä**. 

    ![Määrittää käyttäjälle][401]

4. **Henkilön kutsuminen liity tähän sivustoon** -osan toimimalla seuraavasti:

    ![Määrittää käyttäjälle][402]

    a. Kirjoita **sähköpostiosoitteet** -tekstiruutuun Azure AD Britta Simonin sähköpostiosoite.

    b. Valitse **Lisää käyttäjä**.   


### <a name="assigning-the-azure-ad-test-user"></a>Azure AD-testikäyttäjän määrittäminen

Tässä osassa tavoitteena on Britta Simon käyttämään Azure kertakirjautumisen hänen käyttöoikeuksien myöntämistä 23 videon ottaminen käyttöön.

![Määrittää käyttäjälle][200] 

**Jos haluat määrittää Britta Simon 23 Video, toimi seuraavasti:**

1. Azure perinteinen-portaalissa Avaa sovellukset-näkymässä kansio-näkymässä, valitse **sovellukset** yläreunan-valikossa.

    ![Määrittää käyttäjälle][201] 

2. Valitse sovellukset-luettelosta **23 Video**.

    ![Määrittää käyttäjälle][202] 

1. Valitse ylä-valikossa Valitse **käyttäjiä**.

    ![Määrittää käyttäjälle][203] 

1. Valitse käyttäjät-luettelosta **Britta Simon**.

2. Napsauta työkalurivin alaosassa **määrittäminen**.

    ![Määrittää käyttäjälle][205]



### <a name="testing-single-sign-on"></a>Kertakirjautumisen testaaminen

Tässä osassa tavoitteena on Testaa Azure AD yksittäisen Sign-asetusten käyttäminen Access-paneeli.  
Kun napsautat Access-paneelin 23 Video-ruutu, sinun olisi Hae automaattisesti kirjautunut käyttöön 23 Video-sovelluksen.


## <a name="additional-resources"></a>Lisäresursseja

* [Luettelo siitä, miten voit integroida SaaS sovellusten Azure Active Directory-opetusohjelmat](active-directory-saas-tutorial-list.md)
* [Mikä on sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta?](active-directory-appssoaccess-whatis.md)


<!--Image references-->
[1]: ./media/active-directory-saas-23video-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-23video-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-23video-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-23video-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_01.png
[25]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_02.png

[6]: ./media/active-directory-saas-23video-tutorial/tutorial_general_05.png
[7]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_03.png
[8]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_04.png
[9]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_05.png
[10]: ./media/active-directory-saas-23video-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-23video-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-23video-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-23video-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-23video-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_07.png
[203]: ./media/active-directory-saas-23video-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-23video-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-23video-tutorial/tutorial_general_205.png

[400]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_10.png
[401]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_11.png
[402]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_12.png




