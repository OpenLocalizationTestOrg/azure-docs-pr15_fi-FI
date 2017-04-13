<properties
    pageTitle="Node.js keskustelu-sovelluksen luominen Socket.IO Azure sovelluksen-palvelun kanssa"
    description="Opetusohjelma, joka osoittaa socket.io käyttämisestä isännöimät Azure node.js verkkosovellukseen."
    services="app-service\web"
    documentationCenter="nodejs"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

# <a name="create-a-nodejs-chat-application-with-socketio-in-azure-app-service"></a>Node.js keskustelu-sovelluksen luominen Socket.IO Azure sovelluksen-palvelun kanssa

Socket.IO sisältää reaaliaikaisen viestinnän node.js palvelimen ja asiakkaita käyttämällä WebSockets välillä. Se tukee myös varmistusta muiden tiedonsiirron (esimerkiksi pitkän kysely), jotka toimivat vanhemmat selaimet. Tässä opetusohjelmassa opastusta isännöinnin Socket.IO mukaan keskustelu-sovelluksen kuin Azure web app- ja kuinka voit skaalata sovelluksen [Azure Redis välimuistin]. Saat lisätietoja Socket.IO <http://socket.io/>.

> [AZURE.NOTE] Tämän tehtävän ohjeet koskevat [Palvelua Web sovellukset]; Katso pilvipalveluihin, [Muodosta Node.js Chat sovellus, jolla Socket.IO-Azure-pilvipalvelussa].

## <a name="download-the-chat-example"></a>Lataa keskustelu-Esimerkki

Käytämme [Socket.IO GitHub säilöön]keskustelu esimerkiksi tälle projektille. Seuraavien toimien esimerkin lataaminen ja sen lisääminen aiemmin luotu projekti.

1.  Lataa [ZIP- tai GZ arkistoida release] Socket.IO projektin (versio 1.3.5 käytettiin tämä asiakirja)

1.  Pura arkisto- ja kopio **esimerkkejä\\keskustelu** kansio uuteen paikkaan. Esimerkiksi ** \\solmu\\keskustelun**.

## <a name="modify-appjs-and-install-modules"></a>Muokkaa app.js ja asenna moduulit

1.  Nimeä tiedosto **index.js** **app.js**. Näin Azure vianmäärityksen, että se Node.js-sovelluksen avulla.

1.  Avaa tekstieditorissa **app.js** -tiedosto. Muuta rivin sisältävän `var io = require('../..')(server);` alla kuvatulla tavalla:

        var express = require('express');
        var app = express();
        var server = require('http').createServer(app);
        // var io = require('../..')(server);
        // New:
        var io = require('socket.io')(server);
        var port = process.env.PORT || 3000;

1. Avaa **package.json** -tiedosto ja Lisää viittaus-kohdassa socket.io `dependencies`, alla kuvatulla tavalla:

        "dependencies": {
          "express": "3.4.8",
          "socket.io": "1.3.5"
        }

1. Komentorivin, valitse Muuta ** \\solmu\\keskustelu** hakemistosta ja käytä npm moduulit vaatii tämän sovelluksen asentamiseksi:

        npm install

    Moduulit asennetaan nimeltä **node_modules**alikansioon.

## <a name="create-an-azure-web-app"></a>Azure-Web-sovelluksen luominen

Azure web-sovelluksen luominen, Git julkaisemisen ottaminen käyttöön ja valitse Ota käyttöön WebSocket tuesta web app seuraavasti.

> [AZURE.NOTE] Jotta voit suorittaa tässä opetusohjelmassa, sinun on Azure-tili. Jos sinulla ei ole tiliä, voit luoda ilmainen kokeiluversio tili vain muutaman minuutin. Lisätietoja on artikkelissa <a href="http://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A7171371E" target="_blank">Azure maksuttoman kokeiluversion</a>.

1. Asenna Azure komentorivivalitsimet Interface (Azure CLI) ja liitä Azure-tilaukseen. Katso, [Asenna ja Määritä Azure CLI](../xplat-cli-install.md).

