<properties
    pageTitle="Voit käyttää iOS SDK Azure-mobiilisovellukset"
    description="Voit käyttää iOS SDK Azure-mobiilisovellukset"
    services="app-service\mobile"
    documentationCenter="ios"
    authors="ysxu"
    manager="yochayk"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="yuaxu"/>

# <a name="how-to-use-ios-client-library-for-azure-mobile-apps"></a>Voit käyttää iOS asiakkaan kirjasto Azure-mobiilisovellukset

[AZURE.INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

Tämän oppaan avulla voit suorittaa yleisiä tilanteita, joissa käyttämällä uusimman [Azure-mobiilisovellukset iOS SDK][1]. Jos ole ennen käyttänyt Azure Mobile-sovellusten, ensimmäinen valmis [Azure Mobile sovellusten Pika-aloitus] luominen taustassa-taulukon luominen ja lataa valmiita iOS Xcode projektin. Tässä oppaassa on keskittyä asiakkaan iOS SDK. Lisätietoja taustaan palvelinpuolen SDK-kohdassa palvelimen SDK HOWTOs.

## <a name="reference-documentation"></a>Oppaat

IOS-asiakasohjelman SDK oppaat sijaitsee seuraavassa: [Azure-mobiilisovellukset iOS asiakkaan viittaus][2].

## <a name="supported-platforms"></a>Tuetut käyttöympäristöt

IOS SDK tukee tavoite-C projekteja, Swift 2.2, ja Swift 2.3 projekteissa iOS 8.0 tai uudempi versio.

"Tiedonkulun server-todennus käyttää näkymä esitetty UI.  Jos laite ei voi esittää näkymä-Käyttöliittymä, ja valitse toinen tapa todennus vaaditaan, joka on tuotteen ulkopuolella.  
Tämä SDK ei siis sovellu Watch type tai vastaavasti rajoitettu laitteiden.

##<a name="Setup"></a>Asennus- ja edellytykset

Tässä oppaassa oletetaan, että olet luonut taustassa taulukosta. Tässä oppaassa oletetaan, että taulukossa on samaan rakenteeseen taulukoina näiden Opetusohjelmissa. Tässä oppaassa oletetaan, että omassa koodissasi, voit viitata myös `MicrosoftAzureMobile.framework` ja tuoda `MicrosoftAzureMobile/MicrosoftAzureMobile.h`.

##<a name="create-client"></a>Toimintaohje: asiakkaan luominen

Käyttämään Azure-mobiilisovellukset-taustatietokannan projektin luominen `MSClient`. Korvaa `AppUrl` ja sovelluksen URL-osoite. Sivuista voi jäädä `gatewayURLString` ja `applicationKey` tyhjä. Jos olet määrittänyt todennustavaksi yhdyskäytävän, täytä `gatewayURLString` ja yhdyskäytävän URL-osoite.

**Tavoite-C**:

```
MSClient *client = [MSClient clientWithApplicationURLString:@"AppUrl"];
```

**Swift**:

```
let client = MSClient(applicationURLString: "AppUrl")
```


##<a name="table-reference"></a>Toimintaohje: taulukkoviittauksen luominen

Access- tai Päivitä tiedot, voit luoda viittauksen Taustajärjestelmä taulukon. Korvaa `TodoItem` taulukon nimi

**Tavoite-C**:

```
MSTable *table = [client tableWithName:@"TodoItem"];
```

**Swift**:

```
let table = client.tableWithName("TodoItem")
```


##<a name="querying"></a>Toimintaohje: tietojen kysely

Tietokannan kyselyn luomiseen kysely `MSTable` objekti. Seuraava kysely saa kaikkien kohteiden `TodoItem` ja kirjaa kunkin kohteen tekstiosassa.

**Tavoite-C**:

```
[table readWithCompletion:^(MSQueryResult *result, NSError *error) {
        if(error) { // error is nil if no error occured
                NSLog(@"ERROR %@", error);
        } else {
                for(NSDictionary *item in result.items) { // items is NSArray of records that match query
                        NSLog(@"Todo Item: %@", [item objectForKey:@"text"]);
                }
        }
}];
```

**Swift**:

```
table.readWithCompletion { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let items = result?.items {
        for item in items {
            print("Todo Item: ", item["text"])
        }
    }
}
```

##<a name="filtering"></a>Toimintaohje: Suodatin palautti tietoja

Voit suodattaa tulokset on monia vaihtoehtoa.

Voit suodattaa avulla lause, `NSPredicate` ja `readWithPredicate`. Seuraavat suodattimet palautti tietoja, voit etsiä vain keskeneräiset Todo kohteita.

**Tavoite-C**:

```
// Create a predicate that finds items where complete is false
NSPredicate * predicate = [NSPredicate predicateWithFormat:@"complete == NO"];
// Query the TodoItem table
[table readWithPredicate:predicate completion:^(MSQueryResult *result, NSError *error) {
        if(error) {
                NSLog(@"ERROR %@", error);
        } else {
                for(NSDictionary *item in result.items) {
                        NSLog(@"Todo Item: %@", [item objectForKey:@"text"]);
                }
        }
}];
```

**Swift**:

```
// Create a predicate that finds items where complete is false
let predicate =  NSPredicate(format: "complete == NO")
// Query the TodoItem table
table.readWithPredicate(predicate) { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let items = result?.items {
        for item in items {
            print("Todo Item: ", item["text"])
        }
    }
}
```

##<a name="query-object"></a>Toimintaohje: Käytä MSQuery

Voit suorittaa monimutkaista kyselyä (mukaan lukien lajittelu ja sivutus), luoda `MSQuery` objekti suoraan tai lause, avulla:

**Tavoite-C**:

```
MSQuery *query = [table query];
MSQuery *query = [table queryWithPredicate: [NSPredicate predicateWithFormat:@"complete == NO"]];
```

**Swift**:

```
let query = table.query()
let query = table.queryWithPredicate(NSPredicate(format: "complete == NO"))
```

`MSQuery`Voit hallita useita kyselyn toiminnan.

* Tulokset järjestyksen määrittäminen
* Raja palauttaa kentät
* Kuinka monta tietuetta, palaa rajoittaminen
* Määritä kokonaismäärä vastaus
* Määritä mukautettu kyselyparametri pyyntö
* Muut funktiot

Suorita `MSQuery` kyselyn soittamalla `readWithCompletion` objektin.

## <a name="sorting"></a>Toimintaohje: MSQuery tietojen lajitteleminen

Tulosten lajittelu katsotaan esimerkki. Lajiteltavan kentän "teksti" nousevassa järjestyksessä ja valitse Valmis' laskeva käynnistää `MSQuery` seuraavalla tavalla:

**Tavoite-C**:

```
[query orderByAscending:@"text"];
[query orderByDescending:@"complete"];
[query readWithCompletion:^(MSQueryResult *result, NSError *error) {
        if(error) {
                NSLog(@"ERROR %@", error);
        } else {
                for(NSDictionary *item in result.items) {
                        NSLog(@"Todo Item: %@", [item objectForKey:@"text"]);
                }
        }
}];
```

**Swift**:

```
query.orderByAscending("text")
query.orderByDescending("complete")
query.readWithCompletion { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let items = result?.items {
        for item in items {
            print("Todo Item: ", item["text"])
        }
    }
}
```


## <a name="selecting"></a><a name="parameters"></a>Toimintaohje: rajoittaa kentät ja laajenna kyselyparametri MSQuery kanssa

Voit rajoittaa kentät kyselyn palautetaan, Määritä kenttien nimet **selectFields** -ominaisuudessa. Tässä esimerkissä palauttaa vain teksti- ja valmis kentät:

**Tavoite-C**:

```
query.selectFields = @[@"text", @"complete"];
```

**Swift**:

```
query.selectFields = ["text", "complete"]
```

Voit sisällyttää muita kyselyparametri palvelimen pyynnön (esimerkiksi kun mukautetun palvelinpuolen komentosarjan käyttää niitä), täytä `query.parameters` seuraavalla tavalla:

**Tavoite-C**:

```
query.parameters = @{
    @"myKey1" : @"value1",
    @"myKey2" : @"value2",
};
```

**Swift**:

```
query.parameters = ["myKey1": "value1", "myKey2": "value2"]
```

## <a name="paging"></a>Toimintaohje: sivun koon määrittäminen

Sivun koon ohjaa Azure Mobile-sovellusten kanssa tietueet, jotka näkyvät tiedot kerrallaan Taustajärjestelmä taulukoiden määrän. Kutsu `pull` tiedot sitten erän tiedot, tämä sivukoko perusteella, kunnes on erotettu tietueita ei ole.

Se on mahdollista määrittää sivun koon käyttämällä **MSPullSettings** alla kuvatulla tavalla. Sivun oletuskoko on 50 ja alla olevasta esimerkistä muuttuu arvoksi 3.

Voi määrittää sivukoon nopeuttamiseksi. Jos sinulla on runsaasti pieni tietueita, suuri sivukoko vähentää palvelimen edestakaista. 

Tällä asetuksella vain asiakaspuolen sivukoko. Jos asiakas pyytää sivukoon suurempi kuin mobiilisovellukset Taustajärjestelmä tukee, sivun kokoa enimmillään taustaan on määritetty tukemaan suurin-ruutuun. 

Tämä asetus on myös tietueita ei _tavukoon_ _numero_ .

Jos lisäät asiakkaan sivun koon, [Voit myös suurentaa sivun koon palvelimessa](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#_how-to-adjust-the-table-paging-size).

**Tavoite-C**:

```
  MSPullSettings *pullSettings = [[MSPullSettings alloc] initWithPageSize:3];
  [table  pullWithQuery:query queryId:@nil settings:pullSettings
                        completion:^(NSError * _Nullable error) {
                               if(error) {
                    NSLog(@"ERROR %@", error);
                } 
                           }];
```


**Swift**:

```
let pullSettings = MSPullSettings(pageSize: 3)
table.pullWithQuery(query, queryId:nil, settings: pullSettings) { (error) in
    if let err = error {
        print("ERROR ", err)
    } 
}
```

##<a name="inserting"></a>Toimintaohje: tietojen lisääminen

Lisää taulukkoon uusi rivi, luo `NSDictionary` ja käynnistää `table insert`. [Dynaaminen rakenne] on käytössä, jos Azure App palvelun mobile Taustajärjestelmä luo automaattisesti uuden sarakkeita `NSDictionary`.

Jos `id` on ei, jos taustaan luo automaattisesti uuden yksilöllisen tunnuksen. Anna oman `id` sähköpostin osoitteet, käyttäjänimet, tai oman mukautetun arvot kuin tunnus. Oman tunnuksen antamisen voi helpottaa liitokset ja liiketoimintaprosessit tietokannan logiikan.

`result` Sisältää uuden kohteen, joka on lisätty. Sen mukaan, palvelin-logiikkaa se voi olla lisätyt tai muutetut tiedot, mikä on välitetty palvelimeen verrattuna.

**Tavoite-C**:

```
NSDictionary *newItem = @{@"id": @"custom-id", @"text": @"my new item", @"complete" : @NO};
[table insert:newItem completion:^(NSDictionary *result, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item: %@", [result objectForKey:@"text"]);
    }
}];
```

**Swift**:

```
let newItem = ["id": "custom-id", "text": "my new item", "complete": false]
table.insert(newItem) { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let item = result {
        print("Todo Item: ", item["text"])
    }
}
```

##<a name="modifying"></a>Toimintaohje: tietojen muokkaaminen

Päivittää olemassa olevat rivi, Muokkaa kohdetta ja puhelun `update`:

**Tavoite-C**:

```
NSMutableDictionary *newItem = [oldItem mutableCopy]; // oldItem is NSDictionary
[newItem setValue:@"Updated text" forKey:@"text"];
[table update:newItem completion:^(NSDictionary *result, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item: %@", [result objectForKey:@"text"]);
    }
}];
```

**Swift**:

```
if let newItem = oldItem.mutableCopy() as? NSMutableDictionary {
    newItem["text"] = "Updated text"
    table2.update(newItem as [NSObject: AnyObject], completion: { (result, error) -> Void in
        if let err = error {
            print("ERROR ", err)
        } else if let item = result {
            print("Todo Item: ", item["text"])
        }
    })
}
```

Voit myös antaa rivin tunnusta ja päivittää kenttää:

**Tavoite-C**:

```
[table update:@{@"id":@"custom-id", @"text":"my EDITED item"} completion:^(NSDictionary *result, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item: %@", [result objectForKey:@"text"]);
    }
}];
```

**Swift**:

```
table.update(["id": "custom-id", "text": "my EDITED item"]) { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let item = result {
        print("Todo Item: ", item["text"])
    }
}
```

Vähintään `id` määrite on määritettävä, kun teet päivitykset.

##<a name="deleting"></a>Toimintaohje: tietojen poistaminen

Jos haluat poistaa kohteen, ongelma `delete` -kohteen kanssa:

**Tavoite-C**:

```
[table delete:item completion:^(id itemId, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item ID: %@", itemId);
    }
}];
```

**Swift**:

```
table.delete(newItem as [NSObject: AnyObject]) { (itemId, error) in
    if let err = error {
        print("ERROR ", err)
    } else {
        print("Todo Item ID: ", itemId)
    }
}
```

Voit myös poistaa antamalla rivin tunnus seuraavasti:

**Tavoite-C**:

```
[table deleteWithId:@"37BBF396-11F0-4B39-85C8-B319C729AF6D" completion:^(id itemId, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item ID: %@", itemId);
    }
}];
```

**Swift**:

```
table.deleteWithId("37BBF396-11F0-4B39-85C8-B319C729AF6D") { (itemId, error) in
    if let err = error {
        print("ERROR ", err)
    } else {
        print("Todo Item ID: ", itemId)
    }
}
```

Vähintään `id` määrite on määritettävä, kun tekeminen poistaa.

##<a name="customapi"></a>Toimintaohje: mukautetun Ohjelmointirajapinnan kutsu

Mukautetun API voit näyttää Taustajärjestelmä toiminnot. Se ei ole yhdistäminen taulukossa-toimintoa. Paitsi voit saada messaging määrittää tarkemmin, voit myös luku/määrittäminen ylä- ja vastaus leipätekstin muotoilun muuttaminen. Opettele luomaan mukautettuja API taustassa, voit lukea [Mukautetun API](app-service-mobile-node-backend-how-to-use-server-sdk.md#work-easy-apis)

Soita mukautetun API, soita `MSClient.invokeAPI`. Pyynnön ja vastauksen sisällön käsitellään JSON. Voit käyttää muita mediatyyppejä [käyttää muita on liikaa `invokeAPI` ] [ 5].  Jotta `GET` sijaan pyyntö `POST` pyytää, Määritä parametrin `HTTPMethod` , `"GET"` ja parametri `body` , `nil` (koska GET-pyyntöjä ei ole viestien tekstit.) Jos mukautetun API tukee muiden HTTP-verbien, muuttaa `HTTPMethod` asianmukaisesti.

**Tavoite-C**:

```
[self.client invokeAPI:@"sendEmail"
                  body:@{ @"contents": @"Hello world!" }
            HTTPMethod:@"POST"
            parameters:@{ @"to": @"bill@contoso.com", @"subject" : @"Hi!" }
               headers:nil
            completion: ^(NSData *result, NSHTTPURLResponse *response, NSError *error) {
                if(error) {
                    NSLog(@"ERROR %@", error);
                } else {
                    // Do something with result
                }
            }];
```

**Swift**:

```
client.invokeAPI("sendEmail",
            body: [ "contents": "Hello World" ],
            HTTPMethod: "POST",
            parameters: [ "to": "bill@contoso.com", "subject" : "Hi!" ],
            headers: nil)
            {
                (result, response, error) -> Void in
                if let err = error {
                    print("ERROR ", err)
                } else if let res = result {
                          // Do something with result
                }
        }
```

##<a name="templates"></a>Toimintaohje: Rekisteröi push-mallit, Office kaikissa ympäristöissä ilmoitukset lähetetään

Voit rekisteröidä mallit-välittää malleja **client.push registerDeviceToken** -menetelmällä asiakas-sovelluksessa.

**Tavoite-C**:

```
[client.push registerDeviceToken:deviceToken template:iOSTemplate completion:^(NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    }
}];
```

**Swift**:

```
    client.push?.registerDeviceToken(NSData(), template: iOSTemplate, completion: { (error) in
        if let err = error {
            print("ERROR ", err)
        }
    })
```

Mallisi NSDictionary tyyppi, ja ne voivat sisältää useita malleja seuraavassa muodossa:

**Tavoite-C**:

```
NSDictionary *iOSTemplate = @{ @"templateName": @{ @"body": @{ @"aps": @{ @"alert": @"$(message)" } } } };
```

**Swift**:

```
let iOSTemplate = ["templateName": ["body": ["aps": ["alert": "$(message)"]]]]
```

Pyyntö suojaus poistetaan kaikki tunnisteet.  Asennuksia tai asennusten mallit tunnisteet lisäämisestä on ohjeaiheessa [.NET palvelimeen Azure-mobiilisovellukset SDK käsitteleminen][4].  Lähetä ilmoitukset rekisteröityjen mallien avulla, [Ilmoitus keskittimet ohjelmointirajapinnan]käyttäminen[3].

##<a name="errors"></a>Toimintaohje: käsitellä virheitä

Kun soitat mobile Azure App Service-taustatietokannan, valmistuminen-kenttä sisältää `NSError` parametria. Kun näyttöön tulee virhesanoma, tämä parametri on ei ole nolla. Omassa koodissasi olisi Tarkista tämä parametri ja käsitellä virheen tarvittaessa, jokaisessa edellisen koodikatkelmat.

Tiedoston [`<WindowsAzureMobileServices/MSError.h>`] [6] määrittää vakiot `MSErrorResponseKey`, `MSErrorRequestKey`, ja `MSErrorServerItemKey`. Saat enemmän virheeseen liittyvät tiedot:

**Tavoite-C**:

```
NSDictionary *serverItem = [error.userInfo objectForKey:MSErrorServerItemKey];
```

**Swift**:

```
let serverItem = error.userInfo[MSErrorServerItemKey]
```

Lisäksi tiedoston määrittää kunkin virhekoodit vakioita:

**Tavoite-C**:

```
if (error.code == MSErrorPreconditionFailed) {
```

**Swift**:

```
if (error.code == MSErrorPreconditionFailed) {
```

## <a name="adal"></a>Toimintaohje: todentaa käyttäjät Active Directory käyttöoikeuksien-kirjaston kanssa

Voit kirjautua Azure Active Directoryn avulla sovelluksesi käyttäjät Active Directory käyttöoikeuksien kirjaston (ADAL). Asiakkaan työnkulku todennus tunnistetietojen toimittaja SDK käyttämällä on suositeltavampaa `loginWithProvider:completion:` menetelmää.  Asiakkaan työnkulku todennus on lisää alkuperäisen UX ilmeen ja mahdollistaa muita mukauttaminen.

1. Määritä mobiilisovelluksen-taustatietokannan AAD kirjautumista varten seuraamalla [määrittäminen sovelluksen palvelun Active Directory-kirjautumisen] [ 7] opetusohjelma. Varmista, että Viimeistele valinnainen vaihe native client-sovelluksen. IOS, on suositeltavaa, että URI on lomakkeen uudelleenohjaus `<app-scheme>://<bundle-id>`. Lisätietoja on artikkelissa [ADAL iOS pikaopas][8].

2. Asenna ADAL Cocoapods avulla. Muokkaa yhteyttä Podfile sisällytettävien **YOUR PROJEKTIN** tilalle Xcode projektin nimen määritelmän:

        source 'https://github.com/CocoaPods/Specs.git'
        link_with ['YOUR-PROJECT']
        xcodeproj 'YOUR-PROJECT'

   ja Pod:

        pod 'ADALiOS'

3. Käytä päätteen, suorittamalla `pod install` hakemiston sisältävä projektin ja avaa sitten luotu Xcode työtilan (ei projekti).

4. Lisää seuraava koodi sovelluksen, käytössäsi on kielen mukaan. Tee kunkin, nämä korvauksia:

    * Korvaa **Lisää MYÖNTÄJÄ-tähän** nimi, jossa sovellus on valmisteltu vuokraajan. Muoto on oltava https://login.windows.net/contoso.onmicrosoft.com. Tämän arvon voi kopioida [perinteinen Azure portaalissa] Azure Active Directory-toimialueen-välilehdestä.
    * **Lisää-resurssi-tunnus-tähän** tilalle mobiilisovelluksen Taustajärjestelmä Asiakastunnus. Voit hankkia Asiakastunnus-portaalissa **Azure Active Directory-asetukset** -kohdassa **Lisäasetukset** -välilehti.
    * **Lisää-asiakas-tunnus-tähän** tilalle alkuperäisen asiakassovellus kopioitu Asiakastunnus.
    * **Lisää-REDIRECT-URI-tähän** tilalle sivuston _/.auth/login/done_ päätepisteen HTTPS-mallin avulla. Tämän arvon pitäisi olla samanlainen _https://contoso.azurewebsites.net/.auth/login/done_.

**Tavoite-C**:

    #import <ADALiOS/ADAuthenticationContext.h>
    #import <ADALiOS/ADAuthenticationSettings.h>
    // ...
    - (void) authenticate:(UIViewController*) parent
               completion:(void (^) (MSUser*, NSError*))completionBlock;
    {
        NSString *authority = @"INSERT-AUTHORITY-HERE";
        NSString *resourceId = @"INSERT-RESOURCE-ID-HERE";
        NSString *clientId = @"INSERT-CLIENT-ID-HERE";
        NSURL *redirectUri = [[NSURL alloc]initWithString:@"INSERT-REDIRECT-URI-HERE"];
        ADAuthenticationError *error;
        ADAuthenticationContext *authContext = [ADAuthenticationContext authenticationContextWithAuthority:authority error:&error];
        authContext.parentController = parent;
        [ADAuthenticationSettings sharedInstance].enableFullScreen = YES;
        [authContext acquireTokenWithResource:resourceId
                                     clientId:clientId
                                  redirectUri:redirectUri
                              completionBlock:^(ADAuthenticationResult *result) {
                                  if (result.status != AD_SUCCEEDED)
                                  {
                                      completionBlock(nil, result.error);;
                                  }
                                  else
                                  {
                                      NSDictionary *payload = @{
                                                                @"access_token" : result.tokenCacheStoreItem.accessToken
                                                                };
                                      [client loginWithProvider:@"aad" token:payload completion:completionBlock];
                                  }
                              }];
    }


**Swift**:

    // add the following imports to your bridging header:
    //      #import <ADALiOS/ADAuthenticationContext.h>
    //      #import <ADALiOS/ADAuthenticationSettings.h>

    func authenticate(parent: UIViewController, completion: (MSUser?, NSError?) -> Void) {
        let authority = "INSERT-AUTHORITY-HERE"
        let resourceId = "INSERT-RESOURCE-ID-HERE"
        let clientId = "INSERT-CLIENT-ID-HERE"
        let redirectUri = NSURL(string: "INSERT-REDIRECT-URI-HERE")
        var error: AutoreleasingUnsafeMutablePointer<ADAuthenticationError?> = nil
        let authContext = ADAuthenticationContext(authority: authority, error: error)
        authContext.parentController = parent
        ADAuthenticationSettings.sharedInstance().enableFullScreen = true
        authContext.acquireTokenWithResource(resourceId, clientId: clientId, redirectUri: redirectUri) { (result) in
                if result.status != AD_SUCCEEDED {
                    completion(nil, result.error)
                }
                else {
                    let payload: [String: String] = ["access_token": result.tokenCacheStoreItem.accessToken]
                    client.loginWithProvider("aad", token: payload, completion: completion)
                }
            }
    }

## <a name="facebook-sdk"></a>Toimintaohje: todentaa käyttäjät, joilla on iOS Facebook-SDK

Facebook-SDK iOS avulla voit kirjautua käyttämällä Facebook sovelluksen käyttäjille.  Asiakkaan työnkulku-todennusta on suositeltavampaa `loginWithProvider:completion:` menetelmää.  Asiakkaan työnkulku-todennus on lisää alkuperäisen UX ilmeen ja mahdollistaa muita mukauttaminen.

1. Määritä mobiilisovelluksen-taustatietokannan, Facebook-kirjautuminen seuraamalla [määrittäminen sovelluksen palvelun Facebook-kirjautuminen] [ 9] opetusohjelma.

2. Facebook-SDK iOS asentaminen [IOS - aloittaminen Facebook SDK] [ 10] ohjeissa. Sijaan sovelluksen, voit lisätä aiemmin rekisteröinti iOS-ympäristössä. 

3. Facebook's dokumentaatio sisältää tavoite-C-lisäkoodin App edustajalle. Jos käytössäsi on **Swift**, voit seuraavia käännökset AppDelegate.swift:
  
        // Add the following import to your bridging header:
        //      #import <FBSDKCoreKit/FBSDKCoreKit.h>
        
        func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject : AnyObject]?) -> Bool {
            FBSDKApplicationDelegate.sharedInstance().application(application, didFinishLaunchingWithOptions: launchOptions)
            // Add any custom logic here.
            return true
        }

        func application(application: UIApplication, openURL url: NSURL, sourceApplication: String?, annotation: AnyObject?) -> Bool {
            let handled = FBSDKApplicationDelegate.sharedInstance().application(application, openURL: url, sourceApplication: sourceApplication, annotation: annotation)
            // Add any custom logic here.
            return handled
        }

4. Lisäksi lisääminen `FBSDKCoreKit.framework` projektiin, voit myös lisätä viittaa `FBSDKLoginKit.framework` samalla tavalla. 

4. Lisää seuraava koodi sovelluksen, käytössäsi on kielen mukaan. 

**Tavoite-C**:

    #import <FBSDKLoginKit/FBSDKLoginKit.h>
    #import <FBSDKCoreKit/FBSDKAccessToken.h>
    // ...
    - (void) authenticate:(UIViewController*) parent
               completion:(void (^) (MSUser*, NSError*)) completionBlock;
    {       
        FBSDKLoginManager *loginManager = [[FBSDKLoginManager alloc] init];
        [loginManager
         logInWithReadPermissions: @[@"public_profile"]
         fromViewController:parent
         handler:^(FBSDKLoginManagerLoginResult *result, NSError *error) {
             if (error) {
                 completionBlock(nil, error);
             } else if (result.isCancelled) {
                 completionBlock(nil, error);
             } else {
                 NSDictionary *payload = @{
                                           @"access_token":result.token.tokenString
                                           };
                 [client loginWithProvider:@"facebook" token:payload completion:completionBlock];
             }
         }];
    }

**Swift**:

    // Add the following imports to your bridging header:
    //      #import <FBSDKLoginKit/FBSDKLoginKit.h>
    //      #import <FBSDKCoreKit/FBSDKAccessToken.h>
    
    func authenticate(parent: UIViewController, completion: (MSUser?, NSError?) -> Void) {
        let loginManager = FBSDKLoginManager()
        loginManager.logInWithReadPermissions(["public_profile"], fromViewController: parent) { (result, error) in
            if (error != nil) {
                completion(nil, error)
            }
            else if result.isCancelled {
                completion(nil, error)
            }
            else {
                let payload: [String: String] = ["access_token": result.token.tokenString]
                client.loginWithProvider("facebook", token: payload, completion: completion)
            }
        }
    }

## <a name="twitter-fabric"></a>Toimintaohje: todentaa käyttäjät, joilla on Twitter-kangasta iOS

Voit kirjautua käyttämällä Twitter sovelluksesi käyttäjät iOS kangasta. Asiakkaan työnkulku todennus on suositeltavampaa `loginWithProvider:completion:` menetelmää, koska se sisältää Lisää alkuperäisen UX ilmeen ja sen avulla voidaan muita mukauttaminen.

1. Määritä mobiilisovelluksen-taustatietokannan, Twitter-kirjautuminen [määrittäminen sovelluksen palvelun Twitter-kirjautumiset](app-service-mobile-how-to-configure-twitter-authentication.md) -opetusohjelma.

2. Lisää kangasta projektin seuraavien [iOS - aloittaminen kangasta] asiakirjoista ja määrittämällä TwitterKit.

    > [AZURE.NOTE] Oletusarvon mukaan kangasta Luo Twitter-sovellusta. Voit välttää luominen sovellus rekisteröidään kuluttaja-näppäintä ja kuluttaja salaisuus avulla seuraava koodikatkelmat aiemmin luomasi.  Vaihtoehtoisesti voit korvata kuluttaja-näppäintä ja kuluttaja salaisen arvot, jotka sisältävät sovelluksen palveluun näet [Kangasta Raporttinäkymät-ikkunan]arvoilla. Jos valitset tämän vaihtoehdon, muista määrittää takaisinsoiton URL-Osoitteen paikkamerkin arvoa, kuten `https://<yoursitename>.azurewebsites.net/.auth/login/twitter/callback`.

    Jos haluat käyttää aiemmin luomasi tietoja, Lisää seuraava koodi sovelluksen edustajallasi:
    
    **Tavoite-C**:

        #import <Fabric/Fabric.h>
        #import <TwitterKit/TwitterKit.h>
        // ...
        - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
        {
            [[Twitter sharedInstance] startWithConsumerKey:@"your_key" consumerSecret:@"your_secret"];
            [Fabric with:@[[Twitter class]]];
            // Add any custom logic here.
            return YES;
        }
        
    **Swift**:
    
        import Fabric
        import TwitterKit
        // ...
        func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject : AnyObject]?) -> Bool {
            Twitter.sharedInstance().startWithConsumerKey("your_key", consumerSecret: "your_secret")
            Fabric.with([Twitter.self])
            // Add any custom logic here.
            return true
        }
    
