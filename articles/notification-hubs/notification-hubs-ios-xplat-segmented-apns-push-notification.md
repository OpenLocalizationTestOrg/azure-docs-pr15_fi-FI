<properties
    pageTitle="Ilmoitus keskittimet jakautumisen uutisia opetusohjelma – iOS"
    description="Opettele käyttämään Azure palvelun Bus ilmoituksen keskittimet jakautumisen uutisia ilmoitusten lähettämisen iOS-laitteita."
    services="notification-hubs"
    documentationCenter="ios"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

# <a name="use-notification-hubs-to-send-breaking-news"></a>Ilmoitus keskittimet avulla voit lähettää uutisia

[AZURE.INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]


##<a name="overview"></a>Yleiskatsaus

Tässä ohjeaiheessa esitellään käyttämisestä Azure ilmoituksen keskittimet yleislähetys jakautumisen uutisia ilmoitukset iOS-sovellukseen. Kun olet valmis, jota voi rekisteröidä uudet luokat kiinnostavan purkaa, ja vain kyseisten luokkien push-ilmoitusten vastaanottaminen. Tämä skenaario on useita sovelluksia yleistä mallia, joiden ilmoitukset lähetetään käyttäjäryhmille, joita on aiemmin määritetty korko niitä, kuten RSS lukijan, sovellusten musiikkia tuulettimista jne.

Lähetyksen tilanteita, joissa on otettu sisällyttämällä vähintään yksi _tunnisteita_ luotaessa rekisteröinti ilmoitus-toiminnossa. Ilmoitukset lähetetään tunnisteen, kun kaikki laitteet, joissa olet rekisteröinyt tunnisteen saa ilmoituksen. Koska tunnisteet yksinkertaisesti merkkijonoja, niitä ei tarvitse valmisteltu etukäteen. Lisätietoja tunnisteet lisätietoja [ilmoituksen keskittimet reititys ja tunnisteen lausekkeita](notification-hubs-tags-segment-push-message.md).


##<a name="prerequisites"></a>Edellytykset

Tässä ohjeaiheessa muodostaa luomasi [ilmoituksen keskittimet käytön aloittaminen]-sovellukseen[get-started]. Ennen kuin aloitat Tässä opetusohjelmassa, olet jo tehnyt [ilmoituksen keskittimet käytön aloittaminen][get-started].

##<a name="add-category-selection-to-the-app"></a>Luokka-valinta lisääminen sovellukseen

Ensimmäiseksi on Käyttöliittymän osat lisääminen oman aiemmin kuvakäsikirjoitus, joiden avulla käyttäjä voi valita rekisteröidä luokkia. Laitteessa on tallennettu käyttäjän valittuun luokkaan. Kun sovellus käynnistyy-laitteen rekisteröinti luodaan ilmoitus-toiminnossa valitun luokille tunnisteina.

1. Lisää oman MainStoryboard_iPhone.storyboard objektin kirjastosta seuraavat osat:
    + Otsikko ja teksti "Purkaa uutisia"
    + Otsikoiden ja luokan tekstit "Maailman", "Tutkimuspolitiikasta", "Business", "Tekniikka", "Tiede", "Urheilu"
    + Kuusi valitsimet, yksi luokka, Määritä kunkin valitsimen **tila** on oletusarvoisesti **pois käytöstä** .
    + Yksi painike, jossa "Tilaa"

    Oman kuvakäsikirjoitus pitäisi näyttää seuraavasti:

    ![][3]

2. Avustaja-editorissa luoda kaikkien valitsimien pistorasioita ja kutsua niitä "WorldSwitch", "PoliticsSwitch", "BusinessSwitch", "TechnologySwitch", "ScienceSwitch", "SportsSwitch"


3. Luo nimeltä "tilaa" painikkeen toiminto. Oman ViewController.h pitäisi olla seuraavat:

        @property (weak, nonatomic) IBOutlet UISwitch *WorldSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *PoliticsSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *BusinessSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *TechnologySwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *ScienceSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *SportsSwitch;

        - (IBAction)subscribe:(id)sender;

