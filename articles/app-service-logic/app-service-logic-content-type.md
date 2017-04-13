<properties
   pageTitle="Logiikan sovellusten sisällön Kirjoita käsittely | Microsoft Azure"
   description="Tietoja siitä, miten logiikan sovellukset käsitellään sisältötyypit-rakenne ja runtime"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="dwrede"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="10/18/2016"
   ms.author="jehollan"/>

# <a name="logic-apps-content-type-handling"></a>Logiikan sovellusten sisältötyyppi käsittely

On monta erilaista sisältöä, jotka voivat flow esimerkiksi JSON, XML, kiinteänä tiedostoja ja binaaritietoja logiikan - sovelluksen kautta.  Kun kaikki sisältötyypit tuetaan, joitakin grafiikkatiedostomuotoja tulkitsemaan logiikan sovellukset-ohjelma ja muiden voi vaatia liittyvät tai muuntamisen tarpeen mukaan.  Seuraavassa artikkelissa kerrotaan, miten moduulin käsittelee eri sisältötyypit ja miten niitä voi oikein ratkaista tarpeen mukaan.

## <a name="content-type-header"></a>Sisältötyyppi otsikko

Yksinkertainen käynnistämiseen katsotaan kahden `Content-Types` , joka ei edellytä muuntaminen tai liittyvät logiikan - sovellus käyttää `application/json` ja `text/plain`.

### <a name="applicationjson"></a>Sovelluksen/json

Työnkulun ohjelma on riippuvainen `Content-Type` HTTP ylätunniste soittaa tarvittavat käsittelytavan.  Pyyntö sisältölajille `application/json` tallennetaan ja käsitellään JSON-objekteina.  Lisäksi JSON sisältöä voi jäsentää oletusarvoisesti kaikki liittyvät tarvitsematta.  Niin pyynnön, joka on sisältötyyppi ylätunniste `application/json ` tältä:

```
{
    "data": "a",
    "foo": [
        "bar"
    ]
}
```

jäsentäminen työnkulussa, lauseketta `@body('myAction')['foo'][0]` arvon (Tässä tapauksessa `bar`).  Ei ole muita liittyvät ei tarvita.  Jos käsittelet tiedoilla, jotka on JSON, mutta ei ole määritetty otsikko, voit voit manuaalisesti liittää sen JSON avulla `@json()` toiminto (esimerkiksi: `@json(triggerBody())['foo']`).

### <a name="textplain"></a>Tekstin tai vain

Samalla tavalla kuin `application/json`, HTTP-viestien mukana vastaanotettujen `Content-Type` otsikon `text/plain` tallennetaan sen raaka lomakkeessa.  Lisäksi jos seuraavien toimintojen ilman mitään liittyvät pyynnön siirtyvät kanssa `Content-Type`: `text/plain` otsikko.  Esimerkiksi jos käyttäminen tietuetiedostoon näyttöön voi tulla seuraava HTTP-sisältö:

```
Date,Name,Address
Oct-1,Frank,123 Ave.
```

as `text/plain`.  Jos seuraavan toimen lähetit sen toiseen kokouspyynnön tekstiosassa (`@body('flatfile')`), pyyntö on `text/plain` -sisältötyypin otsikon.  Jos käsittelet tiedoilla, jotka on vain teksti-muotoon, mutta ei ole määritetty otsikko, voit voit manuaalisesti liittää sen tekstiä `@string()` toiminto (esimerkiksi: `@string(triggerBody())`)

### <a name="applicationxml-and-applicationoctet-stream-and-converter-functions"></a>Sovelluksen/xml ja sovelluksen/octet-stream ja muuntimen Funktiot

Logiikan App ohjelma säilyttää aina `Content-Type` , joka saatiin HTTP-pyynnön ja vastauksen.  Tämä tarkoittaa sitä, jos sisältö on vastaanotettu ja `Content-Type` , `application/octet-stream`, mukaan lukien myöhemmin käytössä ei ole liittyvät aiheuttaa lähtevien pyyntö, jonka `Content-Type`: `application/octet-stream`.  Tällä tavalla ohjelma voi guaruntee tiedot ei menetetään, kun se siirtyy työnkulun koko.  Toimintotila (syötteiden ja tulostaa) tallennetaan kuitenkin JSON-objekti, kun se jatkuu koko työnkulun.  Tämä tarkoittaa säilyttämiseksi joitakin tietotyyppejä, moduulin Muuntaa binaariluvun base64 koodatun merkkijonon, jossa tarvittavat metatiedot, joka säilyttää sekä sisällön `$content` ja `$content-type` -joka muunnetaan automaattisesti.  Voit myös manuaalisesti muuntaa sisältötyypit välillä käyttämällä valmista muuntimen funktioissa:

* `@json()`-muuttaa sarakemuotoa tiedot`application/json`
* `@xml()`-muuttaa sarakemuotoa tiedot`application/xml`
* `@binary()`-muuttaa sarakemuotoa tiedot`application/octet-stream`
* `@string()`-muuttaa sarakemuotoa tiedot`text/plain`
* `@base64()`-muuntaa sisällön base64 merkkijono
* `@base64toString()`-muuntaa base64 koodatun merkkijonon`text/plain`
* `@base64toBinary()`-muuntaa base64 koodatun merkkijonon`application/octet-stream`
* `@encodeDataUri()`-käyttää merkkijonon dataUri tavu matriisina.
* `@decodeDataUri()`-purkaa dataUri kyselyjä DBCS-matriisi

Jos olet saanut HTTP-pyyntö, jonka esimerkiksi `Content-Type`: `application/xml` on:

```
<?xml version="1.0" encoding="UTF-8" ?>
<CustomerName>Frank</CustomerName>
```

Voin voitu joka ja käyttää myöhemmin suunnilleen `@xml(triggerBody())`, tai toiminnon, kuten `@xpath(xml(triggerBody()), '/CustomerName')`.

### <a name="other-content-types"></a>Muut sisältötyypit

Muita sisältötyyppejä tuetaan ja toimii logiikan-sovelluksen, mutta saattaa edellyttää haetaan manuaalisesti purkamalla viestin tekstiosaan `$content`.  Esimerkiksi, jos minulla on käynnistävä ulos `application/x-www-url-formencoded` pyynnön, joka näytti seuraavasti:

```
CustomerName=Frank&Address=123+Avenue
```

Koska tämä ei ole vain teksti- tai tallennetaan-toiminnon JSON:

```
...
"body": {
    "$content-type": "application/x-www-url-formencoded",
    "$content": "AAB1241BACDFA=="
}
```

Jos `$content` on koodattu base64 merkkijono, jos haluat säilyttää kaikki tiedot tiedot.  Koska ei tällä hetkellä ole alkuperäisen käänteisfunktio lomakkeen tiedot, voi edelleen käyttää näitä tietoja työnkulussa toiminnon, kuten tiedot manuaalisesti muuttamalla `@string(body('formdataAction'))`.  Jos minulla on myös omat lähtevän pyynnön `application/x-www-url-formencoded` -sisältötyypin otsikon voi vain lisätä se toiminnon teksti ilman mitään liittyvät, kuten `@body('formdataAction')`.  Kuitenkin Tämä toimii vain, jos teksti on vain parametrin `body` näytettävä.  Jos yrität tarkastella sitä `@body('formdataAction')` sisällä, `application/json` pyytää, näkyviin tulee virheilmoitus, kun se lähetetään koodattu teksti.
