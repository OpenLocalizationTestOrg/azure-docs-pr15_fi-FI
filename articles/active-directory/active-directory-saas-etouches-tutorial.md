<properties
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi eTouches | Microsoft Azure"
    description="Opettele määrittämään kertakirjautumisen Azure Active Directory- ja eTouches välillä."
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
    ms.date="10/18/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-etouches"></a>Opetusohjelma: ETouches Azure Active Directory-integrointi

Tässä opetusohjelmassa kerrotaan, miten voit integroida eTouches Azure Active Directory (Azure AD).

Azure AD-integraation eTouches avulla voit seuraavat edut:

- Voit hallita Azure AD, jolla on pääsy eTouches
- Voit ottaa käyttöön automaattisesti Hae kirjautunut käyttöön niiden Azure AD-tilien kanssa (kertakirjautumisen) eTouches käyttäjille
- Voit hallita asiakkaiden yhdessä keskitetyssä sijainnissa - Azure perinteinen portal

Jos haluat tietää tarkempia tietoja SaaS sovelluksen integrointi Azure AD-kohdassa [mitä sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Edellytykset

Määrittää Azure AD-integroinnin eTouches, tarvitset seuraavat:

- Azure AD-tilaus
- ETouches single-merkki käytössä-tilauksessa


> [AZURE.NOTE] Voit esikatsella vaiheet Tässä opetusohjelmassa, Emme suosittele käyttämällä tuotantoympäristössä.


Voit esikatsella vaiheet Tässä opetusohjelmassa-noudattamalla näitä suosituksia:

- Älä käytä tuotantoympäristössä, ellei se on tarpeen.
- Jos sinulla ei ole kokeiluversion Azure AD-ympäristössä, voit hankkia yksi kuukausi kokeiluversion [tähän](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Skenaarion kuvaus
Tässä opetusohjelmassa testaat Azure AD kertakirjautumisen testiympäristössä.

Tässä opetusohjelmassa kuvatut skenaarion kuuluu kaksi tärkeimmät rakenneosien:

1. ETouches lisääminen valikoimasta
2. Yksittäinen määrittäminen ja testaus Azure AD-on


## <a name="adding-etouches-from-the-gallery"></a>ETouches lisääminen valikoimasta
Voit määrittää eTouches integroida Azure AD-haluat lisätä eTouches valikoimasta hallitun SaaS sovellusluettelo.

**Jos haluat lisätä eTouches valikoimasta, toimi seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen portal**, valitse vasemmassa siirtymisruudussa.

    ![Active Directory][1]
2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset][2]

4. Valitse sivun alareunassa **Lisää** .

    ![Sovellukset][3]

5. Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Sovellukset][4]

6. Kirjoita hakuruutuun **eTouches**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_01.png)

7. Valitse tulosruudussa **eTouches**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yksittäinen määrittäminen ja testaus Azure AD-on

Tässä osassa Määritä ja testaa Azure AD kertakirjautumisen eTouches nimeltä "Britta Simon" testikäyttäjän perusteella.

Kertakirjautumisen toimimaan Azure AD on tiedettävä käyttäjälle on eTouches vastaavaan käyttäjän Azure AD. Toisin sanoen suhteen Azure AD-käyttäjä ja Aiheeseen liittyvät eTouches-linkki on vahvistettava.

Linkki-yhteys on muodostettu määrittämällä **käyttäjänimi** arvo arvoksi eTouches **käyttäjänimi** Azure AD.

Määrittäminen ja testaaminen Azure AD kertakirjautumisen eTouches kanssa, sinun on tehtävä seuraavat hakukyselyn:

1. **[Azure AD määrittäminen Single Sign](#configuring-azure-ad-single-sign-on)** - käyttäjät voivat käyttää tätä toimintoa.
2. **[Käyttäjän testata luominen Azure AD](#creating-an-azure-ad-test-user)** - Testaa Azure AD kertakirjautumisen Britta Simon kanssa.
3. **[Luomisesta eTouches testata käyttäjän](#creating-a-predictix-price-reporting-test-user)** - on vastaavaan Britta Simon, valitse eTouches, jotka on linkitetty hän Azure AD-esittäminen.
4. **[Käyttäjän testata määrittäminen Azure AD](#assigning-the-azure-ad-test-user)** - Britta Simon käyttämään Azure AD kertakirjautumisen käyttöön.
5. **[Testaus Single Sign](#testing-single-sign-on)** - voit tarkistaa, toimiiko määritykset.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD kertakirjautumisen määrittäminen

Tässä osassa Azure AD kertakirjautumisen perinteinen portaalissa ottaminen käyttöön ja määritä kertakirjautumisen eTouches-sovelluksessa.

eTouches sovelluksen odottaa SAML vahvistukset tietyssä muodossa. Määritä seuraavat saatavat tämän sovelluksen. Voit hallita näitä määritteitä arvot sovelluksen **"Atrribute"** -välilehdestä. Seuraavassa näyttökuvassa näkyy esimerkki tämän. 

![Kertakirjautumisen määrittäminen](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_07.png) 

**Määrittää Azure AD kertakirjautumisen eTocuhes, toimimalla seuraavasti:**


1. Valitse Azure perinteinen portaalissa **eTouches** sovelluksen integrointi-sivun yläreunassa valikosta **määritteet**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-etouches-tutorial/tutorial_general_80.png) 


2. **SAML-tunnuksen määritteet** -valintaikkunassa kullekin riville seuraavassa taulukossa näkyvät toimimalla seuraavasti:

  	| Määritteen nimi | Määritteen arvo |
  	| --- | --- |    
  	| Sähköpostin | User.Mail |

    a. Valitse **Lisää käyttäjä Attribure** -valintaikkunan avaaminen **Lisää käyttäjä-määrite** .

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-etouches-tutorial/tutorial_general_81.png) 


    b. Kirjoita määritteen nimi näkyy kyseisen rivin **Attrubute nimi** -tekstiruutuun.

    c-näppäinyhdistelmää. **Määritteen arvo** -luettelosta selsect kyseisen rivin määritteen arvo.

    d. Valitse **Valmis**.  
    

3. Valitse perinteinen-portaalissa integrointi **eTouches** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen Single Sign** -valintaikkunan avaaminen.
     
    ![Kertakirjautumisen määrittäminen][6] 

4. **Miten käyttäjät voivat kirjautua eTouches haluat käyttää** sivulla Valitse **Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_03.png) 

5. Valitse **Määritä sovelluksen asetukset** -sivulla toimimalla seuraavasti:

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_04.png) 

    a. Kirjoita **Merkki-URL** -tekstiruutuun URL-osoite käyttämä Sign käyttämällä seuraavaa kaavaa eTouches sovelluksen käyttäjille: **https://www.eiseverywhere.com/saml/accounts/?sso&accountid=\<accountid\>**.
    
    b. Valitse **Seuraava**
 
6. Valitse **Määritä kertakirjautumisen osoitteessa eTouches** -sivulla toimimalla seuraavasti:

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_05.png)

    a. Valitse **Lataa metatiedot**ja tallenna sitten tiedosto tietokoneesta.

    b. Valitse **Seuraava**.


7. Saat määritetty sovelluksen SSO-suorittaa eTouches-sovelluksessa seuraavasti:

    a. Kirjaudu **eTouches** -sovelluksesta valitsemalla järjestelmänvalvojan oikeudet.
    
    b. Siirry **SAML** -määritys

    c-näppäinyhdistelmää. **Yleisasetukset** -kohdassa Liitä tekstiruutuun Azure AD-Federation metatietojen sisältöä.

    d. Valitse **Tallenna ja pidä** -painike

    e. Napsauta SAML metatiedot-osassa **Päivityksen metatiedot** -painiketta. 

    f. Tämä sivu avautuu ja suorittaa SSO. Kun SSO toimii sitten voit voit määrittää käyttäjänimi

    g. Valitse **Username** -kenttä **emailaddress** alla olevassa kuvassa esitetyllä tavalla. 

    h. Kopioi **SSO URL-osoite ja ACS** arvo ja sijoita se olevaan Azure AD sovelluksen kokoonpanon ohjatun Kirjaudu URL-tekstiruutuun.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_06.png)

8. Perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Seuraava**.
    
    ![Azure AD-Single Sign-On][10]

9. Valitse **Sign Vahvista** -sivulla **Valmis**.  
    
 
    ![Azure AD-Single Sign-On][11]


