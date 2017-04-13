<properties
    pageTitle="Kirjaudu Analytics HTTP tietojen kerääminen API | Microsoft Azure"
    description="Voit Log Analytics HTTP tietojen kerääminen Ohjelmointirajapinnan Log Analytics-tietovarasto kirjaa JSON-tietojen lisääminen asiakas, joka voi soittaa REST-Ohjelmointirajapinnalla. Tässä artikkelissa kerrotaan, miten voit käyttää Ohjelmointirajapinnan ja on esimerkkejä siitä, miten voit julkaista tietoja käyttämällä ohjelmoinnin kielten."
    services="log-analytics"
    documentationCenter=""
    authors="bwren"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/26/2016"
    ms.author="bwren"/>


# <a name="log-analytics-http-data-collector-api"></a>Kirjaudu Analytics HTTP tietojen kerääminen Ohjelmointirajapinta

Käytettäessä Azure Log Analytics HTTP tietojen kerääminen Ohjelmointirajapinta, voit lisätä kirjaa JavaScript Object Notation (JSON) tietojen Log Analytics-säilöön asiakaskoneelta, joka voi soittaa REST-Ohjelmointirajapinnalla. Tällä menetelmällä voit lähettää tiedot kolmansien osapuolien sovellukset tai komentosarjat, kuten from Azure automaatio-runbookin.  

## <a name="create-a-request"></a>Luo pyyntö

Seuraavat kaksi taulukoissa on lueteltu määritteet, joita tarvitaan sivupyynnön Log Analytics HTTP tietojen kerääminen ohjelmointirajapinnan. Olemme kuvataan tarkemmin jäljempänä on artikkelissa kunkin määritteen.

### <a name="request-uri"></a>Pyynnön URI

| Määrite | Ominaisuus |
|:--|:--|
| Menetelmä | KIRJAA |
| URI | https://\<Asiakastunnus\>.ods.opinsights.azure.com/api/logs?api-version=2016-04-01 |
| Sisältötyyppi | sovelluksen/json |

### <a name="request-uri-parameters"></a>Pyynnön URI-parametrit
| Parametri | Kuvaus |
|:--|:--|
| Asiakastunnus  | Microsoft toimintojen hallinta Suite työtilan yksilöllinen. |
| Resurssi    | API resurssinimi: / api/lokitiedot. |
| API-versio | Ohjelmointirajapinnan käyttäminen pyyntö versio. Ei tällä hetkellä 2016-04-01. |

### <a name="request-headers"></a>Pyydä otsikot
| Ylätunniste | Kuvaus |
|:--|:--|
| Todennus | Todennus-allekirjoitus. Artikkelin voit lukea luomisesta HMAC SHA256 otsikko. |
| Lokitiedoston tyyppi | Määritä tiedot, jotka lähetetään tietuetyyppiä. Lokitiedoston tyyppi tukee tällä hetkellä vain alfa merkkejä. Se ei tue ovat numeeriset arvot tai erikoismerkkejä. |
| x-ms-päivämäärä | Päivämäärä, jolloin pyynnön käsiteltiin RFC 1123-muodossa. |
| aika-luotu-kenttä | Tietoja, jotka sisältävät tieto-osa aikaleima kentän nimi. Jos määrität kentän **TimeGenerated**käytetään sen sisältöä. Jos kentässä ei ole määritetty, **TimeGenerated** oletusarvo on aika, viestin nautittuina. Sanoma-kentän sisällön pitäisi seurata ISO 8601-muodossa YYYY-MM-DDThh:mm:ssZ. |


## <a name="authorization"></a>Todennus

Pyyntö Log Analytics HTTP tietojen kerääminen ohjelmointirajapinnan on oltava authorization-otsikko. Suorittaa todennusta pyyntö, sinun on kirjauduttava ensisijaisen tai toissijaisen avaimen työtilan, joka tekee pyynnön pyynnön. Välittää sitten allekirjoituksen pyynnön osana.   

