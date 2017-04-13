<properties
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi Tangoe komento Premium Mobilella | Microsoft Azure"
    description="Opettele määrittämään kertakirjautumisen Azure Active Directory- ja Tangoe komento Premium Mobile välillä."
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


# <a name="tutorial-azure-active-directory-integration-with-tangoe-command-premium-mobile"></a>Opetusohjelma: Tangoe komento Premium Mobile Azure Active Directory-integrointi

Tässä opetusohjelmassa kerrotaan, miten voit integroida Tangoe komento Premium Mobile Azure Active Directory (Azure AD).

Azure AD-integraation Tangoe komento Premium Mobile, jonka avulla seuraavat edut:

- Voit hallita Azure AD, jolla on pääsy Tangoe komento Premium Mobile
- Voit ottaa käyttöön automaattisesti Hae kirjautunut sisään Tangoe komento Premium Mobileen (kertakirjautumisen) käyttäjille heidän Azure AD-tilien kanssa
- Voit hallita asiakkaiden yhdessä keskitetyssä sijainnissa - Azure perinteinen portal

Jos haluat tietää tarkempia tietoja SaaS sovelluksen integrointi Azure AD-kohdassa [mitä sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Edellytykset

Azure AD-integroinnin määrittäminen Tangoe komento Premium Mobilella, tarvitset seuraavat:

- Azure tilauksen
- Tangoe komento Premium Mobile single-merkki käytössä-tilauksessa


> [AZURE.NOTE] Voit esikatsella vaiheet Tässä opetusohjelmassa, Emme suosittele käyttämällä tuotantoympäristössä.


Voit esikatsella vaiheet Tässä opetusohjelmassa-noudattamalla näitä suosituksia:

- Älä käytä tuotantoympäristössä, ellei se on tarpeen.
- Jos sinulla ei ole kokeiluversion Azure AD-ympäristössä, voit hankkia yksi kuukausi kokeiluversion [tähän](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Skenaarion kuvaus
Tässä opetusohjelmassa testaat Azure AD kertakirjautumisen testiympäristössä. 

Tässä opetusohjelmassa kuvatut skenaarion kuuluu kaksi tärkeimmät rakenneosien:

1. Tangoe komento Premium Mobile lisääminen valikoimasta
2. Yksittäinen määrittäminen ja testaus Azure AD-on


## <a name="adding-tangoe-command-premium-mobile-from-the-gallery"></a>Tangoe komento Premium Mobile lisääminen valikoimasta
Voit määrittää Tangoe komento Premium Mobile integroida Azure AD-haluat lisätä Tangoe komento Premium Mobile valikoimasta hallitun SaaS sovellusluettelo.

**Voit lisätä Tangoe komento Premium Mobile valikoimasta, toimi seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen portal**, valitse vasemmassa siirtymisruudussa. 

    ![Active Directory][1]

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset][2]

4. Valitse sivun alareunassa **Lisää** .

    ![Sovellukset][3]

5. Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Sovellukset][4]

6. Kirjoita hakuruutuun **Tangoe komento Premium Mobile**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_01.png)

7. Valitse tulosruudussa **Tangoe komento Premium Mobile**ja valitse **Valmis** , voit lisätä sovelluksen.

![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_02.png)




##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yksittäinen määrittäminen ja testaus Azure AD-on
Tässä osassa Määritä ja testaa Azure AD kertakirjautumisen Tangoe komento Premium Mobile test nimeltä "Britta Simon" käyttäjään perustuva.

Kertakirjautumisen toimimaan Azure AD on tiedettävä käyttäjälle on vastaavaan käyttäjän Tangoe komento Premium Mobilessa Azure AD. Toisin sanoen linkki suhteen Azure AD-käyttäjä ja Aiheeseen liittyvät Tangoe komento Premium Mobilessa on vahvistettava.

Linkki-yhteys on muodostettu määrittämällä **käyttäjänimi** arvo arvoksi **käyttäjänimi** Tangoe komento Premium Mobilessa Azure AD.

Määrittäminen ja testaaminen Azure AD kertakirjautumisen Tangoe komento Premium Mobilella, on täytettävä seuraavat hakukyselyn:

1. **[Azure AD määrittäminen Single Sign](#configuring-azure-ad-single-single-sign-on)** - käyttäjät voivat käyttää tätä toimintoa.
2. **[Käyttäjän testata luominen Azure AD](#creating-an-azure-ad-test-user)** - Testaa Azure AD kertakirjautumisen Britta Simon kanssa.
4. **[Käyttäjän luominen Tangoe komento Premium-Mobile testata](#creating-an-tangoe-test-user)** - on tapahtumista Britta Simon Tangoe komento Premium Mobile, joka on linkitetty hän Azure AD esitys.
5. **[Käyttäjän testata määrittäminen Azure AD](#assigning-the-azure-ad-test-user)** - Britta Simon käyttämään Azure AD kertakirjautumisen käyttöön.
5. **[Testaus Single Sign](#testing-single-sign-on)** - voit tarkistaa, toimiiko määritykset.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD-Single Sign-On määrittäminen

Tässä osassa Azure AD kertakirjautumisen perinteinen portaalissa käyttöön ja määrittäminen kertakirjautumisen Tangoe komento Premium Mobile-sovelluksen.


**Voit määrittää Azure AD kertakirjautumisen Tangoe komento Premium Mobilella toimimalla seuraavasti:**

1. Valitse perinteinen-portaalissa **Tangoe komento Premium Mobile** -sovellus integrointi-sivulla **Määritä kertakirjautumisen** **Määrittäminen Single Sign** -valintaikkunan avaaminen.

    ![Kertakirjautumisen määrittäminen][6]


2. **Miten käyttäjät voivat kirjautua Tangoe komento Premium Mobile haluat käyttää** sivulla Valitse **Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_03.png) 


3. Valitse **Määritä sovelluksen asetukset** -sivulla toimimalla seuraavasti:
 
    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_04.png) 


    a. Kirjoita **Merkki-URL** -tekstiruutuun URL-osoite käyttämä Sign käyttämällä seuraavaa kaavaa Tangoe komento Premium Mobile sovelluksen käyttäjille: **"https://sso.tangoe.com/sp/startSSO.ping?PartnerIdpId=\<vuokraajan myöntäjä\>& kohde =\<kohteen URL-Osoitteen\>"**.

    b. Kirjoita **Vastaus URL** -tekstiruutuun URL-Osoitetta seuraavaa kaavaa: **"https://sso.tangoe.com/sp/ACS.saml2"**

    > [AZURE.NOTE]  Jos et tiedä oikeat arvot URL-osoitteet, voit käyttää suuremmat arvot paikkamerkit ja pyydä Tangoe asiakastukea oikeat arvot Liitä.


4. Valitse **Määritä kertakirjautumisen osoitteessa Tangoe komento Premium Mobile** -sivulla toimimalla seuraavasti:
 
    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_05.png) 

    a. Valitse **Lataa metatiedot**ja tallenna sitten tiedosto tietokoneesta.

    b. Valitse **Seuraava**.


5.  Saat määritetty sovelluksen SSO-Ota yhteyttä Tangoe asiakastukea liittää ja kirjoita seuraavat tiedot:


    - Ladatun metatieto-tiedosto
    - **Myöntäjän URL-osoite**
    - **SAML SSO URL-osoite**
    - **Kirjaudu ulos yhden palvelun URL-osoite**


  
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

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-tangoe-tutorial/create_aaduser_09.png) 

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Ylä-valikossa käyttäjäluettelon, näkyviin napsauttamalla **käyttäjät**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-tangoe-tutorial/create_aaduser_03.png) 

4. Avaa **Lisää käyttäjä** -valintaikkunan alareunassa työkalurivillä, valitse **Lisää käyttäjä**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-tangoe-tutorial/create_aaduser_04.png)


