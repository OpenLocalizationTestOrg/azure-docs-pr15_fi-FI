<properties
    pageTitle="Ota offline-synkronoinnin, kun Azure Mobile-sovellus (iOS)"
    description="Opettele käyttämään palvelua mobiilisovellukset välimuistin ja synkronoi offline-tilassa tietojen iOS-sovelluksessa"
    documentationCenter="ios"
    authors="ysxu"
    manager="yochayk"
    editor=""
    services="app-service\mobile"/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-ios"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="yuaxu"/>

# <a name="enable-offline-sync-for-your-ios-mobile-app"></a>IOS-mobiilisovelluksen offline-synkronoinnin ottaminen käyttöön

[AZURE.INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>Yleiskatsaus

Tässä opetusohjelmassa kerrotaan iOS Azure-mobiilisovellukset offline-synkronoinnin ominaisuus. Offline-synkronoinnin sallii loppukäyttäjien kanssa mobiilisovelluksessa&mdash;tarkasteleminen, lisääminen tai muokkaaminen tietojen&mdash;myös silloin, kun yhteyttä ei ole verkossa. Muutokset tallennetaan paikallisessa tietokannassa. Kun laite on online-tilaan, muutokset synkronoidaan remote Taustajärjestelmä.

Jos tämä on ensimmäinen käyttökokemusta Azure Mobile-sovellusten, täyttävän ensin opetusohjelman [luominen iOS-sovelluksessa]. Jos et käytä ladatut pikaopas palvelimen project-tietojen käytön tunniste-paketit on lisääminen projektiin. Katso lisätietoja server tunniste-paketit [.NET palvelimeen Azure-mobiilisovellukset SDK käsitteleminen](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

Saat lisätietoja offline-synkronointi-toiminto on aiheessa [Offline-tilassa tietojen synkronointi Azure Mobile-sovelluksissa].

## <a name="review-sync"></a>Tarkista asiakkaan Synkronoi-koodi

Asiakkaan projekti, jota olet ladannut jo opetusohjelmaan [luominen iOS-sovelluksessa] on tukevat offline-synkronoinnin käyttäminen perustuvat Core tiedot tietokannan koodi. Tässä osassa on yhteenveto ominaisuuksiin jo opetusohjelman koodissa. Katso Käsitteellinen yleiskatsaus ominaisuus [Offline-tilassa tietojen synkronointi Azure Mobile-sovelluksissa].

Azure-mobiilisovellukset, offline-tietojen synkronointi synkronointi-ominaisuuden avulla käyttäjät voivat käsitellä paikalliseen tietokantaan, kun verkko ei ole käytettävissä. Voit käyttää näitä toimintoja sovelluksen, alusta synkronoinnin yhteydessä `MSClient` ja viittaus paikallisesta liikkeestä. Viitata taulukon kautta `MSSyncTable` käyttöliittymän.

1. **QSTodoService.m** (tavoite-C) tai **ToDoTableViewController.swift** (Swift), Huomaa jäsenen tyypin `syncTable` on `MSSyncTable`. Offline-synkronoinnin käyttää Synkronoi tämän taulukon liittymän sijaan `MSTable`. Synkronoi taulukon käytettäessä kaikki toiminnot paikallisen kauppa ja synkronoidaan vain remote Taustajärjestelmä eksplisiittinen push ja salaus puretaan toimintojen kanssa.

    Saat synkronointi taulukon viittaus-menetelmällä `syncTableWithName` - `MSClient`. Voit poistaa offline-synkronoinnin toimintoja `tableWithName` sijaan.

2. Ennen kuin mikä tahansa taulukon toimintoja voi käyttää, paikallista alustaa. Seuraavassa on koodin. 
    
    **Tavoite-C**:
    
    Valitse `QSTodoService.init` menetelmää:
    
    
            MSCoreDataStore *store = [[MSCoreDataStore alloc] initWithManagedObjectContext:context];
            self.client.syncContext = [[MSSyncContext alloc] initWithDelegate:nil dataSource:store callback:nil];
    
    
    **Swift**:
    
    Valitse `ToDoTableViewController.viewDidLoad` menetelmää:
    
    
            let client = MSClient(applicationURLString: "http:// ...") // URI of the Mobile App
            let managedObjectContext = (UIApplication.sharedApplication().delegate as! AppDelegate).managedObjectContext!
            self.store = MSCoreDataStore(managedObjectContext: managedObjectContext)
            client.syncContext = MSSyncContext(delegate: nil, dataSource: self.store, callback: nil)
    

    Tämä luo käyttöliittymässä paikallisen säilön `MSCoreDataStore`, joka on annettu Mobile-sovellusten SDK. Voit sen sijaan Anna eri paikallisen myymälän käyttöönoton `MSSyncContextDataSource` protokolla. 
    
    Lisäksi ensimmäinen parametri on `MSSyncContext` ristiriita käsittelytoiminto määrittämiseen. Koska Microsoft on kulunut `nil`, emme saavat oletusarvon ristiriidan käsittelijä, jossa ristiriidassa epäonnistuu.
    
3. Seuraavaksi luodaan todellinen synkronoinnin suorittaminen ja tietojen noutaminen remote Taustajärjestelmä.

    **Tavoite-C**:
    
    `syncData`Vie ensin uusia muutoksia, valitse soittaa `pullData` tietojen käyttämistä remote Taustajärjestelmä. Taas-menetelmä `pullData` saa uuden kyselyn sopivia tietoja:
    
    
            -(void)syncData:(QSCompletionBlock)completion
            {
                // push all changes in the sync context, then pull new data
                [self.client.syncContext pushWithCompletion:^(NSError *error) {
                    [self logErrorIfNotNil:error];
                    [self pullData:completion];
                }];
            }
    
            -(void)pullData:(QSCompletionBlock)completion
            {
                MSQuery *query = [self.syncTable query];
    
                // Pulls data from the remote server into the local table.
                // We're pulling all items and filtering in the view
                // query ID is used for incremental sync
                [self.syncTable pullWithQuery:query queryId:@"allTodoItems" completion:^(NSError *error) {
                    [self logErrorIfNotNil:error];
    
                    // Let the caller know that we have finished
                    if (completion != nil) {
                        dispatch_async(dispatch_get_main_queue(), completion);
                    }
                }];
            }
        
        
      **Swift**:
        
        
        func onRefresh(sender: UIRefreshControl!) {
            UIApplication.sharedApplication().networkActivityIndicatorVisible = true
            
            self.table!.pullWithQuery(self.table?.query(), queryId: "AllRecords") {
                (error) -> Void in
                
                UIApplication.sharedApplication().networkActivityIndicatorVisible = false
                
                if error != nil {
                    // A real application would handle various errors like network conditions,
                    // server conflicts, etc via the MSSyncContextDelegate
                    print("Error: \(error!.description)")
                    
                    // We will just discard our changes and keep the servers copy for simplicity
                    if let opErrors = error!.userInfo[MSErrorPushResultKey] as? Array<MSTableOperationError> {
                        for opError in opErrors {
                            print("Attempted operation to item \(opError.itemId)")
                            if (opError.operation == .Insert || opError.operation == .Delete) {
                                print("Insert/Delete, failed discarding changes")
                                opError.cancelOperationAndDiscardItemWithCompletion(nil)
                            } else {
                                print("Update failed, reverting to server's copy")
                                opError.cancelOperationAndUpdateItem(opError.serverItem!, completion: nil)
                            }
                        }
                    }
                }
                self.refreshControl?.endRefreshing()
            }
        } 
    
    
    Tavoite-C-versiossa Valitse `syncData`, ensimmäinen kutsu `pushWithCompletion` Synkronoi kontekstin. Tämä menetelmä kuuluu `MSSyncContext` (sen sijaan, että Synkronoi taulukon itse), koska se siirtää muutokset kaikkien taulukoiden välillä. Palvelimeen lähetetään vain tietueet, jotka on muokattu jollakin tavalla paikallisesti (kautta CUD toimintoja). Valitse avustaja `pullData` on nimeltään, mitkä puhelut `MSSyncTable.pullWithQuery` remote tietojen hakemiseen ja tallentaa paikalliseen tietokantaan.
    
    Nopeasti-versiossa ei ole puhelun `pushWithCompletion`. Tämä johtuu siitä push-toiminto ei ole välttämätöntä. Jos Synkronoi kontekstissa taulukko, joka tekee push-toiminto on odottavia muutoksia, voit noutaa aina ongelmat push ensin. Jos sinulla on synkronointi useammasta kuin yhdestä taulukosta, se on kuitenkin parhaiten kutsussa push varmistaa, että kaikki kohdat ovat yhdenmukaisia taulukoiden välillä.
    
    Tavoite-C- ja Swift versioissa, menetelmä `pullWithQuery` avulla voit määrittää kyselyn, joka suodattaa tietueita haluat noutaa. Tässä esimerkissä kysely palauttaa kaikki tietueet kaukosäätimen vain `TodoItem` taulukon.
    
    Toinen parametri `pullWithQuery` kyselyn ID-tunnusta, jota käytetään *vaiheittainen Synkronoi*. Vaiheittainen synkronointi hakee vain ne tietueet, jotka on muokattu viimeksi synkronoinnin jälkeen käyttämällä tietueen `UpdatedAt` aikaleima (kutsutaan `updatedAt` paikallisen säilön.) Kyselytunnus on oltava kuvaava merkkijono, joka on yksilöllinen kunkin sovelluksen looginen kyselylle. Osallistumiseen liittyvät, lisäävän synkronoinnin, voit siirtää `nil` kuin kyselyn tunnus. Huomaa, että tämä voi mahdollisesti tehotonta, koska se hakee kaikki tietueet salaus puretaan kunkin toiminnon.

5. Tavoite-C app Synkronoi on muokata tai lisätä tietoja, käyttäjän suorittaa Päivitä-liikkeen ja käynnistettäessä. Nopeasti sovellus Synkronoi, kun käyttäjä suorittaa päivityksen liikkeen ja käynnistettäessä. 

Koska sovellus Synkronoi aina, kun tiedot on muokannut (tavoite-C) tai aina, kun sovellus käynnistyy (tavoite-C ja Swift), sovelluksen oletetaan, että käyttäjällä on online-tilassa. Toisen osan päivitämme sovelluksen niin, että käyttäjät voivat muokata myös silloin, kun ne ovat offline-tilassa.

## <a name="review-core-data"></a>Tarkista Core tietomalliin

Offline-tilassa Core tietovaraston käytettäessä haluat määrittää tietyn taulukoiden ja kenttien tietomalliin. Esimerkki sovellus sisältää jo tietomallin oikeassa muodossa. Tässä osassa selkeät nämä taulukot ja miten niitä käytetään.

- Avaa **QSDataModel.xcdatamodeld**. On neljä määritetty taulukoiden kolme, SDK-paketissa, joita käytetään ja todo yhden taulukon kohteiden itse:     *MS_TableOperations: seurantaa kohteet, jotka on synkronoitu palvelimen kanssa    * MS_TableOperationErrors: Jos haluat seurata virheitä, jotka käynnistyvät, offline-synkronoinnin aikana     *MS_TableConfig: seurantaa viimeksi päivitetty aika kaikki salaus puretaan toiminnot viimeisin synkronointi-toiminnon    * TodoItem : N todo kohteen tallentaminen. Järjestelmän sarakkeiden **createdAt**, **updatedAt**ja **versio** on valinnainen järjestelmän ominaisuudet.

>[AZURE.NOTE] Azure Mobile Apps SDK varaa sarakkeiden nimet, kanssasi "**``**". Älä käytä tätä etuliite Valitse jokin muu kuin Järjestelmäsarakkeet, muuten sarakkeiden nimet muokataan remote Taustajärjestelmä käytettäessä.

- Kun offline-synkronointi-toiminnon avulla, sinun on määritettävä järjestelmätaulukoiden alla kuvatulla tavalla.

    ### <a name="system-tables"></a>Järjestelmätaulukoiden

    **MS_TableOperations**

    ![][defining-core-data-tableoperations-entity]

  	| Määrite  |    Tyyppi     |
  	|----------- |   ------    |
  	| tunnus         | Kokonaisluku 64  |
  	| itemId     | Merkkijono      |
  	| Ominaisuudet: | Binaaritietoja |
  	| taulukko      | Merkkijono      |
  	| tableKind  | Kokonaisluku 16  |

    <br>**MS_TableOperationErrors**

    ![][defining-core-data-tableoperationerrors-entity]

  	| Määrite  |    Tyyppi     |
  	|----------- |   ------    |
  	| tunnus         | Merkkijono      |
  	| toimintotunnukseksi | Kokonaisluku 64 |
  	| Ominaisuudet: | Binaaritietojen |
  	| tableKind  | Kokonaisluku 16  |

    <br>**MS_TableConfig**

    ![][defining-core-data-tableconfig-entity]

  	| Määrite  |    Tyyppi     |
  	|----------- |   ------    |
  	| tunnus         | Merkkijono      |
  	| avain        | Merkkijono      |
  	| keyType    | Kokonaisluku 64  |
  	| taulukko      | Merkkijono      |
  	| arvo      | Merkkijono      |

    ### <a name="data-table"></a>Arvotaulukko

    **TodoItem**

  	| Määrite    |  Tyyppi   | Huomautus                                                   |
  	|-----------   |  ------ | -------------------------------------------------------|
  	| tunnus           | Merkkijono, joka on merkitty pakolliseksi  | perusavaimen remote säilöön                            |
  	| Viimeistele     | Totuusarvo | TODO kohdekentässä                                        |
  	| teksti         | Merkkijono  | TODO kohdekentässä                                        |
  	| createdAt | Päivämäärä    | (valinnainen) vastaa createdAt järjestelmäominaisuus         |
  	| updatedAt | Päivämäärä    | (valinnainen) vastaa updatedAt järjestelmäominaisuus         |
  	| versio   | Merkkijono  | (valinnainen) käytetään esiintyvien ristiriidat-versioon kartat |


## <a name="setup-sync"></a>Sovellus Synkronoi toiminnan muuttaminen

Tässä osassa muokataan sovelluksen niin, että ei synkronoida sovelluksen käynnistettäessä tai kun lisääminen ja päivitys kohteita, mutta vain, kun Päivitä liike-painike on suoritettu.

**Tavoite-C**:

1. Muuta **QSTodoListViewController.m** **viewDidLoad** tapa poistaa puhelun `[self refresh]` menetelmä lopussa. Nyt tiedot ei ole synkronoitu palvelimen sovelluksen käynnistettäessä kanssa, mutta sen sijaan on paikallista sisältöä.

2. Muokkaa **QSTodoService.m**määritelmän `addItem` niin, että se ei voi synkronoida sen jälkeen, kun kohde on lisätty. Poista `self syncData` estäminen ja korvaa seuraavasti:

            if (completion != nil) {
                dispatch_async(dispatch_get_main_queue(), completion);
            }

3. Muokkaa määritelmän `completeItem` kuin yllä; Poista, estä `self syncData` ja korvata seuraavasti:

            if (completion != nil) {
                dispatch_async(dispatch_get_main_queue(), completion);
            }

**Swift**:

1. Valitse `viewDidLoad` **ToDoTableViewController.swift**kommentti nämä kaksi riviä, valitse Käynnistä sovellus synkronoinnin lopettaminen ulos. Tässä artikkelissa kirjoittaminen milloin Swift Todo sovellus ei päivitä palvelun kun joku lisää tai ei valmistu kohteen vain sovelluksen käynnistettäessä.

        self.refreshControl?.beginRefreshing()
        self.onRefresh(self.refreshControl)


## <a name="test-app"></a>Sovelluksen testaaminen

Tässä osassa voit muodostaa yhteyden voit simuloida offline-skenaarion virheellinen URL-osoite. Kun olet lisännyt tiedot, ne paikallinen Core tietovaraston säilytetään, mutta ei ole synkronoitu mobile Taustajärjestelmä.

1. Muuta **QSTodoService.m** Mobile sovelluksen URL-osoite on virheellinen URL-Osoitteen ja suorita sovellus uudelleen:

    **Tavoite-C** QSTodoService.m:
    
            self.client = [MSClient clientWithApplicationURLString:@"https://sitename.azurewebsites.net.fail"];
    
    **Swift** ToDoTableViewController.swift:

        let client = MSClient(applicationURLString: "https://sitename.azurewebsites.net.fail")

2. Lisää todo kohteita. Lopeta simulator (tai sulje asiakasistuntojen sovelluksen) ja käynnistä se uudelleen. Tarkista muutokset samanlainen.

3. Remote TodoItem taulukon sisällön tarkasteleminen:

    + Node.js taustassa, siirry [Azure portal](https://portal.azure.com/)-ja Mobile-sovelluksesta Taustajärjestelmä saat **Helposti taulukoiden** > **TodoItem** voi tarkastella sisältöä `TodoItem` taulukon.
    + Voit tarkastella taulukon sisältö SQL-työkalua, kuten SQL Server Management Studiossa tai REST-asiakas, kuten Fiddler tai Postman .NET-Taustajärjestelmä.

    Varmista, että uusia kohteita *ei* ole synkronoitu palvelimen:

4. Muuttaa URL-osoite, oikea **QSTodoService.m** ja suorita sovellus uudelleen. Päivitä-ele minäkin kohteiden luetteloa alaspäin. Näet edistymisen askellin.

5. Näytä TodoItem tiedot uudelleen. Uudet ja muuttuneet TodoItems pitäisi nyt näkyä.

## <a name="summary"></a>Yhteenveto

Tukemiseksi offline-synkronointi-toiminto on käytetty `MSSyncTable` käyttöliittymä ja alustaa `MSClient.syncContext` paikallisen kaupan. Tässä tapauksessa paikallista oli perustuvat Core tiedot tietokantaan.

Paikallinen Core tietosäilö käytettäessä sinun on määritettävä useita taulukoita [oikea järjestelmän ominaisuudet](#review-core-data).

Tavallinen CRUD toimintoja Azure-mobiilisovellukset toimivat samalla tavalla kuin sovellus on edelleen yhteydessä, mutta kaikki toiminnot ilmetä paikallista vastaan.

Kun olemme määrittää paikallista synkronoiminen palvelimen kanssa, on käytetty `MSSyncTable.pullWithQuery`menetelmää.


## <a name="additional-resources"></a>Lisäresursseja

* [Offline-tietojen synkronointi Azure Mobile-sovellukset]

* [Cloud kansi: Offline-synkronoinnin Azure Mobile Services] \(Huomautus: Mobile-palvelut on videon, mutta offline-synkronointi toimii samalla tavalla kuin Azure Mobile-sovelluksissa\)

<!-- URLs. -->


[IOS-sovelluksen luominen]: app-service-mobile-ios-get-started.md
[Offline-tietojen synkronointi Azure Mobile-sovellukset]: app-service-mobile-offline-data-sync.md

[defining-core-data-tableoperationerrors-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-tableoperationerrors-entity.png
[defining-core-data-tableoperations-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-tableoperations-entity.png
[defining-core-data-tableconfig-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-tableconfig-entity.png
[defining-core-data-todoitem-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-todoitem-entity.png

[Cloud kansi: Offline-synkronoinnin Azure Mobile Services-palveluissa]: http://channel9.msdn.com/Shows/Cloud+Cover/Episode-155-Offline-Storage-with-Donna-Malayeri
[Azure Friday: Offline-enabled apps in Azure Mobile Services]: http://azure.microsoft.com/en-us/documentation/videos/azure-mobile-services-offline-enabled-apps-with-donna-malayeri/
