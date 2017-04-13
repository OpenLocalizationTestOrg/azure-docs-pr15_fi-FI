<properties
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi Cisco ohjattu | Microsoft Azure"
    description="Opettele määrittämään kertakirjautumisen Azure Active Directory ja Cisco ohjattu välillä."
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
    ms.date="10/20/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-cisco-spark"></a>Opetusohjelma: Cisco ohjattu Azure Active Directory-integrointi

Tässä opetusohjelmassa kerrotaan, miten voit integroida Cisco ohjattu Azure Active Directory (Azure AD).

Azure AD-integraation Cisco ohjattu avulla voit seuraavat edut:

- Voit hallita Azure AD, kenellä on oikeus Cisco ohjattu
- Voit ottaa käyttöön automaattisesti Hae kirjautunut käyttöön Cisco ohjattu (kertakirjautumisen) käyttäjille heidän Azure AD-tilien kanssa
- Voit hallita asiakkaiden yhdessä keskitetyssä sijainnissa - Azure perinteinen portal

Jos haluat tietää tarkempia tietoja SaaS sovelluksen integrointi Azure AD-kohdassa [mitä sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Edellytykset

Määrittää Azure AD-integroinnin Cisco ohjattu, tarvitset seuraavat:

- Azure AD-tilaus
- **Cisco ohjattu** single-merkki käytössä-tilauksessa


> [AZURE.NOTE] Voit esikatsella vaiheet Tässä opetusohjelmassa, Emme suosittele käyttämällä tuotantoympäristössä.


Voit esikatsella vaiheet Tässä opetusohjelmassa-noudattamalla näitä suosituksia:

- Älä käytä tuotantoympäristössä, ellei se on tarpeen.
- Jos sinulla ei ole kokeiluversion Azure AD-ympäristössä, voit hankkia yksi kuukausi kokeiluversion [tähän](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Skenaarion kuvaus
Tässä opetusohjelmassa testaat Azure AD kertakirjautumisen testiympäristössä. Tässä opetusohjelmassa kuvatut skenaarion kuuluu kaksi tärkeimmät rakenneosien:

1. Cisco Ohjattu lisääminen valikoimasta
2. Yksittäinen määrittäminen ja testaus Azure AD-on


## <a name="adding-cisco-spark-from-the-gallery"></a>Cisco Ohjattu lisääminen valikoimasta
Voit määrittää Cisco ohjattu integroida Azure AD-haluat lisätä Cisco ohjattu valikoimasta hallitun SaaS sovellusluettelo.

**Jos haluat lisätä Cisco ohjattu valikoimasta, toimi seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen portal**, valitse vasemmassa siirtymisruudussa. 

    ![Active Directory][1]

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset][2]

4. Valitse sivun alareunassa **Lisää** .

    ![Sovellukset][3]

5. Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Sovellukset][4]

6. Kirjoita hakuruutuun **Cisco ohjattu**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_01.png)

7. Valitse tulosruudussa **Cisco ohjattu**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yksittäinen määrittäminen ja testaus Azure AD-on
Tässä osassa Määritä ja testaa Azure AD kertakirjautumisen Cisco ohjattu nimeltä "Britta Simon" testikäyttäjän perusteella.

Kertakirjautumisen toimimaan Azure AD on tiedettävä käyttäjälle on Cisco ohjattu vastaavaan käyttäjän Azure AD. Toisin sanoen suhteen Azure AD-käyttäjä ja Aiheeseen liittyvät Cisco ohjattu-linkki on vahvistettava.
Linkki-yhteys on muodostettu määrittämällä **käyttäjänimi** arvo arvoksi Cisco ohjattu **käyttäjänimi** Azure AD. Määrittäminen ja testaaminen Azure AD kertakirjautumisen Cisco ohjattu kanssa, sinun on tehtävä seuraavat hakukyselyn:

1. **[Azure AD määrittäminen Single Sign](#configuring-azure-ad-single-single-sign-on)** - käyttäjät voivat käyttää tätä toimintoa.
2. **[Käyttäjän testata luominen Azure AD](#creating-an-azure-ad-test-user)** - Testaa Azure AD kertakirjautumisen Britta Simon kanssa.
4. **[Käyttäjän luominen Cisco ohjattu testata](#creating-a-cisco-spark-test-user)** - tapahtumista Britta Simon ovat Cisco ohjattu kyseistä henkilöä hän Azure AD-esittäminen.
5. **[Käyttäjän testata määrittäminen Azure AD](#assigning-the-azure-ad-test-user)** - Britta Simon käyttämään Azure AD kertakirjautumisen käyttöön.
5. **[Testaus Single Sign](#testing-single-sign-on)** - voit tarkistaa, toimiiko määritykset.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD kertakirjautumisen määrittäminen

Tässä osassa tavoitteena on Azure AD kertakirjautumisen Azure perinteinen portaalissa ottaminen käyttöön ja määrittää kertakirjautumisen Cisco ohjattu-sovelluksessa.

Cisco ohjattu sovelluksen odottaa SAML vahvistukset sisältävät tiettyjä ominaisuuksia. Määritä tämän sovelluksen määritteet. Voit hallita näitä määritteitä arvot sovelluksen **"Atrributes"** -välilehdestä. Seuraavassa näyttökuvassa näkyy esimerkki tämän.

![Kertakirjautumisen määrittäminen](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_03.png) 

**Määrittää Azure AD kertakirjautumisen Cisco ohjattu, toimimalla seuraavasti:**

1. Valitse Azure perinteinen portaalissa **Cisco ohjattu** sovelluksen integrointi-sivun yläreunassa valikosta **määritteet**.
     
    ![Kertakirjautumisen määrittäminen][5]


2. Valitse **SAML suojaustunnuksen määritteet** -valintaikkunassa seuraavasti:

    a. Valitse **Lisää käyttäjä Attribure** -valintaikkunan avaaminen **Lisää käyttäjä-määrite** .

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_05.png)
    
    b. Kirjoita **uid** **Määritteen nimi** -tekstiruutuun.
    
    c-näppäinyhdistelmää. **Määritteen arvo** -luettelosta **user.userprincipal**.
    
    d. Valitse **Valmis**. **Ota muutokset käyttöön** , valitse sivun alareunassa.

3. Valitse ylä-valikossa **Pika-aloitus**.

    ![Kertakirjautumisen määrittäminen][6]

4. Valitse perinteinen-portaalissa integrointi **Cisco ohjattu** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen Single Sign** -valintaikkunan avaaminen.

    ![Kertakirjautumisen määrittäminen][7] 

5. **Miten käyttäjät voivat kirjautua Cisco ohjattu haluat käyttää** sivulla Valitse **Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.
    
    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_06.png)

6. Valitse **Määritä sovelluksen asetukset** -sivulla toimimalla seuraavasti:

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_07.png)


    a. Kirjoita merkki-URL-tekstiruutuun URL-osoite, käyttämällä seuraavaa kaavaa: `https://web.ciscospark.com/#/signin`.

    b. Valitse **Seuraava**.


7. Valitse **Määritä kertakirjautumisen osoitteessa Cisco ohjattu** -sivulla **Lataa metatiedot**- ja tallenna se tietokoneeseen.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_09.png)

8. Kirjaudu [Cisco Cloud yhteistyö hallinnan](https://admin.ciscospark.com/) järjestelmänvalvojan tunnistetiedoilla.
9. Valitse **asetukset** ja **todennus** -kohdasta **Muokkaa**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_10.png)

10. Valitse **liittää 3rd osapuolen palveluntarjoaja. (Lisäasetukset)** ja siirry seuraavaan ikkunaan.
11. Valitse **Lataa metatieto-tiedosto** ja Tallenna tiedosto tietokoneesta.
12. **Metatietojen tuominen Idp** -sivulla joko vetämällä ja pudottamalla sivulle Azure AD metatieto-tiedosto tai tiedoston selaimelta-asetus ja Etsi ja lataa tiedosto Azure AD-metatiedot. Valitse **edellytä varmenteen allekirjoitetun varmenteen myöntäjältä metatiedot (turvallisempi)** ja valitse **Seuraava**. 

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_11.png)

13. Valitse **SSO Testiyhteys**ja uusi selaimen välilehti avatessa todentamismenetelmä Azure AD kirjautumalla.
14. Palaa **Cisco Cloud yhteistyö hallinta** selaimessa-välilehti. Jos testi onnistui, valitse **Tämän testin onnistui. Ottaa käyttöön kertakirjautumisen vaihtoehto** ja valitse **Seuraava**.

7. Azure AD perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Seuraava**.
    
    ![Azure AD-Single Sign-On][10]

8. Valitse **Sign Vahvista** -sivulla **Valmis**.  
    
    ![Azure AD-Single Sign-On][11]

### <a name="creating-an-azure-ad-test-user"></a>Azure AD-testikäyttäjän luominen
Tässä osassa voit luoda testikäyttäjän kutsutaan Britta Simon perinteinen portaalissa.

![Luo Azure AD-käyttäjä][20]

**Voit luoda testikäyttäjän Azure AD-toimimalla seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen portal**, valitse vasemmassa siirtymisruudussa.
    
    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_09.png) 

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Ylä-valikossa käyttäjäluettelon, näkyviin napsauttamalla **käyttäjät**.
    
    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_03.png) 

