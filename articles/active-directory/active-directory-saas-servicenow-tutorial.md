<properties
    pageTitle="Opetusohjelma: Azure Active Directory-integrointi ServiceNow ja ServiceNow Express | Microsoft Azure"
    description="Opettele määrittämään kertakirjautumisen Azure Active Directory ja ServiceNow ja ServiceNow Express välillä."
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
    ms.date="10/27/2016"
    ms.author="jeedes"/>


# <a name="tutorial-azure-active-directory-integration-with-servicenow-and-servicenow-express"></a>Opetusohjelma: Azure Active Directory-integrointi ServiceNow ja ServiceNow Express.


Tässä opetusohjelmassa kerrotaan, miten voit integroida ServiceNow ServiceNow Express Azure Active Directory (Azure AD).

ServiceNow ja ServiceNow Express-integraation Azure AD, jonka avulla seuraavat edut:

- Voit hallita Azure AD, jolla on pääsy ServiceNow ja ServiceNow Express
- Voit ottaa käyttöön automaattisesti Hae kirjautunut sisään ServiceNow ja ServiceNow Express (kertakirjautumisen) niiden Azure AD-tilien käyttäjille
- Voit hallita asiakkaiden yhdessä keskitetyssä sijainnissa - Azure perinteinen portal

Jos haluat tietää tarkempia tietoja SaaS sovelluksen integrointi Azure AD-kohdassa [mitä sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Edellytykset

Määrittää Azure AD-integroinnin ServiceNow ja ServiceNow Express, tarvitset seuraavat:

- Azure AD-tilaus
- ServiceNow, esiintymän eli alihallintasivustossa, ServiceNow, Calgary versio tai sitä uudemmassa versiossa
- ServiceNow Expressin esiintymän ServiceNow Express Helsinki versio tai sitä uudemmassa versiossa
- ServiceNow vuokraajan on oltava käytössä [Useita tarjoajan yksittäinen merkki-laajennus](http://wiki.servicenow.com/index.php?title=Multiple_Provider_Single_Sign-On#gsc.tab=0) . Voit tehdä tämän lähettämällä osoitteessa <https://hi.service-now.com/> palvelupyyntö 


> [AZURE.NOTE] Voit esikatsella vaiheet Tässä opetusohjelmassa, Emme suosittele käyttämällä tuotantoympäristössä.


Voit esikatsella vaiheet Tässä opetusohjelmassa-noudattamalla näitä suosituksia:

- Älä käytä tuotantoympäristössä, ellei se on tarpeen.
- Jos sinulla ei ole kokeiluversion Azure AD-ympäristössä, voit hankkia yksi kuukausi kokeiluversion [tähän](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Skenaarion kuvaus
Tässä opetusohjelmassa testaat Azure AD kertakirjautumisen testiympäristössä. Tässä opetusohjelmassa kuvatut skenaarion kuuluu kaksi tärkeimmät rakenneosien:

1. ServiceNow lisääminen valikoimasta
2. Määrittäminen ja testaus Azure AD yksittäinen Sign ServiceNow tai ServiceNow Express


## <a name="adding-servicenow-from-the-gallery"></a>ServiceNow lisääminen valikoimasta
Voit määrittää ServiceNow tai ServiceNow Express integroida Azure AD-haluat lisätä ServiceNow valikoimasta hallitun SaaS sovellusluettelo. 

**Jos haluat lisätä ServiceNow valikoimasta, toimi seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen portal**, valitse vasemmassa siirtymisruudussa. 

    ![Active Directory][1]

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Avaa sovellukset-näkymässä kansio-näkymässä, valitse ylin-valikossa **sovellukset** .

    ![Sovellukset][2]

4. Valitse sivun alareunassa **Lisää** .

    ![Sovellukset][3]

5. Valitse **Mitä haluat tehdä** -valintaikkunasta **Lisää sovellus-valikoimasta**.

    ![Sovellukset][4]

6. Kirjoita hakuruutuun **ServiceNow**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_01.png)

7. Valitse tulosruudussa **ServiceNow**ja valitse **Valmis** , voit lisätä sovelluksen.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_02.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Yksittäinen määrittäminen ja testaus Azure AD-on
Tässä osassa voit määrittää ja testaa Azure AD kertakirjautumisen ServiceNow tai ServiceNow Express perustuvat testikäyttäjän nimeltä "Britta Simon".

Kertakirjautumisen toimimaan Azure AD on tiedettävä käyttäjälle on ServiceNow vastaavaan käyttäjän Azure AD. Toisin sanoen suhteen Azure AD-käyttäjä ja Aiheeseen liittyvät ServiceNow-linkki on vahvistettava.
Linkki-yhteys on muodostettu määrittämällä **käyttäjänimi** arvo arvoksi ServiceNow **käyttäjänimi** Azure AD. Määrittäminen ja testaaminen Azure AD kertakirjautumisen ServiceNow kanssa, sinun on tehtävä seuraavat hakukyselyn:

1. **[Määrittäminen Azure AD - kertakirjautuminen ServiceNow](#configuring-azure-ad-single-sign-on-for-servicenow)** - käyttäjät voivat käyttää tätä toimintoa.
2. **[Määrittäminen Azure AD - kertakirjautuminen ServiceNow Express](#configuring-azure-ad-single-sign-on-for-servicenow-express)** - käyttäjät voivat käyttää tätä toimintoa.
3. **[Käyttäjän testata luominen Azure AD](#creating-an-azure-ad-test-user)** - Testaa Azure AD kertakirjautumisen Britta Simon kanssa.
4. **[Luomisesta ServiceNow testata käyttäjän](#creating-a-servicenow-test-user)** - tapahtumista Britta Simon ovat ServiceNow, jotka on linkitetty hän Azure AD esitys.
5. **[Käyttäjän testata määrittäminen Azure AD](#assigning-the-azure-ad-test-user)** - Britta Simon käyttämään Azure AD kertakirjautumisen käyttöön.
6. **[Testaus Single Sign](#testing-single-sign-on)** - voit tarkistaa, toimiiko määritykset.

> [AZURE.NOTE] Jos haluat määrittää ServiceNow ohittaa vaiheen 2. Vastaavasti, jos haluat määrittää ServiceNow Express ohittaa vaiheen 1.

### <a name="configuring-azure-ad-single-sign-on-for-servicenow"></a>Azure AD-Single Sign-On ServiceNow määrittäminen

1.  Azure AD perinteinen-portaalissa, integrointi **ServiceNow** sovellus-sivulla valitsemalla **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-servicenow-tutorial/IC749323.png "Määritä kertakirjautuminen")

2.  **Miten käyttäjät voivat kirjautua ServiceNow haluat käyttää** sivulla Valitse **Microsoft Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-servicenow-tutorial/IC749324.png "Määritä kertakirjautuminen")

3.  Suorita **Sovellus asetusten määrittäminen** -sivulla seuraavasti:

    ![Sovelluksen URL-Osoitteen määrittäminen] (./media/active-directory-saas-servicenow-tutorial/IC769497.png "Sovelluksen URL-Osoitteen määrittäminen")

    a. Kirjoita **ServiceNow merkki-URL** -tekstiruutuun URL-osoite käyttämä Sign seuraavan kuvion ServiceNow sovelluksen käyttäjille: `https://<instance-name>.service-now.com`.

    b. Kirjoita **tunnus** -tekstiruutuun käytettävä käyttäjien Sign seuraavan kuvion ServiceNow sovelluksen URL-osoite: `https://<instance-name>.service-now.com`.

    c-näppäinyhdistelmää. Valitse **Seuraava**

4.  Jos haluat määrittää automaattisesti SAML-pohjaisen todennustavaksi ServiceNow Azure AD, kirjoita ServiceNow-esiintymänimi, järjestelmänvalvojan käyttäjänimi ja järjestelmänvalvojan salasanan **automaattinen määrittäminen single Sign** -lomakkeessa ja valitse *Määritä*. Huomaa, että järjestelmänvalvojan käyttäjänimi on annettu on oltava ServiceNow tämän toimimaan **security_admin** rooli. Muussa tapauksessa manuaalisesta ServiceNow käytettävä SAML tunnistetietojen toimittajaa Azure AD-valitsemalla **Määritä manuaalisesti kertakirjautumisen hakeminen**, ja valitse **Seuraava** ja seuraavien vaiheiden mukaisesti.

    ![Sovelluksen URL-Osoitteen määrittäminen] (./media/active-directory-saas-servicenow-tutorial/IC7694971.png "Sovelluksen URL-Osoitteen määrittäminen")



5.  Valitse **Määritä kertakirjautumisen osoitteessa ServiceNow** -sivulla **Lataa sertifikaatti**, Tallenna sertifikaattitiedosto paikallisesti tietokoneeseen.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-servicenow-tutorial/IC749325.png "Määritä kertakirjautuminen")

1. Kertakirjauksen ServiceNow sovelluksen järjestelmänvalvojana.
2. Aktivoi _integrointi - useita tarjoajan yksittäisen Sign-On Installer_ laajennus tekemällä seuraavat toimet:

    a. Siirtymisruudun vasemmassa reunassa **Järjestelmän määritelmä** -kohdassa ja valitse sitten **laajennukset**.

    ![Sovelluksen URL-Osoitteen määrittäminen] (./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_03.png "Aktivoi-lisäosa")
    
    b. Etsi _integrointi - useita tarjoajan yksittäisen Sign-asennusohjelma_.

    ![Sovelluksen URL-Osoitteen määrittäminen] (./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_04.png "Aktivoi-lisäosa")

    c-näppäinyhdistelmää. Valitse laajennus. Rigth ja valitse **Aktivoi/päivitys**.
    
    d. Valitse **Aktivoi** -painike.

2. Siirtymisruudun vasemmassa reunassa Valitse **Ominaisuudet**.  

    ![Sovelluksen URL-Osoitteen määrittäminen] (./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_06.png "Sovelluksen URL-Osoitteen määrittäminen")


3. Valitse **Useita SSO ominaisuudet** -valintaikkunassa seuraavasti:

    ![Sovelluksen URL-Osoitteen määrittäminen] (./media/active-directory-saas-servicenow-tutorial/IC7694981.png "Sovelluksen URL-Osoitteen määrittäminen")

    a. **Useita toimittajan SSO ottaminen käyttöön**Valitse **Kyllä**.

    b. **Ota käyttöön virheenkorjaus kirjaaminen, onko käytössäsi useita tarjoajan SSO integrointi**Valitse **Kyllä**.

    c-näppäinyhdistelmää. Kirjoita **käyttäjänimi** **käyttäjän kenttä taulukkoon, jonka...** -tekstiruutuun.

    d. Valitse **Tallenna**.



1. Valitse vasemmalla puolella olevassa siirtymisruudussa, **x509 varmenteet**.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_05.png "Määritä kertakirjautuminen")


1. Valitse **X.509 varmenteet** -valintaikkunassa **Uusi**.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-servicenow-tutorial/IC7694974.png "Määritä kertakirjautuminen")


1. Valitse **X.509 varmenteet** -valintaikkunassa seuraavasti:

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-servicenow-tutorial/IC7694975.png "Määritä kertakirjautuminen")

    a. Valitse **Uusi**.

    b. Kirjoita **nimi** -tekstiruutuun kokoonpanosi nimi (esimerkiksi: **TestSAML2.0**).

    c-näppäinyhdistelmää. Valitse **aktiivinen**.

    d. Valitse **Muotoile**- **PEM**.

    e. Valitse **tyyppi**- **Luota kaupan varmenne**.
    
    f. Avaa Base64-koodattuja varmennetta Muistiossa, kopioi Leikepöydän sisällön se ja liitä se **PEM varmenne** -tekstiruutuun.

    g. Valitse **Päivitä**.


1. Valitse vasemmalla puolella olevassa siirtymisruudussa- **Tunnistetietojen toimittajat**.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_07.png "Määritä kertakirjautuminen")

1. Valitse **Uusi** **Tunnistetietojen toimittajat** -valintaikkunassa:

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-servicenow-tutorial/IC7694977.png "Määritä kertakirjautuminen")


