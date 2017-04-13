<properties 
    pageTitle="Siirrä tiedot Sybase | Azure Data Factory" 
    description="Lisätietoja tietojen siirtäminen Sybase-tietokannasta Azure Data Factory." 
    services="data-factory" 
    documentationCenter="" 
    authors="linda33wj" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/20/2016" 
    ms.author="jingwang"/>

# <a name="move-data-from-sybase-using-azure-data-factory"></a>Siirrä tiedot käyttämällä Azure Data Factory Sybase 

Tässä artikkelissa käsitellään käyttämisestä kopioi tehtävän Azure tietojen factory-voit siirtää tietoja toiseen tietovaraston Sybase. Tässä artikkelissa perustuu [tietojen siirtämistä tehtävät](data-factory-data-movement-activities.md) -artikkelista, jossa näkyy yleiskatsaus tietojen siirto kopioi tehtävän ja tuettuja kaupan yhdistelmät.

Tietoja Factory-palvelu tukee yhteyden muodostaminen paikalliseen Sybase lähteistä käyttämällä Data Management Gateway. Lisätietoja Data Management Gateway ja vaiheittaiset ohjeet yhdyskäytävän määrittäminen [paikallista sijainnit ja cloud siirtäminen tietojen](data-factory-move-data-between-onprem-and-cloud.md) artikkelista. 

> [AZURE.NOTE]
> Yhdyskäytävän tarvitaan, vaikka Sybase-tietokanta sijaitsee Azure-IaaS AM. Voit asentaa yhdyskäytävän saman IaaS AM, kuin tietovaraston tai eri AM, kunhan yhdyskäytävän muodostaa yhteyden tietokantaan. 

Tietoja factory tukee tällä hetkellä vain siirtäminen tiedot Sybase muiden tietojen stores, ei muiden tietojen myymälät Sybase.

## <a name="installation"></a>Asennus

