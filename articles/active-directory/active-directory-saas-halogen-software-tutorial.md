<properties
    pageTitle="Opetusohjelma: Käytetään ohjelmiston Azure Active Directory-integrointi"
    description="Opettele määrittämään kertakirjautumisen Azure Active Directory ja käytetään ohjelmiston välillä."
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


# <a name="tutorial-azure-active-directory-integration-with-halogen-software"></a>Opetusohjelma: Käytetään ohjelmiston Azure Active Directory-integrointi

Tässä opetusohjelmassa tavoitteena on, kuinka voit integroida käytetään ohjelmiston Azure Active Directory (Azure AD).

Azure AD-integraation käytetään ohjelmisto, jonka avulla seuraavat edut: 

- Voit hallita Azure AD, joilla on käytetään ohjelmiston käyttöoikeus 
- Voit ottaa käyttöön automaattisesti Hae kirjautunut sisään käytetään-ohjelmistoa (kertakirjautumisen) käyttäjille heidän Azure AD-tilien kanssa
- Voit hallita asiakkaiden yhdessä keskitetyssä sijainnissa - Azure perinteinen portal

Jos haluat tietää tarkempia tietoja SaaS sovelluksen integrointi Azure AD-kohdassa [mitä sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Edellytykset 

Määrittää Azure AD-integroinnin käytetään ohjelmiston, sinun on seuraavia kohteita:

- Azure AD-tilaus
- Käytetään ohjelmiston single-merkki käytössä-tilauksessa


> [AZURE.NOTE] Voit esikatsella vaiheet Tässä opetusohjelmassa, Emme suosittele käyttämällä tuotantoympäristössä.


Voit esikatsella vaiheet Tässä opetusohjelmassa-noudattamalla näitä suosituksia:

- Älä käytä tuotantoympäristössä, ellei se on tarpeen.
- Jos sinulla ei ole kokeiluversion Azure AD-ympäristössä, voit hankkia yksi kuukausi kokeiluversion [tähän](https://azure.microsoft.com/pricing/free-trial/). 

 
## <a name="scenario-description"></a>Skenaarion kuvaus
Tässä opetusohjelmassa tavoitteena on, jotta voit testata Azure AD kertakirjautumisen testiympäristössä. 

Tässä opetusohjelmassa kuvatut skenaarion kuuluu kaksi tärkeimmät rakenneosien:

1. Käytetään ohjelmiston lisääminen valikoimasta 
2. Yksittäinen määrittäminen ja testaus Azure AD-on


## <a name="adding-halogen-software-from-the-gallery"></a>Käytetään ohjelmiston lisääminen valikoimasta
Voit määrittää käytetään ohjelmiston integroida Azure AD-haluat lisätä käytetään ohjelmiston valikoimasta hallitun SaaS sovellusluettelo.

**Voit lisätä käytetään ohjelmiston valikoimasta, toimi seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen portal**, valitse vasemmassa siirtymisruudussa. 

    ![Active Directory][1]

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset][2]

4. Valitse sivun alareunassa **Lisää** . 

    ![Sovellukset][3]

5. Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Sovellukset][4]

6. Kirjoita hakuruutuun **käytetään ohjelmiston**.

    ![Sovellukset][5]

7. Valitse tulokset-ruudun Valitse **Käytetään ohjelmiston**ja valitse sitten **Valmis** , voit lisätä sovelluksen.



##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yksittäinen määrittäminen ja testaus Azure AD-on
Tässä osassa tavoitteena on noudattamalla voit määrittää ja testaa Azure AD kertakirjautumisen käytetään ohjelmiston nimeltä "Britta Simon" testikäyttäjän perusteella.

Kertakirjautumisen toimimaan Azure AD on tietää, mitkä asiat vastaavaan käyttäjän käytetään ohjelmiston Azure AD-käyttäjälle. Toisin sanoen linkki Azure AD-käyttäjä ja Aiheeseen liittyvät käytetään ohjelmiston suhteen on vahvistettava.

Linkki-yhteys on muodostettu määrittämällä **käyttäjänimi** arvo arvoksi **käyttäjänimi** käytetään ohjelmiston Azure AD.
 
Määrittäminen ja testaaminen Azure AD kertakirjautumisen käytetään ohjelmistojen kanssa, sinun on tehtävä seuraavat hakukyselyn:

