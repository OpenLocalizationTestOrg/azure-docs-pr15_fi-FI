<properties
    pageTitle="Azure näytön PowerShell-pikaopas objektit. | Microsoft Azure"
    description="PowerShellin avulla voit käyttää Azure näyttö-ominaisuuksia, kuten Automaattinen skaalaus, ilmoitukset, webhooks ja haun lokeja."
    authors="kamathashwin"
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
    ms.date="10/26/2016"
    ms.author="ashwink"/>

# <a name="azure-monitor-powershell-quick-start-samples"></a>Näytteiden Azure näytön PowerShell-pika-aloitusopas

Tässä artikkelissa kerrotaan, esimerkki PowerShell-komennoilla voit käsitellä Azure näytön ominaisuudet. Azure näytön avulla Automaattinen skaalaus pilvipalveluihin näennäiskoneiden ja Web Apps-sovellusten ja voit lähettää ilmoitukset tai Soita web URL-osoitteet on määritetty telemetriatietoja arvojen perusteella.

>[AZURE.NOTE] Azure valvonta on uusi nimi "Azure-tiedot" kutsuttiin 2016 25 syyskuuhun asti. Kuitenkin nimitilan ja näin alla olevia komentoja edelleen sisältää "tiedot".

## <a name="set-up-powershell"></a>PowerShellin määrittäminen
Jos et ole jo, määrittää PowerShell toimimaan tietokoneeseen. Lisätietoja on artikkelissa [asentaminen ja määrittäminen PowerShell](../powershell-install-configure.md) .

## <a name="examples-in-this-article"></a>Tämän artikkelin esimerkkejä

