<properties
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi Alcumus tiedot Exchange | Microsoft Azure"
    description="Opettele määrittämään kertakirjautumisen Azure Active Directory- ja Alcumus tiedot Exchange välillä."
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


# <a name="tutorial-azure-active-directory-integration-with-alcumus-info-exchange"></a>Opetusohjelma: Alcumus tiedot Exchange Azure Active Directory-integrointi

Tässä opetusohjelmassa tavoitteena on, kuinka voit integroida Alcumus tiedot Exchange Azure Active Directory (Azure AD).  
Alcumus tiedot Exchange-integraation Azure AD, jonka avulla seuraavat edut: 

- Voit hallita Azure AD, jolla on pääsy Alcumus tiedot Exchange 
- Voit ottaa käyttöön automaattisesti Hae kirjautunut sisään Alcumus tiedot Exchange (kertakirjautumisen) käyttäjille heidän Azure AD-tilien kanssa
- Voit hallita asiakkaiden yhdessä keskitetyssä sijainnissa - Azure perinteinen portal

Jos haluat tietää tarkempia tietoja SaaS sovelluksen integrointi Azure AD-kohdassa [mitä sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Edellytykset 

Määrittää Azure AD-integroinnin Alcumus tiedot Exchange, tarvitset seuraavat:

- [Azure AD](https://azure.microsoft.com/) -tilaus
- On [Alcumus tiedot Exchange](http://www.alcumusgroup.com/) single-etumerkki käytössä-tilauksessa


> [AZURE.NOTE] Voit esikatsella vaiheet Tässä opetusohjelmassa, Emme suosittele käyttämällä tuotantoympäristössä.


Voit esikatsella vaiheet Tässä opetusohjelmassa-noudattamalla näitä suosituksia:

- Älä käytä tuotantoympäristössä, ellei se on tarpeen.
- Jos sinulla ei ole kokeiluversion Azure AD-ympäristössä, voit hankkia yksi kuukausi kokeiluversion [tähän](https://azure.microsoft.com/pricing/free-trial/). 

 
## <a name="scenario-description"></a>Skenaarion kuvaus
Tässä opetusohjelmassa tavoitteena on, jotta voit testata Azure AD kertakirjautumisen testiympäristössä.  
Tässä opetusohjelmassa kuvatut skenaarion koostuu kolme tärkeimmät rakenneosien:

1. Alcumus tiedot Exchange lisääminen valikoimasta 
2. Yksittäinen määrittäminen ja testaus Azure AD-on


## <a name="adding-alcumus-info-exchange-from-the-gallery"></a>Alcumus tiedot Exchange lisääminen valikoimasta
Voit määrittää Alcumus tiedot Exchange integroida Azure AD-haluat lisätä Alcumus tiedot Exchange valikoimasta hallitun SaaS sovellusluettelo.

**Lisää Alcumus tiedot Exchange-valikoimasta, toimi seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen portal**, valitse vasemmassa siirtymisruudussa. 

    ![Active Directory][1]

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset][2]

4. Valitse sivun alareunassa **Lisää** .

    ![Sovellukset][3]

5. Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Sovellukset][4]

6. Kirjoita hakuruutuun **Alcumus tiedot Exchange**.

    ![Sovellukset][5]

7. Valitse tulosruudussa **Alcumus tiedot Exchange**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![Sovellukset][400]



##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yksittäinen määrittäminen ja testaus Azure AD-on
Tässä osassa tavoitteena on noudattamalla voit määrittää ja testaa Azure AD kertakirjautumisen Alcumus tiedot Exchange nimeltä "Britta Simon" testikäyttäjän perusteella.

Kertakirjautumisen toimimaan Azure AD on tiedettävä vastaavaan käyttäjän Alcumus tiedot Exchange Azure AD-käyttäjälle. Toisin sanoen linkki suhteen Azure AD-käyttäjä ja Aiheeseen liittyvät Alcumus tiedot Exchange on vahvistettava.  
Linkki-yhteys on muodostettu määrittämällä **käyttäjänimi** arvo arvoksi **käyttäjänimi** Alcumus tiedot Exchangen Azure AD.
 
Määrittäminen ja testaaminen Azure AD kertakirjautumisen Alcumus tiedot Exchange-sinun on tehtävä seuraavat hakukyselyn:

1. **[Azure AD määrittäminen Single Sign](#configuring-azure-ad-single-single-sign-on)** - käyttäjät voivat käyttää tätä toimintoa.
2. **[Käyttäjän testata luominen Azure AD](#creating-an-azure-ad-test-user)** - Testaa Azure AD kertakirjautumisen Britta Simon kanssa.
4. **[Käyttäjän luominen Alcumus tiedot-Exchange testata](#creating-a-alcumus-info-exchange-test-user)** - on tapahtumista Britta Simon Alcumus tiedot Exchange kyseistä henkilöä hän Azure AD esitys.
5. **[Käyttäjän testata määrittäminen Azure AD](#assigning-the-azure-ad-test-user)** - Britta Simon käyttämään Azure AD kertakirjautumisen käyttöön.
5. **[Testaus Single Sign](#testing-single-sign-on)** - voit tarkistaa, toimiiko määritykset.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD-Single Sign-On määrittäminen

Tässä osassa tavoitteena on Azure AD kertakirjautumisen Azure perinteinen portaalissa ottaminen käyttöön ja määrittää kertakirjautumisen Alcumus tiedot Exchange-sovelluksessa.

**Voit määrittää Azure AD kertakirjautumisen Alcumus tiedot Exchange toimimalla seuraavasti:**

1. Valitse Azure perinteinen portaalissa integrointi **Alcumus tiedot Exchange** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen Single Sign** -valintaikkunan avaaminen.

    ![Kertakirjautumisen määrittäminen][6]

2. Valitse **kuinka haluat käyttäjien kirjautua Alcumus tiedot Exchange** -sivulla Valitse **Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Azure AD-Single Sign-On][7]

3. Valitse **Määritä sovelluksen asetukset** -sivulla toimimalla seuraavasti: 

    ![Azure AD-Single Sign-On][8]
 
    a. Kirjoita **Vastaus URL** -tekstiruutuun kuluttaja URL-osoite, joka oli puolestasi asetusten mukaan Alcumus tiedot Exchange tukihenkilöstölle.

    > [AZURE.NOTE] Jos et tiedä, mikä oikea arvo on, ota yhteyttä Alcumus tiedot Exchange-tukeen kautta [helpdesk@alcumusgroup.com](mailto:helpdesk@alcumusgroup.com).

    b. Valitse **Seuraava**.
 
4. Valitse **Määritä kertakirjautumisen osoitteessa Alcumus tiedot Exchange** -sivulla valitsemalla **Lataa metatiedot**ja Tallenna paikallisesti tietokoneen metatiedot-tiedostoon.

    ![Mikä on Azure AD Connect][9]

5. Alcumus tiedot Exchange-tukeen kautta [helpdesk@alcumusgroup.com](mailto:helpdesk@alcumusgroup.com)antaa niille metatieto-tiedosto ja niiden ilmoita siitä, että ne Ota SSO puolestasi.


6. Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Seuraava**. 

    ![Mikä on Azure AD Connect][10]

7. Valitse **Sign Vahvista** -sivulla **Valmis**.  

    ![Mikä on Azure AD Connect][11]




### <a name="creating-an-azure-ad-test-user"></a>Azure AD-testikäyttäjän luominen
Tässä osassa tavoitteena on luotava testikäyttäjän kutsutaan Britta Simon Azure perinteinen portaalissa.  

![Luo Azure AD-käyttäjä][20]

**Voit luoda testikäyttäjän Azure AD-toimimalla seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen portal**, valitse vasemmassa siirtymisruudussa.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_02.png) 

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Ylä-valikossa käyttäjäluettelon, näkyviin napsauttamalla **käyttäjät**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_03.png) 
 
4. Avaa **Lisää käyttäjä** -valintaikkunan alareunassa työkalurivillä, valitse **Lisää käyttäjä**. 

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_04.png) 

5. Tee valintaikkunassa **Kerro kyseisen käyttäjän tietoja** -sivulla seuraavasti: 

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_05.png) 

    a. Käyttäjänä tyyppi Valitse uusi käyttäjä organisaatiossa.
  
    b. Kirjoita käyttäjänimi- **tekstiruutuun** **BrittaSimon**.
  
    c-näppäinyhdistelmää. Valitse Seuraava.



6.  Suorita **Käyttäjäprofiili** -sivulla seuraavat toimet: 

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_06.png) 
  

    a. Kirjoita **Ensimmäinen nimi** -tekstiruutuun **Britta**.  
  
    b. Valitse **Sukunimi** txtbox, tyyppi- **Simon**.
  
    c-näppäinyhdistelmää. Kirjoita **Näyttönimi** -tekstiruutuun **Britta Simon**.
  
    d. Valitse **rooli** -luettelosta **käyttäjä**.
  
    e. Valitse **Seuraava**.


7. Valitse **Hae tilapäinen salasana** -valintaikkunan sivulla **Luo**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_07.png) 
 

