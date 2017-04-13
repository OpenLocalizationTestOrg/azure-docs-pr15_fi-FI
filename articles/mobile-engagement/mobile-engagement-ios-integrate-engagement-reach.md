<properties
    pageTitle="Azure Mobile välitys iOS SDK saavuttaa integrointi | Microsoft Azure"
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

#<a name="how-to-integrate-engagement-reach-on-ios"></a>Miten voit integroida välitys saavuttaa iOS

Sinun on noudatettava integrointi kuvatulla tavalla, [miten voit integroida välitys iOS asiakirjan](mobile-engagement-ios-integrate-engagement.md) ennen seuraavan tässä oppaassa.

Näissä ohjeissa on oltava XCode 8. Jos todella XCode 7 määräytyvät voi käyttää [iOS välitys SDK v3.2.4](https://aka.ms/r6oouh). On tunnettu ohjelmavirhe tämän aiemman version esitettävän iOS 10-laitteissa: järjestelmäilmoitukset eivät ole actioned. Voit korjata ongelman Toteuta poistetuista Ohjelmointirajapinnan on `application:didReceiveRemoteNotification:` sovelluksen delegoida seuraavasti:

    - (void)application:(UIApplication*)application
    didReceiveRemoteNotification:(NSDictionary*)userInfo
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:nil];
    }

> [AZURE.IMPORTANT] Kuin tätä toimintoa **ei ole suositeltavaa tämä Vaihtoehtoinen menetelmä** voit muuttaa minkä tahansa tulevista (myös aliversiot) iOS-version päivittämisen koska tämä iOS API on poistettu. Sinun kannattaa siirtyä XCode 8 mahdollisimman pian.

### <a name="enable-your-app-to-receive-silent-push-notifications"></a>Ottaa käyttöön sovelluksen hiljainen Push-ilmoitusten vastaanottotilanteiden

[AZURE.INCLUDE [mobile-engagement-ios-silent-push](../../includes/mobile-engagement-ios-silent-push.md)]

##<a name="integration-steps"></a>Integrointivaiheet

### <a name="embed-the-engagement-reach-sdk-into-your-ios-project"></a>Upottaa iOS projektin välitys saavuttaa SDK-paketissa

-   Lisää Reach sdk Xcode projektin. Siirry Xcode, **projektin \> lisääminen projektiin** ja valitse sitten `EngagementReach` kansio.

### <a name="modify-your-application-delegate"></a>Muokkaa sovelluksen edustajallasi

-   Käyttöönoton tiedoston yläosassa tuoda välitys saavuttaa-moduulia seuraavasti:

        [...]
        #import "AEReachModule.h"

-   Menetelmä sisällä `applicationDidFinishLaunching:` tai `application:didFinishLaunchingWithOptions:`, Luo reach moduuli ja siirtää aiemmin välitys alustus viivan:

        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
          AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
          [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}" modules:reach, nil];
          [...]

          return YES;
        }

-   Muokkaa **'icon.png'** merkkijono, joka sisältää kuvan nimi, jonka haluat ilmoitus-kuvake.
-   Jos haluat käyttää Reach kampanjat *päivityksen merkin arvo* -komentoa tai jos haluat käyttää alkuperäisen push \<SaaS/Reach API/markkinointikampanjan Muotoile/Native Push\> kampanjat, anna, Reach moduulin hallinta merkin kuvake itse (se automaattisesti poista sovellus-merkin ja nollaa myös tallentamat välitys aina, kun sovellus on aloitettu tai foregrounded arvo). Tämä tehdään lisäämällä seuraava rivi Reach moduulin alustamisen jälkeen:

        [reach setAutoBadgeEnabled:YES];

-   Jos haluat käsitellä Reach tietojen push-on Anna sovelluksen edustajallasi mukaisia `AEReachDataPushDelegate` protokolla. Lisää seuraava rivi Reach moduulin alustamisen jälkeen:

        [reach setDataPushDelegate:self];

