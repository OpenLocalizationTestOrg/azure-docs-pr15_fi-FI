<properties
    pageTitle="Azure Mobile välitys iOS SDK päivittäminen toimintosarja | Microsoft Azure"
    description="Uusimmat päivitykset ja ohjeet iOS SDK Azure Mobile välitys"
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
    ms.topic="article"
    ms.date="09/14/2016"
    ms.author="piyushjo" />

#<a name="upgrade-procedures"></a>Päivityksen ohjeita

Jos valmiiksi välitys vanhempi versio on asennettu sovellukseen, sinun on ottaa huomioon seuraavat seikat, kun päivitetään SDK.

Kunkin uuden version SDK, sinun on ensin korvattava (Poista ja tuo uudelleen-xcode) EngagementSDK ja EngagementReach-kansioissa.

##<a name="from-300-to-400"></a>Valitse 3.0.0 4.0.0

### <a name="xcode-8"></a>XCode 8
XCode 8 on pakollinen version 4.0.0 SDK lähtien.

> [AZURE.NOTE] Jos todella XCode 7 määräytyvät voi käyttää [iOS välitys SDK v3.2.4](https://aka.ms/r6oouh). Esitettävän iOS 10-laitteisiin saatavilla moduuli tätä aiemman version ei tunnetut ohjelmavirhe: järjestelmäilmoitukset eivät ole actioned. Voit korjata ongelman Toteuta poistetuista Ohjelmointirajapinnan on `application:didReceiveRemoteNotification:` sovelluksen delegoida seuraavasti:

    - (void)application:(UIApplication*)application
    didReceiveRemoteNotification:(NSDictionary*)userInfo
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:nil];
    }

> [AZURE.IMPORTANT] Kuin tätä toimintoa **ei ole suositeltavaa tämä Vaihtoehtoinen menetelmä** voit muuttaa minkä tahansa tulevista (myös vähäisiä) iOS-version päivittämisen koska tämä iOS API on poistettu. Sinun kannattaa siirtyä XCode 8 mahdollisimman pian.

### <a name="usernotifications-framework"></a>UserNotifications framework
Sinun on lisättävä `UserNotifications` framework luominen-vaiheessa.

Avaa project-ruutu ja valitse oikea kohde project explorer. Avaa **"Muodosta vaiheiden"** -välilehti ja **"binaarinen kanssa kirjastojen"** -valikosta Lisää framework `UserNotifications.framework` -linkki kuin määrittäminen`Optional`

### <a name="application-push-capability"></a>Sovelluksen push-ominaisuus
XCode 8 voi palauttaa sovelluksen push-ominaisuutta, kaksinkertainen Tarkista se `capability` valittu kohde-välilehti.

### <a name="add-the-new-ios-10-notification-registration-code"></a>Lisää uusi iOS 10 ilmoituksen rekisteröintikoodi
Vanhat koodikatkelman rekisteröidä ilmoitukset sovelluksen edelleen toimii, mutta se on käytössä poistetuista ohjelmointirajapinnan esitettävän iOS 10.

Tuo `User Notification` framework:

        #import <UserNotifications/UserNotifications.h> 

-Sovelluksen edustajallasi `application:didFinishLaunchingWithOptions` menetelmä korvaa:

    if ([application respondsToSelector:@selector(registerUserNotificationSettings:)]) {
        [application registerUserNotificationSettings:[UIUserNotificationSettings settingsForTypes:(UIUserNotificationTypeBadge | UIUserNotificationTypeSound | UIUserNotificationTypeAlert) categories:nil]];
        [application registerForRemoteNotifications];
    }
    else {

        [application registerForRemoteNotificationTypes:(UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeAlert)];
    }

mukaan:

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

### <a name="if-you-already-have-your-own-unusernotificationcenterdelegate-implementation"></a>Jos sinulla on jo oma UNUserNotificationCenterDelegate käyttöönotto

SDK on myös omassa soveltaminen UNUserNotificationCenterDelegate-protokollaa. Se käyttää SDK seurannassa elinkaaren välitys ilmoitukset-laitteet, joissa iOS 10 tai uudempi. Jos SDK havaitsee edustajallasi se ei käytä omassa käyttöönoton, koska voi olla vain yksi UNUserNotificationCenter edustajan sovellusta kohden. Tämä tarkoittaa, että pystyt välitys logiikan lisääminen Omat edustajalle.

Tätä kahdella eri tavalla.

