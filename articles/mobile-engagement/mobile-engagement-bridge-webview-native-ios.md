<properties 
    pageTitle="Jossa alkuperäisen Mobile välitys iOS SDK iOS silta näkymä" 
    description="Kerrotaan, miten voit luoda sillan välillä Javascript-ja Mobile välitys alkuperäisen iOS SDK näkymä"      
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
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="bridge-ios-webview-with-native-mobile-engagement-ios-sdk"></a>Jossa alkuperäisen Mobile välitys iOS SDK iOS silta näkymä

> [AZURE.SELECTOR]
- [Android-silta](mobile-engagement-bridge-webview-native-android.md)
- [iOS-silta](mobile-engagement-bridge-webview-native-ios.md)

Jotkin mobile-sovellukset on suunniteltu hybrid-sovellus, jossa sovelluksessa on kehitetty alkuperäisen iOS tavoitteen C kehittäminen, mutta jotkin tai jopa kaikkien ikkunoiden hahmonnetaan sisällä iOS näkymä. Voit silti tarjoaman Mobile välitys iOS SDK: ssa tällaiset-sovelluksista ja tässä opetusohjelmassa kuvataan, miten voit vaihtaa tietoja tekoa. 

Käytettävissä on kaksi tapaa haluat saavuttaa, vaikka molemmat ovat dokumentoimattomia:

- Ensin jotakin on kuvattu tätä [linkkiä](http://stackoverflow.com/questions/9826792/how-to-invoke-objective-c-method-from-javascript-and-send-back-data-to-javascrip) , joka liittyy rekisteröiminen `UIWebViewDelegate` web-näkymä ja todellisen-ja-välittömästi-Peruuta sijainnin muutoksen Javascript valmis. 
- Jokin toinen perustuvat tässä [pidettyä WWDC-Konferenssia 2013 istunnon](https://developer.apple.com/videos/play/wwdc2013/615)toimintatavan, joka on selkeämpi kuin ensimmäistä ja jossa on seuraa tämän oppaan. Huomaa, että tämä menetelmä toimii vain ios7-käyttöjärjestelmää varten ja edellä. 

For iOS silta otoksen, noudata seuraavia ohjeita:

1. Sinun on ensin, varmista, että ovat kadonneet Microsoftin [aloittaminen opetusohjelma](mobile-engagement-ios-get-started.md) integroiminen Mobile välitys-iOS SDK hybrid-sovelluksen kautta. Vaihtoehtoisesti voit myös ottaa testi kirjaaminen seuraavasti niin, että näet SDK menetelmiä kuin olemme käynnistää niitä-näkymä. 
    
        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
           ....
            [EngagementAgent setTestLogEnabled:YES];
           ....
        }

2. Varmista, että hybrid sovelluksen on näyttö, jossa näkymä sitä. Voit lisätä sen, `Main.storyboard` sovelluksen. 

3. Liittäminen oman **ViewController** Tämä näkymä napsauttamalla ja vetämällä näkymä Näytä Controller-näkymän `ViewController.h` Muokkaa aloitusnäytössä asettamisen vain alla `@interface` rivi. 

4. Kun teet näin, valintaikkuna ponnahdusikkuna pyytävä nimi. Anna nimi kuin **näkymä**. Oman `ViewController.h` tiedoston pitäisi näyttää seuraavalta:

        #import <UIKit/UIKit.h>
        #import "EngagementViewController.h"
        
        @interface ViewController : EngagementViewController
        @property (strong, nonatomic) IBOutlet UIWebView *webView;
        
        @end

5. Päivitämme `ViewController.m` tiedostoon myöhemmin, mutta ensin joka luo paketti joitakin usein käytettyjä Mobile välitys iOS SDK menetelmiä silta tiedosto luodaan. Luo uusi otsikko-tiedosto nimeltä **EngagementJsExports.h** , joka käyttää `JSExport` edellä mainittujen [istunnon](https://developer.apple.com/videos/play/wwdc2013/615) , voit näyttää alkuperäisen iOS-menetelmistä on kuvattu järjestelmä. 

        #import <Foundation/Foundation.h>
        #import <JavaScriptCore/JavascriptCore.h>
        
        @protocol EngagementJsExports <JSExport>
        
        + (void) sendEngagementEvent:(NSString*) name :(NSString*)extras;
        + (void) startEngagementJob:(NSString*) name :(NSString*)extras;
        + (void) endEngagementJob:(NSString*) name;
        + (void) sendEngagementError:(NSString*) name :(NSString*)extras;
        + (void) sendEngagementAppInfo:(NSString*) appInfo;
        
        @end

        @interface EngagementJs : NSObject <EngagementJsExports>

        @end

6. Seuraavaksi luodaan silta tiedoston toista osaa. Luo tiedosto nimeltä **EngagementJsExports.m** , joka sisältää luominen todellinen kääreisiin soittamalla Mobile välitys iOS SDK menetelmiä käyttöönotto. Huomaa myös, että olemme Jäsennys `extras` välitettävä näkymä javascript- ja laajennettujen kyselyjä, jotka `NSMutableDictionary` välitys SDK menetelmä soitettaessa siirrettävä objekti.  

        #import <UIKit/UIKit.h>
        #import "EngagementAgent.h"
        #import "EngagementJsExports.h"
        
        @implementation EngagementJs
        
        +(void) sendEngagementEvent:(NSString*)name :(NSString*)extras {
           NSMutableDictionary* extrasInput = [self ParseExtras:extras];
           [[EngagementAgent shared] sendEvent:name extras:extrasInput];
        }
        
        + (void) startEngagementJob:(NSString*) name :(NSString*)extras {
           NSMutableDictionary* extrasInput = [self ParseExtras:extras];
           [[EngagementAgent shared] startJob:name extras:extrasInput];
        }
        
        + (void) endEngagementJob:(NSString*) name {
           [[EngagementAgent shared] endJob:name];
        }
        
        + (void) sendEngagementError:(NSString*) name :(NSString*)extras {
           NSMutableDictionary* extrasInput = [self ParseExtras:extras];
           [[EngagementAgent shared] sendError:name extras:extrasInput];
        }
        
        + (void) sendEngagementAppInfo:(NSString*) appInfo {
           NSMutableDictionary* appInfoInput = [self ParseExtras:appInfo];
           [[EngagementAgent shared] sendAppInfo:appInfoInput];
        }
        
        + (NSMutableDictionary*) ParseExtras:(NSString*) input {
           NSData *data = [input dataUsingEncoding:NSUTF8StringEncoding];
           NSError* error = nil;
           NSMutableDictionary* extras = [NSJSONSerialization JSONObjectWithData:data options:0 error:&error];
           
           return extras;
        }
        
        @end

5. Nyt on palaa **ViewController.m** ja päivität sen seuraava koodi: 
        
        #import <JavaScriptCore/JavaScriptCore.h>
        #import "ViewController.h"
        #import "EngagementJsExports.h"
        
        @interface ViewController ()
        
        @end
        
        @implementation ViewController
        
        - (void)viewDidLoad
        {
           self.webView.delegate = self;
           [super viewDidLoad];
           [self loadWebView];
        }
        
        - (void)loadWebView {
           NSBundle* mainBundle = [NSBundle mainBundle];
           NSURL* htmlPage = [mainBundle URLForResource:@"LocalPage" withExtension:@"html"];
           
           NSURLRequest* urlReq = [NSURLRequest requestWithURL:htmlPage];
           [self.webView loadRequest:urlReq];
        }
        
        - (void)webViewDidFinishLoad:(UIWebView*)wv
        {
           JSContext* context = [wv valueForKeyPath:@"documentView.webView.mainFrame.javaScriptContext"];
           
           context[@"EngagementJs"] = [EngagementJs class];
        }
        
        - (void)webView:(UIWebView*)wv didFailLoadWithError:(NSError*)error
        {
           NSLog(@"Error for WEBVIEW: %@", [error description]);
        }
        
        - (void)didReceiveMemoryWarning {
           [super didReceiveMemoryWarning];
           // Dispose of any resources that can be recreated.
        }
        
        @end

6. Huomaa seuraavat seikat **ViewController.m** tiedoston:

    - Valitse `loadWebView` menetelmässä on ladataan paikallisen HTML-tiedosto nimeltä **LocalPage.html** jonka käydään läpi on seuraava koodi. 
    - Valitse `webViewDidFinishLoad` menetelmässä on ovat otsikkorivialueen `JsContext` ja tutustu paketti luokan liittäminen. Tämä sallii kutsumista Microsoftin paketti SDK menetelmiä käyttämällä kahva **EngagementJs** -näkymä. 

7. Luo tiedosto nimeltä **LocalPage.html** seuraava koodi:

        <!doctype html>
        <html>
           <head>
               <style type='text/css'>
                   html { font-family:Helvetica; color:#222; }
                   h1 { color:steelblue; font-size:22px; margin-top:16px; }
                   h2 { color:grey; font-size:14px; margin-top:18px; margin-bottom:0px; }
               </style>
               
               <script type="text/javascript">
               
               window.onerror = function(err)
               {
                   alert('window.onerror: ' + err);
               }
               
               function send(inputId)
               {
                   var input = document.getElementById(inputId);
                   if(input)
                   {
                       var value = input.value;
                       // Example of how extras info can be passed with the Engagement logs
                       var extras = '{"CustomerId":"MS290011"}';
                   }
                   
                   if(value && value.length > 0)
                   {
                       switch(inputId)
                       {
                           case "event":
                           EngagementJs.sendEngagementEvent(value, extras);
                           break;
                           
                           case "job":
                           EngagementJs.startEngagementJob(value, extras);
                           window.setTimeout(
                           function(){
                               EngagementJs.endEngagementJob(value);
                           }, 10000 );
                           break;
        
                           case "error":
                           EngagementJs.sendEngagementError(value, extras);
                           break;
                           
                           case "appInfo":
                           var appInfo = '{"customer_name":"' + value + '"}';
                           EngagementJs.sendEngagementAppInfo(appInfo);
                           break;
                       }
                   }
               }
               </script>
               
           </head>
           <body>
               <h1>Bridge Tester</h1>
               
               <div id='engagement'>
                   
                   <br/>
                   <h2>Event</h2>
                   <input type="text" id="event" size="35">
                   <button onclick="send('event')">Send</button>
                   
                   <br/>
                   <h2>Job</h2>
                   <input type="text" id="job" size="35">
                   <button onclick="send('job')">Send</button>
                   
                   <br/>
                   <h2>Error</h2>
                   <input type="text" id="error" size="35">
                   <button onclick="send('error')">Send</button
                   
                   <br/>
                   <h2>AppInfo</h2>
                   <input type="text" id="appInfo" size="35">
                   <button onclick="send('appInfo')">Send</button>
               
               </div>
           </body>
        </html>

8. Huomaa seuraavat seikat yllä HTML-tiedoston tietoja:

    -   Se sisältää syötteen ruudut, johon voidaan lisätä tietoja voidaan käyttää tapahtuman, työ, virhe, AppInfo nimet. Napsautettaessa-painikkeen vieressä puhelu soitetaan JavaScript-koodia, johon soittaa myöhemmin menetelmiä välittää tämän puhelun Mobile välitys iOS SDK silta-tiedostosta. 
    -   Olemme tunnisteita, jotkin staattinen lisätiedot tapahtumia, työt ja näytetään, miten tämän voi tehdä myös virheitä. Tämä lisätiedot lähetetään JSON merkkijono, jos olet kohde `EngagementJsExports.m` tiedoston, on jäsentää ja sekä lähettäminen tapahtumat-projektit-virheet on ohitettu. 
    -   Mobile välitys työ on poistettu nimellä määrittämäsi tekstiruudussa, suorita 10 sekuntia ja sulje. 
    -   Mobile välitys appinfo tai tunnisteen välitetään customer_name staattinen avain ja arvo, joka on lisätty syötteen arvona tunnisteen. 
 
9. Suorita sovellus ja näet seuraavasti. Nyt nimetä joitakin testi tapahtuman, kuten jompikumpi seuraavista ja valitse sen vieressä **Lähetä** . 

    ![][1]

10. Nyt siirtyessäsi sovelluksen ja ulkoasua, valitse **Tapahtumat -> tietojen** **Näyttö** -välilehti, näet sekä staattinen sovelluksen-tiedot, jotka on lähettää näkyvät tapahtuman. 

    ![][2]

<!-- Images. -->
[1]: ./media/mobile-engagement-bridge-webview-native-ios/sending-event.png
[2]: ./media/mobile-engagement-bridge-webview-native-ios/event-output.png
