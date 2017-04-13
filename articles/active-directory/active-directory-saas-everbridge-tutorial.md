<properties
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi Everbridge | Microsoft Azure"
    description="Opettele määrittämään kertakirjautumisen Azure Active Directory- ja Everbridge välillä."
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


# <a name="tutorial-azure-active-directory-integration-with-everbridge"></a>Opetusohjelma: Everbridge Azure Active Directory-integrointi

Tässä opetusohjelmassa tavoitteena on, kuinka voit integroida Everbridge Azure Active Directory (Azure AD).

Azure AD-integraation Everbridge avulla voit seuraavat edut:

- Voit hallita Azure AD, jolla on pääsy Everbridge
- Voit ottaa käyttöön automaattisesti Hae kirjautunut käyttöön Everbridge (kertakirjautumisen) niiden Azure AD-tilien käyttäjille
- Voit hallita asiakkaiden yhdessä keskitetyssä sijainnissa - Azure perinteinen portal

Jos haluat tietää tarkempia tietoja SaaS sovelluksen integrointi Azure AD-kohdassa [mitä sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Edellytykset

Määrittää Azure AD-integroinnin Everbridge, tarvitset seuraavat:

- Azure AD-tilaus
- Everbridge single-merkki käytössä-tilauksessa


> [AZURE.NOTE] Voit esikatsella vaiheet Tässä opetusohjelmassa, Emme suosittele käyttämällä tuotantoympäristössä.


Voit esikatsella vaiheet Tässä opetusohjelmassa-noudattamalla näitä suosituksia:

- Älä käytä tuotantoympäristössä, ellei se on tarpeen.
- Jos sinulla ei ole kokeiluversion Azure AD-ympäristössä, voit hankkia yksi kuukausi kokeiluversion [tähän](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Skenaarion kuvaus
Tässä opetusohjelmassa tavoitteena on, jotta voit testata Azure AD kertakirjautumisen testiympäristössä.

Tässä opetusohjelmassa kuvatut skenaarion kuuluu kaksi tärkeimmät rakenneosien:

1. Everbridge lisääminen valikoimasta
2. Yksittäinen määrittäminen ja testaus Azure AD-on


## <a name="adding-everbridge-from-the-gallery"></a>Everbridge lisääminen valikoimasta
Voit määrittää Everbridge integroida Azure AD-haluat lisätä Everbridge valikoimasta hallitun SaaS sovellusluettelo.

**Jos haluat lisätä Everbridge valikoimasta, toimi seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen Portal**, valitse vasemmassa siirtymisruudussa.

    ![Active Directory][1]

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .
    
    ![Sovellukset][2]

4. Valitse sivun alareunassa **Lisää** .

    ![Sovellukset][3]

5. Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Sovellukset][4]

6. Kirjoita hakuruutuun **Everbridge**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-everbridge-tutorial/tutorial_everbridge_01.png)
7. Valitse tulosruudussa **Everbridge**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![Sovelluksen valitseminen valikoimasta](./media/active-directory-saas-everbridge-tutorial/tutorial_everbridge_001.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yksittäinen määrittäminen ja testaus Azure AD-on
Tässä osassa tavoitteena on noudattamalla voit määrittää ja testaa Azure AD kertakirjautumisen Everbridge nimeltä "Britta Simon" testikäyttäjän perusteella.

Kertakirjautumisen toimimaan Azure AD on tiedettävä vastaavaan käyttäjän Everbridge, Azure AD-käyttäjälle. Toisin sanoen suhteen Azure AD-käyttäjä ja Aiheeseen liittyvät Everbridge-linkki on vahvistettava.

Linkki-yhteys on muodostettu määrittämällä **käyttäjänimi** arvo arvoksi Everbridge **käyttäjänimi** Azure AD.

Määrittäminen ja testaaminen Azure AD kertakirjautumisen Everbridge kanssa, sinun on tehtävä seuraavat hakukyselyn:

1. **[Azure AD määrittäminen Single Sign](#configuring-azure-ad-single-single-sign-on)** - käyttäjät voivat käyttää tätä toimintoa.
2. **[Käyttäjän testata luominen Azure AD](#creating-an-azure-ad-test-user)** - Testaa Azure AD kertakirjautumisen Britta Simon kanssa.
3. **[Luomisesta Everbridge testata käyttäjän](#creating-a-everbridge-test-user)** - tapahtumista Britta Simon ovat Everbridge, jotka on linkitetty hän Azure AD esitys.
4. **[Käyttäjän testata määrittäminen Azure AD](#assigning-the-azure-ad-test-user)** - Britta Simon käyttämään Azure AD kertakirjautumisen käyttöön.
5. **[Testaus Single Sign](#testing-single-sign-on)** - voit tarkistaa, toimiiko määritykset.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD-Single Sign-On määrittäminen

Tässä osassa Azure AD kertakirjautumisen perinteinen portaalissa ottaminen käyttöön ja määritä kertakirjautumisen Everbridge-sovelluksessa.

**Määrittää Azure AD kertakirjautumisen Everbridge, toimimalla seuraavasti:**

1. Valitse perinteinen-portaalissa integrointi **Everbridge** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen Single Sign** -valintaikkunan avaaminen.
     
    ![Kertakirjautumisen määrittäminen][6] 

2. **Miten käyttäjät voivat kirjautua Everbridge haluat käyttää** sivulla Valitse **Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-everbridge-tutorial/tutorial_everbridge_03.png) 

3. Valitse **Määritä sovelluksen asetukset** -sivulla suorittamalla seuraavat vaiheet ja valitse **Seuraava**:

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-everbridge-tutorial/tutorial_everbridge_04.png)

    a. Kirjoita **tunnus** -tekstiruutuun URL-osoite, käyttämällä seuraavaa kaavaa:`https://sso.everbridge.net/{<company name>}`

    b. Kirjoita **Vastaus URL** -tekstiruutuun URL-osoite, käyttämällä seuraavaa kaavaa:`https://manager.everbridge.net/saml/SSO/{<company name>}/alias/defaultAlias`

    c-näppäinyhdistelmää. Valitse **Seuraava**

4. Valitse **Määritä kertakirjautumisen osoitteessa Everbridge** -sivulla suorittamalla seuraavat vaiheet ja valitse **Seuraava**:

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-everbridge-tutorial/tutorial_everbridge_05.png)

    a. Valitse **Lataa metatiedot**ja tallenna sitten tiedosto tietokoneesta.

    b. Valitse **Seuraava**.

5. Saat määritetty sovelluksen SSO-haluat Sign Everbridge asiakasympäristöön järjestelmänvalvojana.

6. Valitse ylä-valikossa **asetukset** -välilehti ja valitse **Single Sign-On** kohdassa **Suojaus**.
    
    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-everbridge-tutorial/tutorial_everbridge_002.png)

    a. Kirjoita **nimi** -tekstiruutuun tunnus tarjoajan nimi (esimerkiksi: yrityksen nimi).

    b. Kirjoita nimi API **API nimi** -tekstiruutuun.

    C-NÄPPÄINYHDISTELMÄÄ. Valitse Lataa metatieto-tiedosto, jonka voit ladata **Vaiheessa**4 **Valitse tiedosto** -painiketta.

    d. **SAML tunnistetietojen sijaintia**Valitse "Tunnistetiedot on NameIdentifier osan aihe-lause".

    e. Kopioi Azure AD SAML SSO URL-osoite Everbridge- **tunnistetietojen toimittaja sisäänkirjautumisen URL-osoite** .
    
    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-everbridge-tutorial/tutorial_everbridge_003.png)

    f. **Palveluntarjoajan aloittanut pyytää sidonta**Valitse HTTP-uudelleenohjaus.

7. Perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Seuraava**.

    ![Azure AD-Single Sign-On][10]

8. Valitse **Sign Vahvista** -sivulla **Valmis**.  

    ![Azure AD-Single Sign-On][11]



### <a name="creating-an-azure-ad-test-user"></a>Azure AD-testikäyttäjän luominen
Tässä osassa tavoitteena on luotava testikäyttäjän kutsutaan Britta Simon perinteinen portaalissa.

Valitse käyttäjät-luettelosta **Britta Simon**.
    
![Luo Azure AD-käyttäjä][20]

**Voit luoda testikäyttäjän Azure AD-toimimalla seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen Portal**, valitse vasemmassa siirtymisruudussa.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-everbridge-tutorial/create_aaduser_09.png)

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Ylä-valikossa käyttäjäluettelon, näkyviin napsauttamalla **käyttäjät**.
    
    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-everbridge-tutorial/create_aaduser_03.png)

