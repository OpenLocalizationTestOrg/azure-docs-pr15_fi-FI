<properties
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi rahoitus-portaalissa | Microsoft Azure"
    description="Opettele määrittämään kertakirjautumisen Azure Active Directory ja rahoitus-portaalin välillä."
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
    ms.date="09/02/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-the-funding-portal"></a>Opetusohjelma: Azure Active Directory-integrointi rahoitus-portaalissa

Tässä opetusohjelmassa kerrotaan, miten voit integroida rahoitus-portaalin ja Azure Active Directory (Azure AD).

Azure AD-integraation rahoitus-portaalissa, jonka avulla seuraavat edut:

- Voit hallita Azure AD, kenellä on oikeus rahoitus-portaalissa
- Voit ottaa käyttöön automaattisesti Hae kirjautunut sisään rahoitus-portaaliin (kertakirjautumisen) käyttäjille heidän Azure AD-tilien kanssa
- Voit hallita asiakkaiden yhdessä keskitetyssä sijainnissa - Azure perinteinen portal

Jos haluat tietää tarkempia tietoja SaaS sovelluksen integrointi Azure AD-kohdassa [mitä sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Edellytykset

Voit määrittää Azure AD-integrointi rahoitus-portaalissa, tarvitset seuraavat kohteet:

- Azure AD-tilaus
- **Rahoitus-portaalin** single-merkki käytössä-tilauksessa


> [AZURE.NOTE] Voit esikatsella vaiheet Tässä opetusohjelmassa, Emme suosittele käyttämällä tuotantoympäristössä.


Voit esikatsella vaiheet Tässä opetusohjelmassa-noudattamalla näitä suosituksia:

- Älä käytä tuotantoympäristössä, ellei se on tarpeen.
- Jos sinulla ei ole kokeiluversion Azure AD-ympäristössä, voit hankkia yksi kuukausi kokeiluversion [tähän](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Skenaarion kuvaus
Tässä opetusohjelmassa testaat Azure AD kertakirjautumisen testiympäristössä. Tässä opetusohjelmassa kuvatut skenaarion kuuluu kaksi tärkeimmät rakenneosien:

1. Rahoitus-portaalin lisääminen valikoimasta
2. Yksittäinen määrittäminen ja testaus Azure AD-on


## <a name="adding-the-funding-portal-from-the-gallery"></a>Rahoitus-portaalin lisääminen valikoimasta
Voit määrittää rahoitus-portaalin integroida Azure AD-haluat lisätä rahoitus-portaalin valikoimasta hallitun SaaS sovellusluettelo.

**Voit lisätä rahoitus-portaalin valikoimasta, toimi seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen portal**, valitse vasemmassa siirtymisruudussa. 

    ![Active Directory][1]

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset][2]

4. Valitse sivun alareunassa **Lisää** .

    ![Sovellukset][3]

5. Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Sovellukset][4]

6. Kirjoita hakuruutuun **Rahoitus-portaalissa**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-thefundingportal-tutorial/tutorial_thefundingportal_01.png)

7. Tulokset-ruudussa Valitse **Rahoitus-portaaliin**ja valitse sitten **Valmis** , voit lisätä sovelluksen.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-thefundingportal-tutorial/tutorial_thefundingportal_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yksittäinen määrittäminen ja testaus Azure AD-on
Tässä osassa Määritä ja testaa Azure AD kertakirjautumisen rahoitus Portal nimeltä "Britta Simon" testikäyttäjän perusteella.

Kertakirjautumisen toimimaan Azure AD on tiedettävä käyttäjälle on rahoitus-portaalissa vastaavaan käyttäjän Azure AD. Toisin sanoen linkki suhteen Azure AD-käyttäjä ja Aiheeseen liittyvät rahoitus-portaalissa on vahvistettava.
Linkki-yhteys on muodostettu määrittämällä **käyttäjänimi** arvo arvoksi **käyttäjänimi** rahoitus-portaalissa Azure AD.

Määrittäminen ja testaaminen Azure AD kertakirjautumisen rahoitus-portaalissa, sinun on tehtävä seuraavat hakukyselyn:

1. **[Azure AD määrittäminen Single Sign](#configuring-azure-ad-single-single-sign-on)** - käyttäjät voivat käyttää tätä toimintoa.
2. **[Käyttäjän testata luominen Azure AD](#creating-an-azure-ad-test-user)** - Testaa Azure AD kertakirjautumisen Britta Simon kanssa.
4. **[Käyttäjän luominen rahoitus-portaalin testata](#creating-a-the-funding-portal-test-user)** - tapahtumista Britta Simon on rahoitus-portaalissa, jotka on linkitetty hän Azure AD-esittäminen.
5. **[Käyttäjän testata määrittäminen Azure AD](#assigning-the-azure-ad-test-user)** - Britta Simon käyttämään Azure AD kertakirjautumisen käyttöön.
5. **[Testaus Single Sign](#testing-single-sign-on)** - voit tarkistaa, toimiiko määritykset.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD kertakirjautumisen määrittäminen

Tässä osassa tavoitteena on Azure AD kertakirjautumisen Azure perinteinen portaalissa ottaminen käyttöön ja määrittää kertakirjautumisen rahoitus Portal-sovelluksessa.

Rahoitus Portal-sovellus odottaa SAML vahvistukset sisältämään nimeltä "externalId1" määrite. "ExternalId1" arvon pitäisi olla tunnistettavassa studentID. Määritä tämän sovelluksen "externalId1"-ryhmän. Voit hallita näitä määritteitä arvot sovelluksen **"Atrributes"** -välilehdestä. Seuraavassa näyttökuvassa näkyy esimerkki tämän.

![Kertakirjautumisen määrittäminen](./media/active-directory-saas-thefundingportal-tutorial/tutorial_thefundingportal_03.png) 

**Jos haluat määrittää Azure AD kertakirjautumisen rahoitus-portaalissa, toimimalla seuraavasti:**

1. Valitse Azure perinteinen portaalissa **Rahoitus-portaalissa** sovelluksen integrointi-sivun yläreunassa valikosta **määritteet**.
     
    ![Kertakirjautumisen määrittäminen][5]

2. Valitse SAML suojaustunnuksen määritteet-valintaikkunassa Lisää "externalId1"-määrite.

    a. Valitse **Lisää käyttäjä-määrite** -valintaikkunan avaaminen **Lisää käyttäjä-määrite** . 
    
    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-thefundingportal-tutorial/tutorial_thefundingportal_05.png)

    b. Kirjoita määritteen nimi "externalId1" **Määritteen nimi** -tekstiruutuun.

    c-näppäinyhdistelmää. **Määritteen arvo** -luettelosta määrite, jota haluat käyttää käyttöympäristösi. Esimerkiksi jos olet tallentanut StudentID-arvo ExtensionAttribute1, valitse user.extensionattribute1.

    d. Valitse **Valmis**. Valitse **Ota muutokset käyttöön**.

1. Valitse ylä-valikossa **Pika-aloitus**.

    ![Kertakirjautumisen määrittäminen][6]

2. Valitse perinteinen-portaalissa **Rahoitus Portal** -sovellus integrointi-sivulla **Määritä kertakirjautumisen** **Määrittäminen Single Sign** -valintaikkunan avaaminen.

    ![Kertakirjautumisen määrittäminen][7] 

3. **Miten haluat kirjautua rahoitus-portaaliin käyttäjät** -sivulla Valitse **Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.
    
    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-thefundingportal-tutorial/tutorial_thefundingportal_06.png)

4. Valitse **Määritä sovelluksen asetukset** -sivulla toimimalla seuraavasti: 

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-thefundingportal-tutorial/tutorial_thefundingportal_07.png)


    a. Kirjoita merkki-URL-tekstiruutuun URL-osoite, käyttämällä seuraavaa kaavaa: `https://<subdomain>.regenteducation.net/`.

    b. Valitse **Seuraava**.

5. Valitse **Määritä kertakirjautumisen rahoitus-portaalissa osoitteessa** -sivulla **Lataa metatiedot**- ja tallenna se tietokoneeseen.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-thefundingportal-tutorial/tutorial_thefundingportal_08.png)

6. Saat määritetty sovelluksen SSO-rahoitus-portaalin tuelta. Ne auttavat kanssa ERISNIMI kanavan SSO määrittämiseen. Ota Huomaa, että sinulla on lähettää sähköpostia ja liitä ladattu tiedosto metatietojen info@regenteducation.com.

7. Perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Seuraava**.
    
    ![Azure AD-Single Sign-On][10]

8. Valitse **Sign Vahvista** -sivulla **Valmis**.  
    
    ![Azure AD-Single Sign-On][11]

### <a name="creating-an-azure-ad-test-user"></a>Azure AD-testikäyttäjän luominen
Tässä osassa voit luoda testikäyttäjän kutsutaan Britta Simon perinteinen portaalissa.

![Luo Azure AD-käyttäjä][20]

**Voit luoda testikäyttäjän Azure AD-toimimalla seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen portal**, valitse vasemmassa siirtymisruudussa.
    
    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-thefundingportal-tutorial/create_aaduser_09.png) 

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Ylä-valikossa käyttäjäluettelon, näkyviin napsauttamalla **käyttäjät**.
    
    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-thefundingportal-tutorial/create_aaduser_03.png) 