4. Luo nimeltään uusi **Kaakao kosketa luokan** `Notifications`. Kopioi seuraava koodi-tiedoston Notifications.h liittymän osaan:

        @property NSData* deviceToken;

        - (id)initWithConnectionString:(NSString*)listenConnectionString HubName:(NSString*)hubName;

        - (void)storeCategoriesAndSubscribeWithCategories:(NSArray*)categories
                    completion:(void (^)(NSError* error))completion;

        - (NSSet*)retrieveCategories;

        - (void)subscribeWithCategories:(NSSet*)categories completion:(void (^)(NSError *))completion;

5. Lisää seuraavat tuo direktiivin Notifications.m:

        #import <WindowsAzureMessaging/WindowsAzureMessaging.h>

6. Kopioi seuraava koodi tiedoston Notifications.m käyttöönotto-kohtaan.

        SBNotificationHub* hub;

        - (id)initWithConnectionString:(NSString*)listenConnectionString HubName:(NSString*)hubName{

            hub = [[SBNotificationHub alloc] initWithConnectionString:listenConnectionString
                                        notificationHubPath:hubName];

            return self;
        }

        - (void)storeCategoriesAndSubscribeWithCategories:(NSSet *)categories completion:(void (^)(NSError *))completion {
            NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];
            [defaults setValue:[categories allObjects] forKey:@"BreakingNewsCategories"];

            [self subscribeWithCategories:categories completion:completion];
        }


        - (NSSet*)retrieveCategories {
            NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];

            NSArray* categories = [defaults stringArrayForKey:@"BreakingNewsCategories"];

            if (!categories) return [[NSSet alloc] init];
            return [[NSSet alloc] initWithArray:categories];
        }


        - (void)subscribeWithCategories:(NSSet *)categories completion:(void (^)(NSError *))completion
        {
           //[hub registerNativeWithDeviceToken:self.deviceToken tags:categories completion: completion];

            NSString* templateBodyAPNS = @"{\"aps\":{\"alert\":\"$(messageParam)\"}}";

            [hub registerTemplateWithDeviceToken:self.deviceToken name:@"simpleAPNSTemplate" 
                jsonBodyTemplate:templateBodyAPNS expiryTemplate:@"0" tags:categories completion:completion];
        }



    Tähän luokkaan käyttää paikallisen tallennuspaikkaa voivat tallentaa ja hakea uutisia, joka vastaanottaa laite luokkiin. Lisäksi se sisältää menetelmän näitä luokkia käyttämällä [mallia](notification-hubs-templates-cross-platform-push-messages.md) rekisteröiminen Rekisteröidy.

7. AppDelegate.h-tiedoston lisääminen import-lause Notifications.h ja lisää ilmoitukset-luokan esiintymän ominaisuuden:

        #import "Notifications.h"

        @property (nonatomic) Notifications* notifications;
    

8. Lisää AppDelegate.m **didFinishLaunchingWithOptions** -menetelmä koodin alustaa menetelmä ilmoitukset esiintymän alussa.  
 
    `HUBNAME`ja `HUBLISTENACCESS` (määritetty hubinfo.h) tulisi olla jo `<hub name>` ja `<connection string with listen access>` paikkamerkit korvataan ilmoitus-toiminnossa nimesi ja yhteysmerkkijonon *DefaultListenSharedAccessSignature* , jotka olet hankkinut aiemmassa versiossa

        self.notifications = [[Notifications alloc] initWithConnectionString:HUBLISTENACCESS HubName:HUBNAME];

    > [AZURE.NOTE] Koska tunnistetiedot, jotka asiakas-sovelluksen jaetaan ei yleensä suojatun, kuunnella access näppäintä pitäisi jakaa vain asiakas-sovelluksessa. Kuuntele access ottaa käyttöön sovelluksen Rekisteröidy ilmoitukset, mutta aiemmin merkintöjä ei voi muokata ja et voi lähettää ilmoitukset. Ilmoitusten lähettäminen ja muuttamalla aiemmin merkintöjen suojatun Taustajärjestelmä palvelun käytetään täydet oikeudet-näppäintä.