1. **[Määrittäminen Azure AD yksittäisen Single Sign](#configuring-azure-ad-single-single-sign-on)** - käyttäjät voivat käyttää tätä toimintoa.
2. **[Käyttäjän testata luominen Azure AD](#creating-an-azure-ad-test-user)** - Testaa Azure AD kertakirjautumisen Britta Simon kanssa.
4. **[Käytetään ohjelmistojen luomista testata käyttäjän](#creating-a-halogen-software-test-user)** - tapahtumista Britta Simon ovat käytetään ohjelmia, jotka on linkitetty hän Azure AD esitys.
5. **[Käyttäjän testata määrittäminen Azure AD](#assigning-the-azure-ad-test-user)** - Britta Simon käyttämään Azure AD kertakirjautumisen käyttöön.
5. **[Testaus Single Sign](#testing-single-sign-on)** - voit tarkistaa, toimiiko määritykset.

### <a name="configuring-azure-ad-single-single-sign-on"></a>Azure AD-yhden kertakirjautumisen määrittäminen

Tässä osassa tavoitteena on Azure AD kertakirjautumisen Azure perinteinen portaalissa käyttöön ja määrittäminen kertakirjautumisen käytetään ohjelmistosovellus.


**Voit määrittää Azure AD kertakirjautumisen käytetään-ohjelmistolla toimimalla seuraavasti:**

1. Valitse Azure perinteinen portaalissa **Käytetään ohjelmisto** sovelluksen integrointi-sivun **Määritä kertakirjautumisen** **Määrittäminen Single Sign** -valintaikkunan avaaminen.

    ![Kertakirjautumisen määrittäminen][8]

2. **Miten käyttäjät voivat kirjautua käytetään ohjelmiston haluat käyttää** sivulla Valitse **Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Azure AD-Single Sign-On][9]

3. Valitse **Määritä sovelluksen asetukset** -sivulla toimimalla seuraavasti:  ![App-asetusten määrittäminen][10]
 
     a. Kirjoita **Merkki-URL** -tekstiruutuun käyttää käyttäjien kirjautua käyttämällä seuraavaa kaavaa käytetään ohjelmiston sovelluksen URL-osoite: *https://global.hgncloud.com/fabrikam/welcome.jsp*

     b. Valitse **Seuraava**.
 
4. Valitse **Määritä kertakirjautumisen, jotka lähettävät ohjelmisto** -sivulla valitsemalla **Lataa metatiedot**ja Tallenna paikallisesti tietokoneen metatiedot-tiedostoon.
    
    ![Mikä on Azure AD Connect][11]

5. Eri selainikkunassa Sign **Käytetään ohjelmiston** sovelluksen järjestelmänvalvojana.

6. Valitse **asetukset** -välilehti. 

    ![Mikä on Azure AD Connect][12]


7. Valitse vasemmassa siirtymisruudussa **SAML määritys**. 

    ![Mikä on Azure AD Connect][13]

8. **SAML-määritys** -sivulla toimimalla seuraavasti:  ![Azure AD Connect ominaisuudet][14]

    a. **Yksilöllinen tunnus**Valitse **NameID**.

    b. Valitse **Yksilöllinen tunnus kartat,** **käyttäjänimi**.

    c-näppäinyhdistelmää. Voit ladata ladatut metatieto-tiedosto valitsemalla **Selaa** ja valitse tiedosto ja sitten **Lataa tiedosto**.

    d. Voit testata määrityksen, valitsemalla **Suorita testi**. 

    > [AZURE.NOTE] Sinun täytyy odottaa viestin "*SAML testaus on suoritettu. Sulje tässä ikkunassa*". Sulje avattu selainikkunassa. **Ota käyttöön SAML** -valintaruutu on käytettävissä vain, jos testi on suoritettu.

    e. Valitse **Ota käyttöön SAML**.
    
    f. Valitse **Tallenna muutokset**. 


9. Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Valmis** **Määrittäminen kertakirjautuminen** -valintaikkunassa Sulje. 

    ![Mikä on Azure AD Connect][15]

10. Valitse **Sign Vahvista** -sivulla **Valmis**.  

    ![Mikä on Azure AD Connect][16]




### <a name="creating-an-azure-ad-test-user"></a>Azure AD-testikäyttäjän luominen
Tässä osassa tavoitteena on luotava testikäyttäjän kutsutaan Britta Simon Azure perinteinen portaalissa.

**Voit luoda testikäyttäjän Azure AD-toimimalla seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen portal**, valitse vasemmassa siirtymisruudussa.

    ![Mikä on Azure AD Connect][100] 

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Ylä-valikossa käyttäjäluettelon, näkyviin napsauttamalla **käyttäjät**.

    ![Mikä on Azure AD Connect][101] 

4. Avaa **Lisää käyttäjä** -valintaikkunan alareunassa työkalurivillä, valitse **Lisää käyttäjä**. 

    ![Mikä on Azure AD Connect][102] 

5. Tee valintaikkunassa **Kerro kyseisen käyttäjän tietoja** -sivulla seuraavasti:

    ![Mikä on Azure AD Connect][103] 
 
    a. **Käyttäjän tyyppi**Valitse **Uusi käyttäjä organisaatiossa**.

    b. Kirjoita käyttäjänimi- **tekstiruutuun** **BrittaSimon**.

    c-näppäinyhdistelmää. Valitse Seuraava.

6.  Suorita **Käyttäjäprofiili** -sivulla seuraavat toimet: 

    ![Mikä on Azure AD Connect][104] 

    a. Kirjoita **Ensimmäinen nimi** -tekstiruutuun **Britta**.  

    b. Valitse **Sukunimi** txtbox, tyyppi- **Simon**.

    c-näppäinyhdistelmää. Kirjoita **Näyttönimi** -tekstiruutuun **Britta Simon**.

    d. Valitse **rooli** -luettelosta **käyttäjä**.

    e. Valitse **Seuraava**.

7. Valitse **Hae tilapäinen salasana** -valintaikkunan sivulla **Luo**.

    ![Mikä on Azure AD Connect][105]  

8. **Hae tilapäinen salasana** -valintaikkunan sivulla toimimalla seuraavasti:

    ![Mikä on Azure AD Connect][106]   

    a. Kirjoita **Uusi salasana**arvo muistiin.
    b. Valitse **Valmis**.   
  
 
### <a name="creating-a-halogen-software-test-user"></a>Käytetään ohjelmiston testikäyttäjän luominen

Tässä osassa tavoitteena on nimeltään Britta Simon käytetään ohjelmiston käyttäjän luominen.

**Luo käyttäjä kutsutaan Britta Simon käytetään ohjelmisto, toimi seuraavasti:**

1. Kirjaudu järjestelmänvalvojana oman **Käytetään** ohjelmistosovellus.

2. Valitse **Käyttäjän keskellä** -välilehti ja valitse sitten **Luo käyttäjä**.

    ![Mikä on Azure AD Connect][300]  

3. Valitse **Uusi käyttäjä** -sivulla toimimalla seuraavasti:

    ![Mikä on Azure AD Connect][301]

    a. Kirjoita **Ensimmäinen nimi** -tekstiruutuun **Britta**. 
  
    b. Kirjoita **Simon** **Sukunimi** -tekstiruutuun.
  
    c-näppäinyhdistelmää. Kirjoita **käyttäjänimi** -tekstiruutuun **Brita Simonin käyttäjänimi Azure perinteinen-portaalissa**.
  
    d. Kirjoita salasana **salasana** -tekstiruutuun Britta varten.
  
    e. Valitse **Tallenna**.


### <a name="assigning-the-azure-ad-test-user"></a>Azure AD-testikäyttäjän määrittäminen

Tässä osassa tavoitteena on Britta Simon käyttämään Azure kertakirjautumisen hänen käyttöoikeuksien myöntämistä käytetään ohjelmiston ottaminen käyttöön.

![Mikä on Azure AD Connect][200]

**Jos haluat määrittää Britta Simon käytetään ohjelmiston, toimi seuraavasti:**

1. Azure perinteinen-portaalissa Avaa sovellukset-näkymässä kansio-näkymässä, valitse **sovellukset** yläreunan-valikossa.

    ![Mikä on Azure AD Connect][201]

2. Valitse sovellukset-luettelosta **Käytetään ohjelmiston**.

    ![Mikä on Azure AD Connect][202]

1. Valitse ylä-valikossa Valitse **käyttäjiä**.

    ![Mikä on Azure AD Connect][203]

1. Valitse käyttäjät-luettelosta **Britta Simon**.

    ![Mikä on Azure AD Connect][204]

2. Napsauta työkalurivin alaosassa **määrittäminen**.

    ![Mikä on Azure AD Connect][205]



### <a name="testing-single-sign-on"></a>Kertakirjautumisen testaaminen

Tässä osassa tavoitteena on Testaa Azure AD yksittäisen Sign-asetusten käyttäminen Access-paneeli.

Kun napsautat Access-ruudussa käytetään ohjelmisto-ruutu, sinun olisi Hae automaattisesti kirjautunut käyttöön käytetään ohjelmistosovellus.


## <a name="additional-resources"></a>Lisäresursseja

* [Luettelo siitä, miten voit integroida SaaS sovellusten Azure Active Directory-opetusohjelmat](active-directory-saas-tutorial-list.md)
* [Mikä on sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta?](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_01.png
[2]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_02.png
[3]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_03.png
[4]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_04.png
[5]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_05.png
[6]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_06.png
[7]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_07.png
[8]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_08.png
[9]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_09.png
[10]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_10.png
[11]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_11.png
[12]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_12.png
[13]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_13.png
[14]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_14.png
[15]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_15.png
[16]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_16.png
[100]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_100.png 
[101]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_101.png 
[102]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_102.png 
[103]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_103.png 
[104]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_104.png 
[105]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_105.png 
[106]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_106.png 
[200]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_200.png 
[201]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_201.png 
[202]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_202.png
[203]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_203.png
[204]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_204.png
[205]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_205.png
[300]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_300.png
[301]: ./media/active-directory-saas-halogen-software-tutorial/tutorial_halogen_301.png