<properties 
    pageTitle="Virta-analyysin avulla voit käsitellä tietoja viety sovelluksen tiedot | Microsoft Azure" 
    description="Virta Analytics voit jatkuvasti transform, suodattaa ja reitittää tiedot sovelluksen tiedot viedään." 
    services="application-insights" 
    documentationCenter=""
    authors="noamben" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/18/2016" 
    ms.author="awills"/>

# <a name="use-stream-analytics-to-process-exported-data-from-application-insights"></a>Virta-analyysin avulla voit käsitellä viedyt tiedot sovelluksen tiedot

[Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) on paras mahdollinen työkalu käsittelyyn [viety hakemuksen tiedot](app-insights-export-telemetry.md). Virta Analytics voit noutaa tietoja useista eri lähteistä. Voit muuntaa ja tietojen suodattaminen ja reitittämiseen eri määrä.

Tässä esimerkissä Luo sovittimen, joka hakee tietoja-sovelluksen tiedot, nimeää ja käsittelee kaikkia kenttiä ja piiput Power BI.

> [AZURE.WARNING] On paljon helpompaa ja parantaminen [Suositeltavia tapoja Power BI-sovelluksen tiedot-tietojen näyttämiseen](app-insights-export-power-bi.md). Oheisen polku on vain esimerkki esitä käsitteleminen viedyille tiedoille.

![Vie SA PBI – Lohkokaavio](./media/app-insights-export-stream-analytics/020.png)


## <a name="create-storage-in-azure"></a>Tallennustilan luominen Azure

Jatkuva Vie tulostaa aina Azure-tallennustilan tilin tietoja, joten sinun täytyy luoda muistitilaa ensin.

1.  "Perinteinen-tallennustilan tilin luominen [Azure portal](https://portal.azure.com)-tilaukseesi.

    ![Azure-portaalissa Valitse uusi, tiedot, tallennustila](./media/app-insights-export-stream-analytics/030.png)

2. Luoda säilön

    ![Valitse uusi tallennustilan säiliöiden ja valitse säilöt-ruutu, ja valitse Lisää](./media/app-insights-export-stream-analytics/040.png)

3. Kopioi tallennustilan pikanäppäin

    Tarvitset sitä pian voit määrittää syötteen stream analytics-palveluun.

    ![Muistiin asetukset-näppäimiä, avaaminen ja ohjelmaan Access perusavaimen kopio](./media/app-insights-export-stream-analytics/045.png)

## <a name="start-continuous-export-to-azure-storage"></a>Käynnistä jatkuva Vie Azure-tallennustilan

Hakemuksen tiedot Azure varastoon [Jatkuva Vie](app-insights-export-telemetry.md) siirtää tietoja.

1. Siirry Azure-portaalissa loit sovelluksen hakemuksen tiedot resurssin.

    ![Selaa-sovelluksen tiedot-sovellus](./media/app-insights-export-stream-analytics/050.png)

2. Luo jatkuva Vie.

    ![Valitse asetukset-jatkuva Vie lisääminen](./media/app-insights-export-stream-analytics/060.png)


    Valitse aiemmin luomasi tallennustilan-tili:

    ![Määritä viennin kohde](./media/app-insights-export-stream-analytics/070.png)
    
    Määritä haluamasi tapahtumatyypit:

    ![Valitse tapahtumatyypit](./media/app-insights-export-stream-analytics/080.png)

3. Anna joidenkin tietojen keräämistä. Istut takaisin ja salliminen käyttää sovelluksen jonkin aikaa. Telemetriatietojen toimitetaan ja näet tilastokaavion [metrisillä](app-insights-metrics-explorer.md) Resurssienhallinnassa ja yksittäiset tapahtumat [Diagnostiikan haku](app-insights-diagnostic-search.md). 

    Ja -tallennustilan Vie tiedot. 

