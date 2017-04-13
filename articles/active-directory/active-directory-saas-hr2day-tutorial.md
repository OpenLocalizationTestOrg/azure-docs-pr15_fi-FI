<properties
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi HR2day mukaan Merces | Microsoft Azure"
    description="Opettele määrittämään kertakirjautumisen Azure Active Directory- ja HR2day mukaan Merces välillä."
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


# <a name="tutorial-azure-active-directory-integration-with-hr2day-by-merces"></a>Opetusohjelma: HR2day mukaan Merces Azure Active Directory-integrointi

Tässä opetusohjelmassa tavoitteena on, kuinka voit integroida HR2day mukaan Merces Azure Active Directory (Azure AD).  
Azure AD-integraation HR2day Merces mukaan, jonka avulla seuraavat edut:

- Voit hallita Azure AD, jolla on pääsy HR2day Merces mukaan
- Voit ottaa käyttöön automaattisesti Hae kirjautunut käyttöön HR2day Merces (kertakirjautumisen) mukaan niiden Azure AD-tilien käyttäjille
- Voit hallita asiakkaiden yhdessä keskitetyssä sijainnissa - Azure perinteinen portal

Jos haluat tietää tarkempia tietoja SaaS sovelluksen integrointi Azure AD-kohdassa [mitä sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Edellytykset

Määrittää Azure AD-integroinnin HR2day Merces mukaan, tarvitset seuraavat:

- Azure AD-tilaus
- HR2day Merces single-merkki käytössä tilauksen mukaan


> [AZURE.NOTE] Voit esikatsella vaiheet Tässä opetusohjelmassa, Emme suosittele käyttämällä tuotantoympäristössä.


Voit esikatsella vaiheet Tässä opetusohjelmassa-noudattamalla näitä suosituksia:

- Älä käytä tuotantoympäristössä, ellei se on tarpeen.
- Jos sinulla ei ole kokeiluversion Azure AD-ympäristössä, voit hankkia yksi kuukausi kokeiluversion [tähän](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Skenaarion kuvaus
Tässä opetusohjelmassa tavoitteena on, jotta voit testata Azure AD kertakirjautumisen testiympäristössä.  
Tässä opetusohjelmassa kuvatut skenaarion kuuluu kaksi tärkeimmät rakenneosien:

1. HR2day mukaan Merces lisääminen valikoimasta
2. Yksittäinen määrittäminen ja testaus Azure AD-on


## <a name="adding-hr2day-by-merces-from-the-gallery"></a>HR2day mukaan Merces lisääminen valikoimasta
Voit määrittää HR2day mukaan Merces integroida Azure AD-haluat lisätä HR2day mukaan Merces valikoimasta hallitun SaaS sovellusluettelo.

**Voit lisätä HR2day mukaan Merces valikoimasta, toimi seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen portal**, valitse vasemmassa siirtymisruudussa. 

    ![Active Directory][1]

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .
 
    ![Sovellukset][2]

4. Valitse sivun alareunassa **Lisää** .

    ![Sovellukset][3]

5. Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Sovellukset][4]

6. Kirjoita hakuruutuun **HR2day Merces mukaan**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2day_01.png)

7. Valitse tulosruudussa **HR2day Merces mukaan**ja valitse **Valmis** , voit lisätä sovelluksen.


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yksittäinen määrittäminen ja testaus Azure AD-on
Tässä osassa tavoitteena on kuinka määrittäminen ja testaaminen Azure AD kertakirjautumisen kanssa HR2day nimeltä "Britta Simon" testi käyttäjään perustuva Merces mukaan.

Kertakirjautumisen toimimaan Azure AD on tiedettävä vastaavaan käyttäjän HR2day mukaan Merces, Azure AD-käyttäjälle. Toisin sanoen suhteen Azure AD-käyttäjä ja Aiheeseen liittyvät HR2day mukaan Merces-linkki on vahvistettava.  
Linkki-yhteys on muodostettu määrittämällä **käyttäjänimi** arvo arvoksi HR2day mukaan Merces **käyttäjänimi** Azure AD.

Määrittäminen ja testaaminen Azure AD kertakirjautumisen HR2day mukaan Merces kanssa, sinun on tehtävä seuraavat hakukyselyn:

1. **[Azure AD määrittäminen Single Sign](#configuring-azure-ad-single-single-sign-on)** - käyttäjät voivat käyttää tätä toimintoa.
2. **[Käyttäjän testata luominen Azure AD](#creating-an-azure-ad-test-user)** - Testaa Azure AD kertakirjautumisen Britta Simon kanssa.
4. **[Käyttäjän luominen HR2day mukaan Merces testata](#creating-a-hr2day-by-merces-test-user)** - on tapahtumista Britta Simon HR2day Merces, jotka on linkitetty hän Azure AD-esittäminen mukaan.
5. **[Käyttäjän testata määrittäminen Azure AD](#assigning-the-azure-ad-test-user)** - Britta Simon käyttämään Azure AD kertakirjautumisen käyttöön.
5. **[Testaus Single Sign](#testing-single-sign-on)** - voit tarkistaa, toimiiko määritykset.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD-Single Sign-On määrittäminen

Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjien todentamiseen HR2day Merces mukaan käyttämällä SAML-protokollan perusteella federation Azure AD-tilin kanssa.

Oman HR2day Merces sovelluksen odottaa SAML vahvistukset tietyssä muodossa, jossa pyydetään mukautettujen määritteiden yhdistämismäärityksiä lisääminen SAML suojaustunnuksen määritteet-määritys. Seuraavassa näyttökuvassa näkyy esimerkki tämän. 

![Kertakirjautumisen määrittäminen](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2day_00.png) 

Ennen kuin voit määrittää SAML vahvistus, sinun on tukihenkilöstöltä HR2day kautta [servicedesk@merces.nl](mailto:servicedesk@merces.nl) ja pyydä vuokraajan yksilöllinen tunnus-määritteen arvon. Tarvitset tätä arvoa seuraavan osion vaiheiden suorittamiseen.


**Määrittää Azure AD kertakirjautumisen HR2day Merces mukaan, toimimalla seuraavasti:**

1. Valitse Azure perinteinen portaalissa **HR2day Merces mukaan** sovelluksen integrointi-sivun yläreunassa valikosta **SAML suojaustunnuksen määritteet** -valintaikkunan avaaminen **määritteet** . 

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2day_06.png) 

2. Pakollinen määrite yhdistämismääritykset-ratkaisussa seuraavien toimien, toimimalla seuraavasti: 

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2day_07.png) 


    a. Valitse **Lisää käyttäjä-määrite**.

    b. Kirjoita **"ATTR_LOGINCLAIM"** **Määritteen nimi** -tekstiruutuun.

    c-näppäinyhdistelmää. **Määritteen arvo** -luettelosta **Join()**. 

    d. **Merkkijono1** -luettelosta **User.mail**. 

    e. Kirjoita **yksilöllinen** HR2day-saamiesi **merkkijono2** -tekstiruutuun. 

    f. Kirjoita **erotin** -tekstiruutuun **@**.

    g. Valitse **Valmis**.

  
3. Valitse **Ota muutokset käyttöön**.


1. Valitse ylä-valikossa **Pika-aloitus** -valintaikkunan avaaminen **Pika-aloitus** .

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-hr2day-tutorial/tutorial_general_08.png) 



1. Valitse **Määritä kertakirjautumisen** **Määrittäminen Single Sign** -valintaikkunan avaaminen.

    ![Kertakirjautumisen määrittäminen][6] 

2. **Miten käyttäjät voivat kirjautua HR2day Merces mukaan haluat käyttää** sivulla Valitse **Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2day_03.png) 

3. Valitse **Määritä sovelluksen asetukset** -sivulla toimimalla seuraavasti: 

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2day_04.png) 


    a. Kirjoita merkki-URL-tekstiruutuun URL-osoite käyttämä kertakirjauksen käyttöön oman HR2day käyttäjille Merces sovelluksen käyttämällä seuraavaa kaavaa: **"https://\<vuokraajan nimi\>.force.com/\<esiintymänimi\>"**.

    b. Valitse **Seuraava**.

4. Valitse **Määritä kertakirjautumisen osoitteessa HR2day Merces mukaan** -sivulla toimimalla seuraavasti:

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2day_05.png) 

    a. Valitse **Lataa**ja tallenna sitten tiedosto tietokoneesta.

    b. Valitse **Seuraava**.


5. Saat määritetty sovelluksen SSO-yhteyttä oman HR2day Merces tukihenkilöstön kautta [servicedesk@merces.nl](emailTo:servicedesk@merces.nl) ja ladatut sertifikaattitiedosto liittäminen sähköpostiin. Myös Anna SAML SSO URL-osoite, kirjaudu ulos URL-osoite ja myöntäjä URL-Osoitteen niin, että ne on määritetty SSO-integrointia varten.


> [AZURE.NOTE] Ota mainita Merces työryhmälle integrointi on kohteen tunnus, voit määrittää tämän mallin **https://hr2day.force.com/INSTANCENAME** kanssa



6. Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Seuraava**.

    ![Azure AD-Single Sign-On][10]

