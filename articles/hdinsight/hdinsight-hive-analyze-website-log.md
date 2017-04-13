<properties 
    pageTitle="Rakenteen käyttäminen Hadoop sivuston log analysointiin | Microsoft Azure" 
    description="Opettele rakenteen käyttäminen HDInsight sivuston lokit analysointia varten. Käyttää lokitiedostoon syötteeksi HDInsight-taulukkoon ja kyselyn tiedot HiveQL avulla." 
    services="hdinsight" 
    documentationCenter="" 
    authors="nitinme" 
    manager="jhubbard" 
    editor="cgronlun"
    tags="azure-portal"/>

<tags 
    ms.service="hdinsight" 
    ms.workload="big-data" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/17/2016" 
    ms.author="nitinme"/>

# <a name="use-hive-with-hdinsight-to-analyze-logs-from-websites"></a>Rakenteen käyttäminen HDInsight lokit verkkosivuilta analysointia varten

Opettele HiveQL käyttäminen HDInsight analysointiin lokit sivustosta. Sivuston log analyysi voidaan määritetään yleisö samalla tehtävien perusteella, sivuston käyttäjät luokittelevat demografia ja selvittää sisällön ne näkymän, ne ovat peräisin sivustot ja niin edelleen.

Tässä esimerkissä käytetään HDInsight-klusterin analysoida sivuston lokitiedostojen saat vierailut sivuston ulkoisen verkkosivuilta päivän taajuus huomioon. Myös luoda sivuston virheitä, joita käyttäjät kohdata yhteenveto. Opit, miten voit:

- Muodosta yhteys Azure-Blob-säiliö, joka sisältää sivuston lokitiedostot.
- Voit luoda rakenne taulukoita tehdä kyselyn näistä lokitiedot.
- Voit analysoida tietoja rakenne-kyselyjen luominen.
- Microsoft Excelin avulla voit muodostaa HDInsight (käyttämällä analysoitujen tietojen hakemiseen avoinna olevan tietokannan connectivity (ODBC).

![HDI. Samples.Website.Log.Analysis][img-hdi-weblogs-sample]

##<a name="prerequisites"></a>Edellytykset

- Sinun on valmisteltu Hadoop Azure HDInsight-klusterin. Katso ohjeet [Säännöstä HDInsight klustereiden][hdinsight-provision]. 
- Sinulla on Microsoft Excel 2013: ssa tai Excel 2010: tä.
- Sinulla on oltava [Microsoft rakenne ODBC-ohjain](http://www.microsoft.com/download/details.aspx?id=40886) tietojen tuominen Excel rakenne.


##<a name="to-run-the-sample"></a>Jos haluat suorittaa otosten

1. [Azure-portaalin](https://portal.azure.com/)ja Startboard (Jos klusterin on kiinnitetty)-klusterin-ruutua, jossa haluat suorittaa otosten.

2. Klusterin-sivu **Pikalinkit**-kohdassa Valitse **Klusterin koontinäyttö**, ja valitse sitten- **Klusterin Dashboard** -sivu **HDInsight klusterin Raporttinäkymät-ikkunan**. Vaihtoehtoisesti voit avata suoraan koontinäytön käyttämällä seuraavaa URL-Osoitetta:

        https://<clustername>.azurehdinsight.net
    
    Kun sinulta kysytään, todentaa käyttämällä järjestelmänvalvojan käyttäjänimi ja salasana käytit, kun valmistelu klusterin.
  
2. Web-sivulta, joka avautuu **Käytön aloittaminen-valikoima** -välilehti ja **ratkaisujen esimerkkitiedoilla** , luokka-kohdassa **Sivuston Log Analysis** -malli.

3. Noudata Viimeistele otosten web-sivulla annettuja ohjeita.

##<a name="next-steps"></a>Seuraavat vaiheet
Kokeile seuraavassa esimerkissä: [analysoiminen tunnistimen tiedot rakenteen kanssa Hdinsightista](hdinsight-hive-analyze-sensor-data.md).


[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-sensor-data-sample]: ../hdinsight-use-hive-sensor-data-analysis.md

[img-hdi-weblogs-sample]: ./media/hdinsight-hive-analyze-website-log/hdinsight-weblogs-sample.png
 
