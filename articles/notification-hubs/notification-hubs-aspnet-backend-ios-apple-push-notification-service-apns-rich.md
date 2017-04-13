<properties
    pageTitle="Azure ilmoituksen keskittimet RTF Push"
    description="Lue, miten voit lähettää monipuolisia push-ilmoitukset Azure iOS-sovelluksen. MALLIKOODEJA kirjoitettu tavoite-C- ja C#."
    documentationCenter="ios"
    services="notification-hubs"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

#<a name="azure-notification-hubs-rich-push"></a>Azure ilmoituksen keskittimet RTF Push


##<a name="overview"></a>Yleiskatsaus

Avoimen käyttäjät, joilla on pikaviestien monipuolisia sisältö-sovelluksen hyvä push muutakin kuin vain teksti. Ilmoitukset edistää käyttäjän toimintoja ja esitä sisältöä, kuten URL-osoitteet, äänet tai kuvia ja kuponki. Tässä opetusohjelmassa muodostaa [Ilmoita käyttäjille](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) aiheeseen ja kerrotaan, miten voit lähettää push-ilmoitukset, joka yhdistää paketteja (esimerkiksi kuva).


Tässä opetusohjelmassa on yhteensopiva iOS 7 ja 8.

  ![][IOS1]

Ylätasolla:

1. Sovelluksen: taustaan
    - Tallentaa RTF-paketti (Tässä tapauksessa kuva) Taustajärjestelmä tietokanta tai paikallinen tallennustilaan
    - Lähettää laitteeseen RTF tämä ilmoitus tunnus
2. Sovelluksen laitteeseen:
    - Yhteystietojen pyytää monipuolisia paketti se saa tunnuksella taustaan
    - Lähettää käyttäjille ilmoituksia laitteeseen, kun tietojen noutamista on suoritettu ja näyttää tiedot heti, kun käyttäjät napauttamalla lisätietoja


## <a name="webapi-project"></a>WebAPI projektin

1. Avaa Visual Studion **AppBackend** projekti, jonka loit opetusohjelmassa [Ilmoita käyttäjille](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) .
2. Hanki haluat ilmoittaa käyttäjille, joilla ja sijoita se **img** kansion projektin hakemistossa kuva.
3. Valitse **Näytä kaikki tiedostot** Solution Explorerissa ja hiiren kakkospainikkeella kansiota, **Sisällytä**projektiin.
4. Muuttaa valitun kuvan ominaisuudet-ikkuna sen luominen-toiminto **Upotetun**resurssiin.

    ![][IOS2]

5. Lisää seuraavat **Notifications.cs**lauseella:

        using System.Reflection;

