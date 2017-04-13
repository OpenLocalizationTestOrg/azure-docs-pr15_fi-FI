<properties
    pageTitle="Azure AD-v2.0 iOS-sovelluksessa | Microsoft Azure"
    description="Miten voit luoda iOS-sovellusta, joka kirjautuu kolmannen osapuolen kirjastojen avulla käyttäjät, joilla sekä henkilökohtainen Microsoft-tili tai työpaikan tai oppilaitoksen tilit."
    services="active-directory"
    documentationCenter=""
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="06/28/2016"
    ms.author="brandwe"/>

# <a name="add-sign-in-to-an-ios-app-using-a-third-party-library-with-graph-api-using-the-v20-endpoint"></a>Kirjaudu sisään kolmannen osapuolen kirjaston käyttäminen Graph-Ohjelmointirajapinnan käyttäminen v2.0 päätepisteen iOS-sovelluksen lisääminen

Microsoft identity-ympäristössä käyttää avoimia standardeja, kuten OAuth2 ja OpenID yhteyden. Kehittäjät voivat käyttää kaikissa kirjastoissa he haluavat integroida palveluiden kanssa. Jotta sovelluskehittäjät Microsoftin ympäristö käyttäminen muita kirjastoja, että olet kirjoittanut muutama vaihe vaiheelta, kuten näytettyjä osoittamaan määrittäminen kolmannen osapuolen kirjastot, jos haluat muodostaa yhteyden Microsoft identity-ympäristössä. Useimmat kirjastoissa, jotka toteuttavat [RFC6749 OAuth2 määritys](https://tools.ietf.org/html/rfc6749) muodostaa yhteyden Microsoft identity-ympäristössä.

Käyttäjät voivat, joka luo tätä vaiheittaista sovelluksessa organisaation kirjautuminen ja sitten etsiminen muiden organisaation Graph-Ohjelmointirajapinnan.

Jos ole ennen käyttänyt OAuth2 tai OpenID muodosta, paljon otoksen määritysten saattaa ei tulkita sinulle. On suositeltavaa lukea [v2.0 protokollat - OAuth 2.0 luvan koodin Flow](active-directory-v2-protocols-oauth-code.md) taustan.


> [AZURE.NOTE]
    Microsoftin ympäristö ominaisuuksia, joilla lausekkeen OAuth2 tai OpenID muodosta standardeja, kuten ehdollisen käyttöoikeuden ja Intune hallinnassa edellyttää Microsoftin Avaa Microsoft Azure tunnistetietojen kirjastojen tietolähdettä käytettäessä.

V2.0 päätepiste ei tue kaikkia Azure Active Directory-skenaariot ja toiminnot.

> [AZURE.NOTE]
    Voit selvittää, jos sinun on käytettävä v2.0 päätepisteen, lue lisätietoja [v2.0 rajoitukset](active-directory-v2-limitations.md).

## <a name="download-code-from-github"></a>Lataa koodin GitHub
Tässä opetusohjelmassa koodi määritetään [GitHub](https://github.com/Azure-Samples/active-directory-ios-native-nxoauth2-v2).  Jos haluat seurata, voit [ladata sovelluksen rakenne .zip nimellä](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/skeleton.zip) tai Kloonaa rakenne:

```
git clone --branch skeleton git@github.com:Azure-Samples/active-directory-ios-native-nxoauth2-v2.git
```

Voit ladata yksinkertaisesti otosten ja aloittaa saman tien:

```
git clone git@github.com:Azure-Samples/active-directory-ios-native-nxoauth2-v2.git
```

## <a name="register-an-app"></a>Sovelluksen rekisteröiminen
Uuden sovelluksen luominen [sovelluksen rekisteröinnin portaalissa](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)osoitteessa tai [rekisteröimään sovelluksen v2.0 päätepisteissä](active-directory-v2-app-registration.md)yksityiskohtaiset noudattamalla.  Varmista, että:

- Kopioi **Tunnus** , jolla määritetään sovelluksen, sillä tarvitset sitä pian.
- Lisää, kun sovellus **Mobile** -ympäristössä.
- Kopioi **Uudelleenohjata URI** -portaalista. Sinun on käytettävä oletusarvon `urn:ietf:wg:oauth:2.0:oob`.


## <a name="download-the-third-party-nxoauth2-library-and-create-a-workspace"></a>Lataa kolmannen osapuolen NXOAuth2 kirjasto ja työtilan luominen

Näiden vaiheiden käytetään OAuth2Client-GitHub, joka on Mac OS X-ja iOS (kaakao ja kosketa kaakao) OAuth2-kirjasto. Tämä kirjasto perustuu OAuth2 määritys 10 luonnoksen. Se toteuttaa alkuperäisen sovelluksen profiilin ja tukee käyttäjän luvan päätepisteelle. Nämä ovat asioista, joita sinun täytyy integroida Microsoft identity-ympäristössä.

### <a name="add-the-library-to-your-project-by-using-cocoapods"></a>Kirjaston lisääminen projektiin CocoaPods avulla

CocoaPods on Xcode projektien riippuvuuden valvoja. Edellisen asennusohjeita hallitaan automaattisesti.

```
$ vi Podfile
```
1. Lisää tämä podfile seuraavasti:

    ```
     platform :ios, '8.0'

     target 'QuickStart' do

     pod 'NXOAuth2Client'

     end
    ```

2. Lataa podfile CocoaPods avulla. Tämä vaihtoehto Luo uusi Xcode työtila, johon haluat ladata.

    ```
    $ pod install
    ...
    $ open QuickStart.xcworkspace
    ```

## <a name="explore-the-structure-of-the-project"></a>Tutustu projektin rakenteessa

Seuraava rakenne on määritetty rakenne Microsoftin projektin:

- Täydellisen Käyttäjätunnuksen haulla perustyyli-näkymässä
- Yksityiskohtaisen näkymän tietojen valitun käyttäjän tietoja
- Kirjaudu sisään näkymän kohtaa, johon käyttäjä kirjautumaan sovellukseen kaavion kysely

Olemme siirtyvät eri tiedostot rakenne Lisää todennusta. Koodi, esimerkiksi visual koodin tai muita osia ei koskevat tunnistetiedot, mutta toimitetaan puolestasi.

## <a name="set-up-the-settingsplst-file-in-the-library"></a>Kirjaston tiedoston settings.plst määrittäminen

-   Pikaopas Projectissa, Avaa `settings.plist` tiedosto. Korvaa arvot osien osion vastaavat arvot, joita olet käyttänyt Azure-portaalissa. Koodi viittaa nämä arvot aina, kun se käyttää Active Directory käyttöoikeuksien kirjastoon.
    -   `clientId` On Asiakastunnus-sovelluksen, jonka kopioit portaaliin.
    -   `redirectUri` Uudelleenohjaus URL-osoite portaalin annettu.

## <a name="set-up-the-nxoauth2client-library-in-your-loginviewcontroller"></a>Oman LoginViewController NXOAuth2Client kirjastossa määrittäminen

Joitakin arvoja, voit määrittää edellytetään NXOAuth2Client-kirjastossa. Sen jälkeen, kun tehtävä on valmis, voit soittaa Graph-Ohjelmointirajapinnan luetun tunnuksen. Koska `LoginView` on nimeltään aina annettava todennetaan, se on järkevää määritysten arvot Vie tiedostoon.

- Lisääminen joitakin arvoja `LoginViewController.m` määrittäminen todennus- ja konteksti-tiedosto. Lisätietoja arvot noudattamalla koodi.

    ```objc
    NSString *scopes = @"openid offline_access User.Read";
    NSString *authURL = @"https://login.microsoftonline.com/common/oauth2/v2.0/authorize";
    NSString *loginURL = @"https://login.microsoftonline.com/common/login";
    NSString *bhh = @"urn:ietf:wg:oauth:2.0:oob?code=";
    NSString *tokenURL = @"https://login.microsoftonline.com/common/oauth2/v2.0/token";
    NSString *keychain = @"com.microsoft.azureactivedirectory.samples.graph.QuickStart";
    static NSString * const kIDMOAuth2SuccessPagePrefix = @"session_state=";
    NSURL *myRequestedUrl;
    NSURL *myLoadedUrl;
    bool loginFlow = FALSE;
    bool isRequestBusy;
    NSURL *authcode;
    ```

Katsotaan koodin tietoja.

Ensimmäisen merkkijonon koskee `scopes`.  `User.Read` Arvon avulla voit helposti lukea perustiedot kirjautuneena käyttäjän profiiliin.

Voit lukea lisää kaikki käytettävissä olevat alueet, [Microsoft Graph käyttöoikeuksien käyttöalueet](https://graph.microsoft.io/docs/authorization/permission_scopes).

Saat `authURL`, `loginURL`, `bhh`, ja `tokenURL`, sinun on käytettävä aiemmin annettujen arvojen. Jos käytät Avaa lähde Microsoft Azure tunnistetietojen kirjastot, on avattava tiedot puolestasi käyttämällä Microsoftin metatietojen päätepiste. Olemme tehdä työtä näyttämisestä nämä arvot puolestasi.

`keychain` Arvo on säilö, NXOAuth2Client kirjasto avulla voit luoda oman tunnusten tallentamiseen avainnipun. Jos haluat saada kertakirjautuminen (SSO) käytettävistä useiden sovelluksista, voit määrittää saman avainnipun kaikkien sovellustesi ja pyydä, että avainnipun käyttö Xcode oikeudet. Tämä on selitetty Apple-ohjeista.

Nämä arvot loput tarvitaan kirjasto ja luoda voit suorittaa arvot yhteydessä.

### <a name="create-a-url-cache"></a>Luo URL-välimuisti

Sisällä `(void)viewDidLoad()`, jossa kutsutaan aina, kun näkymä on ladattu, seuraava koodi primes Käytämme välimuistissa.

Lisää seuraava koodi:

```objc
- (void)viewDidLoad {
    [super viewDidLoad];
    self.loginView.delegate = self;
    [self setupOAuth2AccountStore];
    [self requestOAuth2Access];
    NSURLCache *URLCache = [[NSURLCache alloc] initWithMemoryCapacity:4 * 1024 * 1024
                                                         diskCapacity:20 * 1024 * 1024
                                                             diskPath:nil];
    [NSURLCache setSharedURLCache:URLCache];

}
```

### <a name="create-a-webview-for-sign-in"></a>Luo näkymä kirjautumista varten

Näkymä voit käyttäjältä muut tekijät, kuten tekstin tekstiviesti (Jos määritetty) tai palaa käyttäjän virhesanomia. Tähän määrität ylös näkymä ja kirjoita myöhemmin koodin käsittelemään takaisinkutsuja, tapahtuu tunnistetietojen palvelut-näkymä.

```objc
-(void)requestOAuth2Access {
    //to sign in to Microsoft APIs using OAuth2, we must show an embedded browser (UIWebView)
    [[NXOAuth2AccountStore sharedStore] requestAccessToAccountWithType:@"myGraphService"
                                   withPreparedAuthorizationURLHandler:^(NSURL *preparedURL) {
                                       //navigate to the URL returned by NXOAuth2Client

                                       NSURLRequest *r = [NSURLRequest requestWithURL:preparedURL];
                                       [self.loginView loadRequest:r];
                                   }];
}
```

### <a name="override-the-webview-methods-to-handle-authentication"></a>Ohita näkymä menetelmiä käsittelemään todennus

Onko näkymä, mitä tapahtuu, kun käyttäjä on kirjauduttava sisään, kuten edellä aiemmin, voit liittää seuraava koodi.

```objc
- (void)resolveUsingUIWebView:(NSURL *)URL {

    // We get the auth token from a redirect so we need to handle that in the webview.

    if (![NSThread isMainThread]) {
        [self performSelectorOnMainThread:@selector(resolveUsingUIWebView:) withObject:URL waitUntilDone:YES];
        return;
    }

    NSURLRequest *hostnameURLRequest = [NSURLRequest requestWithURL:URL cachePolicy:NSURLRequestUseProtocolCachePolicy timeoutInterval:10.0f];
    isRequestBusy = YES;
    [self.loginView loadRequest:hostnameURLRequest];

    NSLog(@"resolveUsingUIWebView ready (status: UNKNOWN, URL: %@)", self.loginView.request.URL);
}

- (BOOL)webView:(UIWebView *)webView shouldStartLoadWithRequest:(NSURLRequest *)request navigationType:(UIWebViewNavigationType)navigationType {

    NSLog(@"webView:shouldStartLoadWithRequest: %@ (%li)", request.URL, (long)navigationType);

    // The webview is where all the communication happens. Slightly complicated.

    myLoadedUrl = [webView.request mainDocumentURL];
    NSLog(@"***Loaded url: %@", myLoadedUrl);

    //if the UIWebView is showing our authorization URL or consent URL, show the UIWebView control
    if ([request.URL.absoluteString rangeOfString:authURL options:NSCaseInsensitiveSearch].location != NSNotFound) {
        self.loginView.hidden = NO;
    } else if ([request.URL.absoluteString rangeOfString:loginURL options:NSCaseInsensitiveSearch].location != NSNotFound) {
        //otherwise hide the UIWebView, we've left the authorization flow
        self.loginView.hidden = NO;
    } else if ([request.URL.absoluteString rangeOfString:bhh options:NSCaseInsensitiveSearch].location != NSNotFound) {
        //otherwise hide the UIWebView, we've left the authorization flow
        self.loginView.hidden = YES;
        [[NXOAuth2AccountStore sharedStore] handleRedirectURL:request.URL];
    }
    else {
        self.loginView.hidden = NO;
        //read the Location from the UIWebView, this is how Microsoft APIs is returning the
        //authentication code and relation information. This is controlled by the redirect URL we chose to use from Microsoft APIs
        //continue the OAuth2 flow
       // [[NXOAuth2AccountStore sharedStore] handleRedirectURL:request.URL];
    }

    return YES;

}
```

### <a name="write-code-to-handle-the-result-of-the-oauth2-request"></a>Käsittelee OAuth2 pyynnön tulos koodin kirjoittaminen

Seuraava koodi käsittelee redirectURL, joka palauttaa-näkymä. Jos todennus ei onnistu, koodi yrittää uudelleen. Tänä aikana kirjaston toimi, voit tarkastella konsolissa tai käsitellä asynkronisesti virhe.

```objc
- (void)handleOAuth2AccessResult:(NSString *)accessResult {

    AppData* data = [AppData getInstance];

    //parse the response for success or failure
     if (accessResult)
    //if success, complete the OAuth2 flow by handling the redirect URL and obtaining a token
     {
         [[NXOAuth2AccountStore sharedStore] handleRedirectURL:accessResult];
    } else {
        //start over
        [self requestOAuth2Access];
    }
}
```

### <a name="set-up-the-oauth-context-called-account-store"></a>OAuth yhteydessä (eli tilin kaupan) määrittäminen

Voit kutsua `-[NXOAuth2AccountStore setClientID:secret:authorizationURL:tokenURL:redirectURL:forAccountType:]` jaetun tilin säilössä, jotka haluat sovellus voi käyttää kunkin palvelun. Tilin tyyppi on merkkijono, jota käytetään tiettyjen palvelun tunnus. Kun käytät Graph-Ohjelmointirajapinta, koska koodi viittaa siihen kuin `"myGraphService"`. Voit määrittää sitten henkilöitä, joka kertoo, kun mitään muutokset tunnuksen. Kun saat tunnuksen, voit palauttaa käyttäjän takaisin `masterView`.



```objc
- (void)setupOAuth2AccountStore {


        AppData* data = [AppData getInstance];

    [[NXOAuth2AccountStore sharedStore] setClientID:data.clientId
                                             secret:data.secret
                                              scope:[NSSet setWithObject:scopes]
                                   authorizationURL:[NSURL URLWithString:authURL]
                                           tokenURL:[NSURL URLWithString:tokenURL]
                                        redirectURL:[NSURL URLWithString:data.redirectUriString]
                                      keyChainGroup: keychain
                                     forAccountType:@"myGraphService"];

    [[NSNotificationCenter defaultCenter] addObserverForName:NXOAuth2AccountStoreAccountsDidChangeNotification
                                                      object:[NXOAuth2AccountStore sharedStore]
                                                       queue:nil
                                                  usingBlock:^(NSNotification *aNotification) {
                                                      if (aNotification.userInfo) {
                                                          //account added, we have access
                                                          //we can now request protected data
                                                          NSLog(@"Success!! We have an access token.");
                                                          dispatch_async(dispatch_get_main_queue(),^ {

                                                              MasterViewController* masterViewController = [self.storyboard instantiateViewControllerWithIdentifier:@"masterView"];
                                                              [self.navigationController pushViewController:masterViewController animated:YES];
                                                          });
                                                      } else {
                                                          //account removed, we lost access
                                                      }
                                                  }];

    [[NSNotificationCenter defaultCenter] addObserverForName:NXOAuth2AccountStoreDidFailToRequestAccessNotification
                                                      object:[NXOAuth2AccountStore sharedStore]
                                                       queue:nil
                                                  usingBlock:^(NSNotification *aNotification) {
                                                      NSError *error = [aNotification.userInfo objectForKey:NXOAuth2AccountStoreErrorKey];
                                                      NSLog(@"Error!! %@", error.localizedDescription);
                                                  }];
}
```

## <a name="set-up-the-master-view-to-search-and-display-the-users-from-the-graph-api"></a>Määritä haun ja näyttää kaavion Ohjelmointirajapinnan käyttäjiltä perustyyli-näkymä

Perustyyli-näkymässä-ohjain (MVC)-sovellusta, joka näyttää palautetut tiedot ruudukossa ei käsitellä tätä vaiheittaista ja monia online-opetusohjelmat kerrotaan, miten voit luoda yhden. Tämä koodi on rakenne-tiedosto. Kuitenkin haluat käsitellä muutama seikka MVC sovelluksessa:

* Kun käyttäjä kirjoittaa jotain hakukentän leikkauspiste
* Objektin tiedot antaa takaisin MasterView, jolla se näyttää tulokset ruudukossa

Olemme tee ne alla.

### <a name="add-a-check-to-see-if-youre-logged-in"></a>Lisää Tarkista, jos olet kirjautunut sisään

Sovellus ei pientä Jos käyttäjä ei ole kirjautunut sisään, joten voi olla smart Tarkista jo tunnusta välimuistin. Jos et, voit ohjata käsiteltäessä LoginView käyttäjän kirjautuminen. Jos muista, käyttämään on paras tapa tehdä toimintoja näkymän latautuessa `viewDidLoad()` Apple tunnetaan myös us.

```objc
- (void)viewDidLoad {
    [super viewDidLoad];


    NXOAuth2AccountStore *store = [NXOAuth2AccountStore sharedStore];
    NSArray *accounts = [store accountsWithAccountType:@"myGraphService"];

        if (accounts.count == 0) {

        dispatch_async(dispatch_get_main_queue(),^ {

            LoginViewController* userSelectController = [self.storyboard instantiateViewControllerWithIdentifier:@"LoginUserView"];
            [self.navigationController pushViewController:userSelectController animated:YES];
        });
        }
```

### <a name="update-the-table-view-when-data-is-received"></a>Päivitä taulukko-näkymä, kun tiedot on vastaanotettu

Graph-Ohjelmointirajapinnan palauttaa tiedot, kun haluat näyttää tiedot. Yksinkertaisuuden näin koodin Päivitä taulukko. Voit liittää vain oikeita arvoja MVC asiakirjan osien uudelleen käyttäminen koodiin.

```objc
#pragma mark - Table View

- (NSInteger)numberOfSectionsInTableView:(UITableView *)tableView {
    return 1;
}

- (NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section {

        return [upnArray count];
}

- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath {

    UITableViewCell *cell = [tableView dequeueReusableCellWithIdentifier:@"TaskPrototypeCell" forIndexPath:indexPath];

    if ( cell == nil ) {
        cell = [[UITableViewCell alloc] initWithStyle:UITableViewCellStyleDefault reuseIdentifier:@"TaskPrototypeCell"];
    }

    User *user = nil;
     user = [upnArray objectAtIndex:indexPath.row];


    // Configure the cell
    cell.textLabel.text = user.name;
    [cell setAccessoryType:UITableViewCellAccessoryDisclosureIndicator];

    return cell;
}

```

### <a name="provide-a-way-to-call-the-graph-api-when-someone-types-in-the-search-field"></a>Soita Graph-Ohjelmointirajapinta, kun joku kirjoittaa hakukenttään lisäämistapaa

Kun käyttäjä kirjoittaa haku-ruutuun, sinun täytyy shove, Graph-ohjelmointirajapinnan. `GraphAPICaller` Luokan, jossa voit luoda seuraavan koodin, erottaa haku-toiminnon esityksestä. Nyt kirjoittaa oletetaan, että tunnus, jolla syötteet Graph-ohjelmointirajapinnan haku-merkkejä. Olemme tehdä tämän menetelmän antamalla `lookupInGraph`, jossa on merkkijono, jonka haluat etsiä.

```objc

-(void)lookupInGraph:(NSString *)searchText {
if (searchText.length > 0) {

    };



        [GraphAPICaller searchUserList:searchText completionBlock:^(NSMutableArray* returnedUpns, NSError* error) {
            if (returnedUpns) {


                upnArray = returnedUpns;


            }
            else
            {
                UIAlertView *alertView = [[UIAlertView alloc]initWithTitle:nil message:[[NSString alloc]initWithFormat:@"Error : %@", error.localizedDescription] delegate:nil cancelButtonTitle:@"Retry" otherButtonTitles:@"Cancel", nil];

                [alertView setDelegate:self];

                dispatch_async(dispatch_get_main_queue(),^ {
                    [alertView show];
                });
            }


        }];


}
```

## <a name="write-a-helper-class-to-access-the-graph-api"></a>Voit käyttää kaavion Ohjelmointirajapinnan Helper luokan kirjoittaminen

Tämä on Microsoftin sovelluksen core. Olisi muiden oli lisäämiskomento oletusarvon MVC rakenteessa Applelta, tähän kirjoittaa koodin kaavio kysely nimellä käyttäjän ja palauttaa tiedot. Näin koodi ja tarkan sanaa.

### <a name="create-a-new-objective-c-header-file"></a>Luo uusi tavoitteen C otsikko-tiedosto

Nimeä tiedosto `GraphAPICaller.h`, ja lisää seuraava koodi.

```objc
@interface GraphAPICaller : NSObject<NSURLConnectionDataDelegate>

+(void) searchUserList:(NSString*)searchString
       completionBlock:(void (^) (NSMutableArray*, NSError* error))completionBlock;

@end
```

Tässä näet, että määritetyn menetelmä tekstimerkkijonon ja palauttaa completionBlock. Tämä completionBlock, kun sinulla on arvata, päivittää taulukon antamalla objektin käyttäjän hakujen reaaliajassa täyttää tiedot.


### <a name="create-a-new-objective-c-file"></a>Luo uusi tavoitteen C-tiedosto

Nimeä tiedosto `GraphAPICaller.m`, ja lisää seuraavalla tavalla.

```objc
+(void) searchUserList:(NSString*)searchString
       completionBlock:(void (^) (NSMutableArray* Users, NSError* error)) completionBlock
{
    if (!loadedApplicationSettings)
    {
        [self readApplicationSettings];
    }

    AppData* data = [AppData getInstance];

    NSString *graphURL = [NSString stringWithFormat:@"%@%@/users", data.graphApiUrlString, data.apiversion];

    NXOAuth2AccountStore *store = [NXOAuth2AccountStore sharedStore];
    NSDictionary* params = [self convertParamsToDictionary:searchString];

    NSArray *accounts = [store accountsWithAccountType:@"myGraphService"];
    [NXOAuth2Request performMethod:@"GET"
                        onResource:[NSURL URLWithString:graphURL]
                   usingParameters:params
                       withAccount:accounts[0]
               sendProgressHandler:^(unsigned long long bytesSend, unsigned long long bytesTotal) {
                   // e.g., update a progress indicator
               }
                   responseHandler:^(NSURLResponse *response, NSData *responseData, NSError *error) {
                       // Process the response
                       if (responseData) {
                           NSError *error;
                           NSDictionary *dataReturned = [NSJSONSerialization JSONObjectWithData:responseData options:0 error:nil];
                           NSLog(@"Graph Response was: %@", dataReturned);

                           // We can grab the top most JSON node to get our graph data.
                           NSArray *graphDataArray = [dataReturned objectForKey:@"value"];

                           // Don't be thrown off by the key name being "value". It really is the name of the
                           // first node. :-)

                           //each object is a key value pair
                           NSDictionary *keyValuePairs;
                           NSMutableArray* Users = [[NSMutableArray alloc]init];

                           for(int i =0; i < graphDataArray.count; i++)
                           {
                               keyValuePairs = [graphDataArray objectAtIndex:i];

                               User *s = [[User alloc]init];
                               s.upn = [keyValuePairs valueForKey:@"userPrincipalName"];
                               s.name =[keyValuePairs valueForKey:@"displayName"];
                               s.mail =[keyValuePairs valueForKey:@"mail"];
                               s.businessPhones =[keyValuePairs valueForKey:@"businessPhones"];
                               s.mobilePhones =[keyValuePairs valueForKey:@"mobilePhone"];


                               [Users addObject:s];
                           }

                           completionBlock(Users, nil);
                       }
                       else
                       {
                           completionBlock(nil, error);
                       }

                   }];
}

```

Seuraavassa kerrotaan yksityiskohtaisesti tätä menetelmää.

Ovat tärkeä osa koodi `NXOAuth2Request`-menetelmä, joka käyttää parametreja, jotka olet jo määrittänyt settings.plist-tiedostossa.

Ensimmäiseksi on muodostaa oikean Graph API-kutsu. Koska soitat `/users`, määrittää, että Graph API resurssin versio sekä liittämällä. Se on järkevää Aseta näitä ulkoisen asetustiedoston, koska nämä voit muuttaa Ohjelmointirajapinnan kehitystyö.


```objc
NSString *graphURL = [NSString stringWithFormat:@"%@%@/users", data.graphApiUrlString, data.apiversion];
```

Seuraavaksi haluat määrittää parametreja, voit antaa myös kaavion API-kutsu. On *tärkeää* , että voit Laita parametrit resurssin päätepisteen koska, joka on hangattu kaikki-URI Vaatimustenmukaisen merkit suorituksen aikana. Kaikki kyselyn koodi on annettava parametrit.

```objc

NSDictionary* params = [self convertParamsToDictionary:searchString];
```

Voit havaita tämän `convertParamsToDictionary` menetelmä, jota ei ole kirjoitettu vielä. Tee katsotaan nyt tiedoston lopussa:

```objc
+(NSDictionary*) convertParamsToDictionary:(NSString*)searchString
{
    NSMutableDictionary* dictionary = [[NSMutableDictionary alloc]init];

        NSString *query = [NSString stringWithFormat:@"startswith(givenName, '%@')", searchString];

           [dictionary setValue:query forKey:@"$filter"];



    return dictionary;
}

```
Seuraavaksi Käytämme `NXOAuth2Request` tavan tietojen noutaminen takaisin Ohjelmointirajapinnan JSON-muodossa.

```objc
NSArray *accounts = [store accountsWithAccountType:@"myGraphService"];
    [NXOAuth2Request performMethod:@"GET"
                        onResource:[NSURL URLWithString:graphURL]
                   usingParameters:params
                       withAccount:accounts[0]
               sendProgressHandler:^(unsigned long long bytesSend, unsigned long long bytesTotal) {
                   // e.g., update a progress indicator
               }
                   responseHandler:^(NSURLResponse *response, NSData *responseData, NSError *error) {
                       // Process the response
                       if (responseData) {
                           NSError *error;
                           NSDictionary *dataReturned = [NSJSONSerialization JSONObjectWithData:responseData options:0 error:nil];
                           NSLog(@"Graph Response was: %@", dataReturned);

                           // We can grab the top most JSON node to get our graph data.
                           NSArray *graphDataArray = [dataReturned objectForKey:@"value"];
```

Lopuksi katsotaan siitä, miten voit palauttaa tiedot MasterViewController. Tietojen Poista palauttaa, ja se täytyy poistaa ja ladattu objekti, joka MainViewController voi käyttää. Tämän vuoksi rakenne on `User.m/h` tiedosto, joka luo käyttäjäobjektin. Voit lisätä kyseisen käyttäjäobjektin, jossa kaavion tiedot.

```objc
                           // We can grab the top most JSON node to get our graph data.
                           NSArray *graphDataArray = [dataReturned objectForKey:@"value"];

                           // Don't be thrown off by the key name being "value". It really is the name of the
                           // first node. :-)

                           //each object is a key value pair
                           NSDictionary *keyValuePairs;
                           NSMutableArray* Users = [[NSMutableArray alloc]init];

                           for(int i =0; i < graphDataArray.count; i++)
                           {
                               keyValuePairs = [graphDataArray objectAtIndex:i];

                               User *s = [[User alloc]init];
                               s.upn = [keyValuePairs valueForKey:@"userPrincipalName"];
                               s.name =[keyValuePairs valueForKey:@"displayName"];
                               s.mail =[keyValuePairs valueForKey:@"mail"];
                               s.businessPhones =[keyValuePairs valueForKey:@"businessPhones"];
                               s.mobilePhones =[keyValuePairs valueForKey:@"mobilePhone"];


                               [Users addObject:s];
```


## <a name="run-the-sample"></a>Suorita otosten

Jos olet käyttänyt rakenne tai sekä sovelluksen pitäisi nyt suorittaa ongelmatilanteita ja. Käynnistä simulator ja valitse **Kirjaudu sisään** sovelluksen käyttöä varten.

## <a name="get-security-updates-for-our-product"></a>Päivitysten hakeminen tuote

On suositeltavaa, kun suojauksen tapaukset esiintyvät ohjesisältöä [Suojauksen TechCenter](https://technet.microsoft.com/security/dd252948) ja tilaamalla neuvoa suojausvaroitusten ilmoitusten saaminen.
