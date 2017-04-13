<properties
    pageTitle="Azure AD v2.0 NodeJS Web-sovelluksen | Microsoft Azure"
    description="Solmun JS-verkkosovelluksen tietokenttiä kirjautuu-käyttäjät sekä oman Microsoft Account muodostaminen ja työpaikan tai oppilaitoksen tilit."
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

# <a name="add-sign-in-to-a-nodejs-web-app"></a>Lisää nodeJS Web App-kirjautuminen


> [AZURE.NOTE]
    V2.0 päätepisteen tukee kaikkia Azure Active Directory-skenaariot ja toiminnot.  Voit selvittää, jos sinun on käytettävä v2.0 päätepisteen, lue lisätietoja [v2.0 rajoitukset](active-directory-v2-limitations.md).


Käytämme tähän Passportin.

- Käyttäjän kirjautua Azure AD-sovellusta ja v2.0 päätepiste.
- Näyttää käyttäjän tietoja.
- Kirjaudu ulos sovelluksesta käyttäjä.

**Passport** on todennus middleware Node.js varten. Erittäin joustavia ja moduulit, Passport voi unobtrusively vetää ja pudottaa missään Express sijaitsevaan tai Resitify verkkosovellus. Strategioita kattavaa tue todennusta käyttäjänimi ja salasana, Facebook tai Twitter avulla. Microsoft on kehittänyt strategia Microsoft Azure Active Directory. Microsoft tämän-moduulin asentaminen ja lisää sitten Microsoft Azure Active Directory `passport-azure-ad` laajennus.

## <a name="download"></a>Lataa

Tässä opetusohjelmassa koodi määritetään [GitHub](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs).  Jos haluat seurata, voit [ladata sovelluksen rakenne .zip nimellä](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs/archive/skeleton.zip) tai Kloonaa rakenne:

```git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs.git```

Valmiin sovelluksen annetaan jäljempänä tässä opetusohjelmassa.

## <a name="1-register-an-app"></a>1. Sovelluksen rekisteröiminen
Luo uuden sovelluksen [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)tai noudattamalla seuraavia [toimimalla seuraavasti](active-directory-v2-app-registration.md).  Varmista, että:

- Kopioi alaspäin **Sovellustunnus** sovelluksen myönnetyt, sinun on se pian.
- Lisää **WWW** -ympäristö, kun sovellus.
- Kirjoita oikeat **Uudelleenohjata URI**. Uudelleenohjauksen URI ilmaisee Azure AD, jossa todennus vastaukset olisi ohjautuu – Tässä opetusohjelmassa oletusarvo on `http://localhost:3000/auth/openid/return`.

## <a name="2-add-pre-requisities-to-your-directory"></a>2. pre requisities Lisää hakemisto

Valitse komentorivin muuttaa kansioiden pääkansion kansioon Jos näin ei ole jo olemassa ja suorittamalla seuraavat komennot:

- `npm install express`
- `npm install ejs`
- `npm install ejs-locals`
- `npm install restify`
- `npm install mongoose`
- `npm install bunyan`
- `npm install assert-plus`
- `npm install passport`
- `npm install webfinger`
- `npm install body-parser`
- `npm install express-session`
- `npm install cookie-parser`

- Lisäksi, että olet käyttäminen `passport-azure-ad` -pikaopas rakenne.

- `npm install passport-azure-ad`


Asennetaan kirjastot, passport-azure-ad määräytyvät.

## <a name="3-set-up-your-app-to-use-the-passport-node-js-strategy"></a>3. määrittää sovelluksen passport-solmu-js määrittäminen
Tässä on määrittää Express middleware OpenID yhteyden todennus-protokollaa.  Passport käytetään antaa Kirjaudu sisään ja kirjaudu ulos pyyntöjä ja hallita käyttäjän istunnon käyttäjän, muun muassa tietoja.

-   Avaa- `config.js` projektin ylimmällä tiedosto ja kirjoita sinua sovelluksen kokoonpanon arvoja `exports.creds` osa.
    -   `clientID:` On määritetty sovelluksen rekisteröinnin portaalissa **Tunnus** .
    -   `returnURL` On lisätty portaalin **Uudelleenohjata URI** .
    - `clientSecret` On luotu portaalissa toiminta.

- Seuraavan kerran avaat `app.js` proejct ylimmällä tiedoston ja lisätä seuraavasta puhelun käynnistää `OIDCStrategy` strategia mukana tulevaa`passport-azure-ad`


```JavaScript
var OIDCStrategy = require('passport-azure-ad').OIDCStrategy;

// Add some logging
var log = bunyan.createLogger({
    name: 'Microsoft OIDC Example Web Application'
});
```

- Tämän jälkeen käyttää on vain Viitattu Microsoftin kirjautuminen pyyntöjen määrittäminen

```JavaScript
// Use the OIDCStrategy within Passport. (Section 2)
//
//   Strategies in passport require a `validate` function, which accept
//   credentials (in this case, an OpenID identifier), and invoke a callback
//   with a user object.
passport.use(new OIDCStrategy({
    callbackURL: config.creds.returnURL,
    realm: config.creds.realm,
    clientID: config.creds.clientID,
    clientSecret: config.creds.clientSecret,
    oidcIssuer: config.creds.issuer,
    identityMetadata: config.creds.identityMetadata,
    responseType: config.creds.responseType,
    responseMode: config.creds.responseMode,
    skipUserProfile: config.creds.skipUserProfile
    scope: config.creds.scope
  },
  function(iss, sub, profile, accessToken, refreshToken, done) {
    log.info('Example: Email address we received was: ', profile.email);
    // asynchronous verification, for effect...
    process.nextTick(function () {
      findByEmail(profile.email, function(err, user) {
        if (err) {
          return done(err);
        }
        if (!user) {
          // "Auto-registration"
          users.push(profile);
          return done(null, profile);
        }
        return done(null, user);
      });
    });
  }
));
```
Passport käyttää kaikkien strategioita (Twitteriin, Facebook, jne.), joka kaikki strategia kirjoittajat noudata on samanlaisia kuvion. Strategia katsoo näet olemme välittää sen function(), jossa on tunnus ja valmis parametrit sellaisena. Strategia päättäväisesti toimitetaan takaisin us kun se tekee kaikki se on työmäärä. Kun se on kannattaa tallentaa käyttäjän ja stash tunnuksen, jotta ei tarvitse pyytää sitä uudelleen.

> [AZURE.IMPORTANT]
Yllä olevan koodin kestää kuka tahansa käyttäjä, joka todentaa tapahtuu Microsoftin palvelimeen. Tätä kutsutaan automaattinen rekisteröinti. Tuotannon palvelinten Eikö haluat sallia kenen tahansa avaamatta niitä käsitellään rekisteröitymisen, päätät ensimmäisen. Tämä on yleensä kuviota, näet kuluttaja-sovelluksissa, jotka voidaan rekisteröidä Facebookiin, mutta pyytää täyttämään seuraavat tiedot. Jos tämä ei sovelluksen malli, emme voi on vain poimittujen sähköpostin suojaustunnuksen objekti, joka on palautettu ja pyytää ne voivat täyttää tietoja lisätiedot. Koska tämä on testi-palvelin on lisää ne ladatun tietokanta.

- Seuraavaksi lisääminen menetelmiä, voimme seurantaan kirjautuneena käyttäjät Passport halutulla tavalla. Tämä sisältää sarjoitettaessa ja poistettaessa käyttäjän tiedot:

```JavaScript

// Passport session setup. (Section 2)

//   To support persistent login sessions, Passport needs to be able to
//   serialize users into and deserialize users out of the session.  Typically,
//   this will be as simple as storing the user ID when serializing, and finding
//   the user by ID when deserializing.
passport.serializeUser(function(user, done) {
  done(null, user.email);
});

passport.deserializeUser(function(id, done) {
  findByEmail(id, function (err, user) {
    done(err, user);
  });
});

// array to hold logged in users
var users = [];

var findByEmail = function(email, fn) {
  for (var i = 0, len = users.length; i < len; i++) {
    var user = users[i];
    log.info('we are using user: ', user);
    if (user.email === email) {
      return fn(null, user);
    }
  }
  return fn(null, null);
};

```

- Seuraavaksi lisääminen lataaminen express moduulin koodi. Tässä näet Microsoft käyttää oletusarvon /views ja /routes kuviota, jotka ilmaisevat tarjoaa.

```JavaScript

// configure Express (Section 2)

var app = express();


app.configure(function() {
  app.set('views', __dirname + '/views');
  app.set('view engine', 'ejs');
  app.use(express.logger());
  app.use(express.methodOverride());
  app.use(cookieParser());
  app.use(expressSession({ secret: 'keyboard cat', resave: true, saveUninitialized: false }));
  app.use(bodyParser.urlencoded({ extended : true }));
  // Initialize Passport!  Also use passport.session() middleware, to support
  // persistent login sessions (recommended).
  app.use(passport.initialize());
  app.use(passport.session());
  app.use(app.router);
  app.use(express.static(__dirname + '/../../public'));
});

```