1. Valitse **Tunnistetietojen toimittajat** -valintaikkunassa **SAML2 Update1?**:

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-servicenow-tutorial/IC7694978.png "Määritä kertakirjautuminen")


1. Suorita SAML2 Update1 ominaisuudet-valintaikkunassa seuraavat toimet:

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-servicenow-tutorial/IC7694982.png "Määritä kertakirjautuminen")


    a. Kirjoita **nimi** -tekstiruutuun kokoonpanosi nimi (esimerkiksi: **SAML 2.0**).

    b. **Käyttäjäkenttä** tekstiruutu, **sähköpostin** tai **user_id**, sen mukaan, mitä kenttää käytetään yksilöivät ServiceNow-yhdistelmäympäristössä käyttäjät. 
    
    > [AZURE.NOTE] Voit päästää Azure AD-Käyttäjätunnus (käyttäjätunnus) tai sähköpostiosoite muodossa oleva yksilöllinen tunnus SAML-tunnuksen siirtymällä configue Azure AD **ServiceNow > määritteet > kertakirjautumisen** osan Azure perinteinen portaaliin ja yhdistäminen haluamasi kentän **nameidentifier** -määrite. Azure AD (esimerkiksi täydellinen käyttäjätunnus) tallennetaan valitun määritteen arvon on oltava samat tallennettu ServiceNow syötetty (kuten user_id)-kenttään arvo

    c-näppäinyhdistelmää. Perinteinen Azure AD-portaalissa **Tunnistetietojen Toimittajan tunnus** -arvon kopioi ja liitä se sitten **Tunnistetietojen toimittaja URL** -tekstiruutuun.

    d. Perinteinen Azure AD-portaalissa **Käyttöoikeuksien pyytäminen URL-osoite** -arvo kopioi ja liitä se sitten **tunnistetietojen toimittaja AuthnRequest** tekstiruutu.

    e. Valitse perinteinen Azure AD-portaalissa kopioi **Yhden Sign-Out palvelun URL-osoite** -arvo ja liitä se sitten **tunnistetietojen toimittaja SingleLogoutRequest** tekstiruutu.

    f. Kirjoita **ServiceNow aloitussivu** -tekstiruutuun ServiceNow esiintymä-kotisivun URL-osoite.

    > [AZURE.NOTE] ServiceNow esiintymä-aloitussivu on yhdistelmä **ServieNow vuokraajan URL-osoite** ja **/navpage.do** (esimerkiksi:`https://fabrikam.service-now.com/navpage.do`).
 

    g. Valitse **kohteen tunnus / myöntäjä** -tekstiruutuun ServiceNow vuokraajan URL-osoite.

    h. Kirjoita URL-Osoitteen ServiceNow vuokraajan **Yleisön URL** -tekstiruutuun. 

    voin. Kirjoita **Protokolla sidontaa varten IDP SingleLogoutRequest** -tekstiruutuun **urn: oasis: nimet: tc: SAML:2.0:bindings:HTTP-uudelleenohjata**.

    j. Kirjoita NameID käytäntö-tekstiruutuun **urn: oasis: nimet: tc: SAML:1.1:nameid-muoto: Määrittämätön**.

    k. Poista valinta **Luo AuthnContextClass**.

    l. Kirjoita **AuthnContextClassRef menetelmä** `http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password`. Tämä on tarpeen vain, jos olet cloud vain organisaation. Jos käytössäsi on paikalliseen ADFS tai MFA todennustavaksi sitten kannattaa ei määrittää tämän arvon. 

    m. Kirjoita **60** **Kellon jakauman.vinous** -tekstiruutuun.

    n. Valitse **Yksittäinen merkki-komentosarja** **MultiSSO_SAML2_Update1**.

    o-näppäinyhdistelmää. Kuin **x509 sertifikaatin**, valitse edellisessä vaiheessa luomasi varmenne.

    p-näppäinyhdistelmää. Valitse **Lähetä**. 



