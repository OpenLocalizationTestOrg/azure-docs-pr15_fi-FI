<properties
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi varapääministeri | Microsoft Azure"
    description="Opettele määrittämään kertakirjautumisen Azure Active Directory ja varapuheenjohtajan välillä."
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
    ms.date="09/28/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-deputy"></a>Opetusohjelma: Varapääministeri Azure Active Directory-integrointi

Tässä opetusohjelmassa tavoitteena on, kuinka voit integroida varapääministeri Azure Active Directory (Azure AD).

Azure AD-integraation varapääministeri, jonka avulla seuraavat edut:

- Voit hallita Azure AD, kenellä on oikeus varapääministeri
- Voit ottaa käyttöön automaattisesti Hae kirjautunut käyttöön varapääministeri (kertakirjautumisen) niiden Azure AD-tilien käyttäjille
- Voit hallita asiakkaiden yhdessä keskitetyssä sijainnissa - Azure perinteinen portal

Jos haluat tietää tarkempia tietoja SaaS sovelluksen integrointi Azure AD-kohdassa [mitä sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Edellytykset

Määrittää Azure AD-integroinnin varapääministeri, tarvitset seuraavat:

- Azure AD-tilaus
- Varapääministeri single-merkki käytössä-tilauksessa


> [AZURE.NOTE] Voit esikatsella vaiheet Tässä opetusohjelmassa, Emme suosittele käyttämällä tuotantoympäristössä.


Voit esikatsella vaiheet Tässä opetusohjelmassa-noudattamalla näitä suosituksia:

- Älä käytä tuotantoympäristössä, ellei se on tarpeen.
- Jos sinulla ei ole kokeiluversion Azure AD-ympäristössä, voit hankkia yksi kuukausi kokeiluversion [tähän](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Skenaarion kuvaus
Tässä opetusohjelmassa tavoitteena on, jotta voit testata Azure AD kertakirjautumisen testiympäristössä.

Tässä opetusohjelmassa kuvatut skenaarion kuuluu kaksi tärkeimmät rakenneosien:

1. Varapääministeri lisääminen valikoimasta
2. Yksittäinen määrittäminen ja testaus Azure AD-on


## <a name="adding-deputy-from-the-gallery"></a>Varapääministeri lisääminen valikoimasta
Voit määrittää varapääministeri integroida Azure AD-haluat lisätä varapääministeri valikoimasta hallitun SaaS sovellusluettelo.

**Lisää varapääministeri valikoimasta, toimi seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen Portal**, valitse vasemmassa siirtymisruudussa. 

    ![Active Directory][1]

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .
    
    ![Sovellukset][2]

4. Valitse sivun alareunassa **Lisää** .
    
    ![Sovellukset][3]

5. Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Sovellukset][4]

6. Kirjoita hakuruutuun **varapääministeri**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_01.png)

