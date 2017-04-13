<properties 
    pageTitle="Siirrä tiedot – Data Management Gateway | Microsoft Azure"
    description="Tietojen siirtäminen paikallisen käyttöön ja määrittää yhdyskäytävän tiedot. Azure Data Factory Data Management Gatewayn avulla voit siirtää tietoja." 
    keywords="yhdyskäytävän tiedot tietojen integrointi siirtää tietoja, yhdyskäytävän tunnistetiedot"
    services="data-factory" 
    documentationCenter="" 
    authors="linda33wj" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/18/2016" 
    ms.author="jingwang"/>

# <a name="move-data-between-on-premises-sources-and-the-cloud-with-data-management-gateway"></a>Tietojen siirtäminen paikallisesti ja Data Management Gatewayn pilveen
Tässä artikkelissa on yleiskatsaus tietojen integroinnin paikallisen tiedot tallennetaan, ja käyttämällä Data Factory cloud tietojen stores. Se muodostaa [Tietojen siirtämistä toiminnot](data-factory-data-movement-activities.md) on artikkelissa ja muiden tietojen factory core käsitteitä artikkelit: [tietojoukkoja](data-factory-create-datasets.md) ja [putkistot](data-factory-create-pipelines.md). 

## <a name="data-management-gateway"></a>Data Management Gateway
Käyttöön siirtäminen tiedot ja paikallisen-tietovarasto paikalliseen tietokoneeseen on asennettava Data Management Gateway. Yhdyskäytävän voi asentaa samaan tietokoneeseen kuin tietovaraston tai toiseen tietokoneeseen, kunhan yhdyskäytävän muodostaa yhteyden tietovaraston. 

> [AZURE.IMPORTANT] Katso [Data Management Gateway](data-factory-data-management-gateway.md) -artikkelissa lisätietoja Data Management Gateway.   

Seuraava vaihe vaiheelta avulla voit luoda tietojen factory putkijohto, joka siirtää tietoja Azure-blob-säiliö paikallisen **SQL Server** -tietokantaan. Osana ongelmatilanteita Asenna ja määritä Data Management Gateway käyttämääsi laitteeseen. 

## <a name="walkthrough-copy-on-premises-data-to-cloud"></a>Vaiheittainen kuvaus: kopioi paikalliset tiedot pilveen
  
## <a name="create-data-factory"></a>Tietoja factory luominen
Tässä vaiheessa käyttää Azure portaalin erillisen Azure Data Factory-niminen **ADFTutorialOnPremDF**luomiseen. 

