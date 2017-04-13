<properties
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi YouEarnedIt | Microsoft Azure"
    description="Opettele määrittämään kertakirjautumisen Azure Active Directory- ja YouEarnedIt välillä."
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
    ms.date="10/07/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-youearnedit"></a>Opetusohjelma: YouEarnedIt Azure Active Directory-integrointi

Tässä opetusohjelmassa kerrotaan, miten voit integroida YouEarnedIt Azure Active Directory (Azure AD).

Azure AD-integraation YouEarnedIt avulla voit seuraavat edut:

- Voit hallita Azure AD, jolla on pääsy YouEarnedIt
- Voit ottaa käyttöön automaattisesti Hae kirjautunut käyttöön YouEarnedIt (kertakirjautumisen) niiden Azure AD-tilien käyttäjille
- Voit hallita asiakkaiden yhdessä keskitetyssä sijainnissa - Azure perinteinen portal

Jos haluat tietää tarkempia tietoja SaaS sovelluksen integrointi Azure AD-kohdassa [mitä sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Edellytykset

Määrittää Azure AD-integroinnin YouEarnedIt, tarvitset seuraavat:

- Azure AD-tilaus
- YouEarnedIt single-merkki käytössä-tilauksessa


> [AZURE.NOTE] Voit esikatsella vaiheet Tässä opetusohjelmassa, Emme suosittele käyttämällä tuotantoympäristössä.


Voit esikatsella vaiheet Tässä opetusohjelmassa-noudattamalla näitä suosituksia:

- Älä käytä tuotantoympäristössä, ellei se on tarpeen.
- Jos sinulla ei ole kokeiluversion Azure AD-ympäristössä, voit hankkia yksi kuukausi kokeiluversion [tähän](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Skenaarion kuvaus
Tässä opetusohjelmassa testaat Azure AD kertakirjautumisen testiympäristössä.

Tässä opetusohjelmassa kuvatut skenaarion kuuluu kaksi tärkeimmät rakenneosien:

1. YouEarnedIt lisääminen valikoimasta
2. Yksittäinen määrittäminen ja testaus Azure AD-on


## <a name="adding-youearnedit-from-the-gallery"></a>YouEarnedIt lisääminen valikoimasta
Voit määrittää YouEarnedIt integroida Azure AD-haluat lisätä YouEarnedIt valikoimasta hallitun SaaS sovellusluettelo.

**Jos haluat lisätä YouEarnedIt valikoimasta, toimi seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen portal**, valitse vasemmassa siirtymisruudussa.

    ![Active Directory][1]

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset][2]

4. Valitse sivun alareunassa **Lisää** .

    ![Sovellukset][3]

5. Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Sovellukset][4]

6. Kirjoita hakuruutuun **YouEarnedIt**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-youearnedit-tutorial/tutorial_youearnedit_01.png)

7. Valitse tulosruudussa **YouEarnedIt**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-youearnedit-tutorial/tutorial_youearnedit_06.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yksittäinen määrittäminen ja testaus Azure AD-on
Tässä osassa Määritä ja testaa Azure AD kertakirjautumisen YouEarnedIt nimeltä "Britta Simon" testikäyttäjän perusteella.

Kertakirjautumisen toimimaan Azure AD on tiedettävä käyttäjälle on YouEarnedIt vastaavaan käyttäjän Azure AD. Toisin sanoen suhteen Azure AD-käyttäjä ja Aiheeseen liittyvät YouEarnedIt-linkki on vahvistettava.

Linkki-yhteys on muodostettu määrittämällä **käyttäjänimi** arvo arvoksi YouEarnedIt **käyttäjänimi** Azure AD.

Määrittäminen ja testaaminen Azure AD kertakirjautumisen YouEarnedIt kanssa, sinun on tehtävä seuraavat hakukyselyn:

1. **[Azure AD määrittäminen Single Sign](#configuring-azure-ad-single-sign-on)** - käyttäjät voivat käyttää tätä toimintoa.
2. **[Käyttäjän testata luominen Azure AD](#creating-an-azure-ad-test-user)** - Testaa Azure AD kertakirjautumisen Britta Simon kanssa.
3. **[Luomisesta YouEarnedIt testata käyttäjän](#creating-a-predictix-price-reporting-test-user)** - tapahtumista Britta Simon ovat YouEarnedIt, jotka on linkitetty hän Azure AD esitys.
4. **[Käyttäjän testata määrittäminen Azure AD](#assigning-the-azure-ad-test-user)** - Britta Simon käyttämään Azure AD kertakirjautumisen käyttöön.
5. **[Testaus Single Sign](#testing-single-sign-on)** - voit tarkistaa, toimiiko määritykset.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD-Single Sign-On määrittäminen

Tässä osassa Azure AD kertakirjautumisen perinteinen portaalissa ottaminen käyttöön ja määritä kertakirjautumisen YouEarnedIt-sovelluksessa.


**Määrittää Azure AD kertakirjautumisen YouEarnedIt, toimimalla seuraavasti:**

1. Valitse perinteinen-portaalissa integrointi **YouEarnedIt** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen Single Sign** -valintaikkunan avaaminen.
     
    ![Kertakirjautumisen määrittäminen][6] 

2. **Miten käyttäjät voivat kirjautua YouEarnedIt haluat käyttää** sivulla Valitse **Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-youearnedit-tutorial/tutorial_youearnedit_03.png) 

3. Valitse **Määritä sovelluksen asetukset** -sivulla toimimalla seuraavasti:

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-youearnedit-tutorial/tutorial_youearnedit_04.png) 

    a. Kirjoita **Merkki-URL** -tekstiruutuun URL-osoite käyttämä Sign käyttämällä seuraavaa kaavaa YouEarnedIt sovelluksen käyttäjille: 

    - Eristetyn-ympäristöön:`https://<company name>.sandbox.youearnedit.com/users/sign_in`
    - Tuotantoympäristössä:`https://<company name>.youearnedit.com/users/sign_in`
    
    b. Valitse **Seuraava**
 
4. Valitse **Määritä kertakirjautumisen osoitteessa YouEarnedIt** -sivulla toimimalla seuraavasti:

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-youearnedit-tutorial/tutorial_youearnedit_05.png)

    a. Valitse **Lataa**ja tallenna sitten tiedosto tietokoneesta.

    b. Valitse **Seuraava**.


5. Saat määritetty sovelluksen SSO-YouEarnedIt tukeen ja antaa niille seuraavasti:

    • Ladatut **varmenne**

    • **SAML SSO URL-osoite**

6. Perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Seuraava**.
    
    ![Azure AD-Single Sign-On][10]

7. Valitse **Sign Vahvista** -sivulla **Valmis**.  
 
    ![Azure AD-Single Sign-On][11]


### <a name="creating-an-azure-ad-test-user"></a>Azure AD-testikäyttäjän luominen
Tässä osassa voit luoda testikäyttäjän kutsutaan Britta Simon perinteinen portaalissa.


![Luo Azure AD-käyttäjä][20]

**Voit luoda testikäyttäjän Azure AD-toimimalla seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen portal**, valitse vasemmassa siirtymisruudussa.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-youearnedit-tutorial/create_aaduser_09.png) 

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Ylä-valikossa käyttäjäluettelon, näkyviin napsauttamalla **käyttäjät**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-youearnedit-tutorial/create_aaduser_03.png) 

4. Avaa **Lisää käyttäjä** -valintaikkunan alareunassa työkalurivillä, valitse **Lisää käyttäjä**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-youearnedit-tutorial/create_aaduser_04.png) 

5. Suorita valintaikkunan **Kerro kyseisen käyttäjän tietoja** -sivulla seuraavat toimet:  ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-youearnedit-tutorial/create_aaduser_05.png) 

    a. Käyttäjänä tyyppi Valitse uusi käyttäjä organisaatiossa.

    b. Kirjoita käyttäjänimi- **tekstiruutuun** **BrittaSimon**.

    c-näppäinyhdistelmää. Valitse **Seuraava**.

