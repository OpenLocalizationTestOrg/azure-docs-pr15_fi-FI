<properties
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi Tableau online | Microsoft Azure"
    description="Opettele määrittämään kertakirjautumisen Azure Active Directory- ja Tableau Online välillä."
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
    ms.date="10/18/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-tableau-online"></a>Opetusohjelma: Azure Active Directory-integrointi Tableau online

Tässä opetusohjelmassa kerrotaan, miten voit integroida Tableau Online Azure Active Directory (Azure AD).

Azure AD Tableau Online integraation avulla voit seuraavat edut:

- Voit hallita Azure AD, jolla on pääsy Tableau Online
- Voit ottaa käyttöön automaattisesti Hae kirjautunut sisään Tableau online (kertakirjautumisen) käyttäjille heidän Azure AD-tilien kanssa
- Voit hallita asiakkaiden yhdessä keskitetyssä sijainnissa - Azure perinteinen portal

Jos haluat tietää tarkempia tietoja SaaS sovelluksen integrointi Azure AD-kohdassa [mitä sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Edellytykset

Määrittää Azure AD-integroinnin Tableau Online, tarvitset seuraavat:

- Azure AD-tilaus
- On **Tableau Online** single-allekirjoitus käytössä-tilauksessa


> [AZURE.NOTE] Voit esikatsella vaiheet Tässä opetusohjelmassa, Emme suosittele käyttämällä tuotantoympäristössä.


Voit esikatsella vaiheet Tässä opetusohjelmassa-noudattamalla näitä suosituksia:

- Älä käytä tuotantoympäristössä, ellei se on tarpeen.
- Jos sinulla ei ole kokeiluversion Azure AD-ympäristössä, voit hankkia yksi kuukausi kokeiluversion [tähän](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Skenaarion kuvaus
Tässä opetusohjelmassa testaat Azure AD kertakirjautumisen testiympäristössä. Tässä opetusohjelmassa kuvatut skenaarion kuuluu kaksi tärkeimmät rakenneosien:

1. Lisäämällä Tableau Online-valikoimasta
2. Yksittäinen määrittäminen ja testaus Azure AD-on


## <a name="adding-tableau-online-from-the-gallery"></a>Lisäämällä Tableau Online-valikoimasta
Voit määrittää Tableau online integroida Azure AD-sinun on lisättävä Tableau Online valikoimasta hallitun SaaS sovellusluettelo.

**Jos haluat lisätä Tableau Online-valikoimasta, toimimalla seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen portal**, valitse vasemmassa siirtymisruudussa. 

    ![Active Directory][1]

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset][2]

4. Valitse sivun alareunassa **Lisää** .

    ![Sovellukset][3]

5. Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Sovellukset][4]

6. Kirjoita hakuruutuun **Tableau online-tilassa**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_01.png)

7. Valitse tulosruudussa **Tableau Online**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yksittäinen määrittäminen ja testaus Azure AD-on
Tässä osassa Määritä ja testaa Azure AD kertakirjautumisen Tableau online nimeltä "Britta Simon" testikäyttäjän perusteella.

Kertakirjautumisen toimimaan Azure AD on tiedettävä käyttäjälle on Tableau Online vastaavaan käyttäjän Azure AD. Toisin sanoen linkki suhteen Azure AD-käyttäjä ja Aiheeseen liittyvät Tableau Onlinessa on vahvistettava.
Linkki-yhteys on muodostettu määrittämällä **käyttäjänimi** arvo arvoksi **käyttäjänimi** Tableau Onlinessa Azure AD.

Määrittäminen ja testaaminen Azure AD kertakirjautumisen Tableau online-sinun on tehtävä seuraavat hakukyselyn:

1. **[Azure AD määrittäminen Single Sign](#configuring-azure-ad-single-single-sign-on)** - käyttäjät voivat käyttää tätä toimintoa.
2. **[Käyttäjän testata luominen Azure AD](#creating-an-azure-ad-test-user)** - Testaa Azure AD kertakirjautumisen Britta Simon kanssa.
4. **[Käyttäjän testata luominen Tableau-Online](#creating-a-Tableau-Online-test-user)** - tapahtumista Britta Simon ovat Tableau Online, jotka on linkitetty hän Azure AD esitys.
5. **[Käyttäjän testata määrittäminen Azure AD](#assigning-the-azure-ad-test-user)** - Britta Simon käyttämään Azure AD kertakirjautumisen käyttöön.
5. **[Testaus Single Sign](#testing-single-sign-on)** - voit tarkistaa, toimiiko määritykset.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD kertakirjautumisen määrittäminen

Tässä osassa tavoitteena on Azure AD kertakirjautumisen Azure perinteinen portaalissa ottaminen käyttöön ja määrittää kertakirjautumisen Tableau Online-sovelluksessa.

**Jos haluat määrittää Azure AD kertakirjautumisen Tableau online, toimimalla seuraavasti:**

1. Valitse ylä-valikossa **Pika-aloitus**.

    ![Kertakirjautumisen määrittäminen][6]
2. Valitse perinteinen-portaalissa **Tableau Online** integrointi sivulla **määrittäminen kertakirjautumisen** **Määrittäminen Single Sign** -valintaikkunan avaaminen.

    ![Kertakirjautumisen määrittäminen][7] 

3. **Miten haluat kirjautua sisään Tableau Online käyttäjät** -sivulla Valitse **Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.
    
    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_06.png)

4. Valitse **Määritä sovelluksen asetukset** -sivulla toimimalla seuraavasti: 

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_07.png)


    a. Kirjoita merkki-URL-tekstiruutuun URL-osoite, käyttämällä seuraavaa kaavaa:`https://sso.online.tableau.com`

    c-näppäinyhdistelmää. Valitse **Seuraava**.

5. Valitse **Määritä kertakirjautumisen osoitteessa Tableau Online** -sivulla **Lataa metatiedot**- ja tallenna se tietokoneeseen.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_08.png)

6. Valitse Sign määritysten vahvistus ja valitse sitten **Seuraava**.
    
    ![Azure AD-Single Sign-On][10]

7. Valitse **Sign Vahvista** -sivulla **Valmis**.  
    
    ![Azure AD-Single Sign-On][11]
8. Eri selainikkunassa Sign Tableau Online-sovellukseen. Valitse **asetukset** ja sitten **todennus**

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_09.png)

9. **Todennustyypit** -osassa. Tarkista SAML käyttöön **kertakirjautumisen SAML kanssa** -valintaruutu.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_12.png)

10. Vierittää alaspäin, kunnes **Tuo metatietojen tiedosto Tableau Online** -osassa.  Valitse Selaa ja olet ladannut Azure AD-metatiedot-tiedoston tuominen. Valitse **Käytä**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_13.png)

11. **Vastine vahvistukset** -osassa Lisää vastaavan tunnistetietojen toimittaja vahvistus nimen sähköpostiosoite, Etunimi ja sukunimi. Käyttämistä Azure AD nämä tiedot:

    a. Siirry Azure AD. Azure-perinteinen portaalissa-sivulla **Tableau Online** sovelluksen integrointi valikon päälle Valitse **määritteet**. Kopioi arvot nimi: userprincipalname-givenname ja sukunimi.
     
    ![Azure AD-Single Sign-On](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_10.png)

    b. Siirry Tableau Online-sovellukseen ja sitten määrittäminen seuraa **Tableau Online määritteet** -osassa:
    
    -  Sähköposti: **Sähköposti** - tai **userprincipalname**
    -  Etunimi: **givenname**
    -  Sukunimi: **Sukunimi**

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_14.png)

### <a name="creating-an-azure-ad-test-user"></a>Azure AD-testikäyttäjän luominen
Tässä osassa voit luoda testikäyttäjän kutsutaan Britta Simon perinteinen portaalissa.

![Luo Azure AD-käyttäjä][20]

**Voit luoda testikäyttäjän Azure AD-toimimalla seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen portal**, valitse vasemmassa siirtymisruudussa.
    
    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_09.png) 

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Ylä-valikossa käyttäjäluettelon, näkyviin napsauttamalla **käyttäjät**.
    
    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_03.png) 

