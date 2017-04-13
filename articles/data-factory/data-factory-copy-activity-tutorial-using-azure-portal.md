<properties 
    pageTitle="Opetusohjelma: Luo putkijohto kopioi aktiviteettiin Azure-portaalissa | Microsoft Azure" 
    description="Tässä opetusohjelmassa luot Azure Data Factory-myyntijakso kopioi aktiviteettiin editorilla tietojen Factory Azure-portaalissa." 
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
    ms.date="09/16/2016" 
    ms.author="spelluru"/>

# <a name="tutorial-create-a-pipeline-with-copy-activity-using-azure-portal"></a>Opetusohjelma: Luo putkijohto kopioi aktiviteettiin Azure-portaalissa
> [AZURE.SELECTOR]
- [Yleiskatsaus ja edellytykset](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
- [Ohjattu kopiointi](data-factory-copy-data-wizard-tutorial.md)
- [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md)
- [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
- [PowerShellin](data-factory-copy-activity-tutorial-using-powershell.md)
- [Azure Resurssienhallinta-malli](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
- [REST-OHJELMOINTIRAJAPINNALLA](data-factory-copy-activity-tutorial-using-rest-api.md)
- [.NET-OHJELMOINTIRAJAPINTA](data-factory-copy-activity-tutorial-using-dotnet-api.md)



Tässä opetusohjelmassa näytetään, miten voit luoda ja seurata Azure-portaalissa Azure tiedot-factory. Tietoja factory putkijohto käyttää kopioi tehtävän tietojen kopioiminen Azure-Blob-säiliö Azure SQL-tietokantaan.

Näin voit suorittaa tässä opetusohjelmassa osana:

Vaihe | Kuvaus
-----| -----------
[Azure tietojen Factory luominen](#create-data-factory) | Tässä vaiheessa voit luoda nimeltä **ADFTutorialDataFactory**Azure tiedot-factory.  
[Luo linkitetty palvelut](#create-linked-services) | Tässä vaiheessa voit luoda kaksi linkitetyn services: **AzureStorageLinkedService** ja **AzureSqlLinkedService**. <br/><br/>AzureSqlLinkedService linkkien Azure SQL-tietokantaan ADFTutorialDataFactory ja AzureStorageLinkedService linkki Azure-tallennustilan. Syöttötiedot varten putkijohto sijaitsee blob-säilö Azure-blob storage ja tulosteen tiedot tallennetaan taulukon Azure SQL-tietokantaan. Tämän vuoksi lisäät nämä kaksi stores linkitetyn services tietojen factory.      
[Luo syötteen ja tulosteen tietojoukkoja](#create-datasets) | Edellisessä vaiheessa luomasi linkitetyn palvelut, jotka viittaavat tietoja tallennetuista tiedoista, jotka sisältävät i/tietoja. Tässä vaiheessa voit määrittää kahden tietojoukkoja-- **InputDataset** ja **OutputDataset** --, jotka edustavat i/tiedot, jotka on tallennettu tietoja tallennetuista tiedoista. <br/><br/>InputDataset voit määrittää blob-säilö, joka sisältää Blob-objektien lähdetiedot ja OutputDataset, voit määrittää SQL-taulukko, joka tallentaa tiedot. Voit myös määrittää muita ominaisuuksia, kuten rakenne, käytettävyys ja käytännön. 
[Luo putkijohto](#create-pipeline) | Tässä vaiheessa voit luoda **ADFTutorialPipeline** ADFTutorialDataFactory-niminen putkijohto. <br/><br/>**Kopioi toimintojen** lisääminen putkijohto, kopioi syötteen tietoja Azure blob-tulostus Azure SQL-taulukkoon. Kopioi tehtävän suorittaa tietojen siirto Azure Data Factory. Se on kytketty yleisesti saatavilla-palvelu, voit kopioida tiedot eri tavalla turvallinen, luotettava ja skaalattava tietojen stores välillä. Katso [Tietojen siirtämistä toimintoja](data-factory-data-movement-activities.md) artikkelissa lisätietoja kopio-tehtävän. 
[Näytön myyntijakso](#monitor-pipeline) | Tässä vaiheessa voit valvoa syöttö- ja taulukoiden sektoreita Azure-portaalissa.

## <a name="prerequisites"></a>Edellytykset 
Ennen kuin suoritat tässä opetusohjelmassa [Opetusohjelma yleiskatsaus](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) -artikkelissa luetellut edellytykset valmis.

## <a name="create-data-factory"></a>Tietoja factory luominen
Tässä vaiheessa käyttää Azure portaalin nimeltä **ADFTutorialDataFactory**Azure tiedot-factory luomiseen.

1.  Kun kirjautumisesta [Azure portal](https://portal.azure.com/), **Uusi**, valitse **liiketoimintatietojen + Analytics**ja valitse **Data Factory**. 

    ![DataFactory -> Uusi](./media/data-factory-copy-activity-tutorial-using-azure-portal/NewDataFactoryMenu.png)  

6. Valitse **uudet tiedot factory** -sivu:
    1. Kirjoita **ADFTutorialDataFactory** **nimi**. 
    
        ![Uusien tietojen factory-sivu](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-new-data-factory.png)

        Azure tietojen factory nimen on oltava **yksilöivä**. Jos näyttöön tulee seuraava virhesanoma, tietojen factory (esimerkiksi yournameADFTutorialDataFactory) nimen muuttaminen ja yritä uudelleen. Data Factory palvelutiedot nimeämiskäytäntö säännöt [Tietojen Factory - nimeäminen säännöt](data-factory-naming-rules.md) aiheessa.
    
            Data factory name “ADFTutorialDataFactory” is not available  
     
        ![Tietojen Factory nimi ei ole käytettävissä](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-data-factory-not-available.png)
    2. Valitse Azure **tilauksen**.
    3. Resurssiryhmän jokin seuraavista toimista:
        1. Valitse **Käytä aiemmin**ja valitse aiemmin luotu resurssiryhmä avattavasta luettelosta. 
        2. Valitse **Luo uusi**ja kirjoita resurssin nimi.   
    
            Seuraavia asioita Tässä opetusohjelmassa oletetaan, että käytät nimi: **ADFTutorialResourceGroup** resurssiryhmän. Tietoja resurssiryhmät-kohdassa [käyttäminen resurssiryhmät, jos haluat hallita Azure resursseja](../azure-resource-manager/resource-group-overview.md).  
    4. Valitse tiedot-factory **sijainti** . Vain tietojen Factory-palvelun tukemat alueiden näkyvät avattavasta luettelosta.
    5. Valitse **Kiinnitä Startboard**.     
    6. Valitse **Luo**.

        > [AZURE.IMPORTANT] Jos haluat luoda Data Factory esiintymät, on oltava tilauksen/resurssin ryhmittelytasolla [Tietojen Factory osallistuja](../active-directory/role-based-access-built-in-roles.md/#data-factory-contributor) -roolin jäsen.
        >  
        >  Tietoja factory nimi voidaan rekisteröidä myöhemmin ja näin ollen tulevat näkyviin Luettele DNS-nimi.              
9.  Tila-/ ilmoitukset näkyviin napsauttamalla työkalurivin Bellin-kuvaketta. 

    ![Ilmoitukset](./media/data-factory-copy-activity-tutorial-using-azure-portal/Notifications.png) 
10. Kun luominen on valmis, näet **Data Factory** -sivu kuvassa esitetyllä tavalla.

    ![Tietoja factory-kotisivu](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-data-factory-home-page.png)

## <a name="create-linked-services"></a>Luo linkitetty palvelut
Linkitetyn services linkittäminen Microsoft Data tai laskea Azure tietojen factory-palvelut. Katso [Tuetut tiedot tallennetaan](data-factory-data-movement-activities.md##supported-data-stores-and-formats) lähteiden ja kopioi tehtävän tukemat poistumia. Katso, Laske palveluluetteloon Data Factory tukemat [Laske linkitetyn palvelut](data-factory-compute-linked-services.md) . Tässä opetusohjelmassa et käytä mihinkään Laske-palveluun. 

Tässä vaiheessa voit luoda kaksi linkitetyn services: **AzureStorageLinkedService** ja **AzureSqlLinkedService**. AzureSqlLinkedService linkkien Azure SQL-tietokantaan **ADFTutorialDataFactory**ja AzureStorageLinkedService linkitetty palvelun linkit Azure-tallennustilan tilin. Voit luoda putkijohto jäljempänä tässä opetusohjelmassa, kopioi tiedot blob-säilö AzureStorageLinkedService AzureSqlLinkedService SQL taulukkoon.

### <a name="create-a-linked-service-for-the-azure-storage-account"></a>Luo linkitetty palvelu Azure-tallennustilan tilin
1.  Valitse **Data Factory** -sivu **tekijän ja ota käyttöön** ruutu käynnistää tietojen factory **Editorin** .

    ![Luo ja ota käyttöön ruutu](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-author-deploy-tile.png) 
5. **Editorin**napsauttamalla työkalurivin **tallentaa uudet tiedot** -painiketta ja **Azure** tallennustilan avattavasta valikosta. Raportissa pitäisi näkyä luominen Azure tallennustilan linkitetty-palvelun oikeanpuoleisen ruudun JSON mallia. 

    ![Editorin uudet tiedot store-painike](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-editor-newdatastore-button.png)    
6. Korvaa `<accountname>` ja `<accountkey>` tilin nimi ja tilin avainarvot Azure-tallennustilan tilin kanssa. 

    ![Editorin Blob-objektien tallennustilaan JSON](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-editor-blob-storage-json.png) 
6. Valitse **Ota käyttöön** työkalurivillä. Raportissa pitäisi näkyä puunäkymä käyttöön **AzureStorageLinkedService** nyt. 

    ![Editorin Blob-objektien tallennustilaan käyttöönotto](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-editor-blob-storage-deploy.png)

> [AZURE.NOTE]
> Katso lisätietoja JSON ominaisuudet [tiedot-/ Azure-Blob-objektien siirtäminen](data-factory-azure-blob-connector.md#azure-storage-linked-service) .

### <a name="create-a-linked-service-for-the-azure-sql-database"></a>Luo linkitetty palvelu Azure SQL-tietokantaan
1. **Tietojen Factory-editorin** **uudet tiedot tallennetaan** työkalurivin painiketta ja valitse **Azure SQL-tietokanta** avattavasta valikosta. Raportissa pitäisi näkyä JSON-mallin, kun luot linkitetty Azure SQL-palvelu oikeanpuoleisen ruudun.
2. Korvaa `<servername>`, `<databasename>`, `<username>@<servername>`, ja `<password>` nimillä Azure SQL-palvelimeen, tietokannan, tili ja salasana. 
3. Valitse Luo ja ota käyttöön **AzureSqlLinkedService**työkalurivin **käyttöönotto** .
4. Varmista, että näet **AzureSqlLinkedService** puunäkymän. 

> [AZURE.NOTE]
> Katso lisätietoja JSON ominaisuudet [Siirrä tiedot-/ Azure SQL-tietokantaan](data-factory-azure-sql-connector.md#azure-sql-linked-service-properties) .

## <a name="create-datasets"></a>Luo tietojoukkoja
Edellisessä vaiheessa luomasi linkitetyn services **AzureStorageLinkedService** ja **AzureSqlLinkedService** voit linkittää tiedot factory Azure-tallennustilan tilin ja Azure SQL-tietokantaan: **ADFTutorialDataFactory**. Tässä vaiheessa voit määrittää kahden tietojoukkoja-- **InputDataset** ja **OutputDataset** --, jotka edustavat i/tiedot, jotka on tallennettu tarkoitettu AzureStorageLinkedService ja AzureSqlLinkedService tietojen stores. InputDataset voit määrittää blob-säilö, joka sisältää Blob-objektien lähdetiedot ja OutputDataset, voit määrittää SQL-taulukko, joka tallentaa tiedot. 

### <a name="create-input-dataset"></a>Kirjoita tietojoukon luominen 
Tässä vaiheessa voit luoda tietojoukko nimeltä **InputDataset** , joka osoittaa blob-säilö vastaavan **AzureStorageLinkedService** linkitetty palvelun Azure-tallennustilan.

1. **Editorin** Data Factory varten, napsauta **... Lisää**, valitse **Uusi tietojoukko**ja valitse **Azure-Blob-säiliö** avattavasta valikosta. 

    ![Uusi tietojoukko-valikko](./media/data-factory-copy-activity-tutorial-using-azure-portal/new-dataset-menu.png)
2. Korvaa JSON oikeanpuoleisessa ruudussa seuraavat JSON koodikatkelman: 

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
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
              "folderPath": "adftutorial/",
              "fileName": "emp.txt",
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
 
    Jos et määritä **Tulostusalue** **Tiedostonimi** , **kansiopolku** luodut tiedostot nimetään muodossa: tiedot. &lt;Guid\&gt;. txt (Esimerkki: Data.0a405f8a 93ff-4c6f-b3be-f69616f1df7a.txt.).

    Voit määrittää **kansiopolku** ja **Tiedostonimi** dynaamisesti **SliceStart** ajan perusteella, **partitionedBy** -ominaisuuden avulla. Seuraavassa esimerkissä kansiopolku käyttää vuoden, kuukauden ja päivän SliceStart (käsitellään sektoria alkamispäivää)- ja tiedostonimi käyttää tunti-SliceStart. Jos sektoria on yhdistämällä 2016-09-20T08:00:00, kansionimi on määritetty wikidatagateway/wikisampledataout/2016/09/20 ja tiedostonimi on määritetty 08.csv. 

            "folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
            "fileName": "{Hour}.csv",
            "partitionedBy": 
            [
                { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
                { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } }, 
                { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } }, 
                { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } } 
            ],
2. Valitse **Ota käyttöön** luoda ja ottaa käyttöön **InputDataset** tietojoukko-painiketta. Varmista, että näet **InputDataset** puunäkymässä.

> [AZURE.NOTE]
> Katso lisätietoja JSON ominaisuudet [tiedot-/ Azure-Blob-objektien siirtäminen](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties) .

### <a name="create-output-dataset"></a>Tulosteen tietojoukon luominen
Tässä osassa vaiheen Luo nimetty **OutputDataset**tulostus-tietojoukko. Tämä tietojoukko asioista SQL Azure SQL-tietokantaan, joita edustaa **AzureSqlLinkedService**taulukkoon. 

1. **Editorin** Data Factory varten, napsauta **... Lisää**, valitse **Uusi tietojoukko**ja valitse **Azure SQL** -pudotusvalikosta. 
2. Korvaa JSON oikeanpuoleisessa ruudussa seuraavat JSON koodikatkelman:

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
            "linkedServiceName": "AzureSqlLinkedService",
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

3. Valitse **Ota käyttöön** luoda ja ottaa käyttöön **OutputDataset** tietojoukko-painiketta. Varmista, että näet **OutputDataset** puunäkymässä. 

> [AZURE.NOTE]
> Katso lisätietoja JSON ominaisuudet [Siirrä tiedot-/ Azure SQL-tietokantaan](data-factory-azure-sql-connector.md#azure-sql-linked-service-properties) .

## <a name="create-pipeline"></a>Myyntijakso luominen
Tässä vaiheessa voit luoda putkijohto **Kopioi tehtävän** , joka käyttää **InputDataset** syötteenä ja **OutputDataset** kuin tulos.

1. **Editorin** Data Factory varten, napsauta **... Lisää**, ja valitse **Uusi myyntijakso**. Voit vaihtoehtoisesti Napsauta **putkistot** puunäkymässä ja valitse **Uusi myyntijakso**.
2. Korvaa JSON oikeanpuoleisessa ruudussa seuraavat JSON koodikatkelman: 
        
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
                  "retry": 0,
                  "timeout": "01:00:00"
                }
              }
            ],
            "start": "2016-07-12T00:00:00Z",
            "end": "2016-07-13T00:00:00Z"
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
    
4. Valitse Luo ja ota käyttöön **ADFTutorialPipeline**työkalurivin **käyttöönotto** . Vahvista, näet putkijohto puunäkymässä. 
5. Sulje nyt **Editor** -sivu valitsemalla **X**. Valitse uudelleen, jos haluat nähdä **Data Factory** kotisivun **ADFTutorialDataFactory** **X** .

**Onnittelen!** Olet luonut Azure tietojen factory, linkitetyn services, taulukoita ja putkijohto ja ajoitettu putkijohto.   
 
### <a name="view-the-data-factory-in-a-diagram-view"></a>Verkkokaavio-näkymän tiedot factory tarkasteleminen 
1. Valitse **kaavion** **Tietojen Factory** -sivu.

    ![Tietoja Factory sivu - kaavio-ruutu](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-datafactoryblade-diagramtile.png)
2. Kaavion samalla tavalla kuin seuraavassa kuvassa pitäisi näkyä seuraavat: 

    ![Kaavionäkymä](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-diagram-blade.png)

    Voit lähentää, loitontaa, zoomaus 100 %, Zoomaus Sovita, sijoita putkistot ja taulukoiden automaattisesti ja Näytä periytymistiedot (korostaa ja alaspäin kohteet valittujen kohteiden).  Voit kaksoisnapsauttaa objektin (i/taulukon tai myyntijakso) haluamasi ominaisuudet. 
3. Napsauta **ADFTutorialPipeline** kaavionäkymään ja valitse **Avaa myyntijakso**. 

    ![Avaa myyntijakso](./media/data-factory-copy-activity-tutorial-using-azure-portal/DiagramView-OpenPipeline.png)
4. Raportissa pitäisi näkyä syöttö- ja tietojoukkoja toiminnasta sekä putkisto toimintoja. Tässä opetusohjelmassa on vain yksi tehtävä putkijohto (kopioi tehtävä) ja InputDataset syötteen tietojoukko ja OutputDataset kuin tulosteen tietojoukko.   

    ![Myyntijakso-näkymän avaaminen](./media/data-factory-copy-activity-tutorial-using-azure-portal/DiagramView-OpenedPipeline.png)
5. Valitse **tietojen factory** linkkipolussa palaa verkkokaavio-näkymän vasemmassa yläkulmassa. Kaavionäkymässä näkyvät kaikkien putkistot. Tässä esimerkissä olet luonut vain yksi putkijohto.   
 

## <a name="monitor-pipeline"></a>Näytön myyntijakso
Tässä vaiheessa käyttö Azure portaalin seurannassa mistä on kyse Azure tietojen factory. 

### <a name="monitor-pipeline-using-diagram-view"></a>Näytön myyntijakso käyttämällä

1. Valitse Sulje **kaavionäkymään voit tarkastella tietoja factory Data Factory kotisivulle** **X** . Jos olet sulkenut web-selaimessa, toimi seuraavasti: 
    2. Siirry [Azure](https://portal.azure.com/)-portaaliin. 
    2. Kaksoisnapsauta **ADFTutorialDataFactory** - **Startboard** (tai) **tietojen tehtaan** valitsemalla vasemmalla olevasta valikosta ja Hae ADFTutorialDataFactory. 
3. Laske- ja nimet ja taulukoiden myyntijakso, jonka loit tämä sivu pitäisi näkyä.

    ![Aloitussivu ja nimet](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-datafactory-home-page-pipeline-tables.png)
4. Napsauta nyt **tietojoukkoja** -ruutua.
5. Valitse **InputDataset** **tietojoukkoja** -sivu. Tämä tietojoukko on **ADFTutorialPipeline**näytettävä tietojoukkoa.

    ![Tietojoukkoja valittuna InputDataset kanssa](./media/data-factory-copy-activity-tutorial-using-azure-portal/DataSetsWithInputDatasetFromBlobSelected.png)   
5. Valitse **... (kolme pistettä)** Jos haluat nähdä kaikki tiedot sektoria.

    ![Kaikki syötteen tietoja sektorit](./media/data-factory-copy-activity-tutorial-using-azure-portal/all-input-slices.png)  

    Huomaa, että kaikki tiedot nykyisen kellonajan ylöspäin sektorit on **Valmis** , koska **emp.txt** -tiedosto on aina blob-säilö: **adftutorial\input**. Vahvista alaosassa **viimeksi epäonnistui sektorit** -osassa näkyvät ei sektoreita.

    Sekä **viimeksi päivitetty sektoreiden** ja **epäonnistui viimeksi sektoreiden** luetteloiden lajitellaan **VIIMEISIMMÄN päivityksen aika**. 
    
    Valitse **Suodata** suodattaa sektorit-painiketta.  
    
    ![Suodata syötteen sektorit](./media/data-factory-copy-activity-tutorial-using-azure-portal/filter-input-slices.png)
6. Sulje näiden, kunnes näet **tietojoukkoja** -sivu. Valitse **OutputDataset**. Tämä tietojoukko on **ADFTutorialPipeline**tulosteen tietojoukkoa.

    ![arvojoukon sivu](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-datasets-blade.png)
6. **OutputDataset** sivu näkyy seuraavassa kuvassa esitetyllä tavalla:

    ![taulukko-sivu](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-table-blade.png) 
7. Huomaa, että tietojen sektorit ylöspäin nykyisen kellonajan on jo valmistettu ja ne ovat **valmiita**. Ei ole sektoreiden näkyvät alareunassa **ongelma sektorit** -osiossa.
8. Valitse **... (Kolme pistettä)** Jos haluat nähdä sektorit.

    ![tietoja sektoreiden sivu](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-dataslices-blade.png)
9. Napsauta mitä tahansa tietojen sektoria luettelosta ja näkyviin tulee **tietojen muotoilu** -sivu.

    ![tietojen muotoilu-sivu](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-dataslice-blade.png)
  
    Jos sektoria ei ole **Valmis** -tilaan, näet seuraavat sektorit, jotka eivät ole valmis ja estävät nykyinen sektori suorittaminen **ylöspäin sektoreiden, joita ei ole valmis** -luettelossa.
11. **Tietojen MUOTOILU** -sivu näkyy kaikki toimii luettelon alaosassa. Valitse **tehtävän suorittaminen** Nähdäksesi **tehtävän suorittaminen tiedot** -sivu. 

    ![Tehtävän suorittaminen tiedot](./media/data-factory-copy-activity-tutorial-using-azure-portal/ActivityRunDetails.png)
12. Valitse **X** Sulje kaikki näiden, ennen kuin voit palata koti sivu **ADFTutorialDataFactory**varten.
14. (valinnainen) Valitse **putkistot** **ADFTutorialDataFactory**aloitussivulla, valitse **ADFTutorialPipeline** **putkistot** sivu ja siirtyä syötteen (**Kulutettu**) tai taulukoiden tulos (**valmistetuista**).
15. Käynnistä **SQL Server Management Studiossa**, muodosta yhteys Azure SQL-tietokanta ja varmista, että rivit lisätään tietokannan **emp** -taulukkoon.

    ![SQL-kyselytulokset](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-sql-query-results.png)

### <a name="monitor-pipeline-using-monitor--manage-app"></a>Näytön myyntijakso näyttö ja hallinta-sovelluksen käyttäminen
Voit myös käyttää näytön & seurannassa oman putkistot-sovelluksen hallinta. Lisätietoja tämän sovelluksen käyttämisestä on artikkelissa [näyttö ja hallita Azure Data Factory putkistot seuranta ja hallinta-sovelluksen käyttäminen](data-factory-monitor-manage-app.md).

1. Valitsemalla **näytön ja hallinta** tietojen factory kotisivulle ruutu.

    ![Seurata ja hallita ruutu](./media/data-factory-copy-activity-tutorial-using-azure-portal/monitor-manage-tile.png) 
2. Raportissa pitäisi näkyä **näyttö ja hallinta-sovellus**. Muuta **alkamisaika** ja **Päättymisaika** lisätä Käynnistä (2016-07 – 12) ja päättymisajat (2016-07 – 13), että putkijohto ja valitse **Käytä**. 

    ![Seuranta ja hallinta-sovelluksella](./media/data-factory-copy-activity-tutorial-using-azure-portal/monitor-and-manage-app.png) 
3. Valitse tehtävä-ikkunassa **Toiminta Windows** -luettelon ja tarkastella sen tietoja. 
    ![Tehtävän ikkunan tiedot](./media/data-factory-copy-activity-tutorial-using-azure-portal/activity-window-details.png)

## <a name="summary"></a>Yhteenveto 
Tässä opetusohjelmassa luomasi Azure tiedot-factory kopioitavat tiedot Azure blob Azure SQL-tietokantaan. Tietoja factory, linkitetyn services, tietojoukkoja ja putkijohto luomiseen käytetyt Azure portaalin. Seuraavassa on ylätason olet suorittanut tämän opetusohjelman vaiheita:  

1.  Luoda Azure **tietojen factory**.
2.  Luo **linkitetty palvelut**:
    1. **Azuren tallennustilaan** linkitetty palvelu linkitettävän Azure-tallennustilan tilin, joka sisältää syöttötiedot.    
    2. Voit linkittää Azure SQL-tietokannan, joka sisältää tiedot linkitetty **Azure SQL** -palvelu. 
3.  Luoda **tietojoukkoja** , jotka kuvaavat syöttötiedot ja putkistot tulosteen tiedot.
4.  Luotu **myyntijakso** **Kopioi tehtävä** , jonka **BlobSource** lähde-ja **SqlSink** käsittelytoiminto nimellä.  


## <a name="see-also"></a>Katso myös
| Aihe | Kuvaus |
| :---- | :---- |
| [Tietojen siirtämistä toiminnot](data-factory-data-movement-activities.md) | Tässä artikkelissa on tarkkoja tietoja opetusohjelman käytit kopioi tehtävän. |
| [Ajoittamisen ja suorittaminen](data-factory-scheduling-and-execution.md) | Tässä artikkelissa kerrotaan Azure Data Factory-sovelluksen malli ajoittaminen ja suorittamisen ominaisuuksia. |
| [Putkistot](data-factory-create-pipelines.md) | Tässä artikkelissa auttaa sinua ymmärtämään putkistot ja Azure Data Factory toimintoja. |
| [Tietojoukkoja](data-factory-create-datasets.md) | Tässä artikkelissa auttaa sinua ymmärtämään Azure Data Factory tietojoukkoja.
| [Seurata ja hallita putkistot seuranta-sovelluksella](data-factory-monitor-manage-app.md) | Tässä artikkelissa kerrotaan, miten voit seurata ja hallita virheenkorjaus putkistot seuranta ja hallinta-sovelluksen käyttäminen. 


