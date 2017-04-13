<properties
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi Lifesize Cloud | Microsoft Azure"
    description="Opettele määrittämään kertakirjautumisen Azure Active Directory- ja Lifesize Cloud välillä."
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
    ms.date="10/04/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-lifesize-cloud"></a>Opetusohjelma: Lifesize Cloud Azure Active Directory-integrointi

Tässä opetusohjelmassa kerrotaan, miten voit integroida Lifesize Cloud Azure Active Directory (Azure AD).

Azure AD-integraation Lifesize pilven avulla voit seuraavat edut:

- Voit hallita Azure AD, joilla on käyttöoikeus Lifesize pilveen
- Voit ottaa käyttöön automaattisesti Hae kirjautunut sisään Lifesize pilveen (kertakirjautumisen) käyttäjille heidän Azure AD-tilien kanssa
- Voit hallita asiakkaiden yhdessä keskitetyssä sijainnissa - Azure perinteinen portal

Jos haluat tietää tarkempia tietoja SaaS sovelluksen integrointi Azure AD-kohdassa [mitä sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Edellytykset

Määrittää Azure AD-integroinnin Lifesize pilveen, tarvitset seuraavat:

- Azure AD-tilaus
- Lifesize Cloud single-merkki käytössä-tilauksessa


> [AZURE.NOTE] Voit esikatsella vaiheet Tässä opetusohjelmassa, Emme suosittele käyttämällä tuotantoympäristössä.


Voit esikatsella vaiheet Tässä opetusohjelmassa-noudattamalla näitä suosituksia:

- Älä käytä tuotantoympäristössä, ellei se on tarpeen.
- Jos sinulla ei ole kokeiluversion Azure AD-ympäristössä, voit hankkia yksi kuukausi kokeiluversion [tähän](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Skenaarion kuvaus
Tässä opetusohjelmassa testaat Azure AD kertakirjautumisen testiympäristössä.

Tässä opetusohjelmassa kuvatut skenaarion kuuluu kaksi tärkeimmät rakenneosien:

1. Lifesize Cloud lisääminen valikoimasta
2. Yksittäinen määrittäminen ja testaus Azure AD-on


## <a name="adding-lifesize-cloud-from-the-gallery"></a>Lifesize Cloud lisääminen valikoimasta
Voit määrittää Lifesize Cloud integroida Azure AD-haluat lisätä Lifesize Cloud valikoimasta hallitun SaaS sovellusluettelo.

**Voit lisätä Lifesize Cloud valikoimasta, toimi seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen portal**, valitse vasemmassa siirtymisruudussa.

    ![Active Directory][1]
2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset][2]

4. Valitse sivun alareunassa **Lisää** .

    ![Sovellukset][3]

5. Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Sovellukset][4]

6. Kirjoita hakuruutuun **Lifesize pilveen**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_01.png)

7. Valitse tulosruudussa **Lifesize Cloud**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_02.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yksittäinen määrittäminen ja testaus Azure AD-on
Tässä osassa voit määrittää ja testi Azure AD kertakirjautumisen Lifesize Cloud nimeltä "Britta Simon" testikäyttäjän perusteella.

Kertakirjautumisen toimimaan Azure AD on tiedettävä käyttäjälle on Lifesize Cloud vastaavaan käyttäjän Azure AD. Toisin sanoen linkki suhteen Azure AD-käyttäjä ja Aiheeseen liittyvät Lifesize pilvipohjaisia on vahvistettava.

Linkki-yhteys on muodostettu määrittämällä **käyttäjänimi** arvo arvoksi Lifesize Cloud **käyttäjänimi** Azure AD.

Määrittäminen ja testaaminen Azure AD kertakirjautumisen Lifesize Cloud kanssa, sinun on tehtävä seuraavat hakukyselyn:

1. **[Azure AD määrittäminen Single Sign](#configuring-azure-ad-single-sign-on)** - käyttäjät voivat käyttää tätä toimintoa.
2. **[Käyttäjän testata luominen Azure AD](#creating-an-azure-ad-test-user)** - Testaa Azure AD kertakirjautumisen Britta Simon kanssa.
3. **[Käyttäjän luominen Lifesize pilvestä testata](#creating-a-lifesize-cloud-test-user)** - on tapahtumista Britta Simon Lifesize pilvestä, joka on linkitetty hän Azure AD esitys.
4. **[Käyttäjän testata määrittäminen Azure AD](#assigning-the-azure-ad-test-user)** - Britta Simon käyttämään Azure AD kertakirjautumisen käyttöön.
5. **[Testaus Single Sign](#testing-single-sign-on)** - voit tarkistaa, toimiiko määritykset.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD kertakirjautumisen määrittäminen

Tässä osassa Azure AD kertakirjautumisen perinteinen portaalissa ottaminen käyttöön ja määritä kertakirjautumisen Lifesize Cloud-sovelluksessa.


**Määrittää Azure AD kertakirjautumisen Lifesize pilveen, toimimalla seuraavasti:**

1. Valitse perinteinen-portaalissa integrointi **Lifesize Cloud** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen Single Sign** -valintaikkunan avaaminen.
     
    ![Kertakirjautumisen määrittäminen][6] 

2. **Miten haluat kirjautua Lifesize pilveen käyttäjät** -sivulla Valitse **Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_03.png) 

3. Valitse **Määritä sovelluksen asetukset** -sivulla toimimalla seuraavasti:

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_04.png) 

    a. Kirjoita **Merkki-URL** -tekstiruutuun URL-osoite käyttämä Sign käyttämällä seuraavaa kaavaa Lifesize Cloud sovelluksen käyttäjille: **https://login.lifesizecloud.com/ls/?acs**.
    
    b. Valitse **Seuraava**
 
4. Valitse **Määritä kertakirjautumisen osoitteessa Lifesize Cloud** -sivulla toimimalla seuraavasti:

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_05.png)

    a. Valitse **Lataa**ja tallenna sitten tiedosto tietokoneesta.

    b. Valitse **Seuraava**.


5. Saat SSO määritetty sovelluksen, kirjaudu sisään järjestelmänvalvojan oikeuksilla Lifesize Cloud-sovellukseen.

6. Valitse oikeassa yläkulmassa Napsauta nimeäsi ja valitse sitten **Advance-asetukset**

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_06.png)

7. Advance-asetuksia nyt valitsemalla **SSO-määritys** -linkkiä. Tämä avaa oman esiintymän SSO-määritys-sivulla.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_07.png)

8. SSO-kokoonpanossa Käyttöliittymän määrittää seuraavat arvot.    

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_08.png)

    • Arvon myöntäjä URL-osoitteen kopioiminen Azure AD ja liitä, **Tunnistetietojen toimittaja myöntäjä** tekstiruutu.

    • Remote sisäänkirjautumisen URL-osoite solun tietojen kopioiminen Azure AD ja liitä, **Kirjautuminen URL** -tekstiruutuun.

    • Avaa ladatut varmenteen Muistiossa ja varmennetta, Aloita varmenne- ja End varmenteen rivit lukuun ottamatta sisällön kopioiminen, Liitä tämä **X.509-varmenne** -tekstiruutuun.

    • SAML määrite yhdistämismääritykseen **Etunimi** -kenttään arvo arvoksi **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**

    • SAML määrite yhdistämisessä **Sukunimi** -kenttään arvo arvoksi **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**

    • SAML määrite yhdistämisessä **sähköpostin** tekstiruudun Syötä arvo **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**

9. Voit tarkistaa määritykset napsauttamalla **Testaa** -painiketta.

    > [AZURE.NOTE] Onnistuneiden testaaminen sinun on viimeisteltävä ohjattu määritystoiminto Azure AD-ja lisätä access käyttäjille tai ryhmille, jotka voivat suorittaa testi.
    
10. Ota käyttöön SSO valitsemalla **Ota käyttöön SSO** -painiketta.

11. Napsauta nyt **Päivitä** -painiketta, niin, että kaikki asetukset tallennetaan. Tämä luo RelayState-arvo. Kopioi RelayState arvo, joka on luotu tekstiruutuun. Tarvitsemme arvoksi seuraavia vaiheita.

12. Perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Seuraava**.
    
    ![Azure AD-Single Sign-On][10]

13. Valitse **Sign Vahvista** -sivulla **Valmis**.  
 
    ![Azure AD-Single Sign-On][11]

14. Nyt kirjautuminen sisään Azure hallinta-portaalin **https://portal.azure.com** käyttämällä järjestelmänvalvojan tunnistetietoja

15. **Lisää palveluita** -linkkiä vasemmassa siirtymisruudussa
    
    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_09.png)

16. Azure Active Directory ja valitse **Azure Active Directory** -linkin hakeminen
    
    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_10.png)

17. Etsii kaikki **Yrityksen** -painikkeen SaaS sovellusten.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_11.png)

18. Valitse **Kaikki sovellukset** -linkin seuraava sivu
    
    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_12.png)

19. Etsi Lifesize sovellus, jonka haluat määrittää RelayState. 
    
    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_13.png)