9. Korvaa AppDelegate.m **didRegisterForRemoteNotificationsWithDeviceToken** -menetelmä menetelmän koodia seuraava koodi laitteen tunnuksen siirtää ilmoitukset-luokka. Ilmoitukset-luokan suorittaa rekisteröiminen ilmoituksissa luokille. Jos käyttäjä muuttaa luokan valinnat, emme Soita `subscribeWithCategories` menetelmä vastauksena **tilaa** -painikkeesta, voit päivittää ne.

    > [AZURE.NOTE] Koska määritetty mukaan Apple Push-ilmoituksen Service (APN) laitteen tunnuksen voit mahdollista, milloin tahansa, Rekisteröi ilmoitusten usein tapahtuvien virheiden ilmoitus. Tässä esimerkissä Rekisteröi joka kerta, kun sovellus käynnistyy-ilmoituksen. Sovellusten, jotka suoritetaan usein useammin kuin kerran päivässä, voit todennäköisesti ohittaa rekisteröinti Jos edellisen rekisteröinti on kulunut alle yhden päivän kaistanleveyden säilyttää.

        self.notifications.deviceToken = deviceToken;

        // Retrieves the categories from local storage and requests a registration for these categories
        // each time the app starts and performs a registration.

        NSSet* categories = [self.notifications retrieveCategories];
        [self.notifications subscribeWithCategories:categories completion:^(NSError* error) {
            if (error != nil) {
                NSLog(@"Error registering for notifications: %@", error);
            }
        }];


    Huomaa, että tässä vaiheessa olisi muu koodi **didRegisterForRemoteNotificationsWithDeviceToken** -menetelmää.

10. Seuraavista menetelmistä jo esitetään AppDelegate.m suorittamasta [aloittaminen ilmoituksen keskittimet] [ get-started] opetusohjelma.  Jos et lisää ne.

        -(void)MessageBox:(NSString *)title message:(NSString *)messageText
        {
            UIAlertView *alert = [[UIAlertView alloc] initWithTitle:title message:messageText delegate:self
                cancelButtonTitle:@"OK" otherButtonTitles: nil];
            [alert show];
        }

        - (void)application:(UIApplication *)application didReceiveRemoteNotification:
            (NSDictionary *)userInfo {
            NSLog(@"%@", userInfo);
            [self MessageBox:@"Notification" message:[[userInfo objectForKey:@"aps"] valueForKey:@"alert"]];
        }

    Tämä menetelmä käsittelee ilmoitusten, kun sovellus on käynnissä näyttämällä Yksinkertainen **UIAlert**.

11. Lisää import-lause AppDelegate.h ViewController.m, ja kopioi seuraava koodi XCode luodut **tilaa** -menetelmää. Tämä koodi päivittää ilmoituksen rekisteröinnin uusi luokka-tunnisteiden, käyttäjä on valinnut käyttöliittymässä.

        ```
        #import "Notifications.h"
        ```

        NSMutableArray* categories = [[NSMutableArray alloc] init];

        if (self.WorldSwitch.isOn) [categories addObject:@"World"];
        if (self.PoliticsSwitch.isOn) [categories addObject:@"Politics"];
        if (self.BusinessSwitch.isOn) [categories addObject:@"Business"];
        if (self.TechnologySwitch.isOn) [categories addObject:@"Technology"];
        if (self.ScienceSwitch.isOn) [categories addObject:@"Science"];
        if (self.SportsSwitch.isOn) [categories addObject:@"Sports"];

        Notifications* notifications = [(AppDelegate*)[[UIApplication sharedApplication]delegate] notifications];

        [notifications storeCategoriesAndSubscribeWithCategories:categories completion: ^(NSError* error) {
            if (!error) {
                [(AppDelegate*)[[UIApplication sharedApplication]delegate] MessageBox:@"Notification" message:@"Subscribed!"];
            } else {
                NSLog(@"Error subscribing: %@", error);
            }
        }];

    Tämä menetelmä luo **NSMutableArray** luokkien ja käyttää **ilmoitukset** -luokan tallentamista paikalliseen muistiin ja rekisteröi vastaavan tunnisteet ilmoitus-toiminnossa. Muuttuessa luokat rekisteröinnin luodaan uusi luokille.

