<properties
    pageTitle="Yleistä Azure vianmäärityslokit | Microsoft Azure"
    description="Katso, mitä Azure vianmäärityslokit ovat ja miten voit käyttää niitä selvittääksesi, tapahtumia, jotka ilmenevät Azure resurssin."
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
    ms.date="10/12/2016"
    ms.author="johnkem; magoedte"/>

# <a name="overview-of-azure-diagnostic-logs"></a>Azure vianmäärityslokit yleiskatsaus
**Azure vianmäärityslokit** ovat resurssin lähettämän lokit, jotka tarjoavat monipuolisia, usein käytetyt tietoja resurssin toiminnan. Lokitiedostot sisältö määräytyy resurssilaji mukaan (esimerkiksi Windowsin tapahtumalokien järjestelmä on vianmäärityslokeihin VMs ja blob, taulukon yksi luokka ja jonon lokit ovat luokkiin vianmäärityslokit tallennustilan tilien) ja poiketa [tehtävän Log (aiemmalta nimeltään valvontalokin tai toiminnallisten loki)](monitoring-overview-activity-logs.md), joka sisältää huomioon toiminnot, jotka olivat suorittaa resurssit-tilaukseesi. Kaikki resurssit tukevat kuvatut vianmäärityslokit uuden tyypin. Tuetut palveluluetteloon alla näkyy, mitkä resurssityypit tukevat uusi vianmäärityslokit.

![Vianmäärityslokit looginen sijaintia](./media/monitoring-overview-of-diagnostic-logs/logical-placement-chart.png)

## <a name="what-you-can-do-with-diagnostic-logs"></a>Mitä voit tehdä vianmäärityslokit
Seuraavassa on muutamia esimerkkejä, voit hyödyntää Vianmäärityslokit:

- Tallentaa ne **Tallennustilan tilin** valvonta tai Manuaalinen tarkastus. Voit määrittää säilytys aika (päiviä) **Diagnostiikka-asetusten muuttaminen**.
- Kolmannen osapuolen palveluun tai mukautetun analytics-ratkaisun, kuten PowerBI erilaisille [virtauttaa ne **Tapahtuma** -porttiin](monitoring-stream-diagnostic-logs-to-event-hubs.md) .
- Analysointia [OMS Log Analytics](../log-analytics/log-analytics-azure-storage-json.md)

## <a name="diagnostic-settings"></a>Diagnostiikka-asetusten muuttaminen
Laske resurssien vianmäärityslokit määritetään diagnostiikka-asetusten muuttaminen. Resurssin ohjausobjektin **Diagnostiikka-asetusten muuttaminen** :

- Vianmäärityslokit vastaanotettavien (tallennustilan tilin, tapahtuman keskittimet ja/tai OMS Log Analytics).
- Lokitiedoston luokat lähetetään.
- Kuinka kauan log kussakin luokassa säilytettävä tallennustilan tili – nolla päivää säilyttäminen tarkoittaa lokit säilytetään jatkuvasti. Muussa tapauksessa tämä arvo voi olla 1 – 2147483647. Jos säilytyskäytännöt on määritetty, mutta lokien tallentaminen tallennustilan tilillä ei ole käytettävissä (esimerkiksi jos vain tapahtuman keskittimet tai OMS asetukset on valittuna.), säilytyskäytäntöjä ei ole vaikutusta.