6. Azure AD perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Seuraava**. 

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-servicenow-tutorial/IC7694990.png "Määritä kertakirjautuminen")

7. Valitse **Sign Vahvista** -sivulla **Valmis**.
 
    ![Määritä kertakirjautuminen] (./media/active-directory-saas-servicenow-tutorial/IC7694991.png "Määritä kertakirjautuminen")

### <a name="configuring-azure-ad-single-sign-on-for-servicenow-express"></a>Azure AD-Single Sign-On ServiceNow Express määrittäminen

1.  Azure AD perinteinen-portaalissa, integrointi **ServiceNow** sovellus-sivulla valitsemalla **Määritä kertakirjautumisen** **Määrittäminen kertakirjautuminen** -valintaikkunan avaaminen.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-servicenow-tutorial/IC749323.png "Määritä kertakirjautuminen")

2.  **Miten käyttäjät voivat kirjautua ServiceNow haluat käyttää** sivulla Valitse **Microsoft Azure AD kertakirjautumisen**ja valitse sitten **Seuraava**.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-servicenow-tutorial/IC749324.png "Määritä kertakirjautuminen")

3.  Suorita **Sovellus asetusten määrittäminen** -sivulla seuraavasti:

    ![Sovelluksen URL-Osoitteen määrittäminen] (./media/active-directory-saas-servicenow-tutorial/IC769497.png "Sovelluksen URL-Osoitteen määrittäminen")

    a. Kirjoita **ServiceNow merkki-URL** -tekstiruutuun URL-osoite käyttämä Sign seuraavan kuvion ServiceNow sovelluksen käyttäjille: `https://<instance-name>.service-now.com`.

    b. Kirjoita **Myöntäjä URL** -tekstiruutuun URL-osoite käytettävä käyttäjien Sign seuraavan kuvion ServiceNow sovelluksen `https://<instance-name>.service-now.com`.

    c-näppäinyhdistelmää. Valitse **Seuraava**

