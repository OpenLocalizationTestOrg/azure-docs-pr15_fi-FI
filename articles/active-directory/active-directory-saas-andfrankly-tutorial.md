<properties
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi & frankly | Microsoft Azure"
    description="Opettele määrittämään kertakirjautumisen Azure Active Directoryn välillä ja & frankly."
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
    ms.date="08/12/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-frankly"></a>Opetusohjelma: Azure Active Directory-integrointi & frankly

Tässä opetusohjelmassa tavoitteena on kerrotaan, miten voit integroida & frankly ja Azure Active Directory (Azure AD).

Paikallisen ympäristön integroinnissa ja frankly kanssa Azure AD mahdollistaa seuraavat edut:

- Voit hallita Azure AD, joilla on käyttöoikeus & frankly
- Voit ottaa automaattisesti saa kirjautunut- & frankly-käyttäjille (kertakirjautumisen) niiden Azure AD-tilien kanssa
- Voit hallita asiakkaiden yhdessä keskitetyssä sijainnissa - Azure perinteinen portal

Jos haluat tietää tarkempia tietoja SaaS sovelluksen integrointi Azure AD-kohdassa [mitä sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Edellytykset

Määritä Azure AD integrointi ja frankly, voit edellyttää seuraavia:

- Azure AD-tilaus
- A & frankly käytössä tilauksessa single-merkki


> [AZURE.NOTE] Voit esikatsella vaiheet Tässä opetusohjelmassa, Emme suosittele käyttämällä tuotantoympäristössä.


Voit esikatsella vaiheet Tässä opetusohjelmassa-noudattamalla näitä suosituksia:

- Älä käytä tuotantoympäristössä, ellei se on tarpeen.
- Jos sinulla ei ole kokeiluversion Azure AD-ympäristössä, voit hankkia yksi kuukausi kokeiluversion [tähän](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Skenaarion kuvaus
Tässä opetusohjelmassa tavoitteena on, jotta voit testata Azure AD kertakirjautumisen testiympäristössä.

Tässä opetusohjelmassa kuvatut skenaarion kuuluu kaksi tärkeimmät rakenneosien:

1. Lisääminen ja frankly valikoimasta
2. Yksittäinen määrittäminen ja testaus Azure AD-on


## <a name="adding-frankly-from-the-gallery"></a>Lisääminen ja frankly valikoimasta
Voit määrittää integrointi / & frankly suoraan Azure AD-sinun on lisättävä ja frankly hallitun SaaS sovellusten luetteloon valikoimasta.

**Lisää & frankly valikoimasta, toimi seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen Portal**, valitse vasemmassa siirtymisruudussa. 

    ![Active Directory][1]

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .
    
    ![Sovellukset][2]

4. Valitse sivun alareunassa **Lisää** .
    
    ![Sovellukset][3]

5. Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Sovellukset][4]

6. Kirjoita hakuruutuun **ja frankly**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-andfrankly-tutorial/tutorial_andfrankly_01.png)

