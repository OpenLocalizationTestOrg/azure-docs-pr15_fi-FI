<properties
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi Yonyx vuorovaikutteisissa oppaissa | Microsoft Azure"
    description="Opettele määrittämään kertakirjautumisen Azure Active Directory- ja Yonyx vuorovaikutteisissa oppaissa välillä."
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
    ms.date="10/26/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-yonyx-interactive-guides"></a>Opetusohjelma: Yonyx vuorovaikutteisissa oppaissa Azure Active Directory-integrointi

Tässä opetusohjelmassa tavoitteena on, kuinka voit integroida Yonyx vuorovaikutteisissa oppaissa Azure Active Directory (Azure AD).

Azure AD-integraation Yonyx vuorovaikutteisissa oppaissa, jonka avulla seuraavat edut:

- Voit hallita Azure AD, jolla on pääsy Yonyx vuorovaikutteiset oppaat
- Voit ottaa käyttöön automaattisesti Hae kirjautunut käyttöön Yonyx vuorovaikutteisissa oppaissa (kertakirjautumisen) käyttäjille heidän Azure AD-tilien kanssa
- Voit hallita asiakkaiden yhdessä keskitetyssä sijainnissa - Azure perinteinen portal

Jos haluat tietää tarkempia tietoja SaaS sovelluksen integrointi Azure AD-kohdassa [mitä sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Edellytykset

Määrittää Azure AD-integroinnin Yonyx vuorovaikutteisissa oppaissa, tarvitset seuraavat:

- Azure AD-tilaus
- Yonyx vuorovaikutteisissa oppaissa single-merkki käytössä-tilauksessa


> [AZURE.NOTE] Voit esikatsella vaiheet Tässä opetusohjelmassa, Emme suosittele käyttämällä tuotantoympäristössä.


Voit esikatsella vaiheet Tässä opetusohjelmassa-noudattamalla näitä suosituksia:

- Älä käytä tuotantoympäristössä, ellei se on tarpeen.
- Jos sinulla ei ole kokeiluversion Azure AD-ympäristössä, voit hankkia yksi kuukausi kokeiluversion [tähän](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Skenaarion kuvaus
Tässä opetusohjelmassa tavoitteena on, jotta voit testata Azure AD kertakirjautumisen testiympäristössä.

Tässä opetusohjelmassa kuvatut skenaarion kuuluu kaksi tärkeimmät rakenneosien:

1. Yonyx vuorovaikutteisissa oppaissa lisääminen valikoimasta
2. Yksittäinen määrittäminen ja testaus Azure AD-on


## <a name="adding-yonyx-interactive-guides-from-the-gallery"></a>Yonyx vuorovaikutteisissa oppaissa lisääminen valikoimasta
Voit määrittää Yonyx vuorovaikutteisissa oppaissa integroida Azure AD-haluat lisätä Yonyx vuorovaikutteisissa oppaissa valikoimasta hallitun SaaS sovellusluettelo.

**Jos haluat lisätä Yonyx vuorovaikutteisissa oppaissa valikoimasta, toimi seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen Portal**, valitse vasemmassa siirtymisruudussa. 

    ![Active Directory][1]

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .
    
    ![Sovellukset][2]

4. Valitse sivun alareunassa **Lisää** .
    
    ![Sovellukset][3]

5. Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Sovellukset][4]

6. Kirjoita hakuruutuun **Yonyx vuorovaikutteisissa oppaissa**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-yonyx-tutorial/tutorial_yonyx_01.png)

