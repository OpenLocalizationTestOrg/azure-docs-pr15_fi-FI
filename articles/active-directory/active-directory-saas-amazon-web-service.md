<properties
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi kanssa Amazon Web Service (sisältyy AWS) | Microsoft Azure"
    description="Opettele käyttämään sisältyy Amazon Web Services (AWS) Azure Active Directory-hakemistosta käyttöön kertakirjautumisen, automaattinen valmistelu ja lisää!"
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


# <a name="tutorial-azure-active-directory-integration-with-amazon-web-service-aws"></a>Opetusohjelma: Azure Active Directory-integrointi kanssa Amazon Web Service (sisältyy AWS)

Tässä opetusohjelmassa tavoitteena on, kuinka voit integroida Azure Active Directory (Azure AD) sisältyy Amazon Web Service (AWS).  
Azure AD-integraation sisältyy (Amazon Web AWS)-palvelun avulla voit seuraavat edut: 

- Voit hallita Azure AD, joilla on käyttöoikeus, Amazon Web Service (sisältyy AWS) 
- Voit ottaa käyttöön automaattisesti Hae kirjautunut sisään, Amazon Web Service (sisältyy AWS) (kertakirjautumisen) käyttäjille heidän Azure AD-tilien kanssa
- Voit hallita asiakkaiden yhdessä keskitetyssä sijainnissa - Azure perinteinen portal

