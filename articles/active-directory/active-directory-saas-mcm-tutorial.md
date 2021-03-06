<properties 
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi MCM | Microsoft Azure" 
    description="Opettele käyttämään MCM Azure Active Directory-hakemistosta käyttöön kertakirjautumisen, automaattinen valmistelu ja lisää!" 
    services="active-directory" 
    authors="jeevansd"  
    documentationCenter="na" 
    manager="femila"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="08/30/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-mcm"></a>Opetusohjelma: MCM Azure Active Directory-integrointi
  
Tässä opetusohjelmassa tavoitteena on, kuinka voit integroida MCM Azure Active Directory (Azure AD).

Azure AD-integraation MCM avulla voit seuraavat edut:

- Voit hallita Azure AD, jolla on pääsy MCM
- Voit ottaa käyttöön automaattisesti Hae kirjautunut käyttöön MCM (kertakirjautumisen) niiden Azure AD-tilien käyttäjille
- Voit hallita asiakkaiden yhdessä keskitetyssä sijainnissa - Azure perinteinen portal

Jos haluat tietää tarkempia tietoja SaaS sovelluksen integrointi Azure AD-kohdassa [mitä sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Edellytykset

Määrittää Azure AD-integroinnin MCM, tarvitset seuraavat:

- Kelvollinen Azure tilaus
- MCM single-merkki käytössä-tilauksessa


> [AZURE.NOTE] Voit esikatsella vaiheet Tässä opetusohjelmassa, Emme suosittele käyttämällä tuotantoympäristössä.


Voit esikatsella vaiheet Tässä opetusohjelmassa-noudattamalla näitä suosituksia:

- Älä käytä tuotantoympäristössä, ellei se on tarpeen.
- Jos sinulla ei ole kokeiluversion Azure AD-ympäristössä, voit hankkia yksi kuukausi kokeiluversion [tähän](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Skenaarion kuvaus
Tässä opetusohjelmassa tavoitteena on, jotta voit testata Azure AD kertakirjautumisen testiympäristössä.

Tässä opetusohjelmassa kuvatut skenaarion kuuluu kaksi tärkeimmät rakenneosien:

1. MCM lisääminen valikoimasta
2. Yksittäinen määrittäminen ja testaus Azure AD-on

## <a name="adding-mcm-from-the-gallery"></a>MCM lisääminen valikoimasta
Voit määrittää MCM integroida Azure AD-haluat lisätä MCM valikoimasta hallitun SaaS sovellusluettelo.

**Jos haluat lisätä MCM valikoimasta, toimi seuraavasti:**

1.  Valitse **Active Directory**Azure perinteinen portaalin vasemmanpuoleisessa siirtymisruudussa.

    ![Active Directory] (./media/active-directory-saas-mcm-tutorial/tutorial_general_01.png "Active Directory")

2.  Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3.  Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset] (./media/active-directory-saas-mcm-tutorial/tutorial_general_02.png "Sovellukset")

4.  Valitse sivun alareunassa **Lisää** .

    ![Lisää sovellus] (./media/active-directory-saas-mcm-tutorial/tutorial_general_03.png "Lisää sovellus")

5.  Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Lisää sovellus-gallerry] (./media/active-directory-saas-mcm-tutorial/tutorial_general_04.png "Lisää sovellus-gallerry")

6.  Kirjoita **hakuruutuun** **MCM**.

    ![Sovelluksen valikoima] (./media/active-directory-saas-mcm-tutorial/tutorial_mcm_01.png "Sovelluksen valikoima")

7.  Valitse tulosruudussa **MCM**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![MCM] (./media/active-directory-saas-mcm-tutorial/tutorial_mcm_001.png "MCM")

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yksittäinen määrittäminen ja testaus Azure AD-on
Tässä osassa tavoitteena on noudattamalla voit määrittää ja testaa Azure AD kertakirjautumisen MCM nimeltä "Britta Simon" testikäyttäjän perusteella.

Kertakirjautumisen toimimaan Azure AD on tiedettävä vastaavaan käyttäjän MCM, Azure AD-käyttäjälle. Toisin sanoen suhteen Azure AD-käyttäjä ja Aiheeseen liittyvät MCM-linkki on vahvistettava.

