<properties
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi 8 x 8 Virtual Office | Microsoft Azure"
    description="Opettele määrittämään kertakirjautumisen Azure Active kansioon ja 8 x 8 Virtual Office välillä."
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


# <a name="tutorial-azure-active-directory-integration-with-8x8-virtual-office"></a>Opetusohjelma: 8 x 8 Virtual Office Azure Active Directory-integrointi

Tässä opetusohjelmassa tavoitteena on, kuinka voit integroida 8 x 8 Virtual Office Azure Active Directory (Azure AD).

8 x 8 integrointi Virtual kanssa Azure AD Officessa on seuraavat edut:

- Voit hallita Azure AD, joilla on käyttöoikeus 8 x 8 Virtual Officeen
- Voit ottaa käyttöön automaattisesti Hae kirjautunut sisään 8 x 8 Virtual Office (kertakirjautumisen) käyttäjille heidän Azure AD-tilien kanssa
- Voit hallita asiakkaiden yhdessä keskitetyssä sijainnissa - Azure perinteinen portal

Jos haluat tietää tarkempia tietoja SaaS sovelluksen integrointi Azure AD-kohdassa [mitä sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Edellytykset

Azure AD-integroinnin määrittäminen 8 x 8 Virtual Office, tarvitset seuraavat:

- Azure AD-tilaus
- 8 x 8 Virtual Office single-merkki käytössä-tilauksessa


> [AZURE.NOTE] Voit esikatsella vaiheet Tässä opetusohjelmassa, Emme suosittele käyttämällä tuotantoympäristössä.


Voit esikatsella vaiheet Tässä opetusohjelmassa-noudattamalla näitä suosituksia:

- Älä käytä tuotantoympäristössä, ellei se on tarpeen.
- Jos sinulla ei ole kokeiluversion Azure AD-ympäristössä, voit hankkia yksi kuukausi kokeiluversion [tähän](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Skenaarion kuvaus
Tässä opetusohjelmassa tavoitteena on, jotta voit testata Microsoft Azure AD kertakirjautumisen testiympäristössä.

Tässä opetusohjelmassa kuvatut skenaarion kuuluu kaksi tärkeimmät rakenneosien:

1. 8 x 8 Virtual Office lisääminen valikoimasta
2. Määrittäminen ja testaus Microsoft Azure AD Single Sign-On


## <a name="adding-8x8-virtual-office-from-the-gallery"></a>8 x 8 Virtual Office lisääminen valikoimasta
Voit määrittää 8 x 8 Virtual Office integroida Azure AD-haluat lisätä 8 x 8 Virtual Office valikoimasta hallitun SaaS sovellusluettelo.

**Lisää 8 x 8 Virtual Office valikoimasta, toimi seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen Portal**, valitse vasemmassa siirtymisruudussa.

    ![Active Directory][1]

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .
    
    ![Sovellukset][2]

4. Valitse sivun alareunassa **Lisää** .

    ![Sovellukset][3]

5. Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Sovellukset][4]

6. Kirjoita hakuruutuun **8 x 8 Virtual Office**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_01.png)
7. Valitse tulokset-ruudun kohdassa **8 x 8 Virtual Office**ja valitse sitten **Valmis** , voit lisätä sovelluksen.

    ![Sovelluksen valitseminen valikoimasta](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_0001.png)


##  <a name="configuring-and-testing-microsoft-azure-ad-single-sign-on"></a>Määrittäminen ja testaus Microsoft Azure AD Single Sign-On
Tässä osassa tavoitteena on kuinka määrittäminen ja testaaminen Microsoft Azure AD kertakirjautumisen 8 x 8 Virtual Office perusteella nimeltä "Britta Simon" testikäyttäjän kanssa.

Kertakirjautumisen toimimaan Azure AD on tietää, mitä vastaavaan käyttäjän 8 x 8 Virtual Office Azure AD-käyttäjälle. Toisin sanoen linkki suhteen Azure AD-käyttäjä ja Aiheeseen liittyvät 8 x 8 Virtual Office on vahvistettava.

Linkki-yhteys on muodostettu määrittämällä **käyttäjänimi** arvo arvoksi **käyttäjänimi** 8 x 8 Virtual Office Azure AD.

Määrittäminen ja testaaminen Microsoft Azure AD kertakirjautumisen 8 x 8 Virtual Office-sinun on tehtävä seuraavat hakukyselyn:

1. **[Asetusten määrittäminen Microsoft Azure AD Single Sign](#configuring-azure-ad-single-single-sign-on)** - käyttäjät voivat käyttää tätä toimintoa.
2. **[Käyttäjän testata luominen Azure AD](#creating-an-azure-ad-test-user)** - Testaa Microsoft Azure AD kertakirjautumisen Britta Simon kanssa.
3. **[8 x 8 Virtual Office testikäyttäjän luominen](#creating-a-8x8-virtual-office-test-user)** - tapahtumista Britta Simon on 8 x 8 Virtual Office kyseistä henkilöä hän Azure AD esitys.
4. **[Käyttäjän testata määrittäminen Azure AD](#assigning-the-azure-ad-test-user)** - Britta Simon käyttämään Microsoft Azure AD kertakirjautumisen käyttöön.
5. **[Testaus Single Sign](#testing-single-sign-on)** - voit tarkistaa, toimiiko määritykset.

### <a name="configuring-microsoft-azure-ad-single-sign-on"></a>Microsoft Azure AD-Single Sign-On määrittäminen

Tässä osassa Ota käyttöön Microsoft Azure AD kertakirjautumisen perinteinen portaalissa ja määritä kertakirjautumisen 8 x 8 Virtual Office-sovelluksessa.

**Voit määrittää Microsoft Azure AD kertakirjautumisen 8 x 8 Virtual Office toimimalla seuraavasti:**

1. Valitse perinteinen-portaalissa **8 x 8 Virtual Office** -sovellus integrointi-sivulla **Määritä kertakirjautumisen** **Määrittäminen Single Sign** -valintaikkunan avaaminen.
     
    ![Kertakirjautumisen määrittäminen][6] 

2. Valitse **kuinka haluat käyttäjien kirjautua sisään 8 x 8 Virtual Office** -sivulla **Microsoft Azure AD kertakirjautumisen**Valitse ja valitse sitten **Seuraava**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_03.png) 

3. Valitse **Määritä sovelluksen asetukset** -sivulla suorittamalla seuraavat vaiheet ja valitse **Seuraava**:

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_04.png)

    a. Kirjoita **Vastaus URL** -tekstiruutuun:`https://sso.8x8.com/saml2`

    b. Valitse **Seuraava**

4. Seuraavien toimien **kertakirjautumisen kohdassa 8 x 8 Virtual Office määrittäminen** -sivulla ja valitse **Seuraava**:

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_05.png)

    a. Valitse **Lataa**ja tallenna sitten tiedosto tietokoneesta.

    b. Valitse **Seuraava**.

5. Kertakirjauksen 8 x 8 Virtual Office-asiakasympäristöön järjestelmänvalvojana.
6. Valitse **tilin muuttaa Virtual Office** -sovelluksen ruutu.

    ![Sovelluksen reunassa määrittäminen](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_001.png)

7. Valitse **yrityksen** tilin hallinta ja valitse **Kirjaudu sisään** -painike.

    ![Sovelluksen reunassa määrittäminen](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_002.png)

8. Valitse **tilit** -välilehti-valikon luettelossa.

    ![Sovelluksen reunassa määrittäminen](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_003.png)

9. Valitse tililuettelon **Kertakirjautuminen** .

    ![Sovelluksen reunassa määrittäminen](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_004.png)

10. Valitse **Signle Sign** todentamismenetelmän-kohdassa ja valitse **SAML**.

    ![Sovelluksen reunassa määrittäminen](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_005.png)

11. Kopioi **SAML SSO**, **yksi hyvinkin muuttaa mielesi ulos palvelun URL-osoite** ja **myöntäjä URL-Osoitteen** Azure AD **kirjautumaan URL-osoite**, **Kirjaudu ulos URL-osoite** ja 8 x 8 Virtual Office **myöntäjä URL-osoite** . 

    ![Sovelluksen reunassa määrittäminen](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_006.png)

    ![Sovelluksen reunassa määrittäminen](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_007.png)

12. Napsauta **selaimen** Lataa sertifikaatti, jonka latasit Azure AD-painiketta.

13. Napsauta **Tallenna** -painiketta.

14. Perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Seuraava**.

    ![Azure AD-Single Sign-On][10]

15. Valitse **Sign Vahvista** -sivulla **Valmis**.  

    ![Azure AD-Single Sign-On][11]



### <a name="creating-an-azure-ad-test-user"></a>Azure AD-testikäyttäjän luominen
Tässä osassa tavoitteena on luotava testikäyttäjän kutsutaan Britta Simon perinteinen portaalissa.
    
![Luo Azure AD-käyttäjä][20]

**Voit luoda testikäyttäjän Azure AD-toimimalla seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen Portal**, valitse vasemmassa siirtymisruudussa.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_09.png)

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Ylä-valikossa käyttäjäluettelon, näkyviin napsauttamalla **käyttäjät**.
    
    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_03.png)

