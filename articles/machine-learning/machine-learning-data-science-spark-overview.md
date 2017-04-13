<properties
    pageTitle="Yleistä tietojen tiede ohjattu käyttäminen Azure Hdinsightiin | Microsoft Azure"
    description="Ohjattu MLlib työkalujen näyttää huomattavasti koneen learning mallinnus ominaisuuksia, eri aikajaksoille HDInsight-ympäristössä."
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun"  />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/07/2016"
    ms.author="deguhath;bradsev;gokuma" />

# <a name="overview-of-data-science-using-spark-on-azure-hdinsight"></a>Yleistä tietojen tiede ohjattu käyttäminen Azure Hdinsightiin

[AZURE.INCLUDE [machine-learning-spark-modeling](../../includes/machine-learning-spark-modeling.md)]

Tämä suite aiheista näytetään, miten HDInsight ohjattu avulla voit suorittaa yleisiä tietoja tiede tehtäviä esimerkiksi tietojen nieltynä, toiminto-tekniikka, mallinnus ja mallin arviointi. Käytettävät tiedot on otoksen 2013 Kokousesitelmän taksin työmatkan ja maksun DataSet. Sisäisten mallit ovat logistista ja lineaarisen regressiosuoran, satunnainen metsien ja liukuvärin tehosti puut. Aiheiden näyttää tallentaminen näitä malleja Azuren Blob-objektien tallennustilaan (WASB) sekä pisteet ja niiden ennakoivan suorituskyvyn arvioiminen. Monimutkaisemman ohjeaiheissa miten malleja voi olla koulutus rajat kelpoisuustarkistus ja hyper-parametrin sweeping avulla. Yleistä Tässä ohjeaiheessa kuvataan myös määrittämisestä Ohjattu klusterin, sinun on kolme vaiheittaisissa ohjeissa annettujen ohjeiden mukaisesti. 

[Ohjattu](http://spark.apache.org/) on käsittelyn framework, joka tukee ladatun käsittelyn edistää big datasta analyyttisten sovellusten suorituskykyä Avaa lähde-rinnakkain. Ohjattu käsittely ohjelma on luotu nopeus-käyttöä ja yksityiskohtaisen analysoinnin helpottamiseksi. Ohjattu päivän ladatun hajautettu laskenta ominaisuuksia Varmista on hyvä iteratiivinen algoritmien koneen opiskelu- ja kaavion funktiolauseita. [MLlib](http://spark.apache.org/mllib/) on ohjattu 's skaalattava koneen learning kirjasto, joka tuo mallinnus ominaisuuksia tässä jaetussa ympäristössä. 

[Ohjattu Hdinsightista](../hdinsight/hdinsight-apache-spark-overview.md) on Azure isännöityä tarjoaminen Avaa lähde ohjattu. Se on myös **Jupyter PySpark muistikirjat** , jotka voidaan suorittaa vuorovaikutteinen ohjattu SQL-kyselyjä ja muodonmuutoksen, suodattaminen ja kesäolympialaisten visualisointi tiedot, jotka on tallennettu Azure BLOB-objektit (WASB) Ohjattu klusterin tuki. PySpark on ohjattu Python Ohjelmointirajapinta. Koodikatkelmat, joiden ratkaisuja ja Näytä asiaa joudutaan tähän Suorita ohjattu klustereiden asennettuihin Jupyter muistikirjoissa tietojen visualisoimiseen. Näissä ohjeaiheissa mallinnus ohjeita sisältävät koodi, joka näyttää, miten kouluttaminen, arvioida, tallentaa ja tarjoaman kunkin mallin tyypin. 

Määrityksen vaiheet ja koodia omalla tätä vaiheittaista on HDInsight 3.4 ohjattu 1.6. Kuitenkin koodin tähän muistikirjat on yleinen ja pitäisi toimia Ohjattu klusterin. Jos et käytä ohjattu HDInsight-klusterin määrittäminen ja hallinta vaiheet saattavat olla hieman erilainen kuin seuraavassa kuvassa.

## <a name="prerequisites"></a>Edellytykset

1. sinulla on oltava Azure tilaus. Jos sinulla ei ole yhtä, katso [Hae Azure maksuttoman kokeiluversion](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

2. sinun on HDInsight 3.4 ohjattu 1.6-klusterin vaiheiden suorittamiseen. Voit luoda, katso ohjeita [aloittaminen: Luo Apache ohjattu Azure Hdinsightiin](../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md). **Valitse klusterin tyyppi** -valikosta on määritetty klusterin tyyppi ja versio. 


![](./media/machine-learning-data-science-spark-overview/spark-cluster-on-portal.png)

<!-- -->

> [AZURE.NOTE] Aiheen, joka näyttää, miten haluat käyttää Scala Python asemesta tehtävän loppuun tiedot-tiede prosessin-kohdassa [tietoja tiede käyttämällä Scala ohjattu Azure-kanssa](machine-learning-data-science-process-scala-walkthrough.md).

<!-- -->

>[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


## <a name="the-nyc-2013-taxi-data"></a>Kokousesitelmän 2013 taksin tiedot

Kokousesitelmän taksin työmatkan tiedot on noin 20 gt: n pakattu pilkuilla erotetut arvot (CSV)-tiedostot (noin 48 gt puretaan), 173 miljoonaa yksittäisiä trips ja kuljetusmaksut maksettu kunkin työmatkan. Työmatkan kunkin tietueen sisältää Valitse ylös ja latauskirjaston sijainti ja aika-anonymized hakkeroida (ohjaimen) käyttöoikeuden numero ja medallion (taksin on yksilöllinen tunnus) numero. Tietoja kattaa kaikki trips vuoden 2013 ja annetaan seuraavat kaksi tietojoukkoja jokaiselle kuukaudelle:

1. 'Trip_data' CSV-tiedostoja sisältävät työmatkan tiedot, kuten numero henkilöiden, nosta ja keston ja työmatkan pituus dropoff osoittaa työmatkan. Tässä on muutama Esimerkki tietue:

        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count,trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868

2. 'Trip_fare' CSV-mallitiedostot sisältävät tietoja maksettu kunkin työmatkan, kuten maksutyyppi, maksun summa, Lisämaksu ja verojen, vihjeet ja tietullit, maksun ja yhteensä maksetun summan. Tässä on muutama Esimerkki tietue:

        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5


Syy on otettava 0,1 otoksen nämä tiedostot ja liittyneet matkan\_tiedot ja työmatkan\_kuljetusmaksua CVS tiedostojen siirtäminen yhden tietojoukko käytettävä tätä vaiheittaista syötteen tietojoukko. Yksilöivä liittymään työmatkan\_tiedot ja työmatkan\_maksun koostuu kentät: medallion, hakkeroida\_lisenssin ja nouto\_datetime. Kunkin tietueen DataSet sisältää edustava Kokousesitelmän taksin matkan määritteet:


|Kenttä| Lyhyt kuvaus
|------|---------------------------------
| medallion |Anonymized taksin medallion (taksin yksilöllinen tunnus)
| hack_license |    Anonymized Hackney kuljetus käyttöoikeuksien määrä
| vendor_id |   Taksin Toimittajatunnus
| rate_code | Maksun Kokousesitelmän taksin korko
| store_and_fwd_flag | Tallentaminen ja lähettäminen edelleen merkinnän
| pickup_datetime | Nosta päivämäärä ja aika
| dropoff_datetime | Dropoff päivämäärä ja aika
| pickup_hour | Nosta tunti
| pickup_week | Vuoden viikko Nosta
| Viikonpäivä | Viikonpäivä (1 – 7 alue)
| passenger_count | Henkilöiden matkan taksin numero
| trip_time_in_secs | Matkan sekunteina
| trip_distance | Matkan etäisyys ottanut Mailia
| pickup_longitude | Nosta pituutta
| pickup_latitude | Nosta leveyttä
| dropoff_longitude | Dropoff pituutta
| dropoff_latitude | Dropoff leveyttä
| direct_distance | Suora poiminta välisen etäisyyden määrittäminen ja dropoff sijainnit
| payment_type | Maksutyyppi (cas, luottokortin jne.)
| fare_amount | Maksun summa
| lisä | Lisä
| mta_tax | MTA vero
| tip_amount | Vihje summa
| tolls_amount | Tietullit summa
| total_amount | Kokonaissumma
| Kallistettu | Kallistettu (0 ja 1 ei tai Kyllä)
| tip_class | Vihje luokan (0: $0, 1: $0 – 5 ja 2: $6-10-3: $11-20-4: > $20)


## <a name="execute-code-from-a-jupyter-notebook-on-the-spark-cluster"></a>Suorita Ohjattu klusterin Jupyter muistikirjan koodi 

Voit käynnistää Jupyter muistikirjan Azure-portaalista. Etsi Ohjattu-klusterin raporttinäkymät- ja käyttöoikeus ja anna hallinta-sivun yhteyttä klusterin. Avaa Ohjattu klusterin liittyvät muistikirja, valitse **Klusterin raporttinäkymien** -> **Jupyter muistikirja** .

![](./media/machine-learning-data-science-spark-overview/spark-jupyter-on-portal.png)

Voit myös selata ***https://CLUSTERNAME.azurehdinsight.net/jupyter*** Siirry Jupyter muistikirjoihin. Tätä URL-Osoitetta CLUSTERNAME-osan korvaaminen oman klusterin nimen. Tarvitset Siirry muistikirjoihin järjestelmänvalvojan tilin salasana.

![](./media/machine-learning-data-science-spark-overview/spark-jupyter-notebook.png)

Valitse PySpark näet valmiiksi pakattu muistikirjat, jotka käyttävät PySpark Ohjelmointirajapinnan muutamia esimerkkejä sisältävän kansion. Muistikirjat, jotka sisältävät tämän Suite ohjattu aiheen MALLIKOODEJA on osoitteessa [Github](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/Spark/pySpark)


Voit ladata muistikirjat suoraan Github Jupyter muistikirjan palvelimeen ohjattu klusterissa. Oman Jupyter aloitussivulla Valitse näytön oikean puolen **Lataa** -painiketta. Se Avaa Resurssienhallinnassa. Tässä voit liittää muistikirjan Github (raaka-sisältö) URL-osoite ja valitse **Avaa**. PySpark muistikirjat on osoitteessa seuraavat URL-osoitteet:

1.  [pySpark-machine-learning-data-science-spark-data-exploration-modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/pySpark-machine-learning-data-science-spark-data-exploration-modeling.ipynb)
2.  [pySpark-machine-learning-data-science-spark-model-consumption.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/pySpark-machine-learning-data-science-spark-model-consumption.ipynb)
3.  [pySpark-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/pySpark-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb)

Näet tiedostonimen Jupyter tiedosto-luettelossa, jossa **Lataa** -painiketta uudelleen. Napsauttamalla tätä **Lataa** -painiketta. On nyt tuotu muistikirja. Toista nämä vaiheet, voit ladata tämän vaiheittaisen kuvauksen suorittamiseksi muut muistikirjat.

> [AZURE.TIP] Voit linkit selaimen napsauttamalla hiiren kakkospainikkeella ja valitse **Kopioi linkki** github raaka sisällön URL-Osoitteen hankkiminen. Voit liittää Jupyter ladata tiedoston explorer valintaikkunan tätä URL-Osoitetta.

Nyt voit tehdä seuraavia toimia:

- Katso koodi napsauttamalla muistikirjaa.
- Suorita kullekin solulle painamalla näppäinyhdistelmää **VAIHTO + ENTER**.
- Koko muistikirjan suorittaminen **solussa**napsauttamalla -> **Suorita**.
- Automaattinen visualisoinnin kyselyjen avulla

> [AZURE.TIP] PySpark ydin automaattisesti valitseminen puolestaan Visualisoi tulos (HiveQL) SQL-kyselyjä. Voit valita useita erilaisia visualisointeja (taulukko, ympyräkaavio, rivin, alue tai palkin) **tyyppi** -valikosta painikkeiden avulla muistikirjan on esitetty:

![Yleinen lähestymistapa logistista regressiota OHJETIEDOSTO käyrä](./media/machine-learning-data-science-spark-overview/pyspark-jupyter-autovisualization.png)

## <a name="whats-next"></a>Mitä seuraavaksi?

Ohjattu HDInsight-klusterin määritetään ja olet ladannut Jupyter-muistikirjoja, olet valmis käydä läpi ohjeita, jotka vastaavat kolme PySpark muistikirjat. Ne näyttävät, kuinka tietojen ja miten voit luoda ja käyttää malleja. Lisäasetukset tietojen tarkasteluun ja mallinnus muistikirjan esitetään, kuinka voit sisällyttää rajat vahvistusta, hyper-parametrin sweeping, ja mallin arviointi. 

**Tietojen tarkasteluun ja arvioiminen ohjattu:** Tutustu dataset ja luoda, pisteet, ja arvioida konepohjaisten oppimistekniikoiden mallien suorittamalla [binaarinen luokittelu ja ohjattu MLlib työkalujen tietojen regression mallien luominen](machine-learning-data-science-spark-data-exploration-modeling.md) aiheen kautta.

**Malli kulutus:** Lisätietoja sija luotu tämän ohjeaiheen luokittelu ja regression mallit-kohdassa [pisteet ja arvioi ohjattu laadittuihin koneen learning mallien](machine-learning-data-science-spark-model-consumption.md).

**Usean kelpoisuustarkistus ja hyperparameter sweeping**: miten malleja voi nähdä [Advanced tietojen tarkasteluun ja arvioiminen ohjattu](machine-learning-data-science-spark-advanced-data-exploration-modeling.md) koulutus rajat kelpoisuustarkistus ja hyper-parametrin sweeping avulla

