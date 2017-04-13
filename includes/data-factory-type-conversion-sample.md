### <a name="type-conversion-sample"></a>Kirjoita muuntaminen malli
Seuraavassa esimerkissä on kopioimalla tiedot Blob Azure SQL tyyppi muunnosten kanssa.

Oletetaan, että Blob-objektien tietojoukkoa on CSV-muodossa ja se on 3 saraketta. Jonkin niistä on datetime-sarake, jossa mukautetun datetime-muodossa, käyttämällä viikonpäivän lyhennetty Ranskan nimet.

Voit määrittää seuraavasti sekä määritelmät sarakkeille Blob lähde-tietojoukko.

    {
        "name": "AzureBlobTypeSystemInput",
        "properties":
        {
             "structure": 
              [
                    { "name": "userid", "type": "Int64"},
                    { "name": "name", "type": "String"},
                    { "name": "lastlogindate", "type": "Datetime", "culture": "fr-fr", "format": "ddd-MM-YYYY"}
              ],
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/myfolder",
                "fileName":"myfile.csv",
                "format":
                {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                }
            },
            "external": true,
            "availability":
            {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "externalData": {
                    "retryInterval": "00:01:00",
                    "retryTimeout": "00:10:00",
                    "maximumRetry": 3
                }
            }
        }
    }

Annettu SQL .NET tyyppi vastaavuustaulukon yläpuolella, jonka tyyppi määrittää Azure SQL-taulukko, jossa seuraavan rakenteen.

| Sarakkeen nimi | SQL-tyyppi |
| ----------- | -------- |
| käyttäjätunnus | bigint |
| Nimi | teksti |
| lastlogindate | Päivämäärä ja aika |

Seuraavaksi Azure SQL-tietojoukko määrittää seuraavasti. Huomautus: Sinun ei tarvitse määrittää "rakenteen-osan tiedot, koska tiedot on jo määritetty pohjana tietovaraston.

    {
        "name": "AzureSQLOutput",
        "properties": {
            "type": "AzureSqlTable",
            "linkedServiceName": "AzureSqlLinkedService",
            "typeProperties": {
                "tableName": "MyTable"
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }

Tässä tapauksessa tietojen factory automaattisesti tehdä muunnokset, mukaan lukien Datetime-kenttää, jonka mukautetun datetime muotoileminen avulla f f maa-asetus, kun tietojen siirtäminen Azure SQL Blob tyyppi.