7. Tulokset-ruudussa Valitse **Yonyx vuorovaikutteiset oppaat**ja valitse sitten **Valmis** , voit lisätä sovelluksen.

    ![Sovelluksen valitseminen valikoimasta](./media/active-directory-saas-yonyx-tutorial/tutorial_yonyx_0001.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yksittäinen määrittäminen ja testaus Azure AD-on
Tässä osassa tavoitteena on noudattamalla voit määrittää ja testaa Azure AD kertakirjautumisen Yonyx vuorovaikutteisissa oppaissa nimeltä "Britta Simon" testikäyttäjän perusteella.

Kertakirjautumisen toimimaan Azure AD on tietää, mitkä asiat vastaavaan käyttäjän Yonyx vuorovaikutteisissa oppaissa Azure AD-käyttäjälle. Toisin sanoen suhteen Azure AD-käyttäjä ja Aiheeseen liittyvät Yonyx vuorovaikutteisissa oppaissa-linkki on vahvistettava.

Linkki-yhteys on muodostettu määrittämällä **käyttäjänimi** arvo arvoksi Yonyx vuorovaikutteisissa oppaissa **käyttäjänimi** Azure AD.

Määrittäminen ja testaaminen Azure AD kertakirjautumisen Yonyx vuorovaikutteisissa oppaissa kanssa, sinun on tehtävä seuraavat hakukyselyn:

1. **[Azure AD määrittäminen Single Sign](#configuring-azure-ad-single-single-sign-on)** - käyttäjät voivat käyttää tätä toimintoa.
2. **[Käyttäjän testata luominen Azure AD](#creating-an-azure-ad-test-user)** - Testaa Azure AD kertakirjautumisen Britta Simon kanssa.
3. **[Käyttäjän luominen Yonyx vuorovaikutteisissa oppaissa testata](#creating-a-yonyx-interactive-guides-test-user)** - on tapahtumista Britta Simon Yonyx vuorovaikutteisissa oppaissa kyseistä henkilöä hän Azure AD esitys.
4. **[Käyttäjän testata määrittäminen Azure AD](#assigning-the-azure-ad-test-user)** - Britta Simon käyttämään Azure AD kertakirjautumisen käyttöön.
5. **[Testaus Single Sign](#testing-single-sign-on)** - voit tarkistaa, toimiiko määritykset.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD kertakirjautumisen määrittäminen

Tässä osassa Azure AD kertakirjautumisen perinteinen portaalissa ottaminen käyttöön ja määritä kertakirjautumisen Yonyx vuorovaikutteisissa oppaissa-sovelluksessa.

**Määrittää Azure AD kertakirjautumisen Yonyx vuorovaikutteisissa oppaissa, toimimalla seuraavasti:**

1. Valitse perinteinen-portaalissa integrointi **Yonyx vuorovaikutteisissa oppaissa** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen Single Sign** -valintaikkunan avaaminen.
     
    ![Kertakirjautumisen määrittäminen][6] 

2. **Miten käyttäjät voivat kirjautua Yonyx vuorovaikutteisissa oppaissa haluat käyttää** sivulla Valitse **Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.
    
    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-yonyx-tutorial/tutorial_yonyx_03.png)

3. Valitse **Määritä sovelluksen asetukset** -sivulla suorittamalla seuraavat vaiheet ja valitse **Seuraava**:

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-yonyx-tutorial/tutorial_yonyx_04.png)

    a. Kirjoita **Merkki-URL** -tekstiruutuun URL-osoite, käyttämällä seuraavaa kaavaa: `https://<company name>.yonyx.com/y/conversation/?id=<guid number>`.

    b. Kirjoita **tunnus** -tekstiruutuun URL-osoite, käyttämällä seuraavaa kaavaa: `https://<company name>.yonyx.com`.

    c-näppäinyhdistelmää. Valitse **Seuraava**

    > [AZURE.NOTE] Huomaa, että sinulla on Päivitä arvot todellisen merkki-URL-Osoitteen ja tunnus. Hanki nämä arvot ottamalla yhteyttä Yonyx vuorovaikutteisissa oppaissa tekniseen tukeen kautta <mailto:support@yonyx.com>.

4. Valitse **Määritä kertakirjautumisen osoitteessa Yonyx vuorovaikutteisissa oppaissa** -sivulla valitsemalla **Lataa** ja tallenna sitten tiedosto tietokoneesta:

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-yonyx-tutorial/tutorial_yonyx_05.png)

5. SSO määritetty sovellus, yhteyshenkilön Yonyx vuorovaikutteinen apuviivojen tekniseen tukeen kautta pääset <mailto:support@yonyx.com> ja annettava seuraavat:

    • Ladatut **varmenne**

    • **Myöntäjä URL-osoite**

    • **Sign-palvelun URL-osoite**

    • **Yksittäisen Kirjaudu ulos palvelun URL-osoite**

6. Perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Seuraava**.
    
    ![Azure AD-Single Sign-On][10]

7. Valitse **Sign Vahvista** -sivulla **Valmis**.  
    
    ![Azure AD-Single Sign-On][11]



### <a name="creating-an-azure-ad-test-user"></a>Azure AD-testikäyttäjän luominen
Tässä osassa tavoitteena on luotava testikäyttäjän kutsutaan Britta Simon perinteinen portaalissa.

![Luo Azure AD-käyttäjä][20]

**Voit luoda testikäyttäjän Azure AD-toimimalla seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen Portal**, valitse vasemmassa siirtymisruudussa.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-yonyx-tutorial/create_aaduser_09.png)

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Ylä-valikossa käyttäjäluettelon, näkyviin napsauttamalla **käyttäjät**.
    
    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-yonyx-tutorial/create_aaduser_03.png)

4. Avaa **Lisää käyttäjä** -valintaikkunan alareunassa työkalurivillä, valitse **Lisää käyttäjä**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-yonyx-tutorial/create_aaduser_04.png)

