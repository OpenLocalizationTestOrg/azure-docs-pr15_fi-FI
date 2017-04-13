<properties
    pageTitle="Azure Active Directory-B2C: Osallistua verkko-Ohjelmointirajapinnan iOS-sovelluksen kolmannen osapuolen kirjastojen avulla | Microsoft Azure"
    description="Tässä artikkelissa kerrotaan, kuinka iOS-Tehtävät-luettelo-sovelluksen, joka kutsuu Node.js verkko-Ohjelmointirajapinnan käyttämällä OAuth 2.0 haltijan tunnusten käyttämällä kolmansien osapuolten kirjaston luominen"
    services="active-directory-b2c"
    documentationCenter="ios"
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

< tunnisteet ms.service= "aktiivinen-hakemisto-b2c" ms.workload="identity" ms.tgt_pltfrm="na" ms.devlang="objectivec" ms.topic= "pääkuva-artikkelissa"

    ms.date="07/26/2016"
    ms.author="brandwe"/>

# <a name="azure-ad-b2c--call-a-web-api-from-an-ios-application-using-a-third-party-library"></a>Azure AD-B2C: Soita verkko-Ohjelmointirajapinnan käyttäminen kolmannen osapuolen kirjaston iOS-sovelluksesta

<!-- TODO [AZURE.INCLUDE [active-directory-b2c-devquickstarts-web-switcher](../../includes/active-directory-b2c-devquickstarts-web-switcher.md)]-->

