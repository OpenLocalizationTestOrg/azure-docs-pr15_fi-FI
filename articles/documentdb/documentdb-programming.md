<properties 
    pageTitle="DocumentDB ohjelmoinnin: tallennettujen toimintosarjojen, tietokannan käynnistimien ja UDF | Microsoft Azure" 
    description="Opettele käyttämään DocumentDB kirjoittaa JavaScript tallennettujen toimintosarjojen, tietokannan Käynnistimet ja käyttäjän määrittämät funktiot (UDF). Pyydä tietokannan programing vinkkejä ja paljon muuta." 
    keywords="Käynnistimien, tallennetun toimintosarjan, tallennetun toimintosarjan, kuten ohjelmaan, sproc, documentdb, azure, Microsoft azure-tietokannan"
    services="documentdb" 
    documentationCenter="" 
    authors="aliuy" 
    manager="jhubbard" 
    editor="mimig"/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/14/2016" 
    ms.author="andrl"/>

# <a name="documentdb-server-side-programming-stored-procedures-database-triggers-and-udfs"></a>DocumentDB palvelinpuolen ohjelmoinnin: tallennettujen toimintosarjojen, tietokannan käynnistimien ja UDF

Lue, miten Azure DocumentDB kielen integroida, JavaScript tapahtumien suorittamisen avulla Kirjoita **tallennettujen toimintosarjojen**, **käynnistimien** ja **käyttäjän määrittämät funktiot (UDF)** grafiikkatiedostomuotoja JavaScript kehittäjät. Näin voit kirjoittaa tietokantaa ohjelman sovelluksen logiikkaa, jotka on toimitettu ja suorittaa suoraan tietokannan tallennustilan osiot 

Suosittelemme seuraavia videon, jossa Andrew'lle Liu on lyhyt esittely DocumentDB's palvelinpuolen ohjelmoinnin tietokantamallin aloittaminen. 

> [AZURE.VIDEO azure-demo-a-quick-intro-to-azure-documentdbs-server-side-javascript]

Palaa sitten artikkelissa, jossa käydään vastaamalla seuraaviin kysymyksiin:  

- Kuinka liiketoimintasääntö kirjoitetaan tallennettu toimintosarja, käynnistimen tai käyttämällä JavaScript UDF?
- Miten DocumentDB takaa lisätään?
- Miten DocumentDB tapahtumat toimii?
- Mitä ovat valmiiksi Käynnistimet ja käynnistää jälkeinen ja kuinka liiketoimintasääntö kirjoitetaan jokin?
- Miten Rekisteröi ja suorittaa tallennetun toimintosarjan, käynnistimen tai UDF käyttämällä HTTP RESTful tavalla?
- Mitä DocumentDB SDK: T ovat käytettävissä luominen ja suorittaminen tallennettujen toimintosarjojen, käynnistimien ja UDF?

## <a name="introduction-to-stored-procedure-and-udf-programming"></a>Tallennetun toimintosarjan ja UDF-ohjelmoinnin esittely

Tämän menetelmän *"* JavaScript-koodia, Nykyaikainen päivänä T-SQL-vapauttaa sovelluskehittäjät-tyypin järjestelmän lähdeluetteloiden ja objektin relaatio yhdistäminen tekniikoita monimutkaisia. Se on myös useita, voit luoda monipuolisia sovelluksia voidaan käyttää sisäisiä eduista:  

-   **Käsitteellisiä logiikan:** JavaScript kuin ohjelmointikieli, korkean tason tarjoaa monipuolisen ja tutun käyttöliittymän, Express liiketoimintalogiikan. Voit suorittaa monimutkaisten järjestys toimintojen lähemmäksi tietoihin.

-   **Käytettäviä jakamattomia tapahtumia:** DocumentDB takaa, että tietokannan toimintoja suorittaa sisällä yhden tallennetun toimintosarjan tai liipaisin ovat atomisia. Näillä oikeuksilla sovelluksen yhdistää yhden erän liittyviä toimintoja niin, että ne kaikki onnistu tai ne onnistuu. 

-   **Suorituskyvyn:** JSON sisäisesti on nyt yhdistetty Javascript kielen tyyppi-järjestelmään ja tallennustilaa DocumentDB basic yksikköä sallii optimointi, kuten kuitata tiedostoja JSON puskurin resurssivarantoon ja että ne käytettävissä tarvittaessa koodi toteutumista määrä. Lisää liittyvä lähetyksen liiketoimintalogiikka tietokannan suorituskykyä hyöty on:
    -   Jonottaminen – kehittäjät voit ryhmitellä toimintoja, kuten Lisää ja Lähetä tiedot joukkona. Verkko-liikenne viive kustannusten ja luoda erilliset tapahtumia store-katseltavan vähentää huomattavasti. 
    -   Vanhat kääntäminen – DocumentDB precompiles tallennettujen toimintosarjojen, käynnistimien ja käyttäjän määrittämät funktiot (UDF) voit välttää kunkin kutsu JavaScript kääntäminen kustannukset. Katseltavan muodostamisen käsitteellisiä logiikan DBCS-koodi on herkkyysmalleja mahdollisimman vähän arvon.
    -   Järjestyksen – monta toimintojen tarve on side-vaikutus ("käynnistimen"), joka liittyy mahdollisesti suorittamalla yhden tai usean toissijainen säilön toimintoja. Lukuun ottamatta jakamattomuus tämä on useita performant siirrettäessä palvelimeen. 
-   **Kapselointi:** Tallennettujen toimintosarjojen voidaan ryhmitellä liiketoimintalogiikan yhdessä paikassa. Tämä on kaksi etuja:
    -   Lisää otetaan-kerroksen raaka tiedot, joka mahdollistaa tietojen architects muodostaen verkkosovelluksistaan itsenäisesti tiedoista päälle. Tämä on erityisen hyödyllistä, kun tiedot ovat rakenne-pienempi vuoksi hauras perustiedot, saatat joutua baked sovellukseen, jos heillä on käsitellä tietoja suoraan.  
    -   Tämä otetaan avulla yritysten suojata tietonsa erikseen useassa komentosarjat käytöltä.  

