<properties
   pageTitle="Varaa-pohjaisten käyttäjätietojen multitenant sovellusten käsittelemistä | Microsoft Azure"
   description="Miten käyttöön väittää myöntäjä vahvistuksen ja todennus"
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
   ms.date="05/23/2016"
   ms.author="mwasson"/>

# <a name="working-with-claims-based-identities-in-multitenant-applications"></a>Väitepohjaista käyttäjätietojen multitenant sovellusten käyttäminen

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Tässä artikkelissa on [sarjaan kuuluvan]. On myös valmis [sovelluksen malli] , jonka mukana sarjassa.

## <a name="claims-in-azure-ad"></a>Azure AD saatavat

Kun käyttäjä kirjautuu sisään, Azure AD lähettää ID-tunnusta, joka sisältää saatavat käyttäjästä. Hakemuksen on yksinkertaisesti erilaisia tietoja, avain/arvo-pari prosentteina. Esimerkiksi `email` = `bob@contoso.com`.  Vaateita, jotka on myöntäjä &mdash; tässä tapauksessa Azure AD &mdash; eli yritys, joka todentaa käyttäjän ja luo saatavat. Luota saatavat, koska luotat myöntäjä. (Vastaavasti, jos et luota myöntäjä, Älä luota saatavat!)

Ylätasolla:

1.  Käyttäjän todentaa.
2.  IDP lähettää saatavat joukko.
3.  Sovelluksen Normalisoi tai augments saatavat (valinnainen).
4.  Sovellus käyttää saatavat luvan päätösten tekemiseen.

-OpenID yhteyden Määritä vaateita koskevat rajoitukset, jotka saat ohjataan [laajuus parametrin] todennus pyynnön. Kuitenkin Azure AD-ongelmien vianmääritys rajattu kautta OpenID yhteyden; vaateita koskevat rajoitukset Katso [Tuetut tunnuksen ja varaa tyypit]. Jos haluat lisätietoja käyttäjän, tarvitset Azure AD-kaavio-Ohjelmointirajapinnan käyttäminen.

Seuraavassa on kuvattu joitakin AAD, sovellus saattaa yleensä tärkeisiin vaatimukset:

Kirjoita ryhmän tunnus |    Kuvaus
-----------------------|--------------
aud | Kuka varten on annettu tunnus. Tämä on sovelluksen asiakkaan ID-tunnuksellasi. Ei kannata yleensä on huolissaan vaatimus, koska se tarkistaa automaattisesti middleware. Esimerkki:`"91464657-d17a-4327-91f3-2ed99386406f"`
ryhmät   | AAD ryhmät, jonka jäsen käyttäjä on luettelo. Esimerkki:`["93e8f556-8661-4955-87b6-890bc043c30f", "fc781505-18ef-4a31-a7d5-7d931d7b857e"]`
ISS  | [Myöntäjän] OIDC-tunnuksen. Esimerkki:`https://sts.windows.net/b9bd2162-77ac-4fb2-8254-5c36e9c0a9c4/`
Nimi    | Käyttäjän näyttönimi. Esimerkki:`"Alice A."`
OID | Käyttäjän AAD objektitunnus. Tämä arvo on käyttäjän pysyvä ja Uudelleenkäytettävä tunnus. Tämä arvo, ei ole sähköpostia, käyttää yksilöllinen käyttäjille. sähköpostiosoitteet saattavat muuttua. Jos Azure AD-kaavio-Ohjelmointirajapinnan käyttäminen sovelluksen Objektitunnus on käytettävä kyselyn profiilitiedot arvon. Esimerkki:`"59f9d2dc-995a-4ddf-915e-b3bb314a7fa4"`
roolit   | Sovelluksen roolit käyttäjän luettelo. Esimerkki:`["SurveyCreator"]`
TID | Vuokraajan ID-tunnuksellasi. Tämä arvo on Azure AD-vuokraajan yksilöllinen. Esimerkki:`"b9bd2162-77ac-4fb2-8254-5c36e9c0a9c4"`
unique_name | Ihmisten luettavissa näyttönimi käyttäjän. Esimerkki:`"alice@contoso.com"`
täydellisen käyttäjätunnuksen | Täydellinen käyttäjätunnus. Esimerkki:`"alice@contoso.com"`

