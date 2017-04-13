<properties
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi Optimizely | Microsoft Azure"
    description="Opettele määrittämään kertakirjautumisen Azure Active Directory- ja Optimizely välillä."
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
    ms.date="09/11/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-optimizely"></a>Opetusohjelma: Optimizely Azure Active Directory-integrointi

Tässä opetusohjelmassa kerrotaan, miten voit integroida Optimizely Azure Active Directory (Azure AD).

Azure AD-integraation Optimizely avulla voit seuraavat edut:

- Voit hallita Azure AD, jolla on pääsy Optimizely
- Voit ottaa käyttöön automaattisesti Hae kirjautunut käyttöön Optimizely (kertakirjautumisen) niiden Azure AD-tilien käyttäjille
- Voit hallita asiakkaiden yhdessä keskitetyssä sijainnissa - Azure perinteinen portal

Jos haluat tietää tarkempia tietoja SaaS sovelluksen integrointi Azure AD-kohdassa [mitä sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Edellytykset

Määrittää Azure AD-integroinnin Optimizely, tarvitset seuraavat:

- Azure AD-tilaus
- **Optimizely** single-merkki käytössä-tilauksessa


> [AZURE.NOTE] Voit esikatsella vaiheet Tässä opetusohjelmassa, Emme suosittele käyttämällä tuotantoympäristössä.


Voit esikatsella vaiheet Tässä opetusohjelmassa-noudattamalla näitä suosituksia:

- Älä käytä tuotantoympäristössä, ellei se on tarpeen.
- Jos sinulla ei ole kokeiluversion Azure AD-ympäristössä, voit hankkia yksi kuukausi kokeiluversion [tähän](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Skenaarion kuvaus
Tässä opetusohjelmassa testaat Azure AD kertakirjautumisen testiympäristössä. Tässä opetusohjelmassa kuvatut skenaarion kuuluu kaksi tärkeimmät rakenneosien:

1. Optimizely lisääminen valikoimasta
2. Yksittäinen määrittäminen ja testaus Azure AD-on


## <a name="adding-optimizely-from-the-gallery"></a>Optimizely lisääminen valikoimasta
Voit määrittää Optimizely integroida Azure AD-haluat lisätä Optimizely valikoimasta hallitun SaaS sovellusluettelo.

**Jos haluat lisätä Optimizely valikoimasta, toimi seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen portal**, valitse vasemmassa siirtymisruudussa. 

    ![Active Directory][1]

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset][2]

4. Valitse sivun alareunassa **Lisää** .

    ![Sovellukset][3]

5. Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Sovellukset][4]

6. Kirjoita hakuruutuun **Optimizely**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_01.png)

7. Valitse tulosruudussa **Optimizely**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yksittäinen määrittäminen ja testaus Azure AD-on
Tässä osassa Määritä ja testaa Azure AD kertakirjautumisen Optimizely nimeltä "Britta Simon" testikäyttäjän perusteella.

Kertakirjautumisen toimimaan Azure AD on tiedettävä käyttäjälle on Optimizely vastaavaan käyttäjän Azure AD. Toisin sanoen suhteen Azure AD-käyttäjä ja Aiheeseen liittyvät Optimizely-linkki on vahvistettava.
Linkki-yhteys on muodostettu määrittämällä **käyttäjänimi** arvo arvoksi Optimizely **käyttäjänimi** Azure AD.

Määrittäminen ja testaaminen Azure AD kertakirjautumisen Optimizely kanssa, sinun on tehtävä seuraavat hakukyselyn:

1. **[Azure AD määrittäminen Single Sign](#configuring-azure-ad-single-single-sign-on)** - käyttäjät voivat käyttää tätä toimintoa.
2. **[Käyttäjän testata luominen Azure AD](#creating-an-azure-ad-test-user)** - Testaa Azure AD kertakirjautumisen Britta Simon kanssa.
4. **[Käyttäjän luominen Optimizely testata](#creating-an-optimizely-test-user)** - on tapahtumista Britta Simon Optimizely kyseistä henkilöä hän Azure AD esitys.
5. **[Käyttäjän testata määrittäminen Azure AD](#assigning-the-azure-ad-test-user)** - Britta Simon käyttämään Azure AD kertakirjautumisen käyttöön.
5. **[Testaus Single Sign](#testing-single-sign-on)** - voit tarkistaa, toimiiko määritykset.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD-Single Sign-On määrittäminen

Tässä osassa tavoitteena on Azure AD kertakirjautumisen Azure perinteinen portaalissa ottaminen käyttöön ja määrittää kertakirjautumisen Optimizely-sovelluksessa.

Optimizely sovelluksen odottaa SAML vahvistukset sisältämään nimeltä "sähköposti-määrite. "Sähköposti-arvon pitäisi olla tunnistettu Optimizely sähköpostiviestin, voit saada oikeiksi Azure AD. Määritä tämän sovelluksen "sähköpostin"-ryhmän. Voit hallita näitä määritteitä arvot sovelluksen **"Atrributes"** -välilehdestä. Seuraavassa näyttökuvassa näkyy esimerkki tämän. 


![Kertakirjautumisen määrittäminen](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_03.png) 


**Määrittää Azure AD kertakirjautumisen Optimizely, toimimalla seuraavasti:**

1. Valitse Azure perinteinen portaalissa **Optimizely** sovelluksen integrointi-sivun yläreunassa valikosta **määritteet**.
     
    ![Kertakirjautumisen määrittäminen][5]

2. Valitse SAML suojaustunnuksen määritteet-valintaikkunassa Lisää "sähköposti-määrite.

    a. Valitse **Lisää käyttäjä-määrite** -valintaikkunan avaaminen **Lisää käyttäjä-määrite** . 
    
    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_05.png)

    b. Kirjoita määritteen nimi "sähköposti- **Määritteen nimi** -tekstiruutuun.

    c-näppäinyhdistelmää. Valitse **Määritteen arvo** -luettelosta määritteen arvon "userprincipalname- tai jokin arvo, joka sisältää Azure AD tunnista sähköpostia ja Optimizely.

    d. Valitse **Valmis**.
3. Valitse ylä-valikossa **Pika-aloitus**.

    ![Kertakirjautumisen määrittäminen][6]
4. Valitse perinteinen-portaalissa integrointi **Optimizely** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen Single Sign** -valintaikkunan avaaminen.

    ![Kertakirjautumisen määrittäminen][7] 

5. **Miten käyttäjät voivat kirjautua Optimizely haluat käyttää** sivulla Valitse **Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.
    
    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_06.png)

6. Valitse **Määritä sovelluksen asetukset** -sivulla toimimalla seuraavasti: 

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_07.png)


    a. Kirjoita **Merkki-URL** -tekstiruutuun:`https://app.optimizely.net/contoso`

    b. Kirjoita **tunnus** -tekstiruutuun:`urn:auth0:optimizely:contoso`

    c-näppäinyhdistelmää. Valitse **Seuraava**. 


    > [AZURE.NOTE] **Merkki-URL-osoite** ja **tunnuksen** arvot ovat vain todellisten arvojen paikkamerkkejä. Löydät aquiring ohjeet Optimizely todelliset arvot jäljempänä tässä opetusohjelmassa.

7. Valitse **Määritä kertakirjautumisen osoitteessa Optimizely** -sivulla toimimalla seuraavasti:

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_08.png)

    a. Valitse **Lataa**ja tallenna sitten tiedosto tietokoneesta.

    b. Kopioi **Sign-palvelun URL-osoite**.

8. Saat määritetty sovelluksen SSO-yhteyttä Optimizely Tilienhallinta ja anna seuraavat tiedot:

    - Ladatun varmenne 
    - Sign-palvelun URL-osoite
 
    Vastauksena sähköpostin Optimizely voi merkki-URL-osoite (SP käynnistämä SSO) ja tunnuksen (palveluntarjoajan kohteen tunnus)-arvot.

9. Palaa **App-asetusten määrittäminen** valintasivun ja toimi sitten seuraavasti:

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_07.png)

    a. Kirjoita **Merkki-URL** -tekstiruutuun Optimizely myöntämä **SP käynnistämä SSO URL-osoite** .

    b. Kirjoita **tunnus** -tekstiruutuun Optimizely myöntämä **Palvelun palvelun kohteen tunnus** .

    c-näppäinyhdistelmää. Valitse **Seuraava**.

10. Valitse **Määritä kertakirjautumisen osoitteessa Optimizely** -sivulla toimimalla seuraavasti:
    
    ![Azure AD-Single Sign-On][10]

    a. Valitse Sign määritysten vahvistus.

    b. Valitse **Seuraava**.

11. Valitse **Sign Vahvista** -sivulla **Valmis**.  
    
    ![Azure AD-Single Sign-On][11]

12. Eri selainikkunassa Sign Optimizely-sovellukseen.
13. Valitse tilin nimi oikeassa yläkulmassa ja valitse sitten **Tiliasetukset**.

    ![Azure AD-Single Sign-On](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_09.png)

14. Valitse tili-välilehdessä valintaruutu **SSO käyttöön** kertakirjautumisen **Yleiskatsaus** -osan kohdassa.

    ![Azure AD-Single Sign-On](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_10.png)

### <a name="creating-an-azure-ad-test-user"></a>Azure AD-testikäyttäjän luominen
Tässä osassa voit luoda testikäyttäjän kutsutaan Britta Simon perinteinen portaalissa.
Valitse käyttäjät-luettelosta **Britta Simon**.

