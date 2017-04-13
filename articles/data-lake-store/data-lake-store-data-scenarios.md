<properties 
   pageTitle="Tietoja käsittelevissä tilanteissa, joissa järvi tietovaraston | Microsoft Azure" 
   description="Ymmärtämään eri skenaarioissa ja työkaluja, avulla, mitkä tiedot voit nautittuina, käsitellä, ladata ja näyttää järvi tietosäilö" 
   services="data-lake-store" 
   documentationCenter="" 
   authors="nitinme" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="09/06/2016"
   ms.author="nitinme"/>

# <a name="using-azure-data-lake-store-for-big-data-requirements"></a>Azure järvi tietovaraston käytön big datasta vaatimukset

Suuri tietojenkäsittely on neljä tärkeimmät vaiheet:

* Ingesting suurten tietomäärien tuominen tietovaraston reaaliaikainen tai erissä
* Tietojen käsitteleminen
* Tietojen lataaminen
* Kesäolympialaisten visualisointi tiedot

Tässä artikkelissa on katsomalla nämä vaiheet, jotka koskevat Azure Lake Tietosäilölle ymmärtämään asetukset ja työkaluja, big datasta tarpeitasi.

## <a name="ingest-data-into-data-lake-store"></a>Tietojen ingest Lake säilöön

Tässä osassa esitellään eri lähteistä ja eri tavalla, jossa tiedot on nautittuina järvi tietosäilö-tilille.

![Ingest tietojen järvi säilöön] (./media/data-lake-store-data-scenarios/ingest-data.png "Ingest tietojen järvi säilöön")

### <a name="ad-hoc-data"></a>Ad hoc tiedot

Tämä on pienempi tietojoukot, joita käytetään prototyyppiä big datasta-sovellus. Ingesting luonnoslehtiössä tietojen mukaan tietolähteeksi eri tavoilla.