Tässä taulukossa luetellaan varaa tyypit sellaisina kuin ne näkyvät ID-tunnusta. ASP.NET Core 1.0 OpenID yhteyden middleware muuntaa muutamia esimerkkejä varaa kun se täyttää saatavat sivustokokoelman pääasiallista käyttäjälle:

-   OID >`http://schemas.microsoft.com/identity/claims/objectidentifier`
-   TID >`http://schemas.microsoft.com/identity/claims/tenantid`
-   unique_name >`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`
-   täydellisen käyttäjätunnuksen >`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn`

## <a name="claims-transformations"></a>Väitteitä muunnoksia

Todennus-työnkulku aikana voit muokata saatavat, jonka saat IDP. ASP.NET Core 1.0 voit suorittaa OpenID yhteyden middleware saatavat muunnos sisältyy **AuthenticationValidated** tapahtuma. (Katso [todennus tapahtumien].)

Istunnon todennus eväste on tallennettu saatavat, kun lisäät **AuthenticationValidated** aikana. Hän ei Hae miten takaisin, Azure AD.

Seuraavassa on esimerkkejä saatavat muunnos:

-   **Väitteitä normalisointi**tai kaikki tarvittavat saatavat yhdenmukaisia koko käyttäjät. Tämä on erityisen merkitystä, jos vaateita, jotka ovat hankkiminen useita IDPs, joita voi käyttää eri varaa tyypit samanlaisia tietoja.
Esimerkiksi Azure AD lähettää "upn"-ryhmän, joka sisältää käyttäjän sähköposti. Muut IDPs voi lähettää "sähköposti-ryhmän. Seuraava koodi muuntaa "upn"-ryhmän "sähköposti" varaa:

    ```csharp
    var email = principal.FindFirst(ClaimTypes.Upn)?.Value;
    if (!string.IsNullOrWhiteSpace(email))
    {
      identity.AddClaim(new Claim(ClaimTypes.Email, email));
    }
    ```

- Lisää varten saatavat, joita ei ole **oletusarvoisesti väittää arvot** &mdash; määritetään esimerkiksi käyttäjän oletusarvo-rooliin. Joissakin tapauksissa tämä voidaan yksinkertaistaa luvan logiikan.
- Lisää **mukautetun ryhmän tyypit** sovelluksen kielikohtaiset käyttäjän tietoja. Esimerkiksi käyttäjän tietoja voi tallentaa tietokannan. Voit lisätä mukautetun ryhmän nämä ohjeet todennus lippu. Vaatimus tallennetaan eväste, sinun tulee hankkiminen kerran kirjautuminen istunnossa tietokannasta. Toisaalta haluat välttää luomisen kohtuuttoman suuri evästeet, niin kannattaa miettiä trade-off evästeiden koon ja tietokannan haut välillä.   

Todennus-työnkulun päätyttyä saatavat ovat käytettävissä `HttpContext.User`. Tässä vaiheessa olisi käsittele niitä kuin vain luku-sivustokokoelman &mdash; esimerkiksi niiden avulla luvan päätösten tekemiseen.

## <a name="issuer-validation"></a>Myöntäjän vahvistus
OpenID yhteyden myöntäjä vaatimus ("iss") yksilöi IDP, joka on myöntänyt ID-tunnusta. Osa OIDC todennus-työnkulku on Varmista, että myöntäjä-ryhmän vastaa todellinen myöntäjä. OIDC middleware käsittelee tämän puolestasi.

Azure AD-myöntäjä arvo on yksilöllinen AD kohden (`https://sts.windows.net/<tenantID>`). Tämän vuoksi sovelluksen pitäisi tehdä muita valintaruudun, varmista, että myöntäjä edustaa alihallintaa, johon on oikeus sovellukseen kirjautuminen.

Yhden vuokraajan-sovelluksen voit tarkistaa, että on oma Alihallinta. Itse asiassa OIDC middleware tekee tämän automaattisesti oletusarvon mukaan. Usean vuokraajan-sovellukseen sinun on sallittava useita myöntäjistä, vastaava eri vuokraajiin varten. Näin yleisen lähestymistavan käyttäminen:

-   Määritä **ValidateIssuer** OIDC middleware asetus-, EPÄTOSI. Tämä poistaa automaattinen tarkistus käytöstä.
-   Kun palvelutili Rekisteröi tallentaa käyttäjän DB vuokraajan ja myöntäjä.
-   Aina, kun käyttäjä kirjautuu sisään, voit etsiä tietokannan myöntäjä. Jos myöntäjä ei löydy, se tarkoittaa kyseisen alihallinnan ei ole rekisteröitynyt. Voit ohjata ne rekisteröinti-sivu.
-  Voi myös blacklist tiettyjen alihallinnat; esimerkiksi asiakkaat, joilla ei ole maksaa tilauksen.

Katso tarkempia keskustelun [Kirjautuminen ja vuokraaja onboarding multitenant sovelluksessa][signup].

## <a name="using-claims-for-authorization"></a>Vaateita, jotka koskevat luvan käyttäminen

Saatavat, jossa on käyttäjän tunnistetiedot ei ole enää Monoliittiset kohteen. Käyttäjä voi olla esimerkiksi sähköpostiosoitteen, puhelinnumeron, syntymäpäivä, sukupuoli ja niin edelleen. Ehkäpä käyttäjän IDP tallentaa kaikki tiedot. Mutta kun käyttäjä todennetaan, saat yleensä ne saatavat alijoukkoa. Tämän mallin käyttäjän tunnistetiedot ovat vain pikaoppaista vaateita koskevat rajoitukset. Kun teet käyttäjän luvan päätöksiä, voit näyttää tietyn joukkojen vaateita koskevat rajoitukset. Toisin sanoen kysymyksen "Käyttäjä X suorittaa toiminnon Y" kädessä tulee "X on käyttäjän väittää Z".

Seuraavassa on joitakin basic kuviot saatavat tarkistamiseen.

-  Voit tarkistaa, että käyttäjällä on tietyn ryhmän tietyn arvon seuraavasti:

    ```csharp
    if (User.HasClaim(ClaimTypes.Role, "Admin")) { ... }
    ```
    Tämä koodi tarkistaa, onko käyttäjällä on roolin vaatimus arvona "Järjestelmänvalvoja". Tapaus, johon käyttäjällä on roolin ei varaa tai useita roolin saatavat käsittelee oikein.

    **ClaimTypes** -luokka määrittää käytettyjä varaa tyypeissä vakioita. Voit kuitenkin käyttää mikä tahansa merkkijonoarvo ryhmän tyypin.

-   Yksittäisen arvon hakeminen ryhmän tyyppi, vaikka odotat palvelussa voi olla enintään yhden arvon:
    ```csharp
     string email = User.FindFirst(ClaimTypes.Email)?.Value;
    ```
-   Hakee kaikki arvot ryhmän tyyppi:

    ```csharp
     IEnumerable<Claim> groups = User.FindAll("groups");
    ```

Lisätietoja on artikkelissa [Roolipohjainen ja resurssi-pohjainen todennus multitenant sovellusten][authorization].

## <a name="next-steps"></a>Seuraavat vaiheet

- Tutustu seuraavaan artikkeliin sarjassa: [Kirjautuminen ja vuokraaja onboarding multitenant sovelluksessa][signup]
- Lue lisää [väitepohjaista luvan] ASP.NET Core 1.0-asiakirjat

<!-- Links -->
[sarjaan kuuluvan]: guidance-multitenant-identity.md
[laajuus-parametri]: http://nat.sakimura.org/2012/01/26/scopes-and-claims-in-openid-connect/
[Tuetut tunnuksen ja varaa tyypit]: ../active-directory/active-directory-token-and-claims.md
[myöntäjä]: http://openid.net/specs/openid-connect-core-1_0.html#IDToken
[Todennus-tapahtumat]: guidance-multitenant-identity-authenticate.md#authentication-events
[signup]: guidance-multitenant-identity-signup.md
[Väitepohjaisten todennus]: https://docs.asp.net/en/latest/security/authorization/claims.html
[sovelluksen malli]: https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps
[authorization]: guidance-multitenant-identity-authorize.md
