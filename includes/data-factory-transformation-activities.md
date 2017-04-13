Azure Data Factory tukee seuraavia muunnos toimintoja, joita voi lisätä putkistot joko yksitellen tai ketjutettu toiseen aktiviteettiin.

Tehtävän tiedot-muunnos |  Laske ympäristössä 
:----------------------- | :--------------------
[Rakenne](../articles/data-factory/data-factory-hive-activity.md) | HDInsight [Hadoop] 
[Possu](../articles/data-factory/data-factory-pig-activity.md) | HDInsight [Hadoop]  
[MapReduce](../articles/data-factory/data-factory-map-reduce.md) | HDInsight [Hadoop]  
[Hadoop-tietovirta](../articles/data-factory/data-factory-hadoop-streaming-activity.md) | HDInsight [Hadoop]
[Kuormitusryhmälle liittyviä toimintoja: eräkäsittely ja Päivitä resurssi](../articles/data-factory/data-factory-azure-ml-batch-execution-activity.md) | Azure AM 
[Tallennettu toimintosarja](../articles/data-factory/data-factory-stored-proc-activity.md) | Azure SQL Azure SQL-tietovarasto tai SQL Server |
[Tietoja järvi Analytics U-SQL](../articles/data-factory/data-factory-usql-activity.md) | Azure tietojen järvi Analytics 
[DotNet](../articles/data-factory/data-factory-use-custom-activities.md) | HDInsight [Hadoop] tai Azure erä
   
> [AZURE.NOTE] 
> Voit suorittaa ohjattu ohjelmia HDInsight ohjattu klusteri MapReduce tehtävän. [Käynnistä ohjattu ohjelmien Azure Data Factory](../articles/data-factory/data-factory-spark.md) lisätietoja.
> Voit luoda mukautetun tehtävän suorittamaan R komentosarjoja HDInsight-klusterin r asennettuna. Katso [R-komentosarjan suorittaminen Azure Data Factory](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample).