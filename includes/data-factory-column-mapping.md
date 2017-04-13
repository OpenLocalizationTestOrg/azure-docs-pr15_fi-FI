## <a name="column-mapping-with-translator-rules"></a>Sarakkeiden yhdistämismäärityksen translator sääntöihin
Sarakkeiden yhdistämismäärityksen avulla voidaan määrittää, miten sarakkeita rakenteessa määritetty "" lähde-taulukon määrityksen sarakkeisiin käsittelytoiminto taulukon "rakenteen-parametrissa. **ColumnMapping** -ominaisuus on käytettävissä kopioi tehtävän **typeProperties** -osassa.

Sarakkeen yhdistäminen tukee seuraavia vaihtoehtoja:

- Kaikki lähdetaulukon "rakenteen-sarakkeet yhdistetään käsittelytoiminto taulukon"rakenteen-kaikki sarakkeet.
- Alijoukkoa lähdetaulukon "rakenteen-sarakkeet yhdistetään käsittelytoiminto taulukon"rakenteen-kaikki sarakkeet.

Seuraavassa ovat virhetilanteita ja aiheuttaa poikkeus:

- Joko vähemmän sarakkeita tai enemmän sarakkeita "" käsittelytoiminto taulukon rakennetta kuin määritetty määritystä.
- Kaksoiskappaleiden yhdistämismääritys.
- Sarakkeen nimi, joka on määritetty määritystä ei ole SQL-kyselyn tuloksen.

## <a name="column-mapping-samples"></a>Sarakkeen yhdistäminen objektit
> [AZURE.NOTE] Alla mallit ovat Azure SQL-ja Azure-Blob, mutta eivät koske mitään tietosäilö, joka tukee suorakulmainen tietojoukkoja. Sinun on Säädä tietojoukko ja esimerkkejä linkitetyistä määritteitä osoittamaan asiaa tietolähteen tiedot. 

### <a name="sample-1--column-mapping-from-azure-sql-to-azure-blob"></a>Esimerkki 1 – sarakkeen yhdistäminen-Azure SQL Azure-blob
Tässä esimerkissä syötteen taulukko rakenne on, ja se osoittaa taulukon SQL Azure SQL-tietokantaan.

    {
        "name": "AzureSQLInput",
        "properties": {
            "structure": 
             [
               { "name": "userid"},
               { "name": "name"},
               { "name": "group"}
             ],
            "type": "AzureSqlTable",
            "linkedServiceName": "AzureSqlLinkedService",
            "typeProperties": {
                "tableName": "MyTable"
            },
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

Tässä esimerkissä tulostustaulukon rakenne on, ja se osoittaa Blob-objektien Azuren Blob-objektien tallennustilaan.

    {
        "name": "AzureBlobOutput",
        "properties":
        {
             "structure": 
              [
                    { "name": "myuserid"},
                    { "name": "myname" },
                    { "name": "mygroup"}
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
            "availability":
            {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }

Tehtävän JSON näkyy alla. Yhdistetty käsittelytoiminto (**columnMappings**) sarakkeiden **Translator** -ominaisuuden avulla lähteestä sarakkeet.

    {
        "name": "CopyActivity",
        "description": "description", 
        "type": "Copy",
        "inputs":  [ { "name": "AzureSQLInput"  } ],
        "outputs":  [ { "name": "AzureBlobOutput" } ],
        "typeProperties":    {
            "source":
            {
                "type": "SqlSource"
            },
            "sink":
            {
                "type": "BlobSink"
            },
            "translator": 
            {
                "type": "TabularTranslator",
                "ColumnMappings": "UserId: MyUserId, Group: MyGroup, Name: MyName"
            }
        },
       "scheduler": {
              "frequency": "Hour",
              "interval": 1
            }
    }

**Sarakkeen yhdistäminen työnkulku:**

![Sarakkeen yhdistäminen työnkulku](./media/data-factory-data-stores-with-rectangular-tables/column-mapping-flow.png)

### <a name="sample-2--column-mapping-with-sql-query-from-azure-sql-to-azure-blob"></a>Esimerkki 2 – sarake SQL Query from Azure SQL Azure-blob yhdistäminen
Tässä esimerkissä käytetään SQL-kyselyn Poimi tiedot Azure SQL sijaan määrittäminen yksinkertaisesti "rakenteen-kohdassa taulukon nimen ja sarakkeiden nimet. 

    {
        "name": "CopyActivity",
        "description": "description", 
        "type": "CopyActivity",
        "inputs":  [ { "name": " AzureSQLInput"  } ],
        "outputs":  [ { "name": " AzureBlobOutput" } ],
        "typeProperties":
        {
            "source":
            {
                "type": "SqlSource",
                "SqlReaderQuery": "$$Text.Format('SELECT * FROM MyTable WHERE StartDateTime = \\'{0:yyyyMMdd-HH}\\'', WindowStart)"
            },
            "sink":
            {
                "type": "BlobSink"
            },
            "Translator": 
            {
                "type": "TabularTranslator",
                "ColumnMappings": "UserId: MyUserId, Group: MyGroup,Name: MyName"
            }
        },
        "scheduler": {
              "frequency": "Hour",
              "interval": 1
            }
    }

Tässä tapauksessa kyselytulokset yhdistetään ensin sarakkeet on määritetty "rakenteessa" lähde. Seuraavaksi lähteestä "rakenteen-sarakkeet yhdistetään käsittelytoiminto"rakenteen-sarakkeisiin säännöt määritellyt columnMappings kanssa.  Oletetaan, että kysely palauttaa 5 saraketta, kaksi sarakkeita sitten määritettyjä lähteen "rakenteen".

**Sarakkeen yhdistäminen työnkulku**

![Sarakkeen yhdistäminen työnkulku-2](./media/data-factory-data-stores-with-rectangular-tables/column-mapping-flow-2.png)







