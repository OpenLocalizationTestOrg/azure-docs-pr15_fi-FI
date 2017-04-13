<properties
    pageTitle="Excelin yhdistäminen Power Queryn avulla Hadoop | Microsoft Azure"
    description="Opettele ja hyödyntää business intelligence osia käyttämällä Power Query for Excelin access-tiedot tallennetaan Hadoop-Hdinsightista."
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="jgao"/>


#<a name="connect-excel-to-hadoop-by-using-power-query"></a>Excelin yhdistäminen Hadoop Power Queryn avulla

Yksi tärkeimmistä ominaisuus Microsoft big datasta ratkaisu on Microsoft business intelligence (BI) osia Hadoop varausyksiköt Azure Hdinsightiin integrointi. Ensisijainen Esimerkki integrointi on mahdollisuus yhdistää Excel Azure-tallennustilan tilin, joka sisältää Hadoop-klusterin käyttämällä Microsoft Power Query for Excel-apuohjelma liittyviä tietoja. Tässä artikkelissa käydään läpi käyttämisestä Power Queryn kyselyn Hadoop-klusterin hallitaan sen mukaan, HDInsight liittyviä tietoja ja määrittäminen.

> [AZURE.NOTE] Tässä artikkelissa kuvattuja voidaan käyttää Linux- tai Windows-pohjaisesta HDInsight-klusterin, kun Windows tarvitaan asiakastietokoneen.

### <a name="prerequisites"></a>Edellytykset

Ennen kuin aloitat tämän artikkelin, sinulla on oltava seuraavasti:

- **HDInsight-klusterin**. Määritä yksi on artikkelissa [Azure Hdinsightiin käytön aloittaminen][hdinsight-get-started].
- **A työasema** , jossa on käytössä Windows 7: ssä, Windows Server 2008 R2 tai sitä uudemman käyttöjärjestelmän.
- **Office 2013 Professional Plus, Office 365 ProPlus-Excel 2013: n erillisversio tai Office 2010 Professional Plus**.


## <a name="install-power-query"></a>Power Queryn asentaminen

Power Queryn avulla voit tuoda tietoja useista lähteistä Microsoft Exceliin, jossa se power BI-työkaluista, kuten PowerPivot- tai Power View. Erityisesti Power Query tuoda tietoja, jotka tulosteen tai, joka on luotu Hadoop työn HDInsight-klusterin käytössä.

Lataa Microsoft Power Query for Excel, [Microsoft Download Centeristä] [ powerquery-download] ja asenna se.

## <a name="import-hdinsight-data-into-excel"></a>HDInsight-tietojen tuominen Excel

Power Query-apuohjelma Exceliin helpottaa tietojen tuomisesta Hdinsightista-klusterin Exceliin, jossa Liiketoimintatieto-työkaluista kuten PowerPivot- ja Power Map voidaan on rajattu hiusristikon sisään, analysoida ja esittää tiedot.

**Tietojen tuomisesta Hdinsightista-klusterin**

1. Avaa Excel.

2. Luo uusi tyhjä työkirja.

3. **Power Query** -valikkoa ja valitse **From Azure** **Microsoft Azure HDInsight**.

    ![HDI. PowerQuery.SelectHdiSource][image-hdi-powerquery-hdi-source]

    **Huomautus:** Jos et näe **Power Query** -valikko, valitse **Tiedosto** > **asetukset** > **- Apuohjelmat**ja valitse **COM-apuohjelmat** -sivun alaosassa avattavan luettelon **hallinta** -ruudussa. Valitse **Siirry...** -painiketta ja varmista, että valittu ruutu Power Query for Excel-apuohjelma.

    **Huomautus:** Power Queryn avulla voit tuoda tietoja **Muista lähteistä**napsauttamalla HDFS.

3. **Tilin nimi**Kirjoita yhteyttä klusterin liittyvät Azure-Blob-tallennustilan tilin nimi ja valitse sitten **OK**. Tämä voi olla [tallennustilan oletustilin](hdinsight-administer-use-management-portal.md#find-the-default-storage-account) tai linkitetyn tallennustilan tiliä.  Muoto on *https://<StorageAccountName>.blob.core.windows.net/*.

4. **Tili-näppäintä**Kirjoita Blob storage tilin avain ja valitse sitten **Tallenna**. (Sinun on suoritettava tämä vain, kun käytät myymälän ensimmäisen kerran.)

5. Kaksoisnapsauta **siirtymistoiminto** -ruudussa vasemmalla Query-editorissa Blob storage säilön nimi. Oletusarvon mukaan säilön nimi on sama nimi kuin klusterinimeä.

6. Etsi **hivesampledata.txt tuomaan tietoja** **nimi** -sarakkeessa (kansiopolku on **... / rakenne- / varasto/hivesampletable/**), ja valitse sitten **binaarinen** vasemmalla puolella hivesampledata.txt tuomaan tietoja. Hivesampledata.txt tuomaan tietoja sisältyy kaikki klusterin. Vaihtoehtoisesti voit käyttää omaa tiedostoa.

    ![HDI. PowerQuery.ImportData][image-hdi-powerquery-importdata]

7. Jos haluat, voit nimetä uudelleen sarakkeiden nimet. Kun olet valmis, valitse **Sulje ja lataa**.  Tiedot on ladattu työkirjaan:

    ![HDI. PowerQuery.ImportedTable][image-hdi-powerquery-imported-table]

## <a name="next-steps"></a>Seuraavat vaiheet

Tässä artikkelissa opit, miten tietojen noutaminen HDInsight Exceliin Power Queryn avulla. Vastaavasti voit noutaa tietoja HDInsight Azure SQL-tietokantaan. On myös mahdollista ladata tietojen esittely Hdinsightista. Lisätietoja on seuraavissa artikkeleissa:

* [Excelin yhdistäminen Microsoft rakenteen ODBC-ohjaimella Hdinsightiin][hdinsight-ODBC]
* [Tietojen lataaminen Hdinsightiin][hdinsight-upload-data]

[hdinsight-ODBC]: hdinsight-connect-excel-hive-odbc-driver.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-upload-data]: hdinsight-upload-data.md

[image-hdi-powerquery-hdi-source]: ./media/hdinsight-connect-excel-power-query/HDI.PowerQuery.SelectHdiSource.png
[image-hdi-powerquery-importdata]: ./media/hdinsight-connect-excel-power-query/HDI.PowerQuery.ImportData.png
[image-hdi-powerquery-imported-table]: ./media/hdinsight-connect-excel-power-query/HDI.PowerQuery.ImportedTable.PNG

[powerquery-download]: http://go.microsoft.com/fwlink/?LinkID=286689
