<properties
    pageTitle="Käyttämisestä jonon tallennustilaa Node.js | Microsoft Azure"
    description="Opettele Azure jonon-palvelun avulla voit luoda ja poistaa olevien, Lisää, Hae ja poistaa viestit. Esimerkkejä, joiden on kirjoitettu Node.js."
    services="storage"
    documentationCenter="nodejs"
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robinsh"/>


# <a name="how-to-use-queue-storage-from-nodejs"></a>Opi käyttämään jonon tallennustilaa Node.js

[AZURE.INCLUDE [storage-selector-queue-include](../../includes/storage-selector-queue-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Yleiskatsaus

Tämän oppaan avulla voit suorittaa yleisiä tilanteita, joissa on Microsoft Azure jonon-palvelun avulla. Mallit kirjoitetaan Node.js Ohjelmointirajapinnan käyttäminen. Tilanteita, joissa kattaa Sisällytä **lisääminen**, **Vilkaiseminen**, **käytön**ja **poistamalla** niiden sanomien sekä **luominen ja poistaminen olevien**.

[AZURE.INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-nodejs-application"></a>Node.js-sovelluksen luominen

Luo tyhjä Node.js-sovellus. Katso ohjeet luomalla Node.js sovellus [Luo Node.js verkkosovellukseen Azure-sovelluksen käytössä], [Muodosta ja ota käyttöön Node.js-sovelluksen Azure pilvipalveluun] käyttämällä Windows PowerShellin tai [Muodosta ja ota käyttöön, käyttäen Web-matriisin Azure Node.js verkkosovellukseen].

## <a name="configure-your-application-to-access-storage"></a>Voit käyttää tallennustilaa sovelluksen määrittäminen

Azure tallennuskiintiön käyttämään tarvitset Azure-tallennustilan SDK Node.js, joka sisältää helppokäyttöisyys kirjastoja, joissa viestintä tallennustilan muiden palveluiden kanssa.

### <a name="use-node-package-manager-npm-to-obtain-the-package"></a>Hanki paketin solmu paketin hallinta (NPM) avulla

1.  Käytä käyttöliittymä **PowerShell** (Windows), kuten **terminaalissa** (Mac) tai **Bash** (Unix), Selaa kansioon, jossa malli-sovellus on luotu.

2.  Kirjoita **npm asentaa azure-tallennustilan** komentorivi-ikkuna. Seuraavassa esimerkissä muistuttaa tulostus-komennolla.

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

3.  Voit suorittaa manuaalisesti, varmista, että **ls** -komento **solmu\_moduulit** kansio on luotu. Kyseiseen kansioon etsii **azure-tallennustilan** -paketti, joka sisältää kirjastoja, sinun täytyy käyttää tallennustilan.

### <a name="import-the-package"></a>Tuo pakkaaminen

Käytä Muistioon tai toisen tekstieditorissa, Lisää seuraava teksti yläreunaan **server.js** -tiedoston sovelluksen, jos aiot käyttää tallennustilan:

    var azure = require('azure-storage');

## <a name="setup-an-azure-storage-connection"></a>Azure-tallennustilan-yhteyden määrittäminen

Azure moduulin lukee ympäristömuuttujat AZURE\_TALLENNUSTILAN\_tilin ja AZURE\_TALLENNUSTILAN\_ACCESS\_AVAIMEN tai AZURE\_TALLENNUSTILAN\_yhteyden\_merkkijono, voit muodostaa yhteyden tiliisi Azure tallennustilan tarvittavat tiedot. Jos ympäristössä muuttujia ei määritetä, tilitiedot on määritettävä **createQueueService**soitettaessa.

Esimerkki määrittäminen ympäristömuuttujat [Azure Portal](https://portal.azure.com) Azure-sivustoon Katso [Node.js web Appin Azure-taulukosta-palvelun avulla].

## <a name="how-to-create-a-queue"></a>Toimintaohje: Jonon luominen

Seuraava koodi luo **QueueService** -objekti, jonka avulla voit käsitellä olevien.

    var queueSvc = azure.createQueueService();

Käytä **createQueueIfNotExists** -menetelmää, joka palauttaa määritetyn jonossa, jos se on jo käytössä tai luo uuden jonon määritetty nimi, jos se ei ole jo olemassa.

    queueSvc.createQueueIfNotExists('myqueue', function(error, result, response){
      if(!error){
        // Queue created or exists
      }
    });

Jos jonossa on luotu, `result.created` on TOSI. Jos on olemassa jonossa, `result.created` on EPÄTOSI.

### <a name="filters"></a>Suodattimet

Valinnainen suodatus toimintoja voidaan lisätä **QueueService**suoritettujen toimintojen. Suodattaminen toimintojen sisällyttää kirjaaminen, yritetään automaattisesti jne. Suodattimet ovat objekteja, jotka toteuttavat allekirjoituksen menetelmää:

    function handle (requestOptions, next)

Kun teet sen esikäsittely pyynnön valitsemalla asetukset, menetelmä täytyy kutsua "Seuraava" kulkeva takaisinkutsun seuraavat allekirjoituksen:

    function (returnObject, finalCallback, next)

Tämä takaisinkutsussa ja returnObject (vastausta pyynnön palvelimeen) käsittelyn jälkeen takaisinkutsu on ongelma seuraava, jos sellainen on edelleen käsittelyn muut suodattimet tai kutsua ainoastaan finalCallback muussa palvelun käynnistäminen avulla.

Kaksi suodatinta, jotka toteuttavat uudelleen logiikan kuuluvat Node.js, **ExponentialRetryPolicyFilter** ja **LinearRetryPolicyFilter**Azure SDK-paketissa. **QueueService** -objekti, joka käyttää **ExponentialRetryPolicyFilter**Luo seuraavasti:

    var retryOperations = new azure.ExponentialRetryPolicyFilter();
    var queueSvc = azure.createQueueService().withFilter(retryOperations);

## <a name="how-to-insert-a-message-into-a-queue"></a>Toimintaohje: Lisääminen viestiin jonossa

Jos haluat lisätä viestiin jonossa, käytä uuden viestin luominen ja lisääminen jonossa **createMessage** -menetelmää.

    queueSvc.createMessage('myqueue', "Hello world!", function(error, result, response){
      if(!error){
        // Message inserted
      }
    });

## <a name="how-to-peek-at-the-next-message"></a>Toimintaohje: Vilkaista seuraava viesti

Voit vilkaista jonon edestä suojuksen viestin poistamatta jonossa, **peekMessages** -menetelmällä. Oletusarvon mukaan **peekMessages** tarkastelee käyttäminen yhdessä viestissä.

    queueSvc.peekMessages('myqueue', function(error, result, response){
      if(!error){
        // Message text is in messages[0].messageText
      }
    });

`result` Sisältää viestin.

> [AZURE.NOTE] Käytä **peekMessages** , kun on jonossa viestejä ei voi palauttaa virheen, mutta viestejä ei palautetaan.

## <a name="how-to-dequeue-the-next-message"></a>Toimintaohje: Jonosta poistamisen seuraava viesti

Viestin käsittely on kaksivaiheinen prosessi:

1. Jonosta poistamisen viestin.

2. Poista viesti.

Jos haluat jonosta viestin poistamisen, käytä **getMessages**. Näin viestit näkymätön jonossa, jotta muut asiakkaat voivat käsitellä niitä. Kun sovellus on käsitellyt viestin, soita **deleteMessage** poistaminen jonossa. Seuraavassa esimerkissä saa viestin ja poistaa sen:

    queueSvc.getMessages('myqueue', function(error, result, response){
      if(!error){
        // Message text is in messages[0].messageText
        var message = result[0];
        queueSvc.deleteMessage('myqueue', message.messageId, message.popReceipt, function(error, response){
          if(!error){
            //message deleted
          }
        });
      }
    });

> [AZURE.NOTE] Oletusarvon mukaan viestin piilotetaan vain 30 sekunnin ajan, minkä jälkeen se on näkyvissä muille henkilöille. Voit määrittää arvon käyttämällä `options.visibilityTimeout` **getMessages**kanssa.

> [AZURE.NOTE]
> Käytä **getMessages** , kun on jonossa viestejä ei voi palauttaa virheen, mutta viestejä ei palautetaan.

## <a name="how-to-change-the-contents-of-a-queued-message"></a>Toimintaohje: Jonossa viestin sisällön muuttaminen

Voit muuttaa viestin käytönaikainen käyttämällä **updateMessage**jonossa sisällön. Seuraavassa esimerkissä päivittää viestin tekstiin:

    queueSvc.getMessages('myqueue', function(error, result, response){
      if(!error){
        // Got the message
        var message = result[0];
        queueSvc.updateMessage('myqueue', message.messageId, message.popReceipt, 10, {messageText: 'new text'}, function(error, result, response){
          if(!error){
            // Message updated successfully
          }
        });
      }
    });

## <a name="how-to-additional-options-for-dequeuing-messages"></a>Toimintaohje: Lisäasetusten määrittäminen Dequeuing viestit

Voit mukauttaa viestin hakeminen jonon kahdella tavalla:

* `options.numOfMessages`-Noutaa viestien (enintään 32.)
* `options.visibilityTimeout`: Määritä pidentää tai lyhentää invisibility aikakatkaisu.

Seuraavassa esimerkissä **getMessages** menetelmä jotta saat 15 yhden käynnissä. Valitse käsittelee jokaisen viestin käyttämällä silmukan varten. Se myös asettaa invisibility aikakatkaisu viisi minuuttia kaikista viesteistä, funktio palauttaa tällä menetelmällä.

    queueSvc.getMessages('myqueue', {numOfMessages: 15, visibilityTimeout: 5 * 60}, function(error, result, response){
      if(!error){
        // Messages retreived
        for(var index in result){
          // text is available in result[index].messageText
          var message = result[index];
          queueSvc.deleteMessage(queueName, message.messageId, message.popReceipt, function(error, response){
            if(!error){
              // Message deleted
            }
          });
        }
      }
    });

## <a name="how-to-get-the-queue-length"></a>Toimintaohje: Hae jonon pituuden

**GetQueueMetadata** palauttaa metatietoa jonossa, kuten viestejä jonossa olevat lähes määrä.

    queueSvc.getQueueMetadata('myqueue', function(error, result, response){
      if(!error){
        // Queue length is available in result.approximateMessageCount
      }
    });

## <a name="how-to-list-queues"></a>Toimintaohje: Luettelon jonot

Hae olevien luettelo, käytä **listQueuesSegmented**. Voit hakea tietyn etuliitteen mukaan suodatettu luettelo, käyttämällä **listQueuesSegmentedWithPrefix**.

    queueSvc.listQueuesSegmented(null, function(error, result, response){
      if(!error){
        // result.entries contains the list of queues
      }
    });

Jos kaikki olevien ei voi palauttaa, `result.continuationToken` voidaan ensimmäinen parametri on **listQueuesSegmented** tai **listQueuesSegmentedWithPrefix** toisen parametrin lisätuloksia hakemiseen.

## <a name="how-to-delete-a-queue"></a>Toimintaohje: Poista jonossa

Poista jono, ja kaikki sen sisältävät viestit, kutsua jonon objektin **deleteQueue** -menetelmää.

    queueSvc.deleteQueue(queueName, function(error, response){
      if(!error){
        // Queue has been deleted
      }
    });

Poista kaikki viestit jonon poistamatta sen, käytä **clearMessages**.

## <a name="how-to-work-with-shared-access-signatures"></a>Toimintaohje: jaettua käyttöä allekirjoitusten käyttäminen

Jaetun Access allekirjoitukset (SAS) ovat suojatun antaa hajautetun olevien mutta estää tallennustilan tilin nimi tai -avaimet. SAS käytetään usein antamaan oman olevien, kuten salliminen mobiilisovelluksessa lähettämään viestejä rajoitettu käyttöoikeus.

Luotettu sovellus, esimerkiksi pilvipohjaisia palveluja Luo SAS, käyttämällä **QueueService** **generateSharedAccessSignature** ja antaa sen epäluotettavista tai osittain luotettu sovellus. Esimerkiksi mobiilisovelluksessa. Suojaussidokset luodaan ohjatulla käytännön, joka kuvaa alkamis- ja päättymispäivämäärät, jotka Suojaussidokset on voimassa, sekä SAS omistaja myöntänyt käyttöoikeustaso.

Seuraava esimerkki luo uuden jaetun access-käytännön, joka sallii SAS haltijan viestien lisääminen jonossa ja päättyy 100 minuutin ajan sen luomisen jälkeen.

    var startDate = new Date();
    var expiryDate = new Date(startDate);
    expiryDate.setMinutes(startDate.getMinutes() + 100);
    startDate.setMinutes(startDate.getMinutes() - 100);

    var sharedAccessPolicy = {
      AccessPolicy: {
        Permissions: azure.QueueUtilities.SharedAccessPermissions.ADD,
        Start: startDate,
        Expiry: expiryDate
      }
    };

    var queueSAS = queueSvc.generateSharedAccessSignature('myqueue', sharedAccessPolicy);
    var host = queueSvc.host;

Huomaa, että isännän tietojen on oltava myös, kun se on tarpeen, kun SAS haltijan yrittää käyttää jonossa.

Valitse asiakassovellus käyttävä **QueueServiceWithSAS** Suojaussidokset vastaan jonossa toimintojen suorittaminen. Seuraavassa esimerkissä muodostaa yhteyden jonossa ja luo viestin.

    var sharedQueueService = azure.createQueueServiceWithSas(host, queueSAS);
    sharedQueueService.createMessage('myqueue', 'Hello world from SAS!', function(error, result, response){
      if(!error){
        //message added
      }
    });

Koska Suojaussidokset on luotu access Lisää, jos yritys on tehty lukea, päivittää tai poistaa viestit, palautetaan virhe.

### <a name="access-control-lists"></a>Käyttöoikeusluettelot

Voit käyttää myös Access ohjausobjektin luettelo (Käyttöoikeusluettelon) voit määrittää access-käytännön Suojaussidokset. Tämä on kätevä, jos haluat käyttää jonossa, mutta erilaisia käytäntöjen voi kunkin asiakkaan useita asiakkaat.

Käyttöoikeusluettelon on toteutettu matriisin käytön käytännöt käyttäminen kunkin käytännön liittyvä tunnus. Seuraava esimerkki määrittää kahden käytännöt; yksi 'Käyttäjä1' ja 'käyttäjä2' yksi:

    var sharedAccessPolicy = {
      user1: {
        Permissions: azure.QueueUtilities.SharedAccessPermissions.PROCESS,
        Start: startDate,
        Expiry: expiryDate
      },
      user2: {
        Permissions: azure.QueueUtilities.SharedAccessPermissions.ADD,
        Start: startDate,
        Expiry: expiryDate
      }
    };

Seuraava esimerkki noutaa nykyisen Käyttöoikeusluettelon **Oma_jono**ja valitse Lisää uusia käytäntöjä **setQueueAcl**avulla. Tämän menetelmän avulla:

    var extend = require('extend');
    queueSvc.getQueueAcl('myqueue', function(error, result, response) {
      if(!error){
        var newSignedIdentifiers = extend(true, result.signedIdentifiers, sharedAccessPolicy);
        queueSvc.setQueueAcl('myqueue', newSignedIdentifiers, function(error, result, response){
          if(!error){
            // ACL set
          }
        });
      }
    });

Kun Käyttöoikeusluettelon on määritetty, voit luoda SAS, käytännön tunnuksen perusteella. Seuraavassa esimerkissä luodaan uusi SAS 'käyttäjä2' varten:

    queueSAS = queueSvc.generateSharedAccessSignature('myqueue', { Id: 'user2' });

## <a name="next-steps"></a>Seuraavat vaiheet

Nyt kun olet tutustunut jonon tallennustilan perusteet, näistä linkeistä saat lisätietoja tallennustilan monimutkaisia tehtäviä.

-   Siirry [Azure tallennustilan tiimin blogia][].
-   Lisätietoja [Azure-tallennustilan SDK solmun][] säilö GitHub.

  [Azure-tallennustilan SDK solmun]: https://github.com/Azure/azure-storage-node
  [using the REST API]: http://msdn.microsoft.com/library/azure/hh264518.aspx
  [Azure Portal]: https://portal.azure.com
  [Luo Node.js verkkosovellukseen Azure sovelluksen-palvelussa]: ../app-service-web/web-sites-nodejs-develop-deploy-mac.md
  [Node.js Cloud Service with Storage]: ../cloud-services/storage-nodejs-use-table-storage-cloud-service-app.md
  [Azure-taulukosta palveluun node.js web Appissa]: ../app-service-web/storage-nodejs-use-table-storage-web-site.md


  [Queue1]: ./media/storage-nodejs-how-to-use-queues/queue1.png
  [plus-new]: ./media/storage-nodejs-how-to-use-queues/plus-new.png
  [quick-create-storage]: ./media/storage-nodejs-how-to-use-queues/quick-storage.png



  [Muodosta ja ota käyttöön Node.js-sovelluksen Azure pilvipalveluun]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
  [Azure-tallennustilan tiimin blogia]: http://blogs.msdn.com/b/windowsazurestorage/
  [Muodosta ja ota käyttöön Node.js verkkosovellukseen Azure käyttäminen WWW-matriisi]: ../app-service-web/web-sites-nodejs-use-webmatrix.md
