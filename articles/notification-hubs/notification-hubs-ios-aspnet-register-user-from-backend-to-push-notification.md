<properties
    pageTitle="Push-ilmoitukset nykyisen käyttäjän rekisteröityä käyttämällä verkko-Ohjelmointirajapinnan | Microsoft Azure"
    description="Lue, miten voit pyytää push-ilmoitusten rekisteröinnissä iOS-sovelluksessa, jossa Azure ilmoituksen keskittimet, kun registeration suoritetaan ASP.NET-verkko-Ohjelmointirajapinnan."
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
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

# <a name="register-the-current-user-for-push-notifications-by-using-aspnet"></a>Rekisteröi nykyisen käyttäjän ASP.NET käyttämällä push-ilmoitukset

> [AZURE.SELECTOR]
- [iOS](notification-hubs-ios-aspnet-register-user-from-backend-to-push-notification.md)



##<a name="overview"></a>Yleiskatsaus

Tässä ohjeaiheessa esitellään, miten pyytää Azure ilmoituksen keskittimet push ilmoituksen rekisteröinti, kun rekisteröinti suoritetaan ASP.NET-verkko-Ohjelmointirajapinnan. Tässä ohjeaiheessa laajentaa opetusohjelman [Ilmoita käyttäjille, joilla ilmoituksen keskittimet]. Olet jo tehnyt tarvittavat vaiheet kyseisen opetusohjelmassa luominen todennetut mobiilipalvelu. Saat lisätietoja Ilmoita käyttäjille skenaarion [Ilmoita käyttäjille, joilla ilmoituksen keskittimet].

##<a name="update-your-app"></a>Sovelluksen päivittäminen  

1. Lisää oman MainStoryboard_iPhone.storyboard objektin kirjastosta seuraavat osat:

    + **Otsikko**: "Siirtää käyttäjä, jolla ilmoitus keskittimet"
    + **Otsikko**: "InstallationId"
    + **Otsikko**: "Käyttäjä"
    + **Tekstikentän**: "Käyttäjä"
    + **Otsikko**: "Salasana"
    + **Tekstikentän**: "Salasana"
    + **Painike**: "Kirjautuminen"

    Tässä vaiheessa oman kuvakäsikirjoitus näyttää seuraavalta:

    ![][0]

2. Avustaja-editorissa luominen pistorasioita switched ohjausobjektien ja kutsua niitä, tekstikenttiä yhteydessä näkymän ohjauskoneen (edustajan) ja luo **toiminnon** **Sisäänkirjautuminen** -painikkeesta.

    ![][1]

    BreakingNewsViewController.h tiedostojen pitäisi nyt näkyä seuraava koodi:

        @property (weak, nonatomic) IBOutlet UILabel *installationId;
        @property (weak, nonatomic) IBOutlet UITextField *User;
        @property (weak, nonatomic) IBOutlet UITextField *Password;

        - (IBAction)login:(id)sender;

5. Luo nimetty **DeviceInfo**luokka ja kopioi seuraava koodi-tiedoston DeviceInfo.h liittymän osaan:

        @property (readonly, nonatomic) NSString* installationId;
        @property (nonatomic) NSData* deviceToken;

6. Kopioi seuraava koodi DeviceInfo.m tiedoston käyttöönotto-osassa:

            @synthesize installationId = _installationId;

            - (id)init {
                if (!(self = [super init]))
                    return nil;

                NSUserDefaults *defaults = [NSUserDefaults standardUserDefaults];
                _installationId = [defaults stringForKey:@"PushToUserInstallationId"];
                if(!_installationId) {
                    CFUUIDRef newUUID = CFUUIDCreate(kCFAllocatorDefault);
                    _installationId = (__bridge_transfer NSString *)CFUUIDCreateString(kCFAllocatorDefault, newUUID);
                    CFRelease(newUUID);

                    //store the install ID so we don't generate a new one next time
                    [defaults setObject:_installationId forKey:@"PushToUserInstallationId"];
                    [defaults synchronize];
                }

                return self;
            }

            - (NSString*)getDeviceTokenInHex {
                const unsigned *tokenBytes = [[self deviceToken] bytes];
                NSString *hexToken = [NSString stringWithFormat:@"%08X%08X%08X%08X%08X%08X%08X%08X",
                                      ntohl(tokenBytes[0]), ntohl(tokenBytes[1]), ntohl(tokenBytes[2]),
                                      ntohl(tokenBytes[3]), ntohl(tokenBytes[4]), ntohl(tokenBytes[5]),
                                      ntohl(tokenBytes[6]), ntohl(tokenBytes[7])];
                return hexToken;
            }

