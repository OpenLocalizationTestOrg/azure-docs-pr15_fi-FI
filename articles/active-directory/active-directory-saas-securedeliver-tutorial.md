<properties
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi Novatus | Microsoft Azure"
    description="Opettele määrittämään kertakirjautumisen Azure Active Directory ja SUOJATA toimittaa välillä."
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
    ms.date="09/26/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-secure-deliver"></a>Opetusohjelma: Azure Active Directory integrointi suojatun aikana

Tässä opetusohjelmassa tavoitteena on, kuinka voit SUOJATA toimittaa integroida Azure Active Directory (Azure AD).  
SUOJATTU toimittaa integraation Azure AD, jonka avulla seuraavat edut:

- Voit hallita Azure AD, kenellä on oikeus suojatun aikana
- Voit ottaa käyttöön automaattisesti Hae kirjautunut sisään suojatun aikana (kertakirjautumisen) käyttäjille heidän Azure AD-tilien kanssa
- Voit hallita asiakkaiden yhdessä keskitetyssä sijainnissa - Azure perinteinen portal

Jos haluat tietää tarkempia tietoja SaaS sovelluksen integrointi Azure AD-kohdassa [mitä sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Edellytykset

SUOJATTU pitää määrittää Azure AD-integrointi, tarvitset seuraavat:

- Azure tilauksen
- SUOJATTU pitää yhden-merkki käytössä-tilauksessa


> [AZURE.NOTE] Voit esikatsella vaiheet Tässä opetusohjelmassa, Emme suosittele käyttämällä tuotantoympäristössä.


Voit esikatsella vaiheet Tässä opetusohjelmassa-noudattamalla näitä suosituksia:

- Älä käytä tuotantoympäristössä, ellei se on tarpeen.
- Jos sinulla ei ole kokeiluversion Azure AD-ympäristössä, voit hankkia yksi kuukausi kokeiluversion [tähän](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Skenaarion kuvaus
Tässä opetusohjelmassa tavoitteena on, jotta voit testata Azure AD kertakirjautumisen testiympäristössä.  
Tässä opetusohjelmassa kuvatut skenaarion kuuluu kaksi tärkeimmät rakenneosien:

1. SUOJATTU toimittaa lisääminen valikoimasta
2. Yksittäinen määrittäminen ja testaus Azure AD-on


## <a name="adding-secure-deliver-from-the-gallery"></a>SUOJATTU toimittaa lisääminen valikoimasta
Voit määrittää, suojattu toimittaa integroida Azure AD-haluat lisätä suojatun toimittaa valikoimasta hallitun SaaS sovellusten luetteloon.

**Jos haluat lisätä valikoimasta suojatun aikana, toimi seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen portal**, valitse vasemmassa siirtymisruudussa. 

    ![Active Directory][1]

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset][2]

4. Valitse sivun alareunassa **Lisää** .

    ![Sovellukset][3]

5. Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Sovellukset][4]

6. Kirjoita hakuruutuun **Suojatun aikana**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-securedeliver-tutorial/tutorial_securedeliver_01.png)