Artikkelin esimerkeissä kuvataan, Azure näytön cmdlet-komentojen käyttämisestä. Voit myös tarkastella Azure näytön PowerShellin cmdlet-komennot, [Azure-näyttö (tiedot)-cmdlet](https://msdn.microsoft.com/library/azure/mt282452#40v=azure.200#41.aspx)-palvelussa koko luettelon.


## <a name="sign-in-and-use-subscriptions"></a>Kirjaudu sisään ja käytä tilaukset

Kirjaudu ensin Azure tilauksen.

```PowerShell
Login-AzureRmAccount
```

Tämä edellyttää kirjautumista sisään. Kun teet, kirjoita tilisi TenantId ja oletusarvon Tilaustunnus näytetään. Azure cmdlet toimi oletusarvo-tilauksen yhteydessä. Voit tarkastella tilausluettelosta on pääsy, käytä seuraavaa komentoa.

```PowerShell
Get-AzureRmSubscription
```

Jos haluat muuttaa eri tilauksen käytön yhteydessä, käytä seuraavaa komentoa.

```PowerShell
Set-AzureRmContext -SubscriptionId <subscriptionid>
```


## <a name="retrieve-audit-logs-for-a-subscription"></a>Valvontalokien tilauksen hakeminen
Käytä `Get-AzureRmLog` cmdlet-komento.  Seuraavassa on muutamia yleisiä esimerkkejä.

Pyydä lokimerkintöjä tämän aika tai päivämäärä esittää:

```PowerShell
Get-AzureRmLog -StartTime 2016-03-01T10:30
```

Hae lokimerkintöjä aika tai päivämäärä-alueen välille:

```PowerShell
Get-AzureRmLog -StartTime 2015-01-01T10:30 -EndTime 2015-01-01T11:30
```

Pyydä lokimerkintöjä tietyn resurssiryhmän:

```PowerShell
Get-AzureRmLog -ResourceGroup 'myrg1'
```

Pyydä lokimerkintöjä tietyn resurssin palveluntarjoaja aika tai päivämäärä-alueen välille:

```PowerShell
Get-AzureRmLog -ResourceProvider 'Microsoft.Web' -StartTime 2015-01-01T10:30 -EndTime 2015-01-01T11:30
```

Hae kaikki tapahtumat tietyn soittajan kanssa:

```PowerShell
Get-AzureRmLog -Caller 'myname@company.com'
```

Seuraava komento hakee viimeksi 1000 tapahtumat valvontaloki:

```PowerShell
Get-AzureRmLog -MaxEvents 1000
```

`Get-AzureRmLog`tukee useita muut parametrit. Katso `Get-AzureRmLog` lisätietoja viittaus.

>[AZURE.NOTE] `Get-AzureRmLog`sisältää vain 15 päivän historia. **-MaxEvents** parametrin avulla voit kyselyn viimeisen N tapahtumien lisäksi 15 päivää. Käytä Accessin tapahtumat 15 päivää vanhemmat, REST API tai SDK (C# näytteen SDK). Jos et käytä **aloitusajan**, oletusarvo on **Lopetusaika** miinus tunnin. Jos et käytä **Lopetusaika**, oletusarvo on nykyisen kellonajan. Kaikki ajat ovat UTC-aika.

## <a name="retrieve-alerts-history"></a>Noutaa ilmoitusten historia
Voit tarkastella kaikkien ilmoitusten tapahtumia, voit tehdä kyselyn Azure Resurssienhallinta lokit seuraavien esimerkkien avulla.

```PowerShell
Get-AzureRmLog -Caller "Microsoft.Insights/alertRules" -DetailedOutput -StartTime 2015-03-01
```

Voit tarkastella tietyn ilmoitusten säännön historia `Get-AzureRmAlertHistory` cmdlet-kulkeva hälytyksen Resurssitunnus.

```PowerShell
Get-AzureRmAlertHistory -ResourceId /subscriptions/s1/resourceGroups/rg1/providers/microsoft.insights/alertrules/myalert -StartTime 2016-03-1 -Status Activated
```

`Get-AzureRmAlertHistory` Cmdlet-komento tukee eri parametreja. Lisätietoja artikkelissa [Get-AlertHistory](https://msdn.microsoft.com/library/mt282453.aspx).


## <a name="retrieve-information-on-alert-rules"></a>Hakea tietoa ilmoitusten säännöt
Kaikki seuraavista komennoista ratkaisee nimeltä "montest" resurssiryhmä.

Näytä kaikki hälytyksen ominaisuudet:

```PowerShell
Get-AzureRmAlertRule -Name simpletestCPU -ResourceGroup montest -DetailedOutput
```

Hae kaikki ilmoitukset resurssiryhmä:

```PowerShell
Get-AzureRmAlertRule -ResourceGroup montest
```

Noutaa kohde resurssin kaikkien ilmoitusten sääntöjä. Kaikki ilmoitusten säännöt määritetään AM.

```PowerShell
Get-AzureRmAlertRule -ResourceGroup montest -TargetResourceId /subscriptions/s1/resourceGroups/montest/providers/Microsoft.Compute/virtualMachines/testconfig
```

`Get-AzureRmAlertRule`tukee muut parametrit. Lisätietoja on kohdassa [Hae AlertRule](https://msdn.microsoft.com/library/mt282459.aspx) .

## <a name="create-alert-rules"></a>Ilmoitusten sääntöjen luominen
Voit käyttää `Add-AlertRule` cmdlet-komento, voit luoda, päivittää tai ilmoitusten säännön käytöstä.

Sähköpostin ja webhook ominaisuuksien avulla voit luoda `New-AzureRmAlertRuleEmail` ja `New-AzureRmAlertRuleWebhook`, vastaavassa järjestyksessä. Määrittää hälytyksen cmdlet-komento, nämä toiminnot hälytyksen **Toiminnot** -ominaisuutta.

Seuraavassa osassa on otoksen, joka näytetään, miten voit luoda ilmoituksen säännön eri parametreilla.

### <a name="alert-rule-on-a-metric"></a>Ilmoitusten mittarin sääntö
Seuraavassa taulukossa kuvataan parametrit ja ilmoituksen käyttämällä mittarin luomiseen käytetyt arvot.


|parametri|arvo|
|---|---|
|Nimi|  simpletestdiskwrite|
|Tämä ilmoitus sääntö sijainti|   Yhdysvaltojen Itä|
|ResourceGroup| montest|
|TargetResourceId|  /subscriptions/S1/resourceGroups/montest/Providers/Microsoft.Compute/virtualMachines/testconfig|
|Ilmoitus, joka on luotu MetricName|   \PhysicalDisk (_Yhteensä) \Disk kirjoituksia/s. Katso `Get-MetricDefinitions` cmdlet-komennon alla käyttämisestä hakea tarkkaa metrisillä nimet|
|operaattori|  GreaterThan|
|Raja-arvo (Laske/sec-tämän metrijärjestelmän)|    1|
|WindowSize (ss)|  00:05:00|
|kokoaja (arvo, joka käyttää keskiarvon laskeminen tällöin tilasto)|  Keskiarvo|
|mukautetun sähköpostit (merkkijono matriisi)|'foo@example.com','bar@example.com'|
|Sähköpostin lähettäminen omistajat, osallistujat ja lukijat|    -SendToServiceOwners|

Luo sähköposti-toiminto

```PowerShell
$actionEmail = New-AzureRmAlertRuleEmail -CustomEmail myname@company.com
```

Luo Webhook-toiminto

```PowerShell
$actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri https://example.com?token=mytoken
```

Luo hälytyksen perinteinen AM Suoritin-%-arvo

```PowerShell
Add-AzureRmMetricAlertRule -Name vmcpu_gt_1 -Location "East US" -ResourceGroup myrg1 -TargetResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.ClassicCompute/virtualMachines/my_vm1 -MetricName "Percentage CPU" -Operator GreaterThan -Threshold 1 -WindowSize 00:05:00 -TimeAggregationOperator Average -Actions $actionEmail, $actionWebhook -Description "alert on CPU > 1%"
```

Noutaa hälytyksen

```PowerShell
Get-AzureRmAlertRule -Name vmcpu_gt_1 -ResourceGroup myrg1 -DetailedOutput
```

Lisää ilmoitusten cmdlet-komento päivittää säännön Jos ilmoitusten sääntö on jo annetun ominaisuudet. Ilmoitusten säännön käytöstä, Sisällytä parametri **- DisableRule**.

### <a name="alert-on-audit-log-event"></a>Valitse valvonta lokitapahtuman ilmoitus

>[AZURE.NOTE] Tämä toiminto on edelleen esikatselu.

Tässä skenaariossa Lähetä sähköposti, kun sivusto on käynnistetty resurssin ryhmän *abhingrgtest123*-tilauksen.

Sähköpostisäännön asetukset

```PowerShell
$actionEmail = New-AzureRmAlertRuleEmail -CustomEmail myname@company.com
```

Webhook säännön asetukset

```PowerShell
$actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri https://example.com?token=mytoken
```

Luo sääntö tapahtuman

```PowerShell
Add-AzureRmLogAlertRule -Name superalert1 -Location "East US" -ResourceGroup myrg1 -OperationName microsoft.web/sites/start/action -Status Succeeded -TargetResourceGroup abhingrgtest123 -Actions $actionEmail, $actionWebhook
```

Noutaa hälytyksen

```PowerShell
Get-AzureRmAlertRule -Name superalert1 -ResourceGroup myrg1 -DetailedOutput
```

`Add-AlertRule` Cmdlet-komennon avulla eri muut parametrit. Lisätietoja, katso [Lisää AlertRule](https://msdn.microsoft.com/library/mt282468.aspx).

## <a name="get-a-list-of-available-metrics-for-alerts"></a>Saat luettelon käytettävissä käytettävissä olevat arvot ilmoitusasetusten määrittäminen
Voit käyttää `Get-AzureRmMetricDefinition` cmdlet-komennon avulla voit tarkastella tietyn resurssin kaikki mittaukset luetteloa.

```PowerShell
Get-AzureRmMetricDefinition -ResourceId <resource_id>
```

Seuraava esimerkki luo taulukko, jossa metrijärjestelmä nimi ja sen yksikkö.

```PowerShell
Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit
```

Luettelon kaikista käytettävissä `Get-AzureRmMetricDefinition` on saatavilla kohdassa [Hae MetricDefinitions](https://msdn.microsoft.com/library/mt282458.aspx).


## <a name="create-and-manage-autoscale-settings"></a>Voit luoda ja hallita Automaattinen skaalaus-asetukset
Resurssi, kuten Web app-AM, pilvipalvelussa tai AM asteikko Määritä voi olla vain yksi määritettyä Automaattinen skaalaus-asetus.
Automaattinen skaalaus-asetuksista voi kuitenkin olla profiileja. Yksi suorituskyvyn perusteella asteikko profiili ja toinen sarjoille perusteella profiili. Kunkin profiilin voi olla useita sääntöjä, jotka on määritetty. Lisätietoja Automaattinen skaalaus-kohdassa [Automaattinen skaalaus sovelluksen poistamisesta](../cloud-services/cloud-services-how-to-scale.md).

Näin Käytämme:

1. Sääntöjen luominen
2. Luo säännöt, jotka olet luonut yhdistäminen aiemmin profiileista profiilia.
3. Valinnainen: Luo Automaattinen skaalaus-ilmoitusten määrittäminen webhook ja sähköpostin ominaisuudet.
4. Luo Automaattinen skaalaus-asetus kohde resurssin nimellä tulona yhdistämällä profiilit ja ilmoituksia, jonka loit edellisessä vaiheessa.

Seuraavissa esimerkeissä kerrotaan, miten voit luoda automaattinen Skaalausasetuksen määrittäminen Windows-käyttöjärjestelmän mukaan käyttämällä suorittimen käyttö metrijärjestelmä AM-akselille.

Luo sääntö, joka asteikko-kohtaa, jossa esiintymän määrä kasvaa.

```PowerShell
$rule1 = New-AzureRmAutoscaleRule -MetricName "\Processor(_Total)\% Processor Time" -MetricResourceId /subscriptions/s1/resourceGroups/big2/providers/Microsoft.Compute/virtualMachineScaleSets/big2 -Operator GreaterThan -MetricStatistic Average -Threshold 0.01 -TimeGrain 00:01:00 -TimeWindow 00:10:00 -ScaleActionCooldown 00:10:00 -ScaleActionDirection Increase -ScaleActionScaleType ChangeCount -ScaleActionValue 1
```     

Säännön luominen seuraavaksi asteikko sisään, esiintymä-Laske vähenemisen.

```PowerShell
$rule2 = New-AzureRmAutoscaleRule -MetricName "\Processor(_Total)\% Processor Time" -MetricResourceId /subscriptions/s1/resourceGroups/big2/providers/Microsoft.Compute/virtualMachineScaleSets/big2 -Operator GreaterThan -MetricStatistic Average -Threshold 2 -TimeGrain 00:01:00 -TimeWindow 00:10:00 -ScaleActionCooldown 00:10:00 -ScaleActionDirection Decrease -ScaleActionScaleType ChangeCount -ScaleActionValue 1
```

Luo säännöt profiilin.

```PowerShell
$profile1 = New-AzureRmAutoscaleProfile -DefaultCapacity 2 -MaximumCapacity 10 -MinimumCapacity 2 -Rules $rule1,$rule2 -Name "My_Profile"
```

Luo webhook-ominaisuus.

```PowerShell
$webhook_scale = New-AzureRmAutoscaleWebhook -ServiceUri "https://example.com?mytoken=mytokenvalue"
```

Luo Automaattinen skaalaus-asetus, kuten sähköpostin ja webhook aiemmin luoduissa ilmoitus-ominaisuus.

```PowerShell
$notification1= New-AzureRmAutoscaleNotification -CustomEmails ashwink@microsoft.com -SendEmailToSubscriptionAdministrators SendEmailToSubscriptionCoAdministrators -Webhooks $webhook_scale
```

Luo lopuksi Lisää edellä luomasi profiili Automaattinen skaalaus-asetus.

```PowerShell
Add-AzureRmAutoscaleSetting -Location "East US" -Name "MyScaleVMSSSetting" -ResourceGroup big2 -TargetResourceId /subscriptions/s1/resourceGroups/big2/providers/Microsoft.Compute/virtualMachineScaleSets/big2 -AutoscaleProfiles $profile1 -Notifications $notification1
```

Lisätietoja Automaattinen skaalaus-asetusten hallinta-kohdassa [Hae AutoscaleSetting](https://msdn.microsoft.com/library/mt282461.aspx).

## <a name="autoscale-history"></a>Automaattinen skaalaus historia
Seuraavassa esimerkissä esitetään voit tarkastella viimeisimmät Automaattinen skaalaus ja ilmoitusten tapahtumia. Valvonta log-haun avulla voit tarkastella Automaattinen skaalaus-historia.

```PowerShell
Get-AzureRmLog -Caller "Microsoft.Insights/autoscaleSettings" -DetailedOutput -StartTime 2015-03-01
```

Voit käyttää `Get-AzureRmAutoScaleHistory` cmdlet-komento noutaa Automaattinen skaalaus historia.

```PowerShell
Get-AzureRmAutoScaleHistory -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/microsoft.insights/autoscalesettings/myScaleSetting -StartTime 2016-03-15 -DetailedOutput
```

Lisätietoja on artikkelissa [Get-AutoscaleHistory](https://msdn.microsoft.com/library/mt282464.aspx).

### <a name="view-details-for-an-autoscale-setting"></a>Automaattinen skaalaus-asetusten tarkasteleminen
Voit käyttää `Get-Autoscalesetting` cmdlet-komento, voit hakea lisätietoja Automaattinen skaalaus-asetus.

Seuraavassa esimerkissä tietoja kaikki Automaattinen skaalaus-asetukset-resurssien ryhmä "myrg1".

```PowerShell
Get-AzureRmAutoscalesetting -ResourceGroup myrg1 -DetailedOutput
```

Seuraavassa esimerkissä esitetään tietoja resurssin ryhmän 'myrg1' kaikki Automaattinen skaalaus asetukset ja erityisesti nimeltä "MyScaleVMSSSetting" Automaattinen skaalaus-asetus.

```PowerShell
Get-AzureRmAutoscalesetting -ResourceGroup myrg1 -Name MyScaleVMSSSetting -DetailedOutput
```

### <a name="remove-an-autoscale-setting"></a>Poista Automaattinen skaalaus-asetus
Voit käyttää `Remove-Autoscalesetting` cmdlet-komento, poista Automaattinen skaalaus-asetus.

```PowerShell
Remove-AzureRmAutoscalesetting -ResourceGroup myrg1 -Name MyScaleVMSSSetting
```

## <a name="manage-log-profiles-for-audit-logs"></a>Valvontalokien log profiilien hallinta

Voit *log-profiilin* luominen ja tietojen vieminen tallennustilan tilin valvonta-lokit ja voit määrittää sen tietojen säilytys. Voit vaihtoehtoisesti myös käyttää tietoja tapahtuma-keskittimeen. Huomaa, että tämä toiminto ei esikatselussa tai voit luoda vain yhden lokin profiilin tilauskohtaisten. Voit tehdä seuraavat cmdlet-komennot nykyinen tilaus luominen ja hallinta log-profiileista. Voit myös valita tilauksessa. Vaikka PowerShell oletusarvo on voimassa oleva tilaus, voit muuttaa kyseisen käyttämällä `Set-AzureRmContext`. Voit määrittää valvontalokien kyseisen tilauksen piiriin kuuluvien tallennustilan tilin tai tapahtumaa-toiminnossa reitin tietoihin. Tietoja kirjoitetaan blob-tiedostoina JSON-muodossa.

### <a name="get-a-log-profile"></a>Hae log-profiili
Hakeaksesi aiemmin log-profiileista avulla `Get-AzureRmLogProfile` cmdlet-komento.

### <a name="add-a-log-profile-without-data-retention"></a>Lisää log profiili ilman tietojen säilytys

```PowerShell
Add-AzureRmLogProfile -Name my_log_profile_s1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -Locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia
```

### <a name="remove-a-log-profile"></a>Poista log-profiili

```PowerShell
Remove-AzureRmLogProfile -name my_log_profile_s1
```

### <a name="add-a-log-profile-with-data-retention"></a>Lisää log-profiili, jolla tietojen säilytys

Voit määrittää **- RetentionInDays** ominaisuuden päivien positiivinen kokonaislukuna, jossa tiedot säilytetään.

```PowerShell
Add-AzureRmLogProfile -Name my_log_profile_s1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -Locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia -RetentionInDays 90
```

### <a name="add-log-profile-with-retention-and-eventhub"></a>Log-profiili, jolla säilytys- ja EventHub lisääminen
Lisäksi reitittämisestä tallennustilan tilin tiedot, voit myös käyttää tapahtumaa-toiminnossa. Huomaa, että tämä preview-versio ja tallennustilaa tilin määrittäminen pakolliseksi, mutta tapahtumaa-toiminnossa määritys on valinnainen.

```PowerShell
Add-AzureRmLogProfile -Name my_log_profile_s1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey -Locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia -RetentionInDays 90
```

## <a name="configure-diagnostics-logs"></a>Määritä diagnostiikka-lokit
Monia Azure palveluita antaa muita lokit ja telemetriatietojen, mukaan lukien Azure verkon käyttöoikeusryhmät, ohjelmiston kuormituksen tasoitusmääritykset, avaimen säilö Azure Etsi palveluita ja logiikka sovellukset ja ne voidaan määrittää tallentaa Azure-tallennustilan tilin tiedot. Tätä toimintoa voidaan suorittaa vain resurssin hierarkiatasolla ja tallennustilan tilin on sisällettävä resurssiksi kohde saman alue, jossa diagnostiikka-asetus on määritetty.

### <a name="get-diagnostic-setting"></a>Hae diagnostiikan asetus

```PowerShell
Get-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Logic/workflows/andy0315logicapp
```

Diagnostiikan asetusten poistaminen käytöstä

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Logic/workflows/andy0315logicapp -StorageAccountId /subscriptions/s1/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/mystorageaccount -Enable $false
```

Diagnostiikan asetus ilman säilytys

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Logic/workflows/andy0315logicapp -StorageAccountId /subscriptions/s1/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/mystorageaccount -Enable $true
```

Diagnostiikan asetus, jossa säilytys

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Logic/workflows/andy0315logicapp -StorageAccountId /subscriptions/s1/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/mystorageaccount -Enable $true -RetentionEnabled $true -RetentionInDays 90
```

Diagnostiikan asetus tietyn log luokan säilytyksen kanssa

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Network/networkSecurityGroups/viruela1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/sakteststorage -Categories NetworkSecurityGroupEvent -Enable $true -RetentionEnabled $true -RetentionInDays 90
```