6.  Suorita valintaikkunan **Käyttäjäprofiili** -sivulla seuraavat toimet: ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-youearnedit-tutorial/create_aaduser_06.png) 

    a. Kirjoita **Ensimmäinen nimi** -tekstiruutuun **Britta**.  

    b. Kirjoita **Sukunimi** -tekstiruutuun tyyppi- **Simon**.

    c-näppäinyhdistelmää. Kirjoita **Näyttönimi** -tekstiruutuun **Britta Simon**.

    d. Valitse **rooli** -luettelosta **käyttäjä**.

    e. Valitse **Seuraava**.

7. Valitse **Hae tilapäinen salasana** -valintaikkunan sivulla **Luo**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-youearnedit-tutorial/create_aaduser_07.png) 

8. **Hae tilapäinen salasana** -valintaikkunan sivulla toimimalla seuraavasti:

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-youearnedit-tutorial/create_aaduser_08.png) 

    a. Kirjoita **Uusi salasana**arvo muistiin.

    b. Valitse **Valmis**.   



### <a name="creating-an-youearnedit-test-user"></a>YouEarnedIt-testikäyttäjän luominen

Tässä osassa nimeltä Britta Simon YouEarnedIt käyttäjän luominen. Toimi YouEarnedIt tukihenkilöstöösi voit lisätä käyttäjiä YouEarnedIt-ympäristössä.

> [AZURE.NOTE]  YouEarnedIt toiminta tunnistetietojen toimittaja toimittamaan EmailAddress tai käyttäjänimi NameID-määrite. Todennus epäonnistuu, jos vastaava käyttäjänimi tai EmailAddress ei löydy tietokannasta tai ei vastaa täsmälleen. Tämä edellyttää, että tilit voi tuoda YouEarnedIt järjestelmä ennen SSO-integrointia (yleensä joko API- tai CSV-tuonti kautta).

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD-testikäyttäjän määrittäminen

Tässä osassa voit ottaa käyttöön Britta Simon käyttämään Azure kertakirjautumisen hänen käyttöoikeuksien myöntämistä YouEarnedIt.

![Määrittää käyttäjälle][200] 

**Jos haluat määrittää Britta Simon YouEarnedIt, toimi seuraavasti:**

1. Perinteinen-portaalissa Avaa sovellukset-näkymässä kansio-näkymässä, valitse **sovellukset** yläreunan-valikossa.

    ![Määrittää käyttäjälle][201] 

2. Valitse sovellukset-luettelosta **YouEarnedIt**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-youearnedit-tutorial/tutorial_youearnedit_50.png) 

3. Valitse ylä-valikossa Valitse **käyttäjiä**.

    ![Määrittää käyttäjälle][203]

4. Valitse käyttäjät-luettelosta **Britta Simon**.

5. Napsauta työkalurivin alaosassa **määrittäminen**.

    ![Määrittää käyttäjälle][205]


### <a name="testing-single-sign-on"></a>Kertakirjautumisen testaaminen

Tässä osassa voit testata Azure AD yksittäisen Sign-asetusten käyttäminen Access-paneeli.

Access-ruudussa YouEarnedIt-ruutua napsauttaessasi sinun pitäisi saada automaattisesti kirjautunut-on YouEarnedIt sovelluksen.


## <a name="additional-resources"></a>Lisäresursseja

* [Luettelo siitä, miten voit integroida SaaS sovellusten Azure Active Directory-opetusohjelmat](active-directory-saas-tutorial-list.md)
* [Mikä on sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-youearnedit-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-youearnedit-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-youearnedit-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-youearnedit-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-youearnedit-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-youearnedit-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-youearnedit-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-youearnedit-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-youearnedit-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-youearnedit-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-youearnedit-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-youearnedit-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-youearnedit-tutorial/tutorial_general_205.png
