<properties
    pageTitle="Aloittaminen Azure Mobile välitys iOS tavoitteen c | Microsoft Azure"
    description="Opettele käyttämään Azure Mobile välitys iOS-sovellusten kanssa analyysin ja push-ilmoitukset."
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="hero-article"
    ms.date="10/05/2016"
    ms.author="piyushjo" />

# <a name="get-started-with-azure-mobile-engagement-for-ios-apps-in-objective-c"></a>IOS-sovellusten tavoitteen c Azure Mobile välitys aloittaminen

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

Tässä ohjeaiheessa esitellään käyttämisestä Azure Mobile välitys ymmärtää sovelluksen käytön ja lähettää push-ilmoitukset Segmentoitu käyttäjille iOS-sovellukseen.
Tässä opetusohjelmassa voit luoda tyhjän iOS-sovellukseen, joka kerää perustiedot ja vastaanottaa push-ilmoitusten käyttäminen Apple Push ilmoituksen järjestelmän (APN).

Tässä opetusohjelmassa edellyttää seuraavaa:

+ XCode 8, josta voit asentaa MAC-sovelluskaupasta
+ [Mobile välitys iOS SDK-paketissa]

Noudata tässä opetusohjelmassa on kaikki muut Mobile välitys opetusohjelmat for iOS-sovellukset.

> [AZURE.NOTE] Jotta voit suorittaa tässä opetusohjelmassa, on oltava aktiivinen Azure-tili. Jos sinulla ei ole tiliä, voit luoda ilmainen kokeiluversio tili vain muutaman minuutin. Lisätietoja on artikkelissa [Azure maksuttoman kokeiluversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-ios-get-started).

##<a id="setup-azme"></a>IOS-sovelluksen asennuksen Mobile välitys

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

##<a id="connecting-app"></a>Sovelluksen yhdistäminen Mobile välitys taustaan

Tässä opetusohjelmassa näkyy "basic integrointi", joka on vähimmäismäärää, tietojen kerääminen ja Lähetä push-ilmoitus. Täydellinen integrointi asiakirjat löytyvät [Mobile välitys iOS SDK-integrointi](mobile-engagement-ios-sdk-overview.md)

Olemme luodaan basic sovelluksen XCode osoittamaan integrointi.

###<a name="create-a-new-ios-project"></a>IOS uuden projektin luominen

[AZURE.INCLUDE [Create a new iOS Project](../../includes/mobile-engagement-create-new-ios-app.md)]

###<a name="connect-your-app-to-the-mobile-engagement-backend"></a>Sovelluksen yhdistäminen Mobile välitys taustaan

1. Lataa [Mobile välitys iOS SDK].
2. Pura. tar.gz tiedosto tietokoneessa olevaan kansioon.
3. Projektin hiiren kakkospainikkeella ja valitse sitten **Lisää tiedostoja**.

    ![][1]

4. Siirry kansioon, johon purit SDK-paketissa, valitse `EngagementSDK` kansio ja paina sitten **OK**.

    ![][2]

5. Avaa **Luo jaksot** -välilehti ja lisää **Linkki binaarinen kanssa kirjastot** -valikossa kehysten, alla kuvatulla tavalla:

    ![][3]

6. Siirry takaisin sinua sovelluksen **Yhteyden tietojen** sivulla Azure-portaaliin ja kopioi yhteysmerkkijono.

    ![][4]

7. Lisää seuraava rivi koodin **AppDelegate.m** -tiedostossa.

        #import "EngagementAgent.h"

8. Liittää-yhteysmerkkijono `didFinishLaunchingWithOptions` edustajalle.

        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
        {
            [...]   
            [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}"];
            [...]
        }

9. `setTestLogEnabled`on valinnainen lausekkeen, joka mahdollistaa SDK lokit tunnistamista ongelmat. 

##<a id="monitor"></a>Ota reaaliaikainen seuranta

Jotta voit aloittaa lähettävät tietoja ja varmistaa, että käyttäjät ovat käytössä, voit lähettää vähintään yksi näyttö (tehtävä) Mobile välitys Taustajärjestelmä.

1. Avaa **ViewController.h** -tiedosto ja tuo **EngagementViewController.h**:

    `# import "EngagementViewController.h"`

2. Korvaa nyt erittäin luokan mukaan **ViewController** -liittymän `EngagementViewController`:

    `@interface ViewController : EngagementViewController`

