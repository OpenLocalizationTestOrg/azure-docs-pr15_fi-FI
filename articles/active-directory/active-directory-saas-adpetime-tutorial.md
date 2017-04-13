<properties
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi ADP eTime | Microsoft Azure"
    description="Opettele määrittämään kertakirjautumisen Azure Active Directory- ja ADP eTime välillä."
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


# <a name="tutorial-azure-active-directory-integration-with-adp-etime"></a>Opetusohjelma: ADP eTime Azure Active Directory-integrointi

Tässä opetusohjelmassa tavoitteena on, kuinka voit integroida ADP eTime Azure Active Directory (Azure AD).  
Azure AD-integraation ADP eTime avulla voit seuraavat edut:

- Voit hallita Azure AD, kenellä on oikeus ADP eTime
- Voit ottaa käyttöön automaattisesti Hae kirjautunut käyttöön niiden Azure AD-tilien kanssa (kertakirjautumisen) ADP eTime käyttäjille
- Voit hallita asiakkaiden yhdessä keskitetyssä sijainnissa - Azure perinteinen portal


Jos haluat tietää tarkempia tietoja SaaS sovelluksen integrointi Azure AD-kohdassa [mitä sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Edellytykset

Määrittää Azure AD-integroinnin ADP eTime, tarvitset seuraavat:

- Azure AD-tilaus
- ADP eTime single-merkki käytössä-tilauksessa


> [AZURE.NOTE] Voit esikatsella vaiheet Tässä opetusohjelmassa, Emme suosittele käyttämällä tuotantoympäristössä.


Voit esikatsella vaiheet Tässä opetusohjelmassa-noudattamalla näitä suosituksia:

- Älä käytä tuotantoympäristössä, ellei se on tarpeen.
- Jos sinulla ei ole kokeiluversion Azure AD-ympäristössä, voit hankkia yksi kuukausi kokeiluversion [tähän](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Skenaarion kuvaus
Tässä opetusohjelmassa tavoitteena on, jotta voit testata Azure AD kertakirjautumisen testiympäristössä.  
Tässä opetusohjelmassa kuvatut skenaarion kuuluu kaksi tärkeimmät rakenneosien:

1. ADP eTime lisääminen valikoimasta
2. Yksittäinen määrittäminen ja testaus Azure AD-on


## <a name="adding-adp-etime-from-the-gallery"></a>ADP eTime lisääminen valikoimasta
Voit määrittää ADP eTime integroida Azure AD-haluat lisätä ADP eTime valikoimasta hallitun SaaS sovellusluettelo.

**Jos haluat lisätä ADP eTime valikoimasta, toimi seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen portal**, valitse vasemmassa siirtymisruudussa. 

    ![Active Directory][1]

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset][2]

4. Valitse sivun alareunassa **Lisää** .

    ![Sovellukset][3]

5. Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Sovellukset][4]

6. Kirjoita hakuruutuun **ADP eTime**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_01.png)

7. Valitse tulosruudussa **ADP eTime**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_06.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yksittäinen määrittäminen ja testaus Azure AD-on
Tässä osassa tavoitteena on noudattamalla voit määrittää ja testaa Azure AD kertakirjautumisen ADP eTime nimeltä "Britta Simon" testikäyttäjän perusteella.

Kertakirjautumisen toimimaan Azure AD on tietää, mitkä asiat vastaavaan käyttäjän ADP eTime Azure AD-käyttäjälle. Toisin sanoen suhteen Azure AD-käyttäjä ja Aiheeseen liittyvät ADP eTime-linkki on vahvistettava.  
Linkki-yhteys on muodostettu määrittämällä **käyttäjänimi** arvo arvoksi ADP eTime **käyttäjänimi** Azure AD.

Määrittäminen ja testaaminen Azure AD kertakirjautumisen ADP eTime kanssa, sinun on tehtävä seuraavat hakukyselyn:

1. **[Azure AD määrittäminen Single Sign](#configuring-azure-ad-single-single-sign-on)** - käyttäjät voivat käyttää tätä toimintoa.
2. **[Käyttäjän testata luominen Azure AD](#creating-an-azure-ad-test-user)** - Testaa Azure AD kertakirjautumisen Britta Simon kanssa.
4. **[Käyttäjän luominen ADP eTime testata](#creating-a-adpetime-test-user)** - on vastaavaan Britta Simon, valitse ADP eTime kyseistä henkilöä hän Azure AD-esittäminen.
5. **[Käyttäjän testata määrittäminen Azure AD](#assigning-the-azure-ad-test-user)** - Britta Simon käyttämään Azure AD kertakirjautumisen käyttöön.
5. **[Testaus Single Sign](#testing-single-sign-on)** - voit tarkistaa, toimiiko määritykset.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD-Single Sign-On määrittäminen

Tässä osassa tavoitteena on Azure AD kertakirjautumisen Azure perinteinen portaalissa ottaminen käyttöön ja määrittää kertakirjautumisen ADP eTime-sovelluksessa.

ADP eTime sovelluksen odottaa SAML vahvistukset tietyssä muodossa, jossa pyydetään mukautettujen määritteiden yhdistämismäärityksiä lisääminen SAML suojaustunnuksen määritteet-määritys. Seuraavassa näyttökuvassa näkyy esimerkki tämän. Ryhmän nimi on aina **"PersonImmutableID"** ja joista on on yhdistetty ExtensionAttribute2, joka sisältää käyttäjän työntekijöillä arvo. Tähän työntekijöillä tehdään käyttäjän yhdistäminen t Azure AD ADP eTime, mutta voit yhdistää arvon perusteella myös sovelluksen asetukset. Ota niin työ ADP eTime ryhmä ensin voit käyttää käyttäjän oikean tunnisteen ja yhdistää kyseisen arvon **"PersonImmutableID"** -ryhmän kanssa.  

![Kertakirjautumisen määrittäminen](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_02.png) 

Ennen kuin voit määrittää SAML vahvistus, sinun täytyy ADP-eTime tukeen ja pyydä vuokraajan yksilöllinen tunnus-määritteen arvon. Tarvitset tätä arvoa voit määrittää mukautetun ryhmän sovelluksen.


**Määrittää Azure AD kertakirjautumisen ADP eTime, toimimalla seuraavasti:**

1. Valitse Azure perinteinen portaalissa integrointi **ADP eTime** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen Single Sign** -valintaikkunan avaaminen.

    ![Kertakirjautumisen määrittäminen][6] 

2. **Miten käyttäjät voivat kirjautua ADP eTime haluat käyttää** sivulla Valitse **Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_03.png) 

3. Valitse **Määritä sovelluksen asetukset** -sivulla toimimalla seuraavasti:.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_04.png) 


    a. Kirjoita **Vastaus URL** -tekstiruutuun URL-osoite käyttäjien Sign ADP eTime sovelluksen käyttämällä seuraavaa kaavaa: `https://<server name>.adp.com/affwebservices/public/saml2assertionconsumer`.

    b. Valitse **Seuraava**.

4. Valitse **Määritä kertakirjautumisen osoitteessa ADP eTime** -sivulla toimimalla seuraavasti:

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_05.png) 

    a. Valitse **Lataa metatiedot**ja tallenna sitten tiedosto tietokoneesta.

    b. Valitse **Seuraava**.


5. Saat määritetty sovelluksen SSO-tukihenkilöstöltä ADP eTime ja sähköposti ladataan Liitä metatiedot-tiedoston niin, että ne on määritetty SSO-integrointia varten.

    > [AZURE.NOTE] Kun **ADP eTime** työryhmän määrittäminen esiintymää, pyydä ne **RelayState** -arvo. Toimi alla mainitut vaiheet, määritä se. Määritysten jälkeen voit testata integrointi. Niin Huomaa, että tämä on tärkeää määritys toimimaan tämän sovelluksen-integrointia varten.

6. Voit määrittää RelayState arvon Azure AD-toimimalla seuraavasti: 
    
    a. Kirjautuminen järjestelmänvalvojana [Azure hallinta-portaalin](https://portal.azure.com) .

    b. Valitse vasemmassa siirtymisruudussa **Lisää palveluita**. 
    
    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_07.png)

    c-näppäinyhdistelmää. Kirjoita **Etsi** -tekstiruutuun **Azure Active Directory**ja valitse liittyvä linkki.
    
    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_08.png)

    d. Valitse **yrityksen sovellukset**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_09.png)

    e. Valitse **hallinta** -osasta **Kaikki sovellukset**.
    
    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_10.png)

    f. Kirjoita **Etsi** -tekstiruutuun **ADP eTime**ja valitse liittyvä linkki. 
    
    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_11.png)

    g. Valitse **hallinta** -osasta **kertakirjautumisen**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_12.png)

    h. Valitse **Näytä lisäasetukset URL-osoite**.
    
    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_13.png)
    
    voin. Kirjoita **Välityksen tila** -tekstiruutuun käyttämällä seuraavista tavoista:
    
    - Tuotantoympäristössä:`https://fed.adp.com/saml/fedlanding.html?<id>` 
    - Vaiheet-ympäristöön:`https://fed-stag.adp.com/saml/fedlanding.html?PORTAL`

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_14.png)

    j. Tallenna asetukset.

7. Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Seuraava**.

    ![Azure AD-Single Sign-On][10]