8. **Hae tilapäinen salasana** -valintaikkunan sivulla toimimalla seuraavasti:

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-alcumus-info-tutorial/create_aaduser_08.png) 

    a. Kirjoita **Uusi salasana**arvo muistiin.
  
    b. Valitse **Valmis**.   

  
 
### <a name="creating-a-alcumus-info-exchange-test-user"></a>Alcumus tiedot Exchange-testikäyttäjän luominen

Tässä osassa tavoitteena on nimeltään Britta Simon Alcumus tiedot Exchange-käyttäjän luominen.

**Luo kutsutaan Britta Simon Alcumus tiedot Exchange-käyttäjä, toimi seuraavasti:**

1. Alcumus tiedot Exchange-tukeen kautta [helpdesk@alcumusgroup.com](mailto:helpdesk@alcumusgroup.com),


### <a name="assigning-the-azure-ad-test-user"></a>Azure AD-testikäyttäjän määrittäminen

Tässä osassa tavoitteena on Britta Simon käyttämään Azure kertakirjautumisen hänen käyttöoikeuden myöntämisestä Alcumus tiedot Exchange ottaminen käyttöön.

![Määrittää käyttäjälle][200]

**Jos haluat määrittää Britta Simon Alcumus tiedot Exchange, toimi seuraavasti:**

