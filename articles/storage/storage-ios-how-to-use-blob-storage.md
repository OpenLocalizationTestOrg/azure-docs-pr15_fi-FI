<properties
    pageTitle="Opi käyttämään Azure-Blob-säiliö iOS | Microsoft Azure"
    description="Tallentaa erimuotoisia tietoja pilveen Azure-Blob-säiliö (objektin tallennus) kanssa."
    services="storage"
    documentationCenter="ios"
    authors="micurd"
    manager="jahogg"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="micurd"/>

# <a name="how-to-use-blob-storage-from-ios"></a>Opi käyttämään iOS-Blob-objektien tallennustilaan

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Yleiskatsaus

Tässä artikkelissa kerrotaan, miten voit suorittaa yleisiä tilanteita, joissa käyttämällä Microsoft Azure-Blob-säiliö. Mallit on kirjoitettu tavoite-C ja [Azure tallennustilan asiakkaan kirjaston iOS](https://github.com/Azure/azure-storage-ios). Tilanteita, joissa kattaa Sisällytä **lataaminen**, **luettelon**, **lataaminen**ja **poistaminen** BLOB-objektit. Lisätietoja BLOB-kohdassa [Seuraavat vaiheet](#next-steps) . Voit ladata [otoksen app](https://github.com/Azure/azure-storage-ios/tree/master/BlobSample) näet nopeasti Azure-tallennustilan käyttö iOS-sovelluksessa.

[AZURE.INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="import-the-azure-storage-ios-library-into-your-application"></a>Azuren tallennustilaan iOS-kirjaston tuominen sovelluksen

Voit tuoda Azuren tallennustilaan iOS-kirjaston sovelluksen käyttämällä [Azure-tallennustilan CocoaPod](https://cocoapods.org/pods/AZSClient) tai tuomalla **Framework** -tiedosto.

## <a name="cocoapod"></a>CocoaPod

1. Jos et ole tehnyt niin jo [Asentaa CocoaPods](https://guides.cocoapods.org/using/getting-started.html#toc_3) pääte ikkuna avataan ja suorittamalla seuraavan komennon tietokoneeseen

        sudo gem install cocoapods

2. Seuraavaksi project-hakemiston (kansion sisältävä oman `.xcodeproj` tiedosto), Luo uusi tiedosto nimeltä `Podfile`(ei ole tiedostotunnisteen). Lisää seuraavat `Podfile` ja tallentaminen

        pod 'AZSClient'

3. Siirry kansioon, projektin pääte-ikkunassa ja suorita seuraava komento

        pod install

4. Jos yhteyttä `.xcodeproj` on Avaa Xcode, sulje se. Avaa project-kansio, jossa on äskettäin luodun projektitiedoston `.xcworkspace` tunniste. Tämä on työskentelet, nyt-tiedosto.

## <a name="framework"></a>Framework
Jotta voit käyttää Azuren tallennustilaan iOS-kirjaston ensin tarvitset framework-tiedoston luominen.

1. Lataa ensin tai Kloonaa [azure-tallennustilan-ios repo](https://github.com/azure/azure-storage-ios).

2. Siirry *azure-tallennustilan-ios*-kohtaan -> *Lib* -> *Azure tallennustilan asiakkaan kirjasto*ja Avaa `AZSClient.xcodeproj` -Xcode.

3. Milloin Xcode ylä-left muuttaa aktiivisen värimallin "Azure-tallennustilan asiakas-kirjastosta" "Framework".

4. Luo projekti (⌘ + B). Tämä vaihtoehto luo `AZSClient.framework` tiedosto työpöydälle.

Voit tuoda framework-tiedoston sitten sovelluksen toimimalla seuraavasti:

1. Uuden projektin luominen tai avaa aiemmin luotu projektin Xcode.

2. Valitse vasemmanpuoleisesta siirtymispalkista projektin ja valitse *Yleiset* -välilehden Projekti-editorin yläreunassa.

3. *Linkitetyn kehysten ja kirjastojen* -osa-kohdasta Lisää-painiketta (+).

4. Valitse *Lisää muita*. Siirry ja lisää `AZSClient.framework` juuri luomasi tiedosto.

5. *Linkitetyn kehysten ja kirjastojen* -osassa napsauttamalla Lisää-painiketta (+) uudelleen.

6. Kirjastojen jo antanut luettelossa, Etsi `libxml2.2.dylib` ja sen lisääminen projektiin.

7. Valitse project-editorin yläreunassa *Luo asetukset* -välilehti.

8. *Etsi polut* -kohdassa kaksoisnapsauttamalla *Framework haun polkujen* ja lisää polku oman `AZSClient.framework` tiedosto.

## <a name="import-statement"></a>Tuo-lause
Tarvitset kohtaa, johon haluat käynnistää Azure-tallennustilan API-tiedostoon sisällytettävät import-lause.

    // Include the following import statement to use blob APIs.
    #import <AZSClient/AZSClient.h>

[AZURE.INCLUDE [storage-mobile-authentication-guidance](../../includes/storage-mobile-authentication-guidance.md)]

## <a name="asynchronous-operations"></a>Asynkronisten toimintojen
> [AZURE.NOTE] Kaikki menetelmät, vastaan pyynnön suorittaminen ovat asynkronisten toimintojen. MALLIKOODEJA löydät näistä menetelmistä on valmistumisen tapahtumakäsittelijän. Koodin sisällä valmistuminen tapahtumakäsittelijän suoritetaan, **Kun** kutsu on valmis. Kun valmistuminen tapahtumakäsittelijän suoritetaan **samalla, kun** pyynnön koodi tehdään.

## <a name="create-a-container"></a>Luoda säilön
Säilön on sijaittava jokaisen Blob-objektien Azuren tallennustilaan. Seuraavassa esimerkissä esitetään, miten voit luoda säilön, eli *newcontainer*tallennustilan-tili, jos se ei ole vielä käytössä. Oman säilön nimi valittaessa olla tietoisia nimeämiskäytäntö sääntöjen edellä mainituista.

    -(void)createContainer{
      NSError *accountCreationError;

      // Create a storage account object from a connection string.
      AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

      if(accountCreationError){
         NSLog(@"Error in creating account.");
      }

      // Create a blob service client object.
      AZSCloudBlobClient *blobClient = [account getBlobClient];

      // Create a local container object.
      AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"newcontainer"];

      // Create container in your Storage account if the container doesn't already exist
      [blobContainer createContainerIfNotExistsWithCompletionHandler:^(NSError *error, BOOL exists) {
         if (error){
             NSLog(@"Error in creating container.");
         }
      }];
    }

Voit vahvistaa, että tämä toimii [Microsoft Azure-tallennustilan Explorer](http://storageexplorer.com) katsoo ja tarkistaa, että *newcontainer* on luettelossa säilöjä tallennustilan tilin.

## <a name="set-container-permissions"></a>Käyttöoikeuksien määrittäminen-säilö
Säilön käyttöoikeudet on määritetty **Yksityinen** käyttää oletusarvoisesti. Säilöjen kuitenkin säätää muutamalla eri tavalla säilö access:

- **Yksityinen**: säilö ja Blob-objektien tietoja voidaan lukea vain tilin omistajan.

- **BLOB**: Tämän säilön Blob-tietoja voi lukea kautta anonyymi pyynnön, mutta säilö tiedot eivät ole käytettävissä. Asiakkaiden ei luetteloi BLOB säilöön anonyymi pyynnön kautta.

- **Säilön**: säilö ja Blob-objektien tietoja voi lukea anonyymi pyynnön kautta. Asiakkaiden voi luetteloida BLOB säilöön kautta anonyymi pyynnön, mutta ei voi luetteloida säilöjen tallennustilan tilin.

Seuraavassa esimerkissä esitetään, kuinka luoda säilön **säilö** käyttöoikeudet, minkä ansiosta julkinen, vain luku-access kaikille Internetin käyttäjille:

    -(void)createContainerWithPublicAccess{
        NSError *accountCreationError;

        // Create a storage account object from a connection string.
        AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

        if(accountCreationError){
            NSLog(@"Error in creating account.");
        }

        // Create a blob service client object.
        AZSCloudBlobClient *blobClient = [account getBlobClient];

        // Create a local container object.
        AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

        // Create container in your Storage account if the container doesn't already exist
        [blobContainer createContainerIfNotExistsWithAccessType:AZSContainerPublicAccessTypeContainer requestOptions:nil operationContext:nil completionHandler:^(NSError *error, BOOL exists){
            if (error){
                NSLog(@"Error in creating container.");
            }
        }];
    }

## <a name="upload-a-blob-into-a-container"></a>Lataa blob säilöön
Kuten [Blob-palvelun käsitteitä](#blob-service-concepts) -osassa-Blob-objektien tallennustilaan on kolmenlaisia BLOB: estää BLOB, Liitä BLOB-objektit ja BLOB-sivu. Tällä hetkellä Azuren tallennustilaan iOS-kirjaston tukee vain estäminen BLOB-objektit. Useimmissa tapauksissa estä blob on käytettävä suositellut tyyppi.

Seuraavassa esimerkissä esitetään lataaminen lohko-blob NSString kohteesta. Jos samanniminen blob on jo tämä säilö, tämä blob sisällön korvataan.

    -(void)uploadBlobToContainer{
        NSError *accountCreationError;

        // Create a storage account object from a connection string.
        AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

        if(accountCreationError){
            NSLog(@"Error in creating account.");
        }

        // Create a blob service client object.
        AZSCloudBlobClient *blobClient = [account getBlobClient];

        // Create a local container object.
        AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

        [blobContainer createContainerIfNotExistsWithAccessType:AZSContainerPublicAccessTypeContainer requestOptions:nil operationContext:nil completionHandler:^(NSError *error, BOOL exists)
         {
             if (error){
                 NSLog(@"Error in creating container.");
             }
             else{
                 // Create a local blob object
                 AZSCloudBlockBlob *blockBlob = [blobContainer blockBlobReferenceFromName:@"sampleblob"];

                 // Upload blob to Storage
                 [blockBlob uploadFromText:@"This text will be uploaded to Blob Storage." completionHandler:^(NSError *error) {
                     if (error){
                         NSLog(@"Error in creating blob.");
                     }
                 }];
             }
         }];
    }

Voit vahvistaa, että tämä toimii [Microsoft Azure-tallennustilan Explorer](http://storageexplorer.com) katsoo ja tarkistetaan sisältävän säilön *containerpublic*blob *sampleblob*. Tässä esimerkissä on käytetty julkisen säilön, jotta voit tarkistaa, tämä onnistumisen siirtymällä BLOB URI:

    https://nameofyourstorageaccount.blob.core.windows.net/containerpublic/sampleblob

Lisäksi lataamisen estäminen-blob-NSString samalla menetelmiä NSData, NSInputStream tai paikallinen tiedosto.

## <a name="list-the-blobs-in-a-container"></a>Luettelon BLOB-säilö
Seuraavassa esimerkissä esitetään, miten luettelon kaikki BLOB-säilö. Kun tätä toimintoa olla tietoisia seuraavista parametreista:     

- **continuationToken** - jatkumista tunnuksen edustaa mistä luettelo-toimintoa kannattaa aloittaa. Jos ei ole tunnuksen ei anneta, sen sisältyviä BLOB alusta. Minkä tahansa määrän BLOB voivat näkyä nollasta ylöspäin joukon suurin. Vaikka tämä menetelmä palauttaa nolla tuloksia, jos `results.continuationToken` ei ole nolla, voi olla useita BLOB-palvelusta, ei ole mainittu.
- **etuliite** - voit määrittää etuliite käyttämään blob-luettelo. Vain BLOB, jotka alkavat Tämä etuliite näkyvät.
- **useFlatBlobListing** - kuten [Naming ja viittaavan säilöjen ja BLOB](#naming-and-referencing-containers-and-blobs) -osassa Blob-palvelu on tasainen tallennustilan-järjestelmä, mutta voit luoda virtuaalisen hierarkian nimeämällä BLOB polku tiedoilla. Kuitenkin Normaali luettelo on tällä hetkellä ei tueta. Tämä on tulossa pian. Nyt tämän arvon pitäisi olla`YES`
- **blobListingDetails** - voidaan määrittää nimikkeet sisällytettävät luettelon BLOB-objektit
    - `AZSBlobListingDetailsNone`: Luettelosta vain vahvistettu BLOB-objektit ja eivät palauta blob-metatiedot.
    - `AZSBlobListingDetailsSnapshots`:-Luettelosta sitoutunut BLOB-objektit ja Blob-objektien tilannevedoksia.
    - `AZSBlobListingDetailsMetadata`: Kunkin blob noutaa blob-metatiedot palauttaa luettelon.
    - `AZSBlobListingDetailsUncommittedBlobs`: Luettelon vähimmäis- ja ei ole BLOB-objektit.
    - `AZSBlobListingDetailsCopy`: Sisällytä kopioi ominaisuudet luetteloa.
    - `AZSBlobListingDetailsAll`: Luettelon kaikki käytettävissä olevat vahvistettu BLOB-objektit, ei ole BLOB-objektit ja tilannevedoksia ja palauttaa kaikki nämä BLOB metatietojen ja kopioi tilan.
- **maxResults** - toiminnolle tulosten enimmäismäärä. Käytä 1-ei asetettu rajoitukset.
- **completionHandler** - koodin suorittaminen ja luettelo-toiminnon tulos tekstialueen.

Tässä esimerkissä helper menetelmä on käytettävä rekursiivisesti puhelun luettelon BLOB menetelmä aina, kun jatkumista tunnuksen palautetaan.

    -(void)listBlobsInContainer{
        NSError *accountCreationError;

        // Create a storage account object from a connection string.
        AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

        if(accountCreationError){
            NSLog(@"Error in creating account.");
        }

        // Create a blob service client object.
        AZSCloudBlobClient *blobClient = [account getBlobClient];

        // Create a local container object.
        AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

        //List all blobs in container
        [self listBlobsInContainerHelper:blobContainer continuationToken:nil prefix:nil blobListingDetails:AZSBlobListingDetailsAll maxResults:-1 completionHandler:^(NSError *error) {
            if (error != nil){
                NSLog(@"Error in creating container.");
            }
        }];
    }

    //List blobs helper method
    -(void)listBlobsInContainerHelper:(AZSCloudBlobContainer *)container continuationToken:(AZSContinuationToken *)continuationToken prefix:(NSString *)prefix blobListingDetails:(AZSBlobListingDetails)blobListingDetails maxResults:(NSUInteger)maxResults completionHandler:(void (^)(NSError *))completionHandler
    {
        [container listBlobsSegmentedWithContinuationToken:continuationToken prefix:prefix useFlatBlobListing:YES blobListingDetails:blobListingDetails maxResults:maxResults completionHandler:^(NSError *error, AZSBlobResultSegment *results) {
            if (error)
            {
                completionHandler(error);
            }
            else
            {
                for (int i = 0; i < results.blobs.count; i++) {
                    NSLog(@"%@",[(AZSCloudBlockBlob *)results.blobs[i] blobName]);
                }
                if (results.continuationToken)
                {
                    [self listBlobsInContainerHelper:container continuationToken:results.continuationToken prefix:prefix blobListingDetails:blobListingDetails maxResults:maxResults completionHandler:completionHandler];
                }
                else
                {
                    completionHandler(nil);
                }
            }
        }];
    }


## <a name="download-a-blob"></a>Lataa blob

Seuraavassa esimerkissä esitetään blob lataamisesta NSString objektiin.

    -(void)downloadBlobToString{
        NSError *accountCreationError;

        // Create a storage account object from a connection string.
        AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

        if(accountCreationError){
            NSLog(@"Error in creating account.");
        }

        // Create a blob service client object.
        AZSCloudBlobClient *blobClient = [account getBlobClient];

        // Create a local container object.
        AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

        // Create a local blob object
        AZSCloudBlockBlob *blockBlob = [blobContainer blockBlobReferenceFromName:@"sampleblob"];

        // Download blob
        [blockBlob downloadToTextWithCompletionHandler:^(NSError *error, NSString *text) {
            if (error) {
                NSLog(@"Error in downloading blob");
            }
            else{
                NSLog(@"%@",text);
            }
        }];
    }

## <a name="delete-a-blob"></a>Poista blob

Seuraavassa esimerkissä esitetään blob poistamisesta.

    -(void)deleteBlob{
        NSError *accountCreationError;

        // Create a storage account object from a connection string.
        AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

        if(accountCreationError){
            NSLog(@"Error in creating account.");
        }

        // Create a blob service client object.
        AZSCloudBlobClient *blobClient = [account getBlobClient];

        // Create a local container object.
        AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

        // Create a local blob object
        AZSCloudBlockBlob *blockBlob = [blobContainer blockBlobReferenceFromName:@"sampleblob1"];

        // Delete blob
        [blockBlob deleteWithCompletionHandler:^(NSError *error) {
            if (error) {
                NSLog(@"Error in deleting blob.");
            }
        }];
    }

## <a name="delete-a-blob-container"></a>Poista blob-säilö

Seuraavassa esimerkissä esitetään säilön poistaminen.

    -(void)deleteContainer{
      NSError *accountCreationError;

      // Create a storage account object from a connection string.
      AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

      if(accountCreationError){
         NSLog(@"Error in creating account.");
      }

      // Create a blob service client object.
      AZSCloudBlobClient *blobClient = [account getBlobClient];

      // Create a local container object.
      AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

      // Delete container
      [blobContainer deleteContainerIfExistsWithCompletionHandler:^(NSError *error, BOOL success) {
         if(error){
             NSLog(@"Error in deleting container");
         }
      }];
    }

## <a name="next-steps"></a>Seuraavat vaiheet

Nyt kun olet tutustunut käyttämisestä Blob-objektien tallennustilaan iOS-, näistä linkeistä saat lisätietoja iOS-kirjaston ja tallennustilaa palvelu.

- [IOS Azure tallennustilan asiakkaan kirjasto](https://github.com/azure/azure-storage-ios)
- [Azure tallennustilan iOS oppaat](http://azure.github.io/azure-storage-ios/)
- [Azure liittyviä palveluja REST-Ohjelmointirajapinnalla](https://msdn.microsoft.com/library/azure/dd179355.aspx)
- [Azure-tallennustilan tiimin blogia](http://blogs.msdn.com/b/windowsazurestorage)

Jos sinulla on kysymyksiä koskeva tämän kirjaston vapaasti julkaiseminen Microsoftin [MSDN Azure-keskustelupalstalla](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=windowsazuredata) tai [Pinon ylivuoto](http://stackoverflow.com/questions/tagged/windows-azure-storage+or+windows-azure-storage+or+azure-storage-blobs+or+azure-storage-tables+or+azure-table-storage+or+windows-azure-queues+or+azure-storage-queues+or+azure-storage-emulator+or+azure-storage-files).
Jos ominaisuus Azuren tallennustilaan ehdotuksia, Lähetä [Azure-tallennustilan palautetta](https://feedback.azure.com/forums/217298-storage/).
