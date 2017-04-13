<properties
    pageTitle="V2.0 päätepisteen tyypit | Microsoft Azure"
    description="Sovellusten ja Azure AD-v2.0 päätepisteen tukemat skenaariot tyypit."
    services="active-directory"
    documentationCenter=""
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/30/2016"
    ms.author="dastrock"/>

# <a name="types-of-apps-for-the-v20-endpoint"></a>Tyyppisiä sovelluksia v2.0 päätepisteen
V2.0 päätepisteen tukee todennusta Moderni app arkkitehtuureihin, jotka perustuvat alan vakio protokollat [OAuth 2.0](active-directory-v2-protocols.md#oauth2-authorization-code-flow) -ja/tai [OpenID yhteyden](active-directory-v2-protocols.md#openid-connect-sign-in-flow)erilaisille.  Tämän asiakirjan kuvataan lyhyesti tiedostotyypit, voit luoda sovelluksia, riippumatta kielen tai ympäristö laite.  Se auttaa sinua ymmärtämään, ennen kuin voit [valita koodiksi](active-directory-appmodel-v2-overview.md#getting-started)korkean tason skenaarioita.

> [AZURE.NOTE]
    V2.0 päätepisteen tukee kaikkia Azure Active Directory-skenaariot ja toiminnot.  Voit selvittää, jos sinun on käytettävä v2.0 päätepisteen, lue lisätietoja [v2.0 rajoitukset](active-directory-v2-limitations.md).

## <a name="the-basics"></a>Perusteet
Jokaisen sovellusta, joka käyttää v2.0 päätepisteen on [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)rekisteröintiä.  Sovelluksen rekisteröitymisen kerääminen & muutaman arvon varaaminen sovelluksen:

- **Tunnus** , joka yksilöi sovelluksen
- **Ohjaa URI** , jonka avulla voidaan ohjata takaisin sovelluksen vastaukset
- Muutamia muita skenaarion kielikohtaiset arvoja.  Lisätietoja tarkemmin, miten voit [rekisteröidä sovelluksen](active-directory-v2-app-registration.md).

Rekisteröitynyt sovelluksen yhteydessä Azure AD lähettämällä pyynnöt Azure Active Directory-v2.0 päätepiste.  Annamme avoimen lähteen kehysten ja kirjastoja, joissa huolehtia nämä pyynnöt tietoja tai voit ottaa käyttöön käyttöoikeuksien logiikan itse työstämistä pyynnöt nämä päätepisteet mukaan:

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize
https://login.microsoftonline.com/common/oauth2/v2.0/token
```
<!-- TODO: Need a page for libraries to link to -->

## <a name="web-apps"></a>Web Apps-sovelluksista
Verkkosovelluksissa (.NET, PHP, Java, Ruby, Python, solmu ja niin edelleen), joita voi käyttää selaimen kautta voit suorittaa käyttäjän kirjautuminen [OpenID yhdistämistä](active-directory-v2-protocols.md#openid-connect-sign-in-flow)käyttämällä.  OpenID yhteyden-web-sovelluksen vastaanottaa `id_token`, suojaustunnus, joka tarkistaa käyttäjän henkilöllisyyden ja antaa käyttäjän saatavat lomakkeen tiedot:

```
// Partial raw id_token
eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6ImtyaU1QZG1Cd...

// Partial content of a decoded id_token
{
    "name": "John Smith",
    "email": "john.smith@gmail.com",
    "oid": "d9674823-dffc-4e3f-a6eb-62fe4bd48a58"
    ...
}
```

Saat tietoja kaikista tunnusten ja saatavat käytettävissä [v2.0 suojaustunnuksen viittaus](active-directory-v2-tokens.md)-ohjelmaan.

Palvelimen online-kirjautumisen todennus-työnkulku tekee korkean tason seuraavasti:

![Web App kaistat kuva](../media/active-directory-v2-flows/convergence_scenarios_webapp.png)

Vastaanotettu v2.0 päätepisteestä julkisen allekirjoitetun avaimen avulla id_token vahvistamisen riittää varmistaa käyttäjän tunnistetiedot, ja määrittää istunnon eväste, joilla voidaan tunnistaa käyttäjän sivulle pyynnöt.

Tässä skenaariossa käytössä näkyviin kokeilla jokin web app-kirjautuminen koodin mallit Microsoftin [Käytön aloittaminen](active-directory-appmodel-v2-overview.md#getting-started) -osassa.

Yksinkertainen-kirjautuminen palvelimen verkkosovellukseen myös on ehkä käyttää joitakin muita verkkopalvelun kuten REST-Ohjelmointirajapinnalla.  Tässä tapauksessa palvelimen online voi osallistua yhdistetyn OpenID Yhdistä & OAuth 2.0 vuo, käyttämällä [OAuth 2.0 luvan koodin työnkulku](active-directory-v2-protocols.md#oauth2-authorization-code-flow). Tässä skenaariossa käsitellään jäljempänä tämän [aiheen Web App-sovelluksen WebAPI käytön aloittaminen](active-directory-v2-devquickstarts-webapp-webapi-dotnet.md).

## <a name="web-apis"></a>Verkko-ohjelmointirajapinnan
Voit suojata verkkopalvelut sekä, kuten sinua sovelluksen RESTful verkko-Ohjelmointirajapinnan v2.0 päätepiste.  Sen sijaan, että id_tokens ja istunnon evästeet verkko-ohjelmointirajapinnan avulla OAuth 2.0 access_tokens suojata tietonsa ja todentaa pyynnöt.  Verkko-Ohjelmointirajapinnan soittajan liittää HTTP-pyynnön authorization-otsikko access_token:

```
GET /api/items HTTP/1.1
Host: www.mywebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6...
Accept: application/json
...
```

Verkko-Ohjelmointirajapinnan voit käyttää access_token API soittajan henkilöllisyys ja soittajan tiedot poimiminen saatavat, jotka on koodattu access_token.  Saat tietoja kaikista tunnusten ja saatavat käytettävissä [v2.0 suojaustunnuksen viittaus](active-directory-v2-tokens.md)-ohjelmaan.

Verkko-Ohjelmointirajapinnan antaa käyttäjille-ja osallistua-osallistumiseen liittyvät tiettyjen toimintojen tai tietojen esitysmuotoa mukaan paljastaa käyttöoikeudet, eli [alueet](active-directory-v2-scopes.md).  Puheluja sovelluksen hankkia oikeuden käyttöalue käyttäjän on hyväksyy alue työnkulku aikana.  Kysyy käyttäjältä lupaa ja tallennuksen käyttöoikeudet kaikki access_tokens, joka vastaanottaa verkko-Ohjelmointirajapinnan huolehtia v2.0 päätepiste.  Kaikki verkko-Ohjelmointirajapinnan tarvitsee huolehtia on se saa jokaisesta access_tokens tarkistaminen ja suorittamiseen riittävät tarkistukset.

Verkko-Ohjelmointirajapinnan saavat access_tokens kaikki tyyppisiä sovelluksia, kuten palvelimen verkkosovelluksissa, työpöydän ja mobile-sovellukset, yksittäiseltä sivulta sovellukset, palvelimen puoli daemonit ja muut verkko-ohjelmointirajapinnan.  Verkko-ohjelmointirajapinnan todennustavaksi korkean tason työnkulku on seuraavanlainen:

![Verkko-Ohjelmointirajapinnan kaistat kuva](../media/active-directory-v2-flows/convergence_scenarios_webapi.png)

Lue lisätietoja authorization_codes, refresh_tokens ja tarkat vaiheet käytön access_tokens [OAuth 2.0-protokollaa](active-directory-v2-protocols-oauth-code.md).

Lisätietoja suojatun sivuston ohjelmointirajapinnan kanssa OAuth2 access_tokens, tutustu web-ohjelmointirajapinnan MALLIKOODEJA Microsoftin [käytön aloittaminen-osassa](active-directory-appmodel-v2-overview.md#getting-started).


## <a name="mobile-and-native-apps"></a>Matkapuhelin ja alkuperäisen sovellukset
Sovellukset, jotka on asennettu laite, kuten sähköistä tai työpöydän sovellukset on usein Taustajärjestelmä services tai verkko-ohjelmointirajapinnan, joka sisältää tietoja ja suorittaa toimintoja käyttäjän puolesta.  Nämä sovellukset lisätä Kirjaudu sisään- ja luvan Taustajärjestelmä-palveluita käyttämällä [OAuth 2.0 luvan koodin työnkulku](active-directory-v2-protocols-oauth-code.md).  

Tässä työnkulussa sovellus vastaanottaa authorization_code v2.0 päätepisteen yhteydessä käyttäjien sisäänkirjautumisessa, joka edustaa sovelluksen käyttöoikeuksia, Soita Taustajärjestelmä services tällä hetkellä kirjautunut sisään käyttäjän puolesta.  Sovelluksen sitten vaihtoon authoriztion_code OAuth 2.0-access_token ja refresh_token taustalla.  Sovellus access_token avulla voit todentaa Web API-pyyntöjen ja käyttää refresh_token avulla uuden access_tokens kun vanhoja niistä vanhenevat.

![Alkuperäisen sovelluksen kaistat kuva](../media/active-directory-v2-flows/convergence_scenarios_native.png)

## <a name="single-page-apps-javascript"></a>Yksittäisen sivun apps (javascript)
Monta Moderni sovellukset on yhden sivun App (SPA) edusta ensisijaisesti kirjoitettu javascript ja usein käyttämällä kehysten esimerkiksi AngularJS Ember.js, Durandal.  Azure AD-v2.0 päätepisteen tukee nämä sovellukset käyttämällä [OAuth 2.0 implisiittinen Flow](active-directory-v2-protocols-implicit.md).

Tässä työnkulussa sovellus vastaanottaa tunnuksia v2.0 Määritä päätepisteen suoraan suorittamatta minkä tahansa Taustajärjestelmä palvelinten vaihdot.  Näin kaikki todennus logiikan ja istunnon käsittely ottaa sijoittaa kokonaisuudessaan javascript-asiakasohjelmassa suorittamatta ylimääräistä sivua Uudelleenohjaa.

![Implisiittinen työnkulku kaistat kuva](../media/active-directory-v2-flows/convergence_scenarios_implicit.png)

Tässä skenaariossa käytössä näkyviin kokeilla jokin yksittäiseltä sivulta sovelluksen MALLIKOODEJA Microsoftin [Käytön aloittaminen](active-directory-appmodel-v2-overview.md#getting-started) -osassa.

### <a name="daemonsserver-side-apps"></a>Daemonit/palvelimen puolella sovellukset
Sovellukset, jotka sisältävät pitkään prosessit tai, jotka toimivat ilman käyttäjän tavoitettavuustiedot on myös voi käyttää suojattuun resurssit, kuten verkko-ohjelmointirajapinnan.  Nämä sovellukset voi todentaa ja poistaa tunnusten käyttää sovelluksen tunnistetietojen (sijaan valtuutetun käyttäjätiedot) OAuth 2.0-asiakas tunnistetiedot työnkulku.

Tässä työnkulussa sovellus hakee tunnusten mukaan käyttäminen suoraan `/token` päätepisteen:

![Daemon App kaistat kuva](../media/active-directory-v2-flows/convergence_scenarios_daemon.png)

Kehittää daemon sovelluksen, katso Microsoftin [Käytön aloittaminen](active-directory-appmodel-v2-overview.md#getting-started) -osassa asiakas tunnistetiedot-documeenation tai [.NET otoksen sovelluksen](https://github.com/Azure-Samples/active-directory-dotnet-daemon-v2)viitata.

## <a name="current-limitations"></a>Nykyinen rajoitukset
Nämä tyyppisiä sovelluksia ei tällä hetkellä tue v2.0 päätepisteen, mutta ovat mallia.  Lisärajoituksia ja v2.0 päätepisteen rajoitukset on kuvattu [v2.0 rajoitukset artikkelissa](active-directory-v2-limitations.md).

### <a name="chained-web-apis-on-behalf-of"></a>Ketjutetut web API (Valitse-puolesta-)
Monta arkkitehtuureihin Lisää verkko-Ohjelmointirajapinnan on Soita toiseen edeltävät Web API, sekä suojattu v2.0 päätepiste.  Tässä skenaariossa yhteistä alkuperäisen asiakkaat, joilla verkko-Ohjelmointirajapinnan taustassa, joka puolestaan kutsuu Microsoft Online-palvelun, kuten Office 365: ssä tai Graph-Ohjelmointirajapinnan.

Ketjutetut verkko-Ohjelmointirajapinnan artikkelista aiotaan lisätä OAuth 2.0 Jwt haltijan tunnistetiedon myöntäminen, eli [On-Behalf-Of Flow](active-directory-v2-protocols.md#oauth2-on-behalf-of-flow).  Kuitenkin käytössä-puolesta-ja työnkulku ei ole tällä hetkellä käytössä v2.0 päätepiste.  Jos haluat nähdä, miten tämä työnkulku toimii yleisesti saatavilla Azure AD-palvelua, tutustu [koodi--puolesta-, malli-GitHub](https://github.com/AzureADSamples/WebAPI-OnBehalfOf-DotNet).
