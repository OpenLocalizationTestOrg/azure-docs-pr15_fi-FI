<properties
    pageTitle="Vaatimus energiajärjestelmien tekninen opas ennuste | Microsoft Azure"
    description="Ennuste energiajärjestelmien demand tekninen opas kanssa Microsoft Cortana liiketoimintatietojen ratkaisu-malliin."
    services="cortana-analytics"
    documentationCenter=""
    authors="yijichen"
    manager="ilanr9"
    editor="yijichen"/>

<tags
    ms.service="cortana-analytics"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/16/2016"
    ms.author="inqiu;yijichen;ilanr9"/>

# <a name="technical-guide-to-the-cortana-intelligence-solution-template-for-demand-forecast-in-energy"></a>Tekninen opas ennuste energiajärjestelmien demand Cortana liiketoimintatietojen ratkaisu-malliin

## <a name="overview"></a>**Yleiskatsaus**

Ratkaisu malleja on suunniteltu nopeuttamaan luomista E2E-esittely Cortana liiketoimintatietojen Suite päälle. Käyttöön mallin valmistella tilauksen tarvittavat Cortana Liiketoimintatieto-osan kanssa ja luoda väliset suhteet. Se myös alustaa tietojen putkijohto esimerkkitiedoilla käytön luotu simuloinnissa sovelluksesta. Lataa tiedot simulator annetusta linkistä ja asentaa sen paikallisessa tietokoneessa, katso käyttämisestä on Simulator ‑sovellus readme.txt-tiedostossa. Luotu simulator tiedot hydrate tietojen putkijohto ja Käynnistä luodaan tietokoneen learning tekstinsyöttö joka sitten voidaan näyttää Power BI-koontinäytössä.