1. Azure-portaalissa Avaa sovellukset-näkymässä kansio-näkymässä, valitse **sovellukset** yläreunan-valikossa.

    ![Määrittää käyttäjälle][201]

2. Valitse sovellukset-luettelosta **Alcumus tiedot Exchange**.

    ![Määrittää käyttäjälle][202]

1. Valitse ylä-valikossa Valitse **käyttäjiä**.

    ![Määrittää käyttäjälle][203]

1. Valitse käyttäjät-luettelosta **Britta Simon**.

2. Napsauta työkalurivin alaosassa **määrittäminen**.

    ![Määrittää käyttäjälle][205]



### <a name="testing-single-sign-on"></a>Kertakirjautumisen testaaminen

Tässä osassa tavoitteena on Testaa Azure AD yksittäisen Sign-asetusten käyttäminen Access-paneeli.  
Kun napsautat Access-ruudussa Alcumus tiedot Exchange-ruutu, sinun olisi Hae automaattisesti kirjautunut-on Alcumus tiedot Exchange-sovellukseen.


## <a name="additional-resources"></a>Lisäresursseja

* [Luettelo siitä, miten voit integroida SaaS sovellusten Azure Active Directory-opetusohjelmat](active-directory-saas-tutorial-list.md)
* [Mikä on sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta?](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_01.png
[6]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_02.png
[7]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_03.png
[8]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_04.png
[9]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_05.png
[10]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_06.png
[11]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_07.png
[20]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_08.png
[203]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_general_205.png
[400]: ./media/active-directory-saas-alcumus-info-tutorial/tutorial_alcumus_402.png