<properties
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi eteen | Microsoft Azure"
    description="Opettele määrittämään kertakirjautumisen Azure Active Directoryn ja edestä."
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
    ms.date="10/24/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-front"></a>Opetusohjelma: Eteen Azure Active Directory-integrointi

Tässä opetusohjelmassa tavoitteena on, kuinka voit integroida eteen Azure Active Directory (Azure AD).

Azure AD-integraation eteen, jonka avulla seuraavat edut:

- Voit hallita Azure AD, kenellä on oikeus eteen
- Voit ottaa käyttöön automaattisesti Hae kirjautunut sisään eteen (kertakirjautumisen) niiden Azure AD-tilien käyttäjille
- Voit hallita asiakkaiden yhdessä keskitetyssä sijainnissa - Azure perinteinen portal

Jos haluat tietää tarkempia tietoja SaaS sovelluksen integrointi Azure AD-kohdassa [mitä sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Edellytykset

Määrittää Azure AD-integroinnin eteen, tarvitset seuraavat:

- Azure AD-tilaus
- Eteen single-merkki käytössä-tilauksessa


> [AZURE.NOTE] Voit esikatsella vaiheet Tässä opetusohjelmassa, Emme suosittele käyttämällä tuotantoympäristössä.


Voit esikatsella vaiheet Tässä opetusohjelmassa-noudattamalla näitä suosituksia:

- Älä käytä tuotantoympäristössä, ellei se on tarpeen.
- Jos sinulla ei ole kokeiluversion Azure AD-ympäristössä, voit hankkia yksi kuukausi kokeiluversion [tähän](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Skenaarion kuvaus
Tässä opetusohjelmassa tavoitteena on, jotta voit testata Azure AD kertakirjautumisen testiympäristössä.

Tässä opetusohjelmassa kuvatut skenaarion kuuluu kaksi tärkeimmät rakenneosien:

1. Eteen lisääminen valikoimasta
2. Yksittäinen määrittäminen ja testaus Azure AD-on


## <a name="adding-front-from-the-gallery"></a>Eteen lisääminen valikoimasta
Voit määrittää eteen integroida Azure AD-haluat lisätä eteen valikoimasta hallitun SaaS sovellusluettelo.

**Lisää eteen valikoimasta, toimi seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen Portal**, valitse vasemmassa siirtymisruudussa. 

    ![Active Directory][1]

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .
    
    ![Sovellukset][2]

4. Valitse sivun alareunassa **Lisää** .
    
    ![Sovellukset][3]

5. Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Sovellukset][4]

6. Kirjoita hakuruutuun **eteen**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-front-tutorial/tutorial_front_01.png)

