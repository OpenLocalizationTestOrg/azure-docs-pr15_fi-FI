<properties
   pageTitle="Logiikan sovellusten poikkeuksen käsittely | Microsoft Azure"
   description="Lisätietoja kuviot virhe ja poikkeuksen Azure logiikan sovellusten käsitteleminen"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="10/18/2016"
   ms.author="jehollan"/>

# <a name="logic-apps-error-and-exception-handling"></a>Logiikan sovellusten virhe ja poikkeuksen käsittely

Logiikan sovellukset on joukko monipuolisia työkaluja ja kuvioiden avulla ja varmistaa, että integroinnit ovat tehokkaat ja joustavat vastaan virheet.  Yksi haasteita ja minkä tahansa integroinnin arkkitehtuuri on varmistaa, että käyttökatkot tai riippuvaiset järjestelmien ongelmia käsitellään oikein.  Logiikan sovellukset on käsittely virheitä ensimmäisen luokan kokemus antamalla Työkalut, sinun täytyy suorittaa poikkeukset ja virheiden työnkulkuja.

## <a name="retry-policies"></a>Yritä käytännöt

Poikkeus ja Virheenkäsittely yleisin tyyppi on uudelleen-käytäntö.  Käytäntö määritetään toiminto olisi uudelleen, jos ensimmäisen pyynnön aikakatkaisu tai epäonnistui (pyyntö, jonka tuloksena 429 tai vastaus 5xx).  Oletusarvon mukaan kaikki toiminnot uudelleen yli 20 sekunnin välein 4 useita kertoja.  Jos ensimmäinen pyyntö on vastaanotettu `500 Internal Server Error` vastauksen, työnkulun ohjelma keskeyttää 20 sekuntia ja yritys kutsu uudelleen.  Jos kaikki yrityksistä vastaus on edelleen poikkeuksen tai virhe, työnkulun jatkaa ja Merkitse toiminnon tilan `Failed`.

Voit määrittää tietyn toiminnon **syötteiden** uudelleen käytännöt.  Kokeile jopa 4 kertaa yli tunnin välein voi määrittää uudelleen-käytäntö.  Tarkat tiedot syötteen ominaisuudet voivat olla [löytyy MSDN][retryPolicyMSDN].

```json
"retryPolicy" : {
      "type": "<type-of-retry-policy>",
      "interval": <retry-interval>,
      "count": <number-of-retry-attempts>
    }
```

Jos haluat määrittää HTTP-toiminto Odota välinen 10 minuuttia ja yritä 4 kertaa olisi seuraavat:

```json
"HTTP": 
{
    "inputs": {
        "method": "GET",
        "uri": "http://myAPIendpoint/api/action",
        "retryPolicy" : {
            "type": "fixed",
            "interval": "PT10M",
            "count": 4
        }
    },
    "runAfter": {},
    "type": "Http"
}
```

Lisätietoja tuetuista syntaksi on tarkastella [MSDN-osassa uudelleen käytännön][retryPolicyMSDN].

## <a name="runafter-property-to-catch-failures"></a>RunAfter-ominaisuuden arvoksi todellisen virheet

Logiikan kunkin sovelluksen toiminto ilmoittaa, mitkä toiminnot on tehtävä ennen toiminto käynnistyy.  Nämä ajatella voit työnkulun vaiheet järjestystä.  Tämä järjestys on nimeltään `runAfter` toiminnon määritystä-ominaisuutta.  Objekti, joka kuvaa, mitkä toiminnot ja toiminto tiloja suorittaa toiminnon on.  Oletusarvon mukaan kaikki toiminnot, jotka on lisätty kautta suunnittelija määritetään `runAfter` edellisessä vaiheessa Jos edellisessä vaiheessa oli `Succeeded`.  Voit kuitenkin mukauttaa arvoksi Kyselysäännön toiminnot, kun edellisen toiminnot ovat `Failed`, `Skipped`, tai mahdollinen joukko nämä arvot.  Jos haluat lisätä kohteen nimettyjen palvelun Bus aiheen tietyn toiminnon jälkeen `Insert_Row` epäonnistuu, voit käyttää seuraavaa `runAfter` määritykset:

```json
"Send_message": {
    "inputs": {
        "body": {
            "ContentData": "@{encodeBase64(body('Insert_Row'))}",
            "ContentType": "{ \"content-type\" : \"application/json\" }"
        },
        "host": {
            "api": {
                "runtimeUrl": "https://logic-apis-westus.azure-apim.net/apim/servicebus"
            },
            "connection": {
                "name": "@parameters('$connections')['servicebus']['connectionId']"
            }
        },
        "method": "post",
        "path": "/@{encodeURIComponent('failures')}/messages"
    },
    "runAfter": {
        "Insert_Row": [
            "Failed"
        ]
    }
}
```