6. Päivitä koko **ilmoitukset** -luokan seuraava koodi. Muista paikkamerkkien korvaaminen ilmoituksen keskittimeen tunnistetiedot ja kuvatiedoston nimi.

        public class Notification {
            public int Id { get; set; }
            // Initial notification message to display to users
            public string Message { get; set; }
            // Type of rich payload (developer-defined)
            public string RichType { get; set; }
            public string Payload { get; set; }
            public bool Read { get; set; }
        }

        public class Notifications {
            public static Notifications Instance = new Notifications();

            private List<Notification> notifications = new List<Notification>();

            public NotificationHubClient Hub { get; set; }

            private Notifications() {
                // Placeholders: replace with the connection string (with full access) for your notification hub and the hub name from the Azure Classics Portal
                Hub = NotificationHubClient.CreateClientFromConnectionString("{conn string with full access}",  "{hub name}");
            }

            public Notification CreateNotification(string message, string richType, string payload) {
                var notification = new Notification() {
                    Id = notifications.Count,
                    Message = message,
                    RichType = richType,
                    Payload = payload,
                    Read = false
                };

                notifications.Add(notification);

                return notification;
            }

            public Stream ReadImage(int id) {
                var assembly = Assembly.GetExecutingAssembly();
                // Placeholder: image file name (for example, logo.png).
                return assembly.GetManifestResourceStream("AppBackend.img.{logo.png}");
            }
        }

    >[AZURE.NOTE]  (valinnainen) Lisätietoja saat lisätietoja siitä, miten voit lisätä ja hankkia projektiresurssit [upottamisesta ja resurssien avulla Visual C#](http://support.microsoft.com/kb/319292) .

7. **NotificationsController.cs**-uudelleenmääritys **NotificationsController** seuraavat katkelmat kanssa. Lähettää laitteen ensimmäisen hiljainen monipuolisia ilmoituksen tunnus, ja sallii asiakkaan noutamista kuva:

        // Return http response with image binary
        public HttpResponseMessage Get(int id) {
            var stream = Notifications.Instance.ReadImage(id);

            var result = new HttpResponseMessage(HttpStatusCode.OK);
            result.Content = new StreamContent(stream);
            // Switch in your image extension for "png"
            result.Content.Headers.ContentType = new System.Net.Http.Headers.MediaTypeHeaderValue("image/{png}");

            return result;
        }

        // Create rich notification and send initial silent notification (containing id) to client
        public async Task<HttpResponseMessage> Post() {
            // Replace the placeholder with image file name
            var richNotificationInTheBackend = Notifications.Instance.CreateNotification("Check this image out!", "img",  "{logo.png}");

            var usernameTag = "username:" + HttpContext.Current.User.Identity.Name;

            // Silent notification with content available
            var aboutUser = "{\"aps\": {\"content-available\": 1, \"sound\":\"\"}, \"richId\": \"" + richNotificationInTheBackend.Id.ToString() + "\",  \"richMessage\": \"" + richNotificationInTheBackend.Message + "\", \"richType\": \"" + richNotificationInTheBackend.RichType + "\"}";

            // Send notification to apns
            await Notifications.Instance.Hub.SendAppleNativeNotificationAsync(aboutUser, usernameTag);

            return Request.CreateResponse(HttpStatusCode.OK);
        }

8. Nyt on uudelleen tämän sovelluksen käyttöönotto Azure sivustoon, siirrä käytettävissä olevat kaikissa laitteissa. Napsauta **AppBackend** projektin hiiren kakkospainikkeella ja valitse **Julkaise**.

9. Valitse Julkaise kohde Azure sivuston. Kirjaudu sisään Azure-tili ja valitse olemassa oleva vai uusi sivusto ja Merkitse muistiin **linkin URL-osoite** -ominaisuuden **yhteys** -välilehdessä. Viitataan tätä URL-Osoitetta *Taustajärjestelmä päätepisteen* jäljempänä tässä opetusohjelmassa olevassa. Valitse **Julkaise**.

## <a name="modify-the-ios-project"></a>Muokkaa iOS-projekti

Nyt kun olet muokannut sovelluksen-taustatietokannan lähetetään vain ilmoituksen *tunnus* , iOS-sovellukseen käsitellä tunnus ja Nouda monipuolisia viestin taustasta oman muuttuu.

1. Avaa iOS projektin ja **kohteet** -osassa tärkeimmät app kohde siirtymällä remote ilmoitukset käyttöön.

2. Valitse **Ominaisuudet**, ota käyttöön **Taustan tilat**ja **Remote ilmoitukset** -valintaruutu.

    ![][IOS3]

3. Siirry **Main.storyboard**ja varmista, että käytössä on [Ilmoitettava käyttäjän](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) opetusohjelma-näkymän ohjain (vakuudesta näkymän ohjauskoneen kuin tässä opetusohjelmassa).

4. Lisää **Siirtyminen ohjauskoneen** oman kuvakäsikirjoitus ja ohjausobjektin vetämällä Home näkymän ohjauskoneen, jotta se siirtymisruudun **pääkansion näkymä** . Varmista, että määritteet tarkastaminen **On alkuperäinen näkymän ohjauskoneen** on valittuna siirtyminen ohjaimen vain.

5. Lisää **Näkymä ohjauskoneen** kuvakäsikirjoitukseen ja lisää luodun **Näkymän kuvan**. Tällä sivulla, käyttäjät näkevät, kun he haluavat lisätietoja valitsemalla notifiication. Oman kuvakäsikirjoitus pitäisi näyttää seuraavasti:

    ![][IOS4]

6. Napsauta kuvakäsikirjoitus **Näytä ohjauskoneen** ja varmista, että se on **homeViewController** sen **Mukautettu luokka** ja **Kuvakäsikirjoitus tunnus** kohdassa käyttäjätiedot tarkastaminen.

7. Toimi samoin kuva näkymän ohjauskoneen, jos **imageViewController**nimellä.

8. Luo sitten uusi näkymä-ohjain luokka liittyvä **imageViewController** käsittelemään juuri luomasi Käyttöliittymän.

9. Lisää **imageViewController.h**ohjain käyttöliittymän ilmoitukset seuraavasti. Varmista, että ohjausobjektin vetämällä kuvakäsikirjoitus kuva-näkymässä voit linkittää kaksi nämä ominaisuudet:

        @property (weak, nonatomic) IBOutlet UIImageView *myImage;
        @property (strong) UIImage* imagePayload;

10. Lisää **imageViewController.m** **viewDidload**lopussa seuraavasti:

        // Display the UI Image in UI Image View
        [self.myImage setImage:self.imagePayload];

11. Tuoda luomasi kuva ohjauskoneen **AppDelegate.m**:

        #import "imageViewController.h"

12. Lisää LIP-osa, jossa seuraava ilmoitus:

        @interface AppDelegate ()

        @property UIImage* imagePayload;
        @property NSDictionary* userInfo;
        @property BOOL iOS8;

        // Obtain content from backend with notification id
        - (void)retrieveRichImageWithId:(int)richId completion: (void(^)(NSError*)) completion;

        // Redirect to Image View Controller after notification interaction
        - (void)redirectToImageViewWithImage: (UIImage *)img;

        @end

13. **AppDelegate**, varmista, että sovelluksesi Rekisteröi hiljainen ilmoituksia **sovelluksen: didFinishLaunchingWithOptions**:

        // Software version
        self.iOS8 = [[UIApplication sharedApplication] respondsToSelector:@selector(registerUserNotificationSettings:)] && [[UIApplication sharedApplication] respondsToSelector:@selector(registerForRemoteNotifications)];

        // Register for remote notifications for iOS8 and previous versions
        if (self.iOS8) {
            NSLog(@"This device is running with iOS8.");

            // Action
            UIMutableUserNotificationAction *richPushAction = [[UIMutableUserNotificationAction alloc] init];
            richPushAction.identifier = @"richPushMore";
            richPushAction.activationMode = UIUserNotificationActivationModeForeground;
            richPushAction.authenticationRequired = NO;
            richPushAction.title = @"More";

            // Notification category
            UIMutableUserNotificationCategory* richPushCategory = [[UIMutableUserNotificationCategory alloc] init];
            richPushCategory.identifier = @"richPush";
            [richPushCategory setActions:@[richPushAction] forContext:UIUserNotificationActionContextDefault];

            // Notification categories
            NSSet* richPushCategories = [NSSet setWithObjects:richPushCategory, nil];


            UIUserNotificationSettings *settings = [UIUserNotificationSettings settingsForTypes:UIUserNotificationTypeSound |
                                                    UIUserNotificationTypeAlert |
                                                    UIUserNotificationTypeBadge
                                                                                     categories:richPushCategories];

            [[UIApplication sharedApplication] registerUserNotificationSettings:settings];
            [[UIApplication sharedApplication] registerForRemoteNotifications];

        }
        else {
            // Previous iOS versions
            NSLog(@"This device is running with iOS7 or earlier versions.");

            [[UIApplication sharedApplication] registerForRemoteNotificationTypes: UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeNewsstandContentAvailability];
        }

        return YES;

14. Subsitute- **sovellus: didRegisterForRemoteNotificationsWithDeviceToken** seuraavat toteutusta suorittavan kuvakäsikirjoitus Käyttöliittymän muuttuu huomioon:

        // Access navigation controller which is at the root of window
        UINavigationController *nc = (UINavigationController *)self.window.rootViewController;
        // Get home view controller from stack on navigation controller
        homeViewController *hvc = (homeViewController *)[nc.viewControllers objectAtIndex:0];
        hvc.deviceToken = deviceToken;

15. Lisää seuraavista menetelmistä **AppDelegate.m** hakemaan kuvan päätepiste ja Lähetä paikallisen ilmoitus, kun haku on suoritettu loppuun. Varmista, että voit korvata paikkamerkin `{backend endpoint}` Taustajärjestelmä päätepisteen kanssa:

        NSString *const GetNotificationEndpoint = @"{backend endpoint}/api/notifications";

        // Helper: retrieve notification content from backend with rich notification id
        - (void)retrieveRichImageWithId:(int)richId completion: (void(^)(NSError*)) completion {
            UINavigationController *nc = (UINavigationController *)self.window.rootViewController;
            homeViewController *hvc = (homeViewController *)[nc.viewControllers objectAtIndex:0];
            NSString* authenticationHeader = hvc.registerClient.authenticationHeader;
            // Check if authenticated
            if (!authenticationHeader) return;

            NSURLSession* session = [NSURLSession
                                     sessionWithConfiguration:[NSURLSessionConfiguration defaultSessionConfiguration]
                                     delegate:nil
                                     delegateQueue:nil];

            NSURL* requestURL = [NSURL URLWithString:[NSString stringWithFormat:@"%@/%d", GetNotificationEndpoint, richId]];
            NSMutableURLRequest* request = [NSMutableURLRequest requestWithURL:requestURL];
            [request setHTTPMethod:@"GET"];
            NSString* authorizationHeaderValue = [NSString stringWithFormat:@"Basic %@", authenticationHeader];
            [request setValue:authorizationHeaderValue forHTTPHeaderField:@"Authorization"];

            NSURLSessionDataTask* dataTask = [session dataTaskWithRequest:request completionHandler:^(NSData *data, NSURLResponse *response, NSError *error) {

                NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) response;
                if (!error && httpResponse.statusCode == 200) {
                    // From NSData to UIImage
                    self.imagePayload = [UIImage imageWithData:data];

                    completion(nil);
                }
                else {
                    NSLog(@"Error status: %ld, request: %@", (long)httpResponse.statusCode, error);
                    if (error)
                        completion(error);
                    else {
                        completion([NSError errorWithDomain:@"APICall" code:httpResponse.statusCode userInfo:nil]);
                    }
                }
            }];
            [dataTask resume];
        }

        // Handle silent push notifications when id is sent from backend
        - (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult result))handler {
            self.userInfo = userInfo;
            int richId = [[self.userInfo objectForKey:@"richId"] intValue];
            NSString* richType = [self.userInfo objectForKey:@"richType"];

            // Retrieve image data
            if ([richType isEqualToString:@"img"]) {  
                [self retrieveRichImageWithId:richId completion:^(NSError* error) {
                    if (!error){
                        // Send local notification
                        UILocalNotification* localNotification = [[UILocalNotification alloc] init];

                        // "5" is arbitrary here to give you enough time to quit out of the app and receive push notifications
                        localNotification.fireDate = [NSDate dateWithTimeIntervalSinceNow:5];
                        localNotification.userInfo = self.userInfo;
                        localNotification.alertBody = [self.userInfo objectForKey:@"richMessage"];
                        localNotification.timeZone = [NSTimeZone defaultTimeZone];

                        // iOS8 categories
                        if (self.iOS8) {
                            localNotification.category = @"richPush";
                        }

                        [[UIApplication sharedApplication] scheduleLocalNotification:localNotification];

                        handler(UIBackgroundFetchResultNewData);
                    }
                    else{
                        handler(UIBackgroundFetchResultFailed);
                    }
                }];
            }
            // Add "else if" here to handle more types of rich content such as url, sound files, etc.
        }