3. Lisää seuraava koodi sovelluksen, käytössäsi on kielen mukaan. 

**Tavoite-C**:

    #import <TwitterKit/TwitterKit.h>
    // ...
    - (void)authenticate:(UIViewController*)parent completion:(void (^) (MSUser*, NSError*))completionBlock
    {
        [[Twitter sharedInstance] logInWithCompletion:^(TWTRSession *session, NSError *error) {
            if (session) {
                NSDictionary *payload = @{
                                            @"access_token":session.authToken,
                                            @"access_token_secret":session.authTokenSecret
                                        };
                [client loginWithProvider:@"twitter" token:payload completion:completionBlock];
            } else {
                completionBlock(nil, error);
            }
        }];
    }

**Swift**:

    import TwitterKit
    // ...
    func authenticate(parent: UIViewController, completion: (MSUser?, NSError?) -> Void) {
        let client = self.table!.client
        Twitter.sharedInstance().logInWithCompletion { session, error in
            if (session != nil) {
                let payload: [String: String] = ["access_token": session!.authToken, "access_token_secret": session!.authTokenSecret]
                client.loginWithProvider("twitter", token: payload, completion: completion)
            } else {
                completion(nil, error)
            }
        }
    }

## <a name="google-sdk"></a>Toimintaohje: todentaa käyttäjät, joilla on iOS SDK Google-kirjautuminen

