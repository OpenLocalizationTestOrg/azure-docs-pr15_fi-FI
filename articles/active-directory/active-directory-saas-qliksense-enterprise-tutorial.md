<properties
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi Qlik mielessä yrityksen | Microsoft Azure"
    description="Opettele määrittämään kertakirjautumisen Azure Active Directory- ja Qlik mielessä yrityksen välillä."
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
    ms.date="08/31/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-qlik-sense-enterprise"></a>Opetusohjelma: Qlik mielessä yrityksen Azure Active Directory-integrointi

Tässä opetusohjelmassa kerrotaan, miten voit integroida Qlik mielessä yrityksen Azure Active Directory (Azure AD).

Azure AD-Qlik mielessä yrityksen integraation avulla voit seuraavat edut:

- Voit hallita Azure AD, jolla on pääsy Qlik mielessä yrityksen
- Voit ottaa käyttöön automaattisesti Hae kirjautunut sisään Qlik järkevää Enterpriseen (kertakirjautumisen) käyttäjille heidän Azure AD-tilien kanssa
- Voit hallita asiakkaiden yhdessä keskitetyssä sijainnissa - Azure perinteinen portal

Jos haluat tietää tarkempia tietoja SaaS sovelluksen integrointi Azure AD-kohdassa [mitä sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Edellytykset

Määrittää Azure AD-integroinnin Qlik mielessä yrityksen, tarvitset seuraavat:

- Azure AD-tilaus
- Qlik mielessä yrityksen single-merkki käytössä-tilauksessa


> [AZURE.NOTE] Voit esikatsella vaiheet Tässä opetusohjelmassa, Emme suosittele käyttämällä tuotantoympäristössä.


Voit esikatsella vaiheet Tässä opetusohjelmassa-noudattamalla näitä suosituksia:

- Älä käytä tuotantoympäristössä, ellei se on tarpeen.
- Jos sinulla ei ole kokeiluversion Azure AD-ympäristössä, voit hankkia yksi kuukausi kokeiluversion [tähän](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Skenaarion kuvaus
Tässä opetusohjelmassa testaat Azure AD kertakirjautumisen testiympäristössä.

Tässä opetusohjelmassa kuvatut skenaarion kuuluu kaksi tärkeimmät rakenneosien:

1. Qlik mielessä yrityksen lisääminen valikoimasta
2. Yksittäinen määrittäminen ja testaus Azure AD-on


## <a name="adding-qlik-sense-enterprise-from-the-gallery"></a>Qlik mielessä yrityksen lisääminen valikoimasta
Voit määrittää Qlik mielessä yrityksen integroida Azure AD-haluat lisätä Qlik mielessä yrityksen valikoimasta hallitun SaaS sovellusluettelo.

**Jos haluat lisätä Qlik mielessä yrityksen valikoimasta, toimi seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen portal**, valitse vasemmassa siirtymisruudussa.

    ![Active Directory][1]
2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset][2]

4. Valitse sivun alareunassa **Lisää** .

    ![Sovellukset][3]

5. Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Sovellukset][4]

6. Kirjoita hakuruutuun **Qlik mielessä yrityksen**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_01.png)

7. Valitse tulosruudussa **Qlik mielessä yrityksen**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yksittäinen määrittäminen ja testaus Azure AD-on
Tässä osassa Määritä ja testaa Azure AD kertakirjautumisen Qlik mielessä yrityksen nimeltä "Britta Simon" testikäyttäjän perusteella.

Kertakirjautumisen toimimaan Azure AD on tiedettävä käyttäjälle on Qlik mielessä yrityksen vastaavaan käyttäjän Azure AD. Toisin sanoen linkki Azure AD-käyttäjä ja Aiheeseen liittyvät Qlik mielessä yrityksen suhteen on vahvistettava.

Linkki-yhteys on muodostettu määrittämällä **käyttäjänimi** arvo arvoksi Qlik mielessä yrityksen **käyttäjänimi** Azure AD.

Määrittäminen ja testaaminen Azure AD kertakirjautumisen Qlik mielessä Enterprise-sinun on tehtävä seuraavat hakukyselyn:

1. **[Azure AD määrittäminen Single Sign](#configuring-azure-ad-single-sign-on)** - käyttäjät voivat käyttää tätä toimintoa.
2. **[Käyttäjän testata luominen Azure AD](#creating-an-azure-ad-test-user)** - Testaa Azure AD kertakirjautumisen Britta Simon kanssa.
3. **[Käyttäjän luominen Qlik mielessä-yrityksen testata](#creating-a-qliksense-enterprise-test-user)** - on tapahtumista Britta Simon Qlik mielessä yritys, joka on linkitetty hän Azure AD esitys.
4. **[Käyttäjän testata määrittäminen Azure AD](#assigning-the-azure-ad-test-user)** - Britta Simon käyttämään Azure AD kertakirjautumisen käyttöön.
5. **[Testaus Single Sign](#testing-single-sign-on)** - voit tarkistaa, toimiiko määritykset.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD kertakirjautumisen määrittäminen

Tässä osassa ottaminen käyttöön Azure AD kertakirjautumisen perinteinen portaalissa ja määrittäminen kertakirjautumisen Qlik mielessä yrityssovellukseen.


**Voit määrittää Azure AD kertakirjautumisen Qlik mielessä Enterprise toimimalla seuraavasti:**

1. Valitse perinteinen-portaalissa integrointi **Qlik mielessä yrityksen** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen Single Sign** -valintaikkunan avaaminen.
     
    ![Kertakirjautumisen määrittäminen][6] 

2. **Miten käyttäjät voivat kirjautua Qlik mielessä yrityksen haluat käyttää** sivulla Valitse **Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_03.png) 

3. Valitse **Määritä sovelluksen asetukset** -sivulla toimimalla seuraavasti:

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_04.png) 

    a. Kirjoita **Merkki-URL** -tekstiruutuun URL-osoite käyttämä Sign käyttämällä seuraavaa kaavaa Qlik mielessä yrityksen sovelluksen käyttäjille: **https://\<Qlik järkevää täysin Qualifed Hostname\>: 443 / < Virtual välityspalvelimen etuliite\>/samlauthn/**.
    
    > [AZURE.NOTE] Huomaa vinoviiva tämän URI lopussa.  On pakollinen.

    b. Valitse **Seuraava**
 
4. Valitse **Määritä kertakirjautumisen osoitteessa Qlik mielessä Enterprise** -sivulla toimimalla seuraavasti:

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_05.png)

    a. Valitse **Lataa metatiedot**ja tallenna sitten tiedosto tietokoneesta.  Valmistaudu metatietojen tiedoston muokkaaminen ennen Qlik mielessä palvelimelle lataamisen.

    b. Valitse **Seuraava**.

5. Valmistele Federation metatietojen XML-tiedosto, niin, että voit ladata, Qlik mielessä palvelimeen.

    > [AZURE.NOTE] Ennen lataamista IdP metatietojen Qlik mielessä palvelimeen, se on voi muokata, jos haluat poistaa tiedot, jotta ERISNIMI yhteistoiminta Azure AD ja Qlik mielessä.

    ![QlikSense][qs24]

    a. Avaa ladatut Azure tekstieditorissa FederationMetaData.xml tiedosto.

    b. Etsi arvo **RoleDescriptor**.  On neljä merkintöjä (kahdet osan alku- ja).

    c-näppäinyhdistelmää. Poista RoleDescriptor tunnisteet ja kaikki tiedot tiedostosta välillä.

    d. Tallenna tiedosto ja säilyttää sen lähellä käyttöä jäljempänä tässä asiakirjassa.

6. Siirry käyttäjänä, jolla voit luoda virtuaalisen välityspalvelimen määritykset, Qlik mielessä Qlik Management Console (QMC).

7. Napsauta QMC, Virtual välityspalvelimen-valikkovaihtoehto.

    ![QlikSense][qs6] 

8. Valitse Luo uusi-painike näytön alareunasta.

    ![QlikSense][qs7]

9. Virtual välityspalvelimen Muokkaa näyttö tulee näkyviin.  Näytön oikeassa reunassa on tekemiseen näkyvissä asetukset-valikko.

    ![QlikSense][qs9]

10. Tunnus-valikon vaihtoehto valittuna Kirjoita Azure virtual välityspalvelimen määritykset tunnistetietoja.

    ![QlikSense][qs8]  
    
    a. Kuvaus-kenttään on kutsumanimi virtual välityspalvelimen määritykset.  Kirjoita kuvaus-arvoa.
    
    b. Etuliite-kenttä ilmaisee virtual välityspalvelimen päätepisteen kanssa Azure AD Single Sign-On järkevää Qlik muodostamisesta.  Kirjoita yksilöllinen etuliite tämän virtual välityspalvelimen nimi.

    c-näppäinyhdistelmää. Istunnon käyttämättömänä aikakatkaisu (minuutteina) on aikakatkaisu yhteyksien tämän virtual välityspalvelimen kautta.

    d. Istunnon eväste otsikkonimi on tallentaminen istunnon tunnisteen käyttäjä saa onnistuneen todennuksen jälkeen Qlik mielessä istunnon evästenimi.  Tämä nimi on oltava yksilöllinen.

11. Valitse todennus-valikon vaihtoehto saat sen näkyviin.  Todennus-näyttö tulee näkyviin.

    ![QlikSense][qs10]

    a. **Anonyymin käyttötilan** avattavasta määrää, jos anonyymit käyttäjät voivat käyttää Qlik mielessä virtual välityspalvelimen kautta.  Oletusarvo on ei anonyymin.

    b. **Todentamismenetelmä** avattavassa määrittää käyttää virtual välityspalvelimen todennusmalli.  Valitse SAML avattavasta luettelosta.  Lisää vaihtoehtoja näkyy tuloksena.

    c-näppäinyhdistelmää. **SAML host URI-kenttä**Lisää hostname käyttäjät syöttävät käyttämään Qlik mielessä tämän SAML virtual välityspalvelimen kautta.  Isäntänimi on järkevää Qlik palvelimen uri.

    d. Kirjoita **SAML kohteen tunnus**saman SAML host URI-kentän arvo.

    e. **SAML IdP metatietojen** on muokattu aiemmin **Federation-metatiedot Muokkaa Azure AD-määritys** -osassa tiedosto.  Voit poistaa tiedot välillä Azure AD asianmukaisen toiminnan varmistamiseksi **ennen lataamista IdP metatiedot, tiedosto on voi muokata** ja Qlik mielessä.  **Lue ohjeet, jos se ei ole vielä voi muokata.**  Jos tiedosto on muokattu napsauttamalla Selaa-painiketta ja valitse muokattu metatietojen tiedosto ladataan virtual välityspalvelimen määritykset.

    f. Määritteen nimi tai rakenteiden viite SAML-määritteen, joka edustaa **käyttäjätunnus** Azure AD lähettää Qlik mielessä-palvelimeen.  Rakenteen viittaus tiedot ovat käytettävissä Azure app näyttöihin kirjaa määrityksessä.  Jos haluat käyttää name-määrite, **Kirjoita http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name**.

    g. Anna **käyttäjän kansio** , joka on liitettynä käyttäjille, kun ne todentavat Qlik mielessä server Azure AD-arvo.  Englanninkielisissä arvot on kirjoitettava **hakasulkeisiin []**.  Käyttämään lähetetään Azure AD SAML-vahvistus-määrite, Kirjoita määritteen nimi tämä tekstiä ruudun **ilman** hakasulkeita.

    h. **SAML Allekirjoitusalgoritmi** asettaa palveluntarjoaja (Kirjoita tässä tapauksessa Qlik mielessä server) varmenteen allekirjoitukseen virtual välityspalvelimen määritykset.  Jos Qlik mielessä server käyttää luotettu varmenteen, joka on luotu Microsoft parannetun RSA ja salauksen AES-palvelu, muuttaa SAML Allekirjoitusalgoritmi **SHA-256**.

    voin. SAML määrite yhdistäminen-osan avulla muita määritteitä, kuten ryhmät lähetetään Qlik mielessä suojaussäännöt käytettäviksi.

12. Valitse vaihtoehto saat sen näkyviin kuormituksen.  Kuormituksen tasaamisen-näyttö tulee näkyviin.

    ![QlikSense][qs11]

13. Napsauta Lisää uusi palvelimen solmu painiketta, valitse ohjelma-solmu tai solmujen Qlik mielessä lähettää istuntoon ja kuormituksen tasaamisen tarkoituksiin ja napsauta Lisää-painiketta.

    ![QlikSense][qs12]

14. Valitse Lisäasetukset-vaihtoehto, jos haluat tuoda näkyviin. Lisäasetukset-näytön tulee näkyviin.

    ![QlikSense][qs13]

    a. Host valkoinen luettelo tunnistaa isäntänimet, joissa hyväksytään muodostamisessa Qlik mielessä-palvelimeen.  **Anna käyttäjien määrittää muodostamisessa Qlik mielessä palvelimeen isäntänimi.** Isäntänimi on saman arvon kuin SAML host uri https:// ilman.

15. Valitse Käytä-painiketta.

    ![QlikSense][qs14]

16. Voit hyväksyä varoitussanoma, joka ilmoittaa välityspalvelimet linkitetty virtual välityspalvelimen käynnistetään valitsemalla OK.

    ![QlikSense][qs15]

17. Näytön oikeaan reunaan assosioituneissa kohteiden valikko tulee näkyviin.  Valitse välityspalvelimet-valikkovaihtoehto.

    ![QlikSense][qs16]

18. Välityspalvelimen näyttö tulee näkyviin.  Valitse linkki välityspalvelimen virtual välityspalvelimen alaosan linkki-painike.

    ![QlikSense][qs17]

19. Valitse välityspalvelimen solmu, joka tukee virtual välityspalvelimen yhteyden ja linkki-painiketta.  Kun linkität, välityspalvelimen näkyy luettelossa liittyvät välityspalvelimet.

    ![QlikSense][qs18]
    ![QlikSense][qs19]

20. Päivitä QMC viesti näkyy noin 5-10 sekunnin kuluttua.  Napsauta Päivitä QMC-painiketta.

    ![QlikSense][qs20]

21. Kun QMC päivittää, valitse Virtual välityspalvelimet valikkovaihtoehto. Uusi SAML virtual välityspalvelimen merkintä näkyy näytössä taulukossa.  Virtuaalinen välityspalvelimen tapahtuman yhdellä napsautuksella.

    ![QlikSense][qs51]

22. Näytön alareunassa Aktivoi Lataa SP metatiedot-painiketta.  Napsauta Tallenna tiedostoon metatiedot Lataa SP metatiedot-painiketta.

    ![QlikSense][qs52]

23. Avaa sp metatieto-tiedosto.  Noudata **tunnus** tapahtuma- ja **AssertionConsumerService** tapahtuma.  Nämä arvot vastaavat **tunniste** ja **URL-Osoitteen sisäänkirjautuminen** Azure AD-sovelluksen määritys. Jos ne eivät vastaa toisiaan sinun olisi korvaa ne Azure AD-sovelluksen määrittäminen ohjatussa.

    ![QlikSense][qs53]

24. Perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Seuraava**.
    
    ![Azure AD-Single Sign-On][10]

25. Valitse **Sign Vahvista** -sivulla **Valmis**.  
 
    ![Azure AD-Single Sign-On][11]


### <a name="creating-an-azure-ad-test-user"></a>Azure AD-testikäyttäjän luominen
Tässä osassa voit luoda testikäyttäjän kutsutaan Britta Simon perinteinen portaalissa.


![Luo Azure AD-käyttäjä][20]

**Voit luoda testikäyttäjän Azure AD-toimimalla seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen portal**, valitse vasemmassa siirtymisruudussa.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_09.png) 

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Ylä-valikossa käyttäjäluettelon, näkyviin napsauttamalla **käyttäjät**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_03.png) 