7. Valitse tulokset-ruudun Valitse **SECURE aikana**ja valitse sitten **Valmis** , voit lisätä sovelluksen.
    
    ![Sovelluksen logo ja nimi-valikoimassa](./media/active-directory-saas-securedeliver-tutorial/tutorial_securedeliver_06.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yksittäinen määrittäminen ja testaus Azure AD-on
Tässä osassa tavoitteena on noudattamalla voit määrittää ja testaa Azure AD kertakirjautumisen suojatun toimittaa nimeltä "Britta Simon" testikäyttäjän perusteella.

Kertakirjautumisen toimimaan Azure AD on tiedettävä vastaavaan käyttäjän suojatun toimittaa Azure AD-käyttäjälle. Toisin sanoen linkki Azure AD-käyttäjä ja Aiheeseen liittyvät-suojatun toimittaa suhteen on vahvistettava.  
Linkki-yhteys on muodostettu määrittämällä arvo **käyttäjänimi** **käyttäjänimi** suojatun toimittaa arvona Azure AD.

Määrittäminen ja testaaminen Azure AD kertakirjautumisen kanssa suojatun aikana, sinun on tehtävä seuraavat hakukyselyn:

1. **[Azure AD määrittäminen Single Sign](#configuring-azure-ad-single-single-sign-on)** - käyttäjät voivat käyttää tätä toimintoa.
2. **[Käyttäjän testata luominen Azure AD](#creating-an-azure-ad-test-user)** - Testaa Azure AD kertakirjautumisen Britta Simon kanssa.
3. **[Käyttäjän luominen suojatun pitää testata](#creating-a-secure-deliver-test-user)** - tapahtumista Britta Simon ovat henkilöitä, jotka on linkitetty hän Azure AD esitys.
5. **[Käyttäjän testata määrittäminen Azure AD](#assigning-the-azure-ad-test-user)** - Britta Simon käyttämään Azure AD kertakirjautumisen käyttöön.
5. **[Testaus Single Sign](#testing-single-sign-on)** - voit tarkistaa, toimiiko määritykset.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD-Single Sign-On määrittäminen

Tässä osassa tavoitteena on Azure AD kertakirjautumisen Azure perinteinen portaalissa ottaminen käyttöön ja määrittämään kertakirjautumisen suojatun TOIMITA-sovelluksessa.



**SUOJATTU pitää määrittää Azure AD kertakirjautumisen, toimimalla seuraavasti:**

1. Valitse Azure perinteinen portaalissa, valitse **SECURE toimittaa** sovelluksen integrointi-sivulla **Määritä kertakirjautumisen** **Määrittäminen Single Sign** -valintaikkunan avaaminen.

    ![Kertakirjautumisen määrittäminen][6] 

2. **Miten haluat kirjautua sisään suojatun aikana käyttäjät** -sivulla Valitse **Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.
 
    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-securedeliver-tutorial/tutorial_securedeliver_03.png) 

3. Valitse **Määritä sovelluksen asetukset** -sivulla suorittamalla seuraavat vaiheet ja valitse sitten **Seuraava**:
 
    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-securedeliver-tutorial/tutorial_securedeliver_04.png) 

    a. Kirjoita **Merkki-URL** -tekstiruutuun URL-osoite käyttämä Sign käyttämällä seuraavaa kaavaa suojatun toimittaa sovelluksen käyttäjille: **"https://i-securedeliver.jp/sd/\<yrityksen nimi\>/jsf/login/sso"**.

    b. SUOJATTU toimittaa tukeen kautta [iw-sd-support@fujifilm.com](mailto:iw-sd-support@fujifilm.com) saat vuokraajan URL-osoite, jos et tiedä arvo.

    c-näppäinyhdistelmää. Kirjoita **tunnus** -tekstiruutuun vuokraajan URL-osoite. 

    d. Valitse **Seuraava**


4. Valitse **Määritä kertakirjautumisen osoitteessa suojatun TOIMITA** -sivulla suorittamalla seuraavat vaiheet ja valitse sitten **Seuraava**:

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-securedeliver-tutorial/tutorial_securedeliver_05.png) 

    a. Valitse **Lataa**ja tallenna sitten tiedosto tietokoneesta.

    b. Valitse **Seuraava**.


5. Saat määritetty sovelluksen SSO-tukihenkilöstöltä suojatun toimittaa kautta [iw-sd-support@fujifilm.com](mailto:iw-sd-support@fujifilm.com) ja annettava seuraavat:
    
    • Ladatut varmennetiedosto

    • **Kohteen tunnus**

    • **Sign-palvelun URL-osoite**

    • **Yksittäisen Kirjaudu ulos palvelun URL-osoite**



6. Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Seuraava**.

    ![Azure AD-Single Sign-On][10]

7. Valitse **Sign Vahvista** -sivulla **Valmis**.  
  
    ![Azure AD-Single Sign-On][11]




### <a name="creating-an-azure-ad-test-user"></a>Azure AD-testikäyttäjän luominen
Tässä osassa tavoitteena on luotava testikäyttäjän kutsutaan Britta Simon Azure perinteinen portaalissa.

![Luo Azure AD-käyttäjä][20]

**Voit luoda suojatun toimittaa testikäyttäjän Azure AD-toimimalla seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen portal**, valitse vasemmassa siirtymisruudussa.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-securedeliver-tutorial/create_aaduser_09.png) 

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Ylä-valikossa käyttäjäluettelon, näkyviin napsauttamalla **käyttäjät**.
  
    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-securedeliver-tutorial/create_aaduser_03.png) 

4. Avaa **Lisää käyttäjä** -valintaikkunan alareunassa työkalurivillä, valitse **Lisää käyttäjä**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-securedeliver-tutorial/create_aaduser_04.png) 

