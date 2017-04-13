<properties
    pageTitle="Azure ilmoituksen keskittimet suojatun Push"
    description="Lue, miten voit lähettää suojatun push-ilmoitukset Azure iOS-sovelluksen. MALLIKOODEJA kirjoitettu tavoite-C- ja C#."
    documentationCenter="ios"
    authors="ysxu"
    manager="erikre"
    editor=""
    services="notification-hubs"/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

#<a name="azure-notification-hubs-secure-push"></a>Azure ilmoituksen keskittimet suojatun Push

> [AZURE.SELECTOR]
- [Windowsin Universal](notification-hubs-aspnet-backend-windows-dotnet-wns-secure-push-notification.md)
- [iOS](notification-hubs-aspnet-backend-ios-push-apple-apns-secure-notification.md)
- [Android-laitteeseen](notification-hubs-aspnet-backend-android-secure-google-gcm-push-notification.md)


##<a name="overview"></a>Yleiskatsaus

Push-ilmoituksen tuki Microsoft Azure avulla voit käyttää helposti käytettävällä, useita, skaalata ulos push-infrastruktuuria, mikä vähentää huomattavasti yksinkertaistaa push-ilmoitukset mobile ympäristöjen kuluttaja- ja enterprise-sovellusten käyttöönoton.

Vuoksi säädösten tai suojauksen rajoitukset, joskus sovelluksen halutessasi lisätä jotain ilmoituksessa, joka voidaan lähettää vakio push-ilmoituksen infrastruktuurin kautta. Tässä opetusohjelmassa kerrotaan, miten saavuttamiseksi on sama kuvaus lähettämällä luottamuksellisia tietoja asiakaslaitteen ja sovelluksen Taustajärjestelmä välillä suojatun, todennetun-yhteyden kautta.

Korkean tason kulun on seuraavanlainen:

1. -Sovelluksen taustatietokantaan:
    - Säilöjen suojatun paketti-taustatietokannan.
    - Lähettää ilmoituksen tunnus (suojattu mitään tietoja ei lähetetä) laitteeseen.
2. Sovelluksen laitteeseen, kun saat ilmoituksen:
    - Laitteen yhteystietojen pyytää suojatun sisällön taustatietokannan.
    - Sovellus voidaan näyttää tiedot ilmoituksen laitteeseen.

On tärkeää muistaa, että edellisen kulun (ja tässä opetusohjelmassa) oletetaan, että laitteen tallentaa todennus-tunnuksen paikalliseen säilöön sen jälkeen, kun käyttäjä kirjautuu. Tämä takaa laitteen voit hakea ilmoituksen suojatun sisällön käyttäminen tunnuksessa kokonaan sujuvan kokemuksen. Jos sovellusta ei Tallenna todennus tunnusten laitteeseen tai näiden tunnusten on päättynyt, laite-sovelluksen, kun saat ilmoituksen pitäisi näkyä yleinen ilmoituksen käyttäjältä, Käynnistä sovellus. Valitse sovellus todentaa käyttäjän sekä näkyy ilmoitus-paketti.

Suojattu Push Tässä opetusohjelmassa näytetään push-ilmoituksen lähettäminen suojatusti. Opetusohjelman muodostaa [Ilmoita käyttäjille](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) , opetusohjelma, jotta on suoritettava vaiheet ensin, että opetusohjelmassa.

> [AZURE.NOTE] Tässä opetusohjelmassa oletetaan, että olet luonut ja määrittänyt ilmoitus-toiminnossa kuvatulla tavalla [Ilmoituksen keskittimet (iOS) käytön aloittaminen](notification-hubs-ios-apple-push-notification-apns-get-started.md).

[AZURE.INCLUDE [notification-hubs-aspnet-backend-securepush](../../includes/notification-hubs-aspnet-backend-securepush.md)]

## <a name="modify-the-ios-project"></a>Muokkaa iOS-projekti

Nyt kun olet muokannut oman sovelluksen taustatietokantaan lähetetään vain ilmoituksen *tunnus* , sinulla muuttamiseen iOS-sovellukseen Käsittele kyseiseen ilmoituksen ja soittaa takaisin hakemiseen suojatun viestin näytetään oman taustatietokannan.

Tämän tavoitteen saavuttamiseksi on kirjoittaa logiikan suojatun sisällön noutaminen app taustatietokannan.

1. Varmista **AppDelegate.m**sovelluksen rekisteri hiljainen ilmoituksissa, joten käsittelee ilmoituksen tunnus lähettämiin taustaan. Voit lisätä didFinishLaunchingWithOptions **UIRemoteNotificationTypeNewsstandContentAvailability** -vaihtoehto:

        [[UIApplication sharedApplication] registerForRemoteNotificationTypes: UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeNewsstandContentAvailability];

2. Lisää oman **AppDelegate.m** käyttöönotto-osassa yläreunassa, seuraava ilmoitus:

        @interface AppDelegate ()
        - (void) retrieveSecurePayloadWithId:(int)payloadId completion: (void(^)(NSString*, NSError*)) completion;
        @end

3. Lisää käyttöönotto-osassa seuraava koodi, korvaaminen paikkamerkin `{back-end endpoint}` taustatietokantaan oman saatu aiemmin päätepisteen kanssa:

```
        NSString *const GetNotificationEndpoint = @"{back-end endpoint}/api/notifications";

        - (void) retrieveSecurePayloadWithId:(int)payloadId completion: (void(^)(NSString*, NSError*)) completion;
        {
            // check if authenticated
            ANHViewController* rvc = (ANHViewController*) self.window.rootViewController;
            NSString* authenticationHeader = rvc.registerClient.authenticationHeader;
            if (!authenticationHeader) return;


            NSURLSession* session = [NSURLSession
                                     sessionWithConfiguration:[NSURLSessionConfiguration defaultSessionConfiguration]
                                     delegate:nil
                                     delegateQueue:nil];


            NSURL* requestURL = [NSURL URLWithString:[NSString stringWithFormat:@"%@/%d", GetNotificationEndpoint, payloadId]];
            NSMutableURLRequest* request = [NSMutableURLRequest requestWithURL:requestURL];
            [request setHTTPMethod:@"GET"];
            NSString* authorizationHeaderValue = [NSString stringWithFormat:@"Basic %@", authenticationHeader];
            [request setValue:authorizationHeaderValue forHTTPHeaderField:@"Authorization"];

            NSURLSessionDataTask* dataTask = [session dataTaskWithRequest:request completionHandler:^(NSData *data, NSURLResponse *response, NSError *error) {
                NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) response;
                if (!error && httpResponse.statusCode == 200)
                {
                    NSLog(@"Received secure payload: %@", [[NSString alloc] initWithData:data encoding:NSUTF8StringEncoding]);

                    NSMutableDictionary *json = [NSJSONSerialization JSONObjectWithData:data options: NSJSONReadingMutableContainers error: &error];

                    completion([json objectForKey:@"Payload"], nil);
                }
                else
                {
                    NSLog(@"Error status: %ld, request: %@", (long)httpResponse.statusCode, error);
                    if (error)
                        completion(nil, error);
                    else {
                        completion(nil, [NSError errorWithDomain:@"APICall" code:httpResponse.statusCode userInfo:nil]);
                    }
                }
            }];
            [dataTask resume];
        }
```

    This method calls your app back-end to retrieve the notification content using the credentials stored in the shared preferences.

4. Nyt on käsittelemään saapuvia ilmoituksen ja edellä kuvattuja avulla voit hakea näytettävä sisältö. Ensin että on otettava iOS-sovelluksen suorittamisen taustalla, kun push-ilmoituksen. **XCode**-sovelluksen projektin vasemman ruudun ja valitse tärkeimmät app kohde central-ruudusta **kohteet** -osassa.

5. Valitse **Ominaisuudet** -välilehti, Keski-ruudun yläreunassa ja **Remote ilmoitukset** -valintaruutu.

    ![][IOS1]


6. Lisää **AppDelegate.m** käsittelemään push-ilmoitukset seuraavasti:

        -(void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult))completionHandler
        {
            NSLog(@"%@", userInfo);

            [self retrieveSecurePayloadWithId:[[userInfo objectForKey:@"secureId"] intValue] completion:^(NSString * payload, NSError *error) {
                if (!error) {
                    // show local notification
                    UILocalNotification* localNotification = [[UILocalNotification alloc] init];
                    localNotification.fireDate = [NSDate dateWithTimeIntervalSinceNow:0];
                    localNotification.alertBody = payload;
                    localNotification.timeZone = [NSTimeZone defaultTimeZone];
                    [[UIApplication sharedApplication] scheduleLocalNotification:localNotification];

                    completionHandler(UIBackgroundFetchResultNewData);
                } else {
                    completionHandler(UIBackgroundFetchResultFailed);
                }
            }];

        }

    Huomaa, että se on suositeltavampaa käsittelemään ominaisuus puuttuu todennus-otsikon tai hylättäväksi tapauksissa sitten taustatietokantatiedosto mukaan. Tällaisissa tapauksissa käsittely määräytyvät enimmäkseen kohde käyttökokemusta. Yksi vaihtoehto on yleinen kehote ilmoituksen, että käyttäjä todennetaan hakemiseen ilmoituksen näyttämiseen.

## <a name="run-the-application"></a>Suorita sovellus

Suorita sovellus, toimi seuraavasti:

1. Suorita sovellus XCode, fyysinen iOS-laitteessa (push-ilmoitukset eivät toimi simulator).

2. Kirjoita käyttäjänimi ja salasana iOS-sovelluksen Käyttöliittymässä. Nämä voi olla mikä tahansa merkkijono, mutta niiden on oltava sama arvo.

3. Käyttöliittymän iOS-sovelluksessa valitsemalla **Kirjaudu sisään**. Valitse **Lähetä push**. Raportissa pitäisi näkyä suojatun ilmoituksessa, joka on näkyy ilmoitus Centerissä.

[IOS1]: ./media/notification-hubs-aspnet-backend-ios-secure-push/secure-push-ios-1.png