8. Valitse **Sign Vahvista** -sivulla **Valmis**.  

    ![Azure AD-Single Sign-On][11]



### <a name="creating-an-azure-ad-test-user"></a>Azure AD-testikäyttäjän luominen
Tässä osassa tavoitteena on luotava testikäyttäjän kutsutaan Britta Simon Azure perinteinen portaalissa.  
Valitse käyttäjät-luettelosta **Britta Simon**.

![Luo Azure AD-käyttäjä][20]

**Voit luoda testikäyttäjän Azure AD-toimimalla seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen portal**, valitse vasemmassa siirtymisruudussa.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-adpetime-tutorial/create_aaduser_09.png) 

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Ylä-valikossa käyttäjäluettelon, näkyviin napsauttamalla **käyttäjät**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-adpetime-tutorial/create_aaduser_03.png) 

4. Avaa **Lisää käyttäjä** -valintaikkunan alareunassa työkalurivillä, valitse **Lisää käyttäjä**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-adpetime-tutorial/create_aaduser_04.png) 

5. Tee valintaikkunassa **Kerro kyseisen käyttäjän tietoja** -sivulla seuraavasti:

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-adpetime-tutorial/create_aaduser_05.png) 

    a. **Käyttäjän tyyppi**Valitse **Uusi käyttäjä organisaatiossa**.

    b. Kirjoita **Käyttäjänimi** -tekstiruutuun **BrittaSimon**.

    c-näppäinyhdistelmää. Valitse **Seuraava**.

6.  Suorita **Käyttäjäprofiili** -sivulla seuraavat toimet:

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-adpetime-tutorial/create_aaduser_06.png) 

    a. Kirjoita **Ensimmäinen nimi** -tekstiruutuun **Britta**.  

    b. Kirjoita **Sukunimi** -tekstiruutuun tyyppi- **Simon**.

    c-näppäinyhdistelmää. Kirjoita **Näyttönimi** -tekstiruutuun **Britta Simon**.

    d. Valitse **rooli** -luettelosta **käyttäjä**.

    e. Valitse **Seuraava**.

7. Valitse **Hae tilapäinen salasana** -valintaikkunan sivulla **Luo**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-adpetime-tutorial/create_aaduser_07.png) 

8. **Hae tilapäinen salasana** -valintaikkunan sivulla toimimalla seuraavasti:

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-adpetime-tutorial/create_aaduser_08.png) 

    a. Kirjoita **Uusi salasana**arvo muistiin.

    b. Valitse **Valmis**.   



### <a name="creating-a-adp-etime-test-user"></a>ADP eTime testikäyttäjän luominen

Tässä osassa tavoitteena on nimeltään Britta Simon ADP eTime käyttäjän luominen. Toimi ADP eTime tukihenkilöstön voit lisätä käyttäjiä ADP eTime-tilin kanssa. 


> [AZURE.NOTE]Jos haluat luoda luodun käyttäjän manuaalisesti, on ADP eTime-tukeen.


### <a name="assigning-the-azure-ad-test-user"></a>Azure AD-testikäyttäjän määrittäminen

Tässä osassa tavoitteena on Britta Simon käyttämään Azure kertakirjautumisen hänen ADP eTime käyttöoikeuksien myöntäminen ottaminen käyttöön.

![Määrittää käyttäjälle][200] 

**Jos haluat määrittää Britta Simon ADP eTime, toimi seuraavasti:**

1. Azure perinteinen-portaalissa Avaa sovellukset-näkymässä kansio-näkymässä, valitse **sovellukset** yläreunan-valikossa.

    ![Määrittää käyttäjälle][201] 

2. Valitse sovellukset-luettelosta **ADP eTime**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-adpetime-tutorial/tutorial_adpetime_50.png) 

1. Valitse ylä-valikossa Valitse **käyttäjiä**.

    ![Määrittää käyttäjälle][203] 

1. Valitse käyttäjät-luettelosta **Britta Simon**.

2. Napsauta työkalurivin alaosassa **määrittäminen**.

    ![Määrittää käyttäjälle][205]



### <a name="testing-single-sign-on"></a>Kertakirjautumisen testaaminen

Tässä osassa tavoitteena on Testaa Azure AD yksittäisen Sign-asetusten käyttäminen Access-paneeli.  
Access-ruudussa ADP eTime-ruutua napsauttaessasi sinun pitäisi saada automaattisesti kirjautunut-on ADP eTime sovelluksen.


## <a name="additional-resources"></a>Lisäresursseja

* [Luettelo siitä, miten voit integroida SaaS sovellusten Azure Active Directory-opetusohjelmat](active-directory-saas-tutorial-list.md)
* [Mikä on sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-adpetime-tutorial/tutorial_general_205.png
