<properties
   pageTitle="Azure Active Directory v2.0 todennus kirjastojen | Microsoft Azure"
   description="Yhteensopivat asiakkaan kirjastojen ja palvelimen middleware kirjastojen ja liittyvät kirjaston, lähde ja näytteiden linkit Azure Active Directory v2.0 päätepisteen."
   services="active-directory"
   documentationCenter=""
   authors="skwan"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/30/2016"
   ms.author="skwan;bryanla"/>


# <a name="azure-active-directory-v20-authentication-libraries"></a>Azure Active Directory-v2.0 todennus-kirjastot
Azure Active Directory (Azure AD) v2.0 päätepisteen tukee yleisesti OAuth 2.0 ja OpenID yhteyden 1.0-protokollaa. Voit käyttää eri kirjastoihin Microsoftin ja muiden organisaatioiden v2.0 päätepisteen kanssa.

Kun luot v2.0 päätepisteen käyttävä sovellus, on suositeltavaa, että käytät kirjastoissa, jotka on kirjoitettu protokollan toimialueen asiantuntijoiden kuka seuraa Security Development Lifecycle (SDL) kehitysmenetelmistä, kuten [sen perään Microsoft][Microsoft-SDL]. Jos päätät protokollat tuki käsi-koodi, on suositeltavaa seuraa SDL menetelmät ja maksaa suojausasiat huomiota kunkin protokollan mukaisia mukaisesti.

## <a name="types-of-libraries"></a>Kirjastotyypit

Kaksi tiedostomalleja työskentelee Azure AD-v2.0:

- **Asiakkaan kirjastoihin**. Alkuperäisiä asiakkaiden ja palvelinten hakea access tunnuksia soitettavien resurssi, kuten Microsoft Graph asiakkaan kirjastojen avulla.
- **Palvelimen middleware kirjastoihin**. Web Apps-sovellusten käyttää palvelimen middleware kirjastojen käyttäjien sisäänkirjautumisessa. Verkko-ohjelmointirajapinnan Vahvista tunnuksia, jotka lähetetään alkuperäisiä asiakkaiden tai muiden palvelinten palvelimen middleware kirjastojen avulla.

## <a name="library-support"></a>Kirjaston tuki
Voit valita minkä tahansa-standardin mukaisia kirjaston käytettäessä v2.0 päätepisteen, se on tärkeää tietää, miten jatkaa tuki. Ongelmat ja ehdottaa ominaisuuksia kirjaston koodissa kirjaston omistajalta. Kysy Microsoft ongelmat ja ehdottaa ominaisuuksia-palvelun Include protokolla-ympäristöön.

Kirjastojen annettava tuki kahteen luokkaan:

- **Microsoftin tukema**. Microsoft tarjoaa nämä kirjastot korjaukset ja on tehnyt SDL-nämä kirjastot kassaan asianmukaisesti.
- **Yhteensopivat**. Microsoft on testannut nämä kirjastot basic tilanteissa ja varmistaneet, että ne toimivat v2.0 päätepiste. Microsoft ei tarjoa ratkaisuja nämä kirjastot ja ole tehnyt nämä kirjastot lukemalla. Kirjaston Avaa lähde projektin pitäisi ohjautuu ongelmia ja ehdottaa ominaisuuksia.

Katso kirjastoissa, jotka toimivat v2.0 päätepisteen luettelo on tämän artikkelin seuraavien osien.

## <a name="microsoft-supported-client-libraries"></a>Microsoftin tukema asiakas-kirjastot
| Käyttöympäristö| Kirjaston nimi| Lataa | Lähdekoodin | Esimerkki |
| :-: | :-: | :-: | :-: | :-: |
| .NET-Windows-kaupan Xamarin | .NET kirjasto Microsoft-todennus (MSAL) | [Microsoft.Identity.Client (NuGet)][ClientLib-NET-Lib] | [.NET (GitHub) MSAL][ClientLib-NET-Repo] | [Windows desktop native client malli][ClientLib-NET-Sample] |
| Node.js | Microsoft Azure Active Directory Passport.js laajennus | [Passport-Azure-AD (npm)][ClientLib-Node-Lib] | [Passport-Azure-AD (GitHub)][ClientLib-Node-Repo] | Tulossa pian |

<!--- COMMENTING OUT UNTIL THEY ARE READY
| iOS, Mac | Microsoft Authentication Library (MSAL) for ObjC | In development | In development | In development |
| Android | Microsoft Authentication Library (MSAL) for Android | In development | In development | In development |
| JavaScript | Microsoft Authentication Library (MSAL) for JavaScript | In development | In development | In development |
 -->

