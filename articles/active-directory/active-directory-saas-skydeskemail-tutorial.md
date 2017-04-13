<properties
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi Skydesk sähköpostin | Microsoft Azure"
    description="Opettele määrittämään kertakirjautumisen Azure Active Directory- ja Skydesk sähköpostin välillä."
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


# <a name="tutorial-azure-active-directory-integration-with-skydesk-email"></a>Opetusohjelma: Skydesk sähköpostin Azure Active Directory-integrointi

Tässä opetusohjelmassa tavoitteena on, kuinka voit Skydesk sähköpostin integroiminen Azure Active Directory (Azure AD).

Azure AD-integraation Skydesk sähköpostin avulla voit seuraavat edut:

- Voit hallita Azure AD, jolla on pääsy Skydesk sähköposti
- Voit ottaa käyttöön automaattisesti Hae kirjautunut sisään Skydesk sähköpostiin (kertakirjautumisen) käyttäjille heidän Azure AD-tilien kanssa
- Voit hallita asiakkaiden keskitetyn paikkaan – perinteinen Azure Active Directory-portaalissa

Jos haluat tietää tarkempia tietoja SaaS sovelluksen integrointi Azure AD-kohdassa [mitä sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Edellytykset

Azure AD-integroinnin määrittäminen Skydesk sähköpostin kanssa, tarvitset seuraavat:

- Azure AD-tilaus
- Skydesk sähköpostin single-merkki käytössä-tilauksessa


> [AZURE.NOTE] Voit esikatsella vaiheet Tässä opetusohjelmassa, Emme suosittele käyttämällä tuotantoympäristössä.


Voit esikatsella vaiheet Tässä opetusohjelmassa-noudattamalla näitä suosituksia:

- Älä käytä tuotantoympäristössä, ellei se on tarpeen.
- Jos sinulla ei ole kokeiluversion Azure AD-ympäristössä, voit hankkia yksi kuukausi kokeiluversion [tähän](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Skenaarion kuvaus
Tässä opetusohjelmassa tavoitteena on, jotta voit testata Azure AD kertakirjautumisen testiympäristössä. 

Tässä opetusohjelmassa kuvatut skenaarion kuuluu kaksi tärkeimmät rakenneosien:

1. Skydesk sähköpostin lisääminen valikoimasta
2. Yksittäinen määrittäminen ja testaus Azure AD-on


## <a name="adding-skydesk-email-from-the-gallery"></a>Skydesk sähköpostin lisääminen valikoimasta
Voit määrittää Skydesk sähköpostin integroida Azure AD-haluat Skydesk sähköpostin lisääminen valikoimasta hallitun SaaS sovellusluettelo.

**Voit lisätä Skydesk sähköpostin valikoimasta, toimi seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen portal**, valitse vasemmassa siirtymisruudussa. 

    ![Active Directory][1]

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset][2]

4. Valitse sivun alareunassa **Lisää** .

    ![Sovellukset][3]

5. Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Sovellukset][4]

6. Kirjoita hakuruutuun **Skydesk sähköpostin**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_01.png)

7. Valitse tulokset-ruudun Valitse **Skydesk sähköposti**ja valitse sitten **Valmis** , voit lisätä sovelluksen.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yksittäinen määrittäminen ja testaus Azure AD-on
Tässä osassa on noudattamalla voit määrittää ja testaa Azure AD kertakirjautumisen Skydesk sähköpostin nimeltä "Britta Simon" testikäyttäjän perusteella.

Kertakirjautumisen toimimaan Azure AD on tietää, mitkä asiat vastaavaan käyttäjän Skydesk sähköpostitse Azure AD-käyttäjälle. Toisin sanoen linkki suhteen Azure AD-käyttäjä ja Aiheeseen liittyvät Skydesk sähköpostitse on vahvistettava.

Linkki-yhteys on muodostettu määrittämällä **käyttäjänimi** arvo arvoksi Skydesk sähköpostin **käyttäjänimi** Azure AD.

Määrittäminen ja testaaminen Azure AD kertakirjautumisen Skydesk sähköpostin kanssa, sinun on tehtävä seuraavat hakukyselyn:

1. **[Azure AD määrittäminen Single Sign](#configuring-azure-ad-single-single-sign-on)** - käyttäjät voivat käyttää tätä toimintoa.
2. **[Käyttäjän testata luominen Azure AD](#creating-an-azure-ad-test-user)** - Testaa Azure AD kertakirjautumisen Britta Simon kanssa.
3. **[Käyttäjän luominen Skydesk sähköpostin testata](#creating-a-Skydesk-Email-test-user)** - on tapahtumista Britta Simon Skydesk kyseistä henkilöä hän Azure AD esitys sähköpostitse.
4. **[Käyttäjän testata määrittäminen Azure AD](#assigning-the-azure-ad-test-user)** - Britta Simon käyttämään Azure AD kertakirjautumisen käyttöön.
5. **[Testaus Single Sign](#testing-single-sign-on)** - voit tarkistaa, toimiiko määritykset.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD-Single Sign-On määrittäminen

Tässä osassa tavoitteena on Azure AD kertakirjautumisen Azure perinteinen portaalissa ottaminen käyttöön ja määrittää kertakirjautumisen Skydesk sähköposti-sovelluksessa.



**Jos haluat määrittää Azure AD kertakirjautumisen Skydesk sähköpostin kanssa, toimimalla seuraavasti:**

1. Valitse Azure perinteinen portaalissa integrointi **Skydesk sähköpostin** sovellus-sivulla **Määritä kertakirjautumisen** **Määrittäminen Single Sign** -valintaikkunan avaaminen.

    ![Kertakirjautumisen määrittäminen][6] 

2. **Miten käyttäjät voivat kirjautua Skydesk sähköpostin haluat käyttää** sivulla Valitse **Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_03.png) 


3. Valitse **Määritä sovelluksen asetukset** -sivulla toimimalla seuraavasti:
 
    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_04.png) 


    a. Kirjoita merkki-URL-tekstiruutuun URL-osoite käyttämä Sign käyttämällä seuraavaa kaavaa Skydesk sähköpostin sovelluksen käyttäjille: **"https://mail.skydesk.jp/portal/\<yrityksen nimi\>"**.

    b. Valitse **Seuraava**.


4. Valitse **Määritä kertakirjautumisen Skydesk sähköpostin toiminta on** -sivulla toimimalla seuraavasti:

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_05.png) 

    a. Valitse **Lataa**ja tallenna sitten tiedosto tietokoneesta.

    b. Valitse **Seuraava**.


5. Jotta Kertakirjautumista **Skydesk**sähköpostiviestissä, toimi seuraavasti:
 
    a. Kertakirjauksen Skydesk sähköpostitilin järjestelmänvalvojana.

    b. Valitse ylä-valikossa asetukset ja valitse Org. 

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_51.png)

    c-näppäinyhdistelmää. Valitse toimialueiden vasemmassa paneelissa.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_53.png)

    d. Valitse Lisää toimialue.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_54.png)

    e. Kirjoita toimialuenimi ja tarkista toimialue.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_55.png)

    f. Valitse vasemmanpuoleisessa ruudussa **SAML** -todennus

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_52.png)

6. Suorita **SAML todennus** -sivulla seuraavat toimet:

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_56.png)

    > [AZURE.NOTE] SAML-pohjaisen todennus käyttämään joko sinulla on **vahvistanut toimialueen** tai **portaalin URL-Osoitteen** asetukset. Voit määrittää portaalin URL-osoite yksilöllinen nimi.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_57.png)


    a. Perinteinen Azure AD-portaalissa **SAML SSO URL-osoite** -arvo kopioi ja liitä se sitten **Kirjautuminen URL** -tekstiruutuun.

    b. Perinteinen Azure AD-portaalissa kopioi **Yhden Sign-Out palvelun URL-osoite** -arvo ja liitä se sitten **Kirjaudu ulos** URL-tekstiruutuun.

    c-näppäinyhdistelmää. **Muuta salasana URL-osoite** on valinnainen niin jätä se tyhjäksi.

    d. Valitse **Hae avain tiedostosta** ja valitse ladatut Skydesk sähköposti-varmenne ja valitse sitten **Avaa** Lataa sertifikaatti.

    e. **Algoritmin**Valitse **RSA**.

    f. Valitse **Ok** voit tallentaa muutokset.


7. Azure perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Seuraava**.

    ![Azure AD-Single Sign-On][10]

8. Valitse **Sign Vahvista** -sivulla **Valmis**.  
  
    ![Azure AD-Single Sign-On][11]




