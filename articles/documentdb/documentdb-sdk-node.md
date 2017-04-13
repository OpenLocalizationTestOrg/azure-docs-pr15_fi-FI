<properties
    pageTitle="DocumentDB Node.js API & SDK | Microsoft Azure"
    description="Lisätietoja Node.js API ja SDK esimerkiksi release päivämäärät ja käytöstä poistaminen päivämäärien välillä kunkin DocumentDB Node.js SDK-versiossa tehdyt muutokset."
    services="documentdb"
    documentationCenter="nodejs"
    authors="rnagpal"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="documentdb"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="rnagpal"/>

# <a name="documentdb-apis-and-sdks"></a>DocumentDB API ja SDK: T

> [AZURE.SELECTOR]
- [.NET](documentdb-sdk-dotnet.md)
- [Node.js](documentdb-sdk-node.md)
- [Java](documentdb-sdk-java.md)
- [Python](documentdb-sdk-python.md)
- [MUILLE KÄYTTÄJILLE](https://go.microsoft.com/fwlink/?LinkId=402413)
- [SQL](https://msdn.microsoft.com/library/azure/dn782250.aspx)

##<a name="documentdb-nodejs-api-and-sdk"></a>DocumentDB Node.js API ja SDK

<table>
<tr><td>**Lataa SDK**</td><td>[NPM](https://www.npmjs.com/package/documentdb)</td></tr>
<tr><td>**API dokumentaatio**</td><td>[Node.js API-oppaat](http://azure.github.io/azure-documentdb-node/DocumentClient.html)</td></tr>
<tr><td>**SDK asennusohjeet**</td><td>[Asennusohjeet](http://azure.github.io/azure-documentdb-node/)</td></tr>
<tr><td>**Osallistuja SDK-paketissa**</td><td>[GitHub](https://github.com/Azure/azure-documentdb-node/tree/master/source)</td></tr>
<tr><td>**Esimerkkejä, joiden**</td><td>[Node.js MALLIKOODEJA](documentdb-nodejs-samples.md)</td></tr>
<tr><td>**Hae aloittaminen-opetusohjelma**</td><td>[Node.js SDK: N käytön aloittaminen](documentdb-nodejs-get-started.md)</td></tr>
<tr><td>**Web app-opas**</td><td>[Node.js DocumentDB käyttävän verkkosovelluksen luominen](documentdb-nodejs-application.md)</td></tr>
<tr><td>**Nykyinen tuetut käyttöympäristö**</td><td>[Node.js v0.10](https://nodejs.org/en/blog/release/v0.10.0/)<br/>[Node.js v0.12](https://nodejs.org/en/blog/release/v0.12.0/)<br/>[Node.js v4.2.0](https://nodejs.org/en/blog/release/v4.2.0/)</td></tr>
</table></br>

##<a name="release-notes"></a>Julkaisutiedot

###<a name="1.10.0"/>1.10.0</a>

- Tukee nyt cross osion rinnakkain kyselyt.
- Tukee nyt ylös/ORDER BY-kyselyiden osioitua loppu.

###<a name="1.9.0"/>1.9.0</a>

- Lisätty uudelleen käytännön tuki kyselyn pyynnöt. (Kyselyn pyynnöt saavat pyynnön korko liian suuri poikkeuksen, virhekoodi 429.) Oletusarvon mukaan DocumentDB yrittää suorittaa uudelleen yhdeksän kertaa sivupyynnön virhekoodi 429 havaitaan, kun honoring vastauksen otsikon retryAfter-aika. Kiinteä uudelleen välin aika nyt voidaan määrittää RetryOptions-ominaisuuden osana ConnectionPolicy objektin Jos haluat ohittaa palauttama palvelimen välillä uudelleenyritykset retryAfter aika. DocumentDB nyt odottaa enintään 30 sekuntia kunkin pyytää pakotetaan (riippumatta Laske uudelleen) ja palauttaa virhekoodi 429 vastauksen. Tällä hetkellä voi olla myös ohittaa ConnectionPolicy objektin RetryOptions-ominaisuudessa.

- DocumentDB palauttaa nyt x-ms-rajoituksen-yritä-Laske- ja x-ms-throttle-retry-wait-time-ms kuin jokaisen pyynnön rajoituksen osoittamaan vastauksen otsikkoa uudelleen Laske- ja pyyntö odotti välillä uudelleenyritykset cummulative aika.

- RetryOptions-luokka on lisätty paljastaa ConnectionPolicy-luokka, joka voidaan ohittaa joitakin uudelleen oletusasetusten RetryOptions-ominaisuus.

###<a name="1.8.0"/>1.8.0</a>

 - Lisätä monille tietokannan tilit tuki.

###<a name="1.7.0"/>1.7.0</a>

- Lisätä aika-Live(TTL) ominaisuuden asiakirjojen tuki.

###<a name="1.6.0"/>1.6.0</a>

- Toteutuksen [osioitua sivustokokoelmat](documentdb-partition-data.md) ja [käyttäjän määrittämät suorituskykyä](documentdb-performance-levels.md).

###<a name="1.5.6"/>1.5.6</a>

- Kiinteä RangePartitionResolver.resolveForRead ohjelmavirhe kohtaa, johon se on tuota linkit virheelliset ketjutettu tulosten vuoksi.

###<a name="1.5.5"/>1.5.5</a>

- Kiinteä hashParitionResolver resolveForRead(): kun ei ole toimitettu osion tunnus on yliheittävä sen sijaan, että kaikki rekisteröity linkit luettelo, joka palauttaa poikkeuksen vuoksi.

###<a name="1.5.4"/>1.5.4</a>

- Korjaa ongelma [#100](https://github.com/Azure/azure-documentdb-node/issues/100) - omistautunut HTTPS-agentti: välttää muokkaaminen DocumentDB tarkoituksiin yleinen agentti. Käyttää erillinen agentti kaikkien lib pyynnöt.

###<a name="1.5.3"/>1.5.3</a>

- Korjaa ongelma [#81](https://github.com/Azure/azure-documentdb-node/issues/81) - oikein kahvaa katkoviivat-media tunnukset.

###<a name="1.5.2"/>1.5.2</a>

- Korjaa ongelma [#95](https://github.com/Azure/azure-documentdb-node/issues/95) - listener paljastaa varoitus EventEmitter.

###<a name="1.5.1"/>1.5.1</a>

- Korjaa annettava [#92](https://github.com/Azure/azure-documentdb-node/issues/90) - nimeä kansio Hash hash kirjainkoko on merkitsevä Systemsin.

### <a name="1.5.0"/>1.5.0</a>

- Ota käyttöön sharding tuki lisäämällä hash & alue-osion resolvers.

### <a name="1.4.0"/>1.4.0</a>

- Ota käyttöön Upsert. Uusi upsertXXX menetelmiä documentClient.

### <a name="1.3.0"/>1.3.0</a>

- Voit tuoda versioita tasaus ja muut SDK: T ohitetaan.

### <a name="1.2.2"/>1.2.2</a>

- Jaa Q lupaa paketti uuden säilöön.
- Päivitä npm rekisterin pakettitiedosto.

### <a name="1.2.1"/>1.2.1</a>

- Ottaa käyttöön tunnuksen perusteella reititys.
- Korjaa ongelma [#49](https://github.com/Azure/azure-documentdb-node/issues/49) - menetelmä current() nykyisen ominaisuuden ristiriidassa.

### <a name="1.2.0"/>1.2.0</a>

- Tukee nyt Geospatiaalisia indeksi.
- Tarkistaa kaikki resurssit id-ominaisuutta. Resurssien tunnukset ei voi olla?, /, #, & #47; & #47;-merkkiä tai end välilyönnillä.
- Lisää uusi otsikko "indeksi muunnos edistyminen" ResourceResponse.

### <a name="1.1.0"/>1.1.0</a>

- Ottaa käyttöön V2 indeksoinnin käytännön.

### <a name="1.0.3"/>1.0.3</a>

- Ongelman #40 (https://github.com/Azure/azure-documentdb-node/issues/40) - toteutettu eslint ja grunt määrityksiä core ja promise SDK.

### <a name="1.0.2"/>1.0.2</a>

- Ongelman [#45](https://github.com/Azure/azure-documentdb-node/issues/45) – Promises paketti ei sisällä otsikko, jossa on virhe.

### <a name="1.0.1"/>1.0.1</a>

- Kyselyn ristiriitoja lisäämällä siihen readConflicts, readConflictAsync ja queryConflicts toteutettu mahdollisuus.
- Päivitetty API asiakirjat.
- Lähetä [#41](https://github.com/Azure/azure-documentdb-node/issues/41) - client.createDocumentAsync virhe.

### <a name="1.0.0"/>1.0.0</a>

- GA SDK.

## <a name="release--retirement-dates"></a>Vapauta ja käytöstä poistaminen päivämäärät
Microsoft tarjoaa ilmoitus on vähintään **12 kuukautta** ennen poistamisesta SDK jotta tasaiset uudempaan/tuettu versio siirtymä.

Uusia ominaisuuksia ja toimintoja ja optimointi lisätään vain nykyiseen SDK-paketissa, sellaisenaan on suositeltavaa, että aina päivität SDK uusin aikaisin kuin mahdollista.

Minkä tahansa DocumentDB käyttämällä käytöstä poistettuja SDK pyyntö hylätään palvelu.

> [AZURE.WARNING]
Kaikkien versioiden Node.js Azure DocumentDB SDK-paketissa ennen versio **1.0.0** poistettu käytöstä, **29 helmikuussa 2016**.

<br/>

| Versio | Julkaisupäivä | Päivämäärän käytöstä poistaminen
| ---     | ---          | ---
| [1.10.0](#1.10.0) | 03 lokakuussa 2016 |---
| [1.9.0](#1.9.0) | 07 heinäkuussa 2016 |---
| [1.8.0](#1.8.0) | 2016 14 kesäkuussa |---
| [1.7.0](#1.7.0) | 2016: n huhtikuun 26 |---
| [1.6.0](#1.6.0) | 29 maaliskuussa 2016 |---
| [1.5.6](#1.5.6) | 08 maaliskuussa 2016 |---
| [1.5.5](#1.5.5) | 02 helmikuussa 2016 |---
| [1.5.4](#1.5.4) | 01 helmikuussa 2016 |---
| [1.5.2](#1.5.2) | 26 tammikuussa 2016 |---
| [1.5.2](#1.5.2) | 2016 22 tammikuussa |---
| [1.5.1](#1.5.1) | 4 tammikuussa 2016 |---
| [1.5.0](#1.5.0) | 31. joulukuuta 2015 |---
| [1.4.0](#1.4.0) | 06 lokakuussa 2015 |---
| [1.3.0](#1.3.0) | 06 lokakuussa 2015 |---
| [1.2.2](#1.2.2) | 10 syyskuussa 2015 |---
| [1.2.1](#1.2.1) | 15 elokuussa 2015 |---
| [1.2.0](#1.2.0) | 05 elokuussa 2015 |---
| [1.1.0](#1.1.0) | 09 heinäkuussa 2015 |---
| [1.0.3](#1.0.3) | 2015 04 kesäkuussa |---
| [1.0.2](#1.0.2) | 23. toukokuu 2015 |---
| [1.0.1](#1.0.1) | 15. toukokuu 2015 |---
| [1.0.0](#1.0.0) | 08. huhtikuussa 2015 |---
| 0.9.4-Prerelease | 06. huhtikuussa 2015 | 29 helmikuussa 2016
| 0.9.3-Prerelease | 14. tammikuussa 2015 | 29 helmikuussa 2016
| 0.9.2-Prerelease | 18. joulukuussa 2014 | 29 helmikuussa 2016
| 0.9.1-Prerelease | 22. elokuussa 2014 | 29 helmikuussa 2016
| 0.9.0-Prerelease | 21. elokuussa 2014 | 29 helmikuussa 2016


## <a name="faq"></a>USEIN KYSYTYT KYSYMYKSET
[AZURE.INCLUDE [documentdb-sdk-faq](../../includes/documentdb-sdk-faq.md)]

## <a name="see-also"></a>Katso myös

Lisätietoja DocumentDB on artikkelissa [Microsoft Azure DocumentDB](https://azure.microsoft.com/services/documentdb/) palvelu‑sivulla.