| Tietolähde        | Ingest avulla                                                                        |
|--------------------|----------------------------------------------------------------------------------------|
| Paikallisessa tietokoneessa     | <ul> <li>[Azure Portal](/data-lake-store-get-started-portal.md)</li> <li>[Azure PowerShell](data-lake-store-get-started-powershell.md)</li> <li>[Azure Office kaikissa ympäristöissä CLI](data-lake-store-get-started-cli.md)</li> <li>[Käyttämällä järvi Datatyökalut Visual Studio](../data-lake-analytics/data-lake-analytics-data-lake-tools-get-started.md#upload-source-data-files) </li></ul> |
| Tallennustilan Azure-Blob | <ul> <li>[Azure Data Factory](../data-factory/data-factory-azure-datalake-connector.md#sample-copy-data-from-azure-blob-to-azure-data-lake-store)</li> <li>[AdlCopy-työkalu](data-lake-store-copy-data-azure-storage-blob.md)</li><li>[DistCp HDInsight-klusterin käytössä](data-lake-store-copy-data-wasb-distcp.md)</li> </ul> |

 
### <a name="streamed-data"></a>Virtautettua tiedot

Tämä on tietoja, joita voi luoda eri lähteistä, kuten sovellukset, laitteita, anturit. Nämä tiedot voidaan nieltäväksi järvi säilöön eri työkaluja. Nämä työkalut yleensä sieppaaminen ja käsitellä tapahtuman tapahtuma-peruste tietoihin reaaliaikaisesti, ja kirjoita sitten tapahtumat erissä järvi säilöön niin, että ne voidaan käsitellä edelleen. 

Seuraavassa on Työkalut, joiden avulla voit:
 
* [Azure Stream Analytics] (.. / stream analytics-tiedot-järvi-tuloste) - tapahtumien nautittuina kyselyjä tapahtuman keskittimet voidaan kirjoittaa Azure tietojen järvi käyttämällä Azure järvi tietovaraston tulos.
* [Azure Hdinsightiin myrsky](../hdinsight/hdinsight-storm-write-data-lake-store.md) – voit kirjoittaa tiedot suoraan järvi tietovaraston myrsky klusterista.
* [EventProcessorHost](../event-hubs/event-hubs-csharp-ephcs-getstarted.md#receive-messages-with-eventprocessorhost) – voit vastaanottaa tapahtumat tapahtuman keskittimet ja sen jälkeen kirjoittamaan järvi tietovaraston käyttäminen [Tietojen järvi kaupan .NET SDK](data-lake-store-get-started-net-sdk.md).

### <a name="relational-data"></a>Relaatiotietoja

Voit myös tietolähteen tietoja relaatiotietokannasta. Ajanjakson aikana relaatiotietokannasta kerätä tietoja, jotka tarjoavat tärkeimmät tiedot, jos käsittelee big datasta putkijohto erittäin suuri määrä. Voit näiden tietojen siirtäminen järvi tietovaraston seuraavia työkaluja.

* [Apache Sqoop](data-lake-store-data-transfer-sql-sqoop.md)
* [Azure Data Factory](../data-factory/data-factory-data-movement-activities.md)

### <a name="web-server-log-data-upload-using-custom-applications"></a>Web-palvelimen lokitiedot (Lataa mukautetun sovelluksilla)

Tällaista tietojoukko kutsutaan erityisesti, koska web server log tietojen analysointi on yleisiä Käyttötapaus big datasta sovellusten ja vaatii lokitiedostojen ladattava järvi tietovaraston suurista tietomääristä. Kirjoittaa omia komentosarjojen tai sovellusten lataaminen näiden tietojen jollakin seuraavista työkaluista.

* [Azure Office kaikissa ympäristöissä CLI](data-lake-store-get-started-cli.md)
* [Azure PowerShell](data-lake-store-get-started-powershell.md)
* [Azure järvi tietovaraston .NET SDK-paketissa](data-lake-store-get-started-net-sdk.md)
* [Azure Data Factory](../data-factory/data-factory-data-movement-activities.md)

Ladataan WWW-palvelimen lokitiedot, ja myös ladataan muunlaisia tietoja (kuten sosiaalisen lauseiden oikeudet omistaa) on hyvä tapa kirjoittaa omia mukautettuja komentosarjoja ja sovelluksia, koska se lisää joustavuutta sisällytettävien tietojen lataaminen osan suurempi big datasta-sovelluksen osana. Joissakin tapauksissa koodi saattaa kestää komentosarja tai yksinkertainen komentoriviltä apuohjelma. Muussa tapauksessa koodi voidaan suuri tietojenkäsittely integroida business-sovelluksen tai ratkaisu.


### <a name="data-associated-with-azure-hdinsight-clusters"></a>Azure Hdinsightiin klustereiden liittyviä tietoja

Useimmat HDInsight-klusterin tyypit (Hadoop-HBase, myrsky) tue järvi tietovaraston tietojen tallennustilan säilö. HDInsight klustereiden käyttää tietoja from Azure tallennustilan BLOB-objektit (WASB). Parantaa suorituskykyä voit kopioida tiedot WASB liittyvän klusterin järvi tietosäilö-tilille. Voit kopioida tiedot käyttämällä seuraavia työkaluja.

* [Apache DistCp](data-lake-store-copy-data-wasb-distcp.md)
* [AdlCopy-palvelu](data-lake-store-copy-data-azure-storage-blob.md)
* [Azure Data Factory](../data-factory/data-factory-azure-datalake-connector.md#sample-copy-data-from-azure-blob-to-azure-data-lake-store)

### <a name="data-stored-in-on-premise-or-iaas-hadoop-clusters"></a>Paikallinen tai IaaS Hadoop-klustereiden tallennettuja tietoja

Suurten tietomäärien on tallennettu aiemmin Hadoop klustereissa paikallisesti käyttämällä HDFS tietokoneissa. Hadoop-klustereiden voi olla paikallinen käyttöönoton tai saattaa olla IaaS-klusterin Azure-alueella. Voi olla vaatimukset näiden tietojen kopioiminen Azure järvi tietovaraston kertakäyttöisen lähestymistavan tai toistuvien tavalla. Useilla eri tavoilla, voit käyttää tätä. Alla on luettelo vaihtoehtoja ja liittyvä valinnat.

| Lähestymistapa  | Tiedot | Hyvät puolet   | Huomioon otettavia seikkoja  |
|-----------|---------|--------------|-----------------|
| Azure tietojen Factory (SYÖTTÖ) käyttäminen tietojen kopioimiseen suoraan Hadoop klustereiden Azure järvi tietosäilö | [SYÖTTÖ tukee HDFS tietolähteenä](../data-factory/data-factory-hdfs-connector.md) | SYÖTTÖ tukee ulos-valmiin HDFS ja ensimmäisen luokan lopusta loppuun hallinta ja seuranta | Data Management Gateway käyttöön paikallinen tai IaaS klusterin edellyttää |
| Vie tietoja Hadoop-tiedostoina. Kopioi sitten Azure Lake Tietosäilölle käyttämällä tarvittavat tiedostot.                                   | Voit kopioida tiedostoja Azure järvi tietovaraston avulla: <ul><li>[Windows OS Azure PowerShell](data-lake-store-get-started-powershell.md)</li><li>[Azure Office kaikissa ympäristöissä CLI Windows OS varten](data-lake-store-get-started-cli.md)</li><li>Mukautetun sovelluksen, kaikki tiedot järvi kaupan SDK: N avulla</li></ul> | Pääset alkuun nopeasti. Voit tehdä mukautettuja lataukset                                                   | Monivaiheinen prosessi, joka liittyy useita tekniikoita. Hallinta ja seuranta kasvaa hankala voi valita mukautetun laatu Työkalut ajan mittaan |
| Tietojen kopioiminen Hadoop Azuren tallennustilaan Distcp avulla. Kopioi tiedot Azuren tallennustilaan käyttämällä tarvittavat tiedot järvi Store. | Voit kopioida tietoja Azuren tallennustilaan järvi tietovaraston avulla: <ul><li>[Azure Data Factory](../data-factory/data-factory-data-movement-activities.md)</li><li>[AdlCopy-työkalu](data-lake-store-copy-data-azure-storage-blob.md)</li><li>[Apache DistCp HDInsight klustereiden käytössä](data-lake-store-copy-data-wasb-distcp.md)</li></ul>| Voit käyttää Avaa lähde-työkalut. | Monivaiheinen prosessi, johon useita tekniikoita |

### <a name="really-large-datasets"></a>Erittäin suuri tietojoukkoja

Ladataan tietojoukkoja, useita teratavua välillä käyttämällä edellä kuvatun Joskus voi olla hidas ja kallista. Tässä tapauksessa voit käyttää seuraavia.

* **Azure ExpressRoute avulla**. Azure ExpressRoute voit luoda yksityinen infrastruktuuri ja Azure palvelinkeskusten välisten yhteyksien paikallisesti. Tämä on luotettava vaihtoehto siirretään suurista tietomääristä. Saat lisätietoja [Azure ExpressRoute](../expressroute/expressroute-introduction.md)ohjeissa.


* **Tietojen lataaminen "Offline-tilassa"**. Jos käyttämällä Azure ExpressRoute ei ole mahdollista jostakin syystä, voit toimittaa kiintolevyaseman levyasemien Azure tietokeskuksen tietoihisi [Azure Tuo/Vie-palvelun](../storage/storage-import-export-service.md) avulla. Tietoja tuodaan ensin tallennustilan Azure-BLOB. Voit käyttää tietojen kopioiminen Azure-tallennustilan BLOB järvi tietovaraston sitten [Azure Data Factory](../data-factory/data-factory-azure-datalake-connector.md#sample-copy-data-from-azure-blob-to-azure-data-lake-store) tai [AdlCopy-työkalu](data-lake-store-copy-data-azure-storage-blob.md) .

    >[AZURE.NOTE] Kun käyttämällä Tuo/Vie-palvelua, joka lähettää Azure tietokeskuksen levyille tiedostoille ei saa olla yli 200 gt.


## <a name="process-data-stored-in-data-lake-store"></a>Prosessin tiedot tallennetaan järvi tietosäilö

Kun tiedot ovat käytettävissä järvi tietovaraston tuetut big datasta sovelluksilla tiedot voit suorittaa analyysi. Tällä hetkellä voit Azure Hdinsightiin ja Azure tietojen järvi analysoinnin voidaan suorittaa tietojen analysointi työt järvi tietovaraston tallennettuja tietoja. 

![Analysoi tietoja järvi tietosäilö] (./media/data-lake-store-data-scenarios/analyze-data.png "Analysoi tietoja järvi tietosäilö")

Seuraavissa esimerkeissä voi tarkastella.

* [Luo HDInsight-klusterin tietojen järvi kaupan tallennustila](data-lake-store-hdinsight-hadoop-use-portal.md)
* [Azure tietojen järvi Analytics käyttäminen järvi tietosäilö](../data-lake-analytics/data-lake-analytics-get-started-portal.md)



## <a name="download-data-from-data-lake-store"></a>Tietojen lataaminen järvi tietosäilö

Haluat ehkä myös Lataa tietojen siirtäminen tai Azure järvi tietovaraston skenaarioissa seuraavasti:

* Siirrä tiedot muiden säilöjen tietoihin liittymään aiemmin tietojen käsittely-putkistot kanssa. Haluat esimerkiksi siirtää tietoja järvi tietovaraston Azure SQL-tietokanta tai paikallisen SQL Serverin kanssa.
* Lataa tiedot paikalliseen tietokoneeseen muodostettaessa sovelluksen prototyypit IDE ympäristöissä käsittelyä varten.

![Tietosäilö järvi juniin tiedot] (./media/data-lake-store-data-scenarios/egress-data.png "Tietosäilö järvi juniin tiedot")

Tässä tapauksessa jollakin seuraavista vaihtoehdoista:

* [Apache Sqoop](data-lake-store-data-transfer-sql-sqoop.md)
* [Azure Data Factory](../data-factory/data-factory-data-movement-activities.md)
* [Apache DistCp](data-lake-store-copy-data-wasb-distcp.md)

Voit myös kirjoittaa oman komentosarjan/ohjelmien tietojen lataaminen järvi tietovaraston seuraavista tavoista.

* [Azure Office kaikissa ympäristöissä CLI](data-lake-store-get-started-cli.md)
* [Azure PowerShell](data-lake-store-get-started-powershell.md)
* [Azure järvi tietovaraston .NET SDK-paketissa](data-lake-store-get-started-net-sdk.md)

## <a name="visualize-data-in-data-lake-store"></a>Tietosäilö järvi tietojen visualisointi

Mix palvelujen avulla voit luoda visuaalisia järvi tietovaraston tallennettuja tietoja.

![Visualisoi tietoja järvi tietosäilö] (./media/data-lake-store-data-scenarios/visualize-data.png "Visualisoi tietoja järvi tietosäilö")

* Voit aloittaa [Azure Data Factory, jos haluat siirtää tietoja tietojen järvi Storesta Azure SQL-tietovarasto,](../data-factory/data-factory-data-movement-activities.md#supported-data-stores) käyttämällä
* Tämän jälkeen voit luoda tietoihin visuaalisen esityksen [integroida Power BI Azure SQL-tietovarasto kanssa](../sql-data-warehouse/sql-data-warehouse-integrate-power-bi.md) .
