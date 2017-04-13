<properties 
    pageTitle="Order By DocumentDB tietojen lajittelu | Microsoft Azure" 
    description="Katso, miten käyttämään ORDER BY LINQ ja SQL-kyselyissä DocumentDB ja indeksoinnin käytäntö, ORDER BY kyselyjen määrittäminen." 
    services="documentdb" 
    authors="arramac" 
    manager="jhubbard" 
    editor="cgronlun" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/03/2016" 
    ms.author="arramac"/>

# <a name="sorting-documentdb-data-using-order-by"></a>Order By DocumentDB tietojen lajitteleminen
Microsoft Azure DocumentDB tukee kyseltäessä asiakirjojen SQL välityksellä JSON asiakirjoja. Kyselytulosten voi järjestää ORDER BY-lauseen käyttäminen kyselyn SQL-lauseita.

Luettuasi tämän artikkelin pystyt seuraaviin kysymyksiin: 

- Miten tilaus by kyselyn?
- Miten saat Order By indeksoinnin käytäntö määritetään?
- Mitä on tulossa seuraava?

[Esimerkkejä, joiden](#samples) ja [usein kysytyt kysymykset](#faq) myös toimitetaan.

Katso täydellinen viittaus-SQL-kysely- [DocumentDB kyselyn opetusohjelma](documentdb-sql-query.md).

## <a name="how-to-query-with-order-by"></a>Kysely, joka sisältää Order By poistamisesta
Kuten ANSI-SQL-voit nyt lisätä valinnainen Order By-lauseen-SQL-lauseet kun kysely suoritetaan DocumentDB. Lause voi sisältää valinnainen Nouseva tai LASKEVA-argumentti, voit määrittää järjestyksen, jossa tulokset on noudettu. 

### <a name="ordering-using-sql"></a>Järjestys käyttämällä SQL
Seuraavassa on esimerkki kysely, joka noutaa laskevassa järjestyksessä otsikoiden top 10-kirjat. 

    SELECT TOP 10 * 
    FROM Books 
    ORDER BY Books.Title DESC

### <a name="ordering-using-sql-with-filtering"></a>Järjestys SQL käyttäminen suodattaminen
Voit tilata tahansa sisäkkäisiä ominaisuuden asiakirjojen, kuten Books.ShippingDetails.Weight ja voit määrittää Lisäsuodattimet Order By yhdessä WHERE-lauseen, kuten tässä esimerkissä:

    SELECT * 
    FROM Books 
    WHERE Books.SalePrice > 4000
    ORDER BY Books.ShippingDetails.Weight

### <a name="ordering-using-the-linq-provider-for-net"></a>Järjestys .NET LINQ-palvelun avulla
Käyttämällä .NET SDK versio 1.2.0 ja uudemmassa versiossa voit käyttää OrderBy() tai OrderByDescending()-lauseen sisällä LINQ kyselyt, myös tässä esimerkissä:

    foreach (Book book in client.CreateDocumentQuery<Book>(UriFactory.CreateDocumentCollectionUri("db", "books"))
        .OrderBy(b => b.PublishTimestamp)
        .Take(100))
    {
        // Iterate through books
    }

DocumentDB tukee järjestys yksittäisen numeerinen, merkkijono tai totuusarvo kysely-ominaisuus kyselyn käyttämällä tulossa pian. Katso [mitä on tulossa seuraava](#Whats_coming_next) lisätietoja.

## <a name="configure-an-indexing-policy-for-order-by"></a>Indeksoinnin käytännön määrittäminen Lajittelu

Et voi peruuttaa DocumentDB tukee kahdenlaisia indeksit (Hash ja alue), jossa voit määrittää tietyn polkujen ja ominaisuudet-tietotyypit (merkkijonoja tai numerot) ja eri arvot (suurin desimaalien määrä tai kiinteä tarkkuusarvo). Koska DocumentDB Hash indeksoinnin oletukseksi, sinun on luotava uusi sivustokokoelman käytännöllä mukautetun indeksoinnin alue lukuja, merkkijonoja tai molemmat, voi käyttää Order By. 

>[AZURE.NOTE] Merkkijono alueen indeksit otettiin käyttöön 7. heinäkuussa 2015 REST-ohjelmointirajapinnalla versio 2015-06-03. Jotta he voivat luoda käytäntöjä saat Order By vastaan merkkijonoja, sinun on käytettävä SDK versio 1.2.0 .NET SDK tai versio 1.1.0 Python, Node.js tai Java SDK-paketissa.
>
>REST API versio 2015-06-03 ennen sivustokokoelman indeksoinnin oletuskäytäntö on Hash merkkijonot ja lukujen. Tämä on vaihdettu Hash merkkijonot ja alueen lukujen. 

Saat lisätietoja [DocumentDB indeksoinnin käytännöt](documentdb-indexing-policies.md).

### <a name="indexing-for-order-by-against-all-properties"></a>Indeksoinnin vastaan kaikki ominaisuudet lajittelu
Seuraavalla siitä, miten voit luoda kokoelman kanssa "Kaikki alue" indeksoinnin Order By vastaan tahansa ja kaikki numeeriset tai merkkijono, jotka esiintyvät JSON asiakirjojen sen sisältämät ominaisuudet. Seuraavassa on ohittaa oletuslajin indeksi merkkijonoarvoa alue- ja suurin desimaalien määrä (-1).
                   
    DocumentCollection books = new DocumentCollection();
    books.Id = "books";
    books.IndexingPolicy = new IndexingPolicy(new RangeIndex(DataType.String) { Precision = -1 });
    
    await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), books);  

