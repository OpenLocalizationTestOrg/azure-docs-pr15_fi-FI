<properties
    pageTitle="Azure Mobile välitys iOS SDK yleiskatsaus | Microsoft Azure"
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

#<a name="ios-sdk-for-azure-mobile-engagement"></a>iOS SDK Azure Mobile välitys

Saat kaikki tiedot siitä, miten voit integroida Azure Mobile välitys iOS-sovelluksen, Aloita tästä. Jos haluat Kokeile ensin, varmista, että voit tehdä tämän [15 minuutin opetusohjelma](mobile-engagement-ios-get-started.md).

Napsauttamalla saat näkyviin [SDK sisältö](mobile-engagement-ios-sdk-content.md)

##<a name="integration-procedures"></a>Integrointi ohjeita
1. Aloita tästä: [miten voit integroida Mobile välitys iOS-sovelluksessa](mobile-engagement-ios-integrate-engagement.md)

2. Ilmoitukset: [miten voit integroida Reach (ilmoitusten) iOS-sovelluksessa](mobile-engagement-ios-integrate-engagement-reach.md)

3. Tunnisteen suunnitelman käyttöönoton: [Lisäasetukset Mobile-välitys tunnisteita API iOS-sovelluksen käyttäminen](mobile-engagement-ios-use-engagement-api.md)


##<a name="release-notes"></a>Julkaisutiedot

### <a name="400-09122016"></a>4.0.0 (09/12/2016)

-   Kiinteä ilmoitus ei actioned iOS 10-laitteisiin.
-   Käytöstä XCode 7.

Aiemman version, katso [täydellinen julkaisutiedot](mobile-engagement-ios-release-notes.md)

##<a name="upgrade-procedures"></a>Päivityksen ohjeita

Jos valmiiksi välitys vanhempi versio on asennettu sovellukseen, sinun on ottaa huomioon seuraavat seikat, kun päivitetään SDK.

Saatat joutua noudata useita ohjeita, jos vastattu SDK Katso täydellinen [Päivittäminen menettelyt](mobile-engagement-ios-upgrade-procedure.md)useita versioita.

Kunkin uuden version SDK, sinun on ensin korvattava (Poista ja tuo uudelleen-xcode) EngagementSDK ja EngagementReach-kansioissa.

###<a name="from-300-to-400"></a>Valitse 3.0.0 4.0.0

### <a name="xcode-8"></a>XCode 8
XCode 8 on pakollinen version 4.0.0 SDK lähtien.

> [AZURE.NOTE] Jos todella XCode 7 määräytyvät voi käyttää [iOS välitys SDK v3.2.4](https://aka.ms/r6oouh). Esitettävän iOS 10-laitteisiin saatavilla moduuli tätä aiemman version ei tunnetut ohjelmavirhe: järjestelmäilmoitukset eivät ole actioned. Voit korjata ongelman Toteuta poistetuista Ohjelmointirajapinnan on `application:didReceiveRemoteNotification:` sovelluksen delegoida seuraavasti:

    - (void)application:(UIApplication*)application
    didReceiveRemoteNotification:(NSDictionary*)userInfo
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:nil];
    }

> [AZURE.IMPORTANT] Kuin tätä toimintoa **ei ole suositeltavaa tämä Vaihtoehtoinen menetelmä** voit muuttaa minkä tahansa tulevista (myös aliversiot) iOS-version päivittämisen koska tämä iOS API on poistettu. Sinun kannattaa siirtyä XCode 8 mahdollisimman pian.

#### <a name="usernotifications-framework"></a>UserNotifications framework
Sinun on lisättävä `UserNotifications` framework luominen-vaiheessa.

Avaa project-ruutu ja valitse oikea kohde project explorer. Avaa **"Muodosta vaiheiden"** -välilehti ja **"binaarinen kanssa kirjastojen"** -valikosta Lisää framework `UserNotifications.framework` -linkki kuin määrittäminen`Optional`

#### <a name="application-push-capability"></a>Sovelluksen push-ominaisuus
XCode 8 voi palauttaa sovelluksen push-ominaisuutta, kaksinkertainen Tarkista se `capability` valittu kohde-välilehti.

#### <a name="add-the-new-ios-10-notification-registration-code"></a>Lisää uusi iOS 10 ilmoituksen rekisteröintikoodi
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

#### <a name="if-you-already-have-your-own-unusernotificationcenterdelegate-implementation"></a>Jos sinulla on jo oma UNUserNotificationCenterDelegate käyttöönotto

SDK on oma soveltaminen UNUserNotificationCenterDelegate-protokollaa. Se käyttää SDK seurannassa elinkaaren välitys ilmoitukset-laitteet, joissa iOS 10 tai uudempi. Jos SDK havaitsee edustajallasi se ei käytä omassa käyttöönoton, koska voi olla vain yksi UNUserNotificationCenter edustajan sovellusta kohden. Tämä tarkoittaa, että pystyt välitys logiikan lisääminen Omat edustajalle.

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