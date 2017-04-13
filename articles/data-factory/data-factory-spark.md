<properties 
    pageTitle="Käynnistä ohjattu ohjelmien Azure Data Factory" 
    description="Opettele käynnistää ohjattu ohjelmien käyttäminen MapReduce tehtävän Azure tiedot-factory." 
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
    ms.date="08/25/2016" 
    ms.author="spelluru"/>

# <a name="invoke-spark-programs-from-data-factory"></a>Käynnistä ohjattu ohjelmien tietojen Factory
## <a name="introduction"></a>Johdanto
Voit MapReduce tehtävän Data Factory putkijohto suorittaa ohjattu ohjelmia HDInsight ohjattu klusterissa. Katso [MapReduce tehtävän](data-factory-map-reduce.md) artikkelin käyttämiseen tehtävän ennen tämän artikkelin lukeminen yksityiskohtaiset tiedot. 

## <a name="spark-sample-on-github"></a>Ohjattu GitHub-Esimerkki
[Ohjattu - tietojen Factory otoksen GitHub-](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/Spark) esitetään, kuinka voit käynnistää Ohjattu ohjelman MapReduce toimintojen avulla. Ohjattu ohjelma kopioi vain yksi Azure-Blob-säilö tiedot toiseen. 

## <a name="data-factory-entities"></a>Tietoja Factory yksiköt
**Ohjattu-SYÖTTÖ/src/ADFJsons** -kansio sisältää tiedostoja Data Factory kohteiden (linkitetyn services, tietojoukkoja myyntijakso).  

Tässä esimerkissä on kaksi **linkitetyn services** : Azuren tallennustilaan ja Azure Hdinsightista. Määritä Azure tallennustilan nimi ja avainarvot **StorageLinkedService.json** ja clusterUri, käyttäjänimi ja salasana **HDInsightLinkedService.json**.

Tässä esimerkissä on kaksi **tietojoukkoja** : **input.json** ja **output.json**. Nämä tiedostot sijaitsevat **tietojoukkoja** -kansiossa.  Nämä tiedostot edustavat syöttö- ja tietojoukkoja MapReduce tehtävälle

Voit etsiä otoksen putkistot **ADFJsons/myyntijakso** -kansiossa. Tarkista putkijohto osata Ohjattu ohjelman käynnistäminen MapReduce-toiminnon avulla. 

MapReduce-toiminto on määritetty käynnistää **com.adf.sparklauncher.jar** **adflibs** säilön Azuren tallennustilaan (määritetty StorageLinkedService.json). Tämän ohjelman lähdekoodi on ohjattu-SYÖTTÖ/src/pää/java/com/syöttö/kansio ja sen soittaa ohjattu Lähetä ja suorita ohjattu työt. 

> [AZURE.IMPORTANT] 
> Lue [Lueminut-tiedosto. TXT](https://github.com/Azure/Azure-DataFactory/blob/master/Samples/Spark/README.txt) ennen kuin käytät otosten uusimman ja muut tiedot. 
>  
> Käynnistää ohjattu ohjelmat MapReduce tehtävän tämän menetelmän oman HDInsight Ohjattu klusterin avulla. Tarvittaessa-HDInsight-klusterin ei tueta.   


## <a name="see-also"></a>Katso myös
- [Tehtävän rakenne](data-factory-hive-activity.md)
- [Possu toiminta](data-factory-pig-activity.md)
- [MapReduce toiminta](data-factory-map-reduce.md)
- [Hadoop Streaming tehtävä](data-factory-hadoop-streaming-activity.md)
- [Käynnistää R komentosarjat](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)
