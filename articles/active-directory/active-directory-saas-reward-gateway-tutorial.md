<properties
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi toisen yhdyskäytävän | Microsoft Azure"
    description="Opettele määrittämään kertakirjautumisen Azure Active Directory ja toisen yhdyskäytävän välillä."
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
    ms.date="10/17/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-reward-gateway"></a>Opetusohjelma: Toisen yhdyskäytävän Azure Active Directory-integrointi

Tässä opetusohjelmassa kerrotaan, miten voit integroida toisen yhdyskäytävän Azure Active Directory (Azure AD).

Azure AD-integraation toisen yhdyskäytävää, jonka avulla seuraavat edut:

- Voit hallita Azure AD, kenellä on oikeus toisen yhdyskäytävän
- Voit ottaa käyttöön automaattisesti Hae kirjautunut sisään toisen Gateway (kertakirjautumisen) käyttäjille heidän Azure AD-tilien kanssa
- Voit hallita asiakkaiden yhdessä keskitetyssä sijainnissa - Azure perinteinen portal

Jos haluat tietää tarkempia tietoja SaaS sovelluksen integrointi Azure AD-kohdassa [mitä sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Edellytykset

Määrittää toisen yhdyskäytävän Azure AD-integrointi, tarvitset seuraavat:

- Azure AD-tilaus
- Toisen yhdyskäytävän single-merkki käytössä-tilauksessa


> [AZURE.NOTE] Voit esikatsella vaiheet Tässä opetusohjelmassa, Emme suosittele käyttämällä tuotantoympäristössä.


Voit esikatsella vaiheet Tässä opetusohjelmassa-noudattamalla näitä suosituksia:

- Älä käytä tuotantoympäristössä, ellei se on tarpeen.
- Jos sinulla ei ole kokeiluversion Azure AD-ympäristössä, voit hankkia yksi kuukausi kokeiluversion [tähän](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Skenaarion kuvaus
Tässä opetusohjelmassa testaat Azure AD kertakirjautumisen testiympäristössä.

Tässä opetusohjelmassa kuvatut skenaarion kuuluu kaksi tärkeimmät rakenneosien:

1. Toisen yhdyskäytävän lisäämistä valikoimasta
2. Yksittäinen määrittäminen ja testaus Azure AD-on


## <a name="adding-reward-gateway-from-the-gallery"></a>Toisen yhdyskäytävän lisäämistä valikoimasta
Voit määrittää toisen yhdyskäytävän integroida Azure AD-haluat lisätä toisen yhdyskäytävän valikoimasta hallitun SaaS sovellusluettelo.

**Jos haluat lisätä toisen yhdyskäytävän valikoimasta, toimi seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen portal**, valitse vasemmassa siirtymisruudussa.

    ![Active Directory][1]
2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset][2]

4. Valitse sivun alareunassa **Lisää** .

    ![Sovellukset][3]

5. Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Sovellukset][4]

6. Kirjoita hakuruutuun **Toisen yhdyskäytävä**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-reward-gateway-tutorial/tutorial_rewardgateway_01.png)

7. Valitse tulokset-ruudun Valitse **Toisen yhdyskäytävän**ja valitse sitten **Valmis** , voit lisätä sovelluksen.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-reward-gateway-tutorial/tutorial_rewardgateway_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yksittäinen määrittäminen ja testaus Azure AD-on
Tässä osassa Määritä ja testaa Azure AD kertakirjautumisen toisen yhdyskäytävän nimeltä "Britta Simon" testikäyttäjän perusteella.

Kertakirjautumisen toimimaan Azure AD on tiedettävä käyttäjälle on toisen yhdyskäytävän vastaavaan käyttäjän Azure AD. Toisin sanoen suhteen Azure AD-käyttäjä ja Aiheeseen liittyvät toisen yhdyskäytävä-linkki on vahvistettava.

Linkki-yhteys on muodostettu määrittämällä **käyttäjänimi** arvo arvoksi toisen yhdyskäytävän **käyttäjänimi** Azure AD.

Määrittäminen ja testaaminen Azure AD kertakirjautumisen toisen Gatewayn, sinun on tehtävä seuraavat hakukyselyn:

1. **[Azure AD määrittäminen Single Sign](#configuring-azure-ad-single-sign-on)** - käyttäjät voivat käyttää tätä toimintoa.
2. **[Käyttäjän testata luominen Azure AD](#creating-an-azure-ad-test-user)** - Testaa Azure AD kertakirjautumisen Britta Simon kanssa.
3. **[Luoda toisen yhdyskäytävän testata käyttäjän](#creating-a-reward-gateway-test-user)** - tapahtumista Britta Simon ovat toisen yhdyskäytävään, joka on linkitetty hän Azure AD esitys.
4. **[Käyttäjän testata määrittäminen Azure AD](#assigning-the-azure-ad-test-user)** - Britta Simon käyttämään Azure AD kertakirjautumisen käyttöön.
5. **[Testaus Single Sign](#testing-single-sign-on)** - voit tarkistaa, toimiiko määritykset.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD kertakirjautumisen määrittäminen

Tässä osassa Azure AD kertakirjautumisen perinteinen portaalissa ottaminen käyttöön ja määritä kertakirjautumisen toisen yhdyskäytävä-sovelluksessa.


**Jos haluat määrittää toisen Gatewayn Azure AD kertakirjautumisen, toimimalla seuraavasti:**

1. Valitse perinteinen-portaalissa integrointi **Toisen yhdyskäytävän** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen Single Sign** -valintaikkunan avaaminen.
     
    ![Kertakirjautumisen määrittäminen][6] 

2. **Miten Haluatko, että käyttäjät voivat kirjautua toisen yhdyskäytävä** -sivulla Valitse **Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-reward-gateway-tutorial/tutorial_rewardgateway_03.png) 

3. Valitse **Määritä sovelluksen asetukset** -sivulla toimimalla seuraavasti:

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-reward-gateway-tutorial/tutorial_rewardgateway_04.png) 

    a. Kirjoita **Tunniste URL** -tekstiruutuun URL-osoite käyttäjien Sign toisen yhdyskäytävän sovelluksen käyttämällä seuraavaa kaavaa: 
   
  	| Tunniste URL-osoite |
  	| --- |
  	| `https://<company name>.rewardgateway.com/` |
  	| `https://<company name>.rewardgateway.co.uk/` |
  	| `https://<company name>.rewardgateway.co.nz/` |
  	| `https://<company name>.rewardgateway.com.au/` |


    
    b. Kirjoita **Vastaus URL** -tekstiruutuun URL-osoite, käyttämällä seuraavaa kaavaa: 
        
  	| Vastaa URL-osoite |
  	| --- |
  	| `https://<company name>.rewardgateway.com/Authentication/EndLogin?idp=<Unique Id>` |
  	| `https://<company name>.rewardgateway.co.uk/Authentication/EndLogin?idp=<Unique Id>` |
  	| `https://<company name>.rewardgateway.co.nz/Authentication/EndLogin?idp=<Unique Id>` |
  	| `https://<company name>.rewardgateway.com.au/Authentication/EndLogin?idp=<Unique Id>` |


    c-näppäinyhdistelmää. Valitse **Seuraava**
 
4. Valitse **Määritä kertakirjautumisen osoitteessa toisen yhdyskäytävä** -sivulla toimimalla seuraavasti:

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-reward-gateway-tutorial/tutorial_rewardgateway_05.png)

    a. Valitse **Lataa metatiedot**ja tallenna sitten tiedosto tietokoneesta.


5. Saat määritetty sovelluksen SSO-yhteyttä toisen yhdyskäytävän [tekniseen tukeen](mailTo:clientsupport@rewardgateway.com) ja antaa niille seuraavasti:

    • Ladatut **metatiedot**

6. Perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Seuraava**.
    
    ![Azure AD-Single Sign-On][10]

7. Valitse **Sign Vahvista** -sivulla **Valmis**.  
 
    ![Azure AD-Single Sign-On][11]


### <a name="creating-an-azure-ad-test-user"></a>Azure AD-testikäyttäjän luominen
Tässä osassa voit luoda testikäyttäjän kutsutaan Britta Simon perinteinen portaalissa.


![Luo Azure AD-käyttäjä][20]

**Voit luoda testikäyttäjän Azure AD-toimimalla seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen portal**, valitse vasemmassa siirtymisruudussa.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-reward-gateway-tutorial/create_aaduser_09.png) 

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Ylä-valikossa käyttäjäluettelon, näkyviin napsauttamalla **käyttäjät**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-reward-gateway-tutorial/create_aaduser_03.png) 