4.  Napsauta **manuaalisesta kertakirjautumisen hakeminen**, napsauta **Seuraava** ja seuraavien vaiheiden mukaisesti.

    ![Sovelluksen URL-Osoitteen määrittäminen] (./media/active-directory-saas-servicenow-tutorial/IC7694971.png "Sovelluksen URL-Osoitteen määrittäminen")

5.  Valitse **Määritä kertakirjautumisen osoitteessa ServiceNow** -sivulla valitsemalla **Lataa**, Tallenna sertifikaattitiedosto paikallisesti tietokoneeseen ja valitse sitten **Seuraava**.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-servicenow-tutorial/IC749325.png "Määritä kertakirjautuminen")

6. Kirjaudu sisään järjestelmänvalvojana ServiceNow Express-sovellukseen.

7. Valitse vasemmalla puolella olevassa siirtymisruudussa, **Kertakirjautuminen**.  

    ![Sovelluksen URL-Osoitteen määrittäminen] (./media/active-directory-saas-servicenow-tutorial/ic7694980ex.png "Sovelluksen URL-Osoitteen määrittäminen")


8. Valitse **Single Sign** -valintaikkunassa oikeassa yläkulmassa määritys-kuvaketta ja määritä seuraavat ominaisuudet:

    ![Sovelluksen URL-Osoitteen määrittäminen] (./media/active-directory-saas-servicenow-tutorial/ic7694981ex.png "Sovelluksen URL-Osoitteen määrittäminen")

    a. Vaihda **käyttöön useita tarjoajan SSO** oikealle.

    b. Vaihda **Ota lokiin kirjaaminen useita tarjoajan SSO-integroinnin virheenkorjaus** oikealle.

    c-näppäinyhdistelmää. Kirjoita **käyttäjänimi** **käyttäjän kenttä taulukkoon, jonka...** -tekstiruutuun.


9. Valitse **Single Sign** -valintaikkunasta **Lisää uutta varmennetta**.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-servicenow-tutorial/ic7694973ex.png "Määritä kertakirjautuminen")



10. Valitse **X.509 varmenteet** -valintaikkunassa seuraavasti:

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-servicenow-tutorial/IC7694975.png "Määritä kertakirjautuminen")

    a. Kirjoita **nimi** -tekstiruutuun kokoonpanosi nimi (esimerkiksi: **TestSAML2.0**).

    b. Valitse **aktiivinen**.

    c-näppäinyhdistelmää. Valitse **Muotoile**- **PEM**.

    d. Valitse **tyyppi**- **Luota kaupan varmenne**.

    e. Luo Base64-koodattuja tiedosto ladattu varmennetta.
   
    > [AZURE.NOTE] Lisätietoja on artikkelissa [muuntaa binaariluvun varmenteen tekstitiedostoon](http://youtu.be/PlgrzUZ-Y1o).
    
    f. Avaa Base64-koodattuja varmennetta Muistiossa, kopioi Leikepöydän sisällön se ja liitä se **PEM varmenne** -tekstiruutuun.

    g. Valitse **Päivitä**.


11. Valitse **Lisää uusi IdP** **Single Sign** -valintaikkunasta.

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-servicenow-tutorial/ic7694976ex.png "Määritä kertakirjautuminen")