Ratkaisu-mallin löytyy [tähän](https://gallery.cortanaintelligence.com/SolutionTemplate/Demand-Forecasting-for-Energy-1) 

Käyttöönottoprosessin opastaa sinua useita toimenpiteellä ratkaisu tunnistetiedot. Varmista, että voit tallentaa näitä tunnistetietoja, kuten ratkaisun nimi, käyttäjänimi ja salasana käyttöönoton aikana.

Tämän asiakirjan lähinnä kertoaksesi viittaus arkkitehtuuri ja eri osien valmisteltu tilauksen osana ratkaisu tätä mallia. Asiakirjan puheen myös tietoja mallitietojen korvaaminen todellisten tietojen yleislähetyksenä voi tarkastella tietoja/ennusteiden yhteydessä voitettu tiedot. Lisäksi asiakirjan käytön ratkaisumalli, jota voi muokata, jos haluat mukauttaa omiin tietoihisi ratkaisu on osat. Ohjeita Power BI-koontinäytön ratkaisu-malli on tarkoitettu lopussa.

## <a name="big-picture"></a>**Ylemmän tason**

![](media\cortana-analytics-technical-guide-demand-forecast\ca-topologies-energy-forecasting.png)

### <a name="architecture-explained"></a>Arkkitehtuuri kuvaus
Kun ratkaisu on otettu käyttöön, eri Cortana Analytics Suite Azure palvelut otetaan käyttöön (*eli* tapahtumaa-toiminnossa Stream Analytics, HDInsight, Data Factory, tietokoneen Learning, *jne.*). Yllä olevassa arkkitehtuuri kaaviossa näkyy ylätasolla, miten Demand ennusteiden energiajärjestelmien ratkaisu mallin muodostetaan lopusta loppuun. Sinulla voi tutkia palveluista napsauttamalla niitä ratkaisu mallin kaavio luodaan ratkaisun käyttöönotto. Seuraavissa kohdissa kuvataan kutakin.

## <a name="data-source-and-ingestion"></a>**Tietolähteen ja nieltynä**

### <a name="synthetic-data-source"></a>Synteettiset tietolähde

Tämän mallin tietolähteen johdetaan työpöytäsovelluksessa, joka lataa ja suorita paikallisesti onnistuneen käyttöönoton jälkeen. Etsii ohjeita voit ladata ja asentaa tämän sovelluksen ominaisuudet-palkissa, kun valitset ensimmäinen solmu nimeltä energiajärjestelmien ennusteiden tietojen Simulator ratkaisu mallin kaavio. Tämän sovelluksen syötteet arvopisteiden tai tapahtumat, joita käytetään ratkaisu kulun loput [Azure tapahtumaa-toiminnossa](#azure-event-hub) -palvelun.

Tapahtuman luominen sovellus lisää Azure tapahtumaa-toiminnossa vain silloin, kun se suoritetaan tietokoneessa.

### <a name="azure-event-hub"></a>Azure tapahtumaa-toiminnossa

[Azure tapahtumaa-toiminnossa](https://azure.microsoft.com/services/event-hubs/) -palvelu on myöntämä synteettistä tietolähteen yllä olevien ohjeiden syötteen vastaanottaja.

## <a name="data-preparation-and-analysis"></a>**Tietojen valmisteleminen ja analysointia varten**


### <a name="azure-stream-analytics"></a>Azure Stream Analytics

[Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) -palvelua käytetään lähes reaaliajassa analytics-syötteen virta [Azure tapahtumaa-toiminnossa](#azure-event-hub) -palvelusta ja julkaista tulokset sivulle [Power BI](https://powerbi.microsoft.com) -koontinäytön sekä arkistointi kaikki raaka saapuvien tapahtumien [Azure](https://azure.microsoft.com/services/storage/) tallennuspalvelu [Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/) -palvelun myöhempää käsittelyä varten.

### <a name="hd-insights-custom-aggregation"></a>HD havainnollistamisen mukautetun kooste

Azure HD tietoja palvelua käytetään (koordinoitu mukaan Azure Data Factory) [rakenne](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) komentosarjojen suorittaminen antamaan koosteet raaka tapahtumista, jotka on arkistoida Azure Stream Analytics-palvelun avulla.

### <a name="azure-machine-learning"></a>Azure koneen oppiminen

[Azure koneen Learning](https://azure.microsoft.com/services/machine-learning/) -palvelua käytetään (koordinoitu mukaan Azure Data Factory) voit tehdä ennusteen tulevien virrankulutuksen annettu vastaanotettu syötteiden tietyn alueen.

## <a name="data-publishing"></a>**Tietojen julkaiseminen**


### <a name="azure-sql-database-service"></a>Azure SQL-tietokanta-palvelu

[Azure SQL-tietokanta](https://azure.microsoft.com/services/sql-database/) -palvelua käytetään tallentamiseen (hallitsee Azure Data Factory) Azure koneen Learning palvelu, jota käytetty [Power BI](https://powerbi.microsoft.com) -koontinäytön vastaanottanut ennusteiden.

## <a name="data-consumption"></a>**Tietoja kulutus**

### <a name="power-bi"></a>Power BI

[Power BI](https://powerbi.microsoft.com) -palvelua käytetään näyttämään Raporttinäkymät-ikkunan, joka sisältää koosteet edellyttäen mukaan [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) palvelun sekä tarvittaessa ennuste tulokset tallennettu [Azure SQL](https://azure.microsoft.com/services/sql-database/) -tietokantaan, joka on valmistettu [Azure koneen Learning](https://azure.microsoft.com/services/machine-learning/) -palvelun avulla. Ohjeita Power BI-koontinäytön ratkaisu-malli on lisätietoja jäljempänä olevaan kohtaan.

## <a name="how-to-bring-in-your-own-data"></a>**Voit tuoda Omat tietoja**

Tässä osassa kuvataan, miten tuo omiin tietoihisi Azure ja mitä alueita edellyttäisi muutokset tässä arkkitehtuuri tuo tiedot.

On epätodennäköistä, että kaikki tietojoukko voit noutaa vastaa tietojoukko ratkaisu tätä mallia käytetään. Tietoja tietojen ja vaatimukset ovat tärkeitä-siitä, miten voit muokata tätä mallia toimimaan omiin tietoihisi. Jos tämä on ensimmäinen näkyvyyttäsi Azure koneen Learning-palveluun, saat sen johdanto käyttämällä esimerkin [ensimmäinen koe luomisesta](machine-learning\machine-learning-create-experiment.md).

Seuraavissa osissa käsitellään osat, jotka edellyttävät muutokset, kun uusi tietojoukko on otettu käyttöön mallin.

### <a name="azure-event-hub"></a>Azure tapahtumaa-toiminnossa

[Azure tapahtumaa-toiminnossa](https://azure.microsoft.com/services/event-hubs/) -palvelu on yleinen-siten, että tiedot voit kirjata keskittimeen CSV- tai JSON-muodossa. Ei erityistä käsittely tapahtuu Azure tapahtumaa-toiminnossa, mutta on tärkeää ymmärtää tiedot, jotka on syötetään.

Tässä asiakirjassa ei kerrotaan, kuinka voit ingest tietoja, mutta jokin voi lähettää helposti tapahtumia tai tietojen Azure tapahtuma-keskittimeen, [Tapahtuma-toiminnossa Ohjelmointirajapinnan](event-hubs\event-hubs-programming-guide.md)käyttäminen.

### <a name="azure-stream-analytics"></a>Azure Stream Analytics

[Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) -palvelua käytetään antamaan lähellä reaaliaikainen analytics tietojen virtaa luettaessa ja määritetty jokin muu luku lähteiden tiedot.

Ennustamiseen Demand energiajärjestelmien ratkaisu mallin Azure Stream Analytics-kyselyn koostuu kahdesta alikyselyt, kunkin tapahtumia syötteiden Azure tapahtumaa-toiminnossa-palvelusta muissa ja tulostaa tarvitse kahteen eri paikkaan. Nämä tulostaa koostua yhden Power BI-tietojoukko ja yksi Azure tallennuspaikka.

[Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) -kyselyn löytävät perusteella:

-   [Azure hallinta-portaalin](https://manage.windowsazure.com/) kirjaaminen

-   Etsiminen stream analytics työt ![](media\cortana-analytics-technical-guide-demand-forecast\icon-stream-analytics.png) , joka luotiin, kun ratkaisu on otettu käyttöön. Yksi on valitseminen tietojen blob storage (kuten mytest1streaming432822asablob) ja toinen on tarkoitettu tietojen valitseminen Power BI (kuten mytest1streaming432822asapbi).


-   Valitseminen

    -   Voit tarkastella kyselyn syöte ***SYÖTTEIDEN***

    -   Voit tarkastella itse kysely ***kysely***

    -   Jos haluat tarkastella eri tulostus ***TULOSTAA***

Lisätietoja Azure Stream Analytics kyselyn rakenteeseen löytyy MSDN-sivuston [Stream Analytics kyselyn viittaus](https://msdn.microsoft.com/library/azure/dn834998.aspx) .

Tämä ratkaisu Azure Stream Analytics-työ, joka siirtää tietojoukko, jolla on lähellä reaaliaikainen analytics tietoja saapuvien tietovirta Power BI-raporttinäkymän annetaan osana ratkaisu tätä mallia. Koska saapuvan tietomuoto implisiittinen tietosi, nämä kyselyt on muuteta mukaan tietojen muoto.

Azure Stream Analytics-työ tulostaa kaikki [Azuren tallennustilaan](https://azure.microsoft.com/services/storage/) [Tapahtumaa-toiminnossa](https://azure.microsoft.com/services/event-hubs/) tapahtumia ja vaatii näin ollen ei muutosta riippumatta tietojen muoto koko tapahtumatiedot on virtauttaa tallennustilan.

### <a name="azure-data-factory"></a>Azure Data Factory

[Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/) -palvelun orchestrates liikkuvuuden ja tietojen käsittely. Valitse tarvittaessa ennusteiden energiajärjestelmien ratkaisu mallin tiedot factory koostuu 12 [putkistot](data-factory\data-factory-create-pipelines.md) , siirtää ja käsitellä tietoja käyttämällä eri tekniikoita.

  Voit käyttää tietojen factory avaamalla Data Factory-solmu luotu ratkaisun käyttöönoton ratkaisun mallin kaavion alareunassa. Tällöin näyttöön tiedot factory Azure hallinta-portaalissa. Jos näyttöön tulee virhe kohdassa oman tietojoukkoja, voit ohittaa ne sellaisina kuin ne ovat tietojen factory otetaan käyttöön ennen tietojen luontitoiminnon aloitettiin vuoksi. Kyseiset virheet eivät estä tietojen factory toimimasta.

Tässä osassa käsitellään tarvittavat [putkistot](data-factory\data-factory-create-pipelines.md) ja [Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/)sisältyvät [tehtävät](data-factory\data-factory-create-pipelines.md) . Alla on ratkaisun kaavionäkymä.

![](media\cortana-analytics-technical-guide-demand-forecast\ADF2.png)

Viisi Tämä tehdas putkistot sisältää [rakenne](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) komentosarjoja, joita käytetään osio ja koota tietoja. Kun on kerrottu, komentosarjat sijaitsevat [Azure-tallennustilan](https://azure.microsoft.com/services/storage/) tilin luotu asennuksen aikana. Niiden sijainti on: demandforecasting\\\\komentosarjan\\\\rakenteen\\ \\ (tai https://[Your ratkaisu name].blob.core.windows.net/demandforecasting).

Samalla [Azure Stream Analytics](#azure-stream-analytics-1) -kyselyt, [rakenne](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) -komentosarjat on saapuvan tietomuoto implisiittinen tietosi, nämä kyselyt on muutettava muoto ja [ominaisuus tekniikka](machine-learning\machine-learning-feature-selection-and-engineering.md) tarpeen mukaan.

#### <a name="aggregatedemanddatato1hrpipeline"></a>*AggregateDemandDataTo1HrPipeline*

[Myyntijakso](data-factory\data-factory-create-pipelines.md) -myyntijakso sisältää yhden aktiviteetin - käyttämällä [HDInsightLinkedService](https://msdn.microsoft.com/library/azure/dn893526.aspx) , joka suoritetaan aina 10 sekunnin välein kootaan [rakenne](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) komentosarjan [HDInsightHive](data-factory\data-factory-hive-activity.md) tehtävän ala-asema tason kerran tunnissa alueen tasolle tietojen demand virtauttaa ja siirtää [Azuren](https://azure.microsoft.com/services/storage/) tallennustilaan Azure Stream Analytics-työn kautta.

[Rakenne](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) -komentosarja osioinnin tehtävän on ***AggregateDemandRegion1Hr.hql***


#### <a name="loadhistorydemanddatapipeline"></a>*LoadHistoryDemandDataPipeline*

Tämä [myyntijakso](data-factory\data-factory-create-pipelines.md) sisältää kaksi toimintoja:
- [HDInsightHive](data-factory\data-factory-hive-activity.md) toiminto, jossa käytetään [HDInsightLinkedService](https://msdn.microsoft.com/library/azure/dn893526.aspx) , joka suoritetaan rakenteen komentosarjan voit koostaa tunnin välein demand historiatiedot ala-asema tason kerran tunnissa alueen tasolle ja siirtää Azuren tallennustilaan Azure Stream Analytics-työn aikana

- [Kopioi](https://msdn.microsoft.com/library/azure/dn835035.aspx) -toiminto, joka siirtää koostetut tiedot Azuren tallennustilaan Blob-objektien Azure SQL-tietokantaan, joka on valmisteltu ratkaisu malli-asennuksen.

Tämän tehtävän [rakenne](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) -komentosarja on ***AggregateDemandHistoryRegion.hql***.


#### <a name="mlscoringregionxpipeline"></a>*MLScoringRegionXPipeline*

Nämä [putkistot](data-factory\data-factory-create-pipelines.md) sisältää useita tehtäviä ja joiden lopputulos on scored ennusteiden-ratkaisun malliin liitettyä Azure koneen Learning kokeen. He ovat lähes samanlaisia, mutta jokainen niistä vain käsittelee toisella alueella, joka tehdään eri RegionID välitetty SYÖTTÖ putkijohto ja rakenne-komentosarjan kunkin alueen.  
Tämä sisältyvät tehtävät ovat seuraavat:
-   [HDInsightHive](data-factory\data-factory-hive-activity.md) toiminto, jossa käytetään [HDInsightLinkedService](https://msdn.microsoft.com/library/azure/dn893526.aspx) , joka suoritetaan rakenteen komentosarjan suorittaminen koosteet ja ominaisuus tekniikka tarvittavat Azure koneen Learning kokeen. Rakenne-komentosarjoja tähän ovat vastaaviin ***PrepareMLInputRegionX.hql***.

-   [Kopioi](https://msdn.microsoft.com/library/azure/dn835035.aspx) -toiminto, joka siirtää tulokset yhteen Azuren tallennustilaan-blob, joka voi olla [AzureMLBatchScoring](https://msdn.microsoft.com/library/azure/dn894009.aspx) -toiminnon käytön [HDInsightHive](data-factory\data-factory-hive-activity.md) tehtävästä.

-   [AzureMLBatchScoring](https://msdn.microsoft.com/library/azure/dn894009.aspx) -toiminto, joka kutsuu Azure-koneen Learning kokeilla, mitä tulokset tulokset yhteen Azuren tallennustilaan käyttöön blob.

#### <a name="copyscoredresultregionxpipeline"></a>*CopyScoredResultRegionXPipeline*
Nämä [putkistot](data-factory\data-factory-create-pipelines.md) sisältävät yhden aktiviteetin - [kopio](https://msdn.microsoft.com/library/azure/dn835035.aspx) -toiminto, joka siirtää Azure koneen Learning kokeen tulokset vastaaviin ***MLScoringRegionXPipeline*** Azure SQL-tietokantaan, joka on valmisteltu ratkaisu malli-asennuksen.

#### <a name="copyaggdemandpipeline"></a>*CopyAggDemandPipeline*
Tämä [putkistot](data-factory\data-factory-create-pipelines.md) sisältävät yhden aktiviteetin - [kopio](https://msdn.microsoft.com/library/azure/dn835035.aspx) -toiminto, joka siirtää koostettu jatkuvaa tarvittaessa tietoja ***LoadHistoryDemandDataPipeline*** Azure SQL-tietokantaan, joka on valmisteltu ratkaisu malli-asennuksen.

#### <a name="copyregiondatapipeline-copysubstationdatapipeline-copytopologydatapipeline"></a>*CopyRegionDataPipeline, CopySubstationDataPipeline, CopyTopologyDataPipeline*
Nämä [putkistot](data-factory\data-factory-create-pipelines.md) sisältävät yhden aktiviteetin - [kopio](https://msdn.microsoft.com/library/azure/dn835035.aspx) -toiminto, joka siirtää viitetiedot, alue tai asema/Topologygeo, jotka on ladattu Azuren tallennustilaan Blob-objektien Azure SQL-tietokantaan, joka on valmisteltu ratkaisu mallin asennuksen ratkaisu malli-asennuksen.

### <a name="azure-machine-learning"></a>Azure koneen oppiminen
Ratkaisu tätä mallia käytetään [Azure koneen Learning](https://azure.microsoft.com/services/machine-learning/) kokeen on demand alueen tekstinsyöttö. Kokeen on käytetty tietojoukon ja näin ollen edellyttävät muuttaminen tai korvaaminen liittyvät tiedot, jotka tuodaan.

## <a name="monitor-progress"></a>**Näytön edistyminen**
Kun tietojen luontitoiminnon käynnistetään, putkijohto alkaa Hae hydratoitu ja ratkaisu eri osat Aloita kicking toiminnon seuraavat komennot antamien tietojen Factory kyselyjä. Voit valvoa putkijohto kahdella tavalla.

1. Tarkista tiedot Azure-Blob-objektien tallennustilaan.

    Yksi Stream Analytics-työ kirjoittaa raaka saapuvat tiedot Blob-objektien tallennustilaan. Jos valitset **Azure-Blob-säiliö** osalle, että näytössä voit onnistuneesti ratkaisun käyttöön ja valitse sitten **Avaa** oikeanpuoleisessa, se siirtää sinut [Azure hallinta-portaalin](https://portal.azure.com). Kerran, valitse **BLOB-objektit**. Seuraava ruutu tulee näkyviin säilöjen luettelo. Valitse **"energysadata"**. Seuraava ruutu tulee näkyviin **"demandongoing"** -kansioon. Rawdata-kansiossa, näet kansioiden nimet, kuten päivämäärä = 2016-01 – 28 jne. Jos näet nämä kansiot, se osoittaa, että perustietoja on onnistuneesti on luotu tietokoneeseen ja Blob-objektien tallennustilaan tallennettuja. Raportissa pitäisi näkyä tiedostot, joiden pitäisi olla rajallinen koot megatavuina kansiot.

2. Tarkista tiedot Azure SQL-tietokantaan.

    Putkijohto viimeinen vaihe on voi kirjoittaa tietoja (kuten ennusteiden koneen Learning) SQL-tietokantaan. Voit joutua odottamaan enintään 2 tuntia tiedot näkyvät SQL-tietokantaan. Palvelun [Azure hallinta-portaalissa](https://manage.windowsazure.com/)on yksi tapa seurata, kuinka paljon tietoja on käytettävissä SQL-tietokantaan. Etsi vasemmassa paneelissa SQL-TIETOKANTOJA![](media\cortana-analytics-technical-guide-demand-forecast\SQLicon2.png) napsauttamalla sitä. Valitse Etsi tietokannan (eli demo123456db) ja napsauttamalla sitä. Valitse seuraavalla sivulla **"Muodosta yhteys tietokantaan"** -kohdassa **"Suorita Transact-SQL-kyselyjä SQL-tietokannalle"**.

    Tässä kohdassa voit valita uusi kysely-ja kysely, kuinka monta riviä (esimerkiksi "Valitse määrä(*) DemandRealHourly kohteesta)" Kun tietokannan kasvaa, taulukon rivien määrä pitäisi suurentaa.)

3. Valitse Power BI-koontinäytön tiedot.

    Voit määrittää Power BI kuuma polku Raporttinäkymät-ikkunan seurannassa raaka saapuvat tiedot. Noudata käsky "Power BI-koontinäytön"-osassa.



## <a name="power-bi-dashboard"></a>**Power BI-koontinäyttö**

### <a name="overview"></a>Yleiskatsaus

Tässä osassa kuvataan, miten voit määrittää Power BI-raporttinäkymän visualisointi analytics Azure stream (kuuma polun) reaaliaikaiset tiedot sekä ennuste tulokset Azure konepohjaisten oppimistekniikoiden (kylmän polku).


### <a name="setup-hot-path-dashboard"></a>Kuuma polku Raporttinäkymät-ikkunan asetukset

Seuraavat vaiheet opastaa, kuinka haluat esittää reaaliaikaiset tiedot tulosteen Stream Analytics työt, jotka on luotu ratkaisun käyttöönoton yhteydessä. Seuraavien toimien edellyttää [Verkossa Power BI](http://www.powerbi.com/) -tili. Jos sinulla ei ole tiliä, voit [luoda](https://powerbi.microsoft.com/pricing).

1.  Lisää Power BI-tulostus Azure Stream Analytics (ASA).

    -  Noudata tarvitset [Azure Stream Analytics ja Power BI: reaaliaikainen näkyvyyden streaming data reaaliaikainen analytics-Raporttinäkymät-ikkunan](stream-analytics-power-bi-dashboard.md) Azure Stream Analytics työtäsi Power BI-koontinäytöksi tulosteen määrittäminen.

    - Etsi stream analytics työn [Azure hallinta-portaalin](https://manage.windowsazure.com). Projektin nimen tulisi olla: YourSolutionName + "streamingjob" + satunnaisen numeron + "asapbi" (eli demostreamingjob123456asapbi).

    - Lisää PowerBI-tuloste työlle ASA. Määritä **tunnus** **"PBIoutput"**. Määritä **Tietojoukon nimi** ja **taulukkonimi** **"EnergyStreamData"**. Kun olet lisännyt tulosteen, valitse **"Käynnistä-** sivun alareunassa aloittaa Stream Analytics työn. Hanki vahvistussanoman (*kuten*, "Käynnistetään stream analytics työn myteststreamingjob12345asablob onnistui").

2. [Power BI verkossa](http://www.powerbi.com) kirjautuminen

    -   Sinun pitäisi nähdä Power BI vasemmassa paneelissa näyttämisen uusi tietojoukko vasemman ruudun oma työtilan tietojoukkoja-kohdassa. Tämä on Azure Stream Analytics miten edellisessä vaiheessa streaming tiedot.

    -   Varmista, että ***Visualisoinnit*** -ruutu on auki, joka näkyy näytön oikeassa reunassa.

3. Luo "Demand mukaan aikaleima"-ruutu:
    -   Valitse vasemman ruudun tietojoukkoja osan **'EnergyStreamData'** tietojoukko.

    -   **"Viivakaavio"** kuvaketta ![](media\cortana-analytics-technical-guide-demand-forecast\PowerBIpic8.png).

    -   Valitse **kentät** -ruudussa "EnergyStreamData".

    -   Valitse **"Aikaleima"** ja varmista, että se näkyy "akseli-kohdassa. Valitse **"Kuormituksen"** ja varmista, että se näkyy "arvot-kohdassa.

    -   Valitse **Tallenna** ylä- ja nimi "EnergyStreamDataReport" valmiiksi. Raportin nimeltä "EnergyStreamDataReport" näkyy vasemmassa ruudussa raportit-osioon.

    -   Valitse **"PIN-tunnuksen visuaalinen"** ![](media\cortana-analytics-technical-guide-demand-forecast\PowerBIpic6.png) viivakaaviossa oikeassa yläkulmassa-kuvaketta, "Pin-Raporttinäkymät-ikkunan ehkä Näytä sopivat Raporttinäkymät-ikkunan. Valitse "EnergyStreamDataReport" ja valitse "PIN-tunnuksen".

    -   Pidä hiirtä koontinäytössä tämän ruudun päälle valitsemalla "Muokkaa" Voit muuttaa sen otsikko kuin "Demand mukaan aikaleima" oikean yläkulman-kuvake

4.  Luo muita Raporttinäkymät-ikkunan ruudut, tarvittavat tietojoukkoja perusteella. Lopullinen dashboard-näkymä näkyy alla.
        ![](media\cortana-analytics-technical-guide-demand-forecast\PBIFullScreen.png)


### <a name="setup-cold-path-dashboard"></a>Kylmän polku Raporttinäkymät-ikkunan asetukset
Olennaiset tavoitteena on kylmän polku tietojen putkijohto, saat tarvittaessa ennuste kunkin alueen. Power BI muodostaa Azure SQL-tietokantaan sen tietolähteeksi tekstinsyöttö tulokset tallennuspaikka.

> [AZURE.NOTE] 1) kestää muutaman tunnin kerätä koontinäyttö tarpeeksi ennusteen tulokset. Suosittelemme, että aloitat prosessin 2 – 3 tuntia, kun tietojen luontitoiminnon lounaalla. 2) tässä vaiheessa edellytyksenä on ladattava ja asennettava maksuton ohjelmisto [Power BI Desktopin](https://powerbi.microsoft.com/desktop).



1.  Pyydä tietokannan tunnistetietoja.

    Sinun on **Tietokantapalvelimen nimi, tietokannan nimi, käyttäjänimi ja salasana** ennen siirtymistä vaiheisiin. Näin löydät, voit hakea muistiinpanoja.

    -   Kun **"Azure SQL-tietokanta-** ratkaisun mallin kaaviossa muuttuu vihreä, napsauta sitä ja valitse sitten **"Avaa"**. Sinun kehotetaan Azure hallinta-portaalin ja tietokannan tiedot-sivu avautuu myös.

    -   Etsi-sivulla "Tietokanta"-osan. Se näyttää, olet luonut tietokannan. Tietokannan nimen on oltava **"Oman ratkaisun nimeä SATUNNAISLUKU +"db""** (esimerkiksi "mytest12345db").

    -   Valitse tietokannan pannel uusi ponnahdusikkuna, voit etsiä tietokantapalvelimen nimi päällimmäisenä. Tietokannan palvelimen nimi nimi vapaille on **"Oman ratkaisun nimeä SATUNNAISLUKU +"database.windows.net,1433""** (esimerkiksi "mytest12345.database.windows.net,1433").

    -   Tietokannan **käyttäjänimi** ja **salasana** on sama kuin käyttäjänimi ja salasana on tallennettu aiemmin aikana-ratkaisun käyttöönotto.

2.  Päivitä tietolähteen kylmän polku Power BI-tiedoston
    -  Varmista, että olet asentanut uusimman version [Power BI Desktopin](https://powerbi.microsoft.com/desktop).

    -   **"DemandForecastingDataGeneratorv1.0"** -kansioon, olet ladannut Kaksoisnapsauta **' Power BI-Template\DemandForecastPowerBI.pbix'** -tiedosto. Alkuperäinen visualisoinnit ovat tyhjä tietojen perusteella. **Huomautus:** Jos saat virheilmoituksen massage, varmista, että olet asentanut uusimman Power BI Desktopin.

        Kun avaat sen ylälaidassa tiedoston, valitse **Muokkaa kyselyt**. -Ponnahdusikkuna-ikkuna kaksoisnapsauttamalla **'Lähde'** oikeassa paneelissa.
    ![](media\cortana-analytics-technical-guide-demand-forecast\PowerBIpic1.png)

    -   Ponnahdusikkuna-ikkunassa, valitse Korvaa **"Palvelin"** ja **"Tietokanta"** Omat palvelimen ja tietokannan nimet ja valitse sitten **"OK"**. Palvelimen nimi Varmista, että määrität porttia 1433 (**YourSolutionName.database.windows.net, 1433**). Ohita varoitus viestit, jotka näkyvät näytössä.

    -   Ikkunassa Seuraava ponnahdusikkuna näet kaksi asetusta, valitse vasemman ruudun (**Windows** ja **tietokannan**). Valitse **"Tietokanta"**, **"Käyttäjänimi"** ja **"Salasana"** (tämä on käyttäjänimi ja salasana kirjoittamasi ensin käyttöön ratkaisun ja luotu Azure SQL-tietokantaan) täytä. ***Valitse mitä tasoa, voit käyttää näitä asetuksia***Tarkista tietokannan tason vaihtoehtoa. Valitse **"Yhdistä"**.

    -   Kun olet ohjatun takaisin edelliselle sivulle, sulje ikkuna. Viestin ponnahtaa ulos - valitsemalla **Käytä**. Valitse lopuksi **Tallenna** -painiketta, jos haluat tallentaa muutokset. Power BI-tiedosto on nyt muodostanut yhteyden palvelimeen. Jos visualisointeihin ovat tyhjiä, varmista, että Poista valinnat visualisointien kaikkien tietojen visualisointi napsauttamalla oikeassa yläkulmassa selitteitä Pyyhekumi-kuvaketta. Päivitä-painike vastaamaan uusia tietoja visualisointien avulla Aluksi vain näet lähde-tiedot visualisointeihin, kun tietojen factory on ajoitettu päivitys 3 tunnin välein. 3 tunnin jälkeen näkyviin tulee uusi ennusteiden visualisointeihin näkyvät, kun päivität tiedot.

3. (Valinnainen) Julkaise [Verkossa Power BI](http://www.powerbi.com/)kylmän polku Raporttinäkymät-ikkunan. Huomaa, että tämä vaihe on Power BI tili (tai Office 365-tili).

    -   Valitse **"Julkaise,"** ja myöhemmin hetkinen ikkunan näkyy näyttäminen "Julkaiseminen Power BI-Success!" Vihreä valintamerkki. Valitse "Power BI Avaa demoprediction.pbix" alla olevaa linkkiä. Lisätietoja on artikkelissa [Power BI Desktop Julkaise](https://support.powerbi.com/knowledgebase/articles/461278-publish-from-power-bi-desktop).

    -   Luo uusi raporttinäkymät-ikkuna: Valitse **+** vieressä vasemmanpuoleisessa ruudussa **raporttinäkymät** -osa-merkkiä. Kirjoita nimi "Demand ennusteiden esittely" uusi raporttinäkymät-ikkuna.

    -   Kun avaat raportin, valitse ![](media\cortana-analytics-technical-guide-demand-forecast\PowerBIpic6.png) kiinnittäminen koontinäyttöön visualisointeja. Lisätietoja on artikkelissa [PIN-tunnuksen Power BI-koontinäytön raportista ruutu](https://support.powerbi.com/knowledgebase/articles/430323-pin-a-tile-to-a-power-bi-dashboard-from-a-report).
        Siirry raporttinäkymä-sivulle ja koon ja sijainnin visualisointeihin ja muokkaa niiden otsikoita. Lisätietoja siitä, miten voit muokata ruutujen lisätietoja [Muokkaa ruudun--kokoa, siirrä, nimeä uudelleen, PIN-koodi, poista, Lisää hyperlinkki](https://powerbi.microsoft.com/documentation/powerbi-service-edit-a-tile-in-a-dashboard/#rename). Tässä on esimerkki-koontinäytön joitakin kylmän polku-visualisointien kiinnitetyt siihen.

        ![](media\cortana-analytics-technical-guide-demand-forecast\PowerBIpic7.png)

4. (Valinnainen) Tietolähteen tietojen päivityksen ajoittaminen.
    -     Tietojen päivityksen ajoittaminen, Vie hiiren **EnergyBPI lopullinen** tietojoukko päälle, napsauta ![](media\cortana-analytics-technical-guide-demand-forecast\PowerBIpic3.png) ja valitse sitten **Ajoita päivitys**.
    **Huomautus:** Jos näet varoituksen hieronta, valitse **Muokkaa tunnistetiedot** ja varmista, että tietokannan tunnistetiedot ovat samat kuin vaiheen 1.

    ![](media\cortana-analytics-technical-guide-demand-forecast\PowerBIpic4.png)

    -   Laajenna osa **Ajoita päivitys** . Ota käyttöön "tietojen pitäminen ajan tasalla".

    -   Käyttötarkoitukseen päivityksen ajoittaminen. Lisätietoja on artikkelissa [tietojen päivittäminen Power BI](https://powerbi.microsoft.com/documentation/powerbi-refresh-data/).


## <a name="how-to-delete-your-solution"></a>**Voit poistaa ratkaisu**
Varmista, että lopetat tietojen luontitoiminnon käyttämällä ratkaisun aktiivisesti käytössä tietojen luontitoiminnon selvittää kustannuksia. Poista ratkaisu, jos et käytä sitä. Ratkaisu poistetaan kaikki komponentit, jotka on valmisteltu tilaukseesi, kun ratkaisun käyttöön. Voit poistaa ratkaisu valitsemalla ratkaisun nimeäsi vasemmassa ruudussa ratkaisumalli ja valitse Poista.

## <a name="cost-estimation-tools"></a>**Kustannusten arviointi Työkalut**

Seuraavat kaksi työkalut ovat auttavat ymmärtämään yhdistävää käynnissä Demand ennusteiden energiajärjestelmien ratkaisu mallin tilaukseesi kokonaiskustannukset:

-   [Microsoft Azure kustannusten arviointi työkalu (online)](https://azure.microsoft.com/pricing/calculator/)

-   [Microsoft Azure kustannusten arviointi työkalu (Työpöytä)](http://www.microsoft.com/download/details.aspx?id=43376)

## <a name="acknowledgements"></a>**Kuittaussanomien**
Tässä artikkelissa on tekijä tietojen Tiedemies Yijing Chen ja ohjelmisto lähdekoodiksi Qiu Min Microsoftin.
