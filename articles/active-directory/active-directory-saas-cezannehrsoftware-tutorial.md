<properties
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi Cezanne HR ohjelmiston | Microsoft Azure"
    description="Opettele määrittämään kertakirjautumisen Azure Active Directory- ja Cezanne HR ohjelmiston välillä."
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
    ms.date="10/26/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-cezanne-hr-software"></a>Opetusohjelma: Cezanne HR ohjelmiston Azure Active Directory-integrointi

Tässä opetusohjelmassa tavoitteena on, kuinka voit integroida Cezanne HR ohjelmiston Azure Active Directory (Azure AD).

Azure AD-integraation Cezanne HR ohjelmisto, jonka avulla seuraavat edut:

- Voit hallita Azure AD, jolla on pääsy Cezanne HR ohjelmisto
- Voit ottaa käyttöön automaattisesti Hae kirjautunut sisään Cezanne HR-ohjelmistoa (kertakirjautumisen) käyttäjille heidän Azure AD-tilien kanssa
- Voit hallita asiakkaiden yhdessä keskitetyssä sijainnissa - Azure perinteinen portal

Jos haluat tietää tarkempia tietoja SaaS sovelluksen integrointi Azure AD-kohdassa [mitä sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Edellytykset

Määrittää Azure AD-integroinnin Cezanne HR ohjelmiston, sinun on seuraavia kohteita:

- Azure AD-tilaus
- Cezanne HR ohjelmiston single-merkki käytössä-tilauksessa


> [AZURE.NOTE] Voit esikatsella vaiheet Tässä opetusohjelmassa, Emme suosittele käyttämällä tuotantoympäristössä.


Voit esikatsella vaiheet Tässä opetusohjelmassa-noudattamalla näitä suosituksia:

- Älä käytä tuotantoympäristössä, ellei se on tarpeen.
- Jos sinulla ei ole kokeiluversion Azure AD-ympäristössä, voit hankkia yksi kuukausi kokeiluversion [tähän](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Skenaarion kuvaus
Tässä opetusohjelmassa tavoitteena on, jotta voit testata Azure AD kertakirjautumisen testiympäristössä.

Tässä opetusohjelmassa kuvatut skenaarion kuuluu kaksi tärkeimmät rakenneosien:

1. Cezanne HR ohjelmiston lisääminen valikoimasta
2. Yksittäinen määrittäminen ja testaus Azure AD-on


## <a name="adding-cezanne-hr-software-from-the-gallery"></a>Cezanne HR ohjelmiston lisääminen valikoimasta
Voit määrittää Cezanne HR ohjelmisto integroida Azure AD-haluat lisätä Cezanne HR ohjelmisto valikoimasta hallitun SaaS sovellusluettelo.

**Voit lisätä Cezanne HR ohjelmiston valikoimasta, toimi seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen Portal**, valitse vasemmassa siirtymisruudussa. 

    ![Active Directory][1]

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .
    
    ![Sovellukset][2]

4. Valitse sivun alareunassa **Lisää** .
    
    ![Sovellukset][3]

5. Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Sovellukset][4]

6. Kirjoita hakuruutuun **Cezanne HR ohjelmiston**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_01.png)

