<properties
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi RunMyProcess | Microsoft Azure"
    description="Opettele määrittämään kertakirjautumisen Azure Active Directory- ja RunMyProcess välillä."
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
    ms.date="10/21/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-runmyprocess"></a>Opetusohjelma: RunMyProcess Azure Active Directory-integrointi

Tässä opetusohjelmassa tavoitteena on, kuinka voit integroida RunMyProcess Azure Active Directory (Azure AD).

Azure AD-integraation RunMyProcess avulla voit seuraavat edut:

- Voit hallita Azure AD, jolla on pääsy RunMyProcess
- Voit ottaa käyttöön automaattisesti Hae kirjautunut käyttöön RunMyProcess (kertakirjautumisen) niiden Azure AD-tilien käyttäjille
- Voit hallita asiakkaiden yhdessä keskitetyssä sijainnissa - Azure perinteinen portal

Jos haluat tietää tarkempia tietoja SaaS sovelluksen integrointi Azure AD-kohdassa [mitä sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Edellytykset

Määrittää Azure AD-integroinnin RunMyProcess, tarvitset seuraavat:

- Azure AD-tilaus
- RunMyProcess single-merkki käytössä-tilauksessa


> [AZURE.NOTE] Voit esikatsella vaiheet Tässä opetusohjelmassa, Emme suosittele käyttämällä tuotantoympäristössä.


Voit esikatsella vaiheet Tässä opetusohjelmassa-noudattamalla näitä suosituksia:

- Älä käytä tuotantoympäristössä, ellei se on tarpeen.
- Jos sinulla ei ole kokeiluversion Azure AD-ympäristössä, voit hankkia yksi kuukausi kokeiluversion [tähän](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Skenaarion kuvaus
Tässä opetusohjelmassa tavoitteena on, jotta voit testata Azure AD kertakirjautumisen testiympäristössä.

Tässä opetusohjelmassa kuvatut skenaarion kuuluu kaksi tärkeimmät rakenneosien:

1. RunMyProcess lisääminen valikoimasta
2. Yksittäinen määrittäminen ja testaus Azure AD-on


## <a name="adding-runmyprocess-from-the-gallery"></a>RunMyProcess lisääminen valikoimasta
Voit määrittää RunMyProcess integroida Azure AD-haluat lisätä RunMyProcess valikoimasta hallitun SaaS sovellusluettelo.

**Jos haluat lisätä RunMyProcess valikoimasta, toimi seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen portal**, valitse vasemmassa siirtymisruudussa.

    ![Active Directory][1]

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset][2]

4. Valitse sivun alareunassa **Lisää** .

    ![Sovellukset][3]

5. Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Sovellukset][4]

6. Kirjoita hakuruutuun **RunMyProcess**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_01.png)

7. Valitse tulosruudussa **RunMyProcess**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_0001.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yksittäinen määrittäminen ja testaus Azure AD-on
Tässä osassa tavoitteena on noudattamalla voit määrittää ja testaa Azure AD kertakirjautumisen RunMyProcess nimeltä "Britta Simon" testikäyttäjän perusteella.

Kertakirjautumisen toimimaan Azure AD tarvitsee tietää käyttäjälle on RunMyProcess vastaavaan käyttäjän Azure AD-on. Toisin sanoen suhteen Azure AD-käyttäjä ja Aiheeseen liittyvät RunMyProcess-linkki on vahvistettava.

Linkki-yhteys on muodostettu määrittämällä **käyttäjänimi** arvo arvoksi RunMyProcess **käyttäjänimi** Azure AD.

Määrittäminen ja testaaminen Azure AD kertakirjautumisen RunMyProcess kanssa, sinun on tehtävä seuraavat hakukyselyn:

