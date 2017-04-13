<properties
    pageTitle="Luo Cordova sovelluksen Azure App palvelun Mobiilisovellusten | Microsoft Azure"
    description="Katso tämä opetusohjelma, jotta Apache Cordova kehittämiseen Azure mobiilisovelluksen backends avulla pääset alkuun"
    services="app-service\mobile"
    documentationCenter="javascript"
    authors="adrianhall"
    manager="erikre"
    editor=""
    tags=""
    keywords="cordova, javascript--mobiilisovelluksen" />

<tags
    ms.service="app-service-mobile"
    ms.workload="na"
    ms.tgt_pltfrm="mobile-html"
    ms.devlang="javascript"
    ms.topic="hero-article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

#<a name="create-an-apache-cordova-app"></a>Luo Apache Cordova app

[AZURE.INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a>Yleiskatsaus

Tässä opetusohjelmassa näytetään pilvipohjainen Taustajärjestelmä palvelun lisäämisestä Apache Cordova-mobiilisovelluksen käyttämällä Azure mobiilisovelluksen Taustajärjestelmä.  Luo uusi mobiilisovelluksen taustassa ja yksinkertainen _Todo luettelon_ Apache Cordova-sovellusta, joka tallentaa tiedot sovelluksen Azure.

Noudata tässä opetusohjelmassa on kaikki muut Apache Cordova opetusohjelmat mobiilisovellukset-toiminnon käyttämisestä Azure App palvelun.

## <a name="prerequisites"></a>Edellytykset

Tässä opetusohjelmassa suorittamiseen tarvitset seuraavat:

* Tietokoneen kanssa [Visual Studio yhteisön 2015] tai uudempi versio.
* [Visual Studio Tools for Apache Cordova].
* [Aktiivinen Azure-tili](https://azure.microsoft.com/pricing/free-trial/).

Voit myös ohittaa Visual Studio ja käytä Apache Cordova komentoriviä suoraan.  Tästä on hyötyä, kun olet suorittanut opetusohjelman Mac-tietokoneessa.  Tässä opetusohjelmassa ei kata kääntäminen Apache Cordova asiakassovelluksissa komentoriviltä.

## <a name="create-a-new-azure-mobile-app-backend"></a>Luo uusi Azure mobiilisovelluksen taustassa

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

[Katso tämä video, jossa vastaavat vaiheet](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-1-Create-an-Azure-Mobile-App)

## <a name="configure-the-server-project"></a>Määritä server-projekti

[AZURE.INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-the-apache-cordova-app"></a>Lataa ja suorita Apache Cordova-sovellus

[AZURE.INCLUDE [app-service-mobile-cordova-run-app](../../includes/app-service-mobile-cordova-run-app.md)]

## <a name="next-steps"></a>Seuraavat vaiheet

Nyt kun olet suorittanut pikaopas Tässä opetusohjelmassa, siirry jompikumpi seuraavista opetusohjelmat:

* [Lisää todennus] oman Apache Cordova sovelluksen.
* [Lisää Push-ilmoitukset] oman Apache Cordova sovelluksen.

Lisätietoja avaimen käsitteitä Azure App palvelussa.

* [Todennus]
* [Push-ilmoitukset]

Opettele käyttämään SDK: T.

* [Apache Cordova SDK-paketissa]
* [ASP.NET-palvelimen SDK-paketissa]
* [Node.js Server SDK-paketissa]

<!-- Images. -->

<!-- URLs -->
[Azure portal]: https://portal.azure.com/
[Visual Studio yhteisön 2015]: http://www.visualstudio.com/
[Visual Studio Tools for Apache Cordova]: https://www.visualstudio.com/en-us/features/cordova-vs.aspx
[Käyttöoikeuksien lisääminen]: app-service-mobile-cordova-get-started-users.md
[Lisää Push-ilmoitukset]: app-service-mobile-cordova-get-started-push.md
[Todennus]: app-service-mobile-auth.md
[Push-ilmoitukset]: ../notification-hubs/notification-hubs-push-notification-overview.md
[Apache Cordova SDK-paketissa]: app-service-mobile-cordova-how-to-use-client-library.md
[ASP.NET-palvelimen SDK-paketissa]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Node.js Server SDK-paketissa]: app-service-mobile-node-backend-how-to-use-server-sdk.md
