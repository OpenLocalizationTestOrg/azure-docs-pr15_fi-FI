<properties
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi GaggleAMP | Microsoft Azure"
    description="Opettele määrittämään kertakirjautumisen Azure Active Directory- ja GaggleAMP välillä."
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
    ms.date="10/10/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-gaggleamp"></a>Opetusohjelma: GaggleAMP Azure Active Directory-integrointi

Tässä opetusohjelmassa tavoitteena on, kuinka voit integroida GaggleAMP Azure Active Directory (Azure AD).

Azure AD-integraation GaggleAMP avulla voit seuraavat edut:

- Voit hallita Azure AD, jolla on pääsy GaggleAMP
- Voit ottaa käyttöön automaattisesti Hae kirjautunut käyttöön GaggleAMP (kertakirjautumisen) niiden Azure AD-tilien käyttäjille
- Voit hallita asiakkaiden yhdessä keskitetyssä sijainnissa - Azure perinteinen portal 

Jos haluat tietää tarkempia tietoja SaaS sovelluksen integrointi Azure AD-kohdassa [mitä sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Edellytykset

Määrittää Azure AD-integroinnin GaggleAMP, tarvitset seuraavat:

- Azure AD-tilaus
- GaggleAMP single-merkki käytössä-tilauksessa


> [AZURE.NOTE] Voit esikatsella vaiheet Tässä opetusohjelmassa, Emme suosittele käyttämällä tuotantoympäristössä.


Voit esikatsella vaiheet Tässä opetusohjelmassa-noudattamalla näitä suosituksia:

- Älä käytä tuotantoympäristössä, ellei se on tarpeen.
- Jos sinulla ei ole kokeiluversion Azure AD-ympäristössä, voit hankkia yksi kuukausi kokeiluversion [tähän](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Skenaarion kuvaus
Tässä opetusohjelmassa tavoitteena on, jotta voit testata Azure AD kertakirjautumisen testiympäristössä. 

Tässä opetusohjelmassa kuvatut skenaarion kuuluu kaksi tärkeimmät rakenneosien:

1. GaggleAMP lisääminen valikoimasta
2. Yksittäinen määrittäminen ja testaus Azure AD-on


## <a name="adding-gaggleamp-from-the-gallery"></a>GaggleAMP lisääminen valikoimasta

Voit määrittää GaggleAMP integroida Azure AD-haluat lisätä GaggleAMP valikoimasta hallitun SaaS sovellusluettelo.

**Jos haluat lisätä GaggleAMP valikoimasta, toimi seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen portal**, valitse vasemmassa siirtymisruudussa. 

    ![Active Directory][1]

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset][2]

4. Valitse sivun alareunassa **Lisää** .

    ![Sovellukset][3]

5. Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Sovellukset][4]

6. Kirjoita hakuruutuun **GaggleAMP**.
    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-gaggleamp-tutorial/tutorial_gaggleamp_01.png)

7. Valitse tulosruudussa **GaggleAMP**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-gaggleamp-tutorial/tutorial_gaggleamp_02.png)>

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yksittäinen määrittäminen ja testaus Azure AD-on
Tässä osassa tavoitteena on noudattamalla voit määrittää ja testaa Azure AD kertakirjautumisen GaggleAMP nimeltä "Britta Simon" testikäyttäjän perusteella.

Kertakirjautumisen toimimaan, saat Azure AD on tiedettävä GaggleAMP vastaavaan käyttäjän käyttäjälle Azure AD. Toisin sanoen suhteen Azure AD-käyttäjä ja Aiheeseen liittyvät GaggleAMP-linkki on vahvistettava.

Linkki-yhteys on muodostettu määrittämällä **käyttäjänimi** arvo arvoksi GaggleAMP **käyttäjänimi** Azure AD.

Määrittäminen ja testaaminen Azure AD kertakirjautumisen GaggleAMP kanssa, sinun on tehtävä seuraavat hakukyselyn:

1. **[Azure AD määrittäminen Single Sign](#configuring-azure-ad-single-single-sign-on)** - käyttäjät voivat käyttää tätä toimintoa.
2. **[Käyttäjän testata luominen Azure AD](#creating-an-azure-ad-test-user)** - Testaa Azure AD kertakirjautumisen Britta Simon kanssa.
4. **[Luomisesta GaggleAMP testata käyttäjän](#creating-a-GaggleAMP-test-user)** - tapahtumista Britta Simon ovat GaggleAMP, jotka on linkitetty hän Azure AD esitys.
5. **[Käyttäjän testata määrittäminen Azure AD](#assigning-the-azure-ad-test-user)** - Britta Simon käyttämään Azure AD kertakirjautumisen käyttöön.
5. **[Testaus Single Sign](#testing-single-sign-on)** - voit tarkistaa, toimiiko määritykset.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD kertakirjautumisen määrittäminen

Tässä osassa tavoitteena on Azure AD kertakirjautumisen Azure perinteinen portaalissa ottaminen käyttöön ja määrittää kertakirjautumisen GaggleAMP-sovelluksessa.



**Määrittää Azure AD kertakirjautumisen GaggleAMP, toimimalla seuraavasti:**

1. Valitse Azure perinteinen portaalissa integrointi **GaggleAMP** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen Single Sign** -valintaikkunan avaaminen.

    ![Kertakirjautumisen määrittäminen][6] 

2. **Miten käyttäjät voivat kirjautua GaggleAMP haluat käyttää** sivulla Valitse **Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.
 
    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-gaggleamp-tutorial/tutorial_gaggleamp_03.png) 

3. Valitse **Määritä sovelluksen asetukset** -sivulla toimimalla seuraavasti:

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-gaggleamp-tutorial/tutorial_gaggleamp_04.png) 


    a. Kirjoita merkki-URL-tekstiruutuun URL-osoite käyttämä Sign käyttämällä seuraavaa kaavaa GaggleAMP sovelluksen käyttäjille: **"https://secure4.gaggleamp.com"**.

    > [AZURE.NOTE]Ota yhteyttä [GaggleAMP myyntitiimiksi](mailto:sales@gaggleamp.com) tarvittaessa **Kirjaudu URL-** sovellukselle."

    b. Valitse **Seuraava**.


4. Valitse **Määritä kertakirjautumisen osoitteessa GaggleAMP** -sivulla toimimalla seuraavasti:

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-gaggleamp-tutorial/tutorial_gaggleamp_05.png) 

    a. Valitse **Lataa sertifikaatti**ja tallenna sitten tiedosto tietokoneesta. Tämä varmenne ja metatietojen URL-osoitteet (kohteen tunnus, kirjaudu SSO URL- ja kirjaudu ulos URL-osoite) määrittämään SSO GaggleAMP reunassa annettava.

    b. Valitse **Seuraava**.


5. Siirry toiseen selain luodaan tekijä Gaggle tekniseen tukeen SAML SSO-sivulle (esimerkiksi: *https://accounts.gaggleamp.com/saml_configurations/oXH8sQcP79dOzgFPqrMTyw/edit*).

6. **SAML SSO** -sivulla toimi seuraavasti:  
   
    ![GaggleAMP Single Sign-On](./media/active-directory-saas-gaggleamp-tutorial/tutorial_gaggleamp_06.png) 

    a. Azure perinteinen-portaalissa kopioi **Myöntäjä URL-osoite**ja liitä se sitten **Tunnistetietojen toimittaja myöntäjä** -tekstiruutuun. 

    b. Azure perinteinen portaalissa **Sign-palvelun URL**kopioi ja liitä se sitten **Tunnistetietojen toimittaja yhteen Sign-On URL** -tekstiruutuun. 

    c-näppäinyhdistelmää. Valitse **Tallenna**      
    
    d. Lähetä ladatut varmenteen [GaggleAMP myyntitiimiksi](mailto:sales@gaggleamp.com).


6. Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Seuraava**.

    ![Azure AD-Single Sign-On][10]

7. Valitse **Sign Vahvista** -sivulla **Valmis**.  
  
    ![Azure AD-Single Sign-On][11]




### <a name="creating-an-azure-ad-test-user"></a>Azure AD-testikäyttäjän luominen
Tässä osassa tavoitteena on luotava testikäyttäjän kutsutaan Britta Simon Azure perinteinen portaalissa.

![Luo Azure AD-käyttäjä][20]

**Voit luoda testikäyttäjän Azure AD-toimimalla seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen portal**, valitse vasemmassa siirtymisruudussa.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-gaggleamp-tutorial/create_aaduser_09.png) 

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Ylä-valikossa käyttäjäluettelon, näkyviin napsauttamalla **käyttäjät**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-gaggleamp-tutorial/create_aaduser_03.png) 

4. Avaa **Lisää käyttäjä** -valintaikkunan alareunassa työkalurivillä, valitse **Lisää käyttäjä**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-gaggleamp-tutorial/create_aaduser_04.png) 

5. Tee valintaikkunassa **Kerro kyseisen käyttäjän tietoja** -sivulla seuraavasti:

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-gaggleamp-tutorial/create_aaduser_05.png) 

    a. Käyttäjänä tyyppi Valitse uusi käyttäjä organisaatiossa.

    b. Kirjoita käyttäjänimi- **tekstiruutuun** **BrittaSimon**.

    c-näppäinyhdistelmää. Valitse **Seuraava**.

6.  Suorita **Käyttäjäprofiili** -sivulla seuraavat toimet:

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-gaggleamp-tutorial/create_aaduser_06.png) 

    a. Kirjoita **Ensimmäinen nimi** -tekstiruutuun **Britta**.  

    b. Kirjoita **Sukunimi** -tekstiruutuun tyyppi- **Simon**.

    c-näppäinyhdistelmää. Kirjoita **Näyttönimi** -tekstiruutuun **Britta Simon**.

    d. Valitse **rooli** -luettelosta **käyttäjä**.

    e. Valitse **Seuraava**.

7. Valitse **Hae tilapäinen salasana** -valintaikkunan sivulla **Luo**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-gaggleamp-tutorial/create_aaduser_07.png) 

8. **Hae tilapäinen salasana** -valintaikkunan sivulla toimimalla seuraavasti:

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-gaggleamp-tutorial/create_aaduser_08.png) 

    a. Kirjoita **Uusi salasana**arvo muistiin.

    b. Valitse **Valmis**.   



### <a name="creating-a-gaggleamp-test-user"></a>GaggleAMP testikäyttäjän luominen

Tässä osassa tavoitteena on nimeltään Britta Simon GaggleAMP käyttäjän luominen. GaggleAMP tukee vain perustuvan valmistelu, joka on oletusarvoisesti käytössä.

Ei ole toimi puolestasi sisältö. Uusi käyttäjä luodaan yritys käyttää GaggleAMP, jos sitä ei ole vielä aikana. 


### <a name="assigning-the-azure-ad-test-user"></a>Azure AD-testikäyttäjän määrittäminen

Tässä osassa tavoitteena on Britta Simon käyttämään Azure kertakirjautumisen hänen käyttöoikeuden myöntämisestä GaggleAMP ottaminen käyttöön.

![Määrittää käyttäjälle][200] 

**Jos haluat määrittää Britta Simon GaggleAMP, toimi seuraavasti:**

1. Azure-portaalissa Avaa sovellukset-näkymässä kansio-näkymässä, valitse **sovellukset** yläreunan-valikossa.

    ![Määrittää käyttäjälle][201] 

2. Valitse sovellukset-luettelosta **GaggleAMP**.
    ![Azure-luettelo](./media/active-directory-saas-gaggleamp-tutorial/tutorial_gaggleamp_50.png)

1. Valitse ylä-valikossa Valitse **käyttäjiä**.

    ![Määrittää käyttäjälle][203] 

1. Valitse käyttäjät-luettelosta **Britta Simon**.

2. Napsauta työkalurivin alaosassa **määrittäminen**.

    ![Määrittää käyttäjälle][205]



### <a name="testing-single-sign-on"></a>Kertakirjautumisen testaaminen

Tässä osassa tavoitteena on Testaa Azure AD yksittäisen Sign-asetusten käyttäminen Access-paneeli.

Access-ruudussa GaggleAMP-ruutua napsauttaessasi sinun pitäisi saada automaattisesti kirjautunut-on GaggleAMP sovelluksen.


## <a name="additional-resources"></a>Lisäresursseja

* [Luettelo siitä, miten voit integroida SaaS sovellusten Azure Active Directory-opetusohjelmat](active-directory-saas-tutorial-list.md)
* [Mikä on sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_205.png
