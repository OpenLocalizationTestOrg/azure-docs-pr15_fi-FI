<properties
    pageTitle="Windows Azure AD-kaupan aloittaminen | Microsoft Azure"
    description="Windows-kaupan sovellus, jossa Azure AD kirjauduttaessa integroituu ja soittaa Azure AD muodostaminen suojattu ohjelmointirajapinnan käyttäminen OAuth."
    services="active-directory"
    documentationCenter="windows"
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="mobile-windows-store"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="dastrock"/>


# <a name="integrate-azure-ad-with-a-windows-store-app"></a>Integroi Azure AD Windows-kaupan sovellus

[AZURE.INCLUDE [active-directory-devquickstarts-switcher](../../includes/active-directory-devquickstarts-switcher.md)]

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Jos kehität sovelluksen Windows-kaupan, Azure AD on yksinkertainen ja selkeä voi todentaa käyttäjille heidän Active Directory-tilin kanssa.  Ottaa käyttöön myös sovelluksen tarjoaman suojatusti minkä tahansa verkko-Ohjelmointirajapinnan Azure AD, kuten Office 365-ohjelmointirajapinnan tai Azure-Ohjelmointirajapinnan suojattu.

Windows-kaupan Työpöytäsovellukset, joka on suojattu resursseihin Azure AD tarjoaa Active Directory käyttöoikeuksien kirjaston tai ADAL.  ADAL on ainoa tarkoitus-aika on sovelluksen access tunnusten pääset helposti.  Osoittaa, kuinka helppoa, tähän on kehittää "Directory Searcher" Windows-kaupan sovelluksen, joka:

-   Saa käyttää tunnusten soitettavien Azure AD-kaavio-Ohjelmointirajapinnan [OAuth 2.0 todennus-protokollan](https://msdn.microsoft.com/library/azure/dn645545.aspx)avulla.
-   Etsii käyttäjät, joilla on tietyn UPN hakemistosta.
-   Etumerkki käyttäjät ulos.

Tarvitset luonnissa valmis toimimasta-sovelluksen avulla:

2. Voit rekisteröidä sovelluksesi Azure AD.
3. Asenna ja määritä ADAL.
5. Pyydä tunnusten Azure AD ADAL avulla.

Jos haluat aloittaa, [Lataa rakenne projektin](https://github.com/AzureADQuickStarts/NativeClient-WindowsStore/archive/skeleton.zip) tai [Lataa Esimerkki valmiista](https://github.com/AzureADQuickStarts/NativeClient-WindowsStore/archive/complete.zip).  Kunkin on Visual Studio 2015 ratkaisu.  Sinun on myös Azure AD-vuokraajan, jossa voit luoda käyttäjille ja sovelluksen rekisteröiminen.  Jos sinulla ei vielä ole vuokraajan, [Lue, miten voit hankkia sen](active-directory-howto-tenant.md).

## <a name="1-register-the-directory-searcher-application"></a>*1. Directory Searcher sovelluksen rekisteröinti*
Sovelluksen hankkiminen tunnusten käyttöön ensin tarvitset rekisteröidään Azure AD-vuokraajan ja Myönnä käyttöoikeudet Azure AD-kaavio-Ohjelmointirajapinnan:

-   Kirjaudu sisään [Azure hallinta-portaalissa](https://manage.windowsazure.com)
-   Valitse vasemmalla olevasta siirtymisruudussa **Active Directory**
-   Valitse vuokraajan, jolla sovellus rekisteröi.
-   Valitse **sovellukset** -välilehti ja valitse **Lisää** ala-laatikon.
-   Seuraa ohjeita ja luo uusi **Alkuperäisen asiakassovellukseen**.
    -   Sovelluksen **nimen** kuvataan sovelluksesi loppukäyttäjille
    -   **Uudelleenohjata Uri** on värimallin ja merkkijonon yhdistelmä, jota Azure AD käyttää suojaustunnuksen vastaukset palautetaan.  Kirjoita paikkamerkin arvoa nyt, kuten `http://DirectorySearcher`.  Olemme korvaa tämän arvon myöhemmin.
-   Kun olet suorittanut rekisteröinti, AAD Määritä sovelluksen asiakkaan yksilöllinen tunnus.  Tarvitset tätä arvoa seuraavat osiot niin kopioi se **määritys** -välilehti.
- Myös **määrittäminen** -välilehden Etsi "Oikeudet ja muut sovellukset"-kohta.  Lisää "Azure Active Directory-sovellus **kirjautunut sisään käyttäjänä hakemiston** käyttöoikeus **Valtuutetun käyttöoikeudet**-kohdassa.  Tämä ottaa käyttöön sovelluksen käyttäjille Graph-Ohjelmointirajapinnan kysely.

## <a name="2-install--configure-adal"></a>*2. Asenna ja määritä ADAL*
Nyt kun olet luonut Azure AD-sovelluksen, voit asentaa ADAL ja kirjoita käyttäjätietojen liittyvä koodi.  Myös ADAL voivat pitää yhteyttä Azure AD-pystyttävä toimittamaan sovelluksen rekisteröinti tietoja.
-   Aloita lisäämällä ADAL DirectorySearcher projektin pakettien hallinta-konsolin avulla.

```
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
```

-   DirectorySearcher Projectissa, Avaa `MainPage.xaml.cs`.  Korvaa arvot `Config Values` alueen kuvaavat arvoja, jotka olet kirjoittanut Azure-portaaliin.  Nämä arvot viittaavat koodisi aina, kun se käyttää ADAL.
    -   `tenant` Azure AD-vuokraajan, esimerkiksi contoso.onmicrosoft.com toimialue
    -   `clientId` On portaalista kopioimasi sovelluksen clientId.
-   Sinun täytyy nyt Tutustu Windows-kaupan sovelluksen takaisinsoitto uri.  Määrittää keskeytyskohdasta rivillä `MainPage` menetelmää:

```
redirectURI = Windows.Security.Authentication.Web.WebAuthenticationBroker.GetCurrentApplicationCallbackUri();
```
- Luoda ratkaisun, varmistetaan, että kaikki paketin viittaukset palautetaan.  Jos paketit puuttuvat, Avaa Nuget pakettien hallinta-asetukset ja palauttaminen paketit.
- Suorita sovellus ja kopioi varaa arvon `redirectUri` kun keskeytyskohdasta viesti.  Pitäisi näyttää suunnilleen

```
ms-app://s-1-15-2-1352796503-54529114-405753024-3540103335-3203256200-511895534-1429095407/
```

- Takaisin-välilehden **Määritä** sovelluksesi Azure hallinta-portaalissa korvaa **RedirectUri** arvo arvoksi.  

## <a name="3--use-adal-to-get-tokens-from-aad"></a>*3. Käytä ADAL tunnusten käyttämistä AAD*
Perustietoja tarpeen ADAL takana on, että aina, kun sovellus on access-tunnuksen, riittää, että kutsuu `authContext.AcquireToken(…)`, eikä ADAL muille käyttäjille.  

-   Ensimmäiseksi on valmistella sinua sovelluksen `AuthenticationContext` -ADAL on ensisijainen luokka.  Tämä on missä ADAL välittää täytyy Azure AD yhteydenpito ja kuinka se välimuistiin tunnusten koordinaatit.

```C#
public MainPage()
{
    ...

    authContext = new AuthenticationContext(authority);
}
```

- Selaa `Search(...)` menetelmä, joka käynnistyy, kun käyttäjä napsauttaa sovelluksen käyttöliittymässä "Etsi"-painiketta.  Tämä menetelmä tekee GET-pyynnössä kyselyyn Azure AD-kaavio-Ohjelmointirajapinnan käyttäjille, joiden UPN alkaa tietyllä hakusanoja.  Mutta kyselyn Graph-Ohjelmointirajapinta, jotta sinun täytyy lisätä access_token- `Authorization` otsikon tämä on pyynnön - kohtaa, johon ADAL on.

```C#
private async void Search(object sender, RoutedEventArgs e)
{
    ...
    AuthenticationResult result = null;
    try
    {
        result = await authContext.AcquireTokenAsync(graphResourceId, clientId, redirectURI, new PlatformParameters(PromptBehavior.Auto, false));
    }
    catch (AdalException ex)
    {
        if (ex.ErrorCode != "authentication_canceled")
        {
            ShowAuthError(string.Format("If the error continues, please contact your administrator.\n\nError: {0}\n\nError Description:\n\n{1}", ex.ErrorCode, ex.Message));
        }
        return;
    }
    ...
}
```
- Kun sovellus pyytää tunnusta soittamalla `AcquireTokenAsync(...)`, ADAL yrittää palauttaa tunnusta kysymättä käyttäjän tunnistetiedot.  Jos ADAL määrittää, että käyttäjän täytyy kirjautua saat tunnusta, sisäänkirjautuminen-valintaikkunan näkyviin, kerätä käyttäjän tunnistetiedot ja palauttaa tunnuksen yhteydessä todentaminen onnistuu.  Jos ADAL ei voi palauttaa tunnusta jostakin syystä `AuthenticationResult` tila on virhe.

- Nyt kannattaa käyttää juuri hankittu access_token.  Myös tässä `Search(...)` -menetelmä tunnuksen liittäminen Graph API saat pyyntöä Authorization-otsikko:

```C#
// Add the access token to the Authorization Header of the call to the Graph API, and call the Graph API.
httpClient.DefaultRequestHeaders.Authorization = new HttpCredentialsHeaderValue("Bearer", result.AccessToken);

```
- Voit käyttää myös `AuthenticationResult` objekti käyttäjän tietoja sovelluksessa, kuten käyttäjän tunnus:

```C#
// Update the Page UI to represent the signed in user
ActiveUser.Text = result.UserInfo.DisplayableId;
```
- Lopuksi voit kirjautua ulos sovelluksesta sekä käyttäjän ADAL.  Kun käyttäjä napsauttaa "Kirjaudu ulos"-painiketta, haluamme varmistaa, että seuraava puhelu `AcquireTokenAsync(...)` näkyy merkki näkymässä.  ADAL tämä on yhtä helppoa kuin suojaustunnuksen välimuistin tyhjentäminen:

```C#
private void SignOut()
{
    // Clear session state from the token cache.
    authContext.TokenCache.Clear();

    ...
}
```

Onnittelen! Sinun on nyt toimiva Windows-kaupan sovellus, jossa on voi todentaa käyttäjät, Soita suojatusti verkko-ohjelmointirajapinnan käyttäminen OAuth 2.0, ja poistaa käyttäjän perustietoja.  Jos et ole jo, nyt on aika, joka täyttää alihallintaan joidenkin käyttäjien kanssa.  Suorita DirectorySearcher-sovellus ja kirjaudu sisään jokin kyseisille käyttäjille.  Etsi muiden käyttäjien niiden UPN perusteella.  Sulje sovellus ja käyttää sitä.  Huomaa, miten käyttäjän istunnon säilyy muuttumattomana.  Kirjaudu (napsauttamalla alareunaan näkyviin) ja kirjaudu takaisin sisään toiselle käyttäjälle.

ADAL voi liittää kaikkia näitä yleisiä identity-ominaisuuksia sovelluksesi on helppoa.  Muokattu-työstä kestää huolellisesti, - välimuistin hallinnan, OAuth-protokollatuki esitykset käyttäjän kirjautuminen Käyttöliittymä, päivittäminen ja vanhentuneet tunnusten.  Kaikki on varmasti tiedettävä on yksittäinen API-kutsu `authContext.AcquireToken*(…)`.

Viittaus (ilman määritysarvot) Esimerkki valmiista toimitetaan [Tässä](https://github.com/AzureADQuickStarts/NativeClient-WindowsStore/archive/complete.zip).  Voit nyt siirtää Lisää tunnistetiedot skenaarioita.  Voit halutessasi kokeilla:

[Suojattu .NET verkko-Ohjelmointirajapinnan kanssa Azure AD >>](active-directory-devquickstarts-webapi-dotnet.md)

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