7. Valitse **Sign Vahvista** -sivulla **Valmis**.  

    ![Azure AD-Single Sign-On][11]



### <a name="creating-an-azure-ad-test-user"></a>Azure AD-testikäyttäjän luominen
Tässä osassa tavoitteena on luotava testikäyttäjän kutsutaan Britta Simon Azure perinteinen portaalissa.  

![Luo Azure AD-käyttäjä][20]

**Voit luoda testikäyttäjän Azure AD-toimimalla seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen portal**, valitse vasemmassa siirtymisruudussa.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-hr2day-tutorial/create_aaduser_09.png) 

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Ylä-valikossa käyttäjäluettelon, näkyviin napsauttamalla **käyttäjät**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-hr2day-tutorial/create_aaduser_03.png) 

4. Avaa **Lisää käyttäjä** -valintaikkunan alareunassa työkalurivillä, valitse **Lisää käyttäjä**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-hr2day-tutorial/create_aaduser_04.png) 

5. Tee valintaikkunassa **Kerro kyseisen käyttäjän tietoja** -sivulla seuraavasti:

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-hr2day-tutorial/create_aaduser_05.png) 

    a. Käyttäjänä tyyppi Valitse uusi käyttäjä organisaatiossa.

    b. Kirjoita käyttäjänimi- **tekstiruutuun** **BrittaSimon**.

    c-näppäinyhdistelmää. Valitse **Seuraava**.

6.  Suorita **Käyttäjäprofiili** -sivulla seuraavat toimet:

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-hr2day-tutorial/create_aaduser_06.png) 

    a. Kirjoita **Ensimmäinen nimi** -tekstiruutuun **Britta**.  

    b. Kirjoita **Sukunimi** -tekstiruutuun tyyppi- **Simon**.

    c-näppäinyhdistelmää. Kirjoita **Näyttönimi** -tekstiruutuun **Britta Simon**.

    d. Valitse **rooli** -luettelosta **käyttäjä**.

    e. Valitse **Seuraava**.

7. Valitse **Hae tilapäinen salasana** -valintaikkunan sivulla **Luo**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-hr2day-tutorial/create_aaduser_07.png) 

8. **Hae tilapäinen salasana** -valintaikkunan sivulla toimimalla seuraavasti:

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-hr2day-tutorial/create_aaduser_08.png) 

    a. Kirjoita **Uusi salasana**arvo muistiin.

    b. Valitse **Valmis**.   



### <a name="creating-a-hr2day-by-merces-test-user"></a>HR2day Merces testi käyttäjän luominen

Tässä osassa tavoitteena on Luo käyttäjä kutsutaan Britta Simon HR2day Merces mukaan. Ota tutustua kanssa HR2day Merces tukihenkilöstön voit lisätä käyttäjiä HR2day-tilille. 


> [AZURE.NOTE]Jos haluat luoda luodun käyttäjän manuaalisesti, on yhteyttä HR2day Merces tukihenkilöstön.


### <a name="assigning-the-azure-ad-test-user"></a>Azure AD-testikäyttäjän määrittäminen

Tässä osassa tavoitteena on ottaminen käyttöön Britta Simon käyttämään Azure kertakirjautumisen hänen käyttöoikeuden myöntämisestä HR2day Merces mukaan.

![Määrittää käyttäjälle][200] 

**Jos haluat määrittää Britta Simon HR2day Merces mukaan, toimi seuraavasti:**

1. Azure perinteinen-portaalissa Avaa sovellukset-näkymässä kansio-näkymässä, valitse **sovellukset** yläreunan-valikossa.

    ![Määrittää käyttäjälle][201] 

2. Valitse sovellukset-luettelosta **HR2day Merces mukaan**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2day_50.png) 

1. Valitse ylä-valikossa Valitse **käyttäjiä**.

    ![Määrittää käyttäjälle][203] 

1. Valitse käyttäjät-luettelosta **Britta Simon**.

2. Napsauta työkalurivin alaosassa **määrittäminen**.

    ![Määrittää käyttäjälle][205]



### <a name="testing-single-sign-on"></a>Kertakirjautumisen testaaminen

Tässä osassa tavoitteena on Testaa Azure AD yksittäisen Sign-asetusten käyttäminen Access-paneeli.  
Kun napsautat HR2day Merces ruutu Access-ruudussa, sinun kannattaa Hae automaattisesti kirjautunut käyttöön oman HR2day Merces sovelluksen.


## <a name="additional-resources"></a>Lisäresursseja

* [Luettelo siitä, miten voit integroida SaaS sovellusten Azure Active Directory-opetusohjelmat](active-directory-saas-tutorial-list.md)
* [Mikä on sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_205.png
