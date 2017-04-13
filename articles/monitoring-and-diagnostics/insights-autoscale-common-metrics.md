<properties
    pageTitle="Azure näytön automaattisen skaalauksen poistaminen yleisiä arvot. | Microsoft Azure"
    description="Katso, mitkä arvot käytetään yleensä automaattisen skaalauksen poistaminen pilvipalveluihin, näennäiskoneiden ja verkkosovelluksissa."
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
    ms.date="08/02/2016"
    ms.author="ashwink"/>

# <a name="azure-monitor-autoscaling-common-metrics"></a>Azure näytön automaattisen skaalauksen poistaminen yleiset arvot

Azure näytön automaattisen skaalauksen poistaminen avulla voit skaalata käynnissä kerrat, ylös tai alas telemetriatietojen tietojen (arvot) perusteella. Tässä asiakirjassa kerrotaan yleisiä tietoja, joista haluat ehkä käyttää. Valitse pilvipalveluihin ja palvelimen klustereihin Azure-portaalissa skaalata resurssin arvo. Voit myös valita minkä tahansa metrijärjestelmä skaalata eri resurssista.

Seuraavassa on tietoja siitä, miten voit etsiä ja lisätä luetteloon haluat skaalata arvot. Seuraava koskee skaalaus virtuaalikoneen asteikko joukot sekä.

## <a name="compute-metrics"></a>Laske arvot
Oletusarvon mukaan Azure AM v2 on määritetty diagnostiikka-tunniste ja niissä on otettu käyttöön seuraavat arvot.