1. Jos kyseessä on ensimmäisen kerran määritettävän säilön Azure-tietokannassa, haluat luoda kirjautumistiedot. FROM Azure-CLI Kirjoita seuraava komento:

        azure site deployment user set [username] [password]

1. Voit muuttaa ** \\node\chat** hakemistosta ja käytä seuraava komento luo uusi Azure web app ja paikallisen Git säilö. Tämä komento luo myös Git remote nimettyä 'azure".

        azure site create mysitename --git

    'Mysitename' on tilalle koodiin yksilöllinen nimi.

1. Vahvista aiemmin luotujen tiedostojen paikallinen säilöön käyttämällä seuraavia komentoja:

        git add .
        git commit -m "Initial commit"

1. Push tiedostoja Azure Web Apps-säilöön seuraavalla komennolla:

        git push azure master

    Anna tunnistetiedot vaiheessa 2. Saat tilasanomat, kuten moduulit tuodaan palvelimessa. Kun tämä prosessi on valmis, sovelluksen sijaittava Azure koodiin.

    > [AZURE.NOTE] Moduulin asennuksen aikana, saatat huomata virheitä, ' tuodut projektia ei löydy ". Nämä turvallisesti voi ohittaa.

1. Socket.IO käyttää WebSockets, jotka eivät ole käytössä oletusarvoisesti Azure. Web-sockets käyttöön seuraavalla komennolla:

        azure site set -w

    Kirjoita pyydettäessä web-sovelluksen nimeä.

    >[AZURE.NOTE]
    >"Azure sivuston määrittäminen -w-komennon avautuu toimivat vain versio 0.7.4 tai uudempi Azure komentorivivalitsimet käyttöliittymän. Voit myös ottaa WebSocket tuki [Azure-portaalissa](https://portal.azure.com).
    >
    >Jotta WebSockets Azure-portaalissa, valitse Web Apps-sivu web-sovellus, valitse **kaikki asetukset** > **asetukset**. Valitse **Web Sockets**, **Valitse**. Valitse **Tallenna**.

1. Saat näkyviin Azure-web-sovelluksen avulla voit käynnistää selaimen ja siirtyä isännöityä web App-sovellukseen seuraava komento:

        azure site browse

Sovellus on nyt käytössä Azure ja voit välittää keskustelun viestejä eri asiakkaita käyttämällä Socket.IO välillä.

## <a name="scale-out"></a>Skaalaa ulos

Socket.IO sovellukset voi skaalata __sovittimen__ avulla voit jakaa viestit ja tapahtumia useita Palvelusovellusten esiintymät välillä. Kun käytettävissä on useita sovittimia, [socket.io Redis.txt] sovittimen avulla voidaan helposti Azure Redis välimuisti-ominaisuudella.

> [AZURE.NOTE] Lisää tarve laajentaminen Socket.IO ratkaisu on muistilappuja istuntojen tuki. Oletusarvon mukaan käytössä muistilappuja istuntojen kautta Azure pyytää reititys Azure-verkkosovelluksissa. Lisätietoja on artikkelissa [Esiintymän affiniteetti Azure sivustoja].

### <a name="create-a-redis-cache"></a>Luoda Redis.txt välimuistin

Suorita vaiheet [Luo välimuistin Azure Redis välimuistiin] , jos haluat luoda uuden välimuistin.

> [AZURE.NOTE] Tallentaa __isäntänimi__ ja __perusavaimen__ välimuistin, nämä tarvittaessa seuraavia vaiheita.

### <a name="add-the-redis-and-socketio-redis-modules"></a>Lisää Redis.txt ja socket.io Redis.txt moduulit

1. Komentoriviltä, muuttaa __ \\solmu\\keskustelu__ hakemistosta ja käytä seuraavaa komentoa.

        npm install socket.io-redis@0.1.4 redis@0.12.1 --save

    > [AZURE.NOTE] Tällä komennolla määritettyjä versioita käytetään, kun tämän artikkelin testaaminen versioita.