Jos haluat tietää tarkempia tietoja SaaS sovelluksen integrointi Azure AD-kohdassa [mitä sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Edellytykset 

Voit määrittää Azure AD-integroinnin kanssa Amazon Web Service (sisältyy AWS), tarvitset seuraavat kohteet:

- Azure AD-tilaus
- On sisältyy Amazon Web Service (AWS) single-etumerkki käytössä-tilauksessa


> [AZURE.NOTE] Voit esikatsella vaiheet Tässä opetusohjelmassa, Emme suosittele käyttämällä tuotantoympäristössä.


Voit esikatsella vaiheet Tässä opetusohjelmassa-noudattamalla näitä suosituksia:

- Älä käytä tuotantoympäristössä, ellei se on tarpeen.
- Jos sinulla ei ole kokeiluversion Azure AD-ympäristössä, voit hankkia yksi kuukausi kokeiluversion [tähän](https://azure.microsoft.com/pricing/free-trial/). 

 
## <a name="scenario-description"></a>Skenaarion kuvaus
Tässä opetusohjelmassa tavoitteena on, jotta voit testata Azure AD kertakirjautumisen testiympäristössä.  
Tässä opetusohjelmassa kuvatut skenaarion koostuu kolme tärkeimmät rakenneosien:

1. Sisältyy (Amazon Web AWS)-palvelun lisääminen valikoimasta 
2. Yksittäinen määrittäminen ja testaus Azure AD-on


## <a name="adding-amazon-web-service-aws-from-the-gallery"></a>Sisältyy (Amazon Web AWS)-palvelun lisääminen valikoimasta
Voit määrittää, Amazon Web Service (sisältyy AWS) integroida Azure AD-haluat lisätä sisältyy Amazon Web Service (AWS) valikoimasta hallitun SaaS sovellusluettelo.

### <a name="to-add-amazon-web-service-aws-from-the-gallery-perform-the-following-steps"></a>Lisää sisältyy Amazon Web Service (AWS) valikoimasta, toimi seuraavasti:

1. Valitse **Active Directory** **Azure perinteinen portal**, valitse vasemmassa siirtymisruudussa. 

    ![Active Directory][1] 

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** . 
   
    ![Sovellukset][2]

4. Valitse sivun alareunassa **Lisää** . 
   
    ![Sovellukset][3]

5. Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**. 
   
    ![Sovellukset][4]

6. Kirjoita hakuruutuun **Sisältyy Amazon Web Service (AWS)**.
   
    ![Sovellukset][5]

7. Valitse tulosruudussa **Sisältyy Amazon Web Service (AWS)**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![Sovellukset][6]



##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yksittäinen määrittäminen ja testaus Azure AD-on
Tässä osassa tavoitteena on noudattamalla voit määrittää ja testaa Azure AD kertakirjautumisen kanssa Amazon Web Service (sisältyy AWS) nimeltä "Britta Simon" testikäyttäjän perusteella.

Kertakirjautumisen toimimaan Azure AD on tiedettävä Azure AD-käyttäjälle vastaavaan käyttäjä-Amazon Web Service (sisältyy AWS). Toisin sanoen linkki Azure AD-käyttäjä ja liittyvä käyttäjä-Amazon Web Service (sisältyy AWS) suhteen on vahvistettava.  
Linkki-yhteys on muodostettu määrittämällä arvo **käyttäjänimi** **käyttäjänimi** -Amazon Web Service (sisältyy AWS) arvona Azure AD.
 
Määrittäminen ja testaaminen Azure AD kertakirjautumisen kanssa Amazon Web Service (sisältyy AWS), sinun on tehtävä seuraavat hakukyselyn:

1. **[Määrittäminen Azure AD yksittäisen Single Sign](#configuring-azure-ad-single-single-sign-on)** - käyttäjät voivat käyttää tätä toimintoa.
2. **[Käyttäjän testata luominen Azure AD](#creating-an-azure-ad-test-user)** - Testaa Azure AD kertakirjautumisen Britta Simon kanssa.
4. **[Käyttäjän luominen Amazon Web Service (sisältyy AWS) Testaa](#creating-a-halogen-software-test-user)** - on vastaavaan Britta Simon, valitse Amazon Web Service (sisältyy AWS), joka on linkitetty hän Azure AD esitys.
5. **[Käyttäjän testata määrittäminen Azure AD](#assigning-the-azure-ad-test-user)** - Britta Simon käyttämään Azure AD kertakirjautumisen käyttöön.
5. **[Testaus Single Sign](#testing-single-sign-on)** - voit tarkistaa, toimiiko määritykset.

### <a name="configuring-azure-ad-single-single-sign-on"></a>Azure AD-yhden kertakirjautumisen määrittäminen

Tässä osassa tavoitteena on Azure AD kertakirjautumisen Azure perinteinen portaalissa ottaminen käyttöön ja määrittää kertakirjautumisen sisältyy Amazon Web Service (AWS)-sovelluksessa.  
Sisältyy Amazon Web Service (AWS)-sovelluksen odottaa SAML vahvistukset tietyssä muodossa, jossa pyydetään Lisää mukautettujen määritteiden yhdistämismäärityksiä **saml suojaustunnuksen määritteet** -määrityksiin. Seuraavassa näyttökuvassa näkyy esimerkki tämän.


![Kertakirjautumisen määrittäminen][27]

**Jos haluat määrittää Azure AD kertakirjautumisen kanssa Amazon Web Service (sisältyy AWS), toimimalla seuraavasti:**

1. Valitse Azure perinteinen portaalissa integrointi **Sisältyy Amazon Web Service (AWS)** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen Single Sign** -valintaikkunan avaaminen.

    ![Kertakirjautumisen määrittäminen][7]

2. Valitse **miten haluat kirjautua sisään, Amazon Web Service (sisältyy AWS) käyttäjät** -sivulla Valitse **Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Kertakirjautumisen määrittäminen][8]

3. Valitse **Määritä sovelluksen asetukset** -sivulla Seuraava. 

    ![Sovelluksen asetusten määrittäminen][9]
 
4. Valitse **Määritä kertakirjautumisen osoitteessa Amazon Web Service (sisältyy AWS)** -sivulla valitsemalla **Lataa metatiedot**ja Tallenna paikallisesti tietokoneen metatiedot-tiedostoon.

    ![Kertakirjautumisen määrittäminen][10]

5. Eri selainikkunassa Sign sisältyy Amazon Web Service (AWS) yrityksen sivustoon järjestelmänvalvojana.

6. Valitse **Konsolin aloitus**.

    ![Kertakirjautumisen määrittäminen][11]

7. Valitse **tunnistetietojen ja käyttöoikeushallinta**. 

    ![Kertakirjautumisen määrittäminen][12]

8. Valitse **Tunnistetietojen toimittajat**ja valitse sitten **Luo toimittaja**. 

    ![Kertakirjautumisen määrittäminen][13]

9. Valitse **Palvelun määrittäminen** -sivulla toimimalla seuraavasti: 

    ![Kertakirjautumisen määrittäminen][14]

     a. Valitse **SAML** **Tarjoajatyyppi**.

     b. Kirjoita **Nimi** -tekstiruutuun tarjoajan nimi (esimerkiksi: *WAAD*).

     c-näppäinyhdistelmää. Voit ladata ladatut metatieto-tiedosto valitsemalla **Tiedosto**.

     d. Valitse **Seuraava**.


10. Valitse **Tarkista tarjoajan tiedot** -sivulla **Luo**. 

    ![Kertakirjautumisen määrittäminen][15]

11. Valitse **roolit**ja valitse sitten **Luo uusi rooli**. 

    ![Kertakirjautumisen määrittäminen][16]

12. Valitse **Määritä rooli-nimi** -valintaikkunassa seuraavasti: 

    ![Kertakirjautumisen määrittäminen][17]

    a. Kirjoita **Roolinimi** -tekstiruutuun roolinimi (esimerkiksi: *TestUser*).

    b. Valitse **Seuraava**.

13. **Valitse roolityyppi** -valintaikkunassa seuraavasti: 

    ![Kertakirjautumisen määrittäminen][18]

    a. Valitse **rooli Identity-palvelun käytön**.

    b. ** **Myönnä Web kertakirjautumisen (WebSSO) käyttää SAML palvelut** -kohdassa Valitse.**


14. Valitse **Muodostaa luota** -valintaikkunassa seuraavasti:  

    ![Kertakirjautumisen määrittäminen][19]
     
     a. SAML-palvelu, valitse luomasi previousley SAML-palvelu (esimerkiksi: *WAAD*) 

     b. Valitse **Seuraava**.


15. Valitse **Tarkista roolin luota** -valintaikkunan **Seuraavaan vaiheeseen**. 

    ![Kertakirjautumisen määrittäminen][32]


16. Valitse **Liitä käytännön** -valintaikkunan **Seuraavaan vaiheeseen**.  

    ![Kertakirjautumisen määrittäminen][33]


17. Valitse **Tarkista** -valintaikkunassa seuraavasti:   

    ![Kertakirjautumisen määrittäminen][34]

     a. Kopioi **Rooli ARN** -arvo.

     b. Kopioi **Luotetut kohteiden** ARN-arvo.

     c-näppäinyhdistelmää. Valitse **Luo rooli**. 

18. Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Seuraava**.

    ![Mikä on Azure AD Connect][20]

19. Valitse **Sign Vahvista** -sivulla **Valmis** **määrittäminen single Sign** -valintaikkunassa Sulje.

    ![Mikä on Azure AD Connect][22]


20. Valitse ylä-valikossa **määritteet** **SAML suojaustunnuksen määritteet** -valintaikkunan avaaminen. 

    ![Kertakirjautumisen määrittäminen][21]

21. Valitse **Lisää käyttäjä-määrite**. 

    ![Kertakirjautumisen määrittäminen][23]

22. Valitse Lisää käyttäjä-määrite-valintaikkunassa toimimalla seuraavasti. 

    ![Kertakirjautumisen määrittäminen][24] 

    a. Kirjoita **https://aws.amazon.com/SAML/Attributes/Role** **Määritteen nimi** -tekstiruutuun.

    b. Kirjoita **Määritteen arvo** -tekstiruutuun **[rooli ARN arvo], [Luotetut kohteen ARN arvo]**.

    >[AZURE.TIP] Nämä ovat arvoja, kun olet luonut tehtäväsi kopioinut tarkistaminen-valintaikkunassa. 

    c-näppäinyhdistelmää. Valitse **Valmis** , Sulje **Lisää käyttäjä-määrite** -valintaikkuna.

23. Valitse **Lisää käyttäjä-määrite**. 

    ![Kertakirjautumisen määrittäminen][23]


24. Valitse Lisää käyttäjä-määrite-valintaikkunassa toimimalla seuraavasti. 

    ![Kertakirjautumisen määrittäminen][25]


     a. Kirjoita **https://aws.amazon.com/SAML/Attributes/RoleSessionName** **Määritteen nimi** -tekstiruutuun.

     b. Kirjoita **Määritteen arvo** -tekstiruutuun tai valitse **user.userprincipalname** avattavasta luettelosta.
     
    ![Kertakirjautumisen määrittäminen][35]
    

     c-näppäinyhdistelmää. Valitse **Valmis** , Sulje **Lisää käyttäjä-määrite** -valintaikkuna.


25. Valitse **Ota muutokset käyttöön**. 

    ![Kertakirjautumisen määrittäminen][26]





### <a name="creating-an-azure-ad-test-user"></a>Azure AD-testikäyttäjän luominen

Tässä osassa tavoitteena on luotava testikäyttäjän kutsutaan Britta Simon Azure perinteinen portaalissa.

![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-amazon-web-service/create_aaduser_01.png)

**Voit luoda testikäyttäjän Azure AD-toimimalla seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen portal**, valitse vasemmassa siirtymisruudussa.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-amazon-web-service/create_aaduser_02.png) 

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Ylä-valikossa käyttäjäluettelon, näkyviin napsauttamalla **käyttäjät**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-amazon-web-service/create_aaduser_03.png) 
 
4. Avaa **Lisää käyttäjä** -valintaikkunan alareunassa työkalurivillä, valitse **Lisää käyttäjä**. 

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-amazon-web-service/create_aaduser_04.png) 

5. Tee valintaikkunassa **Kerro kyseisen käyttäjän tietoja** -sivulla seuraavasti: 

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-amazon-web-service/create_aaduser_05.png) 

  1. Käyttäjänä tyyppi Valitse uusi käyttäjä organisaatiossa.
  2. Kirjoita käyttäjänimi- **tekstiruutuun** **BrittaSimon**.
  3. Valitse Seuraava.

6.  Suorita **Käyttäjäprofiili** -sivulla seuraavat toimet: 

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-amazon-web-service/create_aaduser_06.png) 

    a. Kirjoita **Ensimmäinen nimi** -tekstiruutuun **Britta**.  

    b. Valitse **Sukunimi** txtbox, tyyppi- **Simon**.
  
    c-näppäinyhdistelmää. Kirjoita **Näyttönimi** -tekstiruutuun **Britta Simon**.

    d. Valitse **rooli** -luettelosta **käyttäjä**.
  
    e. Valitse **Seuraava**.

7. Valitse **Hae tilapäinen salasana** -valintaikkunan sivulla **Luo**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-amazon-web-service/create_aaduser_07.png) 
 
