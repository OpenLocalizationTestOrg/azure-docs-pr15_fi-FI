<properties
    pageTitle="Push-ilmoitukset lähetetään iOS kanssa Azure ilmoituksen keskittimet | Microsoft Azure"
    description="Tässä opetusohjelmassa opit käyttämään Azure ilmoituksen keskittimet push-ilmoitukset lähetetään iOS-sovelluksen."
    services="notification-hubs"
    documentationCenter="ios"
    keywords="Push-ilmoituksen, push-ilmoitukset ios push-ilmoitukset"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="hero-article"
    ms.date="10/03/2016"
    ms.author="yuaxu"/>

# <a name="sending-push-notifications-to-ios-with-azure-notification-hubs"></a>Push-ilmoitukset lähetetään iOS Azure ilmoituksen keskittimet kanssa

[AZURE.INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

##<a name="overview"></a>Yleiskatsaus

> [AZURE.NOTE] Jotta voit suorittaa tässä opetusohjelmassa, on oltava aktiivinen Azure-tili. Jos sinulla ei ole tiliä, voit luoda ilmainen kokeiluversio tili vain muutaman minuutin. Lisätietoja on artikkelissa [Azure maksuttoman kokeiluversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-ios-get-started).

Tässä opetusohjelmassa näytetään, miten push-ilmoitukset lähetetään iOS-sovellukseen Azure ilmoituksen keskittimet avulla. Luo tyhjä iOS-sovellukseen, joka vastaanottaa push-ilmoitukset [Apple Push-ilmoituspalvelu (APN)](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html)avulla. 

Kun olet valmis, pystyt käyttämään yleislähetys push-ilmoituksia, jotka kaikki laitteet, joissa sovelluksen ilmoitus-toiminnossa.

## <a name="before-you-begin"></a>Ennen aloittamista

[AZURE.INCLUDE [notification-hubs-hero-slug](../../includes/notification-hubs-hero-slug.md)]

Tässä opetusohjelmassa valmiit koodi löytyy [GitHub](https://github.com/Azure/azure-notificationhubs-samples/tree/master/iOS/GetStartedNH/GetStarted). 

##<a name="prerequisites"></a>Edellytykset

Tässä opetusohjelmassa edellyttää seuraavaa:

+ [Mobile-palveluihin iOS SDK 1.2.4 versio]
+ [Xcode] uusin versio
+ IOS 8 (tai uudempi versio)-yhteyttä hyödyntäviin laite
+ [Apple kehittäjä-ohjelman](https://developer.apple.com/programs/) jäsenyyttä.

   > [AZURE.NOTE] Push-ilmoitusten määrittäminen koskevat vaatimukset, koska käyttöön ja testaa push-ilmoitukset fyysinen iOS-laite (iPhone tai iPad) iOS Simulator sijaan.

Noudata tässä opetusohjelmassa on kaikki muut ilmoituksen keskittimet opetusohjelmat for iOS-sovellukset.

[AZURE.INCLUDE [Notification Hubs Enable Apple Push Notifications](../../includes/notification-hubs-enable-apple-push-notifications.md)]

##<a name="configure-your-notification-hub-for-ios-push-notifications"></a>Ilmoitus-toiminnossa määrittäminen iOS push-ilmoitukset

Tässä osassa esitellään luodaan uusi ilmoitus-toiminnosta ja käyttöoikeuksien määrittäminen käyttämällä **.p12** push-sertifikaatti, jonka loit APN kautta. Jos haluat käyttää ilmoitus-toiminto, jonka olet jo luonut, voit ohittaa vaiheeseen 5.

[AZURE.INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="6">
<li>
<p><b>Asetukset</b> -sivu <b>Ilmoituksen palvelut</b> -painiketta ja valitse <b>Apple (APN)</b>. <b>Lataa</b> varmenteen ja valitse aiemmin luotu <b>.p12</b> haluamasi tiedosto. Varmista, että voit myös määrittää oikea salasana.</p>
<p>Varmista, että valitset <b>eristetyn</b> tilan, koska tämä on kehittämiseen. Käytä <b>tuotannon</b> vain, jos haluat lähettää käyttäjille, jotka ovat hankkineet kaupasta sovelluksen push-ilmoitukset.</p>
</li>
</ol>
&emsp;&emsp;![Määritä APN Azure-portaalissa](./media/notification-hubs-ios-get-started/notification-hubs-apple-config.png)

&emsp;&emsp;![APN-sertifikaatin määrittäminen Azure-portaalissa](./media/notification-hubs-ios-get-started/notification-hubs-apple-config-cert.png)



Ilmoitus-toiminnossa on nyt määritetty APN-käyttöä varten, ja sinulla on yhteysmerkkijonot rekisteröidä sovelluksen ja Lähetä push-ilmoitukset.

##<a name="connect-your-ios-app-to-notification-hubs"></a>Yhdistä ilmoituksen porttiin iOS-sovellukseen

1. Valitse Xcode iOS uuden projektin luominen- **Yhden näkymän sovelluksen** -malli.

    ![Xcode - samaa näkymää-sovellus][8]

2. Kun uuden projektin asetusvaihtoehdot, varmista, että **Tuotteen nimen** ja- **Organisaation tunnus** , jota käytit, kun määrität pikaoppaista tunnus aiemmin Apple Developer-portaalissa.

    ![Xcode - projektin asetukset][11]

3. **Kohteet**, valitse projektinimi, **Muodostamisasetukset** -välilehti ja laajenna **Koodin allekirjoittaminen käyttäjätiedot**ja määritä henkilöllisyytesi koodin allekirjoitus-kohdassa **Virheenkorjaus**. Vaihda **tasot** **Perus** **kaikki**ja määritä **Valmistelu profiilin** valmistelu profiili, jonka loit aiemmin.

    Jos et näe uusi valmistelu profiili, jonka loit Xcode, kokeile päivittää allekirjoitetun henkilöllisyytesi profiilien. **Xcode** valitsemalla valikkoriviltä **asetukset**, valitse **tili** -välilehti, **Näytä tiedot** -painiketta, valitse allekirjoitetun henkilöllisyytesi ja valitse sitten oikeassa alakulmassa Päivitä-painike.

    ![Xcode - valmistelu profiili][9]

4. Lataa [Mobile-palveluihin iOS SDK versio 1.2.4] ja unzip tiedosto. Valitse Xcode projektin hiiren kakkospainikkeella ja valitse **Tiedostojen lisääminen** -vaihtoehto, jos haluat lisätä **WindowsAzureMessaging.framework** -kansion Xcode projektin. Valitse **luettelokohteille tarvittaessa**ja valitse sitten **Lisää**.

    >[AZURE.NOTE] Ilmoitus keskittimet SDK tällä hetkellä tue bitcode Xcode 7: ssä.  Sinun on määritettävä **Käyttöön Bitcode** projektin **ei** **Luoda asetukset** .

    ![Pura Azure SDK-paketissa][10]

5. Uusi otsikko-tiedoston lisääminen projektiin nimeltä `HubInfo.h`. Tämän tiedoston mahtuu ilmoitus-toiminnossa vakiot.  Lisää seuraavat määritykset ja korvaa merkkijonon literal paikkamerkit *keskittimeen nimi* ja *DefaultListenSharedAccessSignature* muistiin kirjoittamasi aiemmassa versiossa.

        #ifndef HubInfo_h
        #define HubInfo_h
        
            #define HUBNAME @"<Enter the name of your hub>"
            #define HUBLISTENACCESS @"<Enter your DefaultListenSharedAccess connection string"
        
        #endif /* HubInfo_h */

6. Avaa oman `AppDelegate.h` tiedoston Lisää seuraavat tuo direktiivejä:

         #import <WindowsAzureMessaging/WindowsAzureMessaging.h> 
         #import "HubInfo.h"
        
7. Valitse oman `AppDelegate.m file`, Lisää seuraava koodi- `didFinishLaunchingWithOptions` iOS-versiosi perustuva menetelmä. Koodi rekisteröi laite kahvaa APN:

    IOS 8:

        UIUserNotificationSettings *settings = [UIUserNotificationSettings settingsForTypes:UIUserNotificationTypeSound |
                                                UIUserNotificationTypeAlert | UIUserNotificationTypeBadge categories:nil];

        [[UIApplication sharedApplication] registerUserNotificationSettings:settings];
        [[UIApplication sharedApplication] registerForRemoteNotifications];

    IOS-versiot ennen 8:

         [[UIApplication sharedApplication] registerForRemoteNotificationTypes: UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound];


8. Lisää saman tiedoston seuraavista tavoista. Tämä koodi muodostaa yhteyden ilmoituksen keskittimeen määrittämäsi HubInfo.h yhteystiedot. Valitse antaa laitteen tunnuksen ilmoitus-keskittimeen niin, että ilmoitus-toiminnossa voit lähettää ilmoitukset:

        - (void)application:(UIApplication *)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData *) deviceToken {
            SBNotificationHub* hub = [[SBNotificationHub alloc] initWithConnectionString:HUBLISTENACCESS
                                        notificationHubPath:HUBNAME];

            [hub registerNativeWithDeviceToken:deviceToken tags:nil completion:^(NSError* error) {
                if (error != nil) {
                    NSLog(@"Error registering for notifications: %@", error);
                }
                else {
                    [self MessageBox:@"Registration Status" message:@"Registered"];
                }
            }];
        }

        -(void)MessageBox:(NSString *)title message:(NSString *)messageText
        {
            UIAlertView *alert = [[UIAlertView alloc] initWithTitle:title message:messageText delegate:self
                cancelButtonTitle:@"OK" otherButtonTitles: nil];
            [alert show];
        }


9. Lisää saman tiedoston näyttämään **UIAlert** , jos ilmoituksen vastaanotetaan samalla, kun sovellus ole aktiivinen seuraavasti:


        - (void)application:(UIApplication *)application didReceiveRemoteNotification: (NSDictionary *)userInfo {
            NSLog(@"%@", userInfo);
            [self MessageBox:@"Notification" message:[[userInfo objectForKey:@"aps"] valueForKey:@"alert"]];
        }

10. Muodosta ja suorita sovellus laitteessasi, varmista, että ei ole virheitä.

## <a name="send-test-push-notifications"></a>Lähetä testi push-ilmoitukset


Voit testata ilmoitusten vastaanottaminen sovelluksen lähettämällä push-ilmoitukset [Azure-portaalin] kautta **vianmääritys** keskittimeen-sivu (käytä *Testata Lähetä* -vaihtoehto)-osioon.

![Azure Portal - testi lähetys][30]

[AZURE.INCLUDE [notification-hubs-sending-notifications-from-the-portal](../../includes/notification-hubs-sending-notifications-from-the-portal.md)]


## <a name="optional-send-push-notifications-from-the-app"></a>(Valinnainen) Lähetä-sovelluksen push-ilmoitukset

>[AZURE.IMPORTANT] Tämä esimerkki ilmoitusten lähettäminen asiakas-sovelluksen annetaan käyttöön liittyviä ilmoitusta. Koska tämä edellyttää `DefaultFullSharedAccessSignature` voi olla asiakas-sovellukseen, se paljastaa ilmoitus-keskittimeen, että käyttäjä voi saada luvattoman ilmoitukset lähetetään asiakkaiden riskiin.

Jos haluat lähettää push-ilmoitukset-sovelluksen sisällä, tässä osassa on esimerkki siitä, miten voit tehdä tämän REST-käyttöliittymän avulla.

1. Avaa Xcode, `Main.storyboard` ja lisää seuraavat käyttöliittymäosat objektin kirjastosta, jos haluat, että käyttäjä voi lähettää push-ilmoitukset-sovelluksessa:

    - Otsikkotekstiä sisältävä tarra. Sitä käytetään-ilmoitusten lähettäminen virheistä. **Rivit** -ominaisuus on määritettävä **0** niin, että se näkyy automaattisesti kokoa rajoitettu oikealle ja vasemmalle reunukset ja näkymän yläreunassa.
    - Tekstikentän **tekstin** arvoksi **Kirjoita viesti**. Rajoittaa kentän otsikon alapuolella alla kuvatulla tavalla. Määritä Näytä-ohjain laitteen edustaja.
    - **Lähetä ilmoitus** liittyvä painike rajoitettu alapuolella tekstikenttään ja vaakasuunnassa.

    Näkymän pitäisi näyttää seuraavasti:

    ![Xcode suunnittelu][32]


2. [Lisää pistorasioita](https://developer.apple.com/library/ios/recipes/xcode_help-IB_connections/chapters/CreatingOutlet.html) otsikko ja teksti-kentän yhteydessä näkymän, ja päivittää oman `interface` määritelmä tukemaan `UITextFieldDelegate` ja `NSXMLParserDelegate`. Lisää kolme ominaisuuden ilmoitukset tukitehtäviä kutsumalla REST-Ohjelmointirajapinnalla ja jäsennyksen vastauksen alla.

    ViewController.h tiedoston pitäisi näyttää seuraavasti:

        #import <UIKit/UIKit.h>

        @interface ViewController : UIViewController <UITextFieldDelegate, NSXMLParserDelegate>
        {
            NSXMLParser *xmlParser;
        }

        // Make sure these outlets are connected to your UI by ctrl+dragging
        @property (weak, nonatomic) IBOutlet UITextField *notificationMessage;
        @property (weak, nonatomic) IBOutlet UILabel *sendResults;

        @property (copy, nonatomic) NSString *statusResult;
        @property (copy, nonatomic) NSString *currentElement;

        @end

3. Avaa `HubInfo.h` ja lisää seuraavat vakiot joka käytetään ilmoitusten lähettäminen oman keskittimeen. Korvaa paikkamerkin merkkijonoliteraali todellinen *DefaultFullSharedAccessSignature* yhteysmerkkijonon.

        #define API_VERSION @"?api-version=2015-01"
        #define HUBFULLACCESS @"<Enter Your DefaultFullSharedAccess Connection string>"

4. Lisää seuraava `#import` lauseet oman `ViewController.h` tiedosto.

        #import <CommonCrypto/CommonHMAC.h>
        #import "HubInfo.h"

5. Valitse `ViewController.m` Lisää seuraava koodi käyttöliittymän käyttöönotto. Koodi jäsentää *DefaultFullSharedAccessSignature* yhteysmerkkijonon. [REST API viittaus](http://msdn.microsoft.com/library/azure/dn495627.aspx)mainittu jäsennetyn tiedot käytetään SaS tunnusta **luvan** pyynnön tunnistetietojen luomiseen.

        NSString *HubEndpoint;
        NSString *HubSasKeyName;
        NSString *HubSasKeyValue;

        -(void)ParseConnectionString
        {
            NSArray *parts = [HUBFULLACCESS componentsSeparatedByString:@";"];
            NSString *part;

            if ([parts count] != 3)
            {
                NSException* parseException = [NSException exceptionWithName:@"ConnectionStringParseException"
                    reason:@"Invalid full shared access connection string" userInfo:nil];

                @throw parseException;
            }

            for (part in parts)
            {
                if ([part hasPrefix:@"Endpoint"])
                {
                    HubEndpoint = [NSString stringWithFormat:@"https%@",[part substringFromIndex:11]];
                }
                else if ([part hasPrefix:@"SharedAccessKeyName"])
                {
                    HubSasKeyName = [part substringFromIndex:20];
                }
                else if ([part hasPrefix:@"SharedAccessKey"])
                {
                    HubSasKeyValue = [part substringFromIndex:16];
                }
            }
        }

6. Valitse `ViewController.m`, Päivitä `viewDidLoad` menetelmä jäsentää yhteysmerkkijonon näkymän latautuessa. Lisätä apuohjelman menetelmistä, alla käyttöliittymän toteuttamiseen.  


        - (void)viewDidLoad
        {
            [super viewDidLoad];
            [self ParseConnectionString];
            [_notificationMessage setDelegate:self];
        }

        -(NSString *)CF_URLEncodedString:(NSString *)inputString
        {
           return (__bridge NSString *)CFURLCreateStringByAddingPercentEscapes(NULL, (CFStringRef)inputString,
                NULL, (CFStringRef)@"!*'();:@&=+$,/?%#[]", kCFStringEncodingUTF8);
        }

        -(void)MessageBox:(NSString *)title message:(NSString *)messageText
        {
            UIAlertView *alert = [[UIAlertView alloc] initWithTitle:title message:messageText delegate:self
                cancelButtonTitle:@"OK" otherButtonTitles: nil];
            [alert show];
        }





7. Valitse `ViewController.m`, Lisää seuraava koodi käyttöliittymän käyttöönotto luomiseen SaS luvan tunnuksen, joka annetaan **Authorization** -otsikko mainitut [REST API-viittaus](http://msdn.microsoft.com/library/azure/dn495627.aspx).

        -(NSString*) generateSasToken:(NSString*)uri
        {
            NSString *targetUri;
            NSString* utf8LowercasedUri = NULL;
            NSString *signature = NULL;
            NSString *token = NULL;

            @try
            {
                // Add expiration
                uri = [uri lowercaseString];
                utf8LowercasedUri = [self CF_URLEncodedString:uri];
                targetUri = [utf8LowercasedUri lowercaseString];
                NSTimeInterval expiresOnDate = [[NSDate date] timeIntervalSince1970];
                int expiresInMins = 60; // 1 hour
                expiresOnDate += expiresInMins * 60;
                UInt64 expires = trunc(expiresOnDate);
                NSString* toSign = [NSString stringWithFormat:@"%@\n%qu", targetUri, expires];

                // Get an hmac_sha1 Mac instance and initialize with the signing key
                const char *cKey  = [HubSasKeyValue cStringUsingEncoding:NSUTF8StringEncoding];
                const char *cData = [toSign cStringUsingEncoding:NSUTF8StringEncoding];
                unsigned char cHMAC[CC_SHA256_DIGEST_LENGTH];
                CCHmac(kCCHmacAlgSHA256, cKey, strlen(cKey), cData, strlen(cData), cHMAC);
                NSData *rawHmac = [[NSData alloc] initWithBytes:cHMAC length:sizeof(cHMAC)];
                signature = [self CF_URLEncodedString:[rawHmac base64EncodedStringWithOptions:0]];

                // Construct authorization token string
                token = [NSString stringWithFormat:@"SharedAccessSignature sig=%@&se=%qu&skn=%@&sr=%@",
                    signature, expires, HubSasKeyName, targetUri];
            }
            @catch (NSException *exception)
            {
                [self MessageBox:@"Exception Generating SaS Token" message:[exception reason]];
            }
            @finally
            {
                if (utf8LowercasedUri != NULL)
                    CFRelease((CFStringRef)utf8LowercasedUri);
                if (signature != NULL)
                CFRelease((CFStringRef)signature);
            }

            return token;
        }


8. CTRL + vetämällä **Lähetä ilmoitus** -painiketta `ViewController.m` voit lisätä toiminnon nimeltä **SendNotificationMessage** **Kosketus** -tapahtuman. Päivitä menetelmä lähettää ilmoituksen REST-Ohjelmointirajapinnan käyttäminen seuraava koodi.

        - (IBAction)SendNotificationMessage:(id)sender
        {
            self.sendResults.text = @"";
            [self SendNotificationRESTAPI];
        }

        - (void)SendNotificationRESTAPI
        {
            NSURLSession* session = [NSURLSession
                             sessionWithConfiguration:[NSURLSessionConfiguration defaultSessionConfiguration]
                             delegate:nil delegateQueue:nil];

            // Apple Notification format of the notification message
            NSString *json = [NSString stringWithFormat:@"{\"aps\":{\"alert\":\"%@\"}}",
                                self.notificationMessage.text];

            // Construct the message's REST endpoint
            NSURL* url = [NSURL URLWithString:[NSString stringWithFormat:@"%@%@/messages/%@", HubEndpoint,
                                                HUBNAME, API_VERSION]];

            // Generate the token to be used in the authorization header
            NSString* authorizationToken = [self generateSasToken:[url absoluteString]];

            //Create the request to add the APNs notification message to the hub
            NSMutableURLRequest *request = [NSMutableURLRequest requestWithURL:url];
            [request setHTTPMethod:@"POST"];
            [request setValue:@"application/json;charset=utf-8" forHTTPHeaderField:@"Content-Type"];

            // Signify Apple notification format
            [request setValue:@"apple" forHTTPHeaderField:@"ServiceBusNotification-Format"];

            //Authenticate the notification message POST request with the SaS token
            [request setValue:authorizationToken forHTTPHeaderField:@"Authorization"];

            //Add the notification message body
            [request setHTTPBody:[json dataUsingEncoding:NSUTF8StringEncoding]];

            // Send the REST request
            NSURLSessionDataTask* dataTask = [session dataTaskWithRequest:request
                completionHandler:^(NSData *data, NSURLResponse *response, NSError *error)
            {
                NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) response;
                if (error || (httpResponse.statusCode != 200 && httpResponse.statusCode != 201))
                {
                    NSLog(@"\nError status: %d\nError: %@", httpResponse.statusCode, error);
                }
                if (data != NULL)
                {
                    xmlParser = [[NSXMLParser alloc] initWithData:data];
                    [xmlParser setDelegate:self];
                    [xmlParser parse];
                }
            }];
            [dataTask resume];
        }


9. Valitse `ViewController.m`, Lisää seuraava edustajan menetelmä tukemaan tekstikentän näppäimistön sulkeminen. CTRL + vetämällä tekstikentän voit määrittää näkymän ohjauskoneen laitteen edustaja käyttöliittymän suunnittelussa Näytä ohjauskoneen-kuvaketta.

        //===[ Implement UITextFieldDelegate methods ]===

        -(BOOL)textFieldShouldReturn:(UITextField *)textField
        {
            [textField resignFirstResponder];
            return YES;
        }


10. Valitse `ViewController.m`, lisätä edustajan toimista tukemaan vastauksen jäsennystä käyttämällä `NSXMLParser`.

        //===[ Implement NSXMLParserDelegate methods ]===

        -(void)parserDidStartDocument:(NSXMLParser *)parser
        {
            self.statusResult = @"";
        }

        -(void)parser:(NSXMLParser *)parser didStartElement:(NSString *)elementName
            namespaceURI:(NSString *)namespaceURI qualifiedName:(NSString *)qName
            attributes:(NSDictionary *)attributeDict
        {
            NSString * element = [elementName lowercaseString];
            NSLog(@"*** New element parsed : %@ ***",element);

            if ([element isEqualToString:@"code"] | [element isEqualToString:@"detail"])
            {
                self.currentElement = element;
            }
        }

        -(void) parser:(NSXMLParser *)parser foundCharacters:(NSString *)parsedString
        {
            self.statusResult = [self.statusResult stringByAppendingString:
                [NSString stringWithFormat:@"%@ : %@\n", self.currentElement, parsedString]];
        }

        -(void)parserDidEndDocument:(NSXMLParser *)parser
        {
            // Set the status label text on the UI thread
            dispatch_async(dispatch_get_main_queue(),
            ^{
                [self.sendResults setText:self.statusResult];
            });
        }



11. Projektin luominen ja varmista, että on virheitä.


> [AZURE.NOTE] Jos käytössä ilmenee-Xcode7 muodosta virheen tietoja bitcode tuesta, muutettava **Luominen asetukset** > Xcode **ei** voi**Ottaa käyttöön Bitcode (ENABLE_BITCODE)** . Ilmoitus keskittimet SDK ei tue bitcode tällä hetkellä. 

Löydät kaikki mahdolliset ilmoituksen paketteja Apple [paikallisen ja Push-ilmoituksen Programming Guide].


##<a name="checking-if-your-app-can-receive-push-notifications"></a>Jos sovelluksen saavat push-ilmoitukset tarkistaminen

Voit esikatsella push-ilmoitukset iOS-täytyy ottaa käyttöön sovelluksen fyysinen iOS-laitteessa. Et voi lähettää Apple push-ilmoitukset iOS Simulator avulla.

1. Suorita sovellus ja varmista, että rekisteröinti onnistuu, ja valitse sitten **OK**.

    ![iOS-sovelluksen Push-ilmoituksen rekisteröinti testi][33]

2. Voit lähettää ilmoituksen testi push [Azure-portaalin]ja yllä olevien ohjeiden mukaisesti. Jos olet lisännyt koodin lähettämiseen push-ilmoitukset-sovelluksessa, kosketa antamaan ilmoitusviestin teksti-kenttään. **Lähetä** -painiketta ja paina näppäimistön tai näkymässä, kun haluat lähettää ilmoitusviesti **Lähetä ilmoitus** -painiketta.

    ![iOS-sovelluksen Push-ilmoituksen lähettäminen testaaminen][34]

3. Push-ilmoitus lähetetään kaikissa laitteissa, joka on rekisteröity ilmoituksia tietyn ilmoitus-toiminnosta.

    ![iOS-sovelluksen Push-ilmoitus tulee testaaminen][35]


##<a name="next-steps"></a>Seuraavat vaiheet

Tämä esimerkki lähettää push-ilmoitukset kaikkien rekisteröityjen iOS-laitteiden. Suosittelemme kuin seuraavassa vaiheessa oman learning, jatkat [Azure ilmoituksen keskittimet Ilmoita käyttäjille iOS kanssa .NET Taustajärjestelmä] opetusohjelmaan, jossa edetään ohjatusti luominen taustassa tunnisteiden push-ilmoitukset lähetetään. 

Jos haluat määritetään käyttäjien korko ryhmiin, lisäksi siirtäminen [Käytä ilmoituksen keskittimet lähettää uutisia] opetusohjelmaan. 

Saat ilmoituksen keskittimet yleisiä tietoja [Ilmoituksen keskittimet ohjeita].



<!-- Images. -->

[6]: ./media/notification-hubs-ios-get-started/notification-hubs-configure-ios.png
[8]: ./media/notification-hubs-ios-get-started/notification-hubs-create-ios-app.png
[9]: ./media/notification-hubs-ios-get-started/notification-hubs-create-ios-app2.png
[10]: ./media/notification-hubs-ios-get-started/notification-hubs-create-ios-app3.png
[11]: ./media/notification-hubs-ios-get-started/notification-hubs-xcode-product-name.png

[30]: ./media/notification-hubs-ios-get-started/notification-hubs-test-send.png

[31]: ./media/notification-hubs-ios-get-started/notification-hubs-ios-ui.png
[32]: ./media/notification-hubs-ios-get-started/notification-hubs-storyboard-view.png
[33]: ./media/notification-hubs-ios-get-started/notification-hubs-test1.png
[34]: ./media/notification-hubs-ios-get-started/notification-hubs-test2.png
[35]: ./media/notification-hubs-ios-get-started/notification-hubs-test3.png



<!-- URLs. -->
[Mobile-palveluihin iOS SDK 1.2.4 versio]: http://aka.ms/kymw2g
[Mobile Services iOS SDK]: http://go.microsoft.com/fwLink/?LinkID=266533
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253

[Get started with Mobile Services]: /develop/mobile/tutorials/get-started-ios
[Azure Classic Portal]: https://manage.windowsazure.com/
[Ilmoitus keskittimet ohjeet]: http://msdn.microsoft.com/library/jj927170.aspx
[Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[iOS Provisioning Portal]: http://go.microsoft.com/fwlink/p/?LinkId=272456

[Get started with push notifications in Mobile Services]: ../mobile-services-javascript-backend-ios-get-started-push.md
[Azure ilmoituksen keskittimet Ilmoita käyttäjille iOS .NET Taustajärjestelmä kanssa]: notification-hubs-aspnet-backend-ios-apple-apns-notification.md
[Ilmoitus keskittimet avulla voit lähettää uutisia]: notification-hubs-ios-xplat-segmented-apns-push-notification.md

[Paikallinen ja Push-ilmoituksen ohjelmoinnin opas]: http://developer.apple.com/library/mac/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW1
[Azure Portal]: https://portal.azure.com