- [Windowsin AM v2 Vieras mittaukset](#compute-metrics-for-windows-vm-v2-as-a-guest-os)
- [Vieras mittaukset Linux AM v2](#compute-metrics-for-linux-vm-v2-as-a-guest-os)

Voit käyttää `Get MetricDefinitions` API/PoSH/CLI voit tarkastella käytettävissä VMSS resurssin arvot. 

Jos käytät AM asteikko joukkoja ei näy luettelossa tietyn mittarin, on todennäköisesti *käytöstä* diagnostiikka-tunniste.

Jos tietty arvo ei ole parhaillaan näyte tai siirry, kuinka usein, voit päivittää diagnostiikka-määritys.

Jos kummassakin tapauksessa edellä on TOSI, tarkista [PowerShellin Azure diagnostiikka-käyttöjärjestelmässä virtual koneen ottaminen käyttöön](../virtual-machines/virtual-machines-windows-ps-extensions-diagnostics.md) ja Päivitä Azure AM diagnostiikka-tunniste PowerShell tietoja käyttöön lisätiedot. Tässä artikkelissa on myös diagnostiikka määritysten mallitiedosto.

### <a name="compute-metrics-for-windows-vm-v2-as-a-guest-os"></a>Laske Windows AM v2 mittaukset vieraana OS

Kun luot uuden AM (v2) Azure-Diagnostiikka on käytössä käyttämällä diagnostiikka-tunniste.

Voit luoda käyttämällä seuraava komento PowerShell arvot luettelo.


```
Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit
```

Voit luoda ilmoituksen seuraavat arvot.

|Metrijärjestelmän nimi|   Yksikkö|
|---|---|
|\Processor(_Total)\% suoritinaika    |Prosenttia|
|\Processor(_Total)\% etuoikeutettu aika   |Prosenttia|
|\Processor(_Total)\% Käyttäjäaika |Prosenttia|
|\Processor tiedot (_Yhteensä) \Processor taajuus |Laske|
|\System\Processes| Laske|
|\Process (_Yhteensä) \Thread määrä| Laske|
|\Process (_Yhteensä) \Handle määrä  |Laske|
|\Memory\% vahvistetun tavua käytössä   |Prosenttia|
|\Memory\Available tavua|   Tavua|
|\Memory\Committed tavua    |Tavua|
|\Memory\Commit rajoitus|  Tavua|
|\Memory\Pool sivutettu tavua|  Tavua|
|\Memory\Pool tavuja|   Tavua|
|\PhysicalDisk(_Total)\% levyn aika| Prosenttia|
|\PhysicalDisk(_Total)\% levyn lukea aika|    Prosenttia|
|\PhysicalDisk(_Total)\% levyn kirjoittaminen aika|   Prosenttia|
|\PhysicalDisk (_Yhteensä) \Disk siirrot/s   |CountPerSecond|
|\PhysicalDisk (_Yhteensä) \Disk lukuja/s   |CountPerSecond|
|\PhysicalDisk (_Yhteensä) \Disk kirjoituksia/s  |CountPerSecond|
|\PhysicalDisk (_Yhteensä) \Disk tavua/s   |BytesPerSecond|
|\PhysicalDisk (_Yhteensä) \Disk tavua/s| BytesPerSecond|
|\PhysicalDisk (_Yhteensä) \Disk kirjoittaminen tavua/s |BytesPerSecond|
|\PhysicalDisk (_Yhteensä) \Avg. Keskiarvo|  Laske|
|\PhysicalDisk (_Yhteensä) \Avg. Lue keskiarvo| Laske|
|\PhysicalDisk (_Yhteensä) \Avg. Kirjoita keskiarvo |Laske|
|\LogicalDisk(_Total)\% vapaata tilaa| Prosenttia|
|\LogicalDisk (_Yhteensä) \Free megatavua|   Laske|



### <a name="compute-metrics-for-linux-vm-v2-as-a-guest-os"></a>Laske Linux AM v2 mittaukset vieraana OS

Kun luot uuden AM (v2) Azure-Diagnostiikka on käytössä oletusarvoisesti käyttämällä diagnostiikka-tunniste.

Voit luoda käyttämällä seuraava komento PowerShell arvot luettelo.

```
Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit
```

 Voit luoda ilmoituksen seuraavat arvot.

|Metrijärjestelmän nimi                            |Yksikkö|
|---|---|
|\Memory\AvailableMemory                |Tavua|
|\Memory\PercentAvailableMemory         |Prosenttia|
|\Memory\UsedMemory                     |Tavua|
|\Memory\PercentUsedMemory              |Prosenttia|
|\Memory\PercentUsedByCache             |Prosenttia|
|\Memory\PagesPerSec                    |CountPerSecond|
|\Memory\PagesReadPerSec                |CountPerSecond|
|\Memory\PagesWrittenPerSec             |CountPerSecond|
|\Memory\AvailableSwap                  |Tavua|
|\Memory\PercentAvailableSwap           |Prosenttia|
|\Memory\UsedSwap                       |Tavua|
|\Memory\PercentUsedSwap                |Prosenttia|
|\Processor\PercentIdleTime             |Prosenttia|
|\Processor\PercentUserTime             |Prosenttia|
|\Processor\PercentNiceTime             |Prosenttia|
|\Processor\PercentPrivilegedTime       |Prosenttia|
|\Processor\PercentInterruptTime        |Prosenttia|
|\Processor\PercentDPCTime              |Prosenttia|
|\Processor\PercentProcessorTime        |Prosenttia|
|\Processor\PercentIOWaitTime           |Prosenttia|
|\PhysicalDisk\BytesPerSecond           |BytesPerSecond|
|\PhysicalDisk\ReadBytesPerSecond       |BytesPerSecond|
|\PhysicalDisk\WriteBytesPerSecond      |BytesPerSecond|
|\PhysicalDisk\TransfersPerSecond       |CountPerSecond|
|\PhysicalDisk\ReadsPerSecond           |CountPerSecond|
|\PhysicalDisk\WritesPerSecond          |CountPerSecond|
|\PhysicalDisk\AverageReadTime          |Sekunnin ajan|
|\PhysicalDisk\AverageWriteTime         |Sekunnin ajan|
|\PhysicalDisk\AverageTransferTime      |Sekunnin ajan|
|\PhysicalDisk\AverageDiskQueueLength   |Laske|
|\NetworkInterface\BytesTransmitted     |Tavua|
|\NetworkInterface\BytesReceived        |Tavua|
|\NetworkInterface\PacketsTransmitted   |Laske|
|\NetworkInterface\PacketsReceived      |Laske|
|\NetworkInterface\BytesTotal           |Tavua|
|\NetworkInterface\TotalRxErrors        |Laske|
|\NetworkInterface\TotalTxErrors        |Laske|
|\NetworkInterface\TotalCollisions      |Laske|




## <a name="commonly-used-web-server-farm-metrics"></a>Usein käytetyn Web (palvelinklusterin)-arvot

Voit suorittaa Automaattinen skaalaus yleisiä web server arvot kuten Http jonon pituuden perusteella. Sen metrisillä nimi on **HttpQueueLength**.  Seuraavassa osassa on luettelo käytettävissä server-klusterin (online) arvot.

### <a name="web-apps-metrics"></a>Web Apps-arvot

Voit luoda luettelon Web Apps-arvot käyttämällä seuraava komento PowerShell.

```
Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit
```

Voit ilmoittaa tai skaalata mukaan nämä arvot.

|Metrijärjestelmän nimi        |Yksikkö|
|---                |---|
|CpuPercentage      |Prosenttia|
|MemoryPercentage   |Prosenttia|
|DiskQueueLength    |Laske|
|HttpQueueLength    |Laske|
|BytesReceived      |Tavua|
|BytesSent          |Tavua|


## <a name="commonly-used-storage-metrics"></a>Usein käytetyn tallennustilan arvot
Voit skaalata tallennusvälineiden jonon pituuden, joka tallennusvälineiden jonossa olevien viestien määrä on mukaan. Tallennustilan jono on erityinen metrijärjestelmä ja käytetty kynnysarvo päivitetään kohti esiintymän viestien määrä. Tämä tarkoittaa sitä, jos on kaksi esiintymää ja kynnysarvo asetetaan 100, jos se skaalautuvat kun jonossa olevien viestien määrä on 200. Esimerkiksi 100 viestien määrä esiintymä.

Voit määrittää tämä on **asetukset** -sivu Azure-portaalissa. AM asteikko joukkojen voit päivittää ARM-mallin Automaattinen skaalaus-asetus käyttää *metricName* *ApproximateMessageCount* ja välittää tallennustilan jonossa tunnus *metricResourceUri*nimellä.

Esimerkiksi perinteinen tallennustilan tilillä metricTrigger Automaattinen skaalaus-asetus näkyy tällöin:

```
"metricName": "ApproximateMessageCount",
 "metricNamespace": "",
 "metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.ClassicStorage/storageAccounts/mystorage/services/queue/queues/mystoragequeue"
 ```

(-Classic)-tallennustilan tilin näkyy tällöin metricTrigger:

```
"metricName": "ApproximateMessageCount",
"metricNamespace": "",
"metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.Storage/storageAccounts/mystorage/services/queue/queues/mystoragequeue"
```

## <a name="commonly-used-service-bus-metrics"></a>Usein käytetyn palvelun Bus arvot

Voit skaalattuja palvelun Bus jono, joka on palvelun Bus jonossa olevien viestien määrä. Palvelun Bus jono on erityinen metrijärjestelmä ja kynnysarvo käytetty päivitetään kohti esiintymän viestien määrä. Tämä tarkoittaa sitä, jos on kaksi esiintymää ja kynnysarvo asetetaan 100, jos se skaalautuvat kun jonossa olevien viestien määrä on 200. Esimerkiksi 100 viestien määrä esiintymä.

AM asteikko joukkojen voit päivittää ARM-mallin Automaattinen skaalaus-asetus käyttää *metricName* *ApproximateMessageCount* ja välittää tallennusvälineiden jonossa tunnus *metricResourceUri*nimellä.

```
"metricName": "MessageCount",
 "metricNamespace": "",
"metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.ServiceBus/namespaces/mySB/queues/myqueue"
```

>[AZURE.NOTE] Resurssien ryhmä-käsite ei ole palvelun Bus, mutta Azure Resurssienhallinta Luo resurssin oletusryhmän alueittain. Resurssiryhmä on yleensä "oletusasetusten - ServiceBus-[alue]". Esimerkiksi 'Oletus-ServiceBus-EastUS', 'Oletusarvo-ServiceBus-WestUS', 'Oletusarvo-ServiceBus-AustraliaEast' jne.
