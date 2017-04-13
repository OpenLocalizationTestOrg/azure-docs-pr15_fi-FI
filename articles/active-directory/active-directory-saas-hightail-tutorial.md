<properties
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi Hightail | Microsoft Azure"
    description="Opettele määrittämään kertakirjautumisen Azure Active Directory- ja Hightail välillä."
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
    ms.date="09/29/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-hightail"></a>Opetusohjelma: Hightail Azure Active Directory-integrointi

Tässä opetusohjelmassa tavoitteena on, kuinka voit integroida Hightail Azure Active Directory (Azure AD).

Azure AD-integraation Hightail avulla voit seuraavat edut:

- Voit hallita Azure AD, jolla on pääsy Hightail
- Voit ottaa käyttöön automaattisesti Hae kirjautunut käyttöön Hightail (kertakirjautumisen) niiden Azure AD-tilien käyttäjille
- Voit hallita asiakkaiden yhdessä keskitetyssä sijainnissa - Azure perinteinen portal

Jos haluat tietää tarkempia tietoja SaaS sovelluksen integrointi Azure AD-kohdassa [mitä sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Edellytykset

Määrittää Azure AD-integroinnin Hightail, tarvitset seuraavat:

- Azure AD-tilaus
- Hightail single-merkki käytössä-tilauksessa


> [AZURE.NOTE] Voit esikatsella vaiheet Tässä opetusohjelmassa, Emme suosittele käyttämällä tuotantoympäristössä.


Voit esikatsella vaiheet Tässä opetusohjelmassa-noudattamalla näitä suosituksia:

- Älä käytä tuotantoympäristössä, ellei se on tarpeen.
- Jos sinulla ei ole kokeiluversion Azure AD-ympäristössä, voit hankkia yksi kuukausi kokeiluversion [tähän](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Skenaarion kuvaus
Tässä opetusohjelmassa tavoitteena on, jotta voit testata Azure AD kertakirjautumisen testiympäristössä. 

Tässä opetusohjelmassa kuvatut skenaarion kuuluu kaksi tärkeimmät rakenneosien:

1. Hightail lisääminen valikoimasta
2. Yksittäinen määrittäminen ja testaus Azure AD-on


## <a name="adding-hightail-from-the-gallery"></a>Hightail lisääminen valikoimasta
Voit määrittää Hightail integroida Azure AD-haluat lisätä Hightail valikoimasta hallitun SaaS sovellusluettelo.

**Jos haluat lisätä Hightail valikoimasta, toimi seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen portal**, valitse vasemmassa siirtymisruudussa. 
 
    ![Active Directory][1]

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset][2]

4. Valitse sivun alareunassa **Lisää** .

    ![Sovellukset][3]

5. Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Sovellukset][4]

6. Kirjoita hakuruutuun **Hightail**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_01.png)

7. Valitse tulosruudussa **Hightail**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![Sovelluksen valitseminen valikoimasta](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yksittäinen määrittäminen ja testaus Azure AD-on
Tässä osassa tavoitteena on noudattamalla voit määrittää ja testaa Azure AD kertakirjautumisen Hightail nimeltä "Britta Simon" testikäyttäjän perusteella.

Kertakirjautumisen toimimaan Azure AD on tiedettävä vastaavaan käyttäjän Hightail, Azure AD-käyttäjälle. Toisin sanoen suhteen Azure AD-käyttäjä ja Aiheeseen liittyvät Hightail-linkki on vahvistettava.

Linkki-yhteys on muodostettu määrittämällä **käyttäjänimi** arvo arvoksi Hightail **käyttäjänimi** Azure AD.

Määrittäminen ja testaaminen Azure AD kertakirjautumisen Hightail kanssa, sinun on tehtävä seuraavat hakukyselyn:

1. **[Azure AD määrittäminen Single Sign](#configuring-azure-ad-single-sign-on)** - käyttäjät voivat käyttää tätä toimintoa.
2. **[Käyttäjän testata luominen Azure AD](#creating-an-azure-ad-test-user)** - Testaa Azure AD kertakirjautumisen Britta Simon kanssa.
4. **[Luomisesta Hightail testata käyttäjän](#creating-a-hightail-test-user)** - tapahtumista Britta Simon ovat Hightail, jotka on linkitetty hän Azure AD esitys.
5. **[Käyttäjän testata määrittäminen Azure AD](#assigning-the-azure-ad-test-user)** - Britta Simon käyttämään Azure AD kertakirjautumisen käyttöön.
5. **[Testaus Single Sign](#testing-single-sign-on)** - voit tarkistaa, toimiiko määritykset.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD-Single Sign-On määrittäminen

Tässä osassa tavoitteena on Azure AD kertakirjautumisen Azure perinteinen portaalissa ottaminen käyttöön ja määrittää kertakirjautumisen Hightail-sovelluksessa.

Hightail sovelluksen odottaa SAML vahvistukset tietyssä muodossa. Määritä seuraavat saatavat tämän sovelluksen. Voit hallita näitä määritteitä arvot sovelluksen **"Atrribute"** -välilehdestä. Seuraavassa näyttökuvassa näkyy esimerkki tämän. 

![Kertakirjautumisen määrittäminen](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_51.png) 

**Määrittää Azure AD kertakirjautumisen Hightail, toimimalla seuraavasti:**


1. Valitse Azure perinteinen portaalissa **Hightail** sovelluksen integrointi-sivun yläreunassa valikosta **määritteet**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-hightail-tutorial/tutorial_general_81.png) 


1. **SAML-tunnuksen määritteet** -valintaikkunassa kullekin riville seuraavassa taulukossa näkyvät toimimalla seuraavasti:

  	| Määritteen nimi | Määritteen arvo |
  	| --- | --- |    
  	| Etunimi | User.givenName |
  	| Sukunimi  | User.surname |
  	| Sähköpostin | User.Mail |
  	| UserIdentity-kohteeseen | User.Mail |

    a. Valitse **Lisää käyttäjä Attribure** -valintaikkunan avaaminen **Lisää käyttäjä-määrite** .

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-hightail-tutorial/tutorial_general_82.png) 


    b. Kirjoita määritteen nimi näkyy kyseisen rivin **Attrubute nimi** -tekstiruutuun.

    c-näppäinyhdistelmää. **Määritteen arvo** -luettelosta selsect kyseisen rivin määritteen arvo.

    d. Valitse **Valmis**.  
    



1. Valitse ylä-valikossa **Pika-aloitus**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-hightail-tutorial/tutorial_general_83.png)  



2. **Miten käyttäjät voivat kirjautua Hightail haluat käyttää** sivulla Valitse **Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_03.png) 


3. Jos haluat sovelluksen määrittäminen **IDP aloittanut tila**, seuraavien toimien **App-asetusten määrittäminen** -sivulla ja valitse **Seuraava**:

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_04.png) 



    a. Kirjoita **Vastaus URL** -tekstiruutuun URL-Osoitetta seuraavaa kaavaa: **"https://www.hightail.com/samlLogin?phi_action=app/samlLogin & alitoimenpiteen = handleSamlResponse"**

    b. Valitse **Seuraava**

4. Jos haluat määrittää sovelluksen **SP aloittanut tila** -valintaikkunan **App-asetusten määrittäminen** -sivulla, valitse **"Näytä lisäasetukset (valinnainen)"** ja kirjoita **Merkki-URL-osoite** ja valitse **Seuraava**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_06.png) 

    a. Kirjoita merkki-URL-tekstiruutuun URL-osoite käyttämä Sign käyttämällä seuraavaa kaavaa Hightail sovelluksen käyttäjille: **https://www.hightail.com/loginSSO**. Tämä on yleisiä kirjautumissivun kaikki asiakkaat, jotka haluat käyttää SSO.

    b. Valitse **Seuraava**

5. Valitse **Määritä kertakirjautumisen osoitteessa Hightail** -sivulla suorittamalla seuraavat vaiheet ja valitse **Seuraava**:

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_05.png) 


    a. Valitse **Lataa**ja Tallenna tietokoneessa base-64-koodattu varmenne-tiedostoon.

    b. Valitse **Seuraava**.

    > [AZURE.NOTE] Ennen kuin määrität Single Sign On osoitteessa Hightail sovellus, ota valkoinen luettelo Hightail sähköpostin toimialueen ryhmän niin, että kaikki käyttäjät, jotka käyttävät toimialueessa voidaan hyödyntää kertakirjautuminen toimintoja.

6. Saat määritetty sovelluksen SSO-haluat Sign Hightail asiakasympäristöön järjestelmänvalvojana.

    a. Valitse ylä-valikossa Valitse **tili** -välilehti ja valitse **Määritä SAML**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_001.png) 


    b. Valitse **SAML**-todennuksen käyttöön-valintaruutu.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_002.png) 

    c-näppäinyhdistelmää. Avaa base-64-koodattu varmennetta Muistiossa, kopioi se sisällön Leikepöydän ja liitä se **SAML tunnuksen allekirjoitusvarmenne** -ruudun arvo.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_003.png) 


    d. Kopioi Remote kirjautuminen URL-Osoitteen Azure AD **SAML myöntäjä (tunnistetietojen toimittaja)** Hightail.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_005.png)
    
    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_004.png)

    e. Jos haluat sovelluksen määrittäminen **IDP aloittanut tilassa** valitsemalla **"Tunnistetietojen toimittaja (IdP) aloittanut Kirjaudu sisään"**. Jos **SP aloittanut tilassa** valitsemalla **"Palveluntarjoaja (SP) aloittanut Kirjaudu sisään"**.
    
    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_006.png)

    f. Omassa SAML kuluttaja URL-Osoitteen kopioiminen ja liitä se **Vastaa URL** -tekstiruutuun vaiheessa 4 esitetyllä tavalla. 

    g. Valitse **Tallenna**.



6. Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Seuraava**.

    ![Azure AD-Single Sign-On][10]

