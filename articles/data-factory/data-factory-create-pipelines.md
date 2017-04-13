<properties 
    pageTitle="Luo/aikataulu putkistot, ketjut Data Factory toimintojen | Microsoft Azure" 
    description="Opettele luomaan tietojen putkijohto Azure Data Factory ja muuntaa tiedot. Luoda tietojen perustuva työnkulun, joka tuottaa valmis käyttämään tietoja." 
    keywords="tietoja myyntijakso perustuva työnkulun tiedot"
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
    ms.date="09/12/2016" 
    ms.author="shlo"/>

# <a name="pipelines-and-activities-in-azure-data-factory"></a>Putkistot ja Azure Data Factory toimintoja
Tässä artikkelissa auttaa ymmärtämään putkistot ja Azure Data Factory toimintoja ja niiden avulla voit käyttää tietojen siirto ja tietojen käsittely skenaarioiden loppuun tietoihin perustuvien työnkulkuja.  

> [AZURE.NOTE] Tässä artikkelissa oletetaan, että ovat kadonneet [Azure Data Factory johdanto](data-factory-introduction.md)kautta. Jos sinulla ei ole käytännöllisellä-tiloilla-käyttökokemuksen luominen tietojen tehtaan, [Luo ensimmäinen tietojen-factory](data-factory-build-your-first-pipeline.md) opetusohjelma loppuun avulla ymmärrät paremmin tässä artikkelissa.  

## <a name="what-is-a-data-pipeline"></a>Mitä tietoja putkijohto?
**Myyntijakso** on ryhmittely loogisesti liittyviä **toimintoja**. Sitä käytetään Ryhmätehtävät yksiköksi, joka suorittaa tehtävän. Voit kartoittaa putkistot paremmin, joudut hahmottaminen tehtävän ensin. 

## <a name="what-is-an-activity"></a>Mikä on tehtävä?
Toimintojen Määritä suoritettavat tietoihisi toiminnot. Tehtävätyypin on nolla tai useampia [tietojoukkoja](data-factory-create-datasets.md) kuin syötteiden ja tuottaa yhden tai useamman tietojoukkoja kuin tulos. 

Kopioi toimintojen avulla voi esimerkiksi orchestrate kopioiminen toiseen tietovaraston yhden tietovaraston tiedot. HDInsight-rakenne-toimintojen avulla voi vastaavasti rakenteen kyselyn suorittaminen Azure HDInsight-klusterissa tietojen muuntamiseen. Azure Data Factory sisältää laajan valikoiman [muuntamiseen](data-factory-data-transformation-activities.md)ja [tietojen siirto](data-factory-data-movement-activities.md) -toimintoja. Voit myös luoda mukautetun .NET tehtävän suorittamiseen oman koodin. 

