<properties
    pageTitle="Virtauttaa Azure vianmäärityslokit tapahtuman porttiin | Microsoft Azure"
    description="Opettele virtauttaa Azure vianmäärityslokit tapahtuma-porttiin."
    authors="johnkemnetz"
    manager="rboucher"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/08/2016"
    ms.author="johnkem"/>

# <a name="stream-azure-diagnostic-logs-to-event-hubs"></a>Virtauttaa Azure vianmäärityslokit tapahtuman porttiin

**[Azure vianmäärityslokit](monitoring-overview-of-diagnostic-logs.md)** voidaan lähettää lähelle reaaliajassa minkä tahansa sovelluksen valmiin "Vie haluat tapahtuman keskittimet"-vaihtoehtoa käytettäessä portaalissa tai palvelu-Bus sääntötunnus Azure PowerShellin cmdlet-komennot tai Azure CLI diagnostiikan asetusten ottaminen käyttöön.

## <a name="what-you-can-do-with-diagnostics-logs-and-event-hubs"></a>Miten diagnostiikka-lokit ja tapahtuman keskittimet
Voit käyttää streaming ominaisuuksien vianmäärityslokit todella tavoilla:

- **Virta lokit 3 osapuolen kirjaaminen ja telemetriatietojen järjestelmien** – jaettuina tapahtuman keskittimet streaming muuttuu, jolla pipe oman vianmäärityslokit kolmannen osapuolen SIEMs kyselyjä ja kirjaudu analytics-ratkaisuja.

- **Näytä palvelun kunto mukaan streaming "kuuma polku" tietojen PowerBI** – käyttämällä tapahtuman keskittimet, Stream Analytics ja PowerBI, voit helposti muuntaa diagnostiikka tietojen tuominen lähellä reaaliaikaiset tiedot Azure Services. [Tämän artikkelin ohjeet antaa hyvien yleiskatsaus siitä, miten tapahtuman keskittimet määrittäminen, käsitellä Stream Analytics-tietoja ja käyttää PowerBI kuin tulos](../stream-analytics/stream-analytics-power-bi-dashboard.md). Seuraavassa on muutamia vihjeitä käytön määrittäminen Vianmäärityslokit:
    - Tapahtuman keskittimet luokalle vianmäärityslokit luodaan automaattisesti, kun valitset portaalissa tai käyttöön PowerShellin kautta, joten haluat valita tapahtuman keskittimet palvelun Bus-nimitilan, jonka nimi alkaa merkkijonolla "havainnollistamisen-"
    - Tässä on esimerkki Stream Analytics kyselyn avulla voit jäsentää riittää, että kaikki lokitiedot-PowerBI taulukkoon:

```
SELECT
records.ArrayValue.[Properties you want to track]
INTO
[OutputSourceName – the PowerBI source]
FROM
[InputSourceName] AS e
CROSS APPLY GetArrayElements(e.records) AS records
```

