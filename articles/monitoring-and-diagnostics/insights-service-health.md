<properties
    pageTitle="Seurata Azure palvelun kunnon Azure näytön lokeja | Microsoft Azure"
    description="Ota selvää, kun Azure on ilmennyt suorituskyvyn heikkeneminen tai palvelun keskeytyksiä. "
    authors="rboucher"
    manager="carolz"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="robb"/>

# <a name="track-azure-service-health-using-azure-monitor-activity-logs"></a>Seurata Azure palvelun kunnon Azure näytön lokeja

Azure publicizes aina, kun on palvelun keskeytymisen tai suorituskyvyn heikkeneminen. Voit selata näitä tapahtumia Azure-portaalissa, ja voit käyttää [REST API](https://msdn.microsoft.com/library/azure/dn931927.aspx) tai [.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Insights/) myös käyttää kaikkia käytettävissä olevia tapahtumien ohjelmallisesti.

## <a name="browse-the-service-health-logs-for-your-subscription"></a>Selaa tilauksen palvelun kunto-lokit

1. Kirjautuminen [Azure Portal](https://portal.azure.com/).

2. Valitse **Aloitus** näkyy ruutu, jota kutsutaan **palvelun kunto**. Napsauta sitä.

    ![Aloitus](./media/insights-service-health/Insights_Home.png)

3. Näet luettelon Azure kaikkien alueiden. Valitse alue, kun kosketat tehtävän Log-kysely, joka näyttää palvelutapahtumia, jotka ovat vaikuttaneet kaikista tilauksistasi viimeisen 24 tunnin aikana.

    ![Tehtävän Log tilauksen palvelun kunto](./media/insights-service-health/AzureActivityLogServiceHealth3.png)

4. Näet yksittäisen tapahtuma tiedot napsauttamalla taulukon tapahtumaan.

5. Voit muuttaa **aikajakson** Nähdäksesi pidempi hyväksyminen.

## <a name="next-steps"></a>Seuraavat vaiheet

* [Näytön käytettävyys- ja minkä tahansa verkkosivun vasteaikaa](../application-insights/app-insights-monitor-web-app-availability.md) sovelluksen tiedot, jotta voit tarkistaa, jos sivu on poissa käytöstä.
