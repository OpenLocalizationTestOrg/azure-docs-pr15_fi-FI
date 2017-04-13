<properties 
    pageTitle="Azure DocumentDB Geospatiaalisia tietojen käsitteleminen | Microsoft Azure" 
    description="Luo, indeksi ja kyselyjen paikkatietojen objektit, joissa on Azure DocumentDB osaavat." 
    services="documentdb" 
    documentationCenter="" 
    authors="arramac" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="documentdb" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="data-services" 
    ms.date="10/25/2016" 
    ms.author="arramac"/>
    
# <a name="working-with-geospatial-data-in-azure-documentdb"></a>Azure DocumentDB Geospatiaalisia tietojen käsitteleminen

Tässä artikkelissa on esittely [Azure DocumentDB](https://azure.microsoft.com/services/documentdb/)geospatiaalisia toimintoja. Luettuasi tämän osaat seuraaviin kysymyksiin:

- Miten Azure DocumentDB paikkatietojen tiedot tallennetaan?
- Miten geospatiaalisia tietoja Azure DocumentDB LINQ ja SQL-kyselyn?
- Miten ottaminen käyttöön tai poistaa DocumentDB paikkatietojen indeksoinnin käytöstä?

Katso tämä [Github projektin](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/Geospatial/Program.cs) MALLIKOODEJA.

## <a name="introduction-to-spatial-data"></a>Johdanto paikkatietojen tiedot

Paikkatietojen tietojen kuvataan sijainti ja tilaa objektien muodon. Useimmissa sovelluksissa nämä vastaavat maapallon, eli geospatiaalisia tietoja objekteja. Paikkatietojen tietojen avulla voidaan esittää henkilölle, paikka halutut tai kaupunki tai järvi reunan sijainti. Yleisiä Käytä tapauksissa liittyä usein lähialue kyselyt, esimerkiksi "löytää kaikki kahvilassa lähellä Omat nykyisen sijainnin". 

### <a name="geojson"></a>GeoJSON
DocumentDB tukee indeksoinnin ja geospatiaalisia-kohdan tiedot, jotka esitetään [GeoJSON määrityksen](http://geojson.org/geojson-spec.html)kyselyä. GeoJSON rakenteet ovat aina voimassa JSON objekteja, jotta voidaan tallentaa taulukoita ja kyselyjä käyttämällä DocumentDB ilman erityistä työkalut tai kirjastot. DocumentDB SDK: T on helper luokat ja tavoista, joiden avulla on helppo paikkatietojen käyttäminen. 

### <a name="points-linestrings-and-polygons"></a>Pisteet, linestrings ja monikulmio
**Pisteen** ilmaisee yksittäinen sijainti-tilaa. Geospatiaalisia tietoja pisteen edustaa tiettyyn sijaintiin, jotka voivat olla katuosoite ruokakauppa, esitys, Auto tai kaupunki.  Pisteen esitetään GeoJSON (ja DocumentDB) käyttämällä sen koordinaatin pari tai pituutta ja leveyttä. Tässä on esimerkki JSON kohtaa varten.

**DocumentDB asioista**

    {
       "type":"Point",
       "coordinates":[ 31.9, -4.8 ]
    }

>[AZURE.NOTE] GeoJSON määrityksen määrittää pituutta etu- ja leveys toiseen. Kuten yhdistäminen muissa sovelluksissa pituutta ja leveyttä ovat kulmista ja ne esitetään kannalta astetta. Pituutta arvot mitataan pituutta pohjoisnavalta ja välillä 180: ja 180.0 astetta, n ja leveyttä arvot mitataan keskiviivaan ja-90.0 välillä ja 90.0 astetta. 
>
> DocumentDB tulkitsee koordinaatit tavalla WGS 84 viittaus järjestelmän kohden. Katso jäljempänä lisätietoja coordinate järjestelmien.

Tämä voi olla upotettu DocumentDB tiedoston sijainnin tiedot sisältävä käyttäjäprofiilin tämän esimerkin mukaisesti:

**Profiilin käyttäminen DocumentDB tallennetaan sijaintiin**

    {
       "id":"documentdb-profile",
       "screen_name":"@DocumentDB",
       "city":"Redmond",
       "topics":[ "NoSQL", "Javascript" ],
       "location":{
          "type":"Point",
          "coordinates":[ 31.9, -4.8 ]
       }
    }

Lisäksi pisteet GeoJSON myös tukee LineStrings ja monikulmio. **LineStrings** vastaa kahden tai useamman pisteen ja viivasegmenttien, joka yhdistää ne. Geospatiaalisia tietoja linestrings käytetään yleensä esittämään moottoritiet tai joet. **Monikulmion** on rajan yhdistetyn asioista, lomakkeiden suljettu LineString. Monikulmio käytetään yleensä esittämään luonnollinen muodostumiin, kuten järviä tai käyttämällä maissa, kuten kaupunkien ja Yhdysvaltojen. Tässä on esimerkki monikulmion-DocumentDB. 

**DocumentDB monikulmio**

    {
       "type":"Polygon",
       "coordinates":[
           [ 31.8, -5 ],
           [ 31.8, -4.7 ],
           [ 32, -4.7 ],
           [ 32, -5 ],
           [ 31.8, -5 ]
       ]
    }

>[AZURE.NOTE] GeoJSON määrityksen edellyttää, että kelvollinen monikulmio varten annettu viimeisen coordinate pari olisi sama kuin ensimmäisen, voit luoda suljettu muoto.
>
>Monikulmion pisteisiin on määritettävä vastapäivään järjestyksessä. Monikulmion määritetty myötäpäivään järjestyksessä edustaa käänteisen alueella.

GeoJSON määrittää pisteen LineString ja monikulmio lisäksi voit ryhmitellä useiden geospatiaalisia sijaintien sekä siitä, miten voit liittää haluamaansa ominaisuudet: n maantieteellinen sijainti **ominaisuus**esittäminen. Nämä objektit ovat kelvollisia JSON, koska ne kaikki voidaan tallentaa ja käsitellä DocumentDB. Kuitenkin DocumentDB tukee vain automaattisen indeksoinnin asioista.

### <a name="coordinate-reference-systems"></a>Järjestelmien järjestämiseen

Maapallon muoto on epäsäännöllinen, geospatiaalisia tietoja koordinaatit esitetään monta coordinate järjestelmien (TPJ), joissa omia kehykset viitekehyksenä ja mittayksiköitä. Esimerkiksi "kansallinen ruudukon, Ison-Britannian" on viittaus-järjestelmä on hyvin tarkasti Iso-Britannia, mutta ei sen ulkopuolella. 

Suosituimpien TPJ käytössä tänään on maailman geodeettisen järjestelmän [WGS 84](http://earth-info.nga.mil/GandG/wgs84/). GPS laitteet ja monta yhdistäminen-palveluista, kuten Google kartat ja Bing Maps-ohjelmointirajapinnan käyttäminen WGS 84. DocumentDB tukee indeksoinnin ja geospatiaalisia tietoja käyttämällä vain WGS 84 TPJ kyselyä. 

## <a name="creating-documents-with-spatial-data"></a>Tiedostojen luominen paikkatietojen tiedoilla
Kun luot GeoJSON arvoja sisältävien tiedostojen, ne on automaattisesti indeksoitu paikkatietojen indeksi kokoelman indeksoinnin käytännön mukaisesti. Jos työskentelet DocumentDB SDK, kuten Python tai Node.js dynaamisesti kirjoitettu kielellä, sinun on luotava kelvollinen GeoJSON.

**Asiakirjan luominen Node.js Geospatiaalisia tietoja**

    var userProfileDocument = {
       "name":"documentdb",
       "location":{
          "type":"Point",
          "coordinates":[ -122.12, 47.66 ]
       }
    };

    client.createDocument(`dbs/${databaseName}/colls/${collectionName}`, userProfileDocument, (err, created) => {
        // additional code within the callback
    });

Jos käsittelet .NET (tai Java) SDK: T, voit upottaa sijaintitiedot sovelluksen objektien uudet piste- ja monikulmio luokat Microsoft.Azure.Documents.Spatial nimitilan. Näiden luokkien auttaa yksinkertaistamaan Sarjatoiminto ja sarjoituksen paikkatietojen tietojen tuominen GeoJSON.

**Asiakirjan luominen .NET Geospatiaalisia tietoja**

    using Microsoft.Azure.Documents.Spatial;
    
    public class UserProfile
    {
        [JsonProperty("name")]
        public string Name { get; set; }

        [JsonProperty("location")]
        public Point Location { get; set; }
        
        // More properties
    }
    
    await client.CreateDocumentAsync(
        UriFactory.CreateDocumentCollectionUri("db", "profiles"), 
        new UserProfile 
        { 
            Name = "documentdb", 
            Location = new Point (-122.12, 47.66) 
        });

Jos ei ole leveys- ja pituusastetiedot tietoja, mutta fyysinen osoite tai sijainti nimi, esimerkiksi Kaupunki tai maa, voit etsiä todellinen koordinaatteja geokoodauksen-palvelun, kuten Bing Maps muiden palvelujen käyttämällä. Lisätietoja Bing Maps-geokoodauksen [tähän](https://msdn.microsoft.com/library/ff701713.aspx).

## <a name="querying-spatial-types"></a>Kyselyt paikkatietojen tyypit

Kun olemme otettuja Katso, kuinka voit lisätä geospatiaalisia tietoja, voit tarkastella tiedot käyttämällä SQL: n ja LINQ DocumentDB kyselyjen tekeminen.

### <a name="spatial-sql-built-in-functions"></a>Paikkatietojen SQL sisäiset funktiot
DocumentDB tukee seuraavia Avaa Geospatiaalisia Consortium (OGC) sisäiset funktiot geospatiaalisia kyselyt. Lisätietoja sarja SQL-kielen sisäiset funktiot Tutustu [Kyselyn DocumentDB](documentdb-sql-query.md).

<table>
<tr>
  <td><strong>Käyttö</strong></td>
  <td><strong>Kuvaus</strong></td>
</tr>
<tr>
  <td>ST_DISTANCE (point_expr, point_expr)</td>
  <td>Palauttaa kahden GeoJSON kohdassa-lausekkeet etäisyys.</td>
</tr>
<tr>
  <td>ST_WITHIN (point_expr, polygon_expr)</td>
  <td>Palauttaa totuusarvolauseke, joka ilmaisee, onko GeoJSON-kohdan ensimmäisessä argumentissa määritettyä sisällä GeoJSON monikulmio toisena argumenttina.</td>
</tr>
<tr>
  <td>ST_ISVALID</td>
  <td>Palauttaa totuusarvon, joka ilmaisee, onko määritetty GeoJSON piste- tai monikulmion lauseke kelvollinen.</td>
</tr>
<tr>
  <td>ST_ISVALIDDETAILED</td>
  <td>Palauttaa JSON-arvo, joka sisältää totuusarvon arvo, jos määritetty GeoJSON piste- tai monikulmion lauseke on voimassa ja jos se on virheellinen, lisäksi merkkijonoarvona syy.</td>
</tr>
</table>

Paikkatietojen funktioiden avulla voidaan suorittaa lähialue kyselyitä, jotka perustuvat paikkatietojen tiedot. Seuraavassa on esimerkiksi kysely, joka palauttaa kaikki perheen tiedostot, jotka ovat valmiita ST_DISTANCE-funktion käyttäminen määritettyyn sijaintiin 30 km. 

**Kyselyn**

    SELECT f.id 
    FROM Families f 
    WHERE ST_DISTANCE(f.location, {'type': 'Point', 'coordinates':[31.9, -4.8]}) < 30000

**Tulokset**

    [{
      "id": "WakefieldFamily"
    }]

Jos sisällytät paikkatietojen indeksoinnin indeksoinnin käytännön, valitse "etäisyys kyselyjen" voi served tehokkaasti hakemiston kautta. Saat lisätietoja paikkatietojen indeksoinnin alla olevasta osiosta. Jos sinulla ei ole määritetty polkujen paikkatietojen indeksin, voit silti suorittaa paikkatietojen kyselyjen määrittämällä `x-ms-documentdb-query-enable-scan` pyynnön otsikkoon arvojoukko "true". .NET-kohdassa tämä voidaan toteuttaa välittämällä **FeedOptions** valinnaista argumenttia kyselyt, joilla [EnableScanInQuery](https://msdn.microsoft.com/library/microsoft.azure.documents.client.feedoptions.enablescaninquery.aspx#P:Microsoft.Azure.Documents.Client.FeedOptions.EnableScanInQuery) arvo on TOSI. 

ST_WITHIN voidaan tarkistaa Jos pisteen osuu monikulmion. Yleisesti monikulmio avulla voidaan esittää rajoja, kuten postinumeroiden tai valtion rajat luonnollinen muodostumiin. Uudelleen Jos sisällytät paikkatietojen indeksoinnin indeksoinnin käytännön, valitse "tatti" kyselyt voidaan served tehokkaasti hakemiston kautta. 

Monikulmion argumenttien ST_WITHIN voivat sisältää vain yhden soi, eli monikulmio ei saa olla niihin ratkaisuja. 

**Kyselyn**

    SELECT * 
    FROM Families f 
    WHERE ST_WITHIN(f.location, {
        'type':'Polygon', 
        'coordinates': [[[31.8, -5], [32, -5], [32, -4.7], [31.8, -4.7], [31.8, -5]]]
    })

**Tulokset**

    [{
      "id": "WakefieldFamily",
    }]
    
>[AZURE.NOTE] Miten ristiriitaiset DocumentDB kyselyssä, jos sijainti-arvon määritetty argumentti on Vääränlainen tai virheellinen ja valitse se arvioi **Määrittämätön** ja laskea asiakirjan ohitetaan kyselyn tuloksista tyypit toimii samalla. Kysely palauttaa tuloksia, suorita ST_ISVALIDDETAILED, virheenkorjaus miksi spatail tyyppi on virheellinen.     

DocumentDB tukee myös suorittamiseen käänteisen kyselyt, eli voi indeksoida monikulmio tai DocumentDB rivit ja alueet, jotka sisältävät tietyn kyselyn. Tätä mallia käytetään tavallisesti logistiikka, esimerkiksi kun Auto Lisää tai jättää nimetyn alueen. 

**Kyselyn**

    SELECT * 
    FROM Areas a 
    WHERE ST_WITHIN({'type': 'Point', 'coordinates':[31.9, -4.8]}, a.location)


**Tulokset**

    [{
      "id": "MyDesignatedLocation",
      "location": {
        "type":"Polygon", 
        "coordinates": [[[31.8, -5], [32, -5], [32, -4.7], [31.8, -4.7], [31.8, -5]]]
      }
    }]

ST_ISVALID ja ST_ISVALIDDETAILED voidaan tarkistaa, onko paikkatietojen objektin kelvollinen. Esimerkiksi seuraava kysely tarkistaa pisteen poissa-alueen leveyttä arvon (-132.8) kanssa. ST_ISVALID palauttaa vain totuusarvon ja ST_ISVALIDDETAILED palauttaa Boolean ja merkkijonon, joka sisältää syy, miksi viestiä pidetään virheellinen.

**Kyselyn**

    SELECT ST_ISVALID({ "type": "Point", "coordinates": [31.9, -132.8] })

**Tulokset**

    [{
      "$1": false
    }]

Näiden funktioiden avulla voidaan myös tarkistaa monikulmio. Esimerkiksi seuraavat Käytämme ST_ISVALIDDETAILED vahvistamiseen monikulmion, joka ei ole suljettu. 

**Kyselyn**

    SELECT ST_ISVALIDDETAILED({ "type": "Polygon", "coordinates": [[ 
        [ 31.8, -5 ], [ 31.8, -4.7 ], [ 32, -4.7 ], [ 32, -5 ] 
        ]]})

**Tulokset**

    [{
       "$1": { 
          "valid": false, 
          "reason": "The Polygon input is not valid because the start and end points of the ring number 1 are not the same. Each ring of a polygon must have the same start and end points." 
        }
    }]
    
### <a name="linq-querying-in-the-net-sdk"></a>LINQ .NET SDK kyselyt

DocumentDB .NET SDK myös tarjoajat kanta menetelmiä `Distance()` ja `Within()` LINQ lausekkeiden käytettäväksi. DocumentDB LINQ palveluntarjoajan kääntää menetelmä puhelut vastaava SQL-valmiin funktion puhelut (ST_DISTANCE ja ST_WITHIN tarpeen mukaan). 

Tässä on esimerkki LINQ kysely, joka etsii kaikki asiakirjat-"sijainnin" jonka arvo on 30 kilometrin on määritetty sisällä Valitse käyttämällä LINQ DocumentDB kokoelman.

**Etäisyys LINQ kysely**

    foreach (UserProfile user in client.CreateDocumentQuery<UserProfile>(UriFactory.CreateDocumentCollectionUri("db", "profiles"))
        .Where(u => u.ProfileType == "Public" && a.Location.Distance(new Point(32.33, -4.66)) < 30000))
    {
        Console.WriteLine("\t" + user);
    }

Seuraavassa on vastaavasti kyselyn kaikki asiakirjojen, jonka "sijainti" on määritetty ruutuun/monikulmio. 

**LINQ kyselyn sisällä**

    Polygon rectangularArea = new Polygon(
        new[]
        {
            new LinearRing(new [] {
                new Position(31.8, -5),
                new Position(32, -5),
                new Position(32, -4.7),
                new Position(31.8, -4.7),
                new Position(31.8, -5)
            })
        });

    foreach (UserProfile user in client.CreateDocumentQuery<UserProfile>(UriFactory.CreateDocumentCollectionUri("db", "profiles"))
        .Where(a => a.Location.Within(rectangularArea)))
    {
        Console.WriteLine("\t" + user);
    }


Kun on kulunut LINQ ja SQL-tiedostojen kyselyjen tekeminen katsaus, voit asetusten määrittäminen DocumentDB paikkatietojen indeksoinnin.

## <a name="indexing"></a>Indeksointi

Olemme ohjeiden mukaisesti [Rakenteen ympäristöstä riippumattomalla tavalla indeksoinnin ja Azure DocumentDB](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf) paperin, on suunniteltu DocumentDB's tietokantamoduuli on todella rakenteen ympäristöstä riippumattomalla tavalla ja tukea ensimmäisen luokan JSON. DocumentDB optimoitu kirjoitus-tietokantamoduuli ymmärtää grafiikkatiedostomuotoja edustaa GeoJSON-standardin paikkatietojen tiedot (pistettä, monikulmio ja rivit).

Sanottuna geometria on suunniteltu geodeettisen koordinaattien sivulle 2D tasossa sitten jaettu asteittain soluja käyttämällä **quadtree**. Nämä solut on yhdistetty 1D **Hilbertin tilaa täyttämisen käyrän**, joka säilyttää paikka asioista solua sijainnin perusteella. Lisäksi, kun paikkatietojen on indeksoitu, se käy läpi eli **tessellation**, eli kaikki solut, jotka Leikkaa sijainti tunnistetaan ja tallennettujen näppäimet DocumentDB hakemistoon. Kyselyn milloin argumentit, kuten pisteitä ja monikulmio ovat myös tessellated voit poimia haluamasi solualueiden tunnus ja valitse indeksi tietojen hakemiseen käytetty.

Jos määrität indeksoinnin käytännön, joka sisältää paikkatietojen indeksin / * (kaikki polut), valitse löydy kokoelmassa pisteiden indeksoidaan tehokas paikkatietojen kyselyjen (ST_WITHIN ja ST_DISTANCE). Paikkatietojen indeksejä ei ole tarkkuus-arvon ja Käytä aina tarkkuus oletusarvo.

>[AZURE.NOTE] DocumentDB tukee automaattisen indeksoinnin pistettä, monikulmio ja LineStrings

Seuraavat JSON koodikatkelman näkyy indeksoinnin käytännön siten, että paikkatietojen indeksointi käyttöön, indeksi eli GeoJSON tahansa löydetty asiakirjojen paikkatietojen kyselyt. Jos olet muokkaamassa indeksoinnin käytännön Azure-portaalissa, voit määrittää seuraavat JSON indeksoinnin käytännön käyttöön paikkatietojen indeksoinnin kokoelmasta.

**Sivustokokoelman indeksoinnin käytännön JSON pistettä ja monikulmio Spatial kanssa**

    {
       "automatic":true,
       "indexingMode":"Consistent",
       "includedPaths":[
          {
             "path":"/*",
             "indexes":[
                {
                   "kind":"Range",
                   "dataType":"String",
                   "precision":-1
                },
                {
                   "kind":"Range",
                   "dataType":"Number",
                   "precision":-1
                },
                {
                   "kind":"Spatial",
                   "dataType":"Point"
                },
                {
                   "kind":"Spatial",
                   "dataType":"Polygon"
                }                
             ]
          }
       ],
       "excludedPaths":[
       ]
    }

Seuraavassa on koodikatkelman .NET, jossa kerrotaan, miten voit luoda kokoelman kaikki polut, jossa pisteet käytössä paikkatietojen indeksoinnin avulla. 

**Luo kokoelma paikkatietojen indeksoinnin avulla**

    DocumentCollection spatialData = new DocumentCollection()
    spatialData.IndexingPolicy = new IndexingPolicy(new SpatialIndex(DataType.Point)); //override to turn spatial on by default
    collection = await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), spatialData);

Ja näin voit muokata aiemmin sivustokokoelmassa hyödyntää paikkatietojen indeksoinnin pisteitä, jotka on tallennettu asiakirjojen päälle.

**Muokkaa aiemmin sivustokokoelmassa paikkatietojen indeksoinnin avulla**

    Console.WriteLine("Updating collection with spatial indexing enabled in indexing policy...");
    collection.IndexingPolicy = new IndexingPolicy(new SpatialIndex(DataType.Point));
    await client.ReplaceDocumentCollectionAsync(collection);

    Console.WriteLine("Waiting for indexing to complete...");
    long indexTransformationProgress = 0;
    while (indexTransformationProgress < 100)
    {
        ResourceResponse<DocumentCollection> response = await client.ReadDocumentCollectionAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"));
        indexTransformationProgress = response.IndexTransformationProgress;

        await Task.Delay(TimeSpan.FromSeconds(1));
    }

> [AZURE.NOTE] Jos sijainti asiakirjassa GeoJSON arvo on virheellinen tai on virheellinen, valitse se indeksiin paikkatietojen kyselyitä varten. Voit tarkistaa ST_ISVALID ja ST_ISVALIDDETAILED sijainti-arvoja.
>
> Jos sivustokokoelman määritys sisältyy osio-näppäintä, muunnos käynnissä indeksointi ei ole raportoitu. 

## <a name="next-steps"></a>Seuraavat vaiheet
Nyt kun olet vältetyistä käyttämisestä aloittaminen geospatiaalisia tuki DocumentDB, voit tehdä seuraavaa:

- Käynnistä [Geospatiaalisia .NET MALLIKOODEJA-Github](https://github.com/Azure/azure-documentdb-dotnet/blob/fcf23d134fc5019397dcf7ab97d8d6456cd94820/samples/code-samples/Geospatial/Program.cs) -koodaus
- Hanki kädet kanssa geospatiaalisia kyselyt [DocumentDB kyselyn leikkikenttä](http://www.documentdb.com/sql/demo#geospatial) etsiminen
- Lisätietoja [DocumentDB kysely](documentdb-sql-query.md)
- Lisätietoja [DocumentDB indeksoinnin käytännöt](documentdb-indexing-policies.md)