12. ViewController.m Lisää seuraava koodi, voit määrittää aiemmin tallennetut luokkiin perustuvan käyttöliittymän **viewDidLoad** -menetelmää.


        // This updates the UI on startup based on the status of previously saved categories.

        Notifications* notifications = [(AppDelegate*)[[UIApplication sharedApplication]delegate] notifications];

        NSSet* categories = [notifications retrieveCategories];

        if ([categories containsObject:@"World"]) self.WorldSwitch.on = true;
        if ([categories containsObject:@"Politics"]) self.PoliticsSwitch.on = true;
        if ([categories containsObject:@"Business"]) self.BusinessSwitch.on = true;
        if ([categories containsObject:@"Technology"]) self.TechnologySwitch.on = true;
        if ([categories containsObject:@"Science"]) self.ScienceSwitch.on = true;
        if ([categories containsObject:@"Sports"]) self.SportsSwitch.on = true;



Sovelluksen nyt voit tallentaa luokkien avulla voit rekisteröidä ilmoitus-toiminnossa aina, kun sovellus käynnistyy laitteen paikallisen muistiin.  Käyttäjä voi muuttaa valitun luokkien suorituksen aikana ja valitse **tilaa** menetelmä Päivitä laitteen rekisteröinti. Seuraavaksi päivität sovelluksen jakautumisen uutisia ilmoitukset lähetetään suoraan-sovelluksessa.


##<a name="optional-sending-tagged-notifications"></a>(valinnainen) Merkitty ilmoitusten lähettäminen

Jos ei ole Visual Studio käyttöoikeus, voit siirtyä seuraavaan osaan ja ilmoitukset lähetetään sovelluksessa. Voit myös lähettää ERISNIMI mallin ilmoituksen [Azure perinteinen Portal] virheenkorjaus-välilehden avulla, ilmoitus-toiminnossa. 

[AZURE.INCLUDE [notification-hubs-send-categories-template](../../includes/notification-hubs-send-categories-template.md)]


##<a name="optional-send-notifications-from-the-device"></a>(valinnainen) Ilmoitukset lähetetään laitteesta

Tavallisesti ilmoitukset lähetettäviä Taustajärjestelmä-palvelu, mutta voit lähettää jakautumisen uutisia ilmoitukset suoraan sovelluksesta. Voit tehdä tämän päivitämme `SendNotificationRESTAPI` menetelmä, jonka määritimme [ilmoituksen keskittimet aloittaminen] [ get-started] opetusohjelma.


1. ViewController.m päivityksessä `SendNotificationRESTAPI` menetelmää kuin seuraa niin, että se hyväksyy parametrin luokka-tunnisteen ja lähettää ERISNIMI- [malli](notification-hubs-templates-cross-platform-push-messages.md) -ilmoituksen.

        - (void)SendNotificationRESTAPI:(NSString*)categoryTag
        {
            NSURLSession* session = [NSURLSession sessionWithConfiguration:[NSURLSessionConfiguration
                                     defaultSessionConfiguration] delegate:nil delegateQueue:nil];

            NSString *json;

            // Construct the messages REST endpoint
            NSURL* url = [NSURL URLWithString:[NSString stringWithFormat:@"%@%@/messages/%@", HubEndpoint,
                                               HUBNAME, API_VERSION]];

            // Generated the token to be used in the authorization header.
            NSString* authorizationToken = [self generateSasToken:[url absoluteString]];

            //Create the request to add the template notification message to the hub
            NSMutableURLRequest *request = [NSMutableURLRequest requestWithURL:url];
            [request setHTTPMethod:@"POST"];

            // Add the category as a tag
            [request setValue:categoryTag forHTTPHeaderField:@"ServiceBusNotification-Tags"];

            // Template notification
            json = [NSString stringWithFormat:@"{\"messageParam\":\"Breaking %@ News : %@\"}",
                    categoryTag, self.notificationMessage.text];

            // Signify template notification format
            [request setValue:@"template" forHTTPHeaderField:@"ServiceBusNotification-Format"];

            // JSON Content-Type
            [request setValue:@"application/json;charset=utf-8" forHTTPHeaderField:@"Content-Type"];

            //Authenticate the notification message POST request with the SaS token
            [request setValue:authorizationToken forHTTPHeaderField:@"Authorization"];

            //Add the notification message body
            [request setHTTPBody:[json dataUsingEncoding:NSUTF8StringEncoding]];

            // Send the REST request
            NSURLSessionDataTask* dataTask = [session dataTaskWithRequest:request
                       completionHandler:^(NSData *data, NSURLResponse *response, NSError *error)
               {
               NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) response;
                   if (error || httpResponse.statusCode != 200)
                   {
                       NSLog(@"\nError status: %d\nError: %@", httpResponse.statusCode, error);
                   }
                   if (data != NULL)
                   {
                       //xmlParser = [[NSXMLParser alloc] initWithData:data];
                       //[xmlParser setDelegate:self];
                       //[xmlParser parse];
                   }
               }];

            [dataTask resume];
        }



