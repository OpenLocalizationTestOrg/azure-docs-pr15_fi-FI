<properties 
    pageTitle="Ääni-, VoIP- ja Tekstiviestejä käytettäessä Azure Twilio käyttäminen" 
    description="Opettele puhelun soittaminen ja lähettää Azure tekstiviesti Twilio API-palvelussa. Kirjoitettu Node.js MALLIKOODEJA." 
    services="" 
    documentationCenter="nodejs" 
    authors="devinrader" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs" 
    ms.topic="article" 
    ms.date="11/25/2014" 
    ms.author="wpickett"/>


# <a name="using-twilio-for-voice-voip-and-sms-messaging-in-azure"></a>Ääni-, VoIP- ja Tekstiviestejä käytettäessä Azure Twilio käyttäminen

Tässä oppaassa kerrotaan, miten voit luoda sovellukset, jotka yhteydenpito Twilio ja node.js Azure.

<a id="whatis"/>
## <a name="what-is-twilio"></a>Mikä on Twilio?

Twilio on API-ympäristö, joka on helppo tehdä ja puheluiden vastaanottaminen, lähettää ja vastaanottaa tekstiviestejä ja upota VoIP soitat selainpohjaisia ja alkuperäisen mobile-sovellusten kehittäjille.  Oletetaan, että lyhyesti Siirry päälle, miten tämä toimii ennen Sukeltava.

### <a name="receiving-calls-and-text-messages"></a>Lähettää ja vastaanottaa puheluita ja tekstiviestit

Twilio tällä ohjelmalla ohjelmistokehittäjät voivat [hankkia ohjelmoitava puhelinnumerot] [ purchase_phone] joka voidaan lähettää ja vastaanottaa puheluita ja viestejä.  Kun Twilio luvun vastaanottaa saapuvan puhelun tai tekstin, Twilio lähettää web-sovelluksen HTTP POST tai GET-pyynnössä pyytää ohjeita siitä, miten voit käsitellä puhelun tai tekstin.  Palvelimen reagoi Twilio's HTTP-pyyntö, jonka [TwiML][twiml], XML-tunnisteita, jotka sisältävät ohjeet puhelun tai tekstin käsitteleminen yksinkertainen joukko.  Olemme näkevät TwiML esimerkkejä hetkinen.

### <a name="making-calls-and-sending-text-messages"></a>Puheluiden soittamiseen ja tekstiviestien lähettäminen

Tekemällä pyyntöjen Twilio web service API kehittäjät voi lähettää tekstiviestejä tai aloittaa lähtevät puhelut.  Lähtevien puhelujen kehittäjä on myös määritettävä URL-Osoitetta, joka palauttaa TwiML ohjeet käsittelemisestä lähtevä puhelu, kun se on yhdistetty.

### <a name="embedding-voip-capabilities-in-ui-code-javascript-ios-or-android"></a>VoIP-ominaisuuksia upottamisen Käyttöliittymän koodissa (JavaScript-, iOS- tai Android)

Twilio on asiakkaan SDK joka muuttaa työpöydän web-selaimessa, iOS-sovelluksen tai Android-sovelluksen VoIP-puhelimen.  Tässä artikkelissa keskitytään käyttämisestä VoIP kutsumista selaimessa.  Lisäksi Twilio-JavaScript SDK käynnissä selaimessa palvelinpuolen-sovellus (Microsoftin node.js sovelluksen) on käytettävä "ominaisuuksien tunnuksen" antamaan JavaScript-asiakas.  Voit lukea lisää VoIP käyttäminen node.js [Twilio keskihajonta blogista][voipnode].

<a id="signup"/>
## <a name="sign-up-for-twilio-microsoft-discount"></a>Rekisteröidy Twilio (Microsoft alennuksen)

Ennen kuin käytät Twilio palveluja, sinun täytyy ensimmäisen [tilin rekisteröityminen][signup].  Microsoft Azure-asiakkaiden käyttämien määräten alennus - [että rekisteröintiä tähän][signup]!

<a id="azuresite"/>
## <a name="create-and-deploy-a-nodejs-azure-website"></a>Luoda ja ottaa käyttöön node.js Azure-sivusto

Seuraavaksi sinun on käytössä Azure node.js verkkosivuston luonnin.  [Virallinen ohjeissa tekoa sijaitsee seuraavassa][azure_new_site].  Korkean tason sinun olla seuraavasti:

* Jos sitä ei vielä ole Azure-tili rekisteröityminen
* Azure hallintakonsoli avulla voit luoda uuden sivuston
* Tietolähteen ohjausobjektin tuen (olemme olettaa käytit git) lisääminen
* Tiedoston luominen `server.js` yksinkertainen node.js web-sovelluksen kanssa
* Tämän yksinkertaisen Azure sovelluksen käyttöönotto

<a id="twiliomodule"/>
## <a name="configure-the-twilio-module"></a>Määritä Twilio-moduuli

Seuraavaksi on alkaa Kirjoita yksinkertainen node.js-sovellus, joka tekee Twilio Ohjelmointirajapinnan käyttäminen.  Ennen kuin on aloittaa, annettava määrittäminen usealle Twilio tilin tunnistetiedot.  

### <a name="configuring-twilio-credentials-in-system-environment-variables"></a>Määrittäminen Twilio tunnistetiedot järjestelmän ympäristömuuttujat

Siirrä Twilio takaisin loppuun todennetut pyynnöt annettava sekä tilin SUOJAUSTUNNUS ja auth tunnuksen, mitkä funktioon käyttäjänimi ja salasana, tutustu Twilio-tilin määrittäminen. Määritä nämä käytettäväksi Azure solmu-moduulissa turvallisin tapa on järjestelmän ympäristömuuttujia, jotka voit määrittää suoraan Azure järjestelmänvalvojan konsolissa.

Valitse node.js-sivustoon ja sitten "Määrittäminen"-linkki.  Jos voit vierittää alaspäin vähän, näet aluetta, jossa voit määrittää sovelluksen asetukset.  Kirjoita Twilio tilin tunnukset ([löytyy Twilio Raporttinäkymät-ikkunan][twilio_dashboard]) – kuten Varmista, että nimeät ne "TWILIO_ACCOUNT_SID" ja "TWILIO_AUTH_TOKEN" tarpeen mukaan:

![Azure hallintakonsoli][azure-admin-console]

Kun olet määrittänyt nämä muuttujat, Käynnistä sovellus uudelleen Azure-konsolissa.

### <a name="declaring-the-twilio-module-in-packagejson"></a>Määritteleminen package.json Twilio-moduuli

