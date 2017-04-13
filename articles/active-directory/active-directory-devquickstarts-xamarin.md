<properties
    pageTitle="Azure AD-Xamarin aloittaminen | Microsoft Azure"
    description="Miten voit luoda Xamarin-sovellus, joka integroituu Azure AD kirjauduttaessa ja soittaa Azure AD suojattu ohjelmointirajapinnan käyttäminen OAuth."
    services="active-directory"
    documentationCenter="xamarin"
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="mobile-xamarin"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="dastrock"/>


# <a name="integrate-azure-ad-into-a-xamarin-app"></a>Azure AD integroida Xamarin-sovellus

[AZURE.INCLUDE [active-directory-devquickstarts-switcher](../../includes/active-directory-devquickstarts-switcher.md)]

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Xamarin voit kirjoittaa mobiilisovelluksia C#, joka toimii iOS-, Android, ja Windows (mobiililaitteet ja tietokoneiden). Jos luot sovelluksen käyttämällä Xamarin, Azure AD on yksinkertainen ja selkeä voi todentaa käyttäjille heidän Active Directory-tilin kanssa. Ottaa käyttöön myös sovelluksen tarjoaman suojatusti minkä tahansa verkko-Ohjelmointirajapinnan Azure AD, kuten Office 365-ohjelmointirajapinnan tai Azure-Ohjelmointirajapinnan suojattu.

Xamarin-sovellukset, jotka on suojattu resursseihin Azure AD tarjoaa Active Directory käyttöoikeuksien kirjaston tai ADAL. ADAL on ainoa tarkoitus-aika on sovelluksen access tunnusten pääset helposti. Osoittaa, kuinka helppoa, seuraavassa on kehittää "Directory Searcher" sovelluksen, joka:

-   Toimii iOS, Android, Windows-työpöytä, Windows Phone-ja Windows-kaupan.
- Yksittäisen kannettavat luokkakirjasto (PCL) käyttää todentaa käyttäjät ja tunnusten Azure AD-kaavio-Ohjelmointirajapinta
-   Etsii käyttäjät, joilla on tietyn UPN hakemistosta.

Tarvitset luonnissa valmis toimimasta-sovelluksen avulla:

2. Määritä Xamarin kehitysympäristö
2. Voit rekisteröidä sovelluksesi Azure AD.
3. Asenna ja määritä ADAL.
5. Pyydä tunnusten Azure AD ADAL avulla.

Jos haluat aloittaa, [Lataa rakenne projektin](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/skeleton.zip) tai [Lataa Esimerkki valmiista](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/complete.zip). Jokainen on Visual Studio 2013-ratkaisun. Sinun on myös Azure AD-vuokraajan, jossa voit luoda käyttäjille ja sovelluksen rekisteröiminen. Jos sinulla ei vielä ole vuokraajan, [Lue, miten voit hankkia sen](active-directory-howto-tenant.md).

## <a name="0-set-up-your-xamarin-development-environment"></a>*0. Xamarin kehitysympäristö määrittäminen*
Tässä opetusohjelmassa sisältää iOS-, Android-ja Windows projektia, joten sinun on Visual Studio ja Xamarin yhdessä. Voit luoda tarvitse ympäristön noudattamalla [asennus ja asenna Visual Studio ja Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) MSDN-sivuston kattavat ohjeet. Ohjeisiin Sisällytä materiaali, voit tarkastella lisätietoja Xamarin Odotellessasi voit asennusohjelmaa, joista voit suorittaa varten. 

Kerran prosenttiosuuden tarvittavat asetukset, Avaa ratkaisun Visual Studiossa avulla pääset alkuun. Etsii kuuden projektien: viisi käyttöjärjestelmäkohtaiset projektien ja yksi kannettavat luokkakirjasto, joka jaetaan kaikissa ympäristöissä,`DirectorySearcher.cs`

## <a name="1-register-the-directory-searcher-application"></a>*1. Directory Searcher sovelluksen rekisteröinti*
Sovelluksen hankkiminen tunnusten käyttöön ensin tarvitset rekisteröidään Azure AD-vuokraajan ja Myönnä käyttöoikeudet Azure AD-kaavio-Ohjelmointirajapinnan:

-   Kirjaudu sisään [Azure hallinta-portaalissa](https://manage.windowsazure.com)
-   Valitse vasemmalla olevasta siirtymisruudussa **Active Directory**
-   Valitse vuokraajan, jolla sovellus rekisteröi.
-   Valitse **sovellukset** -välilehti ja valitse **Lisää** ala-laatikon.
-   Seuraa ohjeita ja luo uusi **Alkuperäisen asiakassovellukseen**.
    -   Sovelluksen **nimen** kuvataan sovelluksesi loppukäyttäjille
    -   **Uudelleenohjata Uri** on värimallin ja merkkijonon yhdistelmä, jota Azure AD käyttää suojaustunnuksen vastaukset palautetaan. Kirjoita arvo, kuten `http://DirectorySearcher`.
-   Kun olet suorittanut rekisteröinti, AAD Määritä sovelluksen asiakkaan yksilöllinen tunnus. Tarvitset tätä arvoa seuraavat osiot niin kopioi se **määritys** -välilehti.
- Myös **määrittäminen** -välilehden Etsi "Oikeudet ja muut sovellukset"-kohta. "Azure Active Directory"-sovelluksen Lisää **Valtuutetun käyttöoikeudet**-kohdassa **Access Your organisaation hakemisto** -käyttöoikeus. Tämä ottaa käyttöön sovelluksen käyttäjille Graph-Ohjelmointirajapinnan kysely.

## <a name="2-install--configure-adal"></a>*2. Asenna ja määritä ADAL*
Nyt kun olet luonut Azure AD-sovelluksen, voit asentaa ADAL ja kirjoita käyttäjätietojen liittyvä koodi. Myös ADAL voivat pitää yhteyttä Azure AD-pystyttävä toimittamaan sovelluksen rekisteröinti tietoja.
-   Aloita lisäämällä ADAL kaikki projektit-ratkaisun pakettien hallinta-konsolin avulla.

`
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirectorySearcherLib
`

`
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-Android
`

`
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-Desktop
`

`
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-iOS
`

`
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-Universal
`

- Olisi huomaat, että kirjaston sisällyttävä leikkausoperaattori lisätään jokaiselle project - ADAL PCL-osa ja käyttöjärjestelmäkohtaiset osa.

-   DirectorySearcherLib Projectissa, Avaa `DirectorySearcher.cs`. Muuttaa luokan jäsenen arvot kuvaavat arvoja, jotka olet kirjoittanut Azure-portaaliin. Nämä arvot viittaavat koodisi aina, kun se käyttää ADAL.
    -   `tenant` Azure AD-vuokraajan, esimerkiksi contoso.onmicrosoft.com toimialue
    -   `clientId` On portaalista kopioimasi sovelluksen clientId.
    - `returnUri` On syötetty portaalissa, kuten redirectUri `http://DirectorySearcher`.

## <a name="3--use-adal-to-get-tokens-from-aad"></a>*3. Käytä ADAL tunnusten käyttämistä AAD*
Kaikki sovelluksen käyttöoikeuksien logiikan virheellisesti *Almost* `DirectorySearcher.SearchByAlias(...)`. Kaikki, joita tarvitaan käyttöjärjestelmäkohtaiset projektien on välittää tilannekohtaiset parametri `DirectorySearcher` PCL.

- Avaa ensin `DirectorySearcher.cs` ja Lisää uusi parametri `SearchByAlias(...)` menetelmää. `IPlatformParameters`on tilannekohtaiset parametri, joka kapseloi käyttöjärjestelmäkohtaiset objekteja, jotka ADAL on suoritettava todennusta.

```C#
public static async Task<List<User>> SearchByAlias(string alias, IPlatformParameters parent)
{
```

-   Seuraavaksi alusta `AuthenticationContext` -ADAL on ensisijainen luokka. Tämä on missä ADAL välittää pitää yhteyttä Azure AD tarvitaan koordinaatit. Valitse Soita `AcquireTokenAsync(...)`, joka hyväksyy `IPlatformParameters` objekti ja käynnistää tarvittavat tunnusta palaa sovelluksen todennus-työnkulku.

```C#
...
    AuthenticationResult authResult = null;
    try
    {
        AuthenticationContext authContext = new AuthenticationContext(authority);
        authResult = await authContext.AcquireTokenAsync(graphResourceUri, clientId, returnUri, parent);
    }
    catch (Exception ee)
    {
        results.Add(new User { error = ee.Message });
        return results;
    }
...
```
- `AcquireTokenAsync(...)`ensin yrittää palauttaa tunnuksen Pyydetty resurssin (Tässä tapauksessa Graph-API) kysymättä käyttäjä voi syöttää tunnistetietoja (joko välimuistiin tallentaminen tai päivittäminen vanha tunnusten). Vain, jos se on tarpeen, se näkyy käyttäjän Azure AD-merkki sivun ennen pyydetty tunnuksen hankkiminen.


- Voit liittää käyttöoikeustietue sitten Authorization-otsikko Graph API-pyyntö:

```C#
...
    request.Headers.Authorization = new AuthenticationHeaderValue("Bearer", authResult.AccessToken);
...
```

Kaikki on varten `DirectorySearcher` PCL ja sovelluksen käyttäjän tunnistetiedot liittyvät koodi.  Kaikki pysyy on Soita `SearchByAlias(...)` menetelmä eri ympäristöissä näkymissä ja jossa käsittelyyn oikein Käyttöliittymän elinkaari-koodin lisääminen tarvittaessa.

####<a name="android"></a>Android:
- Valitse `MainActivity.cs`, puhelun `SearchByAlias(...)` Valitse painikkeen käsittely:

```C#
List<User> results = await DirectorySearcher.SearchByAlias(searchTermText.Text, new PlatformParameters(this));
```
- Sinun on myös ohittaa `OnActivityResult` elinkaari tapa välittää minkä tahansa todennus ohjaa takaisin sopiva menetelmä.  ADAL on avustaja-menetelmä tälle Android:

```C#
...
protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
{
    base.OnActivityResult(requestCode, resultCode, data);
    AuthenticationAgentContinuationHelper.SetAuthenticationAgentContinuationEventArgs(requestCode, resultCode, data);
}
...
```

####<a name="windows-desktop"></a>Windows-työpöytä:
- - `MainWindow.xaml.cs`, Riittää, että voit soittaa `SearchByAlias(...)` kulkeva `WindowInteropHelper` työpöydän `PlatformParameters` objekti:

```C#
List<User> results = await DirectorySearcher.SearchByAlias(
  SearchTermText.Text,
  new PlatformParameters(PromptBehavior.Auto, this.Handle));
```

####<a name="ios"></a>iOS:
- Valitse `DirSearchClient_iOSViewController.cs`, iOS `PlatformParameters` objekti on yksinkertaisesti näkymä-ohjain viittaus:

```C#
List<User> results = await DirectorySearcher.SearchByAlias(
  SearchTermText.Text,
  new PlatformParameters(PromptBehavior.Auto, this.Handle));
```

####<a name="windows-universal"></a>Windowsin Universal:
- Windowsin yleinen Avaa `MainPage.xaml.cs` ja käyttöönotto `Search` menetelmä, jota käytetään jaetun projektin helper menetelmän Käyttöliittymän tarpeen mukaan update.

```C#
...
    List<User> results = await DirectorySearcherLib.DirectorySearcher.SearchByAlias(SearchTermText.Text, new PlatformParameters(PromptBehavior.Auto, false));
...
```

Onnittelen! Toimiva Xamarin-sovellusta, joka on voi todentaa käyttäjät ja soita suojatusti OAuth 2.0 käyttämällä viittä eri ympäristöissä eri verkko-ohjelmointirajapinnan on nyt. Jos et ole jo, nyt on aika, joka täyttää alihallintaan joidenkin käyttäjien kanssa. Suorita DirectorySearcher-sovellus ja kirjaudu sisään jokin kyseisille käyttäjille. Etsi muiden käyttäjien niiden UPN perusteella.

ADAL voi liittää sovelluksen yleisiä identity-ominaisuuksia on helppoa. Muokattu-työstä kestää huolellisesti, - välimuistin hallinnan, OAuth-protokollatuki esitykset käyttäjän kirjautuminen Käyttöliittymä, päivittäminen ja vanhentuneet tunnusten. Kaikki on varmasti tiedettävä on yksittäinen API-kutsu `authContext.AcquireToken*(…)`.

Viittaus (ilman määritysarvot) Esimerkki valmiista toimitetaan [Tässä](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/complete.zip). Voit nyt siirtää Lisää tunnistetiedot skenaarioita. Voit halutessasi kokeilla:

[Suojattu .NET verkko-Ohjelmointirajapinnan kanssa Azure AD >>](active-directory-devquickstarts-webapi-dotnet.md)

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