7. Tulokset-ruudussa Valitse **Cezanne HR ohjelmiston**ja valitse sitten **Valmis** , voit lisätä sovelluksen.

    ![Sovelluksen valitseminen valikoimasta](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_0001.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yksittäinen määrittäminen ja testaus Azure AD-on
Tässä osassa tavoitteena on noudattamalla voit määrittää ja testaa Azure AD kertakirjautumisen Cezanne HR ohjelmiston nimeltä "Britta Simon" testikäyttäjän perusteella.

Kertakirjautumisen toimimaan Azure AD on tietää, mitkä asiat vastaavaan käyttäjän Cezanne HR ohjelmiston Azure AD-käyttäjälle. Toisin sanoen linkki Azure AD-käyttäjä ja Aiheeseen liittyvät Cezanne HR ohjelmiston suhteen on vahvistettava.

Linkki-yhteys on muodostettu määrittämällä **käyttäjänimi** arvo arvoksi Cezanne HR ohjelmiston **käyttäjänimi** Azure AD.

Määrittäminen ja testaaminen Azure AD kertakirjautumisen Cezanne HR ohjelmistojen kanssa, sinun on tehtävä seuraavat hakukyselyn:

1. **[Azure AD määrittäminen Single Sign](#configuring-azure-ad-single-single-sign-on)** - käyttäjät voivat käyttää tätä toimintoa.
2. **[Käyttäjän testata luominen Azure AD](#creating-an-azure-ad-test-user)** - Testaa Azure AD kertakirjautumisen Britta Simon kanssa.
3. **[Käyttäjän luominen Cezanne HR-ohjelmiston testata](#creating-a-cezanne-hr-software-test-user)** - on tapahtumista Britta Simon Cezanne HR ohjelmia, jotka on linkitetty hän Azure AD-esittäminen.
4. **[Käyttäjän testata määrittäminen Azure AD](#assigning-the-azure-ad-test-user)** - Britta Simon käyttämään Azure AD kertakirjautumisen käyttöön.
5. **[Testaus Single Sign](#testing-single-sign-on)** - voit tarkistaa, toimiiko määritykset.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD kertakirjautumisen määrittäminen

Tässä osassa Azure AD kertakirjautumisen perinteinen portaalissa ottaminen käyttöön ja määrittäminen kertakirjautumisen Cezanne HR ohjelmistosovellus.

**Voit määrittää Azure AD kertakirjautumisen Cezanne HR-ohjelmistolla toimimalla seuraavasti:**

1. Valitse perinteinen-portaalissa integrointi **Cezanne HR ohjelmiston** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen Single Sign** -valintaikkunan avaaminen.
     
    ![Kertakirjautumisen määrittäminen][6] 

2. **Miten Haluatko, että käyttäjät voivat kirjautua Cezanne HR ohjelmisto** -sivulla Valitse **Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.
    
    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_03.png)

3. Valitse **Määritä sovelluksen asetukset** -sivulla suorittamalla seuraavat vaiheet ja valitse **Seuraava**:

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_04.png)

    a. Kirjoita **Merkki-URL** -tekstiruutuun URL-osoite, käyttämällä seuraavaa kaavaa: `https://w3.cezanneondemand.com/cezannehr/-/<tenant id>`.

    b. Kirjoita **Vastaus URL** -tekstiruutuun URL-osoite, käyttämällä seuraavaa kaavaa: `https://w3.cezanneondemand.com:443/CezanneOnDemand/-/<tenant id>/Saml/samlp`.

    c-näppäinyhdistelmää. Valitse **Seuraava**

    > [AZURE.NOTE] Huomaa, että sinulla on nämä arvot päivitetään todellinen merkki-URL-Osoitteen ja vastaa URL-osoite. Nämä arvot, Cezanne HR ohjelmisto tukeen kautta <mailto:info@cezannehr.com>.

4. Valitse **Määritä kertakirjautumisen Cezanne HR-ohjelma** -sivulla suorittamalla seuraavat vaiheet ja valitse **Seuraava**:

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_05.png)

    a. Valitse **Lataa**ja tallenna sitten tiedosto tietokoneesta.

    b. Valitse **Seuraava**.

5. Eri selainikkunassa Sign Cezanne HR ohjelmiston asiakasympäristöön järjestelmänvalvojana.

6. Valitse vasemmassa siirtymisruudussa **Järjestelmän asetukset**. Valitse **tietoturva-asetukset**. Siirry **Sign-määritys**.

    ![Määrittää yksittäisen Sign-On-sovelluksen puoli](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_000.png)

7. **Käyttäjät voivat kirjautua sisään seuraavan Single Sign-On (SSO)-palvelun** Ohjauspaneelin **SAML 2.0** -valintaruutu ja valitse sen vieressä **Advanced Configuration** -vaihtoehto.

    ![Määrittää yksittäisen Sign-On-sovelluksen puoli](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_001.png)

8. Napsauta **Lisää uusi** -painiketta.

    ![Määrittää yksittäisen Sign-On-sovelluksen puoli](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_002.png)

9. Seuraavien toimien **SAML 2.0-tunnistetietojen toimittajat** -osassa.

    ![Määrittää yksittäisen Sign-On-sovelluksen puoli](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_003.png)

    a. Kirjoita tunnistetietojen toimittaja nimi **Näyttönimi**.

    b. **Kohteen tunnus** tekstiruudun sijoittaa **Kohteen tunnus** arvon Azure AD-sovelluksen määritystoimintoa.

    c-näppäinyhdistelmää. Muuta **SAML sidonnan** "Kirjaa".

    d. **Suojauksen suojaustunnuksen Palvelupäätepisteen** tekstiruudun sijoittaa arvon **Sign-palvelun URL** from Azure AD-sovelluksen määritystoimintoa.

    e. Kirjoita "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name" **ID-määrite käyttäjänimi**.

    f. Valitse Lataa ladatut sertifikaatti Azure AD- **ladata** kuvake.

    g. Napsauta **Ok** -painiketta. 

10. Napsauta **Tallenna** -painiketta.

    ![Määrittää yksittäisen Sign-On-sovelluksen puoli](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_004.png)

11. Perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Seuraava**.
    
    ![Azure AD-Single Sign-On][10]

12. Valitse **Sign Vahvista** -sivulla **Valmis**.  
    
    ![Azure AD-Single Sign-On][11]



### <a name="creating-an-azure-ad-test-user"></a>Azure AD-testikäyttäjän luominen
Tässä osassa tavoitteena on luotava testikäyttäjän kutsutaan Britta Simon perinteinen portaalissa.

![Luo Azure AD-käyttäjä][20]

**Voit luoda testikäyttäjän Azure AD-toimimalla seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen Portal**, valitse vasemmassa siirtymisruudussa.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_09.png)

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Ylä-valikossa käyttäjäluettelon, näkyviin napsauttamalla **käyttäjät**.
    
    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_03.png)

4. Avaa **Lisää käyttäjä** -valintaikkunan alareunassa työkalurivillä, valitse **Lisää käyttäjä**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_04.png)

