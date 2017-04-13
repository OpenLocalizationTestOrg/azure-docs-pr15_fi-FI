<properties
    pageTitle="Aloittaminen Azure AD-Windows Phone | Microsoft Azure"
    description="Windows Phone-sovellus, joka integroituu Azure AD kirjauduttaessa ja soittaa Azure AD muodostaminen suojattu ohjelmointirajapinnan käyttäminen OAuth."
    services="active-directory"
    documentationCenter="windows"
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="mobile-windows-phone"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="dastrock"/>



# <a name="integrate-azure-ad-with-a-windows-phone-app"></a>Integroi Azure AD Windows Phone-sovelluksessa

[AZURE.INCLUDE [active-directory-devquickstarts-switcher](../../includes/active-directory-devquickstarts-switcher.md)]

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Jos kehität Windows Phone 8.1-sovelluksen, Azure AD on yksinkertainen ja selkeä voi todentaa käyttäjille heidän Active Directory-tilin kanssa.  Ottaa käyttöön myös sovelluksen tarjoaman suojatusti minkä tahansa verkko-Ohjelmointirajapinnan Azure AD, kuten Office 365-ohjelmointirajapinnan tai Azure-Ohjelmointirajapinnan suojattu.

> [AZURE.NOTE] Koodin tässä esimerkissä käytetään ADAL v2.0.  Uusimman tekniikan voit kokeilla sijaan Microsoftin [Windows yleinen opetusohjelma käyttämällä ADAL 3.0](active-directory-devquickstarts-windowsstore.md).  Jos sovelluksen ovat varmasti luominen Windows Phone 8.1, tämä on oikeassa paikassa.  ADAL v2.0 silti täysin tuettu ja kehittää sovellusten agianst Windows Phone 8.1 suositeltu tapaa käyttää Azure AD.

.NET alkuperäisen asiakkaille, jotka on suojattu resursseihin Azure AD on Active Directory käyttöoikeuksien kirjaston tai ADAL.  ADAL on ainoa tarkoitus-aika on sovelluksen access tunnusten pääset helposti.  Osoittaa, kuinka helppoa, seuraavassa on Laadi "Directory Searcher-sovellus Windows Phone 8.1:

-   Saa käyttää tunnusten soitettavien Azure AD-kaavio-Ohjelmointirajapinnan [OAuth 2.0 todennus-protokollan](https://msdn.microsoft.com/library/azure/dn645545.aspx)avulla.
-   Etsii käyttäjät, joilla on tietyn UPN hakemistosta.
-   Etumerkki käyttäjät ulos.

Tarvitset luonnissa valmis toimimasta-sovelluksen avulla:

2. Voit rekisteröidä sovelluksesi Azure AD.
3. Asenna ja määritä ADAL.
5. Pyydä tunnusten Azure AD ADAL avulla.

Jos haluat aloittaa, [Lataa rakenne projektin](https://github.com/AzureADQuickStarts/NativeClient-WindowsPhone/archive/skeleton.zip) tai [Lataa Esimerkki valmiista](https://github.com/AzureADQuickStarts/NativeClient-WindowsPhone/archive/complete.zip).  Jokainen on Visual Studio 2013-ratkaisun.  Sinun on myös Azure AD-vuokraajan, jossa voit luoda käyttäjille ja sovelluksen rekisteröiminen.  Jos sinulla ei vielä ole vuokraajan, [Lue, miten voit hankkia sen](active-directory-howto-tenant.md).

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
- Myös **määrittäminen** -välilehden Etsi "Oikeudet ja muut sovellukset"-kohta.  "Azure Active Directory"-sovelluksen Lisää **Valtuutetun käyttöoikeudet**-kohdassa **Access Your organisaation hakemisto** -käyttöoikeus.  Tämä ottaa käyttöön sovelluksen käyttäjille Graph-Ohjelmointirajapinnan kysely.

## <a name="2-install--configure-adal"></a>*2. Asenna ja määritä ADAL*
Nyt kun olet luonut Azure AD-sovelluksen, voit asentaa ADAL ja kirjoita käyttäjätietojen liittyvä koodi.  Myös ADAL voivat pitää yhteyttä Azure AD-pystyttävä toimittamaan sovelluksen rekisteröinti tietoja.
-   Aloita lisäämällä ADAL DirectorySearcher projektin pakettien hallinta-konsolin avulla.

```
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
```

-   DirectorySearcher Projectissa, Avaa `MainPage.xaml.cs`.  Korvaa arvot `Config Values` alueen kuvaavat arvoja, jotka olet kirjoittanut Azure-portaaliin.  Nämä arvot viittaavat koodisi aina, kun se käyttää ADAL.
    -   `tenant` Azure AD-vuokraajan, esimerkiksi contoso.onmicrosoft.com toimialue
    -   `clientId` On portaalista kopioimasi sovelluksen clientId.
-   Sinun täytyy nyt Tutustu Windows Phone-sovelluksen takaisinsoitto uri.  Määrittää keskeytyskohdasta rivillä `MainPage` menetelmää:

```
redirectURI = Windows.Security.Authentication.Web.WebAuthenticationBroker.GetCurrentApplicationCallbackUri();
```
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

    // ADAL for Windows Phone 8.1 builds AuthenticationContext instances through a factory
    authContext = AuthenticationContext.CreateAsync(authority).GetResults();
}
```

- Selaa `Search(...)` menetelmä, joka käynnistyy, kun käyttäjä cliks "haku-painiketta sovelluksen käyttöliittymässä.  Tämä menetelmä tekee GET-pyynnössä kyselyyn Azure AD-kaavio-Ohjelmointirajapinnan käyttäjille, joiden UPN alkaa tietyllä hakusanoja.  Mutta kyselyn Graph-Ohjelmointirajapinta, jotta sinun täytyy lisätä access_token- `Authorization` otsikon tämä on pyynnön - kohtaa, johon ADAL on.

```C#
private async void Search(object sender, RoutedEventArgs e)
{
    ...

    // Try to get a token without triggering any user prompt.
    // ADAL will check whether the requested token is in ADAL's token cache or can otherwise be obtained without user interaction.
    AuthenticationResult result = await authContext.AcquireTokenSilentAsync(graphResourceId, clientId);
    if (result != null && result.Status == AuthenticationStatus.Success)
    {
        // A token was successfully retrieved.
        QueryGraph(result);
    }
    else
    {
        // Acquiring a token without user interaction was not possible.
        // Trigger an authentication experience and specify that once a token has been obtained the QueryGraph method should be called
        authContext.AcquireTokenAndContinue(graphResourceId, clientId, redirectURI, QueryGraph);
    }
}
```
- Jos vuorovaikutteinen todennus on tarpeen, ADAL käyttää Windows Phone Web todennus Broker (Windowsin Osoitteiston) ja [jatkamisen mallin](http://www.cloudidentity.com/blog/2014/06/16/adal-for-windows-phone-8-1-deep-dive/) näyttämään Azure AD kirjautumissivusta.  Käyttäjän kirjautuessa sisään sovelluksen tarvitsee välittää ADAL Windowsin Osoitteiston vuorovaikutuksen tulokset.  Tämä onnistuu helposti vain käyttöönoton `ContinueWebAuthentication` käyttöliittymän:

```C#
// This method is automatically invoked when the application
// is reactivated after an authentication interaction through WebAuthenticationBroker.
public async void ContinueWebAuthentication(WebAuthenticationBrokerContinuationEventArgs args)
{
    // pass the authentication interaction results to ADAL, which will
    // conclude the token acquisition operation and invoke the callback specified in AcquireTokenAndContinue.
    await authContext.ContinueAcquireTokenAsync(args);
}
```

- Nyt kannattaa käyttää `AuthenticationResult` , ADAL palautettava sovellus.  Valitse `QueryGraph(...)` takaisinkutsun, hankit access_token liittäminen Authorization-otsikko GET-pyynnössä:

```C#
private async void QueryGraph(AuthenticationResult result)
{
    if (result.Status != AuthenticationStatus.Success)
    {
        MessageDialog dialog = new MessageDialog(string.Format("If the error continues, please contact your administrator.\n\nError: {0}\n\nError Description:\n\n{1}", result.Error, result.ErrorDescription), "Sorry, an error occurred while signing you in.");
        await dialog.ShowAsync();
    }

    // Add the access token to the Authorization Header of the call to the Graph API, and call the Graph API.
    httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);

    ...
}
```
- Voit käyttää myös `AuthenticationResult` objekti käyttäjän tietoja sovellukseen. Valitse `QueryGraph(...)` -menetelmä näkyy käyttäjän tunnus sivun tulos avulla:

```C#
// Update the Page UI to represent the signed in user
ActiveUser.Text = result.UserInfo.DisplayableId;
```
- Lopuksi voit kirjautua ulos sovelluksesta sekä käyttäjän ADAL.  Kun käyttäjä napsauttaa "Kirjaudu ulos"-painiketta, haluamme varmistaa, että seuraava puhelu `AcquireTokenSilentAsync(...)` epäonnistuu.  ADAL tämä on yhtä helppoa kuin suojaustunnuksen välimuistin tyhjentäminen:

```C#
private void SignOut()
{
    // Clear session state from the token cache.
    authContext.TokenCache.Clear();

    ...
}
```

Onnittelen! Sinun on nyt toimiva Windows Phone-sovellusta, joka pystyy todentaa käyttäjät, Soita suojatusti verkko-ohjelmointirajapinnan käyttäminen OAuth 2.0, ja poistaa käyttäjän perustietoja.  Jos et ole jo, nyt on aika, joka täyttää alihallintaan joidenkin käyttäjien kanssa.  Suorita DirectorySearcher-sovellus ja kirjaudu sisään jokin kyseisille käyttäjille.  Etsi muiden käyttäjien niiden UPN perusteella.  Sulje sovellus ja käyttää sitä.  Huomaa, miten käyttäjän istunnon säilyy muuttumattomana.  Kirjaudu ulos ja kirjaudu takaisin sisään toiselle käyttäjälle.

ADAL voi liittää kaikkia näitä yleisiä identity-ominaisuuksia sovelluksesi on helppoa.  Muokattu-työstä kestää huolellisesti, - välimuistin hallinnan, OAuth-protokollatuki esitykset käyttäjän kirjautuminen Käyttöliittymä, päivittäminen ja vanhentuneet tunnusten.  Kaikki on varmasti tiedettävä on yksittäinen API-kutsu `authContext.AcquireToken*(…)`.

Viittaus (ilman määritysarvot) Esimerkki valmiista toimitetaan [Tässä](https://github.com/AzureADQuickStarts/NativeClient-WindowsPhone/archive/complete.zip).  Voit nyt siirtää Lisää tunnistetiedot skenaarioita.  Voit halutessasi kokeilla:

[Suojattu .NET verkko-Ohjelmointirajapinnan kanssa Azure AD >>](active-directory-devquickstarts-webapi-dotnet.md)

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]