<properties 
    pageTitle="DocumentDB Python API & SDK | Microsoft Azure" 
    description="Lisätietoja Python API ja SDK esimerkiksi release päivämäärät ja käytöstä poistaminen päivämäärien välillä kunkin DocumentDB Python SDK-versiossa tehdyt muutokset." 
    services="documentdb" 
    documentationCenter="python" 
    authors="rnagpal" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="python" 
    ms.topic="article" 
    ms.date="09/29/2016" 
    ms.author="rnagpal"/>

# <a name="documentdb-apis-and-sdks"></a>DocumentDB API ja SDK: T

> [AZURE.SELECTOR]
- [.NET](documentdb-sdk-dotnet.md)
- [Node.js](documentdb-sdk-node.md)
- [Java](documentdb-sdk-java.md)
- [Python](documentdb-sdk-python.md)
- [MUILLE KÄYTTÄJILLE](https://go.microsoft.com/fwlink/?LinkId=402413)
- [SQL](https://msdn.microsoft.com/library/azure/dn782250.aspx)

## <a name="documentdb-python-api-and-sdk"></a>DocumentDB Python API ja SDK

<table>
<tr><td>**Lataa SDK**</td><td>[PyPI](https://pypi.python.org/pypi/pydocumentdb)</td></tr>
<tr><td>**API dokumentaatio**</td><td>[Python API oppaat](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.html)</td></tr>
<tr><td>**SDK asennusohjeet**</td><td>[Python SDK asennusohjeet](http://azure.github.io/azure-documentdb-python/)</td></tr>
<tr><td>**Jakamia SDK-paketissa**</td><td>[GitHub](https://github.com/Azure/azure-documentdb-python)</td></tr>
<tr><td>**Käytön aloittaminen**</td><td>[Python SDK: N käytön aloittaminen](documentdb-python-application.md)</td></tr>
<tr><td>**Nykyinen tuetut käyttöympäristö**</td><td>[Python 2.7](https://www.python.org/downloads/) ja [Python 3.5](https://www.python.org/downloads/)</td></tr>
</table></br>

## <a name="release-notes"></a>Julkaisutiedot

### <a name="a-name200200httpspypipythonorgpypipydocumentdb200"></a><a name="2.0.0"/>[2.0.0](https://pypi.python.org/pypi/pydocumentdb/2.0.0)
- Tukee nyt Python 3.5.
- Tukee nyt yhteysvarannon pyynnöt-moduulin.
- Tukee nyt istunnon yhdenmukaisuuden.
- Tukee nyt ylös/lajittelu kyselyjen osioitua loppu.


### <a name="a-name190190httpspypipythonorgpypipydocumentdb190"></a><a name="1.9.0"/>[1.9.0](https://pypi.python.org/pypi/pydocumentdb/1.9.0)
- Lisätty uudelleen käytännön tuki kyselyn pyynnöt. (Kyselyn pyynnöt saavat pyynnön korko liian suuri poikkeuksen, virhekoodi 429.) Oletusarvon mukaan DocumentDB yrittää suorittaa uudelleen yhdeksän kertaa sivupyynnön virhekoodi 429 havaitaan, kun honoring vastauksen otsikon retryAfter-aika. Kiinteä uudelleen välin aika nyt voidaan määrittää RetryOptions-ominaisuuden osana ConnectionPolicy objektin Jos haluat ohittaa palauttama palvelimen välillä uudelleenyritykset retryAfter aika. DocumentDB nyt odottaa enintään 30 sekuntia kunkin pyytää pakotetaan (riippumatta Laske uudelleen) ja palauttaa virhekoodi 429 vastauksen. Tällä hetkellä voi olla myös ohittaa ConnectionPolicy objektin RetryOptions-ominaisuudessa.

- DocumentDB palauttaa nyt x-ms-rajoituksen-yritä-Laske- ja x-ms-throttle-retry-wait-time-ms kuin jokaisen pyynnön rajoituksen osoittamaan vastauksen otsikkoa uudelleen Laske- ja pyyntö odotti välillä uudelleenyritykset cummulative aika.

- RetryPolicy-luokka ja tarjoamia document_client luokan vastaavan ominaisuuden (retry_policy) poistetaan ja käyttöön sen sijaan RetryOptions luokan paljastaa ConnectionPolicy luokka, joka voidaan ohittaa joitakin uudelleen oletusasetusten RetryOptions-ominaisuus.

### <a name="a-name180180httpspypipythonorgpypipydocumentdb180"></a><a name="1.8.0"/>[1.8.0](https://pypi.python.org/pypi/pydocumentdb/1.8.0)
  - Lisätä monille tietokannan tilit tuki.

### <a name="a-name170170httpspypipythonorgpypipydocumentdb170"></a><a name="1.7.0"/>[1.7.0](https://pypi.python.org/pypi/pydocumentdb/1.7.0)
- Lisätä aika-Live(TTL) ominaisuuden asiakirjojen tuki.

### <a name="a-name161161httpspypipythonorgpypipydocumentdb161"></a><a name="1.6.1"/>[1.6.1](https://pypi.python.org/pypi/pydocumentdb/1.6.1)
- Virheenkorjauksia liittyvät palvelimen puoli jakaminen sallimaan erikoismerkkejä partitionkey polku.

### <a name="a-name160160httpspypipythonorgpypipydocumentdb160"></a><a name="1.6.0"/>[1.6.0](https://pypi.python.org/pypi/pydocumentdb/1.6.0)
- Toteutuksen [osioitua sivustokokoelmat](documentdb-partition-data.md) ja [käyttäjän määrittämät suorituskykyä](documentdb-performance-levels.md). 

### <a name="a-name150150httpspypipythonorgpypipydocumentdb150"></a><a name="1.5.0"/>[1.5.0](https://pypi.python.org/pypi/pydocumentdb/1.5.0)
- Lisää Hash & alueen osion resolvers helpottamiseksi sharding sovellusten useita osioiden välillä.

### <a name="a-name142142httpspypipythonorgpypipydocumentdb142"></a><a name="1.4.2"/>[jäljempänä 1.4.2](https://pypi.python.org/pypi/pydocumentdb/1.4.2)
- Ota käyttöön Upsert. Uusien UpsertXXX menetelmien lisätään tue Upsert-ominaisuutta.
- Käyttöönotto-tunnuksen mukaan reititys. Julkisen API muutoksia, kaikki muutokset sisäinen.

### <a name="a-name120120httpspypipythonorgpypipydocumentdb120"></a><a name="1.2.0"/>[1.2.0](https://pypi.python.org/pypi/pydocumentdb/1.2.0)
- Tukee Geospatiaalisia indeksi.
- Tarkistaa kaikki resurssit id-ominaisuutta. Resurssien tunnukset ei voi olla ?, /, #, \, merkkejä tai välilyönnillä loppuun.
- Lisää uusi otsikko "indeksi muunnos edistyminen" ResourceResponse.

### <a name="a-name110110httpspypipythonorgpypipydocumentdb110"></a><a name="1.1.0"/>[1.1.0](https://pypi.python.org/pypi/pydocumentdb/1.1.0)
- Ottaa käyttöön V2 indeksoinnin käytännön.

### <a name="a-name101101httpspypipythonorgpypipydocumentdb101"></a><a name="1.0.1"/>[1.0.1](https://pypi.python.org/pypi/pydocumentdb/1.0.1)
- Tukee yhteyden välityspalvelimen kautta.

### <a name="a-name100100httpspypipythonorgpypipydocumentdb100"></a><a name="1.0.0"/>[1.0.0](https://pypi.python.org/pypi/pydocumentdb/1.0.0)
- GA SDK.

## <a name="release--retirement-dates"></a>Vapauta & ja käytöstä poistaminen
Microsoft tarjoaa ilmoitus on vähintään **12 kuukautta** ennen poistamisesta SDK jotta tasaiset uudempaan/tuettu versio siirtymä.

Uusia ominaisuuksia ja toimintoja ja optimointi lisätään vain nykyiseen SDK-paketissa, sellaisenaan on suositeltavaa, että aina päivität SDK uusin aikaisin kuin mahdollista. 

Minkä tahansa DocumentDB käyttämällä käytöstä poistettuja SDK pyyntö hylätään palvelu.

> [AZURE.WARNING]
Kaikkien versioiden Python Azure DocumentDB SDK-paketissa ennen versio **1.0.0** poistettu käytöstä, **29 helmikuussa 2016**. 

<br/>

| Versio | Julkaisupäivä | Päivämäärän käytöstä poistaminen 
| ---     | ---          | ---
| [2.0.0](#2.0.0) | 29 syyskuussa 2016 |---
| [1.9.0](#1.9.0) | 07 heinäkuussa 2016 |---
| [1.8.0](#1.8.0) | 2016 14 kesäkuussa |---
| [1.7.0](#1.7.0) | 2016: n huhtikuun 26 |---
| [1.6.1](#1.6.1) | 2016: n huhtikuun 08 |---
| [1.6.0](#1.6.0) | 29 maaliskuussa 2016 |---
| [1.5.0](#1.5.0) | 03 tammikuussa 2016 |---
| [jäljempänä 1.4.2](#1.4.2) | 06 lokakuussa 2015 |---
| [1.4.1](#1.4.1) | 06 lokakuussa 2015 |---
| [1.2.0](#1.2.0) | 06 elokuussa 2015 |---
| [1.1.0](#1.1.0) | 09 heinäkuussa 2015 |---
| [1.0.1](#1.0.1) | 25. toukokuu 2015 |---
| [1.0.0](#1.0.0) | 07. huhtikuussa 2015 |---
| 0.9.4-prelease | 14. tammikuussa 2015 | 29 helmikuussa 2016
| 0.9.3-prelease | 09. joulukuussa 2014 | 29 helmikuussa 2016
| 0.9.2-prelease | 25. marraskuussa 2014 | 29 helmikuussa 2016
| 0.9.1-prelease | 23. syyskuussa 2014 | 29 helmikuussa 2016
| 0.9.0-prelease | 21. elokuussa 2014 | 29 helmikuussa 2016

## <a name="faq"></a>USEIN KYSYTYT KYSYMYKSET
[AZURE.INCLUDE [documentdb-sdk-faq](../../includes/documentdb-sdk-faq.md)]

## <a name="see-also"></a>Katso myös

Lisätietoja DocumentDB on artikkelissa [Microsoft Azure DocumentDB](https://azure.microsoft.com/services/documentdb/) palvelu‑sivulla. 