4. Avaa **Lisää käyttäjä** -valintaikkunan alareunassa työkalurivillä, valitse **Lisää käyttäjä**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_04.png) 

5. Suorita valintaikkunan **Kerro kyseisen käyttäjän tietoja** -sivulla seuraavat toimet:  ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_05.png) 

    a. Käyttäjänä tyyppi Valitse uusi käyttäjä organisaatiossa.

    b. Kirjoita käyttäjänimi- **tekstiruutuun** **BrittaSimon**.

    c-näppäinyhdistelmää. Valitse **Seuraava**.

6.  Suorita valintaikkunan **Käyttäjäprofiili** -sivulla seuraavat toimet: ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_06.png) 

    a. Kirjoita **Ensimmäinen nimi** -tekstiruutuun **Britta**.  

    b. Kirjoita **Sukunimi** -tekstiruutuun tyyppi- **Simon**.

    c-näppäinyhdistelmää. Kirjoita **Näyttönimi** -tekstiruutuun **Britta Simon**.

    d. Valitse **rooli** -luettelosta **käyttäjä**.

    e. Valitse **Seuraava**.

7. Valitse **Hae tilapäinen salasana** -valintaikkunan sivulla **Luo**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_07.png) 

8. **Hae tilapäinen salasana** -valintaikkunan sivulla toimimalla seuraavasti:

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_08.png) 

    a. Kirjoita **Uusi salasana**arvo muistiin.

    b. Valitse **Valmis**.   


