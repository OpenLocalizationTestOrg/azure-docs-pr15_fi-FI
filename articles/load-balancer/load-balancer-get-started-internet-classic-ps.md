<properties
   pageTitle="Selainpohjainen vastakkaisten kuormituksen perinteinen tilassa PowerShellin luomisesta | Microsoft Azure"
   description="Opettele luomaan selainpohjainen vastakkaisten kuormituksen perinteinen tilassa PowerShellin avulla"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""
   tags="azure-service-management"
/>
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="04/05/2016"
   ms.author="sewhee" />

# <a name="get-started-creating-an-internet-facing-load-balancer-classic-in-powershell"></a>Luomisesta selainpohjainen vastakkaisten PowerShell kuormituksen (perinteinen)

[AZURE.INCLUDE [load-balancer-get-started-internet-classic-selectors-include.md](../../includes/load-balancer-get-started-internet-classic-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Tässä artikkelissa käsitellään perinteinen käyttöönottomalli. Voit myös [luominen selainpohjainen vastakkaisten kuormituksen Azure resurssien hallinnan avulla](load-balancer-get-started-internet-arm-ps.md).

[AZURE.INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]



## <a name="set-up-load-balancer-using-powershell"></a>Määrittää kuormituksen PowerShellin avulla

Jos haluat määrittää kuormituksen, PowerShellin avulla, noudata seuraavia ohjeita:

1. Jos et ole aikaisemmin käyttänyt PowerShellin Azure-artikkelissa [asentaminen ja määrittäminen PowerShellin Azure](../../articles/powershell-install-configure.md) ja noudata ohjeita päässä Azure Kirjaudu ja valitse tilauksen.


2. Kun olet luonut virtual machine, kuormituksen tasauspalvelun lisääminen virtual koneen sisällä saman pilvipalvelussa PowerShellin cmdlet-komennot avulla.

Seuraavassa esimerkissä lisätään kuormituksen tasauspalvelun määrittäminen nimeltä "webfarm" cloud palvelu "mytestcloud" (tai myctestcloud.cloudapp.net), kuormituksen päätepisteet lisääminen näennäiskoneiden nimeltä "web1" ja "web2". Kuormituksen vastaanottaa verkkoliikennettä porttiin 80 ja välillä (Valitse tämä palvelupyynnön portti 80) päätepiste määrittämiä näennäiskoneiden saldojen lataaminen TCP avulla.


### <a name="step-1"></a>Vaihe 1
Luo kuormitus tasataan päätepisteen ensimmäisen AM "web1"

    Get-AzureVM -ServiceName "mytestcloud" -Name "web1" | Add-AzureEndpoint -Name "HttpIn" -Protocol "tcp" -PublicPort 80 -LocalPort 80 -LBSetName "WebFarm" -ProbePort 80 -ProbeProtocol "http" -ProbePath '/' | Update-AzureVM

### <a name="step-2"></a>Vaihe 2

Luo toinen päätepisteen kuormituksen tasauspalvelun määrittäminen nimeä toisen AM "web2"

    Get-AzureVM -ServiceName "mytestcloud" -Name "web2" | Add-AzureEndpoint -Name "HttpIn" -Protocol "tcp" -PublicPort 80 -LocalPort 80 -LBSetName "WebFarm" -ProbePort 80 -ProbeProtocol "http" -ProbePath '/' | Update-AzureVM

## <a name="remove-a-virtual-machine-from-a-load-balancer"></a>Kuormituksen tasauspalvelun virtual machine poistaminen

Voit käyttää Poista AzureEndpoint kuormituksen virtuaalikoneen päätepisteen poistaminen

    Get-azureVM -ServiceName mytestcloud  -Name web1 |Remove-AzureEndpoint -Name httpin| Update-AzureVM

## <a name="next-steps"></a>Seuraavat vaiheet

Voit myös [sisäinen kuormituksen luomisesta](load-balancer-get-started-ilb-classic-ps.md) ja millaisia [jakauman tilassa](load-balancer-distribution-mode.md) especific kuormituksen tasauspalvelun verkon liikenne-asetusten määrittäminen.

Jos sovellus täytyy pidä yhteydet toiminnassa palvelinten kuormituksen tasauspalvelun takana, ymmärrät Lisää [Käyttämättömät TCP](load-balancer-tcp-idle-timeout.md)-aikakatkaisu asetuksista kuormituksen. Sen avulla tietoja käyttämättömänä yhteyden toiminnan, kun käytät Azure kuormituksen.

