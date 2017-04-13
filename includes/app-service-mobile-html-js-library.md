##<a name="create-client"></a>Asiakkaan yhteyden luominen

Asiakkaan yhteyden luomalla `WindowsAzure.MobileServiceClient` objekti.  Korvaa `appUrl` ja Mobile-sovellus URL-osoite.

```
var client = WindowsAzure.MobileServiceClient(appUrl);
```

##<a name="table-reference"></a>Taulukoiden käsitteleminen

Access- tai Päivitä tiedot, voit luoda viittauksen Taustajärjestelmä taulukon. Korvaa `tableName` taulukon nimi

```
var table = client.getTable(tableName);
```

Kun olet taulukkoviittauksen, voit käyttää muita avoimessa taulukossa:

* [Taulukkoa koskeva kysely](#querying)
  * [Tietojen suodattaminen](#table-filter)
  * [Sivutuksen tietojen välillä](#table-paging)
  * [Tietojen lajitteleminen](#sorting-data)
* [Tietojen lisääminen](#inserting)
* [Tietojen muokkaaminen](#modifying)
* [Tietojen poistaminen](#deleting)

###<a name="querying"></a>Toimintaohje: kyselyn taulukkoviittauksen

Kun taulukkoviittauksen, voit sen avulla kyselyn palvelimeen.  Kyselyjen tehdään "LINQ kaltaisessa" kielellä.
Jos haluat palauttaa kaikki tiedot taulukosta, toimimalla seuraavasti:

```
/**
 * Process the results that are received by a call to table.read()
 *
 * @param {Object} results the results as a pseudo-array
 * @param {int} results.length the length of the results array
 * @param {Object} results[] the individual results
 */
function success(results) {
   var numItemsRead = results.length;

   for (var i = 0 ; i < results.length ; i++) {
       var row = results[i];
       // Each row is an object - the properties are the columns
   }
}

function failure(error) {
    throw new Error('Error loading data: ', error);
}

table
    .read()
    .then(success, failure);
```

Onnistui-funktion nimi on tulokset.   Älä käytä `for (var i in results)` -onnistuu toimi tavalla, joka käytöstä tietojen, joka sisältyy tulosten päälle kun kysely muita toimintoja (kuten `.includeTotalCount()`) käytetään.

Lue lisätietoja kyselysyntaksia [kyselyn objektin ohjeet].

####<a name="table-filter"></a>Tietojen suodattaminen palvelimessa

Voit käyttää `where` lauseen, valitse taulukkoviittaus:

```
table
    .where({ userId: user.userId, complete: false })
    .read()
    .then(success, failure);
```

Voit myös käyttää funktiota, joka suodattaa objekti.  Tässä tapauksessa `this` muuttuja on määritetty nykyisen objektin suodatettavana.  Seuraavat vastaa toiminnaltaan edellisen esimerkin:

```
function filterByUserId(currentUserId) {
    return this.userId === currentUserId && this.complete === false;
}

table
    .where(filterByUserId, user.userId)
    .read()
    .then(success, failure);
```

####<a name="table-paging"></a>Sivutuksen tietojen välillä

Käytä take() ja skip() menetelmiä.  Jos esimerkiksi haluat jakaa taulukon 100 riviä tietueet:

```
var totalCount = 0, pages = 0;

// Step 1 - get the total number of records
table.includeTotalCount().take(0).read(function (results) {
    totalCount = results.totalCount;
    pages = Math.floor(totalCount/100) + 1;
    loadPage(0);
}, failure);

function loadPage(pageNum) {
    let skip = pageNum * 100;
    table.skip(skip).take(100).read(function (results) {
        for (var i = 0 ; i < results.length ; i++) {
            var row = results[i];
            // Process each row
        }
    }
}
```

`.includeTotalCount()` Menetelmää totalCount kentän lisääminen tulokset-objekti.  Tietueet, jotka palautetaan, jos ei ole sivutus käytetään kokonaismäärän täyttää totalCount-kenttään.

Voit käyttää sivujen muuttujan ja jotkin käyttöliittymäpainikkeiden antamaan sivuluettelo; loadPage() avulla voit ladata uusia tietueita jokaiselle sivulle.  Välimuistiin tallentamisen nopeus Accessin tietueeseen, joka on jo ladattu tarkoitus olisi pantava täytäntöön.


####<a name="sorting-data"></a>Toimintaohje: lajiteltu tietojen palauttaminen

Käytä .orderBy() tai .orderByDescending() kyselyn tavoista:

```
table
    .orderBy('name')
    .read()
    .then(success, failure);
```

Lisätietoja kyselyobjektin viitata [kyselyn objektin ohjeiden].

###<a name="inserting"></a>Toimintaohje: tietojen lisääminen

Luo JavaScript-objekti on haluamasi päivämäärän ja kutsu table.insert() asynkronisesti:

```
var newItem = {
    name: 'My Name',
    signupDate: new Date()
};

table
    .insert(newItem)
    .done(function (insertedItem) {
        var id = insertedItem.id;
    }, failure);
```

Onnistuneen kohdistin lisätyn kohteen palautetaan kyseiset kentät, joita tarvitaan synkronointi toimintojen kanssa.  Päivitä omat välimuistin myöhemmät näillä tiedoilla.

Huomautus Azure Mobile sovellusten Node.js Server SDK tukee dynaamisen rakenteen kehittäminen tarkoitukseen.
Dynaamisen rakenteen kyseessä taulukko rakenne on päivitetty suoraan selaimessa, jonka avulla voi lisätä sarakkeita taulukkoon määrittämällä ne insert- tai päivitystoimintoa.  On suositeltavaa dynaamisen rakenteen käytöstä ennen siirtämistä tuotannon sovellusta.

###<a name="modifying"></a>Toimintaohje: tietojen muokkaaminen

Kaltaisilta .insert() menetelmä luotava päivityksen objektin ja kutsua sitten .update().  Update-objekti on oltava tunnus-tietueen päivittäminen kestää – Tämä saadaan tietueen luettaessa tai .insert() soitettaessa.

```
var updateItem = {
    id: '7163bc7a-70b2-4dde-98e9-8818969611bd',
    name: 'My New Name'
};

table
    .update(updateItem)
    .done(function (updatedItem) {
        // You can now update your cached copy
    }, failure);
```

###<a name="deleting"></a>Toimintaohje: tietojen poistaminen

Kutsu tietueen poistaminen .del()-menetelmää.  Välitä tunnus objektiviittaus:

```
table
    .del({ id: '7163bc7a-70b2-4dde-98e9-8818969611bd' })
    .done(function () {
        // Record is now deleted - update your cache
    }, failure);
```
