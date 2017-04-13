<properties 
    pageTitle="Voit käyttää palvelun Bus olevien PHP | Microsoft Azure" 
    description="Opettele käyttämään palvelua Bus olevien Azure-tietokannassa. Kirjoitettu PHP MALLIKOODEJA." 
    services="service-bus" 
    documentationCenter="php" 
    authors="sethmanheim" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="service-bus" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="PHP" 
    ms.topic="article" 
    ms.date="10/04/2016" 
    ms.author="sethm"/>

# <a name="how-to-use-service-bus-queues"></a>Opi käyttämään palvelua Bus olevien

[AZURE.INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

Tämän oppaan avulla voit käyttää palvelun Bus olevien. Mallit on kirjoitettu PHP ja käytä [Azure SDK PHP](../php-download-sdk.md). Tilanteita, joissa kattaa ovat **luominen olevien** **viestien lähettäminen ja vastaanottaminen**ja **poistamalla olevien**.

[AZURE.INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

## <a name="create-a-php-application"></a>PHP-sovelluksen luominen

Vain vaatimus luodaan PHP-sovellusta, joka käyttää Azure-Blob-palvelu on viittaava luokkien [Azure SDK PHP](../php-download-sdk.md) -koodisi sisällä. Mikä tahansa Kehitystyökalut voit luoda sovelluksen tai Muistio.

> [AZURE.NOTE] PHP-asennus on myös oltava [OpenSSL tunniste](http://php.net/openssl) asennettu ja otettu käyttöön.

Tämän oppaan voit käyttää palvelun ominaisuuksia, jotka voidaan kutsua PHP-sovelluksen paikallisesti tai koodin suorittamisen Azure web rooli, Työntekijä roolin tai sivuston.

## <a name="get-the-azure-client-libraries"></a>Hae Azure asiakkaan kirjastot

[AZURE.INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## <a name="configure-your-application-to-use-service-bus"></a>Määritä sovelluksesi voi käyttää palvelua Bus

Palvelun Bus jonossa ohjelmointirajapinnan käyttöön seuraavasti:

1. Viittaus autoloader tiedoston [require_once] [ require_once] lause.
2. Viittaus käytöstä kaikki luokat.

Seuraavassa esimerkissä esitetään autoloader-tiedoston ja viittaa **ServicesBuilder** -luokkaan.

> [AZURE.NOTE] Tässä esimerkissä (ja muita Esimerkkejä tämän artikkelin) oletetaan tietokoneeseen on asennettu Azure kautta tunnus PHP asiakas-kirjastoissa. Jos olet asentanut kirjastot manuaalisesti tai PÄÄRYNÄPUIDEN pakettina, on viitattava **WindowsAzure.php** autoloader tiedosto.

```
require_once 'vendor/autoload.php';
use WindowsAzure\Common\ServicesBuilder;
```

Esimerkeissä `require_once` lauseen näytetään aina, mutta vain luokat, jotka ovat esimerkiksi suorittamiseen tarvittavat viitataan.

## <a name="set-up-a-service-bus-connection"></a>Palvelun Bus-yhteyden määrittäminen

Vahvistaa palvelun Bus asiakas, sinulla on oltava kelvollinen yhteysmerkkijonon tässä muodossa:

```
Endpoint=[yourEndpoint];SharedSecretIssuer=[Default Issuer];SharedSecretValue=[Default Key]
```

Missä **päätepisteen** on yleensä muodon `[yourNamespace].servicebus.windows.net`.

Voit luoda minkä tahansa Azure palvelun asiakasohjelmaa sinun on käytettävä **ServicesBuilder** -luokka. Voit:

* Välittää yhteysmerkkijonon suoraan.
* **CloudConfigurationManager (magnesiitin)** avulla voit tarkistaa useista ulkoisista lähteistä yhteysmerkkijonon varten:
    * Oletusarvon mukaan se sisältää ulkoisen tietolähteen - ympäristön muuttujat tuki
    * Voit lisätä uusia lähteitä pidentämällä **ConnectionStringSource** -luokka

Tässä kuvatut esimerkkejä yhteysmerkkijonon siirtyvät suoraan.

```
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;

$connectionString = "Endpoint=[yourEndpoint];SharedSecretIssuer=[Default Issuer];SharedSecretValue=[Default Key]";

$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
```

## <a name="how-to-create-a-queue"></a>Toimintaohje: jonon luominen

Voit suorittaa palvelun Bus olevien kautta **ServiceBusRestProxy** luokan hallintatoiminnot. **ServiceBusRestProxy** objektin muodostetaan **ServicesBuilder::createServiceBusService** factory menetelmää haluamasi yhteys-merkkijono, joka kapseloi suojaustunnuksen oikeuksia hallita sitä kautta.

Seuraavassa esimerkissä esitetään, kuinka voit vahvistaa **ServiceBusRestProxy** ja soita-niminen jonon luomiseen **ServiceBusRestProxy -> createQueue** `myqueue` sisällä `MySBNamespace` palvelun nimitila:

```
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\QueueInfo;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
    
try {
    $queueInfo = new QueueInfo("myqueue");
        
    // Create queue.
    $serviceBusRestProxy->createQueue($queueInfo);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here: 
    // http://msdn.microsoft.com/library/windowsazure/dd179357
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

> [AZURE.NOTE] Voit käyttää `listQueues` menetelmän `ServiceBusRestProxy` tarkistamaan, jos jono, jolla on määritetty nimi on jo nimitilan objekteja.

## <a name="how-to-send-messages-to-a-queue"></a>Toimintaohje: lähettää viestejä jonossa

Jos haluat lähettää viestin palvelun Bus jonon, sovelluksen kutsuu **ServiceBusRestProxy -> sendQueueMessage** -menetelmää. Seuraava koodi kerrotaan, miten voit lähettää viestin `myqueue` aiemmin luotuihin jonossa `MySBNamespace` palvelun nimitila.

```
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\BrokeredMessage;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
        
try {
    // Create message.
    $message = new BrokeredMessage();
    $message->setBody("my message");
    
    // Send message.
    $serviceBusRestProxy->sendQueueMessage("myqueue", $message);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here: 
    // http://msdn.microsoft.com/library/windowsazure/hh780775
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

Viestit on lähetetty (ja vastaanotettujen kohteesta)-palvelun Bus olevien **BrokeredMessage** luokan esiintymät. **BrokeredMessage** objektit on vakio (kuten **getLabel**, **getTimeToLive**, **setLabel**ja **setTimeToLive**) menetelmät ja ominaisuudet, joita käytetään mukautetun sovelluksen kielikohtaiset ominaisuudet ja haluamaansa sovelluksen tietoja tekstiosaan.

Palvelun Bus olevien tue viestin koko on [Vakio taso](service-bus-premium-messaging.md) 256 Kilotavua ja 1 Megatavu [Premium taso](service-bus-premium-messaging.md). Otsikko, joka sisältää vakio- ja mukautettu-sovelluksen ominaisuudet, voi olla koko on enintään 64 Kilotavua. Ei ole rajoitettu pidetään jonossa viestien määrän, mutta ole pää kokonaiskoko saamisesta jonon viestit. Tämä jono koon yläraja on 5 gt.

## <a name="how-to-receive-messages-from-a-queue"></a>Voit vastaanottaa viestejä jonossa

Paras tapa vastaanottaa viestejä jono on **ServiceBusRestProxy -> receiveQueueMessage** menetelmän käyttämisestä. Kahden eri tiloissa voi vastaanottaa viestejä: **ReceiveAndDelete** (oletusasetus) ja **PeekLock**.

Kun **ReceiveAndDelete** -tilassa on yksittäinen kuva - toiminto, kun palvelua Bus vastaanottaa viestin lukukuittauksen jonossa, se merkitsee viestin parhaillaan kulutettu ja palauttaa sen sovelluksen. **ReceiveAndDelete** tila on helpoin malli, ja se toimii parhaiten skenaarioissa, jossa sovellus hyväksyt ei käsittele viesti, jos. Tutustu tämän harkitse tilanne, jossa kuluttaja vastaanota pyynnön ja sitten kaatuu, ennen kuin sitä käsitellään. Koska palvelun Bus merkittyihin kulutettu, viestin, ja sitten, kun sovellus käynnistyy ja alkaa muissa viestejä uudelleen, se jääneet viesti, joka on käytetty ennen kaatumisen.

**PeekLock** tilassa viestien tulee kaksi vaihetta-toimintoa, joka mahdollistaa sellaisten tuki-sovelluksia, jotka ei hyväksy puuttuu viestejä. Palvelun Bus saa pyynnön, kun se havaitsee seuraava viesti voi olla kulutettu, lukitsee estää muita kuluttajille vastaanottanut sitä ja palaa sovelluksen. Kun sovellus käsittelemisen viestin (tai tallentaa sen luotettavasti myöhempää käsittelyä varten), se suorittaa vastaanota-prosessin toisen vaiheen siirtämällä vastaanotetun viestin **ServiceBusRestProxy -> deleteMessage**. Palvelun Bus näkee **deleteMessage** kutsu, kun se Merkitse viesti on kulutettu-tilassa ja poista se jonossa.

Seuraavassa esimerkissä esitetään, miten viestiin voi vastaanottaa ja käsitellä **PeekLock** -tilassa (ei oletustila).

```
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\ReceiveMessageOptions;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
        
try {
    // Set the receive mode to PeekLock (default is ReceiveAndDelete).
    $options = new ReceiveMessageOptions();
    $options->setPeekLock();
        
    // Receive message.
    $message = $serviceBusRestProxy->receiveQueueMessage("myqueue", $options);
    echo "Body: ".$message->getBody()."<br />";
    echo "MessageID: ".$message->getMessageId()."<br />";
        
    /*---------------------------
        Process message here.
    ----------------------------*/
        
    // Delete message. Not necessary if peek lock is not set.
    echo "Message deleted.<br />";
    $serviceBusRestProxy->deleteMessage($message);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/windowsazure/hh780735
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Toimintaohje: Sovellus kaatuu ja voi lukea viestit

Palvelun Bus on toimintoja, joiden avulla virheet sovelluksessa tai viestin käsittelyn vaikeuksia tilanteen palauttamisessa. Jos vastaanottaja-sovellus ei voi käsitellä viestiä jostain syystä, se voit kutsua **unlockMessage** -menetelmää vastaanotetun viestin (sijaan **deleteMessage** -menetelmä). Tämä aiheuttaa palvelun Bus jonossa viestin lukituksen ja vastaanottaa uudelleen samaan vievää sovelluksen tai jonkin muun vievää sovelluksen käytettäväksi.

On myös lukittu jonossa viestin liittyvät aikakatkaisu, ja jos sovellus ei käsitellä viestiä ennen lukituksen aikakatkaisu vanhenee (esimerkiksi, jos sovellus kaatuu)-palvelun Bus viestin lukituksen automaattisesti ja ottaa sen käyttöön uudelleen vastaanotettu.

Silloin, kun sovellus kaatuu viestin käsittelyn jälkeen mutta ennen **deleteMessage** -pyyntö annetaan, valitse viesti on toimitettu edelleen sovelluksen uudelleenkäynnistyksen yhteydessä. Tätä kutsutaan usein **Vähintään kerran käsittely**; jokaisen viestin käsitellään vähintään kerran, mutta tietyissä tilanteissa sama viesti saattaa olla toimitettu edelleen. Jos skenaariota ei hyväksy kaksoiskappaleiden käsittely, lisäämällä muita logiikan sovellusten käsittelee kaksoiskappaleita viestin lähettämisen suositellaan. Tämä on usein saavuttaa sanoman, joka pysyy vakiona yli toimitusyritysten **getMessageId** -menetelmällä.

## <a name="next-steps"></a>Seuraavat vaiheet

Nyt kun olet tutustunut palvelun Bus olevien perusteet, katso lisätietoja [olevien, aiheet, ja tilaukset][] .

Lisätietoja on Katso myös [PHP Developer Center](/develop/php/).

[Olevien, aiheet ja tilaukset]: service-bus-queues-topics-subscriptions.md
[require_once]: http://php.net/require_once

 
