<properties 
pageTitle="Indeksointitoiminto toiminnot (Azure Search-palvelun REST-Ohjelmointirajapinnalla: 2015-02-28-esikatselu) | Azure Search-esikatselu Ohjelmointirajapinta" 
description="Indeksointitoiminto toiminnot (Azure Search-palvelun REST-Ohjelmointirajapinnalla: 2015-02-28-esikatselu)" 
services="search" 
documentationCenter="" 
authors="chaosrealm" 
manager="pablocas"
editor="" />

<tags 
ms.service="search" 
ms.devlang="rest-api" 
ms.workload="search" 
ms.topic="article"  
ms.tgt_pltfrm="na" 
ms.date="09/07/2016" 
ms.author="eugenesh" />

#<a name="indexer-operations-azure-search-service-rest-api-2015-02-28-preview"></a>Indeksointitoiminto toiminnot (Azure Search-palvelun REST-Ohjelmointirajapinnalla: 2015-02-28-esikatselu)#

> [AZURE.NOTE] Tässä artikkelissa kuvataan [2015-02-28-esikatselu REST API](search-api-2015-02-28-preview.md)Indeksoijia. Tämä API-versio lisää esikatselu-versioiden kanssa asiakirjan poiminnan indeksointitoiminto Azure-Blob-säiliö ja Azure-taulukkotallennus indeksointitoiminto sekä muita parannuksia. Ohjelmointirajapinnan tukee myös yleisesti saatavilla (GA) Indeksoijilla, mukaan lukien Indeksoijilla Azure SQL-tietokanta, SQL Server Azure VMs ja Azure DocumentDB.

## <a name="overview"></a>Yleiskatsaus ##

Azure haku voidaan yhdistää suoraan yleisiä tietolähteitä ei tarvitse kirjoittaa koodia indeksoida tiedot poistetaan. Jos haluat määrittää tämän, voit soittaa Azure haun Ohjelmointirajapinnan luomisen ja hallinnan **Indeksoijilla** ja **tietolähteiden**. 

**Indeksointitoiminto** on resurssi, joka yhdistää tietolähteitä kohde hakuindeksin. Indeksointitoiminto käytetään seuraavilla tavoilla: 

- Voit suorittaa tietojen täyttämiseen indeksin erikseen kopion.
- Synkronoi indeksin aikataulun tietolähteen muutokset. Aikataulun on osa indeksointitoiminto-määritys.
- Käynnistää pyydettäessä hakemisto päivitetään tarpeen mukaan. 

