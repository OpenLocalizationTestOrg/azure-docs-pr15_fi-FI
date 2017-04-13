<properties
    pageTitle="Mitä on tapahtunut WebApi projektin (Visual Studio Azure Active Directory-palvelun yhteydessä) | Microsoft Azure "
    description="Tässä artikkelissa kuvataan, mitä tapahtuu MVC projektin WebApi Visual Studiossa yhdistäminen Azure AD"
  services="active-directory"
    documentationCenter=""
    authors="TomArcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="web"
    ms.tgt_pltfrm="vs-what-happened"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="tarcher"/>

# <a name="what-happened-to-my-webapi-project-visual-studio-azure-active-directory-connected-service"></a>Mitä on tapahtunut WebApi projektin (Visual Studio Azure Active Directory-palvelun yhteydessä)

> [AZURE.SELECTOR]
> - [Käytön aloittaminen](vs-active-directory-webapi-getting-started.md)
> - [Mitä tapahtui](vs-active-directory-webapi-what-happened.md)

##<a name="references-have-been-added"></a>Viittaukset on lisätty

###<a name="nuget-package-references"></a>NuGet paketin viittaukset

- `Microsoft.Owin`
- `Microsoft.Owin.Host.SystemWeb`
- `Microsoft.Owin.Security`
- `Microsoft.Owin.Security.ActiveDirectory`
- `Microsoft.Owin.Security.Jwt`
- `Microsoft.Owin.Security.OAuth`
- `Owin`
- `System.IdentityModel.Tokens.Jwt`

###<a name="net-references"></a>.NET-viittaukset

- `Microsoft.Owin`
- `Microsoft.Owin.Host.SystemWeb`
- `Microsoft.Owin.Security`
- `Microsoft.Owin.Security.ActiveDirectory`
- `Microsoft.Owin.Security.Jwt`
- `Microsoft.Owin.Security.OAuth`
- `Owin`
- `System.IdentityModel.Tokens.Jwt`

##<a name="code-changes"></a>Muutokset

###<a name="code-files-were-added-to-your-project"></a>Koodi-tiedostot on lisätty projektiin

Todennus käynnistys luokan **App_Start/Startup.Auth.cs** on lisätty projektiin sisältävä käynnistys logiikan Azure AD-todennusta varten.

###<a name="startup-code-was-added-to-your-project"></a>Käynnistyskoodi on lisätty projektiin

Jos haluat käyttää aiempaa käynnistys-luokan projektin, **määritys** -menetelmä on päivitetty kutsu `ConfigureAuth(app)`. Muussa tapauksessa käynnistys-luokka on lisätty projektiin.


###<a name="your-appconfig-or-webconfig-file-has-new-configuration-values"></a>App.config tai web.config-tiedosto on uusi määritysarvoja.

Seuraavat määritykset merkinnät on lisätty.
```
    `<appSettings>
            <add key="ida:ClientId" value="ClientId from the new Azure AD App" />
            <add key="ida:Tenant" value="Your selected Azure AD Tenant" />
            <add key="ida:Audience" value="The App ID Uri from the wizard" />
    </appSettings>`
```

###<a name="an-azure-ad-app-was-created"></a>Azure AD-sovellus on luotu

Azure AD-sovellus on luotu kansio, jonka olet valinnut ohjatun toiminnon.

[Lisätietoja Azure Active Directory](https://azure.microsoft.com/services/active-directory/)

##<a name="if-i-checked-disable-individual-user-accounts-authentication-what-additional-changes-were-made-to-my-project"></a>Jos voin valittuna *yksittäisten käyttäjätilien käyttöoikeuksien poistaminen käytöstä*, muut muutokset on tehty projektin?
NuGet paketin viittaukset on poistettu, ja tiedostot on poistettu ja varmuuskopioida. Projektin tila, riippuen saatat joutua manuaalisesti Lisäresurssit tai tiedostojen poistaminen tai muokkaaminen koodia tarvittaessa.

###<a name="nuget-package-references-removed-for-those-present"></a>NuGet paketin viittaukset (niille esitä) poistetaan

- `Microsoft.AspNet.Identity.Core`
- `Microsoft.AspNet.Identity.EntityFramework`
- `Microsoft.AspNet.Identity.Owin`

###<a name="code-files-backed-up-and-removed-for-those-present"></a>Koodin tiedostot varmuuskopioidaan ja poistaa (niille esitä)

Kunkin seuraavat tiedostot on varmuuskopioida ja poistaa projektin. Projektin directory ylimmällä tasolla "Varmuuskopiointi"-kansiossa sijaitsevat varmuuskopiot.

- `App_Start\IdentityConfig.cs`
- `Controllers\AccountController.cs`
- `Controllers\ManageController.cs`
- `Models\IdentityModels.cs`
- `Providers\ApplicationOAuthProvider.cs`

###<a name="code-files-backed-up-for-those-present"></a>Kooditiedostoja varmuuskopioida (niille esitä)

Kunkin seuraavat tiedostot on varmuuskopioida ennen korvata uusilla käyttäjillä. Projektin directory ylimmällä tasolla "Varmuuskopiointi"-kansiossa sijaitsevat varmuuskopiot.

- `Startup.cs`
- `App_Start\Startup.Auth.cs`

##<a name="if-i-checked-read-directory-data-what-additional-changes-were-made-to-my-project"></a>Jos voin valittuna *kansion tietojen*, mitä muut muutokset on tehty projektin?

###<a name="additional-changes-were-made-to-your-appconfig-or-webconfig"></a>App.config tai web.config on tehty muutoksia

Seuraavat lisämäärityksiä merkinnät on lisätty.

```
    `<appSettings>
        <add key="ida:Password" value="Your Azure AD App's new password" />
    </appSettings>`
```

###<a name="your-azure-active-directory-app-was-updated"></a>Azure Active Directory sovelluksen on päivitetty
Azure Active Directory sovelluksen on päivitetty sisältämään *luku kansion tiedot* -oikeudet ja muut avain on luotu, jossa on käytetty sitten *HVT: salasana* `web.config` tiedosto.

[Lisätietoja Azure Active Directory](https://azure.microsoft.com/services/active-directory/)
