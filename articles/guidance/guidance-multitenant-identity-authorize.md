<properties
   pageTitle="Luvan multitenant sovelluksissa | Microsoft Azure"
   description="Tietoja luvan multitenant-sovelluksessa"
   services=""
   documentationCenter="na"
   authors="MikeWasson"
   manager="roshar"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="06/02/2016"
   ms.author="mwasson"/>

# <a name="role-based-and-resource-based-authorization-in-multitenant-applications"></a>Roolipohjainen ja resurssi-pohjainen todennus multitenant-sovelluksissa

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Tässä artikkelissa on [sarjaan kuuluvan]. On myös valmis [sovelluksen malli] , jonka mukana sarjassa.

Tutustu [viittaus käyttöönoton] on ASP.NET Core 1.0-sovellus. Tässä artikkelissa tarkastellaan kaksi yleistä tavoista luvan, luvan ASP.NET Core 1.0 säädetty ohjelmointirajapinnan käyttäminen.

-   **Roolipohjainen käyttöoikeuksien**. Sallimisesta toiminnon, joka on määritetty käyttäjälle roolien perusteella. Esimerkiksi jotkin toiminnot vaativat järjestelmänvalvoja-roolista.
-   **Resurssi-pohjainen todennus**. Sallimisesta toiminnon tietyn resurssin perusteella. Esimerkiksi joka resurssilla on omistaja. Omistaja voi poistaa resurssin; muiden käyttäjien ei.

Tyypillinen sovelluksen käyttää niiden sekoitusta. Esimerkiksi resurssin poistaminen käyttäjän on oltava resurssin omistaja _tai_ järjestelmänvalvojaksi.


## <a name="role-based-authorization"></a>Roolipohjainen käyttöoikeuksien

[Tailspin kyselyt] [ Tailspin] sovellus määrittää seuraavia rooleja:

- Järjestelmänvalvoja. Voit suorittaa kyselyn, joka kuuluu kyseisen alihallinnan kaikki CRUD toimenpiteet.
- Tekijän. Voit luoda uuden kyselyt
- Lukija. Voit lukea kyselyt, jotka kuuluvat kyseisen alihallinnan

Roolien käyttää sovelluksen _käyttäjille_ . Valitse Kyselyt-sovelluksessa käyttäjän on joko järjestelmänvalvojan, luoja tai reader.

Lisätietoja siitä, miten voit määrittää ja roolien hallinta on artikkelissa [sovelluksen roolit].

Riippumatta siitä, miten voit hallita roolit-todennus-koodi muistuttaa. ASP.NET-Core 1.0 esitellään nimeltään [luvan käytännöt]otetaan[policies]. Tämän ominaisuuden luvan suojauskäytäntöjen koodi ja käytä sitten ohjauskoneen toiminnot kyseiset käytännöt. Käytäntö on irrotettu lähettämät.

### <a name="create-policies"></a>Luo käytäntöjä

Jos haluat määrittää käytännön, luo luokkaan, joka toteuttaa `IAuthorizationRequirement`. Se on helpointa johdettu `AuthorizationHandler`. Valitse `Handle` -menetelmä tutkia asiaa claim(s).

Tässä on esimerkki Tailspin kyselyt-sovelluksesta:

```csharp
public class SurveyCreatorRequirement : AuthorizationHandler<SurveyCreatorRequirement>, IAuthorizationRequirement
{
    protected override void Handle(AuthorizationContext context, SurveyCreatorRequirement requirement)
    {
        if (context.User.HasClaim(ClaimTypes.Role, Roles.SurveyAdmin) ||
            context.User.HasClaim(ClaimTypes.Role, Roles.SurveyCreator))
        {
            context.Succeed(requirement);
        }
    }
}
```

> [AZURE.NOTE] Katso [SurveyCreatorRequirement.cs]

Tähän luokkaan määrittää vaatimus käyttäjä voi luoda uuden kyselyn. Käyttäjän on oltava SurveyAdmin tai SurveyCreator rooli.

Määrittää nimetyn käytännön, joka sisältää vähintään yhden vaatimukset käynnistys-luokkaan. Jos määritettynä on useita vaatimukset, käyttäjän on täytettävä _jokaisen_ vaatimus on sallittu. Seuraava koodi määrittää kahden käytännöt:

```csharp
services.AddAuthorization(options =>
{
    options.AddPolicy(PolicyNames.RequireSurveyCreator,
        policy =>
        {
            policy.AddRequirements(new SurveyCreatorRequirement());
            policy.AddAuthenticationSchemes(CookieAuthenticationDefaults.AuthenticationScheme);
        });

    options.AddPolicy(PolicyNames.RequireSurveyAdmin,
        policy =>
        {
            policy.AddRequirements(new SurveyAdminRequirement());
            policy.AddAuthenticationSchemes(CookieAuthenticationDefaults.AuthenticationScheme);
        });
});
```