16. Käsittele paikallisen ilmoituksen edellä avaamalla määrittäminen kuva näkymän controller- **AppDelegate.m** kanssa seuraavilla tavoilla:

        // Helper: redirect users to image view controller
        - (void)redirectToImageViewWithImage: (UIImage *)img {
            UINavigationController *navigationController = (UINavigationController*) self.window.rootViewController;
            UIStoryboard *mainStoryboard = [UIStoryboard storyboardWithName:@"Main"
                                                                     bundle: nil];
            imageViewController *imgViewController = [mainStoryboard instantiateViewControllerWithIdentifier: @"imageViewController"];
            // Pass data/image to image view controller
            imgViewController.imagePayload = img;

            // Redirect
            [navigationController pushViewController:imgViewController animated:YES];
        }

        // Handle local notification sent above in didReceiveRemoteNotification
        - (void)application:(UIApplication *)application didReceiveLocalNotification:(UILocalNotification *)notification {
            if (application.applicationState == UIApplicationStateActive) {
                // Show in-app alert with an extra "more" button
                UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Notification" message:notification.alertBody delegate:self cancelButtonTitle:@"OK" otherButtonTitles:@"More", nil];

                [alert show];
            }
            // App becomes active from user's tap on notification
            else {
                [self redirectToImageViewWithImage:self.imagePayload];
            }
        }

        // Handle buttons in in-app alerts and redirect with data/image
        - (void)alertView:(UIAlertView *)alertView clickedButtonAtIndex:(NSInteger)buttonIndex {
            // Handle "more" button
            if (buttonIndex == 1)
            {
                [self redirectToImageViewWithImage:self.imagePayload];
            }
            // Add "else if" here to handle more buttons
        }

        // Handle notification setting actions in iOS8
        - (void)application:(UIApplication *)application handleActionWithIdentifier:(NSString *)identifier forLocalNotification:(UILocalNotification *)notification completionHandler:(void (^)())completionHandler {
            // Handle richPush related buttons
            if ([identifier isEqualToString:@"richPushMore"]) {
                [self redirectToImageViewWithImage:self.imagePayload];
            }
            completionHandler();
        }

## <a name="run-the-application"></a>Suorita sovellus

1. Suorita sovellus XCode, fyysinen iOS-laitteessa (push-ilmoitukset eivät toimi simulator).

2. IOS-sovelluksessa Käyttöliittymän Syötä todennusta varten käyttäjänimi ja salasana on sama arvo ja valitse **Kirjaudu sisään**.

3. Valitse **Lähetä push** ja näkyviin tulee sovelluskohtaisesta-ilmoituksen. Jos valitset **Lisää**, saatetaan app Taustajärjestelmä valitsit kuva.

4. Voit myös napsauttaa **Lähetä push** ja paina heti laitteesi koti-painiketta. Hetken saat ilmoituksen push. Jos napauttamalla sitä tai valitse Lisää, tuotava sovelluksen ja rich kuva sisällön.


[IOS1]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-1.png
[IOS2]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-2.png
[IOS3]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-3.png
[IOS4]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-4.png
