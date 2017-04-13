<properties
    pageTitle="Ilmoitus keskittimet lokalisoitu purkaa uutisia opetusohjelma iOS"
    description="Opettele käyttämään Azure palvelun Bus ilmoituksen keskittimet lähettämään lokalisoitu jakautumisen uutisia ilmoituksia (iOS)."
    services="notification-hubs"
    documentationCenter="ios"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="yuaxu"/>

# <a name="use-notification-hubs-to-send-localized-breaking-news-to-ios-devices"></a>Ilmoitus keskittimet avulla voit lähettää lokalisoitu uutisia iOS-laitteille

> [AZURE.SELECTOR]
- [Windows-kaupan C#](notification-hubs-windows-store-dotnet-xplat-localized-wns-push-notification)
- [iOS](notification-hubs-ios-xplat-localized-apns-push-notification)


##<a name="overview"></a>Yleiskatsaus

Tässä ohjeaiheessa esitellään, miten broadcast jakautumisen uutisia ilmoituksia, jotka on lokalisoitu kieli-ja laitteen Azure ilmoituksen keskittimet [Mallit](notification-hubs-templates-cross-platform-push-messages.md) -toiminnon avulla. Tässä opetusohjelmassa aloitetaan luotu [Käytä ilmoituksen keskittimet lähettää uutisia]iOS-sovellukseen. Kun olet valmis, osaat Rekisteröidy kiinnostavan luokat, Määritä kieli, jolla haluat vastaanottaa ilmoituksia ja vain valitun luokkien push-ilmoitusten vastaanottaminen kyseisellä kielellä.


Tässä skenaariossa kaksi osaa on:

- iOS-sovelluksen avulla asiakkaan laitteet kielen ja eri jakautumisen uudet luokat; tilaaminen

- sitten taustatietokantatiedosto lähettää käyttävät Azure ilmoituksen keskittimet **tunnisteen** ja **malli** -feautres ilmoitukset.



##<a name="prerequisites"></a>Edellytykset

Jos olet jo tehnyt [Käytä ilmoituksen keskittimet lähettää uutisia] opetusohjelman ja on käytettävissä, koodi, koska tässä opetusohjelmassa rakentuu suoraan koodia.

Visual Studio 2012 tai uudempi versio on valinnainen.



##<a name="template-concepts"></a>Mallin käsitteitä

[Käytä ilmoituksen keskittimet lähettää uutisia] olet luonut sovellus, jonka eri uudet luokat-ilmoitusten tilaaminen **tunnisteiden** avulla.
Useita sovelluksia on kuitenkin kohteen useita markkina-alueet ja edellyttävät lokalisoinnin. Tämä tarkoittaa, että ilmoitukset itse sisältö on lokalisoitu ja toimitetaan laitteiden oikea joukko.
Tässä ohjeaiheessa on näkyy ilmoitus keskittimet **malli** -toiminnon käyttämisestä aikana helposti lokalisoitu jakautumisen uutisia ilmoitukset.

Huomautus: lokalisoitu ilmoitukset lähetetään yksi tapa on luoda kutakin useita versioita. Esimerkiksi englanti, ranska ja Mandariini tukemaan annettava kolme eri tunnisteita, uutisia: "world_en", "world_fr" ja "world_ch". Olemme olisi maailmanlaajuisesti uutisryhmät lokalisoitu lähettäminen kunkin tunnisteet. Tässä ohjeaiheessa Käytämme malleja tunnisteita koskevia ja vaatimus useiden viestien lähettäminen välttämiseksi.

Korkean tason mallit ovat voi määrittää, miten tietyn laitteen saavat ilmoituksen. Mallin määrittää tarkat tiedot viittaamalla ominaisuudet, jotka ovat osa sovellus että taustatietokantaa lähettämä viesti. Tässä tapauksessa lähetämme kaikki tukemilla kielillä sisältävä alue-ympäristöstä riippumattomalla tavalla viesti:

    {
        "News_English": "...",
        "News_French": "...",
        "News_Mandarin": "..."
    }

Valitse Microsoft varmistat, että laitteet rekisteröidä malli, joka viittaa oikea-ominaisuuteen. Esimerkiksi iOS-sovellusta, joka haluaa rekisteröidä Ranskan uutisia rekisteröiminen seuraavasti:

    {
        aps:{
            alert: "$(News_French)"
        }
    }

Mallit ovat hyvin tehokas ominaisuus, voit lukea lisää [malleja](notification-hubs-templates-cross-platform-push-messages.md) tämän artikkelin.

##<a name="the-app-user-interface"></a>Sovelluksen käyttöliittymässä

On nyt muokata purkaa uutisia sovellus, jonka loit artikkelissa [Käytä ilmoituksen keskittimet lähettää uutisia] lähettää lokalisoitu mallien avulla uutisia.


Lisää oman MainStoryboard_iPhone.storyboard Segmentoitu ohjausobjekti, jossa on kolme kielet, joka on tukee: englanti, ranska ja Mandariini.

![][13]

Varmista, että voit lisätä IBOutlet oman ViewController.h alla kuvatulla tavalla:

![][14]

##<a name="building-the-ios-app"></a>IOS-sovelluksen luominen


1. Lisää *retrieveLocale* -menetelmä oman Notification.h ja muokata kauppa ja tilaa menetelmiä alla kuvatulla tavalla:

        - (void) storeCategoriesAndSubscribeWithLocale:(int) locale categories:(NSSet*) categories completion: (void (^)(NSError* error))completion;

        - (void) subscribeWithLocale:(int) locale categories:(NSSet*) categories completion:(void (^)(NSError *))completion;

        - (NSSet*) retrieveCategories;

        - (int) retrieveLocale;

    Muokkaa yhteyttä Notification.m *storeCategoriesAndSubscribe* -menetelmä lisäämällä Aluekohtaiset asetukset-parametrin ja se tallennetaan käyttäjän oletusasetusten:

        - (void) storeCategoriesAndSubscribeWithLocale:(int) locale categories:(NSSet *)categories completion:(void (^)(NSError *))completion {
            NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];
            [defaults setValue:[categories allObjects] forKey:@"BreakingNewsCategories"];
            [defaults setInteger:locale forKey:@"BreakingNewsLocale"];
            [defaults synchronize];

            [self subscribeWithLocale: locale categories:categories completion:completion];
        }

    Valitse Muokkaa *tilaa* menetelmä sisältämään aluekohtaisia asetuksia:

        - (void) subscribeWithLocale: (int) locale categories:(NSSet *)categories completion:(void (^)(NSError *))completion{
            SBNotificationHub* hub = [[SBNotificationHub alloc] initWithConnectionString:@"<connection string>" notificationHubPath:@"<hub name>"];

            NSString* localeString;
            switch (locale) {
                case 0:
                    localeString = @"English";
                    break;
                case 1:
                    localeString = @"French";
                    break;
                case 2:
                    localeString = @"Mandarin";
                    break;
            }

            NSString* template = [NSString stringWithFormat:@"{\"aps\":{\"alert\":\"$(News_%@)\"},\"inAppMessage\":\"$(News_%@)\"}", localeString, localeString];

            [hub registerTemplateWithDeviceToken:self.deviceToken name:@"localizednewsTemplate" jsonBodyTemplate:template expiryTemplate:@"0" tags:categories completion:completion];
        }

    Huomaa, miten menetelmä *registerTemplateWithDeviceToken*, nyt on käytössä *registerNativeWithDeviceToken*sijaan. Kun olemme Rekisteröidy mallia on antamaan json mallia ja mallin nimi (Katso monipuolisimmat on hyvä rekisteröidä erilaisia malleja). Varmista, että Rekisteröi luokat tunnisteita, kuten haluamme Varmista, että vastaanottamaan ne uutisia notifciations.

    Lisää menetelmä noutaa käyttäjän oletusasetukset aluekohtaisia asetuksia:

        - (int) retrieveLocale {
            NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];

            int locale = [defaults integerForKey:@"BreakingNewsLocale"];

            return locale < 0?0:locale;
        }