Nämä asetukset on määritetty helposti kautta Azure-portaalissa resurssin diagnostiikka-sivu, PowerShellin Azure ja CLI komennot kautta tai [Azure näytön REST API](https://msdn.microsoft.com/library/azure/dn931943.aspx)kautta.

> [AZURE.WARNING] Vianmäärityslokit ja Laske resurssit (esimerkiksi VMs tai palvelun kangasta) mittaukset käyttää [erillisen järjestelmä määritys ja tulostaa valinnan](../azure-diagnostics.md).

## <a name="how-to-enable-collection-of-diagnostic-logs"></a>Miten voit ottaa käyttöön kokoelman vianmäärityslokit
Vianmäärityslokit sivustokokoelman voidaan ottaa käyttöön osana resurssin luominen tai kun resurssi on luotu resurssin sivu portaalissa kautta. Voit myös ottaa vianmäärityslokit milloin tahansa PowerShellin Azure tai CLI komennoilla tai käyttämällä Azure näytön REST-Ohjelmointirajapinnalla.

> [AZURE.TIP] Nämä ohjeet eivät koske suoraan resurssi. Saat näkyviin rakenteen ymmärtää määräten vaiheet, jotka koske tiettyjä resurssityypit tämän sivun alareunassa.

[Tässä artikkelissa näytetään, miten voit käyttää resurssimallin käyttöön diagnostiikan asetuksia, kun resurssin luominen](./monitoring-enable-diagnostic-logs-using-template.md)

### <a name="enable-diagnostic-logs-in-the-portal"></a>Ota käyttöön vianmäärityslokit-portaalissa
Voit ottaa vianmäärityslokit Azure-portaalissa, kun luot Laske resurssityypit ottamalla Windows- tai Linux Azure diagnostiikka-tunniste:

1.  **Uusi** ja valitse kiinnostavan resurssi.
2.  Perus-asetusten ja koon, valitsemalla **asetukset** -sivu **Seuranta**-jälkeen valitsemalla **käytössä** ja kohtaa, johon haluat tallentaa vianmäärityslokeihin tallennustilan-tili. Normaali tietojen korvaukset ja tallennustilaa peritään diagnostiikka lähettämistä tallennustilan tilin.

    ![Ota käyttöön vianmäärityslokit resurssin luomisen aikana](./media/monitoring-overview-of-diagnostic-logs/enable-portal-new.png)
3.  Valitse **OK** ja luo resurssi.

Laske resurssien voit ottaa vianmäärityslokit Azure-portaalissa kun resurssi on luotu toimimalla seuraavasti:

1.  Siirry resurssin sivu ja Avaa **Diagnostiikka** -sivu.
2.  **Valitse** ja valitse tallennustilan tilin ja/tai tapahtumaa-toiminnossa.

    ![Ota käyttöön vianmäärityslokit resurssin luomisen jälkeen](./media/monitoring-overview-of-diagnostic-logs/enable-portal-existing.png)
3.  Valitse **Log luokat** , jotka haluat kerätä tai lähettää **lokit**.
4.  Valitse **Tallenna**.

### <a name="enable-diagnostic-logs-via-powershell"></a>Ota käyttöön vianmäärityslokit PowerShellin kautta
Vianmäärityslokit kautta Azure PowerShellin cmdlet-komennot käyttöön käyttää seuraavia komentoja.

Vianmäärityslokit tallennustilan tilin tallennustilan käyttöön tällä komennolla:

    Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -StorageAccountId [your storage account id] -Enabled $true

Tallennustilan Tilitunnus on resurssitunnus, johon haluat lähettää lokit tallennustilan tilin.

Jotta streaming vianmäärityslokit tapahtuma-keskittimeen, käytä seuraavaa komentoa:

    Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -ServiceBusRuleId [your service bus rule id] -Enabled $true

Palvelun Bus Sääntötunnus on merkkijono, joka sisältää tässä muodossa: `{service bus resource ID}/authorizationrules/{key name}`.

Lähettäminen vianmäärityslokit Log Analytics työtilaan käyttöön tällä komennolla:

    Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -WorkspaceId [log analytics workspace id] -Enabled $true

> [AZURE.NOTE] WorkspaceId-parametri ei ole käytettävissä lokakuussa. Se tulee saataville marraskuun-versiossa.

Voit hankkia Log Analytics työtila-tunnuksen Azure-portaalissa.

Voit yhdistää näiden parametrien käyttöön useita tulostusasetukset.

### <a name="enable-diagnostic-logs-via-cli"></a>Ota käyttöön vianmäärityslokit CLI kautta
Jotta vianmäärityslokit Azure-CLI kautta, käytä seuraavia komentoja:

Vianmäärityslokit tallennustilan tilin tallennustilan käyttöön tällä komennolla:

    azure insights diagnostic set --resourceId <resourceId> --storageId <storageAccountId> --enabled true

Tallennustilan Tilitunnus on resurssitunnus, johon haluat lähettää lokit tallennustilan tilin.

Jotta streaming vianmäärityslokit tapahtuma-keskittimeen, käytä seuraavaa komentoa:

    azure insights diagnostic set --resourceId <resourceId> --serviceBusRuleId <serviceBusRuleId> --enabled true

Palvelun Bus Sääntötunnus on merkkijono, joka sisältää tässä muodossa: `{service bus resource ID}/authorizationrules/{key name}`.

Lähettäminen vianmäärityslokit Log Analytics työtilaan käyttöön tällä komennolla:

    azure insights diagnostic set --resourceId <resourceId> --workspaceId <workspaceId> --enabled true

> [AZURE.NOTE] WorkspaceId-parametri ei ole käytettävissä lokakuussa. Se tulee saataville marraskuun-versiossa.

Voit hankkia Log Analytics työtila-tunnuksen Azure-portaalissa.

Voit yhdistää näiden parametrien käyttöön useita tulostusasetukset.

### <a name="enable-diagnostic-logs-via-rest-api"></a>Ota käyttöön vianmäärityslokit REST API kautta
Diagnostiikan asetusten avulla Azure näytön REST API-kohdassa [tämän asiakirjan](https://msdn.microsoft.com/library/azure/dn931931.aspx).

## <a name="manage-diagnostic-settings-in-the-portal"></a>Diagnostiikan asetusten hallinta-portaalissa

Jotta voit varmistaa, että kaikki resurssit on määritetty oikein diagnostiikan asetusten kanssa, siirry portaalissa **Seuranta** -sivu ja Avaa **Vianmäärityslokit** -sivu.

![Diagnostiikan lokit-sivu-portaalissa](./media/monitoring-overview-of-diagnostic-logs/manage-portal-nav.png)

Saatat joutua valitsemalla "Lisää palveluja" Etsi seuranta-sivu.

Tämä sivu voit tarkastella ja suodattaa kaikki resurssit, jotka tukevat vianmäärityslokit, onko heillä on käytössä diagnostiikka ja mitkä tallennustilan tilin, tapahtuma-toiminnossa ja/tai lokin Analytics työtilan kyseiset lokit etenee oikealta.

![Diagnostiikan lokit-sivu tulokset-portaalissa](./media/monitoring-overview-of-diagnostic-logs/manage-portal-blade.png)

Resurssin valitsemalla Näytä kaikki lokit, jotka on tallennettu-tallennustilan tilin ja avulla voit poistaa käytöstä tai muokata diagnostiikan asetuksia. Lataa lokit tietyllä aikajaksolla Lataa-kuvaketta.

![Diagnostiikan lokit sivu yksi resurssi](./media/monitoring-overview-of-diagnostic-logs/manage-portal-logs.png)

> [AZURE.NOTE] Vianmäärityslokit vain tässä näkymässä näkyvät ja voi ladata, jos olet määrittänyt diagnostiikan asetuksia, tallenna ne tallennustilan tilin.

Kun napsautat linkkiä **Diagnostiikan** asetusten Avaa diagnostiikka-asetusten muuttaminen-sivu, jossa voit ottaminen käyttöön, poistaminen käytöstä tai muokata valitun resurssin diagnostiikan asetuksia.

## <a name="supported-services-and-schema-for-diagnostic-logs"></a>Tuetut palvelut ja vianmäärityslokit rakennetta
Vianmäärityslokit rakenteen vaihtelee sen mukaan, resurssi ja kirjaudu-luokka. Seuraavassa on tuettu palveluiden ja niiden rakenne.

| Palvelun                       | Rakenne ja asiakirjoja                                                                                                   |
|-------------------------------|-----------------------------------------------------------------------------------------------------------------|
|    Ohjelmiston kuormituksen     |    [Lokitiedoston analysointitietoja Azure kuormituksen (ennakkoversio)](../load-balancer/load-balancer-monitor-log.md)             |
|    Verkon käyttöoikeusryhmät    |    [Lokitiedoston analysointitietoja verkon käyttöoikeusryhmät (NSGs)](../virtual-network/virtual-network-nsg-manage-log.md)     |
|    Sovelluksen yhdyskäytävät       |    [Diagnostiikan kirjaus käyttöön sovelluksen yhdyskäytävän](../application-gateway/application-gateway-diagnostics.md)     |
|    Avaimen säilö                  |    [Azure avaimen säilö kirjaaminen](../key-vault/key-vault-logging.md)                                                 |
|    Azure haku               |    [Käyttöönotto ja Etsi liikenne Analytics hyödyntäminen](../search/search-traffic-analytics.md)                         |
|    Järvi tietosäilö            |    [Azure järvi tietovaraston vianmäärityslokit käyttäminen](../data-lake-store/data-lake-store-diagnostic-logs.md) |
|    Tietoja järvi Analytics        |    [Vianmäärityslokit käyttäminen Azure tietojen järvi analysoinnissa](../data-lake-analytics/data-lake-analytics-diagnostic-logs.md) |
|    Logiikan sovellukset                 |    Malli ei ole käytettävissä.                                                                                         |
|    Azure erä                |    [Azure erä Diagnostiikan kirjaus](../batch/batch-diagnostics.md)                                              |
|    Azure automaatio           |    [Lokitiedoston analysointitietoja Azure automaatio](../automation/automation-manage-send-joblogs-log-analytics.md)          |
|    Tapahtuma-toiminnossa                  |    Malli ei ole käytettävissä.                                                                                         |
|    Palvelun Bus                |    Malli ei ole käytettävissä.                                                                                         |
|    Virta Analytics           |    Malli ei ole käytettävissä.                                                                                         |

## <a name="supported-log-categories-per-resource-type"></a>Tuetut log luokat kohti resurssilaji

|Resurssilaji|Luokka|Luokan näyttönimi|
|---|---|---|
|Microsoft.Automation/automationAccounts|JobLogs|Työn lokit|
|Microsoft.Automation/automationAccounts|JobStreams|Työn virtaa|
|Microsoft.Batch/batchAccounts|ServiceLog|Lokit|
|Microsoft.DataLakeAnalytics/accounts|Valvonta|Valvontalokien|
|Microsoft.DataLakeAnalytics/accounts|Palvelupyynnöt|Pyydä lokit|
|Microsoft.DataLakeStore/accounts|Valvonta|Valvontalokien|
|Microsoft.DataLakeStore/accounts|Palvelupyynnöt|Pyydä lokit|
|Microsoft.EventHub/namespaces|ArchiveLogs|Arkisto-lokit|
|Microsoft.EventHub/namespaces|OperationalLogs|Toiminnallisia lokit|
|Microsoft.KeyVault/vaults|AuditEvent|Valvontalokien|
|Microsoft.Logic/workflows|WorkflowRuntime|Työnkulun suorituksenaikainen diagnostiikan tapahtumat|
|Microsoft.Network/networksecuritygroups|NetworkSecurityGroupEvent|Verkko-käyttöoikeusryhmän tapahtuma|
|Microsoft.Network/networksecuritygroups|NetworkSecurityGroupRuleCounter|Verkko-käyttöoikeusryhmän säännön laskuri|
|Microsoft.Network/networksecuritygroups|NetworkSecurityGroupFlowEvent|Verkko-käyttöoikeusryhmän säännön työnkulku tapahtuma|
|Microsoft.Network/loadBalancers|LoadBalancerAlertEvent|Lataa tasaustoiminto ilmoitusten tapahtumat|
|Microsoft.Network/loadBalancers|LoadBalancerProbeHealthStatus|Kuormituksen tasauspalvelun näytteenottimen terveyden tila|
|Microsoft.Network/applicationGateways|ApplicationGatewayAccessLog|Sovelluksen yhdyskäytävän Access lokista|
|Microsoft.Network/applicationGateways|ApplicationGatewayPerformanceLog|Sovelluksen yhdyskäytävän suorituskyky lokista|
|Microsoft.Network/applicationGateways|ApplicationGatewayFirewallLog|Sovelluksen yhdyskäytävän palomuuri loki|
|Microsoft.Search/searchServices|OperationLogs|Toimintojen lokit|
|Microsoft.ServerManagement/nodes|RequestLogs|Pyydä lokit|
|Microsoft.ServiceBus/namespaces|OperationalLogs|Toiminnallisia lokit|
|Microsoft.StreamAnalytics/streamingjobs|Suorittaminen|Suorittaminen|
|Microsoft.StreamAnalytics/streamingjobs|Yhtä aikaa muiden kanssa|Yhtä aikaa muiden kanssa|

## <a name="next-steps"></a>Seuraavat vaiheet
- [Virta vianmäärityslokit **tapahtuman** porttiin](monitoring-stream-diagnostic-logs-to-event-hubs.md)
- [Azure näytön REST-Ohjelmointirajapinnan käyttäminen diagnostiikan asetusten muuttaminen](https://msdn.microsoft.com/library/azure/dn931931.aspx)
- [Analysoi OMS Log Analytics lokien](../log-analytics/log-analytics-azure-storage-json.md)
