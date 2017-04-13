<properties
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi Jive | Microsoft Azure"
    description="Opettele määrittämään kertakirjautumisen Azure Active Directory- ja Jive välillä."
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


# <a name="tutorial-azure-active-directory-integration-with-jive"></a>Opetusohjelma: Jive Azure Active Directory-integrointi

Tässä opetusohjelmassa kerrotaan, miten voit integroida Jive Azure Active Directory (Azure AD).

Azure AD-integraation Jive avulla voit seuraavat edut:

- Voit hallita Azure AD, jolla on pääsy Jive
- Voit ottaa käyttöön automaattisesti Hae kirjautunut käyttöön Jive (kertakirjautumisen) niiden Azure AD-tilien käyttäjille
- Voit hallita asiakkaiden yhdessä keskitetyssä sijainnissa - Azure perinteinen portal

Jos haluat tietää tarkempia tietoja SaaS sovelluksen integrointi Azure AD-kohdassa [mitä sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Edellytykset

Määrittää Azure AD-integroinnin Jive, tarvitset seuraavat:

- Azure AD-tilaus
- Jive single-merkki käytössä-tilauksessa


> [AZURE.NOTE] Voit esikatsella vaiheet Tässä opetusohjelmassa, Emme suosittele käyttämällä tuotantoympäristössä.


Voit esikatsella vaiheet Tässä opetusohjelmassa-noudattamalla näitä suosituksia:

- Älä käytä tuotantoympäristössä, ellei se on tarpeen.
- Jos sinulla ei ole kokeiluversion Azure AD-ympäristössä, voit hankkia yksi kuukausi kokeiluversion [tähän](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Skenaarion kuvaus
Tässä opetusohjelmassa testaat Azure AD kertakirjautumisen testiympäristössä.

Tässä opetusohjelmassa kuvatut skenaarion kuuluu kaksi tärkeimmät rakenneosien:

1. Jive lisääminen valikoimasta
2. Yksittäinen määrittäminen ja testaus Azure AD-on


## <a name="adding-jive-from-the-gallery"></a>Jive lisääminen valikoimasta
Määrittämiseen Jive suoraan Azure AD-integrointia sinun on lisättävä Jive valikoimasta hallitun SaaS sovellusluettelo.

**Jos haluat lisätä Jive valikoimasta, toimi seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen portal**, valitse vasemmassa siirtymisruudussa.

    ![Active Directory][1]
2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset][2]

4. Valitse sivun alareunassa **Lisää** .

    ![Sovellukset][3]

5. Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Sovellukset][4]

6. Kirjoita hakuruutuun **Jive**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-jive-tutorial/tutorial_jive_01.png)
7. Valitse tulosruudussa **Jive**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-jive-tutorial/tutorial_jive_02.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yksittäinen määrittäminen ja testaus Azure AD-on
Tässä osassa Määritä ja testaa Azure AD kertakirjautumisen Jive nimeltä "Britta Simon" testikäyttäjän perusteella.

Kertakirjautumisen toimimaan Azure AD on tiedettävä käyttäjälle on Jive vastaavaan käyttäjän Azure AD. Toisin sanoen suhteen Azure AD-käyttäjä ja Aiheeseen liittyvät Jive-linkki on vahvistettava.

Linkki-yhteys on muodostettu määrittämällä **käyttäjänimi** arvo arvoksi Jive **käyttäjänimi** Azure AD.

Määrittäminen ja testaaminen Azure AD kertakirjautumisen Jive kanssa, sinun on tehtävä seuraavat hakukyselyn:

1. **[Azure AD määrittäminen Single Sign](#configuring-azure-ad-single-sign-on)** - käyttäjät voivat käyttää tätä toimintoa.
2. **[Käyttäjän testata luominen Azure AD](#creating-an-azure-ad-test-user)** - Testaa Azure AD kertakirjautumisen Britta Simon kanssa.
3. **[Luomisesta Jive testata käyttäjän](#creating-a-jive-test-user)** - tapahtumista Britta Simon ovat Jive, jotka on linkitetty hän Azure AD esitys.
4. **[Määrittäminen käyttäjän valmistelu](#configuring-user-provisioning)** - jäsentäminen ottamisesta käyttöön käyttäjän valmistelu Active Directory-käyttäjän sähköpostitilejä Jive.
5. **[Käyttäjän testata määrittäminen Azure AD](#assigning-the-azure-ad-test-user)** - Britta Simon käyttämään Azure AD kertakirjautumisen käyttöön.
6. **[Testaus Single Sign](#testing-single-sign-on)** - voit tarkistaa, toimiiko määritykset.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD-Single Sign-On määrittäminen

Tässä osassa tavoitteena on Azure AD kertakirjautumisen Azure perinteinen portaalissa ottaminen käyttöön ja määrittää kertakirjautumisen Jive-sovelluksessa.

**Määrittää Azure AD kertakirjautumisen Jive, toimimalla seuraavasti:**

1. Valitse perinteinen-portaalissa integrointi **Jive** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen Single Sign** -valintaikkunan avaaminen.
     
    ![Kertakirjautumisen määrittäminen][6] 

2. **Miten Haluaisitko kirjautumaan Jive käyttäjät** -sivulla Valitse **Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-jive-tutorial/tutorial_jive_03.png) 

3. Valitse **Määritä sovelluksen asetukset** -sivulla toimimalla seuraavasti:

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-jive-tutorial/tutorial_jive_04.png) 

    a. Kirjoita **Merkki-URL** -tekstiruutuun URL-osoite käyttämä Sign käyttämällä seuraavaa kaavaa Jive sovelluksen käyttäjille: **https://\<asiakkaan nimi\>. jivecustom.com**.
    
    b. Valitse **Seuraava**
 
4. Valitse **Määritä kertakirjautumisen osoitteessa Jive** -sivulla toimimalla seuraavasti:

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-jive-tutorial/tutorial_jive_05.png)

    a. Valitse **Lataa**ja tallenna sitten tiedosto tietokoneesta.

    b. Valitse **Seuraava**.


5. Kertakirjauksen Jive asiakasympäristöön järjestelmänvalvojana.

6. Valitse ylä-valikossa "**Saml**".

    ![Sovelluksen reunassa kertakirjautumisen määrittäminen](./media/active-directory-saas-jive-tutorial/tutorial_jive_002.png)

    a. Valitse **käytössä** **Yleinen** -välilehti.

    b. Napsauta "**tallentaa kaikki saml asetukset**"-painiketta.

7. Siirry "**Idp metatietojen**"-välilehti.

    ![Sovelluksen reunassa kertakirjautumisen määrittäminen](./media/active-directory-saas-jive-tutorial/tutorial_jive_003.png)

    a. Ladatun metatietojen XML-tiedoston sisällön kopioiminen ja liittäminen **tunnistetietojen toimittaja (IDP) metatiedot** -tekstiruutuun.

    b. Napsauta "**tallentaa kaikki saml asetukset**"-painiketta. 

8. Siirry "**Käyttäjän määritteiden yhdistämismääritys**"-välilehti.

    ![Sovelluksen reunassa kertakirjautumisen määrittäminen](./media/active-directory-saas-jive-tutorial/tutorial_jive_004.png)

    a. Kirjoita **sähköpostiosoite** -tekstiruutuun kopioiminen ja liittäminen **sähköpostin** arvon määritteen nimi.

    b. Kirjoita **Ensimmäinen nimi** -tekstiruutuun kopioiminen ja liittäminen **givenname** arvon määritteen nimi.

    c-näppäinyhdistelmää. Kopioi ja liitä **Sukunimi** arvon määritteen nimi **Sukunimi** -tekstiruutuun.
    
9. Azure AD-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Seuraava**.
![Azure AD-Single Sign-On][10]

10. Valitse **Sign Vahvista** -sivulla **Valmis**.  
  ![Azure AD-Single Sign-On][11]


### <a name="creating-an-azure-ad-test-user"></a>Azure AD-testikäyttäjän luominen
Tässä osassa voit luoda testikäyttäjän kutsutaan Britta Simon perinteinen portaalissa.


![Luo Azure AD-käyttäjä][20]

**Voit luoda testikäyttäjän Azure AD-toimimalla seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen portal**, valitse vasemmassa siirtymisruudussa.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-jive-tutorial/create_aaduser_09.png) 

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Ylä-valikossa käyttäjäluettelon, näkyviin napsauttamalla **käyttäjät**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-jive-tutorial/create_aaduser_03.png) 

4. Avaa **Lisää käyttäjä** -valintaikkunan alareunassa työkalurivillä, valitse **Lisää käyttäjä**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-jive-tutorial/create_aaduser_04.png) 

5. Suorita valintaikkunan **Kerro kyseisen käyttäjän tietoja** -sivulla seuraavat toimet:  ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-jive-tutorial/create_aaduser_05.png) 

    a. Käyttäjänä tyyppi Valitse uusi käyttäjä organisaatiossa.

    b. Kirjoita käyttäjänimi- **tekstiruutuun** **BrittaSimon**.

    c-näppäinyhdistelmää. Valitse **Seuraava**.

6.  Suorita valintaikkunan **Käyttäjäprofiili** -sivulla seuraavat toimet: ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-jive-tutorial/create_aaduser_06.png) 

    a. Kirjoita **Ensimmäinen nimi** -tekstiruutuun **Britta**.  

    b. Kirjoita **Sukunimi** -tekstiruutuun tyyppi- **Simon**.

    c-näppäinyhdistelmää. Kirjoita **Näyttönimi** -tekstiruutuun **Britta Simon**.

    d. Valitse **rooli** -luettelosta **käyttäjä**.

    e. Valitse **Seuraava**.

