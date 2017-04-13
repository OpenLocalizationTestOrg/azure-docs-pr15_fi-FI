<properties
    pageTitle="Azure Active Directory-B2C: Extensible toimintaperiaatteet | Microsoft Azure"
    description="Aiheen Azure Active Directory-B2C extensible toimintaperiaatteet ja erilaiset käytännön luomisesta"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/24/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-extensible-policy-framework"></a>Azure Active Directory-B2C: Extensible toimintaperiaatteet

## <a name="the-basics"></a>Perusteet

Azure Active Directory (Azure AD) B2C extensible toimintaperiaatteet on palvelun core voimakkuuden. Käytännöt kuvaavat täysin kuluttaja tunnistetietojen kokemukset esimerkiksi kirjautuminen, kirjaudu sisään tai profiilin muokkaaminen. Esimerkiksi ilmoittautuminen käytännön avulla voit hallita toiminnan määrittämällä seuraavat asetukset:

- Tilityypit (yhteisöpalvelujen, kuten Facebook tai paikalliset tilit, kuten sähköpostiosoite), jotka käyttäjät voivat käyttää sovelluksen rekisteröintiä.
- Määritteet (esimerkiksi Etunimi, postinumero ja nettonykyarvo koko) on koottu kuluttaja rekisteröitymisen yhteydessä.
- Monimenetelmäisen todentamisen käyttö.
- Ja-ulkoasu kaikkien sivujen rekisteröitymistoiminto.
- Tietoja (joka luettelot kuin vaateet tunnuksessa), että sovellus vastaanottaa toiminnon suorittaminen lopettaa käytännön.

Voit luoda useita erityyppisiä käytäntöjä vuokraajan ja käyttää niitä sovellustesi tarpeen mukaan. Käytännöt voidaan käyttää sovellusten välillä. Näin kehittäjät voivat määrittää ja muokata kuluttaja tunnistetietojen kokemukset ja mahdollisimman vähän tai niiden koodilla muutoksia ei ole.

Käytännöt ovat käytettävissä yksinkertainen Kehitystyökalut-liittymän kautta. Sovelluksen käynnistää käyttämällä vakio HTTP käyttöoikeuksien pyytäminen (kulkeva käytännön parametrin pyynnössä) käytännön ja vastaanottaa mukautetun tunnuksen kuin vastaus. Esimerkiksi ainoa ero pyytää käynnistettäessä kirjautumisen käytännön, ja ne käynnistettäessä kirjautumisen käytäntö on käytetty "p" kyselymerkkijonoparametrin käytännön nimi:

```

https://login.microsoftonline.com/contosob2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e      // Your registered Application ID
&redirect_uri=https%3A%2F%2Flocalhost%3A44321%2F    // Your registered Reply URL, url encoded
&response_mode=form_post                            // 'query', 'form_post' or 'fragment'
&response_type=id_token
&scope=openid
&nonce=dummy
&state=12345                                        // Any value provided by your application
&p=b2c_1_siup                                       // Your sign-up policy

```

```

https://login.microsoftonline.com/contosob2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e      // Your registered Application ID
&redirect_uri=https%3A%2F%2Flocalhost%3A44321%2F    // Your registered Reply URL, url encoded
&response_mode=form_post                            // 'query', 'form_post' or 'fragment'
&response_type=id_token
&scope=openid
&nonce=dummy
&state=12345                                        // Any value provided by your application
&p=b2c_1_siin                                       // Your sign-in policy

```

Saat lisätietoja policy framework tämä [blogimerkintä](http://blogs.technet.com/b/ad/archive/2015/11/02/a-look-inside-azuread-b2c-with-kim-cameron.aspx).

## <a name="create-a-sign-up-policy"></a>Kirjautuminen-käytännön luominen

Jotta kirjautuminen sovelluksen on kirjautumisen käytännön luomisesta. Tämän käytännön kuvataan, jotka käyttäjät siirtyvät rekisteröitymisen yhteydessä kokemukset ja tunnusten, jonka sovelluksen saavat onnistuneen ilmoittautumisiin sisältö.

1. [Siirry Azure-portaalissa B2C ominaisuudet-sivu seuraavasti](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Valitse **ilmoittautuminen käytännöt**.
3. Valitse **+ Lisää** yläreunaan sivu.
4. **Nimi** määrittää sovelluksen käyttämän kirjautumisen käytännön nimen. Kirjoita esimerkiksi "SiUp".
5. **Tunnistetietojen toimittajat** ja valitse "Sähköpostin kirjautuminen". Vaihtoehtoisesti voit myös valita sosiaalisen tunnistetietojen toimittajat, jos jo määritetty. Valitse **OK**.
6. Valitse **kirjautumisen määritteet**. Tässä voit valita määritteitä, jotka haluat kerätä kuluttaja rekisteröitymisen yhteydessä. Valitse esimerkiksi "Maa/alue", "Nimi" ja "Postinumero". Valitse **OK**.
7. Valitse **sovelluksen vaateet**. Tässä voit valita saatavat, jonka haluat palauttaa tunnusten lähetetään takaisin sovelluksesi onnistuneen kirjautumisen kokemus jälkeen. Valitse esimerkiksi "Nimi", "Tunnistetietojen toimittaja", "Postinumero", "Käyttäjä on uusi" ja "Käyttäjän Objektitunnus".
8. Valitse **Luo**. Huomaa, että juuri luomaasi käytännön näkyy "**B2C_1_SiUp**" ( **B2C\_1\_ ** osa lisätään automaattisesti)- **kirjautumisen käytännöt** -sivu.
9. Avaa käytäntö valitsemalla "**B2C_1_SiUp**".
10. Valitse "Contoso B2C app- **sovellusten** avattavasta ja `https://localhost:44321/` - **vastaa URL-osoite tai uudelleenohjata URI** avattavan luettelon.
11. Valitse **Suorita**. Uusi selaimen välilehti avautuu ja voit suorittaa sovelluksen rekisteröityminen kuluttaja kokemukset avulla.

    > [AZURE.NOTE]
    Kuluu minuuteiksi käytännön luominen ja päivitykset tulevat voimaan.

## <a name="create-a-sign-in-policy"></a>Kirjaudu sisään-käytännön luominen

Jotta sovelluksesi on kirjauduttava, sinun on kirjautumisen käytännön luomisesta. Tämän käytännön kuvataan kuluttajille läpi menevien Kirjaudu sisään- ja tunnuksia, jotka saavat sovelluksen sisällön aikana onnistuneen merkki-laajennukset-kokemukset.

1. [Siirry Azure-portaalissa B2C ominaisuudet-sivu seuraavasti](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Valitse **Kirjaudu sisään käytännöt**.
3. Valitse **+ Lisää** yläreunaan sivu.
4. **Nimi** määrittää sovelluksen käyttämän kirjautumisen käytännön nimen. Kirjoita esimerkiksi "SiIn".
5. **Tunnistetietojen toimittajat** ja valitse "Paikallisen tilin kirjautumisikkuna". Vaihtoehtoisesti voit myös valita sosiaalisen tunnistetietojen toimittajat, jos jo määritetty. Valitse **OK**.
6. Valitse **sovelluksen saatavat**. Tässä voit valita saatavat, jonka haluat palauttaa tunnusten lähetetään takaisin sovelluksesi onnistuneen kirjautuminen jälkeen. Valitse esimerkiksi "Nimi", "Tunnistetietojen toimittaja", "Postinumero" ja "Käyttäjän Objektitunnus". Valitse **OK**.
7. Valitse **Luo**. Huomaa, että juuri luomaasi käytännön näkyy "**B2C_1_SiIn**" ( **B2C\_1\_ ** osa lisätään automaattisesti)- **kirjautumisen käytännöt** -sivu.
8. Avaa käytäntö valitsemalla "**B2C_1_SiIn**".
9. Valitse "Contoso B2C app" **sovellusten** avattavasta ja `https://localhost:44321/` - **vastaa URL-osoite / uudelleenohjata URI** avattavan luettelon.
10. Valitse **Suorita**. Uusi selaimen välilehti avautuu ja voit suorittaa kuluttaja kokemus sovellukseen sisäänkirjautuminen avulla.

    > [AZURE.NOTE]
    Kuluu minuuteiksi käytännön luominen ja päivitykset tulevat voimaan.

## <a name="create-a-sign-up-or-sign-in-policy"></a>Kirjautuminen tai Kirjaudu sisään-käytännön luominen

Tämän käytännön käsittelee yksittäisen määrityksissä sekä kuluttaja ilmoittautuminen ja kirjaudu sisään kokemukset. Käyttäjät ovat johtanut alaspäin tilanteen mukaan oikealle polku (kirjautuminen tai Kirjaudu sisään). Tässä artikkelissa myös tunnuksia, jotka sovelluksen saavat onnistuneen ilmoittautumisiin tai kirjautumiset sisällön.  Koodi-malli ilmoittautuminen tai Kirjaudu sisään-käytännön on [saatavilla tähän](active-directory-b2c-devquickstarts-web-dotnet-susi.md).

1. [Siirry Azure-portaalissa B2C ominaisuudet-sivu seuraavasti](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Valitse **Kirjautuminen tai Kirjaudu sisään käytännöt**.
3. Valitse **+ Lisää** yläreunaan sivu.
4. **Nimi** määrittää sovelluksen käyttämän kirjautumisen käytännön nimen. Kirjoita esimerkiksi "SiUpIn".
5. **Tunnistetietojen toimittajat** ja valitse "Sähköpostin kirjautuminen". Vaihtoehtoisesti voit valita myös sosiaalisten tunnistetietojen toimittajat, jos jo määritetty. Valitse **OK**.
6. Valitse **ilmoittautuminen määritteet**. Tässä voit valita määritteitä, jotka haluat kerätä kuluttaja rekisteröitymisen yhteydessä. Valitse esimerkiksi "Maa/alue", "Nimi" ja "Postinumero". Valitse **OK**.
7. Valitse **sovelluksen vaateita koskevat rajoitukset**. Tässä voit valita saatavat, jonka haluat palauttaa tunnusten lähetetään takaisin sovelluksesi onnistuneen kirjautumisen tai kirjautumisnimi kokemus jälkeen. Valitse esimerkiksi "Nimi", "Tunnistetietojen toimittaja", "Postinumero", "Käyttäjä on uusi" ja "Käyttäjän Objektitunnus".
8. Valitse **Luo**. Huomaa, että juuri luomaasi käytännön näkyy "**B2C_1_SiUpIn**" ( **B2C\_1\_ ** osa lisätään automaattisesti)- **Kirjautuminen tai Kirjaudu sisään** -sivu.
9. Avaa käytäntö valitsemalla "**B2C_1_SiUpIn**".
10. Valitse "Contoso B2C app- **sovellusten** avattavasta ja `https://localhost:44321/` - **vastaa URL-osoite tai uudelleenohjata URI** avattavan luettelon.
11. Valitse **Suorita**. Uusi selaimen välilehti avautuu ja voit suorittaa avulla kirjautuminen tai Kirjaudu sisään kuluttajien kokemus mukaisena.

    > [AZURE.NOTE]
    Kuluu minuuteiksi käytännön luominen ja päivitykset tulevat voimaan.

## <a name="create-a-profile-editing-policy"></a>Profiilin muokkaaminen käytännön luominen

Jotta profiilin muokkaaminen sovelluksessa, sinun on profiilin muokkaaminen käytännön luominen. Tämän käytännön kuvataan kokemukset, jotka käyttäjät siirtyvät profiilin muokkaaminen ja tunnusten, jonka sovelluksen saavat onnistumiseen sisällön aikana.

1. [Siirry Azure-portaalissa B2C ominaisuudet-sivu seuraavasti](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Valitse **profiilin muokkaus käytännöt**.
3. Valitse **+ Lisää** yläreunaan sivu.
4. **Nimi** määrittää profiilin muokkaaminen sovelluksen käyttämä käytännön nimi. Kirjoita esimerkiksi "SiPe".
5. **Tunnistetietojen toimittajat** ja valitse "Sähköpostiosoite". Vaihtoehtoisesti voit myös valita sosiaalisen tunnistetietojen toimittajat, jos jo määritetty. Valitse **OK**.
6. Valitse **Profiilimääritteet**. Tässä voit valita määritteitä, jotka kuluttaja voi tarkastella ja muokata. Valitse esimerkiksi "Maa/alue", "Nimi" ja "Postinumero". Valitse **OK**.
7. Valitse **sovelluksen vaateet**. Tässä voit valita saatavat, jonka haluat palauttaa tunnusten lähetetään takaisin sovelluksesi onnistuneen profiilin muokkaaminen kokemus jälkeen. Valitse esimerkiksi "Nimi" ja "Postinumero".
8. Valitse **Luo**. Huomaa, että juuri luomaasi käytännön näkyy "**B2C_1_SiPe**" ( **B2C\_1\_ ** osa lisätään automaattisesti)- **Profiilin muokkaaminen käytännöt** -sivu.
9. Avaa käytäntö valitsemalla "**B2C_1_SiPe**".
10. Valitse "Contoso B2C app- **sovellusten** avattavasta ja `https://localhost:44321/` - **vastaa URL-osoite tai uudelleenohjata URI** avattavan luettelon.
11. Valitse **Suorita**. Uusi selaimen välilehti avautuu ja voit suorittaa profiilin muokkaaminen sovelluksen kuluttaja-ratkaisun avulla.

    > [AZURE.NOTE]
    Kuluu minuuteiksi käytännön luominen ja päivitykset tulevat voimaan.
    
## <a name="create-a-password-reset-policy"></a>Vaihda salasana-käytännön luominen

Tarvitset tarkasti rajattuja salasanan-sovelluksen käyttöön salasanan palauttaminen käytännön luomisesta. Huomautus vuokraajan laajuinen salasanan valitsinta [tähän](active-directory-b2c-reference-sspr.md) on edelleen käytettävissä kirjautumisen käytännöt. Tämän käytännön kuvataan kokemukset, kuluttajien käydä läpi salasanan ja tunnusten, jonka sovelluksen saavat onnistumiseen sisällön aikana.

1. [Siirry Azure-portaalissa B2C ominaisuudet-sivu seuraavasti](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Valitse **salasanan käytännöt**.
3. Valitse **+ Lisää** yläreunaan sivu.
4. **Nimi** määrittää salasanan sovelluksen käyttämä käytännön nimi. Kirjoita esimerkiksi "Vaihtaminen".
5. **Tunnistetietojen toimittajat** ja valitse "Sähköpostiosoitteella salasanan". Valitse **OK**.
6. Valitse **sovelluksen vaateet**. Tässä voit valita saatavat, jonka haluat palauttaa tunnusten lähetetään takaisin sovelluksesi, kun onnistuneen salasanan vaihtamista koskeva toiminto. Valitse esimerkiksi "Käyttäjän Objektitunnus".
7. Valitse **Luo**. Huomaa, että juuri luomaasi käytännön näkyy "**B2C_1_SSPR**" ( **B2C\_1\_ ** osa lisätään automaattisesti)- **salasanan käytännöt** -sivu.
8. Avaa käytäntö valitsemalla "**B2C_1_SSPR**".
9. Valitse "Contoso B2C app- **sovellusten** avattavasta ja `https://localhost:44321/` - **vastaa URL-osoite tai uudelleenohjata URI** avattavan luettelon.
10. Valitse **Suorita**. Uusi selaimen välilehti avautuu ja voit suorittaa avulla salasanan palauttaminen kuluttaja kokemus-sovelluksessa.

    > [AZURE.NOTE]
    Kuluu minuuteiksi käytännön luominen ja päivitykset tulevat voimaan.

## <a name="additional-resources"></a>Lisäresursseja

- [Tunnuksen, istunnon ja Sign-määritys](active-directory-b2c-token-session-sso.md).