Linkki-yhteys on muodostettu määrittämällä **käyttäjänimi** arvo arvoksi MCM **käyttäjänimi** Azure AD.

Määrittäminen ja testaaminen Azure AD kertakirjautumisen MCM kanssa, sinun on tehtävä seuraavat hakukyselyn:

1. **[Azure AD määrittäminen Single Sign](#configuring-azure-ad-single-single-sign-on)** - käyttäjät voivat käyttää tätä toimintoa.
2. **[Käyttäjän testata luominen Azure AD](#creating-an-azure-ad-test-user)** - Testaa Azure AD kertakirjautumisen Britta Simon kanssa.
3. **[Luomisesta MCM testata käyttäjän](#creating-a-mcm-test-user)** - tapahtumista Britta Simon ovat MCM, jotka on linkitetty hän Azure AD esitys.
4. **[Käyttäjän testata määrittäminen Azure AD](#assigning-the-azure-ad-test-user)** - Britta Simon käyttämään Azure AD kertakirjautumisen käyttöön.
5. **[Testaus Single Sign](#testing-single-sign-on)** - voit tarkistaa, toimiiko määritykset.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD kertakirjautumisen määrittäminen
  
Tässä osassa Azure AD kertakirjautumisen perinteinen portaalissa ottaminen käyttöön ja määritä kertakirjautumisen MCM-sovelluksessa.

**Määrittää Azure AD kertakirjautumisen MCM, toimimalla seuraavasti:**

1.  Valitse Azure perinteinen portaalissa integrointi **MCM** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-mcm-tutorial/tutorial_general_05.png "Määritä kertakirjautuminen")

2.  **Miten käyttäjät voivat kirjautua MCM haluat käyttää** sivulla Valitse **Microsoft Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Microsoft Azure AD-Single Sign-On] (./media/active-directory-saas-mcm-tutorial/tutorial_mcm_03.png "Microsoft Azure AD-Single Sign-On")

3.  Valitse Määritä sovelluksen asetukset-sivulla toimimalla seuraavasti:

    ![Sovelluksen URL-Osoitteen määrittäminen] (./media/active-directory-saas-mcm-tutorial/tutorial_mcm_04.png "Sovelluksen URL-Osoitteen määrittäminen")

    a. Kirjoita **Merkki-URL** -tekstiruutuun: `https://myaba.co.uk/client-access/<company name>/saml.php`.
    
    b. Valitse **Seuraava**

4.  Valitse **Määritä kertakirjautumisen osoitteessa MCM** -sivulla valitsemalla **Lataa metatiedot**ja Tallenna sertifikaattitiedosto tietokoneeseen.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-mcm-tutorial/tutorial_mcm_05.png "Kertakirjautumisen määrittäminen")

5. Saat määritetty sovelluksen SSO-yhteyden MCM tukeen. Liitä ladatut metatieto-tiedosto ja jaa se määrittämään SSO kiertäminen MCM ryhmän kanssa.

6.  Perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Seuraava**.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-mcm-tutorial/tutorial_mcm_06.png "Kertakirjautumisen määrittäminen")

7. Valitse **Sign Vahvista** -sivulla **Valmis**.

    ![Kertakirjautumisen määrittäminen] (./media/active-directory-saas-mcm-tutorial/tutorial_mcm_07.png "Kertakirjautumisen määrittäminen")


### <a name="creating-an-azure-ad-test-user"></a>Azure AD-testikäyttäjän luominen

Tässä osassa tavoitteena on luotava testikäyttäjän kutsutaan Britta Simon perinteinen portaalissa.

![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-mcm-tutorial/create_aaduser_00.png)

**Voit luoda testikäyttäjän Azure AD-toimimalla seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen Portal**, valitse vasemmassa siirtymisruudussa.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-mcm-tutorial/create_aaduser_01.png)

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Ylä-valikossa käyttäjäluettelon, näkyviin napsauttamalla **käyttäjät**.
    
    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-mcm-tutorial/create_aaduser_02.png)

4. Avaa **Lisää käyttäjä** -valintaikkunan alareunassa työkalurivillä, valitse **Lisää käyttäjä**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-mcm-tutorial/create_aaduser_03.png)

