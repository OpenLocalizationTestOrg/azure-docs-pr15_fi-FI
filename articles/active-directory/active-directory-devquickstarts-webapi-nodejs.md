<properties
    pageTitle="Azure AD-NodeJS aloittaminen | Microsoft Azure"
    description="Miten voit luoda Node.js REST Web API-Liittymän, integroituu Azure AD todennusta varten."
    services="active-directory"
    documentationCenter="nodejs"
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="brandwe"/>

# <a name="getting-started-with-web-api-for-node"></a>Solmun verkko-Ohjelmointirajapinnan aloittaminen

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

**Passport** on todennus middleware Node.js varten. Erittäin joustavia ja moduulit, Passport voi unobtrusively vetää ja pudottaa missään Express sijaitsevaan tai Resitify verkkosovellus. Strategioita kattavaa tue todennusta käyttäjänimi ja salasana, Facebook tai Twitter avulla. Microsoft on kehittänyt strategia Microsoft Azure Active Directory. Microsoft tämän-moduulin asentaminen ja lisää sitten Microsoft Azure Active Directory `passport-azure-ad` laajennus.

Jotta voit tehdä tämän, sinun on:

1. Voit rekisteröidä sovelluksen Azure AD
2. Määrittää sovelluksen Passportin käyttäjän azure ad-palveluissa laajennus.
3. Soita, Tee luettelo Web Ohjelmointirajapinnan asiakassovellus määrittäminen

Tässä opetusohjelmassa koodi määritetään [GitHub](https://github.com/Azure-Samples/active-directory-node-webapi).

> [AZURE.NOTE] Tässä artikkelissa ei käsitellä ottamisesta käyttöön, kirjaudu sisään, kirjautuminen ja Azure AD B2C profiilin hallinta.  Se ohjeessa on puheluja web API, kun käyttäjä on jo todennettu.  Jos et ole jo, [miten voit integroida Azure Active Directory-asiakirjasta](./develop/active-directory-how-to-integrate.md) lisätietoja Azure Active Directory perusteet olisi aloitetaan.


Olemme olet julkaissut kaikki tämän käynnissä olevassa esimerkissä GitHub MIT-käyttöoikeudet-kohdassa lähdekoodi, niin vapaasti Kloonaa (tai sitäkin parempi rinnakkainen!) ja antaa palautetta ja tuoda pyynnöt.

## <a name="about-nodejs-modules"></a>Tietoja Node.js moduulit

Olemme käyttää Node.js moduulit tämän vaiheittaisen kuvauksen suorittamiseksi. Moduulit ovat ladattavissa JavaScript-paketteja, jotka tiettyjen toimintojen tarjoamista sovelluksen. Yleensä asentaa moduulit komentorivin Node.js NPM-työkalun avulla NPM asennuksen hakemistossa, mutta jotkin moduulit, kuten HTTP-moduuli sisältyvät core Node.js paketin.
Asennettujen moduulit tallennetaan node_modules hakemiston Node.js asennuksen hakemistossa ylimmällä tasolla. Kunkin moduulin node_modules hakemistossa ylläpitää omassa node_modules-kansio, joka sisältää kaikki moduulit, se on riippuvainen ja tarvittavat kunkin moduulin on node_modules-kansio. Tämä rekursiivinen kansiorakenne edustaa riippuvuuden ketju.

Tämä riippuvuus ketju rakenne tuloksena on suurempi sovelluksen vaatiman-tallennustilan, mutta se takaa, että kaikki riippuvuudet täyttyvät ja että käytetään kehitteillä moduulit version käytetään myös tuotannon. Tämä on tuotannon-sovelluksen toiminta aiempaa helpommin ennakoitavia ja estää versiotietojen ongelmia, jotka saattavat vaikuttaa käyttäjiin.

## <a name="1-register-a-azure-ad-tenant"></a>1. rekisteröidä Azure AD-vuokraajan

Jos haluat käyttää tässä esimerkissä on Azure Active Directory-vuokraajan. Jos et ole varma mitä palvelutili on ja miten saat yksi, katso [kuinka saat Azure AD vuokraaja](active-directory-howto-tenant.md).

## <a name="2-create-an-application"></a>2.-sovelluksen luominen

Kun haluat luoda sovelluksen hakemistossa, jotka antavat Azure AD sen pitää yhteyttä suojatusti sovelluksen tarvitsemat tiedot.  Asiakas-sovellus ja verkko-Ohjelmointirajapinnan edustaa yksi **Sovellustunnus** tässä tapauksessa jälkeen ne muodostavat yhteen looginen-sovellukseen.  Voit luoda sovelluksen noudattamalla [näitä ohjeita](active-directory-how-applications-are-added.md). Jos olet muodostamassa Liiketoiminta, sovellus [nämä lisäohjeita voi olla hyötyä](active-directory-applications-guiding-developers-for-lob-applications.md).

Muista:

- Kirjaudu sisään Azure hallinta-portaalin.
- Valitse vasemmalla olevasta siirtymisruudussa **Active Directorysta**.
- Valitse missä haluat rekisteröidä sovellus vuokraaja.
- Valitse **sovellukset** -välilehti ja valitse Lisää ala-laatikon.
- Seuraa ohjeita ja luo uusi **Web-sovelluksen ja/tai WebAPI**.
    - Sovelluksen **nimen** kuvataan sovelluksesi loppukäyttäjille
    - **Sign-On URL** on sovelluksen pääkansion URL-osoite.  Esimerkki koodin oletusarvo on `https://localhost:8080`.
    - **Sovelluksen tunnus URI** on yksilöllinen sovelluksen.  Konferenssi on käyttää `https://<tenant-domain>/<app-name>`, kuten`https://contoso.onmicrosoft.com/my-first-aad-app`
- Kun olet suorittanut rekisteröinti, AAD Määritä sovelluksen asiakkaan yksilöllinen tunnus.  Tarvitset tätä arvoa seuraavat osiot niin kopioi se määritys-välilehti.

- MUISTUTUS: **Sovelluksen salaisuus** sovelluksen luominen ja kopioiminen alaspäin.  Tarvitset sitä pian.
- MUISTUTUS: Kopioi **Tunnus** , jolla määritetään sovelluksen alaspäin.  Tarvitset myös sen myöhemmin.


## <a name="3-download-nodejs-for-your-platform"></a>3. Lataa omaa ympäristöäsi node.js
Voit käyttää tätä mallia onnistuneesti, sinulla on oltava Node.js toimimasta Officen asennus.

Asenna Node.js [http://nodejs.org](http://nodejs.org).

## <a name="4-install-mongodb-on-to-your-platform"></a>4. Asenna MongoDB voin omaa ympäristöäsi

Voit käyttää tätä mallia onnistuneesti, sinulla on oltava MongoDB toimimasta Officen asennus. Käytämme MongoDB tekemään Microsoftin REST API persistant eri palvelimen esiintymien välillä.

Asenna MongoDB [http://mongodb.org](http://www.mongodb.org).

> [AZURE.NOTE] Tätä vaiheittaista oletetaan, että käytät MongoDB, joka tässä kirjoittaminen aikaa on asennus- ja päätepisteet: mongodb://localhost


## <a name="5-install-the-restify-modules-in-to-your-web-api"></a>5. asentaa verkko-Ohjelmointirajapinnan Restify moduulit

Olemme käyttävillä Resitfy luonnissa Microsoftin REST-Ohjelmointirajapinnalla. Restify mahdollisimman vähän ja joustavassa Node.js-sovelluksen framework johdetun Expressistä, jossa on lukuisia etsimisen REST API päälle yhteyden ominaisuudet.

### <a name="install-restify"></a>Asenna Restify

Komentorivin muuttaa kansioiden azuread-kansioon. Jos **azuread** -kansiota ei ole, luo se.

`cd azuread - or- mkdir azuread; cd azuread`

Kirjoita seuraava komento:

`npm install restify`

Tämä komento asentaa Restify.

#### <a name="did-you-get-an-error"></a>HANKIT VIRHEEN?

Kun käytät npm joissakin käyttöjärjestelmissä, näyttöön voi tulla virhe virhe: EPERM, chmod "/ usr/paikallisen/bin /.." ja pyyntö tilin Suorita järjestelmänvalvojana. Jos näin tapahtuu, suorita npm käyttöoikeuksien ylempänä sudo-komennon avulla.

#### <a name="did-you-get-an-error-regarding-dtrace"></a>HANKIT DTRACE KOSKEVA VIRHE?

Näkyviin voi tulla tältä, kun asennat Restify:

```Shell
clang: error: no such file or directory: 'HD/azuread/node_modules/restify/node_modules/dtrace-provider/libusdt'
make: *** [Release/DTraceProviderBindings.node] Error 1
gyp ERR! build error
gyp ERR! stack Error: `make` failed with exit code: 2
gyp ERR! stack     at ChildProcess.onExit (/usr/local/lib/node_modules/npm/node_modules/node-gyp/lib/build.js:267:23)
gyp ERR! stack     at ChildProcess.EventEmitter.emit (events.js:98:17)
gyp ERR! stack     at Process.ChildProcess._handle.onexit (child_process.js:789:12)
gyp ERR! System Darwin 13.1.0
gyp ERR! command "node" "/usr/local/lib/node_modules/npm/node_modules/node-gyp/bin/node-gyp.js" "rebuild"
gyp ERR! cwd /Volumes/Development HD/azuread/node_modules/restify/node_modules/dtrace-provider
gyp ERR! node -v v0.10.11
gyp ERR! node-gyp -v v0.10.0
gyp ERR! not ok
npm WARN optional dep failed, continuing dtrace-provider@0.2.8
```


Restify, jolla muut puhelujen soittaminen DTrace jäljittää tehokkaita. Kuitenkin monta käyttöjärjestelmä ei ole käytettävissä DTrace. Voit ohittaa nämä virheet turvallisesti.


Tämä komento pitäisi näyttää seuraavankaltaiselta:


    restify@2.6.1 node_modules/restify
    ├── assert-plus@0.1.4
    ├── once@1.3.0
    ├── deep-equal@0.0.0
    ├── escape-regexp-component@1.0.2
    ├── qs@0.6.5
    ├── tunnel-agent@0.3.0
    ├── keep-alive-agent@0.0.1
    ├── lru-cache@2.3.1
    ├── node-uuid@1.4.0
    ├── negotiator@0.3.0
    ├── mime@1.2.11
    ├── semver@2.2.1
    ├── spdy@1.14.12
    ├── backoff@2.3.0
    ├── formidable@1.0.14
    ├── verror@1.3.6 (extsprintf@1.0.2)
    ├── csv@0.3.6
    ├── http-signature@0.10.0 (assert-plus@0.1.2, asn1@0.1.11, ctype@0.5.2)
    └── bunyan@0.22.0 (mv@0.0.5)


## <a name="6-install-passportjs-in-to-your-web-api"></a>6. Asenna Passport.js-verkko-Ohjelmointirajapinnan

[Passport](http://passportjs.org/) on todennus middleware Node.js varten. Erittäin joustavia ja moduulit, Passport voi unobtrusively vetää ja pudottaa missään Express sijaitsevaan tai Resitify verkkosovellus. Strategioita kattavaa tue todennusta käyttäjänimi ja salasana, Facebook tai Twitter avulla. Microsoft on kehittänyt strategia Azure Active Directory. Tämä-moduulin asentaminen ja lisää sitten laajennuksen Azure Active Directory-strategia.

Komentorivin muuttaa kansioiden azuread-kansioon.

Kirjoita seuraava komento passport.js asentaminen

`npm install passport`

Komennon tulosteen pitäisi näyttää seuraavankaltaiselta:

    passport@0.1.17 node_modules\passport
    ├── pause@0.0.1
    └── pkginfo@0.2.3

## <a name="7-add-passport-azure-ad-to-your-web-api"></a>7. Passport Azure-AD lisääminen verkko-Ohjelmointirajapinnan

Seuraavaksi lisäämme OAuth-strategia käyttämällä passport-azuread, strategioita, joka yhdistää Passport Azure Active Directory kuuluu. Microsoft käyttää tätä strategia haltijan tunnusten Rest API-malli.

> [AZURE.NOTE] Vaikka OAuth2 tarjoaa missä tahansa suojaustunnuksen tunnettu voi myöntää kehys, vain suojaustunnuksen tietyntyyppisten on saatu laajoja käyttöä. Suojaamiseen päätepisteet, joka on ottanut on haltijan-tunnukset. Haltijan-tunnukset on eniten myöntänyt laajasti OAuth2 tunnuksen tyypin ja monta käyttöotot oletetaan, että haltijan-tunnukset ovat tunnuksen myöntää ainoa.

Komentorivin muuttaa kansioiden azuread-kansioon

Kirjoita seuraava komento Passport.js passport-azure-ad-moduulin asentaminen:

`npm install passport-azure-ad`

Komennon tulosteen pitäisi näyttää seuraavankaltaiselta:

    ``
    passport-azure-ad@1.0.0 node_modules/passport-azure-ad
    ├── xtend@4.0.0
    ├── xmldom@0.1.19
    ├── passport-http-bearer@1.0.1 (passport-strategy@1.0.0)
    ├── underscore@1.8.3
    ├── async@1.3.0
    ├── jsonwebtoken@5.0.2
    ├── xml-crypto@0.5.27 (xpath.js@1.0.6)
    ├── ursa@0.8.5 (bindings@1.2.1, nan@1.8.4)
    ├── jws@3.0.0 (jwa@1.0.1, base64url@1.0.4)
    ├── request@2.58.0 (caseless@0.10.0, aws-sign2@0.5.0, forever-agent@0.6.1, stringstream@0.0.4, tunnel-agent@0.4.1, oauth-sign@0.8.0, isstream@0.1.2, extend@2.0.1, json-stringify-safe@5.0.1, node-uuid@1.4.3, qs@3.1.0, combined-stream@1.0.5, mime-types@2.0.14, form-data@1.0.0-rc1, http-signature@0.11.0, bl@0.9.4, tough-cookie@2.0.0, hawk@2.3.1, har-validator@1.8.0)
    └── xml2js@0.4.9 (sax@0.6.1, xmlbuilder@2.6.4)



## <a name="8-add-mongodb-modules-to-your-web-api"></a>8. MongoDB moduulit lisääminen Web-Ohjelmointirajapinta

Olemme käyttävillä MongoDB kuin Microsoftin datastore tästä syystä, voit asentaa molemmat yleisesti käytetty annettava laajennuksen hallinta mallit ja rakenteet kutsuttu Mongoose sekä tietokannan ohjain MongoDB, kutsutaan myös MongoDB.


* `npm install mongoose`

## <a name="9--install-additional-modules"></a>9. Asenna moduuleja

Seuraavaksi on asentaa muut tarvittavat moduulit.


Komentorivin muuttaa kansioiden **azuread** kansioon, jos se ei vielä ole käytettävissä:

`cd azuread`


Kirjoita Asenna seuraavat moduulit node_modules hakemistossa seuraavia komentoja:

* `npm install assert-plus`
* `npm install bunyan`
* `npm update`


## <a name="10-create-a-serverjs-with-your-dependencies"></a>10. luominen server.js riippuvuudet

Server.js-tiedosto tarjoavat Microsoftin toimintoja useimpia Microsoftin verkko-Ohjelmointirajapinnan palvelimen. Microsoft lisäämiseen koodia useimmat tähän tiedostoon. Tuotannon tarkoituksiin refactor pienempi tiedostojen, kuten eri tiet ja ohjaimet toimintoja. Jotta tämä esittely Käytämme server.js tämän toiminnon.

Komentorivin muuttaa kansioiden **azuread** kansioon, jos se ei vielä ole käytettävissä:

`cd azuread`

Luo `server.js` Microsoftin tuttuja editorissa tiedosto ja lisää seuraavat tiedot:

```Javascript
    'use strict';

    /**
    * Module dependencies.
    */

    var fs = require('fs');
    var path = require('path');
    var util = require('util');
    var assert = require('assert-plus');
    var bunyan = require('bunyan');
    var getopt = require('posix-getopt');
    var mongoose = require('mongoose/');
    var restify = require('restify');
    var passport = require('passport');
  var BearerStrategy = require('passport-azure-ad').BearerStrategy;
```

Tallenna tiedosto. Olemme palauttaa sen myöhemmin.

## <a name="11-create-a-config-file-to-store-your-azure-ad-settings"></a>11:. Luo määritystiedosto tallentamaan Azure AD-asetukset

Kooditiedoston välittää määritysten parametrit Azure Active Directory-portaalin Passport.js. Voit luoda määritysten nämä arvot lisätessäsi verkko-Ohjelmointirajapinnan vaiheittaista alkuosa portaaliin. Kerromme ohjata näiden parametrien arvot kopioitu koodi.


Komentorivin muuttaa kansioiden **azuread** kansioon, jos se ei vielä ole käytettävissä:

`cd azuread`

Luo `config.js` Microsoftin tuttuja editorissa tiedosto ja lisää seuraavat tiedot:

```Javascript
 exports.creds = {
     mongoose_auth_local: 'mongodb://localhost/tasklist', // Your mongo auth uri goes here
     clientID: 'your client ID',
     audience: 'your application URL',
    // you cannot have users from multiple tenants sign in to your server unless you use the common endpoint
  // example: https://login.microsoftonline.com/common/.well-known/openid-configuration
     identityMetadata: 'https://login.microsoftonline.com/<your tenant id>/.well-known/openid-configuration',
     validateIssuer: true, // if you have validation on, you cannot have users from multiple tenants sign in to your server
     passReqToCallback: false,
     loggingLevel: 'info' // valid are 'info', 'warn', 'error'. Error always goes to stderr in Unix.

 };


```
Tallenna tiedosto.

## <a name="12-add-configuration-to-your-serverjs-file"></a>12. määritysten lisääminen server.js-tiedostoon

Nämä arvot lukea luomaasi yli tämän sovelluksen määritystiedosto annettava. Voit tehdä tämän on Lisää .config-tiedostossa tarvittavat resurssiksi tämän sovelluksen ja määritä sitten kaltaisia config.js asiakirjan yleiset muuttujat

Komentorivin muuttaa kansioiden **azuread** kansioon, jos se ei vielä ole käytettävissä:

`cd azuread`

Avaa oman `server.js` Microsoftin tuttuja editorissa tiedosto ja lisää seuraavat tiedot:

```Javascript
var config = require('./config');
```
Lisää sitten uusi osa `server.js` se seuraavalla koodilla:

```Javascript
var options = {
    // The URL of the metadata document for your app. We will put the keys for token validation from the URL found in the jwks_uri tag of the in the metadata.
    identityMetadata: config.creds.identityMetadata,
    clientID: config.creds.clientID,
    validateIssuer: config.creds.validateIssuer,
    audience: config.creds.audience,
    passReqToCallback: config.creds.passReqToCallback,
    loggingLevel: config.creds.loggingLevel

};

// array to hold logged in users and the current logged in user (owner)
var users = [];
var owner = null;

// Our logger
var log = bunyan.createLogger({
    name: 'Azure Active Directory Bearer Sample',
         streams: [
        {
            stream: process.stderr,
            level: "error",
            name: "error"
        },
        {
            stream: process.stdout,
            level: "warn",
            name: "console"
        }, ]
});

  // if logging level specified, switch to it.
  if (config.creds.loggingLevel) { log.levels("console", config.creds.loggingLevel); }

// MongoDB setup
// Setup some configuration
var serverPort = process.env.PORT || 8080;
var serverURI = (process.env.PORT) ? config.creds.mongoose_auth_mongohq : config.creds.mongoose_auth_local;
```

Tallenna tiedosto.



## <a name="13-add-the-mongodb-model-and-schema-information-using-moongoose"></a>13. Lisää MongoDB mallia ja rakennetietoja Moongoose käyttäminen

Nyt kaikki tämän käsittelyn suorittaminen Käynnistä maksaa, kun olemme tuuli nämä kolme tiedostoa yhdessä REST API-palveluun.

Näiden vaiheiden on käyttää MongoDB tallentamiseen sekä tehtäviä, kuten edellä ***Vaiheessa***4.

Jos Peruuta `config.js` luomaamme ***Vaihe 11*** tiedosto on nimeltään Microsoftin tietokannan `tasklist` , joka oli olemme valitseminen meidän mogoose_auth_local yhteyden URL-Osoitteen loppuun. Sinun ei tarvitse luoda tietokannan etukäteen MongoDB, se luo tätä yhteyttä Microsoftin (olettaen että se ei ole jo) sovelluksen ensimmäisen käynnistyksen.

Nyt kun olemme olet kertoo palvelimen käyttämään haluamme MongoDB mitä tietokantaa, annettava joitakin muita koodin luominen mallin ja rakenteen Microsoftin palvelimeen tehtävien kirjoittaminen.

#### <a name="discussion-of-the-model"></a>Keskustelun mallin

Tutustu rakenne-malli on erittäin yksinkertaisia ja laajenna halutulla tavalla.

NIMI - nimi, joka on varattu tehtävään. ***Merkkijono***

TEHTÄVÄ - tehtävän itse. ***Merkkijono***

PÄIVÄMÄÄRÄ - päivämäärä, jolloin tehtävä on asianmukaisesti. ***Päivämäärä ja aika***

Valmis – Jos tehtävä on suoritettu vai ei. ***TOTUUSARVO***

#### <a name="creating-the-schema-in-the-code"></a>Mallin luominen koodi


Komentorivin muuttaa kansioiden **azuread** kansioon, jos se ei vielä ole käytettävissä:

`cd azuread`

Avaa oman `server.js` Microsoftin tuttuja editorissa tiedosto ja lisää seuraavat tiedot pienempiä määritykset:

```Javascript
// Connect to MongoDB
global.db = mongoose.connect(serverURI);
var Schema = mongoose.Schema;
log.info('MongoDB Schema loaded');

// Here we create a schema to store our tasks and users. Pretty simple schema for now.
var TaskSchema = new Schema({
    owner: String,
    task: String,
    completed: Boolean,
    date: Date
});

// Use the schema to register a model
mongoose.model('Task', TaskSchema);
var Task = mongoose.model('Task');
```
Erotat reunalliset koodi, kuten on luoda tämän mallin ja luo sitten tallentamiseen tietojamme koko koodi, kun olemme määrittää Microsoftin ***tiet***Käytämme malli-objekti.

## <a name="14-add-our-routes-for-our-task-rest-api-server"></a>14. Lisää Microsoftin tiet, tutustu tehtävän REST API-palvelin

Nyt kun on tietokantamallin käyttöä varten, lisääminen tiet, tutustu REST API-palvelimen Käytämme.

### <a name="about-routes-in-restify"></a>Tietoja tiet kohdassa Restify

Tiet toimi Restify samannäköisinä käyttämällä Express-pino täsmälleen samalla tavalla. Voit määrittää tiet avulla, jolloin oletat Soita asiakassovelluksissa URI. Yleensä että tiet määritetään erilliseen tiedostoon. Tutustu tarkoituksiin on sijoittaa Microsoftin tiet server.js-tiedostossa. Suosittelemme, että voit ottaa huomioon nämä omia tiedostoon tuotannon käyttöä varten.

Tyypillinen kuvio Restify reitin on:

```Javascript
function createObject(req, res, next) {

// do work on Object

 _object.name = req.params.object; // passed value is in req.params under object

 ///...

return next(); // keep the server going
}

....

server.post('/service/:add/:object', createObject); // calls createObject on routes that match this.

```


Tämä on yleisin tasolla kuvio. Resitfy (ja Express) on paljon tarkempaa functionaltiy, kuten määrittäminen sovelluksen tyypit ja suorittamalla monimutkaisia reitittämisen eri päätepisteet. Tämän vuoksi olemme valitseminen pitää nämä tiet hyvin yksinkertaisesti.

### <a name="1-add-default-routes-to-our-server"></a>1. Lisää oletusarvon tiet Microsoftin palvelimeen

Microsoft nyt lisääminen basic CRUD tiet, Luo, Nouda-päivityksen ja poistaminen.

Komentorivin muuttaa kansioiden **azuread** kansioon, jos se ei vielä ole käytettävissä:

`cd azuread`

Avaa oman `server.js` Microsoftin tuttuja editorissa tiedosto ja lisää seuraavat tiedot edellä tekemäsi tietokantamerkintöjen alla:

```Javascript

/**
 *
 * APIs for our REST Task server
 */

// Create a task

function createTask(req, res, next) {

    // Resitify currently has a bug which doesn't allow you to set default headers
    // This headers comply with CORS and allow us to mongodbServer our response to any origin

    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-Control-Allow-Headers", "X-Requested-With");

    // Create a new task model, fill it up and save it to Mongodb
    var _task = new Task();

    if (!req.params.task) {
        req.log.warn('createTodo: missing task');
        next(new MissingTaskError());
        return;
    }

    _task.owner = owner;
    _task.task = req.params.task;
    _task.date = new Date();

    _task.save(function(err) {
        if (err) {
            req.log.warn(err, 'createTask: unable to save');
            next(err);
        } else {
            res.send(201, _task);

        }
    });

    return next();

}


// Delete a task by name

function removeTask(req, res, next) {

    Task.remove({
        task: req.params.task,
        owner: owner
    }, function(err) {
        if (err) {
            req.log.warn(err,
                'removeTask: unable to delete %s',
                req.params.task);
            next(err);
        } else {
            log.info('Deleted task:', req.params.task);
            res.send(204);
            next();
        }
    });
}

// Delete all tasks

function removeAll(req, res, next) {
    Task.remove();
    res.send(204);
    return next();
}


// Get a specific task based on name

function getTask(req, res, next) {

    log.info('getTask was called for: ', owner);
    Task.find({
        owner: owner
    }, function(err, data) {
        if (err) {
            req.log.warn(err, 'get: unable to read %s', owner);
            next(err);
            return;
        }

        res.json(data);
    });

    return next();
}

/// Simple returns the list of TODOs that were loaded.

function listTasks(req, res, next) {
    // Resitify currently has a bug which doesn't allow you to set default headers
    // This headers comply with CORS and allow us to mongodbServer our response to any origin

    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-Control-Allow-Headers", "X-Requested-With");

    log.info("listTasks was called for: ", owner);

    Task.find({
        owner: owner
    }).limit(20).sort('date').exec(function(err, data) {

        if (err) {
            return next(err);
        }

        if (data.length > 0) {
            log.info(data);
        }

        if (!data.length) {
            log.warn(err, "There is no tasks in the database. Did you initalize the database as stated in the README?");
        }

        if (!owner) {
            log.warn(err, "You did not pass an owner when listing tasks.");
        } else {

            res.json(data);

        }
    });

    return next();
}

```

### <a name="2-next-lets-add-some-error-handling-in-our-apis"></a>2. Seuraava, jotkin virheiden käsittelyä koskevat Microsoftin API lisääminen:

```

///--- Errors for communicating something interesting back to the client

function MissingTaskError() {
    restify.RestError.call(this, {
        statusCode: 409,
        restCode: 'MissingTask',
        message: '"task" is a required parameter',
        constructorOpt: MissingTaskError
    });

    this.name = 'MissingTaskError';
}
util.inherits(MissingTaskError, restify.RestError);


function TaskExistsError(owner) {
    assert.string(owner, 'owner');

    restify.RestError.call(this, {
        statusCode: 409,
        restCode: 'TaskExists',
        message: owner + ' already exists',
        constructorOpt: TaskExistsError
    });

    this.name = 'TaskExistsError';
}
util.inherits(TaskExistsError, restify.RestError);


function TaskNotFoundError(owner) {
    assert.string(owner, 'owner');

    restify.RestError.call(this, {
        statusCode: 404,
        restCode: 'TaskNotFound',
        message: owner + ' was not found',
        constructorOpt: TaskNotFoundError
    });

    this.name = 'TaskNotFoundError';
}

util.inherits(TaskNotFoundError, restify.RestError);
```


## <a name="15-create-your-server"></a>15. Luo palvelimellesi.

On Microsoftin tietokannan määritetty, on Microsoftin tiet paikassa ja lopuksi voit tehdä on Lisää Microsoftin esiintymä, joka hallinnoi tämän puhelut.

Restify (ja Express) laaja mukauttaminen, voit tehdä REST API-palvelin on useita, mutta uudelleen Käytämme eniten perusasetukset parhaiten tarkoituksiimme varten.

```Javascript
/**
 * Our Server
 */


var server = restify.createServer({
    name: "Azure Active Directroy TODO Server",
    version: "2.0.1"
});

// Ensure we don't drop data on uploads
server.pre(restify.pre.pause());

// Clean up sloppy paths like //todo//////1//
server.pre(restify.pre.sanitizePath());

// Handles annoying user agents (curl)
server.pre(restify.pre.userAgentConnection());

// Set a per request bunyan logger (with requestid filled in)
server.use(restify.requestLogger());

// Allow 5 requests/second by IP, and burst to 10
server.use(restify.throttle({
    burst: 10,
    rate: 5,
    ip: true,
}));

// Use the common stuff you probably want
server.use(restify.acceptParser(server.acceptable));
server.use(restify.dateParser());
server.use(restify.queryParser());
server.use(restify.gzipResponse());
server.use(restify.bodyParser({
    mapParams: true
})); // Allows for JSON mapping to REST
```

## <a name="16-adding-the-routes-to-the-server-without-authentication-for-now"></a>16. tiet lisääminen palvelimeen (ilman nyt todennus)

```Javascript
/// Now the real handlers. Here we just CRUD
/**
/*
/* Each of these handlers are protected by our OIDCBearerStrategy by invoking 'oidc-bearer'
/* in the pasport.authenticate() method. We set 'session: false' as REST is stateless and
/* we don't need to maintain session state. You can experiement removing API protection
/* by removing the passport.authenticate() method like so:
/*
/* server.get('/tasks', listTasks);
/*
**/
server.get('/tasks', listTasks);
server.get('/tasks', listTasks);
server.get('/tasks/:owner', getTask);
server.head('/tasks/:owner', getTask);
server.post('/tasks/:owner/:task', createTask);
server.post('/tasks', createTask);
server.del('/tasks/:owner/:task', removeTask);
server.del('/tasks/:owner', removeTask);
server.del('/tasks', removeTask);
server.del('/tasks', removeAll, function respond(req, res, next) {
res.send(204);
next();
});
// Register a default '/' handler
server.get('/', function root(req, res, next) {
var routes = [
'GET /',
'POST /tasks/:owner/:task',
'POST /tasks (for JSON body)',
'GET /tasks',
'PUT /tasks/:owner',
'GET /tasks/:owner',
'DELETE /tasks/:owner/:task'
];
res.send(200, routes);
next();
});
server.listen(serverPort, function() {
var consoleMessage = '\n Microsoft Azure Active Directory Tutorial';
consoleMessage += '\n +++++++++++++++++++++++++++++++++++++++++++++++++++++';
consoleMessage += '\n %s server is listening at %s';
consoleMessage += '\n Open your browser to %s/tasks\n';
consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n';
consoleMessage += '\n !!! why not try a $curl -isS %s | json to get some ideas? \n';
consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n\n';
});
```

## <a name="17-before-we-add-oauth-support-lets-run-the-server"></a>17. ennen lisäämme OAuth-tuki, oletetaan, että Suorita palvelimen.

Kokeile palvelimellesi, ennen kuin lisäämme todennus

Helpoin tapa on käyttää kääntö komentorivillä. Ennen kuin olemme vakiomuotoa, annettava yksinkertainen apuohjelma, joka pystyy jäsentää tuloksen kuin JSON. Saadakseen ne asentaa json-työkalun alta esimerkkejä käyttää sitä.

`$npm install -g jsontool`

Tämä asentaa JSON-työkalun yleisesti. Nyt kun olemme olet toimet, jotka – toistaminen palvelimen kanssa:

Varmista ensin, että monogoDB isntance on käynnissä...

`$sudo mongod`

Valitse Siirry kansioon ja Käynnistä curling...

`$ cd azuread`
`$ node server.js`

`$ curl -isS http://127.0.0.1:8080 | json`

```Shell
HTTP/1.1 200 OK
Connection: close
Content-Type: application/json
Content-Length: 171
Date: Tue, 14 Jul 2015 05:43:38 GMT
[
"GET /",
"POST /tasks/:owner/:task",
"POST /tasks (for JSON body)",
"GET /tasks",
"PUT /tasks/:owner",
"GET /tasks/:owner",
"DELETE /tasks/:owner/:task"
]
```

Olemme sitten lisätä tehtävään näin:

`$ curl -isS -X POST http://127.0.0.1:8080/tasks/brandon/Hello`

Vastauksen pitäisi olla:

```Shell
HTTP/1.1 201 Created
Connection: close
Access-Control-Allow-Origin: *
Access-Control-Allow-Headers: X-Requested-With
Content-Type: application/x-www-form-urlencoded
Content-Length: 5
Date: Tue, 04 Feb 2014 01:02:26 GMT
Hello
```
Ja voidaan luetteloida tehtäviä varten Brandon näin:

`$ curl -isS http://127.0.0.1:8080/tasks/brandon/`

Jos kaikki Tämä toimii, emme voit lisätä OAuth REST API-palvelimeen.

**Sinulla on REST API-palvelimen kanssa MongoDB!**


## <a name="18-add-authentication-to-our-rest-api-server"></a>18. käyttöoikeuksien lisääminen Microsoftin REST API-palvelin

Nyt kun on käynnissä REST-Ohjelmointirajapinnalla (congrats, yhteystietoluettelooni!) aloitetaan tekeminen on hyötyä Azure AD vastaan.

Komentorivin muuttaa kansioiden **azuread** kansioon, jos se ei vielä ole käytettävissä:

`cd azuread`

### <a name="1-use-the-oidcbearerstrategy-that-is-included-with-passport-azure-ad"></a>1: Käytä OIDCBearerStrategy, joka sisältyy passport azure-ad

Tähän mennessä on on suunniteltu tyypillinen muiden TODO-palvelimessa ilman kaikenlaista luvan. Tämä on mistä on aloittaa laajennettujen, yhdessä.

Ensin annettava osoittaa, että haluat käyttää Passport. Lisää tämä oikeus muiden server-määritys:

```Javascript
// Let's start using Passport.js

server.use(passport.initialize()); // Starts passport
server.use(passport.session()); // Provides session support
```

> [AZURE.TIP]
API kirjoitettaessa sinun olisi aina linkittäminen tiedot jotain yksilöllinen tunnus, jonka käyttäjä et voi väärentää. Kun tämä palvelin tallentaa TODO kohteita, se tallentaa ne (eli token.oid kautta) tunnus, joka on valitseminen "omistaja-kenttään käyttäjän Objektitunnus perusteella. Näin varmistat, että vain käyttäjä voi käyttää hänen tehtävät ja kukaan muu ei voi käyttää syötetty tehtävät. "Omistaja" API ei ole näyttäminen, joten ulkoinen käyttäjä voi pyytää toisen käyttäjän tehtävät, vaikka ne todennetaan.

Seuraavaksi Käytämme haltijan strategia passport azure-ad mukana tulevaa. Etsi juuri nyt koodia, kerron, se pian. Siirrä tämän jälkeen mitä pated yläpuolella:

```Javascript
/**
/*
/* Calling the OIDCBearerStrategy and managing users
/*
/* Passport pattern provides the need to manage users and info tokens
/* with a FindorCreate() method that must be provided by the implementor.
/* Here we just autoregister any user and implement a FindById().
/* You'll want to do something smarter.
**/

var findById = function(id, fn) {
    for (var i = 0, len = users.length; i < len; i++) {
        var user = users[i];
        if (user.sub === id) {
            log.info('Found user: ', user);
            return fn(null, user);
        }
    }
    return fn(null, null);
};


var bearerStrategy = new BearerStrategy(options,
    function(token, done) {
        log.info('verifying the user');
        log.info(token, 'was the token retreived');
        findById(token.sub, function(err, user) {
            if (err) {
                return done(err);
            }
            if (!user) {
                // "Auto-registration"
                log.info('User was added automatically as they were new. Their sub is: ', token.sub);
                users.push(token);
                owner = token.sub;
                return done(null, token);
            }
            owner = token.sub;
            return done(null, user, token);
        });
    }
);

passport.use(bearerStrategy);
```

Passport käyttää kaikkien strategioita (Twitteriin, Facebook, jne.), joka kaikki strategia kirjoittajat noudata on samanlaisia kuvion. Strategia katsoo näet olemme välittää sen function(), jossa on tunnus ja valmis parametrit sellaisena. Strategia päättäväisesti toimitetaan takaisin us kun se tekee kaikki se on työmäärä. Kun se on kannattaa tallentaa käyttäjän ja stash tunnuksen, jotta ei tarvitse pyytää sitä uudelleen.

> [AZURE.IMPORTANT]
Yllä olevan koodin kestää kuka tahansa käyttäjä, joka todentaa tapahtuu Microsoftin palvelimeen. Tätä kutsutaan automaattinen rekisteröinti. Tuotannon palvelinten Eikö haluat sallia kenen tahansa avaamatta niitä käsitellään rekisteröitymisen, päätät ensimmäisen. Tämä on yleensä kuluttaja-sovellukset, jotka mahdollistavat Rekisteröi Facebookiin, mutta pyytää täyttämään seuraavat tiedot näkyvät mallia. Jos tämä ei komentorivi-ohjelmasta, emme voi on vain poimittujen sähköpostin suojaustunnuksen objekti, joka on palautettu ja pyytää ne voivat täyttää tietoja lisätiedot. Koska tämä on testi-palvelin on lisää ne ladatun tietokanta.

### <a name="2-finally-protect-some-endpoints"></a>2. suojaa lopuksi joitakin päätepisteitä

Voit suojata määrittämällä päätepisteet `passport.authenticate()` puhelu-ja protokolla, jota haluat käyttää.

Palvelimen koodia tekemään tiettyjä toimenpiteitä mielenkiintoisemmaksi Microsoftin reitin muokkaaminen:

```Javascript
server.get('/tasks', passport.authenticate('oauth-bearer', {
session: false
}), listTasks);
server.get('/tasks', passport.authenticate('oauth-bearer', {
session: false
}), listTasks);
server.get('/tasks/:owner', passport.authenticate('oauth-bearer', {
session: false
}), getTask);
server.head('/tasks/:owner', passport.authenticate('oauth-bearer', {
session: false
}), getTask);
server.post('/tasks/:owner/:task', passport.authenticate('oauth-bearer', {
session: false
}), createTask);
server.post('/tasks', passport.authenticate('oauth-bearer', {
session: false
}), createTask);
server.del('/tasks/:owner/:task', passport.authenticate('oauth-bearer', {
session: false
}), removeTask);
server.del('/tasks/:owner', passport.authenticate('oauth-bearer', {
session: false
}), removeTask);
server.del('/tasks', passport.authenticate('oauth-bearer', {
session: false
}), removeTask);
server.del('/tasks', passport.authenticate('oauth-bearer', {
session: false
}), removeAll, function respond(req, res, next) {
res.send(204);
next();
});
```

## <a name="19-run-your-server-application-again-and-ensure-it-rejects-you"></a>19. Suorita palvelimen sovellus uudelleen ja varmista, voit hylkää

Käytetään `curl` uudelleen, jotta näet, jos Microsoft on nyt OAuth2 suojaus vastaan Microsoftin päätepisteet. Microsoft Tee tämä ennen käynnissä vastaan tämän päätepisteen asiakasta SDK: T. Otsikot, funktio palauttaa olisi riittävästi kerro, emme oikea polku alaspäin.

Varmista ensin, että monogoDB esiintymää on käynnissä:

  $sudo mongod

Valitse Siirry kansioon ja Käynnistä curling...

  $ CD-levylle azuread $ solmu server.js

Kokeile basic VIESTIIN:

`$ curl -isS -X POST http://127.0.0.1:8080/tasks/brandon/Hello`

```Shell
HTTP/1.1 401 Unauthorized
Connection: close
WWW-Authenticate: Bearer realm="Users"
Date: Tue, 14 Jul 2015 05:45:03 GMT
Transfer-Encoding: chunked
```

401 on hakua, vastaus, joka ilmaisee Passport kerros yrittää ohjaaminen Hyväksy päätepisteen, joka on täsmälleen oikein.

## <a name="congratulations-you-have-a-rest-api-service-using-oauth2"></a>Onnittelen! Sinulla on käyttämällä OAuth2 REST API-palvelun!

Olet siirtynyt osin voit tehdä sen tämä palvelin ei ole yhteensopiva OAuth2-asiakas. Tarvitset lisää ongelmatilanteita käsitellään.

Jos etsit juuri ottamisesta käyttöön REST-Ohjelmointirajapinnalla Restify ja OAuth2 tietoja, sinun on yli tarpeeksi koodin säilyttää kehittäminen palvelun ja miten voit luoda tämän esimerkin.

Jos olet kiinnostunut seuraavat vaiheet ADAL matkan, seuraavassa on joitakin tuetut ADAL ohjelmat, on suositeltavaa, voit jatkaa työskentelyä:

Kloonaa riittää, että Kehitystyökalut-koneen alaspäin ja määritä ilmoitetulla tavalla vaiheittaisissa ohjeissa.

[IOS ADAL](https://github.com/MSOpenTech/azure-activedirectory-library-for-ios)

[ADAL androidille](https://github.com/MSOpenTech/azure-activedirectory-library-for-android)


[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
