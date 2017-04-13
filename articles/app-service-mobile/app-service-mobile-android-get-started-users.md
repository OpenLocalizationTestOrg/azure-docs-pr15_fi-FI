<properties
    pageTitle="Käyttöoikeuksien lisääminen Mobile-sovellusten kanssa Android | Azure sovelluksen-palvelu"
    description="Opettele todentaa käyttäjät Android-sovelluksen kautta erilaisia tunnistetietojen toimittajat, mukaan lukien Google, Facebookiin, Twitteriin ja Microsoft Azure App palvelun Mobile-sovellusten avulla."
    services="app-service\mobile"
    documentationCenter="android"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="yuaxu"/>

# <a name="add-authentication-to-your-android-app"></a>Käyttöoikeuksien lisääminen Android-sovellukseen

[AZURE.INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

## <a name="summary"></a>Yhteenveto

Tässä opetusohjelmassa käyttöoikeuksien lisääminen todolist pikaopas projektin käyttämiseen Android tuetut tunnistetietojen toimittaja. Tässä opetusohjelmassa perustuu [Mobile-sovellusten käytön aloittaminen] -opetusohjelma, jotka sinun täytyy tehdä.

##<a name="register"></a>Rekisteröi todennustavaksi sovelluksen ja sovelluksen kertakirjauspalvelun asetusten määrittäminen

[AZURE.INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

##<a name="permissions"></a>Todennettujen käyttäjien käyttöoikeuksien rajaamiseen

[AZURE.INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

+ Avaa Android Studio arvioidut olet suorittanut projektin [Mobile-sovellusten käytön aloittaminen]-opetusohjelman. **Suorita** -valikosta valitsemalla **Suorita sovellus** ja varmista, että 401 (ei oikeuksia), tilakoodi käsittelemättömän poikkeuksen käynnistyy, kun sovellus käynnistyy.

     Tätä poikkeusta tapahtuu, koska sovellus yrittää käyttää Todentamattomille käyttäjänä taustatietokannan, mutta _TodoItem_ taulukon nyt edellyttää käyttöoikeuden tarkistusta.

Seuraavaksi voit päivittää sovelluksen todentaa käyttäjät ennen pyytää resurssien Mobile-sovelluksen taustasta.

## <a name="add-authentication-to-the-app"></a>Käyttöoikeuksien lisääminen sovellukseen

[AZURE.INCLUDE [mobile-android-authenticate-app](../../includes/mobile-android-authenticate-app.md)]

## <a name="cache-tokens"></a>Välimuistin todennus tunnusten työasemassa

[AZURE.INCLUDE [mobile-android-authenticate-app-with-token](../../includes/mobile-android-authenticate-app-with-token.md)]

##<a name="next-steps"></a>Seuraavat vaiheet

Nyt kun olet suorittanut perustodentamista Tässä opetusohjelmassa, harkitse jatkuvaa voin jompikumpi seuraavista opetusohjelmat:

+ [Lisää push-ilmoitukset Android-sovellukseen](app-service-mobile-android-get-started-push.md) Opettele määrittämään Mobile-sovelluksen-taustatietokannan push-ilmoitukset lähetetään Azure ilmoituksen keskittimet avulla.

+ [Android-sovelluksen offline-synkronoinnin ottaminen käyttöön](app-service-mobile-android-get-started-offline-data.md) Opettele lisäämään offline-tukea sovelluksen taustassa Mobile-sovelluksen avulla. Offline-synkronoinnin avulla käyttäjät voivat käsitellä mobiilisovelluksessa&mdash;tarkasteleminen, lisääminen tai muokkaaminen tietojen&mdash;myös silloin, kun yhteyttä ei ole verkossa.



<!-- Anchors. -->
[Register your app for authentication and configure Mobile Services]: #register
[Restrict table permissions to authenticated users]: #permissions
[Add authentication to the app]: #add-authentication
[Store authentication tokens on the client]: #cache-tokens
[Refresh expired tokens]: #refresh-tokens
[Next Steps]:#next-steps


<!-- URLs. -->
[Mobile-sovellusten käytön aloittaminen]: app-service-mobile-android-get-started.md
