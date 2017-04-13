<properties 
    pageTitle="Azure SQL-tietokannan muodostaminen Azure hakutuloksia hyödyntämällä Indeksoijilla | Microsoft Azure | Indeksoijilla" 
    description="Opettele hakea tietoa Azure SQL-tietokanta käyttämällä Indeksoijilla Azure hakuindeksin." 
    services="search" 
    documentationCenter="" 
    authors="chaosrealm" 
    manager="pablocas" 
    editor=""/>

<tags 
    ms.service="search" 
    ms.devlang="rest-api" 
    ms.workload="search" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.date="05/26/2016" 
    ms.author="eugenesh"/>

#<a name="connecting-azure-sql-database-to-azure-search-using-indexers"></a>Yhteyden muodostaminen Azure hakutuloksia hyödyntämällä Indeksoijilla Azure SQL-tietokanta

Azure Search-palvelu on isännöityä cloud search-palvelun, joka on helppo antamaan hyvien hakukokemusta. Ennen kuin voit hakea, tarvitset Azure haun indeksi on tietojen täyttämiseen. Jos tiedot sijaitsevat Azure SQL-tietokantaan, uusi **Azure haun indeksointitoiminto Azure SQL-tietokantaan** (tai **Azure SQL-indeksointitoiminto** käyttämistä lyhyen) Azure hakutoiminnossa automatisoida indeksointi. Tämä tarkoittaa sitä, sinulla on vähemmän koodin kirjoittaminen ja vähemmän infrastruktuurin tärkeisiin.

