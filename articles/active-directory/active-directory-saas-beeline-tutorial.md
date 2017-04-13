<properties
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi Beeline | Microsoft Azure"
    description="Opettele määrittämään kertakirjautumisen Azure Active Directory- ja Beeline välillä."
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
    ms.date="09/19/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-beeline"></a>Opetusohjelma: Beeline Azure Active Directory-integrointi

Tässä opetusohjelmassa kerrotaan, miten voit integroida Beeline Azure Active Directory (Azure AD).

Azure AD-integraation Beeline avulla voit seuraavat edut:

- Voit hallita Azure AD, jolla on pääsy Beeline
- Voit ottaa käyttöön automaattisesti Hae kirjautunut käyttöön Beeline (kertakirjautumisen) niiden Azure AD-tilien käyttäjille
- Voit hallita asiakkaiden yhdessä keskitetyssä sijainnissa - Azure perinteinen portal

Jos haluat tietää tarkempia tietoja SaaS sovelluksen integrointi Azure AD-kohdassa [mitä sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Edellytykset

Määrittää Azure AD-integroinnin Beeline, tarvitset seuraavat:

- Azure AD-tilaus
- Beeline single-merkki käytössä-tilauksessa


> [AZURE.NOTE] Voit esikatsella vaiheet Tässä opetusohjelmassa, Emme suosittele käyttämällä tuotantoympäristössä.


Voit esikatsella vaiheet Tässä opetusohjelmassa-noudattamalla näitä suosituksia:

- Älä käytä tuotantoympäristössä, ellei se on tarpeen.
- Jos sinulla ei ole kokeiluversion Azure AD-ympäristössä, voit hankkia yksi kuukausi kokeiluversion [tähän](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Skenaarion kuvaus
Tässä opetusohjelmassa testaat Azure AD kertakirjautumisen testiympäristössä. Tässä opetusohjelmassa kuvatut skenaarion kuuluu kaksi tärkeimmät rakenneosien:

1. Beeline lisääminen valikoimasta
2. Yksittäinen määrittäminen ja testaus Azure AD-on


## <a name="adding-beeline-from-the-gallery"></a>Beeline lisääminen valikoimasta
Voit määrittää Beeline integroida Azure AD-haluat lisätä Beeline valikoimasta hallitun SaaS sovellusluettelo.

**Jos haluat lisätä Beeline valikoimasta, toimi seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen portal**, valitse vasemmassa siirtymisruudussa. 

    ![Active Directory][1]

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset][2]

4. Valitse sivun alareunassa **Lisää** .

    ![Sovellukset][3]

5. Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Sovellukset][4]

6. Kirjoita hakuruutuun **Beeline**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-beeline-tutorial/tutorial_beeline_01.png)

7. Valitse tulosruudussa **Beeline**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-beeline-tutorial/tutorial_beeline_06.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yksittäinen määrittäminen ja testaus Azure AD-on
Tässä osassa Määritä ja testaa Azure AD kertakirjautumisen Beeline nimeltä "Britta Simon" testikäyttäjän perusteella.

Kertakirjautumisen toimimaan Azure AD on tiedettävä käyttäjälle on Beeline vastaavaan käyttäjän Azure AD. Toisin sanoen suhteen Azure AD-käyttäjä ja Aiheeseen liittyvät Beeline-linkki on vahvistettava.
Linkki-yhteys on muodostettu määrittämällä **käyttäjänimi** arvo arvoksi Beeline **käyttäjänimi** Azure AD.

Määrittäminen ja testaaminen Azure AD kertakirjautumisen Beeline kanssa, sinun on tehtävä seuraavat hakukyselyn:

1. **[Azure AD määrittäminen Single Sign](#configuring-azure-ad-single-single-sign-on)** - käyttäjät voivat käyttää tätä toimintoa.
2. **[Käyttäjän testata luominen Azure AD](#creating-an-azure-ad-test-user)** - Testaa Azure AD kertakirjautumisen Britta Simon kanssa.
4. **[Käyttäjän luominen Beeline testata](#creating-an-beeline-test-user)** - on tapahtumista Britta Simon Beeline kyseistä henkilöä hän Azure AD esitys.
5. **[Käyttäjän testata määrittäminen Azure AD](#assigning-the-azure-ad-test-user)** - Britta Simon käyttämään Azure AD kertakirjautumisen käyttöön.
5. **[Testaus Single Sign](#testing-single-sign-on)** - voit tarkistaa, toimiiko määritykset.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD kertakirjautumisen määrittäminen

Tässä osassa Azure AD kertakirjautumisen perinteinen portaalissa ottaminen käyttöön ja määritä kertakirjautumisen Beeline-sovelluksessa.

Beeline sovelluksen odottaa SAML vahvistukset tietyssä muodossa. Toimi Beeline työryhmän ensin tunnistavan oikea käyttäjätunnus johon yhdistetään sovellukseen. Tutustu myös ohjeita siitä, johon ne haluat käyttää yhdistämisen määritettä Beeline kuulumisia. Microsoft suosittelee käyttämään **"NameIdentifier"** -määritteen käyttäjätunnus. Voit hallita tämän määritteen arvo sovelluksen **"Atrribute"** -välilehdestä. Seuraavassa näyttökuvassa näkyy esimerkki tämän. Seuraavassa on on yhdistetty nameidentifier vaatimus **userprincipalname** -määritteen, joka sisältää yksilöllinen Käyttäjätunnus, joka lähetetään jokainen onnistuneen SAML-vastaus Beeline-sovelluksen kanssa.

![Kertakirjautumisen määrittäminen](./media/active-directory-saas-beeline-tutorial/tutorial_beeline_07.png) 


**Määrittää Azure AD kertakirjautumisen Beeline, toimimalla seuraavasti:**

1. Valitse perinteinen-portaalissa integrointi **Beeline** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen Single Sign** -valintaikkunan avaaminen.

     ![Kertakirjautumisen määrittäminen][6] 

2. **Miten käyttäjät voivat kirjautua Beeline haluat käyttää** sivulla Valitse **Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.
    
    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-beeline-tutorial/tutorial_beeline_03.png) 

3. Valitse **Määritä sovelluksen asetukset** -sivulla toimimalla seuraavasti:.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-beeline-tutorial/tutorial_beeline_04.png) 


    a. Kirjoita **tunnus** -tekstiruutuun URL-osoite käyttäjien Sign Beeline sovelluksen käyttämällä seuraavaa kaavaa:`https://projects.beeline.net/<instance name>`

    b. Kirjoita vastaus URL-osoite URL-Osoitetta seuraavaa kaavaa: `https://projects.beeline.net/<instance name>/SSO_External.ashx` tai`https://projects.beeline.net/<company name>/SSO_External.ashx`


4. Valitse **Määritä kertakirjautumisen osoitteessa Beeline** -sivulla toimimalla seuraavasti:

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-beeline-tutorial/tutorial_beeline_05.png) 

    a. Valitse **Lataa metatiedot**ja tallenna sitten tiedosto tietokoneesta.

    b. Valitse **Seuraava**.


5.  Saat määritetty sovelluksen SSO-yhteyshenkilön Beeline tukihenkilöstön ja ne auttavat SSO määrittämiseen. Huomaa, että sinulla on lähettää sähköpostia ja liittää ladatut metatietojen tiedoston ja lisätä kohteen tunnus ja yhteen Kirjaudu ulos palvelun URL.
  
6. Perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Seuraava**.
    
    ![Azure AD-Single Sign-On][10]

7. Valitse **Sign Vahvista** -sivulla **Valmis**.  
    
    ![Azure AD-Single Sign-On][11]



### <a name="creating-an-azure-ad-test-user"></a>Azure AD-testikäyttäjän luominen
Tässä osassa voit luoda testikäyttäjän kutsutaan Britta Simon perinteinen portaalissa.


![Luo Azure AD-käyttäjä][20]

**Voit luoda testikäyttäjän Azure AD-toimimalla seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen portal**, valitse vasemmassa siirtymisruudussa.
    
    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-beeline-tutorial/create_aaduser_09.png) 

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Ylä-valikossa käyttäjäluettelon, näkyviin napsauttamalla **käyttäjät**.
    
    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-beeline-tutorial/create_aaduser_03.png) 