1.  Kirjaudu sisään [Azure portal](https://portal.azure.com). 
2.  Valitse **+ Uusi**, valitse **liiketoimintatietojen + analytics**ja sitten **Data Factory**.

    ![DataFactory -> Uusi](./media/data-factory-move-data-between-onprem-and-cloud/NewDataFactoryMenu.png)  
2. Kirjoita nimi **ADFTutorialOnPremDF** **uudet tiedot factory** -sivu.

    ![Startboard lisääminen](./media/data-factory-move-data-between-onprem-and-cloud/OnPremNewDataFactoryAddToStartboard.png)

    > [AZURE.IMPORTANT] 
    > Azure tietojen factory nimen on oltava yksilöivä. Jos saat virheilmoituksen: **tietojen factory nimi "ADFTutorialOnPremDF" ei ole käytettävissä**, tietojen factory (esimerkiksi yournameADFTutorialOnPremDF) nimen muuttaminen ja yritä uudelleen. Tätä nimeä tekstin näyttäminen ADFTutorialOnPremDF käyttää suorittaessasi tämän opetusohjelman vaiheita.
    > 
    > Tietoja factory nimi voidaan rekisteröidä myöhemmin ja näin ollen tulevat näkyviin Luettele **DNS** -nimi.
3. Valitse kohtaa, johon haluat luoda tietojen tehdas **Azure tilauksen** . 
4.  Valitse aiemmin luotu **resurssiryhmä** tai luo resurssiryhmä. Luo opetusohjelman nimeltä resurssiryhmä: **ADFTutorialResourceGroup**. 
5.  Valitse **Luo** **Uusi tietojen factory** -sivu.

    > [AZURE.IMPORTANT] Jos haluat luoda Data Factory esiintymät, on oltava tilauksen/resurssin ryhmittelytasolla [Tietojen Factory osallistuja](../active-directory/role-based-access-built-in-roles.md/#data-factory-contributor) -roolin jäsen. 
11. Kun luominen on valmis, näyttöön tulee **Data Factory** -sivu, seuraavassa kuvassa esitetyllä tavalla:

    ![Tietoja Factory-kotisivu](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDataFactoryHomePage.png)

## <a name="create-gateway"></a>Yhdyskäytävän luominen
5. Valitse **Data Factory** -sivu **tekijän ja ota käyttöön** ruutu käynnistää tietojen factory **Editorin** .

    ![Luo ja ota käyttöön ruutu](./media/data-factory-move-data-between-onprem-and-cloud/author-deploy-tile.png) 
6.  Tietoja Factory editori valitsemalla **... Lisää** työkalurivin ja valitse sitten **Uusi tietojen yhdyskäytävä**. Voit vaihtoehtoisesti **Tietojen yhdyskäytävien** Kaksoisnapsauta puunäkymässä ja valitse **Uusi tietojen yhdyskäytävä**. 

    ![Uusien tietojen yhdyskäytävän työkalurivi](./media/data-factory-move-data-between-onprem-and-cloud/NewDataGateway.png)
2. Valitse **Luo** -sivu kirjoittaa **adftutorialgateway** **nimi**ja valitse **OK**.    

    ![Luo yhdyskäytävä-sivu](./media/data-factory-move-data-between-onprem-and-cloud/OnPremCreateGatewayBlade.png)
3. Valitse **määritys** -sivu, **Asenna suoraan tietokoneeseen**. Tämä toiminto lataa yhdyskäytävän asennuspaketti, asentaa, määrittää ja rekisteröi yhdyskäytävä tietokoneeseen.  

    > [AZURE.NOTE] 
    > Käytä Internet Explorer tai Microsoft ClickOnce yhteensopiva selain.
    > 
    > Jos käytät Chrome, siirry [Chrome-web-säilön](https://chrome.google.com/webstore/), Etsi "ClickOnce-avainsanalla, valitse jokin ClickOnce-laajennukset ja asenna se. 
    >  
    > Toimi samoin Firefox (Asenna laajennus). **Avaa valikko** -painiketta (**kolme vaakaviivat** oikeassa alakulmassa)-työkalurivin **lisäosat**, Etsi "ClickOnce-avainsanalla, jokin ClickOnce-laajennukset ja asenna se.    

    ![Yhdyskäytävä - sivu määrittäminen](./media/data-factory-move-data-between-onprem-and-cloud/OnPremGatewayConfigureBlade.png)

    Tämä tapa on helpoin tapa (yhdellä napsautuksella) lataaminen, asentaminen, määrittäminen ja rekisteröi yhdyskäytävä yksi vaihe. Näet **Microsoft Data Management Gatewayn määritysten hallinta** -sovellus on asennettu tietokoneeseen. Löytyvät myös suoritettavan **ConfigManager.exe** kansioon: **C:\Program Files\Microsoft Data Management Gateway\2.0\Shared**.

    Voit myös lataaminen ja asentaminen yhdyskäytävän manuaalisesti linkkien avulla tämä sivu ja rekisteröi se näkyy **Uusi avain** tekstiruutuun avaimen avulla.
    
    Katso [Data Management Gateway](data-factory-data-management-gateway.md) -artikkelissa yhdyskäytävän tiedot.

    >[AZURE.NOTE] Sinun on oltava järjestelmänvalvoja paikalliseen tietokoneeseen asennetaan ja määritetään Data Management Gateway onnistuneesti. Voit lisätä muita käyttäjiä **Data Management yhdyskäytävän käyttäjät** paikalliseen Windows-ryhmään. Tämän ryhmän jäsenet voivat määrittää yhdyskäytävän Data Management Gatewayn määritysten hallinta-työkalun avulla. 

5. Odota pari minuuttia ja odota, kunnes näet seuraavan ilmoituksen sanoman:

    ![Yhdyskäytävän asennus onnistui](./media/data-factory-move-data-between-onprem-and-cloud/gateway-install-success.png) 
6. Käynnistä tietokone **Data Management Gatewayn määritysten hallinta** -sovelluksen. Kirjoita **Etsi** -ikkunassa voit käyttää tätä apuohjelmaa **Data Management Gateway** . Löytyvät myös suoritettavan **ConfigManager.exe** kansioon: **C:\Program Files\Microsoft Data Management Gateway\2.0\Shared** 

    ![Gatewayn määritysten hallinta](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDMGConfigurationManager.png)
6. Varmista, että `adftutorialgateway is connected to the cloud service` viestin. Alareuna Tilarivi näyttää **yhteys on muodostettu pilvipalveluun** sekä **Vihreä valintamerkki**.

    Valitse **Aloitus** -välilehdessä voit tehdä myös seuraavat toimenpiteet: 
    - **Voit rekisteröidä** yhdyskäytävän avaimen Azure-portaalista Rekisteröi-painikkeella. 
    - **Lopeta** Data Management Gateway isäntäpalvelu yhdyskäytävän tietokoneessa käynnissä. 
    - **Ajoita päivitykset** on asennettu päivän tiettynä ajankohtana. 
    - Näkymään, jos yhdyskäytävä on **päivitetty**.
    - Määritä aika, jolloin yhdyskäytävän päivitys voidaan asentaa. 

8. Siirry **asetukset** -välilehti. **Varmenne** -kohdan varmennetta käytetään salaus/salauksen purkaminen paikallisen tietovaraston, joka on määritetty portaalin tunnistetiedot. (valinnainen) Valitse **Muuta** käyttämään oman allekirjoituksen. Yhdyskäytävän käyttää oletusarvon mukaan varmenne, joka on automaattisesti luodut Data Factory-palvelu.

    ![Yhdyskäytävän varmenne määritys](./media/data-factory-move-data-between-onprem-and-cloud/gateway-certificate.png)

    Voit tehdä myös **asetukset** -välilehdessä seuraavat toimet: 
    - Tarkastele tai yhdyskäytävän käyttää varmenteen vieminen.
    - Voit muuttaa yhdyskäytävän käyttämä HTTPS-päätepiste.    
    - Määritä HTTP-välityspalvelin käytettävän yhdyskäytävän.   
9. (valinnainen) **Diagnostiikka** -välilehteen, valitse **yksityiskohtainen kirjaus käyttöön** -vaihtoehto, jos haluat ottaa käyttöön yksityiskohtainen kirjaus, voit käyttää mitä tahansa yhdyskäytävän ongelmien vianmääritystä. Valitse **sovellukset- ja palvelulokit** **Tapahtumienvalvonta** löytyvät kirjaamistietoja -> **Data Management Gateway** -solmu. 

    ![Diagnostiikka-välilehti](./media/data-factory-move-data-between-onprem-and-cloud/diagnostics-tab.png)

    Voit suorittaa seuraavia toimintoja myös **Diagnostiikka** -välilehdessä: 
    
    - Käytä **Testiyhteys** osaa käyttämällä yhdyskäytävän paikalliseen tietolähteeseen.
    - Valitse **Näytä lokit** Katso Tapahtumienvalvonta-ikkunan Data Management Gateway-loki. 
    - Valitse **Lähetä lokit** kanssa viimeisten seitsemän päivän aikana lokit zip-tiedoston lataaminen Microsoft helpottamiseksi oman ongelmien vianmäärityksen. 
10. Valitse **Diagnostiikka** -välilehti, valitse **Testaa yhteys** -osassa tietovaraston tyypin **SQL Server** , tietokantapalvelimen tietokannan nimi sekä nimi, Määritä todennustyyppi, käyttäjänimi ja salasana ja valitsemalla **Testaa** ja testaa, onko yhdyskäytävän muodostaa yhteyden tietokantaan. 
11. Siirry selaimella ja **Azure portal**, valitse **OK** , **Määritä** sivu ja sitten **Uusi tietojen yhdyskäytävä** -sivu.
6. Raportissa pitäisi näkyä **adftutorialgateway** **Tietojen yhdyskäytävät** -kohdassa vasemman reunan puunäkymässä.  Jos valitset sen pitäisi näkyä liittyvät JSON. 
    

## <a name="create-linked-services"></a>Luo linkitetty palvelut 
Tässä vaiheessa voit luoda kaksi linkitetyn services: **AzureStorageLinkedService** ja **SqlServerLinkedService**. **SqlServerLinkedService** linkittää paikallisen SQL Server-tietokantaan ja **AzureStorageLinkedService** linkitetyistä linkit Azure-blob tallentaa tiedot valmiiksi. Voit luoda putkijohto myöhemmin tätä vaiheittaista, kopioi paikallisen SQL Server-tietokannan tietojen Azure-blob-säilö. 

#### <a name="add-a-linked-service-to-an-on-premises-sql-server-database"></a>Lisää linkitetyn palvelu paikallisen SQL Server-tietokantaan
1.  **Tietoja Factory-editorin**Valitse työkaluriviltä **Uusi tietovaraston** ja valitse **SQL Server**. 

    ![Uusi linkitetty SQL Server-palvelu](./media/data-factory-move-data-between-onprem-and-cloud/NewSQLServer.png) 
3.  **JSON-editorin** oikealla toimi seuraavasti: 
    1. Määritä **gatewayName** **adftutorialgateway**. 
    2. **ConnectionString**tee seuraavat toimet: 
        1. Kirjoita **palvelimen nimi**, SQL Server-tietokantaan isännöivän palvelimen nimi.
        2. Kirjoita **nimi**-tietokannan nimi.
        3. Napsauta työkalurivin painiketta **Salaa** . Lataa, ja avaa tunnistetietojen hallinta-sovelluksen.
        
            ![Tunnistetietojen hallinta-sovelluksen](./media/data-factory-move-data-between-onprem-and-cloud/credentials-manager-application.png)
        5. Valitse **Asetus tunnistetiedot** -valintaikkunassa todennustyyppi, käyttäjänimi ja salasana ja valitse **OK**. Jos yhteys on muodostettu, salattuja käyttäjätiedot tallennetaan JSON ja sulkee valintaikkunan. 
        6. Sulje tyhjä selaimen välilehteä, joka käynnistää valintaikkuna, jos se ei ole automaattisesti suljettu ja palata välilehti Azure-portaalissa. 
  
            **Yhdyskäytävän tietokoneessa näiden tunnistetiedot salataan sertifikaatin, joka omistaa Data Factory-palvelun avulla.** Jos haluat käyttää varmennetta, joka on liitetty Data Management Gateway sen sijaan, katso [Määritä tunnistetiedot suojatusti](#set-credentials-and-security).    
    1.  Valitse **Ota käyttöön** komentopalkista linkitetty SQL Server-palvelun käyttöön. Raportissa pitäisi näkyä linkitetyn palvelun puunäkymässä. 
        
        ![Puunäkymän linkitetty SQL Server-palvelu](./media/data-factory-move-data-between-onprem-and-cloud/sql-linked-service-in-tree-view.png)  

#### <a name="add-a-linked-service-for-an-azure-storage-account"></a>Linkitetyn palvelun Azure-tallennustilan tilin lisääminen
 
1. **Tietojen Factory editorin**Valitse komentopalkin **tallentaa uudet tiedot** ja valitse **Azure-tallennustilan**.
2. Kirjoita **tilin nimi**Azure-tallennustilan tilin nimi.
3. Kirjoita **tilin avain**Azure-tallennustilan tilin avain.
4. Valitse **Ota** käyttöön **AzureStorageLinkedService**.
   
 
## <a name="create-datasets"></a>Luo tietojoukkoja
Tässä vaiheessa voit luoda syötteen ja tulosteen tietojoukkoja, jotka edustavat syöttö- ja tietojen kopiointi (paikallisen SQL Server-tietokantaan = > Azure-blob-säiliö). Ennen kuin luot tietojoukkoja, tee (lisätietoja kohdasta seuraa luettelon) seuraavasti:

- Luo SQL Server-tietokantaan lisäämäsi linkitetyn palveluna tietojen factory **emp** -taulukon ja Lisää muutama Esimerkki tapahtumat-taulukkoon.
- Luo **adftutorial** Azure-blob-tallennustilan tilin lisäämäsi linkitetyn palveluna tietojen factory-blob-säilö.

### <a name="prepare-on-premises-sql-server-for-the-tutorial"></a>Paikallisen SQL Serverin valmisteleminen opetusohjelman

1. Määritetyn paikallisen SQL Server-tietokannan linkitetyn service (**SqlServerLinkedService**) avulla SQL-komentosarja **emp** -taulukon luominen tietokannassa.

        CREATE TABLE dbo.emp
        (
            ID int IDENTITY(1,1) NOT NULL, 
            FirstName varchar(50),
            LastName varchar(50),
            CONSTRAINT PK_emp PRIMARY KEY (ID)
        )
        GO 
2. Lisää joitakin otosten taulukkoon: 

        INSERT INTO emp VALUES ('John', 'Doe')
        INSERT INTO emp VALUES ('Jane', 'Doe')

### <a name="create-input-dataset"></a>Kirjoita tietojoukon luominen

1. Valitse **Tietojen Factory editorin** **... Lisää**, valitse **Uusi tietojoukko** komentopalkista ja **SQL Server**-taulukko. 
2.  Korvaa JSON oikeanpuoleiseen ruutuun seuraava teksti:
        
            {       
                "name": "EmpOnPremSQLTable",
                "properties": {
                    "type": "SqlServerTable",
                    "linkedServiceName": "SqlServerLinkedService",
                    "typeProperties": {
                        "tableName": "emp"
                    },
                    "external": true,
                    "availability": {
                        "frequency": "Hour",
                        "interval": 1
                    },
                    "policy": {
                        "externalData": {
                            "retryInterval": "00:01:00",
                            "retryTimeout": "00:10:00",
                            "maximumRetry": 3
                        }
                    }
                }
            }    

    Huomaa seuraavat seikat: 

    - **tyyppi** -asetukseksi on määritetty **SqlServerTable**.
    - **taulukon nimi** on määritetty **emp**.
    - **linkedServiceName** on määritetty **SqlServerLinkedService** (oli linkitetyn palvelua aiemmin luomasi tämän vaiheittaisen kuvauksen suorittamiseksi.).
    - Syötteen tietojoukko, joka toisen myyntijakso Azure Data Factory-ei luoda, kun määrität **ulkoisen** **Tosi**. Se merkitsee syöttötiedot on valmistettu ulkoisen Azure Data Factory-palveluun. Voit myös määrittää **käytännön** osan **externalData** -osan avulla ulkoisia tietokäytännöt.    

    Katso lisätietoja JSON ominaisuudet [siirtää tietoja, tai SQL Serveristä](data-factory-sqlserver-connector.md) .
2. Valitse **Ota käyttöön** ottamaan dataset komentopalkista.  


### <a name="create-output-dataset"></a>Tulosteen tietojoukon luominen

1.  **Tietojen Factory-editori**Valitse **Uusi tietojoukko** komentopalkista ja sitten **Azure-Blob-säiliö**.
2.  Korvaa JSON oikeanpuoleiseen ruutuun seuraava teksti: 

            {
                "name": "OutputBlobTable",
                "properties": {
                    "type": "AzureBlob",
                    "linkedServiceName": "AzureStorageLinkedService",
                    "typeProperties": {
                        "folderPath": "adftutorial/outfromonpremdf",
                        "format": {
                            "type": "TextFormat",
                            "columnDelimiter": ","
                        }
                    },
                    "availability": {
                        "frequency": "Hour",
                        "interval": 1
                    }
                }
            }
  
    Huomaa seuraavat seikat: 
    
    - **tyyppi** -asetukseksi on määritetty **AzureBlob**.
    - **linkedServiceName** on määritetty **AzureStorageLinkedService** (olit luonut linkitetyn palvelua vaiheessa 2).
    - **kansiopolku** on määritetty **adftutorial/outfromonpremdf** missä outfromonpremdf adftutorial säilössä kansio. Luo **adftutorial** säilö, jos sitä ei ole jo olemassa. 
    - **Käytettävyys** on määritetty **kerran tunnissa** (**taajuus** arvoksi **Tunti** ja **aikavälin** arvoksi **1**).  Data Factory-palvelun Luo tulostus-tietojen sektoria tunnissa **emp** taulukon Azure SQL-tietokantaan. 

    Jos et määritä **Tulostusalue** **Tiedostonimi** , **kansiopolku** luodut tiedostot nimetään muodossa: tiedot. <Guid>.txt (esimerkiksi:: Data.0a405f8a 93ff-4c6f-b3be-f69616f1df7a.txt.).

    Voit määrittää **kansiopolku** ja **Tiedostonimi** dynaamisesti **SliceStart** ajan perusteella, partitionedBy-ominaisuuden avulla. Seuraavassa esimerkissä kansiopolku käyttää vuoden, kuukauden ja päivän SliceStart (käsitellään sektoria alkamispäivää)- ja tiedostonimi käyttää tunti-SliceStart. Jos sektoria on yhdistämällä 2014, esimerkiksi-10-20T08:00:00, kansionimi on määritetty wikidatagateway/wikisampledataout/2014/10/20 ja tiedostonimi on määritetty 08.csv. 

        "folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
        "fileName": "{Hour}.csv",
        "partitionedBy": 
        [
            { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
            { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } }, 
            { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } }, 
            { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } } 
        ],

 
    Katso lisätietoja JSON ominaisuudet [siirtää tietoja, koska Azure-Blob-objektien tallennustilaan](data-factory-azure-blob-connector.md) .
2.  Valitse **Ota käyttöön** ottamaan dataset komentopalkista. Varmista, että näet sekä tietojoukkoja puunäkymässä.  

## <a name="create-pipeline"></a>Myyntijakso luominen
Tässä vaiheessa voit luoda **myyntijakso** yhden **Kopioi tehtävän** , joka käyttää **EmpOnPremSQLTable** syötteenä ja **OutputBlobTable** kuin tulos.

1.  Valitse tietojen Factory editorissa **... Lisää**, ja valitse **Uusi myyntijakso**. 
2.  Korvaa JSON oikeanpuoleiseen ruutuun seuraava teksti: 
    
            {
                "name": "ADFTutorialPipelineOnPrem",
                "properties": {
                "description": "This pipeline has one Copy activity that copies data from an on-prem SQL to Azure blob",
                "activities": [
                {
                    "name": "CopyFromSQLtoBlob",
                    "description": "Copy data from on-prem SQL server to blob",
                    "type": "Copy",
                    "inputs": [
                    {
                        "name": "EmpOnPremSQLTable"
                    }
                    ],
                    "outputs": [
                    {
                        "name": "OutputBlobTable"
                      }
                    ],
                    "typeProperties": {
                      "source": {
                        "type": "SqlSource",
                        "sqlReaderQuery": "select * from emp"
                      },
                      "sink": {
                        "type": "BlobSink"
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
                "start": "2016-07-05T00:00:00Z",
                "end": "2016-07-06T00:00:00Z",
                "isPaused": false
              }
            }

    > [AZURE.IMPORTANT]
    > Korvaa nykyinen **päivä- ja** arvon seuraavana **Käynnistä** -ominaisuuden arvo.

    Huomaa seuraavat seikat:
 
    - Toiminnot-osassa on vain tehtävä, jonka **tyyppi** on määritetty **kopio**.
    - Tehtävän **syöte** on määritetty **EmpOnPremSQLTable** ja tehtävän **tulos** on määritetty **OutputBlobTable**.
    - **TypeProperties** -osassa **SqlSource** on määritetty **lähdetyyppi** ja **BlobSink **on määritetty **käsittelytoiminto tyyppi**.
    - SQL-kyselyn `select * from emp` on määritetty **SqlSource** **sqlReaderQuery** -ominaisuuden.

     Molemmat Käynnistä ja Lopeta päivämääriä ja aikoja on oltava [ISO](http://en.wikipedia.org/wiki/ISO_8601)-muodossa. Esimerkki: 2014-10-14T16:32:41Z. **Päättymisaika** on valinnainen, mutta Käytämme Tässä opetusohjelmassa. 
    
    Jos **end** -ominaisuuden arvoa ei määritetä, sen lasketaan seuraavasti: "**Käynnistä + 48 tuntia**". Suorita putkijohto jatkuvasti, Määritä **9/9/9999** **end** -ominaisuuden arvon. 
    
    Määrität kesto, jossa tiedot sektorit käsitellään, joka oli määritetty kunkin Azure Data Factory-tietojoukko **käytettävyys** ominaisuuksien perusteella.
    
    Esimerkissä on 24 tietojen sektoreiden, kun kukin tietojen sektori on valmistettu kerran tunnissa.     
2. Valitse **Ota käyttöön** komentopalkista ottamaan tietojoukon (taulukko on suorakulmainen tietojoukko). Vahvista puunäkymässä **putkistot** solmun näkyy putkijohto.  
5. Valitse nyt **X** kahdesti, jos haluat sulkea näiden, kun haluat palata **Data Factory** -sivu **ADFTutorialOnPremDF**varten.

**Onnittelen!** Olet luonut Azure tietojen factory, linkitetyn services, tietojoukkoja ja putkijohto ja ajoitettu putkijohto onnistuneesti.

#### <a name="view-the-data-factory-in-a-diagram-view"></a>Verkkokaavio-näkymän tiedot factory tarkasteleminen 
1. **Azure portal**valitsemalla **kaavio** ruutu **ADFTutorialOnPremDF** tietojen factory kotisivulle. :

    ![Kaavio-linkki](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDiagramLink.png)

2. Kaavion samalla tavalla kuin seuraavassa kuvassa pitäisi näkyä seuraavat:

    ![Kaavionäkymä](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDiagramView.png)

    Voit lähentää, loitontaa, zoomaus 100 %, Zoomaus Sovita, sijoita putkistot ja tietojoukkoja automaattisesti ja Näytä periytymistiedot (korostaa ja alaspäin kohteet valittujen kohteiden).  Voit kaksoisnapsauttaa objektin (i/tietojoukko tai myyntijakso) haluamasi ominaisuudet. 

## <a name="monitor-pipeline"></a>Näytön myyntijakso
Tässä vaiheessa käyttö Azure portaalin seurannassa mistä on kyse Azure tietojen factory. Voit myös seurata tietojoukkoja ja putkistot PowerShellin cmdlet-komennot. Saat lisätietoja seuranta [näyttö ja hallita putkistot](data-factory-monitor-manage-pipelines.md).

5. Kaksoisnapsauta **EmpOnPremSQLTable**kaavioon.  

    ![EmpOnPremSQLTable sektorit](./media/data-factory-move-data-between-onprem-and-cloud/OnPremSQLTableSlicesBlade.png)

6. Huomaa, että kaikki tiedot sektoreiden määrittäminen on **Valmis** tilaan koska myyntijakso kesto (päättymisaika alkamisaika) on jo mennyt. Kannattaa myös koska olet lisännyt tiedot SQL Server-tietokantaan ja onko aina. Vahvista alareunassa **ongelma sektorit** -osassa näkyvät ei sektoreiden. Voit tarkastella sektorit valitsemalla **Katso Lisää** sektoreiden luettelon alareunassa. 
7. Valitse seuraavaksi **OutputBlobTable** **tietojoukkoja** -sivu.

    ![OputputBlobTable sektorit](./media/data-factory-move-data-between-onprem-and-cloud/OutputBlobTableSlicesBlade.png)
9. Napsauta mitä tahansa tietojen sektoria luettelosta ja näkyviin tulee **Tietojen muotoilu** -sivu. Näet tehtävän suoritetaan sektoria. Näet suorittaa yleensä vain yksi tehtävä.  

    ![Tietojen muotoilu-sivu](./media/data-factory-move-data-between-onprem-and-cloud/DataSlice.png)

    Jos sektoria ei ole **Valmis** -tilaan, näet seuraavat sektorit, jotka eivät ole valmis ja estävät nykyinen sektori suorittaminen **ylöspäin sektoreiden, joita ei ole valmis** -luettelossa.

10. Valitse **tehtävän suorittaminen** alaosassa näet **tehtävän suorittaminen tiedot**-luettelosta.

    ![Tehtävän suorittaminen tiedot-sivu](./media/data-factory-move-data-between-onprem-and-cloud/ActivityRunDetailsBlade.png)

    Voit nähdä tietoja, kuten siirtonopeuden, kesto ja yhdyskäytävän avulla siirretään tiedot. 
11. Valitse Sulje kaikki näiden vasta, kun **X** 
12. palata koti sivu **ADFTutorialOnPremDF**varten.
14. (valinnainen) Napsauta **putkistot**, valitse **ADFTutorialOnPremDF**ja siirtyä syötteen taulukot (**Kulutettu**) tai tulosteen tietojoukkoja (**valmistetuista**).
15. Käytä työkaluilla, kuten esimerkiksi [Tallennustilan Exploreria](http://storageexplorer.com/) työkalujen avulla voit varmistaa, että Blob-objektien tai tiedosto luodaan tunnin välein.

    ![Azure-tallennustilan Explorer](./media/data-factory-move-data-between-onprem-and-cloud/OnPremAzureStorageExplorer.png)

## <a name="next-steps"></a>Seuraavat vaiheet

- Katso [Data Management Gateway](data-factory-data-management-gateway.md) -artikkelissa Data Management Gateway tietoja.
- Kohdassa Lisätietoja kopioi toimintojen avulla voit siirtää tietoja käsittelytoiminto tietovaraston tietolähteen tietosäilö [Azure SQL Azure-Blob kopioi tiedot](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) . 