7. Tulokset-ruudussa Valitse **Postikortin etupuoli**ja valitse sitten **Valmis** , voit lisätä sovelluksen.

    ![Sovelluksen valitseminen valikoimasta](./media/active-directory-saas-front-tutorial/tutorial_front_0001.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yksittäinen määrittäminen ja testaus Azure AD-on
Tässä osassa tavoitteena on noudattamalla voit määrittää ja testaa Azure AD kertakirjautumisen nimeltä "Britta Simon" testi käyttäjään perustuva eteen.

Kertakirjautumisen toimimaan Azure AD on tiedettävä vastaavaan käyttäjän edessä Azure AD käyttäjä. Toisin sanoen linkki suhteen Azure AD-käyttäjä ja edessä liittyvä käyttäjä on vahvistettava.

Linkki-yhteys on muodostettu määrittämällä **käyttäjänimi** arvo arvoksi **käyttäjänimi** edessä Azure AD.

Määrittäminen ja testaaminen Azure AD kertakirjautumisen eteen kanssa, on täytettävä seuraavat hakukyselyn:

1. **[Azure AD määrittäminen Single Sign](#configuring-azure-ad-single-single-sign-on)** - käyttäjät voivat käyttää tätä toimintoa.
2. **[Käyttäjän testata luominen Azure AD](#creating-an-azure-ad-test-user)** - Testaa Azure AD kertakirjautumisen Britta Simon kanssa.
3. **[Luomisesta eteen testata käyttäjän](#creating-a-front-test-user)** - on tapahtumista Britta Simon edessä, jotka on linkitetty hän Azure AD esitys.
4. **[Käyttäjän testata määrittäminen Azure AD](#assigning-the-azure-ad-test-user)** - Britta Simon käyttämään Azure AD kertakirjautumisen käyttöön.
5. **[Testaus Single Sign](#testing-single-sign-on)** - voit tarkistaa, toimiiko määritykset.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD kertakirjautumisen määrittäminen

Tässä osassa Azure AD kertakirjautumisen perinteinen portaalissa ottaminen käyttöön ja määritä kertakirjautumisen eteen-sovelluksessa.

**Määrittää Azure AD kertakirjautumisen eteen, toimimalla seuraavasti:**

1. Valitse perinteinen-portaalissa **eteen** integrointi sivulla **määrittäminen kertakirjautumisen** **Määrittäminen Single Sign** -valintaikkunan avaaminen.
     
    ![Kertakirjautumisen määrittäminen][6] 

2. **Miten haluat kirjautua eteen käyttäjät** -sivulla Valitse **Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.
    
    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-front-tutorial/tutorial_front_03.png)

3. Jos haluat sovelluksen määrittäminen **IDP aloittanut tila**, seuraavien toimien **App-asetusten määrittäminen** -sivulla ja valitse **Seuraava**:

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-front-tutorial/tutorial_front_04.png)

    a. Kirjoita **tunnus** -tekstiruutuun URL-osoite, käyttämällä seuraavaa kaavaa:`https://<company name>.frontapp.com`

    b. Kirjoita **Vastaus URL** -tekstiruutuun URL-osoite, käyttämällä seuraavaa kaavaa:`https://<company name>.frontapp.com/sso/saml/callback`

    c-näppäinyhdistelmää. Valitse **Seuraava**

4. Jos haluat määrittää sovelluksen **SP aloittanut tila** -valintaikkunan **App-asetusten määrittäminen** -sivulla, valitse **"Näytä lisäasetukset (valinnainen)"** ja kirjoita **Merkki-URL-osoite** ja valitse **Seuraava**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-front-tutorial/tutorial_front_05.png)

    a. Kirjoita **Merkki-URL** -tekstiruutuun URL-osoite, käyttämällä seuraavaa kaavaa:`https://<company name>.frontapp.com`

    b. Valitse **Seuraava**

    > [AZURE.NOTE] Huomaa, että ne eivät ole todellisia arvoja. Voit joutua päivittämään nämä arvot Todellinen merkki-URL-osoite, tunnus ja vastaa URL-osoite. Saat nämä arvot-voit viitata **vaiheeseen 12** lisätietoja tai ota yhteyttä eteen [support@frontapp.com](emailTo:support@frontapp.com).

5. Valitse **Määritä kertakirjautumisen edessä** -sivulla suorittamalla seuraavat vaiheet ja valitse **Seuraava**:

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-front-tutorial/tutorial_front_06.png)

    a. Valitse **Lataa**ja tallenna sitten tiedosto tietokoneesta.

    b. Valitse **Seuraava**.

6. Kertakirjauksen edestä asiakasympäristöön järjestelmänvalvojana.

7. Siirry **asetukset (hammas kuvake vasemmanpuoleinen sivupalkki alareunassa) > Asetukset**.

    ![Määrittää yksittäisen Sign-On-sovelluksen puoli](./media/active-directory-saas-front-tutorial/tutorial_front_000.png)

8. Napsauta **Kertakirjautuminen** -linkkiä.

    ![Määrittää yksittäisen Sign-On-sovelluksen puoli](./media/active-directory-saas-front-tutorial/tutorial_front_001.png)

9. Valitse avattavasta luettelosta, **Kertakirjautuminen** **SAML** .

    ![Määrittää yksittäisen Sign-On-sovelluksen puoli](./media/active-directory-saas-front-tutorial/tutorial_front_002.png)

10. **Aloituskohtaa** tekstiruudun valitseminen **Sign-palvelun URL** arvon Azure AD-sovelluksen määritystoimintoa.

    ![Määrittää yksittäisen Sign-On-sovelluksen puoli](./media/active-directory-saas-front-tutorial/tutorial_front_003.png)

11. Ladatun sertifikaattitiedosto sisällön kopioiminen ja liittäminen **allekirjoitukseen varmenteen** tekstiruutu.

    ![Määrittää yksittäisen Sign-On-sovelluksen puoli](./media/active-directory-saas-front-tutorial/tutorial_front_004.png)

12. Näitä URL-osoitteet vastaavat vaiheessa 3 asetusten vahvistaminen

    ![Määrittää yksittäisen Sign-On-sovelluksen puoli](./media/active-directory-saas-front-tutorial/tutorial_front_005.png)

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

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-front-tutorial/create_aaduser_09.png)

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Ylä-valikossa käyttäjäluettelon, näkyviin napsauttamalla **käyttäjät**.
    
    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-front-tutorial/create_aaduser_03.png)

