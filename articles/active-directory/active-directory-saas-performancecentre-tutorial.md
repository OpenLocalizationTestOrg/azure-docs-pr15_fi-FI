<properties
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi PerformanceCentre | Microsoft Azure"
    description="Opettele määrittämään kertakirjautumisen Azure Active Directory- ja PerformanceCentre välillä."
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
    ms.date="09/26/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-performancecentre"></a>Opetusohjelma: PerformanceCentre Azure Active Directory-integrointi

Tässä opetusohjelmassa tavoitteena on, kuinka voit integroida PerformanceCentre Azure Active Directory (Azure AD).  
Azure AD-integraation PerformanceCentre avulla voit seuraavat edut: 

- Voit hallita Azure AD, jolla on pääsy PerformanceCentre 
- Voit ottaa käyttöön automaattisesti Hae kirjautunut käyttöön PerformanceCentre (kertakirjautumisen) niiden Azure AD-tilien käyttäjille
- Voit hallita asiakkaiden keskitetyn paikkaan – perinteinen Azure Active Directory-portaalissa

Jos haluat tietää tarkempia tietoja SaaS sovelluksen integrointi Azure AD-kohdassa [mitä sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Edellytykset 

Määrittää Azure AD-integroinnin PerformanceCentre, tarvitset seuraavat:

- Azure AD-tilaus
- PerformanceCentre single-merkki käytössä-tilauksessa


> [AZURE.NOTE] Voit esikatsella vaiheet Tässä opetusohjelmassa, Emme suosittele käyttämällä tuotantoympäristössä.


Voit esikatsella vaiheet Tässä opetusohjelmassa-noudattamalla näitä suosituksia:

- Älä käytä tuotantoympäristössä, ellei se on tarpeen.
- Jos sinulla ei ole kokeiluversion Azure AD-ympäristössä, voit hankkia yksi kuukausi kokeiluversion [tähän](https://azure.microsoft.com/pricing/free-trial/). 

 
## <a name="scenario-description"></a>Skenaarion kuvaus
Tässä opetusohjelmassa tavoitteena on, jotta voit testata Azure AD kertakirjautumisen testiympäristössä.  
Tässä opetusohjelmassa kuvatut skenaarion koostuu kolme tärkeimmät rakenneosien:

1. PerformanceCentre lisääminen valikoimasta 
2. Yksittäinen määrittäminen ja testaus Azure AD-on


## <a name="adding-performancecentre-from-the-gallery"></a>PerformanceCentre lisääminen valikoimasta
Voit määrittää PerformanceCentre integroida Azure AD-haluat lisätä PerformanceCentre valikoimasta hallitun SaaS sovellusluettelo.

**Jos haluat lisätä PerformanceCentre valikoimasta, toimi seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen portal**, valitse vasemmassa siirtymisruudussa. 

    ![Active Directory][1]

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset][2]

4. Valitse sivun alareunassa **Lisää** .

    ![Sovellukset][3]

5. Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Sovellukset][4]

6. Kirjoita hakuruutuun **PerformanceCentre**.
 
    ![Sovellukset][5]

7. Valitse tulosruudussa **PerformanceCentre**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![Sovellukset][500]


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yksittäinen määrittäminen ja testaus Azure AD-on
Tässä osassa tavoitteena on noudattamalla voit määrittää ja testaa Azure AD kertakirjautumisen PerformanceCentre nimeltä "Britta Simon" testikäyttäjän perusteella.

Kertakirjautumisen toimimaan Azure AD on tiedettävä vastaavaan käyttäjän PerformanceCentre, Azure AD-käyttäjälle. Toisin sanoen suhteen Azure AD-käyttäjä ja Aiheeseen liittyvät PerformanceCentre-linkki on vahvistettava.  
Linkki-yhteys on muodostettu määrittämällä **käyttäjänimi** arvo arvoksi PerformanceCentre **käyttäjänimi** Azure AD.
 
Määrittäminen ja testaaminen Azure AD kertakirjautumisen PerformanceCentre kanssa, sinun on tehtävä seuraavat hakukyselyn:

1. **[Azure AD määrittäminen Single Sign](#configuring-azure-ad-single-single-sign-on)** - käyttäjät voivat käyttää tätä toimintoa.
2. **[Käyttäjän testata luominen Azure AD](#creating-an-azure-ad-test-user)** - Testaa Azure AD kertakirjautumisen Britta Simon kanssa.
4. **[Luomisesta PerformanceCentre testata käyttäjän](#creating-a-halogen-software-test-user)** - tapahtumista Britta Simon ovat PerformanceCentre, jotka on linkitetty hän Azure AD esitys.
5. **[Käyttäjän testata määrittäminen Azure AD](#assigning-the-azure-ad-test-user)** - Britta Simon käyttämään Azure AD kertakirjautumisen käyttöön.
5. **[Testaus Single Sign](#testing-single-sign-on)** - voit tarkistaa, toimiiko määritykset.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD-Single Sign-On määrittäminen

Tässä osassa tavoitteena on Azure AD kertakirjautumisen perinteinen Azure AD-portaalissa ottaminen käyttöön ja määrittää kertakirjautumisen PerformanceCentre-sovelluksessa.

**Määrittää Azure AD kertakirjautumisen PerformanceCentre, toimimalla seuraavasti:**

1. Azure AD perinteinen-portaalissa, integrointi **PerformanceCentre** sovellus-sivulla valitsemalla **Määritä kertakirjautumisen** **Määrittäminen Single Sign** -valintaikkunan avaaminen.

    ![Kertakirjautumisen määrittäminen][6] 

2. **Miten käyttäjät voivat kirjautua PerformanceCentre haluat käyttää** sivulla Valitse **Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Azure AD-Single Sign-On][7] 

3. Valitse **Määritä sovelluksen asetukset** -sivulla toimimalla seuraavasti:

    ![Azure AD-Single Sign-On][8] 
 
     a. Kirjoita **Merkki-URL** -tekstiruutuun Sign PerformanceCentre sivustoon käyttäjille käyttämä URL-osoite (esimerkiksi: *http://companyname.performancecentre.com/saml/SSO*).
 
     b. Valitse **Seuraava**.
 
4. Valitse **Määritä kertakirjautumisen osoitteessa PerformanceCentre** -sivulla toimimalla seuraavasti:

    ![Azure AD-Single Sign-On][9] 

    a. Valitse **Lataa metatiedot**ja tallenna sitten tiedosto tietokoneesta.



1. Kertakirjauksen **PerformanceCentre** yrityksen sivustoon järjestelmänvalvojana.

2. Valitse vasemmassa reunassa-välilehdessä valitsemalla **Määritä**.

    ![Azure AD-Single Sign-On][10]

2. Valitse vasemmassa reunassa-välilehdessä **Muut**ja valitse sitten **Kertakirjautuminen**.

    ![Azure AD-Single Sign-On][11]

2. Valitse **SAML**- **protokollaa**.

    ![Azure AD-Single Sign-On][12]

2. Avaa ladatut metatietojen tiedosto Muistiossa, kopioida sisältöä, liittämällä sen **Tunnistetietojen toimittaja metatietojen** tekstiruutu ja valitse sitten **Tallenna**.

    ![Azure AD-Single Sign-On][13]

2. Varmista, että **Kohteen Base URL-osoite** ja **Kohteen tunnus URL-Osoitteen** arvot ovat oikein.

    ![Azure AD-Single Sign-On][14]


6. Azure AD perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Seuraava**. 

    ![Azure AD-Single Sign-On][15]

7. Valitse **Sign Vahvista** -sivulla **Valmis**.  

    ![Azure AD-Single Sign-On][16]




### <a name="creating-an-azure-ad-test-user"></a>Azure AD-testikäyttäjän luominen
Tässä osassa tavoitteena on luotava testikäyttäjän kutsutaan Britta Simon Azure perinteinen portaalissa.  

![Luo Azure AD-käyttäjä][20]

**Voit luoda testikäyttäjän Azure AD-toimimalla seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen portal**, valitse vasemmassa siirtymisruudussa.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-performancecentre-tutorial/create_aaduser_09.png)  

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Ylä-valikossa käyttäjäluettelon, näkyviin napsauttamalla **käyttäjät**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-performancecentre-tutorial/create_aaduser_03.png) 
 
4. Avaa **Lisää käyttäjä** -valintaikkunan alareunassa työkalurivillä, valitse **Lisää käyttäjä**. 

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-performancecentre-tutorial/create_aaduser_04.png) 

5. Tee valintaikkunassa **Kerro kyseisen käyttäjän tietoja** -sivulla seuraavasti: 

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-performancecentre-tutorial/create_aaduser_05.png)  

    a. Käyttäjänä tyyppi Valitse uusi käyttäjä organisaatiossa.

    b. Kirjoita käyttäjänimi- **tekstiruutuun** **BrittaSimon**.

    c-näppäinyhdistelmää. Valitse **Seuraava**.

6.  Suorita **Käyttäjäprofiili** -sivulla seuraavat toimet: 

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-performancecentre-tutorial/create_aaduser_06.png) 
 
    a. Kirjoita **Ensimmäinen nimi** -tekstiruutuun **Britta**.  

    b. Kirjoita **Sukunimi** -tekstiruutuun tyyppi- **Simon**.

    c-näppäinyhdistelmää. Kirjoita **Näyttönimi** -tekstiruutuun **Britta Simon**.

    d. Valitse **rooli** -luettelosta **käyttäjä**.
    e. Valitse **Seuraava**.