1. **[Azure AD määrittäminen Single Sign](#configuring-azure-ad-single-sign-on)** - käyttäjät voivat käyttää tätä toimintoa.
2. **[Käyttäjän testata luominen Azure AD](#creating-an-azure-ad-test-user)** - Testaa Azure AD kertakirjautumisen Britta Simon kanssa.
3. **[Luomisesta RunMyProcess testata käyttäjän](#creating-a-runmyprocess-test-user)** - tapahtumista Britta Simon ovat RunMyProcess, jotka on linkitetty hän Azure AD esitys.
4. **[Käyttäjän testata määrittäminen Azure AD](#assigning-the-azure-ad-test-user)** - Britta Simon käyttämään Azure AD kertakirjautumisen käyttöön.
5. **[Testaus Single Sign](#testing-single-sign-on)** - voit tarkistaa, toimiiko määritykset.


### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD kertakirjautumisen määrittäminen

Tässä osassa Azure AD kertakirjautumisen perinteinen portaalissa ottaminen käyttöön ja määritä kertakirjautumisen RunMyProcess-sovelluksessa.

**Määrittää Azure AD kertakirjautumisen RunMyProcess, toimimalla seuraavasti:**

1. Valitse perinteinen-portaalissa integrointi **RunMyProcess** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen Single Sign** -valintaikkunan avaaminen.
     
    ![Kertakirjautumisen määrittäminen][6] 

2. **Miten käyttäjät voivat kirjautua RunMyProcess haluat käyttää** sivulla Valitse **Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_03.png) 

3. Valitse **Määritä sovelluksen asetukset** -sivulla toimimalla seuraavasti:

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_04.png) 

    a. Kirjoita **Merkki-URL** -tekstiruutuun URL-osoite, käyttämällä seuraavaa kaavaa: `https://live.runmyprocess.com/live/<tenant id>`.
        
    b. Valitse **Seuraava**

    > [AZURE.NOTE] Huomaa, että sinulla on Päivitä arvon todellisen merkki-URL-Osoitteen. Saat tämän arvon-yhteyden RunMyProcess tukeen kautta <mailto:support@runmyprocess.com>.
 
4. Valitse **Määritä kertakirjautumisen osoitteessa RunMyProcess** -sivulla valitsemalla **Lataa sertifikaatti** ja tallenna sitten tiedosto tietokoneesta:

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_05.png)

5. Eri selainikkunassa Sign RunMyProcess asiakasympäristöön järjestelmänvalvojana.

6. Vasemmanpuoleisessa ruudussa Valitse **tili** ja valitse **määritys**.

    ![Sovelluksen reunassa kertakirjautumisen määrittäminen](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_001.png)

7. Siirry **todentamismenetelmän** kohtaan ja tee vaiheet alla:

    ![Sovelluksen reunassa kertakirjautumisen määrittäminen](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_002.png)

    a. Valitse **SSO kanssa Samlv2**- **menetelmää**.

    b. - **SSO uudelleenohjata** tekstiruudun valitseminen **SAML SSO URL-Osoitteen** arvo Azure AD-sovelluksen määritystoimintoa.

    c-näppäinyhdistelmää. Valitse **Kirjaudu ulos uudelleenohjata** tekstiruudun valitseminen arvosta **Yhden Sign-Out palvelun URL-osoite** Azure AD-sovelluksen määritystoiminto.

    d. **Tunnus nimimuotoa** tekstiruudun valitseminen **Tunnus nimimuotoa** arvon Azure AD-sovelluksen määritystoimintoa.

    e. Ladatun sertifikaattitiedosto sisällön kopioiminen ja liittää sen **varmenne** -tekstiruutuun. 

    f. Valitse **Tallenna** -kuvaketta.
    
8. Perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Seuraava**.
    
    ![Azure AD-Single Sign-On][10]

9. Valitse **Sign Vahvista** -sivulla **Valmis**.  
 
    ![Azure AD-Single Sign-On][11]


### <a name="creating-an-azure-ad-test-user"></a>Azure AD-testikäyttäjän luominen
Tässä osassa tavoitteena on luotava testikäyttäjän kutsutaan Britta Simon perinteinen portaalissa.

![Luo Azure AD-käyttäjä][20]

**Voit luoda testikäyttäjän Azure AD-toimimalla seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen portal**, valitse vasemmassa siirtymisruudussa.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_01.png) 

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Ylä-valikossa käyttäjäluettelon, näkyviin napsauttamalla **käyttäjät**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_02.png) 

4. Avaa **Lisää käyttäjä** -valintaikkunan alareunassa työkalurivillä, valitse **Lisää käyttäjä**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_03.png) 

5. Suorita valintaikkunan **Kerro kyseisen käyttäjän tietoja** -sivulla seuraavat toimet:  ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_04.png) 

    a. Käyttäjänä tyyppi Valitse uusi käyttäjä organisaatiossa.

    b. Kirjoita käyttäjänimi- **tekstiruutuun** **BrittaSimon**.

    c-näppäinyhdistelmää. Valitse **Seuraava**.