5. Tee valintaikkunassa **Kerro kyseisen käyttäjän tietoja** -sivulla seuraavasti:

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-mcm-tutorial/create_aaduser_04.png)

    a. Käyttäjänä tyyppi Valitse uusi käyttäjä organisaatiossa.

    b. Kirjoita käyttäjänimi- **tekstiruutuun** **BrittaSimon**.

    c-näppäinyhdistelmää. Valitse **Seuraava**.

6.  Suorita **Käyttäjäprofiili** -sivulla seuraavat toimet:
    
    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-mcm-tutorial/create_aaduser_05.png)

    a. Kirjoita **Ensimmäinen nimi** -tekstiruutuun **Britta**.  

    b. Kirjoita **Sukunimi** -tekstiruutuun tyyppi- **Simon**.

    c-näppäinyhdistelmää. Kirjoita **Näyttönimi** -tekstiruutuun **Britta Simon**.

    d. Valitse **rooli** -luettelosta **käyttäjä**.

    e. Valitse **Seuraava**.

7. Valitse **Hae tilapäinen salasana** -valintaikkunan sivulla **Luo**.
    
    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-mcm-tutorial/create_aaduser_06.png)

8. **Hae tilapäinen salasana** -valintaikkunan sivulla toimimalla seuraavasti:
    
    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-mcm-tutorial/create_aaduser_07.png)

    a. Kirjoita **Uusi salasana**arvo muistiin.

    b. Valitse **Valmis**.   

###<a name="creating-a-mcm-test-user"></a>MCM testikäyttäjän luominen
  
Tässä osassa nimeltä Britta Simon MCM käyttäjän luominen. Toimi MCM tukihenkilöstöösi voit lisätä käyttäjiä MCM-ympäristössä.

>[AZURE.NOTE]Voit käyttää mitä tahansa muita MCM käyttäjän tilin luontityökalut tai ohjelmointirajapinnan myöntämä MCM säännöstä AAD käyttäjätileille.


###<a name="assigning-the-azure-ad-test-user"></a>Azure AD-testikäyttäjän määrittäminen
  
Tässä osassa tavoitteena on Britta Simon käyttämään Azure kertakirjautumisen hänen käyttöoikeuden myöntämisestä MCM ottaminen käyttöön.
    
![Käyttäjien määrittäminen] (./media/active-directory-saas-mcm-tutorial/assign_aaduser_00.png "Käyttäjien määrittäminen")

**Jos haluat määrittää Britta Simon MCM, toimi seuraavasti:**

1. Perinteinen-portaalissa Avaa sovellukset-näkymässä kansio-näkymässä, valitse **sovellukset** yläreunan-valikossa.
    
    ![Käyttäjien määrittäminen] (./media/active-directory-saas-mcm-tutorial/assign_aaduser_01.png "Käyttäjien määrittäminen")

2. Valitse sovellukset-luettelosta **MCM**.
    
    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-mcm-tutorial/tutorial_mcm_08.png)

1. Valitse ylä-valikossa Valitse **käyttäjiä**.
    
    ![Käyttäjien määrittäminen] (./media/active-directory-saas-mcm-tutorial/assign_aaduser_02.png "Käyttäjien määrittäminen")

1. Valitse käyttäjät-luettelosta **Britta Simon**.

2. Napsauta työkalurivin alaosassa **määrittäminen**.
    
    ![Käyttäjien määrittäminen] (./media/active-directory-saas-mcm-tutorial/assign_aaduser_03.png "Käyttäjien määrittäminen")


### <a name="testing-single-sign-on"></a>Kertakirjautumisen testaaminen

Tässä osassa tavoitteena on Testaa Azure AD yksittäisen Sign-asetusten käyttäminen Access-paneeli.
 
Access-ruudussa MCM-ruutua napsauttaessasi sinun pitäisi saada automaattisesti kirjautunut-on MCM sovelluksen.


## <a name="additional-resources"></a>Lisäresursseja

* [Luettelo siitä, miten voit integroida SaaS sovellusten Azure Active Directory-opetusohjelmat](active-directory-saas-tutorial-list.md)
* [Mikä on sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta?](active-directory-appssoaccess-whatis.md)