1. Lisää seuraavat __app.js__ -tiedoston muokkaaminen heti, kun rivejä`var io = require('socket.io')(server);`

        var pub = require('redis').createClient(6379,'redishostname', {auth_pass: 'rediskey', return_buffers: true});
        var sub = require('redis').createClient(6379,'redishostname', {auth_pass: 'rediskey', return_buffers: true});

        var redis = require('socket.io-redis');
        io.adapter(redis({pubClient: pub, subClient: sub}));

    Korvaa __redishostname__ ja __rediskey__ isäntänimi ja Redis.txt välimuistin avain.

    Tämä luo julkaisemisen ja asiakkaan aiemmin luotu Redis.txt välimuistiin tilaa. Asiakkaiden sitten käytetään sovittimen määrittäminen käyttämään Redis.txt välimuistin kulkeva viestit ja tapahtumia välisten esiintymien sovelluksen Socket.IO

    > [AZURE.NOTE] Kun siirtyä Redis.txt viestiä __socket.io Redis.txt__ -sovittimen, nykyinen versio ei tue Azure Redis välimuistin vaatii todennusta. Niin yhteyden luodaan __redis__ -moduulin ja valitse asiakkaan välitetään __socket.io Redis.txt__ sovittimen.
    >
    > Kun Azure Redis välimuistin tukee porttia 6380 suojattuja yhteyksiä, tässä esimerkissä käytetään moduulit eivät tue suojattuja yhteyksiä saavuttaman 7/14/2014. Yllä koodi käyttää oletusarvoisen-6379 suojaamaton portin.

1. Tallenna muokattu __app.js__

### <a name="commit-changes-and-redeploy"></a>Vahvista muutokset ja ota uudelleen

Komentorivin stä __ \\solmu\\keskustelu__ kansioon, käyttää seuraavia komentoja Vahvista muutokset ja ota sovellus uudelleen.

    git add .
    git commit -m "implementing scale out"
    git push azure master

Kun muutokset on tehty miten palvelimeen, voit skaalata sivuston yli useita kertoja käyttämällä seuraavaa komentoa.

    azure site scale instances --instances #

Jos __#__ määrä esiintymien luomiseen.

Voit muodostaa yhteyden koodiin usean selaimet tai voit varmistaa, että viestit lähetetään oikein kaikkiin tietokoneisiin.

## <a name="troubleshooting"></a>Vianmääritys

### <a name="connection-limits"></a>Yhteysrajoitukset

Azure Web Apps-sovelluksista on käytettävissä useita tuotteissa, jotka määrittävät resurssit-sivustoon. Tämä sisältää sallittujen WebSocket yhteyksien määrä. Lisätietoja on artikkelissa [Web Apps hinnat sivun].

### <a name="messages-arent-being-sent-using-websockets"></a>Viestit eivät ole on lähetetty WebSockets

Jos asiakkaan selaimet säilyttää kuuluvien takaisin sijaan WebSockets pitkään kyselyt voi olla jokin seuraavista toimista.

* **Kokeile rajoittaminen vain WebSockets, lähetys**

    Jotta Socket.IO käytettävä WebSockets tekstiviesti transport palvelimen ja asiakkaan tuettava WebSockets. Jos toisen ei tue, Socket.IO neuvottelemalla toiseen transport, kuten pitkä kysely. Tiedonsiirron Socket.IO käyttämä luetteloa ` websocket, htmlfile, xhr-polling, jsonp-polling`. Voit pakottaa käyttämään vain WebSockets lisäämällä **app.js** -tiedosto seuraava koodi jälkeen sisältävän rivin `, nicknames = {};`.

        io.configure(function() {
          io.set('transports', ['websocket']);
        });

    > [AZURE.NOTE] Huomaa, että vanhemmat selaimet tue WebSockets voi muodostaa yhteyttä sivustoon samalla, kun edellä koodi on aktiivinen, kuin se rajoittaa WebSockets vain tietoliikenne.

* **Käytä SSL: ää**

    WebSockets on riippuvainen joitakin suppeampaan käytetyt HTTP-otsikot, kuten **päivittäminen** -otsikon. Keskitason joitakin verkkolaitteita, kuten web-välityspalvelimet voivat poistaa nämä otsikot. Voit välttää tämän ongelman, voit muodostaa WebSocket yhteys SSL: n avulla.

    Helppo tapa tehdä tämä on määritettävä Socket.IO, `match origin protocol`. Tämän tiedon Socket.IO suojaamiseen WebSockets viestintä sama kuin alkuperäiseen HTTP/HTTPS-pyyntö web-sivulle. Jos selaimessa käytetään HTTPS URL-osoitteen Siirry sivustoon, seuraavien WebSocket viestinnän Socket.IO kautta voidaan suojata SSL-yhteyden kautta.

    Tässä esimerkissä voit ottaa tämän määrityksen, lisätä seuraava koodi **app.js** tiedoston jälkeen sisältävän rivin `, nicknames = {};`.

        io.configure(function() {
          io.set('match origin protocol', true);
        });

* **Web.config asetusten tarkistaminen**

    Azure verkkosovelluksissa Node.js sovelluksia isännöivät **seuraavan koodin korostetut** avulla voit reitittää pyynnöt Node.js-sovellus. WebSockets toimisi oikein Node.js-sovellusten kanssa **web.config** on sisällettävä Seuraava.

        <webSocket enabled="false"/>

    Tämä poistaa käytöstä IIS WebSockets-moduulissa, mukaan lukien oma WebSockets ja ristiriitojen Node.js tietyn WebSocket moduulit esimerkiksi Socket.IO soveltaminen. Jos tämä rivi ei ole käytettävissä tai on määritetty `true`, tämä saattaa johtua WebSocket transport ei toimi sovelluksen syy.

    Node.js sovellusten tavallisesti sisällä **web.config** -tiedoston, joten Azure sivustoja luo automaattisesti yksi Node.js sovellusten kun ne on otettu käyttöön. Koska tämä tiedosto luodaan automaattisesti palvelimeen, sinun on käytettävä FTP- tai sivuston URL-osoite FTPS voit tarkastella tätä tiedostoa. Voit etsiä FTPS URL- ja FTP perinteinen portaalissa sivuston valitsemalla web-sovelluksen ja **Dashboard** -linkki. URL-osoitteet näkyvät **quick glance** -osassa.

    > [AZURE.NOTE] Azure-sivustojen luoda **seuraavan koodin korostetut** vain, jos sovellus ei tarjoa jokin. Jos annat sovelluksen projektin ylimmällä **web.config** tiedoston, sitä käytetään Azure verkkosovelluksissa.

    Jos tapahtuma ei ole käytettävissä tai on määritetty arvo `true`, sitten kannattaa luoda **web.config** Node.js sovelluksen ylimmällä ja määritä arvo `false`.  Viitteen alla on oletusarvoinen **web.config** sovelluksen, joka käyttää **app.js** aloituskohdan.

        <?xml version="1.0" encoding="utf-8"?>
        <!--
             This configuration file is required if iisnode is used to run node processes behind
             IIS or IIS Express.  For more information, visit:

             https://github.com/tjanczuk/iisnode/blob/master/src/samples/configuration/web.config
        -->

        <configuration>
          <system.webServer>
            <!-- Visit http://blogs.msdn.com/b/windowsazure/archive/2013/11/14/introduction-to-websockets-on-windows-azure-web-sites.aspx for more information on WebSocket support -->
            <webSocket enabled="false" />
            <handlers>
              <!-- Indicates that the server.js file is a node.js web app to be handled by the iisnode module -->
              <add name="iisnode" path="app.js" verb="*" modules="iisnode"/>
            </handlers>
            <rewrite>
              <rules>
                <!-- Do not interfere with requests for node-inspector debugging -->
                <rule name="NodeInspector" patternSyntax="ECMAScript" stopProcessing="true">
                  <match url="^app.js\/debug[\/]?" />
                </rule>

                <!-- First we consider whether the incoming URL matches a physical file in the /public folder -->
                <rule name="StaticContent">
                  <action type="Rewrite" url="public{REQUEST_URI}"/>
                </rule>

                <!-- All other URLs are mapped to the node.js web app entry point -->
                <rule name="DynamicContent">
                  <conditions>
                    <add input="{REQUEST_FILENAME}" matchType="IsFile" negate="True"/>
                  </conditions>
                  <action type="Rewrite" url="app.js"/>
                </rule>
              </rules>
            </rewrite>
            <!--
              You can control how Node is hosted within IIS using the following options:
                * watchedFiles: semi-colon separated list of files that will be watched for changes to restart the server
                * node_env: will be propagated to node as NODE_ENV environment variable
                * debuggingEnabled - controls whether the built-in debugger is enabled

              See https://github.com/tjanczuk/iisnode/blob/master/src/samples/configuration/web.config for a full list of options
            -->
            <!--<iisnode watchedFiles="web.config;*.js"/>-->
          </system.webServer>
        </configuration>

    Jos sovellus käyttää aloituskohtaa kuin **app.js**, kaikki esiintymät **app.js** on tilalle oikea aloituskohdan. **App.js** tilalle esimerkiksi **server.js**.