4. Avaa **Lisää käyttäjä** -valintaikkunan alareunassa työkalurivillä, valitse **Lisää käyttäjä**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-reward-gateway-tutorial/create_aaduser_04.png) 

5. Suorita valintaikkunan **Kerro kyseisen käyttäjän tietoja** -sivulla seuraavat toimet:  ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-reward-gateway-tutorial/create_aaduser_05.png) 

    a. Käyttäjänä tyyppi Valitse uusi käyttäjä organisaatiossa.

    b. Kirjoita käyttäjänimi- **tekstiruutuun** **BrittaSimon**.

    c-näppäinyhdistelmää. Valitse **Seuraava**.

6.  Suorita valintaikkunan **Käyttäjäprofiili** -sivulla seuraavat toimet: ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-reward-gateway-tutorial/create_aaduser_06.png) 

    a. Kirjoita **Ensimmäinen nimi** -tekstiruutuun **Britta**.  

    b. Kirjoita **Sukunimi** -tekstiruutuun tyyppi- **Simon**.

    c-näppäinyhdistelmää. Kirjoita **Näyttönimi** -tekstiruutuun **Britta Simon**.

    d. Valitse **rooli** -luettelosta **käyttäjä**.

    e. Valitse **Seuraava**.

7. Valitse **Hae tilapäinen salasana** -valintaikkunan sivulla **Luo**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-reward-gateway-tutorial/create_aaduser_07.png) 

8. **Hae tilapäinen salasana** -valintaikkunan sivulla toimimalla seuraavasti:

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-reward-gateway-tutorial/create_aaduser_08.png) 

    a. Kirjoita **Uusi salasana**arvo muistiin.

    b. Valitse **Valmis**.   



### <a name="creating-an-reward-gateway-test-user"></a>Toisen yhdyskäytävä-testikäyttäjän luominen

Tässä osassa kutsutaan Britta Simon yhdyskäytävän toisen käyttäjän luominen. Toimi kanssa toisen yhdyskäytävän [tekniseen tukeen](mailTo:clientsupport@rewardgateway.com) voit lisätä käyttäjiä toisen yhdyskäytävä-ympäristössä.


### <a name="assigning-the-azure-ad-test-user"></a>Azure AD-testikäyttäjän määrittäminen

Tässä osassa voit ottaa käyttöön Britta Simon käyttämään Azure kertakirjautumisen hänen käyttöoikeuksien myöntämistä toisen yhdyskäytävä.

![Määrittää käyttäjälle][200] 

**Jos haluat määrittää toisen yhdyskäytävän Britta Simon, toimi seuraavasti:**

1. Perinteinen-portaalissa Avaa sovellukset-näkymässä kansio-näkymässä, valitse **sovellukset** yläreunan-valikossa.

    ![Määrittää käyttäjälle][201] 

2. Valitse sovellukset-luettelosta **Toisen yhdyskäytävä**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-reward-gateway-tutorial/tutorial_rewardgateway_50.png) 

3. Valitse ylä-valikossa Valitse **käyttäjiä**.

    ![Määrittää käyttäjälle][203]

4. Valitse käyttäjät-luettelosta **Britta Simon**.

5. Napsauta työkalurivin alaosassa **määrittäminen**.

    ![Määrittää käyttäjälle][205]


### <a name="testing-single-sign-on"></a>Kertakirjautumisen testaaminen

Tässä osassa voit testata Azure AD yksittäisen Sign-asetusten käyttäminen Access-paneeli.

Access-ruudussa toisen yhdyskäytävä-ruutua napsauttaessasi voit olisi Hae automaattisesti kirjautunut sisään toisen yhdyskäytävän sovelluksen.


## <a name="additional-resources"></a>Lisäresursseja

* [Luettelo siitä, miten voit integroida SaaS sovellusten Azure Active Directory-opetusohjelmat](active-directory-saas-tutorial-list.md)
* [Mikä on sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-reward-gateway-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-reward-gateway-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-reward-gateway-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-reward-gateway-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-reward-gateway-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-reward-gateway-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-reward-gateway-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-reward-gateway-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-reward-gateway-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-reward-gateway-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-reward-gateway-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-reward-gateway-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-reward-gateway-tutorial/tutorial_general_205.png
