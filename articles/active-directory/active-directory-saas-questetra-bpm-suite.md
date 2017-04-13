<properties
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi Questetra BPM Suite | Microsoft Aure"
    description="Opettele määrittämään kertakirjautumisen Azure Active Directory- ja Questetra BPM Suite välillä."
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
    ms.date="10/28/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-questetra-bpm-suite"></a>Opetusohjelma: Questetra BPM Suite Azure Active Directory-integrointi

Tässä opetusohjelmassa tavoitteena on, kuinka voit integroida Questetra BPM Suite Azure Active Directory (Azure AD).  
Azure AD-Questetra BPM Suite integraation avulla voit seuraavat edut: 

- Voit hallita Azure AD, jolla on pääsy Questetra BPM Suite 
- Voit ottaa käyttöön automaattisesti Hae kirjautunut sisään Questetra BPM ohjelmistopakettiin (kertakirjautumisen) käyttäjille heidän Azure AD-tilien kanssa
- Voit hallita asiakkaiden yhdessä keskitetyssä sijainnissa - Azure perinteinen portal

Jos haluat tietää tarkempia tietoja SaaS sovelluksen integrointi Azure AD-kohdassa [mitä sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Edellytykset 

Määrittää Azure AD-integroinnin Questetra BPM tuotepakettia, sinun on seuraavia kohteita:

- Azure AD-tilaus
- On [Questetra BPM Suite](https://senbon-imadegawa-988.questetra.net/) single-etumerkki käytössä-tilauksessa


> [AZURE.NOTE] Voit esikatsella vaiheet Tässä opetusohjelmassa, Emme suosittele käyttämällä tuotantoympäristössä.


Voit esikatsella vaiheet Tässä opetusohjelmassa-noudattamalla näitä suosituksia:

- Älä käytä tuotantoympäristössä, ellei se on tarpeen.
- Jos sinulla ei ole kokeiluversion Azure AD-ympäristössä, voit hankkia yksi kuukausi kokeiluversion [tähän](https://azure.microsoft.com/pricing/free-trial/). 

 
## <a name="scenario-description"></a>Skenaarion kuvaus
Tässä opetusohjelmassa tavoitteena on, jotta voit testata Azure AD kertakirjautumisen testiympäristössä.  
Tässä opetusohjelmassa kuvatut skenaarion koostuu kolme tärkeimmät rakenneosien:

1. Questetra BPM Suite lisääminen valikoimasta 
2. Yksittäinen määrittäminen ja testaus Azure AD-on


## <a name="adding-questetra-bpm-suite-from-the-gallery"></a>Questetra BPM Suite lisääminen valikoimasta
Voit määrittää Questetra BPM Suite integroida Azure AD-haluat lisätä Questetra BPM Suite valikoimasta hallitun SaaS sovellusluettelo.

**Jos haluat lisätä Questetra BPM Suite valikoimasta, toimi seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen portal**, valitse vasemmassa siirtymisruudussa. 

    ![Active Directory][1]

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset][2]

4. Valitse sivun alareunassa **Lisää** .

    ![Sovellukset][3]

5. Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Sovellukset][4]

6. Kirjoita hakuruutuun **Questetra BPM Suite**.

    ![Sovellukset][5]

7. Valitse tulosruudussa **Questetra BPM Suite**ja valitse **Valmis** , voit lisätä sovelluksen.



##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yksittäinen määrittäminen ja testaus Azure AD-on
Tässä osassa tavoitteena on noudattamalla voit määrittää ja testaa Azure AD kertakirjautumisen Questetra BPM Suite nimeltä "Britta Simon" testikäyttäjän perusteella.

Kertakirjautumisen toimimaan Azure AD on tiedettävä vastaavaan käyttäjän Questetra BPM Suite, Azure AD-käyttäjälle. Toisin sanoen linkki suhteen Azure AD-käyttäjä ja Aiheeseen liittyvät Questetra BPM Suite on vahvistettava.  
Linkki-yhteys on muodostettu määrittämällä **käyttäjänimi** arvo arvoksi Questetra BPM Suite **käyttäjänimi** Azure AD.
 
Määrittäminen ja testaaminen Azure AD kertakirjautumisen Questetra BPM Suite, sinun on tehtävä seuraavat hakukyselyn:

1. **[Azure AD määrittäminen Single Sign](#configuring-azure-ad-single-single-sign-on)** - käyttäjät voivat käyttää tätä toimintoa.
2. **[Käyttäjän testata luominen Azure AD](#creating-an-azure-ad-test-user)** - Testaa Azure AD kertakirjautumisen Britta Simon kanssa.
4. **[Käyttäjän luominen Questetra BPM tuotepakettia testata](#creating-a-questetra-bpm-suite-test-user)** - on tapahtumista Britta Simon Questetra BPM ohjelmistopaketti, joka on linkitetty hän Azure AD esitys.
5. **[Käyttäjän testata määrittäminen Azure AD](#assigning-the-azure-ad-test-user)** - Britta Simon käyttämään Azure AD kertakirjautumisen käyttöön.
5. **[Testaus Single Sign](#testing-single-sign-on)** - voit tarkistaa, toimiiko määritykset.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD-Single Sign-On määrittäminen

Tässä osassa tavoitteena on Azure AD kertakirjautumisen Azure perinteinen portaalissa ottaminen käyttöön ja määrittää kertakirjautumisen Questetra BPM Suite-sovelluksessa.

**Voit määrittää Azure AD kertakirjautumisen Questetra BPM Suite toimimalla seuraavasti:**

1. Valitse Azure perinteinen portaalissa integrointi **Questetra BPM Suite** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen Single Sign** -valintaikkunan avaaminen.

    ![Kertakirjautumisen määrittäminen][8]

2. **Miten haluat kirjautua Questetra BPM ohjelmistopakettiin käyttäjät** -sivulla Valitse **Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Azure AD-Single Sign-On][9]


3. Eri selainikkunassa Kirjaudu **Questetra BPM Suite** yrityksesi sivuston järjestelmänvalvojan oikeuksilla.

4. Valitse ylä-valikossa **Järjestelmäasetuksia**. 

    ![Azure AD-Single Sign-On][10]

5. Avaa **SingleSignOnSAML** -sivu, valitse **SSO (SAML)**. 

    ![Azure AD-Single Sign-On][11]


6. Suorita seuraavat vaiheet Azure perinteinen portaalissa **App-asetusten määrittäminen** -sivulla: 

    ![Sovelluksen asetusten määrittäminen][13]
 
    a. Edelleen voit **Questetra BPM Suite** yrityksen sivuston SP tieto-osasta kopioi **ACS URL-osoite**ja liitä se sitten **Merkki-URL** -tekstiruutuun.

    b. Voit **Questetra BPM Suite** yrityksen sivuston SP tieto-osasta edelleen kopioi **Kohteen tunnus**, ja liitä se sitten **Myöntäjä URL** -tekstiruutuun.

    c-näppäinyhdistelmää. Edelleen voit **Questetra BPM Suite** yrityksen sivuston SP tieto-osasta kopioi **ACS URL-osoite**ja liitä se sitten **Vastaa URL** -tekstiruutuun.

    d. Valitse **Seuraava**.

 
7. Valitse **Määritä kertakirjautumisen Questetra BPM Suite osoitteessa** -sivulla valitsemalla **Lataa**ja Tallenna sertifikaattitiedosto paikallisesti tietokoneeseen.

    ![Kertakirjautumisen määrittäminen][14]


8. Voit **Questetra BPM Suite** yrityksen sivuston Suorita seuraavat toimet: 

    ![Kertakirjautumisen määrittäminen][15]

    a. Valitse **Ota käyttöön kertakirjautumisen**.
     
    b. Azure perinteinen portaalissa **Myöntäjä URL-osoite** -arvo kopioi ja liitä se sitten **Kohteen tunnus** -tekstiruutuun.

    c-näppäinyhdistelmää. Azure perinteinen portaalissa **Sign-palvelun URL** -arvon kopioi ja liitä se sitten **Kirjaudu sisään sivun URL** -tekstiruutuun.

    d. Azure perinteinen portaalissa **Yksittäisen Sign-Out palvelun URL-osoite** -arvo kopioi ja liitä se sitten **Kirjaudu ulos sivun URL** -tekstiruutuun.

    e. Kirjoita **NameID muoto** -tekstiruutuun **urn: oasis: nimet: tc: SAML:1.1:nameid-muoto: emailAddress**.


    f. Luo base-64-koodattu tiedosto ladattu varmennetta. 

    >[AZURE.TIP] Lisätietoja on artikkelissa [muuntaa binaariluvun varmenteen tekstitiedostoon](http://youtu.be/PlgrzUZ-Y1o).

    g. Avaa base-64-koodattu varmennetta Muistiossa, kopioi Leikepöydän sisällön se ja liitä se sitten **todennusvarmennetta** tekstiruutu. 

    h. Valitse **Tallenna**.


9. Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Seuraava**. 

    ![Mikä on Azure AD Connect][17]


10. Valitse **Sign Vahvista** -sivulla **Valmis**.  

    ![Mikä on Azure AD Connect][18]




### <a name="creating-an-azure-ad-test-user"></a>Azure AD-testikäyttäjän luominen
Tässä osassa tavoitteena on luotava testikäyttäjän kutsutaan Britta Simon Azure perinteinen portaalissa.

**Voit luoda testikäyttäjän Azure AD-toimimalla seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen portal**, valitse vasemmassa siirtymisruudussa.

    ![Azure AD-testikäyttäjän luominen][100] 

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Ylä-valikossa käyttäjäluettelon, näkyviin napsauttamalla **käyttäjät**.

    ![Azure AD-testikäyttäjän luominen][101] 

4. Avaa **Lisää käyttäjä** -valintaikkunan alareunassa työkalurivillä, valitse **Lisää käyttäjä**. 

    ![Azure AD-testikäyttäjän luominen][102] 

5. Tee valintaikkunassa **Kerro kyseisen käyttäjän tietoja** -sivulla seuraavasti:

    ![Azure AD-testikäyttäjän luominen][103]
 
    a. **Käyttäjän tyyppi**Valitse **Uusi käyttäjä organisaatiossa**.
  
    b. Kirjoita käyttäjänimi- **tekstiruutuun** **BrittaSimon**.

    c-näppäinyhdistelmää. Valitse Seuraava.

6.  Suorita **Käyttäjäprofiili** -sivulla seuraavat toimet: 

    ![Azure AD-testikäyttäjän luominen][104] 
  
    a. Kirjoita **Ensimmäinen nimi** -tekstiruutuun **Britta**. 
 
    b. Kirjoita **Sukunimi** -tekstiruutuun tyyppi- **Simon**.

    c-näppäinyhdistelmää. Kirjoita **Näyttönimi** -tekstiruutuun **Britta Simon**.

    d. Valitse **rooli** -luettelosta **käyttäjä**.

    e. Valitse **Seuraava**.

7. Valitse **Hae tilapäinen salasana** -valintaikkunan sivulla **Luo**.

    ![Azure AD-testikäyttäjän luominen][105]  

8. **Hae tilapäinen salasana** -valintaikkunan sivulla toimimalla seuraavasti:

    ![Azure AD-testikäyttäjän luominen][106]   
  
    a. Kirjoita **Uusi salasana**arvo muistiin.
  
    b. Valitse **Valmis**.   
  
 
### <a name="creating-a-questetra-bpm-suite-test-user"></a>Questetra BPM Suite testikäyttäjän luominen

Tässä osassa tavoitteena on nimeltään Britta Simon Questetra BPM Suite käyttäjän luominen.

**Luo kutsutaan Britta Simon Questetra BPM Suite käyttäjä, toimi seuraavasti:**

1.  Kertakirjauksen Questetra BPM Suite yrityksen sivustoon järjestelmänvalvojana.
2.  Valitse **Järjestelmäasetukset > käyttäjäluettelon > uusi käyttäjä**. 
3.  Valitse uusi käyttäjä-valintaikkunassa seuraavasti: 

    ![Luo testikäyttäjän][300] 

    a. Kirjoita **nimi** -tekstiruutuun Azure AD Britta henkilön käyttäjänimi.

    b. Kirjoita **sähköpostiosoite** -tekstiruutuun Azure AD Britta henkilön käyttäjänimi.

    c-näppäinyhdistelmää. Kirjoita salasana **salasana** -tekstiruutuun.

4.  Valitse **Lisää uusi käyttäjä**.



### <a name="assigning-the-azure-ad-test-user"></a>Azure AD-testikäyttäjän määrittäminen

Tässä osassa tavoitteena on Britta Simon käyttämään Azure kertakirjautumisen hänen käyttöoikeuksien myöntäminen Questetra BPM ohjelmistopakettiin ottaminen käyttöön.

![Mikä on Azure AD Connect][200]

**Jos haluat määrittää Britta Simon Questetra BPM Suite, toimi seuraavasti:**

1. Azure perinteinen-portaalissa Avaa sovellukset-näkymässä kansio-näkymässä, valitse **sovellukset** yläreunan-valikossa.

    ![Mikä on Azure AD Connect][201]

2. Valitse sovellukset-luettelosta **Questetra BPM Suite**.

    ![Mikä on Azure AD Connect][205]

1. Valitse ylä-valikossa Valitse **käyttäjiä**.

    ![Mikä on Azure AD Connect][202]

1. Valitse käyttäjät-luettelosta **Britta Simon**.

    ![Mikä on Azure AD Connect][203]

2. Napsauta työkalurivin alaosassa **määrittäminen**.

    ![Mikä on Azure AD Connect][204]



### <a name="testing-single-sign-on"></a>Kertakirjautumisen testaaminen

Tässä osassa tavoitteena on Testaa Azure AD yksittäisen Sign-asetusten käyttäminen Access-paneeli.  
Access-ruudussa Questetra BPM Suite-ruutua napsauttaessasi sinun pitäisi saada automaattisesti kirjautunut-on Questetra BPM Suite sovelluksen.


## <a name="additional-resources"></a>Lisäresursseja

* [Luettelo siitä, miten voit integroida SaaS sovellusten Azure Active Directory-opetusohjelmat](active-directory-saas-tutorial-list.md)
* [Mikä on sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta?](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_01.png
[2]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_02.png
[3]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_03.png
[4]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_04.png
[5]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_01.png


[8]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_06.png
[9]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_02.png
[10]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_03.png
[11]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_04.png
[12]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_05.png
[13]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_06.png
[14]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_07.png
[15]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_08.png
[16]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_09.png
[17]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_10.png
[18]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_08.png


[100]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_09.png 
[101]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_10.png 
[102]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_11.png 
[103]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_12.png 
[104]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_13.png 
[105]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_14.png 
[106]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_15.png 


[200]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_16.png 
[201]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_17.png 
[202]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_18.png
[203]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_19.png
[204]: ./media/active-directory-saas-questetra-bpm-suite/tutorial_general_20.png
[205]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_12.png

[300]: ./media/active-directory-saas-questetra-bpm-suite/questera_bpm_suite_11.png 