5. Tee valintaikkunassa **Kerro kyseisen käyttäjän tietoja** -sivulla seuraavasti:

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-tangoe-tutorial/create_aaduser_05.png) 

    a. Käyttäjänä tyyppi Valitse uusi käyttäjä organisaatiossa.

    b. Kirjoita käyttäjänimi- **tekstiruutuun** **BrittaSimon**.

    c-näppäinyhdistelmää. Valitse **Seuraava**.

6.  Suorita **Käyttäjäprofiili** -sivulla seuraavat toimet:

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-tangoe-tutorial/create_aaduser_06.png) 

    a. Kirjoita **Ensimmäinen nimi** -tekstiruutuun **Britta**.  

    b. Kirjoita **Sukunimi** -tekstiruutuun tyyppi- **Simon**.

    c-näppäinyhdistelmää. Kirjoita **Näyttönimi** -tekstiruutuun **Britta Simon**.

    d. Valitse **rooli** -luettelosta **käyttäjä**.

    e. Valitse **Seuraava**.

7. Valitse **Hae tilapäinen salasana** -valintaikkunan sivulla **Luo**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-tangoe-tutorial/create_aaduser_07.png) 

8. **Hae tilapäinen salasana** -valintaikkunan sivulla toimimalla seuraavasti:
 
    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-tangoe-tutorial/create_aaduser_08.png) 

    a. Kirjoita **Uusi salasana**arvo muistiin.

    b. Valitse **Valmis**.   



### <a name="creating-an-tangoe-command-premium-mobile-test-user"></a>Tangoe komento Premium Mobile testikäyttäjän luominen

Tässä osassa nimeltä Britta Simon Tangoe komento Premium Mobilessa käyttäjän luominen. Tangoe komento Premium Mobile-sovellus on kaikkien käyttäjien valmisteltu ennen kuin teet kertakirjautuminen-sovelluksessa. Ota niin työ Tangoe asiakkaan tuki Liitä nämä käyttäjät valmistelu sovellukseen kanssa. 


> [AZURE.NOTE] Jos haluat luoda käyttäjän manuaalisesti tai erän käyttäjien käytettävissä, sinun on Tangoe komento Premium Mobile-tukeen.


### <a name="assigning-the-azure-ad-test-user"></a>Azure AD-testikäyttäjän määrittäminen

Tässä osassa voit ottaa käyttöön Britta Simon käyttämään Azure kertakirjautumisen hänen käyttöoikeuden myöntämisestä Tangoe komento Premium Mobile.

![Määrittää käyttäjälle][200] 

**Jos haluat määrittää Britta Simon Tangoe komento Premium Mobile, toimi seuraavasti:**

1. Perinteinen-portaalissa Avaa sovellukset-näkymässä kansio-näkymässä, valitse **sovellukset** yläreunan-valikossa.

![Määrittää käyttäjälle][201] 

2. Valitse sovellukset-luettelosta **Tangoe komento Premium Mobile**.

![Kertakirjautumisen määrittäminen](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_50.png) 

1. Valitse ylä-valikossa Valitse **käyttäjiä**.

![Määrittää käyttäjälle][203] 

1. Valitse käyttäjät-luettelosta **Britta Simon**.

2. Napsauta työkalurivin alaosassa **määrittäminen**.

![Määrittää käyttäjälle][205]



### <a name="testing-single-sign-on"></a>Kertakirjautumisen testaaminen

Tässä osassa voit testata Azure AD yksittäisen Sign-asetusten käyttäminen Access-paneeli.

Access-ruudussa Tangoe komento Premium Mobile-ruutua napsauttaessasi sinun olisi Hae automaattisesti kirjautunut käyttöön Tangoe komento Premium Mobile-sovelluksen.


## <a name="additional-resources"></a>Lisäresursseja

* [Luettelo siitä, miten voit integroida SaaS sovellusten Azure Active Directory-opetusohjelmat](active-directory-saas-tutorial-list.md)
* [Mikä on sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_205.png
