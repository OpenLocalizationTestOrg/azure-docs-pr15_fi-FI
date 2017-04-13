<properties
    pageTitle="Opetusohjelma: Azure Active Directory integrointi Liitutaulu tietoja | Microsoft Azure"
    description="Opettele määrittämään kertakirjautumisen Azure Active Directory- ja lue Liitutaulu välillä."
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
    ms.date="09/29/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-blackboard-learn"></a>Opetusohjelma: Azure Active Directory integrointi Liitutaulu tietoja

Tässä opetusohjelmassa kerrotaan, miten voit integroida Azure Active Directory (Azure AD) Liitutaulu tietoja.

Liitutaulu tietoja integraation Azure AD, jonka avulla seuraavat edut:

- Voit hallita Azure AD, kenellä on oikeus Liitutaulu tietoja
- Voit ottaa käyttöön automaattisesti Hae kirjautunut sisään Liitutaulu lisätietoja (kertakirjautumisen) käyttäjille heidän Azure AD-tilien kanssa
- Voit hallita asiakkaiden yhdessä keskitetyssä sijainnissa - Azure perinteinen portal

Jos haluat tietää tarkempia tietoja SaaS sovelluksen integrointi Azure AD-kohdassa [mitä sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Edellytykset

Määrittää Azure AD-integroinnin Liitutaulu tietoja, tarvitset seuraavat:

- Azure AD-tilaus
- Liitutaulu tietoja Cloud-ympäristössä single-merkki käytössä-tilauksessa


> [AZURE.NOTE] Voit esikatsella vaiheet Tässä opetusohjelmassa, Emme suosittele käyttämällä tuotantoympäristössä.


Voit esikatsella vaiheet Tässä opetusohjelmassa-noudattamalla näitä suosituksia:

- Älä käytä tuotantoympäristössä, ellei se on tarpeen.
- Jos sinulla ei ole kokeiluversion Azure AD-ympäristössä, voit hankkia yksi kuukausi kokeiluversion [tähän](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Skenaarion kuvaus
Tässä opetusohjelmassa testaat Azure AD kertakirjautumisen testiympäristössä.

Tässä opetusohjelmassa kuvatut skenaarion kuuluu kaksi tärkeimmät rakenneosien:

1. Liitutaulu tietoja lisääminen valikoimasta
2. Yksittäinen määrittäminen ja testaus Azure AD-on


## <a name="adding-blackboard-learn-from-the-gallery"></a>Liitutaulu tietoja lisääminen valikoimasta
Voit määrittää, lue Liitutaulu integroida Azure AD-haluat lisääminen Liitutaulu tietoja valikoimasta hallitun SaaS sovellusluettelo.

**Jos haluat lisätä valikoimasta Liitutaulu tietoja, toimi seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen portal**, valitse vasemmassa siirtymisruudussa.

    ![Active Directory][1]
2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset][2]

4. Valitse sivun alareunassa **Lisää** .

    ![Sovellukset][3]

5. Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Sovellukset][4]

6. Kirjoita hakuruutuun **Liitutaulu tietoja**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_blackboardlearn_01.png)

7. Valitse tulosruudussa **Liitutaulu tietoja**ja valitse **Valmis** , voit lisätä sovelluksen.
    
    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_blackboardlearn_06.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yksittäinen määrittäminen ja testaus Azure AD-on
Tässä osassa Määritä ja testaa Azure AD kertakirjautumisen Liitutaulu tietoja nimeltä "Britta Simon" testikäyttäjän perusteella.

Kertakirjautumisen toimimaan Azure AD on tiedettävä käyttäjälle on vastaavaan käyttäjän Liitutaulu lisätietoja Azure AD. Toisin sanoen linkki suhteen Azure AD-käyttäjä ja Aiheeseen liittyvät-Liitutaulu tietoja on vahvistettava.

Linkki-yhteys on muodostettu määrittämällä **käyttäjänimi** arvo arvoksi Liitutaulu tietoja **käyttäjänimi** Azure AD.

Määrittäminen ja testaaminen Azure AD kertakirjautumisen kanssa Liitutaulu tietoja, sinun on tehtävä seuraavat hakukyselyn:

1. **[Azure AD määrittäminen Single Sign](#configuring-azure-ad-single-sign-on)** - käyttäjät voivat käyttää tätä toimintoa.
2. **[Käyttäjän testata luominen Azure AD](#creating-an-azure-ad-test-user)** - Testaa Azure AD kertakirjautumisen Britta Simon kanssa.
3. **[Käyttäjän luominen Liitutaulu tietoja testata](#creating-a-blackboard-learn-test-user)** - tapahtumista Britta Simon on Liitutaulu tietoja, jotka on linkitetty hän Azure AD-esittäminen.
4. **[Käyttäjän testata määrittäminen Azure AD](#assigning-the-azure-ad-test-user)** - Britta Simon käyttämään Azure AD kertakirjautumisen käyttöön.
5. **[Testaus Single Sign](#testing-single-sign-on)** - voit tarkistaa, toimiiko määritykset.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD-Single Sign-On määrittäminen

Tässä osassa Azure AD kertakirjautumisen perinteinen portaalissa käyttöön ja määritä kertakirjautumisen Liitutaulu Lisätietoja sovelluksen.

Liitutaulu Lue sovelluksen odottaa SAML vahvistukset tietyssä muodossa. Määritä seuraavat saatavat tämän sovelluksen. Voit hallita näitä määritteitä arvot sovelluksen **"Atrribute"** -välilehdestä. Seuraavassa näyttökuvassa näkyy esimerkki tämän. 

![Kertakirjautumisen määrittäminen](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_blackboardlearn_51.png) 

**Määrittää Azure AD kertakirjautumisen Liitutaulu tietoja, toimimalla seuraavasti:**


1. Azure-perinteinen portaalissa-sivulla **Liitutaulu tietoja** sovelluksen integrointi valikon päälle Valitse **määritteet**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_80.png) 


1. Valitse **SAML suojaustunnuksen määritteet** -valintaikkunassa kullekin riville seuraavassa taulukossa näkyvät suorittamalla seuraavat vaiheet: emme on kartta Userprincipalname tähän yksilöllinen määrite, mutta voit yhdistää ne joka erottaa yksilöllisesti käyttäjälle organisaatiossa sopiva ja kartat Liitutaulu Tutustu username-kenttä.

  	| Määritteen nimi | Määritteen arvo |
  	| --- | --- |    
  	| urn:oid:1.3.6.1.4.1.5923.1.1.1.6  | User.UserPrincipalName |
   
 
    a. Valitse **Lisää käyttäjä Attribure** -valintaikkunan avaaminen **Lisää käyttäjä-määrite** .

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_81.png) 


    b. Kirjoita määritteen nimi näkyy kyseisen rivin **Attrubute nimi** -tekstiruutuun.

    c-näppäinyhdistelmää. **Määritteen arvo** -luettelosta selsect kyseisen rivin määritteen arvo.

    d. Valitse **Valmis**.  

2. Valitse perinteinen-portaalissa sivulla **Liitutaulu tietoja** sovelluksen integrointi, **Määritä kertakirjautumisen** **Määrittäminen Single Sign** -valintaikkunan avaaminen.
     
    ![Kertakirjautumisen määrittäminen][6] 

3. **Miten käyttäjät voivat kirjautua Liitutaulu tietoja haluat käyttää** sivulla Valitse **Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_blackboardlearn_03.png) 

4. Valitse **Määritä sovelluksen asetukset** -sivulla toimimalla seuraavasti:

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_blackboardlearn_04.png) 

    a. Kirjoita **Merkki-URL** -tekstiruutuun URL-osoite käyttämä Sign käyttämällä seuraavaa kaavaa Liitutaulu tietoja sovelluksen käyttäjille: **https://\<yrityksen nimi hinnat\>.blackboard.com/**
    
    b. Valitse **Seuraava**
 
5. Valitse **Määritä kertakirjautumisen osoitteessa Liitutaulu tietoja** -sivulla toimimalla seuraavasti:

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_blackboardlearn_05.png)

    a. Valitse **Lataa metatiedot**ja tallenna sitten tiedosto tietokoneesta.

    b. Valitse **Seuraava**.


6. Saat määritetty sovelluksen SSO-Liitutaulu tietoja tukeen ja antaa niille seuraavasti:

    • Ladatut metatiedot


7. Perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Seuraava**.
    
    ![Azure AD-Single Sign-On][10]

8. Valitse **Sign Vahvista** -sivulla **Valmis**.  
 
    ![Azure AD-Single Sign-On][11]


### <a name="creating-an-azure-ad-test-user"></a>Azure AD-testikäyttäjän luominen
Tässä osassa voit luoda testikäyttäjän kutsutaan Britta Simon perinteinen portaalissa.


