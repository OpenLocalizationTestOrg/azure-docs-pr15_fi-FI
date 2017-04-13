<properties
 pageTitle="Käsittele ajoneuvon tunnistimen tietojen Apache myrsky HDInsight | Microsoft Azure"
 description="Opettele käsittelemään ajoneuvon tunnistimen tietojen tapahtuman keskittimet Apache myrsky käyttäminen Hdinsightista. Lisää mallitietojen DocumentDB ja tallentaa tallennustilan tulosteen."
 services="hdinsight,documentdb,notification-hubs"
 documentationCenter=""
 authors="Blackmist"
 manager="jhubbard"
 editor="cgronlun"/>

<tags
ms.service="hdinsight"
ms.devlang="java"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="big-data"
ms.date="08/23/2016"
ms.author="larryfr"/>

#<a name="process-vehicle-sensor-data-from-azure-event-hubs-using-apache-storm-on-hdinsight"></a>Käsittele ajoneuvon tunnistimen tietojen Azure tapahtuman keskittimet Apache myrsky käyttäminen Hdinsightiin

Opettele käsittelemään ajoneuvon tunnistimen tietojen Azure tapahtuman keskittimet Apache myrsky käyttäminen Hdinsightista. Tässä esimerkissä lukee tunnistimen tietojen Azure tapahtuman keskittimet, rikastaa tiedot viittaamalla Azure DocumentDB tallennettuja tietoja ja lopuksi tietojen tallennusta Azure varastoon käyttämällä Hadoop-tiedosto järjestelmän (HDFS).

![HDInsight ja asioita Internet (IoT) arkkitehtuuri-kaavio](./media/hdinsight-storm-iot-eventhub-documentdb/iot.png)

##<a name="overview"></a>Yleiskatsaus

Anturit lisääminen ajoneuvot avulla voit ennustaa laitteiden ongelmien historiallisten trendien perusteella sekä käyttöanalyysin kuvio perustuu tulevien versioiden parantamiseksi. Kun perinteinen MapReduce eräkäsittely voidaan käyttää tätä analyysia varten, sinun on voitava nopeasti ja tehokkaasti tiedot ladataan kaikki Hadoop huomioon ennen MapReduce käsittelyn voi ilmetä. Lisäksi haluat tehdä analyysin reaaliajassa vakavia virheitä polkujen (ohjelma lämpötilan, jarrujen jne.).

Azure tapahtuman keskittimet on suunniteltu käsittelemään anturit luomien tietojen valtaviin äänenvoimakkuuden ja Apache myrsky HDInsight-voidaan käyttää lataamisen ja käsitellä tietoja ennen tallentamista kyselyjä HDFS (palautettua Azure-tallennustilan) MapReduce lisäkäsittelyä varten.

##<a name="solution"></a>Ratkaisu

Ohjelma lämpötila, lämpötilan ja ajoneuvon nopeus telemetriatietojen tiedot on tallennettu anturit, lähetetään sekä auton ajoneuvon tunnus numero (VIN) ja aikaleiman porttiin tapahtuman mukaan. Sieltä Apache-myrsky HDInsight-klusterissa käytössä myrsky-topologian lukee tiedot, käsittelee sen ja tallentaa sen HDFS kyselyjä.

Käsittelyn aikana VIN käytetään mallitiedot noutaa Azure DocumentDB. Tämä on lisätty tietovirta, ennen kuin se on tallennettu.

Käyttää myrsky topologian osat ovat seuraavat:

* **EventHubSpout** - lukee tiedot Azure tapahtuman keskittimet

* **TypeConversionBolt** - muuntaa JSON-merkkijono-tapahtuman keskittimet ohjelma lämpötila, lämpötilan, nopeutta, VIN ja aikaleiman yksittäisten arvopisteiden arvot sisältävä monikon

* **DataReferencBolt** - hakee ajoneuvon mallin avulla VIN DocumentDB-

* **WasbStoreBolt** - tallentaa tiedot HDFS (Azure tallennus)

Seuraavassa on esimerkkejä joistain muista tämä ratkaisu:

![myrsky topologian](./media/hdinsight-storm-iot-eventhub-documentdb/iottopology.png)

> [AZURE.NOTE] Tämä on yksinkertaistettu kaavioon ja jokaisen osan ratkaisun voi olla useita kertoja. Esimerkiksi HDInsight-klusterissa myrsky solmujen jakautuu topologian jokaisen osan useita kertoja.

##<a name="implementation"></a>Käyttöönotto

Valmiina, automaattinen ratkaisu tässä skenaariossa on saatavana osana GitHub [HDInsight-myrsky-esimerkkejä](https://github.com/hdinsight/hdinsight-storm-examples) -säilöön. Jos haluat käyttää tässä esimerkissä, ohjeiden [IoTExample Lueminut-tiedosto. MD](https://github.com/hdinsight/hdinsight-storm-examples/blob/master/IotExample/README.md).

## <a name="next-steps"></a>Seuraavat vaiheet

Katso Lisää Esimerkki myrsky topologioissa [Esimerkki topologioissa myrsky HDInsight-varten](hdinsight-storm-example-topology.md).
