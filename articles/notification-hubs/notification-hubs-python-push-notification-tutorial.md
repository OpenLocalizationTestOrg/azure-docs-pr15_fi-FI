<properties 
    pageTitle="Voit käyttää ilmoituksen keskittimet Python" 
    description="Opi käyttämään Azure ilmoituksen keskittimet Python taustatietokannan." 
    services="notification-hubs" 
    documentationCenter="" 
    authors="ysxu"
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="notification-hubs" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="python" 
    ms.devlang="php" 
    ms.topic="article" 
    ms.date="06/29/2016" 
    ms.author="yuaxu"/>

# <a name="how-to-use-notification-hubs-from-python"></a>Opi käyttämään ilmoituksen keskittimet Python kohteesta
[AZURE.INCLUDE [notification-hubs-backend-how-to-selector](../../includes/notification-hubs-backend-how-to-selector.md)]
        
Voit käyttää kaikkia ilmoituksen keskittimet ominaisuuksia Java/PHP/Python ja Ruby taustatietokantaan ilmoituksen keskittimeen REST-käyttöliittymän avulla [Ilmoituksen keskittimet REST API](http://msdn.microsoft.com/library/dn223264.aspx)MSDN-ohjeaiheessa kuvatulla tavalla.

> [AZURE.NOTE] Tämä on esimerkki viittaus toteutus soveltamisesta lähettää ilmoituksen Python ja ei ole julkisesti tuettuihin ilmoitukset keskittimeen Python SDK.
>
> Tässä esimerkissä on kirjoitettu Python 3.4.

Tässä ohjeaiheessa on Näytä Toimintaohje:

* Luoda ilmoituksen keskittimet tarkoitettuja Python muiden asiakas.
* Lähetä ilmoitus keskittimeen REST API Python käyttöliittymässä ilmoitukset. 
* Pyydä dump HTTP muiden pyynnön ja vastauksen virheenkorjaus/oppilaitoksille tarkoitusta varten. 

Noudattaa [Get aloittaminen opetusohjelma](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) for mobile ympäristö värisinä, toteuttaminen Python taustatietokantaan-osaan.

> [AZURE.NOTE] Otoksen laajuutta on rajallinen, ilmoitukset lähetetään, ja se ei tee mitään rekisteröinnin hallinta.

## <a name="client-interface"></a>Asiakas-käyttöliittymä
Tärkeimmät asiakkaan käyttöliittymässä tarjota samoja menetelmiä, jotka ovat käytettävissä [.NET ilmoituksen keskittimet SDK](http://msdn.microsoft.com/library/jj933431.aspx). Tämä avulla voit kääntää suoraan opetusohjelmat ja tämän sivuston tällä hetkellä käytettävissä olevat mallit ja siirtämien internet-yhteisöön.

Löydät kaikki käytettävissä olevat [Python muiden paketti otoksen]koodi.

Jos esimerkiksi haluat luoda asiakkaan:

    isDebug = True
    hub = NotificationHub("myConnectionString", "myNotificationHubName", isDebug)
    
Voit lähettää Windows ilmoitusruudun seuraavasti:
    
    wns_payload = """<toast><visual><binding template=\"ToastText01\"><text id=\"1\">Hello world!</text></binding></visual></toast>"""
    hub.send_windows_notification(wns_payload)
    
## <a name="implementation"></a>Käyttöönotto
Jos et ole jo, noudata Microsoftin [Get aloittaminen opetusohjelma] viimeinen osa, joihin on toteuttamisesta sitten taustatietokantatiedosto ylöspäin.

Kaikki tiedot, joista Toteuta koko REST-paketti löytyy [MSDN](http://msdn.microsoft.com/library/dn530746.aspx)-sivuston. Tässä osassa on kuvataan tarvitse käyttää ilmoituksen keskittimet muiden päätepisteet ja ilmoitukset lähetetään päävaiheet Python soveltaminen

1. Yhteysmerkkijonon jäsentää
2. Luo luvan tunnus
3. Lähetä käyttämällä HTTP REST API-ilmoitus

### <a name="parse-the-connection-string"></a>Yhteysmerkkijonon jäsentää

Näin käyttöönoton asiakas, jonka konstruktori jäsentää yhteysmerkkijonon tärkeimmät luokan:

    class NotificationHub:
        API_VERSION = "?api-version=2013-10"
        DEBUG_SEND = "&test"
    
        def __init__(self, connection_string=None, hub_name=None, debug=0):
            self.HubName = hub_name
            self.Debug = debug
    
            # Parse connection string
            parts = connection_string.split(';')
            if len(parts) != 3:
                raise Exception("Invalid ConnectionString.")
    
            for part in parts:
                if part.startswith('Endpoint'):
                    self.Endpoint = 'https' + part[11:]
                if part.startswith('SharedAccessKeyName'):
                    self.SasKeyName = part[20:]
                if part.startswith('SharedAccessKey'):
                    self.SasKeyValue = part[16:]


### <a name="create-security-token"></a>Luo suojaustunnus
Suojaus-tunnuksen luominen tiedot ovat käytettävissä [seuraavassa](http://msdn.microsoft.com/library/dn495627.aspx).
Seuraavilla tavoilla on lisättävä **NotificationHub** -luokan luominen tunnuksen perusteella URI nykyisen pyynnön ja yhteysmerkkijonon poimittujen tunnistetiedot.

    @staticmethod
    def get_expiry():
        # By default returns an expiration of 5 minutes (=300 seconds) from now
        return int(round(time.time() + 300))

    @staticmethod
    def encode_base64(data):
        return base64.b64encode(data)

    def sign_string(self, to_sign):
        key = self.SasKeyValue.encode('utf-8')
        to_sign = to_sign.encode('utf-8')
        signed_hmac_sha256 = hmac.HMAC(key, to_sign, hashlib.sha256)
        digest = signed_hmac_sha256.digest()
        encoded_digest = self.encode_base64(digest)
        return encoded_digest

    def generate_sas_token(self):
        target_uri = self.Endpoint + self.HubName
        my_uri = urllib.parse.quote(target_uri, '').lower()
        expiry = str(self.get_expiry())
        to_sign = my_uri + '\n' + expiry
        signature = urllib.parse.quote(self.sign_string(to_sign))
        auth_format = 'SharedAccessSignature sig={0}&se={1}&skn={2}&sr={3}'
        sas_token = auth_format.format(signature, expiry, self.SasKeyName, my_uri)
        return sas_token

### <a name="send-a-notification-using-http-rest-api"></a>Lähetä käyttämällä HTTP REST API-ilmoitus
Ensimmäinen, anna Käytä Määritä luokka, joka edustaa ilmoituksen.

    class Notification:
        def __init__(self, notification_format=None, payload=None, debug=0):
            valid_formats = ['template', 'apple', 'gcm', 'windows', 'windowsphone', "adm", "baidu"]
            if not any(x in notification_format for x in valid_formats):
                raise Exception(
                    "Invalid Notification format. " +
                    "Must be one of the following - 'template', 'apple', 'gcm', 'windows', 'windowsphone', 'adm', 'baidu'")
    
            self.format = notification_format
            self.payload = payload
    
            # array with keynames for headers
            # Note: Some headers are mandatory: Windows: X-WNS-Type, WindowsPhone: X-NotificationType
            # Note: For Apple you can set Expiry with header: ServiceBusNotification-ApnsExpiry
            # in W3C DTF, YYYY-MM-DDThh:mmTZD (for example, 1997-07-16T19:20+01:00).
            self.headers = None

Tämä luokka on säilön alkuperäisen ilmoituksen tekstissä tai joukko ominaisuuksia, jos mallin ilmoituksen ylä joukko, joka sisältää muodossa (alkuperäisen ympäristö tai mallin) ja käyttöjärjestelmäkohtaiset ominaisuuksia (kuten Apple vanheneminen-ominaisuus ja WNS otsikot).

Tutustu [ilmoituksen keskittimet REST API-asiakirjat](http://msdn.microsoft.com/library/dn495827.aspx) ja kaikki käytettävissä olevat vaihtoehdot tietyn ilmoituksen ympäristöjen muodot.

Nyt tähän luokkaan olemme voit kirjoittaa Lähetä ilmoitus menetelmiä sisältyy **NotificationHub** -luokka.

    def make_http_request(self, url, payload, headers):
        parsed_url = urllib.parse.urlparse(url)
        connection = http.client.HTTPSConnection(parsed_url.hostname, parsed_url.port)

        if self.Debug > 0:
            connection.set_debuglevel(self.Debug)
            # adding this querystring parameter gets detailed information about the PNS send notification outcome
            url += self.DEBUG_SEND
            print("--- REQUEST ---")
            print("URI: " + url)
            print("Headers: " + json.dumps(headers, sort_keys=True, indent=4, separators=(' ', ': ')))
            print("--- END REQUEST ---\n")

        connection.request('POST', url, payload, headers)
        response = connection.getresponse()

        if self.Debug > 0:
            # print out detailed response information for debugging purpose
            print("\n\n--- RESPONSE ---")
            print(str(response.status) + " " + response.reason)
            print(response.msg)
            print(response.read())
            print("--- END RESPONSE ---")

        elif response.status != 201:
            # Successful outcome of send message is HTTP 201 - Created
            raise Exception(
                "Error sending notification. Received HTTP code " + str(response.status) + " " + response.reason)

        connection.close()

    def send_notification(self, notification, tag_or_tag_expression=None):
        url = self.Endpoint + self.HubName + '/messages' + self.API_VERSION

        json_platforms = ['template', 'apple', 'gcm', 'adm', 'baidu']

        if any(x in notification.format for x in json_platforms):
            content_type = "application/json"
            payload_to_send = json.dumps(notification.payload)
        else:
            content_type = "application/xml"
            payload_to_send = notification.payload

        headers = {
            'Content-type': content_type,
            'Authorization': self.generate_sas_token(),
            'ServiceBusNotification-Format': notification.format
        }

        if isinstance(tag_or_tag_expression, set):
            tag_list = ' || '.join(tag_or_tag_expression)
        else:
            tag_list = tag_or_tag_expression

        # add the tags/tag expressions to the headers collection
        if tag_list != "":
            headers.update({'ServiceBusNotification-Tags': tag_list})

        # add any custom headers to the headers collection that the user may have added
        if notification.headers is not None:
            headers.update(notification.headers)

        self.make_http_request(url, payload_to_send, headers)

    def send_apple_notification(self, payload, tags=""):
        nh = Notification("apple", payload)
        self.send_notification(nh, tags)

    def send_gcm_notification(self, payload, tags=""):
        nh = Notification("gcm", payload)
        self.send_notification(nh, tags)

    def send_adm_notification(self, payload, tags=""):
        nh = Notification("adm", payload)
        self.send_notification(nh, tags)

    def send_baidu_notification(self, payload, tags=""):
        nh = Notification("baidu", payload)
        self.send_notification(nh, tags)

    def send_mpns_notification(self, payload, tags=""):
        nh = Notification("windowsphone", payload)

        if "<wp:Toast>" in payload:
            nh.headers = {'X-WindowsPhone-Target': 'toast', 'X-NotificationClass': '2'}
        elif "<wp:Tile>" in payload:
            nh.headers = {'X-WindowsPhone-Target': 'tile', 'X-NotificationClass': '1'}

        self.send_notification(nh, tags)

    def send_windows_notification(self, payload, tags=""):
        nh = Notification("windows", payload)

        if "<toast>" in payload:
            nh.headers = {'X-WNS-Type': 'wns/toast'}
        elif "<tile>" in payload:
            nh.headers = {'X-WNS-Type': 'wns/tile'}
        elif "<badge>" in payload:
            nh.headers = {'X-WNS-Type': 'wns/badge'}

        self.send_notification(nh, tags)

    def send_template_notification(self, properties, tags=""):
        nh = Notification("template", properties)
        self.send_notification(nh, tags)

Edellä mainittuja menetelmiä Lähetä ilmoitus-toiminnossa oikea leipätekstin ja otsikot lähetetään ilmoitus /messages päätepiste HTTP POST-pyynnön.

### <a name="using-debug-property-to-enable-detailed-logging"></a>Yksityiskohtainen kirjaus käyttöön virheenkorjaus-ominaisuuden avulla
Ottaminen käyttöön virheenkorjaus ominaisuuden valmisteltaessa ilmoitus-toiminnossa kirjoittaa yksityiskohtainen kirjaus liittyviä tietoja HTTP-pyynnön ja vastauksen dump sekä yksityiskohtaiset ilmoitusviestin lähettäminen tulos. Olemme lisättiin äskettäin tätä ominaisuutta kutsutaan [ilmoituksen keskittimet TestSend ominaisuus](http://msdn.microsoft.com/library/microsoft.servicebus.notifications.notificationhubclient.enabletestsend.aspx) , joka palauttaa ilmoituksen Lähetä tulokset yksityiskohtaisia tietoja. Voit käyttää sitä - alustaa käyttämällä seuraavaa:

    hub = NotificationHub("myConnectionString", "myNotificationHubName", isDebug)

Ilmoitus-toiminnossa Lähetä-pyyntö HTTP URL-osoite saa lisätty "testi" querystring tuloksena. 

##<a name="complete-tutorial"></a>Viimeistele opetusohjelman
Nyt voit tehdä aloittaminen-opetusohjelma lähettämällä ilmoituksen Python taustatietokannan.

Alusta ilmoituksen keskittimet asiakkaan (Vaihda yhteyden merkkijono ja keskittimeen nimi kuin instructed [Get aloittaminen opetusohjelma]):

    hub = NotificationHub("myConnectionString", "myNotificationHubName")

Lisää sen mukaan, että kannettavan kohdeympäristö Lähetä-koodi. Tässä esimerkissä Lisää myös suurempi tason menetelmät, jotka mahdollistavat lähettämisen ympäristössä, kuten Windows; send_windows_notification perustuvat ilmoitukset send_apple_notification (for apple) jne. 

### <a name="windows-store-and-windows-phone-81-non-silverlight"></a>Windows-kaupan ja Windows Phone 8.1 (-Silverlight)

    wns_payload = """<toast><visual><binding template=\"ToastText01\"><text id=\"1\">Test</text></binding></visual></toast>"""
    hub.send_windows_notification(wns_payload)

### <a name="windows-phone-80-and-81-silverlight"></a>Windows Phone 8.0 ja 8.1 Silverlight

    hub.send_mpns_notification(toast)

### <a name="ios"></a>iOS

    alert_payload = {
        'data':
            {
                'msg': 'Hello!'
            }
    }
    hub.send_apple_notification(alert_payload)

### <a name="android"></a>Android-laitteeseen
    gcm_payload = {
        'data':
            {
                'msg': 'Hello!'
            }
    }
    hub.send_gcm_notification(gcm_payload)

### <a name="kindle-fire"></a>Kindle Fireen
    adm_payload = {
        'data':
            {
                'msg': 'Hello!'
            }
    }
    hub.send_adm_notification(adm_payload)

### <a name="baidu"></a>Baidu
    baidu_payload = {
        'data':
            {
                'msg': 'Hello!'
            }
    }
    hub.send_baidu_notification(baidu_payload)

Python koodin suorittaminen tulee tuottaa näkyvä kohde laitteen ilmoituksen.

## <a name="examples"></a>Esimerkkejä:

### <a name="enabling-debug-property"></a>Ottaminen käyttöön virheenkorjaus-ominaisuus
Kun otat virheenkorjaus merkinnän valmisteltaessa NotificationHub sitten näet yksityiskohtaisia HTTP-pyynnön ja vastauksen dump sekä NotificationOutcome seuraavalta jos ymmärrät mitä HTTP-otsikot on välitetty pyynnön ja mitä HTTP-vastaus vastaanotettiin ilmoitus-toiminnosta    ![][1]

Näet yksityiskohtaisia ilmoituksen keskittimeen tuloksen esimerkiksi 

- Kun viesti lähetetään onnistuneesti Push-ilmoituspalvelu. 
    
        <Outcome>The Notification was successfully sent to the Push Notification System</Outcome>

- Jos kohteita ei löydy push-ilmoitusta ei ollut sitten todennäköisesti aiot on kohdassa vastauksessa (mikä tarkoittaa, että käytettävissä on ei löydy aikana ilmoituksen todennäköisesti, koska merkintöjä joidenkin ristiriitaiset tunnisteiden merkintöjen)

        '<NotificationOutcome xmlns="http://schemas.microsoft.com/netservices/2010/10/servicebus/connect" xmlns:i="http://www.w3.org/2001/XMLSchema-instance"><Success>0</Success><Failure>0</Failure><Results i:nil="true"/></NotificationOutcome>'

### <a name="broadcast-toast-notification-to-windows"></a>Lähetä ilmoitusruudun Windowsiin 

Huomaa otsikot, jotka saat lähetetään, kun lähetät lähetyksen ilmoitusruudun Windows-asiakas. 

    hub.send_windows_notification(wns_payload)

![][2]

### <a name="send-notification-specifying-a-tag-or-tag-expression"></a>Lähetä ilmoitus määrittäminen tunniste (tai Tunnistelauseke)

Huomaa, johon ne lisätään HTTP-pyyntö tunnisteet HTTP-otsikon (Seuraavassa esimerkissä on lähetetään ilmoitus vain merkintöjen 'urheilu-tietojen kanssa)

    hub.send_windows_notification(wns_payload, "sports")

![][3]

### <a name="send-notification-specifying-multiple-tags"></a>Lähetä ilmoitus, joka määrittää useita tunnisteita

Näet, kuinka tunnisteet HTTP-otsikko muuttuu, kun useita tunnisteita lähetetään. 
    
    tags = {'sports', 'politics'}
    hub.send_windows_notification(wns_payload, tags)

![][4]

### <a name="templated-notification"></a>Malliin perustuvan ilmoitus

Huomaa, että muoto HTTP-otsikko muuttuu ja sisällön tekstissä lähetetään osana HTTP pyynnön tekstiin:

**Asiakaspuolen - rekisteröidyn mallia**

        var template =
                        @"<toast><visual><binding template=""ToastText01""><text id=""1"">$(greeting_en)</text></binding></visual></toast>";
            
**Palvelinpuolen - lähettäminen paketti**
        
        template_payload = {'greeting_en': 'Hello', 'greeting_fr': 'Salut'}
        hub.send_template_notification(template_payload)

![][5]


## <a name="next-steps"></a>Seuraavat vaiheet
Tässä ohjeaiheessa on osoittanut yksinkertainen Python REST-asiakasohjelman ilmoituksen keskittimien luomisesta. Täällä voit tehdä seuraavaa:

* Lataa koko [Python REST-paketti-Esimerkki], jossa on yllä olevan koodin.
* Lue lisää ilmoituksen keskittimet tunnisteita ominaisuus [purkaa uutisia opetusohjelma]
* Lue lisää ilmoituksen keskittimet mallien ominaisuus [lokalisoiminen uutisia opetusohjelma]

<!-- URLs -->
[Python REST-paketti-Esimerkki]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/notificationhubs-rest-python
[Hae aloittaminen-opetusohjelma]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-get-started/
[Purkaa uutiset-opetusohjelma]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-send-breaking-news/
[Lokalisoiminen uutiset-opetusohjelma]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-send-localized-breaking-news/

<!-- Images. -->
[1]: ./media/notification-hubs-python-backend-how-to/DetailedLoggingInfo.png
[2]: ./media/notification-hubs-python-backend-how-to/BroadcastScenario.png
[3]: ./media/notification-hubs-python-backend-how-to/SendWithOneTag.png
[4]: ./media/notification-hubs-python-backend-how-to/SendWithMultipleTags.png
[5]: ./media/notification-hubs-python-backend-how-to/TemplatedNotification.png
 
