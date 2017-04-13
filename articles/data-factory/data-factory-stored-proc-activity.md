<properties 
    pageTitle="SQL Serverin tallennettu toimintosarja tehtävä" 
    description="Tietoja siitä, miten voit käyttää SQL Serverin tallennettu toimintosarja tehtävän käynnistää tallennetun toimintosarjan Azure SQL-tietokanta tai SQL Azure-tietovarasto Data Factory myyntijakso." 
    services="data-factory" 
    documentationCenter="" 
    authors="spelluru" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/30/2016" 
    ms.author="spelluru"/>

# <a name="sql-server-stored-procedure-activity"></a>SQL Serverin tallennettu toimintosarja tehtävä
> [AZURE.SELECTOR]
[Rakenne](data-factory-hive-activity.md)  
[Possu](data-factory-pig-activity.md)  
[MapReduce](data-factory-map-reduce.md)  
[Hadoop Streaming](data-factory-hadoop-streaming-activity.md)
[Machine Learning](data-factory-azure-ml-batch-execution-activity.md) 
[Tallennettu toimintosarja](data-factory-stored-proc-activity.md)
[Tietojen järvi Analytics U-SQL](data-factory-usql-activity.md)
[.NET mukautettu](data-factory-use-custom-activities.md)

Data Factory [myyntijakso](data-factory-create-pipelines.md) SQL Serverin tallennettu toimintosarja-toiminnon avulla voit käynnistää tallennetun toimintosarjan jollakin seuraavista tietoja tallennetuista tiedoista: 


- Azure SQL-tietokanta 
- Azure SQL-tietovarasto  
- SQL Server-tietokannan yrityksen tai Azure-AM. Sinun täytyy asentaa Data Management Gateway samaan tietokoneeseen, joka isännöi tietokannan tai voit välttää resurssien tietokannassa pikaluistelukilpailuissa eri tietokoneeseen. Data Management Gateway on ohjelmisto, joka yhdistää paikallisen lähteistä/tietojen tietolähteiden hajalla Azure VMs-yhteyttä pilvipalveluihin turvallisemman ja hallitun tavalla. Lisätietoja Data Management Gateway on artikkelissa [paikallisen ja cloud tietojen siirtäminen](data-factory-move-data-between-onprem-and-cloud.md) artikkelissa. 

Tässä artikkelissa perustuu [tietojen muunnos tehtävät](data-factory-data-transformation-activities.md) -artikkelista, jossa näkyy yleiskatsaus muuntamiseen ja tuettujen muunnos-toimintoja.

## <a name="walkthrough"></a>Vaiheittainen kuvaus

### <a name="sample-table-and-stored-procedure"></a>Esimerkkitaulukko ja tallennettu toimintosarja
1. Luo Azure SQL-tietokannan käyttäminen SQL Server Management Studiossa tai muuta työkalua, olet perehtynyt siihen, seuraavassa **taulukossa** . Datetimestamp sarake on päivämäärä ja kellonaika, kun vastaavan tunnus luodaan. 

        CREATE TABLE dbo.sampletable
        (
            Id uniqueidentifier,
            datetimestamp nvarchar(127)
        )
        GO

        CREATE CLUSTERED INDEX ClusteredID ON dbo.sampletable(Id);
        GO

    Tunnus on yksilöllinen tunnistaa ja datetimestamp sarake on päivämäärä ja kellonaika, kun vastaavan tunnus luodaan.
    ![Mallitiedot](./media/data-factory-stored-proc-activity/sample-data.png)

    > [AZURE.NOTE] Tässä esimerkissä käytetään Azure SQL-tietokanta, mutta se toimii samalla tavalla Azure SQL-tietovarasto tai SQL Server-tietokantaan. 
2. Luo seuraavat **tallennetun toimintosarjan** , joka lisää tietojen **sampletable**.

        CREATE PROCEDURE sp_sample @DateTime nvarchar(127)
        AS
        
        BEGIN
            INSERT INTO [sampletable]
            VALUES (newid(), @DateTime)
        END

    > [AZURE.IMPORTANT] **Nimi** ja **kannen** parametrin (Tässä esimerkissä DateTime) on oltava samat, joka on määritetty putkijohto ja tehtävän JSON parametri. Tallennettu toimintosarja-määrityksessä että **@** käytetään etuliitteenä parametrin.
    
### <a name="create-a-data-factory"></a>Tietoja factory luominen  
4. Kirjaudu sisään [Azure](https://portal.azure.com/)-portaaliin. 
5. Valitse vasemmalla olevasta valikosta **Uusi** , valitse **liiketoimintatietojen + Analytics**ja sitten **Data Factory**.
    
    ![Uusien tietojen factory](media/data-factory-stored-proc-activity/new-data-factory.png)   
4.  Kirjoita nimi **SProcDF** **uudet tiedot factory** -sivu. Azure Data Factory nimet ovat **GUID**. Sinun täytyy etuliite nimesi tehdas onnistuneen luomisen tietojen factory nimi.

    ![Uusien tietojen factory](media/data-factory-stored-proc-activity/new-data-factory-blade.png)      
3.  Valitse **Azure tilauksen**. 
4.  **Resurssiryhmä**-jokin seuraavista toimista: 
    1.  Valitse **Luo uusi** ja kirjoita resurssiryhmän nimi.
    2.  Valitse **Käytä aiemmin** ja valitse aiemmin luotu resurssiryhmä.  
5.  Valitse tiedot-factory **sijainti** .
6.  Valitse **raporttinäkymät-ikkunan kiinnittäminen** , jotta näet tietojen factory koontinäytössä kirjaudut sisään seuraavan kerran. 
6.  Valitse **Luo** **Uusi tietojen factory** -sivu.
6.  Näet luomisen **raporttinäkymät-ikkunan** Azure-portaalin tietojen factory. Kun tietojen tehdas on luotu, näyttöön tulee tietojen Tehdas-sivun, joka näyttää tiedot factory sisällön.
    ![Tietoja Factory-kotisivu](media/data-factory-stored-proc-activity/data-factory-home-page.png)

### <a name="create-an-azure-sql-linked-service"></a>Luo linkitetty Azure SQL-palvelu  
Kun olet luonut tietojen factory, Luo linkitetty Azure SQL-palvelu, joka linkittää Azure SQL-tietokannan tietojen factory. Tämä tietokanta sisältää sampletable taulukko ja sp_sample tallennetun toimintosarjan.

7.  Valitse **tekijän ja ota käyttöön** - **SProcDF** käynnistää tietojen Factory-editorin for **Data Factory** -sivu.
2.  Napsauta komentopalkin **tallentaa uudet tiedot** ja valitse **Azure SQL-tietokantaan**. Raportissa pitäisi näkyä JSON komentosarjan luominen linkitetty Azure SQL-palvelun editorissa. 

    ![Uusi tietosäilö](media/data-factory-stored-proc-activity/new-data-store.png)
4. JSON-komentosarjan avulla tee seuraavat muutokset: 
    1. Korvaa ** &lt;palvelimen nimi&gt; ** Azure SQL-tietokanta-palvelimesi nimi.
    2. Korvaa ** &lt;nimi&gt; ** tietokannassa, johon olet luonut taulukon ja tallennettu toimintosarja.
    3. Korvaa ** &lt; username@servername ** kanssa käyttäjätili, jolla on pääsy tietokantaan.
    4. Korvaa ** &lt;salasanan&gt; ** käyttäjätilin salasanalla. 

    ![Uusi tietosäilö](media/data-factory-stored-proc-activity/azure-sql-linked-service.png)
5. Valitse **Ota käyttöön** komentopalkista linkitetyn palvelun käyttöön. Vahvista, näet AzureSqlLinkedService puunäkymän vasemmalla. 

    ![puunäkymän linkitetyn-palvelun kanssa](media/data-factory-stored-proc-activity/tree-view.png)

### <a name="create-an-output-dataset"></a>Tulostus-tietojoukon luominen
6. Valitse **... Lisää** Valitse työkaluriviltä **Uusi tietojoukko**ja **Azure**SQL-lause. **Uusi tietojoukko** komentopalkki ja valitse **Azure SQL**.

    ![puunäkymän linkitetyn-palvelun kanssa](media/data-factory-stored-proc-activity/new-dataset.png)
7. Kopioi ja liitä seuraava JSON-komentosarja-JSON-editorin avulla.

        {               
            "name": "sprocsampleout",
            "properties": {
                "type": "AzureSqlTable",
                "linkedServiceName": "AzureSqlLinkedService",
                "typeProperties": {
                    "tableName": "sampletable"
                },
                "availability": {
                    "frequency": "Hour",
                    "interval": 1
                }
            }
        }
7. Valitse **Ota käyttöön** ottamaan dataset komentopalkista. Vahvista, näet tietojoukko puunäkymässä. 

    ![puunäkymän linkitetyn-palvelujen kanssa](media/data-factory-stored-proc-activity/tree-view-2.png)

### <a name="create-a-pipeline-with-sqlserverstoredprocedure-activity"></a>Luo putkijohto SqlServerStoredProcedure aktiviteettiin
Seuraavaksi luodaan SqlServerStoredProcedure aktiviteettiin putkijohto.
 
9. Valitse **... Lisää** -komento ja valitse **Uusi myyntijakso**. 
9. Kopioi ja liitä seuraava JSON koodikatkelman. **StoredProcedureName** arvoksi **sp_sample**. Nimi ja kannen **DateTime** -parametrin on vastattava nimi ja tallennettu toimintosarja määrityksessä parametrin kirjainkoko.  

        {
            "name": "SprocActivitySamplePipeline",
            "properties": {
                "activities": [
                    {
                        "type": "SqlServerStoredProcedure",
                        "typeProperties": {
                            "storedProcedureName": "sp_sample",
                            "storedProcedureParameters": {
                                "DateTime": "$$Text.Format('{0:yyyy-MM-dd HH:mm:ss}', SliceStart)"
                            }
                        },
                        "outputs": [
                            {
                                "name": "sprocsampleout"
                            }
                        ],
                        "scheduler": {
                            "frequency": "Hour",
                            "interval": 1
                        },
                        "name": "SprocActivitySample"
                    }
                ],
                "start": "2016-08-02T00:00:00Z",
                "end": "2016-08-02T05:00:00Z",
                "isPaused": false
            }
        }

    Jos sinun on välitettävä null parametrin, käyttää seuraavaa syntaksia: "param1": null (pieniksi kirjaimiksi). 
9. Valitse **Ota käyttöön** käyttöönotto putkisto-painiketta.  

### <a name="monitor-the-pipeline"></a>Näytön putkijohto

6. Sulje tiedot Factory editorin lavat ja palata Data Factory-sivu **X** ja valitse **kaavio**.

    ![kaavio-ruutu](media/data-factory-stored-proc-activity/data-factory-diagram-tile.png)
7. **Kaavionäkymä**näet, putkistot ja käyttää tässä opetusohjelmassa tietojoukkoja. 

    ![kaavio-ruutu](media/data-factory-stored-proc-activity/data-factory-diagram-view.png)
8. Kaavionäkymässä Kaksoisnapsauta tietojoukko **sprocsampleout**. Näet sektorit Valmis-tilaan. Pitäisi olla viisi sektoreiden koska sektoria on valmistettu kunkin tunnin alkamisaika ja päättymisaika-JSON välillä.

    ![kaavio-ruutu](media/data-factory-stored-proc-activity/data-factory-slices.png) 
10. Kun sektoria on **valmiina** , suorita *sampletable *Valitse* ** kysely Azure SQL-tietokantaan voit varmistaa, että tiedot on lisätty taulukkoon tallennetun toimintosarjan mukaan.

    ![Siirtää tietoja](./media/data-factory-stored-proc-activity/output.png)

    Katso lisätietoja Azure Data Factory putkistot seuranta [näytön putkijohto](data-factory-monitor-manage-pipelines.md) .  

> [AZURE.NOTE] Tässä esimerkissä SprocActivitySample ei ole syötteitä. Jos haluat ketjut tässä aktiviteetti edeltävät aktiviteettiin (eli edellisen käsittely), seuraavat tehtävän tulostaa voidaan käyttää tässä aktiviteetti syötteiden. Siinä tapauksessa tämä toiminto ei suoriteta, ennen kuin seuraavat tehtävä on suoritettu ja tulostaa seuraavat toiminnot ovat käytettävissä (valmis-tila). Syötteiden ei voi käyttää suoraan parametrin tallennettu toimintosarja-toimintoon

## <a name="json-format"></a>JSON-muoto
    {
        "name": "SQLSPROCActivity",
        "description": "description", 
        "type": "SqlServerStoredProcedure",
        "inputs":  [ { "name": "inputtable"  } ],
        "outputs":  [ { "name": "outputtable" } ],
        "typeProperties":
        {
            "storedProcedureName": "<name of the stored procedure>",
            "storedProcedureParameters":  
            {
                "param1": "param1Value"
                …
            }
        }
    }

## <a name="json-properties"></a>JSON-ominaisuudet

Ominaisuus | Kuvaus | Pakollinen
-------- | ----------- | --------
Nimi | Tehtävän nimi | Kyllä
kuvaus | Kuvausteksti tehtävän käyttötarkoitus | Ei
tyyppi | SqlServerStoredProcedure | Kyllä
syötteiden | Valinnainen. Jos määrität syötteen tietojoukko, sen on oltava käytettävissä ('Valmis' tila) tallennetun toimintosarjan tehtävän suorittamiseen. Kirjoita tietojoukon sitä voi käyttää tallennetun toimintosarjan parametrina. Tarkista ennen aloittamista tallennetun toimintosarjan tehtävän riippuvuuden on vain käyttää. | Ei
tulostus | Määritä tulostus-tietojoukko tallennetun toimintosarjan tehtävän. Tulosteen tietojoukko määrittää **aikataulun** tallennetun toimintosarjan tehtävän (kerran tunnissa, viikoittain, kuukausittain, jne.). <br/><br/>Tulostus-tietojoukko käytettävä **linkitetyistä** , joka viittaa Azure SQL-tietokanta tai Azure SQL-tietovarasto tai SQL Server-tietokantaan, jossa haluat suorittaa tallennetun toimintosarjan. <br/><br/>Tulostus-tietojoukko voi toimia tapa välittää tallennetun toimintosarjan saatu myöhemmin toimintaa ([ketjutus toimintoja](data-factory-scheduling-and-execution.md#chaining-activities)) toisen putkijohto käsittelyä. Kuitenkin Data Factory ei automaattisesti kirjoittaa tallennetun toimintosarjan tulosteen tämä tietojoukko. Se on tallennettu toimintosarja, joka kirjoittaa SQL-taulukko, joka osoittaa tulostus-tietojoukko. <br/><br/>Joissakin tapauksissa tulostus-tietojoukko voi olla **Tyhjä tietojoukko**, jota käytetään vain, jos haluat määrittää aikataulun tallennetun toimintosarjan tehtävän suorittamista varten. | Kyllä
storedProcedureName | Määrittää tallennetun toimintosarjan nimen Azure SQL-tietokantaan tai Azure SQL-tietovarasto, joka edustaa linkitetyn palvelu, joka käyttää tulostusalue. | Kyllä
storedProcedureParameters | Määritä tallennetun toimintosarjan parametrien arvot. Jos haluat siirtää parametrin Null-arvoa, käyttää seuraavaa syntaksia: "param1": null (kaikki pienet kirjaimet). Katso seuraava näyte Saat lisätietoja tämän ominaisuuden avulla.| Ei

## <a name="passing-a-static-value"></a>Kulkeva staattinen arvo 
Nyt seuraavassa taulukossa, joka sisältää nimeltä 'Asiakirjan otoksen' staattisen arvon lisääminen nimeltä 'Skenaarion' toiseen sarakkeeseen.

![Mallitiedot 2](./media/data-factory-stored-proc-activity/sample-data-2.png)

    CREATE PROCEDURE sp_sample @DateTime nvarchar(127) , @Scenario nvarchar(127)
    
    AS
    
    BEGIN
        INSERT INTO [sampletable]
        VALUES (newid(), @DateTime, @Scenario)
    END

Nyt läpäistä skenaarion parametrin ja arvon tallennetun toimintosarjan tehtävästä. Edellisessä esimerkissä typeProperties-osa näyttää seuraavat koodikatkelman:

    "typeProperties":
    {
        "storedProcedureName": "sp_sample",
        "storedProcedureParameters": 
        {
            "DateTime": "$$Text.Format('{0:yyyy-MM-dd HH:mm:ss}', SliceStart)",
            "Scenario": "Document sample"
        }
    }

