<properties
    pageTitle="Tekninen opas ennakoivan ylläpito maanpuolustusteollisuudessa ja muiden yritysten Cortana Intelligence ratkaisu-malliin | Microsoft Azure"
    description="Ennakoivan ylläpito maanpuolustusteollisuudessa, Apuohjelmat ja kuljetus Microsoft Cortana liiketoimintatietojen ratkaisu mallin tekninen opas."
    services="cortana-analytics"
    documentationCenter=""
    authors="fboylu"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="cortana-analytics"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="fboylu" />

# <a name="technical-guide-to-the-cortana-intelligence-solution-template-for-predictive-maintenance-in-aerospace-and-other-businesses"></a>Tekninen opas ennakoivan ylläpito maanpuolustusteollisuudessa ja muiden yritysten Cortana liiketoimintatietojen ratkaisu-malliin

## <a name="acknowledgements"></a>**Kuittaussanomien**
Tässä artikkelissa on tietoja tutkijoiden Yan Zhang Gauher Shaheen, Fidan Boylu Uz ja ohjelmiston lähdekoodiksi Dan Grecoe Microsoftin tekijä.

## <a name="overview"></a>**Yleiskatsaus**

Ratkaisu malleja on suunniteltu nopeuttamaan luomista E2E-esittely Cortana liiketoimintatietojen Suite päälle. Käyttöön mallin valmistella ‑tilauksen tarvittavat Cortana Liiketoimintatieto-osia ja luoda niiden väliset yhteydet. Se myös alustaa tietojen putkijohto esimerkkitiedoilla, joka on luotu luontitoiminto-sovelluksesta, jossa lataaminen ja asentaminen paikallisessa tietokoneessa, kun otat ratkaisumalli. Luotu luontitoiminnon tiedot hydrate tietojen putkijohto ja Käynnistä luodaan tietokoneen learning ennusteiden joka sitten voidaan näyttää Power BI-koontinäytössä. Käyttöönottoprosessin opastaa sinua useita toimenpiteellä ratkaisu tunnistetiedot. Varmista, että voit tallentaa näitä tunnistetietoja, kuten ratkaisun nimi, käyttäjänimi ja salasana käyttöönoton aikana.  

Tämän asiakirjan lähinnä kertoaksesi viittaus arkkitehtuuri ja eri osien valmisteltu tilauksen osana ratkaisu tätä mallia. Asiakirjan puheen myös siitä, miten voit korvata mallitiedot todellisten tietojen yleislähetyksenä voi tarkastella tietoja ja ennusteiden omilla tiedoillasi. Lisäksi asiakirjan käsitellään ratkaisumalli, jota voi muokata, jos haluat mukauttaa omiin tietoihisi ratkaisu on osat. Ohjeita Power BI-koontinäytön ratkaisu-malli on tarkoitettu lopussa.

>[AZURE.TIP] Voit ladata ja tulostaa [tämän asiakirjan PDF-version](http://download.microsoft.com/download/F/4/D/F4D7D208-D080-42ED-8813-6030D23329E9/cortana-analytics-technical-guide-predictive-maintenance.pdf).

## <a name="big-picture"></a>**Ylemmän tason**

![](media/cortana-analytics-technical-guide-predictive-maintenance\predictive-maintenance-architecture.png)

Kun ratkaisu on otettu käyttöön, eri Cortana Analytics Suite Azure palvelut otetaan käyttöön (*eli* tapahtumaa-toiminnossa Stream Analytics, HDInsight, Data Factory, tietokoneen Learning, *jne.*). Yllä olevassa arkkitehtuuri kaaviossa näkyy ylätasolla, miten ennakoivan ylläpito Aerospace ratkaisu mallin muodostetaan lopusta loppuun. Sinulla voi tutkia Yhteisöpalvelut azure-portaalissa valitsemalla niihin ratkaisua mallin kaavio luodaan, kun tämä palvelu on valmisteltu pyydettäessä, kun liittyvät myyntijakso-toiminnot ovat Suorita ja poistetut jälkeenpäin lukuun ottamatta HDInsight-ratkaisun käyttöönotto.
Voit ladata [täydessä kaavioon](http://download.microsoft.com/download/1/9/B/19B815F0-D1B0-4F67-AED3-A40544225FD1/ca-topologies-maintenance-prediction.png).

Seuraavissa kohdissa kuvataan kutakin.

## <a name="data-source-and-ingestion"></a>**Tietolähteen ja nieltynä**

### <a name="synthetic-data-source"></a>Synteettiset tietolähde

Tämän mallin tietolähteen johdetaan työpöytäsovelluksessa, joka lataa ja suorita paikallisesti onnistuneen käyttöönoton jälkeen. Etsii ohjeita voit ladata ja asentaa tämän sovelluksen ominaisuudet-palkissa, kun valitset ensimmäinen solmu nimeltä ennakoivan ylläpito tietojen luontitoiminnon ratkaisu mallin kaavio. Tämän sovelluksen syötteet arvopisteiden tai tapahtumat, joita käytetään ratkaisu kulun loput [Azure tapahtumaa-toiminnossa](#azure-event-hub) -palvelun. Tätä tietolähdettä koostuu tai julkisesti saatavilla olevien tietojen siitä käyttäen [ohivirtausmoottorien ohjelma heikkeneminen simuloinnissa tietojoukon](http://ti.arc.nasa.gov/tech/dash/pcoe/prognostic-data-repository/#turbofan) [NASA tietojen säilöön](http://ti.arc.nasa.gov/tech/dash/pcoe/prognostic-data-repository/) johdettu.

Tapahtuman luominen sovellus lisää Azure tapahtumaa-toiminnossa vain silloin, kun se suoritetaan tietokoneessa.

### <a name="azure-event-hub"></a>Azure tapahtumaa-toiminnossa

[Azure tapahtumaa-toiminnossa](https://azure.microsoft.com/services/event-hubs/) -palvelu on myöntämä synteettistä tietolähteen yllä olevien ohjeiden syötteen vastaanottaja.

## <a name="data-preparation-and-analysis"></a>**Tietojen valmisteleminen ja analysointia varten**


### <a name="azure-stream-analytics"></a>Azure Stream Analytics

[Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) -palvelua käytetään lähes reaaliajassa analytics-syötteen virta [Azure tapahtumaa-toiminnossa](#azure-event-hub) -palvelusta ja julkaista tulokset sivulle [Power BI](https://powerbi.microsoft.com) -koontinäytön sekä arkistointi kaikki raaka saapuvien tapahtumien [Azure](https://azure.microsoft.com/services/storage/) tallennuspalvelu [Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/) -palvelun myöhempää käsittelyä varten.

### <a name="hd-insights-custom-aggregation"></a>HD-tiedot mukautetun kooste

Azure HD tietoja palvelua käytetään (koordinoitu mukaan Azure Data Factory) [rakenne](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) komentosarjojen suorittaminen antamaan koosteet raaka tapahtumista, jotka on arkistoida Azure Stream Analytics-palvelun avulla.

### <a name="azure-machine-learning"></a>Azure koneen oppiminen

[Azure koneen Learning](https://azure.microsoft.com/services/machine-learning/) -palvelua käytetään (koordinoitu mukaan Azure Data Factory) voit tehdä ennusteiden jäljellä oleva käyttöikä (RUL) annettu vastaanotettu syötteiden tietyn ilma-ohjelma.

## <a name="data-publishing"></a>**Tietojen julkaiseminen**


### <a name="azure-sql-database-service"></a>Azure SQL-tietokanta-palvelu

[Azure SQL-tietokanta](https://azure.microsoft.com/services/sql-database/) -palvelua käytetään tallentamiseen (hallitsee Azure Data Factory) Azure koneen Learning palvelu, jota käytetty [Power BI](https://powerbi.microsoft.com) -koontinäytön vastaanottanut ennusteiden.

## <a name="data-consumption"></a>**Tietoja kulutus**

### <a name="power-bi"></a>Power BI

[Power BI](https://powerbi.microsoft.com) -palvelua käytetään näyttämään Raporttinäkymät-ikkunan, joka sisältää koosteet ja ilmoitusten myöntämä [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) -palvelu sekä tallennettu [Azure SQL](https://azure.microsoft.com/services/sql-database/) -tietokantaan RUL ennusteiden, joka on valmistettu [Azure koneen Learning](https://azure.microsoft.com/services/machine-learning/) -palvelun avulla. Ohjeita Power BI-koontinäytön ratkaisu-malli on lisätietoja jäljempänä olevaan kohtaan.

## <a name="how-to-bring-in-your-own-data"></a>**Voit tuoda Omat tietoja**

Tässä osassa kuvataan, miten tuo omiin tietoihisi Azure ja mitä alueita edellyttäisi muutokset tässä arkkitehtuuri tuo tiedot.

On epätodennäköistä, että kaikki tietojoukko voit noutaa vastaa käyttämä [ohivirtausmoottorien ohjelma heikkeneminen simuloinnissa tietojoukon](http://ti.arc.nasa.gov/tech/dash/pcoe/prognostic-data-repository/#turbofan) ratkaisu tätä mallia käytetään tietojoukon. Tietoja tietojen ja vaatimukset ovat tärkeitä-siitä, miten voit muokata tätä mallia toimimaan omiin tietoihisi. Jos tämä on ensimmäinen näkyvyyttäsi Azure koneen Learning-palveluun, saat sen johdanto käyttämällä esimerkin [ensimmäinen kokeen luomisesta](machine-learning-create-experiment.md).

Seuraavissa osissa käsitellään osat, jotka edellyttävät muutokset, kun uusi tietojoukko on otettu käyttöön mallin.

### <a name="azure-event-hub"></a>Azure tapahtumaa-toiminnossa

Azure tapahtumaa-toiminnossa-palvelu on yleinen-siten, että tiedot voit kirjata keskittimeen CSV- tai JSON-muodossa. Ei erityistä käsittely tapahtuu Azure tapahtumaa-toiminnossa, mutta se on tärkeää ymmärtää tiedot, jotka on syötetään.

Tässä asiakirjassa ei kerrotaan, kuinka voit ingest tietoja, mutta voi helposti lähettää tapahtumia tai tietojen Azure tapahtuman keskittimeen tapahtuman keskittimeen Ohjelmointirajapinnan käyttäminen.

### <a name="azure-stream-analytics"></a>Azure Stream Analytics

Azure Stream Analytics-palvelua käytetään antamaan lähellä reaaliaikainen analytics tietojen virtaa luettaessa ja määritetty jokin muu luku lähteiden tiedot.

Aerospace ratkaisu mallin ennakoivan ylläpito Azure Stream Analytics-kyselyn koostuu neljä alikyselyt, kunkin vievää tapahtumia Azure tapahtumaa-toiminnossa-palveluun ja tulostaa on neljää eri paikkaan. Nämä tulostaa koostuvat kolme Power BI-tietojoukkoja ja yksi Azure tallennuspaikka.

Azure Stream Analytics-kyselyn löytävät perusteella:

-   Azure portaalin kirjautumisessa

-   Etsiminen stream analytics työt ![](media\cortana-analytics-technical-guide-predictive-maintenance\icon-stream-analytics.png) , joka luotiin, kun ratkaisu on otettu käyttöön (*kuten*, **maintenancesa02asapbi** ja **maintenancesa02asablob** ennakoivan ylläpito-ratkaisun)

-   Valitseminen

    -   Voit tarkastella kyselyn syöte ***SYÖTTEIDEN***

    -   Voit tarkastella itse kysely ***kysely***

    -   Jos haluat tarkastella eri tulostus ***TULOSTAA***

Lisätietoja Azure Stream Analytics kyselyn rakenteeseen löytyy MSDN-sivuston [Stream Analytics kyselyn viittaus](https://msdn.microsoft.com/library/azure/dn834998.aspx) .

Tämä ratkaisu kyselyt tulosteen kolme tietojoukkoja kanssa lähellä reaaliaikainen analytics saapuvien tietovirta Power BI-raporttinäkymän, joka on annettu osana ratkaisu tämän mallin tietoja. Koska saapuvan tietomuoto implisiittinen tietosi, nämä kyselyt on muuteta mukaan tietojen muoto.

Toisessa muodossa analytics työn **maintenancesa02asablob** kyselyn tulostaa riittää, että kaikki [Azuren tallennustilaan](https://azure.microsoft.com/services/storage/) [Tapahtumaa-toiminnossa](https://azure.microsoft.com/services/event-hubs/) tapahtumia ja vaatii näin ollen ei muutosta riippumatta tietojen muoto koko tapahtumatiedot on virtauttaa tallennustilan.

### <a name="azure-data-factory"></a>Azure Data Factory

[Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/) -palvelun orchestrates liikkuvuuden ja tietojen käsittely. Ennakoivan ylläpitoa Aerospace ratkaisu mallin tiedot factory koostuu kolmesta [putkistot](../data-factory/data-factory-create-pipelines.md) , siirtää ja käsitellä tietoja käyttämällä eri tekniikoita.  Voit käyttää tietojen factory avaamalla ratkaisun mallin kaavion alareunassa Data Factory-solmu, joka on luotu käyttämällä ratkaisun käyttöönotto. Tällöin näyttöön tiedot factory Azure-portaalissa. Jos näyttöön tulee virhe kohdassa oman tietojoukkoja, voit ohittaa ne sellaisina kuin ne ovat tietojen factory otetaan käyttöön ennen tietojen luontitoiminnon aloitettiin vuoksi. Näiden virheiden ei estä tietojen factory toimimasta.

![](media/cortana-analytics-technical-guide-predictive-maintenance/data-factory-dataset-error.png)

Tässä osassa käsitellään tarvittavat [putkistot](../data-factory/data-factory-create-pipelines.md) ja [Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/)sisältyvät [tehtävät](../data-factory/data-factory-create-pipelines.md) . Alla on ratkaisun kaavionäkymä.

![](media/cortana-analytics-technical-guide-predictive-maintenance/azure-data-factory.png)

Kaksi Tämä tehdas putkistot sisältää [rakenne](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) komentosarjoja, joita käytetään osio ja koota tietoja. Kun on kerrottu, komentosarjat sijaitsevat [Azure-tallennustilan](https://azure.microsoft.com/services/storage/) tilin luotu asennuksen aikana. Niiden sijainti on: maintenancesascript\\\\komentosarjan\\\\rakenteen\\ \\ (tai https://[Your ratkaisu name].blob.core.windows.net/maintenancesascript).

Samalla [Azure Stream Analytics](#azure-stream-analytics-1) -kyselyt, [rakenne](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) -komentosarjat on saapuvan tietomuoto implisiittinen tietosi, nämä kyselyt on muutettava muoto ja [ominaisuus tekniikka](machine-learning-feature-selection-and-engineering.md) tarpeen mukaan.

#### <a name="aggregateflightinfopipeline"></a>*AggregateFlightInfoPipeline*

Tämä [myyntijakso](../data-factory/data-factory-create-pipelines.md) sisältää yhden aktiviteetin - [HDInsightHive](../data-factory/data-factory-hive-activity.md) tehtävän käyttämällä [HDInsightLinkedService](https://msdn.microsoft.com/library/azure/dn893526.aspx) , joka suoritetaan [rakenne](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) komentosarjan osion tiedot sijoitetaan [Azuren tallennustilaan](https://azure.microsoft.com/services/storage/) [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) -projektin aikana.

[Rakenne](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) -komentosarja osioinnin tehtävän on ***AggregateFlightInfo.hql***

#### <a name="mlscoringpipeline"></a>*MLScoringPipeline*

Tämä [myyntijakso](../data-factory/data-factory-create-pipelines.md) sisältää useita toimintoja ja jonka lopputulos on scored ennusteiden [Azure koneen Learning](https://azure.microsoft.com/services/machine-learning/) kokeilla liitetty ratkaisu tämän mallin avulla.

Tämä sisältyvät tehtävät ovat seuraavat:

-   [HDInsightHive](../data-factory/data-factory-hive-activity.md) toiminto, jossa käytetään [HDInsightLinkedService](https://msdn.microsoft.com/library/azure/dn893526.aspx) , joka suoritetaan [rakenne](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) komentosarjan suorittaminen koosteet ja ominaisuus tekniikka tarvittavat [Azure koneen Learning](https://azure.microsoft.com/services/machine-learning/) kokeen.
    [Rakenne](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) -komentosarja osioinnin tehtävän on ***PrepareMLInput.hql***.

-   [Kopioi](https://msdn.microsoft.com/library/azure/dn835035.aspx) -toiminto, joka siirtää tulokset yhteen [Azuren tallennustilaan](https://azure.microsoft.com/services/storage/) -blob, joka voi olla [AzureMLBatchScoring](https://msdn.microsoft.com/library/azure/dn894009.aspx) -toiminnon käytön [HDInsightHive](../data-factory/data-factory-hive-activity.md) tehtävästä.

-   [AzureMLBatchScoring](https://msdn.microsoft.com/library/azure/dn894009.aspx) -toiminto, joka kutsuu [Azure koneen Learning](https://azure.microsoft.com/services/machine-learning/) kokeilla, joka johtaa tulokset yhteen [Tallennustilan Azure](https://azure.microsoft.com/services/storage/) -blob-käyttöön.

#### <a name="copyscoredresultpipeline"></a>*CopyScoredResultPipeline*

Tämä [myyntijakso](../data-factory/data-factory-create-pipelines.md) sisältää yhden aktiviteetin - [kopio](https://msdn.microsoft.com/library/azure/dn835035.aspx) -toiminto, joka siirtää [Azure koneen Learning](#azure-machine-learning) tulokset kokeilla ***MLScoringPipeline*** [Azure SQL-tietokantaan](https://azure.microsoft.com/services/sql-database/) , joka on valmisteltu ratkaisu mallin asennuksen yhteydessä.

### <a name="azure-machine-learning"></a>Azure koneen oppiminen

Ratkaisu tätä mallia käytetään [Azure koneen Learning](https://azure.microsoft.com/services/machine-learning/) kokeen on jäljellä olevan hyötyä aika (RUL) ilma-moduulin. Kokeen on käytetty tietojoukon ja näin ollen edellyttävät muuttaminen tai korvaaminen liittyvät tiedot, jotka tuodaan.

Lisätietoja siitä, miten Azure koneen Learning kokeen on luotu on [ennakoivan ylläpito: vaihe 1 / 3-tietojen valmisteleminen ja ominaisuus tekniikka](http://gallery.cortanaanalytics.com/Experiment/Predictive-Maintenance-Step-1-of-3-data-preparation-and-feature-engineering-2).

## <a name="monitor-progress"></a>**Näytön edistyminen**
 Kun tietojen luontitoiminnon käynnistetään, putkijohto alkaa Hae hydratoitu ja ratkaisu eri osat Aloita kicking toiminnon seuraavat komennot antamien tietojen Factory kyselyjä. Voit valvoa putkijohto kahdella tavalla.

1. Yksi Stream Analytics-työ kirjoittaa raaka saapuvat tiedot Blob-objektien tallennustilaan. Jos valitset Blob-objektien tallennustilaan osalle, että näytössä voit onnistuneesti ratkaisun käyttöön ja valitse sitten Avaa oikeanpuoleisessa, se siirtää sinut [hallinta-portaalin](https://portal.azure.com/). Kerran, valitse BLOB-objektit. Seuraava ruutu tulee näkyviin säilöjen luettelo. Valitse **maintenancesadata**. Seuraava ruutu tulee näkyviin **rawdata** -kansio. Rawdata-kansiossa, näet kansioiden nimet, kuten tunti = 17 tuntia = 18 jne. Jos näet nämä kansiot, se osoittaa, että perustietoja on onnistuneesti on luotu tietokoneeseen ja Blob-objektien tallennustilaan tallennettuja. Raportissa pitäisi näkyä csv-tiedostoja, jotka pitäisi olla rajallinen koot megatavuina kansiot.

2. Putkijohto viimeinen vaihe on voi kirjoittaa tietoja (kuten ennusteiden koneen Learning) SQL-tietokantaan. Voit joutua odottamaan enintään kolme tuntia tiedot näkyvät SQL-tietokantaan. Palvelun [azure-portaalissa](https://manage.windowsazure.com/)on yksi tapa seurata, kuinka paljon tietoja on käytettävissä SQL-tietokantaan. Etsi vasemmassa paneelissa SQL-TIETOKANTOJA ![](media\cortana-analytics-technical-guide-predictive-maintenance\icon-SQL-databases.png) napsauttamalla sitä. Valitse Etsi tietokanta- **pmaintenancedb** ja napsauttamalla sitä. Valitse seuraavan sivun alareunassa hallinta

    ![](media\cortana-analytics-technical-guide-predictive-maintenance\icon-manage.png).

    Tässä kohdassa voit valita uusi kysely-ja kysely, kuinka monta riviä (kuten select määrä(*)-PMResult). Kun tietokannan kasvaa, Suurenna olisi taulukon rivien määrä.


## <a name="power-bi-dashboard"></a>**Power BI-koontinäyttö**

### <a name="overview"></a>Yleiskatsaus

Tässä osassa kuvataan Power BI-koontinäytön määrittämisestä visualisointi analytics Azure stream (kuuma polun), reaaliaikaiset tiedot sekä erän tekstinsyöttö tulokset Azure konepohjaisten oppimistekniikoiden (kylmän polun).

### <a name="setup-cold-path-dashboard"></a>Kylmän polku Raporttinäkymät-ikkunan asetukset

Olennaiset tavoitteena on kylmän polku tietojen putkijohto, saat kunkin ilma-ohjelma ennakoivan RUL (jäljellä oleva käyttöikä), kun se päättyy lento (jakson). Ennustamisominaisuudet-tulos päivitetään jokaisen 3 tuntia korrelaatio ilma-ohjelmien, jotka olet lopettanut lento edellisten 3 tunnin aikana.

Power BI muodostaa Azure SQL-tietokantaan sen tietolähteeksi tekstinsyöttö tulokset tallennuspaikka. Huomautus: 1) yhteydessä käyttöönotto-ratkaisun reaali tekstinsyöttö näkyy tietokannan 3 tunnin kuluessa.
Lataa luontitoiminnon mukana toimitettua pbix tiedostossa on lähde tietoja niin, että voit luoda Power BI-koontinäytön saman tien. 2) tässä vaiheessa edellytyksenä on ladattava ja asennettava maksuton [Power BI desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)-ohjelmisto.

Seuraavat vaiheet auttaa sinua siitä, miten voit muodostaa pbix tiedoston SQL-tietokantaan, joka on kehrätty aikaa ratkaisun käyttöönoton, joka sisältää tietoja (*kuten*. Ennakoiva tekstinsyöttö tulokset) visualisoinnille.

1.  Pyydä tietokannan tunnistetietoja.

    Sinun on **Tietokantapalvelimen nimi, tietokannan nimi, käyttäjänimi ja salasana** ennen siirtymistä vaiheisiin. Näin löydät, voit hakea muistiinpanoja.

    -   Kun **Azure SQL-tietokanta** -ratkaisun mallin kaavion muuttuu vihreä, napsauttamalla sitä ja valitse sitten **Avaa**.

    -   Näkyviin tulee uusi välilehti/selainikkunassa, joka näyttää Azure portaalisivu. Valitse **'resurssiryhmät'** vasemmassa paneelissa.

    -   Käytät käyttöönoton ratkaisun tilaus ja valitse sitten **' YourSolutionName\_ResourceGroup'**.

    -   Valitse Ohjauspaneelin uusi ponnahdusikkuna ![](media\cortana-analytics-technical-guide-predictive-maintenance\icon-sql.png) tietokanta-kuvaketta. Tietokannan nimen vieressä tämä on kuvake (*kuten*, **"pmaintenancedb"**) ja **Tietokantapalvelimen nimi** näkyy kohdassa palvelimen nimi-ominaisuus ja pitäisi muistuttaa **YourSoutionName.database.windows.net**.

    -   Tietokannan **käyttäjänimi** ja **salasana** on sama kuin käyttäjänimi ja salasana on tallennettu aiemmin aikana-ratkaisun käyttöönotto.

2.  Päivitä tietolähteen kylmän polku raportin tiedoston Power BI Desktopin.

    -   Kaksoisnapsauta tietokoneeseen, jossa ladattu ja purettuna luontitoiminnon tiedosto-kansiossa **PowerBI\\PredictiveMaintenanceAerospace.pbix** tiedosto. Jos näet varoituksen kaikki viestit, kun avaat tiedoston, voit ohittaa ne. Ylälaidassa tiedoston Valitse **Muokkaa kyselyt**.

        ![](media\cortana-analytics-technical-guide-predictive-maintenance\edit-queries.png)

    -   Näyttöön tulee kaksi taulukkoa, **RemainingUsefulLife** ja **PMResult**. Valitse ensimmäinen taulukko ja valitse ![](media\cortana-analytics-technical-guide-predictive-maintenance\icon-query-settings.png) **'lähde-** kohdassa **' Kyselyasetukset '** oikeanpuoleisessa **Käytetyt vaiheet** -kohdan vieressä. Ohita varoitus viestejä, jotka tulevat näkyviin.

    -   Ponnahdusikkuna-ikkunassa, valitse Korvaa **"Palvelin"** ja **"Tietokanta"** Omat palvelimen ja tietokannan nimet ja valitse sitten **"OK"**. Palvelimen nimi Varmista, että määrität porttia 1433 (**YourSoutionName.database.windows.net, 1433**). Jätä **pmaintenancedb**Database-kenttä. Ohita varoitus viestit, jotka näkyvät näytössä.

    -   Ikkunassa Seuraava ponnahdusikkuna näet kaksi asetusta, valitse vasemman ruudun (**Windows** ja **tietokannan**). Valitse **"Tietokanta"**, täytä **'Käyttäjänimi'** ja **"Salasana"** (tämä on käyttäjänimi ja salasana kirjoittamasi ensin käyttöön ratkaisun ja luotu Azure SQL-tietokantaan). ***Valitse mitä tasoa, voit käyttää näitä asetuksia***Tarkista tietokannan tason vaihtoehtoa. Valitse **"Yhdistä"**.

    -   Napsauta toisen taulukon **PMResult** ja valitse sitten ![](media\cortana-analytics-technical-guide-predictive-maintenance\icon-navigation.png) **'lähde-** kohdassa **' Kyselyasetukset '** oikeassa paneelissa **Käytetyt vaiheet** -kohdan vieressä ja päivitä sitten edellä kuvatut toimet palvelin ja tietokanta nimet ja valitse OK.

    -   Kun olet ohjatun takaisin edelliselle sivulle, sulje ikkuna. Viestin ponnahtaa ulos - valitsemalla **Käytä**. Valitse lopuksi **Tallenna** -painiketta, jos haluat tallentaa muutokset. Power BI-tiedosto on nyt muodostanut yhteyden palvelimeen. Jos visualisointeihin ovat tyhjiä, varmista, että Poista valinnat visualisointien kaikkien tietojen visualisointi napsauttamalla oikeassa yläkulmassa selitteitä Pyyhekumi-kuvaketta. Päivitä-painike vastaamaan uusia tietoja visualisointien avulla Aluksi vain näet lähde-tiedot visualisointeihin, kun tietojen factory on ajoitettu päivitys 3 tunnin välein. 3 tunnin jälkeen näkyviin tulee uusi ennusteiden visualisointeihin näkyvät, kun päivität tiedot.

3.  (Valinnainen) Julkaise [Verkossa Power BI](http://www.powerbi.com/)kylmän polku Raporttinäkymät-ikkunan. Huomaa, että tämä vaihe on Power BI tili (tai Office 365-tili).

    -   Valitse **'Julkaise,'** ja myöhemmin hetkinen ikkunan näkyy näyttäminen "Julkaiseminen Power BI-Success!" Vihreä valintamerkki. Valitse "Avaa PredictiveMaintenanceAerospace.pbix-Power BI-alla olevaa linkkiä. Lisätietoja on artikkelissa [Power BI Desktop Julkaise](https://support.powerbi.com/knowledgebase/articles/461278-publish-from-power-bi-desktop).

    -   Luo uusi raporttinäkymät-ikkuna: Valitse **+** vieressä vasemmanpuoleisessa ruudussa **raporttinäkymät** -osa-merkkiä. Kirjoita nimi "Ennakoivan ylläpito esittely" uusi raporttinäkymät-ikkuna.

    -   Kun avaat raportin, valitse ![](media\cortana-analytics-technical-guide-predictive-maintenance\icon-pin.png) kiinnittäminen kaikki näkymän visualisoinnit koontinäyttöön. Lisätietoja on artikkelissa [PIN-tunnuksen Power BI-koontinäytön raportista ruutu](https://support.powerbi.com/knowledgebase/articles/430323-pin-a-tile-to-a-power-bi-dashboard-from-a-report).
    Siirry raporttinäkymä-sivulle ja koon ja sijainnin visualisointeihin ja muokkaa niiden otsikoita. Lisätietoja siitä, miten voit muokata ruutujen lisätietoja [Muokkaa ruudun--kokoa, siirrä, nimeä uudelleen, PIN-koodi, poista, Lisää hyperlinkki](https://powerbi.microsoft.com/documentation/powerbi-service-edit-a-tile-in-a-dashboard/#rename). Tässä on esimerkki-koontinäytön joitakin kylmän polku-visualisointien kiinnitetyt siihen.  Sen mukaan, kuinka kauan suoritat tietojen luontitoiminnon lukujen visualisointien saattavat olla erilaiset.
    <br/>
    ![](media\cortana-analytics-technical-guide-predictive-maintenance\final-view.png)
<br/>
    -   Tietojen päivityksen ajoittaminen, Vie hiiren **PredictiveMaintenanceAerospace** tietojoukko päälle, napsauta ![](media\cortana-analytics-technical-guide-predictive-maintenance\icon-elipsis.png) ja valitse sitten **Ajoita päivitys**.
<br/>
        **Huomautus:** Jos näet varoituksen hieronta, valitse **Muokkaa tunnistetiedot** ja varmista, että tietokannan tunnistetiedot ovat samat kuin vaiheen 1.
<br/>
    ![](media\cortana-analytics-technical-guide-predictive-maintenance\schedule-refresh.png)
<br/>
    -   Laajenna osa **Ajoita päivitys** . Ota käyttöön "tietojen pitäminen ajan tasalla".
    <br/>
    -   Käyttötarkoitukseen päivityksen ajoittaminen. Lisätietoja on artikkelissa [tietojen päivittäminen Power BI](https://support.powerbi.com/knowledgebase/articles/474669-data-refresh-in-power-bi).

### <a name="setup-hot-path-dashboard"></a>Kuuma polku Raporttinäkymät-ikkunan asetukset

Seuraavat vaiheet opastaa, kuinka haluat esittää reaaliaikaiset tiedot tulosteen Stream Analytics työt, jotka on luotu ratkaisun käyttöönoton yhteydessä. Seuraavien toimien edellyttää [Verkossa Power BI](http://www.powerbi.com/) -tili. Jos sinulla ei ole tiliä, voit [luoda](https://powerbi.microsoft.com/pricing).

1.  Lisää Power BI-tulostus Azure Stream Analytics (ASA).

    -  Noudata tarvitset [Azure Stream Analytics ja Power BI: reaaliaikainen näkyvyyden streaming data reaaliaikainen analytics-Raporttinäkymät-ikkunan](stream-analytics-power-bi-dashboard.md) Azure Stream Analytics työtäsi Power BI-koontinäytöksi tulosteen määrittäminen.
    - ASA kyselyssä on kolme tulostus, jotka ovat **aircraftmonitor**, **aircraftalert**ja **flightsbyhour**. Voit tarkastella kysely valitsemalla kysely-välilehti. Vastaavien nämä taulukot, tarvitset tulos lisääminen ASA. Kun lisäät ensimmäisen tulos (*kuten* **aircraftmonitor**) Varmista, että **Tunnus**, **Tietojoukon nimi** ja **Taulukon nimessä** on sama (**aircraftmonitor**). Toista vaiheet Lisää tulostaa **aircraftalert**ja **flightsbyhour**. Kun olet lisännyt kaikki kolme tulosteen taulukot ja aloittaminen ASA työ, hanki vahvistussanoman (*kuten*, "Käynnistetään stream analytics työn maintenancesa02asapbi onnistui").

2. [Power BI verkossa](http://www.powerbi.com) kirjautuminen

    -   Vasemman ruudun oma työtilan tietojoukkoja-kohdassa pitäisi näkyä ***TIETOJOUKKO*** nimet **aircraftmonitor**, **aircraftalert**ja **flightsbyhour** . Tämä on Azure Stream Analytics miten edellisessä vaiheessa streaming tiedot. Tietojoukon **flightsbyhour** ei välttämättä näy samaan aikaan kuin muut vuoksi takana SQL-kyselyn laatu kaksi tietojoukkoja. Kuitenkin se näytetään tunnin jälkeen.
    -   Varmista, että ***Visualisoinnit*** -ruutu on auki, joka näkyy näytön oikeassa reunassa.

3. Kun Power BI juoksutus tiedot, voit aloittaa kesäolympialaisten visualisointi streaming tiedot. Alla on esimerkkiraporttinäkymä, jonka kuuma polku Joidenkin visualisointien kiinnitetty siihen. Voit luoda muiden Raporttinäkymät-ikkunan ruudut, tarvittavat tietojoukkoja perusteella. Sen mukaan, kuinka kauan suoritat tietojen luontitoiminnon lukujen visualisointien saattavat olla erilaiset.


    ![](media\cortana-analytics-technical-guide-predictive-maintenance\dashboard-view.png)

4. Tässä on muutama luomaan yllä ruudut – "laivaston näkymän, tunnistimen 11 ja kynnysarvo 48.26"-ruutu:

    -   Valitse vasemman ruudun tietojoukkoja osan tietojoukko **aircraftmonitor** .

    -   **Viivakaavio** -kuvaketta.

    -   Valitse **kentät** -ruudusta **käsitteleminen** , niin, että se näkyy "Akselin" Valitse **Visualisoinnit** -ruudussa.

    -   Valitse "s11" ja "s11\_ilmoitus" niin, että ne näkyvät "arvot-kohdassa. Valitse **s11** vieressä olevaa pientä nuolta ja **s11\_ilmoitus**, vaihtaa "Sum" "Keskiarvo".

    -   Valitse yläkulmassa **Tallenna** ja anna raportille nimi "aircraftmonitor". Raportin nimeltä "aircraftmonitor" näkyvät **vasemman reunan ruudussa** **Raportit** -osioon.

    -   Napsauta viivakaaviossa oikeassa yläkulmassa olevaa **Kiinnitä Visual** -kuvaketta. "Pin-Raporttinäkymät-ikkunan ehkä Näytä sopivat Raporttinäkymät-ikkunan. Valitse "Ennakoivan ylläpito esittely", "PIN-tunnuksen".

    -   Hiiren osoitinta koontinäytössä tämä ruutu, "Muokkaa-kuvaketta, voit muuttaa sen"Laivaston näkymän, tunnistimen 11 ja kynnysarvo 48.26"otsikko ja alaotsikko"Laivaston ajan yli keskiarvon"oikeassa yläkulmassa.

## <a name="how-to-delete-your-solution"></a>**Voit poistaa ratkaisu**
Varmista, että lopetat tietojen luontitoiminnon käyttämällä ratkaisun aktiivisesti käytössä tietojen luontitoiminnon selvittää kustannuksia. Poista ratkaisu, jos et käytä sitä. Ratkaisu poistetaan kaikki komponentit, jotka on valmisteltu tilaukseesi, kun ratkaisun käyttöön. Voit poistaa ratkaisu valitsemalla ratkaisun nimeäsi vasemmassa ruudussa ratkaisumalli ja valitse Poista.

## <a name="cost-estimation-tools"></a>**Kustannusten arviointi Työkalut**

Seuraavat kaksi työkalut ovat auttavat ymmärtämään yhdistävää käynnissä ennakoivan ylläpito Aerospace ratkaisu mallin tilauksen kokonaiskustannukset:

-   [Microsoft Azure kustannusten arviointi työkalu (online)](https://azure.microsoft.com/pricing/calculator/)

-   [Microsoft Azure kustannusten arviointi työkalu (Työpöytä)](http://www.microsoft.com/download/details.aspx?id=43376)
