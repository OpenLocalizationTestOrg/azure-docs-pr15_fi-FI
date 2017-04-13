<properties
    pageTitle="Mitä on tapahtunut MVC projektin (Visual Studio Azure Active Directory-palvelun yhteydessä) | Microsoft Azure "
    description="Tässä artikkelissa kuvataan, mitä MVC projektin tapahtuu, kun muodostat yhteyden Azure AD yhdistetty Visual Studio-palvelujen avulla"
    services="active-directory"
    documentationCenter="na"
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

# <a name="what-happened-to-my-mvc-project-visual-studio-azure-active-directory-connected-service"></a>Mitä on tapahtunut MVC projektin (Visual Studio Azure Active Directory-palvelun yhteydessä)?

> [AZURE.SELECTOR]
> - [Käytön aloittaminen](vs-active-directory-dotnet-getting-started.md)
> - [Mitä tapahtui](vs-active-directory-dotnet-what-happened.md)



## <a name="references-have-been-added"></a>Viittaukset on lisätty

### <a name="nuget-package-references"></a>NuGet paketin viittaukset

- **Microsoft.IdentityModel.Protocol.Extensions**
- **Microsoft.Owin**
- **Microsoft.Owin.Host.SystemWeb**
- **Microsoft.Owin.Security**
- **Microsoft.Owin.Security.Cookies**
- **Microsoft.Owin.Security.OpenIdConnect**
- **Owin**
- **System.IdentityModel.Tokens.Jwt**

### <a name="net-references"></a>.NET-viittaukset

- **Microsoft.IdentityModel.Protocol.Extensions**
- **Microsoft.Owin**
- **Microsoft.Owin.Host.SystemWeb**
- **Microsoft.Owin.Security**
- **Microsoft.Owin.Security.Cookies**
- **Microsoft.Owin.Security.OpenIdConnect**
- **Owin**
- **System.IdentityModel**
- **System.IdentityModel.Tokens.Jwt**
- **DataContractSerializer**

## <a name="code-has-been-added"></a>Koodi on lisätty

### <a name="code-files-were-added-to-your-project"></a>Koodi-tiedostot on lisätty projektiin

Todennus käynnistys luokan **App_Start/Startup.Auth.cs** on lisätty projektiin sisältävä käynnistys logiikan Azure AD-todennusta varten. Valvonta-luokan Controllers/AccountController.cs on lisätty **SignIn()** ja **SignOut()** menetelmiä sisältää. Lopuksi osittainen näkymä, **Views/Shared/_LoginPartial.cshtml** on lisätty, joka sisältää toiminnon linkki sisäänkirjautuminen ja uloskirjautuminen.

### <a name="startup-code-was-added-to-your-project"></a>Käynnistyskoodi on lisätty projektiin

Jos haluat käyttää aiempaa käynnistys-luokan projektin, **määritys** -menetelmä on päivitetty **ConfigureAuth(app)**puhelun. Muussa tapauksessa käynnistys-luokka on lisätty projektiin.

### <a name="your-appconfig-or-webconfig-has-new-configuration-values"></a>App.config tai web.config on uudet määritykset arvot

Seuraavat määritykset merkinnät on lisätty.


    <appSettings>
        <add key="ida:ClientId" value="ClientId from the new Azure AD App" />
        <add key="ida:AADInstance" value="https://login.microsoftonline.com/" />
        <add key="ida:Domain" value="The selected Azure AD Domain" />
        <add key="ida:TenantId" value="The Id of your selected Azure AD Tenant" />
        <add key="ida:PostLogoutRedirectUri" value="Your project start page" />
    </appSettings>

### <a name="an-azure-active-directory-ad-app-was-created"></a>Azure Active Directory (AD)-sovellus on luotu
Azure AD-sovellus on luotu kansio, jonka olet valinnut ohjatun toiminnon.

##<a name="if-i-checked-disable-individual-user-accounts-authentication-what-additional-changes-were-made-to-my-project"></a>Jos voin valittuna *yksittäisten käyttäjätilien käyttöoikeuksien poistaminen käytöstä*, muut muutokset on tehty projektin?
NuGet paketin viittaukset on poistettu, ja tiedostot on poistettu ja varmuuskopioida. Projektin tila, riippuen saatat joutua manuaalisesti Lisäresurssit tai tiedostojen poistaminen tai muokkaaminen koodia tarvittaessa.

### <a name="nuget-package-references-removed-for-those-present"></a>NuGet paketin viittaukset (niille esitä) poistetaan

- **Microsoft.AspNet.Identity.Core**
- **Microsoft.AspNet.Identity.EntityFramework**
- **Microsoft.AspNet.Identity.Owin**

### <a name="code-files-backed-up-and-removed-for-those-present"></a>Koodin tiedostot varmuuskopioidaan ja poistaa (niille esitä)

Kunkin seuraavat tiedostot on varmuuskopioida ja poistaa projektin. Projektin directory ylimmällä tasolla "Varmuuskopiointi"-kansiossa sijaitsevat varmuuskopiot.

