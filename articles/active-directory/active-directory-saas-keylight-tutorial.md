<properties
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi Keylight | Microsoft Azure"
    description="Opettele määrittämään kertakirjautumisen Azure Active Directory- ja Keylight välillä."
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


# <a name="tutorial-azure-active-directory-integration-with-keylight"></a>Opetusohjelma: Keylight Azure Active Directory-integrointi

Tässä opetusohjelmassa kerrotaan, miten voit integroida Keylight Azure Active Directory (Azure AD).

Azure AD-integraation Keylight avulla voit seuraavat edut:

- Voit hallita Azure AD, jolla on pääsy Keylight
- Voit ottaa käyttöön automaattisesti Hae kirjautunut käyttöön Keylight (kertakirjautumisen) niiden Azure AD-tilien käyttäjille
- Voit hallita asiakkaiden yhdessä keskitetyssä sijainnissa - Azure perinteinen portal

Jos haluat tietää tarkempia tietoja SaaS sovelluksen integrointi Azure AD-kohdassa [mitä sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Edellytykset

Määrittää Azure AD-integroinnin Keylight, tarvitset seuraavat:

- Azure tilauksen
- Keylight single-merkki käytössä-tilauksessa


> [AZURE.NOTE] Voit esikatsella vaiheet Tässä opetusohjelmassa, Emme suosittele käyttämällä tuotantoympäristössä.


Voit esikatsella vaiheet Tässä opetusohjelmassa-noudattamalla näitä suosituksia:

- Älä käytä tuotantoympäristössä, ellei se on tarpeen.
- Jos sinulla ei ole kokeiluversion Azure AD-ympäristössä, voit hankkia yksi kuukausi kokeiluversion [tähän](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Skenaarion kuvaus
Tässä opetusohjelmassa testaat Azure AD kertakirjautumisen testiympäristössä. 

Tässä opetusohjelmassa kuvatut skenaarion kuuluu kaksi tärkeimmät rakenneosien:

1. Keylight lisääminen valikoimasta
2. Yksittäinen määrittäminen ja testaus Azure AD-on


## <a name="adding-keylight-from-the-gallery"></a>Keylight lisääminen valikoimasta
Voit määrittää Keylight integroida Azure AD-haluat lisätä Keylight valikoimasta hallitun SaaS sovellusluettelo.

**Jos haluat lisätä Keylight valikoimasta, toimi seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen portal**, valitse vasemmassa siirtymisruudussa. 

    ![Active Directory][1]

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset][2]

4. Valitse sivun alareunassa **Lisää** .

    ![Sovellukset][3]

5. Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Sovellukset][4]

6. Kirjoita hakuruutuun **Keylight**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_01.png)

7. Valitse tulosruudussa **Keylight**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yksittäinen määrittäminen ja testaus Azure AD-on
Tässä osassa Määritä ja testaa Azure AD kertakirjautumisen Keylight nimeltä "Britta Simon" testikäyttäjän perusteella.

Määrittäminen ja testaaminen Azure AD kertakirjautumisen Keylight kanssa, sinun on tehtävä seuraavat hakukyselyn:

1. **[Azure AD määrittäminen Single Sign](#configuring-azure-ad-single-single-sign-on)** - käyttäjät voivat käyttää tätä toimintoa.
2. **[Käyttäjän testata luominen Azure AD](#creating-an-azure-ad-test-user)** - Testaa Azure AD kertakirjautumisen Britta Simon kanssa.
4. **[Luomisesta Keylight testata käyttäjän](#creating-a-keylight-test-user)** - tapahtumista Britta Simon ovat Keylight, jotka on linkitetty hän Azure AD esitys.
5. **[Käyttäjän testata määrittäminen Azure AD](#assigning-the-azure-ad-test-user)** - Britta Simon käyttämään Azure AD kertakirjautumisen käyttöön.
5. **[Testaus Single Sign](#testing-single-sign-on)** - voit tarkistaa, toimiiko määritykset.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD-Single Sign-On määrittäminen

Tässä osassa Azure AD kertakirjautumisen Azure perinteinen portaalissa ottaminen käyttöön ja määritä kertakirjautumisen Keylight-sovelluksessa.


**Määrittää Azure AD kertakirjautumisen Keylight, toimimalla seuraavasti:**

1. Valitse Azure perinteinen portaalissa integrointi **Keylight** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen Single Sign** -valintaikkunan avaaminen.

    ![Kertakirjautumisen määrittäminen][6] 


2. **Miten käyttäjät voivat kirjautua Keylight haluat käyttää** sivulla Valitse **Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_03.png) 

3. Valitse **Määritä sovelluksen asetukset** -sivulla toimimalla seuraavasti:
 
    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_04.png) 


    a. Kirjoita merkki-URL-tekstiruutuun URL-osoite käyttämä Sign käyttämällä seuraavaa kaavaa Keylight sovelluksen käyttäjille: **"https://\<yrityksen nimi\>.keylightgrc.com/Login.aspx?saml=1"**.


4. Valitse **Määritä kertakirjautumisen osoitteessa Keylight** -sivulla toimimalla seuraavasti:
 
    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_05.png) 

    a. Valitse **Lataa**ja tallenna sitten tiedosto tietokoneesta.

    b. Valitse **Seuraava**.


5. Jotta SSO Keylight, toimimalla seuraavasti:
 
    a. Sign-tiliisi Keylight järjestelmänvalvojana.

    b. Valitse ylä-valikossa **henkilö**ja **Keylight**asetukset.
       
    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-keylight-tutorial/401.png) 

    c-näppäinyhdistelmää. Valitse vasemman reunan treeview **SAML**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-keylight-tutorial/402.png) 

    d. **SAML-asetukset** -valintaikkunassa valitse **Muokkaa**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-keylight-tutorial/404.png) 
  

5. Valitse **Muokkaa SAML-asetukset** -valintaikkunan sivun toimimalla seuraavasti:

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-keylight-tutorial/405.png) 

    a. **SAML-todennuksen** määrittäminen **aktiiviseksi**.


    b. Perinteinen Azure AD-portaalissa **SAML SSO URL-osoite** -arvo kopioi ja liitä se sitten **Tunnistetietojen toimittaja sisäänkirjautumisen URL-osoite** -tekstiruutuun.

    c-näppäinyhdistelmää. Perinteinen Azure AD-portaalissa kopioi **Yhden Sign-Out palvelun URL-osoite** -arvo ja liitä se sitten **Tunnistetietojen toimittaja Kirjaudu URL-osoite** -tekstiruutuun.

    d. Valitse **Tiedosto** ja valitse ladatut Keylight varmennetta ja valitse sitten **Avaa** Lataa sertifikaatti.


    e. **SAML käyttäjätunnus sijainnin** määrittäminen **NameIdentifier**elementtiin aihe-lauseen.
   
    f. Anna **Keylight palveluntarjoajan käyttämällä seuraavaa kaavaa: **https://&lt;yrityksen nimi&gt;. keylightgrc.com**.

    g. Määritä **Automaattinen - käyttäjien** **aktiivinen**.

    h. Määritä **Automaattinen säännöstä tilityyppi** **Täydet käyttöoikeudet**.

    voin. **Automaattinen valmistelu suojauksen rooli**Valitse **Peruskäyttäjän SAML kanssa**.
   
    j. Valitse **Automaattinen valmistelu suojauksen config** **Vakio Käyttäjäasetukset**.
   
    k. Kirjoita **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**määrite sähköpostiosoite-tekstiruutuun.

    l. Kirjoita **Etunimi-määrite** -tekstiruutuun **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.

    m. Kirjoita **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname** **viimeisen name-määrite** -tekstiruutuun.

    n. Valitse **Tallenna**.
   
  
   
  
6. Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Seuraava**.

    ![Azure AD-Single Sign-On][10]

7. Valitse **Sign Vahvista** -sivulla **Valmis**.  

    ![Azure AD-Single Sign-On][11]




### <a name="creating-an-azure-ad-test-user"></a>Azure AD-testikäyttäjän luominen
Tässä osassa voit luoda testikäyttäjän kutsutaan Britta Simon Azure perinteinen portaalissa.

Valitse käyttäjät-luettelosta **Britta Simon**.

![Luo Azure AD-käyttäjä][20]



