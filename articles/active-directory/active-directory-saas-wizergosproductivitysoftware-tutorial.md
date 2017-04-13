<properties
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi Wizergos tuottavuutta ohjelmiston | Microsoft Azure"
    description="Opettele määrittämään kertakirjautumisen Azure Active Directory- ja Wizergos tuottavuutta ohjelmiston välillä."
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
    ms.date="10/17/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-wizergos-productivity-software"></a>Opetusohjelma: Wizergos tuottavuutta ohjelmiston Azure Active Directory-integrointi 

Tässä opetusohjelmassa tavoitteena on, kuinka voit integroida Wizergos tuottavuutta ohjelmiston Azure Active Directory (Azure AD).

Azure AD-integraation Wizergos tuottavuus ohjelmisto, jonka avulla seuraavat edut:

- Voit hallita Azure AD, jolla on pääsy Wizergos tuottavuutta ohjelmisto
- Voit ottaa käyttöön automaattisesti Hae kirjautunut sisään Wizergos tuottavuutta-ohjelmistoa (kertakirjautumisen) käyttäjille heidän Azure AD-tilien kanssa
- Voit hallita asiakkaiden yhdessä keskitetyssä sijainnissa - Azure perinteinen portal

Jos haluat tietää tarkempia tietoja SaaS sovelluksen integrointi Azure AD-kohdassa [mitä sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Edellytykset

Määrittää Azure AD-integroinnin Wizergos tuottavuutta ohjelmiston, sinun on seuraavia kohteita:

- Azure AD-tilaus
- Wizergos tuottavuutta ohjelmiston single-merkki käytössä-tilauksessa


> [AZURE.NOTE] Voit esikatsella vaiheet Tässä opetusohjelmassa, Emme suosittele käyttämällä tuotantoympäristössä.


Voit esikatsella vaiheet Tässä opetusohjelmassa-noudattamalla näitä suosituksia:

- Älä käytä tuotantoympäristössä, ellei se on tarpeen.
- Jos sinulla ei ole kokeiluversion Azure AD-ympäristössä, voit hankkia yksi kuukausi kokeiluversion [tähän](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Skenaarion kuvaus
Tässä opetusohjelmassa tavoitteena on, jotta voit testata Azure AD kertakirjautumisen testiympäristössä.

Tässä opetusohjelmassa kuvatut skenaarion kuuluu kaksi tärkeimmät rakenneosien:

1. Wizergos tuottavuutta ohjelmiston lisääminen valikoimasta
2. Yksittäinen määrittäminen ja testaus Azure AD-on


## <a name="adding-wizergos-productivity-software-from-the-gallery"></a>Wizergos tuottavuutta ohjelmiston lisääminen valikoimasta
Voit määrittää Wizergos tuottavuutta ohjelmiston integroida Azure AD-haluat lisätä Wizergos tuottavuutta ohjelmiston valikoimasta hallitun SaaS sovellusluettelo.

**Voit lisätä Wizergos tuottavuutta ohjelmiston valikoimasta, toimi seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen Portal**, valitse vasemmassa siirtymisruudussa. 

    ![Active Directory][1]

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .
    
    ![Sovellukset][2]

4. Valitse sivun alareunassa **Lisää** .
    
    ![Sovellukset][3]

5. Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Sovellukset][4]

6. Kirjoita hakuruutuun **Wizergos tuottavuutta ohjelmiston**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_01.png)

