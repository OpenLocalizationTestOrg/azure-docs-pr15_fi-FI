<properties
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi Ceridian Dayforce HCM | Microsoft Azure"
    description="Opettele määrittämään kertakirjautumisen Azure Active Directory- ja Ceridian Dayforce HCM välillä."
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
    ms.date="09/01/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-ceridian-dayforce-hcm"></a>Opetusohjelma: Ceridian Dayforce HCM Azure Active Directory-integrointi

Tässä opetusohjelmassa tavoitteena on kerrotaan, miten voit integroida Ceridian Dayforce HCM Azure Active Directory (Azure AD).  
Azure AD-Ceridian Dayforce HCM integraation avulla voit seuraavat edut:

- Voit hallita Azure AD, jolla on pääsy Ceridian Dayforce HCM
- Voit ottaa käyttöön automaattisesti Hae kirjautunut käyttöön Ceridian Dayforce HCM (kertakirjautumisen) käyttäjille heidän Azure AD-tilien kanssa
- Voit hallita asiakkaiden yhdessä keskitetyssä sijainnissa - Azure perinteinen portal


Jos haluat tietää tarkempia tietoja SaaS sovelluksen integrointi Azure AD-kohdassa [mitä sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Edellytykset

Määrittää Azure AD-integroinnin Ceridian Dayforce HCM, tarvitset seuraavat:

- Azure AD-tilaus
- Ceridian Dayforce HCM single-merkki käytössä-tilauksessa


> [AZURE.NOTE] Voit esikatsella vaiheet Tässä opetusohjelmassa, Emme suosittele käyttämällä tuotantoympäristössä.


Voit esikatsella vaiheet Tässä opetusohjelmassa-noudattamalla näitä suosituksia:

- Älä käytä tuotantoympäristössä, ellei se on tarpeen.
- Jos sinulla ei ole kokeiluversion Azure AD-ympäristössä, voit hankkia yksi kuukausi kokeiluversion [tähän](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Skenaarion kuvaus
Tässä opetusohjelmassa tavoitteena on, jotta voit testata Azure AD kertakirjautumisen testiympäristössä.  
Tässä opetusohjelmassa kuvatut skenaarion kuuluu kaksi tärkeimmät rakenneosien:

1. Ceridian Dayforce HCM lisääminen valikoimasta
2. Yksittäinen määrittäminen ja testaus Azure AD-on


## <a name="adding-ceridian-dayforce-hcm-from-the-gallery"></a>Ceridian Dayforce HCM lisääminen valikoimasta
Voit määrittää Ceridian Dayforce HCM integroida Azure AD-haluat lisätä Ceridian Dayforce HCM valikoimasta hallitun SaaS sovellusluettelo.

**Jos haluat lisätä Ceridian Dayforce HCM valikoimasta, toimi seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen portal**, valitse vasemmassa siirtymisruudussa. 

    ![Active Directory][1]

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset][2]

4. Valitse sivun alareunassa **Lisää** .

    ![Sovellukset][3]

5. Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Sovellukset][4]

6. Kirjoita hakuruutuun **Ceridian Dayforce HCM**.

    ![Sovellukset](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_01.png)

