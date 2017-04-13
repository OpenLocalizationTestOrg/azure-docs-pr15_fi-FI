<properties
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi SAP Business ByDesign | Microsoft Azure"
    description="Opettele määrittämään kertakirjautumisen Azure Active Directory- ja SAP Business ByDesign välillä."
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
    ms.date="09/09/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-sap-business-bydesign"></a>Opetusohjelma: SAP Business ByDesign Azure Active Directory-integrointi

Tässä opetusohjelmassa kerrotaan, miten voit integroida SAP Business ByDesign Azure Active Directory (Azure AD).

Azure AD-integraation SAP Business ByDesign avulla voit seuraavat edut:

- Voit hallita Azure AD, jolla on pääsy SAP Business ByDesign
- Voit ottaa käyttöön automaattisesti Hae kirjautunut käyttöön SAP Business ByDesign (kertakirjautumisen) käyttäjille heidän Azure AD-tilien kanssa
- Voit hallita asiakkaiden yhdessä keskitetyssä sijainnissa - Azure perinteinen portal

Jos haluat tietää tarkempia tietoja SaaS sovelluksen integrointi Azure AD-kohdassa [mitä sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Edellytykset

Määrittää Azure AD-integroinnin SAP Business ByDesign, tarvitset seuraavat:

- Azure AD-tilaus
- SAP: N Business ByDesign single-merkki käytössä-tilauksessa


> [AZURE.NOTE] Voit esikatsella vaiheet Tässä opetusohjelmassa, Emme suosittele käyttämällä tuotantoympäristössä.


Voit esikatsella vaiheet Tässä opetusohjelmassa-noudattamalla näitä suosituksia:

- Älä käytä tuotantoympäristössä, ellei se on tarpeen.
- Jos sinulla ei ole kokeiluversion Azure AD-ympäristössä, voit hankkia yksi kuukausi kokeiluversion [tähän](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Skenaarion kuvaus
Tässä opetusohjelmassa testaat Azure AD kertakirjautumisen testiympäristössä.

Tässä opetusohjelmassa kuvatut skenaarion kuuluu kaksi tärkeimmät rakenneosien:

1. SAP: N Business ByDesign lisääminen valikoimasta
2. Yksittäinen määrittäminen ja testaus Azure AD-on


## <a name="adding-sap-business-bydesign-from-the-gallery"></a>SAP: N Business ByDesign lisääminen valikoimasta
Voit määrittää SAP Business ByDesign integroida Azure AD-haluat lisätä SAP Business ByDesign valikoimasta hallitun SaaS sovellusluettelo.

**Jos haluat lisätä SAP Business ByDesign valikoimasta, toimi seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen portal**, valitse vasemmassa siirtymisruudussa.

    ![Active Directory][1]

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.


3. Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset][2]

4. Valitse sivun alareunassa **Lisää** .

    ![Sovellukset][3]

5. Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Sovellukset][4]

6. Kirjoita hakuruutuun **SAP Business ByDesign**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_01.png)

7. Valitse tulosruudussa **SAP Business ByDesign**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![Active Directory](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_02.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yksittäinen määrittäminen ja testaus Azure AD-on
Tässä osassa Määritä ja testaa Azure AD kertakirjautumisen SAP Business ByDesign nimeltä "Britta Simon" testikäyttäjän perusteella.

Kertakirjautumisen toimimaan Azure AD on tiedettävä käyttäjälle on SAP Business ByDesign vastaavaan käyttäjän Azure AD. Toisin sanoen linkki suhteen Azure AD-käyttäjä- ja SAP Business ByDesign liittyvät käyttäjän on vahvistettava.

Linkki-yhteys on muodostettu määrittämällä **käyttäjänimi** arvo arvoksi SAP Business ByDesign **käyttäjänimi** Azure AD.

Määrittäminen ja testaaminen Azure AD kertakirjautumisen SAP Business ByDesign kanssa, sinun on tehtävä seuraavat hakukyselyn:

1. **[Azure AD määrittäminen Single Sign](#configuring-azure-ad-single-sign-on)** - käyttäjät voivat käyttää tätä toimintoa.
2. **[Käyttäjän testata luominen Azure AD](#creating-an-azure-ad-test-user)** - Testaa Azure AD kertakirjautumisen Britta Simon kanssa.
3. **[SAP: N Business-ByDesign luominen testata käyttäjän](#creating-an-sap-business-bydesign-test-user)** - tapahtumista Britta Simon ovat SAP Business ByDesign kyseistä henkilöä hän Azure AD esitys.
4. **[Käyttäjän testata määrittäminen Azure AD](#assigning-the-azure-ad-test-user)** - Britta Simon käyttämään Azure AD kertakirjautumisen käyttöön.
5. **[Testaus Single Sign](#testing-single-sign-on)** - voit tarkistaa, toimiiko määritykset.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD kertakirjautumisen määrittäminen

Tässä osassa Azure AD kertakirjautumisen perinteinen portaalissa ottaminen käyttöön ja määritä kertakirjautumisen SAP Business ByDesign-sovelluksessa.

SAP: N Business ByDesign sovelluksen odottaa SAML vahvistukset tietyssä muodossa. Määritä seuraavat saatavat tämän sovelluksen. Voit hallita näitä määritteitä arvot sovelluksen **"Atrribute"** -välilehdestä. Seuraavassa näyttökuvassa näkyy esimerkki tämän. 


**Määrittää Azure AD kertakirjautumisen SAP Business ByDesign, toimimalla seuraavasti:**


1. Valitse Azure perinteinen portaalissa **SAP Business ByDesign** sovelluksen integrointi-sivun yläreunassa valikosta **määritteet**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_80.png) 


2. Määritteet SAML suojaustunnuksen määritteet-luettelosta Valitse nameidentifier-määrite ja valitse sitten **Muokkaa**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_84.png) 

3. Valitse Muokkaa käyttäjän määrite-valintaikkunassa seuraavasti:

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_85.png) 

    a. Määritteen arvo-luettelosta **ExtractMailPrefix()** fuction

    b. Valitse Sähköposti-luettelosta käyttäjän-määrite, jota haluat käyttää käyttöympäristösi. 
    Esimerkiksi jos haluat käyttää työntekijöillä yksilöllinen käyttäjätunnus ja olet tallentanut määritteen ExtensionAttribute2, valitse **user.extensionattribute2**. 

    c-näppäinyhdistelmää. Valitse **Valmis**. 
    

4. Valitse perinteinen-portaalissa integrointi **SAP Business ByDesign** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen Single Sign** -valintaikkunan avaaminen.
     
    ![Kertakirjautumisen määrittäminen][6] 

5. **Miten käyttäjät voivat kirjautua SAP Business ByDesign haluat käyttää** sivulla Valitse **Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_03.png) 

6. Valitse **Määritä sovelluksen asetukset** -sivulla toimimalla seuraavasti:

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_04.png) 

    a. Kirjoita **Merkki-URL** -tekstiruutuun URL-osoite käyttämä käyttäjille Sign SAP Business ByDesign sovelluksen käyttämällä seuraavaa kaavaa:`https://<servername>.sapbydesign.com`
    
    b. Valitse **Seuraava**
 
7. Valitse **Määritä kertakirjautumisen osoitteessa SAP Business ByDesign** -sivulla toimimalla seuraavasti:

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_05.png)

    a. Valitse **Lataa metatiedot**ja tallenna sitten tiedosto tietokoneesta.

    b. Valitse **Seuraava**.


8. Saat määritetty sovelluksen SSO-toimimalla seuraavasti:

    a. Kirjaudu SAP Business ByDesign-portaalin järjestelmänvalvojan oikeudet.

    b. Siirry osoitteeseen **sovelluksen ja käyttäjien hallinta yhteisen tehtävän** ja **Tunnistetietojen toimittaja** -välilehti.

    c-näppäinyhdistelmää. Valitse **Uusi tunnistetietojen toimittaja** ja valitse perinteinen Azure-portaalista lataamasi metatietojen XML-tiedosto. Metatietojen tuomalla järjestelmän automaattisesti Lataa palvelimeen tarvittavat allekirjoituksen varmenne ja salausvarmenne.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_54.png)

    d. Voit sisällyttää SAML pyynnön **Vahvistus kuluttaja palvelun URL-osoite** , valitse **Sisällytä vahvistus kuluttaja palvelun URL-osoite**.

    e. Valitse **Ota käyttöön kertakirjautumisen**.

    f. Tallenna muutokset.

    g. Valitse **Oma järjestelmä** -välilehti.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_52.png)

    h. Kopioi **SSO URL-osoite** ja liitä se **Azure AD-merkki URL** -tekstiruutuun.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_53.png)

    voin. Määritä, onko työntekijän voit valita manuaalisesti valitsemalla **Manuaalinen tunnistetietojen toimittaja valinnan**Käyttäjätunnus ja salasana tai SSO sisäänkirjautumista.

    j. Määritä URL-osoite, jota käytetään työntekijän järjestelmän kirjautumisessa **SSO URL-osoite** -kohtaan. 
    Kirjoita URL-osoite lähetetään työntekijän avattavasta luettelosta voit valita seuraavista vaihtoehdoista:
    
    **SSO URL-osoite**
 
    Järjestelmä lähettää työntekijän vain Normaali järjestelmän URL-osoite. Työntekijän ei voi kirjautua sisään käyttäen SSO- ja on Käytä salasanaa tai varmenteen sen sijaan.

    **SSO URL-OSOITE** 

    Järjestelmä lähettää työntekijän vain SSO URL-osoite. Työntekijän voit kirjautua käyttämällä SSO. Todennus-pyyntö ohjataan IdP kautta.

    **Automaattinen valinta**
 
    Jos Kertakirjautumista ei ole aktiivinen, järjestelmä lähettää työntekijän normaalijakauman järjestelmän URL-osoite. Jos SSO on valittuna, järjestelmä tarkistaa, onko työntekijän salasanan. Jos salasana ei ole käytettävissä, sekä SSO ja muut SSO URL-Osoitteen lähetetään työntekijä. Jos työntekijä ei ole salasanaa, vain SSO URL-osoite lähetetään työntekijä.

    k. Tallenna muutokset.

9. Perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Seuraava**.
    
    ![Azure AD-Single Sign-On][10]

10. Valitse **Sign Vahvista** -sivulla **Valmis**.  
 
    ![Azure AD-Single Sign-On][11]


### <a name="creating-an-azure-ad-test-user"></a>Azure AD-testikäyttäjän luominen
Tässä osassa voit luoda testikäyttäjän kutsutaan Britta Simon perinteinen portaalissa.


![Luo Azure AD-käyttäjä][20]

**Voit luoda testikäyttäjän Azure AD-toimimalla seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen portal**, valitse vasemmassa siirtymisruudussa.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_09.png) 

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Ylä-valikossa käyttäjäluettelon, näkyviin napsauttamalla **käyttäjät**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_03.png) 

4. Avaa **Lisää käyttäjä** -valintaikkunan alareunassa työkalurivillä, valitse **Lisää käyttäjä**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_04.png) 

5. Tee valintaikkunassa **Kerro kyseisen käyttäjän tietoja** -sivulla seuraavasti:
    
    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_05.png) 

    a. Käyttäjänä tyyppi Valitse uusi käyttäjä organisaatiossa.

    b. Kirjoita käyttäjänimi- **tekstiruutuun** **BrittaSimon**.

    c-näppäinyhdistelmää. Valitse **Seuraava**.

6.  Suorita **Käyttäjäprofiili** -sivulla seuraavat toimet:
    
    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_06.png) 

    a. Kirjoita **Ensimmäinen nimi** -tekstiruutuun **Britta**.  

    b. Kirjoita **Sukunimi** -tekstiruutuun tyyppi- **Simon**.

    c-näppäinyhdistelmää. Kirjoita **Näyttönimi** -tekstiruutuun **Britta Simon**.

    d. Valitse **rooli** -luettelosta **käyttäjä**.

    e. Valitse **Seuraava**.

7. Valitse **Hae tilapäinen salasana** -valintaikkunan sivulla **Luo**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_07.png) 

8. **Hae tilapäinen salasana** -valintaikkunan sivulla toimimalla seuraavasti:

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_08.png) 

    a. Kirjoita **Uusi salasana**arvo muistiin.

    b. Valitse **Valmis**.   



### <a name="creating-an-sap-business-bydesign-test-user"></a>SAP: N Business ByDesign-testikäyttäjän luominen

Tässä osassa nimeltä Britta Simon SAP Business ByDesign käyttäjän luominen. Toimi SAP Business ByDesign tukihenkilöstöösi voit lisätä käyttäjiä SAP Business ByDesign-ympäristössä. 

> [AZURE.NOTE] Varmista, että NameID arvo on vastattava SAP Business ByDesign platform username-kenttä.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD-testikäyttäjän määrittäminen

Tässä osassa voit ottaa käyttöön Britta Simon käyttämään Azure kertakirjautumisen hänen käyttöoikeuksien myöntämistä SAP Business ByDesign.

![Määrittää käyttäjälle][200] 

**Jos haluat määrittää Britta Simon SAP Business ByDesign, toimi seuraavasti:**

1. Perinteinen-portaalissa Avaa sovellukset-näkymässä kansio-näkymässä, valitse **sovellukset** yläreunan-valikossa.

    ![Määrittää käyttäjälle][201] 

2. Valitse sovellukset-luettelosta **SAP Business ByDesign**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_50.png) 

3. Valitse ylä-valikossa Valitse **käyttäjiä**.

    ![Määrittää käyttäjälle][203]

4. Valitse käyttäjät-luettelosta **Britta Simon**.

5. Napsauta työkalurivin alaosassa **määrittäminen**.

    ![Määrittää käyttäjälle][205]


### <a name="testing-single-sign-on"></a>Kertakirjautumisen testaaminen

Tässä osassa voit testata Azure AD yksittäisen Sign-asetusten käyttäminen Access-paneeli.

Access-ruudussa SAP Business ByDesign-ruutua napsauttaessasi voit olisi Hae automaattisesti kirjautunut sisään SAP Business ByDesign sovelluksen.


## <a name="additional-resources"></a>Lisäresursseja

* [Luettelo siitä, miten voit integroida SaaS sovellusten Azure Active Directory-opetusohjelmat](active-directory-saas-tutorial-list.md)
* [Mikä on sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_205.png