12. Valitse **Lisää uusi tunnistetietojen toimittaja** -valintaikkunassa **Määrittää tunnistetietojen toimittaja**toimimalla seuraavasti:

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-servicenow-tutorial/ic7694982ex.png "Määritä kertakirjautuminen")


    a. Kirjoita **nimi** -tekstiruutuun kokoonpanosi nimi (esimerkiksi: **SAML 2.0**).

    b. Perinteinen Azure AD-portaalissa **Tunnistetietojen Toimittajan tunnus** -arvon kopioi ja liitä se sitten **Tunnistetietojen toimittaja URL** -tekstiruutuun.

    c-näppäinyhdistelmää. Perinteinen Azure AD-portaalissa **Käyttöoikeuksien pyytäminen URL-osoite** -arvo kopioi ja liitä se sitten **tunnistetietojen toimittaja AuthnRequest** tekstiruutu.

    d. Valitse perinteinen Azure AD-portaalissa kopioi **Yhden Sign-Out palvelun URL-osoite** -arvo ja liitä se sitten **tunnistetietojen toimittaja SingleLogoutRequest** tekstiruutu.

    e. Kuin **Tunnistetietojen toimittaja varmenne**Valitse edellisessä vaiheessa luomasi varmenne.


13. Valitse **Lisäasetukset**ja valitse **Tunnistetietojen toimittaja lisäominaisuuksia**, toimi seuraavasti:

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-servicenow-tutorial/ic7694983ex.png "Määritä kertakirjautuminen")

    a. Kirjoita **Protokolla sidontaa varten IDP SingleLogoutRequest** -tekstiruutuun **urn: oasis: nimet: tc: SAML:2.0:bindings:HTTP-uudelleenohjata**.

    b. Kirjoita **NameID käytäntö** -tekstiruutuun **urn: oasis: nimet: tc: SAML:1.1:nameid-muoto: Määrittämätön**.    

    c-näppäinyhdistelmää. Kirjoita **AuthnContextClassRef menetelmä** **http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password**.
    
    d. Poista valinta **Luo AuthnContextClass**.

14. Valitse **Muut ominaisuudet**-toimimalla seuraavasti:

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-servicenow-tutorial/ic7694984ex.png "Määritä kertakirjautuminen")

    a. Kirjoita **ServiceNow aloitussivu** -tekstiruutuun ServiceNow esiintymä-kotisivun URL-osoite.

    > [AZURE.NOTE] ServiceNow esiintymä-aloitussivu on yhdistelmä **ServieNow vuokraajan URL-osoite** ja **/navpage.do** (esimerkiksi: `https://fabrikam.service-now.com/navpage.do`).

    b. Valitse **kohteen tunnus / myöntäjä** -tekstiruutuun ServiceNow vuokraajan URL-osoite.

    c-näppäinyhdistelmää. Kirjoita URL-Osoitteen ServiceNow vuokraajan **Käyttäjäryhmän URI** -tekstiruutuun. 

    d. Kirjoita **60** **Kellon jakauman.vinous** -tekstiruutuun.

    e. **Käyttäjäkenttä** tekstiruutu, **sähköpostin** tai **user_id**, sen mukaan, mitä kenttää käytetään yksilöivät ServiceNow-yhdistelmäympäristössä käyttäjät.
    
    > [AZURE.NOTE] Voit päästää Azure AD-Käyttäjätunnus (käyttäjätunnus) tai sähköpostiosoite muodossa oleva yksilöllinen tunnus SAML-tunnuksen siirtymällä configue Azure AD **ServiceNow > määritteet > kertakirjautumisen** osan Azure perinteinen portaaliin ja yhdistäminen haluamasi kentän **nameidentifier** -määrite. Azure AD (esimerkiksi täydellinen käyttäjätunnus) tallennetaan valitun määritteen arvon on oltava samat tallennettu ServiceNow syötetty (kuten user_id)-kenttään arvo

    f. Valitse **Tallenna**. 