- **Luoda mukautettuja telemetriatietojen ja kirjaaminen ympäristö** – Jos sinulla on custom-built telemetriatietojen platform tai ovat vain pohtivat rakentaminen jokin, erittäin skaalattava Julkaise tilaa laatu tapahtuman keskittimet sallii ingest joustavasti vianmäärityslokit. [Katso Dan Rosanova's opas käyttämällä tapahtuman keskittimet-maailmanlaajuisesti telemetriatietojen ympäristö tähän](https://azure.microsoft.com/documentation/videos/build-2015-designing-and-sizing-a-global-scale-telemetry-platform-on-azure-event-Hubs/).

## <a name="enable-streaming-of-diagnostic-logs"></a>Ota käyttöön streaming vianmäärityslokit.
Voit ottaa streaming, vianmäärityslokit ohjelmallisesti-portaalissa kautta tai käyttämällä [Azure näytön REST API](https://msdn.microsoft.com/library/azure/dn931943.aspx). Kummassakin tapauksessa voit valita palvelun Bus-Namespace ja tapahtuman keskittimet luodaan nimitilaa otat log kussakin luokassa. Diagnostiikan **Log luokka** on lokin, joka resurssilla voi kerätä. Voit valita lokin luokat, jotka haluat kerätä tietyn resurssin Azure-portaalissa, valitse diagnostiikka-sivu.

![Log-luokat-portaalissa](./media/monitoring-stream-diagnostic-logs-to-event-hubs/log-categories.png)

> [AZURE.WARNING] Ottaminen käyttöön ja streaming vianmäärityslokit-resurssit (esimerkiksi VMs tai palvelun kangasta) suorittaminen [edellyttää, että samat vaiheet](../event-hubs/event-hubs-streaming-azure-diags-data.md).

### <a name="via-powershell-cmdlets"></a>Kautta PowerShellin cmdlet-komennot
Jotta streaming kautta [Azure PowerShellin cmdlet-komennot](insights-powershell-samples.md), voit käyttää `Set-AzureRmDiagnosticSetting` näillä parametreilla cmdlet-komento:

```
Set-AzureRmDiagnosticSetting -ResourceId [your resource Id] -ServiceBusRuleId [your service bus rule id] -Enabled $true
```

Palvelun Bus Sääntötunnus on merkkijono, joka sisältää tässä muodossa: `{service bus resource ID}/authorizationrules/{key name}`, esimerkiksi `/subscriptions/{subscription ID}/resourceGroups/Default-ServiceBus-WestUS/providers/Microsoft.ServiceBus/namespaces/{service bus namespace}/authorizationrules/RootManageSharedAccessKey`.


### <a name="via-azure-cli"></a>Azure CLI kautta
Jotta streaming [Azure CLI](insights-cli-samples.md)kautta, voit käyttää `insights diagnostic set` komennon tältä:

```
azure insights diagnostic set --resourceId <resourceId> --serviceBusRuleId <serviceBusRuleId> --enabled true
```

Käytä samassa muodossa palvelun Bus Sääntötunnus kuvatulla tavalla PowerShell cmdlet-komennon.

###<a name="via-azure-portal"></a>Azure portaalin kautta
Jotta streaming Azure-portaalin kautta, siirry diagnostiikan asetusten resurssin ja valitse "Vieminen tapahtumaa-toiminnossa."

![Vie tapahtuman porttiin-portaalissa](./media/monitoring-stream-diagnostic-logs-to-event-hubs/portal-export.png)

Voit määrittää sen, valitsemalla aiemmin luodun palvelun Bus-Namespace. Valittu nimitilan päivitetään, josta tapahtuma keskittimet (jos se on kohdassa ensimmäisen kerran streaming vianmäärityslokit) tai virtauttaa (Jos on jo resurssit, jotka ovat streaming tätä nimitilaa log kyseisen luokan), ja määrittää käytännön, jossa streaming järjestelmä on käyttöoikeudet. Nykyään streaming tapahtuman porttiin edellyttää hallinta-, luku-ja Lähetä. Voit luoda tai muokata palvelun Bus Namespace jaettu käytäntöjen perinteinen portaalissa "Määrittäminen"-välilehdessä Service-Bus Namespace. Voit päivittää yhden diagnostiikan asetuksia, asiakkaan on oltava ListKey käyttöoikeudet palvelun Bus käyttöoikeuksien sääntöä.

##<a name="how-do-i-consume-the-log-data-from-event-hubs"></a>Miten tapahtuman keskittimet lokin tiedot tarjoaman?
Näin tapahtuman keskittimet tulosteen mallitietoja:

```
{
    "records": [
        {
            "time": "2016-07-15T18:00:22.6235064Z",
            "workflowId": "/SUBSCRIPTIONS/DF602C9C-7AA0-407D-A6FB-EB20C8BD1192/RESOURCEGROUPS/JOHNKEMTEST/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/JOHNKEMTESTLA",
            "resourceId": "/SUBSCRIPTIONS/DF602C9C-7AA0-407D-A6FB-EB20C8BD1192/RESOURCEGROUPS/JOHNKEMTEST/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/JOHNKEMTESTLA/RUNS/08587330013509921957/ACTIONS/SEND_EMAIL",
            "category": "WorkflowRuntime",
            "level": "Error",
            "operationName": "Microsoft.Logic/workflows/workflowActionCompleted",
            "properties": {
                "$schema": "2016-04-01-preview",
                "startTime": "2016-07-15T17:58:55.048482Z",
                "endTime": "2016-07-15T18:00:22.4109204Z",
                "status": "Failed",
                "code": "BadGateway",
                "resource": {
                    "subscriptionId": "df602c9c-7aa0-407d-a6fb-eb20c8bd1192",
                    "resourceGroupName": "JohnKemTest",
                    "workflowId": "243aac67fe904cf195d4a28297803785",
                    "workflowName": "JohnKemTestLA",
                    "runId": "08587330013509921957",
                    "location": "westus",
                    "actionName": "Send_email"
                },
                "correlation": {
                    "actionTrackingId": "29a9862f-969b-4c70-90c4-dfbdc814e413",
                    "clientTrackingId": "08587330013509921958"
                }
            }
        },
        {
            "time": "2016-07-15T18:01:15.7532989Z",
            "workflowId": "/SUBSCRIPTIONS/DF602C9C-7AA0-407D-A6FB-EB20C8BD1192/RESOURCEGROUPS/JOHNKEMTEST/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/JOHNKEMTESTLA",
            "resourceId": "/SUBSCRIPTIONS/DF602C9C-7AA0-407D-A6FB-EB20C8BD1192/RESOURCEGROUPS/JOHNKEMTEST/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/JOHNKEMTESTLA/RUNS/08587330012106702630/ACTIONS/SEND_EMAIL",
            "category": "WorkflowRuntime",
            "level": "Information",
            "operationName": "Microsoft.Logic/workflows/workflowActionStarted",
            "properties": {
                "$schema": "2016-04-01-preview",
                "startTime": "2016-07-15T18:01:15.5828115Z",
                "status": "Running",
                "resource": {
                    "subscriptionId": "df602c9c-7aa0-407d-a6fb-eb20c8bd1192",
                    "resourceGroupName": "JohnKemTest",
                    "workflowId": "243aac67fe904cf195d4a28297803785",
                    "workflowName": "JohnKemTestLA",
                    "runId": "08587330012106702630",
                    "location": "westus",
                    "actionName": "Send_email"
                },
                "correlation": {
                    "actionTrackingId": "042fb72c-7bd4-439e-89eb-3cf4409d429e",
                    "clientTrackingId": "08587330012106702632"
                }
            }
        }
    ]
}
```

| Rakenteen nimeä | Kuvaus                                            |
|--------------|--------------------------------------------------------|
|tietueiden       | Kaikki tapahtumien poistaminen tämä paketti-matriisi.            |
|aika          | Aika, jolloin tapahtuman.                      |
|luokka      | Tapahtuman log-luokka.                           |
|resourceId    | Resurssitunnus tapahtuman luoneen resurssin. |
|operationName | Toiminnon nimi.                                 |
|taso         | Valinnainen. Osoittaa lokin tapahtuma.               |
|Ominaisuudet:    | Tapahtuman ominaisuudet.                               |


Voit tarkastella kaikkien resurssien palvelut, jotka tukevat toistamisen tapahtumaa-toiminnossa luettelo [tähän](monitoring-overview-of-diagnostic-logs.md).

##<a name="next-steps"></a>Seuraavat vaiheet
- [Lisätietoja on artikkelissa Azure vianmäärityslokit](monitoring-overview-of-diagnostic-logs.md)
- [Tapahtuman keskittimet käytön aloittaminen](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)