4. Avaa **Lisää käyttäjä** -valintaikkunan alareunassa työkalurivillä, valitse **Lisää käyttäjä**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-front-tutorial/create_aaduser_04.png)

5. Tee valintaikkunassa **Kerro kyseisen käyttäjän tietoja** -sivulla seuraavasti:

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-front-tutorial/create_aaduser_05.png)

    a. Käyttäjänä tyyppi Valitse uusi käyttäjä organisaatiossa.

    b. Kirjoita käyttäjänimi- **tekstiruutuun** **BrittaSimon**.

    c-näppäinyhdistelmää. Valitse **Seuraava**.

6.  Suorita **Käyttäjäprofiili** -sivulla seuraavat toimet:
    
    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-front-tutorial/create_aaduser_06.png)

    a. Kirjoita **Ensimmäinen nimi** -tekstiruutuun **Britta**.  

    b. Kirjoita **Sukunimi** -tekstiruutuun tyyppi- **Simon**.

    c-näppäinyhdistelmää. Kirjoita **Näyttönimi** -tekstiruutuun **Britta Simon**.

    d. Valitse **rooli** -luettelosta **käyttäjä**.

    e. Valitse **Seuraava**.

7. Valitse **Hae tilapäinen salasana** -valintaikkunan sivulla **Luo**.
    
    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-front-tutorial/create_aaduser_07.png)

8. **Hae tilapäinen salasana** -valintaikkunan sivulla toimimalla seuraavasti:
    
    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-front-tutorial/create_aaduser_08.png)

    a. Kirjoita **Uusi salasana**arvo muistiin.

    b. Valitse **Valmis**.   



### <a name="creating-a-front-test-user"></a>Etu-testikäyttäjän luominen

Tässä osassa tavoitteena on voit lisätä käyttäjiä edestä tili nimeltä Britta Simon Front.Please käsitteleminen edestä tukihenkilöstölle käyttäjän luominen.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD-testikäyttäjän määrittäminen

Tässä osassa tavoitteena on Britta Simon käyttämään Azure kertakirjautumisen hänen käyttöoikeuksien myöntäminen eteen ottaminen käyttöön.
    
![Määrittää käyttäjälle][200]

**Jos haluat määrittää Britta Simon eteen, toimi seuraavasti:**

1. Perinteinen-portaalissa Avaa sovellukset-näkymässä kansio-näkymässä, valitse **sovellukset** yläreunan-valikossa.
    
    ![Määrittää käyttäjälle][201]

2. Valitse sovellukset-luettelosta **eteen**.
    
    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-front-tutorial/tutorial_front_50.png)

1. Valitse ylä-valikossa Valitse **käyttäjiä**.
    
    ![Määrittää käyttäjälle][203]

1. Valitse käyttäjät-luettelosta **Britta Simon**.

2. Napsauta työkalurivin alaosassa **määrittäminen**.
    
    ![Määrittää käyttäjälle][205]



### <a name="testing-single-sign-on"></a>Kertakirjautumisen testaaminen

Tässä osassa tavoitteena on Testaa Azure AD yksittäisen Sign-asetusten käyttäminen Access-paneeli.
 
Access-ruudussa eteen-ruutua napsauttaessasi voit olisi Hae automaattisesti kirjautunut sisään eteen-sovellukseen.


## <a name="additional-resources"></a>Lisäresursseja

* [Luettelo siitä, miten voit integroida SaaS sovellusten Azure Active Directory-opetusohjelmat](active-directory-saas-tutorial-list.md)
* [Mikä on sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-front-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-front-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-front-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-front-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-front-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-front-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-front-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-front-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-front-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-front-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-front-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-front-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-front-tutorial/tutorial_general_205.png
