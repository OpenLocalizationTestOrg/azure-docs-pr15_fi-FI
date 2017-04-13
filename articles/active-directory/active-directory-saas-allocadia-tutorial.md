<properties
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi Allocadia | Microsoft Azure"
    description="Opettele määrittämään kertakirjautumisen Azure Active Directory- ja Allocadia välillä."
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


# <a name="tutorial-azure-active-directory-integration-with-allocadia"></a>Opetusohjelma: Allocadia Azure Active Directory-integrointi

Tässä opetusohjelmassa kerrotaan, miten voit integroida Allocadia Azure Active Directory (Azure AD).

Azure AD-integraation Allocadia avulla voit seuraavat edut:

- Voit hallita Azure AD, jolla on pääsy Allocadia
- Voit ottaa käyttöön automaattisesti Hae kirjautunut käyttöön Allocadia (kertakirjautumisen) niiden Azure AD-tilien käyttäjille
- Voit hallita asiakkaiden yhdessä keskitetyssä sijainnissa - Azure perinteinen portal

Jos haluat tietää tarkempia tietoja SaaS sovelluksen integrointi Azure AD-kohdassa [mitä sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Edellytykset

Määrittää Azure AD-integroinnin Allocadia, tarvitset seuraavat:

- Azure AD-tilaus
- Allocadia single-merkki käytössä-tilauksessa


> [AZURE.NOTE] Voit esikatsella vaiheet Tässä opetusohjelmassa, Emme suosittele käyttämällä tuotantoympäristössä.


Voit esikatsella vaiheet Tässä opetusohjelmassa-noudattamalla näitä suosituksia:

- Älä käytä tuotantoympäristössä, ellei se on tarpeen.
- Jos sinulla ei ole kokeiluversion Azure AD-ympäristössä, voit hankkia yksi kuukausi kokeiluversion [tähän](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Skenaarion kuvaus
Tässä opetusohjelmassa testaat Azure AD kertakirjautumisen testiympäristössä. Tässä opetusohjelmassa kuvatut skenaarion kuuluu kaksi tärkeimmät rakenneosien:

1. Allocadia lisääminen valikoimasta
2. Yksittäinen määrittäminen ja testaus Azure AD-on


## <a name="adding-allocadia-from-the-gallery"></a>Allocadia lisääminen valikoimasta
Voit määrittää Allocadia integroida Azure AD-haluat lisätä Allocadia valikoimasta hallitun SaaS sovellusluettelo.

**Jos haluat lisätä Allocadia valikoimasta, toimi seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen portal**, valitse vasemmassa siirtymisruudussa. 

    ![Active Directory][1]

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset][2]

4. Valitse sivun alareunassa **Lisää** .

    ![Sovellukset][3]

5. Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Sovellukset][4]

6. Kirjoita hakuruutuun **Allocadia**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-allocadia-tutorial/tutorial_allocadia_01.png)

7. Valitse tulosruudussa **Allocadia**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-allocadia-tutorial/tutorial_allocadia_06.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yksittäinen määrittäminen ja testaus Azure AD-on
Tässä osassa Määritä ja testaa Azure AD kertakirjautumisen Allocadia nimeltä "Britta Simon" testikäyttäjän perusteella.

Kertakirjautumisen toimimaan Azure AD on tiedettävä käyttäjälle on Allocadia vastaavaan käyttäjän Azure AD. Toisin sanoen suhteen Azure AD-käyttäjä ja Aiheeseen liittyvät Allocadia-linkki on vahvistettava.
Linkki-yhteys on muodostettu määrittämällä **käyttäjänimi** arvo arvoksi Allocadia **käyttäjänimi** Azure AD.

Määrittäminen ja testaaminen Azure AD kertakirjautumisen Allocadia kanssa, sinun on tehtävä seuraavat hakukyselyn:

1. **[Azure AD määrittäminen Single Sign](#configuring-azure-ad-single-single-sign-on)** - käyttäjät voivat käyttää tätä toimintoa.
2. **[Käyttäjän testata luominen Azure AD](#creating-an-azure-ad-test-user)** - Testaa Azure AD kertakirjautumisen Britta Simon kanssa.
4. **[Käyttäjän luominen Allocadia testata](#creating-an-allocadia-test-user)** - on tapahtumista Britta Simon Allocadia kyseistä henkilöä hän Azure AD esitys.
5. **[Käyttäjän testata määrittäminen Azure AD](#assigning-the-azure-ad-test-user)** - Britta Simon käyttämään Azure AD kertakirjautumisen käyttöön.
5. **[Testaus Single Sign](#testing-single-sign-on)** - voit tarkistaa, toimiiko määritykset.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD-Single Sign-On määrittäminen

Tässä osassa Azure AD kertakirjautumisen perinteinen portaalissa ottaminen käyttöön ja määritä kertakirjautumisen Allocadia-sovelluksessa.


Allocadia sovelluksen odottaa SAML vahvistukset tietyssä muodossa. Määritä seuraavat saatavat tämän sovelluksen. Voit hallita näitä määritteitä arvot sovelluksen **"Atrribute"** -välilehdestä. Seuraavassa näyttökuvassa näkyy esimerkki tämän. 

![Kertakirjautumisen määrittäminen](./media/active-directory-saas-allocadia-tutorial/tutorial_allocadia_07.png) 

**Määrittää Azure AD kertakirjautumisen Hightail, toimimalla seuraavasti:**


1. Valitse Azure perinteinen portaalissa **Allocadia** sovelluksen integrointi-sivun yläreunassa valikosta **määritteet**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-allocadia-tutorial/tutorial_general_80.png) 


2. **SAML-tunnuksen määritteet** -valintaikkunassa kullekin riville seuraavassa taulukossa näkyvät toimimalla seuraavasti:

  	| Määritteen nimi | Määritteen arvo |
  	| --- | --- |    
  	| Etunimi | User.givenName |
  	| Sukunimi  | User.surname |
  	| sähköpostin | User.Mail |
    

    a. Valitse **Lisää käyttäjä Attribure** -valintaikkunan avaaminen **Lisää käyttäjä-määrite** .

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-allocadia-tutorial/tutorial_general_81.png) 


    b. Kirjoita määritteen nimi näkyy kyseisen rivin **Attrubute nimi** -tekstiruutuun.

    c-näppäinyhdistelmää. **Määritteen arvo** -luettelosta selsect kyseisen rivin määritteen arvo.

    d. Valitse **Valmis**.  
    

3. Valitse ylä-valikossa **Pika-aloitus**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-allocadia-tutorial/tutorial_general_83.png)  


4. **Miten käyttäjät voivat kirjautua Allocadia haluat käyttää** sivulla Valitse **Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.
    
    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-allocadia-tutorial/tutorial_allocadia_03.png) 

5. Valitse **Määritä sovelluksen asetukset** -sivulla toimimalla seuraavasti:.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-allocadia-tutorial/tutorial_allocadia_04.png) 

    a. Kirjoita TUNNUKSEEN-ruutuun URL-Osoitetta seuraavaa kaavaa: Testaa ympäristön käytettäväksi URL-Osoitteen **"https://na2standby.allocadia.com"** - ja tuotantoympäristössä käytettävä **"https://na2.allocadia.com"**

    b. Kirjoita vastaus URL-osoite URL-Osoitetta seuraavaa kaavaa: Testaa ympäristön käytettäväksi URL-Osoitteen kuvion **"https://na2standby.allocadia.com/allocadia/saml/SSO"** - ja tuotantoympäristössä käyttää **"https://na2.allocadia.com/allocadia/saml/SSO"**


6. Valitse **Määritä kertakirjautumisen osoitteessa Allocadia** -sivulla toimimalla seuraavasti:

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-allocadia-tutorial/tutorial_allocadia_05.png) 

    a. Valitse **Lataa metatiedot**ja tallenna sitten tiedosto tietokoneesta.

    b. Valitse **Seuraava**.


7.  Saat määritetty sovelluksen SSO-yhteyshenkilön [Allocadia](mailTo:support@allocadia.com) tukihenkilöstön ja ne auttavat SSO määrittämiseen. Huomaa, että sinulla on lähettää sähköpostia ja liitä ladata Ota SSO määrittämiseksi Allocadia puolella metatieto-tiedosto.
 
    > [AZURE.NOTE] Varmista, että Allocadia ryhmän arvo tunnus testiympäristössä **"https://na2standby.allocadia.com"** - ja tuotantoympäristössä, sen on oltava: **"https://na2.allocadia.com"**


8. Perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Seuraava**.
    
    ![Azure AD-Single Sign-On][10]

9. Valitse **Sign Vahvista** -sivulla **Valmis**.  
    
    ![Azure AD-Single Sign-On][11]



### <a name="creating-an-azure-ad-test-user"></a>Azure AD-testikäyttäjän luominen

Tässä osassa voit luoda testikäyttäjän kutsutaan Britta Simon perinteinen portaalissa.
Valitse käyttäjät-luettelosta **Britta Simon**.

