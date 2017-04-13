<properties
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi OpsGenie | Microsoft Azure"
    description="Opettele määrittämään kertakirjautumisen Azure Active Directory- ja OpsGenie välillä."
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
    ms.date="10/07/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-opsgenie"></a>Opetusohjelma: OpsGenie Azure Active Directory-integrointi

Tässä opetusohjelmassa tavoitteena on, kuinka voit integroida OpsGenie Azure Active Directory (Azure AD).

Azure AD-integraation OpsGenie avulla voit seuraavat edut:

- Voit hallita Azure AD, jolla on pääsy OpsGenie
- Voit ottaa käyttöön automaattisesti Hae kirjautunut käyttöön OpsGenie (kertakirjautumisen) niiden Azure AD-tilien käyttäjille
- Voit hallita asiakkaiden yhdessä keskitetyssä sijainnissa - Azure perinteinen portal

Jos haluat tietää tarkempia tietoja SaaS sovelluksen integrointi Azure AD-kohdassa [mitä sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Edellytykset

Määrittää Azure AD-integroinnin OpsGenie, tarvitset seuraavat:

- Azure AD-tilaus
- Käytössä OpsGenie kertakirjautumisen tilaus


> [AZURE.NOTE] Voit esikatsella vaiheet Tässä opetusohjelmassa, Emme suosittele käyttämällä tuotantoympäristössä.


Voit esikatsella vaiheet Tässä opetusohjelmassa-noudattamalla näitä suosituksia:

- Älä käytä tuotantoympäristössä, ellei se on tarpeen.
- Jos sinulla ei ole kokeiluversion Azure AD-ympäristössä, voit hankkia yksi kuukausi kokeiluversion [tähän](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Skenaarion kuvaus
Tässä opetusohjelmassa tavoitteena on, jotta voit testata Azure AD kertakirjautumisen testiympäristössä. 

Tässä opetusohjelmassa kuvatut skenaarion kuuluu kaksi tärkeimmät rakenneosien:

1. OpsGenie lisääminen valikoimasta
2. Yksittäinen määrittäminen ja testaus Azure AD-on


## <a name="adding-opsgenie-from-the-gallery"></a>OpsGenie lisääminen valikoimasta
Voit määrittää OpsGenie integroida Azure AD-haluat lisätä OpsGenie valikoimasta hallitun SaaS sovellusluettelo.

**Jos haluat lisätä OpsGenie valikoimasta, toimi seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen portal**, valitse vasemmassa siirtymisruudussa. 

    ![Active Directory][1]

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset][2]

4. Valitse sivun alareunassa **Lisää** .

    ![Sovellukset][3]

5. Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Sovellukset][4]

6. Kirjoita hakuruutuun **OpsGenie**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_01.png)

7. Valitse tulosruudussa **OpsGenie**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_02.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yksittäinen määrittäminen ja testaus Azure AD-on
Tässä osassa tavoitteena on noudattamalla voit määrittää ja testaa Azure AD kertakirjautumisen OpsGenie nimeltä "Britta Simon" testikäyttäjän perusteella.

Kertakirjautumisen toimimaan, saat Azure AD on tiedettävä OpsGenie vastaavaan käyttäjän käyttäjälle Azure AD. Toisin sanoen suhteen Azure AD-käyttäjä ja Aiheeseen liittyvät OpsGenie-linkki on vahvistettava.

Linkki-yhteys on muodostettu määrittämällä **käyttäjänimi** arvo arvoksi OpsGenie **käyttäjänimi** Azure AD.

Määrittäminen ja testaaminen Azure AD kertakirjautumisen OpsGenie kanssa, sinun on tehtävä seuraavat hakukyselyn:

1. **[Azure AD määrittäminen Single Sign](#configuring-azure-ad-single-single-sign-on)** - käyttäjät voivat käyttää tätä toimintoa.
2. **[Käyttäjän testata luominen Azure AD](#creating-an-azure-ad-test-user)** - Testaa Azure AD kertakirjautumisen Britta Simon kanssa.
4. **[Luomisesta OpsGenie testata käyttäjän](#creating-a-opsgenie-test-user)** - tapahtumista Britta Simon ovat OpsGenie, jotka on linkitetty hän Azure AD esitys.
5. **[Käyttäjän testata määrittäminen Azure AD](#assigning-the-azure-ad-test-user)** - Britta Simon käyttämään Azure AD kertakirjautumisen käyttöön.
5. **[Testaus Single Sign](#testing-single-sign-on)** - voit tarkistaa, toimiiko määritykset.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD kertakirjautumisen määrittäminen

Tässä osassa tavoitteena on Azure AD kertakirjautumisen Azure perinteinen portaalissa ottaminen käyttöön ja määrittää kertakirjautumisen OpsGenie-sovelluksessa.



**Määrittää Azure AD kertakirjautumisen OpsGenie, toimimalla seuraavasti:**

1. Valitse Azure perinteinen portaalissa integrointi **OpsGenie** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen Single Sign** -valintaikkunan avaaminen.

    ![Kertakirjautumisen määrittäminen][6] 

2. **Miten käyttäjät voivat kirjautua OpsGenie haluat käyttää** sivulla Valitse **Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_03.png) 

3. Valitse **Määritä sovelluksen asetukset** -sivulla toimimalla seuraavasti:

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_04.png) 


    a. Kirjoita merkki-URL-tekstiruutuun URL-osoite käyttämä Sign käyttämällä seuraavaa kaavaa OpsGenie sovelluksen käyttäjille: **"https://app.opsgenie.com/auth/login"**.

    > [AZURE.NOTE] Ota yhteyttä oman [OpsGenie tekniseen tukeen](mailto:support@opsgenie.com) tarvittaessa Kirjaudu-URL-osoite.

    b. Valitse **Seuraava**.


4. Valitse **Määritä kertakirjautumisen osoitteessa OpsGenie** -sivulla toimimalla seuraavasti:

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_05.png) 

    a. Valitse **Lataa sertifikaatti**ja tallenna sitten tiedosto tietokoneesta. Tämä varmenne ja metatietojen URL-osoitteet (kohteen tunnus, kirjaudu SSO URL- ja kirjaudu ulos URL-osoite) määrittämään SSO OpsGenie reunassa annettava.

    b. Valitse **Seuraava**.


5. Avaa toisen selain ja sitten sisään järjestelmänvalvojana OpsGenie avulla.

6. Valitse **asetukset**ja valitse sitten **Kertakirjautuminen** -välilehti.
 
    ![OpsGenie Single Sign-On](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_06.png) 

7. Jotta Kertakirjautumista valitsemalla **käytössä**.

    ![OpsGenie asetukset](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_07.png) 
   
8. Valitse **palvelu** -osassa **Azure Active Directory** -välilehti.

    ![OpsGenie asetukset](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_08.png) 
    
9. Suorita Azure Active Directory-sivulla seuraavat toimet:
 
    ![OpsGenie asetukset](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_09.png) 

    a. Azure perinteinen portaalissa, valitse **Määritä kertakirjautumisen osoitteessa OpsGenie** -valintasivun **Yksittäinen merkki-palvelun URL-osoite** -arvo kopioi ja liitä se sitten **SAML 2.0 päätepisteen** tekstiruutu.

    b. Luo base-64-koodattu tiedosto ladattu varmennetta.      
    
    > [AZURE.NOTE] Lisätietoja on artikkelissa [muuntaa binaariluvun varmenteen tekstitiedostoon](https://www.youtube.com/watch?v=PlgrzUZ-Y1o&feature=youtu.be).

    c-näppäinyhdistelmää. Avaa base-64-koodattu varmennetta Muistiossa, kopioi Leikepöydän sisällön se ja liitä se **X.500 varmenteen** tekstiruutu.

    d. Valitse **Tallenna muutokset**.


6. Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Seuraava**.

    ![Azure AD-Single Sign-On][10]

7. Valitse **Sign Vahvista** -sivulla **Valmis**.  

    ![Azure AD-Single Sign-On][11]




### <a name="creating-an-azure-ad-test-user"></a>Azure AD-testikäyttäjän luominen
Tässä osassa tavoitteena on luotava testikäyttäjän kutsutaan Britta Simon Azure perinteinen portaalissa.



![Luo Azure AD-käyttäjä][20]

**Voit luoda testikäyttäjän Azure AD-toimimalla seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen Portal**, valitse vasemmassa siirtymisruudussa.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-opsgenie-tutorial/create_aaduser_09.png) 

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Ylä-valikossa käyttäjäluettelon, näkyviin napsauttamalla **käyttäjät**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-opsgenie-tutorial/create_aaduser_03.png) 

4. Avaa **Lisää käyttäjä** -valintaikkunan alareunassa työkalurivillä, valitse **Lisää käyttäjä**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-opsgenie-tutorial/create_aaduser_04.png) 