-   Sitten voit toteuttaa menetelmiä `onDataPushStringReceived:` ja `onDataPushBase64ReceivedWithDecodedBody:andEncodedBody:` -sovelluksen edustajallasi:

        -(BOOL)didReceiveStringDataPushWithCategory:(NSString*)category body:(NSString*)body
        {
           NSLog(@"String data push message with category <%@> received: %@", category, body);
           return YES;
        }

        -(BOOL)didReceiveBase64DataPushWithCategory:(NSString*)category decodedBody:(NSData *)decodedBody encodedBody:(NSString *)encodedBody
        {
           NSLog(@"Base64 data push message with category <%@> received: %@", category, encodedBody);
           // Do something useful with decodedBody like updating an image view
           return YES;
        }

### <a name="category"></a>Luokka

Luokka-parametri on valinnainen, kun luot tietojen Push markkinointikampanjan ja avulla voit suodattaa tietoja Vie. Tämä on kätevä, jos haluat push eri, `Base64` tiedot ja haluat niiden tyypin selvittäminen ennen jäsennyksen ne.

**Sovellus on nyt valmis ja reach sisällön näyttämiseen!**

##<a name="how-to-receive-announcements-and-polls-at-any-time"></a>Kuinka voit saada ilmoituksia ja kyselyiden milloin tahansa

Välitys lähettää Reach ilmoitukset käyttäjiesi milloin tahansa Apple Push-ilmoituspalvelu avulla.

Voit ottaa tämän toiminnon käyttöön, sinun on valmisteleminen sovelluksesi Apple push-ilmoitukset ja muokata sovelluksen edustajallasi.

### <a name="prepare-your-application-for-apple-push-notifications"></a>Valmistele sovelluksesi Apple push-ilmoitukset

Noudata opas: [kuinka sovelluksesi Apple Push-ilmoitukset](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/AddingCapabilities/AddingCapabilities.html#//apple_ref/doc/uid/TP40012582-CH26-SW6)

### <a name="add-the-necessary-client-code"></a>Lisää tarvittavat asiakas-koodi

*Tässä vaiheessa sovelluksen pitäisi olla rekisteröity Apple push sertifikaatti välitys edusta.*

Jos se ei ole ottanut käyttöön, haluat rekisteröidä sovelluksesi push-ilmoitusten vastaanottaminen.

* Tuo `User Notification` framework:

        #import <UserNotifications/UserNotifications.h>

* Lisää seuraava rivi, kun sovellus käynnistyy (yleensä joka `application:didFinishLaunchingWithOptions:`):

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

Sitten haluat tarjota välitys laitteen tunnuksen palauttama Apple-palvelimiin. Tämä tapahtuu nimeltä menetelmä `application:didRegisterForRemoteNotificationsWithDeviceToken:` -sovelluksen edustajallasi:

    - (void)application:(UIApplication*)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData*)deviceToken
    {
        [[EngagementAgent shared] registerDeviceToken:deviceToken];
    }

Lopuksi on ilmoittaa välitys SDK-paketissa, kun sovellus vastaanottaa remote ilmoituksen. Saadakseen ne kutsua menetelmää `applicationDidReceiveRemoteNotification:fetchCompletionHandler:` -sovelluksen edustajallasi:

    - (void)application:(UIApplication*)application didReceiveRemoteNotification:(NSDictionary*)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult result))handler
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:handler];
    }

> [AZURE.NOTE] Edellä kuvattuja on otettu käyttöön iOS 7. Jos kohdennat iOS < 7, varmista, että toteuttamisesta menetelmä `application:didReceiveRemoteNotification:` sovelluksen edustajan ja puhelun `applicationDidReceiveRemoteNotification` EngagementAgent välittämällä sen sijaan, että nolla- `handler` argumentti:

    - (void)application:(UIApplication*)application
    didReceiveRemoteNotification:(NSDictionary*)userInfo
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:nil];
    }

> [AZURE.IMPORTANT] Oletusarvon mukaan välitys saavuttaa valvonnasta completionHandler. Jos haluat vastata manuaalisesti `handler` ehto-koodia, voit siirtää nolla `handler` argumentin ja ohjausobjektin suorittamisen estäminen itse. Katso `UIBackgroundFetchResult` tyyppi luettelon mahdollisista arvoista.


### <a name="full-example"></a>Koko Esimerkki

Näin integrointi koko Esimerkki:

    #pragma mark -
    #pragma mark Application lifecycle

    - (BOOL)application:(UIApplication*)application didFinishLaunchingWithOptions:(NSDictionary*)launchOptions
    {
      /* Reach module */
      AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
      [reach setAutoBadgeEnabled:YES];

      /* Engagement initialization */
      [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}" modules:reach, nil];
      [[EngagementAgent shared] setPushDelegate:self];

      /* Views */
      [window addSubview:[tabBarController view]];
      [window makeKeyAndVisible];

      [application registerForRemoteNotificationTypes:UIRemoteNotificationTypeAlert|UIRemoteNotificationTypeBadge|UIRemoteNotificationTypeSound];
      return YES;
    }

    - (void)application:(UIApplication*)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData*)deviceToken
    {
      [[EngagementAgent shared] registerDeviceToken:deviceToken];
    }

    - (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult result))handler
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:handler];
    }

### <a name="if-you-have-your-own-unusernotificationcenterdelegate-implementation"></a>Jos sinulla on oma UNUserNotificationCenterDelegate käyttöönotto

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

##<a name="how-to-customize-campaigns"></a>Kampanjat mukauttamisesta

### <a name="notifications"></a>Ilmoitukset

Ilmoitusten kahdenlaisia: Järjestelmä- ja sovelluskohtaisesta ilmoitukset.

Järjestelmäilmoituksia käsitellään iOS mukaan eikä sitä voi mukauttaa.

Sovellusten ilmoitukset tehdään näkymän, joka lisätään dynaamisesti nykyinen sovellusikkuna. Tätä kutsutaan ilmoituksen päällekkäin. Ilmoitus päällekkäiset soveltuvat erinomaisesti nopea integrointi, koska niitä ei edellytä voit muokata sovelluksen missä tahansa näkymässä.

#### <a name="layout"></a>Asettelu

Voit muokata sovelluskohtaisesta ilmoitukset ulkoasua, voit yksinkertaisesti muokata tiedostoa `AENotificationView.xib` tarpeitasi, voit pitää tunnisteen arvoja ja aiemmin subviews tyypit pitkä.

Oletusarvon mukaan-sovellusten ilmoitukset esitetään näytön alareunassa. Jos haluat näyttää ne näytön yläreunaan, muokata annettu `AENotificationView.xib` ja muuta `AutoSizing` ensisijaista näkymää, jotta se voidaan säilyttää sen superview yläreunassa-ominaisuus.

#### <a name="categories"></a>Luokat

Kun muokkaat annettu asettelun, voit muokata kaikki ilmoitukset ulkoasua. Luokkien avulla voit määrittää ilmoitusten eri kohdennettujen näyttää (mahdollisesti toiminnan). Luokan voidaan määrittää Reach markkinointikampanjan luodessasi. Ota huomioon, että luokat mahdollistavat myös ilmoitukset ja kyselyiden, mukauttaminen, joka on kuvattu jäljempänä tässä asiakirjassa.

Rekisteröi luokan käsittelytoiminto ilmoituksissa, tarvitset lisää kutsu, kun reach moduulin alustaa.

    AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
    [reach registerNotifier:myNotifier forCategory:@"my_category"];
    ...

`myNotifier`objekti, joka noudattaa protokolla esiintymää on oltava `AENotifier`.

Voit toteuttaa protokolla menetelmiä itse tai voit valita olemassa olevia luokan reimplement `AEDefaultNotifier` joka aiheuttaa suurimman osan.

Jos haluat muokata tietyn luokan ilmoituksen näkymän, voit seurata tässä esimerkissä:

    #import "AEDefaultNotifier.h"
    #import "AENotificationView.h"
    @interface MyNotifier : AEDefaultNotifier
    @end

    @implementation MyNotifier

    -(NSString*)nibNameForCategory:(NSString*)category
    {
      return "MyNotificationView";
    }

    @end

