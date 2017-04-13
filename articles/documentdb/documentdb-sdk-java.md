
<properties
    pageTitle="DocumentDB Java API & SDK | Microsoft Azure"
    description="Lisätietoja Java-Ohjelmointirajapintaan ja SDK esimerkiksi release päivämäärät ja käytöstä poistaminen päivämäärien välillä kunkin DocumentDB Java SDK-versiossa tehdyt muutokset."
    services="documentdb"
    documentationCenter="java"
    authors="rnagpal"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="documentdb"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="java"
    ms.topic="article"
    ms.date="10/28/2016"
    ms.author="rnagpal"/>

# <a name="documentdb-apis-and-sdks"></a>DocumentDB API ja SDK: T

> [AZURE.SELECTOR]
- [.NET](documentdb-sdk-dotnet.md)
- [Node.js](documentdb-sdk-node.md)
- [Java](documentdb-sdk-java.md)
- [Python](documentdb-sdk-python.md)
- [MUILLE KÄYTTÄJILLE](https://go.microsoft.com/fwlink/?LinkId=402413)
- [SQL](https://msdn.microsoft.com/library/azure/dn782250.aspx)

## <a name="documentdb-java-api-and-sdk"></a>DocumentDB Java API ja SDK

<table>
<tr><td>**SDK-Lataa**</td><td>[Maven-testi](http://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22com.microsoft.azure%22%20AND%20a%3A%22azure-documentdb%22)</td></tr>
<tr><td>**API dokumentaatio**</td><td>[Java-Ohjelmointirajapinnan oppaat](http://azure.github.io/azure-documentdb-java/)</td></tr>
<tr><td>**Osallistuja SDK-paketissa**</td><td>[GitHub](https://github.com/Azure/azure-documentdb-java/)</td></tr>
<tr><td>**Käytön aloittaminen**</td><td>[Java SDK: N käytön aloittaminen](documentdb-java-application.md)</td></tr>
<tr><td>**Nykyinen tuetut runtime**</td><td>[JDK 7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html)</td></tr>
</table></br>

## <a name="release-notes"></a>Julkaisutiedot

### <a name="a-name191191httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb191"></a><a name="1.9.1"/>[1.9.1](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.9.1)

  - Tukee nyt BoundedStaleness yhdenmukaisuuden taso.
  - Tukee nyt suora yhteys CRUD toimille osioitua loppu.
  - Kiinteä-tietokantakyselyn SQL ohjelmavirhe.
  - Kiinteä ohjelmavirhe istunnon välimuistin missä istunnon tunnuksen voidaan määrittää väärin.

### <a name="a-name190190httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb190"></a><a name="1.9.0"/>[1.9.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.9.0)

  - Tukee nyt cross osion rinnakkain kyselyt.
  - Tukee nyt ylös/ORDER BY-kyselyiden osioitua loppu.
  - Tukee nyt vahva yhdenmukaisuuden.
  - Tukee nyt nimi pyynnöt suoraa yhteyttä käytettäessä.
  - Tee ActivityId pysy yhdenmukaisia koko kaikki pyynnön uudelleenyritykset kiinnitetty.
  - Kiinteä ohjelmavirhe liittyvät istunnon välimuistiin, kun luotaessa kokoelma, jolla on sama nimi.
  - Lisätty monikulmio ja LineString tietotyypit määrittäminen sivustokokoelman käytännön geo aitaus paikkatietojen kyselyjen indeksoinnin aikana.
  - Kiinteä Java Doc Java 1.8 liittyvät ongelmat.

### <a name="a-name181181httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb181"></a><a name="1.8.1"/>[1.8.1](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.8.1)
  - Kiinteä ohjelmavirhe PartitionKeyDefinitionMap välimuistiin osio sivustokokoelmat ja soita ylimääräisiä Nouda osion avaimen pyynnöt.
  - Kiinteä ohjelmavirhe ei uudelleen, kun virheellinen osion avainarvon on annettu.

### <a name="a-name180180httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb180"></a><a name="1.8.0"/>[1.8.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.8.0)
  - Lisätä monille tietokannan tilit tuki.
  - Valitse kyselyn pyynnöt asetuksilla voit mukauttaa max uudelleenyritykset ja max odotusaikaa automaattisesti uudelleen lisätty tuki.  Katso RetryOptions ja ConnectionPolicy.getRetryOptions().
  - Vanhentuneen IPartitionResolver perusteella osioinnin mukautettua koodia. Käytä osioitua sivustokokoelmat suurempi tallennus-ja siirtonopeuden.

### <a name="a-name171171httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb171"></a><a name="1.7.1"/>[1.7.1](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.7.1)
- Lisätty uudelleen käytännön tuki rajoitusta.  

### <a name="a-name170170httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb170"></a><a name="1.7.0"/>[1.7.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.7.0)
- Asiakirjojen live (TTL)-tuen lisätty aika.

### <a name="a-name160160httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb160"></a><a name="1.6.0"/>[1.6.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.6.0)
- Toteutuksen [osioitua sivustokokoelmat](documentdb-partition-data.md) ja [käyttäjän määrittämät suorituskykyä](documentdb-performance-levels.md).

### <a name="a-name151151httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb151"></a><a name="1.5.1"/>[1.5.1](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.5.1)
- Kiinteä ohjelmavirhe HashPartitionResolver luomiseen little endian yhtenäistämistä muiden SDK: T hash-arvot.

### <a name="a-name150150httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb150"></a><a name="1.5.0"/>[1.5.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.5.0)
- Lisää Hash & alueen osion resolvers helpottamiseksi sharding sovellusten useita osioiden välillä.

### <a name="a-name140140httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb140"></a><a name="1.4.0"/>[1.4.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.4.0)
- Ota käyttöön Upsert. Uusien upsertXXX menetelmien lisätään tue Upsert-ominaisuutta.
- Käyttöönotto-tunnuksen mukaan reititys. Julkisen API muutoksia, kaikki muutokset sisäinen.

### <a name="a-name130130"></a><a name="1.3.0"/>1.3.0
- Vapauta ohitettu voit tuoda versionumero tasaus ja muut SDK: T

### <a name="a-name120120httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb120"></a><a name="1.2.0"/>[1.2.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.2.0)
- Tukee Geospatiaalisia indeksi
- Tarkistaa kaikki resurssit id-ominaisuutta. Resurssien tunnukset ei voi olla ?, /, #, \, merkkejä tai välilyönnillä loppuun.
- Lisää uusi otsikko "indeksi muunnos edistyminen" ResourceResponse.

### <a name="a-name110110httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb110"></a><a name="1.1.0"/>[1.1.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.1.0)
- Toteuttaa V2 indeksoinnin käytäntö

### <a name="a-name100100httpmvnrepositorycomartifactcommicrosoftazureazure-documentdb100"></a><a name="1.0.0"/>[1.0.0](http://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb/1.0.0)
- GA SDK-PAKETISSA

## <a name="release--retirement-dates"></a>Vapauta ja käytöstä poistaminen päivämäärät
Microsoft tarjoaa ilmoitus on vähintään **12 kuukautta** ennen poistamisesta SDK jotta tasaiset uudempaan/tuettu versio siirtymä.

Uusia ominaisuuksia ja toimintoja ja optimointi lisätään vain nykyiseen SDK-paketissa, sellaisenaan on suositeltavaa, että aina päivität SDK uusin aikaisin kuin mahdollista.

Minkä tahansa DocumentDB käyttämällä käytöstä poistettuja SDK pyyntö hylätään palvelu.

> [AZURE.WARNING]
Kaikkien versioiden Java Azure DocumentDB SDK-paketissa ennen versio **1.0.0** poistettu käytöstä, **29 helmikuussa 2016**.

<br/>

| Versio | Julkaisupäivä | Päivämäärän käytöstä poistaminen
| ---     | ---          | ---
| [1.9.1](#1.9.1) | 28 lokakuussa 2016 |---
| [1.9.0](#1.9.0) | 03 lokakuussa 2016 |---
| [1.8.1](#1.8.1) | 30 kesäkuussa 2016 |---
| [1.8.0](#1.8.0) | 2016 14 kesäkuussa |---
| [1.7.1](#1.7.1) | 2016: n huhtikuun 30 |---
| [1.7.0](#1.7.0) | 2016: n huhtikuun 27 |---
| [1.6.0](#1.6.0) | 29 maaliskuussa 2016 |---
| [1.5.1](#1.5.1) | 31. joulukuuta 2015 |---
| [1.5.0](#1.5.0) | 04 joulukuussa 2015 |---
| [1.4.0](#1.4.0) | 05 lokakuussa 2015 |---
| [1.3.0](#1.3.0) | 05 lokakuussa 2015 |---
| [1.2.0](#1.2.0) | 05 elokuussa 2015 |---
| [1.1.0](#1.1.0) | 09 heinäkuussa 2015 |---
| [1.0.1](#1.0.1) | 12. toukokuu 2015 |---
| [1.0.0](#1.0.0) | 07. huhtikuussa 2015 |---
| 0.9.5-prelease | 2015 09 maalis | 29 helmikuussa 2016
| 0.9.4-prelease | 17. helmikuussa 2015 | 29 helmikuussa 2016
| 0.9.3-prelease | 13. tammikuussa 2015 | 29 helmikuussa 2016
| 0.9.2-prelease | 19. joulukuussa 2014 | 29 helmikuussa 2016
| 0.9.1-prelease | 19. joulukuussa 2014 | 29 helmikuussa 2016
| 0.9.0-prelease | 10. joulukuussa 2014 | 29 helmikuussa 2016

## <a name="faq"></a>USEIN KYSYTYT KYSYMYKSET
[AZURE.INCLUDE [documentdb-sdk-faq](../../includes/documentdb-sdk-faq.md)]

## <a name="see-also"></a>Katso myös

Lisätietoja DocumentDB on artikkelissa [Microsoft Azure DocumentDB](https://azure.microsoft.com/services/documentdb/) palvelu‑sivulla.