>[AZURE.NOTE] Huomaa, että Order By vain palauttaa tuloksia tietotyyppien (merkkijono ja numero), joka on indeksoitu RangeIndex kanssa. Esimerkiksi jos indeksoinnin käytännön, joka on vain numerot RangeIndex oletusarvo on Order By vastaan merkkijonoarvoa polku palauttaa asiakirjoja ei.

### <a name="indexing-for-order-by-for-a-single-property"></a>Indeksoinnin yhden ominaisuuden lajittelu
Seuraavassa on siitä, miten voit luoda kokoelma indeksoinnin vastaan vain otsikko-ominaisuutta, joka on merkkijonon Order By. On kaksi polkua, yksi otsikko-ominaisuutta ("/ otsikko /?") alueen indeksoinnin ja muut jokaisen ominaisuuden oletusarvon indeksoinnin järjestelmän, joka on Hash merkkijonot ja alueen lukujen kanssa.                    
    
    booksCollection.IndexingPolicy.IncludedPaths.Add(
        new IncludedPath { 
            Path = "/Title/?", 
            Indexes = new Collection<Index> { 
                new RangeIndex(DataType.String) { Precision = -1 } } 
            });
    
    await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), booksCollection);  


## <a name="samples"></a>Esimerkkejä, joiden
Tutustu tämän [Github näytteiden projekti](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples/Queries) , joka näytetään, miten tilaus-toiminnon, kuten luominen indeksoinnin käytännöt ja sivutus järjestyksessä käyttämällä. Mallit ovat Avaa lähde ja on suositeltavaa lähettää salaus puretaan pyyntöjä maksuja, joka voi hyödyntää muiden DocumentDB kehittäjät kanssa. Lue ohjeita siitä, miten voit osallistua [osuus ohjeita](https://github.com/Azure/azure-documentdb-net/blob/master/Contributing.md) .  

## <a name="faq"></a>USEIN KYSYTYT KYSYMYKSET

**Odotettu pyytää yksikkö (RU) kulutus Order By kyselyt-ominaisuudet**

Koska Order By käyttämällä haut DocumentDB indeksi-pyyntö yksiköiden Order By kyselyjen Kulutettu määrä on samanlainen vastaavat kyselyihin ilman Order By. Muut laskutoimituksen DocumentDB, kuten pyynnön yksiköiden määrän, riippuu siitä, että tiedostojen koot/muotoja sekä kyselyn monimutkaisuutta. 


**Mitä odotettavissa indeksoinnin yleisrasite Order By?**

Indeksoinnin tallennustilan katseltavan on suhteutettu ominaisuuksien määrä. Huonoin tapaus skenaario indeksi katseltavan on 100 %: n tiedot. Ei ole siirtonopeuden (pyytää yksiköt) katseltavan alue/Order By indeksoinnin ja oletusarvo-Hash-indeksoinnin välinen ero.

**Miten Omat tiedot järjestyksessä käyttämällä DocumentDB kyselyn?**

Jotta voit lajitella kyselytulosten käyttäminen Order By, sinun on muokattava kokoelman käyttää alueen indeksi vastaan ominaisuus, jota käytetään lajittelemiseen indeksoinnin käytännön. Katso [muokkaaminen indeksoinnin käytännön](documentdb-indexing-policies.md#modifying-the-indexing-policy-of-a-collection). 

**Mitä rajoituksia nykyisen Order By?**

Order By voidaan määrittää vain vastaan ominaisuutta, joko numeerinen tai merkkijono, kun se on alueen indeksoitu suurin tarkkuudella (-1).

Et voi suorittaa seuraavasti:
 
- Order By sisäinen merkkijono-ominaisuudet, kuten tunnuksen, _rid ja _self (tulossa).
- Order By johdettu tulos (tulossa) sisäisessä asiakirjan-liitoksen ominaisuudet.
- Lajittelu useita ominaisuuksia (tulossa).
- Order By kyselyjen tietokantoja, sivustokokoelmat, käyttäjät, käyttöoikeudet tai liitteet (tulossa).
- Order By laskettu ominaisuuksissa esimerkiksi tulos lausekkeen tai UDF/sisäänrakennettu--funktiota.

Order By ei tällä hetkellä tueta rajat-osion kyselyjen käytettäessä kyselyn Explorer Azure-portaalissa.

## <a name="troubleshooting"></a>Vianmääritys

Jos saat virheilmoituksen, että Order By ei tueta Tarkista varmistaaksesi, että käytät [SDK-paketissa](documentdb-sdk-dotnet.md) , joka tukee Order By versio. 

## <a name="next-steps"></a>Seuraavat vaiheet

Haarautuvat [Github näytteiden project](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples/Queries) ja Käynnistä järjestys tietojen! 

## <a name="references"></a>Viittaukset
* [DocumentDB kyselyn viittaus](documentdb-sql-query.md)
* [DocumentDB indeksoinnin käytännön viitata](documentdb-indexing-policies.md)
* [DocumentDB SQL-viittaus](https://msdn.microsoft.com/library/azure/dn782250.aspx)
* [Esimerkkejä, joiden tilauksen DocumentDB](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples/Queries)
 