- Lopuksi lisääminen VIESTIIN tiet, joka painotaloon todellinen kirjautuminen pyynnöt `passport-azure-ad` ohjelma:

```JavaScript

// Our Auth routes (Section 3)

// GET /auth/openid
//   Use passport.authenticate() as route middleware to authenticate the
//   request.  The first step in OpenID authentication will involve redirecting
//   the user to their OpenID provider.  After authenticating, the OpenID
//   provider will redirect the user back to this application at
//   /auth/openid/return

app.get('/auth/openid',
  passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
  function(req, res) {
    log.info('Authenitcation was called in the Sample');
    res.redirect('/');
  });

// GET /auth/openid/return
//   Use passport.authenticate() as route middleware to authenticate the
//   request.  If authentication fails, the user will be redirected back to the
//   login page.  Otherwise, the primary route function function will be called,
//   which, in this example, will redirect the user to the home page.
app.get('/auth/openid/return',
  passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
  function(req, res) {

    res.redirect('/');
  });

// POST /auth/openid/return
//   Use passport.authenticate() as route middleware to authenticate the
//   request.  If authentication fails, the user will be redirected back to the
//   login page.  Otherwise, the primary route function function will be called,
//   which, in this example, will redirect the user to the home page.

app.post('/auth/openid/return',
  passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
  function(req, res) {

    res.redirect('/');
  });
```

## <a name="4-use-passport-to-issue-sign-in-and-sign-out-requests-to-azure-ad"></a>4. Kirjaudu sisään ja kirjaudu ulos pyynnöt antamaan Azure AD Passport Käytä

Sovellus on nyt määritetty oikein pitää yhteyttä v2.0 päätepisteen OpenID yhteyden todennus-protokollan avulla.  `passport-azure-ad`on otettava varoen kaikkien työstämistä todennus viestejä, vahvistaminen Azure AD tunnusta ja ylläpito käyttäjäistunnon rumalta tietoja.  Kaikki pysyy myös antaa käyttäjille voi kirjautua sisään, kirjaudu ulos ja kerää lisätietoja kirjautuneena-käyttäjä.

- Sallii ensin Lisää oletusarvon, kirjaudu sisään, tili ja kirjaudu menetelmät Microsoftin `app.js` tiedosto:

```JavaScript

//Routes (Section 4)

app.get('/', function(req, res){
  res.render('index', { user: req.user });
});

app.get('/account', ensureAuthenticated, function(req, res){
  res.render('account', { user: req.user });
});

app.get('/login',
  passport.authenticate('azuread-openidconnect', { failureRedirect: '/login' }),
  function(req, res) {
    log.info('Login was called in the Sample');
    res.redirect('/');
});

app.get('/logout', function(req, res){
  req.logout();
  res.redirect('/');
});

```

-   Seuraavaksi tarkastellaan nämä tiedot:
    -   `/` Reitin ohjaa index.ejs näkymän kulkeva käyttäjän pyynnön, (jos olemassa)
    - `/account` Reititys on ensimmäinen, ***Varmista, että todennetaan*** (on pantava täytäntöön, alapuolella) ja siirtää sitten käyttäjän pyynnön niin, että olemme Saat lisätietoja käyttäjän.
    - `/login` Reitin soittavat Microsoftin azuread openidconnect todentaja- `passport-azuread` ja jos, joka ei onnistu ohjaa käyttäjän takaisin /login
    - `/logout` Vain Soita logout.ejs (ja reitittää) tyhjentää evästeet ja palaa käyttäjän index.ejs


- Edellisen osan `app.js`, lisääminen EnsureAuthenticated menetelmä, jota käytetään `/account` yläpuolella.

```JavaScript

// Simple route middleware to ensure user is authenticated. (Section 4)

//   Use this route middleware on any resource that needs to be protected.  If
//   the request is authenticated (typically via a persistent login session),
//   the request will proceed.  Otherwise, the user will be redirected to the
//   login page.
function ensureAuthenticated(req, res, next) {
  if (req.isAuthenticated()) { return next(); }
  res.redirect('/login')
}

```

- Lopuksi todella luominen itse palvelin- `app.js`:

```JavaScript

app.listen(3000);

```


## <a name="5-create-the-views-and-routes-in-express-to-display-our-user-in-the-website"></a>5. luominen näkymät ja tiet pika, jos haluat näyttää tämän käyttäjän sivustossa

