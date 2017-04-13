<properties 
    pageTitle="Luo tietojoukkoja Azure Data Factory | Microsoft Azure" 
    description="Opettele luomaan tietojoukkoja Azure Data Factory-esimerkkien, jotka käyttävät ominaisuuksien, kuten siirtymä- ja anchorDateTime."
    keywords="Luo tietojoukko-tietojoukko esimerkki, siirtymä-Esimerkki"
    services="data-factory" 
    documentationCenter="" 
    authors="sharonlo101" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/13/2016" 
    ms.author="shlo"/>

# <a name="datasets-in-azure-data-factory"></a>Azure Data Factory tietojoukkoja
Tässä artikkelissa kuvataan Azure Data Factory tietojoukkoja ja sisältää esimerkkejä, kuten siirtymä, anchorDateTime ja siirtymä/tyyli-tietokannat.

Kun luot tietojoukko, luot tiedot, joita haluat käsitellä osoitin. Tietoja käsitellään (i/o) toimintaa ja aktiviteetin sisältyy putkijohto. Kirjoita tietojoukon edustaa putkijohto toimintaa syötteen ja tulostus-tietojoukko toiminnon tulos.

Tietojoukkoja tunnistaa tietojen eri tietojen stores, kuten taulukoita, tiedostot, kansiot ja tiedostot. Kun olet luonut tietojoukko, voit käyttää putkijohto toimintojen kanssa. Tietojoukko voi olla esimerkiksi i /-tietojoukko kopioi tehtävän tai HDInsightHive-tehtävän. Azure portaalin tutustutaan ulkoasun putkistot ja tietojen syötteiden ja tulostaa. Yhdellä silmäyksellä näet kaikki yhteydet ja että putkistot riippuvuudet yli omista lähteistä, joten tiedät aina kun tiedot on peräisin ja missä se suorittaminen.

Azure Data Factory-saat tietoja tietojoukko käyttämällä Kopioi tehtävän putkijohto.

> [AZURE.NOTE] Jos ole aiemmin käyttänyt Azure Data Factory-artikkelissa [Azure Data Factory esittely](data-factory-introduction.md) Azure Data Factory-palvelun yleiskatsaus. Katso [luominen ensimmäisen tiedot-factory](data-factory-build-your-first-pipeline.md) varten opetusohjelman ensimmäisen tiedot-factory luomiseen. Kaksi näissä artikkeleissa on taustatietoja ymmärtämistä tämän artikkelin paremmin. 

## <a name="define-datasets"></a>Määritä tietojoukkoja
Azure Data Factory-tietojoukko määritellään seuraavasti: 


    {
        "name": "<name of dataset>",
        "properties": {
            "type": "<type of dataset: AzureBlob, AzureSql etc...>",
            "external": <boolean flag to indicate external data. only for input datasets>,
            "linkedServiceName": "<Name of the linked service that refers to a data store.>",
            "structure": [
                {
                    "name": "<Name of the column>",
                    "type": "<Name of the type>"
                }
            ],
            "typeProperties": {
                "<type specific property>": "<value>",
                "<type specific property 2>": "<value 2>",
            },
            "availability": {
                "frequency": "<Specifies the time unit for data slice production. Supported frequency: Minute, Hour, Day, Week, Month>",
                "interval": "<Specifies the interval within the defined frequency. For example, frequency set to 'Hour' and interval set to 1 indicates that new data slices should be produced hourly>"
            },
           "policy": 
            {      
            }
        }
    }

Seuraavassa taulukossa on kuvattu yllä JSON ominaisuudet:   