7. Valitse **Hae tilapäinen salasana** -valintaikkunan sivulla **Luo**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-performancecentre-tutorial/create_aaduser_07.png) 
 
8. **Hae tilapäinen salasana** -valintaikkunan sivulla toimimalla seuraavasti:

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-performancecentre-tutorial/create_aaduser_08.png) 
  
    a. Kirjoita **Uusi salasana**arvo muistiin.

    b. Valitse **Valmis**.   

  
 
### <a name="creating-a-performancecentre-test-user"></a>PerformanceCentre testikäyttäjän luominen

Tässä osassa tavoitteena on nimeltään Britta Simon PerformanceCentre käyttäjän luominen.

**Luo kutsutaan Britta Simon PerformanceCentre käyttäjä, toimi seuraavasti:**

1. Kirjaudu PerformanceCentre yrityksesi sivuston järjestelmänvalvojana.

2. Valitse vasemmasta valikosta **Interrelate**ja valitse sitten **Luo osallistujan**.

    ![Käyttäjän luominen][400]

4. Valitse **Interrelate – Luo osallistuja** -valintaikkunassa seuraavasti:

    ![Käyttäjän luominen][401]

    a. Kirjoita pakolliset määritteet, Britta Simon liittyvät tekstiruutuja.
    
    > [AZURE.IMPORTANT] PerformanceCentre Britta henkilön käyttäjänimi määritteen on oltava sama kuin käyttäjänimi Azure AD.


    b. Valitse **Asiakkaan järjestelmänvalvojan** **roolin valitseminen**. 

    c-näppäinyhdistelmää. Valitse **Tallenna**.   


### <a name="assigning-the-azure-ad-test-user"></a>Azure AD-testikäyttäjän määrittäminen

Tässä osassa tavoitteena on Britta Simon käyttämään Azure kertakirjautumisen hänen käyttöoikeuden myöntämisestä PerformanceCentre ottaminen käyttöön.

![Määrittää käyttäjälle][200] 

**Jos haluat määrittää Britta Simon PerformanceCentre, toimi seuraavasti:**

1. Azure perinteinen-portaalissa Avaa sovellukset-näkymässä kansio-näkymässä, valitse **sovellukset** yläreunan-valikossa.

    ![Määrittää käyttäjälle][201]

2. Valitse sovellukset-luettelosta **PerformanceCentre**.

    ![Määrittää käyttäjälle][202]

1. Valitse ylä-valikossa Valitse **käyttäjiä**.

    ![Määrittää käyttäjälle][203]

1. Valitse käyttäjät-luettelosta **Britta Simon**.

2. Napsauta työkalurivin alaosassa **määrittäminen**.

    ![Määrittää käyttäjälle][205]



### <a name="testing-single-sign-on"></a>Kertakirjautumisen testaaminen

Tässä osassa tavoitteena on Testaa Azure AD yksittäisen Sign-asetusten käyttäminen Access-paneeli.  
Access-ruudussa PerformanceCentre-ruutua napsauttaessasi sinun pitäisi saada automaattisesti kirjautunut-on PerformanceCentre sovelluksen.


## <a name="additional-resources"></a>Lisäresursseja

* [Luettelo siitä, miten voit integroida SaaS sovellusten Azure Active Directory-opetusohjelmat](active-directory-saas-tutorial-list.md)
* [Mikä on sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_01.png
[500]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_02.png

[6]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_05.png
[7]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_03.png
[8]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_04.png
[9]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_05.png
[10]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_06.png
[11]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_07.png
[12]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_08.png
[13]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_09.png
[14]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_10.png
[15]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_06.png
[16]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_07.png


[20]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_50.png
[203]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_general_205.png


[400]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_11.png
[401]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_12.png
[402]: ./media/active-directory-saas-performancecentre-tutorial/tutorial_performancecentre_402.png