7. Tulokset-ruudussa Valitse **& frankly**ja valitse sitten **Valmis** , voit lisätä sovelluksen.

    ![Sovelluksen valitseminen valikoimasta](./media/active-directory-saas-andfrankly-tutorial/tutorial_andfrankly_0001.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yksittäinen määrittäminen ja testaus Azure AD-on
Tässä osassa tavoitteena on näyttää voit määrittämisestä ja testin Azure AD-single Sign kanssa ja frankly perusteella testikäyttäjän nimeltä "Britta Simon".

Kertakirjautumisen toimimaan Azure AD on tiedettävä vastaavaan käyttäjä- ja frankly Azure AD käyttäjä. Toisin sanoen linkki suhteen Azure AD-käyttäjä ja Aiheeseen liittyvät & frankly on vahvistettava.

Linkki-yhteys on muodostettu määrittämällä **käyttäjänimi** arvo Azure AD **käyttäjänimi** arvona & frankly.

Määritä ja testaa Azure AD kertakirjautumisen kanssa ja frankly, sinun on tehtävä seuraavat hakukyselyn:

1. **[Azure AD määrittäminen Single Sign](#configuring-azure-ad-single-single-sign-on)** - käyttäjät voivat käyttää tätä toimintoa.
2. **[Käyttäjän testata luominen Azure AD](#creating-an-azure-ad-test-user)** - Testaa Azure AD kertakirjautumisen Britta Simon kanssa.
3. **[Luominen & frankly testikäyttäjän](#creating-a-&frankly-test-user)** - vastaavaan Britta Simon, jotta hän Azure AD-esittäminen & frankly, joka on linkitetty.
4. **[Käyttäjän testata määrittäminen Azure AD](#assigning-the-azure-ad-test-user)** - Britta Simon käyttämään Azure AD kertakirjautumisen käyttöön.
5. **[Testaus Single Sign](#testing-single-sign-on)** - voit tarkistaa, toimiiko määritykset.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD kertakirjautumisen määrittäminen

Tässä osassa Azure AD kertakirjautumisen perinteinen portaalissa ottaminen käyttöön ja määrittäminen single Sign-että & frankly sovelluksen.

**Määrittäminen Azure AD single Sign-kanssa & frankly, toimi seuraavasti:**

1. Valitse perinteinen-portaalissa sivulla **& frankly** sovelluksen integrointi, **Määritä kertakirjautumisen** **Määrittäminen Single Sign** -valintaikkunan avaaminen.
     
    ![Kertakirjautumisen määrittäminen][6] 

2. Valitse **miten haluat kirjautua sisään & frankly käyttäjät** -sivulla Valitse **Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.
    
    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-andfrankly-tutorial/tutorial_andfrankly_03.png)

3. Jos haluat sovelluksen määrittäminen **IDP aloittanut tila**, seuraavien toimien **App-asetusten määrittäminen** -sivulla ja valitse **Seuraava**:

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-andfrankly-tutorial/tutorial_andfrankly_04.png)

    a. Kirjoita **tunnus** -tekstiruutuun URL-osoite, käyttämällä seuraavaa kaavaa:`https://andfrankly.com/saml/simplesaml/www/module.php/saml/sp/metadata.php/<tenant id>`

    b. Kirjoita **Vastaus URL** -tekstiruutuun URL-osoite, käyttämällä seuraavaa kaavaa:`https://andfrankly.com/saml/simplesaml/www/module.php/saml/sp/saml2-acs.php/<tenant id>`

    c-näppäinyhdistelmää. Valitse **Seuraava**

4. Jos haluat määrittää sovelluksen **SP aloittanut tila** -valintaikkunan **App-asetusten määrittäminen** -sivulla, valitse **"Näytä lisäasetukset (valinnainen)"** ja kirjoita **Merkki-URL-osoite** ja valitse **Seuraava**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-andfrankly-tutorial/tutorial_andfrankly_05.png)

    a. Kirjoita **Merkki-URL** -tekstiruutuun URL-osoite, käyttämällä seuraavaa kaavaa:`https://andfrankly.com/saml/okta/?saml_sso=<tenant id>`

    b. Valitse **Seuraava**

    > [AZURE.NOTE] Huomaa, että ne eivät ole todellisia arvoja. Voit joutua päivittämään nämä arvot Todellinen merkki-URL-osoite, tunnus ja vastaa URL-osoite. Yhteyshenkilön [help@andfrankly.com](emailTo:help@andfrankly.com) nämä arvot.

5. Valitse **määrittäminen kertakirjautumisen osoitteessa & frankly** -sivulla suorittamalla seuraavat vaiheet ja valitse **Seuraava**:

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-andfrankly-tutorial/tutorial_andfrankly_06.png)

    a. Valitse **Lataa**ja tallenna sitten tiedosto tietokoneesta.

    b. Valitse **Seuraava**.

6. Saat määritetty sovelluksen SSO-Ota yhteyttä & frankly tukeen kautta [help@andfrankly.com](emailTo:help@andfrankly.com). Ladatun metatiedot-tiedoston liittäminen ja jakaa sen kanssa ja määrittämään SSO kiertäminen frankly ryhmän.

7. Perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Seuraava**.
    
    ![Azure AD-Single Sign-On][10]

8. Valitse **Sign Vahvista** -sivulla **Valmis**.  
    
    ![Azure AD-Single Sign-On][11]



### <a name="creating-an-azure-ad-test-user"></a>Azure AD-testikäyttäjän luominen
Tässä osassa tavoitteena on luotava testikäyttäjän kutsutaan Britta Simon perinteinen portaalissa.

![Luo Azure AD-käyttäjä][20]

**Voit luoda testikäyttäjän Azure AD-toimimalla seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen Portal**, valitse vasemmassa siirtymisruudussa.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-andfrankly-tutorial/create_aaduser_09.png)

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Ylä-valikossa käyttäjäluettelon, näkyviin napsauttamalla **käyttäjät**.
    
    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-andfrankly-tutorial/create_aaduser_03.png)

