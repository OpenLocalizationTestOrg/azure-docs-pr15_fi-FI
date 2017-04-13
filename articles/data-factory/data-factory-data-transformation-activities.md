<properties 
    pageTitle="Muunnetaan: Prosessin ja Muunna tiedot | Microsoft Azure" 
    description="Opettele transform Azure Data Factory Hadoop sekä tietokoneen oppiminen ja Azure tietojen järvi Analytics-tiedot tai prosessi." 
    keywords="muunnetaan, prosessin tietoja tiedot-muunnos tehtävän muuntaminen"
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
    ms.date="09/23/2016" 
    ms.author="shlo"/>

# <a name="transform-data-in-azure-data-factory"></a>Muuntaa Azure Data Factory tiedot
> [AZURE.SELECTOR]
[Rakenne](data-factory-hive-activity.md)  
[Possu](data-factory-pig-activity.md)  
[MapReduce](data-factory-map-reduce.md)  
[Hadoop Streaming](data-factory-hadoop-streaming-activity.md)
[Machine Learning](data-factory-azure-ml-batch-execution-activity.md) 
[Tallennettu toimintosarja](data-factory-stored-proc-activity.md)
[Tietojen järvi Analytics U-SQL](data-factory-usql-activity.md)
[.NET mukautettu](data-factory-use-custom-activities.md)
   

## <a name="overview"></a>Yleiskatsaus 
Tässä artikkelissa kerrotaan tietojen muunnos toimintojen Azure Data Factory, että voit muuntaa ja käsittelee raaka tietojen ennusteiden ja tiedot. Muunnoksen tehtävän suorittaa tietojenkäsittely ympäristössä, kuten Azure HDInsight-klusterin tai Azure-erä. Tarjoaa linkkejä artikkeleihin muunnos tehtävätyypin yksityiskohtaiset tiedot.
 
Tietoja Factory tukee seuraavia tietoja muunnos toimintoja, joita voi lisätä [putkistot](data-factory-create-pipelines.md) joko yksitellen tai ketjutettu toiseen aktiviteettiin.

> [AZURE.NOTE] Saat vaiheittaiset ohjeet, katso [luominen putkijohto ja rakenne-muunnos](data-factory-build-your-first-pipeline.md) -artikkelissa.  

## <a name="hdinsight-hive-activity"></a>Tehtävän HDInsight-rakenne
Data Factory putkijohto HDInsight-rakenne-tehtävän suorittaa omat kyselyt rakennetta tai tarvittaessa Windowsin tai Linux-pohjaiset HDInsight-klusterin. Katso [Rakenne tehtävän](data-factory-hive-activity.md) artikkelissa Lisätietoja tässä aktiviteetti. 

## <a name="hdinsight-pig-activity"></a>HDInsight Possu toiminta
Data Factory putkijohto HDInsight Possu aktiviteetti suorittaa omat kyselyt Possu tai tarvittaessa Windowsin tai Linux-pohjaiset HDInsight-klusterin. Katso [Possu tehtävän](data-factory-pig-activity.md) artikkelissa Lisätietoja tässä aktiviteetti. 

## <a name="hdinsight-mapreduce-activity"></a>HDInsight MapReduce toiminta
Data Factory putkijohto HDInsight MapReduce aktiviteetti suorittaa omat ohjelmat MapReduce tai tarvittaessa Windowsin tai Linux-pohjaiset HDInsight-klusterin. Katso [MapReduce tehtävän](data-factory-map-reduce.md) artikkelissa Lisätietoja tässä aktiviteetti.

Voit suorittaa ohjattu ohjelmia HDInsight Ohjattu klusterin MapReduce tehtävän. [Käynnistä ohjattu ohjelmien Azure Data Factory](data-factory-spark.md) lisätietoja.

## <a name="hdinsight-streaming-activity"></a>HDInsight Streaming tehtävä
Data Factory putkijohto HDInsight-Streaming-tehtävän suorittaa Hadoop Streaming ohjelmia Omat tai tarvittaessa Windowsin tai Linux-pohjaiset HDInsight-klusterin. Saat tietoja tässä aktiviteetti [HDInsight Streaming tehtävän](data-factory-hadoop-streaming-activity.md) .

## <a name="machine-learning-activities"></a>Koneen Learning toiminnot
Azure Data Factory avulla voit helposti luoda putkistot, jotka käyttävät julkaistun Azure koneen Learning verkkopalvelun ennakoivan analysoinnissa. [Erän suorittamisen toimintojen](data-factory-azure-ml-batch-execution-activity.md#invoking-a-web-service-using-batch-execution-activity) käyttäminen Azure Data Factory-myyntijakso, voit käynnistää koneen Learning verkkopalvelun tekemään ennusteiden tietojen osalta.

Ajan myötä näkyvissä pistemäärä kokeet koneen Learning ennakoivan malleista on retrained käyttämällä uusi syötteen tietojoukkoja. Sen jälkeen, kun olet käsitellyt uudelleenkoulutusta, haluat päivittää tulosmalli WWW-palvelun retrained Machine Learning mallia. Voit [Päivittää resurssin tehtävän](data-factory-azure-ml-batch-execution-activity.md#updating-models-using-update-resource-activity) Päivitä WWW-palvelun juuri koulutetun mallia.  

[Käytä koneen Learning](data-factory-azure-ml-batch-execution-activity.md) näkevät lisätietoja tietokoneen Learning näitä toimia. 

## <a name="stored-procedure-activity"></a>Tallennetun toimintosarjan tehtävä
Voit käyttää SQL Serverin tallennettu toimintosarja tehtävän Data Factory putkijohto käynnistää tallennetun toimintosarjan jollakin seuraavista tietoja tallennetuista tiedoista: Azure SQL-tietokanta ja Azure SQL-tietovarasto SQL Server-tietokantaan yrityksen tai Azure-AM. Katso [Tallennettu toimintosarja tehtävän](data-factory-stored-proc-activity.md) artikkelin.  

## <a name="data-lake-analytics-u-sql-activity"></a>Tietojen järvi Analytics U-SQL-tehtävä
Tietoja järvi Analytics U SQL tehtävän suoritetaan U-SQL-komentosarja Azure tietojen järvi Analytics-klusterin. Katso [Tietoja Analytics U SQL tehtävän](data-factory-usql-activity.md) artikkelin. 

## <a name="net-custom-activity"></a>.NET mukautetut aktiviteetit
Jos tarvitset niin, että ei tue tietojen Factory tietojen muuntamiseen, voit luoda omat tietojen käsittely logiikan mukautetut aktiviteetit ja toimintojen käyttäminen putkijohto. Voit määrittää mukautetut .NET aktiviteetit, suorittaa Siirtoerän Azure-palvelu tai Azure HDInsight-klusterin. Katso [Käytä mukautettuja toimintoja](data-factory-use-custom-activities.md) artikkelin. 

Voit luoda mukautetun tehtävän suorittamaan R komentosarjoja HDInsight-klusterin r asennettuna. Katso [R-komentosarjan suorittaminen Azure Data Factory](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample). 

## <a name="compute-environments"></a>Laske ympäristöissä
Luo linkitetty palvelu Laske-ympäristön ja käytä linkitetyn palvelun määritettäessä muunnos-tehtävän. On kahdentyyppisiä Data Factory tukemat Laske-ympäristössä. 

1. **Tarvittaessa**: Tässä tapauksessa tietojenkäsittely-ympäristön täysin hallitsee Data Factory. Se luodaan automaattisesti Data Factory-palvelu, ennen kuin työn prosessin tiedot lähetetään ja poistetaan, kun työ on valmis. Voit määrittää ja hallita hajautetun suorittaminen, hallinta ja automaattiseen käynnistykseen toiminnot tarvittaessa Laske-ympäristön asetuksia. 
2. **Tuo Your oman**: Tässä tapauksessa voit rekisteröityä Data Factory linkitetyn palvelu oman tietojenkäsittely ympäristössä (esimerkiksi HDInsight-klusterin). Voit hallitsee tietojenkäsittely-ympäristön ja Data Factory-palvelu käyttää toimintojen suorittamiseen. 

Katso [Laske linkitetyn Services](data-factory-compute-linked-services.md) artikkelista lisätietoa Laske Data Factory tukemia. 


## <a name="summary"></a>Yhteenveto
Azure Data Factory tukee tietojen muunnos-toimintojen ja Laske-ympäristöissä toimia. Muunnos-toimintoja voidaan lisätä putkistot joko yksitellen tai ketjutettu kanssa toiseen tehtävään.

Tehtävän tiedot-muunnos |  Laske ympäristössä 
:----------------------- | :--------------------
[Rakenne](data-factory-hive-activity.md) | HDInsight [Hadoop] 
[Possu](data-factory-pig-activity.md) | HDInsight [Hadoop]  
[MapReduce](data-factory-map-reduce.md) | HDInsight [Hadoop]  
[Hadoop-tietovirta](data-factory-hadoop-streaming-activity.md) | HDInsight [Hadoop]
[Kuormitusryhmälle liittyviä toimintoja: eräkäsittely ja Päivitä resurssi](data-factory-azure-ml-batch-execution-activity.md) | Azure AM 
[Tallennettu toimintosarja](data-factory-stored-proc-activity.md) | Azure SQL Azure SQL-tietovarasto tai SQL Server |
[Tietoja järvi Analytics U-SQL](data-factory-usql-activity.md) | Azure tietojen järvi Analytics 
[DotNet](data-factory-use-custom-activities.md) | HDInsight [Hadoop] tai Azure erä
   