Data Management Gatewayn muodostaminen Sybase-tietokantaan sinun täytyy asentaa [Sybase-tietopalvelu](http://go.microsoft.com/fwlink/?linkid=324846) saman järjestelmässä Data Management Gateway on.

> [AZURE.NOTE] Artikkelissa [vianmääritys yhdyskäytävän ongelmien](data-factory-data-management-gateway.md#troubleshoot-gateway-issues) vianmääritys yhteyden/yhdyskäytävän vihjeitä liittyvät ongelmat. 

## <a name="copy-data-wizard"></a>Ohjattu tietojen kopioiminen
Helpoin tapa luoda myyntijakso, kopioi tiedot Sybase-tietokannasta johonkin seuraavista tuetuista käsittelytoiminto tietoja tallennetuista tiedoista on käyttämällä ohjattua Kopioi tiedot. Katso [Opetusohjelma: Luo kopio ohjatulla putkijohto](data-factory-copy-data-wizard-tutorial.md) nopeasti käyttämällä ohjattua tietojen kopioiminen putkijohto luomisesta saat. 

Seuraavassa esimerkissä on esimerkki JSON määritykset, jonka avulla voit luoda putkijohto [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) tai [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) tai [PowerShellin Azure](data-factory-copy-activity-tutorial-using-powershell.md). Ne näyttävät tietojen kopioiminen Azure-Blob-säiliö Sybase-tietokanta. Tietoja voi kuitenkin kopioida johonkin seuraavista poistumia määritetyllä tavalla [tähän](data-factory-data-movement-activities.md#supported-data-stores) kopio-toimintojen käyttäminen Azure Data Factory.   

## <a name="sample-copy-data-from-sybase-to-azure-blob"></a>Esimerkki: Tietojen kopioiminen Sybase Azure-Blob
Tässä esimerkissä esitetään, kuinka haluat kopioida tiedot Azure-Blob-objektien tallennustilaan Sybase-tietokannasta. Tietoja voi kuitenkin kopioidun **suoraan** johonkin seuraavista poistumia määritetyllä tavalla [tähän](data-factory-data-movement-activities.md#supported-data-stores) kopio-toimintojen käyttäminen Azure Data Factory.  
 
Malli on tietoja factory-kohteet:

1.  Linkitetyn palvelu [OnPremisesSybase](data-factory-onprem-sybase-connector.md#sybase-linked-service-properties)tyyppi.
2.  Tyypin [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties)liked palvelu.
3.  Syötteen [tietojoukko](data-factory-create-datasets.md) tyypin [RelationalTable](data-factory-onprem-sybase-connector.md#sybase-dataset-type-properties).
4.  Tulosteen [tietojoukko](data-factory-create-datasets.md) tyypin [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4.  [Myyntijakso](data-factory-create-pipelines.md) kopioi aktiviteettiin, joka käyttää [RelationalSource](data-factory-onprem-sybase-connector.md#sybase-copy-activity-type-properties) ja [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties).

Otosten kopioi tiedot kyselyn tuloksen Sybase-tietokanta-blob tunnissa. Käyttää näitä esimerkkejä JSON-ominaisuuksia on kuvattu osia seuraavat mallit. 

Ensimmäisessä vaiheessa asennusohjelma data management gateway. Ohjeet ovat [paikallisen sijainnit ja cloud siirtäminen tietojen](data-factory-move-data-between-onprem-and-cloud.md) -artikkelissa.

**Linkitetty Sybase-palvelu:**

    {
        "name": "OnPremSybaseLinkedService",
        "properties": {
            "type": "OnPremisesSybase",
            "typeProperties": {
                "server": "<server>",
                "database": "<database>",
                "schema": "<schema>",
                "authenticationType": "<authentication type>",
                "username": "<username>",
                "password": "<password>",
                "gatewayName": "<gatewayName>"
            }
        }
    }

**Azure-Blob-säiliö linkitetty palvelu:**

    {
        "name": "AzureStorageLinkedService",
        "properties": {
            "type": "AzureStorageLinkedService",
            "typeProperties": {
                "connectionString": "DefaultEndpointsProtocol=https;AccountName=<AccountName>;AccountKey=<AccountKey>"
            }
        }
    }


**Sybase syötteen tietojoukko:**

Otosten oletetaan, että olet luonut taulukon "MyTable" Sybase ja se sisältää sarake nimeltä "aikaleima" sarjan Aikatietojen.

Asetus "ulkoinen": TOSI ilmoittaa Data Factory-palvelu, että tämä tietojoukko on ulkoiset tiedot-factory ja tietojen factory toimintaa ei tuottamat. Huomaa, että linkitetyn palvelun **tyyppi** on määritetty: **RelationalTable**. 
    
    {
        "name": "SybaseDataSet",
        "properties": {
            "type": "RelationalTable",
            "linkedServiceName": "OnPremSybaseLinkedService",
            "typeProperties": {},
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true,
            "policy": {
                "externalData": {
                    "retryInterval": "00:01:00",
                    "retryTimeout": "00:10:00",
                    "maximumRetry": 3
                }
            }
        }
    }


**Azure-Blob tulosteen tietojoukko:**

Tietoja kirjoitetaan uusia Blob-objektien tunnissa (taajuus: tunti, väli: 1). Kansiopolun blob arvioidaan dynaamisesti sektoria, käsitellään alkamispäivää perusteella. Kansiopolku käytetään vuoden, kuukauden, päivän ja alkamisajan tuntia osat.

    {
        "name": "AzureBlobSybaseDataSet",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/sybase/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
                "format": {
                    "type": "TextFormat",
                    "rowDelimiter": "\n",
                    "columnDelimiter": "\t"
                },
                "partitionedBy": [
                    {
                        "name": "Year",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "yyyy"
                        }
                    },
                    {
                        "name": "Month",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "MM"
                        }
                    },
                    {
                        "name": "Day",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "dd"
                        }
                    },
                    {
                        "name": "Hour",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "HH"
                        }
                    }
                ]
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }


**Kopioi aktiviteettiin putkijohto:**

Putkisto sisältää kopio-toiminto, joka on määritetty käyttämään syöttö- ja tietojoukkoja ja on suunniteltu toimimaan kerran tunnissa. Myyntijakso JSON määritelmä- **lähteen** tyyppi on määritetty **RelationalSource** ja **käsittelytoiminto** tyyppi-asetukseksi on määritetty **BlobSink**. SQL-kyselyn, **kysely** -ominaisuuden valitsee tiedot hän. Tilaukset-taulukkoa tietokannassa.


    {
        "name": "CopySybaseToBlob",
        "properties": {
            "description": "pipeline for copy activity",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "RelationalSource",
                            "query": "select * from DBA.Orders"
                        },
                        "sink": {
                            "type": "BlobSink"
                        }
                    },
                    "inputs": [
                        {
                            "name": "SybaseDataSet"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobSybaseDataSet"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00",
                        "concurrency": 1
                    },
                    "scheduler": {
                        "frequency": "Hour",
                        "interval": 1
                    },
                    "name": "SybaseToBlob"
                }
            ],
            "start": "2014-06-01T18:00:00Z",
            "end": "2014-06-01T19:00:00Z"
        }
    }


## <a name="sybase-linked-service-properties"></a>Sybase linkitetty palveluominaisuudet

Seuraavassa taulukossa on kuvaus JSON elementtien tietyn linkitetty Sybase-palveluun.

Ominaisuus | Kuvaus | Pakollinen
-------- | ----------- | --------
tyyppi | Type-ominaisuus on määritettävä: **OnPremisesSybase** | Kyllä
palvelin | Sybase-palvelimen nimi. | Kyllä
tietokannan | Sybase-tietokannan nimi. | Kyllä 
rakenne  | Tietokannan rakenteen nimi. | Ei
authenticationType | Sybase-tietokantayhteyden muodostamisessa todennusta tyyppi. Mahdolliset arvot ovat: anonyymi, Basic ja Windows. | Kyllä
käyttäjänimi | Määritä käyttäjänimi, jos käytössäsi on Basic- tai Windows-todennusta. | Ei
salasana | Määritä käyttäjänimi määritetyn käyttäjätilin salasanan. |  Ei
gatewayName | Yhdyskäytävän, paikallisen Sybase-tietokantayhteyden muodostamisessa käytettävä Data Factory-palvelun nimi. | Kyllä 

Saat lisätietoja määrittäminen paikallista Sybase-tietolähteen tunnistetiedot [määrittää käyttöoikeudet ja suojaus](data-factory-move-data-between-onprem-and-cloud.md#set-credentials-and-security) .

## <a name="sybase-dataset-type-properties"></a>Sybase tietojoukko ominaisuudet

Täydellinen luettelo osien ja ominaisuudet, jotka ovat käytettävissä määrittäminen tietojoukkoja [luominen tietojoukkoja](data-factory-create-datasets.md) on artikkelissa. Osia, kuten rakenne, käytettävyys ja tietojoukko JSON käytännön muistuttavat tyypeissä tietojoukko (Azure SQL Azure-blob, Azuren taulukkojen jne.).

TypeProperties-osassa on eri mistäkin tietojoukko, ja se antaa tietoja tietojen tietovaraston sijainti. Tyypin **RelationalTable** (joka sisältää Sybase tietojoukko) tietojoukko **typeProperties** osiossa on seuraavat ominaisuudet:

Ominaisuus | Kuvaus | Pakollinen
-------- | ----------- | --------
taulukon nimi | Taulukko, joka viittaa linkitetyistä Sybase-tietokanta-esiintymän nimi. | Ei (Jos **RelationalSource** **kysely** on määritetty)

## <a name="sybase-copy-activity-type-properties"></a>Sybase kopioi tehtävän ominaisuudet 

Täydellinen luettelo osat ja ominaisuudet käytettävissä olevat toiminnot, katso [luominen putkijohdot](data-factory-create-pipelines.md) artikkelissa määrittämistä varten. Ominaisuudet, kuten nimi, kuvaus sekä syöttö- ja taulukot ja käytäntö ovat käytettävissä kaikki tehtävälajit. 

Tehtävän typeProperties-osassa käytettävissä olevat ominaisuudet vaihtelevat toisaalta kunkin tehtävätyyppi. Kopioi tehtävälle ne vaihtelevat sen mukaan, lähteiden ja poistumia.

Kun lähteen tyyppi **RelationalSource** (joka sisältää Sybase) on seuraavat ominaisuudet ovat käytettävissä **typeProperties** -osassa:

Ominaisuus | Kuvaus | Sallittu arvo | Pakollinen
-------- | ----------- | -------------- | --------
kyselyn | Mukautetun kyselyn avulla voit lukea tietoja. | SQL-kyselymerkkijonon. Esimerkki: Valitse *-MyTable. | Ei (Jos **taulukon nimi** , **tietojoukko** on määritetty)

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## <a name="type-mapping-for-sybase"></a>Sybase yhdistäminen tyyppi

[Tietojen siirtämistä toiminnot](data-factory-data-movement-activities.md) on artikkelissa mainittu kopioi tehtävän suorittaa Automaattinen tyypin muunnoksia lähteen tyyppiä vastaamaan allas tyypit seuraavista 2 vaihe vaiheelta kanssa:

1. Muuntaminen alkuperäisen tietolähteen tyypit .NET-tyyppi
2. .NET tyypistä muuntaminen alkuperäisen käsittelytoiminto tyyppi

Sybase tukee T-SQL- ja T-SQL-tietotyypit. Yhdistämismäärityksen taulukosta sql-tietotyypit .NET tyyppi-kohdassa [Azure SQL-yhdistin](data-factory-azure-sql-connector.md) artikkelissa.

[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-type-repeatability-for-relational-sources](../../includes/data-factory-type-repeatability-for-relational-sources.md)]

## <a name="performance-and-tuning"></a>Suorituskyky ja säätäminen  
Lisätietoja on artikkelissa [Kopioi tehtävän suorituskyvyn & säätäminen opas](data-factory-copy-activity-performance.md) lisätietoja tärkeimmät seikat, vaikutus suorituskykyä tietojen siirto (kopioi tehtävä) Azure Data Factory ja voidaan optimoida sen usealla tavalla.