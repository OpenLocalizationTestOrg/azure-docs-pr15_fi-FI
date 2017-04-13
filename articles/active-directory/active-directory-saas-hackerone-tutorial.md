<properties
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi HackerOne | Microsoft Azure"
    description="Opettele määrittämään kertakirjautumisen Azure Active Directory- ja HackerOne välillä."
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


# <a name="tutorial-azure-active-directory-integration-with-hackerone"></a>Opetusohjelma: HackerOne Azure Active Directory-integrointi

Tässä opetusohjelmassa HackerOne integroida Azure Active Directory (Azure AD).

Azure AD-integraation HackerOne avulla voit seuraavat edut:

- Voit hallita Azure AD, jolla on pääsy HackerOne
- Voit ottaa käyttöön automaattisesti Hae kirjautunut käyttöön HackerOne (kertakirjautumisen) niiden Azure AD-tilien käyttäjille
- Voit hallita asiakkaiden yhdessä keskitetyssä sijainnissa - Azure perinteinen portal

Jos haluat tietää tarkempia tietoja SaaS sovelluksen integrointi Azure AD-kohdassa [mitä sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Edellytykset

Määrittää Azure AD-integroinnin HackerOne, tarvitset seuraavat:

- Azure tilauksen
- HackerOne single-merkki käytössä-tilauksessa


> [AZURE.NOTE] Voit esikatsella vaiheet Tässä opetusohjelmassa, Emme suosittele käyttämällä tuotantoympäristössä.


Voit esikatsella vaiheet Tässä opetusohjelmassa-noudattamalla näitä suosituksia:

- Älä käytä tuotantoympäristössä, ellei se on tarpeen.
- Jos sinulla ei ole kokeiluversion Azure AD-ympäristössä, voit hankkia yksi kuukausi kokeiluversion [tähän](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Skenaarion kuvaus
Tässä opetusohjelmassa Määritä ja testaa Azure AD kertakirjautumisen testiympäristössä.  
Tässä opetusohjelmassa kuvatut skenaarion kuuluu kaksi tärkeimmät rakenneosien:

1. HackerOne lisääminen valikoimasta
2. Yksittäinen määrittäminen ja testaus Azure AD-on


## <a name="adding-hackerone-from-the-gallery"></a>HackerOne lisääminen valikoimasta
Integroida HackerOne Azure AD-sinun on lisättävä HackerOne valikoimasta hallitun SaaS sovellusluettelo.

**Jos haluat lisätä HackerOne valikoimasta, toimi seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen portal**, valitse vasemmassa siirtymisruudussa. 

    ![Active Directory][1]

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset][2]

4. Valitse sivun alareunassa **Lisää** .

    ![Sovellukset][3]

5. Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

![Sovellukset][4]

6. Kirjoita hakuruutuun **HackerOne**.

![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_01.png)

7. Valitse tulosruudussa **HackerOne**ja valitse **Valmis** , voit lisätä sovelluksen.


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yksittäinen määrittäminen ja testaus Azure AD-on
Seuraavaksi Määritä ja testaa Azure AD kertakirjautumisen HackerOne nimeltä "Britta Simon" testikäyttäjän perusteella.

Kertakirjautumisen toimimaan Azure AD on tiedettävä vastaavaan käyttäjän HackerOne, Azure AD-käyttäjälle. Toisin sanoen suhteen Azure AD-käyttäjä ja Aiheeseen liittyvät HackerOne-linkki on vahvistettava.  
Linkki-yhteys on muodostettu määrittämällä **käyttäjänimi** arvo arvoksi HackerOne **käyttäjänimi** Azure AD.

Määrittäminen ja testaaminen Azure AD kertakirjautumisen HackerOne kanssa, sinun on tehtävä seuraavat hakukyselyn:

1. **[Azure AD määrittäminen Single Sign](#configuring-azure-ad-single-single-sign-on)** - käyttäjät voivat käyttää tätä toimintoa.
2. **[Käyttäjän testata luominen Azure AD](#creating-an-azure-ad-test-user)** - Testaa Azure AD kertakirjautumisen Britta Simon kanssa.
3. **[Luomisesta HackerOne testata käyttäjän](#creating-a-hackerone-test-user)** - tapahtumista Britta Simon ovat sertifioi, jotka on linkitetty hän Azure AD esitys.
4. **[Käyttäjän testata määrittäminen Azure AD](#assigning-the-azure-ad-test-user)** - Britta Simon käyttämään Azure AD kertakirjautumisen käyttöön.
5. **[Testaus Single Sign](#testing-single-sign-on)** - voit tarkistaa, toimiiko määritykset.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD-Single Sign-On määrittäminen

Seuraavaksi otetaan käyttöön Azure AD kertakirjautumisen perinteinen portaalissa ja määrittämään kertakirjautumisen HackerOne-sovelluksessa.

Tämän toimintosarjan osana on base-64-koodattu sertifikaattitiedosto luomiseen.  
Jos et ole tottunut näin, katso, [miten voit muuntaa binaariluvun varmenteen tekstitiedostoon](http://youtu.be/PlgrzUZ-Y1o).

**Määrittää Azure AD kertakirjautumisen HackerOne, toimimalla seuraavasti:**

1. Valitse Azure perinteinen portaalissa integrointi **HackerOne** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen Single Sign** -valintaikkunan avaaminen.

    ![Kertakirjautumisen määrittäminen][6] 

2. **Miten käyttäjät voivat kirjautua HackerOne haluat käyttää** sivulla Valitse **Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_03.png) 

3. Valitse **Määritä sovelluksen asetukset** -sivulla suorittamalla seuraavat vaiheet ja valitse sitten **Seuraava**:

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_04.png) 


    a. Kirjoita **Merkki-URL** -tekstiruutuun URL-osoite käyttämä Sign käyttämällä seuraavaa kaavaa HackerOne sovelluksen käyttäjille: **"https://hackerone.com/\<yrityksen nimi\>/authentication"**. 

    b. HackerOne tukeen kautta [support@hackerone.com](mailto:support@hackerone.com) saat vuokraajan URL-osoite, jos et ole varma.

    c-näppäinyhdistelmää. Kirjoita **tunnus** -tekstiruutuun vuokraajan URL-osoite. 

    d. Valitse **Seuraava**.


4. Valitse **Määritä kertakirjautumisen osoitteessa HackerOne** -sivulla suorittamalla seuraavat vaiheet ja valitse sitten **Seuraava**:

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_05.png) 

    a. Valitse **Lataa**ja tallenna sitten tiedosto tietokoneesta.

    b. Valitse **Seuraava**.


1. Kertakirjauksen HackerOne asiakasympäristöön järjestelmänvalvojana.

1. Valitse ylä-valikossa **asetukset**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_001.png) 

1. Siirry "**todennus**" ja "**Lisää SAML asetukset**".

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_003.png) 


1. Suorita **SAML asetukset** -valintaikkunassa seuraavat toimet:

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_004.png) 

    a. Kirjoita **Sähköpostitoimialueelle** -tekstiruutuun rekisteröity toimialue.

    b. Azure perinteinen portaalissa **Sign-palvelun URL**kopioi ja liitä ne yhteen merkki-URL-tekstiruutuun.

    c-näppäinyhdistelmää. Luo **base-64-koodattu** tiedosto ladattu varmennetta.  

       >[AZURE.TIP] Lisätietoja on artikkelissa [muuntaa binaariluvun varmenteen tekstitiedostoon](http://youtu.be/PlgrzUZ-Y1o)
    
    d. Avaa base-64-koodattu varmennetta Muistiossa, kopioi se sisällön Leikepöydän ja liitä se, **X509 varmenteen** tekstiruutu.

    e. Valitse **Tallenna**


1. Valitse todennusasetukset-valintaikkunassa seuraavasti:

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_005.png) 

    a. Valitse **Suorita testi**.

    b. Jos **tila** arvon kentän yhtä suuri kuin **testata viimeksi tila: luotu**, tukihenkilöstöltä HackerOne kautta [support@hackerone.com](mailto:support@hackerone.com) pyytää asetusten tarkastelu.


6. Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Seuraava**.

    ![Azure AD-Single Sign-On][10]

7. Valitse **Sign Vahvista** -sivulla **Valmis**.  
 
    ![Azure AD-Single Sign-On][11]




### <a name="creating-an-azure-ad-test-user"></a>Azure AD-testikäyttäjän luominen

Seuraavaksi voit luoda testikäyttäjän kutsutaan Britta Simon perinteinen portaalissa.  

![Luo Azure AD-käyttäjä][20]

**Voit luoda suojatun toimittaa testikäyttäjän Azure AD-toimimalla seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen portal**, valitse vasemmassa siirtymisruudussa.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-hackerone-tutorial/create_aaduser_09.png) 

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Ylä-valikossa käyttäjäluettelon, näkyviin napsauttamalla **käyttäjät**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-hackerone-tutorial/create_aaduser_03.png) 

4. Avaa **Lisää käyttäjä** -valintaikkunan alareunassa työkalurivillä, valitse **Lisää käyttäjä**.
    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-hackerone-tutorial/create_aaduser_04.png) 

5. Tee valintaikkunassa **Kerro kyseisen käyttäjän tietoja** -sivulla seuraavasti:

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-hackerone-tutorial/create_aaduser_05.png) 

    a. Käyttäjänä tyyppi Valitse uusi käyttäjä organisaatiossa.

    b. Kirjoita käyttäjänimi- **tekstiruutuun** **BrittaSimon**.

    c-näppäinyhdistelmää. Valitse **Seuraava**.

6.  Suorita **Käyttäjäprofiili** -sivulla seuraavat toimet:

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-hackerone-tutorial/create_aaduser_06.png) 

    a. Kirjoita **Ensimmäinen nimi** -tekstiruutuun **Britta**.  

    b. Kirjoita **Sukunimi** -tekstiruutuun tyyppi- **Simon**.

    c-näppäinyhdistelmää. Kirjoita **Näyttönimi** -tekstiruutuun **Britta Simon**.

    d. Valitse **rooli** -luettelosta **käyttäjä**.

    e. Valitse **Seuraava**.

7. Valitse **Hae tilapäinen salasana** -valintaikkunan sivulla **Luo**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-hackerone-tutorial/create_aaduser_07.png) 

