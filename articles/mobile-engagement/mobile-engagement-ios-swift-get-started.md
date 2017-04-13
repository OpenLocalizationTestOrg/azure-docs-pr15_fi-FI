<properties
    pageTitle="Aloittaminen Azure Mobile välitys iOS Swift | Microsoft Azure"
    description="Opettele käyttämään Azure Mobile välitys Analytics ja Push-ilmoitukset iOS sovellukset."
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="swift"
    ms.topic="hero-article"
    ms.date="09/20/2016"
    ms.author="piyushjo" />

# <a name="get-started-with-azure-mobile-engagement-for-ios-apps-in-swift"></a>Azure Mobile välitys for iOS-sovellusten Swift aloittaminen

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

Tässä ohjeaiheessa esitellään käyttämisestä Azure Mobile välitys ymmärtää sovelluksen käytön ja lähettää push-ilmoitukset Segmentoitu käyttäjille iOS-sovellukseen.
Tässä opetusohjelmassa voit luoda tyhjän iOS-sovellukseen, joka kerää perustiedot ja vastaanottaa push-ilmoitusten käyttäminen Apple Push ilmoituksen järjestelmän (APN).

Tässä opetusohjelmassa edellyttää seuraavaa:

+ XCode 8, josta voit asentaa MAC-sovelluskaupasta
+ [Mobile välitys iOS SDK-paketissa]
+ Push-ilmoitus-varmenne (.p12), voit hankkia Apple-keskihajonta keskelle

> [AZURE.NOTE] Tässä opetusohjelmassa käyttää Swift 3.0-versiota. 

Noudata tässä opetusohjelmassa on kaikki muut Mobile välitys opetusohjelmat for iOS-sovellukset.

> [AZURE.NOTE] Jotta voit suorittaa tässä opetusohjelmassa, on oltava aktiivinen Azure-tili. Jos sinulla ei ole tiliä, voit luoda ilmainen kokeiluversio tili vain muutaman minuutin. Lisätietoja on artikkelissa [Azure maksuttoman kokeiluversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-ios-swift-get-started).

##<a id="setup-azme"></a>IOS-sovelluksen asennuksen Mobile välitys

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

##<a id="connecting-app"></a>Sovelluksen yhdistäminen Mobile välitys taustaan

Tässä opetusohjelmassa näkyy "basic integrointi", joka on vähimmäismäärää, tietojen kerääminen ja Lähetä push-ilmoitus. Täydellinen integrointi asiakirjat löytyvät [Mobile välitys iOS SDK-integrointi](mobile-engagement-ios-sdk-overview.md)

Luodaan basic sovelluksen XCode osoittamaan integroinnin kanssa:

###<a name="create-a-new-ios-project"></a>IOS uuden projektin luominen

[AZURE.INCLUDE [Create a new iOS Project](../../includes/mobile-engagement-create-new-ios-app.md)]

###<a name="connect-your-app-to-mobile-engagement-backend"></a>Sovelluksen yhdistäminen Mobile välitys taustaan

1. Lataa [Mobile välitys iOS SDK-paketissa]
2. Pura. tar.gz tiedoston tietokoneesi kansioon
3. Projektin hiiren kakkospainikkeella ja valitse "Lisää tiedostoja..."

    ![][1]

4. Siirry kansioon, johon purit SDK ja valitse `EngagementSDK` kansio ja valitse OK.

    ![][2]

5. Avaa `Build Phases` välilehti ja `Link Binary With Libraries` valikosta Lisää kehysten alla kuvatulla tavalla:

    ![][3]

8. Luo Siltaus otsikko, jos haluat käyttää on SDK tavoitteen C ohjelmointirajapinnan valitsemalla Tiedosto > Uusi > Tiedosto > iOS > lähde > otsikko.

    ![][4]

9. Muokkaa näyttää nopeasti koodin Mobile välitys tavoite-C-koodi, Lisää seuraavat tuonnin väliaikaisen otsikko-tiedosto:

        /* Mobile Engagement Agent */
        #import "AEModule.h"
        #import "AEPushMessage.h"
        #import "AEStorage.h"
        #import "EngagementAgent.h"
        #import "EngagementTableViewController.h"
        #import "EngagementViewController.h"
        #import "AEUserNotificationHandler.h"
        #import "AEIdfaProvider.h"

10. Muodostamisasetukset-kohdassa Varmista, että asetus kohdassa kääntäjä Swift - koodin luominen tavoitteen C siltauksen otsikon muodosta on tämä ylätunniste polku. Seuraavassa on esimerkki polku: **$(SRCROOT)/MySuperApp/MySuperApp-Bridging-Header.h (sen mukaan, polku)**

    ![][6]

11. Siirry takaisin sinua sovelluksen *Yhteyden tietojen* sivulla Azure-portaaliin ja kopioi yhteysmerkkijono

    ![][5]

12. Liittää-yhteysmerkkijono `didFinishLaunchingWithOptions` delegoida

        func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool
        {
            [...]
                EngagementAgent.init("Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}")
            [...]
        }

##<a id="monitor"></a>Reaaliaikaisen seurannan ottaminen käyttöön

Jotta voit aloittaa lähettävät tietoja ja varmistaa, että käyttäjät ovat käytössä, voit lähettää vähintään yksi näyttö (tehtävä) Mobile välitys Taustajärjestelmä.

