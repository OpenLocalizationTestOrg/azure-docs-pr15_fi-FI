<properties
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi Pluralsight | Microsoft Azure"
    description="Opettele määrittämään kertakirjautumisen Azure Active Directory- ja Pluralsight välillä."
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


# <a name="tutorial-azure-active-directory-integration-with-pluralsight"></a>Opetusohjelma: Pluralsight Azure Active Directory-integrointi

Tässä opetusohjelmassa tavoitteena on, kuinka voit integroida Pluralsight Azure Active Directory (Azure AD).

Azure AD-integraation Pluralsight avulla voit seuraavat edut:

- Voit hallita Azure AD, jolla on pääsy Pluralsight
- Voit ottaa käyttöön automaattisesti Hae kirjautunut käyttöön Pluralsight (kertakirjautumisen) niiden Azure AD-tilien käyttäjille
- Voit hallita asiakkaiden yhdessä keskitetyssä sijainnissa - Azure perinteinen portal


Jos haluat tietää tarkempia tietoja SaaS sovelluksen integrointi Azure AD-kohdassa [mitä sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Edellytykset

Määrittää Azure AD-integroinnin Pluralsight, tarvitset seuraavat:

- Azure tilauksen
- Pluralsight single-merkki käytössä-tilauksessa


> [AZURE.NOTE] Voit esikatsella vaiheet Tässä opetusohjelmassa, Emme suosittele käyttämällä tuotantoympäristössä.


Voit esikatsella vaiheet Tässä opetusohjelmassa-noudattamalla näitä suosituksia:

- Älä käytä tuotantoympäristössä, ellei se on tarpeen.
- Jos sinulla ei ole kokeiluversion Azure AD-ympäristössä, voit hankkia yksi kuukausi kokeiluversion [tähän](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Skenaarion kuvaus
Tässä opetusohjelmassa tavoitteena on, jotta voit testata Azure AD kertakirjautumisen testiympäristössä. 

Tässä opetusohjelmassa kuvatut skenaarion kuuluu kaksi tärkeimmät rakenneosien:

1. Pluralsight lisääminen valikoimasta
2. Yksittäinen määrittäminen ja testaus Azure AD-on


## <a name="adding-pluralsight-from-the-gallery"></a>Pluralsight lisääminen valikoimasta
Voit määrittää Pluralsight integroida Azure AD-haluat lisätä Pluralsight valikoimasta hallitun SaaS sovellusluettelo.

**Jos haluat lisätä Pluralsight valikoimasta, toimi seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen portal**, valitse vasemmassa siirtymisruudussa. 

    ![Active Directory][1]

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset][2]

4. Valitse sivun alareunassa **Lisää** .
 
    ![Sovellukset][3]

5. Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Sovellukset][4]

6. Kirjoita hakuruutuun **Pluralsight**.
 
    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-pluralsight-tutorial/tutorial_pluralsight_01.png)

7. Valitse tulosruudussa **Pluralsight**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-pluralsight-tutorial/tutorial_pluralsight_06.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yksittäinen määrittäminen ja testaus Azure AD-on
Tässä osassa tavoitteena on noudattamalla voit määrittää ja testaa Azure AD kertakirjautumisen Pluralsight nimeltä "Britta Simon" testikäyttäjän perusteella.

Määrittäminen ja testaaminen Azure AD kertakirjautumisen Pluralsight kanssa, sinun on tehtävä seuraavat hakukyselyn:

1. **[Azure AD määrittäminen Single Sign](#configuring-azure-ad-single-single-sign-on)** - käyttäjät voivat käyttää tätä toimintoa.
2. **[Käyttäjän testata luominen Azure AD](#creating-an-azure-ad-test-user)** - Testaa Azure AD kertakirjautumisen Britta Simon kanssa.
4. **[Luomisesta Pluralsight testata käyttäjän](#creating-a-pluralsight-test-user)** - tapahtumista Britta Simon ovat Pluralsight, jotka on linkitetty hän Azure AD esitys.
5. **[Käyttäjän testata määrittäminen Azure AD](#assigning-the-azure-ad-test-user)** - Britta Simon käyttämään Azure AD kertakirjautumisen käyttöön.
5. **[Testaus Single Sign](#testing-single-sign-on)** - voit tarkistaa, toimiiko määritykset.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD-Single Sign-On määrittäminen

Tässä osassa tavoitteena on Azure AD kertakirjautumisen Azure perinteinen portaalissa ottaminen käyttöön ja määrittää kertakirjautumisen Pluralsight-sovelluksessa.

Pluralsight sovelluksen odottaa SAML vahvistukset tietyssä muodossa, jossa pyydetään mukautettujen määritteiden yhdistämismäärityksiä lisääminen SAML suojaustunnuksen määritteet-määritys. Seuraavassa näyttökuvassa näkyy esimerkki tämän. 

![Kertakirjautumisen määrittäminen](./media/active-directory-saas-pluralsight-tutorial/tutorial_pluralsight_02.png) 

Voit myös lisätä **"yksilöllisen tunnuksen"** -määrite, kuten työntekijätunnus tai jotakin muuta oikean arvon, joka sopii organisaatiollesi. Huomaa, että tämä ei ole pakollinen määrite; Voit kuitenkin lisätä se tunnistaa yksittäiselle käyttäjälle. 

**Määrittää Azure AD kertakirjautumisen Pluralsight, toimimalla seuraavasti:**

1. Valitse Azure perinteinen portaalissa **Pluralsight** sovelluksen integrointi-sivun yläreunassa valikosta **määritteet**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-pluralsight-tutorial/tutorial_general_81.png) 



1. Jos haluat poistaa tarpeettomat **SAML suojaustunnuksen määritteitä**, toimimalla seuraavasti: 

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-pluralsight-tutorial/2829.png) 


    a. Punainen-ruudussa edellä olevan taulukon kunkin käyttäjän-määritteen määrite päälle ja valitse sitten Poista. 




1. Voit lisätä tarvittavat **SAML suojaustunnuksen määritteitä**, kullekin riville seuraavassa taulukossa näkyvät toimimalla seuraavasti:

  	| Määritteen nimi | Määritteen arvo |
  	| --- | --- |    
  	| Etunimi| User.givenName |
  	| Sukunimi  | User.surname |
  	| Sähköpostin | User.Mail |

    a. Valitse **Lisää käyttäjä Attribure** -valintaikkunan avaaminen **Lisää käyttäjä-määrite** .

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-pluralsight-tutorial/tutorial_general_82.png) 


    b. Kirjoita määritteen nimi näkyy kyseisen rivin **Attrubute nimi** -tekstiruutuun.

    c-näppäinyhdistelmää. **Määritteen arvo** -luettelosta selsect kyseisen rivin määritteen arvo.

    d. Valitse **Valmis**.  
    


1. Valitse **Ota muutokset käyttöön**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-pluralsight-tutorial/3232.png)  



1. Valitse ylä-valikossa **Pika-aloitus**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-pluralsight-tutorial/tutorial_general_83.png)  









1. Valitse Azure perinteinen portaalissa integrointi **Pluralsight** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen Single Sign** -valintaikkunan avaaminen.

    ![Kertakirjautumisen määrittäminen][6] 

2. **Miten käyttäjät voivat kirjautua Pluralsight haluat käyttää** sivulla Valitse **Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-pluralsight-tutorial/tutorial_pluralsight_03.png) 

3. Valitse **Määritä sovelluksen asetukset** -sivulla toimimalla seuraavasti:.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-pluralsight-tutorial/tutorial_pluralsight_04.png) 


    a. Kirjoita merkki-URL-tekstiruutuun URL-osoite käyttämä Sign käyttämällä seuraavaa kaavaa Pluralsight sovelluksen käyttäjille:`https://<instance name>.pluralsight.com/sso/<comapny name>`

    b. Valitse **Seuraava**.


4. Valitse **Määritä kertakirjautumisen osoitteessa Pluralsight** -sivulla toimimalla seuraavasti:  ![määrittäminen Single Sign-On](./media/active-directory-saas-pluralsight-tutorial/tutorial_pluralsight_05.png) 

    a. Valitse **Lataa metatiedot**ja tallenna sitten tiedosto tietokoneesta.

    b. Valitse **Seuraava**.


5. Saat määritetty sovelluksen SSO-yhteyttä Pluralsight [Professional Services](mailTo:professionalservices@pluralsight.com) -tiimin ja anna ladatut metatieto-tiedosto.


6. Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Seuraava**.
  
    ![Azure AD-Single Sign-On][10]

7. Valitse **Sign Vahvista** -sivulla **Valmis**.  
 
    ![Azure AD-Single Sign-On][11]



### <a name="creating-an-azure-ad-test-user"></a>Azure AD-testikäyttäjän luominen
Tässä osassa tavoitteena on luotava testikäyttäjän kutsutaan Britta Simon Azure perinteinen portaalissa.

