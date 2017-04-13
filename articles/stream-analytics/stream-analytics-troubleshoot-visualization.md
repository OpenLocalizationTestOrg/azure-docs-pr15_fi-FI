<properties
    pageTitle="Visualisointi ja Stream Analytics työt vianmääritys | Microsoft Azure"
    description="Opettele visualisointi Stream Analytics työn putkijohto Omatoiminen vianmäärityksen diagnostiikka kaavio-toiminnon avulla."
    keywords=""
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


# <a name="visualize-and-troubleshoot-stream-analytics-jobs"></a>Visualisointi ja Stream Analytics työt vianmääritys

Virta Analytics muiden pilvipohjainen-tekniikoiden kanssa vianmääritys joskus tarvitaan tarkastelevat miksi työn tuota Odotettu tulos (tai mitä tahansa aihetta tulosteen). Tässä yhteydessä Stream Analytics on varten kesäolympialaisten visualisointi streaming projektin ominaisuuksien. Tämä on myös kätevä kuin mallinnus työkalun ja ne edellyttävät dokumentaatio työssään puoli hyötyä.

Visualisoinnin Ohjauspaneelin syötteiden näkyvät sekä suoritettava kysely ja valitse kaikki tulostaa määritetty. Yhteyden tai määritysten ongelmat, joskus voi olla useampi näennäinen ja se voi olla hyötyä myös Nähdäksesi kokoonpanosi visuaalisessa muodossa.

## <a name="using-the-diagnosis-diagram-tool"></a>Vianmäärityksen kaavio-työkalun avulla

Voit käyttää tätä visualizer, valitsemalla "Asetukset"-sivu "Vianmäärityksen kaavion"-painiketta Stream Analytics työn.

![Stream-Analytics-Troubleshoot-visualization-diagnosis-Diagram](./media/stream-analytics-troubleshoot-visualization/stream-analytics-troubleshoot-visualization-diagnosis-diagram1.png)

Kaikissa syötteen ja tulos on kyseisen osan väreillä osoittamaan nykyisen tilan alla kuvatulla tavalla.

![Stream-Analytics-Troubleshoot-visualization-Input-Map](./media/stream-analytics-troubleshoot-visualization/stream-analytics-troubleshoot-visualization-input-map.png)

Kun käyttäjä haluaa tarkastella keskitason kyselyvaiheita tietojen kulun kuviot sisällä työn ymmärtää, visualisointi-työkalun avulla kyselyn hajautuksen näyttää sen osan vaiheet ja työnkulku-järjestyksessä. Jokainen kyselyvaihe valitsemalla Näytä vastaavaan muokkaaminen ruudussa kuten kyselyn. 

![Stream-Analytics-Troubleshoot-visualization-intermediate-steps](./media/stream-analytics-troubleshoot-visualization/stream-analytics-troubleshoot-visualization-intermediate-steps.png)




## <a name="next-steps"></a>Seuraavat vaiheet

- [Azure Stream Analytics esittely](stream-analytics-introduction.md)
- [Azure Stream Analytics käytön aloittaminen](stream-analytics-get-started.md)
- [Skaalaa Azure Stream Analytics töitä](stream-analytics-scale-jobs.md)
- [Azure Stream Analytics kyselyn kieliohje](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics hallinta REST API-viittaus](https://msdn.microsoft.com/library/azure/dn835031.aspx)
