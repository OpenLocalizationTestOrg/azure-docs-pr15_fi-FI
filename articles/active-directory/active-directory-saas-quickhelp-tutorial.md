<properties
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi QuickHelp | Microsoft Azure"
    description="Opettele määrittämään kertakirjautumisen Azure Active Directory- ja QuickHelp välillä."
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


# <a name="tutorial-azure-active-directory-integration-with-quickhelp"></a>Opetusohjelma: QuickHelp Azure Active Directory-integrointi

Tässä opetusohjelmassa tavoitteena on, kuinka voit integroida QuickHelp Azure Active Directory (Azure AD).  
Azure AD-integraation QuickHelp avulla voit seuraavat edut: 

- Voit hallita Azure AD, jolla on pääsy QuickHelp 
- Voit ottaa käyttöön automaattisesti Hae kirjautunut käyttöön QuickHelp (kertakirjautumisen) niiden Azure AD-tilien käyttäjille
- Voit hallita asiakkaiden yhdessä keskitetyssä sijainnissa - Azure perinteinen portal

Jos haluat tietää tarkempia tietoja SaaS sovelluksen integrointi Azure AD-kohdassa [mitä sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Edellytykset 

Määrittää Azure AD-integroinnin QuickHelp, tarvitset seuraavat:

- Azure AD-tilaus
- QuickHelp single-merkki käytössä-tilauksessa


> [AZURE.NOTE] Voit esikatsella vaiheet Tässä opetusohjelmassa, Emme suosittele käyttämällä tuotantoympäristössä.


Voit esikatsella vaiheet Tässä opetusohjelmassa-noudattamalla näitä suosituksia:

- Älä käytä tuotantoympäristössä, ellei se on tarpeen.
- Jos sinulla ei ole kokeiluversion Azure AD-ympäristössä, voit hankkia yksi kuukausi kokeiluversion [tähän](https://azure.microsoft.com/pricing/free-trial/). 

 
## <a name="scenario-description"></a>Skenaarion kuvaus
Tässä opetusohjelmassa tavoitteena on, jotta voit testata Azure AD kertakirjautumisen testiympäristössä.  
Tässä opetusohjelmassa kuvatut skenaarion kuuluu kaksi tärkeimmät rakenneosien:

1. QuickHelp lisääminen valikoimasta 
2. Yksittäinen määrittäminen ja testaus Azure AD-on


## <a name="adding-quickhelp-from-the-gallery"></a>QuickHelp lisääminen valikoimasta
Voit määrittää QuickHelp integroida Azure AD-haluat lisätä QuickHelp valikoimasta hallitun SaaS sovellusluettelo.

**Jos haluat lisätä QuickHelp valikoimasta, toimi seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen portal**, valitse vasemmassa siirtymisruudussa. 

    ![Active Directory][1]

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset][2]

4. Valitse sivun alareunassa **Lisää** .

    ![Sovellukset][3]

5. Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Sovellukset][4]

6. Kirjoita hakuruutuun **QuickHelp**.

    ![Sovellukset][5]

7. Valitse tulosruudussa **QuickHelp**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![Sovellukset][500]


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yksittäinen määrittäminen ja testaus Azure AD-on
Tässä osassa tavoitteena on noudattamalla voit määrittää ja testaa Azure AD kertakirjautumisen QuickHelp nimeltä "Britta Simon" testikäyttäjän perusteella.


Määrittäminen ja testaaminen Azure AD kertakirjautumisen QuickHelp kanssa, sinun on tehtävä seuraavat hakukyselyn:

1. **[Azure AD määrittäminen Single Sign](#configuring-azure-ad-single-single-sign-on)** - käyttäjät voivat käyttää tätä toimintoa.
2. **[Käyttäjän testata luominen Azure AD](#creating-an-azure-ad-test-user)** - Testaa Azure AD kertakirjautumisen Britta Simon kanssa.
4. **[Luomisesta QuickHelp testata käyttäjän](#creating-a-quickhelp-test-user)** - tapahtumista Britta Simon ovat QuickHelp, jotka on linkitetty hän Azure AD esitys.
5. **[Käyttäjän testata määrittäminen Azure AD](#assigning-the-azure-ad-test-user)** - Britta Simon käyttämään Azure AD kertakirjautumisen käyttöön.
5. **[Testaus Single Sign](#testing-single-sign-on)** - voit tarkistaa, toimiiko määritykset.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD-Single Sign-On määrittäminen

Tässä osassa tavoitteena on Azure AD kertakirjautumisen Azure perinteinen portaalissa ottaminen käyttöön ja määrittää kertakirjautumisen QuickHelp-sovelluksessa.


**Määrittää Azure AD kertakirjautumisen QuickHelp, toimimalla seuraavasti:**

1. Valitse Azure perinteinen portaalissa integrointi **QuickHelp** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen Single Sign** -valintaikkunan avaaminen.

    ![Kertakirjautumisen määrittäminen][6] 

2. **Miten käyttäjät voivat kirjautua QuickHelp haluat käyttää** sivulla Valitse **Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Azure AD-Single Sign-On][7] 

3. Valitse **Määritä sovelluksen asetukset** -sivulla toimimalla seuraavasti:

    ![Sovelluksen asetusten määrittäminen][8] 
 
     a. Kirjoita **Merkki-URL** -tekstiruutuun käyttämä käyttäjille Sign QuickHelp-sivuston URL-osoite (esimerkiksi:* https://quickhelp.com/bsiazure/*).

     > [AZURE.NOTE] Jos et tiedä merkki-URL-Osoitteen arvo, ota yhteyttä QuickHelp tukihenkilöstölle.

     b. Valitse **Seuraava**.

 
4. Valitse **Määritä kertakirjautumisen osoitteessa QuickHelp** -sivulla Suorita vaiheet: Valitse seuraavat **Lataa metatiedot**ja Tallenna paikallisesti tietokoneen metatiedot-tiedostoon.

    ![Mikä on Azure AD Connect][9] 



1. Kertakirjauksen QuickHelp yrityksen sivustoon järjestelmänvalvojana.

2. Valitse ylä-valikossa Valitse **järjestelmänvalvoja**.

    ![Kertakirjautumisen määrittäminen][21]


1. Valitse **QuickHelp järjestelmänvalvoja** -valikossa **asetukset**.

    ![Kertakirjautumisen määrittäminen][22]

1. Valitse **todennusasetukset**.

1. Suorita **Todennusasetukset** -sivulla seuraavat vaiheet

    ![Kertakirjautumisen määrittäminen][23]

    a. Valitse **WSFederation** **SSO tyyppi**.

    b. Lataa tiedostosi ladatut Azure metatietojen, valitsemalla **Selaa**, etsi tiedosto, Lopeta valitse sitten **Lataa metatiedot**.

    c-näppäinyhdistelmää. Kirjoita **sähköpostiosoite** -tekstiruutuun **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.

    d. Kirjoita **Ensimmäinen nimi** -tekstiruutuun **tyyppi http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.

    e. Kirjoita **Sukunimi** -tekstiruutuun **tyyppi http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**

    f. **Toimintorivi**Valitse **Tallenna**.







6. Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Seuraava**. 

    ![Mikä on Azure AD Connect][10]

7. Valitse **Sign Vahvista** -sivulla **Valmis**.  

    ![Mikä on Azure AD Connect][11]




### <a name="creating-an-azure-ad-test-user"></a>Azure AD-testikäyttäjän luominen
Tässä osassa tavoitteena on luotava testikäyttäjän kutsutaan Britta Simon Azure perinteinen portaalissa.  
Valitse käyttäjät-luettelosta **Britta Simon**.

![Luo Azure AD-käyttäjä][20]

**Voit luoda testikäyttäjän Azure AD-toimimalla seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen portal**, valitse vasemmassa siirtymisruudussa.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-quickhelp-tutorial/create_aaduser_02.png) 

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Ylä-valikossa käyttäjäluettelon, näkyviin napsauttamalla **käyttäjät**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-quickhelp-tutorial/create_aaduser_03.png) 
 
4. Avaa **Lisää käyttäjä** -valintaikkunan alareunassa työkalurivillä, valitse **Lisää käyttäjä**. 

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-quickhelp-tutorial/create_aaduser_04.png) 

5. Tee valintaikkunassa **Kerro kyseisen käyttäjän tietoja** -sivulla seuraavasti: 
 
    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-quickhelp-tutorial/create_aaduser_05.png) 

    a. Käyttäjänä tyyppi Valitse uusi käyttäjä organisaatiossa.

    b. Kirjoita käyttäjänimi- **tekstiruutuun** **BrittaSimon**.

    c-näppäinyhdistelmää. Valitse **Seuraava**.

6.  Suorita **Käyttäjäprofiili** -sivulla seuraavat toimet: 

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-quickhelp-tutorial/create_aaduser_06.png) 
 
    a. Kirjoita **Ensimmäinen nimi** -tekstiruutuun **Britta**.  

    b. Kirjoita **Sukunimi** -tekstiruutuun tyyppi- **Simon**.

    c-näppäinyhdistelmää. Kirjoita **Näyttönimi** -tekstiruutuun **Britta Simon**.

    d. Valitse **rooli** -luettelosta **käyttäjä**.
    e. Valitse **Seuraava**.

