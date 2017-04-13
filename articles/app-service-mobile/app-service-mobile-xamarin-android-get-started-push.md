<properties
    pageTitle="Push-ilmoitukset lisääminen Xamarin.Android sovelluksen | Azure sovelluksen-palvelu"
    description="Opettele käyttämään Azure App palvelun ja Azure ilmoituksen keskittimet lähettäminen Xamarin.Android sovelluksen push-ilmoitukset"
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-android"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/12/2016"
    ms.author="yuaxu"/>

# <a name="add-push-notifications-to-your-xamarinandroid-app"></a>Push-ilmoitukset lisääminen Xamarin.Android-sovellukseen

[AZURE.INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

##<a name="overview"></a>Yleiskatsaus


Tässä opetusohjelmassa Lisää push-ilmoitukset [Xamarin.Android nopeasti Käynnistä](app-service-mobile-windows-store-dotnet-get-started.md) projektin niin, että push-ilmoitus lähetetään laitteen aina, kun tietue lisätään.

Jos et käytä ladatut pikaopas server-projekti, sinun on push ilmoituksen tunniste-paketti. Lisätietoja on kohdassa [.NET palvelimeen Azure-mobiilisovellukset SDK käsitteleminen](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) .


##<a name="prerequisites"></a>Edellytykset

Tässä opetusohjelmassa edellyttää seuraavaa:

+ Aktiivinen Google-tili. Voit rekisteröityminen [accounts.google.com](http://go.microsoft.com/fwlink/p/?LinkId=268302)Google-tiliisi.
+ [Google Cloud Messaging asiakasosa](http://components.xamarin.com/view/GCMClient/).

##<a name="configure-hub"></a>Määritä ilmoitus-toiminnossa

[AZURE.INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

##<a id="register"></a>Ota käyttöön Firebase Cloud viestintä

[AZURE.INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

##<a name="configure-azure-to-send-push-requests"></a>Määritä Azure lähettää push-pyyntöjä

[AZURE.INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push-for-firebase.md)]

##<a id="update-server"></a>Päivitä push-ilmoitukset lähetetään server-projekti

[AZURE.INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

##<a id="configure-app"></a>Asiakkaan projektin push-ilmoitusten määrittäminen

[AZURE.INCLUDE [mobile-services-xamarin-android-push-configure-project](../../includes/mobile-services-xamarin-android-push-configure-project.md)]

##<a id="add-push"></a>Push-ilmoitukset-koodin lisääminen sovelluksen

[AZURE.INCLUDE [app-service-mobile-xamarin-android-push-add-to-app](../../includes/app-service-mobile-xamarin-android-push-add-to-app.md)]

## <a name="test"></a>Sovelluksen testi push-ilmoitukset

Voit testata sovelluksen käyttämällä emulaattori virtual laitetta. On määrityksen lisävaiheet, kun käytössä emulointiohjelma.

1. Varmista, että ottaminen käyttöön tai virheenkorjaus virtual laitteessa, joka on määritetty kohde, Google-ohjelmointirajapinnan Android Virtual laitteen (AVD) Managerissa tuoteluokkaryhmien.

    ![](./media/app-service-mobile-xamarin-android-get-started-push/google-apis-avd-settings.png)

2. Google-tilin lisääminen Android-laitteen **sovelluksia**valitsemalla > **asetukset** > **Lisää tili**, noudata ohjeita.

    ![](./media/app-service-mobile-xamarin-android-get-started-push/add-google-account.png)

3. Suorita ennen kuin sovellus todolist ja lisää uuden todo kohteen. Tällä hetkellä ilmoituskuvake näkyy ilmaisinalueella. Voit avata ilmoituksen laatikko ilmoituksen koko tekstin tarkasteleminen.

    ![](./media/app-service-mobile-xamarin-android-get-started-push/android-notifications.png)


<!-- URLs. -->
[Xamarin.Android quick start]: app-service-mobile-xamarin-android-get-started.md
[Google Cloud Messaging Client Component]: http://components.xamarin.com/view/GCMClient/
[Azure Mobile Services Component]: http://components.xamarin.com/view/azure-mobile-services/
