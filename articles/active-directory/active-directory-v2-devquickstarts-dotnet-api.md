<properties
    pageTitle="Azure AD v2.0 .NET verkko-Ohjelmointirajapinnan | Microsoft Azure"
    description=".NET MVC Web API-liittymän, joka hyväksyy tunnusten sekä henkilökohtainen Microsoft-Account muodostaminen ja työpaikan tai oppilaitoksen tilit."
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
    ms.date="10/10/2016"
    ms.author="dastrock"/>

# <a name="secure-an-mvc-web-api"></a>Suojattu MVC verkko-Ohjelmointirajapinnan

Azure Active Directory v2.0 päätepisteen voit suojata Web Ohjelmointirajapinnan käyttäminen [OAuth 2.0](active-directory-v2-protocols.md#oauth2-authorization-code-flow) access tunnusten käyttäjät, joilla on sekä henkilökohtainen Microsoft-tili ja työn ottaminen käyttöön tai oppilaitoksen tilit suojatusti access verkko-Ohjelmointirajapinnan.

> [AZURE.NOTE]
    V2.0 päätepisteen tukee kaikkia Azure Active Directory-skenaariot ja toiminnot.  Voit selvittää, jos sinun on käytettävä v2.0 päätepisteen, lue lisätietoja [v2.0 rajoitukset](active-directory-v2-limitations.md).

ASP.NET Web API voit tehdä tämän käyttämällä Microsoftin OWIN middleware sisältyy .NET Framework 4.5.  OWIN käytetään on tähän muodosta "Tehtäväluettelon" MVC Web API-Liittymän, jonka avulla asiakkaat voivat luoda ja lukea tehtävät käyttäjän Tehtävät-luettelo.  Verkko-Ohjelmointirajapinnan vahvistaa pyynnöt sisältävät kelvollinen käyttöoikeustietue ja hylkää, Älä välitä vahvistus suojatun reitillä pyynnöt.  Tässä esimerkissä on luotu käyttämällä Visual Studio 2015.

## <a name="download"></a>Lataa
Tässä opetusohjelmassa koodi määritetään [GitHub](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet).  Jos haluat seurata, voit [ladata sovelluksen rakenne .zip nimellä](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/skeleton.zip) tai Kloonaa rakenne:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet.git
```

Rakenne-sovelluksen asiakirjan osien uudelleen käyttäminen koodi on yksinkertainen API, mutta puuttuu kaikki käyttäjätiedot liittyvät osat. Jos et halua seurata, voit sen sijaan Kloonaa tai [ladata Esimerkki valmiista](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/skeleton.zip).

```
git clone https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet.git
```

## <a name="register-an-app"></a>Sovelluksen rekisteröiminen
Luo uuden sovelluksen [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)tai noudattamalla seuraavia [toimimalla seuraavasti](active-directory-v2-app-registration.md).  Varmista, että:

- Kopioi alaspäin **Sovellustunnus** sovelluksen myönnetyt, sinun on se pian.

Visual studio-ratkaisun sisältää myös "TodoListClient", joka on yksinkertainen WPF sovellus.  TodoListClient käytetään näytetään, miten käyttäjän merkkejä, ja kuinka asiakas voi myöntää verkko-Ohjelmointirajapinnan pyynnöt.  Tässä tapauksessa TodoListClient ja TodoListService ilmaistaan saman sovellus.  Voit määrittää TodoListClient, sinun on myös:

- Lisää, kun sovellus **Mobile** -ympäristössä.


## <a name="install-owin"></a>Asenna OWIN

Nyt kun sovelluksen rekisteröitymisen haluat määrittää sovelluksen yhteydenpito v2.0 päätepisteen, jotta voit tarkistaa saapuvat pyynnöt ja tunnusten.

- Aloita Avaa ratkaisu ja lisää OWIN middleware NuGet pakettien TodoListService projektin pakettien hallinta-konsolin avulla.

```
PM> Install-Package Microsoft.Owin.Security.OAuth -ProjectName TodoListService
PM> Install-Package Microsoft.Owin.Security.Jwt -ProjectName TodoListService
PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TodoListService
```

## <a name="configure-oauth-authentication"></a>Määritä OAuth-todennus

- Voit lisätä OWIN käynnistys-luokan nimeltä TodoListService projektiin `Startup.cs`.  Oikea napsauttamalla projektin--> **Lisää** --> **Uuden kohteen** --> Etsi "OWIN".  OWIN middleware käynnistää `Configuration(…)` silloin, kun sovellus käynnistyy.
- Muuttaa luokan ilmoituksen `public partial class Startup` -jo indeksoitiin tähän luokkaan osa puolestasi toiseen tiedostoon.  Valitse `Configuration(…)` tehdä ConfgureAuth(...) puhelun web App-sovelluksen käyttöoikeuksien määrittäminen-menetelmä.

```C#
public partial class Startup
{
    public void Configuration(IAppBuilder app)
    {
        ConfigureAuth(app);
    }
}
```

- Avaa tiedosto `App_Start\Startup.Auth.cs` ja käyttöönotto `ConfigureAuth(…)` menetelmä, joka määrittää verkko-Ohjelmointirajapinnan Hyväksy tunnusten v2.0 päätepisteestä.

```C#
public void ConfigureAuth(IAppBuilder app)
{
        var tvps = new TokenValidationParameters
        {
                // In this app, the TodoListClient and TodoListService
                // are represented using the same Application Id - we use
                // the Application Id to represent the audience, or the
                // intended recipient of tokens.

                ValidAudience = clientId,

                // In a real applicaiton, you might use issuer validation to
                // verify that the user's organization (if applicable) has
                // signed up for the app.  Here, we'll just turn it off.

                ValidateIssuer = false,
        };

        // Set up the OWIN pipeline to use OAuth 2.0 Bearer authentication.
        // The options provided here tell the middleware about the type of tokens
        // that will be recieved, which are JWTs for the v2.0 endpoint.

        // NOTE: The usual WindowsAzureActiveDirectoryBearerAuthenticaitonMiddleware uses a
        // metadata endpoint which is not supported by the v2.0 endpoint.  Instead, this
        // OpenIdConenctCachingSecurityTokenProvider can be used to fetch & use the OpenIdConnect
        // metadata document.

        app.UseOAuthBearerAuthentication(new OAuthBearerAuthenticationOptions
        {
                AccessTokenFormat = new Microsoft.Owin.Security.Jwt.JwtFormat(tvps, new OpenIdConnectCachingSecurityTokenProvider("https://login.microsoftonline.com/common/v2.0/.well-known/openid-configuration")),
        });
}
```

- Nyt voit käyttää `[Authorize]` määritteet toiminnot OAuth 2.0 haltijan todennusta huolehtivat ohjaimet.  Koristeleminen `Controllers\TodoListController.cs` luokan Hyväksy-tunniste.  Tämä pakottaa käyttäjän kirjautumaan ennen käyttöä kyseiselle sivulle.

```C#
[Authorize]
public class TodoListController : ApiController
{
```

- Kun valtuutettujen soittajan onnistuneesti käynnistää jonkin `TodoListController` API-toiminnon joutua soittajan tietoja.  OWIN pääsee saatavat sisällä haltijatunnukseen kautta `ClaimsPrincpal` objekti.  

```C#
public IEnumerable<TodoItem> Get()
{
    // You can use the ClaimsPrincipal to access information about the
    // user making the call.  In this case, we use the 'sub' or
    // NameIdentifier claim to serve as a key for the tasks in the data store.

    Claim subject = ClaimsPrincipal.Current.FindFirst(ClaimTypes.NameIdentifier);

    return from todo in todoBag
           where todo.Owner == subject.Value
           select todo;
}
```

-   Avaa- `web.config` TodoListService projektin pääkansiossa tiedosto ja kirjoita määritys-arvot- `<appSettings>` osa.
  - Oman `ida:Audience` on sovelluksen portaalissa edistymistietojen mukainen **Tunnus** .

## <a name="configure-the-client-app"></a>Määritä asiakas-sovellus
Ennen kuin voit tarkastella Todo luettelon palvelun toiminto, haluat määrittää Todo luettelon asiakkaan, niin se tunnusten noutaminen v2.0 päätepisteen ja soittaa-palveluun.

- TodoListClient Projectissa, Avaa `App.config` ja kirjoita määritys-arvoissa `<appSettings>` osa.
  - Oman `ida:ClientId` portaalista kopioimasi tunnus.

Lopuksi Tyhjennä, luominen ja suorittaminen kunkin projektin!  Nyt käytössäsi .NET MVC Web API-Liittymän, joka hyväksyy tunnusta kummankin henkilökohtainen Microsoft-tilin ja työpaikan tai oppilaitoksen tilit.  Kirjaudu sisään TodoListClient ja soita sivustosi api tehtävien lisääminen käyttäjän Tehtävät-luettelo.

Viitteen Esimerkki valmiista (ilman määritysarvot) [tähän .zip taulukkona](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/complete.zip), tai voit voit käytetään sen GitHub:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet.git```

## <a name="next-steps"></a>Seuraavat vaiheet
Voit nyt siirtää aiheisiin sivulle.  Voit halutessasi kokeilla:

[Verkko-Ohjelmointirajapinnan soitettaessa verkkosovellukseen >>](active-directory-v2-devquickstarts-webapp-webapi-dotnet.md)

Muita tietolähteitä Kuittaa ulos:
- [V2.0 Sovelluskehittäjän opas >>](active-directory-appmodel-v2-overview.md)
- [StackOverflow "azure-active directory-tunniste >>](http://stackoverflow.com/questions/tagged/azure-active-directory)

## <a name="get-security-updates-for-our-products"></a>Päivitysten hakeminen tuotteitamme

On suositeltavaa, kun suojauksen tapaukset esiintyvät käymällä [Tämän sivun](https://technet.microsoft.com/security/dd252948) ja tilaamalla neuvoa suojausvaroitusten ilmoitusten saaminen.
