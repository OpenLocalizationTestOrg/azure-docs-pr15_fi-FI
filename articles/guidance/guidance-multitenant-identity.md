<properties
   pageTitle="Tunnistetietojen hallinta Multitenant sovellusten | Microsoft Azure"
   description="Parhaat käytännöt multitenant sovelluksissa todennus, todennus ja tunnistetietojen hallinta."
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

# <a name="identity-management-for-multitenant-applications-in-microsoft-azure"></a>Microsoft Azure multitenant sovelluksille jäsenyyksien hallinta

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Sarjassa on kuvattu parhaat käytännöt multitenancy, käytettäessä Azure AD todennuksen ja tunnistetiedot hallintaa varten.

Kun luot multitenant sovelluksen, yksi ensimmäisen haasteita hallinnoidaan käyttäjätietojen, koska nyt jokainen käyttäjä kuuluu vuokraajalle. Esimerkki:

- Käyttäjät kirjautua sisään organisaation tunnistetietoja.
- Käyttäjillä on pääsy organisaation tiedot, mutta ei tietoja, joihin kuuluu muiden omistajien.
- Organisaatio voi sovelluksen rekisteröityminen ja määritä sitten sen jäsenille sovelluksen roolit.

Azure Active Directory (Azure AD) on joitakin hyvien ominaisuuksia, jotka tukevat kaikkia tilanteista.

Mukana tämä artikkelisarja luomaamme valmiina, [lopusta loppuun-käyttöönoton] [ tailspin] multitenant sovellusta. Artikkelit vastaavat olemme asiat parhaillaan sovelluksen luomiseen. Aloita sovelluksen, katso [Kyselyt-sovelluksen suorittaminen](https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps/blob/master/docs/running-the-app.md).

Seuraavassa on luettelo artikkeleja:

- [Johdanto multitenant sovellusten jäsenyyksien hallinta](guidance-multitenant-identity-intro.md)
- [Tailspin kyselyt-sovellusta](guidance-multitenant-identity-tailspin.md)
- [Todennus käyttämällä Azure AD ja OpenID Yhdistä](guidance-multitenant-identity-authenticate.md)
- [Varaa-pohjaisten käyttäjätietojen käsittelemisestä](guidance-multitenant-identity-claims.md)
- [Kirjautuminen ja vuokraaja onboarding](guidance-multitenant-identity-signup.md)
- [Sovelluksen roolit](guidance-multitenant-identity-app-roles.md)
- [Roolipohjainen ja resurssi-pohjainen todennus](guidance-multitenant-identity-authorize.md)
- [Taustajärjestelmä verkko-Ohjelmointirajapinnan suojaaminen](guidance-multitenant-identity-web-api.md)
- [Välimuistin OAuth2 access tunnusten](guidance-multitenant-identity-token-cache.md)
- [Asiakkaan AD FS multitenant sovellusten Azure-tietokannassa sisällytetyistä](guidance-multitenant-identity-adfs.md)
- [Access-tunnusten saaminen Azure AD asiakkaan vahvistus avulla](guidance-multitenant-identity-client-assertion.md)
- [Suojaa sovelluksen tietoja avaimen säilö avulla](guidance-multitenant-identity-keyvault.md)

[tailspin]: https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps
