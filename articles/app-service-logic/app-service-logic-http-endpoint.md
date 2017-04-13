<properties
   pageTitle="Logiikan sovellusten kutsuttava päätepisteet nimellä"
   description="Voit luoda ja määrittää käynnistimen päätepisteet ja niitä käytetään logiikan-sovellus App Azure-palvelussa"
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


# <a name="logic-apps-as-callable-endpoints"></a>Logiikan sovellusten kutsuttava päätepisteet nimellä

Logiikan sovellusten grafiikkatiedostomuotoja voit näyttää painikkeen HTTP-päätepisteen käynnistämisen.  Voit käyttää myös kutsuttava päätepisteet kuvion käynnistää logiikan sovellusten sisäkkäisiä työnkulkuna logiikan-sovelluksessa ""-työnkulkutoiminnon kautta.

Käynnistimien, joka voi vastaanottaa pyyntöjä 3 perustyyppiä:

* Pyyntö
* ApiConnectionWebhook
* HttpWebhook

Artikkelin loppuosa Käytämme **pyyntö** kuin esimerkissä, mutta kaikki periaatteiden koskevat samannimisen muiden käynnistimien 2 tyypit.

## <a name="adding-a-trigger-to-your-definition"></a>Käynnistimen lisääminen määrityksen
Ensimmäiseksi on käynnistimen lisääminen logiikan sovelluksen määrityksen, joka voi vastaanottaa pyynnöt.  Voit hakea, "HTTP pyytää" Voit lisätä yhteystietokortin käynnistimen suunnittelussa. Voit määrittää pyynnön leipätekstin JSON rakenteen ja suunnittelija Luo tunnusten jäsentää ja välittää tietoja – työnkulun manuaalinen käynnistäminen.  Voin suosittelee Luo JSON rakenteen otoksen leipäteksti-paketti-työkalua, kuten [jsonschema.net](http://jsonschema.net) avulla.

![Pyydä käynnistimen kortti][2]

Kun olet tallentanut logiikan sovelluksen määrityksen, takaisinsoitto URL-osoite luodaan näytettyjä:
 
``` text
https://prod-03.eastus.logic.azure.com:443/workflows/080cb66c52ea4e9cabe0abf4e197deff/triggers/myendpointtrigger?*signature*...
```

Tätä URL-osoite sisältää SAS todentamiseen käytetyn kyselyparametrit-näppäintä.

Voit valita myös tämän päätepisteen Azure-portaalissa:

![][1]

Tai puhelimitse:

``` text
POST https://management.azure.com/{resourceID of your logic app}/triggers/myendpointtrigger/listCallbackURL?api-version=2015-08-01-preview
```

### <a name="security-for-the-trigger-url"></a>Suojauksen käynnistimen URL-osoite

Logiikan App takaisinsoitto URL-osoitteet luodaan käyttämällä suojatusti jaettu Access-allekirjoitus.  Allekirjoituksen välitetään parametrina ja on tarkistettava, ennen kuin logiikan-sovellus Kyselysäännön.  Luo yksilöllinen yhdistelmän salausavaimen kohti logiikan sovelluksen käynnistimen nimi ja suoritetaan toiminnon avulla.  Jos käyttäjällä on salainen logiikan app avaimen käyttöoikeus, ne voi kelvollinen allekirjoituksen luomiseen.

## <a name="calling-the-logic-app-triggers-endpoint"></a>Logiikan sovelluksen käynnistimen päätepisteen kutsumista

Kun olet luonut päätepisteen käynnistin, voit käynnistää sen kautta `POST` koko URL-osoitteeseen. Voit lisätä muita ylä- ja minkä tahansa sisällön tekstissä.

Jos sisältötyyppiä ei `application/json` jälkeen osaat viittaus ominaisuuksien sisällä pyynnön. Muussa tapauksessa sitä käsitellään yhtenä binaarinen yksikkönä, joka voi välittää muiden API, mutta ei voi viitata työnkulun sisällä ilman muuntamalla sisällön.  Jos esimerkiksi `application/xml` sisällön voit hyödyntää `@xpath()` tekemään xpath-poiminnan, tai `@json()` JSON muuntaminen XML: stä.  Saat lisätietoja sisällön käsitteleminen kirjoituskentän [voi olla täällä](app-service-logic-content-type.md)

Lisäksi voit määrittää JSON rakenteen määritys. Tämä aiheuttaa Luo tunnuksia, jotka kulkevat vaiheiden suunnittelu.  Esimerkiksi seuraavat tekee `title` ja `name` tunnuksen suunnittelussa käytettävissä:

```
{
    "properties":{
        "title": {
            "type": "string"
        },
        "name": {
            "type": "string"
        }
    },
    "required": [
        "title",
        "name"
    ],
    "type": "object"
}
```

## <a name="referencing-the-content-of-the-incoming-request"></a>Viittaaminen saapuvan pyynnön sisältö

`@triggerOutputs()` Funktion siirtää saapuvan pyynnön sisällön. Esimerkiksi se näyttäisi samanlaiselta kuin:

```
{
    "headers" : {
        "content-type" : "application/json"
    },
    "body" : {
        "myprop" : "a value"
    }
}
```

Voit käyttää `@triggerBody()` käyttämään pikakuvake `body` ominaisuuden erikseen. 

## <a name="responding-to-the-request"></a>Pyynnön vastaaminen

Jotkin pyynnöt, jotka alkavat logiikan-sovelluksen voit vastata soittajan kanssa sisältöä. On nimeltään **vastaus** , jonka avulla voidaan muodostaa tilakoodin, leipätekstin ja otsikot ovat vastauksesi uuden toiminnon tyypin. Huomaa, että jos **vastauksen** mitään muotoa ei sisällä tietoja, logiikan app päätepisteen tulee *välittömästi* vastaa **202 hyväksytty**.

![HTTP-vastaus-toiminto][3]

``` json
"Response": {
            "conditions": [],
            "inputs": {
                "body": {
                    "name": "@{triggerBody()['name']}",
                    "title": "@{triggerBody()['title']}"
                },
                "headers": {
                    "content-type": "application/json"
                },
                "statusCode": 200
            },
            "type": "Response"
        }
```

Vastaukset ovat seuraavat:

| Ominaisuus | Kuvaus |
| -------- | ----------- |
| statusCode | HTTP-tilakoodin vastata saapuvan pyynnön. Voi olla mikä tahansa kelvollinen tilakoodi, joka alkaa 2xx, 4xx tai 5xx. 3xx tilakoodin ei sallita. | 
| tekstissä | Leipäteksti-objekti, joka voi olla merkkijono, JSON-objektin tai jopa binaarinen sisällön viitata edellisessä vaiheessa. | 
| otsikot | Voit määrittää jokin muu luku otsikon sisällytetään vastaus | 

Kaikki logiikan sovelluksen vastauksen varten tarvittavat vaiheet on suoritettava alkuperäisen pyynnön vastaanottamaan vastauksen **paitsi työnkulun kutsutaan nimellä sisäkkäisten funktioiden-sovelluksen** *60 sekunnin ajan* kuluessa. Jos vastaus tarvittavat toimenpiteet on saavutettu sisällä 60 sekunnin jälkeen saapuvan pyynnön aikakatkaistaan ja vastaanottaa **408 asiakkaan aikakatkaisua** HTTP-vastauksen.  Sisäkkäiset logiikan sovellusten ylemmän tason logiikan App säilyvät vastausta loppuun, riippumatta siitä, kuinka paljon kuluu.

## <a name="advanced-endpoint-configuration"></a>Määritysten Lisäasetukset päätepiste

Logiikan sovellukset on luotu tutustuminen päätepisteen tuki ja Käytä aina `POST` tapa käynnistää logiikan sovelluksen Suorita. **HTTP-kuuntelutoiminnon** API-sovelluksen aiemmin tukee myös muuttaminen URL-Osoitteen osat ja HTTP-menetelmää. Parillinen määrittäminen lisäsuojauksen tai mukautetun toimialueen saattaa lisäämällä se API app host (verkkosovelluksen tietokenttiä isännöidään API-sovellus). 

Tämä toiminto on käytettävissä **API hallinnan**avulla:
* [Pyynnön laskentatavan muuttaminen](https://msdn.microsoft.com/library/azure/dn894085.aspx#SetRequestMethod)
* [Muuta pyynnön URL-osia](https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#RewriteURL)
* API hallinta-toimialueiden **määrittäminen** perinteinen Azure portal-välilehden määrittäminen
* Tarkista perustodentamista (**linkki tarvitaan**) käytännön määrittäminen

## <a name="summary-of-migration-from-2014-12-01-preview"></a>Siirron 2014 – 12-01 – esikatselusta yhteenveto

|  2014 – 12-01-esikatselu | 2016-06-01 |
|---------------------|--------------------|
| Valitse **HTTP-kuuntelutoiminnon** API-sovellukseen | Valitse **manuaalinen käynnistäminen** (API sovellus ei ole pakollinen) |
| HTTP-kuuntelutoiminnon "*lähettää vastauksen automaattisesti*-asetus | Joko sisältävät **vastaus** -toiminnon tai työnkulun määritys |
| Määritä basic tai OAuth-todennus | kautta API hallinta |
| Määritä HTTP-menetelmä | kautta API hallinta |
| Määritä suhteellinen polku | kautta API hallinta |
| Viittaus saapuvien tekstiin kautta`@triggerOutputs().body.Content` | Viittaus kautta`@triggerOutputs().body` |
| HTTP-kuuntelutoiminnon toiminnon **lähettää HTTP-vastaus** | Valitse **kokouspyyntöön HTTP** (API sovellus ei ole pakollinen)


[1]: ./media/app-service-logic-http-endpoint/manualtriggerurl.png
[2]: ./media/app-service-logic-http-endpoint/manualtrigger.png
[3]: ./media/app-service-logic-http-endpoint/response.png