7. Tulokset-ruudussa Valitse **Wizergos tuottavuus ohjelmisto**ja valitse sitten **Valmis** , voit lisätä sovelluksen.

    ![Sovelluksen valitseminen valikoimasta](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_001.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yksittäinen määrittäminen ja testaus Azure AD-on
Tässä osassa tavoitteena on noudattamalla voit määrittää ja testaa Azure AD kertakirjautumisen Wizergos tuottavuutta ohjelmiston nimeltä "Britta Simon" testikäyttäjän perusteella.

Kertakirjautumisen toimimaan Azure AD on tiedettävä vastaavaan käyttäjän Wizergos tuottavuus ohjelmisto, Azure AD-käyttäjälle. Toisin sanoen linkki Azure AD-käyttäjä ja Aiheeseen liittyvät Wizergos tuottavuutta ohjelmiston suhteen on vahvistettava.

Linkki-yhteys on muodostettu määrittämällä **käyttäjänimi** arvo arvoksi Wizergos tuottavuutta ohjelmiston **käyttäjänimi** Azure AD.

Määrittäminen ja testaaminen Azure AD kertakirjautumisen BynWizergos tuottavuutta Softwareder kanssa, sinun on tehtävä seuraavat hakukyselyn:

1. **[Azure AD määrittäminen Single Sign](#configuring-azure-ad-single-single-sign-on)** - käyttäjät voivat käyttää tätä toimintoa.
2. **[Käyttäjän testata luominen Azure AD](#creating-an-azure-ad-test-user)** - Testaa Azure AD kertakirjautumisen Britta Simon kanssa.
3. **[Wizergos tuottavuutta-ohjelmistojen luomista testata käyttäjän](#creating-a-wizergos-productivity-software-test-user)** - tapahtumista Britta Simon ovat Wizergos tuottavuutta ohjelmia, jotka on linkitetty hän Azure AD-esittäminen.
4. **[Käyttäjän testata määrittäminen Azure AD](#assigning-the-azure-ad-test-user)** - Britta Simon käyttämään Azure AD kertakirjautumisen käyttöön.
5. **[Testaus Single Sign](#testing-single-sign-on)** - voit tarkistaa, toimiiko määritykset.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD kertakirjautumisen määrittäminen

Tässä osassa Azure AD kertakirjautumisen perinteinen portaalissa käyttöön ja määrittäminen kertakirjautumisen Wizergos tuottavuutta ohjelmistosovellus.

**Voit määrittää Azure AD kertakirjautumisen Wizergos tuottavuutta ohjelmistolla toimimalla seuraavasti:**

1. Valitse perinteinen-portaalissa **Wizergos tuottavuutta ohjelmisto** sovelluksen integrointi-sivun **Määritä kertakirjautumisen** **Määrittäminen Single Sign** -valintaikkunan avaaminen.
     
    ![Kertakirjautumisen määrittäminen][6] 

2. **Miten Haluatko käyttäjät kirjautumaan Wizergos tuottavuutta ohjelmisto** -sivulla Valitse **Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**:
    
    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_03.png)

3. Valitse **Määritä sovelluksen asetukset** -sivulla **Seuraava**:

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_04.png)

4. Valitse **Määritä kertakirjautumisen Wizergos tuottavuutta-ohjelma** -sivulla **Lataa**ja tallenna sitten tiedosto tietokoneesta:

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_05.png)

5. Eri selainikkunassa Sign Wizergos tuottavuutta ohjelmiston asiakasympäristöön järjestelmänvalvojana.

6. Hamburger-valikosta valitsemalla **järjestelmänvalvoja**.

    ![Määrittää yksittäisen Sign-On-sovelluksen puoli](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_000.png)

7. Valitse järjestelmänvalvoja-sivun vasemmalla olevasta valikosta **todennus** ja valitse **Azure AD**.

    ![Määrittää yksittäisen Sign-On-sovelluksen puoli](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_002.png)

8. Seuraavien toimien **todennus** -osassa.

    ![Määrittää yksittäisen Sign-On-sovelluksen puoli](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_003.png)

    a. Valitse **Lataa** -painiketta Lataa ladatut sertifikaatti Azure AD. 

    b. **Myöntäjä URL-Osoitteen** tekstiruutu sijoittaa **Myöntäjä URL-Osoitteen** arvo Azure AD-sovelluksen määritystoiminto.

    c-näppäinyhdistelmää. **Sign-On URL** -tekstiruutuun sijoittaa arvon **Sign-palvelun URL** Azure AD-sovelluksen määritystoimintoa.

    d. **Yhteen Sign-Out URL** -tekstiruutuun sijoittaa arvosta **Yhden Sign-out palvelun URL-osoite** Azure AD-sovelluksen määritystoimintoa.

    e. Napsauta **Tallenna** -painiketta.

9. Perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Seuraava**.
    
    ![Azure AD-Single Sign-On][10]

10. Valitse **Sign Vahvista** -sivulla **Valmis**.  
    
    ![Azure AD-Single Sign-On][11]



### <a name="creating-an-azure-ad-test-user"></a>Azure AD-testikäyttäjän luominen
Tässä osassa tavoitteena on luotava testikäyttäjän kutsutaan Britta Simon perinteinen portaalissa.

![Luo Azure AD-käyttäjä][20]

**Voit luoda testikäyttäjän Azure AD-toimimalla seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen Portal**, valitse vasemmassa siirtymisruudussa.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_09.png)

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Ylä-valikossa käyttäjäluettelon, näkyviin napsauttamalla **käyttäjät**.
    
    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_03.png)

4. Avaa **Lisää käyttäjä** -valintaikkunan alareunassa työkalurivillä, valitse **Lisää käyttäjä**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_04.png)