##<a id="monitor"></a>Yhdistä app reaaliaikainen seuranta

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

##<a id="integrate-push"></a>Ota käyttöön push-ilmoitukset ja sovelluskohtaisesta viestintä

Mobile välitys avulla voit käsitellä käyttäjille ja SAAVUTTAA push-ilmoitukset ja sovelluksen messaging Kampanjat kontekstissa. Tämä moduuli kutsutaan REACH Mobile välitys-portaalissa.
Seuraavissa osissa on määritetty vastaanottamaan ne sovelluksen.

### <a name="enable-your-app-to-receive-silent-push-notifications"></a>Ottaa käyttöön sovelluksen hiljainen Push-ilmoitusten vastaanottotilanteiden

[AZURE.INCLUDE [mobile-engagement-ios-silent-push](../../includes/mobile-engagement-ios-silent-push.md)]  

### <a name="add-the-reach-library-to-your-project"></a>Saatavilla kirjaston lisääminen projektiin

1. Napsauta hiiren kakkospainikkeella projektin.
2. Valitse **Lisää tiedosto**.
3. Siirry kansioon, johon purit SDK.
4. Valitse `EngagementReach` kansio.
5. Valitse **Lisää**.

### <a name="modify-your-application-delegate"></a>Muokkaa sovelluksen edustajallasi

1. Takaisin sisään **AppDeletegate.m** tiedosto Tuo välitys saavuttaa moduuli.

        #import "AEReachModule.h"
        #import <UserNotifications/UserNotifications.h>

2. Sisään `application:didFinishLaunchingWithOptions` -menetelmä luo Reach moduuli ja siirtää aiemmin välitys alustus viivan:

        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
            AEReachModule * reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
            [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}" modules:reach, nil];
            [...]
            return YES;
        }

###<a name="enable-your-app-to-receive-apns-push-notifications"></a>Ottaa käyttöön sovelluksen APN Push-ilmoitusten vastaanottotilanteiden

1. Lisää seuraava rivi, `application:didFinishLaunchingWithOptions` menetelmää:

        if (NSFoundationVersionNumber >= NSFoundationVersionNumber_iOS_8_0)
        {
            if (NSFoundationVersionNumber > NSFoundationVersionNumber_iOS_9_x_Max)
            {
                [UNUserNotificationCenter.currentNotificationCenter requestAuthorizationWithOptions:(UNAuthorizationOptionBadge | UNAuthorizationOptionSound | UNAuthorizationOptionAlert) completionHandler:^(BOOL granted, NSError * _Nullable error) {}];
            }else
            {
                [application registerUserNotificationSettings:[UIUserNotificationSettings settingsForTypes:(UIUserNotificationTypeBadge | UIUserNotificationTypeSound | UIUserNotificationTypeAlert)   categories:nil]];
            }
            [application registerForRemoteNotifications];
        }
        else
        {
            [application registerForRemoteNotificationTypes:(UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeAlert)];
        }

2. Lisää `application:didRegisterForRemoteNotificationsWithDeviceToken` menetelmä seuraavasti:

        - (void)application:(UIApplication *)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData *)deviceToken
        {
            [[EngagementAgent shared] registerDeviceToken:deviceToken];
            NSLog(@"Registered Token: %@", deviceToken);
        }

3. Lisää `didFailToRegisterForRemoteNotificationsWithError` menetelmä seuraavasti:

        - (void)application:(UIApplication*)application didFailToRegisterForRemoteNotificationsWithError:(NSError*)error
        {
           
           NSLog(@"Failed to get token, error: %@", error);
        }

4. Lisää `didReceiveRemoteNotification:fetchCompletionHandler` menetelmä seuraavasti:

        - (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult result))handler
        {
            [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:handler];
        }

[AZURE.INCLUDE [mobile-engagement-ios-send-push-push](../../includes/mobile-engagement-ios-send-push.md)]

<!-- URLs. -->
[Mobile välitys iOS SDK-paketissa]: http://aka.ms/qk2rnj

<!-- Images. -->
[1]: ./media/mobile-engagement-ios-get-started/xcode-add-files.png
[2]: ./media/mobile-engagement-ios-get-started/xcode-select-engagement-sdk.png
[3]: ./media/mobile-engagement-ios-get-started/xcode-build-phases.png
[4]: ./media/mobile-engagement-ios-get-started/app-connection-info-page.png