8. **Hae tilapäinen salasana** -valintaikkunan sivulla toimimalla seuraavasti:

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-hackerone-tutorial/create_aaduser_08.png) 

    a. Kirjoita **Uusi salasana**arvo muistiin.

    b. Valitse **Valmis**.   


### <a name="creating-a-hackerone-test-user"></a>HackerOne testikäyttäjän luominen

Seuraavaksi kutsutaan Britta Simon HackerOne käyttäjän luominen. HackerOne tukee vain perustuvan valmistelu, jossa on käytössä oletusarvoisesti.

Ei ole toimi puolestasi sisältö. Kun käytät HackerOne, uusi käyttäjä luodaan, jos sitä ei ole vielä. [Azure AD-Single Sign-On määrittäminen](#configuring-azure-ad-single-single-sign-on).

> [AZURE.NOTE] Jos haluat luoda käyttäjän manuaalisesti, on sertifioi-tukeen.




### <a name="assigning-the-azure-ad-test-user"></a>Azure AD-testikäyttäjän määrittäminen

Seuraavaksi voit ottaa käyttöön Britta Simon käyttämään Azure kertakirjautumisen hänen käyttöoikeuksien myöntämistä HackerOne.

![Määrittää käyttäjälle][200] 

**Jos haluat määrittää Britta Simon HackerOne, toimi seuraavasti:**

1. Azure perinteinen-portaalissa Avaa sovellukset-näkymässä kansio-näkymässä, valitse **sovellukset** yläreunan-valikossa.

    ![Määrittää käyttäjälle][201] 

2. Valitse sovellukset-luettelosta **HackerOne**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-hackerone-tutorial/tutorial_hackerone_50.png) 

1. Valitse ylä-valikossa Valitse **käyttäjiä**.

    ![Määrittää käyttäjälle][203] 

1. Valitse käyttäjät-luettelosta **Britta Simon**.

2. Napsauta työkalurivin alaosassa **määrittäminen**.

    ![Määrittää käyttäjälle][205]



### <a name="testing-single-sign-on"></a>Kertakirjautumisen testaaminen

Lopuksi voit testata Azure AD yksittäisen Sign-asetusten käyttäminen Access-paneeli.  
Access-ruudussa HackerOne-ruutua napsauttaessasi sinun pitäisi saada automaattisesti kirjautunut-on HackerOne sovelluksen.


## <a name="additional-resources"></a>Lisäresursseja

* [Luettelo siitä, miten voit integroida SaaS sovellusten Azure Active Directory-opetusohjelmat](active-directory-saas-tutorial-list.md)
* [Mikä on sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-hackerone-tutorial/tutorial_general_205.png