20. Napsauta **kertakirjautumisen** linkki sivu

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_14.png)

21. Näyttöön tulee **Näytä URL-osoite Lisäasetukset** -valintaruutu. Valitse valintaruutu.
    
    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_15.png)
    
22. Nyt määrittää RelayState sovelluksen, joka näkyy Lifesize sovellusten SSO määritys-sivulla. 

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_16.png)

23. Tallenna asetukset.

### <a name="creating-an-azure-ad-test-user"></a>Azure AD-testikäyttäjän luominen
Tässä osassa voit luoda testikäyttäjän kutsutaan Britta Simon perinteinen portaalissa.


![Luo Azure AD-käyttäjä][20]

**Voit luoda testikäyttäjän Azure AD-toimimalla seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen portal**, valitse vasemmassa siirtymisruudussa.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_09.png) 

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Ylä-valikossa käyttäjäluettelon, näkyviin napsauttamalla **käyttäjät**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_03.png) 

4. Avaa **Lisää käyttäjä** -valintaikkunan alareunassa työkalurivillä, valitse **Lisää käyttäjä**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_04.png) 

5. Suorita valintaikkunan **Kerro kyseisen käyttäjän tietoja** -sivulla seuraavat toimet:  ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_05.png) 

    a. Käyttäjänä tyyppi Valitse uusi käyttäjä organisaatiossa.

    b. Kirjoita käyttäjänimi- **tekstiruutuun** **BrittaSimon**.

    c-näppäinyhdistelmää. Valitse **Seuraava**.

6.  Suorita valintaikkunan **Käyttäjäprofiili** -sivulla seuraavat toimet: ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_06.png) 

    a. Kirjoita **Ensimmäinen nimi** -tekstiruutuun **Britta**.  

    b. Kirjoita **Sukunimi** -tekstiruutuun tyyppi- **Simon**.

    c-näppäinyhdistelmää. Kirjoita **Näyttönimi** -tekstiruutuun **Britta Simon**.

    d. Valitse **rooli** -luettelosta **käyttäjä**.

    e. Valitse **Seuraava**.

7. Valitse **Hae tilapäinen salasana** -valintaikkunan sivulla **Luo**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_07.png) 

8. **Hae tilapäinen salasana** -valintaikkunan sivulla toimimalla seuraavasti:

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_08.png) 

    a. Kirjoita **Uusi salasana**arvo muistiin.

    b. Valitse **Valmis**.   



### <a name="creating-an-lifesize-cloud-test-user"></a>Lifesize Cloud-testikäyttäjän luominen

Tässä osassa nimeltä Britta Simon Lifesize Cloud käyttäjän luominen. Lifesize cloud tue automaattinen käyttäjän valmistelu. Kun onnistuneen tarkistuksen Azure AD-käyttäjän valmistelemiseen automaattisesti-sovelluksessa. 


### <a name="assigning-the-azure-ad-test-user"></a>Azure AD-testikäyttäjän määrittäminen

Tässä osassa voit ottaa käyttöön Britta Simon käyttämään Azure kertakirjautumisen hänen käyttöoikeuksien myöntäminen Lifesize pilveen.

![Määrittää käyttäjälle][200] 

**Jos haluat määrittää Britta Simon Lifesize pilveen, toimi seuraavasti:**

1. Perinteinen-portaalissa Avaa sovellukset-näkymässä kansio-näkymässä, valitse **sovellukset** yläreunan-valikossa.

    ![Määrittää käyttäjälle][201] 

2. Valitse sovellukset-luettelosta **Lifesize pilveen**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_50.png) 

3. Valitse ylä-valikossa Valitse **käyttäjiä**.

    ![Määrittää käyttäjälle][203]

4. Valitse käyttäjät-luettelosta **Britta Simon**.

5. Napsauta työkalurivin alaosassa **määrittäminen**.

    ![Määrittää käyttäjälle][205]


### <a name="testing-single-sign-on"></a>Kertakirjautumisen testaaminen

Tässä osassa voit testata Azure AD yksittäisen Sign-asetusten käyttäminen Access-paneeli.

Access-ruudussa Lifesize Cloud-ruutua napsauttaessasi voit olisi Hae automaattisesti kirjautunut sisään Lifesize Cloud-sovellukseen.


## <a name="additional-resources"></a>Lisäresursseja

* [Luettelo siitä, miten voit integroida SaaS sovellusten Azure Active Directory-opetusohjelmat](active-directory-saas-tutorial-list.md)
* [Mikä on sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_205.png
