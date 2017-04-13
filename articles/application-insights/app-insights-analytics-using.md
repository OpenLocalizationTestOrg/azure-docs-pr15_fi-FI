<properties 
    pageTitle="Analytics - sovelluksen tiedot tehokas haku-työkalun avulla | Microsoft Azure" 
    description="Voit käyttää Analytics-sovelluksen tiedot tehokkaita diagnostiikan hakutyökalun. " 
    services="application-insights" 
    documentationCenter=""
    authors="danhadari" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/21/2016" 
    ms.author="awills"/>


# <a name="using-analytics-in-application-insights"></a>Hakemuksen tiedot Analytics käyttäminen


[Analyysin](app-insights-analytics.md) on tehokas haku-toiminnon [Hakemuksen tiedot](app-insights-overview.md). Nämä sivut kuvaavat Analytics-kyselykieltä.

* **[Johdanto videossa](https://applicationanalytics-media.azureedge.net/home_page_video.mp4)**.
* Jos sovellus ei ole lähettää tiedot sovelluksen havainnollistamisen vielä **[Kokeile Analytics-Simuloitu tietojamme](https://analytics.applicationinsights.io/demo)** .

## <a name="open-analytics"></a>Avoinna Analytics

Napsauta sinua sovelluksen koti resurssin sovelluksen tiedot-analyysin.

![Avaa portal.azure.com, Avaa sovellus havainnollistamisen resurssi ja valitse Analytics.](./media/app-insights-analytics-using/001.png)

Sisäinen opetusohjelman tutustutaan joitakin ideoita toiminnot.

Tällä [tähän laajemmat esittely](app-insights-analytics-tour.md).

## <a name="query-your-telemetry"></a>Kyselyn lisääminen telemetriatietojen

### <a name="write-a-query"></a>Kirjoita kyselyn

![Rakenne-näyttö](./media/app-insights-analytics-using/150.png)

Alkavat luettelossa vasemmalle (tai [alueen](app-insights-analytics-reference.md#range-operator) tai [union](app-insights-analytics-reference.md#union-operator) -operaattorit) taulukoiden nimiin. Käytä `|` [toimijoiden](app-insights-analytics-reference.md#queries-and-operators)putkijohto luomiseen. IntelliSense kehottaa operaattorit ja osa, jota voit käyttää lausekkeen osat.

Katso [Analytics kielen yleiskatsaus](app-insights-analytics-tour.md) ja [viittaus language](app-insights-analytics-reference.md).

### <a name="run-a-query"></a>Kyselyn suorittaminen

![Kyselyn suorittaminen](./media/app-insights-analytics-using/130.png)

1. Voit käyttää yhden rivin kyselyn.
2. Aseta kohdistin sisällä tai jonka haluat suorittaa kyselyn lopussa.
3. Valitse Siirry Suorita kysely.
4. Kyselyn ei pidä tyhjiä rivejä. Voit pitää useiden erillisten kyselyjen yhden kysely-välilehdessä erottamalla ne tyhjiä rivejä. Kun kohdistin yksi suoritetaan.

### <a name="save-a-query"></a>Tallenna kysely

![Tallentaminen kyselynä](./media/app-insights-analytics-using/140.png)

1. Tallentaa nykyisen kyselytiedoston.
2. Avaa tallennettu kysely-tiedosto.
3. Luo uusi kyselytiedosto.


## <a name="see-the-details"></a>Katso sen tiedot

Laajenna kaikki rivi tuloksissa nähdäksesi sen luettelo kaikista ominaisuudet. Voit laajentaa edelleen minkä tahansa ominaisuus, joka on rakenteellisia - arvo, esimerkiksi mukautettuja dimensioita ja luetteloa poikkeuksen pinon.

![Laajenna rivi](./media/app-insights-analytics-using/070.png)

 

## <a name="arrange-the-results"></a>Järjestää tulokset

Voit lajitella, suodattaa, sivuta ja ryhmitellä kyselyn palauttamien tulokset.

> [AZURE.NOTE] Lajittelu, ryhmittely ja suodattaminen selaimessa ei suorita kysely uudelleen. Vain ne järjestää uudelleen, jotka on palautettu viimeisen kyselyn tulokset. 
> 
> Näiden tehtävien suorittamiseen palvelimeen, ennen kuin tulokset palautetaan, Kirjoita kyselyn [lajitteleminen](app-insights-analytics-reference.md#sort-operator), [yhteenvedon](app-insights-analytics-reference.md#summarize-operator) ja [jossa](app-insights-analytics-reference.md#where-operator) operaattoreita.

Valitse sarakkeet, jotka haluat nähdä, sarakeotsikoiden avulla järjestystä vetämällä ja sarakkeiden kokoa vetämällä niiden reunat.

![Sarakkeiden järjestäminen](./media/app-insights-analytics-using/030.png)

### <a name="sort-and-filter-items"></a>Kohteiden lajittelu ja suodatus

Lajittele tulokset napsauttamalla sarakkeen otsikko. Lajittele tavalla napsauttamalla uudelleen ja valitse kolmannen, kun haluat palata alkuperäiseen järjestys kyselyn palauttamien.

Suodatinkuvakkeen avulla voit tarkentaa hakua.

![Lajittele ja Suodata-sarakkeet](./media/app-insights-analytics-using/040.png)



### <a name="group-items"></a>Ryhmittelyperuste

Jos haluat lajitella useita sarakkeita, käytä ryhmittely. Ensin otettava ja vedä sarakeotsikoiden taulukon yläpuolella olevaa tilaa.

![Ryhmän](./media/app-insights-analytics-using/060.png)



### <a name="missing-some-results"></a>Puuttuu joitakin tuloksia?

Ei-portaalista palautettujen tulosten noin 10 k rivien enimmäismäärä. Ilmoitus näyttää, jos siirryt ylitetty. Jos näin tapahtuu, lajitteleminen taulukossa tulokset eivät aina näyttää kaikki todellinen ensimmäisen tai viimeisen tulokset. 

Se on hyvä välttää pallolla rajoitus. Käytä operaattoreita, kuten:

* [Jos aikaleima > ago(3d)](app-insights-analytics-reference.md#where-operator)
* [Ensimmäiset 100 aikaleima mukaan](app-insights-analytics-reference.md#top-operator) 
* [Ota 100](app-insights-analytics-reference.md#take-operator)
* [Yhteenveto](app-insights-analytics-reference.md#summarize-operator) 



## <a name="diagrams"></a>Kaaviot

Valitse kaaviotyyppi, kuten:

![Valitse kaaviotyyppi](./media/app-insights-analytics-using/230.png)

Jos sinulla on useita sarakkeita oikealle tietuetyypeistä, voit valita x ja y-akselit ja sarakkeen dimensioiden Jaa tulokset mukaan.

Oletusarvon mukaan tulokset näytetään aluksi taulukoksi ja valitset kaavio manuaalisesti. Mutta voit tehdä [hahmonnetaan direktiivin](app-insights-analytics-reference.md#render-directive) kyselyn lopussa Valitse kaavio.

## <a name="pin-to-dashboard"></a>Raporttinäkymät-ikkunan kiinnittäminen

Voit kiinnittää kaavion tai taulukon yhteen [jaettuun raporttinäkymät](app-insights-dashboards.md) - vain valitsemalla PIN-tunnuksen. (Voit joutua [päivityksen sovelluksen käyttäjän hinnat paketti](app-insights-pricing.md) , voit ottaa tämän toiminnon.) 

![Valitse PIN-tunnus](./media/app-insights-analytics-using/pin-01.png)

Tämä tarkoittaa, että kun lisäät yhdessä Raporttinäkymät-ikkunan avulla voit valvoa suorituskykyä tai WWW-palveluihin, voit lisätä varsin monimutkainen analyysi muita tietoja: n rinnalla. 

Voit kiinnittää taulukon koontinäyttö, jos siinä on neljä tai vähemmän sarakkeet. Vain seitsemän Ylimmät rivit näkyvät.


#### <a name="dashboard-refresh"></a>Raporttinäkymät-ikkunan päivittäminen

Koontinäytön kiinnitettyinä kaavio päivittyy automaattisesti uudelleen suorittamalla kyselyn noin puolessa tunnissa.

#### <a name="automatic-simplifications"></a>Automaattinen yksinkertaistaminen

Joissakin tapauksissa tiettyjen yksinkertaistaminen niitä käytetään kaavion, kun kiinnität sen Raporttinäkymät-ikkunan.

Kun Kiinnitä kaavio, jossa on paljon erilliset palkkien (yleensä palkkikaavion) vähemmän täyttää palkkien automaattisesti ryhmitetty yhden "muut" palkin. Esimerkiksi tämä kysely

    requests | summarize count_search = count() by client_CountryOrRegion

Analytics näyttää seuraavanlaiselta:


![Kaavio, jossa on harvinaisissa](./media/app-insights-analytics-using/pin-07.png)

mutta kun kiinnität raporttinäkymän, se näyttää tältä:


![Kaavio, jossa on rajoitettu palkkien](./media/app-insights-analytics-using/pin-08.png)




## <a name="export-to-excel"></a>Tietojen vieminen Exceliin

Kun suoritat kyselyn, voit ladata .csv-tiedoston. Valitse **Vie Exceliin**.

## <a name="export-to-power-bi"></a>Power BI vieminen

Siirrä kohdistin kysely ja valitse **Vie Power BI**.

![Power BI Analytics vieminen](./media/app-insights-analytics-using/240.png)

Power BI: n Suorita kysely. Voit määrittää sen päivittämiseen aikataulun.

Voit luoda Power BI-koontinäytöt, joka yhdistää tietoja useista eri lähteistä.


[Lisätietoja Power BI vieminen](app-insights-export-power-bi.md)



[AZURE.INCLUDE [app-insights-analytics-footer](../../includes/app-insights-analytics-footer.md)]

