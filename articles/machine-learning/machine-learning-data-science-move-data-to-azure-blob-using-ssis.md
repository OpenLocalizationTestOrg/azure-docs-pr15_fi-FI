<properties
    pageTitle="Tietojen siirtäminen tai pois Azure-Blob-säiliö SSIS yhdistimet | Microsoft Azure"
    description="Tietojen siirtäminen tai pois Azure-Blob-säiliö SSIS yhdistimiä."
    services="machine-learning,storage"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun" />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/14/2016"
    ms.author="bradsev" />

# <a name="move-data-to-or-from-azure-blob-storage-using-ssis-connectors"></a>Tietojen siirtäminen tai pois Azure-Blob-säiliö SSIS yhdistimet

[Azure SQL Server Integration Services Feature Packin](https://msdn.microsoft.com/library/mt146770.aspx) sisältää osat muodostaa Azure-Azure ja paikallisen tietolähteet ja prosessin Azure tallennettuja tietoja tietojen siirto.

Tietojen siirtäminen tai Azure-Blob-säiliö tekniikoista ohjeita on linkitetty tähän:

[AZURE.INCLUDE [blob-storage-tool-selector](../../includes/machine-learning-blob-storage-tool-selector.md)]


Kun asiakkaat olet siirtänyt paikalliset tiedot pilvipalveluun, he voivat avata sen Azure tekniikoiden ohjelmistopaketti koko esitysmuotoa, minkä tahansa Azure-palvelusta. Se voidaan käyttää, esimerkiksi Azure koneen Learning tai HDInsight-klusterin.

Tämä on yleensä ensimmäiseksi teille [SQL](machine-learning-data-science-process-sql-walkthrough.md) - ja [HDInsight](machine-learning-data-science-process-hive-walkthrough.md) vaiheittaisissa ohjeissa.

Lisätietoja canonical skenaariot, jotka käyttävät SSIS suoritettava liiketoimintatarpeiden yleisiä hybrid tietojen integrointi skenaariot [Lisää Azure SQL Server Integration Services Feature Packin harjoittelu](http://blogs.msdn.com/b/ssis/archive/2015/06/25/doing-more-with-sql-server-integration-services-feature-pack-for-azure.aspx) blogimerkinnässä.

> [AZURE.NOTE] Johdatus Azure-blob-säiliö löydät lisätietoja [Azure](../storage/storage-dotnet-how-to-use-blobs.md) -Blob-objektien perustoimintojen ja [Azure](https://msdn.microsoft.com/library/azure/dd179376.aspx)-Blob-palveluun.

## <a name="prerequisites"></a>Edellytykset

Tässä artikkelissa kuvattujen tehtävien suorittamiseen tarvitset Azure tilauksen ja tallennustilaa Azure-tili. Sinun on tiedettävä Azure-tallennustilan tilin nimi ja tilin key-tuotetunnuksen tietojen lataaminen.

- Määrittää on **Azure tilaukseen**, on artikkelissa [yhden kuukauden maksuton](https://azure.microsoft.com/pricing/free-trial/).

- Lisätietoja **tallennustilan tilin** luominen ja tilin ja avaintiedoista on artikkelissa [tietoja Azure-tallennustilan tilit](../storage/storage-create-storage-account.md).


Käyttää **SSIS yhdistimiä**, sinun on ladattava:

- **SQL Server 2014 tai 2016 Standard (tai edellä)**: Asenna sisältää SQL Server-Integrointipalveluilla.
- **Microsoft SQL Server 2014 tai Azure 2016 Integration Services Feature Packin**: nämä voi ladata, vastaavasti [SQL Server 2014 Integration Services](http://www.microsoft.com/download/details.aspx?id=47366) ja [SQL Server 2016 Integration Services](https://www.microsoft.com/download/details.aspx?id=49492) -sivuilla.

> [AZURE.NOTE] SSIS SQL Server on asennettu, mutta ei sisälly Express-version. Saat tietoja siitä, mitkä sovellukset sisältyvät SQL Server eri versioiden, [SQL Server-versioissa](http://www.microsoft.com/en-us/server-cloud/products/sql-server-editions/)

Katso koulutusmateriaalin käyttöön SSIS- [Kädet-koulutusta SSIS](http://www.microsoft.com/download/details.aspx?id=20766)

Tietojen hankkiminen ylä-ja suorittamisen avulla voit luoda yksinkertaisen poiminnan, muuntaminen ja lataaminen (ETL)-paketit ja katso SISS [SSIS-opetusohjelma: yksinkertaisen ETL pakkauksen luominen](https://msdn.microsoft.com/library/ms169917.aspx).

## <a name="download-nyc-taxi-dataset"></a>Lataa Kokousesitelmän taksin tietojoukko  
Esimerkiksi käyttää tätä yleisesti saatavilla tietojoukko-- [Kokousesitelmän taksin Trips](http://www.andresmh.com/nyctaxitrips/) tietojoukko. Tietojoukon koostuu noin 173 miljoonaa taksin kyselyt-Kokousesitelmän vuoden 2013. Tietojen kahdenlaisia: matkan tiedot tiedot ja maksun tiedot. Jokaiselle kuukaudelle on tiedoston, on kaikki, joista jokaisella on noin 2 gt puretaan 24 tiedostot.


## <a name="upload-data-to-azure-blob-storage"></a>Tietojen lataaminen Azure-blob-säiliö
Jos haluat siirtää tietoja SSIS ominaisuus Packin paikallisen Azure-blob-säiliö, Käytämme esiintymän [**Azure Blob Lataa tehtävän**](https://msdn.microsoft.com/library/mt146776.aspx)kuvassa:

![Määritä-tiedot-tiede-AM](./media/machine-learning-data-science-move-data-to-azure-blob-using-ssis/ssis-azure-blob-upload-task.png)


Parametrien, joka käyttää tehtävä on kuvattu seuraavassa:


Kenttä|Kuvaus|
----------------------|----------------|
**AzureStorageConnection**|Määrittää olemassa olevan Azure-tallennustilan Yhteyksienhallinnan tai Luo uusi, joka viittaa Azure-tallennustilan tilin, joka osoittaa, mihin blob-tiedostot sijaitsevat.|
**BlobContainer**|Määrittää nimen, joka sisältää ladatut tiedostot muodossa BLOB blob-säilö.|
**BlobDirectory**|Määrittää blob-kansioon, johon ladatun tiedoston on tallennettu nimellä estä Blob-objektien. Blob-kansio on virtual hierarkkisessa rakenteessa. Jos blob on jo luotu, se ia korvataan.|
**LocalDirectory**|Määrittää paikallisen kansioon, jossa voi ladata tiedostoja.|
**Tiedostonimi**|Määrittää nimi-suodatin, voit valita määritetyn nimen kuvion tiedostoja. Esimerkiksi MySheet\*.xls\* sisältää tiedostot, kuten MySheet001.xls ja MySheetABC.xlsx|
**TimeRangeFrom/TimeRangeTo**|Määrittää ajan aluesuodatin. Tiedostojen muokata *TimeRangeFrom* jälkeen ja ennen *TimeRangeTo* sisältyvät.|


> [AZURE.NOTE] **AzureStorageConnection** tunnistetiedot on oltava oikein ja **BlobContainer** on oltava olemassa ennen siirtoa on liikaa.

## <a name="download-data-from-azure-blob-storage"></a>Tietojen lataaminen Azure-blob-säiliö

Tietojen lataaminen Azure Blob-objektien tallennustilaan paikalliseen SSIS ja tallennustilaa, käytä [Azure Blob Lataa tehtävän](https://msdn.microsoft.com/library/mt146779.aspx)esiintymä.

##<a name="more-advanced-ssis-azure-scenarios"></a>Monimutkaisemman SSIS-Azure-skenaariot
Olemme huomautus tähän että SSIS feature Packin sallii monimutkaisia kulkee käsitellään yhdessä pakkaus tehtävät. Blob-tiedot voi esimerkiksi syötteen suoraan HDInsight-klusterin, jonka tulos on voitu ladata takaisin blob ja paikallinen tallennustilan. SSIS suorittamisen rakenne ja Possu töiden uusia SSIS-yhdistimien käyttämisestä HDInsight-klusterin:

- Voit suorittaa rakenteen komentosarjan Azure HDInsight-klusterissa, jossa SSIS käyttämällä [Azure Hdinsightiin rakenne tehtävän](https://msdn.microsoft.com/library/mt146771.aspx).
- Voit suorittaa Possu komentosarjan Azure HDInsight-klusterissa SSIS kanssa käyttämällä [Azure Hdinsightiin Possu tehtävän](https://msdn.microsoft.com/library/mt146781.aspx).