**Voit luoda testikäyttäjän Azure AD-toimimalla seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen Portal**, valitse vasemmassa siirtymisruudussa.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-keylight-tutorial/create_aaduser_09.png) 


2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Ylä-valikossa käyttäjäluettelon, näkyviin napsauttamalla **käyttäjät**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-keylight-tutorial/create_aaduser_03.png) 


4. Avaa **Lisää käyttäjä** -valintaikkunan alareunassa työkalurivillä, valitse **Lisää käyttäjä**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-keylight-tutorial/create_aaduser_04.png) 

5. Tee valintaikkunassa **Kerro kyseisen käyttäjän tietoja** -sivulla seuraavasti:

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-keylight-tutorial/create_aaduser_05.png) 

    a. Käyttäjänä tyyppi Valitse uusi käyttäjä organisaatiossa.

    b. Kirjoita käyttäjänimi- **tekstiruutuun** **BrittaSimon**.

    c-näppäinyhdistelmää. Valitse **Seuraava**.

6.  Suorita **Käyttäjäprofiili** -sivulla seuraavat toimet:

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-keylight-tutorial/create_aaduser_06.png) 

    a. Kirjoita **Ensimmäinen nimi** -tekstiruutuun **Britta**.  

    b. Kirjoita **Sukunimi** -tekstiruutuun tyyppi- **Simon**.

    c-näppäinyhdistelmää. Kirjoita **Näyttönimi** -tekstiruutuun **Britta Simon**.

    d. Valitse **rooli** -luettelosta **käyttäjä**.

    e. Valitse **Seuraava**.

7. Valitse **Hae tilapäinen salasana** -valintaikkunan sivulla **Luo**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-keylight-tutorial/create_aaduser_07.png) 

8. **Hae tilapäinen salasana** -valintaikkunan sivulla toimimalla seuraavasti:

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-keylight-tutorial/create_aaduser_08.png) 

    a. Kirjoita **Uusi salasana**arvo muistiin.

    b. Valitse **Valmis**.   



### <a name="creating-a-keylight-test-user"></a>Keylight testikäyttäjän luominen

Tässä osassa nimeltä Britta Simon Keylight käyttäjän luominen. Keylight tukee vain perustuvan valmistelu, jossa on käytössä oletusarvoisesti.

Ei ole toimi puolestasi sisältö. Uusi käyttäjä luodaan, kun käytetään Keylight, jos käyttäjä ei ole vielä. 

> [AZURE.NOTE] Jos haluat luoda käyttäjän manuaalisesti, on Keylight-tukeen.


### <a name="assigning-the-azure-ad-test-user"></a>Azure AD-testikäyttäjän määrittäminen

Tässä osassa voit ottaa käyttöön Britta Simon käyttämään Azure kertakirjautumisen hänen käyttöoikeuksien myöntämistä Keylight.

![Määrittää käyttäjälle][200] 

**Jos haluat määrittää Britta Simon Keylight, toimi seuraavasti:**

1. Azure perinteinen-portaalissa Avaa sovellukset-näkymässä kansio-näkymässä, valitse **sovellukset** yläreunan-valikossa.

    ![Määrittää käyttäjälle][201] 

2. Valitse sovellukset-luettelosta **Keylight**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-keylight-tutorial/tutorial_keylight_50.png) 

1. Valitse ylä-valikossa Valitse **käyttäjiä**.

    ![Määrittää käyttäjälle][203] 

1. Valitse käyttäjät-luettelosta **Britta Simon**.

2. Napsauta työkalurivin alaosassa **määrittäminen**.

    ![Määrittää käyttäjälle][205]



### <a name="testing-single-sign-on"></a>Kertakirjautumisen testaaminen

Tässä osassa voit testata Azure AD yksittäisen Sign-asetusten käyttäminen Access-paneeli.

Access-ruudussa Keylight-ruutua napsauttaessasi sinun pitäisi saada automaattisesti kirjautunut-on Keylight sovelluksen.


## <a name="additional-resources"></a>Lisäresursseja

* [Luettelo siitä, miten voit integroida SaaS sovellusten Azure Active Directory-opetusohjelmat](active-directory-saas-tutorial-list.md)
* [Mikä on sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-keylight-tutorial/tutorial_general_205.png
