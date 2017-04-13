###<a name="server-auth"></a>Toimintaohje: todennus palveluntarjoajalta (palvelimen virtaus)

Jotta Mobile-sovellusten hallinta-sovelluksen todennusprosessin, sinun on rekisteröitävä sovelluksen tunnistetietojen toimittajaan. Valitse Azure-sovelluksen palvelussa tarvitset Sovellustunnus ja palveluntarjoajan myöntämä salaisuus määrittäminen
Lisätietoja on artikkelissa opetusohjelma [todennus-sovelluksen Lisää].

Kun olet rekisteröinyt tunnistetietojen toimittaja, Soita .login() menetelmää tarjoajan nimi. Jos esimerkiksi haluat kirjautua sisään ja Facebook seuraava koodi.

```
client.login("facebook").done(function (results) {
     alert("You are now logged in as: " + results.userId);
}, function (err) {
     alert("Error: " + err);
});
```

Jos käytössäsi on muu kuin Facebook tunnistetietojen toimittaja, muuttaa välitetty kirjautuminen noudattamalla jokin seuraavista toimista arvo: `microsoftaccount`, `facebook`, `twitter`, `google`, tai `aad`.

Tässä tapauksessa Azure App palvelun hallitsee OAuth 2.0 todennus kulun näyttämällä valitun palvelun kirjautumissivun ja luodaan sovelluksen palvelun todennus-tunnuksen tunnistetietojen toimittajaan onnistumisen kirjautumisen jälkeen. Sisäänkirjautuminen-funktio, kun olet valmis, palauttaa JSON-objekti (käyttäjä), joka paljastaa Käyttäjätunnus- ja sovelluksen palvelun todennus tunnuksen käyttäjätunnus- ja authenticationToken-kenttiin tarpeen mukaan. Tunnuksessa voidaan välimuistin ja käyttää sitä uudelleen, ennen kuin se päättyy.

###<a name="client-auth"></a>Toimintaohje: todennus palveluntarjoajalta (asiakas-työnkulku)

Sovelluksen voit myös itsenäisesti tunnistetietojen tarjoajalta ja anna palautetut tunnuksen sovelluksen-palvelun todennusta varten. Asiakas-työnkulku mahdollistaa yhden kirjautuminen tarjota käyttäjille tai muokkaalinkki tietojen noutaminen tunnistetietojen toimittaja.

#### <a name="social-authentication-basic-example"></a>Sosiaalisten basic todennus-Esimerkki

Tässä esimerkissä käytetään Facebook-asiakkaan SDK todennusta varten:

```
client.login(
     "facebook",
     {"access_token": token})
.done(function (results) {
     alert("You are now logged in as: " + results.userId);
}, function (err) {
     alert("Error: " + err);
});
```
Tässä esimerkissä oletetaan, että tunnuksen muuttujan on tallennettu vastaaviin tarjoaja SDK annettu tunnus.

#### <a name="microsoft-account-example"></a>Microsoft-Account Esimerkki

Seuraavassa esimerkissä Live SDK, joka tukee single-Sign for Windows-kaupan sovellukset käyttämällä Microsoft-Account:

```
WL.login({ scope: "wl.basic"}).then(function (result) {
      client.login(
            "microsoftaccount",
            {"authenticationToken": result.session.authentication_token})
      .done(function(results){
            alert("You are now logged in as: " + results.userId);
      },
      function(error){
            alert("Error: " + err);
      });
});
```

Tässä esimerkissä hakee tunnusta Live yhteyden, joka toimitetaan App palveluun kutsumalla kirjautuminen-funktiota.

###<a name="auth-getinfo"></a>Toimintaohje: hankkia todennetun käyttäjän tiedot

Nykyisen käyttäjän todennustiedot voit noutaa `/.auth/me` päätepisteen millä tahansa AJAX-menetelmällä.  Varmistaa, voit määrittää `X-ZUMO-AUTH` todennus-tunnuksen otsikkoa.  Todennus-tunnuksen tallennetaan `client.currentUser.mobileServiceAuthenticationToken`.  Jos esimerkiksi haluat käyttää Nouda API:

```
var url = client.applicationUrl + '/.auth/me';
var headers = new Headers();
headers.append('X-ZUMO-AUTH', client.currentUser.mobileServiceAuthenticationToken);
fetch(url, { headers: headers })
    .then(function (data) {
        return data.json()
    }).then(function (user) {
        // The user object contains the claims for the authenticated user
    });
```

Nouda on käytettävissä npm-pakettina ja CDNJS selaimen ladattavaksi. Voit noutaa tietoja voi käyttää myös jQuery tai toisen AJAX API.  Tietoja vastaanotetaan JSON-objekteina.
