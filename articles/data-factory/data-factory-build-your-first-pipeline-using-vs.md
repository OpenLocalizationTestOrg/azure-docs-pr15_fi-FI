<properties
    pageTitle="Luo ensimmäinen tietojen factory (Visual Studio) | Microsoft Azure"
    description="Tässä opetusohjelmassa luominen Visual Studiossa otosten Azure Data Factory putkijohto."
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
    ms.topic="hero-article" 
    ms.date="10/17/2016"
    ms.author="spelluru"/>

# <a name="tutorial-build-your-azure-first-data-factory-using-microsoft-visual-studio"></a>Opetusohjelma: Muodosta Azure ensimmäisen tietojen factory Microsoft Visual Studiossa
> [AZURE.SELECTOR]
- [Yleiskatsaus ja edellytykset](data-factory-build-your-first-pipeline.md)
- [Azure portal](data-factory-build-your-first-pipeline-using-editor.md)
- [Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
- [PowerShellin](data-factory-build-your-first-pipeline-using-powershell.md)
- [Resurssienhallinta-malli](data-factory-build-your-first-pipeline-using-arm.md)
- [REST-OHJELMOINTIRAJAPINNALLA](data-factory-build-your-first-pipeline-using-rest-api.md)

Tässä artikkelissa käyttää Microsoft Visual Studio ensimmäisen Azure tiedot-factory luomiseen.

## <a name="prerequisites"></a>Edellytykset
1. Lue artikkeli [Opetusohjelma yleiskatsaus](data-factory-build-your-first-pipeline.md) ja **edellytyksenä** vaiheiden avulla.
2. Sinun on oltava **Azure-tilauksen järjestelmänvalvoja** voivat julkaista Data Factory kohteiden Visual Studio Azure Data Factory.
3. Sinulla on oltava asennettuna seuraavat: 
    - Visual Studio 2013: n tai Visual Studio 2015
    - Lataa Azure SDK for Visual Studio 2013: n tai Visual Studio 2015. Siirry [Azure-lataussivulta](https://azure.microsoft.com/downloads/) ja sitten **ja 2013** : n tai **ja 2015** **.NET** -osassa.
    - Lataa uusimmat Azure Data Factory-laajennus Visual Studio: [ja 2013](https://visualstudiogallery.msdn.microsoft.com/754d998c-8f92-4aa7-835b-e89c8c954aa5) : n tai [ja 2015](https://visualstudiogallery.msdn.microsoft.com/371a4cf9-0093-40fa-b7dd-be3c74f49005). Voit myös päivittää laajennus toimimalla seuraavasti: Valitse **Työkalut**-valikosta -> **tunnisteet ja päivitykset** -> **Online** -> **Visual Studio valikoima** -> **Microsoft Azure tietojen Factory Tools for Visual Studio** -> **Päivitä**. 
 
Seuraavaksi Käytämme luominen Azure tietojen factory Visual Studio. 


## <a name="create-visual-studio-project"></a>Visual Studio projektin luominen 
1. Käynnistä **Visual Studio 2013** : n tai **Visual Studio 2015**. Valitse **Tiedosto**, valitse **Uusi**ja sitten **Projekti**. Näkyviin tulee **Uusi projekti** -valintaikkuna.  
2. **Uusi projekti** -valintaikkunassa valitse **DataFactory** -malli ja valitse **Tyhjä Factory projektitiedosto**.   

    ![Uusi projekti-valintaikkuna](./media/data-factory-build-your-first-pipeline-using-vs/new-project-dialog.png)

3. Kirjoita **nimi** projektin, **sijainti**ja **ratkaisu**nimi ja valitse **OK**.

    ![Ratkaisunhallinnassa](./media/data-factory-build-your-first-pipeline-using-vs/solution-explorer.png)

## <a name="create-linked-services"></a>Luo linkitetty palvelut
Tietoja factory voi olla yksi tai useampi putkistot. Putkijohto voi olla vähintään yksi toimintoja ei. Esimerkiksi kopioi toiminta on tietojen kopioiminen lähteen kohde tietosäilö ja HDInsight-rakenne-aktiviteetin suorittamalla rakenteen transform syöttötiedot. Katso [Tuetut tiedot tallennetaan](data-factory-data-movement-activities.md##supported-data-stores-and-formats) lähteiden ja kopioi tehtävän tukemat poistumia. Katso, Laske palveluluetteloon Data Factory tukemat [Laske linkitetyn palvelut](data-factory-compute-linked-services.md) . 

Tässä vaiheessa voit linkittää tiedot factory Azure-tallennustilan tilin ja tarvittaessa-Azure HDInsight-klusterin. Azure-tallennustilan tilin on tässä esimerkissä putkijohto syöttö- ja tiedot. Linkitetty HDInsight-palvelua käytetään suorittamalla rakenteen määritetty tässä esimerkissä putkijohto toiminnan. Määritä tietoja kaupan ja Laske services käytetään käyttämässäsi skenaariossa ja linkittää näistä palveluista tietojen factory luomalla linkitetyn palvelut.  

Määrittämäsi nimi ja tiedot factory asetukset myöhemmin, kun julkaiset Data Factory-ratkaisun.

#### <a name="create-azure-storage-linked-service"></a>Luo linkitetty Azure-tallennustilan
Tässä vaiheessa voit linkittää Azure-tallennustilan tilin tiedot factory. Tässä opetusohjelmassa, käyttää samaa Azure-tallennustilan tilin i/tiedot ja HQL-komentosarjatiedosto tallentamiseen. 

4. **Linkitetyn Services** solution Explorerissa napsauttamalla hiiren kakkospainikkeella, valitse **Lisää**ja valitse **Uusi kohde**.      
5. **Lisää uusi kohde** -valintaikkunassa **Azure Tallennuspalvelu linkitetyn** luettelosta ja valitse **Lisää**. 
3. Korvaa **accountname** ja **accountkey** Azure-tallennustilan tilin ja sen avaimen nimiä. Lisätietoja tallennustilan pikanäppäin hankkiminen on artikkelissa [näkymän, kopioi ja muodosta uudelleen sarjanumerot tallennustilan pikanäppäimet](../storage/storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys)

    ![Azure-tallennustilan linkitetty palvelu](./media/data-factory-build-your-first-pipeline-using-vs/azure-storage-linked-service.png)

4. Tallenna tiedosto **AzureStorageLinkedService1.json** .

#### <a name="create-azure-hdinsight-linked-service"></a>Luo linkitetty Azure HDInsight-palvelu
Tässä vaiheessa voit linkittää tarvittaessa-HDInsight-klusterin tietojen factory. HDInsight-klusterin automaattisesti suorituksen luodaan ja poistetaan, kun se on valmis käsittely ja käyttämättömänä tietyn ajanjakson aikana. Voit käyttää omaa HDInsight-klusterin tarvittaessa-HDInsight-klusterin sijaan. [Laske linkitetyn Services](data-factory-compute-linked-services.md) lisätietoja. 

1. **Ratkaisunhallinnassa** **Linkitetyn**palvelut, Vie osoitin **Lisää**ja valitse **Uusi kohde**.
2. Valitse **Kansion HDInsight-Demand linkitetyn palvelu**ja sitten **Lisää**. 
3. Korvaa **JSON** seuraavasti:

        {
          "name": "HDInsightOnDemandLinkedService",
          "properties": {
            "type": "HDInsightOnDemand",
            "typeProperties": {
              "version": "3.2",
              "clusterSize": 1,
              "timeToLive": "00:30:00",
              "linkedServiceName": "AzureStorageLinkedService1"
            }
          }
        }
    
    Seuraavassa taulukossa on käytetty koodikatkelman JSON-ominaisuuksien kuvauksia:
    
    Ominaisuus | Kuvaus
    -------- | -----------
    Versio | Määrittää, että HDInsight-version luonut on 3,2. 
    ClusterSize | Määrittää HDInsight-klusterin koon. 
    TimeToLive | Määrittää, että HDInsight-klusterin joutoaika ennen kuin se poistetaan.
    linkedServiceName | Määrittää tallennustilan-tili, jota käytetään tallentamiseen HDInsight avulla luotujen lokit

    Ota seuraavat seikat huomioon: 
    
    - Data Factory Luo **Windows-pohjaisesta** HDInsight-klusterin edellisen JSON kanssa. Voit myös voi olla se **Linux-pohjaiset** HDInsight-klusterin luominen. Katso [Tarvittaessa HDInsight linkitetyn palvelun](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) tiedot. 
    - Voit käyttää **omaa HDInsight-klusterin** tarvittaessa-HDInsight-klusterin sijaan. Katso [HDInsight linkitetyn palvelun](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) tiedot.
    - HDInsight-klusterin Luo **oletusarvon säilö** blob-säiliö määrittämäsi JSON (**linkedServiceName**). HDInsight Poista tämä säilö klusterin poistamisen yhteydessä. Tämä on suunniteltu ominaisuus. Tarvittaessa HDInsight linkitetty palvelussa HDInsight-klusterin luodaan aina, kun sektoria käsitellään, ellei aiemmin live klusterin (**timeToLive**). Klusterin poistetaan automaattisesti, kun käsittely on valmis.
    
        Lisää sektoreiden käsitellään, näet monta säilöjen Azuren Blob-objektien tallennustilaan. Jos ei tarvitse niitä töiden vianmääritystä varten, haluat ehkä poistaa ne tallennustilan pienentää. Nämä säilöjä nimet noudattavat kuviolla: "syöttö**yourdatafactoryname**-**linkedservicename**- datetimestamp". Poistaa Azure-Blob-objektien tallennustilaan säilöjen Työkalut [Exploreria tallennustilan](http://storageexplorer.com/) kuten avulla.

    Katso [Tarvittaessa HDInsight linkitetyn palvelun](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) tiedot. 
4. Tallenna tiedosto **HDInsightOnDemandLinkedService1.json** .

## <a name="create-datasets"></a>Luo tietojoukkoja
Tässä vaiheessa voit luoda tietojoukkoja esittämään syötteen ja tulostaa tiedot rakenteen käsittelyä varten. Nämä tietojoukkoja viitata **AzureStorageLinkedService1** olet luonut aiemmin tässä opetusohjelmassa. Azure-tallennustilan tilin ja tietojoukkoja linkitetyistä kohdeosoite Määritä säilö, kansion, tiedostonimi muistiin, jossa on syötteen ja tulostaa tiedot.   

#### <a name="create-input-dataset"></a>Kirjoita tietojoukon luominen

1. **Ratkaisunhallinnassa** **taulukoiden**hiiren kakkospainikkeella, Vie osoitin **Lisää**ja valitse **Uusi kohde**. 
2. Valitse **Azure-Blob** -luettelosta, muuta tiedoston nimeä **InputDataSet.json**ja sitten **Lisää**.
3. Korvaa **JSON** -editorissa seuraavasti: 

    JSON-koodikatkelman luot kutsutaan **AzureBlobInput** , joka edustaa putkijohto toimintaa syöttötiedot tietojoukko. Lisäksi voit määrittää, että syöttötiedot sijaitsee kutsutaan **adfgetstarted** blob-säilö ja **inputdata** -nimiseen kansioon
        
        {
            "name": "AzureBlobInput",
            "properties": {
                "type": "AzureBlob",
                "linkedServiceName": "AzureStorageLinkedService1",
                "typeProperties": {
                    "fileName": "input.log",
                    "folderPath": "adfgetstarted/inputdata",
                    "format": {
                        "type": "TextFormat",
                        "columnDelimiter": ","
                    }
                },
                "availability": {
                    "frequency": "Month",
                    "interval": 1
                },
                "external": true,
                "policy": {}
            }
        } 

    Seuraavassa taulukossa on käytetty koodikatkelman JSON-ominaisuuksien kuvauksia:

  	| Ominaisuus | Kuvaus |
  	| :------- | :---------- |
  	| tyyppi | Tyyppi-ominaisuudeksi on määritetty AzureBlob, koska tiedot sijaitsevat Azure Blob-objektien tallennustilaan. |  
  	| linkedServiceName | viittaa aiemmin luomasi AzureStorageLinkedService1. |
  	| Tiedostonimi | Tämä ominaisuus on valinnainen. Jos tätä ominaisuutta, kaikki tiedostot kansiopolku kerätään. Tässä tapauksessa vain input.log käsitellään. |
  	| tyyppi | Lokitiedostojen ovat teksti-muodossa, joten Käytämme TekstinMuoto. | 
  	| columnDelimiter | lokitiedostojen sarakkeissa on erotettu toisistaan pilkku () |
  	| Toistumisvälin / | Kuukausi-ja aikavälin korkojakso on 1, mikä tarkoittaa, että syötteen sektorit ovat käytettävissä kuukausittain. | 
  	| Ulkoinen | Tämä ominaisuus on määritetty tosi, jos syöttötiedot ei luoda Data Factory-palvelu. | 
      
    
3. Tallenna tiedosto **InputDataset.json** . 

 
#### <a name="create-output-dataset"></a>Tulosteen tietojoukon luominen
Nyt voit luoda tulostus-tietojoukko Azure-Blob-objektien tallennustilaan tallennettuja tulosteen tietojen esittämiseen. 

1. **Ratkaisunhallinnassa** **taulukoiden**hiiren kakkospainikkeella, Vie osoitin **Lisää**ja valitse **Uusi kohde**. 
2. Valitse **Azure-Blob** -luettelosta, muuta tiedoston nimeä **OutputDataset.json**ja sitten **Lisää**. 
3. Korvaa **JSON** -editorissa seuraavasti: 

    JSON-koodikatkelman luot kutsutaan **AzureBlobOutput**ja määrittämällä tiedot, jotka rakenteen komentosarjan tuottamat rakenteen tietojoukko. Lisäksi voit määrittää, että tulokset tallennetaan blob-säilö kutsutaan **adfgetstarted** ja **partitioneddata**-nimiseen kansioon. **Käytettävyys** -osassa määrittää, että tulostus-tietojoukko on valmistettu kuukausittain.
    
        {
          "name": "AzureBlobOutput",
          "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService1",
            "typeProperties": {
              "folderPath": "adfgetstarted/partitioneddata",
              "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
              }
            },
            "availability": {
              "frequency": "Month",
              "interval": 1
            }
          }
        }

    Katso **syötteen tietojoukon luominen** -osa näiden ominaisuuksien kuvaukset. Älä määritä ulkoisen ominaisuuden tulosteen dataset-kuin tietojoukon tuottaa Data Factory-palvelun.

4. Tallenna tiedosto **OutputDataset.json** .


### <a name="create-pipeline"></a>Myyntijakso luominen
Tässä vaiheessa voit luoda ensimmäisen myyntijakso **HDInsightHive** toimintojen kanssa. Syötteen sektori on käytettävissä kuukausittain (taajuus: kuukausi, aikaväli: 1), tulos sektori on valmistettu kuukausittain ja tehtävän ajoitus-ominaisuudeksi on määritetty myös kuukausittain. Tulostus-tietojoukko ja tehtävän ajoitus asetukset on vastattava. Tulosteen tietojoukko ei tällä hetkellä, mitä ohjaa aikataulussa, joten sinun on luotava tulostus-tietojoukko, vaikka tehtävän tuota tuloksen. Jos tehtävän sähköpostit tulevat kaikki syötetyt, voit ohittaa syötteen tietojoukon luominen. Seuraavat JSON ominaisuuksia on esitelty tämän osan loppuun.

1. Napsauta **Ratkaisunhallinnassa**hiiren kakkospainikkeella **putkistot**, valitse **Lisää**ja sitten **Uusi kohde.** 
2. Valitse **Muunnos myyntijakso rakenne** -luettelosta ja valitse **Lisää**. 
3. Korvaa **JSON** seuraavat koodikatkelman.

    > [AZURE.IMPORTANT] Korvaa **storageaccountname** tallennustilan tilin nimi.

        {
            "name": "MyFirstPipeline",
            "properties": {
                "description": "My first Azure Data Factory pipeline",
                "activities": [
                    {
                        "type": "HDInsightHive",
                        "typeProperties": {
                            "scriptPath": "adfgetstarted/script/partitionweblogs.hql",
                            "scriptLinkedService": "AzureStorageLinkedService1",
                            "defines": {
                                "inputtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/inputdata",
                                "partitionedtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/partitioneddata"
                            }
                        },
                        "inputs": [
                            {
                                "name": "AzureBlobInput"
                            }
                        ],
                        "outputs": [
                            {
                                "name": "AzureBlobOutput"
                            }
                        ],
                        "policy": {
                            "concurrency": 1,
                            "retry": 3
                        },
                        "scheduler": {
                            "frequency": "Month",
                            "interval": 1
                        },
                        "name": "RunSampleHiveActivity",
                        "linkedServiceName": "HDInsightOnDemandLinkedService"
                    }
                ],
                "start": "2016-04-01T00:00:00Z",
                "end": "2016-04-02T00:00:00Z",
                "isPaused": false
            }
        }

    JSON-koodikatkelman luot putkijohto, joka koostuu yhden aktiviteetin, joka käyttää HDInsight-klusterin kirjautumisprosessin tietojen rakennetta.
    
    JSON-koodikatkelman luot putkijohto, joka koostuu yhden aktiviteetin, joka käyttää HDInsight-klusterin kirjautumisprosessin tietojen rakennetta.
    
    Rakenne-komentosarjatiedosto **partitionweblogs.hql**tallennetaan Azure-tallennustilan tilin (määritetty scriptLinkedService kutsutaan **AzureStorageLinkedService1**) ja säilö **adfgetstarted** **komentosarja** -kansiossa.

    **Määrittää** -osa käytetään välitetään rakenne-komentosarjan rakenteen määritys arvoina runtime-asetusten määrittäminen (esimerkiksi ${hiveconf: inputtable}, {hiveconf:partitionedtable} $).

    **Käynnistä** ja **Lopeta** ominaisuuksien putkijohto määrittää putkijohto aktiivisen kauden.

    Määritä tehtävän JSON, että rakenne-komentosarja suoritetaan **linkedServiceName** – **HDInsightOnDemandLinkedService**määrittämää suorittaminen.

    > [AZURE.NOTE] Katso lisätietoja JSON-ominaisuuksien käyttäminen esimerkissä [putkijohto-julkaisusivuston rakenne](data-factory-create-pipelines.md#anatomy-of-a-pipeline) . 
3. Tallenna tiedosto **HiveActivity1.json** .

### <a name="add-partitionweblogshql-and-inputlog-as-a-dependency"></a>Lisää partitionweblogs.hql ja input.log riippuvuus 

1. **Riippuvuudet** **Ratkaisunhallinnassa** -ikkunassa hiiren kakkospainikkeella, valitse **Lisää**ja valitse **Olemassa olevan kohteen**.  
2. Siirry **C:\ADFGettingStarted** ja **partitionweblogs.hql**, **input.log** tiedostoja, valitse ja sitten **Lisää**. Nämä tiedostot edellytykset osana oli on luotu [Opetusohjelma yleiskatsaus](data-factory-build-your-first-pipeline.md).

Kun julkaiset ratkaisun seuraavassa vaiheessa, **partitionweblogs.hql** -tiedosto on ladattu **adfgetstarted** blob-säilö komentosarjat-kansioon.   

### <a name="publishdeploy-data-factory-entities"></a>Julkaise/käyttöön Data Factory yksiköt

18. Projektin Solution Explorerissa hiiren kakkospainikkeella ja valitse sitten **Julkaise**. 
19. Jos näet **Microsoft-tilille kirjautuminen** -valintaikkuna, anna tunnistetiedot tilille, joka on Azure-tilaus ja valitse **Kirjaudu sisään**.
20. Näkyviin tulee seuraava valintaikkuna:

    ![Julkaise-valintaikkuna](./media/data-factory-build-your-first-pipeline-using-vs/publish.png)

21. Valitse Määritä factory-sivulla seuraavasti: 
    1. Valitse **Luo uusi Data Factory** -vaihtoehto.
    2. Kirjoita tiedot factory yksilöivä **nimi** . Esimerkki: **FirstDataFactoryUsingVS09152016**. Nimen on oltava yksilöivä.  
    
    
        > [AZURE.IMPORTANT] Jos näyttöön tulee virhesanoma, **tietojen factory nimi "FirstDataFactoryUsingVS" ei ole käytettävissä** , kun julkaiset, muuta nimi (esimerkiksi yournameFirstDataFactoryUsingVS). Data Factory palvelutiedot nimeämiskäytäntö säännöt [Tietojen Factory - nimeäminen säännöt](data-factory-naming-rules.md) aiheessa.
3. Valitse **tilaus** -kenttään oikean tilaus.
     
     
        > [AZURE.IMPORTANT] Jos jokin tilaus ei ole näkyvissä, varmista, että olet kirjautunut sisään käyttämällä tiliä, jolla on järjestelmänvalvojan tai Mää-järjestelmänvalvojan tilauksen.  
        
    4. Valitse tietojen factory luoda **resurssiryhmä** . 
    5. Valitse tiedot-factory **alue** . 
    6. Valitse **Seuraava** siirtyäksesi **Julkaista kohteita** -sivulle. (Painamalla **VÄLILEHTI** siirtää pois nimi-kenttä, jos **Seuraava** -painike ei ole käytettävissä.) 
23. **Julkaista kohteita** sivulla että kaikki tiedot-tehtaan kohteiden valitaan ja valitse **Seuraava** siirtyäksesi **Yhteenveto** -sivulle.     
24. Tarkista yhteenveto ja valitse **Seuraava** **Käyttöönoton tilan**ja käynnistää käyttöönottoprosessin.
25. **Käyttöönottotila** -sivulla näkyy käyttöönottoprosessin tila. Kun asennus on valmis, valitse Valmis. 

 
Huomaa tärkeät seikat: 

- Jos saat virheilmoituksen: "**tätä tilausta ei ole rekisteröity käyttämään nimitilan Microsoft.DataFactory**", tee jokin seuraavista toimista ja yritä julkaista uudelleen: 

    - PowerShellin Azure suorittamalla seuraavan komennon rekisteröidä Data Factory-palvelu. 
        
            Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
    
        Voit suorittaa tietojen tehdas palvelu rekisteröidyt Vahvista seuraava komento. 
    
            Get-AzureRmResourceProvider
    - Kirjaudu sisään käyttämällä Azure tilaus- [Azure portaaliin](https://portal.azure.com) ja siirry Data Factory-sivu (tai) luominen tietojen factory Azure-portaalissa. Tämä toiminto rekisteröi palveluntarjoajan puolestasi.
-   Tietoja factory nimi voidaan rekisteröidä myöhemmin ja näin ollen tulevat näkyviin Luettele DNS-nimi.
-   Jos haluat luoda Data Factory esiintymät, sinun on oltava järjestelmänvalvojan tai Mää-järjestelmänvalvojan Azure-tilaukseen

 
## <a name="monitor-pipeline"></a>Näytön myyntijakso

### <a name="monitor-pipeline-using-diagram-view"></a>Näytön myyntijakso käyttämällä
6. Kirjaudu sisään [Azure portal](https://portal.azure.com/), toimi seuraavasti:
    1. Valitse **Lisää palveluja** ja valitse **tietoja tehtaan**.
        ![Selaa tietojen tehtaan](./media/data-factory-build-your-first-pipeline-using-vs/browse-datafactories.png) 
    2. Valitse tiedot-factory nimi (esimerkiksi: **FirstDataFactoryUsingVS09152016**) tietojen tehtaan luettelosta. 
        ![Valitse tiedot-factory](./media/data-factory-build-your-first-pipeline-using-vs/select-first-data-factory.png)
7. Valitse tietojen factory kotisivu- **kaavio**.
  
    ![Kaavio-ruutu](./media/data-factory-build-your-first-pipeline-using-vs/diagram-tile.png)
7. Kaavionäkymässä näet, putkistot ja käyttää tässä opetusohjelmassa tietojoukkoja.
    
    ![Kaavionäkymä](./media/data-factory-build-your-first-pipeline-using-vs/diagram-view-2.png) 
8. Jos haluat tarkastella kaikkia toimintoja putkijohto, napsauta kaavion putkijohto ja valitse Avaa myyntijakso. 

    ![Avaa myyntijakso-valikko](./media/data-factory-build-your-first-pipeline-using-vs/open-pipeline-menu.png)
9. Varmista, että näet putkijohto HDInsightHive-toimintojen. 
  
    ![Avaa myyntijakso-näkymä](./media/data-factory-build-your-first-pipeline-using-vs/open-pipeline-view.png)

    Voit palata edelliseen näkymään, valitse **Data factory** yläreunassa linkkipolun-valikossa. 
10. Kaksoisnapsauta **Kaavionäkymä**tietojoukko **AzureBlobInput**. Varmista, että sektoria on **Valmis** -tilaan. Voi kestää muutaman minuutin sektorin lisääminen valmiina. Jos se ei ole aktivoitu, kun odotat viime, tarkista, ovatko oikean säilö (adfgetstarted) ja (inputdata)-kansio sijoitetaan syötteen tiedosto (input.log).

    ![Syötteen sektoria Valmis-tilaan](./media/data-factory-build-your-first-pipeline-using-vs/input-slice-ready.png)
11. Valitse Sulje **AzureBlobInput** sivu **X** . 
12. Kaksoisnapsauta **Kaavionäkymä**tietojoukko **AzureBlobOutput**. Näet, joka käsitellään parhaillaan sektoria.

    ![Tietojoukko](./media/data-factory-build-your-first-pipeline-using-vs/dataset-blade.png)
9. Kun käsittely on valmis, näyttöön tulee sektoria **Valmis** -tilaan.

    > [AZURE.IMPORTANT] Tarvittaessa-HDInsight-klusterin luominen yleensä kestää viime (noin 20 minuuttia). Tämän vuoksi toiminta voi kestää **noin 30 minuuttia** käsittelemään sektoria putkijohto.  

    ![Tietojoukko](./media/data-factory-build-your-first-pipeline-using-vs/dataset-slice-ready.png) 
    
10. Kun sektoria on **valmiina** , tarkista **adfgetstarted** säilössä Blob-objektien tallennustilaan tulosteen tietojen **partitioneddata** -kansio.  
 
    ![siirtää tietoja](./media/data-factory-build-your-first-pipeline-using-vs/three-ouptut-files.png)
11. Valitse sektoria, jos haluat nähdä lisätietoja sen- **tietojen muotoilu** -sivu.

    ![Tietojen muotoilu](./media/data-factory-build-your-first-pipeline-using-vs/data-slice-details.png)  
12. Valitse aktiviteettia Suorita **tehtävä lohkot luettelosta** lisätietoja aktiviteettia suorittaa **tehtävän suorittaminen tiedot** -ikkunassa (Tässä skenaariossa aktiviteetin rakenne).   
    ![Tehtävän suorittaminen tiedot](./media/data-factory-build-your-first-pipeline-using-vs/activity-window-blade.png)  
    
    Lokitiedostoista näet rakenne-kysely, joka on suoritettu ja tilatiedot. Nämä lokit ovat mahdollisten ongelmien vianmäärityksessä.  
 

Saat ohjeet Azure portaalin avulla voit valvoa putkijohto ja tietojoukkoja olet luonut Tässä opetusohjelmassa [näytön tietojoukkoja ja myyntijakso](data-factory-monitor-manage-pipelines.md) .

### <a name="monitor-pipeline-using-monitor--manage-app"></a>Näytön myyntijakso näyttö ja hallinta-sovelluksen käyttäminen
Voit myös käyttää näytön & seurannassa oman putkistot-sovelluksen hallinta. Lisätietoja tämän sovelluksen käyttämisestä on artikkelissa [näyttö ja hallita Azure Data Factory putkistot seuranta ja hallinta-sovelluksen käyttäminen](data-factory-monitor-manage-app.md).

1. Valitse näytön ja hallitse ruutu.

    ![Seurata ja hallita ruutu](./media/data-factory-build-your-first-pipeline-using-vs/monitor-and-manage-tile.png) 
2. Pitäisi nähdä näyttö ja sovellusten hallinta. **Alkamisaika** ja **Päättymisaika** vastaamaan Käynnistä (04 01 – 2016 klo 24.00)- ja päättymisajat (04-02-2016 klo 24.00), että putkijohto ja sitten **Käytä**.

    ![Seuranta ja hallinta-sovelluksella](./media/data-factory-build-your-first-pipeline-using-vs/monitor-and-manage-app.png) 
3. Valitse tehtävä-ikkunassa toiminta Windows-luettelon ja tarkastella sen tietoja. 
    ![Tehtävän ikkunan tiedot](./media/data-factory-build-your-first-pipeline-using-vs/activity-window-details.png)


> [AZURE.IMPORTANT] Lähdetiedoston saa poistetaan, kun sektoria käsittely on onnistunut. Vuoksi halutessasi uudelleen sektoria tai tehdä opetusohjelman uudelleen input-tiedoston lataaminen (input.log) adfgetstarted säilön inputdata-kansio.
 

## <a name="use-server-explorer-to-view-data-factories"></a>Palvelimen Resurssienhallinnan avulla voit tarkastella tietoja tehtaan

1. **Visual Studiossa**Valitse **Näytä** -valikon ja **Palvelimen**hallinta.
2. Laajenna **Azure** ja laajenna **Data Factory**Server-ikkunan. Jos näet **Visual Studio Kirjaudu**, anna **tilin** Azure-tilaukseen liittyvää ja valitse **Jatka**. Kirjoita **salasana**ja valitse **Kirjaudu sisään**. Visual Studio yrittää kaikki Azure tietojen tehtaan tilaukseesi tietoja. Näet tilan toiminto **Tietojen Factory tehtäväluettelo** -ikkunassa.

    ![Palvelimen Explorer](./media/data-factory-build-your-first-pipeline-using-vs/server-explorer.png)
3. Voit tietojen factory hiiren kakkospainikkeella ja valitse **Vie tiedot Factory uuden projektin** luominen Visual Studio projektin olemassa olevat tiedot-factory perusteella.

    ![Vie tiedot factory](./media/data-factory-build-your-first-pipeline-using-vs/export-data-factory-menu.png)

## <a name="update-data-factory-tools-for-visual-studio"></a>Päivitä tiedot Factory Työkalut Visual Studio

Päivitä Azure Data Factory Työkalut Visual Studio, toimi seuraavasti:

1. Valitse **Työkalut** -valikon ja valitse **tunnisteet ja päivitykset**.
2. Valitse vasemmanpuoleisessa ruudussa **päivitykset** ja valitse sitten **Visual Studio valikoima**.
3. Valitse **Azure Data Factory tools for Visual Studio** ja valitse **Päivitä**. Jos tapahtuma ei ole näkyvissä, sinulla on jo työkalujen uusimman version. 

## <a name="use-configuration-files"></a>Tiedostojen käyttäminen
Voit käyttää tiedostojen Visual Studiossa linkitetyn services/taulukot/putkistot eri tavalla kutakin ympäristöä varten ominaisuuksien määrittäminen. 

Ota huomioon seuraavat JSON määrittelyn Azuren tallennustilaan linkitetty-palvelun. Jos haluat määrittää **connectionString** accountname ja accountkey ympäristöön (keskihajonta-testi-tuotannon), johon Data Factory kohteiden otat eri arvoilla. Näin voit tehostaa käyttämällä eri kokoonpanotiedosto kutakin ympäristöä varten. 

    {
        "name": "StorageLinkedService",
        "properties": {
            "type": "AzureStorage",
            "description": "",
            "typeProperties": {
                "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
            }
        }
    } 

### <a name="add-a-configuration-file"></a>Lisää määritystiedoston
Lisää kokoonpanotiedosto kutakin ympäristöä varten seuraavasti:   

1. Visual Studio-ratkaisun tietojen Factory projektin hiiren kakkospainikkeella, valitse **Lisää**ja valitse **Uusi kohde**.
2. **Config** asennettujen mallien vasemman reunan luettelosta, valitse **Kokoonpanotiedosto**, kirjoita tiedoston **nimi** ja valitse **Lisää**.

    ![Lisää kokoonpanotiedosto](./media/data-factory-build-your-first-pipeline-using-vs/add-config-file.png)
3. Lisää parametreja ja niiden arvot muodossa.

        {
            "$schema": "http://datafactories.schema.management.azure.com/vsschemas/V1/Microsoft.DataFactory.Config.json",
            "AzureStorageLinkedService1": [
                {
                    "name": "$.properties.typeProperties.connectionString",
                    "value": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
                }
            ],
            "AzureSqlLinkedService1": [
                {
                    "name": "$.properties.typeProperties.connectionString",
                    "value":  "Server=tcp:spsqlserver.database.windows.net,1433;Database=spsqldb;User ID=spelluru;Password=Sowmya123;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
                }
            ]
        }

    Tässä esimerkissä määrittää connectionString-ominaisuuden Azuren tallennustilaan linkitetty-palveluun ja linkitetty Azure SQL-palvelun. Huomaa, että nimi, joka määrittää syntaksi on [JsonPath](http://goessner.net/articles/JsonPath/).   

    Jos JSON on ominaisuus, joka sisältää matriisin arvoista, kuten seuraava koodi:  

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
    
    Ominaisuuksien määrittäminen (Käytä nollasta indeksoinnin) seuraavat kokoonpanotiedosto esitetyllä tavalla: 
        
        {
            "name": "$.properties.structure[0].name",
            "value": "FirstName"
        }
        {
            "name": "$.properties.structure[0].type",
            "value": "String"
        }
        {
            "name": "$.properties.structure[1].name",
            "value": "LastName"
        }
        {
            "name": "$.properties.structure[1].type",
            "value": "String"
        }

### <a name="property-names-with-spaces"></a>Ominaisuuksien nimiä välilyöntejä
Jos ominaisuuden nimi on välilyöntejä, käytä hakasulkeita, kuten seuraavassa esimerkissä (tietokantapalvelimen nimi): 

     {
         "name": "$.properties.activities[1].typeProperties.webServiceParameters.['Database server name']",
         "value": "MyAsqlServer.database.windows.net"
     }


### <a name="deploy-solution-using-a-configuration"></a>Ratkaisun avulla määrityksen käyttöönottoa
Kun tallennat ja Azure Data Factory kohteita, voit määrittää, jota haluat käyttää julkaisun toiminnon määritys. 

Voit julkaista kohteita käyttämällä kokoonpanotiedosto Azure Data Factory projektissa seuraavasti:   

1. Napsauta Data Factory-projekti ja valitse **Julkaise** , jos haluat **Julkaista kohteita** -valintaikkunaa. 
2. Valitse aiemmin luotu tietojen-factory tai määritä arvot luominen tietojen factory **määrittäminen tietojen factory** -sivulla ja valitse **Seuraava**.   
3. **Julkaise kohteet** -sivulla: näet avattavan luettelon käytettävissä määritykset, **Valitse käyttöönoton Config** -kentän kanssa.

    ![Valitse määritystiedostossa](./media/data-factory-build-your-first-pipeline-using-vs/select-config-file.png)

4. Valitse **määritystiedostoa** , jotka haluat käyttää ja valitse **Seuraava**. 
5. Varmista, että **Yhteenveto** -sivulla JSON-tiedoston nimi ja valitse **Seuraava**. 
6. Kun käyttöönotto-toiminto on valmis, valitse **Valmis** . 

Kun otat käyttöön, voit määrittää ominaisuuksien arvot Data Factory kohteiden JSON-tiedostot, ennen kuin kohteiden otetaan käyttöön Azure Data Factory-palveluun käytetään määritystiedosto arvot.   

## <a name="summary"></a>Yhteenveto 
Tässä opetusohjelmassa luomasi suorittamalla rakenteen komentosarja HDInsight hadoop klusterissa Azure tiedot-factory prosessin tietoihin. Tietoja Factory-editorin käytössä Azure-portaalissa voit tehdä seuraavat toimet:  

1.  Luoda Azure **tietojen factory**.
2.  Luo kaksi **linkitetyn palvelut**:
    1.  **Azuren tallennustilaan** linkitetty linkki Azure-blob-tallennustilan, joka sisältää/i-tiedostojen tietoja factory-palveluun.
    2.  **Azure Hdinsightiin** tarvittaessa linkitetyn palvelu Linkitä tiedot factory tarvittaessa-HDInsight Hadoop-klusterin. Azure Data Factory luodaan HDInsight Hadoop klusterin juuri---aika Käsittele syöttötiedot ja tuottaa siirtää tietoja. 
3.  Luo kaksi **tietojoukkoja**, jotka kuvaavat syöttö- ja HDInsight rakenteen aktiviteetin putkisto-tiedot. 
4.  Luotu **myyntijakso** **HDInsight-rakenne** -tehtävän.  


## <a name="next-steps"></a>Seuraavat vaiheet
Tässä artikkelissa luomasi putkijohto tarvittaessa-HDInsight-klusterin rakenteen komentosarjaa suorittava muunnos-aktiviteettiin (HDInsight tehtävä). Katso kopio-toimintojen käyttäminen tietojen kopioimiseen Azure SQL Azure-Blob- [Opetusohjelma: kopioi tiedot azuren blob-Azure SQL](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).
  
## <a name="see-also"></a>Katso myös
| Aihe | Kuvaus |
| :---- | :---- |
| [Tietojen muunnos toiminnot](data-factory-data-transformation-activities.md) | Tässä artikkelissa on tietoja muunnos toimintoja (kuten HDInsight-rakenne-muunnos käytit Tässä opetusohjelmassa) luettelo tukemat Azure Data Factory. | 
| [Ajoittamisen ja suorittaminen](data-factory-scheduling-and-execution.md) | Tässä artikkelissa kerrotaan Azure Data Factory-sovelluksen malli ajoittaminen ja suorittamisen ominaisuuksia. |
| [Putkistot](data-factory-create-pipelines.md) | Tässä artikkelissa auttaa sinua ymmärtämään putkistot ja Azure Data Factory ja kuinka niitä käytetään muodostaa skenaarion tai business loppuun tietoihin perustuvien työnkulkuja. |
| [Tietojoukkoja](data-factory-create-datasets.md) | Tässä artikkelissa auttaa sinua ymmärtämään Azure Data Factory tietojoukkoja.
| [Seurata ja hallita putkistot seuranta-sovelluksella](data-factory-monitor-manage-app.md) | Tässä artikkelissa kerrotaan, miten voit seurata ja hallita virheenkorjaus putkistot seuranta ja hallinta-sovelluksen käyttäminen. 