Voit käyttää iOS SDK Google-kirjautuminen käyttäjien kirjautumalla sovelluksen Google-tilin avulla.  Google ilmoitettiin viimeksi niiden OAuth-suojauskäytäntöjä muutokset.  Käytännön muutokset edellyttävät Google-SDK käyttö tulevaisuudessa.

1. Määritä mobiilisovelluksen-taustatietokannan, Google-kirjautuminen [määrittäminen sovelluksen palvelun sisäänkirjautuminen Google](app-service-mobile-how-to-configure-google-authentication.md) -opetusohjelma.

2. Asentaminen iOS Google-SDK [Google Sign-In for iOS - Käynnistä integrointi](https://developers.google.com/identity/sign-in/ios/start-integrating) -ohjeista. Voit ohittaa "Todennetaan kanssa Taustajärjestelmä palvelin"-kohta.

3. Lisää seuraava teksti edustajan `signIn:didSignInForUser:withError:` käyttämäsi kielen mukaan-menetelmä.

**Tavoite-C**:

        NSDictionary *payload = @{
                                  @"id_token":user.authentication.idToken,
                                  @"authorization_code":user.serverAuthCode
                                  };
        
        [client loginWithProvider:@"google" token:payload completion:^(MSUser *user, NSError *error) {
            // ...
        }];

**Swift**:

        let payload: [String: String] = ["id_token": user.authentication.idToken, "authorization_code": user.serverAuthCode]
        client.loginWithProvider("google", token: payload) { (user, error) in
            // ...
        }

4. Varmista, että voit lisätä myös seuraavat `application:didFinishLaunchingWithOptions:` -sovelluksen edustajan "SERVER_CLIENT_ID" tilalle on sama tunnus, jota käytit vaiheessa 1 sovelluksen-palvelun määrittäminen.

**Tavoite-C**:

        [GIDSignIn sharedInstance].serverClientID = @"SERVER_CLIENT_ID";
 
 **Swift**:
 
        GIDSignIn.sharedInstance().serverClientID = "SERVER_CLIENT_ID"

 
 5. Lisää seuraava koodi sovelluksen UIViewController, joka sisältää `GIDSignInUIDelegate` protokolla, käytössäsi on kielen mukaan.  Olet kirjautunut ulos ennen ole kirjautunut sisään uudelleen ja vaikka et tarvitse Anna tunnistetiedot uudelleen, näet suostumusta-valintaikkuna.  Kutsu tätä menetelmää vain, kun istunnon tunnus on vanhentunut.
 
 **Tavoite-C**:

        #import <Google/SignIn.h>
        // ...
        - (void)authenticate
        {
                [GIDSignIn sharedInstance].uiDelegate = self;
                [[GIDSignIn sharedInstance] signOut];
                [[GIDSignIn sharedInstance] signIn];
        }
 
 **Swift**:
    
        // ...
        func authenticate() {
            GIDSignIn.sharedInstance().uiDelegate = self
            GIDSignIn.sharedInstance().signOut()
            GIDSignIn.sharedInstance().signIn()
        }
        
<!-- Anchors. -->

[What is Mobile Services]: #what-is
[Concepts]: #concepts
[Setup and Prerequisites]: #Setup
[How to: Create the Mobile Services client]: #create-client
[How to: Create a table reference]: #table-reference
[How to: Query data from a mobile service]: #querying
[Filter returned data]: #filtering
[Sort returned data]: #sorting
[Return data in pages]: #paging
[Select specific columns]: #selecting
[How to: Bind data to the user interface]: #binding
[How to: Insert data into a mobile service]: #inserting
[How to: Modify data in a mobile service]: #modifying
[How to: Authenticate users]: #authentication
[Cache authentication tokens]: #caching-tokens
[How to: Upload images and large files]: #blobs
[How to: Handle errors]: #errors
[How to: Design unit tests]: #unit-testing
[How to: Customize the client]: #customizing
[Customize request headers]: #custom-headers
[Customize data type serialization]: #custom-serialization
[Next Steps]: #next-steps
[How to: Use MSQuery]: #query-object

<!-- Images. -->

<!-- URLs. -->
[Azure Mobile sovellusten Pika-aloitus]: app-service-mobile-ios-get-started.md

[Add Mobile Services to Existing App]: /develop/mobile/tutorials/get-started-data
[Get started with Mobile Services]: /develop/mobile/tutorials/get-started-ios
[Validate and modify data in Mobile Services by using server scripts]: /develop/mobile/tutorials/validate-modify-and-augment-data-ios
[Mobile Services SDK]: https://go.microsoft.com/fwLink/p/?LinkID=266533
[Authentication]: /develop/mobile/tutorials/get-started-with-users-ios
[iOS SDK]: https://developer.apple.com/xcode

[Handling Expired Tokens]: http://go.microsoft.com/fwlink/p/?LinkId=301955
[Live Connect SDK]: http://go.microsoft.com/fwlink/p/?LinkId=301960
[Permissions]: http://msdn.microsoft.com/library/windowsazure/jj193161.aspx
[Service-side Authorization]: mobile-services-javascript-backend-service-side-authorization.md
[Use scripts to authorize users]: /develop/mobile/tutorials/authorize-users-in-scripts-ios
[Dynaaminen rakenne]: http://go.microsoft.com/fwlink/p/?LinkId=296271
[How to: access custom parameters]: /develop/mobile/how-to-guides/work-with-server-scripts#access-headers
[Create a table]: http://msdn.microsoft.com/library/windowsazure/jj193162.aspx
[NSDictionary object]: http://go.microsoft.com/fwlink/p/?LinkId=301965
[ASCII control codes C0 and C1]: http://en.wikipedia.org/wiki/Data_link_escape_character#C1_set
[CLI to manage Mobile Services tables]: ../virtual-machines-command-line-tools.md#Mobile_Tables
[Conflict-Handler]: mobile-services-ios-handling-conflicts-offline-data.md#add-conflict-handling

[Kangasta Raporttinäkymät-ikkunan]: https://www.fabric.io/home
[Kangasta iOS - käytön aloittaminen]: https://docs.fabric.io/ios/fabric/getting-started.html
[1]: https://github.com/Azure/azure-mobile-apps-ios-client/blob/master/README.md#ios-client-sdk
[2]: http://azure.github.io/azure-mobile-apps-ios-client/
[3]: https://msdn.microsoft.com/library/azure/dn495101.aspx
[4]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#tags
[5]: http://azure.github.io/azure-mobile-services/iOS/v3/Classes/MSClient.html#//api/name/invokeAPI:data:HTTPMethod:parameters:headers:completion:
[6]: https://github.com/Azure/azure-mobile-services/blob/master/sdk/iOS/src/MSError.h
[7]: app-service-mobile-how-to-configure-active-directory-authentication.md
[8]: ../active-directory/active-directory-devquickstarts-ios.md#em1-determine-what-your-redirect-uri-will-be-for-iosem
[9]: app-service-mobile-how-to-configure-facebook-authentication.md
[10]: https://developers.facebook.com/docs/ios/getting-started
