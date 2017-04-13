<properties 
    pageTitle="API sovellusten johdanto | Microsoft Azure" 
    description="Katso, miten Azure sovelluksen-palvelun avulla voit kehittää Host (isäntä)- ja tarjoaman RESTful API." 
    services="app-service\api" 
    documentationCenter=".net" 
    authors="tdykstra" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-api" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="08/23/2016" 
    ms.author="rachelap"/>

# <a name="api-apps-overview"></a>API-sovellusten yleiskatsaus

Azure-sovelluksen Service API-sovellusten tarjouksen ominaisuuksia, jotka helpottavat kehittäminen Host (isäntä)- ja tarjoaman ohjelmointirajapinnan cloud ja paikallisen. API sovellusten saat yrityksen arvosana suojaus, Yksinkertainen käyttöoikeuksien valvonta, hybrid connectivity, automaattinen SDK luonti ja saumattomasti [Logiikan sovellukset](../app-service-logic/app-service-logic-what-are-logic-apps.md).

[Azure App palvelu](../app-service/app-service-value-prop-what-is.md) on täysin hallitun ympäristön Web-mobiili- ja integrointi skenaarioita. API-sovellukset on tarjoamia [Azure App palvelun](../app-service/app-service-value-prop-what-is.md)sovelluksen tyyppejä.

![Azure-sovelluksen palvelun sovelluksen tyypit](./media/app-service-api-apps-why-best-platform/appservicesuite.png)

## <a name="why-use-api-apps"></a>Mitä hyötyä API sovelluksia?

Seuraavassa on API-sovelluksen keskeisiä ominaisuuksia:

- **Tuo oman kuin aiemmin API-on** -ei ole muuta koodin lisääminen aiemmin API voit hyödyntää API sovellusten--koodisi käyttöön vain API-sovelluksen. Oman API millä tahansa kielen tai sovelluksen palvelua, mukaan lukien ASP.NET- ja C#, Java PHP, Node.js tai Python tukemat framework.

- **Helppo kulutus** - [Swagger API metatietojen](http://swagger.io/) integroitu tuki on oman ohjelmointirajapinnan helposti kulutettavia asiakkaiden erilaisilla.  Luo automaattisesti Asiakas-koodin lisääminen useilla kielillä, kuten C#, Java ja Javascript-ohjelmointirajapinnan. Määritä [CORS](app-service-api-cors-consume-javascript.md) helposti muuttamatta koodisi. Lisätietoja on artikkelissa [Service API sovellukset metatietojen API etsiminen ja koodin luontia varten](app-service-api-metadata.md) ja [Kulutettava sovelluksen Ohjelmointirajapinnan käyttäminen CORS JavaScript](app-service-api-cors-consume-javascript.md). 

- **Yksinkertainen käyttöoikeuksien valvonta** - API-sovelluksen suojaaminen Todentamattomille Accessista ilman muutoksia koodisi. Valmiin todennuspalvelujen suojatun ohjelmointirajapinnan käytön muiden palveluiden tai asiakkaat, joka edustaa käyttäjät. Tuetut tunnistetietojen toimittajat ovat Azure Active Directory, Facebookiin, Twitteriin, Google ja Microsoft Account. Asiakkaat voivat käyttää Active Directory käyttöoikeuksien kirjaston (ADAL) tai Mobile-sovellusten SDK. Lisätietoja on artikkelissa [todennus- ja Azure App Service API varten](app-service-api-authentication.md).

- **Visual Studio integrointi** - erillinen Työkalut Visual Studiossa nopeuttaa työmäärä luominen, käyttöönotosta, muissa, virheenkorjaus ja API sovellusten hallinta. Lisätietoja on artikkelissa [Azure SDK 2.8.1 .NET julkaisu](/blog/announcing-azure-sdk-2-8-1-for-net/).

- **Logiikan sovellusten integrointi** - Ohjelmointirajapinnan sovellukset, jotka luot voidaan kulutettu [Palvelun logiikan sovellukset](../app-service-logic/app-service-logic-what-are-logic-apps.md)mukaan.  Lisätietoja on artikkelissa [mukautetun API isännöimät logiikan sovellukset App-palvelun käyttäminen](../app-service-logic/app-service-logic-custom-hosted-api.md) ja [uusi rakenne versio 2015-08-01 – esikatselu](../app-service-logic/app-service-logic-schema-2015-08-01.md).

Lisäksi API-sovelluksen voit hyödyntää [Web Apps](../app-service-web/app-service-web-overview.md) -ja [Mobile-sovellusten](../app-service-mobile/app-service-mobile-value-prop.md)ominaisuuksia. Vastaavasti: Jos käytät verkkosovellukseen tai mobiilisovelluksessa isännöimiseen Ohjelmointirajapinta, se voi viedä API-sovellusten ominaisuuksia, kuten Swagger metatietojen asiakkaan koodin luominen ja CORS toimialueiden selaimen käytön. Vain kolme sovelluksen tyypit (API, web-mobiilisovelluksen) välinen ero on nimi ja käyttää niitä Azure-portaalissa kuvaketta.

## <a name="whats-the-difference-between-api-apps-and-azure-api-management"></a>Mitä eroa API-sovellusten ja Azure API hallinta?

API-sovellusten ja [Azure API hallinta](../api-management/api-management-key-concepts.md) on komplementtivirhefunktion palvelut:

* API hallinta on ohjelmointirajapinnan hallitsemisesta. Sijoita API hallinta edusta Ohjelmointirajapinnan näyttö ja rajoituksen käyttö, ja käsitellä, useita API yhdistäminen yhdeksi päätepiste ja niin edelleen. Hallittavan ohjelmointirajapinnan on isännöitävä missä tahansa.
* API-sovellukset on isännöinti API. Palvelu sisältää ominaisuuksia, jotka helpottavat kehittäminen ja vievää API, mutta se ei toimi, millaisia seuranta, rajoitusta, muokataan tai onko kokoamisesta kyseisen API hallinta. Jos et tarvitse API hallintaominaisuudet, voit isännöidä ohjelmointirajapinnan API-sovelluksissa käyttämättä API hallinta.

Oheinen kaavio kuvaa, joka kuvaa käytetään ohjelmointirajapinnan API-sovelluksissa ja muualla isännöity API hallinta.

![Azure API hallinta ja API-sovellukset](./media/app-service-api-apps-why-best-platform/apia-apim.png)

API hallinta ja API-sovellusten ominaisuuksia on samankaltaisia funktioita.  Esimerkiksi molemmat voit automatisoida CORS tuki. Jos käytät kahta palvelua yhdessä, käytetään API hallinta CORS jälkeen se toimii edusta API-sovelluksiin. 

## <a name="getting-started"></a>Käytön aloittaminen

Aloita API sovellusten ottamalla yksi, katso esimerkkejä koodin kumpi Framework opetusohjelman laite:

* [ASP.NET](app-service-api-dotnet-get-started.md) 
* [Node.js](app-service-api-nodejs-api-app.md) 
* [Java](app-service-api-java-api-app.md) 

Kysy kysymyksiä API-sovellukset, Aloita säikeen [API Apps-keskustelupalsta](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureAPIApps). 
