<properties
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi SciQuest käyttävät johtaja | Microsoft Azure"
    description="Opettele määrittämään kertakirjautumisen Azure Active Directory- ja SciQuest käyttävät johtaja välillä."
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


# <a name="tutorial-azure-active-directory-integration-with-sciquest-spend-director"></a>Opetusohjelma: SciQuest käyttävät johtaja Azure Active Directory-integrointi

Tässä opetusohjelmassa tavoitteena on, kuinka voit integroida SciQuest käyttävät johtaja Azure Active Directory (Azure AD).  
Azure AD-integraation SciQuest käyttävät johtaja voi seuraavat edut: 

- Voit hallita Azure AD, jolla on pääsy SciQuest käyttävät johtaja 
- Voit ottaa käyttöön automaattisesti Hae kirjautunut käyttöön SciQuest käyttävät johtaja (kertakirjautumisen) käyttäjille heidän Azure AD-tilien kanssa
- Voit hallita asiakkaiden yhdessä keskitetyssä sijainnissa - Azure perinteinen portal

Jos haluat tietää tarkempia tietoja SaaS sovelluksen integrointi Azure AD-kohdassa [mitä sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Edellytykset 

Määrittää Azure AD-integroinnin SciQuest käyttävät johtaja, tarvitset seuraavat:

- Azure AD-tilaus
- SciQuest käyttävät johtaja single-merkki käytössä-tilauksessa


> [AZURE.NOTE] Voit esikatsella vaiheet Tässä opetusohjelmassa, Emme suosittele käyttämällä tuotantoympäristössä.


Voit esikatsella vaiheet Tässä opetusohjelmassa-noudattamalla näitä suosituksia:

- Älä käytä tuotantoympäristössä, ellei se on tarpeen.
- Jos sinulla ei ole kokeiluversion Azure AD-ympäristössä, voit hankkia yksi kuukausi kokeiluversion [tähän](https://azure.microsoft.com/pricing/free-trial/). 

 
## <a name="scenario-description"></a>Skenaarion kuvaus
Tässä opetusohjelmassa tavoitteena on, jotta voit testata Azure AD kertakirjautumisen testiympäristössä.  
Tässä opetusohjelmassa kuvatut skenaarion kuuluu kaksi tärkeimmät rakenneosien:

1. SciQuest käyttävät johtaja lisääminen valikoimasta 
2. Yksittäinen määrittäminen ja testaus Azure AD-on


## <a name="adding-sciquest-spend-director-from-the-gallery"></a>SciQuest käyttävät johtaja lisääminen valikoimasta
Voit määrittää SciQuest käyttävät johtaja integroida Azure AD-haluat lisätä SciQuest käyttävät johtaja valikoimasta hallitun SaaS sovellusluettelo.

**Voit lisätä SciQuest käyttävät johtaja valikoimasta, toimi seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen portal**, valitse vasemmassa siirtymisruudussa. 

    ![Active Directory][1]

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset][2]

4. Valitse sivun alareunassa **Lisää** .

    ![Sovellukset][3]

5. Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Sovellukset][4]

6. Kirjoita hakuruutuun **sciQuest käyttävät johtaja**.

    ![Sovellukset][5]

7. Valitse tulosruudussa **SciQuest käyttävät johtaja**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![Sovellukset][6]


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yksittäinen määrittäminen ja testaus Azure AD-on
Tässä osassa tavoitteena on noudattamalla voit määrittää ja testaa Azure AD kertakirjautumisen SciQuest käyttävät johtaja nimeltä "Britta Simon" testikäyttäjän perusteella.

Kertakirjautumisen toimimaan Azure AD on tiedettävä vastaavaan käyttäjän SciQuest käyttävät johtaja, Azure AD-käyttäjälle. Toisin sanoen suhteen Azure AD-käyttäjä ja Aiheeseen liittyvät SciQuest käyttävät Director-linkki on vahvistettava.  
Linkki-yhteys on muodostettu määrittämällä **käyttäjänimi** arvo arvoksi SciQuest käyttävät johtaja **käyttäjänimi** Azure AD.
 
Määrittäminen ja testaaminen Azure AD kertakirjautumisen SciQuest käyttävät johtajan kanssa, sinun on tehtävä seuraavat hakukyselyn:

1. **[Määrittäminen Azure AD yksittäisen Single Sign](#configuring-azure-ad-single-single-sign-on)** - käyttäjät voivat käyttää tätä toimintoa.
2. **[Käyttäjän testata luominen Azure AD](#creating-an-azure-ad-test-user)** - Testaa Azure AD kertakirjautumisen Britta Simon kanssa.
4. **[Käyttäjän luominen SciQuest käyttävät johtaja testata](#creating-a-halogen-software-test-user)** - on tapahtumista Britta Simon SciQuest käyttävät johtaja kyseistä henkilöä hän Azure AD esitys.
5. **[Käyttäjän testata määrittäminen Azure AD](#assigning-the-azure-ad-test-user)** - Britta Simon käyttämään Azure AD kertakirjautumisen käyttöön.
5. **[Testaus Single Sign](#testing-single-sign-on)** - voit tarkistaa, toimiiko määritykset.

### <a name="configuring-azure-ad-single-single-sign-on"></a>Azure AD-yhden kertakirjautumisen määrittäminen

Tässä osassa tavoitteena on Azure AD kertakirjautumisen Azure perinteinen portaalissa ottaminen käyttöön ja määrittää kertakirjautumisen SciQuest käyttävät Director-sovelluksessa.

**Määrittää Azure AD kertakirjautumisen SciQuest käyttävät johtaja, toimimalla seuraavasti:**

1. Valitse Azure perinteinen portaalissa integrointi **SciQuest käyttävät johtaja** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen Single Sign** -valintaikkunan avaaminen.

    ![Kertakirjautumisen määrittäminen][8]

2. **Miten käyttäjät voivat kirjautua SciQuest käyttävät johtaja haluat käyttää** sivulla Valitse **Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Azure AD-Single Sign-On][9]

3. Valitse **Määritä sovelluksen asetukset** -sivulla toimimalla seuraavasti: 

    ![Sovelluksen asetusten määrittäminen][10]
 
     3.1. Kirjoita **Merkki-URL** -tekstiruutuun URL-osoite käyttää käyttäjien kirjautua SciQuest käyttävät johtaja sovelluksen käyttämällä seuraavaa kaavaa: *https://.* sciquest.com/.**

     3.2. Kirjoita **Vastaus URL** -tekstiruutuun on sama arvo, joka on kirjoitettu **Merkki-URL** -tekstiruutuun. 

     3.3. Valitse **Seuraava**.
 
4. Valitse **Määritä kertakirjautumisen osoitteessa SciQuest käyttävät Director** -sivulla valitsemalla **Lataa metatiedot**ja Tallenna paikallisesti tietokoneen metatiedot-tiedostoon.

    ![Mikä on Azure AD Connect][11]

5. Yhteyshenkilön SciQuest tuki käyttöön Tämä todennusmenetelmä yllä ladattu-metatiedot.

6. Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Valmis** **Määrittäminen kertakirjautuminen** -valintaikkunassa Sulje. 

    ![Mikä on Azure AD Connect][15]

10. Valitse **Sign Vahvista** -sivulla **Valmis**.  

    




### <a name="creating-an-azure-ad-test-user"></a>Azure AD-testikäyttäjän luominen
Tässä osassa tavoitteena on luotava testikäyttäjän kutsutaan Britta Simon Azure perinteinen portaalissa.

**Voit luoda testikäyttäjän Azure AD-toimimalla seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen portal**, valitse vasemmassa siirtymisruudussa.

    ![Mikä on Azure AD Connect][100] 

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.
3. Ylä-valikossa käyttäjäluettelon, näkyviin napsauttamalla **käyttäjät**.

    ![Mikä on Azure AD Connect][101] 

4. Avaa **Lisää käyttäjä** -valintaikkunan alareunassa työkalurivillä, valitse **Lisää käyttäjä**. 

    ![Mikä on Azure AD Connect][102] 

5. Tee valintaikkunassa **Kerro kyseisen käyttäjän tietoja** -sivulla seuraavasti:

    ![Mikä on Azure AD Connect][103] 

    a. **Käyttäjän tyyppi**Valitse **Uusi käyttäjä organisaatiossa**.
  
    b. Kirjoita käyttäjänimi- **tekstiruutuun** **BrittaSimon**.
  
    c-näppäinyhdistelmää. Valitse Seuraava.

6.  Suorita **Käyttäjäprofiili** -sivulla seuraavat toimet: 

    ![Mikä on Azure AD Connect][104] 

    a. Kirjoita **Ensimmäinen nimi** -tekstiruutuun **Britta**.  
  
    b. Valitse **Sukunimi** txtbox, tyyppi- **Simon**.
  
    c-näppäinyhdistelmää. Kirjoita **Näyttönimi** -tekstiruutuun **Britta Simon**.
  
    d. Valitse **rooli** -luettelosta **käyttäjä**.
  
    e. Valitse **Seuraava**.

7. Valitse **Hae tilapäinen salasana** -valintaikkunan sivulla **Luo**.

    ![Mikä on Azure AD Connect][105]  

8. **Hae tilapäinen salasana** -valintaikkunan sivulla toimimalla seuraavasti:

    ![Mikä on Azure AD Connect][106]   

    a. Kirjoita **Uusi salasana**arvo muistiin.
  
    b. Valitse **Valmis**.   
  
 
### <a name="creating-a-sciquest-spend-director-test-user"></a>SciQuest käyttävät johtaja testikäyttäjän luominen

Tässä osassa tavoitteena on nimeltään Britta Simon SciQuest käyttävät johtaja käyttäjän luominen.

Sinun täytyy SciQuest käyttävät johtaja tukihenkilöstöltä ja antaa niille hankkiminen luotu Testaa tilin tiedot.

Vaihtoehtoisesti voit myös hyödyntää juuri perustuvan valmistelu yksittäisen Sign-ominaisuus, joka tue SciQuest käyttävät johtaja.  
Jos juuri perustuvan valmistelu on otettu käyttöön, käyttäjät luodaan automaattisesti SciQuest käyttävät johtaja aikana yksittäinen Sign yrityksellä jos niitä ei ole. Tämä ominaisuus ei tarvitse luoda manuaalisesti Sign vastaavaan käyttäjät.

Saat juuri perustuvan valmistelu käytössä-haluat ottaa yhteyttä olet SciQuest käyttävät johtaja tukihenkilöstölle.
  

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD-testikäyttäjän määrittäminen

Tässä osassa tavoitteena on Britta Simon käyttämään Azure kertakirjautumisen hänen käyttöoikeuden myöntämisestä SciQuest käyttävät johtaja ottaminen käyttöön.

![Mikä on Azure AD Connect][200]

**Jos haluat määrittää Britta Simon SciQuest käyttävät johtaja, toimi seuraavasti:**

1. Azure perinteinen-portaalissa Avaa sovellukset-näkymässä kansio-näkymässä, valitse **sovellukset** yläreunan-valikossa.

    ![Mikä on Azure AD Connect][201]

2. Valitse sovellukset-luettelosta **SciQuest käyttävät johtaja**.

    ![Mikä on Azure AD Connect][202]

1. Valitse ylä-valikossa Valitse **käyttäjiä**.

    ![Mikä on Azure AD Connect][203]

1. Valitse käyttäjät-luettelosta **Britta Simon**.

    ![Mikä on Azure AD Connect][204]

2. Napsauta työkalurivin alaosassa **määrittäminen**.

    ![Mikä on Azure AD Connect][205]



### <a name="testing-single-sign-on"></a>Kertakirjautumisen testaaminen

Tässä osassa tavoitteena on Testaa Azure AD yksittäisen Sign-asetusten käyttäminen Access-paneeli.  
Access-ruudussa SciQuest käyttävät Director-ruutua napsauttaessasi voit olisi Hae automaattisesti kirjautunut sisään SciQuest käyttävät Director-sovellukseen.


## <a name="additional-resources"></a>Lisäresursseja

* [Luettelo siitä, miten voit integroida SaaS sovellusten Azure Active Directory-opetusohjelmat](active-directory-saas-tutorial-list.md)
* [Mikä on sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta?](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_01.png
[2]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_02.png
[3]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_03.png
[4]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_04.png
[5]: ./media/active-directory-saas-sciquest-spend-director/tutorial_sciquest_spend_director_01.png
[6]: ./media/active-directory-saas-sciquest-spend-director/tutorial_sciquest_spend_director_05.png
[8]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_06.png
[9]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_07.png
[10]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_08.png
[11]: ./media/active-directory-saas-sciquest-spend-director/tutorial_sciquest_spend_director_03.png
[15]: ./media/active-directory-saas-sciquest-spend-director/tutorial_sciquest_spend_director_04.png

[100]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_09.png 
[101]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_10.png 
[102]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_11.png 
[103]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_12.png 
[104]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_13.png 
[105]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_14.png 
[106]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_15.png 
[200]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_16.png 
[201]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_17.png 
[202]: ./media/active-directory-saas-sciquest-spend-director/tutorial_sciquest_spend_director_06.png
[203]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_18.png
[204]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_19.png
[205]: ./media/active-directory-saas-sciquest-spend-director/tutorial_general_20.png