4. Avaa **Lisää käyttäjä** -valintaikkunan alareunassa työkalurivillä, valitse **Lisää käyttäjä**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_04.png) 

5. Tee valintaikkunassa **Kerro kyseisen käyttäjän tietoja** -sivulla seuraavasti:
 
    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_05.png) 

    a. Käyttäjänä tyyppi Valitse uusi käyttäjä organisaatiossa.

    b. Kirjoita käyttäjänimi- **tekstiruutuun** **BrittaSimon**.

    c-näppäinyhdistelmää. Valitse **Seuraava**.

6.  Suorita **Käyttäjäprofiili** -sivulla seuraavat toimet:

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_06.png) 

    a. Kirjoita **Ensimmäinen nimi** -tekstiruutuun **Britta**.  

    b. Kirjoita **Sukunimi** -tekstiruutuun tyyppi- **Simon**.

    c-näppäinyhdistelmää. Kirjoita **Näyttönimi** -tekstiruutuun **Britta Simon**.

    d. Valitse **rooli** -luettelosta **käyttäjä**.

    e. Valitse **Seuraava**.

7. Valitse **Hae tilapäinen salasana** -valintaikkunan sivulla **Luo**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_07.png) 

8. **Hae tilapäinen salasana** -valintaikkunan sivulla toimimalla seuraavasti:

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_08.png) 

    a. Kirjoita **Uusi salasana**arvo muistiin.

    b. Valitse **Valmis**.   



### <a name="creating-a-tableau-online-test-user"></a>Tableau Online testikäyttäjän luominen

Tässä osassa nimeltä Britta Simon Tableau Online käyttäjän luominen.

1. **Tableau Online**Valitse **asetukset** ja sitten **todentaminen** ‑osassa. Vieritä **Valitse käyttäjät** -osiossa. Valitse **Lisää käyttäjiä** ja **sähköpostiosoitteet**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_15.png)
2. Valitse **Lisää käyttäjiä kertakirjautumisportaaliin (SSO) todennusta varten**. **Kirjoita sähköpostiosoitteet** tekstiruudun lisääminenbritta.simon@contoso.com

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_11.png)

3.  Valitse **Luo**.


### <a name="assigning-the-azure-ad-test-user"></a>Azure AD-testikäyttäjän määrittäminen

Tässä osassa voit ottaa käyttöön Britta Simon käyttämään Azure kertakirjautumisen hänen käyttöoikeuden myöntämisestä Tableau online-tilassa.

![Määrittää käyttäjälle][200] 

**Jos haluat määrittää Britta Simon Tableau Online, toimi seuraavasti:**

1. Perinteinen-portaalissa Avaa sovellukset-näkymässä kansio-näkymässä, valitse **sovellukset** yläreunan-valikossa.

    ![Määrittää käyttäjälle][201] 

3. Valitse sovellukset-luettelosta **Tableau online-tilassa**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_50.png) 

4. Valitse ylä-valikossa Valitse **käyttäjiä**.

    ![Määrittää käyttäjälle][203] 

5. Valitse kaikki käyttäjät-luettelosta **Britta Simon**.

6. Napsauta työkalurivin alaosassa **määrittäminen**.

    ![Määrittää käyttäjälle][205]


### <a name="testing-single-sign-on"></a>Kertakirjautumisen testaaminen

Tässä osassa tavoitteena on Testaa Azure AD yksittäisen Sign-asetusten käyttäminen Access-paneeli.

Access-ruudussa Tableau Online-ruutua napsauttaessasi sinun olisi Hae automaattisesti kirjautunut sisään Tableau Online-sovellukseen.

## <a name="additional-resources"></a>Lisäresursseja

* [Luettelo siitä, miten voit integroida SaaS sovellusten Azure Active Directory-opetusohjelmat](active-directory-saas-tutorial-list.md)
* [Mikä on sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_04.png


[5]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_05.png
[6]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_06.png
[7]:  ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_050.png
[10]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_060.png
[11]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_070.png
[20]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_205.png