4. Avaa **Lisää käyttäjä** -valintaikkunan alareunassa työkalurivillä, valitse **Lisää käyttäjä**.
    
    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-everbridge-tutorial/create_aaduser_04.png)

5. Tee valintaikkunassa **Kerro kyseisen käyttäjän tietoja** -sivulla seuraavasti:

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-everbridge-tutorial/create_aaduser_05.png)

    a. Käyttäjänä tyyppi Valitse uusi käyttäjä organisaatiossa.

    b. Kirjoita käyttäjänimi- **tekstiruutuun** **BrittaSimon**.

    c-näppäinyhdistelmää. Valitse **Seuraava**.

6.  Suorita **Käyttäjäprofiili** -sivulla seuraavat toimet:
    
    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-everbridge-tutorial/create_aaduser_06.png)

    a. Kirjoita **Ensimmäinen nimi** -tekstiruutuun **Britta**.  

    b. Kirjoita **Sukunimi** -tekstiruutuun tyyppi- **Simon**.

    c-näppäinyhdistelmää. Kirjoita **Näyttönimi** -tekstiruutuun **Britta Simon**.

    d. Valitse **rooli** -luettelosta **käyttäjä**.

    e. Valitse **Seuraava**.

7. Valitse **Hae tilapäinen salasana** -valintaikkunan sivulla **Luo**.
    
    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-everbridge-tutorial/create_aaduser_07.png)

