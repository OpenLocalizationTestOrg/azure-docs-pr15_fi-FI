<properties
    pageTitle="Luo Azure App palvelun mobiilisovellukset Android-sovelluksen | Microsoft Azure"
    description="Katso tämä opetusohjelma, jotta Android kehittämiseen Azure mobiilisovelluksen backends avulla pääset alkuun"
    services="app-service\mobile"
    documentationCenter="android"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="na"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="hero-article"
    ms.date="10/01/2016"
    ms.author="yuaxu"/>

#<a name="create-an-android-app"></a>Android-sovelluksen luominen

[AZURE.INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a>Yleiskatsaus

Tässä opetusohjelmassa avulla voit lisätä pilvipohjainen Taustajärjestelmä-palvelun Android mobiilisovelluksen käyttämällä Azure mobiilisovelluksen Taustajärjestelmä.  Luo uusi mobiilisovelluksen taustassa ja yksinkertainen _Todo luettelo_ Android-sovelluksen, joka tallentaa tiedot sovelluksen Azure.

Noudata tässä opetusohjelmassa on muita kaikki Android opetusohjelmat mobiilisovellukset-toiminnon käyttämisestä Azure App palvelun.

## <a name="prerequisites"></a>Edellytykset

Tässä opetusohjelmassa suorittamiseen tarvitset seuraavat:

* [Android Kehitystyökalut](https://developer.android.com/sdk/index.html), joka sisältää Android Studiossa integroitua ja uusimmat Android-ympäristössä.
* Azure Mobile Android SDK-paketissa, joka on automaattisesti viitata osana lataat pikaopas projektin.
* [Aktiivinen Azure-tili](https://azure.microsoft.com/pricing/free-trial/).

## <a name="create-a-new-azure-mobile-app-backend"></a>Luo uusi Azure mobiilisovelluksen taustassa

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

## <a name="configure-the-server-project"></a>Määritä server-projekti

[AZURE.INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-the-android-app"></a>Lataa ja suorita Android-sovelluksen

[AZURE.INCLUDE [app-service-mobile-android-run-app](../../includes/app-service-mobile-android-run-app.md)]

<!-- URLs -->
[Azure portal]: https://portal.azure.com/
[Visual Studio Community 2013]: https://go.microsoft.com/fwLink/p/?LinkID=534203