Tällä hetkellä Indeksoijilla toimivat vain Azure SQL-tietokanta, SQL Server Azure VMs ja [Azure DocumentDB](../documentdb/documentdb-search-indexer.md). Tässä artikkelissa tarkastellaan Indeksoijilla, jotka toimivat Azure SQL-tietokantaan. Jos haluat nähdä tuki Lisää tietolähteitä, Anna palautetta käyttämiseen [Azure haun palautetta keskustelupalstalle](https://feedback.azure.com/forums/263029-azure-search/).

Tässä artikkelissa kattavat suunnittelu, jossa käytetään Indeksoijilla, mutta Olemme myös siirtyä alirakenteeseen ominaisuuksia ja toimintoja, jotka ovat käytettävissä SQL-tietokannat (esimerkiksi integroitu muutosten jäljityksen).

## <a name="indexers-and-data-sources"></a>Indeksoijilla ja tietolähteiden

Voit määrittää ja Määritä Azure SQL-indeksointitoiminto, voit soittaa [Azure-haun REST-Ohjelmointirajapinta](http://go.microsoft.com/fwlink/p/?LinkID=528173) , voit luoda ja hallita **Indeksoijilla** ja **tietolähteiden**. 

Voit käyttää myös [indeksointitoiminto luokan](https://msdn.microsoft.com/library/azure/microsoft.azure.search.models.indexer.aspx) [.NET SDK](https://msdn.microsoft.com/library/azure/dn951165.aspx)tai ohjattu tietojen tuominen [Azure portal](https://portal.azure.com) luominen ja ajoittaminen indeksointitoiminto.

**Tietolähteen** määrittää, mitä tietoja haluat indeksoida, käyttää tietoja ja käytäntöjä, jotka tehokkaasti tiedot (uusi, muokata ja poistaa rivejä) tehdyt muutokset tunnistetaan Azure haku on otettu käyttöön tarvittavat tunnistetiedot. Se on määritetty riippumaton resurssiksi niin, että se voidaan käyttää useita Indeksoijilla.

**Indeksointitoiminto** on resurssi, joka yhdistää tietolähteitä kohde hakuindeksin. Indeksointitoiminto käytetään seuraavilla tavoilla:
 
- Voit suorittaa tietojen täyttämiseen indeksin erikseen kopion.
- Päivitä hakemiston aikataulun tietolähteen muutokset.
- Suorita ohjattu hakemisto päivitetään tarpeen mukaan. 

## <a name="when-to-use-azure-sql-indexer"></a>Azure SQL-indeksointitoiminto käyttäminen

Useiden tekijöiden liittyvät tiedot, mukaan Azure SQL-indeksointitoiminto käyttö voi tai ei ehkä sovellu kaikkiin. Jos tietojen mahtuu seuraavat vaatimukset, voit käyttää Azure SQL-indeksointitoiminto: 

- Kaikki tiedot ovat peräisin yhdestä taulukosta tai näkymän
    - Jos tiedot on ripoteltuna useita taulukoita, voit näkymän luominen ja käyttäminen indeksointitoiminto näkymässä. Muista kuitenkin, että jos käytät näkymän, et voi käyttää SQL Server integroitu muutoksen tunnistus. Katso lisätietoja tässä osassa. 
- Tietolähteen käytetyt tietotyypit tukevat indeksointitoiminto. Useimmissa, mutta ei kaikkiin, SQL tuetut tietotyypit. Lisätietoja on artikkelissa [yhdistettyjen Azure haun tietotyypit](http://go.microsoft.com/fwlink/p/?LinkID=528105). 
- Ei tarvitse lähellä indeksin, kun rivi muuttuu reaaliaikainen päivitykset. 
    - Indeksointitoiminto uudelleen voi indeksoida taulukon enintään viiteen minuuttiin. Tarvittaessa tiedot muuttuvat usein ja muutokset näkyvät myös hakemiston sekuntia tai yksittäisen minuutin kuluessa Suosittelemme [Azure haun indeksi-Ohjelmointirajapinnan](https://msdn.microsoft.com/library/azure/dn798930.aspx) käyttäminen suoraan. 
- Jos käytössäsi on suuri tietojoukon ja aiot Suorita aikataulun rakenteen indeksointitoiminto pystyy tunnistavan tehokkaasti muuttaa (ja poistaa, jos sellainen on) rivit. Lisätietoja on artikkelissa "Sieppaus muuttaa ja poistaa rivejä" alapuolelle. 
- Indeksoidut kentät rivin kokoa ei ole suurempi kuin indeksoinnin pyynnön, joka on 16 Megatavua Azure-haun enimmäiskoon. 

## <a name="create-and-use-an-azure-sql-indexer"></a>Luominen ja käyttäminen Azure SQL-indeksointitoiminto

Luo tietolähde: 

    POST https://myservice.search.windows.net/datasources?api-version=2015-02-28 
    Content-Type: application/json
    api-key: admin-key
    
    { 
        "name" : "myazuresqldatasource",
        "type" : "azuresql",
        "credentials" : { "connectionString" : "Server=tcp:<your server>.database.windows.net,1433;Database=<your database>;User ID=<your user name>;Password=<your password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30;" },
        "container" : { "name" : "name of the table or view that you want to index" }
    }


Saat yhteysmerkkijonoa [Azure perinteinen Portal](https://portal.azure.com); Käytä `ADO.NET connection string` vaihtoehto.

Luo sitten kohde Azure hakuindeksin, jos sitä ei vielä ole. Voit tehdä tämän [portal Käyttöliittymän](https://portal.azure.com) tai [Luoda hakemiston API](https://msdn.microsoft.com/library/azure/dn798941.aspx)käyttämällä.  Varmista, että kohde indeksi rakenne on lähdetaulukon rakenteen kanssa yhteensopivat – Katso lisätietoja [SQL: n ja Azure haun tietotyypin määrittäminen välillä](#TypeMapping) .

Luo lopuksi indeksointitoiminto Anna sen nimi ja viittaava lähde- ja hakemisto:

    POST https://myservice.search.windows.net/indexers?api-version=2015-02-28 
    Content-Type: application/json
    api-key: admin-key
    
    { 
        "name" : "myindexer",
        "dataSourceName" : "myazuresqldatasource",
        "targetIndexName" : "target index name"
    }

Luotu tällä tavalla indeksointitoiminto ei ole aikataulun. Se käynnistyy automaattisesti kerran heti, kun se on luotu. Voit avata sen uudelleen milloin tahansa **indeksointitoiminto suoritetaan** pyyntö avulla:

    POST https://myservice.search.windows.net/indexers/myindexer/run?api-version=2015-02-28 
    api-key: admin-key

Voit mukauttaa eri ominaisuuksia indeksointitoiminto toimintaa, kuten erän koko ja kuinka monta asiakirjoja voidaan ohittaa ennen indeksointitoiminto suorittamisen epäonnistuu. Lisätietoja on artikkelissa [Luo indeksointitoiminto API](https://msdn.microsoft.com/library/azure/dn946899.aspx).
 
Voit joutua sallimaan Azure-palveluilla voit muodostaa yhteys tietokantaan. Saat ohjeet tehdä [Yhteyden Azure](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure) .

Indeksointitoiminnon tila ja suorittamisen historia (Indeksoitu kohteiden määrää, virheet jne.), käytä **indeksointitoiminnon tila** -pyyntö: 

    GET https://myservice.search.windows.net/indexers/myindexer/status?api-version=2015-02-28 
    api-key: admin-key

Vastauksen pitäisi näyttää seuraavankaltaiselta: 

    {
        "@odata.context":"https://myservice.search.windows.net/$metadata#Microsoft.Azure.Search.V2015_02_28.IndexerExecutionInfo",
        "status":"running",
        "lastResult": {
            "status":"success",
            "errorMessage":null,
            "startTime":"2015-02-21T00:23:24.957Z",
            "endTime":"2015-02-21T00:36:47.752Z",
            "errors":[],
            "itemsProcessed":1599501,
            "itemsFailed":0,
            "initialTrackingState":null,
            "finalTrackingState":null 
        },
        "executionHistory":
        [
            {
                "status":"success",
                "errorMessage":null,
                "startTime":"2015-02-21T00:23:24.957Z",
                "endTime":"2015-02-21T00:36:47.752Z",
                "errors":[],
                "itemsProcessed":1599501,
                "itemsFailed":0,
                "initialTrackingState":null,
                "finalTrackingState":null 
            },
            ... earlier history items 
        ]
    }

Suorittamisen historia sisältää enintään 50 viimeksi valmiit suorituskerran, joka on lajiteltu käänteisessä aikajärjestyksessä (niin, että uusin suorittamisen on ensin vastauksessa). Lisätietoja vastauksen löytyvät [Hae indeksointitoiminnon tila](http://go.microsoft.com/fwlink/p/?LinkId=528198)

## <a name="run-indexers-on-a-schedule"></a>Suorita Indeksoijilla aikataulun

Voit myös järjestää indeksointitoiminto suorittaa toistu säännöllisesti. Voit tehdä tämän Lisää **aikataulu** -ominaisuuteen, kun luominen tai päivittäminen indeksointitoiminto. Alla olevassa esimerkissä HYLLYTETTY pyyntö indeksointitoiminto päivittäminen:

    PUT https://myservice.search.windows.net/indexers/myindexer?api-version=2015-02-28 
    Content-Type: application/json
    api-key: admin-key 

    { 
        "dataSourceName" : "myazuresqldatasource",
        "targetIndexName" : "target index name",
        "schedule" : { "interval" : "PT10M", "startTime" : "2015-01-01T00:00:00Z" }
    }

**Aikaväli** -parametri on pakollinen. Aikavälin viittaa kaksi peräkkäistä indeksointitoiminto suorituskerran alkuun välisen ajan. Pienin sallittu aikaväli on 5 minuuttia. Pisin haara on yksi päivä. Se on muotoiltu XSD "dayTimeDuration" arvo (rajoitettu alijoukko [ISO 8601 kesto](http://www.w3.org/TR/xmlschema11-2/#dayTimeDuration) -arvo). Tämä kaava on: `P(nD)(T(nH)(nM))`. Esimerkkejä: `PT15M` 15 minuutin välein `PT2H` , kahden tunnin välein.

Valinnainen **aloitusajan** ilmaisee, kun ajoitetun suorituskerran kannattaa aloittaa; Jos sitä määritetä, käytetään nykyisen UTC-aika. Tällä hetkellä voi olla aiemman – jossa kirjainkoko ensimmäinen suoritus ajoitetaan ikään indeksointitoiminto on ollut käynnissä jatkuvasti aloitusajan jälkeen.  

Vain yksi suorittamisen annetun indeksointitoiminto voidaan suorittaa kerrallaan. Jos indeksointitoiminto suoritetaan jo, kun seuraavaan katsomaan Käynnistä-painiketta, suorittamisen ohitetaan ja seuraavan väliajoin olettaen, että muut indeksointitoiminto työhön ansioluettelot on käynnissä.

Seuraavassa esimerkki, jos haluat tehdä lisää betonin. Oletetaan, että on määritetty seuraavan tunnin välein aikataulun: 

    "schedule" : { "interval" : "PT1H", "startTime" : "2015-03-01T00:00:00Z" }

Tapahtuu seuraavaa: 

1. Ensimmäinen indeksointitoiminto suoritus käynnistyy, tai 1 maaliskuussa 2015 kello ympärille UTC-AIKA.
1. Oletetaan, että suorittaminen kestää 20 minuutin ajan (tai aina pienempi kuin 1 tunti).
1. Toinen suoritus käynnistyy, tai 1 maaliskuussa 2015 1 kello ympärille 
1. Oletetaan, että suorittaminen kestää yli tunnin (tämä edellyttäisi on erittäin suuri määrä tämän yksittäiseksi todella asiakirjoja, mutta se on hyödyllinen kuvassa) – esimerkiksi 70 minuuttia – niin, että se on valmis noin 2:10 kello
1. On nyt 2 kello, aikaa Aloita kolmannen suorittamista varten. Koska toinen suorittamisen-1 kello on käynnissä, kolmas suorittamisen ohitetaan. Kolmas suoritus käynnistyy 3 kello

Voit lisätä, muuttaa tai poistaa aiemmin indeksointitoiminto aikataulun avulla **SIJOITTAA indeksointitoiminto** pyynnön. 

## <a name="capturing-new-changed-and-deleted-rows"></a>Tallentaa uudet, muuttaa ja poistaa rivejä

Jos käytät aikataulun ja taulukko sisältää-trivial rivimäärän, käytettävä tietojen muuttaminen tunnistus käytännön, niin, että indeksointitoiminto tehokkaasti voit hakea vain uusia tai muutettuja rivejä eikä sinun tarvitse indeksoi koko taulukon.

### <a name="sql-integrated-change-tracking-policy"></a>SQL integroitu seuraamisen muuttaminen

Jos SQL-tietokanta tukee [muutosten seuranta](https://msdn.microsoft.com/library/bb933875.aspx), on suositeltavaa käyttää **SQL integroitu muuttaminen seuraamisen**. Tämän käytännön avulla tehokkaasti muutosten jäljityksen ja se sallii Azure haun tunnistavan poistetut rivit eikä sinun tarvitse eksplisiittinen "Pehmeä Poista"-sarakkeen lisääminen taulukkoon.

Integroitu muutosten jäljityksen tuetaan käynnistäminen SQL Server-tietokanta-versioiden kanssa:
 
- SQL Server 2008 R2 ja uudempi versio, jos käytät SQL Server Azure VMs. 
- Azure SQL tietokanta Vipuventtiili V12, jos käytät Azure SQL-tietokantaan.

Kun käyttämällä integroitu SQL muuta seuraamisen, Määritä erillinen tietojen poisto tunnistus käytännön - käytäntö on tukee yksilöimiseen poistaa rivejä.

Tämän käytännön voidaan käyttää vain taulukoihin. se ei voi käyttää näkymiä. Sinun on otettava käyttöön muutosten jäljitys on käytössä, ennen kuin voit käyttää tätä käytäntöä taulukon. Saat ohjeet [käyttöön ja poistaminen käytöstä muutosten seuranta](https://msdn.microsoft.com/library/bb964713.aspx) . 

Jos haluat käyttää tämän käytännön, luominen tai päivittäminen tietolähteen tältä:
 
    { 
        "name" : "myazuresqldatasource",
        "type" : "azuresql",
        "credentials" : { "connectionString" : "connection string" },
        "container" : { "name" : "table or view name" }, 
        "dataChangeDetectionPolicy" : {
           "@odata.type" : "#Microsoft.Azure.Search.SqlIntegratedChangeTrackingPolicy" 
      }
    }

### <a name="high-water-mark-change-detection-policy"></a>Suuri veden Merkitse muutoksen tunnistus käytäntö

Kun SQL integroitu muutosten jäljityksen käytäntö on suositeltavaa, et voi käyttää sitä, jos tiedot ovat näkymässä tai jos käytössäsi on vanhempi versio, Azure SQL-tietokantaan. Siinä tapauksessa kannattaa käyttää suuri veden Merkitse käytännön. Tämän käytännön avulla voidaan, jos taulukossa on sarake, joka täyttää seuraavat ehdot:

- Kaikki Lisää Määritä sarakkeen arvo. 
- Kohteen kaikki päivitykset myös muuttaa sarakkeen arvo. 
- Tämän sarakkeen arvo kasvaa jokaisen muutoksen kanssa.
- Kyselyitä, jotka käyttävät `WHERE` lauseen samalla `WHERE [High Water Mark Column] > [Current High Water Mark Value]` voidaan toteuttaa tehokkaammin.

Esimerkiksi indeksoidut **rowversion** -sarake on paras mahdollinen candidate, suuri veden Merkitse-sarakkeen. Jos haluat käyttää tämän käytännön, luominen tai päivittäminen tietolähteen tältä: 

    { 
        "name" : "myazuresqldatasource",
        "type" : "azuresql",
        "credentials" : { "connectionString" : "connection string" },
        "container" : { "name" : "table or view name" }, 
        "dataChangeDetectionPolicy" : {
           "@odata.type" : "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
           "highWaterMarkColumnName" : "[a row version or last_updated column name]" 
      }
    }

### <a name="soft-delete-column-deletion-detection-policy"></a>Pehmeä poistaa sarakkeen poistaminen tunnistus käytäntö

Kun rivit on poistettu lähdetaulukosta, haluat todennäköisesti poistaa hakuindeksin myös ne rivit. Jos käytät integroitu SQL-muutosten seuraamisen, tämä on otettava hoitamaan puolestasi. Kuitenkin seuraamisen suuri veden Merkitse muutos ei auttavat poistetut rivit. Mitä voi tehdä? 

Jos rivit fyysisesti poistetaan taulukosta tykö poissaolostasi – tällä ei voi johtaa siihen, onko tietueisiin, joita ei enää ole.  Voit kuitenkin Merkitse riviä kuin loogisesti poistetut ilman poistamalla ne taulukosta lisäämällä sarakkeen ja rivien merkitseminen merkki arvon käyttäminen sarakkeen poistettu "Poista Pehmeät"-tekniikka.

Pehmeä Poista tekniikka käytettäessä voit määrittää Pehmeät Poista käytäntö seuraavasti luotaessa tai tietolähteen päivittäminen: 

    { 
        …, 
        "dataDeletionDetectionPolicy" : { 
           "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
           "softDeleteColumnName" : "[a column name]", 
           "softDeleteMarkerValue" : "[the value that indicates that a row is deleted]" 
        }
    }

Huomaa, että **softDeleteMarkerValue** on oltava merkkijono – Käytä todellinen arvo string-esitys. Jos sinulla on kokonaisluku-sarake, jossa poistetut rivit on merkitty arvon 1, käyttää esimerkiksi `"1"`; Jos sinulla on bittinen-sarake, mihin poistetut rivit on merkitty totuusarvon TOSI, `"True"`. 

<a name="TypeMapping"></a>
## <a name="mapping-between-sql-data-types-and-azure-search-data-types"></a>Yhdistäminen välillä Azure haun tietotyypit ja SQL-tietotyypit

|SQL-tietotyyppi | Sallittu kohde indeksin kenttien lajit |Huomautuksia 
|------|-----|----|
|Bit|Edm.Boolean, Edm.String| |
|INT, smallint, tinyint |Edm.Int32, Edm.Int64, Edm.String| |
| bigint | Edm.Int64, Edm.String | |
| reaali-liukuluku |Edm.Double, Edm.String | |
| Smallmoney rahaa desimaaleja numeerinen | Edm.String| Azure haku ei tue desimaaleja tyypit muuntaminen Edm.Double, koska tämä menettävät tarkkuus |
| merkki, nchar, varchar nvarchar | Edm.String<br/>Collection(EDM.String)|Muodonmuutoksen merkkijono-sarakkeen siirtäminen Collection(Edm.String) edellyttää Esikatsele API versio 2015-02-28-esikatselu. Lisätietoja [tämän artikkelin](search-api-indexers-2015-02-28-preview.md#CreateIndexer) tiedot| 
|smalldatetime-, päivämäärä ja aika-, datetime2-, päivämäärä-, DateTimeOffset-arvoa |Edm.DateTimeOffset, Edm.String| |
|uniqueidentifer | Edm.String | |
|Geography | Edm.GeographyPoint | Tuetaan vain geography esiintymät tyyppi tuominen kohtaa SRID 4326 (joka on myös oletus) | | 
|ROWVERSION| PUUTTUU |Rivi-version sarakkeita ei voi tallentaa hakuindeksin, mutta niitä voidaan käyttää muutosten seuranta | |
| aika, aikajakson, binaarinen, varbinary, kuva, xml, geometria, CLR-tietotyypit | PUUTTUU |Ei tueta |


## <a name="frequently-asked-questions"></a>Usein kysytyt kysymykset

**Q:** Azure SQL-indeksointitoiminto Käytä SQL-tietokantoja käytössä IaaS VMs Azure-tietokannassa

V: Kyllä. Sinun on sallittava search-palvelun Muodosta yhteys tietokantaan tekemällä seuraavat kaksi asiaa. Lue lisätietoja artikkelista [Azure haku-indeksointitoiminto SQL Server Azure-AM-yhteyden määrittäminen](search-howto-connecting-azure-sql-iaas-to-azure-search-using-indexers.md) .

1. Joudut ehkä määrittää tietokannan luotettu varmenteen niin, että hakupalvelun voi avata tietokantaa SSL-yhteyksiä.
2. Määritä palomuuri annettavien search-palvelun IP-osoite.

**Q:** Voinko käyttää Azure SQL-indeksointitoiminto käynnissä paikallisen SQL-tietokantoja? 

A: Microsoft ei ole suositeltavaa tai tukevat, tekoa edellyttäisi sinua pyydetään avaamaan tietokannoistasi Internet-liikenne. 

**Q:** Azure SQL-indeksointitoiminto käyttää tietokantoja, joka SQL Server Azure IaaS käytössä 

A: ei tueta tässä skenaariossa koska emme ole testanneet indeksointitoiminto tietokantoja kuin SQL Serverin kanssa.  

**Q:** Voit luoda useita Indeksoijilla aikataulun käytössä? 

V: Kyllä. Kuitenkin vain yksi indeksointitoiminto voi olla käynnissä yhtä solmun samalla kertaa. Jos tarvitset useita Indeksoijilla käynnissä samanaikaisesti, kannattaa harkita skaalaus search-palvelun useita haun kokonaislukuun ylöspäin. 

**Q:** Käytössä indeksointitoiminto vaikuttavat kysely-työmäärää? 

V: Kyllä. Indeksointitoiminto suoritetaan johonkin search-palvelun solmujen ja kyseisen solmun resurssit ovat yhteisiä indeksoinnin ja kohdistetussa kyselyn liikenne ja muut API-pyynnöt. Jos suoritat tehostettu indeksoinnin ja kyselyjen toiminnoista ja suuri korvaus 503 virheitä tai kasvava vastauksen kellonaikoja ilmenee, harkitse skaalaus search-palvelun määrittäminen.