7. Lisää seuraavat ominaisuuden Yksiarvoinen PushToUserAppDelegate.h:

        @property (strong, nonatomic) DeviceInfo* deviceInfo;

8. PushToUserAppDelegate.m **didFinishLaunchingWithOptions** -menetelmä lisää seuraava koodi:

        self.deviceInfo = [[DeviceInfo alloc] init];

        [[UIApplication sharedApplication] registerForRemoteNotificationTypes: UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound];

    Ensimmäisen rivin alustaa **DeviceInfo** Yksiarvoinen. Toiselta riviltä käynnistyy rekisteröinnin push-ilmoitukset, joka on jo esittäminen on olet jo suorittanut [Ilmoituksen keskittimet käytön aloittaminen] -opetusohjelma.

9. Menetelmä **didRegisterForRemoteNotificationsWithDeviceToken** toteuttavien oman AppDelegate PushToUserAppDelegate.m, ja lisää seuraava koodi:

        self.deviceInfo.deviceToken = deviceToken;

    Tämä asetus määrittää pyynnön laitteen tunnus.

    > [AZURE.NOTE] Tässä vaiheessa ei pitäisi olla muu koodi tällä menetelmällä. Jos sinulla on jo **registerNativeWithDeviceToken** menetelmää, joka on lisätty, kun olet suorittanut [Ilmoituksen keskittimet käytön aloittaminen](/manage/services/notification-hubs/get-started-notification-hubs-ios/) -opetusohjelma puhelun, on kommentti-kohtaa tai poistaa puhelua.

10. Lisää PushToUserAppDelegate.m-tiedostoon käsittelijä seuraavasti:

        - (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo {
            NSLog(@"%@", userInfo);
            UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Notification" message:
                                  [userInfo objectForKey:@"inAppMessage"] delegate:nil cancelButtonTitle:
                                  @"OK" otherButtonTitles:nil, nil];
            [alert show];
        }

     Tämä menetelmä näyttää Käyttöliittymän ilmoituksen, kun sovellus saa ilmoituksen sen ollessa käynnissä.

9. Avaa PushToUserViewController.m-tiedosto ja palauttaa näppäimistön seuraavat käyttöönoton:

        - (BOOL)textFieldShouldReturn:(UITextField *)theTextField {
            if (theTextField == self.User || theTextField == self.Password) {
                [theTextField resignFirstResponder];
            }
            return YES;
        }

9. **ViewDidLoad** PushToUserViewController.m tiedostossa tapaa alusta installationId otsikko seuraavasti:

        DeviceInfo* deviceInfo = [(PushToUserAppDelegate*)[[UIApplication sharedApplication]delegate] deviceInfo];
        Self.installationId.text = deviceInfo.installationId;

10. Voit lisätä PushToUserViewController.m käyttöliittymän seuraavat ominaisuudet:

        @property (readonly) NSOperationQueue* downloadQueue;
        - (NSString*)base64forData:(NSData*)theData;

