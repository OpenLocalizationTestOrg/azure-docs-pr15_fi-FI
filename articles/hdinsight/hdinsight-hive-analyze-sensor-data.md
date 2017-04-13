<properties
    pageTitle="Rakenne ja Hadoop tunnistimen tietojen analysointia | Microsoft Azure"
    description="Opettele tunnistimen tietojen analysoiminen käyttämällä kyselyn rakenne-konsolin HDInsight (Hadoop) ja valitse Visualisoi tietoja Microsoft Excelin PowerView."
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/20/2016" 
    ms.author="larryfr"/>

#<a name="analyze-sensor-data-using-the-hive-query-console-on-hadoop-in-hdinsight"></a>Kyselyn rakenne-konsolin käyttäminen Hadoop HDInsight tunnistimen tietojen analysointi

Opettele tunnistimen tietojen analysoiminen käyttämällä kyselyn rakenne-konsolin HDInsight (Hadoop) ja sitten Microsoft Excelin tietojen visualisointi Power View käyttämällä.

> [AZURE.NOTE] Ohjeita tämän asiakirjan toimivat vain Windows-pohjaisesta HDInsight klustereiden.

Tässä esimerkissä käytetään rakenteen prosessin historiatiedoissa tuottamat lämmitys-, tuuletus, ja ilmastointi (LVI) kaaviota tunnistavan järjestelmiin, jotka eivät ole voivat ylläpitää luotettavasti määrittäminen lämpötilan. Opit, miten voit:

- Luomalla rakenteen taulukoita kyselyn Luetteloerottimella erotetut tiedostot (CSV) tallennettuja tietoja.
- Voit analysoida tietoja rakenne-kyselyjen luominen.
- Microsoft Excelin avulla voit muodostaa HDInsight (joko avoinna olevan tietokannan connectivity (ODBC) analysoitujen tietojen hakemiseen.
- Power View tietojen visualisoimiseen.

![Esimerkkejä joistain muista ratkaisuarkkitehtuuri](./media/hdinsight-hive-analyze-sensor-data/hvac-architecture.png)

##<a name="prerequisites"></a>Edellytykset

* HDInsight (Hadoop)-klusterin: Lisätietoja [säännöstä Hadoop klustereiden HDInsight-](hdinsight-provision-clusters.md) klusterin luominen.

* Microsoft Excel 2013: ssa

    > [AZURE.NOTE] Tietojen visualisointi [Power](https://support.office.com/Article/Power-View-Explore-visualize-and-present-your-data-98268d31-97e2-42aa-a52b-a68cf460472e?ui=en-US&rs=en-US&ad=US)View käytetään Microsoft Excelissä.

* [Microsoft rakenteen ODBC-ohjain](http://www.microsoft.com/download/details.aspx?id=40886)

##<a name="to-run-the-sample"></a>Jos haluat suorittaa otosten

1. Siirry selaimesta, seuraava URL-osoite. Korvaa `<clustername>` HDInsight-klusterin nimi.

        https://<clustername>.azurehdinsight.net

    Kun sinulta kysytään, todennetaan käyttämällä järjestelmänvalvojan käyttäjänimi ja salasana käytit, kun tämän klusterin valmistelu.

2. Web-sivulta, joka avautuu **Käytön aloittaminen-valikoima** -välilehti ja **ratkaisujen esimerkkitiedoilla** , luokka-kohdassa **Tunnistimen tietojen analysointi** -malli.

    ![Käytön aloittaminen-valikoiman kuva](./media/hdinsight-hive-analyze-sensor-data/getting-started-gallery.png)

3. Noudata Viimeistele otosten web-sivulla annettuja ohjeita.
