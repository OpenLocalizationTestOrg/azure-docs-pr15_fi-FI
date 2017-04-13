<properties
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi SanSan | Microsoft Azure"
    description="Opettele määrittämään kertakirjautumisen Azure Active Directory- ja SanSan välillä."
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


# <a name="tutorial-azure-active-directory-integration-with-sansan"></a>Opetusohjelma: SanSan Azure Active Directory-integrointi

Tässä opetusohjelmassa kerrotaan, miten voit integroida SanSan Azure Active Directory (Azure AD).

Azure AD-integraation SanSan avulla voit seuraavat edut:

- Voit hallita Azure AD, jolla on pääsy SanSan
- Voit ottaa käyttöön automaattisesti Hae kirjautunut käyttöön SanSan (kertakirjautumisen) niiden Azure AD-tilien käyttäjille
- Voit hallita asiakkaiden yhdessä keskitetyssä sijainnissa - Azure perinteinen portal

Jos haluat tietää tarkempia tietoja SaaS sovelluksen integrointi Azure AD-kohdassa [mitä sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Edellytykset

Määrittää Azure AD-integroinnin SanSan, tarvitset seuraavat:

- Azure AD-tilaus
- SanSan single-merkki käytössä-tilauksessa


> [AZURE.NOTE] Voit esikatsella vaiheet Tässä opetusohjelmassa, Emme suosittele käyttämällä tuotantoympäristössä.


Voit esikatsella vaiheet Tässä opetusohjelmassa-noudattamalla näitä suosituksia:

- Älä käytä tuotantoympäristössä, ellei se on tarpeen.
- Jos sinulla ei ole kokeiluversion Azure AD-ympäristössä, voit hankkia yksi kuukausi kokeiluversion [tähän](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Skenaarion kuvaus
Tässä opetusohjelmassa testaat Microsoft Microsoft Azure AD kertakirjautumisen testiympäristössä. Tässä opetusohjelmassa kuvatut skenaarion kuuluu kaksi tärkeimmät rakenneosien:

1. SanSan lisääminen valikoimasta
2. Määrittäminen ja testaus Microsoft Azure AD Single Sign-On


## <a name="adding-sansan-from-the-gallery"></a>SanSan lisääminen valikoimasta
Voit määrittää SanSan integroida Azure AD-haluat lisätä SanSan valikoimasta hallitun SaaS sovellusluettelo.

**Jos haluat lisätä SanSan valikoimasta, toimi seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen portal**, valitse vasemmassa siirtymisruudussa. 

    ![Active Directory][1]

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset][2]

4. Valitse sivun alareunassa **Lisää** .

    ![Sovellukset][3]

5. Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Sovellukset][4]

6. Kirjoita hakuruutuun **SanSan**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-sansan-tutorial/tutorial_sansan_01.png)

7. Valitse tulosruudussa **SanSan**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-sansan-tutorial/tutorial_sansan_06.png)

##  <a name="configuring-and-testing-microsoft-azure-ad-single-sign-on"></a>Määrittäminen ja testaus Microsoft Azure AD Single Sign-On
Tässä osassa Määritä ja testaa Microsoft Azure AD kertakirjautumisen kanssa SanSan perusteella testikäyttäjän nimeltä "Britta Simon".

Kertakirjautumisen toimimaan Azure AD on tiedettävä käyttäjälle on SanSan vastaavaan käyttäjän Azure AD. Toisin sanoen suhteen Azure AD-käyttäjä ja Aiheeseen liittyvät SanSan-linkki on vahvistettava.
Linkki-yhteys on muodostettu määrittämällä **käyttäjänimi** arvo arvoksi SanSan **käyttäjänimi** Azure AD.

Määrittäminen ja testaaminen Microsoft Azure AD kertakirjautumisen SanSan kanssa, sinun on tehtävä seuraavat hakukyselyn:

1. **[Asetusten määrittäminen Microsoft Azure AD Single Sign](#configuring-azure-ad-single-single-sign-on)** - käyttäjät voivat käyttää tätä toimintoa.
2. **[Käyttäjän testata luominen Azure AD](#creating-an-azure-ad-test-user)** - Testaa Microsoft Azure AD kertakirjautumisen Britta Simon kanssa.
4. **[Käyttäjän luominen SanSan testata](#creating-an-sansan-test-user)** - on tapahtumista Britta Simon SanSan kyseistä henkilöä hän Azure AD esitys.
5. **[Käyttäjän testata määrittäminen Azure AD](#assigning-the-azure-ad-test-user)** - Britta Simon käyttämään Microsoft Azure AD kertakirjautumisen käyttöön.
5. **[Testaus Single Sign](#testing-single-sign-on)** - voit tarkistaa, toimiiko määritykset.

### <a name="configuring-microsoft-azure-ad-single-sign-on"></a>Microsoft Azure AD-Single Sign-On määrittäminen

Tässä osassa Ota käyttöön Microsoft Azure AD kertakirjautumisen perinteinen portaalissa ja määritä kertakirjautumisen SanSan-sovelluksessa.


**Määrittää Microsoft Azure AD kertakirjautumisen SanSan, toimimalla seuraavasti:**


1. Valitse Azure perinteinen portaalissa integrointi **SanSan** sovellus-sivulla Määritä kertakirjautumisen määrittäminen Single Sign-valintaikkunan avaaminen.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-sansan-tutorial/tutorial_general_05.png) 

2. **Miten käyttäjät voivat kirjautua SanSan haluat käyttää** sivulla Valitse **Microsoft Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.
    
    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-sansan-tutorial/tutorial_sansan_03.png) 

3. Valitse **Määritä sovelluksen asetukset** -sivulla toimimalla seuraavasti:

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-sansan-tutorial/tutorial_sansan_04.png) 

    a. Kirjoita **Merkki-URL** -tekstiruutuun URL-Osoitetta seuraavaa kaavaa:

  	| Ympäristön             | URL-OSOITE |
  	| :--                     | :-- |
  	| PC-web                  | `https://ap.sansan.com/v/saml2/<company name>/acs`|
  	| Alkuperäisen mobiilisovelluksessa       | `https://internal.api.sansan.com/saml2/<company name>/acs` |
  	| Mobiiliselaimen asetukset | `https://ap.sansan.com/s/saml2/<company name>/acs` |


    b. Kirjoita **tunnus** -tekstiruutuun URL-Osoitetta seuraavaa kaavaa: 

  	| Ympäristön             | URL-OSOITE |
  	| :--                     | :-- |
  	| PC-web                  | `https://ap.sansan.com/v/saml2/<company name>`|
  	| Alkuperäisen mobiilisovelluksessa       | `https://internal.api.sansan.com/saml2/<company name>` |
  	| Mobiiliselaimen asetukset | `https://ap.sansan.com/s/saml2/<company name>` |


    c-näppäinyhdistelmää. Valitse **Seuraava**.

4. Valitse **Määritä kertakirjautumisen osoitteessa SanSan** -sivulla toimimalla seuraavasti:

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-sansan-tutorial/tutorial_sansan_05.png) 

    a. Valitse **Lataa**ja tallenna sitten tiedosto tietokoneesta.

    b. Valitse **Seuraava**.


5.  Saat määritetty sovelluksen SSO-yhteyshenkilön SanSan tukihenkilöstön ja ne auttavat SSO määrittämiseen. Anna niiden seuraavat tiedot:

    • Ladatut **varmenne**

    • **Tunnistetietojen Toimittajan tunnus**

    • **SAML SSO URL-osoite**

    • **Yksittäisen Kirjaudu ulos palvelun URL-osoite**

> [AZURE.NOTE] Mobiilisovelluksen ja mobiili-ja PC-web-selaimessa toimivat myös tietokoneen selaimessa-asetus. 


6. Perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Seuraava**.
    
    ![Azure AD-Single Sign-On][10]

7. Valitse **Sign Vahvista** -sivulla **Valmis**.  
    
    ![Azure AD-Single Sign-On][11]



### <a name="creating-an-azure-ad-test-user"></a>Azure AD-testikäyttäjän luominen

Tässä osassa voit luoda testikäyttäjän kutsutaan Britta Simon perinteinen portaalissa.
Valitse käyttäjät-luettelosta **Britta Simon**.

![Luo Azure AD-käyttäjä][20]

**Voit luoda testikäyttäjän Azure AD-toimimalla seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen portal**, valitse vasemmassa siirtymisruudussa.
    
    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-sansan-tutorial/create_aaduser_09.png) 

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Ylä-valikossa käyttäjäluettelon, näkyviin napsauttamalla **käyttäjät**.
    
    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-sansan-tutorial/create_aaduser_03.png) 