Tässä yksikertaisessa esimerkissä luokan oletetaan, että tiedoston nimeltä `MyNotificationView.xib` -ensisijaista sovellusta pikaoppaista. Jos menetelmä ei löydä vastaava `.xib`, ilmoitus ei näytetä ja välitys siirtää viestin konsolissa.

Annettu nib tiedoston olisi noudatettava seuraavia sääntöjä:

-   Pitäisi olla vain yksi näkymä.
-   Subviews olisi, samaan tapaan kuin niistä annettu nib-tiedoston nimi`AENotificationView.xib`
-   Subviews on oltava samaa tunnistetta kuin niistä sisällä annettu nib-tiedosto nimeltä`AENotificationView.xib`

> [AZURE.TIP] Kopioi vain annettu nib-tiedosto nimeltä `AENotificationView.xib`, ja aloita työskentely sieltä. Mutta Varo, näkymän nib-tiedoston sisälle liitetään luokan `AENotificationView`. Tämä luokan uudelleen menetelmä `layoutSubViews` , siirtäminen ja koon sen subviews kontekstin mukaan. Voit korvata `UIView` tai mukautetun näkymän luokka.

Jos tarvitset ilmoitukset tarkempaa mukautukset (Jos esimerkiksi haluat ladata näkymän suoraan koodi), on suositeltavaa annettu lähde-koodin ja luokan asiakirjat Tutustu `Protocol ReferencesDefaultNotifier` ja `AENotifier`.

Huomaa, että voit käyttää samaa ilmoitus useita luokkia.

Voit myös määrittää uudelleen oletusarvo-ilmoitus seuraavasti:

    AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
    [reach registerNotifier:myNotifier forCategory:kAEReachDefaultCategory];

##### <a name="notification-handling"></a>Ilmoitus käsittely

Oletus-luokan käytettäessä jotkin elinkaaren menetelmät kutsutaan `AEReachContent` objektin raportin tilastoja ja Päivitä markkinointikampanjan tila:

-   Kun sovellus, näkyy ilmoitus `displayNotification` menetelmää kutsutaan (joka ilmoittaa tilastotiedot) mukaan `AEReachModule` Jos `handleNotification:` palauttaa `YES`.
-   Jos ilmoituksessa, joka on piilotettu, `exitNotification` menetelmää kutsutaan, Tilasto ilmoitetaan ja seuraavan Kampanjat nyt voidaan käsitellä.
-   Jos ilmoituksen napsautetaan, `actionNotification` on nimeltään-tilasto on ilmoitettu ja liittyvä toiminto suoritetaan.

Jos käyttöönoton `AENotifier` ohittaa oletustoiminnon, tarvitsemat elinkaaren näistä tavoista itse. Seuraavissa esimerkeissä kuvataan joissakin tapauksissa, jossa oletustoiminnan on ohitettu:

-   Älä laajentaa `AEDefaultNotifier`, kuten luokan käsittely alusta alkaen on käytössä.
-   Voit overrode `prepareNotificationView:forContent:`, varmista, että voit yhdistää vähintään `onNotificationActioned` tai `onNotificationExited` johonkin oman U.I ohjausobjektit.

> [AZURE.WARNING] Jos `handleNotification:` aiheuttaa poikkeuksen sisältö poistetaan ja `drop` on nimeltään, tämä ilmoitetaan tilastoja ja seuraavan Kampanjat nyt voidaan käsitellä.

#### <a name="include-notification-as-part-of-an-existing-view"></a>Sisällytä ilmoituksen osana aiemmin luotuun näkymään

Päällekkäiset soveltuvat erinomaisesti nopea integrointi, mutta voi olla joskus ei käteviä, tai valita ei-toivottuja vaikutukset.

Jos et ole tyytyväinen kerros-järjestelmän joissakin näkymiä, voit mukauttaa sen näkymissä.

Voit päättää sisällytettävien Microsoftin ilmoitusta asettelun aiemmin luotuja näkymiä. Voit tehdä on kaksi käyttöönoton tyyliä:

1.  Ilmoitus-näkymän käyttöliittymän muodostimen avulla

    -   Avaa *liittymän muodostin*
    -   Aseta 320 x 60 (tai 768 x 60, jos käytössä on iPad) `UIView` kohtaa, johon haluat näkyvän ilmoitus
    -   Tämä näkymä tunnisteen arvoksi asetetaan: **36822491**

2.  Ilmoitus-näkymän lisääminen ohjelmallisesti. Lisää seuraava koodi vain, kun näkymä on valmisteltu:

        UIView* notificationView = [[UIView alloc] initWithFrame:CGRectMake(0, 0, 320, 60)]; //Replace x and y coordinate values to your needs.
        notificationView.tag = NOTIFICATION_AREA_VIEW_TAG;
        [self.view addSubview:notificationView];

`NOTIFICATION_AREA_VIEW_TAG`makron löytyvät `AEDefaultNotifier.h`.

> [AZURE.NOTE] Oletus-ilmoitus tunnistaa automaattisesti ilmoituksen asettelun sisältyy tässä näkymässä ja sitä varten ei lisää näyttäminen videokuvan.

### <a name="announcements-and-polls"></a>Ilmoitukset ja kyselyiden

#### <a name="layouts"></a>Asettelut

Voit muokata tiedostoja `AEDefaultAnnouncementView.xib` ja `AEDefaultPollView.xib` , kunhan säilytät tunnisteen arvoja ja aiemmin subviews tyypit.

#### <a name="categories"></a>Luokat

##### <a name="alternate-layouts"></a>Vaihtoehtoinen asettelut

Ilmoitukset, kuten markkinointikampanja luokan voidaan on vaihtoehtoinen asettelut ilmoitukset ja kyselyjä.

Jos haluat luoda ilmoituksen luokka, laajenna **AEAnnouncementViewController** ja rekisteröitävä se, kun reach moduuli on valmisteltu:

    AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
    [reach registerAnnouncementController:[MyCustomAnnouncementViewController class] forCategory:@"my_category"];

> [AZURE.NOTE] Aina, kun käyttäjä Valitse ilmoitus, jossa luokan ilmoituksen "Omat\_luokan", rekisteröity näkymän ohjaimen (Tässä tapauksessa `MyCustomAnnouncementViewController`)-menetelmällä alustaa `initWithAnnouncement:` ja näkymän lisätään nykyisen sovellusikkunan.

-Ympäristössä `AEAnnouncementViewController` luokka ominaisuus on `announcement` alustaminen oman subviews. Harkitse alla olevassa esimerkissä, jossa kaksi otsikot ovat valmistellaan käyttämällä `title` ja `body` ominaisuudet `AEReachAnnouncement` luokan:

    -(void)loadView
    {
        [super loadView];

        UILabel* titleLabel = [[UILabel alloc] initWithFrame:CGRectMake(10, 20, 300, 60)];
        titleLabel.font = [UIFont systemFontOfSize:32.0];
        titleLabel.text = self.announcement.title;

        UILabel* bodyLabel = [[UILabel alloc] initWithFrame:CGRectMake(10, 20, 300, 60)];
        bodyLabel.font = [UIFont systemFontOfSize:24.0];
        bodyLabel.text = self.announcement.body;

        [self.view addSubview:titleLabel];
        [self.view addSubview:bodyLabel];
    }

Jos et halua ladata näkymien itse, mutta haluat käyttää ilmoitus-näkymän oletusasettelun, voit tehdä vain mukautetun näkymän ohjaimen laajentaa annettu luokan `AEDefaultAnnouncementViewController`. Kaksoiskappaleiden siinä tapauksessa nib tiedoston `AEDefaultAnnouncementView.xib` ja anna sille nimi, jotta se voidaan ladata mukautetun näkymän valvoja (ohjaimen, jonka nimi on `CustomAnnouncementViewController`, Soita nib tiedoston `CustomAnnouncementView.xib`).