### <a name="creating-an-azure-ad-test-user"></a>Azure AD-testikäyttäjän luominen
Tässä osassa voit luoda testikäyttäjän kutsutaan Britta Simon perinteinen portaalissa.


![Luo Azure AD-käyttäjä][20]

**Voit luoda testikäyttäjän Azure AD-toimimalla seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen portal**, valitse vasemmassa siirtymisruudussa.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-etouches-tutorial/create_aaduser_09.png) 

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Ylä-valikossa käyttäjäluettelon, näkyviin napsauttamalla **käyttäjät**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-etouches-tutorial/create_aaduser_03.png) 

4. Avaa **Lisää käyttäjä** -valintaikkunan alareunassa työkalurivillä, valitse **Lisää käyttäjä**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-etouches-tutorial/create_aaduser_04.png) 

5. Suorita valintaikkunan **Kerro kyseisen käyttäjän tietoja** -sivulla seuraavat toimet:  ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-etouches-tutorial/create_aaduser_05.png) 

    a. Käyttäjänä tyyppi Valitse uusi käyttäjä organisaatiossa.

    b. Kirjoita käyttäjänimi- **tekstiruutuun** **BrittaSimon**.

    c-näppäinyhdistelmää. Valitse **Seuraava**.

6.  Suorita valintaikkunan **Käyttäjäprofiili** -sivulla seuraavat toimet: ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-etouches-tutorial/create_aaduser_06.png) 

    a. Kirjoita **Ensimmäinen nimi** -tekstiruutuun **Britta**.  

    b. Kirjoita **Sukunimi** -tekstiruutuun tyyppi- **Simon**.

    c-näppäinyhdistelmää. Kirjoita **Näyttönimi** -tekstiruutuun **Britta Simon**.

    d. Valitse **rooli** -luettelosta **käyttäjä**.

    e. Valitse **Seuraava**.

7. Valitse **Hae tilapäinen salasana** -valintaikkunan sivulla **Luo**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-etouches-tutorial/create_aaduser_07.png) 

8. **Hae tilapäinen salasana** -valintaikkunan sivulla toimimalla seuraavasti:

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-etouches-tutorial/create_aaduser_08.png) 

    a. Kirjoita **Uusi salasana**arvo muistiin.

    b. Valitse **Valmis**.   



### <a name="creating-an-etouches-test-user"></a>ETouches-testikäyttäjän luominen

Tässä osassa nimeltä Britta Simon eTouches käyttäjän luominen. Toimi eTouches tukihenkilöstöösi voit lisätä käyttäjiä eTouches-ympäristössä.


### <a name="assigning-the-azure-ad-test-user"></a>Azure AD-testikäyttäjän määrittäminen

Tässä osassa voit ottaa käyttöön Britta Simon käyttämään Azure kertakirjautumisen myöntämällä eTouches käyttöoikeus.

![Määrittää käyttäjälle][200] 

**Jos haluat määrittää Britta Simon eTouches, toimi seuraavasti:**

1. Perinteinen-portaalissa Avaa sovellukset-näkymässä kansio-näkymässä, valitse **sovellukset** yläreunan-valikossa.

    ![Määrittää käyttäjälle][201] 

2. Valitse sovellukset-luettelosta **eTouches**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-etouches-tutorial/tutorial_etouches_50.png) 

3. Valitse ylä-valikossa Valitse **käyttäjiä**.

    ![Määrittää käyttäjälle][203]

4. Valitse käyttäjät-luettelosta **Britta Simon**.

5. Napsauta työkalurivin alaosassa **määrittäminen**.

    ![Määrittää käyttäjälle][205]


### <a name="testing-single-sign-on"></a>Kertakirjautumisen testaaminen

Tässä osassa voit testata Azure AD yksittäisen Sign-asetusten käyttäminen Access-paneeli.

Access-ruudussa eTouches-ruutua napsauttaessasi sinun pitäisi saada automaattisesti kirjautunut-on eTouches sovelluksen.


## <a name="additional-resources"></a>Lisäresursseja

* [Luettelo siitä, miten voit integroida SaaS sovellusten Azure Active Directory-opetusohjelmat](active-directory-saas-tutorial-list.md)
* [Mikä on sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-etouches-tutorial/tutorial_general_205.png
