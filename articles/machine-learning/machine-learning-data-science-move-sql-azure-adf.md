<properties
    pageTitle="Siirrä tiedot paikallinen SQL Server-palvelimen kanssa Azure Data Factory SQL Azure | Azure"
    description="Määritä SYÖTTÖ-myyntijakso, joka luo kaksi tietojen siirron toimintoja, jotka yhdessä tietojen siirtäminen päivittäin tietokantojen paikallinen ja pilveen viestin."
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun" />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/14/2016"
    ms.author="bradsev" />


# <a name="move-data-from-an-on-premise-sql-server-to-sql-azure-with-azure-data-factory"></a>Siirrä tiedot paikallinen SQL Serveristä SQL Azure Azure Data Factory kanssa

Tässä ohjeaiheessa esitellään tietojen siirtäminen paikallinen SQL Server-tietokannasta SQL Azure-tietokannan kautta Azure-Blob-säiliö Azure tietojen Factory (SYÖTTÖ) avulla.

**Valikon** linkkejä ohjeaiheisiin, joissa kerrotaan, kuinka voit ingest tietojen tuominen kohdeluettelo-ympäristössä, jossa tiedot voidaan tallentaa ja käsitellä ryhmän tietojen tiede aikana.

[AZURE.INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]


## <a name="intro"></a>Johdanto: Mikä on SYÖTTÖ ja milloin sitä käytetään siirretään tietoja?

Azure Data Factory on täysin hallitun pilvipohjainen tietojen integrointi-palvelu, orchestrates ja automatisoi siirto ja tietojen muunnos. Avaimen SYÖTTÖ-mallissa on myyntijakso. Putkijohto on looginen ryhmittely toimintoja, joista jokaisella määrittää toimet tekemässä tietojoukkoja sisältämät tiedot. Linkitetyn palveluita käytetään Data Factory muodostaa yhteyden tiedot-resurssien tarvittavat tiedot.

SYÖTTÖ-ja olemassa olevia tietojen käsittely-palveluja on laaditut kyselyjä, jotka ovat helposti käytettävissä ja hallittujen pilveen tietojen putkistot. Nämä tiedot putkistot voidaan ajoittaa ingest, valmisteleminen, muunna, analysoida ja tietojen julkaiseminen ja SYÖTTÖ hallitsee ja orchestrates monimutkaisten tietojen ja käsittelyn riippuvuudet. Ratkaisujen voidaan nopeasti sisäisten ja ottaa yhteyden kasvava määrä paikallisen pilvipalvelussa, ja cloud tietolähteet.

Harkitse SYÖTTÖ:

- Kun tiedot on siirrettävä jatkuvasti hybrid tilanne, joka käyttää sekä paikallisia että cloud resurssit 
- Kun tiedot on tapahtumallinen tai tarvitsee voi muokata tai lisätä siihen, kun jätettävien liiketoimintalogiikan. 

