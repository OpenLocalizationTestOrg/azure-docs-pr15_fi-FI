<properties
pageTitle="Azure Active Directory v2.0 .NET alkuperäisen sovelluksen | Microsoft Azure"
description=".NET alkuperäisen sovellus, joka kirjautuu-käyttäjät sekä oman Microsoft Account muodostaminen ja työpaikan tai oppilaitoksen tilit."
services="active-directory"
documentationCenter=""
authors="dstrockis"
manager="mbaldwin"
editor=""/>

<tags
ms.service="active-directory"
ms.workload="identity"
ms.tgt_pltfrm="na"
ms.devlang="dotnet"
ms.topic="article"
ms.date="07/30/2016"
ms.author="dastrock; vittorib"/>

# <a name="add-sign-in-to-a-windows-desktop-app"></a>Kirjaudu sisään Windows-työpöytä-sovelluksen lisääminen

Kanssa v2.0 päätepisteen, voit nopeasti lisätä Työpöytäsovelluksen tuki kummankin henkilökohtainen Microsoft-tilin todennus ja työpaikan tai oppilaitoksen tilit.  Sen avulla myös pitää yhteyttä suojatusti Taustajärjestelmä web api sekä [Microsoft Graph](https://graph.microsoft.io) sovelluksen ja [Office 365: n yhdistetyn API](https://www.msdn.com/office/office365/howto/authenticate-Office-365-APIs-using-v2)suoritettavien.

> [AZURE.NOTE] V2.0 päätepisteen tukee kaikkia Azure Active Directory (AD) skenaarioita ja toiminnot.  Voit selvittää, jos sinun on käytettävä v2.0 päätepisteen, lue lisätietoja [v2.0 rajoitukset](active-directory-v2-limitations.md).

[.NET alkuperäisen sovellukset, jotka suoritetaan laitteessa](active-directory-v2-flows.md#mobile-and-native-apps)Azure AD tarjoaa Microsoft tunnistetietojen todennus kirjaston tai MSAL.  MSAL on aika-ainoa tarkoitus on helpompaa, kun sovellus kutsumista verkkopalvelut tunnusten hankkiminen.  Osoittaa, kuinka helppoa, tässä on kehittää .NET WPF tehtäväluettelo sovelluksen, joka:

- Käyttäjä kirjautuu & saa käyttää tunnusten [OAuth 2.0 todennus-protokollan](active-directory-v2-protocols.md#oauth2-authorization-code-flow)avulla.
- Puheluiden taustassa Tehtävät-luettelon WWW-palvelun, joka on myös suojattu OAuth 2.0 suojatusti.
- Käyttäjän uloskirjautuminen.

## <a name="download-sample-code"></a>Ladata malli

Tässä opetusohjelmassa koodi määritetään [GitHub](https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet).  Jos haluat seurata, voit [ladata sovelluksen rakenne .zip nimellä](https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet/archive/skeleton.zip) tai Kloonaa rakenne:

    git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet.git

Valmiin sovelluksen annetaan jäljempänä tässä opetusohjelmassa.

## <a name="register-an-app"></a>Sovelluksen rekisteröiminen
Luo uuden sovelluksen [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)tai noudattamalla seuraavia [toimimalla seuraavasti](active-directory-v2-app-registration.md).  Varmista, että:

- Kopioi alaspäin **Sovellustunnus** sovelluksen myönnetyt, sinun on se pian.
- Lisää, kun sovellus **Mobile** -ympäristössä.

## <a name="install--configure-msal"></a>Asenna ja määritä MSAL
Nyt kun olet luonut sovellus, joka on rekisteröity Microsoft-voit asentaa MSAL ja kirjoita käyttäjätietojen liittyvät koodi.  Jotta MSAL voi olla yhteydessä v2.0 päätepiste sinun on toimittamaan sovelluksen rekisteröinti tietoja.

-   Aloita lisäämällä MSAL TodoListClient projektin pakettien hallinta-konsolin avulla.

```
PM> Install-Package Microsoft.Identity.Client -ProjectName TodoListClient -IncludePrerelease
```

-   TodoListClient Projectissa, Avaa `app.config`.  Elementtien arvojen korvaaminen `<appSettings>` osan kuvaavat arvoja, jotka olet kirjoittanut sovelluksen rekisteröinti-portaaliin.  Nämä arvot viittaavat koodisi aina, kun se käyttää MSAL.
    -   `ida:ClientId` On portaalista kopioimasi sovelluksen **Tunnus** .

- TodoList-palvelu, project Avaa `web.config` projektin pääsivustoon.  
    - Korvaa `ida:Audience` arvon saman **Sovellustunnus** -portaalista.

## <a name="use-msal-to-get-tokens"></a>Tunnusten MSAL avulla
Tavallinen tarpeen MSAL takana on, että aina, kun sovellus on access-tunnuksen, riittää, että soitat `app.AcquireToken(...)`, eikä MSAL muille käyttäjille.  

-   Valitse `TodoListClient` projektin, Avaa `MainWindow.xaml.cs` ja Etsi `OnInitialized(...)` menetelmää.  Ensimmäiseksi on valmistella sinua sovelluksen `PublicClientApplication` -MSAL käyttäjän ensisijaisen luokan edustava alkuperäisen sovellukset.  Tämä on kohtaa, johon siirrät MSAL täytyy Azure AD yhteydenpito ja kuinka se välimuistiin tunnusten koordinaatit.

```C#
protected override async void OnInitialized(EventArgs e)
{
        base.OnInitialized(e);

        app = new PublicClientApplication(new FileCache());
        AuthenticationResult result = null;
        ...
}
```

- Kun sovellus käynnistyy, haluamme Tarkista ja katso, jos käyttäjä on jo kirjautunut sovellukseen.  Mutta et halua käynnistää kirjautumisen Käyttöliittymän vain vielä - olemme tehdä "Kirjaudu sisään" Kiellä käyttäjän.  Myös tässä `OnInitialized(...)` menetelmää:

```C#
// As the app starts, we want to check to see if the user is already signed in.
// You can do so by trying to get a token from MSAL, using the method
// AcquireTokenSilent.  This forces MSAL to throw an exception if it cannot
// get a token for the user without showing a UI.
try
{
    result = await app.AcquireTokenSilentAsync(new string[] { clientId });
    // If we got here, a valid token is in the cache - or MSAL was able to get a new oen via refresh token.
    // Proceed to fetch the user's tasks from the TodoListService via the GetTodoList() method.
    
    SignInButton.Content = "Clear Cache";
    GetTodoList();
}
catch (MsalException ex)
{
    if (ex.ErrorCode == "user_interaction_required")
    {
        // If user interaction is required, the app should take no action,
        // and simply show the user the sign in button.
    }
    else
    {
        // Here, we catch all other MsalExceptions
        string message = ex.Message;
        if (ex.InnerException != null)
        {
            message += "Inner Exception : " + ex.InnerException.Message;
        }
        MessageBox.Show(message);
    }
}

```

- Jos käyttäjä ei ole kirjautunut sisään, ja ne sitten "Kirjaudu sisään"-painike, haluamme kutsua kirjautuminen Käyttöliittymä ja pyydä käyttäjää syöttää tunnistetietoja.  Kirjaudu sisään-painike käsittelijä Toteuta:

```C#
private async void SignIn(object sender = null, RoutedEventArgs args = null)
{
        // TODO: Sign the user out if they clicked the "Clear Cache" button

// If the user clicked the 'Sign-In' button, force
// MSAL to prompt the user for credentials by using
// AcquireTokenAsync, a method that is guaranteed to show a prompt to the user.
// MSAL will get a token for the TodoListService and cache it for you.

AuthenticationResult result = null;
try
{
    result = await app.AcquireTokenAsync(new string[] { clientId });
    SignInButton.Content = "Clear Cache";
    GetTodoList();
}
catch (MsalException ex)
{
    // If MSAL cannot get a token, it will throw an exception.
    // If the user canceled the login, it will result in the
    // error code 'authentication_canceled'.

    if (ex.ErrorCode == "authentication_canceled")
    {
        MessageBox.Show("Sign in was canceled by the user");
    }
    else
    {
        // An unexpected error occurred.
        string message = ex.Message;
        if (ex.InnerException != null)
        {
            message += "Inner Exception : " + ex.InnerException.Message;
        }

        MessageBox.Show(message);
    }

    return;
}


}
```

- Jos käyttäjä onnistuneesti etumerkki-MSAL saat ja välimuistiin tunnusta puolestasi ja voit jatkaa Soita `GetTodoList()` menetelmä ongelmitta.  Kaikki haluamasi jätetään saat käyttäjän tehtävät on pantava täytäntöön `GetTodoList()` menetelmää.

```C#
private async void GetTodoList()
{

AuthenticationResult result = null;
try
{
    // Here, we try to get an access token to call the TodoListService
    // without invoking any UI prompt.  AcquireTokenSilentAsync forces
    // MSAL to throw an exception if it cannot get a token silently.


    result = await app.AcquireTokenSilentAsync(new string[] { clientId });
}
catch (MsalException ex)
{
    // MSAL couldn't get a token silently, so show the user a message
    // and let them click the Sign-In button.

    if (ex.ErrorCode == "user_interaction_required")
    {
        MessageBox.Show("Please sign in first");
        SignInButton.Content = "Sign In";
    }
    else
    {
        // In any other case, an unexpected error occurred.

        string message = ex.Message;
        if (ex.InnerException != null)
        {
            message += "Inner Exception : " + ex.InnerException.Message;
        }
        MessageBox.Show(message);
    }

    return;
}

// Once the token has been returned by MSAL,
// add it to the http authorization header,
// before making the call to access the To Do list service.

httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.Token);


        ...
...


- When the user is done managing their To-Do List, they may finally sign out of the app by clicking the "Clear Cache" button.

```C#
private async void SignIn(object sender = null, RoutedEventArgs args = null)
{
        // If the user clicked the 'clear cache' button,
        // clear the MSAL token cache and show the user as signed out.
        // It's also necessary to clear the cookies from the browser
        // control so the next user has a chance to sign in.

        if (SignInButton.Content.ToString() == "Clear Cache")
        {
                TodoList.ItemsSource = string.Empty;
                app.UserTokenCache.Clear(app.ClientId);
                ClearCookies();
                SignInButton.Content = "Sign In";
                return;
        }

        ...
```

## <a name="run"></a>Suorita

Onnittelen! Työn .NET WPF-sovellus, jossa on voi todentaa käyttäjät ja soita suojatusti verkko-ohjelmointirajapinnan käyttäminen OAuth 2.0 on nyt.  Suorita sekä projektien ja kirjaudu sisään henkilökohtainen Microsoft-tili tai työpaikan tai oppilaitoksen tiliä.  Tehtävien lisääminen kyseisen käyttäjän Tehtävät-luettelo.  Kirjaudu ulos ja kirjaudu takaisin sisään toisena käyttäjänä voit tarkastella niiden tehtäväluettelo.  Sulje sovellus ja käyttää sitä.  Ilmoitus siitä, miten käyttäjän istunto pysyy muuttumattomana – Tämä johtuu sovelluksen välimuistiin paikallisen tiedoston tunnukset.

MSAL on helppo voi liittää-sovelluksen yleisiä käyttäjätietojen ominaisuuksia sekä henkilökohtainen ja työpaikan tilin käyttäminen.  Muokattu-työstä kestää huolellisesti, - välimuistin hallinnan, OAuth-protokollatuki esitykset käyttäjän kirjautuminen Käyttöliittymä, päivittäminen ja vanhentuneet tunnusten.  Kaikki on varmasti tiedettävä on yksittäinen API-kutsu `app.AcquireTokenAsync(...)`.

Viitteen Esimerkki valmiista (ilman määritysarvot) [tähän .zip taulukkona](https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet/archive/complete.zip), tai voit voit käytetään sen GitHub:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet.git```

## <a name="next-steps"></a>Seuraavat vaiheet

Voit nyt siirtää aiheita sivulle.  Voit halutessasi kokeilla:

- [V2.0 päätepisteissä TodoListService verkko-Ohjelmointirajapinnan suojaaminen](active-directory-v2-devquickstarts-dotnet-api.md)

Muita tietolähteitä Kuittaa ulos:  

- [V2.0 Sovelluskehittäjän opas >>](active-directory-appmodel-v2-overview.md)
- [StackOverflow "msal" tunnisteen >>](http://stackoverflow.com/questions/tagged/msal)

## <a name="get-security-updates-for-our-products"></a>Päivitysten hakeminen tuotteitamme

On suositeltavaa, kun suojauksen tapaukset esiintyvät käymällä [Tämän sivun](https://technet.microsoft.com/security/dd252948) ja tilaamalla neuvoa suojausvaroitusten ilmoitusten saaminen.