1. Avaa **ViewController.swift** -tiedosto ja korvaa **ViewController** **EngagementViewController**on kantaluku luokan:

    `class ViewController : EngagementViewController {`

##<a id="monitor"></a>Yhdistä app reaaliaikainen seuranta

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a id="integrate-push"></a>Push-ilmoitukset ja sovelluskohtaisesta messaging ottaminen käyttöön

Mobile välitys voit olla vuorovaikutuksessa ja SAAVUTTAA Push-ilmoitukset ja kampanjat kontekstissa viestit sovelluksen käyttäjien kanssa. Tämä moduuli kutsutaan REACH Mobile välitys-portaalissa.
Seuraavissa osissa määritys sovelluksen vastaanottamaan ne.

### <a name="enable-your-app-to-receive-silent-push-notifications"></a>Ottaa käyttöön sovelluksen hiljainen Push-ilmoitusten vastaanottotilanteiden

[AZURE.INCLUDE [mobile-engagement-ios-silent-push](../../includes/mobile-engagement-ios-silent-push.md)]

### <a name="add-the-reach-library-to-your-project"></a>Saatavilla kirjaston lisääminen projektiin

1. Napsauta hiiren kakkospainikkeella projektin
2. Valitse`Add file to ...`
3. Siirry kansioon, johon purit SDK
4. Valitse `EngagementReach` kansio
5. Valitse Lisää
6. Muokkaa väliaikaisen otsikon tiedosto näyttää Mobile välitys tavoitteen C saavuttaa otsikot ja lisää seuraavat tuonnin:

        /* Mobile Engagement Reach */
        #import "AEAnnouncementViewController.h"
        #import "AEAutorotateView.h"
        #import "AEContentViewController.h"
        #import "AEDefaultAnnouncementViewController.h"
        #import "AEDefaultNotifier.h"
        #import "AEDefaultPollViewController.h"
        #import "AEInteractiveContent.h"
        #import "AENotificationView.h"
        #import "AENotifier.h"
        #import "AEPollViewController.h"
        #import "AEReachAbstractAnnouncement.h"
        #import "AEReachAnnouncement.h"
        #import "AEReachContent.h"
        #import "AEReachDataPush.h"
        #import "AEReachDataPushDelegate.h"
        #import "AEReachModule.h"
        #import "AEReachNotifAnnouncement.h"
        #import "AEReachPoll.h"
        #import "AEReachPollQuestion.h"
        #import "AEViewControllerUtil.h"
        #import "AEWebAnnouncementJsBridge.h"

### <a name="modify-your-application-delegate"></a>Muokkaa sovelluksen edustajallasi

1. Sisään `didFinishLaunchingWithOptions` – Luo reach moduuli ja siirtää aiemmin välitys alustus viivan:

        func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool 
        {
            let reach = AEReachModule.module(withNotificationIcon: UIImage(named:"icon.png")) as! AEReachModule
            EngagementAgent.init("Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}", modulesArray:[reach])
            [...]
            return true
        }

###<a name="enable-your-app-to-receive-apns-push-notifications"></a>Ottaa käyttöön sovelluksen APN Push-ilmoitusten vastaanottotilanteiden
1. Lisää seuraava rivi, `didFinishLaunchingWithOptions` menetelmää:

        if #available(iOS 8.0, *)
        {
            if #available(iOS 10.0, *)
            {
                UNUserNotificationCenter.current().requestAuthorization(options: [.alert, .badge, .sound]) { (granted, error) in }
            }else
            {
                let settings = UIUserNotificationSettings(types: [.alert, .badge, .sound], categories: nil)
                application.registerUserNotificationSettings(settings)
            }
            application.registerForRemoteNotifications()
        }
        else
        {
            application.registerForRemoteNotifications(matching: [.alert, .badge, .sound])
        }

2. Lisää `didRegisterForRemoteNotificationsWithDeviceToken` menetelmä seuraavasti:

        func application(_ application: UIApplication, didRegisterForRemoteNotificationsWithDeviceToken deviceToken: Data) {
            EngagementAgent.shared().registerDeviceToken(deviceToken)
        }

3. Lisää `didReceiveRemoteNotification:fetchCompletionHandler:` menetelmä seuraavasti:

        func application(_ application: UIApplication, didReceiveRemoteNotification userInfo: [AnyHashable : Any], fetchCompletionHandler completionHandler: @escaping (UIBackgroundFetchResult) -> Void) {
            EngagementAgent.shared().applicationDidReceiveRemoteNotification(userInfo, fetchCompletionHandler:completionHandler)
        }

[AZURE.INCLUDE [mobile-engagement-ios-send-push-push](../../includes/mobile-engagement-ios-send-push.md)]

<!-- URLs. -->
[Mobile välitys iOS SDK-paketissa]: http://aka.ms/qk2rnj

<!-- Images. -->
[1]: ./media/mobile-engagement-ios-get-started/xcode-add-files.png
[2]: ./media/mobile-engagement-ios-get-started/xcode-select-engagement-sdk.png
[3]: ./media/mobile-engagement-ios-get-started/xcode-build-phases.png
[4]: ./media/mobile-engagement-ios-swift-get-started/add-header-file.png
[5]: ./media/mobile-engagement-ios-get-started/app-connection-info-page.png
[6]: ./media/mobile-engagement-ios-swift-get-started/add-bridging-header.png
