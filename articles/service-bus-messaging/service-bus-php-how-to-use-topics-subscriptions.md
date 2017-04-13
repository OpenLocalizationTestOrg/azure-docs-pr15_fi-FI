<properties 
    pageTitle="Voit käyttää palvelun Bus aiheet PHP | Microsoft Azure" 
    description="Opettele käyttämään palvelua Bus aiheet PHP Azure kanssa." 
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
    ms.date="10/14/2016" 
    ms.author="sethm"/>


# <a name="how-to-use-service-bus-topics-and-subscriptions"></a>Palvelun Bus aiheet ja tilaukset käyttäminen

[AZURE.INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Tässä artikkelissa kerrotaan palvelun Bus aiheet ja tilaukset. Mallit on kirjoitettu PHP ja käytä [Azure SDK PHP](../php-download-sdk.md). Tilanteita, joissa kattaa ovat **luominen aiheet ja tilaukset**, **tilauksen suodattimien luomisesta**, **aiheen viestien lähettäminen**, **vastaanottaa viestejä tilauksesta**ja **poistaminen aiheet ja tilaukset**.

[AZURE.INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

## <a name="create-a-php-application"></a>PHP-sovelluksen luominen

Vain vaatimus luodaan PHP-sovellusta, joka käyttää Azure-Blob-palvelu on viittaus luokkien [Azure SDK PHP](../php-download-sdk.md) että koodiin. Mikä tahansa Kehitystyökalut voit luoda sovelluksen tai Muistio.

> [AZURE.NOTE] PHP-asennus on myös oltava [OpenSSL tunniste](http://php.net/openssl) asennettu ja otettu käyttöön.

Tässä artikkelissa kerrotaan, miten service-ominaisuuksia, jotka voidaan kutsua PHP-sovelluksen paikallisesti tai koodin suorittamisen Azure web rooli, työntekijän rooli tai sivuston.

## <a name="get-the-azure-client-libraries"></a>Hae Azure asiakkaan kirjastot

[AZURE.INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## <a name="configure-your-application-to-use-service-bus"></a>Määritä sovelluksesi voi käyttää palvelua Bus

Voit käyttää Bus Service API seuraavasti:

1. Viittaus autoloader tiedoston [require_once] [ require-once] lause.
2. Viittaus käytöstä kaikki luokat.

Seuraavassa esimerkissä esitetään autoloader-tiedoston ja viittaa **ServiceBusService** -luokkaan.

> [AZURE.NOTE] Tässä esimerkissä (ja muita Esimerkkejä tämän artikkelin) oletetaan tietokoneeseen on asennettu Azure kautta tunnus PHP asiakas-kirjastoissa. Jos olet asentanut kirjastot manuaalisesti tai PÄÄRYNÄPUIDEN pakettina, on viitattava **WindowsAzure.php** autoloader tiedosto.

```
require_once 'vendor\autoload.php';
use WindowsAzure\Common\ServicesBuilder;
```

Seuraavissa esimerkeissä `require_once` lauseen näytetään aina, mutta vain luokat, jotka ovat esimerkiksi suorittamiseen tarvittavat viitataan.

## <a name="set-up-a-service-bus-connection"></a>Palvelun Bus-yhteyden määrittäminen

Palvelun Bus asiakkaan vahvistamiseen sinulla on oltava kelvollinen yhteysmerkkijonon tässä muodossa:

```
Endpoint=[yourEndpoint];SharedSecretIssuer=[Default Issuer];SharedSecretValue=[Default Key]
```

Jos `Endpoint` on yleensä muodon `https://[yourNamespace].servicebus.windows.net`.

Voit luoda minkä tahansa Azure palvelun asiakasohjelmaa sinun on käytettävä **ServicesBuilder** -luokka. Voit:

* Välittää yhteysmerkkijonon suoraan.
* **CloudConfigurationManager (magnesiitin)** avulla voit tarkistaa useista ulkoisista lähteistä yhteysmerkkijonon varten:
    * Oletusarvon mukaan se sisältyy tuki ulkoisen tietolähteen - ympäristön muuttujat.
    * Voit lisätä uusia lähteitä pidentämällä **ConnectionStringSource** -luokka.

Tässä kuvatut esimerkeissä yhteysmerkkijonon välitetään suoraan.

```
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
    
$connectionString = "Endpoint=[yourEndpoint];SharedSecretIssuer=[Default Issuer];SharedSecretValue=[Default Key]";

$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
```

## <a name="create-a-topic"></a>Aiheen luominen

Voit suorittaa hallintatoiminnot palvelun Bus ohjeita **ServiceBusRestProxy** -luokan kautta. **ServiceBusRestProxy** objektin muodostetaan **ServicesBuilder::createServiceBusService** factory menetelmää haluamasi yhteys-merkkijono, joka kapseloi suojaustunnuksen oikeuksia hallita sitä kautta.

Seuraavassa esimerkissä esitetään, voit vahvistaa **ServiceBusRestProxy** ja soita-niminen aiheen luominen **ServiceBusRestProxy -> createTopic** - `mytopic` sisällä `MySBNamespace` nimitilan:

```
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\TopicInfo;
    
// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
    
try {       
    // Create topic.
    $topicInfo = new TopicInfo("mytopic");
    $serviceBusRestProxy->createTopic($topicInfo);
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

> [AZURE.NOTE] Voit käyttää `listTopics` menetelmän `ServiceBusRestProxy` objekteihin ja tarkista, onko palvelun nimitilan aiheen määritetty nimi, jo olemassa.

## <a name="create-a-subscription"></a>Tilauksen luominen

Aiheen tilaukset luodaan myös **ServiceBusRestProxy -> createSubscription** -menetelmällä. Tilaukset nimetään, ja se voi olla valinnainen suodatin, joka rajoittaa tietuejoukon tilauksen virtual jonon välitetään viestejä.

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Luo tilauksen oletussuodatin (MatchAll)

**MatchAll** suodatin on oletussuodatin, jota käytetään, jos ei suodatusta määritetään, kun luodaan uusi tilaus. Kun **MatchAll** -suodatin on käytössä, kaikki viestit, jotka on julkaistu aiheeseen sijoitetaan tilauksen virtual jonon. Seuraava esimerkki luo tilausta nimeltä 'mysubscription' ja käyttää **MatchAll** oletussuodatin.

```
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\SubscriptionInfo;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
    
try {
    // Create subscription.
    $subscriptionInfo = new SubscriptionInfo("mysubscription");
    $serviceBusRestProxy->createSubscription("mytopic", $subscriptionInfo);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here: 
    // http://msdn.microsoft.com/library/azure/dd179357
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

### <a name="create-subscriptions-with-filters"></a>Luo tilaukset suodattimet

Voit myös määrittää suodattimia, joiden avulla voit määrittää, mitkä aiheen lähetetyt viestit kannattaa tiettyä aihetta-tilauksen piiriin kuuluvien näkyvän. Tuettu tilauksissa suodattimen eniten joustavia tyyppi on **SqlFilter**, joka toteuttaa SQL92 alijoukkoa. Viestit, jotka on julkaistu aiheen ominaisuudet toimivat SQL suodattimet. Saat lisätietoja SqlFilters [SqlFilter.SqlExpression ominaisuuden][sqlfilter].

> [AZURE.NOTE] Tilauksen kunkin sääntöä käsittelee saapuvien viestien itsenäisesti niiden tuloksen viestien lisääminen tilaukseen. Lisäksi kunkin uusi tilaus on oletusarvoinen **sääntö** -objekti, jossa on suodatin, joka lisää kaikki viestit aiheesta tilaukseen. Saat vain viestit, jotka vastaavat suodattimessa, sinun on poistettava oletusarvo-säännön. Voit poistaa oletusarvon säännön avulla `ServiceBusRestProxy->deleteRule` menetelmää.

Seuraavassa esimerkissä luodaan tilausta nimeltä **HighMessages** **SqlFilter** , joka valitsee vain viestit, joissa on suurempi kuin 3 (Lisätietoja on kohdassa [Lähetä viestit aiheen](#send-messages-to-a-topic) mukautettujen ominaisuuksien lisääminen viesteihin) mukautetun **MessageNumber** ominaisuuden kanssa:

```
$subscriptionInfo = new SubscriptionInfo("HighMessages");
$serviceBusRestProxy->createSubscription("mytopic", $subscriptionInfo);

$serviceBusRestProxy->deleteRule("mytopic", "HighMessages", '$Default');

$ruleInfo = new RuleInfo("HighMessagesRule");
$ruleInfo->withSqlFilter("MessageNumber > 3");
$ruleResult = $serviceBusRestProxy->createRule("mytopic", "HighMessages", $ruleInfo);
```

Huomautus Tämä koodi edellyttää muita nimitilan käyttö: `WindowsAzure\ServiceBus\Models\SubscriptionInfo`.

Seuraavassa esimerkissä luodaan vastaavasti tilausta nimeltä **LowMessages** **SqlFilter** , joka valitsee vain viestit, joiden **MessageNumber** ominaisuus pienempi tai yhtä 3 kanssa:

```
$subscriptionInfo = new SubscriptionInfo("LowMessages");
$serviceBusRestProxy->createSubscription("mytopic", $subscriptionInfo);

$serviceBusRestProxy->deleteRule("mytopic", "LowMessages", '$Default');

$ruleInfo = new RuleInfo("LowMessagesRule");
$ruleInfo->withSqlFilter("MessageNumber <= 3");
$ruleResult = $serviceBusRestProxy->createRule("mytopic", "LowMessages", $ruleInfo);
```

Nyt, kun viesti on lähetetty `mytopic` ohjeaiheessa aina toimitusta tilannut vastaanottajia `mysubscription` -tilaukseen ja valikoivasti toimittaa tilannut vastaanottajia `HighMessages` ja `LowMessages` tilaukset (sen mukaan, viestin sisältöä).

## <a name="send-messages-to-a-topic"></a>Viestien lähettäminen aiheen

Jos haluat lähettää viestin palvelun Bus aiheen, sovelluksen kutsuu **ServiceBusRestProxy -> sendTopicMessage** -menetelmää. Seuraava koodi kerrotaan, miten voit lähettää viestin `mytopic` aiheen aiemmin luotuihin `MySBNamespace` palvelun nimitila.

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
    $serviceBusRestProxy->sendTopicMessage("mytopic", $message);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here: 
    // http://msdn.microsoft.com/library/azure/hh780775
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

Palvelun Bus aiheet lähetetyt viestit ovat **BrokeredMessage** luokan esiintymät. **BrokeredMessage** objekteissa on vakio-ominaisuudet ja menetelmiä (kuten **getLabel**, **getTimeToLive** **setLabel**tai **setTimeToLive**), sekä ominaisuudet, joita voidaan käyttää pidä mukautetun sovelluksen kielikohtaiset ominaisuudet. Seuraavassa esimerkissä 5 testi viestien lähettäminen `mytopic` aiheen aiemmin luotu. Jos haluat lisätä mukautetun ominaisuuden käytetään **setProperty** -menetelmää (`MessageNumber`) kaikkiin viesteihin. Huomaa, että `MessageNumber` ominaisuuden arvo vaihtelee jokaisen viestin (Voit tehdä tämä arvo määrittää päättyneet tilaukset vastaanottaa, kuten [Luo tilauksen](#create-a-subscription) -osassa):

```
for($i = 0; $i < 5; $i++){
    // Create message.
    $message = new BrokeredMessage();
    $message->setBody("my message ".$i);
            
    // Set custom property.
    $message->setProperty("MessageNumber", $i);
            
    // Send message.
    $serviceBusRestProxy->sendTopicMessage("mytopic", $message);
}
```

Palvelun Bus ohjeita tukea enimmäiskoon koosta [Vakio taso](service-bus-premium-messaging.md) 256 Kilotavua ja 1 Megatavu [Premium taso](service-bus-premium-messaging.md). Otsikko, joka sisältää vakio- ja mukautettu-sovelluksen ominaisuudet, voi olla koko on enintään 64 Kilotavua. Ei ole rajoitettu viestit säilytetään aiheen määrän, mutta ole pää kokonaiskoko viestit aiheen mukaan. Tässä aiheessa koon yläraja on 5 gt. Saat lisätietoja kiintiöiden [palvelun Bus kiintiön][].

## <a name="receive-messages-from-a-subscription"></a>Vastaanottaa viestejä tilauksesta

**ServiceBusRestProxy -> receiveSubscriptionMessage** menetelmän käyttämisestä on paras tapa vastaanottaa viestejä tilaukseen. Vastaanotettujen viestien työskennellä kahta eri tilat: **ReceiveAndDelete** (oletusasetus) ja **PeekLock**.

Kun **ReceiveAndDelete** -tilassa on yksi vahvistetaan; toiminto Kun palvelun Bus vastaanottaa lukukuittauksen viesti-tilauksen, se merkitsee viestin kulutettu käytössä-tilassa ja palaa sovelluksen. **ReceiveAndDelete** tila on helpoin malli, ja se toimii parhaiten skenaarioissa, jossa sovellus hyväksyt ei käsittele viesti, jos. Tutustu tämän harkitse tilanne, jossa kuluttaja vastaanota pyynnön ja sitten kaatuu, ennen kuin sitä käsitellään. Koska palvelun Bus merkittyihin kulutettu, viestin, ja sitten, kun sovellus käynnistyy ja alkaa muissa viestejä uudelleen, se jääneet viesti, joka on käytetty ennen kaatumisen.

**PeekLock** tilassa viestien tulee kaksi vaihetta-toimintoa, joka mahdollistaa sellaisten tuki-sovelluksia, jotka ei hyväksy puuttuu viestejä. Palvelun Bus saa pyynnön, kun se havaitsee seuraava viesti voi olla kulutettu, lukitsee voit estää muita kuluttajille vastaanottanut sitä ja palaa sovelluksen. Kun sovellus käsittelemisen viestin (tai tallentaa sen luotettavasti myöhempää käsittelyä varten), se suorittaa vastaanota-prosessin toisen vaiheen siirtämällä vastaanotetun viestin **ServiceBusRestProxy -> deleteMessage**. Palvelun Bus näkee **deleteMessage** kutsu, kun se Merkitse viesti on kulutettu-tilassa ja poista se jonossa.

Seuraavassa esimerkissä esitetään, miten voi vastaanottaa ja käsitellä viestin **PeekLock** tilassa (ei oletustila). 

```
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use WindowsAzure\Common\ServiceException;
use WindowsAzure\ServiceBus\Models\ReceiveMessageOptions;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
        
try {
    // Set receive mode to PeekLock (default is ReceiveAndDelete)
    $options = new ReceiveMessageOptions();
    $options->setPeekLock();
    
    // Get message.
    $message = $serviceBusRestProxy->receiveSubscriptionMessage("mytopic", "mysubscription", $options);

    echo "Body: ".$message->getBody()."<br />";
    echo "MessageID: ".$message->getMessageId()."<br />";
        
    /*---------------------------
        Process message here.
    ----------------------------*/
        
    // Delete message. Not necessary if peek lock is not set.
    echo "Deleting message...<br />";
    $serviceBusRestProxy->deleteMessage($message);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/hh780735
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Toimintaohje: Sovellus kaatuu ja voi lukea viestit

Palvelun Bus on toimintoja, joiden avulla virheet sovelluksessa tai viestin käsittelyn vaikeuksia tilanteen palauttamisessa. Jos vastaanottaja-sovellus ei voi käsitellä viestiä jostain syystä, se voit kutsua **unlockMessage** -menetelmää vastaanotetun viestin (sijaan **deleteMessage** -menetelmä). Tämä aiheuttaa palvelun Bus jonossa viestin lukituksen ja vastaanottaa uudelleen samaan vievää sovelluksen tai jonkin muun vievää sovelluksen käytettäväksi.

On myös lukittu jonossa viestin liittyvät aikakatkaisu, ja jos sovellus ei käsitellä viestiä ennen lukituksen aikakatkaisu vanhenee (esimerkiksi, jos sovellus kaatuu)-palvelun Bus viestin lukituksen automaattisesti ja ottaa sen käyttöön uudelleen vastaanotettu.

Silloin, kun sovellus kaatuu viestin käsittelyn jälkeen mutta ennen **deleteMessage** -pyyntö annetaan, valitse viesti on toimitettu edelleen sovelluksen uudelleenkäynnistyksen yhteydessä. Tätä kutsutaan usein **Vähintään kerran käsittely**; jokaisen viestin käsitellään vähintään kerran, mutta tietyissä tilanteissa sama viesti saattaa olla toimitettu edelleen. Jos skenaariota ei hyväksy kaksoiskappaleet käsitellään, sitten sovelluskehittäjät kannattaa lisätä muita logiikan käsittelee kaksoiskappaleita viestin lähettämisen sovellukset. Tämä on usein saavuttaa sanoman, joka pysyy vakiona yli toimitusyritysten **getMessageId** -menetelmällä.

## <a name="delete-topics-and-subscriptions"></a>Poista aiheet ja tilaukset

Voit poistaa aiheen tai tilauksen-sovelluksella- **ServiceBusRestProxy -> deleteTopic** ja **ServiceBusRestProxy -> deleteSubscripton** -kautta, tarpeen mukaan. Huomaa, että aiheen poistaminen myös poistaa tilaukset, jotka on rekisteröity aihetta.

Seuraavassa esimerkissä nimeltä aiheen poistaminen `mytopic` ja sen rekisteröidyn tilaukset.

```
require_once 'vendor/autoload.php';

use WindowsAzure\ServiceBus\ServiceBusService;
use WindowsAzure\ServiceBus\ServiceBusSettings;
use WindowsAzure\Common\ServiceException;

// Create Service Bus REST proxy.
$serviceBusRestProxy = ServicesBuilder::getInstance()->createServiceBusService($connectionString);
    
try {       
    // Delete topic.
    $serviceBusRestProxy->deleteTopic("mytopic");
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here: 
    // http://msdn.microsoft.com/library/azure/dd179357
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

**DeleteSubscription** -menetelmällä voit poistaa tilauksen itsenäisesti:

```
$serviceBusRestProxy->deleteSubscription("mytopic", "mysubscription");
```

## <a name="next-steps"></a>Seuraavat vaiheet

Nyt kun olet tutustunut palvelun Bus olevien perusteet, katso lisätietoja [olevien, aiheet, ja tilaukset][] .

[Olevien, aiheet ja tilaukset]: service-bus-queues-topics-subscriptions.md
[sqlfilter]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx
[require-once]: http://php.net/require_once
[Palvelun Bus kiintiön]: service-bus-quotas.md
