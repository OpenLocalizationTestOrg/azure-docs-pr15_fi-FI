<properties
    pageTitle="Opetusohjelma: Tableau Server Azure Active Directory-integrointi | Microsoft Azure"
    description="Opettele määrittämään kertakirjautumisen Azure Active Directory- ja Tableau Serverin välillä."
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


# <a name="tutorial-azure-active-directory-integration-with-tableau-server"></a>Opetusohjelma: Tableau Server Azure Active Directory-integrointi

Tässä opetusohjelmassa tavoitteena on kerrotaan, miten voit integroida Tableau Server Azure Active Directory (Azure AD).

Azure AD-integraation Tableau palvelimen avulla voit seuraavat edut:

- Voit määrittää, kenellä on oikeus Tableau Server Azure AD-
- Voit ottaa käyttöön automaattisesti Hae kirjautunut sisään Tableau palvelimeen (kertakirjautumisen) käyttäjille heidän Azure AD-tilien kanssa
- Voit hallita asiakkaiden yhdessä keskitetyssä sijainnissa - Azure perinteinen portal

Jos haluat tietää tarkempia tietoja SaaS sovelluksen integrointi Azure AD-kohdassa [mitä sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Edellytykset

Määrittää Tableau Server Azure AD-integrointi, tarvitset seuraavat:

- Azure AD-tilaus
- Tableau palvelimen single-merkki käytössä-tilauksessa


> [AZURE.NOTE] Voit esikatsella vaiheet Tässä opetusohjelmassa, Emme suosittele käyttämällä tuotantoympäristössä.


Voit esikatsella vaiheet Tässä opetusohjelmassa-noudattamalla näitä suosituksia:

- Älä käytä tuotantoympäristössä, ellei se on tarpeen.
- Jos sinulla ei ole kokeiluversion Azure AD-ympäristössä, voit hankkia yksi kuukausi kokeiluversion [tähän](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Skenaarion kuvaus
Tässä opetusohjelmassa tavoitteena on, jotta voit testata Azure AD kertakirjautumisen testiympäristössä. 

Tässä opetusohjelmassa kuvatut skenaarion kuuluu kaksi tärkeimmät rakenneosien:

1. Tableau palvelimen lisääminen valikoimasta
2. Yksittäinen määrittäminen ja testaus Azure AD-on


## <a name="adding-tableau-server-from-the-gallery"></a>Tableau palvelimen lisääminen valikoimasta
Voit määrittää Tableau palvelimen integroida Azure AD-haluat lisätä Tableau palvelimen valikoimasta hallitun SaaS sovellusluettelo.

**Lisää Tableau palvelimen valikoimasta, toimi seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen portal**, valitse vasemmassa siirtymisruudussa. 
 
    ![Active Directory][1]

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset][2]

4. Valitse sivun alareunassa **Lisää** .

    ![Sovellukset][3]

5. Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Sovellukset][4]

6. Kirjoita hakuruutuun **Tableau palvelimeen**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_01.png)

7. Valitse tulokset-ruudun Valitse **Tableau palvelin**ja valitse sitten **Valmis** , voit lisätä sovelluksen.

    ![Sovelluksen valitseminen valikoimasta](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yksittäinen määrittäminen ja testaus Azure AD-on
Tässä osassa tavoitteena on noudattamalla voit määrittää ja testaa Azure AD kertakirjautumisen nimeltä "Britta Simon" testi käyttäjään perustuva Tableau-palvelimen kanssa.

Kertakirjautumisen toimimaan Azure AD on tiedettävä vastaavaan käyttäjän Tableau Server Azure AD-käyttäjälle. Toisin sanoen linkki suhteen Azure AD-käyttäjä ja Aiheeseen liittyvät Tableau Serverissä on vahvistettava.

Linkki-yhteys muodostetaan määrittämällä **käyttäjänimi** arvo Azure AD arvona Tableau palvelimen **käyttäjänimi** .

Määrittäminen ja testaaminen Azure AD kertakirjautumisen Tableau palvelimen kanssa, sinun on tehtävä seuraavat hakukyselyn:

1. **[Azure AD määrittäminen Single Sign](#configuring-azure-ad-single-single-sign-on)** - käyttäjät voivat käyttää tätä toimintoa.
2. **[Käyttäjän testata luominen Azure AD](#creating-an-azure-ad-test-user)** - Testaa Azure AD kertakirjautumisen Britta Simon kanssa.
4. **[Luotaessa Tableau palvelin testata käyttäjän](#creating-a-tableauserver-test-user)** - on tapahtumista Britta Simon Tableau Serverissä, jotka on linkitetty hän Azure AD esitys.
5. **[Käyttäjän testata määrittäminen Azure AD](#assigning-the-azure-ad-test-user)** - Britta Simon käyttämään Azure AD kertakirjautumisen käyttöön.
5. **[Testaus Single Sign](#testing-single-sign-on)** - voit tarkistaa, toimiiko määritykset.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD-Single Sign-On määrittäminen

Tässä osassa tavoitteena on Azure AD kertakirjautumisen Azure perinteinen portaalissa ottaminen käyttöön ja määrittää kertakirjautumisen Tableau Server-sovelluksessa.

Tableau sovelluksen odottaa SAML vahvistukset tietyssä muodossa. Seuraavassa näyttökuvassa näkyy esimerkki tämän. 

![Kertakirjautumisen määrittäminen](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_51.png) 

**Jos haluat määrittää Azure AD kertakirjautumisen Tableau palvelimen kanssa, toimimalla seuraavasti:**


1. Valitse Azure perinteinen portaalissa **Tableau palvelimen** sovelluksen integrointi-sivun yläreunassa valikosta **määritteet**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-tableauserver-tutorial/tutorial_general_81.png) 


1. Valitse **SAML suojaustunnuksen määritteet** -valintaikkunassa seuraavasti:

    

    a. Valitse **Lisää käyttäjä Attribure** -valintaikkunan avaaminen **Lisää käyttäjä-määrite** .

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-tableauserver-tutorial/tutorial_general_82.png) 


    b. Kirjoita **käyttäjänimi** **Attrubute nimi** -tekstiruutuun.

    c-näppäinyhdistelmää. **Määritteen arvo** -luettelosta selsect **user.displayname**.

    d. Valitse **Valmis**.  
    



1. Valitse ylä-valikossa **Pika-aloitus**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-tableauserver-tutorial/tutorial_general_83.png)  








1. Valitse **Määritä kertakirjautumisen** **Määrittäminen Single Sign** -valintaikkunan avaaminen.

    ![Kertakirjautumisen määrittäminen][6] 



2. **Miten käyttäjät voivat kirjautua Tableau palvelimen haluat käyttää** sivulla Valitse **Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_03.png) 


3. Valitse **Määritä sovelluksen asetukset** -sivulla suorittamalla seuraavat vaiheet ja valitse **Seuraava**:

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_04.png) 



    a. Kirjoita **Kirjaudu sisään URL** -tekstiruutuun Tableau palvelimen URL-osoite. 

    b. Tunnus-ruutuun kopion 

    c-näppäinyhdistelmää. Valitse **Seuraava**


4. Valitse **Määritä kertakirjautumisen Tableau palvelimen** -sivulla suorittamalla seuraavat vaiheet ja valitse **Seuraava**:

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_05.png) 


    a. Valitse **Lataa metatiedot**ja tallenna sitten tiedosto tietokoneesta.

    b. Valitse **Seuraava**.


6. Saat määritetty sovelluksen SSO-haluat Sign Tableau Server-asiakasympäristöön järjestelmänvalvojana.

    a. Valitse Tableau palvelinkokoonpanon **SAML** -välilehti.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_001.png) 


    b. Valitse **Käytä SAML kertakirjautumisen varten**-valintaruutu.

    c-näppäinyhdistelmää. Etsi Azure perinteinen portaaliin ladatut Federation metatietojen tiedosto ja lataa se sitten **SAML Idp metatieto-tiedosto**.

    d. Tableau palvelin palauttaa URL-osoite, URL-osoite, Tableau palvelimen käyttäjät pääsevät, kuten http://tableau_server. Käyttämällä http://localhost ei ole suositeltavaa. URL-Osoitteen käyttäminen vinoviiva (esimerkiksi http://tableau_server/) ei tueta. Kopioi **Tableau palvelin palauttaa URL-osoite** ja liitä se Azure AD- **Merkki-URL-Osoitteen** tekstiruutu, kuten vaiheessa 3

    e. SAML kohteen tunnus, kohteen tunnus yksilöi Tableau palvelimen asennuksen niin, että IdP. Voit tähän kohtaan lisäämäsi Tableau palvelimen URL-osoite uudelleen, jos haluat, mutta se ei tarvitse olla Tableau palvelimen URL-osoite. **SAML kohteen tunnus** kopioimalla ja liittämällä se Azure AD- **TUNNUKSEEN** tekstiruudun vaihe 3 esitetyllä tavalla.

    f. Valitse **Vie metatieto-tiedosto** ja avaa se tekstin editori-sovelluksessa. Etsi vahvistus kuluttaja palvelun URL-osoite, Http Post indeksoida 0 ja kopioi URL-osoite. Nyt liittää Azure AD- **Vastauksen URL-Osoitteen** tekstiruutu vaihe 3 esitetyllä tavalla. 

    g. Napsauta **OK** -painiketta, Tableau Server Configiuration-sivulla.

    > [AZURE.NOTE] Jos tarvitset apua Tableau palvelimessa SAML sitten Lue tämän artikkelin [SAML määrittäminen](http://onlinehelp.tableau.com/current/server/en-us/config_saml.htm) 

6. Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Seuraava**.

    ![Azure AD-Single Sign-On][10]

7. Valitse **Sign Vahvista** -sivulla **Valmis**. 
 
    ![Azure AD-Single Sign-On][11]


### <a name="creating-an-azure-ad-test-user"></a>Azure AD-testikäyttäjän luominen
Tässä osassa tavoitteena on luotava testikäyttäjän kutsutaan Britta Simon Azure perinteinen portaalissa.

Valitse käyttäjät-luettelosta **Britta Simon**.

![Luo Azure AD-käyttäjä][20]

**Voit luoda testikäyttäjän Azure AD-toimimalla seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen portal**, valitse vasemmassa siirtymisruudussa.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_09.png) 

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Ylä-valikossa käyttäjäluettelon, näkyviin napsauttamalla **käyttäjät**.
 
    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_03.png) 


4. Avaa **Lisää käyttäjä** -valintaikkunan alareunassa työkalurivillä, valitse **Lisää käyttäjä**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_04.png)

5. Tee valintaikkunassa **Kerro kyseisen käyttäjän tietoja** -sivulla seuraavasti:

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_05.png) 

    a. **Käyttäjän tyyppi**Valitse **Uusi käyttäjä organisaatiossa**.

    b. Kirjoita **Käyttäjänimi** -tekstiruutuun **BrittaSimon**.

    c-näppäinyhdistelmää. Valitse **Seuraava**.

6.  Suorita **Käyttäjäprofiili** -sivulla seuraavat toimet:

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_06.png) 

    a. Kirjoita **Ensimmäinen nimi** -tekstiruutuun **Britta**.  

    b. Kirjoita **Sukunimi** -tekstiruutuun tyyppi- **Simon**.

    c-näppäinyhdistelmää. Kirjoita **Näyttönimi** -tekstiruutuun **Britta Simon**.

    d. Valitse **rooli** -luettelosta **käyttäjä**.

    e. Valitse **Seuraava**.

7. Valitse **Hae tilapäinen salasana** -valintaikkunan sivulla **Luo**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_07.png) 


8. **Hae tilapäinen salasana** -valintaikkunan sivulla toimimalla seuraavasti:
 
    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-tableauserver-tutorial/create_aaduser_08.png) 

    a. Kirjoita **Uusi salasana**arvo muistiin.

    b. Valitse **Valmis**.   



### <a name="creating-a-tableau-server-test-user"></a>Tableau Server testikäyttäjän luominen

Tässä osassa tavoitteena on nimeltään Britta Simon Tableau Serverissä käyttäjän luominen. Voit valmisteltava Tableau palvelimen kaikille käyttäjille. Huomaa, että käyttäjän käyttäjänimi on vastattava arvo, jonka olet määrittänyt Azure AD mukautetun määritteen **käyttäjänimi**. Oikea yhdistäminen integrointi kannattaa käyttää [Määrittäminen Azure AD kertakirjautumisen](#configuring-azure-ad-single-single-sign-on).

> [AZURE.NOTE] Jos haluat luoda luodun käyttäjän manuaalisesti, on organisaation Tableau palvelimen järjestelmänvalvojalta.


### <a name="assigning-the-azure-ad-test-user"></a>Azure AD-testikäyttäjän määrittäminen

Tässä osassa tavoitteena on Britta Simon hänen käyttöoikeuden myöntämisestä Tableau Server Azure kertakirjautumisen käyttöön ottaminen käyttöön.

![Määrittää käyttäjälle][200] 

**Jos haluat määrittää Britta Simon Tableau palvelimeen, toimi seuraavasti:**

1. Azure perinteinen-portaalissa Avaa sovellukset-näkymässä kansio-näkymässä, valitse **sovellukset** yläreunan-valikossa.
 
    ![Määrittää käyttäjälle][201] 

2. Valitse sovellukset-luettelosta **Tableau palvelimeen**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-tableauserver-tutorial/tutorial_tableauserver_50.png) 


1. Valitse ylä-valikossa Valitse **käyttäjiä**.

    ![Määrittää käyttäjälle][203]

1. Valitse käyttäjät-luettelosta **Britta Simon**.

2. Napsauta työkalurivin alaosassa **määrittäminen**.

![Määrittää käyttäjälle][205]



### <a name="testing-single-sign-on"></a>Kertakirjautumisen testaaminen

Tässä osassa tavoitteena on Testaa Azure AD yksittäisen Sign-asetusten käyttäminen Access-paneeli.

Access-ruudussa Tableau Server-ruutua napsauttaessasi voit olisi Hae automaattisesti kirjautunut sisään Tableau Server-sovellukseen.


## <a name="additional-resources"></a>Lisäresursseja

* [Luettelo siitä, miten voit integroida SaaS sovellusten Azure Active Directory-opetusohjelmat](active-directory-saas-tutorial-list.md)
* [Mikä on sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-tableauserver-tutorial/tutorial_general_205.png
