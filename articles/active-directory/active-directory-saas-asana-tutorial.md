<properties
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi Asana | Microsoft Azure"
    description="Opettele määrittämään kertakirjautumisen Azure Active Directory- ja Asana välillä."
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


# <a name="tutorial-azure-active-directory-integration-with-asana"></a>Opetusohjelma: Asana Azure Active Directory-integrointi

Tässä opetusohjelmassa kerrotaan, miten voit integroida Asana Azure Active Directory (Azure AD).

Azure AD-integraation Asana avulla voit seuraavat edut:

- Voit hallita Azure AD, jolla on pääsy Asana
- Voit ottaa käyttöön automaattisesti Hae kirjautunut käyttöön Asana (kertakirjautumisen) niiden Azure AD-tilien käyttäjille
- Voit hallita asiakkaiden yhdessä keskitetyssä sijainnissa - Azure perinteinen portal

Jos haluat tietää tarkempia tietoja SaaS sovelluksen integrointi Azure AD-kohdassa [mitä sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Edellytykset

Määrittää Azure AD-integroinnin Asana, tarvitset seuraavat:

- Azure AD-tilaus
- **Asana** single-merkki käytössä-tilauksessa


> [AZURE.NOTE] Voit esikatsella vaiheet Tässä opetusohjelmassa, Emme suosittele käyttämällä tuotantoympäristössä.


Voit esikatsella vaiheet Tässä opetusohjelmassa-noudattamalla näitä suosituksia:

- Älä käytä tuotantoympäristössä, ellei se on tarpeen.
- Jos sinulla ei ole kokeiluversion Azure AD-ympäristössä, voit hankkia yksi kuukausi kokeiluversion [tähän](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Skenaarion kuvaus
Tässä opetusohjelmassa testaat Azure AD kertakirjautumisen testiympäristössä. Tässä opetusohjelmassa kuvatut skenaarion kuuluu kaksi tärkeimmät rakenneosien:

1. Asana lisääminen valikoimasta
2. Yksittäinen määrittäminen ja testaus Azure AD-on


## <a name="adding-asana-from-the-gallery"></a>Asana lisääminen valikoimasta
Voit määrittää Asana integroida Azure AD-haluat lisätä Asana valikoimasta hallitun SaaS sovellusluettelo.

**Jos haluat lisätä Asana valikoimasta, toimi seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen portal**, valitse vasemmassa siirtymisruudussa. 

    ![Active Directory][1]

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset][2]

4. Valitse sivun alareunassa **Lisää** .

    ![Sovellukset][3]

5. Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Sovellukset][4]

6. Kirjoita hakuruutuun **Asana**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-asana-tutorial/tutorial_asana_01.png)

7. Valitse tulosruudussa **Asana**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-asana-tutorial/tutorial_asana_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yksittäinen määrittäminen ja testaus Azure AD-on
Tässä osassa Määritä ja testaa Azure AD kertakirjautumisen Asana nimeltä "Britta Simon" testikäyttäjän perusteella.

Kertakirjautumisen toimimaan Azure AD on tiedettävä käyttäjälle on Asana vastaavaan käyttäjän Azure AD. Toisin sanoen suhteen Azure AD-käyttäjä ja Aiheeseen liittyvät Asana-linkki on vahvistettava.
Linkki-yhteys on muodostettu määrittämällä **käyttäjänimi** arvo arvoksi Asana **käyttäjänimi** Azure AD.

Määrittäminen ja testaaminen Azure AD kertakirjautumisen Asana kanssa, sinun on tehtävä seuraavat hakukyselyn:

1. **[Azure AD määrittäminen Single Sign](#configuring-azure-ad-single-single-sign-on)** - käyttäjät voivat käyttää tätä toimintoa.
2. **[Käyttäjän testata luominen Azure AD](#creating-an-azure-ad-test-user)** - Testaa Azure AD kertakirjautumisen Britta Simon kanssa.
4. **[Käyttäjän luominen Asana testata](#creating-an-Asana-test-user)** - on tapahtumista Britta Simon Asana kyseistä henkilöä hän Azure AD esitys.
5. **[Käyttäjän testata määrittäminen Azure AD](#assigning-the-azure-ad-test-user)** - Britta Simon käyttämään Azure AD kertakirjautumisen käyttöön.
5. **[Testaus Single Sign](#testing-single-sign-on)** - voit tarkistaa, toimiiko määritykset.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD määrittäminen yksittäinen Sign-On

Tässä osassa tavoitteena on Azure AD kertakirjautumisen Azure perinteinen portaalissa ottaminen käyttöön ja määrittää kertakirjautumisen Asana-sovelluksessa.

**Määrittää Azure AD kertakirjautumisen Asana, toimimalla seuraavasti:**

1. Valitse ylä-valikossa **Pika-aloitus**.

    ![Kertakirjautumisen määrittäminen][6]
2. Valitse perinteinen-portaalissa integrointi **Asana** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen Single Sign** -valintaikkunan avaaminen.

    ![Kertakirjautumisen määrittäminen][7] 

3. **Miten käyttäjät voivat kirjautua Asana haluat käyttää** sivulla Valitse **Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.
    
    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-asana-tutorial/tutorial_asana_06.png)

4. Valitse **Määritä sovelluksen asetukset** -sivulla toimimalla seuraavasti: 

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-asana-tutorial/tutorial_asana_07.png)


    a. Kirjoita merkki-URL-tekstiruutuun URL-osoite, käyttämällä seuraavaa kaavaa:`https://app.asana.com`

    c-näppäinyhdistelmää. Valitse **Seuraava**.

5. Valitse **Määritä kertakirjautumisen osoitteessa Asana** -sivulla **Lataa**, ja tallenna se tietokoneeseen. Kopioi myös arvo SAML SSO URL-osoitteen.
    
    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-asana-tutorial/tutorial_asana_08.png)

8. Varmenteen napsauttamalla hiiren kakkospainikkeella, Avaa Muistiossa sertifikaattitiedosto tai tekstieditorissa ensisijainen. Kopioi sisältö välillä alku ja loppu varmenteen otsikko. Tämä on aiot käyttää Asana määrittäminen SSO X.509-varmenne.

6. Eri selainikkunassa Sign Asana-sovellukseen. Määrittämiseen SSO Asana käyttää työtilan asetukset valitsemalla näytön oikeassa yläkulmassa oleva työtilan nimi. Valitse, ** \<työtilan nimi\> asetukset**. 

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-asana-tutorial/tutorial_asana_09.png)

7. Napsauta **Organisaatioasetukset** -ikkunassa **hallinta**. Valitse **jäsenet kirjautumalla sisään SAML kautta** voit ottaa SSO-määrityksen. Suorita seuraavat toimet:

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-asana-tutorial/tutorial_asana_10.png)

    a. Liitä SAML-Kirjaudu **sisään sivun URL** -texbox URL-osoitetta Azure AD.

    b. Liitä kopioimasi Azure AD X.509-varmenne **X.509-varmenne** -tekstiruutuun.

9. Valitse **Tallenna**. Siirry osoitteeseen [Asana opas SSO määrittäminen](https://asana.com/guide/help/premium/authentication#gl-saml) , jos tarvitset lisäohjeita.

7. Siirry **Määritä kertakirjautumisen osoitteessa Asana** Azure AD-sivulta valitsemalla Sign määritysten vahvistus ja valitse sitten **Seuraava**.
    
    ![Azure AD-Single Sign-On][10]

8. Valitse **Sign Vahvista** -sivulla **Valmis**.  
    
    ![Azure AD-Single Sign-On][11]


### <a name="creating-an-azure-ad-test-user"></a>Azure AD-testikäyttäjän luominen
Tässä osassa voit luoda testikäyttäjän kutsutaan Britta Simon perinteinen portaalissa.

![Luo Azure AD-käyttäjä][20]

**Voit luoda testikäyttäjän Azure AD-toimimalla seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen portal**, valitse vasemmassa siirtymisruudussa.
    
    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-asana-tutorial/create_aaduser_09.png) 

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Ylä-valikossa käyttäjäluettelon, näkyviin napsauttamalla **käyttäjät**.
    
    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-asana-tutorial/create_aaduser_03.png) 

4. Avaa **Lisää käyttäjä** -valintaikkunan alareunassa työkalurivillä, valitse **Lisää käyttäjä**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-asana-tutorial/create_aaduser_04.png) 

5. Tee valintaikkunassa **Kerro kyseisen käyttäjän tietoja** -sivulla seuraavasti:
 
    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-asana-tutorial/create_aaduser_05.png) 

    a. Käyttäjänä tyyppi Valitse uusi käyttäjä organisaatiossa.

    b. Kirjoita käyttäjänimi- **tekstiruutuun** **BrittaSimon**.

    c-näppäinyhdistelmää. Valitse **Seuraava**.

6.  Suorita **Käyttäjäprofiili** -sivulla seuraavat toimet:

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-asana-tutorial/create_aaduser_06.png) 

    a. Kirjoita **Ensimmäinen nimi** -tekstiruutuun **Britta**.  

    b. Kirjoita **Sukunimi** -tekstiruutuun tyyppi- **Simon**.

    c-näppäinyhdistelmää. Kirjoita **Näyttönimi** -tekstiruutuun **Britta Simon**.

    d. Valitse **rooli** -luettelosta **käyttäjä**.

    e. Valitse **Seuraava**.

7. Valitse **Hae tilapäinen salasana** -valintaikkunan sivulla **Luo**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-asana-tutorial/create_aaduser_07.png) 

8. **Hae tilapäinen salasana** -valintaikkunan sivulla toimimalla seuraavasti:

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-asana-tutorial/create_aaduser_08.png) 

    a. Kirjoita **Uusi salasana**arvo muistiin.

    b. Valitse **Valmis**.   



### <a name="creating-an-asana-test-user"></a>Asana-testikäyttäjän luominen

Tässä osassa nimeltä Britta Simon Asana käyttäjän luominen.

1. Siirry **Asana** **ryhmät** -osassa vasemman ruudun. Napsauta plusmerkkiä-painiketta. 

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-asana-tutorial/tutorial_asana_12.png) 

2. Kirjoita sähköpostin britta.simon@contoso.com tekstiruutuun ja valitse sitten **Kutsu**.
3. Valitse **Lähetä kutsu**. Uuden käyttäjän saavat sähköpostiviestin hänen sähköpostitiliin. Hän on luominen ja vahvista tili.


### <a name="assigning-the-azure-ad-test-user"></a>Azure AD-testikäyttäjän määrittäminen

Tässä osassa voit ottaa käyttöön Britta Simon käyttämään Azure kertakirjautumisen hänen käyttöoikeuksien myöntämistä Asana.

![Määrittää käyttäjälle][200] 

**Jos haluat määrittää Britta Simon Asana, toimi seuraavasti:**

1. Perinteinen-portaalissa Avaa sovellukset-näkymässä kansio-näkymässä, valitse **sovellukset** yläreunan-valikossa.

    ![Määrittää käyttäjälle][201] 

2. Valitse sovellukset-luettelosta **Asana**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-asana-tutorial/tutorial_asana_50.png) 

1. Valitse ylä-valikossa Valitse **käyttäjiä**.

    ![Määrittää käyttäjälle][203] 

1. Valitse kaikki käyttäjät-luettelosta **Britta Simon**.

2. Napsauta työkalurivin alaosassa **määrittäminen**.

    ![Määrittää käyttäjälle][205]


### <a name="testing-single-sign-on"></a>Kertakirjautumisen testaaminen

Tässä osassa tavoitteena on Testaa yhteyttä Azure AD kertakirjautumisen.

Siirry Asana kirjautumissivulle. Sähköpostien osoitteen tekstiruutu Lisää sähköpostiosoite britta.simon@contoso.com. Anna salasana-tekstiruutuun tyhjiä, ja valitse sitten **Kirjaudu sisään**. Sinut ohjataan Azure AD-kirjautumissivulle. Suorita Azure AD-tunnistetiedot. Nyt olet kirjautunut Asana.

## <a name="additional-resources"></a>Lisäresursseja

* [Luettelo siitä, miten voit integroida SaaS sovellusten Azure Active Directory-opetusohjelmat](active-directory-saas-tutorial-list.md)
* [Mikä on sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-asana-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-asana-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-asana-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-asana-tutorial/tutorial_general_04.png


[5]: ./media/active-directory-saas-asana-tutorial/tutorial_general_05.png
[6]: ./media/active-directory-saas-asana-tutorial/tutorial_general_06.png
[7]:  ./media/active-directory-saas-asana-tutorial/tutorial_general_050.png
[10]: ./media/active-directory-saas-asana-tutorial/tutorial_general_060.png
[11]: ./media/active-directory-saas-asana-tutorial/tutorial_general_070.png
[20]: ./media/active-directory-saas-asana-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-asana-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-asana-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-asana-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-asana-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-asana-tutorial/tutorial_general_205.png