11. Lisää seuraavat käyttöönoton jälkeen:

            - (NSOperationQueue *)downloadQueue {
                if (!_downloadQueue) {
                    _downloadQueue = [[NSOperationQueue alloc] init];
                    _downloadQueue.name = @"Download Queue";
                    _downloadQueue.maxConcurrentOperationCount = 1;
                }
                return _downloadQueue;
            }

            // base64 encoding
            - (NSString*)base64forData:(NSData*)theData
            {
                const uint8_t* input = (const uint8_t*)[theData bytes];
                NSInteger length = [theData length];

                static char table[] = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/=";

                NSMutableData* data = [NSMutableData dataWithLength:((length + 2) / 3) * 4];
                uint8_t* output = (uint8_t*)data.mutableBytes;

                NSInteger i;
                for (i=0; i < length; i += 3) {
                    NSInteger value = 0;
                    NSInteger j;
                    for (j = i; j < (i + 3); j++) {
                        value <<= 8;

                        if (j < length) {
                            value |= (0xFF & input[j]);
                        }
                    }

                    NSInteger theIndex = (i / 3) * 4;
                    output[theIndex + 0] =                    table[(value >> 18) & 0x3F];
                    output[theIndex + 1] =                    table[(value >> 12) & 0x3F];
                    output[theIndex + 2] = (i + 1) < length ? table[(value >> 6)  & 0x3F] : '=';
                    output[theIndex + 3] = (i + 2) < length ? table[(value >> 0)  & 0x3F] : '=';
                }

                return [[NSString alloc] initWithData:data encoding:NSASCIIStringEncoding];
            }


12. Kopioi seuraava koodi XCode luoma **Kirjautuminen** käsittely-menetelmää.

            DeviceInfo* deviceInfo = [(PushToUserAppDelegate*)[[UIApplication sharedApplication]delegate] deviceInfo];

            // build JSON
            NSString* json = [NSString stringWithFormat:@"{\"platform\":\"ios\", \"instId\":\"%@\", \"deviceToken\":\"%@\"}", deviceInfo.installationId, [deviceInfo getDeviceTokenInHex]];

            // build auth string
            NSString* authString = [NSString stringWithFormat:@"%@:%@", self.User.text, self.Password.text];

            NSMutableURLRequest* request = [NSMutableURLRequest requestWithURL:[NSURL URLWithString:@"http://nhnotifyuser.azurewebsites.net/api/register"]];
            [request setHTTPMethod:@"POST"];
            [request setHTTPBody:[json dataUsingEncoding:NSUTF8StringEncoding]];
            [request addValue:[@([json lengthOfBytesUsingEncoding:NSUTF8StringEncoding]) description] forHTTPHeaderField:@"Content-Length"];
            [request addValue:@"application/json" forHTTPHeaderField:@"Content-Type"];
            [request addValue:[NSString stringWithFormat:@"Basic %@",[self base64forData:[authString dataUsingEncoding:NSUTF8StringEncoding]]] forHTTPHeaderField:@"Authorization"];

            // connect with POST
            [NSURLConnection sendAsynchronousRequest:request queue:[self downloadQueue] completionHandler:^(NSURLResponse* response, NSData* data, NSError* error) {
                // add UIAlert depending on response.
                if (error != nil) {
                    NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*)response;
                    if ([httpResponse statusCode] == 200) {
                        UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Back-end registration" message:@"Registration successful" delegate:nil cancelButtonTitle: @"OK" otherButtonTitles:nil, nil];
                        [alert show];
                    } else {
                        NSLog(@"status: %ld", (long)[httpResponse statusCode]);
                    }
                } else {
                    NSLog(@"error: %@", error);
                }
            }];

    Tämä menetelmä noutaa sekä asennus ID-tunnus ja kanavan push-ilmoitukset ja lähettää sen, sekä laitetyyppi, todennetut verkko-Ohjelmointirajapinnan menetelmää, joka luo rekisteröinti ilmoituksen keskittimet. Verkko-Ohjelmointirajapinnan on määritetty [Ilmoita käyttäjille, joilla ilmoituksen keskittimet].

Nyt kun asiakas-sovellus on päivitetty, palaa [Ilmoita käyttäjille, joilla ilmoituksen keskittimet] ja Päivitä mobiilipalvelu, ilmoitukset lähetetään ilmoitus keskittimet käyttämällä.

<!-- Anchors. -->

<!-- Images. -->
[0]: ./media/notification-hubs-ios-aspnet-register-user-push-notifications/notification-hub-user-aspnet-ios1.png
[1]: ./media/notification-hubs-ios-aspnet-register-user-push-notifications/notification-hub-user-aspnet-ios2.png

<!-- URLs. -->
[Ilmoita käyttäjille ilmoituksen keskittimet kanssa]: /manage/services/notification-hubs/notify-users-aspnet

[Ilmoitus keskittimet käytön aloittaminen]: /manage/services/notification-hubs/get-started-notification-hubs-ios
