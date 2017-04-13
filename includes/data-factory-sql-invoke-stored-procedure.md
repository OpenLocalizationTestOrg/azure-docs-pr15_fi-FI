## <a name="invoking-stored-procedure-for-sql-sink"></a>Määräytyvät tallennetun toimintosarjan SQL allas

Kun kopioit tietoja SQL Server tai Azure SQL-tai SQL Server-tietokantaan, käyttäjän määritetty tallennettu toimintosarja on määritetty ja kutsua muut parametrit. 

Tallennetun toimintosarjan voit hyödyntää, kun valmiin kopioi järjestelmiä ei yhteyshenkilönä aihe. Tämä on yleensä hyödyntää käsiteltäessä ylimääräisiä (sarakkeen, muita arvoja, kohdistin... useita taulukoiksi hakeminen yhdistäminen) on tehtävä ennen lähdetietojen kohdetaulukkoon lopullinen lisäämistä. 

Tallennetun toimintosarjan värisinä voi käynnistää. Seuraavassa esimerkissä esitetään, kuinka voit tallennetun toimintosarjan avulla voit tehdä yksinkertaisia kohdistimen tietokannan taulukkoon. 

**Tulostus-tietojoukko**

Tässä esimerkissä tyyppi on määritetty: SqlServerTable. Määrittää sen käytettäväksi Azure SQL-tietokantaan AzureSqlTable. 

    {
      "name": "SqlOutput",
      "properties": {
        "type": "SqlServerTable",
        "linkedServiceName": "SqlLinkedService",
        "typeProperties": {
          "tableName": "Marketing"
        },
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }
    
Määritä SqlSink-osassa kopioi tehtävän JSON seuraavasti. Soita tallennetun toimintosarjan tietojen lisääminen ja tarvitaan SqlWriterStoredProcedureName ja SqlWriterTableType ominaisuudet.

    "sink":
    {
        "type": "SqlSink",
        "SqlWriterTableType": "MarketingType",
        "SqlWriterStoredProcedureName": "spOverwriteMarketing", 
        "storedProcedureParameters":
                {
                    "stringData": 
                    {
                        "value": "str1"     
                    }
                }
    }

Tietokannan Määritä tallennetun toimintosarjan saman niminen SqlWriterStoredProcedureName. Se käsittelee määritettyä lähde-ja lisää kenttään annettavat tiedot tulostus-taulukkoon. Huomaa, että tallennetun toimintosarjan parametrin nimen tulisi olla sama kuin määritetty taulukon JSON tiedoston nimi.

    CREATE PROCEDURE spOverwriteMarketing @Marketing [dbo].[MarketingType] READONLY, @stringData varchar(256)
    AS
    BEGIN
        DELETE FROM [dbo].[Marketing] where ProfileID = @stringData
        INSERT [dbo].[Marketing](ProfileID, State)
        SELECT * FROM @Marketing
    END

Tietokannan Määritä samanniminen taulukko-tyypin SqlWriterTableType. Huomaa, että haluamasi taulukko rakenne on oltava sama kuin syötetietoja palauttama rakenne.

    CREATE TYPE [dbo].[MarketingType] AS TABLE(
        [ProfileID] [varchar](256) NOT NULL,
        [State] [varchar](256) NOT NULL
    )

Tallennettu toimintosarja-ominaisuus hyödyntää [Table-Valued parametrit](https://msdn.microsoft.com/library/bb675163.aspx).