## <a name="microsoft-supported-server-middleware-libraries"></a>Microsoftin tukema server middleware-kirjastot
| Käyttöympäristö| Kirjaston nimi| Lataa | Lähdekoodin | Esimerkki |
| :-: | :-: | :-: | :-: | :-: |
| .NET 4.x | OWIN OpenID muodostetaan Middleware ASP.NET | [Microsoft.Owin.Security.OpenIdConnect (NuGet)][ServerLib-Net4-Owin-Oidc-Lib] | [Projektin Katana (CodePlex)][ServerLib-Net4-Owin-Oidc-Repo] | [Web-sovelluksen malli][ServerLib-Net4-Owin-Oidc-Sample] |
| .NET 4.x | OWIN OAuth haltijan Middleware ASP.NET varten | [Microsoft.Owin.Security.OAuth (NuGet)][ServerLib-Net4-Owin-Oauth-Lib] | [Projektin Katana (CodePlex)][ServerLib-Net4-Owin-Oauth-Repo] | [Verkko-Ohjelmointirajapinnan malli][ServerLib-Net4-Owin-Oauth-Sample] |
| .NET core | OWIN OpenID muodostetaan Middleware .NET Core | [Microsoft.AspNetCore.Authentication.OpenIdConnect (NuGet)][ServerLib-NetCore-Owin-Oidc-Lib] | [ASP.NET-suojauksen (GitHub)][ServerLib-NetCore-Owin-Oidc-Repo] | [Web-sovelluksen malli][ServerLib-NetCore-Owin-Oidc-Sample] |
| .NET core | OWIN OAuth haltijan Middleware .NET Core varten | [Microsoft.AspNetCore.Authentication.OAuth (NuGet)][ServerLib-NetCore-Owin-Oauth-Lib] | [ASP.NET-suojauksen (GitHub)][ServerLib-NetCore-Owin-Oauth-Repo] | Tulossa pian |
| Node.js | Microsoft Azure Active Directory Passport.js laajennus | [Passport-Azure-AD (npm)][ServerLib-Node-Lib] | [Passport-Azure-AD (GitHub)][ServerLib-Node-Repo] | [Web-sovelluksen malli][ServerLib-Node-Sample] |
<!--- COMMENTING UNTIL SAMPLE IS AVAILABLE
| .NET 4.x, .NET Core | JSON Web Token Handler for .NET | [System.IdentityModel.Tokens.Jwt (NuGet)][ServerLib-Net-Jwt-Lib] | [Azure AD identity model extensions for .NET (GitHub)][ServerLib-Net-Jwt-Repo] | Coming soon |
--->
## <a name="compatible-client-libraries"></a>Yhteensopivat asiakas-kirjastot
| Käyttöympäristö| Kirjaston nimi | Testattu versio | Lähdekoodin | Esimerkki |
| :-: | :-: | :-: | :-: | :-: |
| Android-laitteeseen | [OIDCAndroidLib](https://github.com/kalemontes/OIDCAndroidLib/wiki) | 0.2.1 | [OIDCAndroidLib](https://github.com/kalemontes/OIDCAndroidLib) | [Alkuperäisen sovelluksen malli](active-directory-v2-devquickstarts-android.md) |
| iOS | [NXOAuth2Client](https://github.com/nxtbgthng/OAuth2Client) | 1.2.8 | [NXOAuth2Client](https://github.com/nxtbgthng/OAuth2Client) | [Alkuperäisen sovelluksen malli](active-directory-v2-devquickstarts-ios.md)|
| JavaScript | [Hello.js](https://adodson.com/hello.js/) | 1.13.5 | [Hello.js](https://github.com/MrSwitch/hello.js) | Tulossa pian |
| Python n | [: N OAuthlib](https://github.com/lepture/flask-oauthlib) | 0.9.3 | [: N OAuthlib](https://github.com/lepture/flask-oauthlib) | Tulossa pian |
| Ruby | [OmniAuth](https://github.com/omniauth/omniauth/wiki) | omniauth:1.3.1</br>omniauth-oauth2:1.4.0 | [OmniAuth](https://github.com/omniauth/omniauth)</br>[OmniAuth OAuth2](https://github.com/intridea/omniauth-oauth2) | Tulossa pian |
<!--- REMOVING BRANDON'S FOR NOW
|  |  |  |  |  |
| Android | [OAuth2 Client](https://github.com/wuman/android-oauth-client) |   | [OAuth2 Client](https://github.com/wuman/android-oauth-client)  | Coming soon  |
| Java | [WSO2 Identity Server](https://docs.wso2.com/display/IS500/Introducing+the+Identity+Server) | [Version 5.2.0](http://wso2.com/products/identity-server/) | [Source](https://docs.wso2.com/display/IS500/Building+from+Source) | [Samples index](https://docs.wso2.com/display/IS500/Samples)  |
| Java | [Java Gluu Server](https://gluu.org/docs/) |   | [oxAuth](https://github.com/GluuFederation/oxAuth)  | Coming soon |
| Node.js | [NPM passport-openidconnect](https://www.npmjs.com/package/passport-openidconnect) | 0.0.1  | [Passport-OpenID Connect](https://github.com/jaredhanson/passport-openidconnect) | Coming soon  |
| PHP | [OpenID Connect Basic Client](https://github.com/jumbojett/OpenID-Connect-PHP) |   | [OpenID Connect Basic Client](https://github.com/jumbojett/OpenID-Connect-PHP)  | Coming soon  |
-->

## <a name="compatible-server-middleware-libraries"></a>Yhteensopivat server middleware-kirjastot
Tulossa pian

## <a name="related-content"></a>Aiheeseen liittyvää sisältöä
Saat lisätietoja Azure AD-v2.0 päätepisteen [Azure AD app v2.0 yleiskatsaus][AAD-App-Model-V2-Overview].

Auta meitä tarkentaminen ja muodon pitääkö sisältö käyttämällä Disqus kommentit-toiminto on tämän artikkelin lopussa olevassa antaa palautetta.

<!--Image references-->

<!--Reference style links -->
[AAD-App-Model-V2-Overview]: active-directory-appmodel-v2-overview.md
[ClientLib-NET-Lib]: http://www.nuget.org/packages/Microsoft.Identity.Client
[ClientLib-NET-Repo]: https://github.com/AzureAD/microsoft-authentication-library-for-dotnet
[ClientLib-NET-Sample]: active-directory-v2-devquickstarts-wpf.md
[ClientLib-Node-Lib]: https://www.npmjs.com/package/passport-azure-ad
[ClientLib-Node-Repo]: https://github.com/AzureAD/passport-azure-ad
[ClientLib-Node-Sample]:
[ClientLib-Iosmac-Lib]:
[ClientLib-Iosmac-Repo]:
[ClientLib-Iosmac-Sample]:
[ClientLib-Android-Lib]:
[ClientLib-Android-Repo]:
[ClientLib-Android-Sample]:
[ClientLib-Js-Lib]:
[ClientLib-Js-Repo]:
[ClientLib-Js-Sample]:
[Microsoft-SDL]: http://www.microsoft.com/sdl/default.aspx
[ServerLib-Net4-Owin-Oidc-Lib]: https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect/
[ServerLib-Net4-Owin-Oidc-Repo]: http://katanaproject.codeplex.com/
[ServerLib-Net4-Owin-Oidc-Sample]: active-directory-v2-devquickstarts-dotnet-web.md
[ServerLib-Net4-Owin-Oauth-Lib]: https://www.nuget.org/packages/Microsoft.Owin.Security.OAuth/
[ServerLib-Net4-Owin-Oauth-Repo]: http://katanaproject.codeplex.com/
[ServerLib-Net4-Owin-Oauth-Sample]: https://azure.microsoft.com/en-us/documentation/articles/active-directory-v2-devquickstarts-dotnet-api/
[ServerLib-Net-Jwt-Lib]: https://www.nuget.org/packages/System.IdentityModel.Tokens.Jwt
[ServerLib-Net-Jwt-Repo]: https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet
[ServerLib-Net-Jwt-Sample]:/
[ServerLib-NetCore-Owin-Oidc-Lib]: https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.OpenIdConnect/
[ServerLib-NetCore-Owin-Oidc-Repo]: https://github.com/aspnet/Security
[ServerLib-NetCore-Owin-Oidc-Sample]: https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect-aspnetcore-v2
[ServerLib-NetCore-Owin-Oauth-Lib]: https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.OAuth/
[ServerLib-NetCore-Owin-Oauth-Repo]: https://github.com/aspnet/Security
[ServerLib-NetCore-Owin-Oauth-Sample]:/
[ServerLib-Node-Lib]: https://www.npmjs.com/package/passport-azure-ad
[ServerLib-Node-Repo]: https://github.com/AzureAD/passport-azure-ad/
[ServerLib-Node-Sample]: https://azure.microsoft.com/en-us/documentation/articles/active-directory-v2-devquickstarts-node-web/
