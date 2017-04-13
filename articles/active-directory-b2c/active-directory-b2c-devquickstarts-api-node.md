<properties
    pageTitle="Azure AD-B2C: Suojattu verkko-Ohjelmointirajapinnan käyttämällä Node.js | Microsoft Azure"
    description="Miten voit luoda Node.js verkko-Ohjelmointirajapinnan, joka hyväksyy tunnusta B2C palvelutili"
    services="active-directory-b2c"
    documentationCenter=""
    authors="brandwe"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="javascript"
    ms.topic="hero-article"
    ms.date="08/30/2016"
    ms.author="brandwe"/>

# <a name="azure-ad-b2c-secure-a-web-api-by-using-nodejs"></a>Azure AD-B2C: Suojattu verkko-Ohjelmointirajapinnan Node.js avulla

<!-- TODO [AZURE.INCLUDE [active-directory-b2c-devquickstarts-web-switcher](../../includes/active-directory-b2c-devquickstarts-web-switcher.md)]-->

Azure Active Directory (Azure AD) B2C, jossa voit suojata verkko-Ohjelmointirajapinnan OAuth 2.0 access tunnusten avulla. Näiden tunnusten Salli asiakkaan sovellus, joka todentaa ohjelmointirajapinnan Azure AD B2C avulla. Tämän artikkelin avulla voit luoda "tehtäväluettelo"-Ohjelmointirajapinta, jonka avulla käyttäjät voivat lisätä ja luettelo tehtävistä. Web-Ohjelmointirajapinnan on suojattu Azure AD B2C ja sallii vain todennetut käyttäjät voivat hallita niiden tehtäväluettelo.

> [AZURE.NOTE]  Tässä esimerkissä on kirjoitettu edellyttää yhteyttä käyttämällä Microsoftin [iOS sovelluksen B2C malli](active-directory-b2c-devquickstarts-ios.md). Tee ensin nykyinen hallintapaketteihin ja noudata sitten sekä näytteen.

**Passport** on todennus middleware Node.js varten. Joustava ja moduulit, Passport voidaan asentaa unobtrusively missä tahansa Express-pohjainen tai Restify verkkosovellus. Strategioita kattavaa tukee todennusta käyttämällä käyttäjänimen ja salasanan, Facebook, Twitter ja lisää. Microsoft on kehittänyt strategia Azure Active Directory (Azure AD). Tämä-moduulin asentaminen ja lisää sitten Azure AD `passport-azure-ad` laajennus.

Voit tehdä tässä esimerkissä on:

1. Voit rekisteröidä sovelluksen Azure AD.
2. Määrittäminen käyttämään Passportin käyttäjän sovelluksen `azure-ad-passport` laajennus.
3. Määritä Soita "tehtäväluettelo" verkko-Ohjelmointirajapinnan asiakassovellus.


## <a name="get-an-azure-ad-b2c-directory"></a>Hae Azure AD B2C-kansio

Ennen kuin voit käyttää Azure AD B2C, voit luoda hakemiston tai vuokraaja.  Kansio on kaikille käyttäjille, sovellukset, ryhmiä ja lisää säilö.  Jos sinulla ei ole jokin jo, ennen kuin voit [luoda hakemiston B2C](active-directory-b2c-get-started.md) edelleen.

## <a name="create-an-application"></a>Sovelluksen luominen

Seuraavaksi tarvitset-sovelluksen luominen, joka palauttaa Azure AD tietoja, jotka tarvitaan suojatusti viestintään sovelluksen B2C hakemistossa. Tässä tapauksessa asiakas-sovellus ja verkko-Ohjelmointirajapinnan ilmaistaan yksi **Tunnus**, koska ne muodostavat yhteen looginen-sovellukseen. Voit luoda sovelluksen noudattamalla [näitä ohjeita](active-directory-b2c-app-registration.md). Muista:

- Sisällytä **web app-verkko api** -sovelluksessa
- Kirjoita `http://localhost/TodoListService` **vastaa URL**-osoitteeksi. Tässä esimerkissä koodin oletusarvoisen URL-osoite on.
- **Sovelluksen salaisuus** sovelluksen luominen ja kopioiminen. Tarvitset näitä tietoja myöhemmin. Huomaa, että tämä arvo on oltava [XML-ohitettuja](https://www.w3.org/TR/2006/REC-xml11-20060816/#dt-escape) , ennen kuin käytät sitä.
- Kopioi **Tunnus** , jolla määritetään sovelluksen. Tarvitset näitä tietoja myöhemmin.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>Luoda oman käytäntöjä

Azure AD B2C jokaisen käyttäjäkokemuksen on määritetty [käytännön](active-directory-b2c-reference-policies.md)avulla. Sovelluksen sisältää kaksi identity-versiota: Rekisteröidy ja kirjaudu sisään. Haluat yhden käytännön kunkin tyypin [käytännön viitata artikkelissa](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy)kuvatulla tavalla.  Kun olet luonut kolme käytäntöjä, muista:

- Valitse kirjautumisen käytännön **Näyttönimi** ja ilmoittautuminen määritteet.
- Valitse jokaisen käytännön **Näyttönimi** ja **Objektitunnus** sovelluksen saatavat.  Voit valita muita saatavat.
- Kopioi kunkin käytännön **nimi** muistiin, sen luomisen jälkeen. Tunnisteen pitäisi olla etuliite `b2c_1_`.  Käytännön nimet on myöhemmin.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Luotuasi kolme käytännöt olet valmis luomaan sovelluksen.

Lisätietoja siitä, kuinka käytännöt toimivat Azure AD B2C, aloita [.NET web app aloitusopas](active-directory-b2c-devquickstarts-web-dotnet.md).

## <a name="download-the-code"></a>Lataa koodi

Tämän opetusohjelman [määritetään GitHub](https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS)koodi. Voit luoda otosten kuitenkin, [Lataa rakenne projektin .zip-tiedostona](https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS/archive/skeleton.zip). Voit myös kopioida rakenne:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS.git
```

Valmis-sovellus on myös [saatavilla .zip-tiedostona](https://github.com/AzureADQuickStarts/B2C-WebAPI-NodeJS/archive/complete.zip) tai `complete` saman säilö haaraa.

## <a name="download-nodejs-for-your-platform"></a>Lataa Node.js omaa ympäristöäsi

Voit käyttää tätä mallia onnistuneesti, sinun on Node.js toimimasta Officen asennus. 

Asenna Node.js [nodejs.org](http://nodejs.org).

## <a name="install-mongodb-for-your-platform"></a>Asenna MongoDB omaa ympäristöäsi

Voit käyttää tätä mallia onnistuneesti, sinun on MongoDB toimimasta Officen asennus. MongoDB avulla sijoittaa REST-Ohjelmointirajapinnalla jatkuva eri palvelimen esiintymien välillä.

Asenna MongoDB [mongodb.org](http://www.mongodb.org).

> [AZURE.NOTE] Tämä hallintapaketteihin oletetaan, että käytät asennus- ja päätepisteet MongoDB, joka tässä kirjoittaminen aikaa on `mongodb://localhost`.

## <a name="install-the-restify-modules-in-your-web-api"></a>Asenna Restify moduulit verkko-Ohjelmointirajapinnan

Käytämme Restify luonnissa REST-Ohjelmointirajapinnalla. Restify vähän ja joustavassa Node.js-sovelluksen framework johdetaan Express. Siinä on lukuisia etsimisen REST API päälle yhteyden ominaisuudet.

### <a name="install-restify"></a>Asenna Restify

Komentoriviltä, muuttaminen, hakemistossa `azuread`. Jos `azuread` kansio ei ole, luo se.

`cd azuread`tai`mkdir azuread;`

Kirjoita seuraava komento:

`npm install restify`

Tämä komento asentaa Restify.

#### <a name="did-you-get-an-error"></a>Hankit virheen?

Jotkin käyttöjärjestelmien, kun käytät `npm`, näyttöön voi tulla virhesanoma `Error: EPERM, chmod '/usr/local/bin/..'` ja pyyntö tilin Suorita järjestelmänvalvojana. Jos tämä ongelma ilmenee, `sudo` Suorita-komennon `npm` käyttöoikeuksien ylempänä.

#### <a name="did-you-get-a-dtrace-error"></a>Hankit DTrace virhe?

Näkyviin voi tulla suunnilleen tämän tekstin, kun asennat Restify:

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

Restify on tehokas järjestelmä jäljityksessä muiden soittaa DTrace avulla. Kuitenkin monta käyttöjärjestelmä ei ole käytettävissä DTrace. Voit ohittaa nämä virheet turvallisesti.

Komennon tulosteen pitäisi olla seuraavanlainen teksti:

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

## <a name="install-passport-in-your-web-api"></a>Asenna Passport verkko-Ohjelmointirajapinnan

Komentoriviltä, muuttaminen, hakemistossa `azuread`, ellei se ole jo näkyvissä.

Asenna Passportia seuraava komento:

`npm install passport`

Komennon tulosteen pitäisi olla samankaltainen tekstiin:

    passport@0.1.17 node_modules\passport
    ├── pause@0.0.1
    └── pkginfo@0.2.3

## <a name="add-passport-azuread-to-your-web-api"></a>Lisää verkko-Ohjelmointirajapinnan passport azuread

Lisää seuraavaksi käyttämällä OAuth-strategia `passport-azuread`, strategioita, joka yhdistää Passport Azure AD kuuluu. Käytä tätä strategia haltijan-tunnukset REST API otoksessa.

> [AZURE.NOTE] Vaikka OAuth2 tarjoaa missä tahansa suojaustunnuksen tunnettu voi myöntää kehys, vain tietyt suojaustunnuksen tietotyypit on saatu laajasti käytössä. Päätepisteet suojaamisessa käytettävät tunnukset ovat haltijan-tunnukset. Näiden tunnusten ovat OAuth2 lähetetyn yleisimmät. Monta käyttöotot oletetaan, että haltijan-tunnukset ovat tunnuksen myöntää ainoa.

Komentoriviltä, muuttaminen, hakemistossa `azuread`, ellei se ole jo näkyvissä.

Asenna passin `passport-azure-ad` moduulissa käyttämällä seuraava komento:

`npm install passport-azure-ad`

Komennon tulosteen pitäisi olla samankaltainen tekstiin:

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
``

## <a name="add-mongodb-modules-to-your-web-api"></a>Lisää verkko-Ohjelmointirajapinnan MongoDB moduulit

Tämä esimerkki käyttää MongoDB tiedot tallennetaan. Osalta, asentamalla Mongoose, yleisesti käytetty laajennus hallinta mallit ja rakenteet.

* `npm install mongoose`

## <a name="install-additional-modules"></a>Asenna muita moduulit

Asenna seuraavaksi jäljellä olevat tarvittavat moduulit.

Komentoriviltä, muuttaminen, hakemistossa `azuread`, ellei se ole jo:

`cd azuread`

Asenna moduulit oman `node_modules` hakemisto:

* `npm install assert-plus`
* `npm install ejs`
* `npm install ejs-locals`
* `npm install express`
* `npm install bunyan`


## <a name="create-a-serverjs-file-with-your-dependencies"></a>Luo server.js-tiedosto, jonka riippuvuudet

`server.js` Tiedosto sisältää toimintoja useimpia verkko-Ohjelmointirajapinnan palvelimellesi. 

Komentoriviltä, muuttaminen, hakemistossa `azuread`, ellei se ole jo:

`cd azuread`

Luo `server.js` editorissa tiedosto. Lisää seuraavat tiedot:

```Javascript
'use strict';
/**
* Module dependencies.
*/
var fs = require('fs');
var path = require('path');
var util = require('util');
var assert = require('assert-plus');
var mongoose = require('mongoose/');
var bunyan = require('bunyan');
var restify = require('restify');
var config = require('./config');
var passport = require('passport');
var OIDCBearerStrategy = require('passport-azure-ad').BearerStrategy;
```

Tallenna tiedosto. Palaat siihen myöhemmin.

## <a name="create-a-configjs-file-to-store-your-azure-ad-settings"></a>Luo config.js tiedoston tallentamiseen Azure AD-asetukset

Kooditiedoston välittää määritysten parametrit Azure AD-portaalista `Passport.js` tiedosto. Voit luoda määritysten nämä arvot lisätessäsi verkko-Ohjelmointirajapinnan hallintapaketteihin alkuosa portaaliin. Kerromme ohjata näiden parametrien arvot, kun olet kopioinut koodi.

Komentoriviltä, muuttaminen, hakemistossa `azuread`, ellei se ole jo:

`cd azuread`

Luo `config.js` editorissa tiedosto. Lisää seuraavat tiedot:

```Javascript
// Don't commit this file to your public repos. This config is for first-run
exports.creds = {
clientID: <your client ID for this Web API you created in the portal>
mongoose_auth_local: 'mongodb://localhost/tasklist', // Your mongo auth uri goes here
audience: '<your audience URI>', // the Client ID of the application that is calling your API, usually a web API or native client
identityMetadata: 'https://login.microsoftonline.com/<tenant name>/.well-known/openid-configuration', // Make sure you add the B2C tenant name in the <tenant name> area
tenantName:'<tenant name>', 
policyName:'b2c_1_<sign in policy name>' // This is the policy you'll want to validate against in B2C. Usually this is your Sign-in policy (as users sign in to this API)
passReqToCallback: false // This is a node.js construct that lets you pass the req all the way back to any upstream caller. We turn this off as there is no upstream caller.
};

```

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

### <a name="required-values"></a>Pakolliset arvot

`clientID`Verkko-Ohjelmointirajapinnan sovelluksesi: Asiakastunnus.

`IdentityMetadata`: Missä tämä on `passport-azure-ad` määritysten tietojen etsitään tunnistetietojen toimittaja. Se näyttää myös näppäimet Vahvista JSON-web-tunnukset. 

`audience`: Uniform resource identifier (URI), joka määrittää puheluja sovelluksen-portaalista. 

`tenantName`: Vuokraajan oma nimi (esimerkiksi **contoso.onmicrosoft.com**).

`policyName`:-Käytäntö, jonka haluat vahvistaa tunnusten saapua palvelimellesi. Käytäntö on oltava sama käytännön, joka käyttäminen asiakassovellus kirjautumista varten.

> [AZURE.NOTE] Tutustu B2C esikatselua varten käyttää samaa käytännöt asiakkaan ja palvelimen asetukset. Jos olet jo suorittanut hallintapaketteihin ja luotu kyseiset käytännöt, ei tarvitse tehdä uudelleen. Koska olet valmis hallintapaketteihin, joudut ei kannata määrittäminen uusi asiakas walk-throughs sivustossa.

## <a name="add-configuration-to-your-serverjs-file"></a>Lisää määrityksen server.js-tiedosto

Lukea arvot `config.js` tiedosto luodaan, Lisää `.config` tarvittava resurssi sovelluksen nimellä ja määritä sitten kaltaisia yleiset muuttujat `config.js` asiakirjan.

Komentoriviltä, muuttaminen, hakemistossa `azuread`, ellei se ole jo:

`cd azuread`

Avaa `server.js` editorissa tiedosto. Lisää seuraavat tiedot:

```Javascript
var config = require('./config');
```
Lisää uusi osa, `server.js` , jossa on seuraava koodi:

```Javascript
// We pass these options in to the ODICBearerStrategy.

var options = {
    // The URL of the metadata document for your app. We put the keys for token validation from the URL found in the jwks_uri tag of the in the metadata.
    identityMetadata: config.creds.identityMetadata,
    clientID: config.creds.clientID,
    tenantName: config.creds.tenantName,
    policyName: config.creds.policyName,
    validateIssuer: config.creds.validateIssuer,
    audience: config.creds.audience,
    passReqToCallback: config.creds.passReqToCallback

};
```

Seuraavaksi lisääminen paikkamerkit saamme Microsoftin puheluja sovelluksista käyttäjille.

```Javascript
// array to hold logged in users and the current logged in user (owner)
var users = [];
var owner = null;
```

Nyt ja luo Microsoftin lokin liian.

```Javascript
// Our logger
var log = bunyan.createLogger({
    name: 'Microsoft Azure Active Directory Sample'
});
```

## <a name="add-the-mongodb-model-and-schema-information-by-using-mongoose"></a>Lisää MongoDB malli ja rakenteen tiedot käyttämällä Mongoose

Aiemmissa valmistelu maksaa, kun voit noutaa kolme tiedostoja yhdessä REST API-palvelun.

Saat tämän hallintapaketteihin käyttää MongoDB tallentamiseen tehtäviä, kuten edellä kerrottiin.

Valitse `config.js` tiedoston, voit kutsua tietokanta- **tasklist**. Nimi on myös sijoittaa lopussa `mongoose_auth_local` yhteyden URL-osoite. Ei tarvitse luoda tietokannan etukäteen MongoDB. Se luo tietokannan, palvelin-sovelluksen ensimmäisen käynnistyksen.

Kun voit tarkistaa palvelimen MongoDB-tietokannan, sinun on joitakin muita koodin luominen mallin ja rakenteen palvelimen tehtäviin kirjoittaminen.

### <a name="expand-the-model"></a>Laajenna malli

Tämän mallin rakenne on yksinkertainen. Voit laajentaa sen halutulla tavalla.

`owner`: Kuka on varattu tehtävään. Tämä objekti on **merkkijono**.  

`Text`: Tehtävän itse. Tämä objekti on **merkkijono**.

`date`: Päivämäärä, jona tehtävä on asianmukaisesti. Tämä objekti on **päivämäärä ja aika**.

`completed`: Jos tehtävä on suoritettu. Tämä objekti on **totuusarvo**.

### <a name="create-the-schema-in-the-code"></a>Mallin luominen koodi

Komentoriviltä, muuttaminen, hakemistossa `azuread`, ellei se ole jo:

`cd azuread`

Avaa `server.js` -editorissa tiedosto. Lisää pienempiä määritysten seuraavat tiedot:

```Javascript
// MongoDB setup
// Setup some configuration
var serverPort = process.env.PORT || 3000; // Note we are hosting our API on port 3000
var serverURI = (process.env.PORT) ? config.creds.mongoose_auth_mongohq : config.creds.mongoose_auth_local;

// Connect to MongoDB
global.db = mongoose.connect(serverURI);
var Schema = mongoose.Schema;
log.info('MongoDB Schema loaded');

// Here we create a schema to store our tasks and users. Pretty simple schema for now.
var TaskSchema = new Schema({
    owner: String,
    Text: String,
    completed: Boolean,
    date: Date
});

// Use the schema to register a model
mongoose.model('Task', TaskSchema);
var Task = mongoose.model('Task');
```
Sinun on luotava rakenne ja luo sitten malli-objekti, jonka avulla tallentaa tiedot koko koodi, kun haluat määrittää oman **tiet**.

## <a name="add-routes-for-your-rest-api-task-server"></a>Lisää tiet REST API tehtävä-palvelin

Nyt kun olet luonut tietokantamallin käyttöä varten, Lisää tiet, voit käyttää REST API-palvelin.

### <a name="about-routes-in-restify"></a>Tietoja Restify tiet

Tiet toimivat samalla tavalla, ne toimivat käyttäessään Express-pino Restify. Tiet määrittäminen käyttämällä URI, jonka oletat asiakassovelluksissa Soita. 

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

Restify ja Express voidaan lisätä paljon tarkempaa toimintojen, kuten määrittäminen sovelluksen tyypit ja suorittamalla monimutkaisia reitittämisen eri päätepisteet. Tässä opetusohjelmassa tarkoitetaan säilyy nämä tiet yksinkertainen.

#### <a name="add-default-routes-to-your-server"></a>Lisää oletusarvon tiet palvelimeen

Nyt voit lisätä basic CRUD tiet, **luominen** ja **luettelo** Microsoftin REST API. Muut tiet löytyy `complete` otosten haaran.

Komentoriviltä, muuttaminen, hakemistossa `azuread`, ellei se ole jo:

`cd azuread`

Avaa `server.js` editorissa tiedosto. Alla on tehty edellä tietokantamerkintöjen Lisää seuraavat tiedot:

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

    if (!req.params.Text) {
        req.log.warn({
            params: req.params
        }, 'createTodo: missing task');
        next(new MissingTaskError());
        return;
    }

    _task.owner = owner;
    _task.Text = req.params.Text;
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
```

```Javascript
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

        if (err)
            return next(err);

        if (data.length > 0) {
            log.info(data);
        }

        if (!data.length) {
            log.warn(err, "There is no tasks in the database. Add one!");
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


#### <a name="add-error-handling-for-the-routes"></a>Lisää ne reitityksen Virheenkäsittely

Lisää joitakin kun virheen käsittely niin, että voit viestiä käytössä ilmenee ongelmia takaisin niin, että se ymmärtämään asiakkaalle.

Lisää seuraava koodi:

```Javascript
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


## <a name="create-your-server"></a>Palvelimen luominen

Olet tietokannan määritetty ja että tiet asettaa. Voit tehdä viimeiseksi on lisätä server-esiintymän, joka hallitsee puhelut.

Restify ja Express tarjota laaja mukautuksen REST API-palvelimeen, mutta Käytämme eniten perusasetukset tässä. 

```Javascript

**
 * Our Server
 */


var server = restify.createServer({
    name: "Microsoft Azure Active Directroy TODO Server",
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
server.use(restify.authorizationParser()); // Looks for authorization headers

// Let's start using Passport.js

server.use(passport.initialize()); // Starts passport
server.use(passport.session()); // Provides session support


```
## <a name="add-the-routes-to-the-server-without-authentication"></a>Lisää tiet (ilman todennus)-palvelimeen

```Javascript
server.get('/api/tasks', passport.authenticate('oauth-bearer', {
    session: false
}), listTasks);
server.get('/api/tasks', passport.authenticate('oauth-bearer', {
    session: false
}), listTasks);
server.get('/api/tasks/:owner', passport.authenticate('oauth-bearer', {
    session: false
}), getTask);
server.head('/api/tasks/:owner', passport.authenticate('oauth-bearer', {
    session: false
}), getTask);
server.post('/api/tasks/:owner/:task', passport.authenticate('oauth-bearer', {
    session: false
}), createTask);
server.post('/api/tasks', passport.authenticate('oauth-bearer', {
    session: false
}), createTask);
server.del('/api/tasks/:owner/:task', passport.authenticate('oauth-bearer', {
    session: false
}), removeTask);
server.del('/api/tasks/:owner', passport.authenticate('oauth-bearer', {
    session: false
}), removeTask);
server.del('/api/tasks', passport.authenticate('oauth-bearer', {
    session: false
}), removeTask);
server.del('/api/tasks', passport.authenticate('oauth-bearer', {
    session: false
}), removeAll, function respond(req, res, next) {
    res.send(204);
    next();
});


// Register a default '/' handler

server.get('/', function root(req, res, next) {
    var routes = [
        'GET     /',
        'POST    /api/tasks/:owner/:task',
        'POST    /api/tasks (for JSON body)',
        'GET     /api/tasks',
        'PUT     /api/tasks/:owner',
        'GET     /api/tasks/:owner',
        'DELETE  /api/tasks/:owner/:task'
    ];
    res.send(200, routes);
    next();
});
```

```Javascript

server.listen(serverPort, function() {

    var consoleMessage = '\n Microsoft Azure Active Directory Tutorial';
    consoleMessage += '\n +++++++++++++++++++++++++++++++++++++++++++++++++++++';
    consoleMessage += '\n %s server is listening at %s';
    consoleMessage += '\n Open your browser to %s/api/tasks\n';
    consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n';
    consoleMessage += '\n !!! why not try a $curl -isS %s | json to get some ideas? \n';
    consoleMessage += '+++++++++++++++++++++++++++++++++++++++++++++++++++++ \n\n';

    //log.info(consoleMessage, server.name, server.url, server.url, server.url);

});

``` 

## <a name="add-authentication-to-your-rest-api-server"></a>Lisää todennus REST API-palvelimeen

Nyt, sinulla on käynnissä REST API-palvelimeen, joita voit tehdä hyödyllisiä Azure AD vastaan.

Komentoriviltä, muuttaminen, hakemistossa `azuread`, ellei se ole jo:

`cd azuread`

### <a name="use-the-oidcbearerstrategy-that-is-included-with-passport-azure-ad"></a>Käytä OIDCBearerStrategy, joka sisältyy passport azure-ad


> [AZURE.TIP]
API kirjoitettaessa sinun olisi aina linkittäminen tiedot jotain yksilöllinen tunnus, jonka käyttäjä et voi väärentää. Kun palvelin tallentaa ToDo kohteita, se tekee niin perusteella **oid** käyttäjän tunnuksen (jota kutsutaan token.oid kautta), joka ohjaa "omistaja-kenttään. Tämän arvon varmistaa, että vain kyseisen käyttäjän käyttöoikeus ToDo omia kohteita. "Omistaja" API ei ole näyttäminen, joten ulkoinen käyttäjä voi pyytää muiden ToDo kohteita, vaikka ne todennetaan.

Käytä seuraavaksi haltijan strategia mukana tulevaa `passport-azure-ad`.

```Javascript
var findById = function(id, fn) {
    for (var i = 0, len = users.length; i < len; i++) {
        var user = users[i];
        if (user.oid === id) {
            log.info('Found user: ', user);
            return fn(null, user);
        }
    }
    return fn(null, null);
};


var oidcStrategy = new OIDCBearerStrategy(options,
    function(token, done) {
        log.info('verifying the user');
        log.info(token, 'was the token retreived');
        findById(token.sub, function(err, user) {
            if (err) {
                return done(err);
            }
            if (!user) {
                // "Auto-registration"
                log.info('User was added automatically as they were new. Their sub is: ', token.oid);
                users.push(token);
                owner = token.oid;
                return done(null, token);
            }
            owner = token.sub;
            return done(null, user, token);
        });
    }
);

passport.use(oidcStrategy);
```

Passport käyttää samoissa sen strategioita. Välittää ne `function()` , joka sisältää `token` ja `done` parametreiksi. Strategia tulee takaisin sinulle, kun se on tehnyt kaikki työnsä. Olisi sitten tallentaa käyttäjän ja Tallenna tunnuksen niin, että sinun ei tarvitse pyytää sitä uudelleen.

> [AZURE.IMPORTANT]
Yllä olevan koodin Vie kaikki käyttäjät, jotka tapahtuu tarkistamiseen palvelimellesi. Tämä toimenpide on nimeltään autoregistration. Tuotannon palvelimissa Älä anna käyttäjien käyttöoikeuksia-Ohjelmointirajapinnan tarvitsematta ensimmäisen niiden käyminen läpi rekisteröinnin loppuun. Tämä toimenpide on yleensä kuluttaja-sovellukset, jotta voit rekisteröityä käyttämällä Facebook, mutta pyytää täyttämään seuraavat tiedot näkyvät kuvio. Jos ohjelma ei komentorivin ohjelman, emme voi on poimittujen sähköpostin suojaustunnuksen objekti, joka on palautettu ja pyytää käyttäjät voivat täyttää tietoja lisätiedot. Koska kyseessä on otoksen, ne on lisättävä ladatun-tietokantaan.



## <a name="run-your-server-application-to-verify-that-it-rejects-you"></a>Palvelinsovelluksen voit varmistaa, että se hylkää voit käyttämiseen

Voit käyttää `curl` , onko sinulla että päätepisteet vastaan OAuth2 suojaus nyt. Palauttaa otsikot olisi riittävästi kertoa, että kyseessä on oikea polku.

Varmista, että MongoDB esiintymää on käynnissä:

    $sudo mongodb

Siirry kansioon ja suorita palvelin:

    $ cd azuread
    $ node server.js

Pääte uudessa ikkunassa suorittaminen`curl`

Kokeile basic VIESTIIN:

`$ curl -isS -X POST http://127.0.0.1:3000/api/tasks/brandon/Hello`

```Shell
HTTP/1.1 401 Unauthorized
Connection: close
WWW-Authenticate: Bearer realm="Users"
Date: Tue, 14 Jul 2015 05:45:03 GMT
Transfer-Encoding: chunked
```

401 virhe on vastaus, jonka haluat. Se merkitsee Passport kerros yrittää ohjaaminen Hyväksy päätepiste.


## <a name="you-now-have-a-rest-api-service-that-uses-oauth2"></a>Nyt käytössäsi REST API-palveluun, joka käyttää OAuth2

REST API olet ottanut käyttöön Restify ja OAuth! Riittävän koodi on nyt, niin, että voit jatkaa kehittää palvelun ja tässä esimerkissä muodosta. Ovat kadonneet osin voit käyttämättä OAuth2-yhteensopivan-asiakasohjelman tämän palvelimen kanssa. Käytä muita hallintapaketteihin, kuten Microsoftin [Verkko-Ohjelmointirajapinnan kanssa B2C iOS käyttämällä Yhdistä](active-directory-b2c-devquickstarts-ios.md) ongelmatilanteita, seuraava vaihe.


## <a name="next-steps"></a>Seuraavat vaiheet

Voit nyt siirtyä aiheita, kuten:

[Verkko-Ohjelmointirajapinnan käyttämällä iOS B2C yhdistäminen](active-directory-b2c-devquickstarts-ios.md)