Näin authorization-otsikko muoto:

```
Authorization: SharedKey <WorkspaceID>:<Signature>
```

*WorkspaceID* on yksilöllinen toimintojen hallinta Suite työtilan. *Allekirjoitus* on [Hash-based viestin käyttöoikeuksien koodin (HMAC)](https://msdn.microsoft.com/library/system.security.cryptography.hmacsha256.aspx) , joka on rakennettava pyynnöstä ja sitten luvut [SHA256 algoritmin](https://msdn.microsoft.com/library/system.security.cryptography.sha256.aspx)avulla. Tämän jälkeen voit koodata se käyttämällä Base64 koodaus.

Käytä tätä muotoa koodata **SharedKey** allekirjoitus-merkkijono:

```
StringToSign = VERB + "\n" +
               Content-Length + "\n" +
               Content-Type + "\n" +
               x-ms-date + "\n" +
               "/api/logs";
```

Tässä on esimerkki allekirjoituksen merkkijonon:

```
POST\n1024\napplication/json\nx-ms-date:Mon, 04 Apr 2016 08:00:00 GMT\n/api/logs
```

Kun allekirjoitus-merkkijono, koodata käyttämällä HMAC SHA256 algoritmin UTF-8-koodatun merkkijonon ja koodata Base64 kuin tulos. Käytä tätä muotoa:

```
Signature=Base64(HMAC-SHA256(UTF8(StringToSign)))
```

Seuraavien osien mallit on esimerkkejä-koodin avulla voit luoda authorization-otsikko.

## <a name="request-body"></a>Pyydä tekstissä

JSON on oltava viestin tekstiosaan. Se on oltava vähintään yksi tietueet, joiden ominaisuuden nimen ja arvon tietoparin tässä muodossa:

```
{
"property1": "value1",
" property 2": "value2"
" property 3": "value3",
" property 4": "value4"
}
```

Useita tietueita yhdessä yksittäisen pyynnön voit erän käyttämällä seuraavaa muotoa. Kaikki tietueet on oltava sama tietuetyyppi.

```
{
"property1": "value1",
" property 2": "value2"
" property 3": "value3",
" property 4": "value4"
},
{
"property1": "value1",
" property 2": "value2"
" property 3": "value3",
" property 4": "value4"
}
```

## <a name="record-type-and-properties"></a>Tietueen tyyppi ja ominaisuudet

Voit määrittää mukautetun tietuetyypin lähettäessäsi tietoja Log Analytics HTTP tietojen kerääminen Ohjelmointirajapinnan kautta. Tällä hetkellä et voi kirjoittaa tietoja aiemmin tietuetyyppeihin, jotka on luotu muissa tietotyypit ja ratkaisut. Log Analytics lukee saapuvat tiedot ja luo sitten ominaisuudet, jotka vastaavat kirjoittamiesi arvojen tietotyypit.

Sivupyynnön Log Analytics-ohjelmointirajapinnan on oltava tietuetyypin nimi **Lokitiedoston tyyppi** otsikkoon. Liite **_CL** liitetään automaattisesti nimen kirjoitat erottaa log muista kuin mukautetun lokitiedoston. Esimerkiksi jos kirjoitat nimen **MyNewRecordType**, lokin Analytics luo tietueen tyyppi **MyNewRecordType_CL**. Tämän avulla voit varmistaa, että ristiriitoja ei ole käyttäjän luoma Kirjoita nimet ja niiden toimitettu nykyisen tai tulevan Microsoft-ratkaisujen välillä.

Laji ominaisuuden tietotyyppi, loki Analytics lisää siihen ominaisuuden nimi. Jos ominaisuus on tyhjäarvo-ominaisuus ei sisälly tietueen. Tässä taulukossa luetellaan ominaisuuden tietotyyppi ja vastaavan jälkiliite:

| Ominaisuuden tietotyyppi | Jälkiliite |
|:--|:--|
| Merkkijono    | _S |
| Totuusarvo   | _b |
| Kaksinkertainen    | _d |
| Päivämäärä ja aika | _Liitä tii |
| GUID-TUNNUS      | _g |


Lokitiedoston Analytics käyttää kullekin ominaisuudelle haluamasi tietotyyppi määräytyy sen mukaan, onko uuden tietueen tietuetyypin jo olemassa.

- Jos tietuetyyppi ei ole, loki Analytics Luo uusi tunnus. Lokitiedoston Analytics käyttää JSON tyyppi johtaminen tietotyypin kullekin ominaisuudelle uusi tietue.
- Jos tietuetyyppi ole, loki Analytics yrittää luoda uuden tietueen aiemmin ominaisuuksien perusteella. Jos tietotyyppi uuden tietueen ominaisuus ei vastaa ja ei voi muuntaa aiemmin määritetyn tyypin, tai jos tietue sisältää ominaisuus, joka ei ole, loki Analytics Luo uuden ominaisuuden, jossa on haluamasi liitteen.

Lähetyksen tapahtuma loisi esimerkiksi tietueen kolme ominaisuudet, **number_d**, **boolean_b**ja **string_s**:

![Esimerkki tietueen 1](media/log-analytics-data-collector-api/record-01.png)

Jos olet lähettänyt seuraavan tästä, valitse kaikki arvot, jotka on muotoiltu merkkijonoiksi, ominaisuuksia ei muuttuisi. Nämä arvot voidaan muuntaa aiemmin tietotyypit:

![Esimerkki tietue 2](media/log-analytics-data-collector-api/record-02.png)

Mutta Jos teit seuraavan lomakkeen lähettämisen jälkeen, loki Analytics luoda uusia ominaisuuksia **boolean_d** ja **string_d**. Näitä arvoja ei voi muuntaa:

![Esimerkki tietueen 3](media/log-analytics-data-collector-api/record-03.png)

Jos olet lähettänyt seuraavat tekstiä, valitse ennen tietuetyyppi on luotu, loki Analytics loisi tietueen kolme ominaisuudet, **luku_tot**, **boolean_s**ja **string_s**. Tämän arvon kunkin alkuarvot muotoillaan merkkijonona:

![Esimerkki tietueen 4](media/log-analytics-data-collector-api/record-04.png)


## <a name="data-limits"></a>Tietojen rajoitukset
On joitakin rajoituksia lokitiedot Analytics-kokoelmaan API tiedot ympärille.

- Enintään 30 Megatavua Kirjaa lokin Analytics tietojen kerääminen API kohden. Tämä on yhden viestin enimmäiskoon. Jos tiedot yhden viestin, joka on yli 30 Megatavua, olisi pienempi samankokoista joukkojen ylöspäin tiedon jakaminen ja lähettää heille samanaikaisesti. 
- Enimmäismäärä on 32 kt rajoitus kenttien arvoja. Jos kenttäarvo on suurempi kuin 32 kt, tiedot katkaistaan. 
- Kenttien annetun tyypin suositeltu enimmäismäärä on 50. Tämä on käytännön rajoitukset käytettävyyttä ja Etsi kokemus näkökulmasta.  


## <a name="return-codes"></a>Palauttaa koodit

HTTP-tilakoodin 202 tarkoittaa, että pyyntö on hyväksytty käsittelyyn, mutta käsittely ei ole vielä valmis. Tämä ilmaisee, että toiminto onnistui.

Tässä taulukossa luetellaan tilakoodit, jotka palvelun saattaa tuottaa sarja:

| Koodi | Tila | Virhekoodi | Kuvaus |
|:--|:--|:--|:--|
| 202 | Hyväksytty |  | Pyyntö on hyväksytty. |
| 400 | Pyyntö ei kelpaa | InactiveCustomer | Työtila on suljettu. |
| 400 | Pyyntö ei kelpaa | InvalidApiVersion | API-versio, joka on määritetty ei tunnista palvelu. |
| 400 | Pyyntö ei kelpaa | InvalidCustomerId | Määritetty työtilan tunnus on virheellinen. |
| 400 | Pyyntö ei kelpaa | InvalidDataFormat | Virheellinen JSON lähetettiin. Vastauksen teksti saattaa olla lisätietoja siitä, miten voit ratkaista tämän ongelman. |
| 400 | Pyyntö ei kelpaa | InvalidLogType | Lokitiedoston tyyppi määritetty sisältämät erikoismerkkejä tai ovat numeeriset arvot. |
| 400 | Pyyntö ei kelpaa | MissingApiVersion | API-versio ei ole määritetty. |
| 400 | Pyyntö ei kelpaa | MissingContentType | Sisältötyypin ei ole määritetty. |
| 400 | Pyyntö ei kelpaa | MissingLogType | Pakollinen argumentti lokitiedoston tyyppi ei ole määritetty. |
| 400 | Pyyntö ei kelpaa | UnsupportedContentType | Sisältötyypin ei ole määritetty **sovelluksen/json**. |
| 403 | Käyttö estetty | InvalidAuthorization | Palvelu ei voinut todentaa pyynnön. Varmista, että työtilan tunnuksen ja yhteyden avain ovat kelvollisia. |
| 500 | Sisäinen palvelinvirhe | UnspecifiedError | Palvelun sisäinen virhe. Yritä pyynnön. |
| 503 | Palvelu ei ole käytettävissä | ServiceUnavailable | Tällä hetkellä palvelu ei ole käytettävissä vastaanottamaan pyyntöjä. Yritä puhelinnumeroon. |

## <a name="query-data"></a>Kyselyn tiedot

Kyselyn Log Analytics HTTP tietojen kerääminen Ohjelmointirajapinnan lähettämiä tietoja, etsiä tietueita **tyyppi** , joka on yhtä suuri kuin **LogType** -arvo, jonka määritetyn, liitetyt kanssa **_CL**. Esimerkiksi jos olet käyttänyt **MyCustomLog**, valitse voit palauttavat kaikki tietueet, joiden **tyyppi = MyCustomLog_CL**.


## <a name="sample-requests"></a>Malli-pyynnöt

Seuraavien osien löydät voit lähettää tietoja Log Analytics HTTP tietojen kerääminen ohjelmointirajapinnan käyttämällä ohjelmoinnin kielten-objektit.

Tee kunkin näytteen seuraavia ohjeita voit määrittää muuttujat authorization-otsikko:

1. Toimintojen hallinta Suite-portaalissa **asetukset** -ruutu ja valitse sitten **Yhdistetty tietolähteet** -välilehti.
2. Oikealla puolella **Työtilan tunnus**Valitse Kopioi-kuvake ja liitä **Asiakastunnus** muuttujan arvona tunnus.
3. Oikealla puolella **Perusavaimen**Valitse Kopioi-kuvake ja liitä **Jaettu avain** muuttujan arvona tunnus.

Vaihtoehtoisesti voit muuttaa muuttujat lokitiedoston tyyppi ja JSON tiedot.

### <a name="powershell-sample"></a>PowerShell-Esimerkki

```
# Replace with your Workspace ID
$CustomerId = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"  

# Replace with your Primary Key
$SharedKey = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"

# Specify the name of the record type that you'll be creating
$LogType = "MyRecordType"

# Specify a field with the created time for the records
$TimeStampField = "DateValue"


# Create two records with the same set of properties to create
$json = @"
[{  "StringValue": "MyString1",
    "NumberValue": 42,
    "BooleanValue": true,
    "DateValue": "2016-05-12T20:00:00.625Z",
    "GUIDValue": "9909ED01-A74C-4874-8ABF-D2678E3AE23D"
},
{   "StringValue": "MyString2",
    "NumberValue": 43,
    "BooleanValue": false,
    "DateValue": "2016-05-12T20:00:00.625Z",
    "GUIDValue": "8809ED01-A74C-4874-8ABF-D2678E3AE23D"
}]
"@

# Create the function to create the authorization signature
Function Build-Signature ($customerId, $sharedKey, $date, $contentLength, $method, $contentType, $resource)
{
    $xHeaders = "x-ms-date:" + $date
    $stringToHash = $method + "`n" + $contentLength + "`n" + $contentType + "`n" + $xHeaders + "`n" + $resource

    $bytesToHash = [Text.Encoding]::UTF8.GetBytes($stringToHash)
    $keyBytes = [Convert]::FromBase64String($sharedKey)

    $sha256 = New-Object System.Security.Cryptography.HMACSHA256
    $sha256.Key = $keyBytes
    $calculatedHash = $sha256.ComputeHash($bytesToHash)
    $encodedHash = [Convert]::ToBase64String($calculatedHash)
    $authorization = 'SharedKey {0}:{1}' -f $customerId,$encodedHash
    return $authorization
}


# Create the function to create and post the request
Function Post-OMSData($customerId, $sharedKey, $body, $logType)
{
    $method = "POST"
    $contentType = "application/json"
    $resource = "/api/logs"
    $rfc1123date = [DateTime]::UtcNow.ToString("r")
    $contentLength = $body.Length
    $signature = Build-Signature `
        -customerId $customerId `
        -sharedKey $sharedKey `
        -date $rfc1123date `
        -contentLength $contentLength `
        -fileName $fileName `
        -method $method `
        -contentType $contentType `
        -resource $resource
    $uri = "https://" + $customerId + ".ods.opinsights.azure.com" + $resource + "?api-version=2016-04-01"

    $headers = @{
        "Authorization" = $signature;
        "Log-Type" = $logType;
        "x-ms-date" = $rfc1123date;
        "time-generated-field" = $TimeStampField;
    }

    $response = Invoke-WebRequest -Uri $uri -Method $method -ContentType $contentType -Headers $headers -Body $body -UseBasicParsing
    return $response.StatusCode

}

# Submit the data to the API endpoint
Post-OMSData -customerId $customerId -sharedKey $sharedKey -body ([System.Text.Encoding]::UTF8.GetBytes($json)) -logType $logType  
```

### <a name="c-sample"></a>C#-Esimerkki

```
using System;
using System.Net;
using System.Security.Cryptography;

namespace OIAPIExample
{
    class ApiExample
    {
// An example JSON object, with key/value pairs
        static string json = @"[{""DemoField1"":""DemoValue1"",""DemoField2"":""DemoValue2""},{""DemoField1"":""DemoValue3"",""DemoField2"":""DemoValue4""}]";

// Update customerId to your Operations Management Suite workspace ID
        static string customerId = "xxxxxxxx-xxx-xxx-xxx-xxxxxxxxxxxx";

// For sharedKey, use either the primary or the secondary Connected Sources client authentication key   
        static string sharedKey = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx";

// LogName is name of the event type that is being submitted to Log Analytics
        static string LogName = "DemoExample";

// You can use an optional field to specify the timestamp from the data. If the time field is not specified, Log Analytics assumes the time is the message ingestion time
        static string TimeStampField = "";

        static void Main()
        {
// Create a hash for the API signature
            var datestring = DateTime.UtcNow.ToString("r");
            string stringToHash = "POST\n" + json.Length + "\napplication/json\n" + "x-ms-date:" + datestring + "\n/api/logs";
            string hashedString = BuildSignature(stringToHash, sharedKey);
            string signature = "SharedKey " + customerId + ":" + hashedString;

            PostData(signature, datestring, json);
        }

// Build the API signature
        public static string BuildSignature(string message, string secret)
        {
            var encoding = new System.Text.ASCIIEncoding();
            byte[] keyByte = Convert.FromBase64String(secret);
            byte[] messageBytes = encoding.GetBytes(message);
            using (var hmacsha256 = new HMACSHA256(keyByte))
            {
                byte[] hash = hmacsha256.ComputeHash(messageBytes);
                return Convert.ToBase64String(hash);
            }
        }

// Send a request to the POST API endpoint
        public static void PostData(string signature, string date, string json)
        {
            string url = "https://"+ customerId +".ods.opinsights.azure.com/api/logs?api-version=2016-04-01";
            using (var client = new WebClient())
            {
                client.Headers.Add(HttpRequestHeader.ContentType, "application/json");
                client.Headers.Add("Log-Type", LogName);
                client.Headers.Add("Authorization", signature);
                client.Headers.Add("x-ms-date", date);
                client.Headers.Add("time-generated-field", TimeStampField);
                client.UploadString(new Uri(url), "POST", json);
            }
        }
    }
}
```

### <a name="python-sample"></a>Python malli

```
import json
import requests
import datetime
import hashlib
import hmac
import base64

# Update the customer ID to your Operations Management Suite workspace ID
customer_id = 'xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx'

# For the shared key, use either the primary or the secondary Connected Sources client authentication key   
shared_key = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"

# The log type is the name of the event that is being submitted
log_type = 'WebMonitorTest'

# An example JSON web monitor object
json_data = [{
   "slot_ID": 12345,
    "ID": "5cdad72f-c848-4df0-8aaa-ffe033e75d57",
    "availability_Value": 100,
    "performance_Value": 6.954,
    "measurement_Name": "last_one_hour",
    "duration": 3600,
    "warning_Threshold": 0,
    "critical_Threshold": 0,
    "IsActive": "true"
},
{   
    "slot_ID": 67890,
    "ID": "b6bee458-fb65-492e-996d-61c4d7fbb942",
    "availability_Value": 100,
    "performance_Value": 3.379,
    "measurement_Name": "last_one_hour",
    "duration": 3600,
    "warning_Threshold": 0,
    "critical_Threshold": 0,
    "IsActive": "false"
}]
body = json.dumps(json_data)

#####################
######Functions######  
#####################

# Build the API signature
def build_signature(customer_id, shared_key, date, content_length, method, content_type, resource):
    x_headers = 'x-ms-date:' + date
    string_to_hash = method + "\n" + str(content_length) + "\n" + content_type + "\n" + x_headers + "\n" + resource
    bytes_to_hash = bytes(string_to_hash).encode('utf-8')  
    decoded_key = base64.b64decode(shared_key)
    encoded_hash = base64.b64encode(hmac.new(decoded_key, bytes_to_hash, digestmod=hashlib.sha256).digest())
    authorization = "SharedKey {}:{}".format(customer_id,encoded_hash)
    return authorization

# Build and send a request to the POST API
def post_data(customer_id, shared_key, body, log_type):
    method = 'POST'
    content_type = 'application/json'
    resource = '/api/logs'
    rfc1123date = datetime.datetime.utcnow().strftime('%a, %d %b %Y %H:%M:%S GMT')
    content_length = len(body)
    signature = build_signature(customer_id, shared_key, rfc1123date, content_length, method, content_type, resource)
    uri = 'https://' + customer_id + '.ods.opinsights.azure.com' + resource + '?api-version=2016-04-01'

    headers = {
        'content-type': content_type,
        'Authorization': signature,
        'Log-Type': log_type,
        'x-ms-date': rfc1123date
    }

    response = requests.post(uri,data=body, headers=headers)
    if (response.status_code == 202):
        print 'Accepted'
    else:
        print "Response code: {}".format(response.status_code)

post_data(customer_id, shared_key, body, log_type)
```

## <a name="next-steps"></a>Seuraavat vaiheet

- [Näkymän suunnittelu](log-analytics-view-designer.md) avulla voit luoda mukautettuja näkymiä tiedoista, jotka voit lähettää.
