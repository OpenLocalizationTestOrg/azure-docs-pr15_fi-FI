<properties
    pageTitle="Johdanto Stream Analytics-ikkunassa Funktiot | Microsoft Azure"
    description="Tietoja kolmen ikkunan Funktiot, Stream Analytics (tumbling hopping, liukuva)."
    keywords="tumbling-ikkunassa liukuva-ikkunassa hopping ikkuna"
    documentationCenter=""
    services="stream-analytics"
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"
/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok"
/>


# <a name="introduction-to-stream-analytics-window-functions"></a>Johdanto Stream Analytics-ikkunassa Funktiot

Reaaliaikaisesti monta streaming tilanteita, joissa on Suorita vain ajallinen windows sisältämät tiedot. Tuki windowing funktiot on Azure Stream Analytics, joka siirtää neulan tuottavuutta-yhteiskäyttö KOMPLEKSI stream töitä avaimen ominaisuus. Virta Analytics kehittäjät voivat suorittaa ajallinen streaming tietojen [**Tumbling**](https://msdn.microsoft.com/library/dn835055.aspx), [**Hopping**](https://msdn.microsoft.com/library/dn835041.aspx) ja [**liukuovia**](https://msdn.microsoft.com/library/dn835051.aspx) Windowsin avulla. On otettava huomioon, että [kaikki ikkunatoiminnot](https://msdn.microsoft.com/library/dn835019.aspx) tulosteen tulokset ikkunan **loppuun** . Ikkunan tulos on tarkasteltavat koostefunktio, jota käytetään perusteella. Tapahtuma on ikkunan loppuun aikaleima ja kaikki ikkunan funktiot on määritetty kiinteä pituus. Lopuksi on tärkeää muistaa, että kaikki ikkunan Funktiot käyttää [**GROUP BY**](https://msdn.microsoft.com/library/dn835023.aspx) -lause.

![Virta Analytics-ikkunassa Funktiot käsitteitä](media/stream-analytics-window-functions/stream-analytics-window-functions-conceptual.png)

## <a name="tumbling-window"></a>Tumbling-ikkuna

Tumbling ikkuna tietovirta määritetään eri aika osiin ja suorittaa toiminnon vastaan, kuten seuraavassa esimerkissä funktioita käytetään. Tumbling-ikkunan tärkeimmät differentiators on, että ne toista-ikkunoihin ja tapahtuman ei voi kuulua useita tumbling-ikkuna.

![Virta Analytics-ikkunassa Funktiot tumbling esittely](media/stream-analytics-window-functions/stream-analytics-window-functions-tumbling-intro.png)

## <a name="hopping-window"></a>Hopping ikkuna

Hopping ikkunan Funktiot siirräntävälien eteenpäin verran ajaksi. Lähettää ajatella ne Tumbling ikkunoita, jotka päällekkäin, joten tapahtumat voivat kuulua useita Hopping-ikkunassa tulosjoukko on helppoa. Jotta Hopping ikkunan sama kuin Tumbling yhden ikkunan määrittämällä kohteen koon sama kuin ikkunakoon. 

![Virta Analytics-ikkunassa Funktiot hopping esittely](media/stream-analytics-window-functions/stream-analytics-window-functions-hopping-intro.png)

## <a name="sliding-window"></a>Liukuva ikkuna

Liukuva ikkuna-Funktiot, toisin kuin Tumbling tai Hopping windows tuottaa-tulostus **vain** tapahtuman yhteydessä. Kaikkien ikkunoiden on aina oltava vähintään yksi tapahtuma ja ikkunan jatkuvasti siirtää eteenpäin € (Epsilonin). Kuten Hopping Windowsin tapahtumien voivat kuulua useamman kuin yhden liukuva ikkunan.

![Virta Analytics-ikkunassa Funktiot liukuva esittely](media/stream-analytics-window-functions/stream-analytics-window-functions-sliding-intro.png)

## <a name="getting-help-with-window-functions"></a>Ohjeiden saaminen ikkunan Funktiot

Saat lisäohjeita yritä Microsoftin [Azure Stream Analytics-keskustelupalsta](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Seuraavat vaiheet

- [Azure Stream Analytics esittely](stream-analytics-introduction.md)
- [Azure Stream Analytics käytön aloittaminen](stream-analytics-get-started.md)
- [Skaalaa Azure Stream Analytics töitä](stream-analytics-scale-jobs.md)
- [Azure Stream Analytics kyselyn kieliohje](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics hallinta REST API-viittaus](https://msdn.microsoft.com/library/azure/dn835031.aspx)