5. Tee valintaikkunassa **Kerro kyseisen käyttäjän tietoja** -sivulla seuraavasti:

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_05.png)

    a. Käyttäjänä tyyppi Valitse uusi käyttäjä organisaatiossa.

    b. Kirjoita käyttäjänimi- **tekstiruutuun** **BrittaSimon**.

    c-näppäinyhdistelmää. Valitse **Seuraava**.

6.  Suorita **Käyttäjäprofiili** -sivulla seuraavat toimet:
    
    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_06.png)

    a. Kirjoita **Ensimmäinen nimi** -tekstiruutuun **Britta**.  

    b. Kirjoita **Simon** **Sukunimi** -tekstiruutuun.

    c-näppäinyhdistelmää. Kirjoita **Näyttönimi** -tekstiruutuun **Britta Simon**.

    d. Valitse **rooli** -luettelosta **käyttäjä**.

    e. Valitse **Seuraava**.

7. Valitse **Hae tilapäinen salasana** -valintaikkunan sivulla **Luo**.
    
    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_07.png)

8. **Hae tilapäinen salasana** -valintaikkunan sivulla toimimalla seuraavasti:
    
    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-cezannehrsoftware-tutorial/create_aaduser_08.png)

    a. Kirjoita **Uusi salasana**arvo muistiin.

    b. Valitse **Valmis**.   



### <a name="creating-a-cezanne-hr-software-test-user"></a>Cezanne HR ohjelmiston testikäyttäjän luominen

Jotta voit kirjautua Cezanne HR ohjelmiston Azure AD-käyttäjien käyttöön, ne on valmisteltava Cezanne HR ohjelmiston kyselyjä. Cezanne HR ohjelmisto valmistelu on manuaalinen tehtävä.

####<a name="to-provision-a-user-account-perform-the-following-steps"></a>Valmistelu käyttäjätili, toimi seuraavasti:

1.  Kirjaudu sisään Cezanne HR ohjelmiston yrityksesi sivuston järjestelmänvalvojan oikeuksilla.

2.  Valitse vasemmassa siirtymisruudussa **Järjestelmän asetukset**. Siirry **käyttäjien hallinta**. Siirry **Lisää uusi käyttäjä**.

    ![Uusi käyttäjä] (./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_005.png "Uusi käyttäjä")

3.  **Henkilötiedot** -osassa Suorita vaiheet alla:

    ![Uusi käyttäjä] (./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_006.png "Uusi käyttäjä")

    a. Määritä **sisäinen käyttäjä** ei käytössä.

    b. Kirjoita **Ensimmäinen nimi** -tekstiruutuun **Britta**.  

    c-näppäinyhdistelmää. Kirjoita **Simon** **Sukunimi** -tekstiruutuun.

    d. Kirjoita Britta Simon tilisi sähköpostiosoite **Sähköposti** -tekstiruutuun.

4.  **Tilin tiedot** -kohdassa Suorita vaiheet alla:

    ![Uusi käyttäjä] (./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_007.png "Uusi käyttäjä")

    a. Kirjoita **käyttäjänimi** -tekstiruutuun sen sähköpostitilin osoite, Britta Simon.

    b. Kirjoita **salasana** -tekstiruutuun Britta Simon tilin salasana.

    c-näppäinyhdistelmää. Valitse **Suojaus-roolin** **HR Professional** .

    d. Valitse **OK**.

5. Siirry **Single Sign** -välilehti ja valitse **Lisää uusi** **SAML 2.0 tunnisteet** -alueella.

    ![Käyttäjän] (./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_008.png "Käyttäjän")

6. Valitse tunnistetietojen toimittaja **Tunnistetietojen toimittaja** ja **Käyttäjätunnus**tekstiruutuun, kirjoita Britta Simon tilisi sähköpostiosoite.

    ![Käyttäjän] (./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_009.png "Käyttäjän")
    
7. Napsauta **Tallenna** -painiketta.

    ![Käyttäjän] (./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_010.png "Käyttäjän")


### <a name="assigning-the-azure-ad-test-user"></a>Azure AD-testikäyttäjän määrittäminen

Tässä osassa tavoitteena on Britta Simon käyttämään Azure kertakirjautumisen hänen käyttöoikeuden myöntämisestä Cezanne HR ohjelmiston ottaminen käyttöön.
    
![Määrittää käyttäjälle][200]

**Jos haluat määrittää Britta Simon Cezanne HR ohjelmiston, toimi seuraavasti:**

1. Perinteinen-portaalissa Avaa sovellukset-näkymässä kansio-näkymässä, valitse **sovellukset** yläreunan-valikossa.
    
    ![Määrittää käyttäjälle][201]

2. Valitse sovellukset-luettelosta **Cezanne HR ohjelmiston**.
    
    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_cezannehrsoftware_50.png)

3. Valitse ylä-valikossa Valitse **käyttäjiä**.
    
    ![Määrittää käyttäjälle][203]

4. Valitse käyttäjät-luettelosta **Britta Simon**.

5. Napsauta työkalurivin alaosassa **määrittäminen**.
    
    ![Määrittää käyttäjälle][205]

### <a name="testing-single-sign-on"></a>Kertakirjautumisen testaaminen

Tässä osassa tavoitteena on Testaa Azure AD yksittäisen Sign-asetusten käyttäminen Access-paneeli.
 
Kun napsautat Access-ruudussa Cezanne HR ohjelmisto-ruutu, sinun olisi Hae automaattisesti kirjautunut käyttöön Cezanne HR ohjelmistosovellus.

## <a name="additional-resources"></a>Lisäresursseja

* [Luettelo siitä, miten voit integroida SaaS sovellusten Azure Active Directory-opetusohjelmat](active-directory-saas-tutorial-list.md)
* [Mikä on sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-cezannehrsoftware-tutorial/tutorial_general_205.png