![Luo Azure AD-käyttäjä][20]

**Voit luoda testikäyttäjän Azure AD-toimimalla seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen portal**, valitse vasemmassa siirtymisruudussa.
    
    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-optimizely-tutorial/create_aaduser_09.png) 

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Ylä-valikossa käyttäjäluettelon, näkyviin napsauttamalla **käyttäjät**.
    
    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-optimizely-tutorial/create_aaduser_03.png) 

4. Avaa **Lisää käyttäjä** -valintaikkunan alareunassa työkalurivillä, valitse **Lisää käyttäjä**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-optimizely-tutorial/create_aaduser_04.png) 

5. Tee valintaikkunassa **Kerro kyseisen käyttäjän tietoja** -sivulla seuraavasti:
 
    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-optimizely-tutorial/create_aaduser_05.png) 

    a. Käyttäjänä tyyppi Valitse uusi käyttäjä organisaatiossa.

    b. Kirjoita käyttäjänimi- **tekstiruutuun** **BrittaSimon**.

    c-näppäinyhdistelmää. Valitse **Seuraava**.

6.  Suorita **Käyttäjäprofiili** -sivulla seuraavat toimet:

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-optimizely-tutorial/create_aaduser_06.png) 

    a. Kirjoita **Ensimmäinen nimi** -tekstiruutuun **Britta**.  

    b. Kirjoita **Sukunimi** -tekstiruutuun tyyppi- **Simon**.

    c-näppäinyhdistelmää. Kirjoita **Näyttönimi** -tekstiruutuun **Britta Simon**.

    d. Valitse **rooli** -luettelosta **käyttäjä**.

    e. Valitse **Seuraava**.

7. Valitse **Hae tilapäinen salasana** -valintaikkunan sivulla **Luo**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-optimizely-tutorial/create_aaduser_07.png) 

8. **Hae tilapäinen salasana** -valintaikkunan sivulla toimimalla seuraavasti:

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-optimizely-tutorial/create_aaduser_08.png) 

    a. Kirjoita **Uusi salasana**arvo muistiin.

    b. Valitse **Valmis**.   



### <a name="creating-an-optimizely-test-user"></a>Optimizely-testikäyttäjän luominen

Tässä osassa nimeltä Britta Simon Optimizely käyttäjän luominen.

1. Valitse Aloitus-sivulla **yhteiskäyttäjät** -välilehti
2. Valitse **Uusi Collaborator** uuden collaborator lisääminen projektiin.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-optimizely-tutorial/create_aaduser_10.png)

3.  Kirjoita sähköpostiosoite ja Määritä rooli. Valitse **Kutsu**.


    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-optimizely-tutorial/create_aaduser_11.png)

4. He saavat sähköposti-kutsu. Voit käyttää sähköpostiosoite. heillä on kirjautua Optimizely.


### <a name="assigning-the-azure-ad-test-user"></a>Azure AD-testikäyttäjän määrittäminen

Tässä osassa voit ottaa käyttöön Britta Simon käyttämään Azure kertakirjautumisen hänen käyttöoikeuksien myöntämistä Optimizely.

![Määrittää käyttäjälle][200] 

**Jos haluat määrittää Britta Simon Optimizely, toimi seuraavasti:**

1. Perinteinen-portaalissa Avaa sovellukset-näkymässä kansio-näkymässä, valitse **sovellukset** yläreunan-valikossa.

    ![Määrittää käyttäjälle][201] 

2. Valitse sovellukset-luettelosta **Optimizely**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-optimizely-tutorial/tutorial_optimizely_50.png) 

1. Valitse ylä-valikossa Valitse **käyttäjiä**.

    ![Määrittää käyttäjälle][203] 

1. Valitse kaikki käyttäjät-luettelosta **Britta Simon**.

2. Napsauta työkalurivin alaosassa **määrittäminen**.

    ![Määrittää käyttäjälle][205]


### <a name="testing-single-sign-on"></a>Kertakirjautumisen testaaminen

Tässä osassa tavoitteena on Testaa Azure AD yksittäisen Sign-asetusten käyttäminen Access-paneeli.

Access-ruudussa Optimizely-ruutua napsauttaessasi sinun pitäisi saada automaattisesti kirjautunut-on Optimizely sovelluksen.

## <a name="additional-resources"></a>Lisäresursseja

* [Luettelo siitä, miten voit integroida SaaS sovellusten Azure Active Directory-opetusohjelmat](active-directory-saas-tutorial-list.md)
* [Mikä on sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_04.png


[5]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_05.png
[6]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_06.png
[7]:  ./media/active-directory-saas-optimizely-tutorial/tutorial_general_050.png
[10]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_060.png
[11]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_070.png
[20]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-optimizely-tutorial/tutorial_general_205.png