| Ominaisuus | Kuvaus | Pakollinen | Oletusarvo |
| -------- | ----------- | -------- | ------- |
| Nimi | Tietojoukon nimi. Katso [Azure tietojen Factory - nimeäminen säännöt](data-factory-naming-rules.md) nimeämiskäytäntö säännöt. | Kyllä | PUUTTUU |
| tyyppi | Tietojoukon tyyppi. Määritä yksi Azure Data Factory tukemat tiedostotyypit (esimerkiksi: AzureBlob, AzureSqlTable). <br/><br/>Katso lisätietoja [Tietojoukko tyyppi](#Type) . | Kyllä | PUUTTUU |
| rakenne | Rakenne tietojoukko<br/><br/>Lisätietoja on artikkelissa [Tietojoukko rakenteen](#Structure) osa. | Ei. | PUUTTUU |
| typeProperties | Valitun tyyppiä vastaava ominaisuudet. Lisätietoja on [Tietojoukko tyyppi](#Type) -osassa tiedot tuetuista tyypeistä ja niiden ominaisuuksista. | Kyllä | PUUTTUU |
| Ulkoinen | Totuusarvo lippu, voit määrittää, onko tietojoukko erikseen yhdistämällä luotuja tietoja factory putkijohto, vai ei.  | Ei | EPÄTOSI | 
| käytettävyys | Määrittää käsittely-ikkuna tai tietojoukko tuotannon slicing malli. <br/><br/>Lisätietoja on artikkelissa [Tietojoukko käytettävyys](#Availability) osa. <br/><br/>Lisätietoja mallin, katso [ajoitus ja suorittamisen](data-factory-scheduling-and-execution.md) artikkelissa viipaloiminen tietojoukko. | Kyllä | PUUTTUU
| käytäntö | Määrittää ehdot tai ehto, joka tietojoukko sektorit on täytettävä. <br/><br/>Lisätietoja on artikkelissa [Tietojoukko käytännön](#Policy) osa. | Ei | PUUTTUU |

## <a name="dataset-example"></a>Esimerkki tietojoukko
Seuraavassa esimerkissä dataset edustaa taulukon **MyTable** **Azure SQL-tietokantaan**. 

    {
        "name": "DatasetSample",
        "properties": {
            "type": "AzureSqlTable",
            "linkedServiceName": "AzureSqlLinkedService",
            "typeProperties": 
            {
                "tableName": "MyTable"
            },
            "availability": 
            {
                "frequency": "Day",
                "interval": 1
            }
        }
    }

Huomaa seuraavat seikat: 

- tyyppi-asetukseksi on määritetty AzureSqlTable.
- taulukon nimi tyyppi-ominaisuuden (tietyn AzureSqlTable tyyppi) on määritetty MyTable.
- linkedServiceName viittaa tyypin AzureSqlDatabase linkitetyn palvelu. Katso seuraavat linkitetyn palvelun määritelmä. 
- käytettävyys korkojakso on määritetty päivä ja väli on määritetty 1, mikä tarkoittaa, että on valmistettu sektoria päivittäin.  

AzureSqlLinkedService määritellään seuraavasti:

    {
        "name": "AzureSqlLinkedService",
        "properties": {
            "type": "AzureSqlDatabase",
            "description": "",
            "typeProperties": {
                "connectionString": "Data Source=tcp:<servername>.database.windows.net,1433;Initial Catalog=<databasename>;User ID=<username>@<servername>;Password=<password>;Integrated Security=False;Encrypt=True;Connect Timeout=30"
            }
        }
    }

Yllä JSON-muodossa: 

- tyyppi-asetukseksi on määritetty AzureSqlDatabase
- connectionString tyypiksi tieto yhdistäminen Azure SQL-tietokantaan.  


Kuten näet, linkitetyn palvelun määrittää yhteyden muodostamisesta Azure SQL-tietokantaan. Tietojoukon määrittää, mitä taulukkoa käytetään i/o putkijohto tehtävän. Oman [myyntijakso](data-factory-create-pipelines.md) JSON tehtävän-osa määrittää, onko dataset käytetään syöttö- tai tietojoukko.


> [AZURE.IMPORTANT] Ellei Azure Data Factory yhdistämällä tietojoukko, se merkitään **ulkoisen**. Tämä asetus koskee yleensä syötteiden putkijohto ensimmäisen toimista.   

## <a name="Type"></a>Tietojoukon tyyppi
Tuetut tietolähteet ja tietojoukko tyypit tasataan. Lisätietoja tyypit ja tietojoukkoja määrittäminen [Tietojen siirtämistä toiminnot](data-factory-data-movement-activities.md#supported-data-stores) on artikkelissa Viitattu ohjeaiheissa. Esimerkiksi jos käytät Azure SQL-tietokannan tietojen, tuettuja stores Saat lisätietoja luettelo napsauttamalla Azure SQL-tietokantaan.  

## <a name="Structure"></a>Tietojoukon rakenne
**Rakenne** -osa määrittää dataset rakenne. Esityksessä on kokoelma nimet ja sarakkeiden tietotyyppien.  Seuraavassa esimerkissä dataset on kolme saraketta slicetimestamp kohtien ProjektinNimi ja pageviews ja ne ovat tyyppi: merkkijonon, merkkijonon ja desimaali tarpeen mukaan.

    structure:  
    [ 
        { "name": "slicetimestamp", "type": "String"},
        { "name": "projectname", "type": "String"},
        { "name": "pageviews", "type": "Decimal"}
    ]

## <a name="Availability"></a>Tietojoukon käytettävyys
Tietojoukko **käytettävyys** -osassa määrittää käsittely-ikkunassa (hourly, päivittäin, viikoittain jne.) tai tietojoukon slicing mallista. Katso [ajoitus ja suorittamisen](data-factory-scheduling-and-execution.md) artikkelissa lisätietoja tietojoukko viipaloiminen ja riippuvuuden malli. 

Käytettävyys-osassa määrittää, että tulostus-tietojoukko on valmistettu hourly (tai) syötteen tietojoukko on käytettävissä kerran tunnissa:

    "availability": 
    {   
        "frequency": "Hour",        
        "interval": 1   
    }

Seuraavassa taulukossa on kuvattu ominaisuuksista, joita voit käyttää käytettävyys-osassa: 

| Ominaisuus | Kuvaus | Pakollinen | Oletusarvo |
| -------- | ----------- | -------- | ------- |
| taajuus | Määrittää tietojoukko sektoria tuotannon aikayksikkö.<br/><br/>**Tuetut korkojakso**: minuutti, tunti, päivä, viikko, kuukausi | Kyllä | PUUTTUU |
| väli | Määrittää, kuinka usein kerroin<br/><br/>"X toistumisvälin" määrittää, kuinka usein sektoria on valmistettu.<br/><br/>Jos tarvitset dataset, leikatut tunnissa, voit määrittää **korkojakso** **Tunti**ja **väli** **1**.<br/><br/>**Huomautus:** Jos määrität korkojakso minuutti, on suositeltavaa määrittää aikavälin pienempi kuin 15 | Kyllä | PUUTTUU |
| tyyli | Määrittää, onko sektoria korkeiden välin Aloita/Lopeta.<ul><li>StartOfInterval</li><li>EndOfInterval</li></ul><br/><br/>Jos korkojakso on määritetty kuukausi ja tyyli on määritetty EndOfInterval-sektori on valmistettu kuukauden viimeinen päivä. Jos haluamasi tyyli on määritetty StartOfInterval, sektoria on valmistettu kuukauden ensimmäiselle päivälle.<br/><br/>Jos korkojakso on määritetty päivä ja tyyli on määritetty EndOfInterval, sektoria on valmistettu päivän viimeinen tunnissa.<br/><br/>Jos korkojakso on määritetty tunti ja tyyli on määritetty EndOfInterval, sektoria on valmistettu tunnin lopussa. Esimerkiksi sektoria 1 PM – 2 PM ajaksi, saat sektoria on valmistettu kello 2. | Ei | EndOfInterval |
| anchorDateTime | Määrittää käyttämän ajoituksen laskemiseen tietojoukko sektoria rajat suora sijainti. <br/><br/>**Huomautus:** Jos AnchorDateTime on päivämäärä-osat, jotka ovat eritellympiä kuin taajuus eritellympiä osat ohitetaan. <br/><br/>Jos **väli** on **kerran tunnissa** esimerkiksi (taajuus: tunti ja väli: 1) ja **AnchorDateTime** sisältää **minuutit ja sekunnit**ja valitse AnchorDateTime **minuutit ja sekunnit** osat ohitetaan. | Ei | 01/01/0001 |
| Siirtymä | Aikajakson, jolla siirtyvät alkamis- ja kaikki tietojoukko sektorit loppuun. <br/><br/>**Huomautus:** Jos anchorDateTime ja siirtymä määritetä, tulos on yhdistetty VAIHTO. | Ei | PUUTTUU |

### <a name="offset-example"></a>Siirtymä-Esimerkki

Päivittäinen slices, jotka käynnistyvät kello 6 oletusarvon keskiyön sijaan. 

    "availability":
    {
        "frequency": "Day",
        "interval": 1,
        "offset": "06:00:00"
    }

**Korkojakso** on määritetty **päivä** ja **väli** on määritetty **1** (kerran päivässä): Voit halutessasi 6: 00 sijaan oletusarvon milloin korkeiden sektoria: 12 AM. Muista, että tällä hetkellä on UTC-aika. 

## <a name="anchordatetime-example"></a>anchorDateTime Esimerkki

**Esimerkki:** 23 tuntia tietojoukko sektoreiden, jotka alkavat 2007-04-19T08:00:00

    "availability": 
    {   
        "frequency": "Hour",        
        "interval": 23, 
        "anchorDateTime":"2007-04-19T08:00:00"  
    }

## <a name="offsetstyle-example"></a>Siirtymä tai tyyliä Esimerkki

Jos tarvitset kuukausittain-tietojoukko tietyn päivämäärän ja kellonajan (oletetaan kuukausittain 8:00 Suomen 3rd), käytä **Siirtymä** -tunniste, Määritä päivämäärä ja aika, se pitäisi toimia. 

    {
      "name": "MyDataset",
      "properties": {
        "type": "AzureSqlTable",
        "linkedServiceName": "AzureSqlLinkedService",
        "typeProperties": {
          "tableName": "MyTable"
        },
        "availability": {
          "frequency": "Month",
          "interval": 1,
          "offset": "3.08:10:00",
          "style": "StartOfInterval"
        }
      }
    }


## <a name="Policy"></a>Tietojoukon käytäntö

Tietojoukon määrityksessä **käytäntö** -osa määrittää ehdot tai ehto, joka tietojoukko sektorit on täytettävä.

### <a name="validation-policies"></a>Vahvistus käytännöt

| Käytännön nimi | Kuvaus | Käytetty | Pakollinen | Oletusarvo |
| ----------- | ----------- | ---------- | -------- | ------- |
| minimumSizeMB | Tarkistaa, että tiedot **Azure blob** vastaa vähimmäiskoko (megatavuina). | Azure-Blob | Ei | PUUTTUU |
|minimumRows | Tarkistaa, että tiedot **Azure SQL-tietokantaan** tai aiemmin **Azuren taulukkojen** sisältää rivien vähimmäismäärä. | <ul><li>Azure SQL-tietokanta</li><li>Azure-taulukosta</li></ul> | Ei | PUUTTUU

#### <a name="examples"></a>Esimerkkejä

**minimumSizeMB:**

    "policy":
    
    {
        "validation":
        {
            "minimumSizeMB": 10.0
        }
    }

**minimumRows**

    "policy":
    {
        "validation":
        {
            "minimumRows": 100
        }
    }

### <a name="external-datasets"></a>Ulkoinen tietojoukkoja

Ulkoinen tietojoukkoja ovat oikeat, joka ei ole valmistettu käynnissä myyntijakso tietojen factory. Jos dataset on merkitty **ulkoisen**, **ExternalData** -käytäntö voidaan määrittää vaikuttaa tietojoukko sektoria käytettävyys toimintaa. 

Ellei Azure Data Factory yhdistämällä tietojoukko, se merkitään **ulkoisen**. Tämä asetus koskee yleensä ensimmäinen tehtävä putkijohto-syötteiden paitsi aktiviteetti tai myyntijakso ketjutus käytetään. 

| Nimi | Kuvaus | Pakollinen | Oletusarvo  |
| ---- | ----------- | -------- | -------------- |
| dataDelay | Tarkista tietyn sektoria ulkoisten tietojen saatavuus ajan. Jos tietojen pitäisi olla käytettävissä kerran tunnissa, ulkoiset tiedot ovat käytettävissä ja vastaavan sektori on valmis Tarkista lykätä esimerkiksi dataDelay avulla.<br/><br/>Koskee vain tällä hetkellä.  Esimerkiksi jos tämä arvo on 10 minuuttia on 1:00 PM juuri nyt, vahvistus alkaa 1:10 PM.<br/><br/>Tämä asetus ei vaikuta sektoreiden aiemman (sektoreiden sektoria päättymisaika + dataDelay-< nyt) käsitellään välittömästi.<br/><br/>Aika on suurempi kuin 23:59 tuntia on määritetty day.hours:minutes:seconds-muodossa. Esimerkiksi jos haluat määrittää 24 tuntia, älä käytä 24:00:00 Käytä 1.00:00:00. Jos käytät 24:00:00, sitä kohdellaan 24 päivää (24.00:00:00). Määritä yksi päivä ja 4 tuntia, 1:04:00:00. | Ei | 0 |
| retryInterval | Odotusaika epäonnistui ja seuraavan uudelleen yrityksellä. Koskee tällä hetkellä; Jos edellisen kokeilla epäonnistui, on odotettava tämä pitkään viimeisen yritä jälkeen. <br/><br/>1:00 pm tällä hetkellä on on aloittaa ensimmäisellä yrityksellä. Jos ensimmäisen kelpoisuuden tarkistuksen suorittamiseen kesto on 1 minuutti ja toiminto epäonnistui, seuraava uudelleen on 1:00 + 1 min (kesto) + 1min (väli) = 1:02 pm. <br/><br/>Aiemman sektoreiden on ei viiveen. Yritä tapahtuu heti. | Ei | 00:01:00 (1 minuutti) | 
| retryTimeout | Yritä jokaisen yrityksen aikakatkaisu.<br/><br/>Jos tämä ominaisuus on määritetty 10 minuutin, vahvistus on tarkoitus valmistua 10 minuutin kuluessa. Jos kestää kauemmin kuin 10 minuutin vahvistusta, yritä aikakatkaistaan.<br/><br/>Jos kaikki yritykset kelpoisuuden aikakatkaistaan sektoria on merkitty aikakatkaisu. | Ei | 00:10:00 (10 minuuttia) |
| maximumRetry | Tarkista saatavuus ulkoisten tietojen määrä. Suurin sallittu arvo on 10. | Ei | 3 | 

## <a name="scoped-datasets"></a>Kohdistetuissa tietojoukkoja
Voit luoda tietojoukkoja, joka on rajattu putkijohto **tietojoukkoja** -ominaisuuden avulla. Nämä tietojoukkoja voidaan käyttää vain tässä myyntijakso toimintoja, mutta muiden putkistot toimintojen mukaan. Seuraava esimerkki määrittää putkijohto kaksi tietojoukkoja - InputDataset Etätyöpöytäyhteys ja OutputDataset-Etätyöpöytäyhteys – käytettäviä putkijohto kanssa:  

> [AZURE.IMPORTANT] Kohdistetuissa tietojoukkoja tuetaan vain yhden kerran putkistot (**pipelineMode** **OneTime**asettaminen). Katso lisätietoja [Onetime myyntijakso](data-factory-scheduling-and-execution.md#onetime-pipeline) .

    {
        "name": "CopyPipeline-rdc",
        "properties": {
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "BlobSource",
                            "recursive": false
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "InputDataset-rdc"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "OutputDataset-rdc"
                        }
                    ],
                    "scheduler": {
                        "frequency": "Day",
                        "interval": 1,
                        "style": "StartOfInterval"
                    },
                    "name": "CopyActivity-0"
                }
            ],
            "start": "2016-02-28T00:00:00Z",
            "end": "2016-02-28T00:00:00Z",
            "isPaused": false,
            "pipelineMode": "OneTime",
            "expirationTime": "15.00:00:00",
            "datasets": [
                {
                    "name": "InputDataset-rdc",
                    "properties": {
                        "type": "AzureBlob",
                        "linkedServiceName": "InputLinkedService-rdc",
                        "typeProperties": {
                            "fileName": "emp.txt",
                            "folderPath": "adftutorial/input",
                            "format": {
                                "type": "TextFormat",
                                "rowDelimiter": "\n",
                                "columnDelimiter": ","
                            }
                        },
                        "availability": {
                            "frequency": "Day",
                            "interval": 1
                        },
                        "external": true,
                        "policy": {}
                    }
                },
                {
                    "name": "OutputDataset-rdc",
                    "properties": {
                        "type": "AzureBlob",
                        "linkedServiceName": "OutputLinkedService-rdc",
                        "typeProperties": {
                            "fileName": "emp.txt",
                            "folderPath": "adftutorial/output",
                            "format": {
                                "type": "TextFormat",
                                "rowDelimiter": "\n",
                                "columnDelimiter": ","
                            }
                        },
                        "availability": {
                            "frequency": "Day",
                            "interval": 1
                        },
                        "external": false,
                        "policy": {}
                    }
                }
            ]
        }
    }