6.  Suorita valintaikkunan **Käyttäjäprofiili** -sivulla seuraavat toimet: ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_05.png) 

    a. Kirjoita **Ensimmäinen nimi** -tekstiruutuun **Britta**.  

    b. Kirjoita **Sukunimi** -tekstiruutuun tyyppi- **Simon**.

    c-näppäinyhdistelmää. Kirjoita **Näyttönimi** -tekstiruutuun **Britta Simon**.

    d. Valitse **rooli** -luettelosta **käyttäjä**.

    e. Valitse **Seuraava**.

7. Valitse **Hae tilapäinen salasana** -valintaikkunan sivulla **Luo**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_06.png) 

8. **Hae tilapäinen salasana** -valintaikkunan sivulla toimimalla seuraavasti:

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_07.png) 

    a. Kirjoita **Uusi salasana**arvo muistiin.

    b. Valitse **Valmis**.   

### <a name="creating-a-runmyprocess-test-user"></a>RunMyProcess testikäyttäjän luominen
  
Jos haluat ottaa käyttöön Azure AD-käyttäjät voivat kirjautua sisään RunMyProcess, ne on valmisteltava RunMyProcess kyselyjä. RunMyProcess valmistelu on manuaalinen tehtävä.

#### <a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Valmistelu käyttäjätilit, toimi seuraavasti:

1.  Kirjaudu sisään RunMyProcess yrityksesi sivuston järjestelmänvalvojan oikeuksilla.

2.  Valitse **tili** ja valitse vasemmassa siirtymisruudussa **käyttäjät** ja valitse sitten **Uusi käyttäjä**.

    ![Uusi käyttäjä] (./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_003.png "Uusi käyttäjä")

3.  Valitse **Käyttäjäasetukset** -kohdassa toimimalla seuraavasti:

    ![Profiili] (./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_004.png "Profiili")

    a. Kirjoita **nimi** ja kelvollinen haluat säännöstä kyselyjä liittyvät tekstiruutujen AAD tilin **Sähköposti** .

    b. Valitse on **IDE kieli**-, **kieli** - ja **profiili**.

    c-näppäinyhdistelmää. Valitse **Lähetä minulle luominen sähköposti**.

    d. Valitse **Tallenna**.

    >[AZURE.NOTE] Voit käyttää mitä tahansa muita RunMyProcess käyttäjän tilin luontityökalut tai ohjelmointirajapinnan myöntämä RunMyProcess valmistelu Azure Active Directory käyttäjätilit.
    

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD-testikäyttäjän määrittäminen

Tässä osassa tavoitteena on Britta Simon käyttämään Azure kertakirjautumisen hänen käyttöoikeuden myöntämisestä RunMyProcess ottaminen käyttöön.

![Määrittää käyttäjälle][200] 

**Jos haluat määrittää Britta Simon RunMyProcess, toimi seuraavasti:**

1. Perinteinen-portaalissa Avaa sovellukset-näkymässä kansio-näkymässä, valitse **sovellukset** yläreunan-valikossa.

    ![Määrittää käyttäjälle][201] 

2. Valitse sovellukset-luettelosta **RunMyProcess**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_50.png) 

3. Valitse ylä-valikossa Valitse **käyttäjiä**.

    ![Määrittää käyttäjälle][203]

4. Valitse käyttäjät-luettelosta **Britta Simon**.

5. palkki ja valitse **Liitä**.

    ![Määrittää käyttäjälle][205]


### <a name="testing-single-sign-on"></a>Kertakirjautumisen testaaminen

Tässä osassa tavoitteena on Testaa Azure AD yksittäisen Sign-asetusten käyttäminen Access-paneeli.
 
Access-ruudussa RunMyProcess-ruutua napsauttaessasi sinun pitäisi saada automaattisesti kirjautunut-on RunMyProcess sovelluksen.


## <a name="additional-resources"></a>Lisäresursseja

* [Luettelo siitä, miten voit integroida SaaS sovellusten Azure Active Directory-opetusohjelmat](active-directory-saas-tutorial-list.md)
* [Mikä on sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_205.png
