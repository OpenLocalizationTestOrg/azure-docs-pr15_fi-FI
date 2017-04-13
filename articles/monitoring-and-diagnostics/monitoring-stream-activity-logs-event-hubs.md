<properties
    pageTitle="Virtauttaa Azure tehtävän lokiin tapahtuman porttiin | Microsoft Azure"
    description="Opettele virtauttaa Azure tehtävän lokin tapahtuma-porttiin."
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
    ms.date="10/03/2016"
    ms.author="johnkem"/>

# <a name="stream-the-azure-activity-log-to-event-hubs"></a>Virta Azure tehtävän lokiin tapahtuman porttiin
[**Azure tehtävän loki**](./monitoring-overview-activity-logs.md) voidaan lähettää minkä tahansa sovelluksen valmiin "Vie"-vaihtoehtoa käytettäessä portaalissa tai ottamalla käyttöön palvelun Bus sääntötunnus Log profiilissa Azure PowerShellin cmdlet-komennot tai Azure CLI lähelle reaaliaikaisesti.

## <a name="what-you-can-do-with-the-activity-log-and-event-hubs"></a>Miten tehtävän Log ja tapahtuman keskittimet
Voit käyttää streaming ominaisuuksien toiminnan lokin todella tavoilla:

- **Kolmannen osapuolen kirjaaminen ja telemetriatietojen järjestelmiin stream** – jaettuina tapahtuman keskittimet streaming muuttuu, jolla pipe toimintojen kirjautumistiedot kolmannen osapuolen SIEMs kyselyjä ja kirjaudu analytics-ratkaisuja.
- **Luoda mukautettuja telemetriatietojen ja kirjaaminen ympäristö** – Jos sinulla on custom-built telemetriatietojen platform tai ovat vain pohtivat rakentaminen jokin, erittäin skaalattava Julkaise tilaa laatu tapahtuman keskittimet sallii ingest joustavasti tehtävän loki. [Käytä tapahtuman keskittimet maailmanlaajuisesti telemetriatietojen ympäristö tähän Dan Rosanova oppaassa.](https://azure.microsoft.com/documentation/videos/build-2015-designing-and-sizing-a-global-scale-telemetry-platform-on-azure-event-Hubs/)

## <a name="enable-streaming-of-the-activity-log"></a>Ota käyttöön toimintojen lokin streaming
Voit ottaa streaming tehtävän lokin ohjelmallisesti tai portaalin kautta. Kummassakin tapauksessa voit valita palvelun Bus-Namespace ja jaetun käyttöoikeuskäytäntö kyseisen nimitilan ja tapahtumaa-toiminnossa luodaan kyseisen nimitilan ensimmäisen uuden tehtävän Log tapahtuman yhteydessä. Jos sinulla ei ole palvelun Bus-Namespace, sinun on luotava. Jos olet aiemmin virtauttaa tehtävän Log tapahtumia tämän palvelun Bus Namespace, käyttää uudelleen, jotka on aiemmin luotu tapahtuma-toiminnossa. Jaetun access-käytännöllä määritetään käyttöoikeudet, jotka streaming järjestelmä on. Nykyään streaming tapahtuman porttiin edellyttää **hallinta**-, **luku**-ja **Lähetä** . Voit luoda tai muokata palvelun Bus Namespace jaettu käytäntöjen perinteinen portaalissa "Määrittäminen"-välilehdessä Service-Bus Namespace. Sisällytettävien streaming tehtävän Log log-profiilin päivittäminen, muutoksen käyttäjällä on oltava ListKey käyttöoikeus-palvelun Bus käyttöoikeuksien sääntöä.

### <a name="via-azure-portal"></a>Azure portaalin kautta 
1. Siirry-valikon käyttäminen portaalin vasemmalla puolella **Toimintaa Log** -sivu.

    ![Siirry tehtävän lokin-portaalissa](./media/monitoring-overview-activity-logs/activity-logs-portal-navigate.png)
2. Valitse **Vie** -painiketta sivu yläreunassa.

    ![Vie-painike-portaalissa](./media/monitoring-overview-activity-logs/activity-logs-portal-export.png)
3. Sivu, joka tulee näkyviin voit valita, jonka haluat stream tapahtumien ja palvelun Bus-Namespace, jossa haluat tapahtumaa-toiminnossa luodaan streaming näitä tapahtumia alueet.

    ![Vie tehtävän Log-sivu](./media/monitoring-overview-activity-logs/activity-logs-portal-export-blade.png)
4. Valitse Tallenna asetukset **Tallenna** . Asetuksia käytetään heti tilaukseen.


### <a name="via-powershell-cmdlets"></a>Kautta PowerShellin cmdlet-komennot
Log profiili on jo olemassa, jos sinun on poistettava profiilin.

1. Käytä `Get-AzureRmLogProfile` tunnistavan Jos log-profiili
2. Jos näin on, käytä `Remove-AzureRmLogProfile` poistaa sen.
3. Käytä `Set-AzureRmLogProfile` profiilin luominen:

```
Add-AzureRmLogProfile -Name my_log_profile -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey -Locations global,westus,eastus -RetentionInDays 90 -Categories Write,Delete,Action
```

Palvelun Bus Sääntötunnus on merkkijono, joka sisältää tässä muodossa: {palvelun bus Resurssitunnus} /authorizationrules/ {avaimen name}, esimerkiksi 

### <a name="via-azure-cli"></a>Azure CLI kautta
Log profiili on jo olemassa, jos sinun on poistettava profiilin.

1. Käytä `azure insights logprofile list` tunnistavan Jos log-profiili
2. Jos näin on, käytä `azure insights logprofile delete` poistaa sen.
3. Käytä `azure insights logprofile add` profiilin luominen:

```
azure insights logprofile add --name my_log_profile --storageId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/my_storage --serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey --locations global,westus,eastus,northeurope --retentionInDays 90 –categories Write,Delete,Action
```

Palvelun Bus Sääntötunnus on merkkijono, joka sisältää tässä muodossa: `{service bus resource ID}/authorizationrules/{key name}`.
 
## <a name="how-do-i-consume-the-log-data-from-event-hubs"></a>Miten tapahtuman keskittimet lokin tiedot tarjoaman?
[Tehtävän lokin rakenne on saatavilla täällä](./monitoring-overview-activity-logs.md). Kuhunkin tapahtumaan on matriisin JSON BLOB nimeltä "tietueiden."

## <a name="next-steps"></a>Seuraavat vaiheet
- [Tehtävän lokin tallennustilan tiliin](./monitoring-archive-activity-log.md)
- [Lue Azure tehtävän lokin yleiskatsaus](./monitoring-overview-activity-logs.md)
- [Tehtävän lokitapahtuman perusteella ilmoituksen määrittäminen](./insights-auditlog-to-webhook-email.md)