### <a name="creating-an-qlik-sense-enterprise-test-user"></a>Qlik mielessä Enterprise-testikäyttäjän luominen

Tässä osassa nimeltä Britta Simon Qlik mielessä yrityksen käyttäjän luominen. Toimi Qlik mielessä yrityksen tukihenkilöstöösi voit lisätä käyttäjiä Qlik mielessä Enterprise-ympäristössä.


### <a name="assigning-the-azure-ad-test-user"></a>Azure AD-testikäyttäjän määrittäminen

Tässä osassa voit ottaa käyttöön Britta Simon käyttämään Azure kertakirjautumisen hänen käyttöoikeuden myöntämisestä Qlik mielessä yrityksen.

![Määrittää käyttäjälle][200] 

**Jos haluat määrittää Britta Simon Qlik mielessä yrityksen, toimi seuraavasti:**

1. Perinteinen-portaalissa Avaa sovellukset-näkymässä kansio-näkymässä, valitse **sovellukset** yläreunan-valikossa.

    ![Määrittää käyttäjälle][201] 

2. Valitse sovellukset-luettelosta **Qlik mielessä yrityksen**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_50.png) 

3. Valitse ylä-valikossa Valitse **käyttäjiä**.

    ![Määrittää käyttäjälle][203]

4. Valitse käyttäjät-luettelosta **Britta Simon**.