Korvaa ilmoitukset oletusarvo-luokka, riittää, että rekisteröidä määritetty luokkaan mukautetun näkymän ohjaimen `kAEReachDefaultCategory`:

    [reach registerAnnouncementController:[MyCustomAnnouncementViewController class] forCategory:kAEReachDefaultCategory];

Kyselyiden voi mukauttaa samalla tavalla:

    AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
    [reach registerPollController:[MyCustomPollViewController class] forCategory:@"my_category"];

Tällä hetkellä, annettu `MyCustomPollViewController` on laajennettava `AEPollViewController`. Tai voit laajentaa oletusarvoisen ohjauskoneen: `AEDefaultPollViewController`.

> [AZURE.IMPORTANT] Älä unohda Soita joko `action` (`submitAnswers:` Mukautettu kysely Näytä ohjainten) tai `exit` menetelmä ennen näkymä-ohjain on piilotettu. Muussa tapauksessa tilastotiedot ei lähetetä (eli ei analytics-markkinointikampanja) ja lisää seurannan aikana seuraavan Kampanjat ei lähetetä, ennen kuin hakeminen käynnistetään.

##### <a name="implementation-example"></a>Käyttöönotto-Esimerkki

Tämä toteutus mukautettu ilmoitus-näkymä on ladattu ulkoisen XI b-tiedostosta.

Kuten kehittyneet ilmoituksen mukauttaminen, on suositeltavaa tarkasteltavan vakio käyttöönoton lähdekoodia.

`CustomAnnouncementViewController.h`

    //Interface
    @interface CustomAnnouncementViewController : AEAnnouncementViewController {
      UILabel* titleLabel;
      UITextView* descTextView;
      UIWebView* htmlWebView;
      UIButton* okButton;
      UIButton* cancelButton;
    }

    @property (nonatomic, retain) IBOutlet UILabel* titleLabel;
    @property (nonatomic, retain) IBOutlet UITextView* descTextView;
    @property (nonatomic, retain) IBOutlet UIWebView* htmlWebView;
    @property (nonatomic, retain) IBOutlet UIButton* okButton;
    @property (nonatomic, retain) IBOutlet UIButton* cancelButton;

    -(IBAction)okButtonClicked:(id)sender;
    -(IBAction)cancelButtonClicked:(id)sender;

`CustomAnnouncementViewController.m`

    //Implementation
    @implementation CustomAnnouncementViewController
    @synthesize titleLabel;
    @synthesize descTextView;
    @synthesize htmlWebView;
    @synthesize okButton;
    @synthesize cancelButton;

    -(id)initWithAnnouncement:(AEReachAnnouncement*)anAnnouncement
    {
      self = [super initWithNibName:@"CustomAnnouncementViewController" bundle:nil];
      if (self != nil) {
        self.announcement = anAnnouncement;
      }
      return self;
    }

    - (void) dealloc
    {
      [titleLabel release];
      [descTextView release];
      [htmlWebView release];
      [okButton release];
      [cancelButton release];
      [super dealloc];
    }

    - (void)viewDidLoad {
      [super viewDidLoad];

      /* Init announcement title */
      titleLabel.text = self.announcement.title;

      /* Init announcement body */
      if(self.announcement.type == AEAnnouncementTypeHtml)
      {
        titleLabel.hidden = YES;
        htmlWebView.hidden = NO;
        [htmlWebView loadHTMLString:self.announcement.body baseURL:[NSURL URLWithString:@"http://localhost/"]];
      }
      else
      {
        titleLabel.hidden = NO;
        htmlWebView.hidden = YES;
        descTextView.text = self.announcement.body;
      }

      /* Set action button label */
      if([self.announcement.actionLabel length] > 0)
        [okButton setTitle:self.announcement.actionLabel forState:UIControlStateNormal];

      /* Set exit button label */
      if([self.announcement.exitLabel length] > 0)
        [cancelButton setTitle:self.announcement.exitLabel forState:UIControlStateNormal];
    }

    #pragma mark Actions

    -(IBAction)okButtonClicked:(id)sender
    {
        [self action];
    }

    -(IBAction)cancelButtonClicked:(id)sender
    {
        [self exit];
    }

    @end
