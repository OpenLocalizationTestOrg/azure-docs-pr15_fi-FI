<properties
    pageTitle="Voit käyttää Azure Redis välimuistin Node.js | Microsoft Azure"
    description="Azure Redis välimuistin Node.js ja node_redis käytön aloittaminen."
    services="redis-cache"
    documentationCenter=""
    authors="steved0x"
    manager="douge"
    editor="v-lincan"/>

<tags
    ms.service="cache"
    ms.devlang="nodejs"
    ms.topic="hero-article"
    ms.tgt_pltfrm="cache-redis"
    ms.workload="tbd"
    ms.date="10/25/2016"
    ms.author="sdanie"/>

# <a name="how-to-use-azure-redis-cache-with-nodejs"></a>Voit käyttää Azure Redis välimuistin Node.js

> [AZURE.SELECTOR]
- [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
- [ASP.NET](cache-web-app-howto.md)
- [Node.js](cache-nodejs-get-started.md)
- [Java](cache-java-get-started.md)
- [Python](cache-python-get-started.md)

Azure Redis välimuistin kautta pääset käsiksi suojattua ja erillinen Redis.txt välimuistiin, Microsoft hallitsee. Välimuistin on käytettävissä mistä tahansa sovelluksesta Microsoft Azure kuluessa.

Tässä ohjeaiheessa kerrotaan, miten Aloita Azure Redis välimuistin Node.js avulla. Toinen esimerkki Azure Redis välimuistin käyttäminen Node.js kohdassa [Muodosta Node.js Chat sovellus, jolla Socket.IO Azure-sivustossa](../app-service-web/web-sites-nodejs-chat-app-socketio.md).


## <a name="prerequisites"></a>Edellytykset

Asenna [node_redis](https://github.com/mranney/node_redis):

    npm install redis

Tässä opetusohjelmassa käytetään [node_redis](https://github.com/mranney/node_redis). Esimerkkejä Node.js muiden asiakkaiden ohjeissa yksittäisten [Node.js Redis asiakkaiden](http://redis.io/clients#nodejs)on lueteltu Node.js asiakkaille.

## <a name="create-a-redis-cache-on-azure"></a>Luo Redis.txt välimuistin Azure

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## <a name="retrieve-the-host-name-and-access-keys"></a>Noutaa host nimet ja avaimet

[AZURE.INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

## <a name="connect-to-the-cache-securely-using-ssl"></a>Yhteyden muodostaminen välimuistin suojatusti SSL-yhteys

[Node_redis](https://github.com/mranney/node_redis) uusimmat versiot tukevat Azure Redis välimuistin muodostaminen SSL-yhteys. Seuraavassa esimerkissä esitetään Azure Redis välimuistin muodostaminen käyttämällä 6380 SSL-päätepistettä. Korvaa `<name>` välimuistin nimellä ja `<key>` joko ensisijaisen tai toissijaisen avain kuin kuvattu [noutaa host nimet ja näppäimet](#retrieve-the-host-name-and-access-keys) edellisessä osassa.

     var redis = require("redis");
    
      // Add your cache name and access key.
    var client = redis.createClient(6380,'<name>.redis.cache.windows.net', {auth_pass: '<key>', tls: {servername: '<name>.redis.cache.windows.net'}});


## <a name="add-something-to-the-cache-and-retrieve-it"></a>Lisää jotain välimuistin ja hakea sitä

Seuraavassa esimerkissä esitetään, kuinka Azure Redis välimuisti-esiintymään, tallentaa ja palauttaa kohteen välimuistista. Katso Lisää esimerkkejä Redis.txt käyttäminen [node_redis](https://github.com/mranney/node_redis) asiakkaan [http://redis.js.org/](http://redis.js.org/).

     var redis = require("redis");
    
      // Add your cache name and access key.
    var client = redis.createClient(6380,'<name>.redis.cache.windows.net', {auth_pass: '<key>', tls: {servername: '<name>.redis.cache.windows.net'}});
    
    client.set("key1", "value", function(err, reply) {
            console.log(reply);
        });
    
    client.get("key1",  function(err, reply) {
            console.log(reply);
        });

Tulos:

    OK
    value


## <a name="next-steps"></a>Seuraavat vaiheet

- [Käyttöön välimuistin diagnostiikka](cache-how-to-monitor.md#enable-cache-diagnostics) , niin voit [näytön](cache-how-to-monitor.md) välimuistin kunto.
- Lue virallinen [Redis ohjeissa](http://redis.io/documentation).