Vain toimittamalla edustajallasi soittaa SDK:

    #import <UIKit/UIKit.h>
    #import "EngagementAgent.h"
    #import <UserNotifications/UserNotifications.h>


    @interface MyAppDelegate : NSObject <UIApplicationDelegate, UNUserNotificationCenterDelegate>
    @end

    @implementation MyAppDelegate

    - (void)userNotificationCenter:(UNUserNotificationCenter *)center willPresentNotification:(UNNotification *)notification withCompletionHandler:(void (^)(UNNotificationPresentationOptions options))completionHandler
    {
      // Your own logic.

      [[EngagementAgent shared] userNotificationCenterWillPresentNotification:notification withCompletionHandler:completionHandler]
    }

    - (void)userNotificationCenter:(UNUserNotificationCenter *)center didReceiveNotificationResponse:(UNNotificationResponse *)response withCompletionHandler:(void(^)())completionHandler
    {
      // Your own logic.

      [[EngagementAgent shared] userNotificationCenterDidReceiveNotificationResponse:response withCompletionHandler:completionHandler]
    }
    @end

Tai periminen `AEUserNotificationHandler` luokan

    #import "AEUserNotificationHandler.h"
    #import "EngagementAgent.h"

    @interface CustomUserNotificationHandler :AEUserNotificationHandler
    @end

    @implementation CustomUserNotificationHandler

    - (void)userNotificationCenter:(UNUserNotificationCenter *)center willPresentNotification:(UNNotification *)notification withCompletionHandler:(void (^)(UNNotificationPresentationOptions options))completionHandler
    {
      // Your own logic.

      [super userNotificationCenter:center willPresentNotification:notification withCompletionHandler:completionHandler];
    }

    - (void)userNotificationCenter:(UNUserNotificationCenter *)center didReceiveNotificationResponse: UNNotificationResponse *)response withCompletionHandler:(void(^)())completionHandler
    {
      // Your own logic.

      [super userNotificationCenter:center didReceiveNotificationResponse:response withCompletionHandler:completionHandler];
    }

    @end

> [AZURE.NOTE] Voit määrittää ilmoituksen, joka sisältää välitys tai ei välittämällä sen `userInfo` agentti sanaston `isEngagementPushPayload:` luokan menetelmää.

##<a name="from-200-to-300"></a>Valitse 2.0.0 3.0.0
Pudottaminen tuki iOS 4.X. Sovelluksen käyttöönotto kohde lähtien tämän version on oltava vähintään iOS 6.

Jos käytät Reach sovelluksessa, sinun on lisättävä `remote-notification` arvo `UIBackgroundModes` Info.plist-tiedostossa, jotta he saavat ilmoituksia remote matriisi.

Menetelmä `application:didReceiveRemoteNotification:` on korvataan `application:didReceiveRemoteNotification:fetchCompletionHandler:` -sovelluksen edustajallasi.

"AEPushDelegate.h" on poistettu käyttöliittymässä ja sinun on poistettava kaikki viittaukset. Tämä vaihtoehto sisältää poistaminen `[[EngagementAgent shared] setPushDelegate:self]` ja -sovelluksen edustajallasi edustajan menetelmistä:

    -(void)willRetrieveLaunchMessage;
    -(void)didFailToRetrieveLaunchMessage;
    -(void)didReceiveLaunchMessage:(AEPushMessage*)launchMessage;

##<a name="from-1160-to-200"></a>Valitse 1.16.0 2.0.0
Seuraavassa kerrotaan, miten SDK-integroinnin siirtäminen tarjoaman Capptain SAS sovellukseen Azure Mobile välitys tarjoaa Capptain palvelun.
Jos olet siirtymässä aiemmasta versiosta, tilanteeseesi Capptain sivuston siirtäminen 1.16 ensin sitten Käytä seuraavasti.

>[AZURE.IMPORTANT] Capptain ja Mobile välitys eivät ole samoja palveluja ja alla kuvatulla tavalla korostaa vain siirtäminen asiakas-sovellus. Siirtyminen SDK-sovelluksessa ei siirretä tietojen Capptain-palvelimien Mobile välitys-palvelimiin

### <a name="agent"></a>Agentti

Menetelmä `registerApp:` on korvattu uusi menetelmä `init:`. Sovelluksen edustajallasi on päivitettävä vastaavasti ja käyttää yhteysmerkkijonon seuraavasti:

            - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
            {
              [...]
              [EngagementAgent init:@"YOUR_CONNECTION_STRING"];
              [...]
            }

Sinulla on poistettava kaikki esiintymät SDK on poistettu SmartAd seuranta `AETrackModule` luokan

### <a name="class-name-changes"></a>Luokan Nimenmuutokset

Osana korostetusti huomioitava muutamia luokkatiedosto/nimiä, jotka on muutettava.

"Au" etuliite "CP" etuliite kaikissa luokissa on nimetty uudelleen.

Esimerkki:

-   `CPModule.h`nimetään uudelleen `AEModule.h`.

"Välitys" etuliite "Capptain" etuliite kaikissa luokissa on nimetty uudelleen.

Esimerkkejä:

-   Luokan `CapptainAgent` nimetään uudelleen `EngagementAgent`.
-   Luokan `CapptainTableViewController` nimetään uudelleen `EngagementTableViewController`.
-   Luokan `CapptainUtils` nimetään uudelleen `EngagementUtils`.
-   Luokan `CapptainViewController` nimetään uudelleen `EngagementViewController`.