> [AZURE.NOTE] Katso [Startup.cs]

Tämä koodi asettaa myös todennusmalli, josta kertoo ASP.NET mitä todennusta middleware pitäisi toimia, jos todennus epäonnistuu. Tässä tapauksessa emme Määritä eväste todennus-middleware, koska eväste todennus middleware voit ohjata käyttäjän "Käyttö estetty"-sivu. Käyttö estetty-sivu sijainti määritetään eväste; middleware AccessDeniedPath-vaihtoehto Katso [todennus middleware määrittäminen].

### <a name="authorize-controller-actions"></a>Määritä valvonta-toiminnot

Lopuksi sallivat MVC ohjauskoneen toiminto määrittää käytännön `Authorize` määrite:

```csharp
[Authorize(Policy = "SurveyCreatorRequirement")]
public IActionResult Create()
{
    // ...
}
```

ASP.NET aiemmissa versioissa voit määrittää **roolit** -ominaisuus-määrite:

```csharp
// old way
[Authorize(Roles = "SurveyCreator")]

```

Tämä tuetaan edelleen ASP.NET Core 1.0, mutta se on joitakin haitoista verrattuna luvan käytännöt:

-   Se olettaa tietyn ryhmän tyyppi. Varaa minkä tahansa tarkistaa käytännöt. Roolit ovat vain ryhmän tyypin.
-   Rooli on koodattu määrite. Käytäntöjä luvan logiikan on yhdessä paikassa on helpompaa päivittäminen tai jopa kuormituksen-asetukset.
-   Käytännöt käyttöön monimutkaisia luvan päätöksiä (esimerkiksi ikä > = 21), joka voidaan ilmaista yksinkertainen roolin jäsenyyden mukaan.

## <a name="resource-based-authorization"></a>Resurssien mukaan-todennus

_Resurssin pohjainen todennus_ tapahtuu, kun luvan määräytyy tietyn resurssi, joka vaikuta toiminnon. Tailspin kyselyt-sovelluksessa jokaisen kyselyn on omistaja ja nolla-moneen-osallistujat.

-   Omistaja voi lukea, päivittää, poistaa, julkaiseminen ja julkaisun tutkimuksen.
-   Omistaja määrittää osallistujat kyselyä.
-   Osallistujat voivat lukea ja päivittää kyselyn.

Huomaa, että "omistaja" ja "osallistuja" eivät ole sovelluksen roolit; ne on tallennettu kysely sovellustietokanta kohden. Jos haluat tarkistaa, onko käyttäjä voi poistaa kyselyn, esimerkiksi sovellus tarkistaa, onko käyttäjän kyseisen kyselyn omistaja.

ASP.NET Core 1.0 pantava resurssi-pohjainen todennus **AuthorizationHandler** johtuvat ja ohittaminen **käsitellä** -menetelmää.

```csharp
public class SurveyAuthorizationHandler : AuthorizationHandler<OperationAuthorizationRequirement, Survey>
{
     protected override void Handle(AuthorizationContext context, OperationAuthorizationRequirement operation, Survey resource)
    {
    }
}
```

Huomaa, että kyselyn objektien on erittäin kirjoitettu tähän luokkaan.  Rekisteröidy luokan DI käynnistyksen yhteydessä:

```csharp
services.AddSingleton<IAuthorizationHandler>(factory =>
{
    return new SurveyAuthorizationHandler();
});
```

Voit suorittaa luvan tarkistaa, käyttämällä **IAuthorizationService** -käyttöliittymä, jossa voit lisätä oman ohjaimet kyselyjä. Seuraava koodi tarkistaa, onko käyttäjä voi lukea kyselyyn:

```csharp
if (await _authorizationService.AuthorizeAsync(User, survey, Operations.Read) == false)
{
    return new HttpStatusCodeResult(403);
}
```

Koska olemme välitä `Survey` objekti, puhelun kutsuu `SurveyAuthorizationHandler`.

Todennus-koodisi hyvä tapa on kooste kaikki käyttäjän Roolipohjainen ja resurssien mukaan käyttöoikeudet ja tarkista, kooste määrittäminen vastaan halutun toiminnon.
Tässä on esimerkki kyselyt-sovelluksen. Sovellus määrittää useita käyttöoikeuksia tyypit:

- Järjestelmänvalvoja
- Avustaja
- Tekijä
- Omistaja
- Lukija

Sovellus määrittää myös tutkimusten mahdolliset toiminnot:

- Luo
- Luku
- Päivitys
- Poista
- Julkaiseminen
- Unpublsh

Seuraava koodi luo tietyn käyttäjän ja kyselyn käyttöoikeuksien luettelo. Huomaa, että tämä koodi on sekä käyttäjän app rooleja ja omistaja tai osallistuja-kentät kyselyn.

