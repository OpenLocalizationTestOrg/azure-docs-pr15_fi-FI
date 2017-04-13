<properties 
    pageTitle="Voit käyttää palvelun Bus aiheet Node.js | Microsoft Azure" 
    description="Opettele käyttämään palvelun Bus aiheet ja tilaukset-Azure Node.js-sovelluksesta." 
    services="service-bus" 
    documentationCenter="nodejs" 
    authors="sethmanheim" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="service-bus" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs" 
    ms.topic="article" 
    ms.date="10/04/2016" 
    ms.author="sethm"/>


# <a name="how-to-use-service-bus-topics-and-subscriptions"></a>Käytä palvelun Bus aiheet ja tilaukset

[AZURE.INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Tässä oppaassa kerrotaan, miten palvelun Bus aiheet ja tilaukset Node.js sovelluksista. Tilanteita, joissa kattaa ovat **luominen aiheet ja tilaukset**, **tilauksen suodattimien luomisesta**, **viestien lähettäminen** aiheen, **vastaanottaa viestejä tilauksesta**ja **poistamalla aiheita ja tilaukset**. Lisätietoja aiheet ja tilaukset-kohdassa [seuraavat vaiheet](#next-steps) .

[AZURE.INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

## <a name="create-a-nodejs-application"></a>Node.js-sovelluksen luominen

Luo tyhjä Node.js-sovellus. Ohjeita Node.js sovelluksen luomisesta on artikkelissa [Luo ja ota käyttöön Node.js-sovellus Azure sivuston] [Node.js pilvipalvelussa][] Windows PowerShellin tai Web-sivuston käyttäminen WebMatrix.

## <a name="configure-your-application-to-use-service-bus"></a>Määritä sovelluksesi voi käyttää palvelua Bus

Käyttämään palvelua Bus Lataa Node.js Azure-paketti. Tämä paketti sisältää kirjastoja, joissa viestintä palvelun Bus muiden palvelujen kanssa.

### <a name="use-node-package-manager-npm-to-obtain-the-package"></a>Hanki paketin solmu paketin hallinta (NPM) avulla

1.  Käytä käyttöliittymä **PowerShell** (Windows), kuten **terminaalissa** (Mac) tai **Bash** (Unix), Selaa kansioon, jossa malli-sovellus on luotu.

2.  Kirjoita **npm asentaa azure** komentoikkunassa, joka olisi johtaa seuraava tulos:

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

3.  Voit suorittaa manuaalisesti, varmista, että **ls** -komento **solmu\_moduulit** kansio on luotu. Etsi **azure** -paketti, joka sisältää kirjastoja, sinun täytyy käyttää palvelun Bus aiheet kyseiseen kansioon.

### <a name="import-the-module"></a>Tuo moduuli

Käytä Muistioon tai toisen tekstieditorissa, Lisää seuraava teksti sovelluksen **server.js** -tiedoston alkuun:

```
var azure = require('azure');
```

### <a name="set-up-a-service-bus-connection"></a>Palvelun Bus-yhteyden määrittäminen

Azure moduulin lukee ympäristömuuttujat AZURE\_SERVICEBUS\_nimitila ja AZURE\_SERVICEBUS\_ACCESS\_AVAIMEN muodostaa palvelun Bus tarvittavat tiedot. Jos ympäristössä muuttujia ei määritetä, tilitiedot on määritettävä **createServiceBusService**soitettaessa.

Esimerkki asetus ympäristömuuttujat Azure-pilvipalvelussa määritystiedostossa on artikkelissa [Node.js pilvipalvelussa ja tallennustilaa][].

Esimerkki määrittäminen ympäristömuuttujat [Azure perinteinen portal][] Azure-sivustoon Katso [Node.js Web-sovelluksen ja tallennustilaa][].

## <a name="create-a-topic"></a>Aiheen luominen

**ServiceBusService** objektin avulla voit käsitellä aiheet. Seuraava koodi luo **ServiceBusService** objektin. Lisää tietue **server.js** , tiedoston yläosassa tuo azure moduuli-lausekkeen jälkeen:

```
var serviceBusService = azure.createServiceBusService();
```

Soittamalla **createTopicIfNotExists** **ServiceBusService** objektin määritettyä aihetta palautetaan (jos olemassa) tai uusi aihe samanniminen luodaan. Seuraava koodi käyttää **createTopicIfNotExists** luomiseen ja muodostaa nimeltä "MyTopic" aihe:

```
serviceBusService.createTopicIfNotExists('MyTopic',function(error){
    if(!error){
        // Topic was created or exists
        console.log('topic created or exists.');
    }
});
```

**createServiceBusService** tukee myös lisäasetuksia, joiden avulla voit korvata oletusasetukset aiheen kuten viestin, kun haluat live tai suurin aiheen kokoa. Seuraavassa esimerkissä asetetaan suurin aiheen kokoa 5 gt Live 1 minuutin ajan:

```
var topicOptions = {
        MaxSizeInMegabytes: '5120',
        DefaultMessageTimeToLive: 'PT1M'
    };

serviceBusService.createTopicIfNotExists('MyTopic', topicOptions, function(error){
    if(!error){
        // topic was created or exists
    }
});
```

### <a name="filters"></a>Suodattimet

Valinnainen suodatus toimintoja voidaan lisätä **ServiceBusService**suoritettujen toimintojen. Suodattaminen toimintojen sisällyttää kirjaaminen, yritetään automaattisesti jne. Suodattimet ovat objekteja, jotka toteuttavat menetelmän allekirjoituksen:

```
function handle (requestOptions, next)
```

Suorituksen esikäsittely pyynnön valitsemalla asetukset, jälkeen menetelmä soittaa `next` kulkeva seuraavat allekirjoituksella takaisinkutsun:

```
function (returnObject, finalCallback, next)
```

Tämän takaisinkutsussa ja käsittelyn **returnObject** (vastausta pyynnön palvelimeen) jälkeen joko takaisinsoitto tarpeet kutsua seuraava, jos jatkaa käsittelyä muut suodattimet tai kutsua ainoastaan **finalCallback** muuten avulla palvelun käynnistäminen olemassa.

Kaksi suodatinta, jotka toteuttavat uudelleen logiikan kuuluvat Node.js, **ExponentialRetryPolicyFilter** ja **LinearRetryPolicyFilter**Azure SDK-paketissa. **ServiceBusService** -objekti, joka käyttää **ExponentialRetryPolicyFilter**Luo seuraavasti:

    var retryOperations = new azure.ExponentialRetryPolicyFilter();
    var serviceBusService = azure.createServiceBusService().withFilter(retryOperations);

## <a name="create-subscriptions"></a>Luo tilaukset

Aiheen tilaukset luodaan myös **ServiceBusService** objekti. Tilaukset nimetään, ja se voi olla valinnainen suodatin, joka rajoittaa viestit toimitetaan tilauksen virtual jonossa olevat.

> [AZURE.NOTE] Tilaukset pysyvä ja edelleen olemassa ennen kuin he joko tai aiheen ne liitetään ja poistetaan. Jos sovellus on logiikan tilauksen luomiseen, se on ensin tarkistettava Jos tilaus on jo olemassa **getSubscription** -menetelmällä.

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Luo tilauksen oletussuodatin (MatchAll)

**MatchAll** suodatin on oletussuodatin, jota käytetään, jos ei suodatusta määritetään, kun luodaan uusi tilaus. Kun **MatchAll** -suodatin on käytössä, kaikki viestit, jotka on julkaistu aiheeseen sijoitetaan tilauksen virtual jonon. Seuraava esimerkki luo tilausta nimeltä 'AllMessages' ja käyttää **MatchAll** oletussuodatin.

```
serviceBusService.createSubscription('MyTopic','AllMessages',function(error){
    if(!error){
        // subscription created
    }
});
```

### <a name="create-subscriptions-with-filters"></a>Luo tilaukset suodattimet

Voit myös luoda suodattimia, joiden avulla voit laajuus, joka lähetetään aiheen pitäisi näyttää tiettyä aihetta-tilauksen piiriin kuuluvien.

Tuettu tilauksissa suodattimen eniten joustavia tyyppi on **SqlFilter**, joka toteuttaa SQL92 alijoukkoa. Viestit, jotka on julkaistu aiheen ominaisuudet toimivat SQL suodattimet. Varten tarkempia tietoja lausekkeita, jotka voidaan käyttää SQL-suodatin, tarkista [SqlFilter.SqlExpression] [ SqlFilter.SqlExpression] syntaksia.

Tilauksen voidaan lisätä suodattimia **ServiceBusService** objektin **createRule** -menetelmällä. Tätä menetelmää voit lisätä uusia suodattimia olemassa olevaan tilaukseen.

> [AZURE.NOTE] Koska oletussuodatin käytetään automaattisesti kaikissa uusissa tilauksissa, sinun on ensin poistettava oletussuodatin tai **MatchAll** ohittaa muut voivat määritellä suodattimet. Voit poistaa oletusarvon säännön **ServiceBusService** objektin **deleteRule** -menetelmällä.

Seuraavassa esimerkissä luodaan tilausta nimeltä `HighMessages` **SqlFilter** , joka valitsee vain viestit, jotka on suurempi kuin 3 mukautetun **messagenumber** ominaisuus kanssa:

```
serviceBusService.createSubscription('MyTopic', 'HighMessages', function (error){
    if(!error){
        // subscription created
        rule.create();
    }
});
var rule={
    deleteDefault: function(){
        serviceBusService.deleteRule('MyTopic',
            'HighMessages', 
            azure.Constants.ServiceBusConstants.DEFAULT_RULE_NAME, 
            rule.handleError);
    },
    create: function(){
        var ruleOptions = {
            sqlExpressionFilter: 'messagenumber > 3'
        };
        rule.deleteDefault();
        serviceBusService.createRule('MyTopic', 
            'HighMessages', 
            'HighMessageFilter', 
            ruleOptions, 
            rule.handleError);
    },
    handleError: function(error){
        if(error){
            console.log(error)
        }
    }
}
```

Seuraavassa esimerkissä luodaan vastaavasti tilausta nimeltä `LowMessages` **SqlFilter** , joka valitsee vain viestit, joiden **messagenumber** ominaisuus pienempi tai yhtä 3 kanssa:

```
serviceBusService.createSubscription('MyTopic', 'LowMessages', function (error){
    if(!error){
        // subscription created
        rule.create();
    }
});
var rule={
    deleteDefault: function(){
        serviceBusService.deleteRule('MyTopic',
            'LowMessages', 
            azure.Constants.ServiceBusConstants.DEFAULT_RULE_NAME, 
            rule.handleError);
    },
    create: function(){
        var ruleOptions = {
            sqlExpressionFilter: 'messagenumber <= 3'
        };
        rule.deleteDefault();
        serviceBusService.createRule('MyTopic', 
            'LowMessages', 
            'LowMessageFilter', 
            ruleOptions, 
            rule.handleError);
    },
    handleError: function(error){
        if(error){
            console.log(error)
        }
    }
}
```

Kun viesti on nyt lähetetty `MyTopic`, se toimitetaan aina tilannut vastaanottajia `AllMessages` aihe-tilaukseen ja valikoivasti toimittaa tilannut vastaanottajia `HighMessages` ja `LowMessages` aihe-tilaukset (sen mukaan, viestin sisältöä).

## <a name="how-to-send-messages-to-a-topic"></a>Voit lähettää viestejä aiheen

Jos haluat lähettää viestin palvelun Bus aiheen, sovelluksen on käytettävä **ServiceBusService** objektin **sendTopicMessage** -menetelmää.
Palvelun Bus aiheet lähetetyt viestit ovat **BrokeredMessage** objekteja.
**BrokeredMessage** objekteissa on joukko vakio-ominaisuuksia (esimerkiksi **otsikon** ja **TimeToLive**) sanasto, jota käytetään pidä mukautetun sovelluksen tiettyjen ominaisuuksien ja merkkijonon tiedot. Sovelluksen, voit määrittää viestin tekstiosaan siirtämällä merkkijonoarvo **sendTopicMessage** ja tarvittavat vakio-ominaisuudet täytetään oletusarvojen mukaan.

Seuraavassa esimerkissä viisi testaaminen 'MyTopic' viestin lähettäminen. Huomaa, että kustakin viestistä **messagenumber** ominaisuuden arvo vaihtelee-silmukan iteraatio (tämä määritetään päättyneet tilaukset perille):

```
var message = {
    body: '',
    customProperties: {
        messagenumber: 0
    }
}

for (i = 0;i < 5;i++) {
    message.customProperties.messagenumber=i;
    message.body='This is Message #'+i;
    serviceBusService.sendTopicMessage(topic, message, function(error) {
      if (error) {
        console.log(error);
      }
    });
}
```

Palvelun Bus ohjeita tukea enimmäiskoon koosta [Vakio taso](service-bus-premium-messaging.md) 256 Kilotavua ja 1 Megatavu [Premium taso](service-bus-premium-messaging.md). Otsikko, joka sisältää vakio- ja mukautettu-sovelluksen ominaisuudet, voi olla koko on enintään 64 Kilotavua. Ei ole rajoitettu viestit säilytetään aiheen määrän, mutta ole pää kokonaiskoko viestit aiheen mukaan. Tässä aiheessa kokoa on määritetty luominen aikaa, yläraja on 5 gt.

## <a name="receive-messages-from-a-subscription"></a>Vastaanottaa viestejä tilauksesta

**ServiceBusService** objektin **receiveSubscriptionMessage** -menetelmällä tilauksesta saapuvat viestit. Oletusarvon mukaan viestit poistetaan tilauksesta, kun ne on luettu; Voit kuitenkin lukea (pikanäkymä) ja Lukitse viestin poistamatta tilauksesta, määrittämällä valinnaisten parametrien **isPeekLock** **True**.

Oletusasetuksista lukeminen ja vastaanota-toimintoa osana viestin poistaminen on helpoin mallin, ja se toimii parhaiten skenaarioissa, jossa sovellus hyväksyt ei käsittele viesti, jos. Tutustu tämän harkitse tilanne, jossa kuluttaja vastaanota pyynnön ja sitten kaatuu, ennen kuin sitä käsitellään. Koska palvelun Bus merkittyihin kulutettu, viestin, ja sitten, kun sovellus käynnistyy ja alkaa muissa viestejä uudelleen, se jääneet viesti, joka on käytetty ennen kaatumisen.

Jos **isPeekLock** -parametrin arvo on **Tosi**, vastaanotto tulee kaksi vaihetta-toimintoa, joka mahdollistaa sellaisten tuki-sovelluksia, jotka ei hyväksy puuttuu viestejä. Palvelun Bus saa pyynnön, kun se havaitsee seuraava viesti voi olla kulutettu, lukitsee voit estää muita kuluttajille vastaanottanut sitä ja palaa sovelluksen.
Kun sovellus käsittelemisen viestin (tai tallentaa sen luotettavasti myöhempää käsittelyä varten), se suorittaa vastaanota-prosessin toisen vaiheen **deleteMessage** kutsumista ja tarjoamalla viesti poistetaan parametrina. **DeleteMessage** menetelmä merkitä viestin kulutettu käytössä-tilassa ja poista se tilauksesta.

Seuraavassa esimerkissä näytetään, miten viestit voi vastaanottaa ja käsitellään **receiveSubscriptionMessage**avulla. Esimerkki ensin vastaanottaa ja poistaa viestin 'LowMessages'-tilauksesi ja vastaanottaa sitten viestin 'HighMessages'-tilaukseesi **isPeekLock** arvo on TOSI. Se poistaa viestin **deleteMessage**avulla:

```
serviceBusService.receiveSubscriptionMessage('MyTopic', 'LowMessages', function(error, receivedMessage){
    if(!error){
        // Message received and deleted
        console.log(receivedMessage);
    }
});
serviceBusService.receiveSubscriptionMessage('MyTopic', 'HighMessages', { isPeekLock: true }, function(error, lockedMessage){
    if(!error){
        // Message received and locked
        console.log(lockedMessage);
        serviceBusService.deleteMessage(lockedMessage, function (deleteError){
            if(!deleteError){
                // Message deleted
                console.log('message has been deleted.');
            }
        }
    }
});
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Sovellus kaatuu ja voi lukea viestien käsittelemisestä

Palvelun Bus on toimintoja, joiden avulla virheet sovelluksessa tai viestin käsittelyn vaikeuksia tilanteen palauttamisessa. Jos vastaanottaja-sovellus ei voi käsitellä viestiä jostain syystä, se voit kutsua **unlockMessage** -menetelmää **ServiceBusService** objektin. Tämä aiheuttaa palvelun Bus tilauksen piiriin kuuluvien viestin lukituksen ja vastaanottaa uudelleen samaan vievää sovelluksen tai jonkin muun vievää sovelluksen käytettäväksi.

On myös lukittu tilauksen piiriin kuuluvien viestin liittyvät aikakatkaisu, ja jos sovellus ei käsitellä viestiä ennen lukituksen aikakatkaisu vanhenee (esimerkiksi, jos sovellus kaatuu)-palvelun Bus lukitus viestin automaattisesti ja mahdollistaa sen käytön vastaanotettu uudelleen.

Silloin, kun sovellus kaatuu viestin käsittelyn jälkeen mutta ennen **deleteMessage** -menetelmää kutsutaan, valitse viestin olla toimitettu edelleen sovelluksen uudelleenkäynnistyksen yhteydessä. Tätä kutsutaan usein **Vähintään kun käsittely**-, viesti käsitellään vähintään kerran mutta tietyissä tilanteissa sama viesti saattaa olla toimitettu edelleen. Jos skenaariota ei hyväksy kaksoiskappaleiden käsittely, sitten sovelluskehittäjät Lisää muita logiikan niiden sovelluksen käsittelee kaksoiskappaleita viestin lähettämisen. Tämä on usein saavuttaa sanoman, joka pysyy vakiona yli toimitusyritysten **MessageId** -ominaisuuden avulla.

## <a name="delete-topics-and-subscriptions"></a>Poista aiheet ja tilaukset

Ohjeita ja tilaukset on jatkuva ja täytyy erikseen poistaa [Azure perinteinen portal][] kautta tai sen ohjelmallisesti.
Seuraavassa esimerkissä nimeltä aiheen poistaminen `MyTopic`:

    serviceBusService.deleteTopic('MyTopic', function (error) {
        if (error) {
            console.log(error);
        }
    });

Aiheen poistaminen poistaa myös tilaukset, jotka on rekisteröity aihetta. Tilauksia voi poistaa myös erikseen. Seuraavassa esimerkissä poistamisesta tilausta nimeltä `HighMessages` kohteesta `MyTopic` aihe:

    serviceBusService.deleteSubscription('MyTopic', 'HighMessages', function (error) {
        if(error) {
            console.log(error);
        }
    });

## <a name="next-steps"></a>Seuraavat vaiheet

Nyt kun olet oppinut perusteet palvelun Bus aiheista, noudata näitä linkkejä lisätietoja.

-   Katso [olevien, aiheet, ja tilaukset][].
-   [SqlFilter][]API-viittaus.
-   Lisätietoja [Azure SDK solmun][] säilö GitHub.

  [Solmun Azure SDK]: https://github.com/Azure/azure-sdk-for-node
  [Azure perinteinen portal]: https://manage.windowsazure.com
  [SqlFilter.SqlExpression]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx
  [Olevien, aiheet ja tilaukset]: service-bus-queues-topics-subscriptions.md
  [SqlFilter]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.aspx
  [Node.js pilvipalvelussa]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
  [Luoda ja ottaa käyttöön Node.js-sovellus Azure sivuston]: ../app-service-web/web-sites-nodejs-develop-deploy-mac.md
  [Node.js pilvipalvelussa tallennustilan kanssa]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
  [Node.js verkkosovellusta, jonka tallennustila]: ../cloud-services/storage-nodejs-use-table-storage-cloud-service-app.md
 
