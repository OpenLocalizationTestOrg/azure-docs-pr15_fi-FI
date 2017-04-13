<properties 
    pageTitle="Voit käyttää ilmoituksen keskittimet PHP" 
    description="Opi käyttämään Azure ilmoituksen keskittimet PHP taustatietokannan." 
    services="notification-hubs" 
    documentationCenter="" 
    authors="ysxu" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="notification-hubs" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="php" 
    ms.devlang="php" 
    ms.topic="article" 
    ms.date="06/07/2016" 
    ms.author="yuaxu"/>

# <a name="how-to-use-notification-hubs-from-php"></a>Opi käyttämään ilmoituksen keskittimet PHP kohteesta
[AZURE.INCLUDE [notification-hubs-backend-how-to-selector](../../includes/notification-hubs-backend-how-to-selector.md)]

Voit käyttää kaikkia ilmoituksen keskittimet ominaisuuksia Java/PHP ja Ruby taustatietokantaan ilmoituksen keskittimeen REST-käyttöliittymän avulla [Ilmoituksen keskittimet REST API](http://msdn.microsoft.com/library/dn223264.aspx)MSDN-ohjeaiheessa kuvatulla tavalla.

Tässä ohjeaiheessa on Näytä Toimintaohje:

* Muodosta REST-asiakasohjelman ilmoituksen keskittimet tarkoitettuja PHP;
* Noudata [Get aloittaminen opetusohjelma](notification-hubs-ios-apple-push-notification-apns-get-started.md) mobile ympäristö värisinä, toteuttaminen PHP taustatietokantaan-osaan.

## <a name="client-interface"></a>Asiakas-käyttöliittymä
Tärkeimmät asiakkaan käyttöliittymässä tarjoavat samoja menetelmiä, jotka ovat käytettävissä [.NET ilmoituksen keskittimet SDK](http://msdn.microsoft.com/library/jj933431.aspx), tämän avulla voit kääntää suoraan opetusohjelmat ja tämän sivuston tällä hetkellä käytettävissä olevat mallit ja siirtämien internet-yhteisöön.

Löydät kaikki käytettävissä olevat [PHP muiden paketti otoksen]koodi.

Jos esimerkiksi haluat luoda asiakkaan:

    $hub = new NotificationHub("connection string", "hubname"); 

Voit lähettää iOS alkuperäisen ilmoituksen seuraavasti:
    
    $notification = new Notification("apple", '{"aps":{"alert": "Hello!"}}');
    $hub->sendNotification($notification, null);

## <a name="implementation"></a>Käyttöönotto
Jos et ole jo, noudata Microsoftin [Get aloittaminen opetusohjelma] viimeinen osa, joihin on toteuttamisesta sitten taustatietokantatiedosto ylöspäin.
Lisäksi voit halutessasi voit käyttää koodia [PHP muiden paketti otoksen] ja siirry suoraan kohtaan [valmiina opetusohjelman](#complete-tutorial) .

Kaikki tiedot, joista Toteuta koko REST-paketti löytyy [MSDN](http://msdn.microsoft.com/library/dn530746.aspx)-sivuston. Tässä osassa on kuvataan PHP käyttöönoton ilmoituksen keskittimet muiden päätepisteet käyttämiseen tarvittavat päävaihetta:

1. Yhteysmerkkijonon jäsentää
2. Luo luvan tunnus
3. Suorittaa HTTP-kutsu

### <a name="parse-the-connection-string"></a>Yhteysmerkkijonon jäsentää

Näin käyttöönoton asiakas, jonka konstruktoria, joka jäsentää yhteysmerkkijonon tärkeimmät luokan:

    class NotificationHub {
        const API_VERSION = "?api-version=2013-10";
    
        private $endpoint;
        private $hubPath;
        private $sasKeyName;
        private $sasKeyValue;
    
        function __construct($connectionString, $hubPath) {
            $this->hubPath = $hubPath;
    
            $this->parseConnectionString($connectionString);
        }
    
        private function parseConnectionString($connectionString) {
            $parts = explode(";", $connectionString);
            if (sizeof($parts) != 3) {
                throw new Exception("Error parsing connection string: " . $connectionString);
            }
    
            foreach ($parts as $part) {
                if (strpos($part, "Endpoint") === 0) {
                    $this->endpoint = "https" . substr($part, 11);
                } else if (strpos($part, "SharedAccessKeyName") === 0) {
                    $this->sasKeyName = substr($part, 20);
                } else if (strpos($part, "SharedAccessKey") === 0) {
                    $this->sasKeyValue = substr($part, 16);
                }
            }
        }
    }


### <a name="create-security-token"></a>Luo suojaustunnus
Suojaus-tunnuksen luominen tiedot ovat käytettävissä [seuraavassa](http://msdn.microsoft.com/library/dn495627.aspx).
Seuraavalla tavalla on lisättävä **NotificationHub** -luokan luominen tunnuksen perusteella URI nykyisen pyynnön ja yhteysmerkkijonon poimittujen tunnistetiedot.

    private function generateSasToken($uri) {
        $targetUri = strtolower(rawurlencode(strtolower($uri)));

        $expires = time();
        $expiresInMins = 60;
        $expires = $expires + $expiresInMins * 60;
        $toSign = $targetUri . "\n" . $expires;

        $signature = rawurlencode(base64_encode(hash_hmac('sha256', $toSign, $this->sasKeyValue, TRUE)));

        $token = "SharedAccessSignature sr=" . $targetUri . "&sig="
                    . $signature . "&se=" . $expires . "&skn=" . $this->sasKeyName;

        return $token;
    }

### <a name="send-a-notification"></a>Lähetä ilmoitus
Anna meidän määrittää ensin kuvaavan ilmoituksen luokan.

    class Notification {
        public $format;
        public $payload;
    
        # array with keynames for headers
        # Note: Some headers are mandatory: Windows: X-WNS-Type, WindowsPhone: X-NotificationType
        # Note: For Apple you can set Expiry with header: ServiceBusNotification-ApnsExpiry in W3C DTF, YYYY-MM-DDThh:mmTZD (for example, 1997-07-16T19:20+01:00).
        public $headers;
    
        function __construct($format, $payload) {
            if (!in_array($format, ["template", "apple", "windows", "gcm", "windowsphone"])) {
                throw new Exception('Invalid format: ' . $format);
            }
    
            $this->format = $format;
            $this->payload = $payload;
        }
    }

Tämä luokka on säilön alkuperäisen ilmoitus-tekstissä tai kotelossa mallin ilmoituksen ominaisuudet ja otsikot joukko sisältää muodossa (alkuperäisen ympäristö tai mallin) ja käyttöjärjestelmäkohtaiset ominaisuuksia (kuten Apple vanheneminen-ominaisuus ja WNS otsikot).

Tutustu [ilmoituksen keskittimet REST API-asiakirjat](http://msdn.microsoft.com/library/dn495827.aspx) ja kaikki käytettävissä olevat vaihtoehdot tietyn ilmoituksen ympäristöjen muodot.

Tietoturvariskien tähän luokkaan, on nyt kirjoittaa Lähetä ilmoitus menetelmiä sisältyy **NotificationHub** -luokka.

    public function sendNotification($notification, $tagsOrTagExpression="") {
        if (is_array($tagsOrTagExpression)) {
            $tagExpression = implode(" || ", $tagsOrTagExpression);
        } else {
            $tagExpression = $tagsOrTagExpression;
        }

        # build uri
        $uri = $this->endpoint . $this->hubPath . "/messages" . NotificationHub::API_VERSION;
        $ch = curl_init($uri);

        if (in_array($notification->format, ["template", "apple", "gcm"])) {
            $contentType = "application/json";
        } else {
            $contentType = "application/xml";
        }

        $token = $this->generateSasToken($uri);

        $headers = [
            'Authorization: '.$token,
            'Content-Type: '.$contentType,
            'ServiceBusNotification-Format: '.$notification->format
        ];

        if ("" !== $tagExpression) {
            $headers[] = 'ServiceBusNotification-Tags: '.$tagExpression;
        }

        # add headers for other platforms
        if (is_array($notification->headers)) {
            $headers = array_merge($headers, $notification->headers);
        }
        
        curl_setopt_array($ch, array(
            CURLOPT_POST => TRUE,
            CURLOPT_RETURNTRANSFER => TRUE,
            CURLOPT_SSL_VERIFYPEER => FALSE,
            CURLOPT_HTTPHEADER => $headers,
            CURLOPT_POSTFIELDS => $notification->payload
        ));

        // Send the request
        $response = curl_exec($ch);

        // Check for errors
        if($response === FALSE){
            throw new Exception(curl_error($ch));
        }

        $info = curl_getinfo($ch);

        if ($info['http_code'] <> 201) {
            throw new Exception('Error sending notificaiton: '. $info['http_code'] . ' msg: ' . $response);
        }
    } 

Edellä mainittuja menetelmiä Lähetä ilmoitus-toiminnossa oikea leipätekstin ja otsikot lähetetään ilmoitus /messages päätepiste HTTP POST-pyynnön.

##<a name="complete-tutorial"></a>Viimeistele opetusohjelman
Nyt voit tehdä aloittaminen-opetusohjelma lähettämällä ilmoituksen PHP taustatietokannan.

Alusta ilmoituksen keskittimet asiakkaan (Vaihda yhteyden merkkijono ja keskittimeen nimi kuin instructed [Get aloittaminen opetusohjelma]):

    $hub = new NotificationHub("connection string", "hubname"); 

Lisää sen mukaan, että kannettavan kohdeympäristö Lähetä-koodi.

### <a name="windows-store-and-windows-phone-81-non-silverlight"></a>Windows-kaupan ja Windows Phone 8.1 (-Silverlight)

    $toast = '<toast><visual><binding template="ToastText01"><text id="1">Hello from PHP!</text></binding></visual></toast>';
    $notification = new Notification("windows", $toast);
    $notification->headers[] = 'X-WNS-Type: wns/toast';
    $hub->sendNotification($notification, null);

### <a name="ios"></a>iOS

    $alert = '{"aps":{"alert":"Hello from PHP!"}}';
    $notification = new Notification("apple", $alert);
    $hub->sendNotification($notification, null);

### <a name="android"></a>Android-laitteeseen
    $message = '{"data":{"msg":"Hello from PHP!"}}';
    $notification = new Notification("gcm", $message);
    $hub->sendNotification($notification, null);

### <a name="windows-phone-80-and-81-silverlight"></a>Windows Phone 8.0 ja 8.1 Silverlight

    $toast = '<?xml version="1.0" encoding="utf-8"?>' .
                '<wp:Notification xmlns:wp="WPNotification">' .
                   '<wp:Toast>' .
                        '<wp:Text1>Hello from PHP!</wp:Text1>' .
                   '</wp:Toast> ' .
                '</wp:Notification>';
    $notification = new Notification("windowsphone", $toast);
    $notification->headers[] = 'X-WindowsPhone-Target : toast';
    $notification->headers[] = 'X-NotificationClass : 2';
    $hub->sendNotification($notification, null);


### <a name="kindle-fire"></a>Kindle Fireen
    $message = '{"data":{"msg":"Hello from PHP!"}}';
    $notification = new Notification("adm", $message);
    $hub->sendNotification($notification, null);

PHP koodin suorittaminen tulee tuottaa nyt näkyvä kohde laitteen ilmoituksen.


## <a name="next-steps"></a>Seuraavat vaiheet
Tässä ohjeaiheessa on osoittanut yksinkertainen Java REST-asiakasohjelman ilmoituksen keskittimien luomisesta. Täällä voit tehdä seuraavaa:

* Lataa koko [PHP REST-paketti-Esimerkki], jossa on yllä olevan koodin.
* Lue lisää ilmoituksen keskittimet tunnisteita ominaisuus [purkaa uutisia opetusohjelmassa]
* Lisätietoja ilmoitusten valitseminen yksittäisille käyttäjille [Ilmoita käyttäjille opetusohjelmassa]

Lisätietoja on Katso myös [PHP Developer Center](/develop/php/).

[PHP REST-paketti-Esimerkki]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/notificationhubs-rest-php
[Hae aloittaminen-opetusohjelma]: http://azure.microsoft.com/documentation/articles/notification-hubs-ios-get-started/
 