2. Päivitä ViewController.m **Lähetä ilmoitus** toiminnon koodi, jota seuraa esitetyllä tavalla. Niin, että se käyttämällä kutakin yksitellen ilmoitukset lähetetään ja lähettää useiden ympäristöjen.



        - (IBAction)SendNotificationMessage:(id)sender
        {
            self.sendResults.text = @"";

            NSArray* categories = [NSArray arrayWithObjects: @"World", @"Politics", @"Business",
                                    @"Technology", @"Science", @"Sports", nil];

            // Lets send the message as breaking news for each category to WNS, GCM, and APNS
            // using a template.
            for(NSString* category in categories)
            {
                [self SendNotificationRESTAPI:category];
            }
        }



3. Projektin uudelleen ja varmista, että sinulla on virheitä muodosta.


##<a name="run-the-app-and-generate-notifications"></a>Suorita sovellus ja luo ilmoituksia

1. Paina voivat laatia projektin ja Käynnistä sovellus Suorita-painiketta. Valitse joitakin jakautumisen uutisia tilaa ja valitse sitten **tilaa** -painike. Raportissa pitäisi näkyä valintaikkuna ilmaisee, että ilmoitukset olet tilannut.

    ![][1]

    Kun valitset **tilaa**, sovellus muuntaa valittujen luokkien tunnisteet ja pyytää uusi laitteen rekisteröinti valitun tunnisteiden ilmoitus-toiminnosta.

2. Kirjoita halutessasi viesti lähetetään uutisia ja paina sitten **Lähetä ilmoitus** -painiketta. Vaihtoehtoisesti voit suorittaa Luo ilmoituksia .NET console-sovelluksen.

    ![][2]


3. Kunkin uutisia on tilattu laitetta saavat jakautumisen uutisia ilmoitukset lähetetään vain.



## <a name="next-steps"></a>Seuraavat vaiheet

Tässä opetusohjelmassa on oppinut yleislähetys uutisia luokittain. Harkitse Viimeistellään jompikumpi seuraavista opetusohjelmat, jotka korostavat muiden ilmoituksen keskittimet tilanteita:

+ **[Lähetä lokalisoitu uutisia ilmoituksen keskittimet avulla]**

    Opettele Laajenna jakautumisen uutiset sovelluksen käyttöön lähettämisen lokalisoitu ilmoitukset.





<!-- Images. -->
[1]: ./media/notification-hubs-ios-send-breaking-news/notification-hub-breakingnews-subscribed.png
[2]: ./media/notification-hubs-ios-send-breaking-news/notification-hub-breakingnews-ios1.png
[3]: ./media/notification-hubs-ios-send-breaking-news/notification-hub-breakingnews-ios2.png








<!-- URLs. -->
[How To: Service Bus Notification Hubs (iOS Apps)]: http://msdn.microsoft.com/library/jj927168.aspx
[Lähetä lokalisoitu uutisia ilmoituksen keskittimet avulla]: notification-hubs-ios-xplat-localized-apns-push-notification.md
[Mobile Service]: /develop/mobile/tutorials/get-started
[Notify users with Notification Hubs]: notification-hubs-aspnet-backend-ios-notify-users.md
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/dn530749.aspx
[Notification Hubs How-To for iOS]: http://msdn.microsoft.com/library/jj927168.aspx
[get-started]: /manage/services/notification-hubs/get-started-notification-hubs-ios/
[Azure perinteinen Portal]: https://manage.windowsazure.com