Seuraavaksi annettava luominen package.json hallittavan Microsoftin solmu moduulin riippuvuudet [npm]kautta.  "Server.js"-tiedostona, jonka loit opetusohjelmassa Azure/node.js samalla tasolla Luo tiedosto nimeltä "package.json".  Tämän tiedoston sisälle sijoittaa seuraavasti:

  {"nimi": "sovelluksen-nimi", "version": "0.0.1", "Yksityinen": TOSI, "komentosarjojen": {"Käynnistä-:"solmu palvelin"},"riippuvuudet": {"express":"3.1.0","ejs":"*","twilio":"*"}}

Tämä ilmoittaa twilio moduulin kuin riippuvuus sekä Suositut [express web framework] [ express] ja EJS mallia moduuli.  Aloitetaan nyt on valmis - oletetaan, että jotkin koodin kirjoittaminen!

<a id="makecall"/>
## <a name="make-an-outbound-call"></a>Lähtevän soittaminen

Luodaan Yksinkertainen lomake, joka soittamaan puhelua luku on valita.  Avaa server.js ja kirjoita seuraava koodi.  Huomautus, jossa lukee "CHANGE_ME" - Laita azure sivuston nimi on:

    // Module dependencies
    var express = require('express'), 
      path = require('path'), 
      http = require('http'), 
      twilio = require('twilio');

    // Create Express web application
    var app = express();

    // Express configuration
    app.configure(function(){
      app.set('port', process.env.PORT || 3000);
      app.set('views', __dirname + '/views');
      app.set('view engine', 'ejs');
      app.use(express.favicon());
      app.use(express.logger('dev'));
      app.use(express.bodyParser());
      app.use(express.methodOverride());
      app.use(app.router);
      app.use(express.static(path.join(__dirname, 'public')));
    });
    app.configure('development', function(){
      app.use(express.errorHandler());
    });

    // Render an HTML user interface for the application's home page
    app.get('/', function(request, response) {
      response.render('index');
    });

    // Handle the form POST to place a call
    app.post('/call', function(request, response) {
      var client = twilio();
      client.makeCall({
          // make a call to this number
          to:request.body.number,

          // Change to a Twilio number you bought - see:
          // https://www.twilio.com/user/account/phone-numbers/incoming
          from:'+15558675309',

          // A URL in our app which generates TwiML
          // Change "CHANGE_ME" to your app's name
          url:'https://CHANGE_ME.azurewebsites.net/outbound_call'
      }, function(error, data) {
          // Go back to the home page
          response.redirect('/');
      });
    });

    // Generate TwiML to handle an outbound call
    app.post('/outbound_call', function(request, response) {
      var twiml = new twilio.TwimlResponse();

      // Say a message to the call's receiver 
      twiml.say('hello - thanks for checking out Twilio and Azure', {
          voice:'woman'
      });

      response.set('Content-Type', 'text/xml');
      response.send(twiml.toString());
    });

    // Start server
    http.createServer(app).listen(app.get('port'), function(){
      console.log("Express server listening on port " + app.get('port'));
    });

Seuraavaksi Luo kansio nimeltä "näkymät" - kansion sisällä, Luo tiedosto nimeltä "index.ejs" seuraavat sisältöön:

    <!DOCTYPE html>
    <html>
    <head>
      <title>Twilio Test</title>
      <style>
        input { height:20px; width:300px; font-size:18px; margin:5px; padding:5px; }
      </style>
    </head>
    <body>
      <h1>Twilio Test</h1>
      <form action="/call" method="POST">
          <input placeholder="Enter a phone number" name="number"/>
          <br/>
          <input type="submit" value="Call the number above"/>
      </form>
    </body>
    </html>

Nyt sivuston käyttöön Azure ja avaa kotona.  Sinun pitäisi Anna puhelinnumero tekstikenttään ja vastaanotat puhelun Twilio numero!

<a id="sendmessage"/>
## <a name="send-an-sms-message"></a>Tekstiviestin lähettäminen

Nyt määrittäminen japanin käyttöliittymän ja lomakkeen käsittely logiikan Tekstiviestin lähettäminen.  Avaa "server.js" ja lisää seuraava koodi "app.post" viimeisen puhelu jälkeen:

    app.post('/sms', function(request, response) {
      var client = twilio();
      client.sendSms({
          // send a text to this number
          to:request.body.number,

          // A Twilio number you bought - see:
          // https://www.twilio.com/user/account/phone-numbers/incoming
          from:'+15558675309',

          // The body of the text message
          body: request.body.message
          
      }, function(error, data) {
          // Go back to the home page
          response.redirect('/');
      });
    });

Lisää "views/index.ejs", valitse ensimmäinen luku ja Tekstiviestin lähettäminen toisesta lomakkeesta:

    <form action="/sms" method="POST">
      <input placeholder="Enter a phone number" name="number"/>
      <br/>
      <input placeholder="Enter a message to send" name="message"/>
      <br/>
      <input type="submit" value="Send text to the number above"/>
    </form>

Ota sovelluksesi Azure uudelleen ja lähettää lomakkeen ja lähettää itsellesi (tai jokin lähimmän ystäviesi) tekstiviestin pitäisi nyt!

<a id="nextsteps"/>
## <a name="next-steps"></a>Seuraavat vaiheet

Opit on nyt sovellukset, jotka yhteyden muodostamiseen node.js ja Twilio käytön perusteet.  Mutta näitä esimerkkejä scratch hankala edes erottaa pinta tarjoamiin mahdollisuuksiin Twilio ja node.js.  Katso lisätietoja Twilio käyttäminen node.js on seuraavissa resursseissa:

* [Virallinen moduulin asiakirjoja][docs]
* [VoIP-opetusohjelma node.js-sovellusten kanssa][voipnode]
* [Votr - reaaliaikainen SMS, äänestyksen sovelluksen node.js ja CouchDB (kolme osaa)][votr]
* [Pari ohjelmoinnin node.js kanssa selaimessa][pair]

Toivottavasti löytämistä luvaton käyttö node.js ja Twilio Azure!

[purchase_phone]: https://www.twilio.com/user/account/phone-numbers/available/local
[twiml]: https://www.twilio.com/docs/api/twiml
[signup]: http://ahoy.twilio.com/azure
[azure_new_site]: /app-service-web/web-sites-nodejs-develop-deploy-mac.md
[twilio_dashboard]: https://www.twilio.com/user/account
[npm]: http://npmjs.org
[express]: http://expressjs.com
[voipnode]: http://www.twilio.com/blog/2013/04/introduction-to-twilio-client-with-node-js.html
[docs]: http://twilio.github.io/twilio-node/
[votr]: http://www.twilio.com/blog/2012/09/building-a-real-time-sms-voting-app-part-1-node-js-couchdb.html
[pair]: http://www.twilio.com/blog/2013/06/pair-programming-in-the-browser-with-twilio.html
[azure-admin-console]: ./media/partner-twilio-nodejs-how-to-use-voice-sms/twilio_1.png