4. Avaa **Lisää käyttäjä** -valintaikkunan alareunassa työkalurivillä, valitse **Lisää käyttäjä**.
    
    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_04.png)

5. Tee valintaikkunassa **Kerro kyseisen käyttäjän tietoja** -sivulla seuraavasti:

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_05.png)

    a. Käyttäjänä tyyppi Valitse uusi käyttäjä organisaatiossa.

    b. Kirjoita käyttäjänimi- **tekstiruutuun** **BrittaSimon**.

    c-näppäinyhdistelmää. Valitse **Seuraava**.

6.  Suorita **Käyttäjäprofiili** -sivulla seuraavat toimet:
    
    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_06.png)

    a. Kirjoita **Ensimmäinen nimi** -tekstiruutuun **Britta**.  

    b. Kirjoita **Sukunimi** -tekstiruutuun tyyppi- **Simon**.

    c-näppäinyhdistelmää. Kirjoita **Näyttönimi** -tekstiruutuun **Britta Simon**.

    d. Valitse **rooli** -luettelosta **käyttäjä**.

    e. Valitse **Seuraava**.

7. Valitse **Hae tilapäinen salasana** -valintaikkunan sivulla **Luo**.
    
    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_07.png)

8. **Hae tilapäinen salasana** -valintaikkunan sivulla toimimalla seuraavasti:
    
    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-8x8virtualoffice-tutorial/create_aaduser_08.png)

    a. Kirjoita **Uusi salasana**arvo muistiin.

    b. Valitse **Valmis**.   



### <a name="creating-a-8x8-virtual-office-test-user"></a>8 x 8 Virtual Office testikäyttäjän luominen

Tässä osassa tavoitteena on nimeltään Britta Simon 8 x 8 Virtual Office käyttäjän luominen. 8 x 8 Virtual Office tukee vain perustuvan valmistelu, joka on oletusarvoisesti käytössä.

Ei ole toimi puolestasi sisältö. Uusi käyttäjä luodaan yritys käyttää 8 x 8 Virtual Office, jos sitä ei ole vielä yhteydessä. 

> [AZURE.NOTE] Jos haluat luoda luodun käyttäjän manuaalisesti, on 8 x 8 Virtual Office-tukeen.


### <a name="assigning-the-azure-ad-test-user"></a>Azure AD-testikäyttäjän määrittäminen

Tässä osassa tavoitteena on Britta Simon käyttämään Azure kertakirjautumisen hänen käyttöoikeuksien myöntämistä 8 x 8 Virtual Office ottaminen käyttöön.
    
![Määrittää käyttäjälle][200]

**Jos haluat määrittää Britta Simon 8 x 8 Virtual Office, toimi seuraavasti:**

1. Perinteinen-portaalissa Avaa sovellukset-näkymässä kansio-näkymässä, valitse **sovellukset** yläreunan-valikossa.

    ![Määrittää käyttäjälle][201]

2. Valitse sovellukset-luettelosta **8 x 8 Virtual Office**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_50.png)

3. Valitse ylä-valikossa Valitse **käyttäjiä**.
    
    ![Määrittää käyttäjälle][203]

4. Valitse käyttäjät-luettelosta **Britta Simon**.

5. Napsauta työkalurivin alaosassa **määrittäminen**.

    ![Määrittää käyttäjälle][205]



### <a name="testing-single-sign-on"></a>Kertakirjautumisen testaaminen

Tässä osassa tavoitteena on Testaa Microsoft Azure AD kertakirjautumisen asetusten käyttäminen Access-paneeli.

Kun napsautat Access-ruudussa 8 x 8 Virtual Office-kuvaketta, voit pitäisi saada automaattisesti kirjautunut-on 8 x 8 Virtual Office-sovellukseen.


## <a name="additional-resources"></a>Lisäresursseja

* [Luettelo siitä, miten voit integroida SaaS sovellusten Azure Active Directory-opetusohjelmat](active-directory-saas-tutorial-list.md)
* [Mikä on sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-8x8virtualoffice-tutorial/tutorial_general_205.png
