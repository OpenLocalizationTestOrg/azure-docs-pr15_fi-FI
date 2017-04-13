<properties
    pageTitle="Luo ilmoituksia Azure-palveluiden PowerShellin avulla | Microsoft Azure"
    description="PowerShellin avulla voit luoda Azure ilmoituksia, jotka voivat aiheuttaa ilmoituksia tai automaatio, kun määrittämäsi ehdot täyttyvät."
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

# <a name="use-powershell-to-create-alerts-for-azure-services"></a>Luo ilmoituksia Azure-palveluiden PowerShellin avulla

> [AZURE.SELECTOR]
- [Portal](insights-alerts-portal.md)
- [PowerShellin](insights-alerts-powershell.md)
- [CLI](insights-alerts-command-line-interface.md)

## <a name="overview"></a>Yleiskatsaus

Tämän artikkelin avulla voit määrittää PowerShellin Azure ilmoitukset.  

Voit vastaanottaa ilmoituksen seurantaa mittaukset tai Azure services-tapahtumat.

- **Metrijärjestelmän arvot** - ilmoitusten käynnistimien, kun tietyn mittarin arvo ylittää raja-arvon, voit määrittää jompaankumpaan suuntaan. Toisin sanoen se käynnistää sekä kun ehto täyttyy ja valitse sen jälkeen, kun ehto on enää täyty.    
- **Tehtävän tapahtumien poistaminen** - ilmoituksen esimerkkitilanne *jokaisen* tapahtuman tai, vain silloin, kun tietty määrä tapahtumien ilmetä.

Voit määrittää ilmoituksen, kun se seuraavasti:

- Lähetä sähköposti-ilmoitusten palvelun järjestelmänvalvoja ja apuyhteyshenkilöiden
- Sähköpostin lähettäminen uusia sähköpostiviestejä, jotka määrität.
- Kutsu webhook
- Aloita Azure runbookin, (vain-portaalista Azure) suorittaminen

Voit määrittää ja ilmoitusten sääntöjä käyttämällä koskevien tietojen hakeminen