```csharp
protected override void Handle(AuthorizationContext context, OperationAuthorizationRequirement operation, Survey resource)
{
    var permissions = new List<UserPermissionType>();
    string userTenantId = context.User.GetTenantIdValue();
    int userId = ClaimsPrincipalExtensions.GetUserKey(context.User);
    string user = context.User.GetUserName();

    if (resource.TenantId == userTenantId)
    {
        // Admin can do anything, as long as the resource belongs to the admin's tenant.
        if (context.User.HasClaim(ClaimTypes.Role, Roles.SurveyAdmin))
        {
            context.Succeed(operation);
            return;
        }

        if (context.User.HasClaim(ClaimTypes.Role, Roles.SurveyCreator))
        {
            permissions.Add(UserPermissionType.Creator);
        }
        else
        {
            permissions.Add(UserPermissionType.Reader);
        }

        if (resource.OwnerId == userId)
        {
            permissions.Add(UserPermissionType.Owner);
        }
    }
    if (resource.Contributors != null && resource.Contributors.Any(x => x.UserId == userId))
    {
        permissions.Add(UserPermissionType.Contributor);
    }
    if (ValidateUserPermissions[operation](permissions))
    {
        context.Succeed(operation);
    }
}
```

> [AZURE.NOTE] Katso [SurveyAuthorizationHandler.cs].

Usean vuokraajan sovelluksessa sinun on varmistettava, että käyttöoikeudet ei "paljastaa" toiseen vuokraajan tietoihin. Kyselyt-sovelluksessa osallistuja-oikeudet sallitaan yli alihallinnat &mdash; voit määrittää jonkun toisen vuokraajasta contriubutor nimellä. Käyttöoikeus-tietotyypit on rajoitettu kyseisen käyttäjän vuokraajan kuuluvien resurssien. Tämä vaatimus ottamaan koodi tarkistaa ennen käyttöoikeuksien myöntäminen vuokraajan tunnus. ( `TenantId` Kentän varattujen kyselyn luomisen.)

Seuraavaksi voit tarkistaa toiminnon vastaan käyttöoikeudet (luku, päivitys, poista jne.). Kyselyt-sovelluksen toteuttaa tämän vaiheen hakutaulukkoa toimintoja käyttämällä:

```csharp
static readonly Dictionary<OperationAuthorizationRequirement, Func<List<UserPermissionType>, bool>> ValidateUserPermissions
    = new Dictionary<OperationAuthorizationRequirement, Func<List<UserPermissionType>, bool>>

    {
        { Operations.Create, x => x.Contains(UserPermissionType.Creator) },

        { Operations.Read, x => x.Contains(UserPermissionType.Creator) ||
                                x.Contains(UserPermissionType.Reader) ||
                                x.Contains(UserPermissionType.Contributor) ||
                                x.Contains(UserPermissionType.Owner) },

        { Operations.Update, x => x.Contains(UserPermissionType.Contributor) ||
                                x.Contains(UserPermissionType.Owner) },

        { Operations.Delete, x => x.Contains(UserPermissionType.Owner) },

        { Operations.Publish, x => x.Contains(UserPermissionType.Owner) },

        { Operations.UnPublish, x => x.Contains(UserPermissionType.Owner) }
    };
```


## <a name="next-steps"></a>Seuraavat vaiheet

- Tutustu seuraavaan artikkeliin sarjassa: [suojaaminen taustassa verkko-Ohjelmointirajapinnan multitenant-sovelluksessa][web-api]
- Lue lisää resurssien mukaan luvan ASP.NET 1.0 Core-kohdassa [Resurssin pohjainen todennus][rbac].

<!-- Links -->
[Tailspin]: guidance-multitenant-identity-tailspin.md
[sarjaan kuuluvan]: guidance-multitenant-identity.md
[Sovelluksen roolit]: guidance-multitenant-identity-app-roles.md
[policies]: https://docs.asp.net/en/latest/security/authorization/policies.html
[rbac]: https://docs.asp.net/en/latest/security/authorization/resourcebased.html
[viittaus käyttöönotto]: guidance-multitenant-identity-tailspin.md
[SurveyCreatorRequirement.cs]: https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps/blob/master/src/Tailspin.Surveys.Security/Policy/SurveyCreatorRequirement.cs
[Startup.cs]: https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps/blob/master/src/Tailspin.Surveys.Web/Startup.cs
[Todennus-middleware määrittäminen]: guidance-multitenant-identity-authenticate.md#configuring-the-authentication-middleware
[SurveyAuthorizationHandler.cs]: https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps/blob/master/src/Tailspin.Surveys.Security/Policy/SurveyAuthorizationHandler.cs
[sovelluksen malli]: https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps
[web-api]: guidance-multitenant-identity-web-api.md