SYÖTTÖ mahdollistaa ajoitus ja käyttämällä yksinkertaisia JSON-komentosarjoja, joilla hallitaan tietojen säännöllisin väliajoin liikkuvuuteen töiden seuranta. SYÖTTÖ on myös muita ominaisuuksia, kuten tuki monimutkaisia toimintoja. Lisätietoja SYÖTTÖ on artikkelissa [Azure tietojen Factory (SYÖTTÖ)](https://azure.microsoft.com/services/data-factory/)documentation.


## <a name="scenario"></a>Skenaario

Kokeiluversion määrittäminen SYÖTTÖ-myyntijakso, joka luo viestin kaksi tietojen siirto-toimintoja. Yhdessä ne tietojen siirtäminen päivittäin paikallinen SQL-tietokantaan ja pilveen Azure SQL-tietokantaan. Kaksi tehtävät ovat seuraavat:

* tietojen kopioiminen Azure-Blob-säiliö tilin paikallinen SQL Server-tietokantaan
* Kopioi tietoja Azure-Blob-säiliö tilin Azure SQL-tietokantaan.

>[AZURE.NOTE] Tähän on toimitettu SYÖTTÖ saamiesi yksityiskohtaisempia opetusohjelman näkyvät vaiheet: [Siirrä paikallisesti ja pilvi Data Management Gateway ja tietojen](../data-factory/data-factory-move-data-between-onprem-and-cloud.md) viittaukset artikkelin olennaisista Kohdista, että aiheen toimitetaan tilanteen.


## <a name="prereqs"></a>Edellytykset
Tässä opetusohjelmassa oletetaan, että:

* On **Azure tilauksen**. Jos sinulla ei ole tilaus, voit rekisteröityä [maksuttoman kokeiluversion käyttäjäksi](https://azure.microsoft.com/pricing/free-trial/).
* **Azure-tallennustilan tilin**. Voit käyttää Azure-tallennustilan tilin Tässä opetusohjelmassa tietojen tallentamista varten. Jos sinulla ei ole Azure-tallennustilan tilin, on artikkelissa [Luo tallennustilan tili](storage-create-storage-account.md#create-a-storage-account) . Luotuasi tallennustilan tilin, sinun täytyy käyttää tallennustilaan tilin Key-tunnuksen. Katso [näkymässä, kopioi ja muodosta uudelleen sarjanumerot tallennustilan pikanäppäimet](storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys).
* **Azure SQL-tietokanta**Access. Jos käytössä on Azure SQL-tietokantaan, tpoic [Käytön aloittaminen Microsoft Azure SQL-tietokannan](../sql-database/sql-database-get-started.md) tietoja siitä, miten voit valmistella uuden esiintymän Azure SQL-tietokantaan.
* Asennettu ja määritetty **PowerShellin Azure** paikallisesti. Katso ohjeet [asentaminen ja määrittäminen PowerShellin Azure](../powershell-install-configure.md).

> [AZURE.NOTE] Tässä menetelmässä [Azure portal](https://portal.azure.com/).


##<a name="upload-data"></a>Tietojen lataaminen paikallinen SQL Serverin

[Kokousesitelmän taksin tietojoukon](http://chriswhong.com/open-data/foil_nyc_taxi/) avulla, siirtoprosessia. Kokousesitelmän taksin tietojoukko on käytettävissä merkille siten, että kirjoituksen Azure blob storage [Kokousesitelmän taksin tiedot](http://www.andresmh.com/nyctaxitrips/). Tiedoissa on kaksi tiedostoa, trip_data.csv-tiedosto, joka sisältää työmatkan tiedot, ja trip_far.csv-tiedosto, joka sisältää kullekin työmatkan maksettu maksun tiedot. Esimerkki ja näiden tiedostojen kuvaus toimitetaan [Kokousesitelmän taksin Trips tietojoukko kuvaus](machine-learning-data-science-process-sql-walkthrough.md#dataset).


Voit mukauttaa mukana tähän omiin tietoihisi joukkoon tai noudattamalla ohjeiden avulla Kokousesitelmän taksin tietojoukko. Lataa Kokousesitelmän taksin tietojoukko paikallinen SQL Server-tietokantaan, noudata kuvatut [Joukkona tietojen tuominen SQL Server-tietokantaan](machine-learning-data-science-process-sql-walkthrough.md#dbload). Nämä ohjeet koskevat SQL Server Azure Virtual Machine, mutta ohjeet lataaminen paikallinen SQL Server on sama.


##<a name="create-adf"></a>Azure tietojen Factory luominen

Ohjeet uusien Azure-tietoihin Factory ja resurssiryhmä luomiseen [Azure portal](https://portal.azure.com/) toimitetaan [luominen Azure-tietoihin Factory](../data-factory/data-factory-build-your-first-pipeline-using-editor.md#step-1-creating-the-data-factory). Uusi SYÖTTÖ esiintymän *adfdsp* ja resurssin luotu *adfdsprg*nimi.


## <a name="install-and-configure-up-the-data-management-gateway"></a>Asenna ja määritä Data Management Gateway määrittäminen

Azure tiedot-factory toimimaan paikallinen SQL Server-palvelimen yhteyttä putkistot käyttöön haluat lisätä sen tietoja factory linkitetyn palveluna. Jos haluat luoda linkitetyn palvelu paikallinen SQL Serveriä, sinun täytyy:

- Lataa ja asenna Microsoft Data Management Gateway-paikalliseen tietokoneeseen. 
- määrittää paikallisen tietolähteen yhdyskäytävän käyttämään linkitetyn-palvelun. 

Data Management Gateway serializes ja deserializes lähde- ja putoavat tiedot nykyisessä tietokoneessa.

Määritetään ohjeita ja tietoja Data Management Gateway on artikkelissa [paikallisesti ja pilvi Data Management Gateway ja tietojen siirtäminen](../data-factory/data-factory-move-data-between-onprem-and-cloud.md)


## <a name="adflinkedservices"></a>Luo linkitetty palvelua muodostaa yhteyden tiedot-resurssit

Linkitetyn palvelu määrittää Azure Data Factory muodostaa yhteyden tiedot resurssin tiedot. Vaiheittaiset ohjeet luominen linkitetyn services annetaan [Luo linkitetyn palvelut](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#step-2-create-linked-services).

Tässä tilanteessa, jossa linkitetyn services tarvitaan on kolme resurssit.

1. [Linkitetyn paikallinen SQL Server-palvelu](#adf-linked-service-onprem-sql)
2. [Linkitetyn palvelun Azure-Blob-säiliö](#adf-linked-service-blob-store)
3. [Linkitetyn palvelun Azure SQL-tietokantaan](#adf-linked-service-azure-sql)


###<a name="adf-linked-service-onprem-sql"></a>Linkitetyn palvelun paikallinen SQL Server-tietokantaan

Voit luoda linkitetyn palvelun paikallinen SQL Serverin seuraavasti:

- Valitse **Tietosäilö** Azure perinteinen portaalissa SYÖTTÖ-aloitussivu 
- Valitse **SQL** ja anna *käyttäjänimi* ja *salasana* tunnistetiedot paikallinen SQL Server. Sinun täytyy kirjoittaa **täydellinen palvelimen nimi kenoviiva esiintymänimi (Palvelimen_nimi\esiintymän_nimi)**palvelimen nimi. Nimeä linkitetyistä *adfonpremsql*.

###<a name="adf-linked-service-blob-store"></a>Linkitetyn Blob-palvelu

Voit luoda linkitetyn palvelun Azure-Blob-säiliö tilin seuraavasti:

- Valitse **Tietosäilö** Azure perinteinen portaalissa SYÖTTÖ-aloitussivu
- **Azure-tallennustilan tilin** valitseminen 
- Kirjoita Azure-Blob-säiliö tilin avain ja säilö nimi. Nimeä linkitetyistä *adfds*.

###<a name="adf-linked-service-azure-sql"></a>Linkitetyn palvelun Azure SQL-tietokantaan

Voit luoda linkitetyn palvelun Azure SQL-tietokantaan seuraavasti:

- Valitse **Tietosäilö** Azure perinteinen portaalissa SYÖTTÖ-aloitussivu
- Valitse **Azure SQL** ja kirjoita *käyttäjänimi* ja *salasana* -tunnistetiedot Azure SQL-tietokantaan. *Käyttäjänimi* on määritettävä muodossa *user@servername*.   


##<a name="adf-tables"></a>Määritä ja määritä, kuinka voit käyttää tietojoukkoja taulukoiden luominen

Voit luoda taulukoita, joka määrittää rakenne, sijainti ja käytettävyys tietojoukkoja komentosarjalla seuraavasti. JSON-tiedostoja käytetään taulukoiden määrittämistä. Saat lisätietoja näiden tiedostojen rakenteen [tietojoukkoja](../data-factory/data-factory-create-datasets.md).

> [AZURE.NOTE]  Sinun on suoritettava `Add-AzureAccount` cmdlet-komento, ennen kuin suoritat [Uusi AzureDataFactoryTable](https://msdn.microsoft.com/library/azure/dn835096.aspx) cmdlet-komento varmistaa, että oikea Azure tilaus on valittu komento suorittamista varten. Katso tämä cmdlet-komento asiakirjat, [Lisää AzureAccount](https://msdn.microsoft.com/library/azure/dn790372.aspx).

JSON-pohjainen määritelmiä taulukot käyttämällä seuraavia nimiä:

* paikallinen SQL server- **taulukkonimi** on *nyctaxi_data*
* Azure-Blob-säiliö tilin **säilön nimi** on *containername*  

Kolme määritysten tarvitaan SYÖTTÖ-myyntijakso:

1. [SQL-paikallinen taulukko](#adf-table-onprem-sql)
2. [BLOB-taulukossa](#adf-table-blob-store)
3. [SQL Azure-taulukosta](#adf-table-azure-sql)

> [AZURE.NOTE]  Näillä toimintosarjoilla käyttää PowerShellin Azure SYÖTTÖ-tehtävien luominen ja määrittäminen. Mutta seuraavien toimintojen myös onnistuu Azure-portaalissa. Lisätietoja on artikkelissa [Luo syötteen ja tulosteen tietojoukkoja](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#step-3-create-input-and-output-datasets).

###<a name="adf-table-onprem-sql"></a>SQL-paikallinen taulukko

Taulukkomääritys paikallinen SQL Server on määritetty JSON-tiedostossa:

        {
            "name": "OnPremSQLTable",
            "properties":
            {
                "location":
                {
                "type": "OnPremisesSqlServerTableLocation",
                "tableName": "nyctaxi_data",
                "linkedServiceName": "adfonpremsql"
                },
                "availability":
                {
                "frequency": "Day",
                "interval": 1,   
                "waitOnExternal":
                {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
                }

                }
            }
        }

Sarakkeiden nimet eivät sisälly tähän. Voit valita sarakkeiden nimet-osa lisäämällä ne tähän (koskevat tiedot valintaruudun [SYÖTTÖ dokumentaatio](../data-factory/data-factory-data-movement-activities.md ) aihetta.

Kopioi taulukko JSON määritykset tiedostoon, jota kutsutaan *onpremtabledef.json* tiedosto ja tallenna se käynnistyskansioon (tähän oletetaan olevan *C:\temp\onpremtabledef.json*). Luo taulukon SYÖTTÖ seuraavan Azure PowerShell cmdlet-komennon avulla seuraavasti:

    New-AzureDataFactoryTable -ResourceGroupName ADFdsprg -DataFactoryName ADFdsp –File C:\temp\onpremtabledef.json


###<a name="adf-table-blob-store"></a>BLOB-taulukossa
Blob-objektien tulostuskohde taulukon määritys on seuraavassa (tämä yhdistää nieltäväksi Azure-blob paikalliset tiedot):

        {
            "name": "OutputBlobTable",
            "properties":
            {
                "location":
                {
                "type": "AzureBlobLocation",
                "folderPath": "containername",
                "format":
                {
                "type": "TextFormat",
                "columnDelimiter": "\t"
                },
                "linkedServiceName": "adfds"
                },
                "availability":
                {
                "frequency": "Day",
                "interval": 1
                }
            }
        }

Kopioi taulukko JSON määritykset tiedostoon, jota kutsutaan *bloboutputtabledef.json* tiedosto ja tallenna se käynnistyskansioon (tähän oletetaan olevan *C:\temp\bloboutputtabledef.json*). Luo taulukon SYÖTTÖ seuraavan Azure PowerShell cmdlet-komennon avulla seuraavasti:

    New-AzureDataFactoryTable -ResourceGroupName adfdsprg -DataFactoryName adfdsp -File C:\temp\bloboutputtabledef.json  

###<a name="adf-table-azure-sq"></a>SQL Azure-taulukosta
SQL Azure taulukosta määrittelyn tulosteen (tämän mallin yhdistää blob tulevat tiedot) seuraavat ominaisuudet:

    {
        "name": "OutputSQLAzureTable",
        "properties":
        {
            "structure":
            [
                { "name": "column1", type": "String"},
                { "name": "column2", type": "String"}                
            ],
            "location":
            {
                "type": "AzureSqlTableLocation",
                "tableName": "your_db_name",
                "linkedServiceName": "adfdssqlazure_linked_servicename"
            },
            "availability":
            {
                "frequency": "Day",
                "interval": 1            
            }
        }
    }

Kopioi taulukko JSON määritykset tiedostoon, jota kutsutaan *AzureSqlTable.json* tiedosto ja tallenna se käynnistyskansioon (tähän oletetaan olevan *C:\temp\AzureSqlTable.json*). Luo taulukon SYÖTTÖ seuraavan Azure PowerShell cmdlet-komennon avulla seuraavasti:

    New-AzureDataFactoryTable -ResourceGroupName adfdsprg -DataFactoryName adfdsp -File C:\temp\AzureSqlTable.json  


##<a name="adf-pipeline"></a>Määrittää ja luoda putkijohto

Voit määrittää toimintoja, jotka kuuluvat putkijohto ja luoda putkijohto komentosarjalla seuraavasti. JSON-tiedostoa käytetään myyntijakso-ominaisuuksien määrittämiseen.

* Komentosarjan oletetaan, että **putkijohto nimi** on *AMLDSProcessPipeline*.
* Huomaa, että on määritetty voidaan suorittaa päivittäin ja käyttää oletusarvon suoritusaika työn (12 am UTC-ajan) myyntijakso Mittausjakso.

> [AZURE.NOTE]Ohjeiden avulla PowerShellin Azure määrittäminen ja luoda SYÖTTÖ putkijohto. Mutta tehtävä myös onnistuu Azure-portaalissa. Lisätietoja on kohdassa [Create and Suorita putkijohto](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#step-4-create-and-run-a-pipeline).

Käytä taulukon määritelmiä aiemmin, myyntijakso määritelmä SYÖTTÖ määritetään seuraavasti:

        {
            "name": "AMLDSProcessPipeline",
            "properties":
            {
                "description" : "This pipeline has one Copy activity that copies data from an on-premise SQL to Azure blob",
                 "activities":
                [
                    {
                        "name": "CopyFromSQLtoBlob",
                        "description": "Copy data from on-premise SQL server to blob",     
                        "type": "CopyActivity",
                        "inputs": [ {"name": "OnPremSQLTable"} ],
                        "outputs": [ {"name": "OutputBlobTable"} ],
                        "transformation":
                        {
                            "source":
                            {                               
                                "type": "SqlSource",
                                "sqlReaderQuery": "select * from nyctaxi_data"
                            },
                            "sink":
                            {
                                "type": "BlobSink"
                            }   
                        },
                        "Policy":
                        {
                            "concurrency": 3,
                            "executionPriorityOrder": "NewestFirst",
                            "style": "StartOfInterval",
                            "retry": 0,
                            "timeout": "01:00:00"
                        }       

                     },

                    {
                        "name": "CopyFromBlobtoSQLAzure",
                        "description": "Push data to Sql Azure",        
                        "type": "CopyActivity",
                        "inputs": [ {"name": "OutputBlobTable"} ],
                        "outputs": [ {"name": "OutputSQLAzureTable"} ],
                        "transformation":
                        {
                            "source":
                            {                               
                                "type": "BlobSource"
                            },
                            "sink":
                            {
                                "type": "SqlSink",
                                "WriteBatchTimeout": "00:5:00",             
                            }           
                        },
                        "Policy":
                        {
                            "concurrency": 3,
                            "executionPriorityOrder": "NewestFirst",
                            "style": "StartOfInterval",
                            "retry": 2,
                            "timeout": "02:00:00"
                        }
                     }
                ]
            }
        }

Kopioi tämä JSON määrittely putkijohto tiedoston nimeltä *pipelinedef.json* tiedosto ja tallenna se käynnistyskansioon (tähän oletetaan olevan *C:\temp\pipelinedef.json*). Luo putkijohto SYÖTTÖ seuraavan Azure PowerShell cmdlet-komennon avulla seuraavasti:

    New-AzureDataFactoryPipeline  -ResourceGroupName adfdsprg -DataFactoryName adfdsp -File C:\temp\pipelinedef.json

Varmista, että näet Azure perinteinen portaalissa SYÖTTÖ myyntijakso, näkyvät seuraavat (kun napsautat kaaviota)

![](media/machine-learning-data-science-move-sql-azure-adf/DJP1kji.png)


##<a name="adf-pipeline-start"></a>Käynnistä putkijohto
Putkijohto nyt voit suorittaa seuraavan komennon avulla:

    Set-AzureDataFactoryPipelineActivePeriod -ResourceGroupName ADFdsprg -DataFactoryName ADFdsp -StartDateTime startdateZ –EndDateTime enddateZ –Name AMLDSProcessPipeline

*Alkamispäivä* ja *enddate* parametriarvot on todellisia päivämääriä, joiden välillä haluat suorittaa putkijohto korvaa.

Putkijohto suoritetaan, kun sinun pitäisi nähdä säilö on valittu, blob yhden tiedoston päivässä näkyvät tiedot.

Huomaa, että olemme on hyödyntää ei toimintoja SYÖTTÖ putkien tietoihin soluissa. Lisätietoja siitä, miten voit tehdä tämän ja muita ominaisuuksia myöntämä SYÖTTÖ on [SYÖTTÖ-ohjeista](https://azure.microsoft.com/services/data-factory/).