7. Valitse tulosruudussa **Ceridian Dayforce HCM**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![Sovellukset](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_07.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yksittäinen määrittäminen ja testaus Azure AD-on
Tässä osassa tavoitteena on noudattamalla voit määrittää ja testaa Azure AD kertakirjautumisen Ceridian Dayforce HCM nimeltä "Britta Simon" testikäyttäjän perusteella.

Kertakirjautumisen toimimaan Azure AD on tiedettävä vastaavaan käyttäjän Ceridian Dayforce HCM, Azure AD-käyttäjälle. Toisin sanoen suhteen Azure AD-käyttäjä ja Aiheeseen liittyvät Ceridian Dayforce HCM-linkki on vahvistettava.

Määrittäminen ja testaaminen Azure AD kertakirjautumisen Ceridian Dayforce HCM kanssa, sinun on tehtävä seuraavat hakukyselyn:

1. **[Azure AD määrittäminen Single Sign](#configuring-azure-ad-single-sign-on)** - käyttäjät voivat käyttää tätä toimintoa.
2. **[Käyttäjän testata luominen Azure AD](#creating-an-azure-ad-test-user)** - Testaa Azure AD kertakirjautumisen Britta Simon kanssa.
4. **[Käyttäjän luominen Ceridian Dayforce-HCM testata](#creating-a-ceridian-dayforce-hcm-test-user)** - tapahtumista Britta Simon ovat Ceridian Dayforce HCM kyseistä henkilöä hän Azure AD-esittäminen.
5. **[Käyttäjän testata määrittäminen Azure AD](#assigning-the-azure-ad-test-user)** - Britta Simon käyttämään Azure AD kertakirjautumisen käyttöön.
5. **[Testaus Single Sign](#testing-single-sign-on)** - voit tarkistaa, toimiiko määritykset.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD kertakirjautumisen määrittäminen

Tässä osassa tavoitteena on Azure AD kertakirjautumisen Azure perinteinen portaalissa ottaminen käyttöön ja määrittää kertakirjautumisen Ceridian Dayforce HCM-sovelluksessa.

Ceridian Dayforce HCM sovelluksen odottaa SAML vahvistukset tietyssä muodossa. Toimi Dayforce HCM työryhmän ensin tunnistavan oikea käyttäjätunnus. Microsoft suosittelee käyttäjätunnus muodossa **"nimi"** -määritteen avulla. Voit hallita tämän määritteen **"Atrribute"** -valintaikkunassa. Seuraavassa näyttökuvassa näkyy esimerkki tämän. 

![Kertakirjautumisen määrittäminen](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_02.png) 

**Määrittää Azure AD kertakirjautumisen Ceridian Dayforce HCM, toimimalla seuraavasti:**




1. Valitse Azure perinteinen portaalissa **Ceridian Dayforce HCM** sovelluksen integrointi-sivun yläreunassa valikosta **määritteet**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_81.png) 


1. Määritteet **saml suojaustunnuksen määritteet** -luettelosta Valitse nimimäärite ja valitse sitten **Muokkaa**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_06.png) 


1. Valitse **Muokkaa käyttäjän määrite** -valintaikkunassa seuraavasti:
 
    a. **Määritteen arvo** -luettelosta käyttäjän määritettä haluat käyttää käyttöympäristösi.  
    Esimerkiksi jos haluat käyttää työntekijöillä yksilöllinen käyttäjätunnus ja olet tallentanut määritteen ExtensionAttribute2, valitse **user.extensionattribute2**. 

    b. Valitse **Valmis**.  
    



1. Valitse ylä-valikossa **Pika-aloitus**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-hightail-tutorial/tutorial_general_83.png)  





1. Valitse Azure perinteinen portaalissa integrointi **Ceridian Dayforce HCM** sovellus-sivulla **Määritä kertakirjautumisen**.

    ![Kertakirjautumisen määrittäminen][6] 

2. **Miten käyttäjät voivat kirjautua Ceridian Dayforce HCM haluat käyttää** sivulla Valitse **Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_03.png) 

3. Valitse **Määritä sovelluksen asetukset** -sivulla toimimalla seuraavasti:.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_04.png) 


    a. Kirjoita **Merkki-URL** -tekstiruutuun Sign Ceridian Dayforce HCM sovelluksen käyttäjille käyttämä URL-osoite. Tuotannon ympäristössä Käytä URL-muoto:`https://sso.dayforcehcm.com/DayforcehcmNamespace`

    Selkeyttä korvaaminen ympäristön tai yrityksen Tunnuksesi; nimitilan DayforcehcmNamespace Esimerkki:`https://sso.dayforcehcm.com/contoso`
    
    Testi-ympäristössä Käytä URL-muoto:`https://ssotest.dayforcehcm.com/DayforcehcmNamespace` 

    b. Kirjoita **Vastaus URL** -tekstiruutuun käyttämä Azure AD voivat lähettää vastauksen URL-osoite.  
    Käytä tuotannon ympäristöissä:`https://ncpingfederate.dayforcehcm.com/sp/ACS.saml2`  
    Testi-ympäristössä käytä:`https://fs-test.dayforcehcm.com/sp/ACS.saml2`  
   

4. Valitse **Määritä kertakirjautumisen osoitteessa Ceridian Dayforce HCM** -sivulla toimimalla seuraavasti:

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_05.png) 

    a. Valitse **Lataa**ja tallenna sitten tiedosto tietokoneesta.

    b. Valitse **Seuraava**.


5. Saat määritetty sovelluksen SSO-tukihenkilöstöltä Ceridian Dayforce HCM sähköpostitse ja antaa niille seuraavasti:

    - Ladatun varmennetiedosto
    - **Myöntäjän URL-osoite**
    - **SAML SSO URL-osoite** 
    - **Yksittäisen Kirjaudu ulos palvelun URL-osoite** 


6. Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Seuraava**.

    ![Azure AD-Single Sign-On][10]

7. Valitse **Sign Vahvista** -sivulla **Valmis**.  

    ![Azure AD-Single Sign-On][11]



### <a name="creating-an-azure-ad-test-user"></a>Azure AD-testikäyttäjän luominen
Tässä osassa tavoitteena on luotava testikäyttäjän kutsutaan Britta Simon Azure perinteinen portaalissa.

![Luo Azure AD-käyttäjä][20]

**Voit luoda testikäyttäjän Azure AD-toimimalla seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen portal**, valitse vasemmassa siirtymisruudussa.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_09.png) 

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Ylä-valikossa käyttäjäluettelon, näkyviin napsauttamalla **käyttäjät**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_03.png) 

4. Avaa **Lisää käyttäjä** -valintaikkunan alareunassa työkalurivillä, valitse **Lisää käyttäjä**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_04.png) 

5. Tee valintaikkunassa **Kerro kyseisen käyttäjän tietoja** -sivulla seuraavasti:

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_05.png) 

    a. Käyttäjänä tyyppi Valitse uusi käyttäjä organisaatiossa.

    b. Kirjoita käyttäjänimi- **tekstiruutuun** **BrittaSimon**.

    c-näppäinyhdistelmää. Valitse **Seuraava**.

6.  Suorita **Käyttäjäprofiili** -sivulla seuraavat toimet:

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_06.png) 

    a. Kirjoita **Ensimmäinen nimi** -tekstiruutuun **Britta**.  

    b. Kirjoita **Sukunimi** -tekstiruutuun tyyppi- **Simon**.

    c-näppäinyhdistelmää. Kirjoita **Näyttönimi** -tekstiruutuun **Britta Simon**.

    d. Valitse **rooli** -luettelosta **käyttäjä**.

    e. Valitse **Seuraava**.

7. Valitse **Hae tilapäinen salasana** -valintaikkunan sivulla **Luo**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_07.png) 

8. **Hae tilapäinen salasana** -valintaikkunan sivulla toimimalla seuraavasti:

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-ceridiandayforcehcm-tutorial/create_aaduser_08.png) 

    a. Kirjoita **Uusi salasana**arvo muistiin.

    b. Valitse **Valmis**.   



### <a name="creating-a-ceridian-dayforce-hcm-test-user"></a>Ceridian Dayforce HCM testikäyttäjän luominen

Tässä osassa tavoitteena on nimeltään Britta Simon Ceridian Dayforce HCM käyttäjän luominen. 

Ota käsitteleminen käyttäjiä lisätään Ceridian Dayforce HCM-tukeen yhteyttä Ceridian Dayforce HCM-sovelluksessa. 





### <a name="assigning-the-azure-ad-test-user"></a>Azure AD-testikäyttäjän määrittäminen

Tässä osassa tavoitteena on Britta Simon käyttämään Azure kertakirjautumisen hänen käyttöoikeuden myöntämisestä Ceridian Dayforce HCM ottaminen käyttöön.

![Määrittää käyttäjälle][200] 

**Jos haluat määrittää Britta Simon Ceridian Dayforce HCM, toimi seuraavasti:**

1. Azure perinteinen-portaalissa Avaa sovellukset-näkymässä kansio-näkymässä, valitse **sovellukset** yläreunan-valikossa.

    ![Määrittää käyttäjälle][201] 

2. Valitse sovellukset-luettelosta **Ceridian Dayforce HCM**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_ceridiandayforcehcm_50.png) 

1. Valitse ylä-valikossa Valitse **käyttäjiä**.

    ![Määrittää käyttäjälle][203] 

1. Valitse käyttäjät-luettelosta **Britta Simon**.

2. Napsauta työkalurivin alaosassa **määrittäminen**.

    ![Määrittää käyttäjälle][205]



### <a name="testing-single-sign-on"></a>Kertakirjautumisen testaaminen

Tässä osassa tavoitteena on Testaa Azure AD yksittäisen Sign-asetusten käyttäminen Access-paneeli.  
Access-ruudussa Ceridian Dayforce HCM-ruutua napsauttaessasi sinun pitäisi saada automaattisesti kirjautunut-on Ceridian Dayforce HCM sovelluksen.


## <a name="additional-resources"></a>Lisäresursseja

* [Luettelo siitä, miten voit integroida SaaS sovellusten Azure Active Directory-opetusohjelmat](active-directory-saas-tutorial-list.md)
* [Mikä on sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-ceridiandayforcehcm-tutorial/tutorial_general_205.png
