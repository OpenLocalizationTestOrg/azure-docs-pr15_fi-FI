<properties
    pageTitle="Azure Active Directory-B2C | Microsoft Azure"
    description="Muodostaminen Windows työpöytäsovellus, joka sisältää-kirjautuminen-kirjautuminen, ja käyttämällä Azure Active Directory-B2C profiilin hallinta."
    services="active-directory-b2c"
    documentationCenter=".net"
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="07/22/2016"
    ms.author="dastrock"/>

# <a name="azure-ad-b2c-build-a-windows-desktop-app"></a>Azure AD-B2C: Muodosta Windows-työpöytäsovellus

Azure Active Directory (Azure AD) B2C avulla voit lisätä tehokkaita Omatoiminen tunnistetietojen hallintaominaisuuksien lisääminen ‑Työpöytäsovelluksen lyhyt muutama vaihe. Tässä artikkelissa kerrotaan, miten, joka sisältää käyttäjän kirjautuminen, kirjaudu sisään ja Profiilin hallinta .NET Windows Presentation Foundation (WPF) "tehtäväluettelo-sovelluksen luominen. Sovelluksen sisällytetään tuki kirjautumista ja kirjaudu sisään käyttämällä käyttäjänimi tai sähköpostiosoite. Yhteisöpalvelujen, kuten Facebook- ja Google käyttämällä myös sisällytettävä tuki kirjautumista ja kirjaudu sisään.

## <a name="get-an-azure-ad-b2c-directory"></a>Hae Azure AD B2C-kansio

Ennen kuin voit käyttää Azure AD B2C, voit luoda hakemiston tai vuokraaja.  Kansio on kaikkien käyttäjien, sovellukset, ryhmät ja lisää säilö. Jos sitä ei ole vielä ole, ennen kuin voit [luoda hakemiston B2C](active-directory-b2c-get-started.md) edelleen tässä oppaassa.

## <a name="create-an-application"></a>Sovelluksen luominen

Seuraavaksi haluat luoda sovelluksen B2C hakemistossa. Näin pitää yhteyttä suojatusti sovelluksen tarvitsemiaan Azure AD-tietoja. Voit luoda sovelluksen noudattamalla [näitä ohjeita](active-directory-b2c-app-registration.md).  Muista:

- Sisällytä **alkuperäisen asiakkaan** sovellus.
- Kopioi **uudelleenohjata URI** `urn:ietf:wg:oauth:2.0:oob`. Tässä esimerkissä koodi oletusarvoisen URL-osoite on.
- Kopioi **Tunnus** , jolla määritetään sovelluksen. Tarvitset sen myöhemmin.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Luoda oman käytäntöjä

Azure AD B2C jokaisen käyttäjäkokemus on määritetty [käytännön](active-directory-b2c-reference-policies.md)avulla. Koodi-malli sisältää kolme tunnistetietojen kokemukset: Rekisteröidy, kirjaudu sisään ja Muokkaa profiilia. Tarvitset mistäkin-käytännön luominen [käytännön viitata artikkelissa](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy)kuvatulla tavalla. Kun olet luonut kolme käytäntöjä, muista:

- Valitse **Käyttäjätunnus kirjautuminen** tai **sähköpostin kirjautuminen** tunnistetietojen toimittajat-sivu.
- Valitse kirjautumisen käytännön **Näyttönimi** ja ilmoittautuminen määritteet.
- Valitse sovelluksen vaateet jokaisen käytännön **Näyttönimi** ja **Objektitunnus** vaateet. Voit valita myös muut vaateet.
- Kopioi kunkin käytännön **nimen** , sen luomisen jälkeen. Tunnisteen pitäisi olla etuliite `b2c_1_`.  Tarvitset seuraavat käytännön nimet myöhemmin.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Kun olet luonut kolme käytäntöjä, voit ryhtyä sovelluksen luominen.

## <a name="download-the-code"></a>Lataa koodi