8. **Hae tilapäinen salasana** -valintaikkunan sivulla toimimalla seuraavasti:

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-amazon-web-service/create_aaduser_08.png) 

    a. Kirjoita **Uusi salasana**arvo muistiin.
  
    b. Valitse **Valmis**.   
  
 
### <a name="creating-a-amazon-web-service-aws-test-user"></a>Sisältyy Amazon Web Service (AWS) testikäyttäjän luominen

Tässä osassa tavoitteena on nimeltään Britta Simon-Amazon Web Service (sisältyy AWS) käyttäjän luominen.

### <a name="to-create-a-user-called-britta-simon-in-amazon-web-service-aws-perform-the-following-steps"></a>Luo käyttäjä kutsutaan Britta Simon-Amazon Web Service (sisältyy AWS), toimi seuraavasti:

1. Kirjaudu sisään **Sisältyy Amazon Web Service (AWS)** yrityksesi sivuston järjestelmänvalvojana.

2. Napsauta **Konsolin aloitus** -kuvaketta. 

    ![Kertakirjautumisen määrittäminen][11]

3. Valitse käyttäjä ja käyttää hallinta. 

    ![Kertakirjautumisen määrittäminen][28]

4. Raporttinäkymät-ikkunassa Valitse käyttäjät ja valitse sitten Luo uusia käyttäjiä. 

    ![Kertakirjautumisen määrittäminen][29]

