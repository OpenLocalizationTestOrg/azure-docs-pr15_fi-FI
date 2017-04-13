<properties
    pageTitle="Azure AD-.NET aloittaminen | Microsoft Azure"
    description=".NET Windows-työpöytä-sovellus, joka integroituu Azure AD kirjauduttaessa ja soittaa Azure AD muodostaminen suojattu ohjelmointirajapinnan käyttäminen OAuth."
    services="active-directory"
    documentationCenter=".net"
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="dastrock"/>


# <a name="integrate-azure-ad-into-a-windows-desktop-wpf-app"></a>Integroi työpöydän WPF-sovellus Windows Azure AD

[AZURE.INCLUDE [active-directory-devquickstarts-switcher](../../includes/active-directory-devquickstarts-switcher.md)]

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Jos kehität työpöytäsovellus, Azure AD on yksinkertainen ja selkeä voi todentaa käyttäjille heidän Active Directory-tilin kanssa.  Ottaa käyttöön myös sovelluksen tarjoaman suojatusti minkä tahansa verkko-Ohjelmointirajapinnan Azure AD, kuten Office 365-ohjelmointirajapinnan tai Azure-Ohjelmointirajapinnan suojattu.

.NET alkuperäisen asiakkaille, jotka on suojattu resursseihin Azure AD on Active Directory käyttöoikeuksien kirjaston tai ADAL.  ADAL on ainoa tarkoitus-aika on sovelluksen access tunnusten pääset helposti.  Osoittaa, kuinka helppoa, tähän on muodostaa .NET WPF tehtäväluettelo-sovelluksen, seuraavasti:

-   Saa käyttää tunnusten soitettavien Azure AD-kaavio-Ohjelmointirajapinnan [OAuth 2.0 todennus-protokollan](https://msdn.microsoft.com/library/azure/dn645545.aspx)avulla.
-   Etsii käyttäjät, joilla on tietyn alias hakemistosta.
-   Etumerkki käyttäjät ulos.

Tarvitset luonnissa valmis toimimasta-sovelluksen avulla:

2. Voit rekisteröidä sovelluksesi Azure AD.
3. Asenna ja määritä ADAL.
5. Pyydä tunnusten Azure AD ADAL avulla.

Jos haluat aloittaa, [Lataa sovelluksen rakenne](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/skeleton.zip) tai [Lataa Esimerkki valmiista](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/complete.zip).  Sinun on myös Azure AD-vuokraajan, jossa voit luoda käyttäjille ja sovelluksen rekisteröiminen.  Jos sinulla ei vielä ole vuokraajan, [Lue, miten voit hankkia sen](active-directory-howto-tenant.md).

## <a name="1-register-the-directorysearcher-application"></a>*1. DirectorySearcher-sovelluksen rekisteröinti*
Sovelluksen hankkiminen tunnusten käyttöön ensin tarvitset rekisteröidään Azure AD-vuokraajan ja Myönnä käyttöoikeudet Azure AD-kaavio-Ohjelmointirajapinnan:

-   Kirjaudu sisään Azure hallinta-portaalissa
-   Valitse vasemmalla olevasta siirtymisruudussa **Active Directory**
-   Valitse vuokraajan, jolla sovellus rekisteröi.
-   Valitse **sovellukset** -välilehti ja valitse **Lisää** ala-laatikon.
-   Seuraa ohjeita ja luo uusi **Alkuperäisen asiakassovellukseen**.
    -   Sovelluksen **nimen** kuvataan sovelluksesi loppukäyttäjille
    -   **Uudelleenohjata Uri** on värimallin ja merkkijonon yhdistelmä, jota Azure AD käyttää suojaustunnuksen vastaukset palautetaan.  Anna tietyn arvon sovelluksesi, kuten `http://DirectorySearcher`.
-   Kun olet suorittanut rekisteröinti, AAD Määritä sovelluksesi asiakkaan yksilöllinen tunnus.  Tarvitset tätä arvoa seuraavat osiot niin kopioi se **määritys** -välilehti.
- Myös **määrittäminen** -välilehden Etsi "Oikeudet ja muut sovellukset"-kohta.  "Azure Active Directory"-sovelluksen Lisää **Valtuutetun käyttöoikeudet**-kohdassa **Access Your organisaation hakemisto** -käyttöoikeus.  Tämä ottaa käyttöön sovelluksen käyttäjille Graph-Ohjelmointirajapinnan kysely.

## <a name="2-install--configure-adal"></a>*2. Asenna ja määritä ADAL*
Nyt kun olet luonut Azure AD-sovelluksen, voit asentaa ADAL ja kirjoita käyttäjätietojen liittyvä koodi.  Myös ADAL voivat pitää yhteyttä Azure AD-pystyttävä toimittamaan sovelluksen rekisteröinti tietoja.
-   Aloita lisäämällä ADAL DirectorySearcher projektin pakettien hallinta-konsolin avulla.

```
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
```

-   DirectorySearcher Projectissa, Avaa `app.config`.  Elementtien arvojen korvaaminen `<appSettings>` osan kuvaavat arvoja, jotka olet kirjoittanut Azure-portaaliin.  Nämä arvot viittaavat koodisi aina, kun se käyttää ADAL.
    -   `ida:Tenant` Azure AD-vuokraajan, esimerkiksi contoso.onmicrosoft.com toimialue
    -   `ida:ClientId` On portaalista kopioimasi sovelluksen clientId.
    -   `ida:RedirectUri` Uudelleenohjausta on rekisteröity-portaalin URL-osoite.

## <a name="3--use-adal-to-get-tokens-from-aad"></a>*3. Käytä ADAL tunnusten käyttämistä AAD*
Perustietoja tarpeen ADAL takana on, että aina, kun sovellus on access-tunnuksen, riittää, että kutsuu `authContext.AcquireTokenAsync(...)`, eikä ADAL muille käyttäjille.  

-   Valitse `DirectorySearcher` projektin, Avaa `MainWindow.xaml.cs` ja Etsi `MainWindow()` menetelmää.  Ensimmäiseksi on valmistella sinua sovelluksen `AuthenticationContext` -ADAL on ensisijainen luokka.  Tämä on missä ADAL välittää täytyy Azure AD yhteydenpito ja kuinka se välimuistiin tunnusten koordinaatit.

```C#
public MainWindow()
{
    InitializeComponent();

    authContext = new AuthenticationContext(authority, new FileCache());

    CheckForCachedToken();
}
```

- Selaa `Search(...)` menetelmä, joka käynnistyy, kun käyttäjä cliks "haku-painiketta sovelluksen käyttöliittymässä.  Tämä menetelmä tekee GET-pyynnössä kyselyyn Azure AD-kaavio-Ohjelmointirajapinnan käyttäjille, joiden UPN alkaa tietyllä hakusanoja.  Mutta kyselyn Graph-Ohjelmointirajapinta, jotta sinun täytyy lisätä access_token- `Authorization` otsikon tämä on pyynnön - kohtaa, johon ADAL on.

```C#
private async void Search(object sender, RoutedEventArgs e)
{
    // Validate the Input String
    if (string.IsNullOrEmpty(SearchText.Text))
    {
        MessageBox.Show("Please enter a value for the To Do item name");
        return;
    }

    // Get an Access Token for the Graph API
    AuthenticationResult result = null;
    try
    {
        result = await authContext.AcquireTokenAsync(graphResourceId, clientId, redirectUri, new PlatformParameters(PromptBehavior.Auto));
        UserNameLabel.Content = result.UserInfo.DisplayableId;
        SignOutButton.Visibility = Visibility.Visible;
    }
    catch (AdalException ex)
    {
        // An unexpected error occurred, or user canceled the sign in.
        if (ex.ErrorCode != "access_denied")
            MessageBox.Show(ex.Message);

        return;
    }

    ...
}
```
- Kun sovellus pyytää tunnusta soittamalla `AcquireTokenAsync(...)`, ADAL yrittää palauttaa tunnusta kysymättä käyttäjän tunnistetiedot.  Jos ADAL määrittää, että käyttäjän täytyy kirjautua saat tunnusta, sisäänkirjautuminen-valintaikkunan näkyviin, kerätä käyttäjän tunnistetiedot ja palauttaa tunnuksen yhteydessä todentaminen onnistuu.  Jos ADAL ei voi palauttaa tunnusta jostakin syystä, se palauttaa `AdalException`.
- Huomaa, `AuthenticationResult` objekti sisältää `UserInfo` objektia, jonka avulla voit kerätä tietoja on ehkä sovellus.  Valitse DirectorySearcher `UserInfo` avulla voit mukauttaa sovelluksen-Käyttöliittymä, jossa on käyttäjän tunnus.

- Kun käyttäjä napsauttaa "Kirjaudu ulos"-painiketta, haluamme varmistaa, että seuraava puhelu `AcquireTokenAsync(...)` kysyy, käyttäjä voi kirjautua sisään.  ADAL tämä on yhtä helppoa kuin suojaustunnuksen välimuistin tyhjentäminen:

```C#
private void SignOut(object sender = null, RoutedEventArgs args = null)
{
    // Clear the token cache
    authContext.TokenCache.Clear();

    ...
}
```

- Kuitenkin käyttäjän Valitse "Kirjaudu ulos"-painiketta, jos haluat säilyttää käyttäjän istunnon ne suoritetaan DirectorySearcher seuraavan kerran.  Kun sovellus käynnistyy, voit tarkistaa ADAL päivän vahvistustunnuksen välimuistin aiemmin tunnuksen ja Päivitä Käyttöliittymän vastaavasti.  - `CheckForCachedToken()` -Menetelmä Varmista kutsu `AcquireTokenAsync(...)`, tällä hetkellä kulkevat `PromptBehavior.Never` parametrin.  `PromptBehavior.Never`kertoo ADAL, että käyttäjä ei ole saatetaan pyytää kirjauduttaessa ja ADAL sen sijaan olisi palauttaa poikkeuksen, jos se ei voi palauttaa tunnusta.

```C#
public async void CheckForCachedToken() 
{
    // As the application starts, try to get an access token without prompting the user.  If one exists, show the user as signed in.
    AuthenticationResult result = null;
    try
    {
        result = await authContext.AcquireTokenAsync(graphResourceId, clientId, redirectUri, new PlatformParameters(PromptBehavior.Never));
    }
    catch (AdalException ex)
    {
        if (ex.ErrorCode != "user_interaction_required")
        {
            // An unexpected error occurred.
            MessageBox.Show(ex.Message);
        }

        // If user interaction is required, proceed to main page without singing the user in.
        return;
    }

    // A valid token is in the cache
    SignOutButton.Visibility = Visibility.Visible;
    UserNameLabel.Content = result.UserInfo.DisplayableId;
}
```

Onnittelen! Sinun on nyt toimiva .NET WPF sovellus, jossa on voi todentaa käyttäjät, Soita suojatusti verkko-ohjelmointirajapinnan käyttäminen OAuth 2.0, ja poistaa käyttäjän perustietoja.  Jos et ole jo, nyt on aika, joka täyttää alihallintaan joidenkin käyttäjien kanssa.  Suorita DirectorySearcher-sovellus ja kirjaudu sisään jokin kyseisille käyttäjille.  Etsi muiden käyttäjien niiden UPN perusteella.  Sulje sovellus ja käyttää sitä.  Huomaa, miten käyttäjän istunnon säilyy muuttumattomana.  Kirjaudu ulos ja kirjaudu takaisin sisään toiselle käyttäjälle.

ADAL voi liittää kaikkia näitä yleisiä identity-ominaisuuksia sovelluksesi on helppoa.  Muokattu-työstä kestää huolellisesti, - välimuistin hallinnan, OAuth-protokollatuki esitykset käyttäjän kirjautuminen Käyttöliittymä, päivittäminen ja vanhentuneet tunnusten.  Kaikki on varmasti tiedettävä on yksittäinen API-kutsu `authContext.AcquireTokenAsync(...)`.

Viittaus (ilman määritysarvot) Esimerkki valmiista toimitetaan [Tässä](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/complete.zip).  Voit nyt siirtää Lisää skenaarioita.  Voit halutessasi kokeilla:

[Suojattu .NET verkko-Ohjelmointirajapinnan kanssa Azure AD >>](active-directory-devquickstarts-webapi-dotnet.md)

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