4. Avaa **Lisää käyttäjä** -valintaikkunan alareunassa työkalurivillä, valitse **Lisää käyttäjä**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-andfrankly-tutorial/create_aaduser_04.png)

5. Tee valintaikkunassa **Kerro kyseisen käyttäjän tietoja** -sivulla seuraavasti:

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-andfrankly-tutorial/create_aaduser_05.png)

    a. Käyttäjänä tyyppi Valitse uusi käyttäjä organisaatiossa.

    b. Kirjoita käyttäjänimi- **tekstiruutuun** **BrittaSimon**.

    c-näppäinyhdistelmää. Valitse **Seuraava**.

6.  Suorita **Käyttäjäprofiili** -sivulla seuraavat toimet:
    
    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-andfrankly-tutorial/create_aaduser_06.png)

    a. Kirjoita **Ensimmäinen nimi** -tekstiruutuun **Britta**.  

    b. Kirjoita **Sukunimi** -tekstiruutuun tyyppi- **Simon**.

    c-näppäinyhdistelmää. Kirjoita **Näyttönimi** -tekstiruutuun **Britta Simon**.

    d. Valitse **rooli** -luettelosta **käyttäjä**.

    e. Valitse **Seuraava**.

7. Valitse **Hae tilapäinen salasana** -valintaikkunan sivulla **Luo**.
    
    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-andfrankly-tutorial/create_aaduser_07.png)

8. **Hae tilapäinen salasana** -valintaikkunan sivulla toimimalla seuraavasti:
    
    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-andfrankly-tutorial/create_aaduser_08.png)

    a. Kirjoita **Uusi salasana**arvo muistiin.

    b. Valitse **Valmis**.   



### <a name="creating-a-frankly-test-user"></a>Luodaan ja frankly testikäyttäjän

Tässä osassa nimeltä Britta Simon & frankly käyttäjän luominen. Ota & frankly tukeen kautta [help@andfrankly.com](emailTo:help@andfrankly.com) Lisää käyttäjien & frankly ympäristössä.


### <a name="assigning-the-azure-ad-test-user"></a>Azure AD-testikäyttäjän määrittäminen

Tässä osassa tavoitteena on Britta Simon käyttämään Azure kertakirjautumisen hänen käyttöoikeuksien myöntäminen & frankly ottaminen käyttöön.
    
![Määrittää käyttäjälle][200]

**Jos haluat määrittää Britta Simon & frankly, toimi seuraavasti:**

1. Perinteinen-portaalissa Avaa sovellukset-näkymässä kansio-näkymässä, valitse **sovellukset** yläreunan-valikossa.
    
    ![Määrittää käyttäjälle][201]

2. Valitse sovellukset-luettelosta **ja frankly**.
    
    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-andfrankly-tutorial/tutorial_andfrankly_50.png)

1. Valitse ylä-valikossa Valitse **käyttäjiä**.
    
    ![Määrittää käyttäjälle][203]

1. Valitse käyttäjät-luettelosta **Britta Simon**.

2. Napsauta työkalurivin alaosassa **määrittäminen**.
    
    ![Määrittää käyttäjälle][205]



### <a name="testing-single-sign-on"></a>Kertakirjautumisen testaaminen

Tässä osassa tavoitteena on Testaa Azure AD yksittäisen Sign-asetusten käyttäminen Access-paneeli.
 
Kun napsautat & frankly vierekkäin Access-ruudussa, näyttöön tulee automaattisesti kirjautunut sisään, että & frankly sovelluksen.


## <a name="additional-resources"></a>Lisäresursseja

* [Luettelo siitä, miten voit integroida SaaS sovellusten Azure Active Directory-opetusohjelmat](active-directory-saas-tutorial-list.md)
* [Mikä on sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-andfrankly-tutorial/tutorial_general_205.png