7. Tulokset-ruudussa Valitse **varapääministeri**ja valitse sitten **Valmis** , voit lisätä sovelluksen.

    ![Sovelluksen valitseminen valikoimasta](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_0001.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yksittäinen määrittäminen ja testaus Azure AD-on
Tässä osassa tavoitteena on noudattamalla voit määrittää ja testaa Azure AD kertakirjautumisen varapääministeri nimeltä "Britta Simon" testikäyttäjän perusteella.

Kertakirjautumisen toimimaan Azure AD on tiedettävä vastaavaan käyttäjän varapääministeri, Azure AD-käyttäjälle. Toisin sanoen suhteen Azure AD-käyttäjä ja Aiheeseen liittyvät varapääministeri-linkki on vahvistettava.

Linkki-yhteys on muodostettu määrittämällä **käyttäjänimi** arvo arvoksi varapääministeri **käyttäjänimi** Azure AD.

Määrittäminen ja testaaminen Azure AD kertakirjautumisen varapääministeri kanssa, sinun on tehtävä seuraavat hakukyselyn:

1. **[Azure AD määrittäminen Single Sign](#configuring-azure-ad-single-single-sign-on)** - käyttäjät voivat käyttää tätä toimintoa.
2. **[Käyttäjän testata luominen Azure AD](#creating-an-azure-ad-test-user)** - Testaa Azure AD kertakirjautumisen Britta Simon kanssa.
3. **[Luomisesta varapääministeri testata käyttäjän](#creating-a-deputy-test-user)** - tapahtumista Britta Simon ovat varapääministeri kyseistä henkilöä hän Azure AD esitys.
4. **[Käyttäjän testata määrittäminen Azure AD](#assigning-the-azure-ad-test-user)** - Britta Simon käyttämään Azure AD kertakirjautumisen käyttöön.
5. **[Testaus Single Sign](#testing-single-sign-on)** - voit tarkistaa, toimiiko määritykset.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD kertakirjautumisen määrittäminen

Tässä osassa käyttöön Azure AD kertakirjautumisen perinteinen-portaalissa ja määritä kertakirjautumisen varapääministeri-sovelluksessa.

**Määrittää Azure AD kertakirjautumisen varapääministeri, toimimalla seuraavasti:**

1. Valitse perinteinen-portaalissa **varapuheenjohtajan** sovellus integrointi-sivulla **Määritä kertakirjautumisen** **Määrittäminen Single Sign** -valintaikkunan avaaminen.
     
    ![Kertakirjautumisen määrittäminen][6] 

2. **Miten käyttäjät voivat kirjautua varapääministeri haluat käyttää** sivulla Valitse **Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.
    
    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_03.png)

3. Jos haluat sovelluksen määrittäminen **IDP aloittanut tila**, seuraavien toimien **App-asetusten määrittäminen** -sivulla ja valitse **Seuraava**:

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_04.png)

    a. Kirjoita **tunnus** -tekstiruutuun URL-osoite, käyttämällä seuraavaa kaavaa: `https://<your-subdomain>.<region>.deputy.com`.

    b. Kirjoita **Vastaus URL** -tekstiruutuun URL-osoite, käyttämällä seuraavaa kaavaa: `https://<your-subdomain>.<region>.deputy.com/exec/devapp/samlacs`.

    c-näppäinyhdistelmää. Valitse **Seuraava**.

4. Jos haluat määrittää sovelluksen **SP aloittanut tila** -valintaikkunan **App-asetusten määrittäminen** -sivulla, valitse **"Näytä lisäasetukset (valinnainen)"** ja kirjoita **Merkki-URL-osoite** ja valitse **Seuraava**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_05.png)

    a. Kirjoita **Merkki-URL** -tekstiruutuun URL-osoite, käyttämällä seuraavaa kaavaa: `https://<your-subdomain>.<region>.deputy.com`.

    b. Valitse **Seuraava**.

    > [AZURE.NOTE] Varapääministeri alueen liite on opitional tai käytettävä seuraavasti: au | puuttuu | EU: n | nimellä | la | af | | ent au | ent puuttuu | ent eu | ent-kuin | ENT la | ENT af | ENT on

5. Valitse **Määritä kertakirjautumisen osoitteessa varapääministeri** -sivulla suorittamalla seuraavat vaiheet ja valitse **Seuraava**:

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_06.png)

    a. Valitse **Lataa**ja tallenna sitten tiedosto tietokoneesta.

    
6. Siirry seuraava URL-osoite: https://(your-subdomain).deputy.com/exec/config/system_config. **Tietoturva** -asetukset ja valitse **Muokkaa**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_004.png)

7. Azure perinteinen portaalissa Määritä kertakirjautumisen varapääministeri sivulla, valitse kopioi SAML SSO URL-osoite. 

8. Tee tämä **Tietoturva-asetukset** -sivulla vaiheet alla.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_005.png)

    a. Ota käyttöön **sosiaalisen kirjautuminen**.

    b. Avaa Base64-koodattuja varmennetta Muistiossa, kopioi Leikepöydän sisällön se ja liitä se **OpenSSL varmenne** -tekstiruutuun.

    c-näppäinyhdistelmää. Kirjoita SAM SSO URL-tekstiruutuun`https://<your subdomain>.deputy.com/exec/devapp/samlacs?dpLoginTo=<saml sso url>`
    
    d. Korvaa SAM SSO URL-tekstiruutuun `<your subdomain>` oman alitoimialueen kanssa.

    e. Korvaa SAM SSO URL-tekstiruutuun `<saml sso url>` ja perinteinen Azure-portaalista kopioimasi SAML SSO URL-osoite.

    f. Valitse **Tallenna asetukset**.

9. Perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Seuraava**.
    
    ![Azure AD-Single Sign-On][10]

10. Valitse **Sign Vahvista** -sivulla **Valmis**.  
    
    ![Azure AD-Single Sign-On][11]

### <a name="creating-an-azure-ad-test-user"></a>Azure AD-testikäyttäjän luominen
Tässä osassa tavoitteena on luotava testikäyttäjän kutsutaan Britta Simon perinteinen portaalissa.

![Luo Azure AD-käyttäjä][20]

**Voit luoda testikäyttäjän Azure AD-toimimalla seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen Portal**, valitse vasemmassa siirtymisruudussa.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-deputy-tutorial/create_aaduser_09.png)

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Ylä-valikossa käyttäjäluettelon, näkyviin napsauttamalla **käyttäjät**.
    
    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-deputy-tutorial/create_aaduser_03.png)

4. Avaa **Lisää käyttäjä** -valintaikkunan alareunassa työkalurivillä, valitse **Lisää käyttäjä**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-deputy-tutorial/create_aaduser_04.png)

5. Tee valintaikkunassa **Kerro kyseisen käyttäjän tietoja** -sivulla seuraavasti:

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-deputy-tutorial/create_aaduser_05.png)

    a. Käyttäjänä tyyppi Valitse uusi käyttäjä organisaatiossa.

    b. Kirjoita käyttäjänimi- **tekstiruutuun** **BrittaSimon**.

    c-näppäinyhdistelmää. Valitse **Seuraava**.

