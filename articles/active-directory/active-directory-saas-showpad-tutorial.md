<properties
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi Showpad | Microsoft Azure"
    description="Opettele määrittämään kertakirjautumisen Azure Active Directory- ja Showpad välillä."
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


# <a name="tutorial-azure-active-directory-integration-with-showpad"></a>Opetusohjelma: Showpad Azure Active Directory-integrointi

Tässä opetusohjelmassa tavoitteena on, kuinka voit integroida Showpad Azure Active Directory (Azure AD).

Azure AD-integraation Showpad avulla voit seuraavat edut:

- Voit hallita Azure AD, jolla on pääsy Showpad
- Voit ottaa käyttöön automaattisesti Hae kirjautunut käyttöön Showpad (kertakirjautumisen) niiden Azure AD-tilien käyttäjille
- Voit hallita asiakkaiden yhdessä keskitetyssä sijainnissa - Azure Active Directory-portaalissa

Jos haluat tietää tarkempia tietoja SaaS sovelluksen integrointi Azure AD-kohdassa [mitä sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Edellytykset

Määrittää Azure AD-integroinnin Showpad, tarvitset seuraavat:

- Azure AD-tilaus
- Showpad Tilauksen


> [AZURE.NOTE] Voit esikatsella vaiheet Tässä opetusohjelmassa, Emme suosittele käyttämällä tuotantoympäristössä.


Voit esikatsella vaiheet Tässä opetusohjelmassa-noudattamalla näitä suosituksia:

- Älä käytä tuotantoympäristössä, ellei se on tarpeen.
- Jos sinulla ei ole kokeiluversion Azure AD-ympäristössä, voit hankkia yksi kuukausi kokeiluversion [tähän](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Skenaarion kuvaus
Tässä opetusohjelmassa tavoitteena on, jotta voit testata Azure AD kertakirjautumisen testiympäristössä. 

Tässä opetusohjelmassa kuvatut skenaarion kuuluu kaksi tärkeimmät rakenneosien:

1. Showpad lisääminen valikoimasta
2. Yksittäinen määrittäminen ja testaus Azure AD-on


## <a name="adding-showpad-from-the-gallery"></a>Showpad lisääminen valikoimasta
Voit määrittää Showpad integroida Azure AD-haluat lisätä Showpad valikoimasta hallitun SaaS sovellusluettelo.

**Jos haluat lisätä Showpad valikoimasta, toimi seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen portal**, valitse vasemmassa siirtymisruudussa. 

    ![Sovellukset][1]

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset][2]

4. Valitse sivun alareunassa **Lisää** .

    ![Sovellukset][3]

5. Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.
 
    ![Sovellukset][4]

6. Kirjoita hakuruutuun **Showpad**.

    ![Sovellukset](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_01.png)

7. Valitse tulosruudussa **Showpad**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![Sovellukset](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yksittäinen määrittäminen ja testaus Azure AD-on
Tässä osassa tavoitteena on noudattamalla voit määrittää ja testaa Azure AD kertakirjautumisen Showpad nimeltä "Britta Simon" testikäyttäjän perusteella.

Kertakirjautumisen toimimaan Azure AD on tiedettävä vastaavaan käyttäjän Showpad, Azure AD-käyttäjälle. Toisin sanoen suhteen Azure AD-käyttäjä ja Aiheeseen liittyvät Showpad-linkki on vahvistettava.

Linkki-yhteys on muodostettu määrittämällä **käyttäjänimi** arvo arvoksi Showpad **käyttäjänimi** Azure AD.

Määrittäminen ja testaaminen Azure AD kertakirjautumisen Showpad kanssa, sinun on tehtävä seuraavat hakukyselyn:

1. **[Azure AD määrittäminen Single Sign](#configuring-azure-ad-single-single-sign-on)** - käyttäjät voivat käyttää tätä toimintoa.
2. **[Käyttäjän testata luominen Azure AD](#creating-an-azure-ad-test-user)** - Testaa Azure AD kertakirjautumisen Britta Simon kanssa.
3. **[Luomisesta Showpad testata käyttäjän](#creating-a-showpad-test-user)** - tapahtumista Britta Simon ovat Showpad, jotka on linkitetty hän Azure AD esitys.
4. **[Käyttäjän testata määrittäminen Azure AD](#assigning-the-azure-ad-test-user)** - Britta Simon käyttämään Azure AD kertakirjautumisen käyttöön.
5. **[Testaus Single Sign](#testing-single-sign-on)** - voit tarkistaa, toimiiko määritykset.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD-Single Sign-On määrittäminen

Tässä osassa tavoitteena on Azure AD kertakirjautumisen Azure perinteinen portaalissa ottaminen käyttöön ja määrittää kertakirjautumisen Showpad-sovelluksessa.



**Määrittää Azure AD kertakirjautumisen Showpad, toimimalla seuraavasti:**

1. Valitse Azure perinteinen portaalissa integrointi **Showpad** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen Single Sign** -valintaikkunan avaaminen.

    ![Kertakirjautumisen määrittäminen][6] 

2. **Miten käyttäjät voivat kirjautua Showpad haluat käyttää** sivulla Valitse **Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_03.png)

3. Valitse **Määritä sovelluksen asetukset** -sivulla suorittamalla seuraavat vaiheet ja valitse sitten **Seuraava**:

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_04.png) 


    a. Kirjoita **Merkki-URL** -tekstiruutuun URL-osoite käyttämä Sign käyttämällä seuraavaa kaavaa Showpad sovelluksen käyttäjille:`https://<company name>.showpad.biz/login`

    b. Kirjoita **tunnus** -tekstiruutuun URL-osoite, käyttämällä seuraavaa kaavaa:`https://<company name>.showpad.biz`

    c-näppäinyhdistelmää. Valitse **Seuraava**


4. Valitse **Määritä kertakirjautumisen osoitteessa Showpad** -sivulla suorittamalla seuraavat vaiheet ja valitse sitten **Seuraava**:

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_05.png)

    a. Valitse **Lataa metatiedot**ja tallenna sitten tiedosto tietokoneesta.

    b. Valitse **Seuraava**.


5. Kertakirjauksen Showpad asiakasympäristöön järjestelmänvalvojana.

6. Valitse ylä-valikossa **asetukset**.

    ![Sovelluksen reunassa kertakirjautumisen määrittäminen](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_001.png) 

7. Siirry "**Kertakirjautumisen**" ja valitse "**käyttöön**".
 
    ![Sovelluksen reunassa kertakirjautumisen määrittäminen](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_002.png)

8. Valitse **Lisää palvelu SAML 2.0** -valintaikkunassa seuraavasti:

    ![Sovelluksen reunassa kertakirjautumisen määrittäminen](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_003.png) 

    a. Kirjoita **nimi** -tekstiruutuun tunnus tarjoajan nimi (esimerkiksi: yrityksen nimi).

    b. **Metatietojen lähteen**Valitse **XML**.

    c-näppäinyhdistelmää. Ladatun metatietojen XML-tiedoston sisällön kopioiminen ja liittäminen **Metatietojen XML** -tekstiruutuun.

    d. Valitse **Automaattinen säännöstä tilit uusille käyttäjille, kun he kirjautuvat**.

    e. Valitse **Lähetä**.


10. Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Seuraava**.

    ![Azure AD-Single Sign-On][10]


11. Valitse **Sign Vahvista** -sivulla **Valmis**.  
  
    ![Azure AD-Single Sign-On][11]






### <a name="creating-an-azure-ad-test-user"></a>Azure AD-testikäyttäjän luominen
Tässä osassa tavoitteena on luotava testikäyttäjän kutsutaan Britta Simon Azure perinteinen portaalissa.

![Luo Azure AD-käyttäjä][20]

**Voit luoda Showpad testikäyttäjän Azure AD-toimimalla seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen portal**, valitse vasemmassa siirtymisruudussa.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-showpad-tutorial/create_aaduser_09.png) 

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Ylä-valikossa käyttäjäluettelon, näkyviin napsauttamalla **käyttäjät**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-showpad-tutorial/create_aaduser_03.png) 

4. Avaa **Lisää käyttäjä** -valintaikkunan alareunassa työkalurivillä, valitse **Lisää käyttäjä**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-showpad-tutorial/create_aaduser_04.png) 

5. Tee valintaikkunassa **Kerro kyseisen käyttäjän tietoja** -sivulla seuraavasti:

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-showpad-tutorial/create_aaduser_05.png) 

    a. Kirjoita **Käyttäjänimi** -tekstiruutuun **BrittaSimon**.

    b. Valitse **Seuraava**.