4. Avaa **Lisää käyttäjä** -valintaikkunan alareunassa työkalurivillä, valitse **Lisää käyttäjä**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-sansan-tutorial/create_aaduser_04.png) 

5. Tee valintaikkunassa **Kerro kyseisen käyttäjän tietoja** -sivulla seuraavasti:
 
    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-sansan-tutorial/create_aaduser_05.png) 

    a. Käyttäjänä tyyppi Valitse uusi käyttäjä organisaatiossa.

    b. Kirjoita käyttäjänimi- **tekstiruutuun** **BrittaSimon**.

    c-näppäinyhdistelmää. Valitse **Seuraava**.

6.  Suorita **Käyttäjäprofiili** -sivulla seuraavat toimet:

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-sansan-tutorial/create_aaduser_06.png) 

    a. Kirjoita **Ensimmäinen nimi** -tekstiruutuun **Britta**.  

    b. Kirjoita **Sukunimi** -tekstiruutuun tyyppi- **Simon**.

    c-näppäinyhdistelmää. Kirjoita **Näyttönimi** -tekstiruutuun **Britta Simon**.

    d. Valitse **rooli** -luettelosta **käyttäjä**.

    e. Valitse **Seuraava**.

7. Valitse **Hae tilapäinen salasana** -valintaikkunan sivulla **Luo**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-sansan-tutorial/create_aaduser_07.png) 

8. **Hae tilapäinen salasana** -valintaikkunan sivulla toimimalla seuraavasti:

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-sansan-tutorial/create_aaduser_08.png) 

    a. Kirjoita **Uusi salasana**arvo muistiin.

    b. Valitse **Valmis**.   



### <a name="creating-an-sansan-test-user"></a>SanSan-testikäyttäjän luominen

Tässä osassa nimeltä Britta Simon SanSan käyttäjän luominen. SanSan-sovellus on käyttäjän valmisteltu ennen kuin teet SSO-sovelluksessa. 


> [AZURE.NOTE] Jos haluat luoda käyttäjän manuaalisesti tai erän käyttäjien käytettävissä, sinun on SanSan-tukeen.


### <a name="assigning-the-azure-ad-test-user"></a>Azure AD-testikäyttäjän määrittäminen

Tässä osassa voit ottaa käyttöön Britta Simon käyttämään Azure kertakirjautumisen hänen käyttöoikeuksien myöntämistä SanSan.

![Määrittää käyttäjälle][200] 

**Jos haluat määrittää Britta Simon SanSan, toimi seuraavasti:**

1. Perinteinen-portaalissa Avaa sovellukset-näkymässä kansio-näkymässä, valitse **sovellukset** yläreunan-valikossa.

    ![Määrittää käyttäjälle][201] 

2. Valitse sovellukset-luettelosta **SanSan**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-sansan-tutorial/tutorial_sansan_50.png) 

1. Valitse ylä-valikossa Valitse **käyttäjiä**.

    ![Määrittää käyttäjälle][203] 

1. Valitse käyttäjät-luettelosta **Britta Simon**.

2. Napsauta työkalurivin alaosassa **määrittäminen**.

    ![Määrittää käyttäjälle][205]


### <a name="testing-single-sign-on"></a>Kertakirjautumisen testaaminen

Tässä osassa voit testata Microsoft Azure AD kertakirjautumisen asetusten käyttäminen Access-paneeli.
Access-ruudussa SanSan-ruutua napsauttaessasi sinun pitäisi saada automaattisesti kirjautunut-on SanSan sovelluksen.


## <a name="additional-resources"></a>Lisäresursseja

* [Luettelo siitä, miten voit integroida SaaS sovellusten Azure Active Directory-opetusohjelmat](active-directory-saas-tutorial-list.md)
* [Mikä on sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-sansan-tutorial/tutorial_general_205.png