- [Azure portal](insights-alerts-portal.md)
- [PowerShellin](insights-alerts-powershell.md)
- [käyttöliittymä (CLI)](insights-alerts-command-line-interface.md)
- [Azure näytön REST-Ohjelmointirajapinnalla](https://msdn.microsoft.com/library/azure/dn931945.aspx)


Saat lisätietoja, voit aina kirjoittaa ```get-help``` ja valitse haluamasi aihe PowerShell-komentoa.

## <a name="create-alert-rules-in-powershell"></a>PowerShellin ilmoitusten sääntöjen luominen

1. Azure kirjautuminen.   

    ```PowerShell
    Login-AzureRmAccount

    ```

2. Saat luettelon tilauksista on käytettävissä. Varmista, että käytät oikeaa tilaus. Jos näin ei ole, määrittää sen oikeaa tiliä käyttämällä tuloste `Get-AzureRmSubscription`.

    ```PowerShell
    Get-AzureRmSubscription
    Get-AzureRmContext
    Set-AzureRmContext -SubscriptionId <subscriptionid>
    ```

3.  Luettelon resurssiryhmä aiemmin luotuja sääntöjä, käytä seuraavaa komentoa:

    ```PowerShell
    Get-AzureRmAlertRule -ResourceGroup <myresourcegroup> -DetailedOutput
    ```

4. Jos haluat luoda säännön, on useita keskeistä tietojen ensin. 
    - **Resurssitunnus** resurssin haluat ilmoituksen määrittäminen
    - **Metrijärjestelmän määritelmät** käytettävissä kyseiselle resurssille

    Yksi tapa löytää Resurssitunnus on käyttää Azure-portaalia. Jos resurssi on jo luotu, valitse se portaalissa. Seuraava sivu, valitse *Ominaisuudet* *asetukset* -kohdassa. Resurssitunnus on seuraava sivu-kenttä. Toinen tapa on käyttää [Azure resurssin Explorer](https://resources.azure.com/).

    Esimerkki resurssitunnus web-sovelluksen on

    ```
    /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename
    ```

    Voit käyttää `Get-AzureRmMetricDefinition` voit tarkastella tietyn resurssin kaikkien metrisillä määritysten luettelo.

    ```PowerShell
    Get-AzureRmMetricDefinition -ResourceId <resource_id>
    ```

    Seuraavassa esimerkissä Luo taulukko, jossa metrijärjestelmä nimi ja yksikkö kyseinen arvo.

    ```PowerShell
    Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit

    ```
    Käytettävissä olevat vaihtoehdot Get-AzureRmMetricDefinition täydellinen luettelo on suorittamalla Get-MetricDefinitions.


5. Seuraava esimerkki määrittää ilmoituksen sivuston resurssille. Ilmoitusten käynnistimet aina, kun se saa liikenteen johdonmukaisesti 5 minuuttia ja uudelleen milloin se saa ei liikenne 5 minuuttia.

    ```PowerShell
    Add-AzureRmMetricAlertRule -Name myMetricRuleWithWebhookAndEmail -Location "East US" -ResourceGroup myresourcegroup -TargetResourceId /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename -MetricName "BytesReceived" -Operator GreaterThan -Threshold 2 -WindowSize 00:05:00 -TimeAggregationOperator Total -Description "alert on any website activity"

    ```

6. Voit luoda webhook tai lähettää sähköpostia, kun ilmoituksen, luo ensin sähköposti-ja/tai webhooks. Välittömästi Luo sääntö jälkeenpäin toiminnot-tunniste ja kuten seuraavassa esimerkissä. Ei voi liittää webhook tai sisältävien sähköpostien jo luonut sääntöjä PowerShellin kautta.


    ```PowerShell
    $actionEmail = New-AzureRmAlertRuleEmail -CustomEmail myname@company.com
    $actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri https://www.contoso.com?token=mytoken

    Add-AzureRmMetricAlertRule -Name myMetricRuleWithWebhookAndEmail -Location "East US" -ResourceGroup myresourcegroup -TargetResourceId /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename -MetricName "BytesReceived" -Operator GreaterThan -Threshold 2 -WindowSize 00:05:00 -TimeAggregationOperator Total -Actions $actionEmail, $actionWebhook -Description "alert on any website activity"
    ```


7. Voit luoda ilmoituksen, joka käynnistää tehtävän lokiin tietyn ehdon käyttämällä seuraavan lomakkeen komennot

    ```PowerShell
    $actionEmail = New-AzureRmAlertRuleEmail -CustomEmail myname@company.com
    $actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri https://www.contoso.com?token=mytoken

    Add-AzureRmLogAlertRule -Name myLogAlertRule -Location "East US" -ResourceGroup myresourcegroup -OperationName microsoft.web/sites/start/action -Status Succeeded -TargetResourceGroup resourcegroupbeingmonitored -Actions $actionEmail, $actionWebhook
    ```

    -OperationName vastaa tehtävän lokin tapahtuman tyyppi. Esimerkkejä tällaisista tiedostoista ovat *Microsoft.Compute/virtualMachines/delete* ja *microsoft.insights/diagnosticSettings/write*.

    Voit hankkia luettelo mahdollista operationNames PowerShell-komentoa [Get-AzureRmProviderOperation](https://msdn.microsoft.com/library/mt603720.aspx) . Vaihtoehtoisesti voit kyselyyn tehtävän lokiin ja etsiä aiempia toiminnoista, joita haluat luoda ilmoituksen Azure portaalin. Toiminnot, Näytä kuvan loki helpossa muodossa nimet näkyvät. Etsi JSON-merkinnän ja tuoda esiin OperationName-arvo.   

8. Varmista, että ilmoitukset on luotu oikein katsomalla yksittäisiä säännöt.

    ```PowerShell
    Get-AzureRmAlertRule -Name myMetricRuleWithWebhookAndEmail -ResourceGroup myresourcegroup -DetailedOutput

    Get-AzureRmAlertRule -Name myLogAlertRule -ResourceGroup myresourcegroup -DetailedOutput
    ```

9. Poista ilmoitukset. Nämä komennot Poista aiemmin luotu tämän artikkelin säännöt.

    ```PowerShell
    Remove-AzureRmAlertRule -ResourceGroup myresourcegroup -Name myrule
    Remove-AzureRmAlertRule -ResourceGroup myresourcegroup -Name myMetricRuleWithWebhookAndEmail
    Remove-AzureRmAlertRule -ResourceGroup myresourcegroup -Name myLogAlertRule
    ```

## <a name="next-steps"></a>Seuraavat vaiheet

* [Saat yleiskatsauksen Azure seuranta](monitoring-overview.md) , mukaan lukien tietotyypeistä voit kerätä ja valvonta.
* Lisätietoja [-ilmoituksia määritettäessä webhooks](insights-webhooks-alerts.md).
* Lisätietoja [Azure automaatio Runbooks](..\automation\automation-starting-a-runbook.md).
* Kerää yksityiskohtaisia hyvin usein arvot palvelun [Yleiskatsaus kerääminen vianmäärityslokit](monitoring-overview-of-diagnostic-logs.md) siirtyä.
* Varmista, että palvelu on käytettävissä ja vastaa [arvot sivustokokoelman yleiskatsaus](insights-how-to-customize-monitoring.md) hakeminen