2. Nyt kun on muokattu Microsoftin ilmoitukset-luokka, on on Varmista, että Microsoftin ViewController on uusi UISegmentControl käyttö. Lisää seuraava rivi *viewDidLoad* menetelmä, varmista, että Näytä kieltä, jota ole valittuna:

        self.Locale.selectedSegmentIndex = [notifications retrieveLocale];

    *Tilaa* , tavan muuttaminen *storeCategoriesAndSubscribe* puhelu seuraavankaltaiselta:

        [notifications storeCategoriesAndSubscribeWithLocale: self.Locale.selectedSegmentIndex categories:[NSSet setWithArray:categories] completion: ^(NSError* error) {
            if (!error) {
                UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Notification" message:
                                      @"Subscribed!" delegate:nil cancelButtonTitle:
                                      @"OK" otherButtonTitles:nil, nil];
                [alert show];
            } else {
                NSLog(@"Error subscribing: %@", error);
            }
        }];

3. Lopuksi täytyy päivittää oman AppDelegate.m *didRegisterForRemoteNotificationsWithDeviceToken* menetelmä niin, että voit päivittää rekisteröinti oikein, kun sovellus käynnistyy. Muuttaa *tilaa* menetelmän ilmoitusten kanssa seuraavasti:

        NSSet* categories = [self.notifications retrieveCategories];
        int locale = [self.notifications retrieveLocale];
        [self.notifications subscribeWithLocale: locale categories:categories completion:^(NSError* error) {
            if (error != nil) {
                NSLog(@"Error registering for notifications: %@", error);
            }
        }];