5. Napsauta työkalurivin alaosassa **määrittäminen**.

    ![Määrittää käyttäjälle][205]


## <a name="testing-single-sign-on"></a>Kertakirjautumisen testaaminen

Tässä osassa voit testata Azure AD yksittäisen Sign-asetusten käyttäminen Access-paneeli.

Access-ruudussa Qlik mielessä Enterprise-ruutua napsauttaessasi sinun olisi Hae automaattisesti kirjautunut käyttöön Qlik mielessä yrityssovellukseen.


## <a name="additional-resources"></a>Lisäresursseja

* [Luettelo siitä, miten voit integroida SaaS sovellusten Azure Active Directory-opetusohjelmat](active-directory-saas-tutorial-list.md)
* [Mikä on sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_205.png

[qs6]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_06.png
[qs7]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_07.png
[qs8]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_08.png
[qs9]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_09.png
[qs10]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_10.png
[qs11]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_11.png
[qs12]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_12.png
[qs13]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_13.png
[qs14]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_14.png
[qs15]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_15.png
[qs16]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_16.png
[qs17]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_17.png
[qs18]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_18.png
[qs19]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_19.png
[qs20]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_20.png
[qs21]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_21.png
[qs22]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_22.png
[qs23]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_23.png
[qs24]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_24.png
[qs25]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_25.png
[qs26]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_26.png
[qs51]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_51.png
[qs52]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_52.png
[qs53]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_53.png