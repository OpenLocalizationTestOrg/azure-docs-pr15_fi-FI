<properties
    pageTitle="Käyttämisestä Node.js-Blob-objektien tallennustilaan | Microsoft Azure"
    description="Tallentaa erimuotoisia tietoja pilveen Azure-Blob-säiliö (objektin tallennus) kanssa."
    services="storage"
    documentationCenter="nodejs"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="tamram"/>



# <a name="how-to-use-blob-storage-from-nodejs"></a>Opi käyttämään Node.js-Blob-objektien tallennustilaan

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Yleiskatsaus

Tämän artikkelin avulla voit suorittaa yleisiä tilanteita, joissa käyttämällä Blob-objektien tallennustilaan. Mallit kirjoitetaan Node.js Ohjelmointirajapinnan kautta. Tilanteita, joissa kattaa Sisällytä lataaminen, luettelon, lataa ja poistamisesta BLOB-objektit.

[AZURE.INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-nodejs-application"></a>Node.js-sovelluksen luominen

Ohjeita siitä, miten voit luoda Node.js sovelluksen on artikkelissa [Luo Node.js verkkosovellukseen Azure-sovelluksen käytössä], [Muodosta ja ota käyttöön Node.js-sovelluksen Azure pilvipalveluun] --Windows PowerShellin tai [Muodosta ja ota käyttöön Node.js verkkosovellukseen, käyttäen Web-matriisin Azure].

## <a name="configure-your-application-to-access-storage"></a>Määritä sovelluksesi voi käyttää tallennustilan

Azure tallennuskiintiön käyttämään tarvitset Azure-tallennustilan SDK Node.js, joka sisältää helppokäyttöisyys kirjastoja, joissa viestintä tallennustilan muiden palveluiden kanssa.

### <a name="use-node-package-manager-npm-to-obtain-the-package"></a>Hanki paketin solmu paketin hallinta (NPM) avulla

1.  Siirry kansioon, jossa malli-sovellus on luotu esimerkiksi **PowerShell** (Windows), **terminaalissa** (Mac) tai (Unix) **Bash** käyttöliittymä avulla.

2.  Kirjoita **npm asentaa azure-tallennustilan** komentorivi-ikkuna. Seuraava koodiesimerkki muistuttaa tulostus-komennolla.

        azure-storage@0.5.0 node_modules\azure-storage
        +-- extend@1.2.1
        +-- xmlbuilder@0.4.3
        +-- mime@1.2.11
        +-- node-uuid@1.4.3
        +-- validator@3.22.2
        +-- underscore@1.4.4
        +-- readable-stream@1.0.33 (string_decoder@0.10.31, isarray@0.0.1, inherits@2.0.1, core-util-is@1.0.1)
        +-- xml2js@0.2.7 (sax@0.5.2)
        +-- request@2.57.0 (caseless@0.10.0, aws-sign2@0.5.0, forever-agent@0.6.1, stringstream@0.0.4, oauth-sign@0.8.0, tunnel-agent@0.4.1, isstream@0.1.2, json-stringify-safe@5.0.1, bl@0.9.4, combined-stream@1.0.5, qs@3.1.0, mime-types@2.0.14, form-data@0.2.0, http-signature@0.11.0, tough-cookie@2.0.0, hawk@2.3.1, har-validator@1.8.0)

3.  Voit suorittaa manuaalisesti, varmista, että **ls** -komento **solmu\_moduulit** kansio on luotu. Etsi **azure-tallennustilan** -paketti, joka sisältää kirjastoja, joita haluat käyttää tallennustilan kyseiseen kansioon.

### <a name="import-the-package"></a>Tuo pakkaaminen

Käytä Muistioon tai toisen tekstieditorissa, Lisää seuraava teksti **server.js** -tiedoston sovelluksen, jos aiot käyttää tallennustilan alkuun:

    var azure = require('azure-storage');

## <a name="set-up-an-azure-storage-connection"></a>Azuren tallennustilaan-yhteyden määrittäminen

Azure moduulin lukee ympäristömuuttujat `AZURE_STORAGE_ACCOUNT` ja `AZURE_STORAGE_ACCESS_KEY`, tai `AZURE_STORAGE_CONNECTION_STRING`, muodostettava yhteys Azure-tallennustilan tilin tiedot. Jos ympäristössä muuttujia ei määritetä, tilitiedot on määritettävä **createBlobService**soitettaessa.

Esimerkki määrittäminen ympäristömuuttujat Azure-verkkosovelluksessa [Azure-portaalissa](https://portal.azure.com) on artikkelissa [Node.js online-Azure-taulukosta-palvelun avulla].

## <a name="create-a-container"></a>Luoda säilön

**BlobService** objektin voit käsitteleminen säiliöiden ja BLOB-objektit. Seuraava koodi luo **BlobService** objektin. Lisää **server.js**yläosassa seuraavasti:

    var blobSvc = azure.createBlobService();

> [AZURE.NOTE] Voit käyttää blob nimettömänä käyttämällä **createBlobServiceAnonymous** ja tarjoamalla isännän osoite. Esimerkiksi `var blobSvc = azure.createBlobServiceAnonymous('https://myblob.blob.core.windows.net/');`.

[AZURE.INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

Voit luoda uuden säilön käyttämällä **createContainerIfNotExists**. Koodin seuraavassa esimerkissä luodaan uusi säilö 'mycontainer':

    blobSvc.createContainerIfNotExists('mycontainer', function(error, result, response){
        if(!error){
          // Container exists and is private
        }
    });

Jos säilö juuri luonut, `result.created` on TOSI. Jos säilö on jo olemassa, `result.created` on EPÄTOSI. `response`sisältää tietoja toiminnon, mukaan lukien säilö ETag tiedot.

### <a name="container-security"></a>Säilön suojaus

Oletusarvon mukaan uuden säilöjen yksityisiä eikä sitä voi käyttää anonyymisti. Säilön julkisiksi, jotta voit avata sen nimettömänä, voit määrittää säilön käyttöoikeustaso ja **blob** - **säilö**.

* **blob** - sallii anonyymi lukuoikeus blob sisältöä ja metatietojen tämän säilön, mutta ei säilö metatietoja, kuten luettelon kaikki Blob-objektien säilöön

* **säilön** - sallii anonyymi lukuoikeudet blob sisältöä ja metatiedot sekä säilö metatiedot

Seuraava koodiesimerkki esittelee käyttöoikeustaso asettaminen **blob**:

    blobSvc.createContainerIfNotExists('mycontainer', {publicAccessLevel : 'blob'}, function(error, result, response){
        if(!error){
          // Container exists and allows
          // anonymous read access to blob
          // content and metadata within this container
        }
    });

Vaihtoehtoisesti voit muokata käyttöoikeustasoa säilön **setContainerAcl** avulla voit määrittää haluamasi käyttöoikeustaso. Seuraavassa esimerkissä koodin muuttuu käyttöoikeustaso säilöön:

    blobSvc.setContainerAcl('mycontainer', null /* signedIdentifiers */, {publicAccessLevel : 'container'} /* publicAccessLevel*/, function(error, result, response){
      if(!error){
        // Container access level set to 'container'
      }
    });

Tulos on toiminto, kuten nykyisen **ETag** säilö tietoja.

### <a name="filters"></a>Suodattimet

Voit käyttää **BlobService**suoritettujen toimintojen valinnainen suodatuksen toimintoja. Suodattaminen toimintojen sisällyttää kirjaaminen, yritetään automaattisesti jne. Suodattimet ovat objekteja, jotka toteuttavat allekirjoituksen menetelmää:

    function handle (requestOptions, next)

Kun teet sen esikäsittely pyynnön valitsemalla asetukset, menetelmä täytyy kutsua "seuraavaksi" kulkeva takaisinkutsun seuraavat allekirjoituksen:

    function (returnObject, finalCallback, next)

Tämä takaisinkutsussa ja returnObject (vastausta pyynnön palvelimeen) käsittelyn jälkeen takaisinkutsu on ongelma seuraava, jos sellainen on edelleen käsittelyn muut suodattimet tai kutsua ainoastaan finalCallback palvelun käynnistäminen loppuun.

Kaksi suodatinta, jotka toteuttavat uudelleen logiikan kuuluvat Node.js, **ExponentialRetryPolicyFilter** ja **LinearRetryPolicyFilter**Azure SDK-paketissa. **BlobService** -objekti, joka käyttää **ExponentialRetryPolicyFilter**Luo seuraavasti:

    var retryOperations = new azure.ExponentialRetryPolicyFilter();
    var blobSvc = azure.createBlobService().withFilter(retryOperations);

## <a name="upload-a-blob-into-a-container"></a>Lataa blob säilöön

On kolmenlaisia BLOB: estää BLOB-objektit, sivun BLOB-objektit ja liitä BLOB-objektit. Estä BLOB Salli Lisäasetukset tehokkaasti lataat suuria tiedot. Liitä BLOB on optimoitu Lisää toimintoja. Sivun BLOB-objektit on tarkoitettu kirjoitus-toimintoja. Lisätietoja on artikkelissa [tietoja estä BLOB-objektit, Liitä BLOB ja sivun BLOB-objektit](http://msdn.microsoft.com/library/azure/ee691964.aspx).

### <a name="block-blobs"></a>Estä BLOB-objektit

Tietojen lataaminen lohko-blob-toimimalla seuraavasti:

* **createBlockBlobFromLocalFile** - Luo uusi estä Blob-objektien ja lataa tiedoston sisällön

* **createBlockBlobFromStream** - Luo uusi estä Blob-objektien ja lataa stream sisältö

* **createBlockBlobFromText** - Luo uusi estä Blob-objektien ja lataa merkkijonon sisältö

* **createWriteStreamToBlockBlob** – tarjoaa kirjoitus-muodossa, estä Blob-objektien

Seuraavassa esimerkissä koodi Lataa **test.txt** -tiedoston sisällön **myblob**kyselyjä.

    blobSvc.createBlockBlobFromLocalFile('mycontainer', 'myblob', 'test.txt', function(error, result, response){
      if(!error){
        // file uploaded
      }
    });

`result` Näistä tavoista palauttama sisältää toiminnon, kuten **ETag** blob tiedot.

### <a name="append-blobs"></a>Liitä BLOB-objektit

Lataa tiedot uusien Liitä Blob-objektien, toimimalla seuraavasti:

* **createAppendBlobFromLocalFile** - Luo uusi Liitä Blob-objektien ja lataa tiedoston sisällön

* **createAppendBlobFromStream** - Luo uusi Liitä Blob-objektien ja lataa stream sisältö

* **createAppendBlobFromText** - Luo uusi Liitä Blob-objektien ja lataa merkkijonon sisältö

* **createWriteStreamToNewAppendBlob** - Luo uusi Liitä Blob-objektien ja tarjoaa stream kirjoittaa siihen

Seuraavassa esimerkissä koodi Lataa **test.txt** -tiedoston sisällön **myappendblob**kyselyjä.

    blobSvc.createAppendBlobFromLocalFile('mycontainer', 'myappendblob', 'test.txt', function(error, result, response){
      if(!error){
        // file uploaded
      }
    });

Liittää aiemmin Liitä Blob-objektien eston toimimalla seuraavasti:

* **appendFromLocalFile** - tiedoston sisällön liittää aiemmin Liitä-blob

* **appendFromStream** - stream sisältöä liittää aiemmin Liitä-blob

* **appendFromText** - merkkijono sisältöä liittää aiemmin Liitä-blob

* **appendBlockFromStream** - stream sisältöä liittää aiemmin Liitä-blob

* **appendBlockFromText** - merkkijono sisältöä liittää aiemmin Liitä-blob

> [AZURE.NOTE] appendFromXXX API tällöin toimi joitakin asiakkaan vahvistus epäonnistuu nopeasti voit välttää unncessary palvelimen kutsu. appendBlockFromXXX ei.

Seuraavassa esimerkissä koodi Lataa **test.txt** -tiedoston sisällön **myappendblob**kyselyjä.

    blobSvc.appendFromText('mycontainer', 'myappendblob', 'text to be appended', function(error, result, response){
      if(!error){
        // text appended
      }
    });


### <a name="page-blobs"></a>Sivun BLOB-objektit

Tietojen lataaminen sivun Blob-objektien, toimimalla seuraavasti:

* **createPageBlob** - Luo uusi sivu-blob tietyn pituus

* **createPageBlobFromLocalFile** - Luo uusi sivu-Blob-objektien ja lataa tiedoston sisällön

* **createPageBlobFromStream** - Luo uusi sivu-Blob-objektien ja lataa stream sisältö

* **createWriteStreamToExistingPageBlob** – tarjoaa kirjoitus-virta olemassa olevan sivun Blob-objektien

* **createWriteStreamToNewPageBlob** - Luo uusi sivu-Blob-objektien ja tarjoaa stream kirjoittaa siihen

Seuraavassa esimerkissä koodi Lataa **test.txt** -tiedoston sisällön **mypageblob**kyselyjä.

    blobSvc.createPageBlobFromLocalFile('mycontainer', 'mypageblob', 'test.txt', function(error, result, response){
      if(!error){
        // file uploaded
      }
    });

> [AZURE.NOTE] Sivun BLOB koostuvat 512 tavun "sivut". Saat virheen ladattaessa tietojasi koko, joka ei ole 512 kerrannaiseen.

## <a name="list-the-blobs-in-a-container"></a>Luettelon BLOB-säilö

Luettelon BLOB-säilö, käytä **listBlobsSegmented** -menetelmää. Jos haluat palauttaa BLOB tietyn etuliite, käytä **listBlobsSegmentedWithPrefix**.

    blobSvc.listBlobsSegmented('mycontainer', null, function(error, result, response){
      if(!error){
          // result.entries contains the entries
          // If not all blobs were returned, result.continuationToken has the continuation token.
      }
    });

`result` Sisältää `entries` kokoelma, joka on matriisikaava, jotka kuvaavat kunkin blob-objektien. Jos kaikki Blob-objektien ei voi palauttaa, `result` sekä `continuationToken`, jota voi käyttää toisena parametrina hakemiseen lisäkohteiden.

## <a name="download-blobs"></a>Lataa BLOB-objektit

Tietojen lataaminen blob, toimimalla seuraavasti:

* **getBlobToLocalFile** - kirjoittaa tiedostoon blob-sisältö

* **getBlobToStream** - kirjoittaa stream blob-sisältö

* **getBlobToText** - kirjoittaa merkkijonon blob-sisältö

* **createReadStream** – tarjoaa stream blob lukeminen

Koodin seuraavassa esimerkissä on kuvattu, Lataa **myblob** Blob-objektien sisällön ja tallentaa sen **output.txt** -tiedoston avulla stream **getBlobToStream** avulla:

    var fs = require('fs');
    blobSvc.getBlobToStream('mycontainer', 'myblob', fs.createWriteStream('output.txt'), function(error, result, response){
      if(!error){
        // blob retrieved
      }
    });

`result` Sisältää tietoja Blob-objektien, mukaan lukien **ETag** tiedot.

## <a name="delete-a-blob"></a>Poista blob

Lopuksi voit poistaa blob-kutsu **deleteBlob**. Seuraavassa esimerkissä koodi poistaa nimeltä **myblob**Blob-objektien.

    blobSvc.deleteBlob(containerName, 'myblob', function(error, response){
      if(!error){
        // Blob has been deleted
      }
    });

## <a name="concurrent-access"></a>Samanaikainen käyttö

Tuen samanaikainen käyttö blob useasta asiakasohjelmasta käsin tai useita prosessin esiintymät, voit käyttää **vuokraaminen**tai **ETags** .

* **Etag** - avulla on helppo tunnistaa, blob tai säilö on muutettu toisen prosessin käytössä

* **Varaus** - avulla on helppo saada yksityisesti, uusia, kirjoittaa tai poistaa blob pääsy ajan kuluessa

### <a name="etag"></a>ETag

Käytä ETags, jos sinun on sallittava kirjoittaminen estä Blob-objektien tai sivun useiden asiakkaiden tai esiintymät Blob samanaikaisesti. ETag avulla voit selvittää, jos blob-säilö on muokattu alun perin lukea tai se on luotu, jonka avulla voit välttää korvaaminen muutosten vahvistamiseksi toisen asiakkaan tai prosessin jälkeen.

Voit määrittää ETag ehdot käyttämällä valinnainen `options.accessConditions` parametria. Seuraava koodiesimerkki latauksia **test.txt** tiedoston vain, jos blob on jo olemassa ja on ETag-arvo sisältää mukaan `etagToMatch`.

    blobSvc.createBlockBlobFromLocalFile('mycontainer', 'myblob', 'test.txt', { accessConditions: { EtagMatch: etagToMatch} }, function(error, result, response){
        if(!error){
        // file uploaded
      }
    });

Kun käytät ETags, yleinen kaava on:

1. Hanki ETag luominen, luettelo tai hakutoiminto tuloksena.

2. Suorita kohteelle toiminto, tarkistuksen ETag-arvo ei ole muutettu.

Jos arvo on muokattu, tämä ilmaisee, että toisen asiakkaan tai esiintymän muokannut Blob-objektien tai säilö koska olet hankkinut ETag-arvo.

### <a name="lease"></a>Varaus

Voit hankkia uuden varauksen **acquireLease** menetelmällä, Blob-objektien tai säilön, johon haluat saada varaus. Esimerkiksi seuraava koodi hankkii varauksen **myblob**käyttöön.

    blobSvc.acquireLease('mycontainer', 'myblob', function(error, result, response){
      if(!error) {
        console.log('leaseId: ' + result.id);
      }
    });

Seuraavat toimenpiteet **myblob** on määritettävä `options.leaseId` parametria. Tunnus palauttaa varauksen `result.id` - **acquireLease**.

> [AZURE.NOTE] Oletusarvon mukaan varauksen pituus on ääretön. Voit määrittää jatkuva kestoksi (15 ja 60 sekuntia) antamalla `options.leaseDuration` parametria.

Jos haluat poistaa varaus, käytä **releaseLease**. Voit katkaista varaus, mutta estää muita käyttäjiä hankkiminen uuden varauksen, kunnes alkuperäinen kesto on päättynyt, käyttämällä **breakLease**.

## <a name="work-with-shared-access-signatures"></a>Jaettu käyttö allekirjoitusten käyttäminen

Jaettu käyttö allekirjoitukset (SAS) ovat suojatun antamaan hajautetun pääsy BLOB-objektit ja säilöjä mutta estää tallennustilan tilin nimi tai -avaimet. Jaettu käyttö allekirjoituksia käytetään usein antamaan tiedot, kuten salliminen mobiilisovelluksessa käyttämään BLOB rajoitettu käyttöoikeus.

> [AZURE.NOTE] Voit sallia myös BLOB anonyymin käytön, kun jaettua käyttöä allekirjoitukset mahdollistavat antamaan Lisää hallittu, sinun on luotava Suojaussidokset.

Luotettu sovellus, esimerkiksi pilvipohjaisia palveluja Luo jaettu käyttö allekirjoitusten käyttäminen **BlobService** **generateSharedAccessSignature** ja antaa sen epäluotettavista tai osittain luotettu sovelluksen kuten mobiilisovelluksessa. Käyttöoikeuden allekirjoitukset luodaan käytännön, joka kuvaa alkamis- ja päättymispäivämäärät aikana jaetun access-allekirjoitukset ovat kelvollisia sekä käyttöoikeustaso myöntää jaettua käyttöä allekirjoitukset omistaja.

Seuraava koodiesimerkki Luo uusi jaetun access-käytäntö, jonka avulla jaettuun käyttöön allekirjoitukset haltijan **myblob** Blob-objektien luku toimintojen suorittamiseen ja päättyy 100 minuutin ajan sen luomisen jälkeen.

    var startDate = new Date();
    var expiryDate = new Date(startDate);
    expiryDate.setMinutes(startDate.getMinutes() + 100);
    startDate.setMinutes(startDate.getMinutes() - 100);

    var sharedAccessPolicy = {
      AccessPolicy: {
        Permissions: azure.BlobUtilities.SharedAccessPermissions.READ,
        Start: startDate,
        Expiry: expiryDate
      },
    };

    var blobSAS = blobSvc.generateSharedAccessSignature('mycontainer', 'myblob', sharedAccessPolicy);
    var host = blobSvc.host;

Huomaa, että isännän tiedoista on annettava myös, kun se on tarpeen, kun jaettu käyttö allekirjoitukset haltijan yrittää käyttää säilö.

Valitse asiakassovellus käyttävä **BlobServiceWithSAS** jaettua käyttöä allekirjoitukset vastaan blob toimintojen suorittaminen. Seuraavat saa **myblob**tietoja.

    var sharedBlobSvc = azure.createBlobServiceWithSas(host, blobSAS);
    sharedBlobSvc.getBlobProperties('mycontainer', 'myblob', function (error, result, response) {
      if(!error) {
        // retrieved info
      }
    });

Koska jaetun access-allekirjoitukset luotiin ja vain luku-tilassa, jos yritys tehdä muutoksia blob, palautetaan virhe.

### <a name="access-control-lists"></a>Käyttöoikeusluettelot

Voit myös määrittää access-käytännön suojaussidosten käyttöoikeusluettelo (Käyttöoikeusluettelon). Tämä on kätevä, jos haluat käyttää säilön mutta erilaisia käytäntöjen voi kunkin asiakkaan useita asiakkaat.

Käyttöoikeusluettelon on toteutettu matriisin käytön käytännöt käyttäminen kunkin käytännön liittyvä tunnus. Seuraava koodiesimerkki määrittää kahden käytäntöjä, yksi 'Käyttäjä1' ja yksi 'käyttäjä2':

    var sharedAccessPolicy = {
      user1: {
        Permissions: azure.BlobUtilities.SharedAccessPermissions.READ,
        Start: startDate,
        Expiry: expiryDate
      },
      user2: {
        Permissions: azure.BlobUtilities.SharedAccessPermissions.WRITE,
        Start: startDate,
        Expiry: expiryDate
      }
    };

Seuraava koodiesimerkki noutaa nykyisen Käyttöoikeusluettelon **mycontainer**ja lisää uusia käytäntöjä **setBlobAcl**avulla. Tämän menetelmän avulla:

    var extend = require('extend');
    blobSvc.getBlobAcl('mycontainer', function(error, result, response) {
      if(!error){
        var newSignedIdentifiers = extend(true, result.signedIdentifiers, sharedAccessPolicy);
        blobSvc.setBlobAcl('mycontainer', newSignedIdentifiers, function(error, result, response){
          if(!error){
            // ACL set
          }
        });
      }
    });

Kun Käyttöoikeusluettelon on määritetty, voit luoda jaettu käyttö allekirjoitukset käytännön tunnuksen perusteella. Koodin seuraavassa esimerkissä luodaan uusi "käyttäjä2" jaettu käyttö allekirjoitukset:

    blobSAS = blobSvc.generateSharedAccessSignature('mycontainer', { Id: 'user2' });

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja on seuraavissa resursseissa.

-   [Azure-tallennustilan SDK solmu API-viittaus][]
-   [Azure-tallennustilan tiimin blogia][]
-   Valitse GitHub [Azure-tallennustilan SDK solmun][] säilöön
-   [Node.js Developer Center](/develop/nodejs/)
-   [Siirrä tiedot AzCopy komentorivin-apuohjelman avulla](storage-use-azcopy.md)

[Azure-tallennustilan SDK solmun]: https://github.com/Azure/azure-storage-node

[Luo Node.js verkkosovellukseen Azure sovelluksen-palvelussa]: ../app-service-web/web-sites-nodejs-develop-deploy-mac.md
[Node.js Cloud Service with Storage]: ../cloud-services/storage-nodejs-use-table-storage-cloud-service-app.md
[Azure-taulukosta palveluun node.js web Appissa]: ../app-service-web/storage-nodejs-use-table-storage-web-site.md
[Muodosta ja ota käyttöön Node.js verkkosovellukseen Azure käyttäminen WWW-matriisi]: ../app-service-web/web-sites-nodejs-use-webmatrix.md
[Using the REST API]: http://msdn.microsoft.com/library/azure/hh264518.aspx
[Azure Portal]: https://portal.azure.com
[Muodosta ja ota käyttöön Node.js-sovelluksen Azure pilvipalveluun]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
[Azure-tallennustilan tiimin blogia]: http://blogs.msdn.com/b/windowsazurestorage/
[Azure-tallennustilan SDK solmu API-viittaus]: http://dl.windowsazure.com/nodestoragedocs/index.html
