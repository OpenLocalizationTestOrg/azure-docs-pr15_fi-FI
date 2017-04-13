<properties
    pageTitle="Azure AD-.NET aloittaminen | Microsoft Azure"
    description="Miten voit luoda .NET MVC Web API-Liittymän, todennus- ja Azure Active Directory integroituu."
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


# <a name="protect-a-web-api-using-bearer-tokens-from-azure-ad"></a>Verkko-Ohjelmointirajapinnan käyttäminen haltijan tunnusta Azure AD suojaaminen

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Jos luot sovellus, joka on suojattu resurssien käytön haluat tietää, miten voit suojata resursseja epäoikeudenmukaista Accessista.
Azure AD on yksinkertainen ja selkeä suojata sivuston API OAuth haltijan 2.0 Access tunnusten avulla vain muutamalla koodin rivit.

Asp.NET web Apps-sovelluksissa voit tehdä tämän käyttämällä Microsoftin soveltaminen yhteisön perustuva-OWIN middleware sisältyy .NET Framework 4.5.  Tähän Käytämme OWIN voit luoda "Tehtäväluettelon" verkko-Ohjelmointirajapinnan:
-   Määrittää, mitkä Ohjelmointirajapinnan on suojattu.
-   Tarkistaa, että verkko-Ohjelmointirajapinnan puhelut sisältävät kelvollinen Access-tunnuksen.

Jotta voit tehdä tämän, sinun on:

1. Voit rekisteröidä sovelluksen Azure AD
2. Määrittää sovelluksen OWIN todennus putkijohto.
3. Soita, Tee luettelo Web Ohjelmointirajapinnan asiakassovellus määrittäminen

Jos haluat aloittaa, [Lataa sovelluksen rakenne](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/skeleton.zip) tai [Lataa Esimerkki valmiista](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/complete.zip).  Jokainen on Visual Studio 2013-ratkaisun.  Sinun on myös Azure AD-vuokraajan, jolla sovellus rekisteröidään.  Jos sinulla ei sitä vielä ole, [Lue, miten voit hankkia sen](active-directory-howto-tenant.md).


## <a name="1--register-an-application-with-azure-ad"></a>*1. rekisteröidä Azure AD sovellus*
Suojaamiseen sovelluksen ensin tarvitset-sovelluksen luominen vuokraajan ja antaa Azure AD ohjeisiin tiedot.