![Luo Azure AD-käyttäjä][20]

**Voit luoda testikäyttäjän Azure AD-toimimalla seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen portal**, valitse vasemmassa siirtymisruudussa.
    
    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-allocadia-tutorial/create_aaduser_09.png) 

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Ylä-valikossa käyttäjäluettelon, näkyviin napsauttamalla **käyttäjät**.
    
    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-allocadia-tutorial/create_aaduser_03.png) 

4. Avaa **Lisää käyttäjä** -valintaikkunan alareunassa työkalurivillä, valitse **Lisää käyttäjä**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-allocadia-tutorial/create_aaduser_04.png) 

5. Tee valintaikkunassa **Kerro kyseisen käyttäjän tietoja** -sivulla seuraavasti:
 
    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-allocadia-tutorial/create_aaduser_05.png) 

    a. Käyttäjänä tyyppi Valitse uusi käyttäjä organisaatiossa.

    b. Kirjoita käyttäjänimi- **tekstiruutuun** **BrittaSimon**.

    c-näppäinyhdistelmää. Valitse **Seuraava**.

6.  Suorita **Käyttäjäprofiili** -sivulla seuraavat toimet:

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-allocadia-tutorial/create_aaduser_06.png) 

    a. Kirjoita **Ensimmäinen nimi** -tekstiruutuun **Britta**.  

    b. Kirjoita **Sukunimi** -tekstiruutuun tyyppi- **Simon**.

    c-näppäinyhdistelmää. Kirjoita **Näyttönimi** -tekstiruutuun **Britta Simon**.

    d. Valitse **rooli** -luettelosta **käyttäjä**.

    e. Valitse **Seuraava**.

7. Valitse **Hae tilapäinen salasana** -valintaikkunan sivulla **Luo**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-allocadia-tutorial/create_aaduser_07.png) 

8. **Hae tilapäinen salasana** -valintaikkunan sivulla toimimalla seuraavasti:

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-allocadia-tutorial/create_aaduser_08.png) 

    a. Kirjoita **Uusi salasana**arvo muistiin.

    b. Valitse **Valmis**.   



### <a name="creating-an-allocadia-test-user"></a>Allocadia-testikäyttäjän luominen

Tässä osassa nimeltä Britta Simon Allocadia käyttäjän luominen. Allocadia sovelluksen tuki vain kerran käyttäjän valmistelu. Jos olet määrittänyt saatavat ilmoitetulla yläpuolella **[Määrittäminen Azure AD Single Sign](#configuring-azure-ad-single-single-sign-on)** -osassa sen valmistella sovelluksen käyttäjille. 


> [AZURE.NOTE] Jos haluat luoda käyttäjän manuaalisesti tai erän käyttäjien käytettävissä, sinun on Allocadia-tukeen.


### <a name="assigning-the-azure-ad-test-user"></a>Azure AD-testikäyttäjän määrittäminen

Tässä osassa voit ottaa käyttöön Britta Simon käyttämään Azure kertakirjautumisen hänen käyttöoikeuksien myöntämistä Allocadia.

![Määrittää käyttäjälle][200] 

**Jos haluat määrittää Britta Simon Allocadia, toimi seuraavasti:**

1. Perinteinen-portaalissa Avaa sovellukset-näkymässä kansio-näkymässä, valitse **sovellukset** yläreunan-valikossa.

    ![Määrittää käyttäjälle][201] 

2. Valitse sovellukset-luettelosta **Allocadia**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-allocadia-tutorial/tutorial_allocadia_50.png) 

1. Valitse ylä-valikossa Valitse **käyttäjiä**.

    ![Määrittää käyttäjälle][203] 

1. Valitse käyttäjät-luettelosta **Britta Simon**.

2. Napsauta työkalurivin alaosassa **määrittäminen**.

    ![Määrittää käyttäjälle][205]


### <a name="testing-single-sign-on"></a>Kertakirjautumisen testaaminen

Tässä osassa voit testata Azure AD yksittäisen Sign-asetusten käyttäminen Access-paneeli.
Access-ruudussa Allocadia-ruutua napsauttaessasi sinun pitäisi saada automaattisesti kirjautunut-on Allocadia sovelluksen.


## <a name="additional-resources"></a>Lisäresursseja

* [Luettelo siitä, miten voit integroida SaaS sovellusten Azure Active Directory-opetusohjelmat](active-directory-saas-tutorial-list.md)
* [Mikä on sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-allocadia-tutorial/tutorial_general_205.png
