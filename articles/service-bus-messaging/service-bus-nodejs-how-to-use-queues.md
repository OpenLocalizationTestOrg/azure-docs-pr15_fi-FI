<properties 
    pageTitle="Palvelun Bus olevien käyttämisestä Node.js | Microsoft Azure" 
    description="Opettele käyttämään palvelua Bus olevien Node.js-sovelluksen Azure-tietokannassa." 
    services="service-bus" 
    documentationCenter="nodejs" 
    authors="sethmanheim" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="service-bus" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs" 
    ms.topic="article" 
    ms.date="10/03/2016" 
    ms.author="sethm"/>

# <a name="how-to-use-service-bus-queues"></a>Opi käyttämään palvelua Bus olevien

[AZURE.INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

Tässä artikkelissa kuvataan palvelun Bus olevien käyttämisestä Node.js. Mallit on kirjoitettu JavaScript ja käytä Node.js Azure-moduulin. Tilanteita, joissa kattaa ovat **luominen olevien** **viestien lähettäminen ja vastaanottaminen**ja **poistamalla olevien**. Lisätietoja olevien on kohdassa [vaiheisiin](#next-steps) .

[AZURE.INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

## <a name="create-a-nodejs-application"></a>Node.js-sovelluksen luominen

Luo tyhjä Node.js-sovellus. Ohjeita siitä, miten voit luoda Node.js sovelluksen on artikkelissa [Luo ja ota käyttöön Node.js-sovelluksen Azure sivustoon][], tai [Node.js pilvipalvelussa][] Windows PowerShellin avulla.

## <a name="configure-your-application-to-use-service-bus"></a>Määritä sovelluksesi voi käyttää palvelua Bus

Voit käyttää Azure palvelun Bus, lataa ja Node.js Azure-pakettia. Tämä paketti sisältää kirjastoja, joissa viestintä palvelun Bus muiden palvelujen kanssa.

### <a name="use-node-package-manager-npm-to-obtain-the-package"></a>Hanki paketin solmu paketin hallinta (NPM) avulla

1. Siirtyminen **Windows PowerShell Node.js** -komentoikkunassa avulla **c:\\solmu\\sbqueues\\WebRole1** kansio, jossa malli-sovellus on luotu.

2. Kirjoita **npm asentaa azure** komentoikkunassa, joka olisi johtaa tulosteen seuraavankaltaiselta:

    ```
    azure@0.7.5 node_modules\azure
        ├── dateformat@1.0.2-1.2.3
        ├── xmlbuilder@0.4.2
        ├── node-uuid@1.2.0
        ├── mime@1.2.9
        ├── underscore@1.4.4
        ├── validator@1.1.1
        ├── tunnel@0.0.2
        ├── wns@0.5.3
        ├── xml2js@0.2.7 (sax@0.5.2)
        └── request@2.21.0 (json-stringify-safe@4.0.0, forever-agent@0.5.0, aws-sign@0.3.0, tunnel-agent@0.3.0, oauth-sign@0.3.0, qs@0.6.5, cookie-jar@0.3.0, node-uuid@1.4.0, http-signature@0.9.11, form-data@0.0.8, hawk@0.13.1)
    ```

3. Voit suorittaa manuaalisesti, varmista, että **ls** -komento **solmu\_moduulit** kansio on luotu. Etsi **azure** -paketti, jossa haluat käyttää palvelun Bus olevien kirjastojen on kyseiseen kansioon.

### <a name="import-the-module"></a>Tuo moduuli

Käytä Muistioon tai toisen tekstieditorissa, Lisää seuraava teksti sovelluksen **server.js** -tiedoston alkuun:

```
var azure = require('azure');
```

### <a name="set-up-an-azure-service-bus-connection"></a>Azure-palvelu Bus-yhteyden määrittäminen

Azure moduulin lukee ympäristömuuttujat AZURE\_SERVICEBUS\_nimitila ja AZURE\_SERVICEBUS\_ACCESS\_saat palvelun Bus yhteyden tiedot-näppäintä. Jos ympäristössä muuttujia ei määritetä, tilitiedot on määritettävä **createServiceBusService**soitettaessa.

Esimerkki asetus ympäristömuuttujat Azure-pilvipalvelussa määritystiedostossa on artikkelissa [Node.js pilvipalvelussa ja tallennustilaa][].

Esimerkki määrittäminen ympäristömuuttujat [Azure perinteinen portal][] Azure-sivustoon Katso [Node.js Web-sovelluksen ja tallennustilaa][].

## <a name="create-a-queue"></a>Luo jono

**ServiceBusService** objektin avulla voit käsitellä palvelun Bus olevien. Seuraava koodi luo **ServiceBusService** objektin. Lisää tietue **server.js** , tiedoston yläosassa tuo Azure moduuli-lausekkeen jälkeen:

```
var serviceBusService = azure.createServiceBusService();
```

Soittamalla **createQueueIfNotExists** **ServiceBusService** -objektin määritetyn jonossa palautetaan (jos olemassa) tai uuden jonon samanniminen luodaan. Seuraava koodi käyttää **createQueueIfNotExists** luomiseen ja muodostaa nimeltä jonossa `myqueue`:

```
serviceBusService.createQueueIfNotExists('myqueue', function(error){
    if(!error){
        // Queue exists
    }
});
```

**createServiceBusService** tukee myös lisäasetuksia, joiden avulla voit korvata oletusasetukset jonon esimerkiksi live- tai jonon enimmäiskoko viestin aikaa. Seuraavassa esimerkissä määritetään jonon enimmäiskoko 5 gt ja kellonajan live (TTL) arvon 1 minuutti:

```
var queueOptions = {
      MaxSizeInMegabytes: '5120',
      DefaultMessageTimeToLive: 'PT1M'
    };

serviceBusService.createQueueIfNotExists('myqueue', queueOptions, function(error){
    if(!error){
        // Queue exists
    }
});
```

### <a name="filters"></a>Suodattimet

Valinnainen suodatus toimintoja voidaan lisätä **ServiceBusService**suoritettujen toimintojen. Suodattaminen toimintojen sisällyttää kirjaaminen, yritetään automaattisesti jne. Suodattimet ovat objekteja, jotka toteuttavat menetelmän allekirjoituksen:

```
function handle (requestOptions, next)
```

Kun teet sen vanhat käsittely pyynnön valitsemalla asetukset, menetelmän kutsu `next`, kulkeva takaisinkutsun seuraavia allekirjoituksen:

```
function (returnObject, finalCallback, next)
```

Tämä takaisinkutsussa ja **returnObject** (vastausta pyynnön palvelimeen) käsittelyn jälkeen takaisinkutsu täytyy joko käynnistää `next` Jos jatkaa käsittelyä muut suodattimet tai kutsua ainoastaan olemassa `finalCallback`, joka päättyy palvelun käynnistäminen.

Kaksi suodatinta, jotka toteuttavat uudelleen logiikan kuuluvat Node.js, **ExponentialRetryPolicyFilter** ja **LinearRetryPolicyFilter**Azure SDK-paketissa. **ServiceBusService** -objekti, joka käyttää **ExponentialRetryPolicyFilter**Luo seuraavasti:

```
var retryOperations = new azure.ExponentialRetryPolicyFilter();
var serviceBusService = azure.createServiceBusService().withFilter(retryOperations);
```

## <a name="send-messages-to-a-queue"></a>Viestien lähettäminen jonossa

Jos haluat lähettää viestin palvelun Bus jonon, sovelluksen kutsuu **ServiceBusService** objektin **sendQueueMessage** -menetelmää. Viestien lähetetty (ja vastaanotettujen kohteesta)-palvelun Bus olevien **BrokeredMessage** objekteja ja on vakio-ominaisuuksia (esimerkiksi **otsikon** ja **TimeToLive**) sanasto, jota käytetään pidä mukautetun sovelluksen kielikohtaiset ominaisuudet ja haluamaansa hakemuksen tiedot. Sovellus määrittää viestin tekstiosaan välittämällä merkkijono viestinä. Pakollinen vakio-ominaisuudet ovat kaikille oletusarvoilla.

Seuraavassa esimerkissä kerrotaan, miten testi viestin lähettäminen nimeltä jonossa `myqueue` **sendQueueMessage**avulla:

```
var message = {
    body: 'Test message',
    customProperties: {
        testproperty: 'TestValue'
    }};
serviceBusService.sendQueueMessage('myqueue', message, function(error){
    if(!error){
        // message sent
    }
});
```

Palvelun Bus olevien tue viestin koko on [Vakio taso](service-bus-premium-messaging.md) 256 Kilotavua ja 1 Megatavu [Premium taso](service-bus-premium-messaging.md). Otsikko, joka sisältää vakio- ja mukautettu-sovelluksen ominaisuudet, voi olla koko on enintään 64 Kilotavua. Ei ole rajoitettu pidetään jonossa viestien määrän, mutta ole pää kokonaiskoko saamisesta jonon viestit. Tämä jono koko on määritetty luominen aikaa, yläraja 5 gigatavua. Saat lisätietoja kiintiöiden [palvelun Bus kiintiön][].

## <a name="receive-messages-from-a-queue"></a>Vastaanottaa viestejä jonossa

Viestit saapuvat jonon **ServiceBusService** objektin **receiveQueueMessage** -menetelmällä. Oletusarvon mukaan viestit poistetaan jonossa, kun ne on luettu; Voit kuitenkin lukea (pikanäkymä) ja Lukitse viestin poistamatta jonossa, määrittämällä valinnaisten parametrien **isPeekLock** **Tosi**.

Oletusasetuksista lukeminen ja vastaanota-toimintoa osana viestin poistaminen on helpoin mallin, ja se toimii parhaiten skenaarioissa, jossa sovellus hyväksyt ei käsittele viesti, jos. Tutustu tämän harkitse tilanne, jossa kuluttaja vastaanota pyynnön ja sitten kaatuu, ennen kuin sitä käsitellään. Koska palvelun Bus merkittyihin kulutettu, viestin, ja sitten, kun sovellus käynnistyy ja alkaa muissa viestejä uudelleen, se jääneet viesti, joka on käytetty ennen kaatumisen.

Jos **isPeekLock** -parametrin arvo on **Tosi**, vastaanotto tulee kaksi vaihetta-toimintoa, joka mahdollistaa sellaisten tuki-sovelluksia, jotka ei hyväksy puuttuu viestejä. Palvelun Bus saa pyynnön, kun se havaitsee seuraava viesti voi olla kulutettu, lukitsee voit estää muita kuluttajille vastaanottanut sitä ja palaa sovelluksen. Kun sovellus käsittelemisen viestin (tai tallentaa sen luotettavasti myöhempää käsittelyä varten), se suorittaa vastaanota-prosessin toisen vaiheen **deleteMessage** kutsumista ja tarjoamalla viesti poistetaan parametrina. **DeleteMessage** menetelmä merkitä viestin kulutettu käytössä-tilassa ja poista se jonossa.

Seuraavassa esimerkissä kerrotaan, miten voi vastaanottaa ja käsitellä viestiä, joissa **receiveQueueMessage**. Esimerkki ensin vastaanottaa ja poistaa viestin, ja sitten vastaanottaa viestin **isPeekLock** arvoksi **true**ja valitse poistaa viestin, **deleteMessage**:

```
serviceBusService.receiveQueueMessage('myqueue', function(error, receivedMessage){
    if(!error){
        // Message received and deleted
    }
});
serviceBusService.receiveQueueMessage('myqueue', { isPeekLock: true }, function(error, lockedMessage){
    if(!error){
        // Message received and locked
        serviceBusService.deleteMessage(lockedMessage, function (deleteError){
            if(!deleteError){
                // Message deleted
            }
        });
    }
});
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Sovellus kaatuu ja voi lukea viestien käsittelemisestä

Palvelun Bus on toimintoja, joiden avulla virheet sovelluksessa tai viestin käsittelyn vaikeuksia tilanteen palauttamisessa. Jos vastaanottaja-sovellus ei voi käsitellä viestiä jostain syystä, se voit kutsua **unlockMessage** -menetelmää **ServiceBusService** objektin. Tämä aiheuttaa palvelun Bus jonossa viestin lukituksen ja vastaanottaa uudelleen samaan vievää sovelluksen tai jonkin muun vievää sovelluksen käytettäväksi.

On myös lukittu jonossa viestin liittyvät aikakatkaisu ja jos sovellus ei käsitellä viestiä ennen lukituksen aikakatkaisu vanhenee (esimerkiksi, jos sovellus kaatuu), valitse palvelun Bus viestin lukituksen automaattisesti ja lisätään uudelleen Vastaanotettava.

Silloin, kun sovellus kaatuu viestin käsittelyn jälkeen mutta ennen **deleteMessage** -menetelmää kutsutaan, valitse viestin olla toimitettu edelleen sovelluksen uudelleenkäynnistyksen yhteydessä. Tätä kutsutaan usein **Vähintään kun käsittely**-, viesti käsitellään vähintään kerran mutta tietyissä tilanteissa sama viesti saattaa olla toimitettu edelleen. Jos skenaariota ei hyväksy kaksoiskappaleiden käsittely, sitten sovelluskehittäjät Lisää muita logiikan niiden sovelluksen käsittelee kaksoiskappaleita viestin lähettämisen. Tämä on usein saavuttaa sanoman, joka pysyy vakiona yli toimitusyritysten **MessageId** -ominaisuuden avulla.

## <a name="next-steps"></a>Seuraavat vaiheet

Saat lisätietoja olevien on seuraavissa resursseissa.

-   [Olevien, aiheet ja tilaukset][]
-   Valitse GitHub [Azure SDK solmun][] säilöön
-   [Node.js Developer Center](/develop/nodejs/)

  [Solmun Azure SDK]: https://github.com/Azure/azure-sdk-for-node
  [Azure perinteinen portal]: http://manage.windowsazure.com
  
  [Node.js pilvipalvelussa]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
  [Olevien, aiheet ja tilaukset]: service-bus-queues-topics-subscriptions.md
  [Luoda ja ottaa käyttöön Node.js-sovelluksen Azure-sivustoon]: ../app-service-web/web-sites-nodejs-develop-deploy-mac.md
  [Node.js pilvipalvelussa tallennustilan kanssa]: ../cloud-services/storage-nodejs-use-table-storage-cloud-service-app.md
  [Node.js verkkosovellusta, jonka tallennustila]: ../storage/storage-nodejs-how-to-use-table-storage.md
  [Palvelun Bus kiintiön]: service-bus-quotas.md
 
