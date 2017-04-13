<properties
  pageTitle="Asiakas- ja Mobile-sovellusten ja Mobile Services-palveluissa SDK-versiotietojen | Azure sovelluksen-palvelu"
  description="Luettelo asiakkaan SDK: T ja yhteensopivuus server SDK-versioiden Mobile-palveluihin ja Azure Mobile-sovellusten kanssa"
  services="app-service\mobile"
  documentationCenter=""
  authors="adrianhall"
  manager="erikre"
  editor=""/>

<tags
  ms.service="app-service-mobile"
  ms.workload="mobile"
  ms.tgt_pltfrm="mobile-multiple"
  ms.devlang="dotnet"
  ms.topic="article"
  ms.date="10/01/2016"
  ms.author="adrianha"/>

# <a name="client-and-server-versioning-in-mobile-apps-and-mobile-services"></a>Asiakas- ja versiotietojen Mobile-sovellusten ja Mobile-palvelut

Uusimman version Azure Mobile-palvelut on Azure App palvelun **Mobiilisovellukset** -ominaisuus.

-Mobiilisovellukset asiakkaan ja palvelimen SDK: T alun perin perustuvat kaltaisia Mobile-palveluihin, mutta ne eivät *ole* yhteensopivia toistensa kanssa.
Toisin sanoen on käytettävä *Mobiilisovellukset* asiakkaan SDK *Mobiilisovellukset* SDK-palvelimen kanssa ja samalla tavalla kuin *Mobile-palvelut*. Tämä sopimus on voimassa erityinen otsikko käyttää asiakkaan ja palvelimen SDK: T-arvon kautta `ZUMO-API-VERSION`.

Huomautus: aina, kun tämän asiakirjan viittaa *Mobile-palveluihin* taustassa, sitä ei välttämättä tarvitse sijaittava Mobile-palvelut. On nyt mahdollista siirtää mobiilipalvelu voidaan suorittaa sovelluksen palvelun ilman koodin muutoksia, mutta palvelu yhä käyttää *Mobile-palveluihin* SDK versiot.

Saat lisätietoja sovelluksen palvelun siirtämisestä ilman muutoksia koodi on artikkelissa [artikkelista mobiilipalvelun Azure-sovelluksen palveluun].

## <a name="header-specification"></a>Otsikon määritys

Avaimen `ZUMO-API-VERSION` voidaan määrittää HTTP-otsikon tai kyselymerkkijonon. Arvo on versiomerkkijono-lomakkeen **x.y.z**.

Esimerkki:

HAE https://service.azurewebsites.net/tables/TodoItem

OTSIKOIDEN: ZUMO-API-VERSIO: 2.0.0

Kirjaa https://service.azurewebsites.net/tables/TodoItem?ZUMO-API-VERSION=2.0.0

## <a name="opting-out-of-version-checking"></a>Viestinnästä pois version tarkistaminen

Voit valita version tarkistaminen määrittämällä arvo on **Tosi** -sovelluksen määrittäminen **MS_SkipVersionCheck**ulos. Määritä tämä, että Web.config tai Azure portaalin sovelluksen asetukset-osassa.

> [AZURE.NOTE] On useita toiminnan muutokset Mobile-palveluihin ja Mobile-sovellusten, erityisesti offline-synkronoinnin, todennus ja push-ilmoitukset alueiden välillä. Vain pitäisi valita ulos version jälkeen täydellisen testauksen, varmista, että nämä muutokset eivät riko sinua sovelluksen toimintoja.

## <a name="summary-of-compatibility-for-all-versions"></a>Kaikkien versioiden yhteensopivuuden yhteenveto

Alla olevassa kaaviossa näkyy asiakas- ja kaikentyyppisillä yhteensopivuus. On luokitellaan taustassa palvelimeen, se käyttää SDK perustuvat Mobile- **palveluja** tai Mobile- **sovellukset** .

|                           | **Mobiili-palvelut** Node.js tai .NET | **Mobile-sovellukset** Node.js tai .NET |
| ----------                | -----------------------             |   ----------------              |
| [Asiakkaiden Mobile-palvelut] | Okei                                  | Virhe\*                         |
| [Mobile-sovellusten asiakkaat]     | Virhe\*                             | Okei                              |

\*Tämä voi säätää määrittämällä **MS_SkipVersionCheck**.


<!-- IMPORTANT!  The anchors for Mobile Services and Mobile Apps MUST be 1.0.0 and 2.0.0 respectively, since there is an exception error message that uses those anchors. -->

<!-- NOTE: the fwlink to this document is http://go.microsoft.com/fwlink/?LinkID=690568 -->

## <a name="1.0.0"></a>Mobile-palveluihin asiakkaan ja palvelimen

Alla olevassa taulukossa asiakkaan SDK: T ovat yhteensopivia **Mobile-palvelut**.

