<properties
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi henkilöitä | Microsoft Azure"
    description="Opettele määrittämään kertakirjautumisen Azure Active Directory- ja henkilöt välillä."
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
    ms.date="09/29/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-people"></a>Opetusohjelma: Henkilöt Azure Active Directory-integrointi

Tässä opetusohjelmassa tavoitteena on, kuinka voit henkilöiden integroida Azure Active Directory (Azure AD).

Azure AD-integraation henkilöt, jonka avulla seuraavat edut:

- Voit hallita Azure AD, kenellä on oikeus henkilöt
- Voit ottaa käyttöön automaattisesti Hae kirjautunut sisään henkilöille (kertakirjautumisen) niiden Azure AD-tilien käyttäjille
- Voit hallita asiakkaiden yhdessä keskitetyssä sijainnissa - Azure perinteinen portal

Jos haluat tietää tarkempia tietoja SaaS sovelluksen integrointi Azure AD-kohdassa [mitä sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Edellytykset

Azure AD-integroinnin määrittäminen henkilöiden kanssa, tarvitset seuraavat:

- Azure tilauksen
- Henkilöiden single-merkki käytössä-tilauksessa


> [AZURE.NOTE] Voit esikatsella vaiheet Tässä opetusohjelmassa, Emme suosittele käyttämällä tuotantoympäristössä.


Voit esikatsella vaiheet Tässä opetusohjelmassa-noudattamalla näitä suosituksia:

- Älä käytä tuotantoympäristössä, ellei se on tarpeen.
- Jos sinulla ei ole kokeiluversion Azure AD-ympäristössä, voit hankkia yksi kuukausi kokeiluversion [tähän](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Skenaarion kuvaus
Tässä opetusohjelmassa tavoitteena on, jotta voit testata Azure AD kertakirjautumisen testiympäristössä. Tässä opetusohjelmassa kuvatut skenaarion kuuluu kaksi tärkeimmät rakenneosien:

1. Henkilöiden lisääminen valikoimasta
2. Yksittäinen määrittäminen ja testaus Azure AD-on


## <a name="adding-people-from-the-gallery"></a>Henkilöiden lisääminen valikoimasta
Voit määrittää henkilöt integroida Azure AD-haluat lisätä henkilöitä valikoimasta hallitun SaaS sovellusluettelo.

**Voit lisätä henkilöitä valikoimasta, toimi seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen portal**, valitse vasemmassa siirtymisruudussa. 
 
    ![Active Directory][1]

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset][2]

4. Valitse sivun alareunassa **Lisää** .

    ![Sovellukset][3]

5. Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Sovellukset][4]

6. Kirjoita hakuruutuun **henkilöt**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-people-tutorial/tutorial_people_01.png)

7. Valitse tulokset-ruudun Valitse **henkilöt**ja valitse sitten **Valmis** , voit lisätä sovelluksen.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-people-tutorial/tutorial_people_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yksittäinen määrittäminen ja testaus Azure AD-on
Tässä osassa tavoitteena on noudattamalla voit määrittää ja testaa Azure AD kertakirjautumisen nimeltä "Britta Simon" testi käyttäjään perustuva henkilöiden kanssa.

Määrittäminen ja testaaminen Azure AD kertakirjautumisen henkilöiden kanssa, sinun on tehtävä seuraavat hakukyselyn:

1. **[Azure AD määrittäminen Single Sign](#configuring-azure-ad-single-single-sign-on)** - käyttäjät voivat käyttää tätä toimintoa.
2. **[Käyttäjän testata luominen Azure AD](#creating-an-azure-ad-test-user)** - Testaa Azure AD kertakirjautumisen Britta Simon kanssa.
3. **[Henkilöiden lisääminen luominen testata käyttäjän](#creating-a-people-test-user)** - tapahtumista Britta Simon ovat henkilöitä, jotka on linkitetty hän Azure AD esitys.
4. **[Käyttäjän testata määrittäminen Azure AD](#assigning-the-azure-ad-test-user)** - Britta Simon käyttämään Azure AD kertakirjautumisen käyttöön.
5. **[Testaus Single Sign](#testing-single-sign-on)** - voit tarkistaa, toimiiko määritykset.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD-Single Sign-On määrittäminen

Tässä osassa tavoitteena on Azure AD kertakirjautumisen Azure perinteinen portaalissa ottaminen käyttöön ja määrittää kertakirjautumisen henkilöt-sovelluksessa.



**Jos haluat määrittää Azure AD kertakirjautumisen henkilöiden kanssa, toimimalla seuraavasti:**

1. Valitse Azure perinteinen portaalissa **henkilöt** sovelluksen integrointi-sivun **Määritä kertakirjautumisen** **Määrittäminen Single Sign** -valintaikkunan avaaminen.

    [Kertakirjautumisen määrittäminen][6] 

2. **Miten haluat muiden käyttäjien kirjautua** sivulla Valitse **Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.
 
    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-people-tutorial/tutorial_people_03.png) 

3. Valitse **Määritä sovelluksen asetukset** -sivulla suorittamalla seuraavat vaiheet ja valitse sitten **Seuraava**:
 
    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-people-tutorial/tutorial_people_04.png) 

    a. Kirjoita **Merkki-URL** -tekstiruutuun URL-osoite käyttämä Sign käyttämällä seuraavaa kaavaa henkilöiden sovelluksen käyttäjille: **"https://\<yrityksen nimi\>.peoplehr.com/"**. 

    b. Jos et tiedä vuokraajan URL-osoite, ota yhteyttä henkilöiden tukiryhmän kautta [customerservices@peoplehr.com](mailto:customerservices@peoplehr.com) hankkiminen.  

    c-näppäinyhdistelmää. Kirjoita **tunnus** -tekstiruutuun vuokraajan URL-osoite. 

    d. Kirjoita **Vastaus URL** -tekstiruutuun URL-Osoitetta seuraavaa kaavaa: "**https://itgs.peoplehr.net/Pages/Saml/ConsumeAzureAD.aspx**".

    e. Valitse **Seuraava**


4. Valitse **Määritä kertakirjautumisen milloin henkilöt** -sivulla suorittamalla seuraavat vaiheet ja valitse sitten **Seuraava**:

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-people-tutorial/tutorial_people_05.png) 

    a. Valitse **Lataa metatiedot**ja tallenna sitten tiedosto tietokoneesta.

    b. Valitse **Seuraava**.


5. Saat määritetty sovelluksen SSO-haluat Sign henkilöiden asiakasympäristöön järjestelmänvalvojana.
    
    a. Valitse vasemmassa reunassa-valikossa **asetukset**.
    
    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-people-tutorial/tutorial_people_001.png) 

    b. Valitse **"Yrityksen"**.
    
    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-people-tutorial/tutorial_people_002.png) 

    c-näppäinyhdistelmää. - **"Lataa" kertakirjauksen-' SAML metatiedon tiedosto "**, **Selaa** ladatut metatiedot-tiedoston lataaminen.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-people-tutorial/tutorial_people_003.png)




6. Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Seuraava**.
 
    ![Azure AD-Single Sign-On][10]

7. Valitse **Sign Vahvista** -sivulla **Valmis**.  

    ![Azure AD-Single Sign-On][11]




### <a name="creating-an-azure-ad-test-user"></a>Azure AD-testikäyttäjän luominen
Tässä osassa tavoitteena on luotava testikäyttäjän kutsutaan Britta Simon Azure perinteinen portaalissa.
Valitse käyttäjät-luettelosta **Britta Simon**.

![Luo Azure AD-käyttäjä][20]


**Voit luoda henkilöiden testikäyttäjän Azure AD-toimimalla seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen portal**, valitse vasemmassa siirtymisruudussa.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-people-tutorial/create_aaduser_09.png)

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Ylä-valikossa käyttäjäluettelon, näkyviin napsauttamalla **käyttäjät**.
 
    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-people-tutorial/create_aaduser_03.png) 

4. Avaa **Lisää käyttäjä** -valintaikkunan alareunassa työkalurivillä, valitse **Lisää käyttäjä**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-people-tutorial/create_aaduser_04.png) 

5. Tee valintaikkunassa **Kerro kyseisen käyttäjän tietoja** -sivulla seuraavasti:
 
    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-people-tutorial/create_aaduser_05.png) 

    a. Käyttäjänä tyyppi Valitse uusi käyttäjä organisaatiossa.

    b. Kirjoita käyttäjänimi- **tekstiruutuun** **BrittaSimon**.

    c-näppäinyhdistelmää. Valitse **Seuraava**.

6.  Suorita **Käyttäjäprofiili** -sivulla seuraavat toimet:

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-people-tutorial/create_aaduser_06.png) 

    a. Kirjoita **Ensimmäinen nimi** -tekstiruutuun **Britta**.  

    b. Kirjoita **Sukunimi** -tekstiruutuun tyyppi- **Simon**.

    c-näppäinyhdistelmää. Kirjoita **Näyttönimi** -tekstiruutuun **Britta Simon**.

    d. Valitse **rooli** -luettelosta **käyttäjä**.

    e. Valitse **Seuraava**.

7. Valitse **Hae tilapäinen salasana** -valintaikkunan sivulla **Luo**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-people-tutorial/create_aaduser_07.png) 

8. **Hae tilapäinen salasana** -valintaikkunan sivulla toimimalla seuraavasti:

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-people-tutorial/create_aaduser_08.png) 

    a. Kirjoita **Uusi salasana**arvo muistiin.

    b. Valitse **Valmis**.   



### <a name="creating-a-people-test-user"></a>Henkilöiden testikäyttäjän luominen

Tässä osassa tavoitteena on nimeltään Britta Simon henkilöt käyttäjän luominen. Käyttäjät eivät tue juuri perustuvan valmistelu niin on Ota henkilöt-tukeen luominen luodun käyttäjän manuaalisesti.




### <a name="assigning-the-azure-ad-test-user"></a>Azure AD-testikäyttäjän määrittäminen

Tässä osassa tavoitteena on ottaminen käyttöön Britta Simon käyttämään Azure kertakirjautumisen hänen käyttöoikeuksien myöntäminen käyttäjille.

![Määrittää käyttäjälle][200] 

**Jos haluat määrittää Britta Simon henkilöt, toimi seuraavasti:**

1. Azure perinteinen-portaalissa Avaa sovellukset-näkymässä kansio-näkymässä, valitse **sovellukset** yläreunan-valikossa.

    ![Määrittää käyttäjälle][201] 

2. Valitse **henkilöt**sovellusten luettelossa.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-people-tutorial/tutorial_people_50.png) 

1. Valitse ylä-valikossa Valitse **käyttäjiä**.

    ![Määrittää käyttäjälle][203] 

1. Valitse käyttäjät-luettelosta **Britta Simon**.

2. Napsauta työkalurivin alaosassa **määrittäminen**.

    ![Määrittää käyttäjälle][205]



### <a name="testing-single-sign-on"></a>Kertakirjautumisen testaaminen

Tässä osassa tavoitteena on Testaa Azure AD yksittäisen Sign-asetusten käyttäminen Access-paneeli.
Kun napsautat Access-ruudussa henkilöt-ruutu, voit olisi Hae automaattisesti kirjautunut-on henkilöt-sovellukseen.


## <a name="additional-resources"></a>Lisäresursseja

* [Luettelo siitä, miten voit integroida SaaS sovellusten Azure Active Directory-opetusohjelmat](active-directory-saas-tutorial-list.md)
* [Mikä on sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-people-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-people-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-people-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-people-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-people-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-people-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-people-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-people-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-people-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-people-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-people-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-people-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-people-tutorial/tutorial_general_205.png
