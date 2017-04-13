<properties
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi MOVEit siirto | Microsoft Azure"
    description="Opettele määrittämään kertakirjautumisen Azure Active Directory- ja MOVEit siirto välillä."
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
    ms.date="10/18/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-moveit-transfer"></a>Opetusohjelma: MOVEit siirto Azure Active Directory-integrointi

Tässä opetusohjelmassa tavoitteena on, kuinka voit integroida MOVEit siirto Azure Active Directory (Azure AD).

Azure AD-integraation MOVEit siirron, jonka avulla seuraavat edut:

- Voit hallita Azure AD, jolla on pääsy MOVEit siirtäminen
- Voit ottaa käyttöön automaattisesti Hae kirjautunut sisään MOVEit siirtämisen (kertakirjautumisen) käyttäjille heidän Azure AD-tilien kanssa
- Voit hallita asiakkaiden yhdessä keskitetyssä sijainnissa - Azure perinteinen portal

Jos haluat tietää tarkempia tietoja SaaS sovelluksen integrointi Azure AD-kohdassa [mitä sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Edellytykset

Määrittää Azure AD-integroinnin MOVEit siirron, tarvitset seuraavat:

- Azure AD-tilaus
- MOVEit siirto yhden-merkki käytössä-tilauksessa


> [AZURE.NOTE] Voit esikatsella vaiheet Tässä opetusohjelmassa, Emme suosittele käyttämällä tuotantoympäristössä.


Voit esikatsella vaiheet Tässä opetusohjelmassa-noudattamalla näitä suosituksia:

- Älä käytä tuotantoympäristössä, ellei se on tarpeen.
- Jos sinulla ei ole kokeiluversion Azure AD-ympäristössä, voit hankkia yksi kuukausi kokeiluversion [tähän](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Skenaarion kuvaus
Tässä opetusohjelmassa tavoitteena on, jotta voit testata Azure AD kertakirjautumisen testiympäristössä.

Tässä opetusohjelmassa kuvatut skenaarion kuuluu kaksi tärkeimmät rakenneosien:

1. MOVEit siirto lisääminen valikoimasta
2. Yksittäinen määrittäminen ja testaus Azure AD-on


## <a name="adding-moveit-transfer-from-the-gallery"></a>MOVEit siirto lisääminen valikoimasta
Voit määrittää MOVEit siirto integroida Azure AD-haluat lisätä MOVEit siirto valikoimasta hallitun SaaS sovellusluettelo.

**Lisää MOVEit siirto valikoimasta, toimi seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen Portal**, valitse vasemmassa siirtymisruudussa. 

    ![Active Directory][1]

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .
    
    ![Sovellukset][2]

4. Valitse sivun alareunassa **Lisää** .
    
    ![Sovellukset][3]

5. Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Sovellukset][4]

6. Kirjoita hakuruutuun **MOVEit siirto**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_01.png)

7. Tulokset-ruudussa Valitse **MOVEit Siirrä**ja valitse sitten **Valmis** , voit lisätä sovelluksen.

    ![Sovelluksen valitseminen valikoimasta](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_0001.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yksittäinen määrittäminen ja testaus Azure AD-on
Tässä osassa on noudattamalla voit määrittää ja testaa Azure AD kertakirjautumisen MOVEit siirron nimeltä "Britta Simon" testikäyttäjän perusteella.

Kertakirjautumisen toimimaan Azure AD on tiedettävä vastaavaan käyttäjän MOVEit siirron, Azure AD-käyttäjälle. Toisin sanoen linkki Azure AD-käyttäjä ja Aiheeseen liittyvät MOVEit siirron suhteen on vahvistettava.

Linkki-yhteys on muodostettu määrittämällä **käyttäjänimi** arvo arvoksi MOVEit siirto **käyttäjänimi** Azure AD.

Määrittäminen ja testaaminen Azure AD kertakirjautumisen MOVEit siirron, sinun on tehtävä seuraavat hakukyselyn:

1. **[Azure AD määrittäminen Single Sign](#configuring-azure-ad-single-single-sign-on)** - käyttäjät voivat käyttää tätä toimintoa.
2. **[Käyttäjän testata luominen Azure AD](#creating-an-azure-ad-test-user)** - Testaa Azure AD kertakirjautumisen Britta Simon kanssa.
3. **[Käyttäjän luominen MOVEit siirron testata](#creating-a-moveit-transfer-test-user)** - on tapahtumista Britta Simon MOVEit siirron, jotka on linkitetty hän Azure AD-esittäminen.
4. **[Käyttäjän testata määrittäminen Azure AD](#assigning-the-azure-ad-test-user)** - Britta Simon käyttämään Azure AD kertakirjautumisen käyttöön.
5. **[Testaus Single Sign](#testing-single-sign-on)** - voit tarkistaa, toimiiko määritykset.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD kertakirjautumisen määrittäminen

Tässä osassa Azure AD kertakirjautumisen perinteinen portaalissa käyttöön ja määrittäminen kertakirjautumisen MOVEit siirto-sovellus.

**Voit määrittää Azure AD kertakirjautumisen MOVEit siirron toimimalla seuraavasti:**

1. Valitse perinteinen-portaalissa integrointi **MOVEit siirto** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen Single Sign** -valintaikkunan avaaminen.
     
    ![Kertakirjautumisen määrittäminen][6] 

2. **Miten käyttäjät voivat kirjautua MOVEit siirron haluat käyttää** sivulla Valitse **Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.
    
    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_03.png)

3. Valitse **Määritä sovelluksen asetukset** -sivulla suorittamalla seuraavat vaiheet ja valitse **Seuraava**:

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_04.png)

    a. Kirjoita Kirjaudu sisään URL-osoite toimialueen kanssa käytettäväksi **Merkki-URL** -tekstiruutuun.

    b. Kirjoita **tunnus** -tekstiruutuun kohteen tunnus URL-osoite.

    c-näppäinyhdistelmää. Kirjoita **Vastaus URL** -tekstiruutuun enebled vahvistus kuluttaja käyttöliittymän URL-osoite.

    d. Valitse **Seuraava**

    > [AZURE.NOTE] Huomaa, että sinulla on Päivitä arvot todellisen merkki-URL-Osoitteen ja tunnus. Saat nämä arvot-voit viitata vaihe 8 lisätietoja tai ota yhteyttä [MOVEit siirto](https://www.ipswitch.com/support/technical-support).

4. Valitse **Määritä kertakirjautumisen MOVEit siirtoon** -sivulla suorittamalla seuraavat vaiheet ja valitse **Seuraava**:

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_05.png)

    a. Valitse **Lataa metatiedot**ja tallenna sitten tiedosto tietokoneesta.

    b. Valitse **Seuraava**.

5. Kertakirjauksen MOVEit siirto-asiakasympäristöön järjestelmänvalvojana.

6. Valitse vasemmassa siirtymisruudussa **asetukset**.

    ![Määrittää yksittäisen Sign-On-sovelluksen puoli](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_000.png)

7. Valitse **Yksittäinen rekisteröityminen** linkki alla **suojauskäytäntöjä-> käyttäjän**.

    ![Määrittää yksittäisen Sign-On-sovelluksen puoli](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_001.png)

8. Valitse Lataa metatietojen asiakirja metatietojen URL-osoite-linkki.

    ![Määrittää yksittäisen Sign-On-sovelluksen puoli](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_002.png)

    - Tarkista **tunnus** vastaa **tunnus** -vaihe 3.
    
    - Tarkista **AssertionConsumerService** sijainnin URL-osoite vastaa **Vastaa URL-osoite** -vaihe 3.

    ![Määrittää yksittäisen Sign-On-sovelluksen puoli](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_007.png)

9. Voit lisätä uuden liitetty tunnistetietojen palveluntarjoaja **Lisää tunnistetietojen toimittaja** valitsemalla.

    ![Määrittää yksittäisen Sign-On-sovelluksen puoli](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_003.png)

10. Valitse **Selaa** ja valitse metatieto-tiedosto, jonka voit ladata vaiheessa 4 ja valitse sitten **Lisää tunnistetietojen toimittaja** ladattu tiedosto ladataan. 

    ![Määrittää yksittäisen Sign-On-sovelluksen puoli](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_004.png)

11. Valitse "**Kyllä**" **käytössä** **Muokkaa liitetty tunnistetietojen toimittaja...** asetussivu ja valitse **Tallenna**.

     ![Määrittää yksittäisen Sign-On-sovelluksen puoli](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_005.png)

12. Seuraavien toimien suorittamiseen **Muokkaa liitetty tunnistetietojen toimittaja Käyttäjäasetukset** -sivulla ja valitse sitten **Tallenna**.

    a. Valitse **SAML NameID** **kirjautumisnimi**.

    b. Valitse **Muu** **koko** nimenä ja **määritteen nimi** tekstiruutuun siirtää arvo: http://schemas.microsoft.com/identity/claims/displayname.

    c-näppäinyhdistelmää. Valitse **Muut** **sähköpostina** ja **määritteen nimi** tekstiruutuun siirtää arvo: http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress.

    d. Valitse **Kyllä** **Rekisteröityminen-tilin luominen automaattisesti**.

    e. Napsauta **Tallenna** -painiketta.

    ![Määrittää yksittäisen Sign-On-sovelluksen puoli](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_006.png)

13. Perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Seuraava**.
    
    ![Azure AD-Single Sign-On][10]

14. Valitse **Sign Vahvista** -sivulla **Valmis**.  
    
    ![Azure AD-Single Sign-On][11]



### <a name="creating-an-azure-ad-test-user"></a>Azure AD-testikäyttäjän luominen
Tässä osassa tavoitteena on luotava testikäyttäjän kutsutaan Britta Simon perinteinen portaalissa.

![Luo Azure AD-käyttäjä][20]

**Voit luoda testikäyttäjän Azure AD-toimimalla seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen Portal**, valitse vasemmassa siirtymisruudussa.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-moveittransfer-tutorial/create_aaduser_09.png)

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Ylä-valikossa käyttäjäluettelon, näkyviin napsauttamalla **käyttäjät**.
    
    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-moveittransfer-tutorial/create_aaduser_03.png)

4. Avaa **Lisää käyttäjä** -valintaikkunan alareunassa työkalurivillä, valitse **Lisää käyttäjä**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-moveittransfer-tutorial/create_aaduser_04.png)

5. Tee valintaikkunassa **Kerro kyseisen käyttäjän tietoja** -sivulla seuraavasti:

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-moveittransfer-tutorial/create_aaduser_05.png)

    a. Käyttäjänä tyyppi Valitse uusi käyttäjä organisaatiossa.

    b. Kirjoita käyttäjänimi- **tekstiruutuun** **BrittaSimon**.

    c-näppäinyhdistelmää. Valitse **Seuraava**.