On Microsoftin `app.js` valmis. Nyt vain annettava Lisää tiet ja näkymät, jossa näkyvät tiedot on siirtyä käyttäjän sekä kahvaa `/logout` ja `/login` tiet, että olet luonut.

- Luo `/routes/index.js` reitin pääkansioon.

```JavaScript

/*
 * GET home page.
 */

exports.index = function(req, res){
  res.render('index', { title: 'Express' });
};
```

- Luo `/routes/user.js` reitin pääkansioon

```JavaScript

/*
 * GET users listing.
 */

exports.list = function(req, res){
  res.send("respond with a resource");
};
```

Nämä yksinkertainen tiet vain välittää pyynnön pitkin Microsoftin näkymään, mukaan lukien käyttäjä, jos se on valittavissa.

- Luo `/views/index.ejs` näkymän pääkansioon. Tämä on yksinkertainen sivu, joka kutsua Microsoftin sisään- ja uloskirjautuminen menetelmiä ja Salli käytetty tilin tiedot. Huomaa, että Microsoft käyttää ehdollista `if (!user)` käyttäjänä välitettävä kautta pyynnössä on on kirjautunut käyttäjä näyttöä.

```JavaScript
<% if (!user) { %>
    <h2>Welcome! Please log in.</h2>
    <a href="/login">Log In</a>
<% } else { %>
    <h2>Hello, <%= user.displayName %>.</h2>
    <a href="/account">Account Info</a></br>
    <a href="/logout">Log Out</a>
<% } %>
```

- Luo `/views/account.ejs` tarkastella pääkansioon niin, että olemme voit tarkastella lisätietoja, `passport-azuread` on sijoittaa käyttäjän pyynnöt.

```Javascript
<% if (!user) { %>
    <h2>Welcome! Please log in.</h2>
    <a href="/login">Log In</a>
<% } else { %>
<p>displayName: <%= user.displayName %></p>
<p>givenName: <%= user.name.givenName %></p>
<p>familyName: <%= user.name.familyName %></p>
<p>UPN: <%= user._json.upn %></p>
<p>Profile ID: <%= user.id %></p>
<p>Full Claimes</p>
<%- JSON.stringify(user) %>
<p></p>
<a href="/logout">Log Out</a>
<% } %>
```

- Lopuksi japanin tehdä tämän ulkoasun melko lisäämällä asettelu. Luo "/ views/layout.ejs' Näytä pääkansioon

```HTML

<!DOCTYPE html>
<html>
    <head>
        <title>Passport-OpenID Example</title>
    </head>
    <body>
        <% if (!user) { %>
            <p>
            <a href="/">Home</a> |
            <a href="/login">Log In</a>
            </p>
        <% } else { %>
            <p>
            <a href="/">Home</a> |
            <a href="/account">Account</a> |
            <a href="/logout">Log Out</a>
            </p>
        <% } %>
        <%- body %>
    </body>
</html>
```

Lopuksi luominen ja suorittaminen sovelluksen!

Suorita `node app.js` ja siirry`http://localhost:3000`


Kirjaudu sisään Microsoft-Account tai työpaikan tai oppilaitoksen tiliä, niin huomaat, miten käyttäjän tunnistetiedot näkyy /account luettelossa.  Nyt on suojattu alan vakio protokollia, jotka voi todentaa niiden henkilökohtainen ja työpaikan tai oppilaitoksen tilin käyttäjille verkkosovellukseen.

##<a name="next-steps"></a>Seuraavat vaiheet

Viitteen Esimerkki valmiista (ilman määritysarvot) [tähän .zip taulukkona](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs/archive/complete.zip), tai voit voit käytetään sen GitHub:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs.git```

Voit nyt siirtää aiheita sivulle.  Voit halutessasi kokeilla:

[Suojattu node.js verkko-ohjelmointirajapinnan käyttäminen v2.0 päätepisteen >>](active-directory-v2-devquickstarts-node-api.md)

Muita tietolähteitä Kuittaa ulos:
- [V2.0 Sovelluskehittäjän opas >>](active-directory-appmodel-v2-overview.md)
- [StackOverflow "azure-active directory-tunniste >>](http://stackoverflow.com/questions/tagged/azure-active-directory)

## <a name="get-security-updates-for-our-products"></a>Päivitysten hakeminen tuotteitamme

On suositeltavaa, kun suojauksen tapaukset esiintyvät käymällä [Tämän sivun](https://technet.microsoft.com/security/dd252948) ja tilaamalla neuvoa suojausvaroitusten ilmoitusten saaminen.