5. Tee valintaikkunassa **Kerro kyseisen käyttäjän tietoja** -sivulla seuraavasti:

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-opsgenie-tutorial/create_aaduser_05.png) 

    a. Käyttäjänä tyyppi Valitse uusi käyttäjä organisaatiossa.

    b. Kirjoita käyttäjänimi- **tekstiruutuun** **BrittaSimon**.

    c-näppäinyhdistelmää. Valitse **Seuraava**.

6.  Suorita **Käyttäjäprofiili** -sivulla seuraavat toimet:

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-opsgenie-tutorial/create_aaduser_06.png) 

    a. Kirjoita **Ensimmäinen nimi** -tekstiruutuun **Britta**.  

    b. Kirjoita **Sukunimi** -tekstiruutuun tyyppi- **Simon**.

    c-näppäinyhdistelmää. Kirjoita **Näyttönimi** -tekstiruutuun **Britta Simon**.

    d. Valitse **rooli** -luettelosta **käyttäjä**.

    e. Valitse **Seuraava**.

7. Valitse **Hae tilapäinen salasana** -valintaikkunan sivulla **Luo**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-opsgenie-tutorial/create_aaduser_07.png) 

8. **Hae tilapäinen salasana** -valintaikkunan sivulla toimimalla seuraavasti:

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-opsgenie-tutorial/create_aaduser_08.png) 

    a. Kirjoita **Uusi salasana**arvo muistiin.

    b. Valitse **Valmis**.   



### <a name="creating-a-opsgenie-test-user"></a>OpsGenie testikäyttäjän luominen

Tässä osassa tavoitteena on nimeltään Britta Simon OpsGenie käyttäjän luominen. 

1.  Web-selainikkunassa Kirjaudu sisään OpsGenie-asiakasympäristöön järjestelmänvalvojana.

2.  Siirry Käyttäjät luettelosta valitsemalla **käyttäjän** vasemmassa paneelissa.
   
    ![OpsGenie asetukset](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_10.png) 

3.  Valitsemalla "**Lisää käyttäjä**".

3.  Valitse **Lisää käyttäjä** -valintaikkunassa seuraavasti:

    ![OpsGenie asetukset](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_11.png) 

    a. Kirjoita **sähköpostiosoite** -tekstiruutuun Azure Active Directoryn Britta henkilön sähköpostiosoite.

    b. Kirjoita **KokoNimi** -tekstiruutuun **Britta Simon**.

    c-näppäinyhdistelmää. Valitse **Tallenna**. 

Britta saavat sähköpostiviesti, jossa hänen profiilin määrittämisestä.


### <a name="assigning-the-azure-ad-test-user"></a>Azure AD-testikäyttäjän määrittäminen

Tässä osassa tavoitteena on Britta Simon käyttämään Azure kertakirjautumisen hänen käyttöoikeuden myöntämisestä OpsGenie ottaminen käyttöön.

![Määrittää käyttäjälle][200] 

**Jos haluat määrittää Britta Simon OpsGenie, toimi seuraavasti:**

1. Azure perinteinen-portaalissa Avaa sovellukset-näkymässä kansio-näkymässä, valitse **sovellukset** yläreunan-valikossa.
 
    ![Määrittää käyttäjälle][201] 

2. Valitse sovellukset-luettelosta **OpsGenie**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_50.png) 

1. Valitse ylä-valikossa Valitse **käyttäjiä**.

    ![Määrittää käyttäjälle][203] 

1. Valitse käyttäjät-luettelosta **Britta Simon**.

2. Napsauta työkalurivin alaosassa **määrittäminen**.

    ![Määrittää käyttäjälle][205]



### <a name="testing-single-sign-on"></a>Kertakirjautumisen testaaminen

Tässä osassa tavoitteena on Testaa Azure AD yksittäisen Sign-asetusten käyttäminen Access-paneeli.

Access-ruudussa OpsGenie-ruutua napsauttaessasi sinun pitäisi saada automaattisesti kirjautunut-on OpsGenie sovelluksen.


## <a name="additional-resources"></a>Lisäresursseja

* [Luettelo siitä, miten voit integroida SaaS sovellusten Azure Active Directory-opetusohjelmat](active-directory-saas-tutorial-list.md)
* [Mikä on sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_205.png
