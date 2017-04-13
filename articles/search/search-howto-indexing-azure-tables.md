<properties
pageTitle="Indeksoinnin Azure haulla Azure-taulukkotallennus"
description="Opettele indeksoida Azure haulla Azure taulukoihin tallennetut tiedot"
services="search"
documentationCenter=""
authors="chaosrealm"
manager="pablocas"
editor="" />

<tags
ms.service="search"
ms.devlang="rest-api"
ms.workload="search" ms.topic="article"  
ms.tgt_pltfrm="na"
ms.date="08/16/2016"
ms.author="eugenesh" />

# <a name="indexing-azure-table-storage-with-azure-search"></a>Indeksoinnin Azure haulla Azure-taulukkotallennus

Tässä artikkelissa kerrotaan, miten Azure-haun avulla indeksi Azure-taulukoiden tallennustilaan tallennettuja tietoja. Uusi Azure haun taulukon indeksointitoiminto on tämän prosessin saumattoman ja nopeasti. 

> [AZURE.IMPORTANT] Tätä toimintoa ei tällä hetkellä esikatselu. Se on käytettävissä vain REST-Ohjelmointirajapinnalla versio **2015-02-28-esikatselu** ja .NET SDK versiota 2.0-esikatselu. Ota muista, esikatsella ohjelmointirajapinnan on tarkoitettu testaus ja arviointi ja ei voi käyttää tuotantoympäristössä.

## <a name="setting-up-azure-table-indexing"></a>Azuren taulukkojen indeksoinnin määrittäminen

Voit määrittää ja määritä Azuren taulukkojen indeksointitoiminto, voit käyttää Azure haun REST-Ohjelmointirajapinta voit luoda ja hallita **Indeksoijilla** ja **tietolähteiden** kuvatulla tavalla [indeksointitoiminto toiminnot](https://msdn.microsoft.com/library/azure/dn946891.aspx). Voit käyttää myös .NET SDK [versiota 2.0-esikatselu](https://msdn.microsoft.com/library/mt761536%28v=azure.103%29.aspx) . Jatkossa tuki taulukon indeksoinnin lisätään Azure-portaaliin.

Tietolähteen määrittää, mitä tietoja haluat indeksoida, käyttää tietoja ja käytäntöjä, jotka tehokkaasti tiedot (uusi, muokata ja poistaa rivejä) tehdyt muutokset tunnistetaan Azure haku on otettu käyttöön tarvittavat tunnistetiedot.

Indeksointitoiminto lukee tietoja tietolähteestä ja latautuu kohde hakuindeksin.

Taulukon indeksoinnin määrittäminen:

1. Tietolähteen luominen
    - Määritä `type` parametri`azuretable`
    - Välitä kuin tallennustilan tilin yhteysmerkkijono `credentials.connectionString` parametri
    - Määritä taulukon nimi käyttämällä `container.name` parametri
    - Voit myös määrittää kyselyn avulla `container.query` parametria. Mahdollisuuksien mukaan suodattamalla-PartitionKey parhaan suorituskyvyn; muut kyselyn tulos koko taulukon tarkistuksen, mikä voi johtaa suurten taulukoiden huonosti.
2. Etsi indeksin luominen rakenteen, joka vastaa taulukon, jonka haluat indeksoida sarakkeiden kanssa. 
3. Voit luoda indeksointitoiminto yhdistämällä tietolähteen hakuindeksin.

### <a name="create-data-source"></a>Tietolähteen luominen

    POST https://[service name].search.windows.net/datasources?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "table-datasource",
        "type" : "azuretable",
        "credentials" : { "connectionString" : "<my storage connection string>" },
        "container" : { "name" : "my-table", "query" : "PartitionKey eq '123'" }
    }   

Saat lisätietoja Ohjelmointirajapinnan tietolähteen luominen-kohdassa [Luo tietolähde](search-api-indexers-2015-02-28-preview.md#create-data-source).

### <a name="create-index"></a>Indeksin luominen 

    POST https://[service name].search.windows.net/indexes?api-version=2015-02-28
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "my-target-index",
        "fields": [
            { "name": "key", "type": "Edm.String", "key": true, "searchable": false },
            { "name": "SomeColumnInMyTable", "type": "Edm.String", "searchable": true }
        ]
    }