4. Tarkasta viedyille tiedoille. Valitse Visual Studion **tarkastella / Cloud Explorer**, ja avaa Azure / tallennustilan. (Jos tämä vaihtoehto ei ole, sinun täytyy asentaa Azure SDK: Avaa uusi projekti-valintaikkuna ja Avaa Visual C# / Cloud / hakeminen Microsoft Azure SDK .NET.)

    ![](./media/app-insights-export-stream-analytics/04-data.png)

    Merkitse muistiin polun nimi, joka johdetaan nimi- ja sovellusnäppäintä Yleiset-osan. 

Tapahtumat kirjoitetaan blob-tiedostoja JSON-muodossa. Kunkin tiedostossa voi olla yksi tai useampi tapahtumat. Niin haluamme lukea tapahtumatietoja ja suodattaa pois haluamme kentät. On kaikenlaisten emme voi käyttää tietoja, mutta Microsoftin suunnitelman tänään on käytettävä Stream Analytics voit tulostaa tiedot Power BI.

## <a name="create-an-azure-stream-analytics-instance"></a>Azure Stream Analytics luominen

[Perinteinen Azure-portaalin](https://manage.windowsazure.com/)ja Azure Stream Analytics-palvelu ja luo uusi Stream Analytics työ:


![](./media/app-insights-export-stream-analytics/090.png)



![](./media/app-insights-export-stream-analytics/100.png)

Kun uusi projekti on luotu, Laajenna sen tiedot:

![](./media/app-insights-export-stream-analytics/110.png)


### <a name="set-blob-location"></a>Määritä Blob-objektien sijainti

Määritä toteuttamaan syötteen jatkuva Vie-blob:

![](./media/app-insights-export-stream-analytics/120.png)

Nyt tarvitset tallennustilan-tililtä, jonka kirjoittamasi Access perusavain. Määritä tallennustilan tilin avaimeksi.

![](./media/app-insights-export-stream-analytics/130.png)

### <a name="set-path-prefix-pattern"></a>Määritä polku etuliitemalli 

![](./media/app-insights-export-stream-analytics/140.png)


**Muista päivämäärämuodon asettaminen YYYY-MM-DD (ja katkoviivat).**

Polku etuliite malli määrittää, missä muodossa Analytics löytää syötteen tiedostoja muistiin. Sinun täytyy määrittää sen vastaamaan miten jatkuva Vie tallentaa tiedot. Määritä tältä:

    webapplication27_12345678123412341234123456789abcdef0/PageViews/{date}/{time}

Tässä esimerkissä:

* `webapplication27`on hakemuksen tiedot resurssin **kaikki pieniksi kirjaimiksi**.
* `1234...`on hakemuksen tiedot resurssin **pois katkoviivat**instrumentation-näppäintä. 
* `PageViews`on tietotyypin haluat analysoida. Saatavilla olevat tyypit määräytyvät sen mukaan, mitä voit määrittää jatkuva Vie suodatin. Voit tarkastella muita käytettävissä olevia tyyppejä viedyille tiedoille tarkastelee ja tuo näkyviin [tietomallin Vie](app-insights-export-data-model.md).
* `/{date}/{time}`kirjoitetaan kuvion literaaleina.

> [AZURE.NOTE] Tarkasta tallennustilan varmistaaksesi, että saat polku oikealle.

### <a name="finish-initial-setup"></a>Ensimmäinen asennus valmis

Vahvista Sarjatoiminto muoto:

![Vahvista ja sulje ohjattu toiminto](./media/app-insights-export-stream-analytics/150.png)

Sulje ohjattu toiminto ja odota, Viimeistele määritys.

> [AZURE.TIP] Esimerkki-komennon avulla voit ladata tietoja. Säilyttää sen näyte korjaamisessa kyselyn.

## <a name="set-the-output"></a>Määritä tulos

Nyt työtäsi ja määritä tulos.

![Valitse uuden kanavan, valitse tulostus, Lisää Power BI](./media/app-insights-export-stream-analytics/160.png)

Anna **työpaikan tai oppilaitoksen tilin** sallivat Stream Analytics käyttämään Power BI-resurssia. Valitse varasto tulostukseen ja kohde Power BI-tietojoukko ja taulukon nimi.

![Varaston kolme nimet](./media/app-insights-export-stream-analytics/170.png)

## <a name="set-the-query"></a>Kyselyn määrittäminen

Kyselyn ohjaa syötetyistä siirrettävät käännös.

![Valitse projekti ja valitse kysely. Liitä alla malli.](./media/app-insights-export-stream-analytics/180.png)


Testi-funktion avulla voit tarkistaa, että saat oikealle output. Anna esimerkkitiedot, ottamiasi syötteiden-sivulta. 

### <a name="query-to-display-counts-of-events"></a>Kyselyn, joka näyttää tapahtumien määrä

Liitä tämä kysely:

```SQL

    SELECT
      flat.ArrayValue.name,
      count(*)
    INTO
      [pbi-output]
    FROM
      [export-input] A
    OUTER APPLY GetElements(A.[event]) as flat
    GROUP BY TumblingWindow(minute, 1), flat.ArrayValue.name
```

* Vie-syöte on on antanut syötteen stream alias
* pbi – tulos on määritimme tulostus-alias
* Käytämme [ULOMPI käyttää GetElements](https://msdn.microsoft.com/library/azure/dn706229.aspx) sillä sisäkkäisiä JSON-arrray tapahtuman nimi. Valitse Valitse valitsee tapahtuman nimi-ja aika-asteikon samanniminen esiintymien määrä. [Group By](https://msdn.microsoft.com/library/azure/dn835023.aspx) -lauseeseen ryhmien elementit ajanjaksoon 1 minuutin.


### <a name="query-to-display-metric-values"></a>Kyselyn metrisillä arvojen näyttämiseen


```SQL

    SELECT
      A.context.data.eventtime,
      avg(CASE WHEN flat.arrayvalue.myMetric.value IS NULL THEN 0 ELSE  flat.arrayvalue.myMetric.value END) as myValue
    INTO
      [pbi-output]
    FROM
      [export-input] A
    OUTER APPLY GetElements(A.context.custom.metrics) as flat
    GROUP BY TumblingWindow(minute, 1), A.context.data.eventtime

``` 

* Tämä kysely liittyvä kyselyjä arvot telemetriatietojen tapahtuma-aika ja kustannusarvo. Metrijärjestelmän arvot eivät sisällä matriisin, joten Käytämme ULOMPI käyttää GetElements kuvion Pura rivit. "myMetric" on tässä tapauksessa nimi arvo. 

### <a name="query-to-include-values-of-dimension-properties"></a>Kyselyn dimension ominaisuuksien arvot

```SQL

    WITH flat AS (
    SELECT
      MySource.context.data.eventTime as eventTime,
      InstanceId = MyDimension.ArrayValue.InstanceId.value,
      BusinessUnitId = MyDimension.ArrayValue.BusinessUnitId.value
    FROM MySource
    OUTER APPLY GetArrayElements(MySource.context.custom.dimensions) MyDimension
    )
    SELECT
     eventTime,
     InstanceId,
     BusinessUnitId
    INTO AIOutput
    FROM flat

```

* Tämä kysely on kiinteä indeksissä dimensio-matriisissa on tietyn dimension mukaan ilman ominaisuuksien dimension arvot.

## <a name="run-the-job"></a>Suorita työ

Voit valita päivämäärän menneisyydessä työn käynnistämiseen. 

![Valitse projekti ja valitse kysely. Liitä alla malli.](./media/app-insights-export-stream-analytics/190.png)

Odota, kunnes työ on käynnissä.

## <a name="see-results-in-power-bi"></a>Hakutuloksia Power BI

> [AZURE.WARNING] On paljon helpompaa ja parantaminen [Suositeltavia tapoja Power BI-sovelluksen tiedot-tietojen näyttämiseen](app-insights-export-power-bi.md). Oheisen polku on vain esimerkki esitä käsitteleminen viedyille tiedoille.

Avaa Power BI työpaikan tai oppilaitoksen tilillä ja valitse tietojoukko ja taulukon, jonka Stream Analytics-työ tulosteen määritelty.

![Valitse Power BI-tietojoukko ja kentät.](./media/app-insights-export-stream-analytics/200.png)

Voit nyt käyttää tätä tietojoukko raporttien ja raporttinäkymien [Power](https://powerbi.microsoft.com)BI.


![Valitse Power BI-tietojoukko ja kentät.](./media/app-insights-export-stream-analytics/210.png)



## <a name="no-data"></a>Tietoja?

* Tarkista, että voit [määrittää päivämäärämuodon](#set-path-prefix-pattern) oikein YYYY-MM-DD (ja katkoviivat).


## <a name="video"></a>Video

Noam Ben Zeev esitetään, kuinka voit käsitellä käyttämällä Stream Analytics viedyille tiedoille.

> [AZURE.VIDEO export-to-power-bi-from-application-insights]

## <a name="next-steps"></a>Seuraavat vaiheet

* [Jatkuva vienti](app-insights-export-telemetry.md)
* [Yksityiskohtaisia tietoja mallin viittaus ominaisuuden tiedostotyypit ja -arvoja.](app-insights-export-data-model.md)
* [Hakemuksen tiedot](app-insights-overview.md)
* [Lisää näytteiden ja vaihe vaiheelta](app-insights-code-samples.md)
 