Valitse käyttäjät-luettelosta **Britta Simon**.

![Luo Azure AD-käyttäjä][20]

**Voit luoda testikäyttäjän Azure AD-toimimalla seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen portal**, valitse vasemmassa siirtymisruudussa.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-pluralsight-tutorial/create_aaduser_09.png) 

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Ylä-valikossa käyttäjäluettelon, näkyviin napsauttamalla **käyttäjät**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-pluralsight-tutorial/create_aaduser_03.png) 

4. Avaa **Lisää käyttäjä** -valintaikkunan alareunassa työkalurivillä, valitse **Lisää käyttäjä**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-pluralsight-tutorial/create_aaduser_04.png) 

5. Tee valintaikkunassa **Kerro kyseisen käyttäjän tietoja** -sivulla seuraavasti:

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-pluralsight-tutorial/create_aaduser_05.png) 

    a. Käyttäjänä tyyppi Valitse uusi käyttäjä organisaatiossa.

    b. Kirjoita käyttäjänimi- **tekstiruutuun** **BrittaSimon**.

    c-näppäinyhdistelmää. Valitse **Seuraava**.

6.  Suorita **Käyttäjäprofiili** -sivulla seuraavat toimet:

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-pluralsight-tutorial/create_aaduser_06.png) 

    a. Kirjoita **Ensimmäinen nimi** -tekstiruutuun **Britta**.  

    b. Kirjoita **Sukunimi** -tekstiruutuun tyyppi- **Simon**.

    c-näppäinyhdistelmää. Kirjoita **Näyttönimi** -tekstiruutuun **Britta Simon**.

    d. Valitse **rooli** -luettelosta **käyttäjä**.

    e. Valitse **Seuraava**.

7. Valitse **Hae tilapäinen salasana** -valintaikkunan sivulla **Luo**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-pluralsight-tutorial/create_aaduser_07.png) 

8. **Hae tilapäinen salasana** -valintaikkunan sivulla toimimalla seuraavasti:

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-pluralsight-tutorial/create_aaduser_08.png) 

    a. Kirjoita **Uusi salasana**arvo muistiin.

    b. Valitse **Valmis**.   



### <a name="creating-a-pluralsight-test-user"></a>Pluralsight testikäyttäjän luominen

Tässä osassa tavoitteena on nimeltään Britta Simon Pluralsight käyttäjän luominen. Toimi Pluralsight tukihenkilöstöösi voit lisätä käyttäjiä Pluralsight-tilille. 


> [AZURE.NOTE]Jos haluat luoda luodun käyttäjän manuaalisesti, on Pluralsight-tukeen.


### <a name="assigning-the-azure-ad-test-user"></a>Azure AD-testikäyttäjän määrittäminen

Tässä osassa tavoitteena on Britta Simon käyttämään Azure kertakirjautumisen hänen käyttöoikeuden myöntämisestä Pluralsight ottaminen käyttöön.

![Määrittää käyttäjälle][200] 

**Jos haluat määrittää Britta Simon Pluralsight, toimi seuraavasti:**

1. Azure perinteinen-portaalissa Avaa sovellukset-näkymässä kansio-näkymässä, valitse **sovellukset** yläreunan-valikossa.

    ![Määrittää käyttäjälle][201] 

2. Valitse sovellukset-luettelosta **Pluralsight**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-pluralsight-tutorial/tutorial_pluralsight_50.png) 

1. Valitse ylä-valikossa Valitse **käyttäjiä**.

    ![Määrittää käyttäjälle][203] 

1. Valitse käyttäjät-luettelosta **Britta Simon**.

2. Napsauta työkalurivin alaosassa **määrittäminen**.

    ![Määrittää käyttäjälle][205]



### <a name="testing-single-sign-on"></a>Kertakirjautumisen testaaminen

Tässä osassa tavoitteena on Testaa Azure AD yksittäisen Sign-asetusten käyttäminen Access-paneeli.

Access-ruudussa Pluralsight-ruutua napsauttaessasi sinun pitäisi saada automaattisesti kirjautunut-on Pluralsight sovelluksen.


## <a name="additional-resources"></a>Lisäresursseja

* [Luettelo siitä, miten voit integroida SaaS sovellusten Azure Active Directory-opetusohjelmat](active-directory-saas-tutorial-list.md)
* [Mikä on sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_205.png
