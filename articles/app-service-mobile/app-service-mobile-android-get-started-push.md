<properties
    pageTitle="Lisää Android-sovelluksessa, jossa Azure Mobiilisovellusten Push-ilmoitukset"
    description="Opi käyttämään Azure-mobiilisovellukset Android-sovelluksen push-ilmoitukset lähetetään."
    services="app-service\mobile"
    documentationCenter="android"
    manager="erikre"
    editor=""
    authors="ysxu"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="10/12/2016"
    ms.author="yuaxu"/>

# <a name="add-push-notifications-to-your-android-app"></a>Lisää Android-sovelluksen Push-ilmoitukset

[AZURE.INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>Yleiskatsaus
Tässä opetusohjelmassa Lisää push-ilmoitukset [Android pikaopas] projektin niin, että push-ilmoitus lähetetään laitteen aina, kun tietue lisätään.

Jos et käytä ladatut pikaopas server-projekti, sinun on push ilmoituksen tunniste-paketti. Lisätietoja on aiheessa [.NET palvelimeen Azure-mobiilisovellukset SDK käsitteleminen](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

## <a name="prerequisites"></a>Edellytykset

Tarvitset seuraavat:

* IDE projektin Taustajärjestelmä mukaan:

    * Jos sovellus on Node.js taustassa [Android Studio](https://developer.android.com/sdk/index.html) .

    * [Visual Studio yhteisön 2013](https://go.microsoft.com/fwLink/p/?LinkID=391934) tai uudempi versio, jos sovellus on .net taustassa.

* Android 2.3 tai uudempi versio, Google säilöön version 27 tai uudempi versio ja Google Play-palveluiden 9.0.2 vähintään Firebase Cloud viestiliikenteen.

* Suorita [Android pika-aloitusopas].

## <a name="create-a-project-that-supports-firebase-cloud-messaging"></a>Luo projekti, joka tukee Firebase Cloud viestintä

[AZURE.INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

## <a name="configure-a-notification-hub"></a>Määritä ilmoitus-toiminnossa

[AZURE.INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <a name="configure-azure-to-send-push-notifications"></a>Azure lähettää push-ilmoitusten määrittäminen

[AZURE.INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push-for-firebase.md)]

## <a name="enable-push-notifications-for-the-server-project"></a>Server project push-ilmoitusten ottaminen käyttöön

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-configure-push-google](../../includes/app-service-mobile-dotnet-backend-configure-push-google.md)]

## <a name="add-push-notifications-to-your-app"></a>Sovelluksen lisääminen push-ilmoitukset

Tässä osassa Päivitä asiakkaan Android sovelluksen käsittelemään push-ilmoitukset.

### <a name="verify-android-sdk-version"></a>Android SDK-Version tarkastaminen

[AZURE.INCLUDE [app-service-mobile-verify-android-sdk-version](../../includes/app-service-mobile-verify-android-sdk-version.md)]

Seuraava vaihe on asentaa Google Play-palveluja. Google Cloud viestintä on joitakin API tason vähimmäisvaatimukset kehittäminen ja testaus, jossa luettelo **minSdkVersion** -ominaisuus on oltava.

Jos testaat vanhempia laitteen kanssa, ota yhteyttä [Määrittäminen määrittäminen Google Play Services SDK] voit selvittää, kuinka pieni voit Aseta arvoksi ja määritetty oikein.

### <a name="add-google-play-services-to-the-project"></a>Voit lisätä projektiin Google Play-palvelut

[AZURE.INCLUDE [Add Play Services](../../includes/app-service-mobile-add-google-play-services.md)]

### <a name="add-code"></a>Koodin lisääminen

[AZURE.INCLUDE [app-service-mobile-android-getting-started-with-push](../../includes/app-service-mobile-android-getting-started-with-push.md)]

## <a name="test-the-app-against-the-published-mobile-service"></a>Testaa sovellusta julkaistun mobiilipalvelu

Voit testata sovelluksen liittämällä suoraan USB-kaapeli Android-puhelimeen tai käyttämällä virtual laitteen emulaattori.

## <a name="more"></a>Lisää

<!-- URLs -->
[Android-pikaopas]: app-service-mobile-android-get-started.md

[Google Play-palveluiden SDK määrittäminen]:https://developers.google.com/android/guides/setup