![Luo Azure AD-käyttäjä][20]

**Voit luoda testikäyttäjän Azure AD-toimimalla seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen portal**, valitse vasemmassa siirtymisruudussa.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-blackboard-learn-tutorial/create_aaduser_09.png) 

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Ylä-valikossa käyttäjäluettelon, näkyviin napsauttamalla **käyttäjät**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-blackboard-learn-tutorial/create_aaduser_03.png) 

4. Avaa **Lisää käyttäjä** -valintaikkunan alareunassa työkalurivillä, valitse **Lisää käyttäjä**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-blackboard-learn-tutorial/create_aaduser_04.png) 

5. Suorita valintaikkunan **Kerro kyseisen käyttäjän tietoja** -sivulla seuraavat toimet:  ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-blackboard-learn-tutorial/create_aaduser_05.png) 

    a. Käyttäjänä tyyppi Valitse uusi käyttäjä organisaatiossa.

    b. Kirjoita käyttäjänimi- **tekstiruutuun** **BrittaSimon**.

    c-näppäinyhdistelmää. Valitse **Seuraava**.

6.  Suorita valintaikkunan **Käyttäjäprofiili** -sivulla seuraavat toimet: ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-blackboard-learn-tutorial/create_aaduser_06.png) 

    a. Kirjoita **Ensimmäinen nimi** -tekstiruutuun **Britta**.  

    b. Kirjoita **Sukunimi** -tekstiruutuun tyyppi- **Simon**.

    c-näppäinyhdistelmää. Kirjoita **Näyttönimi** -tekstiruutuun **Britta Simon**.

    d. Valitse **rooli** -luettelosta **käyttäjä**.

    e. Valitse **Seuraava**.

7. Valitse **Hae tilapäinen salasana** -valintaikkunan sivulla **Luo**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-blackboard-learn-tutorial/create_aaduser_07.png) 

8. **Hae tilapäinen salasana** -valintaikkunan sivulla toimimalla seuraavasti:

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-blackboard-learn-tutorial/create_aaduser_08.png) 

    a. Kirjoita **Uusi salasana**arvo muistiin.

    b. Valitse **Valmis**.   



### <a name="creating-an-blackboard-learn-test-user"></a>Liitutaulu tietoja testi-käyttäjän luominen

Tässä osassa nimeltä Britta Simon Liitutaulu tietoja käyttäjän luominen. 

Liitutaulu Lue sovellus tukee vain kerran käyttäjän valmistelu. Varmista, että olet määrittänyt saatavat ** [Määrittäminen Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on) -kohdassa kuvatulla tavalla**

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD-testikäyttäjän määrittäminen

Tässä osassa voit ottaa käyttöön Britta Simon käyttämään Azure kertakirjautumisen hänen käyttöoikeuksien myöntämistä Liitutaulu tietoja.

![Määrittää käyttäjälle][200] 

**Jos haluat määrittää Britta Simon Liitutaulu tietoja, toimi seuraavasti:**

1. Perinteinen-portaalissa Avaa sovellukset-näkymässä kansio-näkymässä, valitse **sovellukset** yläreunan-valikossa.

    ![Määrittää käyttäjälle][201] 

2. Valitse sovellukset-luettelosta **Liitutaulu tietoja**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-blackboard-learn-tutorial/tutorial_blackboardlearn_50.png) 

3. Valitse ylä-valikossa Valitse **käyttäjiä**.

    ![Määrittää käyttäjälle][203]

4. Valitse käyttäjät-luettelosta **Britta Simon**.

5. Napsauta työkalurivin alaosassa **määrittäminen**.

    ![Määrittää käyttäjälle][205]


### <a name="testing-single-sign-on"></a>Kertakirjautumisen testaaminen

Tässä osassa voit testata Azure AD yksittäisen Sign-asetusten käyttäminen Access-paneeli.

Liitutaulu Lue ohjelmien tuki Access-ruudussa Liitutaulu Lue-ruutu napsauttamalla, hanki automaattisesti kirjautunut sisään Liitutaulu Lue-sovellukseen.


## <a name="additional-resources"></a>Lisäresursseja

* [Luettelo siitä, miten voit integroida SaaS sovellusten Azure Active Directory-opetusohjelmat](active-directory-saas-tutorial-list.md)
* [Mikä on sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-blackboard-learn-tutorial/tutorial_general_205.png