8. **Hae tilapäinen salasana** -valintaikkunan sivulla toimimalla seuraavasti:
    
    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-everbridge-tutorial/create_aaduser_08.png)

    a. Kirjoita **Uusi salasana**arvo muistiin.

    b. Valitse **Valmis**.   



### <a name="creating-a-everbridge-test-user"></a>Everbridge testikäyttäjän luominen

Tässä osassa nimeltä Britta Simon Everbridge käyttäjän luominen. Ota käsitteleminen Everbridge tukihenkilöstön kautta <mailto:support@everbridge.com> voit lisätä käyttäjiä Everbridge-ympäristössä.


### <a name="assigning-the-azure-ad-test-user"></a>Azure AD-testikäyttäjän määrittäminen

Tässä osassa tavoitteena on Britta Simon käyttämään Azure kertakirjautumisen hänen käyttöoikeuden myöntämisestä Everbridge ottaminen käyttöön.
    
![Määrittää käyttäjälle][200]

**Jos haluat määrittää Britta Simon Everbridge, toimi seuraavasti:**

1. Perinteinen-portaalissa Avaa sovellukset-näkymässä kansio-näkymässä, valitse **sovellukset** yläreunan-valikossa.

    ![Määrittää käyttäjälle][201]

2. Valitse sovellukset-luettelosta **Everbridge**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-everbridge-tutorial/tutorial_everbridge_50.png)

3. Valitse ylä-valikossa Valitse **käyttäjiä**.
    
    ![Määrittää käyttäjälle][203]

4. Valitse käyttäjät-luettelosta **Britta Simon**.

5. Napsauta työkalurivin alaosassa **määrittäminen**.

    ![Määrittää käyttäjälle][205]



### <a name="testing-single-sign-on"></a>Kertakirjautumisen testaaminen

Tässä osassa tavoitteena on Testaa Azure AD yksittäisen Sign-asetusten käyttäminen Access-paneeli.

Access-ruudussa Everbridge-ruutua napsauttaessasi sinun pitäisi saada automaattisesti kirjautunut-on Everbridge sovelluksen.


## <a name="additional-resources"></a>Lisäresursseja

* [Luettelo siitä, miten voit integroida SaaS sovellusten Azure Active Directory-opetusohjelmat](active-directory-saas-tutorial-list.md)
* [Mikä on sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-everbridge-tutorial/tutorial_general_205.png