Luominen ja suorittaminen tietokannan käynnistimien, tallennetun toimintosarjan ja mukautettua kyselyä-operaattoreita tuetaan [REST API](https://msdn.microsoft.com/library/azure/dn781481.aspx), [DocumentDB Studio](https://github.com/mingaliu/DocumentDBStudio/releases)ja monta ympäristöjen, kuten .NET, Node.js ja JavaScript- [asiakasohjelman SDK: T](documentdb-sdk-dotnet.md) .

**Tässä opetusohjelmassa käyttää [Node.js SDK Q Promises kanssa](http://azure.github.io/azure-documentdb-node-q/) ** esitä syntaksista ja tallennettujen toimintosarjojen ja käynnistimien UDF.   

## <a name="stored-procedures"></a>Tallennettujen toimintosarjojen

### <a name="example-write-a-simple-stored-procedure"></a>Esimerkki: Yksinkertaisen tallennetun toimintosarjan kirjoittaminen 
Aloitetaan yksinkertainen tallennetun toimintosarjan, joka palauttaa "Hei-maailman" vastausta.

    var helloWorldStoredProc = {
        id: "helloWorld",
        body: function () {
            var context = getContext();
            var response = context.getResponse();
    
            response.setBody("Hello, World");
        }
    }


Tallennettujen toimintosarjojen kokoelman rekisteröity ja voivat käyttää mitä tahansa asiakirjan ja esitä kyseistä kokoelmaa liite. Seuraavat koodikatkelman esitetään, kuinka voit rekisteröidä helloWorld tallennettu toimintosarja kokoelma. 

    // register the stored procedure
    var createdStoredProcedure;
    client.createStoredProcedureAsync('dbs/testdb/colls/testColl', helloWorldStoredProc)
        .then(function (response) {
            createdStoredProcedure = response.resource;
            console.log("Successfully created stored procedure");
        }, function (error) {
            console.log("Error", error);
        });


Tallennetun toimintosarjan rekisteröitynyt on suoritettava kokoelma ja lue takaisin asiakastietokoneen tulokset. 

    // execute the stored procedure
    client.executeStoredProcedureAsync('dbs/testdb/colls/testColl/sprocs/helloWorld')
        .then(function (response) {
            console.log(response.result); // "Hello, World"
        }, function (err) {
            console.log("Error", error);
        });


Kontekstiobjekti sisältää kaikki toiminnot, jotka voidaan suorittaa DocumentDB tallennustilan käyttöoikeus- ja käyttää pyynnön ja vastauksen objekteja. Tässä tapauksessa on käytetty vastauksen objektin voit määrittää vastauksen, joka on lähetetty takaisin asiakkaaseen tekstiosaan. Lisätietoja on viitata [DocumentDB JavaScript-palvelimen SDK ohjeissa](http://azure.github.io/azure-documentdb-js-server/).  

Anna meidän laajentaa vain tässä esimerkissä ja lisää Lisää tietokannan tallennetun toimintosarjan Aiheeseen liittyviä toimintoja. Tallennettujen toimintosarjojen voit luoda, päivittää, lukea, kysely ja poistaa asiakirjat ja liitteet sivustokokoelman sisällä.    

### <a name="example-write-a-stored-procedure-to-create-a-document"></a>Esimerkki: Kirjoita tallennettu toimintosarja asiakirjan luominen 
Seuraava koodikatkelman esitetään, kuinka voit käsitellä DocumentDB resurssien konteksti-objektin avulla.

    var createDocumentStoredProc = {
        id: "createMyDocument",
        body: function createMyDocument(documentToCreate) {
            var context = getContext();
            var collection = context.getCollection();

            var accepted = collection.createDocument(collection.getSelfLink(),
                  documentToCreate,
                  function (err, documentCreated) {
                      if (err) throw new Error('Error' + err.message);
                      context.getResponse().setBody(documentCreated.id)
                  });
            if (!accepted) return;
        }
    }


Tallennetun tässä ohjeessa kerrotaan kuin syötteen documentToCreate kokoelman luotavan asiakirjan leipätekstiin. Tällaisia toimintoja ovat asynkroninen ja JavaScript-funktion takaisinkutsuja määräytyvät. Takaisinsoitto-funktio on kaksi parametria, yhden tapauksessa epäonnistuu error-objektin ja yksi luotu objekti. Sisällä takaisinkutsun käyttäjät voidaan käsitellä poikkeuksen tai palauttaa virheen. Siltä varalta, ilmenee virhe takaisinkutsun ei ole toimitettu ja DocumentDB runtime ilmoittaa virheestä.   

Edellä olevassa esimerkissä takaisinkutsu ilmoittaa virheestä, jos toiminto epäonnistui. Muussa tapauksessa se asettaa luodun asiakirjan tunnus asiakkaan vastauksen tekstinä. Näin miten tämä tallennettu toimintosarja suoritetaan syöteparametrit kanssa.

    // register the stored procedure
    client.createStoredProcedureAsync('dbs/testdb/colls/testColl', createDocumentStoredProc)
        .then(function (response) {
            var createdStoredProcedure = response.resource;
    
            // run stored procedure to create a document
            var docToCreate = {
                id: "DocFromSproc",
                book: "The Hitchhiker’s Guide to the Galaxy",
                author: "Douglas Adams"
            };
    
            return client.executeStoredProcedureAsync('dbs/testdb/colls/testColl/sprocs/createMyDocument',
                  docToCreate);
        }, function (error) {
            console.log("Error", error);
        })
    .then(function (response) {
        console.log(response); // "DocFromSproc"
    }, function (error) {
        console.log("Error", error);
    });

    
Huomaa, että tämä tallennettu toimintosarja voi muokata asiakirjan laitokset matriisin syötteenä ja luo ne samaan tallennetun toimintosarjan suorittamisen, sen sijaan, että useita etätietokone Luo kullekin niistä yksitellen. Tämä voidaan toteuttaa tehokas joukkona tuontitoiminto DocumentDB (kuvataan jäljempänä tässä opetusohjelmassa) varten.   

Esimerkki, joka on kuvattu osoittanut tallennettujen toimintosarjojen käyttämisestä. Käsiteltävät aiheet Käynnistimet ja käyttäjän määrittämät funktiot (UDF) myöhemmin-opetusohjelman.

## <a name="database-program-transactions"></a>Tietokannan ohjelma tapahtumat
Tapahtuman tyypilliset tietokannan voidaan määrittää loogisen yhtenä yksikkönä tehdyn työn suorittaa toimintoja sarjana. Myyntitapahtumien on **lisätään takaa**. LISÄTÄÄN on tunnettu lyhenne, neljä ominaisuudet - atomisuuden, yhdenmukaisuuden, eristystaso ja kestävyyttä lyhenne.  

Lyhyesti, atomisuuden takaa, että kaikki tapahtuman sisällä työt käsitellään yhtenä yksikkönä missä joko kaikki nämä on vahvistettu tai ei mitään. Yhdenmukaisuuden varmistetaan, että tiedot ovat aina hyvä sisäinen tila yli tapahtumat. Eristystaso takaa, että kaksi tapahtumia häiritä toistensa – yleensä, useimmat kaupalliset järjestelmien antaa eristystaso tasoja, joita voidaan käyttää sovelluksen tarpeiden perusteella. Kestävyyttä varmistaa, että kaikki muutokset, jotka on vahvistettu tietokannan on aina esitä.   

DocumentDB JavaScript isännöidään saman muistitilan kuin tietokannan. Näin ollen pyynnöt tehdyt tallennettujen toimintosarjojen ja käynnistimien suorittaa saman tietokannan istunnon aluetta. Näin voit varmistaa lisätään kaikki toiminnot, jotka ovat yhtä tallennetun toimintosarjan tai käynnistintä DocumentDB. Ota huomioon seuraavat tallennetun toimintosarjan määritys:

    // JavaScript source code
    var exchangeItemsSproc = {
        name: "exchangeItems",
        body: function (playerId1, playerId2) {
            var context = getContext();
            var collection = context.getCollection();
            var response = context.getResponse();
    
            var player1Document, player2Document;
    
            // query for players
            var filterQuery = 'SELECT * FROM Players p where p.id  = "' + playerId1 + '"';
            var accept = collection.queryDocuments(collection.getSelfLink(), filterQuery, {},
                function (err, documents, responseOptions) {
                    if (err) throw new Error("Error" + err.message);
    
                    if (documents.length != 1) throw "Unable to find both names";
                    player1Document = documents[0];
    
                    var filterQuery2 = 'SELECT * FROM Players p where p.id = "' + playerId2 + '"';
                    var accept2 = collection.queryDocuments(collection.getSelfLink(), filterQuery2, {},
                        function (err2, documents2, responseOptions2) {
                            if (err2) throw new Error("Error" + err2.message);
                            if (documents2.length != 1) throw "Unable to find both names";
                            player2Document = documents2[0];
                            swapItems(player1Document, player2Document);
                            return;
                        });
                    if (!accept2) throw "Unable to read player details, abort ";
                });
    
            if (!accept) throw "Unable to read player details, abort ";
    
            // swap the two players’ items
            function swapItems(player1, player2) {
                var player1ItemSave = player1.item;
                player1.item = player2.item;
                player2.item = player1ItemSave;
    
                var accept = collection.replaceDocument(player1._self, player1,
                    function (err, docReplaced) {
                        if (err) throw "Unable to update player 1, abort ";
    
                        var accept2 = collection.replaceDocument(player2._self, player2,
                            function (err2, docReplaced2) {
                                if (err) throw "Unable to update player 2, abort"
                            });
    
                        if (!accept2) throw "Unable to update player 2, abort";
                    });
    
                if (!accept) throw "Unable to update player 1, abort";
            }
        }
    }
    
    // register the stored procedure in Node.js client
    client.createStoredProcedureAsync(collection._self, exchangeItemsSproc)
        .then(function (response) {
            var createdStoredProcedure = response.resource;
        }
    );

Tämä tallennettu toimintosarja käyttää tapahtumia gaming-sovelluksen kaupan kohteiden välillä kaksi pelaajat yhdellä kertaa. Tallennetun toimintosarjan yrittää lukea kahden asiakirjan jokaisen vastaavat Playerin tunnukset välitetty argumenttina. Jos Playerin molemmissa asiakirjoissa on useita, tallennetun toimintosarjan päivittää asiakirjat vaihtaminen niiden kohteiden mukaan. Jos virheitä pitkin haluamallasi tavalla, se ilmoittaa JavaScript-poikkeus, keskeyttää implisiittisesti tapahtuman.

Jos sivustokokoelman tallennetun toimintosarjan rekisteröinyt vastaan on kokoelma single-osioon ja valitse tapahtuma on määritetty kaikki kokoelmassa docuemnts. Jos kokoelma on osioitu, tallennettujen toimintosarjojen suoritetaan tapahtuman laajuus osio-näppäintä. Jokainen tallennetun toimintosarjan suorittamisen jälkeen on sisällettävä vastaavan tapahtuma on suoritettava laajuuden osion avaimen arvon. Lisätietoja on artikkelissa [DocumentDB jakaminen](documentdb-partition-data.md).

### <a name="commit-and-rollback"></a>Vahvista ja palauttaminen
Tapahtumat ovat monitasoisissa ja grafiikkatiedostomuotoja integroitu DocumentDB's programming JavaScript-malli. JavaScript-funktion sisällä kaikki toiminnot ovat automaattisesti Valitse yksittäisen tapahtuman rivittää. Jos JavaScript-koodia toteuttaa ilman poikkeuksen, tietokannan toiminnot ovat vahvistettu. Relaatiotietokannasta "Tapahtuma ALKAA" ja "VAHVISTA tapahtuman"-lauseet ovat implisiittinen DocumentDB.  
 
Jos näkyvissä on poikkeuksen, joka on välitetty komentosarjan, DocumentDB's JavaScript runtime palauttaa koko tapahtumaan. Aiemman esimerkin esitetyllä poikkeuksen lähetys vastaa tehokkaasti "Peruutus tapahtuma" DocumentDB.
 
### <a name="data-consistency"></a>Tietojen yhdenmukaisuuden
Tallennettujen toimintosarjojen ja käynnistimien suoritetaan aina DocumentDB sivustokokoelman ensisijainen se käyttöön. Näin varmistat, että lukujen sisällä tallennettu menettelyt tarjouksen vahva yhdenmukaisuuden. Ensisijaisen tai toissijaisen replikaa voidaan suorittaa kyselyjen käyttäjän määrittämät funktiot, mutta varmistamme mukaiseksi valitsemalla haluamasi se pyydetty yhdenmukaisuuden taso.

## <a name="bounded-execution"></a>Joka suorittaminen
Kaikki DocumentDB toiminnot on suoritettava määritetty Serverissä pyytää aikakatkaisun kesto. Tämä rajoitus koskee myös JavaScript toiminnot (tallennettujen toimintosarjojen, käynnistimien ja käyttäjän määrittämien funktioiden). Toiminto ei suorita määräaikaa kanssa, jos tapahtuma on peruutettu. JavaScript toiminnot on valmis määräajassa tai toteuta jatkumista mukaan mallin erä/jatkaminen suorittamisen.  

Yksinkertaistaa tallennettujen toimintosarjojen ja käsittelemään määräaikoja käynnistimien kehittäminen, jotta kaikki toiminnot-kohdassa sivustokokoelman objektin (esimerkiksi luominen, lukeminen korvaa- ja Poista asiakirjat ja liitteet) palauttaa totuusarvon, joka ilmaisee, onko kyseinen toiminto suoritetaan. Jos tämä arvo on EPÄTOSI, se on maininta määräaika on päättymässä ja että menettelyä on rivityksen määrittäminen suorittamisen.  Toimintojen jonossa edellisen ensimmäisen tekemänsä säilöön-toiminto on varmasti Jos tallennettu toimintosarja on valmis ajoissa ja jono ei lisää pyyntöihin.  

JavaScript toiminnot ovat myös joka resurssin kulutus. DocumentDB varaa siirtonopeuden kokoelman tietokannan tilin valmistellun koon perusteella. Siirtonopeuden ilmoitetaan suorittimen ja muistin IO kulutus kutsutaan pyynnön yksiköt tai RUs normitettu yksikköä. JavaScript toiminnot mahdollisesti käyttää paljon RUs lyhyessä ajassa ja saada korko-vain, jos eikä kokoelman enimmäismäärä on saavutettu. Resurssin tehostettu tallennettujen toimintosarjojen myös ehkä karanteenissa varmistamiseksi käytettävyys alkeistyyppi tietokanta-toimintoa.  

### <a name="example-bulk-importing-data-into-a-database-program"></a>Esimerkki: Joukkosähköposti tietoja tuodaan tietokantaohjelma
Alla on esimerkki tallennetun toimintosarjan, joka on kirjoitettu joukkona-tuo tiedostojen kokoelmaan. Huomaa, miten tallennetun toimintosarjan käsittelee joka suorittamisen valitsemalla Boolean palautusarvon createDocument, ja seurata ja jatka edistymisen yli erissä asiakirjat lisätty kunkin kutsu tallennetun toimintosarjan määrää.

    function bulkImport(docs) {
        var collection = getContext().getCollection();
        var collectionLink = collection.getSelfLink();
    
        // The count of imported docs, also used as current doc index.
        var count = 0;
    
        // Validate input.
        if (!docs) throw new Error("The array is undefined or null.");
    
        var docsLength = docs.length;
        if (docsLength == 0) {
            getContext().getResponse().setBody(0);
        }
    
        // Call the create API to create a document.
        tryCreate(docs[count], callback);
    
        // Note that there are 2 exit conditions:
        // 1) The createDocument request was not accepted. 
        //    In this case the callback will not be called, we just call setBody and we are done.
        // 2) The callback was called docs.length times.
        //    In this case all documents were created and we don’t need to call tryCreate anymore. Just call setBody and we are done.
        function tryCreate(doc, callback) {
            var isAccepted = collection.createDocument(collectionLink, doc, callback);
    
            // If the request was accepted, callback will be called.
            // Otherwise report current count back to the client, 
            // which will call the script again with remaining set of docs.
            if (!isAccepted) getContext().getResponse().setBody(count);
        }
    
        // This is called when collection.createDocument is done in order to process the result.
        function callback(err, doc, options) {
            if (err) throw err;
    
            // One more document has been inserted, increment the count.
            count++;
    
            if (count >= docsLength) {
                // If we created all documents, we are done. Just set the response.
                getContext().getResponse().setBody(count);
            } else {
                // Create next document.
                tryCreate(docs[count], callback);
            }
        }
    }

## <a id="trigger"></a>Tietokannan Käynnistimet
### <a name="database-pre-triggers"></a>Tietokannan vanhat Käynnistimet
DocumentDB on käynnistimien, jotka suoritetaan tai käynnistämä asiakirjan toiminnon. Voit esimerkiksi määrittää vanhat käynnistimen kun luot asiakirjan – vanhat käynnistin suoritetaan, ennen kuin asiakirja on luotu. Seuraavassa on esimerkki siitä, miten vanhat käynnistimien avulla voidaan tarkistaa asiakirjan, joka luodaan ominaisuudet:

    var validateDocumentContentsTrigger = {
        name: "validateDocumentContents",
        body: function validate() {
            var context = getContext();
            var request = context.getRequest();
    
            // document to be created in the current operation
            var documentToCreate = request.getBody();
    
            // validate properties
            if (!("timestamp" in documentToCreate)) {
                var ts = new Date();
                documentToCreate["my timestamp"] = ts.getTime();
            }
    
            // update the document that will be created
            request.setBody(documentToCreate);
        },
        triggerType: TriggerType.Pre,
        triggerOperation: TriggerOperation.Create
    }


Ja vastaava käynnistimen Node.js asiakkaan rekisteröinti koodi:

    // register pre-trigger
    client.createTriggerAsync(collection.self, validateDocumentContentsTrigger)
        .then(function (response) {
            console.log("Created", response.resource);
            var docToCreate = {
                id: "DocWithTrigger",
                event: "Error",
                source: "Network outage"
            };
    
            // run trigger while creating above document 
            var options = { preTriggerInclude: "validateDocumentContents" };
    
            return client.createDocumentAsync(collection.self,
                  docToCreate, options);
        }, function (error) {
            console.log("Error", error);
        })
    .then(function (response) {
        console.log(response.resource); // document with timestamp property added
    }, function (error) {
        console.log("Error", error);
    });


Vanhat käynnistimien ei voi olla syöteparametrit. Pyyntö objektin voidaan käsitellä liittyvä toiminto pyynnön viesti. Tässä vanhat käynnistin suoritetaan asiakirjan luomisen ja pyynnön viestin tekstissä esiintyy asiakirjan luodaan JSON-muodossa.   

Kun käynnistimet on rekisteröity, käyttäjät voivat määrittää toiminnoista, joita voit suorittaa käyttäen. Tämä käynnistin on luotu TriggerOperation.Create, mikä tarkoittaa, että näin ei ole sallittua.

    var options = { preTriggerInclude: "validateDocumentContents" };
    
    client.replaceDocumentAsync(docToReplace.self,
                  newDocBody, options)
    .then(function (response) {
        console.log(response.resource);
    }, function (error) {
        console.log("Error", error);
    });
    
    // Fails, can’t use a create trigger in a replace operation

### <a name="database-post-triggers"></a>Tietokannan jälkeinen Käynnistimet
Jälkeinen käynnistimet vanhat käynnistimien, kuten asiakirjan toiminnon liittyvät ja ei ota syöteparametrit. Ne suoritetaan, **Kun** toiminto on suoritettu ja käyttämään vastausviesti, joka lähetetään asiakkaalle.   

Seuraavassa esimerkissä jälkeinen käynnistimien käytössä:

    var updateMetadataTrigger = {
        name: "updateMetadata",
        body: function updateMetadata() {
            var context = getContext();
            var collection = context.getCollection();
            var response = context.getResponse();
    
            // document that was created
            var createdDocument = response.getBody();
    
            // query for metadata document
            var filterQuery = 'SELECT * FROM root r WHERE r.id = "_metadata"';
            var accept = collection.queryDocuments(collection.getSelfLink(), filterQuery,
                updateMetadataCallback);
            if(!accept) throw "Unable to update metadata, abort";
     
            function updateMetadataCallback(err, documents, responseOptions) {
                if(err) throw new Error("Error" + err.message);
                         if(documents.length != 1) throw 'Unable to find metadata document';
                         
                         var metadataDocument = documents[0];
                         
                         // update metadata
                         metadataDocument.createdDocuments += 1;
                         metadataDocument.createdNames += " " + createdDocument.id;
                         var accept = collection.replaceDocument(metadataDocument._self,
                               metadataDocument, function(err, docReplaced) {
                                      if(err) throw "Unable to update metadata, abort";
                               });
                         if(!accept) throw "Unable to update metadata, abort";
                         return;                    
            }                                                                                           
        },
        triggerType: TriggerType.Post,
        triggerOperation: TriggerOperation.All
    }


Käynnistin voidaan rekisteröidä seuraava näyte esitetyllä tavalla.

    // register post-trigger
    client.createTriggerAsync('dbs/testdb/colls/testColl', updateMetadataTrigger)
        .then(function(createdTrigger) { 
            var docToCreate = { 
                name: "artist_profile_1023",
                artist: "The Band",
                albums: ["Hellujah", "Rotators", "Spinning Top"]
            };
    
            // run trigger while creating above document 
            var options = { postTriggerInclude: "updateMetadata" };
        
            return client.createDocumentAsync(collection.self,
                  docToCreate, options);
        }, function(error) {
            console.log("Error" , error);
        })
    .then(function(response) {
        console.log(response.resource); 
    }, function(error) {
        console.log("Error" , error);
    });


Tämä käynnistin metatietojen asiakirjan kyselyt ja päivittää siihen äskettäin luodun asiakirjan tietoja.  

On tärkeää, Huomaa, että kuitenkaan on DocumentDB käynnistimien **tapahtumien** suorittamisen. Tämä jälkeinen käynnistin suorittaa saman tapahtuman kuin alkuperäisessä asiakirjassa luomisen osana. Sen vuoksi, jos olemme palauttaa poikkeuksen jälkeinen käynnistimestä (esimerkiksi jos emme voi päivittää metatietojen asiakirja), koko tapahtumaan epäonnistuu ja peruutettava. Tiedostoa ei ole luodaan ja poikkeuksen palautetaan.  

##<a id="udf"></a>Käyttäjän määrittämät funktiot
Käyttäjän määrittämät funktiot (UDF) käytetään DocumentDB SQL-kyselyn kieli kieliopin ja toteuttaa mukautetun liiketoimintalogiikan. Ne voidaan kutsua ainoastaan sisällä kyselyt. Ne ei ole yhteydessä-objekti ja on tarkoitettu käytettäväksi vain Laske JavaScript. Tämän vuoksi UDF voidaan suorittaa toissijaisen replikoiden DocumentDB-palvelun.  
 
Seuraava näyte Luo UDF laskemiseen veroa perusteella eri tulot hakasulkeita korvaukset ja käyttää sitä sitten sisällä kyselyn Etsi kaikki henkilöt, joilla on enemmän kuin 20 000 dollaria maksettu verot.

    var taxUdf = {
        name: "tax",
        body: function tax(income) {
    
            if(income == undefined) 
                throw 'no input';
    
            if (income < 1000) 
                return income * 0.1;
            else if (income < 10000) 
                return income * 0.2;
            else
                return income * 0.4;
        }
    }


UDF myöhemmin voidaan kyselyt, seuraavassa esimerkissä:

    // register UDF
    client.createUserDefinedFunctionAsync('dbs/testdb/colls/testColl', taxUdf)
        .then(function(response) { 
            console.log("Created", response.resource);
    
            var query = 'SELECT * FROM TaxPayers t WHERE udf.tax(t.income) > 20000'; 
            return client.queryDocuments('dbs/testdb/colls/testColl',
                   query).toArrayAsync();
        }, function(error) {
            console.log("Error" , error);
        })
    .then(function(response) {
        var documents = response.feed;
        console.log(response.resource); 
    }, function(error) {
        console.log("Error" , error);
    });

## <a name="javascript-language-integrated-query-api"></a>JavaScript kielen integroitu kyselyn Ohjelmointirajapinta
Lisäksi kyselyjen DocumentDB's SQL kieliopin antamista palvelinpuolen SDK avulla voit suorittaa optimoitu kyselyjen sujuva JavaScript-liittymän ilman mitään tuntemus SQL. JavaScript-kyselyn API avulla voit muodostaa ohjelmallisesti välittämällä predikaatin Funktiot kyselyjä chainable funktion kyselyjen puhelujen syntaksin, joka on tuttu ECMAScript5 on taulukko, jossa ja Suositut JavaScript-kirjastoja, kuten lodash. Kyselyjen ovat jäsentää, joka suoritetaan tehokkaammin DocumentDB's indeksien JavaScript-runtime.

> [AZURE.NOTE]`__` (double-alaviiva) on tunnusnimi `getContext().getCollection()`.
> <br/>
> Toisin sanoen voit käyttää `__` tai `getContext().getCollection()` käyttää JavaScript-kyselyn API.

Tuetut toiminnot ovat seuraavat:
<ul>
<li>
<b>Chain().... arvo ([takaisinsoitto] [, asetukset])</b>
<ul>
<li>
Ketjutetut kutsua, joka on lopetettava alkaa value().
</li>
</ul>
</li>
<li>
<b>Suodata (predicateFunction [-asetukset] [, takaisinsoitto])</b>
<ul>
<li>
Suodattaa syötettä predikaatin funktiota, joka palauttaa arvon TOSI tai EPÄTOSI, jotta suodatetaan/ulos syötteen asiakirjojen tuloksena on joukko. Tämä toimii samalla tavalla kuin SQL WHERE-lause.
</li>
</ul>
</li>
<li>
<b>Yhdistä (transformationFunction [-asetukset] [, takaisinsoitto])</b>
<ul>
<li>
Koskee ennuste, joka on antanut muunnos-funktiota, joka yhdistää syötteen kunkin kohteen JavaScript-objektin tai arvon. Tämä toimii samalla tavalla kuin SQL SELECT-lauseessa.
</li>
</ul>
</li>
<li>
<b>pluck ([propertyName] [, asetukset] [, takaisinsoitto])</b>
<ul>
<li>
Tämä on pikakuvakkeen Map, joka poimii syötteen kohteille yhden ominaisuuden arvon.
</li>
</ul>
</li>
<li>
<b>Litistä ([isShallow] [, asetukset] [, takaisinsoitto])</b>
<ul>
<li>
Yhdistää ja matriiseja yksittäisen matriisin kunkin syötteen kohteesta objektiksi. Tämä toimii samalla tavalla kuin SelectMany LINQ.
</li>
</ul>
</li>
<li>
<b>sortBy ([lause] [, asetukset] [, takaisinsoitto])</b>
<ul>
<li>
Lajitteleminen nousevassa järjestyksessä käyttämällä annetun lause syötteen asiakirjan virta tiedostoissa uusi Asiakirjajoukon tuottavaa. Tämä toimii samalla tavalla kuin SQL ORDER BY-lause.
</li>
</ul>
</li>
<li>
<b>sortByDescending ([lause] [, asetukset] [, takaisinsoitto])</b>
<ul>
<li>
Uusi Asiakirjajoukon tuottaa lajittelemalla syötteen asiakirjan virta laskevassa järjestyksessä käyttämällä annetun lause asiakirjoissa. Tämä toimii samalla tavalla kuin x DESC ORDER BY-lause SQL.
</li>
</ul>
</li>
</ul>


Kun lause ja/tai valitsin funktioiden sisällä, seuraavat JavaScript-rakenteita Hae automaattisesti optimoitu käytettäväksi suoraan DocumentDB indeksien:

* Yksinkertainen operaattorit: = + - * / % | ^ &amp; == != === !=== &lt; &gt; &lt;= &gt;= || &amp;&amp; &lt;&lt; &gt;&gt; &gt;&gt;&gt;! ~
* Literaalien, mukaan lukien literal objekti: {}
* var-palautus

Seuraavat JavaScript-rakenteita ei Hae optimoida DocumentDB indeksien:

* Työnkulku määrittää (esimerkiksi jos, kun)
* Funktiokutsut

Lisätietoja on artikkelissa Microsoftin [Palvelinpuolen JSDocs](http://azure.github.io/azure-documentdb-js-server/).

### <a name="example-write-a-stored-procedure-using-the-javascript-query-api"></a>Esimerkki: Kirjoita tallennetun toimintosarjan API JavaScript-kyselyn avulla

Seuraava koodi malli on esimerkki siitä, miten kyselyn JavaScript-Ohjelmointirajapinnan voidaan tallennetun toimintosarjan kontekstissa. Tallennetun toimintosarjan asiakirjan antamien syöteparametria, Lisää ja päivitysten metatietojen asiakirjan avulla `__.filter()` minSize, maxSize ja virhettä syötteen asiakirjan koko-ominaisuuden mukaan-menetelmä.

    /**
     * Insert actual doc and update metadata doc: minSize, maxSize, totalSize based on doc.size.
     */
    function insertDocumentAndUpdateMetadata(doc) {
      // HTTP error codes sent to our callback funciton by DocDB server.
      var ErrorCode = {
        RETRY_WITH: 449,
      }

      var isAccepted = __.createDocument(__.getSelfLink(), doc, {}, function(err, doc, options) {
        if (err) throw err;

        // Check the doc (ignore docs with invalid/zero size and metaDoc itself) and call updateMetadata.
        if (!doc.isMetadata && doc.size > 0) {
          // Get the meta document. We keep it in the same collection. it's the only doc that has .isMetadata = true.
          var result = __.filter(function(x) {
            return x.isMetadata === true
          }, function(err, feed, options) {
            if (err) throw err;

            // We assume that metadata doc was pre-created and must exist when this script is called.
            if (!feed || !feed.length) throw new Error("Failed to find the metadata document.");

            // The metadata document.
            var metaDoc = feed[0];

            // Update metaDoc.minSize:
            // for 1st document use doc.Size, for all the rest see if it's less than last min.
            if (metaDoc.minSize == 0) metaDoc.minSize = doc.size;
            else metaDoc.minSize = Math.min(metaDoc.minSize, doc.size);

            // Update metaDoc.maxSize.
            metaDoc.maxSize = Math.max(metaDoc.maxSize, doc.size);

            // Update metaDoc.totalSize.
            metaDoc.totalSize += doc.size;

            // Update/replace the metadata document in the store.
            var isAccepted = __.replaceDocument(metaDoc._self, metaDoc, function(err) {
              if (err) throw err;
              // Note: in case concurrent updates causes conflict with ErrorCode.RETRY_WITH, we can't read the meta again 
              //       and update again because due to Snapshot isolation we will read same exact version (we are in same transaction).
              //       We have to take care of that on the client side.
            });
            if (!isAccepted) throw new Error("replaceDocument(metaDoc) returned false.");
          });
          if (!result.isAccepted) throw new Error("filter for metaDoc returned false.");
        }
      });
      if (!isAccepted) throw new Error("createDocument(actual doc) returned false.");
    }

## <a name="sql-to-javascript-cheat-sheet"></a>SQL Javascript huijata taulukkoon
Seuraavassa taulukossa esitellään eri SQL-kyselyjä ja vastaavan JavaScript-kyselyt.

SQL-kyselyjen avulla asiakirjaan ominaisuuden näppäimet (kuten `doc.id`) kirjainkoko on merkitsevä.

<br/>
<table border="1" width="100%">
<colgroup>
<col span="1" style="width: 40%;">
<col span="1" style="width: 40%;">
<col span="1" style="width: 20%;">
</colgroup>
<tbody>
<tr>
<th>SQL</th>
<th>JavaScript-kyselyn Ohjelmointirajapinta</th>
<th>Tiedot</th>
</tr>
<tr>
<td>
<pre>
Valitse *-tiedostot
</pre>
</td>
<td>
<pre>
__.Map(Function(doc) {palauttaa tiedostoikkunan;});
</pre>
</td>
<td>Kaikki asiakirjat (sivut numeroitu jatkumista tunnuksen kanssa) kuin tuloksena on.</td>
</tr>
<tr>
<td>
<pre>
Valitse docs.id, docs.message kuin viesti, docs.actions FROM asiakirjoja
</pre>
</td>
<td>
<pre>
__.Map(Function(doc) {palauttaa {id: doc.id, viesti: doc.message, toiminnot: doc.actions};});
</pre>
</td>
<td>Projektien tunnus, viestin (aliaksen viesti) ja kaikki tiedostot-toiminnon.</td>
</tr>
<tr>
<td>
<pre>
Valitse *-asiakirjoja missä docs.id="X998_Y998"
</pre>
</td>
<td>
<pre>
__.Filter(Function(doc) {palauttaa doc.id === "X998_Y998"});
</pre>
</td>
<td>Kyselyt lause sisältävien asiakirjojen: id = "X998_Y998".</td>
</tr>
<tr>
<td>
<pre>
Valitse *-asiakirjoja missä ARRAY_CONTAINS(docs. Tunnisteita, 123)
</pre>
</td>
<td>
<pre>
__.Filter(Function(x) {palauttaa x.Tags & & x.Tags.indexOf(123) > -1,});
</pre>
</td>
<td>Kyselyjä ja asiakirjoja, joissa on tunnisteet-ominaisuutta ja tunnisteita on matriisikaava, joka sisältää arvon 123.</td>
</tr>
<tr>
<td>
<pre>
Valitse docs.id, docs.message kuin viesti FROM asiakirjoja missä docs.id="X998_Y998"
</pre>
</td>
<td>
<pre>
__.Chain() .filter(function(doc) {palauttaa doc.id === "X998_Y998"}) .map(function(doc) {palauttaa {id: doc.id, viesti: doc.message};}) .value();
</pre>
</td>
<td>Kyselyjen asiakirjoihin, joissa lause id = "X998_Y998" ja sitten projektit tunnus ja viestin (aliaksen viesti).</td>
</tr>
<tr>
<td>
<pre>
Valitse arvo tunnisteen FROM docs liity tunnisteen asiakirjoja. Tunnisteiden ORDER BY docs._ts
</pre>
</td>
<td>
<pre>
__.Chain() .filter(function(doc) {palaa asiakirjaan. Tunnisteiden & & Array.isArray (asiakirjaan. Tunnisteiden); {palautuksen doc._ts;}}), .sortBy(function(doc)) .pluck("Tags") .flatten() .value()
</pre>
</td>
<td>Asiakirjat, jotka on taulukko-ominaisuutta, tunnisteita, ja tuloksena olevat asiakirjat lajittelussa _ts aikaleima system-ominaisuus ja sitten projektit + objektiksi tunnisteet-matriisin suodattimet.</td>
</tr>
</tbody>
</table>

## <a name="runtime-support"></a>Runtimen tuki
[DocumentDB JavaScript-palvelinpuolen SDK](http://azure.github.io/azure-documentdb-js-server/) tuki mainstream JavaScript kieliominaisuuksien kuin standardoitu mukaan [ECMA 262](http://www.ecma-international.org/publications/standards/Ecma-262.htm)kaiken irti.

### <a name="security"></a>Tietoturva
JavaScript tallennettujen toimintosarjojen ja käynnistimien ovat eristetyn niin, että yhden komentosarjan tehosteita ei paljastaa toiseen avaamatta tilannevedoksen tapahtuman eristystaso tietokannan tasolla. Runtime-ympäristöissä jaetut mutta puhdistaa kontekstin kunkin asennuksen jälkeen. Näin ollen ne ovat taata turvallisten tahattomia puoli tehosteet, päähän toisistaan.

### <a name="pre-compilation"></a>Vanhat kääntäminen
Tallennettujen toimintosarjojen, käynnistimien ja UDF ovat siten vältät kääntäminen kustannukset kunkin komentosarjan kutsuminen milloin implisiittisesti esikoostettu tavuinen koodi-muotoon. Tällä varmistetaan, että ohjelmarakennekaaviossa tallennettujen toimintosarjojen ovat nopea ja pienen vaatiman tallennustilan.

## <a name="client-sdk-support"></a>Asiakkaan SDK-tuki
Lisäksi [Node.js](documentdb-sdk-node.md) asiakkaan DocumentDB tukee [.NET](documentdb-sdk-dotnet.md)-, [Java](documentdb-sdk-java.md)-, [JavaScript](http://azure.github.io/azure-documentdb-js/)- ja [Python SDK: T](documentdb-sdk-python.md). Tallennettujen toimintosarjojen ja käynnistimien UDF voidaan luoda ja suorittaa näitä SDK: T sekä avulla. Seuraavassa esimerkissä esitetään, miten voit luoda ja suorittaa tallennetun toimintosarjan .NET-asiakas. Huomautus kuinka .NET-tyypit välitetty tallennetun toimintosarjan kuin JSON ja luettava soluryhmä.

    var markAntiquesSproc = new StoredProcedure
    {
        Id = "ValidateDocumentAge",
        Body = @"
                function(docToCreate, antiqueYear) {
                    var collection = getContext().getCollection();    
                    var response = getContext().getResponse();    
    
                    if(docToCreate.Year != undefined && docToCreate.Year < antiqueYear){
                        docToCreate.antique = true;
                    }
    
                    collection.createDocument(collection.getSelfLink(), docToCreate, {}, 
                        function(err, docCreated, options) { 
                            if(err) throw new Error('Error while creating document: ' + err.message);                              
                            if(options.maxCollectionSizeInMb == 0) throw 'max collection size not found'; 
                            response.setBody(docCreated);
                    });
            }"
    };
    
    // register stored procedure
    StoredProcedure createdStoredProcedure = await client.CreateStoredProcedureAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"), markAntiquesSproc);
    dynamic document = new Document() { Id = "Borges_112" };
    document.Title = "Aleph";
    document.Year = 1949;
    
    // execute stored procedure
    Document createdDocument = await client.ExecuteStoredProcedureAsync<Document>(UriFactory.CreateStoredProcedureUri("db", "coll", "sproc"), document, 1920);


Tässä esimerkissä esitetään, kuinka vanhat käynnistimen luominen ja asiakirjan luominen käytössä käynnistin [.NET SDK](https://msdn.microsoft.com/library/azure/dn948556.aspx) avulla. 

    Trigger preTrigger = new Trigger()
    {
        Id = "CapitalizeName",
        Body = @"function() {
            var item = getContext().getRequest().getBody();
            item.id = item.id.toUpperCase();
            getContext().getRequest().setBody(item);
        }",
        TriggerOperation = TriggerOperation.Create,
        TriggerType = TriggerType.Pre
    };
    
    Document createdItem = await client.CreateDocumentAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"), new Document { Id = "documentdb" },
        new RequestOptions
        {
            PreTriggerInclude = new List<string> { "CapitalizeName" },
        });


Ja seuraavassa esimerkissä esitetään, miten voit luoda käyttäjän määrittämiä funktioita (UDF) ja käyttää sitä [DocumentDB SQL-kysely](documentdb-sql-query.md).

    UserDefinedFunction function = new UserDefinedFunction()
    {
        Id = "LOWER",
        Body = @"function(input) 
        {
            return input.toLowerCase();
        }"
    };
    
    foreach (Book book in client.CreateDocumentQuery(UriFactory.CreateDocumentCollectionUri("db", "coll"),
        "SELECT * FROM Books b WHERE udf.LOWER(b.Title) = 'war and peace'"))
    {
        Console.WriteLine("Read {0} from query", book);
    }

## <a name="rest-api"></a>REST-OHJELMOINTIRAJAPINNALLA
Kaikki DocumentDB toimintoja voi käyttää RESTful tavalla. Tallennettujen toimintosarjojen, käynnistimien ja käyttäjän määrittämien funktioiden voi rekisteröidä kokoelma-kohdassa HTTP POST avulla. Seuraavassa on esimerkki siitä, miten voit rekisteröidä tallennetun toimintosarjan:

    POST https://<url>/sprocs/ HTTP/1.1
    authorization: <<auth>>
    x-ms-date: Thu, 07 Aug 2014 03:43:10 GMT
    
    
    var x = {
      "name": "createAndAddProperty",
      "body": function (docToCreate, addedPropertyName, addedPropertyValue) {
                var collectionManager = getContext().getCollection();
                collectionManager.createDocument(
                    collectionManager.getSelfLink(),
                    docToCreate,
                    function(err, docCreated) {
                      if(err) throw new Error('Error:  ' + err.message);
                      docCreated[addedPropertyName] = addedPropertyValue;
                      getContext().getResponse().setBody(docCreated);
                    });
            }
    }


Tallennettu toimintosarja on rekisteröity suorittamalla Kirjaa pyyntö vastaan URI hidastustekniikkaa/testitietokanta/colls/testColl/sprocs runko, joka sisältää tallennetun toimintosarjan luominen kanssa. Käynnistimien ja UDF voidaan rekisteröidä samalla tavalla kuin lähettämällä VIESTIIN/Käynnistimet ja /udfs vastaan.
Tallennettu toimintosarja can, sitten suoritettava kirjaa pyynnön vastaan sen resurssin linkkiä:

    POST https://<url>/sprocs/<sproc> HTTP/1.1
    authorization: <<auth>>
    x-ms-date: Thu, 07 Aug 2014 03:43:20 GMT
    
    
    [ { "name": "TestDocument", "book": "Autumn of the Patriarch"}, "Price", 200 ]


Tässä kohdassa tallennetun toimintosarjan syöte välitetään pyynnön tekstissä. Huomaa, että syöte välitetään JSON matriisina, syöteparametrit. Tallennetun toimintosarjan kestää ensimmäisellä syöte, joka on vastaus leipäteksti-tiedostoiksi. Saamme vastaus on seuraavasti:

    HTTP/1.1 200 OK
     
    { 
      name: 'TestDocument',
      book: ‘Autumn of the Patriarch’,
      id: ‘V7tQANV3rAkDAAAAAAAAAA==‘,
      ts: 1407830727,
      self: ‘dbs/V7tQAA==/colls/V7tQANV3rAk=/docs/V7tQANV3rAkDAAAAAAAAAA==/’,
      etag: ‘6c006596-0000-0000-0000-53e9cac70000’,
      attachments: ‘attachments/’,
      Price: 200
    }


Käynnistimien, toisin kuin tallennettujen toimintosarjojen ei voi suorittaa suoraan. Sen sijaan ne suoritetaan toiminnon asiakirjan osana. Emme voi määrittää käynnistimet toimimaan käyttäen HTTP-otsikot. Seuraavassa on pyyntö asiakirjan luominen.

    POST https://<url>/docs/ HTTP/1.1
    authorization: <<auth>>
    x-ms-date: Thu, 07 Aug 2014 03:43:10 GMT
    x-ms-documentdb-pre-trigger-include: validateDocumentContents 
    x-ms-documentdb-post-trigger-include: bookCreationPostTrigger
    
    
    {
       "name": "newDocument",
       “title”: “The Wizard of Oz”,
       “author”: “Frank Baum”,
       “pages”: 92
    }


Tähän pyynnön toimimaan vanhat käynnistin on määritetty x-ms-documentdb-pre-trigger-include otsikko. Vastaavasti jälkeisen käynnistimet esitetään x-ms-documentdb-post-trigger-include otsikko. Huomaa, että sekä vanhat ja jälkeinen käynnistimet voidaan määrittää tietyn pyynnön.

## <a name="sample-code"></a>Esimerkki koodi

Löydät lisää palvelinpuolen koodiesimerkkejä (mukaan lukien [joukkona Poista](https://github.com/Azure/azure-documentdb-js-server/tree/master/samples/stored-procedures/bulkDelete.js)ja [Päivitä](https://github.com/Azure/azure-documentdb-js-server/tree/master/samples/stored-procedures/update.js)) meidän [Github säilöön](https://github.com/Azure/azure-documentdb-js-server/tree/master/samples).

Haluatko jakaa awesome toimintosarjan? Lähetä meille salaus puretaan-pyyntö! 

## <a name="next-steps"></a>Seuraavat vaiheet

Kun olet vähintään yksi tallennettujen toimintosarjojen, käynnistimien ja käyttäjän määrittämät funktiot, jotka on luotu, voit ladata ne ja tarkastella niitä Azure-portaalissa komentosarjan Resurssienhallinnan avulla. Lisätietoja on artikkelissa [näkymän tallennettujen toimintosarjojen, käynnistimien, ja käyttäjän määrittämien funktioiden DocumentDB komentosarjan Resurssienhallinnan avulla](documentdb-view-scripts.md).

Kannattaa myös tutustua seuraavat viitteet ja resurssien lisätietoja DocumentDB palvelinpuolen ohjelmoinnin path hyötyä:

- [Azure DocumentDB SDK: T](https://msdn.microsoft.com/library/azure/dn781482.aspx)
- [DocumentDB Studio](https://github.com/mingaliu/DocumentDBStudio/releases)
- [JSON](http://www.json.org/) 
- [JavaScript-ECMA 262](http://www.ecma-international.org/publications/standards/Ecma-262.htm)
- [JavaScript – JSON tyyppi järjestelmän](http://www.json.org/js.html) 
- [Tietokannan suojaaminen ja kannettavat laajennettavuus](http://dl.acm.org/citation.cfm?id=276339) 
- [Palvelun aloittaminen tietokanta-arkkitehtuuri](http://dl.acm.org/citation.cfm?id=1066267&coll=Portal&dl=GUIDE) 
- [Microsoft SQL Serverin .NET-suorituksenaikainen isännöinnin](http://dl.acm.org/citation.cfm?id=1007669)