Microsoft identity-ympäristössä käyttää avoimia standardeja, kuten OAuth2 ja OpenID yhteyden. Näin kehittäjät voivat hyödyntää kaikissa kirjastoissa he haluavat integroida palveluiden kanssa. Helpota sovelluskehittäjille käyttämällä Microsoftin platform muita kirjastoja on muistiinpanoa muutama vaihe vaiheelta, kuten tämän palvelun demonstate määrittäminen kolmannen osapuolen kirjastot, jos haluat muodostaa yhteyden Microsoft identity-ympäristössä. Useimmat kirjastoissa, jotka toteuttavat [RFC6749 OAuth2 määritys](https://tools.ietf.org/html/rfc6749) voivat muodostaa yhteyden Microsoft Identity-ympäristössä.


Jos ole ennen käyttänyt OAuth2 tai OpenID muodosta paljon otoksen määritysten voi tehdä paljon mielessä ei sinulle. Suosittelemme, että voit tarkastella lyhyt [yleiskatsaus on on kuvattu seuraavassa protokolla](active-directory-b2c-reference-protocols.md).

> [AZURE.NOTE]
    Microsoftin ympäristö ominaisuuksia, joilla lausekkeen nämä vaatimukset ehdollisen käyttöoikeuden ja Intune hallinnassa, kuten edellyttää Microsoftin avoimen lähteen Microsoft Azure tunnistetietojen kirjastojen avulla. 
   
Kaikki Azure Active Directory-skenaariot & ominaisuuksia tuetaan B2C-ympäristössä.  Voit selvittää, jos sinun on käytettävä B2C-ympäristö, lue lisätietoja [B2C rajoitukset](active-directory-b2c-limitations.md).


## <a name="get-an-azure-ad-b2c-directory"></a>Hae Azure AD B2C-kansio

Ennen kuin voit käyttää Azure AD B2C, voit luoda hakemiston tai vuokraaja. Kansio on kaikkien käyttäjien, sovellukset, ryhmät ja lisää säilö. Jos sinulla ei ole jokin jo, ennen kuin voit [luoda hakemiston B2C](active-directory-b2c-get-started.md) edelleen.

## <a name="create-an-application"></a>Sovelluksen luominen

Seuraavaksi haluat luoda sovelluksen B2C hakemistossa. Näin pitää yhteyttä suojatusti sovelluksen tarvitsemiaan Azure AD-tietoja. Sovelluksen ja verkko-Ohjelmointirajapinnan ilmaistaan yksi **Sovellustunnus** tässä tapauksessa koska ne muodostavat yhteen looginen-sovellukseen. Voit luoda sovelluksen noudattamalla [näitä ohjeita](active-directory-b2c-app-registration.md). Muista:

- Sisällytä sovelluksen **mobiililaitteeseen** .
- Kopioi **Tunnus** , jolla määritetään sovelluksen. Tarvitset myös tämän myöhemmin.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Luoda oman käytäntöjä

Azure AD B2C jokaisen käyttäjäkokemus on määritetty [käytännön](active-directory-b2c-reference-policies.md)avulla. Sovellus on yksi identity-versio: yhdistetty sign in ja kirjautuminen. Haluat luoda tämän käytännön kunkin tyypin [käytännön viitata artikkelissa](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy)kuvatulla tavalla. Kun luot käytäntöä, muista:

- Valitse käytäntöjen **Näyttönimi** ja ilmoittautuminen määritteet.
- Valitse jokaisen käytännön **Näyttönimi** ja **Objektitunnus** sovelluksen saatavat. Voit valita myös muut vaateet.
- Kopioi kunkin käytännön **nimen** , sen luomisen jälkeen. Tunnisteen pitäisi olla etuliite `b2c_1_`.  Tarvitset käytännön nimen myöhemmin.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Luotuasi oman käytännöt olet valmis luomaan sovelluksen.


## <a name="download-the-code"></a>Lataa koodi

Tässä opetusohjelmassa koodi määritetään [GitHub](https://github.com/Azure-Samples/active-directory-ios-native-nxoauth2-b2c).  Voit seurata, voit [ladata sovelluksen nimellä .zip](https://github.com/Azure-Samples/active-directory-ios-native-nxoauth2-b2c)/archive/master.zip) tai Kloonaa sitä:

```
git clone git@github.com:Azure-Samples/active-directory-ios-native-nxoauth2-b2c.git
```

Tai vain lataamalla valmiin koodi ja aloittaa saman tien: 

```
git clone --branch complete git@github.com:Azure-Samples/active-directory-ios-native-nxoauth2-b2c.git
```

## <a name="download-the-third-party-library-nxoauth2-and-launch-a-workspace"></a>Lataa kolmannen osapuolen kirjaston nxoauth2 ja Käynnistä työtilan

Näiden vaiheiden Käytämme OAuth2Client-GitHub, OAuth2-kirjasto Mac OS X- ja iOS (kaakao ja kosketa kaakao). Tämä kirjasto perustuu OAuth2 määritys 10 luonnoksen. Se toteuttaa alkuperäisen sovelluksen profiilin ja tukee loppukäyttäjien luvan päätepiste. Nämä ovat asioista, joita annettava järjestyksessä integrat Microsoft identity-ympäristön kanssa.

### <a name="adding-the-library-to-your-project-using-cocoapods"></a>Kirjaston lisääminen projektiin CocoaPods avulla

CocoaPods on Xcode projektien riippuvuuden valvoja. Yllä asennusohjeita hallitaan automaattisesti.

```
$ vi Podfile
```
Lisää tämä podfile seuraavasti:

```
 platform :ios, '8.0'
 
 target 'SampleforB2C' do
 
 pod 'NXOAuth2Client'
 
 end
```

Lataa nyt käyttämällä cocoapods podfile. Tämä luo uuden XCode työtilan haluat ladata.

```
$ pod install
...
$ open SampleforB2C.xcworkspace

```

## <a name="the-structure-of-the-project"></a>Projektin rakenteessa

On seuraava rakenne rakenne-projektin määrittäminen:

* Tehtäväruudun **Perustyylinäkymä**
* **Lisää tehtävänäkymä** valitun tehtävän tiedot
* **Kirjautuminen näkymä** , jossa käyttäjät kirjautumaan sisään-sovellukseen.

Olemme johtaa Lisää todennus projektin eri tiedostoja. Muita osia, kuten visual koodi koodi ei ole germane käyttäjätietojen ja annetaan puolestasi.

## <a name="create-the-settingsplist-file-for-your-application"></a>Luo `settings.plist` tiedoston sovelluksen

On helpompi määrittää sovelluksen, jos on keskitetyn paikan ja sijoittaa Microsoftin määritysten arvot. Se myös auttaa sinua ymmärtämään kunkin asetuksen mitä sovelluksessa. Microsoft hyödyntää *Ominaisuusluettelosta* avulla nämä arvot-sovellukseen.

* Luo tai avaa `settings.plist` kohdassa tiedoston `Supporting Files` sovelluksen-työtilassa

* Kirjoita seuraavat arvot (perehdymme läpi yksityiskohtaisesti pian)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>accountIdentifier</key>
    <string>B2C_Acccount</string>
    <key>clientID</key>
    <string><client ID></string>
    <key>clientSecret</key>
    <string></string>
    <key>authURL</key>
    <string>https://login.microsoftonline.com/<tenant name>/oauth2/v2.0/authorize?p=<policy name></string>
    <key>loginURL</key>
    <string>https://login.microsoftonline.com/<tenant name>/login</string>
    <key>bhh</key>
    <string>urn:ietf:wg:oauth:2.0:oob</string>
    <key>tokenURL</key>
    <string>https://login.microsoftonline.com/<tenant name>/oauth2/v2.0/token?p=<policy name></string>
    <key>keychain</key>
    <string>com.microsoft.azureactivedirectory.samples.graph.QuickStart</string>
    <key>contentType</key>
    <string>application/x-www-form-urlencoded</string>
    <key>taskAPI</key>
    <string>https://aadb2cplayground.azurewebsites.net</string>
</dict>
</plist>
```

Mennään näihin yksityiskohtaiset tiedot.


Saat `authURL`, `loginURL`, `bhh`, `tokenURL` huomaat, sinun on täytettävä vuokraajan nimi. Tämä on B2C alihallintaan, joka on määritetty sinulle vuokraajan nimi. Esimerkiksi `kidventusb2c.onmicrosoft.com`. Jos käytössäsi on Microsoftin Avaa lähde Microsoft Azure tunnistetietojen kirjastojen, nämä tiedot tuoda käyttämällä Microsoftin metatietojen päätepiste. Olemme tehdä työtä näyttämisestä nämä arvot puolestasi.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

`keychain` Arvo on säilö, NXOAuth2Client kirjasto avulla voit luoda oman tunnusten tallentamiseen avainnipun. Jos haluat saada SSO käytettävistä useiden sovelluksista voit voit määrittää saman avainnipun kaikkien sovellustesi sekä pyytää, että avainnipun käyttö XCode entitements. Tämä käsitellään Apple-ohjeista.

`<policy name>` URL-Osoitteen lopussa on merkit kohtaa, johon haluat sijoittaa edellä luomasi käytännön. Sovelluksen soittaa käytännöt kulun mukaan.

`taskAPI` Muiden päätepisteen olemme soittavat kanssa B2C-tunnuksen joko lisätä tehtäviä tai olemassa olevien tehtävien kyselyn. Tämä on määritetty tässä esimerkissä varten. Ei tarvitse muuttaa otoksen toimimaan.

Nämä arvot loput tarvitaan kirjasto ja luoda riittää, että voit suorittaa kontekstiin arvot.

Nyt kun on `settings.plist` luodun tiedoston annettava se muotoon.

## <a name="set-up-a-appdata-class-to-read-our-settings"></a>AppData-luokan lukemaan Microsoftin asetusten määrittäminen

Varmista seuraavaksi yksinkertainen tiedostoon, jonka juuri jäsentää Microsoftin `settngs.plist` tiedosto on luotu edellä ja tehdä näiden asetusten avaialble myöhemmin kaikki luokan. Koska olemme halua luoda uuden tiedot aina, kun luokan kysyy, syy käyttää Yksiarvoinen kuviota ja palauttaa vain samassa esiintymässä luodaan aina pyyntö tehdään asetukset

* Luo `AppData.h` tiedosto:

```objc
#import <Foundation/Foundation.h>

@interface AppData : NSObject

@property(strong) NSString *accountIdentifier;
@property(strong) NSString *taskApiString;
@property(strong) NSString *authURL;
@property(strong) NSString *clientID;
@property(strong) NSString *loginURL;
@property(strong) NSString *bhh;
@property(strong) NSString *keychain;
@property(strong) NSString *tokenURL;
@property(strong) NSString *clientSecret;
@property(strong) NSString *contentType;

+ (id)getInstance;

@end
```

* Luo `AppData.m` tiedosto:

```objc
#import "AppData.h"

@implementation AppData

+ (id)getInstance {
  static AppData *instance = nil;
  static dispatch_once_t onceToken;

  dispatch_once(&onceToken, ^{
    instance = [[self alloc] init];

    NSDictionary *dictionary = [NSDictionary
        dictionaryWithContentsOfFile:[[NSBundle mainBundle]
                                         pathForResource:@"settings"
                                                  ofType:@"plist"]];
    instance.accountIdentifier = [dictionary objectForKey:@"accountIdentifier"];
    instance.clientID = [dictionary objectForKey:@"clientID"];
    instance.clientSecret = [dictionary objectForKey:@"clientSecret"];
    instance.authURL = [dictionary objectForKey:@"authURL"];
    instance.loginURL = [dictionary objectForKey:@"loginURL"];
    instance.bhh = [dictionary objectForKey:@"bhh"];
    instance.tokenURL = [dictionary objectForKey:@"tokenURL"];
    instance.keychain = [dictionary objectForKey:@"keychain"];
    instance.contentType = [dictionary objectForKey:@"contentType"];
    instance.taskApiString = [dictionary objectForKey:@"taskAPI"];

  });

  return instance;
}
@end
```

Nyt on voit helposti hakea tietojamme soittamalla yksinkertaisesti `  AppData *data = [AppData getInstance];` näkyy alla kaikista Microsoftin luokkien tavalliseen tapaan.



## <a name="set-up-the-nxoauth2client-library-in-your-appdelegate"></a>Oman AppDelegate NXOAuth2Client kirjastossa määrittäminen

Joitakin arvoja, voit määrittää edellytetään NXOAuthClient-kirjastossa. Kerran, joka on valmis voit tunnuksen, joka on aquired Soita REST-Ohjelmointirajapinnalla. Koska Microsoft tietää, että `AppDelegate` tahansa olemme ladata sovellusta kutsutaan se on järkevää olemme valitseminen Microsoftin määritysten arvot tähän tiedostoon.
* Avaa `AppDelegate.m` tiedosto

* Tuo Käytämme myöhemmin ylätunnisteen tiedostoja.

```objc
#import "NXOAuth2.h" // the Identity library we are using
#import "AppData.h" // the class we just created we will use to load the settings of our application
```

* Lisää `setupOAuth2AccountStore` AppDelegate-menetelmällä

Annettava luominen AccountStore ja syötteen on vain lukea-tiedot `settings.plist` tiedosto.

Asiat on otettava huomioon B2C palvelua koskevia tässä vaiheessa, joka tekee koodi Lisää ymmärrettäviä:


1. Azure AD-B2C käyttää *käytännön* kyselyparametrit mukainen vastaamisessa puhelinnumeroon. Näin Azure Active Directory toimimaan vain sovellukselle riippumaton palveluna. Jotta voit antaa nämä ylimääräisiä kyselyn parametrit on annettava `kNXOAuth2AccountStoreConfigurationAdditionalAuthenticationParameters:` menetelmä mukautetun käytännön meidän parametreilla. 

2. Azure AD-B2C käyttää käyttöalueen lähes samalla tavalla kuin muihin OAuth2-palvelimiin. Kuitenkin koska B2C käyttö on yhtä paljon tietoja käyttöoikeudet kuin käytettäessä resurssien tarkistetaan joidenkin alueiden ehdottoman tarvitaan, jotta työnkulku toimii oikein. Tämä on `openid` aluetta. Tutustu Microsoftin tunnistetietojen SDK: T määrittää automaattisesti `openid` laajuutta, jotta et näe, tutustu SDK-määritys. Kolmannen osapuolen kirjastossa on käytössä, koska kuitenkin on määritettävä tämän alueen.

```objc
- (void)setupOAuth2AccountStore {
  AppData *data = [AppData getInstance]; // The singleton we use to get the settings

  NSDictionary *customHeaders =
      [NSDictionary dictionaryWithObject:@"application/x-www-form-urlencoded"
                                  forKey:@"Content-Type"];

  // Azure B2C needs
  // kNXOAuth2AccountStoreConfigurationAdditionalAuthenticationParameters for
  // sending policy to the server,
  // therefore we use -setConfiguration:forAccountType:
  NSDictionary *B2cConfigDict = @{
    kNXOAuth2AccountStoreConfigurationClientID : data.clientID,
    kNXOAuth2AccountStoreConfigurationSecret : data.clientSecret,
    kNXOAuth2AccountStoreConfigurationScope :
        [NSSet setWithObjects:@"openid", data.clientID, nil],
    kNXOAuth2AccountStoreConfigurationAuthorizeURL :
        [NSURL URLWithString:data.authURL],
    kNXOAuth2AccountStoreConfigurationTokenURL :
        [NSURL URLWithString:data.tokenURL],
    kNXOAuth2AccountStoreConfigurationRedirectURL :
        [NSURL URLWithString:data.bhh],
    kNXOAuth2AccountStoreConfigurationCustomHeaderFields : customHeaders,
    //      kNXOAuth2AccountStoreConfigurationAdditionalAuthenticationParameters:customAuthenticationParameters
  };

  [[NXOAuth2AccountStore sharedStore] setConfiguration:B2cConfigDict
                                        forAccountType:data.accountIdentifier];
}
```
Varmista seuraavaksi, että kutsu-AppDelegate `didFinishLaunchingWithOptions:` menetelmää. 

```
[self setupOAuth2AccountStore];
```


## <a name="create-a-loginviewcontroller-class-that-we-will-use-to-handle-authentication-requests"></a>Luo `LoginViewController` luokka, joka käsittelee käyttöoikeuksien Käytämme

Käytämme näkymä tilin kirjautumista varten. Näin käyttäjältä muut tekijät, kuten tekstin tekstiviesti (Jos määritetty) tai virhesanomia antaminen käyttäjälle. Tässä on määrittää määrittäminen näkymä ja kirjoita koodi, tapahtuu Microsoftin Identity-palvelusta näkymä takaisinkutsuja käsittelemään myöhemmin.

* Luo `LoginViewController.h` luokan

```objc
@interface LoginViewController : UIViewController <UIWebViewDelegate>
@property(weak, nonatomic) IBOutlet UIWebView *loginView; // Our webview that we will use to do authentication

- (void)handleOAuth2AccessResult:(NSURL *)accessResult; // Allows us to get a token after we've received an Access code.
- (void)setupOAuth2AccountStore; // We will need to add to our OAuth2AccountStore we setup in our AppDelegate
- (void)requestOAuth2Access; // This is where we invoke our webview.
```

Kaikkien näiden menetelmien luodaan.

> [AZURE.NOTE] 
    Varmista, että sidot `loginView` , että kuvakäsikirjoitus sisällä oleva todellinen näkymä. Muussa tapauksessa ei tarvitse näkymä, joka voi ponnahdusikkuna, kun on aika todentaa.

* Luo `LoginViewController.m` luokan

* Lisää suorittamaan tilan, kun olemme todennetaan joitakin muuttujat

```objc
NSURL *myRequestedUrl; \\ The URL request to Azure Active Directory 
NSURL *myLoadedUrl; \\ The URL loaded for Azure Active Directory
bool loginFlow = FALSE; 
bool isRequestBusy; \\ A way to give status to the thread that the request is still happening
NSURL *authcode; \\ A placeholder for our auth code.
```

* Ohita näkymä menetelmiä käsittelemään todennus

Onko näkymä haluamme kun edellyttää, että käyttäjän kirjautuminen kuin edellä mainittujen toiminnan annettava. Voit vain Leikkaa ja liitä seuraava koodi.

```objc
- (void)resolveUsingUIWebView:(NSURL *)URL {
  // We get the auth token from a redirect so we need to handle that in the
  // webview.

  if (![NSThread isMainThread]) {
    [self performSelectorOnMainThread:@selector(resolveUsingUIWebView:)
                           withObject:URL
                        waitUntilDone:YES];
    return;
  }

  NSURLRequest *hostnameURLRequest =
      [NSURLRequest requestWithURL:URL
                       cachePolicy:NSURLRequestUseProtocolCachePolicy
                   timeoutInterval:10.0f];
  isRequestBusy = YES;
  [self.loginView loadRequest:hostnameURLRequest];

  NSLog(@"resolveUsingUIWebView ready (status: UNKNOWN, URL: %@)",
        self.loginView.request.URL);
}

- (BOOL)webView:(UIWebView *)webView
    shouldStartLoadWithRequest:(NSURLRequest *)request
                navigationType:(UIWebViewNavigationType)navigationType {
  AppData *data = [AppData getInstance];

  NSLog(@"webView:shouldStartLoadWithRequest: %@ (%li)", request.URL,
        (long)navigationType);

  // The webview is where all the communication happens. Slightly complicated.

  myLoadedUrl = [webView.request mainDocumentURL];
  NSLog(@"***Loaded url: %@", myLoadedUrl);

  // if the UIWebView is showing our authorization URL or consent URL, show the
  // UIWebView control
  if ([request.URL.absoluteString rangeOfString:data.authURL
                                        options:NSCaseInsensitiveSearch]
          .location != NSNotFound) {
    self.loginView.hidden = NO;
  } else if ([request.URL.absoluteString rangeOfString:data.loginURL
                                               options:NSCaseInsensitiveSearch]
                 .location != NSNotFound) {
    // otherwise hide the UIWebView, we've left the authorization flow
    self.loginView.hidden = NO;
  } else if ([request.URL.absoluteString rangeOfString:data.bhh
                                               options:NSCaseInsensitiveSearch]
                 .location != NSNotFound) {
    // otherwise hide the UIWebView, we've left the authorization flow
    self.loginView.hidden = YES;
    [[NXOAuth2AccountStore sharedStore] handleRedirectURL:request.URL];
  } else {
    self.loginView.hidden = NO;
    // read the Location from the UIWebView, this is how Microsoft APIs is
    // returning the
    // authentication code and relation information. This is controlled by the
    // redirect URL we chose to use from Microsoft APIs
    // continue the OAuth2 flow
    // [[NXOAuth2AccountStore sharedStore] handleRedirectURL:request.URL];
  }

  return YES;
}

```

* Käsittelee OAuth2 pyynnön tulos koodin kirjoittaminen

Microsoft on koodi, joka käsittelee redirectURL, joka tulee takaisin näkymä. Jos se ei onnistunut, Yritämme uudelleen. Tutustu kirjaston antaa virheen, voit tarkastella konsolissa tai käsitellä asyncronously. 

```objc
- (void)handleOAuth2AccessResult:(NSURL *)accessResult {
  // parse the response for success or failure
  if (accessResult)
  // if success, complete the OAuth2 flow by handling the redirect URL and
  // obtaining a token
  {
    [[NXOAuth2AccountStore sharedStore] handleRedirectURL:accessResult];
  } else {
    // start over
    [self requestOAuth2Access];
  }
}
```

* Määritä ilmoituksen tehtaan.

Olemme muuttaminen samalla menetelmällä luodaan `AppDelegate` edellä, mutta tällä hetkellä lisäämme joitakin `NSNotification`s kerro, mitä tapahtuu meidän service. Kokeiluversion määrittäminen henkilöitä, joka kertoo us mitään muuttuessa tunnuksen kanssa. Kun olemme Hae tunnuksen palautettavien käyttäjän takaisin `masterView`.



```objc
- (void)setupOAuth2AccountStore {
  [[NSNotificationCenter defaultCenter]
      addObserverForName:NXOAuth2AccountStoreAccountsDidChangeNotification
                  object:[NXOAuth2AccountStore sharedStore]
                   queue:nil
              usingBlock:^(NSNotification *aNotification) {
                if (aNotification.userInfo) {
                  // account added, we have access
                  // we can now request protected data
                  NSLog(@"Success!! We have an access token.");
                  dispatch_async(dispatch_get_main_queue(), ^{

                    MasterViewController *masterViewController =
                        [self.storyboard
                            instantiateViewControllerWithIdentifier:@"master"];
                    [self.navigationController
                        pushViewController:masterViewController
                                  animated:YES];
                  });
                } else {
                  // account removed, we lost access
                }
              }];

  [[NSNotificationCenter defaultCenter]
      addObserverForName:NXOAuth2AccountStoreDidFailToRequestAccessNotification
                  object:[NXOAuth2AccountStore sharedStore]
                   queue:nil
              usingBlock:^(NSNotification *aNotification) {
                NSError *error = [aNotification.userInfo
                    objectForKey:NXOAuth2AccountStoreErrorKey];
                NSLog(@"Error!! %@", error.localizedDescription);
              }];
}

```
* Lisää tunnus, jolla käyttäjä käsittelee aina, kun pyyntö on aloitettu Kirjaudu native varten

Luo menetelmän, joita kutsutaan aina, kun on pyyntö todennusta varten. Tämä on sopiva menetelmä luo näkymä

```objc
- (void)requestOAuth2Access {
  AppData *data = [AppData getInstance];

  // in order to login to Mircosoft APIs using OAuth2 we must show an embedded
  // browser (UIWebView)
  [[NXOAuth2AccountStore sharedStore]
           requestAccessToAccountWithType:data.accountIdentifier
      withPreparedAuthorizationURLHandler:^(NSURL *preparedURL) {
        // navigate to the URL returned by NXOAuth2Client

        NSURLRequest *r = [NSURLRequest requestWithURL:preparedURL];
        [self.loginView loadRequest:r];
      }];
}
```

* Soita kaikki näistä tavoista, että olet kirjoittanut yläpuolella aina lopuksi oletetaan, että `LoginViewController` Lataa. Olemme tehdä tämän lisäämällä näistä tavoista, tutustu `viewDidLoad` Apple tapa antaa us

```objc
  [super viewDidLoad];
  // Do any additional setup after loading the view.

  // OAuth2 Code

  self.loginView.delegate = self;
  [self requestOAuth2Access];
  [self setupOAuth2AccountStore];
  NSURLCache *URLCache =
      [[NSURLCache alloc] initWithMemoryCapacity:4 * 1024 * 1024
                                    diskCapacity:20 * 1024 * 1024
                                        diskPath:nil];
  [NSURLCache setSharedURLCache:URLCache];
```

Olet nyt valmis luomisessa on käsitellä tämän sovelluksen kirjauduttaessa pääasiassa. Vaikka emme olet kirjautunut sisään, emme on käyttää Microsoftin tunnusten on saanut. Kohdalla olemme Luo helper lisäkoodin, jotka soittavat REST API meille käyttämällä tämän kirjaston varten.


## <a name="create-a-graphapicaller-class-to-handle-our-requests-to-a-rest-api"></a>Luo `GraphAPICaller` luokan Microsoftin pyyntöjen REST API

On ladattu aina, kun olemme ladata monipuolisimmat määrityksen. Nyt on sinun tehtävä joitakin sitä, kun tunnus on. 

* Luo `GraphAPICaller.h` tiedosto

```objc
@interface GraphAPICaller : NSObject <NSURLConnectionDataDelegate>

+ (void)addTask:(Task *)task
completionBlock:(void (^)(bool, NSError *error))completionBlock;

+ (void)getTaskList:(void (^)(NSMutableArray *, NSError *error))completionBlock;

@end
```

Näet tämän koodista, että olemme luovatko kahdella tavalla: yksi hakee tehtävät Ohjelmointirajapinnan ja toinen tehtävien lisääminen Ohjelmointirajapinnan.

Nyt, että olet määrittänyt meidän käyttöliittymän, lisääminen todellinen toteutuksen:

* Luo`GraphAPICaller.m file`

```objc
@implementation GraphAPICaller

// 
// Gets the tasks from our REST endpoint we specified in settings
//

+ (void)getTaskList:(void (^)(NSMutableArray *, NSError *))completionBlock

{
  AppData *data = [AppData getInstance];

  NSString *taskURL =
      [NSString stringWithFormat:@"%@%@", data.taskApiString, @"/api/tasks"];

  NXOAuth2AccountStore *store = [NXOAuth2AccountStore sharedStore];
  NSMutableArray *Tasks = [[NSMutableArray alloc] init];

  NSArray *accounts = [store accountsWithAccountType:data.accountIdentifier];
  [NXOAuth2Request performMethod:@"GET"
      onResource:[NSURL URLWithString:taskURL]
      usingParameters:nil
      withAccount:accounts[0]
      sendProgressHandler:^(unsigned long long bytesSend,
                            unsigned long long bytesTotal) {
        // e.g., update a progress indicator
      }
      responseHandler:^(NSURLResponse *response, NSData *responseData,
                        NSError *error) {
        // Process the response
        if (!error) {
          NSDictionary *dataReturned =
              [NSJSONSerialization JSONObjectWithData:responseData
                                              options:0
                                                error:nil];
          NSLog(@"Graph Response was: %@", dataReturned);

          if ([dataReturned count] != 0) {

            for (NSMutableDictionary *theTask in dataReturned) {

              Task *t = [[Task alloc] init];
              t.name = [theTask valueForKey:@"Text"];

              [Tasks addObject:t];
            }
          }

          completionBlock(Tasks, nil);
        } else {
          completionBlock(nil, error);
        }

      }];
}

// 
// Adds a task from our REST endpoint we specified in settings
//

+ (void)addTask:(Task *)task
completionBlock:(void (^)(bool, NSError *error))completionBlock {

  AppData *data = [AppData getInstance];

  NSString *taskURL =
      [NSString stringWithFormat:@"%@%@", data.taskApiString, @"/api/tasks"];

  NXOAuth2AccountStore *store = [NXOAuth2AccountStore sharedStore];
  NSDictionary *params = [self convertParamsToDictionary:task.name];

  NSArray *accounts = [store accountsWithAccountType:data.accountIdentifier];
  [NXOAuth2Request performMethod:@"POST"
      onResource:[NSURL URLWithString:taskURL]
      usingParameters:params
      withAccount:accounts[0]
      sendProgressHandler:^(unsigned long long bytesSend,
                            unsigned long long bytesTotal) {
        // e.g., update a progress indicator
      }
      responseHandler:^(NSURLResponse *response, NSData *responseData,
                        NSError *error) {
        // Process the response
        if (responseData) {
          NSDictionary *dataReturned =
              [NSJSONSerialization JSONObjectWithData:responseData
                                              options:0
                                                error:nil];
          NSLog(@"Graph Response was: %@", dataReturned);

          completionBlock(TRUE, nil);
        } else {
          completionBlock(FALSE, error);
        }

      }];
}

+ (NSDictionary *)convertParamsToDictionary:(NSString *)task {
  NSMutableDictionary *dictionary = [[NSMutableDictionary alloc] init];

  [dictionary setValue:task forKey:@"Text"];

  return dictionary;
}

@end
```

## <a name="run-the-sample-app"></a>Suorita malli-sovellus

Lopuksi muodosta ja suorita sovellus Xcode. Rekisteröi-sovellukseen sisäänkirjautuminen ja luoda kirjautunut sisään käyttäjän tehtävät. Kirjaudu ulos ja kirjaudu takaisin sisään eri käyttäjänä ja tehtävien kyseisen käyttäjän luominen.

Huomaa, että tehtäviä on tallennettu käyttäjäkohtainen-Ohjelmointirajapinnan, koska Ohjelmointirajapinnan poimii käyttäjän tunnistetiedot, jotka se saa käyttöoikeustietue.


## <a name="next-steps"></a>Seuraavat vaiheet

Voit nyt siirtää B2C aiheisiin sivulle. Voit yrittää:

[Soita Node.js verkko-Ohjelmointirajapinnan Node.js web Appista]()

[UX B2C sovelluksen mukauttaminen]()