5. Tee valintaikkunassa **Kerro kyseisen käyttäjän tietoja** -sivulla seuraavasti:

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-securedeliver-tutorial/create_aaduser_05.png) 

    a. Käyttäjänä tyyppi Valitse uusi käyttäjä organisaatiossa.

    b. Kirjoita käyttäjänimi- **tekstiruutuun** **BrittaSimon**.

    c-näppäinyhdistelmää. Valitse **Seuraava**.

6.  Suorita **Käyttäjäprofiili** -sivulla seuraavat toimet:

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-securedeliver-tutorial/create_aaduser_06.png) 

    a. Kirjoita **Ensimmäinen nimi** -tekstiruutuun **Britta**.  

    b. Kirjoita **Sukunimi** -tekstiruutuun tyyppi- **Simon**.

    c-näppäinyhdistelmää. Kirjoita **Näyttönimi** -tekstiruutuun **Britta Simon**.

    d. Valitse **rooli** -luettelosta **käyttäjä**.

    e. Valitse **Seuraava**.

7. Valitse **Hae tilapäinen salasana** -valintaikkunan sivulla **Luo**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-securedeliver-tutorial/create_aaduser_07.png) 

8. **Hae tilapäinen salasana** -valintaikkunan sivulla toimimalla seuraavasti:

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-securedeliver-tutorial/create_aaduser_08.png) 

    a. Kirjoita **Uusi salasana**arvo muistiin.

    b. Valitse **Valmis**.   



### <a name="creating-a-secure-deliver-test-user"></a>SUOJATTU toimittaa testikäyttäjän luominen

Tässä osassa tavoitteena on eli Britta Simon suojatun toimittaa käyttäjän luominen. Toimi tukihenkilöstöösi suojatun aikana voit lisätä käyttäjiä suojatun TOIMITA-tilille.

> [AZURE.NOTE] Jos haluat luoda luodun käyttäjän manuaalisesti, on suojattu TOIMITA-tukeen.


### <a name="assigning-the-azure-ad-test-user"></a>Azure AD-testikäyttäjän määrittäminen

Tässä osassa tavoitteena on ottaminen käyttöön Britta Simon käyttämään Azure kertakirjautumisen hänen käyttöoikeuksien myöntäminen suojatun aikana.

![Määrittää käyttäjälle][200] 

**Määritä Britta Simon suojatun aikana, toimi seuraavasti:**

1. Azure perinteinen-portaalissa Avaa sovellukset-näkymässä kansio-näkymässä, valitse **sovellukset** yläreunan-valikossa.

    ![Määrittää käyttäjälle][201] 

2. Valitse sovellukset-luettelosta **Suojattu aikana**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-securedeliver-tutorial/tutorial_securedeliver_50.png) 

1. Valitse ylä-valikossa Valitse **käyttäjiä**.

    ![Määrittää käyttäjälle][203] 

1. Valitse käyttäjät-luettelosta **Britta Simon**.

2. Napsauta työkalurivin alaosassa **määrittäminen**.

    ![Määrittää käyttäjälle][205]



### <a name="testing-single-sign-on"></a>Kertakirjautumisen testaaminen

Tässä osassa tavoitteena on Testaa Azure AD yksittäisen Sign-asetusten käyttäminen Access-paneeli.  
Napsautettaessa suojatun toimittaa ruudun Access-ruudussa voit pitäisi saada automaattisesti kirjautunut-on suojattu toimittaa sovelluksen.


## <a name="additional-resources"></a>Lisäresursseja

* [Luettelo siitä, miten voit integroida SaaS sovellusten Azure Active Directory-opetusohjelmat](active-directory-saas-tutorial-list.md)
* [Mikä on sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-securedeliver-tutorial/tutorial_general_205.png