5. Valitse Luo käyttäjä-valintaikkunassa seuraavasti: 

    ![Kertakirjautumisen määrittäminen][30]

     a. Kirjoita **Kirjoita käyttäjien nimet** , tekstiruutujen Azure AD Brita Simonin käyttäjänimi (userprincipalname).

     b. Valitse **Luo**.




### <a name="assigning-the-azure-ad-test-user"></a>Azure AD-testikäyttäjän määrittäminen

Tässä osassa tavoitteena on ottaminen käyttöön Britta Simon käyttämään Azure kertakirjautumisen hänen käyttöoikeuksien myöntäminen, Amazon Web Service (sisältyy AWS).

![Määrittää käyttäjälle][31]

**Jos haluat määrittää Britta Simon CloudPassage, toimi seuraavasti:**

1. Azure perinteinen-portaalissa Avaa sovellukset-näkymässä kansio-näkymässä, valitse **sovellukset** yläreunan-valikossa.

    ![Määrittää käyttäjälle][26]

2. Valitse sovellukset-luettelosta **Sisältyy Amazon Web Service (AWS)**.

    ![Määrittää käyttäjälle][27]

1. Valitse ylä-valikossa Valitse **käyttäjiä**.

    ![Määrittää käyttäjälle][25]

1. Valitse käyttäjät-luettelosta **Britta Simon**.