**Indeksointitoiminto** on hyötyä, kun haluat indeksin päivitykset säännöllisesti. Voit määrittää tekstiin-aikataulun osana indeksointitoiminto määritys tai suorittaa pyydettäessä [Suorita indeksointitoiminto](#RunIndexer)avulla. 

**Tietolähteen** määrittää, mitkä tiedot on indeksoitava, tunnistetiedot, joita käyttää tietoja ja käytännöt tehokkaasti tiedot (kuten muokata tai poistaa tietokannan taulukon rivien) tehdyt muutokset tunnistetaan Azure haku on otettu käyttöön. Se on määritetty riippumaton resurssiksi niin, että se voidaan käyttää useita Indeksoijilla.

Tällä hetkellä tueta seuraaviin tietolähteisiin:

- **Azure SQL-tietokanta** ja **SQL Server Azure VMs**. Katso [tämän artikkelin](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers-2015-02-28.md)kohdennettujen hallintapaketteihin. 
- **Azure DocumentDB**. Katso [tämän artikkelin](../documentdb/documentdb-search-indexer.md)kohdennettujen hallintapaketteihin. 
- **Azure-Blob-säiliö**, mukaan lukien seuraavia tiedostomuotoja: PDF, Microsoft Office (DOCX/asiakirjaan, XSLX/XLS, PPTX/PPT, viesti), HTML, XML, POSTINUMERO ja vain teksti tiedostot (mukaan lukien JSON). Katso [tämän artikkelin](search-howto-indexing-azure-blob-storage.md)kohdennettujen hallintapaketteihin.
- **Azure-taulukkotallennus**. Katso [tämän artikkelin](search-howto-indexing-azure-tables.md)kohdennettujen hallintapaketteihin.
     
Olemme ovat lähitulevaisuudessa lisääminen Lisää tietolähteitä tuesta myöhemmin. Auta meitä priorisoida päätökset, anna palaute käyttämiseen [Azure haun palautetta keskustelupalstalle](http://feedback.azure.com/forums/263029-azure-search).

Katso [Palvelun rajoitukset](search-limits-quotas-capacity.md) suurin indeksointitoiminto ja tietojen lähteen resursseihin liittyviä rajoituksia.

## <a name="typical-usage-flow"></a>Tyypillinen käyttö työnkulku ##

Voit luoda ja hallita Indeksoijilla ja tietolähteiden yksinkertainen pyyntöjen (POST, GET-HYLLYTETTY, poista) kautta vastaan tietyn `data source` tai `indexer` resurssi.

Automaattisen indeksoinnin määrittäminen on yleensä neljä vaihetta:

1. Määritä tietolähde, joka sisältää tiedot, jotka on indeksoitava. Ota huomioon, että Azure haun eivät tue kaikkien ole tietolähteessä tietotyypit. Katso [tuetut tietotyypit](https://msdn.microsoft.com/library/azure/dn798938.aspx) luettelossa.

2. Luo indeksi Azure haun, jonka rakenne on yhteensopiva tietolähde.
  
3. Azure haku-tietolähteen luominen, [Luo tietolähde](#CreateDataSource)kuvatulla tavalla.
  
4. Luo Azure haun indeksointitoiminto kuvattu [Luominen indeksointitoiminto](#CreateIndexer).

Kannattaa suunnitella yhden indeksointitoiminto jokaisen kohteen indeksi ja tietojen lähteen yhdistelmän luomisesta. Sinulla voi olla useita Indeksoijilla kirjoittaminen kyselyjä sama indeksi ja voit käyttää useita Indeksoijilla saman tietolähde. Kuitenkin indeksointitoiminto kerrallaan vain voi käyttää yksi tietolähde ja voivat kirjoittaa vain yhden indeksi. 

Kun olet luonut indeksointitoiminto, voit hakea [Hae indeksointitoiminnon tila](#GetIndexerStatus) -toiminnon suorittaminen sen tila. Voit suorittaa milloin tahansa (sijaan tai suorittamalla se säännöllisesti aikataulun) indeksointitoiminto [Suoritetaan indeksointitoiminto](#RunIndexer) -toiminnon.

<!-- MSDN has 2 art files plus a API topic link list -->


## <a name="create-data-source"></a>Tietolähteen luominen ##

Azure hakutoiminnossa tietolähteen käyttämisestä Indeksoijilla, tarjoavat kohde indeksiä luonnoslehtiössä tai ajoitettu tietojen päivitys yhteystiedot. Voit luoda uuden tietolähteen käyttämällä HTTP POST-pyynnön Azure Search-palvelun.
    
    POST https://[service name].search.windows.net/datasources?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

Voit vaihtoehtoisesti käyttää HYLLYTETTY ja Määritä tietolähteen nimi URI. Jos tietolähde ei ole, se luodaan.

    PUT https://[service name].search.windows.net/datasources/[datasource name]?api-version=[api-version]

**Huomautus**: tietolähteiden enimmäismäärä sallittu vaihtelee hinnoittelu taso. Vapaa-palvelun avulla enintään 3 tietolähteet. Vakio-palvelun avulla 50 tietolähteet. Katso lisätietoja [Palvelun rajoitukset](search-limits-quotas-capacity.md) .

**Pyyntö**

HTTPS tarvitaan kaikkia palvelupyyntöjä. **Luo tietolähde** -pyyntö on rakennettava viestin tai HYLLYTETTY-menetelmällä. Kun käytät viestin, sinun on määritettävä tietolähteen nimi sekä tietolähteen määrityksen pyynnön tekstissä. HYLLYTETTY, jonka nimi on URL-Osoitteen osa. Jos tietolähde ei ole valittu, se luodaan. Jos se on jo olemassa, se päivitetään uuden määritelmän. 

Tietolähteen nimi on kirjoitettava pienillä kirjaimilla, aloittaminen kirjaimen tai numeron, ei ole vinoviivaa tai pistettä ja voi olla pienempi kuin 128: aa merkkiä. Käynnistyksen tietolähteen nimen kanssa kirjaimen tai numeron jälkeen loput nimi voi sisältää kirjain, numero ja katkoviivat, kunhan viivat eivät ole peräkkäisiä. Lisätietoja on kohdassa [Naming säännöt](https://msdn.microsoft.com/library/azure/dn857353.aspx) .

`api-version` Tarvitaan. Nykyistä versiota ei `2015-02-28`.

**Pyydä otsikot**

Seuraavassa luettelossa kuvataan pakolliset ja valinnaiset pyynnön otsikot. 

- `Content-Type`: Pakollinen. Määritä tähän`application/json`
- `api-key`: Pakollinen. `api-key` Todennetaan Search-palvelun pyynnön. On merkkijonoarvo, yksilöllisiä palvelussa. **Luo tietolähde** -pyyntö on sisällettävä `api-key` otsikon määrittää järjestelmänvalvojan key-tuotetunnuksen (eikä kyselyn avain). 
 
Sinun on myös palvelunimi muodostaa pyyntö URL-osoite. Saat sekä palvelunimi ja `api-key` service-koontinäytössä [Azure-portaalissa](https://portal.azure.com/). Katso Ohje sivuissa Siirtyminen [Luo hakupalvelun portaalissa](search-create-service-portal.md) .

<a name="CreateDataSourceRequestSyntax"></a>
**Leipäteksti syntaksi**

Pyynnön leipätekstissä tietojen lähteen määritys, joka sisältää tietolähdetyyppi, jos haluat nähdä tiedot sekä valinnainen tietojen muuttaminen tunnistaminen ja tietojen poisto tunnistus käytäntöjä, jotka tunnistetaan tehokkaasti muuttaa tai poistaa säännöllisesti ajoitetun indeksointitoiminto käytettäessä tietolähteen tunnistetiedot. 

Pyyntö paketti jäsentämiseen syntaksi on seuraava. Esimerkki pyyntö on annettu edelleen artikkelista.

    { 
        "name" : "Required for POST, optional for PUT. The name of the data source",
        "description" : "Optional. Anything you want, or nothing at all",
        "type" : "Required. Must be one of 'azuresql', 'documentdb', 'azureblob', or 'azuretable'",
        "credentials" : { "connectionString" : "Required. Connection string for your data source" },
        "container" : { "name" : "Required. The name of the table, collection, or blob container you wish to index" },
        "dataChangeDetectionPolicy" : { Optional. See below for details }, 
        "dataDeletionDetectionPolicy" : { Optional. See below for details }
    }

Pyyntö sisältää seuraavat ominaisuudet: 

- `name`: Pakollinen. Tietolähteen nimi. Tietolähteen nimi on vain sisältävät pieniä kirjaimia, numeroita tai katkoviivat, voi aloittaa tai perässä viivoja ja 128: aa merkkiä.
- `description`: Valinnainen kuvaus. 
- `type`: Pakollinen. Edellyttää, että jokin tuetut tietotyypit lähteestä:
    - `azuresql`-Azure SQL-tietokanta tai SQL Server Azure VMs
    - `documentdb`-Azure DocumentDB
    - `azureblob`-Azure-Blob-säiliö
    - `azuretable`-Azure-taulukkotallennus
- `credentials`:
    - Pakollinen `connectionString` ominaisuus määrittää tietolähteen yhteysmerkkijono. Yhteysmerkkijonon muoto määräytyy tietolähteen tyyppi: 
        - Azure SQL-tämä on tavallista yhteysmerkkijono. Jos käytät Azure portaalin noutaminen yhteysmerkkijono `ADO.NET connection string` vaihtoehto.
        - DocumentDB, yhteysmerkkijono on oltava seuraavassa muodossa: `"AccountEndpoint=https://[your account name].documents.azure.com;AccountKey=[your account key];Database=[your database id]"`. Kaikki arvot ovat pakollisia. Voit hakea muistiinpanoja [Azure portal](https://portal.azure.com/).  
        - Azure-Blob-objektien ja taulukkotallennus tämä on tallennustilan tilin yhteysmerkkijonon. Muoto on kuvattu [seuraavassa](https://azure.microsoft.com/documentation/articles/storage-configure-connection-string/). Päätepisteen HTTPS-protokollan tarvitaan.  
- `container`, pakollinen: määrittää indeksoida käyttämällä tiedot `name` ja `query` ominaisuudet: 
    - `name`, pakollinen:
        - Azure SQL: määrittää taulukon tai näkymän. Voit käyttää rakenteen qualified nimet, kuten `[dbo].[mytable]`.
        - DocumentDB: määrittää kokoelmasta. 
        - Azure-Blob-säiliö: määrittää tallennustilan säilö.
        - Azure taulukkotallennus: määrittää taulukon nimi. 
    - `query`, valinnainen:
        - DocumentDB: Voit määrittää kyselyn, joka objektiksi haluamaansa JSON-asiakirjan asettelun tasainen rakenteen, joka Azure-haku voi indeksoida kyselyjä.  
        - Azure-Blob-säiliö: Voit määrittää näennäiskansio sisällä blob-säilö. Esimerkiksi blob polku `mycontainer/documents/blob.pdf`, `documents` voidaan käyttää virtuaalisen kansion.
        - Azure taulukkotallennus: Voit määrittää kyselyn, joka suodattaa Rivijoukko tuomista.
        - Azure SQL: kyselyä ei tueta. Jos tarvitset tätä toimintoa, äänestää Hyväksy [ehdotus](https://feedback.azure.com/forums/263029-azure-search/suggestions/9893490-support-user-provided-query-in-sql-indexer)
- Valinnainen `dataChangeDetectionPolicy` ja `dataDeletionDetectionPolicy` ominaisuudet ovat jäljempänä.

<a name="DataChangeDetectionPolicies"></a>
**Tietojen muuttaminen tunnistus käytännöt**

Tietojen muuttaminen tunnistus käytännön tarkoituksena on tunnistaa tehokkaasti muutetut tiedot kohteet. Tuetut käytännöt määräytyvät tietolähteen tyyppi. Seuraavissa osissa kuvataan kunkin käytännön. 

***Yläraja Muuta tunnistus käytäntö*** 

Käytä tämän käytännön, kun tietolähde sisältää sarakkeen tai ominaisuus, joka täyttää seuraavat ehdot:
 
- Kaikki Lisää Määritä sarakkeen arvo. 
- Kohteen kaikki päivitykset myös muuttaa sarakkeen arvo. 
- Tämän sarakkeen arvo kasvaa jokaisen muutoksen kanssa.
- Seuraavan kaltainen suodatinlausekkeen käyttävien kyselyjen `WHERE [High Water Mark Column] > [Current High Water Mark Value]` voidaan toteuttaa tehokkaammin.

Esimerkiksi Azure SQL tietolähteistä, indeksoitu käytettäessä `rowversion` sarake on paras mahdollinen candidate suuri veden Merkitse käytännön käytettäväksi. 

Tämän käytännön voidaan määrittää seuraavasti:

    { 
        "@odata.type" : "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
        "highWaterMarkColumnName" : "[a row version or last_updated column name]" 
    } 

> [AZURE.NOTE] Kun käytät DocumentDB tietolähteitä, sinun on käytettävä `_ts` DocumentDB myöntämä ominaisuus. 

> [AZURE.NOTE] Käytettäessä automaattisesti Azure-Blob-tietolähteistä, Azure haku käyttää hyvin vesileiman muuttaminen tunnistus käytäntö Blob-objektien on viimeksi muokattu aikaleima; perusteella ei tarvitse määrittää tällainen käytäntö.   

***SQL integroitu Muuta tunnistus käytäntö***

Jos SQL-tietokanta tukee [muutosten seuranta](https://msdn.microsoft.com/library/bb933875.aspx), on suositeltavaa käyttää SQL integroitu muuttaminen seuraamisen. Tämän käytännön avulla tehokkaasti muutosten jäljityksen ja tunnistavan poistetut rivit eikä sinun tarvitse olla eksplisiittinen "Pehmeä Poista"-sarake tai rakenteen Azure haun avulla.

Integroitu muutosten jäljityksen tuetaan käynnistäminen SQL Server-tietokanta-versioiden kanssa: 
- SQL Server 2008 R2, jos käytät SQL Server Azure VMs.
- Azure SQL tietokanta Vipuventtiili V12, jos käytät Azure SQL-tietokantaan.  

Kun SQL integroitu muutosten jäljityksen käytännön avulla et määritä erillinen tietojen poisto tunnistus käytännön - käytäntö on tukee yksilöimiseen poistaa rivejä. 

Tämän käytännön voidaan käyttää vain taulukoihin. se ei voi käyttää näkymiä. Sinun on otettava käyttöön muutosten jäljitys on käytössä, ennen kuin voit käyttää tätä käytäntöä taulukon. Saat ohjeet [käyttöön ja poistaminen käytöstä muutosten seuranta](https://msdn.microsoft.com/library/bb964713.aspx) .    
 
Kun rakenteen suunnitteluun **Luo tietolähde** pyynnön, integroitu SQL muuta seuranta-käytäntö voidaan määrittää seuraavasti:

    { 
        "@odata.type" : "#Microsoft.Azure.Search.SqlIntegratedChangeTrackingPolicy" 
    }

<a name="DataDeletionDetectionPolicies"></a>
**Tietojen poistaminen tunnistus käytännöt**

Tietojen poistaminen tunnistus käytännön tarkoituksena on tunnistaa tehokkaasti poistettujen tietojen kohteet. Ainoa tuettu käytäntöä ei tällä hetkellä `Soft Delete` käytännön, joka sallii tunnistaminen poistetut arvon perusteella `soft delete` sarakkeen tai tietolähde-ominaisuutta. Tämän käytännön voidaan määrittää seuraavasti:

    { 
        "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
        "softDeleteColumnName" : "the column that specifies whether a row was deleted", 
        "softDeleteMarkerValue" : "the value that identifies a row as deleted" 
    }

**Huomautus:** Tuetaan vain sarakkeet, joissa on merkkijono, kokonaisluku tai totuusarvoja. Arvo, jota käytetään kuin `softDeleteMarkerValue` on oltava merkkijono, vaikka vastaavan sarakkeen pitää kokonaisluvut tai totuusarvoja. Esimerkiksi jos tietolähteen oleva arvo on 1, käytä `"1"` nimellä `softDeleteMarkerValue`.    

<a name="CreateDataSourceRequestExamples"></a>
**Pyydä leipäteksti-esimerkkejä**

Jos aiot käyttää tietolähteen indeksointitoiminto, joka suoritetaan aikataulun, tässä esimerkissä näytetään muuttaminen ja poistaminen tunnistus käytäntöjen määrittäminen: 

    { 
        "name" : "asqldatasource",
        "description" : "a description",
        "type" : "azuresql",
        "credentials" : { "connectionString" : "Server=tcp:....database.windows.net,1433;Database=...;User ID=...;Password=...;Trusted_Connection=False;Encrypt=True;Connection Timeout=30;" },
        "container" : { "name" : "sometable" },
        "dataChangeDetectionPolicy" : { "@odata.type" : "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy", "highWaterMarkColumnName" : "RowVersion" }, 
        "dataDeletionDetectionPolicy" : { "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy", "softDeleteColumnName" : "IsDeleted", "softDeleteMarkerValue" : "true" }
    }

Jos haluat vain tietolähteen käyttäminen tietojen kopion käytännöt jätetään pois:

    { 
        "name" : "asqldatasource",
        "description" : "anything you want, or nothing at all",
        "type" : "azuresql",
        "credentials" : { "connectionString" : "Server=tcp:....database.windows.net,1433;Database=...;User ID=...;Password=...;Trusted_Connection=False;Encrypt=True;Connection Timeout=30;" },
        "container" : { "name" : "sometable" }
    } 

**Vastaus**

Onnistuneiden pyynnön: 201 luotu. 

<a name="UpdateDataSource"></a>
## <a name="update-data-source"></a>Tietolähteen päivittäminen ##

Voit päivittää aiemmin luotuun tietolähteeseen käyttämällä HTTP valitseminen pyynnön. Voit myös määrittää pyynnön URI Päivitä tietolähteen nimi:

    PUT https://[service name].search.windows.net/datasources/[datasource name]?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

`api-version` Tarvitaan. Nykyistä versiota ei `2015-02-28`. [Azure haun versiotiedot](https://msdn.microsoft.com/library/azure/dn864560.aspx) on tiedot ja lisätietoja vaihtoehtoisen versioista.

`api-key` On oltava järjestelmänvalvoja-näppäintä (eikä kyselyn avain). Lisätietoja [Haun palvelun REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx) lisätietoja näppäimet todennus-osassa. [Luo hakupalvelun portaalissa](search-create-service-portal.md) kerrotaan, miten palvelun URL-osoite ja pyynnön keskeisiä ominaisuuksia.

**Pyyntö**

Pyyntö leipätekstin syntaksi on sama kuin [Luo tietolähde pyynnöt](#CreateDataSourceRequestSyntax).

> [AZURE.NOTE] Joitakin ominaisuuksia ei voi päivittää aiemmin luotuun tietolähteeseen. Et voi muuttaa aiemmin luotuun tietolähteeseen tyyppi.  

> [AZURE.NOTE] Jos et halua muuttaa aiemmin luotuun tietolähteeseen yhteysmerkkijonon, voit määrittää literaalimerkit `<unchanged>` yhteysmerkkijonon varten. Tästä on hyötyä tilanteissa, jossa haluat päivittää tiedot tietolähteen, mutta pääse kätevästi käyttämään yhteysmerkkijonon, koska se on suojaukseen liittyviä tietoja.

**Vastaus**

Onnistuneiden pyynnön: 201 luotu Jos uusi tietolähde on luotu ja 204 ei sisältöä Jos aiemmin luotuun tietolähteeseen on päivitetty.

<a name="ListDataSource"></a>
## <a name="list-data-sources"></a>Luettele tietolähteet ##

**Luettele tietolähteet** -toiminto palauttaa Azure Search-palvelun tietolähteiden luettelo. 

    GET https://[service name].search.windows.net/datasources?api-version=[api-version]
    api-key: [admin key]

`api-version` Tarvitaan. Nykyistä versiota ei `2015-02-28`. 

`api-key` On oltava järjestelmänvalvoja-näppäintä (eikä kyselyn avain). Lisätietoja [Haun palvelun REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx) lisätietoja näppäimet todennus-osassa. [Luo hakupalvelun portaalissa](search-create-service-portal.md) kerrotaan, miten palvelun URL-osoite ja pyynnön keskeisiä ominaisuuksia.

**Vastaus**

Onnistuneiden pyynnön: 200 OK.

Tässä on esimerkki-vastauksen tekstissä:

    {
      "value" : [
        {
          "name": "datasource1",
          "type": "azuresql",
          ... other data source properties
        }]
    }

Huomaa, että voit suodattaa vastauksen alaspäin käyttämällä ominaisuudet. Esimerkiksi jos haluat vain luettelon tietolähteiden nimet, käytä OData `$select` kyselyn vaihtoehto:

    GET /datasources?api-version=205-02-28&$select=name

Tässä tapauksessa edellä olevassa esimerkissä vastaus näkyy seuraavasti: 

    {
      "value" : [ { "name": "datasource1" }, ... ]
    }

Tämä on hyödyllinen tapa tallentaa kaistanleveyden, jos sinulla on useita tietolähteitä Search-palvelun.

<a name="GetDataSource"></a>
## <a name="get-data-source"></a>Hae tietolähde ##

**Hae tietolähde** -toiminto saa tietolähteen määrityksen Azure haku.

    GET https://[service name].search.windows.net/datasources/[datasource name]?api-version=[api-version]
    api-key: [admin key]

`api-version` Tarvitaan. Nykyistä versiota ei `2015-02-28`. 

`api-key` On oltava järjestelmänvalvoja-näppäintä (eikä kyselyn avain). Lisätietoja [Haun palvelun REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx) lisätietoja näppäimet todennus-osassa. [Luo hakupalvelun portaalissa](search-create-service-portal.md) kerrotaan, miten palvelun URL-osoite ja pyynnön keskeisiä ominaisuuksia.

**Vastaus**

Tilakoodin: 200 OK palautetaan onnistuneen vastauksen odottaminen.

[Luo tietolähde Esimerkki pyynnöt](#CreateDataSourceRequestExamples)esimerkkejä on vastaus: 

    { 
        "name" : "asqldatasource",
        "description" : "a description",
        "type" : "azuresql",
        "credentials" : { "connectionString" : "Server=tcp:....database.windows.net,1433;Database=...;User ID=...;Password=...;Trusted_Connection=False;Encrypt=True;Connection Timeout=30;" },
        "container" : { "name" : "sometable" },
        "dataChangeDetectionPolicy" : { 
            "@odata.type" : "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
            "highWaterMarkColumnName" : "RowVersion" }, 
        "dataDeletionDetectionPolicy" : { 
            "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
            "softDeleteColumnName" : "IsDeleted", 
            "softDeleteMarkerValue" : "true" }
    }

> [AZURE.NOTE] Ei määritetä `Accept` pyynnön otsikko `application/json;odata.metadata=none` kun kutsutaan nimellä tällöin Tämä API aiheuttaa `@odata.type` määrite vastauksen ja pois voi erottaa tietojen muuttaminen ja tietojen poisto tunnistus käytännöt erityyppisiä. 

<a name="DeleteDataSource"></a>
## <a name="delete-data-source"></a>Poista tietolähde ##

**Poista tietolähde** -toiminto poistaa tietolähteen Azure Search-palvelun.

    DELETE https://[service name].search.windows.net/datasources/[datasource name]?api-version=[api-version]
    api-key: [admin key]

> [AZURE.NOTE] Jos kaikki Indeksoijilla viittaus tietolähteen, jossa poistettava; poistotoiminto edelleen Jatka. Kuitenkin näiden Indeksoijilla siirtymä virhe-tilaan, niiden seuraava asennuksen yhteydessä.  

`api-version` Tarvitaan. Nykyistä versiota ei `2015-02-28`. 

`api-key` On oltava järjestelmänvalvoja-näppäintä (eikä kyselyn avain). Lisätietoja [Haun palvelun REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx) lisätietoja näppäimet todennus-osassa. [Luo hakupalvelun portaalissa](search-create-service-portal.md) kerrotaan, miten palvelun URL-osoite ja pyynnön keskeisiä ominaisuuksia.

**Vastaus**

Tilakoodin: 204 ei sisältöä palautetaan onnistuneen vastauksen odottaminen.

<a name="CreateIndexer"></a>
## <a name="create-indexer"></a>Indeksin luominen ##

Voit luoda uuden indeksointitoiminto käyttämällä HTTP POST-pyynnön Azure Search-palvelun.
    
    POST https://[service name].search.windows.net/indexers?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

Voit vaihtoehtoisesti käyttää HYLLYTETTY ja Määritä tietolähteen nimi URI. Jos tietolähde ei ole, se luodaan.

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=[api-version]

> [AZURE.NOTE] Indeksoijilla sallittu enimmäismäärä vaihtelee hinnoittelu taso. Vapaa-palvelun avulla 3 Indeksoijilla. Vakio-palvelun avulla 50 Indeksoijilla. Katso lisätietoja [Palvelun rajoitukset](search-limits-quotas-capacity.md) .

`api-version` Tarvitaan. Nykyistä versiota ei `2015-02-28`. 

`api-key` On oltava järjestelmänvalvoja-näppäintä (eikä kyselyn avain). Lisätietoja [Haun palvelun REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx) lisätietoja näppäimet todennus-osassa. [Luo hakupalvelun portaalissa](search-create-service-portal.md) kerrotaan, miten palvelun URL-osoite ja pyynnön keskeisiä ominaisuuksia.


<a name="CreateIndexerRequestSyntax"></a>
**Leipäteksti syntaksi**

Pyynnön leipätekstissä indeksointitoiminto määritys, joka määrittää tietolähteen ja indeksoinnin, sekä valinnainen indeksoinnin aikataulun ja parametrit kohde-hakemisto. 


Pyyntö paketti jäsentämiseen syntaksi on seuraava. Esimerkki pyyntö on annettu edelleen artikkelista.

    { 
        "name" : "Required for POST, optional for PUT. The name of the indexer",
        "description" : "Optional. Anything you want, or null",
        "dataSourceName" : "Required. The name of an existing data source",
        "targetIndexName" : "Required. The name of an existing index",
        "schedule" : { Optional. See Indexing Schedule below. },
        "parameters" : { Optional. See Indexing Parameters below. },
        "fieldMappings" : { Optional. See Field Mappings below. },
        "disabled" : Optional boolean value indicating whether the indexer is disabled. False by default.  
    }

**Indeksointitoiminto aikataulu**

Indeksointitoiminto voit myös määrittää aikataulun. Jos aikataulun ei sisällä tietoja, indeksointitoiminto suoritetaan säännöllisesti aikataulun mukaan. Aikataulun on määritteet:

- `interval`: Pakollinen. Kesto-arvo, joka määrittää välin tai kauden indeksointitoiminto suoritetaan. Pienin sallittu aikaväli on 5 minuuttia. Pisin haara on yksi päivä. Se on muotoiltu XSD "dayTimeDuration" arvo (rajoitettu alijoukko [ISO 8601 kesto](http://www.w3.org/TR/xmlschema11-2/#dayTimeDuration) -arvo). Tämä kaava on: `"P[nD][T[nH][nM]]"`. Esimerkkejä: `PT15M` 15 minuutin välein `PT2H` , kahden tunnin välein. 

- `startTime`: Pakollinen. UTC ja, kun indeksointitoiminto alkamisajan käynnissä. 

**Indeksointitoiminto parametrit**

Indeksointitoiminto voit myös määrittää useita parametreja, jotka vaikuttavat sen toiminnan. Kaikki parametrit ovat valinnaisia.  

- `maxFailedItems`Kohteet, jotka voi epäonnistua voi indeksoida, ennen kuin indeksointitoiminto Suorita katsotaan epäonnistui: määrä. Oletusarvo on 0. Tietoja virheelliset kohteet palautetaan [Hae indeksointitoiminnon tila](#GetIndexerStatus) -toiminnolla. 

- `maxFailedItemsPerBatch`Kohteet, jotka voi epäonnistua indeksoida kuhunkin joukkoon, ennen kuin indeksointitoiminto Suorita katsotaan epäonnistui: määrä. Oletusarvo on 0.

- `base64EncodeKeys`: Määrittää, onko tiedoston näppäimet base-64-koodattu. Azure haun valtuutetut merkit, jotka voivat olla asiakirjan-näppäintä. Kuitenkin lähdetietojen arvot voivat sisältää merkkejä, jotka ovat virheellisiä. Jos se on tarpeen indeksoida kyseiset arvot kuin asiakirjan näppäimet, voit määrittää tämän merkinnän tosi. Oletusarvo on `false`.

- `batchSize`: Määrittää yhden erän, jotka ovat tietolähteen lukea ja indeksoitu kohteiden määrää, jotta voit parantaa suorituskykyä. Oletusarvo riippuu tietolähteen tyyppi: 1000 Azure SQL-ja DocumentDB ja Azure-Blob-säiliö 10.

**Kenttien yhdistämismääritykset**

Kentän yhdistämismäärityksiä avulla voit yhdistää tietolähteen kentän nimen kohde hakemiston eri kenttänimi. Harkitse esimerkiksi lähdetaulukon kentän kanssa `_id`. Azure haku ei salli alkaen alaviiva, jotta kenttä on nimettävä kenttänimi. Tämän voi tehdä käyttämällä `fieldMappings` ominaisuuden indeksointitoiminto seuraavasti: 
    
    "fieldMappings" : [ { "sourceFieldName" : "_id", "targetFieldName" : "id" } ] 

Voit määrittää useita kenttien yhdistämismääritykset: 

    "fieldMappings" : [ 
        { "sourceFieldName" : "_id", "targetFieldName" : "id" },
        { "sourceFieldName" : "_timestamp", "targetFieldName" : "timestamp" },
     ]

Lähde- ja kohdesivustojen kenttänimet ovat asennustavan.

<a name="FieldMappingFunctions"></a>
***Kentän määritystä Funktiot***

Kentän yhdistämismäärityksiä myös voidaan muuntaa kenttäarvot *Yhdistäminen-funktioiden*käyttäminen.

Vain yksi tällainen funktio on tällä hetkellä tueta: `jsonArrayToStringCollection`. Se jäsentää kenttä, joka sisältää merkkijonon, joka on muotoiltu JSON matriisina kohde hakemiston Collection(Edm.String) kenttään. Se on tarkoitettu käytettäväksi Azure SQL-indeksointitoiminto erityisesti, koska SQL ei ole alkuperäisen kokoelman tietotyyppi. Sillä voidaan seuraavasti: 

    "fieldMappings" : [ { "sourceFieldName" : "tags", "mappingFunction" : { "name" : "jsonArrayToStringCollection" } } ] 

Jos lähdekenttä sisältää merkkijonon esimerkiksi `["red", "white", "blue"]`, valitse kohdekentän tyyppi `Collection(Edm.String)` täytetään kolme arvoa `"red"`, `"white"` ja `"blue"`.

Huomaa, että `targetFieldName` ominaisuus on valinnainen; Jos jättää pois, `sourceFieldName` arvoa käytetään. 

<a name="CreateIndexerRequestExamples"></a>
**Pyydä leipäteksti-esimerkkejä**

Seuraavassa esimerkissä luodaan indeksointitoiminto, kopioi tiedot käyttämät taulukosta `ordersds` tietolähdettä `orders` indeksi aikataulun, joka alkaa 1 tammi 2015 UTC-ajan ja suorittaa kerran tunnissa. Kunkin indeksointitoiminto kutsu ole onnistunut, jos enintään 5 kohdetta ei voi indeksoida jokaisen erän ja enintään 10 kohdetta ei voi indeksoida yhteensä. 

    {
        "name" : "myindexer",
        "description" : "a cool indexer",
        "dataSourceName" : "ordersds",
        "targetIndexName" : "orders",
        "schedule" : { "interval" : "PT1H", "startTime" : "2015-01-01T00:00:00Z" },
        "parameters" : { "maxFailedItems" : 10, "maxFailedItemsPerBatch" : 5, "base64EncodeKeys": false }
    }

**Vastaus**

Onnistuneiden pyyntö 201 luotu.


<a name="UpdateIndexer"></a>
## <a name="update-indexer"></a>Päivitä indeksointitoiminto ##

Voit päivittää aiemmin luodun indeksointitoiminto käyttämällä HTTP valitseminen pyynnön. Voit myös määrittää päivittämään pyynnön URI indeksointitoiminto nimi:

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

`api-version` Tarvitaan. Nykyistä versiota ei `2015-02-28`. 

`api-key` On oltava järjestelmänvalvoja-näppäintä (eikä kyselyn avain). Lisätietoja [Haun palvelun REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx) lisätietoja näppäimet todennus-osassa. [Luo hakupalvelun portaalissa](search-create-service-portal.md) kerrotaan, miten palvelun URL-osoite ja pyynnön keskeisiä ominaisuuksia.

**Pyyntö**

Pyyntö leipätekstin syntaksi on sama kuin [Luo indeksointitoiminto pyynnöt](#CreateIndexerRequestSyntax).

**Vastaus**

Onnistuneiden pyynnön: 201 luotu Jos uusi indeksointitoiminto on luotu ja 204 ei sisältöä Jos aiemmin indeksointitoiminto on päivitetty.


<a name="ListIndexers"></a>
## <a name="list-indexers"></a>Luettelon Indeksoijilla ##

**Luettelon Indeksoijilla** -toiminto palauttaa Azure-hakupalvelun Indeksoijilla luettelo. 

    GET https://[service name].search.windows.net/indexers?api-version=[api-version]
    api-key: [admin key]


`api-version` Tarvitaan. Preview-versiosta on `2015-02-28-Preview`. [Azure haun versiotiedot](https://msdn.microsoft.com/library/azure/dn864560.aspx) on tiedot ja lisätietoja vaihtoehtoisen versioista.

`api-key` On oltava järjestelmänvalvoja-näppäintä (eikä kyselyn avain). Lisätietoja [Haun palvelun REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx) lisätietoja näppäimet todennus-osassa. [Luo hakupalvelun portaalissa](search-create-service-portal.md) kerrotaan, miten palvelun URL-osoite ja pyynnön keskeisiä ominaisuuksia.

**Vastaus**

Onnistuneiden pyynnön: 200 OK.

Tässä on esimerkki-vastauksen tekstissä:

    {
      "value" : [
      {
        "name" : "myindexer",
        "description" : "a cool indexer",
        "dataSourceName" : "ordersds",
        "targetIndexName" : "orders",
        ... other indexer properties
      }]
    }

Huomaa, että voit suodattaa vastauksen alaspäin käyttämällä ominaisuudet. Esimerkiksi jos haluat vain nimiluettelon indeksointitoiminto, käytä OData `$select` kyselyn vaihtoehto:

    GET /indexers?api-version=2014-10-20-Preview&$select=name

Tässä tapauksessa edellä olevassa esimerkissä vastaus näkyy seuraavasti: 

    {
      "value" : [ { "name": "myindexer" } ]
    }

Tämä on hyödyllinen tapa tallentaa kaistanleveyden, jos sinulla on paljon Indeksoijilla Search-palvelun.


<a name="GetIndexer"></a>
## <a name="get-indexer"></a>Hae indeksointitoiminto ##

**Hae indeksointitoiminto** toiminto saa indeksointitoiminto määritelmän Azure haku.

    GET https://[service name].search.windows.net/indexers/[indexer name]?api-version=[api-version]
    api-key: [admin key]

`api-version` Tarvitaan. Preview-versiosta on `2015-02-28-Preview`. 

`api-key` On oltava järjestelmänvalvoja-näppäintä (eikä kyselyn avain). Lisätietoja [Haun palvelun REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx) lisätietoja näppäimet todennus-osassa. [Luo hakupalvelun portaalissa](search-create-service-portal.md) kerrotaan, miten palvelun URL-osoite ja pyynnön keskeisiä ominaisuuksia.

**Vastaus**

Tilakoodin: 200 OK palautetaan onnistuneen vastauksen odottaminen.

Vastaus on [luoda indeksointitoiminto Esimerkki pyynnöt](#CreateIndexerRequestExamples)esimerkeissä: 

    {
        "name" : "myindexer",
        "description" : "a cool indexer",
        "dataSourceName" : "ordersds",
        "targetIndexName" : "orders",
        "schedule" : { "interval" : "PT1H", "startTime" : "2015-01-01T00:00:00Z" },
        "parameters" : { "maxFailedItems" : 10, "maxFailedItemsPerBatch" : 5, "base64EncodeKeys": false }
    }


<a name="DeleteIndexer"></a>
## <a name="delete-indexer"></a>Indeksin poistaminen ##

**Poista indeksointitoiminto** toiminto poistaa indeksointitoiminto Azure Search-palvelun.

    DELETE https://[service name].search.windows.net/indexers/[indexer name]?api-version=[api-version]
    api-key: [admin key]

Kun indeksointitoiminto poistetaan, aikalohkon käynnissä suorituskerran indeksointitoiminto suoritetaan loppuun, mutta ei ole vielä suorituskerran ajoitetaan. HTTP-tilakoodin 404 aiheuttaa yrittää käyttää olematon indeksointitoiminto ei löydy. 
 
`api-version` Tarvitaan. Preview-versiosta on `2015-02-28-Preview`. 

`api-key` On oltava järjestelmänvalvoja-näppäintä (eikä kyselyn avain). Lisätietoja [Haun palvelun REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx) lisätietoja näppäimet todennus-osassa. [Luo hakupalvelun portaalissa](search-create-service-portal.md) kerrotaan, miten palvelun URL-osoite ja pyynnön keskeisiä ominaisuuksia.

**Vastaus**

Tilakoodin: 204 ei sisältöä palautetaan onnistuneen vastauksen odottaminen.

<a name="RunIndexer"></a>
## <a name="run-indexer"></a>Suorita indeksointitoiminto ##

Lisäksi on säännöllisesti aikataulun, indeksointitoiminto myös voidaan kutsua pyydettäessä **Suorita indeksointitoiminto** -toiminnon kautta: 

    POST https://[service name].search.windows.net/indexers/[indexer name]/run?api-version=[api-version]
    api-key: [admin key]

`api-version` Tarvitaan. Preview-versiosta on `2015-02-28-Preview`. 

`api-key` On oltava järjestelmänvalvoja-näppäintä (eikä kyselyn avain). Lisätietoja [Haun palvelun REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx) lisätietoja näppäimet todennus-osassa. [Luo hakupalvelun portaalissa](search-create-service-portal.md) kerrotaan, miten palvelun URL-osoite ja pyynnön keskeisiä ominaisuuksia.

**Vastaus**

Tilakoodin: 202 hyväksytyt palautetaan onnistuneen vastauksen odottaminen.

<a name="GetIndexerStatus"></a>
## <a name="get-indexer-status"></a>Indeksointitoiminnon tila ##

**Hae indeksointitoiminnon tila** -toiminto hakee nykyisen tilan ja suorittamisen historia indeksointitoiminto: 

    GET https://[service name].search.windows.net/indexers/[indexer name]/status?api-version=[api-version]
    api-key: [admin key]


`api-version` Tarvitaan. Preview-versiosta on `2015-02-28-Preview`. 

`api-key` On oltava järjestelmänvalvoja-näppäintä (eikä kyselyn avain). Lisätietoja [Haun palvelun REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx) lisätietoja näppäimet todennus-osassa. [Luo hakupalvelun portaalissa](search-create-service-portal.md) kerrotaan, miten palvelun URL-osoite ja pyynnön keskeisiä ominaisuuksia.

**Vastaus**

Tilakoodin: 200 OK onnistuneen vastauksen odottaminen.

Vastauksen tekstissä esiintyy yleisen indeksoinnin kuntoa tilan, viimeisen indeksointitoiminto kutsu sekä viimeisimmät indeksointitoiminto ohjelmarakennekaaviossa historiatiedot tiedot (Jos löytyy). 

Malli-vastauksen tekstissä on seuraavanlainen: 

    {
        "status":"running",
        "lastResult": {
            "status":"success",
            "errorMessage":null,
            "startTime":"2014-11-26T03:37:18.853Z",
            "endTime":"2014-11-26T03:37:19.012Z",
            "errors":[],
            "itemsProcessed":11,
            "itemsFailed":0,
            "initialTrackingState":null,
            "finalTrackingState":null
         },
        "executionHistory":[ {
            "status":"success",
            "errorMessage":null,
            "startTime":"2014-11-26T03:37:18.853Z",
            "endTime":"2014-11-26T03:37:19.012Z",
            "errors":[],
            "itemsProcessed":11,
            "itemsFailed":0,
            "initialTrackingState":null,
            "finalTrackingState":null
        }]
    }

**Indeksointitoiminnon tila**

Indeksointitoiminnon tila voi olla jokin seuraavista arvoista:

- `running`Ilmaisee, että indeksointitoiminto on käynnissä normaalisti. Huomaa, että jotkin indeksointitoiminto suorituskerran edelleen olla viallinen, joten kannattaa tarkistaa `lastResult` sekä ominaisuus. 

- `error`Ilmaisee, että indeksointitoiminto ilmeni virhe, joka ei korjata automaattisesti. Esimerkiksi tietolähteen tunnistetietoja voi olla vanhentunut tai tietolähteen tai kohde indeksin rakenne on muuttunut jakautumisen tavalla. 

**Indeksointitoiminto suorittamisen tulos**

Indeksointitoiminto-suorittamisen tulos on yksittäinen indeksointitoiminto suorittamisen tietoja. Uusimman tulos on esiin nimellä `lastResult` indeksointitoiminnon tila-ominaisuus. Muut viimeisimmät tulokset, jos se on valittavissa, palautetaan `executionHistory` indeksointitoiminnon tila-ominaisuus. 

Indeksointitoiminto suorittamisen tulos sisältää seuraavat ominaisuudet: 

- `status`: suorittamisen tila. [Indeksointitoiminto suorituksen tilan](#IndexerExecutionStatus) lisätietoja. 

- `errorMessage`: epäonnistui suorittamisen virhesanoma. 

- `startTime`: suorittaminen käynnistyessään UTC-aika.

- `endTime`: kun suorittaminen päättyi UTC-aika. Tämä arvo ei ole määritetty, jos suorittaminen on kesken.

- `errors`: virheet, jos sellainen on Kohdetason matriisin. Tekstien sisältää asiakirjan avaimen (`key` ominaisuus) ja virhesanoma (`errorMessage` ominaisuutta). 

- `itemsProcessed`: tietolähteen kohteita (esimerkiksi taulukon rivejä), jotka indeksointitoiminto yritti indeksoida tämän suorituksen aikana määrä. 

- `itemsFailed`:, joka epäonnistui tämän suorituksen aikana kohteiden määrän.  
 
- `initialTrackingState`: aina `null` ensimmäisen indeksointitoiminto suorittamisen, tai jos tiedot muuta seuranta-käytäntö ei ole otettu käyttöön tietolähteen. Jos tällainen käytäntö on käytössä, seuraavien suorituskerran tämä arvo viittaa ensimmäinen (alin) muutosten jäljityksen suorittaminen käsittelemien arvo. 

- `finalTrackingState`: aina `null` Jos tiedot muuttuvat seuranta-käytäntö ei ole otettu käyttöön tietolähteen. Muussa tapauksessa osoittaa uusimman (suurin) muutosten jäljityksen suorittaminen käsitteli arvo. 

<a name="IndexerExecutionStatus"></a>
**Indeksointitoiminto suorittamisen tila**

Indeksointitoiminto suorituksen tilan sieppaa yksittäisen indeksointitoiminto suorittamisen tila. Asiakirjassa voi olla seuraavat arvot:

- `success`osoittaa indeksointitoiminto suorittaminen onnistui.

- `inProgress`Ilmaisee, että indeksointitoiminto suorittamisen on käynnissä. 

- `transientFailure`Ilmaisee, että indeksointitoiminto suorittaminen epäonnistui. Katso `errorMessage` ominaisuuden tiedot. Virhe voi tai ei voi vaatia ihmisten toimia, jos haluat korjata – esimerkiksi korjaaminen rakenteen yhteensopivuusongelman välillä tietolähteen ja kohde-hakemisto edellyttää käyttäjän, mutta tietojen tilapäinen lähde-käyttökatkot ei. Indeksointitoiminto ohjelmarakennekaaviossa edelleen kohti aikataulu, jos se on käytettävissä. 

- `persistentFailure`Ilmaisee, että indeksointitoiminto epäonnistui niin, että edellyttää ihmisen. Aikataulun mukainen indeksointitoiminto suorituskerran Seis. Jälkeen yhteystietoryhmälle ongelman, Käynnistä ajoitetun suorituskerran Palauta indeksointitoiminto API avulla. 

- `reset`osoittaa indeksointitoiminto Palauta kutsu Palauta indeksointitoiminto API (Katso alla oleva kuva). 

<a name="ResetIndexer"></a>
## <a name="reset-indexer"></a>Palauta indeksointitoiminto ##

**Palauta indeksointitoiminto** -toiminto poistaa muutosten jäljityksen indeksointitoiminto liittyvä osavaltio. Näin voit käynnistää alkaen-uudelleenindeksointi (esimerkiksi jos tietolähteen rakenne on muuttunut) tai muuttaa tietojen muuttaminen tunnistus käytäntö indeksointitoiminto liittyvä tietolähde.   

    POST https://[service name].search.windows.net/indexers/[indexer name]/reset?api-version=[api-version]
    api-key: [admin key]

`api-version` Tarvitaan. Preview-versiosta on `2015-02-28-Preview`. 

`api-key` On oltava järjestelmänvalvoja-näppäintä (eikä kyselyn avain). Lisätietoja [Haun palvelun REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx) lisätietoja näppäimet todennus-osassa. [Luo hakupalvelun portaalissa](search-create-service-portal.md) kerrotaan, miten palvelun URL-osoite ja pyynnön keskeisiä ominaisuuksia.

**Vastaus**

Tilakoodin: 204 ei sisältöä onnistuneen vastauksen odottaminen.

## <a name="mapping-between-sql-data-types-and-azure-search-data-types"></a>SQL-tietotyypit ja Azure haku-tietotyypit ##

<table style="font-size:12">
<tr>
<td>SQL-tietotyyppi</td>  
<td>Sallittu kohde indeksin kenttien lajit</td>
<td>Huomautuksia</td>
</tr>
<tr>
<td>Bit</td>
<td>Edm.Boolean, Edm.String</td>
<td></td>
</tr>
<tr>
<td>INT, smallint, tinyint</td>
<td>Edm.Int32, Edm.Int64, Edm.String</td>
<td></td>
</tr>
<tr>
<td>bigint</td>
<td>Edm.Int64, Edm.String</td>
<td></td>
</tr>
<tr>
<td>reaali-liukuluku</td>
<td>Edm.Double, Edm.String</td>
<td></td>
</tr>
<tr>
<td>Smallmoney rahaa<br>desimaaliluku<br>numeerinen
</td>
<td>Edm.String</td>
<td>Azure haku ei tue desimaaleja tyypit muuntaminen Edm.Double, koska tämä menettävät tarkkuus
</td>
</tr>
<tr>
<td>merkki, nchar, varchar nvarchar</td>
<td>Edm.String<br/>Collection(EDM.String)</td>
<td>Lisätietoja siitä, miten voit muuntaa merkkijonossa sarakkeen Collection(Edm.String) tämän asiakirjan [Kentän yhdistäminen Funktiot](#FieldMappingFunctions) kohdassa</td>
</tr>
<tr>
<td>smalldatetime-, päivämäärä ja aika-, datetime2-, päivämäärä-, DateTimeOffset-arvoa</td>
<td>Edm.DateTimeOffset, Edm.String</td>
<td></td>
</tr>
<tr>
<td>uniqueidentifer</td>
<td>Edm.String</td>
<td></td>
</tr>
<tr>
<td>Geography</td>
<td>Edm.GeographyPoint</td>
<td>Tuetaan vain geography esiintymät tyyppi tuominen kohtaa SRID 4326 (joka on myös oletus)</td>
</tr>
<tr>
<td>ROWVERSION</td>
<td>PUUTTUU</td>
<td>Rivi-version sarakkeita ei voi tallentaa hakuindeksin, mutta niitä voidaan käyttää muutosten seuranta</td>
</tr>
<tr>
<td>aika-aikajakson<br>Binaarinen, varbinary, kuva<br>XML-geometria CLR-tietotyypit</td>
<td>PUUTTUU</td>
<td>Ei tueta</td>
</tr>
</table>

## <a name="mapping-between-json-data-types-and-azure-search-data-types"></a>JSON-tietotyypit ja Azure haku-tietotyypit ##

<table style="font-size:12">
<tr>
<td>JSON-tietotyyppi</td> 
<td>Sallittu kohde indeksin kenttien lajit</td>
<td>Huomautuksia</td>
</tr>
<tr>
<td>bool</td>
<td>Edm.Boolean, Edm.String</td>
<td></td>
</tr>
<tr>
<td>Kiinteä numerot</td>
<td>Edm.Int32, Edm.Int64, Edm.String</td>
<td></td>
</tr>
<tr>
<td>Perus numerot</td>
<td>Edm.Double, Edm.String</td>
<td></td>
</tr>
<tr>
<td>merkkijono</td>
<td>Edm.String</td>
<td></td>
</tr>
<tr>
<td>alkuperäisten matriisien kirjoittaa, esimerkiksi ["a", "b", "c"]</td>
<td>Collection(EDM.String)</td>
<td></td>
</tr>
<tr>
<td>Merkkijonoja, jotka näyttävät päivämäärät</td>
<td>Edm.DateTimeOffset, Edm.String</td>
<td></td>
</tr>
<tr>
<td>GeoJSON pisteen objektit</td>
<td>Edm.GeographyPoint</td>
<td>GeoJSON pisteet ovat JSON objekteja seuraavassa muodossa: {"tyyppi": "Kohdan", "koordinaatit": [pitkä lat]} </td>
</tr>
<tr>
<td>Muut JSON-objektit</td>
<td>PUUTTUU</td>
<td>Ei tueta. Azure haun tukee tällä hetkellä vain primitiivityyppejä ja merkkijonon sivustokokoelmat</td>
</tr>
</table>