<properties     
    pageTitle="Azure tietojen Factory - objektit" 
    description="On esimerkkejä, joiden toimittaa tietoja Azure Data Factory-palvelussa." 
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
    ms.date="10/18/2016" 
    ms.author="shlo"/>

# <a name="azure-data-factory---samples"></a>Azure tietojen Factory - objektit

## <a name="samples-on-github"></a>GitHub malleja
[GitHub Azure-DataFactory säilöön](https://github.com/azure/azure-datafactory) sisältää useita esimerkkejä, joiden avulla voit nopeasti Tutustu Azure Data Factory-palvelun kanssa (tai) Muokkaa komentosarjat ja käyttää sitä omasta-sovelluksessa. Samples\JSON kansio sisältää JSON katkelmat Yleisiä tilanteita, joissa varten.

| Esimerkki | Kuvaus |
| :----- | :---------- | 
| [SYÖTTÖ vaiheittainen kuvaus](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ADFWalkthrough) | Tässä esimerkissä on lopusta loppuun-ongelmatilanteita käyttämällä Azure Data Factory ottamaan tietojen lokitiedostojen havainnollistamisen lokitiedostojen käsittelyä varten. <br/><br/>Tätä vaiheittaista Data Factory putkijohto kerää otoksen lokit, prosessit ja rikastaa lokien tietojasi viitetiedot ja muuntaa tietoja, jotka on hiljattain käynnistetty markkinointikampanjan tehokkuuden arvioinnissa. |
| [JSON-objektit](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/JSON) | Tässä esimerkissä on yleisiä tilanteita, joissa JSON esimerkkejä. | 
| [HTTP-Downloader näyte](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/HttpDataDownloaderSample) | Tämä esimerkki showcases lataaminen Azure-Blob-säiliö .NET mukautetut aktiviteetit HTTP-päätepisteen tiedot. |
| [Toimintojen välinen AppDomain piste nettonykyarvon tehtävän malli](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/CrossAppDomainDotNetActivitySample) | Tämän mallin avulla voit tuottaa mukautetun .NET-toiminto, joka on rajoitettu ei kokoonpano-versiot käyttää SYÖTTÖ-avain (esimerkiksi WindowsAzure.Storage v4.3.0, Newtonsoft.Json v6.0.x jne.). |
| [Suorittamalla R](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample) |  Tässä esimerkissä on Data Factory mukautetut aktiviteetit, jonka avulla voidaan käynnistää RScript.exe. Tässä esimerkissä toimii vain oman (ei tarvittaessa) HDInsight-klusterin, jossa on jo sitä R asennettuna. |
| [Käynnistä ohjattu työt HDInsight Hadoop-klusterissa](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/Spark) | Tässä esimerkissä esitetään, kuinka voit käynnistää Ohjattu ohjelman MapReduce toimintojen avulla. Ohjattu ohjelma kopioi vain yksi Azure-Blob-säilö tiedot toiseen. |
| [Twitter-Azure koneen Learning erä näkyvissä pistemäärä tehtävän tietojen analysointi](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/TwitterAnalysisSample-AzureMLBatchScoringActivity) | Tässä esimerkissä esitetään, kuinka voit käyttää AzureMLBatchScoringActivity käynnistää Azure koneen Learning-malli, joka suorittaa twitter markkinatunnelma analyysi-näkyvissä pistemäärä tekstinsyöttö jne. |
| [Twitter-analyysin avulla mukautetut aktiviteetit](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/TwitterAnalysisSample-CustomC%23Activity) |  Tässä esimerkissä näytetään, miten voit käyttää .NET mukautetun toiminnon käynnistäminen Azure koneen Learning-malli, joka suorittaa twitter markkinatunnelma analyysi-näkyvissä pistemäärä tekstinsyöttö jne. |
| [Parametroitu putkistot Azure koneen oppiminen](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ParameterizedPipelinesForAzureML/) | Malli sisältää ottamaan N putkistot näkyvissä pistemäärä ja uudelleenkoulutusta molemmissa mistä luettelon alueista on peräisin parameters.txt-tiedosto, joka sisältyy tässä esimerkissä toisen alueen parametri on lopusta loppuun C# koodi. | 
| [Viittaus Azure Stream Analytics töiden tietojen päivittäminen](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ReferenceDataRefreshForASAJobs) |  Tässä esimerkissä esitetään, kuinka voit käyttää Azure Data Factory ja Azure Stream Analytics yhdessä viitetiedot aikataulun päivityksen asennuksen ja suorittaa muutoskyselyn viittaus tiedoilla. |
| [Hybrid putkijohto ja paikallisen Hortonworks Hadoop](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/HybridPipelineWithOnPremisesHortonworksHadoop) | Otosten käyttää paikalliseen Hadoop-klusterin Laske kohteeksi töitä Data Factory-samalla tavalla kuin Lisää Laske muut kohteet kuin HDInsight pohjainen Hadoop-klusterin pilvipohjaisia. |
| [JSON muuntotyökalu](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/JSONConversionTool) | Tämän työkalun avulla voit muuntaa JSONs 2015-07-01 – esikatselu uusimmat tai 2015-07-01 – esikatselu (oletus): a vanhemmalla versiolla. |  
| [U-SQL-syötteen mallitiedosto](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/U-SQL%20Sample%20Input%20File) |  Tiedosto on U-SQL-aktiviteetin käyttämän mallitiedosto. | 

## <a name="azure-resource-manager-templates"></a>Azure Resurssienhallinta-mallit
Voit etsiä Data Factory-Github seuraavat Azure Resurssienhallinta-mallit. 

| Malli | Kuvaus |
| -------- | ----------- | 
| [Kopioi Azure-Blob-säiliö Azure SQL-tietokanta](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-blob-to-sql-copy) | Tämän mallin käyttöönotto Luo Azure tietojen factory myyntijakso, kopioi tiedot Azure SQL-tietokantaan määritettyä Azure-blob-säiliö |    
| [Kopioi Salesforce Azure-Blob-säiliö](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-salesforce-to-blob-copy) | Azure tietojen factory käyttöönoton tukimalli Luo myyntijakso, kopioi tiedot on määritetty Salesforce-tili Azure-blob-säiliö. |    
| [Muuntaa tiedot suorittamalla rakenteen komentosarja Azure HDInsight-klusterissa](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-hive-transformation) | Azure tietojen factory käyttöönoton tukimalli Luo myyntijakso, joka muuntaa tietoja suorittamalla rakenteen komentosarjan Azure Hdinsightiin Hadoop-klusterin. | 


## <a name="samples-in-azure-portal"></a>Esimerkkejä, joiden Azure-portaalissa
Voit käyttää **otoksen putkistot** -ruutu tietojen factory aloitussivulla otoksen putkistot ja niiden liittyvät kohteet (tietojoukkoja ja linkitetyt palvelut)-käyttöön tietojen factory. 

1. Tietoja factory Luo tai avaa aiemmin luotu tietojen-factory. Katso [Azure Data Factory käytön aloittaminen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md#CreateDataFactory) ohjeet tietojen factory luomiseen.
2. Saat tietoja factory **DATA FACTORY** -sivu Valitse **otoksen putkistot** -ruutu.

    ![Esimerkki putkistot-ruutu](./media/data-factory-samples/SamplePipelinesTile.png)

2. Valitse **malli** , jonka haluat ottaa käyttöön **otoksen putkistot** -sivu. 
    
    ![Esimerkki putkistot sivu](./media/data-factory-samples/SampleTile.png)

3. Määritä otosten asetukset. Esimerkiksi oman Azure-tallennustilan tilin nimi ja tilin avaimen, Azure SQL-palvelimen nimi, tietokannan, Käyttäjätunnus- ja salasana jne. 

    ![Esimerkki sivu](./media/data-factory-samples/SampleBlade.png)

4. Kun olet käsitellyt kokoonpanon määrittäminen, valitsemalla **Luo** luominen ja käyttöönotto otoksen putkistot ja linkitetyt services ja taulukot putkistot käyttämä.
5. Näet tilan käyttöönoton valitsit aiemmin **otoksen putkistot** -sivu-malli-ruutu.

    ![Käyttöönottotila](./media/data-factory-samples/DeploymentStatus.png)

6. Kun näet **käyttöönoton onnistui** -viestin malli-ruutu, Sulje **otoksen putkistot** -sivu.  
5. **DATA FACTORY** -sivu näet, että linkitetyn services, tietojoukot ja putkistot on lisätty tietoja factory.  

    ![Tietoja Factory-sivu](./media/data-factory-samples/DataFactoryBladeAfter.png)
   
## <a name="samples-in-visual-studio"></a>Esimerkkejä, joiden Visual Studiossa

### <a name="prerequisites"></a>Edellytykset

Sinulla on oltava asennettuna seuraavat: 

- Visual Studio 2013: n tai Visual Studio 2015
- Lataa Azure SDK for Visual Studio 2013: n tai Visual Studio 2015. Siirry [Azure-lataussivulta](https://azure.microsoft.com/downloads/) ja sitten **ja 2013** : n tai **ja 2015** **.NET** -osassa.
- Lataa uusimmat Azure Data Factory-laajennus Visual Studio: [ja 2013](https://visualstudiogallery.msdn.microsoft.com/754d998c-8f92-4aa7-835b-e89c8c954aa5) : n tai [ja 2015](https://visualstudiogallery.msdn.microsoft.com/371a4cf9-0093-40fa-b7dd-be3c74f49005). Jos käytössäsi on Visual Studio 2013, voit myös päivittää laajennus toimimalla seuraavasti: Valitse **Työkalut**-valikosta -> **tunnisteet ja päivitykset** -> **Online** -> **Visual Studio-Galleria** -> **Microsoft Azure tietojen Factory Tools for Visual Studio** -> **Päivitä**.

### <a name="use-data-factory-templates"></a>Käytä Factory Tietomallit

1. Valitse **Tiedosto** -valikosta, valitse **Uusi**ja sitten **Projekti**. 
2. **Uusi projekti** -valintaikkunassa seuraavat toimet: 
    1. Valitse **Mallit**-kohdassa **DataFactory** . 
    2. Valitse oikeanpuoleisen ruudun **Factory tietomalleja** . 
    3. Kirjoita projektin **nimi** . 
    4. Valitse projektin **sijainti** . 
    5. Valitse **OK**. 

    ![Uusi projekti-valintaikkuna](./media/data-factory-samples/vs-new-project-adf-templates.png)
6. **Tietomallit Factory** -valintaikkunassa Valitse seuraavassa **Käyttötapaus mallit** -osassa ja valitse **Seuraava**. Seuraavat vaiheet opastusta **Asiakkaan profilointi** -mallin avulla. Vaiheet ovat samanlaiset muut mallit. 

    ![Tietojen Factory mallit-valintaikkuna](./media/data-factory-samples/vs-data-factory-templates-dialog.png) 
7. **Tietojen Factory määritys** -valintaikkunassa valitse **Tietojen Factory perustiedot** -sivulla **Seuraava** .
8. **Määritä tiedot factory** -sivulla seuraavat toimet: 
    1. Valitse **Luo uusi Data Factory**. Voit myös valita **Käytä aiemmin luotuja tietoja factory**.
    2. Kirjoita **nimi** tietojen factory.
    3. Valitse **Azure tilaus** , johon haluat luoda tietojen-factory. 
    4. Valitse tiedot-factory **resurssiryhmä** .
    5. Valitse **Länsi US**, **Yhdysvaltojen Itä**tai **Pohjois Europe** **alue**.
    6. Valitse **Seuraava**. 
9. **Määritä tiedot tallennetaan** -sivulla Määritä aiemmin **Azure SQL-tietokantaan** ja **Azure-tallennustilan tilin** (tai) luominen ja tallentaminen ja sitten Seuraava. 
10. **Määritä Laske** -sivulla Valitse oletusarvot ja valitse **Seuraava**. 
11. **Yhteenveto** -sivulla kaikki asetusten tarkistaminen ja valitse **Seuraava**. 
12. **Käyttöönottotila** -sivulla Odota, kunnes asennus on valmis, ja valitse **Valmis**.
13. Projektin Solution Explorerissa hiiren kakkospainikkeella ja valitse sitten **Julkaise**. 
19. Jos näet **Microsoft-tilille kirjautuminen** -valintaikkuna, anna tunnistetiedot tilille, joka on Azure-tilaus ja valitse **Kirjaudu sisään**.
20. Näkyviin tulee seuraava valintaikkuna:

    ![Julkaise-valintaikkuna](./media/data-factory-build-your-first-pipeline-using-vs/publish.png)

21. **Määritä tiedot factory** -sivulla seuraavat toimet: 
    1. Varmista, että **Käytä aiemmin luotuja tietoja factory** -vaihtoehto.
    2. Valitse **tietojen factory** , valitse oli mallia käytettäessä. 
    6. Valitse **Seuraava** siirtyäksesi **Julkaista kohteita** -sivulle. (Painamalla **VÄLILEHTI** siirtää pois nimi-kenttä, jos **Seuraava** -painike ei ole käytettävissä.) 
23. **Julkaista kohteita** sivulla että kaikki tiedot-tehtaan kohteiden valitaan ja valitse **Seuraava** siirtyäksesi **Yhteenveto** -sivulle.     
24. Tarkista yhteenveto ja valitse **Seuraava** **Käyttöönoton tilan**ja käynnistää käyttöönottoprosessin.
25. **Käyttöönottotila** -sivulla näkyy käyttöönottoprosessin tila. Kun asennus on valmis, valitse Valmis. 

Katso lisätietoja tekijän tiedot Factory kohteiden Visual Studion avulla ja julkaista ne Azure [luominen ensimmäisen tietojen factory (Visual Studio)](data-factory-build-your-first-pipeline-using-vs.md) .          