##<a name="optional-send-localized-template-notifications-from-net-console-app"></a>(valinnainen) Lokalisoitu mallin ilmoitukset lähetetään .NET console-sovelluksesta.

[AZURE.INCLUDE [notification-hubs-localized-back-end](../../includes/notification-hubs-localized-back-end.md)]



##<a name="optional-send-localized-template-notifications-from-the-device"></a>(valinnainen) Lokalisoitu mallin ilmoitukset lähetetään laitteesta

Jos saat Visual Studio käyttämään tai haluat vain Testaa lokalisoitu malli-ilmoitusten lähettäminen suoraan laitteessa-sovelluksesta.  Voit tehdä yksinkertaisia lokalisoitu mallia parametrit `SendNotificationRESTAPI` on määritetty edellisessä opetusohjelmassa-menetelmää.

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
            json = [NSString stringWithFormat:@"{\"messageParam\":\"Breaking %@ News : %@\","
                    \"News_English\":\"Breaking %@ News in English : %@\","
                    \"News_French\":\"Breaking %@ News in French : %@\","
                    \"News_Mandarin\":\"Breaking %@ News in Mandarin : %@\","
                    categoryTag, self.notificationMessage.text,
                    categoryTag, self.notificationMessage.text,  // insert English localized news here
                    categoryTag, self.notificationMessage.text,  // insert French localized news here
                    categoryTag, self.notificationMessage.text]; // insert Mandarin localized news here

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




## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja mallien avulla on artikkelissa:

- [Ilmoita käyttäjille, joilla ilmoituksen keskittimet: ASP.NET]
- [Ilmoita käyttäjille, joilla ilmoituksen keskittimet: Mobile-palvelut]






<!-- Images. -->

[13]: ./media/notification-hubs-ios-send-localized-breaking-news/ios_localized1.png
[14]: ./media/notification-hubs-ios-send-localized-breaking-news/ios_localized2.png






<!-- URLs. -->
[How To: Service Bus Notification Hubs (iOS Apps)]: http://msdn.microsoft.com/library/jj927168.aspx
[Ilmoitus keskittimet avulla voit lähettää uutisia]: /manage/services/notification-hubs/breaking-news-ios
[Mobile Service]: /develop/mobile/tutorials/get-started
[Ilmoita käyttäjille, joilla ilmoituksen keskittimet: ASP.NET]: /manage/services/notification-hubs/notify-users-aspnet
[Ilmoita käyttäjille, joilla ilmoituksen keskittimet: Mobile-palvelut]: /manage/services/notification-hubs/notify-users
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[Get started with Mobile Services]: /develop/mobile/tutorials/get-started/#create-new-service
[Get started with data]: /develop/mobile/tutorials/get-started-with-data-ios
[Get started with authentication]: /develop/mobile/tutorials/get-started-with-users-ios
[Get started with push notifications]: /develop/mobile/tutorials/get-started-with-push-ios
[Push notifications to app users]: /develop/mobile/tutorials/push-notifications-to-users-ios
[Authorize users with scripts]: /develop/mobile/tutorials/authorize-users-in-scripts-ios
[JavaScript and HTML]: ../get-started-with-push-js.md

[Windows Developer Preview registration steps for Mobile Services]: ../mobile-services-windows-developer-preview-registration.md
[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for iOS]: http://msdn.microsoft.com/library/jj927168.aspx
