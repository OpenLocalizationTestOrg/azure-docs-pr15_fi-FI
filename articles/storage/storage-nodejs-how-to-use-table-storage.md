<properties
    pageTitle="Azure-taulukkotallennus-Node.js käyttämisestä | Microsoft Azure"
    description="Voit tallentaa jäsenneltyjen tietojen avulla Azure-taulukkotallennus, NoSQL tietosäilö pilvipalveluun."
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


# <a name="how-to-use-azure-table-storage-from-nodejs"></a>Opi käyttämään from Node.js Azure-taulukkotallennus

[AZURE.INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>Yleiskatsaus

Tässä ohjeaiheessa esitellään, miten suorittaa yleisiä tilanteita, joissa Azure-taulukosta-palvelun avulla Node.js-sovelluksessa.

Tämän artikkelin koodi Esimerkeissä oletetaan, että sinulla on jo Node.js-sovellus. Katso lisätietoja Node.js-sovelluksen luominen Azure näitä aiheita:

- [Luo Node.js verkkosovellukseen Azure sovelluksen-palvelussa](../app-service-web/web-sites-nodejs-develop-deploy-mac.md)
- [Muodosta ja ota käyttöön Node.js verkkosovellukseen Azure WebMatrix käyttäminen](../app-service-web/web-sites-nodejs-use-webmatrix.md)
- [Muodosta ja ota käyttöön Node.js-sovelluksen Azure pilvipalveluun](../cloud-services/cloud-services-nodejs-develop-deploy-app.md) (Windows PowerShellin avulla)


[AZURE.INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]


## <a name="configure-your-application-to-access-azure-storage"></a>Määritä sovellus käyttää Azuren tallennustilaan

Haluat käyttää Azure-tallennustilan, tarvitset Azure-tallennustilan SDK Node.js, joka sisältää helppokäyttöisyys kirjastoja, joissa viestintä tallennustilan muiden palveluiden kanssa.

### <a name="use-node-package-manager-npm-to-install-the-package"></a>Asenna paketti solmu paketin hallinta (NPM) avulla

1.  Käytä esimerkiksi **PowerShell** (Windows), **terminaalissa** (Mac) tai (Unix) **Bash** käyttöliittymä ja Selaa kansioon, jossa sovellus on luotu.

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

Lisää seuraava koodi sovelluksen **server.js** tiedoston alkuun:

    var azure = require('azure-storage');

## <a name="set-up-an-azure-storage-connection"></a>Azuren tallennustilaan-yhteyden määrittäminen

Azure moduulin lukee ympäristömuuttujat AZURE\_TALLENNUSTILAN\_tilin ja AZURE\_TALLENNUSTILAN\_ACCESS\_AVAIMEN tai AZURE\_TALLENNUSTILAN\_yhteyden\_merkkijono, voit muodostaa yhteyden tiliisi Azure tallennustilan tarvittavat tiedot. Jos ympäristössä muuttujia ei määritetä, tilitiedot on määritettävä **TableService**soitettaessa.

Esimerkki määrittäminen ympäristömuuttujat [Azure Portal](https://portal.azure.com) Azure-sivustoon Katso [Node.js web Appin Azure-taulukosta-palvelun avulla].

## <a name="create-a-table"></a>Taulukon luominen

Seuraava koodi luo **TableService** objektin ja käyttää sitä Luo uusi taulukko. Lisää seuraava **server.js**yläosassa.

    var tableSvc = azure.createTableService();

Puhelun **createTableIfNotExists** Luo uusi taulukko samanniminen, jos sitä ei ole jo olemassa. Seuraava esimerkki luo uuden taulukon nimi "mytable", jos sitä ei ole jo:

    tableSvc.createTableIfNotExists('mytable', function(error, result, response){
      if(!error){
        // Table exists or created
      }
    });

`result.created` On `true` Jos uusi taulukko luodaan, ja `false` Jos taulukko on jo olemassa. `response` Sisältää tietoja pyynnön.

### <a name="filters"></a>Suodattimet

Valinnainen suodatus toimintoja voidaan lisätä **TableService**suoritettujen toimintojen. Suodattaminen toimintojen sisällyttää kirjaaminen, yritetään automaattisesti jne. Suodattimet ovat objekteja, jotka toteuttavat allekirjoituksen menetelmää:

    function handle (requestOptions, next)

Kun teet sen esikäsittely pyynnön valitsemalla asetukset, menetelmä täytyy kutsua "seuraavaksi" kulkeva takaisinkutsun seuraavat allekirjoituksen:

    function (returnObject, finalCallback, next)

Tämä takaisinkutsussa ja returnObject (vastausta pyynnön palvelimeen) käsittelyn jälkeen takaisinkutsu on ongelma seuraava, jos sellainen on edelleen käsittelyn muut suodattimet tai kutsua ainoastaan finalCallback muuten Lopeta palvelun käynnistäminen.

Kaksi suodatinta, jotka toteuttavat uudelleen logiikan kuuluvat Node.js, **ExponentialRetryPolicyFilter** ja **LinearRetryPolicyFilter**Azure SDK-paketissa. **TableService** -objekti, joka käyttää **ExponentialRetryPolicyFilter**Luo seuraavasti:

    var retryOperations = new azure.ExponentialRetryPolicyFilter();
    var tableSvc = azure.createTableService().withFilter(retryOperations);

## <a name="add-an-entity-to-a-table"></a>Kohteen lisääminen taulukkoon

Jos haluat lisätä kohteen, luo ensin objekti, joka määrittää kohteen ominaisuudet. Kaikkien kohteiden on oltava **PartitionKey** ja **RowKey**, jotka ovat yksilölliset tunnukset-kohteen.

* Määrittää **PartitionKey** - osio, joka kohde on tallennettu

* **RowKey** - yksilöi kohde osiossa

**PartitionKey** ja **RowKey** on oltava merkkijonoarvoa. Lisätietoja on artikkelissa [taulukon Service-tietomallin perusteet](http://msdn.microsoft.com/library/azure/dd179338.aspx).

Seuraavassa on esimerkki kohteen määrittäminen. Huomaa, että **Määräpäivä** on määritetty **Edm.DateTime**tyyppi. Määrittämällä on valinnainen ja tyypit voi johtaa Jos näin ei ole määritetty.

    var task = {
      PartitionKey: {'_':'hometasks'},
      RowKey: {'_': '1'},
      description: {'_':'take out the trash'},
      dueDate: {'_':new Date(2015, 6, 20), '$':'Edm.DateTime'}
    };

> [AZURE.NOTE] On myös **aikaleiman** kentän kunkin tietueen, joka on määritetty Azure mukaan, kun kohde on lisätty tai päivitetty.

Voit myös luoda kohteiden **entityGenerator** . Seuraavassa esimerkissä luodaan käyttämällä **entityGenerator**saman tehtävä-kohteen.

    var entGen = azure.TableUtilities.entityGenerator;
    var task = {
      PartitionKey: entGen.String('hometasks'),
      RowKey: entGen.String('1'),
      description: entGen.String('take out the trash'),
      dueDate: entGen.DateTime(new Date(Date.UTC(2015, 6, 20))),
    };

Jos haluat lisätä kohteen taulukon, välittää yksikön objektin **insertEntity** -menetelmää.

    tableSvc.insertEntity('mytable',task, function (error, result, response) {
      if(!error){
        // Entity inserted
      }
    });

Jos toiminto onnistuu, `result` sisältää lisätyn tietueen [ETag](http://en.wikipedia.org/wiki/HTTP_ETag) ja `response` sisältää tietoja toiminto.

Esimerkki vastaus:

    { '.metadata': { etag: 'W/"datetime\'2015-02-25T01%3A22%3A22.5Z\'"' } }

> [AZURE.NOTE] Oletusarvon mukaan **insertEntity** ei palauta lisätyn kohteen osana `response` tiedot. Jos aiot suorittaa muita laskutoimituksia tämän kohteen tai haluat tallentaa tiedot, se voi olla hyödyllistä sen osana `result`. Voit tehdä tämän ottamalla **echoContent** seuraavasti:
>
> `tableSvc.insertEntity('mytable', task, {echoContent: true}, function (error, result, response) {...}`

## <a name="update-an-entity"></a>Päivitä kohteen

On olemassa olevan kohteen päivittää usealla eri tavalla:

* **replaceEntity** - päivittää olemassa olevan kohteen korvaamalla se

* **mergeEntity** - päivittää olemassa olevan kohteen yhdistämisessä ominaisuusarvoihin uusien olemassa olevan kohteen

* **insertOrReplaceEntity** - päivittää olemassa olevan kohteen korvaamalla se. Jos kohde ei ole olemassa, uuden lisätään

* **insertOrMergeEntity** - päivittää olemassa olevan kohteen nykyisen yhdistämisessä uusi ominaisuusarvoihin. Jos kohde ei ole olemassa, uuden lisätään

Seuraavassa esimerkissä päivittäminen kohteen **replaceEntity**avulla:

    tableSvc.replaceEntity('mytable', updatedTask, function(error, result, response){
      if(!error) {
        // Entity updated
      }
    });

> [AZURE.NOTE] Oletusarvon mukaan kohteen päivittäminen ei tarkista onko tietojen päivittämisen aiemmin muokattu toisen prosessin käytössä. Tukemaan samanaikaiset päivitykset:
>
> 1. Hae haluamasi objekti on päivitetty ETag. Tämä funktio palauttaa osana `response` yhteisöihin liittyvä toiminto ja voi hakea kautta `response['.metadata'].etag`.
>
> 2. Suoritettaessa kohteen toiminnon päivitys lisää ETag tiedot, jotka on haettu aiemmin uuden kohteen. Esimerkki:
>
>     `entity2['.metadata'].etag = currentEtag;`
>
> 3. Suorita päivitystoiminto. Jos kohde on muokattu, koska esiintymässä sovelluksesi, kuten noudettu ETag-arvo `error` palautetaan, jossa ilmoitetaan, että pyynnössä Päivitä ehto ei täyty.

**ReplaceEntity** ja **mergeEntity**, jos yritys, joka päivitetään ei ole, valitse päivitystoimintoa epäonnistuu. Tämän vuoksi Jos haluat tallentaa kohteen riippumatta siitä, onko se on jo olemassa, käytä **insertOrReplaceEntity** tai **insertOrMergeEntity**.

`result` Onnistuneen päivityksen toiminnot sisältävät **Etag** päivitetyn kohteen.

## <a name="work-with-groups-of-entities"></a>Kohteiden ryhmien käyttäminen

Joskus kannattaa lähettää useita toimintoja yhdessä erä varmistamiseksi atomisia käsittely-palvelin. Voit tehdä tämän **TableBatch** -luokan avulla voit luoda erää ja oletusmäärä toimintojen suorittaminen **TableService** **executeBatch** -menetelmän avulla.

 Seuraavassa esimerkissä on kuvattu kaksi kohteiden erän lähettäminen:

    var task1 = {
      PartitionKey: {'_':'hometasks'},
      RowKey: {'_': '1'},
      description: {'_':'Take out the trash'},
      dueDate: {'_':new Date(2015, 6, 20)}
    };
    var task2 = {
      PartitionKey: {'_':'hometasks'},
      RowKey: {'_': '2'},
      description: {'_':'Wash the dishes'},
      dueDate: {'_':new Date(2015, 6, 20)}
    };

    var batch = new azure.TableBatch();

    batch.insertEntity(task1, {echoContent: true});
    batch.insertEntity(task2, {echoContent: true});

    tableSvc.executeBatch('mytable', batch, function (error, result, response) {
      if(!error) {
        // Batch completed
      }
    });

Onnistuneiden erä toimintoja `result` sisältää tiedot kutakin toimintoa osalta.

### <a name="work-with-batched-operations"></a>Oletusmäärä toimintojen käyttäminen

Toiminnot, jotka on lisätty erän voit tarkastaa tarkastelemalla `operations` ominaisuus. Voit käyttää myös seuraavilla tavoilla voit käyttää toimintoja:

* **Poista** - poistaa kaikki erän-toiminnot

* **getOperations** - toiminnon hakee erän

* **hasOperations** - palauttaa arvon TOSI, jos erä sisältää toiminnot

* poistaa **removeOperations** - toiminto

* **koon** - palauttaa toimintoja osalta

## <a name="retrieve-an-entity-by-key"></a>Hakea kohteen avain

Palauttaa tietyn kohteen **PartitionKey** ja **RowKey**perusteella, käytä **retrieveEntity** -menetelmää.

    tableSvc.retrieveEntity('mytable', 'hometasks', '1', function(error, result, response){
      if(!error){
        // result contains the entity
      }
    });

Kun tämä toiminto on valmis, `result` sisältää kohteen.

## <a name="query-a-set-of-entities"></a>Kyselyn kohteiden määrittäminen

Kyselyn taulukon, kyselylausekkeessa käyttämällä seuraavia lauseet laatia **TableQuery** objektin avulla:

* **Valitse** - kentät kyselyn palauttamat

* **missä** - where-lause

    * **ja** - `and` where-ehto

    * **tai** - `or` where-ehto

* **ylä** - tai Kokoustietojen kohteiden määrä


Seuraava esimerkki luo kysely, joka palauttaa viisi ylimmän PartitionKey kanssa, "hometasks".

    var query = new azure.TableQuery()
      .top(5)
      .where('PartitionKey eq ?', 'hometasks');

**Valitse** ei käytetä, koska kaikki kentät palautetaan. Voit suorittaa taulukon kysely, käyttämällä **queryEntities**. Seuraavassa esimerkissä tämä kysely palauttaa kohteiden 'mytable'.

    tableSvc.queryEntities('mytable',query, null, function(error, result, response) {
      if(!error) {
        // query was successful
      }
    });

Jos onnistuu, `result.entries` sisältää matriisin kohteet, jotka vastaavat kyselyä. Jos kyselyä ei voi palauttaa kaikkia kohteita `result.continuationToken` on muu kuin*null* ja voidaan **queryEntities** kolmannen parametrina hakemiseen enemmän tuloksia. Alkuperäisen kyselyn käyttäminen kolmannen parametrin *null* .

### <a name="query-a-subset-of-entity-properties"></a>Kyselyn alijoukkoa kohteen ominaisuudet

Taulukon kyselyn noutaa todella kentät kohteen.
Tämä vähentää kaistanleveyden ja parantaa kyselyn suorituskykyä, erityisesti niiden suurimpia kohteita. **Valitse** -lauseen ja välittää palautetaan kenttien nimiä. Esimerkiksi seuraava kysely palauttaa vain **kuvaus** ja **Määräpäivä** -kentät.

    var query = new azure.TableQuery()
      .select(['description', 'dueDate'])
      .top(5)
      .where('PartitionKey eq ?', 'hometasks');

## <a name="delete-an-entity"></a>Poista kohde

Voit poistaa kohteen sen osio ja rivi-näppäimen avulla. Tässä esimerkissä **task1** objekti sisältää kohde poistetaan **RowKey** ja **PartitionKey** arvot. Valitse objekti siirretään **deleteEntity** -menetelmää.

    var task = {
      PartitionKey: {'_':'hometasks'},
      RowKey: {'_': '1'}
    };

    tableSvc.deleteEntity('mytable', task, function(error, response){
      if(!error) {
        // Entity deleted
      }
    });

> [AZURE.NOTE] Kannattaa käyttää ETags poistettaessa kohteita, varmista, että toinen prosessi ei ole muokattu kohteen. Lisätietoja ETags käyttämisestä on kohdassa [Päivitä kohteen](#update-an-entity) .

## <a name="delete-a-table"></a>Taulukon poistaminen

Seuraava koodi poistaa taulukon tallennustilan tilin.

    tableSvc.deleteTable('mytable', function(error, response){
        if(!error){
            // Table deleted
        }
    });

Jos et ole varma, onko taulukko on olemassa, käytä **deleteTableIfExists**.

## <a name="use-continuation-tokens"></a>Käytä jatkumista tunnusten

Kun suoritat kyselyn tulokset suuria määriä taulukot, Etsi jatkumista tunnukset. Suurten tietomäärien ole käytettävissä kyselyn, joka saattaa huomaat Jos voit luoda ei tunnista, kun jatkumista tunnuksen ei sisällä tietoja.

Tulokset objektin palautti aikana kyselyt kohteiden joukkoja `continuationToken` ominaisuutta, kun esimerkiksi tunnuksen ei sisällä tietoja. Sitten voit tämän kyselyn suoritettaessa Siirry osion ja taulukon kohteiden välillä.

Kun kysely suoritetaan, continuationToken parametrin voidaan antaa kyselyn objektiesiintymän ja takaisinkutsutoiminto välillä:

```
var nextContinuationToken = null;
dc.table.queryEntities(tableName,
    query,
    nextContinuationToken,
    function (error, results) {
        if (error) throw error;

        // iterate through results.entries with results

        if (results.continuationToken) {
            nextContinuationToken = results.continuationToken;
        }

    });
```

Jos tarkistat `continuationToken` kohdetta, saat näkyviin ominaisuudet kuten `nextPartitionKey`, `nextRowKey` ja `targetLocation`, jota voidaan käyttää käydä läpi kaikki tulokset.

On myös jatkumista otoksen GitHub Azure-tallennustilan Node.js-repo kuluessa. Etsi `examples/samples/continuationsample.js`.

## <a name="work-with-shared-access-signatures"></a>Jaettu käyttö allekirjoitusten käyttäminen

Jaettu käyttö allekirjoitukset (SAS) ovat suojatun antaa hajautetun taulukoiden mutta estää tallennustilan tilin nimi tai -avaimet. SAS käytetään usein antamaan tiedot, kuten salliminen mobiilisovelluksessa kyselyn tietueiden rajoitettu käyttöoikeus.

Luotettu sovellus, esimerkiksi pilvipohjaisia palveluja Luo SAS, käyttämällä **TableService** **generateSharedAccessSignature** ja antaa sen epäluotettavista tai osittain luotettu sovelluksen kuten mobiilisovelluksessa. Suojaussidokset luodaan ohjatulla käytännön, joka kuvaa alkamis- ja päättymispäivämäärät, jotka Suojaussidokset on voimassa, sekä SAS omistaja myöntänyt käyttöoikeustaso.

Seuraava esimerkki luo uuden jaetun access-käytännön, joka sallii SAS haltijan kyselyyn ("r") taulukon ja päättyy 100 minuutin ajan sen luomisen jälkeen.

    var startDate = new Date();
    var expiryDate = new Date(startDate);
    expiryDate.setMinutes(startDate.getMinutes() + 100);
    startDate.setMinutes(startDate.getMinutes() - 100);

    var sharedAccessPolicy = {
      AccessPolicy: {
        Permissions: azure.TableUtilities.SharedAccessPermissions.QUERY,
        Start: startDate,
        Expiry: expiryDate
      },
    };

    var tableSAS = tableSvc.generateSharedAccessSignature('mytable', sharedAccessPolicy);
    var host = tableSvc.host;

Huomaa, että isännän tietojen on oltava myös, kun se on tarpeen, kun SAS haltijan yrittää käyttää taulukon.

Valitse asiakassovellus käyttävä **TableServiceWithSAS** Suojaussidokset voit suorittaa taulukossa. Seuraavassa esimerkissä taulukko yhdistää ja suorittaa kyselyn.

    var sharedTableService = azure.createTableServiceWithSas(host, tableSAS);
    var query = azure.TableQuery()
      .where('PartitionKey eq ?', 'hometasks');

    sharedTableService.queryEntities(query, null, function(error, result, response) {
      if(!error) {
        // result contains the entities
      }
    });

Koska Suojaussidokset luotiin vain kyselyn käyttöoikeudet, jos yritys tehtiin Lisää, Päivitä tai poista kohteita, palautetaan virhe.

### <a name="access-control-lists"></a>Käyttöoikeusluettelot

Voit käyttää myös Access ohjausobjektin luettelo (Käyttöoikeusluettelon) voit määrittää access-käytännön Suojaussidokset. Tämä on kätevä, jos haluat useita Asiakkaat-taulukkoa, mutta erilaisia käytäntöjen voi kunkin asiakkaan.

Käyttöoikeusluettelon on toteutettu matriisin käytön käytännöt käyttäminen kunkin käytännön liittyvä tunnus. Seuraavassa esimerkissä määrittää kahden käytäntöjä, yksi 'Käyttäjä1' ja yksi 'käyttäjä2':

    var sharedAccessPolicy = {
      user1: {
        Permissions: azure.TableUtilities.SharedAccessPermissions.QUERY,
        Start: startDate,
        Expiry: expiryDate
      },
      user2: {
        Permissions: azure.TableUtilities.SharedAccessPermissions.ADD,
        Start: startDate,
        Expiry: expiryDate
      }
    };

Seuraavassa esimerkissä saa nykyisen Käyttöoikeusluettelon **hometasks** taulukon ja lisää uusia käytäntöjä **setTableAcl**avulla. Tämän menetelmän avulla:

    var extend = require('extend');
    tableSvc.getTableAcl('hometasks', function(error, result, response) {
    if(!error){
        var newSignedIdentifiers = extend(true, result.signedIdentifiers, sharedAccessPolicy);
        tableSvc.setTableAcl('hometasks', newSignedIdentifiers, function(error, result, response){
          if(!error){
            // ACL set
          }
        });
      }
    });

Kun Käyttöoikeusluettelon on määritetty, voit luoda SAS, käytännön tunnuksen perusteella. Seuraavassa esimerkissä luodaan uusi SAS 'käyttäjä2' varten:

    tableSAS = tableSvc.generateSharedAccessSignature('hometasks', { Id: 'user2' });

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja on seuraavissa resursseissa.

-   [Azure-tallennustilan tiimin blogia][].
-   [Azure-tallennustilan SDK solmun][] säilöön-GitHub.
-   [Node.js Developer Center](/develop/nodejs/)

  [Azure-tallennustilan SDK solmun]: https://github.com/Azure/azure-storage-node
  [OData.org]: http://www.odata.org/
  [Using the REST API]: http://msdn.microsoft.com/library/azure/hh264518.aspx
  [Azure Portal]: portal.azure.com

  [Node.js Cloud Service]: ../cloud-services-nodejs-develop-deploy-app.md
  [Azure-tallennustilan tiimin blogia]: http://blogs.msdn.com/b/windowsazurestorage/
  [Website with WebMatrix]: ../web-sites-nodejs-use-webmatrix.md
  [Node.js Cloud Service with Storage]: ../storage-nodejs-use-table-storage-cloud-service-app.md
  [Azure-taulukosta palveluun node.js web Appissa]: ../storage-nodejs-use-table-storage-web-site.md
  [Create and deploy a Node.js application to an Azure website]: ../web-sites-nodejs-develop-deploy-mac.md