Saat lisätietoja Ohjelmointirajapinnan indeksin luominen- [Indeksin luominen](https://msdn.microsoft.com/library/dn798941.aspx)

### <a name="create-indexer"></a>Indeksin luominen 

Luo lopuksi indeksointitoiminto, joka viittaa tietolähteen ja kohde-hakemisto. Esimerkki:

    POST https://[service name].search.windows.net/indexers?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "table-indexer",
      "dataSourceName" : "table-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" }
    }

Lisätietoja Ohjelmointirajapinnan indeksointitoiminto luominen Tutustu [Luominen indeksointitoiminto](search-api-indexers-2015-02-28-preview.md#create-indexer).

Tämä on kaikki ei tarvitse tehdä mitään - hyvää indeksoinnin!

## <a name="dealing-with-different-field-names"></a>Eri kenttien nimet käsittely

Usein aiemmin luotua indeksiä kenttien nimet on erilainen taulukon ominaisuuksien nimet. Voit yhdistää ominaisuuksien nimet-taulukosta, joita Etsi indeksi-kenttien nimet **kentän yhdistämismäärityksiä** . Lisätietoja kentän yhdistämismäärityksiä on artikkelissa [Azure haun indeksointitoiminto kentän yhdistämismäärityksiä silta tietolähteet ja hakuindeksin erot](search-indexer-field-mappings.md).

## <a name="handling-document-keys"></a>Asiakirjan näppäimet käsittely

Azure hakutoiminnossa asiakirjan avain yksilöi asiakirjan. Jokaisen hakuindeksin on oltava täsmälleen yksi tärkeimmistä kentän tietotyyppi on `Edm.String`. Avaimen kenttä on tarvittavat kunkin asiakirjan, joka lisätään indeksi (itse asiassa se on vain pakollisen kentän).

Koska taulukkorivit on avain, Azure haun Luo synteettistä-nimisen kentän `Key` , joka on yhdistelmä osion avain ja rivin avainarvot. Esimerkiksi jos rivin PartitionKey on `PK1` ja RowKey `RK1`, valitse `Key` -kentän arvo on `PK1RK1`. 

> [AZURE.NOTE] `Key` Arvo saattaa sisältää merkkejä, joita ei voi käyttää asiakirjan näppäimet, kuten katkoviivat. Voit käsitellä virheellisiä merkkejä avulla `base64Encode` [kentän yhdistäminen-funktiota](search-indexer-field-mappings.md#base64EncodeFunction). Jos teet tämän, muista myös käyttää URL-Osoitteen sopivaa Base64 koodauksen kulkeva asiakirjan näppäimet API soittaa esimerkiksi haku.

## <a name="incremental-indexing-and-deletion-detection"></a>Vaiheittainen indeksoinnin ja poistaminen tunnistus
 
Kun määrität taulukon indeksointitoiminto suoritetaan aikataulun, vain uusi tai päivitetty rivejä, joka määräytyy rivin reindexes `Timestamp` arvo. Sinun ei tarvitse määrittää käytännön muutoksen tunnistaminen – lisäävän indeksoinnin on otettu käyttöön automaattisesti. 

Lisää ominaisuus, joka osoittaa, että se on poistettu, ja Pehmeä poistaminen tunnistus käytännön tietolähteestä määrittäminen osoittamaan, että tiettyjä asiakirjoja on poistettava hakemiston, voit käyttää pehmeitä Poista – strategia sijaan rivin poistamista. Esimerkiksi alla käytännön harkitse rivi poistetaan, jos se on ominaisuus `IsDeleted` arvolla `"true"`: 

    PUT https://[service name].search.windows.net/datasources?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]
    
    {
        "name" : "my-table-datasource",
        "type" : "azuretable",
        "credentials" : { "connectionString" : "<your storage connection string>" },
        "container" : { "name" : "table name", "query" : "query" },
        "dataDeletionDetectionPolicy" : { "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy", "softDeleteColumnName" : "IsDeleted", "softDeleteMarkerValue" : "true" }
    }   


## <a name="help-us-make-azure-search-better"></a>Auta meitä parantamaan Azure haun toimintaa

Jos sinulla on ehdottaa ominaisuuksia tai ideoita tehokkuutta, ota saavuttaminen meille [UserVoice-sivustossa](https://feedback.azure.com/forums/263029-azure-search/).