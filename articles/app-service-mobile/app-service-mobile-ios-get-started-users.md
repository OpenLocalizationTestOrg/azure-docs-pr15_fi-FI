<properties
    pageTitle="Käyttöoikeuksien lisääminen iOS Azure Mobile-sovellusten kanssa"
    description="Opettele todentaa käyttäjät iOS-sovelluksen kautta erilaisia tunnistetietojen toimittajat, mukaan lukien AAD, Google, Facebookiin, Twitteriin ja Microsoft Azure Mobile-sovellusten avulla."
    services="app-service\mobile"
    documentationCenter="ios"
    authors="ysxu"
    manager="yochayk"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="yuaxu"/>

# <a name="add-authentication-to-your-ios-app"></a>Käyttöoikeuksien lisääminen iOS-sovellukseen

[AZURE.INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

Tässä opetusohjelmassa käyttöoikeuksien lisääminen [nopeasti iOS Käynnistä] projekti käyttämällä tuetut tunnistetietojen toimittaja. Tässä opetusohjelmassa perustuu [iOS nopeasti aloittaminen] -opetusohjelma, jotka sinun täytyy tehdä.

##<a name="register"></a>Rekisteröi todennustavaksi sovelluksen ja sovelluksen kertakirjauspalvelun asetusten määrittäminen

[AZURE.INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

##<a name="permissions"></a>Todennettujen käyttäjien käyttöoikeuksien rajaamiseen

[AZURE.INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

Paina Xcode, Käynnistä sovellus **Suorita** . Poikkeuksen korotetaan, koska sovellus yrittää käyttää Todentamattomille käyttäjänä taustatietokannan, mutta _TodoItem_ taulukon nyt edellyttää käyttöoikeuden tarkistusta.

##<a name="add-authentication"></a>Käyttöoikeuksien lisääminen sovellukseen

[AZURE.INCLUDE [app-service-mobile-ios-authenticate-app](../../includes/app-service-mobile-ios-authenticate-app.md)]


<!-- URLs. -->

[iOS-pikaopas]: app-service-mobile-ios-get-started.md

[Azure portal]: https://portal.azure.com
