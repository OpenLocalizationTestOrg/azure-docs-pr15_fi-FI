<properties
    pageTitle="Azure Active Directory-B2C: Sivun Käyttöliittymän mukauttaminen helper työkalun | Microsoft Azure"
    description="Avustaja-työkalu, jolla osoittamaan Azure Active Directory-B2C sivun Käyttöliittymän mukauttaminen-ominaisuus"
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
    ms.date="07/22/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-a-helper-tool-used-to-demonstrate-the-page-user-interface-ui-customization-feature"></a>Azure Active Directory-B2C: Helper työkalu, jolla osoittamaan sivun käyttäjän käyttöliittymän mukauttaminen-ominaisuus

Tässä artikkelissa on Azure Active Directory (Azure AD) B2C [Käyttöliittymän mukauttaminen pääartikkeli](active-directory-b2c-reference-ui-customization.md) -avustaja. Seuraavassa kuvataan, miten oikeuksien sivun Käyttöliittymän mukauttaminen-ominaisuus käyttämällä HTML- ja CSS-Esimerkki sisältöön, joka on määritetty.

## <a name="get-an-azure-ad-b2c-tenant"></a>Hae Azure AD B2C vuokraajan

Ennen kuin voit mukauttaa mitään, sinun on saat [Azure AD B2C vuokraajan](active-directory-b2c-get-started.md) , jos sinulla ei ole vielä yksi.

## <a name="create-a-sign-up-or-sign-in-policy"></a>Kirjautuminen tai Kirjaudu sisään-käytännön luominen

Esimerkkisisältö on määritetty voidaan customze kahden sivujen [Kirjautuminen tai Kirjaudu sisään](active-directory-b2c-reference-policies.md): [yhdistetty kirjautumissivulla](active-directory-b2c-reference-ui-customization.md) ja [sertifikaattiin itse määritteet-sivu](active-directory-b2c-reference-ui-customization.md). Kun [Kirjautuminen tai Kirjaudu sisään-käytännön luominen](active-directory-b2c-reference-policies.md#create-a-sign-up-or-sign-in-policy), Lisää paikallistilin (sähköpostiosoite), Facebook, Google ja Microsoft **tunnistetietojen toimittajat**. Nämä ovat vain IDPs, joka hyväksyy tämän otoksen HTML-sisällön.  Voit myös lisätä nämä IDPs alijoukkoa, jos haluat, että.

## <a name="register-an-application"></a>Sovelluksen rekisteröiminen

Sinun on [sovelluksen](active-directory-b2c-app-registration.md) rekisteröiminen, jonka avulla voidaan suorittaa käytäntöjen B2C vuokraajan. Kun sovellus, sinulla on useita vaihtoehtoja, joiden avulla voit suorittaa kirjautumisen käytäntö:

- Muodosta jokin Azure AD-B2C quick start-sovellusten luettelossa "aloittaminen-osassa [Kirjaudu ylös ja kirjaudu sisään sovelluksesi käyttäjät](active-directory-b2c-overview.md#getting-started).
- Käyttää valmiita [Azure AD B2C leikkikenttä](https://aadb2cplayground.azurewebsites.net) -sovellusta. Jos haluat käyttää leikkikenttä, sinun on rekisteröitävä sovelluksen käyttämällä **uudelleenohjata URI** B2C vuokraajan `https://aadb2cplayground.azurewebsites.net/`.
- Käytä käytäntöjen [Azure portal](https://portal.azure.com/)- **Suorita** -painiketta.

## <a name="customize-your-policy"></a>Käytäntöjen mukauttaminen

Käytäntöjen käyttötuntuman mukauttamiseen sinun on luotava Azure AD B2C tietyn nimeämiskäytännön HTML- ja CSS tiedostot. Voit ladata staattiseksi sisällöksi sitten yleiseen käyttöön sijaintiin niin, että Azure AD B2C voit käyttää sitä. Tämä voi olla oma web-palvelin, Azure-Blob-säiliö, Azure sisällön toimituksen verkossa tai minkä tahansa muun staattinen resurssin isännöinnin palveluntarjoajan. Vain vaatimuksia sisältöä on käytettävissä HTTPS ja niitä voi käyttää CORS avulla. Kun olet tarjoamia staattiset sisältöä verkossa, voit muokata käytäntöjen tämän sijainnin ja esittää sisältöä asiakkaillesi. [Käyttöliittymän mukauttaminen pääartikkeli](active-directory-b2c-reference-ui-customization.md) kuvataan yksityiskohtaisesti, kuinka Azure AD B2C mukauttaminen-ominaisuus toimii.

Tässä opetusohjelmassa tarkoitetaan on olet jo luotuihin otoksen sisältöä ja isännöi Azure-Blob-objektien tallennustilaan. Esimerkkisisältö on hyvin basic mukauttaminen teeman, tutustu kuvitteelliseen yritykseen "Wingtip Toys". Voit kokeilla sitä käytännössä oman, toimimalla seuraavasti:

1. [Azure portal](https://portal.azure.com/) alihallintaan kirjautuminen ja siirry B2C ominaisuudet-sivu.
2. **Kirjautuminen tai Kirjaudu sisään** ja valitse sitten käytäntöjen (esimerkiksi "b2c\_1\_Kirjaudu\_määrittäminen\_Kirjaudu\_-").
3. Valitse **sivun Käyttöliittymän mukauttaminen** ja **yhdistetty kirjautuminen tai Kirjaudu sisään-sivulla**.
4. Ottaa **käyttöön mukautettu sivu** Vaihda arvoksi **Kyllä**. Lisää **Mukautettu sivu URI** -kenttään `https://wingtiptoysb2c.blob.core.windows.net/b2c/wingtip/unified.html`. Valitse **OK**.
5. Valitse **paikallisen tilin kirjautumissivulle**. Vaihda **Käytä mukautettua mallia** Vaihda arvoksi **Kyllä**. Lisää **Mukautettu sivu URI** -kenttään `https://wingtiptoysb2c.blob.core.windows.net/b2c/wingtip/selfasserted.html`.
5. Toista sama vaihe **sosiaalisen tilin kirjautumissivulle**.
 Valitse **OK** kahdesti, jos haluat sulkea Käyttöliittymän mukauttaminen näiden.
6. Valitse **Tallenna**.

Voit nyt kokeilla mukautetun käytännön. Voit käyttää itse sovelluksen tai Azure AD B2C leikkikenttä, jos haluat, mutta voit myös valita käytännön sivu **Suorita** -komentoa. Valitse sovelluksen avattavan luetteloruudun ja valitse haluamasi uudelleenohjaus URI. Napsauta **Suorita** -painiketta. Uusi selaimen välilehti avautuu ja voit suorittaa avulla, ja uusi sisältö on lisätty sovelluksen rekisteröityminen käyttäjäkokemus!

## <a name="upload-the-sample-content-to-azure-blob-storage"></a>Esimerkki sisällön lataaminen Azure-Blob-säiliö

Jos haluat käyttää Azure-Blob-säiliö isännöimiseen sivun sisältöä, voit luoda omia tallennustilan tilin ja lataa tiedostot sekä B2C avustaja-työkalun avulla.

### <a name="create-a-storage-account"></a>Tallennustilan tilin luominen

1. Kirjautuminen [Azure portal](https://portal.azure.com/).
2. Valitse **+ Uusi** > **tietojen + tallennustilan** > **tallennustilan tilin**. Sinun on Azure tilauksen Azure-Blob-säiliö tilin luominen. Voit rekisteröityä maksuttoman kokeiluversion [Azure sivuston](https://azure.microsoft.com/pricing/free-trial/).
3. Anna **nimi** tallennustilan tilin (esimerkiksi "contoso") ja valitse haluamasi vaihtoehdot **hinnoittelu taso**, **resurssiryhmä** ja **tilauksen**. Varmista, että sinulla on **Startboard Kiinnitä** -asetus on valittuna. Valitse **Luo**.
4. Palaa Startboard ja valitse juuri luomasi tallennustilan tili.
5. **Yhteenveto** -osassa **säilöjen**ja valitse sitten **+ Lisää**.
6. Anna **nimi** säilön (esimerkiksi "b2c") ja valitse **Blob-objektien** **käyttöoikeustyyppi**. Valitse **OK**.
7. Säilön, jonka loit näkyvät **BLOB** -sivu-luettelossa. Kirjoita muistiin säilö; URL-osoite esimerkiksi pitäisi nyt muistuttaa `https://contoso.blob.core.windows.net/b2c`. Sulje **BLOB** -sivu.
8. Valitse tallennustilan tili-sivu **näppäimet** ja kirjoita se muistiin **Tallennustilan tilin nimi** ja **Perusavain Access** -kenttien arvot.

1. Kirjautuminen [Azure portal](https://portal.azure.com/).
2. Valitse **+ Uusi** > **tietojen + tallennustilan** > **tallennustilan tilin**. Sinun on Azure tilauksen Azure-Blob-säiliö tilin luominen. Voit rekisteröityä maksuttoman kokeiluversion [Azure sivuston](https://azure.microsoft.com/pricing/free-trial/).
3. Valitse **Tilin tyyppi** **-Blob-säiliö** ja jätä muut arvot oletukseksi.  Voit muokata resurssiryhmä & sijainti halutessasi.  Valitse **Luo**.
4. Palaa Startboard ja valitse juuri luomasi tallennustilan tili.
5. Valitse **Yhteenveto** -osassa **+ säilö**.
6. Anna **nimi** säilön (esimerkiksi "b2c") ja valitse **Blob-objektien** **käyttöoikeustyyppi**. Valitse **OK**.
7. Avaa säilö **Ominaisuudet**ja kirjoita muistiin säilö; URL-osoite esimerkiksi pitäisi nyt muistuttaa `https://contoso.blob.core.windows.net/b2c`. Sulje säilö-sivu.
8. Valitse tallennustilan tili-sivu **Avain kuvake** ja kirjoita se muistiin **Tallennustilan tilin nimi** ja **Perusavain Access** -kenttien arvot.

> [AZURE.NOTE]
    **Access-perusavain** on tärkeän tunnistetiedon.

### <a name="download-the-helper-tool-and-sample-files"></a>Lataa työkalu ja otoksen aputiedostot

Voit ladata [Azure-Blob-säiliö työkalu ja otoksen aputiedostot .zip-tiedostona](https://github.com/azureadquickstarts/b2c-azureblobstorage-client/archive/master.zip) tai Kloonaa GitHub kohteesta:

```
git clone https://github.com/azureadquickstarts/b2c-azureblobstorage-client
```

Tämä säilö sisältää `sample_templates\wingtip` kansio, joka sisältää esimerkiksi HTML, CSS-koodin ja kuvat. Näiden mallien viittaamaan Azure-Blob-säiliö-tilin sinun on HTML-tiedostojen muokkaamiseen. Avaa `unified.html` ja `selfasserted.html` ja korvaa kaikki esiintymät `https://localhost` ja oman säilö, joka edellisessä vaiheessa kirjoittamasi URL-osoite. Sinun on käytettävä absoluuttisen polun HTML-tiedostoja, koska tällöin ja served HTML mukaan Azure AD-kohdassa toimialueen `https://login.microsoftonline.com`.

### <a name="upload-the-sample-files"></a>Tiedostojen lataaminen

Saman säilössä unzip `B2CAzureStorageClient.zip` ja suorita `B2CAzureStorageClient.exe` tiedoston sisällä. Ohjelma vain lisätä hakemiston, voit määrittää tilisi tallennustilan tiedostot ja CORS käytön kyseiset tiedostot. Jos olet toiminut edellä kuvatut vaiheet, HTML ja CSS-tiedostojen nyt valitsemalla tallennustilan-tiliisi. Huomaa, että tallennustilan tilin nimi on osa, joka edeltää `blob.core.windows.net`; esimerkiksi `contoso`. Voit varmistaa, että sisältö on ladattu oikein yrittää käyttää `https://{storage-account-name}.blob.core.windows.net/{container-name}/wingtip/unified.html` selaimen. Myös varmistaa, että sisältö on nyt käytössä CORS [http://test-cors.org/](http://test-cors.org/) avulla. (Etsi "XHR tila: 200" tuloksessa.)

### <a name="customize-your-policy-again"></a>Mukauta käytäntöjen, uudelleen

Nyt kun lisättyjen otoksen sisällön tallennustilan-tiliisi kirjautuminen käytäntöjen voit viitata on muokattava. Toista vaiheet yllä ["Mukauta käytäntöjen"](#customize-your-policy) -osasta tällä kertaa käyttäen tallennustilan oman tilin URL-osoitteet. Esimerkiksi sijainti oman `unified.html` tiedoston olisi `<url-of-your-container>/wingtip/unified.html`.

Voit nyt käyttää **Suorita** -painike tai oman sovelluksen suorittamaan käytäntöjen uudelleen. Tulos pitäisi näyttää lähes täsmälleen sama – voit käyttää samoja Esimerkki HTML- ja CSS kummassakin tapauksessa. Oman käytännöt nyt viittaat oman esiintymässä Azure-Blob-säiliö, ja voit muokata ja ladata tiedostoja uudelleen tavalliseen tapaan Ota. Lisätietoja HTML- ja CSS mukauttamisesta on lisätietoja [Käyttöliittymän mukauttaminen pääartikkeli](active-directory-b2c-reference-ui-customization.md).