### <a name="creating-an-azure-ad-test-user"></a>Azure AD-testikäyttäjän luominen
Tässä osassa tavoitteena on luotava testikäyttäjän kutsutaan Britta Simon Azure perinteinen portaalissa.

![Luo Azure AD-käyttäjä][20]

**Voit luoda testikäyttäjän Azure AD-toimimalla seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen portal**, valitse vasemmassa siirtymisruudussa.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_09.png) 

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Ylä-valikossa käyttäjäluettelon, näkyviin napsauttamalla **käyttäjät**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_03.png) 

4. Avaa **Lisää käyttäjä** -valintaikkunan alareunassa työkalurivillä, valitse **Lisää käyttäjä**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_04.png) 

5. Tee valintaikkunassa **Kerro kyseisen käyttäjän tietoja** -sivulla seuraavasti:

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_05.png) 

    a. Käyttäjänä tyyppi Valitse uusi käyttäjä organisaatiossa.

    b. Kirjoita käyttäjänimi- **tekstiruutuun** **BrittaSimon**.

    c-näppäinyhdistelmää. Valitse **Seuraava**.

6.  Suorita **Käyttäjäprofiili** -sivulla seuraavat toimet:

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_06.png) 

    a. Kirjoita **Ensimmäinen nimi** -tekstiruutuun **Britta**.  

    b. Kirjoita **Sukunimi** -tekstiruutuun tyyppi- **Simon**.

    c-näppäinyhdistelmää. Kirjoita **Näyttönimi** -tekstiruutuun **Britta Simon**.

    d. Valitse **rooli** -luettelosta **käyttäjä**.

    e. Valitse **Seuraava**.

7. Valitse **Hae tilapäinen salasana** -valintaikkunan sivulla **Luo**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_07.png) 

8. **Hae tilapäinen salasana** -valintaikkunan sivulla toimimalla seuraavasti:
 
    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_08.png) 

    a. Kirjoita **Uusi salasana**arvo muistiin.

    b. Valitse **Valmis**.   



### <a name="creating-a-skydesk-email-test-user"></a>Skydesk sähköpostin testikäyttäjän luominen

Tässä osassa nimeltä Britta Simon Skydesk sähköpostin käyttäjän luominen.

a. Valitse **Käyttöoikeudet** -vasemmassa paneelissa Skydesk sähköpostitse ja kirjoita käyttäjänimesi. 

![Kertakirjautumisen määrittäminen](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_58.png)


[AZURE.NOTE] Jos haluat luoda joukkona käyttäjiä, sinun on Skydesk sähköposti-tukeen.


### <a name="assigning-the-azure-ad-test-user"></a>Azure AD-testikäyttäjän määrittäminen

Tässä osassa tavoitteena on Britta Simon käyttämään Azure kertakirjautumisen hänen käyttöoikeuden myöntämisestä Skydesk sähköpostin ottaminen käyttöön.

![Määrittää käyttäjälle][200] 

**Jos haluat määrittää Britta Simon Skydesk sähköpostin, toimi seuraavasti:**

1. Azure perinteinen-portaalissa Avaa sovellukset-näkymässä kansio-näkymässä, valitse **sovellukset** yläreunan-valikossa.
 
    ![Määrittää käyttäjälle][201] 

2. Valitse sovellukset-luettelosta **Skydesk sähköpostin**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_50.png) 

1. Valitse ylä-valikossa Valitse **käyttäjiä**.

    ![Määrittää käyttäjälle][203] 

1. Valitse käyttäjät-luettelosta **Britta Simon**.

2. Napsauta työkalurivin alaosassa **määrittäminen**.

    ![Määrittää käyttäjälle][205]



### <a name="testing-single-sign-on"></a>Kertakirjautumisen testaaminen

Tässä osassa tavoitteena on Testaa Azure AD yksittäisen Sign-asetusten käyttäminen Access-paneeli.

Kun napsautat Access-ruudussa Skydesk Sähköposti-ruutu, voit olisi Hae automaattisesti kirjautunut käyttöön Skydesk sähköpostisovellus.


## <a name="additional-resources"></a>Lisäresursseja

* [Luettelo siitä, miten voit integroida SaaS sovellusten Azure Active Directory-opetusohjelmat](active-directory-saas-tutorial-list.md)
* [Mikä on sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_205.png
