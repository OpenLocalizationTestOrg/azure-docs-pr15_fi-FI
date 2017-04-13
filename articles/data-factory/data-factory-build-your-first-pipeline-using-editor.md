<properties
    pageTitle="Luo ensimmäinen tietojen factory (Azure portaalin) | Microsoft Azure"
    description="Tässä opetusohjelmassa luot otoksen Azure Data Factory putkijohto tiedot-Factory editorilla Azure-portaalissa."
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
    ms.date="09/14/2016"
    ms.author="spelluru"/>

# <a name="tutorial-build-your-first-azure-data-factory-using-azure-portal"></a>Opetusohjelma: Luo ensimmäinen factory Azure tietojen Azure-portaalissa
> [AZURE.SELECTOR]
- [Yleiskatsaus ja edellytykset](data-factory-build-your-first-pipeline.md)
- [Azure portal](data-factory-build-your-first-pipeline-using-editor.md)
- [Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
- [PowerShellin](data-factory-build-your-first-pipeline-using-powershell.md)
- [Resurssienhallinta-malli](data-factory-build-your-first-pipeline-using-arm.md)
- [REST-OHJELMOINTIRAJAPINNALLA](data-factory-build-your-first-pipeline-using-rest-api.md)

Tässä artikkelissa kerrotaan, miten [Azure portal](https://portal.azure.com/) avulla voit luoda ensimmäisen Azure tiedot-factory. 

## <a name="prerequisites"></a>Edellytykset        
1. Lue artikkeli [Opetusohjelma yleiskatsaus](data-factory-build-your-first-pipeline.md) ja **edellytyksenä** vaiheiden avulla.
2. Tässä artikkelissa ei tarjoa Azure Data Factory-palvelun Käsitteellinen yleiskatsaus. On suositeltavaa, että Siirry [Azure Data Factory johdanto](data-factory-introduction.md) artikkelin yksityiskohtainen yhteenveto-palvelun kautta.  

## <a name="create-data-factory"></a>Tietoja factory luominen
Tietoja factory voi olla yksi tai useampi putkistot. Putkijohto voi olla vähintään yksi toimintoja ei. Kopioi tehtävän, voit kopioida tiedot kohteen tietovaraston tietolähteen ja HDInsight-rakenne-aktiviteetin suorittamalla rakenteen transform syöttötiedot tuotteeseen tulosteen tiedot. Aloitetaan luominen tietojen factory tässä vaiheessa. 

1.  Kirjaudu sisään [Azure portal](https://portal.azure.com/).
2.  Valitse vasemmalla olevasta valikosta **Uusi** , valitse **tietoja + Analytics**ja sitten **Data Factory**.
        
    ![Luo sivu](./media/data-factory-build-your-first-pipeline-using-editor/create-blade.png)

2.  Kirjoita nimi **GetStartedDF** **uudet tiedot factory** -sivu.

    ![Uusien tietojen factory-sivu](./media/data-factory-build-your-first-pipeline-using-editor/new-data-factory-blade.png)

    > [AZURE.IMPORTANT] 
    > Azure tietojen factory nimen on oltava **yksilöivä**. Jos saat virheilmoituksen: **tietojen factory nimi "GetStartedDF" ei ole käytettävissä**. Tietoja factory (esimerkiksi yournameGetStartedDF) nimen muuttaminen ja yritä uudelleen. Data Factory palvelutiedot nimeämiskäytäntö säännöt [Tietojen Factory - nimeäminen säännöt](data-factory-naming-rules.md) aiheessa.
    > 
    > Tietoja factory nimi voidaan rekisteröidä myöhemmin ja näin ollen tulee Luettele näkyvät **DNS** -nimi.

3.  Valitse kohtaa, johon haluat luoda tietojen tehdas **Azure tilauksen** . 
4.  Valitse aiemmin luotu **resurssiryhmä** tai luo resurssiryhmä. Luo opetusohjelman nimeltä resurssiryhmä: **ADFGetStartedRG**. 
5.  Valitse **Luo** **Uusi tietojen factory** -sivu.

    > [AZURE.IMPORTANT] Jos haluat luoda Data Factory esiintymät, on oltava tilauksen/resurssin ryhmittelytasolla [Tietojen Factory osallistuja](../active-directory/role-based-access-built-in-roles.md/#data-factory-contributor) -roolin jäsen. 
6.  Saat tietoja tehdas on luotu **Startboard** Azure-portaalin seuraavasti:   

    ![Tietojen factory tila luominen](./media/data-factory-build-your-first-pipeline-using-editor/creating-data-factory-image.png)
7. Onnittelen! Olet luonut ensimmäisen tiedot-factory. Kun tietojen tehdas on luotu, näyttöön tulee tietojen Tehdas-sivun, joka näyttää tiedot factory sisällön.   

    ![Tietoja Factory-sivu](./media/data-factory-build-your-first-pipeline-using-editor/data-factory-blade.png)

Ennen tietojen factory putkijohto luontia, sinun täytyy luoda muutaman Data Factory kohteiden ensin. Sinun on luotava linkitetyn palveluiden tietojen stores/laskee linkittäminen tiedot tallennetaan, määrittää syötteen ja tulosteen linkitettyjä tietoja stores-i/koskevat tiedot ja luoda sitten aktiviteettiin, joka käyttää näitä tietojoukkoja putkijohto tietojoukkoja. 

## <a name="create-linked-services"></a>Luo linkitetty palvelut
Tässä vaiheessa voit linkittää tiedot factory Azure-tallennustilan tilin ja tarvittaessa-Azure HDInsight-klusterin. Azure-tallennustilan tilin on tässä esimerkissä putkijohto syöttö- ja tiedot. Linkitetty HDInsight-palvelua käytetään suorittamalla rakenteen määritetty tässä esimerkissä putkijohto toiminnan. Määritä, mitä [tietovaraston](data-factory-data-movement-activities.md)/[Laske services](data-factory-compute-linked-services.md) käytetään käyttämässäsi skenaariossa ja linkittää tiedot factory näistä palveluista luomalla linkitetyn palvelut.  

### <a name="create-azure-storage-linked-service"></a>Luo linkitetty Azure-tallennustilan
Tässä vaiheessa voit linkittää Azure-tallennustilan tilin tiedot factory. Tässä opetusohjelmassa käyttää samaa Azure-tallennustilan tilin i/tiedot ja HQL-komentosarjatiedosto tallentamiseen. 

1.  Valitse **tekijän ja ota käyttöön** -for **GetStartedDF** **DATA FACTORY** -sivu. Näkyviin tulee tietojen Factory-editori. 
     
    ![Luo ja ota käyttöön ruutu](./media/data-factory-build-your-first-pipeline-using-editor/data-factory-author-deploy.png)
2.  **Tallentaa uudet tiedot** ja valitse **Azure-tallennustilan**.

    ![Uusien tietojen tallentaminen - Azuren tallennustilaan - valikko](./media/data-factory-build-your-first-pipeline-using-editor/new-data-store-azure-storage-menu.png)

3.  Raportissa pitäisi näkyä JSON komentosarjan luominen Azure-tallennustilan linkitetty service-editorissa. 
    
    ![Azure linkitetty tallennuspalvelu](./media/data-factory-build-your-first-pipeline-using-editor/azure-storage-linked-service.png)
     
4. Korvaa **tilinimen** pikanäppäin Azure-tallennustilan tilin nimi Azure-tallennustilan tilin ja **tilin avain** . Lisätietoja tallennustilan pikanäppäin hankkiminen on artikkelissa [näkymän, kopioi ja muodosta uudelleen sarjanumerot tallennustilan pikanäppäimet](../storage/storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys)
5. Valitse **Ota käyttöön** komentopalkista linkitetyn palvelun käyttöön.

    ![Ota käyttöön-painike](./media/data-factory-build-your-first-pipeline-using-editor/deploy-button.png)

   Kun linkitetyn palvelu on otettu käyttöön onnistuneesti, **Luonnos-1** -ikkunassa pitäisi hävitä ja tuo näkyviin **AzureStorageLinkedService** puunäkymän vasemmalla. 
    ![Linkitetyn Tallennuspalvelu-valikko](./media/data-factory-build-your-first-pipeline-using-editor/StorageLinkedServiceInTree.png)   

 
### <a name="create-azure-hdinsight-linked-service"></a>Luo linkitetty Azure HDInsight-palvelu
Tässä vaiheessa voit linkittää tarvittaessa-HDInsight-klusterin tietojen factory. HDInsight-klusterin automaattisesti suorituksen luodaan ja poistetaan, kun se on valmis käsittely ja käyttämättömänä tietyn ajanjakson aikana. 

1. Valitse **Tietojen Factory editorin** **... Lisää**, valitse **Uusi laskemiseen**ja valitse **tarvittaessa HDInsight-klusterin**.

    ![Uusi suorittaminen](./media/data-factory-build-your-first-pipeline-using-editor/new-compute-menu.png)
2. Kopioi ja liitä seuraava koodikatkelman **Luonnos-1** -ikkunaan. JSON-koodikatkelman kuvataan ominaisuudet, joita käytetään luomaan HDInsight klusterin tarvittaessa. 

        {
          "name": "HDInsightOnDemandLinkedService",
          "properties": {
            "type": "HDInsightOnDemand",
            "typeProperties": {
              "version": "3.2",
              "clusterSize": 1,
              "timeToLive": "00:30:00",
              "linkedServiceName": "AzureStorageLinkedService"
            }
          }
        }
    
    Seuraavassa taulukossa on käytetty koodikatkelman JSON-ominaisuuksien kuvauksia:
    
  	| Ominaisuus | Kuvaus |
  	| :------- | :---------- |
  	| Versio | Määrittää, että HDInsight-version luonut on 3,2. | 
  	| ClusterSize | Määrittää HDInsight-klusterin koon. | 
  	| TimeToLive | Määrittää, että HDInsight-klusterin joutoaika ennen kuin se poistetaan. |
  	| linkedServiceName | Määrittää tallennustilan-tili, jota käytetään lokit, jotka on luotu HDInsight tallentamiseen. |

    Huomaa seuraavat seikat: 
    
    - Data Factory Luo **Windows-pohjaisesta** HDInsight-klusterin, JSON kanssa. Voit myös voi olla se **Linux-pohjaiset** HDInsight-klusterin luominen. Katso [Tarvittaessa HDInsight linkitetyn palvelun](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) tiedot. 
    - Voit käyttää **omaa HDInsight-klusterin** tarvittaessa-HDInsight-klusterin sijaan. Katso [HDInsight linkitetyn palvelun](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) tiedot.
    - HDInsight-klusterin Luo **oletusarvon säilö** blob-säiliö määrittämäsi JSON (**linkedServiceName**). HDInsight Poista tämä säilö klusterin poistamisen yhteydessä. Tämä on suunniteltu ominaisuus. Tarvittaessa HDInsight linkitetty palvelussa HDInsight-klusterin luodaan aina, kun sektoria käsitellään, ellei aiemmin live klusterin (**timeToLive**). Klusterin poistetaan automaattisesti, kun käsittely on valmis.
    
        Lisää sektoreiden käsitellään, näet monta säilöjen Azuren Blob-objektien tallennustilaan. Jos ei tarvitse niitä töiden vianmääritystä varten, haluat ehkä poistaa ne tallennustilan pienentää. Nämä säilöjä nimet noudattavat kuviolla: "syöttö**yourdatafactoryname**-**linkedservicename**- datetimestamp". Poistaa Azure-Blob-objektien tallennustilaan säilöjen Työkalut [Exploreria tallennustilan](http://storageexplorer.com/) kuten avulla.

    Katso [Tarvittaessa HDInsight linkitetyn palvelun](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) tiedot.
3. Valitse **Ota käyttöön** komentopalkista linkitetyn palvelun käyttöön. 

    ![Ottaa käyttöön tarvittaessa linkitetty HDInsight-palvelu](./media/data-factory-build-your-first-pipeline-using-editor/ondemand-hdinsight-deploy.png)

4. Varmista, että näet **AzureStorageLinkedService** ja **HDInsightOnDemandLinkedService** puun tarkastella vasemmalla.

    ![Puunäkymän linkitetyn-palvelujen kanssa](./media/data-factory-build-your-first-pipeline-using-editor/tree-view-linked-services.png)

## <a name="create-datasets"></a>Luo tietojoukkoja
Tässä vaiheessa voit luoda tietojoukkoja esittämään syötteen ja tulostaa tiedot rakenteen käsittelyä varten. Nämä tietojoukkoja viitata **AzureStorageLinkedService** olet luonut aiemmin tässä opetusohjelmassa. Azure-tallennustilan tilin ja tietojoukkoja linkitetyistä kohdeosoite Määritä säilö, kansion, tiedostonimi muistiin, jossa on syötteen ja tulostaa tiedot.   

### <a name="create-input-dataset"></a>Kirjoita tietojoukon luominen

1. Valitse **Tietojen Factory editorin** **... Lisää** komentopalkin, valitse **Uusi tietojoukko**ja valitse **Azure-Blob-säiliö**.

    ![Uusi tietojoukko](./media/data-factory-build-your-first-pipeline-using-editor/new-data-set.png)
2. Kopioi ja liitä seuraava koodikatkelman luonnos-1-ikkunaan. JSON-koodikatkelman luot kutsutaan **AzureBlobInput** , joka edustaa putkijohto toimintaa syöttötiedot tietojoukko. Lisäksi voit määrittää, että syöttötiedot sijaitsee kutsutaan **adfgetstarted** blob-säilö ja **inputdata**-nimiseen kansioon.
        
        {
            "name": "AzureBlobInput",
            "properties": {
                "type": "AzureBlob",
                "linkedServiceName": "AzureStorageLinkedService",
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
  	| linkedServiceName | viittaa aiemmin luomasi AzureStorageLinkedService. |
  	| Tiedostonimi | Tämä ominaisuus on valinnainen. Jos tätä ominaisuutta, kaikki tiedostot kansiopolku kerätään. Tässä tapauksessa vain input.log käsitellään. |
  	| tyyppi | Lokitiedostojen ovat teksti-muodossa, joten Käytämme TekstinMuoto. | 
  	| columnDelimiter | lokitiedostojen sarakkeissa on erotettu toisistaan pilkku () |
  	| Toistumisvälin / | Kuukausi-ja aikavälin korkojakso on 1, mikä tarkoittaa, että syötteen sektorit ovat käytettävissä kuukausittain. | 
  	| Ulkoinen | Tämä ominaisuus on määritetty tosi, jos syöttötiedot ei luoda Data Factory-palvelu. | 
        
3. Valitse **Ota käyttöön** ottamaan juuri luomasi tietojoukko komentopalkista. Raportissa pitäisi näkyä tietojoukko puunäkymässä vasemmalla. 


### <a name="create-output-dataset"></a>Tulosteen tietojoukon luominen
Nyt voit luoda tulostus-tietojoukko Azure-Blob-objektien tallennustilaan tallennettuja tulosteen tietojen esittämiseen. 

1. Valitse **Tietojen Factory editorin** **... Lisää** komentopalkin, valitse **Uusi tietojoukko**ja valitse **Azure-Blob-säiliö**.  
2. Kopioi ja liitä seuraava koodikatkelman luonnos-1-ikkunaan. JSON-koodikatkelman luot kutsutaan **AzureBlobOutput**ja määrittämällä tiedot, jotka rakenteen komentosarjan tuottamat rakenteen tietojoukko. Lisäksi voit määrittää, että tulokset tallennetaan blob-säilö kutsutaan **adfgetstarted** ja **partitioneddata**-nimiseen kansioon. **Käytettävyys** -osassa määrittää, että tulostus-tietojoukko on valmistettu kuukausittain.
    
        {
          "name": "AzureBlobOutput",
          "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
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
3. Valitse **Ota käyttöön** ottamaan juuri luomasi tietojoukko komentopalkista.
4. Tarkista tietojoukon luominen onnistui.

    ![Puunäkymän linkitetyn-palvelujen kanssa](./media/data-factory-build-your-first-pipeline-using-editor/tree-view-data-set.png)

## <a name="create-pipeline"></a>Myyntijakso luominen
Tässä vaiheessa voit luoda ensimmäisen myyntijakso **HDInsightHive** toimintojen kanssa. Syötteen sektori on käytettävissä kuukausittain (korkojakso: kuukausi, aikaväli: 1), tulos sektori on valmistettu kuukausittain ja tehtävän ajoitus-ominaisuudeksi on määritetty myös kuukausittain. Tulostus-tietojoukko ja tehtävän ajoitus asetukset on vastattava. Tulosteen tietojoukko ei tällä hetkellä, mitä ohjaa aikataulussa, joten sinun on luotava tulostus-tietojoukko, vaikka tehtävän tuota tuloksen. Jos tehtävän sähköpostit tulevat kaikki syötetyt, voit ohittaa syötteen tietojoukon luominen. Seuraavat JSON ominaisuuksia on esitelty tämän osan loppuun. 

1. Napsauta **Tietojen Factory editorin** **kolme pistettä (...) Lisää komentoja** ja valitse sitten **Uusi myyntijakso**.
    
    ![Uusi myyntijakso-painike](./media/data-factory-build-your-first-pipeline-using-editor/new-pipeline-button.png)
2. Kopioi ja liitä seuraava koodikatkelman luonnos-1-ikkunaan.

    > [AZURE.IMPORTANT] Korvaa **storageaccountname** JSON-tallennustilan tilin nimi.
        
        {
            "name": "MyFirstPipeline",
            "properties": {
                "description": "My first Azure Data Factory pipeline",
                "activities": [
                    {
                        "type": "HDInsightHive",
                        "typeProperties": {
                            "scriptPath": "adfgetstarted/script/partitionweblogs.hql",
                            "scriptLinkedService": "AzureStorageLinkedService",
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
    
    Rakenne-komentosarjatiedosto, **partitionweblogs.hql**tallennetaan Azure-tallennustilan tilin (määritetty scriptLinkedService kutsutaan **AzureStorageLinkedService**) ja säilö **adfgetstarted** **komentosarja** -kansiossa.

    **Määrittää** -osa käytetään välitetään rakenne-komentosarjan rakenteen määritys arvoina runtime-asetusten määrittäminen (esimerkiksi ${hiveconf: inputtable}, {hiveconf:partitionedtable} $).

    **Käynnistä** ja **Lopeta** ominaisuuksien putkijohto määrittää putkijohto aktiivisen kauden.

    Määritä tehtävän JSON, että rakenne-komentosarja suoritetaan **linkedServiceName** – **HDInsightOnDemandLinkedService**määrittämää suorittaminen.

    > [AZURE.NOTE] Katso lisätietoja JSON-ominaisuuksien käyttäminen esimerkissä [putkijohto-julkaisusivuston rakenne](data-factory-create-pipelines.md#anatomy-of-a-pipeline) . 

3. Tarkista seuraavat: 
    1. **Input.log** tiedosto on Azure-blob-säiliö **adfgetstarted** -säilön **inputdata** -kansiossa
    2. **partitionweblogs.hql** tiedosto on Azure-blob-säiliö **adfgetstarted** -säilö **komentosarja** -kansiossa. Valmis täytä edellytyksiä vaiheet [Opetusohjelma yleiskatsaus](data-factory-build-your-first-pipeline.md) Jos et näe nämä tiedostot. 
    3. Varmista, että **storageaccountname** korvattu myyntijakso JSON-tallennustilan tilin nimi. 
2. Valitse **Ota käyttöön** ottamaan putkijohto komentopalkista. Koska ** **Käynnistä** - ja** päättymisajat asetetaan aiemman ja **isPaused** on määritetty epätosi, putkijohto (tehtävä putkijohto) suorittaa heti, kun otat käyttöön. 
4. Vahvista, näet putkijohto puunäkymässä.

    ![Puunäkymän myyntijakso kanssa](./media/data-factory-build-your-first-pipeline-using-editor/tree-view-pipeline.png)
5. Onnittelut, olet luonut ensimmäisen myyntijakso!

## <a name="monitor-pipeline"></a>Näytön myyntijakso

### <a name="monitor-pipeline-using-diagram-view"></a>Näytön myyntijakso käyttämällä

6. Sulje tiedot Factory editorin lavat ja palata Data Factory-sivu **X** ja valitse **kaavio**.
  
    ![Kaavio-ruutu](./media/data-factory-build-your-first-pipeline-using-editor/diagram-tile.png)
7. Kaavionäkymässä näet, putkistot ja käyttää tässä opetusohjelmassa tietojoukkoja.
    
    ![Kaavionäkymä](./media/data-factory-build-your-first-pipeline-using-editor/diagram-view-2.png) 
8. Jos haluat tarkastella kaikkia toimintoja putkijohto, napsauta kaavion putkijohto ja valitse Avaa myyntijakso. 

    ![Avaa myyntijakso-valikko](./media/data-factory-build-your-first-pipeline-using-editor/open-pipeline-menu.png)
9. Varmista, että näet putkijohto HDInsightHive-toimintojen. 
  
    ![Avaa myyntijakso-näkymä](./media/data-factory-build-your-first-pipeline-using-editor/open-pipeline-view.png)

    Voit palata edelliseen näkymään, valitse **Data factory** yläreunassa linkkipolun-valikossa. 
10. Kaksoisnapsauta **Kaavionäkymä**tietojoukko **AzureBlobInput**. Varmista, että sektoria on **Valmis** -tilaan. Voi kestää muutaman minuutin sektorin lisääminen valmiina. Jos se ei ole aktivoitu, kun odotat viime, tarkista, ovatko oikean säilö (adfgetstarted) ja (inputdata)-kansio sijoitetaan syötteen tiedosto (input.log).

    ![Syötteen sektoria Valmis-tilaan](./media/data-factory-build-your-first-pipeline-using-editor/input-slice-ready.png)
11. Valitse Sulje **AzureBlobInput** sivu **X** . 
12. Kaksoisnapsauta **Kaavionäkymä**tietojoukko **AzureBlobOutput**. Näet, joka käsitellään parhaillaan sektoria.

    ![Tietojoukko](./media/data-factory-build-your-first-pipeline-using-editor/dataset-blade.png)
9. Kun käsittely on valmis, näyttöön tulee sektoria **Valmis** -tilaan.
    
>[AZURE.IMPORTANT] Tarvittaessa-HDInsight-klusterin luominen yleensä kestää viime (noin 20 minuuttia). Tämän vuoksi toiminta voi kestää **noin 30 minuuttia** käsittelemään sektoria putkijohto.    

    ![Dataset](./media/data-factory-build-your-first-pipeline-using-editor/dataset-slice-ready.png) 
    
10. Kun sektoria on **valmiina** , tarkista **adfgetstarted** säilössä Blob-objektien tallennustilaan tulosteen tietojen **partitioneddata** -kansio.  
 
    ![siirtää tietoja](./media/data-factory-build-your-first-pipeline-using-editor/three-ouptut-files.png)
11. Valitse sektoria, jos haluat nähdä lisätietoja sen- **tietojen muotoilu** -sivu.

    ![Tietojen muotoilu](./media/data-factory-build-your-first-pipeline-using-editor/data-slice-details.png)  
12. Valitse tehtävän suorittaminen **tehtävän lohkot luettelosta** lisätietoja aktiviteetin suorittaa **tehtävän suorittaminen tiedot** -ikkunassa (Tässä skenaariossa aktiviteetin rakenne).   

    ![Tehtävän suorittaminen tiedot](./media/data-factory-build-your-first-pipeline-using-editor/activity-window-blade.png)  
    
    Lokitiedostoista näet rakenne-kysely, joka on suoritettu ja tilatiedot. Nämä lokit ovat mahdollisten ongelmien vianmäärityksessä.
Katso [näyttö ja hallita käyttämällä Azure portaalin lavat putkistot](data-factory-monitor-manage-pipelines.md) artikkelissa lisätietoja. 

> [AZURE.IMPORTANT] Lähdetiedoston saa poistetaan, kun sektoria käsittely on onnistunut. Vuoksi halutessasi uudelleen sektoria tai tehdä opetusohjelman uudelleen input-tiedoston lataaminen (input.log) adfgetstarted säilön inputdata-kansio.

### <a name="monitor-pipeline-using-monitor--manage-app"></a>Näytön myyntijakso näyttö ja hallinta-sovelluksen käyttäminen
Voit myös käyttää näytön & seurannassa oman putkistot-sovelluksen hallinta. Lisätietoja tämän sovelluksen käyttämisestä on artikkelissa [näyttö ja hallita Azure Data Factory putkistot seuranta ja hallinta-sovelluksen käyttäminen](data-factory-monitor-manage-app.md).

1. Valitsemalla **näytön ja hallinta** tietojen factory kotisivulle ruutu.

    ![Seurata ja hallita ruutu](./media/data-factory-build-your-first-pipeline-using-editor/monitor-and-manage-tile.png) 
2. Raportissa pitäisi näkyä **näyttö ja hallinta-sovellus**. **Alkamisaika** ja **Päättymisaika** vastaamaan Käynnistä (04 01 – 2016 klo 24.00)- ja päättymisajat (04-02-2016 klo 24.00), että putkijohto ja sitten **Käytä**.

    ![Seuranta ja hallinta-sovelluksella](./media/data-factory-build-your-first-pipeline-using-editor/monitor-and-manage-app.png) 
3. Valitse tehtävä-ikkunassa **Toiminta Windows** -luettelon ja tarkastella sen tietoja. 
    ![Tehtävän ikkunan tiedot](./media/data-factory-build-your-first-pipeline-using-editor/activity-window-details.png)


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

  

