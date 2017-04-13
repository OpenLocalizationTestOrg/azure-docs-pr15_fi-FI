<properties 
    pageTitle="Johdanto DocumentDB, JSON tietokannan | Microsoft Azure" 
    description="Lisätietoja Azure DocumentDB NoSQL JSON-tietokantaan. Tämä asiakirja-tietokanta on luotu big datasta ja joustavasti skaalattavuus suuren käytettävyyden." 
    keywords="JSON-tietokantaan, asiakirjan tietokanta"
    services="documentdb" 
    authors="mimig1" 
    manager="jhubbard" 
    editor="monicar" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="09/13/2016" 
    ms.author="mimig"/>

# <a name="introduction-to-documentdb-a-nosql-json-database"></a>Johdanto DocumentDB: NoSQL JSON-tietokanta

##<a name="what-is-documentdb"></a>Mikä on DocumentDB?

DocumentDB on täysin hallitun NoSQL tietokannan palvelu laadittuihin ratkaisuihin nopea ja ennakoitavissa suorituskyvyn, suuri käytettävyys, joustavasti skaalaus, yleinen jakelu ja ohjelmoinnin helpottamiseksi. NoSQL rakenteen vapaa-tietokantana DocumentDB tarjoaa monipuolisen ja tuttuja SQL kyselyn ominaisuuksia yhdenmukaisia pienen viiveitä JSON tietojen - varmistaa, että lukee 99 % avautuvat alle 10 millisekuntia ja että kirjoituksia 99 % avautuvat kohdassa 15 millisekuntia. Nämä yksilöllinen edut Tee DocumentDB hienoa Sovita Web-mobiili-gaming, ja IoT ja monet muut sovellukset, jotka tarvitsevat saumaton asteikko ja yleisen replikoinnin.

## <a name="how-can-i-learn-about-documentdb"></a>Miten DocumentDB voit tutustua? 

Tietoja DocumentDB ja katso, miten sitä käytetään nopeasti on kolme seuraavasti: 

1. Katso kahden minuutin [DocumentDB ominaisuudet?](https://azure.microsoft.com/documentation/videos/what-is-azure-documentdb/) video, jossa esitellään DocumentDB eduista.
2. Katso kolme minuutit [Luominen DocumentDB Azure-](https://azure.microsoft.com/documentation/videos/create-documentdb-on-azure/) video, joka korostaa voit aloittaa DocumentDB Azure-portaalissa.
3. Seuraavassa [Kyselyn leikkikenttä](http://www.documentdb.com/sql/demo), jossa voi käydä läpi eri toiminnoista, saat lisätietoja käytettävissä olevista DocumentDB monipuolisia kyseltäessä toiminnoista. Sitten head päälle eristetyn-välilehteen ja suorita omia mukautettuja SQL-kyselyjä ja kokeilla DocumentDB.

Valitse palaa tämän artikkelin kohtaa, johon on tarkastella tarkemmin –.  

## <a name="what-capabilities-and-key-features-does-documentdb-offer"></a>Mitä ominaisuuksia ja ominaisuuksia DocumentDB tarjoavat?  

Azure DocumentDB tarjoaa seuraavia keskeisiä ominaisuuksia ja edut:

-   **Elastically skaalattava siirtonopeuden ja tallennustilaa:** Helposti skaalata tai skaalata DocumentDB JSON tietokannan sovelluksen omien tarpeiden alaspäin. Tiedot tallennetaan pienen ennakoitavissa viiveitä levyjen tasainen tila (Suoritettaessa). DocumentDB tukee säilöjen kutsutaan sivustokokoelmat, joka voi skaalata lähes rajoittamattoman tallennustilan koko ja valmistellun siirtonopeuden JSON tietojen tallentamista varten. Voit elastically skaalata DocumentDB ennakoitavissa suorituskyky saumattomasti kun sovelluksen kasvaa. 

-   **Monille replikoinnin:** DocumentDB kopioi tietojen läpinäkyvä kaikki alueet, joita olet liittyvä DocumentDB-tiliä, voit kehittää sovelluksia, jotka edellyttävät tietojen yleisen käytön käyttämisessä kompromissien yhdenmukaisuuden, käytettävyys ja suorituskyvyn, kaikki vastaavat oikeudet kanssa välillä. DocumentDB on läpinäkyvä alueellisen automaattisesti usean homing ohjelmointirajapinnan kanssa ja voi skaalata elastically yli maapallon siirtonopeuden ja tallennustilaa. Lue lisää tekstimuodossa [jakaa tietojasi yleisesti DocumentDB](documentdb-distribute-data-globally.md).

-   **Itsenäisten kyselyjen ja tuttuja SQL-syntaksi:** Erilaisten DocumentDB sisällä JSON tiedostojen tallentaminen ja kyselyjen tiedostot tuttuja SQL-syntaksi kautta. DocumentDB käyttää vapaa erittäin samanaikainen lukituksen, loki rakenteellinen indeksoida automaattisesti asiakirjan koko sisällön indeksoinnin tekniikka. Näin monipuolisia reaaliaikainen kyselyjen ilman, että voit määrittää rakenteen vihjeitä, toissijaisia indeksejä tai näkymiä. Lisätietoja [Kyselyn DocumentDB](documentdb-sql-query.md). 

-   **JavaScript suorittamisen tietokannasta:** Voit ilmaista sovelluksen logiikkaa tallennettujen toimintosarjojen, käynnistimien ja käyttäjän määrittämät funktiot (UDF) käyttämällä vakio JavaScript. Näin sovelluksen-logiikkaa toimimaan ilman sovelluksen ja tietokantarakenteen toisiaan katkeamisesta tietojen päälle. DocumentDB on JavaScript-sovelluksen logiikkaa suoraan tietokantamoduuli sisällä täydellistä tapahtumien suorittamisen. Laaja JavaScript-integrointia ottaa käyttöön lisää, korvaa, poista ja valitse Toiminnot JavaScript-ohjelmassa suorittamisen erillään tapahtumana. Lue lisää [DocumentDB palvelinpuolen](documentdb-programming.md)ohjelmoinnin.

-   **Tunable yhdenmukaisuuden tasot:** Valitse määritetty yhdenmukaisuuden neljä tasoa optimaalisen trade-off välillä yhdenmukaisuutta ja suorituskyvyn saavuttamiseksi. Kyselyjen ja lue toimintojen DocumentDB on neljä erillistä yhdenmukaisuuden tasoa: vahva, joka-staleness-istunnon ja potentiaalisen. Hajautetun, hyvin määritetyn yhdenmukaisuuden nämä tasot avulla voit tehdä äänen valinnat yhdenmukaisuuden, käytettävyys ja viive välillä. Lisätietoja [yhdenmukaisuuden tasot, jos haluat suurentaa saatavuudesta ja suorituskyvyn DocumentDB avulla](documentdb-consistency-levels.md).

-   **Täysin hallitun:** Poista tietokannan ja tietokoneen resurssien ei tarvitse. Microsoft Azure täysin hallitun-palvelu ei tarvitse hallita näennäiskoneiden, käyttöönotto ja ohjelman asetukset määritetään, hallita skaalaus tai käsitellä monimutkaisten tietojen tason päivitykset. Jokaisella tietokannan varmuuskopioida ja suojattu alueellisen virheet automaattisesti. Voit lisätä DocumentDB tilin ja valmistella kapasiteettia, kun tarvitset, joilla voit keskittyä sovelluksesi sen sijaan, että liiketoiminnan ja tietokannan hallinta. 

-   **Avaa rakenteen mukaan:** Pääset alkuun nopeasti käyttämällä vanhoja taitojasi ja työkalut. Ohjelmoinnin vastaan DocumentDB on yksinkertainen, approachable ja ei edellytä voit antaa uusia työkaluja tai mukautettuja laajennuksia JSON tai JavaScript noudattaa. Voit käyttää kaikkia tietokannan toimintoja, esimerkiksi CRUD kyselyn ja JavaScript käsittelyn yksinkertainen RESTful HTTP-liittymän kautta. DocumentDB yleisstrategian valmiita muotoja, kielet ja standardit silti suuren arvon tietokannan ominaisuuksien niiden päälle.

-   **Automaattisen indeksoinnin:** Oletusarvoisesti DocumentDB [Indeksoi automaattisesti](documentdb-indexing.md) tietokannan kaikki asiakirjat ja ei odota tai edellytä rakennetta tai toissijaisen indeksien luominen. Etkö halua indeksoimaan kaikkia kohteita? Ei hätää, voit [osallistua polut JSON tiedostojen ulos](documentdb-indexing-policies.md) -painiketta liian.

##<a name="data-management"></a>Miten DocumentDB hallita tiedot?

Azure DocumentDB hallitsee JSON tietojen hyvin määritetyn tietokannan resurssit. Nämä resurssit suuren replikoida ja ovat käytettävissä yksilöllisesti niiden loogisen URI. DocumentDB on yksinkertainen HTTP-pohjaista RESTful ohjelmoinnin mallin kaikille resursseille. 

DocumentDB tietokannan tili on yksilöllinen nimitilan, jonka avulla voit käyttää Azure DocumentDB. Ennen kuin voit luoda tietokantatilin, tarvitset Azure tilaus, jonka kautta pääset käsiksi erilaisia Azure palveluja. 

Kaikki resurssit DocumentDB sisällä mallintaa ja JSON-tiedostoina tallennettuja. Resurssien hallinnan kohteet, jotka ovat JSON asiakirjojen sisältävän metatietojen ja kuin syötteet on kohdekokoelmia. Kohdejoukkojen sisältyvät vastaaviin syötteitä.

Alla olevassa kuvassa näkyy DocumentDB resurssien väliset suhteet:

![DocumentDB, NoSQL JSON tietokannan resurssien hierarkkista suhteen][1] 

Tietokannan tilin koostuu joukosta tietokantojen kussakin useita sivustokokoelmat, joihin voi olla tallennettujen toimintosarjojen, käynnistimien, UDF, tiedostoja ja Aiheeseen liittyvät liitteet. Tietokannan on liitetty myös käyttäjille, kunkin ja määrittää käyttöoikeuksia eri muiden sivustokokoelmat tallennettujen toimintosarjojen, käynnistimien, UDF, asiakirjat ja liitteet. Kun tietokantoja, käyttäjät, käyttöoikeudet ja sivustokokoelmat ovat tunnetun rakenteita järjestelmän resursseja – asiakirjoja, tallennettujen toimintosarjojen, käynnistimien, UDF ja liitteitä sisältävät haluamaansa, käyttäjän määrittämä JSON sisältöä.  

##<a name="develop"></a>Miten sovellusten DocumentDB kanssa voit kehittää?

Azure DocumentDB paljastaa resursseja kautta REST-Ohjelmointirajapinta, joka voidaan kutsua minkä tahansa kielen voi tehdä HTTP/HTTPS-pyyntöjen perusteella. Lisäksi DocumentDB on Suositut kielet-ohjelmoinnin kirjastoissa. Nämä kirjastot yksinkertaistaa monia käsitteleminen Azure DocumentDB sisällyttämällä tiedot, kuten osoite välimuistiin, poikkeuksen hallinta, automaattinen uudelleenyritykset ja niin edelleen. Kirjastojen ovat tällä hetkellä käytettävissä seuraavat kielet ja ympäristöissä:  

Lataa | Ohjeet
--- | ---
[.NET SDK-PAKETISSA](http://go.microsoft.com/fwlink/?LinkID=402989) | [.NET-kirjasto](https://msdn.microsoft.com/library/azure/dn948556.aspx)
[Node.js SDK-paketissa](http://go.microsoft.com/fwlink/?LinkID=402990) | [Node.js kirjastoon](http://azure.github.io/azure-documentdb-node/)
[Java SDK-paketissa](http://go.microsoft.com/fwlink/?LinkID=402380) | [Java-kirjasto](http://azure.github.io/azure-documentdb-java/)
[JavaScript-SDK](http://go.microsoft.com/fwlink/?LinkID=402991) | [JavaScript-kirjasto](http://azure.github.io/azure-documentdb-js/)
puuttuu | [Palvelinpuolen JavaScript SDK-paketissa](http://azure.github.io/azure-documentdb-js-server/)
[Python SDK-paketissa](https://pypi.python.org/pypi/pydocumentdb) | [Python kirjastoon](http://azure.github.io/azure-documentdb-python/)

Muutakin kuin tavallinen luominen, lukeminen, päivittäminen ja poistaminen toimintoja, DocumentDB tarjoaa monipuolisia SQL-kysely-liittymän haetaan JSON asiakirjat ja palvelinpuolen tukea JavaScript-sovelluksen logiikkaa tapahtumien suorittamisen. Kyselyn ja komentosarjojen suorittamisen liittymät ovat saatavilla kaikkiin ympäristö kirjastoihin sekä REST API. 

### <a name="sql-query"></a>SQL-kysely
Azure DocumentDB tukee tiedostojen SQL-kielen, joka sijaitsee JavaScript tyyppi järjestelmän ja lausekkeiden tukeen relaatio, hierarkkinen ja paikkatietojen kyselyt kyselyt. DocumentDB-kyselykieltä on kyselyn JSON asiakirjojen yksinkertaisen mutta tehokkaan käyttöliittymä. Kieli tukee alijoukkoa ANSI SQL-kieliopin ja Lisää JavaScript-objekti, matriisin, objektin muodostaminen ja funktion kutsu laaja integrointi. DocumentDB on kyselyn mallin ilman mitään eksplisiittinen rakenteen tai indeksoinnin vihjeitä kehittäjä.

Käyttäjän määrittämät funktiot (UDF) voit rekisteröidä DocumentDB ja viitata osana SQL-kyselyn laajentaminen siten tukemaan mukautetun sovelluksen Logiikan tarkistus. Nämä UDF kirjoitettu JavaScript-ohjelmat ja suorittaa tietokannasta. 

.NET-kehittäjille DocumentDB on LINQ kyselyn palveluntarjoaja [.NET SDK](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.linq.aspx)osana. 

### <a name="transactions-and-javascript-execution"></a>Tapahtumien ja JavaScript-suorittaminen
DocumentDB voit kirjoittaa sovelluksen logiikkaa nimellä nimetyt kokonaan JavaScript-ohjelmia. Näissä ohjelmissa kokoelman rekisteröity ja voi myöntää tietokannan tiedostojen tietyn sivustokokoelman toimintoja. JavaScript voidaan rekisteröidä suorittamisen käynnistin, tallennetun toimintosarjan tai käyttäjän määrittämä funktio. Käynnistimien ja tallennettujen toimintosarjojen voit luoda, lukea, päivittää ja tiedostojen poistaminen olisi käyttäjän määrittämien funktioiden suorittaminen kyselyn suorituksen logiikan ilman kirjoitusoikeutta kokoelmaan osana.

JavaScript-suorittamisen sisällä DocumentDB perustuu relaatiotietokannasta järjestelmien kanssa JavaScript kuin Transact-SQL Moderni korvaa tukemat käsitteitä. Kaikki JavaScript-logiikkaa suoritetaan ympäröivä HAPPAMAN tapahtumasta tilannevedoksen eristystaso kanssa. Sen suorittamisen aikana, jos JavaScript-koodia ilmoittaa poikkeuksen, valitse koko tapahtuma on keskeytetty.

## <a name="next-steps"></a>Seuraavat vaiheet
Onko sinulla jo Azure-tili? Sitten voit voivat aloittaa DocumentDB [Azure Portal](https://portal.azure.com/#gallery/Microsoft.DocumentDB) luomalla [DocumentDB tietokanta-tili](documentdb-create-account.md).

Ei ole Azure-tili? Voit:

- Tilaa [Azure maksuttoman kokeiluversion](https://azure.microsoft.com/free/), jonka avulla voit 30 päivää ja 200 Kokeile Azure palvelut. 
- Jos sinulla on MSDN-tilaus, voit soveltuvat [150-vapaa Azure hyvitykset kuukaudessa](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) käyttämään Azure-palvelusta. 

Kun olet valmis, jos haluat lisätietoja, käy Microsoftin [oppimispolku](https://azure.microsoft.com/documentation/learning-paths/documentdb/) siirtyä käytettävissäsi oppimisresurssit. 


[1]: ./media/documentdb-introduction/json-database-resources1.png
 
