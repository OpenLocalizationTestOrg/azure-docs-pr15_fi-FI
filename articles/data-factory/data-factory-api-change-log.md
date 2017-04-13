<properties 
    pageTitle="Tietoja Factory - .NET API muutosloki | Microsoft Azure" 
    description="Tässä artikkelissa kuvataan uusimmat muutokset, lisäykset ominaisuus, virheenkorjauksia muille... .NET Ohjelmointirajapinta Azure Data Factory tietyn versiossa." 
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
    ms.date="09/21/2016" 
    ms.author="spelluru"/>

# <a name="azure-data-factory---net-api-change-log"></a>Azure tietojen Factory - .NET API muutosloki 
Tässä artikkelissa on tietoja muutokset Azure tietojen Factory SDK tietyn versiossa. Voit etsiä uusinta NuGet paketin Azure Data Factory [tähän](https://www.nuget.org/packages/Microsoft.Azure.Management.DataFactories) 

## <a name="version-4110"></a>Version 4.11.0
Ominaisuuden lisäyksiä:

- Linkitetyn palvelun seuraavanlaisia on lisätty:
    - [OnPremisesMongoDbLinkedService](https://msdn.microsoft.com/library/mt765129.aspx)
    - [AmazonRedshiftLinkedService](https://msdn.microsoft.com/library/mt765121.aspx)
    - [AwsAccessKeyLinkedService](https://msdn.microsoft.com/library/mt765144.aspx)
- Tietojoukon seuraavanlaisia on lisätty: 
    - [MongoDbCollectionDataset](https://msdn.microsoft.com/library/mt765145.aspx)
    - [AmazonS3Dataset](https://msdn.microsoft.com/library/mt765112.aspx)
- Kopioi lähde seuraavanlaisia on lisätty:
    - [MongoDbSource](https://msdn.microsoft.com/en-US/library/mt765123.aspx)

## <a name="version-4100"></a>Version 4.10.0
- Seuraavia valinnaisia ominaisuuksia on lisätty TekstinMuoto:
    - [SkipLineCount](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.textformat.skiplinecount.aspx)
    - [Ensimmäinenriviotsikkona](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.textformat.firstrowasheader.aspx)
    - [TreatEmptyAsNull](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.textformat.treatemptyasnull.aspx)
- Linkitetyn palvelun seuraavanlaisia on lisätty:
    - [OnPremisesCassandraLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.onpremisescassandralinkedservice.aspx)
    - [SalesforceLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.salesforcelinkedservice.aspx)
- Tietojoukon seuraavanlaisia on lisätty:
    - [OnPremisesCassandraTableDataset](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.onpremisescassandratabledataset.aspx)
- Kopioi lähde seuraavanlaisia on lisätty:
    - [CassandraSource](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.cassandrasource.aspx)
- Lisää AzureMLBatchExecutionActivity [WebServiceInputs](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.azuremlbatchexecutionactivity.webserviceinputs.aspx) -ominaisuus 
    - Ota käyttöön kulkeva useita web palvelun syötteiden Azure koneen Learning kokeen


## <a name="version-491"></a>Version 4.9.1

### <a name="bug-fix"></a>Ohjelmavirhe korjaaminen

- Käytöstä [WebLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.weblinkedservice.authenticationtype.aspx)WebApi-pohjainen todentaminen.

## <a name="version-490"></a>Version 4.9.0

### <a name="feature-additions"></a>Ominaisuuden lisäykset

- Lisää CopyActivity [EnableStaging](https://msdn.microsoft.com/library/mt767916.aspx) ja [StagingSettings](https://msdn.microsoft.com/library/mt767918.aspx) ominaisuudet. Lisätietoja [vaiheistettu kopio](data-factory-copy-activity-performance.md#staged-copy) -toiminnon.


### <a name="bug-fix"></a>Ohjelmavirhe korjaaminen

- Esittele liikaa [ActivityWindowOperationExtensions.List](https://msdn.microsoft.com/library/mt767915.aspx) menetelmän, joka kestää [ActivityWindowsByActivityListParameters](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.activitywindowsbyactivitylistparameters.aspx) esiintymä.
- Merkitse [WriteBatchSize](https://msdn.microsoft.com/library/dn884293.aspx) ja [WriteBatchTimeout](https://msdn.microsoft.com/library/dn884245.aspx) valinnaisia CopySink.

## <a name="version-480"></a>Version 4.8.0

### <a name="feature-additions"></a>Ominaisuuden lisäykset
- Kopioi tehtävätyypin käyttöön kopioi suorituskyvyn säätö on lisätty seuraavia valinnaisia ominaisuuksia:
    - [ParallelCopies](https://msdn.microsoft.com/library/mt767910.aspx)
    - [CloudDataMovementUnits](https://msdn.microsoft.com/library/mt767912.aspx)

## <a name="version-470"></a>Version 4.7.0

### <a name="feature-additions"></a>Ominaisuuden lisäykset
* Lisätä uusia StorageFormat tyyppi [OrcFormat](https://msdn.microsoft.com/library/mt723391.aspx) tyyppi tiedostojen kopioiminen sarakemuodossa optimoitu rivi (ORC).
* Lisää SqlDWSink [AllowPolyBase](https://msdn.microsoft.com/library/mt723396.aspx) ja PolyBaseSettings ominaisuudet.
    * Mahdollistaa PolyBase kopioida tietoja SQL-tietovarasto.

## <a name="version-461"></a>Version 4.6.1

### <a name="bug-fixes"></a>Virheenkorjauksia
* Korjaa luettelon toiminta windows HTTP-pyynnön.
    * Poistaa pyynnön paketti resurssin ryhmänimi ja tiedot factory nimi.

## <a name="version-460"></a>Version 4.6.0

### <a name="feature-additions"></a>Ominaisuuden lisäykset

- [PipelineProperties](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.pipelineproperties_properties.aspx)on lisätty seuraavat ominaisuudet:
    - [PipelineMode](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.pipelineproperties.pipelinemode.aspx)
    - [ExpirationTime](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.pipelineproperties.expirationtime.aspx)
    - [Tietojoukkoja](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.pipelineproperties.datasets.aspx)
- [PipelineRuntimeInfo](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.common.models.pipelineruntimeinfo.aspx)on lisätty seuraavat ominaisuudet:
    - [PipelineState](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.common.models.pipelineruntimeinfo.pipelinestate.aspx)
- Lisätty uusi [StorageFormat](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.storageformat.aspx) type [JsonFormat](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.jsonformat.aspx) määrittämään tietojoukkoja, jonka tiedot on JSON-muodossa. 

## <a name="version-450"></a>Versio 4.5.0

### <a name="feature-additions"></a>Ominaisuuden lisäykset
* Lisätty [tehtävä-ikkunan toimintojen luettelo](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.activitywindowoperationsextensions.aspx).
    * Lisätty menetelmät hakemiseen toiminta windows suodattimilla kohde tiedostotyypit (eli tietojen tehtaan, tietojoukkoja, putkistot ja tehtävät) perusteella.
* Linkitetyn palvelun seuraavanlaisia on lisätty: 
    * [ODataLinkedService](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.odatalinkedservice.aspx), [WebLinkedService](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.weblinkedservice.aspx)
* Tietojoukon seuraavanlaisia on lisätty: 
    * [ODataResourceDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.odataresourcedataset.aspx), [WebTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.webtabledataset.aspx)
* Kopioi lähde seuraavanlaisia on lisätty:  
    * [WebSource](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.websource.aspx)

## <a name="version-440"></a>Version 4.4.0

### <a name="feature-additions"></a>Ominaisuuden lisäykset

- Linkitetyn palvelutyyppi on lisätty tietolähteet ja kopioi toimintojen täyttyvät:
    - [AzureStorageSasLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.azurestoragesaslinkedservice.aspx). Katso [Azure SAS linkitetyn Tallennuspalvelu](data-factory-azure-blob-connector.md#azure-storage-sas-linked-service) käsitteellisiä tietoja ja esimerkkejä. 

## <a name="version-430"></a>Version 4.3.0

### <a name="feature-additions"></a>Ominaisuuden lisäykset

- Linkitetyn palvelun tyypit-puuttuu on lisättyjä kopioi toiminnoista:
    - [HdfsLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.hdfslinkedservice.aspx). Katso [siirtää tiedot käyttämällä Data Factory HDFS](data-factory-hdfs-connector.md) käsitteellisiä tietoja ja esimerkkejä. 
    - [OnPremisesOdbcLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.onpremisesodbclinkedservice.aspx). Katso [siirtää ODBC-tiedot tallennetaan käyttämällä Azure Data Factory](data-factory-odbc-connector.md) käsitteellisiä tietoja ja esimerkkejä. 

## <a name="version-420"></a>Version 4.2.0

### <a name="feature-additions"></a>Ominaisuuden lisäykset

- Seuraavat uuden tehtävän tyyppi on lisätty: [AzureMLUpdateResourceActivity](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuremlupdateresourceactivity.aspx). Lisätietoja tehtävästä on artikkelissa [Azure ML päivittäminen mallien päivityksen resurssi-toiminnon avulla](data-factory-azure-ml-batch-execution-activity.md#updating-azure-ml-models-using-the-update-resource-activity).
- Uusi valinnainen ominaisuus [updateResourceEndpoint](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuremllinkedservice.updateresourceendpoint.aspx) on lisätty [AzureMLLinkedService luokka](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuremllinkedservice.aspx). 
- [LongRunningOperationInitialTimeout](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.datafactorymanagementclient.longrunningoperationinitialtimeout.aspx) ja [LongRunningOperationRetryTimeout](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.datafactorymanagementclient.longrunningoperationretrytimeout.aspx) ominaisuudet on lisätty [DataFactoryManagementClient](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.datafactorymanagementclient.aspx) -luokka. 
- Salli määrittäminen aikakatkaisu asiakkaan puheluihin Data Factory-palveluun. 


## <a name="version-410"></a>Version 4.1.0-sovelluksessa

### <a name="feature-additions"></a>Ominaisuuden lisäykset
* Linkitetyn palvelun seuraavanlaisia on lisätty: 
    * [AzureDataLakeStoreLinkedService](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestorelinkedservice.aspx)
    * [AzureDataLakeAnalyticsLinkedService](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakeanalyticslinkedservice.aspx)
* Seuraavat tehtävätyypit on lisätty: 
    * [DataLakeAnalyticsUSQLActivity](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datalakeanalyticsusqlactivity.aspx)
* Tietojoukon seuraavanlaisia on lisätty: 
    * [AzureDataLakeStoreDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestoredataset.aspx)
* Lähde- ja käsittelytoiminto seuraavanlaisia kopioi tehtävälle on lisätty:
    * [AzureDataLakeStoreSource](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestoresource.aspx)
    * [AzureDataLakeStoreSink](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestoresink.aspx)


## <a name="version-401"></a>Version 4.0.1

### <a name="breaking-changes"></a>Tärkeimmät muutokset
Seuraavat luokat on nimetty uudelleen. Uudet nimet on alkuperäinen luokkien nimet ennen 4.0.0 vapauttamista. 
 
4.0.0 nimi | 4.0.1 nimi
:------------ | :------------ 
AzureSqlDataWarehouseDataset | [AzureSqlDataWarehouseTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuresqldatawarehousetabledataset.aspx)
AzureSqlDataset | [AzureSqlTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuresqltabledataset.aspx)
AzureDataset | [AzureTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuretabledataset.aspx)
OracleDataset | [OracleTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.oracletabledataset.aspx)
RelationalDataset | [RelationalTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.relationaltabledataset.aspx)
SqlServerDataset | [SqlServerTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.sqlservertabledataset.aspx)


## <a name="version-400"></a>Version 4.0.0

### <a name="breaking-changes"></a>Tärkeimmät muutokset



- Seuraavat luokat ja liittymät on nimetty uudelleen.

| Vanha nimi | Uusi nimi |
| :-------- | :-------- |
| ITableOperations | [IDatasetOperations](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.idatasetoperations.aspx) |  
| Taulukko | [Tietojoukko](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.dataset.aspx) | 
| TableProperties | [DatasetProperties](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetproperties.aspx) | 
| TableTypeProprerties | [DatasetTypeProperties](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasettypeproperties.aspx) |
| TableCreateOrUpdateParameters | [DatasetCreateOrUpdateParameters](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetcreateorupdateparameters.aspx) | 
| TableCreateOrUpdateResponse | [DatasetCreateOrUpdateResponse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetcreateorupdateresponse.aspx) | 
| TableGetResponse | [DatasetGetResponse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetgetresponse.aspx) | 
| TableListResponse | [DatasetListResponse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetlistresponse.aspx) |
| CreateOrUpdateWithRawJsonContentParameters | [DatasetCreateOrUpdateWithRawJsonContentParameters](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetcreateorupdatewithrawjsoncontentparameters.aspx) | 
    

- **Luettelon** menetelmiä palauttaa sivutettu tulokset nyt. Jos vastaus on muu kuin tyhjä **NextLink** ominaisuus, asiakassovellus on edelleen haetaan seuraavalle sivulle, kunnes kaikki sivut palautetaan.  Tässä on esimerkki: 

        PipelineListResponse response = client.Pipelines.List("ResourceGroupName", "DataFactoryName");
        var pipelines = new List<Pipeline>(response.Pipelines);
    
        string nextLink = response.NextLink;
        while (string.IsNullOrEmpty(response.NextLink))
        {
            PipelineListResponse nextResponse = client.Pipelines.ListNext(nextLink);
            pipelines.AddRange(nextResponse.Pipelines);
    
            nextLink = nextResponse.NextLink;
        }
    
- **Luettelon** myyntijakso API palauttaa vain sen sijaan, että tarkat tiedot putkijohto yhteenveto. Esimerkiksi toimintojen myyntijakso yhteenveto sisältää vain nimi ja tyyppi.

### <a name="feature-additions"></a>Ominaisuuden lisäykset
- [SqlDWSink](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.sqldwsink.aspx) luokan tukee kahta uusia ominaisuuksia, **SliceIdentifierColumnName** ja **SqlWriterCleanupScript**tukemaan Azure SQL-tietovarasto idempotent kopioi. On artikkelissa [Azure SQL-tietovarasto](data-factory-azure-sql-data-warehouse-connector.md) , erityisesti [järjestelmä 1](data-factory-azure-sql-data-warehouse-connector.md#mechanism-1) - ja lisätietoja näiden ominaisuuksien [järjestelmä 2](data-factory-azure-sql-data-warehouse-connector.md#mechanism-2) osissa.

- Sovellus tukee nyt käynnissä tallennetun toimintosarjan vastaan Azure SQL-tietokantaan ja SQL Azure-tietovarasto kopioi tehtävän osana. [SqlSource](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.sqlsource.aspx) ja [SqlDWSource](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.sqldwsource.aspx) luokkia on seuraavat ominaisuudet: **SqlReaderStoredProcedureName** ja **StoredProcedureParameters**. Artikkeleissa [Azure SQL-tietokantaan](data-factory-azure-sql-connector.md#sqlsource) ja [SQL Azure-tietovarasto](data-factory-azure-sql-data-warehouse-connector.md#sqldwsource) -Azure.com lisätietoja nämä ominaisuudet.  