6.  Suorita **Käyttäjäprofiili** -sivulla seuraavat toimet:
    
    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-moveittransfer-tutorial/create_aaduser_06.png)

    a. Kirjoita **Ensimmäinen nimi** -tekstiruutuun **Britta**.  

    b. Kirjoita **Sukunimi** -tekstiruutuun tyyppi- **Simon**.

    c-näppäinyhdistelmää. Kirjoita **Näyttönimi** -tekstiruutuun **Britta Simon**.

    d. Valitse **rooli** -luettelosta **käyttäjä**.

    e. Valitse **Seuraava**.

7. Valitse **Hae tilapäinen salasana** -valintaikkunan sivulla **Luo**.
    
    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-moveittransfer-tutorial/create_aaduser_07.png)

8. **Hae tilapäinen salasana** -valintaikkunan sivulla toimimalla seuraavasti:
    
    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-moveittransfer-tutorial/create_aaduser_08.png)

    a. Kirjoita **Uusi salasana**arvo muistiin.

    b. Valitse **Valmis**.   



### <a name="creating-a-moveit-transfer-test-user"></a>MOVEit siirto-testikäyttäjän luominen

Tässä osassa tavoitteena on nimeltään Britta Simon MOVEit siirto käyttäjän luominen. MOVEit siirto tukee juuri perustuvan valmistelu, jotka on otettu käyttöön.

Ei ole toimi puolestasi sisältö. Uusi käyttäjä luodaan yritys käyttää MOVEit siirron, jos sitä ei ole vielä aikana.

> [AZURE.NOTE] Jos haluat luoda luodun käyttäjän manuaalisesti, on MOVEit siirto-tukeen.


### <a name="assigning-the-azure-ad-test-user"></a>Azure AD-testikäyttäjän määrittäminen

Tässä osassa tavoitteena on Britta Simon käyttämään Azure kertakirjautumisen hänen käyttöoikeuden myöntämisestä MOVEit siirto ottaminen käyttöön.
    
![Määrittää käyttäjälle][200]

**Jos haluat määrittää Britta Simon MOVEit siirron, toimi seuraavasti:**

1. Perinteinen-portaalissa Avaa sovellukset-näkymässä kansio-näkymässä, valitse **sovellukset** yläreunan-valikossa.
    
    ![Määrittää käyttäjälle][201]

2. Valitse sovellukset-luettelosta **MOVEit siirto**.
    
    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_50.png)

3. Valitse ylä-valikossa Valitse **käyttäjiä**.
    
    ![Määrittää käyttäjälle][203]

4. Valitse käyttäjät-luettelosta **Britta Simon**.

5. Napsauta työkalurivin alaosassa **määrittäminen**.
    
    ![Määrittää käyttäjälle][205]

### <a name="testing-single-sign-on"></a>Kertakirjautumisen testaaminen

Tässä osassa tavoitteena on Testaa Azure AD yksittäisen Sign-asetusten käyttäminen Access-paneeli.
 
Access-ruudussa MOVEit siirto-ruutua napsauttaessasi voit olisi Hae automaattisesti kirjautunut sisään MOVEit siirto-sovellukseen.

## <a name="additional-resources"></a>Lisäresursseja

* [Luettelo siitä, miten voit integroida SaaS sovellusten Azure Active Directory-opetusohjelmat](active-directory-saas-tutorial-list.md)
* [Mikä on sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_205.png
