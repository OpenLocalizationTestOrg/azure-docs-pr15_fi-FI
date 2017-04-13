<properties
    pageTitle="Mukautetun koontinäytön luominen Log Analytics | Microsoft Azure"
    description="Tämän oppaan avulla voit selvittää, kuinka Log Analytics raporttinäkymien voit visualisointi kaikkiin tallennetun lokin hakua antamalla yhden linssin tarkastelemaan ympäristön."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="banders"/>

# <a name="create-a-custom-dashboard-in-log-analytics"></a>Luo mukautettu Raporttinäkymät-ikkunan Log Analytics

Tämän oppaan avulla ymmärtää, kuinka loki Analytics raporttinäkymien voit visualisointi kaikkiin tallennetun lokin hakua sinun on yksittäinen linssin tarkastelemaan ympäristön.

![Esimerkki Raporttinäkymät-ikkunan](./media/log-analytics-dashboards/oms-dashboards-example-dash.png)

Kaikki mukautetut raporttinäkymiä, jotka luot OMS portaalissa ovat käytettävissä myös OMS Mobile-sovelluksesta. Katso lisätietoja sovellukset seuraavilla sivuilla.

- [Microsoft-Storesta OMS mobiilisovelluksessa](http://www.windowsphone.com/store/app/operational-insights/4823b935-83ce-466c-82bb-bd0a3f58d865)
- [Apple itunesista OMS mobiilisovelluksessa](https://itunes.apple.com/app/microsoft-operations-management/id1042424859?mt=8)

![Mobiili-koontinäyttö](./media/log-analytics-dashboards/oms-search-mobile.png)

## <a name="how-do-i-create-my-dashboard"></a>Miten voin luoda Dashboard-sivulla?

Jos haluat aloittaa, siirry OMS Yhteenveto-sivulla. Näyttöön tulee **Oma Dashboard** -ruudun vasemmassa reunassa. Voit siirtyä raporttinäkymän alirakenteeseen napsauttamalla sitä.

![Yleiskatsaus](./media/log-analytics-dashboards/oms-dashboards-overview.png)


## <a name="adding-a-tile"></a>Ruudun lisääminen

Raporttinäkymät-ruutujen muistiin tallennettu loki hakuun. OMS sisältyy useita tehneet valmiiksi tallennettu loki haut, joten voit aloittaa saman tien. Noudata seuraavia ohjeita, miten voit aloittaa jäsentää.

Oma Dashboard-näkymän yksinkertaisesti napsauttamalla **Mukauta** antamaan Mukauta tilaa.

![Kaupallista](./media/log-analytics-dashboards/oms-dashboards-pictorial01.png)

 Ohjauspaneelin, joka avaa sivun oikeassa reunassa näyttää kaikkien oman työtilan tallennettu loki haut. Visualisoimiseen tallennettu loki haun ruuduksi tallennetun haun päälle ja valitse sitten **plus** -merkkiä.

![Ruutujen 1 lisääminen](./media/log-analytics-dashboards/oms-dashboards-pictorial02.png)

Kun napsautat **plus** -merkkiä, uusi ruutu näkyy Omat raporttinäkymät-näkymässä.

![Ruutujen 2 lisääminen](./media/log-analytics-dashboards/oms-dashboards-pictorial03.png)


## <a name="edit-a-tile"></a>Ruudun muokkaaminen

Oma Raporttinäkymät-ikkunan näkymässä yksinkertaisesti valitsemalla **Mukauta** antamaan Mukauta tilaa. Napsauta ruutua, jota haluat muokata. Oikeanpuoleisessa muutokset haluat muokata, ja antaa valikoiman asetukset:

![Ruudun muokkaaminen](./media/log-analytics-dashboards/oms-dashboards-pictorial04.png)

![Ruudun muokkaaminen](./media/log-analytics-dashboards/oms-dashboards-pictorial05.png)

### <a name="tile-visualizations"></a>Ruudun visualisoinnit#
Ruutu visualisointeja valittavana kolme erilaista:

|kaaviolaji|kuvaus|
|---|---|
|![Palkkikaavio](./media/log-analytics-dashboards/oms-dashboards-bar-chart.png)|Näyttää tallennetun lokin etsinnän tulokset aikajana palkkikaavio tai tulosluettelon riippuen kentän mukaan, jos lokiin haun koostaa kentän tulokset vai ei.
|![arvo](./media/log-analytics-dashboards/oms-dashboards-metric.png)|Näyttää lokitiedoston haku-tulos-osumien ruutujen numero. Metrijärjestelmän ruutujen avulla voit määrittää raja-arvon, joka korostaa ruudun kun raja-arvo on saavutettu.|
|![rivi](./media/log-analytics-dashboards/oms-dashboards-line.png)|Näyttää aikajanan tallennettu loki haun tulokset osumien arvoilla viivakaavio.|

### <a name="threshold"></a>Raja-arvo
Voit luoda raja-arvon käyttämällä metrisillä visualisoinnin-ruutu. Valitse Luo raja-arvo-ruutu. Valitse Korosta ruutu, kun arvo on päällä tai valittua raja-kohdassa ja valitse Määritä raja-arvo.

## <a name="organizing-the-dashboard"></a>Koontinäytön järjestäminen
**Mukauta** antamaan Mukauta järjestää raporttinäkymän, siirry oma Dashboard-näkymä ja valitse tilaa. Napsauta ja vedä ruutu, jonka haluat siirtää, ja siirrä se haluamaasi yhteyttä ruutu on.

![Raporttinäkymän järjestäminen](./media/log-analytics-dashboards/oms-dashboards-organize.png)

## <a name="remove-a-tile"></a>Ruudun poistaminen
Haluat poistaa ruudun, siirry oma Dashboard-näkymä ja valitse **Mukauta** antamaan Mukauta tilaa. Valitse ruutu, jonka haluat poistaa ja valitse oikeassa paneelissa **Poista vierekkäin**.

![Ruudun poistaminen](./media/log-analytics-dashboards/oms-dashboards-remove-tile.png)

## <a name="next-steps"></a>Seuraavat vaiheet

- Luo [ilmoituksia](log-analytics-alerts.md) Log Analytics Luo ilmoituksia ja riskien parantaminen ongelmia.
