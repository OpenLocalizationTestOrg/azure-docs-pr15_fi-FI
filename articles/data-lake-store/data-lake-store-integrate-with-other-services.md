<properties
   pageTitle="Tietosäilö järvi integraation Azure muiden | Microsoft Azure"
   description="Tietoja siitä, miten järvi tietovaraston integroituu Azure muiden"
   documentationCenter=""
   services="data-lake-store"
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/28/2016"
   ms.author="nitinme"/>

# <a name="integrating-data-lake-store-with-other-azure-services"></a>Integrointi järvi tietovaraston Azure muiden palvelujen kanssa

Azure järvi tietovaraston voidaan ottaa käyttöön skenaarioiden monenlaisille yhdessä muiden Azure-palvelujen kanssa. Seuraavassa artikkelissa on lueteltu palvelut, jotka järvi tietovaraston integroitu.

## <a name="use-data-lake-store-with-azure-hdinsight"></a>Käytä tietojen järvi Storeen Azure Hdinsightiin

Voit valmistella [Azure HDInsight](https://azure.microsoft.com/documentation/learning-paths/hdinsight-self-guided-hadoop-training/) -klusterin, joka käyttää järvi tietovaraston ÄÄN yhteensopivaan tallennustilan. Tässä versiossa, Hadoop ja myrsky klustereiden Windows ja Linux, voit käyttää järvi tietovaraston vain Lisää tallennustilaa. Näiden klustereiden käyttää Azure-tallennustilan (WASB) edelleen oletusarvo-tallennustilan. Kuitenkin HBase klustereiden Windows ja Linux, voit järvi tietovaraston oletusarvon tallennusta, tai lisää tallennustilaa tai molempina.

Ohjeita siitä, miten voit valmistella HDInsight-klusterin järvi tietovaraston kanssa on artikkelissa:

* [Valmisteleminen HDInsight-klusterin järvi tietovaraston Azure-portaalissa](data-lake-store-hdinsight-hadoop-use-portal.md)
* [Tietosäilö järvi PowerShellin Azure HDInsight-klusterin valmisteleminen](data-lake-store-hdinsight-hadoop-use-powershell.md)

**Mieluummin videoita?** Noudata alla olevia videoita, kuinka järvi tietovaraston käytettäväksi HDInsight klustereiden linkkejä.

* [Luo HDInsight-klusterin Lake Tietosäilölle käyttöönsä](https://mix.office.com/watch/l93xri2yhtp2)
* Kun klusterin on määritetty, [Access-tietojen järvi tietovaraston rakenne ja Possu komentosarjojen avulla](https://mix.office.com/watch/1n9g5w0fiqv1q)


## <a name="use-data-lake-store-with-azure-data-lake-analytics"></a>Käytä tietojen järvi Storeen Azure tietojen järvi Analytics

[Azure tietojen järvi analyysin](../data-lake-analytics/data-lake-analytics-overview.md) avulla voit käsitellä Big datasta cloud tasolla. Se dynaamisesti valmistelee resurssit ja avulla voit tehdä analytics teratavua tai jopa exatavua tiedoista, jotka voidaan tallentaa tuettujen tietolähteiden, jonkin niistä parhaillaan järvi tietovaraston määrä. Tietoja järvi Analytics erityisesti optimoitu toimimaan Azure tietojen järvi Store - tarjoaa parhaan mahdollisen suorituskyvyn ja siirtonopeuden parallelization puolestasi big datasta toiminnoista.

Katso ohjeet tietojen järvi Analytics käyttämisestä tietojen järvi kaupan [Tietojen järvi Analytics-tietojen järvi säilöä käytön aloittaminen](../data-lake-analytics/data-lake-analytics-get-started-portal.md).

**Mieluummin videoita?** Noudata alla olevia videoita, kuinka järvi tietovaraston käytettäväksi HDInsight klustereiden linkkejä.

* [Azure tietojen järvi Analytics yhdistäminen Azure järvi tietosäilö](https://mix.office.com/watch/qwji0dc9rx9k)
* [Accessin Azure järvi tietovaraston tietojen järvi Analytics kautta](https://mix.office.com/watch/1n0s45up381a8)


## <a name="use-data-lake-store-with-azure-data-factory"></a>Käytä tietojen järvi Storeen Azure Data Factory

Voit [Azure Data Factory](https://azure.microsoft.com/services/data-factory/) ingest Azure taulukot, Azure SQL-tietokanta, Azure SQL-DataWarehouse, Azure tallennustilan BLOB-objektit ja paikallisen tietokannan tietoja. Parhaillaan ensimmäisen luokan kansalaisten Azure ekosysteemissä Azure Data Factory avulla voidaan orchestrate tietoja näiden lähteestä Azure järvi tietovaraston nieltynä.

Katso ohjeet käyttämisestä Azure Data Factory tietojen järvi kaupan [Siirrä tiedot ja tietoja järvi kaupasta käyttämällä Data Factory](../data-factory/data-factory-azure-datalake-connector.md).

**Uudelleen videot!** Katso [Tietoja tiedonsiirron Azure Data Factory Azure tietojen järvi säilön avulla](https://mix.office.com/watch/1oa7le7t2u4ka). 

## <a name="copy-data-from-azure-storage-blobs-into-data-lake-store"></a>Kopioi tiedot Azure-tallennustilan BLOB järvi tietosäilö

Azure järvi tietosäilö sisältää komentorivin-työkalun AdlCopy, joka mahdollistaa tietojen kopioiminen Azure-Blob-säiliö järvi tietosäilö-tilille. Lisätietoja on artikkelissa [Azure-tallennustilan BLOB tietojen järvi Store tietojen kopioiminen](data-lake-store-copy-data-azure-storage-blob.md).

## <a name="copy-data-between-azure-sql-database-and-data-lake-store"></a>Azure SQL-tietokanta ja järvi tietovaraston tietojen kopioiminen

Voit tuoda ja viedä Azure SQL-tietokanta ja järvi tietovaraston tietojen Apache Sqoop. Lisätietoja on artikkelissa [järvi tietovaraston ja Azure SQL-tietokantaan käyttämällä Sqoop tietojen kopioiminen](data-lake-store-data-transfer-sql-sqoop.md).

**Katsomalla tämän videon** [Avulla Apache Sqoop relaatio lähteiden ja Azure järvi tietovaraston tietojen siirtäminen](https://mix.office.com/watch/1butcdjxmu114).

## <a name="use-data-lake-store-with-stream-analytics"></a>Käytä tietojen järvi Storeen Stream Analytics

Yhtenä hyväksyttäväksi järvi tietovaraston avulla voidaan tallentaa tiedot virtauttaa Azure virta-analyysin avulla. Lisätietoja on artikkelissa [Azure-tallennustilan Blob järvi säilöön Stream tiedot käyttämällä Azure Stream Analytics](data-lake-store-stream-analytics.md).

## <a name="use-data-lake-store-with-power-bi"></a>Käytä Lake Tietosäilölle Power BI

Power BI avulla voit tuoda tietoja haluat analysoida ja Visualisoi tietoja järvi tietosäilö-tililtä. Lisätietoja on artikkelissa [järvi tietovaraston analysoi tietoja käyttämällä Power BI](data-lake-store-power-bi.md).

## <a name="use-data-lake-store-with-data-catalog"></a>Käytä tietojen järvi Storeen tietoluettelo

Voit rekisteröidä järvi tietovaraston tietojen tuominen Azure tietoluettelon kirjastonäkymiä tietoja koko organisaatiossa. Katso lisätietoja [rekisteröidä järvi tietovaraston Azure-tietoluetteloon tiedot](data-lake-store-with-data-catalog.md).


## <a name="see-also"></a>Katso myös

- [Yleistä Azure järvi tietosäilö](data-lake-store-overview.md)
- [Tietosäilö järvi portaalissa käytön aloittaminen](data-lake-store-get-started-portal.md)
- [Tietosäilö järvi PowerShellin avulla aloittaminen](data-lake-store-get-started-powershell.md)  
