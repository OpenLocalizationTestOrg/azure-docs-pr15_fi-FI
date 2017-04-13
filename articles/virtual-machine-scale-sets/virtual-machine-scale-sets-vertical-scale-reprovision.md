<properties
    pageTitle="Skaalaa pystysuunnassa Azure virtuaalikoneen asteikko joukot | Microsoft Azure"
    description="Miten pystysuunnassa Virtual Machine vastauksena valvominen ilmoitusten Azure automaatio kanssa"
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="gbowerman"
    manager="madhana"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/03/2016"
    ms.author="guybo"/>

# <a name="vertical-autoscale-with-virtual-machine-scale-sets"></a>Pystysuuntainen Automaattinen skaalaus ja virtuaalikoneen asteikko-vaihtoehdon avulla

Tässä artikkelissa käsitellään skaalata pystysuunnassa Azure [Virtuaalikoneen asteikko joukot](https://azure.microsoft.com/services/virtual-machine-scale-sets/) kanssa tai ilman reprovisioning. Pystysuuntainen skaalauksen VMs, jotka eivät ole asteikko joukoissa, viitata [skaalata pystysuunnassa Azure virtual machine Azure automaatio kanssa](../virtual-machines/virtual-machines-windows-vertical-scaling-automation.md).

Skaalauksen pystysuunnassa, eli _skaalata_ ja _skaalata_, tarkoittaa suurenee tai pienenee virtuaalikoneen (AM) koot vastauksena työmäärä. Vertaa tätä [skaalauksen vaakasuunnassa](./virtual-machine-scale-sets-autoscale-overview.md), jota kutsutaan myös _asteikko ulos_ ja _-asteikkoa_, jossa VMs määrä on muutettu työmäärää mukaan.

Reprovisioning tarkoittaa poistaminen aiemmin AM ja tilalle uusi. Suurenna tai Pienennä VMs AM-asteikko-joukko, joissakin tilanteissa haluat aiemmin VMs kokoa ja säilyttää tiedot, kun muissa tapauksissa sinun täytyy ottaa käyttöön uusi koko uusi VMs. Tässä asiakirjassa kerrotaan kummassakin tapauksessa.

Skaalauksen pystysuunnassa voi olla hyötyä kun:

- Palvelu rakennettu näennäiskoneiden on kohdassa käyttää (kuten viikonloppuisin). AM koon pienentäminen pienentää kuukausittain kustannukset.
- Lisääntyvien AM koon, jossa suuremmaksi tarvittaessa voitava ilman muita VMs luomista.

Voit määrittää skaalauksen pystysuunnassa olevan käynnistettävät mukaan metrisillä perustuvat hälytykset AM-asteikko-määrittäminen. Kun ilmoituksen aktivoidaan se käynnistyy webhook kyseisen käynnistimien runbookin, joka voi skaalata oman asteikko määrittäminen ylös tai alas. Skaalauksen pystysuunnassa voidaan määrittää seuraavasti:

1. Luo Azure automaatio-tili ja Suorita nimellä-ominaisuutta.
2. Tuo Azure automaatio pystysuora asteikko runbooks AM asteikko joukkojen tilauksen.
3. Lisää oman runbookin webhook.
4. Lisää ilmoituksen oman AM asteikko määrittää käyttämällä webhook ilmoituksen.

> [AZURE.NOTE] Pystysuuntainen automaattisen skaalauksen poistaminen voi kestää vain tiettyjä alueita AM kokoisille kohtaan. Vertaa jokaisen koon määritykset ennen kuin päätät skaalata muodosta toiseen (suurempi arvo ei aina tarkoita isommaksi AM kokoa). Voit valita skaalata seuraavat parit kokoisille välillä:

>| AM kokoa skaalaus pari |   |
|---|---|
|  Standard_A0 | Standard_A11 |
|  Standard_D1 |  Standard_D14 |
|  Standard_DS1 |  Standard_DS14 |
|  Standard_D1v2 |  Standard_D15v2 |
|  Standard_G1 |  Standard_G5 |
|  Standard_GS1 |  Standard_GS5 |

## <a name="create-an-azure-automation-account-with-run-as-capability"></a>Luo tili Azure automaatio ja Suorita nimellä-ominaisuus

Huomaa, sinun on suoritettava ei luoda Azure automaatio-tiliä, joka isännöi runbooks käytettävä skaalata AM asteikko määrittäminen esiintymät. Viimeksi [Azure automaatio](https://azure.microsoft.com/services/automation/) käyttöön "Suorita tiliksi"-toiminto, jolla on suunniteltu asetus palvelun lyhennys määrittäminen automaattinen suorittaminen runbooks käyttäjän puolesta hyvin helppoa. Lue lisätietoja tämän artikkelin ohjeiden mukaisesti:

* [Todennetaan Runbooks Azure Suorita nimellä-tilillä](../automation/automation-sec-configure-azure-runas-account.md)

## <a name="import-azure-automation-vertical-scale-runbooks-into-your-subscription"></a>Tilauksen Azure automaatio pystysuora asteikko runbooks tuominen

Tarvitaan skaalata pystysuunnassa AM asteikko-joukot runbooks on jo julkaistu Azure automaatio Runbookin valikoiman. Voit tuoda ne tilauksen noudattamalla tämän artikkelin ohjeita:

* [Azure automatisointiin Runbookin ja moduulin valikoimat](../automation/automation-runbook-gallery.md)

Valitse Selaa valokuvavalikoima-asetus Runbooks-valikosta:

![Runbooks tuomista][runbooks]

Runbooks vaikuttavat voi tuoda ovat näkyvissä. Valitse Haluatko, että pystysuora skaalaus kanssa tai ilman reprovisioning perusteella runbookin:

![Runbooks valikoima][gallery]

## <a name="add-a-webhook-to-your-runbook"></a>Oman runbookin webhook lisääminen

Kun olet tuonut runbooks, sinun on Lisää webhook: n runbookin, jotta se on saatu esiin hälytyksen AM-asteikko-määrittäminen. Tietoja oman Runbookin webhook luominen on kuvattu artikkelissa:

* [Azure automaatio webhooks](../automation/automation-webhooks.md)

> [AZURE.NOTE] Varmista, että voit kopioida webhook URI ennen kuin suljet webhook-valintaikkuna, kun tarvitset tämä seuraavan osion.

## <a name="add-an-alert-to-your-vm-scale-set"></a>Lisää ilmoituksen AM-asteikko-määrittäminen

Alla on PowerShell-komentosarjaa, jossa kerrotaan, miten voit lisätä ilmoituksen AM asteikko joukkoon. Kyselysäännön ilmoituksen metrijärjestelmä nimen seuraavassa artikkelissa: [Azure näytön automaattisen skaalauksen poistaminen yleiset arvot](../monitoring-and-diagnostics/insights-autoscale-common-metrics.md).

```
$actionEmail = New-AzureRmAlertRuleEmail -CustomEmail user@contoso.com
$actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri <uri-of-the-webhook>
$threshold = <value-of-the-threshold>
$rg = <resource-group-name>
$id = <resource-id-to-add-the-alert-to>
$location = <location-of-the-resource>
$alertName = <name-of-the-resource>
$metricName = <metric-to-fire-the-alert-on>
$timeWindow = <time-window-in-hh:mm:ss-format>
$condition = <condition-for-the-threshold> # Other valid values are LessThanOrEqual, GreaterThan, GreaterThanOrEqual
$description = <description-for-the-alert>

Add-AzureRmMetricAlertRule  -Name  $alertName `
                            -Location  $location `
                            -ResourceGroup $rg `
                            -TargetResourceId $id `
                            -MetricName $metricName `
                            -Operator  $condition `
                            -Threshold $threshold `
                            -WindowSize  $timeWindow `
                            -TimeAggregationOperator Average `
                            -Actions $actionEmail, $actionWebhook `
                            -Description $description
```

> [AZURE.NOTE] On suositeltavaa määrittäminen suositeltavaa kerralla aikaikkunan ilmoituksen siten vältät käynnistävä skaalauksen pystysuunnassa ja liitetyn palvelun keskeytymisen, liian usein. Harkitse ikkunan vähintään vähintään 20 – 30 minuuttia. Harkitse, vaakasuora skaalausta, jos haluat välttää keskeytymisen.

Lue lisätietoja siitä, miten voit luoda ilmoituksia on seuraavissa artikkeleissa:

* [Näytteiden Azure näytön PowerShell-pika-aloitusopas](../monitoring-and-diagnostics/insights-powershell-samples.md)
* [Azure näytön Office kaikissa ympäristöissä CLI pika-aloitusopas-objektit](../monitoring-and-diagnostics/insights-cli-samples.md)

## <a name="summary"></a>Yhteenveto

Tässä artikkelissa on osoittanut yksinkertainen pystysuora skaalauksen esimerkkejä. Voit yhdistää nämä rakenneosat - automaatio-tili, runbooks, webhooks, ilmoitukset - rich eri tilanteista niin, että käytössä on mukautettuja toimintoja.

[runbooks]: ./media/virtual-machine-scale-sets-vertical-scale-reprovision/runbooks.png
[gallery]: ./media/virtual-machine-scale-sets-vertical-scale-reprovision/runbooks-gallery.png