5. Tee valintaikkunassa **Kerro kyseisen käyttäjän tietoja** -sivulla seuraavasti:

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_05.png)

    a. Käyttäjänä tyyppi Valitse uusi käyttäjä organisaatiossa.

    b. Kirjoita käyttäjänimi- **tekstiruutuun** **BrittaSimon**.

    c-näppäinyhdistelmää. Valitse **Seuraava**.

6.  Suorita **Käyttäjäprofiili** -sivulla seuraavat toimet:
    
    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_06.png)

    a. Kirjoita **Ensimmäinen nimi** -tekstiruutuun **Britta**.  

    b. Kirjoita **Sukunimi** -tekstiruutuun tyyppi- **Simon**.

    c-näppäinyhdistelmää. Kirjoita **Näyttönimi** -tekstiruutuun **Britta Simon**.

    d. Valitse **rooli** -luettelosta **käyttäjä**.

    e. Valitse **Seuraava**.

7. Valitse **Hae tilapäinen salasana** -valintaikkunan sivulla **Luo**.
    
    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_07.png)

8. **Hae tilapäinen salasana** -valintaikkunan sivulla toimimalla seuraavasti:
    
    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/create_aaduser_08.png)

    a. Kirjoita **Uusi salasana**arvo muistiin.

    b. Valitse **Valmis**.   



### <a name="creating-a-wizergos-productivity-software-test-user"></a>Wizergos tuottavuutta ohjelmiston testikäyttäjän luominen

Tässä osassa nimeltä Britta Simon Wizergos tuottavuutta ohjelmiston käyttäjän luominen. Ota käsitteleminen Wizergos tuottavuus ohjelmisto tukihenkilöstön kautta [support@wizergos.com](emailTo:support@wizergos.com) voit lisätä käyttäjiä Wizergos tuottavuus ohjelmisto-ympäristössä.


### <a name="assigning-the-azure-ad-test-user"></a>Azure AD-testikäyttäjän määrittäminen

Tässä osassa tavoitteena on Britta Simon käyttämään Azure kertakirjautumisen hänen käyttöoikeuden myöntämisestä Wizergos tuottavuutta ohjelmiston ottaminen käyttöön.
    
   ![Määrittää käyttäjälle][200]

**Jos haluat määrittää Britta Simon Wizergos tuottavuutta ohjelmiston, toimi seuraavasti:**

1. Perinteinen-portaalissa Avaa sovellukset-näkymässä kansio-näkymässä, valitse **sovellukset** yläreunan-valikossa.
    
    ![Määrittää käyttäjälle][201]

2. Valitse sovellukset-luettelosta **Wizergos tuottavuutta ohjelmiston**.
    
    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_wizergosproductivitysoftware_50.png)

1. Valitse ylä-valikossa Valitse **käyttäjiä**.
    
    ![Määrittää käyttäjälle][203]

1. Valitse käyttäjät-luettelosta **Britta Simon**.

2. Napsauta työkalurivin alaosassa **määrittäminen**.
    
    ![Määrittää käyttäjälle][205]



### <a name="testing-single-sign-on"></a>Kertakirjautumisen testaaminen

Tässä osassa tavoitteena on Testaa Azure AD yksittäisen Sign-asetusten käyttäminen Access-paneeli.
 
Access-ruudussa Wizergos tuottavuutta ohjelmisto-ruutua napsauttaessasi sinun olisi Hae automaattisesti kirjautunut käyttöön Wizergos tuottavuutta ohjelmistosovellus.


## <a name="additional-resources"></a>Lisäresursseja

* [Luettelo siitä, miten voit integroida SaaS sovellusten Azure Active Directory-opetusohjelmat](active-directory-saas-tutorial-list.md)
* [Mikä on sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-wizergosproductivitysoftware-tutorial/tutorial_general_205.png
