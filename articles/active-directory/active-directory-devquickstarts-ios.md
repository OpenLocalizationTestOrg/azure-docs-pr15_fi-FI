<properties
    pageTitle="Azure AD-iOS aloittaminen | Microsoft Azure"
    description="IOS-sovellus, joka integroituu Azure AD kirjauduttaessa ja kutsuu Azure AD muodostaminen suojattu ohjelmointirajapinnan käyttäminen OAuth."
    services="active-directory"
    documentationCenter="ios"
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="brandwe"/>

# <a name="integrate-azure-ad-into-an-ios-app"></a>Integroida Azure AD iOS-sovelluksessa

[AZURE.INCLUDE [active-directory-devquickstarts-switcher](../../includes/active-directory-devquickstarts-switcher.md)]

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)] 

Azure AD on Active Directory käyttöoikeuksien kirjaston tai ADAL-iOS-asiakkaille, jotka on suojattu resursseihin.  ADAL on ainoa tarkoitus-aika on sovelluksen access tunnusten pääset helposti.  Osoittaa, kuinka helppoa, tähän on Laadi tavoitteen C tehtäväluettelo-sovellus, joka:

-   Saa käyttää tunnusten soitettavien Azure AD-kaavio-Ohjelmointirajapinnan [OAuth 2.0 todennus-protokollan](https://msdn.microsoft.com/library/azure/dn645545.aspx)avulla.
-   Etsii käyttäjät, joilla on tietyn alias hakemistosta.

Tarvitset luonnissa valmis toimimasta-sovelluksen avulla:

2. Voit rekisteröidä sovelluksesi Azure AD.
3. Asenna ja määritä ADAL.
5. Pyydä tunnusten Azure AD ADAL avulla.

Jos haluat aloittaa, [Lataa sovelluksen rakenne](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/skeleton.zip) tai [Lataa Esimerkki valmiista](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/complete.zip).  Sinun on myös Azure AD-vuokraajan, jossa voit luoda käyttäjille ja sovelluksen rekisteröiminen.  Jos sinulla ei vielä ole vuokraajan, [Lue, miten voit hankkia sen](active-directory-howto-tenant.md).

> [AZURE.TIP] Kokeile Microsoftin, joka auttaa sinua lisämääritykset Azure Active Directory-hakemistosta vain muutaman minuutin kuluttua uuden [developer-portaalissa](https://identity.microsoft.com/Docs/iOS) esikatselu!  Developer-portaalissa opastaa sovellus rekisteröidään ja Azure AD integroiminen koodisi.  Kun olet valmis, käytössäsi on yksinkertainen sovellus, joka voi todentaa alihallintaan ja taustassa käyttäjät, jotka voit hyväksyä tunnusten ja suorittaa vahvistus. 

## <a name="1-determine-what-your-redirect-uri-will-be-for-ios"></a>*1. Selvitä, mitä uudelleenohjaaminen-URI on iOS*

Jos haluat käynnistää turvallisesti sovellusten SSO tietyissä tilanteissa on edellyttävät **Uudelleenohjaaminen URI** luominen tietyssä muodossa. Voit varmistaa, että paikkamerkkejä palauttaa oikean sovellukseen, joka pyytää niiden käytetään uudelleenohjaaminen URI.

Ohjaa URI iOS-muoto on:

```
<app-scheme>://<bundle-id>
```

-   **aap ja sen** – tämä on rekisteröity XCode projektin. Kuinka muut sovellukset voivat soittaa sinulle on. Voit etsiä tästä kohdassa Info.plist -> URL-Osoitteen tyypit -> URL-tunniste. Yksi kannattaa luoda, jos sinulla ei vielä ole vähintään yksi määritetty.
-   **pikaoppaista tunnus** - tämä on löytyy kohdasta "käyttäjätiedot" Pikaoppaista tunnus un XCode projektin asetukset.

Esimerkki pikaopas-koodin: ***msquickstart://com.microsoft.azureactivedirectory.samples.graph.QuickStart***

## <a name="2-register-the-directorysearcher-application"></a>*2. DirectorySearcher-sovelluksen rekisteröinti*
Sovelluksen hankkiminen tunnusten käyttöön ensin tarvitset rekisteröidään Azure AD-vuokraajan ja Myönnä käyttöoikeudet Azure AD-kaavio-Ohjelmointirajapinnan:

-   Kirjaudu sisään Azure hallinta-portaalissa
-   Valitse vasemmalla olevasta siirtymisruudussa **Active Directory**
-   Valitse vuokraajan, jolla sovellus rekisteröi.
-   Valitse **sovellukset** -välilehti ja valitse **Lisää** ala-laatikon.
-   Seuraa ohjeita ja luo uusi **Alkuperäisen asiakassovellukseen**.
    -   Sovelluksen **nimen** kuvataan sovelluksesi loppukäyttäjille
    -   **Uudelleenohjata Uri** on värimallin ja merkkijonon yhdistelmä, jota Azure AD käyttää suojaustunnuksen vastaukset palautetaan.  Kirjoita arvo, joka on tietyn sovelluksen edellä mainittujen tietojen perusteella.
-   Kun olet suorittanut rekisteröinti, AAD Määritä sovelluksesi asiakkaan yksilöllinen tunnus.  Tarvitset tätä arvoa seuraavat osiot niin kopioi se **määritys** -välilehti.
- Myös **määrittäminen** -välilehden Etsi "Oikeudet ja muut sovellukset"-kohta.  "Azure Active Directory"-sovelluksen Lisää **Valtuutetun käyttöoikeudet**-kohdassa **Access Your organisaation hakemisto** -käyttöoikeus.  Tämä ottaa käyttöön sovelluksen käyttäjille Graph-Ohjelmointirajapinnan kysely.

## <a name="3-install--configure-adal"></a>*3. Asenna ja määritä ADAL*
Nyt kun olet luonut Azure AD-sovelluksen, voit asentaa ADAL ja kirjoita käyttäjätietojen liittyvä koodi.  Myös ADAL voivat pitää yhteyttä Azure AD-pystyttävä toimittamaan sovelluksen rekisteröinti tietoja.
-   Aloita lisäämällä ADAL DirectorySearcher projektin Cocapods.

```
$ vi Podfile
```
Lisää tämä podfile seuraavasti:

```
source 'https://github.com/CocoaPods/Specs.git'
link_with ['QuickStart']
xcodeproj 'QuickStart'

pod 'ADALiOS'
```

Lataa nyt käyttämällä cocoapods podfile. Tämä luo uuden XCode työtilan haluat ladata.

```
$ pod install
...
$ open QuickStart.xcworkspace
```

-   Pikaopas Projectissa, Avaa tiedosto plist `settings.plist`.  Korvaa arvot osien osion kuvaavat arvoja, jotka olet kirjoittanut Azure-portaaliin.  Nämä arvot viittaavat koodisi aina, kun se käyttää ADAL.
    -   `tenant` Azure AD-vuokraajan, esimerkiksi contoso.onmicrosoft.com toimialue
    -   `clientId` On portaalista kopioimasi sovelluksen clientId.
    -   `redirectUri` Uudelleenohjausta on rekisteröity-portaalin URL-osoite.

## <a name="4--use-adal-to-get-tokens-from-aad"></a>*4. Käytä ADAL tunnusten käyttämistä AAD*
Perustietoja tarpeen ADAL takana on, että aina, kun sovellus on access-tunnuksen, riittää, että kutsuu completionBlock `+(void) getToken : `, eikä ADAL muille käyttäjille.  

-   Valitse `QuickStart` projektin, Avaa `GraphAPICaller.m` ja Etsi `// TODO: getToken for generic Web API flows. Returns a token with no additional parameters provided.` kommentti lähellä sivun.  Tämä on jossa ADAL välittää koordinaatteja CompletionBlock Azure AD yhteydenpito ja kuinka se välimuistiin tunnusten kautta.

```ObjC
+(void) getToken : (BOOL) clearCache
           parent:(UIViewController*) parent
completionHandler:(void (^) (NSString*, NSError*))completionBlock;
{
    AppData* data = [AppData getInstance];
    if(data.userItem){
        completionBlock(data.userItem.accessToken, nil);
        return;
    }

    ADAuthenticationError *error;
    authContext = [ADAuthenticationContext authenticationContextWithAuthority:data.authority error:&error];
    authContext.parentController = parent;
    NSURL *redirectUri = [[NSURL alloc]initWithString:data.redirectUriString];

    [ADAuthenticationSettings sharedInstance].enableFullScreen = YES;
    [authContext acquireTokenWithResource:data.resourceId
                                 clientId:data.clientId
                              redirectUri:redirectUri
                           promptBehavior:AD_PROMPT_AUTO
                                   userId:data.userItem.userInformation.userId
                     extraQueryParameters: @"nux=1" // if this strikes you as strange it was legacy to display the correct mobile UX. You most likely won't need it in your code.
                          completionBlock:^(ADAuthenticationResult *result) {

                              if (result.status != AD_SUCCEEDED)
                              {
                                  completionBlock(nil, result.error);
                              }
                              else
                              {
                                  data.userItem = result.tokenCacheStoreItem;
                                  completionBlock(result.tokenCacheStoreItem.accessToken, nil);
                              }
                          }];
}

```

- Nyt annettava tunnuksessa avulla voit etsiä käyttäjiä kaaviossa. Etsi `// TODO: implement SearchUsersList` kommentoi menetelmä tekee GET-pyynnössä kyselyyn Azure AD-kaavio-Ohjelmointirajapinnan käyttäjille, joiden UPN alkaa tietyllä hakusanoja.  Mutta kyselyn Graph-Ohjelmointirajapinta, jotta sinun täytyy lisätä access_token- `Authorization` otsikon tämä on pyynnön - kohtaa, johon ADAL on.

```ObjC
+(void) searchUserList:(NSString*)searchString
                parent:(UIViewController*) parent
       completionBlock:(void (^) (NSMutableArray* Users, NSError* error)) completionBlock
{
    if (!loadedApplicationSettings)
    {
        [self readApplicationSettings];
    }

    AppData* data = [AppData getInstance];

    NSString *graphURL = [NSString stringWithFormat:@"%@%@/users?api-version=%@&$filter=startswith(userPrincipalName, '%@')", data.taskWebApiUrlString, data.tenant, data.apiversion, searchString];


    [self craftRequest:[self.class trimString:graphURL]
                parent:parent
     completionHandler:^(NSMutableURLRequest *request, NSError *error) {

         if (error != nil)
         {
             completionBlock(nil, error);
         }
         else
         {

             NSOperationQueue *queue = [[NSOperationQueue alloc]init];

             [NSURLConnection sendAsynchronousRequest:request queue:queue completionHandler:^(NSURLResponse *response, NSData *data, NSError *error) {

                 if (error == nil && data != nil){

                     NSDictionary *dataReturned = [NSJSONSerialization JSONObjectWithData:data options:0 error:nil];

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
                         s.name =[keyValuePairs valueForKey:@"givenName"];

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
     }];

}

```
- Kun sovellus pyytää tunnusta soittamalla `getToken(...)`, ADAL yrittää palauttaa tunnusta kysymättä käyttäjän tunnistetiedot.  Jos ADAL määrittää, että käyttäjän täytyy kirjautua saat tunnusta, sisäänkirjautuminen-valintaikkunan näkyviin, kerätä käyttäjän tunnistetiedot ja palauttaa tunnuksen yhteydessä todentaminen onnistuu.  Jos ADAL ei voi palauttaa tunnusta jostakin syystä, se palauttaa `AdalException`.
- Huomaa, `AuthenticationResult` objekti sisältää `tokenCacheStoreItem` objektia, jonka avulla voit kerätä tietoja on ehkä sovellus.  Pikaopas, valitse `tokenCacheStoreItem` avulla voit määrittää authenitcation on tapahtunut jo.


## <a name="step-5-build-and-run-the-application"></a>Vaihe 5: Muodosta ja suorita sovellus



Onnittelen! Sinun on nyt toimimasta iOS-sovellus, joka on voi todentaa käyttäjät, Soita suojatusti verkko-ohjelmointirajapinnan käyttäminen OAuth 2.0, ja poistaa käyttäjän perustietoja.  Jos et ole jo, nyt on aika, joka täyttää alihallintaan joidenkin käyttäjien kanssa.  Suorita pikaopas-sovellus ja kirjaudu sisään jokin kyseisille käyttäjille.  Etsi muiden käyttäjien niiden UPN perusteella.  Sulje sovellus ja käyttää sitä.  Huomaa, miten käyttäjän istunnon säilyy muuttumattomana.

ADAL voi liittää kaikkia näitä yleisiä identity-ominaisuuksia sovelluksesi on helppoa.  Muokattu-työstä kestää huolellisesti, - välimuistin hallinnan, OAuth-protokollatuki esitykset käyttäjän kirjautuminen Käyttöliittymä, päivittäminen ja vanhentuneet tunnusten.  Kaikki on varmasti tiedettävä on yksittäinen API-kutsu `getToken`.

Viittaus (ilman määritysarvot) Esimerkki valmiista toimitetaan [Tässä](https://github.com/AzureADQuickStarts/NativeClient-iOS/archive/complete.zip).  

## <a name="additional-scenarios"></a>Lisää skenaarioita
Voit nyt siirtää Lisää skenaarioita.  Voit halutessasi kokeilla:

- [Suojatun Node.JS verkko-Ohjelmointirajapinnan kanssa Azure AD](active-directory-devquickstarts-webapi-nodejs.md)
- [IOS käyttämällä ADAL SSO rajat-sovelluksen](active-directory-sso-ios.md) käyttöönotto  

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