Huomautus: SDK: T *eivät* Mobile-palveluihin asiakas lähettää otsikon arvo `ZUMO-API-VERSION`. Jos palvelun saa tämän ylä- tai kyselymerkkijonon arvon, virhe palautetaan, ellet ole nimenomaisesti valinnut pois yllä olevien ohjeiden mukaisesti.

### <a name="MobileServicesClients"></a>*Services* -mobiilisovelluksen SDK: T

| Asiakkaan ympäristö                   | Versio                                                                   | Version otsikon arvo |
| -------------------               | ------------------------                                                  | -------------------  |
| Hallitut asiakkaan (Windows, Xamarin) | [1.3.2](https://www.nuget.org/packages/WindowsAzure.MobileServices/1.3.2) | puuttuu                  |
| iOS                               | [2.2.2](http://aka.ms/gc6fex)                                             | puuttuu                  |
| Android-laitteeseen                           | [2.0.3](https://go.microsoft.com/fwLink/?LinkID=280126)                   | puuttuu                  |
| HTML                              | [1.2.7](http://ajax.aspnetcdn.com/ajax/mobileservices/MobileServices.Web-1.2.7.min.js) | puuttuu     |

### <a name="mobile-services-server-sdks"></a>Mobiili *Services* -palvelimen SDK: T

| Server-ympäristössä  | Versio                                                                                                        | Hyväksytyt version otsikosta |
| ---------------- | ------------------------------------------------------------                                                   | ----------------------- |
| .NET             | [WindowsAzure.MobileServices.Backend.* versio 1.0.x](https://www.nuget.org/packages/WindowsAzure.MobileServices.Backend/) | **Versio ei ole otsikkoa** |
| Node.js          | (tulossa)                        | **Versio ei ole otsikkoa** |

<!-- TODO: add Node npm version -->

### <a name="behavior-of-mobile-services-backends"></a>Mobile-palveluihin backends toiminta

| ZUMO-API-VERSIO | MS_SkipVersionCheck arvo | Vastaus |
| ---------------- | ---------------------------- | -------- |
| Ei ole määritetty    | Mikä tahansa                          | 200 – OK |
| Mikä tahansa arvo        | TOSI                         | 200 – OK |
| Mikä tahansa arvo        | EPÄTOSI tai ei määritetty          | 400 – Virheellinen pyyntö |

## <a name="2.0.0"></a>Azure mobiilisovellukset asiakkaan ja palvelimen

### <a name="MobileAppsClients"></a>*Sovellukset* -mobiilisovelluksen SDK: T

Versiota, vaan otettiin käynnistetään **Azure-mobiilisovellukset**seuraavat asiakkaan SDK-versioiden kanssa:

| Asiakkaan ympäristö                   | Versio                   | Version otsikon arvo |
| -------------------               | ------------------------  | -----------------    |
| Hallitut asiakkaan (Windows, Xamarin) | [2.0.0](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/2.0.0) | 2.0.0 |
| iOS                               | [3.0.0](http://go.microsoft.com/fwlink/?LinkID=529823) | 2.0.0  |
| Android-laitteeseen                           | [3.0.0](http://go.microsoft.com/fwlink/?LinkID=717033&clcid=0x409) | 3.0.0 |

<!-- TODO: add HTML version when released -->

### <a name="mobile-apps-server-sdks"></a>Mobiili *Apps* Serverin SDK: T

Versiota, vaan sisältyy seuraavan palvelimen SDK versioita:

| Server-ympäristössä  | SDK-PAKETISSA                                                                                                        | Hyväksytyt version otsikosta |
| ---------------- | ------------------------------------------------------------                                                   | ----------------------- |
| .NET             | [Microsoft.Azure.Mobile.Server](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/) | 2.0.0 |
| Node.js          | [Azure-mobile-sovellukset)](https://www.npmjs.com/package/azure-mobile-apps)                         | 2.0.0 |

### <a name="behavior-of-mobile-apps-backends"></a>Mobile-sovellusten backends toiminta

| ZUMO-API-VERSIO | MS_SkipVersionCheck arvo | Vastaus |
| ---------------- | ---------------------------- | -------- |
| x.y.z tai Null    | TOSI                         | 200 – OK |
| Null             | EPÄTOSI tai ei määritetty          | 400 – Virheellinen pyyntö |
| 1.x.y            | EPÄTOSI tai ei määritetty          | 400 – Virheellinen pyyntö |
| 2.0.0-2.x.y      | EPÄTOSI tai ei määritetty          | 200 – OK |
| 3.0.0-3.x.y      | EPÄTOSI tai ei määritetty          | 400 – Virheellinen pyyntö |


## <a name="next-steps"></a>Seuraavat vaiheet

- [Siirtää Azure App palvelun Mobile-palvelu]


[Asiakkaiden Mobile-palvelut]: #MobileServicesClients
[Mobile-sovellusten asiakkaat]: #MobileAppsClients


[Mobile App Server SDK]: http://www.nuget.org/packages/microsoft.azure.mobile.server
[Siirtää Azure App palvelun Mobile-palvelu]: app-service-mobile-migrating-from-mobile-services.md