7. Valitse **Hae tilapäinen salasana** -valintaikkunan sivulla **Luo**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-jive-tutorial/create_aaduser_07.png) 

8. **Hae tilapäinen salasana** -valintaikkunan sivulla toimimalla seuraavasti:

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-jive-tutorial/create_aaduser_08.png) 

    a. Kirjoita **Uusi salasana**arvo muistiin.

    b. Valitse **Valmis**.   



###<a name="creating-a-jive-test-user"></a>Jive testikäyttäjän luominen

Tässä osassa nimeltä Britta Simon Jive käyttäjän luominen. Toimi Jive tukihenkilöstöösi voit lisätä käyttäjiä Jive-ympäristössä.


###<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjän valmistelu Active Directory-käyttäjätilien Jive.  
Tämän toimintosarjan osana on antamaan Jive.com pyytämisestä on käyttäjän suojaustunnus.
  
Seuraavassa näyttökuvassa näkyy esimerkki liittyvät valintaikkunan Azure AD:

![Määrittää käyttäjän valmistelu] (./media/active-directory-saas-jive-tutorial/IC698794.png "Määrittää käyttäjän valmistelu")

####<a name="to-configure-user-provisioning-perform-the-following-steps"></a>Voit määrittää käyttäjän valmistelu toimimalla seuraavasti:

1.  Valitse **Määritä käyttäjän valmistelu** -valintaikkunan avaaminen **Määritä käyttäjän valmistelu** Azure-hallinta-portaalin integrointi **Jive** sovellus-sivulla.

2.  Valitse **Jive tunnistetietosi Ota käyttöön automaattinen käyttäjän valmistelu** -sivulla on seuraavat asetukset:

    1.  Kirjoita Jive tilin nimi, jolla on **Järjestelmänvalvojan** profiiliin määritetty Jive.com **Jive järjestelmänvalvojan käyttäjänimi** -tekstiruutuun.

    2.  Kirjoita **Jive järjestelmänvalvojan salasana** -tekstiruutuun tilin salasana.

    3.  Kirjoita **Jive vuokraajan URL** -tekstiruutuun Jive vuokraajan URL-osoite.

        >[AZURE.NOTE]Jive vuokraajan URL on URL-osoite, jota käytetään organisaation kirjautua Jive.  
        Yleensä URL-osoite on muotoa: **www.\< organisaation\>. jive.com**.

    4.  Valitse **Vahvista** vahvistamiseksi kokoonpanosi.

    5.  Napsauta Avaa **Vahvista** -sivulla **Seuraava** -painiketta.

3.  Valitse **Vahvista** -sivulla Tallenna kokoonpanosi valintamerkki.
  
Voit nyt luoda Testaa tilin, odota 10 minuuttia ja varmista, että tili on synkronoitu Jive.com.




### <a name="assigning-the-azure-ad-test-user"></a>Azure AD-testikäyttäjän määrittäminen

Tässä osassa voit ottaa käyttöön Britta Simon käyttämään Azure kertakirjautumisen hänen käyttöoikeuksien myöntämistä Jive.

![Määrittää käyttäjälle][200] 

**Jos haluat määrittää Britta Simon Jive, toimi seuraavasti:**

1. Perinteinen-portaalissa Avaa sovellukset-näkymässä kansio-näkymässä, valitse **sovellukset** yläreunan-valikossa.

    ![Määrittää käyttäjälle][201] 

2. Valitse sovellukset-luettelosta **Jive**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-jive-tutorial/tutorial_jive_50.png) 

3. Valitse ylä-valikossa Valitse **käyttäjiä**.

    ![Määrittää käyttäjälle][203]

4. Valitse käyttäjät-luettelosta **Britta Simon**.

5. Napsauta työkalurivin alaosassa **määrittäminen**.

    ![Määrittää käyttäjälle][205]


### <a name="testing-single-sign-on"></a>Kertakirjautumisen testaaminen

Tässä osassa voit testata Azure AD yksittäisen Sign-asetusten käyttäminen Access-paneeli.

Access-ruudussa Jive-ruutua napsauttaessasi sinun pitäisi saada automaattisesti kirjautunut-on Jive sovelluksen.


## <a name="additional-resources"></a>Lisäresursseja

* [Luettelo siitä, miten voit integroida SaaS sovellusten Azure Active Directory-opetusohjelmat](active-directory-saas-tutorial-list.md)
* [Mikä on sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-jive-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-jive-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-jive-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-jive-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-jive-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-jive-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-jive-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-jive-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-jive-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-jive-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-jive-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-jive-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-jive-tutorial/tutorial_general_205.png