4. Avaa **Lisää käyttäjä** -valintaikkunan alareunassa työkalurivillä, valitse **Lisää käyttäjä**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-thefundingportal-tutorial/create_aaduser_04.png) 

5. Tee valintaikkunassa **Kerro kyseisen käyttäjän tietoja** -sivulla seuraavasti:
 
    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-thefundingportal-tutorial/create_aaduser_05.png) 

    a. Käyttäjänä tyyppi Valitse uusi käyttäjä organisaatiossa.

    b. Kirjoita käyttäjänimi- **tekstiruutuun** **BrittaSimon**.

    c-näppäinyhdistelmää. Valitse **Seuraava**.

6.  Suorita **Käyttäjäprofiili** -sivulla seuraavat toimet:

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-thefundingportal-tutorial/create_aaduser_06.png) 

    a. Kirjoita **Ensimmäinen nimi** -tekstiruutuun **Britta**.  

    b. Kirjoita **Sukunimi** -tekstiruutuun tyyppi- **Simon**.

    c-näppäinyhdistelmää. Kirjoita **Näyttönimi** -tekstiruutuun **Britta Simon**.

    d. Valitse **rooli** -luettelosta **käyttäjä**.

    e. Valitse **Seuraava**.

7. Valitse **Hae tilapäinen salasana** -valintaikkunan sivulla **Luo**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-thefundingportal-tutorial/create_aaduser_07.png) 

8. **Hae tilapäinen salasana** -valintaikkunan sivulla toimimalla seuraavasti:

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-thefundingportal-tutorial/create_aaduser_08.png) 

    a. Kirjoita **Uusi salasana**arvo muistiin.

    b. Valitse **Valmis**.   



### <a name="creating-a-the-funding-portal-test-user"></a>Rahoitus Portal-testikäyttäjän luominen

Tässä osassa voit luoda käyttäjän kutsutaan Britta Simon rahoitus-portaalissa. Jos et tiedä, miten voit lisätä Britta Simon rahoitus-portaalissa, toimi Ota rahoitus-portaalin tukihenkilöstöösi testikäyttäjän lisääminen ja SSO ottaminen käyttöön. Ota niitä info@regenteducation.com.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD-testikäyttäjän määrittäminen

Tässä osassa voit ottaa käyttöön Britta Simon käyttämään Azure kertakirjautumisen hänen käyttöoikeuksien myöntäminen rahoitus-portaaliin.

![Määrittää käyttäjälle][200] 

**Jos haluat määrittää Britta Simon rahoitus-portaaliin, toimi seuraavasti:**

1. Perinteinen-portaalissa Avaa sovellukset-näkymässä kansio-näkymässä, valitse **sovellukset** yläreunan-valikossa.

    ![Määrittää käyttäjälle][201] 

2. Valitse sovellukset-luettelosta **Rahoitus-portaalissa**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-thefundingportal-tutorial/tutorial_thefundingportal_09.png) 

1. Valitse ylä-valikossa Valitse **käyttäjiä**.

    ![Määrittää käyttäjälle][203] 

1. Valitse kaikki käyttäjät-luettelosta **Britta Simon**.

2. Napsauta työkalurivin alaosassa **määrittäminen**.

    ![Määrittää käyttäjälle][205]


### <a name="testing-single-sign-on"></a>Kertakirjautumisen testaaminen

Tässä osassa tavoitteena on Testaa Azure AD yksittäisen Sign-asetusten käyttäminen Access-paneeli.

Access-ruudussa rahoitus Portal-ruutua napsauttaessasi sinun pitäisi saada automaattisesti kirjautunut käyttöön rahoitus Portal-sovellus.

## <a name="additional-resources"></a>Lisäresursseja

* [Luettelo siitä, miten voit integroida SaaS sovellusten Azure Active Directory-opetusohjelmat](active-directory-saas-tutorial-list.md)
* [Mikä on sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_04.png


[5]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_05.png
[6]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_06.png
[7]:  ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_050.png
[10]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_060.png
[11]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_070.png
[20]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-thefundingportal-tutorial/tutorial_general_205.png