Tämän opetusohjelman [määritetään GitHub](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet)koodi. Voit luoda otosten kuitenkin, [Lataa rakenne projektin .zip-tiedostona](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet/archive/skeleton.zip). Voit myös kopioida rakenne:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet.git
```

Valmis-sovellus on myös [saatavilla .zip-tiedostona](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet/archive/complete.zip) tai `complete` saman säilö haaraa.

Kun olet ladannut sample code, Avaa Visual Studio .sln tiedosto avulla pääset alkuun. `TaskClient` Projekti on WPF työpöytäsovellus, jotka käyttäjä käsittelee. Tässä opetusohjelmassa tarkoitetaan kutsuu taustatietokantaan tehtävän verkko-Ohjelmointirajapinnan, ylläpidettävä Azure, joka tallentaa kunkin käyttäjän Tehtävät-luettelo.  Sinun ei tarvitse luoda verkko-Ohjelmointirajapinnan, on jo se on käynnissä.

Lue, miten verkko-Ohjelmointirajapinnan todentaa pyynnöt suojatusti Azure AD B2C avulla, tutustu [Verkko-Ohjelmointirajapinnan aloittaminen artikkelissa](active-directory-b2c-devquickstarts-api-dotnet.md).

## <a name="execute-policies"></a>Suorita käytännöt
Sovelluksen yhteydessä Azure AD B2C todennus-viestejä, joka määrittää käytännön, he haluavat osana HTTP-pyynnön suorittaminen. .NET-työpöytäsovelluksiin OAuth 2.0-todennus lähettämiseen, suorittaa käytännöt esikatselun Microsoft todennus kirjaston (MSAL) avulla ja hakea tunnuksia, verkko-ohjelmointirajapinnan kutsu.

### <a name="install-msal"></a>Asenna MSAL
MSAL voit lisätä `TaskClient` projektin Visual Studio paketin hallinta-konsolin avulla.

```
PM> Install-Package Microsoft.Identity.Client -IncludePrerelease
```

### <a name="enter-your-b2c-details"></a>Kirjoita B2C tiedot
Avaa tiedosto `Globals.cs` ja korvaa kunkin ominaisuuden arvot omalla. Tähän luokkaan käytetään koko `TaskClient` käytettyjä viittaus arvoihin.

```C#
public static class Globals
{
    ...

    // TODO: Replace these five default with your own configuration values
    public static string tenant = "fabrikamb2c.onmicrosoft.com";
    public static string clientId = "90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6";
    public static string signInPolicy = "b2c_1_sign_in";
    public static string signUpPolicy = "b2c_1_sign_up";
    public static string editProfilePolicy = "b2c_1_edit_profile";

    ...
}
```

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]


### <a name="create-the-publicclientapplication"></a>Luo PublicClientApplication
MSAL ensisijainen luokka on `PublicClientApplication`. Tämä luokka kuvaa sovelluksesi Azure AD B2C-järjestelmässä. Kun sovellus, initalizes luoda esiintymää `PublicClientApplication` - `MainWindow.xaml.cs`. Tämä voidaan koko ikkunan.

```C#
protected async override void OnInitialized(EventArgs e)
{
    base.OnInitialized(e);

    pca = new PublicClientApplication(Globals.clientId)
    {
        // MSAL implements an in-memory cache by default.  Since we want tokens to persist when the user closes the app, 
        // we've extended the MSAL TokenCache and created a simple FileCache in this app.
        UserTokenCache = new FileCache(),
    };
    
    ...
```

### <a name="initiate-a-sign-up-flow"></a>Aloita rekisteröitymistoiminto työnkulku
Kun käyttäjä valitsee etumerkki ylös, haluat aloittaa kirjautumisen työnkulun, joka käyttää luomasi kirjautumisen käytännön. Käyttämällä MSAL vain soitat `pca.AcquireTokenAsync(...)`. Voit siirtää parametrit `AcquireTokenAsync(...)` selvittää, mitkä tunnuksen näyttöön, ja käyttöoikeuksien tarkistuspyynnön käytännön.

```C#
private async void SignUp(object sender, RoutedEventArgs e)
{
    AuthenticationResult result = null;
    try
    {
        // Use the app's clientId here as the scope parameter, indicating that
        // you want a token to the your app's backend web API (represented by
        // the cloud hosted task API).  Use the UiOptions.ForceLogin flag to
        // indicate to MSAL that it should show a sign-up UI no matter what.
        result = await pca.AcquireTokenAsync(new string[] { Globals.clientId },
                string.Empty, UiOptions.ForceLogin, null, null, Globals.authority,
                Globals.signUpPolicy);

        // Upon success, indicate in the app that the user is signed in.
        SignInButton.Visibility = Visibility.Collapsed;
        SignUpButton.Visibility = Visibility.Collapsed;
        EditProfileButton.Visibility = Visibility.Visible;
        SignOutButton.Visibility = Visibility.Visible;

        // When the request completes successfully, you can get user 
        // information from the AuthenticationResult
        UsernameLabel.Content = result.User.Name;

        // After the sign up successfully completes, display the user's To-Do List
        GetTodoList();
    }

    // Handle any exeptions that occurred during execution of the policy.
    catch (MsalException ex)
    {
        if (ex.ErrorCode != "authentication_canceled")
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

### <a name="initiate-a-sign-in-flow"></a>Aloita työnkulku-kirjautuminen
Voit aloittaa kirjautumisen vuo aloittaa ilmoittautuminen kulun samalla tavalla. Käyttäjän kirjautuessa sisään Tee sama kutsu MSAL, tällä hetkellä kirjautumisen käytännön avulla:

```C#
private async void SignIn(object sender = null, RoutedEventArgs args = null)
{
    AuthenticationResult result = null;
    try
    {
        result = await pca.AcquireTokenAsync(new string[] { Globals.clientId },
                    string.Empty, UiOptions.ForceLogin, null, null, Globals.authority,
                    Globals.signInPolicy);
        ...
```

### <a name="initiate-an-edit-profile-flow"></a>Muokkaa profiilia-työnkulun aloittaminen
Uudelleen voit suorittaa Muokkaa profiilia-käytäntö samalla tavalla:

```C#
private async void EditProfile(object sender, RoutedEventArgs e)
{
    AuthenticationResult result = null;
    try
    {
        result = await pca.AcquireTokenAsync(new string[] { Globals.clientId },
                    string.Empty, UiOptions.ForceLogin, null, null, Globals.authority,
                    Globals.editProfilePolicy);
```

Kaikissa tapauksissa MSAL palauttaa joko-tunnuksen `AuthenticationResult` tai ilmoittaa poikkeuksen. Aina, kun sinulla on tunnusta MSAL, voit käyttää `AuthenticationResult.User` objektin päivittämään käyttäjätietoja-sovelluksessa, kuten Käyttöliittymän. ADAL välimuistiin myös tunnuksen sovelluksen muiden osien käytettäviksi.


### <a name="check-for-tokens-on-app-start"></a>Tarkista tunnusten app käynnistettäessä
Voit käyttää myös MSAL seurantaan käyttäjän kirjautuminen tila.  Tämän sovelluksen haluamme käyttäjä voi edelleen kirjautuneena, vaikka ne Sulje sovellus ja avaa se uudelleen.  Takaisin sisään `OnInitialized` ohittaa, käytä MSAL's `AcquireTokenSilent` Tarkista menetelmä välimuistiin tunnusten:

```C#
AuthenticationResult result = null;
try
{
    // If the user has has a token cached with any policy, we'll display them as signed-in.
    TokenCacheItem tci = pca.UserTokenCache.ReadItems(Globals.clientId).Where(i => i.Scope.Contains(Globals.clientId) && !string.IsNullOrEmpty(i.Policy)).FirstOrDefault();
    string existingPolicy = tci == null ? null : tci.Policy;
    result = await pca.AcquireTokenSilentAsync(new string[] { Globals.clientId }, string.Empty, Globals.authority, existingPolicy, false);

    SignInButton.Visibility = Visibility.Collapsed;
    SignUpButton.Visibility = Visibility.Collapsed;
    EditProfileButton.Visibility = Visibility.Visible;
    SignOutButton.Visibility = Visibility.Visible;
    UsernameLabel.Content = result.User.Name;
    GetTodoList();
}
catch (MsalException ex)
{
    if (ex.ErrorCode == "failed_to_acquire_token_silently")
    {
        // There are no tokens in the cache.  Proceed without calling the To Do list service.
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
```

## <a name="call-the-task-api"></a>Soita tehtävän Ohjelmointirajapinta
Olet käyttänyt MSAL nyt suorittaa käytännöt ja tunnusten.  Kun haluat käyttää näiden tunnusten Soita tehtävän API, voit käyttää uudelleen MSAL's `AcquireTokenSilent` Tarkista menetelmä välimuistiin tunnusten:

```C#
private async void GetTodoList()
{
    AuthenticationResult result = null;
    try
    {
        // Here we want to check for a cached token, independent of whatever policy was used to acquire it.
        TokenCacheItem tci = pca.UserTokenCache.ReadItems(Globals.clientId).Where(i => i.Scope.Contains(Globals.clientId) && !string.IsNullOrEmpty(i.Policy)).FirstOrDefault();
        string existingPolicy = tci == null ? null : tci.Policy;

        // Use AcquireTokenSilent to indicate that MSAL should throw an exception if a token cannot be acquired
        result = await pca.AcquireTokenSilentAsync(new string[] { Globals.clientId }, string.Empty, Globals.authority, existingPolicy, false);

    }
    // If a token could not be acquired silently, we'll catch the exception and show the user a message.
    catch (MsalException ex)
    {
        // There is no access token in the cache, so prompt the user to sign-in.
        if (ex.ErrorCode == "failed_to_acquire_token_silently")
        {
            MessageBox.Show("Please sign up or sign in first");
            SignInButton.Visibility = Visibility.Visible;
            SignUpButton.Visibility = Visibility.Visible;
            EditProfileButton.Visibility = Visibility.Collapsed;
            SignOutButton.Visibility = Visibility.Collapsed;
            UsernameLabel.Content = string.Empty;
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
    ...
```

Kun puhelu `AcquireTokenSilentAsync(...)` onnistuu ja tunnusta löytyy välimuistin, voit lisätä tunnuksen `Authorization` HTTP-pyynnön otsikon. Tehtävän verkko-Ohjelmointirajapinnan käyttämällä tämän otsikon todentaa pyynnön lukea käyttäjän Tehtävät-luettelo:

```C#
    ...
    // Once the token has been returned by MSAL, add it to the http authorization header, before making the call to access the To Do list service.
    httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.Token);

    // Call the To Do list service.
    HttpResponseMessage response = await httpClient.GetAsync(Globals.taskServiceUrl + "/api/tasks");
    ...
```

## <a name="sign-the-user-out"></a>Käyttäjän uloskirjautuminen
Lopuksi voit lopettaa käyttäjän istunnon sovelluksen kanssa, kun käyttäjä valitsee **Uloskirjautuminen**MSAL avulla.  Kun käytät MSAL, tämä tehdään poistamalla kaikki suojaustunnuksen välimuistista tunnusten:

```C#
private void SignOut(object sender, RoutedEventArgs e)
{
    // Clear any remnants of the user's session.
    pca.UserTokenCache.Clear(Globals.clientId);

    // This is a helper method that clears browser cookies in the browser control that MSAL uses, it is not part of MSAL.
    ClearCookies();

    // Update the UI to show the user as signed out.
    TaskList.ItemsSource = string.Empty;
    SignInButton.Visibility = Visibility.Visible;
    SignUpButton.Visibility = Visibility.Visible;
    EditProfileButton.Visibility = Visibility.Collapsed;
    SignOutButton.Visibility = Visibility.Collapsed;
    return;
}
```

## <a name="run-the-sample-app"></a>Suorita malli-sovellus

Lopuksi luominen ja suorittaminen otosten.  Tilaa sovelluksen käyttämällä sähköposti- tai käyttäjän nimi. Kirjaudu ulos ja kirjaudu takaisin sisään sama käyttäjä. Muokkaa kyseisen käyttäjän profiiliin. Kirjaudu ulos ja Kirjaudu toiselle käyttäjälle.

## <a name="add-social-idps"></a>Lisää sosiaalisen IDPs

Sovellus tukee tällä hetkellä vain käyttäjän kirjautuminen ja kirjautuminen, jotka käyttävät **paikalliset tilit**. Nämä ovat B2C hakemistossa tallennetut tilit, jotka käyttävät käyttäjänimi ja salasana. Azure AD B2C avulla voit lisätä muita tunnistetietojen toimittajat (IDPs) tuki muuttamatta koodisi.

Voit lisätä sovelluksen sosiaalisen IDPs, Aloita näistä artikkeleista yksityiskohtaisia ohjeita noudattamalla. Kunkin IDP haluat tukea sinun on sovellus rekisteröidään järjestelmän ja hanki on asiakas.

- [Facebook IDP määrittäminen](active-directory-b2c-setup-fb-app.md)
- [Määritetty Google IDP](active-directory-b2c-setup-goog-app.md)
- [Amazon IDP määrittäminen](active-directory-b2c-setup-amzn-app.md)
- [LinkedIn IDP määrittäminen](active-directory-b2c-setup-li-app.md)

Kun olet lisännyt tunnistetietojen toimittajat B2C-kansioon, sinun on muokattava kunkin oman kolme käytännöt sisältämään uuden IDPs [käytännön viitata artikkelissa](active-directory-b2c-reference-policies.md)kuvatulla tavalla. Kun olet tallentanut oman käytäntöjä, suorita sovellus uudelleen. Näkyviin tulee uusi IDPs lisätään Kirjaudu sisään ja kirjautumisvaihtoehdot kussakin henkilöllisyytesi kohtaa.

Voit kokeilla oman käytännöt ja noudata tehosteita malli-sovellukseen. Lisää tai poista IDPs, käsitellä sovelluksen vaateet tai vaihtaa rekisteröitymistoiminto määritteet. Kokeile, kunnes näet, miten käytäntöjä, käyttöoikeuksien ja MSAL sitominen yhdessä.

Viitteen, valmis malli [on annettu .zip-tiedostona](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet/archive/complete.zip). Voit myös Kloonaa se GitHub kohteesta:

```git clone --branch complete https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet.git```