5. Tee valintaikkunassa **Kerro kyseisen käyttäjän tietoja** -sivulla seuraavasti:

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-yonyx-tutorial/create_aaduser_05.png)

    a. Käyttäjänä tyyppi Valitse uusi käyttäjä organisaatiossa.

    b. Kirjoita käyttäjänimi- **tekstiruutuun** **BrittaSimon**.

    c-näppäinyhdistelmää. Valitse **Seuraava**.

6.  Suorita **Käyttäjäprofiili** -sivulla seuraavat toimet:
    
    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-yonyx-tutorial/create_aaduser_06.png)

    a. Kirjoita **Ensimmäinen nimi** -tekstiruutuun **Britta**.  

    b. Kirjoita **Simon** **Sukunimi** -tekstiruutuun.

    c-näppäinyhdistelmää. Kirjoita **Näyttönimi** -tekstiruutuun **Britta Simon**.

    d. Valitse **rooli** -luettelosta **käyttäjä**.

    e. Valitse **Seuraava**.

7. Valitse **Hae tilapäinen salasana** -valintaikkunan sivulla **Luo**.
    
    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-yonyx-tutorial/create_aaduser_07.png)

8. **Hae tilapäinen salasana** -valintaikkunan sivulla toimimalla seuraavasti:
    
    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-yonyx-tutorial/create_aaduser_08.png)

    a. Kirjoita **Uusi salasana**arvo muistiin.

    b. Valitse **Valmis**.   



### <a name="creating-a-yonyx-interactive-guides-test-user"></a>Yonyx vuorovaikutteisissa oppaissa testikäyttäjän luominen

Tässä osassa tavoitteena on nimeltään Britta Simon Yonyx vuorovaikutteisissa oppaissa käyttäjän luominen. Yonyx vuorovaikutteisissa oppaissa tukee vain perustuvan valmistelu, joka on oletusarvoisesti käytössä.

Ei ole toimi puolestasi sisältö. Uusi käyttäjä luodaan yritys käyttää Adobe Creative pilveen, jos sitä ei ole vielä aikana.

> [AZURE.NOTE] Jos haluat luoda luodun käyttäjän manuaalisesti, sinun on Yonyx vuorovaikutteisissa oppaissa tukeen kautta <mailto:support@yonyx.com>.


### <a name="assigning-the-azure-ad-test-user"></a>Azure AD-testikäyttäjän määrittäminen

Tässä osassa tavoitteena on Britta Simon käyttämään Azure kertakirjautumisen hänen käyttöoikeuden myöntämisestä Yonyx vuorovaikutteisissa oppaissa ottaminen käyttöön.
    
![Määrittää käyttäjälle][200]

**Jos haluat määrittää Britta Simon Yonyx vuorovaikutteisissa oppaissa, toimi seuraavasti:**

1. Perinteinen-portaalissa Avaa sovellukset-näkymässä kansio-näkymässä, valitse **sovellukset** yläreunan-valikossa.
    
    ![Määrittää käyttäjälle][201]

2. Valitse sovellukset-luettelosta **Yonyx vuorovaikutteisissa oppaissa**.
    
    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-yonyx-tutorial/tutorial_yonyx_50.png)

3. Valitse ylä-valikossa Valitse **käyttäjiä**.
    
    ![Määrittää käyttäjälle][203]

4. Valitse käyttäjät-luettelosta **Britta Simon**.

5. Napsauta työkalurivin alaosassa **määrittäminen**.
    
    ![Määrittää käyttäjälle][205]

### <a name="testing-single-sign-on"></a>Kertakirjautumisen testaaminen

Tässä osassa tavoitteena on Testaa Azure AD yksittäisen Sign-asetusten käyttäminen Access-paneeli.
 
Access-ruudussa Yonyx vuorovaikutteisissa oppaissa-ruutua napsauttaessasi voit olisi Hae automaattisesti kirjautunut sisään Yonyx vuorovaikutteisissa oppaissa sovelluksen.

## <a name="additional-resources"></a>Lisäresursseja

* [Luettelo siitä, miten voit integroida SaaS sovellusten Azure Active Directory-opetusohjelmat](active-directory-saas-tutorial-list.md)
* [Mikä on sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-yonyx-tutorial/tutorial_general_205.png