7. Valitse **Hae tilapäinen salasana** -valintaikkunan sivulla **Luo**.

![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-quickhelp-tutorial/create_aaduser_07.png) 
 
8. **Hae tilapäinen salasana** -valintaikkunan sivulla toimimalla seuraavasti:

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-quickhelp-tutorial/create_aaduser_08.png) 
  
    a. Kirjoita **Uusi salasana**arvo muistiin.

    b. Valitse **Valmis**.   

  
 
### <a name="creating-a-quickhelp-test-user"></a>QuickHelp-testikäyttäjän luominen

Tässä osassa tavoitteena on nimeltään Britta Simon QuickHelp käyttäjän luominen.
Kertakirjautumisen toimimaan Azure AD on tiedettävä vastaavaan käyttäjän QuickHelp, Azure AD-käyttäjälle. Toisin sanoen suhteen Azure AD-käyttäjä ja Aiheeseen liittyvät QuickHelp-linkki on vahvistettava.

QuickHelp tukee vain perustuvan valmistelu. Tämä tarkoittaa sitä, jos määrittäminen pakolliseksi, käyttäjätili luodaan automaattisesti QuickHelp ja tili on linkitetty Azure AD-tilille.

Ei ole toimi puolestasi sisältö.


### <a name="assigning-the-azure-ad-test-user"></a>Azure AD-testikäyttäjän määrittäminen

Tässä osassa tavoitteena on Britta Simon käyttämään Azure kertakirjautumisen hänen käyttöoikeuden myöntämisestä QuickHelp ottaminen käyttöön.

![Määrittää käyttäjälle][200] 

**Jos haluat määrittää Britta Simon QuickHelp, toimi seuraavasti:**

1. Azure perinteinen-portaalissa Avaa sovellukset-näkymässä kansio-näkymässä, valitse **sovellukset** yläreunan-valikossa.

    ![Määrittää käyttäjälle][201] 

2. Valitse sovellukset-luettelosta **QuickHelp**.

    ![Määrittää käyttäjälle][202] 

1. Valitse ylä-valikossa Valitse **käyttäjiä**.

    ![Määrittää käyttäjälle][203] 

1. Valitse käyttäjät-luettelosta **Britta Simon**.

2. Napsauta työkalurivin alaosassa **määrittäminen**.

    ![Määrittää käyttäjälle][205]



### <a name="testing-single-sign-on"></a>Kertakirjautumisen testaaminen

Tässä osassa tavoitteena on Testaa Azure AD yksittäisen Sign-asetusten käyttäminen Access-paneeli.  
Access-ruudussa QuickHelp-ruutua napsauttaessasi voit olisi Hae automaattisesti kirjautunut sisään QuickHelp-sovellukseen.


## <a name="additional-resources"></a>Lisäresursseja

* [Luettelo siitä, miten voit integroida SaaS sovellusten Azure Active Directory-opetusohjelmat](active-directory-saas-tutorial-list.md)
* [Mikä on sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_01.png
[500]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_14.png


[6]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_05.png
[7]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_02.png
[8]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_03.png
[9]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_04.png
[10]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_100.png
[21]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_05.png
[22]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_06.png
[23]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_07.png
[24]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_08.png
[25]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_09.png
[26]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_10.png
[27]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_11.png
[28]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_12.png

[200]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_quickhelp_13.png
[203]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-quickhelp-tutorial/tutorial_general_205.png


[400]: ./media/active-directory-saas-QuickHelp-tutorial/tutorial_QuickHelp_400.png
[401]: ./media/active-directory-saas-QuickHelp-tutorial/tutorial_QuickHelp_401.png
[402]: ./media/active-directory-saas-QuickHelp-tutorial/tutorial_QuickHelp_402.png