Ilmoitus `runAfter` -ominaisuudeksi on määritetty Kyselysäännön, jos `Insert_Row` toiminto on `Failed`.  Voit suorittaa toiminnon, jos toimintoa tila on `Succeeded`, `Failed`, tai `Skipped` syntaksi on:

```json
"runAfter": {
        "Insert_Row": [
            "Failed", "Succeeded", "Skipped"
        ]
    }
```

>[AZURE.TIP] Toiminnot, jotka suorittaa, kun edellisen toimenpiteen on epäonnistui ja suoritettu merkitty `Succeeded`.  Tämä ongelma tarkoittaa sitä, jos olet onnistuneesti todellisen kaikki virheiden työnkulun itse suorita on merkitty `Succeeded`.

## <a name="scopes-and-results-to-evaluate-actions"></a>Käyttöalueen ja tulokset arvioidaan toiminnot

Samalla tavalla kuin siitä, miten voit suorittaa jälkeen toiminnoista, voit myös ryhmän toiminnot yhdessä [laajuus](app-service-logic-loops-and-scopes.md) - sisällä joka looginen ryhmittely toimintojen edustajana.  Käyttöalueen on hyötyä sekä järjestämisessä logiikan sovelluksen toiminnot ja käyttöalue tilan kooste arvioinnit tekemistä varten.  Laajuuden itse saavat tila, kun alueen kaikki toiminnot on valmis.  Laajuus-tila määritetään samaan ehtoja kuin Suorita--suorittamisen haaran lopullinen-toiminnon ollessa `Failed` tai `Aborted` tila on `Failed`.

Voit tehdä `runAfter` käyttöalue on merkitty `Failed` Kyselysäännön alueessa mahdollisia virheitä toimenpiteitä.  Kun käyttöalue epäonnistuu avulla voit luoda yhden toiminnon havaitsemaan epäonnistuu, jos *mitään* toimia alueessa.

### <a name="getting-the-context-of-failures-with-results"></a>Käytön yhteydessä virheet ja tulos

Virheet-alueen pyytämisestä on erittäin hyödyllinen, mutta haluat myös ehkä yhteydessä ymmärtää täsmälleen epäonnistui, mitkä toiminnot ja virheitä tai tilakoodin, jotka on palautettu.  `@result()` Työnkulun-funktion avulla yhteydessä kaikki toiminnot alueen sisällä tulokseen.

`@result()`yksittäinen parametri, alueen nimi tulee, ja palauttaa kaikki toiminnon tuloksia tarvittavalla alueella.  Toiminnon objektit ovat samat määritteet kuin `@actions()` objektin, kuten toiminnon alkamisaika, toiminnon päättymisaika, toiminnon tila, toiminto syötteiden, toiminto korrelaatio tunnukset ja toiminto tulostaa.  Voit helposti pariliitos `@result()` toimivan `runAfter` lähettäminen kontekstia toimenpiteistä, jotka epäonnistui alueen sisällä.

Jos haluat suorittaa toiminnon toiminto *kunkin* alueen, `Failed`, voit pariliitos `@result()` **[Suodattimen matriisi](../connectors/connectors-native-query.md)** -toiminto ja **[ForEach](app-service-logic-loops-and-scopes.md)** silmukan.  Voit suodattaa tulokset toiminnot, jotka epäonnistui matriisi.  Voit siirtää suodatettu tulos-matriisi ja suorittaa toiminnon kunkin käyttämällä **ForEach** tapahtuu virhe.  Esimerkki alla tarkan perään.  Tässä esimerkissä lähetetään HTTP POST-pyyntö, jonka toiminnot, jotka epäonnistui vastauksen leipätekstiin alueessa `My_Scope`.

```json
"Filter_array": {
    "inputs": {
        "from": "@result('My_Scope')",
        "where": "@equals(item()['status'], 'Failed')"
    },
    "runAfter": {
        "My_Scope": [
            "Failed"
        ]
    },
    "type": "Query"
},
"For_each": {
    "actions": {
        "Log_Exception": {
            "inputs": {
                "body": "@item()['outputs']['body']",
                "method": "POST",
                "headers": {
                    "x-failed-action-name": "@item()['name']",
                    "x-failed-tracking-id": "@item()['clientTrackingId']"
                },
                "uri": "http://requestb.in/"
            },
            "runAfter": {},
            "type": "Http"
        }
    },
    "foreach": "@body('Filter_array')",
    "runAfter": {
        "Filter_array": [
            "Succeeded"
        ]
    },
    "type": "Foreach"
}
```

Näin version tapahtumat:

1. **Suodatin-taulukko** -toiminto, joka suodattaa `@result('My_Scope')` saat tuloksen kaikkia toimintoja.`My_Scope`
1. Ehto **Suodattimen matriisi** on mikä tahansa `@result()` kohde, jonka tila on yhtä suuri kuin `Failed`.  Tämä suodattaa kaikki toiminnon tulokset matriisin `My_Scope` vain epäonnistui toiminnon tulosten matriisiin.
1. Suorita **Suodatettu matriisin** tulostaa **kullekin** -toiminto.  Tämä suorittaa-toiminto *kunkin* epäonnistui toiminnon tulos on suodatettu yläpuolella.
    - Jos oli yksi toiminto alueessa, joka epäonnistui, toiminnot `foreach` suoritetaan vain kerran.  Monta epäonnistuneiden toimintojen aiheuttaa virheen kohden yksi toiminto.
1. Lähettää HTTP POST `foreach` kohteen vastauksen leipätekstiin tai `@item()['outputs']['body']`.  `@result()` Kohteen muoto on sama kuin `@actions()` muotoileminen ja voi jäsentää samalla tavalla.
1. Myös kaksi mukautetut otsikot epäonnistui toiminto-niminen `@item()['name']` ja epäonnistunutta Suorita seuranta tunnus asiakkaan `@item()['clientTrackingId']`.

Käyttöä, seuraavassa on esimerkki yhden `@result()` kohteen.  Näet `name`, `body`, ja `clientTrackingId` ominaisuudet jäsentää yllä olevassa esimerkissä.  Huomattava ulkopuolella, joka `foreach`, `@result()` palauttaa nämä objektit.

```json
{
    "name": "Example_Action_That_Failed",
    "inputs": {
        "uri": "https://myfailedaction.azurewebsites.net",
        "method": "POST"
    },
    "outputs": {
        "statusCode": 404,
        "headers": {
            "Date": "Thu, 11 Aug 2016 03:18:18 GMT",
            "Server": "Microsoft-IIS/8.0",
            "X-Powered-By": "ASP.NET",
            "Content-Length": "68",
            "Content-Type": "application/json"
        },
        "body": {
            "code": "ResourceNotFound",
            "message": "/docs/foo/bar does not exist"
        }
    },
    "startTime": "2016-08-11T03:18:19.7755341Z",
    "endTime": "2016-08-11T03:18:20.2598835Z",
    "trackingId": "bdd82e28-ba2c-4160-a700-e3a8f1a38e22",
    "clientTrackingId": "08587307213861835591296330354",
    "code": "NotFound",
    "status": "Failed"
}
```

Yllä lausekkeiden avulla voit suorittaa eri poikkeuksen käsittely kuviot.  Voit halutessasi suorittaa toiminnon ulkopuolella, joka hyväksyy virheet ja poista koko suodatetut matriisin käsittely yhden poikkeuksen `foreach`.  Voit lisätä myös muita hyödyllisiä ominaisuuksia `@result()` yllä vastaus.

## <a name="azure-diagnostics-and-telemetry"></a>Azure diagnostiikka- ja telemetriatietojen

Yllä kuvioita on erinomainen tapa käsittelemään virheet ja poikkeukset sisällä, mutta voit tunnistaa ja vastata virheitä riippumaton Suorita itse.  [Azure diagnostiikka](app-service-logic-monitor-your-logic-apps.md) on helppo tapa lähettää kaikki Työnkulkutapahtumat (mukaan lukien kaikki Suorita ja toiminto tilat) Azure-tallennustilan tilin tai Azure-tapahtumaa-toiminnossa.  Voit seurata lokit ja arvot tai julkaista niitä mitään seuranta työkaluun haluat arvioida Suorita tilat.  Yksi mahdollinen vaihtoehto on lähettää kyselyjä [Stream Analytics](https://azure.microsoft.com/services/stream-analytics/)Azure tapahtumaa-toiminnossa läpi kaikki tapahtumat.  Virta Analytics voit kirjoittaa live kyselyjen luettelosta poikkeamia, keskiarvoja tai virheiden vianmäärityslokeihin.  Virta Analytics voit helposti näyttää muista lähteistä, kuten olevien, aiheet, SQL, DocumentDB ja Power BI.

## <a name="next-steps"></a>Seuraavat vaiheet
- [Katso miten yhtä Käyttömukavuuden laadittuihin tehokkaat Virheenkäsittely logiikan sovelluksilla](app-service-logic-scenario-error-and-exception-handling.md)
- [Uusien logiikan sovellusten esimerkkejä ja skenaariot](app-service-logic-examples-and-scenarios.md)
- [Opettele luomaan automaattinen logiikan sovellukset-versioiden](app-service-logic-create-deploy-template.md)
- [Suunnittelu ja käyttöönotto Visual Studio sovelluksia logiikka](app-service-logic-deploy-from-vs.md)


<!-- References -->
[retryPolicyMSDN]: https://msdn.microsoft.com/library/azure/mt643939.aspx#Anchor_9