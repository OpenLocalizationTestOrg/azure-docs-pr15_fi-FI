<properties
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi Litmos | Microsoft Azure"
    description="Opettele määrittämään kertakirjautumisen Azure Active Directory- ja Litmos välillä."
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


# <a name="tutorial-azure-active-directory-integration-with-litmos"></a>Opetusohjelma: Litmos Azure Active Directory-integrointi

Tässä opetusohjelmassa tavoitteena on, kuinka voit integroida Litmos Azure Active Directory (Azure AD).  
Azure AD-integraation Litmos avulla voit seuraavat edut: 

- Voit hallita Azure AD, jolla on pääsy Litmos 
- Voit ottaa käyttöön automaattisesti Hae kirjautunut käyttöön Litmos (kertakirjautumisen) niiden Azure AD-tilien käyttäjille
- Voit hallita keskitetyssä sijainnissa - Azure Active Directory-tilit 

Jos haluat tietää tarkempia tietoja SaaS sovelluksen integrointi Azure AD-kohdassa [mitä sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Edellytykset 

Määrittää Azure AD-integroinnin Litmos, tarvitset seuraavat:

- Azure AD-tilaus
- Litmos single-merkki käytössä-tilauksessa


> [AZURE.NOTE] Voit esikatsella vaiheet Tässä opetusohjelmassa, Emme suosittele käyttämällä tuotantoympäristössä.


Voit esikatsella vaiheet Tässä opetusohjelmassa-noudattamalla näitä suosituksia:

- Älä käytä tuotantoympäristössä, ellei se on tarpeen.
- Jos sinulla ei ole kokeiluversion Azure AD-ympäristössä, voit hankkia yksi kuukausi kokeiluversion [tähän](https://azure.microsoft.com/pricing/free-trial/). 

 
## <a name="scenario-description"></a>Skenaarion kuvaus
Tässä opetusohjelmassa tavoitteena on, jotta voit testata Azure AD kertakirjautumisen testiympäristössä.  
Tässä opetusohjelmassa kuvatut skenaarion koostuu kolme tärkeimmät rakenneosien:

1. Litmos lisääminen valikoimasta 
2. Yksittäinen määrittäminen ja testaus Azure AD-on


## <a name="adding-litmos-from-the-gallery"></a>Litmos lisääminen valikoimasta
Voit määrittää Litmos integroida Azure AD-haluat lisätä Litmos valikoimasta hallitun SaaS sovellusluettelo.

**Jos haluat lisätä Litmos valikoimasta, toimi seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen portal**, valitse vasemmassa siirtymisruudussa. 

    ![Active Directory][1]

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset][2]

4. Valitse sivun alareunassa **Lisää** .

    ![Sovellukset][3]

5. Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Sovellukset][4]

6. Kirjoita hakuruutuun **Litmos**.

    ![Sovellukset][5]

7. Valitse tulosruudussa **Litmos**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![Sovellukset][500]


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yksittäinen määrittäminen ja testaus Azure AD-on
Tässä osassa tavoitteena on noudattamalla voit määrittää ja testaa Azure AD kertakirjautumisen Litmos nimeltä "Britta Simon" testikäyttäjän perusteella.

Kertakirjautumisen toimimaan Azure AD on tiedettävä vastaavaan käyttäjän Litmos, Azure AD-käyttäjälle. Toisin sanoen suhteen Azure AD-käyttäjä ja Aiheeseen liittyvät Litmos-linkki on vahvistettava.  
Linkki-yhteys on muodostettu määrittämällä **käyttäjänimi** arvo arvoksi Litmos **käyttäjänimi** Azure AD.
 
Määrittäminen ja testaaminen Azure AD kertakirjautumisen Litmos kanssa, sinun on tehtävä seuraavat hakukyselyn:

1. **[Azure AD määrittäminen Single Sign](#configuring-azure-ad-single-single-sign-on)** - käyttäjät voivat käyttää tätä toimintoa.
2. **[Käyttäjän testata luominen Azure AD](#creating-an-azure-ad-test-user)** - Testaa Azure AD kertakirjautumisen Britta Simon kanssa.
4. **[Luomisesta Litmos testata käyttäjän](#creating-a-halogen-software-test-user)** - tapahtumista Britta Simon ovat Litmos, jotka on linkitetty hän Azure AD esitys.
5. **[Käyttäjän testata määrittäminen Azure AD](#assigning-the-azure-ad-test-user)** - Britta Simon käyttämään Azure AD kertakirjautumisen käyttöön.
5. **[Testaus Single Sign](#testing-single-sign-on)** - voit tarkistaa, toimiiko määritykset.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD-Single Sign-On määrittäminen

Tässä osassa tavoitteena on Azure AD kertakirjautumisen perinteinen Azure AD-portaalissa ottaminen käyttöön ja määrittää kertakirjautumisen Litmos-sovelluksessa.  
Tämän toimintosarjan osana on base-64-koodattu sertifikaattitiedosto luomiseen.  
Jos et ole tottunut näin, katso, [miten voit muuntaa binaariluvun varmenteen tekstitiedostoon](http://youtu.be/PlgrzUZ-Y1o).

Osana määritykset haluat mukauttaa Litmos sovelluksen **SAML suojaustunnuksen määritteet** .  

![Azure AD-Single Sign-On][17] 

**Määrittää Azure AD kertakirjautumisen Litmos, toimimalla seuraavasti:**

1. Azure AD perinteinen-portaalissa, integrointi **Litmos** sovellus-sivulla valitsemalla **Määritä kertakirjautumisen** **Määrittäminen Single Sign** -valintaikkunan avaaminen.

    ![Kertakirjautumisen määrittäminen][6] 

2. **Miten käyttäjät voivat kirjautua Litmos haluat käyttää** sivulla Valitse **Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.
 
    ![Azure AD-Single Sign-On][7] 


1. Kertakirjauksen Litmos yrityksen sivustoon (esimerkiksi: *https://azureapptest.litmos.com/account/Login*) järjestelmänvalvojana.

    ![Azure AD-Single Sign-On][21] 


1. Valitse vasemman reunan siirtymispalkissa **tilit**.

    ![Azure AD-Single Sign-On][22] 


1. Valitse **integroinnit** -välilehti.

    ![Azure AD-Single Sign-On][23] 


1. **Integroinnit** -välilehdessä **3 osapuolen integroinnit**Vieritä ja valitse sitten **SAML 2.0** -välilehti.

    ![Azure AD-Single Sign-On][24] 

1. Kopioi-arvoa **for litmos SAML endoiint on:**.

    ![Azure AD-Single Sign-On][26] 


3. Suorita seuraavat vaiheet Azure perinteinen portaalissa **App-asetusten määrittäminen** -sivulla:

    ![Azure AD-Single Sign-On][8] 
 
    a. Kirjoita **tunnus** -tekstiruutuun käytettävä käyttäjien Sign Litmos sovelluksen URL-osoite (esimerkiksi: *https://azureapptest.litmos.com/account/Login*).
     
    b. Liitä kopioimasi Litmos edellisessä vaiheessa sovelluksesta arvo **Vastaa URL** -tekstiruutuun.

    c-näppäinyhdistelmää. Valitse **Seuraava**.
 
4. Valitse **Määritä kertakirjautumisen osoitteessa Litmos** -sivulla toimimalla seuraavasti:

    ![Azure AD-Single Sign-On][2] 

    a. Valitse Lataa sertifikaatti ja tallenna sitten tiedosto tietokoneesta.


1. **Litmos** -sovelluksessa toimi seuraavasti:

    ![Azure AD-Single Sign-On][25] 

    a. Valitse **Ota käyttöön SAML**.

    b. Luo **base-64-koodattu** tiedosto ladattu varmennetta.  

    >[AZURE.TIP] Lisätietoja on artikkelissa [muuntaa binaariluvun varmenteen tekstitiedostoon](http://youtu.be/PlgrzUZ-Y1o)

    c-näppäinyhdistelmää. Avaa base-64-koodattu varmennetta Muistiossa, kopioi Leikepöydän sisällön se ja liitä se **SAML X.509-varmenne** -tekstiruutuun.

    d. Valitse **Tallenna muutokset**.


6. Azure AD perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Seuraava**. 

    ![Azure AD-Single Sign-On][10]

7. Valitse **Sign Vahvista** -sivulla **Valmis**.  
  
    ![Azure AD-Single Sign-On][11]


20. Valitse ylä-valikossa **määritteet** **SAML suojaustunnuksen määritteet** -valintaikkunan avaaminen. 

    ![Kertakirjautumisen määrittäminen][12]


24. Valitse **Lisää käyttäjä-määrite** -valintaikkunassa seuraavasti: 

    ![Kertakirjautumisen määrittäminen][14]

  	| Määritteen nimi | Määritteen arvo |
  	| ---            | ---             |
  	| Sähköpostin          | User.Mail       |
  	| Etunimi      | User.givenName  |
  	| Sukunimi       | User.surname    |

    Tietoja kullekin riville edellä olevassa taulukossa toimi seuraavasti:
   
    a. Valitse **Lisää käyttäjä-määrite**. 

    ![Kertakirjautumisen määrittäminen][15]


    a. Kirjoita **Määritteen nimi** näkyy kyseisen rivin **Määritteen nimi** -tekstiruutuun.

    b. Valitse **Määritteen** kyseisen rivin.

    c-näppäinyhdistelmää. Valitse **Valmis** , Sulje **Lisää käyttäjä-määrite** -valintaikkuna.


25. Valitse **Ota muutokset käyttöön**. 

    ![Kertakirjautumisen määrittäminen][16]




### <a name="creating-an-azure-ad-test-user"></a>Azure AD-testikäyttäjän luominen
Tässä osassa tavoitteena on luotava testikäyttäjän kutsutaan Britta Simon Azure perinteinen portaalissa.  

![Luo Azure AD-käyttäjä][20]

**Voit luoda testikäyttäjän Azure AD-toimimalla seuraavasti:**

1. Valitse **Active Directory** **Azure clasic portal**, valitse vasemmassa siirtymisruudussa.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-litmos-tutorial/create_aaduser_09.png)  

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Ylä-valikossa käyttäjäluettelon, näkyviin napsauttamalla **käyttäjät**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-litmos-tutorial/create_aaduser_03.png) 
 
4. Avaa **Lisää käyttäjä** -valintaikkunan alareunassa työkalurivillä, valitse **Lisää käyttäjä**. 

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-litmos-tutorial/create_aaduser_04.png) 

5. Tee valintaikkunassa **Kerro kyseisen käyttäjän tietoja** -sivulla seuraavasti: 

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-litmos-tutorial/create_aaduser_05.png)  

    a. **Käyttäjän tyyppi**Valitse **Uusi käyttäjä organisaatiossa**.

    b. Kirjoita käyttäjänimi- **tekstiruutuun** **BrittaSimon**.

    c-näppäinyhdistelmää. Valitse **Seuraava**.

6.  Suorita **Käyttäjäprofiili** -sivulla seuraavat toimet: 

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-litmos-tutorial/create_aaduser_06.png) 
 
    a. Kirjoita **Ensimmäinen nimi** -tekstiruutuun **Britta**.  

    b. Kirjoita **Sukunimi** -tekstiruutuun tyyppi- **Simon**.

    c-näppäinyhdistelmää. Kirjoita **Näyttönimi** -tekstiruutuun **Britta Simon**.

    d. Valitse **rooli** -luettelosta **käyttäjä**.
    e. Valitse **Seuraava**.

7. Valitse **Hae tilapäinen salasana** -valintaikkunan sivulla **Luo**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-litmos-tutorial/create_aaduser_07.png) 
 
8. **Hae tilapäinen salasana** -valintaikkunan sivulla toimimalla seuraavasti:

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-litmos-tutorial/create_aaduser_08.png) 
  
    a. Kirjoita **Uusi salasana**arvo muistiin.

    b. Valitse **Valmis**.   

  
 
### <a name="creating-a-litmos-test-user"></a>Litmos testikäyttäjän luominen

Tässä osassa tavoitteena on nimeltään Britta Simon Litmos käyttäjän luominen.  
Litmos-sovellus tukee vain kerran valmistelu. Tämä tarkoittaa sitä, käyttäjätili luodaan automaattisesti tarvittaessa aikana yritys käyttää sovelluksen Access-paneeli.

**Luo kutsutaan Britta Simon Litmos käyttäjä, toimi seuraavasti:**


1. Kertakirjauksen Litmos yrityksen sivustoon (esimerkiksi: *https://azureapptest.litmos.com/account/Login*) järjestelmänvalvojana.

    ![Azure AD-Single Sign-On][21] 


1. Valitse vasemman reunan siirtymispalkissa **tilit**.

    ![Azure AD-Single Sign-On][22] 


1. Valitse **integroinnit** -välilehti.

    ![Azure AD-Single Sign-On][23] 


1. **Integroinnit** -välilehdessä **3 osapuolen integroinnit**Vieritä ja valitse sitten **SAML 2.0** -välilehti.

    ![Azure AD-Single Sign-On][24] 

1. Valitse **muodostetaan käyttäjät:**.

    ![Azure AD-Single Sign-On][27] 


### <a name="assigning-the-azure-ad-test-user"></a>Azure AD-testikäyttäjän määrittäminen

Tässä osassa tavoitteena on Britta Simon käyttämään Azure kertakirjautumisen hänen käyttöoikeuden myöntämisestä Litmos ottaminen käyttöön.

![Määrittää käyttäjälle][200] 

**Jos haluat määrittää Britta Simon Litmos, toimi seuraavasti:**

1. Azure perinteinen-portaalissa Avaa sovellukset-näkymässä kansio-näkymässä, valitse **sovellukset** yläreunan-valikossa.

    ![Määrittää käyttäjälle][201] 

2. Valitse sovellukset-luettelosta **Litmos**.

    ![Määrittää käyttäjälle][202] 

1. Valitse ylä-valikossa Valitse **käyttäjiä**.

    ![Määrittää käyttäjälle][203] 

1. Valitse käyttäjät-luettelosta **Britta Simon**.

2. Napsauta työkalurivin alaosassa **määrittäminen**.

    ![Määrittää käyttäjälle][205]



### <a name="testing-single-sign-on"></a>Kertakirjautumisen testaaminen

Tässä osassa tavoitteena on Testaa Azure AD yksittäisen Sign-asetusten käyttäminen Access-paneeli.  
Access-ruudussa Litmos-ruutua napsauttaessasi sinun pitäisi saada automaattisesti kirjautunut-on Litmos sovelluksen.


## <a name="additional-resources"></a>Lisäresursseja

* [Luettelo siitä, miten voit integroida SaaS sovellusten Azure Active Directory-opetusohjelmat](active-directory-saas-tutorial-list.md)
* [Mikä on sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_01.png
[500]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_02.png

[6]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_05.png
[7]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_03.png
[8]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_04.png
[9]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_05.png
[10]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_07.png
[12]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_80.png
[13]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_81.png
[14]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_82.png
[15]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_81.png
[16]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_19.png
[17]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_67.png


[20]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_100.png
[21]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_60.png
[22]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_61.png
[23]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_62.png
[24]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_63.png
[25]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_64.png
[26]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_65.png
[27]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_66.png

[200]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_68.png
[203]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-litmos-tutorial/tutorial_general_205.png


[400]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_400.png
[401]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_401.png
[402]: ./media/active-directory-saas-litmos-tutorial/tutorial_litmos_402.png