15. Azure AD perinteinen-portaalissa Valitse vahvistus Sign-määritys ja valitse sitten **Seuraava**. 

    ![Määritä kertakirjautuminen] (./media/active-directory-saas-servicenow-tutorial/IC7694990.png "Määritä kertakirjautuminen")

16. Valitse **Sign Vahvista** -sivulla **Valmis**.
 
    ![Määritä kertakirjautuminen] (./media/active-directory-saas-servicenow-tutorial/IC7694991.png "Määritä kertakirjautuminen")

##<a name="configuring-user-provisioning"></a>Määrittäminen käyttäjän valmistelu
  
Tässä osassa tavoitteena on jäsentäminen ottamisesta käyttöön käyttäjän valmistelu Active Directory-käyttäjätilien ServiceNow.


### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>Voit määrittää käyttäjän valmistelu toimimalla seuraavasti:

1. Azure-hallinnan perinteinen portal-integroinnin **ServiceNow** sovellus-sivulla Valitse **Määritä käyttäjän valmistelu**. 

    ![Käyttäjän valmistelu] (./media/active-directory-saas-servicenow-tutorial/IC769498.png "Käyttäjän valmistelu")


2. Anna **ServiceNow tunnistetietosi Ota käyttöön automaattinen käyttäjän valmistelu** -sivulla seuraavat asetukset: Määritä käyttäjän valmistelu 

     a. Kirjoita **ServiceNow esiintymänimi** -tekstiruutuun ServiceNow esiintymänimi.

     b. Kirjoita **ServiceNow järjestelmänvalvojan käyttäjänimi** -tekstiruutuun ServiceNow järjestelmänvalvojatilin nimi.

     c-näppäinyhdistelmää. Kirjoita **ServiceNow järjestelmänvalvojan salasana** -tekstiruutuun tilin salasana.

     d. Valitse **Vahvista** vahvistamiseksi kokoonpanosi.

     e. Napsauta Avaa **seuraavat vaiheet** -sivulla **Seuraava** -painiketta.

     f. Jos haluat kaikkien käyttäjien tämän sovelluksen käytön valmistelu, valitse "**valmistella automaattisesti kaikki käyttäjätilit tämän sovelluksen hakemistossa**". 

    ![Seuraavat vaiheet] (./media/active-directory-saas-servicenow-tutorial/IC698804.png "Seuraavat vaiheet")

     g. Valitse **seuraavat vaiheet** -sivulla **Valmis** tallentamaan kokoonpanosi.

### <a name="creating-an-azure-ad-test-user"></a>Azure AD-testikäyttäjän luominen
Tässä osassa voit luoda testikäyttäjän kutsutaan Britta Simon perinteinen portaalissa.

![Luo Azure AD-käyttäjä][20]

**Voit luoda testikäyttäjän Azure AD-toimimalla seuraavasti:**

1. Valitse **Active Directory** **Azure perinteinen portal**, valitse vasemmassa siirtymisruudussa.
    
    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-servicenow-tutorial/create_aaduser_09.png) 

2. Valitse kansio, jonka haluat ottaa käyttöön yhteystietojen integrointi **Hakemisto** -luettelosta.

3. Ylä-valikossa käyttäjäluettelon, näkyviin napsauttamalla **käyttäjät**.
    
    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-servicenow-tutorial/create_aaduser_03.png) 