6.  Suorita **Käyttäjäprofiili** -sivulla seuraavat toimet:

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-showpad-tutorial/create_aaduser_06.png) 

    a. Kirjoita **Ensimmäinen nimi** -tekstiruutuun **Britta**.  

    b. Kirjoita **Sukunimi** -tekstiruutuun tyyppi- **Simon**.

    c-näppäinyhdistelmää. Kirjoita **Näyttönimi** -tekstiruutuun **Britta Simon**.

    d. Valitse **rooli**, **käyttäjä**.

    e. Valitse **Seuraava**.

7. Valitse **Hae tilapäinen salasana** -valintaikkunan sivulla **Luo**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-showpad-tutorial/create_aaduser_07.png)

8. **Hae tilapäinen salasana** -valintaikkunan sivulla toimimalla seuraavasti:

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-showpad-tutorial/create_aaduser_08.png) 

    a. Kirjoita **Uusi salasana**arvo muistiin.

    b. Valitse **Valmis**.


### <a name="creating-a-showpad-test-user"></a>Showpad testikäyttäjän luominen

Tässä osassa tavoitteena on nimeltään Britta Simon Showpad käyttäjän luominen. 

Showpad tukee vain perustuvan valmistelu. Olet ottanut valmistelu **[Määrittäminen Azure AD kertakirjautumisen](#configuring-azure-ad-single-single-sign-on)**. 

Ei ole toimi puolestasi sisältö. 




### <a name="assigning-the-azure-ad-test-user"></a>Azure AD-testikäyttäjän määrittäminen

Tässä osassa tavoitteena on Britta Simon käyttämään Azure kertakirjautumisen hänen käyttöoikeuden myöntämisestä Showpad ottaminen käyttöön.

![Määrittää käyttäjälle][200]

**Jos haluat määrittää Britta Simon Showpad, toimi seuraavasti:**

1. Azure-perinteinen portaalissa-valikon päälle Valitse **sovellukset**.

    ![Määrittää käyttäjälle][201] 

2. Valitse sovellukset-luettelosta **Showpad**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-showpad-tutorial/tutorial_showpad_50.png) 

1. Valitse ylä-valikossa Valitse **käyttäjiä**.

    ![Määrittää käyttäjälle][203]

1. Valitse käyttäjät-luettelosta **Britta Simon**.

2. Napsauta työkalurivin alaosassa **määrittäminen**.

    ![Määrittää käyttäjälle][205]




### <a name="testing-single-sign-on"></a>Kertakirjautumisen testaaminen

Tässä osassa tavoitteena on Testaa Azure AD yksittäisen Sign-asetusten käyttäminen Access-paneeli.

Access-ruudussa **Showpad** -ruutua napsauttaessasi sinun pitäisi saada automaattisesti kirjautunut-on Showpad sovelluksen.


## <a name="additional-resources"></a>Lisäresursseja

* [Luettelo siitä, miten voit integroida SaaS sovellusten Azure Active Directory-opetusohjelmat](active-directory-saas-tutorial-list.md)
* [Mikä on sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-showpad-tutorial/tutorial_general_205.png