7. Valitse **Sign Vahvista** -sivulla **Valmis**. 
 
    ![Azure AD-Single Sign-On][11]




### <a name="creating-an-azure-ad-test-user"></a>Azure AD-testikäyttäjän luominen
Tässä osassa tavoitteena on luotava testikäyttäjän kutsutaan Britta Simon Azure perinteinen portaalissa.

Valitse käyttäjät-luettelosta **Britta Simon**.

![Luo Azure AD-käyttäjä][20]

**Voit luoda testikäyttäjän Azure AD-toimimalla seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen portal**, valitse vasemmassa siirtymisruudussa.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-hightail-tutorial/create_aaduser_09.png) 

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Ylä-valikossa käyttäjäluettelon, näkyviin napsauttamalla **käyttäjät**.
 
    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-hightail-tutorial/create_aaduser_03.png) 


4. Avaa **Lisää käyttäjä** -valintaikkunan alareunassa työkalurivillä, valitse **Lisää käyttäjä**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-hightail-tutorial/create_aaduser_04.png)

5. Tee valintaikkunassa **Kerro kyseisen käyttäjän tietoja** -sivulla seuraavasti:

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-hightail-tutorial/create_aaduser_05.png) 

    a. **Käyttäjän tyyppi**Valitse **Uusi käyttäjä organisaatiossa**.

    b. Kirjoita **Käyttäjänimi** -tekstiruutuun **BrittaSimon**.

    c-näppäinyhdistelmää. Valitse **Seuraava**.

6.  Suorita **Käyttäjäprofiili** -sivulla seuraavat toimet:

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-hightail-tutorial/create_aaduser_06.png) 

    a. Kirjoita **Ensimmäinen nimi** -tekstiruutuun **Britta**.  

    b. Kirjoita **Sukunimi** -tekstiruutuun tyyppi- **Simon**.

    c-näppäinyhdistelmää. Kirjoita **Näyttönimi** -tekstiruutuun **Britta Simon**.

    d. Valitse **rooli** -luettelosta **käyttäjä**.

    e. Valitse **Seuraava**.

7. Valitse **Hae tilapäinen salasana** -valintaikkunan sivulla **Luo**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-hightail-tutorial/create_aaduser_07.png) 


8. **Hae tilapäinen salasana** -valintaikkunan sivulla toimimalla seuraavasti:
 
    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-hightail-tutorial/create_aaduser_08.png) 

    a. Kirjoita **Uusi salasana**arvo muistiin.

    b. Valitse **Valmis**.   



### <a name="creating-a-hightail-test-user"></a>Hightail testikäyttäjän luominen

Tässä osassa tavoitteena on nimeltään Britta Simon Hightail käyttäjän luominen. 

Ei ole toimi puolestasi sisältö. Hightail tukee vain perustuvan käyttäjän valmistelu perusteella mukautetun saatavat. Jos olet määrittänyt mukautetun saatavat osassa **[Määrittäminen Azure AD kertakirjautumisen](#configuring-azure-ad-single-sign-on)** edellä kuvatulla käyttäjä luodaan automaattisesti ei ole vielä-sovelluksessa. 

> [AZURE.NOTE] Jos haluat luoda luodun käyttäjän manuaalisesti, sinun on Hightail tukeen kautta [support@hightail.com](mailto:support@hightail.com).


### <a name="assigning-the-azure-ad-test-user"></a>Azure AD-testikäyttäjän määrittäminen

Tässä osassa tavoitteena on Britta Simon käyttämään Azure kertakirjautumisen hänen käyttöoikeuden myöntämisestä Hightail ottaminen käyttöön.

![Määrittää käyttäjälle][200] 

**Jos haluat määrittää Britta Simon Hightail, toimi seuraavasti:**

1. Azure perinteinen-portaalissa Avaa sovellukset-näkymässä kansio-näkymässä, valitse **sovellukset** yläreunan-valikossa.
 
    ![Määrittää käyttäjälle][201] 

2. Valitse sovellukset-luettelosta **Hightail**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-hightail-tutorial/tutorial_hightail_50.png) 


1. Valitse ylä-valikossa Valitse **käyttäjiä**.

    ![Määrittää käyttäjälle][203]

1. Valitse käyttäjät-luettelosta **Britta Simon**.

2. Napsauta työkalurivin alaosassa **määrittäminen**.

![Määrittää käyttäjälle][205]



### <a name="testing-single-sign-on"></a>Kertakirjautumisen testaaminen

Tässä osassa tavoitteena on Testaa Azure AD yksittäisen Sign-asetusten käyttäminen Access-paneeli.

Access-ruudussa Hightail-ruutua napsauttaessasi sinun pitäisi saada automaattisesti kirjautunut-on Hightail sovelluksen.


## <a name="additional-resources"></a>Lisäresursseja

* [Luettelo siitä, miten voit integroida SaaS sovellusten Azure Active Directory-opetusohjelmat](active-directory-saas-tutorial-list.md)
* [Mikä on sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-hightail-tutorial/tutorial_general_205.png