4. Avaa **Lisää käyttäjä** -valintaikkunan alareunassa työkalurivillä, valitse **Lisää käyttäjä**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-servicenow-tutorial/create_aaduser_04.png) 

5. Tee valintaikkunassa **Kerro kyseisen käyttäjän tietoja** -sivulla seuraavasti:
 
    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-servicenow-tutorial/create_aaduser_05.png) 

    a. Käyttäjänä tyyppi Valitse uusi käyttäjä organisaatiossa.

    b. Kirjoita käyttäjänimi- **tekstiruutuun** **BrittaSimon**.

    c-näppäinyhdistelmää. Valitse **Seuraava**.

6.  Suorita **Käyttäjäprofiili** -sivulla seuraavat toimet:

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-servicenow-tutorial/create_aaduser_06.png) 

    a. Kirjoita **Ensimmäinen nimi** -tekstiruutuun **Britta**.  

    b. Kirjoita **Sukunimi** -tekstiruutuun tyyppi- **Simon**.

    c-näppäinyhdistelmää. Kirjoita **Näyttönimi** -tekstiruutuun **Britta Simon**.

    d. Valitse **rooli** -luettelosta **käyttäjä**.

    e. Valitse **Seuraava**.

7. Valitse **Hae tilapäinen salasana** -valintaikkunan sivulla **Luo**.

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-servicenow-tutorial/create_aaduser_07.png) 

8. **Hae tilapäinen salasana** -valintaikkunan sivulla toimimalla seuraavasti:

    ![Azure AD-testikäyttäjän luominen](./media/active-directory-saas-servicenow-tutorial/create_aaduser_08.png) 

    a. Kirjoita **Uusi salasana**arvo muistiin.

    b. Valitse **Valmis**.   


### <a name="creating-a-servicenow-test-user"></a>ServiceNow testikäyttäjän luominen

Tässä osassa nimeltä Britta Simon ServiceNow käyttäjän luominen. Tässä osassa nimeltä Britta Simon ServiceNow käyttäjän luominen. Jos et tiedä käyttäjän lisäämisestä ServiceNow tai ServiceNow Express-tilisi, ota ServiceNow tukihenkilöstöön.

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD-testikäyttäjän määrittäminen

Tässä osassa voit ottaa käyttöön Britta Simon käyttämään Azure kertakirjautumisen hänen käyttöoikeuksien myöntämistä ServiceNow.

![Määrittää käyttäjälle][200] 

**Jos haluat määrittää Britta Simon ServiceNow, toimi seuraavasti:**

1. Perinteinen-portaalissa Avaa sovellukset-näkymässä kansio-näkymässä, valitse **sovellukset** yläreunan-valikossa.

    ![Määrittää käyttäjälle][201] 

2. Valitse sovellukset-luettelosta **ServiceNow**.

    ![Kertakirjautumisen määrittäminen](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_10.png) 

1. Valitse ylä-valikossa Valitse **käyttäjiä**.

    ![Määrittää käyttäjälle][203] 

1. Valitse kaikki käyttäjät-luettelosta **Britta Simon**.

2. Napsauta työkalurivin alaosassa **määrittäminen**.

    ![Määrittää käyttäjälle][205]


### <a name="testing-single-sign-on"></a>Kertakirjautumisen testaaminen

Tässä osassa tavoitteena on Testaa Azure AD yksittäisen Sign-asetusten käyttäminen Access-paneeli.

Access-ruudussa ServiceNow-ruutua napsauttaessasi sinun pitäisi saada automaattisesti kirjautunut-on ServiceNow sovelluksen.

## <a name="additional-resources"></a>Lisäresursseja

* [Luettelo siitä, miten voit integroida SaaS sovellusten Azure Active Directory-opetusohjelmat](active-directory-saas-tutorial-list.md)
* [Mikä on sovellusten käyttö- ja kertakirjautumisen Azure Active Directory-hakemistosta?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_04.png


[5]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_05.png
[6]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_06.png
[7]:  ./media/active-directory-saas-servicenow-tutorial/tutorial_general_050.png
[10]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_060.png
[11]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_070.png
[20]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_205.png