6.  Suorita **Käyttäjäprofiili** -sivulla seuraavat toimet:
    
    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-deputy-tutorial/create_aaduser_06.png)

    a. Kirjoita **Ensimmäinen nimi** -tekstiruutuun **Britta**.  

    b. Kirjoita **Sukunimi** -tekstiruutuun tyyppi- **Simon**.

    c-näppäinyhdistelmää. Kirjoita **Näyttönimi** -tekstiruutuun **Britta Simon**.

    d. Valitse **rooli** -luettelosta **käyttäjä**.

    e. Valitse **Seuraava**.

7. Valitse **Hae tilapäinen salasana** -valintaikkunan sivulla **Luo**.
    
    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-deputy-tutorial/create_aaduser_07.png)

8. **Hae tilapäinen salasana** -valintaikkunan sivulla toimimalla seuraavasti:
    
    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-deputy-tutorial/create_aaduser_08.png)

    a. Kirjoita **Uusi salasana**arvo muistiin.

    b. Valitse **Valmis**.   



### <a name="creating-a-deputy-test-user"></a>Varapääministeri testikäyttäjän luominen

Jotta voit kirjautua varapääministeri Azure AD käyttäjät, ne on valmisteltava varapääministeri kyselyjä. Varapääministeri valmistelu on manuaalinen tehtävä.

####<a name="to-provision-a-user-account-perform-the-following-steps"></a>Valmistelu käyttäjätili, toimi seuraavasti:

1.  Kirjaudu sisään varapääministeri yrityksesi sivuston järjestelmänvalvojan oikeuksilla.

2.  Valitse yläreunan siirtymisruudussa **henkilöt**.

    ![Henkilöt] (./media/active-directory-saas-deputy-tutorial/tutorial_deputy_001.png "Henkilöt")

3.  **Lisää henkilöitä** -painiketta ja valitse **Lisää yhdelle henkilölle**.

    ![Henkilöiden lisääminen] (./media/active-directory-saas-deputy-tutorial/tutorial_deputy_002.png "Henkilöiden lisääminen")

4.  Suorita seuraavat vaiheet ja valitse **Tallenna ja kutsu**.

    ![Uusi käyttäjä] (./media/active-directory-saas-deputy-tutorial/tutorial_deputy_003.png "Uusi käyttäjä")

    a. Kirjoita **nimi** -tekstiruutuun **Britta** ja **Simon**.  

    b. Kirjoita sähköpostiosoite, jos haluat säätää Azure AD-tilin **sähköpostiosoite** -tekstiruutuun.

    c-näppäinyhdistelmää. Kirjoita **työtä** -tekstiruutuun bussniess nimi.

    d. Valitse **Tallenna ja kutsua** -painike.

    >[AZURE.NOTE]AAD tilin omistaja vastaanottaa sähköpostia ja johtavan linkin Vahvista tilin, ennen kuin se on aktiivinen. Voit käyttää mitä tahansa muita varapääministeri käyttäjän tilin luontityökalut tai ohjelmointirajapinnan myöntämä varapääministeri säännöstä AAD käyttäjätileille.


### <a name="assigning-the-azure-ad-test-user"></a>Azure AD-testikäyttäjän määrittäminen

Tässä osassa tavoitteena on Britta Simon käyttämään Azure kertakirjautumisen hänen käyttöoikeuksien myöntämistä varapääministeri ottaminen käyttöön.
    
![Määrittää käyttäjälle][200]

**Jos haluat määrittää Britta Simon varapääministeri, toimi seuraavasti:**

1. Perinteinen-portaalissa Avaa sovellukset-näkymässä kansio-näkymässä, valitse **sovellukset** yläreunan-valikossa.
    
    ![Määrittää käyttäjälle][201]

2. Valitse sovellukset-luettelosta **varapääministeri**.
    
    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-deputy-tutorial/tutorial_deputy_50.png)

3. Valitse ylä-valikossa Valitse **käyttäjiä**.
    
    ![Määrittää käyttäjälle][203]

4. Valitse käyttäjät-luettelosta **Britta Simon**.

5. Napsauta työkalurivin alaosassa **määrittäminen**.
    
    ![Määrittää käyttäjälle][205]

### <a name="testing-single-sign-on"></a>Kertakirjautumisen testaaminen

Tässä osassa tavoitteena on Testaa Azure AD yksittäisen Sign-asetusten käyttäminen Access-paneeli.
 
Access-ruudussa varapääministeri-ruutua napsauttaessasi voit olisi Hae automaattisesti kirjautunut sisään varapääministeri sovelluksen.

## <a name="additional-resources"></a>Lisäresursseja

* [Luettelo siitä, miten voit integroida SaaS sovellusten Azure Active Directory-opetusohjelmat](active-directory-saas-tutorial-list.md)
* [Mikä on sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-deputy-tutorial/tutorial_general_205.png
