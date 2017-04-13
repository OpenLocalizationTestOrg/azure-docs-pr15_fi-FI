<properties
    pageTitle="Push-ilmoitukset lisääminen iOS-sovelluksessa Azure Mobile-sovellusten kanssa"
    description="Opettele Azure Mobile-sovellusten avulla voit lähettää push-ilmoitukset iOS-sovellukseen."
    services="app-service\mobile"
    documentationCenter="ios"
    manager="yochayk"
    editor=""
    authors="ysxu"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="yuaxu"/>


# <a name="add-push-notifications-to-your-ios-app"></a>Push-ilmoitukset lisääminen oman iOS-sovelluksessa

[AZURE.INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>Yleiskatsaus
Tässä opetusohjelmassa Lisää push-ilmoitukset [iOS nopeasti Käynnistä] projektin niin, että push-ilmoitus lähetetään laitteen aina, kun tietue lisätään.

Jos et käytä ladatut pikaopas server-projekti, sinun on push ilmoituksen tunniste-paketti. Lisätietoja on kohdassa [.NET palvelimeen Azure-mobiilisovellukset SDK käsitteleminen](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) .

[IOS simulator ei tue push-ilmoitukset](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/iOS_Simulator_Guide/TestingontheiOSSimulator.html). Tarvitset fyysinen iOS-laitteessa ja [Apple kehittäjä-ohjelman jäsenyyttä](https://developer.apple.com/programs/ios/).

##<a name="configure-hub"></a>Määritä ilmoitus-toiminnossa

[AZURE.INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <a id="register"></a>Rekisteröi-sovelluksen push-ilmoitukset

[AZURE.INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]

## <a name="configure-azure-to-send-push-notifications"></a>Azure lähettää push-ilmoitusten määrittäminen

[AZURE.INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

##<a id="update-server"></a>Päivitä Taustajärjestelmä lähettää push-ilmoitukset

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-configure-push-apns](../../includes/app-service-mobile-dotnet-backend-configure-push-apns.md)]

## <a id="add-push"></a>Push-ilmoitukset lisääminen sovellukseen

[AZURE.INCLUDE [app-service-mobile-add-push-notifications-to-ios-app.md](../../includes/app-service-mobile-add-push-notifications-to-ios-app.md)]

## <a id="test"></a>Testaa push-ilmoitukset

[AZURE.INCLUDE [Test Push Notifications in App](../../includes/test-push-notifications-in-app.md)]

##<a id="more"></a>Lisää

* Mallien avulla voit lähettää Office kaikissa ympäristöissä työntää lokalisoitu työntää joustavuutta. [Voit käyttää iOS asiakkaan kirjasto Azure Mobile-sovellusten](app-service-mobile-ios-how-to-use-client-library.md#templates) avulla voit rekisteröidä malleja.

<!-- Anchors.  -->

<!-- Images. -->

<!-- URLs. -->
[iOS-pikaopas]: app-service-mobile-ios-get-started.md
