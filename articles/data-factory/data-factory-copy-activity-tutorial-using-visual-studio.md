<properties 
    pageTitle="Opetusohjelma: Luo putkijohto kopioi aktiviteettiin Visual Studiossa | Microsoft Azure" 
    description="Tässä opetusohjelmassa luot Azure Data Factory-myyntijakso kopioi aktiviteettiin Visual Studion avulla." 
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
    ms.topic="get-started-article" 
    ms.date="10/17/2016" 
    ms.author="spelluru"/>

# <a name="tutorial-create-a-pipeline-with-copy-activity-using-visual-studio"></a>Opetusohjelma: Luo putkijohto kopioi aktiviteettiin Visual Studiossa
> [AZURE.SELECTOR]
- [Yleiskatsaus ja edellytykset](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
- [Ohjattu kopiointi](data-factory-copy-data-wizard-tutorial.md)
- [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md)
- [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
- [PowerShellin](data-factory-copy-activity-tutorial-using-powershell.md)
- [Azure Resurssienhallinta-malli](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
- [REST-OHJELMOINTIRAJAPINNALLA](data-factory-copy-activity-tutorial-using-rest-api.md)
- [.NET-OHJELMOINTIRAJAPINTA](data-factory-copy-activity-tutorial-using-dotnet-api.md)


Tässä opetusohjelmassa näytetään, miten voit luoda ja seurata Visual Studiossa Azure tiedot-factory. Tietoja factory putkijohto käyttää kopioi tehtävän tietojen kopioiminen Azure-Blob-säiliö Azure SQL-tietokantaan.

Näin voit suorittaa tässä opetusohjelmassa osana:

1. Luo kaksi linkitetyn services: **AzureStorageLinkedService1** ja **AzureSqlinkedService1**. 

    AzureSqlLinkedService1 linkkien tietojen factory Azure SQL-tietokantaan ja AzureStorageLinkedService1 linkit Azure tallennustilan: **ADFTutorialDataFactoryVS**. Putkijohto syöttötiedot sijaitsee blob-säilö Azure-blob-säiliö ja tulostus-tiedot tallennetaan Azure SQL-tietokannan taulukkoon. Tämän vuoksi lisäät nämä kaksi stores linkitetyn services tietojen factory.
2. Luo kaksi tietojoukkoja: **InputDataset** ja **OutputDataset**, jotka edustavat i/tietoja, jotka on tallennettu tietoja tallennetuista tiedoista. 

    Määritä InputDataset-blob-säilö, joka sisältää Blob-objektien lähdetiedot. Määritä OutputDataset, joka tallentaa tiedot SQL-taulukkoon. Voit myös määrittää muita ominaisuuksia, kuten rakenne, käytettävyys ja käytännön.
3. Luo **ADFTutorialPipeline** ADFTutorialDataFactoryVS-niminen putkijohto. 

    Putkijohto on **Kopio tehtävän** , joka kopioi syötteen tietoja Azure blob-tulostus Azure SQL-taulukkoon. Kopioi tehtävän suorittaa tietojen siirto Azure Data Factory. Tehtävä on tarjoaa yleisesti saatavilla palvelu, jota voit kopioida tiedot eri tavalla turvallinen, luotettava ja skaalattava tietojen stores välillä. Katso [Tietojen siirtämistä toimintoja](data-factory-data-movement-activities.md) artikkelissa lisätietoja kopio-tehtävän. 
4. Luo nimetty **VSTutorialFactory**tiedot-factory. Ottaa käyttöön tietojen factory ja kaikki tiedot Factory-kohteet (linkitetyn services, taulukoita ja putkijohto).    

## <a name="prerequisites"></a>Edellytykset

1. Lue artikkeli [Opetusohjelma yleiskatsaus](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) ja **edellytyksenä** vaiheiden avulla. 
2. Sinun on oltava **Azure-tilauksen järjestelmänvalvoja** voivat julkaista Data Factory kohteiden Azure Data Factory.  
3. Sinulla on oltava asennettuna seuraavat: 
    - Visual Studio 2013: n tai Visual Studio 2015
    - Lataa Azure SDK for Visual Studio 2013: n tai Visual Studio 2015. Siirry [Azure-lataussivulta](https://azure.microsoft.com/downloads/) ja sitten **ja 2013** : n tai **ja 2015** **.NET** -osassa.
    - Lataa uusimmat Azure Data Factory-laajennus Visual Studio: [ja 2013](https://visualstudiogallery.msdn.microsoft.com/754d998c-8f92-4aa7-835b-e89c8c954aa5) : n tai [ja 2015](https://visualstudiogallery.msdn.microsoft.com/371a4cf9-0093-40fa-b7dd-be3c74f49005). Voit myös päivittää laajennus tekemällä seuraavat toimet: Valitse **Työkalut**-valikosta -> **tunnisteet ja päivitykset** -> **Online** -> **Visual Studio-Galleria** -> **Microsoft Azure tietojen Factory Tools for Visual Studio** -> **Päivitä**.

## <a name="create-visual-studio-project"></a>Visual Studio projektin luominen 
1. Käynnistä **Visual Studio 2013**. Valitse **Tiedosto**, valitse **Uusi**ja sitten **Projekti**. Näkyviin tulee **Uusi projekti** -valintaikkuna.  
2. **Uusi projekti** -valintaikkunassa valitse **DataFactory** -malli ja valitse **Tyhjä Factory projektitiedosto**. Jos et näe DataFactory-mallin, sulkemalla Visual Studiossa, asenna Azure SDK Visual Studio 2013: n ja avaamalla Visual Studio.  

    ![Uusi projekti-valintaikkuna](./media/data-factory-copy-activity-tutorial-using-visual-studio/new-project-dialog.png)

3. Kirjoita **nimi** projektin, **sijainti**ja **ratkaisu**nimi ja valitse **OK**.

    ![Ratkaisunhallinnassa](./media/data-factory-copy-activity-tutorial-using-visual-studio/solution-explorer.png) 

## <a name="create-linked-services"></a>Luo linkitetty palvelut
Linkitetyn services linkittäminen Microsoft Data tai laskea Azure tietojen factory-palvelut. Katso [Tuetut tiedot tallennetaan](data-factory-data-movement-activities.md##supported-data-stores-and-formats) lähteiden ja kopioi tehtävän tukemat poistumia. Katso, Laske palveluluetteloon Data Factory tukemat [Laske linkitetyn palvelut](data-factory-compute-linked-services.md) . Tässä opetusohjelmassa et käytä mihinkään Laske-palveluun. 

Tässä vaiheessa voit luoda kaksi linkitetyn services: **AzureStorageLinkedService1** ja **AzureSqlLinkedService1**. AzureSqlLinkedService linkkien Azure SQL-tietokannan tietojen factory ja AzureStorageLinkedService1 linkitetty palvelun linkit Azure-tallennustilan tilin: **ADFTutorialDataFactory**. 

### <a name="create-the-azure-storage-linked-service"></a>Luo linkitetty Azure-tallennustilan-palvelu

4. **Linkitetyn Services** solution Explorerissa napsauttamalla hiiren kakkospainikkeella, valitse **Lisää**ja valitse **Uusi kohde**.      
5. **Lisää uusi kohde** -valintaikkunassa **Azure Tallennuspalvelu linkitetyn** luettelosta ja valitse **Lisää**. 

    ![Uusi linkitetty palvelu](./media/data-factory-copy-activity-tutorial-using-visual-studio/new-linked-service-dialog.png)
 
3. Korvaa `<accountname>` ja `<accountkey>`* Azure-tallennustilan tilin ja sen avaimen nimi. 

    ![Azure-tallennustilan linkitetty palvelu](./media/data-factory-copy-activity-tutorial-using-visual-studio/azure-storage-linked-service.png)

4. Tallenna tiedosto **AzureStorageLinkedService1.json** .

> Katso lisätietoja JSON ominaisuudet [tiedot-/ Azure-Blob-objektien siirtäminen](data-factory-azure-blob-connector.md#azure-storage-linked-service) .

### <a name="create-the-azure-sql-linked-service"></a>Luo linkitetty Azure SQL-palvelu

5. Napsauta **Ratkaisunhallinnassa** **Linkitetyn palvelut** -solmu uudelleen, valitse **Lisää**ja valitse **Uusi kohde**. 
6. Tällä hetkellä, valitse **Linkitetty Azure SQL-palvelu**ja sitten **Lisää**. 
7. Korvaa **AzureSqlLinkedService1.json tiedoston** `<servername>`, `<databasename>`, `<username@servername>`, ja `<password>` nimillä Azure SQL-palvelimeen, tietokannan, tili ja salasana.    
8.  Tallenna tiedosto **AzureSqlLinkedService1.json** . 

> [AZURE.NOTE]
> Katso lisätietoja JSON ominaisuudet [Siirrä tiedot-/ Azure SQL-tietokantaan](data-factory-azure-sql-connector.md#azure-sql-linked-service-properties) .

## <a name="create-datasets"></a>Luo tietojoukkoja
Edellisessä vaiheessa luomasi linkitetyn services **AzureStorageLinkedService1** ja **AzureSqlLinkedService1** voit linkittää tiedot factory Azure-tallennustilan tilin ja Azure SQL-tietokantaan: **ADFTutorialDataFactory**. Tässä vaiheessa voit määrittää kahden tietojoukkoja-- **InputDataset** ja **OutputDataset** --, jotka edustavat i/tiedot, jotka on tallennettu tarkoitettu AzureStorageLinkedService1 ja AzureSqlLinkedService1 tietojen stores. Määritä InputDataset-blob-säilö, joka sisältää Blob-objektien lähdetiedot. Määritä OutputDataset, joka tallentaa tiedot SQL-taulukkoon.

### <a name="create-input-dataset"></a>Kirjoita tietojoukon luominen
Tässä vaiheessa voit luoda tietojoukko nimeltä **InputDataset** , joka osoittaa blob-säilö vastaavan **AzureStorageLinkedService1** linkitetty palvelun Azure-tallennustilan. Taulukon on suorakulmainen tietojoukko ja tietojoukko tukee tällä hetkellä vain tyyppi. 

9. Napsauta **Ratkaisunhallinnassa** **taulukoita** , valitse **Lisää**ja valitse **Uusi kohde**.
10. **Lisää uusi kohde** -valintaikkunassa valitse **Azure-Blob-objektien**ja sitten **Lisää**.   
10. JSON-tekstin korvaaminen seuraava teksti ja tallenna se **AzureBlobLocation1.json** . 

        {
          "name": "InputDataset",
          "properties": {
            "structure": [
              {
                "name": "FirstName",
                "type": "String"
              },
              {
                "name": "LastName",
                "type": "String"
              }
            ],
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService1",
            "typeProperties": {
              "folderPath": "adftutorial/",
              "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
              }
            },
            "external": true,
            "availability": {
              "frequency": "Hour",
              "interval": 1
            }
          }
        }

     Huomaa seuraavat seikat: 
    
    - tietojoukon **tyyppi** on määritetty **AzureBlob**.
    - **linkedServiceName** on määritetty **AzureStorageLinkedService**. Olet luonut linkitetyt palvelua vaiheessa 2.
    - **kansiopolku** on määritetty **adftutorial** säilö. Voit myös määrittää blob-kansioon käyttämällä **fileName** -ominaisuuden nimi. Koska määrittämättä blob nimi, kaikki Blob-objektien säilössä tiedot pidetään kenttään annettavat tiedot.  
    - muodon **tyyppi** on määritetty **TekstinMuoto**
    - Kaksi kenttää on erotettu toisistaan pilkulla-merkki (**columnDelimiter**) tekstitiedostoon – **Etunimi** ja **Sukunimi** – 
    - **Käytettävyys** on määritetty **kerran tunnissa** (**korkojakso** on määritetty **Tunti** ja **väli** on määritetty **1**). Vuoksi Data Factory etsitään syöttötiedot tunnissa blob-säilö (**adftutorial**) määrittämääsi pääkansioon. 
    
    Jos et määritä **Tiedostonimi** **syötteen** tietojoukko, kaikki tiedostot ja BLOB syötteen kansiosta (**kansiopolku**) pidetään syötteiden. Jos määrität tiedostonimi JSON, vain määritetyn tiedoston/blob pidetään asn syöte.
 
    Jos et määritä **Tulostusalue** **Tiedostonimi** , **kansiopolku** luodut tiedostot nimetään muodossa: tiedot. &lt;GUID-tunnus\&gt;. txt (Esimerkki: Data.0a405f8a 93ff-4c6f-b3be-f69616f1df7a.txt.).

    Voit määrittää **kansiopolku** ja **Tiedostonimi** dynaamisesti **SliceStart** ajan perusteella, **partitionedBy** -ominaisuuden avulla. Seuraavassa esimerkissä kansiopolku käyttää vuoden, kuukauden ja päivän SliceStart (käsitellään sektoria alkamispäivää)- ja tiedostonimi käyttää tunti-SliceStart. Jos sektoria on yhdistämällä 2016-09-20T08:00:00, kansionimi on määritetty wikidatagateway/wikisampledataout/2016/09/20 ja tiedostonimi on määritetty 08.csv. 

            "folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
            "fileName": "{Hour}.csv",
            "partitionedBy": 
            [
                { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
                { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } }, 
                { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } }, 
                { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } } 

> [AZURE.NOTE]
> Katso lisätietoja JSON ominaisuudet [tiedot-/ Azure-Blob-objektien siirtäminen](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties) .

### <a name="create-output-dataset"></a>Tulosteen tietojoukon luominen
Tässä vaiheessa voit luoda nimeltä **OutputDataset**tulostus-tietojoukko. Tämä tietojoukko asioista SQL Azure SQL-tietokantaan, joita edustaa **AzureSqlLinkedService1**taulukkoon. 

11. Napsauta **Ratkaisunhallinnassa** **taulukot** uudelleen, valitse **Lisää**ja valitse **Uusi kohde**.
12. Valitse **Lisää uusi kohde** -valintaikkunassa **Azure SQL**ja valitse **Lisää**. 
13. Korvaa JSON-teksti seuraavista JSON ja tallenna se **AzureSqlTableLocation1.json** .

        {
          "name": "OutputDataset",
          "properties": {
            "structure": [
              {
                "name": "FirstName",
                "type": "String"
              },
              {
                "name": "LastName",
                "type": "String"
              }
            ],
            "type": "AzureSqlTable",
            "linkedServiceName": "AzureSqlLinkedService1",
            "typeProperties": {
              "tableName": "emp"
            },
            "availability": {
              "frequency": "Hour",
              "interval": 1
            }
          }
        }

     Huomaa seuraavat seikat: 
    
    - tietojoukon **tyyppi** on määritetty **AzureSQLTable**.
    - **linkedServiceName** on määritetty **AzureSqlLinkedService** (luomastasi linkitetyn palvelua vaiheessa 2).
    - **taulukon nimi** on määritetty **emp**.
    - Tietokannan emp-taulukossa on kolme saraketta – **tunnus**, **Etunimi**ja **Sukunimi** –. Tunnus on tunnussarake, joten sinun on määritettävä **Etunimi** ja **Sukunimi** tähän.
    - **Käytettävyys** on määritetty **kerran tunnissa** (**taajuus** arvoksi **Tunti** ja **aikavälin** arvoksi **1**).  Data Factory-palvelun Luo tulostus-tietojen sektoria tunnissa **emp** taulukon Azure SQL-tietokantaan.

> [AZURE.NOTE]
> Katso lisätietoja JSON ominaisuudet [Siirrä tiedot-/ Azure SQL-tietokantaan](data-factory-azure-sql-connector.md#azure-sql-linked-service-properties) .

## <a name="create-pipeline"></a>Myyntijakso luominen 
Olet luonut i/linkitetyn palvelut ja taulukoiden tähän mennessä. Nyt voit luoda putkijohto kopioi **Kopioi tehtävän** tiedot Azure blob Azure SQL-tietokantaan. 


1. Napsauta **Ratkaisunhallinnassa** **putkistot** hiiren kakkospainikkeella, valitse **Lisää**ja valitse **Uusi kohde**.  
15. **Kopioi tiedot myyntijakso** **Lisää uusi kohde** -valintaikkunassa ja valitse **Lisää**. 
16. Korvaa JSON seuraavat JSON ja tallenna se **CopyActivity1.json** .
            
        {
          "name": "ADFTutorialPipeline",
          "properties": {
            "description": "Copy data from a blob to Azure SQL table",
            "activities": [
              {
                "name": "CopyFromBlobToSQL",
                "type": "Copy",
                "inputs": [
                  {
                    "name": "InputDataset"
                  }
                ],
                "outputs": [
                  {
                    "name": "OutputDataset"
                  }
                ],
                "typeProperties": {
                  "source": {
                    "type": "BlobSource"
                  },
                  "sink": {
                    "type": "SqlSink",
                    "writeBatchSize": 10000,
                    "writeBatchTimeout": "60:00:00"
                  }
                },
                "Policy": {
                  "concurrency": 1,
                  "executionPriorityOrder": "NewestFirst",
                  "style": "StartOfInterval",
                  "retry": 0,
                  "timeout": "01:00:00"
                }
              }
            ],
            "start": "2015-07-12T00:00:00Z",
            "end": "2015-07-13T00:00:00Z",
            "isPaused": false
          }
        }

    Huomaa seuraavat seikat:

    - Valitse Toiminnot-osassa on vain yksi tehtävä, jonka **tyyppi** on määritetty **kopio**.
    - Tehtävän syöte on määritetty **InputDataset** ja tehtävän tulos on määritetty **OutputDataset**.
    - **TypeProperties** -osassa **BlobSource** on määritetty lähteen tyyppi ja **SqlSink** on määritetty käsittelytoiminto tyyppi.

    Korvaa nykyinen **päivä- ja** arvon seuraavana **Käynnistä** -ominaisuuden arvo. Voit määrittää päivämääräosa ja ohittaa päivämäärä kellonaika kellonaika-osa. Esimerkiksi "2016-02-03", joka vastaa "2016-02-03T00:00:00Z"
    
    Molemmat Käynnistä ja Lopeta päivämääriä ja aikoja on oltava [ISO](http://en.wikipedia.org/wiki/ISO_8601)-muodossa. Esimerkki: 2016-10-14T16:32:41Z. **Päättymisaika** on valinnainen, mutta Käytämme Tässä opetusohjelmassa. 
    
    Jos **end** -ominaisuuden arvoa ei määritetä, sen lasketaan seuraavasti: "**Käynnistä + 48 tuntia**". Suorita putkijohto jatkuvasti, Määritä **9999-09-09** **end** -ominaisuuden arvon.
    
    Edellisessä esimerkissä on 24 tietojen sektoreiden, kun kukin tietojen sektori on valmistettu kerran tunnissa.

## <a name="publishdeploy-data-factory-entities"></a>Julkaise/käyttöön Data Factory yksiköt
Tässä vaiheessa voit julkaista Data Factory kohteiden (linkitetyn services, tietojoukkoja ja myyntijakso) aiemmin luomasi. Myös määrittää uusien tietojen factory, pitoon nämä objektit luodaan nimi.  

18. Projektin Solution Explorerissa hiiren kakkospainikkeella ja valitse sitten **Julkaise**. 
19. Jos näet **Microsoft-tilille kirjautuminen** -valintaikkuna, anna tunnistetiedot tilille, joka on Azure-tilaus ja valitse **Kirjaudu sisään**.
20. Näkyviin tulee seuraava valintaikkuna:

    ![Julkaise-valintaikkuna](./media/data-factory-copy-activity-tutorial-using-visual-studio/publish.png)
21. Valitse Määritä factory-sivulla seuraavat toimet: 
    1. Valitse **Luo uusi Data Factory** -vaihtoehto.
    2. Kirjoita **VSTutorialFactory** **nimi**.  
    
        > [AZURE.IMPORTANT]  
        > Azure tietojen factory nimen on oltava yksilöivä. Jos saat virheilmoituksen tietojen factory nimestä julkaistessasi, muuta nimi factory tiedot (esimerkiksi yournameVSTutorialFactory) ja kokeile julkaista uudelleen. Data Factory palvelutiedot nimeämiskäytäntö säännöt [Tietojen Factory - nimeäminen säännöt](data-factory-naming-rules.md) aiheessa.     
    3. Valitse Azure tilauksen **tilauksen** kentälle.
     
        > [AZURE.IMPORTANT]Jos jokin tilaus ei ole näkyvissä, varmista, että olet kirjautunut sisään käyttämällä tiliä, jolla on järjestelmänvalvojan tai Mää-järjestelmänvalvojan tilauksen.  
    4. Valitse tiedot factory luoda **resurssiryhmä** . 5. Valitse tiedot-factory **alue** . Vain tietojen Factory-palvelun tukemat alueiden näkyvät avattavasta luettelosta.
6. Valitse **Seuraava** siirtyäksesi **Julkaista kohteita** -sivulle.
    
        ![Tietoja factory sivun määrittäminen](media/data-factory-copy-activity-tutorial-using-visual-studio/configure-data-factory-page.png)   
23. **Julkaista kohteita** sivulla että kaikki tiedot-tehtaan kohteiden valitaan ja valitse **Seuraava** siirtyäksesi **Yhteenveto** -sivulle.
    
    ![Kohteiden sivun julkaiseminen](media/data-factory-copy-activity-tutorial-using-visual-studio/publish-items-page.png)     
24. Tarkista yhteenveto ja valitse **Seuraava** **Käyttöönoton tilan**ja käynnistää käyttöönottoprosessin.

    ![Julkaise yhteenveto-sivu](media/data-factory-copy-activity-tutorial-using-visual-studio/publish-summary-page.png)
25. **Käyttöönottotila** -sivulla näkyy käyttöönottoprosessin tila. Kun asennus on valmis, valitse Valmis. 
    ![Käyttöönoton tila-sivun](media/data-factory-copy-activity-tutorial-using-visual-studio/deployment-status.png) Huomaa seuraavat seikat: 

- Jos saat virheilmoituksen: "**tätä tilausta ei ole rekisteröity käyttämään nimitilan Microsoft.DataFactory**", tee jokin seuraavista toimista ja yritä julkaista uudelleen: 

    - PowerShellin Azure suorittamalla seuraavan komennon rekisteröidä Data Factory-palvelu. 
        
            Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
    
        Voit suorittaa tietojen tehdas palvelu rekisteröidyt Vahvista seuraava komento. 
    
            Get-AzureRmResourceProvider
    - Kirjaudu sisään käyttämällä Azure tilauksen [Azure portaaliin](https://portal.azure.com) ja siirry Data Factory-sivu (tai) luominen tietojen factory Azure-portaalissa. Tämä toiminto rekisteröi palveluntarjoajan puolestasi.
-   Tietoja factory nimi voidaan rekisteröidä myöhemmin ja näin ollen tulevat näkyviin Luettele DNS-nimi.

> [AZURE.IMPORTANT] Jos haluat luoda Data Factory esiintymät, sinun on oltava järjestelmänvalvojan/Mää-järjestelmänvalvojan Azure-tilaukseen

## <a name="summary"></a>Yhteenveto
Tässä opetusohjelmassa luomasi Azure tiedot-factory kopioitavat tiedot Azure blob Azure SQL-tietokantaan. Tietoja factory, linkitetyn services, tietojoukkoja ja putkijohto luomiseen käytetyt Visual Studio. Seuraavassa on ylätason olet suorittanut tämän opetusohjelman vaiheita:  

1.  Luoda Azure **tietojen factory**.
2.  Luo **linkitetty palvelut**:
    1. **Azuren tallennustilaan** linkitetty palvelu linkitettävän Azure-tallennustilan tilin, joka sisältää syöttötiedot.    
    2. Voit linkittää Azure SQL-tietokannan, joka sisältää tiedot linkitetty **Azure SQL** -palvelu. 
3.  Luoda **tietojoukkoja**, jotka kuvaavat syöttötiedot ja putkistot tulosteen tiedot.
4.  Luotu **myyntijakso** **Kopioi tehtävä** , jonka **BlobSource** lähde-ja **SqlSink** käsittelytoiminto nimellä. 


## <a name="use-server-explorer-to-view-data-factories"></a>Palvelimen Resurssienhallinnan avulla voit tarkastella tietoja tehtaan

1. **Visual Studiossa**Valitse **Näytä** -valikon ja **Palvelimen**hallinta.
2. Laajenna **Azure** ja laajenna **Data Factory**Server-ikkunan. Jos näet **Visual Studio Kirjaudu**, anna **tilin** Azure-tilaukseen liittyvää ja valitse **Jatka**. Kirjoita **salasana**ja valitse **Kirjaudu sisään**. Visual Studio yrittää kaikki Azure tietojen tehtaan tilaukseesi tietoja. Näet tilan toiminto **Tietojen Factory tehtäväluettelo** -ikkunassa.
    ![Palvelimen Explorer](./media/data-factory-copy-activity-tutorial-using-visual-studio/server-explorer.png)
3. Voit tietojen factory Napsauta hiiren kakkospainikkeella ja valitse Vie tiedot Factory uuden projektin luominen Visual Studio projektin olemassa olevat tiedot-factory perusteella.
    ![Vie tiedot tehdas ja projektin](./media/data-factory-copy-activity-tutorial-using-visual-studio/export-data-factory-menu.png)  

## <a name="update-data-factory-tools-for-visual-studio"></a>Päivitä tiedot Factory Työkalut Visual Studio
Päivitä Azure Data Factory Työkalut Visual Studio, toimi seuraavasti:

1. Valitse **Työkalut** -valikon ja valitse **tunnisteet ja päivitykset**. 
2. Valitse vasemmanpuoleisessa ruudussa **päivitykset** ja valitse sitten **Visual Studio valikoima**.
4. Valitse **Azure Data Factory tools for Visual Studio** ja valitse **Päivitä**. Jos tapahtuma ei ole näkyvissä, sinulla on jo työkalujen uusimman version. 

Saat ohjeet Azure portaalin avulla voit valvoa putkijohto ja tietojoukkoja olet luonut Tässä opetusohjelmassa [näytön tietojoukkoja ja myyntijakso](data-factory-copy-activity-tutorial-using-azure-portal.md#monitor-pipeline) .

## <a name="see-also"></a>Katso myös
| Aihe | Kuvaus |
| :---- | :---- |
| [Tietojen siirtämistä toiminnot](data-factory-data-movement-activities.md) | Tässä artikkelissa on tarkkoja tietoja opetusohjelman käytit kopioi tehtävän. |
| [Ajoittamisen ja suorittaminen](data-factory-scheduling-and-execution.md) | Tässä artikkelissa kerrotaan Azure Data Factory-sovelluksen malli ajoittaminen ja suorittamisen ominaisuuksia. |
| [Putkistot](data-factory-create-pipelines.md) | Tässä artikkelissa auttaa sinua ymmärtämään putkistot ja Azure Data Factory toimintoja |
| [Tietojoukkoja](data-factory-create-datasets.md) | Tässä artikkelissa auttaa sinua ymmärtämään Azure Data Factory tietojoukkoja.
| [Seurata ja hallita putkistot seuranta-sovelluksella](data-factory-monitor-manage-app.md) | Tässä artikkelissa kerrotaan, miten voit seurata ja hallita virheenkorjaus putkistot seuranta ja hallinta-sovelluksen käyttäminen. 