- **App_Start\IdentityConfig.cs**
- **Controllers\ManageController.cs**
- **Models\IdentityModels.cs**
- **Models\ManageViewModels.cs**

### <a name="code-files-backed-up-for-those-present"></a>Kooditiedostoja varmuuskopioida (niille esitä)

Kunkin seuraavat tiedostot on varmuuskopioida ennen korvata uusilla käyttäjillä. Projektin directory ylimmällä tasolla "Varmuuskopiointi"-kansiossa sijaitsevat varmuuskopiot.

- **Startup.cs**
- **App_Start\Startup.auth.cs**
- **Controllers\AccountController.cs**
- **Views\Shared\_LoginPartial.cshtml**

## <a name="if-i-checked-read-directory-data-what-additional-changes-were-made-to-my-project"></a>Jos voin valittuna *kansion tietojen*, mitä muut muutokset on tehty projektin?

Lisäresurssit on lisätty.

###<a name="additional-nuget-package-references"></a>NuGet-paketin Lisäresurssit

- **EntityFramework**
- **Microsoft.Azure.ActiveDirectory.GraphClient**
- **Microsoft.Data.Edm**
- **Microsoft.Data.OData**
- **Microsoft.Data.Services.Client**
- **Microsoft.IdentityModel.Clients.ActiveDirectory**
- **System.Spatial**

###<a name="additional-net-references"></a>.NET Lisäresurssit

- **EntityFramework**
- **EntityFramework.SqlServer**
- **Microsoft.Azure.ActiveDirectory.GraphClient**
- **Microsoft.Data.Edm**
- **Microsoft.Data.OData**
- **Microsoft.Data.Services.Client**
- **Microsoft.IdentityModel.Clients.ActiveDirectory**
- **Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms**
- **System.Spatial**

###<a name="additional-code-files-were-added-to-your-project"></a>Kooditiedostoja on lisätty projektiin

Kaksi tiedostot on lisätty tukemaan suojaustunnuksen välimuistiin: **Models\ADALTokenCache.cs** ja **Models\ApplicationDbContext.cs**.  Lisää ohjaimen ja Näytä on lisätty esitä käytettäessä käyttäjäprofiilin tietoja Azure Graphin ohjelmointirajapinnan käyttämisestä.  Nämä tiedostot ovat **Controllers\UserProfileController.cs** ja **Views\UserProfile\Index.cshtml**.

###<a name="additional-startup-code-was-added-to-your-project"></a>Lisää käynnistys-koodi on lisätty projektiin

**Startup.auth.cs** -tiedosto uusi **OpenIdConnectAuthenticationNotifications** -objekti on lisätty **OpenIdConnectAuthenticationOptions** **ilmoitukset** jäsen.  Tämä on käyttöön vastaanottaminen OAuth-koodin ja exchange käyttöoikeustietue varten.

###<a name="additional-changes-were-made-to-your-appconfig-or-webconfig"></a>App.config tai web.config on tehty muutoksia

Seuraavat lisämäärityksiä merkinnät on lisätty.

    <appSettings>
        <add key="ida:ClientSecret" value="Your Azure AD App's new client secret" />
    </appSettings>

Määritysten seuraavissa ja yhteysmerkkijonon on lisätty.

    <configSections>
        <!-- For more information on Entity Framework configuration, visit http://go.microsoft.com/fwlink/?LinkID=237468 -->
        <section name="entityFramework" type="System.Data.Entity.Internal.ConfigFile.EntityFrameworkSection, EntityFramework, Version=6.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" requirePermission="false" />
    </configSections>
    <connectionStrings>
        <add name="DefaultConnection" connectionString="Data Source=(localdb)\MSSQLLocalDB;AttachDbFilename=|DataDirectory|\aspnet-[AppName + Generated Id].mdf;Initial Catalog=aspnet-[AppName + Generated Id];Integrated Security=True" providerName="System.Data.SqlClient" />
    </connectionStrings>
    <entityFramework>
        <defaultConnectionFactory type="System.Data.Entity.Infrastructure.LocalDbConnectionFactory, EntityFramework">
          <parameters>
            <parameter value="mssqllocaldb" />
          </parameters>
        </defaultConnectionFactory>
        <providers>
          <provider invariantName="System.Data.SqlClient" type="System.Data.Entity.SqlServer.SqlProviderServices, EntityFramework.SqlServer" />
        </providers>
    </entityFramework>


###<a name="your-azure-active-directory-app-was-updated"></a>Azure Active Directory sovelluksen on päivitetty
Azure Active Directory sovelluksen on päivitetty sisältämään *directory tietojen* käyttöoikeudet ja muita avain on luotu, jossa on käytetty sitten *HVT: ClientSecret* - **seuraavan koodin korostetut** .

[Lisätietoja Azure Active Directory](https://azure.microsoft.com/services/active-directory/)