>[AZURE.NOTE] Jos haluat aloittaa Azure App palvelun ennen rekisteröimässä Azure-tili, siirry [Yritä App palvelu], jossa lyhytkestoinen starter verkkosovellukseen heti voit luoda sovelluksen-palvelussa. Ei ole pakollinen; luottokortit ei ole sitoumukset.

## <a name="next-steps"></a>Seuraavat vaiheet

Tässä opetusohjelmassa opit luomisesta keskustelu-sovelluksen pitäminen Azure web App-sovelluksessa. Voit ylläpitää myös kyseisen sovelluksen Azure-pilvipalvelussa. Ohjeita siitä, miten voit tehdä tämän on kohdassa [Muodosta Node.js Chat sovellus, jolla Socket.IO-Azure-pilvipalvelussa].

Lisätietoja on Katso myös [Node.js Developer Center].

## <a name="whats-changed"></a>Mikä on muuttunut

* Katso muutoksen opas verkkosivuilta App palveluun: [Azure App palvelu ja sen vaikutus aiemmin Azure-palvelut].

<!-- URL List -->

[Azure Redis.txt välimuisti]: /documentation/services/redis-cache/
[Sovelluksen palvelun Web Apps-sovelluksista]: http://go.microsoft.com/fwlink/?LinkId=529714
[Verkkosivun sovellusten hinnat]: http://go.microsoft.com/fwlink/?LinkId=511643
[Muodosta Node.js keskustelu-sovellusta, jonka Socket.IO Azure pilvipalvelussa]: ../cloud-services/cloud-services-nodejs-chat-app-socketio.md
[Install and Configure the Azure CLI]: ../xplat-cli-install.md
[Azure App palvelu ja sen vaikutus aiemmin Azure palvelut]: http://go.microsoft.com/fwlink/?LinkId=529714
[Node.js Developer Center]: /develop/nodejs/
[Kokeile sovelluksen-palvelu]: http://go.microsoft.com/fwlink/?LinkId=523751
[Esiintymän affiniteetti Azure sivustojen]: https://azure.microsoft.com/blog/2013/11/18/disabling-arrs-instance-affinity-in-windows-azure-web-sites/
[Luoda välimuistin Azure Redis välimuisti]: ../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md

[Socket.IO Redis.txt]: https://github.com/socketio/socket.io-redis
[Socket.IO GitHub säilöön]: https://github.com/socketio/socket.io
[ZIP- tai GZ arkistoida julkaisu]: https://github.com/socketio/socket.io/releases

<!-- IMG List -->

[chat-example-view]: ./media/web-sites-nodejs-chat-app-socketio/socketio-2.png
[npm-output]: ./media/web-sites-nodejs-chat-app-socketio/socketio-7.png
[completed-app]: ./media/web-sites-nodejs-chat-app-socketio/websitesocketcomplete.png