2. Napsauta työkalurivin alaosassa **määrittäminen**.

    ![Määrittää käyttäjälle][29]

### <a name="testing-single-sign-on"></a>Kertakirjautumisen testaaminen

Tässä osassa tavoitteena on Testaa Azure AD yksittäisen Sign-asetusten käyttäminen Access-paneeli.  
Access-ruudussa sisältyy Amazon Web Service (AWS)-ruutua napsauttaessasi voit olisi Hae automaattisesti kirjautunut sisään sisältyy Amazon Web Service (AWS)-sovellukseen.


## <a name="additional-resources"></a>Lisäresursseja

* [Luettelo siitä, miten voit integroida SaaS sovellusten Azure Active Directory-opetusohjelmat](active-directory-saas-tutorial-list.md)
* [Mikä on sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta?](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-amazon-web-service/tutorial_general_01.png
[2]: ./media/active-directory-saas-amazon-web-service/tutorial_general_02.png
[3]: ./media/active-directory-saas-amazon-web-service/tutorial_general_03.png
[4]: ./media/active-directory-saas-amazon-web-service/tutorial_general_04.png
[5]: ./media/active-directory-saas-amazon-web-service/ic795019.png
[6]: ./media/active-directory-saas-amazon-web-service/ic795020.png
[7]: ./media/active-directory-saas-amazon-web-service/ic795027.png
[8]: ./media/active-directory-saas-amazon-web-service/ic795028.png
[9]: ./media/active-directory-saas-amazon-web-service/capture23.png
[10]: ./media/active-directory-saas-amazon-web-service/capture24.png
[11]: ./media/active-directory-saas-amazon-web-service/ic795031.png
[12]: ./media/active-directory-saas-amazon-web-service/ic795032.png
[13]: ./media/active-directory-saas-amazon-web-service/ic795033.png
[14]: ./media/active-directory-saas-amazon-web-service/ic795034.png
[15]: ./media/active-directory-saas-amazon-web-service/ic795035.png
[16]: ./media/active-directory-saas-amazon-web-service/ic795022.png
[17]: ./media/active-directory-saas-amazon-web-service/ic795023.png
[18]: ./media/active-directory-saas-amazon-web-service/ic795024.png
[19]: ./media/active-directory-saas-amazon-web-service/ic795025.png
[20]: ./media/active-directory-saas-amazon-web-service/ic7950351.png
[21]: ./media/active-directory-saas-amazon-web-service/tutorial_general_80.png
[22]: ./media/active-directory-saas-amazon-web-service/ic7950352.png
[23]: ./media/active-directory-saas-amazon-web-service/tutorial_general_81.png
[24]: ./media/active-directory-saas-amazon-web-service/ic7950353.png
[25]: ./media/active-directory-saas-amazon-web-service/tutorial_general_15.png
[26]: ./media/active-directory-saas-amazon-web-service/tutorial_general_18.png
[27]: ./media/active-directory-saas-amazon-web-service/ic7950357.png
[28]: ./media/active-directory-saas-amazon-web-service/ic7950321.png
[29]: ./media/active-directory-saas-amazon-web-service/tutorial_general_16.png
[30]: ./media/active-directory-saas-amazon-web-service/ic795038.png
[31]: ./media/active-directory-saas-amazon-web-service/tutorial_general_17.png
[32]: ./media/active-directory-saas-amazon-web-service/ic7950251.png
[33]: ./media/active-directory-saas-amazon-web-service/ic7950252.png
[34]: ./media/active-directory-saas-amazon-web-service/ic7950253.png
[35]: ./media/active-directory-saas-amazon-web-service/user_attributes_01.png






