4. Avaa **Lisää käyttäjä** -valintaikkunan alareunassa työkalurivillä, valitse **Lisää käyttäjä**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_04.png) 

5. Tee valintaikkunassa **Kerro kyseisen käyttäjän tietoja** -sivulla seuraavasti:
 
    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_05.png) 

    a. Käyttäjänä tyyppi Valitse uusi käyttäjä organisaatiossa.

    b. Kirjoita käyttäjänimi- **tekstiruutuun** **BrittaSimon**.

    c-näppäinyhdistelmää. Valitse **Seuraava**.

6.  Suorita **Käyttäjäprofiili** -sivulla seuraavat toimet:

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_06.png) 

    a. Kirjoita **Ensimmäinen nimi** -tekstiruutuun **Britta**.  

    b. Kirjoita **Sukunimi** -tekstiruutuun tyyppi- **Simon**.

    c-näppäinyhdistelmää. Kirjoita **Näyttönimi** -tekstiruutuun **Britta Simon**.

    d. Valitse **rooli** -luettelosta **käyttäjä**.

    e. Valitse **Seuraava**.

7. Valitse **Hae tilapäinen salasana** -valintaikkunan sivulla **Luo**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_07.png) 

8. **Hae tilapäinen salasana** -valintaikkunan sivulla toimimalla seuraavasti:

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-cisco-spark-tutorial/create_aaduser_08.png) 

    a. Kirjoita **Uusi salasana**arvo muistiin.

    b. Valitse **Valmis**.   



### <a name="creating-a-cisco-spark-test-user"></a>Cisco ohjattu testikäyttäjän luominen

Tässä osassa nimeltä Britta Simon Cisco ohjattu käyttäjän luominen. Tässä osassa nimeltä Britta Simon Cisco ohjattu käyttäjän luominen.

1. Siirry [Cisco Cloud yhteistyö hallinnan](https://admin.ciscospark.com/) järjestelmänvalvojan tunnistetiedoilla.
2. Valitse **käyttäjät** ja **käyttäjien hallinta**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_12.png) 

3. Valitse **Manage User** -ikkunassa **manuaalisesti lisätä** tai muokata käyttäjiä ja valitse **Seuraava**.
4. Valitse **nimet ja sähköpostiosoite**. Täytä sitten tekstiruutu tekemällä:

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_13.png) 

    a. Kirjoita **Ensimmäinen nimi** -tekstiruutuun **Britta**

    b. Kirjoita **Sukunimi** -tekstiruutuun **Simon**

    c-näppäinyhdistelmää. Kirjoita **sähköpostiosoite** -tekstiruutuun**britta.simon@contoso.com**

5. Valitse Lisää Britta Simon plusmerkkiä. Valitse sitten **Seuraava**.
6. Valitse **Lisää palveluita käyttäjille** -ikkunassa **Tallenna** ja sitten **Valmis**.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD-testikäyttäjän määrittäminen

Tässä osassa sinun on otettava käyttöön Azure kertakirjautumisen hänen käyttöoikeuksien myöntämistä Cisco ohjattu Britta Simon.

![Määrittää käyttäjälle][200] 

**Jos haluat määrittää Britta Simon Cisco ohjattu, toimi seuraavasti:**

1. Perinteinen-portaalissa Avaa sovellukset-näkymässä kansio-näkymässä, valitse **sovellukset** yläreunan-valikossa.

    ![Määrittää käyttäjälle][201] 

2. Valitse sovellukset-luettelosta **Cisco ohjattu**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-cisco-spark-tutorial/tutorial_cisco_spark_14.png) 

1. Valitse ylä-valikossa Valitse **käyttäjiä**.

    ![Määrittää käyttäjälle][203] 

1. Valitse kaikki käyttäjät-luettelosta **Britta Simon**.

2. Napsauta työkalurivin alaosassa **määrittäminen**.

    ![Määrittää käyttäjälle][205]


### <a name="testing-single-sign-on"></a>Kertakirjautumisen testaaminen

Tässä osassa tavoitteena on Testaa Azure AD yksittäisen Sign-asetusten käyttäminen Access-paneeli.

Access-ruudussa Cisco ohjattu-ruutua napsauttaessasi voit olisi Hae automaattisesti kirjautunut sisään Cisco ohjattu sovelluksen.

## <a name="additional-resources"></a>Lisäresursseja

* [Luettelo siitä, miten voit integroida SaaS sovellusten Azure Active Directory-opetusohjelmat](active-directory-saas-tutorial-list.md)
* [Mikä on sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_04.png


[5]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_05.png
[6]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_06.png
[7]:  ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_050.png
[10]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_060.png
[11]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_070.png
[20]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-cisco-spark-tutorial/tutorial_general_205.png