4. Avaa **Lisää käyttäjä** -valintaikkunan alareunassa työkalurivillä, valitse **Lisää käyttäjä**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-beeline-tutorial/create_aaduser_04.png) 

5. Tee valintaikkunassa **Kerro kyseisen käyttäjän tietoja** -sivulla seuraavasti:
 
    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-beeline-tutorial/create_aaduser_05.png) 

    a. Käyttäjänä tyyppi Valitse uusi käyttäjä organisaatiossa.

    b. Kirjoita käyttäjänimi- **tekstiruutuun** **BrittaSimon**.

    c-näppäinyhdistelmää. Valitse **Seuraava**.

6.  Suorita **Käyttäjäprofiili** -sivulla seuraavat toimet:

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-beeline-tutorial/create_aaduser_06.png) 

    a. Kirjoita **Ensimmäinen nimi** -tekstiruutuun **Britta**.  

    b. Kirjoita **Sukunimi** -tekstiruutuun tyyppi- **Simon**.

    c-näppäinyhdistelmää. Kirjoita **Näyttönimi** -tekstiruutuun **Britta Simon**.

    d. Valitse **rooli** -luettelosta **käyttäjä**.

    e. Valitse **Seuraava**.

7. Valitse **Hae tilapäinen salasana** -valintaikkunan sivulla **Luo**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-beeline-tutorial/create_aaduser_07.png) 

8. **Hae tilapäinen salasana** -valintaikkunan sivulla toimimalla seuraavasti:

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-beeline-tutorial/create_aaduser_08.png) 

    a. Kirjoita **Uusi salasana**arvo muistiin.

    b. Valitse **Valmis**.   



### <a name="creating-an-beeline-test-user"></a>Beeline-testikäyttäjän luominen

Tässä osassa nimeltä Britta Simon Beeline käyttäjän luominen. Beeline-sovellus on kaikkien käyttäjien valmisteltu ennen kuin teet kertakirjautuminen-sovelluksessa. Ota niin työ Beeline asiakkaan kanssa tuki Liitä nämä käyttäjät valmistelu sovellukseen. 


> [AZURE.NOTE] Jos haluat luoda käyttäjän manuaalisesti tai erän käyttäjien käytettävissä, sinun on Beeline-tukeen.


### <a name="assigning-the-azure-ad-test-user"></a>Azure AD-testikäyttäjän määrittäminen

Tässä osassa voit ottaa käyttöön Britta Simon käyttämään Azure kertakirjautumisen hänen käyttöoikeuksien myöntämistä Beeline.

![Määrittää käyttäjälle][200] 

**Jos haluat määrittää Britta Simon Beeline, toimi seuraavasti:**

1. Perinteinen-portaalissa Avaa sovellukset-näkymässä kansio-näkymässä, valitse **sovellukset** yläreunan-valikossa.

    ![Määrittää käyttäjälle][201] 

2. Valitse sovellukset-luettelosta **Beeline**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-beeline-tutorial/tutorial_beeline_50.png) 

1. Valitse ylä-valikossa Valitse **käyttäjiä**.

    ![Määrittää käyttäjälle][203] 

1. Valitse käyttäjät-luettelosta **Britta Simon**.

2. Napsauta työkalurivin alaosassa **määrittäminen**.

    ![Määrittää käyttäjälle][205]


### <a name="testing-single-sign-on"></a>Kertakirjautumisen testaaminen

Tässä osassa voit testata Azure AD yksittäisen Sign-asetusten käyttäminen Access-paneeli.
Access-ruudussa Beeline-ruutua napsauttaessasi sinun pitäisi saada automaattisesti kirjautunut-on Beeline sovelluksen.


## <a name="additional-resources"></a>Lisäresursseja

* [Luettelo siitä, miten voit integroida SaaS sovellusten Azure Active Directory-opetusohjelmat](active-directory-saas-tutorial-list.md)
* [Mikä on sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-beeline-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-beeline-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-beeline-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-beeline-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-beeline-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-beeline-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-beeline-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-beeline-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-beeline-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-beeline-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-beeline-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-beeline-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-beeline-tutorial/tutorial_general_205.png