## <a name="sample-copy-pipeline"></a>Esimerkki kopioi myyntijakso
Seuraava esimerkki putkijohto on yksi tehtävä tyypin **Kopioi** **Toiminnot** -kohdassa. Tässä esimerkissä [Kopioi toiminto](data-factory-data-movement-activities.md) kopioi tiedot Azure-Blob-säiliö Azure SQL-tietokantaan. 

    {
      "name": "CopyPipeline",
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

Katso täydellinen vaiheista luomisesta tähän myyntijakso- [Opetusohjelma: tietojen kopioiminen Blob-objektien tallennustilaan SQL-tietokantaan](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md). 

## <a name="sample-transformation-pipeline"></a>Esimerkki muunnos myyntijakso
Seuraava esimerkki putkijohto on yksi tehtävä tyypin **HDInsightHive** **Toiminnot** -osassa. Tässä esimerkissä [HDInsight rakenteen tehtävän](data-factory-hive-activity.md) muunnoksia suorittamalla komentosarja rakennetiedostoon Azure Hdinsightiin Hadoop-klusterissa Azure-Blob-säiliö tiedot. 

    {
        "name": "TransformPipeline",
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

Huomaa seuraavat seikat: 

- Valitse Toiminnot-osassa on vain yksi tehtävä, jonka **tyyppi** on määritetty **HDInsightHive**.
- Rakenne-komentosarjatiedosto **partitionweblogs.hql**tallennetaan Azure-tallennustilan tilin (määritetty scriptLinkedService kutsutaan **AzureStorageLinkedService**) ja säilö **adfgetstarted** **komentosarja** -kansiossa.
- **Määrittää** -osa käytetään välitetään rakenne-komentosarjan rakenteen määritys arvoina runtime-asetusten määrittäminen (esimerkiksi ${hiveconf: inputtable}, {hiveconf:partitionedtable} $).

Katso täydellinen vaiheista luomisesta tähän myyntijakso- [Opetusohjelma: muodostaa yhteyttä ensimmäisen myyntijakso prosessin tietoihin käyttämällä Hadoop-klusterin](data-factory-build-your-first-pipeline.md). 

## <a name="chaining-activities"></a>Ketjutus toiminnot
Jos sinulla on useita tehtäviä putkijohto ja toiminnon tulos ei ole toiseen tehtävään syötteestä, toimintojen ehkä toimi rinnakkain, jos syöttötiedot sektoreiden toiminnasta on valmis. 

Voit ketjut kahden toiminnan siten, että muut tehtävän syötteen tietojoukko yhden toiminnan tulostus-tietojoukko. Toimintojen voi olla samaa putkijohtoa tai eri putkistot. Toinen tehtävä suorittaa vain kun ensimmäinen on valmis. 

Esimerkiksi seuraava tilanne:
 
1.  Myyntijakso P1 on tehtävä A1, joka edellyttää ulkoisen syötteen tietojoukko D1 ja tuottaa **tulosteen** tietojoukko **D2**.
2.  Myyntijakso P2 on tehtävä A2, joka edellyttää **input** -tietojoukko **D2**ja tuottaa tulosteen tietojoukko D3.
 
Tässä skenaariossa aktiviteetin A1 suoritetaan, kun ulkoiset tiedot ovat käytettävissä ja ajoituksen korkojakso on saavutettu.  Tehtävän A2 suoritetaan, kun ajoitetun sektorit D2-palvelupakettiisi ja ajoituksen korkojakso on saavutettu. Jos virheen tietojoukko D2 sektorit A2 ei voi käyttää sitä varten, kunnes se on käytettävissä.

Verkkokaavio-näkymän:

![Kaksi yhdistettyä putkijohtoa ketjutus toimintoja](./media/data-factory-create-pipelines/chaining-two-pipelines.png)

Verkkokaavio-näkymän kanssa sekä samaa putkijohtoa toimintoja: 

![Samaa putkijohtoa ketjutus toimintoja](./media/data-factory-create-pipelines/chaining-one-pipeline.png)

Lisätietoja on artikkelissa [ajoittaminen ja suorittamisen](#chaining-activities). 

## <a name="scheduling-and-execution"></a>Ajoittamisen ja suorittaminen
Tähän mennessä on ymmärretty, mitä putkistot ja toiminnot ovat. Sinun on myös tarkastellut kuinka ovat ne määritetyn ja joka ylätason näkymän Azure Data Factory-toimintoja. Anna meidän näyttää, miten ne suoriteta. 

Putkijohto on aktiivinen vain sen alkamisaika ja päättymisaika välillä. Se ei suoriteta ennen alkamisajan ja päättymisajan jälkeen. Jos putkijohto on keskeytetty, se ei suoriteta riippumatta sen alkamis- ja -aika. Saat putkijohto, jos haluat suorittaa se pitäisi ei keskeyttää. Itse asiassa ei putkijohto, joka saa suorittaa. On putkisto-toimintoja, jotka haluat suorittaa. Kuitenkin ne kiellä putkijohto yleinen kontekstissa. 

Katso [ajoitus ja suorittamisen](data-factory-scheduling-and-execution.md) ymmärtää Azure Data Factory ajoittaminen ja suorittamisen toiminta.

## <a name="create-pipelines"></a>Luo putkistot
Azure Data Factory tarjoaa erilaisia menetelmiä tekijän ja otetaan käyttöön putkistot (joka puolestaan sisältää vähintään yhden tehtäviä ei). 

### <a name="using-azure-portal"></a>Azure-portaalissa
Voit Data Factory-editorin Azure portaalin putkijohto luomiseen. Saat lopusta loppuun-ongelmatilanteita [Azure Data Factory (tietojen Factory Editor) käytön aloittaminen](data-factory-build-your-first-pipeline-using-editor.md) . 

### <a name="using-visual-studio"></a>Visual Studiossa 
Voit käyttää Visual Studio tekijän ja putkistot ottamisesta käyttöön Azure Data Factory. Saat lopusta loppuun-ongelmatilanteita [Azure Data Factory (Visual Studio) käytön aloittaminen](data-factory-build-your-first-pipeline-using-vs.md) . 

### <a name="using-azure-powershell"></a>Azure PowerShellin avulla
Voit luoda putkistot Azure Data Factory PowerShellin Azure. Yammer-myyntijakso JSON on määritelty c:\DPWikisample.json tiedosto. Voit ladata sen Azure Data Factory-esiintymässä, kuten seuraavassa esimerkissä:

    New-AzureRmDataFactoryPipeline -ResourceGroupName ADF -Name DPWikisample -DataFactoryName wikiADF -File c:\DPWikisample.json

Saat lopusta loppuun-ongelmatilanteita luomiseen tietojen factory putkijohto [Azure Data Factory (PowerShellin Azure) käytön aloittaminen](data-factory-build-your-first-pipeline-using-powershell.md) . 

### <a name="using-net-sdk"></a>Käyttämällä .NET SDK-paketissa
Voit luoda ja ottaa myyntijakso kautta .NET SDK liian. Se voidaan luoda putkistot ohjelmallisesti. Lisätietoja on artikkelissa [luominen, hallinta- ja seurata tietoja tehtaan ohjelmallisesti](data-factory-create-data-factories-programmatically.md). 


### <a name="using-azure-resource-manager-template"></a>Azure Resurssienhallinta-mallin avulla
Voit luoda ja ottaa käyttöön myyntijakso Azure Resurssienhallinta-mallin avulla. Lisätietoja on artikkelissa [Azure Data Factory (Azure Resurssienhallinta) käytön aloittaminen](data-factory-build-your-first-pipeline-using-arm.md). 

### <a name="using-rest-api"></a>REST-Ohjelmointirajapinnan käyttäminen
Voit luoda ja ottaa myyntijakso käyttämällä REST API liian. Se voidaan luoda putkistot ohjelmallisesti. Lisätietoja on kohdassa [Luo tai päivitä putkijohto](https://msdn.microsoft.com/library/azure/dn906741.aspx). 


## <a name="monitor-and-manage-pipelines"></a>Seurata ja hallita putkistot  
Kun putkijohto on otettu käyttöön, voit hallita ja seurata putkistot, sektoreiden ja suoritetaan. Lue lisää se tässä: [näyttö ja hallita putkistot](data-factory-monitor-manage-pipelines.md).


## <a name="pipeline-json"></a>Myyntijakso JSON   
Anna meidän Tutustu tarkemmin, miten putkijohto on määritetty JSON-muodossa. Yleinen rakenne putkijohto näyttää seuraavasti:

    {
        "name": "PipelineName",
        "properties": 
        {
            "description" : "pipeline description",
            "activities":
            [
    
            ],
            "start": "<start date-time>",
            "end": "<end date-time>"
        }
    }

**Toiminnot** -osassa voi olla vähintään yksi määritetyssä se toimintoja. Tehtävätyypin seuraavat ylimmän tason rakenne on seuraava:

    {
        "name": "ActivityName",
        "description": "description", 
        "type": "<ActivityType>",
        "inputs":  "[]",
        "outputs":  "[]",
        "linkedServiceName": "MyLinkedService",
        "typeProperties":
        {
    
        },
        "policy":
        {
        }
        "scheduler":
        {
        }
    }

Seuraavat taulukossa kuvataan koskevaa toimintaa ja myyntijakso JSON määritelmien ominaisuuksia:

Tunniste | Kuvaus | Pakollinen
--- | ----------- | --------
Nimi | Tehtävän tai putkijohto nimi. Määritä nimi, joka edustaa toimintoa että myyntijakso tai tehtävän on määritetty tehtävä<br/><ul><li>Merkkien enimmäismäärä: 260</li><li>On alettava kirjain luvun tai alaviivaa (_)</li><li>Seuraavat merkit eivät ole sallittuja: ".", "+", "?", "/", "<",">", "*", "%", "&", ":","\\"</li></ul> | Kyllä
kuvaus | Kuvausteksti myyntijakso tai tehtävän käyttötarkoitus | Kyllä
tyyppi | Määrittää tehtävän tyyppi. Erilaisia toimintoja [Tietojen siirtämistä toimintojen](data-factory-data-movement-activities.md) ja [tietojen muunnos](data-factory-data-transformation-activities.md) -artikkeleissa. | Kyllä
syötteiden | Syötteen käyttämän tehtävän taulukot<br/><br/>yhden syötteen taulukko<br/>"syötteiden": [{"nimi": "inputtable1"}],<br/><br/>syötteen taulukot <br/>"syötteiden": [{"nimi": "inputtable1"}, {"nimi": "inputtable2"}], | Kyllä
tulostus | Tulosteen taulukoiden activity.// yksi tulos-taulukko<br/>"tulostaa": [{"nimi": "outputtable1"}],<br/><br/>tulostus-taulukot<br/>"tulostaa": [{"nimi": "outputtable1"}, {"nimi": "outputtable2"}], | Kyllä
linkedServiceName | Linkitetyn palvelun käyttämän tehtävän nimi. <br/><br/>Tehtävän voi vaatia määrittää linkitetyn-palvelun, joka toimii linkkinä tarvittavat Laske-ympäristössä. | Kyllä HDInsight-toimintojen ja Azure Konepohjaisten Oppimistekniikoiden erä näkyvissä tehtävän pistemäärä <br/><br/>Kaikki muut ei
typeProperties | TypeProperties-osan ominaisuudet riippuvat tehtävän tyyppi. | Ei
käytäntö | Käytäntöjä, jotka vaikuttavat tehtävän suorituksenaikainen toimintaa. Jos sitä ei ole määritetty, käytetään oletuskäytäntöjä. | Ei
aloittaminen | Käynnistä päivämäärän ja kellonajan putkijohto varten. On oltava [ISO-muodossa](http://en.wikipedia.org/wiki/ISO_8601). Esimerkki: 2014-10-14T16:32:41Z. <br/><br/>On mahdollista määrittää paikallista aikaa, kuten EST-aika. Esimerkki: "2016-02-27T06:00:00**-05: 00**", joka on 6 AM arvioitu<br/><br/>Aloitus- ja ominaisuudet Määritä yhdessä putkijohto aktiivista kautta. Tulosteen sektoreiden on valmistettu vain kanssa aktiivinen tänä aikana. | Ei<br/><br/>Jos määrität end-ominaisuuden arvon, Käynnistä-ominaisuuden arvo on määritettävä.<br/><br/>Alkamis- ja päättymisajat sekä voidaan tyhjä putkijohto luomiseen. Molemmat-arvojen aktiivisen kauden, suorita myyntijakso varten on määritettävä. Jos et määritä alkamis- ja päättymisajat luodessasi putkijohto, voit määrittää ne määrittäminen AzureRmDataFactoryPipelineActivePeriod cmdlet-komennon avulla myöhemmin.
Lopeta | Lopeta päivämäärän ja kellonajan putkijohto varten. Jos määritetty on oltava ISO-muodossa. Esimerkki: 2014-10-14T17:32:41Z <br/><br/>On mahdollista määrittää paikallista aikaa, kuten EST-aika. Esimerkki: "2016-02-27T06:00:00**-05: 00**", joka on 6 AM arvioitu<br/><br/>Suorita putkijohto jatkuvasti, Määritä 9999-09-09 end-ominaisuuden arvon. | Ei <br/><br/>Jos määrität alkamis-ominaisuuden arvon, end-ominaisuuden arvo on määritettävä.<br/><br/>Katso **Käynnistä** -ominaisuuden huomautuksia.
isPaused | Jos arvo on tosi putkijohto ei suoriteta. Oletusarvo = false. Tämän ominaisuuden avulla voit ottaa käyttöön tai poistaa käytöstä. | Ei 
ajoitus | "ajoitus-ominaisuus on käytössä, voit määrittää haluamasi tehtävän ajoitusta. Sen alakokoelmista ovat samat kuin [tietojoukko käytettävyys-ominaisuus](data-factory-create-datasets.md#Availability). | Ei |   
| pipelineMode | Ajoitustapa, jota käytetään suoritetaan putkijohto. Sallitut arvot ovat: suunniteltu (oletus) etsii kerran.<br/><br/>Ajoitettu"ilmaisee, että putkijohto suoritetaan määritetyn aikavälin mukaan aktiivisen kauden (alkamis- ja -aika). "Etsii kerran" ilmaisee, että putkijohto suoritetaan vain kerran. Kun luonut etsii kerran putkistot ei voi muokata tai päivittää tällä hetkellä. Lisätietoja [Onetime myyntijakso](data-factory-scheduling-and-execution.md#onetime-pipeline) etsii kerran määrittämisestä. | Ei | 
| expirationTime | Ajaksi, jolle putkijohto on voimassa ja pitää valmistellun luonnin jälkeen. Jos se ei ole missään aktiivinen epäonnistui, tai on suoritettu, kunnes putkijohto poistetaan automaattisesti kerran saavuttaa päättymisaika. | Ei | 
| tietojoukkoja | Käytettävän toimintoja, jotka on määritelty putkijohto tietojoukkoja luettelo. Tämän ominaisuuden avulla voidaan määrittää tietojoukkoja, jotka ovat tämän putkijohto ja joita ei ole määritetty tietojen factory. Tämä myyntijakso määritetyssä tietojoukkoja voidaan käyttää vain tässä putkijohto ja ei voi jakaa. Katso lisätietoja [Suodatetut tietojoukkoja](data-factory-create-datasets.md#scoped-datasets) .| Ei |  
 

### <a name="policies"></a>Käytännöt
Käytännöt vaikuttavat aktiviteettiin, suorituksenaikainen toimintaa, erityisesti silloin, kun taulukon sektoria käsitellään. Seuraavassa taulukossa on tietoja.

Ominaisuus | Sallitut arvot | Oletusarvo | Kuvaus
-------- | ----------- | -------------- | ---------------
samanaikainen | Kokonaisluku <br/><br/>Suurin arvo: 10 | 1 | Samanaikainen suorituskerran tehtävän määrä.<br/><br/>Se määrittää rinnakkainen tehtävä suorituskerran, jotka liittyvät voidaan tehdä eri sektoreiden määrän. Esimerkiksi jos tehtävä on käsitellään käytettävissä olevat tiedot, samanaikainen suurempi arvo on suuri joukko nopeuttaa tietojen käsittelyyn. 
executionPriorityOrder | NewestFirst<br/><br/>OldestFirst | OldestFirst | Määrittää tietojen sektoreiden, käsitellään järjestystä.<br/><br/>Esimerkiksi jos sinulla on 2 sektoreiden (yksi asetuksen kello 4 ja 5 kello toinen) ja molemmat odottavia suorittamisen. Jos määrität executionPriorityOrder on NewestFirst, kello 5 sektoria käsitellään ensin. Lisää vastaavasti Jos määrität executionPriorityORder on OldestFIrst, sitten sektoria kello 4 käsitellään. 
Yritä uudelleen | Kokonaisluku<br/><br/>Suurin arvo voi olla 10 | 3 | Ennen kuin sektoria Tiedonkäsittelyn merkitään virheen uudelleenyritykset määrä. Tehtävän tiedot-sektori suorittamisen yritetään ylöspäin määritettyyn uudelleenyritysmäärä. Mahdollisimman pian virheen jälkeen on valmis yritä uudelleen.
aikakatkaisu | Aikajakson | 00:00:00 | Tehtävän aikakatkaisu. Esimerkki: 00:10:00 (sisältää aikakatkaisu 10 minuutin)<br/><br/>Jos arvo ei ole määritetty tai on 0, aikakatkaisun on ääretön.<br/><br/>Jos tietojen käsittelyaika-asetus on sektori ylittää aikakatkaisun, se on peruutettu ja järjestelmä yrittää uudelleen käsittely. Uudelleenyritykset määrä riippuu siitä, yritä-ominaisuutta. Aikakatkaisu tapahtuu, kun tila on määritetty aikakatkaisu.
viive | Aikajakson | 00:00:00 | Määritä viive ennen kuin tietojen käsittely sektoria käynnistyy.<br/><br/>Tehtävän tiedot-sektori suorittamisen käynnistetään viive on jo mennyt odotettu suoritusaika.<br/><br/>Esimerkki: 00:10:00 (sisältää 10 minuutin viive)
longRetry | Kokonaisluku<br/><br/>Suurin arvo: 10 | 1 | Pitkä uudelleenyritykset ennen sektoria suoritusvirhe määrä.<br/><br/>longRetry yritykset ovat peräkkäin longRetryInterval mukaan. Jos haluat määrittää uudelleenyritykset välisen ajan, käytä niin longRetry. Jos uudelleen ja longRetry on määritetty, longRetry jokaisen yrityksen sisältää uudelleenyritykset ja yritykset enimmäismäärä on uudelleen * longRetry.<br/><br/>Esimerkki: tehtävän käytäntö on seuraavat asetukset:<br/>Yritä uudelleen: 3<br/>longRetry: 2<br/>longRetryInterval: 01:00:00<br/><br/>Oletetaan, että on vain yksi sektori suorittaminen (odottaa tila) ja tehtävän suorittaminen epäonnistui, joka kerta. Alun perin olla 3 peräkkäiset suorittamisen yritykset. Jokaisen yrityksen jälkeen sektoria tila olisi uudelleen. Kun 3 ensimmäistä yritykset ylittävät sektoria tila olisi LongRetry.<br/><br/>Tunti (eli longRetryInteval's arvo), kun voidaan eri 3 peräkkäiset suorittamisen yritykset. Tämän jälkeen sektoria tila epäonnistui ja lisää uudelleenyritykset ei voidaan yrittää. Näin ollen yleistä 6 yritykset tekemät muutokset.<br/><br/>Jos kaikki suorittamisen onnistuu, sektoria tila on valmis ja ei lisää uudelleenyritykset kokeillaan.<br/><br/>longRetry voidaan käyttää tilanteissa, joissa riippuvaiset tietoja saapuu-deterministic aikoja tai yleinen-ympäristö on kohdassa mitä tietojenkäsittely ilmenee flaky. Tässä tapauksessa uudelleenyritykset peräkkäin saattaa auta harjoittelu ja tällöin ajan kuluttua aikaa haluttu tulos tulokset.<br/><br/>Varoitus Word: ei määritetä ylimmät arvot longRetry tai longRetryInterval. Yleensä suurempia arvoja ilmentävät systeeminen muita ongelmia. 
longRetryInterval | Aikajakson | 00:00:00 | Uudelleenyritykset pitkä viive 


## <a name="next-steps"></a>Seuraavat vaiheet

- Lisätietoja [ajoitus sekä Azure Data Factory suoritus](data-factory-scheduling-and-execution.md).  
- Lue lisää [tietojen siirto](data-factory-data-movement-activities.md) ja Azure Data Factory- [muunnos ominaisuudet](data-factory-data-transformation-activities.md)
- Tietoja [hallinta ja Azure Data Factory seuranta](data-factory-monitor-manage-pipelines.md).
- [Muodosta ja ota käyttöön luettelon liitetietoihin myyntijakso](data-factory-build-your-first-pipeline.md). 
