<properties
    pageTitle="Luo ensimmäinen tietojen factory (REST) | Microsoft Azure"
    description="Tässä opetusohjelmassa luominen käyttämällä tietojen Factory REST API otoksen Azure Data Factory putkijohto."
    services="data-factory"
    documentationCenter=""
    authors="spelluru"
    manager="jhubbard"
    editor="monicar"
/>

<tags
    ms.service="data-factory"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="08/16/2016"
    ms.author="spelluru"/>

# <a name="tutorial-build-your-first-azure-data-factory-using-data-factory-rest-api"></a>Opetusohjelma: Luo ensimmäinen factory Azure tietojen käyttäminen tietojen Factory REST-Ohjelmointirajapinnalla
> [AZURE.SELECTOR]
- [Yleiskatsaus ja edellytykset](data-factory-build-your-first-pipeline.md)
- [Azure portal](data-factory-build-your-first-pipeline-using-editor.md)
- [Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
- [PowerShellin](data-factory-build-your-first-pipeline-using-powershell.md)
- [Resurssienhallinta-malli](data-factory-build-your-first-pipeline-using-arm.md)
- [REST-OHJELMOINTIRAJAPINNALLA](data-factory-build-your-first-pipeline-using-rest-api.md)

Tässä artikkelissa käyttää tietojen Factory REST API ensimmäisen Azure tiedot-factory luomiseen.

## <a name="prerequisites"></a>Edellytykset
- Lue artikkeli [Opetusohjelma yleiskatsaus](data-factory-build-your-first-pipeline.md) ja **edellytyksenä** vaiheiden avulla.
- Asenna tietokoneeseen [Curl](https://curl.haxx.se/dlwiz/) . KÄÄNTÖ-työkalun käyttäminen muut komennot tietojen factory luomiseen. 
- Noudata [tämän artikkelin](../resource-group-create-service-principal-portal.md) ohjeita: 
    1. Luo nimetty **ADFGetStartedApp** Azure Active Directory-verkkosovellus.
    2. Hanki **Asiakastunnus** ja **salausavaimen**. 
    3. Hae **vuokraajan tunnus**. 
    4. Määrittää **ADFGetStartedApp** sovelluksen **Tietojen Factory osallistujan** rooli.  
- [Azure PowerShellin](../powershell-install-configure.md)asentaminen.  
- Käynnistä **PowerShell** ja suorittamalla seuraavan komennon. Pidä PowerShellin Azure auki tässä opetusohjelmassa loppuun saakka. Sulje ja avaa se uudelleen, jos haluat suorittaa komennot uudelleen.
    1. Suorita **Kirjautuminen AzureRmAccount** ja Anna käyttäjänimi ja salasana, jonka avulla Kirjaudu Azure-portaaliin.  
    2. Voit tarkastella kaikkien tilausten tilin **Get-AzureRmSubscription** suorittaminen
    3. Suorita **Get-AzureRmSubscription - SubscriptionName NameOfAzureSubscription | Määritä AzureRmContext** Valitse tilaus, jota haluat käyttää. Korvaa **NameOfAzureSubscription** Azure tilauksen nimeä. 
3. Luo nimetty **ADFTutorialResourceGroup** suorittamalla seuraava komento PowerShell Azure resurssiryhmä:  

        New-AzureRmResourceGroup -Name ADFTutorialResourceGroup  -Location "West US"

    Seuraavia asioita Tässä opetusohjelmassa oletetaan, että käytät nimeltä ADFTutorialResourceGroup resurssiryhmä. Jos käytät eri resurssiryhmä, sinun on käytettävä resurssiryhmän tekstin näyttäminen ADFTutorialResourceGroup nimi Tässä opetusohjelmassa.

## <a name="create-json-definitions"></a>Luo JSON määritelmät
Luo seuraavat JSON tiedostot-kansio, johon curl.exe sijaitsee. 

### <a name="datafactoryjson"></a>DataFactory.JSON 
> [AZURE.IMPORTANT] Nimen on oltava yksilöivä, jotta voit halutessasi etuliite/loppuliite ADFCopyTutorialDF, minkä ansiosta yksilöivä nimi. 

    {  
        "name": "FirstDataFactoryREST",  
        "location": "WestUS"
    }  

### <a name="azurestoragelinkedservicejson"></a>azurestoragelinkedservice.JSON
> [AZURE.IMPORTANT] Korvaa **accountname** ja **accountkey** nimen ja avaimen Azure-tallennustilan tilin. Lisätietoja tallennustilan pikanäppäin hankkiminen on artikkelissa [näkymän, kopioi ja muodosta uudelleen sarjanumerot tallennustilan pikanäppäimet](../storage/storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys).

    {
        "name": "AzureStorageLinkedService",
        "properties": {
            "type": "AzureStorage",
            "typeProperties": {
                "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
            }
        }
    }


### <a name="hdinsightondemandlinkedservicejson"></a>hdinsightondemandlinkedservice.JSON

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
| ClusterSize | HDInsight-klusterin kokoa. | 
| TimeToLive | Määrittää, että HDInsight-klusterin joutoaika ennen kuin se poistetaan. |
| linkedServiceName | Määrittää tallennustilan-tili, jota käytetään tallentamiseen HDInsight avulla luotujen lokit |

Huomaa seuraavat seikat: 

- Data Factory Luo **Windows-pohjaisesta** HDInsight-klusterin yllä JSON kanssa. Voit myös voi olla se **Linux-pohjaiset** HDInsight-klusterin luominen. Katso [Tarvittaessa HDInsight linkitetyn palvelun](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) tiedot. 
- Voit käyttää **omaa HDInsight-klusterin** tarvittaessa-HDInsight-klusterin sijaan. Katso [HDInsight linkitetyn palvelun](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) tiedot.
- HDInsight-klusterin Luo **oletusarvon säilö** blob-säiliö määrittämäsi JSON (**linkedServiceName**). HDInsight Poista tämä säilö klusterin poistamisen yhteydessä. Tämä on suunniteltu ominaisuus. Tarvittaessa linkitetty HDInsight-palveluun HDInsight, klusterin luodaan aina, kun sektoria käsitellään, ellei aiemmin luodun klusterin (**timeToLive**) live ja poistetaan, kun käsittely on valmis.

    Lisää sektoreiden käsitellään, näet monta säilöjen Azuren Blob-objektien tallennustilaan. Jos ei tarvitse niitä töiden vianmääritystä varten, haluat ehkä poistaa ne tallennustilan pienentää. Nämä säilöjä nimet noudattavat kuviolla: "syöttö**yourdatafactoryname**-**linkedservicename**- datetimestamp". Poistaa Azure-Blob-objektien tallennustilaan säilöjen Työkalut [Exploreria tallennustilan](http://storageexplorer.com/) kuten avulla.

Katso [Tarvittaessa HDInsight linkitetyn palvelun](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) tiedot. 

### <a name="inputdatasetjson"></a>inputdataset.JSON

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


JSON määrittää nimeltä **AzureBlobInput**, joka edustaa putkijohto toimintaa syöttötiedot tietojoukko. Lisäksi se määrittää, että syöttötiedot sijaitsee kutsutaan **adfgetstarted** blob-säilö ja **inputdata**-nimiseen kansioon.

Seuraavassa taulukossa on käytetty koodikatkelman JSON-ominaisuuksien kuvauksia:

| Ominaisuus | Kuvaus |
| :------- | :---------- |
| tyyppi | Tyyppi-ominaisuudeksi on määritetty AzureBlob, koska tiedot sijaitsevat Azure Blob-objektien tallennustilaan. |  
| linkedServiceName | viittaa aiemmin luomasi StorageLinkedService. |
| Tiedostonimi | Tämä ominaisuus on valinnainen. Jos tätä ominaisuutta, kaikki tiedostot kansiopolku kerätään. Tässä tapauksessa vain input.log käsitellään. |
| tyyppi | Lokitiedostojen ovat teksti-muodossa, joten Käytämme TekstinMuoto. | 
| columnDelimiter | lokitiedostojen sarakkeissa on erotettu toisistaan pilkku () |
| Toistumisvälin / | Kuukausi-ja aikavälin korkojakso on 1, mikä tarkoittaa, että syötteen sektorit ovat käytettävissä kuukausittain. | 
| Ulkoinen | Tämä ominaisuus on määritetty tosi, jos syöttötiedot ei luoda Data Factory-palvelu. | 

### <a name="outputdatasetjson"></a>outputdataset.JSON

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

JSON määrittää nimeltä **AzureBlobOutput**, joka edustaa tulosteen tiedot putkijohto toimintaa tietojoukko. Lisäksi se määrittää, että tulokset tallennetaan blob-säilö kutsutaan **adfgetstarted** ja **partitioneddata**-nimiseen kansioon. **Käytettävyys** -osassa määrittää, että tulostus-tietojoukko on valmistettu kuukausittain.

### <a name="pipelinejson"></a>Pipeline.JSON
> [AZURE.IMPORTANT] Korvaa **storageaccountname** Azure-tallennustilan tilin nimi. 


    {
        "name": "MyFirstPipeline",
        "properties": {
            "description": "My first Azure Data Factory pipeline",
            "activities": [{
                "type": "HDInsightHive",
                "typeProperties": {
                    "scriptPath": "adfgetstarted/script/partitionweblogs.hql",
                    "scriptLinkedService": "AzureStorageLinkedService",
                    "defines": {
                        "inputtable": "wasb://adfgetstarted@<stroageaccountname>.blob.core.windows.net/inputdata",
                        "partitionedtable": "wasb://adfgetstarted@<stroageaccountname>t.blob.core.windows.net/partitioneddata"
                    }
                },
                "inputs": [{
                    "name": "AzureBlobInput"
                }],
                "outputs": [{
                    "name": "AzureBlobOutput"
                }],
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
            }],
            "start": "2016-07-10T00:00:00Z",
            "end": "2016-07-11T00:00:00Z",
            "isPaused": false
        }
    }

JSON-koodikatkelman luot putkijohto, joka koostuu yhden aktiviteetin, joka käyttää rakenteen HDInsight-klusterin prosessin tietoihin.

Rakenne-komentosarjatiedosto **partitionweblogs.hql**tallennetaan Azure-tallennustilan tilin (määritetty scriptLinkedService kutsutaan **StorageLinkedService**) ja säilö **adfgetstarted** **komentosarja** -kansiossa.

**Määrittää** osa määrittää runtime-asetuksia, jotka siirretään rakenne-komentosarjan rakenteen määritys arvoina (esimerkiksi ${hiveconf: inputtable}, {hiveconf:partitionedtable} $).

**Käynnistä** ja **Lopeta** ominaisuuksien putkijohto määrittää putkijohto aktiivisen kauden.

Määritä tehtävän JSON, että rakenne-komentosarja suoritetaan **linkedServiceName** – **HDInsightOnDemandLinkedService**määrittämää suorittaminen.

> [AZURE.NOTE] Katso lisätietoja edellisessä esimerkissä käytetty JSON-ominaisuudet [putkijohto-julkaisusivuston rakenne](data-factory-create-pipelines.md#anatomy-of-a-pipeline) . 

## <a name="set-global-variables"></a>Määritä yleiset muuttujat

PowerShellin Azure-Suorita jälkeen arvojen korvaaminen omalla seuraavia komentoja:

> [AZURE.IMPORTANT] Katso [edellytykset](#prerequisites) osa ohjeita käytön Asiakastunnus-asiakasohjelman salaisuus vuokraaja tunnuksen ja tilauksen tunnus.   

    $client_id = "<client ID of application in AAD>"
    $client_secret = "<client key of application in AAD>"
    $tenant = "<Azure tenant ID>";
    $subscription_id="<Azure subscription ID>";

    $rg = "ADFTutorialResourceGroup"
    $adf = "FirstDataFactoryREST"



## <a name="authenticate-with-aad"></a>AAD todentamismenetelmä

    $cmd = { .\curl.exe -X POST https://login.microsoftonline.com/$tenant/oauth2/token  -F grant_type=client_credentials  -F resource=https://management.core.windows.net/ -F client_id=$client_id -F client_secret=$client_secret };
    $responseToken = Invoke-Command -scriptblock $cmd;
    $accessToken = (ConvertFrom-Json $responseToken).access_token;
    
    (ConvertFrom-Json $responseToken) 



## <a name="create-data-factory"></a>Tietoja factory luominen

Tässä vaiheessa voit luoda Azure Data Factory nimeltä **FirstDataFactoryREST**. Tietoja factory voi olla yksi tai useampi putkistot. Putkijohto voi olla vähintään yksi toimintoja ei. Esimerkiksi kopioi toiminta on tietojen kopioiminen lähteen kohde tietosäilö ja HDInsight-rakenne-aktiviteetin suorittamalla rakenteen tietojen muuntamiseen. Suorita seuraavat komennot tietojen factory luomiseen: 

1. Määritä muuttujan **cmd**komento. 

    Varmista, että tietojen factory nimi määrittämiäsi (ADFCopyTutorialDF) vastaa määritettyjä **datafactory.json**nimeä. 

        $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data “@datafactory.json” https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/FirstDataFactoryREST?api-version=2015-10-01};
2. Suorita-komento **Käynnistä-komennon**avulla.

        $results = Invoke-Command -scriptblock $cmd;
3. Tarkastele tuloksia. Jos tietojen tehdas on luotu, näet tietoja factory **tulokset**; JSON-MUOTOON Muussa tapauksessa näyttöön tulee virhesanoma.  

        Write-Host $results

Huomaa seuraavat seikat:
 
- Azure Data Factory nimen on oltava yksilöivä. Jos näet tulokset virheen: **tietojen factory nimi "FirstDataFactoryREST" ei ole käytettävissä**, toimi seuraavasti:  
    1. Muuta **datafactory.json** -tiedoston nimi (esimerkiksi yournameFirstDataFactoryREST). Data Factory palvelutiedot nimeämiskäytäntö säännöt [Tietojen Factory - nimeäminen säännöt](data-factory-naming-rules.md) aiheessa.
    2. Valitse missä **$cmd** muuttujan määritetään arvo ensimmäisen komennon FirstDataFactoryREST tilalle uusi nimi ja suorita-komento. 
    3. Suorita seuraavat kaksi komennot käynnistää REST-Ohjelmointirajapinnalla tietojen factory luomiseen ja tulostamiseen toiminnon tulokset. 
- Voit luoda Data Factory esiintymät, sinun on oltava osallistuja tai järjestelmänvalvojan Azure-tilaukseen
- Tietoja factory nimi voidaan rekisteröidä myöhemmin ja näin ollen tulevat näkyviin julkisesti DNS-nimi.
- Jos saat virheilmoituksen: "**tätä tilausta ei ole rekisteröity käyttämään nimitilan Microsoft.DataFactory**", tee jokin seuraavista toimista ja yritä julkaista uudelleen: 

    - PowerShellin Azure suorittamalla seuraavan komennon rekisteröidä Data Factory-palvelu: 
        
            Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
    
        Voit suorittamalla seuraavan komennon voit varmistaa, että palvelu on rekisteröity tietojen Factory: 
    
            Get-AzureRmResourceProvider
    - Kirjaudu sisään käyttämällä Azure tilauksen [Azure portaaliin](https://portal.azure.com) ja siirry Data Factory-sivu (tai) luominen tietojen factory Azure-portaalissa. Tämä toiminto rekisteröi palveluntarjoajan puolestasi.

Ennen kuin luot putkijohto, sinun täytyy luoda muutaman Data Factory kohteiden ensin. Sinun on luotava linkitetyn palveluiden tietojen stores/laskee linkittäminen tiedot tallennetaan, Määritä syötteen ja tulosteen tietojoukkoja linkitettyjä tietoja tallentaa tiedot. 

## <a name="create-linked-services"></a>Luo linkitetty palvelut 
Tässä vaiheessa voit linkittää tiedot factory Azure-tallennustilan tilin ja tarvittaessa-Azure HDInsight-klusterin. Azure-tallennustilan tilin on tässä esimerkissä putkijohto syöttö- ja tiedot. Linkitetty HDInsight-palvelua käytetään suorittamalla rakenteen määritetty tässä esimerkissä putkijohto toiminnan. 

### <a name="create-azure-storage-linked-service"></a>Luo linkitetty Azure-tallennustilan
Tässä vaiheessa voit linkittää Azure-tallennustilan tilin tiedot factory. Tämän opetusohjelman käyttää samaa Azure-tallennustilan tilin i/tiedot ja HQL-komentosarjatiedosto tallentamiseen.

1. Määritä muuttujan **cmd**komento. 

        $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data “@azurestoragelinkedservice.json” https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/linkedservices/AzureStorageLinkedService?api-version=2015-10-01};
2. Suorita-komento **Käynnistä-komennon**avulla.
 
        $results = Invoke-Command -scriptblock $cmd;
3. Tarkastele tuloksia. Jos linkitetty palvelu on luotu, näet **tulokset**; linkitetyn palvelun JSON Muussa tapauksessa näyttöön tulee virhesanoma.
  
        Write-Host $results

### <a name="create-azure-hdinsight-linked-service"></a>Luo linkitetty Azure HDInsight-palvelu
Tässä vaiheessa voit linkittää tarvittaessa-HDInsight-klusterin tietojen factory. HDInsight-klusterin automaattisesti suorituksen luodaan ja poistetaan, kun se on valmis käsittely ja käyttämättömänä tietyn ajanjakson aikana. Voit käyttää omaa HDInsight-klusterin tarvittaessa-HDInsight-klusterin sijaan. [Laske linkitetyn Services](data-factory-compute-linked-services.md) lisätietoja.  

1. Määritä muuttujan **cmd**komento.
 
        $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@hdinsightondemandlinkedservice.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/linkedservices/hdinsightondemandlinkedservice?api-version=2015-10-01};
2. Suorita-komento **Käynnistä-komennon**avulla.

        $results = Invoke-Command -scriptblock $cmd;
3. Tarkastele tuloksia. Jos linkitetty palvelu on luotu, näet **tulokset**; linkitetyn palvelun JSON Muussa tapauksessa näyttöön tulee virhesanoma.  

        Write-Host $results

## <a name="create-datasets"></a>Luo tietojoukkoja
Tässä vaiheessa voit luoda tietojoukkoja esittämään syötteen ja tulostaa tiedot rakenteen käsittelyä varten. Nämä tietojoukkoja viitata **StorageLinkedService** olet luonut aiemmin tässä opetusohjelmassa. Azure-tallennustilan tilin ja tietojoukkoja linkitetyistä kohdeosoite Määritä säilö, kansion, tiedostonimi muistiin, jossa on syötteen ja tulostaa tiedot.   

### <a name="create-input-dataset"></a>Kirjoita tietojoukon luominen
Tässä vaiheessa voit luoda syötteen tiedot, jotka on tallennettu Azure-Blob-säiliö syötteen tietojoukko.

1. Määritä muuttujan **cmd**komento. 

        $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@inputdataset.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datasets/AzureBlobInput?api-version=2015-10-01};
2. Suorita-komento **Käynnistä-komennon**avulla.

        $results = Invoke-Command -scriptblock $cmd;
3. Tarkastele tuloksia. Jos dataset on luotu, näet **tulokset**; tietojoukko JSON-MUOTOON Muussa tapauksessa näyttöön tulee virhesanoma.
  
        Write-Host $results
### <a name="create-output-dataset"></a>Tulosteen tietojoukon luominen
Tässä vaiheessa voit luoda tulostus-tiedot, jotka on tallennettu Azure-Blob-säiliö tulostus-tietojoukko.

1. Määritä muuttujan **cmd**komento.
 
        $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@outputdataset.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datasets/AzureBlobOutput?api-version=2015-10-01};
2. Suorita-komento **Käynnistä-komennon**avulla.

        $results = Invoke-Command -scriptblock $cmd;
3. Tarkastele tuloksia. Jos dataset on luotu, näet **tulokset**; tietojoukko JSON-MUOTOON Muussa tapauksessa näyttöön tulee virhesanoma.
  
        Write-Host $results 

## <a name="create-pipeline"></a>Myyntijakso luominen
Tässä vaiheessa voit luoda ensimmäisen myyntijakso **HDInsightHive** toimintojen kanssa. Syötteen sektori on käytettävissä kuukausittain (korkojakso: kuukausi, aikaväli: 1), tulos sektori on valmistettu kuukausittain ja tehtävän ajoitus-ominaisuudeksi on määritetty myös kuukausittain. Tulostus-tietojoukko ja tehtävän ajoitus asetukset on vastattava. Tulosteen tietojoukko ei tällä hetkellä, mitä ohjaa aikataulussa, joten sinun on luotava tulostus-tietojoukko, vaikka tehtävän tuota tuloksen. Jos tehtävän sähköpostit tulevat kaikki syötetyt, voit ohittaa syötteen tietojoukon luominen.  

Vahvista **input.log** tiedostossa Azure-blob-säiliö **adfgetstarted/inputdata** -kansiossa ja suorittamalla seuraavan komennon ottamaan putkijohto. Koska ** **Käynnistä** - ja** päättymisajat asetetaan aiemman ja **isPaused** on määritetty epätosi, putkijohto (tehtävä putkijohto) suorittaa heti, kun otat käyttöön. 

1. Määritä muuttujan **cmd**komento.
 
        $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@pipeline.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datapipelines/MyFirstPipeline?api-version=2015-10-01};
2. Suorita-komento **Käynnistä-komennon**avulla.

        $results = Invoke-Command -scriptblock $cmd;
3. Tarkastele tuloksia. Jos dataset on luotu, näet **tulokset**; tietojoukko JSON-MUOTOON Muussa tapauksessa näyttöön tulee virhesanoma.  

        Write-Host $results
5. Onnittelut, että ensimmäinen myyntijakso Azure PowerShellin avulla on luotu!

## <a name="monitor-pipeline"></a>Näytön myyntijakso
Tässä vaiheessa käyttö tietojen Factory REST API sektoreiden yhdistämällä luotuja putkijohto seurannassa.

    $ds ="AzureBlobOutput"

    $cmd = {.\curl.exe -X GET -H "Authorization: Bearer $accessToken" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datasets/$ds/slices?start=1970-01-01T00%3a00%3a00.0000000Z"&"end=2016-08-12T00%3a00%3a00.0000000Z"&"api-version=2015-10-01};

    $results2 = Invoke-Command -scriptblock $cmd;

    IF ((ConvertFrom-Json $results2).value -ne $NULL) {
        ConvertFrom-Json $results2 | Select-Object -Expand value | Format-Table
    } else {
            (convertFrom-Json $results2).RemoteException
    }


> [AZURE.IMPORTANT] 
> Tarvittaessa-HDInsight-klusterin luominen yleensä kestää viime (noin 20 minuuttia). Tämän vuoksi toiminta voi kestää **noin 30 minuuttia** käsittelemään sektoria putkijohto.  

Suorita Käynnistä-komento ja sitten seuraavan muutoksen, kunnes näet sektoria **Valmis** alueella tai **epäonnistui** tila. Kun sektoria on valmiina, tarkista **adfgetstarted** säilössä Blob-objektien tallennustilaan tulosteen tietojen **partitioneddata** -kansio.  Yleensä tarvittaessa-HDInsight-klusterin luominen kestää jonkin aikaa.

![siirtää tietoja](./media/data-factory-build-your-first-pipeline-using-rest-api/three-ouptut-files.png)

> [AZURE.IMPORTANT] Lähdetiedoston saa poistetaan, kun sektoria käsittely on onnistunut. Vuoksi halutessasi uudelleen sektoria tai tehdä opetusohjelman uudelleen input-tiedoston lataaminen (input.log) adfgetstarted säilön inputdata-kansio.

Voit käyttää myös Azure portal sektoreiden ja mahdollisten ongelmien vianmääritys. Lisätietoja [valvoa putkistot Azure-portaalissa](data-factory-build-your-first-pipeline-using-editor.md##monitor-pipeline) .  

## <a name="summary"></a>Yhteenveto 
Tässä opetusohjelmassa luomasi suorittamalla rakenteen komentosarja HDInsight hadoop klusterissa Azure tiedot-factory prosessin tietoihin. Tietoja Factory-editorin käytössä Azure-portaalissa voit tehdä seuraavat toimet:  

1.  Luoda Azure **tietojen factory**.
2.  Luo kaksi **linkitetyn palvelut**:
    1.  **Azuren tallennustilaan** linkitetty linkki Azure-blob-tallennustilan, joka sisältää/i-tiedostojen tietoja factory-palveluun.
    2.  **Azure Hdinsightiin** tarvittaessa linkitetyn palvelu Linkitä tiedot factory tarvittaessa-HDInsight Hadoop-klusterin. Azure Data Factory luodaan HDInsight Hadoop klusterin juuri---aika Käsittele syöttötiedot ja tuottaa siirtää tietoja. 
3.  Luo kaksi **tietojoukkoja**, jotka kuvaavat syöttö- ja HDInsight rakenteen aktiviteetin putkisto-tiedot. 
4.  Luotu **myyntijakso** **HDInsight-rakenne** -tehtävän. 

## <a name="next-steps"></a>Seuraavat vaiheet
Tässä artikkelissa luomasi putkijohto tarvittaessa-Azure HDInsight-klusterin rakenteen komentosarjaa suorittava muunnos-aktiviteettiin (HDInsight tehtävä). Katso kopio-toimintojen käyttäminen tietojen kopioimiseen Azure SQL Azure-Blob- [Opetusohjelma: tietojen kopioiminen Azure SQL Azure-Blob-objektien](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

## <a name="see-also"></a>Katso myös
| Aihe | Kuvaus |
| :---- | :---- |
| [Tietoja Factory REST API-viittaus](https://msdn.microsoft.com/library/azure/dn906738.aspx) |  Täydellinen ohjeissa Data Factory cmdlet-komennot |
| [Tietojen muunnos toiminnot](data-factory-data-transformation-activities.md) | Tässä artikkelissa on tietoja muunnos toimintoja (kuten HDInsight-rakenne-muunnos käytit Tässä opetusohjelmassa) luettelo tukemat Azure Data Factory. |
| [Ajoittamisen ja suorittaminen](data-factory-scheduling-and-execution.md) | Tässä artikkelissa kerrotaan Azure Data Factory-sovelluksen malli ajoittaminen ja suorittamisen ominaisuuksia. |
| [Putkistot](data-factory-create-pipelines.md) | Tässä artikkelissa auttaa sinua ymmärtämään putkistot ja Azure Data Factory ja kuinka niitä käytetään muodostaa skenaarion tai business loppuun tietoihin perustuvien työnkulkuja. |
| [Tietojoukkoja](data-factory-create-datasets.md) | Tässä artikkelissa auttaa sinua ymmärtämään Azure Data Factory tietojoukkoja.
| [Näytön ja hallita putkistot käyttämällä Azure portaalin lavat](data-factory-monitor-manage-pipelines.md) | Tässä artikkelissa käsitellään seurata ja hallita virheenkorjaus oman putkistot Azure portaalin lavat avulla. |
| [Seurata ja hallita putkistot seuranta-sovelluksella](data-factory-monitor-manage-app.md) | Tässä artikkelissa kerrotaan, miten voit seurata ja hallita virheenkorjaus putkistot seuranta ja hallinta-sovelluksen käyttäminen. 