-   Kirjaudu sisään [Azure hallinta-portaalissa](https://manage.windowsazure.com)
-   Valitse vasemmalla olevasta siirtymisruudussa **Active Directory**
-   Valitse vuokraajan, jolla sovellus rekisteröi.
-   Valitse **sovellukset** -välilehti ja valitse **Lisää** ala-laatikon.
-   Seuraa ohjeita ja luo uusi **Web-sovelluksen ja/tai WebAPI**.
    -   Sovelluksen **nimen** kuvataan loppukäyttäjien sovelluksen.  Kirjoita "tehtäväluettelo palvelun".
    -   **Ohjaa Uri** on värimallin ja merkkijonon yhdistelmä, jota Azure AD käyttäisi palauttaa kaikki tunnusten pyydetty sovelluksen. Kirjoita `https://localhost:44321/` tämän arvon.
-   Kun olet suorittanut rekisteröinti, siirry **määritys** -välilehti ja Etsi **Sovellus tunnus URI** -kenttä.  Kirjoita vuokraajan kielikohtaiset tunnus arvoa, kuten`https://contoso.onmicrosoft.com/TodoListService`
- Tallenna määritykset.  Jätä portaalin Avaa – sinun on myös Rekisteröi asiakassovellus pian.

## <a name="2-set-up-your-app-to-use-the-owin-authentication-pipeline"></a>*2. määrittää sovelluksen OWIN todennus myyntijakso*

Nyt kun olet rekisteröitynyt Azure AD sovelluksen, sinun täytyy sovelluksesi viestintään Azure AD, jotta voit tarkistaa saapuvat pyynnöt ja tunnusten määrittäminen.

-   Aloita Avaa ratkaisu ja lisää OWIN middleware NuGet pakettien TodoListService projektin pakettien hallinta-konsolin avulla.

```
PM> Install-Package Microsoft.Owin.Security.ActiveDirectory -ProjectName TodoListService
PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TodoListService
```

-   Voit lisätä OWIN käynnistys-luokan nimeltä TodoListService projektiin `Startup.cs`.  Oikea napsauttamalla projektin--> **Lisää** --> **Uuden kohteen** --> Etsi "OWIN".  OWIN middleware käynnistää `Configuration(…)` silloin, kun sovellus käynnistyy.
-   Muuttaa luokan ilmoituksen `public partial class Startup` -jo indeksoitiin osa tämän luokan puolestasi toiseen tiedostoon.  Valitse `Configuration(…)` tehdä ConfgureAuth(...) puhelun web App-sovelluksen käyttöoikeuksien määrittäminen-menetelmä.

```C#
public partial class Startup
{
    public void Configuration(IAppBuilder app)
    {
        ConfigureAuth(app);
    }
}
```

-   Avaa tiedosto `App_Start\Startup.Auth.cs` ja käyttöönotto `ConfigureAuth(…)` menetelmää.  Saadaan Parametrit `WindowsAzureActiveDirectoryBearerAuthenticationOptions` toimivat sovelluksen koordinaatit Azure AD yhteydessä.

```C#
public void ConfigureAuth(IAppBuilder app)
{
    app.UseWindowsAzureActiveDirectoryBearerAuthentication(
        new WindowsAzureActiveDirectoryBearerAuthenticationOptions
        {
            Audience = ConfigurationManager.AppSettings["ida:Audience"],
            Tenant = ConfigurationManager.AppSettings["ida:Tenant"]
        });
}
```

-   Nyt voit käyttää `[Authorize]` määritteet toiminnot JWT haltijan todennusta huolehtivat ohjaimet.  Koristeleminen `Controllers\TodoListController.cs` luokan Hyväksy-tunniste.  Tämä pakottaa käyttäjän kirjautumaan ennen käyttöä kyseiselle sivulle.

```C#
[Authorize]
public class TodoListController : ApiController
{
```

- Kun valtuutettujen soittajan onnistuneesti käynnistää jonkin `TodoListController` API-toiminnon joutua soittajan tietoja.  OWIN pääsee saatavat sisällä haltijatunnukseen kautta `ClaimsPrincpal` objekti.  
- Vahvista "käyttöalueiden" paikalla olevat tunnuksen on yleinen vaatimus Web API - Näin varmistat, että käyttäjä on hyväksynyt Todo luettelon palvelun tarvittavat oikeudet:

```C#
public IEnumerable<TodoItem> Get()
{
    // user_impersonation is the default permission exposed by applications in AAD
    if (ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/scope").Value != "user_impersonation")
    {
        throw new HttpResponseException(new HttpResponseMessage {
          StatusCode = HttpStatusCode.Unauthorized,
          ReasonPhrase = "The Scope claim does not contain 'user_impersonation' or scope claim not found"
        });
    }
    ...
}
```

-   Avaa- `web.config` TodoListService projektin pääkansiossa tiedosto ja kirjoita määritys-arvot- `<appSettings>` osa.
  - `ida:Tenant` On Azure AD-vuokraajan, kuten "contoso.onmicrosoft.com" nimi.
  - Oman `ida:Audience` on sovelluksen tunnus URI-sovelluksen Azure-portaalissa edistymistietojen mukainen.

## <a name="3--configure-a-client-application--run-the-service"></a>*3. määrittäminen asiakassovellus ja suorita palvelu*
Ennen kuin voit tarkastella Todo luettelon palvelun toiminto, haluat määrittää Todo luettelon asiakkaan, niin se tunnusten noutaminen AAD ja soittaa-palveluun.

- Siirry [Azure hallinta-portaalin](https://manage.windowsazure.com) takaisin
- Uuden sovelluksen luominen Azure AD-vuokraajan ja valitse **Alkuperäisen asiakassovellus** tuloksena kehotteen.
    -   Sovelluksen **nimen** kuvataan sovelluksesi loppukäyttäjille
    -   Kirjoita `http://TodoListClient/` **Uudelleenohjata Uri** -arvo.
- Kun olet suorittanut rekisteröinti, AAD määrittää sovelluksen yksilöllinen **Asiakastunnus**. Tarvitset tätä arvoa seuraavat vaiheet niin kopioi se määritys-välilehti.
- Myös **määrittäminen** -välilehden Etsi "Oikeudet ja muut sovellukset"-kohta. Valitsemalla "Lisää sovellus." Valitse "Kaikki sovellukset" "Näytä-valikko ja valitse ylemmässä valintamerkki. Etsi ja valitse voit tehdä luettelon palvelusi ja valitse ala-valintaruudun, voit lisätä sovelluksen. Valitse "Access voit tehdä luettelon palvelun" "Valtuutetun käyttöoikeudet" avattavasta valikosta ja Tallenna määritykset.


- Avaa Visual Studion `App.config` project-TodoListClient ja kirjoita määritys-arvot- `<appSettings>` osa.
  - `ida:Tenant` On Azure AD-vuokraajan, kuten "contoso.onmicrosoft.com" nimi.
  - Oman `ida:ClientId` Azure-portaalista kopioimasi Sovellustunnus.
  - Oman `todo:TodoListResourceId` on sovelluksen tunnus URI, Tee luettelo palvelusovelluksen Azure-portaalissa edistymistietojen mukainen.

Lopuksi Tyhjennä, luominen ja suorittaminen kunkin projektin!  Jos et ole jo, nyt on aika, voit luoda uuden käyttäjän kanssa vuokraajan *. onmicrosoft.com-toimialue.  Tehtäväluettelon asiakkaalle kyseisen käyttäjän kirjautuminen ja lisätä joitakin tehtäviä käyttäjän Tehtävät-luettelo.

Viittaus (ilman määritysarvot) Esimerkki valmiista toimitetaan [Tässä](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/complete.zip).  Voit nyt siirtää Lisää muita tunnistetietojen skenaarioita.

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
