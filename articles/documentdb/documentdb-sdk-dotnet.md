<properties 
    pageTitle="DocumentDB .NET API & SDK | Microsoft Azure" 
    description="Lisätietoja .NET API ja SDK esimerkiksi release päivämäärät ja käytöstä poistaminen päivämäärien välillä kunkin DocumentDB .NET SDK-versiossa tehdyt muutokset." 
    services="documentdb" 
    documentationCenter=".net" 
    authors="rnagpal" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="10/27/2016" 
    ms.author="rnagpal"/>

# <a name="documentdb-apis-and-sdks"></a>DocumentDB API ja SDK: T 

> [AZURE.SELECTOR]
- [.NET](documentdb-sdk-dotnet.md)
- [Node.js](documentdb-sdk-node.md)
- [Java](documentdb-sdk-java.md)
- [Python](documentdb-sdk-python.md)
- [MUILLE KÄYTTÄJILLE](https://go.microsoft.com/fwlink/?LinkId=402413)
- [SQL](https://msdn.microsoft.com/library/azure/dn782250.aspx)

## <a name="documentdb-net-api-and-sdk"></a>DocumentDB .NET API ja SDK

<table>
<tr><td>**SDK-Lataa**</td><td>[NuGet](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/)</td></tr>
<tr><td>**API dokumentaatio**</td><td>[.NET-Ohjelmointirajapinnan oppaat](https://msdn.microsoft.com/library/azure/dn948556.aspx)</td></tr>
<tr><td>**Esimerkkejä, joiden**</td><td>[.NET MALLIKOODEJA](documentdb-dotnet-samples.md)</td></tr>
<tr><td>**Käytön aloittaminen**</td><td>[DocumentDB .NET SDK: N käytön aloittaminen](documentdb-get-started.md)</td></tr>
<tr><td>**Web app-opas**</td><td>[Web-sovellusten kehittämiseen DocumentDB kanssa](documentdb-dotnet-application.md)</td></tr>
<tr><td>**Nykyisen tuetut framework**</td><td>[Microsoft .NET Framework 4.5](https://www.microsoft.com/download/details.aspx?id=30653)</td></tr>
</table></br>

## <a name="release-notes"></a>Julkaisutiedot

> [AZURE.IMPORTANT] Versiota 1.9.2 alkaen saattaa tulla System.NotSupportedException kun kyselyt osioitua sivustokokoelmat. Voit välttää tämän virheen, varmista, että host-prosessi on 64-bittinen. Suoritettava projektien tämä onnistuu poistamalla Luo-välilehden projektin ominaisuudet-ikkunassa "Ensisijainen 32-bittinen"-asetus.

### <a name="a-name11001100httpswwwnugetorgpackagesmicrosoftazuredocumentdb1100"></a><a name="1.10.0"/>[1.10.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.10.0)

  - Lisätty suora yhteys tuki osioitua sivustokokoelmat.
  - Parannettu suorituskyky, joka Staleness yhdenmukaisuuden tason.
  - Lisätty monikulmio ja LineString tietotyypit määrittäminen sivustokokoelman käytännön geo aitaus paikkatietojen kyselyjen indeksoinnin aikana.
  - LINQ tukee nyt StringEnumConverter, IsoDateTimeConverter ja UnixDateTimeConverter aikana kääntäminen predikaatit.
  - Eri SDK virheenkorjauksia.

### <a name="a-name195195httpswwwnugetorgpackagesmicrosoftazuredocumentdb195"></a><a name="1.9.5"/>[1.9.5](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.9.5)

  - Kiinteä ongelman, joka aiheuttaa seuraavat NotFoundException: Lue istunnon ei ole käytettävissä syötteen istunnon tunnuksen. Tätä poikkeusta tapahtui joissakin tapauksissa, kun kysely suoritetaan geo distributed tilin luku-alue.
  - Näkyvät ResourceResponse-luokka, joka mahdollistaa tutustuminen pohjana virta-vastauksen ResponseStream-ominaisuutta.

### <a name="a-name194194httpswwwnugetorgpackagesmicrosoftazuredocumentdb194"></a><a name="1.9.4"/>[1.9.4](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.9.4)

  - Muokannut ResourceResponse, FeedResponse, StoredProcedureResponse ja MediaResponse luokkia toteuttamisesta vastaavan julkisen liittymän niin, että voit mocked testin perustuva käyttöönoton (TDD).
  - Voit korjata ongelman, joka aiheuttaa Vääränlainen osion avaimen ylätunnisteen käytettäessä mukautettu JsonSerializerSettings objekti sarjoittamista tiedoille.

### <a name="a-name193193httpswwwnugetorgpackagesmicrosoftazuredocumentdb193"></a><a name="1.9.3"/>[1.9.3](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.9.3)

  - Kiinteä ongelman, joka aiheuttaa pitkään käynnissä kyselyt voi epäonnistua virheen vuoksi: luvan tunnuksen ei ole kelvollinen tällä hetkellä.
  - Voit korjata ongelman, joka poistaa alkuperäisen SqlParameterCollection-osion Kyselyt ylös /-lajittelu cross.

### <a name="a-name192192httpswwwnugetorgpackagesmicrosoftazuredocumentdb192"></a><a name="1.9.2"/>[1.9.2](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.9.2)

  - Tukee nyt rinnakkain kyselyjen osioitua loppu.
  - Tukee nyt cross osion lajittelu- ja TOP kyselyjen osioitua loppu.
  - Korjaa puuttuvat viittaukset DocumentDB.Spatial.Sql.dll ja Microsoft.Azure.Documents.ServiceInterop.dll, joita tarvitaan viittaat DocumentDB projektin DocumentDB Nuget paketin viittaus.
  - Kiinteä voi käyttää erityyppisiä parametreja käyttäjän määrittämien funktioiden käyttäminen LINQ. 
  - Kiinteä ohjelmavirhe yleisesti replikoitua tilien, jossa Upsert puhelut parhaillaan ohjataan lukemaan sijainnit sijaan kirjoittaminen sijainnit.
  - Lisätty menetelmät IDocumentClient liittymään, jotka olivat puuttuu: 
      - UpsertAttachmentAsync menetelmä, joka kestää mediaStream ja parametreiksi asetukset
      - CreateAttachmentAsync menetelmä, joka kestää parametrina asetukset
      - CreateOfferQuery menetelmä, joka kestää querySpec parametrina.
  - Sinetit julkisen luokat, jotka ovat tarjoamia IDocumentClient käyttöliittymän.

### <a name="a-name180180httpswwwnugetorgpackagesmicrosoftazuredocumentdb180"></a><a name="1.8.0"/>[1.8.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.8.0)
  - Lisätä monille tietokannan tilit tuki.
  - Tukee nyt kyselyn pyynnöt, yritä uudelleen.  Käyttäjä voi mukauttaa uudelleenyritykset ja max odotusaika määrittämällä ConnectionPolicy.RetryOptions-ominaisuus.
  - Lisätä näkyy IDocumentClient-käyttöliittymä, joka määrittää kaikki DocumenClient ominaisuuksien ja menetelmien allekirjoitukset.  Osana muutoksen muuttaa myös tunniste menetelmiä, jotka luovat IQueryable ja IOrderedQueryable menetelmiä DocumentClient luokka itse.
  - Lisätä määritykset, kun haluat määrittää tietyn DocumentDB päätepisteen Uri ServicePoint.ConnectionLimit.  ConnectionPolicy.MaxConnectionLimit avulla voit muuttaa oletusarvon, joka on määritetty 50.
  - Vanhentuneen IPartitionResolver ja ylläpitokustannusten.  IPartitionResolver tuki on nyt vanhentunut. On suositeltavaa, että käytät osioitu sivustokokoelmat suurempi säilytys- ja siirtonopeuden.


### <a name="a-name171171httpswwwnugetorgpackagesmicrosoftazuredocumentdb171"></a><a name="1.7.1"/>[1.7.1](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.7.1)
  - Lisätä URI-osoite liikaa perusteella, joka vie RequestOptions parametrina ExecuteStoredProcedureAsync-menetelmää.
  
### <a name="a-name170170httpswwwnugetorgpackagesmicrosoftazuredocumentdb170"></a><a name="1.7.0"/>[1.7.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.7.0)
  - Asiakirjojen live (TTL)-tuen lisätty aika.

### <a name="a-name163163httpswwwnugetorgpackagesmicrosoftazuredocumentdb163"></a><a name="1.6.3"/>[1.6.3](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.6.3)
  - Kiinteä ohjelmavirhe Nuget pakkauksen .NET SDK pakkaamista sitä pilvipalvelussa Azure-ratkaisun osana.
  
### <a name="a-name162162httpswwwnugetorgpackagesmicrosoftazuredocumentdb162"></a><a name="1.6.2"/>[1.6.2](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.6.2)
  - Toteutuksen [osioitua sivustokokoelmat](documentdb-partition-data.md) ja [käyttäjän määrittämät suorituskykyä](documentdb-performance-levels.md). 

### <a name="a-name153153httpswwwnugetorgpackagesmicrosoftazuredocumentdb153"></a><a name="1.5.3"/>[1.5.3](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.5.3)
  - **[Kiinteä]** Kyselyt DocumentDB päätepisteen aiheuttaa: "System.Net.Http.HttpRequestException: Virhe sisällön kopioiminen stream.

### <a name="a-name152152httpswwwnugetorgpackagesmicrosoftazuredocumentdb152"></a><a name="1.5.2"/>[1.5.2](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.5.2)
  - Laajennettu LINQ tuki, mukaan lukien uudet sivutuksen, ehdollisten lausekkeiden operaattorit ja alue vertailu.
    - Ota käyttöön Valitse TOP toimintaa LINQ operaattori
    - CompareTo operaattori ottamaan merkkijonon alueen vertailu
    - Ehdollinen (?) ja yhdistettyä kyselyä operaattorit (?)
  - **[Kiinteä]** ArgumentOutOfRangeException yhdisteltäessä mallin ennuste ja mihin-linq kyselyn.  [#81](https://github.com/Azure/azure-documentdb-dotnet/issues/81)

### <a name="a-name151151httpswwwnugetorgpackagesmicrosoftazuredocumentdb151"></a><a name="1.5.1"/>[1.5.1](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.5.1)
 - **[Kiinteä]** Jos valitse ei ole viimeinen lausekkeen LINQ-palvelu ei ole ennuste oletetaan ja valmistettu Valitse * väärin.  [#58](https://github.com/Azure/azure-documentdb-dotnet/issues/58)

### <a name="a-name150150httpswwwnugetorgpackagesmicrosoftazuredocumentdb150"></a><a name="1.5.0"/>[1.5.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.5.0)
 - Toteutettu Upsert lisätty UpsertXXXAsync menetelmät
 - Kaikki palvelupyynnöt suorituskykyparannukset
 - Ehdollinen, LINQ tarjoajan tuki yhdistettyä kyselyä ja CompareTo menetelmiä merkkijonojen yhteydessä
 - **[Kiinteä]** LINQ tarjoajan--> toteuttaminen sisältää menetelmä luo IEnumerable ja matriisi kuin saman SQL-luettelossa
 - **[Kiinteä]** BackoffRetryUtility käyttää samaa HttpRequestMessage uudelleen sen sijaan, että luot uuden uudelleen
 - **[Vanhentunut]** UriFactory.CreateCollection--> pitäisi nyt käyttää UriFactory.CreateDocumentCollection
 
### <a name="a-name141141httpswwwnugetorgpackagesmicrosoftazuredocumentdb141"></a><a name="1.4.1"/>[1.4.1](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.4.1)
 - **[Kiinteä]** Sovellus on lokalisoitu ongelmat käytettäessä ei fi culture tiedot, kuten noudattavat-noudattavat jne. 
 
### <a name="a-name140140httpswwwnugetorgpackagesmicrosoftazuredocumentdb140"></a><a name="1.4.0"/>[1.4.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.4.0)
  - Tunnuksen mukaan reititys
    - Uusi UriFactory helper helpottamiseksi rakentaa tunnuksen perusteella resurssin linkit
    - Uusi Osastollasi DocumentClient URI ottamisesta käyttöön
  - Lisätty IsValid() ja geospatiaalisia LINQ IsValidDetailed()
  - Parannettu LINQ tarjoajan tuki
    - **Matemaattiset** - itseisarvo-Acos-Asin Atan-enimmäismäärän Cos eksponentti, Floor, Log, Log10, Pow, PYÖRISTÄ-funktiota, merkki, Sin, Sqrt-Tan, rajaa
    - **Merkkijono** - ketjutettu, sisältää EndsWith, IndexOf, Laske, ToLower, TrimStart, korvaa, peruuttaa TrimEnd, StartsWith, alimerkkijono ToUpper
    - **Matriisi** - ketjutettu, sisältää, määrä
    - **IN** -operaattori

### <a name="a-name130130httpswwwnugetorgpackagesmicrosoftazuredocumentdb130"></a><a name="1.3.0"/>[1.3.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.3.0)
  - Tukee nyt indeksoinnin käytännöt muokkaaminen
    - Uusi ReplaceDocumentCollectionAsync menetelmä DocumentClient
    - Uusi IndexTransformationProgress ominaisuuden ResourceResponse<T> , indeksi käytännön muutokset prosentti etenemisen seuranta
    - DocumentCollection.IndexingPolicy on nyt mutable
  - Lisätty indeksoinnin ja kysely-tuki
    - Uusi Microsoft.Azure.Documents.Spatial nimitilan sarjoitettaessa/poistettaessa paikkatietojen tyypit, kuten piste- ja monikulmio
    - Uuden SpatialIndex luokan indeksoinnin GeoJSON DocumentDB tallennetut tiedot
  - **[Vakio]** : luotu linq lausekkeen [#38](https://github.com/Azure/azure-documentdb-net/issues/38) virheellinen SQL-kysely

### <a name="a-name120120httpswwwnugetorgpackagesmicrosoftazuredocumentdb120"></a><a name="1.2.0"/>[1.2.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.2.0)
- Newtonsoft.Json v5.0.7 riippuvuus 
- Order By tukemaan muutokset
  - Palvelun LINQ OrderBy() tai OrderByDescending() tuki
  - Order By tukemaan IndexingPolicy 
  
        **NB: Possible breaking change** 
  
        If you have existing code that provisions collections with a custom indexing policy, then your existing code will need to be updated to support the new IndexingPolicy class. If you have no custom indexing policy, then this change does not affect you.

### <a name="a-name110110httpswwwnugetorgpackagesmicrosoftazuredocumentdb110"></a><a name="1.1.0"/>[1.1.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.1.0)
- Tietojen jakaminen käyttämällä uusia HashPartitionResolver ja RangePartitionResolver luokkia ja IPartitionResolver tuki
- DataContract Sarjatoiminto
- LINQ tarjoajan GUID-tunnus-tuki
- LINQ UDF-tuki

### <a name="a-name100100httpswwwnugetorgpackagesmicrosoftazuredocumentdb100"></a><a name="1.0.0"/>[1.0.0](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/1.0.0)
- GA SDK-PAKETISSA

> [AZURE.NOTE]
Tapahtui NuGet pakettinimi esikatselu ja GA. muutos Olemme siirretään **Microsoft.Azure.Documents.Client** **Microsoft.Azure.DocumentDB**
<br/>


### <a name="a-name09x-preview09x-previewhttpswwwnugetorgpackagesmicrosoftazuredocumentsclient"></a><a name="0.9.x-preview"/>[0.9.x-Preview](https://www.nuget.org/packages/Microsoft.Azure.Documents.Client)
- Esikatselu SDK: T [vanhentuneen]

## <a name="release--retirement-dates"></a>Vapauta ja käytöstä poistaminen päivämäärät
Microsoft tarjoaa ilmoitus on vähintään **12 kuukautta** ennen poistamisesta SDK jotta tasaiset uudempaan/tuettu versio siirtymä.

Uusia ominaisuuksia ja toimintoja ja optimointi lisätään vain nykyiseen SDK-paketissa, sellaisenaan on suositeltavaa, että aina päivität SDK uusin aikaisin kuin mahdollista. 

Minkä tahansa DocumentDB käyttämällä käytöstä poistettuja SDK pyyntö hylätään palvelu.

> [AZURE.WARNING]
Kaikkien versioiden .NET Azure DocumentDB SDK-paketissa ennen versio **1.0.0** poistettu käytöstä, **29 helmikuussa 2016**. 
 
<br/>
 
| Versio | Julkaisupäivä | Päivämäärän käytöstä poistaminen 
| ---     | ---          | ---
| [1.10.0](#1.10.0) | 27 syyskuussa 2016 |---
| [1.9.5](#1.9.5) | 01 syyskuussa 2016 |---
| [1.9.4](#1.9.4) | 24 elokuussa 2016 |---
| [1.9.3](#1.9.3) | 15 elokuussa 2016 |---
| [1.9.2](#1.9.2) | 23 heinäkuussa 2016 |---
| 1.9.1 | Poistettu |---
| 1.9.0 | Poistettu |---
| [1.8.0](#1.8.0) | 2016 14 kesäkuussa |---
| [1.7.1](#1.7.1) | 2016 06 toukokuu |---
| [1.7.0](#1.7.0) | 26 huhtikuussa 2016 |---
| [1.6.3](#1.6.3) | 2016: n huhtikuun 08 |---
| [1.6.2](#1.6.2) | 29 maaliskuussa 2016 |---
| [1.5.3](#1.5.3) | 19 helmikuussa 2016 |---
| [1.5.2](#1.5.2) | 14 joulukuussa 2015 |---
| [1.5.1](#1.5.1) | 23 marraskuun 2015 |---
| [1.5.0](#1.5.0) | 05 lokakuussa 2015 |---
| [1.4.1](#1.4.1) | 25 elokuussa 2015 |---
| [1.4.0](#1.4.0) | 13 elokuussa 2015 |---
| [1.3.0](#1.3.0) | 05 elokuussa 2015 |---
| [1.2.0](#1.2.0) | 06 heinäkuussa 2015 |---
| [1.1.0](#1.1.0) | 30. huhtikuussa 2015 |---
| [1.0.0](#1.0.0) | 08. huhtikuussa 2015 |---
| [0.9.3-prelease](#0.9.x-preview) | 12. maaliskuu 2015 | 29 helmikuussa 2016
| [0.9.2-prelease](#0.9.x-preview) | Tammikuussa 2015 | 29 helmikuussa 2016
| [.9.1-prelease](#0.9.x-preview) | 13. lokakuussa 2014 | 29 helmikuussa 2016
| [0.9.0-prelease](#0.9.x-preview) | 21. elokuussa 2014 | 29 helmikuussa 2016

## <a name="faq"></a>USEIN KYSYTYT KYSYMYKSET
[AZURE.INCLUDE [documentdb-sdk-faq](../../includes/documentdb-sdk-faq.md)]

## <a name="see-also"></a>Katso myös

Lisätietoja DocumentDB on artikkelissa [Microsoft Azure DocumentDB](https://azure.microsoft.com/services/documentdb/) palvelu‑sivulla. 
